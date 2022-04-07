---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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



# Utilización del complemento Istio gestionado (beta)
{: #istio}

Istio on {{site.data.keyword.containerlong}} facilita la instalación de Istio, las actualizaciones automáticas y la gestión del ciclo de vida de los componentes de plano de control de Istio, además de la integración con las herramientas de registro y supervisión de la plataforma.
{: shortdesc}

Con una sola pulsación, puede obtener todos los componentes principales de Istio, rastreo adicional, supervisión y visualización, y tener la app de ejemplo BookInfo activa y en ejecución. Istio on {{site.data.keyword.containerlong_notm}} se ofrece como un complemento gestionado, por lo que {{site.data.keyword.Bluemix_notm}} mantiene actualizados todos los componentes de Istio automáticamente.

## Visión general de Istio on {{site.data.keyword.containerlong_notm}}
{: #istio_ov}

### ¿Qué es Istio?
{: #istio_ov_what_is}

[Istio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/info/istio) es una plataforma de red de servicios abierta para conectar, proteger, controlar y observar microservicios en plataformas de nube, como Kubernetes en {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Cuando pasa de aplicaciones monolíticas a una arquitectura de microservicios distribuidos, se debe enfrentar a una serie de retos, como por ejemplo cómo controlar el tráfico de los microservicios, iniciar aplicaciones y probar servicios, gestionar errores, proteger la comunicación de los servicios, observar los servicios e imponer políticas de acceso coherentes en toda la gama de servicios. Para resolver estas dificultades, puede aprovechar una red de servicios. Una red de servicio proporciona una red transparente e independiente del lenguaje para conectar, observar, proteger y controlar la conectividad entre los microservicios. Istio proporciona información y control sobre la red de servicios para que pueda gestionar el tráfico de red, equilibrar la carga entre microservicios, aplicar políticas de acceso, verificar identidades de servicio, etc.

Por ejemplo, el uso de Istio en la red de microservicios puede ayudarle a:
- Conseguir una mejor visibilidad de las apps que se ejecutan en el clúster
- Desplegar versiones de prueba de apps y controlar el tráfico que se les envía
- Habilitar el cifrado automático de datos que se transfieren entre los microservicios
- Imponer limitaciones de velocidad y políticas de listas blancas y listas negras basadas en atributos

Una red de servicio de Istio se compone de un plano de datos y de un plano de control. El plano de datos consta de emisarios de proxy Envoy en cada pod de app, que median en la comunicación entre los microservicios. El plano de control consta de Pilot, telemetría y política de Mixer y Citadel, que aplican las configuraciones de Istio en el clúster. Para obtener más información sobre cada uno de estos componentes, consulte la [descripción de complementos de `istio`](#istio_components).

### ¿Qué es Istio on {{site.data.keyword.containerlong_notm}} (beta)?
{: #istio_ov_addon}

Istio on {{site.data.keyword.containerlong_notm}} se ofrece como un complemento gestionado que integra Istio directamente con el clúster de Kubernetes.
{: shortdesc}

El complemento de Istio gestionado se clasifica como beta y puede ser inestable o cambiar con frecuencia. Las características beta también pueden no proporcionar el mismo nivel de rendimiento o compatibilidad que proporcionan las características con disponibilidad general, y no están pensadas para su uso en un entorno de producción.
{: note}

**¿Qué aspecto tiene esto en mi clúster?**</br>
Cuando instala el complemento de Istio, los planos de control y de datos de Istio utilizan las VLAN a las que ya está conectado el clúster. El tráfico de configuración fluye a través de la red privada dentro del clúster, y no requiere que se abran puertos o direcciones IP adicionales en el cortafuegos. Si expone sus apps gestionadas por Istio con una pasarela de Istio, las solicitudes de tráfico externo destinadas a las apps fluyen a través de la VLAN pública.

**¿Cómo funciona el proceso de actualización?**</br>
La versión de Istio en el complemento gestionado ha sido probada por {{site.data.keyword.Bluemix_notm}} y se ha aprobado su uso en {{site.data.keyword.containerlong_notm}}. Para actualizar los componentes de Istio a la versión más reciente de Istio soportada por {{site.data.keyword.containerlong_notm}}, siga los pasos de [Actualización de complementos gestionados](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons).  

Si tiene que utilizar la versión más reciente de Istio o personalizar la instalación de Istio, puede instalar la versión de código abierto de Istio siguiendo los pasos de la [Guía de aprendizaje de inicio rápido de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/setup/kubernetes/quick-start-ibm/).
{: tip}

**¿Existe alguna limitación?** </br>
No puede habilitar el complemento Istio gestionado en el clúster si ha instalado el [controlador de admisiones del gestor de seguridad de imágenes de contenedor](/docs/services/Registry?topic=registry-security_enforce#security_enforce) en el clúster.

<br />


## ¿Qué puedo instalar?
{: #istio_components}

Istio on {{site.data.keyword.containerlong_notm}} se ofrece como tres complementos gestionados en el clúster.
{: shortdesc}

<dl>
<dt>Istio (`istio`)</dt>
<dd>Instala los componentes principales de Istio, incluido Prometheus. Para obtener más información sobre cualquiera de los siguientes componentes del plano de control, consulte la [documentación de Istio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/concepts/what-is-istio/).
  <ul><li>`Envoy` gestiona mediante proxy el tráfico de entrada y de salida para todos los servicios de la red. Envoy se despliega como un contenedor adicional en el mismo pod que el contenedor de la app.</li>
  <li>`Mixer` proporciona controles de política y recopilación de medidas telemétricas.<ul>
    <li>Las pods de telemetría se habilitan en un punto final Prometheus, que agrega todos los datos de telemetría del proxy de Envoy y de los servicios a los pods de la app.</li>
    <li>Los pods de política imponen el control de acceso, incluida la limitación de velocidad y la aplicación de políticas de lista blanca y de lista negra.</li></ul>
  </li>
  <li>`Pilot` proporciona servicios de descubrimiento de servicios de Envoy y configura las reglas de direccionamiento de gestión de tráfico para los complementos.</li>
  <li>`Citadel` utiliza la gestión de identidades y de credenciales para proporcionar autenticación de servicio a servicio y de usuario final.</li>
  <li>`Galley` valida los cambios en la configuración de los otros componentes del plano de control de Istio.</li>
</ul></dd>
<dt>Extras de Istio (`istio-extras`)</dt>
<dd>Opcional: Instala [Grafana ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://grafana.com/), [Jaeger ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.jaegertracing.io/) y [Kiali ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.kiali.io/) para proporcionar funciones extra de supervisión, rastreo y visualización para Istio.</dd>
<dt>App de ejemplo BookInfo (`istio-sample-bookinfo`)</dt>
<dd>Opcional: Despliega la [aplicación de ejemplo BookInfo para Istio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/examples/bookinfo/). Este despliegue incluye la configuración de la demostración base y las reglas de destino predeterminadas para que pueda probar las prestaciones de Istio de forma inmediata.</dd>
</dl>

<br>
Siempre puede ver los componentes de Istio que están habilitados en el clúster con el siguiente mandato:
```
ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
```
{: pre}

<br />


## Instalación de Istio on {{site.data.keyword.containerlong_notm}}
{: #istio_install}

Instale los complementos gestionados de Istio en un clúster existente.
{: shortdesc}

**Antes de empezar**</br>
* Asegúrese de que tiene el [rol de servicio **Escritor** o **Gestor** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre {{site.data.keyword.containerlong_notm}}.
* [Cree un clúster estándar o utilice uno existente con al menos 3 nodos trabajadores que tenga cada uno 4 núcleos y 16 GB de memoria (`b3c.4x16`) o más](/docs/containers?topic=containers-clusters#clusters_ui). Además, el clúster y los nodos trabajadores deben ejecutar al menos la versión mínima soportada de Kubernetes, que puede obtener con el mandato `ibmcloud ks addon-versions --addon istio`.
* [Defina su clúster como destino de la CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
* Si utiliza un clúster existente y ha instalado previamente Istio en el clúster mediante el diagrama de Helm de IBM o con otro método, [limpie la instalación de Istio](#istio_uninstall_other).

### Instalación de complementos de Istio gestionados en la CLI
{: #istio_install_cli}

1. Habilite el complemento `istio`.
  ```
  ibmcloud ks cluster-addon-enable istio --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. Opcional: Habilite el complemento `istio-extras`.
  ```
  ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID>
  ```
  {: pre}

3. Opcional: Habilite el complemento `istio-sample-bookinfo`.
  ```
  ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID>
  ```
  {: pre}

4. Verifique que los complementos de Istio gestionado que ha instalado están habilitados en este clúster.
  ```
  ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Salida de ejemplo:
  ```
  Name                      Version
  istio                     1.1.5
  istio-extras              1.1.5
  istio-sample-bookinfo     1.1.5
  ```
  {: screen}

5. También puede consultar los componentes individuales de cada complemento en el clúster.
  - Componentes de `istio` y de `istio-extras`: Asegúrese de que se han desplegado los servicios de Istio y sus pods correspondientes.
    ```
    kubectl get svc -n istio-system
    ```
    {: pre}

    ```
    NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
    grafana                  ClusterIP      172.21.98.154    <none>          3000/TCP                                                       2m
    istio-citadel            ClusterIP      172.21.221.65    <none>          8060/TCP,9093/TCP                                              2m
    istio-egressgateway      ClusterIP      172.21.46.253    <none>          80/TCP,443/TCP                                                 2m
    istio-galley             ClusterIP      172.21.125.77    <none>          443/TCP,9093/TCP                                               2m
    istio-ingressgateway     LoadBalancer   172.21.230.230   169.46.56.125   80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                              8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP   2m
    istio-pilot              ClusterIP      172.21.171.29    <none>          15010/TCP,15011/TCP,8080/TCP,9093/TCP                          2m
    istio-policy             ClusterIP      172.21.140.180   <none>          9091/TCP,15004/TCP,9093/TCP                                    2m
    istio-sidecar-injector   ClusterIP      172.21.248.36    <none>          443/TCP                                                        2m
    istio-telemetry          ClusterIP      172.21.204.173   <none>          9091/TCP,15004/TCP,9093/TCP,42422/TCP                          2m
    jaeger-agent             ClusterIP      None             <none>          5775/UDP,6831/UDP,6832/UDP                                     2m
    jaeger-collector         ClusterIP      172.21.65.195    <none>          14267/TCP,14268/TCP                                            2m
    jaeger-query             ClusterIP      172.21.171.199   <none>          16686/TCP                                                      2m
    kiali                    ClusterIP      172.21.13.35     <none>          20001/TCP                                                      2m
    prometheus               ClusterIP      172.21.105.229   <none>          9090/TCP                                                       2m
    tracing                  ClusterIP      172.21.125.177   <none>          80/TCP                                                         2m
    zipkin                   ClusterIP      172.21.1.77      <none>          9411/TCP                                                       2m
    ```
    {: screen}

    ```
    kubectl get pods -n istio-system
    ```
    {: pre}

    ```
    NAME                                      READY   STATUS    RESTARTS   AGE
    grafana-76dcdfc987-94ldq                  1/1     Running   0          2m
    istio-citadel-869c7f9498-wtldz            1/1     Running   0          2m
    istio-egressgateway-69bb5d4585-qxxbp      1/1     Running   0          2m
    istio-galley-75d7b5bdb9-c9d9n             1/1     Running   0          2m
    istio-ingressgateway-5c8764db74-gh8xg     1/1     Running   0          2m
    istio-pilot-55fd7d886f-vv6fb              2/2     Running   0          2m
    istio-policy-6bb6f6ddb9-s4c8t             2/2     Running   0          2m
    istio-sidecar-injector-7d9845dbb7-r8nq5   1/1     Running   0          2m
    istio-telemetry-7695b4c4d4-tlvn8          2/2     Running   0          2m
    istio-tracing-55bbf55878-z4rd2            1/1     Running   0          2m
    kiali-77566cc66c-kh6lm                    1/1     Running   0          2m
    prometheus-5d5cb44877-lwrqx               1/1     Running   0          2m
    ```
    {: screen}

  - Componentes de `istio-sample-bookinfo`: Asegúrese de que se han desplegado los microservicios de BookInfo y sus pods correspondientes.
    ```
    kubectl get svc -n default
    ```
    {: pre}

    ```
    NAME                      TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)          AGE
    details                   ClusterIP      172.21.19.104    <none>         9080/TCP         2m
    productpage               ClusterIP      172.21.168.196   <none>         9080/TCP         2m
    ratings                   ClusterIP      172.21.11.131    <none>         9080/TCP         2m
    reviews                   ClusterIP      172.21.117.164   <none>         9080/TCP         2m
    ```
    {: screen}

    ```
    kubectl get pods -n default
    ```
    {: pre}

    ```
    NAME                                     READY     STATUS      RESTARTS   AGE
    details-v1-6865b9b99d-7v9h8              2/2       Running     0          2m
    productpage-v1-f8c8fb8-tbsz9             2/2       Running     0          2m
    ratings-v1-77f657f55d-png6j              2/2       Running     0          2m
    reviews-v1-6b7f6db5c5-fdmbq              2/2       Running     0          2m
    reviews-v2-7ff5966b99-zflkv              2/2       Running     0          2m
    reviews-v3-5df889bcff-nlmjp              2/2       Running     0          2m
    ```
    {: screen}

### Instalación de complementos de Istio gestionados en la interfaz de usuario
{: #istio_install_ui}

1. En el [panel de control del clúster ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/kubernetes/clusters), pulse el nombre de un clúster.

2. Pulse el separador **Complementos**.

3. En la tarjeta Istio, pulse **Instalar**.

4. El recuadro de selección **Istio** ya está seleccionado. Para instalar también los extras de Istio y la app de ejemplo BookInfo, marque los recuadros de selección **Extras de Istio** y **Ejemplo de Istio**.

5. Pulse **Instalar**.

6. En la tarjeta Istio, verifique que los complementos que ha habilitado aparecen listados.

A continuación puede probar las prestaciones de Istio consultando la [app de ejemplo BookInfo](#istio_bookinfo).

<br />


## Prueba de la app de ejemplo BookInfo
{: #istio_bookinfo}

El complemento BookInfo (`istio-sample-bookinfo`) despliega la [aplicación de ejemplo BookInfo para Istio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/examples/bookinfo/) en el espacio de nombres `default`. Este despliegue incluye la configuración de la demostración base y las reglas de destino predeterminadas para que pueda probar las prestaciones de Istio de forma inmediata.
{: shortdesc}

Los cuatro microservicios de BookInfo incluyen:
* `productpage` llama a los microservicios `details` y `reviews` para llenar la página.
* `details` contiene información sobre libros.
* `reviews` contiene reseñas de libros y llamadas al microservicio `ratings`.
* `ratings` contiene información sobre clasificación de libros que acompaña a una revisión de un libro.

El microservicio `reviews` tiene varias versiones:
* `v1` no llama al microservicio `ratings`.
* `v2` llama al microservicio `ratings` y muestra clasificaciones como estrellas negras, de 1 a 5.
* `v3` llama al microservicio `ratings` y muestra clasificaciones como estrellas rojas, de 1 a 5.

Los archivos YAML de despliegue para cada uno de estos microservicios se modifican para que los proxies de complemento de Envoy se inyectan previamente como contenedores en los pods de los microservicios antes de que se desplieguen. Para obtener más información sobre la inyección manual de complementos, consulte la [documentación de Istio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/setup/kubernetes/sidecar-injection/). La app BookInfo ya está también expuesta en una dirección de entrada IP pública mediante una pasarela de Istio. Aunque la app BookInfo puede ayudarle a ponerse en marcha, no está pensada para su uso en producción.

Antes de empezar, [instale los complementos gestionados `istio`, `istio-extras` e `istio-sample-bookinfo`](#istio_install) en un clúster.

1. Obtenga la dirección pública del clúster.
  1. Defina el host de Ingress.
    ```
    export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
    ```
    {: pre}

  2. Defina el puerto de Ingress.
    ```
    export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
    ```
    {: pre}

  3. Cree una variable de entorno `GATEWAY_URL` que utilice el host y el puerto de Ingress.
     ```
     export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
     ```
     {: pre}

2. Ejecute curl sobre la variable `GATEWAY_URL` para comprobar que la app BookInfo se está ejecutando. Una respuesta `200` significa que la app BookInfo se está ejecutando correctamente con Istio.
   ```
   curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
   ```
   {: pre}

3.  Consulte la página web de BookInfo en un navegador.

    Mac OS o Linux:
    ```
    open http://$GATEWAY_URL/productpage
    ```
    {: pre}

    Windows:
    ```
    start http://$GATEWAY_URL/productpage
    ```
    {: pre}

4. Intente renovar la página varias veces. Las diferentes versiones de la sección de revisiones se muestran con estrellas rojas, estrellas negras y sin estrellas.

### Visión general de lo que ha sucedido
{: #istio_bookinfo_understanding}

El ejemplo BookInfo muestra cómo tres componentes de gestión del tráfico de Istio funcionan en combinación para direccionar el tráfico de entrada a la app.
{: shortdesc}

<dl>
<dt>`Pasarela`</dt>
<dd>La [pasarela ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/) `bookinfo-gateway` describe un equilibrador de carga, el servicio `istio-ingressgateway` en el espacio de nombres `istio-system` que actúa como punto de entrada para el tráfico HTTP/TCP correspondiente a BookInfo. Istio configura el equilibrador de carga para que escuche las solicitudes entrantes a las apps gestionadas por Istio en los puertos definidos en el archivo de configuración de pasarela.
</br></br>Para ver el archivo de configuración de la pasarela de BookInfo, ejecute el mandato siguiente.
<pre class="pre"><code>kubectl get gateway bookinfo-gateway -o yaml</code></pre></dd>

<dt>`VirtualService`</dt>
<dd>El [`servicio virtual` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/) `bookinfo` define las reglas que controlan la forma en que las solicitudes se direccionan dentro de la red de servicios mediante la definición de microservicios como `destinos`. En el servicio virtual `bookinfo`, el URI `/productpage` de una solicitud se direcciona al host `productpage` en el puerto `9080`. De este modo, todas las solicitudes a la app BookInfo se direccionan primero al microservicio `productpage`, que a continuación llama a los otros microservicios de BookInfo.
</br></br>Para ver la regla de servicio virtual que se aplica a BookInfo, ejecute el mandato siguiente.
<pre class="pre"><code>kubectl get virtualservice bookinfo -o yaml</code></pre></dd>

<dt>`DestinationRule`</dt>
<dd>Después de que la pasarela direccione la solicitud de acuerdo con la regla de servicio virtual, las [`DestinationRules` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/) `details`, `productpage`, `ratings` y `reviews` definen políticas que se aplican a la solicitud cuando accede a un microservicio. Por ejemplo, si renueva la página de productos de BookInfo, los cambios que verá son el resultado de que el microservicio `productpage` llame aleatoriamente a distintas versiones, `v1`, `v2` y `v3`, del microservicio `reviews`. Las versiones se seleccionan aleatoriamente porque la regla de destino `reviews` otorga el mismo peso a los `subconjuntos`, o versiones con nombre, del microservicio. Las reglas del servicio virtual utilizan estos subconjuntos cuando se direcciona el tráfico a versiones específicas del servicio.
</br></br>Para ver las reglas de destino que se aplican a BookInfo, ejecute el mandato siguiente.
<pre class="pre"><code>kubectl describe destinationrules</code></pre></dd>
</dl>

</br>

Luego puede [exponer BookInfo mediante el subdominio Ingress que proporciona IBM](#istio_expose_bookinfo) o [registrar, supervisar, rastrear y visualizar](#istio_health) la red de servicios de la app BookInfo.

<br />


## Registro, supervisión, rastreo y visualización de Istio
{: #istio_health}

Para registrar, supervisar, rastrear y visualizar las apps gestionadas por Istio on {{site.data.keyword.containerlong_notm}}, puede iniciar los paneles de control de Grafana, Jaeger y Kiali que están instalados en el complemento `istio-extras` o desplegar LogDNA y Sysdig como servicios de terceros en los nodos trabajadores.
{: shortdesc}

### Inicio de los paneles de control de Grafana, Jaeger y Kiali
{: #istio_health_extras}

El complemento extras de Istio (`istio-extras`) instala [Grafana ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://grafana.com/), [Jaeger ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.jaegertracing.io/) y [Kiali ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.kiali.io/). Inicie los paneles de control de cada uno de estos servicios para proporcionar funciones adicionales de supervisión, rastreo y visualización para Istio.
{: shortdesc}

Antes de empezar, [instale los complementos gestionados `istio` e `istio-extras`](#istio_install) en un clúster.

**Grafana**</br>
1. Inicie el reenvío de puerto de Kubernetes para el panel de control de Grafana.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
  ```
  {: pre}

2. Para abrir el panel de control de Istio Grafana, vaya al siguiente URL: http://localhost:3000/dashboard/db/istio-mesh-dashboard. Si ha instalado el [complemento BookInfo](#istio_bookinfo), el panel de control de Istio muestra métricas para el tráfico que ha generado al renovar la página del producto unas cuantas veces. Para obtener más información sobre cómo utilizar el panel de control de Istio Grafana, consulte [Visualización del panel de control de Istio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/tasks/telemetry/using-istio-dashboard/) en la documentación de código fuente abierto de Istio.

**Jaeger**</br>
1. De forma predeterminada, Istio genera el rastreo de 1 de cada 100 solicitudes, que es una tasa de muestreo del 1%. Debe enviar al menos 100 solicitudes antes de que se pueda ver el primer rastreo. Para enviar 100 solicitudes al servicio `productpage` del [Complemento BookInfo](#istio_bookinfo), ejecute el mandato siguiente.
  ```
  for i in `seq 1 100`; do curl -s -o /dev/null http://$GATEWAY_URL/productpage; done
  ```
  {: pre}

2. Inicie el reenvío de puerto de Kubernetes para el panel de control de Jaeger.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686 &
  ```
  {: pre}

3. Para abrir la IU de Jaeger, vaya a la siguiente dirección URL: http://localhost:16686.

4. Si ha instalado el complemento BookInfo, puede seleccionar `productpage` en la lista **Servicio** y pulsar **Buscar rastreos**. Se muestran los rastreos correspondientes al tráfico que ha generado al renovar la página de productos varias veces. Para obtener más información sobre el uso de Jaeger con Istio, consulte [Generación de rastreos mediante el ejemplo BookInfo ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/tasks/telemetry/distributed-tracing/#generating-traces-using-the-bookinfo-sample) en la documentación de código abierto de Istio.

**Kiali**</br>
1. Inicie el reenvío de puerto de Kubernetes para el panel de control de Kiali.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001 &
  ```
  {: pre}

2. Para abrir la IU de Kiali, vaya a la siguiente dirección URL: http://localhost:20001/kiali/console.

3. Especifique `admin` para nombre de usuario y frase de contraseña. Para obtener más información sobre cómo utilizar Kiale para ver los microservicios gestionados por Istio, consulte [Generación de un gráfico de servicio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://archive.istio.io/v1.0/docs/tasks/telemetry/kiali/#generating-a-service-graph) en la documentación de código fuente abierto de Istio.

### Configuración del registro con {{site.data.keyword.la_full_notm}}
{: #istio_health_logdna}

Puede gestionar fácilmente los registros del contenedor de la app y de cada contenedor del complemento proxy de Envoy de cada pod mediante el despliegue de LogDNA en los nodos trabajadores para que reenvíe los registros a {{site.data.keyword.loganalysislong}}.
{: shortdesc}

Para utilizar [{{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about), despliegue un agente de registro en cada nodo trabajador del clúster. Este agente recopila los registros con la extensión `*.log` y los archivos sin extensión almacenados en el directorio `/var/log` de su pod desde todos los espacios de nombres, incluido `kube-system`. Estos registros incluyen registros del contenedor de app y del contenedor del complemento proxy de Envoy en cada pod. A continuación, el agente reenvía los registros al servicio {{site.data.keyword.la_full_notm}}.

Para empezar, configure LogDNA para el clúster siguiendo los pasos del apartado [Gestión de registros de clúster de Kubernetes con {{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).




### Configuración de la supervisión con {{site.data.keyword.mon_full_notm}}
{: #istio_health_sysdig}

Obtenga visibilidad operativa sobre el rendimiento y el estado de las apps gestionadas por Istio mediante el despliegue de Sysdig en sus nodos trabajadores para reenviar métricas a {{site.data.keyword.monitoringlong}}.
{: shortdesc}

Con Istio on {{site.data.keyword.containerlong_notm}}, el complemento `istio` gestionado instala Prometheus en el clúster. Los pods `istio-mixer-telemetry` del clúster se anotan con un punto final Prometheus de modo que Prometheus pueda agregar todos los datos de telemetría correspondientes a sus pods. Cuando despliega un agente Sysdig en cada nodo trabajador del clúster, Sysdig ya está habilitado automáticamente para detectar y extraer los datos de estos puntos finales de Prometheus a fin de mostrarlos en el panel de control de supervisión de {{site.data.keyword.Bluemix_notm}}.

Como todo el trabajo de Prometheus ya está hecho, lo único que le queda por hacer es desplegar Sysdig en el clúster.

1. Configure Sysdig siguiendo los pasos de [Análisis de métricas para una app desplegadas en un clúster de Kubernetes](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster).

2. [Inicie la IU de Sysdig ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_step3).

3. Pulse **Añadir nuevo panel de control**.

4. Busque `Istio` y seleccione uno de los paneles de control de Istio predefinidos de Sysdig.

Para obtener más información sobre cómo hacer referencia a métricas y a paneles de control, cómo supervisar los componentes internos de Istio y cómo supervisar despliegues de tipo A/B de Istio y despliegues de prueba, consulte [Cómo supervisar Istio, la red de servicio de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://sysdig.com/blog/monitor-istio/). Busque la sección sobre "Supervisión de Istio: referencia a métricas y paneles de control" en la publicación del blog.

<br />


## Configuración de la inyección de complementos para las apps
{: #istio_sidecar}

¿Está listo para gestionar sus propias apps mediante Istio? Antes de desplegar la app, debe decidir cómo desea inyectar los complementos de proxy de Envoy en los pods de la app.
{: shortdesc}

Cada pod de app debe estar en ejecución en el complemento de proxy de Envoy para que los microservicios se puedan incluir en la red de servicios. Puede asegurarse de que los complementos se inyectan en cada pod de la app de forma automática o manual. Para obtener más información sobre la inyección de complementos, consulte la [documentación de Istio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/setup/kubernetes/sidecar-injection/).

### Habilitación de la inyección automática de complementos
{: #istio_sidecar_automatic}

Cuando la inyección automática de complementos está habilitada, un espacio de nombres escucha cualquier nuevo despliegue y modifica automáticamente la especificación de la plantilla de pod de forma que los pods de app se creen con contenedores de complementos de proxy de Envoy. Habilite la inyección automática de complementos para un espacio de nombres cuando vaya a desplegar varias apps que desee integrar con Istio en ese espacio de nombres. La inyección automática de complementos no está habilitada de forma predeterminada para ninguno de los espacios de nombres en el complemento gestionado de Istio.

Para habilitar la inyección automática de complementos para un espacio de nombres:

1. Obtenga el nombre del espacio de nombres en el que desea desplegar las apps gestionadas por Istio.
  ```
  kubectl get namespaces
  ```
  {: pre}

2. Etiquete el espacio de nombres como `istio-injection=enabled`.
  ```
  kubectl label namespace <namespace> istio-injection=enabled
  ```
  {: pre}

3. Despliegue las apps en el espacio de nombres etiquetado o vuelva a desplegar las apps que ya están en el espacio de nombres.
  * Para desplegar una app en el espacio de nombres etiquetado:
    ```
    kubectl apply <myapp>.yaml --namespace <namespace>
    ```
    {: pre}
  * Para volver a desplegar una app que ya está desplegada en dicho espacio de nombres, suprima el pod de la app para que se vuelva a desplegar con el complemento inyectado.
    ```
    kubectl delete pod -l app=<myapp>
    ```
    {: pre}

5. Si no ha creado un servicio para exponer la app, cree un servicio de Kubernetes. El servicio de Kubernetes debe exponer la app para incluirla como microservicio en la red de servicios de Istio. Asegúrese de cumplir con los [requisitos de Istio para pods y servicios ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/setup/kubernetes/spec-requirements/).

  1. Defina un servicio para la app.
    ```
    apiVersion: v1
          kind: Service
          metadata:
            name: myappservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
       - protocol: TCP
             port: 8080
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes del archivo YAML del servicio</th>
    </thead>
    <tbody>
    <tr>
    <td><code>selector</code></td>
    <td>Especifique el par de clave de etiqueta (<em>&lt;selector_key&gt;</em>) y valor (<em>&lt;selector_value&gt;</em>) que desee utilizar para establecer como destino los pods en los que se ejecuta la app.</td>
     </tr>
     <tr>
     <td><code>port</code></td>
     <td>El puerto que en el que está a la escucha el servicio.</td>
     </tr>
     </tbody></table>

  2. Cree el servicio en el clúster. Asegúrese de que el servicio se despliega en el mismo espacio de nombres que la app.
    ```
    kubectl apply -f myappservice.yaml -n <namespace>
    ```
    {: pre}

Los pods de la app están ahora integrados en la red de servicios de Istio porque tienen el contenedor de complementos de Istio que se ejecutan junto con el contenedor de la app.

### Inyección manual de complementos
{: #istio_sidecar_manual}

Si no desea habilitar la inyección automática de complementos en un espacio de nombres, puede inyectar manualmente el complemento en un archivo YAML de despliegue. Inyectar complementos manualmente cuando las apps se ejecuten en espacios de nombres junto con otros despliegues en los que no desea que se inyecten complementos automáticamente.

Para inyectar manualmente complementos en un despliegue:

1. Descargue el cliente `istioctl`.
  ```
  curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.1.5 sh -
  ```

2. Vaya al directorio del paquete de Istio.
  ```
  cd istio-1.1.5
  ```
  {: pre}

3. Inyecte el complemento Envoy en el archivo YAML de despliegue de la app.
  ```
  istioctl kube-inject -f <myapp>.yaml | kubectl apply -f -
  ```
  {: pre}

4. Despliegue la app.
  ```
  kubectl apply <myapp>.yaml
  ```
  {: pre}

5. Si no ha creado un servicio para exponer la app, cree un servicio de Kubernetes. El servicio de Kubernetes debe exponer la app para incluirla como microservicio en la red de servicios de Istio. Asegúrese de cumplir con los [requisitos de Istio para pods y servicios ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/setup/kubernetes/spec-requirements/).

  1. Defina un servicio para la app.
    ```
    apiVersion: v1
          kind: Service
          metadata:
            name: myappservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
       - protocol: TCP
             port: 8080
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes del archivo YAML del servicio</th>
    </thead>
    <tbody>
    <tr>
    <td><code>selector</code></td>
    <td>Especifique el par de clave de etiqueta (<em>&lt;selector_key&gt;</em>) y valor (<em>&lt;selector_value&gt;</em>) que desee utilizar para establecer como destino los pods en los que se ejecuta la app.</td>
     </tr>
     <tr>
     <td><code>port</code></td>
     <td>El puerto que en el que está a la escucha el servicio.</td>
     </tr>
     </tbody></table>

  2. Cree el servicio en el clúster. Asegúrese de que el servicio se despliega en el mismo espacio de nombres que la app.
    ```
    kubectl apply -f myappservice.yaml -n <namespace>
    ```
    {: pre}

Los pods de la app están ahora integrados en la red de servicios de Istio porque tienen el contenedor de complementos de Istio que se ejecutan junto con el contenedor de la app.

<br />


## Exposición de apps gestionadas por Istio utilizando un nombre de host proporcionado por IBM
{: #istio_expose}

Después de [configurar la inyección de complementos de proxy de Envoy](#istio_sidecar) y de desplegar las apps en la red de servicios de Istio, puede exponer las apps gestionadas por Istio a solicitudes públicas utilizando un nombre de host proporcionado por IBM.
{: shortdesc}

Istio utiliza [pasarelas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/) y [servicios virtuales ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/) para controlar cómo se direcciona el tráfico a las apps. Una pasarela configura un equilibrador de carga, `istio-ingressgateway`, que actúa como punto de entrada para las apps gestionadas por Istio. Puede exponer las apps gestionadas por Istio registrando la dirección IP externa del equilibrador de carga `istio-ingressgateway` con una entrada DNS y un nombre de host.

Puede probar primero el [ejemplo para exponer BookInfo](#istio_expose_bookinfo) o [exponer públicamente sus propias apps gestionadas por Istio](#istio_expose_link).

### Ejemplo: Exposición de BookInfo utilizando un nombre de host proporcionado por IBM
{: #istio_expose_bookinfo}

Cuando se habilita el complemento BookInfo en el clúster, se crea la pasarela de Istio `bookinfo-gateway`. La pasarela utiliza reglas de servicio virtual de Istio y de destino para configurar un equilibrador de carga, `istio-ingressgateway`, que expone públicamente la app BookInfo. En los pasos siguientes, creará un nombre de host para la dirección IP del equilibrador de carga `istio-ingressgateway` a través del cual podrá acceder públicamente a BookInfo.
{: shortdesc}

Antes de empezar, [habilite el complemento gestionado `istio-sample-bookinfo`](#istio_install) en un clúster.

1. Obtenga la dirección **EXTERNAL-IP** del equilibrador de carga `istio-ingressgateway`.
  ```
  kubectl get svc -n istio-system
  ```
  {: pre}

  En la siguiente información de salida de ejemplo, la **EXTERNAL-IP** es `168.1.1.1`.
  ```
  NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
  ...
  istio-ingressgateway     LoadBalancer   172.21.XXX.XXX   169.1.1.1       80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                            8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP  22m
  ```
  {: screen}

2. Registre la IP creando un nombre de host DNS.
  ```
  ibmcloud ks nlb-dns-create --cluster <cluster_name_or_id> --ip <LB_IP>
  ```
  {: pre}

3. Verifique que se ha creado el nombre de host.
  ```
  ibmcloud ks nlb-dnss --cluster <cluster_name_or_id>
  ```
  {: pre}

  Salida de ejemplo:
  ```
  Hostname                                                                                IP(s)              Health Monitor   SSL Cert Status           SSL Cert Secret Name
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     ["168.1.1.1"]      None             created                   <certificate>
  ```
  {: screen}

4. En un navegador web, abra la página de productos de BookInfo.
  ```
  https://<host_name>/productpage
  ```
  {: codeblock}

5. Intente renovar la página varias veces. El ALB recibe las solicitudes destinadas a `http://<host_name>/productpage` y las reenvía al equilibrador de carga de la pasarela de Istio. Las distintas versiones del microservicio `reviews` se siguen devolviendo aleatoriamente porque la pasarela de Istio gestiona las reglas de direccionamiento de destino y servicio virtual para los microservicios.

Para obtener más información sobre la pasarela, las reglas de servicio virtual y las reglas de destino para la app BookInfo, consulte [Visión general de lo que ha sucedido](#istio_bookinfo_understanding). Para obtener más información sobre el registro de nombres de host DNS en {{site.data.keyword.containerlong_notm}}, consulte [Registro de un nombre de host de NLB](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname).

### Exposición pública de apps gestionadas por Istio utilizando un nombre de host proporcionado por IBM
{: #istio_expose_link}

Exponga públicamente las apps gestionadas por Istio creando una pasarela Istio, un servicio virtual que defina las reglas de gestión de tráfico para los servicios gestionados por Istio, y un nombre de host DNS para la dirección IP externa del equilibrador de carga `istio-ingressgateway`.
{: shortdesc}

**Antes de empezar:**
1. [Instale el complemento gestionado de `istio`](#istio_install) en un clúster.
2. Instale el cliente `istioctl`.
  1. Descargue `istioctl`.
    ```
    curl -L https://git.io/getLatestIstio | sh -
    ```
  2. Vaya al directorio del paquete de Istio.
    ```
    cd istio-1.1.5
    ```
    {: pre}
3. [Configure la inyección de complementos para los microservicios de la app, despliegue los microservicios de la app en un espacio de nombres y cree servicios de Kubernetes para los microservicios de la app para que se puedan incluir en la red de servicios de Istio](#istio_sidecar).

</br>
**Para exponer públicamente las apps gestionadas por Istio con un nombre de host:**

1. Cree una pasarela. Esta pasarela de ejemplo utiliza el servicio de equilibrador de carga `istio-ingressgateway` para exponer el puerto 80 para HTTP. Sustituya `<namespace>` por el espacio de nombres en el que se han desplegado los microservicios gestionados por Istio. Si los microservicios escuchan en un puerto distinto al `80`, añada dicho puerto. Para obtener más información sobre los componentes de YAML de pasarela, consulte la [documentación de referencia de Istio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/).
  ```
  apiVersion: networking.istio.io/v1alpha3
  kind: Gateway
  metadata:
    name: my-gateway
    namespace: <namespace>
  spec:
    selector:
      istio: ingressgateway
    servers:
    - port:
        name: http
        number: 80
        protocol: HTTP
      hosts:
      - '*'
  ```
  {: codeblock}

2. Aplique la pasarela en el espacio de nombres en el que se han desplegado los microservicios gestionados por Istio.
  ```
  kubectl apply -f my-gateway.yaml -n <namespace>
  ```
  {: pre}

3. Cree un servicio virtual que utilice la pasarela `my-gateway` y defina las reglas de direccionamiento para los microservicios de la app. Para obtener más información sobre los componentes de YAML de servicio virtual, consulte la [documentación de referencia de Istio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/).
  ```
  apiVersion: networking.istio.io/v1alpha3
  kind: VirtualService
  metadata:
    name: my-virtual-service
    namespace: <namespace>
  spec:
    gateways:
    - my-gateway
    hosts:
    - '*'
    http:
    - match:
      - uri:
          exact: /<service_path>
      route:
      - destination:
          host: <service_name>
          port:
            number: 80
  ```
  {: codeblock}

  <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
  </thead>
  <tbody>
  <tr>
  <td><code>namespace</code></td>
  <td>Sustituya <em>&lt;namespace&gt;</em> por el espacio de nombres en el que se han desplegado los microservicios gestionados por Istio.</td>
  </tr>
  <tr>
  <td><code>gateways</code></td>
  <td><code>my-gateway</code> se especifica para que la pasarela pueda aplicar estas reglas de direccionamiento de servicio virtual al equilibrador de carga <code>istio-ingressgateway</code>.<td>
  </tr>
  <tr>
  <td><code>http.match.uri.exact</code></td>
  <td>Sustituya <em>&lt;service_path&gt;</em> por la vía de acceso en la que escucha el microservicio del punto de entrada. Por ejemplo, en la app BookInfo, la vía de acceso se ha definido como <code>/productpage</code>.</td>
  </tr>
  <tr>
  <td><code>http.route.destination.host</code></td>
  <td>Sustituya <em>&lt;service_name&gt;</em> por el nombre del microservicio de punto de entrada. Por ejemplo, en la app BookInfo, <code>productpage</code> sirve como microservicio de punto de entrada que llama a los otros microservicios de la app.</td>
  </tr>
  <tr>
  <td><code>http.route.destination.port.number</code></td>
  <td>Si el microservicio escucha en un puerto distinto, sustituya <em>&lt;80&gt;</em> por el puerto.</td>
  </tr>
  </tbody></table>

4. Aplique las reglas de servicio virtual en el espacio de nombres en el que se ha desplegado el microservicio gestionado por Istio.
  ```
  kubectl apply -f my-virtual-service.yaml -n <namespace>
  ```
  {: pre}

5. Obtenga la dirección **EXTERNAL-IP** del equilibrador de carga `istio-ingressgateway`.
  ```
  kubectl get svc -n istio-system
  ```
  {: pre}

  En la siguiente información de salida de ejemplo, la **EXTERNAL-IP** es `168.1.1.1`.
  ```
  NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
  ...
  istio-ingressgateway     LoadBalancer   172.21.XXX.XXX   169.1.1.1       80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                            8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP  22m
  ```
  {: screen}

6. Registre la IP del equilibrador de carga `istio-ingressgateway` creando un nombre de host DNS.
  ```
  ibmcloud ks nlb-dns-create --cluster <cluster_name_or_id> --ip <LB_IP>
  ```
  {: pre}

7. Verifique que se ha creado el nombre de host.
  ```
  ibmcloud ks nlb-dnss --cluster <cluster_name_or_id>
  ```
  {: pre}

  Salida de ejemplo:
  ```
  Hostname                                                                                IP(s)              Health Monitor   SSL Cert Status           SSL Cert Secret Name
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     ["168.1.1.1"]      None             created                   <certificate>
  ```
  {: screen}

7. En un navegador web, verifique que el tráfico se está direccionando a los microservicios gestionados por Istio especificando el URL del microservicio de la app al que se accede.
  ```
  http://<host_name>/<service_path>
  ```
  {: codeblock}

En resumen, ha creado una pasarela denominada `my-gateway`. Esta pasarela utiliza el servicio de equilibrador de carga `istio-ingressgateway` existente para exponer la app. El equilibrador de carga `istio-ingressgateway` utiliza las reglas que ha definido en el servicio virtual `my-virtual-service` para direccionar el tráfico a la app. Por último, ha creado un nombre de host para el equilibrador de carga `istio-ingressgateway`. Todas las solicitudes de usuario para el nombre de host se reenvían a la app de acuerdo con las reglas de direccionamiento de Istio. Para obtener más información sobre el registro de nombres de host DNS en {{site.data.keyword.containerlong_notm}}, incluyendo información sobre cómo configurar comprobaciones de estado personalizadas para los nombres de host, consulte [Registro de un nombre de host de NLB](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname).

¿Busca un control aún más detallado sobre el direccionamiento? Para crear reglas que se aplican después de que el equilibrador de carga direccione el tráfico a cada microservicio, como por ejemplo reglas para enviar tráfico a diferentes versiones de un microservicio, puede crear y aplicar [`DestinationRules` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/).
{: tip}

<br />


## Actualización de Istio on {{site.data.keyword.containerlong_notm}}
{: #istio_update}

La versión de Istio en el complemento gestionado de Istio ha sido probada por {{site.data.keyword.Bluemix_notm}} y se ha aprobado su uso en {{site.data.keyword.containerlong_notm}}. Para actualizar los componentes de Istio a la versión más reciente de Istio soportada por {{site.data.keyword.containerlong_notm}}, consulte [Actualización de complementos gestionados](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons).
{: shortdesc}

## Desinstalación de Istio on {{site.data.keyword.containerlong_notm}}
{: #istio_uninstall}

Si ha terminado de trabajar con Istio, puede limpiar los recursos de Istio en el clúster desinstalando los complementos de Istio.
{:shortdesc}

El complemento `istio` es una dependencia para los complementos `istio-extras`, `istio-sample-bookinfo` y [`knative`](/docs/containers?topic=containers-serverless-apps-knative). El complemento `istio-extras` es una dependencia para el complemento `istio-sample-bookinfo`.
{: important}

**Opcional**: Los recursos que haya creado o modificado en el espacio de nombres `istio-system` y todos los recursos de Kubernetes que se hayan generado automáticamente mediante definiciones de recursos personalizados (CRD) se eliminan. Si desea conservar esos recursos, debe guardarlos antes de desinstalar los complementos de `istio`.
1. Guarde los recursos, como por ejemplo los archivos de configuración de un servicio o app, que haya creado o modificado en el espacio de nombres `istio-system`.
   Mandato de ejemplo:
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

2. Guarde los recursos de Kubernetes que se han generado automáticamente mediante CRD en `istio-system` a un archivo YAML de la máquina local.
   1. Obtenga las CRD en `istio-system`.
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. Guarde los recursos creados a partir de estos CRD.

### Desinstalación de complementos de Istio gestionados en la CLI
{: #istio_uninstall_cli}

1. Inhabilite el complemento `istio-sample-bookinfo`.
  ```
  ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. Inhabilite el complemento `istio-extras`.
  ```
  ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
  ```
  {: pre}

3. Inhabilite el complemento `istio`.
  ```
  ibmcloud ks cluster-addon-disable istio --cluster <cluster_name_or_ID> -f
  ```
  {: pre}

4. Verifique que todos los complementos de Istio gestionados están inhabilitados en este clúster. No se devuelve ningún complemento de Istio en la salida.
  ```
  ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
  ```
  {: pre}

### Desinstalación de complementos de Istio gestionados en la IU
{: #istio_uninstall_ui}

1. En el [panel de control del clúster ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/kubernetes/clusters), pulse el nombre de un clúster.

2. Pulse el separador **Complementos**.

3. En la tarjeta Istio, pulse el icono de menú.

4. Desinstale complementos de Istio individuales o todos ellos.
  - Complementos de Istio individuales:
    1. Pulse **Gestionar**.
    2. Desmarque los recuadros de selección correspondientes a los complementos que desea inhabilitar. Si borra un complemento, es posible que se borren automáticamente otros complementos que requieren dicho complemento como una dependencia.
    3. Pulse **Gestionar**. Los complementos de Istio se inhabilitan y los recursos correspondientes a dichos complementos se eliminan de este clúster.
  - Todos los complementos de Istio:
    1. Pulse **Desinstalar**. Todos los complementos de Istio gestionados se inhabilitan en este clúster y todos los recursos de Istio de este clúster se eliminan.

5. En la tarjeta Istio, verifique que los complementos que ha desinstalado ya no aparecen listados.

<br />


### Desinstalación de otras instalaciones de Istio en el clúster
{: #istio_uninstall_other}

Si ha instalado anteriormente Istio en el clúster mediante el diagrama de Helm de IBM o por otro método, limpie la instalación de Istio antes de habilitar los complementos de Istio gestionados en el clúster. Para comprobar si Istio ya se encuentra en un clúster, ejecute `kubectl get namespaces`
y busque el espacio de nombres `istio-system` en la salida.
{: shortdesc}

- Si ha instalado Istio mediante el diagrama de Helm de {{site.data.keyword.Bluemix_notm}}:
  1. Desinstale el despliegue de Helm de Istio.
    ```
    helm del istio --purge
    ```
    {: pre}

  2. Si ha utilizado Helm 2.9 o anterior, suprima el recurso de trabajo adicional.
    ```
    kubectl -n istio-system delete job --all
    ```
    {: pre}

- Si ha instalado Istio manualmente o ha utilizado el diagrama de Helm de la comunidad de Istio, consulte la [documentación de desinstalación de Istio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/setup/kubernetes/quick-start/#uninstall-istio-core-components).
* Si ha instalado anteriormente BookInfo en el clúster, limpie estos recursos.
  1. Cambie el directorio a la ubicación de archivo de Istio.
    ```
    cd <filepath>/istio-1.1.5
    ```
    {: pre}

  2. Suprima todos los servicios, pods y despliegues de BookInfo del clúster.
    ```
    samples/bookinfo/platform/kube/cleanup.sh
    ```
    {: pre}

<br />


## ¿Qué es lo siguiente?
{: #istio_next}

* Para explorar más Istio, puede encontrar más guías en la [documentación de Istio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/).
* Siga la clase sobre [Iniciación a los microservicios con Istio y el servicio IBM Cloud Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cognitiveclass.ai/courses/get-started-with-microservices-istio-and-ibm-cloud-container-service/). **Nota**: puede saltarse la sección sobre instalación de este curso.
* Consulte esta publicación del blog sobre cómo utilizar [Vistio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://itnext.io/vistio-visualize-your-istio-mesh-using-netflixs-vizceral-b075c402e18e) para visualizar la malla de servicios de Istio.
