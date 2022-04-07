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


# Casos de uso de {{site.data.keyword.Bluemix_notm}} en el entorno gubernamental
{: #cs_uc_gov}

En estos casos de uso se describe cómo las cargas de trabajo de {{site.data.keyword.containerlong_notm}} se benefician de la nube pública. Estas cargas de trabajo tienen aislamiento con Trusted Compute, se encuentran en regiones globales para la soberanía de datos, utilizan machine learning de Watson en lugar del código net-new y se conectan a bases de datos locales.
{: shortdesc}

## El gobierno regional mejora la colaboración y la velocidad de desarrolladores de la comunidad que combinan datos públicos y privados
{: #uc_data_mashup}

Un ejecutivo de un programa de datos de gobierno abierto necesita compartir datos públicos con la comunidad y con el sector privado, pero los datos están bloqueados en un sistema monolítico local.
{: shortdesc}

Por qué {{site.data.keyword.Bluemix_notm}}: con {{site.data.keyword.containerlong_notm}}, el ejecutivo proporciona el valor transformador de los datos públicos y privados combinados. Paralelamente, el servicio ofrece una plataforma de nube pública para refactorizar y exponer los microservicios de las apps locales monolíticas. Además, la nube pública permite a los gobiernos y a las asociaciones públicas utilizar servicios de nube externos y herramientas de código abierto que facilitan la colaboración.

Tecnologías clave:    
* [Clústeres que se adaptan a las diversas necesidades de CPU, RAM y almacenamiento](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Herramientas nativas de DevOps, que incluyen cadenas de herramientas abiertas en {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [Se proporciona acceso a datos públicos con {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about)
* [Servicios IBM Cloud Analytics de tipo conectar y usar](https://www.ibm.com/cloud/analytics)

**Contexto: el gobierno mejora la colaboración y la velocidad con los desarrolladores de la comunidad que combinan datos públicos y privados**
* Un modelo de "gobierno abierto" es el futuro, pero esta agencia gubernamental regional no puede dar el salto con sus sistemas locales.
* Quieren apoyar la innovación y fomentar el desarrollo en colaboración entre el sector privado, los ciudadanos y las agencias públicas.
* Grupos diversos de desarrolladores de organizaciones gubernamentales y privadas no tienen una plataforma unificada de código abierto donde puedan compartir fácilmente API y datos.
* Los datos gubernamentales están bloqueados en sistemas locales sin fácil acceso público.

**La solución**

Se debe crear una transformación de gobierno abierto en una base que proporcione rendimiento, resistencia, continuidad de negocio y seguridad. A medida que avanzan la innovación y el desarrollo en cooperación, las agencias y los ciudadanos dependen del software, los servicios y las compañías de la infraestructura para "proteger y dar servicio".

Para reducir la burocracia y transformar la relación del gobierno con su electorado, han adoptado estándares abiertos para crear una plataforma para la colaboración:

* DATOS ABIERTOS - almacén de datos en el que ciudadanos, agencias gubernamentales y empresas acceden, comparten y mejoran datos libremente
* API ABIERTAS – una plataforma de desarrollo en la que todos los participantes de la comunidad contribuyen y reutilizan las API
* INNOVACIÓN ABIERTA - un conjunto de servicios en la nube que permiten a los desarrolladores incorporar innovación en lugar de tenerla que codificar manualmente

Para empezar, el gobierno utiliza {{site.data.keyword.cos_full_notm}} para almacenar sus datos públicos en la nube. Este almacén es de uso y reutilización libres, lo puede compartir cualquiera y solo está sujeto a atribuciones y comparticiones similares. Los datos confidenciales se limpian antes de que se coloquen en la nube. Además, se configuran controles de acceso de forma que la nube limite el nuevo almacén de datos, en el que la comunidad puede mostrar pruebas de concepto de mejora de los datos existentes.

El siguiente paso del gobierno en lo referente a la combinación de datos públicos y privados consiste en establecer una economía de API alojada en {{site.data.keyword.apiconnect_long}}. Allí la comunidad y los desarrolladores de las empresas facilitar el acceso a los datos en forma de API. Sus objetivos son disponer de API REST públicas para permitir la interoperatividad y para acelerar la integración de apps. Utilizan IBM {{site.data.keyword.SecureGateway}} para volver a conectar con fuentes de datos privadas locales.

Por último, las apps basadas en estas API compartidas se alojan en {{site.data.keyword.containerlong_notm}}, donde resulta fácil utilizar clústeres. Con este entorno, desarrolladores de la comunidad, del sector privado y del gobierno pueden crear fácilmente apps en colaboración. En resumen, los desarrolladores tienen que centrarse en la codificación en lugar de en gestionar la infraestructura. Por lo tanto, eligieron {{site.data.keyword.containerlong_notm}} porque IBM simplifica la gestión de la infraestructura:
* Gestión de nodo maestro de Kubernetes, IaaS y componentes operativos, como Ingress y almacenamiento
* Supervisión del estado y la recuperación de los nodos trabajadores
* Suministro de cálculo global, para que los desarrolladores no tengan que hacer frente a la infraestructura de las regiones de todo el mundo donde necesitan que se encuentren las cargas de trabajo y los datos

Sin embargo, el traslado de las cargas de trabajo de cálculo a {{site.data.keyword.Bluemix_notm}} no es suficiente. El gobierno también debe transformar métodos y procesos. Mediante la adopción de las prácticas del método IBM Garage Method, el proveedor puede implementar un proceso de entrega ágil e iterativo que dé soporte a las prácticas modernas de DevOps, como Continuous Integration and Delivery (CI/CD).

Gran parte del propio proceso de CI/CD se automatiza con {{site.data.keyword.contdelivery_full}} en la nube. El proveedor puede definir cadenas de herramientas de flujos de trabajo para preparar imágenes de contenedor, comprobar si hay vulnerabilidades y desplegarlas en el clúster de Kubernetes.

**Modelo de solución**

Las herramientas de cálculo según demanda, almacenamiento y API se ejecutan en la nube pública con acceso seguro a y desde fuentes de datos locales.

Solución técnica:
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cos_full_notm}} y {{site.data.keyword.cloudant}}
* {{site.data.keyword.apiconnect_long}}
* IBM {{site.data.keyword.SecureGateway}}
* {{site.data.keyword.contdelivery_full}}

**Paso 1: Almacenar datos en la nube**
* {{site.data.keyword.cos_full_notm}} proporciona almacenamiento de datos históricos, accesibles para todos en la nube pública.
* Utilizar {{site.data.keyword.cloudant}} con claves proporcionadas por el desarrollador para almacenar en la memoria caché los datos de la nube.
* Utilizar IBM {{site.data.keyword.SecureGateway}} para mantener conexiones seguras con las bases de datos locales existentes.

**Paso 2: Proporcionar acceso a los datos con API**
* Utilizar {{site.data.keyword.apiconnect_long}} para la plataforma de economía de API. Las API permiten a los sectores público y privado combinar datos en sus apps.
* Crear clústeres para apps públicas y privadas, controladas por las API.
* Estructurar las apps en un conjunto de microservicios cooperativos que se ejecuten en {{site.data.keyword.containerlong_notm}}, que se basa en áreas funcionales de apps y sus dependencias.
* Desplegar las apps en contenedores que se ejecutan en {{site.data.keyword.containerlong_notm}}. Las herramientas de alta disponibilidad integradas en {{site.data.keyword.containerlong_notm}} equilibran las cargas de trabajo e incluyen reparación automática y equilibrio de carga.
* Proporcionar paneles de control de DevOps estandarizados a través de Kubernetes y herramientas de código abierto con las que estén familiarizados todos los tipos de desarrolladores.

**Paso 3: Innovar con IBM Garage y los servicios de nube**
* Adoptar las prácticas de desarrollo ágiles e iterativas de IBM Garage Method para permitir releases frecuentes de características, parches y arreglos sin ocasionar tiempos de inactividad.
* Tanto si los desarrolladores pertenecen al sector público como si son del sector privado, {{site.data.keyword.contdelivery_full}} les ayuda a suministrar rápidamente una cadena de herramientas integrada utilizando plantillas que se pueden personalizar y compartir.
* Después de que los desarrolladores creen y prueben las apps en sus clústeres de desarrollo y prueba, utilizan las cadenas de herramientas de {{site.data.keyword.contdelivery_full}} para desplegar las apps en clústeres de producción.
* Con Watson AI, machine learning y las herramientas de deep learning disponibles en el catálogo de {{site.data.keyword.Bluemix_notm}}, los desarrolladores se centran en los problemas del dominio. En lugar de un código ML exclusivo personalizado, se incorpora a las apps lógica ML con enlaces a servicios.

**Resultados**
* La coordinación entre el sector público y privado, que suele ser un proceso lento, permite el desarrollo de apps en semanas en lugar de en meses. Estas asociaciones de desarrollo ahora ofrecen funciones y correcciones de errores hasta 10 veces por semana.
* El desarrollo se acelera cuando todos los participantes utilizan herramientas de código abierto conocidas, como por ejemplo Kubernetes. Las curvas de aprendizaje largas ya no constituyen un problema.
* Se ofrece a los ciudadanos y al sector privado transparencia en cuanto a actividades, información y planes. Además, se integra a los ciudadanos en los procesos gubernamentales, los servicios y el apoyo.
* La asociación público-privado conquistan tareas difíciles, como el seguimiento del virus del Zika, la distribución de electricidad inteligente, el análisis de estadísticas sobre delitos y las nuevas tendencias en formación universitaria.

## Un gran puerto público protege el intercambio de datos y los manifiestos de transporte marítimo entre organizaciones públicas y privadas
{: #uc_port}

Los ejecutivos de TI de una compañía naviera privada y el puerto, cuyo funcionamiento controla el gobierno, tienen que conectar, proporcionar visibilidad e intercambiar de forma segura información portuaria. Pero no existía ningún sistema unificado para conectar la información del puerto público y los manifiestos de transporte marítimo privados.
{: shortdesc}

Por qué {{site.data.keyword.Bluemix_notm}}: {{site.data.keyword.containerlong_notm}} permite al gobierno y a las asociaciones públicas utilizar servicios de nube externos y herramientas de código abierto que facilitan la colaboración. Los contenedores proporcionaron una plataforma compartible en la que tanto el puerto como la compañía naviera confiaban y sabían que la información compartida estaba alojada en una plataforma segura. Y dicha plataforma se puede escalar cuando se pasa de pequeños sistemas de desarrollo y prueba a sistemas de producción. Las cadenas de herramientas abiertas también aceleraron el desarrollo mediante la automatización de los procesos de creación, prueba y despliegue.

Tecnologías clave:    
* [Clústeres que se adaptan a las diversas necesidades de CPU, RAM y almacenamiento](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Seguridad y aislamiento de contenedores](/docs/containers?topic=containers-security#security)
* [Herramientas nativas de DevOps, que incluyen cadenas de herramientas abiertas en {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**Contexto: el puerto protege el intercambio de datos portuarios y de manifiestos de transporte marítimo entre organizaciones públicas y privadas.**

* Grupos diversos de desarrolladores de organizaciones gubernamentales y de la compañía naviera no tienen una plataforma de unificada en la que puedan colaborar, lo que ralentiza los despliegues de actualizaciones y de características.
* Los desarrolladores están repartidos por todo el mundo y en distintas organizaciones, lo que significa que el código abierto y PaaS constituyen la mejor opción.
* La seguridad es una preocupación primordial, y esto aumenta la carga de colaboración que afecta a las características y actualizaciones del software, especialmente después de que las apps se encuentren en producción.
* Los datos de última hora significan que los sistemas de todo el mundo deben estar muy disponibles para reducir los retrasos en las operaciones de tránsito. Los horarios de las terminales del puerto están muy controlados y en algunos casos son inflexibles. El uso de la web está creciendo, por lo que la inestabilidad podría dar lugar a una experiencia deficiente para el usuario.

**La solución**

El puerto y la compañía naviera desarrollan conjuntamente un sistema de comercio unificado para poder presentar electrónicamente la información relacionada con el cumplimiento para la autorización de mercancías y barcos una sola vez, en lugar tenerlo que presentar a varias agencias. Las apps de manifiestos y aduanas pueden compartir rápidamente información sobre un barco en concreto y asegurarse de que todos los documentos se transfieran y se procesen electrónicamente por agencias del puerto.

Por lo tanto, crean una asociación dedicada a soluciones para el sistema de comercio:
* DECLARACIONES - App que recibe manifiestos de transporte marítimo y procesa digitalmente el papeleo aduanero típico y detecta los artículos que no cumplen con las políticas para que se investiguen
* TARIFAS - App para calcular tarifas, enviar los cargos electrónicamente al expedidor y recibir pagos digitales
* REGULACIONES - App flexible y configurable que suministra datos a las dos apps anteriores sobre políticas y regulaciones en cambio constante que afectan a importaciones, exportaciones y proceso de tarifas

Los desarrolladores comenzaron por desplegar sus apps en contenedores con {{site.data.keyword.containerlong_notm}}. Crearon clústeres para un entorno de desarrollo compartido que permiten a los desarrolladores de todo el mundo desplegar de forma colaborativa mejoras de apps rápidamente. Los contenedores permiten que cada equipo de desarrollo utilice el lenguaje que desee.

La seguridad es lo primero: el ejecutivo de TI eligió Trusted Compute para el servidor nativo donde alojar los clústeres. Con el entorno nativo para {{site.data.keyword.containerlong_notm}}, las cargas de trabajo con datos confidenciales ahora tienen un aislamiento familiar, pero dentro de la flexibilidad de la nube pública. El entorno nativo proporciona Trusted Compute, que verifica el hardware subyacente para protegerlo frente a una posible manipulación.

Debido a que la compañía naviera también quiere trabajar con otros puertos, la seguridad de las apps resulta crucial. Los manifiestos de transporte marítimo la información aduanera son altamente confidenciales. Desde el punto de vista de la seguridad, Vulnerability Advisor ofrece los siguientes escaneos:
* Escaneos de vulnerabilidad de imágenes
* Escaneos de políticas basados en ISO 27k
* Escaneos de contenedores
* Escaneos de paquetes en busca de malware conocido

Paralelamente, {{site.data.keyword.iamlong}} ayuda a controlar quién tiene nivel de acceso a los recursos.

Los desarrolladores se centran en los problemas del dominio, utilizando las herramientas existentes: los desarrolladores, en lugar de escribir código de registro y supervisión exclusivo, incorporan apps enlazando los servicios de {{site.data.keyword.Bluemix_notm}} a los clústeres. Los desarrolladores también se liberan de las tareas de gestión de la infraestructura, ya que IBM se ocupa de las actualizaciones de Kubernetes y de la infraestructura, de la seguridad y de más.

**Modelo de solución**

Cálculo según demanda, almacenamiento y kits de inicio de nodos que se ejecutan en la nube pública con acceso seguro a los datos de transporte marítimo en todo el mundo, según sea necesario. El cálculo de los clústeres es a prueba de manipulaciones y está aislado en entornos nativos.  

Solución técnica:
* {{site.data.keyword.containerlong_notm}} con Trusted Compute
* {{site.data.keyword.openwhisk}}
* {{site.data.keyword.cloudant}}
* IBM {{site.data.keyword.SecureGateway}}

**Paso 1: Contenerizar apps, mediante microservicios**
* Utilizar el kit de inicio Node.js de IBM para agilizar el desarrollo.
* Estructurar las apps en un conjunto de microservicios cooperativos que se ejecuten en {{site.data.keyword.containerlong_notm}} en función de las áreas funcionales de la app y sus dependencias.
* Despliegue las apps de manifiesto y de envío en el contenedor que se ejecuta en {{site.data.keyword.containerlong_notm}}.
* Proporcionar paneles de control de DevOps estandarizados a través de Kubernetes.
* Utilizar IBM {{site.data.keyword.SecureGateway}} para mantener conexiones seguras con las bases de datos locales existentes.

**Paso 2: Garantizar la disponibilidad global**
* Después de que los desarrolladores desplieguen las apps en sus clústeres de desarrollo y prueba, utilizan las cadenas de herramientas de {{site.data.keyword.contdelivery_full}} y Helm para desplegar las apps específicas de cada país en clústeres de todo el mundo.
* Las cargas de trabajo y los datos se ajustan a las normativas regionales.
* Las herramientas de alta disponibilidad integradas en {{site.data.keyword.containerlong_notm}} equilibran la carga de trabajo dentro de cada región geográfica e incluyen reparación automática y equilibrio de carga.

**Paso 3: Compartir datos**
* {{site.data.keyword.cloudant}} es una moderna base de datos NoSQL que se adapta a muchos casos de uso controlados por datos: desde el par clave-valor hasta el almacenamiento y consulta complejos de datos orientados a documentos.
* Para minimizar las consultas a las bases de datos regionales, se utiliza {{site.data.keyword.cloudant}} para almacenar en memoria caché los datos de sesión de usuario entre las apps.
* Esta configuración mejora el rendimiento y la usabilidad para el usuario final entre las apps de {{site.data.keyword.containershort}}.
* Mientras las apps de {{site.data.keyword.containerlong_notm}} analizan los datos locales y guardan los resultados en {{site.data.keyword.cloudant}}, {{site.data.keyword.openwhisk}} reacciona ante cambios y limpia automáticamente los datos de entrada.
* Paralelamente, se pueden activar notificaciones de envíos en una región mediante cargas de datos para que los consumidores puedan acceder a los datos actualizados.

**Resultados**
* Con kits de inicio de IBM, {{site.data.keyword.containerlong_notm}} y las herramientas de {{site.data.keyword.contdelivery_full}}, los desarrolladores globales establecen asociaciones entre organizaciones y gobiernos. Desarrollan de forma colaborativa apps de aduanas, con herramientas familiares e interoperables.
* Los microservicios reducen significativamente el tiempo de entrega de parches, correcciones de errores y funciones nuevas. El desarrollo inicial es rápido y se ofrecen actualizaciones frecuentes (hasta 10 veces por semana).
* Los clientes de transporte marítimo y los funcionarios del gobierno tienen acceso a datos de manifiesto y pueden compartir datos de aduanas, a la vez que cumplen con las regulaciones locales.
* La compañía naviera se beneficia de una mejor gestión de la logística en la cadena de suministro: reducción de costes y autorizaciones rápidas.
* El 99% son declaraciones digitales y el 90% de las importaciones se procesan sin intervención humana.
