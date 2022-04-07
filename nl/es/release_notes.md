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

# Notas del release
{: #iks-release}

Utilice las notas del release para conocer los cambios más recientes en la documentación de {{site.data.keyword.containerlong}}, agrupados por mes.
{:shortdesc}

## Junio de 2019
{: #jun19}

<table summary="La tabla muestra las notas del release. Las filas se leen de izquierda derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Actualizaciones en la documentación de {{site.data.keyword.containerlong_notm}} de junio de 2019</caption>
<thead>
<th>Fecha</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
  <td>7 de junio de 2019</td>
  <td><ul>
  <li><strong>Acceso al nodo maestro de Kubernetes a través del punto final de servicio privado</strong>: se han añadido [pasos](/docs/containers?topic=containers-clusters#access_on_prem) para exponer el punto final de servicio privado a través de un equilibrador de carga privado. Después de seguir estos pasos, los usuarios autorizados del clúster podrán acceder al nodo maestro de Kubernetes desde una conexión VPN o {{site.data.keyword.Bluemix_notm}} Direct Link.</li>
  <li><strong>{{site.data.keyword.BluDirectLink}}</strong>: se ha añadido {{site.data.keyword.Bluemix_notm}} Direct Link a las páginas [Conectividad VPN](/docs/containers?topic=containers-vpn) y [Nube híbrida](/docs/containers?topic=containers-hybrid_iks_icp) como método para crear una conexión directa y privada entre los entornos de red remotos y {{site.data.keyword.containerlong_notm}} sin direccionamiento a través de Internet público.</li>
  <li><strong>Registro de cambios de ALB de Ingress</strong>: se ha actualizado la imagen de [ALB `ingress-auth` al build 330](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>OpenShift beta</strong>: [se ha añadido una lección](/docs/containers?topic=containers-openshift_tutorial#openshift_logdna_sysdig) sobre cómo modificar despliegues de app para restricciones de contexto de seguridad con privilegios para los complementos {{site.data.keyword.la_full_notm}} y {{site.data.keyword.mon_full_notm}}.</li>
  </ul></td>
</tr>
<tr>
  <td>6 de junio de 2019</td>
  <td><ul>
  <li><strong>Registro de cambios de Fluentd</strong>: se ha añadido un [registro de cambios de versión de Fluentd](/docs/containers?topic=containers-cluster-add-ons-changelog#fluentd_changelog).</li>
  <li><strong>Registro de cambios de ALB de Ingress</strong>: se ha actualizado la imagen de [ALB `nginx-ingress` al build 470](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  </ul></td>
</tr>
<tr>
  <td>5 de junio de 2019</td>
  <td><ul>
  <li><strong>Consulta de CLI</strong>: se ha actualizado la [página de consulta de CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) para reflejar varios cambios correspondientes al [release de la versión 0.3.34](/docs/containers?topic=containers-cs_cli_changelog) del plugin de la CLI de {{site.data.keyword.containerlong_notm}}.</li>
  <li><strong>Novedad: Clústeres de Red Hat OpenShift on IBM Cloud (beta)</strong>: con la versión beta de Red Hat OpenShift on IBM Cloud, puede crear clústeres de {{site.data.keyword.containerlong_notm}} con nodos trabajadores que vienen instalados con el software de la plataforma de orquestación de contenedores OpenShift. Se obtienen todas las ventajas de {{site.data.keyword.containerlong_notm}} gestionado para el entorno de la infraestructura de clúster, junto con las [herramientas OpenShift y el catálogo ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.openshift.com/container-platform/3.11/welcome/index.html) que se ejecuta en Red Hat Enterprise Linux para los despliegues de sus apps. Para empezar, consulte la [Guía de aprendizaje: Creación de un clúster de Red Hat OpenShift on IBM Cloud (beta)](/docs/containers?topic=containers-openshift_tutorial).</li>
  </ul></td>
</tr>
<tr>
  <td>4 de junio de 2019</td>
  <td><ul>
  <li><strong>Registros de cambios de versión</strong>: se han actualizado los registros de cambios para los releases de parches [1.14.2_1521](/docs/containers?topic=containers-changelog#1142_1521), [1.13.6_1524](/docs/containers?topic=containers-changelog#1136_1524), [1.12.9_1555](/docs/containers?topic=containers-changelog#1129_1555) y [1.11.10_1561](/docs/containers?topic=containers-changelog#11110_1561).</li></ul>
  </td>
</tr>
<tr>
  <td>3 de junio de 2019</td>
  <td><ul>
  <li><strong>Traer su propio controlador Ingress</strong>: se han actualizado los [pasos](/docs/containers?topic=containers-ingress#user_managed) para reflejar los cambios en el controlador de la comunidad predeterminado y para requerir una comprobación de estado para las direcciones IP del controlador en clústeres multizona.</li>
  <li><strong>{{site.data.keyword.cos_full_notm}}</strong>: se han actualizado los [pasos](/docs/containers?topic=containers-object_storage#install_cos) para instalar el plugin de {{site.data.keyword.cos_full_notm}} con o sin el servidor Helm, Tiller.</li>
  <li><strong>Registro de cambios de ALB de Ingress</strong>: se ha actualizado la imagen de [ALB `nginx-ingress` al build 467](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>Kustomize</strong>: se ha añadido un ejemplo de [reutilización de los archivos de configuración de Kubernetes entre varios entornos con Kustomize](/docs/containers?topic=containers-app#kustomize).</li>
  <li><strong>Razee</strong>: se ha añadido [Razee ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/razee-io/Razee) a las integraciones soportadas para visualizar información de despliegue en el clúster y para automatizar el despliegue de recursos de Kubernetes. </li></ul>
  </td>
</tr>
</tbody></table>

## Mayo de 2019
{: #may19}

<table summary="La tabla muestra las notas del release. Las filas se leen de izquierda derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Actualizaciones en la documentación de {{site.data.keyword.containerlong_notm}} de mayo de 2019</caption>
<thead>
<th>Fecha</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
  <td>30 de mayo de 2019</td>
  <td><ul>
  <li><strong>Consulta de CLI</strong>: se ha actualizado la [página de consulta de CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) para reflejar varios cambios correspondientes al [release de la versión 0.3.33](/docs/containers?topic=containers-cs_cli_changelog) del plugin de la CLI de {{site.data.keyword.containerlong_notm}}.</li>
  <li><strong>Resolución de problemas de almacenamiento</strong>: <ul>
  <li>Se ha añadido un archivo y un tema de resolución de problemas de almacenamiento en bloque para los [estados pendientes de PVC](/docs/containers?topic=containers-cs_troubleshoot_storage#file_pvc_pending).</li>
  <li>Se ha añadido un tema de resolución de problemas de almacenamiento en bloque para cuando [una app no puede acceder a PVC ni escribir en PVC](/docs/containers?topic=containers-cs_troubleshoot_storage#block_app_failures).</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>28 de mayo de 2019</td>
  <td><ul>
  <li><strong>Registro de cambios de complementos de clúster</strong>: se ha actualizado la imagen de [ALB `nginx-ingress` al build 462](/docs/containers?topic=containers-cluster-add-ons-changelog).</li>
  <li><strong>Registro de resolución de problemas</strong>: se ha añadido un [tema de resolución de problemas](/docs/containers?topic=containers-cs_troubleshoot_clusters#ts_image_pull_create) para cuando el clúster no puede extraer imágenes de {{site.data.keyword.registryfull}} debido a un error durante la creación del clúster.
  </li>
  <li><strong>Resolución de problemas de almacenamiento</strong>: <ul>
  <li>Se ha añadido un tema correspondiente a la [depuración de anomalías de almacenamiento persistente](/docs/containers?topic=containers-cs_troubleshoot_storage#debug_storage).</li>
  <li>Se ha añadido un tema de resolución de problemas para las [anomalías de creación de PVC debido a que faltan permisos](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions).</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>23 de mayo de 2019</td>
  <td><ul>
  <li><strong>Consulta de CLI</strong>: se ha actualizado la [página de consulta de CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) para reflejar varios cambios correspondientes al [release de la versión 0.3.28](/docs/containers?topic=containers-cs_cli_changelog) del plugin de la CLI de {{site.data.keyword.containerlong_notm}}.</li>
  <li><strong>Registro de cambios de complementos de clúster</strong>: se ha actualizado la imagen de [ALB `nginx-ingress` al build 457](/docs/containers?topic=containers-cluster-add-ons-changelog).</li>
  <li><strong>Estados de clúster y de nodos trabajadores</strong>: se ha actualizado la [página de registro y supervisión](/docs/containers?topic=containers-health#states) para añadir tablas de referencia sobre los estados del clúster y de los nodos trabajadores.</li>
  <li><strong>Planificación y creación de clústeres</strong>: ahora puede encontrar información sobre la planificación, la creación y la eliminación de clústeres y sobre la planificación de red en las páginas siguientes:
    <ul><li>[Planificación de la configuración de la red del clúster](/docs/containers?topic=containers-plan_clusters)</li>
    <li>[Planificación del clúster para obtener una alta disponibilidad](/docs/containers?topic=containers-ha_clusters)</li>
    <li>[Planificación de la configuración del nodo trabajador](/docs/containers?topic=containers-planning_worker_nodes)</li>
    <li>[Creación de clústeres](/docs/containers?topic=containers-clusters)</li>
    <li>[Adición de nodos trabajadores y de zonas a clústeres](/docs/containers?topic=containers-add_workers)</li>
    <li>[Eliminación de clústeres](/docs/containers?topic=containers-remove)</li>
    <li>[Cambio de puntos finales de servicio o de conexiones de VLAN](/docs/containers?topic=containers-cs_network_cluster)</li></ul>
  </li>
  <li><strong>Actualizaciones de la versión de clúster</strong>: se ha actualizado la [política de versiones no soportadas](/docs/containers?topic=containers-cs_versions) para indicar que no se pueden actualizar clústeres si la versión maestra está tres o más versiones por detrás de la versión soportada más antigua. Para ver si el clúster **no recibe soporte**, consulte el campo **Estado** cuando se listen los clústeres.</li>
  <li><strong>Istio</strong>: se ha actualizado la [página de Istio](/docs/containers?topic=containers-istio) para eliminar la limitación de que Istio no funciona en clústeres conectados únicamente a una VLAN privada. Se ha añadido un paso al [tema Actualización de complementos gestionados](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons) para volver a crear las pasarelas de Istio que utilizan secciones TLS después de que se haya completado la actualización del complemento gestionado de Istio.</li>
  <li><strong>Temas populares</strong>: se han sustituido los temas populares por esta página de notas de release para las nuevas características y actualizaciones que son específicas de {{site.data.keyword.containershort_notm}}. Para ver la información más reciente sobre los productos {{site.data.keyword.Bluemix_notm}}, consulte los [Anuncios](https://www.ibm.com/cloud/blog/announcements).</li>
  </ul></td>
</tr>
<tr>
  <td>20 de mayo de 2019</td>
  <td><ul>
  <li><strong>Registros de cambios de versión</strong>: se han añadido [registros de cambios de fix pack de nodo trabajador](/docs/containers?topic=containers-changelog).</li>
  </ul></td>
</tr>
<tr>
  <td>16 de mayo de 2019</td>
  <td><ul>
  <li><strong>Consulta de CLI</strong>: se ha actualizado la [página de consulta de CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) para añadir puntos finales de COS para los mandatos `logging-collect` y para aclarar que `apiserver-refresh` reinicia los componentes del nodo maestro de Kubernetes.</li>
  <li><strong>No soportado: Kubernetes versión 1.10</strong>: [Kubernetes versión 1.10](/docs/containers?topic=containers-cs_versions#cs_v114) ya no recibe soporte.</li>
  </ul></td>
</tr>
<tr>
  <td>15 de mayo de 2019</td>
  <td><ul>
  <li><strong>Versión de Kubernetes predeterminada</strong>: ahora la versión predeterminada de Kubernetes es la 1.13.6.</li>
  <li><strong>Límites de servicio</strong>: se ha añadido un [tema sobre limitaciones de servicio](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#tech_limits).</li>
  </ul></td>
</tr>
<tr>
  <td>13 de mayo de 2019</td>
  <td><ul>
  <li><strong>Registro de cambios de versión</strong>: se ha añadido que hay nuevas actualizaciones de parches disponibles para [1.14.1_1518](/docs/containers?topic=containers-changelog#1141_1518), [1.13.6_1521](/docs/containers?topic=containers-changelog#1136_1521), [1.12.8_1552](/docs/containers?topic=containers-changelog#1128_1552), [1.11.10_1558](/docs/containers?topic=containers-changelog#11110_1558) y [1.10.13_1558](/docs/containers?topic=containers-changelog#11013_1558).</li>
  <li><strong>Tipos de nodos trabajadores</strong>: se han eliminado todos los [tipos de nodos trabajadores de máquina virtual](/docs/containers?topic=containers-planning_worker_nodes#vm), que son 48 o más núcleos por [estado de nube ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/status?component=containers-kubernetes&selected=status). Puede seguir suministrando [nodos trabajadores nativos](/docs/containers?topic=containers-plan_clusters#bm) con 48 o más núcleos.</li></ul></td>
</tr>
<tr>
  <td>8 de mayo de 2019</td>
  <td><ul>
  <li><strong>API</strong>: se ha añadido un enlace a [documentación de swagger de API global ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://containers.cloud.ibm.com/global/swagger-global-api/#/).</li>
  <li><strong>Cloud Object Storage</strong>: [se ha añadido una guía de resolución de problemas de Cloud Object Storage](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_pvc_pending) en los clústeres de {{site.data.keyword.containerlong_notm}}.</li>
  <li><strong>Estrategia de Kubernetes</strong>: se ha añadido un tema sobre [¿Qué conocimientos técnicos se recomienda tener antes de mover mis apps a {{site.data.keyword.containerlong_notm}}?](/docs/containers?topic=containers-strategy#knowledge).</li>
  <li><strong>Kubernetes versión 1.14</strong>: se ha añadido que el [release 1.14 de Kubernetes](/docs/containers?topic=containers-cs_versions#cs_v114) está certificado.</li>
  <li><strong>Temas de consulta</strong>: se ha actualizado la información correspondiente a distintas operaciones de enlace de servicio, `registro` y `nlb` en las páginas de consulta de [acceso de usuario](/docs/containers?topic=containers-access_reference) y de la [CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli).</li></ul></td>
</tr>
<tr>
  <td>7 de mayo de 2019</td>
  <td><ul>
  <li><strong>Proveedor de DNS de clúster</strong>: [se explican las ventajas de CoreDNS](/docs/containers?topic=containers-cluster_dns), ahora que los clústeres que ejecutan Kubernetes 1.14 y posteriores solo dan soporte a CoreDNS.</li>
  <li><strong>Nodos de extremo</strong>: se ha añadido soporte de equilibrador de carga privado para [nodos de extremo](/docs/containers?topic=containers-edge).</li>
  <li><strong>Clústeres gratuitos</strong>: se clarifica dónde se da soporte a los [clústeres gratuitos](/docs/containers?topic=containers-regions-and-zones#regions_free).</li>
  <li><strong>Novedad: Integraciones</strong>: se ha añadido y se ha reestructurado la información sobre [integraciones entre servicios de {{site.data.keyword.Bluemix_notm}} y de terceros](/docs/containers?topic=containers-ibm-3rd-party-integrations), [integraciones populares](/docs/containers?topic=containers-supported_integrations) y [asociaciones](/docs/containers?topic=containers-service-partners).</li>
  <li><strong>Novedad: Kubernetes versión 1.14</strong>: cree o actualice sus clústeres a [Kubernetes 1.14](/docs/containers?topic=containers-cs_versions#cs_v114).</li>
  <li><strong>Versión 1.11 de Kubernetes en desuso</strong>: [actualice](/docs/containers?topic=containers-update) los clústeres que ejecuten [Kubernetes 1.11](/docs/containers?topic=containers-cs_versions#cs_v111) antes de que dejen de recibir soporte.</li>
  <li><strong>Permisos</strong>: se ha añadido una pregunta frecuente: [¿Qué políticas de acceso debo dar a los usuarios de mi clúster?](/docs/containers?topic=containers-faqs#faq_access)</li>
  <li><strong>Agrupaciones de nodos trabajadores</strong>: se han añadido instrucciones sobre cómo [aplicar etiquetas a las agrupaciones de nodos trabajadores existentes](/docs/containers?topic=containers-add_workers#worker_pool_labels).</li>
  <li><strong>Temas de consulta</strong>: para dar soporte a nuevas características, como por ejemplo Kubernetes 1.14, se han actualizado las páginas de consulta de [changelog](/docs/containers?topic=containers-changelog#changelog).</li></ul></td>
</tr>
<tr>
  <td>1 de mayo de 2019</td>
  <td><strong>Asignación de acceso a la infraestructura</strong>: se han revisado los [pasos a seguir para asignar permisos de IAM para abrir casos de soporte](/docs/containers?topic=containers-users#infra_access).</td>
</tr>
</tbody></table>


