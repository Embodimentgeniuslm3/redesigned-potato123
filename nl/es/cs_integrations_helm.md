---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, helm, without tiller, private cluster tiller, integrations, helm chart

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



# Adición de servicios utilizando diagramas de Helm
{: #helm}

Puede añadir apps de Kubernetes complejas a su clúster utilizando diagramas de Helm.
{: shortdesc}

**¿Qué es Helm y cómo debo utilizarlo?** </br>
[Helm ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://helm.sh) es un gestor de paquetes de Kubernetes que utiliza diagramas de Helm para definir, instalar y actualizar apps de Kubernetes complejas en su clúster. Los diagramas de Helm empaquetan las especificaciones para generar archivos YAML para los recursos de Kubernetes que crean la app. Estos recursos de Kubernetes se aplican automáticamente en el clúster y Helm les asigna una versión. También puede utilizar Helm para especificar y empaquetar su propia app y dejar que Helm genere los archivos YAML para los recursos de Kubernetes.  

Para utilizar Helm en el clúster, debe instalar la CLI de Helm en la máquina local y el tiller del servidor de Helm en cada clúster en el que desee utilizar Helm.

**¿Qué diagramas de Helm están soportados en {{site.data.keyword.containerlong_notm}}?** </br>
Para obtener una visión general de los diagramas de Helm disponibles, consulte el [catálogo de diagramas de Helm ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/kubernetes/solutions/helm-charts). Los diagramas de Helm que se muestran en este catálogo están agrupados de la siguiente manera:

- **iks-charts**: diagramas de Helm aprobados para {{site.data.keyword.containerlong_notm}}. El nombre de este repositorio se ha cambiado de `ibm` a `iks-charts`.
- **ibm-charts**: diagramas de Helm aprobados para clústeres privados de {{site.data.keyword.containerlong_notm}} y {{site.data.keyword.Bluemix_notm}}.
- **kubernetes**: diagramas de Helm proporcionados por la comunidad de Kubernetes y que el gobierno de la comunidad considera `stable`. Estos gráficos no se ha verificado que funcionen en clústeres privados de {{site.data.keyword.containerlong_notm}} o {{site.data.keyword.Bluemix_notm}}.
- **kubernetes-incubator**: diagramas de Helm proporcionados por la comunidad de Kubernetes y que el gobierno de la comunidad considera `incubator`. Estos gráficos no se ha verificado que funcionen en clústeres privados de {{site.data.keyword.containerlong_notm}} o {{site.data.keyword.Bluemix_notm}}.

Los diagramas de Helm de los repositorios **iks-charts** e **ibm-charts** están completamente integrados en la organización de soporte de {{site.data.keyword.Bluemix_notm}}. Si tiene alguna pregunta o algún problema con el uso de estos diagramas de Helm, puede utilizar uno de los canales de soporte de {{site.data.keyword.containerlong_notm}}. Para obtener más información, consulte [Obtención de ayuda y soporte](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

**¿Cuáles son los requisitos previos para utilizar Helm? ¿Puede utilizar Helm en un clúster privado?** </br>
Para desplegar diagramas de Helm, debe instalar la CLI de Helm en la máquina local e instalar el tiller del servidor de Helm en el clúster. La imagen de Tiller se almacena en el registro de contenedores de Google público. Para acceder a la imagen durante la instalación de Tiller, el clúster debe permitir la conectividad de red pública con el registro público de contenedores de Google. Los clústeres que habilitan el punto final de servicio público puede acceder automáticamente a la imagen. Los clústeres privados que están protegidos con un cortafuegos personalizado, o los clústeres que solo han habilitado el punto final de servicio privado, no permiten el acceso a la imagen de Tiller. Puede [extraer la imagen en su máquina local y enviar la imagen al espacio de nombres en {{site.data.keyword.registryshort_notm}}](#private_local_tiller), o bien [instalar los diagramas de Helm sin utilizar tiller](#private_install_without_tiller).


## Configuración de Helm en un clúster con acceso público
{: #public_helm_install}

Si el clúster ha habilitado el punto final de servicio público, puede instalar el tiller del servidor de Helm utilizando la imagen pública del registro de contenedores de Google.
{: shortdesc}

Antes de empezar:
- [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Para instalar Tiller con una cuenta de servicio de Kubernetes y el enlace de rol de clúster en el espacio de nombres `kube-system`, asegúrese de tener el [rol `cluster-admin`](/docs/containers?topic=containers-users#access_policies).

Para instalar Helm en un clúster con acceso público:

1. Instale la <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI de Helm <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> en la máquina local.

2. Compruebe si ya ha instalado Tiller con una cuenta de servicio de Kubernetes en el clúster.
   ```
   kubectl get serviceaccount --all-namespaces | grep tiller
   ```
   {: pre}

   Salida de ejemplo si Tiller está instalado:
   ```
   kube-system      tiller                               1         189d
   ```
   {: screen}

   La salida de ejemplo incluye el espacio de nombres Kubernetes y el nombre de la cuenta de servicio de Tiller. Si Tiller no está instalado con una cuenta de servicio en el clúster, no se devuelve ninguna salida de CLI.

3. **Importante**: Para mantener la seguridad en el clúster, instale Tiller en el clúster con una cuenta de servicio y un enlace de rol de clúster.
   - **Si Tiller se ha instalado con una cuenta de servicio:**
     1. Cree un enlace de rol de clúster para la cuenta de servicio de Tiller. Sustituya `<namespace>` por el espacio de nombres donde Tiller está instalado en el clúster.
        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=<namespace>:tiller -n <namespace>
        ```
        {: pre}

     2. Actualice Tiller. Sustituya `<tiller_service_account_name>` por el nombre de la cuenta de servicio de Kubernetes para Tiller que ha recuperado en el paso anterior.
        ```
        helm init --upgrade --service-account <tiller_service_account_name>
        ```
        {: pre}

     3. Verifique que el pod `tiller-deploy` tiene el **Estado** `En ejecución` en el clúster.
        ```
        kubectl get pods -n <namespace> -l app=helm
        ```
        {: pre}

        Salida de ejemplo:

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

   - **Si Tiller no se ha instalado con una cuenta de servicio:**
     1. Cree una cuenta de servicio de Kubernetes y un enlace de rol de clúster para Tiller en el espacio de nombres `kube-system` de su clúster.
        ```
        kubectl create serviceaccount tiller -n kube-system
        ```
        {: pre}

        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=kube-system:tiller -n kube-system
        ```
        {: pre}

     2. Verifique que se ha creado la cuenta de servicio de Tiller.
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

     3. Inicialice la CLI de Helm e instale Tiller en el clúster con la cuenta de servicio que ha creado.
        ```
        helm init --service-account tiller
        ```
        {: pre}

     4. Verifique que el pod `tiller-deploy` tiene el **Estado** `En ejecución` en el clúster.
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        Salida de ejemplo:
        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

4. Añada los repositorios de Helm de {{site.data.keyword.Bluemix_notm}} a la instancia de Helm.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

5. Actualice los repositorios para recuperar las versiones más recientes de todos los diagramas de Helm.
   ```
   helm repo update
   ```
   {: pre}

6. Obtenga una lista de los diagramas de Helm disponibles actualmente en los repositorios de {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

7. Identifique el diagrama de Helm que desea instalar y siga las instrucciones del archivo `README` del diagrama de Helm para instalar el diagrama de Helm en el clúster.


## Clústeres privados: Envío de la imagen de Tiller al espacio de nombres en IBM Cloud Container Registry
{: #private_local_tiller}

Puede extraer la imagen de Tiller en la máquina local, enviarla a su espacio de nombres en {{site.data.keyword.registryshort_notm}} e instalar Tiller en el clúster privado utilizando la imagen de {{site.data.keyword.registryshort_notm}}.
{: shortdesc}

Si desea instalar un diagrama de Helm sin utilizar Tiller, consulte [Clústeres privados: Instalación de diagramas de Helm sin utilizar Tiller](#private_install_without_tiller).
{: tip}

Antes de empezar:
- Instale Docker en la máquina local. Si ha instalado la [CLI de {{site.data.keyword.Bluemix_notm}}](/docs/cli?topic=cloud-cli-getting-started), Docker ya está instalado.
- [Instale el plugin de la CLI de {{site.data.keyword.registryshort_notm}} y configure un espacio de nombres](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install).
- Para instalar Tiller con una cuenta de servicio de Kubernetes y el enlace de rol de clúster en el espacio de nombres `kube-system`, asegúrese de tener el [rol `cluster-admin`](/docs/containers?topic=containers-users#access_policies).

Para instalar Tiller mediante {{site.data.keyword.registryshort_notm}}:

1. Instale la <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI de Helm <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> en la máquina local.
2. Conéctese a su clúster privado mediante el túnel VPN de la infraestructura de {{site.data.keyword.Bluemix_notm}} que ha configurado.
3. **Importante**: para mantener la seguridad del clúster, cree una cuenta de servicio para Tiller en el espacio de nombres `kube-system` y un enlace de rol de clúster RBAC de Kubernetes para el pod `tiller-deploy` mediante la aplicación del siguiente archivo YAML del repositorio de [{{site.data.keyword.Bluemix_notm}} `kube-samples`](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml).
    1. [Obtenga la cuenta del servicio de Kubernetes y el archivo YAML de vinculación de roles del clúster ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml).

    2. Cree los recursos de Kubernetes en el clúster.
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [Localice la versión de Tiller ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30) que desea instalar en el clúster. Si no necesita una versión específica, utilice la última.

5. Extraiga la imagen de Tiller del registro de contenedores de Google público a la máquina local.
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   Salida de ejemplo:
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [Envíe la imagen de Tiller al espacio de nombres en {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing).

7. Para acceder a la imagen en {{site.data.keyword.registryshort_notm}} desde dentro del clúster, [copie el secreto de extracción de imágenes del espacio de nombres predeterminado al espacio de nombres `kube-system`](/docs/containers?topic=containers-images#copy_imagePullSecret).

8. Instale Tiller en el clúster privado mediante la imagen que ha almacenado en el espacio de nombres en {{site.data.keyword.registryshort_notm}}.
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. Añada los repositorios de Helm de {{site.data.keyword.Bluemix_notm}} a la instancia de Helm.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

10. Actualice los repositorios para recuperar las versiones más recientes de todos los diagramas de Helm.
    ```
    helm repo update
    ```
    {: pre}

11. Obtenga una lista de los diagramas de Helm disponibles actualmente en los repositorios de {{site.data.keyword.Bluemix_notm}}.
    ```
    helm search iks-charts
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. Identifique el diagrama de Helm que desea instalar y siga las instrucciones del archivo `README` del diagrama de Helm para instalar el diagrama de Helm en el clúster.


## Clústeres privados: Instalación de diagramas de Helm sin utilizar Tiller
{: #private_install_without_tiller}

Si no desea instalar Tiller en el clúster privado, puede crear manualmente los archivos YAML del diagrama de Helm y aplicarlos mediante mandatos `kubectl`.
{: shortdesc}

En los pasos de este ejemplo se muestra cómo instalar diagramas de Helm desde los repositorios de diagramas de Helm de {{site.data.keyword.Bluemix_notm}} en el clúster privado. Si desea instalar un diagrama de Helm que no esté almacenado en uno de los repositorios de diagramas de Helm de {{site.data.keyword.Bluemix_notm}}, debe seguir las instrucciones de este tema para crear los archivos YAML para el diagrama de Helm. Además, debe descargar la imagen del diagrama de Helm del registro de contenedor público, enviarla al espacio de nombres de {{site.data.keyword.registryshort_notm}} y actualizar el archivo `values.yaml` para que utilice la imagen de {{site.data.keyword.registryshort_notm}}.
{: note}

1. Instale la <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI de Helm <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> en la máquina local.
2. Conéctese a su clúster privado mediante el túnel VPN de la infraestructura de {{site.data.keyword.Bluemix_notm}} que ha configurado.
3. Añada los repositorios de Helm de {{site.data.keyword.Bluemix_notm}} a la instancia de Helm.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

4. Actualice los repositorios para recuperar las versiones más recientes de todos los diagramas de Helm.
   ```
   helm repo update
   ```
   {: pre}

5. Obtenga una lista de los diagramas de Helm disponibles actualmente en los repositorios de {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. Identifique el diagrama de Helm que desea instalar, descargue el diagrama de Helm en la máquina local y desempaquete los archivos de su diagrama de Helm. En el ejemplo siguiente se muestra cómo descargar el diagrama de Helm para el programa de escalado automático de clústeres 1.0.3 y desempaquetar los archivos en el directorio `cluster-autoscaler`.
   ```
   helm fetch iks-charts/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Vaya al directorio en el que ha desempaquetado los archivos del diagrama de Helm.
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Cree un directorio `output` para los archivos YAML que genere utilizando los archivos de su diagrama de Helm.
   ```
   mkdir output
   ```
   {: pre}

9. Abra el archivo `values.yaml` y realice los cambios necesarios según las instrucciones de instalación del diagrama de Helm.
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. Utilice la instalación de Helm local para crear todos los archivos YAML de Kubernetes para el diagrama de Helm. Los archivos YAML se almacenan en el directorio `output` que ha creado anteriormente.
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Salida de ejemplo:
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. Despliegue todos los archivos YAML en el clúster privado.
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. Opcional: Elimine todos los archivos YAML del directorio `output`.
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}

## Enlaces de Helm relacionados
{: #helm_links}

Revise los enlaces siguientes para encontrar información adicional de Helm.
{: shortdesc}

* Consulte los diagramas de Helm disponibles que puede utilizar con {{site.data.keyword.containerlong_notm}} en el [catálogo de diagramas de Helm ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/kubernetes/solutions/helm-charts).
* Obtenga más información sobre los mandatos Helm que puede utilizar para configurar y gestionar diagramas de Helm en la <a href="https://docs.helm.sh/helm/" target="_blank">documentación de Helm <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.
* Obtenga más información sobre cómo [aumentar la velocidad del despliegue con diagramas de Helm de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).
