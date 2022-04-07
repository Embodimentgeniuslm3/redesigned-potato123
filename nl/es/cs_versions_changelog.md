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


# Registro de cambios de versiones
{: #changelog}

Visualice información de cambios de versiones de actualizaciones mayores, menores y parches de actualización que están disponibles para los clústeres {{site.data.keyword.containerlong}} Kubernetes. Los cambios incluyen actualizaciones a Kubernetes y a los componentes de Proveedores de {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

A menos que se indique lo contrario en los registros de cambios, la versión de proveedor de {{site.data.keyword.containerlong_notm}} habilita las API de Kubernetes y las características que están en fase beta. Las características alfa de Kubernetes, que están sujetas a cambios, están inhabilitadas.

Para obtener más información acerca de las versiones principales, secundarias y de parche y sobre las acciones de preparación entre versiones menores, consulte [Versiones de Kubernetes](/docs/containers?topic=containers-cs_versions).
{: tip}

Para obtener información sobre los cambios desde la versión anterior, consulte los siguientes registros de cambios.
-  [Registros de cambios](#114_changelog) de la versión 1.14.
-  [Registros de cambios](#113_changelog) de la versión 1.13.
-  [Registros de cambios](#112_changelog) de la versión 1.12.
-  **En desuso**: [Registros de cambios](#111_changelog) de la versión 1.11.
-  [Archivo](#changelog_archive) de registros de cambios para versiones no soportadas.

Algunos registros de cambios son para _fixpacks de nodos trabajadores_ y solo se aplican a los nodos trabajadores. Debe [aplicar estos parches](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) para garantizar la conformidad de seguridad de los nodos trabajadores. Estos fixpacks de nodo trabajador pueden estar en una versión posterior que el maestro porque algunos fixpacks de compilación son específicos de los nodos trabajadores. Otros registros de cambios son para _fixpacks de nodo maestro_ y solo se aplican al nodo maestro del clúster. Es posible que los fixpacks de nodo maestro no se apliquen automáticamente. Puede optar por [aplicarlos manualmente](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update). Para obtener más información sobre los tipos de parches, consulte [Tipos de actualizaciones](/docs/containers?topic=containers-cs_versions#update_types).
{: note}

</br>

## Registro de cambios de la versión 1.14
{: #114_changelog}

### Registro de cambios para 1.14.2_1521, publicado el 4 de junio de 2019
{: #1142_1521}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.14.2_1521.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.14.1_1519">
<caption>Cambios desde la versión 1.14.1_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de DNS de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha corregido un error que podía provocar que los pods de Kubernetes DNS y de CoreDNS siguieran ejecutándose después de operaciones `create` o `update`.</td>
</tr>
<tr>
<td>Configuración de alta disponibilidad de nodo maestro de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha actualizado la configuración para minimizar los errores intermitentes de conectividad de red maestra durante una actualización de nodo maestro.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Consulte las [notas del release de etcd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Se ha actualizado la imagen para [CVE-2018-10844 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) y [CVE-2019-5436 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.14.1-71</td>
<td>v1.14.2-100</td>
<td>Se ha actualizado para dar soporte al release 1.14.2 de Kubernetes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.14.1</td>
<td>v1.14.2</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.2).</td>
</tr>
<tr>
<td>Servidor de métricas de Kubernetes</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>Consulte las [notas del release del servidor de métricas de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Se ha actualizado la imagen para [CVE-2018-10844 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) y [CVE-2019-5436 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.14.1_1519, publicado el 20 de mayo de 2019
{: #1141_1519}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.14.1_1519.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.14.1_1518">
<caption>Cambios desde la versión 1.14.1_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel de Ubuntu 16.04</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-12126 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) y [CVE-2018-12130 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Kernel de Ubuntu 18.04</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-12126 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) y [CVE-2018-12130 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.14.1_1518, publicado el 13 de mayo de 2019
{: #1141_1518}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.14.1_1518.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.14.1_1516">
<caption>Cambios desde la versión 1.14.1_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy de alta disponibilidad de maestro de clúster</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Consulte las [notas del release de HAProxy ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La actualización resuelve
[CVE-2019-6706 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configuración de la política de auditoría del servidor de API de Kubernetes se ha actualizado para que no registre el URL de solo lectura `/openapi/v2*`. Además, la configuración del gestor del controlador de Kubernetes ha aumentado la duración de validez de los certificados de `kubelet` firmados de 1 a 3 años.</td>
</tr>
<tr>
<td>Configuración del cliente OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>El pod `vpn-*` del cliente de OpenVPN del espacio de nombres `kube-system` ahora establece `dnsPolicy` en `Default` para evitar que el pod falle cuando el DNS del clúster esté inactivo.</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>e7182c7</td>
<td>13c7ef0</td>
<td>Se ha actualizado la imagen para [CVE-2016-7076 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076) y
[CVE-2017-1000368 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.14.1_1516, publicado el 7 de mayo de 2019
{: #1141_1516}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.14.1_1516.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.13.5_1519">
<caption>Cambios desde la versión 1.13.5_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.4.4</td>
<td>v3.6.1</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.6/release-notes/).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.6</td>
<td>1.3.1</td>
<td>Consulte las [notas del release de CoreDNS ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://coredns.io/2019/01/13/coredns-1.3.1-release/). La actualización incluye la adición de un [puerto de métricas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://coredns.io/plugins/metrics/) en el servicio DNS de clúster. <br><br>Ahora CoreDNS es el único proveedor de DNS de clúster soportado. Si actualiza un clúster a Kubernetes versión 1.14 desde una versión anterior y utiliza KubeDNS, KubeDNS se migra automáticamente a CoreDNS durante la actualización del clúster. Para obtener más información o para probar CoreDNS antes de actualizar, consulte [Configuración del proveedor de DNS de clúster](https://cloud.ibm.com/docs/containers?topic=containers-cluster_dns#cluster_dns).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>9ff3fda</td>
<td>ed0dafc</td>
<td>Se ha actualizado la imagen para [CVE-2019-1543 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.13.5-107</td>
<td>v1.14.1-71</td>
<td>Se ha actualizado para dar soporte al release 1.14.1 de Kubernetes. Además, la versión de `calicoctl` se ha actualizado a 3.6.1. Se han corregido las actualizaciones a los equilibradores de carga de la versión 2.0 con solo un nodo trabajador disponible para los pods de equilibrador de carga. Ahora los equilibradores de carga privados se pueden ejecutar en [nodos trabajadores de extremo privados](/docs/containers?topic=containers-edge#edge).</td>
</tr>
<tr>
<td>Políticas de seguridad de pod de IBM</td>
<td>N/D</td>
<td>N/D</td>
<td>Las [políticas de seguridad de pod de IBM](/docs/containers?topic=containers-psp#ibm_psp) se han actualizado para dar soporte a la característica [RunAsGroup ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#users-and-groups) de Kubernetes.</td>
</tr>
<tr>
<td>Configuración de `kubelet`</td>
<td>N/D</td>
<td>N/D</td>
<td>Establezca la opción `--pod-max-pids` en `14336` para evitar que un solo pod consulta todos los ID de proceso de un nodo trabajador.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.14.1</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.1) y el [blog de Kubernetes 1.14 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/blog/2019/03/25/kubernetes-1-14-release-announcement/).<br><br>Las políticas de control de acceso basado en rol (RBAC) predeterminadas de Kubernetes ya no otorgan acceso a las [API de descubrimiento y de comprobación de permisos para usuarios autenticados ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles). Este cambio solo se aplica a los nuevos clústeres de la versión 1.14. Si actualiza un clúster desde una versión anterior, los usuarios no autenticados siguen teniendo acceso a las API de descubrimiento y comprobación de permisos.</td>
</tr>
<tr>
<td>Configuración de controladores de admisión de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td><ul>
<li>Se ha añadido `NodeRestriction` a la opción `--enable-admission-plugins` para el servidor de API de Kubernetes del clúster y se han configurado los recursos relacionados del clúster para que den soporte a esta mejora en la seguridad.</li>
<li>Se ha eliminado `Initializers` de la opción `--enable-admission-plugins` y `admissionregistration.k8s.io/v1alpha1=true` de la opción `--runtime-config` para el servidor de API de Kubernetes del clúster porque estas API ya no reciben soporte. En su lugar, puede utilizar [webhooks de admisión de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/).</li></ul></td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.3.0</td>
<td>1.4.0</td>
<td>Consulte las [notas del release de Kubernetes DNS Autoscaler ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.4.0).</td>
</tr>
<tr>
<td>Configuración de puertas de características de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td><ul>
  <li>Se ha añadido `RuntimeClass=false` para inhabilitar la selección de la configuración de tiempo de ejecución de contenedor.</li>
  <li>Se ha eliminado `ExperimentalCriticalPodAnnotation=true` porque la anotación de pod `scheduler.alpha.kubernetes.io/critical-pod` ya no recibe soporte. En su lugar, puede utilizar la [prioridad de pod de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/docs/containers?topic=containers-pod_priority#pod_priority).</li></ul></td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>e132aa4</td>
<td>e7182c7</td>
<td>Se ha actualizado la imagen para [CVE-2019-11068 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

<br />


## Registro de cambios de la versión 1.13
{: #113_changelog}

### Registro de cambios para 1.13.6_1524, publicado el 4 de junio de 2019
{: #1136_1524}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.13.6_1524.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.13.6_1522">
<caption>Cambios desde la versión 1.13.6_1522</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de DNS de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha corregido un error que podía provocar que los pods de Kubernetes DNS y de CoreDNS siguieran ejecutándose después de operaciones `create` o `update`.</td>
</tr>
<tr>
<td>Configuración de alta disponibilidad de nodo maestro de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha actualizado la configuración para minimizar los errores intermitentes de conectividad de red maestra durante una actualización de nodo maestro.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Consulte las [notas del release de etcd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Se ha actualizado la imagen para [CVE-2018-10844 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) y [CVE-2019-5436 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Servidor de métricas de Kubernetes</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>Consulte las [notas del release del servidor de métricas de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Se ha actualizado la imagen para [CVE-2018-10844 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) y [CVE-2019-5436 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.13.6_1522, publicado el 20 de mayo de 2019
{: #1136_1522}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.13.6_1522.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.13.6_1521">
<caption>Cambios desde la versión 1.13.6_1521</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel de Ubuntu 16.04</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-12126 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) y [CVE-2018-12130 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Kernel de Ubuntu 18.04</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-12126 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) y [CVE-2018-12130 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.13.6_1521, publicado el 13 de mayo de 2019
{: #1136_1521}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.13.6_1521.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.13.5_1519">
<caption>Cambios desde la versión 1.13.5_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy de alta disponibilidad de maestro de clúster</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Consulte las [notas del release de HAProxy ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La actualización resuelve
[CVE-2019-6706 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Se ha actualizado la imagen para [CVE-2019-1543 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.13.5-107</td>
<td>v1.13.6-139</td>
<td>Se ha actualizado para dar soporte al release 1.13.6 de Kubernetes. Además, se ha corregido el proceso de actualización para el equilibrador de carga de la versión 2.0 que solo tiene un nodo trabajador disponible para los pods del equilibrador de carga.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.13.6</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.6).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configuración de la política de auditoría del servidor de API de Kubernetes se ha actualizado para que no registre el URL de solo lectura `/openapi/v2*`. Además, la configuración del gestor del controlador de Kubernetes ha aumentado la duración de validez de los certificados de `kubelet` firmados de 1 a 3 años.</td>
</tr>
<tr>
<td>Configuración del cliente OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>El pod `vpn-*` del cliente de OpenVPN del espacio de nombres `kube-system` ahora establece `dnsPolicy` en `Default` para evitar que el pod falle cuando el DNS del clúster esté inactivo.</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Se ha actualizado la imagen para [CVE-2016-7076 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) y [CVE-2019-11068 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.13.5_1519, publicado el 29 de abril de 2019
{: #1135_1519}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.13.5_1519.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.13.5_1518">
<caption>Cambios desde la versión 1.13.5_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones de los paquetes de Ubuntu instalados.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.5</td>
<td>1.2.6</td>
<td>Consulte las [notas del release de containerd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.2.6).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.13.5_1518, publicado el 15 de abril de 2019
{: #1135_1518}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.13.5_1518.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.13.5_1517">
<caption>Cambios desde la versión 1.13.5_1517</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones en los paquetes Ubuntu instalados, que incluyen `systemd` para [CVE-2019-3842 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.13.5_1517, publicado el 8 de abril de 2019
{: #1135_1517}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.13.5_1517.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.13.4_1516">
<caption>Cambios desde la versión 1.13.4_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.4.0</td>
<td>v3.4.4</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.4/releases/#v344). La actualización resuelve
[CVE-2019-9946 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Proxy de alta disponibilidad de maestro de clúster</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Consulte las [notas del release de HAProxy ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La actualización resuelve [CVE-2018-0732 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) y [CVE-2019-1559 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.13.4-86</td>
<td>v1.13.5-107</td>
<td>Se ha actualizado para dar soporte a los releases 1.13.5 de Kubernetes y 3.4.4 de Calico.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.4</td>
<td>v1.13.5</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.5).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Se ha actualizado la imagen para [CVE-2017-12447 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Kernel de Ubuntu 16.04</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2019-9213 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Kernel de Ubuntu 18.04</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2019-9213 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.13.4_1516, publicado el 1 de abril de 2019
{: #1134_1516}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.13.4_1516.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.13.4_1515">
<caption>Cambios desde la versión 1.13.4_1515</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilización de recursos del nodo trabajador</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han incrementado las reservas de memoria para kubelet y containerd para evitar que estos componentes se puedan quedar sin recursos. Para obtener más información, consulte [Reservas de recursos del nodo trabajador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Registro de cambios para fixpack de nodo maestro 1.13.4_1515, publicado el 26 de marzo de 2019
{: #1134_1515}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo maestro 1.13.4_1515.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.13.4_1513">
<caption>Cambios desde la versión 1.13.4_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de DNS de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha corregido el proceso de actualización de Kubernetes versión 1.11 para evitar que la actualización cambie el proveedor de DNS de clúster a CoreDNS. Todavía puede [configurar CoreDNS como proveedor de DNS de clúster](/docs/containers?topic=containers-cluster_dns#set_coredns) después de la actualización.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>345</td>
<td>346</td>
<td>Se ha actualizado la imagen para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>166</td>
<td>167</td>
<td>Corrige errores intermitentes de `context deadline exceeded` y `timeout` para gestionar secretos de Kubernetes. Además, corrige las actualizaciones al servicio de gestión de claves que podrían dejar secretos existentes de Kubernetes sin cifrar. La actualización incluye el arreglo para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Equilibrador de carga y supervisor de equilibrador de carga para el proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>143</td>
<td>146</td>
<td>Se ha actualizado la imagen para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.13.4_1513, publicado el 20 de marzo de 2019
{: #1134_1513}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.13.4_1513.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.13.4_1510">
<caption>Cambios desde la versión 1.13.4_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de DNS de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha corregido un error que podía provocar que operaciones de clúster maestro, como `refresh` o `update`, fallaran cuando se debía reducir el DNS de clúster no utilizado.</td>
</tr>
<tr>
<td>Configuración de proxy de alta disponibilidad de maestro de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha actualizado la configuración para manejar mejor los errores de conexión intermitentes con el clúster maestro.</td>
</tr>
<tr>
<td>Configuración de CoreDNS</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha actualizado la configuración de CoreDNS para dar soporte a [varios Corefiles ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://coredns.io/2017/07/23/corefile-explained/) después de actualizar la versión 1.12 de Kubernetes del clúster.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.4</td>
<td>1.2.5</td>
<td>Consulte las [notas del release de containerd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.2.5). La actualización incluye el arreglo mejorado para [CVE-2019-5736 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Se han actualizado los controladores de GPU a [418.43 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.nvidia.com/object/unix.html). La actualización incluye el arreglo para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>344</td>
<td>345</td>
<td>Se ha añadido soporte para [puntos finales de servicio privados](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2019-6133 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>136</td>
<td>166</td>
<td>Se ha actualizado la imagen para [CVE-2018-16890 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) y [CVE-2019-3823 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Se ha actualizado la imagen para [CVE-2018-10779 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) y [CVE-2019-7663 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.13.4_1510, publicado el 4 de marzo de 2019
{: #1134_1510}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.13.4_1510.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.13.2_1509">
<caption>Cambios desde la versión 1.13.2_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proveedor de DNS de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha aumentado el límite de memoria de DNS de Kubernetes y de pod de CoreDNS de `170 Mi` a `400 Mi` para poder gestionar más servicios de clúster.</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Se han actualizado las imágenes para [CVE-2019-6454 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.13.2-62</td>
<td>v1.13.4-86</td>
<td>Se ha actualizado para dar soporte al release 1.13.4 de Kubernetes. Se han solucionado problemas de conectividad periódicos para los equilibradores de carga de la versión 1.0 que establecen `externalTrafficPolicy` en `local`. Se han actualizado los sucesos de la versión 1.0 y 2.0 del equilibrador de carga para que utilice los últimos enlaces de la documentación de {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>342</td>
<td>344</td>
<td>Se ha cambiado el sistema operativo base para la imagen de Fedora a Alpine. Se ha actualizado la imagen para [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>122</td>
<td>136</td>
<td>Se ha aumentado el tiempo de espera del cliente en {{site.data.keyword.keymanagementservicefull_notm}}. Se ha actualizado la imagen para [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.2</td>
<td>v1.13.4</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.4). La actualización resuelve [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) y [CVE-2019-1002100 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha añadido `ExperimentalCriticalPodAnnotation=true` a la opción `--feature-gates`. Este valor ayuda a migrar los pods de la anotación `scheduler.alpha.kubernetes.io/critical-pod` en desuso para [dar soporte a la prioridad de pod de Kubernetes](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Equilibrador de carga y supervisor de equilibrador de carga para el proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>132</td>
<td>143</td>
<td>Se ha actualizado la imagen para [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Cliente y servidor de OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Se ha actualizado la imagen para [CVE-2019-1559 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Se ha actualizado la imagen para [CVE-2019-6454 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.13.2_1509, publicado el 27 de febrero de 2019
{: #1132_1509}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.13.2_1509.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.13.2_1508">
<caption>Cambios desde la versión 1.13.2_1508</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2018-19407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.13.2_1508, publicado el 15 de febrero de 2019
{: #1132_1508}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.13.2_1508.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.13.2_1507">
<caption>Cambios desde la versión 1.13.2_1507</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de proxy de alta disponibilidad de maestro de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha cambiado el valor de configuración de pod `spec.priorityClassName` por `system-node-critical` y se ha establecido el valor `spec.priority` en `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.2</td>
<td>1.2.4</td>
<td>Consulte las [notas del release de containerd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.2.4). La actualización resuelve
[CVE-2019-5736 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configuración de `kubelet` de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha habilitado el control de la característica `ExperimentalCriticalPodAnnotation` para evitar el desalojo de pods estáticos críticos. Establezca la opción `event-qps` en `0` para evitar la creación de sucesos de limitación de la velocidad.</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.13.2_1507, publicado el 5 de febrero de 2019
{: #1132_1507}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.13.2_1507.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.4_1535">
<caption>Cambios desde la versión 1.12.4_1535</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.4.0</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.4/releases/#v340).</td>
</tr>
<tr>
<td>Proveedor de DNS de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>CoreDNS es ahora el proveedor de DNS de clúster predeterminado para los nuevos clústeres. Si actualiza un clúster existente a 1.13 que utiliza KubeDNS como proveedor de DNS de clúster, KubeDNS continúa siendo el proveedor de DNS de clúster. Sin embargo, puede optar por [utilizar CoreDNS en su lugar](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.2.2</td>
<td>Consulte las [notas del release de containerd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.2.2).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.2</td>
<td>1.2.6</td>
<td>Consulte las [notas del release de CoreDNS ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coredns/coredns/releases/tag/v1.2.6). Además, la configuración de CoreDNS se ha actualizado para [dar soporte a varios Corefiles ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://coredns.io/2017/07/23/corefile-explained/).</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Consulte las [notas del release de etcd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coreos/etcd/releases/v3.3.11). Además, las suites de cifrado soportadas para etcd están ahora están restringidas a un subconjunto con cifrado de alta resistencia (128 bits o más).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Se han actualizado las imágenes para [CVE-2019-3462 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) y
[CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.4-118</td>
<td>v1.13.2-62</td>
<td>Se ha actualizado para dar soporte al release 1.13.2 de Kubernetes. Además, la versión de `calicoctl` se ha actualizado a 3.4.0. Se han corregido actualizaciones de configuración innecesarias en los equilibradores de carga de la versión 2.0 en los cambios de estado de nodo trabajador.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>338</td>
<td>342</td>
<td>El plugin de almacenamiento de archivos se ha actualizado de la forma siguiente:
<ul><li>Admite el suministro dinámico con la [planificación de topología de volúmenes](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Pasa por alto los errores de supresión de reclamación de volumen persistente (PVC) si el almacenamiento ya se ha suprimido.</li>
<li>Añade una anotación de mensaje de anomalía a las PVC anómalas.</li>
<li>Optimiza los valores de elección de líder del controlador de suministro de almacenamiento y de periodo de resincronización y aumenta el tiempo de espera de suministro de 30 minutos a 1 hora.</li>
<li>Comprueba los permisos de usuario antes de iniciar el suministro.</li>
<li>Añade una tolerancia `CriticalAddonsOnly` a los despliegues de `ibm-file-plugin` e `ibm-storage-watcher`
en el espacio de nombres `kube-system`.</li></ul></td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>111</td>
<td>122</td>
<td>Se ha añadido lógica de reintento para evitar anomalías temporales cuando los secretos de Kubernetes están gestionados por {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.13.2</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.2).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configuración de la política de auditoría del servidor de API de Kubernetes se ha actualizado para incluir los metadatos de registro para las solicitudes `cluster-admin` y el registro del cuerpo de la solicitud de las solicitudes `create`, `update`
y `patch` de la carga de trabajo.</td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.2.0</td>
<td>1.3.0</td>
<td>Consulte las [notas del release de Kubernetes DNS Autoscaler ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.3.0).</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Se ha actualizado la imagen para [CVE-2018-0734 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) y
[CVE-2018-5407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Se ha añadido la tolerancia `CriticalAddonsOnly` al despliegue de `vpn` en el espacio de nombres `kube-system`. Además, la configuración de pod ahora se obtiene a partir de un secreto en lugar de obtenerse de un mapa de configuración.</td>
</tr>
<tr>
<td>Servidor OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Se ha actualizado la imagen para [CVE-2018-0734 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) y
[CVE-2018-5407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Parche de seguridad para [CVE-2018-16864 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

<br />


## Registro de cambios de la versión 1.12
{: #112_changelog}

Revise el registro de cambios de la versión 1.12.
{: shortdesc}

### Registro de cambios para 1.12.9_1555, publicado el 4 de junio de 2019
{: #1129_1555}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.12.9_1555.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.8_1553">
<caption>Cambios desde la versión 1.12.8_1553</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de DNS de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha corregido un error que podía provocar que los pods de Kubernetes DNS y de CoreDNS siguieran ejecutándose después de operaciones `create` o `update`.</td>
</tr>
<tr>
<td>Configuración de alta disponibilidad de nodo maestro de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha actualizado la configuración para minimizar los errores intermitentes de conectividad de red maestra durante una actualización de nodo maestro.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Consulte las [notas del release de etcd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Se ha actualizado la imagen para [CVE-2018-10844 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) y [CVE-2019-5436 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.8-210</td>
<td>v1.12.9-227</td>
<td>Se ha actualizado para dar soporte al release 1.12.9 de Kubernetes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.8</td>
<td>v1.12.9</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.9).</td>
</tr>
<tr>
<td>Servidor de métricas de Kubernetes</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>Consulte las [notas del release del servidor de métricas de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Se ha actualizado la imagen para [CVE-2018-10844 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) y [CVE-2019-5436 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.12.8_1553, publicado el 20 de mayo de 2019
{: #1128_1533}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.12.8_1553.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.8_1553">
<caption>Cambios desde la versión 1.12.8_1553</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel de Ubuntu 16.04</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-12126 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) y [CVE-2018-12130 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Kernel de Ubuntu 18.04</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-12126 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) y [CVE-2018-12130 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.12.8_1552, publicado el 13 de mayo de 2019
{: #1128_1552}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.12.8_1552.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.7_1550">
<caption>Cambios desde la versión 1.12.7_1550</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy de alta disponibilidad de maestro de clúster</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Consulte las [notas del release de HAProxy ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La actualización resuelve
[CVE-2019-6706 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Se ha actualizado la imagen para [CVE-2019-1543 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.7-180</td>
<td>v1.12.8-210</td>
<td>Se ha actualizado para dar soporte al release 1.12.8 de Kubernetes. Además, se ha corregido el proceso de actualización para el equilibrador de carga de la versión 2.0 que solo tiene un nodo trabajador disponible para los pods del equilibrador de carga.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.7</td>
<td>v1.12.8</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.8).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configuración de la política de auditoría del servidor de API de Kubernetes se ha actualizado para que no registre el URL de solo lectura `/openapi/v2*`. Además, la configuración del gestor del controlador de Kubernetes ha aumentado la duración de validez de los certificados de `kubelet` firmados de 1 a 3 años.</td>
</tr>
<tr>
<td>Configuración del cliente OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>El pod `vpn-*` del cliente de OpenVPN del espacio de nombres `kube-system` ahora establece `dnsPolicy` en `Default` para evitar que el pod falle cuando el DNS del clúster esté inactivo.</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Se ha actualizado la imagen para [CVE-2016-7076 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) y [CVE-2019-11068 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.12.7_1550, publicado el 29 de abril de 2019
{: #1127_1550}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.12.7_1550.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.7_1549">
<caption>Cambios desde la versión 1.12.7_1549</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones de los paquetes de Ubuntu instalados.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>Consulte las [notas del release de containerd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.1.7).</td>
</tr>
</tbody>
</table>


### Registro de cambios para el fixpack de nodo trabajador 1.12.7_1549, publicado el 15 de abril de 2019
{: #1127_1549}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.12.7_1549.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.7_1548">
<caption>Cambios desde la versión 1.12.7_1548</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones en los paquetes Ubuntu instalados, que incluyen `systemd` para [CVE-2019-3842 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.12.7_1548, publicado el 8 de abril de 2019
{: #1127_1548}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.12.7_1548.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.6_1547">
<caption>Cambios desde la versión 1.12.6_1547</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.3/releases/#v336). La actualización resuelve
[CVE-2019-9946 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Proxy de alta disponibilidad de maestro de clúster</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Consulte las [notas del release de HAProxy ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La actualización resuelve [CVE-2018-0732 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) y [CVE-2019-1559 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.6-157</td>
<td>v1.12.7-180</td>
<td>Se ha actualizado para dar soporte a los releases 1.12.7 de Kubernetes y 3.3.6 de Calico.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.6</td>
<td>v1.12.7</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.7).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Se ha actualizado la imagen para [CVE-2017-12447 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Kernel de Ubuntu 16.04</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2019-9213 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Kernel de Ubuntu 18.04</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2019-9213 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.12.6_1547, publicado el 1 de abril de 2019
{: #1126_1547}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.12.6_1547.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.6_1546">
<caption>Cambios desde la versión 1.12.6_1546</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilización de recursos del nodo trabajador</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han incrementado las reservas de memoria para kubelet y containerd para evitar que estos componentes se puedan quedar sin recursos. Para obtener más información, consulte [Reservas de recursos del nodo trabajador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>


### Registro de cambios para fixpack de nodo maestro 1.12.6_1546, publicado el 26 de marzo de 2019
{: #1126_1546}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo maestro 1.12.6_1546.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.6_1544">
<caption>Cambios desde la versión 1.12.6_1544</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>345</td>
<td>346</td>
<td>Se ha actualizado la imagen para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>166</td>
<td>167</td>
<td>Corrige errores intermitentes de `context deadline exceeded` y `timeout` para gestionar secretos de Kubernetes. Además, corrige las actualizaciones al servicio de gestión de claves que podrían dejar secretos existentes de Kubernetes sin cifrar. La actualización incluye el arreglo para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Equilibrador de carga y supervisor de equilibrador de carga para el proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>143</td>
<td>146</td>
<td>Se ha actualizado la imagen para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.12.6_1544, publicado el 20 de marzo de 2019
{: #1126_1544}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.12.6_1544.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.6_1541">
<caption>Cambios desde la versión 1.12.6_1541</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de DNS de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha corregido un error que podía provocar que operaciones de clúster maestro, como `refresh` o `update`, fallaran cuando se debía reducir el DNS de clúster no utilizado.</td>
</tr>
<tr>
<td>Configuración de proxy de alta disponibilidad de maestro de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha actualizado la configuración para manejar mejor los errores de conexión intermitentes con el clúster maestro.</td>
</tr>
<tr>
<td>Configuración de CoreDNS</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha actualizado la configuración de CoreDNS para dar soporte a [varios Corefiles ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://coredns.io/2017/07/23/corefile-explained/).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Se han actualizado los controladores de GPU a [418.43 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.nvidia.com/object/unix.html). La actualización incluye el arreglo para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>344</td>
<td>345</td>
<td>Se ha añadido soporte para [puntos finales de servicio privados](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2019-6133 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>136</td>
<td>166</td>
<td>Se ha actualizado la imagen para [CVE-2018-16890 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) y [CVE-2019-3823 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Se ha actualizado la imagen para [CVE-2018-10779 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) y [CVE-2019-7663 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.12.6_1541, publicado el 4 de marzo de 2019
{: #1126_1541}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.12.6_1541.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.5_1540">
<caption>Cambios desde la versión 1.12.5_1540</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proveedor de DNS de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha aumentado el límite de memoria de DNS de Kubernetes y de pod de CoreDNS de `170 Mi` a `400 Mi` para poder gestionar más servicios de clúster.</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Se han actualizado las imágenes para [CVE-2019-6454 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.5-137</td>
<td>v1.12.6-157</td>
<td>Se ha actualizado para dar soporte al release 1.12.6 de Kubernetes. Se han solucionado problemas de conectividad periódicos para los equilibradores de carga de la versión 1.0 que establecen `externalTrafficPolicy` en `local`. Se han actualizado los sucesos de la versión 1.0 y 2.0 del equilibrador de carga para que utilice los últimos enlaces de la documentación de {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>342</td>
<td>344</td>
<td>Se ha cambiado el sistema operativo base para la imagen de Fedora a Alpine. Se ha actualizado la imagen para [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>122</td>
<td>136</td>
<td>Se ha aumentado el tiempo de espera del cliente en {{site.data.keyword.keymanagementservicefull_notm}}. Se ha actualizado la imagen para [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.5</td>
<td>v1.12.6</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.6). La actualización resuelve [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) y [CVE-2019-1002100 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha añadido `ExperimentalCriticalPodAnnotation=true` a la opción `--feature-gates`. Este valor ayuda a migrar los pods de la anotación `scheduler.alpha.kubernetes.io/critical-pod` en desuso para [dar soporte a la prioridad de pod de Kubernetes](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Equilibrador de carga y supervisor de equilibrador de carga para el proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>132</td>
<td>143</td>
<td>Se ha actualizado la imagen para [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Cliente y servidor de OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Se ha actualizado la imagen para [CVE-2019-1559 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Se ha actualizado la imagen para [CVE-2019-6454 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.12.5_1540, publicado el 27 de febrero de 2019
{: #1125_1540}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.12.5_1540.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.5_1538">
<caption>Cambios desde la versión 1.12.5_1538</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2018-19407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.12.5_1538, publicado el 15 de febrero de 2019
{: #1125_1538}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.12.5_1538.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.5_1537">
<caption>Cambios desde la versión 1.12.5_1537</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de proxy de alta disponibilidad de maestro de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha cambiado el valor de configuración de pod `spec.priorityClassName` por `system-node-critical` y se ha establecido el valor `spec.priority` en `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>Consulte las [notas del release de containerd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.1.6). La actualización resuelve
[CVE-2019-5736 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configuración de `kubelet` de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha habilitado el control de la característica `ExperimentalCriticalPodAnnotation` para evitar el desalojo de pods estáticos críticos.</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.12.5_1537, publicado el 5 de febrero de 2019
{: #1125_1537}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.12.5_1537.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.4_1535">
<caption>Cambios desde la versión 1.12.4_1535</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Consulte las [notas del release de etcd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coreos/etcd/releases/v3.3.11). Además, las suites de cifrado soportadas para etcd están ahora están restringidas a un subconjunto con cifrado de alta resistencia (128 bits o más).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Se han actualizado las imágenes para [CVE-2019-3462 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) y
[CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.4-118</td>
<td>v1.12.5-137</td>
<td>Se ha actualizado para dar soporte al release 1.12.5 de Kubernetes. Además, la versión de `calicoctl` se ha actualizado a 3.3.1. Se han corregido actualizaciones de configuración innecesarias en los equilibradores de carga de la versión 2.0 en los cambios de estado de nodo trabajador.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>338</td>
<td>342</td>
<td>El plugin de almacenamiento de archivos se ha actualizado de la forma siguiente:
<ul><li>Admite el suministro dinámico con la [planificación de topología de volúmenes](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Pasa por alto los errores de supresión de reclamación de volumen persistente (PVC) si el almacenamiento ya se ha suprimido.</li>
<li>Añade una anotación de mensaje de anomalía a las PVC anómalas.</li>
<li>Optimiza los valores de elección de líder del controlador de suministro de almacenamiento y de periodo de resincronización y aumenta el tiempo de espera de suministro de 30 minutos a 1 hora.</li>
<li>Comprueba los permisos de usuario antes de iniciar el suministro.</li></ul></td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>111</td>
<td>122</td>
<td>Se ha añadido lógica de reintento para evitar anomalías temporales cuando los secretos de Kubernetes están gestionados por {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.12.5</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.5).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configuración de la política de auditoría del servidor de API de Kubernetes se ha actualizado para incluir los metadatos de registro para las solicitudes `cluster-admin` y el registro del cuerpo de la solicitud de las solicitudes `create`, `update`
y `patch` de la carga de trabajo.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Se ha actualizado la imagen para [CVE-2018-0734 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) y
[CVE-2018-5407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Además, la configuración de pod ahora se obtiene a partir de un secreto en lugar de obtenerse de un mapa de configuración.</td>
</tr>
<tr>
<td>Servidor OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Se ha actualizado la imagen para [CVE-2018-0734 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) y
[CVE-2018-5407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Parche de seguridad para [CVE-2018-16864 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.12.4_1535, publicado el 28 de enero de 2019
{: #1124_1535}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.12.4_1535.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.4_1534">
<caption>Cambios desde la versión 1.12.4_1534</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones en los paquetes Ubuntu instalados, que incluyen `apt` para [CVE-2019-3462 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) y [USN-3863-1 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>


### Registro de cambios para 1.12.4_1534, publicado el 21 de enero de 2019
{: #1124_1534}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.12.3_1534.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.3_1533">
<caption>Cambios desde la versión 1.12.3_1533</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.3-91</td>
<td>v1.12.4-118</td>
<td>Se ha actualizado para dar soporte al release 1.12.4 de Kubernetes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.3</td>
<td>v1.12.4</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.4).</td>
</tr>
<tr>
<td>Redimensionador de complementos de Kubernetes</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Consulte las [notas del release del redimensionador de complementos de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Panel de control de Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>Consulte las [notas del release del panel de control de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). La actualización resuelve
[CVE-2018-18264 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).</td>
</tr>
<tr>
<td>Instalador de GPU</td>
<td>390.12</td>
<td>410.79</td>
<td>Se han actualizado los controladores de GPU instalados a 410.79.</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.12.3_1533, publicado el 7 de enero de 2019
{: #1123_1533}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.12.3_1533.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.3_1532">
<caption>Cambios desde la versión 1.12.3_1532</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2017-5753, CVE-2018-18690 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.12.3_1532, publicado el 17 de diciembre de 2018
{: #1123_1532}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.12.2_1532.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.3_1531">
<caption>Cambios desde la versión 1.12.3_1531</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones de los paquetes de Ubuntu instalados.</td>
</tr>
</tbody>
</table>


### Registro de cambios para 1.12.3_1531, publicado el 5 de diciembre de 2018
{: #1123_1531}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.12.3_1531.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.2_1530">
<caption>Cambios desde la versión 1.12.2_1530</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.2-68</td>
<td>v1.12.3-91</td>
<td>Se ha actualizado para dar soporte al release 1.12.3 de Kubernetes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.2</td>
<td>v1.12.3</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.3). La actualización resuelve
[CVE-2018-1002105 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.12.2_1530, publicado el 4 de diciembre de 2018
{: #1122_1530}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.12.2_1530.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.2_1529">
<caption>Cambios desde la versión 1.12.2_1529</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilización de recursos del nodo trabajador</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han añadido cgroups dedicados para kubelet y containerd para evitar que estos componentes se puedan quedar sin recursos. Para obtener más información, consulte [Reservas de recursos del nodo trabajador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>



### Registro de cambios para 1.12.2_1529, publicado el 27 de noviembre de 2018
{: #1122_1529}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.12.2_1529.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.2_1528">
<caption>Cambios desde la versión 1.12.2_1528</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.3/releases/#v331). La actualización resuelve
[Tigera Technical Advisory TTA-2018-001 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Configuración de DNS de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha corregido un error que podía provocar que se ejecutaran los pods tanto de Kubernetes DNS como de CoreDNS tras las operaciones de creación o actualización del clúster.</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.2.0</td>
<td>v1.1.5</td>
<td>Consulte las [notas del release de containerd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.1.5). Se ha actualizado containerd para arreglar un punto muerto que puede [impedir que los pods terminen
![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/issues/2744).</td>
</tr>
<tr>
<td>Cliente y servidor de OpenVPN</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Se ha actualizado la imagen para [CVE-2018-0732 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) y
[CVE-2018-0737 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.12.2_1528, publicado el 19 de noviembre de 2018
{: #1122_1528}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.12.2_1528.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.12.2_1527">
<caption>Cambios desde la versión 1.12.2_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2018-7755 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>


### Registro de cambios para 1.12.2_1527, publicado el 7 de noviembre de 2018
{: #1122_1527}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.12.2_1527.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.3_1533">
<caption>Cambios desde la versión 1.11.3_1533</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de Calico</td>
<td>N/D</td>
<td>N/D</td>
<td>Los pods `calico-*` de Calico del espacio de nombres `kube-system` ahora establecen las solicitudes de recursos de CPU y memoria para todos los contenedores.</td>
</tr>
<tr>
<td>Proveedor de DNS de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Kubernetes DNS (KubeDNS) sigue siendo el proveedor de DNS de clúster predeterminado. No obstante, ahora tiene la opción de
[cambiar el proveedor de DNS de clúster a CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>Proveedor de métricas de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>El servidor de métricas de Kubernetes sustituye a Kubernetes Heapster (en desuso desde la versión 1.8 de Kubernetes) como proveedor de métricas de clúster. Para los elementos de acción, consulte [la acción de preparación `metrics-server`](/docs/containers?topic=containers-cs_versions#metrics-server).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.4</td>
<td>1.2.0</td>
<td>Consulte las [notas del release de containerd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.2.0).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>N/D</td>
<td>1.2.2</td>
<td>Consulte las [notas del release de CoreDNS ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coredns/coredns/releases/tag/v1.2.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.12.2</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.2).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han añadido tres nuevas políticas de pod de IBM y sus roles de clúster asociados. Para obtener más información, consulte
[Explicación de los recursos predeterminados para la gestión de clústeres de IBM](/docs/containers?topic=containers-psp#ibm_psp).</td>
</tr>
<tr>
<td>Panel de control de Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.0</td>
<td>Consulte las [notas del release del panel de control de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.0).<br><br>
Si accede al panel de control a través de `kubectl proxy`, se elimina el botón **OMITIR** de la página de inicio de sesión. En su lugar, [utilice una **Señal** para iniciar una sesión](/docs/containers?topic=containers-app#cli_dashboard). Además, ahora puede ampliar el número de pods de panel de control de Kubernetes ejecutando `kubectl -n kube-system scale deploy kubernetes-dashboard --replicas=3`.</td>
</tr>
<tr>
<td>DNS de Kubernetes</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>Consulte las [notas del release de Kubernetes DNS ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Servidor de métricas de Kubernetes</td>
<td>N/D</td>
<td>v0.3.1</td>
<td>Consulte las [notas del release del servidor de métricas de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.1).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.3-118</td>
<td>v1.12.2-68</td>
<td>Se ha actualizado para dar soporte al release 1.12 de Kubernetes. Entre los cambios adicionales se incluyen los siguientes:
<ul><li>Los pods de equilibrador de carga (`ibm-cloud-provider-ip-*` en el espacio de nombres `ibm-system`) ahora establecen las solicitudes de recursos de CPU y memoria.</li>
<li>Se ha añadido la anotación `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` para especificar la VLAN en la que se despliega el servicio de equilibrador de carga. Para ver las VLAN disponibles en el clúster, ejecute `ibmcloud ks vlans --zone <zone>`.</li>
<li>Hay un nuevo [equilibrador de carga 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) disponible en fase beta.</li></ul></td>
</tr>
<tr>
<td>Configuración del cliente OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>El cliente OpenVPN `vpn-* pod` del espacio de nombres `kube-system` ahora establece las solicitudes de recursos de CPU y memoria.</td>
</tr>
</tbody>
</table>

## En desuso: Registro de cambios de la versión 1.11
{: #111_changelog}

Revise el registro de cambios de la versión 1.11.
{: shortdesc}

La versión 1.11 de Kubernetes está en desuso y dejará de recibir soporte a partir del 27 de junio de 2019 (provisional). [Revise el impacto potencial](/docs/containers?topic=containers-cs_versions#cs_versions) de cada actualización de versión de Kubernetes y luego [actualice los clústeres](/docs/containers?topic=containers-update#update) inmediatamente al menos a la versión 1.12.
{: deprecated}

### Registro de cambios para 1.11.10_1561, publicado el 4 de junio de 2019
{: #11110_1561}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.10_1561.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.10_1559">
<caption>Cambios desde la versión 1.11.10_1559</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de alta disponibilidad de nodo maestro de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha actualizado la configuración para minimizar los errores intermitentes de conectividad de red maestra durante una actualización de nodo maestro.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Consulte las [notas del release de etcd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Se ha actualizado la imagen para [CVE-2018-10844 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) y [CVE-2019-5436 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Se ha actualizado la imagen para [CVE-2018-10844 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) y [CVE-2019-5436 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.11.10_1559, publicado el 20 de mayo de 2019
{: #11110_1559}

En la tabla siguiente se muestran los cambios incluidos en el paquete de parches 1.11.10_1559.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.10_1558">
<caption>Cambios desde la versión 1.11.10_1558</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel de Ubuntu 16.04</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-12126 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) y [CVE-2018-12130 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Kernel de Ubuntu 18.04</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-12126 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) y [CVE-2018-12130 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.11.10_1558, publicado el 13 de mayo de 2019
{: #11110_1558}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.10_1558.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.9_1556">
<caption>Cambios desde la versión 1.11.9_1556</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy de alta disponibilidad de maestro de clúster</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Consulte las [notas del release de HAProxy ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La actualización resuelve
[CVE-2019-6706 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Se ha actualizado la imagen para [CVE-2019-1543 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.9-241</td>
<td>v1.11.10-270</td>
<td>Se ha actualizado para dar soporte al release 1.11.10 de Kubernetes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.9</td>
<td>v1.11.10</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.10).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configuración de la política de auditoría del servidor de API de Kubernetes se ha actualizado para que no registre el URL de solo lectura `/openapi/v2*`. Además, la configuración del gestor del controlador de Kubernetes ha aumentado la duración de validez de los certificados de `kubelet` firmados de 1 a 3 años.</td>
</tr>
<tr>
<td>Configuración del cliente OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>El pod `vpn-*` del cliente de OpenVPN del espacio de nombres `kube-system` ahora establece `dnsPolicy` en `Default` para evitar que el pod falle cuando el DNS del clúster esté inactivo.</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Se ha actualizado la imagen para [CVE-2016-7076 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) y [CVE-2019-11068 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.11.9_1556, publicado el 29 de abril de 2019
{: #1119_1556}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.11.9_1556.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.9_1555">
<caption>Cambios desde la versión 1.11.9_1555</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones de los paquetes de Ubuntu instalados.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>Consulte las [notas del release de containerd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.1.7).</td>
</tr>
</tbody>
</table>


### Registro de cambios para el fixpack de nodo trabajador 1.11.9_1555, publicado el 15 de abril de 2019
{: #1119_1555}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.11.9_1555.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.9_1554">
<caption>Cambios desde la versión 1.11.9_1554</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones en los paquetes Ubuntu instalados, que incluyen `systemd` para [CVE-2019-3842 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.11.9_1554, publicado el 8 de abril de 2019
{: #1119_1554}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.9_1554.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.8_1553">
<caption>Cambios desde la versión 1.11.8_1553</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.3/releases/#v336). La actualización resuelve
[CVE-2019-9946 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Proxy de alta disponibilidad de maestro de clúster</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Consulte las [notas del release de HAProxy ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La actualización resuelve [CVE-2018-0732 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) y [CVE-2019-1559 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.8-219</td>
<td>v1.11.9-241</td>
<td>Se ha actualizado para dar soporte a los releases 1.11.9 de Kubernetes y 3.3.6 de Calico.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.8</td>
<td>v1.11.9</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.9).</td>
</tr>
<tr>
<td>DNS de Kubernetes</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>Consulte las [notas del release de Kubernetes DNS ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Se ha actualizado la imagen para [CVE-2017-12447 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Kernel de Ubuntu 16.04</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2019-9213 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Kernel de Ubuntu 18.04</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2019-9213 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.11.8_1553, publicado el 1 de abril de 2019
{: #1118_1553}

En la siguiente tabla se muestran los cambios incluidos en el arreglo de nodo trabajador 1.11.8_1553.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.8_1552">
<caption>Cambios desde la versión 1.11.8_1552</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilización de recursos del nodo trabajador</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han incrementado las reservas de memoria para kubelet y containerd para evitar que estos componentes se puedan quedar sin recursos. Para obtener más información, consulte [Reservas de recursos del nodo trabajador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Registro de cambios para fixpack de nodo maestro 1.11.8_1552, publicado el 26 de marzo de 2019
{: #1118_1552}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo maestro 1.11.8_1552.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.8_1550">
<caption>Cambios desde la versión 1.11.8_1550</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>345</td>
<td>346</td>
<td>Se ha actualizado la imagen para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>166</td>
<td>167</td>
<td>Corrige errores intermitentes de `context deadline exceeded` y `timeout` para gestionar secretos de Kubernetes. Además, corrige las actualizaciones al servicio de gestión de claves que podrían dejar secretos existentes de Kubernetes sin cifrar. La actualización incluye el arreglo para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Equilibrador de carga y supervisor de equilibrador de carga para el proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>143</td>
<td>146</td>
<td>Se ha actualizado la imagen para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.11.8_1550, publicado el 20 de marzo de 2019
{: #1118_1550}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.8_1550.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.8_1547">
<caption>Cambios desde la versión 1.11.8_1547</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de proxy de alta disponibilidad de maestro de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha actualizado la configuración para manejar mejor los errores de conexión intermitentes con el clúster maestro.</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Se han actualizado los controladores de GPU a [418.43 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.nvidia.com/object/unix.html). La actualización incluye el arreglo para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>344</td>
<td>345</td>
<td>Se ha añadido soporte para [puntos finales de servicio privados](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2019-6133 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>136</td>
<td>166</td>
<td>Se ha actualizado la imagen para [CVE-2018-16890 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) y [CVE-2019-3823 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Se ha actualizado la imagen para [CVE-2018-10779 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) y [CVE-2019-7663 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.11.8_1547, publicado el 4 de marzo de 2019
{: #1118_1547}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.8_1547.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.7_1546">
<caption>Cambios desde la versión 1.11.7_1546</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Se han actualizado las imágenes para [CVE-2019-6454 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.7-198</td>
<td>v1.11.8-219</td>
<td>Se ha actualizado para dar soporte al release 1.11.8 de Kubernetes. Se han solucionado problemas de conectividad periódicos para los equilibradores de carga que establecen `externalTrafficPolicy` en `local`. Se han actualizado los sucesos del equilibrador de carga para que utilice los últimos enlaces de la documentación de {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>342</td>
<td>344</td>
<td>Se ha cambiado el sistema operativo base para la imagen de Fedora a Alpine. Se ha actualizado la imagen para [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>122</td>
<td>136</td>
<td>Se ha aumentado el tiempo de espera del cliente en {{site.data.keyword.keymanagementservicefull_notm}}. Se ha actualizado la imagen para [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.7</td>
<td>v1.11.8</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.8). La actualización resuelve [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) y [CVE-2019-1002100 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha añadido `ExperimentalCriticalPodAnnotation=true` a la opción `--feature-gates`. Este valor ayuda a migrar los pods de la anotación `scheduler.alpha.kubernetes.io/critical-pod` en desuso para [dar soporte a la prioridad de pod de Kubernetes](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>DNS de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha aumentado el límite de memoria de pod de DNS de `170 Mi` a `400 Mi` para poder gestionar más servicios de clúster.</td>
</tr>
<tr>
<td>Equilibrador de carga y supervisor de equilibrador de carga para el proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>132</td>
<td>143</td>
<td>Se ha actualizado la imagen para [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Cliente y servidor de OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Se ha actualizado la imagen para [CVE-2019-1559 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Se ha actualizado la imagen para [CVE-2019-6454 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.11.7_1546, publicado el 27 de febrero de 2019
{: #1117_1546}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.11.7_1546.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.7_1546">
<caption>Cambios desde la versión 1.11.7_1546</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2018-19407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.11.7_1544, publicado el 15 de febrero de 2019
{: #1117_1544}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.11.7_1544.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.7_1543">
<caption>Cambios desde la versión 1.11.7_1543</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de proxy de alta disponibilidad de maestro de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha cambiado el valor de configuración de pod `spec.priorityClassName` por `system-node-critical` y se ha establecido el valor `spec.priority` en `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>Consulte las [notas del release de containerd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.1.6). La actualización resuelve
[CVE-2019-5736 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configuración de `kubelet` de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha habilitado el control de la característica `ExperimentalCriticalPodAnnotation` para evitar el desalojo de pods estáticos críticos.</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.11.7_1543, publicado el 5 de febrero de 2019
{: #1117_1543}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.7_1543.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.6_1541">
<caption>Cambios desde la versión 1.11.6_1541</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Consulte las [notas del release de etcd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coreos/etcd/releases/v3.3.11). Además, las suites de cifrado soportadas para etcd están ahora están restringidas a un subconjunto con cifrado de alta resistencia (128 bits o más).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Se han actualizado las imágenes para [CVE-2019-3462 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) y
[CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.6-180</td>
<td>v1.11.7-198</td>
<td>Se ha actualizado para dar soporte al release 1.11.7 de Kubernetes. Además, la versión de `calicoctl` se ha actualizado a 3.3.1. Se han corregido actualizaciones de configuración innecesarias en los equilibradores de carga de la versión 2.0 en los cambios de estado de nodo trabajador.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>338</td>
<td>342</td>
<td>El plugin de almacenamiento de archivos se ha actualizado de la forma siguiente:
<ul><li>Admite el suministro dinámico con la [planificación de topología de volúmenes](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Pasa por alto los errores de supresión de reclamación de volumen persistente (PVC) si el almacenamiento ya se ha suprimido.</li>
<li>Añade una anotación de mensaje de anomalía a las PVC anómalas.</li>
<li>Optimiza los valores de elección de líder del controlador de suministro de almacenamiento y de periodo de resincronización y aumenta el tiempo de espera de suministro de 30 minutos a 1 hora.</li>
<li>Comprueba los permisos de usuario antes de iniciar el suministro.</li></ul></td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>111</td>
<td>122</td>
<td>Se ha añadido lógica de reintento para evitar anomalías temporales cuando los secretos de Kubernetes están gestionados por {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.6</td>
<td>v1.11.7</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.7).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configuración de la política de auditoría del servidor de API de Kubernetes se ha actualizado para incluir los metadatos de registro para las solicitudes `cluster-admin` y el registro del cuerpo de la solicitud de las solicitudes `create`, `update`
y `patch` de la carga de trabajo.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Se ha actualizado la imagen para [CVE-2018-0734 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) y
[CVE-2018-5407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Además, la configuración de pod ahora se obtiene a partir de un secreto en lugar de obtenerse de un mapa de configuración.</td>
</tr>
<tr>
<td>Servidor OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Se ha actualizado la imagen para [CVE-2018-0734 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) y
[CVE-2018-5407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Parche de seguridad para [CVE-2018-16864 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.11.6_1541, publicado el 28 de enero de 2019
{: #1116_1541}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.11.6_1541.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.6_1540">
<caption>Cambios desde la versión 1.11.6_1540</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones en los paquetes Ubuntu instalados, que incluyen `apt` para [CVE-2019-3462 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) y [USN-3863-1 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.11.6_1540, publicado el 21 de enero de 2019
{: #1116_1540}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.6_1540.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.5_1539">
<caption>Cambios desde la versión 1.11.5_1539</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.5-152</td>
<td>v1.11.6-180</td>
<td>Se ha actualizado para dar soporte al release 1.11.6 de Kubernetes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.5</td>
<td>v1.11.6</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.6).</td>
</tr>
<tr>
<td>Redimensionador de complementos de Kubernetes</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Consulte las [notas del release del redimensionador de complementos de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Panel de control de Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>Consulte las [notas del release del panel de control de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). La actualización resuelve
[CVE-2018-18264 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).<br><br>Si accede al panel de control a través de `kubectl proxy`, se elimina el botón **OMITIR** de la página de inicio de sesión. En su lugar, [utilice una **Señal** para iniciar una sesión](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td>Instalador de GPU</td>
<td>390.12</td>
<td>410.79</td>
<td>Se han actualizado los controladores de GPU instalados a 410.79.</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.11.5_1539, publicado el 7 de enero de 2019
{: #1115_1539}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.11.5_1539.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.5_1538">
<caption>Cambios desde la versión 1.11.5_1538</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2017-5753, CVE-2018-18690 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.11.5_1538, publicado el 17 de diciembre de 2018
{: #1115_1538}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.11.5_1538.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.5_1537">
<caption>Cambios desde la versión 1.11.5_1537</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones de los paquetes de Ubuntu instalados.</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.11.5_1537, publicado el 5 de diciembre de 2018
{: #1115_1537}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.5_1537.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.4_1536">
<caption>Cambios desde la versión 1.11.4_1536</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.4-142</td>
<td>v1.11.5-152</td>
<td>Se ha actualizado para dar soporte al release 1.11.5 de Kubernetes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.4</td>
<td>v1.11.5</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.5). La actualización resuelve
[CVE-2018-1002105 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.11.4_1536, publicado el 4 de diciembre de 2018
{: #1114_1536}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.11.4_1536.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.4_1535">
<caption>Cambios desde la versión 1.11.4_1535</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilización de recursos del nodo trabajador</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han añadido cgroups dedicados para kubelet y containerd para evitar que estos componentes se puedan quedar sin recursos. Para obtener más información, consulte [Reservas de recursos del nodo trabajador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.11.4_1535, publicado el 27 de noviembre de 2018
{: #1114_1535}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.4_1535.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.3_1534">
<caption>Cambios desde la versión 1.11.3_1534</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.3/releases/#v331). La actualización resuelve
[Tigera Technical Advisory TTA-2018-001 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.1.4</td>
<td>v1.1.5</td>
<td>Consulte las [notas del release de containerd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.1.5). Se ha actualizado containerd para arreglar un punto muerto que puede [impedir que los pods terminen
![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/issues/2744).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.3-127</td>
<td>v1.11.4-142</td>
<td>Se ha actualizado para dar soporte al release 1.11.4 de Kubernetes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.11.4</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.4).</td>
</tr>
<tr>
<td>Cliente y servidor de OpenVPN</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Se ha actualizado la imagen para [CVE-2018-0732 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) y
[CVE-2018-0737 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.11.3_1534, publicado el 19 de noviembre de 2018
{: #1113_1534}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.11.3_1534.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.3_1533">
<caption>Cambios desde la versión 1.11.3_1533</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2018-7755 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>


### Registro de cambios para 1.11.3_1533, publicado el 7 de noviembre de 2018
{: #1113_1533}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.3_1533.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.3_1531">
<caption>Cambios desde la versión 1.11.3_1531</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Actualización de maestro de clúster de alta disponibilidad</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha corregido la actualización a maestros de alta disponibilidad (HA) para clústeres que utilizan webhooks de admisión como
`initializerconfigurations`, `mutatingwebhookconfigurations` o `validatingwebhookconfigurations`. Puede utilizar estos webhooks con diagramas de Helm como, por ejemplo, para la [imposición de seguridad de imágenes de contenedores](/docs/services/Registry?topic=registry-security_enforce#security_enforce).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.3-100</td>
<td>v1.11.3-127</td>
<td>Se ha añadido la anotación `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` para especificar la VLAN en la que se despliega el servicio de equilibrador de carga. Para ver las VLAN disponibles en el clúster, ejecute `ibmcloud ks vlans --zone <zone>`.</td>
</tr>
<tr>
<td>Kernel habilitado para TPM</td>
<td>N/D</td>
<td>N/D</td>
<td>Los nodos trabajadores nativos con chips TPM para Trusted Compute utilizan el kernel de Ubuntu predeterminado hasta que se habilita la confianza. Si
[habilita la confianza](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) en un clúster existente, necesitará
[volver a cargar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) los nodos trabajadores nativos existentes con chips TPM. Para comprobar si un nodo trabajador local tiene un chip TPM, revise el campo **Trustable** después de ejecutar
el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-types --zone`.</td>
</tr>
</tbody>
</table>

### Registro de cambios para fixpack de nodo maestro 1.11.3_1531, publicado el 1 de noviembre de 2018
{: #1113_1531_ha-master}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo maestro 1.11.3_1531.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.3_1527">
<caption>Cambios desde la versión 1.11.3_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Maestro de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha actualizado la configuración del maestro del clúster para aumentar la alta disponibilidad (HA). Los clústeres tienen ahora tres réplicas del maestro de Kubernetes que se configuran con una configuración de alta disponibilidad (HA), con cada maestro desplegado en hosts físicos independientes. Además, si el clúster está en una zona con capacidad multizona, los maestros se distribuyen entre las zonas.<br>Para ver las acciones que debe realizar, consulte
[Actualización a maestros de clúster de alta disponibilidad](/docs/containers?topic=containers-cs_versions#ha-masters). Estas acciones preparatorias se aplican:<ul>
<li>Si tiene un cortafuegos o políticas de red Calico personalizadas.</li>
<li>Si utiliza los puertos de host `2040` o `2041` en sus nodos trabajadores.</li>
<li>Si ha utilizado la dirección IP del maestro del clúster para el acceso al maestro dentro del clúster.</li>
<li>Si tiene una automatización que llama a la API o a la CLI de Calico (`calicoctl`), como para crear políticas de Calico.</li>
<li>Si utiliza políticas de red de Kubernetes o Calico para controlar el acceso de salida de pod al maestro.</li></ul></td>
</tr>
<tr>
<td>Proxy de alta disponibilidad de maestro de clúster</td>
<td>N/D</td>
<td>1.8.12-alpine</td>
<td>Se ha añadido un pod `ibm-master-proxy-*` para el equilibrio de carga del lado del cliente en todos los nodos trabajadores, de manera que cada cliente de nodo trabajador pueda direccionar solicitudes a una réplica disponible del maestro de alta disponibilidad.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>Consulte las [notas del release de etcd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coreos/etcd/releases/v3.3.1).</td>
</tr>
<tr>
<td>Cifrado de datos en etcd</td>
<td>N/D</td>
<td>N/D</td>
<td>Anteriormente, los datos de etcd se almacenaban en una instancia de almacenamiento de archivos NFS del maestro que se cifraba en reposo. Ahora, los datos de etcd se almacenan en el disco local del maestro y se realiza una copia de seguridad en {{site.data.keyword.cos_full_notm}}. Los datos se cifran durante el tránsito a {{site.data.keyword.cos_full_notm}} y en reposo. No obstante, los datos de etcd en el disco local del maestro no se cifran. Si desea que se cifren los datos de etcd locales del maestro, [habilite {{site.data.keyword.keymanagementservicelong_notm}} en el clúster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.11.3_1531, publicado el 26 de octubre de 2018
{: #1113_1531}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.11.3_1531.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.3_1525">
<caption>Cambios desde la versión 1.11.3_1525</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Gestión de interrupciones del sistema operativo</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha sustituido el daemon del sistema de solicitud de interrupción (IRQ) por un manejador de interrupciones de mayor rendimiento.</td>
</tr>
</tbody>
</table>

### Registro de cambios para fixpack de nodo maestro 1.11.3_1527, publicado el 15 de octubre de 2018
{: #1113_1527}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo maestro 1.11.3_1527.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.3_1524">
<caption>Cambios desde la versión 1.11.3_1524</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de Calico</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha corregido el analizador de preparación del contenedor `calico-node` para manejar mejor las anomalías de nodo.</td>
</tr>
<tr>
<td>Actualización de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha corregido el problema con la actualización de los complementos de clúster cuando el nodo maestro se actualiza desde una versión no soportada.</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.11.3_1525, publicado el 10 de octubre de 2018
{: #1113_1525}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.11.3_1525.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.3_1524">
<caption>Cambios desde la versión 1.11.3_1524</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-14633, CVE-2018-17182 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Tiempo de espera de sesión inactiva</td>
<td>N/D</td>
<td>N/D</td>
<td>Establezca el tiempo de espera de sesión inactiva en 5 minutos por razones de conformidad.</td>
</tr>
</tbody>
</table>


### Registro de cambios para 1.11.3_1524, publicado el 2 de octubre de 2018
{: #1113_1524}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.3_1524.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.3_1521">
<caption>Cambios desde la versión 1.11.3_1521</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>1.1.3</td>
<td>1.1.4</td>
<td>Consulte las [notas del release de containerd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.1.4).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.3-91</td>
<td>v1.11.3-100</td>
<td>Se ha actualizado el enlace de la documentación en los mensajes de error del equilibrador de carga.</td>
</tr>
<tr>
<td>Clases de almacenamiento de archivos de IBM</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha eliminado el parámetro `reclaimPolicy` duplicado de las clases de almacenamiento de archivos de IBM.<br><br>
Además, ahora, cuando actualiza el nodo maestro del clúster, la clase de almacenamiento de archivos predeterminada de IBM no se modifica. Si desea cambiar la clase de almacenamiento predeterminada, ejecute el mandato `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` y sustituya `<storageclass>` por el nombre de la clase de almacenamiento.</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.11.3_1521, publicado el 20 de septiembre de 2018
{: #1113_1521}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.3_1521.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.2_1516">
<caption>Cambios desde la versión 1.11.2_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.2-71</td>
<td>v1.11.3-91</td>
<td>Se ha actualizado para dar soporte al release 1.11.3 de Kubernetes.</td>
</tr>
<tr>
<td>Clases de almacenamiento de archivos de IBM</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha eliminado `mountOptions` en las clases de almacenamiento de archivos de IBM para utilizar el valor predeterminado que proporciona el nodo trabajador.<br><br>
Además, ahora, cuando actualiza el nodo maestro del clúster, la clase de almacenamiento de archivos predeterminada de IBM sigue siendo `ibmc-file-bronze`. Si desea cambiar la clase de almacenamiento predeterminada, ejecute el mandato `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` y sustituya `<storageclass>` por el nombre de la clase de almacenamiento.</td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha añadido la capacidad de utilizar el proveedor Kubernetes de servicio de gestión de claves (KMS) en el clúster, para dar soporte a {{site.data.keyword.keymanagementservicefull}}. Cuando se [habilita {{site.data.keyword.keymanagementserviceshort}} en el clúster](/docs/containers?topic=containers-encryption#keyprotect), todos los secretos de Kubernetes se cifran.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.2</td>
<td>v1.11.3</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.3).</td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>Consulte las [notas del release de Kubernetes DNS Autoscaler ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0).</td>
</tr>
<tr>
<td>Rotación de registros</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha modificado para utilizar temporizadores `systemd` en lugar de `cronjobs` para evitar que `logrotate` falle en los nodos trabajadores que no se han vuelto a cargar o no se han actualizado en 90 días. **Nota**: en
todas las versiones anteriores de este release menor, el disco primario se llenaba después de que fallar el trabajo cron porque los registros no se rotaban. El trabajo cron falla después de que el nodo trabajador esté activo durante 90 días sin que se haya actualizado o recargado. Si los registros llenan todo el disco primario, el nodo trabajador entra en un estado de error. El nodo trabajador se puede arreglar mediante el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` o el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>Caducidad de la contraseña raíz</td>
<td>N/D</td>
<td>N/D</td>
<td>Las contraseñas raíz para los nodos trabajadores caducan después de 90 días por razones de conformidad. Si las herramientas de automatización tienen que iniciar la sesión en el nodo trabajador como usuario root o se basan en trabajos cron que se ejecutan como root, puede inhabilitar la caducidad de la contraseña iniciando una sesión en el nodo trabajador y ejecutando `chage -M -1 root`. **Nota**: si tiene requisitos de conformidad de seguridad que impiden la ejecución como root o la eliminación de la caducidad de la contraseña, no inhabilite la caducidad. En lugar de ello, puede [actualizar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) o [volver a cargar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) los nodos trabajadores al menos cada 90 días.</td>
</tr>
<tr>
<td>Componentes de tiempo de ejecución de nodo trabajador (`kubelet`, `kube-proxy`, `containerd`)</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han eliminado las dependencias de los componentes de tiempo de ejecución en el disco primario. Esta mejora impide que los nodos trabajadores fallen cuando se llena el disco primario.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/D</td>
<td>N/D</td>
<td>Limpie de forma periódica las unidades de montaje transitorias para evitar que se desborden. Esta acción está relacionada con el [problema de Kubernetes 57345 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.11.2_1516, publicado el 4 de septiembre de 2018
{: #1112_1516}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.2_1516.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.2_1514">
<caption>Cambios desde la versión 1.11.2_1514</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.2/releases/#v321).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.2</td>
<td>1.1.3</td>
<td>Consulte las [notas del release de `containerd` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.1.3).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.2-60</td>
<td>v1.11.2-71</td>
<td>Se ha cambiado la configuración del proveedor de nube para manejar mejor las actualizaciones para los servicios de equilibrador de carga con `externalTrafficPolicy` establecido en `local`.</td>
</tr>
<tr>
<td>Configuración del plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha eliminado la versión de NFS predeterminada de las opciones de montaje de las clases de almacenamiento de archivos proporcionadas por IBM. El sistema operativo del host ahora negocia la versión de NFS con el servidor NFS de la infraestructura de IBM Cloud (SoftLayer). Para establecer manualmente una versión de NFS específica, o para cambiar la versión de NFS de su PV que ha sido negociada por el sistema operativo del host, consulte [Cambio de la versión predeterminada de NFS](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fixpack de nodo trabajador 1.11.2_1514, publicado el 23 de agosto de 2018
{: #1112_1514}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.11.2_1514.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.11.2_1513">
<caption>Cambios desde la versión 1.11.2_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Se ha actualizado `systemd` para arreglar la fuga de `cgroup`.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-3620, CVE-2018-3646 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.11.2_1513, publicado el 14 de agosto de 2018
{: #1112_1513}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.11.2_1513.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.5_1518">
<caption>Cambios desde la versión 1.10.5_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>N/D</td>
<td>1.1.2</td>
<td>`containerd` sustituye a Docker como el nuevo tiempo de ejecución de contenedor para Kubernetes. Consulte las [notas del release de `containerd` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/containerd/containerd/releases/tag/v1.1.2). Para conocer las acciones que debe llevar a cabo, consulte [Actualización a `containerd` como tiempo de ejecución de contenedor](/docs/containers?topic=containers-cs_versions#containerd).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/D</td>
<td>N/D</td>
<td>`containerd` sustituye a Docker como el nuevo tiempo de ejecución de contenedor para Kubernetes, para mejorar el rendimiento. Para conocer las acciones que debe llevar a cabo, consulte [Actualización a `containerd` como tiempo de ejecución de contenedor](/docs/containers?topic=containers-cs_versions#containerd).</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.14</td>
<td>v3.2.18</td>
<td>Consulte las [notas del release de etcd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coreos/etcd/releases/v3.2.18).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.5-118</td>
<td>v1.11.2-60</td>
<td>Se ha actualizado para dar soporte al release 1.11 de Kubernetes. Además, los pods de equilibrador de carga ahora utilizan la nueva clase de prioridad de pod `ibm-app-cluster-critical`.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>334</td>
<td>338</td>
<td>Se ha actualizado la versión de `incubator` a 1.8. El almacenamiento de archivos se suministra a la zona específica que seleccione. No puede actualizar las etiquetas de instancia de PV existentes (estáticas) a menos que esté utilizando un clúster multizona y tenga que [añadir las etiquetas de zona y de región](/docs/containers?topic=containers-kube_concepts#storage_multizone).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.11.2</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.2).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha actualizado la configuración de OpenID Connect para el servidor de API de Kubernetes del clúster para dar soporte a los grupos de acceso de {{site.data.keyword.Bluemix_notm}} Identity Access and Management (IAM). Se ha añadido `Priority` a la opción `--enable-admission-plugins` para el servidor de API de Kubernetes del clúster y se ha configurado el clúster para que dé soporte a la prioridad de pod. Para obtener más información, consulte:
<ul><li>[Grupos de acceso de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#rbac)</li>
<li>[Configuración de la prioridad de pod](/docs/containers?topic=containers-pod_priority#pod_priority)</li></ul></td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.2</td>
<td>v.1.5.4</td>
<td>Se han aumentado los límites de recursos para el contenedor `heapster-nanny`. Consulte las notas del release de [Kubernetes Heapster ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/heapster/releases/tag/v1.5.4).</td>
</tr>
<tr>
<td>Configuración de registro</td>
<td>N/D</td>
<td>N/D</td>
<td>El directorio de registro de contenedor ahora es `/var/log/pods/` en lugar del valor anterior `/var/lib/docker/containers/`.</td>
</tr>
</tbody>
</table>

<br />


## Archivo
{: #changelog_archive}

Versiones no soportadas de Kubernetes:
*  [Versión 1.10](#110_changelog)
*  [Versión 1.9](#19_changelog)
*  [Versión 1.8](#18_changelog)
*  [Versión 1.7](#17_changelog)

### Registro de cambios de la versión 1.10 (sin soporte desde el 16 de mayo de 2019)
{: #110_changelog}

Revise los registros de cambios de la versión 1.10.
{: shortdesc}

*   [registro de cambios para el fixpack de nodo trabajador 1.10.13_1558, publicado el 13 de mayo de 2019](#11013_1558)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.13_1557, publicado el 29 de abril de 2019](#11013_1557)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.13_1556, publicado el 15 de abril de 2019](#11013_1556)
*   [Registro de cambios para 1.10.13_1555, publicado el 8 de abril de 2019](#11013_1555)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.13_1554, publicado el 1 de abril de 2019](#11013_1554)
*   [Registro de cambios para fixpack de nodo maestro 1.10.13_1553, publicado el 26 de marzo de 2019](#11118_1553)
*   [Registro de cambios para 1.10.13_1551, publicado el 20 de marzo de 2019](#11013_1551)
*   [Registro de cambios para 1.10.13_1548, publicado el 4 de marzo de 2019](#11013_1548)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.12_1546, publicado el 27 de febrero de 2019](#11012_1546)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.12_1544, publicado el 15 de febrero de 2019](#11012_1544)
*   [Registro de cambios para 1.10.12_1543, publicado el 5 de febrero de 2019](#11012_1543)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.12_1541, publicado el 28 de enero de 2019](#11012_1541)
*   [Registro de cambios para 1.10.12_1540, publicado el 21 de enero de 2019](#11012_1540)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.11_1538, publicado el 7 de enero de 2019](#11011_1538)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.11_1537, publicado el 17 de diciembre de 2018](#11011_1537)
*   [Registro de cambios para 1.10.11_1536, publicado el 4 de diciembre de 2018](#11011_1536)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.8_1532, publicado el 27 de noviembre de 2018](#1108_1532)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.8_1531, publicado el 19 de noviembre de 2018](#1108_1531)
*   [Registro de cambios para 1.10.8_1530, publicado el 7 de noviembre de 2018](#1108_1530_ha-master)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.8_1528, publicado el 26 de octubre de 2018](#1108_1528)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.8_1525, publicado el 10 de octubre de 2018](#1108_1525)
*   [Registro de cambios para 1.10.8_1524, publicado el 2 de octubre de 2018](#1108_1524)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.7_1521, publicado el 20 de septiembre de 2018](#1107_1521)
*   [Registro de cambios para 1.10.7_1520, publicado el 4 de septiembre de 2018](#1107_1520)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.5_1519, publicado el 23 de agosto de 2018](#1105_1519)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.5_1518, publicado el 13 de agosto de 2018](#1105_1518)
*   [Registro de cambios para 1.10.5_1517, publicado el 27 de julio de 2018](#1105_1517)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.3_1514, publicado el 3 de julio de 2018](#1103_1514)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.3_1513, publicado el 21 de junio de 2018](#1103_1513)
*   [Registro de cambios para 1.10.3_1512, publicado el 12 de junio de 2018](#1103_1512)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.1_1510, publicado el 18 de mayo de 2018](#1101_1510)
*   [Registro de cambios para el fixpack de nodo trabajador 1.10.1_1509, publicado el 16 de mayo de 2018](#1101_1509)
*   [Registro de cambios para 1.10.1_1508, publicado el 01 de mayo de 2018](#1101_1508)

#### Registro de cambios para el fixpack de nodo trabajador 1.10.13_1558, publicado el 13 de mayo de 2019
{: #11013_1558}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.13_1558.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.13_1557">
<caption>Cambios desde la versión 1.10.13_1557</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy de alta disponibilidad de maestro de clúster</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Consulte las [notas del release de HAProxy ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La actualización resuelve
[CVE-2019-6706 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.13_1557, publicado el 29 de abril de 2019
{: #11013_1557}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.13_1557.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.13_1556">
<caption>Cambios desde 1.10.13_1556</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones de los paquetes de Ubuntu instalados.</td>
</tr>
</tbody>
</table>


#### Registro de cambios para el fixpack de nodo trabajador 1.10.13_1556, publicado el 15 de abril de 2019
{: #11013_1556}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.13_1556.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.13_1555">
<caption>Cambios desde la versión 1.10.13_1555</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones en los paquetes Ubuntu instalados, que incluyen `systemd` para [CVE-2019-3842 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.10.13_1555, publicado el 8 de abril de 2019
{: #11013_1555}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.10.13_1555.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.13_1554">
<caption>Cambios desde la versión 1.10.13_1554</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy de alta disponibilidad de maestro de clúster</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Consulte las [notas del release de HAProxy ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La actualización resuelve [CVE-2018-0732 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) y [CVE-2019-1559 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>DNS de Kubernetes</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>Consulte las [notas del release de Kubernetes DNS ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Se ha actualizado la imagen para [CVE-2017-12447 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Kernel de Ubuntu 16.04</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2019-9213 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Kernel de Ubuntu 18.04</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2019-9213 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.13_1554, publicado el 1 de abril de 2019
{: #11013_1554}

En la siguiente tabla se muestran los cambios incluidos en el arreglo de nodo trabajador 1.10.13_1554.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.13_1553">
<caption>Cambios desde la versión 1.10.13_1553</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilización de recursos del nodo trabajador</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han incrementado las reservas de memoria para kubelet y containerd para evitar que estos componentes se puedan quedar sin recursos. Para obtener más información, consulte [Reservas de recursos del nodo trabajador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>


#### Registro de cambios para fixpack de nodo maestro 1.10.13_1553, publicado el 26 de marzo de 2019
{: #11118_1553}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo maestro 1.10.13_1553.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.13_1551">
<caption>Cambios desde la versión 1.10.13_1551</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>345</td>
<td>346</td>
<td>Se ha actualizado la imagen para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>166</td>
<td>167</td>
<td>Corrige errores intermitentes de `context deadline exceeded` y `timeout` para gestionar secretos de Kubernetes. Además, corrige las actualizaciones al servicio de gestión de claves que podrían dejar secretos existentes de Kubernetes sin cifrar. La actualización incluye el arreglo para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Equilibrador de carga y supervisor de equilibrador de carga para el proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>143</td>
<td>146</td>
<td>Se ha actualizado la imagen para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.10.13_1551, publicado el 20 de marzo de 2019
{: #11013_1551}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.10.13_1551.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.13_1548">
<caption>Cambios desde la versión 1.10.13_1548</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de proxy de alta disponibilidad de maestro de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha actualizado la configuración para manejar mejor los errores de conexión intermitentes con el clúster maestro.</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Se han actualizado los controladores de GPU a [418.43 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.nvidia.com/object/unix.html). La actualización incluye el arreglo para [CVE-2019-9741 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>344</td>
<td>345</td>
<td>Se ha añadido soporte para [puntos finales de servicio privados](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2019-6133 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>136</td>
<td>166</td>
<td>Se ha actualizado la imagen para [CVE-2018-16890 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) y [CVE-2019-3823 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Se ha actualizado la imagen para [CVE-2018-10779 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) y [CVE-2019-7663 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.10.13_1548, publicado el 4 de marzo de 2019
{: #11013_1548}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.10.13_1548.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.12_1546">
<caption>Cambios desde la versión 1.10.12_1546</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Se han actualizado las imágenes para [CVE-2019-6454 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.12-252</td>
<td>v1.10.13-288</td>
<td>Se ha actualizado para dar soporte al release 1.10.13 de Kubernetes. Se han solucionado problemas de conectividad periódicos para los equilibradores de carga que establecen `externalTrafficPolicy` en `local`. Se han actualizado los sucesos del equilibrador de carga para que utilice los últimos enlaces de la documentación de {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>342</td>
<td>344</td>
<td>Se ha cambiado el sistema operativo base para la imagen de Fedora a Alpine. Se ha actualizado la imagen para [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>122</td>
<td>136</td>
<td>Se ha aumentado el tiempo de espera del cliente en {{site.data.keyword.keymanagementservicefull_notm}}. Se ha actualizado la imagen para [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.12</td>
<td>v1.10.13</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.13).</td>
</tr>
<tr>
<td>DNS de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha aumentado el límite de memoria de pod de DNS de `170 Mi` a `400 Mi` para poder gestionar más servicios de clúster.</td>
</tr>
<tr>
<td>Equilibrador de carga y supervisor de equilibrador de carga para el proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>132</td>
<td>143</td>
<td>Se ha actualizado la imagen para [CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Cliente y servidor de OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Se ha actualizado la imagen para [CVE-2019-1559 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agente de cálculo fiable</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Se ha actualizado la imagen para [CVE-2019-6454 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.12_1546, publicado el 27 de febrero de 2019
{: #11012_1546}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.12_1546.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.12_1544">
<caption>Cambios desde la versión 1.10.12_1544</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2018-19407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.12_1544, publicado el 15 de febrero de 2019
{: #11012_1544}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.12_1544.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.12_1543">
<caption>Cambios desde la versión 1.10.12_1543</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>18.06.1-ce</td>
<td>18.06.2-ce</td>
<td>Consulte las [notas del release de Docker Community Edition ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/docker/docker-ce/releases/tag/v18.06.2-ce). La actualización resuelve
[CVE-2019-5736 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configuración de `kubelet` de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha habilitado el control de la característica `ExperimentalCriticalPodAnnotation` para evitar el desalojo de pods estáticos críticos.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.10.12_1543, publicado el 5 de febrero de 2019
{: #11012_1543}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.10.12_1543.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.12_1541">
<caption>Cambios desde la versión 1.10.12_1541</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Consulte las [notas del release de etcd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coreos/etcd/releases/v3.3.11). Además, las suites de cifrado soportadas para etcd están ahora están restringidas a un subconjunto con cifrado de alta resistencia (128 bits o más).</td>
</tr>
<tr>
<td>Plugin de dispositivo GPU e instalador</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Se han actualizado las imágenes para [CVE-2019-3462 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) y
[CVE-2019-6486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>338</td>
<td>342</td>
<td>El plugin de almacenamiento de archivos se ha actualizado de la forma siguiente:
<ul><li>Admite el suministro dinámico con la [planificación de topología de volúmenes](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Pasa por alto los errores de supresión de reclamación de volumen persistente (PVC) si el almacenamiento ya se ha suprimido.</li>
<li>Añade una anotación de mensaje de anomalía a las PVC anómalas.</li>
<li>Optimiza los valores de elección de líder del controlador de suministro de almacenamiento y de periodo de resincronización y aumenta el tiempo de espera de suministro de 30 minutos a 1 hora.</li>
<li>Comprueba los permisos de usuario antes de iniciar el suministro.</li></ul></td>
</tr>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>111</td>
<td>122</td>
<td>Se ha añadido lógica de reintento para evitar anomalías temporales cuando los secretos de Kubernetes están gestionados por {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configuración de la política de auditoría del servidor de API de Kubernetes se ha actualizado para incluir los metadatos de registro para las solicitudes `cluster-admin` y el registro del cuerpo de la solicitud de las solicitudes `create`, `update`
y `patch` de la carga de trabajo.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Se ha actualizado la imagen para [CVE-2018-0734 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) y
[CVE-2018-5407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Además, la configuración de pod ahora se obtiene a partir de un secreto en lugar de obtenerse de un mapa de configuración.</td>
</tr>
<tr>
<td>Servidor OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Se ha actualizado la imagen para [CVE-2018-0734 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) y
[CVE-2018-5407 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Parche de seguridad para [CVE-2018-16864 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.12_1541, publicado el 28 de enero de 2019
{: #11012_1541}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.12_1541.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.12_1540">
<caption>Cambios desde la versión 1.10.12_1540</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones en los paquetes Ubuntu instalados, que incluyen `apt` para [CVE-2019-3462 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) y [USN-3863-1 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.10.12_1540, publicado el 21 de enero de 2019
{: #11012_1540}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.10.12_1540.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.11_1538">
<caption>Cambios desde la versión 1.10.11_1538</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.11-219</td>
<td>v1.10.12-252</td>
<td>Se ha actualizado para dar soporte al release 1.10.12 de Kubernetes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.11</td>
<td>v1.10.12</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.12).</td>
</tr>
<tr>
<td>Redimensionador de complementos de Kubernetes</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Consulte las [notas del release del redimensionador de complementos de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Panel de control de Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>Consulte las [notas del release del panel de control de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). La actualización resuelve
[CVE-2018-18264 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).<br><br>Si accede al panel de control a través de `kubectl proxy`, se elimina el botón **OMITIR** de la página de inicio de sesión. En su lugar, [utilice una **Señal** para iniciar una sesión](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td>Instalador de GPU</td>
<td>390.12</td>
<td>410.79</td>
<td>Se han actualizado los controladores de GPU instalados a 410.79.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.11_1538, publicado el 7 de enero de 2019
{: #11011_1538}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.11_1538.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.11_1537">
<caption>Cambios desde la versión 1.10.11_1537</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2017-5753, CVE-2018-18690 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.11_1537, publicado el 17 de diciembre de 2018
{: #11011_1537}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.11_1537.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.11_1536">
<caption>Cambios desde la versión 1.10.11_1536</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones de los paquetes de Ubuntu instalados.</td>
</tr>
</tbody>
</table>


#### Registro de cambios para 1.10.11_1536, publicado el 4 de diciembre de 2018
{: #11011_1536}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.10.11_1536.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.8_1532">
<caption>Cambios desde la versión 1.10.8_1532</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.3/releases/#v331). La actualización resuelve
[Tigera Technical Advisory TTA-2018-001 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.8-197</td>
<td>v1.10.11-219</td>
<td>Se ha actualizado para dar soporte al release 1.10.11 de Kubernetes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.8</td>
<td>v1.10.11</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.11). La actualización resuelve
[CVE-2018-1002105 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
<tr>
<td>Cliente y servidor de OpenVPN</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Se ha actualizado la imagen para [CVE-2018-0732 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) y
[CVE-2018-0737 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
<tr>
<td>Utilización de recursos del nodo trabajador</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han añadido cgroups dedicados para kubelet y docker para evitar que estos componentes se puedan quedar sin recursos. Para obtener más información, consulte [Reservas de recursos del nodo trabajador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.8_1532, publicado el 27 de noviembre de 2018
{: #1108_1532}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.8_1532.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.8_1531">
<caption>Cambios desde la versión 1.10.8_1531</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>Consulte las [notas del release de Docker ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.docker.com/engine/release-notes/#18061-ce).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.8_1531, publicado el 19 de noviembre de 2018
{: #1108_1531}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.8_1531.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.8_1530">
<caption>Cambios desde la versión 1.10.8_1530</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2018-7755 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.10.8_1530, publicado el 7 de noviembre de 2018
{: #1108_1530_ha-master}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.10.8_1530.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.8_1528">
<caption>Cambios desde la versión 1.10.8_1528</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Maestro de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha actualizado la configuración del maestro del clúster para aumentar la alta disponibilidad (HA). Los clústeres tienen ahora tres réplicas del maestro de Kubernetes que se configuran con una configuración de alta disponibilidad (HA), con cada maestro desplegado en hosts físicos independientes. Además, si el clúster está en una zona con capacidad multizona, los maestros se distribuyen entre las zonas.<br>Para ver las acciones que debe realizar, consulte
[Actualización a maestros de clúster de alta disponibilidad](/docs/containers?topic=containers-cs_versions#ha-masters). Estas acciones preparatorias se aplican:<ul>
<li>Si tiene un cortafuegos o políticas de red Calico personalizadas.</li>
<li>Si utiliza los puertos de host `2040` o `2041` en sus nodos trabajadores.</li>
<li>Si ha utilizado la dirección IP del maestro del clúster para el acceso al maestro dentro del clúster.</li>
<li>Si tiene una automatización que llama a la API o a la CLI de Calico (`calicoctl`), como para crear políticas de Calico.</li>
<li>Si utiliza políticas de red de Kubernetes o Calico para controlar el acceso de salida de pod al maestro.</li></ul></td>
</tr>
<tr>
<td>Proxy de alta disponibilidad de maestro de clúster</td>
<td>N/D</td>
<td>1.8.12-alpine</td>
<td>Se ha añadido un pod `ibm-master-proxy-*` para el equilibrio de carga del lado del cliente en todos los nodos trabajadores, de manera que cada cliente de nodo trabajador pueda direccionar solicitudes a una réplica disponible del maestro de alta disponibilidad.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>Consulte las [notas del release de etcd ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/coreos/etcd/releases/v3.3.1).</td>
</tr>
<tr>
<td>Cifrado de datos en etcd</td>
<td>N/D</td>
<td>N/D</td>
<td>Anteriormente, los datos de etcd se almacenaban en una instancia de almacenamiento de archivos NFS del maestro que se cifraba en reposo. Ahora, los datos de etcd se almacenan en el disco local del maestro y se realiza una copia de seguridad en {{site.data.keyword.cos_full_notm}}. Los datos se cifran durante el tránsito a {{site.data.keyword.cos_full_notm}} y en reposo. No obstante, los datos de etcd en el disco local del maestro no se cifran. Si desea que se cifren los datos de etcd locales del maestro, [habilite {{site.data.keyword.keymanagementservicelong_notm}} en el clúster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.8-172</td>
<td>v1.10.8-197</td>
<td>Se ha añadido la anotación `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` para especificar la VLAN en la que se despliega el servicio de equilibrador de carga. Para ver las VLAN disponibles en el clúster, ejecute `ibmcloud ks vlans --zone <zone>`.</td>
</tr>
<tr>
<td>Kernel habilitado para TPM</td>
<td>N/D</td>
<td>N/D</td>
<td>Los nodos trabajadores nativos con chips TPM para Trusted Compute utilizan el kernel de Ubuntu predeterminado hasta que se habilita la confianza. Si
[habilita la confianza](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) en un clúster existente, necesitará
[volver a cargar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) los nodos trabajadores nativos existentes con chips TPM. Para comprobar si un nodo trabajador local tiene un chip TPM, revise el campo **Trustable** después de ejecutar
el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-types --zone`.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.8_1528, publicado el 26 de octubre de 2018
{: #1108_1528}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.8_1528.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.8_1527">
<caption>Cambios desde la versión 1.10.8_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Gestión de interrupciones del sistema operativo</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha sustituido el daemon del sistema de solicitud de interrupción (IRQ) por un manejador de interrupciones de mayor rendimiento.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para fixpack de nodo maestro 1.10.8_1527, publicado el 15 de octubre de 2018
{: #1108_1527}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo maestro 1.10.8_1527.
{: shortdesc}

<table summary="Resumen de cambios realizados desde la versión 1.10.8_1524">
<caption>Cambios desde la versión 1.10.8_1524</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuración de Calico</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha corregido el analizador de preparación del contenedor `calico-node` para manejar mejor las anomalías de nodo.</td>
</tr>
<tr>
<td>Actualización de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha corregido el problema con la actualización de los complementos de clúster cuando el nodo maestro se actualiza desde una versión no soportada.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.8_1525, publicado el 10 de octubre de 2018
{: #1108_1525}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.8_1525.
{: shortdesc}

<table summary="Resumen de cambios realizados desde la versión 1.10.8_1524">
<caption>Cambios desde la versión 1.10.8_1524</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-14633, CVE-2018-17182 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Tiempo de espera de sesión inactiva</td>
<td>N/D</td>
<td>N/D</td>
<td>Establezca el tiempo de espera de sesión inactiva en 5 minutos por razones de conformidad.</td>
</tr>
</tbody>
</table>


#### Registro de cambios para 1.10.8_1524, publicado el 2 de octubre de 2018
{: #1108_1524}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.10.8_1524.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.7_1520">
<caption>Cambios desde la versión 1.10.7_1520</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proveedor de servicios de gestión de claves</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha añadido la capacidad de utilizar el proveedor Kubernetes de servicio de gestión de claves (KMS) en el clúster, para dar soporte a {{site.data.keyword.keymanagementservicefull}}. Cuando se [habilita {{site.data.keyword.keymanagementserviceshort}} en el clúster](/docs/containers?topic=containers-encryption#keyprotect), todos los secretos de Kubernetes se cifran.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.7</td>
<td>v1.10.8</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.8).</td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>Consulte las [notas del release de Kubernetes DNS Autoscaler ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.7-146</td>
<td>v1.10.8-172</td>
<td>Se ha actualizado para dar soporte al release 1.10.8 de Kubernetes. También se ha actualizado el enlace de la documentación en los mensajes de error del equilibrador de carga.</td>
</tr>
<tr>
<td>Clases de almacenamiento de archivos de IBM</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha eliminado `mountOptions` en las clases de almacenamiento de archivos de IBM para utilizar el valor predeterminado que proporciona el nodo trabajador. Se ha eliminado el parámetro `reclaimPolicy` duplicado de las clases de almacenamiento de archivos de IBM.<br><br>
Además, ahora, cuando actualiza el nodo maestro del clúster, la clase de almacenamiento de archivos predeterminada de IBM no se modifica. Si desea cambiar la clase de almacenamiento predeterminada, ejecute el mandato `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` y sustituya `<storageclass>` por el nombre de la clase de almacenamiento.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.7_1521, publicado el 20 de septiembre de 2018
{: #1107_1521}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.7_1521.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.7_1520">
<caption>Cambios desde la versión 1.10.7_1520</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Rotación de registros</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha modificado para utilizar temporizadores `systemd` en lugar de `cronjobs` para evitar que `logrotate` falle en los nodos trabajadores que no se han vuelto a cargar o no se han actualizado en 90 días. **Nota**: en
todas las versiones anteriores de este release menor, el disco primario se llenaba después de que fallar el trabajo cron porque los registros no se rotaban. El trabajo cron falla después de que el nodo trabajador esté activo durante 90 días sin que se haya actualizado o recargado. Si los registros llenan todo el disco primario, el nodo trabajador entra en un estado de error. El nodo trabajador se puede arreglar mediante el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` o el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>Componentes de tiempo de ejecución de nodo trabajador (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han eliminado las dependencias de los componentes de tiempo de ejecución en el disco primario. Esta mejora impide que los nodos trabajadores fallen cuando se llena el disco primario.</td>
</tr>
<tr>
<td>Caducidad de la contraseña raíz</td>
<td>N/D</td>
<td>N/D</td>
<td>Las contraseñas raíz para los nodos trabajadores caducan después de 90 días por razones de conformidad. Si las herramientas de automatización tienen que iniciar la sesión en el nodo trabajador como usuario root o se basan en trabajos cron que se ejecutan como root, puede inhabilitar la caducidad de la contraseña iniciando una sesión en el nodo trabajador y ejecutando `chage -M -1 root`. **Nota**: si tiene requisitos de conformidad de seguridad que impiden la ejecución como root o la eliminación de la caducidad de la contraseña, no inhabilite la caducidad. En lugar de ello, puede [actualizar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) o [volver a cargar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) los nodos trabajadores al menos cada 90 días.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/D</td>
<td>N/D</td>
<td>Limpie de forma periódica las unidades de montaje transitorias para evitar que se desborden. Esta acción está relacionada con el [problema de Kubernetes 57345 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/D</td>
<td>N/D</td>
<td>Inhabilite el puente de Docker predeterminado para que se utilice el rango de IP `172.17.0.0/16` para las rutas privadas. Si crea los contenedores Docker en los nodos trabajadores ejecutando los mandatos `docker` en el host directamente o utilizando un pod que monta el socket de Docker, elija una de las opciones siguientes.<ul><li>Para garantizar la conectividad de red externa al crear el contenedor, ejecute `docker build . --network host`.</li>
<li>Para crear de forma explícita una red para utilizarla al crear el contenedor, ejecute `docker network create` y luego utilice esta red.</li></ul>
**Nota**: ¿Tiene dependencias en el socket Docker o Docker directamente? [Actualice a `containerd` en lugar de a `docker` como tiempo de ejecución de contenedor](/docs/containers?topic=containers-cs_versions#containerd) para que los clústeres estén preparados para ejecutar Kubernetes versión 1.11 o posteriores.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.10.7_1520, publicado el 4 de septiembre de 2018
{: #1107_1520}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.10.7_1520.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.5_1519">
<caption>Cambios desde la versión 1.10.5_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.2/releases/#v321).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.5-118</td>
<td>v1.10.7-146</td>
<td>Se ha actualizado para dar soporte al release 1.10.7 de Kubernetes. Además, se ha cambiado la configuración del proveedor de nube para manejar mejor las actualizaciones para los servicios de equilibrador de carga con `externalTrafficPolicy` establecido en `local`.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>334</td>
<td>338</td>
<td>Se ha actualizado la versión de incubator a 1.8. El almacenamiento de archivos se suministra a la zona específica que seleccione. No puede actualizar las etiquetas de una instancia de PV existente (estática) a menos que esté utilizando un clúster multizona y tenga que añadir las etiquetas de zona y de región.<br><br> Se ha eliminado la versión de NFS predeterminada de las opciones de montaje de las clases de almacenamiento de archivos proporcionadas por IBM. El sistema operativo del host ahora negocia la versión de NFS con el servidor NFS de la infraestructura de IBM Cloud (SoftLayer). Para establecer manualmente una versión de NFS específica, o para cambiar la versión de NFS de su PV que ha sido negociada por el sistema operativo del host, consulte [Cambio de la versión predeterminada de NFS](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.10.7</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.7).</td>
</tr>
<tr>
<td>Configuración de Kubernetes Heapster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han aumentado los límites de recursos para el contenedor `heapster-nanny`.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.5_1519, publicado el 23 de agosto de 2018
{: #1105_1519}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.5_1519.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.5_1518">
<caption>Cambios desde la versión 1.10.5_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Se ha actualizado `systemd` para arreglar la fuga de `cgroup`.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-3620, CVE-2018-3646 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>


#### Registro de cambios para el fixpack de nodo trabajador 1.10.5_1518, publicado el 13 de agosto de 2018
{: #1105_1518}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.5_1518.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.5_1517">
<caption>Cambios desde la versión 1.10.5_1517</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones de los paquetes de Ubuntu instalados.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.10.5_1517, publicado el 27 de julio de 2018
{: #1105_1517}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.10.5_1517.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.3_1514">
<caption>Cambios desde la versión 1.10.3_1514</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.1</td>
<td>v3.1.3</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/releases/#v313).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.3-85</td>
<td>v1.10.5-118</td>
<td>Se ha actualizado para dar soporte al release 1.10.5 de Kubernetes. Además, ahora los sucesos del servicio de LoadBalancer `create failure` incluyen cualquier error de subred portátil.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>320</td>
<td>334</td>
<td>Se ha aumentado el tiempo de espera para la creación de un volumen persistente de 15 a 30 minutos. Se ha cambiado el tipo de facturación predeterminado a `por hora`. Se han añadido opciones de montaje a las clases de almacenamiento predefinidas. En la instancia de almacenamiento de archivos NFS de la cuenta de infraestructura de IBM Cloud (SoftLayer), se ha cambiado el campo **Notas** por el formato JSON y se ha añadido el espacio de nombres de Kubernetes en el que se ha desplegado el PV. Para dar soporte a clústeres multizona, se han añadido etiquetas de zona y región a los volúmenes persistentes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.3</td>
<td>v1.10.5</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.5).</td>
</tr>
<tr>
<td>Kernel</td>
<td>N/D</td>
<td>N/D</td>
<td>Mejoras menores en los valores de red del nodo trabajador para las cargas de trabajo de red de alto rendimiento.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>El despliegue de cliente de OpenVPN `vpn` que se ejecuta en el espacio de nombres `kube-system` ahora se gestiona mediante `addon-manager` de Kubernetes.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.3_1514, publicado el 3 de julio de 2018
{: #1103_1514}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.3_1514.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.3_1513">
<caption>Cambios desde la versión 1.10.3_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha optimizado `sysctl` para las cargas de trabajo de red de alto rendimiento.</td>
</tr>
</tbody>
</table>


#### Registro de cambios para el fixpack de nodo trabajador 1.10.3_1513, publicado el 21 de junio de 2018
{: #1103_1513}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.3_1513.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.3_1512">
<caption>Cambios desde la versión 1.10.3_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/D</td>
<td>N/D</td>
<td>Para los tipos de máquina no cifrados, el disco secundario se limpia obteniendo un sistema de archivos nuevo cuando se vuelve a cargar o se actualiza el nodo trabajador.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.10.3_1512, publicado el 12 de junio de 2018
{: #1103_1512}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.10.3_1512.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.1_1510">
<caption>Cambios desde la versión 1.10.1_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.10.1</td>
<td>v1.10.3</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.3).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha añadido `PodSecurityPolicy` a la opción `--enable-admission-plugins` para el servidor de API de Kubernetes del clúster y se ha configurado el clúster para que dé soporte a las políticas de seguridad de pod. Para obtener más información, consulte [Configuración de políticas de seguridad de pod](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>Configuración de Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha habilitado la opción `--authentication-token-webhook` para dar soporte a las señales de portadora de API y de cuenta de servicio para la autenticación ante el punto final HTTPS de `kubelet`.</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.1-52</td>
<td>v1.10.3-85</td>
<td>Se ha actualizado para dar soporte al release 1.10.3 de Kubernetes.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha añadido `livenessProbe` al despliegue de cliente de OpenVPN `vpn` que se ejecuta en el espacio de nombres `kube-system`.</td>
</tr>
<tr>
<td>Actualización del kernel</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Nuevas imágenes de nodo trabajador con actualización del kernel para [CVE-2018-3639 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>



#### Registro de cambios para el fixpack de nodo trabajador 1.10.1_1510, publicado el 18 de mayo de 2018
{: #1101_1510}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.1_1510.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.1_1509">
<caption>Cambios desde la versión 1.10.1_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Corregido un error que se producía al utilizar el plugin de almacenamiento en bloques.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.10.1_1509, publicado el 16 de mayo de 2018
{: #1101_1509}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.10.1_1509.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.10.1_1508">
<caption>Cambios desde la versión 1.10.1_1508</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Los datos que se almacenan en el `kubelet` del directorio raíz se guardan ahora en el disco secundario más grande de la máquina del nodo trabajador.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.10.1_1508, publicado el 01 de mayo de 2018
{: #1101_1508}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.10.1_1508.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.7_1510">
<caption>Cambios desde la versión 1.9.7_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v3.1.1</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/releases/#v311).</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.0</td>
<td>v1.5.2</td>
<td>Consulte las [notas del release de Kubernetes Heapster ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.10.1</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Añadido <code>StorageObjectInUseProtection</code> a la opción <code>--enable-admission-plugins</code> para el servidor de API de Kubernetes del clúster.</td>
</tr>
<tr>
<td>DNS de Kubernetes</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>Consulte las [notas del release de DNS de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/dns/releases/tag/1.14.10).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.7-102</td>
<td>v1.10.1-52</td>
<td>Se ha actualizado para dar soporte al release 1.10 de Kubernetes.</td>
</tr>
<tr>
<td>Soporte de GPU</td>
<td>N/D</td>
<td>N/D</td>
<td>Ahora hay disponible soporte para [cargas de trabajo de contenedor con unidad de proceso de gráficos (GPU)](/docs/containers?topic=containers-app#gpu_app) para su planificación y ejecución. Para obtener una lista de los tipos de máquina con GPU que hay disponibles, consulte [Hardware para nodos trabajadores](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes). Para obtener más información, consulte la documentación de Kubernetes de [planificación de GPU ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).</td>
</tr>
</tbody>
</table>

<br />


### Registro de cambios de la versión 1.9 (sin soporte desde el 27 de diciembre de 2018)
{: #19_changelog}

Revise los registros de cambios de la versión 1.9.
{: shortdesc}

*   [Registro de cambios para el fixpack de nodo trabajador 1.9.11_1539, publicado el 17 de diciembre de 2018](#1911_1539)
*   [Registro de cambios para el fixpack de nodo trabajador 1.9.11_1538, publicado el 4 de diciembre de 2018](#1911_1538)
*   [Registro de cambios para el fixpack de nodo trabajador 1.9.11_1537, publicado el 27 de noviembre de 2018](#1911_1537)
*   [Registro de cambios para 1.9.11_1536, publicado el 19 de noviembre de 2018](#1911_1536)
*   [Registro de cambios para el fixpack de nodo trabajador 1.9.10_1532, publicado el 7 de noviembre de 2018](#1910_1532)
*   [Registro de cambios para el fixpack de nodo trabajador 1.9.10_1531, publicado el 26 de octubre de 2018](#1910_1531)
*   [Registro de cambios para fixpack de nodo maestro 1.9.10_1530, publicado el 15 de octubre de 2018](#1910_1530)
*   [Registro de cambios para el fixpack de nodo trabajador 1.9.10_1528, publicado el 10 de octubre de 2018](#1910_1528)
*   [Registro de cambios para 1.9.10_1527, publicado el 2 de octubre de 2018](#1910_1527)
*   [Registro de cambios para el fixpack de nodo trabajador 1.9.10_1524, publicado el 20 de septiembre de 2018](#1910_1524)
*   [Registro de cambios para 1.9.10_1523, publicado el 4 de septiembre de 2018](#1910_1523)
*   [Registro de cambios para el fixpack de nodo trabajador 1.9.9_1522, publicado el 23 de agosto de 2018](#199_1522)
*   [Registro de cambios para el fixpack de nodo trabajador 1.9.9_1521, publicado el 13 de agosto de 2018](#199_1521)
*   [Registro de cambios para 1.9.9_1520, publicado el 27 de julio de 2018](#199_1520)
*   [Registro de cambios para el fixpack de nodo trabajador 1.9.8_1517, publicado el 3 de julio de 2018](#198_1517)
*   [Registro de cambios para el fixpack de nodo trabajador 1.9.8_1516, publicado el 21 de junio de 2018](#198_1516)
*   [Registro de cambios para 1.9.8_1515, publicado el 19 de junio de 2018](#198_1515)
*   [Registro de cambios para el fixpack de nodo trabajador 1.9.7_1513, publicado el 11 de junio de 2018](#197_1513)
*   [Registro de cambios para el fixpack de nodo trabajador 1.9.7_1512, publicado el 18 de mayo de 2018](#197_1512)
*   [Registro de cambios para el fixpack de nodo trabajador 1.9.7_1511, publicado el 16 de mayo de 2018](#197_1511)
*   [Registro de cambios para 1.9.7_1510, publicado el 30 de abril de 2018](#197_1510)

#### Registro de cambios para el fixpack de nodo trabajador 1.9.11_1539, publicado el 17 de diciembre de 2018
{: #1911_1539}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.11_1539.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.11_1538">
<caption>Cambios desde la versión 1.9.11_1538</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones de los paquetes de Ubuntu instalados.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.9.11_1538, publicado el 4 de diciembre de 2018
{: #1911_1538}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.11_1538.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.11_1537">
<caption>Cambios desde la versión 1.9.11_1537</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilización de recursos del nodo trabajador</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han añadido cgroups dedicados para kubelet y docker para evitar que estos componentes se puedan quedar sin recursos. Para obtener más información, consulte [Reservas de recursos del nodo trabajador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.9.11_1537, publicado el 27 de noviembre de 2018
{: #1911_1537}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.11_1537.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.11_1536">
<caption>Cambios desde la versión 1.9.11_1536</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>Consulte las [notas del release de Docker ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.docker.com/engine/release-notes/#18061-ce).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.9.11_1536, publicado el 19 de noviembre de 2018
{: #1911_1536}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.9.11_1536.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.10_1532">
<caption>Cambios desde la versión 1.9.10_1532</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v2.6.12</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v2.6/releases/#v2612). La actualización resuelve
[Tigera Technical Advisory TTA-2018-001 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Se han actualizado las imágenes de nodo trabajador con actualización del kernel para [CVE-2018-7755 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.10</td>
<td>v1.9.11</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.11).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.10-219</td>
<td>v1.9.11-249</td>
<td>Se ha actualizado para dar soporte al release 1.9.11 de Kubernetes.</td>
</tr>
<tr>
<td>Cliente y servidor de OpenVPN</td>
<td>2.4.4-r2</td>
<td>2.4.6-r3-IKS-8</td>
<td>Se ha actualizado la imagen para [CVE-2018-0732 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) y
[CVE-2018-0737 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.9.10_1532, publicado el 7 de noviembre de 2018
{: #1910_1532}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.11_1532.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.10_1531">
<caption>Cambios desde la versión 1.9.10_1531</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel habilitado para TPM</td>
<td>N/D</td>
<td>N/D</td>
<td>Los nodos trabajadores nativos con chips TPM para Trusted Compute utilizan el kernel de Ubuntu predeterminado hasta que se habilita la confianza. Si
[habilita la confianza](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) en un clúster existente, necesitará
[volver a cargar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) los nodos trabajadores nativos existentes con chips TPM. Para comprobar si un nodo trabajador local tiene un chip TPM, revise el campo **Trustable** después de ejecutar
el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-types --zone`.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.9.10_1531, publicado el 26 de octubre de 2018
{: #1910_1531}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.10_1531.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.10_1530">
<caption>Cambios desde la versión 1.9.10_1530</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Gestión de interrupciones del sistema operativo</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha sustituido el daemon del sistema de solicitud de interrupción (IRQ) por un manejador de interrupciones de mayor rendimiento.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para fixpack de nodo maestro 1.9.10_1530, publicado el 15 de octubre de 2018
{: #1910_1530}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.10_1530.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.10_1527">
<caption>Cambios desde la versión 1.9.10_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Actualización de clúster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha corregido el problema con la actualización de los complementos de clúster cuando el nodo maestro se actualiza desde una versión no soportada.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.9.10_1528, publicado el 10 de octubre de 2018
{: #1910_1528}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.10_1528.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.10_1527">
<caption>Cambios desde la versión 1.9.10_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-14633, CVE-2018-17182 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Tiempo de espera de sesión inactiva</td>
<td>N/D</td>
<td>N/D</td>
<td>Establezca el tiempo de espera de sesión inactiva en 5 minutos por razones de conformidad.</td>
</tr>
</tbody>
</table>


#### Registro de cambios para 1.9.10_1527, publicado el 2 de octubre de 2018
{: #1910_1527}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.9.10_1527.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.10_1523">
<caption>Cambios desde la versión 1.9.10_1523</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.10-192</td>
<td>v1.9.10-219</td>
<td>Se ha actualizado el enlace de la documentación en los mensajes de error del equilibrador de carga.</td>
</tr>
<tr>
<td>Clases de almacenamiento de archivos de IBM</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha eliminado `mountOptions` en las clases de almacenamiento de archivos de IBM para utilizar el valor predeterminado que proporciona el nodo trabajador. Se ha eliminado el parámetro `reclaimPolicy` duplicado de las clases de almacenamiento de archivos de IBM.<br><br>
Además, ahora, cuando actualiza el nodo maestro del clúster, la clase de almacenamiento de archivos predeterminada de IBM no se modifica. Si desea cambiar la clase de almacenamiento predeterminada, ejecute el mandato `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` y sustituya `<storageclass>` por el nombre de la clase de almacenamiento.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.9.10_1524, publicado el 20 de septiembre de 2018
{: #1910_1524}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.10_1524.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.10_1523">
<caption>Cambios desde la versión 1.9.10_1523</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Rotación de registros</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha modificado para utilizar temporizadores `systemd` en lugar de `cronjobs` para evitar que `logrotate` falle en los nodos trabajadores que no se han vuelto a cargar o no se han actualizado en 90 días. **Nota**: en
todas las versiones anteriores de este release menor, el disco primario se llenaba después de que fallar el trabajo cron porque los registros no se rotaban. El trabajo cron falla después de que el nodo trabajador esté activo durante 90 días sin que se haya actualizado o recargado. Si los registros llenan todo el disco primario, el nodo trabajador entra en un estado de error. El nodo trabajador se puede arreglar mediante el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` o el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>Componentes de tiempo de ejecución de nodo trabajador (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han eliminado las dependencias de los componentes de tiempo de ejecución en el disco primario. Esta mejora impide que los nodos trabajadores fallen cuando se llena el disco primario.</td>
</tr>
<tr>
<td>Caducidad de la contraseña raíz</td>
<td>N/D</td>
<td>N/D</td>
<td>Las contraseñas raíz para los nodos trabajadores caducan después de 90 días por razones de conformidad. Si las herramientas de automatización tienen que iniciar la sesión en el nodo trabajador como usuario root o se basan en trabajos cron que se ejecutan como root, puede inhabilitar la caducidad de la contraseña iniciando una sesión en el nodo trabajador y ejecutando `chage -M -1 root`. **Nota**: si tiene requisitos de conformidad de seguridad que impiden la ejecución como root o la eliminación de la caducidad de la contraseña, no inhabilite la caducidad. En lugar de ello, puede [actualizar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) o [volver a cargar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) los nodos trabajadores al menos cada 90 días.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/D</td>
<td>N/D</td>
<td>Limpie de forma periódica las unidades de montaje transitorias para evitar que se desborden. Esta acción está relacionada con el [problema de Kubernetes 57345 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/D</td>
<td>N/D</td>
<td>Inhabilite el puente de Docker predeterminado para que se utilice el rango de IP `172.17.0.0/16` para las rutas privadas. Si crea los contenedores Docker en los nodos trabajadores ejecutando los mandatos `docker` en el host directamente o utilizando un pod que monta el socket de Docker, elija una de las opciones siguientes.<ul><li>Para garantizar la conectividad de red externa al crear el contenedor, ejecute `docker build . --network host`.</li>
<li>Para crear de forma explícita una red para utilizarla al crear el contenedor, ejecute `docker network create` y luego utilice esta red.</li></ul>
**Nota**: ¿Tiene dependencias en el socket Docker o Docker directamente? [Actualice a `containerd` en lugar de a `docker` como tiempo de ejecución de contenedor](/docs/containers?topic=containers-cs_versions#containerd) para que los clústeres estén preparados para ejecutar Kubernetes versión 1.11 o posteriores.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.9.10_1523, publicado el 4 de septiembre de 2018
{: #1910_1523}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.9.10_1523.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.9_1522">
<caption>Cambios desde la versión 1.9.9_1522</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.9-167</td>
<td>v1.9.10-192</td>
<td>Se ha actualizado para dar soporte al release 1.9.10 de Kubernetes. Además, se ha cambiado la configuración del proveedor de nube para manejar mejor las actualizaciones para los servicios de equilibrador de carga con `externalTrafficPolicy` establecido en `local`.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>334</td>
<td>338</td>
<td>Se ha actualizado la versión de incubator a 1.8. El almacenamiento de archivos se suministra a la zona específica que seleccione. No puede actualizar las etiquetas de una instancia de PV existente (estática) a menos que esté utilizando un clúster multizona y tenga que añadir las etiquetas de zona y de región.<br><br>Se ha eliminado la versión de NFS predeterminada de las opciones de montaje de las clases de almacenamiento de archivos proporcionadas por IBM. El sistema operativo del host ahora negocia la versión de NFS con el servidor NFS de la infraestructura de IBM Cloud (SoftLayer). Para establecer manualmente una versión de NFS específica, o para cambiar la versión de NFS de su PV que ha sido negociada por el sistema operativo del host, consulte [Cambio de la versión predeterminada de NFS](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.9</td>
<td>v1.9.10</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.10).</td>
</tr>
<tr>
<td>Configuración de Kubernetes Heapster</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han aumentado los límites de recursos para el contenedor `heapster-nanny`.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.9.9_1522, publicado el 23 de agosto de 2018
{: #199_1522}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.9_1522.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.9_1521">
<caption>Cambios desde la versión 1.9.9_1521</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Se ha actualizado `systemd` para arreglar la fuga de `cgroup`.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-3620, CVE-2018-3646 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>


#### Registro de cambios para el fixpack de nodo trabajador 1.9.9_1521, publicado el 13 de agosto de 2018
{: #199_1521}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.9_1521.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.9_1520">
<caption>Cambios desde la versión 1.9.9_1520</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones de los paquetes de Ubuntu instalados.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.9.9_1520, publicado el 27 de julio de 2018
{: #199_1520}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.9.9_1520.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.8_1517">
<caption>Cambios desde la versión 1.9.8_1517</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.8-141</td>
<td>v1.9.9-167</td>
<td>Se ha actualizado para dar soporte al release 1.9.9 de Kubernetes. Además, ahora los sucesos del servicio de LoadBalancer `create failure` incluyen cualquier error de subred portátil.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>320</td>
<td>334</td>
<td>Se ha aumentado el tiempo de espera para la creación de un volumen persistente de 15 a 30 minutos. Se ha cambiado el tipo de facturación predeterminado a `por hora`. Se han añadido opciones de montaje a las clases de almacenamiento predefinidas. En la instancia de almacenamiento de archivos NFS de la cuenta de infraestructura de IBM Cloud (SoftLayer), se ha cambiado el campo **Notas** por el formato JSON y se ha añadido el espacio de nombres de Kubernetes en el que se ha desplegado el PV. Para dar soporte a clústeres multizona, se han añadido etiquetas de zona y región a los volúmenes persistentes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.8</td>
<td>v1.9.9</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.9).</td>
</tr>
<tr>
<td>Kernel</td>
<td>N/D</td>
<td>N/D</td>
<td>Mejoras menores en los valores de red del nodo trabajador para las cargas de trabajo de red de alto rendimiento.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>El despliegue de cliente de OpenVPN `vpn` que se ejecuta en el espacio de nombres `kube-system` ahora se gestiona mediante `addon-manager` de Kubernetes.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.9.8_1517, publicado el 3 de julio de 2018
{: #198_1517}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.8_1517.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.8_1516">
<caption>Cambios desde la versión 1.9.8_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha optimizado `sysctl` para las cargas de trabajo de red de alto rendimiento.</td>
</tr>
</tbody>
</table>


#### Registro de cambios para el fixpack de nodo trabajador 1.9.8_1516, publicado el 21 de junio de 2018
{: #198_1516}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.8_1516.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.8_1515">
<caption>Cambios desde la versión 1.9.8_1515</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/D</td>
<td>N/D</td>
<td>Para los tipos de máquina no cifrados, el disco secundario se limpia obteniendo un sistema de archivos nuevo cuando se vuelve a cargar o se actualiza el nodo trabajador.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.9.8_1515, publicado el 19 de junio de 2018
{: #198_1515}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.9.8_1515.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.7_1513">
<caption>Cambios desde la versión 1.9.7_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.9.8</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.8).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha añadido `PodSecurityPolicy` a la opción `--admission-control` para el servidor de API de Kubernetes del clúster y se ha configurado el clúster para que dé soporte a las políticas de seguridad de pod. Para obtener más información, consulte [Configuración de políticas de seguridad de pod](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>Proveedor de IBM Cloud</td>
<td>v1.9.7-102</td>
<td>v1.9.8-141</td>
<td>Se ha actualizado para dar soporte al release 1.9.8 de Kubernetes.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha añadido <code>livenessProbe</code> al despliegue de cliente de OpenVPN <code>vpn</code> que se ejecuta en el espacio de nombres <code>kube-system</code>.</td>
</tr>
</tbody>
</table>


#### Registro de cambios para el fixpack de nodo trabajador 1.9.7_1513, publicado el 11 de junio de 2018
{: #197_1513}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.7_1513.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.7_1512">
<caption>Cambios desde la versión 1.9.7_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Actualización del kernel</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Nuevas imágenes de nodo trabajador con actualización del kernel para [CVE-2018-3639 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.9.7_1512, publicado el 18 de mayo de 2018
{: #197_1512}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.7_1512.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.7_1511">
<caption>Cambios desde la versión 1.9.7_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Corregido un error que se producía al utilizar el plugin de almacenamiento en bloques.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.9.7_1511, publicado el 16 de mayo de 2018
{: #197_1511}

En la siguiente tabla se muestran los cambios incluidos en el fixpack de nodo trabajador 1.9.7_1511.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.7_1510">
<caption>Cambios desde la versión 1.9.7_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Los datos que se almacenan en el `kubelet` del directorio raíz se guardan ahora en el disco secundario más grande de la máquina del nodo trabajador.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.9.7_1510, publicado el 30 de abril de 2018
{: #197_1510}

En la siguiente tabla se muestran los cambios incluidos en el parche 1.9.7_1510.
{: shortdesc}

<table summary="Cambios realizados desde la versión 1.9.3_1506">
<caption>Cambios desde la versión 1.9.3_1506</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.3</td>
<td>v1.9.7	</td>
<td><p>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7). Este release corrige las vulnerabilidades [CVE-2017-1002101 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) y [CVE-2017-1002102 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p><strong>Nota</strong>: ahora se montan como de solo lectura los volúmenes proyectados, `secret`, `configMap` y `downwardAPI`. Anteriormente, las apps podían escribir datos en estos volúmenes, pero el sistema podía revertir automáticamente los datos. Si sus apps se basaban en este comportamiento inseguro anterior, modifíquelas en consecuencia.</p></td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Añadido `admissionregistration.k8s.io/v1alpha1=true` a la opción `--runtime-config` para el servidor de API de Kubernetes del clúster.</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.3-71</td>
<td>v1.9.7-102</td>
<td>Ahora los servicios `NodePort` y `LoadBalancer` dan soporte a la [conservación de la dirección IP de origen del cliente](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) estableciendo `service.spec.externalTrafficPolicy` en `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corregida la configuración de tolerancia del [nodo extremo](/docs/containers?topic=containers-edge#edge) en clústeres antiguos.</td>
</tr>
</tbody>
</table>

<br />


### Registro de cambios de la versión 1.8 (no soportada)
{: #18_changelog}

Revise los registros de cambios de la versión 1.8.
{: shortdesc}

*   [Registro de cambios para el fixpack de nodo trabajador 1.8.15_1521, publicado el 20 de septiembre de 2018](#1815_1521)
*   [Registro de cambios para el fixpack de nodo trabajador 1.8.15_1520, publicado el 23 de agosto de 2018](#1815_1520)
*   [Registro de cambios para el fixpack de nodo trabajador 1.8.15_1519, publicado el 13 de agosto de 2018](#1815_1519)
*   [Registro de cambios para 1.8.15_1518, publicado el 27 de julio de 2018](#1815_1518)
*   [Registro de cambios para el fixpack de nodo trabajador 1.8.13_1516, publicado el 3 de julio de 2018](#1813_1516)
*   [Registro de cambios para el fixpack de nodo trabajador 1.8.13_1515, publicado el 21 de junio de 2018](#1813_1515)
*   [Registro de cambios para 1.8.13_1514, publicado el 19 de junio de 2018](#1813_1514)
*   [Registro de cambios para el fixpack de nodo trabajador 1.8.11_1512, publicado el 11 de junio de 2018](#1811_1512)
*   [Registro de cambios para el fixpack de nodo trabajador 1.8.11_1511, publicado el 18 de mayo de 2018](#1811_1511)
*   [Registro de cambios para el fixpack de nodo trabajador 1.8.11_1510, publicado el 16 de mayo de 2018](#1811_1510)
*   [Registro de cambios para 1.8.11_1509, publicado el 19 de abril de 2018](#1811_1509)

#### Registro de cambios para el fixpack de nodo trabajador 1.8.15_1521, publicado el 20 de septiembre de 2018
{: #1815_1521}

<table summary="Cambios realizados desde la versión 1.8.15_1520">
<caption>Cambios desde la versión 1.8.15_1520</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Rotación de registros</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha modificado para utilizar temporizadores `systemd` en lugar de `cronjobs` para evitar que `logrotate` falle en los nodos trabajadores que no se han vuelto a cargar o no se han actualizado en 90 días. **Nota**: en
todas las versiones anteriores de este release menor, el disco primario se llenaba después de que fallar el trabajo cron porque los registros no se rotaban. El trabajo cron falla después de que el nodo trabajador esté activo durante 90 días sin que se haya actualizado o recargado. Si los registros llenan todo el disco primario, el nodo trabajador entra en un estado de error. El nodo trabajador se puede arreglar mediante el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` o el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>Componentes de tiempo de ejecución de nodo trabajador (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/D</td>
<td>N/D</td>
<td>Se han eliminado las dependencias de los componentes de tiempo de ejecución en el disco primario. Esta mejora impide que los nodos trabajadores fallen cuando se llena el disco primario.</td>
</tr>
<tr>
<td>Caducidad de la contraseña raíz</td>
<td>N/D</td>
<td>N/D</td>
<td>Las contraseñas raíz para los nodos trabajadores caducan después de 90 días por razones de conformidad. Si las herramientas de automatización tienen que iniciar la sesión en el nodo trabajador como usuario root o se basan en trabajos cron que se ejecutan como root, puede inhabilitar la caducidad de la contraseña iniciando una sesión en el nodo trabajador y ejecutando `chage -M -1 root`. **Nota**: si tiene requisitos de conformidad de seguridad que impiden la ejecución como root o la eliminación de la caducidad de la contraseña, no inhabilite la caducidad. En lugar de ello, puede [actualizar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) o [volver a cargar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) los nodos trabajadores al menos cada 90 días.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/D</td>
<td>N/D</td>
<td>Limpie de forma periódica las unidades de montaje transitorias para evitar que se desborden. Esta acción está relacionada con el [problema de Kubernetes 57345 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.8.15_1520, publicado el 23 de agosto de 2018
{: #1815_1520}

<table summary="Cambios realizados desde la versión 1.8.15_1519">
<caption>Cambios desde la versión 1.8.15_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Se ha actualizado `systemd` para arreglar la fuga de `cgroup`.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Se han actualizado las imágenes de nodo trabajador con la actualización de kernel para [CVE-2018-3620, CVE-2018-3646 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.8.15_1519, publicado el 13 de agosto de 2018
{: #1815_1519}

<table summary="Cambios realizados desde la versión 1.8.15_1518">
<caption>Cambios desde la versión 1.8.15_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paquetes Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Actualizaciones de los paquetes de Ubuntu instalados.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.8.15_1518, publicado el 27 de julio de 2018
{: #1815_1518}

<table summary="Cambios realizados desde la versión 1.8.13_1516">
<caption>Cambios desde la versión 1.8.13_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.8.13-176</td>
<td>v1.8.15-204</td>
<td>Se ha actualizado para dar soporte al release 1.8.15 de Kubernetes. Además, ahora los sucesos del servicio de LoadBalancer `create failure` incluyen cualquier error de subred portátil.</td>
</tr>
<tr>
<td>Plugin de almacenamiento de archivos de {{site.data.keyword.Bluemix_notm}}</td>
<td>320</td>
<td>334</td>
<td>Se ha aumentado el tiempo de espera para la creación de un volumen persistente de 15 a 30 minutos. Se ha cambiado el tipo de facturación predeterminado a `por hora`. Se han añadido opciones de montaje a las clases de almacenamiento predefinidas. En la instancia de almacenamiento de archivos NFS de la cuenta de infraestructura de IBM Cloud (SoftLayer), se ha cambiado el campo **Notas** por el formato JSON y se ha añadido el espacio de nombres de Kubernetes en el que se ha desplegado el PV. Para dar soporte a clústeres multizona, se han añadido etiquetas de zona y región a los volúmenes persistentes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.8.13</td>
<td>v1.8.15</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.15).</td>
</tr>
<tr>
<td>Kernel</td>
<td>N/D</td>
<td>N/D</td>
<td>Mejoras menores en los valores de red del nodo trabajador para las cargas de trabajo de red de alto rendimiento.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>El despliegue de cliente de OpenVPN `vpn` que se ejecuta en el espacio de nombres `kube-system` ahora se gestiona mediante `addon-manager` de Kubernetes.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.8.13_1516, publicado el 3 de julio de 2018
{: #1813_1516}

<table summary="Cambios realizados desde la versión 1.8.13_1515">
<caption>Cambios desde la versión 1.8.13_1515</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha optimizado `sysctl` para las cargas de trabajo de red de alto rendimiento.</td>
</tr>
</tbody>
</table>


#### Registro de cambios para el fixpack de nodo trabajador 1.8.13_1515, publicado el 21 de junio de 2018
{: #1813_1515}

<table summary="Cambios realizados desde la versión 1.8.13_1514">
<caption>Cambios desde la versión 1.8.13_1514</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/D</td>
<td>N/D</td>
<td>Para los tipos de máquina no cifrados, el disco secundario se limpia obteniendo un sistema de archivos nuevo cuando se vuelve a cargar o se actualiza el nodo trabajador.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.8.13_1514, publicado el 19 de junio de 2018
{: #1813_1514}

<table summary="Cambios realizados desde la versión 1.8.11_1512">
<caption>Cambios desde la versión 1.8.11_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.11</td>
<td>v1.8.13</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.13).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha añadido `PodSecurityPolicy` a la opción `--admission-control` para el servidor de API de Kubernetes del clúster y se ha configurado el clúster para que dé soporte a las políticas de seguridad de pod. Para obtener más información, consulte [Configuración de políticas de seguridad de pod](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>Proveedor de IBM Cloud</td>
<td>v1.8.11-126</td>
<td>v1.8.13-176</td>
<td>Se ha actualizado para dar soporte al release 1.8.13 de Kubernetes.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>Se ha añadido <code>livenessProbe</code> al despliegue de cliente de OpenVPN <code>vpn</code> que se ejecuta en el espacio de nombres <code>kube-system</code>.</td>
</tr>
</tbody>
</table>


#### Registro de cambios para el fixpack de nodo trabajador 1.8.11_1512, publicado el 11 de junio de 2018
{: #1811_1512}

<table summary="Cambios realizados desde la versión 1.8.11_1511">
<caption>Cambios desde la versión 1.8.11_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Actualización del kernel</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Nuevas imágenes de nodo trabajador con actualización del kernel para [CVE-2018-3639 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>


#### Registro de cambios para el fixpack de nodo trabajador 1.8.11_1511, publicado el 18 de mayo de 2018
{: #1811_1511}

<table summary="Cambios realizados desde la versión 1.8.11_1510">
<caption>Cambios desde la versión 1.8.11_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Corregido un error que se producía al utilizar el plugin de almacenamiento en bloques.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.8.11_1510, publicado el 16 de mayo de 2018
{: #1811_1510}

<table summary="Cambios realizados desde la versión 1.8.11_1509">
<caption>Cambios desde la versión 1.8.11_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Los datos que se almacenan en el `kubelet` del directorio raíz se guardan ahora en el disco secundario más grande de la máquina del nodo trabajador.</td>
</tr>
</tbody>
</table>


#### Registro de cambios para 1.8.11_1509, publicado el 19 de abril de 2018
{: #1811_1509}

<table summary="Cambios realizados desde la versión 1.8.8_1507">
<caption>Cambios desde la versión 1.8.8_1507</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td><p>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11). Este release corrige las vulnerabilidades [CVE-2017-1002101 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) y [CVE-2017-1002102 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p>Ahora se montan como de solo lectura los volúmenes proyectados, `secret`, `configMap` y `downwardAPI`. Anteriormente, las apps podían escribir datos en estos volúmenes, pero el sistema podía revertir automáticamente los datos. Si sus apps se basaban en este comportamiento inseguro anterior, modifíquelas en consecuencia.</p></td>
</tr>
<tr>
<td>Imagen de contenedor en pausa</td>
<td>3.0</td>
<td>3.1</td>
<td>Elimina procesos zombie huérfanos heredados.</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>Ahora los servicios `NodePort` y `LoadBalancer` dan soporte a la [conservación de la dirección IP de origen del cliente](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) estableciendo `service.spec.externalTrafficPolicy` en `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corregida la configuración de tolerancia del [nodo extremo](/docs/containers?topic=containers-edge#edge) en clústeres antiguos.</td>
</tr>
</tbody>
</table>

<br />


### Registro de cambios de la versión 1.7 (no soportada)
{: #17_changelog}

Revise los registros de cambios de la versión 1.7.
{: shortdesc}

*   [Registro de cambios para el fixpack de nodo trabajador 1.7.16_1514, publicado el 11 de junio de 2018](#1716_1514)
*   [Registro de cambios para el fixpack de nodo trabajador 1.7.16_1513, publicado el 18 de mayo de 2018](#1716_1513)
*   [Registro de cambios para el fixpack de nodo trabajador 1.7.16_1512, publicado el 16 de mayo de 2018](#1716_1512)
*   [Registro de cambios para 1.7.16_1511, publicado el 19 de abril de 2018](#1716_1511)

#### Registro de cambios para el fixpack de nodo trabajador 1.7.16_1514, publicado el 11 de junio de 2018
{: #1716_1514}

<table summary="Cambios realizados desde la versión 1.7.16_1513">
<caption>Cambios desde la versión 1.7.16_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Actualización del kernel</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Nuevas imágenes de nodo trabajador con actualización del kernel para [CVE-2018-3639 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.7.16_1513, publicado el 18 de mayo de 2018
{: #1716_1513}

<table summary="Cambios realizados desde la versión 1.7.16_1512">
<caption>Cambios desde la versión 1.7.16_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Corregido un error que se producía al utilizar el plugin de almacenamiento en bloques.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fixpack de nodo trabajador 1.7.16_1512, publicado el 16 de mayo de 2018
{: #1716_1512}

<table summary="Cambios realizados desde la versión 1.7.16_1511">
<caption>Cambios desde la versión 1.7.16_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Los datos que se almacenan en el `kubelet` del directorio raíz se guardan ahora en el disco secundario más grande de la máquina del nodo trabajador.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.7.16_1511, publicado el 19 de abril de 2018
{: #1716_1511}

<table summary="Cambios realizados desde la versión 1.7.4_1509">
<caption>Cambios desde la versión 1.7.4_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td><p>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16). Este release corrige las vulnerabilidades [CVE-2017-1002101 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) y [CVE-2017-1002102 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p>Ahora se montan como de solo lectura los volúmenes proyectados, `secret`, `configMap` y `downwardAPI`. Anteriormente, las apps podían escribir datos en estos volúmenes, pero el sistema podía revertir automáticamente los datos. Si sus apps se basaban en este comportamiento inseguro anterior, modifíquelas en consecuencia.</p></td>
</tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>Ahora los servicios `NodePort` y `LoadBalancer` dan soporte a la [conservación de la dirección IP de origen del cliente](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) estableciendo `service.spec.externalTrafficPolicy` en `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corregida la configuración de tolerancia del [nodo extremo](/docs/containers?topic=containers-edge#edge) en clústeres antiguos.</td>
</tr>
</tbody>
</table>
