---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# Registro y supervisión de Ingress
{: #ingress_health}

Personalice el registro y configure la supervisión para ayudarle a resolver los problemas y a mejorar el rendimiento de la configuración de Ingress.
{: shortdesc}

## Visualización de registros de Ingress
{: #ingress_logs}

Si desea resolver problemas de Ingress o supervisar la actividad de Ingress, puede revisar los registros de Ingress.
{: shortdesc}

Los registros se recopilan automáticamente para los ALB de Ingress. Para ver los registros de ALB, elija entre dos opciones.
* [Cree una configuración de registro para el servicio Ingress](/docs/containers?topic=containers-health#configuring) en el clúster.
* Consulte los registros desde la CLI. **Nota**: Debe tener al menos el [rol de servicio **Lector** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre el espacio de nombres `kube-system`.
    1. Obtenga el ID de un pod para un ALB.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. Abra los registros correspondientes a dicho pod ALB.
        ```
        kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

</br>El contenido de registro de Ingress predeterminado se formatea en JSON y muestra los campos comunes que describen la sesión de conexión entre un cliente y la app. Un registro de ejemplo con los campos predeterminados tiene el aspecto siguiente:

```
{"time_date": "2018-08-21T17:33:19+00:00", "client": "108.162.248.42", "host": "albhealth.multizone.us-south.containers.appdomain.cloud", "scheme": "http", "request_method": "GET", "request_uri": "/", "request_id": "c2bcce90cf2a3853694da9f52f5b72e6", "status": 200, "upstream_addr": "192.168.1.1:80", "upstream_status": 200, "request_time": 0.005, "upstream_response_time": 0.005, "upstream_connect_time": 0.000, "upstream_header_time": 0.005}
```
{: screen}

<table>
<caption>Campos en el formato de registro de Ingress predeterminado</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Campos en el formato de registro de Ingress predeterminado</th>
</thead>
<tbody>
<tr>
<td><code>"time_date": "$time_iso8601"</code></td>
<td>La hora local en el formato estándar ISO 8601 cuando se escribe el registro.</td>
</tr>
<tr>
<td><code>"client": "$remote_addr"</code></td>
<td>La dirección IP del paquete de la solicitud que el cliente ha enviado a la app. Esta dirección IP puede cambiar en función de las situaciones siguientes:<ul><li>Cuando una solicitud de cliente para su app se envía a su clúster, la solicitud se direcciona a un pod para el servicio de equilibrador de carga que expone el ALB. Si no existen pods de app en el mismo nodo trabajador que el pod del servicio equilibrador de carga, el equilibrador de carga reenvía las solicitudes a un pod de app en un nodo trabajador diferente. La dirección IP de origen del paquete de la solicitud se cambia por la dirección IP pública del nodo trabajador en el que se ejecuta el pod de la app.</li><li>Si [la conservación de IP de origen está habilitada](/docs/containers?topic=containers-ingress#preserve_source_ip), en su lugar se registra la dirección IP original de la solicitud de cliente a la app.</li></ul></td>
</tr>
<tr>
<td><code>"host": "$http_host"</code></td>
<td>El host, o subdominio, a través del que se puede acceder a las apps. Este subdominio se configura en los archivos de recursos de Ingress de los ALB.</td>
</tr>
<tr>
<td><code>"scheme": "$scheme"</code></td>
<td>El tipo de solicitud: <code>HTTP</code> o <code>HTTPS</code>.</td>
</tr>
<tr>
<td><code>"request_method": "$request_method"</code></td>
<td>El método de la llamada de solicitud a la app de fondo, como por ejemplo <code>GET</code> o <code>POST</code>.</td>
</tr>
<tr>
<td><code>"request_uri": "$uri"</code></td>
<td>El URI de solicitud original a la vía de acceso de la app. Los ALB procesan las vías de acceso en las que escuchan las apps como prefijos. Cuando un ALB recibe una solicitud de un cliente a una app, el ALB comprueba el recurso de Ingress para una vía de acceso (como prefijo) que coincide con la vía de acceso del URI de solicitud.</td>
</tr>
<tr>
<td><code>"request_id": "$request_id"</code></td>
<td>Un identificador de solicitud exclusivo generado a partir de 16 bytes aleatorios.</td>
</tr>
<tr>
<td><code>"status": $status</code></td>
<td>El código de estado para la sesión de conexión.<ul>
<li><code>200</code>: la sesión se ha completado correctamente</li>
<li><code>400</code>: los datos de cliente no se pueden analizar</li>
<li><code>403</code>: acceso prohibido; por ejemplo, cuando el acceso está limitado para determinadas direcciones IP de cliente</li>
<li><code>500</code>: error de servidor interno</li>
<li><code>502</code>: pasarela errónea; por ejemplo, si un servidor en sentido ascendente no se puede seleccionar o alcanzar</li>
<li><code>503</code>: servicio no disponible; por ejemplo, cuando el acceso está limitado por el número de conexiones</li>
</ul></td>
</tr>
<tr>
<td><code>"upstream_addr": "$upstream_addr"</code></td>
<td>La dirección IP y el puerto o la vía de acceso al socket de dominio de UNIX del servidor en sentido ascendente. Si se contacta con varios servidores durante el proceso de solicitud, sus direcciones se separan mediante comas: <code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock"</code>. Si la solicitud se redirige internamente de un grupo de servidores a otro, las direcciones de servidor de distintos grupos se separan por dos puntos: <code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock : 192.168.10.1:80, 192.168.10.2:80"</code>. Si el ALB no puede seleccionar un servidor, en su lugar se registra el nombre del grupo de servidores.</td>
</tr>
<tr>
<td><code>"upstream_status": $upstream_status</code></td>
<td>El código de estado de la respuesta que se obtiene del servidor en sentido ascendente para la app de fondo, como códigos de respuesta HTTP estándar. Los códigos de estado de varias respuestas se separan mediante comas y puntos, como las direcciones en la variable <code>$upstream_addr</code>. Si el ALB no puede seleccionar un servidor, se registra el código de estado 502 (Pasarela errónea).</td>
</tr>
<tr>
<td><code>"request_time": $request_time</code></td>
<td>El tiempo de proceso de la solicitud, medido en segundos con una resolución de milisegundos. Este tiempo empieza cuando el ALB lee los primeros bytes de la solicitud de cliente y se detiene cuando el ALB envía los últimos bytes de la respuesta al cliente. El registro se escribe inmediatamente después de que se detenga el tiempo de proceso de la solicitud.</td>
</tr>
<tr>
<td><code>"upstream_response_time": $upstream_response_time</code></td>
<td>El tiempo que tarda el ALB en recibir la respuesta del servidor en sentido ascendente para la app de fondo, medido en segundos con una resolución de milisegundos. Los tiempos de varias respuestas se separan mediante comas y puntos, como las direcciones en la variable <code>$upstream_addr</code>.</td>
</tr>
<tr>
<td><code>"upstream_connect_time": $upstream_connect_time</code></td>
<td>El tiempo que tarda el ALB en establecer una conexión con el servidor en sentido ascendente para la app de fondo, medido en segundos con una resolución de milisegundos. Si TLS/SSL está habilitado en la configuración de recursos de Ingress, este tiempo incluye el tiempo empleado en el reconocimiento. Los tiempos de varias conexiones se separan mediante comas y puntos, como las direcciones en la variable <code>$upstream_addr</code>.</td>
</tr>
<tr>
<td><code>"upstream_header_time": $upstream_header_time</code></td>
<td>El tiempo que tarda el ALB en recibir la cabecera de respuesta del servidor en sentido ascendente para la app de fondo, medido en segundos con una resolución de milisegundos. Los tiempos de varias conexiones se separan mediante comas y puntos, como las direcciones en la variable <code>$upstream_addr</code>.</td>
</tr>
</tbody></table>

## Personalización del formato y el contenido del registro
{: #ingress_log_format}

Existe la posibilidad de personalizar el contenido y los formatos de los registros que se recopilan para el ALB de Ingress.
{:shortdesc}

De forma predeterminada, los registros de Ingress están en formato JSON y visualizan los campos más comunes del registro. Sin embargo, también puede crear un formato de registro personalizado eligiendo los componentes de registro que se reenvían y cómo se disponen los componentes en la salida de registro

Antes de empezar, asegúrese de que tiene el [rol de servicio **Escritor** o **Gestor** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre el espacio de nombres `kube-system`.

1. Edite el archivo de configuración del recurso del mapa de configuración `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Añada una sección <code>data</code>. Añada el campo `log-format` y, si lo desea, el campo `log-format-escape-json`.

    ```
    apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    <table>
    <caption>Componentes del archivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de la configuración del formato de registro (log-format)</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>Sustituya <code>&lt;key&gt;</code> por el nombre del componente de registro y <code>&lt;log_variable&gt;</code> por una variable correspondiente al componente de registro que desea recopilar en las entradas de registro. Puede incluir el texto y la puntuación que desea que contenga la entrada de registro, como por ejemplo comillas alrededor de valores de serie y comas para separar los componentes de registro. Por ejemplo, si formatea un componente como <code>request: "$request"</code>, se genera la siguiente entrada de registro: <code>request: "GET / HTTP/1.1"</code>. Para ver una lista de todas las variables que puede utilizar, consulte el <a href="http://nginx.org/en/docs/varindex.html">Índice de variables NGINX</a>.<br><br>Para registrar una cabecera adicional, como por ejemplo <em>x-custom-ID</em>, añada el siguiente par de clave-valor al contenido de registro personalizado: <br><pre class="codeblock"><code>customID: $http_x_custom_id</code></pre> <br>Los guiones (<code>-</code>) se convierten en signos de subrayado (<code>_</code>) y se debe anteponer <code>$http_</code> al nombre personalizado de cabecera.</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>Opcional: De forma predeterminada, los registros se generan en formato de texto. Para generar registros en formato JSON, añada el campo <code>log-format-escape-json</code> y utilice el valor <code>true</code>.</td>
    </tr>
    </tbody></table>

    Por ejemplo, el formato de registro puede contener las variables siguientes:
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    Una entrada de registro de acuerdo con este formato tiene el aspecto del ejemplo siguiente:
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    Para crear un formato de registro personalizado basado en el formato predeterminado de los registros de ALB, modifique la siguiente sección en consecuencia y añádala al mapa de configuración:
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. Guarde el archivo de configuración.

5. Verifique que se hayan aplicado los cambios del mapa de configuración.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

4. Para ver los registros de ALB de Ingress, elija entre dos opciones.
    * [Cree una configuración de registro para el servicio Ingress](/docs/containers?topic=containers-health#logging) en el clúster.
    * Consulte los registros desde la CLI.
        1. Obtenga el ID de un pod para un ALB.
            ```
            kubectl get pods -n kube-system | grep alb
            ```
            {: pre}

        2. Abra los registros correspondientes a dicho pod ALB. Verifique que los registros siguen el formato actualizado.
            ```
            kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
            ```
            {: pre}

<br />


## Supervisión de los ALB de Ingress
{: #ingress_monitoring}

Para supervisar los ALB, despliegue un exportador de métricas y el agente Prometheus en el clúster.
{: shortdesc}

El exportador de métricas de ALB utiliza la directriz de NGINX, `vhost_traffic_status_zone`, para recopilar datos de métricas del punto final `/status/format/json` en cada pod de ALB de Ingress. El exportador de métricas reformatea automáticamente cada campo de datos del archivo JSON en una métrica que pueda leer Prometheus. A continuación, un agente Prometheus recopila las métricas generadas por el exportador y hace que las métricas sean visibles en un panel de control de Prometheus.

### Instalación del diagrama de Helm de exportador de métricas
{: #metrics-exporter}

Instale el diagrama de Helm de exportador de métricas para supervisar un ALB en el clúster.
{: shortdesc}

Las pods del exportador de métricas de ALB se deben desplegar en los mismos nodos trabajadores en los que se han desplegado los ALB. Si los ALB se ejecutan en nodos trabajadores de extremo, y dichos nodos de extremo se han definido como antagónicos para evitar otros despliegues de carga de trabajo, no se pueden planificar los pods del exportador de métricas. Debe eliminar los antagonismos con el mandato `kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-`.
{: note}

1.  **Importante**: [Siga las instrucciones](/docs/containers?topic=containers-helm#public_helm_install) para instalar el cliente de Helm en la máquina local, instale el servidor Helm (tiller) con una cuenta de servicio y añada los repositorios de Helm de {{site.data.keyword.Bluemix_notm}}.

2. Instale el diagrama de Helm `ibmcloud-alb-metrics-exporter` en el clúster. Este diagrama de Helm despliega un exportador de métricas de ALB y crea una cuenta de servicio `alb-metrics-service-account` en el espacio de nombres `kube-system`. Sustituya <alb-ID> por el ID del ALB para el que desea recopilar métricas. Para ver los ID de los ALB del clúster, ejecute <code>ibmcloud ks albs --cluster &lt;cluster_name&gt;</code>.
  Debe desplegar un gráfico para cada ALB que desee supervisar.
  {: note}
  ```
  helm install iks-charts/ibmcloud-alb-metrics-exporter --name ibmcloud-alb-metrics-exporter --set metricsNameSpace=kube-system --set albId=<alb-ID>
  ```
  {: pre}

3. Compruebe el estado de despliegue del diagrama. Cuando el diagrama está listo, el campo **STATUS**, situado cerca de la parte superior de la salida, tiene el valor `DEPLOYED`.
  ```
  helm status ibmcloud-alb-metrics-exporter
  ```
  {: pre}

4. Verifique que los pods `ibmcloud-alb-metrics-exporter` se están ejecutando.
  ```
  kubectl get pods -n kube-system -o wide
  ```
  {:pre}

  Salida de ejemplo:
  ```
  NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
  ...
  alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  ```
  {:screen}

5. Opcional: [Instale el agente Prometheus](#prometheus-agent) para recopilar las métricas generadas por el exportador y hacer que sean visibles en un panel de control de Prometheus.

### Instalación del diagrama de Helm del agente Prometheus
{: #prometheus-agent}

Después de instalar el [exportador de métricas](#metrics-exporter), puede instalar el diagrama de Helm del agente Prometheus para recopilar las métricas generadas por el exportador y hacer que sean visibles en un panel de control de Prometheus.
{: shortdesc}

1. Descargue el archivo TAR para el exportador de métricas del diagrama de Helm de https://icr.io/helm/iks-charts/charts/ibmcloud-alb-metrics-exporter-1.0.7.tgz

2. Navegue hasta la subcarpeta de Prometheus.
  ```
  cd ibmcloud-alb-metrics-exporter-1.0.7.tar/ibmcloud-alb-metrics-exporter/subcharts/prometheus
  ```
  {: pre}

3. Instale el diagrama de Helm de Prometheus en el clúster. Sustituya <ingress_subdomain> por el subdominio de Ingress del clúster. El URL del panel de control de Prometheus es una combinación del subdominio Prometheus predeterminado, `prom-dash` y su subdominio de Ingress, por ejemplo `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`. Para encontrar el subdominio de Ingress correspondiente a su clúster, ejecute <code>ibmcloud ks cluster-get --cluster &lt;cluster_name&gt;</code>.
  ```
  helm install --name prometheus . --set nameSpace=kube-system --set hostName=prom-dash.<ingress_subdomain>
  ```
  {: pre}

4. Compruebe el estado de despliegue del diagrama. Cuando el diagrama está listo, el campo **STATUS**, situado cerca de la parte superior de la salida, tiene el valor `DEPLOYED`.
    ```
    helm status prometheus
    ```
    {: pre}

5. Verifique que el pod `prometheus` está en ejecución.
    ```
    kubectl get pods -n kube-system -o wide
    ```
    {:pre}

    Salida de ejemplo:
    ```
    NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
    alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    prometheus-9fbcc8bc7-2wvbk                       1/1       Running     0          1m        172.30.xxx.xxx   10.xxx.xx.xxx
    ```
    {:screen}

6. En un navegador, especifique el URL del panel de control de Prometheus. Este nombre de host tiene el formato `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`. Se abre el panel de control de Prometheus para el ALB.

7. Revise más información sobre las métricas [ALB](#alb_metrics), [server](#server_metrics) y [upstream](#upstream_metrics) mostradas en el panel de control.

### Métricas de ALB
{: #alb_metrics}

`alb-metrics-exporter` reformatea automáticamente cada campo de datos del archivo JSON en una métrica que pueda leer Prometheus. Las métricas de ALB recopilan datos sobre las conexiones y las respuestas que maneja el ALB.
{: shortdesc}

Las métricas de ALB están en el formato `kube_system_<ALB-ID>_<METRIC-NAME> <VALUE>`. Por ejemplo, si un ALB recibe 23 respuestas con códigos de estado de nivel 2xx, las métricas se formatean como `kube_system_public_crf02710f54fcc40889c301bfd6d5b77fe_alb1_totalHandledRequest {.. metric="2xx"} 23`, donde `metric` es la etiqueta de prometheus.

En la tabla siguiente se muestran los nombres de las métricas de ALB admitidas con las etiquetas de métrica en el formato `<ALB_metric_name>_<metric_label>`
<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono idea"/> Métricas de ALB admitidas</th>
</thead>
<tbody>
<tr>
<td><code>connections_reading</code></td>
<td>El número total de conexiones de cliente de lectura.</td>
</tr>
<tr>
<td><code>connections_accepted</code></td>
<td>El número total de conexiones de cliente aceptadas.</td>
</tr>
<tr>
<td><code>connections_active</code></td>
<td>El número total de conexiones de cliente activas.</td>
</tr>
<tr>
<td><code>connections_handled</code></td>
<td>El número total de conexiones de cliente manejadas.</td>
</tr>
<tr>
<td><code>connections_requests</code></td>
<td>El número total de conexiones de cliente solicitadas.</td>
</tr>
<tr>
<td><code>connections_waiting</code></td>
<td>El número total de conexiones de cliente en espera.</td>
</tr>
<tr>
<td><code>connections_writing</code></td>
<td>El número total de conexiones de cliente de escritura.</td>
</tr>
<tr>
<td><code>totalHandledRequest_1xx</code></td>
<td>El número de respuestas con códigos de estado 1xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_2xx</code></td>
<td>El número de respuestas con códigos de estado 2xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_3xx</code></td>
<td>El número de respuestas con códigos de estado 3xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_4xx</code></td>
<td>El número de respuestas con códigos de estado 4xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_5xx</code></td>
<td>El número de respuestas con códigos de estado 5xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_total</code></td>
<td>El número total de solicitudes de cliente recibidas de los clientes.</td>
  </tr></tbody>
</table>

### Métricas de servidor
{: #server_metrics}

`alb-metrics-exporter` reformatea automáticamente cada campo de datos del archivo JSON en una métrica que pueda leer Prometheus. Las métricas de servidor recopilan datos del subdominio definido en un recurso de Ingress; por ejemplo, `dev.demostg1.stg.us.south.containers.appdomain.cloud`.
{: shortdesc}

Las métricas de servidor están en el formato `kube_system_server_<ALB-ID>_<SUB-TYPE>_<SERVER-NAME>_<METRIC-NAME> <VALUE>`.

`<SERVER-NAME>_<METRIC-NAME>` se formatean como etiquetas. Por ejemplo, `albId="dev_demostg1_us-south_containers_appdomain_cloud",metric="out"`

Por ejemplo, si el servidor ha enviado un total de 22319 bytes a los clientes, la métrica se formatea del siguiente modo:
```
kube_system_server_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="dev_demostg1_us-south_containers_appdomain_cloud",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="out",pod_template_hash="3805183417"} 22319
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general del formato de métricas del servidor</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>El ID de ALB. En el ejemplo anterior, el ID de ALB es <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>.</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>El subtipo de métrica. Cada subtipo corresponde a uno o varios nombres de métricas.
<ul>
<li><code>bytes</code> y <code>processing\_time</code> corresponden a las métricas <code>in</code> y <code>out</code>.</li>
<li><code>cache</code> corresponde a las métricas <code>bypass</code>, <code>expired</code>, <code>hit</code>, <code>miss</code>, <code>revalidated</code>, <code>scare</code>, <code>stale</code> y <code>updating</code>.</li>
<li><code>requests</code> corresponde a las métricas <code>requestMsec</code>, <code>1xx</code>, <code>2xx</code>, <code>3xx</code>, <code>4xx</code>, <code>5xx</code> y <code>total</code>.</li></ul>
En el ejemplo anterior, el subtipo es <code>bytes</code>.</td>
</tr>
<tr>
<td><code>&lt;SERVER-NAME&gt;</code></td>
<td>El nombre del servidor que se ha definido en el recurso Ingress. Para mantener la compatibilidad con Prometheus, los puntos (<code>.</code>) se sustituyen por caracteres de subrayado <code>(\_)</code>. En el ejemplo anterior, el nombre del servidor es <code>dev_demostg1_stg_us_south_containers_appdomain_cloud</code>.</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>El nombre del tipo de métrica recopilado. Para ver una lista de nombres de métrica, consulte la tabla siguiente "Métricas de servidor admitidas". En el ejemplo anterior, el nombre de métrica es <code>out</code>.</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>El valor de la métrica recopilada. En el ejemplo anterior, el subtipo es <code>22319</code>.</td>
</tr>
</tbody></table>

En la tabla siguiente se muestran los nombres de las métricas de servidor admitidas.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono idea"/> Métricas de servidor admitidas</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>El número total de bytes recibidos de los clientes.</td>
</tr>
<tr>
<td><code>out</code></td>
<td>El número total de bytes enviados a los clientes.</td>
</tr>
<tr>
<td><code>bypass</code></td>
<td>El número de veces que se ha captado del servidor de origen un elemento que se puede colocar en memoria caché porque no ha alcanzado el umbral para que se coloque en memoria caché (por ejemplo, número de solicitudes).</td>
</tr>
<tr>
<td><code>expired</code></td>
<td>El número de veces que se ha encontrado un elemento en la memoria caché pero que no se ha seleccionado porque ha caducado.</td>
</tr>
<tr>
<td><code>hit</code></td>
<td>El número de veces que se ha seleccionado un elemento válido de la memoria caché.</td>
</tr>
<tr>
<td><code>miss</code></td>
<td>El número de veces que no se ha encontrado ningún elemento de memoria caché válido en la memoria caché y el servidor ha captado el elemento del servidor de origen.</td>
</tr>
<tr>
<td><code>revalidated</code></td>
<td>El número de veces que se ha revalidado un elemento caducado en la memoria caché.</td>
</tr>
<tr>
<td><code>scarce</code></td>
<td>El número de veces que la memoria caché ha eliminado elementos poco utilizados o de baja prioridad para liberar memoria.</td>
</tr>
<tr>
<td><code>stale</code></td>
<td>El número de veces que se ha encontrado un elemento caducado en la memoria caché, pero, debido a que otra solicitud ha provocado que el servidor capte el elemento desde el servidor de origen, el elemento se ha seleccionado de la memoria caché.</td>
</tr>
<tr>
<td><code>actualizar</code></td>
<td>El número de veces que se ha actualizado contenido obsoleto.</td>
</tr>
<tr>
<td><code>requestMsec</code></td>
<td>El tiempo medio de proceso de solicitudes en milisegundos.</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>El número de respuestas con códigos de estado 1xx.</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>El número de respuestas con códigos de estado 2xx.</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>El número de respuestas con códigos de estado 3xx.</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>El número de respuestas con códigos de estado 4xx.</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>El número de respuestas con códigos de estado 5xx.</td>
</tr>
<tr>
<td><code>total</code></td>
<td>El número total de respuestas con códigos de estado.</td>
  </tr></tbody>
</table>

### Métricas en sentido ascendente
{: #upstream_metrics}

`alb-metrics-exporter` reformatea automáticamente cada campo de datos del archivo JSON en una métrica que pueda leer Prometheus. Las métricas en sentido ascendente recopilan datos del servicio de fondo definido en un recurso de Ingress.
{: shortdesc}

Las métricas en sentido ascendente se formatean de dos maneras.
* El [tipo 1](#type_one) incluye el nombre del servicio en sentido ascendente.
* El [tipo 2](#type_two) incluye el nombre de servicio en sentido ascendente y una dirección IP de pod en sentido ascendente específico.

#### Métricas en sentido ascendente de tipo 1
{: #type_one}

Las métricas de tipo 1 en sentido ascendente están en el formato `kube_system_upstream_<ALB-ID>_<SUB-TYPE>_<UPSTREAM-NAME>_<METRIC-NAME> <VALUE>`.
{: shortdesc}

`<UPSTREAM-NAME>_<METRIC-NAME>` se formatean como etiquetas. Por ejemplo, `albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",metric="in"`

Por ejemplo, si el servicio en sentido ascendente ha recibido un total de 1227 bytes del ALB, la métrica se formatea como:
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="in",pod_template_hash="3805183417"} 1227
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general del formato de métricas de tipo 1 en sentido ascendente</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>El ID de ALB. En el ejemplo anterior, el ID de ALB es <code>public\_crf02710f54fcc40889c301bfd6d5b77fe\_alb1</code>.</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>El subtipo de métrica. Los valores admitidos son <code>bytes</code>, <code>processing\_time</code> y <code>requests</code>. En el ejemplo anterior, el subtipo es <code>bytes</code>.</td>
</tr>
<tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>El nombre del servicio en sentido ascendente que se ha definido en el recurso Ingress. Para mantener la compatibilidad con Prometheus, los puntos (<code>.</code>) se sustituyen por caracteres de subrayado <code>(\_)</code>. En el ejemplo anterior, el nombre en sentido ascendente es <code>default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc</code>.</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>El nombre del tipo de métrica recopilado. Para ver una lista de nombres de métrica, consulte la tabla siguiente "Métricas de tipo 1 en sentido ascendente admitidas". En el ejemplo anterior, el nombre de métrica es <code>in</code>.</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>El valor de la métrica recopilada. En el ejemplo anterior, el subtipo es <code>1227</code>.</td>
</tr>
</tbody></table>

En la tabla siguiente se muestran los nombres de las métricas de tipo 1 en sentido admitidas.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono idea"/> Métricas de tipo 1 en sentido ascendente admitidas</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>El número total de bytes recibidos del servidor ALB.</td>
</tr>
<tr>
<td><code>out</code></td>
<td>El número total de bytes enviados al servidor ALB.</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>El número de respuestas con códigos de estado 1xx.</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>El número de respuestas con códigos de estado 2xx.</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>El número de respuestas con códigos de estado 3xx.</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>El número de respuestas con códigos de estado 4xx.</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>El número de respuestas con códigos de estado 5xx.</td>
</tr>
<tr>
<td><code>total</code></td>
<td>El número total de respuestas con códigos de estado.</td>
  </tr></tbody>
</table>

#### Métricas en sentido ascendente de tipo 2
{: #type_two}

Las métricas de tipo 2 en sentido ascendente están en el formato `kube_system_upstream_<ALB-ID>_<METRIC-NAME>_<UPSTREAM-NAME>_<POD-IP> <VALUE>`.
{: shortdesc}

`<UPSTREAM-NAME>_<POD-IP>` se formatean como etiquetas. Por ejemplo, `albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",backend="172_30_75_6_80"`

Por ejemplo, si el servicio en sentido ascendente tiene un tiempo medio de proceso de solicitudes (incluida la secuencia ascendente) de 40 milisegundos, la métrica se formatea como:
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_requestMsec{albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",app="alb-metrics-exporter",backend="172_30_75_6_80",instance="172.30.75.3:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-swkls",pod_template_hash="3805183417"} 40
```

{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general del formato de métricas de tipo 2 en sentido ascendente</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>El ID de ALB. En el ejemplo anterior, el ID de ALB es <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>.</td>
</tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>El nombre del servicio en sentido ascendente que se ha definido en el recurso Ingress. Para mantener la compatibilidad con Prometheus, los puntos (<code>.</code>) se sustituyen por caracteres de subrayado (<code>\_</code>). En el ejemplo anterior, el nombre del servicio en sentido ascendente es <code>demostg1\_stg\_us\_south\_containers\_appdomain\_cloud\_tea\_svc</code>.</td>
</tr>
<tr>
<td><code>&lt;POD\_IP&gt;</code></td>
<td>La dirección IP y el puerto de un determinado pod de servicio en sentido ascendente. Para mantener la compatibilidad con Prometheus, los puntos (<code>.</code>) y los dos puntos (<code>:</code>) se sustituyen por caracteres de subrayado <code>(_)</code>. En el ejemplo anterior, la IP del pod en sentido ascendente es <code>172_30_75_6_80</code>.</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>El nombre del tipo de métrica recopilado. Para ver una lista de nombres de métrica, consulte la tabla siguiente "Métricas de tipo 2 en sentido ascendente admitidas". En el ejemplo anterior, el nombre de métrica es <code>requestMsec</code>.</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>El valor de la métrica recopilada. En el ejemplo anterior, el subtipo es <code>40</code>.</td>
</tr>
</tbody></table>

En la tabla siguiente se muestran los nombres de las métricas de tipo 2 en sentido admitidas.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono idea"/> Métricas de tipo 2 en sentido ascendente admitidas</th>
</thead>
<tbody>
<tr>
<td><code>requestMsec</code></td>
<td>Los tiempos medios de proceso de solicitudes, incluidas las de sentido ascendente, en milisegundos.</td>
</tr>
<tr>
<td><code>responseMsec</code></td>
<td>El tiempo medio de proceso de respuestas solo en sentido ascendente en milisegundos.</td>
  </tr></tbody>
</table>

<br />


## Aumento del tamaño de la zona de memoria compartida para la recopilación de métricas de Ingress
{: #vts_zone_size}

Las zonas de memoria compartida se definen de forma que los procesos de los nodos trabajadores puedan compartir información como, por ejemplo, la memoria caché, la persistencia de sesiones y los límites de velocidad. Se configura una zona de memoria compartida, denominada zona de estado de tráfico de host virtual, para que Ingress recopile datos de métricas para un ALB.
{:shortdesc}

En el mapa de configuración de Ingress `ibm-cloud-provider-ingress-cm`, el campo `vts-status-zone-size` define el tamaño de la zona de memoria compartida para la recopilación de datos de métricas. De forma predeterminada, `vts-status-zone-size` se establece en `10m`. Si tiene un entorno grande que requiere más memoria para la recopilación de métricas, puede modificar el valor predeterminado para utilizar en su lugar un valor mayor siguiendo estos pasos.

Antes de empezar, asegúrese de que tiene el [rol de servicio **Escritor** o **Gestor** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre el espacio de nombres `kube-system`.

1. Edite el archivo de configuración del recurso del mapa de configuración `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Cambie el valor de `vts-status-zone-size` de `10m` por un valor mayor.

   ```
   apiVersion: v1
   data:
     vts-status-zone-size: "10m"
   kind: ConfigMap
   metadata:
     name: ibm-cloud-provider-ingress-cm
     namespace: kube-system
   ```
   {: codeblock}

3. Guarde el archivo de configuración.

4. Verifique que se hayan aplicado los cambios del mapa de configuración.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}
