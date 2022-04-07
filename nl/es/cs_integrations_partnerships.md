---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, helm

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


# Socios del servicio Kubernetes de IBM Cloud
{: #service-partners}

IBM se dedica a convertir {{site.data.keyword.containerlong_notm}} en el mejor servicio de Kubernetes que le ayude a migrar, administrar y trabajar con cargas de trabajo de Kubernetes. Para proporcionarle todas las prestaciones que necesita para ejecutar cargas de trabajo de producción en la nube, {{site.data.keyword.containerlong_notm}} se asocia con otros proveedores de servicios de terceros para mejorar el clúster con las mejores herramientas de registro, supervisión y almacenamiento.
{: shortdesc}

Consulte nuestros asociados y las ventajas que aporta cada solución que proporcionan. Para ver otros servicios de {{site.data.keyword.Bluemix_notm}} de propiedad y servicios de código abierto de terceros que puede utilizar en su clúster, consulte el apartado [Visión general de las integraciones de {{site.data.keyword.Bluemix_notm}} y de terceros](/docs/containers?topic=containers-ibm-3rd-party-integrations).

## LogDNA
{: #logdna-partner}

{{site.data.keyword.la_full_notm}} proporciona [LogDNA ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://logdna.com/) como servicio de terceros que puede utilizar para agregar prestaciones de registro inteligente a su clúster y a sus apps.

### Ventajas
{: #logdna-benefits}

Consulte la tabla siguiente para ver una lista de las principales ventajas que puede obtener si utiliza LogDNA.
{: shortdesc}

|Ventaja|Descripción|
|-------------|------------------------------|
|Gestión centralizada de registros y análisis de registros|Cuando configure el clúster como un origen de registro, LogDNA inicia automáticamente la recopilación de información de registro para nodos trabajadores, pods, apps y red. Los registros se analizan automáticamente, se indexan, se etiquetan y se agregan mediante LogDNA y se visualizan en el panel de control de LogDNA para que pueda ver fácilmente información sobre los recursos del clúster. Puede utilizar la herramienta de gráfica incorporada para visualizar la mayoría de los códigos de error o las entradas de registro más comunes. |
|Búsqueda fácil con la sintaxis parecida a la de Google|LogDNA utiliza una sintaxis de búsqueda parecida a la de Google que da soporte a términos estándares y a operaciones `AND` y `OR` y que le permite excluir o combinar términos de búsqueda para ayudarle a localizar sus registros con más facilidad. Con el indexado inteligente de registros, puede ir a una entrada de registro específica en cualquier momento. |
|Cifrado en tránsito y en reposo|LogDNA cifra automáticamente los registros para protegerlos durante el tránsito y en reposo. |
|Alertas y vistas de registro personalizadas|Puede utilizar el panel de control para encontrar los registros que coinciden con los criterios de búsqueda, guardar estos registros en una vista y compartir esta vista con otros usuarios para simplificar la depuración entre los miembros del equipo. También puede utilizar esta vista para crear una alerta que puede enviar a otros sistemas como PagerDuty, Slack o correo electrónico.   |
|Paneles de control listos para ser utilizados y personalizados|Puede elegir entre una variedad de paneles de control existentes o puede crear su propio panel de control para visualizar registros de la forma que necesite. |

### Integración con {{site.data.keyword.containerlong_notm}}
{: #logdna-integration}

LogDNA se proporciona mediante {{site.data.keyword.la_full_notm}}, un servicio de plataforma de {{site.data.keyword.Bluemix_notm}} que puede utilizar con el clúster. {{site.data.keyword.la_full_notm}} recibe soporte de LogDNA en asociación con IBM.
{: shortdesc}

Para utilizar LogDNA en el clúster, debe suministrar una instancia de {{site.data.keyword.la_full_notm}} en su cuenta de {{site.data.keyword.Bluemix_notm}} y configurar los clústeres de Kubernetes como un origen de registro. Después de configurar el clúster, los registros se recopilan y se reenvían automáticamente a la instancia de servicio de {{site.data.keyword.la_full_notm}}. Puede utilizar el panel de control de {{site.data.keyword.la_full_notm}} para acceder a los registros.   

Para obtener más información, consulte [Gestión de registros de clúster de Kubernetes con {{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube).

### Facturación y soporte
{: #logdna-billing-support}

{{site.data.keyword.la_full_notm}} está totalmente integrado en el sistema de soporte de {{site.data.keyword.Bluemix_notm}}. Si tiene un problema al utilizar {{site.data.keyword.la_full_notm}}, publique una pregunta en el canal `logdna-on-iks` en [Slack de {{site.data.keyword.containerlong_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-container-service.slack.com/) o abra un [caso de soporte de {{site.data.keyword.Bluemix_notm}}](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support). Inicie la sesión en Slack con su IBMid. Si no utiliza un IBMid para la cuenta de {{site.data.keyword.Bluemix_notm}}, [solicite una invitación a este Slack ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://bxcs-slack-invite.mybluemix.net/).

## Sysdig
{: #sydig-partner}

{{site.data.keyword.mon_full_notm}} proporciona [Sysdig Monitor ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://sysdig.com/products/monitor/) como sistema de análisis de contenedores nativo de la nube de terceros que puede utilizar para ver información detallada sobre el rendimiento y el estado de sus hosts de cálculo, apps, contenedores y redes.
{: shortdesc}

### Ventajas
{: #sydig-benefits}

Consulte la tabla siguiente para ver una lista de las principales ventajas que puede obtener si utiliza Sysdig.
{: shortdesc}

|Ventaja|Descripción|
|-------------|------------------------------|
|Acceso automático a métricas nativas de la nube y personalizadas de Prometheus|Elija entre una variedad de métricas predefinidas nativas de la nube y personalizadas de Prometheus para ver información sobre el rendimiento y el estado de sus hosts de cálculo, apps, contenedores y redes.|
|Resolución de problemas con filtros avanzados|Sysdig Monitor crea topologías de red que muestran cómo se conectan los nodos trabajadores y cómo se comunican entre sí los servicios de Kubernetes. Puede ir desde nodos trabajadores a contenedores y llamadas de sistema individuales, al tiempo que agrupa y consulta métricas importantes para cada recurso. Por ejemplo, utilice estas métricas para localizar los servicios que reciben la mayoría de las solicitudes, o los servicios con consultas y tiempos de respuesta lentos. Puede combinar estos datos con sucesos de Kubernetes, sucesos de CI/CD personalizados o confirmaciones de código.
|Detección automática de anomalías y alertas personalizadas|Defina reglas y umbrales para cuando desee que se le notifique para detectar anomalías en el clúster o en los recursos de grupo para permitir que Sysdig le notifique cuando un recurso actúe de forma distinta que el resto. Puede enviar estas alertas a otras herramientas, como por ejemplo ServiceNow, PagerDuty, Slack, VictorOps o correo electrónico.|
|Paneles de control listos para ser utilizados y personalizados|Puede elegir entre una variedad de paneles de control existentes o puede crear su propio panel de control para visualizar las métricas de los microservicios de la forma que necesite. |
{: caption="Ventajas de utilizar Sysdig Monitor" caption-side="top"}

### Integración con {{site.data.keyword.containerlong_notm}}
{: #sysdig-integration}

Sysdig Monitor se proporciona mediante {{site.data.keyword.mon_full_notm}}, un servicio de plataforma de {{site.data.keyword.Bluemix_notm}} que puede utilizar con el clúster. {{site.data.keyword.mon_full_notm}} recibe soporte de Sysdig en asociación con IBM.
{: shortdesc}

Para utilizar Sysdig Monitor en el clúster, debe suministrar una instancia de {{site.data.keyword.mon_full_notm}} en su cuenta de {{site.data.keyword.Bluemix_notm}} y configurar los clústeres de Kubernetes como un origen de métricas. Después de configurar el clúster, las métricas se recopilan y se reenvían automáticamente a la instancia de servicio de {{site.data.keyword.mon_full_notm}}. Puede utilizar el panel de control de {{site.data.keyword.mon_full_notm}} para acceder a las métricas.   

Para obtener más información, consulte
[Análisis de métricas para una app desplegada en un clúster de Kubernetes](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster).

### Facturación y soporte
{: #sysdig-billing-support}

Puesto que {{site.data.keyword.mon_full_notm}} proporciona Sysdig Monitor, el uso se incluye en la factura de {{site.data.keyword.Bluemix_notm}} correspondiente a los servicios de la plataforma. Para obtener información sobre los precios, revise los planes disponibles en el [catálogo de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/observe/monitoring/create).

{{site.data.keyword.mon_full_notm}} está totalmente integrado en el sistema de soporte de {{site.data.keyword.Bluemix_notm}}. Si tiene un problema al utilizar {{site.data.keyword.mon_full_notm}}, publique una pregunta en el canal `sysdig-monitoring` en [Slack de {{site.data.keyword.containerlong_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-container-service.slack.com/) o abra un [caso de soporte de {{site.data.keyword.Bluemix_notm}}](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support). Inicie la sesión en Slack con su IBMid. Si no utiliza un IBMid para la cuenta de {{site.data.keyword.Bluemix_notm}}, [solicite una invitación a este Slack ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://bxcs-slack-invite.mybluemix.net/).

## Portworx
{: #portworx-parter}

[Portworx ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://portworx.com/products/introduction/) es una solución de almacenamiento definida por software de alta disponibilidad que puede utilizar para gestionar el almacenamiento persistente local para bases de datos contenerizadas y otras apps con estado, o para compartir datos entre pods de varias zonas.
{: shortdesc}

**¿Qué es el almacenamiento definido por software (SDS)?** </br>
Una solución SDS abarca dispositivos de almacenamiento de varios tipos y tamaños o de distintos proveedores que están conectados a los nodos trabajadores del clúster. Los nodos trabajadores con almacenamiento disponible en discos duros se añaden como un nodo a un clúster de almacenamiento. En este clúster, el almacenamiento físico se virtualiza y se presenta al usuario como una agrupación de almacenamiento virtual. El clúster de almacenamiento está gestionado por el software SDS. Si se deben almacenar datos en el clúster de almacenamiento, el software SDS decide dónde almacenar los datos para obtener la máxima disponibilidad. El almacenamiento virtual se suministra con un conjunto común de funciones y servicios que puede aprovechar sin preocuparse de la arquitectura de almacenamiento subyacente.

### Ventajas
{: #portworx-benefits}

Consulte la tabla siguiente para ver una lista de las principales ventajas que puede obtener si utiliza Portworx.
{: shortdesc}

|Ventaja|Descripción|
|-------------|------------------------------|
|Almacenamiento nativo de la nube y gestión de datos para apps con estado|Portworx agrega el almacenamiento local disponible que está conectado a los nodos trabajadores y que puede variar en cuanto a tamaño o tipo y crea una capa de almacenamiento persistente unificada para bases de datos contenerizadas u otras apps con estado que desee ejecutar en el clúster. Mediante las reclamaciones de volumen persistente (PVC) de Kubernetes, puede añadir almacenamiento persistente local a las apps para almacenar los datos.|
|Datos de alta disponibilidad con réplica de volúmenes|Portworx realiza automáticamente réplicas de los datos en los volúmenes de los nodos trabajadores y las zonas del clúster para que se pueda acceder a los datos en todo momento y se pueda replanificar la app con estado en otro nodo trabajador en el caso de que se produzca un error o se rearranque un nodo trabajador. |
|Soporte para ejecutar `hyper-converged`|Portworx se puede configurar de modo que ejecute [`hyper-converged` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/hyperconvergence/) para garantizar que los recursos de cálculo y el almacenamiento siempre se colocan en el mismo nodo trabajador. Cuando se debe volver a planificar la app, Portworx mueve la app a un nodo trabajador en el que reside una de las réplicas de volumen para garantizar la velocidad de acceso al disco local y un alto rendimiento para la app con estado. |
|Cifrado de datos con {{site.data.keyword.keymanagementservicelong_notm}}|Puede [configurar claves de cifrado de {{site.data.keyword.keymanagementservicelong_notm}}](/docs/containers?topic=containers-portworx#encrypt_volumes) protegidas por módulos de seguridad de hardware (HSM) basados en la nube con la certificación FIPS 140-2 Nivel 2 para proteger los datos en los volúmenes. Puede elegir entre utilizar una clave de cifrado para cifrar todos los volúmenes de un clúster o utilizar una clave de cifrado para cada volumen. Portworx utiliza esta clave para cifrar los datos en reposo y en tránsito cuando se envían datos a otro nodo trabajador.|
|Instantáneas incorporadas y copias de seguridad de la nube|Puede guardar el estado actual de un volumen y sus datos creando una [instantánea de Portworx ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/create-snapshots/). Las instantáneas se pueden almacenar en el clúster local de Portworx o en la nube.|
|Supervisión integrada con Lighthouse|[Lighthouse ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.portworx.com/reference/lighthouse/) es una herramienta gráfica e intuitiva que le ayuda a gestionar y a supervisar los clústeres y las instantáneas de volúmenes de Portworx. Con Lighthouse, puede ver el estado del clúster de Portworx, incluidos el número de nodos de almacenamiento disponibles y los volúmenes y la capacidad disponibles, y analizar los datos en Prometheus, Grafana o Kibana.|
{: caption="Ventajas de utilizar Portworx" caption-side="top"}

### Integración con {{site.data.keyword.containerlong_notm}}
{: #portworx-integration}

{{site.data.keyword.containerlong_notm}} proporciona tipos de nodos trabajadores optimizados para el uso de SDS y que vienen con uno o varios discos locales sin formato y sin montar que puede utilizar para almacenar los datos. Portworx ofrece el mejor rendimiento cuando se utilizan [máquinas de nodo trabajador SDS](/docs/containers?topic=containers-planning_worker_nodes#sds) con una velocidad de red de 10 Gbps. Sin embargo, puede instalar Portworx en tipos de nodos trabajadores que no sean SDS, pero es posible que no obtenga las ventajas de rendimiento que requiere la app.
{: shortdesc}

Portworx se instala mediante un [diagrama de Helm](/docs/containers?topic=containers-portworx#install_portworx). Cuando se instala el diagrama de Helm, Portworx analiza automáticamente el almacenamiento persistente local que está disponible en el clúster y añade el almacenamiento a la capa de almacenamiento de Portworx. Para añadir almacenamiento desde la capa de almacenamiento de Portworx a las apps, debe utilizar [reclamaciones de volúmenes persistentes de Kubernetes](/docs/containers?topic=containers-portworx#add_portworx_storage).

Para instalar Portworx en el clúster, debe disponer de una licencia de Portworx. Si es un nuevo usuario, puede utilizar la edición `px-enterprise` como versión de prueba. La versión de prueba le proporciona toda la funcionalidad de Portworx, que puede probar durante 30 días. Después de que caduque la versión de prueba, debe [adquirir una licencia de Portworx ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/containers?topic=containers-portworx#portworx_license) para seguir utilizando el clúster Portworx.

Para obtener más información sobre cómo instalar y utilizar Portworx con {{site.data.keyword.containerlong_notm}}, consulte [Almacenamiento de datos en el almacenamiento definido por software (SDS) con Portworx](/docs/containers?topic=containers-portworx).

### Facturación y soporte
{: #portworx-billing-support}

Las máquinas de nodo trabajador de SDS que vienen con discos locales y las máquinas virtuales que se utilizan para Portworx están incluidas en la factura mensual de {{site.data.keyword.containerlong_notm}}. Para obtener información sobre precios, consulte
el [catálogo de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/kubernetes/catalog/cluster). La licencia de Portworx tiene un coste aparte y no se incluye en su factura mensual.
{: shortdesc}

Si tiene un problema con el uso de Portworx o desea conversar sobre las configuraciones de Portworx para el caso de uso específico, publique una pregunta en el canal `portworx-on-iks` en el [Slack de {{site.data.keyword.containerlong_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-container-service.slack.com/). Inicie la sesión en Slack con su IBMid. Si no utiliza un IBMid para la cuenta de {{site.data.keyword.Bluemix_notm}}, [solicite una invitación a este Slack ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://bxcs-slack-invite.mybluemix.net/).
