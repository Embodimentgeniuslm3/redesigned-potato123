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


# Registro de cambios de CLI
{: #cs_cli_changelog}

En el terminal, se le notifica cuando están disponibles las actualizaciones de la CLI y los plugins de `ibmcloud`. Asegúrese de mantener actualizada la CLI para poder utilizar todos los mandatos y distintivos disponibles.
{:shortdesc}

Para instalar el plugin de CLI de {{site.data.keyword.containerlong}}, consulte [Instalación de la CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).

Consulte la tabla siguiente para ver un resumen de los cambios para cada versión del plugin de CLI de {{site.data.keyword.containerlong_notm}}.

<table summary="Visión general de los cambios de versión del plugin de CLI de {{site.data.keyword.containerlong_notm}} ">
<caption>Registro de cambios para el plugin de CLI de {{site.data.keyword.containerlong_notm}}</caption>
<thead>
<tr>
<th>Versión</th>
<th>Fecha de lanzamiento</th>
<th>Cambios</th>
</tr>
</thead>
<tbody>
<tr>
<td>0.3.34</td>
<td>31 de mayo de 2019</td>
<td>Añade soporte para crear clústeres de Red Hat OpenShift on IBM Cloud.<ul>
<li>Añade soporte para versiones de OpenShift en el distintivo `--kube-version` del mandato `cluster-create`. Por ejemplo, para crear un clúster de OpenShift estándar, puede pasar `--kube-version 3.11_openshift` en el mandato `cluster-create`.</li>
<li>Añade el mandato `versions` para obtener una lista de todas las versiones soportadas de Kubernetes y de OpenShift.</li>
<li>El mandato `kube-versions` queda en desuso.</li>
</ul></td>
</tr>
<tr>
<td>0.3.33</td>
<td>30 de mayo de 2019</td>
<td><ul>
<li>Añade el distintivo <code>--powershell</code> al mandato `cluster-config` para recuperar las variables de entorno de Kubernetes en formato de Windows PowerShell.</li>
<li>Los mandatos `region-get`, `region-set` y `regions` quedan en desuso. Para obtener más información, consulte [funcionalidad de punto final global](/docs/containers?topic=containers-regions-and-zones#endpoint).</li>
</ul></td>
</tr>
<tr>
<td>0.3.28</td>
<td>23 de mayo de 2019</td>
<td><ul><li>Añade el mandato [<code>ibmcloud ks alb-create</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_create) para crear los ALB de Ingress. Para obtener más información, consulte [Escalado de ALB](/docs/containers?topic=containers-ingress#scale_albs).</li>
<li>Añade el mandato [<code>ibmcloud ks infra-permissions-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get) para comprobar si a las credenciales que permiten el [acceso al portafolio de la infraestructura de IBM Cloud (SoftLayer)](/docs/containers?topic=containers-users#api_key) para el grupo de recursos y la región de destino les faltan permisos recomendados o necesarios de la infraestructura.</li>
<li>Añade el distintivo <code>--private-only</code> al mandato `zone-network-set` para desconfigurar la VLAN pública para los metadatos de la agrupación de nodos trabajadores de modo que los nodos trabajadores posteriores de dicha zona de agrupación de nodos trabajadores estén conectados únicamente a una VLAN privada.</li>
<li>Elimina el distintivo <code>--force-update</code> del mandato `worker-update`.</li>
<li>Añade la columna **ID de VLAN** a la salida de los mandatos `albs` y `alb-get`.</li>
<li>Añade la columna **Multizone Metro** a la salida del mandato `supported-locations` para indicar las zonas con capacidad multizona.</li>
<li>Añade los campos **Master State** y **Master Health** a la salida del mandato `cluster-get`. Para obtener más información, consulte [Estados maestros](/docs/containers?topic=containers-health#states_master).</li>
<li>Actualiza las traducciones del texto de ayuda.</li>
</ul></td>
</tr>
<tr>
<td>0.3.8</td>
<td>30 de abril de 2019</td>
<td>Añade soporte para la [funcionalidad de punto final global](/docs/containers?topic=containers-regions-and-zones#endpoint) en la versión `0.3`. De forma predeterminada, ahora puede ver y gestionar todos los recursos de {{site.data.keyword.containerlong_notm}} en todas las ubicaciones. No es necesario que elija como destino una región para trabajar con recursos.</li>
<ul><li>Añade el mandato [<code>ibmcloud ks supported-locations</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_supported-locations) para obtener una lista de todas las ubicaciones a las que {{site.data.keyword.containerlong_notm}} da soporte.</li>
<li>Añade el distintivo <code>--locations</code> a los mandatos `clusters` y `zones` para filtrar los recursos por una o varias ubicaciones.</li>
<li>Añade el distintivo <code>--region</code> a los mandatos `credential-set/unset/get`, `api-key-reset` y `vlan-spanning-get`. Para ejecutar estos mandatos, debe especificar una región en el distintivo `--region`.</li></ul></td>
</tr>
<tr>
<td>0.2.102</td>
<td>15 de abril de 2019</td>
<td>Añade el [grupo de mandatos `ibmcloud ks nlb-dns`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#nlb-dns) para registrar y gestionar un nombre de host para direcciones IP del equilibrador de carga de la red (NLB) y el [grupo de mandatos `ibmcloud ks nlb-dns-monitor`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor) para crear y modificar supervisores de comprobación de estado para nombres de host de NLB. Para obtener más información, consulte [Registro de los IP de NLB en un nombre de host DNS](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname_dns).
</td>
</tr>
<tr>
<td>0.2.99</td>
<td>9 de abril de 2019</td>
<td><ul>
<li>Actualiza el texto de ayuda.</li>
<li>Actualiza la versión de Go a 1.12.2.</li>
</ul></td>
</tr>
<tr>
<td>0.2.95</td>
<td>3 de abril de 2019</td>
<td><ul>
<li>Añade soporte de mantenimiento de versiones para los complementos de clúster gestionados.</li>
<ul><li>Añade el mandato [<code>ibmcloud ks addon-versions</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_addon_versions).</li>
<li>Añade el distintivo <code>--version</code> a los mandatos [ibmcloud ks cluster-addon-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable).</li></ul>
<li>Actualiza las traducciones del texto de ayuda.</li>
<li>Actualiza los enlaces cortos a la documentación en el texto de ayuda.</li>
<li>Corrige un error por el que los mensajes de error JSON se imprimían en un formato incorrecto.</li>
<li>Corrige un error por el que el uso del distintivo silencioso (`-s`) en algunos mandatos impedía que se mostraran los errores.</li>
</ul></td>
</tr>
<tr>
<td>0.2.80</td>
<td>19 de marzo de 2019</td>
<td><ul>
<li>Incorpora soporte para habilitar la [comunicación entere maestro y trabajador con puntos finales de servicio](/docs/containers?topic=containers-plan_clusters#workeruser-master) en clústeres estándares que ejecutan Kubernetes versión 1.11 o posterior en [cuentas habilitadas para VRF](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).<ul>
<li>Incorpora los distintivos `--private-service-endpoint` y `--public-service-endpoint` al mandato [<code>ibmcloud ks cluster-create</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create).</li>
<li>Incorpora los campos **Public Service Endpoint URL** (URL de punto final de servicio público) y **Private Service Endpoint URL** (URL de punto final de servicio privado) a la salida de <code>ibmcloud ks cluster-get</code>.</li>
<li>Incorpora el mandato [<code>ibmcloud ks cluster-feature-enable private-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_private_service_endpoint).</li>
<li>Incorpora el mandato [<code>ibmcloud ks cluster-feature-enable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_public_service_endpoint).</li>
<li>Incorpora el mandato [<code>ibmcloud ks cluster-feature-disable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable).</li>
</ul></li>
<li>Actualiza la documentación y la traducción.</li>
<li>Actualiza la versión de Go a 1.11.6.</li>
<li>Resuelve problemas de red intermitentes para los usuarios de macOS.</li>
</ul></td>
</tr>
<tr>
<td>0.2.75</td>
<td>14 de marzo de 2019</td>
<td><ul><li>Oculta HTML sin formato de la información de salida de error.</li>
<li>Arregla errores tipográficos en el texto de ayuda.</li>
<li>Corrige la traducción del texto de ayuda.</li>
</ul></td>
</tr>
<tr>
<td>0.2.61</td>
<td>26 de febrero de 2019</td>
<td><ul>
<li>Incorpora el mandato `cluster-pull-secret-apply`, que crea un ID de servicio de IAM para el clúster, políticas, clave de API y secretos de extracción de imágenes para que los contenedores que se ejecutan en el espacio de nombres `default` de Kubernetes puedan extraer imágenes del registro de IBM Cloud Container. Para los clústeres nuevos, los secretos de extracción de imágenes que utilizan las credenciales de IAM se crean de forma predeterminada. Utilice este mandato para actualizar los clústeres existentes o si el clúster tiene un error secreto de obtención de imagen durante la creación. Para obtener más información, consulte [la documentación](https://test.cloud.ibm.com/docs/containers?topic=containers-images#cluster_registry_auth).</li>
<li>Arregla un error por el que las anomalías de `ibmcloud ks init` hacían que se imprimiese la salida de la ayuda.</li>
</ul></td>
</tr>
<tr>
<td>0.2.53</td>
<td>19 de febrero de 2019</td>
<td><ul><li>Soluciona un error en el que se pasaba por alto la región para `ibmcloud ks api-key-reset`, `ibmcloud ks credential-get/set` e `ibmcloud ks vlan-spanning-get`.</li>
<li>Mejora el rendimiento de `ibmcloud ks worker-update`.</li>
<li>Incorpora la versión del complemento en las solicitudes `ibmcloud ks cluster-addon-enable`.</li>
</ul></td>
</tr>
<tr>
<td>0.2.44</td>
<td>8 de febrero de 2019</td>
<td><ul>
<li>Incorpora la opción `--skip-rbac` al mandato `ibmcloud ks cluster-config` para omitir la adición de roles RBAC de usuario de Kubernetes en función de los roles de acceso al servicio de {{site.data.keyword.Bluemix_notm}} IAM sobre la configuración del clúster. Incluya esta opción solo si [gestiona sus propios roles RBAC de Kubernetes](/docs/containers?topic=containers-users#rbac). Si utiliza [Roles de acceso al servicio de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-access_reference#service) para gestionar todos los usuarios de RBAC, no incluya esta opción.</li>
<li>Actualiza la versión de Go a 1.11.5.</li>
</ul></td>
</tr>
<tr>
<td>0.2.40</td>
<td>6 de febrero de 2019</td>
<td><ul>
<li>Incorpora los mandatos [<code>ibmcloud ks cluster-addons</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addons), [<code>ibmcloud ks cluster-addon-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable) e [<code>ibmcloud ks cluster-addon-disable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_disable)
para trabajar con complementos de clúster gestionado, como los complementos gestionados [Istio](/docs/containers?topic=containers-istio) y [Knative](/docs/containers?topic=containers-serverless-apps-knative) para {{site.data.keyword.containerlong_notm}}.</li>
<li>Mejora el texto de la ayuda para usuarios de {{site.data.keyword.Bluemix_dedicated_notm}} del mandato <code>ibmcloud ks vlans</code>.</li></ul></td>
</tr>
<tr>
<td>0.2.30</td>
<td>31 de enero de 2019</td>
<td>Aumenta el valor de tiempo de espera predeterminado para `ibmcloud ks cluster-config` a `500s`.</td>
</tr>
<tr>
<td>0.2.19</td>
<td>16 de enero de 2019</td>
<td><ul><li>Incorpora la variable de entorno `IKS_BETA_VERSION` para habilitar la versión beta rediseñada de la CLI del plugin {{site.data.keyword.containerlong_notm}}. Para probar la versión rediseñada, consulte [Utilización de la estructura de mandatos beta](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_beta).</li>
<li>Aumenta el valor de tiempo de espera predeterminado para `ibmcloud ks subnets` a `60s`.</li>
<li>Arreglos de errores menores y de traducción.</li></ul></td>
</tr>
<tr>
<td>0.1.668</td>
<td>18 de diciembre de 2018</td>
<td><ul><li>Cambia el punto final de API predeterminado <code>https://containers.bluemix.net</code> por <code>https://containers.cloud.ibm.com</code>.</li>
<li>Corrige un error para que las traducciones se muestren correctamente para la ayuda de mandatos y los mensajes de error.</li>
<li>Visualiza la ayuda del mandato más rápido.</li></ul></td>
</tr>
<tr>
<td>0.1.654</td>
<td>05 de diciembre de 2018</td>
<td>Actualiza la documentación y la traducción.</td>
</tr>
<tr>
<td>0.1.638</td>
<td>15 de noviembre de 2018</td>
<td>
<ul><li>Incorpora el alias [<code>ibmcloud ks cluster-refresh</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh) al mandato `apiserver-refresh`.</li>
<li>Incorpora el nombre de grupo de recursos a la salida de <code>ibmcloud ks cluster-get</code> e <code>ibmcloud ks clusters</code>.</li></ul>
</td>
</tr>
<tr>
<td>0.1.635</td>
<td>06 de noviembre de 2018</td>
<td>Incorpora mandatos para gestionar actualizaciones automáticas del complemento de clúster de ALB de Ingress:<ul>
<li>[<code>ibmcloud ks alb-autoupdate-disable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_disable)</li>
<li>[<code>ibmcloud ks alb-autoupdate-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_enable)</li>
<li>[<code>ibmcloud ks alb-autoupdate-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_get)</li>
<li>[<code>ibmcloud ks alb-rollback</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_rollback)</li>
<li>[<code>ibmcloud ks alb-update</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_update)</li>
</ul></td>
</tr>
<tr>
<td>0.1.621</td>
<td>30 de octubre de 2018</td>
<td><ul>
<li>Incorpora el mandato [<code>ibmcloud ks credential-get</code>.](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credential_get).</li>
<li>Incorpora soporte para el origen de registro <code>storage</code> a todos los mandatos de registro de clúster. Para obtener más información, consulte
<a href="/docs/containers?topic=containers-health#logging">Visión general del reenvío de registros de clúster y de app</a>.</li>
<li>Añade el distintivo `--network` al [mandato <code>ibmcloud ks cluster-config</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config), que descarga el archivo de configuración de Calico para ejecutar todos los mandatos de Calico.</li>
<li>Arreglos de errores menores y refactorización</li></ul>
</td>
</tr>
<tr>
<td>0.1.593</td>
<td>10 de octubre de 2018</td>
<td><ul><li>Incorpora el ID de grupo de recursos a la salida de <code>ibmcloud ks cluster-get</code>.</li>
<li>Cuando [{{site.data.keyword.keymanagementserviceshort}} está habilitado](/docs/containers?topic=containers-encryption#keyprotect) como proveedor de servicios de gestión de claves (KMS), añade el campo habilitado para KMS en la salida de <code>ibmcloud ks cluster-get</code>.</li></ul></td>
</tr>
<tr>
<td>0.1.591</td>
<td>2 de octubre de 2018</td>
<td>Incorpora soporte para [grupos de recursos](/docs/containers?topic=containers-clusters#cluster_prepare).</td>
</tr>
<tr>
<td>0.1.590</td>
<td>1 de octubre de 2018</td>
<td><ul>
<li>Incorpora los mandatos [<code>ibmcloud ks logging-collect</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect) e [<code>ibmcloud ks logging-collect-status</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect_status) para recopilar registros del servidor de API en el clúster.</li>
<li>Añade el [mandato <code>ibmcloud ks key-protect-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_key_protect) para habilitar {{site.data.keyword.keymanagementserviceshort}} como servicio de gestión de claves (KMS) en el clúster.</li>
<li>Incorpora el distintivo <code>--skip-master-health</code> a los mandatos [ibmcloud ks worker-reboot](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) e [ibmcloud ks worker-reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) para omitir la comprobación de estado del nodo maestro antes de realizar un reinicio o una recarga.</li>
<li>Cambia el nombre de <code>Owner Email</code> por <code>Owner</code> en la salida de <code>ibmcloud ks cluster-get</code>.</li></ul></td>
</tr>
</tbody>
</table>
