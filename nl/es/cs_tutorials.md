---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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



# Guía de aprendizaje: Creación de clústeres de Kubernetes
{: #cs_cluster_tutorial}

Con esta guía de aprendizaje podrá desplegar y gestionar un clúster de Kubernetes en {{site.data.keyword.containerlong}}. Aprenderá a automatizar el despliegue, la operación, el escalado y la supervisión de apps contenerizadas en un clúster.
{:shortdesc}

En esta serie de guías de aprendizaje verá cómo una firma de relaciones públicas ficticia utiliza funciones de Kubernetes para desplegar una app contenerizada en {{site.data.keyword.Bluemix_notm}}. Mediante {{site.data.keyword.toneanalyzerfull}}, la empresa PR analiza sus notas de prensa y recibe comentarios.


## Objetivos
{: #tutorials_objectives}

En esta primera guía de aprendizaje, actuará como administrador de redes de la empresa PR. Configurará un clúster de Kubernetes personalizado que se utilizará para desplegar y probar una versión de Hello World de la app en {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

-   Creará un clúster con una agrupación de nodos trabajadores de un nodo trabajador.
-   Instalará las CLI para ejecutar [mandatos de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubectl.docs.kubernetes.io/) y para gestionar imágenes de Docker en {{site.data.keyword.registrylong_notm}}.
-   Creará un repositorio de imágenes privadas en {{site.data.keyword.registrylong_notm}} para almacenar las imágenes.
-   Añadirá el servicio {{site.data.keyword.toneanalyzershort}} al clúster para que cualquier app del clúster pueda utilizar el servicio.


## Tiempo necesario
{: #tutorials_time}

40 minutos


## Público
{: #tutorials_audience}

Esta guía de aprendizaje está destinada a los desarrolladores de software y administradores de la red que están creando un clúster de Kubernetes por primera vez.
{: shortdesc}

## Requisitos previos
{: #tutorials_prereqs}

-  Consulte los pasos que debe llevar a cabo para [prepararse para crear un clúster](/docs/containers?topic=containers-clusters#cluster_prepare).
-  Asegúrese de tener las políticas de acceso siguientes:
    - El [rol de plataforma **Administrador** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para {{site.data.keyword.containerlong_notm}}
    - El [rol de plataforma **Administrador** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para {{site.data.keyword.registrylong_notm}}
    - El [rol de servicio **Escritor** o **Gestor** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre {{site.data.keyword.containerlong_notm}}


## Lección 1: Creación de un clúster y configuración de la CLI
{: #cs_cluster_tutorial_lesson1}

Cree su clúster de Kubernetes en la consola de {{site.data.keyword.Bluemix_notm}} e instale las CLI necesarias.
{: shortdesc}

**Para crear el clúster**

Puesto que el suministro puede tardar unos minutos, cree el clúster antes de instalar las CLI.

1.  [En la consola de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/kubernetes/catalog/cluster/create), cree un clúster gratuito o estándar con una agrupación de nodos trabajadores que contenga un nodo trabajador.

    También puede crear un [clúster en la CLI](/docs/containers?topic=containers-clusters#clusters_cli_steps).
    {: tip}

A medida que se suministra el clúster, instale las siguientes CLI para gestionar los clústeres:
-   CLI de {{site.data.keyword.Bluemix_notm}}
-   Plugin de {{site.data.keyword.containerlong_notm}}
-   CLI de Kubernetes
-   Plugin de {{site.data.keyword.registryshort_notm}}

Si en su lugar desea utilizar la consola de {{site.data.keyword.Bluemix_notm}} después de crear el clúster, puede ejecutar mandatos de CLI directamente desde el navegador web en [Kubernetes Terminal](/docs/containers?topic=containers-cs_cli_install#cli_web).
{: tip}

</br>
**Para instalar las CLI y sus requisitos previos**

1. Instale la [CLI de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli?topic=cloud-cli-getting-started). Esta instalación incluye:
  - La CLI básica de {{site.data.keyword.Bluemix_notm}}. El prefijo para ejecutar mandatos mediante la CLI de {{site.data.keyword.Bluemix_notm}} es `ibmcloud`.
  - El plugin de {{site.data.keyword.containerlong_notm}}. El prefijo para ejecutar mandatos utilizando la CLI de {{site.data.keyword.Bluemix_notm}} es `ibmcloud ks`.
  - Plugin de {{site.data.keyword.registryshort_notm}}. Utilice este plugin para configurar y gestionar un repositorio de imágenes privadas en {{site.data.keyword.registryshort_notm}}. El prefijo para ejecutar mandatos de registro es `ibmcloud cr`.

2. Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Escriba
sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite.
  ```
  ibmcloud login
  ```
  {: pre}

  Si tiene un ID federado, utilice el distintivo `--sso` para iniciar la sesión. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso.
  {: tip}

3. Siga las indicaciones para seleccionar una cuenta.

5. Verifique que los plugins están instalados correctamente.
  ```
  ibmcloud plugin list
  ```
  {: pre}

  El plugin {{site.data.keyword.containerlong_notm}} se visualiza en los resultados como **container-service** y el plugin
{{site.data.keyword.registryshort_notm}} se visualiza en los resultados como **container-registry**.

6. Para desplegar apps en los clústeres, [instale la CLI de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). El prefijo para ejecutar mandatos utilizando la CLI de Kubernetes es `kubectl`.

  1. Descargue la versión `major.minor` de la CLI de Kubernetes que coincida con la versión `major.minor` del clúster de Kubernetes que piensa utilizar. La versión de Kubernetes predeterminada actual de {{site.data.keyword.containerlong_notm}} es la 1.13.6.
    - **OS X**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl)
    - **Linux**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl)
    - **Windows**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe)

  2. Si está utilizando OS X o Linux, siga estos pasos.

    1. Mueva el archivo ejecutable al directorio `/usr/local/bin`.
      ```
      mv /filepath/kubectl /usr/local/bin/kubectl
      ```
      {: pre}

    2. Asegúrese de que `/usr/local/bin` aparezca en la variable del sistema `PATH`. La variable `PATH` contiene todos los directorios en los que el sistema operativo puede encontrar archivos ejecutables. Los directorios que se muestran en la variable `PATH` sirven para distintos propósitos. `/usr/local/bin` se utiliza para guardar archivos ejecutables correspondientes a software que no forma parte del sistema operativo y que ha instalado manualmente el administrador del sistema.
      ```
      echo $PATH
      ```
      {: pre}
      Ejemplo de salida de CLI:
      ```
      /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
      ```
      {: screen}

    3. Convierta el archivo en ejecutable.
      ```
      chmod +x /usr/local/bin/kubectl
      ```
      {: pre}

¡Buen trabajo! Ha instalado correctamente las CLI para las siguientes lecciones y guías de aprendizaje. A continuación, configure el entorno de clúster y añada el servicio {{site.data.keyword.toneanalyzershort}}.


## Lección 2: Configuración de su registro privado
{: #cs_cluster_tutorial_lesson2}

Configure un repositorio de imágenes privadas en {{site.data.keyword.registryshort_notm}} y añada secretos a su clúster de Kubernetes para que la app pueda acceder al servicio {{site.data.keyword.toneanalyzershort}}.
{: shortdesc}

1.  Si el clúster está en un grupo de recursos distinto de `default`, elija como destino dicho grupo de recursos. Para ver el grupo de recursos al que pertenece cada clúster, ejecute `ibmcloud ks clusters`.
   ```
   ibmcloud target -g <resource_group_name>
   ```
   {: pre}

2.  Configure su propio repositorio de imágenes privadas en {{site.data.keyword.registryshort_notm}} para almacenar y compartir de forma segura imágenes de Docker con todos los usuarios del clúster. Un repositorio privado en {{site.data.keyword.Bluemix_notm}} se identifica mediante un espacio de nombres. El espacio de nombres sirve para crear un URL exclusivo para el repositorio privado que los desarrolladores pueden utilizar para acceder a las imágenes de Docker.

    Obtenga más información sobre cómo [proteger su información personal](/docs/containers?topic=containers-security#pi) cuando se trabaja con imágenes de contenedor.

    En este ejemplo, la empresa PR desea crear un solo repositorio de imágenes en {{site.data.keyword.registryshort_notm}}, por lo que eligen `pr_firm` como el espacio de nombres para agrupar todas las imágenes de la cuenta. Sustituya &lt;namespace&gt; por un espacio de nombres de su elección que no esté relacionado con la guía de aprendizaje.

    ```
    ibmcloud cr namespace-add <namespace>
    ```
    {: pre}

3.  Antes de continuar con el paso siguiente, verifique que el despliegue del nodo trabajador se ha completado.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Cuando el suministro del nodo trabajador finaliza, el estado pasa a **Preparado** y puede empezar a enlazar servicios de {{site.data.keyword.Bluemix_notm}}.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Zone   Version
    kube-mil01-pafe24f557f070463caf9e31ecf2d96625-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   free           normal   Ready    mil01      1.13.6
    ```
    {: screen}

## Lección 3: Configuración del entorno de clúster
{: #cs_cluster_tutorial_lesson3}

Establezca el contexto para el clúster de Kubernetes en la CLI.
{: shortdesc}

Cada vez que inicie la sesión en la CLI de {{site.data.keyword.containerlong}} para trabajar con clústeres, deberá ejecutar estos mandatos para establecer el archivo de configuración del clúster como una variable de la sesión. La CLI de Kubernetes utiliza esta variable para buscar un archivo de configuración local y los certificados necesarios para conectar con el clúster en {{site.data.keyword.Bluemix_notm}}.

1.  Obtenga el mandato para establecer la variable de entorno y descargar los archivos de configuración de Kubernetes.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Cuando termine la descarga de los archivos de configuración, se muestra un mandato que puede utilizar para establecer la vía de acceso al archivo de configuración de
Kubernetes como variable de entorno.

    Ejemplo para OS X:
    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}
    ¿Utiliza PowerShell de Windows? Incluya el distintivo `--powershell` para obtener las variables de entorno en formato de PowerShell de Windows.
    {: tip}

2.  Copie y pegue el mandato que se muestra en el terminal para definir la variable de entorno `KUBECONFIG`.

3.  Compruebe que la variable de entorno `KUBECONFIG` se haya establecido correctamente.

    Ejemplo para OS X:

    ```
    echo $KUBECONFIG
    ```
    {: pre}

    Salida:

    ```
    /Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

4.  Compruebe que los mandatos `kubectl` se ejecutan correctamente con el clúster comprobando la versión del cliente de la CLI de Kubernetes.

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

## Lección 4: Adición de un servicio al clúster
{: #cs_cluster_tutorial_lesson4}

Con los servicios de {{site.data.keyword.Bluemix_notm}}, puede aprovechar la funcionalidad ya desarrollada de sus apps. Cualquier servicio de {{site.data.keyword.Bluemix_notm}} que esté enlazado al clúster de Kubernetes puede ser utilizado por cualquier app desplegada en ese clúster. Repita los pasos siguientes para cada servicio de {{site.data.keyword.Bluemix_notm}} que desee utilizar con sus apps.
{: shortdesc}

1.  Añada el servicio {{site.data.keyword.toneanalyzershort}} a la cuenta de {{site.data.keyword.Bluemix_notm}}. Sustituya <service_name> con un nombre para su instancia de servicio.

    Cuando añada el servicio {{site.data.keyword.toneanalyzershort}} a su cuenta, se visualizará un mensaje que indica que el servicio no es gratuito. Si limita la llamada a la API, en esta guía de aprendizaje no se incurre en ningún cargo por el servicio {{site.data.keyword.watson}}. [Revise la información sobre precios correspondientes al servicio {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/catalog/services/tone-analyzer).
    {: note}

    ```
    ibmcloud resource service-instance-create <service_name> tone-analyzer standard us-south
    ```
    {: pre}

2.  Vincule la instancia de {{site.data.keyword.toneanalyzershort}} al espacio de nombres `default` de Kubernetes correspondiente al clúster. Luego podrá crear sus propios espacios de nombres para gestionar el acceso de los usuarios a los recursos de Kubernetes, pero por ahora utilice el espacio de nombres `default`. Los espacios de nombres de Kubernetes son diferentes del espacio de nombres del registro que ha creado anteriormente.

    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace default --service <service_name>
    ```
    {: pre}

    Salida:

    ```
    ibmcloud ks cluster-service-bind pr_firm_cluster default mytoneanalyzer
    Binding service instance to namespace...
    OK
    Namespace:	default
    Secret name:	binding-mytoneanalyzer
    ```
    {: screen}

3.  Verifique que el secreto de Kubernetes se ha creado en el espacio de nombres del clúster. Cada servicio de
{{site.data.keyword.Bluemix_notm}} se define mediante un archivo JSON que incluye información confidencial como la clave de API de Identity and Access Management (IAM) de {{site.data.keyword.Bluemix_notm}} y el URL que utiliza el contenedor para obtener acceso. Para almacenar esta información de forma segura, se utilizan secretos de Kubernetes. En este ejemplo, el secreto incluye la clave de API para acceder a la instancia de {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} suministrada en su cuenta.

    ```
    kubectl get secrets --namespace=default
    ```
    {: pre}

    Salida:

    ```
    NAME                       TYPE                                  DATA      AGE
    binding-mytoneanalyzer     Opaque                                1         1m
    bluemix-default-secret     kubernetes.io/dockercfg               1         1h
    default-token-kf97z        kubernetes.io/service-account-token   3         1h
    ```
    {: screen}

</br>
¡Buen trabajo! El clúster está configurado y su entorno local está listo para que inicie el despliegue de apps en el clúster.

## ¿Qué es lo siguiente?
{: #tutorials_next}

* Probar sus conocimientos y [responder a un cuestionario ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibmcloud-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php).
* Probar la [Guía de aprendizaje: Despliegue de apps en clústeres de Kubernetes](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial) para desplegar la app de la empresa PR en el clúster que ha creado.
