---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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


# Despliegue de apps sin servidor con Knative
{: #serverless-apps-knative}

Aprenda a instalar y utilizar Knative en un clúster de Kubernetes en {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

**¿Qué es Knative y por qué debo utilizarlo?**</br>
[Knative](https://github.com/knative/docs) es una plataforma de código abierto que ha sido desarrollada por IBM, Google, Pivotal, Red Hat, Cisco y otros. Su objetivo es ampliar las prestaciones de Kubernetes para ayudarle a crear apps modernas, centradas en el código fuente y sin servidor en la parte superior del clúster de Kubernetes. La plataforma está diseñada para satisfacer las necesidades de los desarrolladores que deben decidir qué tipo de app se debe ejecutar en la nube: apps de 12 factores, contenedores o funciones. Cada tipo de app necesita una solución de código abierto o privada adaptada a estas apps: Cloud Foundry para apps de 12 factores, Kubernetes para contenedores y OpenWhisk y otros para funciones. En el pasado, los desarrolladores tenían que decidir qué enfoque querían seguir, lo que daba lugar a un entorno inflexible y complejo cuando se tenían que combinar diferentes tipos de apps.  

Knative utiliza un enfoque coherente entre lenguajes de programación e infraestructuras para facilitar la carga operativa derivada de crear, desplegar y gestionar cargas de trabajo en Kubernetes de modo que los desarrolladores puedan centrarse en lo que más les importa: el código fuente. Puede utilizar los paquetes de compilación probados con los que ya está familiarizado, como Cloud Foundry, Kaniko, Dockerfile, Bazel y otros. Mediante la integración con Istio, Knative garantiza que las cargas de trabajo sin servidor y contenerizadas se pueden exponer fácilmente en Internet, supervisar y controlar, y que los datos se cifran durante el tránsito.

**¿Cómo funciona Knative?**</br>
Knative viene con tres componentes clave, o _primitivos_, que le ayudan a crear, desplegar y gestionar las apps sin servidor en el clúster de Kubernetes:

- **Build:** el primitivo de compilación, `Build`, da soporte a la creación de un conjunto de pasos para crear la app a partir del código fuente en una imagen de contenedor. Imagine que utiliza una plantilla de compilación sencilla en la que especifique el repositorio de origen para encontrar el código de la app y el registro de contenedor en el que desea alojar la imagen. Con un solo mandato puede indicar a Knative que tome esta plantilla de compilación, extraiga el código fuente, cree la imagen y envíe por push la imagen al registro de contenedor para que pueda utilizar la imagen en el contenedor.
- **Serving:** El primitivo de servicio, `Serving`, le ayuda a desplegar apps sin servidor como servicios Knative y a escalarlos automáticamente, incluso hasta llegar a cero instancias. Para exponer las cargas de trabajo sin servidor y contenerizadas, Knative utiliza Istio. Cuando instala el complemento Knative gestionado, el complemento Istio gestionado también se instala automáticamente. Mediante las funciones de gestión del tráfico y de direccionamiento inteligente de Istio, puede controlar el tráfico que se dirige a una determinada versión del servicio, lo que facilitar al desarrollador la tarea de probar y desplegar una nueva versión de la app o de realizar una prueba de tipo A-B.
- **Eventing:** con el primitivo de gestión de sucesos, `Eventing`, puede crear activadores o corrientes de sucesos a los que se pueden suscribir otros servicios. Por ejemplo, supongamos que desea iniciar una nueva compilación de la app cada vez que se envía por push código a su repositorio maestro de GitHub. O que desea ejecutar una app sin servidor solo si la temperatura cae por debajo del punto de congelación. Por ejemplo, el primitivo `Eventing` se puede integrar en la interconexión CI/CD para automatizar la compilación y el despliegue de apps en caso de que se produzca un suceso específico.

**¿Qué es el complemento Managed Knative on {{site.data.keyword.containerlong_notm}} (experimental)?** </br>
Managed Knative on {{site.data.keyword.containerlong_notm}} es un [complemento gestionado](/docs/containers?topic=containers-managed-addons#managed-addons) que integra Knative e Istio directamente con el clúster de Kubernetes. IBM prueba la versión de Knative y de Istio en el complemento, que se puede utilizar en {{site.data.keyword.containerlong_notm}}. Para obtener más información sobre los complementos gestionados, consulte [Adición de servicios utilizando complementos gestionados](/docs/containers?topic=containers-managed-addons#managed-addons).

**¿Existe alguna limitación?** </br>
Si ha instalado el [controlador de admisiones del gestor de seguridad de imágenes de contenedor](/docs/services/Registry?topic=registry-security_enforce#security_enforce) en el clúster, no puede habilitar el complemento Knative gestionado en el clúster.

## Configuración de Knative en el clúster
{: #knative-setup}

Knative se compila sobre Istio para asegurar que las cargas de trabajo sin servidor y contenerizadas se pueden exponer dentro del clúster y en internet. Con Istio, también puede supervisar y controlar el tráfico de la red entre los servicios y asegurarse de que los datos se cifran durante el tránsito. Cuando instala el complemento Knative gestionado, el complemento Istio gestionado también se instala automáticamente.
{: shortdesc}

Antes de empezar:
-  [Instale la CLI de IBM Cloud, el plugin de {{site.data.keyword.containerlong_notm}} y la CLI de Kubernetes](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps). Asegúrese de instalar una versión de la CLI de `kubectl` que coincida con la versión de Kubernetes de su clúster.
-  [Cree un clúster estándar con al menos 3 nodos trabajadores con 4 núcleos y 16 GB de memoria (`b3c.4x16`) o más cada uno](/docs/containers?topic=containers-clusters#clusters_ui). Además, el clúster y los nodos trabajadores deben ejecutar al menos la versión mínima soportada de Kubernetes, que puede obtener con el mandato `ibmcloud ks addon-versions --addon knative`.
-  Asegúrese de que tiene el [rol de servicio **Escritor** o **Gestor** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre {{site.data.keyword.containerlong_notm}}.
-  [Defina su clúster como destino de la CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
</br>

Para instalar Knative en el clúster:

1. Habilite el complemento Knative gestionado en el clúster. Cuando habilita Knative en el clúster, Istio y todos los componentes de Knative se instalan en el clúster.
   ```
   ibmcloud ks cluster-addon-enable knative --cluster <cluster_name_or_ID> -y
   ```
   {: pre}

   Salida de ejemplo:
   ```
   Enabling add-on knative for cluster knative...
   OK
   ```
   {: screen}

   La instalación de todos los componentes de Knative puede tardar unos minutos en completarse.

2. Verifique que Istio se ha instalado correctamente. Todos los pods correspondientes a los nueve servicios de Istio y al pod para Prometheus deben estar en el estado `Running` (En ejecución).
   ```
   kubectl get pods --namespace istio-system
   ```
   {: pre}

   Salida de ejemplo:
   ```
   NAME                                       READY     STATUS      RESTARTS   AGE
   istio-citadel-748d656b-pj9bw               1/1       Running     0          2m
   istio-egressgateway-6c65d7c98d-l54kg       1/1       Running     0          2m
   istio-galley-65cfbc6fd7-bpnqx              1/1       Running     0          2m
   istio-ingressgateway-f8dd85989-6w6nj       1/1       Running     0          2m
   istio-pilot-5fd885964b-l4df6               2/2       Running     0          2m
   istio-policy-56f4f4cbbd-2z2bk              2/2       Running     0          2m
   istio-sidecar-injector-646655c8cd-rwvsx    1/1       Running     0          2m
   istio-statsd-prom-bridge-7fdbbf769-8k42l   1/1       Running     0          2m
   istio-telemetry-8687d9d745-mwjbf           2/2       Running     0          2m
   prometheus-55c7c698d6-f4drj                1/1       Running     0          2m
   ```
   {: screen}

3. Opcional: si desea utilizar Istio para todas las apps en el espacio de nombres `default`, añada la etiqueta `istio-injection=enabled` al espacio de nombres. Cada pod de app sin servidor debe ejecutar un complemento de proxy de Envoy para que la app se pueda incluir en la red de servicios de Istio. Esta etiqueta permite a Istio modificar automáticamente la especificación de la plantilla de pod en los nuevos despliegues de apps para que los pods se creen con contenedores de complementos de proxy de Envoy.
   ```
   kubectl label namespace default istio-injection=enabled
   ```
   {: pre}

4. Verifique que todos los pods del componente `Serving` de Knative están en el estado `Running`.
   ```
   kubectl get pods --namespace knative-serving
   ```
   {: pre}

   Salida de ejemplo:
   ```
   NAME                          READY     STATUS    RESTARTS   AGE
   activator-598b4b7787-ps7mj    2/2       Running   0          21m
   autoscaler-5cf5cfb4dc-mcc2l   2/2       Running   0          21m
   controller-7fc84c6584-qlzk2   1/1       Running   0          21m
   webhook-7797ffb6bf-wg46v      1/1       Running   0          21m
   ```
   {: screen}

## Utilización de los servicios de Knative para desplegar una app sin servidor
{: #knative-deploy-app}

Después de configurar Knative en el clúster, puede desplegar la app sin servidor como un servicio de Knative.
{: shortdesc}

**¿Qué es un servicio Knative?** </br>
Para desplegar una app con Knative, debe especificar un recurso de `servicio` Knative. Un servicio Knative se gestiona mediante el primitivo `Serving` de Knative y es el responsable de gestionar todo el ciclo de vida de la carga de trabajo. Cuando se crea el servicio, el primitivo `Serving` de Knative crea automáticamente una versión para la app sin servidor y añade esta versión al historial de revisiones del servicio. A la app sin servidor se le asigna un URL público del subdominio de Ingress en el formato `<knative_service_name>.<namespace>.<ingress_subdomain>` que puede utilizar para acceder a la app desde Internet. Además, se asigna un nombre de host privado a la app en el formato `<knative_service_name>.<namespace>.cluster.local` que puede utilizar para acceder a la app desde dentro del clúster.

**¿Qué ocurre de trasfondo cuando cree el servicio Knative?**</br>
Cuando se crea un servicio Knative, la app se despliega automáticamente como un pod de Kubernetes en el clúster y se expone mediante un servicio de Kubernetes. Para asignar el nombre de host público, Knative utiliza el subdominio de Ingress proporcionado por IBM y el certificado TLS. El tráfico de red de entrada se direcciona en función de las reglas de direccionamiento predeterminadas de Ingress proporcionadas por IBM.

**¿Cómo puedo desplegar una nueva versión de mi app?**</br>
Cuando actualiza el servicio Knative, se crea una nueva versión de la app sin servidor. A esta versión se le asignan los mismos nombres de host público y privado que la versión anterior. De forma predeterminada, todo el tráfico de red de entrada se direcciona a la última versión de la app. Sin embargo, también puede especificar el porcentaje de tráfico de red de entrada que desea direccionar a una determinada versión de la app para que pueda realizar las pruebas de tipo A-B. Puede dividir el tráfico de red de entrada entre dos versiones de app, la versión actual de la app y la nueva versión a la que desea desplegar.  

**¿Puedo traer mi propio dominio personalizado y certificado TLS?** </br>
Puede cambiar el mapa de configuración de la pasarela Ingress de Istio y las reglas de direccionamiento de Ingress para que utilicen su nombre de dominio y su certificado TLS cuando asigne un nombre de host a la app sin servidor. Para obtener más información, consulte [Configuración de nombres de dominio y certificados personalizados](#knative-custom-domain-tls).

Para desplegar la app sin servidor como un servicio Knative:

1. Cree un archivo YAML para la primera app [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) sin servidor en Go con Knative. Cuando envía una solicitud a la app de ejemplo, la app lee la variable de entorno `TARGET` y muestra `"Hello ${TARGET}!"`. Si esta variable de entorno está vacía, se devuelve `"Hello World!"`.
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Go Sample v1"
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
    <td>El nombre del servicio Knative.</td>
    </tr>
    <tr>
    <td><code>metadata.namespace</td>
    <td>Opcional: el espacio de nombres de Kubernetes en el que desea desplegar la app como un servicio Knative. De forma predeterminada, todos los servicios se despliegan en el espacio de nombres de Kubernetes <code>default</code>. </td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>El URL al registro de contenedor donde se está guardada la imagen. En este ejemplo, se despliega la app de Knative Hello World que está almacenada en el espacio de nombres <code>ibmcom</code> en Docker Hub. </td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>Opcional: una lista de las variables de entorno que desea que tenga el servicio Knative. En este ejemplo, la app de ejemplo lee el valor de la variable de entorno <code>TARGET</code> y se devuelve cuando el usuario envía una solicitud a la app en el formato <code>"Hello ${TARGET}!"</code>. Si no se proporciona ningún valor, la app de ejemplo devuelve <code> "Hello World!"</code>.  </td>
    </tr>
    </tbody>
    </table>

2. Cree el servicio Knative en el clúster.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   Salida de ejemplo:
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. Verifique que el servicio Knative se ha creado. En la salida de la CLI, puede ver el dominio (**DOMAIN**) público asignado a la app sin servidor. Las columnas **LATESTCREATED** y **LATESTREADY** muestran la versión de la última app que se ha creado y que está desplegada actualmente en el formato `<knative_service_name>-<version>`. La versión que se asigna a la app es un valor de serie aleatorio. En este ejemplo, la versión de la app sin servidor es `rjmwt`. Cuando actualice el servicio, se creará una nueva versión de la app y se le asignará una nueva serie aleatoria.  
   ```
   kubectl get ksvc/kn-helloworld
   ```
   {: pre}

   Salida de ejemplo:
   ```
   NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
      kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-rjmwt   kn-helloworld-rjmwt   True
   ```
   {: screen}

4. Pruebe la app `Hello World` enviando una solicitud al URL público que se ha asignado a su app.
   ```
   curl -v <public_app_url>
   ```
   {: pre}

   Salida de ejemplo:
   ```
   * Rebuilt URL to: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud/
   *   Trying 169.46.XX.XX...
   * TCP_NODELAY set
   * Connected to kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud (169.46.XX.XX) port 80 (#0)
   > GET / HTTP/1.1
   > Host: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud
   > User-Agent: curl/7.54.0
   > Accept: */*
   >
   < HTTP/1.1 200 OK
      < Date: Thu, 21 Mar 2019 01:12:48 GMT
      < Content-Type: text/plain; charset=utf-8
      < Content-Length: 20
      < Connection: keep-alive
      < x-envoy-upstream-service-time: 17
      <
      Hello Go Sample v1!
   * Connection #0 to host kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud left intact
   ```
   {: screen}

5. Obtenga una lista del número de pods que se han creado para el servicio Knative. En el ejemplo de este tema, se despliega un pod que consta de dos contenedores. Un contenedor ejecuta la app `Hello World` y el otro contenedor es un complemento que ejecuta las herramientas de supervisión y registro de Istio y Knative.
   ```
   kubectl get pods
   ```
   {: pre}

   Salida de ejemplo:
   ```
   NAME                                             READY     STATUS    RESTARTS   AGE
   kn-helloworld-rjmwt-deployment-55db6bf4c5-2vftm  2/2      Running   0          16s
   ```
   {: screen}

6. Espere unos minutos para dejar que Knative escala el pod hacia abajo. Knative evalúa el número de pods que deben estar activos a la vez para procesar la carga de trabajo de entrada. Si no se recibe ningún tráfico de red, Knative escala automáticamente sus pods hacia abajo, incluso hasta llegar a cero pods, tal como se muestra en este ejemplo.

   ¿Quiere ver cómo Knative escala sus pods hacia arriba? Intente aumentar la carga de trabajo para la app, utilizando, por ejemplo, herramientas como el [verificador simple de carga basado en la nube](https://loader.io/).
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   Si no ve ningún pod `kn-helloworld`, significa que Knative ha escalado la app hacia abajo hasta llegar a cero pods.

7. Actualice el ejemplo de servicio Knative y especifique otro valor para la variable de entorno `TARGET`.

   Archivo YAML de servicio de ejemplo:
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Mr. Smith"
   ```
   {: codeblock}

8. Aplique el cambio al servicio. Cuando cambia la configuración, Knative crea automáticamente una nueva versión de la app.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

9. Compruebe que se ha desplegado una nueva versión de la app. En la salida de la CLI, puede ver la nueva versión de la app en la columna **LATESTCREATED**. Si ver la misma versión de la app en la columna **LATESTREADY**, significa que la app está configurada y lista para recibir tráfico de red de entrada en el URL público asignado.
   ```
   kubectl get ksvc/kn-helloworld
   ```
   {: pre}

   Salida de ejemplo:
   ```
   NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
   kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-ghyei   kn-helloworld-ghyei   True
   ```
   {: screen}

9. Haga una nueva solicitud a la app para verificar que se ha aplicado el cambio.
   ```
   curl -v <service_domain>
   ```

   Salida de ejemplo:
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

10. Verifique que Knative ha escalado de nuevo el pod hacia arriba para dar respuesta al aumento del tráfico de red.
    ```
    kubectl get pods
    ```

    Salida de ejemplo:
    ```
    NAME                                              READY     STATUS    RESTARTS   AGE
    kn-helloworld-ghyei-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
    ```
    {: screen}

11. Opcional: Limpie el servicio Knative.
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}


## Configuración de nombres de dominio y certificados personalizados
{: #knative-custom-domain-tls}

Puede configurar Knative de modo que asigne nombres de host desde su propio dominio personalizado que ha configurado con TLS.
{: shortdesc}

De forma predeterminada, a cada app se le asigna un subdominio público desde el subdominio de Ingress en el formato `<knative_service_name>.<namespace>.<ingress_subdomain>` que puede utilizar para acceder a la app desde Internet. Además, se asigna un nombre de host privado a la app en el formato `<knative_service_name>.<namespace>.cluster.local` que puede utilizar para acceder a la app desde dentro del clúster. Si desea asignar nombres de host desde un dominio personalizado de su propiedad, puede cambiar el mapa de configuración de Knative para que utilice el dominio personalizado.

1. Cree un dominio personalizado. Para registrar un dominio personalizado, póngase en contacto con su proveedor de DNS (Domain Name Service) o con [IBM Cloud DNS](/docs/infrastructure/dns?topic=dns-getting-started).
2. Configure el dominio de modo que direccione el tráfico de red de entrada a la pasarela de Ingress proporcionado por IBM. Puede elegir entre las siguientes opciones:
   - Defina un alias para el dominio personalizado especificando el dominio proporcionado por IBM como CNAME (Registro de nombre canónico). Para encontrar el dominio de Ingress proporcionado por IBM, ejecute `ibmcloud ks cluster-get --cluster <cluster_name>` y busque el campo **Subdominio de Ingress**. Se prefiere el uso de un CNAME porque IBM proporciona comprobaciones de estado automáticas en el subdominio de IBM y elimina cualquier IP anómala de la respuesta de DNS.
   - Correlacione el dominio personalizado con la dirección IP pública portátil de la pasarela de Ingress añadiendo la dirección IP como un registro. Para localizar la dirección IP pública de la pasarela de Ingress, ejecute `nslookup <ingress_subdomain>`.
3. Adquiera un certificado TLS comodín oficial para su dominio personalizado. Si desea adquirir varios certificados TLS, asegúrese de que el [CN ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://support.dnsimple.com/articles/what-is-common-name/) sea distinto para cada certificado.
4. Cree un secreto de Kubernetes para el certificado y para la clave.
   1. Codifique el cert y la clave en la base 64 y guarde el valor codificado en base 64 en un nuevo archivo.
      ```
      openssl base64 -in tls.key -out tls.key.base64
      ```
      {: pre}

   2. Vea el valor codificado en base 64 para el cert y la clave.
      ```
      cat tls.key.base64
      ```
      {: pre}

   3. Cree un archivo YAML secreto utilizando el certificado y la clave.
      ```
      apiVersion: v1
      kind: Secret
      metadata:
        name: mydomain-ssl
      type: Opaque
      data:
        tls.crt: <client_certificate>
        tls.key: <client_key>
      ```
      {: codeblock}

   4. Cree el certificado en el clúster.
      ```
      kubectl create -f secret.yaml
      ```
      {: pre}

5. Abra el recurso Ingress `iks-knative-ingress` en el espacio de nombres `istio-system` de su clúster para empezar a editarlo.
   ```
   kubectl edit ingress iks-knative-ingress -n istio-system
   ```
   {: pre}

6. Cambie las reglas de direccionamiento predeterminadas de Ingress.
   - Añada el dominio comodín personalizado a la sección `spec.rules.host` para que todo el tráfico de red de entrada procedente del dominio personalizado y de cualquier subdominio se direccione a `istio-ingressgateway`.
   - Configure todos los hosts del dominio comodín personalizado de modo que utilicen el secreto TLS que ha creado anteriormente en la sección `spec.tls.hosts`.

   Ejemplo de Ingress:
   ```
   apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
     name: iks-knative-ingress
     namespace: istio-system
   spec:
     rules:
     - host: '*.mydomain'
       http:
         paths:
         - backend:
             serviceName: istio-ingressgateway
             servicePort: 80
           path: /
     tls:
     - hosts:
       - '*.mydomain'
       secretName: mydomain-ssl
   ```
   {: codeblock}

   Las secciones `spec.rules.host` y `spec.tls.hosts` son listas y pueden incluir varios dominios personalizados y certificados de TLS.
   {: tip}

7. Modifique el mapa de configuración `config-domain` de Knative para que utilice el dominio personalizado para asignar nombres de host a los nuevos servicios de Knative.
   1. Abra el mapa de configuración `config-domain` para empezar a editarlo.
      ```
      kubectl edit configmap config-domain -n knative-serving
      ```
      {: pre}

   2. Especifique el dominio personalizado en la sección `data` del mapa de correlación de configuración y elimine el dominio predeterminado que se ha establecido para el clúster.
      - **Ejemplo para asignar un nombre de host desde el dominio personalizado para todos los servicios de Knative**:
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        Si añade `""` al dominio personalizado, a todos los servicios de Knative que cree se les asigna un nombre de host desde el dominio personalizado.  

      - **Ejemplo para asignar un nombre de host desde el dominio personalizado para servicios seleccionados de Knative**:
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: |
            selector:
              app: sample
          mycluster.us-south.containers.appdomain.cloud: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        Para asignar un nombre de host desde el dominio personalizado solo para servicios seleccionados de Knative, añada una clave de etiqueta `data.selector` y un valor al mapa de configuración. En este ejemplo, a todos los servicios con la etiqueta `app: sample` se les asigna un nombre de host desde el dominio personalizado. Asegúrese de que también tiene un nombre de dominio que desea asignar a todas las demás apps que no tienen la etiqueta `app: sample`. En este ejemplo, se utiliza el dominio proporcionado por IBM predeterminado `mycluster.us-south.containers.appdomain.cloud`.
    3. Guarde los cambios.

Con las reglas de direccionamiento de Ingress y los mapas de configuración de Knative configurados, puede crear servicios de Knative con el dominio personalizado y el certificado TLS.

## Acceso a un servicio Knative desde otro servicio Knative
{: #knative-access-service}

Puede acceder al servicio Knative desde otro servicio Knative mediante una llamada de API REST al URL asignado al servicio Knative.
{: shortdesc}

1. Obtenga una lista de todos los servicios Knative del clúster.
   ```
   kubectl get ksvc --all-namespaces
   ```
   {: pre}

2. Recupere el valor **DOMAIN** asignado a su servicio Knative.
   ```
   kubect get ksvc/<knative_service_name>
   ```
   {: pre}

   Salida de ejemplo:
   ```
   NAME        DOMAIN                                                            LATESTCREATED     LATESTREADY       READY   REASON
   myservice   myservice.default.mycluster.us-south.containers.appdomain.cloud   myservice-rjmwt   myservice-rjmwt   True
   ```
   {: screen}

3. Utilice el nombre de dominio para implementar una llamada a la API REST para acceder a su servicio Knative. Esta llamada a la API REST debe formar parte de la app para la cual crea un servicio Knative. Si el servicio de Knative al que desea acceder tiene asignado un URL local en el formato `<service_name>.<namespace>.svc.cluster.local`, Knative mantiene la solicitud de API REST dentro de la red interna del clúster.

   Fragmento de código de ejemplo en Go:
   ```go
   resp, err := http.Get("<knative_service_domain_name>")
   if err != nil {
       fmt.Fprintf(os.Stderr, "Error: %s\n", err)
   } else if resp.Body != nil {
       body, _ := ioutil.ReadAll(resp.Body)
       fmt.Printf("Response: %s\n", string(body))
   }
   ```
   {: codeblock}

## Valores comunes del servicio Knative
{: #knative-service-settings}

Revise los valores comunes del servicio Knative, que le pueden resultar útiles cuando desarrolle su app sin servidor.
{: shortdesc}

- [Establecimiento del número mínimo y máximo de pods](#knative-min-max-pods)
- [Especificación del número máximo de solicitudes por pod](#max-request-per-pod)
- [Creación de apps sin servidor solo privadas](#knative-private-only)
- [Cómo obligar al servicio Knative a que vuelva a extraer una imagen de contenedor](#knative-repull-image)

### Establecimiento del número mínimo y máximo de pods
{: #knative-min-max-pods}

Puede especificar el número mínimo y máximo de pods que desea ejecutar para las apps utilizando una anotación. Por ejemplo, si no desea que Knative reduzca la app a cero instancias, establezca el número mínimo de pods en 1.
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: ...
spec:
  runLatest:
    configuration:
      revisionTemplate:
        metadata:
          annotations:
            autoscaling.knative.dev/minScale: "1"
            autoscaling.knative.dev/maxScale: "100"
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>Visión general de los componentes del archivo YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
</thead>
<tbody>
<tr>
<td><code>autoscaling.knative.dev/minScale</code></td>
<td>Especifique el número mínimo de pods que desea ejecutar en el clúster. Knative no puede reducir la app por debajo del número que establezca, aunque la app no reciba tráfico de red. El valor predeterminado de pods es cero.  </td>
</tr>
<tr>
<td><code>autoscaling.knative.dev/maxScale</code></td>
<td>Especifique el número máximo de pods que desea ejecutar en el clúster. Knative no puede aumentar la app por encima del número que establezca, aunque tenga más solicitudes de las que pueden manejar las instancias actuales de la app.</td>
</tr>
</tbody>
</table>

### Especificación del número máximo de solicitudes por pod
{: #max-request-per-pod}

Puede especificar el número máximo de solicitudes que una instancia de app puede recibir y procesar antes de que Knative considere la posibilidad de aumentar el número de instancias de la app. Por ejemplo, si establece el número máximo de solicitudes en 1, la instancia de la app puede recibir solicitudes de una en una. Si llega una segunda solicitud antes de que la primera se haya procesado por completo, Knative aumenta en otra instancia.

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image: myimage
          containerConcurrency: 1
....
```
{: codeblock}

<table>
<caption>Visión general de los componentes del archivo YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
</thead>
<tbody>
<tr>
<td><code>containerConcurrency</code></td>
<td>Especifique el número máximo de solicitudes que una instancia de app puede recibir simultáneamente antes de que Knative considere la posibilidad de aumentar el número de instancias de la app.  </td>
</tr>
</tbody>
</table>

### Creación de apps sin servidor solo privadas
{: #knative-private-only}

De forma predeterminada, a cada servicio Knative se le asigna una ruta pública desde el subdominio de Ingress de Istio y una ruta privada con el formato `<service_name>.<namespace>.cluster.local`. Puede utilizar la ruta pública para acceder a la app desde la red pública. Si desea mantener el servicio privado, puede añadir la etiqueta `serving.knative.dev/visibility` a su servicio Knative. Esta etiqueta indica a Knative que asigne un nombre de host privado solo a su servicio.
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
  labels:
    serving.knative.dev/visibility: cluster-local
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>Visión general de los componentes del archivo YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
</thead>
<tbody>
<tr>
<td><code>serving.knative.dev/visibility</code></td>
  <td>Si añade la etiqueta <code>serving.knative.dev/visibility: cluster-local</code>, a su servicio solo se le asigna una ruta privada con el formato <code>&lt;service_name&gt;.&lt;namespace&gt;.cluster.local</code>. Puede utilizar el nombre de host privado para acceder a su servicio desde dentro del clúster, pero no puede acceder a su servicio desde la red pública.  </td>
</tr>
</tbody>
</table>

### Cómo obligar al servicio Knative a que vuelva a extraer una imagen de contenedor
{: #knative-repull-image}

La implementación actual de Knative no proporciona una forma estándar de obligar al componente `Serving` de Knative a que vuelva a extraer una imagen de contenedor. Para que vuelva a extraer una imagen del registro, elija entre las opciones siguientes:

- **Modifique el valor `revisionTemplate`** del servicio Knative: el valor `revisionTemplate` de un servicio Knative sirve para crear una revisión del servicio Knative. Si modifica esta plantilla de revisión y, por ejemplo, añade la anotación `repullFlag`, Knative debe crear una nueva revisión para la app. Como parte de la creación de la revisión, Knative debe comprobar si hay actualizaciones de la imagen del contenedor. Si establece `imagePullPolicy: Always`, Knative no puede utilizar la memoria caché de imagen del clúster, sino que debe extraer la imagen del registro del contenedor.
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: <service_name>
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           metadata:
             annotations:
               repullFlag: 123
           spec:
             container:
               image: <image_name>
               imagePullPolicy: Always
    ```
    {: codeblock}

    Debe cambiar el valor de `repullFlag` cada vez que desee crear una nueva revisión del servicio que extraiga la versión de imagen más reciente del registro de contenedor. Asegúrese de utilizar un valor exclusivo para cada revisión para evitar que Knative utilice una versión de imagen antigua debido a la existencia de dos configuraciones de servicio de Knative idénticas.  
    {: note}

- **Utilice códigos para crear imágenes de contenedor exclusivas**: puede utilizar códigos exclusivos para cada imagen de contenedor que cree y hacer referencia a esta imagen en la configuración de `container.image` del servicio Knative. En el ejemplo siguiente, se utiliza `v1` como código de imagen. Para obligar a Knative a extraer una nueva imagen del registro de contenedor, debe cambiar el código de la imagen. Por ejemplo, utilice `v2` como nuevo código de imagen.
  ```
  apiVersion: serving.knative.dev/v1alpha1
  kind: Service
  metadata:
    name: <service_name>
  spec:
    runLatest:
      configuration:
          spec:
            container:
              image: myapp:v1
              imagePullPolicy: Always
    ```
    {: codeblock}


## Enlaces relacionados  
{: #knative-related-links}

- Pruebe este [taller de Knative ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/IBM/knative101/tree/master/workshop) para desplegar la primera app fibonacci de `Node.js` en el clúster.
  - Explore cómo utilizar el primitivo `Build` de Knative para crear una imagen a partir de un Dockerfile en GitHub y enviar la imagen por push automáticamente al espacio de nombres en {{site.data.keyword.registrylong_notm}}.  
  - Aprenda a configurar el direccionamiento para el tráfico de red desde el subdominio de Ingress proporcionado por IBM a la pasarela de Ingress de Istio proporcionada por Knative.
  - Despliegue una nueva versión de la app y utilice Istio para controlar la cantidad de tráfico que se direcciona a cada versión de la app.
- Explore los ejemplos de [`Eventing` de Knative ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/knative/docs/tree/master/eventing/samples).
- Obtenga más información sobre Knative con la documentación de [Knative ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/knative/docs).
