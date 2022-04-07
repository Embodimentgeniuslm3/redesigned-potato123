---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, ingress

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



# Personalización de Ingress con anotaciones
{: #ingress_annotation}

Para añadir prestaciones al equilibrador de carga de aplicación (ALB) de Ingress, especifique anotaciones como metadatos en un recurso Ingress.
{: shortdesc}

Antes de utilizar anotaciones, asegúrese de que ha definido correctamente la configuración del servicio Ingress siguiendo los pasos del apartado [Equilibrio de carga HTTPS con equilibradores de carga de aplicación (ALB) de Ingress](/docs/containers?topic=containers-ingress). Cuando haya configurado el ALB Ingress con una configuración básica, puede ampliar sus prestaciones añadiendo anotaciones al archivo de recursos de Ingress.
{: note}

<table>
<caption>Anotaciones generales</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Anotaciones generales</th>
<th>Nombre</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td><a href="#custom-errors">Acciones de error personalizadas</a></td>
<td><code>custom-errors, custom-error-actions</code></td>
<td>Indique las acciones personalizadas que puede emprender el ALB para errores HTTP específicos.</td>
</tr>
<tr>
<td><a href="#location-snippets">Fragmentos de ubicación</a></td>
<td><code>location-snippets</code></td>
<td>Añada una configuración de bloque de ubicación personalizada para un servicio.</td>
</tr>
<tr>
<td><a href="#alb-id">Direccionamiento de ALB privado</a></td>
<td><code>ALB-ID</code></td>
<td>Direccione las solicitudes entrantes a sus apps con un ALB privado.</td>
</tr>
<tr>
<td><a href="#server-snippets">Fragmentos de servidor</a></td>
<td><code>server-snippets</code></td>
<td>Añada una configuración de bloque de servidor personalizado.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Anotaciones de conexión</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Anotaciones de conexión</th>
 <th>Nombre</th>
 <th>Descripción</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">Tiempos de espera excedidos de conexión y tiempos de espera excedidos de lectura personalizados</a></td>
  <td><code>proxy-connect-timeout, proxy-read-timeout</code></td>
  <td>Defina el tiempo que el ALB espera para conectarse a la app de fondo y para leer información de la misma antes de considerar que la app de fondo no está disponible.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">Solicitudes de estado activo</a></td>
  <td><code>keepalive-requests</code></td>
  <td>Establece el número máximo de solicitudes que se pueden servir a través de una conexión en estado activo.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">Tiempo de espera en estado activo</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>Establece el tiempo máximo que se mantiene abierta una conexión en estado activo en el servidor.</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">Siguiente proxy sentido ascendente</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>Establece cuándo el ALB puede pasar una solicitud al siguiente servidor en sentido ascendente.</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">Afinidad de sesión con cookies</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>El tráfico de red de entrada siempre se direcciona al mismo servidor en sentido ascendente mediante una "sticky cookie".</td>
  </tr>
  <tr>
  <td><a href="#upstream-fail-timeout">Tiempo de espera de error en sentido ascendente</a></td>
  <td><code>upstream-fail-timeout</code></td>
  <td>Establezca el periodo de tiempo durante el cual el ALB puede intentar conectar con el servidor antes de que se considere que el servidor no está disponible.</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">Estado activo en sentido ascendente</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>Establece el número máximo de conexiones de estado activo para un servidor en sentido ascendente.</td>
  </tr>
  <tr>
  <td><a href="#upstream-max-fails">Número máximo de errores en sentido ascendente</a></td>
  <td><code>upstream-max-fails</code></td>
  <td>Establezca el número máximo de intentos fallidos de comunicarse con el servidor antes de que se considere que el servidor no está disponible.</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>Anotaciones de autenticación TLS/SSL y HTTPS</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>Anotaciones de autenticación TLS/SSL y HTTPS</th>
  <th>Nombre</th>
  <th>Descripción</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#custom-port">Puertos HTTP y HTTPS personalizados</a></td>
  <td><code>custom-port</code></td>
  <td>Cambie los puertos predeterminados para el tráfico de red HTTP (puerto 80) y HTTPS (puerto 443).</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">HTTP redirige a HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>Redirige las solicitudes HTTP no seguras del dominio a HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#hsts">HTTP con seguridad de transporte estricta (HSTS)</a></td>
  <td><code>hsts</code></td>
  <td>Configure al navegador para acceder al dominio utilizando únicamente HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">Autenticación mutua</a></td>
  <td><code>mutual-auth</code></td>
  <td>Configure la autenticación mutua para el ALB.</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">Soporte de servicios SSL</a></td>
  <td><code>ssl-services</code></td>
  <td>Permita el soporte de servicios SSL para cifrar el tráfico en las apps ascendentes que requieren HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#tcp-ports">Puertos TCP</a></td>
  <td><code>tcp-ports</code></td>
  <td>Acceda a una app a través de un puerto TCP no estándar.</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>Anotaciones de direccionamiento de vía de acceso</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Anotaciones de direccionamiento de vía de acceso</th>
<th>Nombre</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-external-service">Servicios externos</a></td>
<td><code>proxy-external-service</code></td>
<td>Añada definiciones de vía de acceso a servicios externos, como un servicio alojado en {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td><a href="#location-modifier">Modificador de ubicación</a></td>
<td><code>location-modifier</code></td>
<td>Modifique la forma en la que el ALB coteja el URI de solicitud con la vía de acceso de la app.</td>
</tr>
<tr>
<td><a href="#rewrite-path">Vías de acceso de reescritura</a></td>
<td><code>rewrite-path</code></td>
<td>Se direcciona el tráfico de red entrante a una vía de acceso distinta de aquella en la que escucha la app de fondo.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Anotaciones de almacenamiento intermedio de proxy</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Anotaciones de almacenamiento intermedio de proxy</th>
 <th>Nombre</th>
 <th>Descripción</th>
 </thead>
 <tbody>
 <tr>
<td><a href="#large-client-header-buffers">Almacenamientos intermedios de cabeceras de cliente largas</a></td>
<td><code>large-client-header-buffers</code></td>
<td>Establezca el tamaño y el número máximo de los almacenamientos intermedios que leen cabeceras de solicitud de cliente largas.</td>
</tr>
 <tr>
 <td><a href="#proxy-buffering">Colocación en almacenamiento intermedio de datos de respuesta del cliente</a></td>
 <td><code>proxy-buffering</code></td>
 <td>Inhabilita la colocación en almacenamiento intermedio de una respuesta del cliente en el ALB al enviar la respuesta al cliente.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">Almacenamientos intermedios de proxy</a></td>
 <td><code>proxy-buffers</code></td>
 <td>Establece el número y el tamaño de los almacenamientos intermedios que leen una respuesta de una única conexión desde el servidor mediante proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">Tamaño de almacenamiento intermedio de proxy</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>Establezca el tamaño del almacenamiento intermedio que lee la primera parte de la respuesta que se recibe del servidor proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">Tamaño de los almacenamientos intermedios ocupados del proxy</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>Establezca el tamaño de los almacenamientos intermedios de proxy que puedan estar ocupados.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>Anotaciones de solicitud y respuesta</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Anotaciones de solicitud y respuesta</th>
<th>Nombre</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td><a href="#add-host-port">Adición del puerto de servidor a la cabecera de servidor</a></td>
<td><code>add-host-port</code></td>
<td>Añada el puerto de servidor al host para las solicitudes de direccionamiento.</td>
</tr>
<tr>
<td><a href="#client-max-body-size">Tamaño de cuerpo de solicitud del cliente</a></td>
<td><code>client-max-body-size</code></td>
<td>Defina el tamaño máximo del cuerpo que puede enviar el cliente como parte de una solicitud.</td>
</tr>
<tr>
<td><a href="#proxy-add-headers">Solicitud de cliente o cabecera de respuesta adicional</a></td>
<td><code>proxy-add-headers, response-add-headers</code></td>
<td>Añade información de cabecera a una solicitud de cliente antes de reenviar la solicitud a la app de fondo, o añade una respuesta del cliente antes de enviar la respuesta al cliente.</td>
</tr>
<tr>
<td><a href="#response-remove-headers">Eliminación de cabecera de respuesta del cliente</a></td>
<td><code>response-remove-headers</code></td>
<td>Elimina la información de cabecera de una respuesta del cliente antes de reenviar la respuesta al cliente.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Anotaciones de límite de servicio</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Anotaciones de límite de servicio</th>
<th>Nombre</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">Límites de velocidad global</a></td>
<td><code>global-rate-limit</code></td>
<td>Limite la velocidad de procesamiento de solicitudes y el número de conexiones por una clave definida para todos los servicios.</td>
</tr>
<tr>
<td><a href="#service-rate-limit">Límites de velocidad de servicio</a></td>
<td><code>service-rate-limit</code></td>
<td>Limite la velocidad de procesamiento de solicitudes y el número de conexiones por una clave definida para servicios específicos.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Anotaciones de autenticación de usuario</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Anotaciones de autenticación de usuario</th>
<th>Nombre</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td><a href="#appid-auth">Autenticación de {{site.data.keyword.appid_short}}</a></td>
<td><code>appid-auth</code></td>
<td>Utilice {{site.data.keyword.appid_full}} para autenticarse con la app.</td>
</tr>
</tbody></table>

<br>

## Anotaciones generales
{: #general}

### Acciones de error personalizadas (`custom-errors`, `custom-error-actions`)
{: #custom-errors}

Indique las acciones personalizadas que puede emprender el ALB para errores HTTP específicos.
{: shortdesc}

**Descripción**</br>
Para manejar errores de HTTP específicos que puedan producirse, puede configurar acciones de error personalizadas que debe emprender el ALB.

* La anotación `custom-errors` define el nombre de servicio, el error HTTP que se debe gestionar y el nombre de la acción de error que el ALB debe emprender cuando encuentre el error HTTP especificado para el servicio.
* La anotación `custom-error-actions` define acciones de error personalizadas en fragmentos de código NGINX.

Por ejemplo, en la anotación `custom-errors`, puede configurar el ALB para que gestione los errores HTTP `401` para `app1` devolviendo una acción de error personalizada llamada `/errorAction401`. A continuación, en la anotación `custom-error-actions`, puede definir un fragmento de código llamado `/errorAction401` para que el ALB devuelva una página de error personalizada al cliente.</br>

También puede utilizar la anotación `custom-errors` para redirigir al cliente a un servicio de errores que gestiona. Debe definir la vía de acceso a este servicio de error en la sección `paths` del archivo de recursos Ingress.

**YAML del recurso de Ingress de ejemplo**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/custom-errors: "serviceName=<app1> httpError=<401> errorActionName=</errorAction401>;serviceName=<app2> httpError=<403> errorActionName=</errorPath>"
    ingress.bluemix.net/custom-error-actions: |
         errorActionName=</errorAction401>
         #Example custom error snippet
         proxy_pass http://example.com/forbidden.html;
         <EOS>
  spec:
    tls:
    - hosts:
      - mydomain
      secretName: mysecret
    rules:
    - host: mydomain
      http:
        paths:
        - path: /path1
          backend:
            serviceName: app1
            servicePort: 80
        - path: /path2
          backend:
            serviceName: app2
            servicePort: 80
        - path: </errorPath>
          backend:
            serviceName: <error-svc>
            servicePort: 80
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>app1</em>&gt;</code> por el nombre del servicio de Kubernetes al que se aplica el error personalizado. El error personalizado se aplica únicamente a las vías de acceso específicas que utilizan este mismo servicio en sentido ascendente. Si no se establece un nombre de servicio, los errores personalizados se aplican a todas las vías de acceso de servicio.</td>
</tr>
<tr>
<td><code>httpError</code></td>
<td>Sustituya <code>&lt;<em>401</em>&gt;</code> por el código de error HTTP que desea gestionar con una acción de error personalizada.</td>
</tr>
<tr>
<td><code>errorActionName</code></td>
<td>Sustituya <code>&lt;<em>/errorAction401</em>&gt;</code> por el nombre de una acción de error personalizada que se debe llevar o por la vía de acceso a un servicio de error.<ul>
<li>Si especifica el nombre de una acción de error personalizada, debe definir dicha acción de error en un fragmento de código en la anotación <code>custom-error-actions</code>. En el archivo YAML de ejemplo, <code>app1</code> utiliza <code>/errorAction401</code>, que se define en el fragmento de código en la anotación <code>custom-error-actions</code>.</li>
<li>Si especifica la vía de acceso a un servicio de error, debe especificar la vía de acceso de error y el nombre del servicio de error en la sección <code>paths</code>. En el archivo YAML de ejemplo, <code>app2</code> utiliza <code>/errorPath</code>, que se define al final de la sección <code>paths</code>.</li></ul></td>
</tr>
<tr>
<td><code>ingress.bluemix.net/custom-error-actions</code></td>
<td>Defina una acción de error personalizada que debe emprender el ALB para el servicio y el error HTTP que ha especificado. Utilice un fragmento de código NGINX y termine cada fragmento de código con <code>&lt;EOS&gt;</code>. En el archivo YAML de ejemplo, el ALB pasa una página de error personalizada, <code>http://example.com/forbidden.html</code>, al cliente cuando se produce un error <code>401</code> para <code>app1</code>.</td>
</tr>
</tbody></table>

<br />


### Fragmentos de ubicación (`location-snippets`)
{: #location-snippets}

Añada una configuración de bloque de ubicación personalizada para un servicio.
{:shortdesc}

**Descripción**</br>
Un bloque de servidor es una directriz NGINX que define la configuración para el servidor virtual ALB. Un bloque de ubicación es una directriz NGINX definida dentro del bloque de servidor. Los bloques de ubicación definen el modo en que Ingress procesa el URI de solicitud, o la parte de la solicitud que viene después del nombre de dominio o la dirección IP y el puerto.

Cuando un bloque del servidor recibe una solicitud, el bloque de ubicación compara el URI con una vía de acceso y la solicitud se reenvía a la dirección IP del pod en el que se ha desplegado la app. Mediante la anotación `location-snippets`, puede modificar el modo en que el bloque de ubicación reenvía solicitudes a determinados servicios.

Para modificar el bloque del servidor en su totalidad, consulte la anotación [`server-snippets`](#server-snippets).

Para ver los bloques de servidor y de ubicación del archivo de configuración de NGINX, ejecute el siguiente mandato para uno de los pods de ALB: `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**YAML del recurso de Ingress de ejemplo**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/location-snippets: |
      serviceName=<myservice1>
      # Example location snippet
      proxy_request_buffering off;
      rewrite_log on;
      proxy_set_header "x-additional-test-header" "location-snippet-header";
      <EOS>
      serviceName=<myservice2>
      proxy_set_header Authorization "";
      <EOS>
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio que ha creado para la app.</td>
</tr>
<tr>
<td>Fragmento de ubicación</td>
<td>Especifique el fragmento de código de configuración que desea utilizar para el servicio especificado. El fragmento de código de ejemplo del servicio <code>myservice1</code> configura el bloque de ubicación para desactivar la colocación en almacenamiento intermedio de la solicitud de proxy, activar las reescrituras de registro y establecer cabeceras adicionales cuando se reenvía una solicitud al servicio. El fragmento de código de ejemplo del servicio <code>myservice2</code> establece una cabecera <code>Authorization</code> vacía. Cada fragmento de código de ubicación termina con el valor <code>&lt;EOS&gt;</code>.</td>
</tr>
</tbody></table>

<br />


### Direccionamiento de ALB privado (`ALB-ID`)
{: #alb-id}

Direccione las solicitudes entrantes a sus apps con un ALB privado.
{:shortdesc}

**Descripción**</br>
Elija un ALB privado para direccionar las solicitudes entrantes en lugar del ALB público.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/ALB-ID: "<private_ALB_ID_1>;<private_ALB_ID_2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>El ID de su ALB. Para encontrar el ID de ALB privado, ejecute <code>ibmcloud ks albs --cluster &lt;my_cluster&gt;</code>.<p>
Si tiene un clúster multizona con más de un ALB privado habilitado, puede proporcionar una lista de los ID de ALB separados por <code>;</code>. Por ejemplo: <code>ingress.bluemix.net/ALB-ID: &lt;private_ALB_ID_1&gt;;&lt;private_ALB_ID_2&gt;;&lt;private_ALB_ID_3&gt;</code></p>
</td>
</tr>
</tbody></table>

<br />


### Fragmentos de código del servidor (`server-snippets`)
{: #server-snippets}

Añada una configuración de bloque de servidor personalizado.
{:shortdesc}

**Descripción**</br>
Un bloque de servidor es una directriz NGINX que define la configuración para el servidor virtual ALB. Al proporcionar un fragmento de código de configuración personalizado en la anotación `server-snippets`, puede modificar el modo en que el ALB maneja las solicitudes a nivel de servidor.

Para ver los bloques de servidor y de ubicación del archivo de configuración de NGINX, ejecute el siguiente mandato para uno de los pods de ALB: `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**YAML del recurso de Ingress de ejemplo**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/server-snippets: |
      # Example snippet
      location = /health {
      return 200 'Healthy';
      add_header Content-Type text/plain;
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td>Fragmento de código del servidor</td>
<td>Especifique el fragmento de configuración que desea utilizar. Este fragmento de código de ejemplo especifica un bloque de ubicación para manejar las solicitudes <code>/health</code>. El bloque de ubicación está configurado para devolver una respuesta de estado correcto y añadir una cabecera cuando se reenvíe una solicitud.</td>
</tr>
</tbody>
</table>

Puede utilizar la anotación `server-snippets` para añadir una cabecera para todas las respuestas de servicio a nivel de servidor:
{: tip}

```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: codeblock}

<br />


## Anotaciones de conexión
{: #connection}

Con las anotaciones de conexión, puede cambiar el modo en que ALB se conecta a la app de fondo y servidores en sentido ascendente, así como establecer tiempos de espera o un número máximo de conexiones de estado activo antes de que se considere que la app o el servidor no está disponible.
{: shortdesc}

### Tiempos de espera excedidos de conexión y tiempos de espera excedidos de lectura personalizados (`proxy-connect-timeout`, `proxy-read-timeout`)
{: #proxy-connect-timeout}

Defina el tiempo que el ALB espera para conectarse a la app de fondo y para leer información de la misma antes de considerar que la app de fondo no está disponible.
{:shortdesc}

**Descripción**</br>
Cuando se envía una solicitud del cliente al ALB de Ingress, el ALB abre una conexión con la app de fondo. De forma predeterminada, el ALB espera 60 segundos para recibir una respuesta de la app de fondo. Si la app de fondo no responde en un plazo de 60 segundos, la solicitud de conexión se cancela y se considera que la app de fondo no está disponible.

Después de que el ALB se conecte a la app de fondo, lee los datos de la respuesta procedente de la app de fondo. Durante esta operación de lectura, el ALB espera un máximo de 60 segundos entre dos operaciones de lectura para recibir datos de la app de fondo. Si la app de fondo no envía datos en un plazo de 60 segundos, la conexión con la app de fondo se cierra y se considera que la app no está disponible.

60 segundos es el valor predeterminad para el tiempo de espera excedido de conexión (connect-timeout) y el tiempo de espera excedido de lectura (read-timeout) en un proxy y lo recomendable es no modificarlo.

Si la disponibilidad de la app no es estable o si la app es lenta en responder debido a cargas de trabajo elevadas, es posible que desee aumentar el valor de connect-timeout o de read-timeout. Tenga en cuenta que aumentar el tiempo de espera excedido afecta al rendimiento del ALB, ya que la conexión con la app de fondo debe permanecer abierta hasta que se alcanza el tiempo de espera excedido.

Por otro lado, puede reducir el tiempo de espera para aumentar el rendimiento del ALB. Asegúrese de que la app de fondo es capaz de manejar las solicitudes dentro del tiempo de espera especificado, incluso con cargas de trabajo altas.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-connect-timeout: "serviceName=<myservice> timeout=<connect_timeout>"
   ingress.bluemix.net/proxy-read-timeout: "serviceName=<myservice> timeout=<read_timeout>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>&lt;connect_timeout&gt;</code></td>
<td>El número de segundos o minutos que se debe esperar para conectar con la app de fondo, por ejemplo <code>65s</code> o <code>1m</code>. El valor de connect-timeout no puede superar los 75 segundos.</td>
</tr>
<tr>
<td><code>&lt;read_timeout&gt;</code></td>
<td>El número de segundos o minutos que se debe esperar a que se lea la app de fondo, por ejemplo <code>65s</code> o <code>2m</code>.
</tr>
</tbody></table>

<br />


### Solicitudes de estado activo (`keepalive-requests`)
{: #keepalive-requests}

**Descripción**</br>
Establece el número máximo de solicitudes que se pueden servir a través de una conexión en estado activo.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/keepalive-requests: "serviceName=<myservice> requests=<max_requests>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: <myservice>
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app. Este parámetro
es opcional. La configuración se aplica a todos los servicios del subdominio de Ingress, a menos que se especifique un servicio. Si se proporciona el parámetro, se establecen las solicitudes de estado activo para el servicio en cuestión. Si no se proporciona el parámetro, las solicitudes de estado activo se establecen a nivel de servidor de <code>nginx.conf</code> para todos los servicios que no tienen configuradas solicitudes de estado activo.</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>Sustituya <code>&lt;<em>max_requests</em>&gt;</code> por el número máximo de solicitudes que se pueden servir a través de una conexión de estado activo.</td>
</tr>
</tbody></table>

<br />


### Tiempo de espera de estado activo (`keepalive-timeout`)
{: #keepalive-timeout}

**Descripción**</br>
Establece el tiempo máximo que se mantiene abierta una conexión en estado activo en el servidor.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/keepalive-timeout: "serviceName=<myservice> timeout=<time>s"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app. Este parámetro
es opcional. Si se proporciona el parámetro, se establece el tiempo de espera de estado activo para el servicio en cuestión. Si no se proporciona el parámetro, el tiempo de espera de estado activo se establece a nivel de servidor de <code>nginx.conf</code> para todos los servicios que no tienen configurado el tiempo de espera de estado activo.</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Sustituya <code>&lt;<em>time</em>&gt;</code> por una cantidad de tiempo en segundos. Ejemplo: <code>timeout=20s</code>. El valor <code>0</code> inhabilita las conexiones de cliente de estado activo.</td>
</tr>
</tbody></table>

<br />


### Siguiente proxy en sentido ascendente (`proxy-next-upstream-config`)
{: #proxy-next-upstream-config}

Establece cuándo el ALB puede pasar una solicitud al siguiente servidor en sentido ascendente.
{:shortdesc}

**Descripción**</br>
El ALB de Ingress actúa como un proxy entre la app del cliente y la app. Algunas configuraciones de app precisan de varios servidores en sentido ascendente para manejar las solicitudes de cliente entrantes desde el ALB. A veces el servidor proxy que utiliza el ALB no puede establecer una conexión con un servidor en sentido ascendente que utilice la app. El ALB, puede entonces intentar establecer una conexión con el siguiente servidor en sentido ascendente para pasarle en su lugar la solicitud. Puede utilizar la anotación `proxy-next-upstream-config` para definir en qué casos, cuánto tiempo y cuántas veces el ALB puede intentar pasar una solicitud al siguiente servidor en sentido ascendente.

Siempre se
configura un tiempo de espera al utilizar `proxy-next-upstream-config`, de forma que no añada `timeout=true` a esta anotación.
{: note}

**YAML del recurso de Ingress de ejemplo**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-next-upstream-config: "serviceName=<myservice1> retries=<tries> timeout=<time> error=true http_502=true; serviceName=<myservice2> http_403=true non_idempotent=true"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 80
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>Sustituya <code>&lt;<em>tries</em>&gt;</code> con el número máximo de veces que ALB debe intentar pasar una solicitud al siguiente servidor en sentido ascendente. Este valor incluye la solicitud original. Para desactivar esta limitación, utilice <code>0</code>. Si no especifica un valor, se utiliza el valor predeterminado <code>0</code>.
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Sustituya <code>&lt;<em>time</em>&gt;</code> con el máximo de tiempo, en segundos, que ALB debe intentar pasar una solicitud al siguiente servidor en sentido ascendente. Por ejemplo, para establecer un tiempo de 30 segundos, especifique <code>30s</code>. Para desactivar esta limitación, utilice <code>0</code>. Si no especifica un valor, se utiliza el valor predeterminado <code>0</code>.
</td>
</tr>
<tr>
<td><code>error</code></td>
<td>Si se establece en <code>true</code>, el ALB pasa una solicitud al siguiente servidor en sentido ascendente cuando se produce un error al establecer una conexión con el primer servidor en sentido ascendente, pasando una solicitud al mismo, o leyendo la cabecera de respuesta.
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>Si se establece en <code>true</code>, el ALB pasa una solicitud al siguiente servidor en sentido ascendente cuando el primer servidor en sentido ascendente devuelve una respuesta vacía o no válida.
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>Si se establece en <code>true</code>, el ALB pasa una solicitud al siguiente servidor en sentido ascendente cuando el primer servidor en sentido ascendente devuelve una respuesta con el código 502. Puede designar los siguientes códigos de respuesta HTTP: <code>500</code>, <code>502</code>, <code>503</code>, <code>504</code>, <code>403</code>, <code>404</code>, <code>429</code>.
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>Si se establece en <code>true</code>, el ALB puede pasar solicitudes con un método no idempotente al siguiente servidor en sentido ascendente. De forma predeterminada, el ALB no pasa estas solicitudes al siguiente servidor en sentido ascendente.
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>Establezca este valor en <code>true</code> para evitar que el ALB pase solicitudes al siguiente servidor en sentido ascendente.
</td>
</tr>
</tbody></table>

<br />


### Afinidad de sesión con cookies (`sticky-cookie-services`)
{: #sticky-cookie-services}

Utilice la anotación de cookie sticky para añadir afinidad de sesión al ALB y direccionar siempre el tráfico de red entrante al mismo servidor en sentido ascendente.
{:shortdesc}

**Descripción**</br>
Para la alta disponibilidad, es posible que la configuración de la app requiera que despliegue varios servidores en sentido ascendente para que gestionen las solicitudes entrantes del cliente y garanticen una mayor disponibilidad. Cuando un cliente se conecta a la app de fondo, podría ser útil que un cliente recibiera el servicio del mismo servidor en sentido ascendente mientras dure la sesión o durante el tiempo que se tarda en completar una tarea. Puede configurar el ALB de modo que asegure la afinidad de sesión direccionando siempre el tráfico de red de entrada al mismo servidor en sentido ascendente.

El ALB asigna cada cliente que se conecta a la app de fondo a uno de los servidores en sentido ascendente disponibles. El ALB crea una cookie de sesión que se almacena en la app del cliente y que se incluye en la información de cabecera de cada solicitud entre el ALB y el cliente. La información de la cookie garantiza que todas las solicitudes se gestionan en el mismo servidor en sentido ascendente durante todo el periodo de la sesión.

El estado de las sesiones "sticky" puede añadir complejidad y reducir la disponibilidad. Por ejemplo, podría tener un servidor HTTP que mantuviera un estado de sesión durante una conexión inicial de modo que el servicio HTTP solo aceptara posteriores solicitudes con el mismo valor de estado de sesión. Sin embargo, esto impide el escalado horizontal sencillo del servicio HTTP. Considere la posibilidad de utilizar una base de datos externa, como por ejemplo Redis o Memcached, para almacenar el valor de la sesión de solicitud HTTP para que pueda mantener el estado de la sesión en varios servidores.
{: note}

Si incluye varios servicios, utilice únicamente un punto y coma (;) para separarlos.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/sticky-cookie-services: "serviceName=<myservice1> name=<cookie_name1> expires=<expiration_time1> path=<cookie_path1> hash=<hash_algorithm1>;serviceName=<myservice2> name=<cookie_name2> expires=<expiration_time2> path=<cookie_path2> hash=<hash_algorithm2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>Sustituya <code>&lt;<em>cookie_name</em>&gt;</code> por el nombre de una "stickie cookie" que se crea durante una sesión.</td>
</tr>
<tr>
<td><code>expires</code></td>
<td>Sustituya <code>&lt;<em>expiration_time</em>&gt;</code> por el tiempo en segundos (s), minutos (m) u horas (h) antes de que caduque la "sticky cookie". Este tiempo depende de la actividad del usuario. Una vez caducada la cookie, el navegador web del cliente la suprime y se deja de enviar al ALB. Por ejemplo, para establecer un tiempo de caducidad de 1 segundo, 1 minutos o 1 hora, escriba <code>1s</code>, <code>1m</code> o <code>1h</code>.</td>
</tr>
<tr>
<td><code>path</code></td>
<td>Sustituya <code>&lt;<em>cookie_path</em>&gt;</code> por vía de acceso que se añade al subdominio de Ingress y que indica los dominios y subdominios para los que se envía la cookie al ALB. Por ejemplo, si el dominio de Ingress es <code>www.myingress.com</code> y desea enviar la cookie en cada solicitud del cliente, debe establecer <code>path=/</code>. Si desea enviar la cookie solo para <code>www.myingress.com/myapp</code> y todos sus subdominios, establezca este valor en <code>path=/myapp</code>.</td>
</tr>
<tr>
<td><code>hash</code></td>
<td>Sustituya <code>&lt;<em>hash_algorithm</em>&gt;</code> por el algoritmo hash que protege la información en la cookie. Solo se admite <code>sha1</code>. SHA1 crea una suma de hash en función de la información de la cookie y añade esta suma de hash a la cookie. El servidor puede descifrar la información de la cookie y verificar la integridad de los datos.</td>
</tr>
</tbody></table>

<br />


### Tiempo de espera de error en sentido ascendente (`upstream-fail-timeout`)
{: #upstream-fail-timeout}

Establezca el periodo de tiempo durante el cual el ALB puede intentar conectar con el servidor.
{:shortdesc}

**Descripción**</br>
Establezca el periodo de tiempo durante el cual el ALB puede intentar conectar con un servidor antes de que se considere que el servidor no está disponible. Para que se considere que un servidor no está disponible, el ALB debe alcanzar el número máximo de intentos de conexión fallidos definido por la [anotación `upstream-max-fails`](#upstream-max-fails) dentro del periodo de tiempo establecido. Este periodo de tiempo también determina durante cuánto tiempo se considera que el servidor no está disponible.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-fail-timeout: "serviceName=<myservice> fail-timeout=<fail_timeout>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName(Optional)</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
</tr>
<tr>
<td><code>fail-timeout</code></td>
<td>Sustituya <code>&lt;<em>fail_timeout</em>&gt;</code> por el periodo de tiempo que el ALB puede intentar conectar con un servidor antes de que se considere que el servidor no está disponible. El valor predeterminado es <code>10s</code>. El tiempo debe estar en segundos.</td>
</tr>
</tbody></table>

<br />


### Estado activo en sentido ascendente (`upstream-keepalive`)
{: #upstream-keepalive}

Establece el número máximo de conexiones de estado activo para un servidor en sentido ascendente.
{:shortdesc}

**Descripción**</br>
Defina el número máximo de conexiones de estado activo desocupadas para el servidor en sentido ascendente de un servicio determinado. El servidor en sentido ascendente tiene 64 conexiones de estado activo desocupadas de forma predeterminada.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-keepalive: "serviceName=<myservice> keepalive=<max_connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
</tr>
<tr>
<td><code>keepalive</code></td>
<td>Sustituya <code>&lt;<em>max_connections</em>&gt;</code> por el número máximo de conexiones de estado activo desocupadas para el servidor en sentido ascendente. El valor predeterminado es <code>64</code>. Un valor de <code>0</code> inhabilita las conexiones de estado activo en sentido ascendente para el servicio determinado.</td>
</tr>
</tbody></table>

<br />


### Número máximo de errores en sentido ascendente (`upstream-max-fails`)
{: #upstream-max-fails}

Establezca el número máximo de intentos fallidos de comunicarse con el servidor.
{:shortdesc}

**Descripción**</br>
Establezca el número máximo de veces que el ALB puede fallar su intento de conectar con el servidor antes de que se considere que el servidor no está disponible. Para que se considere que el servidor no está disponible, el ALB debe alcanzar el número máximo dentro del intervalo de tiempo establecido por la [anotación `upstream-fail-timeout`](#upstream-fail-timeout). La duración del intervalo en que se considera que el servidor no está disponible también se define con la anotación `upstream-fail-timeout`.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-max-fails: "serviceName=<myservice> max-fails=<max_fails>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName(Optional)</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
</tr>
<tr>
<td><code>max-fails</code></td>
<td>Sustituya <code>&lt;<em>max_fails</em>&gt;</code> por el número máximo de intentos fallidos que puede realizar el ALB para comunicar con el servidor. El valor predeterminado es <code>1</code>. El valor <code>0</code> inhabilita la anotación.</td>
</tr>
</tbody></table>

<br />


## Anotaciones de autenticación TLS/SSL y HTTPS
{: #https-auth}

Con las anotaciones de autenticación TLS/SSL y HTTPS, puede configurar el ALB para el tráfico HTTPS, cambiar los puertos HTTPS predeterminados, habilitar el cifrado SSL para el tráfico que se envía a las apps de fondo o configurar la autenticación mutua.
{: shortdesc}

### Puertos HTTP y HTTPS personalizados (`custom-port`)
{: #custom-port}

Cambie los puertos predeterminados para el tráfico de red HTTP (puerto 80) y HTTPS (puerto 443).
{:shortdesc}

**Descripción**</br>
De forma predeterminada, el ALB de Ingress está configurado para escuchar el tráfico de red de entrada HTTP en el puerto 80 y el tráfico de red de entrada HTTPS en el puerto 443. Puede cambiar los puertos predeterminados para añadir seguridad a su dominio del ALB o para habilitar solo un puerto HTTPS.

Para habilitar la autenticación mutua en un puerto, [configure el ALB de modo que abra el puerto válido](/docs/containers?topic=containers-ingress#opening_ingress_ports) y luego especifique dicho puerto en la [anotación `mutual-auth`](#mutual-auth). No utilice la anotación `custom-port` para especificar un puerto para la autenticación mutua.
{: note}

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/custom-port: "protocol=<protocol1> port=<port1>;protocol=<protocol2> port=<port2>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>Especifique <code>http</code> o <code>https</code> para cambiar el puerto predeterminado para el tráfico de red de entrada HTTP o HTTPS.</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>Especifique el número de puerto que se debe utilizar para el tráfico de red HTTP o HTTPS entrante.  <p class="note">Cuando se especifica un puerto personalizado para HTTP o HTTPS, los puertos predeterminados dejan de ser válidos para HTTP y HTTPS. Por ejemplo, para cambiar el puerto predeterminado de HTTPS por 8443, pero utilizar el puerto predeterminado para HTTP, debe definir ciertos puertos personalizados para ambos: <code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>.</p></td>
 </tr>
</tbody></table>


**Uso**</br>
1. Revise los puertos abiertos en el ALB.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  La salida de la CLI se parecerá a la siguiente:
  ```
  NAME                                             TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. Abra el mapa de configuración del ALB.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. Añada los puertos HTTP y HTTPS no predeterminados al mapa de configuración. Sustituya `<port>` por el puerto HTTP o HTTPS que desea abrir.
  <p class="note">De forma predeterminada, los puertos 80 y 443 están abiertos. Si desea mantener los puertos 80 y 443 abiertos, también debe incluirlos, además de cualquier otro puerto TCP que especifique en el campo `public-ports`. Si ha habilitado un ALB privado, también debe especificar los puertos que desea mantener abiertos en el campo `private-ports`. Para obtener más información, consulte [Apertura de puertos en el ALB de Ingress](/docs/containers?topic=containers-ingress#opening_ingress_ports).</p>
  ```
  apiVersion: v1
  kind: ConfigMap
  data:
    public-ports: <port1>;<port2>
  metadata:
    creationTimestamp: 2017-08-22T19:06:51Z
    name: ibm-cloud-provider-ingress-cm
    namespace: kube-system
    resourceVersion: "1320"
    selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
    uid: <uid>
  ```
  {: codeblock}

4. Verifique que el ALB se ha vuelto a configurar con los puertos no predeterminados.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  La salida de la CLI se parecerá a la siguiente:
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                           AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. Configure Ingress para que utilice los puertos no predeterminados cuando direccione el tráfico de entrada de la red a sus servicios. Utilice la anotación del archivo YAML de ejemplo de esta referencia.

6. Actualice la configuración del ALB.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Abra el navegador web preferido para acceder a la app. Ejemplo: `https://<ibmdomain>:<port>/<service_path>/`

<br />


### Redirecciones HTTP a HTTPS (`redirect-to-https`)
{: #redirect-to-https}

Convierte las solicitudes de cliente HTTP no seguras en HTTPS.
{:shortdesc}

**Descripción**</br>
Puede configurar el ALB de Ingress de modo que proteja el dominio con el certificado TLS proporcionado por IBM o por un certificado TLS personalizado. Es posible que algunos usuarios intenten acceder a sus apps mediante una solicitud `http` no segura al dominio del ALB, por ejemplo `http://www.myingress.com`, en lugar hacerlo de mediante `https`. Puede utilizar la anotación redirect para convertir siempre las solicitudes HTTP no seguras en HTTPS. Si no utiliza esta anotación, las solicitudes HTTP no seguras no se convertirán en solicitud HTTPS de forma predeterminada y se podría exponer información confidencial sin cifrado al público.


La redirección de solicitudes HTTP a HTTPS está inhabilitada de forma predeterminada.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/redirect-to-https: "True"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<br />


### HTTP con seguridad de transporte estricta (`hsts`)
{: #hsts}

**Descripción**</br>
HSTS indica al navegador que acceda sólo a un dominio mediante HTTPS. Incluso si el usuario entra o sigue un enlace HTTP sin formato, el navegador actualiza estrictamente la conexión a HTTPS.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/hsts: enabled=true maxAge=<31536000> includeSubdomains=true
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          ```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>Utilice <code>true</code> para habilitar HSTS.</td>
</tr>
<tr>
<td><code>maxAge</code></td>
<td>Sustituya <code>&lt;<em>31536000</em>&gt;</code> con un entero que represente cuántos segundos almacenará en caché el navegador el envío de solicitudes directamente a HTTPS. El valor predeterminado es <code>31536000</code>, que equivale a 1 año.</td>
</tr>
<tr>
<td><code>includeSubdomains</code></td>
<td>Utilice <code>true</code> para indicar al navegador que la política de HSTS también se aplica a todos los subdominios del dominio actual. El valor predeterminado es <code>true</code>. </td>
</tr>
</tbody></table>

<br />


### Autenticación mutua (`mutual-auth`)
{: #mutual-auth}

Configure la autenticación mutua para el ALB.
{:shortdesc}

**Descripción**</br>
Configure la autenticación mutua para el tráfico en sentido descendente para el ALB de Ingress. El cliente externo autentica el servidor y el servidor también autentica el cliente mediante certificados. La autenticación mutua también se conoce como autenticación basada en certificado o autenticación bidireccional.

Utilice la anotación `mutual-auth` para la terminación SSL entre el cliente y el ALB Ingress. Utilice la [anotación `ssl-services`](#ssl-services) para la terminación SSL entre el ALB de Ingress y la app de fondo.

La anotación de autenticación mutua valida los certificados del cliente. Para reenviar certificados de cliente en una cabecera para que las aplicaciones gestionen la autorización, puede utilizar la siguiente [anotación `proxy-add-headers`](#proxy-add-headers): `"ingress.bluemix.net/proxy-add-headers": "serviceName=router-set {\n X-Forwarded-Client-Cert $ssl_client_escaped_cert;\n}\n"`
{: tip}

**Requisitos previos**</br>

* Debe tener un secreto de autenticación mutua válido que contenga el `ca.crt` necesario. Para crear un secreto de autenticación mutua, consulte los pasos que se indican al final de esta sección.
* Para habilitar la autenticación mutua en un puerto distinto de 443, [configure el ALB para abrir el puerto válido](/docs/containers?topic=containers-ingress#opening_ingress_ports) y luego especifique dicho puerto en esta anotación. No utilice la anotación `custom-port` para especificar un puerto para la autenticación mutua.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/mutual-auth: "secretName=<mysecret> port=<port> serviceName=<servicename1>,<servicename2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>Sustituya <code>&lt;<em>mysecret</em>&gt;</code> por el nombre del recurso de secreto.</td>
</tr>
<tr>
<td><code>port</code></td>
<td>Sustituya <code>&lt;<em>port</em>&gt;</code> con el número de puerto del ALB.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>servicename</em>&gt;</code> con el nombre de uno o varios recursos de Ingress. Este parámetro
es opcional.</td>
</tr>
</tbody></table>

**Para crear un secreto de autenticación mutua:**

1. Genere un certificado de la autoridad de certificados (CA) y una clave de su proveedor de certificados. Si tiene su propio dominio, compre un certificado TLS oficial para el mismo. Asegúrese de que el [CN ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://support.dnsimple.com/articles/what-is-common-name/) sea distinto para cada certificado.
    A efectos de prueba, puede crear un certificado autofirmado mediante OpenSSL. Para obtener más información, consulte esta [guía de aprendizaje sobre certificados SSL autofirmados ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.akadia.com/services/ssh_test_certificate.html) o esta [guía de aprendizaje sobre autenticación mutua que incluye la creación de su propia CA ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/).
    {: tip}
2. [Convierta el certificado a base-64 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.base64encode.org/).
3. Cree un archivo YAML secreto utilizando el certificado.
   ```
   apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       ca.crt: <ca_certificate>
   ```
   {: codeblock}
4. Cree el certificado como un secreto de Kubernetes.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### Soporte de servicios SSL (`ssl-services`)
{: #ssl-services}

Permite las solicitudes HTTPS y cifra el tráfico en las apps ascendentes.
{:shortdesc}

**Descripción**</br>
Si la configuración del recurso Ingress tiene una sección TLS, el ALB Ingress puede manejar solicitudes URL con protección de HTTPS dirigidas a la app. De forma predeterminada, el ALB termina la terminación TLS y descifra la solicitud antes de utilizar el protocolo HTTP para reenviar el tráfico a las apps. Si tiene apps que requieren el protocolo HTTPS y necesita que el tráfico se cifre, utilice la anotación `ssl-services`. Con la anotación `ssl-services`, el ALB termina la conexión TLS externa y, a continuación, crea una nueva conexión SSL entre el ALB y el pod de app. El tráfico se vuelve a cifrar antes de que se envíe a los pods en sentido ascendente.

Si la app de fondo puede manejar TLS y desea añadir seguridad adicional, puede añadir la autenticación unidireccional o mutua proporcionando un certificado que esté contenido en un secreto.

Utilice la anotación `ssl-services` para la terminación SSL entre el ALB de Ingress y la app de fondo. Utilice la [anotación `mutual-auth`](#mutual-auth) para la terminación SSL entre el cliente y el ALB Ingress.
{: tip}

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: <myingressname>
  annotations:
    ingress.bluemix.net/ssl-services: ssl-service=<myservice1> ssl-secret=<service1-ssl-secret>;ssl-service=<myservice2> ssl-secret=<service2-ssl-secret>
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          ```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>ssl-service</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio que precisa HTTPS. Se cifrará el tráfico entre el ALB y este servicio de app.</td>
</tr>
<tr>
<td><code>ssl-secret</code></td>
<td>Si la app de fondo puede manejar TLS y desea añadir seguridad adicional, sustituya <code>&lt;<em>service-ssl-secret</em>&gt;</code> con el secreto de autenticación mutua o unidireccional para el servicio.<ul><li>Si desea proporcionar un secreto de autenticación unidireccional, el valor debe contener el <code>trusted.crt</code> del servidor en sentido ascendente. Para crear un secreto unidireccional, consulte los pasos que se indican al final de esta sección.</li><li>Si desea proporcionar un secreto de autenticación mutua, el valor debe contener los <code>client.crt</code> y <code>client.key</code> necesarios que la app espera del cliente. Para crear un secreto de autenticación mutua, consulte los pasos que se indican al final de esta sección.</li></ul><p class="important">Si no proporciona un secreto, se permiten las conexiones inseguras. Es posible que elija omitir un secreto si desea probar la conexión y no tiene certificados preparados, o si los certificados están caducados y desea permitir conexiones inseguras.</p></td>
</tr>
</tbody></table>


**Para crear un secreto de autenticación unidireccional:**

1. Obtenga la clave y el certificado de la entidad emisora de certificados (CA) del servidor de carga y un certificado del cliente SSL. El ALB de IBM se basa en NGINX, que requiere el certificado raíz, el certificado intermedio y el certificado de programa de fondo. Para obtener más información, consulte la [documentación de NGINX ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.nginx.com/nginx/admin-guide/security-controls/securing-http-traffic-upstream/).
2. [Convierta el certificado a base-64 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.base64encode.org/).
3. Cree un archivo YAML secreto utilizando el certificado.
   ```
   apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       trusted.crt: <ca_certificate>
   ```
   {: codeblock}

   Para imponer también la autenticación mutua para el tráfico en sentido ascendente, puede suministrar un archivo `client.crt` y un archivo `client.key` además de `trusted.crt` en la sección de datos.
   {: tip}

4. Cree el certificado como un secreto de Kubernetes.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

</br>
**Para crear un secreto de autenticación mutua:**

1. Genere un certificado de la autoridad de certificados (CA) y una clave de su proveedor de certificados. Si tiene su propio dominio, compre un certificado TLS oficial para el mismo. Asegúrese de que el [CN ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://support.dnsimple.com/articles/what-is-common-name/) sea distinto para cada certificado.
    A efectos de prueba, puede crear un certificado autofirmado mediante OpenSSL. Para obtener más información, consulte esta [guía de aprendizaje sobre certificados SSL autofirmados ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.akadia.com/services/ssh_test_certificate.html) o esta [guía de aprendizaje sobre autenticación mutua que incluye la creación de su propia CA ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/).
    {: tip}
2. [Convierta el certificado a base-64 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.base64encode.org/).
3. Cree un archivo YAML secreto utilizando el certificado.
   ```
   apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       ca.crt: <ca_certificate>
   ```
   {: codeblock}
4. Cree el certificado como un secreto de Kubernetes.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### Puertos TCP (`tcp-ports`)
{: #tcp-ports}

Acceda a una app a través de un puerto TCP no estándar.
{:shortdesc}

**Descripción**</br>
Utilice esta anotación para una app que esté ejecutando una carga de trabajo con secuencias TCP.

<p class="note">El ALB funciona en modalidad de paso y envía tráfico a las apps de fondo. En este caso, no se soporta la terminación SSL. La conexión TLS no se termina y pasa sin tocarla.</p>

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/tcp-ports: "serviceName=<myservice> ingressPort=<ingress_port> servicePort=<service_port>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio Kubernetes para acceder a través del puerto TCP no estándar.</td>
</tr>
<tr>
<td><code>ingressPort</code></td>
<td>Sustituya <code>&lt;<em>ingress_port</em>&gt;</code> por el puerto TCP en el que desea acceder a la app.</td>
</tr>
<tr>
<td><code>servicePort</code></td>
<td>Este parámetro
es opcional. Cuando se proporciona, el puerto se sustituye por este valor antes de que el tráfico se envíe a la app de fondo. En caso contrario, el puerto permanece igual que el puerto de Ingress. Si no desea establecer este parámetro, puede eliminarlo de la configuración. </td>
</tr>
</tbody></table>


**Uso**</br>
1. Revise los puertos abiertos en el ALB.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  La salida de la CLI se parecerá a la siguiente:
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. Abra el mapa de configuración del ALB.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. Añada los puertos TCP al mapa de configuración. Sustituya `<port>` por los puertos TCP que desee abrir.
  De forma predeterminada, los puertos 80 y 443 están abiertos. Si desea mantener los puertos 80 y 443 abiertos, también debe incluirlos, además de cualquier otro puerto TCP que especifique en el campo `public-ports`. Si ha habilitado un ALB privado, también debe especificar los puertos que desea mantener abiertos en el campo `private-ports`. Para obtener más información, consulte [Apertura de puertos en el ALB de Ingress](/docs/containers?topic=containers-ingress#opening_ingress_ports).
  {: note}
  ```
  apiVersion: v1
  kind: ConfigMap
  data:
    public-ports: 80;443;<port1>;<port2>
  metadata:
    creationTimestamp: 2017-08-22T19:06:51Z
    name: ibm-cloud-provider-ingress-cm
    namespace: kube-system
    resourceVersion: "1320"
    selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
    uid: <uid>
   ```
  {: codeblock}

4. Verifique que el ALB se ha vuelto a configurar con los puertos TCP.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}
  La salida de la CLI se parecerá a la siguiente:
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                               AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. Configure el ALB para acceder a la app a través de un puerto TCP no estándar. Utilice la anotación `tcp-ports` del archivo YAML de ejemplo de esta referencia.

6. Cree el recurso ALB o actualice la configuración de ALB existente.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Ejecute curl sobre el subdominio de Ingress para acceder a la app. Ejemplo: `curl <domain>:<ingressPort>`

<br />


## Anotaciones de direccionamiento de vía de acceso
{: #path-routing}

El ALB de Ingress direcciona el tráfico a las vías de acceso en las que escuchan las apps de fondo. Con las anotaciones de direccionamiento de vía de acceso, puede configurar el modo en que el ALB direcciona el tráfico a las apps.
{: shortdesc}

### Servicios externos (`proxy-external-service`)
{: #proxy-external-service}

Añada definiciones de vía de acceso a servicios externos, como servicios alojados en {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

**Descripción**</br>
Añada definiciones de vía de acceso a servicios externos. Utilice esta anotación sólo si su app opera en un servicio externo en lugar de un servicio de fondo. Cuando se utiliza esta anotación para crear una ruta de servicio externo, sólo las anotaciones `client-max-body-size`, `proxy-read-timeout`, `proxy-connect-timeout` y `proxy-buffering` están soportadas de forma conjunta. Las demás anotaciones no están soportadas de forma conjunta con `proxy-external-service`.

No puede especificar varios hosts para un solo servicio y vía de acceso.
{: note}

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-external-service: "path=<mypath> external-svc=https:<external_service> host=<mydomain>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>path</code></td>
<td>Sustituya <code>&lt;<em>mypath</em>&gt;</code> por la vía de acceso en la que escucha el servicio externo.</td>
</tr>
<tr>
<td><code>external-svc</code></td>
<td>Sustituya <code>&lt;<em>external_service</em>&gt;</code> por el servicio externo al que se debe llamar. Por ejemplo, <code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>.</td>
</tr>
<tr>
<td><code>host</code></td>
<td>Sustituya <code>&lt;<em>mydomain</em>&gt;</code> por el dominio de host para el servicio externo.</td>
</tr>
</tbody></table>

<br />


### Modificador de ubicación (`location-modifier`)
{: #location-modifier}

Modifique la forma en la que el ALB coteja el URI de solicitud con la vía de acceso de la app.
{:shortdesc}

**Descripción**</br>
De forma predeterminada, los ALB procesan las vías de acceso en las que escuchan las apps como prefijos. Cuando un ALB recibe una solicitud para una app, el ALB comprueba el recurso de Ingress para una vía de acceso (como prefijo) que coincide con el comienzo del URI de solicitud. Si se encuentra una coincidencia, la solicitud se reenvía a la dirección IP del pod en el que se ha desplegado la app.

La anotación `location-modifier` cambia la forma en la que el ALB busca coincidencias modificando la configuración de bloques de ubicación. El bloque de ubicación determina cómo se gestionan las solicitudes de vía de acceso de la app.

Para gestionar vías de acceso de expresión regular (regex), es necesaria esta anotación.
{: note}

**Modificadores soportados**</br>
<table>
<caption>Modificadores soportados</caption>
<col width="10%">
<col width="90%">
<thead>
<th>Modificador</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td><code>=</code></td>
<td>El modificador del signo igual provoca que el ALB seleccione sólo coincidencias exactas. Cuando se encuentra una coincidencia exacta, la búsqueda se detiene y se selecciona la vía de acceso coincidente.<br>Por ejemplo, si su app está a la escucha en <code>/tea</code>, el ALB únicamente selecciona las vías de acceso <code>/tea</code> exactamente cuando coinciden con una solicitud para su app.</td>
</tr>
<tr>
<td><code>~</code></td>
<td>El modificador de tilde provoca que el ALB procese las vías de acceso como vía de acceso de expresión regular sensibles a las mayúsculas y minúsculas durante la coincidencia.<br>Por ejemplo, si su app escucha en <code>/coffee</code>, el ALB puede seleccionar las vías de acceso <code>/ab/coffee</code> o <code>/123/coffee</code> al encontrar una solicitud coincidente con su app incluso si las vías de acceso no se han establecido explícitamente para su app.</td>
</tr>
<tr>
<td><code>~\*</code></td>
<td>El modificador de tilde seguida por un asterisco provoca que el ALB procese las vías de acceso como vías de acceso de expresión regular ignorando las mayúsculas y minúsculas durante la coincidencia.<br>Por ejemplo, si su app escucha en <code>/coffee</code>, el ALB puede seleccionar las vías de acceso <code>/ab/Coffee</code> o <code>/123/COFFEE</code> al encontrar una solicitud coincidente con su app incluso si las vías de acceso no se han establecido explícitamente para su app.</td>
</tr>
<tr>
<td><code>^~</code></td>
<td>El modificador de acento circunflejo seguido de provoca que el ALB seleccione la mejor coincidencia que no sea expresión regular en lugar de una vía de acceso de expresión regular.</td>
</tr>
</tbody>
</table>


**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/location-modifier: "modifier='<location_modifier>' serviceName=<myservice1>;modifier='<location_modifier>' serviceName=<myservice2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>modifier</code></td>
<td>Sustituya <code>&lt;<em>location_modifier</em>&gt;</code> con el modificador de ubicación desea utilizar para la vía de acceso. Los modificadores soportados son <code>'='</code>, <code>'~'</code>, <code>'~\*'</code> y <code>'^~'</code>. Debe rodear los modificadores con comillas simples.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
</tr>
</tbody></table>

<br />


### Vías de acceso de reescritura (`rewrite-path`)
{: #rewrite-path}

Direccione el tráfico de red de entrada en una vía de acceso de dominio de ALB a una vía de acceso diferente en la que la app de fondo está a la escucha.
{:shortdesc}

**Descripción**</br>
El dominio del ALB de Ingress direcciona el tráfico de entrada de la red de `mykubecluster.us-south.containers.appdomain.cloud/beans` a la app. La app escucha en `/coffee` en lugar de escuchar en `/beans`. Para reenviar el tráfico de red entrante a la app, añada la anotación de reescritura al archivo de configuración del recurso. La anotación de reescritura garantiza que el tráfico de red entrante de `/beans` se reenvíe a la app utilizando la vía de acceso `/coffee`. Cuando incluya varios servicios, utilice solo un punto y coma (;) sin espacio antes o después del punto y coma para separarlos.

**YAML del recurso de Ingress de ejemplo**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/rewrite-path: "serviceName=<myservice1> rewrite=<target_path1>;serviceName=<myservice2> rewrite=<target_path2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /beans
        backend:
          serviceName: myservice1
          servicePort: 80
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>Sustituya <code>&lt;<em>target_path</em>&gt;</code> por la vía de acceso en la que escucha la app. El tráfico de red de entrada del dominio de ALB se reenvía al servicio Kubernetes mediante esta vía de acceso. La mayoría de las apps no escuchan en una vía de acceso específica, sino que utilizan la vía de acceso raíz y un puerto específico. En el ejemplo de <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code>, la vía de acceso de reescritura es <code>/coffee</code>. <p class= "note">Si aplica este archivo y el URL muestra la respuesta <code>404</code>, la app de fondo podría estar escuchando en una vía de acceso que termina en `/`. Intente añadir un `/` al final en este campo de reescritura y, a continuación, vuelva a aplicar el archivo y vuelva a intentar el URL.</p></td>
</tr>
</tbody></table>

<br />


## Anotaciones de almacenamiento intermedio de proxy
{: #proxy-buffer}

El ALB de Ingress actúa como un proxy entre la app de fondo y el navegador web del cliente. Con las anotaciones de almacenamiento intermedio de proxy, puede configurar cómo se almacenan los datos en el almacenamiento intermedio en el ALB al enviar o recibir paquetes de datos.  
{: shortdesc}

### Almacenamientos intermedios de cabeceras de cliente largas (`large-client-header-buffers`)
{: #large-client-header-buffers}

Establezca el tamaño y el número máximo de los almacenamientos intermedios que leen cabeceras de solicitud de cliente largas.
{:shortdesc}

**Descripción**</br>
Los almacenamientos intermedios que leen cabeceras largas de solicitud de cliente sólo se asignan bajo demanda: si una conexión se cambia al estado activo al final del proceso de la solicitud, estos almacenamientos intermedios se liberan. De forma predeterminada, hay `4` almacenamientos intermedios y el tamaño de almacenamiento intermedio es igual a `8K` bytes. Si una línea de la solicitud supera el tamaño máximo de un almacenamiento intermedio, el cliente recibe el error de HTTP `414 Request-URI Too Large`. Además, si un campo de la cabecera de la solicitud supera el tamaño máximo de un almacenamiento intermedio, el cliente recibe el error `400 Bad Request`. Puede ajustar el tamaño y el número máximo de los almacenamientos intermedios que se utilizan para leer cabeceras de solicitud de cliente largas.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/large-client-header-buffers: "number=<number> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>El número máximo de almacenamientos intermedios que deben asignarse para leer cabeceras largas de solicitud del cliente. Por ejemplo, para establecerlo en 4, defina <code>4</code>.</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>El tamaño máximo de los almacenamientos intermedios que leen cabeceras largas de solicitud del cliente. Por ejemplo, para establecerlo en 16 kilobytes, defina <code>16k</code>. El tamaño debe terminar en <code>k</code> para kilobytes o <code>m</code> para megabytes.</td>
 </tr>
</tbody></table>

<br />


### Colocación en almacenamiento intermedio de datos de respuesta del cliente (`proxy-buffering`)
{: #proxy-buffering}

Utilice la anotación buffer para inhabilitar el almacenamiento de datos de respuesta en el ALB mientras los datos se envían al cliente.
{:shortdesc}

**Descripción**</br>
El ALB de Ingress actúa como un proxy entre la app de fondo y el navegador web del cliente. Cuando se envía una respuesta de la app de fondo al cliente, los datos de la respuesta se colocan en almacenamiento intermedio en el ALB de forma predeterminada. El ALB procesa mediante proxy la respuesta del cliente y empieza a enviar la respuesta al cliente al ritmo del cliente. Cuando el ALB ha recibido todos los datos procedentes de la app de fondo, la conexión con la app de fondo se cierra. La conexión entre el ALB y el cliente permanece abierta hasta que el cliente haya recibido todos los datos.

Si se inhabilita la colocación en almacenamiento intermedio de los datos de la respuesta en el ALB, los datos se envían inmediatamente del ALB al cliente. El cliente debe ser capaz de manejar los datos de entrada al ritmo del ALB. Si el cliente es demasiado lento, la conexión en sentido ascendente permanece abierta hasta que el cliente pueda ponerse al día.

La colocación en almacenamiento intermedio de datos en el ALB está habilitada de forma predeterminada.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-buffering: "enabled=<false> serviceName=<myservice1>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>Para inhabilitar la creación de almacenamientos intermedios de datos de respuesta en el ALB, establezca el valor en <code>false</code>.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code><em>&lt;myservice1&gt;</em></code> por el nombre del servicio de Kubernetes que ha creado para la app. Separe varios servicios con punto y coma (;). Este campo es opcional. Si no especifica un nombre de servicio, todos los servicios utilizarán esta anotación.</td>
</tr>
</tbody></table>

<br />


### Almacenamientos intermedios de proxy (`proxy-buffers`)
{: #proxy-buffers}

Configure el número y el tamaño de los almacenamientos intermedios de proxy para el ALB.
{:shortdesc}

**Descripción**</br>
Establezca el número y el tamaño de los almacenamientos intermedios que leen una respuesta de una única conexión desde el servidor mediante proxy. La configuración se aplica a todos los servicios del subdominio de Ingress, a menos que se especifique un servicio. Por ejemplo, si se especifica una configuración como `serviceName=SERVICE number=2 size=1k`, se aplica 1 k al servicio. Si se especifica una configuración como `number=2 size=1k`, se aplica 1 k a todos los servicios del subdominio de Ingress.</br>
<p class="tip">Si obtiene el mensaje de error `upstream sent too big header while reading response header from upstream`, significa que el servidor en sentido ascendente del programa de fondo ha enviado un tamaño de cabecera mayor que el límite predeterminado. Aumente el límite para `proxy-buffers` y [`proxy-buffer-size`](#proxy-buffer-size).</p>

**YAML del recurso de Ingress de ejemplo**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffers: "serviceName=<myservice> number=<number_of_buffers> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre de un servicio para aplicar almacenamientos intermedios de proxy.</td>
</tr>
<tr>
<td><code>number</code></td>
<td>Sustituya <code>&lt;<em>number_of_buffers</em>&gt;</code> por un número, como <code>2</code>.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Sustituya <code>&lt;<em>size</em>&gt;</code> por el tamaño de cada almacenamiento intermedio en kilobytes (k o K), como <code>1K</code>.</td>
</tr>
</tbody>
</table>

<br />


### Tamaño de almacenamiento intermedio de proxy (`proxy-buffer-size`)
{: #proxy-buffer-size}

Configure el tamaño del almacenamiento intermedio de proxy que lee la primera parte de la respuesta.
{:shortdesc}

**Descripción**</br>
Establezca el tamaño del almacenamiento intermedio que lee la primera parte de la respuesta que se recibe del servidor proxy. Esta parte de la respuesta normalmente contiene una cabecera de respuesta pequeña. La configuración se aplica a todos los servicios del subdominio de Ingress, a menos que se especifique un servicio. Por ejemplo, si se especifica una configuración como `serviceName=SERVICE size=1k`, se aplica 1 k al servicio. Si se especifica una configuración como `size=1k`, se aplica 1 k a todos los servicios del subdominio de Ingress.

Si obtiene el mensaje de error `upstream sent too big header while reading response header from upstream`, significa que el servidor en sentido ascendente del programa de fondo ha enviado un tamaño de cabecera mayor que el límite predeterminado. Aumente el límite para `proxy-buffer-size` y [`proxy-buffers`](#proxy-buffers).
{: tip}

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffer-size: "serviceName=<myservice> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre de un servicio para aplicar proxy-buffers-size.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Sustituya <code>&lt;<em>size</em>&gt;</code> por el tamaño de cada almacenamiento intermedio en kilobytes (k o K), como <code>1K</code>. Para calcular el tamaño adecuado, consulte [esta publicación del blog ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.getpagespeed.com/server-setup/nginx/tuning-proxy_buffer_size-in-nginx). </td>
</tr>
</tbody></table>

<br />


### Tamaño de almacenamientos intermedios de proxy ocupados (`proxy-busy-buffers-size`)
{: #proxy-busy-buffers-size}

Configure el tamaño de los almacenamientos intermedios de proxy que puedan estar ocupados.
{:shortdesc}

**Descripción**</br>
Limite el tamaño de los almacenamientos intermedios que están enviando una respuesta al cliente mientras la respuesta aún no está totalmente leída. Mientras tanto, el resto de los almacenamientos intermedios pueden leer la respuesta y, si es necesario, almacenar en el almacenamiento intermedio parte de la respuesta en un archivo temporal. La configuración se aplica a todos los servicios del subdominio de Ingress, a menos que se especifique un servicio. Por ejemplo, si se especifica una configuración como `serviceName=SERVICE size=1k`, se aplica 1 k al servicio. Si se especifica una configuración como `size=1k`, se aplica 1 k a todos los servicios del subdominio de Ingress.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-busy-buffers-size: "serviceName=<myservice> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
         ```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre de un servicio para aplicar proxy-busy-buffers-size.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Sustituya <code>&lt;<em>size</em>&gt;</code> por el tamaño de cada almacenamiento intermedio en kilobytes (k o K), como <code>1K</code>.</td>
</tr>
</tbody></table>

<br />


## Anotaciones de solicitud y respuesta
{: #request-response}

Utilice anotaciones de solicitud y respuesta para añadir o eliminar información de cabecera de las solicitudes de cliente y servidor, así como para cambiar el tamaño del cuerpo que puede enviar el cliente.
{: shortdesc}

### Adición del puerto del servidor a la cabecera del host (`add-host-port`)
{: #add-host-port}

Añada un puerto de servidor a la solicitud de cliente antes de que se reenvíe la solicitud a la app de fondo.
{: shortdesc}

**Descripción**</br>
Añada `:server_port` a la cabecera de host de una solicitud de cliente antes de reenviar la solicitud a la app de fondo.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/add-host-port: "enabled=<true> serviceName=<myservice>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>Para habilitar el valor de server_port para el subdominio, establézcalo en <code>true</code>.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code><em>&lt;myservice&gt;</em></code> por el nombre del servicio de Kubernetes que ha creado para la app. Separe varios servicios con punto y coma (;). Este campo es opcional. Si no especifica un nombre de servicio, todos los servicios utilizarán esta anotación.</td>
</tr>
</tbody></table>

<br />


### Cabecera de respuesta o solicitud de cliente adicional (`proxy-add-headers`, `response-add-headers`)
{: #proxy-add-headers}

Añada información de cabecera adicional a una solicitud de cliente antes de enviar la solicitud a la app de fondo o a una respuesta del cliente antes de enviar la respuesta al cliente.
{:shortdesc}

**Descripción**</br>
El ALB de Ingress actúa como un proxy entre la app del cliente y la app de fondo. Las solicitudes del cliente que se envían al ALB se procesan (mediante proxy) y se colocan en una nueva solicitud que se envía a la app de fondo. Asimismo, las respuestas de la app de fondo que se envían al ALB se procesan (mediante proxy) y se colocan en una nueva respuesta que se envía al cliente. El proceso mediante proxy de una solicitud o respuesta elimina la información de cabecera HTTP, como por ejemplo el nombre de usuario, que se envió inicialmente desde el cliente o la app de fondo.

Si la app de fondo necesita la información de cabecera HTTP, puede utilizar la anotación `proxy-add-headers` para añadir la información de cabecera a la solicitud del cliente antes de que el ALB reenvíe la solicitud a la app de fondo. Si la app web del cliente necesita la información de cabecera HTTP, puede utilizar la anotación `response-add-headers` para añadir la información de cabecera a la respuesta antes de que el ALB reenvíe la respuesta a la app web del cliente.<br>

Por ejemplo, puede que necesite añadir la información de cabecera X-Forward siguiente a la solicitud antes de reenviarla a la app:
```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```
{: screen}

Para añadir la información de cabecera X-Forward a la solicitud que se envía a la app, utilice la anotación `proxy-add-headers` de la siguiente forma:
```
ingress.bluemix.net/proxy-add-headers: |
  serviceName=<myservice1> {
  Host $host;
  X-Real-IP $remote_addr;
  X-Forwarded-Proto $scheme;
  X-Forwarded-For $proxy_add_x_forwarded_for;
  }
```
{: codeblock}

</br>

La anotación `response-add-headers` no admite cabeceras globales para todos los servicios. Para añadir una cabecera para todas las respuestas de servicio a nivel de servidor, puede utilizar la anotación [`server-snippets`](#server-snippets):
{: tip}
```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: pre}
</br>

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-add-headers: |
      serviceName=<myservice1> {
      <header1> <value1>;
      <header2> <value2>;
      }
      serviceName=<myservice2> {
      <header3> <value3>;
      }
    ingress.bluemix.net/response-add-headers: |
      serviceName=<myservice1> {
      <header1>:<value1>;
      <header2>:<value2>;
      }
      serviceName=<myservice2> {
      <header3>:<value3>;
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>La clave de la información de cabecera que se va a añadir a la solicitud del cliente o a la respuesta del cliente.</td>
</tr>
<tr>
<td><code>&lt;value&gt;</code></td>
<td>El valor de la información de cabecera que se va a añadir a la solicitud del cliente o a la respuesta del cliente.</td>
</tr>
</tbody></table>

<br />


### Eliminación de cabecera de respuesta de cliente (`response-remove-headers`)
{: #response-remove-headers}

Elimine la información de cabecera que se incluye en la respuesta del cliente de la app de fondo antes de que la respuesta se envíe al cliente.
{:shortdesc}

**Descripción**</br>
El ALB de Ingress actúa como un proxy entre la app de fondo y el navegador web del cliente. Las respuestas del cliente procedentes de la app de fondo que se envían al ALB se procesan (mediante proxy) y se colocan en una nueva respuesta que se envía del ALB al navegador web del cliente. Aunque el proceso mediante proxy de una respuesta elimina la información de cabecera http que se envió inicialmente desde la app de fondo, es posible que este proceso no elimine todas las cabeceras específicas de la app de fondo. Elimine la información de cabecera de la respuesta del cliente antes de que la respuesta se reenvíe del ALB al navegador web del cliente.

**YAML del recurso de Ingress de ejemplo**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/response-remove-headers: |
      serviceName=<myservice1> {
      "<header1>";
      "<header2>";
      }
      serviceName=<myservice2> {
      "<header3>";
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>La clave de la cabecera que se eliminará de la respuesta del cliente.</td>
</tr>
</tbody></table>

<br />


### Tamaño de cuerpo de solicitud del cliente (`client-max-body-size`)
{: #client-max-body-size}

Defina el tamaño máximo del cuerpo que puede enviar el cliente como parte de una solicitud.
{:shortdesc}

**Descripción**</br>
Para mantener el rendimiento esperado, el tamaño máximo del cuerpo de la solicitud del cliente se establece en 1 megabyte. Cuando se envía una solicitud del cliente con un tamaño de cuerpo que supera el límite al ALB de Ingress y el cliente no permite dividir los datos, el ALB devuelve al cliente una respuesta HTTP 413 (la entidad de la solicitud es demasiado grande). No se puede establecer una conexión entre el cliente y el ALB hasta que se reduce el tamaño del cuerpo de la solicitud. Si el cliente puede dividir los datos en varios bloques, los datos se dividen en paquetes de 1 megabyte y se envían al ALB.

Es posible que desee aumentar el tamaño máximo del cuerpo porque espera solicitudes del cliente con un tamaño de cuerpo superior a 1 megabyte. Por ejemplo, si desea que el cliente pueda cargar archivos grandes. El hecho de aumentar el tamaño máximo del cuerpo de la solicitud puede afectar el rendimiento del ALB, ya que la conexión con el cliente debe permanecer abierta hasta que se recibe la solicitud.

Algunos navegadores web de cliente no pueden mostrar correctamente el mensaje de respuesta de HTTP 413.
{: note}

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/client-max-body-size: "serviceName=<myservice> size=<size>; size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Opcional: para aplicar un tamaño máximo de cuerpo de cliente a un servicio específico, sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio. Si no especifica un nombre de servicio, el tamaño se aplica a todos los servicios. En el archivo YAML de ejemplo, el formato <code>"serviceName=&lt;myservice&gt; size=&lt;size&gt;; size=&lt;size&gt;"</code> aplica el primer tamaño al servicio <code>myservice</code> y el segundo tamaño a los demás servicios.</li>
</tr>
<td><code>&lt;size&gt;</code></td>
<td>El tamaño máximo del cuerpo de la respuesta del cliente. Por ejemplo, para establecer el tamaño máximo en 200 megabytes, defina <code>200m</code>. Puede establecer el tamaño en 0 para inhabilitar la comprobación del tamaño del cuerpo de la solicitud del cliente.</td>
</tr>
</tbody></table>

<br />


## Anotaciones de límite de servicio
{: #service-limit}

Con las anotaciones de límite de servicio, puede cambiar la velocidad de proceso de solicitudes predeterminada y el número de conexiones que pueden proceder de una única dirección IP.
{: shortdesc}

### Límites de velocidad global (`global-rate-limit`)
{: #global-rate-limit}

Limite la velocidad de procesamiento de solicitudes y el número de conexiones por una clave definida para todos los servicios.
{:shortdesc}

**Descripción**</br>
Para todos los servicios, limite la velocidad de servicio de solicitudes y el número de conexiones por una clave definida que provienen de una única dirección IP para todas las vías de acceso de los programas de fondo seleccionados.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/global-rate-limit: "key=<key> rate=<rate> conn=<number-of-connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>key</code></td>
<td>Los valores admitidos son `location`, cabeceras `$http_` y `$uri`. Para establecer un límite global para las solicitudes entrantes basado en la zona o servicio, utilice `key=location`. Para establecer un límite global para las solicitudes entrantes basado en la cabecera, utilice `X-USER-ID key=$http_x_user_id`.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>Sustituya <code>&lt;<em>rate</em>&gt;</code> por la velocidad de procesamiento. Especifique un valor como velocidad por segundo (r/s) o velocidad por minuto (r/m). Por ejemplo: <code>50r/m</code>.</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>Sustituya <code>&lt;<em>number-of-connections</em>&gt;</code> por el número de conexiones.</td>
</tr>
</tbody></table>

<br />


### Límites de velocidad de servicio (`service-rate-limit`)
{: #service-rate-limit}

Limita la velocidad de procesamiento de solicitudes y el número de conexiones para servicios específicos.
{:shortdesc}

**Descripción**</br>
Para servicios específicos, limite la velocidad de servicio de solicitudes y el número de conexiones por una clave definida que provienen de una única dirección IP para todas las vías de acceso de los programas de fondo seleccionados.

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/service-rate-limit: "serviceName=<myservice> key=<key> rate=<rate> conn=<number_of_connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio para el que desea limitar la velocidad de procesamiento.</li>
</tr>
<tr>
<td><code>key</code></td>
<td>Los valores admitidos son `location`, cabeceras `$http_` y `$uri`. Para establecer un límite global para las solicitudes entrantes basado en la zona o servicio, utilice `key=location`. Para establecer un límite global para las solicitudes entrantes basado en la cabecera, utilice `X-USER-ID key=$http_x_user_id`.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>Sustituya <code>&lt;<em>rate</em>&gt;</code> por la velocidad de procesamiento. Para definir una velocidad por segundo, utilice r/s: <code>10r/s</code>. Para definir una velocidad por minuto, utilice r/m: <code>50r/m</code>.</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>Sustituya <code>&lt;<em>number-of-connections</em>&gt;</code> por el número de conexiones.</td>
</tr>
</tbody></table>

<br />


## Anotaciones de autenticación de usuario
{: #user-authentication}

Utilice anotaciones de autenticación de usuario si desea utilizar {{site.data.keyword.appid_full_notm}} para autenticarse con las apps.
{: shortdesc}

### Autenticación de {{site.data.keyword.appid_short_notm}} (`appid-auth`)
{: #appid-auth}

Utilice {{site.data.keyword.appid_full_notm}} para autenticarse con la app.
{:shortdesc}

**Descripción**</br>
Autentique las solicitudes HTTP/HTTPS de API o web con {{site.data.keyword.appid_short_notm}}.

Si establece el tipo de solicitud en web, se valida una solicitud web que contiene una señal de acceso de {{site.data.keyword.appid_short_notm}}. Si la validación de señal falla, la solicitud web es rechazada. Si la solicitud no contiene ninguna señal de acceso, la solicitud se redirige a la página de inicio de sesión de {{site.data.keyword.appid_short_notm}}. Para que funcione la autenticación web de {{site.data.keyword.appid_short_notm}}, las cookies deben estar habilitadas en el navegador del usuario.

Si establece el tipo de solicitud en api, se valida una solicitud de API que contiene una señal de acceso de {{site.data.keyword.appid_short_notm}}. Si la solicitud no contiene ninguna señal de acceso, el usuario recibe un mensaje de error 401: Unauthorized.

Por razones de seguridad, la autenticación de {{site.data.keyword.appid_short_notm}} solo da soporte a sistemas de fondo que tenga TLS/SSL habilitado.
{: note}

**YAML del recurso de Ingress de ejemplo**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/appid-auth: "bindSecret=<bind_secret> namespace=<namespace> requestType=<request_type> serviceName=<myservice> [idToken=false]"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>bindSecret</code></td>
<td>Sustituya <em><code>&lt;bind_secret&gt;</code></em> por el secreto de Kubernetes que almacena el secreto de enlace correspondiente a la instancia de servicio de {{site.data.keyword.appid_short_notm}}.</td>
</tr>
<tr>
<td><code>namespace</code></td>
<td>Sustituya <em><code>&lt;namespace&gt;</code></em> con el espacio de nombres del secreto de enlace. El valor predeterminado de este campo es el espacio de nombres `default`.</td>
</tr>
<tr>
<td><code>requestType</code></td>
<td>Sustituya <code><em>&lt;request_type&gt;</em></code> con el tipo de solicitud que desea enviar a {{site.data.keyword.appid_short_notm}}. Los valores aceptados son `web` o `api`. El valor predeterminado es `api`.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code><em>&lt;myservice&gt;</em></code> por el nombre del servicio de Kubernetes que ha creado para la app. Este campo es obligatorio. Si no se incluye un nombre de servicio, la anotación se habilita para todos los servicios.  Si se incluye un nombre de servicio, la anotación se habilita solo para ese servicio. Separe varios servicios con una
coma (,).</td>
</tr>
<tr>
<td><code>idToken=false</code></td>
<td>Opcional: el cliente OIDC de Liberty no puede analizar a la vez el acceso y la señal de identidad. Al trabajar con Liberty, establezca este valor en false para que la señal de identidad no se envíe al servidor de Liberty.</td>
</tr>
</tbody></table>

**Uso**</br>

Puesto que la app utiliza {{site.data.keyword.appid_short_notm}} para la autenticación, debe suministrar una instancia de {{site.data.keyword.appid_short_notm}}, configurar la instancia con URI de redirección válidos y generar un secreto de enlace vinculando la instancia al clúster.

1. Elija una existente o cree una nueva instancia de {{site.data.keyword.appid_short_notm}}.
  * Para utilizar una instancia existente, asegúrese de que el nombre de la instancia de servicio no contenga espacios. Para eliminar espacios, seleccione el menú de más opciones que hay junto al nombre de la instancia de servicio y seleccione **Cambiar nombre de servicio**.
  * Para suministrar una [nueva instancia de {{site.data.keyword.appid_short_notm}}](https://cloud.ibm.com/catalog/services/app-id):
      1. Sustituya el valor de **Nombre de servicio** que se suministra por su propio nombre exclusivo correspondiente a la instancia de servicio. El nombre de la instancia de servicio de puede contener espacios.
      2. Elija la región en la que se ha desplegado el clúster.
      3. Pulse **Crear**.

2. Añada los URL de redirección para la app. Un URL de redirección es el punto final de devolución de llamada de la app. Para evitar ataques de suplantación, el ID de app valida el URL de solicitud comparándolo con la lista blanca de los URL de redirección.
  1. En la consola de gestión de {{site.data.keyword.appid_short_notm}}, vaya a **Gestionar autenticación**.
  2. En el separador **Proveedores de identidades**, asegúrese de tener seleccionado un proveedor de identidades. Si no se ha seleccionado ningún proveedor de identidad, el usuario no se autenticará, pero se emitirá una señal de acceso para el acceso anónimo a la app.
  3. En el separador **Valores de autenticación**, añada el URL de redirección para la app en el formato `http://<hostname>/<app_path>/appid_callback` o `https://<hostname>/<app_path>/appid_callback`.

    {{site.data.keyword.appid_full_notm}} ofrece una función de cierre de sesión: si aparece `/logout` en la vía de acceso de {{site.data.keyword.appid_full_notm}}, las cookies se eliminan y se envía al usuario a la página de inicio de sesión. Para utilizar esta función, debe añadir `/appid_logout` a su dominio en el formato `https://<hostname>/<app_path>/appid_logout` y debe incluir este URL en la lista de URL de redirección.
    {: note}

3. Enlace la instancia de servicio de {{site.data.keyword.appid_short_notm}} al clúster. El mandato crea una clave de servicio para la instancia de servicio, o bien puede incluir el distintivo `--key` para utilizar las credenciales de clave de servicio existentes.
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>]
  ```
  {: pre}
  Cuando el servicio se haya añadido correctamente al clúster, se crea un secreto de clúster que contiene las credenciales de la instancia de servicio. Ejemplo de salida de CLI:
  ```
  ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service appid1
    Binding service instance to namespace...
    OK
    Namespace:    mynamespace
    Secret name:  binding-<service_instance_name>
  ```
  {: screen}

4. Obtenga el secreto que se ha creado en el espacio de nombres del clúster.
  ```
  kubectl get secrets --namespace=<namespace>
  ```
  {: pre}

5. Utilice el secreto de enlace y el espacio de nombres del clúster para añadir la anotación `appid-auth` al recurso de Ingress.


