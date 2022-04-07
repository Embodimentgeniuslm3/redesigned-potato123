---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, oks, iro, openshift, red hat, red hat openshift, rhos

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

# Guía de aprendizaje: Creación de un clúster de Red Hat OpenShift on IBM Cloud (beta)
{: #openshift_tutorial}

Red Hat OpenShift on IBM Cloud está disponible como versión beta para probar clústeres de OpenShift. No todas las características de {{site.data.keyword.containerlong}} están disponibles durante la versión beta. Además, los clústeres beta de OpenShift que cree solo duran 30 días después de que finalice la versión beta y Red Hat OpenShift on IBM Cloud esté disponible a nivel general.
{: preview}

Con la versión **beta de Red Hat OpenShift on IBM Cloud**, puede crear clústeres de {{site.data.keyword.containerlong_notm}} con nodos trabajadores que vienen instalados con el software de la plataforma de orquestación de contenedores OpenShift. Obtiene todas las [ventajas de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-responsibilities_iks) gestionado para su entorno de infraestructura de clúster, al tiempo que utiliza las [herramientas y el catálogo de OpenShift ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.openshift.com/container-platform/3.11/welcome/index.html) que se ejecuta en Red Hat Enterprise Linux para los despliegues de sus apps.
{: shortdesc}

Los nodos trabajadores de OpenShift solo están disponibles para los clústeres estándares. Red Hat OpenShift on IBM Cloud solo da soporte a OpenShift versión 3.11, que incluye Kubernetes versión 1.11.
{: note}

## Objetivos
{: #openshift_objectives}

En las lecciones de la guía de aprendizaje, creará un clúster estándar de Red Hat OpenShift on IBM Cloud, abrirá la consola de OpenShift, accederá a los componentes incorporados de OpenShift, desplegará una app que utiliza servicios de {{site.data.keyword.Bluemix_notm}} en un proyecto OpenShift y expondrá la app en una ruta de OpenShift para que los usuarios externos puedan acceder al servicio.
{: shortdesc}

Esta página también incluye información sobre la arquitectura de clúster de OpenShift, las limitaciones de la versión beta y sobre cómo enviar comentarios y obtener soporte.

## Tiempo necesario
{: #openshift_time}
45 minutos

## Público
{: #openshift_audience}

Esta guía de aprendizaje está destinada a los administradores de clústeres que deseen aprender a crear un clúster de Red Hat OpenShift on IBM Cloud por primera vez.
{: shortdesc}

## Requisitos previos
{: #openshift_prereqs}

*   Asegúrese de tener las siguientes políticas de acceso de {{site.data.keyword.Bluemix_notm}} IAM.
    *   El [rol de plataforma de **Administrador**](/docs/containers?topic=containers-users#platform) para {{site.data.keyword.containerlong_notm}}
    *   El [rol de servicio de **Escritor** o de **Gestor**](/docs/containers?topic=containers-users#platform) para {{site.data.keyword.containerlong_notm}}
    *   El [rol de plataforma de **Administrador**](/docs/containers?topic=containers-users#platform) para {{site.data.keyword.registrylong_notm}}
*    Asegúrese de que la [clave de API](/docs/containers?topic=containers-users#api_key) correspondiente al grupo de recursos y a la región de {{site.data.keyword.Bluemix_notm}} está configurada con los permisos correctos de infraestructura, **Superusuario**, o con los [roles mínimos](/docs/containers?topic=containers-access_reference#infra) para crear un clúster.
*   Instale las herramientas de línea de mandatos.
    *   [Instale la CLI de {{site.data.keyword.Bluemix_notm}} (`ibmcloud`), el plugin {{site.data.keyword.containershort_notm}} (`ibmcloud ks`) y el plugin {{site.data.keyword.registryshort_notm}} (`ibmcloud cr`)](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).
    *   [Instale las CLI de OpenShift Origin (`oc`) y de Kubernetes (`kubectl`)](/docs/containers?topic=containers-cs_cli_install#cli_oc).

<br />


## Visión general de la arquitectura
{: #openshift_architecture}

En el siguiente diagrama y tabla se describen los componentes predeterminados que se configuran en una arquitectura de Red Hat OpenShift on IBM Cloud.
{: shortdesc}

![Arquitectura de clúster de Red Hat OpenShift on IBM Cloud](images/cs_org_ov_both_ses_rhos.png)

| Componentes maestros| Descripción |
|:-----------------|:-----------------|
| Réplicas | Los componentes maestros, incluido el servidor de API de Kubernetes de OpenShift y el almacén de datos etcd, tienen tres réplicas y, si se encuentran en un área metropolitana multizona, se distribuyen entre las zonas para ofrecer una mayor disponibilidad. Se hace copia de seguridad de los componentes maestros cada 8 horas.|
| `rhos-api` | El servidor de API de Kubernetes de OpenShift sirve como punto de entrada principal para todas las solicitudes de gestión del clúster procedentes del nodo trabajador destinadas al nodo maestro. El servidor de API valida y procesa las solicitudes que cambian el estado de los recursos de Kubernetes, como pods o servicios, y guarda este estado en el almacén de datos etcd.|
| `openvpn-server` | El servidor OpenVPN funciona con el cliente OpenVPN para conectar de forma segura el nodo maestro con el nodo trabajador. Esta conexión admite llamadas `apiserver proxy` a los pods y servicios, y llamadas `kubectl exec`, `attach` y `logs` a kubelet.|
| `etcd` | etcd es un almacén de valores de claves de alta disponibilidad que almacena el estado de todos los recursos de Kubernetes de un clúster, como servicios, despliegues y pods. Se realiza una copia de seguridad de los datos de etcd en una instancia de almacenamiento cifrada que gestiona IBM.|
| `rhos-controller` | El gestor del controlador de OpenShift observa los pods recién creados y decide dónde se desplegarán en función de la capacidad, las necesidades de rendimiento, las restricciones de políticas, las especificaciones de antiafinidad y los requisitos de la carga de trabajo. Si no se encuentra ningún nodo trabajador que se ajuste a los requisitos, el pod no se despliega en el clúster. El controlador también observa el estado de los recursos de clúster, como por ejemplo los conjuntos de réplicas. Cuando cambia el estado de un recurso, por ejemplo si cae un pod de un conjunto de réplicas, el gestor del controlador inicia acciones de corrección para conseguir el estado necesario. El `rhos-controller` funciona como planificador y como gestor de controlador en una configuración de Kubernetes nativa. |
| `cloud-controller-manager` | El gestor de controlador de nube gestiona componentes específicos del proveedor de la nube, como por ejemplo el equilibrador de carga de {{site.data.keyword.Bluemix_notm}}.|
{: caption="Tabla 1. Componentes maestros de OpenShift." caption-side="top"}
{: #rhos-components-1}
{: tab-title="Master"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

| Componentes de nodo trabajador| Descripción |
|:-----------------|:-----------------|
| Sistema operativo | Los nodos trabajadores de Red Hat OpenShift on IBM Cloud se ejecutan en el sistema operativo Red Hat Enterprise Linux 7 (RHEL 7). |
| Proyectos | OpenShift organiza los recursos en proyectos, que son espacios de nombres de Kubernetes con anotaciones, e incluye muchos más componentes que los clústeres Kubernetes nativos para ejecutar características de OpenShift, como el catálogo. En las siguientes filas se describen determinados componentes de los proyectos. Para obtener más información, consulte el apartado sobre [Proyectos y usuarios ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.openshift.com/container-platform/3.11/architecture/core_concepts/projects_and_users.html).|
| `kube-system` | Este espacio de nombres incluye muchos componentes que se utilizan para ejecutar Kubernetes en el nodo trabajador.<ul><li>**`ibm-master-proxy`**: `ibm-master-proxy` es un conjunto de daemons que reenvía solicitudes procedentes del nodo trabajador a las direcciones IP de las réplicas del nodo maestro de alta disponibilidad. En clústeres de una sola zona, el maestro tiene tres réplicas en distintos hosts. Para clústeres que se encuentran en una zona con capacidad multizona, el maestro tiene tres réplicas que se dispersan entre zonas. Un equilibrador de carga de alta disponibilidad reenvía las solicitudes dirigidas al nombre de dominio maestro a las réplicas maestras.</li><li>**`openvpn-client`**: el cliente OpenVPN funciona con el servidor OpenVPN para conectar de forma segura el nodo maestro con el nodo trabajador. Esta conexión admite llamadas `apiserver proxy` a los pods y servicios, y llamadas `kubectl exec`, `attach` y `logs` a kubelet.</li><li>**`kubelet`**: el kubelet es un agente de nodo trabajador que se ejecuta en cada nodo trabajador y que es el responsable de supervisar el estado de los pods que se ejecutan en el nodo trabajador y de ver los sucesos que envía el servidor de API de Kubernetes. Basándose en los sucesos, el kubelet crea o elimina pods, garantiza sondeos de actividad y de preparación e informa sobre el estado de los pods al servidor de API de Kubernetes.</li><li>**`calico`**: Calico gestiona las políticas de red para el clúster, e incluye algunos componentes para gestionar la conectividad de red de los contenedores, la asignación de direcciones IP y el control del tráfico de red.</li><li>**Otros componentes**: el espacio de nombres `kube-system` también incluye componentes para gestionar los recursos proporcionados por IBM, como por ejemplo los plugins de almacenamiento para el almacenamiento de archivos y en bloque, el equilibrador de carga de aplicación (ALB) de Ingress, el registro de `fluentd` y `keepalived`.</li></ul>|
| `ibm-system` | Este espacio de nombres incluye el despliegue `ibm-cloud-provider-ip` que funciona con `keepalived` para proporcionar comprobación de estado y equilibrio de carga de capa 4 para las solicitudes dirigidas a los pods de la app.|
| `kube-proxy-and-dns`| Este espacio de nombres incluye los componentes para validar el tráfico de red de entrada con las reglas de `iptables` configuradas en el nodo trabajador y envía por proxy las solicitudes que tienen permiso para entrar en el clúster o para salir del mismo.|
| `default` | Este espacio de nombres se utiliza si no especifica un espacio de nombres o crea un proyecto para sus recursos de Kubernetes. Además, el espacio de nombres default incluye los componentes siguientes para dar soporte a los clústeres de OpenShift.<ul><li>**`router`**: OpenShift utiliza [rutas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html) para exponer un servicio de la app en un nombre de host para que los clientes externos puedan acceder al servicio. El direccionador correlaciona el servicio con el nombre de host.</li><li>**`docker-registry`** y **`registry-console`**: OpenShift proporciona un [registro de imágenes de contenedor ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.openshift.com/container-platform/3.11/install_config/registry/index.html) interno que puede utilizar para gestionar y ver localmente las imágenes mediante la consola. Como alternativa, puede configurar el {{site.data.keyword.registrylong_notm}} privado.</li></ul>|
| Otros proyectos | Otros componentes se instalan en diversos espacios de nombres de forma predeterminada para habilitar diversas funciones, como el registro, la supervisión y la consola de OpenShift.<ul><li><code>ibm-cert-store</code></li><li><code>kube-public</code></li><li><code>kube-service-catalog</code></li><li><code>openshift</code></li><li><code>openshift-ansible-service-broker</code></li><li><code>openshift-console</code></li><li><code>openshift-infra</code></li><li><code>openshift-monitoring</code></li><li><code>openshift-node</code></li><li><code>openshift-template-service-broker</code></li><li><code>openshift-web-console</code></li></ul>|
{: caption="Tabla 2. Componentes de nodo trabajador de OpenShift." caption-side="top"}
{: #rhos-components-2}
{: tab-title="Worker nodes"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

<br />


## Lección 1: Creación de un clúster de Red Hat OpenShift on IBM Cloud
{: #openshift_create_cluster}

Puede crear un clúster de Red Hat OpenShift on IBM Cloud en {{site.data.keyword.containerlong_notm}} mediante la [consola](#openshift_create_cluster_console) o la [CLI](#openshift_create_cluster_cli). Para obtener más información sobre los componentes que se configuran cuando se crea un clúster, consulte la [Visión general de la arquitectura](#openshift_architecture). OpenShift solo está disponible para clústeres estándares. Encontrará más información sobre el precio de los clústeres estándares en las [preguntas frecuentes](/docs/containers?topic=containers-faqs#charges).
{:shortdesc}

Solo puede crear clústeres en el grupo de recursos **default**. Los clústeres de OpenShift que cree durante la versión beta duran 30 días después de que finalice la versión beta y Red Hat OpenShift on IBM Cloud esté disponible a nivel general.
{: important}

### Creación de un clúster con la consola
{: #openshift_create_cluster_console}

Cree un clúster estándar de OpenShift en la consola de {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Antes de empezar, [complete los requisitos previos](#openshift_prereqs) para asegurarse de que dispone de los permisos adecuados para crear un clúster.

1.  Cree un clúster.
    1.  Inicie una sesión en su cuenta de [{{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/).
    2.  Desde el menú de hamburguesa ![icono de menú de hamburguesa](../icons/icon_hamburger.svg "icono de menú de hamburguesa"), seleccione **Kubernetes** y pulse **Crear clúster**.
    3.  Elija los detalles y el nombre de la configuración del clúster. Para la versión beta, los clústeres de OpenShift solo están disponibles como clústeres estándares ubicados en los centros de datos de Washington, DC y Londres.
        *   Para **Seleccionar un plan**, elija **Estándar**.
        *   Para **Grupo de recursos**, debe utilizar el valor **default**.
        *   Para la **Ubicación**, establezca la geografía en **América del Norte** o en **Europa**, seleccione una disponibilidad de **Zona única** o **Multizona** y, a continuación, seleccione las zonas de nodos trabajadores **Washington, DC** o **Londres**.
        *   Para **Agrupación de nodos trabajadores predeterminada**, seleccione la versión del clúster de **OpenShift**. Red Hat OpenShift on IBM Cloud solo da soporte a OpenShift versión 3.11, que incluye Kubernetes versión 1.11. Elija un tipo disponible para los nodos trabajadores; lo ideal es que tengan al menos cuatro núcleos de 16 GB de RAM.
        *   Establezca el número de nodos trabajadores que desea crear por zona, como por ejemplo 3.
    4.  Para finalizar, pulse **Crear clúster**.<p class="note">El proceso de creación del clúster puede tardar un rato en completarse. Cuando el estado del clúster sea **Normal**, la red del clúster y los componentes de equilibrio de carga tardan unos 10 minutos más en desplegarse y en actualizar el dominio de clúster que utiliza para la consola web de OpenShift y otras rutas. Espere hasta que el clúster esté listo antes de continuar con el paso siguiente comprobando que el **Subdominio de Ingress** sigue un patrón de `<cluster_name>.<region>.containers.appdomain.cloud`.</p>
2.  En la página de detalles del clúster, pulse **Consola web de OpenShift**.
3.  En el menú desplegable de la barra de menús de la plataforma de contenedor de OpenShift, pulse **Consola de aplicación**. La consola de aplicación muestra todos los espacios de nombres de proyectos del clúster. Puede ir a un espacio de nombres para ver las aplicaciones, las compilaciones y otros recursos de Kubernetes.
4.  Para completar la lección siguiente trabajando en el terminal, pulse el perfil **IAM#user.name@email.com > Copia mandato de inicio de sesión**. Pegue el mandato de inicio de sesión `oc` que ha copiado en el terminal para autenticarse mediante la CLI.

### Creación de un clúster con la CLI
{: #openshift_create_cluster_cli}

Cree un clúster estándar de OpenShift mediante la CLI de {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

Antes de empezar, [complete los requisitos previos](#openshift_prereqs) para asegurarse de que tiene los permisos adecuados para crear un clúster, la CLI de `ibmcloud` y los plugins, así como las CLI `oc` y `kubectl`.

1.  Inicie una sesión en la cuenta que ha configurado para crear clústeres de OpenShift. Elija como destino la región **us-east** o **eu-gb** y el grupo de recursos **default**. Si tiene una cuenta federada, incluya el distintivo `--sso`.
    ```
    ibmcloud login -r (us-east|eu-gb) -g default [--sso]
    ```
    {: pre}
2.  Cree un clúster.
    ```
    ibmcloud ks cluster-create --name <name> --location <zone> --kube-version <openshift_version> --machine-type <worker_node_flavor> --workers <number_of_worker_nodes_per_zone> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    Mandato de ejemplo para crear un clúster con tres nodos trabajadores que tienen cuatro núcleos y 16 GB de memoria en Washington, DC.

    ```
    ibmcloud ks cluster-create --name my_openshift --location wdc04 --kube-version 3.11_openshift --machine-type b3c.4x16.encrypted  --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    <table summary="La primera fila de la tabla abarca ambas columnas. El resto de las filas se debe leer de izquierda a derecha, con el componente del mandato en la columna uno y la descripción correspondiente en la columna dos.">
    <caption>Componentes de cluster-create</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>Mandato para crear un clúster de la infraestructura clásica en la cuenta de {{site.data.keyword.Bluemix_notm}}.</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>Especifique un nombre para el clúster. El nombre debe empezar por una letra, puede contener letras, números, guiones (-) y debe tener 35 caracteres como máximo. Utilice un nombre que sea exclusivo en las distintas regiones de {{site.data.keyword.Bluemix_notm}}.</td>
    </tr>
    <tr>
    <td><code>--location <em>&lt;zone&gt;</em></code></td>
    <td>Especifique la zona en la que desea crear el clúster. Para la versión beta, las zonas disponibles son `wdc04, wdc06, wdc07, lon04, lon05,` o `lon06`.</p></td>
    </tr>
    <tr>
      <td><code>--kube-version <em>&lt;openshift_version&gt;</em></code></td>
      <td>Debe elegir una versión de OpenShift soportada. Las versiones de OpenShift incluyen una versión de Kubernetes que difiere de las versiones de Kubernetes que están disponibles en los clústeres nativos de Kubernetes Ubuntu. Para ver una lista de las versiones disponibles de OpenShift, ejecute `ibmcloud ks versions`. Para crear un clúster con la versión de parche más reciente, puede especificar solo la versión principal y menor, como por ejemplo `3.11_openshift`.<br><br>Red Hat OpenShift on IBM Cloud solo da soporte a OpenShift versión 3.11, que incluye Kubernetes versión 1.11.</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;worker_node_flavor&gt;</em></code></td>
    <td>Elija un tipo de máquina. Puede desplegar los nodos trabajadores como máquinas virtuales en hardware dedicado o compartido, o como máquinas físicas en servidores nativos. Los tipos de máquinas físicas y virtuales varían según la zona en la que se despliega el clúster. Para ver una lista de los tipos de máquina disponibles, ejecute `ibmcloud ks machine-types --zone <zone>`.</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number_of_worker_nodes_per_zone&gt;</em></code></td>
    <td>El número de nodos trabajadores que desea incluir en el clúster. Supongamos que desee especificar al menos tres nodos trabajadores para que el clúster tenga recursos suficientes para ejecutar los componentes predeterminados y para ofrecer alta disponibilidad. Si no se especifica la opción <code>--workers</code>, se crea un nodo trabajador.</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>Si ya tiene una VLAN pública configurada en su cuenta de infraestructura de IBM Cloud (SoftLayer) para esta zona, escriba el ID de la VLAN pública. Para comprobar las VLAN disponibles, ejecute `ibmcloud ks vlans --zone <zone>`. <br><br>Si no tiene una VLAN pública en su cuenta, no especifique esta opción. {{site.data.keyword.containerlong_notm}} crea automáticamente una VLAN pública.</td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>Si ya tiene una VLAN privada configurada en su cuenta de infraestructura de IBM Cloud (SoftLayer) para esta zona, escriba el ID de la VLAN privada. Para comprobar las VLAN disponibles, ejecute `ibmcloud ks vlans --zone <zone>`. <br><br>Si no tiene una VLAN privada en su cuenta, no especifique esta opción. {{site.data.keyword.containerlong_notm}} crea automáticamente una VLAN privada.</td>
    </tr>
    </tbody></table>
3.  Obtenga una lista de los detalles del clúster. Revise el **Estado** del clúster, compruebe el **Subdominio de Ingress** y anote el **URL maestro**.<p class="note">El proceso de creación del clúster puede tardar un rato en completarse. Cuando el estado del clúster sea **Normal**, la red del clúster y los componentes de equilibrio de carga tardan unos 10 minutos más en desplegarse y en actualizar el dominio de clúster que utiliza para la consola web de OpenShift y otras rutas. Espere hasta que el clúster esté listo antes de continuar con el paso siguiente comprobando que el **Subdominio de Ingress** sigue un patrón de `<cluster_name>.<region>.containers.appdomain.cloud`.</p>
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
    ```
    {: pre}
4.  Descargue los archivos de configuración para conectar con el clúster.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Cuando termine la descarga de los archivos de configuración, se muestra un mandato que puede copiar y pegar para establecer la vía de acceso al archivo de configuración de
Kubernetes como variable de entorno.

    Ejemplo para OS X:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
5.  En el navegador, vaya a la dirección de su **URL maestro** y añada `/console`. Por ejemplo, `https://c0.containers.cloud.ibm.com:23652/console`.
6.  Pulse el perfil **IAM#user.name@email.com > Copiar mandato de inicio de sesión**. Pegue el mandato de inicio de sesión `oc` que ha copiado en el terminal para autenticarse mediante la CLI.<p class="tip">Guarde el URL del maestro del clúster para acceder a la consola de OpenShift más adelante. En futuras sesiones, puede omitir el paso `cluster-config` y copiar el mandato de inicio de sesión desde la consola.</p>
7.  Compruebe que los mandatos `oc` se ejecutan correctamente con el clúster comprobando la versión.

    ```
    oc version
    ```
    {: pre}

    Salida de ejemplo:

    ```
    oc v3.11.0+0cbc58b
    kubernetes v1.11.0+d4cacc0
    features: Basic-Auth

    Server https://c0.containers.cloud.ibm.com:23652
    openshift v3.11.98
    kubernetes v1.11.0+d4cacc0
    ```
    {: screen}

    Si no puede realizar operaciones que requieran permisos de administrador, como por ejemplo obtener una lista de todos los nodos trabajadores o pods de un clúster, descargue los certificados TLS y los archivos de permisos para el administrador del clúster ejecutando el mandato `ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin`.
    {: tip}

<br />


## Lección 2: Acceso a los servicios incorporados de OpenShift
{: #openshift_access_oc_services}

Red Hat OpenShift on IBM Cloud se proporciona con servicios integrados que puede utilizar como ayuda para utilizar el clúster, como la consola OpenShift, Prometheus y Grafana. En la versión beta, para acceder a estos servicios puede utilizar el host local de una [ruta ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html). Los nombres de dominio de ruta predeterminados siguen un patrón específico de clúster de `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.
{:shortdesc}

Puede acceder a las rutas incorporadas del servicio OpenShift desde la [consola](#openshift_services_console) o desde la [CLI](#openshift_services_cli). Supongamos que desea utilizar la consola para navegar por los recursos de Kubernetes de un proyecto. Mediante la CLI, puede obtener una lista de recursos, como por ejemplo rutas entre proyectos.

### Acceso a los servicios incorporados de OpenShift desde la consola
{: #openshift_services_console}
1.  Desde la consola web de OpenShift, en el menú desplegable de la barra de menús de la plataforma de contenedor de OpenShift, pulse **Consola de aplicación**.
2.  Seleccione el proyecto **default** y luego, en el panel de navegación, pulse **Aplicaciones > Pods**.
3.  Compruebe que los pods **router** tienen el estado **Running**. El direccionador funciona como punto de entrada para el tráfico de red externo. Puede utilizar el direccionador para exponer públicamente los servicios del clúster en la dirección IP externa del direccionador utilizando una ruta. El direccionador escucha en la interfaz de red de host pública, a diferencia de los pods de la app, que escuchan solo en IP privadas. El direccionador envía por proxy solicitudes externas para direccionar los nombres de host a las IP de los pods de la app identificados por el servicio que ha asociado con el nombre de host de la ruta.
4.  En el panel de navegación del proyecto **default**, pulse **Aplicaciones > Despliegues** y, a continuación, pulse el despliegue **registry-console**. Para abrir la consola de registro interno, debe actualizar el URL del proveedor para poder acceder al mismo externamente.
    1.  En el separador **Entorno** de la página de detalles de **registry-console**, busque el campo **OPENSHIFT_OAUTH_PROVIDER_URL**. 
    2. En el campo de valor, añada `-e` después de `c1`, por ejemplo `https://c1-e.eu-gb.containers.cloud.ibm.com:20399`. 
    3. Pulse **Guardar**. Ahora se puede acceder al despliegue de la consola de registro a través del punto final de API pública del nodo maestro de clúster.
    4.  En el panel de navegación del proyecto **default**, pulse **Aplicaciones > Rutas**. Para abrir la consola de registro, pulse el valor **Nombre de host**, como por ejemplo `https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.<p class="note">Para la versión beta, la consola de registro utiliza certificados TLS autofirmados, por lo que debe elegir continuar para acceder a la consola de registro. En Google Chrome, pulse **Avanzado > Continuar en < cluster_master_URL>**. Otros navegadores tienen opciones similares. Si no puede continuar con este valor, intente abrir el URL en un navegador privado.</p>
5.  En la barra de menús de la plataforma de contenedor de OpenShift, en el menú desplegable pulse **Consola del clúster**.
6.  En el panel de navegación, amplíe **Supervisión**.
7.  Pulse la herramienta de supervisión incorporada a la que desea acceder, como por ejemplo **Paneles de control**. Se abre la ruta de Grafana, `https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.<p class="note">La primera vez que acceda al nombre de host, es posible que tenga que autenticarse, por ejemplo pulsando **Iniciar sesión con OpenShift** y autorizando el acceso a la identidad de IAM.</p>

### Acceso a los servicios incorporados de OpenShift desde la CLI
{: #openshift_services_cli}

1.  En la consola web de OpenShift, pulse el perfil **IAM#user.name@email.com > Copiar mandato de inicio de sesión** y pegue el mandato de inicio de sesión en el terminal para autenticarse.
    ```
    oc login https://c1-e.<region>.containers.cloud.ibm.com:<port> --token=<access_token>
    ```
    {: pre}
2.  Verifique que el direccionador está desplegado. El direccionador funciona como punto de entrada para el tráfico de red externo. Puede utilizar el direccionador para exponer públicamente los servicios del clúster en la dirección IP externa del direccionador utilizando una ruta. El direccionador escucha en la interfaz de red de host pública, a diferencia de los pods de la app, que escuchan solo en IP privadas. El direccionador envía por proxy solicitudes externas para direccionar los nombres de host a las IP de los pods de la app identificados por el servicio que ha asociado con el nombre de host de la ruta.
    ```
    oc get svc router -n default
    ```
    {: pre}

    Salida de ejemplo:
    ```
    NAME      TYPE           CLUSTER-IP               EXTERNAL-IP     PORT(S)                      AGE
    router    LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:30399/TCP,443:32651/TCP                      5h
    ```
    {: screen}
2.  Obtenga el nombre de host de **Host/Puerto** de la ruta del servicio al que desea acceder. Por ejemplo, supongamos que desea acceder al panel de control de Grafana para comprobar las métricas sobre el uso de recursos de su clúster. Los nombres de dominio de ruta predeterminados siguen un patrón específico de clúster de `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.
    ```
    oc get route --all-namespaces
    ```
    {: pre}

    Salida de ejemplo:
    ```
    NAMESPACE                          NAME                HOST/PORT                                                                    PATH                  SERVICES            PORT               TERMINATION          WILDCARD
    default                            registry-console    registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                              registry-console    registry-console   passthrough          None
    kube-service-catalog               apiserver           apiserver-kube-service-catalog.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                        apiserver           secure             passthrough          None
    openshift-ansible-service-broker   asb-1338            asb-1338-openshift-ansible-service-broker.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud            asb                 1338               reencrypt            None
    openshift-console                  console             console.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                                              console             https              reencrypt/Redirect   None
    openshift-monitoring               alertmanager-main   alertmanager-main-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                alertmanager-main   web                reencrypt            None
    openshift-monitoring               grafana             grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                          grafana             https              reencrypt            None
    openshift-monitoring               prometheus-k8s      prometheus-k8s-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                   prometheus-k8s      web                reencrypt
    ```
    {: screen}
3.  **Actualización única de un registro**: para que se pueda acceder a la consola de registro desde Internet, edite el despliegue de `registry-console` para que utilice el punto final de API pública del nodo maestro del clúster como URL del proveedor de OpenShift. El punto final de API pública tiene el mismo formato que el punto final de API privada, pero incluye un distintivo `-e` adicional en el URL.
    ```
    oc edit deploy registry-console -n default
    ```
    {: pre}
    
    En el campo `Pod Template.Containers.registry-console.Environment.OPENSHIFT_OAUTH_PROVIDER_URL`, añada `-e` después de `c1`, como en `https://ce.eu-gb.containers.cloud.ibm.com:20399`.
    ```
    Name:                   registry-console
    Namespace:              default
    ...
    Pod Template:
      Labels:  name=registry-console
      Containers:
       registry-console:
        Image:      registry.eu-gb.bluemix.net/armada-master/iksorigin-registrconsole:v3.11.98-6
        ...
        Environment:
          OPENSHIFT_OAUTH_PROVIDER_URL:  https://c1-e.eu-gb.containers.cloud.ibm.com:20399
          ...
    ```
    {: screen}
4.  En el navegador web, abra la ruta a la que desea acceder, como por ejemplo `https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`. La primera vez que acceda al nombre de host, es posible que tenga que autenticarse, por ejemplo pulsando **Iniciar sesión con OpenShift** y autorizando el acceso a la identidad de IAM.

<br>
¡Ya está en la app OpenShift integrada! Por ejemplo, si está en Grafana, puede comprobar el uso de la CPU de espacio de nombres u otros gráficos. Para acceder a otras herramientas incorporadas, abra los nombres de host de la ruta.

<br />


## Lección 3: Despliegue de una app en el clúster de OpenShift
{: #openshift_deploy_app}

Con Red Hat OpenShift on IBM Cloud, puede crear una nueva app y exponer el servicio de la app a través de un direccionador de OpenShift para que la puedan utilizar usuarios externos.
{: shortdesc}

Si se ha tomado un descanso después de la última lección y ha iniciado un nuevo terminal, asegúrese de volver a iniciar la sesión en el clúster. Abra la consola de OpenShift en `https://<master_URL>/console`. Por ejemplo, `https://c0.containers.cloud.ibm.com:23652/console`. Luego pulse el perfil **IAM#user.name@email.com > Copiar mandato de inicio de sesión** y pegue el mandato de inicio de sesión `oc` que ha copiado en el terminal para autenticarse mediante la CLI.
{: tip}

1.  Cree un proyecto para la app Hello World. Un proyecto es una versión de OpenShift de un espacio de nombres de Kubernetes con anotaciones adicionales.
    ```
    oc new-project hello-world
    ```
    {: pre}
2.  Cree la app de ejemplo [a partir del código fuente ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/IBM/container-service-getting-started-wt). Con el mandato de OpenShift `new-app`, puede hacer referencia a un directorio en un repositorio remoto que contenga el Dockerfile y el código de la app para crear la imagen. El mandato crea la imagen, almacena la imagen en el registro de Docker local y crea las configuraciones de despliegue de la app (`dc`) y los servicios (`svc`). Para obtener información sobre cómo crear apps nuevas, [consulte la documentación de OpenShift ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.openshift.com/container-platform/3.11/dev_guide/application_lifecycle/new_app.html).
    ```
    oc new-app --name hello-world https://github.com/IBM/container-service-getting-started-wt --context-dir="Lab 1"
    ```
    {: pre}
3.  Compruebe que se han creado los componentes de la app Hello World de ejemplo.
    1.  Compruebe que existe la imagen **hello-world** en el registro de Docker integrado del clúster accediendo a la consola de registro en el navegador. Asegúrese de que ha actualizado el URL del proveedor de la consola de registro con `-e` tal como se ha descrito en la lección anterior.
        ```
        https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud/
        ```
        {: codeblock}
    2.  Obtenga una lista de los servicios **hello-world** y anote el nombre del servicio. La app escucha el tráfico en estas direcciones IP internas del clúster a menos que cree una ruta para el servicio de modo que el direccionador pueda reenviar solicitudes de tráfico externas a la app.
        ```
        oc get svc -n hello-world
        ```
        {: pre}

        Salida de ejemplo:
        ```
        NAME          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
        hello-world   ClusterIP   172.21.xxx.xxx   <nones>       8080/TCP   31m
        ```
        {: screen}
    3.  Obtenga una lista de los pods. Los pods que contienen `build` en su nombre son trabajos que se han **completado** como parte del proceso de compilación de la nueva app. Asegúrese de que el estado del pod **hello-world** es **Running**.
        ```
        oc get pods -n hello-world
        ```
        {: pre}

        Salida de ejemplo:
        ```
        NAME                READY     STATUS             RESTARTS   AGE
        hello-world-9cv7d   1/1       Running            0          30m
        hello-world-build   0/1       Completed          0          31m
        ```
        {: screen}
4.  Configure una ruta de modo que pueda acceder públicamente al servicio {{site.data.keyword.toneanalyzershort}}. De forma predeterminada, el nombre de host está en el formato `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`. Si desea personalizar el nombre de host, incluya el distintivo `--hostname=<hostname>`.
    1.  Cree una ruta para el servicio **hello-world**.
        ```
        oc create route edge --service=hello-world -n hello-world
        ```
        {: pre}
    2.  Obtenga la dirección de nombre de host de la ruta desde la salida de **Host/Port**.
        ```
        oc get route -n hello-world
        ```
        {: pre}
        Salida de ejemplo:
        ```
        NAME          HOST/PORT                         PATH                                        SERVICES      PORT       TERMINATION   WILDCARD
        hello-world   hello-world-hello.world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud    hello-world   8080-tcp   edge/Allow    None
        ```
        {: screen}
5.  Acceda a la app.
    ```
    curl https://hello-world-hello-world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud
    ```
    {: pre}
    
    Salida de ejemplo:
    ```
    Hello world from hello-world-9cv7d! Your app is up and running in a cluster!
    ```
    {: screen}
6.  **Opcional** Para limpiar los recursos que ha creado en esta lección, puede utilizar las etiquetas asignadas a cada app.
    1.  Obtenga una lista de los recursos de cada app del proyecto `hello-world`.
        ```
        oc get all -l app=hello-world -o name -n hello-world
        ```
        {: pre}
        Salida de ejemplo:
        ```
        pod/hello-world-1-dh2ff
        replicationcontroller/hello-world-1
        service/hello-world
        deploymentconfig.apps.openshift.io/hello-world
        buildconfig.build.openshift.io/hello-world
        build.build.openshift.io/hello-world-1
        imagestream.image.openshift.io/hello-world
        imagestream.image.openshift.io/node
        route.route.openshift.io/hello-world
        ```
        {: screen}
    2.  Suprima todos los recursos que ha creado.
        ```
        oc delete all -l app=hello-world -n hello-world
        ```
        {: pre}



<br />


## Lección 4: Configuración de los complementos LogDNA y Sysdig para supervisar el estado del clúster
{: #openshift_logdna_sysdig}

Puesto que de forma predeterminada OpenShift establece [restricciones de contexto de seguridad (SCC) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) más estrictas que Kubernetes nativo, es posible que algunas apps o complementos de clúster que utilice en Kubernetes nativo no se puedan desplegar en OpenShift del mismo modo. En concreto, muchas imágenes se tienen que ejecutar como usuario `root` o como contenedor con privilegios, lo que se impide en OpenShift de forma predeterminada. En esta lección, aprenderá a modificar las SCC predeterminadas mediante la creación de cuentas de seguridad con privilegios y la actualización de `securityContext` en la especificación del pod para que utilice dos populares complementos de {{site.data.keyword.containerlong_notm}}: {{site.data.keyword.la_full_notm}} y {{site.data.keyword.mon_full_notm}}.
{: shortdesc}

Antes de empezar, inicie una sesión en el clúster como administrador.
1.  Abra la consola de OpenShift en `https://<master_URL>/console`. Por ejemplo, `https://c0.containers.cloud.ibm.com:23652/console`.
2.  Pulse el perfil **IAM#user.name@email.com > Copiar mandato de inicio de sesión** y pegue el mandato de inicio de sesión `oc` que ha copiado en el terminal para autenticarse mediante la CLI.
3.  Descargue los archivos de configuración de administrador para el clúster.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin
    ```
    {: pre}
    
    Cuando termine la descarga de los archivos de configuración, se muestra un mandato que puede copiar y pegar para establecer la vía de acceso al archivo de configuración de
Kubernetes como variable de entorno.

    Ejemplo para OS X:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
4.  Continúe con la lección para configurar [{{site.data.keyword.la_short}}](#openshift_logdna) y [{{site.data.keyword.mon_short}}](#openshift_sysdig).

### Lección 4a: Configuración de LogDNA
{: #openshift_logdna}

Configure un proyecto y una cuenta de servicio con privilegios para {{site.data.keyword.la_full_notm}}. A continuación, cree una instancia de {{site.data.keyword.la_short}} en la cuenta de {{site.data.keyword.Bluemix_notm}}. Para integrar la instancia de {{site.data.keyword.la_short}} con el clúster OpenShift, debe modificar el conjunto de daemons que se ha desplegado para utilizar la cuenta de servicio con privilegios para que se ejecute como root.
{: shortdesc}

1.  Configure el proyecto y la cuenta de servicio con privilegios para LogDNA.
    1.  Como administrador del clúster, cree un proyecto `logdna`.
        ```
        oc adm new-project logdna
        ```
        {: pre}
    2.  Elija como destino el proyecto de modo que los recursos siguientes que cree estén en el espacio de nombres del proyecto `logdna`.
        ```
        oc project logdna
        ```
        {: pre}
    3.  Cree una cuenta de servicio para el proyecto `logdna`.
        ```
        oc create serviceaccount logdna
        ```
        {: pre}
    4.  Añada una restricción de contexto de seguridad con privilegios a la cuenta de servicio para el proyecto `logdna`.<p class="note">Si desea comprobar qué autorización otorga la política de SCC `con privilegios` a la cuenta de servicio, ejecute `oc describe scc privileged`. Para obtener más información sobre las SCC, consulte la [documentación de OpenShift ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html).</p>
        ```
        oc adm policy add-scc-to-user privileged -n logdna -z logdna
        ```
        {: pre}
2.  Cree la instancia de {{site.data.keyword.la_full_notm}} en el mismo grupo de recursos que el clúster. Seleccione un plan de precios que determine el periodo de retención de los registros, como por ejemplo `lite`, que conserva los registros durante 0 días. No es necesario que la región coincida con la región del clúster. Para obtener más información, consulte [Suministro de una instancia](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-provision) y [Planes de precios](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about#overview_pricing_plans).
    ```
    ibmcloud resource service-instance-create <service_instance_name> logdna (lite|7-days|14-days|30-days) <region> [-g <resource_group>]
    ```
    {: pre}
    
    Mandato de ejemplo:
    ```
    ibmcloud resource service-instance-create logdna-openshift logdna lite us-south
    ```
    {: pre}
    
    En la salida, anote el **ID** de la instancia de servicio, que está en el formato `crn:v1:bluemix:public:logdna:<region>:<ID_string>::`.
    ```
    Service instance <name> was created.
                 
    Name:         <name>   
    ID:           crn:v1:bluemix:public:logdna:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
3.  Obtenga la clave de ingestión de la instancia de {{site.data.keyword.la_short}}. La clave de ingestión de LogDNA se utiliza para abrir un socket web seguro con el servidor de ingestión LogDNA y para autenticar el agente de registro con el servicio {{site.data.keyword.la_short}}.
    1.  Cree una clave de servicio para la instancia de LogDNA.
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <logdna_instance_ID>
        ```
        {: pre}
    2.  Anote el valor **ingestion_key** de la clave de servicio.
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        Salida de ejemplo:
        ```
        Name:          <key_name>  
        ID:            crn:v1:bluemix:public:logdna:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>     
                       ingestion_key:            111a11aa1a1a11a1a11a1111aa1a1a11    
        ```
        {: screen}
4.  Cree un secreto de Kubernetes para guardar la clave de ingestión de LogDNA correspondiente a la instancia de servicio.
    ```
     oc create secret generic logdna-agent-key --from-literal=logdna-agent-key=<logDNA_ingestion_key>
    ```
    {: pre}
5.  Cree un conjunto de daemons Kubernetes para desplegar el agente de LogDNA en cada nodo trabajador del clúster de Kubernetes. El agente de LogDNA recopila los registros con la extensión `*.log` y los archivos sin extensión almacenados en el directorio `/var/log` de su pod. De forma predeterminada, se recopilan registros de todos los espacios de nombres, incluido `kube-system`, y se reenvían automáticamente al servicio {{site.data.keyword.la_short}}.
    ```
    oc create -f https://assets.us-south.logging.cloud.ibm.com/clients/logdna-agent-ds.yaml
    ```
    {: pre}
6.  Edite la configuración del conjunto de daemons del agente de LogDNA para que haga referencia a la cuenta de servicio que ha creado anteriormente y para establecer el contexto de seguridad en con privilegios.
    ```
    oc edit ds logdna-agent
    ```
    {: pre}
    
    En el archivo de configuración, añada las especificaciones siguientes.
    *   En `spec.template.spec`, añada `serviceAccount: logdna`.
    *   En `spec.template.spec.containers`, añada `securityContext: privileged: true`.
    *   Si ha creado la instancia de {{site.data.keyword.la_short}} en una región que no sea `us-south`, actualice los valores de las variables de entorno `spec.template.spec.containers.env` correspondientes a `LDAPIHOST` y `LDLOGHOST` con `<region>`.

    Salida de ejemplo:
    ```
    apiVersion: extensions/v1beta1
    kind: DaemonSet
    spec:
      ...
      template:
        ...
        spec:
          serviceAccount: logdna
          containers:
          - securityContext:
              privileged: true
            ...
            ent:
            - name: LOGDNA_AGENT_KEY
              valueFrom:
                secretKeyRef:
                  key: logdna-agent-key
                  name: logdna-agent-key
            - name: LDAPIHOST
              value: api.<region>.logging.cloud.ibm.com
            - name: LDLOGHOST
              value: logs.<region>.logging.cloud.ibm.com
          ...
    ```
    {: screen}
7.  Compruebe que el pod `logdna-agent` de cada nodo esté en estado **Running**.
    ```
    oc get pods
    ```
    {: pre}
8.  En la consola [Observabilidad de {{site.data.keyword.Bluemix_notm}} > Registro](https://cloud.ibm.com/observe/logging), en la fila correspondiente a su instancia de {{site.data.keyword.la_short}}, pulse **Ver LogDNA**. Se abre el panel de control de LogDNA y puede empezar a analizar sus registros.

Para obtener más información sobre cómo utilizar {{site.data.keyword.la_short}}, consulte la [documentación sobre Siguientes pasos](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube#kube_next_steps).

### Lección 4b: Configuración de Sysdig
{: #openshift_sysdig}

Cree una instancia de {{site.data.keyword.mon_full_notm}} en la cuenta de {{site.data.keyword.Bluemix_notm}}. Para integrar la instancia de {{site.data.keyword.mon_short}} con el clúster de OpenShift, debe ejecutar un script que cree un proyecto y una cuenta de servicio con privilegios para el agente de Sysdig.
{: shortdesc}

1.  Cree la instancia de {{site.data.keyword.mon_full_notm}} en el mismo grupo de recursos que el clúster. Seleccione un plan de precios que determine el periodo de retención de los registros, como por ejemplo `lite`. No es necesario que la región coincida con la región del clúster. Para obtener más información, consulte [Suministro de una instancia](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-provision).
    ```
    ibmcloud resource service-instance-create <service_instance_name> sysdig-monitor (lite|graduated-tier) <region> [-g <resource_group>]
    ```
    {: pre}
    
    Mandato de ejemplo:
    ```
    ibmcloud resource service-instance-create sysdig-openshift sysdig-monitor lite us-south
    ```
    {: pre}
    
    En la salida, anote el **ID** de la instancia de servicio, que está en el formato `crn:v1:bluemix:public:logdna:<region>:<ID_string>::`.
    ```
    Service instance <name> was created.
                 
    Name:         <name>   
    ID:           crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::   
    GUID:         <guid>   
    Location:     <region>   
    ...
    ```
    {: screen}    
2.  Obtenga la clave de acceso de la instancia de {{site.data.keyword.mon_short}}. La clave de acceso de Sysdig se utiliza para abrir un socket web seguro con el servidor de ingestión de Sysdig y para autenticar el agente de supervisión con el servicio {{site.data.keyword.mon_short}}.
    1.  Cree una clave de servicio para la instancia de Sysdig.
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <sysdig_instance_ID>
        ```
        {: pre}
    2.  Anote la **Clave de acceso de Sysdig** y el **Punto final del recopilador de Sysdig** de la clave de servicio.
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        Salida de ejemplo:
        ```
        Name:          <key_name>  
        ID:            crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::    
        Created At:    Thu Jun  6 21:31:25 UTC 2019   
        State:         active   
        Credentials:                                   
                       Sysdig Access Key:           11a1aa11-aa1a-1a1a-a111-11a11aa1aa11      
                       Sysdig Collector Endpoint:   ingest.<region>.monitoring.cloud.ibm.com      
                       Sysdig Customer Id:          11111      
                       Sysdig Endpoint:             https://<region>.monitoring.cloud.ibm.com  
                       apikey:                   <api_key_value>      
                       iam_apikey_description:   Auto-generated for key <ID>     
                       iam_apikey_name:          <key_name>       
                       iam_role_crn:             crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::<ID_string>       
        ```
        {: screen}
3.  Ejecute el script para configurar un proyecto `ibm-observe` con una cuenta de servicio con privilegios y un conjunto de daemons de Kubernetes para desplegar el agente de Sysdig en cada nodo trabajador del clúster de Kubernetes. El agente de Sysdig recopila métricas, como el uso de CPU del nodo trabajador, el uso de memoria del nodo trabajador, el tráfico HTTP de entrada y de salida de los contenedores y datos sobre diversos componentes de la infraestructura. 

    En el siguiente mandato, sustituya <sysdig_access_key> y <sysdig_collector_endpoint> por los valores de la clave de servicio que ha creado anteriormente. Para <tag>, puede asociar códigos a su agente de Sysdig, como por ejemplo `role:service,location:us-south`, lo que ayuda a identificar el entorno del que proceden las métricas.

    ```
    curl -sL https://ibm.biz/install-sysdig-k8s-agent | bash -s -- -a <sysdig_access_key> -c <sysdig_collector_endpoint> -t <tag> -ac 'sysdig_capture_enabled: false' --openshift
    ```
    {: pre}
    
    Salida de ejemplo: 
    ```
    * Detecting operating system
    * Downloading Sysdig cluster role yaml
    * Downloading Sysdig config map yaml
    * Downloading Sysdig daemonset v2 yaml
    * Creating project: ibm-observe
    * Creating sysdig-agent serviceaccount in project: ibm-observe
    * Creating sysdig-agent access policies
    * Creating sysdig-agent secret using the ACCESS_KEY provided
    * Retreiving the IKS Cluster ID and Cluster Name
    * Setting cluster name as <cluster_name>
    * Setting ibm.containers-kubernetes.cluster.id 1fbd0c2ab7dd4c9bb1f2c2f7b36f5c47
    * Updating agent configmap and applying to cluster
    * Setting tags
    * Setting collector endpoint
    * Adding additional configuration to dragent.yaml
    * Enabling Prometheus
    configmap/sysdig-agent created
    * Deploying the sysdig agent
    daemonset.extensions/sysdig-agent created
    ```
    {: screen}
        
4.  Compruebe que los pods de `sydig-agent` de cada nodo muestran que hay **1/1** pods listos y que cada pod está en el estado **Running**.
    ```
    oc get pods
    ```
    {: pre}
    
    Salida de ejemplo:
    ```
    NAME                 READY     STATUS    RESTARTS   AGE
    sysdig-agent-qrbcq   1/1       Running   0          1m
    sysdig-agent-rhrgz   1/1       Running   0          1m
    ```
    {: screen}
5.  En la consola [Observabilidad de {{site.data.keyword.Bluemix_notm}} > Supervisión](https://cloud.ibm.com/observe/logging), en la fila correspondiente a su instancia de {{site.data.keyword.mon_short}}, pulse **Ver Sysdig**. Se abre el panel de control de Sysdig y puede empezar a analizar las métricas del clúster.

Para obtener más información sobre cómo utilizar {{site.data.keyword.mon_short}}, consulte la [documentación sobre Siguientes pasos](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_next_steps).

### Opcional: Limpieza
{: #openshift_logdna_sysdig_cleanup}

Elimine las instancias de {{site.data.keyword.la_short}} y de {{site.data.keyword.mon_short}} del clúster y de la cuenta de {{site.data.keyword.Bluemix_notm}}. Tenga en cuenta que, a menos que guarde los registros y las métricas en [almacenamiento persistente](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-archiving), no podrá acceder a esta información después de suprimir las instancias de la cuenta.
{: shortdesc}

1.  Limpie las instancias de {{site.data.keyword.la_short}} y de {{site.data.keyword.mon_short}} en el clúster eliminando los proyectos que ha creado para las mismas. Cuando se suprime un proyecto, sus recursos, como las cuentas de servicio y los conjuntos de daemons, también se eliminan.
    ```
    oc delete project logdna
    ```
    {: pre}
    ```
    oc delete project ibm-observe
    ```
    {: pre}
2.  Elimine las instancias de la cuenta de {{site.data.keyword.Bluemix_notm}}.
    *   [Eliminación de una instancia de {{site.data.keyword.la_short}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-remove).
    *   [Eliminación de una instancia de {{site.data.keyword.mon_short}}](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-remove).

<br />


## Limitaciones
{: #openshift_limitations}

La versión beta de Red Hat OpenShift on IBM Cloud se ofrece con las siguientes limitaciones.
{: shortdesc}

**Clúster**:
*   Solo puede crear clústeres estándares, no clústeres gratuitos.
*   Las ubicaciones están disponibles en dos áreas metropolitanas multizona, Washington, DC y Londres. Las zonas soportadas son `wdc04, wdc06, wdc07, lon04, lon05,` y `lon06`.
*   No se puede crear un clúster con nodos trabajadores que ejecuten varios sistemas operativos, como OpenShift on Red Hat Enterprise Linux y Kubernetes nativo en Ubuntu.
*   El [programa de escalado automático de clústeres](/docs/containers?topic=containers-ca) no recibe soporte porque necesita Kubernetes versión 1.12 o posterior. OpenShift 3.11 solo incluye Kubernetes versión 1.11.



**Almacenamiento**:
*   Se da soporte al almacenamiento de objetos de nube, en bloque y de archivos de {{site.data.keyword.Bluemix_notm}}. No se da soporte al almacenamiento definido por software (SDS) de Portworx.
*   Debido al modo en que el almacenamiento de archivos NFS de {{site.data.keyword.Bluemix_notm}} configura los permisos de usuarios de Linux, es posible que reciba errores si utiliza el almacenamiento de archivos. Si es así, es posible que tenga que configurar [restricciones de contexto de seguridad de OpenShift ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) o que tenga que utilizar otro tipo de almacenamiento.

**Redes**:
*   Se utiliza Calico como proveedor de políticas de red en lugar de OpenShift SDN.

**Complementos, integraciones y otros servicios**:
*   Los complementos de {{site.data.keyword.containerlong_notm}}, como Istio, Knative y el terminal de Kubernetes, no están disponibles.
*   No está certificado que los diagramas de Helm funcionen en clústeres de OpenShift, con la excepción de {{site.data.keyword.Bluemix_notm}} Object Storage.
*   Los clústeres no se despliegan con secretos de extracción de imágenes para los dominios `icr.io` de {{site.data.keyword.registryshort_notm}}. Puede [crear sus propios secretos de extracción de imágenes](/docs/containers?topic=containers-images#other_registry_accounts), o bien puede utilizar el registro de Docker incorporado para clústeres de OpenShift.

**Apps**:
*   OpenShift configura de forma predeterminada valores de seguridad más estrictos que los de Kubernetes nativo. Para obtener más información, consulte la documentación de OpenShift sobre [Gestión de restricciones de contexto de seguridad (SCC) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html).
*   Por ejemplo, las apps configuradas para que se ejecuten como root pueden fallar, con los pods en estado `CrashLoopBackOff`. Para resolver este problema, puede modificar las restricciones de contexto de seguridad predeterminadas o puede utilizar una imagen que no se ejecute como root.
*   OpenShift se configura de forma predeterminada con un registro de Docker local. Si desea utilizar imágenes almacenadas en los nombres de dominio privados remotos de {{site.data.keyword.registrylong_notm}} `icr.io`, debe crear usted mismo los secretos para cada registro global y regional. Puede [copiar los secretos `default-<region>-icr-io`](/docs/containers?topic=containers-images#copy_imagePullSecret) del espacio de nombres `default` en el espacio de nombres del que desea extraer imágenes, o bien puede [crear su propio secreto](/docs/containers?topic=containers-images#other_registry_accounts). A continuación, [añada el secreto de extracción de imágenes](/docs/containers?topic=containers-images#use_imagePullSecret) a la configuración de despliegue o a la cuenta de servicio del espacio de nombres.
*   Se utiliza la consola de OpenShift en lugar del panel de control de Kubernetes.

<br />


## ¿Qué es lo siguiente?
{: #openshift_next}

Para obtener más información sobre cómo trabajar con sus apps y servicios de direccionamiento, consulte la [Guía del desarrollador de OpenShift](https://docs.openshift.com/container-platform/3.11/dev_guide/index.html).

<br />


## Comentarios y preguntas
{: #openshift_support}

Durante la vigencia de la versión beta, los clústeres de Red Hat OpenShift on IBM Cloud no quedan cubiertos por el soporte de IBM ni por el soporte de Red Hat. Cualquier soporte que se proporcione es para ayudarle a evaluar el producto como preparación para su disponibilidad general.
{: important}

Para realizar preguntas y enviar comentarios, publique en Slack. 
*   Si es un usuario externo, publíquelo en el canal [#openshift ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-container-service.slack.com/messages/CKCJLJCH4). 
*   Si es un empleado de IBM, utilice el canal [#iks-openshift-users ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-argonauts.slack.com/messages/CJH0UPN2D).

Si no utiliza un IBMid para la cuenta de {{site.data.keyword.Bluemix_notm}}, [solicite una invitación](https://bxcs-slack-invite.mybluemix.net/) a este Slack.
{: tip}
