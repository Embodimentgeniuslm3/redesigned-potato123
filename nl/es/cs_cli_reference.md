---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-18"

keywords: kubernetes, iks, ibmcloud, ic, ks

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


# CLI del servicio Kubernetes de IBM Cloud
{: #cs_cli_reference}

Consulte estos mandatos para crear y gestionar clústeres de Kubernetes en {{site.data.keyword.containerlong}}.
{:shortdesc}

Para instalar el plugin de CLI, consulte [Instalación de la CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).

En el terminal, se le notifica cuando están disponibles las actualizaciones de la CLI y los plugins de `ibmcloud`. Asegúrese de mantener actualizada la CLI para poder utilizar todos los mandatos y distintivos disponibles.

¿Está buscando mandatos `ibmcloud cr`? Revise la [Consulta de CLI de {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-registry_cli_reference#registry_cli_reference). ¿Está buscando mandatos `kubectl`? Consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubectl.docs.kubernetes.io/).
{:tip}


## Utilización del plugin beta de {{site.data.keyword.containerlong_notm}}
{: #cs_beta}

Está disponible una versión rediseñada del plugin de {{site.data.keyword.containerlong_notm}} en versión beta. El plugin rediseñado de {{site.data.keyword.containerlong_notm}} agrupa los mandatos en categorías y cambia los mandatos, que pasan de una estructura con guiones a una con espacios.
{: shortdesc}

Para utilizar el plugin rediseñado de {{site.data.keyword.containerlong_notm}}, establezca la variable de entorno `IKS_BETA_VERSION`
en la versión beta que desea utilizar:

```
export IKS_BETA_VERSION=<beta_version>
```
{: pre}

Están disponibles las siguientes versiones beta del plugin rediseñado de {{site.data.keyword.containerlong_notm}}. Tenga en cuenta que el comportamiento predeterminado es `0.2`.

<table>
<caption>Versiones beta del plugin rediseñado de {{site.data.keyword.containerlong_notm}}</caption>
  <thead>
    <th>Versión beta</th>
    <th>Estructura de salida de `ibmcloud ks help`</th>
    <th>Estructura del mandato</th>
  </thead>
  <tbody>
    <tr>
      <td><code>0.2</code> (valor predeterminado)</td>
      <td>Antigua: Los mandatos se muestran en la estructura de guiones y se listan alfabéticamente.</td>
      <td>Antigua y beta: Puede ejecutar mandatos con la estructura antigua de guiones (`ibmcloud ks alb-cert-get`) o en la estructura beta de espacios (`ibmcloud ks alb cert get`).</td>
    </tr>
    <tr>
      <td><code>0.3</code></td>
      <td>Beta: Los mandatos se muestran en la estructura de espacios y se listan por categorías.</td>
      <td>Antigua y beta: Puede ejecutar mandatos con la estructura antigua de guiones (`ibmcloud ks alb-cert-get`) o en la estructura beta de espacios (`ibmcloud ks alb cert get`).</td>
    </tr>
    <tr>
      <td><code>1.0</code></td>
      <td>Beta: Los mandatos se muestran en la estructura de espacios y se listan por categorías.</td>
      <td>Beta: Solo puede ejecutar mandatos en la estructura beta de espacios (`ibmcloud ks alb cert get`).</td>
    </tr>
  </tbody>
</table>



## Mandatos ibmcloud ks
{: #cs_commands}

**Sugerencia:** Para ver la versión del plugin de {{site.data.keyword.containerlong_notm}}, consulte el siguiente mandato.

```
ibmcloud plugin list
```
{: pre}

<table summary="Tabla de mandatos de API">
<caption>Mandatos de API</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de API</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks api](#cs_cli_api)</td>
    <td>[ibmcloud ks api-key-info](#cs_api_key_info)</td>
    <td>[ibmcloud ks api-key-reset](#cs_api_key_reset)</td>
    <td>[ibmcloud ks apiserver-config-get](#cs_apiserver_config_get)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks apiserver-config-set](#cs_apiserver_config_set)</td>
    <td>[ibmcloud ks apiserver-config-unset](#cs_apiserver_config_unset)</td>
    <td>[ibmcloud ks apiserver-refresh](#cs_apiserver_refresh)</td>
    <td></td>
 </tr>
</tbody>
</table>

<br>

<table summary="Tabla de mandatos de uso del plugin de CLI">
<caption>Mandatos de uso del plugin de CLI</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de uso del plugin de CLI</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks help](#cs_help)</td>
    <td>[ibmcloud ks init](#cs_init)</td>
    <td>[ibmcloud ks messages](#cs_messages)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="Tabla Mandatos de clúster: Gestión">
<caption>Mandatos de clúster: Mandatos de gestión</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de clúster: Gestión</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks addon-versions](#cs_addon_versions)</td>
    <td>[ibmcloud ks cluster-addon-disable](#cs_cluster_addon_disable)</td>
    <td>[ibmcloud ks cluster-addon-enable](#cs_cluster_addon_enable)</td>
    <td>[ibmcloud ks cluster-addons](#cs_cluster_addons)</td>
  </tr>
  <tr>
  <td>[ibmcloud ks cluster-config](#cs_cluster_config)</td>
    <td>[ibmcloud ks cluster-create](#cs_cluster_create)</td>
    <td>[ibmcloud ks cluster-feature-disable](#cs_cluster_feature_disable)</td>
    <td>[ibmcloud ks cluster-feature-enable](#cs_cluster_feature_enable)</td>
  </tr>
  <tr>
  <td>[ibmcloud ks cluster-get](#cs_cluster_get)</td>
    <td>[ibmcloud ks cluster-pull-secret-apply](#cs_cluster_pull_secret_apply)</td>
    <td>[ibmcloud ks cluster-refresh](#cs_cluster_refresh)</td>
    <td>[ibmcloud ks cluster-rm](#cs_cluster_rm)</td>
  </tr>
  <tr>
  <td>[ibmcloud ks cluster-update](#cs_cluster_update)</td>
    <td>[ibmcloud ks clusters](#cs_clusters)</td>
    <td>[ibmcloud ks kube-versions](#cs_kube_versions)</td>
    <td> </td>
  </tr>
</tbody>
</table>

<br>

<table summary="Tabla Mandatos de clúster: Servicios e integraciones">
<caption>Mandatos de clúster: Mandatos de servicios e integraciones</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de clúster: Servicios e integraciones</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[ibmcloud ks cluster-service-unbind](#cs_cluster_service_unbind)</td>
    <td>[ibmcloud ks cluster-services](#cs_cluster_services)</td>
    <td>[ibmcloud ks va](#cs_va)</td>
  </tr>
    <td>[ibmcloud ks webhook-create](#cs_webhook_create)</td>
  <tr>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabla Mandatos de clúster: Subredes">
<caption>Mandatos de clúster: Mandatos de subredes</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de clúster: Subredes</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[ibmcloud ks cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[ibmcloud ks cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[ibmcloud ks cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks subnets](#cs_subnets)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabla Mandatos de infraestructura">
<caption>Mandatos de clúster: Mandatos de infraestructura</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de infraestructura</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks credential-get](#cs_credential_get)</td>
    <td>[ibmcloud ks credential-set](#cs_credentials_set)</td>
    <td>[ibmcloud ks credential-unset](#cs_credentials_unset)</td>
    <td>[ibmcloud ks machine-types](#cs_machine_types)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks vlans](#cs_vlans)</td>
    <td>[ibmcloud ks vlan-spanning-get](#cs_vlan_spanning_get)</td>
    <td> </td>
    <td> </td>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabla Mandatos del equilibrador de carga de aplicación (ALB) de Ingress">
<caption>Mandatos del equilibrador de carga de aplicación (ALB) de Ingress</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Mandatos del equilibrador de carga de aplicación (ALB) de Ingress</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[ibmcloud ks alb-autoupdate-disable](#cs_alb_autoupdate_disable)</td>
      <td>[ibmcloud ks alb-autoupdate-enable](#cs_alb_autoupdate_enable)</td>
      <td>[ibmcloud ks alb-autoupdate-get](#cs_alb_autoupdate_get)</td>
      <td>[ibmcloud ks alb-cert-deploy](#cs_alb_cert_deploy)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks alb-cert-get](#cs_alb_cert_get)</td>
      <td>[ibmcloud ks alb-cert-rm](#cs_alb_cert_rm)</td>
      <td>[ibmcloud ks alb-certs](#cs_alb_certs)</td>
      <td>[ibmcloud ks alb-configure](#cs_alb_configure)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks alb-get](#cs_alb_get)</td>
      <td>[ibmcloud ks alb-rollback](#cs_alb_rollback)</td>
      <td>[ibmcloud ks alb-types](#cs_alb_types)</td>
      <td>[ibmcloud ks alb-update](#cs_alb_update)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks albs](#cs_albs)</td>
      <td> </td>
      <td> </td>
      <td> </td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tabla Mandatos de registro">
<caption>Mandatos de registro</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Mandatos de registro</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[ibmcloud ks logging-autoupdate-enable](#cs_log_autoupdate_enable)</td>
      <td>[ibmcloud ks logging-autoupdate-disable](#cs_log_autoupdate_disable)</td>
      <td>[ibmcloud ks logging-autoupdate-get](#cs_log_autoupdate_get)</td>
      <td>[ibmcloud ks logging-config-create](#cs_logging_create)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks logging-config-get](#cs_logging_get)</td>
      <td>[ibmcloud ks logging-config-refresh](#cs_logging_refresh)</td>
      <td>[ibmcloud ks logging-config-rm](#cs_logging_rm)</td>
      <td>[ibmcloud ks logging-config-update](#cs_logging_update)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks logging-filter-create](#cs_log_filter_create)</td>
      <td>[ibmcloud ks logging-filter-update](#cs_log_filter_update)</td>
      <td>[ibmcloud ks logging-filter-get](#cs_log_filter_view)</td>
      <td>[ibmcloud ks logging-filter-rm](#cs_log_filter_delete)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks logging-collect](#cs_log_collect)</td>
      <td>[ibmcloud ks logging-collect-status](#cs_log_collect_status)</td>
      <td> </td>
      <td> </td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tabla Mandatos de equilibrador de carga de red (NLB)">
<caption>Mandatos de equilibrador de carga de red (NLB)</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Mandatos de equilibrador de carga de red (NLB)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[ibmcloud ks nlb-dns-add](#cs_nlb-dns-add)</td>
      <td>[ibmcloud ks nlb-dns-create](#cs_nlb-dns-create)</td>
      <td>[ibmcloud ks nlb-dnss](#cs_nlb-dns-ls)</td>
      <td>[ibmcloud ks nlb-dns-rm](#cs_nlb-dns-rm)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks nlb-dns-monitor-configure](#cs_nlb-dns-monitor-configure)</td>
      <td>[ibmcloud ks nlb-dns-monitor-get](#cs_nlb-dns-monitor-get)</td>
      <td>[ibmcloud ks nlb-dns-monitor-disable](#cs_nlb-dns-monitor-disable)</td>
      <td>[ibmcloud ks nlb-dns-monitor-enable](#cs_nlb-dns-monitor-enable)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks nlb-dns-monitor-ls](#cs_nlb-dns-monitor-ls)</td>
      <td>[ibmcloud ks nlb-dns-monitor-status](#cs_nlb-dns-monitor-status)</td>
      <td> </td>
      <td> </td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tabla Mandatos de región">
<caption>Mandatos de región</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de región</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks region](#cs_region)</td>
    <td>[ibmcloud ks region-set](#cs_region-set)</td>
    <td>[ibmcloud ks regions](#cs_regions)</td>
  <td>[ibmcloud ks zones](#cs_datacenters)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabla Mandatos de nodo trabajador">
<caption>Mandatos de nodo trabajador</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de nodo trabajador</th>
 </thead>
 <tbody>
    <tr>
      <td>En desuso: [ibmcloud ks worker-add](#cs_worker_add)</td>
      <td>[ibmcloud ks worker-get](#cs_worker_get)</td>
      <td>[ibmcloud ks worker-reboot](#cs_worker_reboot)</td>
      <td>[ibmcloud ks worker-reload](#cs_worker_reload)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks worker-rm](#cs_worker_rm)</td>
      <td>[ibmcloud ks worker-update](#cs_worker_update)</td>
      <td>[ibmcloud ks workers](#cs_workers)</td>
      <td> </td>
    </tr>
  </tbody>
</table>

<table summary="Tabla de mandatos de la agrupación de nodos trabajadores">
<caption>Mandatos de la agrupación de nodos trabajadores</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de la agrupación de nodos trabajadores</th>
 </thead>
 <tbody>
    <tr>
      <td>[ibmcloud ks worker-pool-create](#cs_worker_pool_create)</td>
      <td>[ibmcloud ks worker-pool-get](#cs_worker_pool_get)</td>
      <td>[ibmcloud ks worker-pool-rebalance](#cs_rebalance)</td>
      <td>[ibmcloud ks worker-pool-resize](#cs_worker_pool_resize)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks worker-pool-rm](#cs_worker_pool_rm)</td>
      <td>[ibmcloud ks worker-pools](#cs_worker_pools)</td>
      <td>[ibmcloud ks zone-add](#cs_zone_add)</td>
      <td>[ibmcloud ks zone-network-set](#cs_zone_network_set)</td>
    </tr>
    <tr>
     <td>[ibmcloud ks zone-rm](#cs_zone_rm)</td>
     <td></td>
    </tr>
  </tbody>
</table>

## Mandatos de API
{: #api_commands}

### ibmcloud ks api
{: #cs_cli_api}

Establecer como destino el punto final de API para {{site.data.keyword.containerlong_notm}}. Si no especifica un punto final, puede ver información sobre el punto final actual de destino.
{: shortdesc}

Se da soporte a los puntos finales siguientes:
* Global: `https://containers.cloud.ibm.com`
* Dallas (EE. UU. sur, us-south): `https://us-south.containers.cloud.ibm.com`
* Frankfurt (UE central, eu-de): `https://eu-central.containers.cloud.ibm.com`
* Londres (RU sur, eu-gb): `https://uk-south.containers.cloud.ibm.com`
* Sídney (AP sur, au-syd): `https://ap-south.containers.cloud.ibm.com`
* Tokio (AP norte, jp-tok): `https://ap-north.containers.cloud.ibm.com`
* Washington, D.C. (EE. UU. este, us-east): `https://us-east.containers.cloud.ibm.com`

```
ibmcloud ks api --endpoint ENDPOINT [--insecure] [--skip-ssl-validation] [--api-version VALUE] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: ninguno

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--endpoint <em>ENDPOINT</em></code></dt>
   <dd>El punto final de API de {{site.data.keyword.containerlong_notm}}. Tenga en cuenta que este punto final es distinto de los puntos finales de {{site.data.keyword.Bluemix_notm}}. Este valor es necesario para establecer el punto final de API.
   </dd>

   <dt><code>--insecure</code></dt>
   <dd>Permite una conexión HTTP insegura. Este distintivo es opcional.</dd>

   <dt><code>--skip-ssl-validation</code></dt>
   <dd>Permite certificados SSL inseguros. Este distintivo es opcional.</dd>

   <dt><code>--api-version VALUE</code></dt>
   <dd>Especifica la versión de API del servicio que desea utilizar. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**: Ver información sobre el punto final de API actual de destino.
```
ibmcloud ks api
```
{: pre}

```
API Endpoint:          https://containers.cloud.ibm.com
API Version:           v1
Skip SSL Validation:   false
Region:                us-south
```
{: screen}


### ibmcloud ks api-key-info
{: #cs_api_key_info}

Ver el nombre y la dirección de correo electrónico del propietario de la clave de API de {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) en una región y grupo de recursos de {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks api-key-info --cluster CLUSTER [--json] [-s]
```
{: pre}

La clave de API de {{site.data.keyword.Bluemix_notm}} se establece automáticamente para una región y para un grupo de recursos cuando se realiza la primera acción que requiere la política de acceso de administrador de {{site.data.keyword.containerlong_notm}}. Por ejemplo, uno de los usuarios administrativos crea el primer clúster en el grupo de recursos `default` en la región `us-south`. De ese modo, la clave de API de {{site.data.keyword.Bluemix_notm}} IAM para este usuario se almacena en la cuenta para este grupo de recursos y región. La clave de API se utiliza para pedir recursos en la infraestructura de IBM Cloud (SoftLayer), como nodos trabajadores nuevos o VLAN. Se puede establecer una clave de API distinta para cada región dentro de un grupo de recursos.

Cuando un usuario distinto realiza una acción en este grupo de recursos y región que requiere interacción con el portafolio de infraestructura de IBM Cloud (SoftLayer), como crear un nuevo clúster o recargar un nodo trabajador, la clave de API almacenada se utiliza para determinar si existen suficientes permisos para realizar esa acción. Para asegurarse de que las acciones relacionadas con la infraestructura en el clúster se realicen correctamente, puede asignar a los usuarios administradores de {{site.data.keyword.containerlong_notm}} la política de acceso a infraestructura **Superusuario**. Para obtener más información, consulte [Gestión de acceso de usuario](/docs/containers?topic=containers-users#infra_access).

Si necesita actualizar la clave de API que hay almacenada para un grupo de recursos y región, puede hacerlo mediante la ejecución del mandato [ibmcloud ks api-key-reset](#cs_api_key_reset). Este mandato requiere la política de acceso de administrador de {{site.data.keyword.containerlong_notm}} y almacena la clave de API del usuario que ejecuta este mandato en la cuenta.

**Nota:** la clave de API que se devuelve en este mandato no se puede utilizar si las credenciales de infraestructura de IBM Cloud (SoftLayer) se han establecido manualmente mediante el mandato [ibmcloud ks credential-set](#cs_credentials_set).

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  ibmcloud ks api-key-info --cluster my_cluster
  ```
  {: pre}


### ibmcloud ks api-key-reset
{: #cs_api_key_reset}

Sustituir la clave de API de {{site.data.keyword.Bluemix_notm}} IAM actual en una región de {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks api-key-reset [-s]
```
{: pre}

Este mandato requiere la política de acceso de administrador de {{site.data.keyword.containerlong_notm}} y almacena la clave de API del usuario que ejecuta este mandato en la cuenta. La clave de API de {{site.data.keyword.Bluemix_notm}} IAM es necesaria para pedir infraestructura del portafolio de infraestructura de IBM Cloud (SoftLayer). Una vez almacenada, la clave de API se utiliza para cada acción en una región que requiere permisos de infraestructura, con independencia del usuario que ejecute este mandato. Para obtener más información sobre cómo funcionan las claves de API de {{site.data.keyword.Bluemix_notm}} IAM, consulte el [mandato `ibmcloud ks api-key-info`](#cs_api_key_info).

Antes de utilizar este mandato, asegúrese de que el usuario que ejecuta este mandato tiene los [permisos de {{site.data.keyword.containerlong_notm}} y de la infraestructura IBM Cloud (SoftLayer)](/docs/containers?topic=containers-users#users) necesarios.
{: important}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>


**Ejemplo**:

  ```
  ibmcloud ks api-key-reset
  ```
  {: pre}



### ibmcloud ks apiserver-config-get
{: #cs_apiserver_config_get}

Obtener información sobre una opción para la configuración de servidor de API de Kubernetes de un clúster. Este mandato debe combinarse con uno de los siguientes submandatos para la opción de configuración de la que desea información.
{: shortdesc}

#### ibmcloud ks apiserver-config-get audit-webhook
{: #cs_apiserver_config_get_audit_webhook}

Ver el URL para el servicio de registro remoto al que está enviando registros de auditoría de servidor de API. El URL se ha especificado al crear
el programa de fondo del webhook para la configuración del servidor de API.
{: shortdesc}

```
ibmcloud ks apiserver-config-get audit-webhook --cluster CLUSTER
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks apiserver-config-get audit-webhook --cluster my_cluster
  ```
  {: pre}


### ibmcloud ks apiserver-config-set
{: #cs_apiserver_config_set}

Establecer una opción para la configuración de servidor de API de Kubernetes de un clúster. Este mandato debe combinarse con uno de los siguientes submandatos para la opción de configuración que desea establecer.
{: shortdesc}

#### ibmcloud ks apiserver-config-set audit-webhook
{: #cs_apiserver_set}

Establecer el programa de fondo de webhook para la configuración del servidor de API. El programa de fondo del webhook reenvía registros de auditoría del servidor de API a un servidor remoto. Se crea una configuración de webhook basándose en la información que proporciona en los distintivos de este mandato. Si no proporciona ninguna información en los distintivos, se utiliza una configuración de webhook predeterminada.
{: shortdesc}

```
ibmcloud ks apiserver-config-set audit-webhook --cluster CLUSTER [--remoteServer SERVER_URL_OR_IP] [--caCert CA_CERT_PATH] [--clientCert CLIENT_CERT_PATH] [--clientKey CLIENT_KEY_PATH]
```
{: pre}

Después de establecer el webhook, debe ejecutar el mandato `ibmcloud ks apiserver-refresh` para aplicar los cambios en el maestro de Kubernetes.
{: note}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--remoteServer <em>SERVER_URL</em></code></dt>
   <dd>La dirección IP o el URL del servicio de registro remoto al que desea enviar registros de auditoría. Si proporciona un URL de servidor no seguro, se ignoran los certificados. Este valor es opcional.</dd>

   <dt><code>--caCert <em>CA_CERT_PATH</em></code></dt>
   <dd>La vía de acceso de archivo del certificado de CA que se utiliza para verificar el servicio de registro remoto. Este valor es opcional.</dd>

   <dt><code>--clientCert <em>CLIENT_CERT_PATH</em></code></dt>
   <dd>La vía de acceso de archivo del certificado de cliente que se utiliza para autenticarse en el servicio de registro remoto. Este valor es opcional.</dd>

   <dt><code>--clientKey <em> CLIENT_KEY_PATH</em></code></dt>
   <dd>La vía de acceso de archivo de la clave de cliente correspondiente que se utiliza para conectarse al servicio de registro remoto. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks apiserver-config-set audit-webhook --cluster my_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver-audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver-audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver-audit/key.pem
  ```
  {: pre}


### ibmcloud ks apiserver-config-unset
{: #cs_apiserver_config_unset}

Inhabilitar una opción para la configuración de servidor de API de Kubernetes de un clúster. Este mandato debe combinarse con uno de los siguientes submandatos para la opción de configuración que desea eliminar.
{: shortdesc}

#### ibmcloud ks apiserver-config-unset audit-webhook
{: #cs_apiserver_unset}

Inhabilitar la configuración del programa de fondo del webhook para el servidor de API del clúster. Al inhabilitar el programa de fondo del webhook, se detiene el reenvío de registros de auditoría del servidor de API a un servidor remoto.
{: shortdesc}

```
ibmcloud ks apiserver-config-unset audit-webhook --cluster CLUSTER
```
{: pre}

Después de inhabilitar el webhook, debe ejecutar el mandato `ibmcloud ks apiserver-refresh` para aplicar los cambios en el maestro de Kubernetes.
{: note}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks apiserver-config-unset audit-webhook --cluster my_cluster
  ```
  {: pre}

### ibmcloud ks apiserver-refresh
{: #cs_apiserver_refresh}

Aplicar los cambios de configuración para el maestro de Kubernetes que se solicitan con los mandatos `ibmcloud ks apiserver-config-set`,
`apiserver-config-unset`, `cluster-feature-enable` o `cluster-feature-disable`. Si un cambio de configuración requiere un reinicio, se reinicia el componente maestro de Kubernetes afectado. Si los cambios de configuración se pueden aplicar sin un reinicio, no se reinicia ningún componente maestro de Kubernetes. Sus nodos trabajadores, apps y recursos no se modifican y continúan en ejecución.
{: shortdesc}

```
ibmcloud ks apiserver-refresh --cluster CLUSTER [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  ibmcloud ks apiserver-refresh --cluster my_cluster
  ```
  {: pre}


<br />


## Mandatos de uso del plugin de CLI
{: #cli_plug-in_commands}

### ibmcloud ks help
{: #cs_help}

Ver una lista de mandatos y parámetros admitidos.
{: shortdesc}

```
ibmcloud ks help
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: ninguno

<strong>Opciones del mandato</strong>: Ninguna


### ibmcloud ks init
{: #cs_init}

Inicializar el plugin de {{site.data.keyword.containerlong_notm}} o especifique la región donde desea crear o acceder a clústeres de Kubernetes.
{: shortdesc}

Se da soporte a los puntos finales siguientes:
* Global: `https://containers.cloud.ibm.com`
* Dallas (EE. UU. sur, us-south): `https://us-south.containers.cloud.ibm.com`
* Frankfurt (UE central, eu-de): `https://eu-central.containers.cloud.ibm.com`
* Londres (RU sur, eu-gb): `https://uk-south.containers.cloud.ibm.com`
* Sídney (AP sur, au-syd): `https://ap-south.containers.cloud.ibm.com`
* Tokio (AP norte, jp-tok): `https://ap-north.containers.cloud.ibm.com`
* Washington, D.C. (EE. UU. este, us-east): `https://us-east.containers.cloud.ibm.com`

```
ibmcloud ks init [--host HOST] [--insecure] [-p] [-u] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: ninguno

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--host <em>HOST</em></code></dt>
   <dd>El punto final de API de {{site.data.keyword.containerlong_notm}} que se va a utilizar. Este valor es opcional. [Ver valores disponibles de punto final de API.](/docs/containers?topic=containers-regions-and-zones#container_regions)</dd>

   <dt><code>--insecure</code></dt>
   <dd>Permite una conexión HTTP insegura.</dd>

   <dt><code>-p</code></dt>
   <dd>Su contraseña de IBM Cloud.</dd>

   <dt><code>-u</code></dt>
   <dd>Su nombre de usuario de IBM Cloud</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplos**:
*  Ejemplo para definir como destino el punto final de la región sur de Estados Unidos:
  ```
  ibmcloud ks init --host https://us-south.containers.cloud.ibm.com
  ```
  {: pre}
*  Ejemplo para definir como destino el punto final global:
  ```
  ibmcloud ks init --host https://containers.cloud.ibm.com
  ```
  {: pre}

### ibmcloud ks messages
{: #cs_messages}

Ver los mensajes actuales para el usuario IBMid.
{: shortdesc}

```
ibmcloud ks messages
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: ninguno

<br />


## Mandatos de clúster: Gestión
{: #cluster_mgmt_commands}

### ibmcloud ks addon-versions
{: #cs_addon_versions}

Ver una lista de las versiones soportadas de complementos gestionados en {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks addon-versions [--addon ADD-ON_NAME] [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: ninguno

**Opciones del mandato**:

  <dl>
  <dt><code>--addon <em>ADD-ON_NAME</em></code></dt>
  <dd>Opcional: Especifique un nombre de complemento, como por ejemplo <code>istio</code> o <code>knative</code>, para filtrar las versiones.</dd>

  <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplo**:

  ```
  ibmcloud ks addon-versions --addon istio
  ```
  {: pre}

### ibmcloud ks cluster-addon-disable
{: #cs_cluster_addon_disable}

Inhabilitar un complemento gestionado en un clúster existente. Este mandato debe combinarse con uno de los siguientes submandatos para el complemento gestionado que se desea inhabilitar.
{: shortdesc}

#### ibmcloud ks cluster-addon-disable <ph class="ignoreSpelling">istio</ph>
{: #cs_cluster_addon_disable_istio}

Inhabilitar el complemento de Istio gestionado. Elimina todos los componentes principales de Istio del clúster, incluido Prometheus.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable istio --cluster CLUSTER [-f]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:
<dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>
   <dt><code>-f</code></dt>
   <dd>Opcional: Este complemento es una dependencia para los complementos gestionados <code>istio-extras</code>, <code>istio-sample-bookinfo</code> y <code>knative</code>. Incluya este distintivo para inhabilitar también dichos complementos.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-addon-disable istio --cluster my_cluster
  ```
  {: pre}

#### ibmcloud ks cluster-addon-disable istio-extras
{: #cs_cluster_addon_disable_istio_extras}

Inhabilitar el complemento adicional de Istio gestionado. Elimina Grafana, Jeager y Kiali del clúster.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable istio-extras --cluster CLUSTER [-f]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Opcional: Este complemento es una dependencia para el complemento gestionado <code>istio-sample-bookinfo</code>. Incluya este distintivo para inhabilitar también dicho complemento.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-addon-disable istio-extras --cluster my_cluster
  ```
  {: pre}

#### ibmcloud ks cluster-addon-disable istio-sample-bookinfo
{: #cs_cluster_addon_disable_istio_sample_bookinfo}

Inhabilitar el complemento BookInfo de Istio gestionado. Elimina todos los despliegues, pods y otros recursos de la app BookInfo del clúster.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster CLUSTER
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster my_cluster
  ```
  {: pre}

#### ibmcloud ks cluster-addon-disable <ph class="ignoreSpelling">knative</ph>
{: #cs_cluster_addon_disable_knative}

Inhabilitar el complemento Knative gestionado para eliminar la infraestructura sin servidor de Knative del clúster.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable knative --cluster CLUSTER
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks cluster-addon-disable knative --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-disable kube-terminal
{: #cs_cluster_addon_disable_kube-terminal}

Inhabilitar el complemento [Kubernetes Terminal](/docs/containers?topic=containers-cs_cli_install#cli_web). Para utilizar Kubernetes Terminal en la consola del clúster de {{site.data.keyword.containerlong_notm}}, antes debe volver a habilitar el complemento.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable kube-terminal --cluster CLUSTER
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>
</dl>

**Ejemplo**:

```
ibmcloud ks cluster-addon-disable kube-terminal --cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-addon-enable
{: #cs_cluster_addon_enable}

Habilitar un complemento gestionado en un clúster existente. Este mandato debe combinarse con uno de los siguientes submandatos para el complemento gestionado que se desea habilitar.
{: shortdesc}

#### ibmcloud ks cluster-addon-enable <ph class="ignoreSpelling">istio</ph>
{: #cs_cluster_addon_enable_istio}

Habilitar el [complemento Istio](/docs/containers?topic=containers-istio) gestionado. Instala los componentes principales de Istio, incluido Prometheus.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable istio --cluster CLUSTER [--version VERSION]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Opcional: especifique la versión del complemento a instalar. Si no se especifica ninguna versión, se instala la versión predeterminada.</dd>
</dl>

**Ejemplo**:

```
ibmcloud ks cluster-addon-enable istio --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-enable istio-extras
{: #cs_cluster_addon_enable_istio_extras}

Habilitar el complemento adicional de Istio gestionado. Instala Grafana, Jeager y Kiali para proporcionar funciones adicionales de supervisión,
rastreo y visualización para Istio.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable istio-extras --cluster CLUSTER [--version VERSION] [-y]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Opcional: especifique la versión del complemento a instalar. Si no se especifica ninguna versión, se instala la versión predeterminada.</dd>

<dt><code>-y</code></dt>
<dd>Opcional: Habilite la dependencia de complemento <code>istio</code>.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks cluster-addon-enable istio-extras --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-enable istio-sample-bookinfo
{: #cs_cluster_addon_enable_istio_sample_bookinfo}

Habilitar el complemento BookInfo de Istio gestionado. Despliega la [aplicación de ejemplo BookInfo para Istio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://istio.io/docs/examples/bookinfo/) en el espacio de nombres <code>default</code>.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster CLUSTER [--version VERSION] [-y]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Opcional: especifique la versión del complemento a instalar. Si no se especifica ninguna versión, se instala la versión predeterminada.</dd>

<dt><code>-y</code></dt>
<dd>Opcional: Habilite las dependencias de complementos <code>istio</code> e <code>istio-extras</code>.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-enable <ph class="ignoreSpelling">knative</ph>
{: #cs_cluster_addon_enable_knative}

Habilitar el [complemento Knative](/docs/containers?topic=containers-knative_tutorial) gestionado para instalar la infraestructura sin servidor de Knative.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable knative --cluster CLUSTER [--version VERSION] [-y]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Opcional: especifique la versión del complemento a instalar. Si no se especifica ninguna versión, se instala la versión predeterminada.</dd>

<dt><code>-y</code></dt>
<dd>Opcional: Habilite la dependencia de complemento <code>istio</code>.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks cluster-addon-enable knative --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-enable kube-terminal
{: #cs_cluster_addon_enable_kube-terminal}

Habilitar el complemento [Kubernetes Terminal](/docs/containers?topic=containers-cs_cli_install#cli_web) para utilizar Kubernetes Terminal en la consola del clúster de {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable kube-terminal --cluster CLUSTER [--version VERSION]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Opcional: especifique la versión del complemento a instalar. Si no se especifica ninguna versión, se instala la versión predeterminada.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks cluster-addon-enable kube-terminal --cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-addons
{: #cs_cluster_addons}

Obtener una lista de los complementos gestionados que están habilitados en el clúster.
{: shortdesc}

```
ibmcloud ks cluster-addons --cluster CLUSTER
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks cluster-addons --cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-config
{: #cs_cluster_config}

Después de iniciar sesión, descargar certificados y datos de configuración de Kubernetes para conectar el clúster y ejecutar mandatos `kubectl`. Los archivos se descargan en `user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>`.
{: shortdesc}

```
ibmcloud ks cluster-config --cluster CLUSTER [--admin] [--export] [--network] [--skip-rbac] [-s] [--yaml]
```
{: pre}

**Permisos mínimos necesarios**: Rol de servicio **Visor** o **Lector** de {{site.data.keyword.Bluemix_notm}} IAM para el clúster en {{site.data.keyword.containerlong_notm}}. Además, si solo tiene un rol de plataforma o solo un rol de servicio, se aplican restricciones adicionales.
* **Plataforma**: Si solo tiene un rol de plataforma, puede ejecutar este mandato, pero necesita un [rol de servicio](/docs/containers?topic=containers-users#platform) o una [política de RBAC personalizada](/docs/containers?topic=containers-users#role-binding) para llevar a cabo acciones de Kubernetes en el clúster.
* **Servicio**: Si solo tiene un rol de servicio, puede ejecutar este mandato. Sin embargo, el administrador del clúster debe proporcionarle el nombre y el ID del clúster, ya que no puede ejecutar el mandato `ibmcloud ks clusters` ni iniciar la consola de {{site.data.keyword.containerlong_notm}} para ver clústeres. Después, puede [iniciar el panel de control de Kubernetes desde la CLI](/docs/containers?topic=containers-app#db_cli) y trabajar con Kubernetes.

**Opciones del mandato**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--admin</code></dt>
<dd>Descargar los certificados TLS y archivos de permisos para el rol de superusuario. Puede utilizar los certificados para automatizar las tareas de un clúster sin tener que volverse a autenticar. Los archivos se descargan en `<user_home_directory>/.bluemix/plugins/container-service/clusters/<cluster_name>-admin`. Este valor es opcional.</dd>

<dt><code>--network</code></dt>
<dd>Descargar el archivo de configuración de Calico, los certificados TLS y los archivos de permisos necesarios para ejecutar los mandatos <code>calicoctl</code> en el clúster. Este valor es opcional. **Nota**: para obtener el mandato de exportación para los certificados y los datos de configuración de Kubernetes descargados, debe ejecutar este mandato sin este distintivo.</dd>

<dt><code>--export</code></dt>
<dd>Descargar los datos de configuración y certificados de Kubernetes sin ningún mensaje aparte del mandato de exportación. Como no se muestran mensajes, puede utilizar este distintivo cuando cree scripts automatizados. Este valor es opcional.</dd>

<dt><code>--skip-rbac</code></dt>
<dd>Omitir la adición de roles de RBAC de Kubernetes de usuario basados en los roles de acceso al servicio {{site.data.keyword.Bluemix_notm}} IAM a la configuración del clúster. Incluya esta opción solo si [gestiona sus propios roles RBAC de Kubernetes](/docs/containers?topic=containers-users#rbac). Si utiliza [Roles de acceso al servicio de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-access_reference#service) para gestionar todos los usuarios de RBAC, no incluya esta opción.</dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

<dt><code>--yaml</code></dt>
<dd>Imprime la salida del mandato en formato YAML. Este valor es opcional.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks cluster-config --cluster my_cluster
```
{: pre}


### ibmcloud ks cluster-create
{: #cs_cluster_create}

Crear un clúster en la organización. Para los clústeres gratuitos, especifique el nombre de clúster; todo lo demás se establece en un valor predeterminado. Un clúster gratuito se suprime automáticamente después de 30 días. Puede disponer de un clúster gratuito al mismo tiempo. Para aprovechar todas las funciones de Kubernetes, cree un clúster estándar.
{: shortdesc}

```
ibmcloud ks cluster-create [--file FILE_LOCATION] [--hardware HARDWARE] --zone ZONE --machine-type MACHINE_TYPE --name NAME [--kube-version MAJOR.MINOR.PATCH] [--no-subnet] [--private-vlan PRIVATE_VLAN] [--public-vlan PUBLIC_VLAN] [--private-only] [--private-service-endpoint] [--public-service-endpoint] [--workers WORKER] [--disable-disk-encrypt] [--trusted] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>:
* Rol de plataforma **administrador** para {{site.data.keyword.containerlong_notm}} a nivel de cuenta
* Rol de plataforma **administrador** para {{site.data.keyword.registrylong_notm}} a nivel de cuenta
* Rol de **superusuario** para la infraestructura de IBM Cloud (SoftLayer)

<strong>Opciones del mandato</strong>

<dl>
<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>La vía de acceso al archivo YAML para crear el clúster estándar. En lugar de definir las características de su clúster mediante las opciones que se proporcionan en este mandato, puede utilizar un archivo YAML. Este valor es opcional para clústeres estándares y no está disponible para clústeres gratuitos. <p class="note">Si especifica la misma opción en el mandato y como parámetro en el archivo YAML, el valor en el mandato prevalece sobre el valor del archivo YAML. Por ejemplo, supongamos que define una ubicación en el archivo YAML y utiliza la opción <code>--zone</code> en el mandato; el valor que especifique en la opción del mandato prevalece sobre el valor del archivo YAML.</p>

<pre class="codeblock">
<code>name: <em>&lt;cluster_name&gt;</em>
zone: <em>&lt;zone&gt;</em>
no-subnet: <em>&lt;no-subnet&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
kube-version: <em>&lt;kube-version&gt;</em>
diskEncryption: <em>false</em>
trusted: <em>true</em>
</code></pre>
</dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>El nivel de aislamiento del hardware del nodo trabajador. Utilice el valor dedicated para tener recursos físicos disponibles dedicados solo a usted, o shared para permitir que los recursos físicos se compartan con otros clientes de IBM. El valor predeterminado es shared. Este valor es opcional para clústeres estándares VM y no está disponible para clústeres gratuitos. Para los tipos de máquina nativos, especifique `dedicated`.</dd>

<dt><code>--zone <em>ZONE</em></code></dt>
<dd>Este valor es obligatorio para los clústeres estándar. Se pueden crear clústeres gratuitos en la región que se ha fijado como destino con el mandato <code>ibmcloud ks region-set</code>, pero no se puede especificar la zona.

<p>Consulte las [zonas disponibles](/docs/containers?topic=containers-regions-and-zones#zones).</p>

<p class="note">Si selecciona una zona que se encuentra fuera de su país, tenga en cuenta que es posible que se requiera autorización legal para poder almacenar datos físicamente en un país extranjero.</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Elija un tipo de máquina. Puede desplegar los nodos trabajadores como máquinas virtuales en hardware dedicado o compartido, o como máquinas físicas en servidores nativos. Los tipos de máquinas físicas y virtuales varían según la zona en la que se despliega el clúster. Para obtener más información, consulte la documentación correspondiente al [mandato](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types) `ibmcloud ks machine-types`. Este valor es obligatorio para clústeres estándares y no está disponible para clústeres gratuitos.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>El nombre del clúster.  Este valor es obligatorio. El nombre debe empezar por una letra, puede contener letras, números, guiones (-) y debe tener 35 caracteres como máximo. El nombre del clúster y la región en la que el clúster se despliega forman el nombre de dominio completo para el subdominio de Ingress. Para garantizar que el subdominio de Ingress es exclusivo dentro de una región, el nombre del clúster se puede truncar y se le puede añadir un valor aleatorio al nombre de dominio de Ingress.
</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>La versión Kubernetes del nodo maestro del clúster. Este valor es opcional. Cuando no se especifica la versión, el clúster se crea con el valor predeterminado de las versiones de Kubernetes soportadas. Para ver todas las versiones disponibles, ejecute <code>ibmcloud ks kube-versions</code>.
</dd>

<dt><code>--no-subnet</code></dt>
<dd>De forma predeterminada, se crea una subred portátil pública y una privada en la VLAN asociada con el clúster. Incluya el distintivo <code>--no-subnet</code> para evitar crear subredes con el clúster. Puede [crear](#cs_cluster_subnet_create) o [añadir](#cs_cluster_subnet_add) subredes a un clúster más adelante.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>

<ul>
<li>Este parámetro no está disponible para clústeres gratuitos.</li>
<li>Si este es el primer clúster estándar que crea en esta zona, no incluya este distintivo. Al crear clústeres se crea automáticamente una VLAN privada.</li>
<li>Si previamente ha creado un clúster estándar en esta zona o una VLAN privada en la infraestructura de IBM Cloud (SoftLayer), debe especificar la VLAN privada. Los direccionadores de VLAN privadas siempre
empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre
empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos.</li>
</ul>

<p>Para saber si ya tiene una VLAN privada para una zona específica o para encontrar el nombre de una VLAN privada existente, ejecute <code>ibmcloud ks vlans <em>&lt;zone&gt;</em></code>.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>
<ul>
<li>Este parámetro no está disponible para clústeres gratuitos.</li>
<li>Si este es el primer clúster estándar que crea en esta zona, no utilice este distintivo. Al crear el clúster se crea automáticamente una VLAN pública.</li>
<li>Si previamente ha creado un clúster estándar en esta zona o una VLAN pública en la infraestructura de IBM Cloud (SoftLayer), especifique la VLAN pública. Si desea conectar los nodos trabajadores solo a una VLAN privada, no especifique esta opción. Los direccionadores de VLAN privadas siempre
empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre
empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos.</li>
</ul>

<p>Para saber si ya tiene una VLAN pública para una zona específica o para encontrar el nombre de una VLAN pública existente, ejecute <code>ibmcloud ks vlans <em>&lt;zone&gt;</em></code>.</p></dd>

<dt><code>--private-only</code></dt>
  <dd>Utilice esta opción para evitar que se cree una VLAN pública. Sólo es necesario cuando especifica el distintivo `--private-vlan` y no incluye el distintivo `--public-vlan`. <p class="note">Si los nodos trabajadores se han configurado únicamente con una VLAN privada, debe habilitar el punto final de servicio privado o configurar un dispositivo de pasarela. Para obtener más información, consulte [Clústeres privados](/docs/containers?topic=containers-plan_clusters#private_clusters).</p></dd>

<dt><code>--private-service-endpoint</code></dt>
  <dd>**Clústeres estándares que ejecutan Kubernetes versión 1.11 o posteriores en [cuentas habilitadas para VRF](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)**: Habilite el [punto final de servicio privado](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private) para que el maestro de Kubernetes y los nodos trabajadores se comuniquen sobre la VLAN privada. Además, puede optar por habilitar el punto final de servicio público utilizando el distintivo `--public-service-endpoint` para acceder a su clúster a través de Internet. Si solo habilita el punto final de servicio privado, debe estar conectado a la VLAN privada para comunicarse con el maestro de Kubernetes. Después de habilitar un punto final de servicio privado, no podrá inhabilitarlo más tarde.<br><br>Después de crear el clúster, puede obtener el punto final con el mandato `ibmcloud ks cluster-get <cluster_name_or_ID>`.</dd>

<dt><code>--public-service-endpoint</code></dt>
  <dd>**Clústeres estándares que ejecutan Kubernetes versión 1.11 o posteriores**: Habilite el [punto final de servicio público](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_public) para que se pueda acceder al maestro de Kubernetes a través de la red pública, por ejemplo para ejecutar mandatos `kubectl` desde el terminal. Si tiene una [cuenta habilitada para VRF](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started) y también incluye el distintivo `--private-service-endpoint`, [la comunicación entre el maestro y el nodo trabajador](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_both) se realiza en la red privada. Posteriormente puede inhabilitar el punto final de servicio público si desea un clúster solo privado.<br><br>Después de crear el clúster, puede obtener el punto final con el mandato `ibmcloud ks cluster-get <cluster_name_or_ID>`.</dd>

<dt><code>--workers WORKER</code></dt>
<dd>El número de nodos trabajadores que desea desplegar en el clúster. Si no especifica esta opción, se crea un clúster con 1 nodo trabajador. Este valor es opcional para clústeres estándares y no está disponible para clústeres gratuitos.

<p class="important">A cada nodo trabajador se la asigna un ID exclusivo y un nombre de dominio que no se debe cambiar de forma manual después de haber creado el clúster. Si se cambia el nombre de dominio o el ID se impide que el maestro de Kubernetes gestione el clúster.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Los nodos trabajadores tienen cifrado de disco AES de 256 bits de forma predeterminada; [más información](/docs/containers?topic=containers-security#encrypted_disk). Para inhabilitar el cifrado, incluya esta opción.</dd>

<dt><code>--trusted</code></dt>
<dd><p>**Solo nativos**: Habilite [Trusted Compute](/docs/containers?topic=containers-security#trusted_compute) para verificar que los nodos trabajadores nativos no se manipulan de forma indebida. Si no habilita la confianza al crear el clúster pero la desea posteriormente, puede utilizar el [mandato](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Una vez que habilita la confianza, no puede inhabilitarla posteriormente.</p>
<p>Para comprobar si el tipo de máquina vacía da soporte a la confianza, compruebe el campo `Trustable` de la información de salida del [mandato](#cs_machine_types) `ibmcloud ks machine-types <zone>`. Para verificar que un clúster tiene habilitada la confianza, consulte el campo **Trust ready** en la información de salida del [mandato](#cs_cluster_get) `ibmcloud ks cluster-get`. Para verificar que un nodo trabajador nativo tiene habilitada la confianza, consulte el campo **Trust** en la información de salida del [mandato](#cs_worker_get) `ibmcloud ks worker-get`.</p></dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplos**:

**Crear un clúster gratuito**: Especifique únicamente el nombre del clúster, el resto se establece en los valores predeterminados. Un clúster gratuito se suprime automáticamente después de 30 días. Puede disponer de un clúster gratuito al mismo tiempo. Para aprovechar todas las funciones de Kubernetes, cree un clúster estándar.

```
ibmcloud ks cluster-create --name my_cluster
```
{: pre}

**Crear su primer clúster estándar**: el primer clúster estándar que se crea en una zona también se crea con una VLAN privada. Por lo tanto, no incluya el distintivo `--public-vlan`.
{: #example_cluster_create}

```
ibmcloud ks cluster-create --zone dal10 --private-vlan my_private_VLAN_ID --machine-type b3c.4x16 --name my_cluster --hardware shared --workers 2
```
{: pre}

**Crear clústeres estándar posteriores**: si ya ha creado un clúster estándar en esta zona o con anterioridad ha creado una VLAN pública en la infraestructura de IBM Cloud (SoftLayer), especifique dicha VLAN pública con el distintivo `--public-vlan`. Para saber si ya tiene una VLAN pública para una zona específica o para encontrar el nombre de una VLAN pública existente, ejecute `ibmcloud ks vlans <zone>`.

```
ibmcloud ks cluster-create --zone dal10 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type b3c.4x16 --name my_cluster --hardware shared --workers 2
```
{: pre}

### ibmcloud ks cluster-feature-disable public-service-endpoint
{: #cs_cluster_feature_disable}

**En clústeres que ejecutan Kubernetes versión 1.11 o posteriores**: Inhabilite el punto final de servicio público para un clúster.
{: shortdesc}

```
ibmcloud ks cluster-feature-disable public-service-endpoint --cluster CLUSTER [-s] [-f]
```
{: pre}

**Importante**: Antes de inhabilitar el punto final público, primero debe seguir los pasos siguientes para habilitar el punto final de servicio privado:
1. Habilite el punto final de servicio privado con el mandato `ibmcloud ks cluster-feature-enable private-service-endpoint-endpoint --cluster <cluster_name>`.
2. Siga la indicación de la CLI para renovar el servidor de API maestro de Kubernetes.
3. [Vuelva a cargar todos los nodos trabajadores del clúster para que adopten la configuración de punto final privado.](#cs_worker_reload)

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

```
ibmcloud ks cluster-feature-disable public-service-endpoint --cluster my_cluster
```
{: pre}


### ibmcloud ks cluster-feature-enable
{: #cs_cluster_feature_enable}

Habilitar una característica en un clúster existente. Este mandato debe combinarse con uno de los siguientes submandatos para la característica que se desea habilitar.
{: shortdesc}

#### ibmcloud ks cluster-feature-enable private-service-endpoint
{: #cs_cluster_feature_enable_private_service_endpoint}

**En clústeres que ejecutan Kubernetes versión 1.11 o posteriores**: Habilite el [punto final de servicio privado](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private) para que el maestro de clúster sea de acceso privado.
{: shortdesc}

```
ibmcloud ks cluster-feature-enable private-service-endpoint --cluster CLUSTER [-s]
```
{: pre}

Para ejecutar este mandato:
1. Habilite [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) en la cuenta de la infraestructura de IBM Cloud (SoftLayer).
2. [Habilite la cuenta de {{site.data.keyword.Bluemix_notm}} para que utilice puntos finales de servicio](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
3. Ejecute `ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name>`.
4. Siga la indicación de la CLI para renovar el servidor de API maestro de Kubernetes.
5. [Vuelva a cargar todos los nodos trabajadores](#cs_worker_reload) del clúster para que adopten la configuración de punto final privado.

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

```
ibmcloud ks cluster-feature-enable private-service-endpoint --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-feature-enable public-service-endpoint
{: #cs_cluster_feature_enable_public_service_endpoint}

**En clústeres que ejecutan Kubernetes versión 1.11 o posteriores**: Habilite el [punto final de servicio público](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_public) para que el maestro de clúster sea de acceso público.
{: shortdesc}

```
ibmcloud ks cluster-feature-enable public-service-endpoint --cluster CLUSTER [-s]
```
{: pre}

Después de ejecutar este mandato, debe renovar el servidor de API para que utilice el punto final de servicio siguiendo la indicación de la CLI.

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

```
ibmcloud ks cluster-feature-enable public-service-endpoint --cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-feature-enable trusted
{: #cs_cluster_feature_enable_trusted}

Habilitar [Trusted Compute](/docs/containers?topic=containers-security#trusted_compute) para todos los nodos trabajadores locales admitidos que están en el clúster. Después de habilitar la confianza, no puede inhabilitarla posteriormente para el clúster.
{: shortdesc}

```
ibmcloud ks cluster-feature-enable trusted --cluster CLUSTER [-s] [-f]
```
{: pre}

Para comprobar si el tipo de máquina vacía da soporte a la confianza, compruebe el campo **Trustable** de la información de salida del [mandato](#cs_machine_types) `ibmcloud ks machine-types <zone>`. Para verificar que un clúster tiene habilitada la confianza, consulte el campo **Trust ready** en la información de salida del [mandato](#cs_cluster_get) `ibmcloud ks cluster get`. Para verificar que un nodo trabajador nativo tiene habilitada la confianza, consulte el campo **Trust** en la información de salida del [mandato](#cs_worker_get) `ibmcloud ks worker get`.

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar la opción <code>--trusted</code> sin solicitudes de usuario. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

```
ibmcloud ks cluster-feature-enable trusted --cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-get
{: #cs_cluster_get}

Ver información sobre un clúster de la organización.
{: shortdesc}

```
ibmcloud ks cluster-get --cluster CLUSTER [--json] [--showResources] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code><em>--showResources</em></code></dt>
   <dd>Mostrar más recursos del clúster, como complementos, VLAN, subredes y almacenamiento.</dd>


  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-get --cluster my_cluster --showResources
  ```
  {: pre}

**Salida de ejemplo**:

```
Name:                           mycluster
ID:                             df253b6025d64944ab99ed63bb4567b6
State:                          normal
Created:                        2018-09-28T15:43:15+0000
Location:                       dal10
Master URL:                     https://c3.<region>.containers.cloud.ibm.com:30426
Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
Master Location:                Dallas
Master Status:                  Ready (21 hours ago)
Ingress Subdomain:              mycluster.us-south.containers.appdomain.cloud
Ingress Secret:                 mycluster
Workers:                        6
Worker Zones:                   dal10, dal12
Version:                        1.11.3_1524
Owner:                          owner@email.com
Monitoring Dashboard:           ...
Resource Group ID:              a8a12accd63b437bbd6d58fb6a462ca7
Resource Group Name:            Default

Subnet VLANs
  VLAN ID   Subnet CIDR         Public   User-managed
  2234947   10.xxx.xx.xxx/29    false    false
  2234945   169.xx.xxx.xxx/29  true    false

```
{: screen}

### ibmcloud ks cluster-pull-secret-apply
{: #cs_cluster_pull_secret_apply}

Crear un ID de servicio de {{site.data.keyword.Bluemix_notm}} IAM para el clúster, crear una política para el ID de servicio que signa el rol de **Lector** de acceso al servicio en {{site.data.keyword.registrylong_notm}} y crear una clave de API para el ID de servicio. A continuación, la clave de API se almacena en un `imagePullSecret` de Kubernetes para que pueda extraer imágenes de los espacios de nombres de {{site.data.keyword.registryshort_notm}} para los contenedores que están en el espacio de nombres de Kubernetes `default`. Este proceso se produce automáticamente cuando se crea un clúster. Si ha obtenido un error durante el proceso de creación del clúster o tiene un clúster existente, puede utilizar este mandato para volver a aplicar el proceso.
{: shortdesc}

Cuando ejecuta este mandato, se inicia la creación de credenciales de IAM y de secretos de extracción de imágenes, que puede tardar un rato en finalizar. No puede desplegar contenedores que extraigan una imagen de los dominios de {{site.data.keyword.registrylong_notm}}
`icr.io` hasta que se creen los secretos de extracción de la imagen. Para comprobar los secretos de extracción de la imagen, ejecute `kubectl get secrets | grep icr`.
{: important}

```
ibmcloud ks cluster-pull-secret-apply --cluster CLUSTER
```
{: pre}

Este método de clave de API sustituye el método anterior de autorización de un clúster para acceder a {{site.data.keyword.registrylong_notm}} mediante la creación automática de una [señal](/docs/services/Registry?topic=registry-registry_access#registry_tokens) y el almacenamiento de la señal en un secreto de extracción de imágenes. Ahora, mediante el uso de claves de API de IAM para acceder a {{site.data.keyword.registrylong_notm}}, puede personalizar las políticas de IAM para el ID de servicio para restringir el acceso a los espacios de nombres o a imágenes específicas. Por ejemplo, puede cambiar las políticas de ID de servicio en el secreto de extracción de imágenes del clúster para obtener imágenes solo de una determinada región de registro o un espacio de nombres. Para poder personalizar las políticas de IAM, debe [habilitar las políticas de {{site.data.keyword.Bluemix_notm}} IAM para {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-user#existing_users).

Para obtener más información, consulte [Visión general sobre cómo está autorizado el clúster para extraer imágenes de {{site.data.keyword.registrylong_notm}}](/docs/containers?topic=containers-images#cluster_registry_auth).

Si ha añadido políticas de IAM a un ID de servicio existente, como por ejemplo para restringir el acceso a un registro regional, este mandato restablece el ID de servicio, las políticas de IAM y la clave de API para el secreto de extracción de imágenes.
{: important}

<strong>Permisos mínimos necesarios</strong>:
*  Rol de plataforma de **Operador o Administrador** sobre el clúster en {{site.data.keyword.containerlong_notm}}
*  Rol de plataforma de **Administrador** en {{site.data.keyword.registrylong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   </dl>

**Ejemplo**:

```
ibmcloud ks cluster-pull-secret-apply --cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-refresh
{: #cs_cluster_refresh}

Reiniciar los nodos maestros del clúster para aplicar los nuevos cambios en la configuración de API de Kubernetes. Sus nodos trabajadores, apps y recursos no se modifican y continúan en ejecución.
{: shortdesc}

```
ibmcloud ks cluster-refresh --cluster CLUSTER [-f] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar la eliminación de un clúster sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-refresh --cluster my_cluster
  ```
  {: pre}

### ibmcloud ks cluster-rm
{: #cs_cluster_rm}

Eliminar un clúster de la organización.
{: shortdesc}

```
ibmcloud ks cluster-rm --cluster CLUSTER [--force-delete-storage] [-f] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--force-delete-storage</code></dt>
   <dd>Suprime el clúster y cualquier almacenamiento persistente que utilice el clúster. **Atención**: si incluye este distintivo, los datos almacenados en el clúster o en las instancias de almacenamiento asociadas no se pueden recuperar. Este valor es opcional.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar la eliminación de un clúster sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-rm --cluster my_cluster
  ```
  {: pre}


### ibmcloud ks cluster-update
{: #cs_cluster_update}

Actualizar el nodo maestro de Kubernetes a la versión predeterminada de la API. Durante la actualización, no puede acceder ni cambiar el clúster. Los nodos trabajadores, las apps y los recursos que los usuarios del clúster despliegan no se modifican y continúan ejecutándose.
{: shortdesc}

```
ibmcloud ks cluster-update --cluster CLUSTER [--kube-version MAJOR.MINOR.PATCH] [--force-update] [-f] [-s]
```
{: pre}

Es posible que tenga que modificar los archivos YAML para futuros despliegues. Revise esta [nota del release](/docs/containers?topic=containers-cs_versions) para ver detalles.

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>La versión de Kubernetes del clúster. Si no especifica una versión, el maestro de Kubernetes se actualiza a la versión predeterminada de la API. Para ver todas las versiones disponibles, ejecute [ibmcloud ks kube-versions](#cs_kube_versions). Este valor es opcional.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Intente la actualización incluso si el cambio es superior a dos versiones anteriores. Este valor es opcional.</dd>

   <dt><code>-f</code></dt>
   <dd>Forzar que el mandato se ejecute sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-update --cluster my_cluster
  ```
  {: pre}


### ibmcloud ks clusters
{: #cs_clusters}

Ver una lista de los clústeres de la organización.
{: shortdesc}

```
ibmcloud ks clusters [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

  <dl><dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplo**:

  ```
  ibmcloud ks clusters
  ```
  {: pre}


### ibmcloud ks kube-versions
{: #cs_kube_versions}

Ver una lista de las versiones de Kubernetes soportadas en {{site.data.keyword.containerlong_notm}}. Actualice su [clúster maestro](#cs_cluster_update) y [nodos trabajadores](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) a la versión predeterminada para obtener las prestaciones más estables y recientes.
{: shortdesc}

```
ibmcloud ks kube-versions [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: ninguno

**Opciones del mandato**:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplo**:

  ```
  ibmcloud ks kube-versions
  ```
  {: pre}

<br />



## Mandatos de clúster: Servicios e integraciones
{: #cluster_services_commands}


### ibmcloud ks cluster-service-bind
{: #cs_cluster_service_bind}

Añadir un servicio de {{site.data.keyword.Bluemix_notm}} a un clúster. Para ver los servicios de {{site.data.keyword.Bluemix_notm}} disponibles en el catálogo de {{site.data.keyword.Bluemix_notm}}, ejecute `ibmcloud service offerings`. **Nota**: Sólo puede añadir servicios de {{site.data.keyword.Bluemix_notm}} que den soporte a claves de servicio.
{: shortdesc}

```
ibmcloud ks cluster-service-bind --cluster CLUSTER --namespace KUBERNETES_NAMESPACE --service SERVICE_INSTANCE_GUID [--key SERVICE_INSTANCE_KEY] [--role IAM_ROLE] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}} y rol de **Desarrollador** de Cloud Foundry

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--key <em>SERVICE_INSTANCE_KEY</em></code></dt>
   <dd>El nombre o el GUID de la clave de servicio. Este valor es opcional. Incluya este valor si desea utilizar una clave de servicio existente en lugar de crear una clave de servicio nueva.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>El nombre del espacio de Kubernetes. Este valor es obligatorio.</dd>

   <dt><code>--service <em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>El ID de la instancia de servicio de {{site.data.keyword.Bluemix_notm}} que desea vincular. Para encontrar el ID, ejecute <code>ibmcloud service show <service_instance_name> --guid</code>. Este valor es obligatorio. </dd>

   <dt><code>--role <em>IAM_ROLE</em></code></dt>
   <dd>El rol de {{site.data.keyword.Bluemix_notm}} IAM que desea que tenga la clave de servicio. El valor predeterminado es el rol de servicio de IAM de `Escritor`. No incluya este valor si está utilizando una clave de servicio existente o para servicios que no estén habilitados para IAM, como por ejemplo servicios de Cloud Foundry.<br><br>
   Para ver una lista de los roles disponibles para el servicio, ejecute `ibmcloud iam roles --service <service_name>`. El nombre del servicio es el nombre del servicio en el catálogo, que puede obtener con el mandato `ibmcloud catalog search`.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-service-bind --cluster my_cluster --namespace my_namespace --service my_service_instance
  ```
  {: pre}


### ibmcloud ks cluster-service-unbind
{: #cs_cluster_service_unbind}

Eliminar un servicio de {{site.data.keyword.Bluemix_notm}} de un clúster.
{: shortdesc}

```
ibmcloud ks cluster-service-unbind --cluster CLUSTER --namespace KUBERNETES_NAMESPACE --service SERVICE_INSTANCE_GUID [-s]
```
{: pre}

Cuando se elimina un servicio de {{site.data.keyword.Bluemix_notm}}, las credenciales del servicio se eliminan del clúster. Si un pod sigue utilizando el servicio, falla porque no encuentra las credenciales del servicio.
{: note}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}} y rol de **Desarrollador** de Cloud Foundry

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>El nombre del espacio de Kubernetes. Este valor es obligatorio.</dd>

   <dt><code>--service <em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>El ID de la instancia de servicio de {{site.data.keyword.Bluemix_notm}} que desea eliminar. Para encontrar el ID de la instancia de servicio, ejecute `ibmcloud ks cluster-services <cluster_name_or_ID>`. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-service-unbind --cluster my_cluster --namespace my_namespace --service 8567221
  ```
  {: pre}


### ibmcloud ks cluster-services
{: #cs_cluster_services}

Crear una lista de los servicios que están vinculados a uno o a todos los espacios de nombres de Kubernetes de un clúster. Si no se especifica ninguna opción, se muestran los servicios correspondientes al espacio de nombres predeterminado.
{: shortdesc}

```
ibmcloud ks cluster-services --cluster CLUSTER [--namespace KUBERNETES_NAMESPACE] [--all-namespaces] [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n
<em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Incluya los servicios que están vinculados a un determinado espacio de nombres de un clúster. Este valor es opcional.</dd>

   <dt><code>--all-namespaces</code></dt>
   <dd>Incluya los servicios que están vinculados a todos los espacios de nombres de un clúster. Este valor es opcional.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-services --cluster my_cluster --namespace my_namespace
  ```
  {: pre}


### ibmcloud ks va
{: #cs_va}

Después de [instalar el explorador de contenedores](/docs/services/va?topic=va-va_index#va_install_container_scanner), examinar un informe detallado de evaluación de vulnerabilidad para un contenedor del clúster.
{: shortdesc}

```
ibmcloud ks va --container CONTAINER_ID [--extended] [--vulnerabilities] [--configuration-issues] [--json]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de **Lector** de acceso al servicio para {{site.data.keyword.registrylong_notm}}. **Nota**: no asigne políticas para {{site.data.keyword.registryshort_notm}} a nivel de grupo de recursos.

**Opciones del mandato**:

<dl>
<dt><code>--container CONTAINER_ID</code></dt>
<dd><p>El ID del contenedor. Este valor es obligatorio.</p>
<p>Para buscar el ID del contenedor:<ol><li>[Defina el clúster como destino de la CLI de Kubernetes](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).</li><li>Obtenga una lista de sus pods con el mandato `kubectl get pods`.</li><li>Busque el campo **Container ID** en la información de salida del mandato `kubectl describe pod <pod_name>`. Por ejemplo, `Container ID: containerd://1a11a1aa2b2b22223333c44444ccc555667d7dd777888e8ef99f1011121314g15`.</li><li>Elimine el prefijo `containerd://` del ID antes de utilizar el ID de contenedor para el mandato `ibmcloud ks va`. Por ejemplo, `1a11a1aa2b2b22223333c44444ccc555667d7dd777888e8ef99f1011121314g15`.</li></ol></p></dd>

<dt><code>--extended</code></dt>
<dd><p>Ampliar la información de salida del mandato para mostrar más información sobre arreglos para los paquetes vulnerables. Este valor es opcional.</p>
<p>De forma predeterminada, los resultados de la exploración muestran el ID, el estado de la política, los paquetes afectados y la solución. Con el distintivo `--extended`, se añade información como el resumen, el aviso de seguridad del proveedor y un enlace con un aviso oficial.</p></dd>

<dt><code>--vulnerabilities</code></dt>
<dd>Restringir la información de salida del mandato para mostrar solo las vulnerabilidades del paquete. Este valor es opcional. No puede utilizar este distintivo si utiliza el distintivo `--configuration-issues`.</dd>

<dt><code>--configuration-issues</code></dt>
<dd>Restringir la información de salida del mandato para mostrar solo los problemas de configuración. Este valor es opcional. No puede utilizar este distintivo si utiliza el distintivo `--vulnerabilities`.</dd>

<dt><code>--json</code></dt>
<dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

```
ibmcloud ks va --container 1a11a1aa2b2b22223333c44444ccc555667d7dd777888e8ef99f1011121314g15 --extended --vulnerabilities --json
```
{: pre}

### ibmcloud ks key-protect-enable
{: #cs_key_protect}

Cifrar sus secretos de Kubernetes mediante [{{site.data.keyword.keymanagementservicefull}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/key-protect?topic=key-protect-getting-started-tutorial#getting-started-tutorial) como [proveedor de servicios de gestión de claves (KMS) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/kms-provider/) en su clúster. Para rotar una clave en un clúster con cifrado de claves existente, vuelva a ejecutar este mandato con un ID de clave raíz nuevo.
{: shortdesc}

```
ibmcloud ks key-protect-enable --cluster CLUSTER_NAME_OR_ID --key-protect-url ENDPOINT --key-protect-instance INSTANCE_GUID --crk ROOT_KEY_ID
```
{: pre}

No suprima las claves raíz de la instancia de {{site.data.keyword.keymanagementserviceshort}}. No suprima claves aunque rote claves para utilizar una clave nueva. No se puede acceder ni eliminar los datos en etcd ni los datos de los secretos del clúster si se suprime una clave raíz.
{: important}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

**Opciones del mandato**:

<dl>
<dt><code>--container CLUSTER_NAME_OR_ID</code></dt>
<dd>El nombre o ID del clúster.</dd>

<dt><code>--key-protect-url ENDPOINT</code></dt>
<dd>El punto final de {{site.data.keyword.keymanagementserviceshort}} para la instancia del clúster. Para obtener el punto final, consulte [puntos finales de servicio por región](/docs/services/key-protect?topic=key-protect-regions#service-endpoints).</dd>

<dt><code>--key-protect-instance INSTANCE_GUID</code></dt>
<dd>El GUID de la instancia de {{site.data.keyword.keymanagementserviceshort}}. Para obtener el GUID de la instancia, ejecute <code>ibmcloud resource service-instance SERVICE_INSTANCE_NAME --id</code> y copie el segundo valor (no el CRN completo).</dd>

<dt><code>--crk ROOT_KEY_ID</code></dt>
<dd>Su ID de clave raíz de {{site.data.keyword.keymanagementserviceshort}}. Para obtener la CRK, consulte [Visualización de claves](/docs/services/key-protect?topic=key-protect-view-keys#view-keys).</dd>
</dl>

**Ejemplo**:

```
ibmcloud ks key-protect-enable --cluster mycluster --key-protect-url keyprotect.us-south.bluemix.net --key-protect-instance a11aa11a-bbb2-3333-d444-e5e555e5ee5 --crk 1a111a1a-bb22-3c3c-4d44-55e555e55e55
```
{: pre}


### ibmcloud ks webhook-create
{: #cs_webhook_create}

Registrar un webhook.
{: shortdesc}

```
ibmcloud ks webhook-create --cluster CLUSTER --level LEVEL --type slack --url URL  [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd>El nivel de notificación, como por ejemplo <code>Normal</code> o <code>Warning</code>. <code>Warning</code> es el valor predeterminado. Este valor es opcional.</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>El tipo de webhook. Actualmente se admite slack. Este valor es obligatorio.</dd>

   <dt><code>--url <em>URL</em></code></dt>
   <dd>El URL del webhook. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks webhook-create --cluster my_cluster --level Normal --type slack --url http://github.com/mywebhook
  ```
  {: pre}


<br />


## Mandatos de clúster: Subredes
{: #cluster_subnets_commands}

### ibmcloud ks cluster-subnet-add
{: #cs_cluster_subnet_add}

Puede añadir subredes privadas o públicas portátiles existentes desde la cuenta de infraestructura de IBM Cloud (SoftLayer) a su clúster de Kubernetes o puede reutilizar subredes de un clúster suprimido en lugar de utilizar las subredes suministradas automáticamente.
{: shortdesc}

```
ibmcloud ks cluster-subnet-add --cluster CLUSTER --subnet-id SUBNET [-s]
```
{: pre}

<p class="important">Las direcciones IP públicas portátiles se facturan mensualmente. Si elimina direcciones IP públicas portátiles una vez suministrado el clúster, todavía tendrá que pagar el cargo mensual, aunque solo las haya utilizado un breve periodo de tiempo.</br>
</br>Cuando pone una subred a disponibilidad de un clúster, las direcciones IP de esta subred se utilizan para la gestión de redes del clúster. Para evitar conflictos de direcciones IP, asegúrese de utilizar una subred con un solo clúster. No utilice una subred para varios clústeres o para otros fines externos a {{site.data.keyword.containerlong_notm}} al mismo tiempo.</br>
</br>Para habilitar la comunicación entre nodos trabajadores que están en distintas subredes en la misma VLAN, debe [habilitar el direccionamiento entre subredes en la misma VLAN](/docs/containers?topic=containers-subnets#subnet-routing).</p>

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--subnet-id <em>SUBNET</em></code></dt>
   <dd>El ID de la subred. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-subnet-add --cluster my_cluster --subnet-id 1643389
  ```
  {: pre}


### ibmcloud ks cluster-subnet-create
{: #cs_cluster_subnet_create}

Crear una subred en una cuenta de infraestructura de IBM Cloud (SoftLayer) y ponerla a disponibilidad de un determinado clúster en {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks cluster-subnet-create --cluster CLUSTER --size SIZE --vlan VLAN_ID [-s]
```
{: pre}

<p class="important">Cuando pone una subred a disponibilidad de un clúster, las direcciones IP de esta subred se utilizan para la gestión de redes del clúster. Para evitar conflictos de direcciones IP, asegúrese de utilizar una subred con un solo clúster. No utilice una subred para varios clústeres o para otros fines externos a {{site.data.keyword.containerlong_notm}} al mismo tiempo.</br>
</br>Si tiene varias VLAN para un clúster, varias subredes en la misma VLAN o un clúster multizona, debe habilitar la [función de direccionador virtual (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para la cuenta de infraestructura de IBM Cloud (SoftLayer) para que los nodos trabajadores puedan comunicarse entre sí en la red privada. Para habilitar VRF, [póngase en contacto con el representante de su cuenta de la infraestructura de IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si no puede o no desea habilitar VRF, habilite la [distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para llevar a cabo esta acción, necesita el [permiso de la infraestructura](/docs/containers?topic=containers-users#infra_access) **Red > Gestionar distribución de VLAN de red** o bien puede solicitar al propietario de la cuenta que lo habilite. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.</p>

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio. Para obtener una lista de los clústeres, utilice el [mandato](#cs_clusters) `ibmcloud ks clusters`.</dd>

   <dt><code>--size <em>SIZE</em></code></dt>
   <dd>El número de direcciones IP de la subred. Este valor es obligatorio. Los valores posibles son 8, 16, 32 o 64.</dd>

   <dd>La VLAN en el que se va a crear la subred. Este valor es obligatorio. Para ver una lista de las VLAN disponibles, utilice el [mandato](#cs_vlans) `ibmcloud ks vlans <zone>`. La subred se suministra en la misma zona en la que se encuentra la VLAN.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-subnet-create --cluster my_cluster --size 8 --vlan 1764905
  ```
  {: pre}


### ibmcloud ks cluster-user-subnet-add
{: #cs_cluster_user_subnet_add}

Traer su propia subred privada a sus clústeres de {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks cluster-user-subnet-add --cluster CLUSTER --subnet-cidr SUBNET_CIDR --private-vlan PRIVATE_VLAN
```
{: pre}

Esta subred privada no es la que proporciona la infraestructura de IBM Cloud (SoftLayer). Por lo tanto, debe configurar el direccionamiento del tráfico de la red de entrada y de salida para la subred. Para añadir una subred de infraestructura de IBM Cloud (SoftLayer), utilice el [mandato](#cs_cluster_subnet_add) `ibmcloud ks cluster-subnet-add`.

<p class="important">Cuando pone una subred a disponibilidad de un clúster, las direcciones IP de esta subred se utilizan para la gestión de redes del clúster. Para evitar conflictos de direcciones IP, asegúrese de utilizar una subred con un solo clúster. No utilice una subred para varios clústeres o para otros fines externos a {{site.data.keyword.containerlong_notm}} al mismo tiempo.</br>
</br>Si tiene varias VLAN para un clúster, varias subredes en la misma VLAN o un clúster multizona, debe habilitar la [función de direccionador virtual (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para la cuenta de infraestructura de IBM Cloud (SoftLayer) para que los nodos trabajadores puedan comunicarse entre sí en la red privada. Para habilitar VRF, [póngase en contacto con el representante de su cuenta de la infraestructura de IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si no puede o no desea habilitar VRF, habilite la [distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para llevar a cabo esta acción, necesita el [permiso de la infraestructura](/docs/containers?topic=containers-users#infra_access) **Red > Gestionar distribución de VLAN de red** o bien puede solicitar al propietario de la cuenta que lo habilite. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.</p>

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--subnet-cidr <em>SUBNET_CIDR</em></code></dt>
   <dd>El Classless InterDomain Routing (CIDR) de la subred. Este valor es obligatorio y no debe estar en conflicto con ninguna subred que utilice la infraestructura de IBM Cloud (SoftLayer).

   Los prefijos válidos son los comprendidos entre `/30` (1 dirección IP) y `/24` (253 direcciones IP). Si establece el valor de CIDR en una longitud de prefijo y luego lo tiene que modificar, añada primero un nuevo CIDR y luego [elimine el CIDR antiguo](#cs_cluster_user_subnet_rm).</dd>

   <dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
   <dd>El ID de la VLAN privada. Este valor es obligatorio. Debe coincidir con el ID de VLAN privada de uno o varios nodos trabajadores del clúster.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-user-subnet-add --cluster my_cluster --subnet-cidr 169.xx.xxx.xxx/29 --private-vlan 1502175
  ```
  {: pre}


### ibmcloud ks cluster-user-subnet-rm
{: #cs_cluster_user_subnet_rm}

Eliminar su propia subred privada de un clúster especificado. Cualquier servicio que se haya desplegado en una dirección IP desde su propia subred privada permanece activo después de que se elimine la subred.
{: shortdesc}

```
ibmcloud ks cluster-user-subnet-rm --cluster CLUSTER --subnet-cidr SUBNET_CIDR --private-vlan PRIVATE_VLAN
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--subnet-cidr <em>SUBNET_CIDR</em></code></dt>
   <dd>El Classless InterDomain Routing (CIDR) de la subred. Este valor es obligatorio y debe coincidir con el CIDR especificado mediante el [mandato](#cs_cluster_user_subnet_add) `ibmcloud ks cluster-user-subnet-add`.</dd>

   <dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
   <dd>El ID de la VLAN privada. Este valor es obligatorio y debe coincidir con la VLAN especificada mediante el [mandato](#cs_cluster_user_subnet_add) `ibmcloud ks cluster-user-subnet-add`.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks cluster-user-subnet-rm --cluster my_cluster --subnet-cidr 169.xx.xxx.xxx/29 --private-vlan 1502175
  ```
  {: pre}


### ibmcloud ks subnets
{: #cs_subnets}

Ver una lista de subredes que están disponibles en una cuenta de infraestructura de IBM Cloud (SoftLayer).
{: shortdesc}

```
ibmcloud ks subnets [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplo**:

  ```
  ibmcloud ks subnets
  ```
  {: pre}


<br />


## Mandatos del equilibrador de carga de aplicación (ALB) de Ingress
{: #alb_commands}

### ibmcloud ks alb-autoupdate-disable
{: #cs_alb_autoupdate_disable}

De forma predeterminada, las actualizaciones automáticas del complemento de equilibrador de carga de aplicación (ALB) de Ingress están habilitadas. Los pods de ALB se actualizan automáticamente cuando hay una nueva versión de compilación disponible. Para actualizar el complemento manualmente en su lugar, utilice este mandato para inhabilitar las actualizaciones automáticas. A continuación, actualice los pods de ALB ejecutando el mandato
[`ibmcloud ks alb-update`](#cs_alb_update).
{: shortdesc}

```
ibmcloud ks alb-autoupdate-disable --cluster CLUSTER
```
{: pre}

Cuando actualice la versión principal o menor de Kubernetes del clúster, IBM realizará automáticamente los cambios necesarios en el despliegue de Ingress, pero no cambiará la versión de compilación del complemento ALB de Ingress. Deberá comprobar la compatibilidad de las versiones más recientes de Kubernetes y las imágenes del complemento ALB de Ingress.

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Ejemplo**:

```
ibmcloud ks alb-autoupdate-disable --cluster mycluster
```
{: pre}

### ibmcloud ks alb-autoupdate-enable
{: #cs_alb_autoupdate_enable}

Si las actualizaciones automáticas para el complemento ALB de Ingress están inhabilitadas, puede volver a habilitar las actualizaciones automáticas. Siempre que la siguiente compilación esté disponible, los ALB se actualizan automáticamente a la compilación más reciente.
{: shortdesc}

```
ibmcloud ks alb-autoupdate-enable --cluster CLUSTER
```
{: pre}

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Ejemplo**:

```
ibmcloud ks alb-autoupdate-enable --cluster mycluster
```
{: pre}

### ibmcloud ks alb-autoupdate-get
{: #cs_alb_autoupdate_get}

Comprobar si las actualizaciones automáticas para el complemento ALB de Ingress están habilitadas y si los ALB están actualizados a la versión de compilación más reciente.
{: shortdesc}

```
ibmcloud ks alb-autoupdate-get --cluster CLUSTER
```
{: pre}

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Ejemplo**:

```
ibmcloud ks alb-autoupdate-get --cluster mycluster
```
{: pre}

### ibmcloud ks alb-cert-deploy
{: #cs_alb_cert_deploy}

Desplegar o actualizar un certificado de la instancia de {{site.data.keyword.cloudcerts_long_notm}} al ALB de un clúster. Sólo puede actualizar certificados que importados de la misma instancia de {{site.data.keyword.cloudcerts_long_notm}}.
{: shortdesc}

```
ibmcloud ks alb-cert-deploy [--update] --cluster CLUSTER --secret-name SECRET_NAME --cert-crn CERTIFICATE_CRN [--update] [-s]
```
{: pre}

Cuando importa un certificado con este mandato, el secreto de certificado se crea en un espacio de nombres denominado `ibm-cert-store`. A continuación, se crea una referencia a este secreto en el espacio de nombres `default`, al que puede acceder cualquier recurso de Ingress en cualquier espacio de nombres. Cuando el ALB está procesando solicitudes, sigue esta referencia para seleccionar y utilizar el secreto de certificado del espacio de nombres `ibm-cert-store`.

Para permanecer dentro de los [límites de velocidad](https://cloud.ibm.com/apidocs/certificate-manager#rate-limiting) establecido por {{site.data.keyword.cloudcerts_short}}, espere al menos 45 segundos entre los mandatos `alb-cert-deploy` y `alb-cert-deploy --update` sucesivos.
{: note}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--update</code></dt>
   <dd>Actualiza el certificado para un secreto del ALB en un clúster. Este valor es opcional.</dd>

   <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
   <dd>Especifique un nombre para el secreto de ALB cuando se cree en el clúster. Este valor es obligatorio. Asegúrese de que no crea el secreto con el mismo nombre que el secreto de Ingress proporcionado por IBM. Puede obtener el nombre del secreto de Ingress proporcionado por IBM ejecutando el mandato <code>ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress</code>.</dd>

   <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
   <dd>El CRN de certificado. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplos**:

Ejemplo para desplegar un secreto de ALB:

   ```
   ibmcloud ks alb-cert-deploy --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
   ```
   {: pre}

Ejemplo para actualizar un secreto de ALB existente:

 ```
 ibmcloud ks alb-cert-deploy --update --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:7e21fde8ee84a96d29240327daee3eb2
 ```
 {: pre}


### ibmcloud ks alb-cert-get
{: #cs_alb_cert_get}

Si ha importado un certificado de {{site.data.keyword.cloudcerts_short}} en el ALB en un clúster, visualice información sobre el certificado TLS, como los secretos que asociados con el mismo.
{: shortdesc}

```
ibmcloud ks alb-cert-get --cluster CLUSTER [--secret-name SECRET_NAME] [--cert-crn CERTIFICATE_CRN] [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>El nombre del secreto de ALB. Este valor es necesario para obtener información sobre un secreto de ALB específico en el clúster.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>El CRN de certificado. Este valor es necesario para obtener información sobre todos los secretos de ALB coincidentes con un CRN de certificado determinado en el clúster.</dd>

  <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplos**:

 Ejemplo para captar un secreto de ALB:

 ```
 ibmcloud ks alb-cert-get --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 Ejemplo para obtener información sobre todos los secretos de ALB que coinciden con un CRN de certificado especificado:

 ```
 ibmcloud ks alb-cert-get --cluster my_cluster --cert-crn  crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### ibmcloud ks alb-cert-rm
{: #cs_alb_cert_rm}

Si ha importado un certificado de {{site.data.keyword.cloudcerts_short}} en el ALB en un clúster, elimine el secreto del clúster.
{: shortdesc}

```
ibmcloud ks alb-cert-rm --cluster CLUSTER [--secret-name SECRET_NAME] [--cert-crn CERTIFICATE_CRN] [-s]
```
{: pre}

Para permanecer dentro de los [límites de velocidad](https://cloud.ibm.com/apidocs/certificate-manager#rate-limiting) establecido por {{site.data.keyword.cloudcerts_short}}, espere al menos 45 segundos entre mandatos `alb-cert-rm` sucesivos.
{: note}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>El nombre del secreto de ALB. Este valor es necesario para eliminar un secreto de ALB específico en el clúster.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>El CRN de certificado. Este valor es necesario para eliminar todos los secretos de ALB coincidentes con un CRN de certificado determinado en el clúster.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

  </dl>

**Ejemplos**:

 Ejemplo para eliminar un secreto de ALB:

 ```
 ibmcloud ks alb-cert-rm --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 Ejemplo para eliminar todos los secretos de ALB que coinciden con un CRN de certificado especificado:

 ```
 ibmcloud ks alb-cert-rm --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### ibmcloud ks alb-certs
{: #cs_alb_certs}

Obtener una lista de los certificados que ha importado de la instancia de {{site.data.keyword.cloudcerts_long_notm}} en los ALB de un clúster.
{: shortdesc}

```
ibmcloud ks alb-certs --cluster CLUSTER [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>
   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>
   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

 ```
 ibmcloud ks alb-certs --cluster my_cluster
 ```
 {: pre}

### ibmcloud ks alb-configure
{: #cs_alb_configure}

Habilitar o inhabilitar un ALB en el clúster estándar. El ALB público está habilitado de forma predeterminada.
{: shortdesc}

```
ibmcloud ks alb-configure --albID ALB_ID [--enable] [--user-ip USER_IP] [--disable] [--disable-deployment] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Opciones del mandato**:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>El ID de un ALB. Ejecute <code>ibmcloud ks albs <em>--cluster </em>CLUSTER</code> para ver los ID de los ALB de un clúster. Este valor es obligatorio.</dd>

   <dt><code>--enable</code></dt>
   <dd>Incluya este distintivo para habilitar un ALB en un clúster.</dd>

   <dt><code>--disable</code></dt>
   <dd>Incluya este distintivo para inhabilitar un ALB en un clúster.</dd>

   <dt><code>--disable-deployment</code></dt>
   <dd>Incluya este distintivo para inhabilitar el despliegue de ALB proporcionado por IBM. Este distintivo no elimina el registro de DNS para el subdominio de Ingress proporcionado por IBM o el servicio del equilibrador de carga que se utiliza para exponer el controlador Ingress.</dd>

   <dt><code>--user-ip <em>USER_IP</em></code></dt>
   <dd><ul>
     <li>Este parámetro está disponible para habilitar un ALB privado únicamente.</li>
     <li>El ALB privado se despliega con una dirección IP de una subred privada proporcionada por un usuario. Si no se proporciona la dirección IP, el ALB se despliega con una dirección IP privada de la subred privada portátil que se suministra automáticamente cuando se crea el clúster.</li>
    </ul></dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplos**:

  Ejemplo para habilitar un ALB:

  ```
  ibmcloud ks alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable
  ```
  {: pre}

  Ejemplo para habilitar un ALB con una dirección IP proporcionada por un usuario:

  ```
  ibmcloud ks alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable --user-ip user_ip
  ```
  {: pre}

  Ejemplo para inhabilitar un ALB:

  ```
  ibmcloud ks alb-configure --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --disable
  ```
  {: pre}


### ibmcloud ks alb-get
{: #cs_alb_get}

Visualizar los detalles de un ALB.
{: shortdesc}

```
ibmcloud ks alb-get --albID ALB_ID [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>El ID de un ALB. Ejecute <code>ibmcloud ks albs --cluster <em>CLUSTER</em></code> para ver los ID de los ALB de un clúster. Este valor es obligatorio.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  ibmcloud ks alb-get --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1
  ```
  {: pre}

### ibmcloud ks alb-rollback
{: #cs_alb_rollback}

Si los pods de ALB se han actualizado recientemente, pero una configuración personalizada de los ALB se ve afectada por la última compilación, puede retrotraer la actualización a la versión de compilación que ejecutaban los pods de ALB anteriormente. Después de retrotraer una actualización, se inhabilitan las actualizaciones automáticas para los pods de ALB. Para volver a habilitar las actualizaciones automáticas, utilice el [mandato `alb-autoupdate-enable`](#cs_alb_autoupdate_enable).
{: shortdesc}

```
ibmcloud ks alb-rollback --cluster CLUSTER
```
{: pre}

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Ejemplo**:

```
ibmcloud ks alb-rollback --cluster mycluster
```
{: pre}

### ibmcloud ks alb-types
{: #cs_alb_types}

Visualizar los tipos de ALB que están soportados en la región.
{: shortdesc}

```
ibmcloud ks alb-types [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplo**:

  ```
  ibmcloud ks alb-types
  ```
  {: pre}

### ibmcloud ks alb-update
{: #cs_alb_update}

Si las actualizaciones automáticas para el complemento ALB de Ingress están inhabilitadas y desea actualizar el complemento, puede forzar una actualización de una sola vez de los pods de ALB. Al elegir actualizar el complemento de forma manual, todos los pods de ALB del clúster se actualizan a la versión de compilación más reciente. No puede actualizar un ALB individual ni elegir a qué versión de compilación actualizar el complemento. Las actualizaciones automáticas siguen estando inhabilitadas.
{: shortdesc}

```
ibmcloud ks alb-update --cluster CLUSTER
```
{: pre}

Cuando actualice la versión principal o menor de Kubernetes del clúster, IBM realizará automáticamente los cambios necesarios en el despliegue de Ingress, pero no cambiará la versión de compilación del complemento ALB de Ingress. Deberá comprobar la compatibilidad de las versiones más recientes de Kubernetes y las imágenes del complemento ALB de Ingress.

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Ejemplo**:

```
ibmcloud ks alb-update --cluster <cluster_name_or_ID>
```
{: pre}

### ibmcloud ks albs
{: #cs_albs}

Visualizar el estado de todos los ALB de un clúster. Si no se devuelve ningún ID de ALB, significa que el clúster no tiene subred portátil. Puede [crear](#cs_cluster_subnet_create) o [añadir](#cs_cluster_subnet_add) subredes a un clúster.
{: shortdesc}

```
ibmcloud ks albs --cluster CLUSTER [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>--cluster </em>CLUSTER</code></dt>
   <dd>El nombre o ID del clúster en el que se listan los ALB disponibles. Este valor es obligatorio.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  ibmcloud ks albs --cluster my_cluster
  ```
  {: pre}


<br />



## Mandatos de infraestructura
{: #infrastructure_commands}

### ibmcloud ks credential-get
{: #cs_credential_get}

Si configura la cuenta de IBM Cloud para que utilice credenciales distintas para acceder al portafolio de la infraestructura de IBM Cloud, obtenga el nombre de usuario de infraestructura para la región y grupo de recursos a los que esté destinado actualmente.
{: shortdesc}

```
ibmcloud ks credential-get [-s] [--json]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

 <dl>
 <dt><code>--json</code></dt>
 <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>
 <dt><code>-s</code></dt>
 <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
 </dl>

**Ejemplo:**
```
ibmcloud ks credential-get
```
{: pre}

**Salida de ejemplo:**
```
Infrastructure credentials for user name user@email.com set for resource group default.
```

### ibmcloud ks credential-set
{: #cs_credentials_set}

Definir las credenciales de cuenta de infraestructura de IBM Cloud (SoftLayer) para un grupo de recursos y región de {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks credential-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME [-s]
```
{: pre}

Si tiene una cuenta de pago según uso de {{site.data.keyword.Bluemix_notm}}, tiene acceso al portafolio de infraestructura de IBM Cloud (de SoftLayer) de forma predeterminada. Sin embargo, es posible que desee utilizar otra cuenta de infraestructura de IBM Cloud (SoftLayer) que ya tenga para solicitar infraestructura. Puede enlazar esta cuenta de infraestructura a la cuenta de {{site.data.keyword.Bluemix_notm}} mediante este mandato.

Si las credenciales de infraestructura de IBM Cloud (SoftLayer) se establecen manualmente para un grupo de recursos y una región, estas credenciales se utilizan para pedir infraestructura para todos los clústeres dentro de dicha región en el grupo de recursos. Estas credenciales se utilizan para determinar permisos de infraestructura, incluso si ya existe una [clave de API de {{site.data.keyword.Bluemix_notm}} IAM](#cs_api_key_info) para el grupo de recursos y la región. Si el usuario cuyas credenciales se almacenan no tiene los permisos necesarios para pedir infraestructura, entonces las acciones relacionadas con la infraestructura, como crear un clúster o recargar un nodo trabajador, pueden fallar.

No se pueden establecer varias credenciales para el mismo grupo de recursos y región de {{site.data.keyword.containerlong_notm}}.

Antes de utilizar este mandato, asegúrese de que el usuario cuyas credenciales se utilizan tiene los [permisos de infraestructura de IBM Cloud (SoftLayer) y de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-users#users) necesarios.
{: important}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>Nombre usuario de la API de la cuenta de infraestructura de IBM Cloud (SoftLayer). Este valor es obligatorio. Tenga en cuenta que el nombre de usuario de la API de la infraestructura no es el mismo que el IBMid. Para ver el nombre de usuario de la API de la infraestructura:
   <ol>
   <li>Inicie una sesión en la [consola de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com).
   <li>En la barra de menús, seleccione **Gestionar > Acceso (IAM)**.
   <li>Seleccione el separador **Usuarios** y pulse el nombre de usuario.
   <li>En el panel **Claves de API**, busque la entrada **Clave de API de infraestructura clásica**
y pulse el **Menú de acción** ![Icono de menú de acción](../icons/action-menu-icon.svg "Icono de menú de acción") **> Detalles**.
   <li>Copie el nombre de usuario de la API.
   </ol></dd>


   <dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
   <dd>Clave de API de la cuenta de infraestructura de IBM Cloud (SoftLayer). Este valor es obligatorio. Para ver o generar una clave de API de infraestructura:
  <li>Inicie una sesión en la [consola de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com).
  <li>En la barra de menús, seleccione **Gestionar > Acceso (IAM)**.
  <li>Seleccione el separador **Usuarios** y pulse el nombre de usuario.
  <li>En el panel **Claves de API**, busque la entrada **Clave de API de infraestructura clásica**
y pulse el **Menú de acción** ![Icono de menú de acción](../icons/action-menu-icon.svg "Icono de menú de acción") **> Detalles**. Si no ve una clave de API de infraestructura clásica, genere una pulsando **Crear una clave de API de {{site.data.keyword.Bluemix_notm}}**.
  <li>Copie el nombre de usuario de la API.
  </ol>
  </dd>
  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

  </dl>

**Ejemplo**:

  ```
  ibmcloud ks credential-set --infrastructure-api-key <api_key> --infrastructure-username dbmanager
  ```
  {: pre}


### ibmcloud ks credential-unset
{: #cs_credentials_unset}

Eliminar las credenciales de cuenta de infraestructura de IBM Cloud (SoftLayer) de una región de {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Una vez eliminadas las credenciales, la [clave de API de {{site.data.keyword.Bluemix_notm}} IAM](#cs_api_key_info) se utiliza para pedir recursos en la infraestructura de IBM Cloud (SoftLayer).

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

```
ibmcloud ks credential-unset [-s]
```
{: pre}

<strong>Opciones del mandato</strong>:

  <dl>
  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>


### ibmcloud ks machine-types
{: #cs_machine_types}

Ver una lista de los tipos de máquinas disponibles para sus nodos trabajadores. Los tipos de máquina varían por zona. Cada tipo de máquina incluye cantidad de CPU virtual, memoria y espacio de disco para cada nodo trabajador del clúster. De forma predeterminada, el directorio de disco de almacenamiento secundario en el que se almacenan todos los datos de contenedor está cifrado con cifrado de LUKS AES de 256 bits. Si la opción `disable-disk-encrypt` se incluye durante la creación de clústeres, los datos de tiempo de ejecución de contenedor del host no se cifrarán. [Más información sobre el cifrado](/docs/containers?topic=containers-security#encrypted_disk).
{:shortdesc}

```
ibmcloud ks machine-types --zone ZONE [--json] [-s]
```
{: pre}

Puede suministrar el nodo trabajador como una máquina virtual en hardware dedicado o compartido, o como una máquina física en un servidor nativo. [Más información sobre las opciones de tipo de máquina](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node).

<strong>Permisos mínimos necesarios</strong>: ninguno

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--zone <em>ZONE</em></code></dt>
   <dd>Especifique la zona de la que desea ver una lista de tipos de máquina disponibles. Este valor es obligatorio. Consulte las [zonas disponibles](/docs/containers?topic=containers-regions-and-zones#zones).</dd>

   <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplo**:

  ```
  ibmcloud ks machine-types --zone dal10
  ```
  {: pre}



### ibmcloud ks <ph class="ignoreSpelling">vlans</ph>
{: #cs_vlans}

Crear una lista de las VLAN públicas y privadas disponibles para una zona en la cuenta de infraestructura de IBM Cloud (SoftLayer). Para ver una lista de las VLAN disponibles,
debe tener una cuenta de pago.
{: shortdesc}

```
ibmcloud ks vlans --zone ZONE [--all] [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>:
* Para ver las VLAN a las que está conectado el clúster en una zona: rol de plataforma **Visor** para el clúster en
{{site.data.keyword.containerlong_notm}}
* Para obtener una lista de todas las VLAN disponibles en una zona: rol de plataforma **Visor** para la región en
{{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--zone <em>ZONE</em></code></dt>
   <dd>Escriba la zona donde desea listar sus VLAN públicas y privadas. Este valor es obligatorio. Consulte las [zonas disponibles](/docs/containers?topic=containers-regions-and-zones#zones).</dd>

   <dt><code>--all</code></dt>
   <dd>Obtenga una lista de todas las VLAN disponibles. De forma predeterminada, las VLAN se filtran para mostrar solo las VLAN que son válidas. Para ser válida, una VLAN debe estar asociada con infraestructura que pueda alojar un trabajador con almacenamiento en disco local.</dd>

   <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks vlans --zone dal10
  ```
  {: pre}


### ibmcloud ks vlan-spanning-get
{: #cs_vlan_spanning_get}

Ver el estado de distribución de VLAN para una cuenta de infraestructura de IBM Cloud (SoftLayer). La distribución de VLAN permite que todos los dispositivos de una cuenta se comuniquen entre sí mediante la red privada, independientemente de la VLAN asignada.
{: shortdesc}

```
ibmcloud ks vlan-spanning-get [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
    <dt><code>--json</code></dt>
      <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

    <dt><code>-s</code></dt>
      <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks vlan-spanning-get
  ```
  {: pre}

<br />


## Mandatos de registro
{: #logging_commands}

### ibmcloud ks logging-autoupdate-disable
{: #cs_log_autoupdate_disable}

Inhabilitar las actualizaciones automáticas de los pods de Fluentd en un clúster específico. Cuando actualice la versión principal o menor de Kubernetes del clúster, IBM realizará automáticamente los cambios necesarios en el mapa de configuración de Fluentd, pero no cambiará la versión de compilación del complemento Fluentd para registro. Deberá comprobar la compatibilidad de las versiones más recientes de Kubernetes y las imágenes del complemento.
{: shortdesc}

```
ibmcloud ks logging-autoupdate-disable --cluster CLUSTER
```
{: pre}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster donde desea inhabilitar las actualizaciones automáticas del complemento Fluentd. Este valor es obligatorio.</dd>
</dl>

### ibmcloud ks logging-autoupdate-enable
{: #cs_log_autoupdate_enable}

Habilitar las actualizaciones automáticas para los pods de Fluentd en un clúster específico. Los pods de Fluentd se actualizan automáticamente cuando hay una nueva versión de compilación disponible.
{: shortdesc}

```
ibmcloud ks logging-autoupdate-enable --cluster CLUSTER
```
{: pre}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster donde desea habilitar las actualizaciones automáticas del complemento Fluentd. Este valor es obligatorio.</dd>
</dl>

### ibmcloud ks logging-autoupdate-get
{: #cs_log_autoupdate_get}

Comprobar si los pods de Fluentd están establecidos para que se actualicen automáticamente en un clúster específico.
{: shortdesc}

```
ibmcloud ks logging-autoupdate-get --cluster CLUSTER
```
{: pre}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster donde desea comprobar si las actualizaciones automáticas del complemento Fluentd están habilitadas. Este valor es obligatorio.</dd>
</dl>

### ibmcloud ks logging-config-create
{: #cs_logging_create}

Crear una configuración de registro. Puede utilizar este mandato para reenviar los registros correspondientes a contenedores, aplicaciones, nodos trabajadores, clústeres de Kubernetes y equilibradores de carga de aplicación de Ingress a {{site.data.keyword.loganalysisshort_notm}} o a un servidor syslog externo.
{: shortdesc}

```
ibmcloud ks logging-config-create --cluster CLUSTER --logsource LOG_SOURCE --type LOG_TYPE [--namespace KUBERNETES_NAMESPACE] [--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT] [--space CLUSTER_SPACE] [--org CLUSTER_ORG] [--app-containers CONTAINERS] [--app-paths PATHS_TO_LOGS] [--syslog-protocol PROTOCOL]  [--json] [--skip-validation] [--force-update][-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Editor** para el clúster para todos los orígenes de registro excepto `kube-audit` y rol de plataforma **Administrador** para el clúster para el origen de registro `kube-audit`

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster.</dd>

  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>El origen del registro para habilitar su reenvío. Este argumento da soporte a una lista separada por comas de orígenes de registro a los que aplicar la configuración. Los valores aceptados son <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>storage</code>, <code>ingress</code> y <code>kube-audit</code>. Si no especifica un origen de registro, las configuraciones se crean para los <code>container</code> e <code>ingress</code>.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>El destino al que desea reenviar los registros. Las opciones son <code>ibm</code>, que reenvía los registros a {{site.data.keyword.loganalysisshort_notm}} y <code>syslog</code>, que reenvía los registros a un servidor externo.</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>El espacio de nombres de Kubernetes desde el que desea reenviar registros. El reenvío de registros no recibe soporte para los espacios de nombres de Kubernetes <code>ibm-system</code> y <code>kube-system</code>. Este valor sólo es válido para el origen de registro de contenedor y es opcional. Si no especifica un espacio de nombres, todos los espacios de nombres del clúster utilizarán esta configuración.</dd>

  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
    <dd>Cuando el tipo de registro es <code>syslog</code>, el nombre de host o la dirección IP del servidor del recopilador de registro. Este valor es obligatorio para <code>syslog</code>. Cuando el tipo de registro es <code>ibm</code>, el URL de ingesta de {{site.data.keyword.loganalysislong_notm}}. Puede encontrar la lista de URL de ingesta disponibles [aquí](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_ingestion#log_ingestion_urls). Si no especifica un URL de ingesta, se utiliza el punto final de la región en la que se ha creado el clúster.</dd>

  <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
    <dd>El puerto del servidor del recopilador de registro. Este valor es opcional. Si no especifica un puerto, se utiliza el puerto estándar <code>514</code> para <code>syslog</code> y el puerto estándar <code>9091</code> para <code>ibm</code>.</dd>

  <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
    <dd>Opcional: el nombre del espacio de Cloud Foundry al que desea enviar registros. Este valor sólo es válido para el registro de tipo <code>ibm</code> y es opcional. Si no especifica un espacio, los registros se envían al nivel de cuenta. Si lo especifica, también debe especificar una org.</dd>

  <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
    <dd>Opcional: el nombre de la organización de Cloud Foundry en la que está el espacio. Este valor sólo es válido para el registro de tipo <code>ibm</code> y es necesario si ha especificado un espacio.</dd>

  <dt><code>--app-paths</code></dt>
    <dd>La vía de acceso al contenedor donde las apps realizan registros. Para reenviar registros con el tipo de origen <code>application</code>, debe proporcionar una vía de acceso. Para especificar más de una vía de acceso, utilice una lista separada por comas. Este valor es necesario para el origen de registro <code>application</code>. Ejemplo: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

  <dt><code>--syslog-protocol</code></dt>
    <dd>El protocolo de la capa de transferencia que se utiliza cuando el tipo de registro es <code>syslog</code>. Los valores admitidos son <code>tcp</code>,
<code>tls </code> y el valor predeterminado <code>udp</code>. Al reenviar a un servidor syslog con el protocolo <code>udp</code>, se truncan los registros por encima de 1 KB.</dd>

  <dt><code>--app-containers</code></dt>
    <dd>Para reenviar registros de apps, puede especificar el nombre del contenedor que contiene la app. Puede especificar más de un contenedor mediante una lista separada por comas. Si no se especifica ningún contenedor, se reenvían los registros de todos los contenedores que contienen las vías de acceso indicadas. Esta opción únicamente es válida para el origen de registro <code>application</code>.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>--skip-validation</code></dt>
    <dd>Omite la validación de los nombres de espacio y organización cuando se especifican. Al omitir la validación, disminuye el tiempo de proceso, pero si la configuración de registro no es válida, los registros no se reenviarán correctamente. Este valor es opcional.</dd>

  <dt><code>--force-update</code></dt>
    <dd>Forzar la actualización de sus pods de Fluentd a la versión más reciente. Fluentd debe estar a la versión más reciente para poder realizar cambios en las configuraciones de registro.</dd>

    <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplos**:

Ejemplo de registro de tipo `ibm` que reenvía desde un origen de registro de `contenedor` en el puerto predeterminado 9091:

  ```
  ibmcloud ks logging-config-create my_cluster --logsource container --namespace my_namespace --hostname ingest.logging.ng.bluemix.net --type ibm
  ```
  {: pre}

Ejemplo de registro de tipo `syslog` que reenvía desde un origen de registro de `contenedor` en el puerto predeterminado 514:

  ```
  ibmcloud ks logging-config-create my_cluster --logsource container --namespace my_namespace  --hostname 169.xx.xxx.xxx --type syslog
  ```
  {: pre}

Ejemplo de registro de tipo `syslog` que reenvía registros desde un origen de `ingress` en un puerto distinto al predeterminado:

  ```
  ibmcloud ks logging-config-create --cluster my_cluster --logsource container --hostname 169.xx.xxx.xxx --port 5514 --type syslog
  ```
  {: pre}


### ibmcloud ks logging-config-get
{: #cs_logging_get}

Ver todas las configuraciones de reenvío de registro para un clúster, o filtre las configuraciones de registro en función del origen del registro.
{: shortdesc}

```
ibmcloud ks logging-config-get --cluster CLUSTER [--logsource LOG_SOURCE] [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

 <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>El tipo de origen de registro que desea filtrar. Sólo se devolverán las configuraciones de registro de este origen de registro en el clúster. Los valores aceptados son <code>container</code>, <code>storage</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>ingress</code> y <code>kube-audit</code>. Este valor es opcional.</dd>

  <dt><code>--show-covering-filters</code></dt>
    <dd>Muestra los filtros de registro que hicieron los filtros anteriores obsoletos.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
 </dl>

**Ejemplo**:

  ```
  ibmcloud ks logging-config-get --cluster my_cluster --logsource worker
  ```
  {: pre}


### ibmcloud ks logging-config-refresh
{: #cs_logging_refresh}

Renovar la configuración de registro para el clúster. Renueva la señal de registro para cualquier configuración de registro que se esté reenviando al nivel de espacio en el clúster.
{: shortdesc}

```
ibmcloud ks logging-config-refresh --cluster CLUSTER [--force-update] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--force-update</code></dt>
     <dd>Forzar la actualización de sus pods de Fluentd a la versión más reciente. Fluentd debe estar a la versión más reciente para poder realizar cambios en las configuraciones de registro.</dd>

   <dt><code>-s</code></dt>
     <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks logging-config-refresh --cluster my_cluster
  ```
  {: pre}



### ibmcloud ks logging-config-rm
{: #cs_logging_rm}

Suprimir una configuración de reenvío de registros o todas las configuraciones de registro de un clúster. Esto detiene el reenvío del registro a un servidor syslog remoto o a {{site.data.keyword.loganalysisshort_notm}}.
{: shortdesc}

```
ibmcloud ks logging-config-rm --cluster CLUSTER [--id LOG_CONFIG_ID] [--all] [--force-update] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Editor** para el clúster para todos los orígenes de registro excepto `kube-audit` y rol de plataforma **Administrador** para el clúster para el origen de registro `kube-audit`

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>Si desea eliminar una única configuración de registro, el ID de la configuración de registro.</dd>

  <dt><code>--all</code></dt>
   <dd>El distintivo para eliminar todas las configuraciones de registro de un clúster.</dd>

  <dt><code>--force-update</code></dt>
    <dd>Forzar la actualización de sus pods de Fluentd a la versión más reciente. Fluentd debe estar a la versión más reciente para poder realizar cambios en las configuraciones de registro.</dd>

   <dt><code>-s</code></dt>
     <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks logging-config-rm --cluster my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e
  ```
  {: pre}


### ibmcloud ks logging-config-update
{: #cs_logging_update}

Actualizar los detalles de una configuración de reenvío de registros.
{: shortdesc}

```
ibmcloud ks logging-config-update --cluster CLUSTER --id LOG_CONFIG_ID --type LOG_TYPE  [--namespace NAMESPACE] [--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT] [--space CLUSTER_SPACE] [--org CLUSTER_ORG] [--app-paths PATH] [--app-containers PATH] [--json] [--skipValidation] [--force-update] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>El ID de configuración de registro que desea actualizar. Este valor es obligatorio.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
   <dd>El protocolo de reenvío de registros que desea utilizar. Actualmente se da soporte a <code>syslog</code> e <code>ibm</code>. Este valor es obligatorio.</dd>

  <dt><code>--namespace <em>NAMESPACE</em></code>
    <dd>El espacio de nombres de Kubernetes desde el que desea reenviar registros. El reenvío de registros no recibe soporte para los espacios de nombres de Kubernetes <code>ibm-system</code> y <code>kube-system</code>. Este valor sólo es válido para el origen de registro <code>container</code>. Si no especifica un espacio de nombres, todos los espacios de nombres del clúster utilizarán esta configuración.</dd>

  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
   <dd>Cuando el tipo de registro es <code>syslog</code>, el nombre de host o dirección IP del servidor del recopilador de registro. Este valor es obligatorio para <code>syslog</code>. Cuando el tipo de registro es <code>ibm</code>, el URL de ingesta de {{site.data.keyword.loganalysislong_notm}}. Puede encontrar la lista de URL de ingesta disponibles [aquí](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_ingestion#log_ingestion_urls). Si no especifica un URL de ingesta, se utiliza el punto final de la región en la que se ha creado el clúster.</dd>

   <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
   <dd>El puerto del servidor del recopilador de registro. Este valor es opcional cuando el tipo de registro es <code>syslog</code>. Si no especifica un puerto, se utiliza el puerto estándar <code>514</code> para <code>syslog</code> y el <code>9091</code> para <code>ibm</code>.</dd>

   <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
   <dd>Opcional: el nombre del espacio al que desea enviar registros. Este valor sólo es válido para el registro de tipo <code>ibm</code> y es opcional. Si no especifica un espacio, los registros se envían al nivel de cuenta. Si lo especifica, también debe especificar una org.</dd>

   <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
   <dd>Opcional: el nombre de la organización de Cloud Foundry en la que está el espacio. Este valor sólo es válido para el registro de tipo <code>ibm</code> y es necesario si ha especificado un espacio.</dd>

   <dt><code>--app-paths <em>PATH</em>,<em>PATH</em></code></dt>
     <dd>Vía de acceso de archivo absoluta en el contenedor del que recopilar los registros. Es posible utilizar comodines como, por ejemplo, '/var/log/*.log', sin embargo no es posible utilizar expresiones recursivas como, por ejemplo '/var/log/**/test.log'. Para especificar más de una vía de acceso, utilice una lista separada por comas. Este valor es obligatorio cuando se especifica 'application' como el origen de registro. </dd>

   <dt><code>--app-containers <em>PATH</em>,<em>PATH</em></code></dt>
     <dd>La vía de acceso en los contenedores en los que las apps están creando registros. Para reenviar registros con el tipo de origen <code>application</code>, debe proporcionar una vía de acceso. Para especificar más de una vía de acceso, utilice una lista separada por comas. Ejemplo: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

   <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code>--skipValidation</code></dt>
    <dd>Omite la validación de los nombres de espacio y organización cuando se especifican. Al omitir la validación, disminuye el tiempo de proceso, pero si la configuración de registro no es válida, los registros no se reenviarán correctamente. Este valor es opcional.</dd>

  <dt><code>--force-update</code></dt>
    <dd>Forzar la actualización de sus pods de Fluentd a la versión más reciente. Fluentd debe estar a la versión más reciente para poder realizar cambios en las configuraciones de registro.</dd>

   <dt><code>-s</code></dt>
     <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplo de registro de tipo `ibm`**:

  ```
  ibmcloud ks logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**Ejemplo de registro de tipo `syslog`**:

  ```
  ibmcloud ks logging-config-update --cluster my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}



### ibmcloud ks logging-filter-create
{: #cs_log_filter_create}

Crear un filtro de registro. Utilice este mandato para filtrar los registros que su configuración de registro reenvía.
{: shortdesc}

```
ibmcloud ks logging-filter-create --cluster CLUSTER --type LOG_TYPE [--logging-configs CONFIGS] [--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME] [--level LOGGING_LEVEL] [--message MESSAGE] [--regex-message MESSAGE] [--force-update] [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>Nombre o ID de clúster que desea para crear un filtro de registro. Este valor es obligatorio.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>Tipo de registro al que aplicar el filtro. Actualmente se da soporte a <code>all</code>, <code>container</code> y <code>host</code>.</dd>

  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>Una lista separada por comas de los ID de configuración de registro. Si no se proporciona, el filtro se aplica a todas las configuraciones de registro de clúster que se pasan al filtro. Puede ver las configuraciones de registro que coinciden con el filtro utilizando con el mandato el distintivo <code>--show-matching-configs</code>. Este valor es opcional.</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>Espacio de nombres de Kubernetes para el que desea filtrar registros. Este valor es opcional.</dd>

  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>Nombre del contenedor desde el que desea filtrar registros. Este distintivo sólo se aplica cuando se utiliza el tipo de registro de <code>container</code>. Este valor es opcional.</dd>

  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>Filtra los registros en el nivel especificado y en los inferiores. Valores aceptables en su orden canónico son <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> y <code>trace</code>. Este valor es opcional. Por ejemplo, si filtra registros al nivel <code>info</code>, también se filtran los niveles <code>debug</code> y <code>trace</code>. **Nota**: Puede utilizar este distintivo sólo cuando los mensajes de registro están en formato JSON y contienen un campo de nivel. Salida de ejemplo:
<code>{"log": "hello", "level": "info"}</code></dd>

  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>Filtra los registros que contienen un mensaje concreto en el registro. Este valor es opcional. Ejemplo: El mensaje "Hello", "!", y "Hello, World!", se aplicarían al registro "Hello, World!".</dd>

  <dt><code>--regex-message <em>MESSAGE</em></code></dt>
    <dd>Filtra todos los registros que contienen un mensaje concreto que se escribe como una expresión regular. Este valor es opcional. Ejemplo: el patrón "hello [0-9]" se aplicaría a "hello 1", "hello 2" y "hello 9".</dd>

  <dt><code>--force-update</code></dt>
    <dd>Forzar la actualización de sus pods de Fluentd a la versión más reciente. Fluentd debe estar a la versión más reciente para poder realizar cambios en las configuraciones de registro.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplos**:

Este ejemplo filtra todos los registros reenviados desde contenedores con el nombre `test-container` en el espacio de nombres predeterminado que se encuentren en el nivel debug o inferior con un mensaje que contenga "GET request".

  ```
  ibmcloud ks logging-filter-create --cluster example-cluster --type container --namespace default --container test-container --level debug --message "GET request"
  ```
  {: pre}

Este ejemplo filtra todos los registros que se reenvían, en un nivel `info` o inferior, desde un clúster específico. La salida se devuelve como JSON.

  ```
  ibmcloud ks logging-filter-create --cluster example-cluster --type all --level info --json
  ```
  {: pre}



### ibmcloud ks logging-filter-get
{: #cs_log_filter_view}

Visualizar una configuración de filtro de registro. Utilice este mandato para visualizar los filtros de registro que haya creado.
{: shortdesc}

```
ibmcloud ks logging-filter-get --cluster CLUSTER [--id FILTER_ID] [--show-matching-configs] [--show-covering-filters] [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>Nombre o ID de clúster del que desea visualizar los filtros. Este valor es obligatorio.</dd>

  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>ID del filtro de registro que desea visualizar.</dd>

  <dt><code>--show-matching-configs</code></dt>
    <dd>Muestra las configuraciones de registro que coincidan con la configuración que está visualizando. Este valor es opcional.</dd>

  <dt><code>--show-covering-filters</code></dt>
    <dd>Muestra los filtros de registro que hicieron los filtros anteriores obsoletos. Este valor es opcional.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
     <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

```
ibmcloud ks logging-filter-get mycluster --id 885732 --show-matching-configs
```
{: pre}

### ibmcloud ks logging-filter-rm
{: #cs_log_filter_delete}

Suprimir un filtro de registro. Utilice este mandato para eliminar un filtro de registro que haya creado.
{: shortdesc}

```
ibmcloud ks logging-filter-rm --cluster CLUSTER [--id FILTER_ID] [--all] [--force-update] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>Nombre o ID del clúster del que desea suprimir un filtro.</dd>

  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>El ID del filtro de registro para suprimir.</dd>

  <dt><code>--all</code></dt>
    <dd>Suprimir todos filtros de reenvío de registro. Este valor es opcional.</dd>

  <dt><code>--force-update</code></dt>
    <dd>Forzar la actualización de sus pods de Fluentd a la versión más reciente. Fluentd debe estar a la versión más reciente para poder realizar cambios en las configuraciones de registro.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

```
ibmcloud ks logging-filter-rm mycluster --id 885732
```
{: pre}

### ibmcloud ks logging-filter-update
{: #cs_log_filter_update}

Actualizar un filtro de registro. Utilice este mandato para actualizar un filtro de registro que haya creado.
{: shortdesc}

```
ibmcloud ks logging-filter-update --cluster CLUSTER --id FILTER_ID --type LOG_TYPE [--logging-configs CONFIGS] [--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME] [--level LOGGING_LEVEL] [--message MESSAGE] [--regex-message MESSAGE] [--force-update] [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>Nombre o ID de clúster que desea para actualizar un filtro de registro. Este valor es obligatorio.</dd>

 <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>El ID del filtro de registro para actualizar.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>Tipo de registro al que aplicar el filtro. Actualmente se da soporte a <code>all</code>, <code>container</code> y <code>host</code>.</dd>

  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>Una lista separada por comas de los ID de configuración de registro. Si no se proporciona, el filtro se aplica a todas las configuraciones de registro de clúster que se pasan al filtro. Puede ver las configuraciones de registro que coinciden con el filtro utilizando con el mandato el distintivo <code>--show-matching-configs</code>. Este valor es opcional.</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>Espacio de nombres de Kubernetes para el que desea filtrar registros. Este valor es opcional.</dd>

  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>Nombre del contenedor desde el que desea filtrar registros. Este distintivo sólo se aplica cuando se utiliza el tipo de registro de <code>container</code>. Este valor es opcional.</dd>

  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>Filtra los registros en el nivel especificado y en los inferiores. Valores aceptables en su orden canónico son <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> y <code>trace</code>. Este valor es opcional. Por ejemplo, si filtra registros al nivel <code>info</code>, también se filtran los niveles <code>debug</code> y <code>trace</code>. **Nota**: Puede utilizar este distintivo sólo cuando los mensajes de registro están en formato JSON y contienen un campo de nivel. Salida de ejemplo:
<code>{"log": "hello", "level": "info"}</code></dd>

  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>Filtra los registros que contienen un mensaje concreto en el registro. Este valor es opcional. Ejemplo: El mensaje "Hello", "!", y "Hello, World!", se aplicarían al registro "Hello, World!".</dd>

  <dt><code>--regex-message <em>MESSAGE</em></code></dt>
    <dd>Filtra todos los registros que contienen un mensaje concreto que se escribe como una expresión regular. Este valor es opcional. Ejemplo: el patrón "hello [0-9]" se aplicaría a "hello 1", "hello 2" y "hello 9"</dd>

  <dt><code>--force-update</code></dt>
    <dd>Forzar la actualización de sus pods de Fluentd a la versión más reciente. Fluentd debe estar a la versión más reciente para poder realizar cambios en las configuraciones de registro.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplos**:

Este ejemplo filtra todos los registros reenviados desde contenedores con el nombre `test-container` en el espacio de nombres predeterminado que se encuentren en el nivel debug o inferior con un mensaje que contenga "GET request".

  ```
  ibmcloud ks logging-filter-update --cluster example-cluster --id 885274 --type container --namespace default --container test-container --level debug --message "GET request"
  ```
  {: pre}

Este ejemplo filtra todos los registros que se reenvían, en un nivel `info` o inferior, desde un clúster específico. La salida se devuelve como JSON.

  ```
  ibmcloud ks logging-filter-update --cluster example-cluster --id 274885 --type all --level info --json
  ```
  {: pre}

### ibmcloud ks logging-collect
{: #cs_log_collect}

Realizar una solicitud de una instantánea para sus registros en un punto en el tiempo específico y guardar los registros en un grupo de {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

```
ibmcloud ks logging-collect --cluster CLUSTER --cos-bucket BUCKET_NAME --cos-endpoint ENDPOINT --hmac-key-id HMAC_KEY_ID --hmac-key HMAC_KEY --type LOG_TYPE [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>Nombre o ID del clúster para el que desea para crear una instantánea. Este valor es obligatorio.</dd>

 <dt><code>--cos-bucket <em>BUCKET_NAME</em></code></dt>
    <dd>El nombre del grupo de {{site.data.keyword.cos_short}} en el que desea almacenar los registros. Este valor es obligatorio.</dd>

  <dt><code>--cos-endpoint <em>ENDPOINT</em></code></dt>
    <dd>El punto final de {{site.data.keyword.cos_short}} correspondiente al grupo en el que desea almacenar los registros. Este valor es obligatorio.</dd>

  <dt><code>--hmac-key-id <em>HMAC_KEY_ID</em></code></dt>
    <dd>El ID exclusivo de las credenciales HMAC para la instancia de almacenamiento de objetos. Este valor es obligatorio.</dd>

  <dt><code>--hmac-key <em>HMAC_KEY</em></code></dt>
    <dd>La clave de HMAC para la instancia de {{site.data.keyword.cos_short}}. Este valor es obligatorio.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>El tipo de registros de los que desea crear una instantánea. Actualmente, `master` es la única opción, así como el valor predeterminado.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks logging-collect --cluster mycluster --cos-bucket mybucket --cos-endpoint s3-api.us-geo.objectstorage.softlayer.net --hmac-key-id e2e7f5c9fo0144563c418dlhi3545m86 --hmac-key c485b9b9fo4376722f692b63743e65e1705301ab051em96j
  ```
  {: pre}

**Salida de ejemplo**:

  ```
  There is no specified log type. The default master will be used.
  Submitting log collection request for master logs for cluster mycluster...
  OK
  The log collection request was successfully submitted. To view the status of the request run ibmcloud ks logging-collect-status mycluster.
  ```
  {: screen}


### ibmcloud ks logging-collect-status
{: #cs_log_collect_status}

Comprobar el estado de la solicitud de instantánea de recopilación de registros para el clúster.
{: shortdesc}

```
ibmcloud ks logging-collect-status --cluster CLUSTER [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Administrador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>Nombre o ID del clúster para el que desea para crear una instantánea. Este valor es obligatorio.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks logging-collect-status --cluster mycluster
  ```
  {: pre}

**Salida de ejemplo**:

  ```
  Getting the status of the last log collection request for cluster mycluster...
  OK
  State     Start Time             Error   Log URLs
  success   2018-09-18 16:49 PDT   - s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-0-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-1-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-2-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  ```
  {: screen}

<br />


## Mandatos de equilibrador de carga de red (`nlb-dns`)
{: #nlb-dns}

Utilice este grupo de mandatos para crear y gestionar los nombres de host para las direcciones IP del equilibrador de carga de red (NLB) y los supervisores de comprobación de estado para los nombres de host. Para obtener más información, consulte [Cómo registrar un nombre de host de equilibrador de carga](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname).
{: shortdesc}

### ibmcloud ks nlb-dns-add
{: #cs_nlb-dns-add}

Puede añadir una IP de equilibrador de carga de red (NLB) a un nombre de host existente que haya creado con el mandato [`ibmcloud ks nlb-dns-create`](#cs_nlb-dns-create).
{: shortdesc}

```
ibmcloud ks nlb-dns-add --cluster CLUSTER --ip IP --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

Por ejemplo, en un clúster multizona, puede crear un NLB en cada zona para exponer una app. Puede registrar una IP de NLB en una zona con un nombre de host ejecutando `ibmcloud ks nlb-dns-create`, por lo que ahora puede añadir las IP de NLB de las otras zonas a este nombre de host existente. Cuando un usuario accede al nombre de host de su app, el cliente accede a una de estas direcciones IP al azar y la solicitud se envía a ese NLB. Tenga en cuenta que debe ejecutar el siguiente mandato para cada dirección IP que desee añadir.

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Opciones del mandato**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--ip <em>IP</em></code></dt>
<dd>La IP de NLB que desea añadir al nombre de host. Para ver las IP de NLB, ejecute <code>kubectl get svc</code>.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>El nombre de host al que desea añadir las IP. Para ver los nombres de host existentes, ejecute <code>ibmcloud ks nlb-dnss</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks nlb-dns-add --cluster mycluster --ip 1.1.1.1 --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

### ibmcloud ks nlb-dns-create
{: #cs_nlb-dns-create}

Exponer públicamente la app mediante la creación de un nombre de host DNS para registrar una IP del equilibrador de carga de red (NLB).
{: shortdesc}

```
ibmcloud ks nlb-dns-create --cluster CLUSTER --ip IP [--json] [-s]
```
{: pre}

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Opciones del mandato**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--ip <em>IP</em></code></dt>
<dd>La dirección IP del equilibrador de carga de red que desea registrar. Para ver las IP de NLB, ejecute <code>kubectl get svc</code>. Recuerde que inicialmente puede crear el nombre de host con una sola dirección IP. Si tiene NLB en cada zona de un clúster multizona que expone una app, puede añadir las IP de los otros NLB al nombre de host ejecutando el [mandato `ibmcloud ks nlb-dns-add`](#cs_nlb-dns-add).</dd>

<dt><code>--json</code></dt>
<dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks nlb-dns-create --cluster mycluster --ip 1.1.1.1
```
{: pre}

### ibmcloud ks nlb-dnss
{: #cs_nlb-dns-ls}

Listar los nombres de host de equilibrador de carga de red y las direcciones IP que hay registradas en un clúster.
{: shortdesc}

```
ibmcloud ks nlb-dnss --cluster CLUSTER [--json] [-s]
```
{: pre}

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Opciones del mandato**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--json</code></dt>
<dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks nlb-dnss --cluster mycluster
```
{: pre}

### ibmcloud ks nlb-dns-rm
{: #cs_nlb-dns-rm}

Eliminar una dirección IP del equilibrador de carga de red de un nombre de host. Si elimina todas las IP de un nombre de host, el nombre de host sigue existiendo, pero no hay ninguna IP asociada al mismo. Tenga en cuenta que debe ejecutar este mandato para cada dirección IP que desee eliminar.
{: shortdesc}

```
ibmcloud ks nlb-dns-rm --cluster CLUSTER --ip IP --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Opciones del mandato**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--ip <em>IP</em></code></dt>
<dd>La IP de NLB que desea eliminar. Para ver las IP de NLB, ejecute <code>kubectl get svc</code>.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>El nombre de host del que desea eliminar una IP. Para ver los nombres de host existentes, ejecute <code>ibmcloud ks nlb-dnss</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks nlb-dns-rm --cluster mycluster --ip 1.1.1.1 --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

### ibmcloud ks nlb-dns-monitor
{: #cs_nlb-dns-monitor}

Crear, modificar y ver supervisores de comprobación de estado para los nombres de host de equilibrador de carga de red en un clúster. Este mandato se debe combinar con uno de los submandatos siguientes.
{: shortdesc}

#### ibmcloud ks nlb-dns-monitor-configure
{: #cs_nlb-dns-monitor-configure}

Configurar y habilitar opcionalmente un supervisor de comprobación de estado para un nombre de host de NLB existente en un clúster. Cuando se habilita un supervisor para el nombre de host, el supervisor de estado comprueba la IP de NLB en cada zona y mantiene actualizados los resultados de búsqueda de DNS en base a estas comprobaciones de estado.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-configure --cluster CLUSTER --nlb-host HOST NAME [--enable] [--desc DESCRIPTION] [--type TYPE] [--method METHOD] [--path PATH] [--timeout TIMEOUT] [--retries RETRIES] [--interval INTERVAL] [--port PORT] [--header HEADER] [--expected-body BODY STRING] [--expected-codes HTTP CODES] [--follows-redirects TRUE] [--allows-insecure TRUE] [--json] [-s]
```
{: pre}

Puede utilizar este mandato para crear y habilitar un supervisor de comprobación de estado nuevo, o para actualizar los valores de un supervisor de comprobación de estado existente. Para crear un supervisor nuevo, incluya el distintivo `--enable` y los distintivos para todos los valores que desee configurar. Para actualizar un supervisor existente, incluya sólo los distintivos para los valores que desee cambiar.

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Opciones del mandato**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o el ID del clúster en el que está registrado el nombre de host.</dd>

<dt><code>--nlb-host <em>HOST NAME</em></code></dt>
<dd>El nombre de host para el que desea configurar un supervisor de comprobación de estado. Para obtener una lista de nombres de host, ejecute <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>.</dd>

<dt><code>--enable</code></dt>
<dd>Incluya este distintivo para crear y habilitar un nuevo supervisor de comprobación de estado para un nombre de host.</dd>

<dt><code>--description <em>DESCRIPTION</em></code></dt>
<dd>Una descripción del monitor de estado.</dd>

<dt><code>--type <em>TYPE</em></code></dt>
<dd>El protocolo a utilizar para la comprobación de estado: <code>HTTP</code>, <code>HTTPS</code> o <code>TCP</code>. Valor predeterminado: <code>HTTP</code></dd>

<dt><code>--method <em>METHOD</em></code></dt>
<dd>El método a utilizar para la comprobación de estado. Valor predeterminado para <code>type</code> <code>HTTP</code> y <code>HTTPS</code>: <code>GET</code>. Valor predeterminado para <code>type</code> <code>TCP</code>: <code>connection_established</code>.</dd>

<dt><code>--path <em>PATH</em></code></dt>
<dd>Cuando <code>type</code> es <code>HTTPS</code>: La vía de acceso del punto final contra el que se realizará la comprobación de estado. Valor predeterminado: <code>/</code></dd>

<dt><code>--timeout <em>TIMEOUT</em></code></dt>
<dd>El tiempo de espera, en segundos, antes de considerar la IP en mal estado. Valor predeterminado: <code>5</code></dd>

<dt><code>--retries <em>RETRIES</em></code></dt>
<dd>Cuando se excede el tiempo de espera, el número de reintentos ´llevarán a cabo antes de considerar la IP en mal estado. Los reintentos se realizan inmediatamente. Valor predeterminado: <code>2</code></dd>

<dt><code>--interval <em>INTERVAL</em></code></dt>
<dd>El intervalo, en segundos, entre cada comprobación de estado. Los intervalos cortos pueden mejorar el tiempo de migración tras error, pero aumentar la carga en los IP. Valor predeterminado: <code>60</code></dd>

<dt><code>--port <em>PORT</em></code></dt>
<dd>El número de puerto al que conectar para la comprobación de estado. Cuando <code>type</code> es <code>TCP</code>, este parámetro es obligatorio. Cuando <code>type</code> es <code>HTTP</code> o <code>HTTPS</code>, sólo es necesario definir el puerto si se utiliza un puerto distinto de 80 para HTTP o de 443 para HTTPS. Valor predeterminado para TCP: <code>0</code>. Valor predeterminado para HTTP: <code>80</code>. Valor predeterminado para HTTPS: <code>443</code>.</dd>

<dt><code>--header <em>HEADER</em></code></dt>
<dd>Cuando <code>type</code> es <code>HTTPS</code> o <code>HTTPS</code>: las cabeceras de solicitud HTTP a enviar en la comprobación de estado, como por ejemplo una cabecera Host. La cabecera User-Agent no se puede sobrescribir.</dd>

<dt><code>--expected-body <em>BODY STRING</em></code></dt>
<dd>Cuando <code>type</code> es <code>HTTP</code> o <code>HTTPS</code>: una subserie no sensible a las mayúsculas y minúsculas que la comprobación de estado busca en el cuerpo de la respuesta. Si no se encuentra esta serie, se considera que la IP está en mal estado.</dd>

<dt><code>--expected-codes <em>HTTP CODES</em></code></dt>
<dd>Cuando <code>type</code> es <code>HTTP</code> o <code>HTTPS</code>: códigos HTTP que la comprobación de estado busca en la respuesta. Si no se encuentra el código HTTP, se considera que la IP está en mal estado. Valor predeterminado: <code>2xx</code></dd>

<dt><code>--allows-insecure <em>TRUE</em></code></dt>
<dd>Cuando <code>type</code> es <code>HTTP</code> o <code>HTTPS</code>: se establece en <code>true</code> para no validar el certificado.</dd>

<dt><code>--follows-redirects <em>TRUE</em></code></dt>
<dd>Cuando <code>type</code> es <code>HTTP</code> o <code>HTTPS</code>: se establece en <code>true</code> para seguir cualquier redirección que devuelva la IP.</dd>

<dt><code>--json</code></dt>
<dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks nlb-dns-monitor-configure --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud --enable --desc "Login page monitor" --type HTTPS --method GET --path / --timeout 5 --retries 2 --interval 60  --expected-body "healthy" --expected-codes 2xx --follows-redirects true
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-get
{: #cs_nlb-dns-monitor-get}

Ver los valores de un supervisor de comprobación de estado existente.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-get --cluster CLUSTER --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Opciones del mandato**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>El nombre de host que supervisa el supervisor de estado. Para obtener una lista de nombres de host, ejecute <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks nlb-dns-monitor-get --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-disable
{: #cs_nlb-dns-monitor-disable}

Inhabilitar un supervisor de comprobación de estado existente para un nombre de host en un clúster.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-disable --cluster CLUSTER --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Opciones del mandato**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>El nombre de host que supervisa el supervisor de estado. Para obtener una lista de nombres de host, ejecute <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks nlb-dns-monitor-disable --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-enable
{: #cs_nlb-dns-monitor-enable}

Habilitar un supervisor de comprobación de estado que haya configurado.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-enable --cluster CLUSTER --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

Tenga en cuenta que la primera vez que crea un supervisor de comprobación de estado, debe configurarlo y habilitarlo con el mandato `ibmcloud ks nlb-dns-monitor-configure`. Utilice el mandato `ibmcloud ks nlb-dns-monitor-enable` sólo para habilitar un supervisor que haya configurado pero que todavía no haya habilitado, o para volver a habilitar un supervisor que haya inhabilitado anteriormente.

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Opciones del mandato**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>El nombre de host que supervisa el supervisor de estado. Para obtener una lista de nombres de host, ejecute <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks nlb-dns-monitor-enable --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-ls
{: #cs_nlb-dns-monitor-ls}

Listar los valores del supervisor de comprobación de estado para cada nombre de host de NLB en un clúster.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-ls --cluster CLUSTER [--json] [-s]
```
{: pre}

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Opciones del mandato**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--json</code></dt>
<dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks nlb-dns-monitor-ls --cluster mycluster
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-status
{: #cs_nlb-dns-monitor-status}

Enumerar el estado de la comprobación de estado para las IP detrás de los nombres de host de NLB de un clúster.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-status --cluster CLUSTER [--json] [-s]
```
{: pre}

**Permisos mínimos necesarios**: rol de plataforma **Editor** para el clúster en {{site.data.keyword.containerlong_notm}}

**Opciones del mandato**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>-- <em></em></code></dt>
<dd>Incluya este distintivo para ver el estado de sólo un nombre de host. Para obtener una lista de nombres de host, ejecute <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:
```
ibmcloud ks nlb-dns-monitor-status --cluster mycluster
```
{: pre}

<br />


## Mandatos de región
{: #region_commands}

Utilice este grupo de mandatos para ver las ubicaciones disponibles, ver la región actualmente establecida como destino y para establecer la región de destino.
{: shortdesc}

### ibmcloud ks region
{: #cs_region}

Buscar la región {{site.data.keyword.containerlong_notm}} que tiene establecida como destino actualmente. Puede crear y gestionar clústeres específicos para la región. Utilice el mandato `ibmcloud ks region-set` para cambiar regiones.
{: shortdesc}

```
ibmcloud ks region
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: ninguno

### ibmcloud ks region-set
{: #cs_region-set}

Establecer la región para {{site.data.keyword.containerlong_notm}}. Puede crear y gestionar clústeres específicos para la región, y es posible que desee clústeres en varias regiones para tener una alta disponibilidad.
{: shortdesc}

Por ejemplo, puede iniciar una sesión en {{site.data.keyword.Bluemix_notm}} en la Región EE. UU. sur y crear un clúster. A continuación, puede utilizar `ibmcloud ks region-set eu-central` para establecer la región UE central como destino y crear otro clúster. Por último, puede utilizar `ibmcloud ks region-set us-south` para volver a EE. UU. sur para gestionar el clúster en esa región.

<strong>Permisos mínimos necesarios</strong>: ninguno

```
ibmcloud ks region-set [--region REGION]
```
{: pre}

**Opciones del mandato**:

<dl>
<dt><code>--region <em>REGION</em></code></dt>
<dd>Especifique la región que desea establecer como destino. Este valor es opcional. Si no indica ninguna región, puede seleccionarla de la lista en la salida.

Para obtener una lista de regiones disponibles, consulte [regiones y zonas](/docs/containers?topic=containers-regions-and-zones) o utilice el [mandato](#cs_regions) `ibmcloud ks regions`.</dd></dl>

**Ejemplo**:

```
ibmcloud ks region-set eu-central
```
{: pre}

```
ibmcloud ks region-set
```
{: pre}

**Salida**:
```
Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 3
OK
```
{: screen}

### ibmcloud ks regions
{: #cs_regions}

Listar las regiones disponibles. `Region Name` es el nombre de {{site.data.keyword.containerlong_notm}} y `Region Alias` es el nombre de {{site.data.keyword.Bluemix_notm}} general para la región.
{: shortdesc}

<strong>Permisos mínimos necesarios</strong>: ninguno

**Ejemplo**:

```
ibmcloud ks regions
```
{: pre}

**Salida**:
```
Region Name   Region Alias
ap-north      jp-tok
ap-south      au-syd
eu-central    eu-de
uk-south      eu-gb
us-east       us-east
us-south      us-south
```
{: screen}

### ibmcloud ks zones
{: #cs_datacenters}

Ver una lista de las zonas disponibles en las que puede crear un clúster. Las zonas disponibles varían en función de la región en la que se ha conectado. Para cambiar de región, ejecute `ibmcloud ks region-set`.
{: shortdesc}

```
ibmcloud ks zones [--region-only] [--json] [-s]
```
{: pre}

<strong>Opciones del mandato</strong>:

   <dl><dt><code>--region-only</code></dt>
   <dd>Listar sólo varias zonas dentro de la región en la que ha iniciado la sesión. Este valor es opcional.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Permisos mínimos**: ninguno

<br />



## Mandatos de nodo trabajador
{: worker_node_commands}


### En desuso: ibmcloud ks worker-add
{: #cs_worker_add}

Añadir a su clúster nodos trabajadores autónomos que no estén en una agrupación de nodos trabajadores.
{: deprecated}

```
ibmcloud ks worker-add --cluster CLUSTER [--file FILE_LOCATION] [--hardware HARDWARE] --machine-type MACHINE_TYPE --workers NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN [--disable-disk-encrypt] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>La vía de acceso al archivo YAML para añadir nodos trabajadores al clúster. En lugar de definir los nodos trabajadores adicionales mediante las opciones que se proporcionan en este mandato, puede utilizar un archivo YAML. Este valor es opcional.

<p class="note">Si especifica la misma opción en el mandato y como parámetro en el archivo YAML, el valor en el mandato prevalece sobre el valor del archivo YAML. Por ejemplo, supongamos que define un tipo de máquina en el archivo YAML y utiliza la opción --machine-type en el mandato; el valor que especifique en la opción del mandato prevalece sobre el valor del archivo YAML.</p>

<pre class="codeblock">
<code>name: <em>&lt;cluster_name_or_ID&gt;</em>
zone: <em>&lt;zone&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
diskEncryption: <em>false</em></code></pre>

<table>
<caption>Visión general de los componentes del archivo YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td>Sustituya <code><em>&lt;cluster_name_or_ID&gt;</em></code> por el nombre o ID del clúster donde desea añadir nodos trabajadores.</td>
</tr>
<tr>
<td><code><em>zone</em></code></td>
<td>Sustituya <code><em>&lt;zone&gt;</em></code> por la zona de despliegue de los nodos trabajadores. Las zonas disponibles dependen de la región en la que ha iniciado la sesión. Para ver una lista de las zonas disponibles, ejecute <code>ibmcloud ks zones</code>.</td>
</tr>
<tr>
<td><code><em>machine-type</em></code></td>
<td>Sustituya <code><em>&lt;machine_type&gt;</em></code> por el tipo de máquina en el que desea desplegar los nodos trabajadores. Puede desplegar los nodos trabajadores como máquinas virtuales en hardware dedicado o compartido, o como máquinas físicas en servidores nativos. Los tipos de máquinas físicas y virtuales varían según la zona en la que se despliega el clúster. Para obtener más información, consulte el [mandato](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types) `ibmcloud ks machine-types`.</td>
</tr>
<tr>
<td><code><em>private-vlan</em></code></td>
<td>Sustituya <code><em>&lt;private_VLAN&gt;</em></code> por el ID de la VLAN privada que desea utilizar para sus nodos trabajadores. Para ver una lista de las VLAN disponibles, ejecute <code>ibmcloud ks vlans <em>&lt;zone&gt;</em></code> y busque direccionadores de VLAN que empiecen por <code>bcr</code> ("back-end router", direccionador de fondo).</td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td>Sustituya <code>&lt;public_VLAN&gt;</code> por el ID de la VLAN pública que desea utilizar para sus nodos trabajadores. Para ver una lista de las VLAN disponibles, ejecute <code>ibmcloud ks vlans &lt;zone&gt;</code> y busque direccionadores de VLAN que empiecen por <code>fcr</code> ("front-end router", direccionador frontal). <br><strong>Nota</strong>: Si los nodos trabajadores se han configurado solo con una VLAN privada, debe permitir que los nodos trabajadores y el maestro del clúster se comuniquen mediante la [habilitación del punto final de servicio privado](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private) o la [configuración de un dispositivo de pasarela](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_gateway).</td>
</tr>
<tr>
<td><code>hardware</code></td>
<td>Para tipos de máquina virtual: El nivel de aislamiento del hardware del nodo trabajador. Utilice el valor dedicated si desea tener recursos físicos disponibles dedicados solo a usted, o el valor shared para permitir que los recursos físicos se compartan con otros clientes de IBM. El valor predeterminado es shared.</td>
</tr>
<tr>
<td><code>workerNum</code></td>
<td>Sustituya <code><em>&lt;number_workers&gt;</em></code> por el número de nodos trabajadores que desea desplegar.</td>
</tr>
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>Los nodos trabajadores tienen cifrado de disco AES de 256 bits de forma predeterminada; [más información](/docs/containers?topic=containers-security#encrypted_disk). Para inhabilitar el cifrado, incluya esta opción y establezca el valor en <code>false</code>.</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>El nivel de aislamiento del hardware del nodo trabajador. Utilice el valor dedicated si desea tener recursos físicos disponibles dedicados solo a usted, o el valor shared para permitir que los recursos físicos se compartan con otros clientes de IBM. El valor predeterminado es shared. Este valor es opcional. Para los tipos de máquina nativos, especifique `dedicated`.</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Elija un tipo de máquina. Puede desplegar los nodos trabajadores como máquinas virtuales en hardware dedicado o compartido, o como máquinas físicas en servidores nativos. Los tipos de máquinas físicas y virtuales varían según la zona en la que se despliega el clúster. Para obtener más información, consulte la documentación correspondiente al [mandato](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types) `ibmcloud ks machine-types`. Este valor es obligatorio para clústeres estándares y no está disponible para clústeres gratuitos.</dd>

<dt><code>--workers <em>NUMBER</em></code></dt>
<dd>Un entero que representa el número de nodos trabajadores que desea crear en el clúster. El valor predeterminado es 1. Este valor es opcional.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>La VLAN privada que se ha especificado al crear el clúster. Este valor es obligatorio. Los direccionadores de VLAN privadas siempre
empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre
empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos.</dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>La VLAN pública que se ha especificado al crear el clúster. Este valor es opcional. Si desea que los nodos trabajadores existan solo en una VLAN privada, no proporcione ningún ID de VLAN pública. Los direccionadores de VLAN privadas siempre
empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre
empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos.<p class="note">Si los nodos trabajadores se han configurado solo con una VLAN privada, debe permitir que los nodos trabajadores y el maestro del clúster se comuniquen mediante la [habilitación del punto final de servicio privado](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private) o la [configuración de un dispositivo de pasarela](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_gateway).</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Los nodos trabajadores tienen cifrado de disco AES de 256 bits de forma predeterminada; [más información](/docs/containers?topic=containers-security#encrypted_disk). Para inhabilitar el cifrado, incluya esta opción.</dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

</dl>

**Ejemplos**:

  ```
  ibmcloud ks worker-add --cluster my_cluster --workers 3 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type b3c.4x16 --hardware shared
  ```
  {: pre}

### ibmcloud ks worker-get
{: #cs_worker_get}

Visualizar los detalles de un nodo trabajador.
{: shortdesc}

```
ibmcloud ks worker-get --cluster [CLUSTER_NAME_OR_ID] --worker WORKER_NODE_ID [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER_NAME_OR_ID</em></code></dt>
   <dd>El nombre o ID del clúster del nodo trabajador. Este valor es opcional.</dd>

   <dt><code>--worker <em>WORKER_NODE_ID</em></code></dt>
   <dd>El nombre del nodo trabajador. Ejecute <code>ibmcloud ks workers <em>CLUSTER</em></code> para ver los ID de los nodos trabajadores de un clúster. Este valor es obligatorio.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks worker-get --cluster my_cluster --worker kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1
  ```
  {: pre}

**Salida de ejemplo**:

  ```
  ID:           kube-dal10-123456789-w1
  State:        normal
  Status:       Ready
  Trust:        disabled
  Private VLAN: 223xxxx
  Public VLAN:  223xxxx
  Private IP:   10.xxx.xx.xxx
  Public IP:    169.xx.xxx.xxx
  Hardware:     shared
  Zone:         dal10
  Version:      1.8.11_1509
  ```
  {: screen}

### ibmcloud ks worker-reboot
{: #cs_worker_reboot}

Rearrancar un nodo trabajador de un clúster. Durante el rearranque, el estado del nodo trabajador no cambia. Por ejemplo, puede utilizar un rearranque si el estado de nodo trabajador en la infraestructura de IBM Cloud (SoftLayer) es `Apagado` y tiene que activar el nodo trabajador. Un rearranque borra los directorios temporales, pero no borra el sistema de archivos completo ni reformatea los discos. La dirección IP pública y privada del nodo trabajador sigue siendo la misma después de la operación de rearranque.
{: shortdesc}

```
ibmcloud ks worker-reboot [-f] [--hard] --cluster CLUSTER --worker WORKER [WORKER] [--skip-master-healthcheck] [-s]
```
{: pre}

**Atención:** Rearrancar un nodo trabajador puede provocar que se corrompan los datos en el nodo trabajador. Utilice este mandato con precaución y cuando sepa que el rearranque puede ayudar a recuperar el nodo trabajador. En todos los demás casos, [recargue el nodo trabajador](#cs_worker_reload) en su lugar.

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

Antes de rearrancar el nodo trabajador, asegúrese de que los pods se vuelven a programar en otros nodos trabajadores para evitar el tiempo de inactividad de la app o la corrupción de los datos en el nodo trabajador.

1. Liste todos los nodos trabajadores del clúster y anote el **nombre** del nodo trabajador que desea rearrancar.
   ```
   kubectl get nodes
   ```
   {: pre}
   El **nombre** que se devuelve en este mandato es la dirección IP privada que se asigna al nodo trabajador. Encontrará más información sobre el nodo trabajador si ejecuta el mandato `ibmcloud ks workers <cluster_name_or_ID>` y busca el nodo trabajador con la misma dirección **IP privada**.
2. Marque el nodo trabajador como no programable en un proceso que se conoce como acordonamiento. Cuando se acordona un nodo trabajador, deja de estar disponible para programar pods en el futuro. Utilice el **nombre** del nodo trabajador que ha recuperado en el paso anterior.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Verifique que la planificación de pods está inhabilitada en el nodo trabajador.
   ```
   kubectl get nodes
   ```
   {: pre}
   La planificación de pods está inhabilitada en el nodo trabajador si el estado es **`SchedulingDisabled`**.
 4. Fuerce la eliminación de los pods del nodo trabajador y vuelva a programarlos en los demás nodos trabajadores del clúster.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    Este proceso puede tardar unos minutos.
 5. Rearranque el nodo trabajador. Utilice el ID del nodo trabajador que se devuelve del mandato `ibmcloud ks workers <cluster_name_or_ID>`.
    ```
    ibmcloud ks worker-reboot --cluster <cluster_name_or_ID> --workers <worker_name_or_ID>
    ```
    {: pre}
 6. Espere unos 5 minutos antes de volver a permitir la planificación de pods en el nodo trabajador para asegurarse de que finalice el rearranque. Durante el rearranque, el estado del nodo trabajador no cambia. Por lo general, el rearranque de un nodo trabajador se realiza en pocos segundos.
 7. Permita la planificación de pods en el nodo trabajador. Utilice el **nombre** del nodo trabajador que se devuelve del mandato `kubectl get nodes`.
    ```
    kubectl uncordon <worker_name>
    ```
    {: pre}

    </br>

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar el reinicio de un nodo trabajador sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code>--hard</code></dt>
   <dd>Utilice esta opción para forzar un reinicio de un nodo trabajador, cortando el suministro eléctrico al nodo trabajador. Utilice esta opción si el nodo trabajador no responde o si el tiempo de ejecución del contenedor del nodo trabajador no responde. Este valor es opcional.</dd>

   <dt><code>--workers <em>WORKER</em></code></dt>
   <dd>El nombre o ID de uno o varios nodos trabajadores. Utilice un espacio para ver una lista de varios nodos trabajadores. Este valor es obligatorio.</dd>

   <dt><code>--skip-master-healthcheck</code></dt>
   <dd>Omita una comprobación de estado de su nodo maestro antes de volver a cargar o volver a arrancar los nodos trabajadores.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks worker-reboot --cluster my_cluster --worker kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}



### ibmcloud ks worker-reload
{: #cs_worker_reload}

Volver a cargar las configuraciones para un nodo trabajador. Una recarga puede ser útil si el nodo trabajador tiene problemas, como un rendimiento lento, o si queda bloqueado en un estado incorrecto. Tenga en cuenta que, durante la recarga, la máquina del nodo trabajador se actualizará con la imagen más reciente y se suprimirán los datos si no se han [almacenado fuera del nodo trabajador](/docs/containers?topic=containers-storage_planning#persistent_storage_overview). La dirección IP del nodo trabajador sigue siendo la misma después de la operación de recarga.
{: shortdesc}

```
ibmcloud ks worker-reload [-f] --cluster CLUSTER --workers WORKER [WORKER] [--skip-master-healthcheck] [-s]
```
{: pre}

La recarga de un nodo trabajador aplica actualizaciones de versiones de parche a dicho nodo trabajador, pero no actualizaciones mayores o menores. Para ver los cambios entre versiones de parches, revise la documentación de [Registro de cambios de versiones](/docs/containers?topic=containers-changelog#changelog).
{: tip}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

Antes de recargar el nodo trabajador, asegúrese de que los pods se vuelven a programar en otros nodos trabajadores para evitar el tiempo de inactividad de la app o la corrupción de los datos en el nodo trabajador.

1. Liste todos los nodos trabajadores del clúster y anote el **nombre** del nodo trabajador que desea recargar.
   ```
   kubectl get nodes
   ```
   El **nombre** que se devuelve en este mandato es la dirección IP privada que se asigna al nodo trabajador. Encontrará más información sobre el nodo trabajador si ejecuta el mandato `ibmcloud ks workers <cluster_name_or_ID>` y busca el nodo trabajador con la misma dirección **IP privada**.
2. Marque el nodo trabajador como no programable en un proceso que se conoce como acordonamiento. Cuando se acordona un nodo trabajador, deja de estar disponible para programar pods en el futuro. Utilice el **nombre** del nodo trabajador que ha recuperado en el paso anterior.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Verifique que la planificación de pods está inhabilitada en el nodo trabajador.
   ```
   kubectl get nodes
   ```
   {: pre}
   La planificación de pods está inhabilitada en el nodo trabajador si el estado es **`SchedulingDisabled`**.
 4. Fuerce la eliminación de los pods del nodo trabajador y vuelva a programarlos en los demás nodos trabajadores del clúster.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    Este proceso puede tardar unos minutos.
 5. Recargue el nodo trabajador. Utilice el ID del nodo trabajador que se devuelve del mandato `ibmcloud ks workers <cluster_name_or_ID>`.
    ```
    ibmcloud ks worker-reload --cluster <cluster_name_or_ID> --workers <worker_name_or_ID>
    ```
    {: pre}
 6. Espere a que finalice la recarga.
 7. Permita la planificación de pods en el nodo trabajador. Utilice el **nombre** del nodo trabajador que se devuelve del mandato `kubectl get nodes`.
    ```
    kubectl uncordon <worker_name>
    ```
</br>
<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar que se vuelva a cargar un nodo trabajador sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code>--worker <em>WORKER</em></code></dt>
   <dd>El nombre o ID de uno o varios nodos trabajadores. Utilice un espacio para ver una lista de varios nodos trabajadores. Este valor es obligatorio.</dd>

   <dt><code>--skip-master-healthcheck</code></dt>
   <dd>Omita una comprobación de estado de su nodo maestro antes de volver a cargar o volver a arrancar los nodos trabajadores.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks worker-reload --cluster my_cluster --workers kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### ibmcloud ks worker-rm
{: #cs_worker_rm}

Eliminar uno o varios nodos trabajadores de un clúster. Si elimina un nodo trabajador, el clúster se desequilibra. Para volver a equilibrar automáticamente la agrupación de nodos trabajadores, ejecute el [mandato](#cs_rebalance) `ibmcloud ks worker-pool-rebalance`.
{: shortdesc}

```
ibmcloud ks worker-rm [-f] --cluster CLUSTER --workers WORKER[,WORKER] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

Antes de eliminar el nodo trabajador, asegúrese de que los pods se vuelven a programar en otros nodos trabajadores para evitar el tiempo de inactividad de la app o la corrupción de los datos en el nodo trabajador.
{: tip}

1. Liste todos los nodos trabajadores del clúster y anote el **nombre** del nodo trabajador que desea eliminar.
   ```
   kubectl get nodes
   ```
   {: pre}
   El **nombre** que se devuelve en este mandato es la dirección IP privada que se asigna al nodo trabajador. Encontrará más información sobre el nodo trabajador si ejecuta el mandato `ibmcloud ks workers <cluster_name_or_ID>` y busca el nodo trabajador con la misma dirección **IP privada**.
2. Marque el nodo trabajador como no programable en un proceso que se conoce como acordonamiento. Cuando se acordona un nodo trabajador, deja de estar disponible para programar pods en el futuro. Utilice el **nombre** del nodo trabajador que ha recuperado en el paso anterior.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Verifique que la planificación de pods está inhabilitada en el nodo trabajador.
   ```
   kubectl get nodes
   ```
   {: pre}
   La planificación de pods está inhabilitada en el nodo trabajador si el estado es **`SchedulingDisabled`**.
4. Fuerce la eliminación de los pods del nodo trabajador y vuelva a programarlos en los demás nodos trabajadores del clúster.
   ```
   kubectl drain <worker_name>
   ```
   {: pre}
   Este proceso puede tardar unos minutos.
5. Elimine el nodo trabajador. Utilice el ID del nodo trabajador que se devuelve del mandato `ibmcloud ks workers <cluster_name_or_ID>`.
   ```
   ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_name_or_ID>
   ```
   {: pre}

6. Verifique que se ha eliminado el nodo trabajador.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}
</br>
<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar la eliminación de un nodo trabajador sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code>--workers <em>WORKER</em></code></dt>
   <dd>El nombre o ID de uno o varios nodos trabajadores. Utilice un espacio para ver una lista de varios nodos trabajadores. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks worker-rm --cluster my_cluster --workers kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}



### ibmcloud ks worker-update
{: #cs_worker_update}

Actualizar los nodos trabajadores para aplicar las últimas actualizaciones y parches de seguridad para el sistema operativo, y para actualizar la versión de Kubernetes para que coincida con la versión del nodo maestro de Kubernetes. Puede actualizar la versión de Kubernetes del nodo maestro con el [mandato](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_update) `ibmcloud ks cluster-update`. La dirección IP del nodo trabajador sigue siendo la misma después de la operación de actualización.
{: shortdesc}

```
ibmcloud ks worker-update [-f] --cluster CLUSTER --workers WORKER[,WORKER] [--force-update] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

La ejecución de `ibmcloud ks worker-update` puede causar un tiempo de inactividad para sus apps y servicios. Durante la actualización, todos los pods se vuelven a planificar en otros nodos trabajadores, se vuelve a crear la imagen del nodo trabajador y los datos se suprimen si no se guardan fuera del pod. Para evitar el tiempo de inactividad, [asegúrese de tener suficientes nodos trabajadores para manejar la carga de trabajo mientras se estén actualizando los nodos trabajadores seleccionados](/docs/containers?topic=containers-update#worker_node).
{: important}

Es posible que tenga que modificar los archivos YAML para futuros despliegues antes de la actualización. Revise esta [nota del release](/docs/containers?topic=containers-cs_versions) para ver detalles.

<strong>Opciones del mandato</strong>:

   <dl>

   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster en el que se listan los nodos trabajadores disponibles. Este valor es obligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar la actualización del maestro sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Intente la actualización incluso si el cambio es superior a dos versiones anteriores. Este valor es opcional.</dd>

   <dt><code>--workers <em>WORKER</em></code></dt>
   <dd>El ID de uno o varios nodos trabajadores. Utilice un espacio para ver una lista de varios nodos trabajadores. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  ibmcloud ks worker-update --cluster my_cluster --worker kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### ibmcloud ks workers
{: #cs_workers}

Ver una lista de los nodos trabajadores y el estado de cada uno de ellos en un clúster.
{: shortdesc}

```
ibmcloud ks workers --cluster CLUSTER [--worker-pool POOL] [--show-pools] [--show-deleted] [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster para los nodos trabajadores disponibles. Este valor es obligatorio.</dd>

   <dt><code>--worker-pool <em>POOL</em></code></dt>
   <dd>Ver sólo los nodos trabajadores que pertenecen a la agrupación de nodos trabajadores. Para ver una lista de las agrupaciones de nodos trabajadores disponibles, ejecute `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`. Este valor es opcional.</dd>

   <dt><code>--show-pools</code></dt>
   <dd>Obtener una lista de la agrupación de nodos trabajadores a la que pertenece cada nodo trabajador. Este valor es opcional.</dd>

   <dt><code>--show-deleted</code></dt>
   <dd>Visualiza los nodos trabajadores que se han suprimido del clúster, incluido el motivo de la supresión. Este valor es opcional.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  ibmcloud ks workers --cluster my_cluster
  ```
  {: pre}

<br />



## Mandatos de la agrupación de nodos trabajadores
{: #worker-pool}

### ibmcloud ks worker-pool-create
{: #cs_worker_pool_create}

Puede crear una agrupación de nodos trabajadores en el clúster. Cuando se añade una agrupación de nodos trabajadores, no se le asigna una zona de forma predeterminada. Debe especificar el número de nodos trabajadores que desea en cada zona y los tipos de máquina para los trabajadores. A la agrupación de nodos trabajadores se le asignan las versiones de Kubernetes predeterminadas. Para terminar de crear los nodos trabajadores, [añada una o varias zonas](#cs_zone_add) a la agrupación.
{: shortdesc}

```
ibmcloud ks worker-pool-create --name POOL_NAME --cluster CLUSTER --machine-type MACHINE_TYPE --size-per-zone WORKERS_PER_ZONE --hardware ISOLATION [--labels LABELS] [--disable-disk-encrypt] [-s] [--json]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:
<dl>

  <dt><code>--name <em>POOL_NAME</em></code></dt>
    <dd>El nombre que desea asignar a la agrupación de nodos trabajadores.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
    <dd>Elija un tipo de máquina. Puede desplegar los nodos trabajadores como máquinas virtuales en hardware dedicado o compartido, o como máquinas físicas en servidores nativos. Los tipos de máquinas físicas y virtuales varían según la zona en la que se despliega el clúster. Para obtener más información, consulte la documentación correspondiente al [mandato](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types) `ibmcloud ks machine-types`. Este valor es obligatorio para clústeres estándares y no está disponible para clústeres gratuitos.</dd>

  <dt><code>--size-per-zone <em>WORKERS_PER_ZONE</em></code></dt>
    <dd>El número de trabajadores a crear en cada zona. Este valor es obligatorio y debe ser 1 y superior.</dd>

  <dt><code>--hardware <em>ISOLATION</em></code></dt>
    <dd>El nivel de aislamiento del hardware del nodo trabajador. Utilice `dedicated` si desea tener recursos físicos disponibles dedicados solo a usted, o `shared` para permitir que los recursos físicos se compartan con otros clientes de IBM. El valor predeterminado es `shared`. Para los tipos de máquina nativos, especifique `dedicated`. Este valor es obligatorio.</dd>

  <dt><code>--labels <em>LABELS</em></code></dt>
    <dd>Las etiquetas que desea asignar a los trabajadores en su agrupación. Ejemplo: `<key1>=<val1>`,`<key2>=<val2>`</dd>

  <dt><code>--disable-disk-encrpyt</code></dt>
    <dd>Especifica que el disco no está cifrado. El valor predeterminado es <code>false</code>.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks worker-pool-create --name my_pool --cluster my_cluster --machine-type b3c.4x16 --size-per-zone 6
  ```
  {: pre}


### ibmcloud ks worker-pool-get
{: #cs_worker_pool_get}

Visualizar los detalles de una agrupación de trabajadores.
{: shortdesc}

```
ibmcloud ks worker-pool-get --worker-pool WORKER_POOL --cluster CLUSTER [-s] [--json]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>El nombre de la agrupación de nodos trabajadores cuyos detalles desea visualizar. Para ver una lista de las agrupaciones de nodos trabajadores disponibles, ejecute `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`. Este valor es obligatorio.</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o el ID del clúster en el que se encuentra la agrupación de trabajadores. Este valor es obligatorio.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks worker-pool-get --worker-pool pool1 --cluster my_cluster
  ```
  {: pre}

**Salida de ejemplo**:

  ```
  Name:               pool
  ID:                 a1a11b2222222bb3c33c3d4d44d555e5-f6f777g
  State:              active
  Hardware:           shared
  Zones:              dal10,dal12
  Workers per zone:   3
  Machine type:       b3c.4x16.encrypted
  Labels:             -
  Version:            1.12.7_1512
  ```
  {: screen}

### ibmcloud ks worker-pool-rebalance
{: #cs_rebalance}

Puede volver a equilibrar la agrupación de nodos trabajadores después de suprimir un nodo trabajador. Cuando se ejecuta este mandato, se añaden uno o varios nodos trabajadores nuevos a la agrupación de nodos trabajadores.
{: shortdesc}

```
ibmcloud ks worker-pool-rebalance --cluster CLUSTER --worker-pool WORKER_POOL [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code><em>--cluster CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>
  <dt><code><em>--worker-pool WORKER_POOL</em></code></dt>
    <dd>La agrupación de nodos trabajadores que desea reequilibrar. Este valor es obligatorio.</dd>
  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks worker-pool-rebalance --cluster my_cluster --worker-pool my_pool
  ```
  {: pre}

### ibmcloud ks worker-pool-resize
{: #cs_worker_pool_resize}

Redimensionar su agrupación de trabajadores para aumentar o disminuir el número de nodos trabajadores que están en cada zona de su clúster. La agrupación de nodos trabajadores debe tener al menos un nodo trabajador.
{: shortdesc}

```
ibmcloud ks worker-pool-resize --worker-pool WORKER_POOL --cluster CLUSTER --size-per-zone WORKERS_PER_ZONE [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>El nombre de la agrupación de nodos trabajadores que desea actualizar. Este valor es obligatorio.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster cuyas agrupaciones de trabajadores desea redimensionar. Este valor es obligatorio.</dd>

  <dt><code>--size-per-zone <em>WORKERS_PER_ZONE</em></code></dt>
    <dd>El número de nodos trabajadores que desea tener en cada zona. Este valor es obligatorio y debe ser 1 y superior.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

</dl>

**Ejemplo**:

  ```
  ibmcloud ks worker-pool-resize --cluster my_cluster --worker-pool my_pool --size-per-zone 3
  ```
  {: pre}

### ibmcloud ks worker-pool-rm
{: #cs_worker_pool_rm}

Eliminar del clúster una agrupación de trabajadores. Se suprimen todos los nodos trabajadores en la agrupación. Al realizar la supresión se vuelven a planificar todos los pods. Para evitar el tiempo de inactividad, asegúrese de que haya suficientes trabajadores para ejecutar la carga de trabajo.
{: shortdesc}

```
ibmcloud ks worker-pool-rm --worker-pool WORKER_POOL --cluster CLUSTER [-f] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>El nombre de la agrupación de nodos trabajadores que desea eliminar. Este valor es obligatorio.</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID de clúster del que desea eliminar la agrupación de trabajadores. Este valor es obligatorio.</dd>
  <dt><code>-f</code></dt>
    <dd>Forzar que el mandato se ejecute sin solicitudes de usuario. Este valor es opcional.</dd>
  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks worker-pool-rm --cluster my_cluster --worker-pool pool1
  ```
  {: pre}

### ibmcloud ks worker-pools
{: #cs_worker_pools}

Visualizar las agrupaciones de trabajadores que tiene en un clúster.
{: shortdesc}

```
ibmcloud ks worker-pools --cluster CLUSTER [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Visor** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER_NAME_OR_ID</em></code></dt>
    <dd>El nombre o ID del clúster cuyas agrupaciones de trabajadores desea listar. Este valor es obligatorio.</dd>
  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>
  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks worker-pools --cluster my_cluster
  ```
  {: pre}

### ibmcloud ks zone-add
{: #cs_zone_add}

**Solo clústeres multizona**: después de crear un clúster o una agrupación de nodos trabajadores, puede añadir una zona. Cuando se añade una zona, se añaden nodos trabajadores a la nueva zona para que coincidan con el número de nodos trabajadores por zona que ha especificado para la agrupación de nodos trabajadores.
{: shortdesc}

```
ibmcloud ks zone-add --zone ZONE --cluster CLUSTER --worker-pools WORKER_POOL1[,WORKER_POOL2] --private-vlan PRIVATE_VLAN [--public-vlan PUBLIC_VLAN] [--private-only] [--json] [-s]
```
{: pre}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--zone <em>ZONE</em></code></dt>
    <dd>La zona que desea añadir. Debe ser una [zona con soporte multizona](/docs/containers?topic=containers-regions-and-zones#zones) dentro de la región del clúster. Este valor es obligatorio.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>--worker-pool <em>WORKER_POOLS</em></code></dt>
    <dd>Una lista separada por comas de agrupaciones de nodos trabajadores a las que se añade la zona. Se necesita como mínimo una agrupación de nodos trabajadores.</dd>

  <dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
    <dd><p>El ID de la VLAN privada. Este valor es condicional.</p>
    <p>Si tiene una VLAN privada en la zona, este valor debe coincidir con el ID de VLAN privada de uno o varios nodos trabajadores del clúster. Para ver las VLAN que tiene disponibles, ejecute <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code>. Los nuevos nodos trabajadores se añaden a la VLAN que especifique, pero las VLAN de los nodos trabajadores existentes no se modifican.</p>
    <p>Si no tiene una VLAN privada o una VLAN pública en dicha zona, no especifique esta opción. Se crea automáticamente una VLAN privada y pública cuando se añade inicialmente una zona nueva a la agrupación de nodos trabajadores.</p>
    <p>Si tiene varias VLAN para un clúster, varias subredes en la misma VLAN o un clúster multizona, debe habilitar la [función de direccionador virtual (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para la cuenta de infraestructura de IBM Cloud (SoftLayer) para que los nodos trabajadores puedan comunicarse entre sí en la red privada. Para habilitar VRF, [póngase en contacto con el representante de su cuenta de la infraestructura de IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si no puede o no desea habilitar VRF, habilite la [distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para llevar a cabo esta acción, necesita el [permiso de la infraestructura](/docs/containers?topic=containers-users#infra_access) **Red > Gestionar distribución de VLAN de red** o bien puede solicitar al propietario de la cuenta que lo habilite. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.</p></dd>

  <dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
    <dd><p>ID de la VLAN pública. Este valor es obligatorio si desea exponer las cargas de trabajo en los nodos al público después de crear el clúster. Debe coincidir con el ID de VLAN pública de uno o varios nodos trabajadores del clúster para la zona. Para ver las VLAN que tiene disponibles, ejecute <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code>. Los nuevos nodos trabajadores se añaden a la VLAN que especifique, pero las VLAN de los nodos trabajadores existentes no se modifican.</p>
    <p>Si no tiene una VLAN privada o una VLAN pública en dicha zona, no especifique esta opción. Se crea automáticamente una VLAN privada y pública cuando se añade inicialmente una zona nueva a la agrupación de nodos trabajadores.</p>
    <p>Si tiene varias VLAN para un clúster, varias subredes en la misma VLAN o un clúster multizona, debe habilitar la [función de direccionador virtual (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para la cuenta de infraestructura de IBM Cloud (SoftLayer) para que los nodos trabajadores puedan comunicarse entre sí en la red privada. Para habilitar VRF, [póngase en contacto con el representante de su cuenta de la infraestructura de IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si no puede o no desea habilitar VRF, habilite la [distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para llevar a cabo esta acción, necesita el [permiso de la infraestructura](/docs/containers?topic=containers-users#infra_access) **Red > Gestionar distribución de VLAN de red** o bien puede solicitar al propietario de la cuenta que lo habilite. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.</p></dd>

  <dt><code>--private-only</code></dt>
    <dd>Utilice esta opción para evitar que se cree una VLAN pública. Sólo es necesario cuando especifica el distintivo `--private-vlan` y no incluye el distintivo `--public-vlan`. <p class="note">Si los nodos trabajadores se han configurado únicamente con una VLAN privada, debe habilitar el punto final de servicio privado o configurar un dispositivo de pasarela. Para obtener más información, consulte [Clústeres privados](/docs/containers?topic=containers-plan_clusters#private_clusters).</p></dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks zone-add --zone dal10 --cluster my_cluster --worker-pools pool1,pool2,pool3 --private-vlan 2294021
  ```
  {: pre}

### ibmcloud ks zone-network-set
{: #cs_zone_network_set}

**Solo clústeres multizona:**: establezca los metadatos de red para una agrupación de nodos trabajadores para utilizar para la zona una VLAN pública o privada distinta de la utilizada anteriormente. Los nodos trabajadores que ya se han creado en la agrupación siguen utilizando la VLAN pública o privada anterior, pero los nuevos nodos trabajadores de la agrupación utilizan los nuevos datos de red.
{: shortdesc}

```
ibmcloud ks zone-network-set --zone ZONE --cluster CLUSTER --worker-pools WORKER_POOL1[,WORKER_POOL2] --private-vlan PRIVATE_VLAN [--public-vlan PUBLIC_VLAN] [-f] [-s]
```
{: pre}

  <strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

  Los direccionadores de VLAN privadas siempre
empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre
empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos.
  <ol><li>Compruebe las VLAN que están disponibles en el clúster. <pre class="pre"><code>ibmcloud ks cluster-get --cluster &lt;cluster_name_or_ID&gt; --showResources</code></pre><p>Salida de ejemplo:</p>
  <pre class="screen"><code>Subnet VLANs
VLAN ID   Subnet CIDR         Public   User-managed
229xxxx   169.xx.xxx.xxx/29   true     false
229xxxx   10.xxx.xx.x/29      false    false</code></pre></li>
  <li>Compruebe que los ID de VLAN pública y privada que desea utilizar son compatibles. Para que sean compatibles, el <strong>Router</strong> debe tener el mismo ID de pod.<pre class="pre"><code>ibmcloud ks vlans --zone &lt;zone&gt;</code></pre><p>Salida de ejemplo:</p>
  <pre class="screen"><code>ID        Name   Number   Type      Router         Supports Virtual Workers
229xxxx          1234     private   bcr01a.dal12   true
229xxxx          5678     public    fcr01a.dal12   true</code></pre><p>Observe que los ID de pod de <strong>Router</strong> coinciden: `01a` y `01a`. Si un ID de pod fuese `01a` y el otro fuese `02a`, no se podrían establecer estos ID de VLAN pública y privada para la agrupación de trabajadores.</p></li>
  <li>Si no tiene ninguna VLAN disponible, puede <a href="/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans">solicitar nuevas VLAN</a>.</li></ol>

  <strong>Opciones del mandato</strong>:

  <dl>
    <dt><code>--zone <em>ZONE</em></code></dt>
      <dd>La zona que desea añadir. Debe ser una [zona con soporte multizona](/docs/containers?topic=containers-regions-and-zones#zones) dentro de la región del clúster. Este valor es obligatorio.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>--worker-pool <em>WORKER_POOLS</em></code></dt>
    <dd>Una lista separada por comas de agrupaciones de nodos trabajadores a las que se añade la zona. Se necesita como mínimo una agrupación de nodos trabajadores.</dd>

  <dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
    <dd>El ID de la VLAN privada. Este valor es necesario, tanto si desea utilizar la misma VLAN privada que la que ha utilizado para los otros nodos trabajadores como si desea utilizar una distinta. Los nuevos nodos trabajadores se añaden a la VLAN que especifique, pero las VLAN de los nodos trabajadores existentes no se modifican.<p class="note">Las VLAN públicas y privadas deben ser compatibles, lo que se puede determinar por el prefijo de ID de **Router**.</p></dd>

  <dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
    <dd>ID de la VLAN pública. Este valor es obligatorio solo si desea cambiar la VLAN pública para la zona. Para cambiar la VLAN pública, siempre debe proporcionar una VLAN privada compatible. Los nuevos nodos trabajadores se añaden a la VLAN que especifique, pero las VLAN de los nodos trabajadores existentes no se modifican.<p class="note">Las VLAN públicas y privadas deben ser compatibles, lo que se puede determinar por el prefijo de ID de **Router**.</p></dd>

  <dt><code>-f</code></dt>
    <dd>Forzar que el mandato se ejecute sin solicitudes de usuario. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

  **Ejemplo**:

  ```
  ibmcloud ks zone-network-set --zone dal10 --cluster my_cluster --worker-pools pool1,pool2,pool3 --private-vlan 2294021
  ```
  {: pre}

### ibmcloud ks zone-rm
{: #cs_zone_rm}

**Solo clústeres multizona**: elimine una zona de todas las agrupaciones de nodos trabajadores del clúster. Se suprimen todos los nodos trabajadores en la agrupación de nodos trabajadores para esta zona.
{: shortdesc}

```
ibmcloud ks zone-rm --zone ZONE --cluster CLUSTER [-f] [-s]
```
{: pre}

Antes de eliminar una zona, asegúrese de que tiene suficientes nodos trabajadores en otras zonas del clúster para que sus pods se puedan volver a planificar para evitar un tiempo de inactividad para la app o que los datos del nodo trabajador resulten dañados.
{: tip}

<strong>Permisos mínimos necesarios</strong>: rol de plataforma **Operador** para el clúster en {{site.data.keyword.containerlong_notm}}

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--zone <em>ZONE</em></code></dt>
    <dd>La zona que desea añadir. Debe ser una [zona con soporte multizona](/docs/containers?topic=containers-regions-and-zones#zones) dentro de la región del clúster. Este valor es obligatorio.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>-f</code></dt>
    <dd>Forzar la actualización sin solicitudes de usuario. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

  ```
  ibmcloud ks zone-rm --zone dal10 --cluster my_cluster
  ```
  {: pre}


