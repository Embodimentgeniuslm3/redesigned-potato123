---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, multi az, multi-az, szr, mzr

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


# Planificación de la configuración de la red del clúster
{: #plan_clusters}

Diseñe una configuración de red para el clúster de {{site.data.keyword.containerlong}} que satisfaga las necesidades de las cargas de trabajo y del entorno.
{: shortdesc}

En un clúster de {{site.data.keyword.containerlong_notm}}, las apps contenerizadas se alojan en hosts de cálculo que se denominan nodos trabajadores. Los nodos trabajadores están gestionados por el maestro de Kubernetes. La configuración de la comunicación entre los nodos trabajadores y el nodo maestro de Kubernetes, otros servicios, Internet u otras redes privadas depende de cómo se configure la red de la infraestructura de IBM Cloud (SoftLayer).

¿Es la primera vez que crea un clúster? Pruebe primero nuestra [guía de aprendizaje](/docs/containers?topic=containers-cs_cluster_tutorial) y vuelva aquí cuando esté preparado para planificar clústeres listos para la producción.
{: tip}

Para planificar la configuración de la red del clúster, primero debe [entender los conceptos básicos sobre la red del clúster](#plan_basics). A continuación, puede consultar tres potenciales configuraciones de red de clúster que se ajustan a distintos entornos, que incluyen la [ejecución de cargas de trabajo de app conectadas a Internet](#internet-facing), la [ampliación de un centro de datos local con acceso público limitado](#limited-public) y la [ampliación de un centro de datos local solo en la red privada](#private_clusters).

## Visión general de los conceptos básicos de red de clúster
{: #plan_basics}

Cuando cree el clúster, debe elegir una configuración de red de modo que ciertos componentes del clúster se puedan comunicar entre sí y con redes o servicios externos al clúster.
{: shortdesc}

* [Comunicación entre nodo trabajador y nodo trabajador](#worker-worker): todos los nodos trabajadores deben poder comunicarse entre sí en la red privada. En muchos casos, se debe permitir la comunicación entre varias VLAN privadas para permitir que los trabajadores de distintas VLAN y en diferentes zonas se conecten entre sí.
* [Comunicación entre nodo trabajador y maestro y entre usuario y maestro](#workeruser-master): los nodos trabajadores y los usuarios autorizados del clúster se pueden comunicar con el nodo maestro de Kubernetes de forma segura sobre la red pública con TLS o sobre la red privada a través de puntos finales privados de servicio.
* [Comunicación entre nodo trabajador y otros servicios de {{site.data.keyword.Bluemix_notm}} o redes locales](#worker-services-onprem): permita que los nodos trabajadores se comuniquen de forma segura con otros servicios de {{site.data.keyword.Bluemix_notm}}, como {{site.data.keyword.registrylong}}, y con una red local.
* [Comunicación externa con apps que se ejecutan en nodos trabajadores](#external-workers): permita solicitudes públicas o privadas en el clúster, así como solicitudes fuera del clúster con un punto final público.

### Comunicación entre nodo trabajador y nodo trabajador
{: #worker-worker}

Cuando se crea un clúster, los nodos trabajadores del clúster se conectan automáticamente a una VLAN privada y se pueden conectar opcionalmente a una VLAN pública. Una VLAN configura un grupo de nodos trabajadores y pods como si estuvieran conectadas a la misma conexión física y ofrece un canal para la conectividad entre los nodos trabajadores.
{: shortdesc}

**Conexiones de VLAN para los nodos trabajadores**</br> Todos los nodos trabajadores deben estar conectados a una VLAN privada para que cada nodo trabajador pueda enviar y recibir información a otros nodos trabajadores. Cuando se crea un clúster con nodos trabajadores que también están conectados a una VLAN pública, los nodos trabajadores se pueden comunicar con el nodo maestro de Kubernetes automáticamente sobre la VLAN pública y sobre la VLAN privada si habilita el punto final de servicio privado. La VLAN pública también proporciona conectividad de red pública para que pueda exponer las apps del clúster en Internet. Sin embargo, si necesita proteger sus apps de la interfaz pública, hay varias opciones disponibles para proteger el clúster, como el uso de políticas de red de Calico o el aislamiento de la carga de trabajo de la red externa a los nodos trabajadores de extremo.
* Clústeres gratuitos: en los clústeres gratuitos, los nodos trabajadores del clúster se conectan de forma predeterminada a una VLAN privada y VLAN pública propiedad de IBM. Puesto que IBM controla las VLAN, las subredes y las direcciones IP, no puede crear clústeres multizona ni añadir subredes a un clúster, y solo puede utilizar servicios NodePort para exponer la app.</dd>
* Clústeres estándares: en los clústeres estándares, la primera vez que crea un clúster en una zona, se suministra automáticamente una VLAN pública y una VLAN privada en dicha zona en su cuenta de infraestructura de IBM Cloud (SoftLayer). Si especifica que los nodos trabajadores deben estar conectados únicamente a una VLAN privada, se suministrará automáticamente una VLAN privada en dicha zona. Para los demás clústeres que cree en dicha zona, puede especificar el par de VLAN que desee utilizar. Puede reutilizar las mismas VLAN pública y privada que se han creado porque varios clústeres pueden compartir las VLAN.

Para obtener más información sobre las VLAN, las subredes y las direcciones IP, consulte [Visión general de la red en {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-subnets#basics).

**Comunicación de nodos trabajadores a través de subredes y VLAN**</br> En varias situaciones, se debe permitir la comunicación de los componentes del clúster entre varias VLAN privadas. Por ejemplo, si desea crear un clúster multizona, si tiene varias VLAN para un clúster o si tiene varias subredes en la misma VLAN, los nodos trabajadores de distintas subredes de la misma VLAN o de distintas VLAN no se pueden comunicar automáticamente entre sí. Debe habilitar VRF (Virtual Routing and Forwarding) o la distribución de VLAN para su cuenta de infraestructura de IBM Cloud (SoftLayer).

* [Virtual Routing and Forwarding (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud): VRF permite que todas las VLAN y subredes privadas de la cuenta de la infraestructura se comuniquen entre sí. Además, se necesita VRF para permitir que los nodos trabajadores y el nodo maestro se comuniquen sobre el punto final de servicio privado y se comuniquen con otras instancias de {{site.data.keyword.Bluemix_notm}} que dan soporte a puntos finales de servicio privados. Para habilitar VRF, ejecute `ibmcloud account update --service-endpoint-enable true`. La salida de este mandato le solicita que abra un caso de soporte para habilitar la cuenta para que utilice VRF y puntos finales de servicio. VRF elimina la opción de distribución de VLAN para la cuenta, ya que todas las VLAN se pueden comunicar.</br></br>
Cuando VRF está habilitado, cualquier sistema que esté conectado a cualquiera de las VLAN privadas de la misma cuenta de {{site.data.keyword.Bluemix_notm}} se puede comunicar con los nodos trabajadores del clúster. Puede aislar el clúster de otros sistemas de la red privada aplicando [Políticas de red privada de Calico](/docs/containers?topic=containers-network_policies#isolate_workers).</dd>
* [Distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning): si no puede o no desea habilitar VRF, por ejemplo si no necesita que se pueda acceder al nodo maestro en la red privada o si utiliza un dispositivo de pasarela para acceder al nodo maestro a través de la VLAN pública, habilite la distribución de VLAN. Por ejemplo, si tiene un dispositivo de pasarela existente y luego añade un clúster, las nuevas subredes portátiles que se soliciten para el clúster no se configuran en el dispositivo de pasarela, pero la distribución de VLAN permite el direccionamiento entre las subredes. Para habilitar la distribución de VLAN, necesita el [permiso de la infraestructura](/docs/containers?topic=containers-users#infra_access) **Red > Gestionar distribución de VLAN de red** o bien puede solicitar al propietario de la cuenta que lo habilite. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`. No puede habilitar el punto final de servicio privado si elige habilitar la distribución de VLAN en lugar de VRF.

</br>

### Comunicación entre nodo trabajador y maestro y entre usuario y maestro
{: #workeruser-master}

Se debe configurar un canal de comunicación para que los nodos trabajadores puedan establecer una conexión con el nodo maestro de Kubernetes. Puede permitir que los nodos trabajadores y el nodo maestro de Kubernetes se comuniquen habilitando solo el punto final de servicio público, puntos finales de servicio público y privado o solo el punto final de servicio privado.
{: shortdesc}

Para proteger la comunicación sobre los puntos finales de servicio público y privado, {{site.data.keyword.containerlong_notm}} configura automáticamente una conexión OpenVPN entre el maestro de Kubernetes y el nodo trabajador cuando se crea el clúster. Los nodos trabajadores se comunican de forma segura con el nodo maestro a través de certificados TLS y el maestro se comunica con los nodos trabajadores a través de la conexión OpenVPN.

**Solo punto final de servicio público**</br> Si no desea o no puede habilitar VRF para su cuenta, los nodos trabajadores se pueden conectar automáticamente al nodo maestro de Kubernetes sobre la VLAN pública a través del punto final de servicio público.
* La comunicación entre los nodos trabajadores y el maestro
se establece de forma segura sobre la red pública a través del punto final de servicio público.
* Los usuarios autorizados del clúster pueden acceder de forma pública al nodo maestro a través del punto final de servicio público. Los usuarios del clúster pueden acceder de forma segura al nodo maestro de Kubernetes sobre internet, por ejemplo para ejecutar mandatos `kubectl`.

**Puntos finales de servicio público y privado**</br> Para que los usuarios del clúster puedan acceder al nodo maestro de forma pública o privada, puede habilitar los puntos finales de servicio público y privado. Se necesita VRF en la cuenta de {{site.data.keyword.Bluemix_notm}} y debe habilitar la cuenta para que utilice puntos finales de servicio. Para habilitar VRF y los puntos finales de servicio, ejecute `ibmcloud account update --service-endpoint-enable true`.
* Si los nodos trabajadores están conectados a las VLAN pública y privada, la comunicación entre los nodos trabajadores y el nodo maestro se establece tanto sobre la red privada a través del punto final de servicio privado como sobre la red pública a través del punto final de servicio público. Direccionando la mitad del tráfico de trabajador a maestro por el punto final público y la otra mitad por el punto final privado, la comunicación de maestro a trabajador está protegida de posibles interrupciones en la red pública o privada. Si los nodos trabajadores solo están conectados a las VLAN privadas, la comunicación entre los nodos trabajadores y el nodo maestro se establece sobre la red privada solo a través del punto final de servicio privado.
* Los usuarios autorizados del clúster pueden acceder de forma pública al nodo maestro a través del punto final de servicio público. Se puede acceder de forma privada al nodo maestro a través del punto final de servicio privado si los usuarios autorizados del clúster están en la red privada de {{site.data.keyword.Bluemix_notm}} o están conectados a la red privada a través de una conexión VPN o {{site.data.keyword.Bluemix_notm}} Direct Link. Tenga en cuenta que debe [exponer el punto final maestro a través de un equilibrador de carga de red privado](/docs/containers?topic=containers-clusters#access_on_prem) para que los usuarios puedan acceder al nodo maestro a través de una VPN o de una conexión {{site.data.keyword.Bluemix_notm}} Direct Link.

**Solo punto final de servicio privado**</br> Para solo se pueda acceder al nodo maestro de forma privada, puede habilitar el punto final de servicio privado. Se necesita VRF en la cuenta de {{site.data.keyword.Bluemix_notm}} y debe habilitar la cuenta para que utilice puntos finales de servicio. Para habilitar VRF y los puntos finales de servicio, ejecute `ibmcloud account update --service-endpoint-enable true`. Tenga en cuenta que el uso únicamente de un punto final de servicio privado no incurre en cargos facturados ni de ancho de banda según medición.
* La comunicación entre los nodos trabajadores y el nodo maestro se establece sobre la red privada a través del punto final de servicio privado.
* Se accede de forma privada al nodo maestro si los usuarios autorizados del clúster están en la red privada de {{site.data.keyword.Bluemix_notm}} o están conectados a la red privada a través de una conexión VPN o DirectLink. Tenga en cuenta que debe [exponer el punto final maestro a través de un equilibrador de carga privado](/docs/containers?topic=containers-clusters#access_on_prem) para que los usuarios puedan acceder al nodo maestro a través de una conexión VPN o DirectLink.

</br>

### Comunicación entre nodos trabajadores y otros servicios de {{site.data.keyword.Bluemix_notm}} o redes locales
{: #worker-services-onprem}

Permita que los nodos trabajadores se comuniquen de forma segura con otros servicios de {{site.data.keyword.Bluemix_notm}}, como {{site.data.keyword.registrylong}}, y con una red local.
{: shortdesc}

**Comunicación con otros servicios de {{site.data.keyword.Bluemix_notm}} a través de la red privada o pública**</br> Los nodos trabajadores se comunican automáticamente y de forma segura con otros servicios de {{site.data.keyword.Bluemix_notm}} que dan soporte a puntos finales de servicio privados, como {{site.data.keyword.registrylong}}, a través de la red privada de infraestructura de IBM Cloud (SoftLayer). Si un servicio de {{site.data.keyword.Bluemix_notm}} no da soporte a puntos finales de servicio privados, los nodos trabajadores deben estar conectados a una VLAN pública para que se puedan comunicar de forma segura con los servicios a través de la red pública.

Si utiliza políticas de red de Calico para bloquear la red pública en el clúster, es posible que tenga que permitir el acceso a las direcciones IP públicas y privadas de los servicios que desea utilizar en las políticas de Calico. Si utiliza un dispositivo de pasarela, como por ejemplo Virtual Router Appliance (Vyatta), debe [permitir el acceso a las direcciones IP privadas de los servicios que desea utilizar](/docs/containers?topic=containers-firewall#firewall_outbound) en el cortafuegos del dispositivo de pasarela.
{: note}

**{{site.data.keyword.BluDirectLink}} para la comunicación a través de la red privada con recursos de centros de datos locales**</br> Para conectar el clúster con el centro de datos local, por ejemplo con {{site.data.keyword.icpfull_notm}}, puede configurar [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). Con {{site.data.keyword.Bluemix_notm}} Direct Link, puede crear una conexión directa y privada entre los entornos de red remotos y {{site.data.keyword.containerlong_notm}} sin direccionamiento a través de Internet público.

**Conexión de VPN IPSec de strongSwan para la comunicación a través de la red pública con recursos en centros de datos locales**
* Nodos trabajadores conectados a VLAN pública y privada: Configure un [servicio VPN IPSec de strongSwan ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.strongswan.org/about.html) directamente en el clúster. El servicio VPN IPSec de strongSwan proporciona un canal de comunicaciones de extremo a extremo seguro sobre Internet que está basado en la suite de protocolos
Internet Protocol Security (IPSec) estándar del sector. Para configurar una conexión segura entre el clúster y una red local, [configure y despliegue el servicio VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) directamente en un pod del clúster.
* Solo nodos trabajadores conectados a una VLAN privada: Configure un punto final de VPN IPSec en un dispositivo de pasarela, como por ejemplo Virtual Router Appliance (Vyatta). A continuación, [configure el servicio VPN IPSec de strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) en el clúster de modo que utilice el punto final de VPN en la pasarela. Si no desea utilizar strongSwan, puede [configurar la conectividad de VPN directamente con VRA](/docs/containers?topic=containers-vpn#vyatta).

</br>

### Comunicación externa a las apps que se ejecutan en nodos trabajadores
{: #external-workers}

Permita las solicitudes de tráfico público o privado desde el exterior del clúster a las apps que se ejecutan en nodos trabajadores.
{: shortdesc}

**Tráfico privado a las apps del clúster**</br> Cuando despliega una app en el clúster, es posible que desee que solo puedan acceder a la app los usuarios y servicios que se encuentren en la misma red privada que el clúster. El equilibrio de carga privado es ideal para hacer que la app esté disponible para las solicitudes desde fuera del clúster sin exponer la app al público en general. También puede utilizar el equilibrio de carga privado para probar el acceso, el direccionamiento de solicitudes y otras configuraciones para la app antes de exponer su app al público con los servicios de red públicos. Para permitir solicitudes de tráfico privadas desde fuera del clúster a las apps, puede crear servicios de red privados de Kubernetes, como por ejemplo nodos privados, NLB y ALB de Ingress. Luego puede utilizar políticas pre-DNAT de Calico para bloquear el tráfico destinado a NodePorts públicos de servicios de red privados. Para obtener más información, consulte [Planificación del equilibrio de carga externo privado](/docs/containers?topic=containers-cs_network_planning#private_access).

**Tráfico público a apps del clúster**</br> Para que se pueda acceder a las apps desde Internet público, puede crear NodePorts públicos, equilibradores de carga de red (NLB) y equilibradores de carga de aplicación (ALB) de Ingress. Los servicios de red públicos se conectan a esta interfaz de red pública proporcionando a su app una dirección IP pública y, opcionalmente, un URL público. Cuando se expone públicamente una app, cualquiera que tenga la dirección IP pública del servicio o el URL que haya establecido para la app puede enviar una solicitud a la app. A continuación, puede utilizar las políticas pre-DNAT de Calico para controlar el tráfico a los servicios de red públicos; por ejemplo, puede colocar en una lista blanca solo determinadas direcciones IP de origen o CIDR y bloquear el resto del tráfico. Para obtener más información, consulte [Planificación del equilibrio de carga externo público](/docs/containers?topic=containers-cs_network_planning#private_access).

Para obtener seguridad adicional, aísle las cargas de trabajo de red a los nodos trabajadores de extremo. Los nodos trabajadores de extremo pueden mejorar la seguridad de su clúster al permitir el acceso externo a un número inferior de nodos trabajadores conectados a las VLAN públicas y aislar la carga de trabajo en red. Cuando se [etiquetan nodos trabajadores como nodos de extremo](/docs/containers?topic=containers-edge#edge_nodes), los pods de ALB y NLB se despliegan solo en los nodos trabajadores especificados. Para evitar también que se ejecuten otras cargas de trabajo en los nodos de extremo, puede [establecer como antagónicos los nodos de extremo](/docs/containers?topic=containers-edge#edge_workloads). En Kubernetes versión 1.14 y posteriores, puede desplegar los NLB públicos y privados y ALB en los nodos de extremo.
Por ejemplo, si los nodos trabajadores están conectados únicamente a una VLAN privada, pero tiene que permitir el acceso público a una app en el clúster, puede crear una agrupación de nodos trabajadores de extremo en la que los nodos de extremo se conecten a las VLAN públicas y privadas. Puede desplegar NLB públicos y ALB en estos nodos de extremo para asegurarse de que solo los nodos trabajadores manejen las conexiones públicas.

Si los nodos trabajadores están conectados únicamente a una VLAN privada y utiliza un dispositivo de pasarela para establecer comunicación entre los nodos trabajadores y el nodo maestro del clúster, también puede configurar el dispositivo como un cortafuegos público o privado. Para permitir solicitudes de tráfico públicas o privadas desde fuera del clúster a sus apps, puede crear NodePorts públicos o privados, NLB y ALB de Ingress. A continuación, debe [abrir los puertos y las direcciones IP necesarios](/docs/containers?topic=containers-firewall#firewall_inbound) en el cortafuegos del dispositivo de pasarela para permitir el tráfico de entrada a estos servicios a través de la red pública o privada.
{: note}

<br />


## Caso práctico: Ejecución de cargas de trabajo de una app de cara a Internet en un clúster
{: #internet-facing}

En este caso práctico, desea ejecutar cargas de trabajo en un clúster que son accesibles para las solicitudes desde Internet de modo que los usuarios finales puedan acceder a sus apps. Desea establecer la opción de aislar el acceso público en el clúster y de controlar las solicitudes públicas que se permiten en el clúster. Además, los nodos trabajadores tienen acceso automático a todos los servicios de {{site.data.keyword.Bluemix_notm}} que desee conectar con el clúster.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_internet.png" alt="Imagen de arquitectura de un clúster que ejecuta cargas de trabajo de cara a Internet"/> <figcaption>Arquitectura de un clúster que ejecuta cargas de trabajo de cara a Internet</figcaption> </figure>
</p>

Para lograr esta configuración, cree un clúster conectando los nodos trabajadores a las VLAN públicas y privadas.

Si crea el clúster con VLAN tanto pública como privada, no podrá eliminar posteriormente todas las VLAN públicas de dicho clúster. La eliminación de todas las VLAN públicas de un clúster hace que varios componentes del clúster dejen de funcionar. En su lugar, cree una nueva agrupación de trabajadores que esté conectada únicamente a una VLAN privada.
{: note}

Puede optar por permitir la comunicación entre nodo trabajador y nodo maestro y entre usuario y nodo maestro a través de las redes pública y privada o solo a través de la red pública.
* Puntos finales de servicio públicos y privados: su cuenta debe estar habilitada con VRF y habilitada para utilizar puntos finales de servicio. La comunicación entre nodos trabajadores y nodo maestro se establece tanto sobre la red privada a través del punto final de servicio privado como sobre la red pública a través del punto final de servicio público. Los usuarios autorizados del clúster pueden acceder de forma pública al nodo maestro a través del punto final de servicio público.
* Solo punto final de servicio público: si no desea o no puede habilitar VRF para su cuenta, los nodos trabajadores y los usuarios autorizados del clúster se pueden conectar automáticamente al nodo maestro de Kubernetes sobre la red pública a través del punto final de servicio público.

Los nodos trabajadores se comunican automáticamente y de forma segura con otros servicios de {{site.data.keyword.Bluemix_notm}} que dan soporte a puntos finales de servicio privados a través de la red privada de infraestructura de IBM Cloud (SoftLayer). Si un servicio de {{site.data.keyword.Bluemix_notm}} no da soporte a puntos finales de servicio privados, los nodos trabajadores pueden comunicarse de forma segura con los servicios a través de la red pública. Puede bloquear las interfaces públicas o privadas de los nodos trabajadores mediante políticas de red de Calico para el aislamiento de red pública o de red privada. Es posible que tenga que permitir el acceso a las direcciones IP públicas y privadas de los servicios que desea utilizar en estas políticas de aislamiento de Calico.

Para exponer una app del clúster en Internet, puede crear un servicio equilibrador de carga de red (NLB) público o un servicio equilibrador de carga de aplicación (ALB) de Ingress. Puede mejorar la seguridad del clúster mediante la creación de una agrupación de nodos trabajadores etiquetados como nodos de extremo. Los pods correspondientes a los servicios de red públicos se despliegan en los nodos de extremo de modo que las cargas de trabajo de tráfico externas estén aisladas solo a unos pocos nodos trabajadores del clúster. Puede controlar más el tráfico público a los servicios de red que exponen sus apps mediante la creación de políticas pre-DNAT de Calico, como por ejemplo políticas de lista blanca y de lista negra.

Si los nodos trabajadores necesitan acceder a servicios en redes privadas fuera de su cuenta de {{site.data.keyword.Bluemix_notm}}, puede configurar y desplegar el servicio VPN IPSec de strongSwan en el clúster o aprovechar los servicios {{site.data.keyword.Bluemix_notm}} Direct Link para conectar con estas redes.

¿Está listo para empezar a trabajar con un clúster en este caso práctico? Después de planificar las configuraciones de [alta disponibilidad](/docs/containers?topic=containers-ha_clusters) y de [nodo trabajador](/docs/containers?topic=containers-planning_worker_nodes), consulte el tema sobre [Creación de clústeres](/docs/containers?topic=containers-clusters#cluster_prepare).

<br />


## Caso práctico: Ampliación del centro de datos local a un clúster de la red privada y adición de acceso público limitado
{: #limited-public}

En este caso práctico, desea ejecutar cargas de trabajo en un clúster que sean accesibles para servicios, bases de datos u otros recursos del centro de datos local. Sin embargo, es posible que tenga que proporcionar un acceso público limitado al clúster y que desee asegurarse de que cualquier acceso público esté controlado y aislado en el clúster. Por ejemplo, es posible que necesite que los nodos trabajadores accedan a un servicio de {{site.data.keyword.Bluemix_notm}} que no dé soporte a puntos finales de servicio privados y al que se deba acceder a través de la red pública. O bien, es posible que tenga que proporcionar un acceso público limitado a una app que se ejecuta en el clúster.
{: shortdesc}

Para lograr esta configuración de clúster, puede crear un cortafuegos [utilizando nodos de extremo y políticas de red Calico](#calico-pc) o [utilizando un dispositivo de pasarela](#vyatta-gateway).

### Utilización de nodos de extremo y de políticas de red de Calico
{: #calico-pc}

Permita la conectividad pública limitada a su clúster mediante el uso de nodos de extremo como una pasarela pública y políticas de red de Calico como un cortafuegos público.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_calico.png" alt="Imagen de arquitectura de un clúster que utiliza nodos de extremo y políticas de red de Calico para un acceso público seguro"/>
 <figcaption>Arquitectura de un clúster que utiliza nodos de extremo y políticas de red de Calico para un acceso público seguro</figcaption> </figure>
</p>

Con esta configuración, crea un clúster conectando los nodos trabajadores solo a una VLAN privada. Su cuenta debe estar habilitada con VRF y habilitada para utilizar puntos finales de servicio privados.

Se puede acceder al nodo maestro de Kubernetes a través del punto final de servicio privado si los usuarios autorizados del clúster están en su red privada de {{site.data.keyword.Bluemix_notm}} o si se conectan a la red privada a través de una [conexión VPN](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) o [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). Sin embargo, la comunicación con el nodo maestro de Kubernetes sobre el punto final de servicio privado debe pasar a través del rango de direcciones IP <code>166.X.X.X</code>, que no se puede direccionar desde una conexión VPN ni a través de {{site.data.keyword.Bluemix_notm}} Direct Link. Puede exponer el punto final de servicio privado del nodo maestro para los usuarios del clúster utilizando un equilibrador de carga de red (NLB) privado. El NLB privado expone el punto final de servicio privado del nodo maestro como un rango de direcciones IP <code>10.X.X.X</code> interno al que pueden acceder los usuarios con la VPN o con la conexión de {{site.data.keyword.Bluemix_notm}} Direct Link. Si solo habilita el punto final de servicio privado, puede utilizar el panel de control de Kubernetes o puede habilitar temporalmente el punto final de servicio público para crear el NLB privado.

A continuación, puede crear una agrupación de nodos trabajadores que estén conectados a las VLAN públicas y privadas y etiquetados como nodos de extremo. Los nodos de extremo pueden mejorar la seguridad de su clúster al permitir que solo se pueda acceder a unos pocos nodos trabajadores de forma externa y aislando la carga de trabajo de red a dichos nodos trabajadores.

Los nodos trabajadores se comunican automáticamente y de forma segura con otros servicios de {{site.data.keyword.Bluemix_notm}} que dan soporte a puntos finales de servicio privados a través de la red privada de infraestructura de IBM Cloud (SoftLayer). Si un servicio de {{site.data.keyword.Bluemix_notm}} no da soporte a puntos finales de servicio privados, los nodos de extremo que se conectan a una VLAN pública se pueden comunicar de forma segura con los servicios a través de la red pública. Puede bloquear las interfaces públicas o privadas de los nodos trabajadores mediante políticas de red de Calico para el aislamiento de red pública o de red privada. Es posible que tenga que permitir el acceso a las direcciones IP públicas y privadas de los servicios que desea utilizar en estas políticas de aislamiento de Calico.

Para proporcionar acceso privado a una app del clúster, puede crear un equilibrador de carga de red (NLB) privado o un equilibrador de carga de aplicación (ALB) de Ingress para exponer la app solo a la red privada. Puede bloquear todo el tráfico público a estos servicios de red que exponen sus apps mediante la creación de políticas pre-DNAT de Calico, como por ejemplo políticas para bloquear los NodePorts públicos en los nodos trabajadores. Si tiene que proporcionar un acceso público limitado a una app del clúster, puede crear un NLB o ALB públicos para exponer la app. Luego debe desplegar sus apps en estos nodos de extremo de modo que los NLB o los ALB puedan dirigir el tráfico público a los pods de la app. Puede controlar más el tráfico público a los servicios de red que exponen sus apps mediante la creación de políticas pre-DNAT de Calico, como por ejemplo políticas de lista blanca y de lista negra. Los pods correspondientes a los servicios de red tanto privados como públicos se despliegan en los nodos de extremo de modo que las cargas de trabajo de tráfico externas se restrinjan solo a unos pocos nodos trabajadores del clúster.  

Para acceder de forma segura a servicios externos a {{site.data.keyword.Bluemix_notm}} y de otras redes locales, puede configurar y desplegar el servicio VPN IPSec de strongSwan en el clúster. El pod del equilibrador de carga de strongSwan se despliega en un nodo trabajador de la agrupación de extremo, donde el pod establecer una conexión segura con la red local a través de un túnel VPN cifrado sobre la red pública. De forma alternativa, puede utilizar los servicios {{site.data.keyword.Bluemix_notm}} Direct Link para conectar el clúster a su centro de datos local solo a través de la red privada.

¿Está listo para empezar a trabajar con un clúster en este caso práctico? Después de planificar las configuraciones de [alta disponibilidad](/docs/containers?topic=containers-ha_clusters) y de [nodo trabajador](/docs/containers?topic=containers-planning_worker_nodes), consulte el tema sobre [Creación de clústeres](/docs/containers?topic=containers-clusters#cluster_prepare).

</br>

### Utilización de un dispositivo de pasarela
{: #vyatta-gateway}

Permita la conectividad pública limitada a su clúster mediante la configuración de un dispositivo de pasarela, como por ejemplo Virtual Router Appliance (Vyatta), como pasarela pública y cortafuegos.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_gateway.png" alt="Imagen de arquitectura de un clúster que utiliza un dispositivo de pasarela para el acceso público seguro"/> <figcaption>Arquitectura de un clúster que utiliza un dispositivo de pasarela para el acceso público seguro</figcaption> </figure>
</p>

Si configura los nodos trabajadores solo en una VLAN privada y no desea o no puede habilitar VRF para su cuenta, debe configurar un dispositivo de pasarela para proporcionar conectividad de red entre los nodos trabajadores y el nodo maestro a través de la red pública. Por ejemplo, puede configurar un [dispositivo direccionador virtual](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) o un [dispositivo de seguridad Fortigate](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations).

Puede configurar un dispositivo de pasarela con políticas de red personalizadas para proporcionar seguridad de red dedicada para el clúster y para detectar y solucionar problemas de intrusión en la red. Cuando configure un cortafuegos en la red pública, debe abrir los puertos y las direcciones IP necesarios para cada región de modo que el nodo maestro y los nodos trabajadores se puedan comunicar. Si también configura este cortafuegos para la red privada, también debe abrir los puertos y las direcciones IP privadas necesarios para permitir la comunicación entre los nodos trabajadores y dejar que el clúster acceda a los recursos de la infraestructura a través de la red privada. También debe habilitar la distribución de VLAN para la cuenta de modo que las subredes puedan direccionar en la misma VLAN y entre varias VLAN.

Para conectar de forma segura los nodos trabajadores y las apps a una red local o a servicios fuera de {{site.data.keyword.Bluemix_notm}}, configure un punto final de VPN IPSec en el dispositivo de pasarela y el servicio VPN IPSec de strongSwan en el clúster de modo que utilicen el punto final de VPN de pasarela. Si no desea utilizar strongSwan, puede configurar la conectividad de VPN directamente con VRA.

Los nodos trabajadores pueden comunicarse de forma segura con otros servicios de {{site.data.keyword.Bluemix_notm}} y con servicios públicos externos a {{site.data.keyword.Bluemix_notm}} a través del dispositivo de pasarela. Puede configurar el cortafuegos de modo que permita el acceso a las direcciones IP públicas y privadas solo de los servicios que desea utilizar.

Para proporcionar acceso privado a una app del clúster, puede crear un equilibrador de carga de red (NLB) privado o un equilibrador de carga de aplicación (ALB) de Ingress para exponer la app solo a la red privada. Si tiene que proporcionar un acceso público limitado a una app del clúster, puede crear un NLB o ALB públicos para exponer la app. Debido a que todo el tráfico pasa por el cortafuegos del dispositivo de pasarela, puede controlar el tráfico público a los servicios de red que exponen sus apps abriendo los puertos y las direcciones IP en el servicio en el cortafuegos para permitir el tráfico de entrada a estos servicios.

¿Está listo para empezar a trabajar con un clúster en este caso práctico? Después de planificar las configuraciones de [alta disponibilidad](/docs/containers?topic=containers-ha_clusters) y de [nodo trabajador](/docs/containers?topic=containers-planning_worker_nodes), consulte el tema sobre [Creación de clústeres](/docs/containers?topic=containers-clusters#cluster_prepare).

<br />


## Caso práctico: Ampliación del centro de datos local a un clúster de la red privada
{: #private_clusters}

En este caso práctico, desea ejecutar cargas de trabajo en un clúster de {{site.data.keyword.containerlong_notm}}. Sin embargo, desea que solo puedan acceder a estas cargas de trabajo servicios, bases de datos u otros recursos del centro de datos local, como por ejemplo {{site.data.keyword.icpfull_notm}}. Es posible que las cargas de trabajo del clúster tengan que acceder a otros servicios de {{site.data.keyword.Bluemix_notm}} que dan soporte a la comunicación sobre la red privada, como {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_extend.png" alt="Imagen de arquitectura de un clúster que se conecta a un centro de datos local en la red privada"/>
 <figcaption>Arquitectura de un clúster que se conecta a un centro de datos local en la red privada</figcaption> </figure>
</p>

Para conseguir esta configuración, crea un clúster conectando los nodos trabajadores solo a una VLAN privada. Para proporcionar conectividad entre el nodo maestro del clúster y los nodos trabajadores sobre la red privada solo a través del punto final de servicio privado, la cuenta debe estar habilitada con VRF y habilitada para utilizar puntos finales de servicio. Puesto que el clúster resulta visible para cualquier recurso de la red privada cuando VRF está habilitado, puede aislar el clúster de otros sistemas de la red privada aplicando políticas de red privada de Calico.

Se puede acceder al nodo maestro de Kubernetes a través del punto final de servicio privado si los usuarios autorizados del clúster están en su red privada de {{site.data.keyword.Bluemix_notm}} o si se conectan a la red privada a través de una [conexión VPN](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) o [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). Sin embargo, la comunicación con el nodo maestro de Kubernetes sobre el punto final de servicio privado debe pasar a través del rango de direcciones IP <code>166.X.X.X</code>, que no se puede direccionar desde una conexión VPN ni a través de {{site.data.keyword.Bluemix_notm}} Direct Link. Puede exponer el punto final de servicio privado del nodo maestro para los usuarios del clúster utilizando un equilibrador de carga de red (NLB) privado. El NLB privado expone el punto final de servicio privado del nodo maestro como un rango de direcciones IP <code>10.X.X.X</code> interno al que pueden acceder los usuarios con la VPN o con la conexión de {{site.data.keyword.Bluemix_notm}} Direct Link. Si solo habilita el punto final de servicio privado, puede utilizar el panel de control de Kubernetes o puede habilitar temporalmente el punto final de servicio público para crear el NLB privado.

Los nodos trabajadores se comunican automáticamente y de forma segura con otros servicios de {{site.data.keyword.Bluemix_notm}} que dan soporte a puntos finales de servicio privados, como {{site.data.keyword.registrylong}}, a través de la red privada de infraestructura de IBM Cloud (SoftLayer). Por ejemplo, los entornos de hardware dedicados para todas las instancias del plan estándar de {{site.data.keyword.cloudant_short_notm}} dan soporte a puntos finales de servicio privados. Si un servicio de {{site.data.keyword.Bluemix_notm}} no da soporte a puntos finales de servicio privados, el clúster no puede acceder a dicho servicio.

Para proporcionar acceso privado a una app del clúster, puede crear un equilibrador de carga de red (NLB) privado o un equilibrador de carga de aplicación (ALB) de Ingress. Estos servicios de red de Kubernetes solo exponen la app a la red privada de modo que cualquier sistema local con una conexión con la subred en la que está la IP de NLB pueda acceder a la app.

¿Está listo para empezar a trabajar con un clúster en este caso práctico? Después de planificar las configuraciones de [alta disponibilidad](/docs/containers?topic=containers-ha_clusters) y de [nodo trabajador](/docs/containers?topic=containers-planning_worker_nodes), consulte el tema sobre [Creación de clústeres](/docs/containers?topic=containers-clusters#cluster_prepare).
