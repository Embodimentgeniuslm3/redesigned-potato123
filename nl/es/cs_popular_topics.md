---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-15"

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



# Temas populares sobre {{site.data.keyword.containerlong_notm}}
{: #cs_popular_topics}

Manténgase al día con lo que está pasando con {{site.data.keyword.containerlong}}. Explore las nuevas características, obtenga consejos o conozca temas populares que otros desarrolladores ahora mismo han encontrado útiles.
{:shortdesc}



## Temas populares en abril de 2019
{: #apr19}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en abril de 2019</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>15 de abril de 2019</td>
<td>[Registro de un nombre de host de equilibrador de carga de red (NLB)](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname)</td>
<td>Una vez haya configurado los equilibradores de carga de red pública (NLB) para exponer sus apps a Internet, puede crear entradas de DNS para las IP de NLB creando nombres de host. {{site.data.keyword.Bluemix_notm}} se ocupa de la generación y mantenimiento del certificado SSL de comodín para el nombre de host. También puede configurar los supervisores de TCP/HTTP (S) para comprobar el estado de las direcciones IP de NLB detrás de cada nombre de host.</td>
</tr>
<tr>
<td>8 de abril de 2019</td>
<td>[Kubernetes Terminal en su navegador web (beta)](/docs/containers?topic=containers-cs_cli_install#cli_web)</td>
<td>Si utiliza el panel de control del clúster de la consola de {{site.data.keyword.Bluemix_notm}} para gestionar los clústeres pero desea hacer más cambios de configuración avanzados de forma rápida, ahora puede ejecutar mandatos de CLI directamente desde el navegador web en Kubernetes Terminal. En la página de detalles del clúster, inicie Kubernetes Terminal pulsando el botón **Terminal**. Tenga en cuenta que Kubernetes Terminal se publica como un complemento beta y no está pensado para su uso en clústeres de producción.</td>
</tr>
<tr>
<td>4 de abril de 2019</td>
<td>[Maestros de alta disponibilidad ahora en Sídney](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>Cuando se [crea un clúster](/docs/containers?topic=containers-clusters#clusters_ui) en una ubicación metropolitana multizona, incluyendo ahora Sídney, las réplicas del maestro de Kubernetes se distribuyen entre las zonas para ofrecer una alta disponibilidad.</td>
</tr>
</tbody></table>

## Temas populares en marzo de 2019
{: #mar19}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en marzo de 2019</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>21 de marzo de 2019</td>
<td>Introducción de puntos finales de servicio privados para el maestro de clúster de Kubernetes</td>
<td>De forma predeterminada, {{site.data.keyword.containerlong_notm}} configura el clúster con acceso a una VLAN pública y a una VLAN privada. Anteriormente, si deseaba un [clúster de solo VLAN privada](/docs/containers?topic=containers-plan_clusters#private_clusters), tenía que configurar un dispositivo de pasarela para conectar los nodos trabajadores del clúster con el maestro. Ahora puede utilizar el punto final de servicio privado. Con el punto final de servicio privado habilitado, todo el tráfico entre los nodos trabajadores y el maestro se encuentra en la red privada, sin necesidad de disponer de un dispositivo de pasarela. Además de este aumento en la seguridad, el tráfico de entrada y de salida de la red privada es [ilimitado y no se factura ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/bandwidth). Todavía puede mantener un punto final de servicio público para el acceso seguro a su maestro de Kubernetes a través de Internet, por ejemplo para ejecutar mandatos `kubectl` sin estar en la red privada.<br><br>
Para utilizar puntos finales de servicio privados, debe habilitar [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)
y [puntos finales de servicio](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started) para la cuenta de la infraestructura de IBM Cloud (SoftLayer). El clúster debe ejecutar Kubernetes versión 1.11 o posterior. Si el clúster ejecuta una versión anterior de Kubernetes, [actualice al menos a la versión 1.11](/docs/containers?topic=containers-update#update). Para obtener más información, consulte los siguientes enlaces:<ul>
<li>[Visión general de la comunicación entre maestro y trabajador con puntos finales de servicio](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master)</li>
<li>[Configuración del punto final de servicio privado](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)</li>
<li>[Conmutación entre puntos finales de servicio públicos y privados](/docs/containers?topic=containers-cs_network_cluster#migrate-to-private-se)</li>
<li>Si tiene un cortafuegos en la red privada, [adición de direcciones IP privadas para {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.registrylong_notm}} y otros servicios de {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-firewall#firewall_outbound)</li>
</ul>
<p class="important">Si cambia a un clúster de punto final de servicio solo privado, asegúrese de que el clúster todavía se puede comunicar con los otros servicios de {{site.data.keyword.Bluemix_notm}} que utilice. El [almacenamiento definido por software Portworx (SDS)](/docs/containers?topic=containers-portworx#portworx) y el [programa de escalado automático de clúster](/docs/containers?topic=containers-ca#ca) no admiten el punto final de servicio solo privado. Utilice en su lugar un clúster con puntos finales de servicio tanto públicos como privados. El [Almacenamiento de archivos basado en NFS](/docs/containers?topic=containers-file_storage#file_storage) está soportado si su clúster ejecuta Kubernetes versión 1.13.4_1513, 1.12.6_1544, 1.11.8_1550, 1.10.13_1551 o posterior.</p>
</td>
</tr>
<tr>
<td>7 de marzo de 2019</td>
<td>[El programa de escalado automático de clúster pasa de beta a GA](/docs/containers?topic=containers-ca#ca)</td>
<td>El programa de escalado automático de clúster está disponible a nivel general. Instale el plugin de Helm y empiece a escalar las agrupaciones de nodos trabajadores del clúster automáticamente para aumentar o disminuir el número de nodos trabajadores en función de las necesidades de dimensionamiento de las cargas de trabajo planificadas.<br><br>
¿Necesita ayuda o desea realizar comentarios sobre el programa de escalado automático del clúster? Si es un usuario externo, [regístrese para Slack público ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://bxcs-slack-invite.mybluemix.net/) y publique el mensaje en el canal [#cluster-autoscaler ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-container-service.slack.com/messages/CF6APMLBB). Si es empleado de IBM, publíquelo en el canal [Slack interno ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-argonauts.slack.com/messages/C90D3KZUL).</td>
</tr>
</tbody></table>




## Temas populares en febrero de 2019
{: #feb19}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en febrero de 2019</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>25 de febrero</td>
<td>Control más granular sobre la extracción de imágenes de {{site.data.keyword.registrylong_notm}}</td>
<td>Cuando despliegue las cargas de trabajo en clústeres de {{site.data.keyword.containerlong_notm}}, ahora los contenedores pueden extraer imágenes de los [nuevos nombres de dominio de `icr.io` para {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-registry_overview#registry_regions). Además, puede utilizar las políticas de acceso adaptadas en {{site.data.keyword.Bluemix_notm}} IAM para controlar el acceso a las imágenes. Para obtener más información, consulte [Visión general de cómo está autorizado el clúster para extraer imágenes](/docs/containers?topic=containers-images#cluster_registry_auth).</td>
</tr>
<tr>
<td>21 de febrero</td>
<td>[Zona disponible en México](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)</td>
<td>Se da la bienvenida a México (`mex01`) como nueva zona para clústeres en la región EE. UU. sur. Si tiene un cortafuegos, [abra los puertos del cortafuegos](/docs/containers?topic=containers-firewall#firewall_outbound) para esta zona y para las otras dentro de la región en la que se encuentra el clúster.</td>
</tr>
<tr><td>6 de febrero de 2019</td>
<td>[Istio on {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-istio)</td>
<td>Istio on {{site.data.keyword.containerlong_notm}} facilita la instalación de Istio, las actualizaciones automáticas y la gestión del ciclo de vida de los componentes de plano de control de Istio, además de la integración con las herramientas de registro y supervisión de la plataforma. Con una sola pulsación, puede obtener todos los componentes principales de Istio, rastreo adicional, supervisión y visualización, y tener la app de ejemplo BookInfo activa y en ejecución. Istio on {{site.data.keyword.containerlong_notm}} se ofrece como un complemento gestionado, por lo que {{site.data.keyword.Bluemix_notm}} mantiene actualizados todos los componentes de Istio automáticamente.</td>
</tr>
<tr>
<td>6 de febrero de 2019</td>
<td>[Knative on {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-knative_tutorial)</td>
<td>Knative es una plataforma de código abierto que amplía las prestaciones de Kubernetes para ayudarle a crear apps modernas, centradas en el código fuente y sin servidor en la parte superior del clúster de Kubernetes. Managed Knative on {{site.data.keyword.containerlong_notm}} es un complemento gestionado que integra Knative e Istio directamente con el clúster de Kubernetes. IBM prueba la versión de Knative e Istio en el complemento, que se puede utilizar en {{site.data.keyword.containerlong_notm}}.</td>
</tr>
</tbody></table>


## Temas populares en enero de 2019
{: #jan19}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en enero de 2019</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>30 de enero</td>
<td>Roles de acceso al servicio de {{site.data.keyword.Bluemix_notm}} IAM y espacios de nombres de Kubernetes</td>
<td>Ahora {{site.data.keyword.containerlong_notm}} da soporte a los [roles de acceso al servicio](/docs/containers?topic=containers-access_reference#service) de {{site.data.keyword.Bluemix_notm}} IAM. Estos roles de acceso al servicio se combinan con [RBAC de Kubernetes](/docs/containers?topic=containers-users#role-binding) para autorizar a los usuarios a realizar acciones `kubectl` dentro del clúster para gestionar los recursos de Kubernetes, como pods o despliegues. Además, puede [limitar el acceso de usuario a un espacio de nombres de Kubernetes específico](/docs/containers?topic=containers-users#platform) dentro del clúster mediante los roles de acceso al servicio de {{site.data.keyword.Bluemix_notm}} IAM. Ahora los [roles de acceso de plataforma](/docs/containers?topic=containers-access_reference#iam_platform) se utilizan para autorizar a los usuarios a realizar acciones `ibmcloud ks` para gestionar la infraestructura del clúster, como los nodos trabajadores.<br><br>Los roles de acceso al servicio de {{site.data.keyword.Bluemix_notm}} IAM se añaden automáticamente a las cuentas de {{site.data.keyword.containerlong_notm}} y a los clústeres existentes en función de los permisos que los usuarios tenían anteriormente con los roles de acceso de plataforma de IAM. Además, puede utilizar IAM para crear grupos de acceso, añadir usuarios a grupos de acceso y asignar a los grupos roles de plataforma o de acceso al servicio. Para obtener más información, consulte el blog de [Introducción
de roles de servicio y de espacios de nombres en IAM para obtener un control más granular del acceso de clúster ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/).</td>
</tr>
<tr>
<td>8 de enero</td>
<td>[Presentación beta del programa de escalado automático de clústeres](/docs/containers?topic=containers-ca#ca)</td>
<td>Escale las agrupaciones de nodos trabajadores del clúster de forma automática para aumentar o reducir el número de nodos trabajadores de la agrupación de nodos trabajadores en función de los requisitos de tamaño de las cargas de trabajo planificadas. Para probar esta versión beta, debe solicitar acceso a la lista blanca.</td>
</tr>
<tr>
<td>8 de enero</td>
<td>[Herramienta de diagnósticos y de depuración de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)</td>
<td>¿A veces le resulta difícil obtener todos los archivos YAML y otra información que necesita para solucionar un problema? Pruebe la herramienta de diagnósticos y de depuración de {{site.data.keyword.containerlong_notm}} para obtener una interfaz gráfica de usuario que le ayuda a recopilar información de clúster, de despliegue y de red.</td>
</tr>
</tbody></table>

## Temas populares en diciembre de 2018
{: #dec18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en diciembre de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>11 de diciembre</td>
<td>Soporte de SDS con Portworx</td>
<td>Gestione el almacenamiento persistente para bases de datos contenerizadas y otras apps con estado, o comparta datos entre pods de varias zonas con Portworx. [Portworx ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://portworx.com/products/introduction/) es una solución de almacenamiento definida por software (SDS) de alta disponibilidad que gestiona el almacenamiento en bloque local de los nodos trabajadores para crear una capa de almacenamiento persistente unificada para las apps. Mediante el uso de la réplica de volúmenes de cada volumen a nivel de contenedor entre varios nodos trabajadores, Portworx garantiza la persistencia de los datos y la accesibilidad a los datos en las distintas zonas. Para obtener más información, consulte [Almacenamiento de datos en almacenamiento definido por software (SDS) con Portworx](/docs/containers?topic=containers-portworx#portworx).    </td>
</tr>
<tr>
<td>6 de diciembre</td>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>Obtenga visibilidad operativa sobre el rendimiento y el estado de las apps mediante el despliegue de Sysdig como servicio de terceros en sus nodos trabajadores para reenviar métricas a {{site.data.keyword.monitoringlong}}. Para obtener más información, consulte
[Análisis de métricas para una app desplegada en un clúster de Kubernetes](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). **Nota**: si utiliza {{site.data.keyword.mon_full_notm}} con clústeres que ejecutan Kubernetes versión 1.11 o posterior, no se recopilan todas las métricas de contenedor, ya que actualmente Sysdig no tiene soporte para `containerd`.</td>
</tr>
<tr>
<td>4 de diciembre</td>
<td>[Reservas de recursos del nodo trabajador](/docs/containers?topic=containers-plan_clusters#resource_limit_node)</td>
<td>¿Está desplegando tantas apps que le preocupa sobrepasar la capacidad del clúster? Las reservas de recursos de nodo trabajador y los desalojos de Kubernetes protegen la capacidad de cálculo de su clúster para que el clúster tenga recursos suficientes para seguir activo. Solo tiene que añadir algunos nodos trabajadores y sus pods empezarán a reprogramarlos automáticamente. Las reservas de recursos de nodo trabajador se han actualizado en los últimos [cambios de parches de versión](/docs/containers?topic=containers-changelog#changelog).</td>
</tr>
</tbody></table>

## Temas populares en noviembre de 2018
{: #nov18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en noviembre de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>29 de noviembre</td>
<td>[Zona disponible en Chennai](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Se da la bienvenida a Chennai, India, como una nueva zona para clústeres en la región AP norte. Si tiene un cortafuegos, [abra los puertos del cortafuegos](/docs/containers?topic=containers-firewall#firewall) para esta zona y para las otras dentro de la región en la que se encuentra el clúster.</td>
</tr>
<tr>
<td>27 de noviembre</td>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>Añada prestaciones de gestión de registros al clúster desplegando LogDNA como servicio de terceros en sus nodos trabajadores para gestionar registros de sus contenedores de pod. Para obtener más información, consulte
[Gestión de registros de clúster de Kubernetes con {{site.data.keyword.loganalysisfull_notm}} con LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>7 de noviembre</td>
<td>Equilibrador de carga de red (NLB) 2.0 (beta)</td>
<td>Ahora puede elegir entre [NLB 1.0 o 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) para exponer de forma segura sus apps de clúster al público.</td>
</tr>
<tr>
<td>7 de noviembre</td>
<td>Kubernetes versión 1.12 está disponible</td>
<td>Ahora, puede actualizar o crear clústeres que ejecuten [Kubernetes versión 1.12](/docs/containers?topic=containers-cs_versions#cs_v112). Los clústeres de la versión 1.12 se suministran con maestros de Kubernetes de alta disponibilidad de forma predeterminada.</td>
</tr>
<tr>
<td>7 de noviembre</td>
<td>Maestros de alta disponibilidad</td>
<td>Los maestros de alta disponibilidad están disponibles en clústeres que ejecutan Kubernetes versión 1.10. Todas las ventajas descritas en la entrada anterior para los clústeres 1.11 se aplican a los clústeres 1.10, así como los [pasos de preparación](/docs/containers?topic=containers-cs_versions#110_ha-masters) que debe llevar a cabo.</td>
</tr>
<tr>
<td>1 de noviembre</td>
<td>Maestros de alta disponibilidad en clústeres que ejecutan Kubernetes versión 1.11</td>
<td>En una única zona, el maestro está altamente disponible e incluye réplicas en hosts físicos independientes para que el servidor de API de Kubernetes, etcd, el planificador y el gestor de controladores puedan protegerse frente a una interrupción como, por ejemplo, la actualización del clúster. Si el clúster está en una zona con capacidad multizona, el maestro de alta disponibilidad se distribuye también en zonas para ayudar a protegerse frente a un fallo zonal.<br>Para ver las acciones que debe realizar, consulte
[Actualización a maestros de clúster de alta disponibilidad](/docs/containers?topic=containers-cs_versions#ha-masters). Estas acciones preparatorias se aplican:<ul>
<li>Si tiene un cortafuegos o políticas de red Calico personalizadas.</li>
<li>Si utiliza los puertos de host `2040` o `2041` en sus nodos trabajadores.</li>
<li>Si ha utilizado la dirección IP del maestro del clúster para el acceso al maestro dentro del clúster.</li>
<li>Si tiene una automatización que llama a la API o a la CLI de Calico (`calicoctl`), como para crear políticas de Calico.</li>
<li>Si utiliza políticas de red de Kubernetes o Calico para controlar el acceso de salida de pod al maestro.</li></ul></td>
</tr>
</tbody></table>

## Temas populares en octubre de 2018
{: #oct18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en octubre de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>25 de octubre</td>
<td>[Zona disponible en Milán](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Damos la bienvenida a Milán, Italia, como nueva zona para clústeres de pago en la región UE central. Antes Milán solo estaba disponible para clústeres gratuitos. Si tiene un cortafuegos, [abra los puertos del cortafuegos](/docs/containers?topic=containers-firewall#firewall) para esta zona y para las otras dentro de la región en la que se encuentra el clúster.</td>
</tr>
<tr>
<td>22 de octubre</td>
<td>[Nueva ubicación multizona en Londres, `lon05`](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>En la ubicación metropolitana de Londres multizona se ha sustituido la zona `lon02` por la nueva zona `lon05`, una zona con más recursos de infraestructura que `lon02`. Cree nuevos clústeres multizona con `lon05`. Los clústeres existentes con `lon02` reciben soporte, pero los nuevos clústeres multizona deben utilizar `lon05` en su lugar.</td>
</tr>
<tr>
<td>5 de octubre</td>
<td>Integración con {{site.data.keyword.keymanagementservicefull}} (beta)</td>
<td>Puede cifrar secretos de Kubernetes en el clúster mediante la [habilitación de {{site.data.keyword.keymanagementserviceshort}} (beta)](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
<tr>
<td>4 de octubre</td>
<td>[{{site.data.keyword.registrylong}} se ha integrado con {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)](/docs/services/Registry?topic=registry-iam#iam)</td>
<td>Puede utilizar {{site.data.keyword.Bluemix_notm}} IAM para controlar el acceso a los recursos de registro, como la extracción, el envío y la creación de imágenes. Cuando se crea un clúster, también se crea una señal de registro para que el clúster pueda trabajar con el registro. Por lo tanto, necesita el rol de gestión de la plataforma de **Administrador** de registro a nivel de cuenta para crear un clúster. Para habilitar {{site.data.keyword.Bluemix_notm}} IAM para su cuenta de registro, consulte [Habilitación de la imposición de políticas para los usuarios existentes](/docs/services/Registry?topic=registry-user#existing_users).</td>
</tr>
<tr>
<td>1 de octubre</td>
<td>[Grupos de recursos](/docs/containers?topic=containers-users#resource_groups)</td>
<td>Puede utilizar grupos de recursos para separar sus recursos de {{site.data.keyword.Bluemix_notm}} en conductos, departamentos u otras agrupaciones para ayudar a asignar el acceso y a calibrar el uso. Ahora, {{site.data.keyword.containerlong_notm}} da soporte a la creación de clústeres en el grupo `default` o en cualquier otro grupo de recursos que cree.</td>
</tr>
</tbody></table>

## Temas populares en septiembre de 2018
{: #sept18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en septiembre de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>25 de septiembre</td>
<td>[Nuevas zonas disponibles](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Ahora dispone de aún más opciones en las que desplegar sus apps.
<ul><li>Damos la bienvenida a San José como dos nuevas zonas en la región de EE. UU. sur, `sjc03` y `sjc04`. Si tiene un cortafuegos, [abra los puertos del cortafuegos](/docs/containers?topic=containers-firewall#firewall) para esta zona y para las otras dentro de la región en la que se encuentra el clúster.</li>
<li>Con las dos nuevas zonas `tok04` y `tok05`, ahora puede [crear clústeres multizona](/docs/containers?topic=containers-plan_clusters#multizone) en Tokio en la región AP norte.</li></ul></td>
</tr>
<tr>
<td>05 de septiembre</td>
<td>[Zona disponible en Oslo](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Damos la bienvenida a Oslo, Noruega, como nueva zona en la región UE central. Si tiene un cortafuegos, [abra los puertos del cortafuegos](/docs/containers?topic=containers-firewall#firewall) para esta zona y para las otras dentro de la región en la que se encuentra el clúster.</td>
</tr>
</tbody></table>

## Temas populares en agosto de 2018
{: #aug18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en agosto de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>31 de agosto</td>
<td>{{site.data.keyword.cos_full_notm}} ahora está integrado con {{site.data.keyword.containerlong}}</td>
<td>Utilice las reclamaciones de volumen persistente (PVC) nativas de Kubernetes para suministrar {{site.data.keyword.cos_full_notm}} en el clúster. {{site.data.keyword.cos_full_notm}} se utiliza mejor para cargas de trabajo de lectura intensiva y si desea almacenar datos en varias zonas de un clúster multizona. Empiece por [crear una instancia de servicio de {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#create_cos_service) e [instalar el plugin de {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#install_cos) en el clúster. </br></br>¿No está seguro de qué solución de almacenamiento podría ser la correcta para usted? Empiece [aquí](/docs/containers?topic=containers-storage_planning#choose_storage_solution) a analizar los datos y elija la solución de almacenamiento adecuada para los datos. </td>
</tr>
<tr>
<td>14 de agosto</td>
<td>Actualice los clústeres a las versiones 1.11 de Kubernetes para asignar la prioridad de pod</td>
<td>Después de actualizar el clúster a la [versión 1.11 de Kubernetes](/docs/containers?topic=containers-cs_versions#cs_v111), puede aprovechar las nuevas funciones, como el aumento del rendimiento del tiempo de ejecución de contenedor con `containerd` o la [asignación de prioridad de pod](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
</tbody></table>

## Temas populares en julio de 2018
{: #july18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en julio de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>30 de julio</td>
<td>[Traiga su propio controlador de Ingress](/docs/containers?topic=containers-ingress#user_managed)</td>
<td>¿Tiene requisitos de seguridad específicos u otros requisitos personalizados para el controlador de Ingress del clúster? Si es así, es posible que desee ejecutar su propio controlador de Ingress en lugar de utilizar el controlador predeterminado.</td>
</tr>
<tr>
<td>10 de julio</td>
<td>Incorporación de clústeres multizona</td>
<td>¿Desea mejorar la disponibilidad del clúster? Ahora puede distribuir el clúster en varias zonas en determinadas áreas metropolitanas. Para obtener más información, consulte [Creación de clústeres multizona en {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-plan_clusters#multizone).</td>
</tr>
</tbody></table>

## Temas populares en junio de 2018
{: #june18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en junio de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>13 de junio</td>
<td>El nombre del mandato de CLI `bx` se ha cambiado por la CLI `ic`</td>
<td>Cuando descargue la versión más reciente de la CLI de {{site.data.keyword.Bluemix_notm}}, ahora ejecute mandatos utilizando el prefijo `ic` en lugar de `bx`. Por ejemplo, para obtener una lista de clústeres, ejecute `ibmcloud ks clusters`.</td>
</tr>
<tr>
<td>12 de junio</td>
<td>[Políticas de seguridad de pod](/docs/containers?topic=containers-psp)</td>
<td>Para los clústeres que ejecutan Kubernetes 1.10.3 o posterior, puede configurar políticas de seguridad de pod para definir quién puede crear y actualizar pods en {{site.data.keyword.containerlong_notm}}.</td>
</tr>
<tr>
<td>6 de junio</td>
<td>[Soporte de TLS para el subdominio comodín de Ingress suministrado por IBM](/docs/containers?topic=containers-ingress#wildcard_tls)</td>
<td>Para los clústeres creados a partir del 6 de junio de 2018, el certificado TLS del subdominio de Ingress proporcionado por IBM ahora es un certificado comodín y se puede utilizar para el subdominio comodín registrado. Para los clústeres creados antes del 6 de junio de 2018, el certificado TLS se actualizará a un certificado comodín cuando se renueve el certificado TLS actual.</td>
</tr>
</tbody></table>

## Temas populares en mayo de 2018
{: #may18}


<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en mayo de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>24 de mayo</td>
<td>[Nuevo formato subdominio de Ingress](/docs/containers?topic=containers-ingress)</td>
<td>Los clústeres creados después del 24 de mayo son asignados a un subdominio de Ingress en el nuevo formato: <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>. Cuando se utiliza Ingress para exponer sus apps, puede utilizar el nuevo subdominio para acceder a las apps desde Internet.</td>
</tr>
<tr>
<td>14 de mayo</td>
<td>[Actualización: Despliegue en todo el mundo de cargas de trabajo en máquinas nativas con GPU](/docs/containers?topic=containers-app#gpu_app)</td>
<td>Si tiene un [tipo de máquina con GPU (Graphics Processing Unit) nativa](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node) en el clúster, puede planificar apps matemáticas intensivas. El nodo trabajador con GPU puede procesar la carga de trabajo de la app tanto con la CPU como con la GPU para incrementar el rendimiento.</td>
</tr>
<tr>
<td>3 de mayo</td>
<td>[Container Image Security Enforcement (beta)](/docs/services/Registry?topic=registry-security_enforce#security_enforce)</td>
<td>¿Su equipo necesita un poco más ayuda para saber qué imagen tiene que extraer en sus contenedores de app? Pruebe Container Image Security Enforcement beta para verificar las imágenes de contenedor antes de desplegarlas. Disponible para clústeres que ejecutan Kubernetes 1.9 o posterior.</td>
</tr>
<tr>
<td>01 de mayo</td>
<td>[Despliegue del panel de control de Kubernetes desde la consola](/docs/containers?topic=containers-app#cli_dashboard)</td>
<td>¿Alguna vez ha deseado acceder al panel de control de Kubernetes con una sola pulsación? Pruebe el botón **Panel de control de Kubernetes** en la consola de {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
</tbody></table>




## Temas populares en abril de 2018
{: #apr18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en abril de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>17 de abril</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>Instale el [plugin](/docs/containers?topic=containers-block_storage#install_block) {{site.data.keyword.Bluemix_notm}} Block Storage para guardar datos persistentes en almacenamiento en bloque. A continuación, puede [crear nuevo almacenamiento en bloque](/docs/containers?topic=containers-block_storage#add_block) o [utilizar almacenamiento en bloque existente](/docs/containers?topic=containers-block_storage#existing_block) para su clúster.</td>
</tr>
<tr>
<td>13 de abril</td>
<td>[Nueva guía de aprendizaje para migrar apps de Cloud Foundry a clústeres](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)</td>
<td>¿Tiene una app de Cloud Foundry? Aprenda a desplegar el mismo código de la app en un contenedor que se ejecute en un clúster de Kubernetes.</td>
</tr>
<tr>
<td>5 de abril</td>
<td>[Filtrado de registros](/docs/containers?topic=containers-health#filter-logs)</td>
<td>Filtre registros específicos al reenviarlos. Los registros se pueden filtrar por una serie de mensaje, un nivel de registro, un nombre de contenedor o un espacio de nombres específico.</td>
</tr>
</tbody></table>

## Temas populares en marzo de 2018
{: #mar18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en marzo de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>16 de marzo</td>
<td>[Suministre un clúster nativo con Trusted Compute](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)</td>
<td>Cree un clúster nativo que ejecute la [versión 1.9 de Kubernetes](/docs/containers?topic=containers-cs_versions#cs_v19) o posterior y habilitar Trusted Compute para verificar que los nodos trabajadores no se manipulan de forma indebida.</td>
</tr>
<tr>
<td>14 de marzo</td>
<td>[Inicio de sesión seguro con {{site.data.keyword.appid_full}}](/docs/containers?topic=containers-supported_integrations#appid)</td>
<td>Mejore las apps que se ejecutan en {{site.data.keyword.containerlong_notm}} requiriendo a los usuarios que inicien sesión.</td>
</tr>
<tr>
<td>13 de marzo</td>
<td>[Zona disponible en São Paulo](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Damos la bienvenida a São Paulo, Brasil, como nueva zona en la región EE. UU. sur. Si tiene un cortafuegos, [abra los puertos del cortafuegos](/docs/containers?topic=containers-firewall#firewall) para esta zona y para las otras dentro de la región en la que se encuentra el clúster.</td>
</tr>
</tbody></table>

## Temas populares en febrero de 2018
{: #feb18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en febrero de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>27 de febrero</td>
<td>Imágenes de máquina virtual de hardware (HVM) para nodos trabajadores</td>
<td>Aumente el rendimiento de E/S de las cargas de trabajo con imágenes de HVM. Active cada nodo trabajador existente con el [mandato](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) `ibmcloud ks worker-reload` o con el [mandato](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>26 de febrero</td>
<td>[Escalado automático de KubeDNS](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>KubeDNS ahora se amplía con el clúster a medida que crece. Puede ajustar los índices de escalado mediante el mandato siguiente: `kubectl -n kube-system edit cm kube-dns-autoscaler`.</td>
</tr>
<tr>
<td>23 de febrero</td>
<td>Vea la consola web para los [registros](/docs/containers?topic=containers-health#view_logs) y las [métricas](/docs/containers?topic=containers-health#view_metrics)</td>
<td>Vea fácilmente los datos de registros y métricas del clúster y sus componentes con una mejor interfaz de usuario web. Consulte la página de detalles de clúster para el acceso.</td>
</tr>
<tr>
<td>20 de febrero</td>
<td>Imágenes cifradas y [contenido firmado y de confianza](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)</td>
<td>En {{site.data.keyword.registryshort_notm}}, puede firmar y cifrar imágenes para garantizar su integridad al almacenar en el espacio de nombres del registro. Ejecute las instancias de contenedor utilizando únicamente contenido fiable.</td>
</tr>
<tr>
<td>19 de febrero</td>
<td>[Configure la VPN IPSec de strongSwan](/docs/containers?topic=containers-vpn#vpn-setup)</td>
<td>Despliegue rápidamente el diagrama de Helm de la VPN IPSec de strongSwan Helm para conectar el clúster de {{site.data.keyword.containerlong_notm}} de forma segura al centro de datos local sin un Virtual Router Appliance.</td>
</tr>
<tr>
<td>14 de febrero</td>
<td>[Zona disponible en Seúl](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Justo a tiempo para los Juegos Olímpicos, despliegue un clúster de Kubernetes en Seúl en la región AP norte. Si tiene un cortafuegos, [abra los puertos del cortafuegos](/docs/containers?topic=containers-firewall#firewall) para esta zona y para las otras dentro de la región en la que se encuentra el clúster.</td>
</tr>
<tr>
<td>8 de febrero</td>
<td>[Actualice Kubernetes 1.9](/docs/containers?topic=containers-cs_versions#cs_v19)</td>
<td>Revise los cambios que debe realizar en los clústeres antes de actualizar Kubernetes 1.9.</td>
</tr>
</tbody></table>

## Temas populares en enero de 2018
{: #jan18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en enero de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<td>25 de enero</td>
<td>[Registro global disponible](/docs/services/Registry?topic=registry-registry_overview#registry_regions)</td>
<td>Con {{site.data.keyword.registryshort_notm}}, puede utilizar `registry.bluemix.net` global para extraer imágenes públicas proporcionadas por IBM.</td>
</tr>
<tr>
<td>23 de enero</td>
<td>[Zonas disponibles en Singapur y Montreal, CA](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Singapur y Montreal son zonas disponibles en las regiones AP norte y EE. UU. este de {{site.data.keyword.containerlong_notm}}. Si tiene un cortafuegos, [abra los puertos del cortafuegos](/docs/containers?topic=containers-firewall#firewall) para estas zonas y para las otras dentro de la región en la que se encuentra el clúster.</td>
</tr>
<tr>
<td>8 de enero</td>
<td>[Disponibles tipos mejorados](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)</td>
<td>Los tipos de máquina virtual de la serie 2 incluyen cifrado de disco y almacenamiento SSD. [Traspase sus cargas de trabajo](/docs/containers?topic=containers-update#machine_type) a estos tipos para una mayor estabilidad y rendimiento.</td>
</tr>
</tbody></table>

## Chat con desarrolladores de Slack
{: #slack}

Vea de qué están hablando otros desarrolladores y realice sus propias preguntas en [Slack de {{site.data.keyword.containerlong_notm}}. ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-container-service.slack.com)
{:shortdesc}

Si no utiliza un ID de IBM para la cuenta de {{site.data.keyword.Bluemix_notm}}, [solicite una invitación](https://bxcs-slack-invite.mybluemix.net/) a este Slack.
{: tip}
