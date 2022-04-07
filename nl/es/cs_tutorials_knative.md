---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

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


# Guía de aprendizaje: Utilización de Knative gestionado para ejecutar apps sin servidor en clústeres de Kubernetes
{: #knative_tutorial}

Con esta guía de aprendizaje, puede aprender cómo instalar Knative en un clúster de Kubernetes en {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

**¿Qué es Knative y por qué debo utilizarlo?**</br>
[Knative](https://github.com/knative/docs) es una plataforma de código abierto que ha sido desarrollada por IBM, Google, Pivotal, Red Hat, Cisco y otros con el objetivo de ampliar las prestaciones de Kubernetes para ayudarle a crear apps modernas, centradas en contenedores y sin servidor sobre su clúster de Kubernetes. La plataforma está diseñada para satisfacer las necesidades de los desarrolladores que deben decidir qué tipo de app se debe ejecutar en la nube: apps de 12 factores, contenedores o funciones. Cada tipo de app necesita una solución de código abierto o privada adaptada a estas apps: Cloud Foundry para apps de 12 factores, Kubernetes para contenedores y OpenWhisk y otros para funciones. En el pasado, los desarrolladores tenían que decidir qué enfoque querían seguir, lo que daba lugar a un entorno inflexible y complejo cuando se tenían que combinar diferentes tipos de apps.  

Knative utiliza un enfoque coherente entre lenguajes de programación e infraestructuras para facilitar la carga operativa derivada de crear, desplegar y gestionar cargas de trabajo en Kubernetes de modo que los desarrolladores puedan centrarse en lo que más les importa: el código fuente. Puede utilizar los paquetes de compilación probados con los que ya está familiarizado, como Cloud Foundry, Kaniko, Dockerfile, Bazel y otros. Mediante la integración con Istio, Knative garantiza que las cargas de trabajo sin servidor y contenerizadas se pueden exponer fácilmente en Internet, supervisar y controlar, y que los datos se cifran durante el tránsito.

**¿Cómo funciona Knative?**</br>
Knative viene con 3 componentes clave, o _primitivos_, que le ayudan a crear, desplegar y gestionar las apps sin servidor en el clúster de Kubernetes:

- **Build:** el primitivo de compilación, `Build`, da soporte a la creación de un conjunto de pasos para crear la app a partir del código fuente en una imagen de contenedor. Imagine que utiliza una plantilla de compilación sencilla en la que especifique el repositorio de origen para encontrar el código de la app y el registro de contenedor en el que desea alojar la imagen. Con un solo mandato puede indicar a Knative que tome esta plantilla de compilación, extraiga el código fuente, cree la imagen y envíe por push la imagen al registro de contenedor para que pueda utilizar la imagen en el contenedor.
- **Serving:** el primitivo de servicio, `Serving`, le ayuda a desplegar apps sin servidor como servicios Knative y a escalarlos automáticamente, incluso hasta llegar a cero instancias. Mediante las funciones de gestión del tráfico y de direccionamiento inteligente de Istio, puede controlar el tráfico que se dirige a una determinada versión del servicio, lo que facilitar al desarrollador la tarea de probar y desplegar una nueva versión de la app o de realizar una prueba de tipo A-B.
- **Eventing:** con el primitivo de gestión de sucesos, `Eventing`, puede crear activadores o corrientes de sucesos a los que se pueden suscribir otros servicios. Por ejemplo, supongamos que desea iniciar una nueva compilación de la app cada vez que se envía por push código a su repositorio maestro de GitHub. O que desea ejecutar una app sin servidor solo si la temperatura cae por debajo del punto de congelación. El primitivo `Eventing` se puede integrar en la interconexión CI/CD para automatizar la compilación y el despliegue de apps en caso de que se produzca un suceso específico.

**¿Qué es el complemento Managed Knative on {{site.data.keyword.containerlong_notm}} (experimental)?** </br>
Managed Knative on {{site.data.keyword.containerlong_notm}} es un complemento gestionado que integra Knative e Istio directamente con el clúster de Kubernetes. IBM prueba la versión de Knative e Istio en el complemento, que se puede utilizar en {{site.data.keyword.containerlong_notm}}. Para obtener más información sobre los complementos gestionados, consulte [Adición de servicios utilizando complementos gestionados](/docs/containers?topic=containers-managed-addons#managed-addons).

**¿Existe alguna limitación?** </br>
Si ha instalado el [controlador de admisiones del gestor de seguridad de imágenes de contenedor](/docs/services/Registry?topic=registry-security_enforce#security_enforce) en el clúster, no puede habilitar el complemento Knative gestionado en el clúster.

¿Suena bien? Siga esta guía de aprendizaje para empezar a empezar a utilizar Knative en {{site.data.keyword.containerlong_notm}}.

## Objetivos
{: #knative_objectives}

- Aprender los conceptos básicos de Knative y de los primitivos de Knative.  
- Instalar el complemento Knative gestionado y el complemento Istio gestionado en el clúster.
- Desplegar la primera app sin servidor con Knative y exponer la app en Internet mediante el primitivo `Serving` de Knative.
- Explorar las prestaciones de escalado y de revisión de Knative.

## Tiempo necesario
{: #knative_time}

30
minutos

## Público
{: #knative_audience}

Esta guía de aprendizaje está pensada para los desarrolladores que están interesados en aprender a utilizar Knative para desplegar una app sin servidor en un clúster de Kubernetes y para los administradores de clústeres que deseen aprender a configurar Knative en un clúster.

## Requisitos previos
{: #knative_prerequisites}

-  [Instale la CLI de IBM Cloud, el plugin de {{site.data.keyword.containerlong_notm}} y la CLI de Kubernetes](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps). Asegúrese de instalar una versión de la CLI de `kubectl` que coincida con la versión de Kubernetes de su clúster.
-  [Cree un clúster con al menos 3 nodos trabajadores con 4 núcleos y 16 GB de memoria (`b3c.4x16`) o más cada uno](/docs/containers?topic=containers-clusters#clusters_cli). Todos los nodos trabajadores deben ejecutar Kubernetes versión 1.12 o superior.
-  Asegúrese de que tiene el [rol de servicio **Escritor** o **Gestor** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre {{site.data.keyword.containerlong_notm}}.
-  [Defina su clúster como destino de la CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

## Lección 1: Configurar el complemento Knative gestionado
{: #knative_setup}

Knative se compila sobre Istio para asegurar que las cargas de trabajo sin servidor y contenerizadas se pueden exponer dentro del clúster y en internet. Con Istio, también puede supervisar y controlar el tráfico de la red entre los servicios y asegurarse de que los datos se cifran durante el tránsito. Cuando instala el complemento Knative gestionado, el complemento Istio gestionado también se instala automáticamente.
{: shortdesc}

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

2. Verifique que Istio se ha instalado correctamente. Todos los pods correspondientes a los 9 servicios de Istio y al pod para Prometheus deben estar en el estado `Running` (En ejecución).
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

4. Verifique que todos los componentes de Knative se han instalado correctamente.
   1. Verifique que todos los pods del componente `Serving` de Knative están en el estado `Running`.  
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

   2. Verifique que todos los pods del componente `Build` de Knative están en el estado `Running`.  
      ```
      kubectl get pods --namespace knative-build
      ```
      {: pre}

      Salida de ejemplo:
      ```
      NAME                                READY     STATUS    RESTARTS   AGE
      build-controller-79cb969d89-kdn2b   1/1       Running   0          21m
      build-webhook-58d685fc58-twwc4      1/1       Running   0          21m
      ```
      {: screen}

   3. Verifique que todos los pods del componente `Eventing` de Knative están en el estado `Running`.
      ```
      kubectl get pods --namespace knative-eventing
      ```
      {: pre}

      Salida de ejemplo:

      ```
      NAME                                            READY     STATUS    RESTARTS   AGE
      eventing-controller-847d8cf969-kxjtm            1/1       Running   0          22m
      in-memory-channel-controller-59dd7cfb5b-846mn   1/1       Running   0          22m
      in-memory-channel-dispatcher-67f7497fc-dgbrb    2/2       Running   1          22m
      webhook-7cfff8d86d-vjnqq                        1/1       Running   0          22m
      ```
      {: screen}

   4. Verifique que todos los pods del componente `Sources` de Knative están en el estado `Running`.
      ```
      kubectl get pods --namespace knative-sources
      ```
      {: pre}

      Salida de ejemplo:
      ```
      NAME                   READY     STATUS    RESTARTS   AGE
      controller-manager-0   1/1       Running   0          22m
      ```
      {: screen}

   5. Verifique que todos los pods del componente `Monitoring` de Knative están en el estado `Running`.
      ```
      kubectl get pods --namespace knative-monitoring
      ```
      {: pre}

      Salida de ejemplo:
      ```
      NAME                                  READY     STATUS                 RESTARTS   AGE
      elasticsearch-logging-0               1/1       Running                0          22m
      elasticsearch-logging-1               1/1       Running                0          21m
      grafana-79cf95cc7-mw42v               1/1       Running                0          21m
      kibana-logging-7f7b9698bc-m7v6r       1/1       Running                0          22m
      kube-state-metrics-768dfff9c5-fmkkr   4/4       Running                0          21m
      node-exporter-dzlp9                   2/2       Running                0          22m
      node-exporter-hp6gv                   2/2       Running                0          22m
      node-exporter-hr6vs                   2/2       Running                0          22m
      prometheus-system-0                   1/1       Running                0          21m
      prometheus-system-1                   1/1       Running                0          21m
      ```
      {: screen}

¡Estupendo! Con Knative e Istio configurados, ya puede desplegar la primera app sin servidor en el clúster.

## Lección 2: Desplegar una app sin servidor en el clúster
{: #deploy_app}

En esta lección, desplegará la primera app [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) sin servidor en Go. Cuando envía una solicitud a la app de ejemplo, la app lee la variable de entorno `TARGET` y muestra `"Hello ${TARGET}!"`. Si esta variable de entorno está vacía, se devuelve `"Hello World!"`.
{: shortdesc}

1. Cree un archivo YAML para la primera app `Hello World` sin servidor en Knative. Para desplegar una app con Knative, debe especificar un recurso de servicio Knative. Un servicio se gestiona mediante el primitivo `Serving` de Knative y es el responsable de gestionar todo el ciclo de vida de la carga de trabajo. El servicio garantiza que cada despliegue tenga una revisión de Knative, una ruta y una configuración. Cuando actualiza el servicio, se crea una nueva versión de la app y se añade al historial de revisiones del servicio. Las rutas de Knative garantizan que cada revisión de la app se correlaciona con un punto final de la red para que pueda controlar la cantidad de tráfico de red que se direcciona a una determinada revisión. Las configuraciones de Knative contienen los valores de una determinada revisión de modo que siempre pueda retrotraer a una revisión más antigua o cambiar de revisión. Para obtener más información sobre los recursos `Serving` de Knative, consulte la [documentación de Knative](https://github.com/knative/docs/tree/master/serving).
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
    <td>El espacio de nombres de Kubernetes en el que desea desplegar la app como un servicio Knative. </td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>El URL al registro de contenedor donde se está guardada la imagen. En este ejemplo, se despliega la app de Knative Hello World que está almacenada en el espacio de nombres <code>ibmcom</code> en Docker Hub. </td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>Una lista de las variables de entorno que desea que tenga el servicio Knative. En este ejemplo, la app de ejemplo lee el valor de la variable de entorno <code>TARGET</code> y se devuelve cuando el usuario envía una solicitud a la app en el formato <code>"Hello ${TARGET}!"</code>. Si no se proporciona ningún valor, la app de ejemplo devuelve <code> "Hello World!"</code>.  </td>
    </tr>
    </tbody>
    </table>

2. Cree el servicio Knative en el clúster. Cuando se crea el servicio, el primitivo `Serving` de Knative crea una revisión inmutable, una ruta de Knative, una regla de direccionamiento de Ingress, un servicio de Kubernetes, un pod de Kubernetes y un equilibrador de carga para la app. A la app se le asigna un subdominio del subdominio de Ingress en el formato `<knative_service_name>.<namespace>.<ingress_subdomain>` que puede utilizar para acceder a la app desde Internet.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   Salida de ejemplo:
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. Verifique que se ha creado el pod. Su pod consta de dos contenedores. Un contenedor ejecuta la app `Hello World` y el otro contenedor es un complemento que ejecuta las herramientas de supervisión y registro de Istio y Knative. A su pod se le asigna el número de revisión `00001`.
   ```
   kubectl get pods
   ```
   {: pre}

   Salida de ejemplo:
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00001-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
   ```
   {: screen}

4. Pruebe la app `Hello World`.
   1. Obtenga el dominio predeterminado que se ha asignado a su servicio Knative. Si ha cambiado el nombre del servicio Knative, o si ha desplegado la app en otro espacio de nombres, actualice estos valores en la consulta.
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

   2. Haga una solicitud a la app utilizando el subdominio que ha recuperado en el paso anterior.
      ```
      curl -v <service_domain>
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

5. Espere unos minutos para dejar que Knative escala el pod hacia abajo. Knative evalúa el número de pods que deben estar activos a la vez para procesar la carga de trabajo de entrada. Si no se recibe ningún tráfico de red, Knative escala automáticamente sus pods hacia abajo, incluso hasta llegar a cero pods, tal como se muestra en este ejemplo.

   ¿Quiere ver cómo Knative escala sus pods hacia arriba? Intente aumentar la carga de trabajo para la app, utilizando, por ejemplo, herramientas como el [verificador simple de carga basado en la nube](https://loader.io/).
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   Si no ve ningún pod `kn-helloworld`, significa que Knative ha escalado la app hacia abajo hasta llegar a cero pods.

6. Actualice el ejemplo de servicio Knative y especifique otro valor para la variable de entorno `TARGET`.

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

7. Aplique el cambio al servicio. Cuando cambia la configuración, Knative crea automáticamente una nueva revisión, asigna una nueva ruta y, de forma predeterminada, indica a Istio que direccione el tráfico de red entrante a la última revisión.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

8. Haga una nueva solicitud a la app para verificar que se ha aplicado el cambio.
   ```
   curl -v <service_domain>
   ```

   Salida de ejemplo:
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

9. Verifique que Knative ha escalado de nuevo el pod hacia arriba para dar respuesta al aumento del tráfico de red. A su pod se le asigna el número de revisión `00002`. Puede utilizar el número de revisión para hacer referencia a una determinada versión de la app, por ejemplo cuando desee dar instrucciones a Istio para que divida el tráfico entrante entre las dos revisiones.
   ```
   kubectl get pods
   ```

   Salida de ejemplo:
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00002-deployment-55db6bf4c5-2vftm   3/3       Running   0          16s
   ```
   {: screen}

10. Opcional: Limpie el servicio Knative.
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}

¡Impresionante! Ha desplegado correctamente la primera app de Knative en el clúster y ha explorado las posibilidades de revisión y de escalado del primitivo `Serving` de Knative.


## ¿Qué es lo siguiente?   
{: #whats-next}

- Pruebe este [taller de Knative ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/IBM/knative101/tree/master/workshop) para desplegar la primera app fibonacci de `Node.js` en el clúster.
  - Explore cómo utilizar el primitivo `Build` de Knative para crear una imagen a partir de un Dockerfile en GitHub y enviar la imagen por push automáticamente al espacio de nombres en {{site.data.keyword.registrylong_notm}}.  
  - Aprenda a configurar el direccionamiento para el tráfico de red desde el subdominio de Ingress proporcionado por IBM a la pasarela de Ingress de Istio proporcionada por Knative.
  - Despliegue una nueva versión de la app y utilice Istio para controlar la cantidad de tráfico que se direcciona a cada versión de la app.
- Explore los ejemplos de [`Eventing` de Knative ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/knative/docs/tree/master/eventing/samples).
- Obtenga más información sobre Knative con la documentación de [Knative ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/knative/docs).
