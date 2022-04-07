---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, ibmcloud, ic, ks, kubectl

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



# Configuración de la CLI y la API
{: #cs_cli_install}

Puede utilizar la CLI o la API de {{site.data.keyword.containerlong}} para crear y gestionar sus clústeres de Kubernetes.
{:shortdesc}

## Instalación de la CLI y de los plugins de IBM Cloud
{: #cs_cli_install_steps}

Instale las CLI necesarias para crear y gestionar sus clústeres de Kubernetes en {{site.data.keyword.containerlong_notm}} y para desplegar apps contenerizadas en el clúster.
{:shortdesc}

Esta tarea incluye la información para instalar estas CLI y plugins:

-   CLI de {{site.data.keyword.Bluemix_notm}}
-   Plugin de {{site.data.keyword.containerlong_notm}}
-   Plugin de {{site.data.keyword.registryshort_notm}}

Si en su lugar desea utilizar la consola de {{site.data.keyword.Bluemix_notm}} después de crear el clúster, puede ejecutar mandatos de CLI directamente desde el navegador web en [Kubernetes Terminal](#cli_web).
{: tip}

<br>
Para instalar las CLI:

1.  Instale la CLI de [{{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli?topic=cloud-cli-getting-started#idt-prereq). Esta instalación incluye:
    -   La CLI base de {{site.data.keyword.Bluemix_notm}} (`ibmcloud`).
    -   El plugin {{site.data.keyword.containerlong_notm}} (`ibmcloud ks`).
    -   El plugin {{site.data.keyword.registryshort_notm}} (`ibmcloud cr`). Utilice este plugin para configurar su propio espacio de nombres en un registro privado de imágenes multiarrendatario, de alta disponibilidad y escalable alojado por IBM, y para almacenar y compartir imágenes de Docker con otros usuarios. Las imágenes de Docker son necesarias para desplegar contenedores en un clúster.
    -   La CLI de Kubernetes (`kubectl`) que coincida con la versión predeterminada: 1.13.6.<p class="note">Si tiene previsto utilizar un clúster que ejecute otra versión, es posible que tenga que [instalar por separado la versión de la CLI de Kubernetes](#kubectl). Si tiene un clúster (OpenShift), [instale las CLI `oc` y `kubectl` juntas](#cli_oc).</p>
    -   La CLI de Helm (`helm`). Puede utilizar Helm como gestor de paquetes para instalar los servicios de {{site.data.keyword.Bluemix_notm}} y las apps complejas en el clúster mediante los diagramas de Helm. Tiene que [configurar Helm](/docs/containers?topic=containers-helm) en cada clúster en el que desee utilizar Helm.

    ¿Tiene pensado utilizar mucho la CLI? Pruebe [Habilitación del rellenado automático de shell para la CLI de {{site.data.keyword.Bluemix_notm}} (solo Linux/MacOS)](/docs/cli/reference/ibmcloud?topic=cloud-cli-shell-autocomplete#shell-autocomplete-linux).
    {: tip}

2.  Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Escriba
sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite.
    ```
    ibmcloud login
    ```
    {: pre}

    Si tiene un ID federado, utilice `ibmcloud login --sso` para iniciar la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe tiene un ID federado cuando el inicio de sesión falla sin el `--sso` y se lleva a cabo correctamente con la opción `--sso`.
    {: tip}

3.  Verifique que el plugin de {{site.data.keyword.containerlong_notm}} y los plugins de {{site.data.keyword.registryshort_notm}} están instalados correctamente.
    ```
    ibmcloud plugin list
    ```
    {: pre}

    Salida de ejemplo:
    ```
    Listing installed plugins...

    Plugin Name                            Version   Status        
    container-registry                     0.1.373     
    container-service/kubernetes-service   0.3.23   
    ```
    {: screen}

Para obtener información acerca de estas CLI, consulte la documentación de dichas herramientas.

-   [Mandatos `ibmcloud`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
-   [Mandatos `ibmcloud ks`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)
-   [Mandatos `ibmcloud cr`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli)

## Instalación de la CLI de Kubernetes (`kubectl`)
{: #kubectl}

Para ver una versión local del panel de control de Kubernetes y para desplegar apps en los clústeres, instale la CLI de Kubernetes (`kubectl`). Se instala la última versión estable de `kubectl` con la CLI de {{site.data.keyword.Bluemix_notm}} base. Sin embargo, para trabajar con su clúster, en su lugar debe instalar la versión `major.minor` de la CLI de Kubernetes que coincida con la versión `major.minor` del clúster de Kubernetes que piensa utilizar. Si utiliza una versión de CLI `kubectl` que no coincide al menos con la versión `major.minor` de los clústeres, puede experimentar resultados inesperados. Por ejemplo, [Kubernetes no da soporte a las ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/setup/version-skew-policy/) versiones de cliente de `kubectl` que difieran en 2 o más versiones con respecto a la versión del servidor (n +/- 2). Mantenga actualizadas las versiones de la CLI y el clúster de Kubernetes.
{: shortdesc}

¿Utiliza un clúster de OpenShift? Instale en su lugar la CLI de OpenShift Origin (`oc`), que viene con `kubectl`. Si tiene clústeres tanto de Red Hat OpenShift on IBM Cloud como de {{site.data.keyword.containershort_notm}} nativos de Ubuntu, asegúrese de utilizar el archivo binario `kubectl` que coincida con la versión de Kubernetes `principal.menor` de su clúster.
{: tip}

1.  Si ya tiene un clúster, compruebe que la versión de la CLI de `kubectl` del cliente coincida con la versión del servidor de API del clúster.
    1.  [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
    2.  Compare las versiones del cliente y del servidor. Si el cliente no coincide con el servidor, continúe en el paso siguiente. Si las versiones coinciden, ya ha instalado la versión adecuada de `kubectl`.
        ```
        kubectl version  --short
        ```
        {: pre}
2.  Descargue la versión `major.minor` de la CLI de Kubernetes que coincida con la versión `major.minor` del clúster de Kubernetes que piensa utilizar. La versión de Kubernetes predeterminada actual de {{site.data.keyword.containerlong_notm}} es la 1.13.6.
    -   **OS X**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl)
    -   **Linux**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl)
    -   **Windows**: Instale la CLI de Kubernetes en el mismo directorio que la CLI de {{site.data.keyword.Bluemix_notm}}. Esta configuración le ahorra algunos cambios en la vía de acceso a archivo cuando ejecute mandatos posteriormente. [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe)

3.  Si utiliza OS X o Linux, siga estos pasos.
    1.  Mueva el archivo ejecutable al directorio `/usr/local/bin`.
        ```
        mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  Asegúrese de que `/usr/local/bin` aparezca en la variable del sistema `PATH`. La variable `PATH` contiene todos los directorios en los que el sistema operativo puede encontrar archivos ejecutables. Los directorios que se muestran en la variable `PATH` sirven para distintos propósitos. `/usr/local/bin` se utiliza para guardar archivos ejecutables correspondientes a software que no forma parte del sistema operativo y que ha instalado manualmente el administrador del sistema.
        ```
        echo $PATH
        ```
        {: pre}
        Ejemplo de salida de CLI:
        ```
        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
        ```
        {: screen}

    3.  Convierta el archivo en ejecutable.
        ```
        chmod +x /usr/local/bin/kubectl
        ```
        {: pre}
4.  **Opcional**: [habilite la terminación automática para los mandatos `kubectl` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion). Los pasos varían en función del shell que utilice.

A continuación, empiece a [crear clústeres de Kubernetes desde la CLI con {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters#clusters_cli_steps).

Para obtener más información sobre la CLI de Kubernetes, consulte la documentación de referencia de [`kubectl` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubectl.docs.kubernetes.io/).
{: note}

<br />


## Instalación de la versión beta de vista previa de la CLI de OpenShift Origin (`oc`)
{: #cli_oc}

[Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial) está disponible como versión beta para probar clústeres de OpenShift.
{: preview}

Para ver una versión local del panel de control de OpenShift y para desplegar apps en los clústeres de Red Hat OpenShift on IBM Cloud, instale la CLI de OpenShift Origin (`oc`). La CLI de `oc` incluye una versión coincidente de la CLI de Kubernetes (`kubectl`). Para obtener más información, consulte la [documentación de OpenShift ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.openshift.com/container-platform/3.11/cli_reference/get_started_cli.html).
{: shortdesc}

¿Utiliza clústeres tanto de Red Hat OpenShift on IBM Cloud como de {{site.data.keyword.containershort_notm}} nativos de Ubuntu? La CLI de `oc` se proporciona con los binarios `oc` y `kubectl`, pero los distintos clústeres pueden ejecutar distintas versiones de Kubernetes, como por ejemplo 1.11 en OpenShift y 1.13.6 en Ubuntu. Asegúrese de utilizar el binario de `kubectl` que se ajuste a la versión de Kubernetes `principal.menor` de su clúster.
{: note}

1.  [Descargue la CLI de OpenShift Origin ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.okd.io/download.html) para el sistema operativo local y la versión OpenShift. La versión de OpenShift predeterminada actual es la 3.11.

2.  Si utiliza Mac OS o Linux, siga los pasos siguientes para añadir los binarios a la variable del sistema `PATH`. Si utiliza Windows, instale la CLI de `oc` en el mismo directorio que la CLI de {{site.data.keyword.Bluemix_notm}}. Esta configuración le ahorra algunos cambios en la vía de acceso a archivo cuando ejecute mandatos posteriormente.
    1.  Mueva los archivos ejecutables `oc` y `kubectl` al directorio `/usr/local/bin`.
        ```
        mv /<filepath>/oc /usr/local/bin/oc && mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  Asegúrese de que `/usr/local/bin` aparezca en la variable del sistema `PATH`. La variable `PATH` contiene todos los directorios en los que el sistema operativo puede encontrar archivos ejecutables. Los directorios que se muestran en la variable `PATH` sirven para distintos propósitos. `/usr/local/bin` se utiliza para guardar archivos ejecutables correspondientes a software que no forma parte del sistema operativo y que ha instalado manualmente el administrador del sistema.
        ```
        echo $PATH
        ```
        {: pre}
        Ejemplo de salida de CLI:
        ```
        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
        ```
        {: screen}
3.  **Opcional**: [habilite la terminación automática para los mandatos `kubectl` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion). Los pasos varían en función del shell que utilice. Puede repetir los pasos para habilitar la terminación automática para los mandatos `oc`. Por ejemplo, en bash en Linux, en lugar de `kubectl completion bash >/etc/bash_completion.d/kubectl`, puede ejecutar `oc completion bash >/etc/bash_completion.d/oc_completion`.

A continuación, vaya a [Creación de un clúster de Red Hat OpenShift on IBM Cloud (vista previa)](/docs/containers?topic=containers-openshift_tutorial).

Para obtener más información sobre la CLI de OpenShift Origin, consulte la documentación de los mandatos [`oc` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.openshift.com/container-platform/3.11/cli_reference/basic_cli_operations.html).
{: note}

<br />


## Ejecución de la CLI en un contenedor en su sistema
{: #cs_cli_container}

En lugar de instalar cada CLI individualmente en el sistema, puede instalar las CLI en un contenedor que se ejecute en el sistema.
{:shortdesc}

Antes de empezar, [instale Docker ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.docker.com/community-edition#/download) para crear y ejecutar imágenes localmente. Si está utilizando Windows 8 o anterior, puede instalar [Docker Toolbox ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.docker.com/toolbox/toolbox_install_windows/) en su lugar.

1. Cree una imagen del Dockerfile proporcionado.

    ```
    docker build -t <image_name> https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/install-clis-container/Dockerfile
    ```
    {: pre}

2. Despliegue la imagen localmente como un contenedor y monte un volumen para acceder a los archivos locales.

    ```
    docker run -it -v /local/path:/container/volume <image_name>
    ```
    {: pre}

3. Empiece ejecutando los mandatos `ibmcloud ks` y `kubectl` desde el shell interactivo. Si crea datos que desee guardar, guarde los datos en el volumen que desea montar. Cuando salga del shell, el contenedor se detiene.

<br />



## Configuración de la CLI para que ejecute mandatos `kubectl`
{: #cs_cli_configure}

Puede utilizar los mandatos que se proporcionan con la CLI de Kubernetes para gestionar clústeres en {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

Todos los mandatos `kubectl` disponibles en Kubernetes 1.13.6 se pueden utilizar con clústeres en {{site.data.keyword.Bluemix_notm}}. Después de crear un clúster, establezca el contexto de la CLI local para dicho clúster con una variable de entorno. A continuación, puede ejecutar el mandato de Kubernetes `kubectl` para trabajar con el clúster en {{site.data.keyword.Bluemix_notm}}.

Para poder ejecutar mandatos `kubectl`:
* [Instale las CLI necesarias](#cs_cli_install).
* [Cree un clúster](/docs/containers?topic=containers-clusters#clusters_cli_steps).
* Asegúrese de que tiene un [rol de servicio](/docs/containers?topic=containers-users#platform) que otorgue el rol de RBAC de Kubernetes adecuado para que pueda trabajar con los recursos de Kubernetes. Si solo tiene un rol de servicio pero no tiene un rol de plataforma, necesita que el administrador del clúster le dé el nombre y el ID del clúster o el rol de plataforma de **Visor** para ver una lista de clústeres.

Para utilizar mandatos `kubectl`:

1.  Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Escriba
sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite.

    ```
    ibmcloud login
    ```
    {: pre}

    Si tiene un ID federado, utilice `ibmcloud login --sso` para iniciar la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe tiene un ID federado cuando el inicio de sesión falla sin el `--sso` y se lleva a cabo correctamente con la opción `--sso`.
    {: tip}

2.  Seleccione una cuenta de {{site.data.keyword.Bluemix_notm}}. Si se le ha asignado a varias organizaciones de {{site.data.keyword.Bluemix_notm}}, seleccione la
organización en la que se ha creado el clúster. Los clústeres son específicos de una
organización, pero son independientes de un espacio de {{site.data.keyword.Bluemix_notm}}. Por lo tanto, no es necesario que seleccione un espacio.

3.  Para crear y trabajar con clústeres en un grupo de recursos que no sea el predeterminado, seleccione como destino dicho grupo de recursos. Para ver el grupo de recursos al que pertenece cada clúster, ejecute `ibmcloud ks clusters`. **Nota**: debe tener [acceso de **Visor**](/docs/containers?topic=containers-users#platform) sobre el grupo de recursos.
    ```
    ibmcloud target -g <resource_group_name>
    ```
    {: pre}

4.  Obtenga una lista de todos los clústeres de la cuenta para obtener el nombre del clúster. Si solo tiene un rol de servicio de {{site.data.keyword.Bluemix_notm}} IAM y no puede ver clústeres, solicite al administrador del clúster el rol de **Visor** de
plataforma de IAM o bien el nombre y el ID del clúster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

5.  Defina el clúster que ha creado como contexto para esta sesión. Siga estos pasos de configuración cada vez que de trabaje con el clúster.
    1.  Obtenga el mandato para establecer la variable de entorno y descargar los archivos de configuración de Kubernetes. <p class="tip">¿Utiliza PowerShell de Windows? Incluya el distintivo `--powershell` para obtener las variables de entorno en formato de PowerShell de Windows.</p>
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Después de descargar los archivos de configuración, se muestra un mandato que puede utilizar para establecer la vía de acceso al archivo de configuración de
Kubernetes local como variable de entorno.

        Ejemplo:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  Copie y pegue el mandato que se muestra en el terminal para definir la variable de entorno `KUBECONFIG`.

        **Usuarios de Mac o Linux**: en lugar de ejecutar el mandato `ibmcloud ks cluster-config` y copiar la variable de entorno `KUBECONFIG`, puede ejecutar `ibmcloud ks cluster-config --export <cluster-name>`. En función de su shell, puede configurar el shell ejecutando `eval $(ibmcloud ks cluster-config --export <cluster-name>)`.
        {: tip}

    3.  Compruebe que la variable de entorno `KUBECONFIG` se haya establecido correctamente.

        Ejemplo:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Salida:
        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

6.  Compruebe que los mandatos `kubectl` se ejecutan correctamente con el clúster comprobando la versión del cliente de la CLI de Kubernetes.

    ```
    kubectl version  --short
    ```
    {: pre}

    Salida de ejemplo:

    ```
    Client Version: v1.13.6
    Server Version: v1.13.6
    ```
    {: screen}

Ahora puede ejecutar mandatos `kubectl` para gestionar sus clústeres en {{site.data.keyword.Bluemix_notm}}. Para ver una lista completa de mandatos, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubectl.docs.kubernetes.io/).

**Sugerencia:** Si utiliza Windows y la CLI de Kubernetes no está instalada en el mismo directorio que la CLI de {{site.data.keyword.Bluemix_notm}}, debe cambiar los directorios por la vía de acceso donde está instalada la CLI de Kubernetes para poder ejecutar correctamente los mandatos `kubectl`.


<br />




## Actualización de la CLI
{: #cs_cli_upgrade}

Quizás desee actualizar las CLI periódicamente para usar nuevas funciones.
{:shortdesc}

Esta tarea incluye la información para actualizar estas CLI.

-   CLI de {{site.data.keyword.Bluemix_notm}} versión 0.8.0 o posterior
-   Plugin de {{site.data.keyword.containerlong_notm}}
-   CLI de Kubernetes versión 1.13.6 o posterior
-   Plugin de {{site.data.keyword.registryshort_notm}}

<br>
Para actualizar las CLI:

1.  Actualice la CLI de {{site.data.keyword.Bluemix_notm}}. Descargue la [versión más reciente ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli?topic=cloud-cli-getting-started) y ejecute el instalador.

2. Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Escriba
sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite.

    ```
    ibmcloud login
    ```
    {: pre}

     Si tiene un ID federado, utilice `ibmcloud login --sso` para iniciar la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe tiene un ID federado cuando el inicio de sesión falla sin el `--sso` y se lleva a cabo correctamente con la opción `--sso`.
     {: tip}

3.  Actualice el plugin de {{site.data.keyword.containerlong_notm}}.
    1.  Instale la actualización desde el repositorio del plugin de {{site.data.keyword.Bluemix_notm}}.

        ```
        ibmcloud plugin update container-service 
        ```
        {: pre}

    2.  Para verificar la instalación del plugin, ejecute el mandato siguiente
y compruebe la lista de plugins instalados.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        El plugin {{site.data.keyword.containerlong_notm}} se visualiza en los resultados como container-service.

    3.  Inicialice la CLI.

        ```
        ibmcloud ks init
        ```
        {: pre}

4.  [Actualice la CLI de Kubernetes](#kubectl).

5.  Actualice el plugin de {{site.data.keyword.registryshort_notm}}.
    1.  Instale la actualización desde el repositorio del plugin de {{site.data.keyword.Bluemix_notm}}.

        ```
        ibmcloud plugin update container-registry 
        ```
        {: pre}

    2.  Para verificar la instalación del plugin, ejecute el mandato siguiente
y compruebe la lista de plugins instalados.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        El plugin de registro se muestra en los resultados como container-registry.

<br />


## Desinstalación de la CLI
{: #cs_cli_uninstall}

Si ya no necesita una CLI, puede desinstalarla.
{:shortdesc}

Esta tarea incluye la información para eliminar estas CLI:


-   Plugin de {{site.data.keyword.containerlong_notm}}
-   CLI de Kubernetes
-   Plugin de {{site.data.keyword.registryshort_notm}}

Para desinstalar las CLI:

1.  Desinstale el plugin de {{site.data.keyword.containerlong_notm}}.

    ```
    ibmcloud plugin uninstall container-service
    ```
    {: pre}

2.  Desinstale el plugin de {{site.data.keyword.registryshort_notm}}.

    ```
    ibmcloud plugin uninstall container-registry
    ```
    {: pre}

3.  Para verificar la desinstalación de los plugins, ejecute el mandato siguiente y compruebe la lista de plugins de instalados.

    ```
    ibmcloud plugin list
    ```
    {: pre}

    El servicio de contenedor y el plugin de servicio de contenedor no se muestran en los resultados.

<br />


## Uso de Kubernetes Terminal en el navegador web (beta)
{: #cli_web}

Kubernetes Terminal le permite utilizar la CLI de {{site.data.keyword.Bluemix_notm}} para gestionar el clúster directamente desde el navegador web.
{: shortdesc}

Kubernetes Terminal se publica como un complemento beta de {{site.data.keyword.containerlong_notm}} y puede sufrir cambios debidos a comentarios o sugerencias de los usuarios y a las pruebas realizadas. No utilice esta característica en clústeres de producción para evitar efectos secundarios inesperados.
{: important}

Si utiliza el panel de control del clúster de la consola de {{site.data.keyword.Bluemix_notm}} para gestionar los clústeres pero desea hacer más cambios de configuración avanzados de forma rápida, ahora puede ejecutar mandatos de CLI directamente desde el navegador web en Kubernetes Terminal. Kubernetes Terminal está habilitado con la [CLI base de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli?topic=cloud-cli-getting-started), el plugin {{site.data.keyword.containerlong_notm}} y el plugin {{site.data.keyword.registryshort_notm}}. Además el contexto del terminal ya está definido en el clúster con el que está trabajando, de forma que puede ejecutar mandatos de Kubernetes `kubectl` para trabajar con el clúster.

Los archivos que descarga y edita localmente, como por ejemplo archivos YAML, se almacenan temporalmente en Kubernetes Terminal y no persisten entre sesiones.
{: note}

Para instalar e iniciar Kubernetes Terminal:

1.  Inicie una sesión en la [consola de {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/).
2.  En la barra de menús, seleccione la cuenta que desea utilizar.
3.  En el menú ![Icono de menú](../icons/icon_hamburger.svg "Icono de menú"), pulse **Kubernetes**.
4.  En la página **Clústeres**, pulse el clúster al que desea acceder.
5.  En la página de detalles del clúster, pulse el botón **Terminal**.
6.  Pulse **Instalar**. Puede que el complemento terminal tarde unos minutos en instalarse.
7.  Vuelva a pulsar el botón **Terminal**. El terminal se abre en el navegador.

La próxima vez, puede iniciar Kubernetes Terminal simplemente pulsando el botón **Terminal**.

<br />


## Automatización de despliegues de clústeres con la API
{: #cs_api}

Puede utilizar la API de {{site.data.keyword.containerlong_notm}} para automatizar la creación, el despliegue y la gestión de clústeres de Kubernetes.
{:shortdesc}

La API de {{site.data.keyword.containerlong_notm}} precisa de información de cabecera que debe proporcionar en su solicitud de la API y que depende del tipo de API utilizado. Para determinar la información de cabecera que se necesita para la API, consulte la [documentación de la API de {{site.data.keyword.containerlong_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://us-south.containers.cloud.ibm.com/swagger-api).

Para autenticarse con {{site.data.keyword.containerlong_notm}}, debe especificar su señal de {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) que se genera con las credenciales de {{site.data.keyword.Bluemix_notm}} y que incluye el ID de la cuenta de {{site.data.keyword.Bluemix_notm}} en la que se ha creado el clúster. Dependiendo de la forma en que se autentique con {{site.data.keyword.Bluemix_notm}}, puede elegir entre las siguientes opciones para automatizar la creación de la señal de {{site.data.keyword.Bluemix_notm}} IAM.

También puede utilizar el [archivo JSON swagger de API ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://containers.cloud.ibm.com/global/swagger-global-api/swagger.json) para generar un cliente que pueda interactuar con la API como parte del trabajo de automatización.
{: tip}

<table summary="Tipos de ID y opciones con el parámetro de entrada en la columna 1 y el valor en la columna 2.">
<caption>Opciones y tipos de ID</caption>
<thead>
<th>ID de {{site.data.keyword.Bluemix_notm}}</th>
<th>Mis opciones</th>
</thead>
<tbody>
<tr>
<td>ID no federado</td>
<td><ul><li><strong>Generar una clave de API de {{site.data.keyword.Bluemix_notm}}:</strong> Como alternativa a utilizar el nombre de usuario y la contraseña de {{site.data.keyword.Bluemix_notm}}, puede <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">utilizar claves de API de {{site.data.keyword.Bluemix_notm}}</a>. Las claves de API de {{site.data.keyword.Bluemix_notm}} dependen de la cuenta de {{site.data.keyword.Bluemix_notm}} para la que se generan. No puede combinar su clave de API de {{site.data.keyword.Bluemix_notm}} con otro ID de cuenta en la misma señal de {{site.data.keyword.Bluemix_notm}} IAM. Para acceder a los clústeres que se han creado con una cuenta distinta de aquella en la que se basa la clave de API de {{site.data.keyword.Bluemix_notm}}, debe iniciar una sesión en la cuenta para generar una nueva clave de API.</li>
<li>Nombre de usuario y contraseña de <strong>{{site.data.keyword.Bluemix_notm}}:</strong> Puede seguir los pasos de este tema para automatizar por completo la creación de la señal de acceso de {{site.data.keyword.Bluemix_notm}} IAM.</li></ul>
</tr>
<tr>
<td>ID federado</td>
<td><ul><li><strong>Generar una clave de API de {{site.data.keyword.Bluemix_notm}}: </strong> <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">Las claves de API de {{site.data.keyword.Bluemix_notm}}</a> dependen de la cuenta de {{site.data.keyword.Bluemix_notm}} para la que se generan. No puede combinar su clave de API de {{site.data.keyword.Bluemix_notm}} con otro ID de cuenta en la misma señal de {{site.data.keyword.Bluemix_notm}} IAM. Para acceder a los clústeres que se han creado con una cuenta distinta de aquella en la que se basa la clave de API de {{site.data.keyword.Bluemix_notm}}, debe iniciar una sesión en la cuenta para generar una nueva clave de API.</li>
<li><strong>Utilizar un código de acceso puntual: </strong> Si se autentica con {{site.data.keyword.Bluemix_notm}} mediante un código de acceso puntual, no puede automatizar completamente la creación de la señal de {{site.data.keyword.Bluemix_notm}} IAM porque la recuperación del código de acceso puntual requiere una interacción manual con el navegador web. Para automatizar por completo la creación de la señal de {{site.data.keyword.Bluemix_notm}} IAM, debe crear en su lugar una clave de API de {{site.data.keyword.Bluemix_notm}}.</ul></td>
</tr>
</tbody>
</table>

1.  Cree la señal de acceso de {{site.data.keyword.Bluemix_notm}} IAM. La información del cuerpo que se incluye en la solicitud varía en función del método de autenticación de {{site.data.keyword.Bluemix_notm}} que utilice.

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Parámetros de entrada para recuperar señales IAM con el parámetro de entrada en la columna 1 y el valor en la columna 2.">
    <caption>Parámetros de entrada para obtener señales IAM.</caption>
    <thead>
        <th>Parámetros de entrada</th>
        <th>Valores</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabecera</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>Nota</strong>: <code>Yng6Yng=</code> equivale a la autorización codificada de URL para el nombre de usuario <strong>bx</strong> y la contraseña <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente al nombre de usuario y contraseña de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: Su nombre de usuario de {{site.data.keyword.Bluemix_notm}}.</li>
    <li>`password`: Su contraseña de {{site.data.keyword.Bluemix_notm}}.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:`</br><strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar ningún valor.</li></ul></td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente a las claves de API de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: Su clave de API de {{site.data.keyword.Bluemix_notm}}</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar ningún valor.</li></ul></td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente al código de acceso puntual de {{site.data.keyword.Bluemix_notm}}</td>
      <td><ul><li><code>grant_type: urn:ibm:params:oauth:grant-type:passcode</code></li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: Su código de acceso puntual de {{site.data.keyword.Bluemix_notm}}. Ejecute `ibmcloud login --sso` y siga las instrucciones de la salida de la CLI para recuperar el código de acceso puntual mediante su navegador web.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar ningún valor.</li></ul>
    </td>
    </tr>
    </tbody>
    </table>

    Salida de ejemplo para utilizar una clave de API:

    ```
    {
    "access_token": "< iam_access_token>", "refresh_token": "< iam_refresh_token>", "uaa_token": "< uaa_refresh_token": "< uaa_refresh_token>", "token_type": "Bearer", "Bearer", "expires_in": 3600, "expiration": 1493747503 "scope": "ibm openid"
    }

    ```
    {: screen}

    Encontrará la señal de {{site.data.keyword.Bluemix_notm}} IAM en el campo **access_token** de la salida de API. Anote la señal de {{site.data.keyword.Bluemix_notm}} IAM para recuperar información de cabecera adicional en los pasos siguientes.

2.  Recupere el ID de la cuenta de {{site.data.keyword.Bluemix_notm}} con la que desee trabajar. Sustituya `<iam_access_token>` por la señal de {{site.data.keyword.Bluemix_notm}} IAM que ha recuperado del campo **access_token** de la salida de la API en el paso anterior. En la salida de la API, puede encontrar el ID de su cuenta de {{site.data.keyword.Bluemix_notm}} en el campo **resources.metadata.guid**.

    ```
    GET https://accountmanagement.ng.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="Parámetros de entrada para obtener el ID de la cuenta de {{site.data.keyword.Bluemix_notm}} con el parámetro de entrada en la columna 1 y el valor en la columna 2.">
    <caption>Parámetros de entrada para obtener un ID de cuenta de {{site.data.keyword.Bluemix_notm}}.</caption>
    <thead>
  	<th>Parámetros de entrada</th>
  	<th>Valores</th>
    </thead>
    <tbody>
  	<tr>
  		<td>Cabeceras</td>
      <td><ul><li><code>Content-Type: application/json</code></li>
        <li>`Authorization: bearer <iam_access_token>`</li>
        <li><code>Accept: application/json</code></li></ul></td>
  	</tr>
    </tbody>
    </table>

    Salida de ejemplo:

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
            "guid": "<account_ID>",
            "url": "/v1/accounts/<account_ID>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

3.  Genere una nueva señal de {{site.data.keyword.Bluemix_notm}} IAM que incluya sus credenciales de {{site.data.keyword.Bluemix_notm}} y el ID de la cuenta con el que desea trabajar.

    Si utiliza una clave de API de {{site.data.keyword.Bluemix_notm}}, debe utilizar el ID de cuenta de {{site.data.keyword.Bluemix_notm}} y la clave de API se ha creado para la misma. Para acceder a los clústeres en otras cuentas, inicie una sesión en esta cuenta y cree una clave de API de {{site.data.keyword.Bluemix_notm}} que se base en esta cuenta.
    {: note}

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Parámetros de entrada para recuperar señales IAM con el parámetro de entrada en la columna 1 y el valor en la columna 2.">
    <caption>Parámetros de entrada para obtener señales IAM.</caption>
    <thead>
        <th>Parámetros de entrada</th>
        <th>Valores</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabecera</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`<p><strong>Nota</strong>: <code>Yng6Yng=</code> equivale a la autorización codificada de URL para el nombre de usuario <strong>bx</strong> y la contraseña <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente al nombre de usuario y contraseña de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: Su nombre de usuario de {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`password`: Su contraseña de {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar ningún valor.</li>
    <li>`bss_account`: El ID de cuenta de {{site.data.keyword.Bluemix_notm}} que ha recuperado en el paso anterior.</li></ul>
    </td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente a las claves de API de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: Su clave de API de {{site.data.keyword.Bluemix_notm}}.</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar ningún valor.</li>
    <li>`bss_account`: El ID de cuenta de {{site.data.keyword.Bluemix_notm}} que ha recuperado en el paso anterior.</li></ul>
      </td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente al código de acceso puntual de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:passcode`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: Su código de acceso de {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar ningún valor.</li>
    <li>`bss_account`: El ID de cuenta de {{site.data.keyword.Bluemix_notm}} que ha recuperado en el paso anterior.</li></ul></td>
    </tr>
    </tbody>
    </table>

    Salida de ejemplo:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    Encontrará la señal de {{site.data.keyword.Bluemix_notm}} IAM en el campo **access_token**, y la señal de renovación en el campo **refresh_token** de la salida de la API.

4.  Liste las regiones de {{site.data.keyword.containerlong_notm}} disponibles y seleccione la región en la que desea trabajar. Utilice la señal de acceso de IAM y la señal de renovación del paso anterior para crear la información de cabecera.
    ```
    GET https://containers.cloud.ibm.com/v1/regions
    ```
    {: codeblock}

    <table summary="Parámetros de entrada para recuperar las regiones de {{site.data.keyword.containerlong_notm}} con el parámetro de entrada en la columna 1 y el valor en la columna 2.">
    <caption>Parámetros de entrada para recuperar las regiones de {{site.data.keyword.containerlong_notm}}.</caption>
    <thead>
    <th>Parámetros de entrada</th>
    <th>Valores</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabecera</td>
    <td><ul><li>`Authorization: bearer <iam_token>`</li>
    <li>`X-Auth-Refresh-Token: <refresh_token>`</li></ul></td>
    </tr>
    </tbody>
    </table>

    Salida de ejemplo:
    ```
    {
    "regions": [
        {
            "name": "ap-north",
            "alias": "jp-tok",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "ap-south",
            "alias": "au-syd",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "eu-central",
            "alias": "eu-de",
            "cfURL": "api.eu-de.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "uk-south",
            "alias": "eu-gb",
            "cfURL": "api.eu-gb.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "us-east",
            "alias": "us-east",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "us-south",
            "alias": "us-south",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": true
        }
    ]
    }
    ```
    {: screen}

5.  Obtenga una lista de todos los clústeres de la región de {{site.data.keyword.containerlong_notm}} que ha seleccionado. Si desea [ejecutar las solicitudes de API de Kubernetes contra el clúster](#kube_api), asegúrese de anotar el **ID** y la **región** del clúster.

     ```
     GET https://containers.cloud.ibm.com/v1/clusters
     ```
     {: codeblock}

     <table summary="Parámetros de entrada para trabajar con la API de {{site.data.keyword.containerlong_notm}} con el parámetro de entrada en la columna 1 y el valor en la columna 2.">
     <caption>Parámetros de entrada para trabajar con la API de {{site.data.keyword.containerlong_notm}}.</caption>
     <thead>
     <th>Parámetros de entrada</th>
     <th>Valores</th>
     </thead>
     <tbody>
     <tr>
     <td>Cabecera</td>
     <td><ul><li>`Authorization: bearer <iam_token>`</li>
       <li>`X-Auth-Refresh-Token: <refresh_token>`</li><li>`X-Region: <region>`</li></ul></td>
     </tr>
     </tbody>
     </table>

5.  Revise la [documentación de la API de {{site.data.keyword.containerlong_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://containers.cloud.ibm.com/global/swagger-global-api) para obtener una lista de las API soportadas.

<br />


## Cómo trabajar con el clúster utilizando la API de Kubernetes
{: #kube_api}

Puede utilizar la [API de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/using-api/api-overview/) para interactuar con su clúster en {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Las instrucciones siguientes requieren acceso a la red pública en el clúster para conectar con el punto final de servicio público del maestro de Kubernetes.
{: note}

1. Siga los pasos de [Automatización de despliegues de clústeres con la API](#cs_api) para recuperar su señal de acceso de {{site.data.keyword.Bluemix_notm}} IAM, la señal de acceso de IAM, el ID del clúster donde desea ejecutar las solicitudes de API de Kubernetes y la región de {{site.data.keyword.containerlong_notm}} donde se encuentra el clúster.

2. Recupere una señal de renovación delegada de {{site.data.keyword.Bluemix_notm}} IAM.
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="Parámetros de entrada para obtener una señal de renovación delegada de IAM con el parámetro de entrada en la columna 1 y el valor en la columna 2.">
   <caption>Parámetros de entrada para obtener una señal de renovación delegada de IAM. </caption>
   <thead>
   <th>Parámetros de entrada</th>
   <th>Valores</th>
   </thead>
   <tbody>
   <tr>
   <td>Cabecera</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`</br><strong>Nota</strong>: <code>Yng6Yng=</code> equivale a la autorización codificada de URL para el nombre de usuario <strong>bx</strong> y la contraseña <strong>bx</strong>.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>Cuerpo</td>
   <td><ul><li>`delegated_refresh_token_expiry: 600`</li>
   <li>`receiver_client_ids: kube`</li>
   <li>`response_type: delegated_refresh_token` </li>
   <li>`refresh_token`: Su señal de renovación de {{site.data.keyword.Bluemix_notm}} IAM. </li>
   <li>`grant_type: refresh_token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   Salida de ejemplo:
   ```
   {
    "delegated_refresh_token": <delegated_refresh_token>
   }
   ```
   {: screen}

3. Recupere un ID de {{site.data.keyword.Bluemix_notm}} IAM, un acceso de IAM y una señal de renovación de IAM utilizando la señal de renovación delegada del paso anterior. En la salida de la API, puede encontrar la señal de ID de IAM en el campo **id_token**, la señal de acceso de IAM en el campo **access_token** y la señal de renovación IAM en el campo **refresh_token**.
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="Parámetros de entrada para obtener el ID de IAM y las señales de acceso de IAM con el parámetro de entrada en la columna 1 y el valor en la columna 2.">
   <caption>Parámetros de entrada para obtener el ID de IAM y las señales de acceso de IAM.</caption>
   <thead>
   <th>Parámetros de entrada</th>
   <th>Valores</th>
   </thead>
   <tbody>
   <tr>
   <td>Cabecera</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic a3ViZTprdWJl`</br><strong>Note</strong>: <code>a3ViZTprdWJl</code> equals the URL-encoded authorization for the user name <strong><code>kube</code></strong> and the password <strong><code>kube</code></strong>.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>Cuerpo</td>
   <td><ul><li>`refresh_token`: Su señal de renovación delegada de {{site.data.keyword.Bluemix_notm}} IAM. </li>
   <li>`grant_type: urn:ibm:params:oauth:grant-type:delegated-refresh-token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   Salida de ejemplo:
   ```
   {
    "access_token": "<iam_access_token>",
    "id_token": "<iam_id_token>",
    "refresh_token": "<iam_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1553629664,
    "scope": "ibm openid containers-kubernetes"
   }
   ```
   {: screen}

4. Recupere el URL público del maestro de Kubernetes utilizando la señal de acceso de IAM, la señal de ID de IAM, la señal de renovación de IAM y la región de {{site.data.keyword.containerlong_notm}} donde se encuentra el clúster. Puede encontrar el URL en el campo **`publicServiceEndpointURL`** de la salida de la API.
   ```
   GET https://containers.cloud.ibm.com/v1/clusters/<cluster_ID>
   ```
   {: codeblock}

   <table summary="Parámetros de entrada para obtener punto final de servicio público del maestro de Kubernetes con el parámetro de entrada en la columna 1 y el valor en la columna 2.">
   <caption>Parámetros de entrada para obtener punto final de servicio público del maestro de Kubernetes</caption>
   <thead>
   <th>Parámetros de entrada</th>
   <th>Valores</th>
   </thead>
   <tbody>
   <tr>
   <td>Cabecera</td>
     <td><ul><li>`Authorization`: Su señal de acceso de {{site.data.keyword.Bluemix_notm}} IAM.</li><li>`X-Auth-Refresh-Token`: Su señal de renovación de {{site.data.keyword.Bluemix_notm}} IAM.</li><li>`X-Region`: La región de {{site.data.keyword.containerlong_notm}} del clúster que ha recuperado con la API `GET https://containers.cloud.ibm.com/v1/clusters` en [Automatización de despliegues de clústeres con la API](#cs_api). </li></ul>
   </td>
   </tr>
   <tr>
   <td>Vía de acceso</td>
   <td>`<cluster_ID>:` El ID del cluster que ha recuperado con la API `GET https://containers.cloud.ibm.com/v1/clusters` en [Automatización de despliegues de clústeres con la API](#cs_api).      </td>
   </tr>
   </tbody>
   </table>

   Salida de ejemplo:
   ```
   {
    "location": "Dallas",
    "dataCenter": "dal10",
    "multiAzCapable": true,
    "vlans": null,
    "worker_vlans": null,
    "workerZones": [
        "dal10"
    ],
    "id": "1abc123b123b124ab1234a1a12a12a12",
    "name": "mycluster",
    "region": "us-south",
    ...
    "publicServiceEndpointURL": "https://c7.us-south.containers.cloud.ibm.com:27078"
   }
   ```
   {: screen}

5. Ejecute solicitudes de API de Kubernetes en el clúster utilizando la señal de ID de IAM que ha recuperado anteriormente. Por ejemplo, liste la versión de Kubernetes que se ejecuta en el clúster.

   Si ha habilitado la verificación de certificados SSL en la infraestructura de prueba de API, asegúrese de inhabilitar esta característica.
   {: tip}

   ```
   GET <publicServiceEndpointURL>/version
   ```
   {: codeblock}

   <table summary="Parámetros de entrada para ver la versión de Kubernetes que se ejecuta en el clúster con el parámetro de entrada en la columna 1 y el valor en la columna 2. ">
   <caption>Parámetros de entrada para ver la versión de Kubernetes que se ejecuta en el clúster. </caption>
   <thead>
   <th>Parámetros de entrada</th>
   <th>Valores</th>
   </thead>
   <tbody>
   <tr>
   <td>Cabecera</td>
   <td>`Authorization: bearer <id_token>`</td>
   </tr>
   <tr>
   <td>Vía de acceso</td>
   <td>`<publicServiceEndpointURL>`: El **`publicServiceEndpointURL`** del maestro de Kubernetes que ha recuperado en el paso anterior.      </td>
   </tr>
   </tbody>
   </table>

   Salida de ejemplo:
   ```
   {
    "major": "1",
    "minor": "13",
    "gitVersion": "v1.13.4+IKS",
    "gitCommit": "c35166bd86eaa91d17af1c08289ffeab3e71e11e",
    "gitTreeState": "clean",
    "buildDate": "2019-03-21T10:08:03Z",
    "goVersion": "go1.11.5",
    "compiler": "gc",
    "platform": "linux/amd64"
   }
   ```
   {: screen}

6. Revise la [documentación de la API de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/kubernetes-api/) para obtener una lista de las API soportadas en la última versión de Kubernetes. Asegúrese de utilizar la documentación de la API que coincida con la versión de Kubernetes del clúster. Si no utiliza la versión más reciente de Kubernetes, añada la versión al final del URL. Por ejemplo, para acceder a la documentación de la API para la versión 1.12, añada `v1.12`.


## Renovación de señales de acceso de {{site.data.keyword.Bluemix_notm}} IAM y obtención de nuevas señales de renovación con la API
{: #cs_api_refresh}

Cada señal de acceso de {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) que se emite mediante la API caduca transcurrida una hora. Debe renovar la señal de acceso de forma periódica para garantizar el acceso a la API de {{site.data.keyword.Bluemix_notm}}. Puede utilizar los mismos pasos para obtener una nueva señal de renovación.
{:shortdesc}

Antes de empezar, asegúrese de que tiene una señal de renovación de {{site.data.keyword.Bluemix_notm}} IAM o una clave de API de {{site.data.keyword.Bluemix_notm}} para solicitar una nueva señal de acceso.
- **Señal de renovación:** Siga las instrucciones en [Automatización del proceso de creación y gestión de clústeres con la API de {{site.data.keyword.Bluemix_notm}}](#cs_api).
- **Clave de API:** Recupere su clave de API de [{{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/) tal como se indica a continuación.
   1. En la barra de menús, pulse **Gestionar** > **Acceso (IAM)**.
   2. Pulse la página **Usuarios** y, a continuación, selecciónese usted mismo.
   3. En el panel **Claves de API**, pulse **Crear una clave de API de IBM Cloud**.
   4. Especifique un **Nombre** y una **Descripción** para la clave de API y, a continuación, pulse **Crear**.
   4. Pulse **Mostrar** para ver la clave de API generada en su nombre.
   5. Copie la clave de API de forma que la pueda utilizar para recuperar su señal de acceso de {{site.data.keyword.Bluemix_notm}} IAM.

Utilice los pasos siguientes si desea crear una señal de {{site.data.keyword.Bluemix_notm}} IAM o si desea obtener una nueva señal de renovación.

1.  Genere una nueva señal de acceso de {{site.data.keyword.Bluemix_notm}} IAM utilizando la señal de renovación o la clave de API de {{site.data.keyword.Bluemix_notm}}.
    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Parámetros de entrada para una nueva señal de IAM con el parámetro de entrada en la columna 1 y el valor en la columna 2.">
    <caption>Parámetros de entrada para una nueva señal de {{site.data.keyword.Bluemix_notm}} IAM</caption>
    <thead>
    <th>Parámetros de entrada</th>
    <th>Valores</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabecera</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li>
      <li>`Authorization: Basic Yng6Yng=`</br></br><strong>Nota:</strong> <code>Yng6Yng=</code> equivale a la autorización codificada de URL para el nombre de usuario <strong>bx</strong> y la contraseña <strong>bx</strong>.</li></ul></td>
    </tr>
    <tr>
    <td>Cuerpo al utilizar la señal de renovación</td>
    <td><ul><li>`grant_type: refresh_token`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`refresh_token:` Su señal de renovación de {{site.data.keyword.Bluemix_notm}} IAM. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</li>
    <li>`bss_account:` Su ID de cuenta de {{site.data.keyword.Bluemix_notm}}. </li></ul><strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    <tr>
      <td>Cuerpo al utilizar la clave de API de {{site.data.keyword.Bluemix_notm}}</td>
      <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey:` Su clave de API de {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`uaa_client_ID: cf`</li>
        <li>`uaa_client_secret:`</li></ul><strong>Nota:</strong> Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    </tbody>
    </table>

    Ejemplo de salida de API:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    Encontrará la nueva señal de {{site.data.keyword.Bluemix_notm}} IAM en el campo **access_token**, y la señal de renovación en el campo **refresh_token** de la salida de la API.

2.  Continúe trabajando con la [documentación de la API de {{site.data.keyword.containerlong_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://containers.cloud.ibm.com/global/swagger-global-api) utilizando la señal de los pasos anteriores.

<br />


## Renovación de señales de acceso de {{site.data.keyword.Bluemix_notm}} IAM y obtención de nuevas señales de renovación con la CLI
{: #cs_cli_refresh}

Al iniciar una nueva sesión de la CLI, o si ha caducado la sesión actual de la CLI de 24 horas, debe establecer el contexto para el clúster ejecutando `ibmcloud ks cluster-config --cluster <cluster_name>`. Cuando establezca el contexto del clúster con este mandato, se descargará el archivo
`kubeconfig` para el clúster de Kubernetes. Además, se emiten una señal de ID de Identity and Access Management (IAM) de
{{site.data.keyword.Bluemix_notm}} y una señal de renovación para proporcionar autenticación.
{: shortdesc}

**Señal de ID**: cada señal de ID de IAM que se emite a través de la CLI caduca después de una hora. Cuando caduca la señal de ID, se envía la señal de renovación al proveedor de señales para renovar la señal de ID. Se renueva la autenticación y puede seguir ejecutando mandatos en el clúster.

**Señal de renovación**: las señales de renovación caducan cada 30 días. Si caduca la señal de renovación, no se puede renovar la señal de ID y no podrá seguir ejecutando mandatos en la CLI. Puede obtener una nueva señal de renovación ejecutando `ibmcloud ks cluster-config --cluster <cluster_name>`. Este mandato también renueva la señal de ID.
