---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

keywords: kubernetes, iks, compliance, security standards

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
{:download: .download}
{:preview: .preview}
{:faq: data-hd-content-type='faq'}


# Preguntas más frecuentes
{: #faqs}

## ¿Qué es Kubernetes?
{: #kubernetes}
{: faq}

Kubernetes es una plataforma de código abierto para gestionar cargas de trabajo y servicios contenerizados en múltiples hosts, y ofrece herramientas de gestión para desplegar, automatizar, supervisar y escalar apps contenerizadas con una intervención manual mínima o inexistente. Todos los contenedores que componen el microservicio se agrupan en pods, una unidad lógica para garantizar una gestión y un descubrimiento sencillos. Estos pods se ejecutan en hosts de cálculo que se gestionan en un clúster de Kubernetes portable, ampliable y con resolución automática de problemas en caso de que se produzcan errores.
{: shortdesc}

Para obtener más información sobre Kubernetes, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/home/?path=users&persona=app-developer&level=foundational).

## ¿Cómo funciona el Servicio Kubernetes de IBM Cloud?
{: #kubernetes_service}
{: faq}

Con {{site.data.keyword.containerlong_notm}}, puede crear su propio clúster de Kubernetes para desplegar y gestionar apps contenerizadas en {{site.data.keyword.Bluemix_notm}}. Las apps contenerizadas se alojan en hosts de cálculo de la infraestructura de IBM Cloud (SoftLayer) que se denominan nodos trabajadores. Puede elegir suministrar los hosts de cálculo como [máquinas virtuales](/docs/containers?topic=containers-planning_worker_nodes#vm) con recursos compartidos o dedicados, o como [máquinas nativas](/docs/containers?topic=containers-planning_worker_nodes#bm) que se puede optimizar para el uso de GPU y almacenamiento definido por software (SDS). Los nodos trabajadores se controlan mediante un maestro de Kubernetes altamente disponible que configura, supervisa y gestiona IBM. Puede utilizar la CLI o la API de {{site.data.keyword.containerlong_notm}} para trabajar con los recursos de infraestructura del clúster y la CLI o la API de Kubernetes para gestionar los despliegues y servicios.

Para obtener más información sobre cómo se configuran los recursos del clúster, consulte la [Arquitectura del servicio](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#architecture). Para ver una lista de prestaciones y beneficios, consulte [Por qué {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_ov#cs_ov).

## ¿Por qué debo utilizar el Servicio Kubernetes de IBM Cloud?
{: #faq_benefits}
{: faq}

{{site.data.keyword.containerlong_notm}} es una oferta de Kubernetes gestionada que proporciona herramientas potentes, una experiencia de usuario intuitiva y seguridad incorporada para una entrega rápida de apps que puede enlazar con servicios en la nube relacionados con IBM Watson®, AI, IoT, DevOps, seguridad y análisis de datos. Como proveedor de Kubernetes certificado, {{site.data.keyword.containerlong_notm}} proporciona una planificación inteligente, resolución automática de problemas, escalado horizontal, descubrimiento de servicios y equilibrio de carga, despliegues y retrotracciones automatizados, gestión de secretos y configuraciones. El servicio tiene también prestaciones avanzadas en torno a la gestión simplificada del clúster, políticas de aislamiento y seguridad de contenedores, la posibilidad de diseñar su propio clúster, y herramientas operativas integradas para mantener la coherencia en el despliegue.

Para obtener una visión general detallada de las prestaciones y beneficios, consulte [Por qué {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_ov#cs_ov).

## ¿Se proporciona el servicio con nodos trabajadores y un maestro de Kubernetes gestionado?
{: #managed_master_worker}
{: faq}

Cada clúster de Kubernetes de {{site.data.keyword.containerlong_notm}} está controlado por un nodo maestro de Kubernetes gestionado por IBM en una cuenta de infraestructura de {{site.data.keyword.Bluemix_notm}} propiedad de IBM. El nodo maestro de Kubernetes, incluidos todos los componentes maestros y los recursos de cálculo, de red y de almacenamiento, reciben supervisión continua por parte de los ingenieros de fiabilidad del sitio (SRE) de IBM. Los SRE aplican los estándares de seguridad más recientes, detectan y solucionan actividades maliciosas y trabajan para garantizar la fiabilidad y la disponibilidad de {{site.data.keyword.containerlong_notm}}. IBM actualiza automáticamente los complementos, como Fluentd para registro, que se instalan automáticamente cuando se suministra el clúster. No obstante, puede optar por inhabilitar las actualizaciones automáticas de algunos complementos y actualizarlos manualmente de forma independiente de los nodos maestro y trabajador. Para obtener más información, consulte [Actualización de complementos de clúster](/docs/containers?topic=containers-update#addons).

Periódicamente, Kubernetes publica [actualizaciones de parche, mayores o menores](/docs/containers?topic=containers-cs_versions#version_types). Estas actualizaciones pueden afectar a la versión del servidor de API de Kubernetes o a otros componentes del maestro de Kubernetes. IBM actualiza automáticamente la versión del parche, pero usted debe actualizar las versiones principales y secundarias del nodo maestro. Para obtener más información, consulte [Actualización del maestro de Kubernetes](/docs/containers?topic=containers-update#master).

Los nodos trabajadores de los clústeres estándares se suministran en su cuenta de infraestructura de {{site.data.keyword.Bluemix_notm}}. Los nodos trabajadores están dedicados a su cuenta y usted es el responsable de solicitar actualizaciones puntuales para los nodos trabajadores para asegurarse de que el sistema operativo y los componentes de {{site.data.keyword.containerlong_notm}} aplican las últimas actualizaciones de seguridad y parches. Los ingenieros de fiabilidad del sitio (SRE) de IBM, que supervisan de forma continua la imagen de Linux instalada en los nodos trabajadores para detectar vulnerabilidades y problemas de conformidad de seguridad, ponen a su disposición actualizaciones y parches. Para obtener más información, consulte [Actualización de nodos trabajadores](/docs/containers?topic=containers-update#worker_node).

## ¿Están los nodos trabajadores y maestro de Kubernetes altamente disponibles?
{: #faq_ha}
{: faq}

La arquitectura y la infraestructura de {{site.data.keyword.containerlong_notm}} están diseñadas para garantizar la fiabilidad, la baja latencia de procesamiento y el tiempo máximo de actividad del servicio. De forma predeterminada, cada clúster de {{site.data.keyword.containerlong_notm}} está configurado con varias instancias del maestro de Kubernetes para garantizar la disponibilidad y accesibilidad de los recursos del clúster, incluso si una o más instancias del maestro de Kubernetes no están disponibles.

Puede hacer que el clúster esté una disponibilidad incluso mayor y proteger la app del tiempo de inactividad mediante la dispersión de las cargas de trabajo en varios nodos trabajadores de varias zonas de una región. Esta configuración se denomina [clúster multizona](/docs/containers?topic=containers-ha_clusters#multizone) y garantiza que la app sea accesible, incluso aunque un nodo trabajador o una zona completa no esté disponible.

Como protección frente a un fallo en toda la región, cree [varios clústeres y distribúyalos en regiones de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ha_clusters#multiple_clusters). Mediante la configuración de un equilibrador de carga de red (NLB) para los clústeres, puede lograr un equilibrio de carga entre regiones y redes entre regiones para los clústeres.

Si tiene datos que deban estar disponibles, incluso si se produce una interrupción, asegúrese de almacenar los datos en [almacenamiento persistente](/docs/containers?topic=containers-storage_planning#storage_planning).

Para obtener más información sobre cómo conseguir una alta disponibilidad para el clúster, consulte [Alta disponibilidad para {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ha#ha).

## ¿Qué opciones tengo para proteger mi clúster?
{: #secure_cluster}
{: faq}

Puede utilizar características de seguridad incorporadas en {{site.data.keyword.containerlong_notm}} para proteger los componentes del clúster, los datos y los despliegues de apps para garantizar la integridad de los datos y la conformidad de seguridad. Utilice estas características para proteger el servidor de la API de Kubernetes, el almacén de datos etcd, el nodo trabajador, la red, el almacenamiento, las imágenes y los despliegues frente a ataques maliciosos. También puede hacer uso de las herramientas incorporadas de registro y supervisión para detectar ataques maliciosos y patrones de uso sospechosos.

Para obtener más información sobre los componentes del clúster y cómo puede proteger cada componente, consulte [Seguridad para {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-security#security).

## ¿Qué políticas de acceso debo dar a los usuarios de mi clúster?
{: #faq_access}
{: faq}

{{site.data.keyword.containerlong_notm}} utiliza {{site.data.keyword.iamshort}} (IAM) para otorgar acceso a recursos del clúster a través de roles de plataforma de IAM y políticas de control de acceso basadas en rol (RBAC) de Kubernetes a través de los roles de servicio de IAM. Para obtener más información sobre los tipos de políticas de acceso, consulte [Cómo elegir la política de acceso y el rol correctos para los usuarios](/docs/containers?topic=containers-users#access_roles).
{: shortdesc}

Las políticas de acceso que asigna a los usuarios varían en función de lo que desee que los usuarios puedan realizar. Encontrará más información sobre qué roles autorizan los tipos de acciones en la [página de referencia de acceso de usuario](/docs/containers?topic=containers-access_reference) o en los enlaces de la tabla siguiente. Para ver los pasos a seguir para asignar políticas, consulte [Cómo otorgar a los usuarios acceso al clúster a través de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform).

| Caso de uso | Roles y ámbito de ejemplo |
| --- | --- |
| Auditor de apps | [Rol de plataforma de Visor sobre un clúster, una región o un grupo de recursos](/docs/containers?topic=containers-access_reference#view-actions), [Rol de servicio de Lector sobre un clúster, una región o un grupo de recursos](/docs/containers?topic=containers-access_reference#service). |
| Desarrolladores de apps | [Rol de plataforma de Editor sobre un clúster](/docs/containers?topic=containers-access_reference#editor-actions), [Rol de servicio de Escritor limitado a un espacio de nombres](/docs/containers?topic=containers-access_reference#service), [Rol de espacio de desarrollador de Cloud Foundry](/docs/containers?topic=containers-access_reference#cloud-foundry). |
| Facturación | [Rol de plataforma de Visor sobre un clúster, una región o un grupo de recursos](/docs/containers?topic=containers-access_reference#view-actions). |
| Creación de un clúster | Permisos a nivel de cuenta para credenciales de infraestructura de superusuario, rol de plataforma de Administrador sobre {{site.data.keyword.containerlong_notm}}, rol de plataforma de Administrador sobre {{site.data.keyword.registrylong_notm}}. Para obtener más información, consulte [Preparación para crear clústeres](/docs/containers?topic=containers-clusters#cluster_prepare).|
| Administrador del clúster | [Rol de plataforma de Administrador sobre un clúster](/docs/containers?topic=containers-access_reference#admin-actions), [Rol de servicio de Gestor, no limitado a un espacio de nombres (para todo el clúster)](/docs/containers?topic=containers-access_reference#service).|
| Operador de DevOps | [Rol de plataforma de Operador sobre un clúster](/docs/containers?topic=containers-access_reference#operator-actions), [Rol de servicio de Escritor no limitado a un espacio de nombres (para todo el clúster)](/docs/containers?topic=containers-access_reference#service), [Rol de espacio de desarrollador de Cloud Foundry](/docs/containers?topic=containers-access_reference#cloud-foundry).  |
| Operador o ingeniero de fiabilidad del sitio | [Rol de plataforma de Administrador sobre un clúster, una región o un grupo de recursos](/docs/containers?topic=containers-access_reference#admin-actions), [Rol de servicio de Lector sobre un clúster o una región](/docs/containers?topic=containers-access_reference#service) o [Rol de servicio de Gestor sobre todos los espacios de nombres del clúster](/docs/containers?topic=containers-access_reference#service) para poder utilizar mandatos `kubectl top nodes,pods`. |
{: caption="Los tipos de roles que puede asignar para que cumplan distintos casos de uso." caption-side="top"}

## ¿Dónde puedo encontrar una lista de boletines de seguridad que afectan a mi clúster?
{: #faq_security_bulletins}
{: faq}

Si se encuentran vulnerabilidades en Kubernetes, Kubernetes publica CVE en boletines de seguridad para informar a los usuarios y describir las acciones que los usuarios deben tomar para remediar la vulnerabilidad. Los boletines de seguridad de Kubernetes que afectan a los usuarios de {{site.data.keyword.containerlong_notm}} o a la plataforma {{site.data.keyword.Bluemix_notm}} se publican en el [boletín de seguridad de {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/status?component=containers-kubernetes&selected=security).

Algunos CVE requieren la actualización de parches más reciente para una versión de Kubernetes que puede instalar como parte del [proceso de actualización de clúster](/docs/containers?topic=containers-update#update) normal en {{site.data.keyword.containerlong_notm}}. Asegúrese de aplicar los parches de seguridad a tiempo para proteger el clúster frente a ataques maliciosos. Para obtener información sobre lo que se incluye en un parche de seguridad, consulte el [registro de cambios de versión](/docs/containers?topic=containers-changelog#changelog).

## ¿Ofrece el servicio soporte para recursos nativos y GPU?
{: #bare_metal_gpu}
{: faq}

Sí, puede suministrar el nodo trabajador como un servidor nativo físico de un solo arrendatario. Los servidores nativos ofrecen grandes beneficios en cuanto a alto rendimiento para cargas de trabajo como datos, IA y GPU. Además, todos los recursos de hardware están dedicados a sus cargas de trabajo, así que no tiene que preocuparse por "vecinos molestos".

Para obtener más información sobre los tipos de recursos nativos disponibles y en qué difieren los recursos nativos de las máquinas virtuales, consulte [Máquinas físicas (nativas)](/docs/containers?topic=containers-planning_worker_nodes#bm).

## ¿Qué versiones de Kubernetes admite el servicio?
{: #supported_kube_versions}
{: faq}

{{site.data.keyword.containerlong_notm}} da soporte a varias versiones de Kubernetes simultáneamente. Cuando se publica una versión más reciente (n), se da soporte a hasta 2 versiones anteriores (n-2). Las versiones anteriores a 2 versiones anteriores a la versión más reciente (n-3) son las primeras que quedan en desuso y a las que se deja de dar soporte. Actualmente se admiten las versiones siguientes:

*   Más reciente: 1.14.2
*   Predeterminada: 1.13.6
*   Otras: 1.12.9

Para obtener más información sobre las versiones admitidas y las acciones de actualización que debe llevar a cabo para pasar de una versión a otra, consulte [Información de versión y acciones de actualización](/docs/containers?topic=containers-cs_versions#cs_versions).

## ¿Dónde está disponible el servicio?
{: #supported_regions}
{: faq}

{{site.data.keyword.containerlong_notm}} está disponible en todo el mundo. Puede crear clústeres estándar en cada región de {{site.data.keyword.containerlong_notm}} soportada. Los clústeres gratuitos solo están disponibles en determinadas regiones.

Para obtener más información sobre las regiones soportadas, consulte [Ubicaciones](/docs/containers?topic=containers-regions-and-zones#regions-and-zones).

## ¿A qué estándares se ajusta el servicio?
{: #standards}
{: faq}

{{site.data.keyword.containerlong_notm}} implementa controles correspondientes a los estándares siguientes:
- Certificación EU-US Privacy Shield y Certificación Swiss-US Privacy Shield
- Ley de Responsabilidad y Portabilidad de Seguro Médico (HIPAA)
- Estándares de Service Organization Control (SOC 1, SOC 2 Tipo 1)
- International Standard on Assurance Engagements 3402 (ISAE 3402), Informes de verificación sobre controles en organizaciones de servicios
- Organización Internacional de Estandarización (ISO 27001, ISO 27017, ISO 27018)
- Estándar de Seguridad de Datos para la Industria de Tarjetas de
Pago (PCI DSS)

## ¿Puedo utilizar IBM Cloud y otros servicios con mi clúster?
{: #faq_integrations}
{: faq}

Puede añadir servicios de infraestructura y de la plataforma {{site.data.keyword.Bluemix_notm}}, así como servicios de proveedores de terceros al clúster de {{site.data.keyword.containerlong_notm}} para habilitar la automatización, mejorar la seguridad o mejorar las posibilidades de supervisión y registro en el clúster.

Para obtener una lista de servicios admitidos, consulte [Integración de servicios](/docs/containers?topic=containers-supported_integrations#supported_integrations).

## ¿Puedo conectar mi clúster en IBM Cloud Public con apps que se ejecuten en mi centro de datos local?
{: #hybrid}
{: faq}

Puede conectar los servicios de {{site.data.keyword.Bluemix_notm}} público con su centro de datos local para crear su propia configuración de nube híbrida. Entre los ejemplos de cómo puede aprovechar {{site.data.keyword.Bluemix_notm}} público y privado con apps que se ejecutan en su centro de datos local, se incluyen:
- Puede crear un clúster con {{site.data.keyword.containerlong_notm}} en {{site.data.keyword.Bluemix_notm}} público, pero desea conectar el clúster con una base de datos local.
- Puede crear un clúster de Kubernetes en {{site.data.keyword.Bluemix_notm}} privado en su propio centro de datos y despliega apps en el clúster. No obstante, la app podría utilizar un servicio de {{site.data.keyword.ibmwatson_notm}}, como Tone Analyzer, en {{site.data.keyword.Bluemix_notm}} público.

Para habilitar la comunicación entre los servicios que se ejecutan en {{site.data.keyword.Bluemix_notm}} Público y los servicios que se ejecutan localmente, debe [configurar una conexión VPN](/docs/containers?topic=containers-vpn#vpn). Para conectar su entorno de {{site.data.keyword.Bluemix_notm}} público o dedicado con un entorno de {{site.data.keyword.Bluemix_notm}} privado, consulte [Utilización de {{site.data.keyword.containerlong_notm}} con {{site.data.keyword.Bluemix_notm}} privado](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp).

Para obtener una visión general de las ofertas de {{site.data.keyword.containerlong_notm}} admitidas, consulte [Comparación de ofertas y sus combinaciones](/docs/containers?topic=containers-cs_ov#differentiation).

## ¿Puedo desplegar el Servicio Kubernetes de IBM Cloud en mi propio centro de datos?
{: #private}
{: faq}

Si no desea mover sus apps a {{site.data.keyword.Bluemix_notm}} público, pero sigue queriendo aprovechar las características de {{site.data.keyword.containerlong_notm}}, puede instalar {{site.data.keyword.Bluemix_notm}} privado. {{site.data.keyword.Bluemix_notm}} privado es una plataforma de aplicaciones que se puede instalar de manera local en sus propias máquinas y que puede utilizar para desarrollar y gestionar apps contenerizadas locales en su propio entorno controlado detrás de un cortafuegos.

Para obtener más información, consulte la [documentación de producto de {{site.data.keyword.Bluemix_notm}} Privado ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html).

## ¿Qué se me facturará cuando utilice el servicio IBM Cloud Kubernetes?
{: #charges}
{: faq}

Con los clústeres de {{site.data.keyword.containerlong_notm}}, puede utilizar los recursos de cálculo, red y almacenamiento de la infraestructura IBM Cloud (SoftLayer) con servicios de plataforma como Watson AI o Componer Database-as-a-Service. Cada recurso puede implicar sus propios cargos, que pueden ser [fijos, medidos, por niveles o reservados](/docs/billing-usage?topic=billing-usage-charges#charges).
* [Nodos trabajadores](#nodes)
* [Redes de salida](#bandwidth)
* [Direcciones IP de subred](#subnet_ips)
* [Almacenamiento](#persistent_storage)
* [Servicios de {{site.data.keyword.Bluemix_notm}}](#services)
* [Red Hat OpenShift on IBM Cloud](#rhos_charges)

<dl>
<dt id="nodes">Nodos trabajadores</dt>
  <dd><p>Los clústeres pueden tener dos tipos principales de nodos trabajadores: máquinas virtuales o físicas (nativas). La disponibilidad y los precios de los tipos de máquina varían según la zona en la que se despliega el clúster.</p>
  <p>Las <strong>máquinas virtuales</strong> ofrecen una mayor flexibilidad, unos tiempos de suministro más reducidos y proporcionan más características automáticas de escalabilidad que las máquinas nativas. Sin embargo, las VM tienen compensaciones en cuanto a rendimiento en comparación con las especificaciones de las máquinas nativas, tales como los umbrales de Gbps de red, RAM y memoria, y las opciones de almacenamiento. Tenga en cuenta estos factores que afectan a los costes de la máquina virtual:</p>
  <ul><li><strong>Compartida frente a dedicada</strong>: si comparte el hardware subyacente de la máquina virtual, el coste es menor que el del hardware dedicado, pero los recursos físicos no están dedicados a la máquina virtual.</li>
  <li><strong>Solo facturación por hora</strong>: la facturación por hora ofrece más flexibilidad para solicitar y cancelar rápidamente máquinas virtuales.
  <li><strong>Niveles de horas al mes</strong>: la facturación por hora funciona por niveles. A medida que la VM permanezca solicitada durante un nivel de horas dentro del mes de facturación, la tasa por hora que se le factura baja. Los niveles de horas son los siguientes: 0 - 150 horas, 151 - 290 horas, 291 - 540 horas y más de 541 horas.</li></ul>
  <p>Las <strong>máquinas físicas (nativas)</strong> ofrecen grandes beneficios en cuanto a alto rendimiento para cargas de trabajo como datos, IA y GPU. Además, todos los recursos de hardware están dedicados a sus cargas de trabajo, así que no tiene "vecinos molestos". Tenga en cuenta estos factores que afectan a los costes de la máquina nativa:</p>
  <ul><li><strong>Solo facturación mensual</strong>: todas las máquinas nativas se facturan mensualmente.</li>
  <li><strong>Proceso de solicitud más largo</strong>: después de solicitar o de cancelar un servidor nativo, el proceso se completa manualmente en la cuenta de la infraestructura de IBM Cloud (SoftLayer). Por lo tanto, puede ser necesario más de un día laborable para completar la tramitación.</li></ul>
  <p>Para obtener detalles sobre las especificaciones de la máquina, consulte [Hardware disponible para los nodos trabajadores](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes).</p></dd>

<dt id="bandwidth">Ancho de banda público</dt>
  <dd><p>Ancho de banda se refiere a la transferencia de datos públicos del tráfico de red de entrada y salida, tanto a como desde los recursos de {{site.data.keyword.Bluemix_notm}} de los centros de datos de todo el mundo. El ancho de banda público se carga por GB. Para revisar el resumen de ancho de banda actual, inicie una sesión en la [consola de {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/), en el menú ![Icono de menú](../icons/icon_hamburger.svg "Icono de menú") seleccione **Infraestructura clásica** y, a continuación, seleccione la página **Red > Ancho de banda > Resumen**.
  <p>Revise los factores siguientes que afectan a los cargos de ancho de banda público:</p>
  <ul><li><strong>Ubicación</strong>: al igual que sucede con los nodos trabajadores, los cargos varían en función de la zona en la que se despliegan los recursos.</li>
  <li><strong>Ancho de banda incluido o pago según uso</strong>: las máquinas de nodo trabajador pueden venir con una determinada asignación de red de salida al mes, como por ejemplo 250 GB para máquinas virtuales o 500 GB para las nativas. O bien, la asignación puede ser de tipo Pago según uso, que se basa en el uso de GB.</li>
  <li><strong>Paquetes por niveles</strong>: si sobrepasa el ancho de banda incluido, se le carga según un esquema de uso por niveles que varía según la ubicación. Si sobrepasa la asignación de un nivel, también se le puede facturar una tarifa de transferencia de datos estándar.</li></ul>
  <p>Para obtener más información, consulte [Paquetes de ancho de banda ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/bandwidth).</p></dd>

<dt id="subnet_ips">Direcciones IP de subred</dt>
  <dd><p>Cuando se crea un clúster estándar, se solicita una subred pública portátil con 8 direcciones IP públicas y se carga a su cuenta mensualmente.</p><p>Si ya tiene subredes disponibles en su cuenta de infraestructura, puede utilizar estas subredes en su lugar. Cree el clúster con el [distintivo](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) `--no-subnets` y luego [reutilice sus subredes](/docs/containers?topic=containers-subnets#subnets_custom).</p>
  </dd>

<dt id="persistent_storage">Almacenamiento</dt>
  <dd>Cuando suministre almacenamiento, puede elegir el tipo de almacenamiento y la clase de almacenamiento adecuados para usted. Los cargos varían en función del tipo de almacenamiento, de la ubicación y de las especificaciones de la instancia de almacenamiento. Algunas soluciones de almacenamiento, como el almacenamiento de archivos y en bloques, ofrecen planes por horas y mensuales entre los que puede elegir. Para elegir la solución de almacenamiento adecuada, consulte [Planificación del almacenamiento persistente de alta disponibilidad](/docs/containers?topic=containers-storage_planning#storage_planning). Para obtener más información, consulte:
  <ul><li>[Precios de almacenamiento de archivos NFS![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/file-storage/pricing)</li>
  <li>[Precios de almacenamiento en bloque![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/block-storage/pricing)</li>
  <li>[Planes de almacenamiento de objetos![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</li></ul></dd>

<dt id="services">Servicios de {{site.data.keyword.Bluemix_notm}}</dt>
  <dd>Cada servicio que integre en su clúster tiene su propio modelo de precios. Revise la documentación de cada producto y utilice la consola de {{site.data.keyword.Bluemix_notm}} para [estimar los costes](/docs/billing-usage?topic=billing-usage-cost#cost).</dd>

<dt id="rhos_charges">Red Hat OpenShift on IBM Cloud</dt>
  <dd>
  <p class="preview">[Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial) está disponible como versión beta para probar clústeres de OpenShift.</p>Si crea un [clúster de Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial), los nodos trabajadores se instalan con el sistema operativo Red Hat Enterprise Linux, lo que aumenta el precio de las [máquinas de nodo trabajador](#nodes). También debe tener una licencia de OpenShift, que incurre en costes mensuales, además de los costes de la máquina virtual por hora o de los costes de los servidores nativos mensuales. La licencia de OpenShift sirve para cada 2 núcleos de nodo trabajador. Si suprime el nodo de trabajador antes de que acabe el mes, la licencia mensual estará disponible para que la utilicen otros nodos trabajadores de la agrupación de nodos trabajadores. Para obtener más información sobre los clústeres de OpenShift, consulte [Creación de un clúster de Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial).</dd>

</dl>
<br><br>

Los recursos mensuales de un mes se facturarán el primero del mes siguiente. Si solicita un recurso mensual a mediados de mes, se le cargará una cantidad prorrateada correspondiente a ese mes. Sin embargo, si cancela un recurso a mediados de mes, se le cargará la totalidad correspondiente al recurso mensual.
{: note}

## ¿Están consolidados los recursos de mi plataforma y de infraestructura en una sola factura?
{: #bill}
{: faq}

Al utilizar una cuenta de {{site.data.keyword.Bluemix_notm}} facturable, se resumen los recursos de la plataforma y de infraestructura en una sola factura. Si enlaza las cuentas de {{site.data.keyword.Bluemix_notm}} y de infraestructura de IBM Cloud (SoftLayer), recibirá una [factura consolidada](/docs/customer-portal?topic=customer-portal-unifybillaccounts#unifybillaccounts) para sus recursos de infraestructura y de la plataforma {{site.data.keyword.Bluemix_notm}}.

## ¿Puedo calcular mis costes?
{: #cost_estimate}
{: faq}

Sí, consulte [Estimación de costes](/docs/billing-usage?topic=billing-usage-cost#cost). Tenga en cuenta que algunos cargos no se reflejan en la estimación, como los precios por niveles para el uso por hora ampliado. Para obtener más información, consulte [¿Qué se me facturará cuando utilice {{site.data.keyword.containerlong_notm}}?](#charges).

## ¿Puedo ver mi uso actual?
{: #usage}
{: faq}

Puede consultar su uso actual y los totales mensuales estimados de los recursos de infraestructura y de la plataforma {{site.data.keyword.Bluemix_notm}}. Para obtener más información, consulte [Visualización del uso](/docs/billing-usage?topic=billing-usage-viewingusage#viewingusage). Para organizar la facturación, puede agrupar los recursos en [grupos de recursos](/docs/resources?topic=resources-bp_resourcegroups#bp_resourcegroups).
