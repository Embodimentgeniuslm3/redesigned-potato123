---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Resolución de problemas de supervisión y creación de registros
{: #cs_troubleshoot_health}

Si utiliza {{site.data.keyword.containerlong}}, tenga en cuenta estas técnicas para resolver problemas relacionados con la supervisión y la creación de registros.
{: shortdesc}

Si tiene un problema más general, pruebe la [depuración del clúster](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}

## Los registros no aparecen
{: #cs_no_logs}

{: tsSymptoms}
Al acceder al panel de control de Kibana, sus registros no se visualizan.

{: tsResolve}
Revise los siguientes motivos por los que no aparecen sus registros del clúster y los pasos de resolución de problemas correspondientes:

<table>
<caption>Resolución de problemas de registros que no se muestran</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Por qué está ocurriendo</th>
      <th>Cómo solucionarlo</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>No hay ninguna configuración de registro definida.</td>
    <td>Para que se envíen los registros, debe crear una configuración de registro. Para ello, consulte <a href="/docs/containers?topic=containers-health#logging">Configuración del registro de clúster</a>.</td>
  </tr>
  <tr>
    <td>El clúster no se encuentra en estado <code>Normal</code>.</td>
    <td>Para comprobar el estado del clúster, consulte <a href="/docs/containers?topic=containers-cs_troubleshoot#debug_clusters">Depuración de clústeres</a>.</td>
  </tr>
  <tr>
    <td>Se ha alcanzado el límite de almacenamiento de registro.</td>
    <td>Para aumentar los límites del almacenamiento de registros, consulte la <a href="/docs/services/CloudLogAnalysis/troubleshooting?topic=cloudloganalysis-error_msgs">documentación de {{site.data.keyword.loganalysislong_notm}}</a>.</td>
  </tr>
  <tr>
    <td>Si ha especificado un espacio durante la creación del clúster, el propietario de la cuenta no tiene permisos de gestor, desarrollador o auditor para el espacio en cuestión.</td>
      <td>Para cambiar los permisos de acceso para el propietario de la cuenta:
      <ol><li>Para descubrir quién es el propietario de la cuenta del clúster, ejecute <code>ibmcloud ks api-key-info --cluster &lt;cluster_name_or_ID&gt;</code>.</li>
      <li>Para otorgar a dicho propietario de cuenta permisos de acceso de {{site.data.keyword.containerlong_notm}} de Gestor, Desarrollador o Auditor al espacio, consulte <a href="/docs/containers?topic=containers-users">Gestión de acceso a clústeres</a>.</li>
      <li>Para renovar la señal de registro tras cambiar los permisos, ejecute <code>ibmcloud ks logging-config-refresh --cluster &lt;cluster_name_or_ID&gt;</code>.</li></ol></td>
    </tr>
    <tr>
      <td>Tiene una configuración de registro para la app con un enlace simbólico en la vía de acceso de la app.</td>
      <td><p>Para que se puedan enviar los registros, se debe utilizar una vía de acceso absoluta en la configuración de registro o de lo contrario, no se podrán leer los registros. Si la vía de acceso está montada en su nodo trabajador, podría crear un enlace simbólico.</p> <p>Ejemplo: Si la vía de acceso especificada es <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> pero los registros van a <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>, entonces no se podrán leer los registros.</p></td>
    </tr>
  </tbody>
</table>

Para probar los cambios que ha realizado durante la resolución de problemas, puede desplegar *Noisy*, un pod de ejemplo que produce varios sucesos de registro, en un nodo trabajador en el clúster.

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Cree el archivo de configuración `deploy-noisy.yaml`.
    ```
    apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
      - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
      ```
      {: codeblock}

2. Ejecute el archivo de configuración en el contexto del clúster.
    ```
    kubectl apply -f noisy.yaml
    ```
    {:pre}

3. Después de unos minutos, verá los registros en el panel de control. Para acceder al panel de control de Kibana, vaya a uno de los siguientes URL y seleccione la cuenta de {{site.data.keyword.Bluemix_notm}} en la que ha creado el clúster. Si ha especificado un espacio durante la creación del clúster, vaya al espacio.
    - EE. UU. sur y EE. UU. este: `https://logging.ng.bluemix.net`
    - RU sur: `https://logging.eu-gb.bluemix.net`
    - UE central: `https://logging.eu-fra.bluemix.net`
    - AP sur: `https://logging.au-syd.bluemix.net`

<br />


## El panel de control de Kubernetes no muestra los gráficos de utilización
{: #cs_dashboard_graphs}

{: tsSymptoms}
Al acceder al panel de control de Kubernetes, los gráficos de utilización no se visualizan.

{: tsCauses}
En ocasiones después de actualizar un clúster o reiniciar un nodo trabajador, el pod `kube-dashboard` no se actualiza.

{: tsResolve}
Suprima el pod `kube-dashboard` para forzar un reinicio. El pod se volverá a crear con políticas RBAC para acceder a `heapster` para obtener información de utilización.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## La cuota de registro es demasiado baja
{: #quota}

{: tsSymptoms}
Se realiza una configuración de registro en el clúster para reenviar registros a {{site.data.keyword.loganalysisfull}}. Cuando visualice registros, verá un mensaje de error similar al siguiente:

```
Ha alcanzado la cuota diaria asignada al espacio de Bluemix {GUID de espacio} para la instancia de IBM® Cloud Log Analysis {GUID de instancia}. Su asignación diaria actual es de XXX para el almacenamiento de búsqueda de registros, que se retienen durante un periodo de 3 días, durante los cuales se pueden buscar en Kibana. Esto no afecta a la política de retención de registros en el almacenamiento de recopilación de registros. Para actualizar el plan de modo que pueda almacenar más datos en el almacenamiento de búsqueda de registros diario, actualice el plan del servicio Log Analysis para este espacio. Para obtener más información sobre los planes de servicio y sobre cómo actualizar su plan, consulte Planes.
```
{: screen}

{: tsResolve}
Revise los siguientes motivos por los que ha alcanzado la cuota de registro y para ver los pasos a seguir para resolver el problema:

<table>
<caption>Resolución de problemas de almacenamiento de registros</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Por qué está ocurriendo</th>
      <th>Cómo solucionarlo</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>Uno o más pods generan un gran número de registros.</td>
    <td>Puede liberar espacio de almacenamiento de registro evitando que se reenvíen registros de pods específicos. Cree un [filtro de registro](/docs/containers?topic=containers-health#filter-logs) para estos pods.</td>
  </tr>
  <tr>
    <td>Ha superado la asignación diaria de 500 MB para el almacenamiento de registro para el plan Lite.</td>
    <td>En primer lugar, [calcule la cuota de búsqueda y el uso diario](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota) del dominio de registro. A continuación, puede aumentar la cuota de almacenamiento de registro [actualizando el plan del servicio {{site.data.keyword.loganalysisshort_notm}}](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan).</td>
  </tr>
  <tr>
    <td>Ha superado la cuota de almacenamiento de registro para el plan de pago actual.</td>
    <td>En primer lugar, [calcule la cuota de búsqueda y el uso diario](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota) del dominio de registro. A continuación, puede aumentar la cuota de almacenamiento de registro [actualizando el plan del servicio {{site.data.keyword.loganalysisshort_notm}}](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan).</td>
  </tr>
  </tbody>
</table>

<br />


## Las líneas de registro son demasiado largas
{: #long_lines}

{: tsSymptoms}
Se realiza una configuración de registro en el clúster para reenviar registros a {{site.data.keyword.loganalysisfull_notm}}. Cuando visualiza registros, ve un mensaje de registro largo. Además, en Kibana, es posible que solo pueda ver los últimos 600-700 caracteres del mensaje de registro.

{: tsCauses}
Es posible que un mensaje de registro largo se trunque debido a su longitud antes de que lo recopile Fluentd, por lo que es posible que Fluentd no analice correctamente el registro antes de reenviarlo a {{site.data.keyword.loganalysisshort_notm}}.

{: tsResolve}
Para limitar la longitud de línea, puede configurar su propio registrador de modo que tenga una longitud máxima para `stack_trace` en cada registro. Por ejemplo, si utiliza Log4j para el registrador, puede utilizar el valor [`EnhancedPatternLayout` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/EnhancedPatternLayout.html) para limitar `stack_trace` a 15 KB.

## Obtención de ayuda y soporte
{: #health_getting_help}

¿Sigue teniendo problemas con su clúster?
{: shortdesc}

-  En el terminal, se le notifica cuando están disponibles las actualizaciones de la CLI y los plugins de `ibmcloud`. Asegúrese de mantener actualizada la CLI para poder utilizar todos los mandatos y distintivos disponibles.
-   Para ver si {{site.data.keyword.Bluemix_notm}} está disponible, [consulte la página de estado de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/status?selected=status).
-   Publique una pregunta en [Slack de {{site.data.keyword.containerlong_notm}}![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-container-service.slack.com).
    Si no utiliza un ID de IBM para la cuenta de {{site.data.keyword.Bluemix_notm}}, [solicite una invitación](https://bxcs-slack-invite.mybluemix.net/) a este Slack.
    {: tip}
-   Revise los foros para ver si otros usuarios se han encontrado con el mismo problema. Cuando utiliza los foros para formular una pregunta, etiquete la pregunta para que la puedan ver los equipos de desarrollo de {{site.data.keyword.Bluemix_notm}}.
    -   Si tiene preguntas técnicas sobre el desarrollo o despliegue de clústeres o apps con {{site.data.keyword.containerlong_notm}}, publique su pregunta en [Stack Overflow ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) y etiquete su pregunta con `ibm-cloud`, `kubernetes` y `containers`.
    -   Para formular preguntas sobre el servicio y obtener instrucciones de iniciación, utilice el foro [IBM Developer Answers ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Incluya las etiquetas `ibm-cloud` y `containers`.
    Consulte [Obtención de ayuda](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) para obtener más detalles sobre cómo utilizar los foros.
-   Póngase en contacto con el soporte de IBM abriendo un caso. Para obtener información sobre cómo abrir un caso de soporte de IBM, o sobre los niveles de soporte y las gravedades de los casos, consulte [Cómo contactar con el servicio de soporte](/docs/get-support?topic=get-support-getting-customer-support).
Al informar de un problema, incluya el ID de clúster. Para obtener el ID de clúster, ejecute `ibmcloud ks clusters`. También puede utilizar la [herramienta de diagnósticos y de depuración de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) para recopilar y exportar la información pertinente del clúster que se va a compartir con el servicio de soporte de IBM.
{: tip}

