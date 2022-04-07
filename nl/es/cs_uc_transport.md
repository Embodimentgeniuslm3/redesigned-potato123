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


# Casos de uso de {{site.data.keyword.cloud_notm}} en el sector transportes
{: #cs_uc_transport}

En estos casos de uso se muestra cómo pueden aprovechar las cargas de trabajo de {{site.data.keyword.containerlong_notm}} las cadenas de herramientas para actualizar apps rápidamente y para realizar despliegues en diversas regiones de todo el mundo. Al mismo tiempo, estas cargas de trabajo se pueden conectar a sistemas de fondo existentes, utilizar Watson AI para la personalización y acceder a los datos de IOT con {{site.data.keyword.messagehub_full}}.

{: shortdesc}

## Una empresa transportista aumenta la disponibilidad de los sistemas en todo el mundo en el ecosistema de asociados
{: #uc_shipping}

Un ejecutivo de TI dispone de sistemas de direccionamiento y de planificación de envíos a nivel mundial con los que interactúan sus asociados. Los asociados necesitan información de última hora de estos sistemas que acceden a datos de dispositivos IoT. Sin embargo, estos sistemas no podían escalarse en todo el mundo y ofrecer la alta disponibilidad necesaria.
{: shortdesc}

Por qué {{site.data.keyword.cloud_notm}}: {{site.data.keyword.containerlong_notm}} permite escalar las apps contenerizadas con una disponibilidad del 99,999 % para satisfacer las crecientes demandas. Se producen despliegues de apps 40 veces al día cuando los desarrolladores pueden experimentar con facilidad, enviando cambios a los sistemas de desarrollo y de prueba rápidamente. La plataforma IoT facilita el acceso a los datos de IoT.

Tecnologías clave:    
* [Multiregiones para el ecosistema de asociados ](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [Escalado horizontal](/docs/containers?topic=containers-app#highly_available_apps)
* [Cadenas de herramientas abiertas en {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [Servicios de nube para la innovación](https://www.ibm.com/cloud/products/#analytics)
* [{{site.data.keyword.messagehub_full}} para suministrar datos sobre sucesos a las apps](/docs/services/EventStreams?topic=eventstreams-about#about)

**Contexto: una empresa transportista aumenta la disponibilidad de los sistemas en todo el mundo en el ecosistema de asociados**

* Las diferencias regionales en materia de logística de envíos dificultan la gestión de un creciente número de asociados en varios países. Un ejemplo es la reglamentación exclusiva y la logística de tránsito, donde la empresa debe mantener registros coherentes entre fronteras.
* Los datos de última hora significan que los sistemas de todo el mundo deben estar muy disponibles para reducir los retrasos en las operaciones de tránsito. Los horarios de las terminales del puerto están muy controlados y en algunos casos son inflexibles. El uso de la web está creciendo, por lo que la inestabilidad podría dar lugar a una experiencia deficiente para el usuario.
* Los desarrolladores deben desarrollar constantemente las apps relacionadas, pero las herramientas tradicionales ralentizar su capacidad para desplegar actualizaciones y funciones con frecuencia.  

**La solución**

La empresa transportista debe gestionar de forma cohesiva los horarios de los envíos, los inventarios y el papeleo de aduanas. Luego puede compartir de forma precisa la ubicación de los envíos, el contenido de los envíos y los horarios de entrega con sus clientes. Pueden decir con precisión cuándo un artículo (un dispositivo, ropa, etc.) llegará para que los clientes intermediarios de transporte puedan comunicar dicha información a sus clientes.

La solución consta de estos componentes principales:
1. Obtención de datos de sus dispositivos IoT correspondientes al contenedor de envío: manifiesto y ubicación
2. El papeleo de aduanas se comparte digitalmente con los puertos y con los que intervienen en el transporte, incluido control de acceso
3. App para clientes transportistas que agrega y comunica información sobre la llegada de los artículos, incluidas API para clientes transportistas para reutilizar los datos de los envíos en sus propias apps de minoristas y de empresa a empresa

Para que la empresa transportista trabaje con socios globales, los sistemas de direccionamiento y planificación requerían modificaciones locales para ajustarse al idioma, la normativa y la logística de puerto de cada región. {{site.data.keyword.containerlong_notm}} ofrece cobertura global en varias regiones, incluyendo Norteamérica, Europa, Asia y Australia, de modo que las apps reflejen las necesidades de sus asociados en cada país.

Los dispositivos IoT obtienen los datos que {{site.data.keyword.messagehub_full}} distribuye a las apps de puertos regionales y a los almacenes de datos asociados de manifiestos de contenedores y de aduanas. {{site.data.keyword.messagehub_full}} constituye el punto de partida para los sucesos de IoT. Distribuye los sucesos basándose en la conectividad gestionada que Watson IoT Platform ofrece a {{site.data.keyword.messagehub_full}}.

Cuando los sucesos están en {{site.data.keyword.messagehub_full}}, se guardan de forma permanente para su uso inmediato en las apps de tránsito de los puertos y en cualquier momento futuro. Las apps que requieren la latencia más baja se consumen directamente en tiempo real en la secuencia de sucesos ({{site.data.keyword.messagehub_full}}). Otras apps futuras, como las herramientas de análisis, pueden optar por consumir en una modalidad de proceso por lotes desde el almacén de sucesos con {{site.data.keyword.cos_full}}.

Puesto que los datos de los envíos se comparten con los clientes de la empresa, los desarrolladores garantizan que los clientes pueden utilizar las API para ofrecer datos de envíos en sus propias apps. Las apps de seguimiento móvil o las soluciones de comercio electrónico en la web son ejemplos de estas apps. Los desarrolladores también están ocupados con la creación y el mantenimiento de apps de puertos regionales que captan y distribuyen los registros de aduanas y los manifiestos de los envíos. En resumen, tienen que centrarse en la codificación en lugar de en gestionar la infraestructura. Por lo tanto, eligieron {{site.data.keyword.containerlong_notm}} porque IBM simplifica la gestión de la infraestructura:
* Gestión de nodo maestro de Kubernetes, IaaS y componentes operativos, como Ingress y almacenamiento
* Supervisión del estado y la recuperación de los nodos trabajadores
* Suministro de cálculo global, de modo que los desarrolladores no son los responsables de la infraestructura en las regiones en las que residen las cargas de trabajo y los datos

Para lograr una disponibilidad global, los sistemas de desarrollo, prueba y producción se han desplegado en todo el mundo en varios centros de datos. Para conseguir una alta disponibilidad, utilizan una combinación de varios clústeres en distintas regiones geográficas, así como clústeres multizona. Pueden desplegar fácilmente la app de puerto para satisfacer las necesidades de la empresa:
* En Frankfurt, clústeres para cumplir con la normativa europea local.
* En Estados Unidos, clústeres para garantizar la disponibilidad y la recuperación local.

También distribuyen la carga de trabajo entre los clústeres multizona de Frankfurt para asegurarse de que la versión europea de la app está disponible y también equilibra la carga de trabajo de forma eficiente. Debido a que cada región carga datos con la app de puertos, los clústeres de la app están alojados en regiones donde la latencia es baja.

Para los desarrolladores, gran parte del proceso de integración y entrega continua (CI/CD) se puede automatizar con {{site.data.keyword.contdelivery_full}}. La empresa puede definir cadenas de herramientas de flujos de trabajo para preparar imágenes de contenedor, comprobar si hay vulnerabilidades y desplegarlas en el clúster de Kubernetes.

**Modelo de solución**

Cálculo según demanda, almacenamiento y gestión de sucesos que se ejecutan en la nube pública con acceso a datos de envíos en todo el mundo, según sea necesario

Solución técnica:
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cos_full}}
* {{site.data.keyword.contdelivery_full}}

**Paso 1: Contenerizar apps, mediante microservicios**

* Integrar las apps en un conjunto de microservicios cooperativos que se ejecuten en {{site.data.keyword.containerlong_notm}} en función de las áreas funcionales de la app y sus dependencias.
* Desplegar apps en contenedores en {{site.data.keyword.containerlong_notm}}.
* Proporcionar paneles de control de DevOps estandarizados a través de Kubernetes.
* Habilitar el escalado según demanda de la capacidad de cálculo para cargas de trabajo de proceso por lotes y otras cargas de trabajo de inventario que se ejecutan con poca frecuencia.
* Utilizar {{site.data.keyword.messagehub_full}} para gestionar la transferencia de datos desde los dispositivos IoT.

**Paso 2: Garantizar la disponibilidad global**
* Las herramientas de alta disponibilidad integradas en {{site.data.keyword.containerlong_notm}} equilibran la carga de trabajo dentro de cada región geográfica e incluyen reparación automática y equilibrio de carga.
* Equilibrio de carga, cortafuegos y DNS gestionados por los servicios de Internet de IBM Cloud.
* Mediante el uso de herramientas de despliegue de Helm y de cadenas de herramientas, las apps también se despliegan en clústeres en todo el mundo, por lo que las cargas de trabajo y los datos se ajustan a los requisitos regionales.

**Paso 3: Compartir datos**
* {{site.data.keyword.cos_full}} junto con {{site.data.keyword.messagehub_full}} proporcionan almacenamiento de datos en tiempo real e histórico.
* Las API permiten que los clientes de la empresa transportista compartan datos en sus apps.

**Paso 4: Entrega continua**
* {{site.data.keyword.contdelivery_full}} ayuda a los desarrolladores a suministrar rápidamente una cadena de herramientas integrada utilizando plantillas que se pueden personalizar y compartir con herramientas de IBM, de terceros y de código abierto. Permiten automatizar compilaciones y pruebas, controlando la calidad con la analítica.
* Después de que los desarrolladores crean y prueban las apps en sus clústeres de desarrollo y prueba, utilizan las cadenas de herramientas de CI/CD de IBM para desplegar las apps en clústeres de todo el mundo.
* {{site.data.keyword.containerlong_notm}} facilita el despliegue y restablecimiento de apps; se despliegan apps adaptadas que se ajustan a los requisitos regionales mediante el direccionamiento inteligente y el equilibrio de carga de Istio.

**Resultados**

* Con {{site.data.keyword.containerlong_notm}} y las herramientas de CI/CD de IBM, las versiones regionales de las apps se alojan cerca de los dispositivos físicos de los que se recopilan los datos.
* Los microservicios reducen significativamente el tiempo de entrega de parches, correcciones de errores y funciones nuevas. El desarrollo inicial es rápido y se ofrecen actualizaciones frecuentes.
* Los clientes de la empresa transportista tienen acceso en tiempo real a las ubicaciones de los envíos, a los horarios de entrega e incluso a los registros aprobados de los puertos.
* Los socios de tránsito de varios terminales de envío conocen los detalles de los manifiestos y de los envíos, lo que les permite mejorar su logística y evitar retrasos.
* Independientemente de esta historia, [Maersk e IBM se han asociado en un proyecto conjunto](https://www.ibm.com/press/us/en/pressrelease/53602.wss) para mejorar las cadenas de suministro internacionales con Blockchain.

## Una línea aérea consigue un innovador sitio de beneficios de Recursos Humanos (RR.HH.) en menos de 3 emanas
{: #uc_airline}

Un ejecutivo de RR.HH. (CHRO) necesita un nuevo sitio de beneficios de RR.HH. con un chatbot innovador, pero con sus herramientas de desarrollo y su plataforma actuales el tiempo que se tarda en activar las apps es demasiado largo. Esta situación incluye esperas largas para la adquisición de hardware.
{: shortdesc}

Por qué {{site.data.keyword.cloud_notm}}: {{site.data.keyword.containerlong_notm}} agiliza y simplifica el desarrollo de cálculo. Los desarrolladores pueden experimentar fácilmente, impulsando cambios en los sistemas de desarrollo y prueba rápidamente con las cadenas de herramientas abiertas. Sus herramientas tradicionales de desarrollo de software adquieren un nuevo impulso cuando se incorpora IBM Watson Assistant. El nuevo sitio de beneficios se creó en menos de 3 semanas.

Tecnologías clave:    
* [Clústeres que se adaptan a las diversas necesidades de CPU, RAM y almacenamiento](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Servicio de chatbot con el respaldo de Watson](https://developer.ibm.com/code/patterns/create-cognitive-banking-chatbot/)
* [Herramientas nativas de DevOps, que incluyen cadenas de herramientas abiertas en {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**Contexto: creación y despliegue rápidos de un innovador sitio de beneficios de recursos humanos en menos de 3 semanas**
* El aumento del número de empleados y los cambios en las políticas de RR.HH. significaban la necesidad de desarrollar un nuevo sitio para inscripciones anuales.
* Se esperaba que las características interactivas, como un chatbot, ayudaran a comunicar las nuevas políticas de RR.HH. a los empleados existentes.
* Debido al crecimiento del número de empleados, el tráfico del sitio está aumentando, pero su presupuesto de infraestructura sigue siendo el mismo.
* El equipo de RR.HH. tuvo que moverse más rápido: desplegar las nuevas funciones del sitio rápidamente y publicar cambios de última hora en los beneficios con frecuencia.
* El periodo de inscripción dura dos semanas, por lo que no se tolera tiempo de inactividad para la nueva app.

**La solución**

La línea aérea quiere diseñar una cultura abierta en la que se dé prioridad a las personas. El ejecutivo de RR.HH. es muy consciente de que centrarse en que la incentivación y retención de talento afecta a la rentabilidad de la línea aérea. Por lo tanto, el despliegue anual de beneficios es un aspecto clave para incentivar una cultura centrada en los empleados.

Necesitan una solución que ayude a los desarrolladores y a sus usuarios:
* MOSTRAR LOS BENEFICIOS EXISTENTES: seguros médicos, ofertas en formación y en bienestar, etc.
* CARACTERÍSTICAS ESPECÍFICAS DE CADA REGIÓN: cada país tiene políticas de RR.HH. exclusivas, de modo que el sitio global se debe parecer, pero a la vez reflejar los beneficios específicos de cada región.
* HERRAMIENTAS DEL DESARROLLADOR que aceleran el despliegue de características y la solución de problemas.
* CHATBOT para ofrecer conversaciones auténticas sobre los beneficios y para solucionar de forma eficiente dudas y solicitudes de los usuarios.

Solución técnica:
* {{site.data.keyword.containerlong_notm}}
* IBM Watson Assistant y Tone Analyzer
* {{site.data.keyword.contdelivery_full}}
* IBM Logging and Monitoring
* {{site.data.keyword.SecureGatewayfull}}
* {{site.data.keyword.appid_full_notm}}

El desarrollo acelerado constituye una victoria clave para el ejecutivo de RR.HH. El equipo comienza por contenerizar sus apps y colocarlas en la nube. Mediante el uso de contenedores modernos, los desarrolladores pueden experimentar fácilmente con SDK Node.js y enviar los cambios a los sistemas de desarrollo y prueba, escalados en clústeres separados. Esos envíos se han automatizado con cadenas de herramientas abiertas y {{site.data.keyword.contdelivery_full}}. Las actualizaciones para mantener el sitio de RR.HH. ya no pasan por procesos lentos y propensos a errores. Pueden distribuir actualizaciones incrementales a su sitio, a diario o incluso con mayor frecuencia.  Además, el registro y la supervisión del sitio de recursos humanos se integran rápidamente, especialmente en lo que respecta a la forma en que el sitio capta datos personalizados de los sistemas de beneficios de fondo. Los desarrolladores no pierdan el tiempo creando sistemas de registro complejos, solo para poder solucionar los problemas de los sistemas activos. Los desarrolladores no tienen que ser expertos en seguridad en la nube; pueden imponer la autenticación controlada por políticas fácilmente utilizando {{site.data.keyword.appid_full_notm}}.

Con {{site.data.keyword.containerlong_notm}}, pasaron de un hardware sobredimensionado en un centro de datos privado a un cálculo personalizable que reduce las operaciones de TI, el mantenimiento y la energía. Para alojar el sitio de recursos humanos, pudieron diseñar fácilmente clústeres de Kubernetes que se ajustaran a sus necesidades de CPU, RAM y almacenamiento. Otro factor para reducir los costes de personal es que IBM gestiona Kubernetes, de modo que los desarrolladores se pueden centrar en ofrecer a los empleados una mejor experiencia para inscribirse en los beneficios ofrecidos.

{{site.data.keyword.containerlong_notm}} proporciona recursos de cálculo escalables y los paneles de control de DevOps asociados para crear, escalar y eliminar apps y servicios según demanda. Mediante el uso de la tecnología de contenedores estándar del sector, se pueden desarrollar apps rápidamente y se pueden compartir entre los entornos de desarrollo, prueba y producción. Esta configuración proporciona la ventaja inmediata de la escalabilidad. Gracias al despliegue de Kubernetes y a los objetos de tiempo de ejecución, el equipo de RR.HH. puede supervisar y gestionar las actualizaciones de las apps de forma fiable. También pueden duplicar y escalar las apps mediante el uso de reglas definidas y el orquestador automático de Kubernetes.

**Paso 1: Contenedores, microservicios y Garage Method**
* Las apps se organizan en un conjunto de microservicios cooperativos que se ejecuten en {{site.data.keyword.containerlong_notm}}. La arquitectura representa las áreas funcionales de la app con más problemas de calidad.
* Desplegar apps en contenedores en {{site.data.keyword.containerlong_notm}}, que se exploran continuamente con IBM Vulnerability Advisor.
* Proporcionar paneles de control de DevOps estandarizados a través de Kubernetes.
* Adoptar las prácticas de desarrollo ágiles e iterativas esenciales de IBM Garage Method para permitir releases frecuentes de funciones, parches y arreglos sin ocasionar tiempos de inactividad.

**Paso 2: Conexiones con el programa de fondo de beneficios existente**
* Se utiliza {{site.data.keyword.SecureGatewayfull}} para crear un túnel seguro con los sistemas locales que alojan los sistemas de beneficios.  
* La combinación de datos locales y {{site.data.keyword.containerlong_notm}} les permite acceder a datos confidenciales a la vez que cumplen con la normativa.
* Las conversaciones de chatbot suministran datos a las políticas de RR.HH., lo que permite que el sitio de beneficios refleje los beneficios más y menos populares, incluidas mejoras destinadas a las iniciativas que peor funcionan.

**Paso 3: Chatbot y personalización**
* IBM Watson Assistant proporciona herramientas para desarrollar una chatbot que puede proporcionar a los usuarios la información sobre beneficios adecuada.
* Watson Tone Analyzer garantiza que los clientes estén satisfechos con las conversaciones del chatbot y requieran intervención humana cuando sea necesario.

**Paso 4: Entrega continua en todo el mundo**
* {{site.data.keyword.contdelivery_full}} ayuda a los desarrolladores a suministrar rápidamente una cadena de herramientas integrada utilizando plantillas que se pueden personalizar y compartir con herramientas de IBM, de terceros y de código abierto. Permiten automatizar compilaciones y pruebas, controlando la calidad con la analítica.
* Después de que los desarrolladores crean y prueban las apps en sus clústeres de desarrollo y prueba, utilizan las cadenas de herramientas de CI/CD de IBM para desplegar las apps en clústeres de producción de todo el mundo.
* {{site.data.keyword.containerlong_notm}} facilita el despliegue y restablecimiento de apps. Se despliegan apps adaptadas que se ajustan a los requisitos regionales mediante el direccionamiento inteligente y el equilibrio de carga de Istio.
* Las herramientas de alta disponibilidad integradas en {{site.data.keyword.containerlong_notm}} equilibran la carga de trabajo dentro de cada región geográfica e incluyen reparación automática y equilibrio de carga.

**Resultados**
* Con herramientas como el chatbot, el equipo de RR.HH. demostró a sus empleados que la innovación es parte de la cultura corporativa, no solo una palabra de moda.
* La autenticidad junto con la personalización en el sitio satisface las expectativas actuales de los empleados de la línea aérea.
* Se pueden implantar rápidamente actualizaciones de última hora en el sitio de RR.HH., incluidas las impulsadas por las conversaciones de chatbot de los empleados, porque los desarrolladores envían cambios al menos 10 veces al día.
* Con la gestión de la infraestructura en manos de IBM, el equipo de desarrollo dispuso del tiempo necesario para implantar el sitio en solo 3 semanas.
