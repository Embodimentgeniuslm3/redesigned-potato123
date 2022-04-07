---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-04"

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



# Compilación de contenedores a partir de imágenes
{: #images}

Una imagen de Docker es la base para cada contenedor que pueda crear con {{site.data.keyword.containerlong}}.
{:shortdesc}

Se crea una imagen a partir de Dockerfile, que es un archivo que contiene instrucciones para crear la imagen. Un Dockerfile puede hacer referencia a artefactos de compilación en sus instrucciones que se almacenan por separado, como por ejemplo una app, la configuración de la app y sus dependencias.

## Planificación de registros de imagen
{: #planning_images}

Las imágenes normalmente se almacenan en un registro que puede ser tanto de acceso público (registro público) como estar configurado con acceso limitado para un pequeño grupo de usuarios (registro privado).
{:shortdesc}

Los registros públicos, como por ejemplo Docker Hub, se pueden utilizar para empezar a trabajar con Docker y Kubernetes para crear la primera app contenerizada de un clúster. Pero, cuando se trate de aplicaciones de empresa, utilice un registro privado, como el que se suministra en {{site.data.keyword.registryshort_notm}}, para proteger sus imágenes frente a un posible uso y modificación por parte de usuarios no autorizados. El administrador del clúster debe configurar los registros privados para asegurarse de que las credenciales para acceder al registro privado están disponibles para los usuarios del clúster.


Puede utilizar varios registros con {{site.data.keyword.containerlong_notm}} para desplegar apps en el clúster.

|Registro|Descripción|Ventaja|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started)|Con esta opción, puede configurar su propio repositorio seguro de imágenes de Docker en {{site.data.keyword.registryshort_notm}} donde puede almacenar y compartir de forma segura imágenes entre los usuarios del clúster.|<ul><li>Gestione el acceso a las imágenes de la cuenta.</li><li>Utilice las apps de ejemplo y las imágenes proporcionadas por {{site.data.keyword.IBM_notm}}, como {{site.data.keyword.IBM_notm}} Liberty, como imagen padre y añádales su propio código de app.</li><li>Exploración automática de imágenes en busca de vulnerabilidades potenciales por parte de Vulnerability Advisor, que incluyen recomendaciones específicas del sistema operativo para solucionarlas.</li></ul>|
|Cualquier otro registro privado|Para conectar cualquier registro privado existente a su clúster, cree un [secreto de extracción de imágenes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/containers/images/). El secreto se utiliza para guardar el URL del registro y las credenciales de forma segura en un secreto de Kubernetes.|<ul><li>Utilice registros privados existentes independientemente de su origen (Docker Hub, registros propiedad de la organización u otros registros privados de Cloud).</li></ul>|
|[Public Docker Hub![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://hub.docker.com/){: #dockerhub}|Utilice esta opción para utilizar imágenes públicas existentes en Docker Hub directamente en sus [despliegues de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) cuando no sean necesarios cambios de Dockerfile. <p>**Nota:** Tenga en cuenta que es posible que esta opción no se ajuste a los requisitos de seguridad de su organización, como la gestión de accesos, la exploración de vulnerabilidades o la privacidad de las apps.</p>|<ul><li>No se requiere configuración adicional para el clúster.</li><li>Incluye diversas aplicaciones de código abierto.</li></ul>|
{: caption="Opciones de registro de imágenes público y privado" caption-side="top"}

Después de configurar el registro de imágenes, los usuarios del clúster pueden utilizar las imágenes para desplegar apps en el clúster.

Obtenga más información sobre cómo [proteger su información personal](/docs/containers?topic=containers-security#pi) cuando se trabaja con imágenes de contenedor.

<br />


## Configuración de contenido de confianza para imágenes de contenedor
{: #trusted_images}

Puede crear contenedores a partir de imágenes de confianza que están firmadas y almacenadas en {{site.data.keyword.registryshort_notm}}, y evitar despliegues de imágenes vulnerables o que no hayan sido firmadas.
{:shortdesc}

1.  [Firme imágenes para un contenido de confianza](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent). Después de establecer la confianza en sus imágenes, puede gestionar el contenido de confianza y los firmantes que pueden extraer las imágenes para su registro.
2.  Para imponer una política en la que únicamente se pueden utilizar imágenes firmadas para crear contenedores en su clúster, [añada Container Image Security Enforcement (beta)](/docs/services/Registry?topic=registry-security_enforce#security_enforce).
3.  Despliegue la app.
    1. [Despliegue en el espacio de nombres `default` de Kubernetes](#namespace).
    2. [Despliegue en un espacio de nombres de Kubernetes diferente, o desde una cuenta o región diferente de {{site.data.keyword.Bluemix_notm}}](#other).

<br />


## Despliegue de contenedores desde una imagen de {{site.data.keyword.registryshort_notm}} en el espacio de nombres `default` de Kubernetes
{: #namespace}

Puede desplegar contenedores en el clúster desde una imagen pública proporcionada por IBM o desde una imagen privada almacenada en el espacio de nombres de {{site.data.keyword.registryshort_notm}}. Para obtener más información sobre cómo el clúster accede a las imágenes del registro, consulte [Visión general sobre cómo está autorizado el clúster para extraer imágenes de {{site.data.keyword.registrylong_notm}}](#cluster_registry_auth).
{:shortdesc}

Antes de empezar:
1. [Configure un espacio de nombres en {{site.data.keyword.registryshort_notm}} y envíe imágenes a este espacio de nombres](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2. [Cree un clúster](/docs/containers?topic=containers-clusters#clusters_ui).
3. Si tiene un clúster existente creado antes del **25 de febrero de 2019**, [actualice el clúster para que utilice la clave de API `imagePullSecret`](#imagePullSecret_migrate_api_key).
4. [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Para desplegar un contenedor en el espacio de nombres **predeterminado** del clúster:

1.  Cree un archivo de configuración de despliegue denominado `mydeployment.yaml`.
2.  Defina el despliegue y la imagen que desee a utilizar del espacio de nombres de {{site.data.keyword.registryshort_notm}}.

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app_name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - name: <app_name>
            image: <region>.icr.io/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    Sustituya las variables de URL de imagen por la información de la imagen:
    *  **`<app_name>`**: el nombre de la app.
    *  **`<region>`**: el punto final de API de {{site.data.keyword.registryshort_notm}} del dominio del registro. Para obtener una lista del dominio de la región donde ha iniciado sesión, ejecute `ibmcloud cr api`.
    *  **`<namespace>`**: el espacio de nombres del registro. Para obtener información de su espacio de nombres, ejecute `ibmcloud cr namespace-list`.
    *  **`<my_image>:<tag>`**: la imagen y la etiqueta que desea utilizar para crear el contenedor. Para obtener las imágenes disponibles en el registro, ejecute `ibmcloud cr images`.

3.  Cree el despliegue en el clúster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

<br />


## Cómo autorizar al clúster para que puede extraer imágenes de un registro
{: #cluster_registry_auth}

Para extraer imágenes de un registro, su clúster de {{site.data.keyword.containerlong_notm}} utiliza un tipo especial de Kubernetes secret, un `imagePullSecret`. Este secreto de extracción de imágenes almacena las credenciales para acceder a un registro de contenedor. El registro de contenedor puede ser su espacio de nombres en {{site.data.keyword.registrylong_notm}}, un espacio de nombres de {{site.data.keyword.registrylong_notm}} que pertenece a una cuenta distinta de {{site.data.keyword.Bluemix_notm}} o cualquier otro registro privado como, por ejemplo, Docker. Su clúster está configurado para extraer imágenes del espacio de nombres en {{site.data.keyword.registrylong_notm}} y desplegar los contenedores de estas imágenes en el espacio de nombres `default` de Kubernetes en el clúster. Si tiene que extraer imágenes en otros espacios de nombres de Kubernetes de clúster o de otros registros, debe configurar el secreto de extracción de imágenes.
{:shortdesc}

**¿Cómo se configura el clúster para extraer imágenes del espacio de nombres `default` de Kubernetes?**<br>
Cuando se crea un clúster, este tiene un ID de servicio de {{site.data.keyword.Bluemix_notm}} IAM al que se otorga a una política de rol de acceso al servicio de IAM de **Lector** sobre {{site.data.keyword.registrylong_notm}}. Las credenciales de ID de servicio se copian en una clave de API que no caduca y que se guarda en secretos de extracción de imágenes en el clúster. Los secretos de extracción de imágenes se añaden al espacio de nombres `default` de Kubernetes y la lista de secretos en la cuenta de servicio `default` para este espacio de nombres. Mediante el uso de secretos de extracción de imágenes, los despliegues pueden extraer imágenes (acceso de solo lectura) en el [registro global y regional](/docs/services/Registry?topic=registry-registry_overview#registry_regions) para crear contenedores en el espacio de nombres `default` de Kubernetes. El registro global almacena de forma segura imágenes públicas proporcionadas por IBM a las que puede hacer referencia en sus despliegues en lugar de tener referencias distintas para las imágenes que se almacenan en cada registro regional. El registro regional almacena de forma segura sus imágenes de Docker privadas.

**¿Puedo restringir el acceso de obtención a un determinado registro regional?**<br>
Sí, puede [editar la política de IAM existente del ID de servicio](/docs/iam?topic=iam-serviceidpolicy#access_edit) que restringe el rol de acceso al servicio de **Lector** a dicho registro regional o a un recurso del registro, como por ejemplo un espacio de nombres. Para poder personalizar las políticas de IAM de registro, debe [habilitar las políticas de {{site.data.keyword.Bluemix_notm}} IAM para {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-user#existing_users).

  ¿Desea proteger aún más sus credenciales de registro? Solicite al administrador del clúster que [habilite {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) en
el clúster para cifrar los secretos de Kubernetes en el clúster, como por ejemplo el `imagePullSecret` que almacena las credenciales de registro.
  {: tip}

**¿Puedo extraer imágenes en espacios de nombres de Kubernetes que no sean `default`?**<br>
No de forma predeterminada. Utilizando la configuración de clúster predeterminada, puede desplegar contenedores desde cualquier imagen almacenada en su espacio de nombres de {{site.data.keyword.registrylong_notm}} al espacio de nombres `default` de Kubernetes de su clúster. Para utilizar estas imágenes en otros espacios de nombres de Kubernetes o en otras cuentas de {{site.data.keyword.Bluemix_notm}}, [tiene la opción de copiar o crear su propio secreto de extracción de imágenes](#other).

**¿Puedo extraer imágenes de una cuenta de {{site.data.keyword.Bluemix_notm}} distinta?**<br>
Sí, cree una clave de API en la cuenta de {{site.data.keyword.Bluemix_notm}} que desea utilizar. A continuación, cree un secreto de extracción de imágenes que almacene las credenciales de clave de API en cada clúster y en cada espacio de nombres de clúster y de los que desee extraer. [Siga el este ejemplo que utiliza una clave de API de ID de servicio autorizada](#other_registry_accounts).

Para utilizar un registro que no sea de {{site.data.keyword.Bluemix_notm}}, como por ejemplo Docker, consulte [Acceso a imágenes almacenadas en otros registros privados](#private_images).

**¿La clave de API tiene que ser para un ID de servicio? ¿Qué ocurre si alcanzo el límite de ID de servicio para mi cuenta?**<br>
La configuración de clúster predeterminada crea un ID de servicio para almacenar las credenciales de clave de API de {{site.data.keyword.Bluemix_notm}} IAM en el secreto de extracción de imágenes. No obstante, también puede crear una clave de API para un usuario individual y almacenar esas credenciales en un secreto de extracción de imágenes. Si alcanza el [límite de IAM para los ID de servicio](/docs/iam?topic=iam-iam_limits#iam_limits), el clúster se crea sin el ID de servicio ni el secreto de extracción de imágenes y no puede extraer las imágenes de los dominios del registro `icr.io` de forma predeterminada. Debe [crear su propio secreto de extracción de imágenes](#other_registry_accounts), pero utilizando una clave de API para un usuario individual como, por ejemplo, un ID funcional, no un ID de servicio de {{site.data.keyword.Bluemix_notm}} IAM.

**El secreto de extracción de imágenes de mi clúster utiliza una señal de registro. ¿Todavía funciona una señal?**<br>

El método anterior de autorizar el acceso del clúster {{site.data.keyword.registrylong_notm}} mediante la creación automática de una [señal](/docs/services/Registry?topic=registry-registry_access#registry_tokens)
y el almacenamiento de la señal en un secreto de extracción de imágenes recibe soporte, pero ha quedado en desuso.
{: deprecated}

Las señales autorizan el acceso a los dominios del registro `registry.bluemix.net` en desuso, mientras que las claves de API autorizan el acceso a los dominios del registro `icr.io`. Durante el periodo de transición de la autenticación basada en señales a la basada en claves de API, durante un periodo de tiempo se crearán tanto secretos de obtención de imágenes basados en señales como basados en claves de API. Si tiene secretos de obtención de imágenes basados tanto en señales como en claves de API, su clúster podrá extraer imágenes tanto del dominio `registry.bluemix.net` como del dominio `icr.io` en el espacio de nombres `default` de Kubernetes.

Antes de que las señales y dominios de `registry.bluemix.net` en desuso dejen de estar soportados, actualice los secretos de extracción de imágenes para que utilicen el método de claves de API para el espacio de nombres [`default` de Kubernetes namespace](#imagePullSecret_migrate_api_key) y [cualquier otro espacio de nombres o cuenta](#other) que utilice. A continuación, actualice los despliegues para extraer de los dominios del registro `icr.io`.

**Después de copiar o de crear un secreto de extracción de imágenes otro espacio de nombres de Kubernetes, ¿he terminado?**<br>
No del todo. Los contenedores deben tener autorización para extraer imágenes utilizando el secreto que ha creado. Puede añadir el secreto de extracción de imágenes a la cuenta de servicio para el espacio de nombres, o bien puede hacer referencia al secreto en cada despliegue. Para obtener instrucciones, consulte [Utilización del secreto de extracción de imágenes para desplegar contenedores](/docs/containers?topic=containers-images#use_imagePullSecret).

<br />


## Actualización de clústeres existentes para que utilicen el secreto de obtención de imágenes de clave de API
{: #imagePullSecret_migrate_api_key}

Los nuevos clústeres de {{site.data.keyword.containerlong_notm}} almacenan una clave de API en [un secreto de extracción de imágenes para autorizar el acceso a {{site.data.keyword.registrylong_notm}}](#cluster_registry_auth). Con estos secretos de extracción de imágenes, puede desplegar contenedores desde imágenes almacenadas en los dominios de registro `icr.io`. Para los clústeres creados antes del **25 de febrero de 2019**, debe actualizar el clúster para que almacene una clave de API en lugar de una señal de registro en el secreto de extracción de imágenes.
{: shortdesc}

**Antes de empezar**:
*   [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*   Asegúrese de que tiene los siguientes permisos:
    *   Rol de plataforma **Operador o Administrador** de {{site.data.keyword.Bluemix_notm}} IAM para {{site.data.keyword.containerlong_notm}}. El propietario de la cuenta puede otorgarle el rol ejecutando:
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name containers-kubernetes --roles Administrator,Operator
        ```
        {: pre}
    *   Rol de plataforma **Administrador** de {{site.data.keyword.Bluemix_notm}} IAM para {{site.data.keyword.registrylong_notm}}, en todas las regiones y grupos de recursos. El propietario de la cuenta puede otorgarle el rol ejecutando:
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
        ```
        {: pre}

**Para actualizar el secreto de extracción de imágenes del espacio de nombres `default` de Kubernetes**:
1.  Obtenga el ID del clúster.
    ```
    ibmcloud ks clusters
    ```
    {: pre}
2.  Ejecute el mandato siguiente para crear un ID de servicio para el clúster, asigne al ID de servicio el rol de servicio de IAM de **Lector** sobre {{site.data.keyword.registrylong_notm}}, cree una clave de API para copiar las credenciales de ID de servicio y almacene la clave de API en un secreto de extracción de imágenes de Kubernetes en el clúster. El secreto de extracción de imágenes está en el espacio de nombres `default` de Kubernetes.
    ```
    ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Cuando ejecuta este mandato, se inicia la creación de credenciales de IAM y de secretos de extracción de imágenes, que puede tardar un rato en finalizar. No puede desplegar contenedores que extraigan una imagen de los dominios de {{site.data.keyword.registrylong_notm}}
`icr.io` hasta que se creen los secretos de extracción de la imagen.
    {: important}

3.  Verifique que los secretos de extracción de imágenes se han creado en el clúster. Tenga en cuenta que tiene un secreto de extracción de imágenes para cada región de {{site.data.keyword.registrylong_notm}}.
    ```
    kubectl get secrets
    ```
    {: pre}
    Salida de ejemplo:
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
4.  Actualice los despliegues de contenedor para extraer imágenes del nombre de dominio `icr.io`.
5.  Opcional: Si tiene un cortafuegos, asegúrese de que [permite el tráfico de red de salida a las subredes de registro](/docs/containers?topic=containers-firewall#firewall_outbound) para los dominios que utiliza.

**¿Qué es lo siguiente?**
*   Para extraer imágenes en espacios de nombres de Kubernetes que no sean `default` o de otras cuentas de {{site.data.keyword.Bluemix_notm}}, [copie o cree otro secreto de extracción de imágenes](/docs/containers?topic=containers-images#other).
*   Para restringir el acceso al secreto de extracción de imágenes a determinados recursos de registro, como por ejemplo espacios de nombres o regiones:
    1.  Asegúrese de que [las políticas de {{site.data.keyword.Bluemix_notm}} IAM para {{site.data.keyword.registrylong_notm}} están habilitadas](/docs/services/Registry?topic=registry-user#existing_users).
    2.  [Edite las políticas de {{site.data.keyword.Bluemix_notm}} IAM](/docs/iam?topic=iam-serviceidpolicy#access_edit) para el ID de servicio o [cree otro secreto de extracción de imágenes](/docs/containers?topic=containers-images#other_registry_accounts).

<br />


## Utilización de un secreto de extracción de imágenes para acceder a otros espacios de nombres de Kubernetes del clúster, otras cuentas de {{site.data.keyword.Bluemix_notm}} o registros privados externos
{: #other}

Configure su propio secreto de extracción de imágenes en su clúster para desplegar contenedores en espacios de nombres de Kubernetes que no sean `default`, para utilizar imágenes almacenadas en otras cuentas de {{site.data.keyword.Bluemix_notm}} o para utilizar imágenes almacenadas en registros privados externos. Además, puede crear su propio secreto de extracción de imágenes para aplicar políticas de acceso de IAM que restrinjan los permisos a determinados espacios de nombres de imágenes de registro o acciones (como por ejemplo `push` o `pull`).
{:shortdesc}

Después de crear el secreto de extracción de imágenes, los contenedores deben utilizar el secreto para que reciban autorización para extraer una imagen del registro. Puede añadir el secreto de extracción de imágenes a la cuenta de servicio para el espacio de nombres, o bien puede hacer referencia al secreto en cada despliegue. Para obtener instrucciones, consulte [Utilización del secreto de extracción de imágenes para desplegar contenedores](/docs/containers?topic=containers-images#use_imagePullSecret).

Los secretos de extracción de imágenes solo son válidos para los espacios de nombres de Kubernetes para los que fueron creados. Repita estos pasos para cada espacio de nombres en el que desee desplegar contenedores. Las imágenes de [DockerHub](#dockerhub) no necesitan secretos de extracción de imágenes.
{: tip}

Antes de empezar:

1.  [Configure un espacio de nombres en {{site.data.keyword.registryshort_notm}} y envíe imágenes a este espacio de nombres](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2.  [Cree un clúster](/docs/containers?topic=containers-clusters#clusters_ui).
3.  Si tiene un clúster existente creado antes del **25 de febrero de 2019**, [actualice el clúster para que utilice secreto de extracción de imágenes de claves de API](#imagePullSecret_migrate_api_key).
4.  [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

<br/>
Para utilizar su propio secreto de extracción de imágenes, elija una de las opciones siguientes:
- [Copie el secreto de extracción de imágenes](#copy_imagePullSecret) del espacio de nombres predeterminado de Kubernetes a otros espacios de nombres de su clúster.
- [Cree nuevas credenciales de API de IAM y almacénelas en un secreto de obtención de imágenes](#other_registry_accounts) para acceder a las imágenes de otras cuentas de {{site.data.keyword.Bluemix_notm}} o para aplicar políticas IAM que restrinjan el acceso a determinados dominios de registro o espacios de nombres.
- [Crear un secreto de extracción de imágenes para acceder a imágenes en registros privados externos](#private_images).

<br/>
Si ya ha creado un secreto de extracción de imágenes en el espacio de nombres que desea utilizar en el despliegue, consulte [Despliegue de contenedores mediante el `imagePullSecret` creado](#use_imagePullSecret).

### Copiar un secreto de extracción de imágenes existente
{: #copy_imagePullSecret}

Puede copiar un secreto de extracción de imágenes, como el que se crea automáticamente para el espacio de nombres `default` de Kubernetes, a otros espacios de nombres en el clúster. Pero si desea utilizar diferentes credenciales de clave de API de {{site.data.keyword.Bluemix_notm}} IAM para este espacio de nombres, como por ejemplo para restringir el acceso a espacios de nombres específicos o para extraer imágenes de otras cuentas de {{site.data.keyword.Bluemix_notm}}, se recomienda que [cree un secreto de extracción de imágenes](#other_registry_accounts).
{: shortdesc}

1.  Obtenga una lista de los espacios de nombres de Kubernetes disponibles en el clúster, o cree un espacio de nombres que utilizar.
    ```
    kubectl get namespaces
    ```
    {: pre}

    Salida de ejemplo:
    ```
    default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
    ```
    {: screen}

    Para crear un espacio de nombres:
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  Obtenga una lista de los secretos de extracción de imágenes existentes en el espacio de nombres `default` de Kubernetes para {{site.data.keyword.registrylong_notm}}.
    ```
    kubectl get secrets -n default | grep icr
    ```
    {: pre}
    Salida de ejemplo:
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
3.  Copie cada secreto de extracción de imágenes del espacio de nombres `default` en el espacio de nombres que elija. Los nuevos secretos de extracción de imágenes se denominan `<namespace_name>-icr-<region>-io`.
    ```
    kubectl get secret default-us-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-uk-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-de-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-au-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-jp-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
4.  Verifique que los secretos se han creado correctamente.
    ```
    kubectl get secrets -n <namespace_name>
    ```
    {: pre}
5.  [Añada el secreto de extracción de imágenes a una cuenta de servicio de Kubernetes para que cualquier pod del espacio de nombres pueda utilizar el secreto de extracción de imágenes cuando despliegue un contenedor](#use_imagePullSecret).

### Creación de un secreto de extracción de imágenes con credenciales de clave de API de IAM distintas para tener un mayor control o acceso a las imágenes de otras cuentas de {{site.data.keyword.Bluemix_notm}}
{: #other_registry_accounts}

Puede asignar políticas de acceso de {{site.data.keyword.Bluemix_notm}} IAM a usuarios o a un ID de servicio para restringir los permisos a determinados espacios de nombres de imágenes de registro o acciones (como por ejemplo `push` o `pull`). A continuación, cree una clave de API y almacene estas credenciales de registro en un secreto de extracción de imágenes para el clúster.
{: shortdesc}

Por ejemplo, para acceder a las imágenes de otras cuentas de {{site.data.keyword.Bluemix_notm}}, cree una clave de API que almacene las credenciales de {{site.data.keyword.registryshort_notm}} de un usuario o un ID de servicio en dicha cuenta. A continuación, en la cuenta de su clúster, guarde las credenciales de clave de API en un secreto de extracción de imágenes para cada clúster y cada espacio de nombres de clúster.

En los pasos siguientes se crea una clave de API que almacena las credenciales de un ID de servicio de {{site.data.keyword.Bluemix_notm}} IAM. En lugar de utilizar un ID de servicio, es posible que desee crear una clave de API para un ID de usuario que tenga una política de acceso al servicio de {{site.data.keyword.Bluemix_notm}} IAM sobre {{site.data.keyword.registryshort_notm}}. Sin embargo, asegúrese de que el usuario sea un ID funcional o tenga un plan para que el clúster pueda acceder al registro en el caso de que el usuario se vaya.
{: note}

1.  Obtenga una lista de los espacios de nombres de Kubernetes disponibles en el clúster, o cree un espacio de nombres para utilizar donde desea desplegar los contenedores desde las imágenes de registro.
    ```
    kubectl get namespaces
    ```
    {: pre}

    Salida de ejemplo:
    ```
    default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
    ```
    {: screen}

    Para crear un espacio de nombres:
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  Cree un ID de servicio de {{site.data.keyword.Bluemix_notm}} IAM para el clúster que se utiliza para las políticas de IAM y las credenciales de clave de API en el secreto de extracción de imágenes. Asegúrese de proporcionar al ID de servicio una descripción que le ayude a recuperar el ID de servicio posteriormente; por ejemplo, puede incluir el nombre del clúster y el del espacio de nombres.
    ```
    ibmcloud iam service-id-create <cluster_name>-<kube_namespace>-id --description "Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
3.  Cree una política de {{site.data.keyword.Bluemix_notm}} IAM personalizada para el ID de servicio del clúster que otorgue acceso a {{site.data.keyword.registryshort_notm}}.
    ```
    ibmcloud iam service-policy-create <cluster_service_ID> --roles <service_access_role> --service-name container-registry [--region <IAM_region>] [--resource-type namespace --resource <registry_namespace>]
    ```
    {: pre}
    <table>
    <caption>Descripción de los componentes de este mandato</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster_service_ID&gt;</em></code></td>
    <td>Obligatorio. Sustituya este valor por el ID de servicio `<cluster_name>-<kube_namespace>-id` que ha creado anteriormente para el clúster de Kubernetes.</td>
    </tr>
    <tr>
    <td><code>--service-name <em>container-registry</em></code></td>
    <td>Obligatorio. Especifique `container-registry` para que la política de IAM se aplique a {{site.data.keyword.registrylong_notm}}.</td>
    </tr>
    <tr>
    <td><code>--roles <em>&lt;service_access_role&gt;</em></code></td>
    <td>Obligatorio. Especifique el [rol de acceso al servicio para {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-iam#service_access_roles) al que desea limitar el acceso al ID de servicio. Los valores posibles son `Reader` (Lector), `Writer` (Escritor) y `Manager` (Gestor).</td>
    </tr>
    <tr>
    <td><code>--region <em>&lt;IAM_region&gt;</em></code></td>
    <td>Opcional. Si desea limitar la política de acceso a determinadas regiones de IAM, especifique las regiones en una lista separada por comas. Los valores posibles son `au-syd`, `eu-gb`, `eu-de`, `jp-tok`, `us-south` y `global`.</td>
    </tr>
    <tr>
    <td><code>--resource-type <em>namespace</em> --resource <em>&lt;registry_namespace&gt;</em></code></td>
    <td>Opcional. Si desea limitar el acceso solo a imágenes de determinados [espacios de nombres de {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-registry_overview#registry_namespaces), escriba `namespace` para el tipo de recurso y especifique el `<registry_namespace>`. Para ver una lista de los espacios de nombres, ejecute `ibmcloud cr namespaces`.</td>
    </tr>
    </tbody></table>
4.  Cree una clave de API para el ID de servicio. Asigne a la clave de API un nombre parecido al del ID de servicio e incluya el ID de servicio que ha creado anteriormente, ``<cluster_name>-<kube_namespace>-id`. Asegúrese de especificar para la clave de API una descripción que le ayude a recuperar la clave posteriormente.
    ```
    ibmcloud iam service-api-key-create <cluster_name>-<kube_namespace>-key <cluster_name>-<kube_namespace>-id --description "API key for service ID <service_id> in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
5.  Recupere el valor **API Key** de la salida del mandato anterior.
    ```
    Conserve la clave de API. No se podrá recuperar una vez creada.

    Name          <cluster_name>-<kube_namespace>-key   
    Description   key_for_registry_for_serviceid_for_kubernetes_cluster_multizone_namespace_test   
    Bound To      crn:v1:bluemix:public:iam-identity::a/1bb222bb2b33333ddd3d3333ee4ee444::serviceid:ServiceId-ff55555f-5fff-6666-g6g6-777777h7h7hh   
    Created At    2019-02-01T19:06+0000   
    API Key       i-8i88ii8jjjj9jjj99kkkkkkkkk_k9-llllll11mmm1   
    Locked        false   
    UUID          ApiKey-222nn2n2-o3o3-3o3o-4p44-oo444o44o4o4   
    ```
    {: screen}
6.  Cree un secreto de extracción de imágenes de Kubernetes para almacenar las credenciales de clave de API en el espacio de nombres del clúster. Repita este paso para cada dominio `icr.io`, espacio de nombres de Kubernetes y clúster del que desea extraer de imágenes del registro con estas credenciales de IAM de ID de servicio.
    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name> --docker-server=<registry_URL> --docker-username=iamapikey --docker-password=<api_key_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Descripción de los componentes de este mandato</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obligatorio. Especifique el espacio de nombres de Kubernetes del clúster que ha utilizado para el nombre de ID de servicio.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obligatorio. Especifique un nombre para el secreto de extracción de imágenes.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obligatorio. Establezca el URL en el registro de imágenes en el que se ha configurado el espacio de nombres de registro. Los dominios de registro disponibles son:<ul>
    <li>AP norte (Tokio): `jp.icr.io`</li>
    <li>AP sur (Sídney): `au.icr.io`</li>
    <li>UE central (Frankfurt): `de.icr.io`</li>
    <li>RU sur (Londres): `uk.icr.io`</li>
    <li>EE. UU. sur (Dallas): `us.icr.io`</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username iamapikey</code></td>
    <td>Obligatorio. Especifique el nombre de usuario para iniciar una sesión en su registro privado. Para {{site.data.keyword.registryshort_notm}}, el nombre de usuario se establece en el valor <strong><code>iamapikey</code></strong>.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obligatorio. Especifique el valor de **API Key** que ha recuperado anteriormente.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obligatorio. Si tiene una, especifique la dirección de correo electrónico de Docker. Si no la tiene, especifique una dirección de correo electrónico ficticia, como por ejemplo `a@b.c`. Este correo electrónico es necesario para crear un secreto de Kubernetes, pero no se utiliza después de la creación.</td>
    </tr>
    </tbody></table>
7.  Verifique que el secreto se haya creado correctamente. Sustituya <em>&lt;kubernetes_namespace&gt;</em> por el espacio de nombres en el que ha creado el secreto de extracción de imágenes.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}
8.  [Añada el secreto de extracción de imágenes a una cuenta de servicio de Kubernetes para que cualquier pod del espacio de nombres pueda utilizar el secreto de extracción de imágenes cuando despliegue un contenedor](#use_imagePullSecret).

### Acceso a imágenes almacenadas en otros registros privados
{: #private_images}

Si ya tiene un registro privado, debe almacenar las credenciales del registro en un secreto de extracción de imágenes de Kubernetes y hacer referencia a dicho secreto en su archivo de configuración.
{:shortdesc}

Antes de empezar:

1.  [Cree un clúster](/docs/containers?topic=containers-clusters#clusters_ui).
2.  [Defina su clúster como destino de la CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Para crear un secreto de extracción de imágenes:

1.  Cree el secreto de Kubernetes para almacenar las credenciales del registro privado.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Descripción de los componentes de este mandato</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obligatorio. El espacio de nombres de Kubernetes del clúster en el que desea utilizar el secreto y desplegar los contenedores. Ejecute <code>kubectl get namespaces</code> para obtener una lista de todos los espacios de nombres del clúster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obligatorio. El nombre que desea utilizar para su <code>imagePullSecret</code>.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obligatorio. El URL al registro en el que se están almacenadas sus imágenes privadas.</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Obligatorio. El nombre de usuario para iniciar una sesión en su registro privado.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obligatorio. El valor de la señal de registro que ha recuperado anteriormente.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obligatorio. Si tiene una, especifique la dirección de correo electrónico de Docker. Si no la tiene, especifique una dirección de correo electrónico ficticia, como por ejemplo `a@b.c`. Este correo electrónico es necesario para crear un secreto de Kubernetes, pero no se utiliza después de la creación.</td>
    </tr>
    </tbody></table>

2.  Verifique que el secreto se haya creado correctamente. Sustituya <em>&lt;kubernetes_namespace&gt;</em> por el nombre del espacio de nombres en el que ha creado `imagePullSecret`.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [Cree un pod que haga referencia al secreto de extracción de imágenes](#use_imagePullSecret).

<br />


## Utilización del secreto de extracción de imágenes para desplegar contenedores
{: #use_imagePullSecret}

Puede definir un secreto de extracción de imágenes en su despliegue de pod o almacenar el secreto de extracción de imágenes en su cuenta de servicio de Kubernetes para que esté disponible para todos los despliegues que no especifican una cuenta de servicio.
{: shortdesc}

Seleccione una de las opciones siguientes:
* [Hacer referencia al secreto de extracción de imágenes en su despliegue de pod](#pod_imagePullSecret): Utilice esta opción si de forma predeterminada no desea otorgar acceso a su registro a todos los pods en su espacio de nombres.
* [Almacenar el secreto de extracción de imágenes en la cuenta de servicio de Kubernetes](#store_imagePullSecret): Utilice esta opción para otorgar acceso a las imágenes en su registro para todos los despliegues en los espacios de nombres de Kubernetes seleccionados.

Antes de empezar:
* [Cree un secreto de extracción de imágenes](#other) para acceder a las imágenes de otros registros o espacios de nombres de Kubernetes, distintos de `default`.
* [Defina su clúster como destino de la CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

### Cómo hacer referencia al secreto de extracción de imágenes en el despliegue del pod
{: #pod_imagePullSecret}

Cuando haga referencia al secreto de extracción de imágenes en un despliegue de pod, el secreto de extracción de imágenes solo es válido para este pod y no se puede compartir con otros pods en el espacio de nombres.
{:shortdesc}

1.  Cree un archivo de configuración de pod denominado `mypod.yaml`.
2.  Defina el pod y el secreto de extracción de imágenes para acceder a las imágenes de {{site.data.keyword.registrylong_notm}}.

    Para acceder a una imagen privada:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    Para acceder a una imagen pública de {{site.data.keyword.Bluemix_notm}}:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: icr.io/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    <table>
    <caption>Visión general de los componentes del archivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>El nombre del contenedor para desplegar en el clúster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>El espacio de nombres donde se almacena la imagen. Para obtener una lista de los espacios de nombres disponibles, ejecute `ibmcloud cr namespace-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>El nombre de la imagen a utilizar. Para ver una lista de todas las imágenes disponibles en una cuenta de {{site.data.keyword.Bluemix_notm}}, ejecute `ibmcloud cr image-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>La versión de la imagen que desea utilizar. Si no se especifica ninguna etiqueta, se utiliza la imagen etiquetada como <strong>la más reciente (latest)</strong>.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>El nombre del secreto de extracción de imágenes que ha creado anteriormente.</td>
    </tr>
    </tbody></table>

3.  Guarde los cambios.
4.  Cree el despliegue en el clúster.
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### Almacenamiento del secreto de extracción de imágenes en la cuenta de servicio de Kubernetes para el espacio de nombres seleccionado
{:#store_imagePullSecret}

Cada espacio de nombres tiene una cuenta de servicio de Kubernetes que se denomina `default`. Puede añadir el secreto de extracción de imágenes a esta cuenta de servicio para otorgar acceso a las imágenes en el registro. Los despliegues que automáticamente no especifican una cuenta de servicio utiliza la cuenta de servicio `default` para este espacio de nombres.
{:shortdesc}

1. Compruebe si ya existe un secreto de extracción de imágenes para la cuenta de servicio predeterminada.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   Cuando `<none>` se visualiza en la entrada **Image pull secrets**, no existe ningún secreto de extracción de imágenes.  
2. Añada el secreto de extracción de imágenes a su cuenta de servicio predeterminada.
   - **Para añadir el secreto de extracción de imágenes cuando no se ha definido ningún secreto de extracción de imágenes:**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "<image_pull_secret_name>"}]}'
       ```
       {: pre}
   - **Para añadir el secreto de extracción de imágenes cuando ya se ha definido un secreto de extracción de imágenes:**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"<image_pull_secret_name>"}}]'
       ```
       {: pre}
3. Verifique que su secreto de extracción de imágenes se ha añadido a la cuenta de servicio predeterminada.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   Salida de ejemplo:
   ```
   Name:                default
   Namespace:           <namespace_name>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  <image_pull_secret_name>
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. Despliegue un contenedor desde una imagen en el registro.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <container_name>
         image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
   ```
   {: codeblock}

5. Crear el despliegue en el clúster.
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


## En desuso: Utilización de una señal de registro para desplegar contenedores de una imagen de {{site.data.keyword.registrylong_notm}}
{: #namespace_token}

Puede desplegar contenedores en su clúster desde una imagen pública proporcionada por IBM o desde una imagen privada almacenada en el espacio de nombres de {{site.data.keyword.registryshort_notm}}. Los clústeres existentes utilizan una [señal](/docs/services/Registry?topic=registry-registry_access#registry_tokens) de registro que se almacena en un `imagePullSecret` del clúster para autorizar el acceso para extraer imágenes de los nombres de dominio `registry.bluemix.net`.
{:shortdesc}

Cuando crea un clúster, se crean automáticamente señales y secretos tanto para [el registro regional más cercano y el registro global](/docs/services/Registry?topic=registry-registry_overview#registry_regions). El registro global almacena de forma segura imágenes públicas proporcionadas por IBM a las que puede hacer referencia en sus despliegues en lugar de tener referencias distintas para las imágenes que se almacenan en cada registro regional. El registro regional almacena de forma segura sus imágenes de Docker privadas. Las señales se utilizan para autorizar el acceso de solo lectura a cualquiera de los espacios de nombres que configure en {{site.data.keyword.registryshort_notm}} para que pueda trabajar con estas imágenes públicas (registro global) y privadas (registros regionales).

Cada señal se debe guardar en `imagePullSecret` de Kubernetes para que resulte accesible para un clúster de Kubernetes cuando se despliegue una app contenerizada. Cuando se crea el clúster, {{site.data.keyword.containerlong_notm}} almacena automáticamente las señales para los registros global (imágenes públicas proporcionadas por IBM) y regionales en los secretos de extracción de imágenes de Kubernetes. Los secretos de extracción de imágenes se añaden al espacio de nombres `default` de Kubernetes, al espacio de nombres `kube-system` y a la lista de secretos de la cuenta de servicio `default` correspondiente a estos espacios de nombres.

Este método de utilizar una señal para autorizar el acceso del clúster a {{site.data.keyword.registrylong_notm}} recibe soporte para los nombres de dominio `registry.bluemix.net`, pero está en desuso. En su lugar, [utilice el método de clave de API](#cluster_registry_auth) para autorizar el acceso del clúster a los nuevos nombres de dominio de registro de `icr.io`.
{: deprecated}

En función de dónde esté la imagen y de dónde esté el contenedor, debe desplegar contenedores siguiendo distintos pasos.
*   [Despliegue un contenedor en el espacio de nombres `default` de Kubernetes con una imagen que se encuentre en la misma región que el clúster](#token_default_namespace)
*   [Despliegue un contenedor en un espacio de nombres de Kubernetes que no sea `default`](#token_copy_imagePullSecret)
*   [Despliegue un contenedor con una imagen que esté en otra región o en otra cuenta de {{site.data.keyword.Bluemix_notm}} distinta a la de su clúster](#token_other_regions_accounts)
*   [Despliegue un contenedor con una imagen que sea de un registro privado, no de IBM](#private_images)

Con esta configuración inicial, puede desplegar contenedores desde cualquier imagen disponible en un espacio de nombres de la cuenta de {{site.data.keyword.Bluemix_notm}} en el espacio de nombres **default** del clúster. Para desplegar un contenedor en otros espacios de nombres del clúster, o para utilizar una imagen almacenada en otra región de {{site.data.keyword.Bluemix_notm}} o en otra cuenta de {{site.data.keyword.Bluemix_notm}}, debe [crear su propio secreto de extracción de imágenes para el clúster](#other).
{: note}

### En desuso: Despliegue de imágenes en el espacio de nombres `default` de Kubernetes con una señal de registro
{: #token_default_namespace}

Con la señal de registro almacenada en el secreto de extracción de imágenes, puede desplegar un contenedor desde cualquier imagen que esté disponible en {{site.data.keyword.registrylong_notm}} regional en el espacio de nombres **default** del clúster.
{: shortdesc}

Antes de empezar:
1. [Configure un espacio de nombres en {{site.data.keyword.registryshort_notm}} y envíe imágenes a este espacio de nombres](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2. [Cree un clúster](/docs/containers?topic=containers-clusters#clusters_ui).
3. [Defina su clúster como destino de la CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Para desplegar un contenedor en el espacio de nombres **default** del clúster, cree un file de configuración.

1.  Cree un archivo de configuración de despliegue denominado `mydeployment.yaml`.
2.  Defina el despliegue y la imagen que desee utilizar del espacio de nombres de {{site.data.keyword.registryshort_notm}}.

    Para utilizar una imagen privada de un espacio de nombres de {{site.data.keyword.registryshort_notm}}:

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app_name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - name: <app_name>
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **Sugerencia:** Para recuperar la información del espacio de nombres, ejecute `ibmcloud cr namespace-list`.

3.  Cree el despliegue en el clúster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Sugerencia:** También puede desplegar un archivo de configuración existente, como por ejemplo una de las imágenes públicas proporcionadas por IBM. En este ejemplo se utiliza la imagen **ibmliberty** de la región EE. UU. sur.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}

### En desuso: Copia del secreto de extracción de imágenes basado en señal del espacio de nombres predeterminado en otros espacios de nombres del clúster
{: #token_copy_imagePullSecret}

Puede copiar el secreto de extracción de imágenes con las credenciales de señal de registro que se crea automáticamente para el espacio de nombres `default` de Kubernetes en otros espacios de nombres en el clúster.
{: shortdesc}

1. Obtenga una lista de los espacios de nombres disponibles en el clúster.
   ```
   kubectl get namespaces
   ```
   {: pre}

   Salida de ejemplo:
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. Opcional: Cree un espacio de nombres en el clúster.
   ```
   kubectl create namespace <namespace_name>
   ```
   {: pre}

3. Copie los secretos de extracción de imágenes del espacio de nombres `default` en el espacio de nombres que elija. Los nuevos secretos de extracción de imágenes se denominan `bluemix-<namespace_name>-secret-regional` y `bluemix-<namespace_name>-secret-international`.
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  Verifique que los secretos se han creado correctamente.
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. [Despliegue un contenedor utilizando el `imagePullSecret`](#use_imagePullSecret) en su espacio de nombres.


### En desuso: Creación de un secreto de extracción de imágenes basado en señal para acceder a imágenes de otras regiones y cuentas de {{site.data.keyword.Bluemix_notm}}
{: #token_other_regions_accounts}

Para acceder a imágenes de otras regiones o cuentas de {{site.data.keyword.Bluemix_notm}}, debe crear una señal de registro y guardar sus credenciales en un secreto de extracción de imágenes.
{: shortdesc}

1.  Si no tiene una señal, [cree una señal para el registro al que desea acceder.](/docs/services/Registry?topic=registry-registry_access#registry_tokens_create)
2.  Obtenga una lista de las señales en la cuenta de {{site.data.keyword.Bluemix_notm}}.

    ```
    ibmcloud cr token-list
    ```
    {: pre}

3.  Anote el ID de la señal que desea utilizar.
4.  Recupere el valor de la señal. Sustituya <em>&lt;token_ID&gt;</em> por el ID de la señal que ha recuperado en el paso anterior.

    ```
    ibmcloud cr token-get <token_id>
    ```
    {: pre}

    El valor de la señal se muestra en el campo **Token** de la salida de la CLI.

5.  Cree el secreto de Kubernetes para almacenar la información de la señal.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Descripción de los componentes de este mandato</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obligatorio. El espacio de nombres de Kubernetes del clúster en el que desea utilizar el secreto y desplegar los contenedores. Ejecute <code>kubectl get namespaces</code> para obtener una lista de todos los espacios de nombres del clúster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obligatorio. El nombre que desea utilizar para su secreto de extracción de imágenes.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obligatorio. El URL del registro de imágenes en el que está configurado el espacio de nombres.<ul><li>Para los espacios de nombres definidos en EE. UU. sur y EE. UU. este es <code>registry.ng.bluemix.net</code></li><li>Para los espacios de nombres definidos en RU sur es <code>registry.eu-gb.bluemix.net</code></li><li>Para los espacios de nombres definidos en UE central (Frankfurt) es <code>registry.eu-de.bluemix.net</code></li><li>Para los espacios de nombres definidos en Australia (Sídney) es <code>registry.au-syd.bluemix.net</code></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Obligatorio. El nombre de usuario para iniciar una sesión en su registro privado. Para {{site.data.keyword.registryshort_notm}}, el nombre de usuario se establece en el valor <strong><code>token</code></strong>.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obligatorio. El valor de la señal de registro que ha recuperado anteriormente.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obligatorio. Si tiene una, especifique la dirección de correo electrónico de Docker. Si no tiene, especifique una dirección de correo electrónico ficticia, como por ejemplo a@b.c. Este correo electrónico es obligatorio para crear un secreto de Kubernetes, pero no se utiliza después de la creación.</td>
    </tr>
    </tbody></table>

6.  Verifique que el secreto se haya creado correctamente. Sustituya <em>&lt;kubernetes_namespace&gt;</em> por el espacio de nombres en el que ha creado el secreto de extracción de imágenes.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  [Despliegue un contenedor utilizando el secreto de extracción de imágenes](#use_imagePullSecret) en el espacio de nombres.
