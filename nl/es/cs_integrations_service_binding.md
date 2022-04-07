---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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



# Adición de servicios utilizando enlaces de servicios de IBM Cloud
{: #service-binding}

Añada servicios de {{site.data.keyword.Bluemix_notm}} para mejorar el clúster de Kubernetes con prestaciones adicionales en áreas como, por ejemplo, Watson AI, datos, seguridad e Internet de las cosas (IoT).
{:shortdesc}

**¿Qué tipos de servicios puedo enlazar a mi clúster?** </br>
Cuando añade servicios de {{site.data.keyword.Bluemix_notm}} a su clúster, puede elegir entre servicios habilitados para {{site.data.keyword.Bluemix_notm}} IAM (Identity and Access Management) y servicios basados en Cloud Foundry. Los servicios habilitados para IAM ofrecen un control de acceso más preciso y se pueden gestionar en un grupo de recursos de {{site.data.keyword.Bluemix_notm}}. Los servicios de Cloud Foundry deben añadirse a una organización y un espacio de Cloud Foundry, y no se pueden añadir a un grupo de recursos. Para controlar el acceso a la instancia de servicio de Cloud Foundry, se deban utilizar roles de Cloud Foundry. Para obtener más información acerca de los servicios habilitados para IAM y los servicios de Cloud Foundry, consulte [¿Qué es un recurso?](/docs/resources?topic=resources-resource#resource).

Para obtener una lista de los servicios de {{site.data.keyword.Bluemix_notm}} admitidos, consulte el [catálogo de {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/catalog).

**¿Qué es el enlace de servicios de {{site.data.keyword.Bluemix_notm}}?**</br>
El enlace de servicios es una forma rápida de crear credenciales de servicio para un servicio de {{site.data.keyword.Bluemix_notm}} y almacenar estas credenciales en un secreto de Kubernetes en el clúster. Para enlazar un servicio al clúster, primero debe suministrar una instancia del servicio. A continuación, debe utilizar el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind) `ibmcloud ks cluster-service-bind` para crear las credenciales de servicio y el secreto de Kubernetes. El secreto de Kubernetes se cifra automáticamente en etcd para proteger los datos.

¿Desea proteger aún más sus secretos? Solicite al administrador del clúster que [habilite {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) en el clúster para cifrar los secretos nuevos y existentes, como el secreto que almacena las credenciales de las instancias de servicio de {{site.data.keyword.Bluemix_notm}}.
{: tip}

**¿Puedo utilizar todos los servicios de {{site.data.keyword.Bluemix_notm}} en mi clúster?**</br>
Puede utilizar el enlace de servicios sólo para los servicios que admiten claves de servicio de forma que las credenciales de servicio se puedan crear y almacenar automáticamente en un secreto de Kubernetes. Para consultar una lista de servicios que admiten claves de servicio, consulte [Habilitación de apps externas para utilizar servicios de {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).

Los servicios que no admiten claves de servicio suelen proporcionar una API que puede utilizar en la app. El método de enlace de servicios no configura automáticamente el acceso de API para la app. Asegúrese de revisar la documentación de la API del servicio e implementar la interfaz de API en la app.

## Adición de servicios de IBM Cloud a clústeres
{: #bind-services}

Utilice el enlace de servicios de {{site.data.keyword.Bluemix_notm}} para crear automáticamente credenciales de servicio para los servicios de {{site.data.keyword.Bluemix_notm}} y almacene estas credenciales en un secreto de Kubernetes.
{: shortdesc}

Antes de empezar:
- Asegúrese de que tiene los roles siguientes:
    - [Rol de acceso a la plataforma de {{site.data.keyword.Bluemix_notm}} IAM de **Editor** o **Administrador**](/docs/containers?topic=containers-users#platform) para el clúster donde desea enlazar un servicio
    - [Rol de servicio de {{site.data.keyword.Bluemix_notm}} IAM de **Escritor** o **Gestor**](/docs/containers?topic=containers-users#platform) para el espacio de nombres de Kubernetes donde desea enlazar el servicio
    - Para los servicios Cloud Foundry: [Rol de Cloud Foundry de **Desarrollador**](/docs/iam?topic=iam-mngcf#mngcf) para el espacio donde desea suministrar el servicio
- [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Para añadir un servicio {{site.data.keyword.Bluemix_notm}} al clúster:

1. [Cree una instancia del servicio {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).
    * Algunos servicios de {{site.data.keyword.Bluemix_notm}} solo están disponibles en determinadas regiones. Puede enlazar un servicio con el clúster sólo si el servicio está disponible en la misma región que el clúster. Además, si desea crear una instancia de servicio en la zona de Washington DC, debe utilizar la CLI.
    * **Para los servicios habilitados para IAM**: Debe crear la instancia de servicio en el mismo grupo de recursos que el clúster. Un servicio solo se puede crear en un grupo de recursos y no se puede cambiar después.

2. Compruebe el tipo de servicio que ha creado y tome nota del **Nombre** de la instancia de servicio.
   - **Servicios de Cloud Foundry:**
     ```
     ibmcloud service list
     ```
     {: pre}

     Salida de ejemplo:
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **Servicios habilitados para {{site.data.keyword.Bluemix_notm}} IAM:**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}

     Salida de ejemplo:
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   También puede ver los distintos tipos de servicio en el panel de control de {{site.data.keyword.Bluemix_notm}} como **Servicios de Cloud Foundry** y **Servicios**.

3. Identifique el espacio de nombres del clúster que desea utilizar para añadir el servicio.
   ```
   kubectl get namespaces
   ```
   {: pre}

4. Enlace el servicio a su clúster para crear las credenciales de servicio para el servicio y almacene las credenciales en un secreto de Kubernetes. Si tiene credenciales de servicio existentes, utilice el distintivo `--key` para especificar las credenciales. Para los servicios habilitados para IAM, las credenciales se crean automáticamente con el rol de acceso al servicio de **Escritor**, pero puede utilizar el distintivo `--role` para especificar otro rol de acceso al servicio distinto. Si utiliza el distintivo `--key`, no incluya el distintivo `--role`.
   ```
   ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
   ```
   {: pre}

   Cuando la creación de las credenciales de servicio es satisfactoria, se crea un secreto de Kubernetes con el nombre `binding-<service_instance_name>`.  

   Salida de ejemplo:
   ```
   ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
   ```
   {: screen}

5. Verifique las credenciales de servicio en el secreto de Kubernetes.
   1. Obtenga los detalles del secreto y anote el valor de **binding**. El valor de **binding** está codificado en base64 y contiene las credenciales para la instancia de servicio en formato JSON.
      ```
      kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
      ```
      {: pre}

      Salida de ejemplo:
      ```
      apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
      ```
      {: screen}

   2. Descodifique el valor de binding.
      ```
      echo "<binding>" | base64 -D
      ```
      {: pre}

      Salida de ejemplo:
      ```
      {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

   3. Opcional: compare las credenciales de servicio que ha descodificado en el paso anterior con las credenciales de servicio que encuentre para la instancia de servicio en el panel de control de {{site.data.keyword.Bluemix_notm}}.

6. Ahora que el servicio está enlazado con el clúster, debe configurar la app para que [acceda a las credenciales de servicio en el secreto de Kubernetes](#adding_app).


## Acceso a las credenciales de servicio desde las apps
{: #adding_app}

Para acceder a una instancia de servicio de {{site.data.keyword.Bluemix_notm}} desde la app, debe permitir que la app acceda a las credenciales de servicio almacenadas en el secreto de Kubernetes.
{: shortdesc}

Las credenciales de una instancia de servicio están codificadas como base64 y se almacenan en el secreto en formato JSON. Para acceder a los datos del secreto, elija una de las opciones siguientes:
- [Monte el secreto como un volumen en el pod](#mount_secret)
- [Haga referencia al secreto en las variables de entorno](#reference_secret)
<br>

Antes de empezar:
-  Asegúrese de que tiene el [rol de servicio **Escritor** o **Gestor** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre el espacio de nombres `kube-system`.
- [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- [Añada un servicio de {{site.data.keyword.Bluemix_notm}} a su clúster](#bind-services).

### Montaje del secreto como un volumen en el pod
{: #mount_secret}

Cuando monta el secreto como volumen en el pod, un archivo denominado `binding` se almacena en el directorio de montaje del volumen. El archivo `binding` en formato JSON incluye toda la información y las credenciales que necesita para acceder al servicio de {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

1.  Liste los secretos disponibles en el clúster y anote el **nombre** del secreto. Busque un secreto de tipo **opaco**. Si existen varios secretos, póngase en contacto con el administrador del clúster para identificar el secreto correcto del servicio.
    ```
    kubectl get secrets
    ```
    {: pre}

    Salida de ejemplo:
    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
    ```
    {: screen}

2.  Cree un archivo YAML para el despliegue de Kubernetes y monte el secreto como un volumen en el pod.
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: icr.io/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>Visión general de los componentes del archivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>La vía de acceso absoluta del directorio en el que el que está montado el volumen dentro del contenedor.</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>El nombre del volumen que va a montar en el pod.</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>Los permisos de lectura y escritura en el secreto. Utilice `420` para establecer permisos de solo lectura. </td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>El nombre del secreto que ha anotado en el paso anterior.</td>
    </tr></tbody></table>

3.  Cree el pod y monte el secreto como un volumen.
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  Verifique que se ha creado el pod.
    ```
    kubectl get pods
    ```
    {: pre}

    Ejemplo de salida de CLI:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  Acceda a las credenciales de servicio.
    1. Inicie una sesión en el pod.
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. Vaya a la vía de acceso de montaje de volumen que ha definido anteriormente y liste los archivos en la vía de acceso de montaje del volumen.
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       Salida de ejemplo:
       ```
       binding
       ```
       {: screen}

       El archivo `binding` incluye las credenciales de servicio que ha almacenado en el secreto de Kubernetes.

    4. Consulte las credenciales de servicio. Las credenciales se almacenan como pares de valor de clave en formato JSON.
       ```
       cat binding
       ```
       {: pre}

       Salida de ejemplo:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. Configure la app para analizar el contenido JSON y recuperar la información que necesita para acceder al servicio.

### Cómo hacer referencia al secreto en las variables de entorno
{: #reference_secret}

Puede añadir las credenciales de servicio y otros pares de valores de clave del secreto de Kubernetes como variables de entorno en el despliegue.
{: shortdesc}

1. Liste los secretos disponibles en el clúster y anote el **nombre** del secreto. Busque un secreto de tipo **opaco**. Si existen varios secretos, póngase en contacto con el administrador del clúster para identificar el secreto correcto del servicio.
   ```
   kubectl get secrets
   ```
   {: pre}

   Salida de ejemplo:
   ```
   NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
   ```
   {: screen}

2. Obtenga los detalles del secreto para encontrar pares potenciales de valores de clave a los que pueda hacer referencia como variables de entorno en el pod. Las credenciales de servicio se almacenan en la clave `binding` del secreto.
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   Salida de ejemplo:
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Cree un archivo YAML para el despliegue de Kubernetes y especifique una variable de entorno que haga referencia a la clave `binding`.
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: icr.io/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>Visión general de los componentes del archivo YAML</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>El nombre de la variable de entorno.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>El nombre del secreto que ha anotado en el paso anterior.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>La clave que forma parte de su secreto y a la que desea hacer referencia en la variable de entorno. Para hacer referencia a las credenciales de servicio, debe utilizar la clave <strong>binding</strong>.  </td>
     </tr>
     </tbody></table>

4. Cree el pod que hace referencia a la clave `binding` del secreto como variable de entorno.
   ```
   kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. Verifique que se ha creado el pod.
   ```
   kubectl get pods
   ```
   {: pre}

   Ejemplo de salida de CLI:
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. Compruebe que la variable de entorno se haya establecido correctamente.
   1. Inicie una sesión en el pod.
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. Enumere todas las variables de entorno en el pod.
      ```
      env
      ```
      {: pre}

      Salida de ejemplo:
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. Configure la app para leer la variable de entorno y analizar el contenido JSON para recuperar la información que necesita para acceder al servicio.

   Código de ejemplo en Python:
   ```python
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

8. Opcional: Como precaución, añada manejo de errores a la app por si la variable de entorno `BINDING` no se ha establecido correctamente.

   Código de ejemplo en Java:
   ```java
   if (System.getenv("BINDING") == null) {
    throw new RuntimeException("Environment variable 'SECRET' is not set!");
   }
   ```
   {: codeblock}

   Código de ejemplo en Node.js:
   ```js
   if (!process.env.BINDING) {
    console.error('ENVIRONMENT variable "BINDING" is not set!');
    process.exit(1);
   }
   ```
   {: codeblock}
