---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="blank"}
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


# En desuso: Nube dedicada
{: #dedicated}

{{site.data.keyword.containerlong}} en {{site.data.keyword.Bluemix_dedicated_notm}} está en desuso. No se pueden crear clústeres en un entorno de {{site.data.keyword.Bluemix_dedicated_notm}}. Para crear clústeres en {{site.data.keyword.Bluemix_notm}} Público, consulte [Iniciación a {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started).
{: deprecated}

Si tiene una cuenta de {{site.data.keyword.Bluemix_dedicated_notm}}, puede desplegar clústeres de Kubernetes en un entorno de nube dedicado (`https://<my-dedicated-cloud-instance>.bluemix.net`) y conectarse a servicios de {{site.data.keyword.Bluemix_notm}} preseleccionados que también se ejecuten allí.
{:shortdesc}

Si no tiene cuenta de {{site.data.keyword.Bluemix_dedicated_notm}}, puede [empezar a trabajar con {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started) en una cuenta pública de {{site.data.keyword.Bluemix_notm}}.

## Acerca del entorno de nube dedicada
{: #dedicated_environment}

Con una cuenta de {{site.data.keyword.Bluemix_dedicated_notm}}, los recursos físicos disponibles se dedican únicamente a su clúster y no se comparten con otros clústeres de otros clientes de {{site.data.keyword.IBM_notm}}. Podría elegir configurar un entorno {{site.data.keyword.Bluemix_dedicated_notm}} cuando desee aislamiento para el clúster y requerir aislamiento para otros servicios de {{site.data.keyword.Bluemix_notm}} que utilice. Si no tiene una cuenta dedicada, puede [crear clústeres con hardware dedicado en {{site.data.keyword.Bluemix_notm}} público](/docs/containers?topic=containers-clusters#clusters_ui).
{: shortdesc}

Con {{site.data.keyword.Bluemix_dedicated_notm}}, puede crear clústeres desde el catálogo en la consola dedicada o mediante la CLI de {{site.data.keyword.containerlong_notm}}. Para utilizar la consola dedicada, se inicia sesión en la cuenta dedicada y en la pública simultáneamente con el IBMid. Utilice el inicio de sesión dual para acceder a los clústeres públicos utilizando la consola dedicada. Para utilizar la CLI, inicie una sesión utilizando el punto final dedicado (`api.<my-dedicated-cloud-instance>.bluemix.net.`). Defina como destino el punto final de la API de {{site.data.keyword.containerlong_notm}} de la región pública que está asociada con el entorno dedicado.

Las diferencias más importantes entre {{site.data.keyword.Bluemix_notm}} público y dedicado son las siguientes.

*   En {{site.data.keyword.Bluemix_dedicated_notm}}, {{site.data.keyword.IBM_notm}} posee y gestiona la cuenta de infraestructura de IBM Cloud (SoftLayer) en la que se despliegan los nodos trabajadores, las VLAN y las subredes. En {{site.data.keyword.Bluemix_notm}} público, usted es el propietario de la cuenta de infraestructura de IBM Cloud (SoftLayer).
*   En {{site.data.keyword.Bluemix_dedicated_notm}}, las especificaciones de las VLAN y las subredes en la cuenta de infraestructura de IBM Cloud (SoftLayer) gestionada por {{site.data.keyword.IBM_notm}} se determinan cuando el entorno dedicado está habilitado. En {{site.data.keyword.Bluemix_notm}} público, las especificaciones de VLAN y subredes se determinan cuando se crea el clúster.

### Diferencias en la gestión de clústeres entre entornos de nube
{: #dedicated_env_differences}

<table>
<caption>Diferencias en la gestión de clústeres</caption>
<col width="20%">
<col width="40%">
<col width="40%">
 <thead>
 <th>Área</th>
 <th>{{site.data.keyword.Bluemix_notm}} público</th>
 <th>{{site.data.keyword.Bluemix_dedicated_notm}}</th>
 </thead>
 <tbody>
 <tr>
 <td>Creación del clúster</td>
 <td>Cree un clúster gratuito o un clúster estándar.</td>
 <td>Cree un clúster estándar.</td>
 </tr>
 <tr>
 <td>Propiedad y hardware del clúster</td>
 <td>En los clústeres estándares, el hardware se puede compartir con otros clientes de {{site.data.keyword.IBM_notm}} o puede estar dedicado a usted únicamente. Las VLAN privadas y públicas las posee y gestiona cada usuario en su cuenta de infraestructura de IBM Cloud (SoftLayer).</td>
 <td>En los clústeres del entorno de {{site.data.keyword.Bluemix_dedicated_notm}}, el hardware es siempre dedicado. Las VLAN públicas y privadas que están disponibles para la creación del clúster están predefinidas al configurar el entorno de {{site.data.keyword.Bluemix_dedicated_notm}} y es IBM quien las gestiona en su nombre. La zona que está disponible durante la creación del clúster también se define de forma previa para el entorno de {{site.data.keyword.Bluemix_notm}}.</td>
 </tr>
 <tr>
 <td>Redes de Ingress y de equilibrio de carga</td>
 <td>Durante el suministro del clúster estándar, se llevan a cabo de forma automática las siguientes acciones.<ul><li>Se vinculan una subred portátil pública y una subred portátil privada al clúster y se asignan a su cuenta de infraestructura de IBM Cloud (SoftLayer). Se pueden solicitar más subredes a través de su cuenta de infraestructura de IBM Cloud (SoftLayer).</li></li><li>Se utiliza una dirección IP pública portátil para un equilibrador de carga de aplicación (ALB) de Ingress de alta disponibilidad y se asigna una ruta pública exclusiva en el formato <code>&lt;cluster_name&gt;. containers.appdomain.cloud</code>. Puede utilizar esta ruta para exponer varias apps al público. Para un ALB privado se utiliza una dirección IP privada portátil.</li><li>Se asignan cuatro direcciones IP públicas portátiles y cuatro direcciones IP privadas portátiles al clúster que se pueden utilizar para servicios de equilibrio de carga.</ul></td>
 <td>Cuando se crea una cuenta para el entorno Dedicado, se toma la decisión de conectividad sobre cómo se desea exponer los servicios del clúster y acceder a ellos. Para utilizar los rangos de IP de su propia empresa (IP gestionadas por el usuario), debe proporcionarlos cuando [configure un entorno de {{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated?topic=dedicated-dedicated#setupdedicated). <ul><li>De forma predeterminada, no se vincula ninguna subred portátil pública a los clústeres que crea en su cuenta dedicada. En su lugar, tiene la flexibilidad de elegir el modelo de conectividad que mejor se adapte a su empresa.</li><li>Después de crear el clúster, elija el tipo de subredes que desea enlazar y utilizar con el clúster para la conectividad de Ingress o del equilibrador de carga.<ul><li>Para las subredes portátiles públicas o privadas, puede [añadir subredes a los clústeres](/docs/containers?topic=containers-subnets#subnets)</li><li>Para las direcciones IP gestionadas por el usuario que ha proporcionado a IBM en la incorporación dedicada, puede [añadir subredes gestionadas por el usuario a los clústeres](#dedicated_byoip_subnets).</li></ul></li><li>Después de enlazar una subred a su clúster, se crea el ALB de Ingress. Se crea una ruta de Ingress pública solo si utiliza una subred pública portátil.</li></ul></td>
 </tr>
 <tr>
 <td>Redes de NodePort</td>
 <td>Exponga un puerto público en el nodo trabajador y utilice la dirección IP pública del nodo trabajador para acceder de forma pública al servicio en el clúster.</td>
 <td>Todas las direcciones IP públicas de los nodos trabajadores están bloqueados por un cortafuegos. Sin embargo, para los servicios de {{site.data.keyword.Bluemix_notm}} que se añadan al clúster, se puede acceder al NodePort mediante una dirección IP pública o una dirección IP privada.</td>
 </tr>
 <tr>
 <td>Almacén persistente</td>
 <td>Utiliza el [suministro dinámico](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning) o el [suministro estático](/docs/containers?topic=containers-kube_concepts#static_provisioning) de volúmenes.</td>
 <td>Utiliza el [suministro dinámico](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning) de volúmenes. [Abra un caso de soporte](/docs/get-support?topic=get-support-getting-customer-support) para solicitar una copia de seguridad y una restauración de los volúmenes y para realizar otras funciones de almacenamiento.</li></ul></td>
 </tr>
 <tr>
 <td>URL de registro de imágenes en {{site.data.keyword.registryshort_notm}}</td>
 <td><ul><li>EE. UU. sur y EE. UU. este: <code>registry.ng bluemix.net</code></li><li>RU sur: <code>registry.eu-gb.bluemix.net</code></li><li>UE central (Frankfurt): <code>registry.eu-de.bluemix.net</code></li><li>Australia (Sídney): <code>registry.au-syd.bluemix.net</code></li></ul></td>
 <td><ul><li>Para los nuevos espacios de nombres, utilice los mismos registros basados en regiones que los definidos para el entorno de {{site.data.keyword.Bluemix_notm}} público.</li><li>Para los espacios de nombres que se configuraron para contenedores escalables y únicos en {{site.data.keyword.Bluemix_dedicated_notm}}, utilice <code>registry.&lt;dedicated_domain&gt;</code></li></ul></td>
 </tr>
 <tr>
 <td>Acceso al registro</td>
 <td>Consulte las opciones del apartado sobre [Utilización de registros de imágenes privadas y públicas con {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-images).</td>
 <td><ul><li>Para los espacios de nombres nuevos, consulte las opciones del apartado sobre [Utilización de registros de imágenes privadas y públicas con {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-images).</li><li>Para espacios de nombres que se configuraron para grupos escalables y únicos, se [utiliza una señal para crear un secreto de Kubernetes](#cs_dedicated_tokens) para la autenticación.</li></ul></td>
 </tr>
 <tr>
 <td>Clústeres multizona</td>
 <td>Para crear [clústeres multizona](/docs/containers?topic=containers-ha_clusters#multizone), añada más zonas a las agrupaciones de nodos trabajadores.</td>
 <td>Cree [clústeres de una sola zona](/docs/containers?topic=containers-ha_clusters#single_zone). La zona disponible se ha definido previamente al configurar el entorno {{site.data.keyword.Bluemix_dedicated_notm}}. De forma predeterminada, un clúster de una sola zona se configura con una agrupación de nodos trabajadores denominada `default`. La agrupación de nodos trabajadores agrupa los nodos trabajadores con la misma configuración, como por ejemplo el tipo de máquina, que ha definido durante la creación del clúster. Puede añadir más nodos trabajadores a su clúster [cambiando el tamaño de una agrupación de nodos trabajadores existente](/docs/containers?topic=containers-add_workers#resize_pool) o [añadiendo una nueva agrupación de nodos trabajadores](/docs/containers?topic=containers-add_workers#add_pool). Cuando añada una agrupación de nodos trabajadores, debe añadir la zona disponible a la agrupación de nodos trabajadores para que los nodos trabajadores se puedan desplegar en la zona. Sin embargo, no puede añadir otras zonas a las agrupaciones de nodos trabajadores.</td>
 </tr>
</tbody></table>
{: caption="Diferencias entre las características de {{site.data.keyword.Bluemix_notm}} público y {{site.data.keyword.Bluemix_dedicated_notm}}" caption-side="top"}

<br />



### Arquitectura del servicio
{: #dedicated_ov_architecture}

Cada nodo trabajador está configurado recursos de cálculo, sistema de red y servicio de volúmenes separados.
{:shortdesc}

Las características integradas de seguridad proporcionan aislamiento, funciones de gestión de recursos y conformidad con la seguridad de los nodos trabajadores. El nodo trabajador se comunica con el maestro mediante certificados TLS seguros y conexión OpenVPN.


*Arquitectura de Kubernetes y sistema de red en {{site.data.keyword.Bluemix_dedicated_notm}}*

![{{site.data.keyword.containerlong_notm}} Arquitectura de Kubernetes en {{site.data.keyword.Bluemix_dedicated_notm}}](images/cs_dedicated_arch.png)

<br />


## Configuración de {{site.data.keyword.containerlong_notm}} en Dedicado
{: #dedicated_setup}

Cada entorno de {{site.data.keyword.Bluemix_dedicated_notm}} tiene una cuenta corporativa pública y propiedad del cliente en {{site.data.keyword.Bluemix_notm}}. Para que los usuarios en el entorno dedicado creen clústeres, el administrador debe añadir a los usuarios a una cuenta corporativa pública.
{:shortdesc}

Antes de empezar:
  * [Configure un entorno de {{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated?topic=dedicated-dedicated#setupdedicated).
  * Si el sistema local o la red corporativa controla puntos finales de Internet público mediante proxies o cortafuegos, debe [abrir los puertos y direcciones IP necesarios en el cortafuegos](/docs/containers?topic=containers-firewall#firewall).
  * [Descargue la CLI de Cloud Foundry de ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/cloudfoundry/cli/releases).

Para permitir a los usuarios de {{site.data.keyword.Bluemix_dedicated_notm}} acceder a clústeres:

1.  El propietario de la cuenta pública de {{site.data.keyword.Bluemix_notm}} debe generar una clave de API.
    1.  Inicie una sesión en el punto final de la instancia de {{site.data.keyword.Bluemix_dedicated_notm}}. Especifique las credenciales de {{site.data.keyword.Bluemix_notm}} del propietario de la cuenta pública y seleccione su cuenta cuando se le solicite.

        ```
        ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        Si tiene un ID federado, utilice `ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` para iniciar la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe si tiene un ID federado cuando el inicio de sesión falla sin `--sso` y se lleva a cabo correctamente con la opción `--sso`.
        {: tip}

    2.  Genere una clave de API para invitar a los usuarios a la cuenta pública. Anote el valor de la clave de API, que el administrador de la cuenta dedicada debe utilizar en el paso siguiente.

        ```
        ibmcloud iam api-key-create <key_name> -d "Key to invite users to <dedicated_account_name>"
        ```
        {: pre}

    3.  Anote el GUID de la organización de cuenta pública a la que desea invitar a los usuarios, que el administrador de la cuenta dedicada debe utilizar en el siguiente paso.

        ```
        ibmcloud account orgs
        ```
        {: pre}

2.  El propietario de la cuenta de {{site.data.keyword.Bluemix_dedicated_notm}} puede invitar a uno o varios usuarios a su cuenta pública.
    1.  Inicie una sesión en el punto final de la instancia de {{site.data.keyword.Bluemix_dedicated_notm}}. Especifique las credenciales de {{site.data.keyword.Bluemix_notm}} del propietario de la cuenta dedicada y seleccione su cuenta cuando se le solicite.

        ```
        ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        Si tiene un ID federado, utilice `ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` para iniciar la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe si tiene un ID federado cuando el inicio de sesión falla sin `--sso` y se lleva a cabo correctamente con la opción `--sso`.
        {: tip}

    2.  Invite a los usuarios a la cuenta pública.
        * Para invitar a un solo usuario:

            ```
            ibmcloud cf bluemix-admin invite-users-to-public -userid=<user_email> -apikey=<public_API_key> -public_org_id=<public_org_ID>
            ```
            {: pre}

            Sustituya <em>&lt;user_IBMid&gt;</em> con el correo electrónico del usuario al que desea invitar, <em>&lt;public_API_key&gt;</em> con la clave de API generada en el paso anterior y <em>&lt;public_org_ID&gt;</em> con el GUID de la organización de la cuenta pública. 

        * Para invitar a todos los usuarios de una organización de cuenta dedicada:

            ```
            ibmcloud cf bluemix-admin invite-users-to-public -organization=<dedicated_org_ID> -apikey=<public_API_key> -public_org_id=<public_org_ID>
            ```

            Sustituya <em>&lt;dedicated_org_ID&gt;</em> con el ID de la organización de cuenta dedicada, <em>&lt;public_API_key&gt;</em> con la clave de API generada en el paso anterior y <em>&lt;public_org_ID&gt;</em> con el GUID de la organización de la cuenta pública. 

    3.  Si existe un IBMid para un usuario, el usuario se añade automáticamente a la organización especificada en la cuenta pública. Si no existe un IBMid para un usuario, se envía una invitación a la dirección de correo electrónico del usuario. Después de que el usuario acepte la invitación, se crea un IBMid para el usuario y el usuario se añade a la organización especificada en la cuenta pública.

    4.  Verifique que los usuarios se han añadido a la cuenta.

        ```
        ibmcloud cf bluemix-admin invite-users-status -apikey=<public_API_key>
        ```
        {: pre}

        Los usuarios invitados que tienen un IBMid existente tiene el estado `ACTIVE`. Los usuarios invitados que no tienen un IBMid existente tienen un estado `PENDING` antes de aceptar la invitación. Los usuarios invitados tendrán el estado `ACTIVE` después de aceptar la invitación.

3.  Si un usuario necesita privilegios de creación de clústeres, debe otorgarle el rol de Administrador.

    1.  En la barra de menús de la consola pública, pulse **Gestionar > Seguridad > Identidad y acceso** y pulse **Usuarios**.

    2.  En la fila del usuario al que desea asignar acceso, seleccione el menú **Acciones** y, a continuación, pulse **Asignar acceso**.

    3.  Seleccione **Asignar acceso a recursos**.

    4.  En la lista **Servicios**, seleccione
**{{site.data.keyword.containerlong}}**.

    5.  En la lista **Región**, seleccione **Todas las regiones actuales** o una región específica, si se le solicita.

    6. En **Seleccionar roles**, seleccione Administrador.

    7. Pulse **Asignar**.

4.  Los usuarios ya pueden iniciar sesión en el punto final de la cuenta dedicada para empezar a crear clústeres.

    1.  Inicie una sesión en el punto final de la instancia de {{site.data.keyword.Bluemix_dedicated_notm}}. Escriba el IBMid cuando se lo soliciten.

        ```
        ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        Si tiene un ID federado, utilice `ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` para iniciar la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe si tiene un ID federado cuando el inicio de sesión falla sin `--sso` y se lleva a cabo correctamente con la opción `--sso`.
        {: tip}

    2.  Si inicia sesión por primera vez, proporcione su ID de usuario dedicado y su contraseña cuando se le solicite. La cuenta dedicada se autentica y se enlazan conjuntamente la cuenta pública y la cuenta dedicada. Cada vez que inicie sesión después de esta primera vez, sólo utilizará el IBMid. Para obtener más información, consulte [Conexión de un ID dedicado a su IBMid público](/docs/iam?topic=iam-connect_dedicated_id#connect_dedicated_id).

        Debe iniciar sesión en su cuenta dedicada y en su cuenta pública para crear clústeres. Si solo desea iniciar sesión en la cuenta dedicada, utilice el distintivo `--no-iam` al iniciar sesión en el punto final dedicado.
        {: note}

    3.  Para crear o acceder a clústeres en el entorno dedicado, debe establecer la región que se asocia con ese entorno. **Nota**: no
puede crear clústeres en grupos de recursos que no sean `default`.

        ```
        ibmcloud ks region-set
        ```
        {: pre}

5.  Si desea desenlazar sus cuentas, puede desconectar el IBMid del ID de usuario dedicado. Para obtener más información, consulte [Desconectar el ID dedicado del IBMid público](/docs/iam?topic=iam-connect_dedicated_id#disconnect_id).

    ```
    ibmcloud iam dedicated-id-disconnect
    ```
    {: pre}

<br />


## Creación de clústeres
{: #dedicated_administering}

Diseñe la configuración de su clúster de {{site.data.keyword.Bluemix_dedicated_notm}} para maximizar su disponibilidad y capacidad.
{:shortdesc}

### Creación de clústeres con la consola de {{site.data.keyword.Bluemix_notm}}
{: #dedicated_creating_ui}

1. Abra la consola dedicada: `https://<my-dedicated-cloud-instance>.bluemix.net`.

2. Marque el recuadro de selección **Iniciar sesión también en {{site.data.keyword.Bluemix_notm}} público** y pulse **Iniciar sesión**.

3. Siga las indicaciones para iniciar sesión con el IBMid. Si inicia una sesión en su cuenta dedicada por primera vez, siga las solicitudes para iniciar una sesión en {{site.data.keyword.Bluemix_dedicated_notm}}.

4. En el catálogo, seleccione **Contenedores** y pulse **Clúster de Kubernetes**.

5. Configure los detalles del clúster.

    1. Escriba un **Nombre de clúster**. El nombre debe empezar por una letra, puede contener letras, números, guiones (-) y debe tener 35 caracteres como máximo. Utilice un nombre que sea exclusivo entre regiones. El nombre del clúster y la región en la que el clúster se despliega forman el nombre de dominio completo para el subdominio de Ingress. Para garantizar que el subdominio de Ingress es exclusivo dentro de una región, el nombre del clúster se puede truncar y se le puede añadir un valor aleatorio al nombre de dominio de Ingress.

    2. Seleccione la **Zona** en la que desea desplegar el clúster. La zona disponible se ha definido previamente al configurar el entorno {{site.data.keyword.Bluemix_dedicated_notm}}.

    3. Elija la versión del servidor de API de Kubernetes para el nodo maestro del clúster.

    4. Seleccione un tipo de aislamiento de hardware. La opción virtual se factura por hora, y la nativa se factura mensualmente.

        - **Virtual - Dedicado**: Los nodos trabajadores están alojados en una infraestructura que está dedicada a su cuenta. Sus recursos físicos están completamente aislados.

        - **Nativo**: Los servidores nativos se facturan de forma mensual y su suministro lo realiza manualmente la infraestructura de IBM Cloud (SoftLayer) después de que realice el pedido, por lo que puede tardar más de un día laborable en realizarse. Los servidores nativos son más apropiados para aplicaciones de alto rendimiento que necesitan más recursos y control de host. Para determinados tipos de máquina, también puede optar por habilitar [Trusted Compute](/docs/containers?topic=containers-security#trusted_compute) para verificar que los nodos trabajadores no se manipulan de forma indebida. Si no habilita la confianza al crear el clúster pero la desea posteriormente, puede utilizar el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Una vez que habilita la confianza, no puede inhabilitarla posteriormente.

        Asegúrese de que desea suministrar una máquina nativa. Puesto que se factura mensualmente, si cancela la operación inmediatamente tras realizar un pedido por error, se le cobrará el mes completo.
        {:tip}

    5. Seleccione un **Tipo de máquina**. El tipo de máquina define la cantidad de memoria, espacio de disco y CPU virtual que se configura en cada nodo trabajador y que está disponible para todos los contenedores. Los tipos de máquinas nativas y virtuales varían según la zona en la que se despliega el clúster. Para obtener más información, consulte el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-type`. Después de crear el clúster, puede añadir distintos tipos de máquina añadiendo un nodo trabajador al clúster.

    6. Elija el **Número de nodos trabajadores** que necesita. Seleccione `3` para garantizar una alta disponibilidad del clúster.

    7. Seleccione una **VLAN pública** (opcional) y una **VLAN privada** (obligatorio). Las VLAN públicas y privadas se definen de forma previa al configurar el entorno de {{site.data.keyword.Bluemix_dedicated_notm}}. Ambas VLAN comunican entre nodos trabajadores, pero la VLAN pública también se comunica con el maestro de Kubernetes gestionado por IBM. Puede utilizar la misma VLAN para varios clústeres.
        Si los nodos trabajadores se han configurado solo con una VLAN privada, debe permitir que los nodos trabajadores y el maestro del clúster se comuniquen mediante la [habilitación del punto final de servicio privado](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se) o la [configuración de un dispositivo de pasarela](/docs/containers?topic=containers-plan_clusters#workeruser-master).
        {: note}

    8. De forma predeterminada, **Cifrar disco local** está seleccionado. Si elige desmarcar el recuadro de selección, no se cifran los datos de tiempo de ejecución de contenedor del host. [Más información sobre el cifrado](/docs/containers?topic=containers-security#encrypted_disk).

6. Pulse **Crear clúster**. Verá el progreso del despliegue del nodo trabajador en el separador **Nodos trabajadores**. Cuando finalice el despliegue, podrá ver que el clúster está listo en el separador **Visión general**.
    A cada nodo trabajador se la asigna un ID exclusivo y un nombre de dominio que no se debe cambiar de forma manual después de haber creado el clúster. Si se cambia el nombre de dominio o el ID se impide que el maestro de Kubernetes gestione el clúster.
    {: important}

### Creación de clústeres con la CLI
{: #dedicated_creating_cli}

1.  Instale la CLI de {{site.data.keyword.Bluemix_notm}} y el [plugin {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_cli_install#cs_cli_install).
2.  Inicie una sesión en el punto final de la instancia de {{site.data.keyword.Bluemix_dedicated_notm}}. Especifique sus credenciales de {{site.data.keyword.Bluemix_notm}} y seleccione la cuenta cuando se le solicite.

    ```
    ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
    ```
    {: pre}

    Si tiene un ID federado, utilice `ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` para iniciar la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe si tiene un ID federado cuando el inicio de sesión falla sin `--sso` y se lleva a cabo correctamente con la opción `--sso`.
    {: tip}

3.  Seleccione como destino un punto final regional. Se da soporte a los siguientes puntos finales regionales:
  * Dallas (EE. UU. sur, us-south): `https://us-south.containers.cloud.ibm.com`
  * Frankfurt (EU Central, eu-de): `https://eu-de.containers.cloud.ibm.com`
  * Londres (RU sur, eu-gb): `https://eu-gb.containers.cloud.ibm.com`
  * Sídney (AP sur, au-syd): `https://au-syd.containers.cloud.ibm.com`
  * Tokio (AP norte, jp-tok): `https://jp-tok.containers.cloud.ibm.com`
  * Washington, D.C. (EE. UU. este, us-east): `https://us-east.containers.cloud.ibm.com`
  ```
  ibmcloud ks init --host <endpoint>
  ```
  {: pre}
  No puede utilizar el punto final global, `https://containers.cloud.ibm.com`. Debe elegir como destino un punto final regional para crear o para trabajar con los clústeres de dicha región.
  {: important}

4.  Cree un clúster con el mandato `cluster-create`. Cuando se crea un clúster estándar, el hardware del nodo trabajador se factura por horas de uso.

    Ejemplo:

    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --name <cluster_name> --workers <number>
    ```
    {: pre}

    <table>
    <caption>Descripción de los componentes de este mandato</caption>
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
    <td>Especifique el ID de zona de {{site.data.keyword.Bluemix_notm}} configurado para su entorno dedicado.</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>Especifique un tipo de máquina. Puede desplegar los nodos trabajadores como máquinas virtuales en hardware dedicado, o como máquinas físicas en servidores nativos. Los tipos de máquinas físicas y virtuales varían según la zona en la que se despliega el clúster. Para obtener más información, consulte el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-type`.</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;machine_type&gt;</em></code></td>
    <td>Especifique el ID de la VLAN pública configurada para utilizar con el entorno dedicado. Si desea conectar los nodos trabajadores solo a una VLAN privada, no especifique esta opción.<p class="note">Si los nodos trabajadores se han configurado solo con una VLAN privada, debe permitir que los nodos trabajadores y el maestro del clúster se comuniquen mediante la [habilitación del punto final de servicio privado](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se) o la [configuración de un dispositivo de pasarela](/docs/containers?topic=containers-plan_clusters#workeruser-master).</p></td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;machine_type&gt;</em></code></td>
    <td>Especifique el ID de la VLAN privada configurada para utilizar con el entorno dedicado.</td>
    </tr>  
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>Especifique un nombre para el clúster. El nombre debe empezar por una letra, puede contener letras, números, guiones (-) y debe tener 35 caracteres como máximo. Utilice un nombre que sea exclusivo entre regiones. El nombre del clúster y la región en la que el clúster se despliega forman el nombre de dominio completo para el subdominio de Ingress. Para garantizar que el subdominio de Ingress es exclusivo dentro de una región, el nombre del clúster se puede truncar y se le puede añadir un valor aleatorio al nombre de dominio de Ingress.
</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>Especifique el número de nodos trabajadores que desea incluir en el clúster. Si no se especifica la opción <code>--workers</code>, se crea un nodo trabajador.</td>
    </tr>
    <tr>
    <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
    <td>La versión Kubernetes del nodo maestro del clúster. Este valor es opcional. Cuando no se especifica la versión, el clúster se crea con el valor predeterminado de las versiones de Kubernetes soportadas. Para ver las versiones disponibles, ejecute <code>ibmcloud ks versions</code>.
</td>
    </tr>
    <tr>
    <td><code>--disable-disk-encrypt</code></td>
    <td>De forma predeterminada, los nodos trabajadores ofrecen un [cifrado de disco](/docs/containers?topic=containers-security#encrypted_disk) AES de 256 bits. Si desea inhabilitar el cifrado, incluya esta opción.</td>
    </tr>
    <tr>
    <td><code>--trusted</code></td>
    <td>Habilite [Trusted Compute](/docs/containers?topic=containers-security#trusted_compute) para verificar que los nodos trabajadores nativos no se manipulan de forma indebida. Si no habilita la confianza al crear el clúster pero la desea posteriormente, puede utilizar el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Una vez que habilita la confianza, no puede inhabilitarla posteriormente.</td>
    </tr>
    </tbody></table>

5.  Verifique que ha solicitado la creación del clúster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

    * Para máquinas virtuales, se puede tardar varios minutos en pedir las máquinas de nodo trabajador y en configurar y suministrar el clúster en la cuenta. El suministro de las máquinas físicas nativas se realiza mediante interacción manual con la infraestructura de IBM Cloud (SoftLayer), por lo que puede tardar más de un día laborable en realizarse.
    * Si ve el siguiente mensaje de error, [abra un caso de soporte](/docs/get-support?topic=get-support-getting-customer-support).
        ```
        Excepción de infraestructura de {{site.data.keyword.Bluemix_notm}}: No se ha podido realizar el pedido. No hay suficientes recursos tras el direccionador 'router_name' para realizar la solicitud para los siguientes invitados: 'worker_id'.
        ```

    Una vez completado el suministro del clúster, el estado del clúster pasa a ser **deployed**.

    ```
    Name         ID                                   State      Created          Workers    Zone      Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1          mil01     1.13.6      Default
    ```
    {: screen}

6.  Compruebe el estado de los nodos trabajadores.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Cuando los nodos trabajadores están listos, el estado pasa a **normal** y el estado es **Ready**. Cuando el estado del nodo sea **Preparado**, podrá acceder al clúster.

    A cada nodo trabajador se la asigna un ID exclusivo y un nombre de dominio que no se debe cambiar de forma manual después de haber creado el clúster. Si se cambia el nombre de dominio o el ID se impide que el maestro de Kubernetes gestione el clúster.
    {: important}

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Zone   Version
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    free           normal   Ready    mil01      1.13.6
    ```
    {: screen}

7.  Defina el clúster que ha creado como contexto para esta sesión. Siga estos pasos de configuración cada vez que de trabaje con el clúster.

    1.  Obtenga el mandato para establecer la variable de entorno y descargar los archivos de configuración de Kubernetes.

        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Cuando termine la descarga de los archivos de configuración, se muestra un mandato que puede utilizar para establecer la vía de acceso al archivo de configuración de
Kubernetes como variable de entorno.

        Ejemplo para OS X:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

    2.  Copie y pegue el mandato en la salida para establecer la variable de entorno `KUBECONFIG`.
    3.  Compruebe que la variable de entorno `KUBECONFIG` se haya establecido correctamente.

        Ejemplo para OS X:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Salida:

        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml

        ```
        {: screen}

8.  Acceda al panel de control de Kubernetes con el puerto predeterminado 8001.
    1.  Establezca el proxy con el número de puerto predeterminado.

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

### Adición de nodos trabajadores
{: #add_workers_dedicated}

Con {{site.data.keyword.Bluemix_dedicated_notm}}, solo puede crear [clústeres de una sola zona](/docs/containers?topic=containers-ha_clusters#single_zone). De forma predeterminada, un clúster de una sola zona se configura con una agrupación de nodos trabajadores denominada `default`. La agrupación de nodos trabajadores agrupa los nodos trabajadores con la misma configuración, como por ejemplo el tipo de máquina, que ha definido durante la creación del clúster. Puede añadir más nodos trabajadores a su clúster [cambiando el tamaño de una agrupación de nodos trabajadores existente](/docs/containers?topic=containers-add_workers#resize_pool) o [añadiendo una nueva agrupación de nodos trabajadores](/docs/containers?topic=containers-add_workers#add_pool). Cuando añada una agrupación de nodos trabajadores, debe añadir la zona disponible a la agrupación de nodos trabajadores para que los nodos trabajadores se puedan desplegar en la zona. Sin embargo, no puede añadir otras zonas a las agrupaciones de nodos trabajadores.
{: shortdesc}

### Utilización de registros de imagen privada y pública
{: #dedicated_images}

Obtenga más información sobre cómo [proteger su información personal](/docs/containers?topic=containers-security#pi) cuando se trabaja con imágenes de contenedor.

Para los espacios de nombres nuevos, consulte las opciones del apartado sobre [Utilización de registros de imágenes privadas y públicas con {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-images). Para espacios de nombres que se configuraron para grupos escalables y únicos, se [utiliza una señal para crear un secreto de Kubernetes](#cs_dedicated_tokens) para la autenticación.

### Adición de subredes a clústeres
{: #dedicated_cluster_subnet}

Cambie la agrupación de las direcciones IP públicas portátiles disponibles añadiendo subredes a su clúster. Para obtener más información, consulte [Adición de subredes a clústeres](/docs/containers?topic=containers-subnets#subnets). Revise las diferencias siguientes para añadir subredes a clústeres dedicados.
{: shortdesc}

#### Adición de más subredes y direcciones IP gestionadas por el usuario a los clústeres de Kubernetes
{: #dedicated_byoip_subnets}

Proporcione más subredes propias desde una red local que desee utilizar para acceder a {{site.data.keyword.containerlong_notm}}. Puede añadir direcciones IP privadas desde dichas subredes a servicios de Ingress y del equilibrador de carga en el clúster de Kubernetes. Las subredes gestionadas por el usuario se configuran de dos maneras, según el formato de la subred que desee utilizar.
{: shortdesc}

Requisitos:
- Las subredes gestionadas por el usuario solo se pueden añadir a VLAN privadas.
- El límite de longitud del prefijo de subred es de /24 a /30. Por ejemplo, `203.0.113.0/24` especifica 253 direcciones IP privadas que se pueden utilizar, mientras que `203.0.113.0/30` especifica 1 dirección IP privada que se puede utilizar.
- La primera dirección IP de la subred se debe utilizar como pasarela para la subred.

Antes de empezar: configure el direccionamiento del tráfico de red de entrada y de salida de la red de empresa a la red de {{site.data.keyword.Bluemix_dedicated_notm}} que utilizará la subred gestionada por el usuario.

1. Para utilizar su propia subred, [abra un caso de soporte](/docs/get-support?topic=get-support-getting-customer-support) y proporcione la lista de CIDR de subred que desea utilizar. **Nota**: la forma en la que se gestionan los equilibradores de carga y los ALB para la conectividad de cuenta interna y local varía según el formato del CIDR de subred. Consulte el paso final para ver las diferencias de configuración.

2. Una vez que {{site.data.keyword.IBM_notm}} suministre las subredes gestionadas por el usuario, ponga la subred a disposición del clúster de Kubernetes.

    ```
    ibmcloud ks cluster-user-subnet-add --cluster <cluster_name> --subnet-cidr <subnet_CIDR> --private-vlan <private_VLAN>
    ```
    {: pre}
    Sustituya <em>&lt;cluster_name&gt;</em> con el nombre o el ID del clúster, <em>&lt;subnet_CIDR&gt;</em> con uno de los CIDR de subred que ha proporcionado en el caso de soporte y <em>&lt;private_VLAN&gt;</em> con un ID de VLAN privada. Encontrará el ID de la VLAN privada disponible ejecutando `ibmcloud ks vlans`.

3. Compruebe que las subredes se hayan añadido al clúster. El campo **User-managed** de las subredes proporcionadas por el usuario es _`true`_.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR         Public       User-managed
    1555503   169.xx.xxx.xxx/24   true         false
    1555505   10.xxx.xx.xxx/24    false        false
    1555505   10.xxx.xx.xxx/24    false        true
    ```
    {: screen}

4. **Importante**: Si tiene varias VLAN para un clúster, varias subredes en la misma VLAN o un clúster multizona, debe habilitar la [función de direccionador virtual (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para la cuenta de infraestructura de IBM Cloud (SoftLayer) para que los nodos trabajadores puedan comunicarse entre sí en la red privada. Para habilitar VRF, [póngase en contacto con el representante de su cuenta de la infraestructura de IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si no puede o no desea habilitar VRF, habilite la [distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para llevar a cabo esta acción, necesita el [permiso de la infraestructura](/docs/containers?topic=containers-users#infra_access) **Red > Gestionar distribución de VLAN de red** o bien puede solicitar al propietario de la cuenta que lo habilite. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.

5. Para configurar la conectividad de cuenta interna y local, elija una de estas opciones:
  - Si ha utilizado un rango de direcciones IP privadas 10.x.x.x para la subred, utilice IP válidas de ese rango para configurar la conectividad de cuenta interna y local con Ingress y un equilibrador de carga. Para obtener más información, consulte [Planificación de redes con NodePort, equilibrador de carga o servicios Ingress](/docs/containers?topic=containers-cs_network_planning#external).
  - Si no ha utilizado un rango de direcciones IP privadas 10.x.x.x para la subred, utilice IP válidas de ese rango para configurar la conectividad local con Ingress y un equilibrador de carga. Para obtener más información, consulte [Planificación de redes con NodePort, equilibrador de carga o servicios Ingress](/docs/containers?topic=containers-cs_network_planning#external). No obstante, debe utilizar una subred privada portátil de infraestructura de IBM Cloud (SoftLayer) para configurar la conectividad de cuenta interna entre el clúster y otros servicios basados en Cloud Foundry. Puede crear una subred privada portátil con el mandato [`ibmcloud ks cluster-subnet-add`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add). En este caso, el clúster tiene una subred gestionada por el usuario para la conectividad local y una subred privada portátil de infraestructura de IBM Cloud (SoftLayer) para la conectividad de cuenta interna.

### Otras configuraciones de clúster
{: #dedicated_other}

Revise las siguientes opciones de otras configuraciones de clúster:
  * [Gestión de acceso a clústeres](/docs/containers?topic=containers-users#access_policies)
  * [Actualización del maestro de Kubernetes](/docs/containers?topic=containers-update#master)
  * [Actualización de nodos trabajadores](/docs/containers?topic=containers-update#worker_node)
  * [Configuración del registro de clúster](/docs/containers?topic=containers-health#logging). La habilitación de registro no está soportada desde el punto final dedicado. Debe iniciar sesión en el punto final de {{site.data.keyword.cloud_notm}} público y definir como objetivo el espacio y la organización públicos para permitir el reenvío de registros.
  * [Configuración de la supervisión del clúster](/docs/containers?topic=containers-health#view_metrics). Existe un clúster `ibm-monitoring` dentro de cada cuenta de {{site.data.keyword.Bluemix_dedicated_notm}}. Este clúster supervisa continuamente el estado de {{site.data.keyword.containerlong_notm}} en el entorno dedicado, comprobando la estabilidad y la conectividad del entorno. No elimine este clúster del entorno.
  * [Eliminación de clústeres](/docs/containers?topic=containers-remove)

<br />


## Despliegue de apps en clústeres
{: #dedicated_apps}

Puede utilizar las técnicas de Kubernetes para desplegar apps en clústeres de {{site.data.keyword.Bluemix_dedicated_notm}} y asegurarse de que sus apps estén siempre activas y en funcionamiento.
{:shortdesc}

Para desplegar apps en clústeres, siga las instrucciones para [desplegar apps en clústeres públicos de {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-app#app). Revise las siguientes diferencias para clústeres de {{site.data.keyword.Bluemix_dedicated_notm}}.

Obtenga más información sobre cómo [proteger su información personal](/docs/containers?topic=containers-security#pi) cuando se trabaja recursos de Kubernetes.

### Cómo permitir el acceso público a apps
{: #dedicated_apps_public}

Para entornos de {{site.data.keyword.Bluemix_dedicated_notm}}, las direcciones IP públicas principales están bloqueadas por un cortafuegos. Para hacer que una app esté disponible a nivel público, utilice un [servicio equilibrador de carga](#dedicated_apps_public_load_balancer) o [Ingress](#dedicated_apps_public_ingress) en lugar de un servicio NodePort. Si necesita acceder a un servicio equilibrador de carga o Ingress que cuenta con direcciones IP públicas portátiles, proporcione una lista blanca de cortafuegos de empresa a IBM en el momento de la incorporación del servicio.
{: shortdesc}

#### Configuración del acceso a una app utilizando el tipo de servicio LoadBalancer
{: #dedicated_apps_public_load_balancer}

Si desea utilizar direcciones IP públicas para el equilibrador de carga, asegúrese de que se ha proporcionado una lista blanca de cortafuegos de empresa a IBM, o [abra un caso de soporte](/docs/get-support?topic=get-support-getting-customer-support) para configurar la lista blanca del cortafuegos. A continuación, siga los pasos de [Equilibrio de carga básica y de DSR con equilibradores de carga de red (NLB - Network Load Balancers)](/docs/containers?topic=containers-loadbalancer).
{: shortdesc}

#### Configuración del acceso público a una app utilizando Ingress
{: #dedicated_apps_public_ingress}

Si desea utilizar direcciones IP públicas para Ingress ALB, asegúrese de que se ha proporcionado una lista blanca de cortafuegos de empresa a IBM, o [abra un caso de soporte](/docs/get-support?topic=get-support-getting-customer-support) para configurar la lista blanca del cortafuegos. A continuación, siga los pasos en [Exposición de apps al público](/docs/containers?topic=containers-ingress#ingress_expose_public).
{: shortdesc}

### Creación de almacenamiento persistente
{: #dedicated_apps_volume_claim}

Para ver las opciones de creación de almacenamiento persistente, consulte [Opciones de almacenamiento de datos persistente para alta disponibilidad](/docs/containers?topic=containers-storage_planning#persistent_storage_overview). Para solicitar una copia de seguridad de los volúmenes, una restauración de los volúmenes, una supresión de volúmenes o una instantánea periódica de almacenamiento de archivos, debe [abrir un caso de soporte](/docs/get-support?topic=get-support-getting-customer-support).
{: shortdesc}

Si decide suministrar [almacenamiento de archivos](/docs/containers?topic=containers-file_storage#file_predefined_storageclass), seleccione clases de almacenamiento sin retención. Elegir clases de almacenamiento sin retención permite evitar las instancias de almacenamiento persistente huérfanas en la infraestructura de IBM Cloud (SoftLayer) que sólo puede eliminar abriendo un caso de soporte.

## Creación de una señal de {{site.data.keyword.registryshort_notm}} para un registro de imágenes de {{site.data.keyword.Bluemix_dedicated_notm}}
{: #cs_dedicated_tokens}

Cree una señal que no caduque para un registro de imágenes que pueda utilizar para grupos escalables y únicos con clústeres en {{site.data.keyword.containerlong}}.
{:shortdesc}

1.  Solicite una señal de registro permanente para la sesión actual. Esta señal otorga acceso a las imágenes en el espacio de nombres actual.
    ```
    ibmcloud cr token-add --description "<description>" --non-expiring -q
    ```
    {: pre}

2.  Verifique el secreto de Kubernetes.

    ```
    kubectl describe secrets
    ```
    {: pre}

    Utilice este secreto para trabajar con {{site.data.keyword.containerlong}}.

3.  Cree el secreto de Kubernetes para almacenar la información de la señal.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Descripción de los componentes de este mandato</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace &lt;kubernetes_namespace&gt;</code></td>
    <td>Obligatorio. El espacio de nombres de Kubernetes del clúster en el que desea utilizar el secreto y desplegar los contenedores. Ejecute <code>kubectl get namespaces</code> para obtener una lista de todos los espacios de nombres del clúster.</td>
    </tr>
    <tr>
    <td><code>&lt;secret_name&gt;</code></td>
    <td>Obligatorio. El nombre que desea utilizar para su secreto de extracción de imágenes.</td>
    </tr>
    <tr>
    <td><code>--docker-server=&lt;registry_url&gt;</code></td>
    <td>Obligatorio. El URL del registro de imágenes donde está configurado el espacio de nombres: <code>registry.&lt;dedicated_domain&gt;</code></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username=token</code></td>
    <td>Obligatorio. No cambie este valor.</td>
    </tr>
    <tr>
    <td><code>--docker-password=&lt;token_value&gt;</code></td>
    <td>Obligatorio. El valor de la señal de registro que ha recuperado anteriormente.</td>
    </tr>
    <tr>
    <td><code>--docker-email=&lt;docker-email&gt;</code></td>
    <td>Obligatorio. Si tiene una, especifique la dirección de correo electrónico de Docker. Si no tiene una, especifique una dirección de correo electrónico ficticia, como por ejemplo a@b.c. Este correo electrónico es obligatorio para crear un secreto de Kubernetes, pero no se utiliza después de la creación.</td>
    </tr>
    </tbody></table>

4.  Cree un pod que haga referencia al secreto de extracción de imágenes.

    1.  Abra el editor de texto que prefiera y cree un script de configuración de pod llamado `mypod.yaml`.
    2.  Defina el pod y el secreto de extracción de imágenes que desea utilizar para acceder al registro. Para utilizar una imagen privada de un espacio de nombres:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<dedicated_domain>/<my_namespace>/<my_image>:<tag>
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>Visión general de los componentes del archivo YAML</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
        </thead>
        <tbody>
        <tr>
        <td><code>&lt;pod_name&gt;</code></td>
        <td>El nombre del pod que desea crear.</td>
        </tr>
        <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>El nombre del contenedor que desea desplegar en el clúster.</td>
        </tr>
        <tr>
        <td><code>&lt;my_namespace&gt;</code></td>
        <td>El espacio de nombres donde se almacena la imagen. Para obtener una lista de los espacios de nombres disponibles, ejecute `ibmcloud cr namespace-list`.</td>
        </tr>
        <td><code>&lt;my_image&gt;</code></td>
        <td>El nombre del imagen que desea utilizar. Para ver una lista de todas las imágenes disponibles en una cuenta de {{site.data.keyword.Bluemix_notm}}, ejecute <code>ibmcloud cr image-list</code>.</td>
        </tr>
        <tr>
        <td><code>&lt;tag&gt;</code></td>
        <td>La versión de la imagen que desea utilizar. Si no se especifica ninguna etiqueta, se utiliza la imagen etiquetada como <strong>la más reciente (latest)</strong>.</td>
        </tr>
        <tr>
        <td><code>&lt;secret_name&gt;</code></td>
        <td>El nombre del secreto de extracción de imágenes que ha creado anteriormente.</td>
        </tr>
        </tbody></table>

    3.  Guarde los cambios.

    4.  Cree el despliegue en el clúster.

          ```
          kubectl apply -f mypod.yaml -n <namespace>
          ```
          {: pre}
