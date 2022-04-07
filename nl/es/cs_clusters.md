---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

keywords: kubernetes, iks, clusters, worker nodes, worker pools, delete

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
{:gif: data-image-type='gif'}


# Creación de clústeres
{: #clusters}

Cree un clúster de Kubernetes en {{site.data.keyword.containerlong}}.
{: shortdesc}

¿Aún se está familiarizando? Pruebe la [guía de aprendizaje para la creación de un clúster de Kubernetes](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial). ¿No está seguro de qué configuración de clúster elegir? Consulte [Planificación de la configuración de la red de clúster](/docs/containers?topic=containers-plan_clusters).
{: tip}

¿Ha creado un clúster antes y solo está buscando mandatos de ejemplo rápido? Pruebe estos ejemplos.
*  **Clúster gratuito**:
   ```
   ibmcloud ks cluster-create --name my_cluster
   ```
   {: pre}
*  **Clúster estándar, máquina virtual compartida**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **Clúster estándar, nativo**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type mb2c.4x32 --hardware dedicated --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **Clúster estándar, máquina virtual con puntos finales de servicio público y privado en una cuenta habilitada para VRF**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-service-endpoint --private-service-endpoint --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **Clúster estándar que solo utiliza VLAN privadas y el punto final de servicio privado**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --private-service-endpoint --private-vlan <private_VLAN_ID> --private-only
   ```
   {: pre}

<br />


## Preparación para crear clústeres a nivel de cuenta
{: #cluster_prepare}

Prepare su cuenta de {{site.data.keyword.Bluemix_notm}} para {{site.data.keyword.containerlong_notm}}. Es posible que no tenga que cambiar estos preparativos cada vez que cree un clúster después de que los implante el administrador de la cuenta. Sin embargo, cada vez que cree un clúster tiene que verificar que el estado actual de nivel de cuenta es el que necesita.
{: shortdesc}

1. [Cree o actualice su cuenta a una cuenta de pago (Pago según uso o Suscripción de {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/registration/)).

2. [Configure una clave de API de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-users#api_key) en las regiones en las que desea crear clústeres. Asigne la clave de API con los permisos adecuados para crear clústeres:
  * Rol de **Superusuario** para la infraestructura de IBM Cloud (SoftLayer).
  * Rol de gestión de la plataforma de **Administrador** para {{site.data.keyword.containerlong_notm}} a nivel de cuenta.
  * Rol de gestión de la plataforma de **Administrador** para {{site.data.keyword.registrylong_notm}} a nivel de cuenta. Si la cuenta es anterior al 4 de octubre de 2018, tiene que [habilitar las políticas de {{site.data.keyword.Bluemix_notm}} IAM para {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-user#existing_users). Con las políticas de IAM, puede controlar el acceso a los recursos, como por ejemplo los espacios de nombres de registro.

  ¿Es el propietario de la cuenta? Si es así, ya tiene los permisos necesarios. Cuando se crea un clúster, la clave de API correspondiente a esa región y grupo de recursos se establece con las credenciales.
  {: tip}

3. Verifique que tiene el rol de plataforma de **Administrador** para {{site.data.keyword.containerlong_notm}}. Para permitir que el clúster extraiga imágenes del registro privado, también necesita el rol de plataforma de **Administrador** de {{site.data.keyword.registrylong_notm}}.
  1. En la barra de menús de la [consola de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/), pulse **Gestionar > Acceso (IAM)**.
  2. Pulse la página **Usuarios** y, a continuación, en la tabla, selecciónese a usted mismo.
  3. En el separador **Políticas de acceso**, confirme que el **Rol** es **Administrador**. Puede ser el **Administrador** para todos los recursos de la cuenta, o al menos para {{site.data.keyword.containershort_notm}}. **Nota**: si tiene el rol de **Administrador** para {{site.data.keyword.containershort_notm}} en un solo grupo de recursos o región en lugar de en toda la cuenta, debe tener al menos el rol de **Visor** en el nivel de cuenta para ver las VLAN de la cuenta.
  <p class="tip">Asegúrese de que el administrador de la cuenta no le asigna el rol de plataforma **Administrador** al mismo tiempo que un rol de servicio. Los roles de plataforma y de servicio se deben asignar por separado.</p>

4. Si la cuenta utiliza varios grupos de recursos, debe decidir la estrategia de su cuenta para [gestionar grupos de recursos](/docs/containers?topic=containers-users#resource_groups).
  * El clúster se crea en el grupo de recursos que ha elegido como destino al iniciar la sesión en {{site.data.keyword.Bluemix_notm}}. Si no elige como destino un grupo de recursos, se establece automáticamente el grupo de recursos predeterminado.
  * Si desea crear un clúster en un grupo de recursos distinto del predeterminado, necesitará al menos el rol **Visor** para el grupo de recursos. Si no tiene ningún rol para el grupo de recursos pero es **Administrador** del servicio dentro del grupo de recursos, el clúster se crea en el grupo de recursos predeterminado.
  * No se puede cambiar el grupo de recursos de un clúster. Además, si necesita utilizar el [mandato](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind) `ibmcloud ks cluster-service-bind` para [integrar con un servicio de {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-service-binding#bind-services), este servicio debe estar en el mismo grupo de recursos que el clúster. Los servicios que no utilizan grupos de recursos, como {{site.data.keyword.registrylong_notm}}, o que no necesitan enlaces a servicios, como {{site.data.keyword.la_full_notm}}, funcionan aunque el clúster se encuentre en otro grupo de recursos distinto.
  * Si tiene previsto utilizar [{{site.data.keyword.monitoringlong_notm}} para métricas](/docs/containers?topic=containers-health#view_metrics), asigne al clúster un nombre que sea exclusivo entre todos los grupos de recursos y regiones de la cuenta para evitar conflictos de nombres de métricas.
  * Los clústeres gratuitos se crean en el grupo de recursos `default`.

5. **Clústeres estándares**: planifique la [configuración de red](/docs/containers?topic=containers-plan_clusters) del clúster para que este se ajuste a los requisitos de sus cargas de trabajo y de su entorno. Luego configure la red de la infraestructura de IBM Cloud (SoftLayer) de modo que permita la comunicación entre nodo trabajador y maestro y entre usuario y maestro:
  * Para utilizar solo el punto final de servicio privado o los puntos finales de servicio públicos y privados (para ejecutar cargas de trabajo de cara a Internet o para ampliar el centro de datos local):
    1. Habilite [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) en la cuenta de la infraestructura de IBM Cloud (SoftLayer).
    2. [Habilite la cuenta de {{site.data.keyword.Bluemix_notm}} para que utilice puntos finales de servicio](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
    <p class="note">Se puede acceder al nodo maestro de Kubernetes a través del punto final de servicio privado si los usuarios autorizados del clúster están en su red privada de {{site.data.keyword.Bluemix_notm}} o si se conectan a la red privada a través de una [conexión VPN](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) o [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). Sin embargo, la comunicación con el nodo maestro de Kubernetes sobre el punto final de servicio privado debe pasar a través del rango de direcciones IP <code>166.X.X.X</code>, que no se puede direccionar desde una conexión VPN ni a través de {{site.data.keyword.Bluemix_notm}} Direct Link. Puede exponer el punto final de servicio privado del nodo maestro para los usuarios del clúster utilizando un equilibrador de carga de red (NLB) privado. El NLB privado expone el punto final de servicio privado del nodo maestro como un rango de direcciones IP <code>10.X.X.X</code> interno al que pueden acceder los usuarios con la VPN o con la conexión de {{site.data.keyword.Bluemix_notm}} Direct Link. Si solo habilita el punto final de servicio privado, puede utilizar el panel de control de Kubernetes o puede habilitar temporalmente el punto final de servicio público para crear el NLB privado. Para obtener más información, consulte [Acceso a clústeres a través del punto final de servicio privado](/docs/containers?topic=containers-clusters#access_on_prem).</p>

  * Para utilizar únicamente el punto final de servicio público (para ejecutar cargas de trabajo de cara a Internet):
    1. Habilite la [distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) para la cuenta de infraestructura de IBM Cloud (SoftLayer) para que los nodos trabajadores puedan comunicarse entre sí en la red privada. Para llevar a cabo esta acción, necesita el [permiso de la infraestructura](/docs/containers?topic=containers-users#infra_access) **Red > Gestionar distribución de VLAN de red** o bien puede solicitar al propietario de la cuenta que lo habilite. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.
  * Para utilizar un dispositivo de pasarela (para ampliar el centro de datos local):
    1. Habilite la [distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) para la cuenta de infraestructura de IBM Cloud (SoftLayer) para que los nodos trabajadores puedan comunicarse entre sí en la red privada. Para llevar a cabo esta acción, necesita el [permiso de la infraestructura](/docs/containers?topic=containers-users#infra_access) **Red > Gestionar distribución de VLAN de red** o bien puede solicitar al propietario de la cuenta que lo habilite. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.
    2. Configure un dispositivo de pasarela. Por ejemplo, puede configurar un [dispositivo direccionador virtual](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) o un [dispositivo de seguridad Fortigate](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations) para que actúe como cortafuegos para permitir el tráfico necesario y bloquear el tráfico no deseado.
    3. [Abra los puertos y las direcciones IP necesarios](/docs/containers?topic=containers-firewall#firewall_outbound) para cada región de modo que el nodo maestro y los nodos trabajadores se puedan comunicar y para los servicios de {{site.data.keyword.Bluemix_notm}} que vaya a utilizar.

<br />


## Preparación para crear clústeres a nivel de clúster
{: #prepare_cluster_level}

Después de configurar la cuenta para crear clústeres, prepare la configuración del clúster. Estos preparativos afectan al clúster cada vez que se crea un clúster.
{: shortdesc}

1. Decida entre un [clúster gratuito o estándar](/docs/containers?topic=containers-cs_ov#cluster_types). Puede crear un clúster gratuito para probar algunas de las funcionalidades durante 30 días, o crear clústeres estándares totalmente personalizables con el aislamiento de hardware que elija. Cree un clúster estándar para obtener más ventajas y controlar el rendimiento del clúster.

2. Para clústeres estándares, planifique la configuración del clúster.
  * Decida si desea crear un clúster de [una sola zona](/docs/containers?topic=containers-ha_clusters#single_zone) o [multizona](/docs/containers?topic=containers-ha_clusters#multizone). Tenga en cuenta que los clústeres multizona solo están disponibles en determinadas ubicaciones.
  * Elija el tipo de [hardware y aislamiento](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) que desea para los nodos trabajadores del clúster, incluida la decisión entre máquinas virtuales o nativas.

3. Para clústeres estándares, puede [estimar el coste](/docs/billing-usage?topic=billing-usage-cost#cost) en la consola de {{site.data.keyword.Bluemix_notm}}. Para obtener más información sobre los cargos que no incluye el estimador, consulte [Precios y facturación](/docs/containers?topic=containers-faqs#charges).

4. Si crea el clúster en un entorno detrás de un cortafuegos, como en el caso de los clústeres que amplían el centro de datos local, [permita el tráfico de red de salida a las IP públicas y privadas](/docs/containers?topic=containers-firewall#firewall_outbound) para los servicios de {{site.data.keyword.Bluemix_notm}} que tiene previsto utilizar.

<br />


## Creación de un clúster gratuito
{: #clusters_free}

Puede utilizar 1 clúster gratuito para familiarizarse con el funcionamiento de {{site.data.keyword.containerlong_notm}}. Con los clústeres gratuitos puede aprender la terminología, completar una guía de aprendizaje y familiarizarse con el sistema antes de dar el salto a los clústeres estándares de nivel de producción. No se preocupe, seguirá disponiendo de un clúster gratuito aunque tenga una cuenta facturable.
{: shortdesc}

Los clústeres gratuitos incluyen un nodo de trabajador configurado con 2 vCPU y 4 GB de memoria y tienen un periodo de vida de 30 días. Transcurrido este periodo, el clúster caduca y el clúster y sus datos se suprimen. {{site.data.keyword.Bluemix_notm}} no hace copia de seguridad de los datos suprimidos y no se pueden restaurar. Asegúrese de realizar una copia de seguridad de los datos importantes.
{: note}

### Creación de un clúster gratuito en la consola
{: #clusters_ui_free}

1. En el [catálogo ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/catalog?category=containers), seleccione
**{{site.data.keyword.containershort_notm}}** para crear un clúster.
2. Seleccione el plan de clúster **Gratuito**.
3. Seleccione una geografía en la que desplegar el clúster.
4. Seleccione una ubicación metropolitana en la geografía. El clúster se crea en una zona dentro de esta zona metropolitana.
5. Asigne un nombre al clúster. El nombre debe empezar por una letra, puede contener letras, números, guiones (-) y debe tener 35 caracteres como máximo. Utilice un nombre que sea exclusivo entre regiones.
6. Pulse **Crear clúster**. De forma predeterminada, se crea una agrupación de nodos trabajadores con un nodo trabajador. Verá el progreso del despliegue del nodo trabajador en el separador **Nodos trabajadores**. Cuando finalice el despliegue, podrá ver que el clúster está listo en el separador **Visión general**. Tenga en cuenta que, aunque el clúster esté listo, algunas partes del clúster que utilizan otros servicios, como los secretos de obtención de imágenes de registro, podrían estar aún en proceso.

  A cada nodo trabajador se la asigna un ID exclusivo y un nombre de dominio que no se debe cambiar de forma manual después de haber creado el clúster. Si se cambia el nombre de dominio o el ID se impide que el maestro de Kubernetes gestione el clúster.
  {: important}
7. Una vez que se haya creado el clúster, puede [empezar a trabajar con el clúster configurando la sesión de CLI](#access_cluster).

### Creación de un clúster gratuito en la CLI
{: #clusters_cli_free}

Antes de empezar, instale la CLI de {{site.data.keyword.Bluemix_notm}} y el [plugin {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_cli_install#cs_cli_install).

1. Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}.
  1. Inicie una sesión y escriba sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite.
     ```
     ibmcloud login
     ```
     {: pre}

     Si tiene un ID federado, utilice `ibmcloud login --sso` para iniciar la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe tiene un ID federado cuando el inicio de sesión falla sin el `--sso` y se lleva a cabo correctamente con la opción `--sso`.
     {: tip}

  2. Si tiene varias cuentas de {{site.data.keyword.Bluemix_notm}}, seleccione la cuenta donde desea crear el clúster de Kubernetes.

  3. Para crear el clúster gratuito en una región específica, debe seleccionar dicha región como destino. Puede crear un clúster gratuito en `ap-south`, `eu-central`, `uk-south` o `us-south`. El clúster se crea en una zona dentro de dicha región.
     ```
     ibmcloud ks region-set
     ```
     {: pre}

2. Cree un clúster.
  ```
  ibmcloud ks cluster-create --name my_cluster
  ```
  {: pre}

3. Verifique que ha solicitado la creación del clúster. La solicitud de la máquina del nodo trabajador puede tardar unos minutos.
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    Una vez completado el suministro del clúster, el estado del clúster pasa a ser **deployed**.
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

4. Compruebe el estado del nodo trabajador.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Cuando el nodo trabajador está listo, el estado pasa a **normal** y el estado es **Ready**. Cuando el estado del nodo sea **Preparado**, podrá acceder al clúster. Tenga en cuenta que, aunque el clúster esté listo, algunas partes del clúster que utilizan otros servicios, como por ejemplo los secretos de Ingress o los secretos de obtención de imágenes de registro, podrían estar aún en proceso.
    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   free           normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    A cada nodo trabajador se la asigna un ID exclusivo y un nombre de dominio que no se debe cambiar de forma manual después de haber creado el clúster. Si se cambia el nombre de dominio o el ID se impide que el maestro de Kubernetes gestione el clúster.
    {: important}

5. Una vez que se haya creado el clúster, puede [empezar a trabajar con el clúster configurando la sesión de CLI](#access_cluster).

<br />


## Creación de un clúster estándar
{: #clusters_standard}

Utilice la CLI de {{site.data.keyword.Bluemix_notm}} o la consola de {{site.data.keyword.Bluemix_notm}} para crear un clúster estándar que se pueda personalizar por completo con el aislamiento de hardware que elija y acceso a características como varios nodos trabajadores para obtener un entorno de alta disponibilidad.
{: shortdesc}

### Creación de un clúster estándar en la consola
{: #clusters_ui}

1. En el [catálogo ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/catalog?category=containers), seleccione
**{{site.data.keyword.containershort_notm}}** para crear un clúster.

2. Seleccione el grupo de recursos en el que desea crear el clúster.
  **Nota**:
    * Un clúster solo se puede crear en un grupo de recursos y, después de crear el clúster, no puede cambiar su grupo de recursos.
    * Para crear clústeres, que no sea el predeterminado, en un grupo de recursos, debe tener al menos el [rol de **Visor**](/docs/containers?topic=containers-users#platform) para el grupo de recursos.

3. Seleccione una geografía en la que desplegar el clúster.

4. Dele un nombre exclusivo al clúster. El nombre debe empezar por una letra, puede contener letras, números, guiones (-) y debe tener 35 caracteres como máximo. Utilice un nombre que sea exclusivo entre regiones. El nombre del clúster y la región en la que el clúster se despliega forman el nombre de dominio completo para el subdominio de Ingress. Para garantizar que el subdominio de Ingress es exclusivo dentro de una región, el nombre del clúster se puede truncar y se le puede añadir un valor aleatorio al nombre de dominio de Ingress.
 **Nota**: si se cambia el nombre de dominio o el ID exclusivo asignado durante la creación, se impide que el nodo maestro de Kubernetes gestione el clúster.

5. Seleccione la disponibilidad **Una sola zona** o **Multizona**. En un clúster multizona, el nodo maestro se despliega en una zona con soporte multizona y los recursos del clúster se distribuyen entre varias zonas.

6. Especifique los detalles del área metropolitana y de la zona.
  * Clústeres multizona:
    1. Seleccione un área metropolitana. Para obtener el mejor rendimiento, seleccione la ubicación metropolitana que tenga más cerca físicamente. Las opciones pueden verse limitadas por geografía.
    2. Seleccione las zonas específicas en las que desea alojar el clúster. Debe seleccionar al menos 1 zona, pero puede seleccionar tantas como desee. Si selecciona más de 1 zona, los nodos trabajadores se distribuyen entre las zonas que elija, lo que le proporciona una mayor disponibilidad. Si selecciona solo 1 zona, puede [añadir zonas a su clúster](/docs/containers?topic=containers-add_workers#add_zone) después de crearlo.
  * Clústeres de una sola zona: seleccione una zona en la que desea alojar el clúster. Para obtener el mejor rendimiento, seleccione la zona físicamente más cercana. Las opciones pueden verse limitadas por geografía.

7. Para cada zona, seleccione las VLAN.
  * Para crear un clúster en el que pueda ejecutar cargas de trabajo de cara a Internet:
    1. Seleccione una VLAN pública y una VLAN privada en la cuenta de infraestructura de IBM Cloud (SoftLayer). Los nodos trabajadores se comunican entre sí mediante la VLAN privada y se pueden comunicar con el nodo maestro de Kubernetes mediante VLAN pública o privada. Si no tiene una VLAN pública o privada en esta zona, se crea automáticamente una VLAN pública y una VLAN privada. Puede utilizar la misma VLAN para varios clústeres.
  * Para crear un clúster que amplíe el centro de datos local solo en la red privada, que amplíe el centro de datos local con la opción de añadir acceso público limitado más adelante o que amplíe el centro de datos local y proporcione acceso público limitado a través de un dispositivo de pasarela:
    1. Seleccione una VLAN privada en la cuenta de infraestructura de IBM Cloud (SoftLayer). Los nodos trabajadores se comunican entre sí a través de la VLAN privada. Si no tiene una VLAN privada en esta zona, se crea automáticamente una VLAN privada. Puede utilizar la misma VLAN para varios clústeres.
    2. Para la VLAN pública, seleccione **Ninguna**.

8. En **Punto final de servicio maestro**, elija el modo en que se comunican el nodo maestro de Kubernetes y los nodos trabajadores.
  * Para crear un clúster en el que pueda ejecutar cargas de trabajo de cara a Internet:
    * Si VRF y los puntos finales de servicio están habilitados en la cuenta de {{site.data.keyword.Bluemix_notm}}, seleccione **Puntos finales tanto privados como públicos**.
    * Si no puede o no desea habilitar VRF, seleccione **Solo punto final público**.
  * Para crear un clúster que solo amplíe el centro de datos local o un clúster que amplíe el centro de datos local y proporcione acceso público limitado con nodos trabajadores de extremo, seleccione **Puntos finales tanto privados como públicos** o **Solo punto final privado**. Asegúrese de que ha habilitado VRF y los puntos finales de servicio en su cuenta de {{site.data.keyword.Bluemix_notm}}. Tenga en cuenta que si solo habilita el punto final de servicio privado, debe [exponer el punto final maestro a través de un equilibrador de carga de red privado](#access_on_prem) para que los usuarios puedan acceder al nodo maestro a través de una VPN o de una conexión {{site.data.keyword.BluDirectLink}}.
  * Para crear un clúster que amplíe el centro de datos local y proporcione acceso público limitado con un dispositivo de pasarela, seleccione **Solo punto final público**.

9. Configure la agrupación de nodos trabajadores predeterminada. Las agrupaciones de nodos trabajadores son grupos de nodos trabajadores que comparten la misma configuración. Siempre puede añadir más agrupaciones de nodos trabajadores a su clúster posteriormente.
  1. Elija la versión del servidor de API de Kubernetes para el nodo maestro del clúster y para los nodos trabajadores.
  2. Filtre los tipos de nodos trabajadores seleccionando un tipo de máquina. La opción virtual se factura por hora, y la nativa se factura mensualmente.
    - **Nativo**: Los servidores nativos se facturan de forma mensual y su suministro lo realiza manualmente la infraestructura de IBM Cloud (SoftLayer) después de que realice el pedido, por lo que puede tardar más de un día laborable en realizarse. Los servidores nativos son más apropiados para aplicaciones de alto rendimiento que necesitan más recursos y control de host. También puede optar por habilitar [Trusted Compute](/docs/containers?topic=containers-security#trusted_compute) para verificar que los nodos trabajadores no se manipulan de forma indebida. Trusted Compute está disponible para determinados tipos de máquinas nativas. Por ejemplo, los distintos tipos de GPU `mgXc` no dan soporte a Trusted Compute. Si no habilita la confianza al crear el clúster pero la desea posteriormente, puede utilizar el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Una vez que habilita la confianza, no puede inhabilitarla posteriormente.
    Asegúrese de que desea suministrar una máquina nativa. Puesto que se factura mensualmente, si cancela la operación inmediatamente tras realizar un pedido por error, se le cobrará el mes completo.
    {:tip}
    - **Virtual - Compartido**: Los recursos de infraestructura, como por ejemplo el hipervisor y el hardware físico, están compartidos entre usted y otros clientes de IBM, pero cada nodo trabajador es accesible sólo por usted. Aunque esta opción es menos costosa y suficiente en la mayoría de los casos, es posible que desee verificar los requisitos de rendimiento e infraestructura con las políticas de la empresa.
    - **Virtual - Dedicado**: Los nodos trabajadores están alojados en una infraestructura que está dedicada a su cuenta. Sus recursos físicos están completamente aislados.
  3. Seleccione un tipo. El tipo define la cantidad de memoria, espacio de disco y CPU virtual que se configura en cada nodo trabajador y que está disponible para todos los contenedores. Los tipos de máquinas nativas y virtuales varían según la zona en la que se despliega el clúster. Después de crear el clúster, puede añadir distintos tipos de máquina añadiendo un nodo trabajador o una agrupación al clúster.
  4. Especifique el número de nodos trabajadores que necesita en el clúster. El número de nodos trabajadores que especifique se replica entre el número de zonas que ha seleccionado. Esto significa que si tiene 2 zonas y selecciona 3 nodos trabajadores, se suministran 6 nodos y en cada zona residen 3 nodos.

10. Pulse **Crear clúster**. Se crea una agrupación de nodos trabajadores con el número de nodos trabajadores que ha especificado. Verá el progreso del despliegue del nodo trabajador en el separador **Nodos trabajadores**. Cuando finalice el despliegue, podrá ver que el clúster está listo en el separador **Visión general**. Tenga en cuenta que, aunque el clúster esté listo, algunas partes del clúster que utilizan otros servicios, como por ejemplo los secretos de Ingress o los secretos de obtención de imágenes de registro, podrían estar aún en proceso.

  A cada nodo trabajador se la asigna un ID exclusivo y un nombre de dominio que no se debe cambiar de forma manual después de haber creado el clúster. Si se cambia el nombre de dominio o el ID se impide que el maestro de Kubernetes gestione el clúster.
  {: important}

11. Una vez que se haya creado el clúster, puede [empezar a trabajar con el clúster configurando la sesión de CLI](#access_cluster).

### Creación de un clúster estándar en la CLI
{: #clusters_cli_steps}

Antes de empezar, instale la CLI de {{site.data.keyword.Bluemix_notm}} y el [plugin {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_cli_install#cs_cli_install).

1. Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}.
  1. Inicie una sesión y escriba sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite.
     ```
     ibmcloud login
     ```
     {: pre}

     Si tiene un ID federado, utilice `ibmcloud login --sso` para iniciar la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe tiene un ID federado cuando el inicio de sesión falla sin el `--sso` y se lleva a cabo correctamente con la opción `--sso`.
     {: tip}

  2. Si tiene varias cuentas de {{site.data.keyword.Bluemix_notm}}, seleccione la cuenta donde desea crear el clúster de Kubernetes.

  3. Para crear clústeres en un grupo de recursos que no sea el predeterminado, seleccione como destino dicho grupo de recursos. **Nota**:
      * Un clúster solo se puede crear en un grupo de recursos y, después de crear el clúster, no puede cambiar su grupo de recursos.
      * Debe tener al menos el [rol de **Visor**](/docs/containers?topic=containers-users#platform) para el grupo de recursos.

      ```
      ibmcloud target -g <resource_group_name>
      ```
      {: pre}

2. Revise las zonas que están disponibles. En la salida del mandato siguiente, las zonas tienen el **Tipo de ubicación** `dc`. Para distribuir el clúster entre zonas, debe crear el clúster en una [zona con soporte multizona](/docs/containers?topic=containers-regions-and-zones#zones). Las zonas con capacidad de multizona tienen un valor de zona metropolitana en la columna **Multizone Metro**.
    ```
    ibmcloud ks supported-locations
    ```
    {: pre}
    Si selecciona una zona que se encuentra fuera de su país, tenga en cuenta que es posible que se requiera autorización legal para poder almacenar datos físicamente en un país extranjero.
    {: note}

3. Revise los tipos de nodo trabajador que están disponibles en dicha zona. El tipo de nodo trabajador especifica los hosts de cálculo físicos o virtuales que están disponibles para cada nodo trabajador.
  - **Virtual**: Las máquinas virtuales se facturan por horas y se suministran en hardware compartido o dedicado.
  - **Físico**: Los servidores nativos se facturan de forma mensual y su suministro lo realiza manualmente la infraestructura de IBM Cloud (SoftLayer) después de que realice el pedido, por lo que puede tardar más de un día laborable en realizarse. Los servidores nativos son más apropiados para aplicaciones de alto rendimiento que necesitan más recursos y control de host.
  - **Máquinas físicas con Trusted Compute**: También puede optar por habilitar [Trusted Compute](/docs/containers?topic=containers-security#trusted_compute) para verificar que los nodos trabajadores nativos no se manipulan de forma indebida. Trusted Compute está disponible para determinados tipos de máquinas nativas. Por ejemplo, los distintos tipos de GPU `mgXc` no dan soporte a Trusted Compute. Si no habilita la confianza al crear el clúster pero la desea posteriormente, puede utilizar el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Una vez que habilita la confianza, no puede inhabilitarla posteriormente.
  - **Tipos de máquina**: para decidir qué tipo de máquina desplegar, revise el núcleo, la memoria y las combinaciones de almacenamiento del [hardware del nodo trabajador disponible](/docs/containers?topic=containers-planning_worker_nodes#shared_dedicated_node). Después de crear el clúster, puede añadir distintos tipos de máquina física o virtual mediante la [adición de una agrupación de nodos trabajadores](/docs/containers?topic=containers-add_workers#add_pool).

     Asegúrese de que desea suministrar una máquina nativa. Puesto que se factura mensualmente, si cancela la operación inmediatamente tras realizar un pedido por error, se le cobrará el mes completo.
     {:tip}

  ```
  ibmcloud ks machine-types --zone <zone>
  ```
  {: pre}

4. Compruebe si ya existen VLAN para la zona en la infraestructura de IBM Cloud (SoftLayer) para esta cuenta.
  ```
  ibmcloud ks vlans --zone <zone>
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ID        Name                Number   Type      Router
        1519999   vlan   1355     private   bcr02a.dal10
        1519898   vlan   1357     private   bcr02a.dal10
        1518787   vlan   1252     public   fcr02a.dal10
        1518888   vlan   1254     public    fcr02a.dal10
  ```
  {: screen}
  * Para crear un clúster en el que pueda ejecutar cargas de trabajo de cara a Internet, compruebe si existe una VLAN pública y privada. Si ya existen una VLAN pública y privada, anote los direccionadores correspondientes. Los direccionadores de VLAN privadas siempre
empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre
empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos. En la salida de ejemplo, se puede utilizar cualquier VLAN privada con cualquier VLAN pública porque todos los direccionadores incluyen `02a.dal10`.
  * Para crear un clúster que amplíe el centro de datos local solo en la red privada, que amplíe el centro de datos local con la opción de añadir acceso público limitado más adelante a través de nodos trabajadores de extremo o que amplíe el centro de datos local y proporcione acceso público limitado a través de un dispositivo de pasarela, compruebe si existe una VLAN privada. Si tiene una VLAN privada, anote el ID.

5. Ejecute el mandato `cluster-create`. De forma predeterminada, los discos de nodo trabajador estén cifrados en AES de 256 bits y el clúster se factura por horas de uso.
  * Para crear un clúster en el que pueda ejecutar cargas de trabajo de cara a Internet:
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers <number> --name <cluster_name> --kube-version <major.minor.patch> [--private-service-endpoint] [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * Para crear un clúster que amplíe el centro de datos local en la red privada, con la opción de añadir acceso público limitado más adelante a través de nodos trabajadores de extremo:
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --private-service-endpoint [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * Para crear un clúster que amplíe el centro de datos local y proporcione acceso público limitado a través de un dispositivo de pasarela:
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --public-service-endpoint [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}

    <table>
    <caption>Componentes de cluster-create</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>El mandato para crear un clúster en la organización de {{site.data.keyword.Bluemix_notm}}.</td>
    </tr>
    <tr>
    <td><code>--zone <em>&lt;zone&gt;</em></code></td>
    <td>Especifique el ID de la zona de {{site.data.keyword.Bluemix_notm}} en la que desea crear el clúster que ha elegido en el paso 4.</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>Especifique el tipo de máquina que ha elegido en el paso 5.</td>
    </tr>
    <tr>
    <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
    <td>Especifique el nivel de aislamiento del hardware del nodo trabajador. Utilice el valor dedicated para tener recursos físicos disponibles dedicados solo a usted, o shared para permitir que los recursos físicos se compartan con otros clientes de IBM. El valor predeterminado es shared. Este valor es opcional para clústeres estándares de VM. Para los tipos de máquina nativos, especifique `dedicated`.</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>Si ya tiene una VLAN pública configurada en su cuenta de infraestructura de IBM Cloud (SoftLayer) para esta zona, escriba el ID de la VLAN pública que ha obtenido en el paso 4.<p>Los direccionadores de VLAN privadas siempre
empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre
empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos.</p></td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>Si ya tiene una VLAN privada configurada en su cuenta de infraestructura de IBM Cloud (SoftLayer) para esta zona, escriba el ID de la VLAN privada que ha obtenido en el paso 4. Si no tiene una VLAN privada en su cuenta, no especifique esta opción. {{site.data.keyword.containerlong_notm}} crea automáticamente una VLAN privada.<p>Los direccionadores de VLAN privadas siempre
empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre
empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos.</p></td>
    </tr>
    <tr>
    <td><code>--private-only</code></td>
    <td>Cree el clúster solo con las VLAN privadas. Si incluye esta opción, no incluya la opción <code>--public-vlan</code>.</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>Especifique un nombre para el clúster. El nombre debe empezar por una letra, puede contener letras, números, guiones (-) y debe tener 35 caracteres como máximo. Utilice un nombre que sea exclusivo entre regiones. El nombre del clúster y la región en la que el clúster se despliega forman el nombre de dominio completo para el subdominio de Ingress. Para garantizar que el subdominio de Ingress es exclusivo dentro de una región, el nombre del clúster se puede truncar y se le puede añadir un valor aleatorio al nombre de dominio de Ingress.
</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>Especifique el número de nodos trabajadores que desea incluir en el clúster. Si no se especifica la opción <code>--workers</code>, se crea 1 nodo trabajador.</td>
    </tr>
    <tr>
    <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
    <td>La versión Kubernetes del nodo maestro del clúster. Este valor es opcional. Cuando no se especifica la versión, el clúster se crea con el valor predeterminado de las versiones de Kubernetes soportadas. Para ver las versiones disponibles, ejecute <code>ibmcloud ks versions</code>.
</td>
    </tr>
    <tr>
    <td><code>--private-service-endpoint</code></td>
    <td>**En [cuentas habilitadas para VRF](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)**: Habilite el punto final de servicio privado de modo que el nodo maestro de Kubernetes y los nodos trabajadores puedan comunicarse a través de la VLAN privada. Además, puede optar por habilitar el punto final de servicio público utilizando el distintivo `--public-service-endpoint` para acceder a su clúster a través de Internet. Si solo habilita el punto final de servicio privado, debe estar conectado a la VLAN privada para comunicarse con el maestro de Kubernetes. Después de habilitar un punto final de servicio privado, no podrá inhabilitarlo más tarde.<br><br>Después de crear el clúster, puede obtener el punto final con el mandato `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`.</td>
    </tr>
    <tr>
    <td><code>--public-service-endpoint</code></td>
    <td>Habilite el punto final de servicio público de modo que se pueda acceder al nodo maestro de Kubernetes a través de la red pública, por ejemplo para ejecutar mandatos `kubectl` desde el terminal, y de forma que el nodo maestro de Kubernetes y los nodos trabajadores puedan comunicarse a través de la VLAN pública. Posteriormente puede inhabilitar el punto final de servicio público si desea un clúster solo privado.<br><br>Después de crear el clúster, puede obtener el punto final con el mandato `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`.</td>
    </tr>
    <tr>
    <td><code>--disable-disk-encrypt</code></td>
    <td>De forma predeterminada, los nodos trabajadores ofrecen un [cifrado de disco](/docs/containers?topic=containers-security#encrypted_disk) AES de 256 bits. Si desea inhabilitar el cifrado, incluya esta opción.</td>
    </tr>
    <tr>
    <td><code>--trusted</code></td>
    <td>**Clústeres nativos**: Habilite [Trusted Compute](/docs/containers?topic=containers-security#trusted_compute) para verificar que los nodos trabajadores nativos no se manipulan de forma indebida. Trusted Compute está disponible para determinados tipos de máquinas nativas. Por ejemplo, los distintos tipos de GPU `mgXc` no dan soporte a Trusted Compute. Si no habilita la confianza al crear el clúster pero la desea posteriormente, puede utilizar el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Una vez que habilita la confianza, no puede inhabilitarla posteriormente.</td>
    </tr>
    </tbody></table>

6. Verifique que ha solicitado la creación del clúster. Para máquinas virtuales, se puede tardar varios minutos en pedir las máquinas de nodo trabajador y en configurar y suministrar el clúster en la cuenta. El suministro de las máquinas físicas nativas se realiza mediante interacción manual con la infraestructura de IBM Cloud (SoftLayer), por lo que puede tardar más de un día laborable en realizarse.
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    Una vez completado el suministro del clúster, el estado del clúster pasa a ser **deployed**.
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

7. Compruebe el estado de los nodos trabajadores.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Cuando los nodos trabajadores están listos, el estado pasa a **normal** y el estado es **Ready**. Cuando el estado del nodo sea **Preparado**, podrá acceder al clúster. Tenga en cuenta que, aunque el clúster esté listo, algunas partes del clúster que utilizan otros servicios, como por ejemplo los secretos de Ingress o los secretos de obtención de imágenes de registro, podrían estar aún en proceso. Tenga en cuenta que, si ha creado el clúster solo con una VLAN privada, no se asignarán direcciones de **IP pública** a los nodos trabajadores.

    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   standard       normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    A cada nodo trabajador se la asigna un ID exclusivo y un nombre de dominio que no se debe cambiar de forma manual después de haber creado el clúster. Si se cambia el nombre de dominio o el ID se impide que el maestro de Kubernetes gestione el clúster.
    {: important}

8. Una vez que se haya creado el clúster, puede [empezar a trabajar con el clúster configurando la sesión de CLI](#access_cluster).

<br />


## Acceso al clúster
{: #access_cluster}

Una vez que se haya creado el clúster, puede empezar a trabajar con el clúster configurando la sesión de CLI.
{: shortdesc}

### Acceso a clústeres a través del punto final de servicio público
{: #access_internet}

Para trabajar con su clúster, establezca el clúster que ha creado como contexto para una sesión de CLI para ejecutar mandatos `kubectl`.
{: shortdesc}

1. Si la red está protegida por un cortafuegos de empresa, permita el acceso a los puertos y puntos finales de la API de {{site.data.keyword.Bluemix_notm}} y de {{site.data.keyword.containerlong_notm}}.
  1. [Permita el acceso a los puntos finales públicos para la API de `ibmcloud` y la API de `ibmcloud ks` en el cortafuegos](/docs/containers?topic=containers-firewall#firewall_bx).
  2. [Permita que los usuarios autorizados del clúster ejecuten mandatos `kubectl`](/docs/containers?topic=containers-firewall#firewall_kubectl) para acceder al nodo maestro través de puntos finales de servicio solo privados, solo públicos o privados y públicos.
  3. [Permita que los usuarios autorizados del clúster ejecutar mandatos `calicotl`](/docs/containers?topic=containers-firewall#firewall_calicoctl) para gestionar las políticas de red de Calico en el clúster.

2. Defina el clúster que ha creado como contexto para esta sesión. Siga estos pasos de configuración cada vez que de trabaje con el clúster.

  Si desea utilizar en su lugar la consola de {{site.data.keyword.Bluemix_notm}}, puede ejecutar mandatos de CLI directamente desde el navegador web en el [terminal de Kubernetes](/docs/containers?topic=containers-cs_cli_install#cli_web).
  {: tip}
  1. Obtenga el mandato para establecer la variable de entorno y descargar los archivos de configuración de Kubernetes.
      ```
      ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
      ```
      {: pre}
      Cuando termine la descarga de los archivos de configuración, se muestra un mandato que puede utilizar para establecer la vía de acceso al archivo de configuración de
Kubernetes como variable de entorno.

      Ejemplo para OS X:
      ```
      export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}
  2. Copie y pegue el mandato que se muestra en el terminal para definir la variable de entorno `KUBECONFIG`.
  3. Compruebe que la variable de entorno `KUBECONFIG` se haya establecido correctamente.
      Ejemplo para OS X:
      ```
      echo $KUBECONFIG
      ```
      {: pre}

      Salida:
      ```
      /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}

3. Inicie el panel de control de Kubernetes con el puerto predeterminado `8001`.
  1. Establezca el proxy con el número de puerto predeterminado.
      ```
      kubectl proxy
      ```
      {: pre}

      ```
      Starting to serve on 127.0.0.1:8001
      ```
      {: screen}

  2.  Abra el siguiente URL en un navegador web para ver el panel de control de Kubernetes.
      ```
      http://localhost:8001/ui
      ```
      {: codeblock}

### Acceso a clústeres a través del punto final de servicio privado
{: #access_on_prem}

Se puede acceder al nodo maestro de Kubernetes a través del punto final de servicio privado si los usuarios autorizados del clúster están en su red privada de {{site.data.keyword.Bluemix_notm}} o si se conectan a la red privada a través de una [conexión VPN](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) o [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). Sin embargo, la comunicación con el nodo maestro de Kubernetes sobre el punto final de servicio privado debe pasar a través del rango de direcciones IP <code>166.X.X.X</code>, que no se puede direccionar desde una conexión VPN ni a través de {{site.data.keyword.Bluemix_notm}} Direct Link. Puede exponer el punto final de servicio privado del nodo maestro para los usuarios del clúster utilizando un equilibrador de carga de red (NLB) privado. El NLB privado expone el punto final de servicio privado del nodo maestro como un rango de direcciones IP <code>10.X.X.X</code> interno al que pueden acceder los usuarios con la VPN o con la conexión de {{site.data.keyword.Bluemix_notm}} Direct Link. Si solo habilita el punto final de servicio privado, puede utilizar el panel de control de Kubernetes o puede habilitar temporalmente el punto final de servicio público para crear el NLB privado.
{: shortdesc}

1. Si la red está protegida por un cortafuegos de empresa, permita el acceso a los puertos y puntos finales de la API de {{site.data.keyword.Bluemix_notm}} y de {{site.data.keyword.containerlong_notm}}.
  1. [Permita el acceso a los puntos finales públicos para la API de `ibmcloud` y la API de `ibmcloud ks` en el cortafuegos](/docs/containers?topic=containers-firewall#firewall_bx).
  2. [Permita a los usuarios autorizados del clúster ejecutar mandatos `kubectl`](/docs/containers?topic=containers-firewall#firewall_kubectl). Tenga en cuenta que no puede probar la conexión con el clúster en el paso 6 hasta que exponga el punto final de servicio privado del nodo maestro en el clúster mediante un NLB privado.

2. Obtenga el URL y el puerto de punto final de servicio privado para el clúster.
  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  En esta salida de ejemplo, el **URL de punto final de servicio privado** es `https://c1.private.us-east.containers.cloud.ibm.com:25073`.
  ```
  Name:                           setest
  ID:                             b8dcc56743394fd19c9f3db7b990e5e3
  State:                          normal
  Created:                        2019-04-25T16:03:34+0000
  Location:                       wdc04
  Master URL:                     https://c1.private.us-east.containers.cloud.ibm.com:25073
  Public Service Endpoint URL:    -
  Private Service Endpoint URL:   https://c1.private.us-east.containers.cloud.ibm.com:25073
  Master Location:                Washington D.C.
  ...
  ```
  {: screen}

