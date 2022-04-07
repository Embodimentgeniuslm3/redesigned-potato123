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
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}


# Almacenamiento de datos en IBM Cloud Object Storage
{: #object_storage}

[{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about) es un almacenamiento persistente y de alta disponibilidad que puede montar en las apps que se ejecutan en el clúster de Kubernetes mediante el plugin {{site.data.keyword.cos_full_notm}}. Este plugin es un plugin de Kubernetes Flex-Volume que conecta grupos de {{site.data.keyword.cos_short}} de la nube con pods del clúster. La información que se almacenan con {{site.data.keyword.cos_full_notm}} está cifrada y se distribuye entre varias ubicaciones geográficas. Se accede a la misma sobre HTTP utilizando una API REST.
{: shortdesc}

Para conectar con {{site.data.keyword.cos_full_notm}}, el clúster requiere acceso a la red pública para autenticarse con {{site.data.keyword.Bluemix_notm}} Identity and Access Management. Si tiene un clúster solo privado, puede comunicar con el punto final de servicio privado de {{site.data.keyword.cos_full_notm}} si instala la versión `1.0.3` o posterior del plugin y configura la instancia de servicio de {{site.data.keyword.cos_full_notm}} para la autenticación de HMAC. Si no desea utilizar la autenticación HMAC, debe abrir todo el tráfico de red de salida en el puerto 443 para que el plugin funcione correctamente en un clúster privado.
{: important}

Con la versión 1.0.5, se ha cambiado el nombre del plugin de {{site.data.keyword.cos_full_notm}} de `ibmcloud-object-storage-plugin` a `ibm-object-storage-plugin`. Para instalar la nueva versión del plugin, debe [desinstalar la instalación del diagrama de Helm antiguo](#remove_cos_plugin) y [volver a instalar el diagrama de Helm con la nueva versión del plugin de {{site.data.keyword.cos_full_notm}}](#install_cos).
{: note}

## Creación de la instancia de servicio de almacenamiento de objetos
{: #create_cos_service}

Para poder empezar a utilizar el almacenamiento de objetos en el clúster, debe suministrar una instancia de servicio de {{site.data.keyword.cos_full_notm}} en la cuenta.
{: shortdesc}

El plugin {{site.data.keyword.cos_full_notm}} se configura para que funcione con cualquier punto final de API s3. Por ejemplo, es posible que desee utilizar un servidor de almacenamiento de objetos de nube local, como por ejemplo [Minio](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm-charts/ibm-minio-objectstore), o conectarse a un punto final de API s3 que configure en un proveedor de nube distinto en lugar de utilizar una instancia de servicio de {{site.data.keyword.cos_full_notm}}.

Siga estos pasos para crear una instancia de servicio de {{site.data.keyword.cos_full_notm}}. Si tiene previsto utilizar un servidor de almacenamiento de objetos de nube local o un punto final de API s3 diferente, consulte la documentación del proveedor para configurar la instancia de almacenamiento de objetos de nube.

1. Despliegue una instancia de servicio de {{site.data.keyword.cos_full_notm}}.
   1.  Abra la [página del catálogo de {{site.data.keyword.cos_full_notm}}](https://cloud.ibm.com/catalog/services/cloud-object-storage).
   2.  Especifique un nombre para la instancia de servicio, como por ejemplo `cos-backup`, y seleccione el grupo de recursos en el que se encuentra el clúster. Para ver el grupo de recursos de su clúster, ejecute `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`.   
   3.  Consulte las [opciones de plan ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) para conocer la información de tarifas y seleccione un plan.
   4.  Pulse **Crear**. Se abre la página de detalles de servicio.
2. {: #service_credentials}Recupere las credenciales de servicio de {{site.data.keyword.cos_full_notm}}.
   1.  En la navegación de la página de detalles de servicio, pulse **Credenciales de servicio**.
   2.  Pulse **Nueva credencial**. Aparece un recuadro de diálogo.
   3.  Especifique un nombre para las credenciales.
   4.  En el menú desplegable **Rol**, seleccione `Escritor` o `Gestor`. Si selecciona `Lector`, no puede utilizar las credenciales para crear grupos en {{site.data.keyword.cos_full_notm}} y escribir datos en ellos.
   5.  Opcional: en **Añadir parámetros de configuración en línea (opcional)**, especifique `{"HMAC":true}` para crear credenciales de HMAC adicionales para el servicio de {{site.data.keyword.cos_full_notm}}. La autenticación HMAC añade un nivel adicional de seguridad a la autenticación OAuth2, ya que evita el mal uso de las señales OAuth2 caducadas o creadas de forma aleatoria. **Importante**: Si tiene un clúster solo privado sin acceso público, debe utilizar la autenticación HMAC para poder acceder al servicio {{site.data.keyword.cos_full_notm}} a través de la red privada.
   6.  Pulse **Añadir**. Sus nuevas credenciales aparecerán en la tabla **Credenciales de servicio**.
   7.  Pulse **Ver credenciales**.
   8.  Tome nota del valor de **apikey** para utilizar las señales OAuth2 para autenticarse con el servicio de {{site.data.keyword.cos_full_notm}}. Para la autenticación HMAC, en la sección **cos_hmac_keys**, tenga en cuenta el valor de **access_key_id** y de **secret_access_key**.
3. [Almacene las credenciales de servicio en un secreto de Kubernetes dentro del clúster](#create_cos_secret) para habilitar el acceso a la instancia de servicio de {{site.data.keyword.cos_full_notm}}.

## Creación de un secreto para las credenciales de servicio de almacenamiento de objetos
{: #create_cos_secret}

Para acceder a la instancia de servicio de {{site.data.keyword.cos_full_notm}} para leer y escribir datos, debe almacenar de forma segura las credenciales de servicio en un secreto de Kubernetes. El plugin de {{site.data.keyword.cos_full_notm}} utiliza estas credenciales para cada operación de lectura o escritura en el grupo.
{: shortdesc}

Siga estos pasos para crear un secreto de Kubernetes para las credenciales de una instancia de servicio de {{site.data.keyword.cos_full_notm}}. Si tiene previsto utilizar un servidor de almacenamiento de objetos de nube local o un punto final de API s3 distinto, cree un secreto de Kubernetes con las credenciales adecuadas.

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Recupere los valores de **apikey** o de **access_key_id** y **secret_access_key** de las [credenciales de servicio de {{site.data.keyword.cos_full_notm}}](#service_credentials).

2. Obtenga el **GUID** de la instancia de servicio de {{site.data.keyword.cos_full_notm}}.
   ```
   ibmcloud resource service-instance <service_name> | grep GUID
   ```
   {: pre}

3. Cree un secreto de Kubernetes para almacenar las credenciales de servicio. Cuando se crea el secreto, todos los valores se codifican automáticamente en base64.

   **Ejemplo de uso de la clave de API:**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=api-key=<api_key> --from-literal=service-instance-id=<service_instance_guid>
   ```
   {: pre}

   **Ejemplo de autenticación HMAC:**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=access-key=<access_key_ID> --from-literal=secret-key=<secret_access_key>    
   ```
   {: pre}

   <table>
   <caption>Componentes de mandatos</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de mandatos</th>
   </thead>
   <tbody>
   <tr>
   <td><code>api-key</code></td>
   <td>Especifique la clave de API que ha recuperado de las credenciales de servicio de {{site.data.keyword.cos_full_notm}} anteriormente. Si desea utilizar la autenticación HMAC, especifique en su lugar la <code>access-key</code> y la <code>secret-key</code>.  </td>
   </tr>
   <tr>
   <td><code>access-key</code></td>
   <td>Especifique el ID de clave de acceso que ha recuperado de las credenciales de servicio de {{site.data.keyword.cos_full_notm}} anteriormente. Si desea utilizar la autenticación OAuth2, especifique la <code>api-key</code> en su lugar.  </td>
   </tr>
   <tr>
   <td><code>secret-key</code></td>
   <td>Especifique la clave de acceso secreta que ha recuperado de las credenciales de servicio de {{site.data.keyword.cos_full_notm}} anteriormente. Si desea utilizar la autenticación OAuth2, especifique la <code>api-key</code> en su lugar.</td>
   </tr>
   <tr>
   <td><code>service-instance-id</code></td>
   <td>Especifique el GUID de la instancia de servicio de {{site.data.keyword.cos_full_notm}} que ha recuperado anteriormente. </td>
   </tr>
   </tbody>
   </table>

4. Verifique que el secreto se haya creado en el espacio de nombres.
   ```
   kubectl get secret
   ```
   {: pre}

5. [Instale el plugin de {{site.data.keyword.cos_full_notm}}](#install_cos) o, si ya ha instalado el plugin, [decida la configuración]( #configure_cos) del grupo de {{site.data.keyword.cos_full_notm}}.

## Instalación del plugin de IBM Cloud Object Storage
{: #install_cos}

Instale el plugin de {{site.data.keyword.cos_full_notm}} con un diagrama de Helm para configurar las clases de almacenamiento predefinidas para {{site.data.keyword.cos_full_notm}}. Utilice estas clases de almacenamiento para crear una PVC para suministrar {{site.data.keyword.cos_full_notm}} para sus apps.
{: shortdesc}

¿Está buscando instrucciones sobre cómo actualizar o eliminar el plugin de {{site.data.keyword.cos_full_notm}}? Consulte [Actualización del plugin](#update_cos_plugin) y [Eliminación del plugin](#remove_cos_plugin).
{: tip}

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

2.  Elija si desea instalar el plugin de {{site.data.keyword.cos_full_notm}} con o sin el servidor Helm, Tiller. A continuación, [siga las instrucciones](/docs/containers?topic=containers-helm#public_helm_install) para instalar el cliente Helm en la máquina local, y, si desea utilizar Tiller, para instalar Tiller con una cuenta de servicio en el clúster.

3. Si desea instalar el plugin con Tiller, verifique que Tiller esté instalado con una cuenta de servicio.
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

4. Añada el repositorio de {{site.data.keyword.Bluemix_notm}} Helm al clúster.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

4. Actualice el repositorio de Helm para recuperar la última versión de todos los diagramas de Helm de este repositorio.
   ```
   helm repo update
   ```
   {: pre}

5. Descargue los diagramas de Helm y desempaquete los diagramas en el directorio actual.
   ```
   helm fetch --untar iks-charts/ibm-object-storage-plugin
   ```
   {: pre}

7. Si utiliza una distribución OS X o Linux, instale el plugin Helm de {{site.data.keyword.cos_full_notm}} en `ibmc`. El plugin se utiliza para recuperar automáticamente la ubicación del cluster y definir el punto final de API para los grupos de {{site.data.keyword.cos_full_notm}} de las clases de almacenamiento. Si utiliza Windows como sistema operativo, continúe con el paso siguiente.
   1. Instale el plugin de Helm.
      ```
      helm plugin install ./ibm-object-storage-plugin/helm-ibmc
      ```
      {: pre}

      Salida de ejemplo:
      ```
      Installed plugin: ibmc
      ```
      {: screen}
      
      Si ve el error `Error: el plugin ya existe`, elimine el plugin de Helm `ibmc` ejecutando `rm -rf ~/.helm/plugins/helm-ibmc`. 
      {: tip}

   2. Verifique que el plugin `ibmc` esté instalado correctamente.
      ```
      helm ibmc --help
      ```
      {: pre}

      Salida de ejemplo:
      ```
      Instale o actualice diagramas de Helm en IBM K8S Service (IKS) e IBM Cloud Private (ICP)

      Mandatos disponibles:
         helm ibmc install [CHART][flags]                      Instalar un diagrama de Helm
         helm ibmc upgrade [RELEASE][CHART] [flags]            Actualizar el release a una nueva versión del diagrama de Helm
         helm ibmc template [CHART][flags] [--apply|--delete]  Instalar o desinstalar un diagrama de Helm sin tiller

      Distintivos disponibles:
         -h, --help                    (Opcional) Este texto.
         -u, --update                  (Opcional) Actualizar este plugin a la versión más reciente

      Ejemplo de uso:
         Con Tiller:
             Install:   helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
         Sin Tiller:
             Install:   helm ibmc template iks-charts/ibm-object-storage-plugin --apply
             Dry-run:   helm ibmc template iks-charts/ibm-object-storage-plugin
             Uninstall: helm ibmc template iks-charts/ibm-object-storage-plugin --delete

      Nota:
         1. Siempre se recomienda instalar la versión más reciente del diagrama de ibm-object-storage-plugin.
         2. Siempre se recomienda tener el cliente 'kubectl' actualizado.
      ```
      {: screen}

      Si la salida muestra el error `Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permiso denegado`, ejecute `chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh`. Luego vuelva a ejecutar `helm ibmc --help`.
      {: tip}

8. Opcional: limite el plugin de {{site.data.keyword.cos_full_notm}} para acceder únicamente a los secretos de Kubernetes que contienen las credenciales de servicio de {{site.data.keyword.cos_full_notm}}. De forma predeterminada, el plugin está autorizado para acceder a todos los secretos de Kubernetes del clúster.
   1. [Cree la instancia de servicio de {{site.data.keyword.cos_full_notm}}](#create_cos_service).
   2. [Almacene las credenciales de servicio de {{site.data.keyword.cos_full_notm}} en un secreto de Kubernetes](#create_cos_secret).
   3. Vaya al directorio `templates` y obtenga una lista de los archivos disponibles.  
      ```
      cd ibm-object-storage-plugin/templates && ls
      ```
      {: pre}

   4. Abra el archivo `provisioner-sa.yaml` y busque la definición de `ClusterRole` `ibmcloud-object-storage-secret-reader`.
   6. Añada el nombre del secreto que ha creado anteriormente a la lista de secretos a los que el plugin está autorizado para acceder en la sección `resourceNames`.
      ```
      kind: ClusterRole
      apiVersion: rbac.authorization.k8s.io/v1beta1
      metadata:
        name: ibmcloud-object-storage-secret-reader
      rules:
      - apiGroups: [""]
        resources: ["secrets"]
        resourceNames: ["<secret_name1>","<secret_name2>"]
        verbs: ["get"]
      ```
      {: codeblock}
   7. Guarde los cambios.

9. Instale el plugin de {{site.data.keyword.cos_full_notm}}. Cuando instala el plugin, se añaden a su clúster las clases de almacenamiento de almacenamiento predefinidas.

   - **Para OS X y Linux:**
     - Si ha omitido el paso anterior, realice la instalación sin limitación a secretos de Kubernetes específicos.</br>
       **Sin Tiller**: 
       ```
       helm ibmc template iks-charts/ibm-object-storage-plugin --apply
       ```
       {: pre}
       
       **Con Tiller**: 
       ```
       helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
       {: pre}

     - Si ha completado el paso anterior, instale con una limitación a secretos de Kubernetes específicos.</br>
       **Sin Tiller**: 
       ```
       cd ../..
       helm ibmc template ./ibm-object-storage-plugin --apply 
       ```
       {: pre}
       
       **Con Tiller**: 
       ```
       cd ../..
       helm ibmc install ./ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
       {: pre}

   - **Para Windows:**
     1. Recupere la zona en la que se ha desplegado el clúster y almacene la zona en una variable de entorno.
        ```
        export DC_NAME=$(kubectl get cm cluster-info -n kube-system -o jsonpath='{.data.cluster-config\.json}' | grep datacenter | awk -F ': ' '{print $2}' | sed 's/\"//g' |sed 's/,//g')
        ```
        {: pre}

     2. Compruebe que la variable de entorno se haya establecido.
        ```
        printenv
        ```
        {: pre}

     3. Instale el diagrama de Helm.
        - Si ha omitido el paso anterior, realice la instalación sin limitación a secretos de Kubernetes específicos.</br>
          **Sin Tiller**: 
          ```
          helm ibmc template iks-charts/ibm-object-storage-plugin --apply
          ```
          {: pre}
          
          **Con Tiller**: 
          ```
          helm ibmc install iks-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
          ```
          {: pre}

        - Si ha completado el paso anterior, instale con una limitación a secretos de Kubernetes específicos.</br>
          **Sin Tiller**: 
          ```
          cd ../..
          helm ibmc template ./ibm-object-storage-plugin --apply 
          ```
          {: pre}
          
          **Con Tiller**: 
          ```
          cd ../..
          helm ibmc install ./ibm-object-storage-plugin --name ibm-object-storage-plugin
          ```
          {: pre}


   Salida de ejemplo de instalación sin Tiller:
   ```
   Rendering the Helm chart templates...
   DC: dal10
   Chart: iks-charts/ibm-object-storage-plugin
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-cold-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-cold-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-perf-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-perf-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-perf-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-perf-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-vault-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-vault-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/flex-driver-sa.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/provisioner-sa.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/flex-driver.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/tests/check-driver-install.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/provisioner.yaml
   Installing the Helm chart...
   serviceaccount/ibmcloud-object-storage-driver created
   daemonset.apps/ibmcloud-object-storage-driver created
   storageclass.storage.k8s.io/ibmc-s3fs-cold-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-cold-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-perf-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-perf-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-perf-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-perf-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-vault-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-vault-regional created
   serviceaccount/ibmcloud-object-storage-plugin created
   clusterrole.rbac.authorization.k8s.io/ibmcloud-object-storage-plugin created
   clusterrole.rbac.authorization.k8s.io/ibmcloud-object-storage-secret-reader created
   clusterrolebinding.rbac.authorization.k8s.io/ibmcloud-object-storage-plugin created
   clusterrolebinding.rbac.authorization.k8s.io/ibmcloud-object-storage-secret-reader created
   deployment.apps/ibmcloud-object-storage-plugin created
   pod/ibmcloud-object-storage-driver-test created
   ```
   {: screen}

10. Verifique que el plugin está instalado correctamente.
    ```
    kubectl get pod -n kube-system -o wide | grep object
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ibmcloud-object-storage-driver-9n8g8                              1/1       Running   0          2m
   ibmcloud-object-storage-plugin-7c774d484b-pcnnx                   1/1       Running   0          2m
    ```
    {: screen}

    La instalación es satisfactoria si ve un pod `ibmcloud-object-storage-plugin` y uno o varios pods `ibmcloud-object-storage-driver`. El número de pods `ibmcloud-object-storage-driver` corresponde al número de nodos trabajadores de su clúster. Todos los pods deben estar en un estado `En ejecución` para que el plugin funcione correctamente. Si fallan los pods, ejecute `kubectl describe pod -n kube-system <pod_name>` para encontrar la causa raíz de la anomalía.

11. Verifique que las clases de almacenamiento se han creado correctamente.
    ```
    kubectl get storageclass | grep s3
    ```
    {: pre}

    Salida de ejemplo:
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

12. Repita los pasos para todos los clústeres en los que desea acceder a los grupos de {{site.data.keyword.cos_full_notm}}.

### Actualización del plugin de IBM Cloud Object Storage
{: #update_cos_plugin}

Puede actualizar el plugin de {{site.data.keyword.cos_full_notm}} existente a la última versión.
{: shortdesc}

1. Si ha instalado previamente la versión 1.0.4 o anterior del diagrama de Helm denominado `ibmcloud-object-storage-plugin`, elimine esta instalación de Helm de su clúster. A continuación, vuelva a instalar el diagrama de Helm.
   1. Compruebe si la versión anterior del diagrama de Helm de {{site.data.keyword.cos_full_notm}} está instalada en el clúster.  
      ```
      helm ls | grep ibmcloud-object-storage-plugin
      ```
      {: pre}

      Salida de ejemplo:
      ```
      ibmcloud-object-storage-plugin	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.4	default
      ```
      {: screen}

   2. Si tiene la versión 1.0.4 o anterior del diagrama de Helm denominado `ibmcloud-object-storage-plugin`, elimine el diagrama de Helm del clúster. Si tiene la versión 1.0.5 o posterior del diagrama de Helm denominado `ibm-object-storage-plugin`, continúe en el paso 2.
      ```
      helm delete --purge ibmcloud-object-storage-plugin
      ```
      {: pre}

   3. Siga los pasos del apartado sobre [Instalación del plugin de {{site.data.keyword.cos_full_notm}}](#install_cos) para instalar la versión más reciente plugin de {{site.data.keyword.cos_full_notm}}.

2. Actualice el repositorio de Helm de {{site.data.keyword.Bluemix_notm}} para recuperar la versión más reciente de todos los diagramas de Helm de este repositorio.
   ```
   helm repo update
   ```
   {: pre}

3. Si utiliza una distribución de OS X o de Linux, actualice el plugin de Helm de {{site.data.keyword.cos_full_notm}} `ibmc` a la versión más reciente.
   ```
   helm ibmc --update
   ```
   {: pre}

4. Descargue el último diagrama de Helm de {{site.data.keyword.cos_full_notm}} en la máquina local y extraiga el paquete para revisar el archivo `release.md` a fin de encontrar la información de release más reciente.
   ```
   helm fetch --untar iks-charts/ibm-object-storage-plugin
   ```
   {: pre}

5. Actualice el plugin. </br>
   **Sin Tiller**: 
   ```
   helm ibmc template iks-charts/ibm-object-storage-plugin --update
   ```
   {: pre}
     
   **Con Tiller**: 
   1. Busque el nombre de la instalación de su diagrama de Helm.
      ```
      helm ls | grep ibm-object-storage-plugin
      ```
      {: pre}

      Salida de ejemplo:
      ```
      <helm_chart_name> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibm-object-storage-plugin-1.0.5	default
      ```
      {: screen}

   2. Actualice el diagrama de Helm de {{site.data.keyword.cos_full_notm}} a la última versión.
      ```   
      helm ibmc upgrade <helm_chart_name> iks-charts/ibm-object-storage-plugin --force --recreate-pods -f
      ```
      {: pre}

6. Verifique que `ibmcloud-object-storage-plugin` se haya actualizado correctamente.  
   ```
   kubectl rollout status deployment/ibmcloud-object-storage-plugin -n kube-system
   ```
   {: pre}

   La actualización del plugin se realiza correctamente cuando se ve `deployment "ibmcloud-object-storage-plugin" successfully rolled out` en la salida de la CLI.

7. Verifique que `ibmcloud-object-storage-driver` se haya actualizado correctamente.
   ```
   kubectl rollout status ds/ibmcloud-object-storage-driver -n kube-system
   ```
   {: pre}

   La actualización se realiza correctamente cuando se ve `daemon set "ibmcloud-object-storage-driver" successfully rolled out` en la salida de la CLI.

8. Verifique que los pods de {{site.data.keyword.cos_full_notm}} se encuentran en un estado `En ejecución`.
   ```
   kubectl get pods -n kube-system -o wide | grep object-storage
   ```
   {: pre}


### Eliminación del plugin de IBM Cloud Object Storage
{: #remove_cos_plugin}

Si no desea suministrar y utilizar {{site.data.keyword.cos_full_notm}} en el clúster, puede desinstalar el plugin.
{: shortdesc}

Al eliminar el plugin no elimina los datos, las PVC o los PV. Cuando se elimina el plugin, se eliminan del clúster todos los conjuntos de daemons y pods relacionados. No puede suministrar un nuevo {{site.data.keyword.cos_full_notm}} para el clúster ni utilizar las PVC y los PV existentes después de eliminar el plugin, a menos que configure la app para que utilice la API de {{site.data.keyword.cos_full_notm}} directamente.
{: important}

Antes de empezar:

- [Defina su clúster como destino de la CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- Asegúrese de que no haya PVC ni PV en el clúster que utiliza {{site.data.keyword.cos_full_notm}}. Para obtener una lista de todos los pods que montan una PVC específica, ejecute `kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"`.

Para eliminar el plugin:

1. Elimine el plugin del clúster. </br>
   **Con Tiller**: 
   1. Busque el nombre de la instalación de su diagrama de Helm.
      ```
      helm ls | grep object-storage-plugin
      ```
      {: pre}

      Salida de ejemplo:
      ```
      <helm_chart_name> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.0	default
      ```
      {: screen}

   2. Suprima el plugin de {{site.data.keyword.cos_full_notm}} eliminando el diagrama de Helm.
      ```
      helm delete --purge <helm_chart_name>
      ```
      {: pre}

   **Sin Tiller**: 
   ```
   helm ibmc template iks-charts/ibm-object-storage-plugin --delete
   ```
   {: pre}

2. Verifique que los pods de {{site.data.keyword.cos_full_notm}} se han eliminado.
   ```
   kubectl get pod -n kube-system | grep object-storage
   ```
   {: pre}

   La eliminación de los pods es satisfactoria si dejan de aparecer en la salida de la CLI.

3. Verifique que las clases de almacenamiento se han eliminado.
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

   La eliminación de las clases de almacenamiento es satisfactoria si dejan de aparecer en la salida de la CLI.

4. Si utiliza una distribución OS X o Linux, elimine el plugin de Helm `ibmc`. Si utiliza Windows, este paso no es necesario.
   1. Elimine el plugin `ibmc`.
      ```
      rm -rf ~/.helm/plugins/helm-ibmc
      ```
      {: pre}

   2. Verifique que se ha eliminado el plugin `ibmc`.
      ```
      helm plugin list
      ```
      {: pre}

      Salida de ejemplo:
     ```
     NAME	VERSION	DESCRIPTION
     ```
     {: screen}

     El plugin `ibmc` se elimina correctamente si el plugin `ibmc` no aparece en la lista de la salida de la CLI.


## Cómo decidir la configuración del almacenamiento de objetos
{: #configure_cos}

{{site.data.keyword.containerlong_notm}} proporciona clases de almacenamiento predefinidas que puede utilizar para crear grupos con una configuración específica.
{: shortdesc}

1. Obtenga una lista de las clases de almacenamiento disponibles en {{site.data.keyword.containerlong_notm}}.
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

   Salida de ejemplo:
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

2. Elija una clase de almacenamiento que se ajuste a sus requisitos de acceso a datos. La clase de almacenamiento determina las [tarifas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) de la capacidad de almacenamiento, las operaciones de lectura y escritura y el ancho de banda de un grupo. La opción adecuada para usted depende de la frecuencia con la que se lean y se escriban datos en la instancia de servicio.
   - **Estándar**: esta opción se utiliza para datos calientes a los que se accede con frecuencia. Las apps web o móviles son ejemplos habituales de este caso.
   - **Caja fuerte**: esta opción se utiliza para cargas de trabajo o datos fríos a los que se accede con poca frecuencia, como una vez al mes o menos. El archivado, la retención de datos a corto plazo, la conservación de activos digitales, la sustitución de cinta y la recuperación tras desastre son ejemplos habituales de este caso.
   - **Frío**: esta opción se utiliza para datos fríos a los que se accede con muy poca frecuencia (cada 90 días o menos), o datos inactivos. Los archivados, las copias de seguridad a largo plazo, los datos históricos que se conservan por motivos de conformidad o las cargas de trabajo y apps a las que se accede con muy poca frecuencia son ejemplos habituales de este caso.
   - **Flexible**: esta opción se utiliza para las cargas de trabajo y los datos que no siguen un patrón de uso específico o que son demasiado grandes para determinar o prever un patrón de uso. **Sugerencia:** consulte este [blog ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/bluemix/2017/03/interconnect-2017-changing-rules-storage/) para saber cómo funciona la clase de almacenamiento flexible en comparación con los niveles de almacenamiento tradicionales.   

3. Decida el nivel de resiliencia de los datos que se almacenan en el grupo.
   - **Entre regiones**: con esta opción, los datos se almacenan en tres regiones dentro de una geolocalización para obtener la mayor disponibilidad. Si tiene cargas de trabajo que se distribuyen entre regiones, las solicitudes se direccionan al punto final regional más próximo. El punto final de API de la geolocalización lo establece automáticamente el plugin de Helm `ibmc` que ha instalado anteriormente en función de la ubicación en la que se encuentra el clúster. Por ejemplo, si el clúster está en `EE. UU. sur`, las clases de almacenamiento se configuran para utilizar el punto final de API `GEO EE. UU.` para los grupos. Consulte [Regiones y puntos finales](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints) para obtener más información.  
   - **Regional**: con esta opción, los datos se replican en varias zonas dentro de una región. Si tiene cargas de trabajo que se encuentran en la misma región, observará una menor latencia y un mejor rendimiento que en una configuración entre regiones. El punto final regional lo establece automáticamente el plugin de Helm `ibm` que ha instalado anteriormente en función de la ubicación en la que se encuentra el clúster. Por ejemplo, si el clúster está en `EE. UU. sur`, las clases de almacenamiento se configuran para utilizar `EE. UU. sur` como punto final regional para los grupos. Consulte [Regiones y puntos finales](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints) para obtener más información.

4. Revise la configuración de grupo de {{site.data.keyword.cos_full_notm}} detallada de una clase de almacenamiento.
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   Salida de ejemplo:
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
   <caption>Detalles de la clase de almacenamiento</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
   </thead>
   <tbody>
   <tr>
   <td><code>ibm.io/chunk-size-mb</code></td>
   <td>El tamaño de un fragmento de datos que se lee o en el que se escribe en {{site.data.keyword.cos_full_notm}} en megabytes. Las clases de almacenamiento con <code>perf</code> en el nombre se configuran con 52 megabytes. Las clases de almacenamiento sin <code>perf</code> en el nombre utilizan fragmentos de 16 megabytes. Por ejemplo, si desea leer un archivo que es de 1 GB, el plugin lee este archivo en varios fragmentos de 16 o 52 megabytes. </td>
   </tr>
   <tr>
   <td><code>ibm.io/curl-debug</code></td>
   <td>Habilita el registro de solicitudes que se envían a la instancia de servicio de {{site.data.keyword.cos_full_notm}}. Si está habilitado, los registros se envían a `syslog` y puede [reenviar los registros a un servidor de registro externo](/docs/containers?topic=containers-health#logging). De forma predeterminada, todas las clases de almacenamiento se establecen en <strong>false</strong> para inhabilitar esta característica de registro. </td>
   </tr>
   <tr>
   <td><code>ibm.io/debug-level</code></td>
   <td>El nivel de registro establecido por el plugin de {{site.data.keyword.cos_full_notm}}. Todas las clases de almacenamiento se configuran con el nivel de registro <strong>WARN</strong>. </td>
   </tr>
   <tr>
   <td><code>ibm.io/iam-endpoint</code></td>
   <td>Punto final de API para {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). </td>
   </tr>
   <tr>
   <td><code>ibm.io/kernel-cache</code></td>
   <td>Habilita o inhabilita la memoria caché del almacenamiento intermedio del kernel para el punto de montaje del volumen. Si está habilitado, los datos que se leen de {{site.data.keyword.cos_full_notm}} se almacenan en la memoria caché del kernel para garantizar el acceso de lectura rápido a los datos. Si está inhabilitado, los datos no se almacenan en la memoria caché y siempre se leen desde {{site.data.keyword.cos_full_notm}}. La memoria caché de kernel está habilitada para las clases de almacenamiento <code>estándar</code> y <code>flexible</code>, e inhabilitada para las clases de almacenamiento <code>frío</code> y <code>caja fuerte</code>. </td>
   </tr>
   <tr>
   <td><code>ibm.io/multireq-max</code></td>
   <td>El número máximo de solicitudes paralelas que se pueden enviar a la instancia de servicio de {{site.data.keyword.cos_full_notm}} para obtener una lista de los archivos en un único directorio. Todas las clases de almacenamiento se configuran con un máximo de 20 solicitudes paralelas.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-endpoint</code></td>
   <td>El punto final de la API que se debe utilizar para acceder al grupo en la instancia de servicio de {{site.data.keyword.cos_full_notm}}. El punto final se establece automáticamente en función de la región del clúster. **Nota**: si desea acceder a un grupo existente que se encuentra en una región distinta a la del clúster, debe crear una [clase de almacenamiento personalizada](/docs/containers?topic=containers-kube_concepts#customized_storageclass) y utilizar el punto final de la API para el grupo.</td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-storage-class</code></td>
   <td>El nombre de la clase de almacenamiento. </td>
   </tr>
   <tr>
   <td><code>ibm.io/parallel-count</code></td>
   <td>El número máximo de solicitudes paralelas que se pueden enviar a la instancia de servicio de {{site.data.keyword.cos_full_notm}} para una sola operación de lectura o escritura. Las clases de almacenamiento con <code>perf</code> en el nombre se configuran con un máximo de 20 solicitudes paralelas. Las clases de almacenamiento sin <code>perf</code> se configuran con 2 solicitudes paralelas de forma predeterminada.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/s3fs-fuse-retry-count</code></td>
   <td>El número máximo de reintentos para una operación de lectura o escritura antes de que se considere que la operación no es satisfactoria. Todas las clases de almacenamiento se configuran con un máximo de 5 reintentos.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/stat-cache-size</code></td>
   <td>El número máximo de registros que se conservan en la memoria caché de metadatos de {{site.data.keyword.cos_full_notm}}. Cada registro puede ocupar un máximo de 0,5 kilobytes. Todas las clases de almacenamiento establecen el número máximo de registros en 100000 de forma predeterminada. </td>
   </tr>
   <tr>
   <td><code>ibm.io/tls-cipher-suite</code></td>
   <td>La suite de cifrado TLS que se debe utilizar cuando se establece una conexión a {{site.data.keyword.cos_full_notm}} mediante el punto final de HTTPS. El valor de la suite de cifrado debe seguir el [formato OpenSSL ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html). Todas las clases de almacenamiento utilizan la suite de cifrado <strong><code>AESGCM</code></strong> de forma predeterminada.  </td>
   </tr>
   </tbody>
   </table>

   Para obtener más información sobre cada clase de almacenamiento, consulte la [referencia de clases de almacenamiento](#cos_storageclass_reference). Si desea cambiar alguno de los valores preestablecidos, cree su propia [clase de almacenamiento personalizada](/docs/containers?topic=containers-kube_concepts#customized_storageclass).
   {: tip}

5. Decida un nombre para el grupo. El nombre del grupo debe ser exclusivo en {{site.data.keyword.cos_full_notm}}. También puede optar por crear automáticamente un nombre para el grupo mediante el plugin de {{site.data.keyword.cos_full_notm}}. Para organizar los datos en un grupo, puede crear subdirectorios.

   La clase de almacenamiento que ha seleccionado anteriormente determina las tarifas de todo el grupo. No se pueden definir distintas clases de almacenamiento para los subdirectorios. Si desea almacenar datos con diferentes requisitos de acceso, considere la posibilidad de crear varios grupos mediante varias PVC.
   {: note}

6. Decida si desea conservar los datos y el grupo después de que se suprima el clúster o la reclamación de volumen persistente (PVC). Cuando se suprime la PVC, siempre se suprime el PV. Puede elegir si desea también suprimir automáticamente los datos y el grupo cuando suprime la PVC. La instancia de servicio de {{site.data.keyword.cos_full_notm}} es independiente de la política de retención que selecciona para los datos y nunca se elimina cuando suprime una PVC.

Ahora que ha decidido la configuración que desea, está preparado para [crear una PVC](#add_cos) para suministrar {{site.data.keyword.cos_full_notm}}.

## Adición de almacenamiento de objetos a apps
{: #add_cos}

Cree una reclamación de volumen persistente (PVC) para suministrar {{site.data.keyword.cos_full_notm}} al clúster.
{: shortdesc}

En función de los valores que elija en la PVC, puede suministrar {{site.data.keyword.cos_full_notm}} de las formas siguientes:
- [Suministro dinámico](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning): al crear la PVC, se crean automáticamente el grupo y el volumen persistente (PV) correspondiente en la instancia de servicio de {{site.data.keyword.cos_full_notm}}.
- [Suministro estático](/docs/containers?topic=containers-kube_concepts#static_provisioning): puede hacer referencia a un grupo existente en la instancia de servicio de {{site.data.keyword.cos_full_notm}} en la PVC. Cuando se crea la PVC, sólo se crea automáticamente el PV correspondiente y se vincula con el grupo existente en {{site.data.keyword.cos_full_notm}}.

Antes de empezar:
- [Cree y prepare la instancia de servicio de {{site.data.keyword.cos_full_notm}}](#create_cos_service).
- [Cree un secreto para almacenar las credenciales de servicio de {{site.data.keyword.cos_full_notm}}](#create_cos_secret).
- [Decida la configuración de {{site.data.keyword.cos_full_notm}}](#configure_cos).

Para añadir {{site.data.keyword.cos_full_notm}} al clúster:

1. Cree un archivo de configuración para definir la reclamación de volumen persistente (PVC).
   ```
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: <pvc_name>
     namespace: <namespace>
     annotations:
       ibm.io/auto-create-bucket: "<true_or_false>"
       ibm.io/auto-delete-bucket: "<true_or_false>"
       ibm.io/bucket: "<bucket_name>"
       ibm.io/object-path: "<bucket_subdirectory>"
       ibm.io/secret-name: "<secret_name>"
       ibm.io/endpoint: "https://<s3fs_service_endpoint>"
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi # Enter a fictitious value
     storageClassName: <storage_class>
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
   <td><code>metadata.namespace</code></td>
   <td>Especifique el espacio de nombres en el que desea crear la PVC. La PVC se debe crear en el mismo espacio de nombres en el que ha creado el secreto de Kubernetes para las credenciales de servicio de {{site.data.keyword.cos_full_notm}} y en el que desea ejecutar el pod. </td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-create-bucket</code></td>
   <td>Seleccione una de las opciones siguientes: <ul><li><strong>true</strong>: al crear la PVC, se crean automáticamente el grupo y el PV en la instancia de servicio de {{site.data.keyword.cos_full_notm}}. Elija esta opción para crear un nuevo grupo en la instancia de servicio de {{site.data.keyword.cos_full_notm}}. </li><li><strong>false</strong>: elija esta opción si desea acceder a los datos de un grupo existente. Cuando se crea la PVC, el PV se crea automáticamente y se enlaza con el grupo que haya especificado en <code>ibm.io/bucket</code>.</td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-delete-bucket</code></td>
   <td>Seleccione una de las opciones siguientes: <ul><li><strong>true</strong>: los datos, el grupo y el PV se eliminan automáticamente cuando se suprime la PVC. La instancia de servicio de {{site.data.keyword.cos_full_notm}} se conserva y no se suprime. Si elige establecer esta opción en <strong>true</strong>, debe establecer <code>ibm.io/auto-create-bucket: true</code> e <code>ibm.io/bucket: ""</code> para que el grupo se cree automáticamente con un nombre con el formato <code>tmp-s3fs-xxxx</code>. </li><li><strong>false</strong>: al suprimir la PVC, el PV se suprime automáticamente, pero se conservan los datos del grupo de la instancia de servicio de {{site.data.keyword.cos_full_notm}}. Para acceder a los datos, debe crear una nueva PVC con el nombre del grupo existente. </li></ul>
   <tr>
   <td><code>ibm.io/bucket</code></td>
   <td>Seleccione una de las opciones siguientes: <ul><li>Si <code>ibm.io/auto-create-bucket</code> se establece en <strong>true</strong>: especifique el nombre del grupo que desea crear en {{site.data.keyword.cos_full_notm}}. Si, además, <code>ibm.io/auto-delete-bucket</code> se establece en <strong>true</strong>, debe dejar este campo en blanco para asignar automáticamente al grupo un nombre con el formato <code>tmp-s3fs-xxxx</code>. El nombre debe ser exclusivo en {{site.data.keyword.cos_full_notm}}. </li><li>Si <code>ibm.io/auto-create-bucket</code> se establece en <strong>false</strong>: especifique el nombre del grupo existente al que desea acceder en el clúster. </li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/object-path</code></td>
   <td>Opcional: especifique el nombre del subdirectorio existente del grupo que desea montar. Utilice esta opción si desea montar únicamente un subdirectorio y no todo el grupo. Para montar un subdirectorio, debe establecer <code>ibm.io/auto-create-bucket: "false"</code> e indicar el nombre del grupo en <code>ibm.io/bucket</code>. </li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/secret-name</code></td>
   <td>Especifique el nombre del secreto que contiene las credenciales de {{site.data.keyword.cos_full_notm}} que ha creado anteriormente. </td>
   </tr>
   <tr>
  <td><code>ibm.io/endpoint</code></td>
  <td>Si ha creado la instancia de servicio de {{site.data.keyword.cos_full_notm}} en una ubicación distinta de la del clúster, especifique el punto final de servicio público o privado de la instancia de servicio de {{site.data.keyword.cos_full_notm}} que desea utilizar. Para obtener una visión general de los puntos finales de servicio disponibles, consulte [Información adicional sobre puntos finales](/docs/services/cloud-object-storage?topic=cloud-object-storage-advanced-endpoints). De forma predeterminada, el plugin de Helm <code>ibmc</code> recupera automáticamente la ubicación del clúster y crea las clases de almacenamiento utilizando el punto final de servicio privado de {{site.data.keyword.cos_full_notm}} que coincide con la ubicación del clúster. Si el clúster se encuentra en una de las zonas metropolitanas, como por ejemplo `dal10`, se utiliza el punto final de servicio privado de {{site.data.keyword.cos_full_notm}} correspondiente al área metropolitana, en este caso Dallas. Para verificar que el punto final de servicio de las clases de almacenamiento coincide con el punto final de servicio de la instancia de servicio, ejecute `kubectl describe storageclass <storageclassname>`. Asegúrese de especificar el punto final de servicio en el formato `https://<s3fs_private_service_endpoint>` para puntos finales de servicio privados o `http://<s3fs_public_service_endpoint>` para puntos finales de servicio público. Si el punto final de servicio de la clase de almacenamiento coincide con el punto final de servicio de la instancia de servicio de {{site.data.keyword.cos_full_notm}}, no incluya la opción <code>ibm.io/endpoint</code> en el archivo YAML de la PVC. </td>
  </tr>
   <tr>
   <td><code>resources.requests.storage</code></td>
   <td>Un tamaño ficticio para el grupo de {{site.data.keyword.cos_full_notm}} en gigabytes. El tamaño es necesario para Kubernetes, pero no se respeta en {{site.data.keyword.cos_full_notm}}. Puede especificar cualquier tamaño que desee. El espacio real que utilice en {{site.data.keyword.cos_full_notm}} puede ser diferente y se factura en función de la [tabla de tarifas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api). </td>
   </tr>
   <tr>
   <td><code>spec.storageClassName</code></td>
   <td>Seleccione una de las opciones siguientes: <ul><li>Si <code>ibm.io/auto-create-bucket</code> se establece en <strong>true</strong>: especifique la clase de almacenamiento que desea utilizar para el nuevo grupo. </li><li>Si <code>ibm.io/auto-create-bucket</code> se establece en <strong>false</strong>: especifique la clase de almacenamiento que ha utilizado para crear el grupo existente. </br></br>Si ha creado manualmente el grupo en la instancia de servicio de {{site.data.keyword.cos_full_notm}} o no recuerda la clase de almacenamiento que ha utilizado, busque la instancia de servicio en el panel de control de {{site.data.keyword.Bluemix}} y compruebe la <strong>Clase</strong> y la <strong>Ubicación</strong> del grupo existente. A continuación, utilice la [clase de almacenamiento](#cos_storageclass_reference) adecuada. <p class="note">El punto final de la API de {{site.data.keyword.cos_full_notm}} que se establece en la clase de almacenamiento se basa en la región en la que se encuentra el clúster. Si desea acceder a un grupo que se encuentra en una región distinta a la del clúster, debe crear una [clase de almacenamiento personalizada](/docs/containers?topic=containers-kube_concepts#customized_storageclass) y utilizar el punto final apropiado de la API para el grupo.</p></li></ul>  </td>
   </tr>
   </tbody>
   </table>

2. Cree la PVC.
   ```
   kubectl apply -f filepath/pvc.yaml
   ```
   {: pre}

3. Verifique que la PVC se ha creado y se ha vinculado al PV.
   ```
   kubectl get pvc
   ```
   {: pre}

   Salida de ejemplo:
   ```
   NAME                  STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS                     AGE
   s3fs-test-pvc         Bound     pvc-b38b30f9-1234-11e8-ad2b-t910456jbe12   8Gi        RWO            ibmc-s3fs-standard-cross-region  1h
   ```
   {: screen}

4. Opcional: si tiene previsto acceder a los datos con un usuario no root, o añadir archivos a un grupo de {{site.data.keyword.cos_full_notm}} existente mediante la consola o la API directamente, asegúrese de que los [archivos tengan el permiso correcto](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_nonroot_access) asignado para que la app pueda leer y actualizar correctamente los archivos según sea necesario.

4.  {: #cos_app_volume_mount}Para montar el PV en el despliegue, cree un archivo `.yaml` de configuración y especifique la PVC que enlaza el PV.

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
            securityContext:
              runAsUser: <non_root_user>
              fsGroup: <non_root_user> #only applicable for clusters that run Kubernetes version 1.13 or later
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
    <td><code>spec.containers.securityContext.runAsUser</code></td>
    <td>Opcional: Para acceder a su app con un usuario no root en un clúster con Kubernetes versión 1.12 o anterior, especifique el [contexto de seguridad ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) para su pod definiendo el usuario no root sin establecer `fsGroup` en el archivo YAML de despliegue al mismo tiempo. Al establecer `fsGroup`, se provoca que el plugin de {{site.data.keyword.cos_full_notm}} actualice los permisos de grupo para todos los archivos de un grupo cuando se despliega el pod. La actualización de los permisos es una operación de escritura y afecta al rendimiento. En función de la cantidad de archivos que tenga, la actualización de los permisos puede impedir que el pod se proporcione y pase al estado <code>En ejecución</code>. </br></br>Si tiene un clúster con Kubernetes versión 1.13 o posterior y el plugin de {{site.data.keyword.Bluemix_notm}} Object Storage versión 1.0.4 o posterior, puede cambiar el propietario del punto de montaje s3fs. Para cambiar el propietario, especifique el contexto de seguridad estableciendo `runAsUser` y `fsGroup` en el mismo ID de usuario no root que desea que sea el propietario del punto de montaje s3fs. Si estos dos valores no coinciden, el punto de montaje automáticamente es propiedad del usuario `root`.  </td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>La vía de acceso absoluta del directorio en el que el que está montado el volumen dentro del contenedor. Si desea compartir un volumen entre distintas apps, puede especificar [sub vías de acceso de volumen ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath) para cada una de las apps.</td>
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

7. Verifique que puede escribir datos en la instancia de servicio de {{site.data.keyword.cos_full_notm}}.
   1. Inicie la sesión en el pod que monta el PV.
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. Vaya a la vía de acceso de montaje de volumen que ha definido en el despliegue de la app.
   3. Cree un archivo de texto.
      ```
      echo "This is a test" > test.txt
      ```
      {: pre}

   4. En el panel de control de {{site.data.keyword.Bluemix}}, vaya a la instancia de servicio de {{site.data.keyword.cos_full_notm}}.
   5. En el menú, seleccione **Grupos**.
   6. Abra el grupo y verifique que puede ver el archivo `test.txt` que ha creado.


## Utilización del almacenamiento de objetos en un conjunto con estado
{: #cos_statefulset}

Si tiene una app con estado como, por ejemplo, una base de datos, puede crear conjuntos con estado que utilicen {{site.data.keyword.cos_full_notm}} para almacenar los datos de la app. Como alternativa, puede utilizar una base de datos como servicio de {{site.data.keyword.Bluemix_notm}}, como por ejemplo {{site.data.keyword.cloudant_short_notm}}, y almacenar los datos en la nube.
{: shortdesc}

Antes de empezar:
- [Cree y prepare la instancia de servicio de {{site.data.keyword.cos_full_notm}}](#create_cos_service).
- [Cree un secreto para almacenar las credenciales de servicio de {{site.data.keyword.cos_full_notm}}](#create_cos_secret).
- [Decida la configuración de {{site.data.keyword.cos_full_notm}}](#configure_cos).

Para desplegar un conjunto con estado que utilice el almacenamiento de objetos:

1. Cree un archivo de configuración para el conjunto con estado y el servicio que utiliza para exponer el conjunto con estado. En los ejemplos siguientes se muestra cómo desplegar NGINX como un conjunto con estado con 3 réplicas, cada una de las cuales utiliza un grupo distinto o todas comparten el mismo grupo.

   **Ejemplo para crear un conjunto con estado con 3 réplicas, cada una de las cuales utiliza un grupo distinto**:
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

   **Ejemplo para crear un conjunto con estado con 3 réplicas que comparten el mismo grupo `mybucket`**:
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
    <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
    <td style="text-align:left">Especifique todas las etiquetas que desee incluir en el conjunto con estado y en la PVC. Kubernetes no reconoce las etiquetas que se incluyen en <code>volumeClaimTemplates</code> del conjunto con estado. Debe definir estas etiquetas en la sección <code>spec.selector.matchLabels</code> y en la sección <code>spec.template.metadata.labels</code> del archivo YAML del conjunto con estado. Para asegurarse de que todas las réplicas del conjunto con estado se incluyen en el equilibrio de carga del servicio, incluya la misma etiqueta que ha utilizado en la sección <code>spec.selector</code> del archivo YAML del servicio. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
    <td style="text-align:left">Especifique las mismas etiquetas que ha añadido a la sección <code>spec.selector.matchLabels</code> del archivo YAML del conjunto con estado. </td>
    </tr>
    <tr>
    <td><code>spec.template.spec.</code></br><code>terminationGracePeriodSeconds</code></td>
    <td>Especifique el número de segundos que se otorga a <code>kubelet</code> para que termine el pod que ejecuta la réplica del conjunto con estado. Para obtener más información, consulte [Suprimir pods ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/run-application/force-delete-stateful-set-pod/#delete-pods). </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>metadata.name</code></td>
    <td style="text-align:left">Especifique un nombre para el volumen. Utilice el mismo nombre que ha definido en la sección <code>spec.containers.volumeMount.name</code>. El nombre que escriba aquí se utiliza para crear el nombre de la PVC con el siguiente formato: <code>&lt;nombre_volumen&gt;-&lt;nombre_conjuntoconestado&gt;-&lt;número_réplica&gt;</code>. </td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-create-bucket</code></td>
    <td>Seleccione una de las opciones siguientes: <ul><li><strong>true: </strong>elija esta opción para crear automáticamente un grupo para cada réplica del conjunto con estado. </li><li><strong>false: </strong>elija esta opción si desea compartir un grupo existente entre las réplicas del conjunto con estado. Asegúrese de definir el nombre del grupo en la sección <code>spec.volumeClaimTemplates.metadata.annotions.ibm.io/bucket</code> del archivo YAML del conjunto con estado.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-delete-bucket</code></td>
    <td>Seleccione una de las opciones siguientes: <ul><li><strong>true: </strong>los datos, el grupo y el PV se eliminan automáticamente cuando se suprime la PVC. La instancia de servicio de {{site.data.keyword.cos_full_notm}} se conserva y no se suprime. Si elige establecer esta opción en true, debe establecer <code>ibm.io/auto-create-bucket: true</code> e <code>ibm.io/bucket: ""</code> para que el grupo se cree automáticamente con un nombre con el formato <code>tmp-s3fs-xxxx</code>. </li><li><strong>false: </strong>al suprimir la PVC, el PV se suprime automáticamente, pero se conservan los datos del grupo de la instancia de servicio de {{site.data.keyword.cos_full_notm}}. Para acceder a los datos, debe crear una nueva PVC con el nombre del grupo existente.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/bucket</code></td>
    <td>Seleccione una de las opciones siguientes: <ul><li><strong>Si <code>ibm.io/auto-create-bucket</code> se establece en true: </strong>especifique el nombre del grupo que desea crear en {{site.data.keyword.cos_full_notm}}. Si, además, <code>ibm.io/auto-delete-bucket</code> se establece en <strong>true</strong>, debe dejar este campo en blanco para asignar automáticamente al grupo un nombre con el formato tmp-s3fs-xxxx. El nombre debe ser exclusivo en {{site.data.keyword.cos_full_notm}}.</li><li><strong>Si <code>ibm.io/auto-create-bucket</code> se establece en false: </strong>especifique el nombre del grupo existente al que desea acceder en el clúster.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/secret-name</code></td>
    <td>Especifique el nombre del secreto que contiene las credenciales de {{site.data.keyword.cos_full_notm}} que ha creado anteriormente.</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.</code></br><code>annotations.volume.beta.</code></br><code>kubernetes.io/storage-class</code></td>
    <td style="text-align:left">Especifique la clase de almacenamiento que desea utilizar. Seleccione una de las opciones siguientes: <ul><li><strong>Si <code>ibm.io/auto-create-bucket</code> se establece en true: </strong>especifique la clase de almacenamiento que desea utilizar para el nuevo grupo.</li><li><strong>Si <code>ibm.io/auto-create-bucket</code> se establece en false: </strong>especifique la clase de almacenamiento que ha utilizado para crear el grupo existente. </li></ul></br>  Para ver una lista de las clases de almacenamiento existentes, ejecute <code>kubectl get storageclasses | grep s3</code>. Si no especifica una clase de almacenamiento, la PVC se crea con la clase de almacenamiento predeterminada establecida en el clúster. Asegúrese de que la clase de almacenamiento predeterminada utiliza el suministrador <code>ibm.io/ibmc-s3fs</code> para que el conjunto con estado se suministre con almacenamiento de objetos.</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
    <td>Especifique la clase de almacenamiento que ha especificado en la sección <code>spec.volumeClaimTemplates.metadata.annotations.volume.beta.kubernetes.io/storage-class</code> del archivo YAML del conjunto con estado.  </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.</code></br><code>resource.requests.storage</code></td>
    <td>Especifique un tamaño ficticio para el grupo de {{site.data.keyword.cos_full_notm}} en gigabytes. El tamaño es necesario para Kubernetes, pero no se respeta en {{site.data.keyword.cos_full_notm}}. Puede especificar cualquier tamaño que desee. El espacio real que utilice en {{site.data.keyword.cos_full_notm}} puede ser diferente y se factura en función de la [tabla de tarifas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api).</td>
    </tr>
    </tbody></table>


## Copia de seguridad y restauración de datos
{: #cos_backup_restore}

{{site.data.keyword.cos_full_notm}} se configura para proporcionar una alta durabilidad para los datos y evitar que se pierdan. El SLA está disponible en los [términos de servicio de {{site.data.keyword.cos_full_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm-7857-03).
{: shortdesc}

{{site.data.keyword.cos_full_notm}} no proporciona un historial de versiones para los datos. Si necesita conservar y acceder a las versiones anteriores de los datos, debe configurar la app para gestionar el historial de los datos o implementar soluciones alternativas de copia de seguridad. Por ejemplo, puede que desee almacenar los datos de {{site.data.keyword.cos_full_notm}} en la base de datos local o utilizar cintas para archivar los datos.
{: note}

## Referencia de clases de almacenamiento
{: #cos_storageclass_reference}

### Estándar
{: #standard}

<table>
<caption>Clase de almacenamiento de objetos: estándar</caption>
<thead>
<th>Características</th>
<th>Valor</th>
</thead>
<tbody>
<tr>
<td>Nombre</td>
<td><code>ibmc-s3fs-standard-cross-region</code></br><code>ibmc-s3fs-standard-perf-cross-region</code></br><code>ibmc-s3fs-standard-regional</code></br><code>ibmc-s3fs-standard-perf-regional</code></td>
</tr>
<tr>
<td>Punto final de resiliencia predeterminado</td>
<td>El punto final de resiliencia se establece automáticamente en función de la ubicación en la que se encuentra el clúster. Consulte [Regiones y puntos finales](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints) para obtener más información. </td>
</tr>
<tr>
<td>Tamaño de fragmento</td>
<td>Clases de almacenamiento sin `perf`: 16 MB</br>Clases de almacenamiento con `perf`: 52 MB</td>
</tr>
<tr>
<td>Caché de Kernel</td>
<td>Habilitada</td>
</tr>
<tr>
<td>Facturación</td>
<td>Mensual</td>
</tr>
<tr>
<td>Tarifas</td>
<td>[Tarifas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Caja fuerte
{: #Vault}

<table>
<caption>Clase de almacenamiento de objetos: caja fuerte</caption>
<thead>
<th>Características</th>
<th>Valor</th>
</thead>
<tbody>
<tr>
<td>Nombre</td>
<td><code>ibmc-s3fs-vault-cross-region</code></br><code>ibmc-s3fs-vault-regional</code></td>
</tr>
<tr>
<td>Punto final de resiliencia predeterminado</td>
<td>El punto final de resiliencia se establece automáticamente en función de la ubicación en la que se encuentra el clúster. Consulte [Regiones y puntos finales](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints) para obtener más información. </td>
</tr>
<tr>
<td>Tamaño de fragmento</td>
<td>16 MB</td>
</tr>
<tr>
<td>Caché de Kernel</td>
<td>Inhabilitada</td>
</tr>
<tr>
<td>Facturación</td>
<td>Mensual</td>
</tr>
<tr>
<td>Tarifas</td>
<td>[Tarifas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Frío
{: #cold}

<table>
<caption>Clase de almacenamiento de objetos: frío</caption>
<thead>
<th>Características</th>
<th>Valor</th>
</thead>
<tbody>
<tr>
<td>Nombre</td>
<td><code>ibmc-s3fs-flex-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-flex-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>Punto final de resiliencia predeterminado</td>
<td>El punto final de resiliencia se establece automáticamente en función de la ubicación en la que se encuentra el clúster. Consulte [Regiones y puntos finales](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints) para obtener más información. </td>
</tr>
<tr>
<td>Tamaño de fragmento</td>
<td>16 MB</td>
</tr>
<tr>
<td>Caché de Kernel</td>
<td>Inhabilitada</td>
</tr>
<tr>
<td>Facturación</td>
<td>Mensual</td>
</tr>
<tr>
<td>Tarifas</td>
<td>[Tarifas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Flexible
{: #flex}

<table>
<caption>Clase de almacenamiento de objetos: flexible</caption>
<thead>
<th>Características</th>
<th>Valor</th>
</thead>
<tbody>
<tr>
<td>Nombre</td>
<td><code>ibmc-s3fs-cold-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-cold-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>Punto final de resiliencia predeterminado</td>
<td>El punto final de resiliencia se establece automáticamente en función de la ubicación en la que se encuentra el clúster. Consulte [Regiones y puntos finales](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints) para obtener más información. </td>
</tr>
<tr>
<td>Tamaño de fragmento</td>
<td>Clases de almacenamiento sin `perf`: 16 MB</br>Clases de almacenamiento con `perf`: 52 MB</td>
</tr>
<tr>
<td>Caché de Kernel</td>
<td>Habilitada</td>
</tr>
<tr>
<td>Facturación</td>
<td>Mensual</td>
</tr>
<tr>
<td>Tarifas</td>
<td>[Tarifas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>
