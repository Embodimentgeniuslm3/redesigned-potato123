---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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
{:preview: .preview}



# Programas de utilidad de almacenamiento de IBM Cloud
{: #utilities}

## Instalación del plugin IBM Cloud Block Storage Attacher (beta)
{: #block_storage_attacher}

Utilice el plugin {{site.data.keyword.Bluemix_notm}} Block Storage Attacher para conectar almacenamiento de bloque sin formato y sin montar en un nodo trabajador del clúster.  
{: shortdesc}

Por ejemplo, suponga que desea almacenar sus datos con una solución de almacenamiento definida por software (SDS), como por ejemplo [Portworx](/docs/containers?topic=containers-portworx), pero no desea utilizar nodos trabajadores nativos que estén optimizados para uso de SDS y que vienen con discos locales adicionales. Para añadir discos locales al nodo trabajador no SDS, debe crear manualmente los dispositivos de almacenamiento en bloque en la cuenta de la infraestructura de {{site.data.keyword.Bluemix_notm}} y utilizar {{site.data.keyword.Bluemix_notm}} Block Volume Attacher para conectar el almacenamiento al nodo trabajador no SDS.

El plugin {{site.data.keyword.Bluemix_notm}} Block Volume Attacher crea pods en cada nodo trabajador del clúster como parte de un conjunto de daemons y configura una clase de almacenamiento de Kubernetes que luego puede utilizar para conectar el dispositivo de almacenamiento en bloque con el nodo trabajador no SDS.

¿Está buscando instrucciones sobre cómo actualizar o eliminar el plugin {{site.data.keyword.Bluemix_notm}} Block Volume Attacher? Consulte [Actualización del plugin](#update_block_attacher) y [Eliminación del plugin](#remove_block_attacher).
{: tip}

1.  [Siga las instrucciones](/docs/containers?topic=containers-helm#public_helm_install) para instalar el cliente Helm en la máquina local e instale el servidor Helm (tiller) con una cuenta de servicio en el clúster.

2.  Verifique que el tiller se ha instalado con una cuenta de servicio.

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    Salida de ejemplo:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. Actualice el repositorio de Helm para recuperar la última versión de todos los diagramas de Helm de este repositorio.
   ```
   helm repo update
   ```
   {: pre}

4. Instale el plugin {{site.data.keyword.Bluemix_notm}} Block Volume Attacher. Cuando instala el plugin, se añaden a su clúster las clases de almacenamiento de almacenamiento en bloque predefinidas.
   ```
   helm install iks-charts/ibm-block-storage-attacher --name block-attacher
   ```
   {: pre}

   Salida de ejemplo:
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

5. Verifique que el conjunto de daemons de {{site.data.keyword.Bluemix_notm}} Block Volume Attacher se ha instalado correctamente.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   Salida de ejemplo:
   ```
   ibmcloud-block-storage-attacher-z7cv6           1/1       Running            0          19m
   ```
   {: screen}

   La instalación se ha realizado correctamente si ve uno o varios pods **ibmcloud-block-storage-attacher**. El número de pods es igual al número de nodos trabajadores de su clúster. Todos los pods deben estar en un estado **Running**.

6. Verifique que la clase de almacenamiento correspondiente a {{site.data.keyword.Bluemix_notm}} Block Volume Attacher se ha creado correctamente.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   Salida de ejemplo:
   ```
   ibmc-block-attacher       ibm.io/ibmc-blockattacher   11m
   ```
   {: screen}

### Actualización del plugin IBM Cloud Block Storage Attacher
{: #update_block_attacher}

Actualice el plugin {{site.data.keyword.Bluemix_notm}} Block Storage Attacher existente a la última versión.
{: shortdesc}

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

3. Localice el nombre del diagrama de Helm correspondiente al plugin {{site.data.keyword.Bluemix_notm}} Block Storage Attacher.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   Salida de ejemplo:
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

4. Actualice {{site.data.keyword.Bluemix_notm}} Block Storage Attacher a la última versión.
   ```
   helm upgrade --force --recreate-pods <helm_chart_name> ibm-block-storage-attacher
   ```
   {: pre}

### Eliminación del plugin IBM Cloud Block Volume Attacher
{: #remove_block_attacher}

Si no desea suministrar y utilizar el plugin {{site.data.keyword.Bluemix_notm}} Block Storage Attacher en el clúster, puede desinstalar el diagrama de Helm.
{: shortdesc}

1. Localice el nombre del diagrama de Helm correspondiente al plugin {{site.data.keyword.Bluemix_notm}} Block Storage Attacher.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   Salida de ejemplo:
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

2. Suprima el plugin {{site.data.keyword.Bluemix_notm}} Block Storage Attacher eliminando el diagrama de Helm.
   ```
   helm delete <helm_chart_name> --purge
   ```
   {: pre}

3. Verifique que los pods de {{site.data.keyword.Bluemix_notm}} Block Storage Attacher se han eliminado.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   La eliminación de los pods es satisfactoria si dejan de aparecer en la salida de la CLI.

4. Verifique que la clase de almacenamiento de {{site.data.keyword.Bluemix_notm}} Block Storage Attacher se ha eliminado.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   La eliminación de la clase de almacenamiento se ha ejecutado correctamente si no se muestra ninguna clase de almacenamiento en la salida de la CLI.

## Suministro automático de almacenamiento en bloque sin formato y autorización a los nodos trabajadores para acceder al almacenamiento
{: #automatic_block}

Puede utilizar el plugin {{site.data.keyword.Bluemix_notm}} Block Volume Attacher para añadir automáticamente almacenamiento en bloque sin formato y sin montar con la misma configuración a todos los nodos trabajadores del clúster.
{: shortdesc}

El contenedor `mkpvyaml` que se incluye en el plugin {{site.data.keyword.Bluemix_notm}} Block Volume Attacher se configura de modo que se ejecute como un script que busca todos los nodos trabajadores del clúster, crea almacenamiento en bloque sin formato en el portal de la infraestructura de {{site.data.keyword.Bluemix_notm}} y autoriza a los nodos trabajadores a acceder al almacenamiento.

Para añadir otras configuraciones de almacenamiento en bloque, añada almacenamiento en bloque solo a un subconjunto de nodos trabajadores, o, para tener más control sobre el proceso de suministro, opte por [añadir manualmente almacenamiento en bloque](#manual_block).
{: tip}


1. Inicie una sesión en {{site.data.keyword.Bluemix_notm}} y elija como destino el grupo de recursos en el que está el clúster.
   ```
   ibmcloud login
   ```
   {: pre}

2.  Clone el repositorio de programas de utilidad de almacenamiento de {{site.data.keyword.Bluemix_notm}}.
    ```
    git clone https://github.com/IBM/ibmcloud-storage-utilities.git
    ```
    {: pre}

3. Vaya al directorio `block-storage-utilities`.
   ```
   cd ibmcloud-storage-utilities/block-storage-provisioner
   ```
   {: pre}

4. Abra el archivo `yamlgen.yaml` y especifique la configuración de almacenamiento en bloque que desea añadir a cada nodo trabajador del clúster.
   ```
   #
   # Can only specify 'performance' OR 'endurance' and associated clause
   #
   cluster:  <cluster_name>    #  Enter the name of your cluster
   region:   <region>          #  Enter the IBM Cloud Kubernetes Service region where you created your cluster
   type:  <storage_type>       #  Enter the type of storage that you want to provision. Choose between "performance" or "endurance"
   offering: storage_as_a_service
   # performance:
   #    - iops:  <iops>
   endurance:
     - tier:  2                #   [0.25|2|4|10]
   size:  [ 20 ]               #   Enter the size of your storage in gigabytes
   ```
   {: codeblock}

   <table>
   <caption>Visión general de los componentes del archivo YAML</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
   </thead>
   <tbody>
   <tr>
   <td><code>cluster</code></td>
   <td>Especifique el nombre del clúster en el que desea añadir almacenamiento en bloque sin formato. </td>
   </tr>
   <tr>
   <td><code>region</code></td>
   <td>Especifique la región de {{site.data.keyword.containerlong_notm}} en la que ha creado el clúster. Ejecute <code>[bxcs] cluster-get --cluster &lt;cluster_name_or_ID&gt;</code> para encontrar la región de su clúster.  </td>
   </tr>
   <tr>
   <td><code>type</code></td>
   <td>Especifique el tipo de almacenamiento que desea suministrar. Elija entre <code>performance</code> o <code>endurance</code>. Para obtener más información, consulte [Cómo decidir la configuración del almacenamiento en bloque](/docs/containers?topic=containers-block_storage#block_predefined_storageclass).  </td>
   </tr>
   <tr>
   <td><code>performance.iops</code></td>
   <td>Si desea suministrar almacenamiento de tipo `performance`, escriba el número de IOPS. Para obtener más información, consulte [Cómo decidir la configuración del almacenamiento en bloque](/docs/containers?topic=containers-block_storage#block_predefined_storageclass). Si desea suministrar almacenamiento de tipo `endurance`, elimine esta sección o coméntela añadiendo el símbolo `#` al principio de cada línea.
   </tr>
   <tr>
   <td><code>endurance.tier</code></td>
   <td>Si desea suministrar almacenamiento de tipo `endurance`, escriba el número de IOPS por gigabyte. Por ejemplo, si desea suministrar almacenamiento en bloque definido en la clase de almacenamiento `ibmc-block-bronze`, especifique 2. Para obtener más información, consulte [Cómo decidir la configuración del almacenamiento en bloque](/docs/containers?topic=containers-block_storage#block_predefined_storageclass). Si desea suministrar almacenamiento de tipo `performance`, elimine esta sección o coméntela añadiendo el símbolo `#` al principio de cada línea. </td>
   </tr>
   <tr>
   <td><code>size</code></td>
   <td>Especifique el tamaño del almacenamiento en gigabytes. Consulte [Cómo decidir la configuración de almacenamiento en bloque](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) para ver los tamaños admitidos para el nivel de almacenamiento. </td>
   </tr>
   </tbody>
   </table>  

5. Recupere el nombre de usuario y la clave de API de la infraestructura de IBM Cloud (SoftLayer). El script `mkpvyaml` utiliza el nombre de usuario y la clave de API para acceder al clúster.
   1. Inicie una sesión en la [consola de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/).
   2. En el menú ![Icono de menú](../icons/icon_hamburger.svg "Icono de menú"), seleccione **Infraestructura**.
   3. En la barra de menús, seleccione **Cuenta** > **Usuarios** > **Lista de usuarios**.
   4. Busque el usuario cuyo nombre de usuario y clave de API desea recuperar.
   5. Pulse **Generar** para generar una clave de API o **Ver** para ver la clave de API existente. Se abre una ventana emergente que muestra el nombre de usuario y la clave de API de la infraestructura.

6. Almacene las credenciales en una variable de entorno.
   1. Añada las variables de entorno.
      ```
      export SL_USERNAME=<infrastructure_username>
      ```
      {: pre}

      ```
      export SL_API_KEY=<infrastructure_api_key>
      ```
      {: pre}

   2. Verifique que las variables de entorno se han creado correctamente.
      ```
      printenv
      ```
      {: pre}

7.  Cree y ejecute el contenedor `mkpvyaml`. Cuando se ejecuta el contenedor desde la imagen, se ejecuta el script `mkpvyaml.py`. El script añade un dispositivo de almacenamiento en bloque a cada nodo trabajador del clúster y autoriza a cada nodo trabajador a acceder al dispositivo de almacenamiento en bloque. Al final del script, se genera un archivo YAML `pv-<cluster_name>.yaml` que puede utilizar posteriormente para crear volúmenes persistentes en el clúster.
    1.  Cree el contenedor `mkpvyaml`.
        ```
        docker build -t mkpvyaml .
        ```
        {: pre}
        Salida de ejemplo:
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
    2.  Ejecute el contenedor para ejecutar el script `mkpvyaml.py`.
        ```
        docker run --rm -v `pwd`:/data -v ~/.bluemix:/config -e SL_API_KEY=$SL_API_KEY -e SL_USERNAME=$SL_USERNAME mkpvyaml
        ```
        {: pre}

        Salida de ejemplo:
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
        Output file created as :  pv-<cluster_name>.yaml
        ```
        {: screen}

7. [Adjunte los dispositivos de almacenamiento en bloque a los nodos trabajadores](#attach_block).

## Adición manual de almacenamiento en bloque a nodos trabajadores específicos
{: #manual_block}

Utilice esta opción si desea añadir otras configuraciones de almacenamiento en bloque, añadir almacenamiento en bloque solo a un subconjunto de nodos trabajadores o tener más control sobre el proceso de suministro.
{: shortdesc}

1. Obtenga una lista de los nodos trabajadores del clúster y anote la dirección IP privada y la zona de los nodos trabajadores no SDS en los que desea añadir un dispositivo de almacenamiento en bloque.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

2. Revise los pasos 3 y 4 del apartado sobre [Cómo decidir la configuración de almacenamiento en bloque](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) para elegir el tipo, el tamaño y el número de IOPS del dispositivo de almacenamiento en bloque que desea añadir al nodo trabajador no SDS.    

3. Cree el dispositivo de almacenamiento en bloque en la misma zona en la que se encuentra el nodo trabajador no SDS.

   **Ejemplo de suministro de almacenamiento en bloque resistente de 20 GB con dos IOPS por GB:**
   ```
   ibmcloud sl block volume-order --storage-type endurance --size 20 --tier 2 --os-type LINUX --datacenter dal10
   ```
   {: pre}

   **Ejemplo de suministro de almacenamiento en bloque de rendimiento de 20 GB con 100 IOPS:**
   ```
   ibmcloud sl block volume-order --storage-type performance --size 20 --iops 100 --os-type LINUX --datacenter dal10
   ```
   {: pre}

4. Verifique que se ha creado el dispositivo de almacenamiento en bloque y anote el **`id`** del volumen. **Nota:** Si no ve el dispositivo de almacenamiento en bloque de inmediato, espere unos minutos. A continuación, ejecute de nuevo este mandato.
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}

   Salida de ejemplo:
   ```
   id         username          datacenter   storage_type                capacity_gb   bytes_used   ip_addr         lunId   active_transactions
   123456789  IBM02SL1234567-8  dal10        performance_block_storage   20            -            161.12.34.123   0       0   
   ```
   {: screen}

5. Revise los detalles del volumen y anote la **`IP de destino`** y el **`ID de LUN`**.
   ```
   ibmcloud sl block volume-detail <volume_ID>
   ```
   {: pre}

   Salida de ejemplo:
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

6. Autorice al nodo trabajador no SDS a acceder al dispositivo de almacenamiento en bloque. Sustituya `<volume_ID>` por el ID de volumen del dispositivo de almacenamiento en bloque que ha recuperado anteriormente y `<private_worker_IP>` por la dirección IP privada del nodo trabajador no SDS a la que desea conectar el dispositivo.

   ```
   ibmcloud sl block access-authorize <volume_ID> -p <private_worker_IP>
   ```
   {: pre}

   Salida de ejemplo:
   ```
   The IP address 123456789 was authorized to access <volume_ID>.
   ```
   {: screen}

7. Verifique que el nodo trabajador no SDS esté correctamente autorizado y anote los valores de **`host_iqn`**, **`username`** y **`password`**.
   ```
   ibmcloud sl block access-list <volume_ID>
   ```
   {: pre}

   Salida de ejemplo:
   ```
   ID          name                 type   private_ip_address   source_subnet   host_iqn                                      username   password           allowed_host_id
   123456789   <private_worker_IP>  IP     <private_worker_IP>  -               iqn.2018-09.com.ibm:ibm02su1543159-i106288771   IBM02SU1543159-I106288771   R6lqLBj9al6e2lbp   1146581   
   ```
   {: screen}

   La autorización se ha realizado correctamente cuando se asignan los valores **`host_iqn`**, **`username`** y **`password`**.

8. [Adjunte los dispositivos de almacenamiento en bloque a los nodos trabajadores](#attach_block).


## Conexión de almacenamiento en bloque sin formato a nodos trabajadores no SDS
{: #attach_block}

Para conectar el dispositivo de almacenamiento en bloque a un nodo trabajador no SDS, debe crear un volumen persistente (PV) con la clase de almacenamiento {{site.data.keyword.Bluemix_notm}} Block Volume Attacher y los detalles del dispositivo de almacenamiento en bloque.
{: shortdesc}

**Antes de empezar**:
- Asegúrese de que ha creado almacenamiento en bloque sin formato y sin montar de forma [automática](#automatic_block) o [manual](#manual_block) en los nodos trabajadores no SDS.
- [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**Para conectar almacenamiento en bloque sin formato a nodos trabajadores no SDS**:
1. Prepare la creación de PV.  
   - **Si ha utilizado el contenedor `mkpvyaml`:**
     1. Abra el archivo `pv-<cluster_name>.yaml`.
        ```
        nano pv-<cluster_name>.yaml
        ```
        {: pre}

     2. Revise la configuración de los PV.

   - **Si ha añadido el almacenamiento en bloque de forma manual:**
     1. Cree un archivo `pv.yaml`. El siguiente mandato crea el archivo con el editor `nano`.
        ```
        nano pv.yaml
        ```
        {: pre}

     2. Añada al PV los detalles del dispositivo de almacenamiento en bloque.
        ```
        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: <pv_name>
          annotations:
            ibm.io/iqn: "<IQN_hostname>"
            ibm.io/username: "<username>"
            ibm.io/password: "<password>"
            ibm.io/targetip: "<targetIP>"
            ibm.io/lunid: "<lunID>"
            ibm.io/nodeip: "<private_worker_IP>"
            ibm.io/volID: "<volume_ID>"
        spec:
          capacity:
            storage: <size>
          accessModes:
            - ReadWriteOnce
          hostPath:
              path: /
          storageClassName: ibmc-block-attacher
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
      	<td>Especifique un nombre para el PV.</td>
      	</tr>
        <tr>
        <td><code>ibm.io/iqn</code></td>
        <td>Especifique el nombre de host IQN que ha recuperado anteriormente. </td>
        </tr>
        <tr>
        <td><code>ibm.io/username</code></td>
        <td>Especifique el nombre de usuario de la infraestructura de IBM Cloud (SoftLayer) que ha recuperado anteriormente. </td>
        </tr>
        <tr>
        <td><code>ibm.io/password</code></td>
        <td>Especifique la contraseña de la infraestructura de IBM Cloud (SoftLayer) que ha recuperado anteriormente. </td>
        </tr>
        <tr>
        <td><code>ibm.io/targetip</code></td>
        <td>Especifique la IP de destino que ha recuperado anteriormente. </td>
        </tr>
        <tr>
        <td><code>ibm.io/lunid</code></td>
        <td>Especifique el ID de LUN del dispositivo de almacenamiento en bloque que ha recuperado anteriormente. </td>
        </tr>
        <tr>
        <td><code>ibm.io/nodeip</code></td>
        <td>Especifique la dirección IP privada del nodo trabajador al que desea adjuntar el dispositivo de almacenamiento en bloque y que ha autorizado anteriormente a acceder al dispositivo de almacenamiento en bloque. </td>
        </tr>
        <tr>
          <td><code>ibm.io/volID</code></td>
        <td>Especifique el ID del volumen de almacenamiento en bloque que ha recuperado anteriormente. </td>
        </tr>
        <tr>
        <td><code>storage</code></td>
        <td>Especifique el tamaño del dispositivo de almacenamiento en bloque que ha recuperado anteriormente. Por ejemplo, si el dispositivo de almacenamiento en bloque es de 20 gigabytes, especifique <code>20Gi</code>.  </td>
        </tr>
        </tbody>
        </table>
2. Cree el PV para conectar el dispositivo de almacenamiento en bloque con el nodo trabajador no SDS.
   - **Si ha utilizado el contenedor `mkpvyaml`:**
     ```
     kubectl apply -f pv-<cluster_name>.yaml
     ```
     {: pre}

   - **Si ha añadido el almacenamiento en bloque de forma manual:**
     ```
     kubectl apply -f  block-volume-attacher/pv.yaml
     ```
     {: pre}

3. Verifique que el almacenamiento en bloque se ha conectado correctamente al nodo trabajador.
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   Salida de ejemplo:
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

   El dispositivo de almacenamiento en bloque se ha conectado correctamente cuando **ibm.io/dm** se establece en un ID de dispositivo, por ejemplo `/dev/dm/1`, y ve **ibm.io/attachstatus=arrached** en la sección **Annotations** de la salida de la CLI.

Si desea desconectar un volumen, suprima el PV. Aún pueden acceder a los volúmenes desconectados nodos trabajadores específicos y se vuelven a conectar cuando se crea un nuevo PV con la clase de almacenamiento {{site.data.keyword.Bluemix_notm}} Block Volume Attacher para adjuntar otro volumen al mismo nodo trabajador. Para evitar el tener que volver a conectar el volumen desconectado antiguo, desautorice al nodo trabajador a acceder al volumen desconectado con el mandato `ibmcloud sl block access-revoke`. El hecho de desconectar el volumen no elimina el volumen de la cuenta de la infraestructura de IBM Cloud (SoftLayer). Para cancelar la facturación de su volumen, debe [eliminar el almacenamiento de la cuenta de infraestructura de IBM Cloud (SoftLayer)](/docs/containers?topic=containers-cleanup) de forma manual.
{: note}


