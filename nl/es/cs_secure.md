---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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


# Seguridad para {{site.data.keyword.containerlong_notm}}
{: #security}

Puede utilizar características integradas de seguridad en {{site.data.keyword.containerlong}} para análisis de riesgos y protección de la seguridad. Estas características le ayudan a proteger la infraestructura del clúster de Kubernetes y las comunicaciones en la red, a aislar sus recursos de cálculo y a garantizar la conformidad de la seguridad de los componentes de la infraestructura y de los despliegues de contenedores.
{: shortdesc}

## Visión general de las amenazas de seguridad para el clúster
{: #threats}

Para evitar que el clúster se vea comprometido, debe comprender las amenazas de seguridad potenciales para el clúster y lo que puede hacer para reducir la exposición a vulnerabilidades.
{: shortdesc}

<img src="images/cs_security_threats.png" width="400" alt="Amenazas de seguridad para el clúster" style="width:400px; border-style: none"/>

La seguridad en la nube y la protección de sus sistemas, infraestructura y datos frente a ataques se han convertido en asuntos de gran importantes en los últimos dos años, a medida que las empresas siguen trasladando sus cargas de trabajo a la nube pública. Un clúster consta de varios componentes, y cada uno puede poner en riesgo su entorno ante ataques maliciosos. Para proteger el clúster frente a estas amenazas de seguridad, debe asegurarse de que aplica las últimas características y actualizaciones de seguridad de {{site.data.keyword.containerlong_notm}} y Kubernetes en todos los componentes del clúster.

Estos componentes incluyen:
- [Servidor de API Kubernetes y almacén de datos etcd](#apiserver)
- [Nodos trabajadores](#workernodes)
- [Red](#network)
- [Almacén persistente](#storage)
- [Supervisión y registro](#monitoring_logging)
- [Registro e imágenes de contenedor](#images_registry)
- [Seguridad y aislamiento de contenedores](#container)
- [Información personal](#pi)

<br />


## Servidor de API Kubernetes y etcd
{: #apiserver}

El servidor de API Kubernetes y etcd son los componentes más vulnerables que se ejecutan en el nodo maestro de Kubernetes. Si un usuario o sistema no autorizado consigue acceder al servidor de API de Kubernetes, el usuario o sistema puede cambiar los valores, manipular o tomar el control del clúster, lo que pone el clúster en riesgo de sufrir ataques maliciosos.
{: shortdesc}

Para proteger el servidor de API Kubernetes y el almacén de datos etcd, debe proteger y limitar el acceso al servidor de API Kubernetes tanto a usuarios humanos como a cuentas del servicio Kubernetes.

**¿Cómo se otorga acceso a mi servidor de API Kubernetes?** </br>
De forma predeterminada, Kubernetes requiere que cada solicitud pase por varias etapas antes de que se conceda el acceso al servidor de API:

<ol><li><strong>Autenticación: </strong>valida la identidad de un usuario registrado o una cuenta de servicio.</li><li><strong>Autorización: </strong>limita los permisos de los usuarios autenticados y las cuentas de servicio para asegurarse de que pueden acceder y operar solo con los componentes de clúster que desee.</li><li><strong>Control de admisiones: </strong>valida o muta solicitudes antes de que las procese el servidor de API de Kubernetes. Muchas características de Kubernetes requieren controladores de admisiones para funcionar correctamente.</li></ol>

**¿Qué hace {{site.data.keyword.containerlong_notm}} para proteger mi servidor de API Kubernetes y el almacén de datos etcd?** </br>
En la imagen siguiente se muestran los valores de seguridad de clúster predeterminados que abordan la autenticación, la autorización, el control de admisiones y la conectividad segura entre el nodo maestro Kubernetes y los nodos trabajadores.

<img src="images/cs_security_apiserver_access.png" width="600" alt="Fases de seguridad cuando se accede al servidor de API de Kubernetes" style="width:600px; border-style: none"/>

<table>
<caption>Seguridad del servidor de API Kubernetes y de etcd</caption>
  <thead>
  <th>Característica de seguridad</th>
  <th>Descripción</th>
  </thead>
  <tbody>
    <tr>
      <td>Maestro de Kubernetes totalmente gestionado y dedicado</td>
      <td><p>Cada clúster de Kubernetes de {{site.data.keyword.containerlong_notm}} está controlado por un maestro de Kubernetes gestionado por IBM en una cuenta de infraestructura de IBM Cloud (SoftLayer) propiedad de IBM. El nodo maestro de Kubernetes se configura con los siguientes componentes dedicados que no se comparten con otros clientes de IBM.</p>
        <ul><li><strong>Almacén de datos de etcd:</strong> almacena todos los recursos de Kubernetes de un clúster, como `Servicios`, `Despliegues` y `Pods`. Los `ConfigMaps` y los `Secretos` de Kubernetes son datos de app que se almacenan como pares de clave y valor para que los pueda utilizar una app que se ejecuta en un pod. Los datos de etcd se almacenan en el disco local del maestro de Kubernetes y se realiza una copia de seguridad en
{{site.data.keyword.cos_full_notm}}. Los datos se cifran durante el tránsito a {{site.data.keyword.cos_full_notm}} y en reposo. Puede optar por habilitar el cifrado de los datos de etcd en el disco local del maestro de Kubernetes mediante la [habilitación del cifrado de {{site.data.keyword.keymanagementservicelong_notm}}](/docs/containers?topic=containers-encryption#encryption) para el clúster. Los datos de etcd para clústeres que ejecutan una versión anterior de Kubernetes, de los que se hace una copia de seguridad diaria, se almacenan en un disco cifrado gestionado por IBM. Cuando los datos de etcd se envían a un pod, los datos se cifran mediante TLS para garantizar la protección y la integridad de los datos.</li>
          <li><strong>kube-apiserver:</strong> sirve como punto de entrada principal para todas las solicitudes de gestión del clúster procedentes del nodo trabajador destinadas al maestro de Kubernetes. kube-apiserver valida y procesa las solicitudes y puede leer y escribir en el almacén de datos etcd.</li>
          <li><strong>kube-scheduler:</strong> decide dónde desplegar los pods, considerando los requisitos de capacidad y de rendimiento, las restricciones de política de hardware y de software, las especificaciones anti afinidad y los requisitos de carga de trabajo. Si no se encuentra ningún nodo trabajador que se ajuste a los requisitos, el pod no se despliega en el clúster.</li>
          <li><strong>kube-controller-manager:</strong> responsable de supervisar los conjuntos de réplicas y de crear los pods correspondientes para alcanzar el estado especificado.</li>
          <li><strong>OpenVPN:</strong> componente específico de {{site.data.keyword.containerlong_notm}} que proporciona conectividad de red segura para la comunicación entre el maestro de Kubernetes y el nodo trabajador. La comunicación entre el nodo maestro de Kubernetes y el nodo trabajador la inicia el usuario e incluye mandatos <code>kubectl</code>, como <code>logs</code>, <code>attach</code>, <code>exec</code> y <code>top</code>.</li></ul></td>
    </tr>
    <tr>
    <td>Supervisión continua por parte de los ingenieros de fiabilidad del sitio (SRE) de IBM</td>
    <td>El nodo maestro de Kubernetes, incluidos todos los componentes maestros y los recursos de cálculo, de red y de almacenamiento, reciben supervisión continua por parte de los ingenieros de fiabilidad del sitio (SRE) de IBM. Los SRE aplican los estándares de seguridad más recientes, detectan y solucionan actividades maliciosas y trabajan para garantizar la fiabilidad y la disponibilidad de {{site.data.keyword.containerlong_notm}}. </td>
    </tr>
    <tr>
      <td>Comunicación segura a través de TLS</td>
      <td>Para utilizar {{site.data.keyword.containerlong_notm}}, debe autenticarse con el servicio utilizando sus credenciales. Cuando se ha autenticado, {{site.data.keyword.containerlong_notm}} genera certificados TLS que cifran la comunicación a y desde el servidor de API de Kubernetes y el almacén de datos etcd para garantizar una comunicación segura de extremo a extremo entre los nodos trabajadores y el nodo maestro de Kubernetes. Estos certificados nunca se comparten entre clústeres ni entre componentes de maestro de Kubernetes. </td>
    </tr>
    <tr>
      <td>Conectividad OpenVPN con nodos trabajadores</td>
      <td>Aunque Kubernetes protege la comunicación entre el maestro de Kubernetes y los nodos trabajadores mediante el protocolo <code>https</code>, no se proporciona autenticación en el nodo trabajador de forma predeterminada. Para proteger esta comunicación, {{site.data.keyword.containerlong_notm}} configura automáticamente una conexión OpenVPN entre el maestro de Kubernetes y el nodo trabajador cuando se crea el clúster.</td>
    </tr>
    <tr>
      <td>Control de acceso preciso</td>
      <td>Como administrador de la cuenta, puede [otorgar acceso a otros usuarios para {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-users#users) utilizando {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). {{site.data.keyword.Bluemix_notm}} IAM proporciona autenticación segura con la plataforma {{site.data.keyword.Bluemix_notm}}, {{site.data.keyword.containerlong_notm}} y todos los recursos de la cuenta. La configuración de los roles y permisos de usuario adecuados resulta clave para limitar quién puede acceder a sus recursos y limitar el daño que puede hacer un usuario cuando se utilizan mal los permisos legítimos. </br></br>Puede seleccionar entre los siguientes roles de usuario predefinidos que determinan el conjunto de acciones que el usuario puede realizar: <ul><li><strong>Roles de plataforma:</strong> determinan las acciones relacionadas con el clúster y el nodo trabajador que un usuario puede realizar en {{site.data.keyword.containerlong_notm}}.</li><li><strong>Roles de infraestructura:</strong> determinan los permisos para solicitar, actualizar o eliminar recursos de infraestructura como, por ejemplo, nodos trabajadores, VLAN o subredes.</li><li><strong>Roles de RBAC de Kubernetes:</strong> determinan los mandatos `kubectl` que los usuarios pueden ejecutar cuando están autorizados para acceder a un clúster. Los roles de RBAC se configuran automáticamente para el espacio de nombres predeterminado de un clúster. Para utilizar los mismos roles de RBAC en otros espacios de nombres, puede copiar los roles de RBAC del espacio de nombres predeterminado.  </li></ul> </br> En lugar de utilizar roles de usuario predefinidos, puede optar por [personalizar los permisos de infraestructura](/docs/containers?topic=containers-users#infra_access) o [configurar sus propios roles de RBAC](/docs/containers?topic=containers-users#rbac) para añadir un control de acceso más preciso. </td>
    </tr>
    <tr>
      <td>Controladores de admisiones</td>
      <td>Los controladores de admisiones se implementan para características específicas en Kubernetes y {{site.data.keyword.containerlong_notm}}. Con los controladores de admisiones puede configurar políticas en el clúster que determinen si se permite o no una acción concreta en el clúster. En la política, puede especificar condiciones bajo las que un usuario no puede realizar una acción, incluso si esta acción forma parte de los permisos generales que ha asignado al usuario utilizando RBAC. Por lo tanto, los controladores de admisiones pueden proporcionar una capa adicional de seguridad para el clúster antes de que el servidor de API Kubernetes procese una solicitud de API. </br></br> Cuando crea un clúster, {{site.data.keyword.containerlong_notm}} instala automáticamente los siguientes [controladores de admisiones de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/admin/admission-controllers/) en el nodo maestro de Kubernetes, que el usuario no puede cambiar: <ul>
      <li>`DefaultTolerationSeconds`</li>
      <li>`DefaultStorageClass`</li>
      <li>`GenericAdmissionWebhook`</li>
      <li>`Initializers` (Kubernetes 1.13 o anteriores)</li>
      <li>`LimitRanger`</li>
      <li>`MutatingAdmissionWebhook`</li>
      <li>`NamespaceLifecycle`</li>
      <li>`NodeRestriction` (Kubernetes 1.14 o posteriores)</li>
      <li>`PersistentVolumeLabel`</li>
      <li>[`PodSecurityPolicy`](/docs/containers?topic=containers-psp#ibm_psp)</li>
      <li>[`Priority`](/docs/containers?topic=containers-pod_priority#pod_priority) (Kubernetes 1.11 o posteriores)</li>
      <li>`ResourceQuota`</li>
      <li>`ServiceAccount`</li>
      <li>`StorageObjectInUseProtection`</li>
      <li>`TaintNodesByCondition` (Kubernetes 1.12 o posteriores)</li>
      <li>`ValidatingAdmissionWebhook`</li></ul></br>
      Puede [instalar sus propios controladores de admisiones en el clúster ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/#admission-webhooks) o puede elegir entre los controladores de admisiones opcionales que proporciona {{site.data.keyword.containerlong_notm}}: <ul><li><strong>[Container Image Security Enforcer](/docs/services/Registry?topic=registry-security_enforce#security_enforce):</strong> utilice este controlador de admisiones para imponer políticas de Vulnerability Advisor en el clúster para bloquear despliegues procedentes de imágenes vulnerables.</li></ul></br><p class="note">Si ha instalado manualmente controladores de admisiones y ya no desea utilizarlos, asegúrese de eliminarlos por completo. Si los controladores de admisiones no se eliminan por completo, es posible que bloqueen todas las acciones que desee realizar en el clúster.</p></td>
    </tr>
  </tbody>
</table>

**¿Qué más puedo hacer para proteger mi servidor de API de Kubernetes?**</br>

Si el clúster está conectado a una VLAN privada y pública, {{site.data.keyword.containerlong_notm}} establece automáticamente una conexión OpenVPN segura entre el nodo maestro y los trabajadores del clúster a través de un punto final de servicio público. Si VRF está habilitado en la cuenta de {{site.data.keyword.Bluemix_notm}}, puede permitir que los nodos maestro y trabajadores del clúster se comuniquen sobre la red privada a través de un punto final de servicio privado.

Los puntos finales de servicio determinan cómo pueden acceder los nodos trabajadores y los usuarios del clúster al nodo maestro del clúster.
* Solo punto final de servicio público: Se establece una conexión OpenVPN segura entre el nodo maestro del clúster y los nodos trabajadores a través de la red pública. Los usuarios del clúster pueden acceder de forma pública al nodo maestro.
* Puntos finales de servicio públicos y privados:
La comunicación se establece tanto sobre la red privada a través del punto final de servicio privado como sobre la red pública a través del punto final de servicio público. Direccionando la mitad del tráfico de trabajador a maestro por el punto final público y la otra mitad por el punto final privado, la comunicación de maestro a trabajador está protegida de posibles interrupciones en la red pública o privada. Se puede acceder de forma privada al nodo maestro a través del punto final de servicio privado si los usuarios autorizados del clúster están en la red privada de {{site.data.keyword.Bluemix_notm}} o están conectados a la red privada a través de una conexión VPN. Si no es así, los usuarios autorizados del clúster pueden acceder de forma pública al nodo maestro a través del punto final de servicio público.
* Solo punto final de servicio privado: La comunicación entre los nodos maestro y trabajadores se establece a través de la red privada. Los usuarios del clúster deben estar en la red privada de {{site.data.keyword.Bluemix_notm}} o deben conectarse a la red privada a través de una conexión VPN para poder acceder al nodo maestro.

Para obtener más información sobre los puntos finales de servicio, consulte [Comunicación entre nodo trabajador y maestro y entre usuario y maestro](/docs/containers?topic=containers-plan_clusters#workeruser-master).

<br />


## Nodo trabajador
{: #workernodes}

Los nodos trabajadores se ocupan de los despliegues y los servicios que forman la app. Si aloja las cargas de trabajo en la nube pública, desea asegurarse de que la app está protegido frente al acceso, modificación y supervisión por parte de un usuario o software no autorizado.
{: shortdesc}

**¿Quién es el propietario del nodo trabajador y soy yo el responsable de protegerlo?** </br>
La propiedad de un nodo trabajador depende del tipo de clúster que haya creado. Los nodos trabajadores de los clústeres gratuitos se suministran a la cuenta de infraestructura de IBM Cloud (SoftLayer) que es propiedad de IBM. Puede desplegar apps en nodos trabajadores, pero no pueden cambiar los valores ni instalar software adicional en el nodo trabajador. Debido a la capacidad limitada y a las características limitadas de {{site.data.keyword.containerlong_notm}}, no ejecute cargas de trabajo de producción en clústeres gratuitos. Considere la posibilidad de utilizar clústeres estándares para las cargas de producción.

Los nodos trabajadores de los clústeres estándares se suministran en la cuenta de infraestructura de IBM Cloud (SoftLayer) asociada con su cuenta de {{site.data.keyword.Bluemix_notm}} pública o dedicada. Los nodos trabajadores están dedicados a su cuenta y usted es el responsable de solicitar actualizaciones puntuales para los nodos trabajadores para asegurarse de que el sistema operativo y los componentes de {{site.data.keyword.containerlong_notm}} aplican las últimas actualizaciones de seguridad y parches.

Utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update` de forma regular (por ejemplo, una vez al mes) para desplegar actualizaciones y parches de seguridad en el sistema operativo y para actualizar la versión de Kubernetes. Cuando hay actualizaciones disponibles, se le notifica cuando visualiza información sobre los nodos maestro y trabajador en la CLI o consola de {{site.data.keyword.Bluemix_notm}}, por ejemplo con los mandatos `ibmcloud ks clusters` o `ibmcloud ks workers --cluster <cluster_name>`. IBM proporciona las actualizaciones de nodo trabajador como una imagen de nodo trabajador completo que incluye los parches de seguridad más recientes. Para aplicar las actualizaciones, se debe volver a crear una imagen del nodo trabajador y se debe volver a cargar con la nueva imagen. Las claves para el usuario root rotan automáticamente cuando se vuelve a cargar el nodo trabajador.
{: important}

**¿Qué aspecto tiene la configuración de mi nodo trabajador?**</br>
En la imagen siguiente se muestran los componentes que se configuran para cada nodo trabajador para proteger el nodo trabajador frente a ataques maliciosos.

La imagen no incluye componentes que garanticen la comunicación segura de extremo a extremo a y desde el nodo trabajador. Consulte [seguridad de red](#network) para obtener más información.
{: note}

<img src="images/cs_worker_setup.png" width="600" alt="Configuración de nodo trabajador (sin incluir seguridad de red)" style="width:600px; border-style: none"/>

<table>
<caption>Configuración de la seguridad del nodo trabajador</caption>
  <thead>
  <th>Característica de seguridad</th>
  <th>Descripción</th>
  </thead>
  <tbody>
    <tr><td>Imagen de Linux conforme con CIS</td><td>Cada nodo trabajador se configura con un sistema operativo Ubuntu que implementa los puntos de referencia publicados por el Centro de seguridad de Internet (CIS). Ni el usuario ni el propietario de la máquina pueden modificar el sistema operativo Ubuntu. Para comprobar la versión de Ubuntu actual, ejecute <code>kubectl get nodes -o wide</code>. IBM trabaja con equipos de asesoramiento de seguridad interna y externa para abordar las vulnerabilidades potenciales de conformidad de seguridad. Las actualizaciones de seguridad y los parches para el sistema operativo están disponibles a través de {{site.data.keyword.containerlong_notm}} y los debe instalar el usuario para mantener seguro el nodo trabajador.<p class="important">{{site.data.keyword.containerlong_notm}} utiliza un kernel de Ubuntu Linux para nodos trabajadores. Puede ejecutar contenedores basados en cualquier distribución de Linux en {{site.data.keyword.containerlong_notm}}. Verifique con el proveedor de la imagen de contenedor si da soporte a la ejecución de la imagen de contenedor en los kernels de Ubuntu Linux.</p></td></tr>
    <tr>
    <td>Supervisión continua por parte de los ingenieros de fiabilidad del sitio (SRE) </td>
    <td>La imagen de Linux instalada en los nodos trabajadores recibe supervisión continua por parte de los ingenieros de fiabilidad del sitio (SRE) de IBM para detectar vulnerabilidades y problemas de conformidad de seguridad. Para hacer frente a las vulnerabilidades, los SRE crean parches de seguridad y fixpacks para los nodos trabajadores. Asegúrese de aplicar estos parches cuando estén disponibles para garantizar un entorno seguro para los nodos trabajadores y las apps que se ejecutan sobre los mismos.</td>
    </tr>
    <tr>
  <td>Aislamiento de cálculo</td>
  <td>Los nodos trabajadores están dedicados a un clúster y no albergan cargas de trabajo de otros clústeres. Cuando crea un clúster estándar, puede optar por suministrar a los nodos trabajadores como [máquinas físicas (nativas) o máquinas virtuales](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) que se ejecutan en hardware físico compartido o dedicado. El nodo trabajador de un clúster gratuito se suministra automáticamente como nodo compartido virtual en la cuenta de infraestructura de IBM Cloud (SoftLayer).</td>
</tr>
<tr>
<td>Opción para despliegue nativo</td>
<td>Si elige suministrar los nodos trabajadores en servidores físicos nativos (en lugar de instancias de servidor virtual), tiene más control sobre el host de cálculo, como por ejemplo la memoria o la CPU. Esta configuración elimina el hipervisor de máquina virtual que asigna recursos físicos a máquinas virtuales que se ejecutan en el host. En su lugar, todos los recursos de una máquina nativa están dedicados exclusivamente al trabajador, por lo que no es necesario preocuparse por "vecinos ruidosos" que compartan recursos o ralenticen el rendimiento. Los servidores nativos están dedicados a usted, con todos los recursos disponibles para que los utilice el clúster.</td>
</tr>
<tr>
  <td id="trusted_compute">Opción para cálculo de confianza</td>
    <td>Si despliega el clúster en un sistema nativo que da soporte a Trusted Compute (cálculo de confianza), puede [habilitar la confianza](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable). El chip de TPM (módulo de plataforma de confianza) está habilitado en cada nodo trabajador nativo en el clúster compatible con Trusted Compute (incluidos los nodos futuros que añada al clúster). Por consiguiente, después de habilitar la confianza, no puede inhabilitarla posteriormente para el clúster. Se despliega un servidor de confianza en el nodo maestro, y se despliega un agente de confianza como un pod en el nodo trabajador. Cuando el nodo trabajador se inicia, el pod del agente de confianza supervisa cada etapa del proceso.<p>El hardware es la base de un sistema fiable, que envía medidas mediante TPM. TPM genera las claves de cifrado que se utilizan para proteger la transmisión de datos sobre medidas durante el proceso. El agente de confianza transmite al servidor de confianza la medida de cada componente en el proceso de arranque: desde el firmware de BIOS que interactúa con el hardware TPM hasta el cargador de arranque y el kernel del sistema operativo. Luego, el agente de confianza compara estas medidas con los valores esperados en el servidor de confianza para evaluar si el arranque ha sido válido. El proceso de cálculo de confianza no supervisa otros pods de los nodos trabajadores, como pueden ser aplicaciones.</p><p>Por ejemplo, si un usuario no autorizado accede a su sistema y modifica el kernel del sistema operativo con lógica adicional para recopilar datos, el agente de confianza detecta este cambio y marca el nodo como de no confianza. Con el cálculo de confianza, puede proteger los nodos trabajadores frente a una manipulación indebida.</p>
    <p class="note">Trusted Compute está disponible para determinados tipos de máquinas nativas. Por ejemplo, los distintos tipos de GPU `mgXc` no dan soporte a Trusted Compute.</p>
    <p><img src="images/trusted_compute.png" alt="Cálculo de confianza para clústeres nativos" width="480" style="width:480px; border-style: none"/></p></td>
  </tr>
    <tr>
  <td id="encrypted_disk">Discos cifrados</td>
    <td>De forma predeterminada, cada nodo trabajador se suministra con dos particiones de datos cifrados SSD locales AES de 256 bits. La primera partición contiene la imagen de kernel que se utiliza para arrancar el nodo trabajador y que no está cifrada. La segunda partición contiene el sistema de archivos de contenedor y se desbloquea mediante las claves de cifrado LUKS. Cada nodo trabajador de cada clúster de Kubernetes tiene su propia clave de cifrado LUKS, que gestiona {{site.data.keyword.containerlong_notm}}. Cuando se crea un clúster o se añade un nodo trabajador a un clúster existente, las claves se extraen de forma segura y luego se descartan una vez desbloqueado el disco cifrado. <p class="note">El cifrado puede afectar al rendimiento de E/S de disco. Para las cargas de trabajo que requieren E/S de disco de alto rendimiento, pruebe un clúster con cifrado habilitado e inhabilitado para decidir mejor si desactiva el cifrado.</p></td>
      </tr>
    <tr>
      <td>Políticas AppArmor de experto</td>
      <td>Cada nodo trabajador se configura con políticas de seguridad y de acceso que se imponen mediante perfiles de [AppArmor ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://wiki.ubuntu.com/AppArmor), que se cargan en el nodo trabajador durante el programa de carga. Ni el usuario ni el propietario de la máquina pueden modificar los perfiles de AppArmor. </td>
    </tr>
    <tr>
      <td>SSH inhabilitado</td>
      <td>De forma predeterminada, el acceso SSH está inhabilitado en el nodo trabajador para proteger el clúster frente a ataques maliciosos. Cuando el acceso SSH está inhabilitado, el acceso al clúster se impone mediante el servidor de API de Kubernetes. El servidor de API de Kubernetes requiere que cada solicitud se compruebe con las políticas establecidas en la autenticación, autorización y módulo de control de admisiones antes de que la solicitud se ejecute en el clúster. </br></br>  Si tiene un clúster estándar y desea instalar más características en el nodo trabajador, puede elegir entre los complementos proporcionados por {{site.data.keyword.containerlong_notm}} o utilizar [conjuntos de daemons de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) para cualquier cosa que desee ejecutar en cada nodo trabajador. Para cualquier acción individual que deba ejecutar, utilice los [Trabajos de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/).</td>
    </tr>
  </tbody>
  </table>

<br />


## Red
{: #network}
El enfoque clásico para proteger la red de una empresa consiste en configurar un cortafuegos y bloquear todo el tráfico de red no deseado dirigido a sus apps. Aunque esto sigue vigente, recientes investigaciones muestran que muchos ataques malintencionados provienen de personal interno o de usuarios autorizados que hacer un mal uso de sus permisos asignados.
{: shortdesc}

Para proteger la red y limitar el rango de daños que puede infligir un usuario cuando se otorga el acceso a una red, debe asegurarse de que las cargas de trabajo estén tan aisladas en la medida de lo posible y de que limite el número de apps y nodos trabajadores que están expuestos públicamente.

**¿Qué tráfico de red está permitido para mi clúster de forma predeterminada?**</br>
Todos los contenedores están protegidos por [valores predefinidos de política de red de Calico](/docs/containers?topic=containers-network_policies#default_policy)
que se configuran en cada nodo trabajador durante la creación del clúster. De forma predeterminada, todo el tráfico de red de salida está permitido para todos los nodos trabajadores. El tráfico de red de entrada se bloquea excepto en algunos puertos abiertos para que IBM pueda supervisar el tráfico de la red y pueda instalar automáticamente las actualizaciones de seguridad en el nodo maestro de Kubernetes. El acceso desde el nodo maestro de Kubernetes al kubelet del nodo trabajador está protegido por un túnel OpenVPN. Para obtener más información, consulte la [arquitectura de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology).

Si desea permitir el tráfico de red entrante desde Internet, debe exponer sus apps con un [servicio NodePort, un equilibrador de carga de red (NLB) o un equilibrador de carga de aplicación (ALB) de Ingress](/docs/containers?topic=containers-cs_network_planning#external).  

{: #network_segmentation}
**¿Qué es la segmentación de red y cómo puedo configurarla para un clúster?** </br>
La segmentación de red describe el enfoque para dividir una red en varias subredes. Puede agrupar apps y datos relacionados a los que vaya a acceder un determinado grupo de la organización. Las apps que se ejecutan en una subred no pueden ver ni acceder a las apps de otra subred. La segmentación de red también limita el acceso que se proporciona a un software interno o de terceros y puede limitar el rango de actividades maliciosas.   

{{site.data.keyword.containerlong_notm}} proporciona VLAN de infraestructura de IBM Cloud (SoftLayer) que garantizan un rendimiento de red de calidad y el aislamiento de red de los nodos trabajadores. Una VLAN configura un grupo de nodos trabajadores y pods como si estuvieran conectadas a la misma conexión física. Las VLAN están dedicadas a su
cuenta de {{site.data.keyword.Bluemix_notm}} y no se comparten entre los clientes de IBM. Si tiene varias VLAN para un clúster, varias subredes en la misma VLAN o un clúster multizona, debe habilitar la [función de direccionador virtual (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para la cuenta de infraestructura de IBM Cloud (SoftLayer) para que los nodos trabajadores puedan comunicarse entre sí en la red privada. Para habilitar VRF, [póngase en contacto con el representante de su cuenta de la infraestructura de IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si no puede o no desea habilitar VRF, habilite la [distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para llevar a cabo esta acción, necesita el [permiso de la infraestructura](/docs/containers?topic=containers-users#infra_access) **Red > Gestionar distribución de VLAN de red** o bien puede solicitar al propietario de la cuenta que lo habilite. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.

Si habilita VRF o la distribución de VLAN para la cuenta, la segmentación de red se elimina de los clústeres.

Revise la siguiente tabla para ver las opciones sobre cómo conseguir la segmentación de red cuando se habilita VRF o la distribución de VLAN para la cuenta.

|Característica de seguridad|Descripción|
|-------|----------------------------------|
|Configuración de políticas de red personalizadas con Calico|Puede utilizar la interfaz integrada de Calico para [configurar políticas de red de Calico personalizadas](/docs/containers?topic=containers-network_policies#network_policies) para los nodos trabajadores. Por ejemplo, puede permitir o bloquear el tráfico de red en interfaces de red específicas, para pods específicos o servicios. Para configurar políticas de red personalizadas, debe [instalar la CLI <code>calicoctl</code>](/docs/containers?topic=containers-network_policies#cli_install).|
|Soporte para cortafuegos de red de infraestructura de IBM Cloud (SoftLayer)|{{site.data.keyword.containerlong_notm}} es compatible con todas las [ofertas de cortafuegos de infraestructura de IBM Cloud (SoftLayer) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud-computing/bluemix/network-security). En {{site.data.keyword.Bluemix_notm}} Público, puede configurar un cortafuegos con políticas de red personalizadas para proporcionar seguridad de red dedicada para el clúster estándar y para detectar y solucionar problemas de intrusión en la red. Por ejemplo, podría elegir configurar un [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) para que actuase como cortafuegos para bloquear el tráfico no deseado. Si configura un cortafuegos, [también debe abrir los puertos y direcciones IP necesarios](/docs/containers?topic=containers-firewall#firewall) para cada región para que los nodos maestro y trabajador se puedan comunicar.|
{: caption="Opciones de segmentación de red" caption-side="top"}

**¿Qué otra cosa puedo hacer para reducir la superficie de ataques externos?**</br>
Cuantas más apps o nodos trabajadores exponga públicamente, más pasos debe llevar a cabo para evitar ataques maliciosos externos. Revise la siguiente tabla para ver las opciones disponibles para mantener la privacidad de sus apps y nodos trabajadores.

|Característica de seguridad|Descripción|
|-------|----------------------------------|
|Limitación del número de apps expuestas|De forma predeterminada, no se puede acceder sobre internet público a las apps y servicios que se ejecutan en el clúster. Puede elegir si desea exponer sus apps al público, o si desea que solo se pueda acceder a sus apps y servicios en la red privada. Si mantiene la privacidad de las apps y los servicios, puede aprovechar las características de seguridad integradas para garantizar una comunicación segura entre nodos trabajadores y pods. Para exponer servicios y apps a Internet pública, puede aprovechar el [soporte de NLB y ALB de Ingress](/docs/containers?topic=containers-cs_network_planning#external) para poner los servicios a disponibilidad pública de forma segura. Asegúrese de que solo se expongan los servicios necesarios y vuelva a visitar la lista de apps expuesta de forma regular para asegurarse de que siguen siendo válidas. |
|Mantenimiento de la privacidad de los nodos trabajadores|Cuando crea un clúster, cada clúster se conecta automáticamente a una VLAN privada. La VLAN privada determina la dirección IP privada que se asigna a un nodo trabajador. Puede optar por mantener la privacidad de los nodos trabajadores conectándolos solo a una VLAN privada. Las VLAN privadas en clústeres gratuitos están gestionadas por IBM, y las VLAN privadas de los clústeres estándares se gestionan mediante la cuenta de infraestructura de IBM Cloud (SoftLayer). </br></br><strong>Atención:</strong> tenga en cuenta que, para comunicarse con el nodo maestro de Kubernetes y para que {{site.data.keyword.containerlong_notm}} funcione correctamente, debe configurar la conectividad pública con [URL y direcciones IP específicos](/docs/containers?topic=containers-firewall#firewall_outbound). Para configurar esta conectividad pública, puede configurar un cortafuegos, como un [dispositivo direccionador virtual](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra), frente a los nodos trabajadores y habilitar el tráfico de red a estos URL y direcciones IP.|
|Limitación de la conectividad de Internet pública con los nodos de extremo|De forma predeterminada, cada nodo trabajador se configura de modo que acepte pods de app y pods de ingress o de equilibrador de carga asociados. Puede etiquetar los nodos trabajadores como [nodos de extremo](/docs/containers?topic=containers-edge#edge) para forzar el despliegue del equilibrador de carga y de los pods ingress solo en estos nodos trabajadores. Además, puede [establecer como antagónicos los nodos trabajadores](/docs/containers?topic=containers-edge#edge_workloads) de modo que los pods de app no se puedan planificar en los nodos de extremo. Con nodos de extremo puede aislar la carga de trabajo de red en menos nodos trabajadores en el clúster y mantener la privacidad de los otros nodos trabajadores del clúster.|
{: caption="Opciones de servicios privados y de nodos trabajadores" caption-side="top"}

**¿Qué ocurre si deseo conectar mi clúster a un centro de datos local?**</br>
Para conectar los nodos trabajadores y las apps a un centro de datos local, puede configurar un [punto final VPN IPSec con un servicio strongSwan, un dispositivo direccionador virtual o un dispositivo de seguridad Fortigate](/docs/containers?topic=containers-vpn#vpn).

### Servicios LoadBalancer e Ingress
{: #network_lb_ingress}

Puede utilizar los servicios de red de equilibrador de carga de red (NLB) y de equilibrador de carga de aplicación de Ingress (ALB) para conectar sus apps a internet público o a redes privadas externas. Revise los siguientes valores opcionales para los NLB y los ALB que puede utilizar para satisfacer los requisitos de seguridad de las apps de fondo o para cifrar el tráfico a medida que circula a través del clúster.
{: shortdesc}

**¿Puedo utilizar grupos de seguridad para gestionar el tráfico de red de mi clúster?** </br>
Para utilizar los servicios de NLB y ALB de Ingress, utilice [políticas de Calico y Kubernetes](/docs/containers?topic=containers-network_policies) para gestionar el tráfico de red de entrada y salida del clúster. No utilice [grupos de seguridad](/docs/infrastructure/security-groups?topic=security-groups-about-ibm-security-groups#about-ibm-security-groups) de la infraestructura de IBM Cloud (SoftLayer). Los grupos de seguridad de la infraestructura de IBM Cloud (SoftLayer) se aplican a la interfaz de red de un solo servidor virtual para filtrar el tráfico en el nivel de hipervisor. Sin embargo, los grupos de seguridad no dan soporte al protocolo VRRP, que {{site.data.keyword.containerlong_notm}} utiliza para gestionar la dirección IP de NLB. Si no dispone del protocolo VRRP para gestionar la IP de NLB, los servicios de NLB y ALB de Ingress no funcionan correctamente. Si no utiliza los servicios de NLB y ALB de Ingress y desea aislar completamente el nodo trabajador del público, puede utilizar grupos de seguridad.

**¿Cómo puedo proteger la IP de origen dentro del clúster?** </br>
En los NLB de la versión 2.0, de forma predeterminada se conserva la dirección IP de origen de la solicitud de cliente. No obstante, en los NLB de la versión 1.0 NLB y en todos los ALB de Ingress, la dirección IP de origen de la solicitud de cliente no se conserva. Cuando se envía al clúster una solicitud de cliente para la app, a solicitud se direcciona a un pod para el NLB 1.0 o el ALB. Si no existen pods de app en el mismo nodo trabajador que el pod del servicio equilibrador de carga, el NLB o el ALB reenvía las solicitudes a un pod de app en un nodo trabajador diferente. La dirección IP de origen del paquete se cambia a la dirección IP pública del nodo trabajador donde el pod de app está en ejecución.

Conservar la dirección IP del cliente es útil, por ejemplo, cuando los servidores de apps tienen que aplicar políticas de control de acceso y seguridad. Para conservar la dirección IP de origen original de la solicitud de cliente, puede habilitar la conservación de IP de origen para [los NLB de la versión 1.0](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) o [los ALB de Ingress](/docs/containers?topic=containers-ingress#preserve_source_ip).

**¿Cómo puedo cifrar el tráfico con TLS?** </br>
El servicio Ingress ofrece terminación de TLS en dos puntos en el flujo del tráfico:
* [Descifrado de paquete al llegar](/docs/containers?topic=containers-ingress#public_inside_2): de forma predeterminada, el ALB Ingress equilibra la carga del tráfico de red HTTP con las apps del clúster. Para equilibrar también la carga de conexiones HTTPS entrantes, puede configurar el ALB para descifrar el tráfico de red y reenviar las solicitudes descifradas a las apps expuestas en su clúster. Si está utilizando el subdominio de Ingress proporcionado por IBM, puede utilizar el certificado TLS proporcionado por IBM. Si utiliza un dominio personalizado, puede utilizar su propio certificado TLS para gestionar la terminación TLS.
* [Recifrado de paquete antes de que se reenvíe
a las apps en sentido ascendente](/docs/containers?topic=containers-ingress_annotation#ssl-services): el ALB descifra las solicitudes HTTPS antes de reenviar el tráfico a las apps. Si tiene apps que requieren HTTPS y necesita que el tráfico se cifre antes de que se reenvíen a dichas apps en sentido ascendente, puede utilizar la anotación ssl-services. Si sus apps en sentido ascendente pueden manejar TLS, puede proporcionar de forma opcional un certificado contenido en un secreto TLS de autenticación mutua o de un solo sentido.

Para proteger la comunicación de servicio a servicio, puede utilizar la [autenticación TLS mutua de Istio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/concepts/security/mutual-tls/). Istio es un servicio de código fuente abierto que ofrece a los desarrolladores una forma de conectarse, proteger, gestionar y supervisar una red de microservicios, también conocida como red de servicios, en plataformas de orquestación de nube como Kubernetes.

<br />


## Almacén persistente
{: #storage}

Cuando se suministra almacenamiento persistente para almacenar datos en el clúster, los datos se cifran automáticamente sin coste adicional cuando se almacenan en la compartición de archivos o en el almacenamiento en bloque. El cifrado incluye instantáneas y almacenamiento replicado.
{: shortdesc}

Para obtener más información sobre cómo se cifran los datos para el tipo de almacenamiento específico, consulte los siguientes enlaces.
- [Almacenamiento de archivos NFS](/docs/infrastructure/FileStorage?topic=FileStorage-encryption#encryption)
- [Almacenamiento en bloque](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption#block-storage-encryption-at-rest) </br>

También puede utilizar un servicio de base de datos de {{site.data.keyword.Bluemix_notm}}, como por ejemplo [{{site.data.keyword.cloudant}} NoSQL DB](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started), para persistir datos en una base de datos gestionada fuera del clúster. Se puede acceder a los datos que se almacenan con un servicio de base de datos de nube a través de clústeres, zonas y regiones. Para obtener información relacionada con la seguridad acerca de IBM Cloudant NoSQL DB, consulte la [documentación del servicio](/docs/services/Cloudant/offerings?topic=cloudant-security#security).

<br />


## Supervisión y registro
{: #monitoring_logging}

La clave para detectar ataques maliciosos en el clúster es la supervisión y el registro adecuados de las métricas y de todos los sucesos que se producen en el clúster. La supervisión y el registro también pueden ayudarle a comprender la capacidad de clúster y la disponibilidad de los recursos para su app, lo que le ayudará a realizar una planificación consecuente para proteger sus apps frente a un tiempo de inactividad.
{: shortdesc}

**¿Supervisa IBM mi clúster?**</br>
IBM supervisa continuamente cada nodo maestro de Kubernetes para controlar y solucionar los ataques de tipo denegación de servicio (DOS) a nivel de proceso. {{site.data.keyword.containerlong_notm}} explora automáticamente cada nodo en el que se ha desplegado el nodo maestro de Kubernetes en busca de vulnerabilidades y arreglos de seguridad específicos de Kubernetes y del sistema operativo. Si se encuentran vulnerabilidades, {{site.data.keyword.containerlong_notm}} aplica automáticamente los arreglos y soluciona las vulnerabilidades en nombre del usuario para asegurarse de la protección del nodo maestro.  

**¿Qué información se registra?**</br>
Para los clústeres estándares, puede [configurar el reenvío de registros](/docs/containers?topic=containers-health#logging) para todos los sucesos relacionados con el clúster de distintos orígenes a {{site.data.keyword.loganalysislong_notm}} o a otro servidor externo para que pueda filtrar y analizar los registros. Estos orígenes incluyen registros procedentes de:

- **Contenedores**: los registros que se escriben en STDOUT o STDERR.
- **Apps**: los registros que se escriben en una vía de acceso específica dentro de la app.
- **Nodos trabajadores**: los registros del sistema operativo Ubuntu que se envían a /var/log/syslog y /var/log/auth.log.
- **Servidor de API de Kubernetes**: cada acción relacionada con el clúster que se envía al servidor de API de Kubernetes se registra por motivos de auditoría, e incluyen hora, usuario y recurso afectado. Para obtener más información, consulte [Registros de auditoría de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/debug-application-cluster/audit/)
- **Componentes del sistema Kubernetes**: los registros procedentes de `kubelet`, `kube-proxy` y otros componentes que se ejecutan en el espacio de nombres `kube-system`.
- **Ingress**: Registra un equilibrador de carga de aplicación (ALB) de Ingress que gestiona el tráfico de red que entra en un clúster.

Puede elegir los sucesos que desea registrar para el clúster y dónde desea reenviar los registros. Para detectar actividades maliciosas y para verificar el estado del clúster, debe analizar los registros de forma continua.

**¿Cómo puedo supervisar el estado y el rendimiento de mi clúster?**</br>
Puede verificar la capacidad y el rendimiento del clúster supervisando los componentes del clúster y los recursos de cálculo, como por ejemplo el uso de CPU y de memoria. {{site.data.keyword.containerlong_notm}} envía automáticamente métricas para los clústeres estándares a {{site.data.keyword.monitoringlong}} para que pueda [verlos y analizarlos en Grafana](/docs/containers?topic=containers-health#view_metrics).

También puede utilizar herramientas incorporadas, como la página de detalles de {{site.data.keyword.containerlong_notm}} o el panel de control de Kubernetes, o puede [configurar integraciones de terceros](/docs/containers?topic=containers-supported_integrations#health_services), como Prometheus, Sysdig, LogDNA, Weave Scope y otros.

Para configurar un sistema de detección de intrusiones basado en host (HIDS) y la supervisión de registros de sucesos de seguridad (SELM), instale herramientas de terceros diseñadas para supervisar el clúster y las apps contenerizadas a fin de detectar una intrusión o un mal uso, como [Twistlock ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.twistlock.com/) o el [proyecto Sysdig Falco ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://sysdig.com/opensource/falco/). Sysdig Falco es una herramienta independiente y no se incluye si opta por instalar el [complemento Sysdig](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster) proporcionado por IBM en el clúster.  

**¿Cómo puedo auditar los sucesos que se producen en mi clúster?**</br>
Puede [configurar {{site.data.keyword.cloudaccesstraillong}} en el clúster de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-at_events#at_events). Para obtener más información, consulte la [documentación de {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).

**¿Cuáles son mis opciones para habilitar la confianza en mi clúster?** </br>
De forma predeterminada, {{site.data.keyword.containerlong_notm}} proporciona muchas características para los componentes del clúster de modo que pueda desplegar las apps contenerizadas en un entorno de seguridad. Amplíe su nivel de confianza en su clúster para asegurarse de que lo que sucede en su clúster es lo que tiene previsto. La confianza en el clúster se puede implementar de varias maneras, tal como se muestra en el siguiente diagrama.

<img src="images/trusted_story.png" width="700" alt="Despliegue de contenedores con contenido fiable" style="width:700px; border-style: none"/>

1.  **{{site.data.keyword.containerlong_notm}} con Trusted Compute**: la confianza se puede habilitar en los nodos trabajadores nativos. El agente de confianza supervisa el proceso de inicio de hardware e informa de cualquier cambio para que pueda verificar si sus nodos trabajadores nativos han sido manipulados. Con Trusted Compute, puede desplegar contenedores en hosts nativos verificados de forma que sus cargas de trabajo se ejecuten en un hardware de confianza. Tenga en cuenta que algunas máquinas nativas, por ejemplo que utilizan GPU, no dan soporte a Trusted Compute. [Más información sobre cómo Trusted Compute funciona](#trusted_compute).

2.  **Confianza de contenido en sus imágenes**: Asegúrese de la integridad de sus imágenes habilitando la confianza de contenido en {{site.data.keyword.registryshort_notm}}. Con el contenido de confianza podrá controlar los usuarios que pueden firmar imágenes como imágenes de confianza. Después los firmantes de confianza transferirán una imagen a su registro y los usuarios podrán extraer el contenido firmado de forma que podrán verificar el origen de la misma. Para obtener más información, consulte [Firma de imágenes para contenido de confianza](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent).

3.  **Container Image Security Enforcement (beta)**: Crea un controlador de admisión con políticas personalizadas de forma que es posible verificar las imágenes del contenedor antes de desplegarlas. Con Container Image Security Enforcement, podrá controlar dónde se despliegan las imágenes y asegurarse de que se satisfacen las políticas de [Vulnerability Advisor](/docs/services/va?topic=va-va_index) o los requisitos del [contenido de confianza](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent). Si un despliegue no cumple las políticas que ha establecido, la aplicación de la seguridad impide modificaciones en el clúster. Para obtener más información, consulte [Imposición de seguridad de imagen de contenedor (Beta)](/docs/services/Registry?topic=registry-security_enforce#security_enforce).

4.  **Container Vulnerability Scanner**: De forma predeterminada, Vulnerability Advisor explora imágenes almacenadas en {{site.data.keyword.registryshort_notm}}. Para comprobar el estado de los contenedores activos que están en ejecución en el clúster, puede instalar el explorador de contenedores. Para obtener más información, consulte [Instalación del explorador de contenedores](/docs/services/va?topic=va-va_index#va_install_container_scanner).

5.  **Análisis de red con Security Advisor (vista previa)**: Con {{site.data.keyword.Bluemix_notm}} Security Advisor podrá centralizar el conocimiento de seguridad de los servicios de {{site.data.keyword.Bluemix_notm}} como, por ejemplo, de Vulnerability Advisor y {{site.data.keyword.cloudcerts_short}}. Cuando se habilita Security Advisor en el clúster, puede ver informes sobre el tráfico de red entrante y saliente sospechoso. Para obtener más información, consulte [Analíticas de red](/docs/services/security-advisor?topic=security-advisor-setup-network#setup-network). Para instalar, consulte [Configuración de supervisión de direcciones de IP y clientes sospechosos para un clúster de Kubernetes](/docs/services/security-advisor?topic=security-advisor-setup-network#setup-network).

6.  **{{site.data.keyword.cloudcerts_long_notm}}**: Si desea [exponer la app utilizando un dominio personalizado con TLS](/docs/containers?topic=containers-ingress#ingress_expose_public), puede almacenar el certificado TLS en {{site.data.keyword.cloudcerts_short}}. Los certificados caducados o a punto de caducar también se pueden notificar en el panel de control de {{site.data.keyword.security-advisor_short}}. Para obtener más información, consulte [Iniciación a {{site.data.keyword.cloudcerts_short}}](/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started).

<br />


## Imagen y registro
{: #images_registry}

Cada despliegue se basa en una imagen que contiene las instrucciones sobre cómo utilizar el contenedor que ejecuta la app. Estas instrucciones incluyen el sistema operativo dentro del contenedor y el software adicional que desea instalar. Para proteger la app, debe proteger la imagen y establecer comprobaciones para garantizar la integridad de la imagen.
{: shortdesc}

**¿Debo utilizar un registro público o privado para almacenar mis imágenes?** </br>
Los registros públicos, como por ejemplo Docker Hub, se pueden utilizar para empezar a trabajar con imágenes de Docker y Kubernetes para crear la primera app contenerizada de un clúster. Pero, cuando se trata de aplicaciones de empresa, evite los registros que no conozca o en los que no confía para proteger el clúster de imágenes maliciosas. Mantenga las imágenes en un registro privado, como el que se proporciona en {{site.data.keyword.registryshort_notm}}, y asegúrese de controlar el acceso al registro y el contenido de la imagen que se puede enviar por push.

**¿Por qué es importante comprobar las imágenes frente a vulnerabilidades?** </br>
Las investigaciones muestran que la mayoría de los ataques maliciosos aprovechan las vulnerabilidades conocidas de software y las configuraciones débiles del sistema. Cuando despliega un contenedor a partir de una imagen, el contenedor se utiliza con el sistema operativo y con los binarios adicionales que ha descrito en la imagen. Al igual que protege su máquina virtual o física, debe eliminar las vulnerabilidades conocidas en el sistema operativo y los binarios que utiliza dentro del contenedor para evitar que usuarios no autorizados accedan a la app. </br>

Para proteger sus apps, tenga en cuenta la posibilidad de abordar las siguientes áreas:

1. **Automatizar el proceso de creación y limitar los permisos**: </br>
Automatice el proceso para crear la imagen del contenedor a partir del código fuente para eliminar las variaciones de código fuente y defectos. Al integrar el proceso de creación en el conducto CI/CD, puede garantizar que la imagen solo se explora y se crea si la pasa las comprobaciones de seguridad que ha especificado. Para evitar que los desarrolladores apliquen hot fixes a imágenes confidenciales, limite el número de personas de la organización que tienen acceso al proceso de creación.

2. **Explorar las imágenes antes de desplegarlas en producción:** </br>
Asegúrese de explorar cada imagen antes de desplegar un contenedor a partir de la misma. Por ejemplo, si utiliza {{site.data.keyword.registryshort_notm}}, todas las imágenes se exploran automáticamente en busca de vulnerabilidades cuando envía la imagen a su espacio de nombres. Si se encuentran vulnerabilidades, considere la posibilidad de eliminar las vulnerabilidades o de bloquear el despliegue para dichas imágenes. Busque la persona o el equipo de su organización responsable de supervisar y eliminar las vulnerabilidades. En función de su estructura organizativa, esta persona puede formar parte de un equipo de seguridad, de operaciones o de despliegue. Utilice controladores de admisión, como la [imposición de seguridad de imágenes de contenedor](/docs/services/Registry?topic=registry-security_enforce#security_enforce), para bloquear despliegues desde imágenes que no han pasado comprobaciones de vulnerabilidad y para habilitar la [confianza del contenido](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent) para que un firmante de confianza deba aprobar las imágenes antes de que se envíen al registro del contenedor.

3. **Explorar de forma regular los contenedores en ejecución:** </br>
Aunque haya desplegado un contenedor a partir de una imagen que haya pasado la comprobación de vulnerabilidad, el sistema operativo o los binarios que se ejecutan en el contenedor pueden volverse vulnerables con el tiempo. Para proteger la app, debe asegurarse de que los contenedores en ejecución se exploran de forma regular para que pueda detectar y solucionar las vulnerabilidades. Dependiendo de la app, para añadir seguridad adicional puede establecer un proceso que desactive los contenedores vulnerables después de que se detecten.

**¿Cómo puede {{site.data.keyword.registryshort_notm}} ayudarme a proteger mis imágenes y proceso de despliegue?**  

![Despliegue de contenedores con contenido de confianza](images/cs_image_security.png)

<table>
<caption>Seguridad para imágenes y despliegues</caption>
  <thead>
    <th>Característica de seguridad</th>
    <th>Descripción</th>
  </thead>
  <tbody>
    <tr>
      <td>Repositorio de imágenes privadas seguras de Docker en {{site.data.keyword.registryshort_notm}}</td>
      <td>Configure su propio [repositorio de imágenes](/docs/services/Registry?topic=registry-getting-started#getting-started) de Docker en un registro privado de imágenes multiarrendatario, de alta disponibilidad y escalable alojado y gestionado por IBM. Con este registro, podrá crear, almacenar de forma segura y compartir imágenes de Docker con todos los usuarios del clúster. </br></br>Obtenga más información sobre cómo [proteger su información personal](/docs/containers?topic=containers-security#pi) cuando se trabaja con imágenes de contenedor.</td>
    </tr>
    <tr>
      <td>Solo envíe por push imágenes con contenido fiable</td>
      <td>Asegure la integridad de las imágenes habilitando la [confianza de contenido](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent) en el repositorio de imágenes. Con el contenido de confianza, puede controlar quién puede firmar imágenes como fiables y enviar por push imágenes a un espacio de nombres de registro específico. Después los firmantes de confianza envíen por push una imagen a un espacio de nombres de registro, los usuarios podrán extraer el contenido firmado y podrán verificar el origen de la imagen.</td>
    </tr>
    <tr>
      <td>Exploraciones automáticas de vulnerabilidades</td>
      <td>Si utiliza {{site.data.keyword.registryshort_notm}}, puede aprovechar la exploración de seguridad integrada que proporciona [Vulnerability Advisor](/docs/services/va?topic=va-va_index#va_registry_cli). Cada imagen que se publica en el espacio de nombres del registro se explora automáticamente para detectar vulnerabilidades especificadas en una base de datos de problemas conocidos de CentOS, Debian, Red Hat y Ubuntu. Si se encuentran vulnerabilidades, Vulnerability Advisor proporciona instrucciones sobre cómo resolverlas para garantizar la integridad y la seguridad.</td>
    </tr>
    <tr>
      <td>Despliegues en bloque desde imágenes vulnerables o usuarios no fiables</td>
      <td>Crea un controlador de admisiones con políticas personalizadas de forma que se puedan verificar las imágenes del contenedor antes de desplegarlas. Con [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce), podrá controlar dónde se despliegan las imágenes y asegurarse de que se satisfacen las políticas de Vulnerability Advisor o los requisitos del contenido de confianza. Si un despliegue no cumple las políticas que ha establecido, el controlador de admisiones impide el despliegue en su clúster.</td>
    </tr>
    <tr>
      <td>Exploración directa de contenedores</td>
      <td>Para detectar vulnerabilidades en contenedores en ejecución, puede instalar [ibmcloud-container-scanner](/docs/services/va?topic=va-va_index#va_install_container_scanner). De forma similar a lo que sucede con las imágenes, puede configurar el explorador de contenedores para que supervise los contenedores en busca de vulnerabilidades en todos los espacios de nombres de clúster. Cuando se encuentren vulnerabilidades, actualice la imagen de origen y vuelva a desplegar el contenedor.</td>
    </tr>
  </tbody>
  </table>

<br />


## Seguridad y aislamiento de contenedores
{: #container}

**¿Qué es un espacio de nombres de Kubernetes y por qué debo utilizarlo?** </br>
Los espacios de nombres de Kubernetes constituyen una forma de dividir virtualmente un clúster y de proporcionar aislamiento para los despliegues y los grupos de usuarios que deseen mover su carga de trabajo al clúster. Con los espacios de nombres, puede organizar los recursos entre los nodos trabajadores y entre las zonas de los clústeres multizona.  

Cada clúster se configura con los siguientes espacios de nombres:
- **default:** el espacio de nombres en el que se despliega todo lo que no define un espacio de nombres específico. Si asigna a un usuario el rol de plataforma Visor, Editor y Operador, el usuario puede acceder al espacio de nombres predeterminado, pero no a los espacios de nombres `kube-system`, `ibm-system` ni `ibm-cloud-cert`.
- **kube-system e ibm-system:** este espacio de nombres contiene los despliegues y servicios necesarios para que Kubernetes y {{site.data.keyword.containerlong_notm}} gestionen el clúster. Los administradores del clúster pueden utilizar este espacio de nombres para poner un recurso de Kubernetes a disponibilidad de los espacios de nombres.
- **ibm-cloud-cert:** este espacio de nombres se utiliza para los recursos que están relacionados con {{site.data.keyword.cloudcerts_long_notm}}.
- **kube-public:** todos los usuarios pueden acceder a este espacio de nombres, aunque no estén autenticados con el clúster. Tenga cuidado al desplegar recursos en este espacio de nombres, ya que puede poner su clúster en peligro.

Los administradores de clúster pueden configurar espacios de nombres adicionales en el clúster y personalizarlos en función de sus necesidades.

Para cada espacio de nombres que tenga en el clúster, asegúrese de configurar las [políticas de RBAC](/docs/containers?topic=containers-users#rbac) adecuadas para limitar el acceso a este espacio de nombres, de controlar lo que se despliega y de establecer las [cuotas de recursos ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/policy/resource-quotas/) y los [rangos de límites ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/) adecuados.
{: important}

**¿Debería configurar un clúster de un solo arrendatario o un clúster multiarrendatario?** </br>
En un clúster de un solo arrendatario, puede crear un clúster para cada grupo de personas que deben ejecutar cargas de trabajo en un clúster. Normalmente, este equipo es el responsable de gestionar el clúster y de configurarlo y protegerlo correctamente. Los clústeres multiarrendatario utilizan varios espacios de nombres para aislar los arrendatarios y sus cargas de trabajo.

<img src="images/cs_single_multitenant.png" width="600" alt="Clúster de un solo arrendatario frente a clúster multiarrendatario" style="width:600px; border-style: none"/>

Los clústeres de un solo arrendatario y multiarrendatario proporcionan el mismo nivel de aislamiento para las cargas de trabajo y tienen el mismo coste aproximadamente. La opción adecuada para cada usuario depende del número de equipos que deben ejecutar cargas de trabajo en un clúster, de sus requisitos de servicio y del tamaño del servicio.

Un clúster de un solo arrendatario puede resultar su opción adecuada si tiene un montón de equipos con servicios complejos, y cada uno de ellos debe controlar el ciclo de vida del clúster. Esto incluye la libertad de decidir cuándo se actualiza un clúster o qué recursos se pueden desplegar en el clúster. Tenga en cuenta que para gestionar un clúster se necesitan conocimientos profundos de Kubernetes y de la infraestructura para garantizar la capacidad y la seguridad de los despliegues.  

Los clústeres multiarrendatario ofrecen la ventaja de que puede utilizar el mismo nombre de servicio en distintos espacios de nombres, lo que puede resultar útil si tiene previsto utilizar espacios de nombres para separar el entorno de producción, de transferencia y de desarrollo. Aunque los clústeres multiarrendatario suelen requerir menos personas para gestionar y administrar el clúster, a menudo resultan más complejos en las áreas siguientes:

- **Acceso:** cuando configure varios espacios de nombres, debe configurar las políticas de RBAC adecuadas para cada espacio de nombres para garantizar el aislamiento de recursos. Las políticas de RBAC son complejas y requieren conocimientos profundos de Kubernetes.
- **Limitación de recursos de cálculo:** para asegurarse de que cada equipo tiene los recursos necesarios para desplegar servicios y ejecutar apps en el clúster, debe configurar [cuotas de recursos](https://kubernetes.io/docs/concepts/policy/resource-quotas/) para cada espacio de nombres. Las cuotas de recursos determinan las restricciones de despliegue para un espacio de nombres, como el número de recursos de Kubernetes que se pueden desplegar y la cantidad de CPU y de memoria que pueden consumir estos recursos. Después de establecer una cuota, los usuarios deben incluir los límites y las solicitudes de recursos en sus despliegues.
- **Recursos de clúster compartidos:** si ejecuta varios arrendatarios en un clúster, algunos recursos de clúster, como por ejemplo el equilibrador de carga de aplicación (ALB) de Ingress o las direcciones IP portátiles disponibles, se comparten entre los arrendatarios. Es posible que a los servicios pequeños les cueste utilizar recursos compartidos si deben competir con servicios del clúster de gran tamaño.
- **Actualizaciones:** solo puede ejecutar una versión de API de Kubernetes en un momento determinado. Todas las apps que se ejecutan en un clúster deben cumplir con la versión de la API Kubernetes actual, independientemente del equipo que posea la app. Cuando desee actualizar un clúster, debe asegurarse de que todos los equipos estén listos para cambiar a una nueva versión de API de Kubernetes y que las apps se hayan actualizado para que funcionen con la nueva versión de la API de Kubernetes. Esto también significa que los equipos individuales tienen menos control sobre la versión de la API de Kubernetes que desean ejecutar.
- **Cambios en la configuración del clúster:** si desea cambiar la configuración del clúster o replanificar cargas de trabajo a nuevos nodos trabajadores, debe desplegar este cambio entre los arrendatarios. Este despliegue requiere más conciliación y pruebas que en un clúster de un solo arrendatario.
- **Proceso de comunicación:** cuando gestione varios arrendatarios, considere la posibilidad de configurar un proceso de comunicación para que los arrendatarios sepan a dónde ir cuando exista un problema con el clúster o cuando necesiten más recursos para sus servicios. Este proceso de comunicación también incluye informar a los arrendatarios acerca de todos los cambios en la configuración del clúster o de las actualizaciones planificadas.

**¿Qué más puedo hacer para proteger mi contenedor?**

|Característica de seguridad|Descripción|
|-------|----------------------------------|
|Limitar el número de contenedores privilegiados|Los contenedores se ejecutan como un proceso Linux independiente en el host de cálculo que está aislado de otros procesos. Aunque los usuarios tienen acceso de usuario root dentro del contenedor, los permisos de este usuario están limitados fuera del contenedor para proteger otros procesos de Linux, el sistema de archivos de host y los dispositivos de host. Algunas apps requieren acceso al sistema de archivos de host o permisos avanzados para que se ejecuten correctamente. Puede ejecutar contenedores en modalidad privilegiada para permitir que el contenedor tenga el mismo acceso que los procesos que se ejecutan en el host de cálculo.<p class="important">Tenga en cuenta que los contenedores privilegiados causan grandes daños en el clúster y en el host de cálculo subyacente si se ven comprometidos. Intente limitar el número de contenedores que se ejecutan en modalidad privilegiada y considere la posibilidad de cambiar la configuración de la app de modo que la app se pueda ejecutar sin permisos avanzados. Si desea evitar que los contenedores privilegiados se ejecuten en el clúster, considere la posibilidad de configurar [políticas de seguridad de pod](/docs/containers?topic=containers-psp#customize_psp) personalizadas.</p>|
|Establecer límites de CPU y de memoria para contenedores|Cada contenedor necesita una cantidad específica de CPU y de memoria para iniciarse correctamente y para continuar ejecutándose. Puede definir [límites de recursos y de solicitudes de recursos de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) para sus contenedores a fin de limitar la cantidad de CPU y de memoria que puede consumir el contenedor. Si no hay límites para la CPU y la memoria establecidos y el contenedor está ocupado, el contenedor utiliza todos los recursos disponibles. Este alto consumo de recursos puede afectar a otros contenedores del nodo trabajador que no tienen recursos suficientes para iniciarse o para ejecutarse correctamente, y pone en riesgo el nodo trabajador ante ataques de tipo denegación de servicio.|
|Aplicar valores de seguridad del sistema operativo a los pods|Puede añadir la sección [<code>securityContext</code> ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) al despliegue del pod para aplicar valores de seguridad específicos de Linux al pod o a un contenedor específico del pod. Los valores de seguridad incluyen el control sobre el ID de usuario y el ID de grupo que ejecuta scripts dentro del contenedor, como por ejemplo el script de punto de entrada, o el ID de usuario y la IP de grupo propietarios de la vía de acceso de montaje del volumen. </br></br><strong>Consejo</strong> si desea utilizar <code>securityContext</code> para establecer el ID de usuario <code>runAsUser</code> o el ID de grupo <code>fsGroup</code>, considere la posibilidad de bloquear el almacenamiento cuando [cree almacenamiento persistentes](/docs/containers?topic=containers-block_storage#add_block). El almacenamiento NFS no da soporte a <code>fsGroup</code> y <code>runAsUser</code> se debe establecer a nivel de contenedor, no a nivel de pod. |
|Imponer autenticación según políticas|Puede añadir una anotación de Ingress a los despliegues que le permiten controlar el acceso a los servicios y a las API. Mediante el uso de {{site.data.keyword.appid_short_notm}} y de la seguridad declarativa, puede garantizar la autenticación de usuarios y la validación de señales. |
{: caption="Otras protecciones de seguridad" caption-side="top"}

<br />


## Almacenamiento de información personal
{: #pi}

El usuario es responsable de garantizar la seguridad de la información personal en las imágenes de contenedor y en los recursos de Kubernetes. La información personal incluye nombre, dirección, número de teléfono, dirección de correo electrónico u otra información que permita identificar, ponerse en contacto o ubicar a los clientes, o a cualquier otro individuo.
{: shortdesc}

<dl>
  <dt>Utilización de un secreto de Kubernetes para almacenar información personal</dt>
  <dd>Únicamente almacene información personal en recursos Kubernetes que estén diseñados para alojar información personal. Por ejemplo, no utilice su nombre en el nombre de una correlación de configuración, servicio, despliegue o espacio de nombres de Kubernetes. En lugar de ello, para un cifrado y protección adecuados, almacene la información personal en los <a href="/docs/containers?topic=containers-encryption#secrets">secretos de Kubernetes</a>.</dd>

  <dt>Utilización de `imagePullSecret` de Kubernetes para almacenar credenciales de registros de imagen</dt>
  <dd>No almacene información personal en espacios de nombres de registro ni imágenes de contenedor. En lugar de ello, para un cifrado y protección adecuados, almacene las credenciales de registro en <a href="/docs/containers?topic=containers-images#other">`imagePullSecrets` de Kubernetes</a> y otra información personal en los <a href="/docs/containers?topic=containers-encryption#secrets">secretos de Kubernetes</a>. Recuerde que si la información personal se almacena en una capa previa de una imagen, la supresión de la imagen no será suficiente para suprimir esta información personal.</dd>
  </dl>

Para configurar el cifrado para los secretos, consulte [Cifrado de secretos de Kubernetes mediante {{site.data.keyword.keymanagementserviceshort}}](/docs/containers?topic=containers-encryption#keyprotect).

En nodos trabajadores nativos habilitados para SGX, puede cifrar los datos en uso mediante el [servicio {{site.data.keyword.datashield_short}} (Beta)](/docs/services/data-shield?topic=data-shield-getting-started#getting-started). De forma parecida a cómo funciona el cifrado para los datos en reposo y para los datos en tránsito, Fortanix Runtime Encryption, integrado con {{site.data.keyword.datashield_short}}, protege claves, datos y apps ante amenazas internas y externas. Las amenazas pueden incluir usuarios maliciosos, proveedores de nube, hacks a nivel de sistema operativo o intrusos de red.

## Boletines de seguridad de Kubernetes
{: #security_bulletins}

Si se encuentran vulnerabilidades en Kubernetes, Kubernetes publica CVE en boletines de seguridad para informar a los usuarios y describir las acciones que los usuarios deben tomar para remediar la vulnerabilidad. Los boletines de seguridad de Kubernetes que afectan a los usuarios de {{site.data.keyword.containerlong_notm}} o a la plataforma {{site.data.keyword.Bluemix_notm}} se publican en el [boletín de seguridad de {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/status?component=containers-kubernetes&selected=security).

Algunos CVE requieren la actualización de parches más reciente para una versión de Kubernetes que puede instalar como parte del [proceso de actualización de clúster](/docs/containers?topic=containers-update#update) normal en {{site.data.keyword.containerlong_notm}}. Asegúrese de aplicar los parches de seguridad a tiempo para proteger el clúster frente a ataques maliciosos. Para obtener información sobre lo que se incluye en un parche de seguridad, consulte el [registro de cambios de versión](/docs/containers?topic=containers-changelog#changelog).
