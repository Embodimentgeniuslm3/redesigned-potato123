---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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


# Responsabilidades al utilizar {{site.data.keyword.containerlong_notm}}
{: #responsibilities_iks}

Conozca las responsabilidades de gestión del clúster y los términos y las condiciones existentes para utilizar {{site.data.keyword.containerlong}}.
{:shortdesc}

## Responsabilidades de la gestión de clústeres
{: #responsibilities}

IBM proporciona una plataforma en la nube empresarial que le permite desplegar apps en servicios de {{site.data.keyword.Bluemix_notm}} DevOps, de inteligencia artificial, de datos y de seguridad. Puede elegir cómo configurar, integrar y operar sus apps y servicios en la nube.
{:shortdesc}

<table summary="En la tabla se muestran las responsabilidades de IBM y del usuario. Las filas se leen de izquierda a derecha, en la columna uno aparece un icono que representa cada responsabilidad y en la columna dos una descripción.">
<caption>Responsabilidades de IBM y del usuario</caption>
  <thead>
  <th colspan=2>Responsabilidades por tipo</th>
  </thead>
  <tbody>
    <tr>
    <td align="center"><img src="images/icon_clouddownload.svg" alt="Icono de una nube con una flecha hacia abajo"/><br>Infraestructura de nube</td>
    <td>
    **Responsabilidades de IBM**:
    <ul><li>Desplegar un maestro dedicado totalmente gestionado y altamente disponible en una cuenta de infraestructura protegida, propiedad de IBM, para cada clúster.</li>
    <li>Suministrar nodos trabajadores en la cuenta de infraestructura de IBM Cloud (SoftLayer).</li>
    <li>Configurar los componentes de gestión de clústeres, como por ejemplo, VLAN y equilibradores de carga.</li>
    <li>Cumplimentar solicitudes de más infraestructura, como por ejemplo añadir y eliminar nodos de trabajo, crear subredes predeterminadas y suministrar volúmenes de almacenamiento en respuesta a las reclamaciones de volúmenes persistentes.</li>
    <li>Integrar los recursos de infraestructura ordenados para trabajar de forma automática con la arquitectura del clúster y estar disponible para las apps y cargas de trabajo desplegadas.</li></ul>
    <br><br>
    **Responsabilidades del usuario**:
    <ul><li>Utilizar la API, la CLI o las herramientas de consola para ajustar la capacidad de [cálculo](/docs/containers?topic=containers-clusters#clusters) y [almacenamiento](/docs/containers?topic=containers-storage_planning#storage_planning) y para ajustar la [configuración de red](/docs/containers?topic=containers-cs_network_cluster#cs_network_cluster) según las necesidades de la carga de trabajo.</li></ul><br><br>
    </td>
     </tr>
     <tr>
     <td align="center"><img src="images/icon_tools.svg" alt="Icono de una llave inglesa"/><br>Clúster gestionado</td>
     <td>
     **Responsabilidades de IBM**:
     <ul><li>Proporcionar una suite de herramientas para automatizar la gestión de clústeres, como por ejemplo {{site.data.keyword.containerlong_notm}} [API ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://containers.cloud.ibm.com/global/swagger-global-api/), [plugin de CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) y [consola ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/kubernetes/clusters).</li>
     <li>Aplicar automáticamente los parches del SO, las nuevas versiones y las actualizaciones seguridad en el maestro de Kubernetes. Poner a disposición del usuario las actualizaciones mayores y menores para que las pueda aplicar.</li>
     <li>Actualizar y recuperar los componentes operativos de {{site.data.keyword.containerlong_notm}} y de Kubernetes dentro del clúster, como por ejemplo el equilibrador de carga de aplicación de Ingress y el plugin de almacenamiento de archivos.</li>
     <li>Realizar copia de seguridad y recuperación de datos en etcd, como por ejemplo de los archivos de configuración de carga de trabajo de Kubernetes</li>
     <li>Configurar una conexión OpenVPN entre el nodo maestro y los nodos trabajadores cuando se crea el clúster</li>
     <li>Supervisar e informar del estado de los nodos maestro y trabajadores en las diversas interfaces.</li>
     <li>Proporcionar actualizaciones mayores, menores, de parches de SO, de versión y de seguridad para los nodos trabajadores</li>
     <li>Responder a las solicitudes de automatización para actualizar y recuperar nodos de trabajo. Proporcionar la funcionalidad opcional de [Autorecovery para los nodos trabajadores](/docs/containers?topic=containers-health#autorecovery).</li>
     <li>Proporcionar herramientas, como por ejemplo el [escalado automático de clústeres](/docs/containers?topic=containers-ca#ca), para permitir al usuario ampliar la infraestructura de clústeres.</li>
     </ul>
     <br><br>
     **Responsabilidades del usuario**:
     <ul>
     <li>Utilizar la API, la CLI o las herramientas de consola para [aplicar](/docs/containers?topic=containers-update#update) las actualizaciones mayores y menores para el maestro de Kubernetes y las actualizaciones mayores, menores y de parches para los nodos trabajadores proporcionadas.</li>
     <li>Utilizar la API, la CLI o las herramientas de consola para [recuperar](/docs/containers?topic=containers-cs_troubleshoot#cs_troubleshoot) los recursos de infraestructura o para configurar la funcionalidad opcional de [Autorecovery para los nodos trabajadores](/docs/containers?topic=containers-health#autorecovery).</li></ul>
     <br><br></td>
      </tr>
    <tr>
      <td align="center"><img src="images/icon_locked.svg" alt="Icono de candado"/><br>Entorno enriquecido con seguridad</td>
      <td>
      **Responsabilidades de IBM**:
      <ul>
      <li>Mantener controles correspondientes a [diversos estándares de conformidad del sector](/docs/containers?topic=containers-faqs#standards), como por ejemplo PCI DSS.</li>
      <li>Supervisar, aislar y recuperar el maestro de clúster.</li>
      <li>Proporcionar réplicas de alta disponibilidad de los componentes de servidor de API maestro de Kubernetes, etcd, planificador y gestor de controladores para protegerse frente a una interrupción del nodo maestro.</li>
      <li>Aplicar automáticamente actualizaciones de parches de seguridad maestros y proporcionar actualizaciones de parches de seguridad de nodo trabajador.</li>
      <li>Habilitar determinados valores de seguridad, como por ejemplo discos cifrados en nodos trabajadores</li>
      <li>Inhabilitar determinadas acciones inseguras para los nodos trabajadores, como por ejemplo, no permitir a los usuarios acceso SSH en el host.</li>
      <li>Cifrar la comunicación entre los nodos maestro y trabajadores con TLS.</li>
      <li>Proporcionar imágenes Linux compatibles con CIS para los sistemas operativos de los nodos trabajadores.</li>
      <li>Supervisar de forma continua las imágenes de los nodos maestro y trabajadores para detectar problemas vulnerabilidad y de conformidad con la seguridad.</li>
      <li>Suministrar nodos trabajadores con dos particiones de datos cifrados SSD locales AES de 256 bits.</li>
      <li>Proporcionar opciones para la conectividad de red de clúster, como por ejemplo puntos finales de servicio público y privado.</li>
      <li>Proporcionar opciones para el aislamiento de cálculo, tales como máquinas virtuales dedicadas, nativas y nativas con Cálculo fiable.</li>
      <li>Integrar el control de acceso basado en roles de Kubernetes (RBAC) con Identity and Access Management (IAM) de {{site.data.keyword.Bluemix_notm}}.</li>
      </ul>
      <br><br>
      **Responsabilidades del usuario**:
      <ul>
      <li>Utilizar la API, la CLI o las herramientas de consola para aplicar las [actualizaciones de parches de seguridad](/docs/containers?topic=containers-changelog#changelog) a los nodos trabajadores.</li>
      <li>Elegir cómo configurar la [red de clúster](/docs/containers?topic=containers-plan_clusters) y configurar más [valores de seguridad](/docs/containers?topic=containers-security#security) para satisfacer las necesidades de seguridad y de conformidad de la carga de trabajo. Si procede, configurar el [cortafuegos](/docs/containers?topic=containers-firewall#firewall).</li></ul>
      <br><br></td>
      </tr>

      <tr>
        <td align="center"><img src="images/icon_code.svg" alt="Icono de corchetes angulares"/><br>Orquestación de app</td>
        <td>
        **Responsabilidades de IBM**:
        <ul>
        <li>Suministrar clústeres con los componentes de Kubernetes instalados para que pueda acceder a la API de Kubernetes.</li>
        <li>Proporcionar una serie de complementos gestionados para permitir al usuario ampliar las prestaciones de su app, como por ejemplo [Istio](/docs/containers?topic=containers-istio#istio) y [Knative](/docs/containers?topic=containers-serverless-apps-knative). El mantenimiento se simplifica para el usuario porque IBM proporciona la instalación y las actualizaciones para los complementos gestionados.</li>
        <li>Ofrecer la integración de clústeres con tecnologías asociadas seleccionadas de terceros, como por ejemplo {{site.data.keyword.la_short}}, {{site.data.keyword.mon_short}} y Portworx.</li>
        <li>Proporcionar automatización para permitir enlazar los servicios a otros servicios de {{site.data.keyword.Bluemix_notm}}.</li>
        <li>Crear clústeres con secretos de obtención de imagen para que los despliegues en el espacio de nombres `default` de Kubernetes puedan extraer las imágenes de {{site.data.keyword.registrylong_notm}}.</li>
        <li>Proporcionar clases de almacenamiento y plugins para dar soporte a volúmenes persistentes para utilizarlos con las apps.</li>
        <li>Crear clústeres con direcciones IP de subred reservadas para utilizar para exponer las apps externamente.</li>
        <li>Dar soporte a equilibradores de carga públicos y privados de Kubernetes y a rutas de Ingress para exponer servicios externamente.</li>
        </ul>
        <br><br>
        **Responsabilidades del usuario**:
        <ul>
        <li>Utilizar las herramientas y las características proporcionadas para [configurar y desplegar](/docs/containers?topic=containers-app#app); [definir permisos](/docs/containers?topic=containers-users#users); [integrar con otros servicios](/docs/containers?topic=containers-supported_integrations#supported_integrations); [dar servicio externamente](/docs/containers?topic=containers-cs_network_planning#cs_network_planning); [supervisar el estado](/docs/containers?topic=containers-health#health); [guardar, hacer copia de seguridad y restaurar datos](/docs/containers?topic=containers-storage_planning#storage_planning) y gestionar las cargas de trabajo [de alta disponibilidad](/docs/containers?topic=containers-ha#ha) y resilientes.</li>
        </ul>
        </td>
        </tr>
  </tbody>
  </table>

<br />


## Abuso en {{site.data.keyword.containerlong_notm}}
{: #terms}

Los clientes no pueden utilizar {{site.data.keyword.containerlong_notm}} de forma inapropiada.
{:shortdesc}

Entre los usos no apropiados se incluye:

*   Cualquier actividad ilegal
*   Distribuir o ejecutar malware
*   Provocar cualquier daño a {{site.data.keyword.containerlong_notm}} o interferir a cualquier usuario con la utilización de {{site.data.keyword.containerlong_notm}}
*   Provocar daños o interferir la utilización de otros usuarios con cualquier otro sistema o servicio
*   Acceder de forma no autorizada a los servicios o sistemas
*   Realizar modificaciones no autorizadas de los servicios o sistemas
*   Incumplir los derechos de terceros

Consulte [Términos de los servicios en la nube](/docs/overview/terms-of-use?topic=overview-terms#terms) para obtener una visión general de los términos uso.
