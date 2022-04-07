---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Resolución de problemas del almacenamiento del clúster
{: #cs_troubleshoot_storage}

Si utiliza {{site.data.keyword.containerlong}}, tenga en cuenta estas técnicas para solucionar problemas relacionados con el almacenamiento del clúster.
{: shortdesc}

Si tiene un problema más general, pruebe la [depuración del clúster](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}


## Depuración de anomalías de almacenamiento persistente
{: #debug_storage}

Revise las opciones para depurar el almacenamiento persistente y encontrar las causas raíz de los errores.
{: shortdesc}

1. Verifique que utiliza la última versión de {{site.data.keyword.Bluemix_notm}} y del plugin de {{site.data.keyword.containerlong_notm}}. 
   ```
   ibmcloud update
   ```
   {: pre}
   
   ```
   ibmcloud plugin repo-plugins
   ```
   {: pre}

2. Verifique que la versión de la CLI de `kubectl` que se ejecuta en la máquina local coincide con la versión de Kubernetes que está instalada en el clúster. 
   1. Visualice la versión de la CLI de `kubectl` que está instalada en el clúster y en la máquina local. 
      ```
      kubectl version
      ```
      {: pre} 
   
      Salida de ejemplo: 
      ```
      Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5", GitCommit:"641856db18352033a0d96dbc99153fa3b27298e5", GitTreeState:"clean", BuildDate:"2019-03-25T15:53:57Z", GoVersion:"go1.12.1", Compiler:"gc", Platform:"darwin/amd64"}
      Server Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5+IKS", GitCommit:"e15454c2216a73b59e9a059fd2def4e6712a7cf0", GitTreeState:"clean", BuildDate:"2019-04-01T10:08:07Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
      ```
      {: screen}
    
      Las versiones de la CLI coinciden si aparece la misma versión en `GitVersion` para el cliente y para el servidor. Puede pasar por alto la parte `+IKS` de la versión del servidor. 
   2. Si la versión de la CLI de `kubectl` de la máquina local y del clúster no coinciden, [actualice el clúster](/docs/containers?topic=containers-update) o [instale otra versión de la CLI en la máquina local](/docs/containers?topic=containers-cs_cli_install#kubectl). 

3. Solo para almacenamiento en bloque, almacenamiento de objetos y Portworx: asegúrese de que [ha instalado el servidor de Helm de Tiller con una cuenta de servicios de Kubernetes](/docs/containers?topic=containers-helm#public_helm_install). 

4. Solo para almacenamiento en bloque, almacenamiento de objetos y Portworx: asegúrese de que ha instalado la versión más reciente del diagrama de Helm para el plugin. 
   
   **Almacenamiento en bloque y de objetos**: 
   
   1. Actualice los repositorios del diagrama de Helm. 
      ```
      helm repo update
      ```
      {: pre}
      
   2. Obtenga una lista de los diagramas de Helm del repositorio `iks-charts`. 
      ```
      helm search iks-charts
      ```
      {: pre}
      
      Salida de ejemplo: 
      ```
      iks-charts/ibm-block-storage-attacher          	1.0.2        A Helm chart for installing ibmcloud block storage attach...
      iks-charts/ibm-iks-cluster-autoscaler          	1.0.5        A Helm chart for installing the IBM Cloud cluster autoscaler
      iks-charts/ibm-object-storage-plugin           	1.0.6        A Helm chart for installing ibmcloud object storage plugin  
      iks-charts/ibm-worker-recovery                 	1.10.46      IBM Autorecovery system allows automatic recovery of unhe...
      ...
      ```
      {: screen}
      
   3. Obtenga una lista de los diagramas de Helm instalados en el clúster y compare la versión que ha instalado con la versión que está disponible. 
      ```
      helm ls
      ```
      {: pre}
      
   4. Si hay una versión más reciente disponible, instale esta versión. Para obtener instrucciones, consulte [Actualización del plugin {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in) y [Actualización del plugin {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#update_cos_plugin). 
   
   **Portworx**: 
   
   1. Localice la [versión más reciente del diagrama de Helm ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/portworx/helm/tree/master/charts/portworx) que está disponible. 
   
   2. Obtenga una lista de los diagramas de Helm instalados en el clúster y compare la versión que ha instalado con la versión que está disponible. 
      ```
      helm ls
      ```
      {: pre}
   
   3. Si hay una versión más reciente disponible, instale esta versión. Para obtener instrucciones, consulte [Actualización de Portworx en el clúster](/docs/containers?topic=containers-portworx#update_portworx). 
   
5. Verifique que el controlador de almacenamiento y los pods del plugin muestran el estado **Running**. 
   1. Obtenga una lista de los pods del espacio de nombres `kube-system`. 
      ```
      kubectl get pods -n kube-system 
      ```
      {: pre}
      
   2. Si los pods no muestran el estado **Running**, obtenga más detalles sobre el pod para encontrar la causa raíz. En función del estado del pod, es posible que no pueda ejecutar todos los mandatos siguientes. 
      ```
      kubectl describe pod <pod_name> -n kube-system
      ```
      {: pre}
      
      ```
      kubectl logs <pod_name> -n kube-system
      ```
      {: pre}
      
   3. Analice la sección **Events** de la salida de la CLI del mandato `kubectl describe pod` y los registros más recientes para encontrar la causa raíz del error. 
   
6. Compruebe si la PVC se ha suministrado correctamente. 
   1. Compruebe el estado de la PVC. Una PVC se ha suministrado correctamente si la PVC muestra el estado **Bound**. 
      ```
      kubectl get pvc
      ```
      {: pre}
      
   2. Si el estado de la PVC es **Pending**, analice el error para ver por qué la PVC sigue en estado pendiente. 
      ```
      kubectl describe pvc <pvc_name>
      ```
      {: pre}
      
   3. Revise los errores comunes que se pueden producir durante la creación de una PVC. 
      - [Almacenamiento de archivos y almacenamiento en bloque: la PVC permanece en estado pendiente](#file_pvc_pending)
      - [Almacenamiento de objetos: la PVC permanece en estado pendiente](#cos_pvc_pending)
   
7. Compruebe si el pod que monta la instancia de almacenamiento se ha desplegado correctamente. 
   1. Obtenga una lista de pods en el clúster. Un pod se ha desplegado correctamente si muestra el estado **Running**. 
      ```
      kubectl get pods
      ```
      {: pre}
      
   2. Obtenga los detalles de su pod y compruebe si se muestran errores en la sección **Events** de la salida de la CLI. 
      ```
      kubectl describe pod <pod_name>
      ```
      {: pre}
   
   3. Recupere los registros de la app y compruebe si se ve algún mensaje de error. 
      ```
      kubectl logs <pod_name>
      ```
      {: pre}
   
   4. Revise los errores comunes que pueden producirse cuando se monta una PVC en una app. 
      - [Almacenamiento de archivos: la app no puede acceder a la PVC o no puede escribir en la PVC](#file_app_failures)
      - [Almacenamiento en bloque: la app no puede acceder a la PVC o no puede escribir en la PVC](#block_app_failures)
      - [Almacenamiento de objetos: falla el acceso a los archivos con un usuario no root](#cos_nonroot_access)
      

## Almacenamiento de archivos y almacenamiento en bloque: la PVC permanece en estado pendiente
{: #file_pvc_pending}

{: tsSymptoms}
Cuando se crea una PVC y se ejecuta `kubectl get pvc <pvc_name>`, la PVC permanece en estado **Pending**, incluso después de esperar algún tiempo. 

{: tsCauses}
Durante la creación y el enlace de la PVC, el plugin de almacenamiento de archivos y en bloque ejecuta varias tareas diferentes. Cada tarea puede fallar y provocar un mensaje de error distinto.

{: tsResolve}

1. Localice la causa raíz de que la PVC permanezca en estado **Pending**. 
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. Revise las descripciones y las resoluciones de los mensajes de error comunes.
   
   <table>
   <thead>
     <th>Mensaje de error</th>
     <th>Descripción</th>
     <th>Pasos para resolverlo</th>
  </thead>
  <tbody>
    <tr>
      <td><code>El usuario no tiene permisos para crear o para gestionar almacenamiento</code></br></br><code>No se han podido encontrar credenciales de softlayer válidas en el archivo de configuración</code></br></br><code>El almacenamiento con el ID de pedido %d no se ha podido crear después de reintentarlo durante %d segundos.</code></br></br><code>No se ha podido localizar el centro de datos con el nombre <datacenter_name>.</code></td>
      <td>La clave de API de IAM o la clave de API de infraestructura de IBM Cloud (SoftLayer) almacenada en el secreto de Kubernetes `storage-secret-store` del clúster no tiene todos los permisos necesarios para suministrar almacenamiento persistente. </td>
      <td>Consulte [No se puede crear la PVC porque faltan permisos](#missing_permissions). </td>
    </tr>
    <tr>
      <td><code>Su pedido superará el número máximo de volúmenes de almacenamiento permitidos. Póngase en contacto con el equipo de ventas</code></td>
      <td>Cada cuenta de {{site.data.keyword.Bluemix_notm}} se configura con un número máximo de instancias de almacenamiento que se pueden crear. Al crear la PVC, se excede el número máximo de instancias de almacenamiento. </td>
      <td>Para crear un PVC, elija entre las opciones siguientes. <ul><li>Elimine las PVC no utilizadas.</li><li>Solicite al propietario de la cuenta de {{site.data.keyword.Bluemix_notm}} que aumente su cuota de almacenamiento [abriendo un caso de soporte](/docs/get-support?topic=get-support-getting-customer-support).</li></ul> </td>
    </tr>
    <tr>
      <td><code>No se puede encontrar el valor de ItemPriceIds(type|size|iops) exacto para el almacenamiento especificado</code> </br></br><code>No se ha podido realizar el pedido de almacenamiento con el proveedor de almacenamiento</code></td>
      <td>El tamaño de almacenamiento y el valor de IOPS que ha especificado en la PVC no reciben soporte del tipo de almacenamiento que ha elegido y no se pueden utilizar con la clase de almacenamiento especificada. </td>
      <td>Revise [Cómo decidir la configuración del almacenamiento de archivos](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) y [Cómo decidir la configuración del almacenamiento en bloque](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) para ver los tamaños de almacenamiento y el valor de IOPS admitidos para la clase de almacenamiento que desea utilizar. Corrija el tamaño y el valor de IOPS y vuelva a crear la PVC. </td>
    </tr>
    <tr>
  <td><code>No se ha encontrado el nombre del centro de datos en el archivo de configuración</code></td>
      <td>El centro de datos que ha especificado en la PVC no existe. </td>
  <td>Ejecute <code>ibmcloud ks locations</code> para ver una lista de los centros de datos disponibles. Corrija el centro de datos en la PVC y vuelva a crear la PVC.</td>
    </tr>
    <tr>
  <td><code>No se ha podido realizar el pedido de almacenamiento con el proveedor de almacenamiento</code></br></br><code>El almacenamiento con el ID de pedido 12345 no se ha podido crear después de volver a intentarlo durante xx segundos. </code></br></br><code>No se han podido realizar las autorizaciones de subred para el almacenamiento 12345.</code><code>El almacenamiento 12345 tiene transacciones activas en curso y no se ha podido completar después de reintentarlo durante xx segundos.</code></td>
  <td>Es posible que el tamaño de almacenamiento, el valor de IOPS y el tipo de almacenamiento sean incompatibles con la clase de almacenamiento que ha elegido o que el punto final de la API de infraestructura de {{site.data.keyword.Bluemix_notm}} no está disponible en este momento. </td>
  <td>Revise [Cómo decidir la configuración del almacenamiento de archivos](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) y [Cómo decidir la configuración del almacenamiento en bloque](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) para ver los tamaños de almacenamiento y el valor de IOPS admitidos para la clase de almacenamiento y el tipo de almacenamiento que desea utilizar. A continuación, suprima la PVC y vuelva a crearla. </td>
  </tr>
  <tr>
  <td><code>No se ha podido encontrar el almacenamiento con el ID de almacenamiento 12345.</code></td>
  <td>Desea crear una PVC para una instancia de almacenamiento existente mediante el suministro estático de Kubernetes, pero no se ha encontrado la instancia de almacenamiento que ha especificado. </td>
  <td>Siga las instrucciones para suministrar el [almacenamiento de archivos](/docs/containers?topic=containers-file_storage#existing_file) o el [almacenamiento en bloque](/docs/containers?topic=containers-block_storage#existing_block) existente en el clúster y asegúrese de recuperar la información correcta para la instancia de almacenamiento existente. A continuación, suprima la PVC y vuelva a crearla.  </td>
  </tr>
  <tr>
  <td><code>No se ha especificado el tipo de almacenamiento; el tipo de almacenamiento esperado es `Endurance` o `Performance`. </code></td>
  <td>Ha creado una clase de almacenamiento personalizada y ha especificado un tipo de almacenamiento que no recibe soporte.</td>
  <td>Actualice la clase de almacenamiento personalizada de modo que especifique `Endurance` o `Performance` como tipo de almacenamiento. Para ver ejemplos de clases de almacenamiento personalizadas, consulte las clases de almacenamiento personalizadas de ejemplo para [almacenamiento de archivos](/docs/containers?topic=containers-file_storage#file_custom_storageclass) y [almacenamiento en bloque](/docs/containers?topic=containers-block_storage#block_custom_storageclass). </td>
  </tr>
  </tbody>
  </table>
  
## Almacenamiento de archivos: la app no puede acceder a la PVC o no puede escribir en la PVC
{: #file_app_failures}

Cuando se monta una PVC a un pod, es posible que se produzcan errores al acceder o al escribir en la PVC. 
{: shortdesc}

1. Obtenga una lista de los pods del clúster y revise el estado del pod. 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. Localice la causa raíz de que la app no pueda acceder a la PVC o no pueda escribir en la misma.
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. Revise los errores comunes que pueden producirse cuando se monta una PVC en una pod. 
   <table>
   <thead>
     <th>Síntoma o mensaje de error</th>
     <th>Descripción</th>
     <th>Pasos para resolverlo</th>
  </thead>
  <tbody>
    <tr>
      <td>El pod está atascado en el estado <strong>ContainerCreating</strong>. </br></br><code>MountVolume.SetUp failed for volume ... read-only file system</code></td>
      <td>El programa de fondo de la infraestructura de {{site.data.keyword.Bluemix_notm}} ha experimentado problemas de red. Para proteger los datos y para evitar que resulten dañados, {{site.data.keyword.Bluemix_notm}} ha desconectado automáticamente el servidor de almacenamiento de archivos para evitar operaciones de escritura en de archivos NFS compartidos.  </td>
      <td>Consulte [Almacenamiento de archivos: los sistemas de archivos para los nodos trabajadores cambian a solo lectura](#readonly_nodes)</td>
      </tr>
      <tr>
  <td><code>write-permission</code> </br></br><code>do not have required permission</code></br></br><code>cannot create directory '/bitnami/mariadb/data': Permission denied </code></td>
  <td>En el despliegue, ha especificado un usuario que no es el root como propietario de la vía de acceso de montaje del almacenamiento de archivos NFS. De forma predeterminada, los usuarios no root no tienen permiso de escritura sobre la vía de acceso de montaje del volumen para el almacenamiento respaldado por NFS. </td>
  <td>Consulte [Almacenamiento de archivos: la app falla cuando un usuario que no es root es el propietario de la vía de acceso de montaje del almacenamiento de archivos NFS](#nonroot)</td>
  </tr>
  <tr>
  <td>Después de especificar un usuario no root como propietario de la vía de acceso de montaje de almacenamiento de archivo NFS o de desplegar un diagrama de Helm con un ID de usuario no root especificado, el usuario no puede escribir en el almacenamiento montado.</td>
  <td>En la configuración del diagrama de Helm o del despliegue se especifica el contexto de seguridad para el ID de grupo (<code>fsGroup</code>) y el ID de usuario (<code>runAsUser</code>) del pod.</td>
  <td>Consulte [Almacenamiento de archivos: la adición de acceso de usuario no root al almacenamiento persistente no se realiza correctamente](#cs_storage_nonroot)</td>
  </tr>
</tbody>
</table>

### Almacenamiento de archivos: los sistemas de archivos de los nodos trabajadores cambian a solo lectura
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
Puede ver uno de los siguientes síntomas:
- Cuando ejecuta `kubectl get pods -o wide`, ve que varios pods que se están ejecutando en el mismo nodo trabajador quedan bloqueados en el estado `ContainerCreating`.
- Cuando se ejecuta un mandato `kubectl describe`, ve el siguiente error en la sección **Events**: `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
El sistema de archivos del nodo trabajador es de sólo lectura.

{: tsResolve}
1.  Haga una copia de seguridad de los datos que puedan estar almacenados en el nodo trabajador o en los contenedores.
2.  Para un arreglo a corto plazo para el nodo trabajador existente, recargue el nodo trabajador.
    <pre class="pre"><code>ibmcloud ks worker-reload --cluster &lt;cluster_name&gt; --worker &lt;worker_ID&gt;</code></pre>

Para un arreglo a largo plazo, [actualice el tipo de máquina de la agrupación de nodos trabajadores](/docs/containers?topic=containers-update#machine_type).

<br />



### Almacenamiento de archivos: la app falla cuando un usuario no root es propietario de la vía de acceso de montaje del almacenamiento de archivos NFS
{: #nonroot}

{: tsSymptoms}
Después de [añadir almacenamiento NFS](/docs/containers?topic=containers-file_storage#file_app_volume_mount) a su despliegue, el despliegue de su contenedor falla. Al recuperar los registros del contenedor, podría ver errores como los siguientes. El pod falla y queda atascado en un ciclo de recarga.

```
write-permission
```
{: screen}

```
no tiene el permiso necesario
```
{: screen}

```
no se puede crear el directorio '/bitnami/mariadb/data': Permiso denegado
```
{: screen}

{: tsCauses}
De forma predeterminada, los usuarios no root no tienen permiso de escritura sobre la vía de acceso de montaje del volumen para el almacenamiento respaldado por NFS. Algunas imágenes comunes de app, como por ejemplo Jenkins y Nexus3, especifican un usuario no root que posee la vía de acceso de montaje en Dockerfile. Cuando se crea un contenedor desde este Dockerfile, la creación del contenedor falla debido a permisos insuficientes del usuario no root en la vía de acceso de montaje. Para otorgar permiso de escritura, puede modificar el Dockerfile para añadir temporalmente el usuario no root al grupo de usuarios root antes de cambiar los permisos de la vía de acceso de montaje, o utilizar un contenedor de inicialización.

Si utiliza un diagrama de Helm para desplegar la imagen, edite el despliegue de Helm para utilizar un contenedor de inicialización.
{:tip}



{: tsResolve}
Cuando se incluye un [contenedor de inicialización ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) en el despliegue, puede dar a un usuario no root especificado en el Dockerfile permisos de escritura para la vía de acceso de montaje del volumen dentro del contenedor. El contenedor de inicialización de inicia antes que el contenedor de la app. El contenedor de inicialización crea la vía de acceso de montaje del volumen dentro del contenedor, cambia la propiedad de la vía de acceso de montaje al usuario (no root) correcto y se cierra. A continuación, se inicia el contenedor de la app con el usuario no root que debe escribir en la vía de acceso de montaje. Dado que la vía de acceso ya es propiedad del usuario no root, la escritura en la vía de acceso de montaje se realiza correctamente. Si no desea utilizar un contenedor de inicialización, puede modificar el Dockerfile para añadir acceso de usuario no root al almacenamiento de archivos NFS.


Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Abra el Dockerfile para la app y obtenga el ID de usuario (UID) y el ID de grupo (GID) del usuario al que desea dar permiso de escritura en la vía de acceso de montaje del volumen. En el ejemplo de un Dockerfile de Jenkins, la información es la siguiente:
    - UID: `1000`
    - GID: `1000`

    **Ejemplo**:

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

2.  Añada almacenamiento persistente a la app creando una reclamación de volumen persistente (PVC). En este ejemplo se utiliza la clase de almacenamiento `ibmc-file-bronze`. Para revisar las clases de almacenamiento disponibles, ejecute `kubectl get storageclasses`.

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

3.  Cree la PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  En el archivo `.yaml` de despliegue, añada el contenedor de inicialización. Incluya el UID y el GID que ha recuperado anteriormente.

    ```
    initContainers:
    - name: initcontainer # Or replace the name
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Replace UID and GID with values from the Dockerfile
      volumeMounts:
      - name: volume # Or you can replace with any name
        mountPath: /mount # Must match the mount path in the args line
    ```
    {: codeblock}

    **Ejemplo** para un despliegue de Jenkins:

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

5.  Cree el pod y monte la PVC en el pod.

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. Verifique que el volumen se ha montado correctamente en el pod. Anote el nombre de pod y la vía de acceso de **Containers/Mounts**.

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **Salida de ejemplo**:

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
        Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName: mypvc
        ReadOnly: false

    ```
    {: screen}

7.  Inicie la sesión en el pod utilizando el nombre de pod que ha anotado anteriormente.

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. Verifique los permisos de la vía de acceso de montaje del contenedor. En el ejemplo, la vía de acceso de montaje es `/var/jenkins_home`.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **Salida de ejemplo**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    Esta salida muestra que el GID y el UID del Dockerfile (en este ejemplo, `1000` y `1000`) poseen la vía de acceso de montaje dentro del contenedor.

<br />


### Almacenamiento de archivos: la adición de acceso de usuario no root al almacenamiento persistente no se realiza correctamente
{: #cs_storage_nonroot}

{: tsSymptoms}
Después de [añadir acceso de usuario no root al almacenamiento persistente](#nonroot) o de desplegar un diagrama de Helm con un ID de usuario no root especificado, el usuario no puede escribir en el almacenamiento montado.

{: tsCauses}
En la configuración del diagrama de Helm o el despliegue se especifica el [contexto de seguridad](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) para el ID de grupo (`fsGroup`) y el ID de usuario (`runAsUser`) del pod. Actualmente, {{site.data.keyword.containerlong_notm}} no soporta la especificación de `fsGroup` y sólo soporta que `runAsUser` se establezca como `0` (permisos root).

{: tsResolve}
Elimine los campos `securityContext` de la configuración para `fsGroup` y `runAsUser` de la imagen, despliegue o archivo de configuración de diagrama de Helm y vuelva a desplegar. Si tiene que cambiar la propiedad de la vía de acceso de montaje de `nobody`, [añada acceso de usuario non-root](#nonroot). Después de añadir [non-root `initContainer`](#nonroot), establezca `runAsUser` al nivel del contenedor, no a nivel de pod.

<br />




## Almacenamiento en bloque: la app no puede acceder a la PVC o no puede escribir en la PVC
{: #block_app_failures}

Cuando se monta una PVC a un pod, es posible que se produzcan errores al acceder o al escribir en la PVC. 
{: shortdesc}

1. Obtenga una lista de los pods del clúster y revise el estado del pod. 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. Localice la causa raíz de que la app no pueda acceder a la PVC o no pueda escribir en la misma.
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. Revise los errores comunes que pueden producirse cuando se monta una PVC en una pod. 
   <table>
   <thead>
     <th>Síntoma o mensaje de error</th>
     <th>Descripción</th>
     <th>Pasos para resolverlo</th>
  </thead>
  <tbody>
    <tr>
      <td>El pod está atascado en el estado <strong>ContainerCreating</strong> o <strong>CrashLoopBackOff</strong>. </br></br><code>MountVolume.SetUp failed for volume ... read-only.</code></td>
      <td>El programa de fondo de la infraestructura de {{site.data.keyword.Bluemix_notm}} ha experimentado problemas de red. Para proteger los datos y para evitar que resulten dañados, {{site.data.keyword.Bluemix_notm}} ha desconectado automáticamente el servidor de almacenamiento en bloque para evitar operaciones de escritura en las instancias del almacenamiento en bloque.  </td>
      <td>Consulte [Almacenamiento en bloque: cambio del almacenamiento en bloque a solo lectura](#readonly_block)</td>
      </tr>
      <tr>
  <td><code>failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32</code> </td>
        <td>Desea montar una instancia existente de almacenamiento en bloque configurada con un sistema de archivos <code>XFS</code>. Cuando ha creado el PV y la PVC correspondiente, ha especificado un <code>ext4</code> o no ha especificado ningún sistema de archivos. El sistema de archivos que especifique en el PV debe ser el mismo sistema de archivos que se ha configurado en la instancia de almacenamiento en bloque. </td>
  <td>Consulte [Almacenamiento en bloque: el montaje del almacenamiento en bloque existente en un pod falla debido al sistema de archivos incorrecto](#block_filesystem)</td>
  </tr>
</tbody>
</table>

### Almacenamiento en bloque: cambio del almacenamiento en bloque a solo lectura
{: #readonly_block}

{: tsSymptoms}
Es posible que
note los siguientes síntomas:
- Cuando ejecuta `kubectl get pods -o wide`, ve que varios pods del mismo nodo trabajador quedan bloqueados en el estado `ContainerCreating` o `CrashLoopBackOff`. Todos estos pods utilizan la misma instancia de almacenamiento en bloque.
- Cuando ejecuta un mandato `kubectl describe pod`, ve el siguiente error en la sección **Events**: `MountVolume.SetUp failed for volume ... read-only`.

{: tsCauses}
Si se produce un error de red mientras un pod escribe en un volumen, la infraestructura de IBM Cloud (SoftLayer) protege los datos del volumen para que no resulten dañados cambiando el volumen a una modalidad de solo lectura. Los pods que utilizan este volumen no pueden seguir escribiendo en el volumen y dan un error.

{: tsResolve}
1. Compruebe la versión del plugin {{site.data.keyword.Bluemix_notm}} Block Storage que está instalado en el clúster.
   ```
   helm ls
   ```
   {: pre}

2. Compruebe que utiliza la [versión más reciente del plugin {{site.data.keyword.Bluemix_notm}} Block Storage](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm/ibmcloud-block-storage-plugin). Si no es así, [actualice el plugin](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in).
3. Si ha utilizado un despliegue de Kubernetes para el pod, reinicie el pod que está fallando eliminando el pod y dejando que Kubernetes lo vuelva a crear. Si no ha utilizado un despliegue, recupere el archivo YAML que se ha utilizado para crear el pod con el mandato `kubectl get pod <pod_name> -o yaml >pod.yaml`. A continuación, suprima y vuelva a crear manualmente el pod.
    ```
    kubectl delete pod <pod_name>
    ```
    {: pre}

4. Compruebe si volver a crear el pod ha resuelto el problema. Si no es así, vuelva a cargar el nodo trabajador.
   1. Busque el nodo trabajador en el que se ejecuta el pod y anote la dirección IP privada asignada al nodo trabajador.
      ```
      kubectl describe pod <pod_name> | grep Node
      ```
      {: pre}

      Salida de ejemplo:
      ```
      Node:               10.75.XX.XXX/10.75.XX.XXX
      Node-Selectors:  <none>
      ```
      {: screen}

   2. Recupere el **ID** del nodo trabajador utilizando la dirección IP privada del paso anterior.
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

   3. [Vuelva a cargar el nodo trabajador](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload).


<br />


### Almacenamiento en bloque: el montaje del almacenamiento en bloques existente en un pod falla debido al sistema de archivos incorrecto
{: #block_filesystem}

{: tsSymptoms}
Cuando ejecuta `kubectl describe pod <pod_name>`, ve el siguiente error:
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
Puede tener un dispositivo de almacenamiento en bloques configurado con un sistema de archivos `XFS`. Para montar este dispositivo en el pod, [creó un PV](/docs/containers?topic=containers-block_storage#existing_block) que especificaba `ext4` como su sistema de archivos o bien no se especificaba ningún sistema de archivos en la sección `spec/flexVolume/fsType`. Si no se define un sistema de archivos, el valor predeterminado para el PV es `ext4`.
El PV se creó satisfactoriamente y se enlazó a la instancia de almacenamiento en bloques existente. Sin embargo, cuando se intentó montar el PV a su clúster mediante una PVC coincidente, el volumen no se pudo montar. No se puede montar una instancia de almacenamiento en bloques `XFS` con un sistema de archivos `ext4` en el pod.

{: tsResolve}
Actualice el sistema de archivos en el PV existente de `ext4` a `XFS`.

1. Liste los PV existentes en el clúster y anote el nombre del PV que ha utilizado para la instancia de almacenamiento en bloques existente.
   ```
   kubectl get pv
   ```
   {: pre}

2. Guarde el archivo YAML del PV en la máquina local.
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. Abra el archivo YAML y cambie el `fsType` de `ext4` a `xfs`.
4. Sustituya el PV en el clúster.
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. Inicie una sesión en el pod en el que ha montado el PV.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. Verifique el sistema de archivos ha cambiado a `XFS`.
   ```
   df -Th
   ```
   {: pre}

   Salida de ejemplo:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## Almacenamiento de objetos: la instalación del plugin de Helm `ibmc` de {{site.data.keyword.cos_full_notm}} falla
{: #cos_helm_fails}

{: tsSymptoms}
Cuando instala el plugin `ibmc` Helm de {{site.data.keyword.cos_full_notm}}, la instalación falla con los errores siguientes:
```
Error: symlink /Users/ibm/ibmcloud-object-storage-plugin/helm-ibmc /Users/ibm/.helm/plugins/helm-ibmc: file exists
```
{: screen}

```
Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied
```
{: screen}

{: tsCauses}
Cuando se instala el plugin `ibmc` Helm, se crea un enlace simbólico desde el directorio `./helm/plugins/helm-ibmc` al directorio donde se encuentra el plugin `ibmc` Helm en el sistema local, que normalmente se encuentra en `./ibmcloud-object-storage-plugin/helm-ibmc`. Cuando se elimina el plugin `ibmc` Helm del sistema local o se mueve el directorio del plugin `ibmc` a una ubicación distinta, el enlace simbólico no se elimina.

Si ve un error de tipo `permiso denegado`, significa no tiene los permisos de `lectura`, `escritura` y `ejecución` necesarios sobre el archivo bash de `ibmc.sh` para poder ejecutar mandatos del plugin de Helm `ibmc`. 

{: tsResolve}

**Para errores symlink**: 

1. Elimine el plugin Helm de {{site.data.keyword.cos_full_notm}}.
   ```
   rm -rf ~/.helm/plugins/helm-ibmc
   ```
   {: pre}

2. Siga los pasos de la [documentación](/docs/containers?topic=containers-object_storage#install_cos) para volver a instalar el plugin de Helm `ibmc` y el plugin {{site.data.keyword.cos_full_notm}}.

**Para errores de permisos**: 

1. Cambie los permisos para el plugin `ibmc`. 
   ```
   chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh
   ```
   {: pre}
   
2. Pruebe el plugin de Helm `ibm`. 
   ```
   helm ibmc --help
   ```
   {: pre}
   
3. [Continúe instalando el plugin {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#install_cos). 


<br />


## Almacenamiento de objetos: la PVC permanece en estado pendiente
{: #cos_pvc_pending}

{: tsSymptoms}
Cuando se crea una PVC y se ejecuta `kubectl get pvc <pvc_name>`, la PVC permanece en estado **Pending**, incluso después de esperar algún tiempo. 

{: tsCauses}
Durante la creación y el enlace de la PVC, el plugin {{site.data.keyword.cos_full_notm}} ejecuta varias tareas diferentes. Cada tarea puede fallar y provocar un mensaje de error distinto.

{: tsResolve}

1. Localice la causa raíz de que la PVC permanezca en estado **Pending**. 
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. Revise las descripciones y las resoluciones de los mensajes de error comunes.
   
   <table>
   <thead>
     <th>Mensaje de error</th>
     <th>Descripción</th>
     <th>Pasos para resolverlo</th>
  </thead>
  <tbody>
    <tr>
      <td>`User doesn't have permissions to create or manage Storage`</td>
      <td>La clave de API de IAM o la clave de API de infraestructura de IBM Cloud (SoftLayer) almacenada en el secreto de Kubernetes `storage-secret-store` del clúster no tiene todos los permisos necesarios para suministrar almacenamiento persistente. </td>
      <td>Consulte [No se puede crear la PVC porque faltan permisos](#missing_permissions). </td>
    </tr>
    <tr>
      <td>`cannot get credentials: cannot get secret <secret_name>: secrets "<secret_name>" not found`</td>
      <td>El secreto de Kubernetes que contiene las credenciales de servicio de {{site.data.keyword.cos_full_notm}} no está en el mismo espacio de nombres que la PVC o el pod. </td>
      <td>Consulte [La creación de PVC o pod falla debido a que no se encuentra el secreto de Kubernetes](#cos_secret_access_fails).</td>
    </tr>
    <tr>
      <td>`cannot get credentials: Wrong Secret Type.Provided secret of type XXXX.Expected type ibm/ibmc-s3fs`</td>
      <td>El secreto de Kubernetes que ha creado para la instancia de servicio de {{site.data.keyword.cos_full_notm}} no incluye `type: ibm/ibmc-s3fs`.</td>
      <td>Edite el secreto de Kubernetes que contiene las credenciales de {{site.data.keyword.cos_full_notm}} y añada o modifique el valor de `type` por `ibm/ibmc-s3fs`. </td>
    </tr>
    <tr>
      <td>`Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>`</br> </br> `Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname< or https://<hostname>`</td>
      <td>El punto final de API s3fs o de API de IAM tiene un formato incorrecto, o no se ha podido recuperar el punto final de API s3fs en función de la ubicación del clúster.  </td>
      <td>Consulte [No se puede crear la PVC porque el punto final de API de s3fs es incorrecto](#cos_api_endpoint_failure).</td>
    </tr>
    <tr>
      <td>`object-path cannot be set when auto-create is enabled`</td>
      <td>Ha especificado un subdirectorio existente en el grupo que desea montar en la PVC utilizando la anotación `ibm.io/object-path`. Si establece un subdirectorio, debe inhabilitar la característica de creación automática (auto-create) del grupo.  </td>
      <td>En la PVC, establezca `ibm.io/auto-create-bucket: "false"` y especifique el nombre del grupo existente en `ibm.io/bucket`.</td>
    </tr>
    <tr>
    <td>`bucket auto-create must be enabled when bucket auto-delete is enabled`</td>
    <td>En la PVC, ha establecido `ibm.io/auto-delete-bucket: true` para suprimir automáticamente los datos, el grupo y el PV cuando elimine la PVC. Esta opción necesita que `ibm.io/auto-create-bucket` se establezca en <strong>true</strong> y que `ibm.io/bucket` se establezca en `""` a la vez.</td>
    <td>En la PVC, establezca `ibm.io/auto-create-bucket: true` e `ibm.io/bucket: ""` de modo que el grupo se cree automáticamente con un nombre con el formato `tmp-s3fs-xxxx`. </td>
    </tr>
    <tr>
    <td>`bucket cannot be set when auto-delete is enabled`</td>
    <td>En la PVC, ha establecido `ibm.io/auto-delete-bucket: true` para suprimir automáticamente los datos, el grupo y el PV cuando elimine la PVC. Esta opción necesita que `ibm.io/auto-create-bucket` se establezca en <strong>true</strong> y que `ibm.io/bucket` se establezca en `""` a la vez.</td>
    <td>En la PVC, establezca `ibm.io/auto-create-bucket: true` e `ibm.io/bucket: ""` de modo que el grupo se cree automáticamente con un nombre con el formato `tmp-s3fs-xxxx`. </td>
    </tr>
    <tr>
    <td>`cannot create bucket using API key without service-instance-id`</td>
    <td>Si desea utilizar las claves de API de IAM para acceder a su instancia de servicio de {{site.data.keyword.cos_full_notm}}, debe almacenar la clave de API y el ID de la instancia de servicio de {{site.data.keyword.cos_full_notm}} en un secreto de Kubernetes.  </td>
    <td>Consulte [Creación de un secreto para las credenciales de servicio de almacenamiento de objetos](/docs/containers?topic=containers-object_storage#create_cos_secret). </td>
    </tr>
    <tr>
      <td>`object-path “<subdirectory_name>” not found inside bucket <bucket_name>`</td>
      <td>Ha especificado un subdirectorio existente en el grupo que desea montar en la PVC utilizando la anotación `ibm.io/object-path`. Este subdirectorio no se ha encontrado en el grupo que ha especificado. </td>
      <td>Verifique que el subdirectorio que ha especificado en `ibm.io/object-path` existe en el grupo que ha especificado en `ibm.io/bucket`. </td>
    </tr>
        <tr>
          <td>`BucketAlreadyExists: The requested bucket name is not available. The bucket namespace is shared by all users of the system. Please select a different name and try again.`</td>
          <td>Ha establecido `ibm.io/auto-create-bucket: true` y ha especificado un nombre de grupo al mismo tiempo, o bien ha especificado un nombre de grupo que ya existe en {{site.data.keyword.cos_full_notm}}. Los nombres de grupo deben ser exclusivos en todas las instancias de servicio y regiones de {{site.data.keyword.cos_full_notm}}.  </td>
          <td>Asegúrese de que ha establecido `ibm.io/auto-create-bucket: false` y de que ha especificado un nombre de grupo que es exclusivo en {{site.data.keyword.cos_full_notm}}. Si desea utilizar el plugin {{site.data.keyword.cos_full_notm}} para crear automáticamente un nombre de grupo, establezca `ibm.io/auto-create-bucket: true` e `ibm.io/bucket: ""`. El grupo se crea con un nombre exclusivo en el formato `tmp-s3fs-xxxx`. </td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: NotFound: Not Found`</td>
          <td>Ha intentado acceder a un grupo que no ha creado, o bien la clase de almacenamiento y el punto final de API s3fs que ha especificado no coinciden con la clase de almacenamiento y el punto final de API s3fs utilizados cuando se creó el grupo. </td>
          <td>Consulte [No se puede acceder a un grupo existente](#cos_access_bucket_fails). </td>
        </tr>
        <tr>
          <td>`Put https://s3-api.dal-us-geo.objectstorage.service.networklayer.com/<bucket_name>: net/http: invalid header field value "AWS4-HMAC-SHA256 Credential=1234a12a123a123a1a123aa1a123a123\n\n/20190412/us-standard/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=12aa1abc123456aabb12aas12aa123456sb123456abc" for key Authorization`</td>
          <td>Los valores del secreto de Kubernetes no se han codificado correctamente en base64.  </td>
          <td>Revise los valores del secreto de Kubernetes y codifique cada valor en base64. También puede utilizar el mandato [`kubectl create secret`](/docs/containers?topic=containers-object_storage#create_cos_secret) para crear un nuevo secreto y dejar que Kubernetes codifique automáticamente sus valores en base64. </td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: Forbidden: Forbidden`</td>
          <td>Ha especificado `ibm.io/auto-create-bucket: false` y ha intentado acceder a un grupo que no ha creado o bien la clave de acceso al servicio o el ID de la clave de acceso de las credenciales de HMAC de {{site.data.keyword.cos_full_notm}} son incorrectos.  </td>
          <td>No puede acceder a un grupo que no ha creado. Cree un nuevo grupo estableciendo `ibm.io/auto-create-bucket: true` e `ibm.io/bucket: ""`. Si es el propietario del grupo, consulte [La creación de la PVC falla debido a credenciales erróneas o acceso denegado](#cred_failure) para comprobar sus credenciales. </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessDenied: Access Denied`</td>
          <td>Ha especificado `ibm.io/auto-create-bucket: true` para crear automáticamente un grupo en {{site.data.keyword.cos_full_notm}}, pero las credenciales que ha especificado en el secreto de Kubernetes tienen asignado el rol de acceso al servicio de **Lector** de IAM. Este rol no permite crear un grupo en {{site.data.keyword.cos_full_notm}}. </td>
          <td>Consulte [La creación de la PVC falla debido a credenciales erróneas o acceso denegado](#cred_failure). </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessForbidden: Access Forbidden`</td>
          <td>Ha especificado `ibm.io/auto-create-bucket: true` y ha proporcionado un nombre de un grupo existente en `ibm.io/bucket`. Además, las credenciales que ha especificado en el secreto de Kubernetes tienen asignado el rol de acceso al servicio de **Lector** de IAM. Este rol no permite crear un grupo en {{site.data.keyword.cos_full_notm}}. </td>
          <td>Para utilizar un grupo existente, establezca `ibm.io/auto-create-bucket: false` y especifique el nombre del grupo existente en `ibm.io/bucket`. Para crear automáticamente un grupo utilizando el secreto de Kubernetes existente, establezca `ibm.io/bucket: ""` y siga [La creación de la PVC falla debido a credenciales erróneas o acceso denegado](#cred_failure) para verificar las credenciales del secreto de Kubernetes.</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details`</td>
          <td>La clave de acceso al secreto de {{site.data.keyword.cos_full_notm}} de las credenciales de HMAC que ha proporcionado en el secreto de Kubernetes no es correcta. </td>
          <td>Consulte [La creación de la PVC falla debido a credenciales erróneas o acceso denegado](#cred_failure).</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`</td>
          <td>El ID de clave de acceso o la clave de acceso al secreto de {{site.data.keyword.cos_full_notm}} de las credenciales de HMAC que ha proporcionado en el secreto de Kubernetes no son correctos.</td>
          <td>Consulte [La creación de la PVC falla debido a credenciales erróneas o acceso denegado](#cred_failure). </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: CredentialsEndpointError: failed to load credentials` </br> </br> `cannot access bucket <bucket_name>: CredentialsEndpointError: failed to load credentials`</td>
          <td>La clave de la API de {{site.data.keyword.cos_full_notm}} de las credenciales de IAM y el GUID de la instancia de servicio de {{site.data.keyword.cos_full_notm}} no son correctos.</td>
          <td>Consulte [La creación de la PVC falla debido a credenciales erróneas o acceso denegado](#cred_failure). </td>
        </tr>
  </tbody>
    </table>
    

### Almacenamiento de objetos: la creación de PVC o pod falla debido a que no encuentra el secreto de Kubernetes
{: #cos_secret_access_fails}

{: tsSymptoms}
Cuando se crea la PVC o se despliega un pod que monta la PVC, la creación o el despliegue fallan.

- Mensaje de error de ejemplo para un error de creación de PVC:
  ```
  cannot get credentials: cannot get secret tsecret-key: secrets "secret-key" not found
  ```
  {: screen}

- Mensaje de error de ejemplo para un error de creación de pod:
  ```
  persistentvolumeclaim "pvc-3" not found (repeated 3 times)
  ```
  {: screen}

{: tsCauses}
El secreto de Kubernetes donde se almacenan las credenciales de servicio de {{site.data.keyword.cos_full_notm}}, la PVC y el pod no están en el mismo espacio de nombres de Kubernetes. Cuando se despliega el secreto en un espacio de nombres distinto que la PVC o el pod, no se puede acceder al secreto.

{: tsResolve}
Esta tarea requiere el [rol de servicio de {{site.data.keyword.Bluemix_notm}} IAM de **Escritor** o de **Gestor**](/docs/containers?topic=containers-users#platform) sobre todos los espacios de nombres.

1. Liste los secretos del clúster y revise el espacio de nombres de Kubernetes en el que se ha creado el secreto de Kubernetes para la instancia de servicio de {{site.data.keyword.cos_full_notm}}. El secreto debe mostrar `ibm/ibmc-s3fs` como **Tipo**.
   ```
   kubectl get secrets --all-namespaces
   ```
   {: pre}

2. Compruebe el archivo de configuración YAML para la PVC y el pod para verificar que ha utilizado el mismo espacio de nombres. Si desea desplegar un pod en un espacio de nombres distinto al del secreto, [cree otro secreto](/docs/containers?topic=containers-object_storage#create_cos_secret) en dicho espacio de nombres.

3. Cree la PVC o despliegue el pod en el espacio de nombres adecuado.

<br />


### Almacenamiento de objetos: la creación de la PVC falla debido a credenciales erróneas o acceso denegado
{: #cred_failure}

{: tsSymptoms}
Cuando crea la PVC, ve un mensaje de error similar a uno de los siguientes:

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

```
InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`
```
{: screen}

```
cannot access bucket <bucket_name>: Forbidden: Forbidden
```
{: screen}


{: tsCauses}
Las credenciales de servicio de {{site.data.keyword.cos_full_notm}} que utiliza para acceder a la instancia de servicio pueden ser erróneas, o permitir solo el acceso de lectura al grupo.

{: tsResolve}
1. En la navegación de la página de detalles de servicio, pulse **Credenciales de servicio**.
2. Busque las credenciales y, a continuación, pulse **Ver credenciales**.
3. En la sección **iam_role_crn**, verifique que tiene el rol de `Escritor` o `Gestor`. Si no tiene el rol correcto, debe crear nuevas credenciales de servicio de {{site.data.keyword.cos_full_notm}} con el permiso correcto. 
4. Si el rol es correcto, verifique que utiliza el valor correcto de **access_key_id** y **secret_access_key** en el secreto de Kubernetes. 
5. [Cree un nuevo secreto con los valores actualizados de **access_key_id** y **secret_access_key**](/docs/containers?topic=containers-object_storage#create_cos_secret). 


<br />


### Almacenamiento de objetos: la creación de la PVC falla porque el punto final de API de IAM o s3fs es incorrecto
{: #cos_api_endpoint_failure}

{: tsSymptoms}
Cuando se crea el PVC, esta permanece en un estado pendiente. Después de ejecutar el mandato `kubectl describe pvc <pvc_name>`, aparece uno de los siguientes errores: 

```
Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

```
Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

{: tsCauses}
Es posible que el punto final de API de s3fs del grupo que desea utilizar tenga un formato incorrecto o que el clúster se haya desplegado en una ubicación que recibe soporte en {{site.data.keyword.containerlong_notm}} pero que no recibe soporte actualmente del plugin {{site.data.keyword.cos_full_notm}}. 

{: tsResolve}
1. Compruebe el punto final de API de s3fs que ha asignado automáticamente el plugin de Helm `ibmc` a las clases de almacenamiento durante la instalación del plugin {{site.data.keyword.cos_full_notm}}. El punto final se basa en la ubicación en la que se ha desplegado el clúster.  
   ```
   kubectl get sc ibmc-s3fs-standard-regional -o yaml | grep object-store-endpoint
   ```
   {: pre}

   Si el mandato devuelve `ibm.io/object-store-endpoint: NA`, significa que el clúster se ha desplegado en una ubicación que recibe soporte en {{site.data.keyword.containerlong_notm}}, pero que no recibe soporte actualmente del plugin {{site.data.keyword.cos_full_notm}}. Para añadir la ubicación a {{site.data.keyword.containerlong_notm}}, publique una pregunta en nuestro Slack público o abra un caso de soporte de {{site.data.keyword.Bluemix_notm}}. Para obtener más información, consulte [Obtención de ayuda y soporte](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help). 
   
2. Si ha añadido el punto final de API de s3fs manualmente con la anotación `ibm.io/endpoint` o el punto final de API de IAM con la anotación `ibm.io/iam-endpoint` en la PVC, asegúrese de que ha añadido los puntos finales en el formato `https://<s3fs_api_endpoint>` y `https://<iam_api_endpoint>`. La anotación sobrescribe los puntos finales de API que ha establecido automáticamente el plugin `ibmc` en las clases de almacenamiento de {{site.data.keyword.cos_full_notm}}. 
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}
   
<br />


### Almacenamiento de objetos: no se puede acceder a un grupo existente
{: #cos_access_bucket_fails}

{: tsSymptoms}
Cuando se crea la PVC, no se puede acceder al grupo de {{site.data.keyword.cos_full_notm}}. Aparece un mensaje de error similar al siguiente:

```
Failed to provision volume with StorageClass "ibmc-s3fs-standard-regional": pvc:1b2345678b69175abc98y873e2:cannot access bucket <bucket_name>: NotFound: Not Found
```
{: screen}

{: tsCauses}
Es posible que haya utilizado la clase de almacenamiento errónea para acceder al grupo, o que haya intentado acceder a un grupo que no ha creado. No puede acceder a un grupo que no ha creado. 

{: tsResolve}
1. Desde el [panel de control de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/), seleccione la instancia de servicio de {{site.data.keyword.cos_full_notm}}.
2. Seleccione **Grupos**.
3. Revise la información de **Clase** y **Ubicación** para el grupo existente.
4. Elija la [clase de almacenamiento](/docs/containers?topic=containers-object_storage#cos_storageclass_reference) adecuada.
5. Asegúrese de que ha establecido `ibm.io/auto-create-bucket: false` y de que ha especificado el nombre correcto del grupo existente. 

<br />


## Almacenamiento de objetos: falla el acceso a los archivos con un usuario no root
{: #cos_nonroot_access}

{: tsSymptoms}
Ha subido archivos a la instancia de servicio de {{site.data.keyword.cos_full_notm}} utilizando la consola o la API REST. Cuando intenta acceder a estos archivos con un usuario no root que ha definido con `runAsUser` en el despliegue de la app, se deniega el acceso a los archivos.

{: tsCauses}
En Linux, un archivo o un directorio tienen 3 grupos de acceso: `Owner`, `Group` y `Other`. Cuando sube un archivo a {{site.data.keyword.cos_full_notm}} mediante la consola o la API REST, se eliminan los permisos de `Owner`, `Group` y `Other`. El permiso de cada archivo tiene el aspecto siguiente:

```
d--------- 1 root root 0 Jan 1 1970 <file_name>
```
{: screen}

Cuando carga un archivo utilizando el plugin de {{site.data.keyword.cos_full_notm}}, los permisos para el archivo se conservan y no se modifican.

{: tsResolve}
Para acceder al archivo con un usuario no root, el usuario no root debe tener permisos de lectura y escritura en el archivo. Para cambiar el permiso de un archivo como parte del despliegue del pod, es necesaria una operación de escritura. {{site.data.keyword.cos_full_notm}} no está diseñado para cargas de trabajo de escritura. La actualización de permisos durante el despliegue de pod puede impedir que el pod pase a un estado `En ejecución`.

Para resolver este problema, antes de montar la PVC en el pod de la app, cree otro pod para establecer el permiso correcto para el usuario no root.

1. Compruebe los permisos de los archivos en el grupo.
   1. Cree un archivo de configuración para el pod `test-permission` y nombre el archivo `test-permission.yaml`.
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
            claimName: <pvc_name>
      ```
      {: codeblock}

   2. Cree el pod `test-permission`.
      ```
      kubectl apply -f test-permission.yaml
      ```
      {: pre}

   3. Inicie una sesión en el pod.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   4. Vaya a la vía de acceso de montaje y liste los permisos de los archivos.
      ```
      cd test && ls -al
      ```
      {: pre}

      Salida de ejemplo:
      ```
      d--------- 1 root root 0 Jan 1 1970 <file_name>
      ```
      {: screen}

2. Suprima el pod.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

3. Cree un archivo de configuración para el pod que utilice para corregir los permisos de los archivos y denomínelo `fix-permission.yaml`.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: fix-permission 
     namespace: <namespace>
   spec:
     containers:
     - name: fix-permission
       image: busybox
       command: ['sh', '-c']
       args: ['chown -R <nonroot_userID> <mount_path>/*; find <mount_path>/ -type d -print -exec chmod u=+rwx,g=+rx {} \;']
       volumeMounts:
       - mountPath: "<mount_path>"
         name: cos-volume
     volumes:
     - name: cos-volume
       persistentVolumeClaim:
         claimName: <pvc_name>
    ```
    {: codeblock}

3. Cree el pod `fix-permission`.
   ```
   kubectl apply -f fix-permission.yaml
   ```
   {: pre}

4. Espere a que el pod pase al estado `Completed`.  
   ```
   kubectl get pod fix-permission
   ```
   {: pre}

5. Suprima el pod `fix-permission`.
   ```
   kubectl delete pod fix-permission
   ```
   {: pre}

5. Vuelva a crear el pod `test-permission` que ha utilizado anteriormente para comprobar los permisos.
   ```
   kubectl apply -f test-permission.yaml
   ```
   {: pre}

5. Verifique que los permisos de los archivos se han actualizado.
   1. Inicie una sesión en el pod.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   2. Vaya a la vía de acceso de montaje y liste los permisos de los archivos.
      ```
      cd test && ls -al
      ```
      {: pre}

      Salida de ejemplo:
      ```
      -rwxrwx--- 1 <nonroot_userID> root 6193 Aug 21 17:06 <file_name>
      ```
      {: screen}

6. Suprima el pod `test-permission`.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

7. Monte la PVC en la app con el usuario no root.

   Defina el usuario no root como `runAsUser` sin establecer `fsGroup` en el YAML de despliegue al mismo tiempo. Al establecer `fsGroup`, se provoca que el plugin de {{site.data.keyword.cos_full_notm}} actualice los permisos de grupo para todos los archivos de un grupo cuando se despliega el pod. La actualización de los permisos es una operación de escritura y puede impedir que el pod pase a un estado `En ejecución`.
   {: important}

Después de establecer los permisos de archivo correctos en la instancia de servicio de {{site.data.keyword.cos_full_notm}}, no cargue archivos utilizando la consola ni la API REST. Utilice el plugin de {{site.data.keyword.cos_full_notm}} para añadir archivos a la instancia de servicio.
{: tip}

<br />


   
## No se puede crear la PVC porque faltan permisos
{: #missing_permissions}

{: tsSymptoms}
Cuando se crea una PVC, esta permanece en un estado pendiente. Cuando se ejecuta `kubectl describe pvc <pvc_name>`, aparece un mensaje de error similar al siguiente: 

```
User doesn't have permissions to create or manage Storage
```
{: screen}

{: tsCauses}
La clave de API de IAM o la clave de API de infraestructura de IBM Cloud (SoftLayer) almacenada en el secreto de Kubernetes `storage-secret-store` del clúster no tiene todos los permisos necesarios para suministrar almacenamiento persistente. 

{: tsResolve}
1. Recupere la clave de IAM o la clave de API de infraestructura de IBM Cloud (SoftLayer) almacenada en el secreto de Kubernetes `storage-secret-store` del clúster y verifique que se ha utilizado la clave de API correcta. 
   ```
   kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
   ```
   {: pre}
   
   Salida de ejemplo: 
   ```
   [Bluemix]
   iam_url = "https://iam.bluemix.net"
   iam_client_id = "bx"
   iam_client_secret = "bx"
   iam_api_key = "<iam_api_key>"
   refresh_token = ""
   pay_tier = "paid"
   containers_api_route = "https://us-south.containers.bluemix.net"

   [Softlayer]
   encryption = true
   softlayer_username = ""
   softlayer_api_key = ""
   softlayer_endpoint_url = "https://api.softlayer.com/rest/v3"
   softlayer_iam_endpoint_url = "https://api.softlayer.com/mobile/v3"
   softlayer_datacenter = "dal10"
   ```
   {: screen}
   
   La clave de API de IAM se muestra en la sección `Bluemix.iam_api_key` de la salida de la CLI. Si `Softlayer.softlayer_api_key` está vacío a la vez, se utiliza la clave de API de IAM para determinar los permisos de la infraestructura. El usuario que ejecuta la primera acción que necesita el rol de plataforma de **Administrador** de IAM en un grupo de recursos y región establece automáticamente la clave de API de IAM. Si se ha establecido otra clave de API en `Softlayer.softlayer_api_key`, esta clave prevalece sobre la clave de API de IAM. `Softlayer.softlayer_api_key` se establece cuando un administrador del clúster ejecuta el mandato `ibmcloud ks credentials-set`. 
   
2. Si desea cambiar las credenciales, actualice la clave de API que se utiliza. 
    1.  Para actualizar la clave de API de IAM, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset) `ibmcloud ks api-key-reset`. Para actualizar la clave de la infraestructura de IBM Cloud (SoftLayer), utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) `ibmcloud ks credential-set`.
    2. Espere entre 10 y 15 minutos hasta que se actualice el secreto de Kubernetes `storage-secret-store` y luego verifique que la clave se ha actualizado.
       ```
       kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
       ```
       {: pre}
   
3. Si la clave de API es correcta, verifique que la clave tiene el permiso correcto para suministrar almacenamiento persistente.
   1. Póngase en contacto con el propietario de la cuenta para verificar el permiso de la clave de API. 
   2. Como propietario de la cuenta, seleccione **Gestionar** > **Acceso (IAM)** en el área de navegación de la consola de {{site.data.keyword.Bluemix_notm}}.
   3. Seleccione **Usuarios** y localice el usuario cuya clave de API desea utilizar. 
   4. En el menú de acciones, seleccione **Gestionar detalles de usuario**. 
   5. Vaya al separador **Infraestructura clásica**. 
   6. Amplíe la categoría **Cuenta** y verifique que se ha asignado el permiso **Añadir/actualizar almacenamiento (StorageLayer)**. 
   7. Amplíe la categoría **Servicios** y verifique que se ha asignado el permiso **Gestión de almacenamiento**. 
4. Elimine la PVC que ha fallado. 
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}
   
5. Vuelva a crear la PVC. 
   ```
   kubectl apply -f pvc.yaml
   ```
   {: pre}


## Obtención de ayuda y soporte
{: #storage_getting_help}

¿Sigue teniendo problemas con su clúster?
{: shortdesc}

-  En el terminal, se le notifica cuando están disponibles las actualizaciones de la CLI y los plugins de `ibmcloud`. Asegúrese de mantener actualizada la CLI para poder utilizar todos los mandatos y distintivos disponibles.
-   Para ver si {{site.data.keyword.Bluemix_notm}} está disponible, [consulte la página de estado de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/status?selected=status).
-   Publique una pregunta en [Slack de {{site.data.keyword.containerlong_notm}}![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-container-service.slack.com).
    Si no utiliza un ID de IBM para la cuenta de {{site.data.keyword.Bluemix_notm}}, [solicite una invitación](https://bxcs-slack-invite.mybluemix.net/) a este Slack.
    {: tip}
-   Revise los foros para ver si otros usuarios se han encontrado con el mismo problema. Cuando utiliza los foros para formular una pregunta, etiquete la pregunta para que la puedan ver los equipos de desarrollo de {{site.data.keyword.Bluemix_notm}}.
    -   Si tiene preguntas técnicas sobre el desarrollo o despliegue de clústeres o apps con {{site.data.keyword.containerlong_notm}}, publique su pregunta en [Stack Overflow ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) y etiquete su pregunta con `ibm-cloud`, `kubernetes` y `containers`.
    -   Para formular preguntas sobre el servicio y obtener instrucciones de iniciación, utilice el foro [IBM Developer Answers ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Incluya las etiquetas `ibm-cloud` y `containers`.
    Consulte [Obtención de ayuda](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) para obtener más detalles sobre cómo utilizar los foros.
-   Póngase en contacto con el soporte de IBM abriendo un caso. Para obtener información sobre cómo abrir un caso de soporte de IBM, o sobre los niveles de soporte y las gravedades de los casos, consulte [Cómo contactar con el servicio de soporte](/docs/get-support?topic=get-support-getting-customer-support).
Al informar de un problema, incluya el ID de clúster. Para obtener el ID de clúster, ejecute `ibmcloud ks clusters`. También puede utilizar la [herramienta de diagnósticos y de depuración de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) para recopilar y exportar la información pertinente del clúster que se va a compartir con el servicio de soporte de IBM.
{: tip}

