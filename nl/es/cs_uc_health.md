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


# Casos de uso de {{site.data.keyword.cloud_notm}} en el sector sanitario
{: #cs_uc_health}

En estos casos de uso se describe cómo las cargas de trabajo de {{site.data.keyword.containerlong_notm}} se benefician de la nube pública. Disponen de cálculo seguro en un entorno nativo aislado, utilización sencilla de clústeres para un desarrollo más rápido, migración desde máquinas virtuales y compartición de datos en las bases de datos de la nube.
{: shortdesc}

## Un proveedor de atención sanitaria migra cargas de trabajo desde máquinas virtuales ineficaces a contenedores que facilitan el funcionamiento para sistemas de informes y pacientes
{: #uc_migrate}

Un ejecutivo de TI de un proveedor de atención sanitaria tiene sistemas locales de informes del negocio y de pacientes. Estos sistemas pasan por ciclos lentos de mejora, lo que hace que se estanquen los niveles de servicio al paciente.
{: shortdesc}

Por qué {{site.data.keyword.cloud_notm}}: para mejorar el servicio al paciente, el proveedor pensó en {{site.data.keyword.containerlong_notm}} y {{site.data.keyword.contdelivery_full}} para reducir el gasto en TI y acelerar el desarrollo, todo ello en una plataforma segura. Los sistemas SaaS del proveedor, que albergaban tanto los sistemas de registros de pacientes como las apps de informes de la empresa, necesitaban actualizaciones frecuentes. Sin embargo, el entorno local obstaculiza el desarrollo ágil. El proveedor también quería reducir los crecientes costes laborales y el presupuesto.

Tecnologías clave:
* [Clústeres que se adaptan a las diversas necesidades de CPU, RAM y almacenamiento](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Escalado horizontal](/docs/containers?topic=containers-app#highly_available_apps)
* [Seguridad y aislamiento de contenedores](/docs/containers?topic=containers-security#security)
* [Herramientas nativas de DevOps, que incluyen cadenas de herramientas abiertas en {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)
* [Capacidad de inicio de sesión sin cambiar el código de la app mediante {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-getting-started)

Empezaron por contenerizar sus sistemas SaaS y colocarlos en la nube. Desde este primer paso, pasaron de un hardware sobredimensionado en un centro de datos privado a un cálculo personalizable que reduce las operaciones de TI, el mantenimiento y la energía. Para alojar los sistemas SaaS, diseñaron fácilmente clústeres de Kubernetes que se ajustaran a sus necesidades de CPU, RAM y almacenamiento. Otro factor para reducir los costes de personal es que IBM gestiona Kubernetes, de modo que el proveedor puede centrarse en ofrecer un mejor servicio al cliente.

El desarrollo acelerado constituye una victoria clave para el ejecutivo de TI. Con el traslado a la nube pública, los desarrolladores pueden experimentar fácilmente con SDK Node.js, enviando los cambios a los sistemas de desarrollo y prueba, escalados en clústeres separados. Esos envíos se han automatizado con cadenas de herramientas abiertas y {{site.data.keyword.contdelivery_full}}. Las actualizaciones del sistema SaaS ya no pasan por procesos de compilación lentos y propensos a errores. Los desarrolladores distribuir actualizaciones incrementales a sus usuarios, a diario o incluso con mayor frecuencia.  Además, el registro y la supervisión de los sistemas SaaS, especialmente en lo referente a la interacción entre los sistemas de informes internos y externos de los pacientes, se integran rápidamente en el sistema. Los desarrolladores no pierdan el tiempo creando sistemas de registro complejos, solo para poder solucionar los problemas de los sistemas activos.

La seguridad es lo primero: con el entorno nativo para {{site.data.keyword.containerlong_notm}}, las cargas de trabajo confidenciales de los pacientes ahora tienen un aislamiento familiar, pero dentro de la flexibilidad de la nube pública. El entorno nativo proporciona Trusted Compute, que verifica el hardware subyacente para protegerlo frente a una posible manipulación. Desde el punto de vista de la seguridad, Vulnerability Advisor ofrece los siguientes escaneos:
* Escaneo de vulnerabilidad de imágenes
* Escaneo de políticas basadas en ISO 27k
* Escaneo de contenedores
* Escaneo de paquetes en busca de malware conocido

Los datos protegidos de los pacientes se traducen en pacientes satisfechos.

**Contexto: Migración de carga de trabajo para el proveedor de servicios sanitarios**

* La deuda técnica, que está asociada a largos ciclos de publicación de releases, está obstaculizando los sistemas de creación de informes y de gestión de pacientes del proveedor.
* Sus apps personalizadas de fondo y frontales se distribuyen de forma local en imágenes monolíticas de máquina virtual.
* Necesitan revisar sus procesos, sus métodos y sus herramientas, pero no saben muy bien por dónde empezar.
* Su deuda técnica está creciendo, en lugar de reducirse, por la incapacidad para sacar al mercado software de calidad para mantenerse al día con las demandas del mercado.
* La seguridad es muy importante, y este problema se suma a la carga que supone la distribución, que causa aún más retrasos.
* Los presupuestos de gastos de capital están muy restringidos, y el personal de TI tiene la sensación de no disponer de presupuesto o de personal para crear entornos necesarios de prueba con sus sistemas locales.

**Modelo de solución**

Servicios de cálculo según demanda, almacenamiento y E/S que se ejecutan en la nube pública con acceso seguro a los activos locales de la empresa. Implementar un proceso CI/CD y otros elementos de IBM Garage Method para acortar significativamente los ciclos de entrega.

**Paso 1: Proteger la plataforma de cálculo**
* Las apps que gestionan datos muy confidenciales de los pacientes se pueden realojar en {{site.data.keyword.containerlong_notm}} que se ejecuta en un entorno nativo para Trusted Compute.
* Trusted Compute puede verificar el hardware subyacente para protegerlo frente a una posible manipulación.
* Desde ese punto de partida, Vulnerability Advisor proporciona funciones de exploración de imágenes, políticas, contenedores y vulnerabilidades de exploración de paquetes en busca de software malicioso conocido.
* Aplique de forma coherente la autenticación controlada por políticas a los servicios y las API con una anotación de Ingress simple. Con la seguridad declarativa, puede garantizar la autenticación de usuarios y la validación de señales mediante {{site.data.keyword.appid_short_notm}}.

**Paso 2: Transformación**
* Migrar las imágenes de máquina virtual a imágenes de contenedor que se ejecutan en {{site.data.keyword.containerlong_notm}} a la nube pública.
* Proporcionar paneles de control de DevOps estandarizados y prácticas a través de Kubernetes.
* Habilitar el escalado según demanda de la capacidad de cálculo para cargas de trabajo de proceso por lotes y otras cargas de trabajo de fondo se ejecutan con poca frecuencia.
* Utilizar {{site.data.keyword.SecureGatewayfull}} para mantener conexiones seguras con DBMS local.
* Los costes del centro de datos privado y de capital local se reducen enormemente y se sustituyen por un modelo de cálculo de utilidad que se basa en la demanda de la carga de trabajo.

**Paso 3: Microservicios y Garage Method**
* Rediseñar las apps en un conjunto de microservicios cooperativos. Este conjunto se ejecuta dentro de {{site.data.keyword.containerlong_notm}} que se basa en áreas funcionales de la app con más problemas de calidad.
* Utilizar {{site.data.keyword.cloudant}} con claves proporcionadas por el cliente para almacenar en memoria caché los datos de la nube.
* Adopte prácticas de integración y entrega continua (CI/CD) para que los desarrolladores creen versiones y releases de un microservicio siguiendo su propia planificación según sea necesario. {{site.data.keyword.contdelivery_full}} proporciona las cadenas de herramientas de flujo de trabajo para el proceso de CI/CD junto con funciones de creación de imágenes y de exploración de vulnerabilidades de las imágenes del contenedor.
* Adoptar las prácticas de desarrollo ágiles e iterativas de IBM Garage Method para permitir releases frecuentes de funciones, parches y arreglos sin ocasionar tiempos de inactividad.

**Solución técnica**
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGatewayfull}}
* {{site.data.keyword.appid_short_notm}}

Para las cargas de trabajo más confidenciales, los clústeres se pueden alojar en {{site.data.keyword.containerlong_notm}} para el entorno nativo.  Proporciona una plataforma de cálculo fiable que explora automáticamente el hardware y el código de tiempo de ejecución para detectar vulnerabilidades. Mediante el uso de la tecnología contenedores estándar del sector, las apps se pueden realojar inicialmente en {{site.data.keyword.containerlong_notm}} de forma rápida sin tener que realizar cambios arquitectónicos importantes. Este cambio proporciona la ventaja inmediata de la escalabilidad.

Pueden duplicar y escalar las apps mediante el uso de reglas definidas y el orquestador automático de Kubernetes. {{site.data.keyword.containerlong_notm}} proporciona recursos de cálculo escalables y los paneles de control de DevOps asociados para crear, escalar y eliminar apps y servicios según demanda. Gracias al despliegue de Kubernetes y a los objetos de tiempo de ejecución, el proveedor puede supervisar y gestionar las actualizaciones de las apps de forma fiable.

Se utiliza {{site.data.keyword.SecureGatewayfull}} para crear una interconexión segura con bases de datos y documentos locales para apps realojadas para que se ejecuten en {{site.data.keyword.containerlong_notm}}.

{{site.data.keyword.cloudant}} es una moderna base de datos NoSQL que se adapta a muchos casos de uso controlados por datos: desde el par clave-valor hasta el almacenamiento y consulta complejos de datos orientados a documentos. Para minimizar las consultas a las bases de datos relacionales de fondo, se utiliza {{site.data.keyword.cloudant}} para almacenar en memoria caché los datos de sesión de usuario entre las apps. Estas opciones mejoran el rendimiento y la usabilidad para el usuario final entre las apps de {{site.data.keyword.containerlong_notm}}.

Sin embargo, el traslado de las cargas de trabajo de cálculo a {{site.data.keyword.cloud_notm}} no es suficiente. El proveedor también debe transformar métodos y procesos. Mediante la adopción de las prácticas del método IBM Garage Method, el proveedor puede implementar un proceso de entrega ágil e iterativo que dé soporte a las prácticas modernas de DevOps, como CI/CD.

Gran parte del propio proceso de CI/CD se automatiza con el servicio Continuous Delivery de IBM en la nube. El proveedor puede definir cadenas de herramientas de flujos de trabajo para preparar imágenes de contenedor, comprobar si hay vulnerabilidades y desplegarlas en el clúster de Kubernetes.

**Resultados**
* El traslado de las VM monolíticas existentes a contenedores alojados en la nube fue un primer paso que permitió al proveedor ahorrar en costes de capital y comenzar a modernizar las prácticas de DevOps.
* La reestructuración de apps monolíticas en un conjunto de microservicios personalizados redujo significativamente el tiempo de entrega de parches, arreglos y nuevas funciones.
* Paralelamente, el proveedor implementó iteraciones sencillas para controlar la deuda técnica existente.

## Una empresa de investigación sin ánimo de lucro aloja datos confidenciales, al tiempo que aumenta la investigación con asociados
{: #uc_research}

Un ejecutivo de desarrollo de una organización sin ánimo de lucro de investigación de enfermedades dispone de investigadores académicos e industriales que no pueden compartir fácilmente los datos de sus investigaciones. Trabajan de forma aislada en distintos puntos del globo debido a las normativas de conformidad regionales y a las bases de datos centralizadas.
{: shortdesc}

Por qué {{site.data.keyword.cloud_notm}}: {{site.data.keyword.containerlong_notm}} proporciona un cálculo seguro que puede alojar el proceso de datos confidenciales de alto rendimiento en una plataforma abierta. Esa plataforma global se encuentra en regiones cercanas. De modo que cumple con las normativas locales, lo que inspira confianza a los pacientes y a los investigadores sobre la protección local de sus datos y genera mejores resultados.

Tecnologías clave:
* [Una planificación inteligente coloca las cargas de trabajo donde se necesitan](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [{{site.data.keyword.cloudant}} para almacenar datos de forma permanente y sincronizarlos entre apps](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)
* [Exploración y aislamiento de vulnerabilidades de las cargas de trabajo](/docs/services/Registry?topic=va-va_index#va_index)
* [Herramientas nativas de DevOps, que incluyen cadenas de herramientas abiertas en {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [{{site.data.keyword.openwhisk}} para sanear los datos y notificar a los investigadores sobre cambios en la estructura de datos](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant#openwhisk_cloudant)

**Contexto: alojamiento y compartición seguros de datos sobre enfermedades para la organización de investigación sin ánimo de lucro**

* Grupos dispares de investigadores de diversas instituciones no tienen una forma unificada de compartir datos, lo que ralentiza la colaboración.
* La preocupación por la seguridad se suma a la carga de colaboración, lo que hace que aún se comparta menos la investigación.
* Los desarrolladores e investigadores están repartidos por todo el mundo y en distintas organizaciones, lo que significa que PaaS y Saas son la mejor opción para cada grupo de usuarios.
* Las diferencias regionales en las normativas sobre sanidad exigen que algunos datos y procesos de datos permanezcan dentro de esa región.

**La solución**

La organización de investigación sin ánimo de lucrativo quiere agregar datos de investigación sobre el cáncer de todo el mundo. Así que crea una división que está dedicada a ofrecer soluciones a sus investigadores:
* INGESTA - Apps para ingerir datos de investigación. Los investigadores utilizan hojas de cálculo, documentos, productos comerciales y bases de datos de su propiedad para registrar los resultados de las investigaciones. Es poco probable que esta situación cambie con el intento de la organización de centralizar el análisis de datos.
* ANONIMIZACIÓN - Apps para que los datos sean anónimos. Se debe eliminar la información personal confidencial para cumplir con la normativa regional en cuanto a sanidad.
* ANÁLISIS - Apps para analizar los datos. El patrón básico consiste en almacenar los datos en un formato normal y luego consultarlo y procesarlos utilizando tecnología de IA y de machine learning (ML), regresiones simples, etc.

Los investigadores tienen que afiliarse a un clúster regional, y las apps proporcionan datos, los transforman y los anonimizan:
1. Sincronización de datos anónimos de clústeres regionales o envío de los mismos a un almacén de datos centralizado
2. Proceso de los datos, mediante ML como PyTorch en nodos trabajadores nativos que proporcionan GPU

**INGESTA** Se utiliza {{site.data.keyword.cloudant}} en cada clúster regional que almacena documentos de datos de los investigadores, que se pueden consultar y procesar según sea necesario. {{site.data.keyword.cloudant}} cifra los datos en reposo y en tránsito, lo que cumple con las leyes de privacidad de datos regionales.

Se utiliza {{site.data.keyword.openwhisk}} para crear funciones de proceso que ingieren datos de investigación y los almacenan como documentos de datos estructurados en {{site.data.keyword.cloudant}}. {{site.data.keyword.SecureGatewayfull}} proporciona un método sencillo para que {{site.data.keyword.openwhisk}} acceda a los datos locales de forma segura.

Las apps web de los clústeres regionales se desarrollan en nodeJS para la entrada manual de datos de resultados, la definición de esquemas y la afiliación de las organizaciones de investigación. IBM Key Protect ayuda a proteger el acceso a los datos de {{site.data.keyword.cloudant}} e IBM Vulnerability Advisor explora los contenedores de apps y las imágenes en busca de vulnerabilidades de seguridad.

**ANONIMIZAR** Cada vez que se guardan datos nuevos en {{site.data.keyword.cloudant}}, se desencadena un suceso y una función de Cloud anonimiza los datos y elimina la información personal confidencial del documento de datos. Estos documentos con datos anónimos se almacenan separados de los datos "brutos" ingeridos y son los únicos documentos que se comparten entre regiones para su análisis.

**ANALIZAR** Las infraestructuras de machine learning consumen muchos recursos de cálculo, por lo que la organización sin ánimo de lucro configura un clúster de proceso global de nodos trabajadores nativos. El clúster de proceso global tiene asociada una base de datos de {{site.data.keyword.cloudant}} con datos anónimos. Un trabajo cron desencadena de forma periódica una función de Cloud para enviar los documentos con datos anónimos de los centros de datos a la instancia de {{site.data.keyword.cloudant}} del clúster de proceso global.

El clúster de cálculo ejecuta la infraestructura PyTorch ML y las apps de machine learning se escriben en Python para analizar los datos agregados. Además de las apps ML, los investigadores del grupo colectivo también desarrollan sus propias apps que se pueden publicar y ejecutar en el clúster global.

La organización sin ánimo de lucro también proporciona apps que se ejecutan en nodos nativos del clúster global. Las apps ven y extraen los datos agregados y la salida de la app ML. Estas apps son accesibles mediante un punto final público, que está protegido por la pasarela de API. A continuación, los investigadores y analistas de datos de cualquier lugar del mundo pueden descargar conjuntos de datos y realizar sus propios análisis.

**Alojamiento de cargas de trabajo de investigación en {{site.data.keyword.containerlong_notm}}**

Los desarrolladores comenzaron por desplegar sus apps SaaS para compartir investigaciones en contenedores con {{site.data.keyword.containerlong_notm}}. Crearon clústeres para un entorno de desarrollo compartido que permiten a los desarrolladores de todo el mundo desplegar de forma colaborativa mejoras de apps rápidamente.

La seguridad es lo primero: el ejecutivo de desarrollo eligió Trusted Compute para el servidor nativo donde alojar los clústeres de investigación. Con el entorno nativo para {{site.data.keyword.containerlong_notm}}, las cargas de trabajo confidenciales de investigación ahora tienen un aislamiento familiar, pero dentro de la flexibilidad de la nube pública. El entorno nativo proporciona Trusted Compute, que verifica el hardware subyacente para protegerlo frente a una posible manipulación. Puesto que la organización sin ánimo de lucro tiene vínculos con empresas farmacéuticas, la seguridad de las apps resulta crucial. La competencia es feroz, y ahí tiene cabida el espionaje corporativo. Desde el punto de vista de la seguridad, Vulnerability Advisor ofrece los siguientes escaneos:
* Escaneo de vulnerabilidad de imágenes
* Escaneo de políticas basadas en ISO 27k
* Escaneo de contenedores
* Escaneo de paquetes en busca de malware conocido

Las apps de investigación seguras aumentan la participación en ensayos clínicos.

Para lograr una disponibilidad global, los sistemas de desarrollo, prueba y producción se despliegan en todo el mundo en varios centros de datos. Para conseguir una alta disponibilidad, utilizan una combinación de clústeres en varias regiones geográficas, así como clústeres multizona. Pueden desplegar fácilmente la app de investigación en clústeres de Frankfurt para cumplir con la normativa europea local. También despliegan la app en los clústeres de Estados Unidos para garantizar la disponibilidad y la recuperación local. También distribuyen la carga de trabajo de investigación entre los clústeres multizona de Frankfurt para asegurarse de que la app europea está disponible y también equilibra la carga de trabajo de forma eficiente. Debido a que los investigadores cargan datos confidenciales con la app para compartir investigaciones, los clústeres de la app están alojados en regiones donde se aplican regulaciones más estrictas.

Los desarrolladores se centran en los problemas del dominio, utilizando las herramientas existentes: en lugar de escribir código ML exclusivo, la lógica ML se incorpora a las apps, enlazando los servicios de {{site.data.keyword.cloud_notm}} a los clústeres. Los desarrolladores también se liberan de las tareas de gestión de la infraestructura, ya que IBM se ocupa de las actualizaciones de Kubernetes y de la infraestructura, de la seguridad y de más.

**La solución**

Cálculo según demanda, almacenamiento y kits de inicio de nodos que se ejecutan en la nube pública con acceso seguro a los datos de investigación de todo el mundo, según sea necesario. El cálculo de los clústeres es a prueba de manipulaciones y está aislado en entornos nativos.

Solución técnica:
* {{site.data.keyword.containerlong_notm}} con Trusted Compute
* {{site.data.keyword.openwhisk}}
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGatewayfull}}

**Paso 1: Contenerizar las apps mediante el uso de microservicios**
* Utilizar el kit de inicio Node.js de IBM para el desarrollo inicial.
* Estructurar las apps en un conjunto de microservicios cooperativos que se ejecuten en {{site.data.keyword.containerlong_notm}} en función de las áreas funcionales de la app y sus dependencias.
* Desplegar las apps de investigación en contenedores en {{site.data.keyword.containerlong_notm}}.
* Proporcionar paneles de control de DevOps estandarizados a través de Kubernetes.
* Habilitar el escalado según demanda de la capacidad de cálculo para cargas de trabajo de proceso por lotes y otras cargas de trabajo de investigación que se ejecutan con poca frecuencia.
* Utilizar {{site.data.keyword.SecureGatewayfull}} para mantener las conexiones seguras con las bases de datos locales existentes.

**Paso 2: Utilizar un sistema cálculo seguro y de alto rendimiento**
* Las apps ML que requieren un cálculo de mayor rendimiento están alojadas en {{site.data.keyword.containerlong_notm}} en el entorno nativo. Este clúster ML está centralizado, por lo que cada clúster regional no incurre en el gasto de los nodos trabajadores nativos; los despliegues de Kubernetes también resultan más sencillos.
* Las apps que procesan datos clínicos altamente confidenciales se pueden alojar en {{site.data.keyword.containerlong_notm}} que se ejecuta en un entorno nativo para Trusted Compute.
* Trusted Compute puede verificar el hardware subyacente para protegerlo frente a una posible manipulación. Desde ese punto de partida, Vulnerability Advisor proporciona funciones de exploración de imágenes, políticas, contenedores y vulnerabilidades de exploración de paquetes en busca de software malicioso conocido.

**Paso 3: Garantizar la disponibilidad global**
* Después de que los desarrolladores crean y prueban las apps en sus clústeres de desarrollo y prueba, utilizan las cadenas de herramientas de CI/CD de IBM para desplegar las apps en clústeres de todo el mundo.
* Las herramientas de alta disponibilidad integradas en {{site.data.keyword.containerlong_notm}} equilibran la carga de trabajo dentro de cada región geográfica e incluyen reparación automática y equilibrio de carga.
* Mediante el uso de herramientas de despliegue de Helm y de cadenas de herramientas, las apps también se despliegan en clústeres en todo el mundo, por lo que las cargas de trabajo y los datos se ajustan a las normativas regionales.

**Paso 4: Compartir datos**
* {{site.data.keyword.cloudant}} es una moderna base de datos NoSQL que se adapta a muchos casos de uso controlados por datos: desde el par clave-valor hasta el almacenamiento y consulta complejos de datos orientados a documentos.
* Para minimizar las consultas a las bases de datos regionales, se utiliza {{site.data.keyword.cloudant}} para almacenar en memoria caché los datos de sesión de usuario entre las apps.
* Esta opción mejora el rendimiento y la usabilidad para el usuario final entre las apps de {{site.data.keyword.containerlong_notm}}.
* Mientras las apps de {{site.data.keyword.containerlong_notm}} analizan los datos locales y guardan los resultados en {{site.data.keyword.cloudant}}, {{site.data.keyword.openwhisk}} reacciona ante cambios y limpia automáticamente los datos de entrada.
* Paralelamente, se pueden activar notificaciones sobre avances en la investigación en una región mediante cargas de datos de modo que todos los investigadores puedan aprovechar los nuevos datos.

**Resultados**
* Con los kits de inicio, {{site.data.keyword.containerlong_notm}} y las herramientas de IBM CI/CD, los desarrolladores globales trabajan en instituciones y desarrollan apps de investigación en colaboración, con herramientas interoperables con las que están familiarizados.
* Los microservicios reducen significativamente el tiempo de entrega de parches, correcciones de errores y funciones nuevas. El desarrollo inicial es rápido y se ofrecen actualizaciones frecuentes.
* Los investigadores tienen acceso a datos clínicos y pueden compartir datos clínicos, a la vez que cumplen con las normativas locales.
* Los pacientes que participan en la investigación de enfermedades se sienten seguros de que sus datos están protegidos, lo que marca una gran diferencia cuando los datos se comparten con grandes equipos de investigación.