3. Cree un archivo YAML llamado `kube-api-via-nlb.yaml`. Este archivo YAML crea un servicio `LoadBalancer` privado y expone el punto final de servicio privado a través de ese NLB. Sustituya `<private_service_endpoint_port>` por el puerto que ha encontrado en el paso anterior.
  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: kube-api-via-nlb
    annotations:
      service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: private
    namespace: default
  spec:
    type: LoadBalancer
    ports:
    - protocol: TCP
      port: <private_service_endpoint_port>
      targetPort: <private_service_endpoint_port>
  ---
  kind: Endpoints
  apiVersion: v1
  metadata:
    name: kube-api-via-nlb
  subsets:
    - addresses:
        - ip: 172.20.0.1
      ports:
        - port: 2040
  ```
  {: codeblock}

4. Para crear el NLB privado, debe estar conectado al nodo maestro del clúster. Puesto que todavía no se puede conectar a través del punto final de servicio privado desde una VPN o {{site.data.keyword.Bluemix_notm}} Direct Link, debe conectarse al nodo maestro de clúster y crear el NLB utilizando el punto final de servicio público o el panel de control de Kubernetes.
  * Si solo ha habilitado el punto final de servicio privado, puede utilizar el panel de control de Kubernetes para crear el NLB. El panel de control direcciona automáticamente todas las solicitudes al punto final de servicio privado del nodo maestro.
    1.  Inicie una sesión en la [consola de {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/).
    2.  En la barra de menús, seleccione la cuenta que desea utilizar.
    3.  En el menú ![Icono de menú](../icons/icon_hamburger.svg "Icono de menú"), pulse **Kubernetes**.
    4.  En la página **Clústeres**, pulse el clúster al que desea acceder.
    5.  En la página de detalles del clúster, pulse **Panel de control de Kubernetes**.
    6.  Pulse **+ Crear**.
    7.  Seleccione **Crear a partir de archivo**, cargue el archivo `kube-api-via-nlb.yaml` y pulse **Cargar**.
    8.  En la página **Visión general**, verifique que se ha creado el servicio `kube-api-via-nlb`. En la columna **Puntos finales externos**, anote la dirección `10.x.x.x`. Esta dirección IP expone el punto final de servicio privado para el nodo maestro de Kubernetes en el puerto que ha especificado en el archivo YAML.

  * Si también ha habilitado el punto final de servicio público, ya tiene acceso al nodo maestro.
    1. Cree el NLB y el punto final.
      ```
      kubectl apply -f kube-api-via-nlb.yaml
      ```
      {: pre}
    2. Verifique que se ha creado el NLB `kube-api-via-nlb`. En la salida, anote la dirección `10.x.x.x` **EXTERNAL-IP**. Esta dirección IP expone el punto final de servicio privado para el nodo maestro de Kubernetes en el puerto que ha especificado en el archivo YAML.
      ```
      kubectl get svc -o wide
      ```
      {: pre}

      En esta salida de ejemplo, la dirección IP correspondiente al punto final de servicio privado del nodo maestro de Kubernetes es `10.186.92.42`.
      ```
      NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)          AGE   SELECTOR
      kube-api-via-nlb         LoadBalancer   172.21.150.118   10.186.92.42     443:32235/TCP    10m   <none>
      ...
      ```
      {: screen}

  <p class="note">Si desea conectarse al nodo maestro mediante el [servicio VPN de strongSwan](/docs/containers?topic=containers-vpn#vpn-setup), anote la **IP de clúster** `172.21.x.x` que utilizará en el paso siguiente. Puesto que el pod de VPN de strongSwan se ejecuta dentro del clúster, puede acceder al NLB mediante la dirección IP del servicio IP de clúster interno. En el archivo `config.yaml` del diagrama de Helm de strongSwan, asegúrese de que el CIDR de la subred de servicio de Kubernetes, `172.21.0.0/16`, aparezca en la lista en el valor `local.subnet`.</p>

5. En las máquinas cliente en las que usted o sus usuarios ejecutan mandatos `kubectl`, añada la dirección IP de NLB y el URL de punto final de servicio privado al archivo `/etc/hosts`. No incluya ningún puerto en la dirección IP y en el URL y no incluya `https://` en el URL.
  * Para usuarios de OSX y Linux:
    ```
    sudo nano /etc/hosts
    ```
    {: pre}
  * Para usuarios de Windows:
    ```
    notepad C:\Windows\System32\drivers\etc\hosts
    ```
    {: pre}

    En función de los permisos de la máquina local, es posible que tenga que ejecutar el Bloc de notas como administrador para poder editar el archivo hosts.
    {: tip}

  Ejemplo de texto que debe añadir:
  ```
  10.186.92.42      c1.private.us-east.containers.cloud.ibm.com
  ```
  {: codeblock}

