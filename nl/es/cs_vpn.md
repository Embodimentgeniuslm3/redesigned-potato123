---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

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


# Configuración de la conectividad de VPN
{: #vpn}

La conectividad de VPN le permite conectar de forma segura apps de un clúster de Kubernetes en {{site.data.keyword.containerlong}} a una red local. También puede conectar apps que son externas al clúster a una app que se ejecuta dentro del clúster.
{:shortdesc}

Para conectar de forma segura sus nodos trabajadores y apps a un centro de datos local, puede utilizar una de las siguientes opciones.

- **Servicio VPN IPSec de strongSwan**: puede configurar un [servicio VPN IPSec de strongSwan ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.strongswan.org/about.html) que se conecte de forma segura al clúster de Kubernetes con una red local. El servicio VPN IPSec de strongSwan proporciona un canal de comunicaciones de extremo a extremo seguro sobre Internet que está basado en la suite de protocolos
Internet Protocol Security (IPSec) estándar del sector. Para configurar una conexión segura entre el clúster y una red local, [configure y despliegue el servicio VPN IPSec strongSwan](#vpn-setup) directamente en un pod del clúster.

- **{{site.data.keyword.BluDirectLink}}**:
[{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link) le permite crear una conexión privada directa entre entornos de red remotos y {{site.data.keyword.containerlong_notm}} sin tener que direccionar sobre Internet público. Las ofertas de {{site.data.keyword.Bluemix_notm}} Direct Link resultan útiles cuando se deben implementar cargas de trabajo híbridas, cargas de trabajo entre proveedores, transferencias de datos grandes o frecuentes o cargas de trabajo privadas. Para elegir una conexión de {{site.data.keyword.Bluemix_notm}} Direct Link y configurar una conexión de {{site.data.keyword.Bluemix_notm}} Direct Link, consulte [Iniciación a IBM Cloud {{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-) en la documentación de {{site.data.keyword.Bluemix_notm}} Direct Link.

- **Virtual Router Appliance (VRA) o Fortigate Security Appliance (FSA)**: Podría elegir entre configurar un [VRA (Vyatta)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) o un [FSA](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations) para configurar un punto final de VPN IPSec. Esta opción es útil si tiene un clúster grande, desea acceder a varios clústeres sobre una sola VPN o necesita una VPN basada en ruta. Para configurar VRA, consulte [Configuración de la conectividad de VPN con VRA](#vyatta).

## Utilización del diagrama de Helm del servicio VPN IPSec de strongSwan
{: #vpn-setup}

Utilice un diagrama de Helm para configurar y desplegar el servicio VPN IPSec de strongSwan dentro de un pod de Kubernetes.
{:shortdesc}

Puesto que strongSwan está integrado en su clúster, no necesita un dispositivo de pasarela externo. Cuando se establece una conectividad de VPN, las rutas se configuran automáticamente en todos los nodos trabajadores del clúster. Estas rutas permiten la conectividad bidireccional a través del túnel VPN entre pods en cualquier nodo trabajador y el sistema remoto. Por ejemplo, en el diagrama siguiente se muestra cómo una app en {{site.data.keyword.containerlong_notm}} puede comunicarse con un servidor local a través de una conexión VPN strongSwan:

<img src="images/cs_vpn_strongswan.png" width="700" alt="Exponer una app en {{site.data.keyword.containerlong_notm}} utilizando un equilibrador de carga de red (NLB)" style="width:700px; border-style: none"/>

1. Una app en el clúster, `myapp`, recibe una solicitud desde un servicio Ingress o LoadBalancer. Dicha app debe conectarse de forma segura a los datos en su red local.

2. La solicitud al centro de datos local se reenvía al pod de VPN IPSec strongSwan. La dirección IP de destino se utiliza para determinar qué paquetes de red deben enviarse al pod de VPN IPSec strongSwan.

3. La solicitud se cifra y envía a través del túnel VPN al centro de datos local.

4. La solicitud entrante pasa a través del cortafuegos local y se entrega al punto final de túnel VPN (direccionador) donde se descifra.

5. El punto final de túnel VPN (direccionador) reenvía la solicitud al servidor o sistema principal local, dependiendo de la dirección IP de destino especificada en el paso 2. Los datos necesarios se envían a través de la conexión VPN a `myapp` mediante el mismo proceso.

## Consideraciones sobre el servicio VPN de strongSwan
{: #strongswan_limitations}

Antes de utilizar el diagrama de Helm strongSwan, revise las siguientes consideraciones y limitaciones.
{: shortdesc}

* El diagrama de Helm strongSwan requiere que el punto final VPN remoto haya habilitado el cruce de NAT. El cruce de NAT requiere el puerto UDP 4500, además del puerto IPSec de IPSec predeterminado de 500. Los dos puertos UDP deben estar permitidos a través de cualquier cortafuegos que esté configurado.
* El diagrama de Helm strongSwan no da soporte a VPN IPSec basadas en rutas.
* El diagrama de Helm strongSwan da soporte a VPN IPSec que utilizan claves precompartidas, pero no da soporte a VPN IPSec que requieren certificados.
* El diagrama de Helm strongSwan no permite que varios clústeres y otros recursos de IaaS compartan una sola conexión VPN.
* El diagrama de Helm strongSwan se ejecuta como una pod de Kubernetes dentro del clúster. El rendimiento de VPN se ve afectado por el uso de memoria y de red por parte de Kubernetes y de otros pods que se ejecuten en el clúster. Si tiene un entorno en el que el rendimiento sea un factor clave, tenga en cuenta la posibilidad de utilizar una solución VPN que se ejecute fuera del clúster en hardware dedicado.
* El diagrama de Helm strongSwan ejecuta un solo pod de VPN como punto final de túnel IPSec. Si el pod falla, el clúster reinicia el pod. Sin embargo, puede experimentar un breve periodo de tiempo mientras se inicia el nuevo pod y se restablece la conexión VPN. Si necesita una recuperación de errores más rápida o una solución de alta disponibilidad más elaborada, tenga en cuenta la posibilidad de utilizar una solución VPN que se ejecute fuera del clúster en hardware dedicado.
* El diagrama de Helm strongSwan no proporciona métricas ni supervisión del tráfico de red que fluye a través de la conexión VPN. Para ver una lista de las herramientas de supervisión a las que se da soporte, consulte [Servicios de registro y de supervisión](/docs/containers?topic=containers-supported_integrations#health_services).

Los usuarios del clúster pueden utilizar el servicio VPN de strongSwan para conectar con el nodo maestro de Kubernetes a través del punto final de servicio privado. Sin embargo, la comunicación con el nodo maestro de Kubernetes sobre el punto final de servicio privado debe pasar a través del rango de direcciones IP <code>166.X.X.X</code>, que no se puede direccionar desde una conexión VPN. Puede exponer el punto final de servicio privado del nodo maestro para los usuarios del clúster [utilizando un equilibrador de carga de red (NLB) privado](/docs/containers?topic=containers-clusters#access_on_prem). El NLB privado expone el punto final de servicio privado del nodo maestro como una dirección IP interna de clúster `172.21.x.x` a la que puede acceder el pod VPN de strongSwan. Si solo habilita el punto final de servicio privado, puede utilizar el panel de control de Kubernetes o puede habilitar temporalmente el punto final de servicio público para crear el NLB privado.
{: tip}

<br />


## Configuración de la VPN strongSwan en un clúster multizona
{: #vpn_multizone}

Los clústeres multizona proporcionan alta disponibilidad para las apps en el caso de que se produzca una interrupción al poner las instancias de la app a disposición de los nodos trabajadores en varias zonas. Sin embargo, configurar el servicio de VPN strongSwan en un clúster multizona es más complejo que configurar strongSwan en un clúster de una sola zona.
{: shortdesc}

Antes de configurar strongSwan en un clúster multizona, intente desplegar un diagrama de Helm de strongSwan en un clúster de una sola zona. Cuando se establece por primera vez una conexión VPN entre un clúster de una sola zona y una red local, puede determinar con más facilidad los valores de cortafuegos de red remota que son importantes para una configuración de strongSwan multizona:
* Algunos puntos finales de VPN remota tienen valores, como por ejemplo `leftid` o `rightid`, en el archivo `ipsec.conf`. Si tiene estos valores, compruebe si debe establecer el valor de `leftid` en la dirección IP del túnel IPSec de VPN.
*	Si la conexión es de entrada al clúster desde la red remota, compruebe si el punto final de VPN remota puede volver a establecer la conexión VPN con una dirección IP distinta en caso de que se produzca una anomalía en el equilibrador de carga en una zona.

Para empezar a trabajar con strongSwan en un clúster multizona, elija una de las opciones siguientes.
* Si puede utilizar una conexión VPN de salida, puede optar por configurar solo un despliegue de VPN de strongSwan. Consulte [Configuración de una conexión VPN de salida desde un clúster multizona](#multizone_one_outbound).
* Si necesita una conexión VPN de entrada, los valores de configuración que puede utilizar dependen de si el punto final de la VPN remota se puede configurar de modo que vuelva a establecer la conexión VPN con otra IP pública del equilibrador de carga si se detecta una interrupción.
  * Si el punto final de VPN remota puede volver a establecer automáticamente la conexión VPN con otra IP, puede optar por configurar solo un despliegue de VPN de strongSwan. Consulte [Configuración de una conexión de VPN de entrada a un clúster multizona](#multizone_one_inbound).
  * Si el punto final de VPN remota no puede volver a establecer automáticamente la conexión VPN con otra IP, debe desplegar un servicio VPN de strongSwan independiente en cada zona. Consulte [Configuración de una conexión VPN en cada zona de un clúster multizona](#multizone_multiple).

Intente configurar el entorno de modo que solo necesite un despliegue de VPN strongSwan para una conexión de VPN de salida o de entrada con el clúster multizona. Si debe configurar VPN de strongSwan separadas en cada zona, asegúrese de planificar cómo gestionar esta complejidad añadida y el aumento en el uso de recursos.
{: note}

### Configuración de una sola conexión VPN de salida desde un clúster multizona
{: #multizone_one_outbound}

La solución más sencilla para configurar el servicio VPN de strongSwan en un clúster multizona consiste en utilizar una única conexión VPN de salida que flote entre distintos nodos trabajadores en todas las zonas de disponibilidad del clúster.
{: shortdesc}

Cuando la conexión VPN es de salida del clúster multizona, solo se necesita un despliegue de strongSwan. Si se elimina un nodo trabajador o se experimenta un tiempo de inactividad, `kubelet` vuelve a planificar el pod de VPN en un nuevo nodo trabajador. Si una zona de disponibilidad experimenta una interrupción, `kubelet` vuelve a planificar el pod de VPN en un nuevo nodo trabajador en otra zona.

1. [Configurar un diagrama de Helm de VPN de strongSwan](/docs/containers?topic=containers-vpn#vpn_configure). Cuando siga los pasos de dicha sección, asegúrese de especificar los valores siguientes:
    - `ipsec.auto`: Cambie su valor por `start`. Las conexiones son de salida del clúster.
    - `loadBalancerIP`: No especifique una dirección IP. Deje este valor en blanco.
    - `zoneLoadBalancer`: Especifique una dirección IP pública de equilibrador de carga para cada zona en la que tenga nodos trabajadores. [Puede comprobar las direcciones IP públicas disponibles](/docs/containers?topic=containers-subnets#review_ip) o [liberar una dirección IP usada](/docs/containers?topic=containers-subnets#free). Puesto que el pod de VPN de strongSwan se puede planificar en un nodo trabajador de cualquier zona, la lista de las IP garantiza que se puede utilizar una IP del equilibrador de carga en cualquier zona en la que se haya planificado el pod de VPN.
    - `connectUsingLoadBalancerIP`: Establezca su valor en `true`. Cuando el pod de VPN de strongSwan se planifica en un nodo trabajador, el servicio strongSwan selecciona la dirección IP del equilibrador de carga que se encuentra en la misma zona y utiliza esta dirección IP para establecer la conexión de salida.
    - `local.id`: Especifique un valor fijo que reciba soporte del punto final de VPN remota. Si el punto final de VPN remota requiere que establezca la opción `local.id` (valor `leftid` en `ipsec.conf`) en la dirección IP pública del túnel VPN IPSec, establezca `local.id` en `%loadBalancerIP`. Este valor establece automáticamente el valor de `leftid` de `ipsec.conf` en la dirección IP del equilibrador de carga que se utiliza para la conexión.

2. En el cortafuegos de red remota, permita las conexiones VPN entrantes de IPSec desde las direcciones IP públicas que aparecen en la lista del valor `zoneLoadBalancer`.

3. Configure el punto final de VPN remota de modo que permita una conexión de VPN entrante desde cada una de las posibles IP del equilibrador de carga que aparecen en la lista del valor `zoneLoadBalancer`.

### Configuración de una sola conexión VPN de entrada a un clúster multizona
{: #multizone_one_inbound}

Cuando necesite conexiones VPN entrantes y el punto final de VPN remota puede volver a establecer automáticamente la conexión VPN con otra IP cuando se detecta una anomalía, puede utilizar una única conexión VPN de entrada que flote entre distintos nodos trabajadores en todas las zonas de disponibilidad del clúster.
{: shortdesc}

El punto final de VPN remota puede establecer la conexión VPN con cualquiera de los equilibradores de carga de strongSwan en cualquiera de las zonas. La solicitud de entrada se envía al pod de VPN independientemente de la zona en la que se encuentre el pod de VPN. Las respuestas procedentes del pod de VPN se envían de nuevo a través del equilibrador de carga original al punto final de VPN remota. Esta opción garantiza una alta disponibilidad debido a que `kubelet` vuelve a planificar el pod de VPN en un nuevo nodo trabajador si se elimina un nodo trabajador o si se experimenta un tiempo de inactividad. Además, si una zona de disponibilidad experimenta una interrupción, el punto final de VPN remota puede volver a establecer la conexión VPN con la dirección IP del equilibrador de carga de otra zona para que se pueda acceder al pod de VPN.

1. [Configurar un diagrama de Helm de VPN de strongSwan](/docs/containers?topic=containers-vpn#vpn_configure). Cuando siga los pasos de dicha sección, asegúrese de especificar los valores siguientes:
    - `ipsec.auto`: Cambie su valor por `add`. Las conexiones son de entrada en el clúster.
    - `loadBalancerIP`: No especifique una dirección IP. Deje este valor en blanco.
    - `zoneLoadBalancer`: Especifique una dirección IP pública de equilibrador de carga para cada zona en la que tenga nodos trabajadores. [Puede comprobar las direcciones IP públicas disponibles](/docs/containers?topic=containers-subnets#review_ip) o [liberar una dirección IP usada](/docs/containers?topic=containers-subnets#free).
    - `local.id`: Si el punto final de VPN remota requiere que establezca la opción `local.id` (valor `leftid` en `ipsec.conf`) en la dirección IP pública del túnel VPN IPSec, establezca `local.id` en `%loadBalancerIP`. Este valor establece automáticamente el valor de `leftid` de `ipsec.conf` en la dirección IP del equilibrador de carga que se utiliza para la conexión.

2. En el cortafuegos de red remota, permita las conexiones VPN de salida de IPSec a las direcciones IP públicas que aparecen en la lista del valor `zoneLoadBalancer`.

### Configuración de una conexión VPN de entrada en cada zona de un clúster multizona
{: #multizone_multiple}

Cuando necesite que las conexiones VPN de entrada y el punto final de VPN remota no pueden volver a establecer automáticamente la conexión VPN con otra IP, debe desplegar un servicio VPN de strongSwan independiente en cada zona.
{: shortdesc}

El punto final de VPN remota se debe actualizar para que establezca una conexión VPN independiente con un equilibrador de carga en cada una de las zonas. Además, debe configurar valores específicos de zona en el punto final de VPN remota para que cada una de estas conexiones VPN sea exclusiva. Asegúrese de que estas múltiples conexiones VPN entrantes estén activas en todo momento.

Después de desplegar cada diagrama de Helm, cada despliegue de VPN de strongSwan se inicia como un servicio de equilibrador de carga de Kubernetes en la zona correcta. Las solicitudes de entrada a dicha IP pública se reenvían al pod de VPN que también está asignado a la misma zona. Si la zona experimenta una interrupción, las conexiones VPN que se establecen en las otras zonas no se ven afectadas.

1. [Configure un diagrama de Helm de VPN de strongSwan](/docs/containers?topic=containers-vpn#vpn_configure) para cada zona. Cuando siga los pasos de dicha sección, asegúrese de especificar los valores siguientes:
    - `loadBalancerIP`: Especifique una dirección IP pública disponible de equilibrador de carga que esté en la zona en la que despliega este servicio strongSwan. [Puede comprobar las direcciones IP públicas disponibles](/docs/containers?topic=containers-subnets#review_ip) o [liberar una dirección IP usada](/docs/containers?topic=containers-subnets#free).
    - `zoneSelector`: Especifique la zona en la que desea planificar el pod de VPN.
    - Es posible que se necesiten valores adicionales, como por ejemplo `zoneSpecificRoutes`, `remoteSubnetNAT`,
`localSubnetNAT` o `enableSingleSourceIP`, en función de los recursos a los que se debe acceder a través de la VPN. Consulte el paso siguiente si desea información más detallada.

2. Configure valores específicos de zona en ambos lados del túnel de VPN para asegurarse de que cada conexión VPN sea exclusiva. En función de los recursos a los que se debe acceder a través de la VPN, tiene dos opciones para conseguir que las conexiones se puedan distinguir:
    * Si los pods del clúster deben acceder a los servicios de la red local remota:
      - `zoneSpecificRoutes`: Establezca su valor en `true`. Este valor restringe la conexión VPN a una única zona del clúster. Los pods de una zona específica solo utilizan la conexión VPN que se ha configurado para dicha zona específica. Esta solución reduce el número de pods de strongSwan que se necesitan para dar soporte a varias VPN en un clúster multizona, mejora el rendimiento de VPN porque el tráfico de VPN solo fluye a los nodos trabajadores ubicados en la zona actual y garantiza que la conectividad VPN para cada zona no se vea afectada por la conectividad VPN, la interrupción de pods o las detenciones de zona de otras zonas. Observe que no es necesario configurar `remoteSubnetNAT`. Varias VPN que utilicen el valor `zoneSpecificRoutes` pueden tener el mismo valor de `remote.subnet` porque el direccionamiento se configura por zona.
      - `enableSingleSourceIP`: Establezca este valor en `true` y establezca el valor de `local.subnet` en una única dirección IP /32. Esta combinación de valores oculta todas las direcciones IP privadas del clúster tras una sola dirección IP /32. Esta dirección IP /32 exclusiva permite a la red local remota devolver respuestas sobre la conexión VPN correcta al pod correcto en el clúster que ha iniciado la solicitud. Tenga en cuenta que la única dirección IP /32 que se configura para la opción `local.subnet` debe ser exclusiva en cada configuración de VPN de strongSwan.
    * Si las aplicaciones de la red local remota deben acceder a servicios en el clúster:    
      - `localSubnetNAT`: Asegúrese de que una aplicación de la red remota local pueda seleccionar una conexión VPN específica a la que enviar y de la que recibir tráfico del clúster. En cada configuración de Helm de strongSwan, utilice `localSubnetNAT`
para identificar de forma exclusiva los recursos del clúster a los que puede acceder la aplicación local remota. Puesto que se establecen varias VPN desde la red local remota al clúster, debe añadir lógica a la aplicación en la red local para que pueda seleccionar la VPN que debe utilizar cuando acceda a servicios del clúster. Tenga en cuenta que se puede acceder a los servicios del clúster a través de varias subredes distintas en función de lo que haya configurado para `localSubnetNAT` en cada configuración de VPN de strongSwan.
      - `remoteSubnetNAT`: Asegúrese de que un pod del clúster utilice la misma conexión VPN para devolver el tráfico a la red remota. En cada archivo de despliegue strongSwan, correlacione la subred local remota con una subred exclusiva mediante el valor `remoteSubnetNAT`. El tráfico que recibe un pod del clúster desde un `remoteSubnetNAT` específico de VPN se vuelve a enviar a ese mismo `remoteSubnetNAT` específico de VPN y se transmite a través de esa misma conexión VPN.

3. Configure el software de punto final de VPN remota de modo que establezca una conexión VPN independiente con la IP del equilibrador de carga en cada zona.

<br />


## Configuración del diagrama de Helm strongSwan
{: #vpn_configure}

Antes de instalar el diagrama de Helm strongSwan, debe elegir su configuración de strongSwan.
{: shortdesc}

Antes de empezar:
* Instale una pasarela de VPN IPSec en su centro de datos local.
* Asegúrese de que tiene el [rol de servicio **Escritor** o **Gestor** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre el espacio de nombres `default`.
* [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
  * **Nota**: todas las configuraciones de strongSwan están permitidas en clústeres estándares. Si utiliza un clúster gratuito, solo puede elegir una conexión VPN de salida en el [Paso 3](#strongswan_3). Las conexiones de VPN de entrada requieren un equilibrador de carga en el clúster y los equilibradores de carga no están disponibles para clústeres gratuitos.

### Paso 1: Obtenga el diagrama de Helm strongSwan
{: #strongswan_1}

Instale Helm y obtenga el diagrama de Helm strongSwan para ver las posibles configuraciones.
{: shortdesc}

1.  [Siga las instrucciones](/docs/containers?topic=containers-helm#public_helm_install) para instalar el cliente de Helm en la máquina local, instale el servidor Helm (tiller) con una cuenta de servicio y añada el repositorio de Helm de {{site.data.keyword.Bluemix_notm}}. Tenga en cuenta que se requiere Helm versión 2.8 o posterior.

2.  Verifique que el tiller se ha instalado con una cuenta de servicio.

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    Salida de ejemplo:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. Guarde los valores de configuración predeterminados para el diagrama de Helm de strongSwan en un archivo YAML local.

    ```
    helm inspect values iks-charts/strongswan > config.yaml
    ```
    {: pre}

4. Abra el archivo `config.yaml`.

### Paso 2: Configure los valores básicos de IPSec
{: #strongswan_2}

Para controlar el establecimiento de la conexión VPN, modifique los siguientes valores básicos de IPSec.
{: shortdesc}

Para obtener más información sobre cada valor, lea la documentación que se proporciona en el archivo `config.yaml` para el diagrama de Helm.
{: tip}

1. Si el punto final de túnel VPN local no admite `ikev2` como protocolo para inicializar la conexión, cambie el valor de `ipsec.keyexchange` por `ikev1`.
2. Establezca `ipsec.esp` como una lista de algoritmos de autenticación y cifrado de ESP que el punto final de túnel de VPN local utiliza para la conexión.
    * Si `ipsec.keyexchange` se establece en `ikev1`, se debe especificar este valor.
    * Si `ipsec.keyexchange` se establece en `ikev2`, este valor es opcional.
    * Si deja este valor en blanco, se utilizan para la conexión los algoritmos predeterminados de strongSwan `aes128-sha1,3des-sha1`.
3. Establezca `ipsec.ike` como una lista de algoritmos de autenticación y cifrado de IKE/ISAKMP que el punto final de túnel de VPN local utiliza para la conexión. Los algoritmos deben ser específicos en el formato `encryption-integrity[-prf]-dhgroup`.
    * Si `ipsec.keyexchange` se establece en `ikev1`, se debe especificar este valor.
    * Si `ipsec.keyexchange` se establece en `ikev2`, este valor es opcional.
    * Si deja este valor en blanco, se utilizan para la conexión los algoritmos predeterminados de strongSwan `aes128-sha1-modp2048,3des-sha1-modp1536`.
4. Cambie el valor de `local.id` por la serie que desee utilizar para identificar el lado del clúster de Kubernetes local que utiliza el punto final de túnel VPN. El valor predeterminado es `ibm-cloud`. Algunas implementaciones de VPN requieren que utilice la dirección IP pública para el punto final local.
5. Cambie el valor de `remote.id` por la serie que desee utilizar para identificar el lado local remoto que utiliza el punto final de túnel VPN. El valor predeterminado es `on-prem`. Algunas implementaciones de VPN requieren que utilice la dirección IP pública para el punto final remoto.
6. Cambie el valor de `preshared.secret` por el secreto precompartido que la pasarela de punto final de túnel VPN local utiliza para la conexión. Este valor se almacena en `ipsec.secrets`.
7. Opcional: establezca `remote.privateIPtoPing` en cualquier dirección IP privada de la subred remota para ejecutar ping como parte de la prueba de validación de conectividad de Helm.

### Paso 3: Seleccione una conexión VPN de entrada o de salida
{: #strongswan_3}

Cuando configure una conexión VPN strongSwan, debe elegir si la conexión VPN es de entrada al clúster o de salida del clúster.
{: shortdesc}

<dl>
<dt>Entrada</dt>
<dd>El punto final VPN local de la red remota inicia la conexión VPN y el clúster escucha la conexión.</dd>
<dt>Salida</dt>
<dd>El clúster inicia la conexión VPN y el punto final VPN local de la red remota escucha la conexión.</dd>
</dl>

Si utiliza un clúster gratuito, solo puede elegir una conexión VPN de salida. Las conexiones de VPN de entrada requieren un equilibrador de carga en el clúster y los equilibradores de carga no están disponibles para clústeres gratuitos.

Para establecer una conexión VPN de entrada, modifique los valores siguientes:
1. Verifique que `ipsec.auto` esté establecido en `add`.
2. Opcional: establezca `loadBalancerIP` en una dirección IP pública portátil para el servicio VPN de strongSwan. Especificar una dirección IP es útil cuando necesita una dirección IP estable como, por ejemplo, cuando debe designar las direcciones IP permitidas a través de un cortafuegos local. El clúster debe tener disponible al menos una dirección IP pública de equilibrador de carga. [Puede comprobar las direcciones IP públicas disponibles](/docs/containers?topic=containers-subnets#review_ip) o [liberar una dirección IP usada](/docs/containers?topic=containers-subnets#free).
    * Si deja este valor en blanco, se utilizará una de las direcciones IP públicas portátiles disponibles.
    * También debe configurar la dirección IP pública que seleccione o la dirección IP pública que se asigna al punto final de VPN del clúster en el punto final VPN local.

Para establecer una conexión VPN de salida, modifique los valores siguientes:
1. Cambie `ipsec.auto` por `start`.
2. Establezca `remote.gateway` en la dirección IP pública para el punto final VPN local en la red remota.
3. Elija una de las opciones siguientes para la dirección IP para el punto final VPN del clúster:
    * **Dirección IP pública de la pasarela privada del clúster**: si los nodos trabajadores solo están conectados a una VLAN privada, la solicitud VPN de salida se direcciona a través de la pasarela privada para acceder a Internet. La dirección IP pública de la pasarela privada se utiliza para la conexión VPN.
    * **Dirección IP pública del nodo trabajador en el que se ejecuta el pod strongSwan**: si el nodo trabajador en el que se ejecuta el pod strongSwan está conectado a una VLAN pública, se utiliza la dirección IP pública del nodo trabajador para la conexión VPN.
        <br>
        * Si el pod strongSwan se suprime y se vuelve a planificar en otro nodo trabajador del clúster, la dirección IP pública de VPN cambia. El punto final VPN local de la red remota debe permitir que se establezca la conexión VPN a partir de la dirección IP pública de cualquiera de los nodos trabajadores del clúster.
        * Si el punto final VPN remoto no puede manejar las conexiones VPN desde varias direcciones IP públicas, limite los nodos en los que se despliega el pod de VPN strongSwan. Establezca `nodeSelector` en las direcciones IP de nodos trabajadores específicos o en una etiqueta de nodo trabajador. Por ejemplo, el valor `kubernetes.io/hostname: 10.232.xx.xx` permite que el pod de VPN se despliegue únicamente en ese nodo trabajador. El valor `strongswan: vpn` restringe la ejecución del pod de VPN a los nodos trabajadores que tengan esa etiqueta. Puede utilizar cualquier etiqueta de nodo trabajador. Para permitir el uso de distintos nodos trabajadores con distintos despliegues de diagrama de Helm, utilice `strongswan: <release_name>`. Para obtener una alta disponibilidad, seleccione al menos dos nodos trabajadores.
    * **Dirección IP pública del servicio strongSwan**: para establecer la conexión utilizando la dirección IP del servicio VPN strongSwan, establezca `connectUsingLoadBalancerIP` en `true`. La dirección IP del servicio strongSwan es una dirección IP pública portátil que puede especificar en el valor `loadBalancerIP` o bien una dirección IP pública portátil disponible que se asigna automáticamente al servicio.
        <br>
        * Si elige seleccionar una dirección IP mediante el valor `loadBalancerIP`, el clúster debe tener al menos una dirección IP pública de equilibrador de carga disponible. [Puede comprobar las direcciones IP públicas disponibles](/docs/containers?topic=containers-subnets#review_ip) o [liberar una dirección IP usada](/docs/containers?topic=containers-subnets#free).
        * Todos los nodos trabajadores del clúster deben estar en la misma VLAN pública. De lo contrario, debe utilizar el valor `nodeSelector` para asegurarse de que el pod de VPN se despliega en un nodo trabajador en la misma VLAN pública que `loadBalancerIP`.
        * Si `connectUsingLoadBalancerIP` tiene el valor `true` e `ipsec.keyexchange` tiene el valor `ikev1`, debe establecer para `enableServiceSourceIP` el valor `true`.

### Paso 4: Acceda a recursos de clúster a través de la conexión VPN
{: #strongswan_4}

Determinar los recursos de clúster a los que debe poder acceder la red remota a través de la conexión VPN.
{: shortdesc}

1. Añada los CIDR de una o varias subredes del clúster a la configuración `local.subnet`. Debe configurar los CIDR de subred locales en el punto final VPN local. Esta lista puede incluir las subredes siguientes:  
    * El CIDR de subred del pod de Kubernetes: `172.30.0.0/16`. La comunicación bidireccional está habilitada entre todos los pods del clúster y cualquiera de los hosts de las subredes de red remotas que se muestran en el valor `remote.subnet`. Si tiene que impedir que algún host `remote.subnet` acceda a los pods del clúster por motivos de seguridad, no añada la subred de pods de Kubernetes al valor `local.subnet`.
    * El CIDR de subred del servicio de Kubernetes: `172.21.0.0/16`. Las direcciones IP de servicio proporcionan una forma de exponer varios pods de app que se despliegan en varios nodos trabajadores detrás de una única IP.
    * Si las apps están expuestas por un servicio NodePort en una red privada en un Ingress ALB privado, añada el CIDR de subred privada del nodo trabajador. Recupere los tres primeros octetos de la dirección de IP privada del nodo trabajador con el mandato `ibmcloud ks worker <cluster_name>`. Por ejemplo, si es `10.176.48.xx`, anote `10.176.48`. A continuación, obtenga el CIDR de subred privado del trabajador ejecutando el siguiente mandato, sustituyendo `<xxx.yyy.zz>` por el octeto que ha recuperado anteriormente: `ibmcloud sl subnet list | grep <xxx.yyy.zzz>`. **Nota**: si se añade un nodo trabajador en una nueva subred privada, debe añadir el nuevo CIDR de subred privada al valor `local.subred` y al punto final VPN local. Luego se debe reiniciar la conexión VPN.
    * Si tiene apps expuestas por los servicios de LoadBalancer en la red privada, añada el CIDR de subred privada gestionada por el usuario del clúster. Para encontrar estos valores, ejecute `ibmcloud ks cluster-get --cluster <cluster_name> --showResources`. En la sección **VLAN**, busque los CIDR que tengan los valores **Public** o `false`. **Nota**: Si `ipsec.keyexchange` se establece en `ikev1`, solo puede especificar una subred. Sin embargo, puede utilizar el valor `localSubnetNAT` para combinar varias subredes de clúster en una única subred.

2. Opcional: vuelva a correlacionar las subredes del clúster utilizando el valor `localSubnetNAT`. NAT (conversión de direcciones de red) para subredes supone una solución temporal para los conflictos de subred entre la red del clúster y la red remota local. Puede utilizar NAT para volver a correlacionar las subredes IP locales privadas del clúster, la subred del pod (172.30.0.0/16) o la subred del servicio del pod (172.21.0.0/16) a una subred privada diferente. El túnel VPN ve subredes IP correlacionadas en lugar de las subredes originales. La correlación ocurre antes de que los paquetes se envíen a través del túnel VPN, así como después de que lleguen del túnel VPN. Puede exponer las redes correlacionadas y las no correlacionadas al mismo tiempo a través de la VPN. Para habilitar NAT, puede añadir una subred entera o direcciones IP individuales.
    * Si añade toda una subred en el formato `10.171.42.0/24=10.10.10.0/24`, la correlación es de 1 a 1: todas las direcciones IP de la subred de red interna se correlacionan con la subred de red externa, y viceversa.
    * Si añade direcciones IP individuales en el formato `10.171.42.17/32=10.10.10.2/32,10.171.42.29/32=10.10.10.3/32`, solo estas direcciones IP internas se correlacionan con las direcciones IP externas especificadas.

3. Opcional para diagramas de Helm strongSwan de la versión 2.2.0 y posteriores: oculte todas las direcciones IP del clúster que hay tras una sola dirección IP estableciendo para `enableSingleSourceIP` el valor `true`. Esta opción proporciona una de las configuraciones más seguras para la conexión VPN porque no se permiten conexiones entre la red remota y el clúster.
    <br>
    * Este valor requiere que todo el flujo de datos a través de la conexión VPN sea de salida, independientemente de si la conexión VPN se establece desde el clúster o desde la red remota.
    * `local.subnet` se debe establecer solo en una subred /32.

4. Opcional para diagramas de Helm strongSwan de versión 2.2.0 y posteriores: habilite el servicio strongSwan para direccionar las solicitudes de entrada procedentes de la red remota a un servicio que exista fuera del clúster utilizando el valor `localNonClusterSubnet`.
    <br>
    * El servicio que no es de clúster debe existir en la misma red privada o en una red privada a la que puedan acceder los nodos trabajadores.
    * El nodo trabajador que no es de clúster no puede iniciar el tráfico con la red remota a través de la conexión VPN, pero el nodo que no es de clúster puede ser el destino de las solicitudes de entrada procedentes de la red remota.
    * Debe obtener una lista de los CIDR de las subredes que no son de clúster en el valor `local.subnet`.

### Paso 5: Acceda a recursos de red remotos a través de la conexión VPN
{: #strongswan_5}

Determine los recursos de red remotos a los que debe poder acceder el clúster a través de la conexión VPN.
{: shortdesc}

1. Añada los CIDR de una o varias subredes privadas locales al valor `remote.subnet`. **Nota**: Si `ipsec.keyexchange` se establece en `ikev1`, solo puede especificar una subred.
2. Opcional para los diagramas de Helm strongSwan de la versión 2.2.0 y posteriores: vuelva a correlacionar las subredes de red remota utilizando el valor `remoteSubnetNAT`. NAT (conversión de direcciones de red) para subredes supone una solución temporal para los conflictos de subred entre la red del clúster y la red remota local. Puede utilizar NAT para volver a correlacionar las subredes IP remotas con otra subred privada. La correlación ocurre antes de que los paquetes se envíen a través del túnel VPN. Los pods del clúster ven las subredes IP correlacionadas en lugar de las subredes originales. Antes de que los pods envíen de nuevo los datos a través del túnel de VPN, la subred IP que se ha vuelto a correlacionar se cambia de nuevo a la subred real que está utilizando la red remota. Puede exponer las redes correlacionadas y las no correlacionadas al mismo tiempo a través de la VPN.

### Paso 6 (opcional): Habilite la supervisión con la integración del webhook de Slack
{: #strongswan_6}

Para supervisar el estado de la VPN de strongSwan, puede configurar un webhook para publicar automáticamente mensajes de conectividad de VPN en un canal de Slack.
{: shortdesc}

1. Inicie sesión en el espacio de trabajo de Slack.

2. Vaya a la [página de app WebHooks entrantes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://slack.com/apps/A0F7XDUAZ-incoming-webhooks).

3. Pulse **Solicitud de instalación**. Si esta app no aparece en la configuración de Slack, póngase en contacto con el propietario del espacio de trabajo de Slack.

4. Una vez que se haya aprobado la solicitud e instalación, pulse **Añadir configuración**.

5. Elija un canal de Slack o cree un canal nuevo al que enviar los mensajes de VPN.

6. Copie del URL de webhook que se genera. El formato de URL tiene un aspecto similar al siguiente:
  ```
  https://hooks.slack.com/services/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
  ```
  {: screen}

7. Para verificar que el webhook de Slack está instalado, envíe un mensaje de prueba al URL de webhook ejecutando el mandato siguiente:
    ```
    curl -X POST -H 'Content-type: application/json' -d '{"text":"VPN test message"}' <webhook_URL>
    ```
    {: pre}

8. Vaya al canal de Slack que haya elegido para verificar que el mensaje de prueba se envía correctamente.

9. En el archivo `config.yaml` del diagrama de Helm, configure el webhook para supervisar su conexión VPN.
    1. Cambie `monitoring.enable` a `true`.
    2. Añada las direcciones IP privadas o puntos finales HTTP de la subred remota a
`monitoring.privateIPs` o `monitoring.httpEndpoints`, para los cuales desea asegurarse de que sean accesibles a través de la conexión VPN. Por ejemplo, puede añadir la IP del valor
`remote.privateIPtoPing` a `monitoring.privateIPs`.
    3. Añada el URL de webhook a `monitoring.slackWebhook`.
    4. Cambie los demás valores de `monitoring` opcionales según sea necesario.

### Paso 7: Despliegue el diagrama de Helm
{: #strongswan_7}

Despliegue el diagrama de Helm strongSwan en el clúster con las configuraciones que ha elegido anteriormente.
{: shortdesc}

1. Si necesita configurar valores más avanzados, siga la documentación que se proporciona para cada valor en el diagrama de Helm.

3. Guarde el archivo `config.yaml` actualizado.

4. Instale el diagrama de Helm en el clúster con el archivo `config.yaml` actualizado.

    Si tiene varios despliegues de VPN en un solo clúster, puede evitar conflictos de denominación y diferenciar entre los despliegues eligiendo nombres de release más descriptivos que `vpn`. Para evitar el truncamiento del nombre de release, limite el nombre de release a 35 caracteres o menos.
    {: tip}

    ```
    helm install -f config.yaml --name=vpn iks-charts/strongswan
    ```
    {: pre}

5. Compruebe el estado de despliegue del diagrama. Cuando el diagrama está listo, el campo **STATUS**, situado cerca de la parte superior de la salida, tiene el valor `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

6. Una vez desplegado el diagrama, verifique que se han utilizado los valores actualizados del archivo `config.yaml`.

    ```
    helm get values vpn
    ```
    {: pre}

## Prueba y verificación de la conectividad VPN de strongSwan
{: #vpn_test}

Después de desplegar el diagrama de Helm, pruebe la conectividad de VPN.
{:shortdesc}

1. Si la VPN de la pasarela local no está activa, inicie la VPN.

2. Establezca la variable de entorno `STRONGSWAN_POD`.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

3. Compruebe el estado de la VPN. Un estado `ESTABLISHED` significa que la conexión VPN se ha realizado correctamente.

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    Salida de ejemplo:

    ```
    Security Associations (1 up, 0 connecting):
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    * Cuando intenta establecer la conectividad de VPN con el diagrama de Helm de strongSwan, es probable que el estado de VPN no sea `ESTABLISHED` la primera vez. Puede que necesite comprobar los valores de punto final de VPN local y cambiar el archivo de configuración varias veces antes de que la conexión sea correcta:
        1. Ejecute `helm delete --purge <release_name>`
        2. Corrija los valores incorrectos en el archivo de configuración.
        3. Ejecute `helm install -f config.yaml --name=<release_name> ibm/strongswan`
      También puede ejecutar más comprobaciones en el paso siguiente.

    * Si el pod de VPN está en estado `ERROR` o sigue bloqueándose y reiniciándose, puede que se deba a la validación de parámetro de los valores de `ipsec.conf` en la correlación de configuración del diagrama.
        1. Compruebe los errores de validación en los registros del pod de strongSwan ejecutando `kubectl logs $STRONGSWAN_POD`.
        2. Si existen errores de validación, ejecute `helm delete --purge <release_name>`
        3. Corrija los valores incorrectos en el archivo de configuración.
        4. Ejecute `helm install -f config.yaml --name=<release_name> ibm/strongswan`

4. También puede probar la conectividad de VPN ejecutando las cinco pruebas de Helm que se incluyen en la definición del diagrama de strongSwan.

    ```
    helm test vpn
    ```
    {: pre}

    * Si pasa todas las pruebas, la conexión VPN de strongSwan está configurada correctamente.
    * Si alguno de los pasos fallan, continúe en el siguiente paso.

5. Puede consultar la salida de una prueba que ha fallado en los registros del pod de prueba.

    ```
    kubectl logs <test_program>
    ```
    {: pre}

    Algunas de las pruebas tienen requisitos que son valores opcionales en la configuración de VPN. Si alguna de las pruebas falla, los errores pueden ser aceptables en función de si ha especificado estos valores opcionales. Consulte la tabla siguiente para obtener más información sobre cada prueba y por qué puede fallar.
    {: note}

    {: #vpn_tests_table}
    <table>
    <caption>Descripción de las pruebas de conectividad de VPN de Helm</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de las pruebas de conectividad de VPN de Helm</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td>Valida la sintaxis del archivo <code>ipsec.conf</code> que se genera desde el archivo <code>config.yaml</code>. Esta prueba puede fallar debido a valores incorrectos en el archivo <code>config.yaml</code>.</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>Comprueba que la conexión VPN tiene el estado <code>ESTABLISHED</code>. Esta prueba puede fallar por las siguientes razones:<ul><li>Diferencias entre los valores del archivo <code>config.yaml</code> y los valores del punto final de VPN local.</li><li>Si el clúster está en modalidad "de escucha" (<code>ipsec.auto</code> se establece en <code>add</code>), la conexión no se establece en el lado local.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td>Hace ping a la dirección IP pública de <code>remote.gateway</code> que ha configurado en el archivo <code>config.yaml</code>. Si la conexión VPN tiene el estado <code>ESTABLISHED</code>, puede pasar por alto el resultado de esta prueba. Si la conexión VPN no tiene el estado <code>ESTABLISHED</code>, esta prueba podría fallar por los motivos siguientes:<ul><li>No haber especificado una dirección IP de pasarela VPN local. Si <code>ipsec.auto</code> se establece en <code>start</code>, la dirección IP de <code>remote.gateway</code> es obligatoria.</li><li>Los paquetes ICMP (ping) están siendo bloqueados por un cortafuegos.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>Hace ping a la dirección IP privada <code>remote.privateIPtoPing</code> de la pasarela VPN local desde el pod de VPN en el clúster. Esta prueba puede fallar por las siguientes razones:<ul><li>No haber especificado una dirección IP <code>remote.privateIPtoPing</code>. Si intencionalmente no especifica una dirección IP, este error es aceptable.</li><li>No haber especificado el CIDR de subred del pod del clúster, <code>172.30.0.0/16</code>, en la lista <code>local.subnet</code>.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>Hace ping a la dirección IP privada <code>remote.privateIPtoPing</code> de la pasarela VPN local desde el nodo trabajador en el clúster. Esta prueba puede fallar por las siguientes razones:<ul><li>No haber especificado una dirección IP <code>remote.privateIPtoPing</code>. Si intencionalmente no especifica una dirección IP, este error es aceptable.</li><li>No haber especificado el CIDR de subred privada del nodo trabajador del clúster en la lista <code>local.subnet</code>.</li></ul></td>
    </tr>
    </tbody></table>

6. Suprima el diagrama de Helm actual.

    ```
    helm delete --purge vpn
    ```
    {: pre}

7. Abra el archivo `config.yaml` y corrija los valores incorrectos.

8. Guarde el archivo `config.yaml` actualizado.

9. Instale el diagrama de Helm en el clúster con el archivo `config.yaml` actualizado. Las propiedades actualizadas se almacenan en una correlación de configuración para el diagrama.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

10. Compruebe el estado de despliegue del diagrama. Cuando el diagrama está listo, el campo **STATUS**, situado cerca de la parte superior de la salida, tiene el valor `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

11. Una vez desplegado el diagrama, verifique que se han utilizado los valores actualizados del archivo `config.yaml`.

    ```
    helm get values vpn
    ```
    {: pre}

12. Limpie los pods de prueba actuales.

    ```
    kubectl get pods -a -l app=strongswan-test
    ```
    {: pre}

    ```
    kubectl delete pods -l app=strongswan-test
    ```
    {: pre}

13. Ejecute las pruebas de nuevo.

    ```
    helm test vpn
    ```
    {: pre}

<br />


## Limitación del tráfico de VPN de strongSwan por espacio de nombres o nodo trabajador
{: #limit}

Si tiene un clúster de un solo arrendatario, o si tiene un clúster multiarrendatario en el que los recursos del clúster se comparten entre los arrendatarios, puede [limitar el tráfico de VPN para cada despliegue de strongSwan a los pods de determinados espacios de nombres](#limit_namespace). Si tiene un clúster multiarrendatario en el que los recursos del clúster están dedicados a los arrendatarios, puede
[limitar el tráfico de VPN para cada despliegue de strongSwan a los nodos trabajadores dedicados a cada arrendatario](#limit_worker).
{: shortdesc}

### Limitación del tráfico de VPN de strongSwan por espacio de nombres
{: #limit_namespace}

Cuando dispone de un clúster de un solo arrendatario o multiarrendatario, puede limitar el tráfico de VPN a los pods en determinados espacios de nombres únicamente.
{: shortdesc}

Por ejemplo, digamos que desea que los pods de un solo espacio de nombres específico, `my-secure-namespace`, envíen y reciban datos a través de la VPN. No desea que los pods de otros espacios de nombres, como `kube-system`, `ibm-system` o `default`, accedan a la red local. Para limitar el tráfico de VPN únicamente a
`my-secure-namespace`, puede crear políticas de red globales de Calico.

Antes de utilizar esta solución, revise las siguientes consideraciones y limitaciones.
* No necesita desplegar el diagrama de Helm strongSwan en el espacio de nombres especificado. El pod de VPN de strongSwan y el conjunto de daemons de rutas se pueden desplegar en `kube-system` o en cualquier otro espacio de nombres. Si la VPN de strongSwan no se despliega en el espacio de nombres especificado, la prueba de Helm `vpn-strongswan-ping-remote-ip-1` fallará. Este error es esperado y aceptable. La prueba ejecuta ping sobre la dirección IP privada `remote.privateIPtoPing` de la pasarela VPN local desde un pod que no está en el espacio de nombres que tiene acceso directo a la subred remota. Sin embargo, el pod de VPN sigue siendo capaz de reenviar tráfico a los pods de los espacios de nombres que no tienen rutas a la subred remota, y el tráfico puede seguir fluyendo correctamente. El estado de VPN sigue siendo
`ESTABLISHED` y los pods del espacio de nombres especificado se pueden conectar a través de la VPN.

* Las políticas de red globales de Calico de los pasos siguientes no impiden que los pods de Kubernetes que utilizan redes de host puedan enviar y recibir datos a través de la VPN. Cuando se configura un pod con redes de host, la app que se ejecuta en el pod puede estar a la escucha en las interfaces de red del nodo trabajador en el que se encuentra. Estos pods de redes de host pueden existir en cualquier espacio de nombres. Para determinar qué pods tienen redes de host, ejecute `kubectl get pods --all-namespaces -o wide` y busque los pods que no tienen una dirección IP de pod de `172.30.0.0/16`. Si desea evitar que los pods de redes de host puedan enviar y recibir datos a través de la VPN, puede establecer las opciones siguientes en el archivo de despliegue
`values.yaml`: `local.subnet: 172.30.0.0/16` y `enablePodSNAT: false`. Estos valores de configuración exponen todos los pods de Kubernetes de toda la conexión VPN en la red remota. No obstante, únicamente serán accesibles a través de la VPN aquellos pods que se encuentren en el espacio de nombres seguro especificado.

Antes de empezar:
* [Despliegue el diagrama de Helm strongSwan](#vpn_configure) y [asegúrese de que la conectividad de VPN funciona correctamente](#vpn_test).
* [Instale y configure la CLI de Calico](/docs/containers?topic=containers-network_policies#cli_install).

Para limitar el tráfico de VPN a un espacio de nombres determinado:

1. Cree una política de red global de Calico denominada `allow-non-vpn-outbound.yaml`. Esta política permite que todos los espacios de nombres puedan seguir enviando tráfico de salida a todos los destinos, excepto a la subred remota a la que accede la VPN de strongSwan. Sustituya `<remote.subnet>` por la `remote.subnet` (subred remota) que ha especificado en el archivo de configuración `values.yaml` de Helm. Para especificar varias subredes remotas, consulte la [Documentación de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/globalnetworkpolicy).
    ```yaml
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: allow-non-vpn-outbound
    spec:
      selector: has(projectcalico.org/namespace)
      egress:
      - action: Allow
        destination:
          notNets:
          - <remote.subnet>
      order: 900
      types:
      - Egress
    ```
    {: codeblock}

2. Aplique la política.

    ```
    calicoctl apply -f allow-non-vpn-outbound.yaml --config=filepath/calicoctl.cfg
    ```
    {: pre}

3. Cree otra política de red global de Calico denominada `allow-vpn-from-namespace.yaml`. Esta política permite que únicamente un espacio de nombres especificado pueda enviar tráfico saliente a la subred remota a la que accede la VPN de strongSwan. Sustituya `<namespace>` por el espacio de nombres que puede acceder a la VPN y `<remote.subnet>` por la `remote.subnet` (subred remota) que ha especificado en el archivo de configuración `values.yaml` de Helm. Para especificar varios espacios de nombres o subredes remotas, consulte la [Documentación de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/globalnetworkpolicy).
    ```yaml
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: allow-vpn-from-namespace
    spec:
      selector: projectcalico.org/namespace == "<namespace>"
      egress:
      - action: Allow
        destination:
          nets:
          - <remote.subnet>
      order: 900
      types:
      - Egress
    ```
    {: codeblock}

4. Aplique la política.

    ```
    calicoctl apply -f allow-vpn-from-namespace.yaml --config=filepath/calicoctl.cfg
    ```
    {: pre}

5. Verifique que se crean las políticas de red globales en el clúster.
    ```
    calicoctl get GlobalNetworkPolicy -o wide --config=filepath/calicoctl.cfg
    ```
    {: pre}

### Limitación del tráfico de VPN de strongSwan por nodo trabajador
{: #limit_worker}

Cuando tiene varios despliegues de VPN de strongSwan en un clúster multiarrendatario, puede limitar el tráfico de VPN para cada despliegue a nodos trabajadores específicos que estén dedicados a cada arrendatario.
{: shortdesc}

Al desplegar un diagrama de Helm strongSwan, se crea un despliegue de VPN de strongSwan. Los pods de VPN de strongSwan se despliegan en cualquier nodo trabajador limpio. Además, se crea un conjunto de daemons de Kubernetes. Este conjunto de daemons configura automáticamente rutas en todos los nodos trabajadores limpios del clúster con cada una de las subredes remotas. El pod de VPN de strongSwan utiliza las rotas en los nodos trabajadores para reenviar solicitudes a la subred remota en la red local.

No se configuran rutas en nodos antagónicos a menos que especifique el antagonismo en el valor
`tolerations` del archivo `value.yaml`. Al establecer como antagónicos los nodos trabajadores, puede evitar que se configuren rutas VPN en dichos trabajadores. A continuación, puede especificar el antagonismo en el valor
`tolerations` únicamente para el despliegue de VPN que desea permitir en los trabajadores antagónicos. De este modo, los pods de VPN de strongSwan del despliegue del diagrama de Helm de un arrendatario solo utilizan las rutas en los nodos trabajadores de dicho arrendatario para reenviar el tráfico a través de la conexión VPN con la subred remota.

Antes de utilizar esta solución, revise las siguientes consideraciones y limitaciones.
* De forma predeterminada, Kubernetes coloca los pods de app en los nodos trabajadores no contaminados que estén disponibles. Para asegurarse de que esta solución funciona correctamente, cada arrendatario debe asegurarse primero de desplegar sus pods de app únicamente en trabajadores que sean antagónicos para el arrendatario correcto. Además, cada nodo trabajador antagónico debe tener también una tolerancia para permitir que los pods de app se puedan colocar en el nodo. Para obtener más información sobre antagonismos y tolerancias, consulte la [Documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/).
* Es posible que los recursos del clúster no se utilicen de manera óptima debido a que ningún arrendatario puede colocar pods de app los nodos compartidos no antagónicos.

Los pasos siguientes para limitar el tráfico de VPN de strongSwan por nodo trabajador utilizan este caso de ejemplo: digamos que tiene un clúster de {{site.data.keyword.containerlong_notm}} multiarrendatario con seis nodos trabajadores. El clúster da soporte al arrendatario A y al arrendatario B. Puede establecer como antagónicos los nodos trabajadores de las maneras siguientes:
* Se establecen como antagónicos dos nodos trabajadores, de manera que solo se planifican en los trabajadores los pods del arrendatario A.
* Se establecen como antagónicos dos nodos trabajadores, de manera que solo se planifican en los trabajadores los pods del arrendatario B.
* Dos nodos trabajadores no se establecen como antagónicos, ya que se necesitan al menos 2 nodos trabajadores para los pods de VPN de strongSwan y la IP del equilibrador de carga en el que se ejecuta.

Para limitar el tráfico de VPN a los nodos antagónicos para cada arrendatario:

1. Para limitar el tráfico de VPN únicamente a los trabajadores dedicados al arrendatario A en este ejemplo, debe especificar la tolerancia siguiente (`toleration`) en el archivo `values.yaml` para el diagrama de Helm strongSwan del arrendatario A:
    ```
    tolerations:
     - key: dedicated
       operator: "Equal"
       value: "tenantA"
       effect: "NoSchedule"
    ```
    {: codeblock}
    Esta tolerancia permite que se ejecute el conjunto de daemons de ruta en los dos nodos trabajadores que tienen el antagonismo
`dedicated="tenantA"` y en los dos nodos trabajadores no antagónicos. Los pods de VPN de strongSwan para este despliegue se ejecutan en los dos nodos trabajadores no antagónicos.

2. Para limitar el tráfico de VPN únicamente a los trabajadores dedicados al arrendatario B en este ejemplo, debe especificar la tolerancia siguiente (`toleration`) en el archivo `values.yaml` para el diagrama de Helm strongSwan del arrendatario B:
    ```
    tolerations:
     - key: dedicated
       operator: "Equal"
       value: "tenantB"
       effect: "NoSchedule"
    ```
    {: codeblock}
    Esta tolerancia permite que se ejecute el conjunto de daemons de ruta en los dos nodos trabajadores que tienen el antagonismo
`dedicated="tenantB"` y en los dos nodos trabajadores no antagónicos. Los pods de VPN de strongSwan para este despliegue también se ejecutan en los dos nodos trabajadores no antagónicos.

<br />


## Actualización del diagrama de Helm de strongSwan
{: #vpn_upgrade}

Actualice el diagrama de Helm de strongSwan para asegurarse de que esté actualizado.
{:shortdesc}

Para actualizar el diagrama de Helm de strongSwan a la última versión:

  ```
  helm upgrade -f config.yaml <release_name> ibm/strongswan
  ```
  {: pre}

## Inhabilitación del servicio VPN IPSec de strongSwan
{: vpn_disable}

Puede inhabilitar la conexión VPN suprimiendo el diagrama de Helm.
{:shortdesc}

  ```
  helm delete --purge <release_name>
  ```
  {: pre}

<br />


## Utilización de un dispositivo de direccionador virtual (Virtual Router Appliance, VRA)
{: #vyatta}

[Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) proporciona el último sistema operativo Vyatta 5600 para servidores nativos x86. Puede utilizar VRA como una pasarela VPN para conectarse de forma segura a una red local.
{:shortdesc}

Todo el tráfico de red privado y público que entre o salga de las VLAN del clúster se direcciona a través de VRA. Puede utilizar VRA como un punto final de VPN para crear un túnel IPSec cifrado entre servidores en la infraestructura (Softlayer) de IBM Cloud y los recursos locales. Por ejemplo, en el diagrama siguiente se muestra cómo una app en un nodo trabajador privado en {{site.data.keyword.containerlong_notm}} puede comunicarse con un servidor local a través de una conexión VPN Vyatta:

<img src="images/cs_vpn_vyatta.png" width="725" alt="Exponer una app en {{site.data.keyword.containerlong_notm}} mediante un equilibrador de carga" style="width:725px; border-style: none"/>

1. Una app en el clúster, `myapp2`, recibe una solicitud desde un servicio Ingress o LoadBalancer. Dicha app debe conectarse de forma segura a los datos en su red local.

2. Puesto que `myapp2` se encuentra en un nodo trabajador que está en una VLAN privada únicamente, el dispositivo VRA actúa como una conexión segura entre los nodos trabajadores y la red local. El dispositivo VRA utiliza la dirección IP de destino para determinar qué paquetes de red deben enviarse a la red local.

3. La solicitud se cifra y envía a través del túnel VPN al centro de datos local.

4. La solicitud entrante pasa a través del cortafuegos local y se entrega al punto final de túnel VPN (direccionador) donde se descifra.

5. El punto final de túnel VPN (direccionador) reenvía la solicitud al servidor o sistema principal local, dependiendo de la dirección IP de destino especificada en el paso 2. Los datos necesarios se envían a través de la conexión VPN a `myapp2` mediante el mismo proceso.

Para configurar un Virtual Router Appliance:

1. [Solicitar un VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started).

2. [Configurar la VLAN privada en el VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans).

3. Para habilitar una conexión VPN utilizando el VRA, [configure VRRP en el VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#high-availability-vpn-with-vrrp).

Si tiene un dispositivo direccionador existente y luego añade un clúster, las nuevas subredes portátiles que se soliciten para el clúster no se configuran en el dispositivo direccionador. Para poder utilizar los servicios de red, debe habilitar el direccionamiento entre las subredes de la misma VLAN [habilitando la distribución de VLAN](/docs/containers?topic=containers-subnets#subnet-routing). Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.
{: important}
