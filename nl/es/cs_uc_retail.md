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


# Casos de uso de {{site.data.keyword.cloud_notm}} en el sector minorista
{: #cs_uc_retail}

En estos casos de uso se muestra cómo pueden aprovechar las cargas de trabajo de {{site.data.keyword.containerlong_notm}} las analíticas para obtener información sobre el mercado, realizar despliegues en varias regiones en todo el mundo y gestionar el inventario con {{site.data.keyword.messagehub_full}} y almacenamiento de objetos.
{: shortdesc}

## Los minoristas con establecimiento físico comparten datos mediante el uso de API con asociados comerciales globales para realizar ventas a través de varios canales
{: #uc_data-share}

Un ejecutivo de línea de negocio (LOB) necesita aumentar los canales de ventas, pero el sistema de ventas al por menor se limita a un centro de datos local. La competencia tiene asociados comerciales globales para realizar ventas cruzadas y ascendentes de sus productos: tanto en tiendas físicas como en sitios en línea.
{: shortdesc}

Por qué {{site.data.keyword.cloud_notm}}: {{site.data.keyword.containerlong_notm}} proporciona un ecosistema de nube pública cuyos contenedores permiten a otros asociados comerciales y a participantes externos desarrollar conjuntamente apps y datos a través de las API. Ahora que el sistema minorista está en la nube pública, las API también simplifican la compartición de datos y agilizan el desarrollo de nuevas apps. Los despliegues de apps aumentan cuando los desarrolladores pueden experimentar con facilidad, enviando cambios a los sistemas de desarrollo y de prueba rápidamente con cadenas de herramientas.

{{site.data.keyword.containerlong_notm}} y tecnologías clave:
* [Clústeres que se adaptan a las diversas necesidades de CPU, RAM y almacenamiento](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [{{site.data.keyword.cos_full}} para almacenar datos de forma permanente y sincronizarlos entre apps](/docs/tutorials?topic=solution-tutorials-pub-sub-object-storage#pub-sub-object-storage)
* [Herramientas nativas de DevOps, que incluyen cadenas de herramientas abiertas en {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)

**Contexto: un minorista comparte datos, utilizando API con asociados comerciales globales, para impulsar las ventas a través de varios canales**

* El minorista se enfrenta a fuertes presiones de la competencia. En primer lugar, necesita afrontar la complejidad del paso hacia nuevos productos y nuevos canales. Por ejemplo, tienen que ampliar la sofisticación de sus productos. Paralelamente, necesita más simplicidad para que sus clientes salten de marca a marca.
* Esa habilidad de saltar de marca significa que el ecosistema minorista requiere conectividad con los asociados comerciales. En este contexto, la nube puede aportar un nuevo valor de los asociados comerciales, los clientes y otros participantes externos.
* Los eventos que arrastran usuarios, como el Black Friday, presionan los sistemas en línea existentes, forzando al minorista a saturar la infraestructura de cálculo.
* Los desarrolladores del minorista necesitaban hacer evolucionar constantemente las apps, pero las herramientas tradicionales ralentizan su capacidad para desplegar actualizaciones y funciones con frecuencia, especialmente cuando colaboran con equipos de asociados.  

**La solución**

Se necesita una experiencia de compra más inteligente para aumentar la fidelidad de los clientes y el margen de beneficio bruto. El modelo de ventas tradicional del minorista estaba sufriendo debido a la falta de inventario de asociados para realizar ventas cruzadas y ascendentes. Sus compradores buscan la comodidad, para poder encontrar rápidamente artículos relacionados, como pantalones de yoga y alfombrillas.

El minorista también debe proporcionar a los clientes contenidos útiles, como información sobre los productos, información sobre productos alternativos, opiniones y visibilidad de inventario en tiempo real. Y estos clientes lo quieren mientras están en línea y en la tienda a través de dispositivos móviles personales y tiendas asociadas con equipos móviles.

La solución consta de estos componentes principales:
* INVENTARIO: app para el ecosistema de asociados que agrega y comunica inventario, especialmente introducciones de nuevos productos, incluidas las API para que reutilicen los asociados comerciales en sus propias apps de minorista y B2B
* VENTAS CRUZADAS Y ASCENDENTES: app que descubren oportunidades de ventas cruzadas y ascendentes con API que se pueden utilizar en diversas apps de comercio electrónico y para dispositivos móviles
* ENTORNO DE DESARROLLO: clúster de Kubernetes para sistemas de desarrollo, prueba y producción que aumentan la colaboración y la compartición de datos entre el minorista y sus asociados comerciales

Para que el minorista trabaje con asociados comerciales globales, las API de inventario requieren cambios para que se ajusten a las preferencias de idioma y de mercado de cada región. {{site.data.keyword.containerlong_notm}} ofrece cobertura en varias regiones, que incluyen Norteamérica, Europa, Asia y Australia, de modo que las API reflejen las necesidades de cada país y se garantice una latencia baja para las llamadas de API.

Otro requisito es que los datos de inventario se deben poder compartir con los asociados comerciales y los clientes de la empresa. Con las API de inventario, los desarrolladores pueden mostrar información en apps, como apps de inventario móvil o soluciones de comercio electrónico en la web. Los desarrolladores también están ocupados con la creación y el mantenimiento del sitio de comercio electrónico primario. En resumen, tienen que centrarse en la codificación en lugar de en gestionar la infraestructura.

Por lo tanto, eligieron {{site.data.keyword.containerlong_notm}} porque IBM simplifica la gestión de la infraestructura:
* Gestión de nodo maestro de Kubernetes, IaaS y componentes operativos, como Ingress y almacenamiento
* Supervisión del estado y la recuperación de los nodos trabajadores
* Suministro de cálculo global, de modo que los desarrolladores disponen de una infraestructura de hardware propia en las regiones en las que deben residir las cargas de trabajo y los datos

Además, el registro y la supervisión de los microservicios de API, especialmente el modo en que extraen datos personalizados de los sistemas de fondo, se integra fácilmente con {{site.data.keyword.containerlong_notm}}. Los desarrolladores no pierdan el tiempo creando sistemas de registro complejos, solo para poder solucionar los problemas de los sistemas activos.

{{site.data.keyword.messagehub_full}} actúa como la plataforma de sucesos justo a tiempo para incorporar la información en constante cambio de los sistemas de inventario de los asociados comerciales en {{site.data.keyword.cos_full}}.

**Modelo de solución**

Cálculo según demanda, almacenamiento y gestión de sucesos que se ejecutan en la nube pública con acceso a inventarios minoristas de todo el mundo, según sea necesario

Solución técnica:
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cos_full}}
* {{site.data.keyword.contdelivery_full}}
* IBM Cloud Logging and Monitoring
* {{site.data.keyword.appid_short_notm}}

**Paso 1: Contenerizar las apps mediante el uso de microservicios**
* Estructurar las apps en un conjunto de microservicios cooperativos que se ejecuten en {{site.data.keyword.containerlong_notm}} en función de las áreas funcionales de la app y sus dependencias.
* Desplegar apps en imágenes de contenedor que se ejecutan en {{site.data.keyword.containerlong_notm}}.
* Proporcionar paneles de control de DevOps estandarizados a través de Kubernetes.
* Habilitar el escalado según demanda de la capacidad de cálculo para cargas de trabajo de proceso por lotes y otras cargas de trabajo de inventario que se ejecutan con poca frecuencia.

**Paso 2: Garantizar la disponibilidad global**
* Las herramientas de alta disponibilidad integradas en {{site.data.keyword.containerlong_notm}} equilibran la carga de trabajo dentro de cada región geográfica e incluyen reparación automática y equilibrio de carga.
* Equilibrio de carga, cortafuegos y DNS gestionados por los servicios de Internet de IBM Cloud.
* Mediante el uso de herramientas de despliegue de Helm y de cadenas de herramientas, las apps también se despliegan en clústeres en todo el mundo, por lo que las cargas de trabajo y los datos se ajustan a los requisitos regionales, especialmente en cuanto a personalización.

**Paso 3: Comprender a los usuarios**
* {{site.data.keyword.appid_short_notm}} proporciona prestaciones de inicio de sesión sin necesidad de cambiar el código de la app.
* Una vez que los usuarios han iniciado la sesión, puede utilizar {{site.data.keyword.appid_short_notm}} para crear perfiles y personalizar la experiencia de un usuario de la aplicación.

**Paso 4: Compartir datos**
* {{site.data.keyword.cos_full}} más {{site.data.keyword.messagehub_full}} proporciona almacenamiento de datos históricos y en tiempo real, de modo que las ofertas de ventas cruzadas representan el inventario disponible de los asociados comerciales.
* Las API permiten a los asociados comerciales del minorista compartir datos en sus apps de comercio electrónico y B2B.

**Paso 5: Entrega continua**
* La depuración de las API desarrolladas en cooperación se facilita cuando se añaden herramientas de registro y supervisión de IBM Cloud, basadas en la nube y a las que pueden acceder los distintos desarrolladores.
* {{site.data.keyword.contdelivery_full}} ayuda a los desarrolladores a suministrar rápidamente una cadena de herramientas integrada utilizando plantillas que se pueden personalizar y compartir con herramientas de IBM, de terceros y de código abierto. Permiten automatizar compilaciones y pruebas, controlando la calidad con la analítica.
* Después de que los desarrolladores creen y prueben las apps en sus clústeres de desarrollo y prueba, utilizan las cadenas de herramientas de integración y entrega continua (CI y CD) de IBM para desplegar las apps en clústeres en todo el mundo.
* {{site.data.keyword.containerlong_notm}} facilita el despliegue y restablecimiento de apps; se despliegan apps adaptadas para campañas de pruebas mediante el direccionamiento inteligente y el equilibrio de carga de Istio.

**Resultados**
* Los microservicios reducen significativamente el tiempo de entrega de parches, correcciones de errores y funciones nuevas. El desarrollo inicial en todo el mundo es rápido y se ofrecen actualizaciones frecuentes (hasta 40 veces por semana).
* El minorista y sus asociados comerciales tienen acceso inmediato a las planificaciones de disponibilidad de inventario y de entrega, utilizando las API.
* Con {{site.data.keyword.containerlong_notm}} y las herramientas CI y CD de IBM, las versiones A-B de las apps están listas para las campañas de pruebas.
* {{site.data.keyword.containerlong_notm}} proporciona un cálculo escalable, de modo que las cargas de trabajo de API de inventario y ventas cruzadas pueden crecer durante periodos de alto volumen del año, como por ejemplo las vacaciones de otoño.

## El tendero tradicional aumenta el tráfico de clientes y las ventas con conocimientos digitales
{: #uc_grocer}

Un director de marketing (CMO) necesita aumentar el tráfico de clientes en un 20 % en las tiendas convirtiendo las tiendas en un activo diferenciador. Las grandes superficies y los minoristas en línea le están robando ventas. Paralelamente, el CMO tiene que reducir el inventario sin rebajas porque mantener el inventario demasiado tiempo paraliza el capital.
{: shortdesc}

Por qué {{site.data.keyword.cloud_notm}}: {{site.data.keyword.containerlong_notm}} facilita el aumento de la capacidad de cálculo, con lo cual los desarrolladores pueden añadir rápidamente servicios de analítica de nube para obtener información valiosa sobre el comportamiento de las ventas y la capacidad de adaptación al mercado digital.

Tecnologías clave:    
* [Escalado horizontal para acelerar el desarrollo](/docs/containers?topic=containers-app#highly_available_apps)
* [Clústeres que se adaptan a las diversas necesidades de CPU, RAM y almacenamiento](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Datos valiosos sobre tendencias del mercado con Watson Discovery](https://www.ibm.com/watson/services/discovery/)
* [Herramientas nativas de DevOps, que incluyen cadenas de herramientas abiertas en {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [Gestión de inventario con {{site.data.keyword.messagehub_full}}](/docs/services/EventStreams?topic=eventstreams-about#about)

**Contexto: el tendero tradicional aumenta el tráfico de clientes y las ventas con conocimientos digitales**

* Las presiones de la competencia, procedentes de minoristas con presencia en línea y grandes superficies, afectaron a los modelos tradicionales de negocio minorista de comestibles. Las ventas están disminuyendo, lo que queda en evidencia por el poco tráfico a pie en las tiendas físicas.
* Su programa de fidelidad necesita un empujón con un enfoque moderno de los cupones impresos de caja. Así que los desarrolladores deben desarrollar constantemente las apps relacionadas, pero las herramientas tradicionales ralentizar su capacidad para desplegar actualizaciones y funciones con frecuencia.  
* Parte del inventario de gran valor no se están moviendo todo lo bien que se esperaba, y paralelamente el movimiento "foodie" parece estar creciendo en los principales mercados metropolitanos.

**La solución**

El tendero necesita una app para aumentar la conversión y el tráfico en la tienda para generar nuevas ventas y asegurarse la fidelidad del cliente en una plataforma de analítica de nube reutilizable. La experiencia destinada a la tienda física puede ser un evento, junto con un proveedor de servicios o productos que atraiga tanto la fidelidad de los clientes existentes como a nuevos clientes, en función de la afinidad con el evento específico. La tienda y los asociados ofrecen incentivos para acudir al evento, así como para comprar productos en la tienda o del asociado comercial.  

Después del evento, se acompaña a los clientes a comprar los productos necesarios, para que puedan repetir la actividad demostrada por su cuenta en el futuro. La experiencia del cliente se mide con el canjeo de incentivos y con las nuevas inscripciones de los clientes a programas de fidelidad. La combinación de un evento de marketing altamente personalizado y una herramienta para realizar un seguimiento de las compras en la tienda puede convertir la experiencia personalizada en compras de productos. Todas estas acciones dan lugar a un mayor tráfico y a conversiones.

Como evento de ejemplo, se invita a un chef local a la tienda para mostrar cómo hace un menú gourmet. La tienda ofrece un incentivo para captar la atención. Por ejemplo, ofrece un aperitivo gratuito en el restaurante del chef y un incentivo adicional para comprar los ingredientes del menú de la demostración (por ejemplo, un descuento de 20 dólares por compras superiores a 150 dólares).

La solución consta de estos componentes principales:
1. ANÁLISIS DE INVENTARIO: los eventos en la tienda (recetas, listas de ingredientes y ubicaciones de productos) están hechos pensando en la comercialización del inventario con poca rotación.
2. La APP MÓVIL DE FIDELIZACIÓN proporciona marketing personalizado con cupones digitales, listas de compras, inventario de productos (precios, disponibilidad) en un mapa de la tienda, y compartición en redes sociales.
3. La ANALÍTICA DE REDES SOCIALES proporciona personalización ya que detecta las preferencias de los clientes en términos de tendencias: tipos de cocina, chefs e ingredientes. Las analíticas conectan tendencias regionales con la actividad en Twitter, Pinterest e Instagram de un individuo.
4. Las HERRAMIENTAS DEL DESARROLLADOR aceleran el despliegue de características y la solución de problemas.

Los sistemas de inventario de fondo para el inventario de productos, la reposición de las tiendas y la previsión de productos disponen de una gran cantidad de información, pero la analítica moderna puede revelar nuevas perspectivas sobre cómo mover mejor los productos. Mediante el uso de una combinación de {{site.data.keyword.cloudant}} e IBM Streaming Analytics, el CMO puede dar con la combinación perfecta de ingredientes que se adapten a los eventos personalizados en la tienda.

{{site.data.keyword.messagehub_full}} actúa como la plataforma de sucesos justo a tiempo para incorporar la información en constante cambio de los sistemas de inventario a IBM Streaming Analytics.

La analítica de redes sociales con Watson Discovery (datos sobre personalidad y estilo) también se alimentan de las tendencias del análisis de inventario para mejorar la previsión del producto.

La app móvil de fidelización proporciona información de personalización detallada, especialmente cuando los clientes utilizan sus funciones de compartición en redes social, como por ejemplo para publicar recetas.

Además de la app móvil, los desarrolladores están ocupados con la creación y el mantenimiento de la app de fidelización existente, que está vinculada a los tradicionales cupones de caja. En resumen, tienen que centrarse en la codificación en lugar de en gestionar la infraestructura. Por lo tanto, eligieron {{site.data.keyword.containerlong_notm}} porque IBM simplifica la gestión de la infraestructura:
* Gestión de nodo maestro de Kubernetes, IaaS y componentes operativos, como Ingress y almacenamiento
* Supervisión del estado y la recuperación de los nodos trabajadores
* Suministro de cálculo global, para que los desarrolladores no sean los responsables de configurar la infraestructura en los centros de datos

**Modelo de solución**

Cálculo según demanda, almacenamiento y gestión de sucesos que se ejecutan en la nube pública con acceso a sistemas ERP de fondo

Solución técnica:
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cloudant}}
* IBM Streaming Analytics
* IBM Watson Discovery

**Paso 1: Contenerizar apps, mediante microservicios**

* Estructurar el análisis de inventario y las apps para móviles con microservicios y despliéguelos en contenedores en {{site.data.keyword.containerlong_notm}}.
* Proporcionar paneles de control de DevOps estandarizados a través de Kubernetes.
* Escalar el cálculo según demanda para cargas de trabajo de inventario y por lotes que se ejecutan con menos frecuencia.

**Paso 2: Analizar inventario y tendencias**
* {{site.data.keyword.messagehub_full}} actúa como la plataforma de sucesos justo a tiempo para incorporar la información en constante cambio de los sistemas de inventario a IBM Streaming Analytics.
* El análisis de redes sociales con Watson Discovery y los datos de sistemas de inventario se integra con IBM Streaming Analytics para ofrecer asesoramiento de marketing y comercialización.

**Paso 3: Ofrecer promociones con la app de fidelización para móviles**
* Agilizar el desarrollo de la app para móviles con IBM Mobile Starter Kit y otros servicios para móviles de IBM, como {{site.data.keyword.appid_full_notm}}.
* Las promociones en forma de cupones y otros formatos se envían a la app móvil de los usuarios. Las promociones se identifican utilizando el inventario y el análisis de redes sociales, además de otros sistemas.
* El almacenamiento de recetas en promoción en la app para móviles y conversiones (cupones que se canjean en caja) se vuelven a enviar a los sistemas ERP para un análisis adicional.

**Resultados**
* Con {{site.data.keyword.containerlong_notm}}, los microservicios reducen significativamente el tiempo de entrega de parches, correcciones de errores y funciones nuevas. El desarrollo inicial es rápido y se ofrecen actualizaciones frecuentes.
* El tráfico de clientes y las ventas se incrementaron en las tiendas, convirtiendo las propias tiendas en un activo diferenciador.
* Al mismo tiempo, la información extraída de redes sociales y análisis reducen los gastos operativos (OpEx) de inventario.
* La compartición en redes sociales de la app móvil también ayuda a identificar y comercializar a nuevos clientes.