6. Verifique que está conectado a la red privada a través de una VPN o de una conexión de {{site.data.keyword.Bluemix_notm}} Direct Link.

7. Compruebe que los mandatos `kubectl` se ejecutan correctamente con el clúster a través del punto final de servicio privado comprobando la versión del cliente de la CLI de Kubernetes.
  ```
  kubectl version  --short
  ```
  {: pre}

  Salida de ejemplo:
  ```
  Client Version: v1.13.6
  Server Version: v1.13.6
  ```
  {: screen}

<br />


## Siguientes pasos
{: #next_steps}

Cuando el clúster esté activo y en ejecución, puede realizar las siguientes tareas:
- Si ha creado el clúster en una zona con capacidad multizona, [distribuya los nodos trabajadores añadiendo una zona a su clúster](/docs/containers?topic=containers-add_workers).
- [Desplegar una app en el clúster.](/docs/containers?topic=containers-app#app_cli)
- [Configure su propio registro privado en {{site.data.keyword.Bluemix_notm}} para almacenar y compartir imágenes de Docker con otros usuarios.](/docs/services/Registry?topic=registry-getting-started)
- [Configure el programa de escalado automático de clústeres](/docs/containers?topic=containers-ca#ca) para añadir o eliminar automáticamente nodos trabajadores de las agrupaciones de nodos trabajadores en función de las solicitudes de recursos de la carga de trabajo.
- Controle quién puede crear pods en el clúster con las [políticas de seguridad de pod](/docs/containers?topic=containers-psp).
- Habilite los complementos gestionados por [Istio](/docs/containers?topic=containers-istio) y por [Knative](/docs/containers?topic=containers-serverless-apps-knative) para ampliar las prestaciones del clúster.

A continuación, consulte los siguientes pasos de configuración de red para la configuración del clúster:

### Ejecución de cargas de trabajo de apps de cara a Internet en un clúster
{: #next_steps_internet}

* Aísle las cargas de trabajo de la red en [nodos trabajadores de extremo](/docs/containers?topic=containers-edge).
* Exponga sus apps con [servicios de red públicos](/docs/containers?topic=containers-cs_network_planning#public_access).
* Controle el tráfico público a los servicios de red que exponen sus apps mediante la creación de [políticas preDNAT de Calico](/docs/containers?topic=containers-network_policies#block_ingress), como por ejemplo políticas de lista blanca y de lista negra.
* Conecte el clúster con servicios en redes privadas fuera de la cuenta de {{site.data.keyword.Bluemix_notm}} mediante la configuración de un [servicio de VPN IPSec de strongSwan](/docs/containers?topic=containers-vpn).

### Ampliación del centro de datos local a un clúster y otorgamiento de acceso público limitado mediante nodos de extremo y políticas de red de Calico
{: #next_steps_calico}

* Conecte el clúster con servicios de redes privadas fuera de la cuenta de {{site.data.keyword.Bluemix_notm}} mediante la configuración de [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) o del [servicio VPN IPSec de strongSwan](/docs/containers?topic=containers-vpn#vpn-setup). {{site.data.keyword.Bluemix_notm}} Direct Link permite la comunicación entre apps y servicios del clúster y una red local a través de la red privada, mientras que strongSwan permite la comunicación a través de un túnel de VPN cifrado sobre la red pública.
* Aísle las cargas de trabajo de red pública creando una [agrupación de nodos trabajadores de extremo](/docs/containers?topic=containers-edge) de nodos trabajadores que están conectados a las VLAN públicas y privadas.
* Exponga sus apps con [servicios de red privados](/docs/containers?topic=containers-cs_network_planning#private_access).
* [Crear políticas de red de host de Calico](/docs/containers?topic=containers-network_policies#isolate_workers) para bloquear el acceso público a los pods, aislar el clúster en la red privada y permitir el acceso a otros servicios de {{site.data.keyword.Bluemix_notm}}.

### Ampliación del centro de datos local a un clúster y otorgamiento de acceso público limitado mediante un dispositivo de pasarela
{: #next_steps_gateway}

* Si también configura un cortafuegos de pasarela para la red privada, debe [permitir la comunicación entre nodos trabajadores y dejar que el clúster acceda a recursos de la infraestructura a través de la red privada](/docs/containers?topic=containers-firewall#firewall_private).
* Para conectar de forma segura los nodos trabajadores y las apps a redes privadas fuera de la cuenta de {{site.data.keyword.Bluemix_notm}}, configure un punto final de VPN IPSec en el dispositivo de pasarela. A continuación, [configure el servicio de VPN IPSec de strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) en el clúster de modo que utilice el punto final de VPN en la pasarela o [configure la conectividad de VPN directamente con VRA](/docs/containers?topic=containers-vpn#vyatta).
* Exponga sus apps con [servicios de red privados](/docs/containers?topic=containers-cs_network_planning#private_access).
* [Abra los puertos y las direcciones IP necesarios](/docs/containers?topic=containers-firewall#firewall_inbound) en el cortafuegos del dispositivo de pasarela para permitir el tráfico de entrada a los servicios de red.

### Ampliación del centro de datos local a un clúster solo en la red privada
{: #next_steps_extend}

* Si tiene un cortafuegos en la red privada, [permita la comunicación entre los nodos trabajadores y deje que el clúster acceda a los recursos de la infraestructura a través de la red privada](/docs/containers?topic=containers-firewall#firewall_private).
* Conecte el clúster con los servicios de las redes privadas fuera de la cuenta de {{site.data.keyword.Bluemix_notm}} mediante la configuración de [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link).
* Exponga sus apps en la red privada con [servicios de red privados](/docs/containers?topic=containers-cs_network_planning#private_access).
