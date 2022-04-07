---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-03"

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


# Definición de la estrategia de Kubernetes
{: #strategy}

Con {{site.data.keyword.containerlong}}, puede desplegar cargas de trabajo de contenedor de forma rápida y segura para las apps en producción. Obtenga más información de forma que cuando planifique la estrategia para su clúster, pueda optimizar la configuración para aprovechar al máximo las capacidades de despliegue automatizado, escalado y gestión de orquestación de [Kubernetes![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/).
{:shortdesc}

## Traspasar las cargas de trabajo a {{site.data.keyword.Bluemix_notm}}
{: #cloud_workloads}

Tiene muchos motivos para traspasar las cargas de trabajo a {{site.data.keyword.Bluemix_notm}}: reducir el coste total de la propiedad, aumentar la alta disponibilidad de las apps en un entorno seguro y compatible, escalar hacia arriba y hacia abajo en respuesta a la demanda del usuario, y muchos más. {{site.data.keyword.containerlong_notm}} combina la tecnología de contenedor con herramientas de código abierto como, por ejemplo, Kubernetes de forma que puede crear una app nativa de nube que pueda migrar a través de distintos entornos de nube, evitando la obligación de permanencia con un proveedor.
{:shortdesc}

¿Pero cómo se accede a la nube? ¿Qué opciones hay? Y ¿cómo se gestionan las cargas de trabajo una vez se ha accedido?

Utilice esta página para obtener más información sobre las estrategias para los despliegues de Kubernetes en {{site.data.keyword.containerlong_notm}}. Y no dude en ponerse en contacto con nuestro equipo en [Slack. ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-container-service.slack.com)

¿Todavía no está en slack? [Solicite una invitación](https://bxcs-slack-invite.mybluemix.net/)
{: tip}

### ¿Qué puedo traspasar a {{site.data.keyword.Bluemix_notm}}?
{: #move_to_cloud}

Con {{site.data.keyword.Bluemix_notm}}, tiene flexibilidad para crear clústeres de Kubernetes en [entornos de nube fuera de local, locales o de nube híbridos](/docs/containers?topic=containers-cs_ov#differentiation). En la tabla siguiente se proporcionan algunos ejemplos de los tipos de cargas de trabajo que los usuarios suelen traspasar a los distintos tipos de nubes. También podría elegir un enfoque híbrido en el que tenga clústeres en ejecución en ambos entornos.
{: shortdesc}

| Carga de trabajo | externo a {{site.data.keyword.containershort_notm}} | local |
| --- | --- | --- |
| Herramientas de habilitación de DevOps | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" /> | |
| Desarrollo y prueba de apps | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" /> | |
| Apps que tienen grandes cambios de demanda y se tiene que escalar rápidamente | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" /> | |
| Apps empresariales como CRM, HCM, ERP y E-commerce | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" /> | |
| Herramientas sociales y de colaboración como el correo electrónico | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" /> | |
| Cargas de trabajo de Linux y x86 | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" /> | |
| Recursos de cálculo de GPU y nativos | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" /> |
| Cargas de trabajo conformes con PCI e HIPAA | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" /> |
| Apps existentes con restriccions y dependencias de plataforma e infraestructura | | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" /> |
| Apps propietarias con diseños, licencias o normativas estrictos | | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" /> |
| Escalar apps en la nube pública y sincronizar los datos en una base de datos privada local | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" />  | <img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" /> |
{: caption="Las implementaciones de {{site.data.keyword.Bluemix_notm}} dan soporte a sus cargas de trabajo" caption-side="top"}

**¿Está listo para ejecutar cargas de trabajo externas en {{site.data.keyword.containerlong_notm}}?**</br>
¡Estupendo! Ya ha llegado hasta nuestra documentación de nube pública. Siga leyendo para obtener más ideas sobre estrategia o empiece a toda marcha [creando un clúster ya](/docs/containers?topic=containers-getting-started).

**¿Le interesa una nube local?**</br>
Explore la [documentación de {{site.data.keyword.Bluemix_notm}} Private ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.1/kc_welcome_containers.html). Si ya ha hecho inversiones importantes en tecnología de IBM, como por ejemplo en WebSphere Application Server y Liberty, puede optimizar su estrategia de modernización de {{site.data.keyword.Bluemix_notm}} Private con diversas herramientas.

**¿Desea ejecutar cargas de trabajo en nubes tanto locales como externas?**</br>
Empiece configuranso una cuenta de {{site.data.keyword.Bluemix_notm}} Private. A continuación, consulte [Uso de {{site.data.keyword.containerlong_notm}} con {{site.data.keyword.Bluemix_notm}} Private](/docs/containers?topic=containers-hybrid_iks_icp) para conectar el entorno de {{site.data.keyword.Bluemix_notm}} Private con un clúster en {{site.data.keyword.Bluemix_notm}} Public. Para gestionar varios clústeres de Kubernetes de nube, como por ejemplo entre {{site.data.keyword.Bluemix_notm}} Público y {{site.data.keyword.Bluemix_notm}} Privado, consulte [IBM Multicloud Manager ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).

### ¿Qué tipo de apps puedo ejecutar en {{site.data.keyword.containerlong_notm}}?
{: #app_types}

Su app contenerizada debe poderse ejecutar en el sistema operativo soportado, Ubuntu 16.64, 18.64. También debe la situación de estado de su app.
{: shortdesc}

<dl>
<dt>Apps sin estado</dt>
  <dd><p>Para los entornos de nube nativa como Kubernetes se prefieren las apps sin estado. Son fáciles de migrar y escalar porque declaran dependencias y almaenan las configuraciones de forma separada del código, y tratan los servicios de respaldo como por ejemplo las bases de datos como recursos adjuntos en lugar de emparejados a la app. Los pods de las apps no requieren un almacenamiento de datos persistente ni una dirección IP de red estable, como tales, los pods se pueden terminar, replanificar y escalar como respuesta a las demandas de carga de trabajo. La app utiliza Database-as-a-Service para los datos persistentes y NodePort, un equilibrador de carga, o los servicios de Ingress para exponer la carga de trabajo en una dirección IP estable.</p></dd>
<dt>Apps con estado</dt>
  <dd><p>Las apps con estado son más complicadas de configurar, gestionar y escalar que las apps sin estado porque los pods requieren datos persistentes y una identidad de red estable. Las apps con estado son a menudo bases de datos u otras cargas de trabajo distribuidas, intensivas en datos, en las que el procesamiento es más eficiente más cerca de los datos propiamente dichos.</p>
  <p>Si desea desplegar una app con estado, tiene que configurar el almacenamiento persistente y montar un volumen persistente en el pod controlado por un objeto StatefulSet. Puede elegir añadir almacenamiento de [archivos](/docs/containers?topic=containers-file_storage#file_statefulset), [bloques](/docs/containers?topic=containers-block_storage#block_statefulset) u [objetos](/docs/containers?topic=containers-object_storage#cos_statefulset) como almacenamiento persistente para el conjunto con estado. También puede instalar [Portworx](/docs/containers?topic=containers-portworx) sobre los nodos trabajadores nativos y utilizar Portworx como solución de almacenamiento definida por software de alta disponibilidad para gestionar el almacenamiento persistente para las apps con estado. Para obtener más información sobre cómo funcionan los conjuntos con estado, consulte la [documentación de Kubernetes![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/).</p></dd>
</dl>

### Algunas directrices para desarrollar apps sin estado, nativas de la nube
{: #12factor}

Compruebe el documento [Twelve-Factor App ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://12factor.net/), una metodología independiente del lenguaje para pensar cómo desarrollar su app mediante doce factores, que se resumen a continuación.
{: shortdesc}

1.  **Código Base**: Utilice un solo código base en un sistema de control de versiones para sus despliegues. Al extraer una imagen para el despliegue del contenedor, especifique una etiqueta de imagen probada en lugar de utilizar `latest`.
2.  **Dependencias**: Declare explícitamente y aisle las dependencias externas.
3.  **Configuración**: Almacene la configuración específica del despliegue en variables de entorno, no en el código.
4.  **Servicios de copia de seguridad**: Trate los servicios de copia de seguridad, tales como almacenes de datos o colas de mensajes, como recursos adjuntos o sustituibles.
5.  **Estapas de una app**: Cree las apps en etapas diferenciadas, como por ejemplo `build`, `release`, `run`, con una separación extricta entre ellas.
6.  **Procesos**: Ejecute la app como uno o más procesos sin estado que no compartan nada y utilice [almacenamiento persistente](/docs/containers?topic=containers-storage_planning) para guardar los datos.
7.  **Enlace de puertos**: Los enlaces de puertos deben ser independientes y proporcionar un punto final de servicio en un host y un puero bien definidos.
8.  **Concurrencia**: Gestione y escale la app a través de instancias de proceso tales como réplicas y escalado horizontal. Establezca solicitudes de recursos y límites para los despliegues. Hay que tener presente que las políticas de red de Calico no permiten limitar el ancho de banda. En su lugar, se puede utilizar [Istio](/docs/containers?topic=containers-istio).
9.  **Desechabilidad**: Diseñe su app para que sea fácil de desechar, con un inicio rápido, una conclusión ordenada y buena tolerencia a terminaciones abruptas de procesos. Recuerde, los contenedores, los pods, e incluso los nodos trabajadores deben ser desechables, por lo que debe planificar su app en consecuencia.
10.  **Paridad en desarrollo y producción**: Diseñe una [integración continua](https://www.ibm.com/cloud/garage/content/code/practice_continuous_integration/) y un [entrega continua](https://www.ibm.com/cloud/garage/content/deliver/practice_continuous_delivery/) de la app, con una diferencia mínima entre la app en desarrollo y la app en producción.
11.  **Registros**: Trate los archivos de registro como secuencias de sucesos: el entorno externo o de alojamiento es quien procesa y direcciona los archivos de registro. **Importante**: En {{site.data.keyword.containerlong_notm}}, los archivos de registro no están activados de forma predeterminada. Para habilitarlos, consulte [Configuración del reenvío de archivos de registro](/docs/containers?topic=containers-health#configuring).
12.  **Procesos de administración**: Guarde los scripts de administración que se ejecutan una vez con la app y ejecútelos como un [objeto de Trabajo de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) para asegurarse de que los scripts de administración se ejecutan en el mismo entorno que la propia app. Para la orquestación de paquetes más grandes que desee ejecutar en los clústeres de Kubernetes, considere la posibilidad de icono un gestor de paquetes como por ejemplo [Helm ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://helm.sh/).

### Ya tengo una app. ¿Cómo puedo migrarla a {{site.data.keyword.containerlong_notm}}?
{: #migrate_containerize}

Puede realizar algunos pasos generales para contenerizar la app como se indica a continuación.
{: shortdesc}

1.  Utilice el documento [Twelve-Factor App ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://12factor.net/) como guía para aislar dependencias, separar procesos en servicios distintos y reducir la opción de estado de su app el máximo posible.
2.  Busque una imagen base encontrar para utilizar. Puede utilizar imágenes disponibles públicamente de [Docker Hub ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://hub.docker.com/), en [imágenes públicas de IBM](/docs/services/Registry?topic=registry-public_images#public_images), o bien crear y gestionar sus propios en {{site.data.keyword.registryshort_notm}} privado.
3.  Añada a su imagen de Docker sólo aquello que sea necesario para ejecutar la app.
4.  En lugar de utilizar un almacenamiento local, planifique utilizar soluciones de almacenamiento persistente o de base de datos como servicio en la nube para realizar copias de seguridad de los datos de su app.
5.  Con el tiempo, refactorie los procesos de la app en microservicios.

Para obtener más información, consulte las siguientes guías de aprendizaje:
*  [Migración de una app desde Cloud Foundry a un clúster](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)
*  [Transición de una app basada en VM a Kubernetes](/docs/tutorials?topic=solution-tutorials-vm-to-containers-and-kubernetes)



Continúe con los temas siguientes para obtener más consideraciones al trasladar cargas de trabajo, como entornos de Kubernetes, alta disponibilidad, descubrimiento de servicios y despliegues.

<br />


### ¿Qué conocimientos técnicos se recomienda tener antes de mover mis apps a {{site.data.keyword.containerlong_notm}}?
{: #knowledge}

Kubernetes se ha diseñado para proporcionar funciones a dos personas principales, el administrador del clúster y el desarrollador de la app. Cada persona utiliza diferentes conocimientos técnicos para ejecutar y desplegar las apps de forma satisfactoria en un clúster.
{: shortdesc}

**¿Cuáles son las principales tareas que lleva a cabo el administrador del clúster y qué conocimientos técnicos debe tener?** </br>
El administrador del clúster es el responsable de configurar, poner en funcionamiento, proteger y gestionar la infraestructura de {{site.data.keyword.Bluemix_notm}} para el clúster. Las tareas que suele realizar incluyen las siguientes:
- Dimensionar el clúster para proporcionar suficiente capacidad para las cargas de trabajo.
- Diseñar un clúster que se ajuste a los estándares de alta disponibilidad, recuperación tras desastre y conformidad de la empresa.
- Proteger el clúster mediante la configuración de permisos de usuario y la limitación de las acciones que se pueden realizar dentro del clúster a fin de proteger los recursos de cálculo, la red y los datos.
- Planificar y gestionar la comunicación a través de la red entre los componentes de la infraestructura para garantizar la seguridad de la red, la segmentación y la conformidad.
- Planificar opciones de almacenamiento persistente que satisfagan los requisitos de residencia de datos y de protección de datos.

El administrador del clúster debe tener un amplio conocimiento que incluya cálculo, red, almacenamiento, seguridad y conformidad. Generalmente estos conocimientos se distribuye entre varios especialistas, como ingenieros de sistemas, administradores de sistemas, ingenieros de red, arquitectos de red, gestores de TI o especialistas en seguridad y conformidad. Considere la posibilidad de asignar el rol de administración de clúster a varias personas de la empresa para conseguir los conocimientos necesarios para trabajar correctamente con el clúster.

**¿Cuáles son las principales tareas que lleva a cabo el desarrollador de apps y qué conocimientos técnicos debe tener?** </br>
El desarrollador diseña, crea, protege, despliega, prueba, ejecuta y supervisa apps contenerizadas nativas de la nube en un clúster de Kubernetes. Para crear y ejecutar estas apps, debe estar familiarizado con el concepto de microservicios, con las directrices de [app de 12 factores](#12factor), con los [principios de Docker y de contenerización](https://www.docker.com/) y con las [opciones de despliegue de Kubernetes](/docs/containers?topic=containers-app#plan_apps) disponibles. Si desea desplegar apps sin servidor, debe estar familiarizado con [Knative](/docs/containers?topic=containers-cs_network_planning).

Kubernetes y {{site.data.keyword.containerlong_notm}} ofrecen varias opciones para [exponer una app y mantenerla en el ámbito privado](/docs/containers?topic=containers-cs_network_planning), [añadir almacenamiento persistente](/docs/containers?topic=containers-storage_planning), [integrar otros servicios](/docs/containers?topic=containers-ibm-3rd-party-integrations) y [proteger las cargas de trabajo y los datos confidenciales](/docs/containers?topic=containers-security#container). Antes de trasladar una app a un clúster en {{site.data.keyword.containerlong_notm}}, verifique que puede ejecutar la app como una app contenerizada en el sistema operativo Ubuntu 16.64, 18.64 soportado y que Kubernetes y {{site.data.keyword.containerlong_notm}} ofrecen las prestaciones que necesita su carga de trabajo.

**¿Interactúan entre sí los administradores y los desarrolladores del clúster?** </br>
Sí. Los administradores y los desarrolladores del clúster deben interactuar con frecuencia para que los administradores del clúster comprendan los requisitos de la carga de trabajo para proporcionar la capacidad necesaria y para que los desarrolladores conozcan las limitaciones, las integraciones y los principios de seguridad que deben tener en cuenta en el proceso de desarrollo de apps.

## Dimensionar el clúster de Kubernetes para dar soporte a la carga de trabajo
{: #sizing}

Saber cuántos nodos trabajadores necesita en el clúster para dar soporte a su carga de trabajo no es una ciencia exacta. Es posible que tenga que probar diferentes configuraciones y adaptarlas. Es bueno que esté utilizando {{site.data.keyword.containerlong_notm}}, puesto que puede añadir o eliminar nodos trabajadores según las demandas de la carga de trabajo.
{: shortdesc}

Para empezar a dimensionar el clúster, hágase las siguientes preguntas.

### ¿Cuántos recursos necesita mi app?
{: #sizing_resources}

En primer lugar, empecemos por el uso de carga de trabajo que tiene actualmente o que proyecta tener.

1.  Calcule el uso promedio de CPU y de memoria de la carga de trabajo. Por ejemplo, puede ver el Gestor de tareas en una máquina Windows, o ejecutar el mandato `top` en un sistema Mac o Linux. Tambén puede utilizar un servicio de métricas y ejecutar informes para calcular el uso de carga de trabajo.
2.  Anticipe el número de solicitudes a las que debe servir la carga de trabajo y así podrá decidir cuántas réplicas de la app desea que maneje la carga de trabajo. Por ejemplo, puede diseñar una instancia de app que maneje 1000 solicitudes por minuto y anticipar que su carga de trabajo debe servir 10000 solicitudes por minuto. En ese caso, puede decidir crear 12 réplicas de la app, 10 para manejar la cantidad prevista y 2 para poder hacer frente a sobrecargas.

### ¿Qué más, aparte de mi app, puede utilizar recursos del clúster?
{: #sizing_other}

Veamos ahora algunas otras características que podría utilizar.



1.  Imagine que su app extrae muchas imágenes o muy grandes, esto puede ocupar bastante espacio de almacenamiento local en el nodo trabajador.
2.  Decida si desea [integrar servicios](/docs/containers?topic=containers-supported_integrations#supported_integrations) en el clúster, como por ejemplo [Helm](/docs/containers?topic=containers-helm#public_helm_install) o [Prometheus ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus). Estos servicios integrados y complementos sincronizan los pods que consumen recursos del clúster.

### ¿Qué tipo de disponibilidad desea que tenga mi carga de trabajo?
{: #sizing_availability}

No olvide que quiere que su carga de trabajo esté activa lo más posible

1.  Planifique su estrategia de [clústeres de alta disponibilidad](/docs/containers?topic=containers-ha_clusters#ha_clusters), como por ejemplo decidiendo entre utilizar clústeres de una zona o multizona.
2.  Revise [despliegues de alta disponibilidad](/docs/containers?topic=containers-app#highly_available_apps) para ayudarle a decidir cómo puede hacer que la app esté disponible.

### ¿Cuántos nodos trabajadores necesito para manejar mi carga de trabajo?
{: #sizing_workers}

Ahora que tiene una idea de cómo es de su carga de trabajo, correlacionaremos el uso estimado con las configuraciones de clúster disponibles.

1.  Calcule la capacidad máxima del nodo trabajador, que dependerá del tipo de clúster que tenga. No desea que se supere la capacidad máxima del nodo trabajador en caso de que se produzca una sobrecarga u otro suceso temporal.
    *  **Clústeres de una sola zona**: Planifique tener al menos 3 nodos trabajadores en el clúster. Además, desea tener disponible una capacidad adicional de CPU y memoria equivalente al valor de 1 nodo en el clúster.
    *  **Clústeres multizona**: Planifique tener al menos 2 nodos trabajadores por zona, es decir, si tiene 3 zonas, 6 nodos en total. Además, planifique que la capacidad total del clúster sea al menos el 150% de la capacidad requerida de la carga de trabajo, de forma que si una zona deja de funcionar, tenga recursos disponibles para mantener la carga de trabajo.
2.  Alinee el tamaño de la app y la capacidad de los nodos trabajadores con uno de los [tipos de nodo trabajador disponibles](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes). Para ver los tipos disponibles en una zona, ejecute el mandato `ibmcloud ks machine-types <zone>`.
    *   **No sobrecarque los nodos trabajadores**: Para evitar que sus pods compitan por la CPU o se ejecuten de forma ineficiente, debe saber qué recursos requieren sus apps de forma que pueda planificar el número de nodos trabajadores que necesita. Por ejemplo, si las apps requieren menos recursos que los que hay disponibles en el nodo trabajador, puede limitar el número de pods que despliega en un nodo trabajador. Mantenga el nodo de trabajo a un 75% de capacidad aproximadamente para dejar espacio para otros pods que se puedan programar. Si las apps requieren más recursos de los que hay disponibles en el nodo trabajador, utilice un tipo de nodo trabajador distinto que pueda cumplir estos requisitos. Sabe que los nodos trabajadores están sobrecargados cuando notifican con frecuencia el estado `NotReady` o desalojan pods debido a falta de memoria o de otros recursos.
    *   **Tipos de nodo trabajador más grandes o más pequeños**: los nodos más grandes pueden ser más eficientes en coste que los nodos más pequeños, en particular para las cargas de trabajo que están diseñadas para aumentar la eficiencia cuando se procesan en una máquina de alto rendimiento. No obstante, si un nodo trabajador grande queda inactivo, tiene que asegurarse de que el clúster tiene la capacidad suficiente para volver a planificar correctamente todos los pods de carga de trabajo en otros nodos de trabajo del clúster. El uso de nodos más pequeños puede permitirle escalar más fácilmente.
    *   **Réplicas de la app**: Para determinar el número de nodos trabajadores que desea, también puede considerar cuántas réplicas de la app desea ejecutar. Por ejemplo, si sabe que la carga de trabajo requiere 32 núcleos de CPU, y tiene previsto ejecutar 16 réplicas de la app, cada pod de réplica necesita 2 núcleos de CPU. Si desea ejecutar solo un pod de app por nodo trabajador, puede solicitar un número apropiado de nodos trabajadores para su tipo de clúster para dar soporte a esta configuración.
3.  Ejecute pruebas de rendimiento para continuar refinando el número de nodos de trabajo que necesita en el clúster, con la latencia, la escalabilidad, el conjunto de datos y los requisitos de carga de trabajo representativos.
4.  Para las cargas de trabajo que deben ampliarse y reducirse en respuesta a las solicitudes de recursos, configure el [programa de escalado automático horizontal de pods](/docs/containers?topic=containers-app#app_scaling) y el [programa de escalado automático de agrupaciones de trabajadores de clúster](/docs/containers?topic=containers-ca#ca).

<br />


## Estructuración del entorno de Kubernetes
{: #kube_env}

Su {{site.data.keyword.containerlong_notm}} está vinculado sólo a un portafolio de infraestructura de IBM Cloud (SoftLayer). Dentro de la cuenta, puede crear clústeres que estén formados por un nodo maestro y varios nodos trabajadores. IBM gestiona el maestro, y puede crear una mezcla de agrupaciones de trabajadores que agrupan máquinas individuales del mismo tipo, o especificaciones de memoria y CPU. Dentro del clúster, puede organizar más los recursos utilizando los espacios de nombres y las etiquetas. Elija la combinación adecuada de clústeres, tipos de máquina y estrategias de organización para poder asegurarse de que sus equipos y cargas de trabajo obtienen los recursos que necesitan.
{:shortdesc}

### ¿Qué tipo de clúster y de máquinas debo obtener?
{: #env_flavors}

**Tipos de clústeres**: decida si desea una [configuración de una única zona, de varias zonas o de varios clústeres](/docs/containers?topic=containers-ha_clusters#ha_clusters). Los clústeres multizona están disponibles en [las seis regiones metropolitanas de {{site.data.keyword.Bluemix_notm}} en todo el mundo](/docs/containers?topic=containers-regions-and-zones#zones). Asimismo, tenga en cuenta que los nodos trabajadores varían según la zona.

**Tipos de nodos trabajadores**: en general, es más adecuado ejecutar las cargas de trabajo intensivas en máquinas físicas nativas, mientras que para realizar un trabajo de prueba y desarrollo efectivo en coste, puede elegir máquinas virtuales en hardware compartido o compartido dedicado. Con nodos trabajadores nativos, el clúster tiene una velocidad de red de 10 Gbps y núcleos de hiper-hebras que ofrecen un rendimiento más alto. Las máquinas virtuales tienen una velocidad de red de 1 Gbps y núcleos regulares que no ofrecen la funcionalidad de hiper-hebra. [Consulte los tipos y aislamiento de máquina disponibles](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes).

### ¿Utilizo varios clústeres, o simplemente añado más trabajadores a un clúster existente?
{: #env_multicluster}

El número de clústeres que cree dependerá de su carga de trabajo, de las políticas y normas de la empresa y de lo que desee hacer con los recursos de cálculo. También puede revisar la información de seguridad sobre esta decisión en [Seguridad y aislamiento de contenedores](/docs/containers?topic=containers-security#container).

**Varios clústeres**: tiene que configurar [un equilibrador de carga global](/docs/containers?topic=containers-ha_clusters#multiple_clusters) y copiar y aplicar los mismos archivos YAML de configuración en cada uno para equilibrar las cargas de trabajo entre los clústeres. Por lo tanto, los clústeres múltiples suelen ser más complejos de gestionar, pero pueden ayudarle a alcanzar objetivos importantes como, por ejemplo, los siguientes.
*  Cumplir con las políticas de seguridad que requieren aislar las cargas de trabajo.
*  Probar cómo se ejecuta la app en una versión distinta de Kubernetes u otro software de clúster como por ejemplo Calico.
*  Crear un clúster con la app en otra región para obtener un rendimiento más alto para los usuarios de esa área geográfica.
*  Configurar el acceso de usuario a nivel de instancia de clúster en lugar de personalizar y gestionar varias políticas RBAC para controlar el acceso dentro de un clúster a nivel de espacio de nombres.

**Menos clústeres o clúster único**: El uso de menos clústeres puede ayudarle a reducir el esfuerzo operativo y los costes por clúster de los recursos fijos. En lugar de hacer más clústeres, puede añadir agrupaciones de trabajadores para distintos tipos de máquina de recursos informáticos disponibles para la app y los componentes de servicio que desea utilizar. Cuando desarrolla la app, los recursos que utiliza están en la misma zona, o bien estrechamente conectados en una multizona, de forma que puede hacer presunciones sobre latencia, ancho de banda o errores correlacionados. Sin embargo, es aún más importante que organice el clúster utilizando los espacios de nombres, las cuotas de recursos y las etiquetas.

### ¿Cómo puedo configurar mis recursos dentro del clúster?
{: #env_resources}

<dl>
<dt>Considere la capacidad del nodo trabajador.</dt>
  <dd>Para obtener el máximo rendimiento del rendimiento del nodo trabajador, tenga en cuenta lo siguiente:
  <ul><li><strong>Mantenga la fuerza del núcleo</strong>: Cada máquina tiene un determinado número de núcleos. Según la carga de trabajo de su app, establezca un número límite de pods por núcleo, como por ejemplo 10.</li>
  <li><strong>Evite la sobrecarga de los nodos</strong>: De forma similar, sólo porque un nodo pueda contener más de 100 pods no significa que esto sea deseable. Según la carga de trabajo de su app, establezca un límite de pods por nodo, como por ejemplo 40.</li>
  <li><strong>No llegue al máximo de ancho de banda del clúster</strong>: Recuerde que el ancho de banda de red en máquinas virtuales de escalado es de alrededor de 1000 Mbps. Si necesita centenares de nodos trabajadores en un clúster, divídalo en varios clústeres con menos nodos o solicite nodos nativos.</li>
  <li><strong>Ordenación de los servicios</strong>: Planifique cuántos servicios necesita para su carga de trabajo antes de realizar el despliegue. Las reglas de red y de reenvío de puerto se colocan en Iptables. Si prevé un número mayor de servicios, como por ejemplo más de 5.000 servicios, divida el clúster en varios clústeres.</li></ul></dd>
<dt>Suministre diferentes tipos de máquinas para una mezcla de recursos informáticos.</dt>
  <dd>A todo el mundo le gusta poder elegir, ¿verdad? Con {{site.data.keyword.containerlong_notm}}, tiene [una mezcla de tipos de máquina](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) que puede desplegar desde máquinas nativas para cargas de trabajo intensivas a máquinas virtuales para un rápido escalado. Utilice etiquetas o espacios de nombres para organizar los despliegues de las máquinas. Cuando cree un despliegue, limítelo para que el pod de su app sólo se despliegue en máquinas con la mezcla correcta de recursos. Por ejemplo, puede que desee limitar una aplicación de base de datos a una máquina nativa con un volumen importante de almacenamiento en disco local como por ejemplo `md1c.28x512.4x4tb`.</dd>
<dt>Configure varios espacios de nombres cuando tenga varios equipos y proyectos que compartan el clúster.</dt>
  <dd><p>Los espacios de nombres son un tipo de clúster dentro del clúster. Son una forma de dividir los recursos del clúster utilizando [cuotas de recursos ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/policy/resource-quotas/) y [límites predeterminados ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/). Cuando cree nuevos espacios de nombres, asegúrese de configurar [políticas de RBAC](/docs/containers?topic=containers-users#rbac) adecuadas para controlar el acceso. Para obtener más información, consulte [Compartir un clúster con espacios de nombres ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/) en la documentación de Kubernetes.</p>
  <p>Si tiene un pequeño clúster, un par de docenas de usuarios y recursos que son similares (por ejemplo, diferentes versiones del mismo software), probablemente no necesite varios espacios de nombres. En su lugar, puede utilizar etiquetas.</p></dd>
<dt>Establezca las cuotas de recursos de forma que los usuarios del clúster deban utilizar solicitudes y límites de recursos.</dt>
  <dd>Para asegurarse de que cada equipo tiene los recursos necesarios para desplegar servicios y ejecutar apps en el clúster, debe configurar [cuotas de recursos](https://kubernetes.io/docs/concepts/policy/resource-quotas/) para cada espacio de nombres. Las cuotas de recursos determinan las restricciones de despliegue para un espacio de nombres, como el número de recursos de Kubernetes que se pueden desplegar y la cantidad de CPU y de memoria que pueden consumir estos recursos. Después de establecer una cuota, los usuarios deben incluir los límites y las solicitudes de recursos en sus despliegues.</dd>
<dt>Organice los objetos de Kubernetes con etiquetas</dt>
  <dd><p>Para organizar y seleccionar los recursos de Kubernetes, como por ejemplo `pods` o `nodes`, [use etiquetas de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/). De forma predeterminada, {{site.data.keyword.containerlong_notm}} aplica algunas etiquetas, como `arch`, `os`, `region`, `zone` y `machine-type`.</p>
  <p>Los casos de uso de ejemplo para las etiquetas incluyen [limitar el tráfico de red a nodos trabajadores de extremo](/docs/containers?topic=containers-edge), [desplegar una app en una máquina GPU](/docs/containers?topic=containers-app#gpu_app) y [restringir las cargas de trabajo de la app![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/) para que se ejecuten en nodos de trabajo que cumplan ciertas capacidades de tipo de máquina o SDS, como por ejemplo nodos trabajadores nativos. Para ver las etiquetas que ya se han aplicado a un recurso, utilice el mandato <code>kubectl get</code> con el distintivo <code>--show-labels</code>. Por ejemplo:</p>
  <p><pre class="pre"><code>kubectl get node &lt;node_ID&gt; --show-labels</code></pre></p>
  Para aplicar etiquetas a nodos trabajadores, [cree la agrupación de nodos trabajadores](/docs/containers?topic=containers-add_workers#add_pool) con etiquetas o [actualice una agrupación de nodos trabajadores existente](/docs/containers?topic=containers-add_workers#worker_pool_labels)</dd>
</dl>




<br />


## Cómo hacer que sus recursos estén altamente disponibles
{: #kube_ha}

Si bien ningún sistema es completamente seguro, puede tomar medidas para aumentar la alta disponibilidad de sus apps y servicios en {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Revise más información sobre cómo hacer que los recursos estén altamente disponibles.
* [Reduzca los puntos potenciales de error](/docs/containers?topic=containers-ha#ha).
* [Cree clústeres multizona](/docs/containers?topic=containers-ha_clusters#ha_clusters).
* [Planifique despliegues de alta disponibilidad](/docs/containers?topic=containers-app#highly_available_apps) que utilicen características tales como conjuntos de réplicas y antiafinidad de pod entre multizonas.
* [Ejecute contenedores basados en imágenes en un registro público basado en la nube](/docs/containers?topic=containers-images).
* [Planifique el almacenamiento de datos](/docs/containers?topic=containers-storage_planning#persistent_storage_overview). Especialmente para clústeres multizona, considere la posibilidad de utilizar un servicio de nube como por ejemplo [{{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) o [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about).
* Para clústeres multizona, habilite un [servicio de equilibrador de carga](/docs/containers?topic=containers-loadbalancer#multi_zone_config) o el [equilibrador de carga multizona](/docs/containers?topic=containers-ingress#ingress) de Ingress para exponer sus apps públicamente.

<br />


## Configuración del descubrimiento de servicios
{: #service_discovery}

Cada uno de los de su clúster de Kubernetes tiene una dirección IP. Pero cuando despliega una app en el clúster, no desea depender de la dirección IP del pod para el descubrimiento de servicios y la red. Las pods se eliminan y se sustituyen con frecuencia y dinámicamente. En su lugar, utilice un servicio de Kubernetes, que representa un grupo de pods y proporciona un punto de entrada estable a través de la dirección IP virtual del servicio, denominado `IP de clúster`. Para obtener más información, consulte la documentación de Kubernetes en [Servicios ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/services-networking/service/#discovering-services).
{:shortdesc}

### ¿Puedo personalizar el proveedor de DNS del clúster de Kubernetes?
{: #services_dns}

Cuando se crean servicios y pods, se les asigna un nombre DNS para que los contenedores de apps puedan utilizar la IP de servicio de DNS para resolver los nombres DNS. Puede personalizar el DNS del pod para especificar los servidores de nombres, las búsquedas y las opciones de lista de objetos. Para obtener más información, consulte [Configuración del proveedor de DNS del clúster](/docs/containers?topic=containers-cluster_dns#cluster_dns).
{: shortdesc}



### ¿Cómo puedo asegurarme de que mis servicios están conectados a los despliegues correctos y listos para funcionar?
{: #services_connected}

Para la mayoría de los servicios, añada un selector al archivo de servicio `.yaml` de modo que se aplique a los pods que ejecutan las apps por esa etiqueta. Muchas veces cuando su app se inicia por primera vez, no desea que procese las solicitudes en seguida. Añada un sondeo de preparación a su despliegue de modo que el tráfico sólo se envíe a un pod que se considere que está preparado. Para ver un ejemplo de un despliegue con un servicio que utiliza etiquetas y establece un sondeo de preparación, consulte [NGINX YAML](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml).
{: shortdesc}

A veces, no desea que el servicio utilice una etiqueta. Por ejemplo, es posible que tenga una base de datos externa o que desee apuntar el servicio a otro servicio de un espacio de nombres diferente dentro del clúster. Cuando esto sucede, tiene que añadir manualmente un objeto de puntos finales y enlazarlo al servicio.


### ¿Cómo controlo el tráfico de red entre los servicios que se ejecutan en mi clúster?
{: #services_network_traffic}

De forma predeterminada, los pods pueden comunicarse con otros pods en el clúster, pero puede bloquear el tráfico a determinados pods o espacios de nombres con políticas de red. Además, si expone la app externamente utilizando un servicio de NodePort, de equilibrador de carga o de Ingress, es posible que desee configurar políticas de red avanzadas para bloquear el tráfico. En {{site.data.keyword.containerlong_notm}}, puede utilizar Calico para gestionar las [políticas de red de Kubernetes y Calico para controlar el tráfico](/docs/containers?topic=containers-network_policies#network_policies).

Si tiene diversos microservicios que se ejecutan en plataformas para las que necesita conectarse, gestionar y proteger el tráfico de red, considere la posibilidad de utilizar una herramienta de red de servicios como, por ejemplo, el [complemento Istio gestionado](/docs/containers?topic=containers-istio).

También puede [configurar nodos de extremo](/docs/containers?topic=containers-edge#edge) para aumentar la seguridad y el aislamiento de su clúster restringiendo la carga de trabajo de red para seleccionar nodos trabajadores.



### ¿Cómo puedo exponer mis servicios en Internet?
{: #services_expose_apps}

Puede crear tres tipos de servicios para redes externas: NodePort, LoadBalancer e Ingress. Para obtener más información, consulte [Planificación de servicios de red](/docs/containers?topic=containers-cs_network_planning#external).

Al planificar cuántos objetos `Service` necesita en el clúster, tenga en cuenta que Kubernetes utiliza `iptables` para gestionar las reglas de red y de reenvío de puerto. Si ejecuta un gran número de servicios en el clúster, por ejemplo, 5000, el rendimiento puede verse afectado.



## Despliegue de cargas de trabajo de apps en clústeres
{: #deployments}

Con Kubernetes, debe declarar muchos tipos de objetos en archivos de configuración YAML, como por ejemplo pods, despliegues y trabajos. Estos objetos describen cosas tales como las apps en contenedores que se están ejecutando, los recursos que utilizan y las políticas que gestionan su comportamiento a la hora de reiniciar, actualizar, replicar, etc. Para obtener más información, consulte los documentos de Kubernetes sobre [Mejores prácticas de configuración ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/configuration/overview/).
{: shortdesc}

### Creía que tenía que poner mi app en un contenedor. ¿Qué es todo esto de los pods?
{: #deploy_pods}

Un [pod ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/pods/pod/) es la unidad desplegable más pequeña que Kubernetes puede gestionar. El usuario pone su contenedor (o un grupo de contenedores) en un pod y usa el archivo de configuración del pod para indicar al pod cómo ejecutar el contenedor y compartir recursos con otros pods. Todos los contenedores que coloque en un pod se ejecutan en un contexto compartido, lo que significa que comparten la misma máquina virtual o física.
{: shortdesc}

**Qué se debe poner en un contenedor**: Al pensar en los componentes de la aplicación, tenga en cuenta si tienen requisitos de recursos significativamente diferentes para cosas como la CPU y la memoria. ¿Algunos componentes podrían ejecutarse mejor reduciendo un poco para desviar recursos hacia otras áreas? ¿Hay otro componente que es de cara al cliente, por lo que es de gran importancia que se mantenga activo? Divídalos en contenedores separados. Siempre puede desplegarlos en el mismo pod de modo que se ejecuten de forma sincronizada.

**Qué se debe poner en un pod**: Los contenedores de la app no siempre tienen que estar en el mismo pod. De hecho, si tiene un componente con estado y difícil de escalar, como por ejemplo un servicio de base de datos, póngalo en un pod distinto que pueda planificar en un nodo trabajador con más recursos para manejar la carga de trabajo. Si los contenedores funcionan correctamente si se ejecutan en nodos de trabajo diferentes, utilice varios pods. Si tienen que estar en la misma máquina y escalarse juntos, agrupe los contenedores en el mismo pod.

### Así pues, si puedo utilizar un solo pod, ¿para qué necesito todos estos diferentes tipos de objetos?
{: #deploy_objects}

Crear un archivo YAML de pod es fácil. Puede escribir una con sólo unas pocas líneas como se indica a continuación.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
{: codeblock}

Pero con esto no es suficiente. Si el nodo con el que se ejecuta el pod queda inactivo, el pod queda inactivo también y no se vuelve a programar. En lugar de ello, utilice un [despliegue ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) para dar soporte a la reprogramación de pods, los conjuntos de réplicas y actualizaciones continuas. Un despliegue básico es casi tan fácil de hace como un pod. En lugar de definir sólo el contenedor en la `spec`, debe especificar `replicas` y una `template` en la `spec` del despliegue. La plantilla tiene su propia `spec` para los contenedores dentro de la misma, como por ejemplo el código siguiente.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```
{: codeblock}

Puede seguir añadiendo características como, por ejemplo, antiafinidad o límites de recursos, todos en el mismo archivo YAML.

Para obtener una explicación más detallada de las distintas características que puede añadir al despliegue, consulte el tema [Cómo crear el archivo YAML de despliegue de la app](/docs/containers?topic=containers-app#app_yaml).
{: tip}

### ¿Cómo puedo organizar mis despliegues para hacerlos más fáciles de actualizar y gestionar?
{: #deploy_organize}

Ahora que tiene una idea de lo que desea incluir en el despliegue, quizás se pregunte cómo va a gestionar todos estos archivos YAML distintos. Por no mencionar los objetos que crean en el entorno de Kubernetes.

Algunas sugerencias para organizar los archivos YAML del despliegue:
*  Utilice un sistema de control de versiones, como por ejemplo Git.
*  Agrupe los objetos Kubernetes estrechamente relacionados dentro de un solo archivo YAML. Por ejemplo, si está creando un `deployment`, también puede añadir el archivo `service` al YAML. Separe los objetos mediante `---` como se indica a continuación:
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    ...
    ---
    apiVersion: v1
    kind: Service
    metadata:
    ...
    ```
    {: codeblock}
*  Puede utilizar el mandato `kubectl apply -f` para aplicar a un directorio entero, no sólo a un archivo.
*  Pruebe el [proyecto `kustomize`](/docs/containers?topic=containers-app#kustomize) que puede utilizar para ayudar a escribir, personalizar y reutilizar configuraciones de YAML de recursos de Kubernetes.

Dentro del archivo YAML, puede utilizar etiquetas o anotaciones como metadatos para gestionar los despliegues.

**Etiquetas**: Las [etiquetas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) son pares de clave-valor (`key:value`) que se pueden adjuntar a objetos de Kubernetes como por ejemplo pods y despliegues. Pueden ser lo que desee y son útiles para seleccionar objetos basándose en la información de la etiqueta. Las etiquetas proporcionan la base para la agrupación de objetos. Algunas ideas de etiquetas:
* `app: nginx`
* `version: v1`
* `env: dev`

**Anotaciones**: Las [anotaciones ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) son parecidas a las etiquetas porque también son pares de clave-valor (`key:value`). Son mejores para información que no es identificativa y que pueden aprovechar las herramientas o las bibliotecas como, por ejemplo, información adicional sobre la procedencia de un objeto, cómo utilizar el objeto, punteros a repositorios de seguimiento relacionados, o una política sobre el objeto. Los objetos no se seleccionan en base a las anotaciones.

### ¿Qué más puedo hacer para preparar mi app para su despliegue?
{: #deploy_prep}

Muchas cosas. Consulte [Preparar su app contenerizada para ejecutarla en clústeres](/docs/containers?topic=containers-app#plan_apps). Este tema incluye información sobre:
*  Los tipos de apps que se pueden ejecutar en Kubernetes, incluyendo sugerencias para apps con estado y sin estado.
*  Migración de apps a Kubernetes.
*  Dimensionamiento del clúster según los requisitos de carga de trabajo.
*  Configuración de recursos de app adicionales, como por ejemplo servicios de IBM, registro de almacenamiento y supervisión.
*  Utilización de variables dentro del despliegue.
*  Control del acceso a la app.

<br />


## Empaquetado de la aplicación
{: #packaging}

Si desea ejecutar la app en varios clústeres, en entornos públicos y privados, o incluso en varios proveedores de nube, quizás se pregunte cómo puede hacer que su estrategia de despliegue funcione en estos entornos. Con {{site.data.keyword.Bluemix_notm}} y otras herramientas de código abierto, puede empaquetar la aplicación para ayudar a automatizar los despliegues.
{: shortdesc}

<dl>
<dt>Automatice la infraestructura</dt>
  <dd>Puede utilizar la herramienta de código abierto [Terraform](/docs/terraform?topic=terraform-getting-started#getting-started) para automatizar el suministro de la infraestructura de {{site.data.keyword.Bluemix_notm}}, incluyendo los clústeres de Kubernetes. Siga con esta guía de aprendizaje para [planificar, crear y actualizar entornos de despliegue](/docs/tutorials?topic=solution-tutorials-plan-create-update-deployments#plan-create-update-deployments). Después de crear un clúster, también puede configurar el [programa de escalado automático de clústeres de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ca) para que la agrupación de trabajadores escale hacia arriba y hacia abajo los nodos trabajadores en respuesta a las solicitudes de recursos de la carga de trabajo.</dd>
<dt>Configure un conducto de integración y entrega continuas (CI/CD)</dt>
  <dd>Con los archivos de configuración de la app organizados en un sistema de gestión de control de origen como, por ejemplo, Git, puede crear el conducto para probar y desplegar código en distintos entornos, como `test` y `prod`. Consulte [esta guía de aprendizaje sobre el despliegue continuo en Kubernetes](/docs/tutorials?topic=solution-tutorials-continuous-deployment-to-kubernetes#continuous-deployment-to-kubernetes).</dd>
<dt>Empaquete los archivos de configuración de la app</dt>
  <dd>Con el gestor de paquetes [Helm ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://helm.sh/docs/) de Kubernetes, puede especificar todos los recursos de Kubernetes que la app requiere en un diagrama de Helm. A continuación, puede utilizar Helm para crear los archivos de configuración de YAML y desplegar estos archivos en el clúster. También puede [integrar diagramas de Helm proporcionados por {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) para ampliar las posibilidades de su clúster, como por ejemplo, con un plugin de almacenamiento de bloques.<p class="tip">¿Está buscando una forma sencilla de crear plantillas de archivo YAML? Algunas personas utilizan Helm para hacerlo, o puede probar otras herramientas de la comunidad como [`ytt`![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://get-ytt.io/).</p></dd>
</dl>

<br />


## Mantener la app actualizada
{: #updating}

Ha puesto un montón de esfuerzo para prepararse para la próxima versión de su app. Puede utilizar herramientas de actualización de {{site.data.keyword.Bluemix_notm}} y de Kubernetes para asegurarse de que su app se está ejecutando en un entorno de clúster protegido, así como para implantar distintas versiones de su app.
{: shortdesc}

### ¿Cómo puedo mantener mi clúster en estado soportado?
{: #updating_kube}

Asegúrese de que el clúster ejecuta una [versión de Kubernetes soportada](/docs/containers?topic=containers-cs_versions#cs_versions) en todo momento. Cuando se libera una nueva versión menor de Kubernetes, una versión anterior está en desuso poco después y después pasa a ser no soportada. Para obtener más información, consulte [Actualización del maestro de Kubernetes](/docs/containers?topic=containers-update#master) y [nodos trabajadores](/docs/containers?topic=containers-update#worker_node).

### ¿Qué estrategias de actualización de apps puedo utilizar?
{: #updating_apps}

Para actualizar la app, puede elegir entre una variedad de estrategias como las siguientes. Puede empezar con un despliegue continuo o un conmutador instantáneo antes de avanzar hacia un despliegue de prueba más complicado.

<dl>
<dt>Despliegue</dt>
  <dd>Puede utilizar las funciones nativas de Kubernetes para crear un despliegue de la `v2` y sustituir gradualmente el despliegue anterior de la `v1`. Este enfoque requiere que las apps sean compatibles con versiones anteriores de modo que los usuarios a los que se sirve la versión `v2` de la app no experimenten ningún cambio radical. Para obtener más información, consulte [Gestión de despliegues continuos para actualizar las apps](/docs/containers?topic=containers-app#app_rolling).</dd>
<dt>Conmutación instantánea</dt>
  <dd>También conocido como un despliegue de azul-verde, una conmutación instantánea requiere el doble de recursos informáticos para tener dos versiones de una app en ejecución a la vez. Con este enfoque, puede conmutar a los usuarios a la versión más reciente en tiempo real. Asegúrese de utilizar los selectores de etiquetas de servicio (como `version: green` y `version: blue`) para asegurarse de que las solicitudes se envían a la versión de la app correcta. Puede crear el nuevo despliegue `version: green`, esperar a que esté preparado y, a continuación, suprimir el despliegue `version: blue`. O puede llevar a cabo una [actualización continua](/docs/containers?topic=containers-app#app_rolling), pero establezca el parámetro `maxUnavailable` a `0%` y el parámetro `maxSurge` a `100%`.</dd>
<dt>Despliegue de prueba o despliegue A/B</dt>
  <dd>Una estrategia de actualización más compleja, un despliegue de prueba es cuando se elige un porcentaje de usuarios como, por ejemplo, el 5% y se les envía la nueva versión de la app. Se recopilan métricas en las herramientas de registro y supervisión sobre cómo se funciona la nueva versión de la app, se hacen pruebas A/B y, a continuación, se implanta la actualización a más usuarios. Al igual que con todos los despliegues, el etiquetado de la app (como por ejemplo `version: stable` and `version: canary`) es crítico. Para gestionar despliegues de prueba, puede [instalar la red de servicios de complementos de Istio](/docs/containers?topic=containers-istio#istio), [configurar la supervisión de Sysdig para su clúster](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster), y después utilizar la red de servicios de Istio para las pruebas A/B como se describe [en esta publicación de blog ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://sysdig.com/blog/monitor-istio/). O bien, utilice Knative para despliegues de prueba.</dd>
</dl>

<br />


## Supervisión del rendimiento del clúster
{: #monitoring_health}

Con el registro y la supervisión efectivos del clúster y las apps, puede comprender mejor el entorno para optimizar la utilización de los recursos y resolver los problemas que puedan surgir. Para configurar las soluciones de registro y supervisión para el clúster, consulte [Registro y supervisión](/docs/containers?topic=containers-health#health).
{: shortdesc}

A medida que configure el registro y la supervisión, piense en las consideraciones siguientes.

<dl>
<dt>Recopilar registros y métricas para determinar la salud del clúster</dt>
  <dd>Kubernetes incluye un servidor de métricas para ayudar a determinar el rendimiento a nivel de clúster básico. Puede revisar estas métricas en el [panel de control de Kubernetes](/docs/containers?topic=containers-app#cli_dashboard) o en un terminal ejecutando los mandatos `kubectl top (pods | nodes)`. Puede incluir estos mandatos en la automatización.<br><br>
  Reenvíe los registros a una herramienta de análisis de registro para poder analizar los registros más adelante. Defina el nivel de detalle y el nivel de los registros que desea registrar para evitar el almacenamiento de más registros de los que necesita. Los registros pueden consumir rápidamente un montón de almacenamiento, lo que puede afectar el rendimiento de la app y puede hacer más difícil el análisis de registros.</dd>
<dt>Pruebe el rendimiento de la app</dt>
  <dd>Una vez que haya configurado el registro y la supervisión, lleve a cabo pruebas de rendimiento. En un entorno de prueba, cree deliberadamente una serie de escenarios no ideales, como por ejemplo suprimir todos los nodos trabajadores de una zona para replicar un error zonal. Revise los registros y las métricas para ver cómo se recupera la app.</dd>
<dt>Prepárese para las auditorías</dt>
  <dd>Además de los registros de apps y las métricas de clúster, desea configurar el seguimiento de actividades para tener un registro auditable de quién ha realizado las acciones de clúster y de Kubernetes. Para obtener más información, consulte [{{site.data.keyword.cloudaccesstrailshort}}](/docs/containers?topic=containers-at_events#at_events).</dd>
</dl>
