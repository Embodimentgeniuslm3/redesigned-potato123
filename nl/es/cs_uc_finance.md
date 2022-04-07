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


# Casos de uso de {{site.data.keyword.cloud_notm}} en el sector de servicios financieros
{: #cs_uc_finance}

En estos casos de uso se muestra cómo las cargas de trabajo de {{site.data.keyword.containerlong_notm}} pueden aprovechar la alta disponibilidad, el cálculo de alto rendimiento, la agilización de los clústeres para facilitar su desarrollo y la IA de {{site.data.keyword.ibmwatson}}.
{: shortdesc}

## Una empresa hipotecaria recorta los costes y acelera la conformidad normativa
{: #uc_mortgage}

El responsable de gestión de riesgos de una empresa hipotecaria procesa 70 millones de registros al día, pero el sistema local era lento y también inexacto. Los gastos de TI se dispararon porque el hardware se quedó obsoleto y no se sacaba el máximo provecho del mismo. Mientras esperaban el suministro de hardware, el cumplimiento con las normativas se ralentizó.
{: shortdesc}

Por qué {{site.data.keyword.Bluemix_notm}}: para mejorar el análisis de riesgos, la empresa examinó los servicios {{site.data.keyword.containerlong_notm}} e IBM Cloud Analytic para reducir costes, para aumentar la disponibilidad a nivel mundial y, en última instancia, para acelerar el cumplimiento de normativas. Con {{site.data.keyword.containerlong_notm}} en varias regiones, sus apps de análisis se pueden contenerizar y desplegar en todo el mundo, mejorando la disponibilidad y atendiendo las regulaciones locales. Estos despliegues se aceleran con herramientas de código abierto ampliamente conocidas, que ya forman parte de {{site.data.keyword.containerlong_notm}}.

{{site.data.keyword.containerlong_notm}} y tecnologías clave:
* [Escalado horizontal](/docs/containers?topic=containers-app#highly_available_apps)
* [Varias regiones para alta disponibilidad](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [Clústeres que se adaptan a las diversas necesidades de CPU, RAM y almacenamiento](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Seguridad y aislamiento de contenedores](/docs/containers?topic=containers-security#security)
* [{{site.data.keyword.cloudant}} para almacenar datos de forma permanente y sincronizarlos entre apps](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**La solución**

Empezaron por contenerizar las apps de análisis y colocarlas en la nube. De inmediato, sus problemas de hardware desaparecieron. También diseñaron fácilmente clústeres de Kubernetes que se ajustaban a sus requisitos de alto rendimiento, CPU, RAM, almacenamiento y seguridad. Y, cuando sus apps de análisis cambian, pueden añadir o reducir la capacidad de cálculo sin grandes inversiones en hardware. Con el escalado horizontal de {{site.data.keyword.containerlong_notm}}, sus apps se adaptan el número creciente de registros, lo que les permite confeccionar informes de normativas con mayor rapidez. {{site.data.keyword.containerlong_notm}} proporciona recursos de cálculo elásticos en todo el mundo que son seguros y de alto rendimiento para aprovechar los modernos recursos de cálculo.

Ahora esas apps reciben grandes volúmenes de datos de un almacén de datos en {{site.data.keyword.cloudant}}. El almacenamiento basado en nube de {{site.data.keyword.cloudant}} garantiza una mayor disponibilidad que cuando estaba limitado a un sistema local. Dado que la disponibilidad resulta esencial, las apps se despliegan en los centros de datos globales: también para DR y para latencia.

También han acelerado el proceso de conformidad y de análisis de riesgos. Sus funciones de análisis predictivo y de riesgo, al igual que los cálculos de Monte Carlo, se actualizan constantemente mediante despliegues ágiles e iterativos. La organización de contenedores se gestiona mediante Kubernetes para que también se reduzcan los costes de las operaciones. Por último, el análisis de los riesgos de las hipotecas responde mejor a los rápidos cambios que se producen en el mercado.

**Contexto: modelos de conformidad y financiero para hipotecas residenciales**

* La necesidad de mejorar la gestión de los riesgos financieros aumenta los requisitos de supervisión de normativas. Las mismas necesidades impulsan la revisión de los procesos de evaluación de riesgos y la divulgación de informes normativos más detallados, integrados y completos.
* Las cuadrículas de cálculo de alto rendimiento constituyen los componentes clave de la infraestructura para el modelado financiero.

El problema de la empresa en este momento es el escalado y el tiempo de entrega.

Su entorno actual tiene más 7 años de antigüedad, es local y tiene una capacidad limitada de cálculo, de almacenamiento y de E/S.
Las renovaciones del servidor resultan caras y taren bastante tiempo en completarse.
Las actualizaciones de software y de apps siguen un proceso informal y no son repetibles.
La cuadrícula de HPC actual es difícil de programar. La API es demasiado compleja para los nuevos desarrolladores de la compañía y requieren conocimientos que no están documentados.
Y las actualizaciones importantes de la app tardan entre 6 y 9 meses en implementarse.

**Modelo de solución: servicios de cálculo según demanda, almacenamiento y E/S que se ejecutan en la nube pública con acceso seguro a los activos locales de la empresa cuando sea necesario**

* Almacenamiento de documentos seguro y escalable que da soporte a la consulta de documentos estructurados y no estructurados
* Movimiento de activos y de apps existentes de la empresa entre entornos, al tiempo que permiten la integración en los sistemas locales que no se van a migrar
* Reducción del tiempo de despliegue de soluciones e implementación de procesos estándares de DevOps y de supervisión para solucionar los problemas que afectan a la precisión de los informes

**Solución detallada**

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cos_full_notm}}
* {{site.data.keyword.sqlquery_notm}} (Spark)
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGateway}}

{{site.data.keyword.containerlong_notm}} proporciona recursos de cálculo escalables y los paneles de control de DevOps asociados para crear, escalar y eliminar apps y servicios según demanda. Mediante el uso de contenedores estándares del sector, las apps se pueden realojar inicialmente en {{site.data.keyword.containerlong_notm}} de forma rápida sin tener que realizar cambios arquitectónicos importantes.

Esta solución proporciona la ventaja inmediata de la escalabilidad. Gracias al completo conjunto de objetos de despliegue y de tiempo de ejecución de Kubernetes, la empresa hipotecaria supervisa y gestiona las actualizaciones de las apps de forma fiable. También pueden duplicar y escalar las apps mediante el uso de reglas definidas y el orquestador automático de Kubernetes.

Se utiliza {{site.data.keyword.SecureGateway}} para crear una interconexión segura con bases de datos y documentos locales para apps realojadas para que se ejecuten en {{site.data.keyword.containerlong_notm}}.

{{site.data.keyword.cos_full_notm}} sirve tanto para documentos sin formato como para el almacenamiento de datos. Para las simulaciones de Monte Carlo, se utiliza un conducto de flujo de trabajo en el que los datos de simulación están en archivos estructurados que se almacenan en {{site.data.keyword.cos_full_notm}}. Un desencadenante para iniciar los servicios de cálculo escala los servicios de simulación de {{site.data.keyword.containerlong_notm}} para dividir los datos de los archivos en N grupos de sucesos para el proceso de simulación. {{site.data.keyword.containerlong_notm}} escala automáticamente a N ejecuciones de servicios asociados y escribe los resultados intermedios en {{site.data.keyword.cos_full_notm}}. Estos resultados se procesan mediante otro conjunto de servicios de cálculo de {{site.data.keyword.containerlong_notm}} para generar los resultados finales.

{{site.data.keyword.cloudant}} es una moderna base de datos NoSQL que resulta útil para muchos casos de uso controlados por datos: desde el par clave-valor hasta el almacenamiento y consulta complejos de datos orientados a documentos. Para gestionar el conjunto cada vez mayor de normas de informes de gestión y regulatorias, la empresa hipotecaria utiliza {{site.data.keyword.cloudant}} para almacenar los documentos asociados con los datos de regulación sin formato que llegan a la empresa. Los procesos de cálculo de {{site.data.keyword.containerlong_notm}} se desencadenan para compilar, procesar y publicar los datos en diversos formatos de informes. Los resultados intermedios comunes a los informes se almacenan como documentos de {{site.data.keyword.cloudant}} de modo que se puedan utilizar procesos controlados por plantillas para generar los informes necesarios.

**Resultados**

* Las simulaciones financieras complejas se completaron en un 25% del tiempo que tardaba antes con los sistemas locales existentes.
* El tiempo de despliegue ha pasado de los 6-9 meses anteriores a 1-3 semanas de promedio. Esta mejora se produce porque {{site.data.keyword.containerlong_notm}} habilita un proceso controlado y disciplinado para aumentar los contenedores de apps y sustituirlos por versiones más recientes. Los errores de informes se pueden solucionar rápidamente, abordando problemas como, por ejemplo, la precisión.
* Los costes de informes reglamentarios se han reducido con un conjunto coherente y escalable de servicios de almacenamiento y de cálculo que incorporan {{site.data.keyword.containerlong_notm}} y {{site.data.keyword.cloudant}}.
* Con el tiempo, las apps originales que inicialmente se transferían a la nube se han reorganizado en microservicios cooperativos que se ejecutan en {{site.data.keyword.containerlong_notm}}. Esta acción ha acelerado aún más el desarrollo y el tiempo de despliegue y ha facilitado la innovación gracias a su relativa facilidad de experimentación. También publicaron apps innovadoras con nuevas versiones de microservicios para aprovechar las condiciones del mercado y del negocio (es decir, las llamadas apps situacionales y microservicios).

## Una empresa tecnológica de pagos agiliza la productividad del desarrollador, desplegando herramientas habilitadas para IA para sus socios 4 veces más rápido
{: #uc_payment_tech}

Un ejecutivo de desarrollo tiene desarrolladores que utilizan herramientas locales tradicionales que ralentizan la creación de prototipos mientras esperan la adquisición de hardware.
{: shortdesc}

Por qué {{site.data.keyword.Bluemix_notm}}: {{site.data.keyword.containerlong_notm}} facilita el uso de la capacidad de cálculo mediante el uso de tecnología estándar de código abierto. Después de que la empresa realizar la transición a {{site.data.keyword.containerlong_notm}}, los desarrolladores tienen acceso a herramientas que facilitan DevOps, como por ejemplo contenedores portátiles y de fácil compartición.

Los desarrolladores pueden experimentar fácilmente, impulsando cambios en los sistemas de desarrollo y prueba rápidamente con las cadenas de herramientas abiertas. Sus herramientas tradicionales de desarrollo de software adquieren un nuevo aspecto cuando agregan servicios de nube de IA a apps con una simple pulsación.

Tecnologías clave:
* [Clústeres que se adaptan a las diversas necesidades de CPU, RAM y almacenamiento](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Prevención de fraudes con {{site.data.keyword.watson}} AI](https://www.ibm.com/cloud/watson-studio)
* [Herramientas nativas de DevOps, que incluyen cadenas de herramientas abiertas en {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)
* [Capacidad de inicio de sesión sin cambiar el código de la app mediante {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-getting-started)

**Contexto: agilización de la productividad del desarrollador y despliegue de herramientas de IA en asociados 4 veces más rápido**

* La disrupción va en aumento en la industria de pagos, que también ha experimentado un crecimiento excepcional tanto en el segmento del cliente como en el de empresa a empresa. Sin embargo, las actualizaciones se las herramientas de pago han sido lentas.
* Se necesitan características cognitivas para hacer frente a las transacciones fraudulentas de formas nuevas y rápidas.
* Con el número creciente de socios y de transacciones, el tráfico de herramientas aumenta, pero el presupuesto de la infraestructura tiene se que reducir, mediante la maximización de la eficiencia de los recursos.
* Su deuda técnica está creciendo, en lugar de reducirse, por la incapacidad para sacar al mercado software de calidad para mantenerse al día con las demandas del mercado.
* Los presupuestos de gastos de capital están muy restringidos, y el personal de TI tiene la sensación de no disponer de presupuesto o de personal para crear entornos de prueba con sus sistemas locales.
* La seguridad es cada vez más importante, y esta preocupación solo se suma a la carga que supone la distribución, que causa aún más retrasos.

**La solución**

El ejecutivo de desarrollo se enfrenta a muchos retos en el sector dinámico de pagos. La normativa, las conductas de los consumidores, los fraudes, los competidores y las infraestructuras de mercado están evolucionando rápidamente. El desarrollo a ritmo rápido resulta esencial para formar parte del futuro mundo del sector de pagos.

Su modelo de negocio consiste en proporcionar herramientas de pago a los asociados de la empresa, para que puedan ayudar a estas instituciones financieras y a otras organizaciones a ofrecer experiencias de pago digital muy seguras.

Necesitan una solución que ayude a los desarrolladores y a sus asociados comerciales:
* INTERFAZ FRENTE A LAS HERRAMIENTAS DE PAGO: sistemas de cuotas, seguimiento de pagos, incluso entre fronteras, cumplimiento de normativas, biometría, giros y más
* CARACTERÍSTICAS ESPECÍFICAS DE LA NORMATIVA: cada país tiene una normativa exclusiva, de modo que el conjunto de herramientas general puede parecer similar, pero debe mostrar ventajas específicas de cada país
* HERRAMIENTAS DEL DESARROLLADOR que aceleran el despliegue de características y la solución de problemas
* DETECCIÓN DE FRAUDES COMO SERVICIO (FDaaS) utiliza la {{site.data.keyword.watson}} AI para mantenerse un paso por delante de las acciones fraudulentas frecuentes y en aumento

**Modelo de solución**

Herramientas DevOps de cálculo bajo demanda e IA que se ejecutan en la nube pública con acceso a los sistemas de pago de fondo. Implemente un proceso CI/CD para acortar significativamente los ciclos de entrega.

Solución técnica:
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.contdelivery_full}}
* IBM Cloud Logging and Monitoring
* {{site.data.keyword.ibmwatson_notm}} para servicios financieros
* {{site.data.keyword.appid_full_notm}}

Empezaron por contenerizar las VM de las herramientas de pago y colocarlas en la nube. De inmediato, sus problemas de hardware desaparecieron. También diseñaron fácilmente clústeres de Kubernetes que se ajustaban a sus requisitos de CPU, RAM, almacenamiento y seguridad. Y, cuando sus herramientas de pagos requieren un cambio, pueden añadir o reducir la capacidad de cálculo sin grandes adquisiciones de hardware, que resultan lentas y caras.

Con el escalado horizontal de {{site.data.keyword.containerlong_notm}}, sus apps se adaptan el número creciente de asociados, lo que les crecer rápidamente. {{site.data.keyword.containerlong_notm}} proporciona recursos de cálculo elásticos en todo el mundo que son seguros para aprovechar los modernos recursos de cálculo.

El desarrollo acelerado constituye una victoria clave para el ejecutivo. Con el uso de contenedores modernos, los desarrolladores pueden experimentar fácilmente en los idiomas que elijan, enviando los cambios a los sistemas de desarrollo y prueba, escalados en clústeres separados. Esos envíos se han automatizado con cadenas de herramientas abiertas y {{site.data.keyword.contdelivery_full}}. Las actualizaciones de las herramientas ya no pasan por procesos lentos y propensos a errores. Pueden distribuir actualizaciones incrementales a sus herramientas, a diario o incluso con mayor frecuencia.

Además, el registro y la supervisión de las herramientas, especialmente si utilizan {{site.data.keyword.watson}} AI, se integran rápidamente en el sistema. Los desarrolladores no pierdan el tiempo creando sistemas de registro complejos, solo para poder solucionar los problemas de los sistemas activos. Un factor clave para reducir los costes de personal es que IBM gestiona Kubernetes, por lo que los desarrolladores pueden centrarse en crear mejores herramientas de pago.

La seguridad es lo primero: con el entorno nativo para {{site.data.keyword.containerlong_notm}}, las herramientas de pago sensibles ahora tienen un aislamiento familiar, pero dentro de la flexibilidad de la nube pública. El entorno nativo proporciona Trusted Compute, que verifica el hardware subyacente para protegerlo frente a una posible manipulación. Constantemente se ejecutan exploraciones en busca de vulnerabilidades y programas maliciosos.

**Paso 1: Adopción de un sistema de cálculo seguro**
* Las apps que gestionan datos altamente confidenciales se pueden realojar en {{site.data.keyword.containerlong_notm}} que se ejecuta en un entorno nativo para Trusted Compute. Trusted Compute puede verificar el hardware subyacente para protegerlo frente a una posible manipulación.
* Migre las imágenes de máquina virtual a imágenes de contenedor que se ejecutan en {{site.data.keyword.containerlong_notm}} en {{site.data.keyword.Bluemix_notm}} público.
* Desde ese punto de partida, Vulnerability Advisor proporciona funciones de exploración de vulnerabilidades de imágenes, políticas, contenedores y paquetes en busca de software malicioso conocido.
* Los costes del centro de datos privado y de capital local se reducen enormemente y se sustituyen por un modelo de cálculo de utilidad que se basa en la demanda de la carga de trabajo.
* Aplique de forma coherente la autenticación controlada por políticas a los servicios y las API con una anotación de Ingress simple. Con la seguridad declarativa, puede garantizar la autenticación de usuarios y la validación de señales mediante {{site.data.keyword.appid_short_notm}}.

**Paso 2: Operaciones y conexiones con sistemas de fondo de pago existentes**
* Utilice IBM {{site.data.keyword.SecureGateway}} para mantener conexiones seguras con sistemas de herramientas locales.
* Proporcionar paneles de control de DevOps estandarizados y prácticas a través de Kubernetes.
* Después de que los desarrolladores creen y prueben las apps en sus clústeres de desarrollo y prueba, utilizan las cadenas de herramientas de {{site.data.keyword.contdelivery_full}} para desplegar las apps en clústeres de {{site.data.keyword.containerlong_notm}} en todo el mundo.
* Las herramientas de alta disponibilidad integradas en {{site.data.keyword.containerlong_notm}} equilibran la carga de trabajo dentro de cada región geográfica e incluyen reparación automática y equilibrio de carga.

**Paso 3: Analizar y evitar fraudes**
* Despliegue IBM {{site.data.keyword.watson}} para servicios financieros para prevenir y detectar fraudes.
* Mediante el uso de herramientas de despliegue de Helm y de cadenas de herramientas, las apps también se despliegan en clústeres de {{site.data.keyword.containerlong_notm}} en todo el mundo. Luego las cargas de trabajo y los datos cumplen las normativas regionales.

**Resultados**
* El traslado de las VM monolíticas existentes a contenedores alojados en la nube fue un primer paso que permitió al ejecutivo de desarrollo ahorrar en costes de capital y operaciones.
* Con la gestión de la infraestructura en manos de IBM, el equipo de desarrollo dispuso del tiempo necesario para ofrecer actualizaciones 10 veces al día.
* Paralelamente, el proveedor implementó iteraciones sencillas para controlar la deuda técnica existente.
* Con el número de transacciones que procesan, pueden escalar sus operaciones de forma exponencial.
* Al mismo tiempo, un nuevo análisis de fraude con {{site.data.keyword.watson}} ha aumentado la velocidad del proceso de detección y prevención, reduciendo el fraude 4 veces más que la media de la región.
