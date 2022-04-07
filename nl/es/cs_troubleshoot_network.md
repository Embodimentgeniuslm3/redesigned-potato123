---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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


# Resolución de problemas en las redes del clúster
{: #cs_troubleshoot_network}

Si utiliza {{site.data.keyword.containerlong}}, tenga en cuenta estas técnicas para solucionar problemas relacionados con la red del clúster.
{: shortdesc}

¿Tiene problemas para conectarse a su app a través de Ingress? Intente [depurar Ingress](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress).
{: tip}

Mientras resuelve problemas, puede utilizar la [herramienta de diagnósticos y de depuración de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) para ejecutar pruebas y recopilar información de red, de Ingress y de strongSwan del clúster.
{: tip}

## No se puede conectar a una app mediante un servicio de equilibrador de carga de red (NLB)
{: #cs_loadbalancer_fails}

{: tsSymptoms}
Ha expuesto a nivel público la app creando un servicio NLB en el clúster. Cuando intenta conectar con la app utilizando la dirección IP pública del NLB, la conexión falla o supera el tiempo de espera.

{: tsCauses}
Posibles motivos por los que el servicio de NLB no funciona correctamente:

-   El clúster es un clúster gratuito o un clúster estándar con un solo nodo trabajador.
-   El clúster todavía no se ha desplegado por completo.
-   El script de configuración del servicio de NLB incluye errores.

{: tsResolve}
Para resolver problemas del servicio de NLB:

1.  Compruebe que ha configurado un clúster estándar que se ha desplegado por completo y que tiene al menos dos nodos trabajadores para garantizar la alta disponibilidad del servicio de NLB.

  ```
  ibmcloud ks workers --cluster <cluster_name_or_ID>
  ```
  {: pre}

    En la salida de la CLI, asegúrese de que el **Estado** de los nodos trabajadores sea **Listo** y que el **Tipo de máquina** muestre un tipo de máquina que no sea **gratuito (free)**.

2. Para los NLB versión 2.0: asegúrese de completar los [requisitos previos del NLB 2.0](/docs/containers?topic=containers-loadbalancer#ipvs_provision).

3. Compruebe la precisión del archivo de configuración del servicio de NLB.
    * NLB versión 2.0:
        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myservice
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
        spec:
          type: LoadBalancer
          selector:
            <selector_key>:<selector_value>
          ports:
           - protocol: TCP
           port: 8080
        externalTrafficPolicy: Local
        ```
        {: screen}

        1. Verifique que ha definido **LoadBalancer** como tipo de servicio.
        2. Compruebe que ha incluido la anotación `service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"`.
        3. En la sección `spec.selector` del servicio LoadBalancer, asegúrese de que `<selector_key>` y `<selector_value>` corresponden al par de clave/valor utilizado en la sección `spec.template.metadata.labels` de su archivo YAML de despliegue. Si las etiquetas no coinciden, la sección **Endpoints** en el servicio LoadBalancer visualiza **<none>** y la app no es accesible desde Internet.
        4. Compruebe que ha utilizado el **puerto** en el que escucha la app.
        5. Compruebe que ha establecido `externalTrafficPolicy` en `Local`.

    * NLB versión 1.0:
        ```
        apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      type: LoadBalancer
      selector:
        <selector_key>:<selector_value>
      ports:
           - protocol: TCP
             port: 8080
        ```
        {: screen}

        1. Verifique que ha definido **LoadBalancer** como tipo de servicio.
        2. En la sección `spec.selector` del servicio LoadBalancer, asegúrese de que `<selector_key>` y `<selector_value>` corresponden al par de clave/valor utilizado en la sección `spec.template.metadata.labels` de su archivo YAML de despliegue. Si las etiquetas no coinciden, la sección **Endpoints** en el servicio LoadBalancer visualiza **<none>** y la app no es accesible desde Internet.
        3. Compruebe que ha utilizado el **puerto** en el que escucha la app.

3.  Compruebe el servicio de NLB y revise la sección de **Sucesos** para ver si hay errores.

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    Busque los siguientes mensajes de error:

    <ul><li><pre class="screen"><code>Los clústeres con un nodo deben utilizar servicios de tipo NodePort</code></pre></br>Para utilizar el servicio de NLB, debe tener un clúster estándar con al menos dos nodos trabajadores.</li>
    <li><pre class="screen"><code>No hay ninguna IP de proveedor de nube disponible para dar respuesta a la solicitud de servicio del NLB. Añada una subred portátil al clúster y vuélvalo a intentar</code></pre></br>Este mensaje de error indica que no queda ninguna dirección IP pública portátil que se pueda asignar al servicio de NLB. Consulte la sección sobre <a href="/docs/containers?topic=containers-subnets#subnets">Adición de subredes a clústeres</a> para ver información sobre cómo solicitar direcciones IP públicas portátiles para el clúster. Cuando haya direcciones IP públicas portátiles disponibles para el clúster, el servicio de NLB se creará automáticamente.</li>
    <li><pre class="screen"><code>La IP de proveedor de nube solicitada <cloud-provider-ip> no está disponible. Están disponibles las siguientes IP de proveedor de nube: <available-cloud-provider-ips></code></pre></br>Ha definido una dirección IP pública portátil para el YAML del equilibrador de carga utilizando la sección **`loadBalancerIP`**, pero esta dirección IP pública portátil no está disponible en la subred pública portátil. En la sección **`loadBalancerIP`** del script de configuración, elimine la dirección IP existente y añada una de las direcciones IP públicas portátiles disponibles. También puede eliminar la sección **`loadBalancerIP`** del script para que la dirección IP pública portátil disponible se pueda asignar automáticamente.</li>
    <li><pre class="screen"><code>No hay nodos disponibles para el servicio de NLB</code></pre>No tiene suficientes nodos trabajadores para desplegar un servicio de NLB. Una razón posible es que ha desplegado un clúster estándar con más de un nodo trabajador, pero el suministro de los nodos trabajadores ha fallado.</li>
    <ol><li>Obtenga una lista de los nodos trabajadores disponibles.</br><pre class="pre"><code>kubectl get nodes</code></pre></li>
    <li>Si se encuentran al menos dos nodos trabajadores disponibles, obtenga una lista de los detalles de los nodos trabajadores.</br><pre class="pre"><code>ibmcloud ks worker-get --cluster &lt;cluster_name_or_ID&gt; --worker &lt;worker_ID&gt;</code></pre></li>
    <li>Asegúrese de que los ID de las VLAN públicas y privadas correspondientes a los nodos trabajadores devueltos por los mandatos <code>kubectl get nodes</code> e <code>ibmcloud ks worker-get</code> coinciden.</li></ol></li></ul>

4.  Si utiliza un dominio personalizado para conectar con el servicio de NLB, asegúrese de que el dominio personalizado está correlacionado con la dirección IP pública del servicio de NLB.
    1.  Busque la dirección IP pública del servicio de NLB.
        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  Compruebe que el dominio personalizado esté correlacionado con la dirección IP pública portátil del servicio de NLB en el registro de puntero
(PTR).

<br />


## No se puede conectar a una app mediante Ingress
{: #cs_ingress_fails}

{: tsSymptoms}
Ha expuesto a nivel público la app creando un recurso de Ingress para la app en el clúster. Cuando intenta conectar con la app utilizando la dirección IP pública o subdominio del equilibrador de carga de aplicación (ALB), la conexión falla o supera el tiempo de espera.

{: tsResolve}
En primer lugar, compruebe que el clúster esté totalmente desplegado y que tenga al menos 2 nodos trabajadores disponibles por zona para garantizar la alta disponibilidad para el ALB.
```
ibmcloud ks workers --cluster <cluster_name_or_ID>
```
{: pre}

En la salida de la CLI, asegúrese de que el **Estado** de los nodos trabajadores sea **Listo** y que el **Tipo de máquina** muestre un tipo de máquina que no sea **gratuito (free)**.

* Si el clúster estándar está totalmente desplegado y tiene al menos 2 nodos trabajadores por zona, pero no hay ningún **subdominio de Ingress** disponible, consulte [No se puede obtener un subdominio para el ALB de Ingress](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit).
* Para otros problemas, solucione la configuración de Ingress siguiendo los pasos de [Depuración de Ingress](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress).

<br />


## Problemas secretos del equilibrador de carga de aplicación (ALB) de Ingress
{: #cs_albsecret_fails}

{: tsSymptoms}
Después de desplegar un secreto de equilibrador de carga de aplicación (ALB) de Ingress en el clúster mediante el mandato `ibmcloud ks alb-cert-deploy`, el campo `Descripción` no se actualiza con el nombre de secreto al visualizar el certificado en {{site.data.keyword.cloudcerts_full_notm}}.

Cuando lista información sobre el secreto del ALB, el estado indica `*_failed`. Por ejemplo, `create_failed`, `update_failed`, `delete_failed`.

{: tsResolve}
Revise los motivos siguientes por los que puede fallar el secreto del ALB y los pasos de resolución de problemas correspondientes:

<table>
<caption>Resolución de problemas de secretos del equilibrador de carga de aplicación de Ingress</caption>
 <thead>
 <th>Por qué está ocurriendo</th>
 <th>Cómo solucionarlo</th>
 </thead>
 <tbody>
 <tr>
 <td>No dispone de los roles de acceso necesarios para descargar y actualizar los datos de certificado.</td>
 <td>Solicite al administrador de la cuenta que le asigne los roles de {{site.data.keyword.Bluemix_notm}} IAM siguientes:<ul><li>Los roles de servicio **Gestor** y **Escritor** para la instancia de
{{site.data.keyword.cloudcerts_full_notm}}. Para obtener más información, consulte <a href="/docs/services/certificate-manager?topic=certificate-manager-managing-service-access-roles#managing-service-access-roles">Gestión del acceso del servicio</a> para {{site.data.keyword.cloudcerts_short}}.</li><li>El <a href="/docs/containers?topic=containers-users#platform">rol de plataforma **Administrador**</a> para el clúster.</li></ul></td>
 </tr>
 <tr>
 <td>El CRN de certificado proporcionado en el momento de la creación, actualización o eliminación no pertenece a la misma cuenta que el clúster.</td>
 <td>Compruebe que el CRN de certificado que ha proporcionado se haya importado a una instancia del servicio de {{site.data.keyword.cloudcerts_short}} en la misma cuenta que su clúster.</td>
 </tr>
 <tr>
 <td>El CRN de certificado proporcionado en el momento de la creación no es correcto.</td>
 <td><ol><li>Compruebe que la serie de CRN de certificado es correcta.</li><li>Si el CRN de certificado no es correcto, intente actualizar el secreto: <code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Si el resultado de este mandato es <code>update_failed</code>, elimine el secreto: <code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Vuelva a desplegar el secreto: <code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>El CRN de certificado proporcionado en el momento de la actualización no es correcto.</td>
 <td><ol><li>Compruebe que la serie de CRN de certificado es correcta.</li><li>Si el CRN de certificado no es correcto, elimine el secreto: <code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Vuelva a desplegar el secreto: <code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Intente actualizar el secreto: <code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>El servicio {{site.data.keyword.cloudcerts_long_notm}} está experimentando un tiempo de inactividad.</td>
 <td>Compruebe que el servicio {{site.data.keyword.cloudcerts_short}} esté activo y en ejecución.</td>
 </tr>
 <tr>
 <td>El secreto importado tiene el mismo nombre que el secreto de Ingress proporcionado por IBM.</td>
 <td>Cambie el nombre de su secreto. Puede comprobar el nombre del secreto de Ingress proporcionado por IBM con el mandato `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress`.</td>
 </tr>
 </tbody></table>

<br />


## No se puede obtener un subdominio para ALB de Ingress, ALB no se despliega en una zona o se no puede desplegar un equilibrador de carga
{: #cs_subnet_limit}

{: tsSymptoms}
* Sin subdominio de Ingress: cuando ejecuta `ibmcloud ks cluster-get --cluster <cluster>`, el clúster está en un estado `normal`, sin embargo no hay disponible un **Subdominio de Ingress**.
* Un ALB no se despliega en una zona: si tiene un clúster multizona y ejecuta `ibmcloud ks albs --cluster <cluster>`, no se despliega ningún ALB en una zona. Por ejemplo, si tiene nodos trabajadores en 3 zonas, es posible que vea una información de salida similar a la siguiente, en la que no se ha desplegado un ALB público en la tercera zona.
  ```
  ALB ID                                            Enabled    Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
  private-cr96039a75fddb4ad1a09ced6699c88888-alb1   false      disabled   private   -                dal10   ingress:411/ingress-auth:315   2294021
  private-cr96039a75fddb4ad1a09ced6699c88888-alb2   false      disabled   private   -                dal12   ingress:411/ingress-auth:315   2234947
  private-cr96039a75fddb4ad1a09ced6699c88888-alb3   false      disabled   private   -                dal13   ingress:411/ingress-auth:315   2234943
  public-cr96039a75fddb4ad1a09ced6699c88888-alb1    true       enabled    public    169.xx.xxx.xxx   dal10   ingress:411/ingress-auth:315   2294019
  public-cr96039a75fddb4ad1a09ced6699c88888-alb2    true       enabled    public    169.xx.xxx.xxx   dal12   ingress:411/ingress-auth:315   2234945
  ```
  {: screen}
* No se puede desplegar un equilibrador de carga: cuando describe el mapa de configuración `ibm-cloud-provider-vlan-ip-config`, es posible que vea un mensaje de error parecido al de la siguiente salida de ejemplo.
  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config
  ```
  {: pre}
  ```
  Warning  CreatingLoadBalancerFailed ... ErrorSubnetLimitReached: There are already the maximum number of subnets permitted in this VLAN.
  ```
  {: screen}

{: tsCauses}
En los clústeres estándares, la primera vez que crea un clúster en una zona, se suministra automáticamente una VLAN pública y una VLAN privada en dicha zona en su cuenta de infraestructura de IBM Cloud (SoftLayer). En dicha zona, se solicita una subred pública portátil en la VLAN pública que especifique y una subred privada portátil en la VLAN privada que especifique. En {{site.data.keyword.containerlong_notm}}, las VLAN tienen un límite de 40 subredes. Si la VLAN del clúster de una zona ya ha alcanzado ese límite, el **subdominio de Ingress** no se suministra, el ALB de Ingress público para dicha zona no se suministra o bien no tiene una dirección IP pública portátil disponible para crear un equilibrador de carga de red (NLB).

Para ver cuántas subredes tiene una VLAN:
1.  En la [consola (SoftLayer) de la infraestructura de IBM Cloud](https://cloud.ibm.com/classic?), seleccione **Red** > **Gestión de IP** > **VLAN**.
2.  Pulse el **Número de VLAN** de la VLAN que utilizó para crear el clúster. Revise la sección **Subnets** para ver si hay 40 o más subredes.

{: tsResolve}
Si necesita una nueva VLAN, [póngase en contacto con el equipo de soporte de {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans) para solicitar una. A continuación, [cree un clúster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) que utilice esta nueva VLAN.

Si tiene otra VLAN que esté disponible, puede [configurar la expansión de la VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) en el clúster existente. Después, puede añadir nuevos nodos trabajadores al clúster que utilicen otra VLAN con subredes disponibles. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.

Si no utiliza todas las subredes de la VLAN, puede reutilizar subredes de la VLAN añadiéndolas al clúster.
1. Compruebe que la subred que desea utilizar está disponible.
  <p class="note">La cuenta de infraestructura que utiliza podría compartirse entre varias cuentas de {{site.data.keyword.Bluemix_notm}}. En este caso, aunque ejecute el mandato `ibmcloud ks subnets` para ver subredes con **Clústeres enlazados**, solo puede ver información de sus clústeres. Compruebe con el propietario de la cuenta de infraestructura para asegurarse de que las subredes están disponibles y que no las estén utilizando otra cuenta o equipo.</p>

2. Utilice el [mandato `ibmcloud ks cluster-subnet-add`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add) para poner una subred existente a disposición del clúster.

3. Compruebe que la subred se haya creado y añadido correctamente al clúster. El CIDR de subred se muestra en la sección **Subnet VLANs**.
    ```
    ibmcloud ks cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

    En esta salida de ejemplo, se ha añadido una segunda subred a la VLAN pública `2234945`:
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR          Public   User-managed
    2234947   10.xxx.xx.xxx/29     false    false
    2234945   169.xx.xxx.xxx/29    true     false
    2234945   169.xx.xxx.xxx/29    true     false
    ```
    {: screen}

4. Verifique que las direcciones IP portátiles de la subred que ha añadido se utilizan para los ALB o para los equilibradores de carga del clúster. Los servicios pueden tardar varios minutos en utilizar las direcciones IP portátiles de la subred recién añadida.
  * Sin subdominio de Ingress: ejecute `ibmcloud ks cluster-get --cluster <cluster>` para verificar que el **subdominio de Ingress** se llena.
  * Un ALB no se despliega en una zona: ejecute `ibmcloud ks albs --cluster <cluster>` para verificar que el ALB que falta se ha desplegado.
  * No se puede desplegar un equilibrador de carga: ejecute `kubectl get svc -n kube-system` para verificar que el equilibrador de carga tiene una **EXTERNAL-IP**.

<br />


## La conexión a través de WebSocket se cierra después de 60 segundos
{: #cs_ingress_websocket}

{: tsSymptoms}
El servicio Ingress expone una app que utiliza un WebSocket. Sin embargo, la conexión entre un cliente y la app de WebSocket se cierra cuando no se envía tráfico entre ellos durante 60 segundos.

{: tsCauses}
La conexión a la app de WebSocket se puede eliminar después de 60 segundos de inactividad por una de las siguientes razones:

* La conexión de Internet tiene un proxy o un cortafuegos que no tolera las conexiones largas.
* Un tiempo de espera excedido en el ALB a la app de WebSocket termina la conexión.

{: tsResolve}
Para evitar que la conexión se cierre después de 60 segundos de inactividad:

1. Si se conecta a la app de WebSocket a través de un proxy o un cortafuegos, asegúrese de que el proxy o el cortafuegos no estén configurados para terminar automáticamente las conexiones largas.

2. Para mantener la conexión activa, puede aumentar el valor del tiempo de espera o configurar un latido en la app.
<dl><dt>Cambiar el tiempo de espera</dt>
<dd>Aumente el valor de `proxy-read-timeout` en la configuración de ALB. Por ejemplo, para cambiar el tiempo de espera de `60s` a un valor mayor, como `300s`, añada esta [anotación](/docs/containers?topic=containers-ingress_annotation#connection) al archivo de recursos de Ingress: `ingress.bluemix.net/proxy-read-timeout: "serviceName=<service_name> timeout=300s"`. El tiempo de espera se cambia para todos los ALB públicos del clúster.</dd>
<dt>Configurar un latido</dt>
<dd>Si no desea cambiar el valor de tiempo de espera de lectura predeterminado de ALB, configure un latido en la app de WebSocket. Cuando se configura un protocolo de latido utilizando una infraestructura como [WAMP ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://wamp-proto.org/), el servidor en sentido ascendente de la app envía periódicamente un mensaje "ping" en un intervalo de tiempo y el cliente responde con un mensaje "pong". Establezca el intervalo de latido en 58 segundos o menos para que el tráfico "ping/pong" mantenga la conexión abierta antes de que se aplique el tiempo de espera de 60 segundos.</dd></dl>

<br />


## La conservación de la IP de origen falla cuando se utilizan nodos antagónicos
{: #cs_source_ip_fails}

{: tsSymptoms}
Ha habilitado la conservación de IP de origen para un servicio [equilibrador de carga versión 1.0](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) o [Ingress ALB](/docs/containers?topic=containers-ingress#preserve_source_ip) cambiando `externalTrafficPolicy` por `Local` en el archivo de configuración del servicio. Sin embargo, no hay tráfico que llegue al servicio de fondo para su app.

{: tsCauses}
Cuando habilita la conservación de IP para los servicios equilibrador de carga o Ingress ALB, la dirección IP de origen de la solicitud de cliente se conserva. El servicio reenvía el tráfico a los pods de app del mismo nodo trabajador solo para asegurarse de que la dirección IP del paquete de solicitud no se cambia. Generalmente, los pods del servicio equilibrador de carga o de Ingress ALB se despliegan en los nodos trabajadores donde se despliegan los pods de app. Sin embargo, existen algunas situaciones donde los pods de servicio y los pods de app podrían no planificarse en los mismos nodos trabajadores. Si ve [nodos antagónicos de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) en nodos trabajadores, se impide que los pods que no tienen tolerancia a antagonismo se ejecuten en los nodos trabajadores antagónicos. Es posible que la conservación de IP de origen no esté funcionando dependiendo del tipo de antagonismo que utilice:

* **Antagonismos de nodo extremo**: ha [añadido la etiqueta `dedicated=edge`](/docs/containers?topic=containers-edge#edge_nodes) a dos o más nodos trabajadores en cada VLAN pública del clúster para asegurarse de que los pods Ingress y de equilibrador de carga solo se desplieguen en estos nodos trabajadores. A continuación, también [ha definido como antagónicos estos nodos de extremo](/docs/containers?topic=containers-edge#edge_workloads) para evitar que se ejecuten otras cargas de trabajo en los nodos extremo. Sin embargo, no ha añadido una regla de afinidad de nodo extremo y tolerancia al despliegue de la app. Las pods de la app no se pueden planificar en los mismos nodos antagónicos que los pods de servicio, y el tráfico que llega al servicio de fondo para la app.

* **Nodos antagónicos personalizados**: ha utilizado nodos antagónicos personalizados de diversos nodos de modo que solo los pods de la app con tolerancia a antagonismo se pueden desplegar en dichos nodos. Ha añadido reglas de afinidad y tolerancias a los despliegues de la app y al servicio equilibrador de carga o Ingress para que sus pods solo se desplieguen en esos nodos. Sin embargo, los pods `ibm-cloud-provider-ip` `keepalived` que se crean automáticamente en el espacio de nombres `ibm-system` garantizan que los pods de equilibrador de carga y los pods de app siempre se planifican en el mismo nodo trabajador. Estos pods `keepalived` no tienen tolerancia a los nodos antagónicos que ha utilizado. No se pueden planificar en los mismos nodos antagónicos en los que se ejecutan los pods de la app, y el tráfico que llega al servicio de fondo para la app.

{: tsResolve}
Para solucionar el problema, elija una de las opciones siguientes:

* **Nodos de extremo antagónicos**: para asegurarse de que el equilibrador de carga y los pods de la app se despliegan en nodos de extremo antagónicos, [añada reglas de afinidad de nodo de extremo y tolerancias al despliegue de la app](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes). Las pods de equilibrador de carga y de Ingress ALB tienen estas reglas de afinidad y tolerancias de forma predeterminada.

* **Antagonismos personalizados**: elimine los antagonismos personalizados para los que los pods `keepalived` no tienen tolerancia. En su lugar, puede añadir [nodos trabajadores de etiqueta como nodos de extremo, y luego definir antagonismo para dichos nodos de extremo](/docs/containers?topic=containers-edge).

Si selecciona una de las opciones anteriores, pero los pods `keepalived` siguen sin planificarse, puede obtener más información sobre los pods `keepalived`:

1. Obtenga los pods `keepalived`.
    ```
    kubectl get pods -n ibm-system
    ```
    {: pre}

2. En la salida, busque los pods `ibm-cloud-provider-ip` que tienen para **Status** el valor `Pending`. Ejemplo:
    ```
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t     0/1       Pending   0          2m        <none>          <none>
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-8ptvg     0/1       Pending   0          2m        <none>          <none>
    ```
    {:screen}

3. Describa cada pod `keepalived` y busque la sección **Events**. Solucione cualquier mensaje de error o de aviso que aparezca.
    ```
    kubectl describe pod ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t -n ibm-system
    ```
    {: pre}

<br />


## No se puede establecer la conectividad de VPN con el diagrama de Helm de strongSwan
{: #cs_vpn_fails}

{: tsSymptoms}
Cuando comprueba la conectividad de VPN ejecutando `kubectl exec  $STRONGSWAN_POD -- ipsec status`, no ve el estado `ESTABLISHED`, o el pod de VPN está en estado `ERROR` o sigue bloqueándose y reiniciándose.

{: tsCauses}
El archivo de configuración del diagrama de Helm tiene valores incorrectos, errores de sintaxis o le faltan valores.

{: tsResolve}
Cuando intenta establecer la conectividad de VPN con el diagrama de Helm de strongSwan, es probable que el estado de VPN no sea `ESTABLISHED` la primera vez. Puede que necesite comprobar varios tipos de problema y cambiar el archivo de configuración en consonancia. Para resolver el problema de conectividad de VPN de strongSwan:

1. [Pruebe y verifique la conectividad de VPN de strongSwan](/docs/containers?topic=containers-vpn#vpn_test) ejecutando las cinco pruebas de Helm que se incluyen en la definición del diagrama de strongSwan.

2. Si no puede establecer la conectividad de VPN después de ejecutar las pruebas de Helm, puede ejecutar la herramienta de depuración de VPN incluida en el paquete de la imagen del pod de VPN.

    1. Establezca la variable de entorno `STRONGSWAN_POD`.

        ```
        export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. Ejecute la herramienta de depuración.

        ```
        kubectl exec  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        La herramienta genera varias páginas de información, ya que ejecuta varias pruebas para problemas comunes de red. Las líneas de la salida que empiezan por `ERROR`, `WARNING`, `VERIFY` o `CHECK` indican posibles errores con la conectividad de VPN.

    <br />


## No se puede instalar un nuevo release del diagrama de Helm strongSwan
{: #cs_strongswan_release}

{: tsSymptoms}
Supongamos que modifica su diagrama de Helm strongSwan e intenta instalar el nuevo release ejecutando `helm install -f config.yaml --name=vpn ibm/strongswan`. Sin embargo, recibe el siguiente error:
```
Error: release vpn failed: deployments.extensions "vpn-strongswan" already exists
```
{: screen}

{: tsCauses}
Este error indica que el release anterior del diagrama strongSwan no se ha desinstalado por completo.

{: tsResolve}

1. Suprima el release del diagrama anterior.
    ```
    helm delete --purge vpn
    ```
    {: pre}

2. Suprima el despliegue correspondiente al release anterior. La supresión del despliegue y del pod asociado tarda 1 minuto como máximo.
    ```
    kubectl delete deploy vpn-strongswan
    ```
    {: pre}

3. Verifique que el despliegue se ha suprimido. El despliegue `vpn-strongswan` no aparece en la lista.
    ```
    kubectl get deployments
    ```
    {: pre}

4. Vuelva a instalar el diagrama de Helm strongSwan actualizado con un nuevo nombre de release.
    ```
    helm install -f config.yaml --name=vpn ibm/strongswan
    ```
    {: pre}

<br />


## La conectividad de VPN de strongSwan falla después de añadir o suprimir nodos trabajadores
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
Ha establecido previamente una conexión VPN activa utilizando el servicio VPN IPSec de strongSwan. Sin embargo, después de haber añadido o suprimido un nodo trabajador en el clúster, aparecen uno o varios de los siguientes síntomas:

* El estado de la VPN no es `ESTABLISHED`
* No se puede acceder a los nuevos nodos trabajadores desde la red local
* No se puede acceder a la red remota desde los pods que se ejecutan en los nuevos nodos trabajadores

{: tsCauses}
Si ha añadido un nodo trabajador a una agrupación de nodos trabajadores:

* El nodo trabajador se ha suministrado en una nueva subred privada que no se expone a través de la conexión VPN existente mediante los valores `localSubnetNAT` o `local.subnet`
* Las rutas de VPN no pueden añadirse al nodo trabajador porque el trabajador tiene antagonismos o etiquetas que no están incluidas en los valores actuales de `tolerations` o `nodeSelector`
* El pod de VPN se ejecuta en el nodo trabajador nuevo, pero la dirección IP pública de dicho nodo trabajador no está permitida por el cortafuegos local

Si ha suprimido un nodo trabajador:

* Ese nodo trabajador era el único nodo en el que se estaba ejecutando un pod de VPN, debido a las restricciones en algunos antagonismos o etiquetas en los valores existentes de `tolerations` o `nodeSelector`

{: tsResolve}
Actualice los valores de diagrama de Helm para reflejar los cambios del nodo trabajador:

1. Suprima el diagrama de Helm existente.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Abra el archivo de configuración para el servicio VPN de strongSwan.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Compruebe los valores siguientes y cambie los valores para reflejar los nodos trabajadores añadidos o suprimidos necesarios.

    Si ha añadido un nodo trabajador:

    <table>
    <caption>Valores de nodo trabajador</caption>
     <thead>
     <th>Valor</th>
     <th>Descripción</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>El trabajador añadido puede estar desplegado en una subred privada nueva diferente a las demás subredes existentes en las que se encuentran otros nodos trabajadores. Si utiliza NAT de subred para volver a correlacionar las direcciones IP locales privadas del clúster y el trabajador se añade a una nueva subred, añada el CIDR de la nueva subred a este valor.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Si anteriormente ha limitado el despliegue pods de VPN a los trabajadores con una etiqueta específica, asegúrese de que el nodo trabajador añadido también tiene dicha etiqueta.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Si el nodo trabajador añadido tiene antagonismos, cambie este valor para permitir que el pod de VPN se ejecute en todos los trabajadores con cualquier antagonismo o con antagonismos específicos.</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>El trabajador añadido puede estar desplegado en una subred privada nueva diferente a las subredes existentes en las que se encuentran otros trabajadores. Si las apps están expuestas por los servicios NodePort o LoadBalancer en la red privada y las apps están en el trabajador añadido, añada el CIDR de la nueva subred a este valor. **Nota**: Si añade valores a `local.subnet`, compruebe los valores de VPN para la subred local para ver si también deben actualizarse.</td>
     </tr>
     </tbody></table>

    Si ha suprimido un nodo trabajador:

    <table>
    <caption>Valores de nodo trabajador</caption>
     <thead>
     <th>Valor</th>
     <th>Descripción</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>Si utiliza NAT de subred para volver a correlacionar direcciones IP locales privadas específicas, elimine las direcciones IP de esta configuración que pertenezcan al trabajador antiguo. Si utiliza NAT de subred para volver a correlacionar subredes enteras y no quedan trabajadores en una subred, elimine dicho CIDR de subred de este valor.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Si con anterioridad limitó el despliegue de pod de VPN a un trabajador individual y dicho trabajador fue suprimido, cambie este valor para permitir que el pod de VPN se ejecute en otros trabajadores.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Si el trabajador que suprimió no poseía antagonismos, y los únicos trabajadores que quedan poseen antagonismos, cambie este valor para permitir que todos los pods de VPN se ejecuten en trabajadores con cualquier antagonismo o con antagonismos específicos.
     </td>
     </tr>
     </tbody></table>

4. Instale el nuevo diagrama de Helm con los valores actualizados.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. Compruebe el estado de despliegue del diagrama. Cuando el diagrama está listo, el campo **STATUS**, situado cerca de la parte superior de la salida, tiene el valor `DEPLOYED`.

    ```
    helm status <release_name>
    ```
    {: pre}

6. En algunos casos, es posible que tenga que cambiar los valores locales y los valores del cortafuegos para que coincidan con los cambios realizados en el archivo de configuración de VPN.

7. Inicie la VPN.
    * Si el clúster inicia la conexión VPN (`ipsec.auto` se establece en `start`), inicie la VPN en la pasarela local y luego inicie la VPN en el clúster.
    * Si la pasarela local inicia la conexión VPN (`ipsec.auto` se establece en `auto`), inicie la VPN en el clúster y luego inicie la VPN en la pasarela local.

8. Establezca la variable de entorno `STRONGSWAN_POD`.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. Compruebe el estado de la VPN.

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * Si la conexión VPN tiene el estado `ESTABLISHED`, la conexión VPN se ha realizado correctamente. No es necesario realizar ninguna otra acción.

    * Si sigue teniendo problemas de conexión, consulte [No se puede establecer la conectividad de VPN con el diagrama de Helm de strongSwan](#cs_vpn_fails) para solucionar más problemas de conexión VPN.

<br />



## No se pueden recuperar políticas de red de Calico
{: #cs_calico_fails}

{: tsSymptoms}
Cuando intenta ver las políticas de red de Calico en su clúster ejecutando `calicoctl get policy`, obtiene resultados no esperados o mensajes de error:
- Una lista vacía
- Una lista de políticas de Calico v2 antiguas en lugar de políticas de la v3
- `No se ha podido crear el cliente de API de Calico: error de sintaxis en calicoctl.cfg: archivo de configuración no válido: APIVersion desconocida 'projectcalico.org/v3'`

Cuando intenta ver las políticas de red de Calico en su clúster ejecutando `calicoctl get GlobalNetworkPolicy`, obtiene resultados no esperados o mensajes de error:
- Una lista vacía
- `No se ha podido crear el cliente de API de Calico: error de sintaxis en calicoctl.cfg: archivo de configuración no válido: APIVersion desconocida 'v1'`
- `No se ha podido crear el cliente de API de Calico: error de sintaxis en calicoctl.cfg: archivo de configuración no válido: APIVersion desconocida 'projectcalico.org/v3'`
- `No se ha podido obtener recursos: No se da soporte al tipo de recurso 'GlobalNetworkPolicy'`

{: tsCauses}
Para utilizar políticas de Calico, se deben alinear cuatro factores: la versión de Kubernetes del clúster, la versión de la CLI de Calico, la sintaxis del archivo de configuración de Calico y los mandatos de visualización de política. Uno o más de estos factores no están en la versión correcta.

{: tsResolve}
Debe utilizar la v3.3 o posterior de la CLI de Calico, la sintaxis del archivo de configuración de la v3 `calicoctl.cfg` y los mandatos `calicoctl get GlobalNetworkPolicy` y `calicoctl get NetworkPolicy`.

Para asegurarse de que se han alineado todos los factores de Calico:

1. [Instale y configure una CLI de Calico versión 3.3 o posterior](/docs/containers?topic=containers-network_policies#cli_install).
2. Asegúrese de que las políticas que desea crear y aplicar al clúster utilizan la [sintaxis Calico v3 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/networkpolicy). Si tiene un archivo de política `.yaml` o `.json` existente en sintaxis de Calico v2, puede convertirlo a la sintaxis de Calico v3 utilizando el mandato [`calicoctl convert` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.3/reference/calicoctl/commands/convert).
3. Para [visualizar políticas](/docs/containers?topic=containers-network_policies#view_policies), asegúrese de utilizar `calicoctl get GlobalNetworkPolicy` para políticas globales y `calicoctl get NetworkPolicy --namespace <policy_namespace>` para políticas cuyo ámbito sean espacios de nombres específicos.

<br />


## No se pueden añadir nodos trabajadores debido a un ID de VLAN no válido
{: #suspended}

{: tsSymptoms}
Su cuenta de {{site.data.keyword.Bluemix_notm}} ha sido suspendida, o bien se han suprimido todos los nodos trabajadores del clúster. Después de que se reactive la cuenta, no puede añadir nodos trabajadores cuando intenta cambiar el tamaño de la agrupación de nodo trabajadores o cuando intenta volverla a equilibrar. Aparece un mensaje de error similar al siguiente:

```
SoftLayerAPIError(SoftLayer_Exception_Public): Could not obtain network VLAN with id #123456.
```
{: screen}

{: tsCauses}
Cuando se suspende una cuenta, se suprimen los nodos trabajadores de la cuenta. Si un clúster no tiene nodos trabajadores, la infraestructura de IBM Cloud (SoftLayer) reclama las VLAN públicas y privadas asociadas. Sin embargo, la agrupación de nodos trabajadores del clúster todavía tiene los ID de VLAN anteriores en sus metadatos y utiliza estos ID no disponibles cuando se reequilibra o se redimensiona la agrupación. Los nodos no se pueden crear porque las VLAN ya no están asociadas al clúster.

{: tsResolve}

Puede [suprimir la agrupación de nodos trabajadores existente](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_rm) y luego [crear una nueva agrupación de nodos trabajadores](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create).

Como alternativa, puede conservar la agrupación de nodos trabajadores existente solicitando nuevas VLAN y utilizándolas para crear nuevos nodos trabajadores en la agrupación.

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Para obtener las zonas para las que necesita nuevos ID de VLAN, anote la **Ubicación** de la información de salida del siguiente mandato. **Nota**: si el clúster es multizona, necesita los ID de VLAN de cada zona.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

2.  Obtenga una nueva VLAN privada y pública para cada zona en la que esté el clúster; para ello, [póngase en contacto con el servicio de soporte de {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans).

3.  Anote los nuevos ID de VLAN privada y pública para cada zona.

4.  Anote el nombre de las agrupaciones de nodos trabajadores.

    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

5.  Utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set) `zone-network-set` para cambiar los metadatos de red de la agrupación de nodos trabajadores.

    ```
    ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> -- worker-pools <worker-pool> --private-vlan <private_vlan_ID> --public-vlan <public_vlan_ID>
    ```
    {: pre}

6.  **Solo clúster multizona**: repita el **Paso 5** para cada zona del clúster.

7.  Reequilibre o redimensione la agrupación de nodos trabajadores para añadir nodos trabajadores que utilizan los nuevos ID de VLAN. Por ejemplo:

    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <worker_pool> --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

8.  Verifique que se han creado los nodos trabajadores.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <worker_pool>
    ```
    {: pre}

<br />



## Obtención de ayuda y soporte
{: #network_getting_help}

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

