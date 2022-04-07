---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-03"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}rwo
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}



# Almacenamiento de datos en IBM Block Storage for IBM Cloud
{: #block_storage}

{{site.data.keyword.Bluemix_notm}} Block Storage es un almacenamiento iSCSI persistente y de alto rendimiento que puede añadir a las apps mediante volúmenes persistentes (PV) de Kubernetes. Puede elegir los niveles de almacenamiento predefinidos con tamaños de GB e IOPS que cumplan los requisitos de sus cargas de trabajo. Para averiguar si {{site.data.keyword.Bluemix_notm}} Block Storage es la opción de almacenamiento adecuada para usted, consulte [Elección de una solución de almacenamiento](/docs/containers?topic=containers-storage_planning#choose_storage_solution). Para obtener información sobre los precios, consulte [Facturación](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#billing).
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} Block Storage solo está disponible para clústeres estándares. Si el clúster no puede acceder a la red pública, como por ejemplo un clúster privado detrás de un cortafuegos o un clúster con solo el punto final de servicio privado habilitado, asegúrese de que ha instalado el plugin {{site.data.keyword.Bluemix_notm}} Block Storage versión 1.3.0 o posterior para conectarse a la instancia de almacenamiento en bloque a través de la red privada. Las instancias de almacenamiento en bloque son específicas de una sola zona. Si tiene un clúster multizona, tenga en cuenta las [opciones de almacenamiento persistente multizona](/docs/containers?topic=containers-storage_planning#persistent_storage_overview).
{: important}

## Instalación del plugin {{site.data.keyword.Bluemix_notm}} Block Storage en el clúster
{: #install_block}

Instale el plugin {{site.data.keyword.Bluemix_notm}} Block Storage con un diagrama de Helm para configurar las clases de almacenamiento predefinidas para el almacenamiento en bloque. Utilice estas clases de almacenamiento para crear una PVC para suministrar almacenamiento en bloque para sus apps.
{: shortdesc}

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Asegúrese de que el nodo trabajador aplica el parche más reciente para su versión menor.
   1. Obtenga una lista de la versión de parche actual de los nodos trabajadores.
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

      Salida de ejemplo:
      ```
      OK
      ID                                                  Public IP        Private IP     Machine Type           State    Status   Zone    Version
      kube-dal10-crb1a23b456789ac1b20b2nc1e12b345ab-w26   169.xx.xxx.xxx    10.xxx.xx.xxx   b3c.4x16.encrypted     normal   Ready    dal10   1.13.6_1523*
      ```
      {: screen}

      Si el nodo trabajador no aplica la última versión de parche, verá un asterisco (`*`) en la columna **Version** de la salida de la CLI.

   2. Revise el [registro de cambios de versión](/docs/containers?topic=containers-changelog#changelog) para encontrar los cambios que se incluyen en la última versión del parche.

   3. Aplique la versión de parche más reciente volviendo a cargar el nodo trabajador. Siga las instrucciones del [mandato ibmcloud ks worker-reload](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) para volver a planificar correctamente cualquier pod en ejecución en el nodo trabajador antes de volver a cargar el nodo trabajador. Tenga en cuenta que, durante la recarga, la máquina del nodo trabajador se actualizará con la imagen más reciente y se suprimirán los datos si no se han [almacenado fuera del nodo trabajador](/docs/containers?topic=containers-storage_planning#persistent_storage_overview).

2.  [Siga las instrucciones](/docs/containers?topic=containers-helm#public_helm_install) para instalar el cliente Helm en la máquina local e instale el servidor Helm (Tiller) con una cuenta de servicio en el clúster.

    La instalación del tiller del servidor Helm requiere una conexión de red pública con el registro de contenedor de Google público. Si el clúster no puede acceder a la red pública, como por ejemplo un clúster privado detrás de un cortafuegos o un clúster con solo el punto final de servicio privado habilitado, puede optar por [extraer la imagen de Tiller en la máquina local y enviar por push la imagen a su espacio de nombres en {{site.data.keyword.registryshort_notm}}](/docs/containers?topic=containers-helm#private_local_tiller), o bien por [instalar el diagrama de Helm sin utilizar Tiller](/docs/containers?topic=containers-helm#private_install_without_tiller).
    {: note}

3.  Verifique que el Tiller se ha instalado con una cuenta de servicio.

    ```
    kubectl get serviceaccount -n kube-system tiller
    ```
    {: pre}

    Salida de ejemplo:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

4. Añada el repositorio de diagramas de Helm de {{site.data.keyword.Bluemix_notm}} al clúster donde desee utilizar el plugin de Almacenamiento en bloque de {{site.data.keyword.Bluemix_notm}}.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

5. Actualice el repositorio de Helm para recuperar la última versión de todos los diagramas de Helm de este repositorio.
   ```
   helm repo update
   ```
   {: pre}

6. Instale el plugin {{site.data.keyword.Bluemix_notm}} Block Storage. Cuando instala el plugin, se añaden a su clúster las clases de almacenamiento de almacenamiento en bloque predefinidas.
   ```
   helm install iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

   Salida de ejemplo:
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

7. Verifique que la instalación ha sido satisfactoria.
   ```
   kubectl get pod -n kube-system | grep block
   ```
   {: pre}

   Salida de ejemplo:
   ```
   ibmcloud-block-storage-driver-kh4mt                              1/1       Running   0          27d       10.118.98.19   10.118.98.19
   ibmcloud-block-storage-plugin-58c5f9dc86-pbl4t                   1/1       Running   0          14d       172.21.0.204   10.118.98.19
   ```
   {: screen}

   La instalación es satisfactoria si ve un pod `ibmcloud-block-storage-plugin` y uno o varios pods `ibmcloud-block-storage-driver`. El número de pods `ibmcloud-block-storage-driver` corresponde al número de nodos trabajadores de su clúster. Todos los pods deben estar en un estado **Running**.

8. Verifique que las clases de almacenamiento para el almacenamiento en bloque se añaden a su clúster.
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}

   Salida de ejemplo:
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

9. Repita estos pasos para cada clúster donde desea suministrar almacenamiento en bloques.

Ahora puede seguir para [crear una PVC](#add_block) para suministrar almacenamiento en bloque a la app.


### Actualización del plugin {{site.data.keyword.Bluemix_notm}} Block Storage
Actualice el plugin {{site.data.keyword.Bluemix_notm}} Block Storage existente a la última versión.
{: shortdesc}

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Actualice el repositorio de Helm para recuperar la última versión de todos los diagramas de Helm de este repositorio.
   ```
   helm repo update
   ```
   {: pre}

2. Opcional: descargue el último diagrama de Helm en la máquina local. A continuación, extraiga el paquete y revise el archivo `release.md`
para ver la información de release más reciente.
   ```
   helm fetch iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

3. Busque el nombre del diagrama de Helm de almacenamiento en bloque que ha instalado en el clúster.
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   Salida de ejemplo:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

4. Actualice el plugin {{site.data.keyword.Bluemix_notm}} Block Storage a la última versión.
   ```
   helm upgrade --force --recreate-pods <helm_chart_name>  iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

5. Opcional: cuando actualice el plugin, la clase de almacenamiento `default` deja de estar establecida. Si desea establecer como clase de almacenamiento predeterminada la clase de almacenamiento de su elección, ejecute el mandato siguiente.
   ```
   kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
   ```
   {: pre}


### Eliminación del plugin {{site.data.keyword.Bluemix_notm}} Block Storage
Si no desea suministrar y utilizar {{site.data.keyword.Bluemix_notm}} Block Storage en el clúster, puede desinstalar el diagrama de Helm.
{: shortdesc}

Al eliminar el plugin no elimina los datos, las PVC o los PV. Cuando se elimina el plugin, se eliminan del clúster todos los conjuntos de daemons y pods relacionados. No puede suministrar nuevo almacenamiento en bloque para el clúster o utilizar PV o PVC de almacenamiento en bloque existente después de eliminar el plugin.
{: important}

Antes de empezar:
- [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Asegúrese de que no haya PVC ni PV en el clúster que utilice almacenamiento en bloque.

Para eliminar el plugin:

1. Busque el nombre del diagrama de Helm de almacenamiento en bloque que ha instalado en el clúster.
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   Salida de ejemplo:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. Suprima el plugin {{site.data.keyword.Bluemix_notm}} Block Storage.
   ```
   helm delete <helm_chart_name>
   ```
   {: pre}

3. Verifique que se hayan eliminado los pods de almacenamiento en bloque.
   ```
   kubectl get pod -n kube-system | grep ibmcloud-block-storage-plugin
   ```
   {: pre}
   La eliminación de los pods es satisfactoria si dejan de aparecer en la salida de la CLI.

4. Verifique que se hayan eliminado las clases de almacenamiento en bloque.
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}
   La eliminación de las clases de almacenamiento es satisfactoria si dejan de aparecer en la salida de la CLI.

<br />



## Cómo decidir la configuración del almacenamiento en bloque
{: #block_predefined_storageclass}

{{site.data.keyword.containerlong}} proporciona clases de almacenamiento predefinidas para el almacenamiento en bloque que puede utilizar para suministrar almacenamiento en bloque con una configuración específica.
{: shortdesc}

Cada clase de almacenamiento especifica el tipo de almacenamiento en bloque que suministra, incluidos tamaño disponible, IOPS, sistema de archivos y política de retención.  

Asegúrese de elegir la configuración de almacenamiento cuidadosamente para tener suficiente capacidad para almacenar los datos. Después de suministrar un tipo específico de almacenamiento utilizando una clase de almacenamiento, no puede cambiar tamaño, tipo, IOPS ni política de retención del dispositivo de almacenamiento. Si necesita más almacenamiento o almacenamiento con otra configuración, debe [crear una nueva instancia de almacenamiento y copiar los datos](/docs/containers?topic=containers-kube_concepts#update_storageclass) de la instancia de almacenamiento antigua en la nueva.
{: important}

1. Obtenga una lista de las clases de almacenamiento disponibles en {{site.data.keyword.containerlong}}.
    ```
    kubectl get storageclasses | grep block
    ```
    {: pre}

    Salida de ejemplo:
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

2. Revise la configuración de una clase de almacenamiento.
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   Para obtener más información sobre cada clase de almacenamiento, consulte la [referencia de clases de almacenamiento](#block_storageclass_reference). Si no encuentra lo que está buscando, considere la posibilidad de crear su propia clase de almacenamiento personalizada. Para empezar, compruebe los [ejemplos de clase de almacenamiento personalizada](#block_custom_storageclass).
   {: tip}

3. Elija el tipo de almacenamiento en bloque que desea suministrar.
   - **Clases de almacenamiento de bronce, plata y oro:** estas clases de almacenamiento suministran [almacenamiento resistente](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance). El almacenamiento resistente le permite elegir el tamaño del almacenamiento en gigabytes en los niveles de IOPS predefinidos.
   - **Clase de almacenamiento personalizada:** esta clase de almacenamiento contiene [almacenamiento de rendimiento](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provperformance). Con el almacenamiento de rendimiento, tiene más control sobre el tamaño del almacenamiento y de IOPS.

4. Elija el tamaño e IOPS para el almacenamiento en bloque. El tamaño y el número de IOPS definen el número total de IOPS (operaciones de entrada/salida por segundo), lo que sirve como indicador de la rapidez del almacenamiento. Cuantas más IOPS tenga el almacenamiento, más rápido se procesarán las operaciones de entrada y salida.
   - **Clases de almacenamiento de bronce, plata y oro:** estas clases de almacenamiento se suministran con un número fijo de IOPS por gigabytes y se suministran en discos duros SSD. El número total de IOPS depende del tamaño del almacenamiento que elija. Puede seleccionar cualquier número entero de gigabytes comprendido dentro del rango de tamaño permitido, como por ejemplo 20 Gi, 256 Gi o 11854 Gi. Para determinar el número total de IOPS, debe multiplicar IOPS por el tamaño seleccionado. Por ejemplo, si selecciona un tamaño de almacenamiento en bloque de 1000 Gi en la clase de almacenamiento de plata que se suministra con 4 IOPS por GB, el almacenamiento tendrá un total de 4000 IOPS.  
     <table>
         <caption>Tabla de IOPS y rangos de tamaño de clase de almacenamiento por gigabyte</caption>
         <thead>
         <th>Clase de almacenamiento</th>
         <th>IOPS por gigabyte</th>
         <th>Rango de tamaño en gigabytes</th>
         </thead>
         <tbody>
         <tr>
         <td>Bronce</td>
         <td>2 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Plata</td>
         <td>4 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Oro</td>
         <td>10 IOPS/GB</td>
         <td>20-4000 Gi</td>
         </tr>
         </tbody></table>
   - **Clase de almacenamiento personalizada:** cuando elige esta clase de almacenamiento, tiene más control sobre el tamaño y el número de IOPS que desea. Para el tamaño, puede seleccionar cualquier número entero de gigabytes comprendido dentro del rango de tamaño permitido. El tamaño que elija determina el rango de IOPS que tendrá a su disponibilidad. Puede elegir un tamaño de IOPS que sea un múltiplo de 100 y que esté en el rango especificado. Las IOPS que elige son estáticas y no se escalan con el tamaño del almacenamiento. Por ejemplo, si elige 40 Gi con 100 IOPS, el número total de IOPS seguirá siendo 100. </br></br>La proporción entre IOPS y gigabytes también determina el tipo de disco duro que se suministra. Por ejemplo, si tiene 500 Gi a 100 IOPS, la proporción entre IOPS y gigabyte será de 0,2. Si la proporción es menor o igual a 0,3, el almacenamiento se suministra en discos duros SATA. Si la proporción es mayor que 0,3, el almacenamiento se suministran en los discos duros SSD.
     <table>
         <caption>Tabla de IOPS y rangos de tamaño de clase de almacenamiento personalizadas</caption>
         <thead>
         <th>Rango de tamaño en gigabytes</th>
         <th>Rango de IOPS en múltiplos de 100</th>
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

5. Decida si desea conservar los datos después de que se suprima el clúster o la reclamación de volumen persistente (PVC).
   - Si desea conservar los datos, seleccione la clase de almacenamiento `retain`. Cuando se suprime la PVC, únicamente ésta se suprime. El PV, el dispositivo de almacenamiento físico de la cuenta de infraestructura de IBM Cloud (SoftLayer) y los datos seguirán existiendo. Para reclamar el almacenamiento y volverlo a utilizar en el clúster, debe eliminar el PV y seguir los pasos de [utilización de almacenamiento en bloque existente](#existing_block).
   - Si desea que el PV, los datos y el dispositivo físico de almacenamiento en bloques se supriman cuando suprima la PVC, elija una clase de almacenamiento sin la opción `retain`.

6. Decida si desea que se le facture por horas o por meses. Consulte las [tarifas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/block-storage/pricing) para obtener más información. De forma predeterminada, todos los dispositivos de almacenamiento en bloque se suministran con un tipo de facturación por hora.

<br />



## Adición de almacenamiento en bloques a apps
{: #add_block}

Cree una reclamación de volumen persistente (PVC) para [suministrar de forma dinámica](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning) almacenamiento en bloque para el clúster. El suministro dinámico crea automáticamente el volumen persistente (PV) adecuado y solicita el dispositivo de almacenamiento real en la cuenta de infraestructura de IBM Cloud (SoftLayer).
{:shortdesc}

El almacenamiento en bloque se suministra con la modalidad de acceso `ReadWriteOnce`. Solo puede montarlo en un pod en un nodo trabajador en el clúster al mismo tiempo.
{: note}

Antes de empezar:
- Si tiene un cortafuegos, [permita el acceso de salida](/docs/containers?topic=containers-firewall#pvc) para los rangos de IP de la infraestructura de IBM Cloud (SoftLayer) de las zonas en las que están en los clústeres, de modo que pueda crear las PVC.
- Instale el [plugin de almacenamiento en bloque de {{site.data.keyword.Bluemix_notm}}](#install_block).
- [Decida si desea utilizar una clase de almacenamiento predefinida](#block_predefined_storageclass) o crear una [clase de almacenamiento personalizada](#block_custom_storageclass).

¿Desea desplegar almacenamiento en bloque en un conjunto con estado? Consulte [Utilización del almacenamiento en bloque en un conjunto con estado](#block_statefulset) para obtener más información.
{: tip}

Para añadir almacenamiento en bloque:

1.  Cree un archivo de configuración para definir su reclamación de volumen persistente (PVC) y guarde la configuración como archivo `.yaml`.

    -  **Ejemplo de clases de almacenamiento bronce, plata y oro**:
       El siguiente archivo `.yaml` crea una reclamación que se denomina `mypvc` de la clase de almacenamiento `"ibmc-block-silver"`, que se factura con el valor `"hourly"`, con un tamaño de gigabyte de `24Gi`.

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

    -  **Ejemplo de utilización de una clase de almacenamiento personalizada**:
       El siguiente archivo `.yaml` crea una reclamación denominada `mypvc` de la clase de almacenamiento `ibmc-block-retain-custom`, con una facturación de tipo `"hourly"` y un tamaño en gigabytes de `45Gi` y de IOPS de `"300"`.

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
       <caption>Visión general de los componentes del archivo YAML</caption>
       <thead>
       <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
       </thead>
       <tbody>
       <tr>
       <td><code>metadata.name</code></td>
       <td>Escriba el nombre de la PVC.</td>
       </tr>
       <tr>
         <td><code>metadata.labels.billingType</code></td>
         <td>Especifique la frecuencia con la que desea que se calcule la factura de almacenamiento, los valores son "monthly" u "hourly". El valor predeterminado es "hourly".</td>
       </tr>
       <tr>
       <td><code>metadata.labels.region</code></td>
       <td>Especifique la región en la que desea suministrar el almacenamiento en bloque. Si especifica la región, también debe especificar una zona. Si no especifica una región, o si la región especificada no se encuentra, el almacenamiento se crea en la misma región que el clúster. <p class="note">Esta opción solo recibe soporte con el plugin IBM Cloud Block Storage versión 1.0.1 o superior. Para las versiones anteriores del plugin, si tiene un clúster multizona, la zona en la que se suministra el almacenamiento se selecciona en una iteración cíclica para equilibrar las solicitudes de volumen de forma uniforme entre todas las zonas. Para especificar la zona para el almacenamiento, puede crear en primer lugar una [clase de almacenamiento personalizada](#block_multizone_yaml). A continuación, cree una PVC con la clase de almacenamiento personalizada.</p></td>
       </tr>
       <tr>
       <td><code>metadata.labels.zone</code></td>
	<td>Especifique la zona en la que desea suministrar el almacenamiento en bloque. Si especifica la zona, también debe especificar una región. Si no especifica una zona o si la zona especificada no se encuentra en un clúster multizona, la zona se selecciona en una iteración cíclica. <p class="note">Esta opción solo recibe soporte con el plugin IBM Cloud Block Storage versión 1.0.1 o superior. Para las versiones anteriores del plugin, si tiene un clúster multizona, la zona en la que se suministra el almacenamiento se selecciona en una iteración cíclica para equilibrar las solicitudes de volumen de forma uniforme entre todas las zonas. Para especificar la zona para el almacenamiento, puede crear en primer lugar una [clase de almacenamiento personalizada](#block_multizone_yaml). A continuación, cree una PVC con la clase de almacenamiento personalizada.</p></td>
	</tr>
        <tr>
        <td><code>spec.resources.requests.storage</code></td>
        <td>Indique el tamaño del almacenamiento en bloque, en gigabytes (Gi). Una vez suministrado el almacenamiento, no puede cambiar el tamaño del almacenamiento en bloque. Asegúrese de especificar un tamaño que coincida con la cantidad de datos que desea almacenar. </td>
        </tr>
        <tr>
        <td><code>spec.resources.requests.iops</code></td>
        <td>Esta opción solo está disponible para las clases de almacenamiento personalizadas (`ibmc-block-custom / ibmc-block-retain-custom`). Especifique el IOPS total para el almacenamiento seleccionando un múltiplo de 100 dentro del rango permitido. Si elige un IOPS distinto del que aparece en la lista, el IOPS se redondea.</td>
        </tr>
	<tr>
	<td><code>spec.storageClassName</code></td>
	<td>El nombre de la clase de almacenamiento que desea utilizar para suministrar almacenamiento en bloque. Puede optar por utilizar una de las [clases de almacenamiento proporcionadas por IBM](#block_storageclass_reference) o [crear su propia clase de almacenamiento](#block_custom_storageclass). </br> Si no especifica ninguna clase de almacenamiento, el PV se crea con la clase de almacenamiento predeterminada <code>ibmc-file-bronze</code><p>**Sugerencia:** Si desea cambiar la clase de almacenamiento predeterminada, ejecute <code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code> y sustituya <code>&lt;storageclass&gt;</code> con el nombre de la clase de almacenamiento.</p></td>
	</tr>
        </tbody></table>

    Si desea utilizar una clase de almacenamiento personalizada, cree su PVC con el nombre de clase de almacenamiento correspondiente, unas IOPS válidas y un tamaño.   
    {: tip}

2.  Cree la PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

3.  Verifique que la PVC se ha creado y se ha vinculado al PV. Este proceso puede tardar unos minutos.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    Salida de ejemplo:

    ```
    Name: mypvc
    Namespace: default
    StorageClass: ""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWO
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m 3m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume  for claim "default/my-persistent-volume-claim"
       3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-block", expecting that  a volume for the claim is provisioned either manually or via external software
       1m 1m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume  pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

4.  {: #block_app_volume_mount}Para montar el PV en el despliegue, cree un archivo `.yaml` de configuración y especifique la PVC que enlaza el PV.

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <deployment_name>
      labels:
        app: <deployment_label>
    spec:
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - image: <image_name>
            name: <container_name>
            volumeMounts:
            - name: <volume_name>
              mountPath: /<file_path>
          volumes:
          - name: <volume_name>
            persistentVolumeClaim:
              claimName: <pvc_name>
    ```
    {: codeblock}

    <table>
    <caption>Visión general de los componentes del archivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
        <tr>
    <td><code>metadata.labels.app</code></td>
    <td>Una etiqueta para el despliegue.</td>
      </tr>
      <tr>
        <td><code>spec.selector.matchLabels.app</code> <br/> <code>spec.template.metadata.labels.app</code></td>
        <td>Una etiqueta para la app.</td>
      </tr>
    <tr>
    <td><code>template.metadata.labels.app</code></td>
    <td>Una etiqueta para el despliegue.</td>
      </tr>
    <tr>
    <td><code>spec.containers.image</code></td>
    <td>El nombre del imagen que desea utilizar. Para ver una lista de todas las imágenes disponibles en su cuenta de {{site.data.keyword.registryshort_notm}}, ejecute `ibmcloud cr image-list`.</td>
    </tr>
    <tr>
    <td><code>spec.containers.name</code></td>
    <td>El nombre del contenedor que desea desplegar en el clúster.</td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>La vía de acceso absoluta del directorio en el que el que está montado el volumen dentro del contenedor. Los datos que se escriben en la vía de acceso de montaje se almacenan en el directorio raíz de la instancia de almacenamiento en bloque físico. Si desea compartir un volumen entre distintas apps, puede especificar [sub vías de acceso de volumen ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath) para cada una de las apps. </td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.name</code></td>
    <td>El nombre del volumen que va a montar en el pod.</td>
    </tr>
    <tr>
    <td><code>volumes.name</code></td>
    <td>El nombre del volumen que va a montar en el pod. Normalmente, este nombre es el mismo que <code>volumeMounts/name</code>.</td>
    </tr>
    <tr>
    <td><code>volumes.persistentVolumeClaim.claimName</code></td>
    <td>El nombre de la PVC que enlaza el PV que desea utilizar. </td>
    </tr>
    </tbody></table>

5.  Cree el despliegue.
     ```
     kubectl apply -f <local_yaml_path>
     ```
     {: pre}

6.  Verifique que el PV se ha montado correctamente.

     ```
     kubectl describe deployment <deployment_name>
     ```
     {: pre}

     El punto de montaje se muestra en el campo **Volume Mounts** y el volumen se muestra en el campo **Volumes**.

     ```
      Volume Mounts:
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
          /volumemount from myvol (rw)
     ...
     Volumes:
      myvol:
        Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName: mypvc
        ReadOnly: false
     ```
     {: screen}

<br />




## Utilización de almacenamiento en bloque existente en el clúster
{: #existing_block}

Si dispone de un dispositivo de almacenamiento físico existente que desea utilizar en el clúster, puede crear manualmente el PV y la PVC para [suministrar de forma estática](/docs/containers?topic=containers-kube_concepts#static_provisioning) el almacenamiento.
{: shortdesc}

Antes de empezar a montar el almacenamiento existente en una app, debe recuperar toda la información necesaria para su PV.  

### Paso 1: Recuperación de la información del almacenamiento en bloque existente
{: #existing-block-1}

1.  Recupere o genere una clave de API para su cuenta (SoftLayer) de infraestructura de IBM Cloud.
    1. Inicie sesión en el [portal de la infraestructura de IBM Cloud (SoftLayer) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/classic?).
    2. Seleccione **Cuenta**, **Usuarios** y, a continuación, **Lista de usuarios**.
    3. Encuentre su ID de usuario.
    4. En la columna de **Clave de API**, pulse **Generar** para generar una clave de API o **Ver** para ver su clave de API existente.
2.  Recupere el nombre de usuario de API de su cuenta (SoftLayer) de infraestructura de IBM Cloud.
    1. Desde el menú **Lista de usuarios**, seleccione su ID de usuario.
    2. En la sección **Información de acceso de API**, busque su **Nombre de usuario de API**.
3.  Inicie una sesión en el plugin de CLI de la infraestructura de IBM Cloud.
    ```
    ibmcloud sl init
    ```
    {: pre}

4.  Elija autenticarse utilizando el nombre de usuario y la clave de API de su cuenta (SoftLayer) de infraestructura de IBM Cloud.
5.  Especifique el nombre de usuario y la clave de API que recuperó en los pasos anteriores.
6.  Obtenga una lista de los dispositivos de almacenamiento en bloque disponibles.
    ```
    ibmcloud sl block volume-list
    ```
    {: pre}

    Salida de ejemplo:
    ```
    id         username            datacenter   storage_type              capacity_gb   bytes_used   ip_addr         lunId   active_transactions
    38642141   IBM02SEL1543159-1   dal10        endurance_block_storage   20            -            169.xx.xxx.xxx   170     0
    ```
    {: screen}

7.  Anote los valores de `id`, `ip_addr`, `capacity_gb`, `datacenter` y `lunId` del dispositivo de almacenamiento en bloque que desea montar para el clúster. **Nota:** para montar el almacenamiento existente en un clúster, debe tener un nodo trabajador en la misma zona que el almacenamiento. Para verificar la zona del nodo trabajador, ejecute `ibmcloud ks workers --cluster <cluster_name_or_ID>`.

### Paso 2: Creación de un volumen persistente (PV) y de una reclamación de volumen persistente (PVC) coincidente
{: #existing-block-2}

1.  Opcional: si tiene almacenamiento que ha suministrado con una clase de almacenamiento de tipo `retain`, cuando elimine la PVC, el PV y el dispositivo de almacenamiento físico no se eliminarán. Para volver a utilizar el almacenamiento en el clúster, primero debe eliminar el PV.
    1. Obtenga una lista de los PV existentes.
       ```
       kubectl get pv
       ```
       {: pre}

       Busque el PV perteneciente a su almacenamiento persistente. El PV está en el estado `released`.

    2. Elimine el PV.
       ```
       kubectl delete pv <pv_name>
       ```
       {: pre}

    3. Verifique que el PV se ha eliminado.
       ```
       kubectl get pv
       ```
       {: pre}

2.  Cree un archivo de configuración para el PV. Incluya los valores de `id`, `ip_addr`, `capacity_gb`, `datacenter` y `lunIdID` del almacenamiento en bloque que ha recuperado anteriormente.

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
        storage: "<storage_size>"
      accessModes:
        - ReadWriteOnce
      flexVolume:
        driver: "ibm/ibmc-block"
        fsType: "<fs_type>"
        options:
          "Lun": "<lun_ID>"
          "TargetPortal": "<IP_address>"
          "VolumeID": "<volume_ID>"
          "volumeName": "<volume_name>"
      ```
      {: codeblock}

    <table>
    <caption>Visión general de los componentes del archivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>Especifique el nombre del PV que crear.</td>
    </tr>
    <tr>
    <td><code>metadata.labels</code></td>
    <td>Especifique la región y la zona que ha recuperado anteriormente. Debe tener al menos un nodo trabajador en la misma región y zona que el almacenamiento persistente para montar el almacenamiento en el clúster. Si ya existe un PV para el almacenamiento, [añada la etiqueta de zona y región](/docs/containers?topic=containers-kube_concepts#storage_multizone) a su PV.
    </tr>
    <tr>
    <td><code>spec.flexVolume.fsType</code></td>
    <td>Especifique el tipo de sistema de archivos que está configurado para el almacenamiento en bloques existente. Elija entre <code>ext4</code> o <code>xfs</code>. Si no especifica esta opción, de forma predeterminada será <code>ext4</code> para el PV. Cuando se define el `fsType` erróneo, se sigue creando el PV, sin embargo, el montaje del PV para el pod fallará. </td></tr>	    
    <tr>
    <td><code>spec.capacity.storage</code></td>
    <td>Especifique el tamaño del almacenamiento en bloque existente que ha recuperado en el paso anterior como <code>capacity-gb</code>. El tamaño de almacenamiento se debe especificar en gigabytes, por ejemplo, 20Gi (20 GB) o 1000Gi (1 TB).</td>
    </tr>
    <tr>
    <td><code>flexVolume.options.Lun</code></td>
    <td>Especifique el ID de la unidad lógica de su almacenamiento en bloque que ha recuperado anteriormente como <code>lunId</code>.</td>
    </tr>
    <tr>
    <td><code>flexVolume.options.TargetPortal</code></td>
    <td>Especifique la dirección IP del almacenamiento en bloque que ha recuperado anteriormente como <code>ip_addr</code>. </td>
    </tr>
    <tr>
	    <td><code>flexVolume.options.VolumeId</code></td>
	    <td>Especifique el ID del almacenamiento en bloque que ha recuperado anteriormente como <code>id</code>.</td>
	    </tr>
	    <tr>
		    <td><code>flexVolume.options.volumeName</code></td>
		    <td>Especifique un nombre para el volumen.</td>
	    </tr>
    </tbody></table>

3.  Cree el PV en el clúster.
    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

4. Verifique que se ha creado el PV.
    ```
    kubectl get pv
    ```
    {: pre}

5. Cree otro archivo de configuración para crear la PVC. Para que la PVC coincida con el PV que ha creado anteriormente, debe elegir el mismo valor para `storage` y `accessMode`. El campo `storage-class` debe estar vacío. Si alguno de estos campos no coincide con el PV, se crea automáticamente un nuevo PV.

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
          storage: "<storage_size>"
      storageClassName:
     ```
     {: codeblock}

6.  Cree la PVC.
     ```
     kubectl apply -f mypvc.yaml
     ```
     {: pre}

7.  Verifique que la PVC se ha creado y se ha enlazado al PV que ha creado con anterioridad. Este proceso puede tardar unos minutos.
     ```
     kubectl describe pvc mypvc
     ```
     {: pre}

     Salida de ejemplo:

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

Ha creado correctamente un PV y lo ha enlazado a una PVC. Ahora los usuarios del clúster pueden [montar la PVC](#block_app_volume_mount) en sus despliegues y empezar a leer el PV y a escribir en el mismo.

<br />



## Utilización del almacenamiento en bloque en un conjunto con estado
{: #block_statefulset}

Si tiene una app con estado como, por ejemplo, una base de datos, puede crear conjuntos con estado que utilicen el almacenamiento en bloque para almacenar los datos de la app. Como alternativa, puede utilizar una base de datos como servicio de {{site.data.keyword.Bluemix_notm}} y almacenar los datos en la nube.
{: shortdesc}

**¿Qué debo tener en cuenta cuando añada almacenamiento en bloque a un conjunto con estado?** </br>
Para añadir almacenamiento a un conjunto con estado, debe especificar la configuración del almacenamiento en la sección `volumeClaimTemplates` del archivo YAML del conjunto con estado. `volumeClaimTemplates` constituye la base para la PVC y puede incluir la clase de almacenamiento y el tamaño o IOPS del almacenamiento en bloque que desea suministrar. Sin embargo, si desea incluir etiquetas en `volumeClaimTemplates`, Kubernetes no incluye estas etiquetas al crear la PVC. En su lugar, debe añadir las etiquetas directamente al conjunto con estado.

No se pueden desplegar dos conjuntos con estado al mismo tiempo. Si intenta crear un conjunto con estado antes de que otro se despliegue por completo, el despliegue de su conjunto con estado puede dar lugar a resultados inesperados.
{: important}

**¿Cómo se crea un conjunto con estado en una zona específica?** </br>
En un clúster multizona, puede especificar la zona y la región en las que desea crear el conjunto con estado en la sección `spec.selector.matchLabels` y en la sección `spec.template.metadata.labels` del archivo YAML del conjunto con estado. Como alternativa, puede añadir estas etiquetas a una [clase de almacenamiento personalizada](/docs/containers?topic=containers-kube_concepts#customized_storageclass) y utilizar esta clase de almacenamiento en la sección `volumeClaimTemplates` de su conjunto con estado.

**¿Puedo retrasar el enlace de un PV a mi pod con estado hasta que el pod esté listo?**<br>
Sí, puede [crear una clase de almacenamiento personalizada](#topology_yaml) para la PVC que incluya el campo [`volumeBindingMode: WaitForFirstConsumer` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/storage/storage-classes/#volume-binding-mode).

**¿Qué opciones tengo para añadir almacenamiento en bloque a un conjunto con estado?** </br>
Si desea crear automáticamente la PVC al crear el conjunto con estado, utilice el [suministro dinámico](#block_dynamic_statefulset). También puede optar por [realizar un suministro previo de las PVC o utilizar PVC existentes](#block_static_statefulset) con su conjunto con estado.  

### Suministro dinámico: Creación de la PVC al crear un conjunto con estado
{: #block_dynamic_statefulset}

Utilice esta opción si desea crear automáticamente la PVC al crear el conjunto con estado.
{: shortdesc}

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Verifique que todos los conjuntos con estado existentes del clúster estén totalmente desplegados. Si todavía se está desplegando un conjunto con estado, no puede empezar a crear su conjunto con estado. Debe esperar a que todos los conjuntos con estado del clúster se hayan desplegado por completo para evitar resultados inesperados.
   1. Obtenga una lista de los conjuntos con estado existentes en el clúster.
      ```
      kubectl get statefulset --all-namespaces
      ```
      {: pre}

      Salida de ejemplo:
      ```
      NAME              DESIRED   CURRENT   AGE
      mystatefulset     3         3         6s
      ```
      {: screen}

   2. Visualice el **estado de los pods** de cada conjunto con estado para asegurarse de que el despliegue del conjunto con estado haya finalizado.  
      ```
      kubectl describe statefulset <statefulset_name>
      ```
      {: pre}

      Salida de ejemplo:
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

      Un conjunto con estado se ha desplegado por completo cuando el número de réplicas que encuentra en la sección **Replicas** de la salida de CLI es igual al número de pods con el estado **Running** en la sección **Pods Status**. Si un conjunto con estado aún no se ha desplegado por completo, espere hasta que el despliegue haya finalizado antes de continuar.

2. Cree un archivo de configuración para el conjunto con estado y el servicio que utiliza para exponer el conjunto con estado.

   - **Ejemplo de conjunto con estado que especifica una zona:**

     En el ejemplo siguiente se muestra cómo desplegar NGINX como un conjunto con estado con 3 réplicas. Para cada réplica, se proporciona un dispositivo de almacenamiento en bloque de 20 gigabytes en función de las especificaciones definidas en la clase de almacenamiento `ibmc-block-retain-bronze`. Todos los dispositivos de almacenamiento se suministran en la zona `dal10`. Puesto que no se puede acceder al almacenamiento en bloque desde otras zonas, todas las réplicas del conjunto de estado también se despliegan en un nodo trabajador que se encuentra en `dal10`.

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

   - **Ejemplo de conjunto con estado con regla de antiafinidad y creación retrasada de almacenamiento en bloque:**

     En el ejemplo siguiente se muestra cómo desplegar NGINX como un conjunto con estado con 3 réplicas. El conjunto con estado no especifica la región y la zona en las que se ha creado el almacenamiento en bloque. En su lugar, el conjunto con estado utiliza una regla de antiafinidad para asegurarse de que los pods se distribuyen entre nodos trabajadores y zonas. Al definir `topologykey: failure-domain.beta.kubernetes.io/zone`, el planificador de Kubernetes no puede planificar un pod en un nodo trabajador si el nodo trabajador se encuentra en la misma zona que un pod que tiene la etiqueta `app: nginx`. Para cada pod de conjunto con estado, se crean dos PVC definidas en la sección `volumeClaimTemplates`, pero la creación de las instancias de almacenamiento en bloque se retrasa hasta que se planifica un pod de conjunto con estado que utiliza el almacenamiento. Esta configuración se conoce como [planificación de volúmenes que tienen en cuenta la topología](https://kubernetes.io/blog/2018/10/11/topology-aware-volume-provisioning-in-kubernetes/).

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
     <caption>Visión general de los componentes del archivo YAML de un conjunto con estado</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Visión general de los componentes del archivo YAML de un conjunto con estado</th>
     </thead>
     <tbody>
     <tr>
     <td style="text-align:left"><code>metadata.name</code></td>
     <td style="text-align:left">Especifique un nombre para el conjunto con estado. El nombre que escriba se utiliza para crear el nombre de la PVC con el siguiente formato: <code>&lt;nombre_volumen&gt;-&lt;nombre_conjuntoconestado&gt;-&lt;número_réplica&gt;</code>. </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.serviceName</code></td>
     <td style="text-align:left">Especifique el nombre del servicio que desea utilizar para exponer el conjunto con estado. </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.replicas</code></td>
     <td style="text-align:left">Especifique el número de réplicas para el conjunto con estado. </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.podManagementPolicy</code></td>
     <td style="text-align:left">Especifique la política de gestión de pod que desea utilizar para su conjunto con estado. Seleccione una de las opciones siguientes: <ul><li><strong>`OrderedReady`: </strong>con esta opción, las réplicas del conjunto con estado se despliegan una después de otra. Por ejemplo, si ha especificado 3 réplicas, Kubernetes crea la PVC para la primera réplica, espera hasta que la PVC esté enlazada, despliegue la réplica del conjunto con estado y monta la PVC en la réplica. Una vez que haya finalizado el despliegue, se despliega la segunda réplica. Para obtener más información sobre esta opción, consulte [Gestión de pod `OrderedReady` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#orderedready-pod-management). </li><li><strong>Parallel: </strong>con esta opción, el despliegue de todas las réplicas del conjunto con estado comienza a la vez. Si la app da soporte al despliegue de réplicas en paralelo, utilice esta opción para ahorrar tiempo de despliegue para las PVC y las réplicas de conjuntos con estado. </li></ul></td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
     <td style="text-align:left">Especifique todas las etiquetas que desee incluir en el conjunto con estado y en la PVC. Kubernetes no reconoce las etiquetas que se incluyen en <code>volumeClaimTemplates</code> del conjunto con estado. Ejemplos de etiquetas que quizás desee incluir: <ul><li><code><strong>region</strong></code> y <code><strong>zone</strong></code>: si desea que todas las réplicas y PVC del conjunto con estado se creen en una zona específica, añada ambas etiquetas. También puede especificar la zona y la región en la clase de almacenamiento que utiliza. Si no especifica una zona y región y tiene un clúster multizona, la zona en la que se suministra el almacenamiento se selecciona en una iteración cíclica para equilibrar las solicitudes de volumen de forma uniforme entre todas las zonas.</li><li><code><strong>billingType</strong></code>: escriba el tipo de facturación que desea utilizar para los PVC. Elija entre <code>hourly</code> o <code>monthly</code>. Si no especifica esta etiqueta, todas las PVC se crean con un tipo de facturación por hora. </li></ul></td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
     <td style="text-align:left">Especifique las mismas etiquetas que ha añadido a la sección <code>spec.selector.matchLabels</code>. </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.template.spec.affinity</code></td>
     <td style="text-align:left">Especifique la regla de antiafinidad para asegurarse de que los pods de conjunto con estado se distribuyen entre nodos trabajadores y zonas. El ejemplo muestra una regla de antiafinidad en la que el pod del conjunto con estado prefiere no estar planificado en un nodo trabajador en el que se ejecuta un pod que tiene la etiqueta `app: nginx`. La clave `topologykey: failure-domain.beta.kubernetes.io/zone` restringe aún más esta regla antiafinidad e impide que el pod se planifique en un nodo trabajador si este está en la misma zona que un pod con la etiqueta `app: nginx`. Mediante esta regla antiafinidad, puede lograr la afinidad entre los nodos trabajadores y las zonas. </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.name</code></td>
     <td style="text-align:left">Especifique un nombre para el volumen. Utilice el mismo nombre que ha definido en la sección <code>spec.containers.volumeMount.name</code>. El nombre que escriba aquí se utiliza para crear el nombre de la PVC con el siguiente formato: <code>&lt;nombre_volumen&gt;-&lt;nombre_conjuntoconestado&gt;-&lt;número_réplica&gt;</code>. </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.storage</code></td>
     <td style="text-align:left">Indique el tamaño del almacenamiento en bloque, en gigabytes (Gi).</td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.resources.</code></br><code>requests.iops</code></td>
     <td style="text-align:left">Si desea suministrar [almacenamiento de rendimiento](#block_predefined_storageclass), especifique el número de IOPS. Si utiliza una clase de almacenamiento de resistencia y especifica un número de IOPS, se pasa por alto el número de IOPS. En su lugar se utiliza el número de IOPS especificado en la clase de almacenamiento.  </td>
     </tr>
     <tr>
     <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
     <td style="text-align:left">Especifique la clase de almacenamiento que desea utilizar. Para ver una lista de las clases de almacenamiento existentes, ejecute <code>kubectl get storageclasses | grep block</code>. Si no especifica una clase de almacenamiento, la PVC se crea con la clase de almacenamiento predeterminada establecida en el clúster. Asegúrese de que la clase de almacenamiento predeterminada utiliza el suministrador <code>ibm.io/ibmc-block</code> para que el conjunto con estado se suministre con almacenamiento en bloque.</td>
     </tr>
     </tbody></table>

4. Cree su conjunto con estado.
   ```
   kubectl apply -f statefulset.yaml
   ```
   {: pre}

5. Espere a que se despliegue el conjunto con estado.
   ```
   kubectl describe statefulset <statefulset_name>
   ```
   {: pre}

   Para ver el estado actual de los PVC, ejecute `kubectl get pvc`. El nombre de la PVC tiene el formato `<volume_name>-<statefulset_name>-<replica_number>`.
   {: tip}

### Suministro estático: Utilización de PVC existentes con un conjunto con estado
{: #block_static_statefulset}

Puede realizar un suministro previo de las PVC antes de crear el conjunto con estado o utilizar PVC existentes con su conjunto con estado.
{: shortdesc}

Cuando [suministre dinámicamente las PVC al crear el conjunto con estado](#block_dynamic_statefulset), el nombre de la PVC se asigna en función de los valores que ha utilizado en el archivo YAML de conjunto con estado. Para que el conjunto con estado utilice las PVC existentes, el nombre de las PVC debe coincidir con el nombre que se crearía automáticamente si se utilizara el suministro dinámico.

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Si desea suministrar la PVC del conjunto con estado antes de crear el conjunto con estado, siga los pasos del 1 al 3 de la sección [Adición de almacenamiento en bloque a apps](#add_block) para crear un PVC para cada réplica del conjunto con estado. Asegúrese de crear la PVC con un nombre que siga el formato siguiente: `<volume_name>-<statefulset_name>-<replica_number>`.
   - **`<volume_name>`**: utilice el nombre que desea especificar en la sección `spec.volumeClaimTemplates.metadata.name`
del conjunto con estado, como por ejemplo `nginxvol`.
   - **`<statefulset_name>`**: utilice el nombre que desea especificar en la sección `metadata.name` del conjunto con estado, como por ejemplo `nginx_statefulset`.
   - **`<replica_number>`**: especifique el número de la réplica empezando por 0.

   Por ejemplo, si tiene que crear 3 réplicas del conjunto con estado, cree 3 PVC con los siguientes nombres: `nginxvol-nginx_statefulset-0`, `nginxvol-nginx_statefulset-1` y `nginxvol-nginx_statefulset-2`.  

   ¿Está buscando crear un PVC y un PV para un dispositivo de almacenamiento existente? Cree la PVC y el PV mediante [suministro estático](#existing_block).

2. Siga los pasos de [Suministro dinámico: Creación de la PVC al crear un conjunto con estado](#block_dynamic_statefulset) para crear el conjunto con estado. El nombre de la PVC sigue el formato `<volume_name>-<statefulset_name>-<replica_number>`. Asegúrese de utilizar los siguientes valores de nombre de PVC en la especificación del conjunto con estado:
   - **`spec.volumeClaimTemplates.metadata.name`**: especifique el `<volume_name>` del nombre de PVC.
   - **`metadata.name`**: especifique el `<statefulset_name>` del nombre de PVC.
   - **`spec.replicas`**: especifique el número de réplicas que desea crear para el conjunto con estado. El número de réplicas debe ser igual al número de PVC que ha creado anteriormente.

   Si las PVC están en diferentes zonas, no incluya una etiqueta de región o zona en el conjunto con estado.
   {: note}

3. Verifique que las PVC se utilizan en los pods de réplica del conjunto con estado.
   1. Obtenga una lista de pods en el clúster. Identifique los pods que pertenecen a su conjunto con estado.
      ```
      kubectl get pods
      ```
      {: pre}

   2. Verifique que la PVC existente esté montada en la réplica del conjunto con estado. Revise el valor de **`ClaimName`** en la sección **`Volumes`** de la salida de la CLI.
      ```
      kubectl describe pod <pod_name>
      ```
      {: pre}

      Salida de ejemplo:
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


## Cambio del tamaño e IOPS del dispositivo de almacenamiento existente
{: #block_change_storage_configuration}

Si desea aumentar la capacidad de almacenamiento o el rendimiento, puede modificar el volumen existente.
{: shortdesc}

Para ver preguntas sobre la facturación y encontrar los pasos sobre cómo utilizar la consola de
{{site.data.keyword.Bluemix_notm}} para modificar el almacenamiento, consulte
[Ampliación de la capacidad de almacenamiento en bloque](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity#expandingcapacity) y [Ajuste de IOPS](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS). Las actualizaciones que realice desde la consola no se reflejan en el volumen persistente (PV). Para añadir esta información al PV, ejecute `kubectl patch pv <pv_name>` y actualice manualmente el tamaño e IOPS en la sección **Labels** y **Annotation** del PV.
{: tip}

1. Liste las PVC del clúster y tome nota del nombre del PV asociado en la columna **VOLUME**.
   ```
   kubectl get pvc
   ```
   {: pre}

   Salida de ejemplo:
   ```
   NAME             STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS        AGE
   myvol            Bound     pvc-01ac123a-123b-12c3-abcd-0a1234cb12d3   20Gi       RWO            ibmc-block-bronze    147d
   ```
   {: screen}

2. Si desea cambiar el valor de IOPS y el tamaño del almacenamiento en bloque, edite primero el valor de IOPS en la sección `metadata.labels.IOPS` de su PV. Puede aumentar o reducir el valor de IOPS. Asegúrese de especificar un IOPS que reciba soporte del tipo de almacenamiento que tiene. Por ejemplo, si tiene almacenamiento en bloque resistente con 4 IOPS, puede cambiar el valor de IOPS por 2 ó 10. Para ver los valores de IOPS admitidos, consulte [Cómo decidir la configuración del almacenamiento en bloque](/docs/containers?topic=containers-block_storage#block_predefined_storageclass).
   ```
   kubectl edit pv <pv_name>
   ```
   {: pre}

   Para cambiar el valor de IOPS desde la CLI, también debe cambiar el tamaño del almacenamiento en bloque. Si solo desea cambiar modificar IOPS, pero no el tamaño, debe [solicitar el cambio de IOPS desde la consola](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS).
   {: note}

3. Edite la PVC y añada el nuevo tamaño en la sección `spec.resources.requests.storage` de la PVC. Puede cambiar por un tamaño mayor solo hasta la capacidad máxima establecida por la clase de almacenamiento. No puede reducir el tamaño del almacenamiento existente. Para ver los tamaños válidos para la clase de almacenamiento, consulte
[Cómo decidir la configuración del almacenamiento en bloque](/docs/containers?topic=containers-block_storage#block_predefined_storageclass).
   ```
   kubectl edit pvc <pvc_name>
   ```
   {: pre}

4. Verifique que se solicita la ampliación del volumen. La ampliación de volumen se solicita correctamente cuando se ve el mensaje `FileSystemResizePending` en la sección **Conditions** de la salida de la CLI. 
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}

   Salida de ejemplo:
   ```
   ...
   Conditions:
   Type                      Status  LastProbeTime                     LastTransitionTime                Reason  Message
   ----                      ------  -----------------                 ------------------                ------  -------
   FileSystemResizePending   True    Mon, 01 Jan 0001 00:00:00 +0000   Thu, 25 Apr 2019 15:52:49 -0400           Waiting for user to (re-)start a pod to finish file system resize of volume on node.
   ```
   {: screen}

5. Obtenga una lista de los pods que montan la PVC. Si la PVC está montada mediante un pod, la ampliación de volumen se procesa automáticamente. Si la PVC no está montada mediante un pod, debe montar la PVC en un pod para que se pueda procesar la ampliación de volumen. 
   ```
   kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
   ```
   {: pre}

   Los pods montados se devuelven con el formato: `<pod_name>: <pvc_name>`.

6. Si la PVC no está montada mediante un pod, [cree un pod o un despliegue y monte la PVC](/docs/containers?topic=containers-block_storage#add_block). Si la PVC está montada mediante un pod, continúe en el paso siguiente. 

7. Supervise el estado de la ampliación del volumen. La ampliación del volumen finaliza cuando se ve el mensaje `"message":"Success"` en la salida de la CLI.
   ```
   kubectl get pv <pv_name> -o go-template=$'{{index .metadata.annotations "ibm.io/volume-expansion-status"}}\n'
   ```
   {: pre}

   Salida de ejemplo:
   ```
   {"size":50,"iops":500,"orderid":38832711,"start":"2019-04-30T17:00:37Z","end":"2019-04-30T17:05:27Z","status":"complete","message":"Success"}
   ```
   {: screen}

8. Verifique que el tamaño y el número de IOPS se han modificado en la sección **Labels** de la salida de la CLI.
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   Salida de ejemplo:
   ```
   ...
   Labels:       CapacityGb=50
                 Datacenter=dal10
                 IOPS=500
   ```
   {: screen}


## Copia de seguridad y restauración de datos
{: #block_backup_restore}

El almacenamiento en bloque se suministra en la misma ubicación que los nodos trabajadores del clúster. IBM aloja el almacenamiento en servidores en clúster para proporcionar disponibilidad en el caso de que un servidor deje de funcionar. Sin embargo, no se realizan automáticamente copias de seguridad del almacenamiento en bloque, y puede dejar de ser accesible si falla toda la ubicación. Para evitar que los datos que se pierdan o se dañen, puede configurar copias de seguridad periódicas que puede utilizar para restaurar los datos cuando sea necesario.
{: shortdesc}

Consulte las opciones siguientes de copia de seguridad y restauración para el almacenamiento en bloque:

<dl>
  <dt>Configurar instantáneas periódicas</dt>
  <dd><p>Puede [configurar instantáneas periódicas para el almacenamiento en bloque](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots), que son imágenes de solo lectura que capturan el estado de la instancia en un punto en el tiempo. Para almacenar la instantánea, debe solicitar espacio de instantáneas en el almacenamiento en bloque. Las instantáneas se almacenan en la instancia de almacenamiento existente dentro de la misma zona. Puede restaurar datos desde una instantánea si un usuario elimina accidentalmente datos importantes del volumen. </br></br> <strong>Para crear una instantánea para su volumen: </strong><ol><li>[Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)</li><li>Inicie una sesión en la CLI de `ibmcloud sl`. <pre class="pre"><code>ibmcloud sl init</code></pre></li><li>Liste los PV en su clúster. <pre class="pre"><code>kubectl get pv</code></pre></li><li>Obtenga los detalles de los PV para los que desea crear espacio de instantáneas y anote el ID de volumen, el tamaño y las IOPS. <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> El tamaño y el número de IOPS se muestran en la sección <strong>Labels</strong> de la salida de la CLI. Para encontrar el ID de volumen, revise la anotación <code>ibm.io/network-storage-id</code> de la salida de la CLI. </li><li>Cree el tamaño de instantánea para el volumen existente con los parámetros que ha recuperado en el paso anterior. <pre class="pre"><code>ibmcloud sl block snapshot-order &lt;volume_ID&gt; --size &lt;size&gt; --tier &lt;iops&gt;</code></pre></li><li>Espere a que se haya creado el tamaño de la instantánea. <pre class="pre"><code>ibmcloud sl block volume-detail &lt;volume_ID&gt;</code></pre>El tamaño de la instantánea se suministra de forma correcta cuando el valor de <strong>Snapshot Size (GB)</strong> en la salida de la CLI pasa de 0 al tamaño solicitado. </li><li>Cree la instantánea para el volumen y anote el ID de la instantánea que se crea para usted. <pre class="pre"><code>ibmcloud sl block snapshot-create &lt;volume_ID&gt;</code></pre></li><li>Verifique que la instantánea se haya creado correctamente. <pre class="pre"><code>ibmcloud sl block snapshot-list &lt;volume_ID&gt;</code></pre></li></ol></br><strong>Para restaurar los datos desde una instantánea en un volumen existente: </strong><pre class="pre"><code>ibmcloud sl block snapshot-restore &lt;volume_ID&gt; &lt;snapshot_ID&gt;</code></pre></p></dd>
  <dt>Realice una réplica de las instantáneas en otra zona</dt>
 <dd><p>Para proteger los datos ante un error de la zona, puede [replicar instantáneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication) en una instancia de almacenamiento en bloque configurada en otra zona. Los datos únicamente se pueden replicar desde el almacenamiento primario al almacenamiento de copia de seguridad. No puede montar una instancia replicada de almacenamiento en bloque en un clúster. Cuando el almacenamiento primario falla, puede establecer de forma manual el almacenamiento de copia de seguridad replicado para que sea el primario. A continuación, puede montarla en el clúster. Una vez restaurado el almacenamiento primario, puede restaurar los datos del almacenamiento de copia de seguridad.</p></dd>
 <dt>Duplicar almacenamiento</dt>
 <dd><p>Puede [duplicar la instancia de almacenamiento en bloque](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume) en la misma zona que la instancia de almacenamiento original. La instancia duplicada tiene los mismos datos que la instancia de almacenamiento original en el momento de duplicarla. A diferencia de las réplicas, utilice los duplicados como una instancia de almacenamiento independiente de la original. Para duplicar, primero configure instantáneas para el volumen.</p></dd>
  <dt>Haga copia de seguridad de los datos en {{site.data.keyword.cos_full}}</dt>
  <dd><p>Puede utilizar la [**imagen ibm-backup-restore**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter) para utilizar un pod de copia de seguridad y restauración en el clúster. Este pod contiene un script para ejecutar una copia de seguridad puntual o periódico para cualquier reclamación de volumen persistente (PVC) en el clúster. Los datos se almacenan en la instancia de {{site.data.keyword.cos_full}} que ha configurado en una zona.</p><p class="note">El almacenamiento en bloque se monta con la modalidad de acceso RWO. Este acceso solo permite los pods en el almacenamiento en bloque de uno en uno. Para hacer una copia de seguridad de los datos, debe desmontar el pod de la app desde el almacenamiento, montarlo en el pod de copia de seguridad, realizar una copia de seguridad de los datos y volver a montar el almacenamiento en el pod de la app. </p>
Para aumentar la alta disponibilidad de los datos y proteger la app ante un error de la zona, configure una segunda instancia de {{site.data.keyword.cos_short}} y replique los datos entre las zonas. Si necesita restaurar datos desde la instancia de {{site.data.keyword.cos_short}}, utilice el script de restauración que se proporciona con la imagen.</dd>
<dt>Copiar datos a y desde pods y contenedores</dt>
<dd><p>Utilice el [mandato ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) `kubectl cp` para copiar archivos y directorios a y desde pods o contenedores específicos en el clúster.</p>
<p>Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) Si no especifica un contenedor con <code>-c</code>, el mandato utiliza el primer contenedor disponible en el pod.</p>
<p>El mandato se puede utilizar de varias maneras:</p>
<ul>
<li>Copiar datos desde su máquina local a un pod en su clúster: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>Copiar datos desde un pod en su clúster a su máquina local: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>Copiar datos desde su máquina local a un contenedor específico que se ejecuta en un pod en el clúster: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container&gt;</var></code></pre></li>
</ul></dd>
  </dl>

<br />


## Referencia de clases de almacenamiento
{: #block_storageclass_reference}

### Bronce
{: #block_bronze}

<table>
<caption>Clase de almacenamiento en bloque: bronce</caption>
<thead>
<th>Características</th>
<th>Valor</th>
</thead>
<tbody>
<tr>
<td>Nombre</td>
<td><code>ibmc-block-bronze</code></br><code>ibmc-block-retain-bronze</code></td>
</tr>
<tr>
<td>Tipo</td>
<td>[Almacenamiento resistente](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>Sistema de archivos</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS por gigabyte</td>
<td>2</td>
</tr>
<tr>
<td>Rango de tamaño en gigabytes</td>
<td>20-12000 Gi</td>
</tr>
<tr>
<td>Disco duro</td>
<td>SSD</td>
</tr>
<tr>
<td>Facturación</td>
<td>El tipo de facturación predeterminado depende de la versión del plugin {{site.data.keyword.Bluemix_notm}} Block Storage: <ul><li> Versión 1.0.1 y superiores: por hora</li><li>Versión 1.0.0 e inferiores: mensualmente</li></ul></td>
</tr>
<tr>
<td>Tarifas</td>
<td>[Información de precios ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>


### Plata
{: #block_silver}

<table>
<caption>Clase de almacenamiento en bloque: plata</caption>
<thead>
<th>Características</th>
<th>Valor</th>
</thead>
<tbody>
<tr>
<td>Nombre</td>
<td><code>ibmc-block-silver</code></br><code>ibmc-block-retain-silver</code></td>
</tr>
<tr>
<td>Tipo</td>
<td>[Almacenamiento resistente](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>Sistema de archivos</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS por gigabyte</td>
<td>4</td>
</tr>
<tr>
<td>Rango de tamaño en gigabytes</td>
<td>20-12000 Gi</td>
</tr>
<tr>
<td>Disco duro</td>
<td>SSD</td>
</tr>
<tr>
<td>Facturación</td>
<td>El tipo de facturación predeterminado depende de la versión del plugin {{site.data.keyword.Bluemix_notm}} Block Storage: <ul><li> Versión 1.0.1 y superiores: por hora</li><li>Versión 1.0.0 e inferiores: mensualmente</li></ul></td>
</tr>
<tr>
<td>Tarifas</td>
<td>[Información de precios ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

### Oro
{: #block_gold}

<table>
<caption>Clase de almacenamiento en bloque: oro</caption>
<thead>
<th>Características</th>
<th>Valor</th>
</thead>
<tbody>
<tr>
<td>Nombre</td>
<td><code>ibmc-block-gold</code></br><code>ibmc-block-retain-gold</code></td>
</tr>
<tr>
<td>Tipo</td>
<td>[Almacenamiento resistente](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance)</td>
</tr>
<tr>
<td>Sistema de archivos</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS por gigabyte</td>
<td>10</td>
</tr>
<tr>
<td>Rango de tamaño en gigabytes</td>
<td>20-4000 Gi</td>
</tr>
<tr>
<td>Disco duro</td>
<td>SSD</td>
</tr>
<tr>
<td>Facturación</td>
<td>El tipo de facturación predeterminado depende de la versión del plugin {{site.data.keyword.Bluemix_notm}} Block Storage: <ul><li> Versión 1.0.1 y superiores: por hora</li><li>Versión 1.0.0 e inferiores: mensualmente</li></ul></td>
</tr>
<tr>
<td>Tarifas</td>
<td>[Información de precios ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

### Personalizada
{: #block_custom}

<table>
<caption>Clase de almacenamiento en bloque: personalizada</caption>
<thead>
<th>Características</th>
<th>Valor</th>
</thead>
<tbody>
<tr>
<td>Nombre</td>
<td><code>ibmc-block-custom</code></br><code>ibmc-block-retain-custom</code></td>
</tr>
<tr>
<td>Tipo</td>
<td>[Rendimiento](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provperformance)</td>
</tr>
<tr>
<td>Sistema de archivos</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS y tamaño</td>
<td><strong>Rango de tamaños en gigabytes / Rango de IOPS en múltiplos de 100</strong><ul><li>20-39 Gi / 100-1000 IOPS</li><li>40-79 Gi / 100-2000 IOPS</li><li>80-99 Gi / 100-4000 IOPS</li><li>100-499 Gi / 100-6000 IOPS</li><li>500-999 Gi / 100-10000 IOPS</li><li>1000-1999 Gi / 100-20000 IOPS</li><li>2000-2999 Gi / 200-40000 IOPS</li><li>3000-3999 Gi / 200-48000 IOPS</li><li>4000-7999 Gi / 300-48000 IOPS</li><li>8000-9999 Gi / 500-48000 IOPS</li><li>10000-12000 Gi / 1000-48000 IOPS</li></ul></td>
</tr>
<tr>
<td>Disco duro</td>
<td>La proporción entre IOPS y gigabytes determina el tipo de disco duro que se suministra. Para determinar la proporción entre IOPS y gigabytes, divida el número de IOPS por el tamaño de su almacenamiento. </br></br>Ejemplo: </br>Supongamos que elige 500 Gi de almacenamiento con 100 IOPS. La proporción es de 0,2 (100 IOPS/500 Gi). </br></br><strong>Visión general de los tipos de disco duro por proporción:</strong><ul><li>Menor o igual que 0,3: SATA</li><li>Mayor que 0,3: SSD</li></ul></td>
</tr>
<tr>
<td>Facturación</td>
<td>El tipo de facturación predeterminado depende de la versión del plugin {{site.data.keyword.Bluemix_notm}} Block Storage: <ul><li> Versión 1.0.1 y superiores: por hora</li><li>Versión 1.0.0 e inferiores: mensualmente</li></ul></td>
</tr>
<tr>
<td>Tarifas</td>
<td>[Información de precios ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

<br />


## Clases de almacenamiento personalizadas de ejemplo
{: #block_custom_storageclass}

Puede crear una clase de almacenamiento personalizada y utilizar la clase de almacenamiento en la PVC.
{: shortdesc}

{{site.data.keyword.containerlong_notm}} proporciona [clases de almacenamiento predefinidas](#block_storageclass_reference) para
suministrar almacenamiento en bloque con un nivel y una configuración determinados. En algunos casos, es posible que desee suministrar almacenamiento con otra configuración que no esté cubierta en las clases de almacenamiento predefinidas. Puede utilizar los ejemplos de este tema para encontrar clases de almacenamiento personalizadas de ejemplo.

Para crear la clase de almacenamiento personalizada, consulte [Personalización de una clase de almacenamiento](/docs/containers?topic=containers-kube_concepts#customized_storageclass). A continuación, [utilice la clase de almacenamiento personalizada en la PVC](#add_block).

### Creación de almacenamiento que tenga en cuenta la topología
{: #topology_yaml}

Para utilizar el almacenamiento en bloque en un clúster multizona, su pod debe estar planificado en la misma zona que la instancia de almacenamiento en bloque para que pueda leer y escribir en el volumen. Antes de que Kubernetes incorporara la planificación del volumen que tiene en cuenta la topología, el suministro dinámico del almacenamiento creaba automáticamente la instancia de almacenamiento en bloque al crear una PVC. A continuación, cuando creaba el pod, el planificador de Kubernetes intentaba desplegar el pod en el mismo centro de datos que la instancia de almacenamiento en bloque.
{: shortdesc}

La creación de la instancia de almacenamiento en bloque sin conocer las restricciones del pod puede llevar a resultados no deseados. Por ejemplo, es posible que el pod no pueda planificarse en el mismo nodo trabajador que el almacenamiento porque el nodo trabajador no tiene recursos suficientes o porque el nodo trabajador es antagónico y no permite que se planifique el pod. Con la planificación de volumen que tiene en cuenta la topología, la instancia de almacenamiento en bloque se retrasa hasta que se crea el primer pod que utiliza el almacenamiento.

La planificación de volúmenes que tienen en cuenta la topología recibe solo soporte en clústeres que ejecutan Kubernetes versión 1.12 o posteriores. Para utilizar esta característica, asegúrese de que ha instalado el plugin {{site.data.keyword.Bluemix_notm}} Block Storage versión 1.2.0 o posterior.
{: note}

En los siguientes ejemplos se muestra cómo crear clases de almacenamiento que retrasan la creación de la instancia de almacenamiento en bloque hasta que el primer pod que utiliza este almacenamiento esté listo para ser planificado. Para retrasar la creación, debe incluir la opción `volumeBindingMode: WaitForFirstConsumer`. Si no incluye esta opción, `volumeBindingMode` se establece automáticamente en `Immediate` y la instancia de almacenamiento en bloque se crea cuando se crea la PVC.

- **Ejemplo para almacenamiento en bloque resistente:**
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

- **Ejemplo para el almacenamiento en bloque de rendimiento:**
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

### Especificación de la zona y de la región
{: #block_multizone_yaml}

Si desea crear el almacenamiento en bloque en una zona específica, puede especificar la zona y la región en una clase de almacenamiento personalizada.
{: shortdesc}

Utilice la clase de almacenamiento personalizada si utiliza el plugin {{site.data.keyword.Bluemix_notm}} Block Storage versión 1.0.0 o si desea [suministrar almacenamiento en bloque de forma estática](#existing_block) en una zona específica. En todos los demás casos, [especifique la zona directamente en la PVC](#add_block).
{: note}

El siguiente archivo `.yaml` personaliza una clase de almacenamiento que se basa en la clase de almacenamiento sin retención `ibm-block-silver`: el valor de `type` es `"Endurance"`, el valor de `iopsPerGB` es `4`, el valor de `sizeRange` es `"[20-12000]Gi"` y el valor de `reclaimPolicy` está establecido en `"Delete"`. La zona especificada es `dal12`. Para utilizar otra clase de almacenamiento como base, consulte la [Referencia de clases de almacenamiento](#block_storageclass_reference).

Cree la clase de almacenamiento en la misma región y zona que el clúster y los nodos trabajadores. Para obtener la región del clúster, ejecute `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>` y busque el prefijo de la región en **Master URL**, como por ejemplo `eu-de` en `https://c2.eu-de.containers.cloud.ibm.com:11111`. Para obtener la zona del nodo trabajador, ejecute `ibmcloud ks workers --cluster <cluster_name_or_ID>`.

- **Ejemplo para almacenamiento en bloque resistente:**
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

- **Ejemplo para el almacenamiento en bloque de rendimiento:**
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

### Montaje de almacenamiento en bloque con un sistema de archivos `XFS`
{: #xfs}

En los ejemplos siguientes se crea una clase de almacenamiento que proporciona almacenamiento en bloque con un sistema de archivos `XFS`.
{: shortdesc}

- **Ejemplo para almacenamiento en bloque resistente:**
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

- **Ejemplo para el almacenamiento en bloque de rendimiento:**
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

