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



# Configuración de subredes para clústeres
{: #subnets}

Cambie la agrupación de las direcciones IP públicas o privadas portátiles disponibles para los servicios del equilibrador de carga de red (NLB) añadiendo subredes a su clúster de Kubernetes.
{:shortdesc}



## Visión general de la red en {{site.data.keyword.containerlong_notm}}
{: #basics}

Comprenda los conceptos básicos de la red en los clústeres de {{site.data.keyword.containerlong_notm}}. {{site.data.keyword.containerlong_notm}} utiliza VLAN, subredes y direcciones IP para la conectividad de red de los componentes del clúster.
{: shortdesc}

### VLAN
{: #basics_vlans}

Cuando se crea un clúster, los nodos trabajadores del clúster se conectan automáticamente a una VLAN. Una VLAN configura un grupo de nodos trabajadores y pods como si estuvieran conectadas a la misma conexión física y ofrece un canal para la conectividad entre los nodos trabajadores y los pods.
{: shortdesc}

<dl>
<dt>VLAN para clústeres gratuitos</dt>
<dd>En los clústeres gratuitos, los nodos trabajadores del clúster se conectan de forma predeterminada a una VLAN pública y VLAN privada propiedad de IBM. Puesto que IBM controla las VLAN, las subredes y las direcciones IP, no puede crear clústeres multizona ni añadir subredes a un clúster, y solo puede utilizar servicios NodePort para exponer la app.</dd>
<dt>VLAN para clústeres estándares</dt>
<dd>En los clústeres estándares, la primera vez que crea un clúster en una zona, se suministra automáticamente una VLAN pública y una VLAN privada en dicha zona en su cuenta de infraestructura de IBM Cloud (SoftLayer). Para los demás clústeres que cree en dicha zona, debe especificar el par de VLAN que desee utilizar en la zona. Puede reutilizar las mismas VLAN pública y privada que se han creado porque varios clústeres pueden compartir las VLAN.</br>
</br>Puede conectar los nodos trabajadores tanto a una VLAN pública y a la VLAN privada, o solo a la VLAN privada. Si desea conectar sus nodos trabajadores únicamente a una VLAN privada, utilice el ID de una VLAN privada existente o [cree una VLAN privada](/docs/cli/reference/ibmcloud?topic=cloud-cli-manage-classic-vlans#sl_vlan_create) y utilice el ID durante la creación del clúster.</dd></dl>

Para ver las VLAN que se suministran en cada zona para su cuenta, ejecute `ibmcloud ks vlans --zone <zone>.` Para ver las VLAN en las que se ha suministrado un clúster, ejecute `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> --showResources` y busque el campo **Subdominio de Ingress**.

La infraestructura de IBM Cloud (SoftLayer) gestiona las VLAN que se suministran automáticamente cuando crea el primer clúster en una zona. Si deja que una VLAN quede sin utilizar, por ejemplo eliminando todos los nodos trabajadores de una VLAN, la infraestructura de IBM Cloud (SoftLayer) reclama la VLAN. Después, si necesita una nueva VLAN, [póngase en contacto con el equipo de soporte de {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans).

**¿Puedo cambiar mi decisión sobre VLAN más adelante?**</br>
Puede cambiar la configuración de la VLAN modificando las agrupaciones de nodos trabajadores del clúster. Para obtener más información, consulte [Cambio de las conexiones de VLAN de nodo trabajador](/docs/containers?topic=containers-cs_network_cluster#change-vlans).

### Subredes y direcciones IP
{: #basics_subnets}

Además de los nodos trabajadores y los pods, las subredes también se suministran automáticamente en las VLAN. Las subredes proporcionan conectividad de red a los componentes del clúster mediante la asignación de direcciones IP a los mismos.
{: shortdesc}

Las subredes siguientes se suministran automáticamente en las VLAN públicas y privadas predeterminadas:

**Subredes VLAN públicas**
* La subred pública primaria determina las direcciones IP públicas que se asignan a los nodos trabajadores durante la creación del clúster. Varios clústeres en la misma VLAN pueden compartir una subred pública primaria.
* La subred pública portátil está enlazada a un solo clúster y proporciona al clúster 8 direcciones IP públicas. 3 IP están reservadas para las funciones de la infraestructura IBM Cloud (SoftLayer). El ALB de Ingress público predeterminado utiliza 1 IP y se pueden utilizar 4 IP para crear servicios de equilibrador de carga de red pública (NLB) o más ALB públicos. Las IP públicas portátiles son direcciones IP fijas y permanentes que se pueden utilizar para acceder a los NLB y a los ALB por internet. Si necesita más de 4 IP para los NLB o los ALB, consulte [Adición de direcciones IP portátiles](/docs/containers?topic=containers-subnets#adding_ips).

**Subredes VLAN privadas**
* La subred privada primaria determina las direcciones IP privadas que se asignan a los nodos trabajadores durante la creación del clúster. Varios clústeres en la misma VLAN pueden compartir una subred privada primaria.
* La subred privada portátil está enlazada a un solo clúster y proporciona al clúster 8 direcciones IP privadas. 3 IP están reservadas para las funciones de la infraestructura IBM Cloud (SoftLayer). El ALB de Ingress privado predeterminado utiliza 1 IP y se pueden utilizar 4 IP para crear servicios de equilibrador de carga de red privada (NLB) o más ALB privados. Las IP privadas portátiles son direcciones IP fijas y permanentes, que se pueden utilizar para acceder a los NLB o ALB por una red privada. Si necesita más de 4 IP para los NLB o los ALB privados, consulte [Adición de direcciones IP portátiles](/docs/containers?topic=containers-subnets#adding_ips).

Para ver todas las subredes que se suministran en su cuenta, ejecute `ibmcloud ks subnets`. Para ver las subredes privadas portátiles y públicas portátiles que están enlazadas a un clúster, ejecute `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> --showResources` y busque la sección **Subnet VLANs**.

En {{site.data.keyword.containerlong_notm}}, las VLAN tienen un límite de 40 subredes. Si alcanza este límite, compruebe en primer lugar si puede [reutilizar subredes en la VLAN para crear nuevos clústeres](/docs/containers?topic=containers-subnets#subnets_custom). Si necesita una nueva VLAN, [póngase en contacto con el equipo de soporte de {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans) para solicitar una. A continuación, [cree un clúster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) que utilice esta nueva VLAN.
{: note}

**¿Cambia la dirección IP de mis nodos trabajadores?**</br>
A su nodo trabajador se le asigna una dirección IP en las VLAN públicas o privadas que utiliza el clúster. Una vez que se ha suministrado el nodo trabajador, las direcciones IP no cambian. Por ejemplo, las direcciones IP de nodo trabajador persisten en las operaciones `reload`, `reboot` y `update`. Además, la dirección IP privada del nodo trabajador se utiliza para la identidad del nodo trabajador en la mayoría de los mandatos `kubectl`. Si cambia las VLAN que utiliza la agrupación de trabajadores, los nuevos nodos trabajadores que se suministran en esa agrupación utilizan las nuevas VLAN para sus direcciones IP. Las direcciones IP de los nodos trabajadores existentes no cambian, pero, si lo desea, puede eliminar los nodos trabajadores que utilizan las VLAN antiguas.

### Segmentación de red
{: #basics_segmentation}

La segmentación de red describe el enfoque para dividir una red en varias subredes. Las apps que se ejecutan en una subred no pueden ver ni acceder a las apps de otra subred. Para obtener más información sobre las opciones de segmentación de red y sobre cómo se relacionan con las VLAN, consulte [este tema sobre la seguridad del clúster](/docs/containers?topic=containers-security#network_segmentation).
{: shortdesc}

Sin embargo, en varias situaciones, los componentes del clúster deben poder comunicarse entre varias VLAN privadas. Por ejemplo, si desea crear un clúster multizona, si tiene varias VLAN para un clúster o si tiene varias subredes en la misma VLAN, los nodos trabajadores de distintas subredes de la misma VLAN o de distintas VLAN no se pueden comunicar automáticamente entre sí. Debe habilitar una función de direccionador virtual (VRF) o una distribución de VLAN para la cuenta de infraestructura de IBM Cloud (SoftLayer).

**¿Qué son las funciones de direccionador virtual (VRF) y la distribución de VLAN?**</br>

<dl>
<dt>[Función de direccionador virtual (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)</dt>
<dd>Una VRF permite que todas las VLAN y subredes de la cuenta de la infraestructura se comuniquen entre sí. Además, se necesita una VRF para permitir que los trabajadores y el maestro se comuniquen a través del punto final de servicio privado. Para habilitar VRF, [póngase en contacto con el representante de su cuenta de la infraestructura de IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Tenga en cuenta que VRF elimina la opción de distribución de VLAN para la cuenta, ya que todas las VLAN pueden comunicarse a menos que configure un dispositivo de pasarela para gestionar el tráfico.</dd>
<dt>[Distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)</dt>
<dd>Si no puede o no desea habilitar VRF, habilite la distribución de VLAN. Para llevar a cabo esta acción, necesita el [permiso de la infraestructura](/docs/containers?topic=containers-users#infra_access) **Red > Gestionar distribución de VLAN de red** o bien puede solicitar al propietario de la cuenta que lo habilite. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`. Tenga en cuenta que no puede habilitar el punto final de servicio privado si elige habilitar la distribución de VLAN en lugar de una VRF.</dd>
</dl>

**¿Cómo afecta VRF o la distribución de VLAN a la segmentación de red?**</br>

Cuando se habilita la VRF o la distribución de VLAN, cualquier sistema que esté conectado a cualquiera de las VLAN privadas de la misma cuenta de {{site.data.keyword.Bluemix_notm}} se puede comunicar con los nodos trabajadores. Puede aislar el clúster de otros sistemas de la red privada aplicando [Políticas de red privada de Calico](/docs/containers?topic=containers-network_policies#isolate_workers). {{site.data.keyword.containerlong_notm}} también es compatible con todas las [ofertas de cortafuegos de infraestructura de IBM Cloud (SoftLayer) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud-computing/bluemix/network-security). Puede configurar un cortafuegos, como por ejemplo un [dispositivo de direccionamiento virtual](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra), con políticas de red personalizadas para proporcionar seguridad de red dedicada para el clúster estándar y para detectar y solucionar problemas de intrusión en la red.

<br />



## Utilización de subredes existentes para crear un clúster
{: #subnets_custom}

Cuando se crea un clúster estándar, se crean subredes automáticamente. Sin embargo, en lugar de utilizar las subredes suministradas automáticamente, puede utilizar subredes portátiles existentes de la cuenta de infraestructura de IBM Cloud (SoftLayer) o reutilizar las subredes de un clúster suprimido.
{:shortdesc}

Esta opción permite retener las direcciones IP estáticas estables al crear, eliminar o solicitar bloques grandes de direcciones de IP. Si, en su lugar, desea obtener más direcciones IP privadas portátiles para los servicios de equilibrador de carga de red (NLB) del clúster utilizando su propia subred local, consulte [Adición de IP privadas portátiles mediante la adición de subredes gestionadas por el usuario a VLAN privadas](#subnet_user_managed).

Las direcciones IP públicas portátiles se facturan mensualmente. Si elimina direcciones IP públicas portátiles una vez suministrado el clúster, todavía tendrá que pagar el cargo mensual, aunque solo las haya utilizado un breve periodo de tiempo.
{: note}

Antes de empezar:
- [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Para reutilizar las subredes de un clúster que ya no necesita, suprima el clúster innecesario. Cree el nuevo clúster inmediatamente porque las subredes se suprimen en un plazo de 24 horas si no se reutilizan.

   ```
   ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
   ```
   {: pre}

</br>Para utilizar una subred existente en su portafolio de infraestructura de IBM Cloud (SoftLayer):

1. Obtenga el ID de la subred a utilizar y el ID de la VLAN en la que se encuentra la subred.

    ```
    ibmcloud ks subnets
    ```
    {: pre}

    En este ejemplo, el ID de subred es `1602829` y el ID de la VLAN es `2234945`:
    ```
    Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public
    ```
    {: screen}

2. [Cree un clúster en la CLI](/docs/containers?topic=containers-clusters#clusters_cli_steps) utilizando el ID de VLAN que ha identificado. Incluya el distintivo `--no-subnet` para evitar que se creen de forma automática una nueva subred IP pública portátil y una nueva subred IP privada portátil.

    ```
    ibmcloud ks cluster-create --zone dal10 --machine-type b3c.4x16 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name my_cluster
    ```
    {: pre}
    Si no recuerda la zona en la que está la VLAN para el distintivo `--zone`, puede comprobar si la VLAN está en una zona determinada ejecutando `ibmcloud ks vlans --zone <zone>`.
    {: tip}

3.  Verifique que el clúster se ha creado. Se puede tardar hasta 15 minutos en solicitar las máquinas de nodo trabajador y en configurar y suministrar el clúster en la cuenta.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

    Cuando el clúster se haya suministrado por completo, el **Estado (State)** cambia a `deployed`.

    ```
    Name         ID                                   State      Created          Workers    Zone      Version     Resource Group Name
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3          dal10     1.13.6      Default
    ```
    {: screen}

4.  Compruebe el estado de los nodos trabajadores.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Antes de continuar con el paso siguiente, los nodos trabajadores deben estar preparados. El **Estado (State)** cambia a `normal` y el **Estatus (Status)** es `Ready`.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Zone     Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx  free           normal     Ready    dal10      1.13.6
    ```
    {: screen}

5.  Añada la subred al clúster especificando el ID de subred. Cuando pone una subred a disponibilidad de un clúster, se crea automáticamente una correlación de configuración de Kubernetes que incluye todas las direcciones IP públicas portátiles disponibles que puede utilizar. Si no existe ningún ALB de Ingress en la zona en la que se encuentra la VLAN de la subred, se utiliza automáticamente una dirección IP pública portátil y una dirección IP privada portátil para crear los ALB públicos y privados para dicha zona. Puede utilizar el resto de las direcciones IP públicas y privadas portátiles para crear servicios de NLB para las apps.

  ```
  ibmcloud ks cluster-subnet-add --cluster <cluster_name_or_id> --subnet-id <subnet_ID>
  ```
  {: pre}

  Mandato de ejemplo:
  ```
  ibmcloud ks cluster-subnet-add --cluster mycluster --subnet-id 807861
  ```
  {: screen}

6. **Importante**: para habilitar la comunicación entre nodos trabajadores que están en distintas subredes en la misma VLAN, debe [habilitar el direccionamiento entre subredes en la misma VLAN](#subnet-routing).

<br />


## Gestión de las direcciones IP portátiles existentes
{: #managing_ips}

De forma predeterminada, se pueden utilizar 4 direcciones IP públicas portátiles y 4 direcciones IP privadas portátiles para exponer apps individuales a la red privada o pública mediante la [creación de un servicio de equilibrador de carga de red (NLB)](/docs/containers?topic=containers-loadbalancer) o la [creación de equilibradores de carga de aplicación (ALB) de Ingress adicionales](/docs/containers?topic=containers-ingress#scale_albs). Para crear un servicio de NLB o ALB, debe tener disponible al menos una dirección IP portátil del tipo correcto. Puede ver las direcciones IP portátiles disponibles o puede liberar una dirección IP portátil utilizada.
{: shortdesc}

### Visualización de direcciones IP públicas portátiles disponibles
{: #review_ip}

Para obtener una lista de todas las direcciones IP portátiles del clúster, tanto utilizadas como disponibles, puede ejecutar el siguiente mandato.
{: shortdesc}

```
kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
```
{: pre}

Para obtener una lista solo de las direcciones IP públicas portátiles disponibles para crear NLB o más ALB públicos, puede seguir los pasos siguientes:

Antes de empezar:
-  Asegúrese de que tiene el [rol de servicio **Escritor** o **Gestor** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre el espacio de nombres `default`.
- [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Para ver una lista de las direcciones IP públicas portátiles disponibles:

1.  Cree un archivo de configuración de servicio Kubernetes denominado `myservice.yaml` y defina un servicio de tipo `LoadBalancer` con una dirección IP de NLB ficticia. En el ejemplo siguiente se utiliza la dirección IP 1.1.1.1 como dirección IP de NLB. Sustituya `<zone>` por la zona en la que desea comprobar las IP disponibles.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
      annotations:
        service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "<zone>"
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  Cree el servicio en el clúster.

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  Inspeccione el servicio.

    ```
    kubectl describe service myservice
    ```
    {: pre}

    La creación de este servicio falla porque el maestro de Kubernetes no encuentra la dirección IP de NLB especificada en la correlación de configuración de Kubernetes. Cuando se ejecuta este mandato, puede ver el mensaje de error y una lista de las direcciones IP públicas disponibles para el clúster.

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IP addresses are available: <list_of_IP_addresses>
    ```
    {: screen}

<br />


### Liberación de direcciones IP utilizadas
{: #free}

Puede liberar una dirección IP portátil utilizada suprimiendo el servicio de equilibrador de carga de red (NLB) o inhabilitando el equilibrador de carga de aplicación (ALB) de Ingress que está utilizando la dirección IP portátil.
{:shortdesc}

Antes de empezar:
-  Asegúrese de que tiene el [rol de servicio **Escritor** o **Gestor** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre el espacio de nombres `default`.
- [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Para suprimir un NLB o para inhabilitar un ALB:

1. Obtenga una lista de los servicios disponibles en el clúster.
    ```
    kubectl get services | grep LoadBalancer
    ```
    {: pre}

2. Elimine el servicio equilibrador de carga o inhabilite el ALB que utiliza una dirección IP pública o privada.
  * Suprima un NLB:
    ```
    kubectl delete service <service_name>
    ```
    {: pre}
  * Inhabilite un ALB:
    ```
    ibmcloud ks alb-configure --albID <ALB_ID> --disable
    ```
    {: pre}

<br />


## Adición de direcciones IP portátiles
{: #adding_ips}

De forma predeterminada, se pueden utilizar 4 direcciones IP públicas portátiles y 4 direcciones IP privadas portátiles para exponer apps individuales a la red privada o pública mediante la [creación de un servicio de equilibrador de carga de red (NLB)](/docs/containers?topic=containers-loadbalancer). Para crear más de 4 NLB públicos o 4 privados, puede obtener más direcciones IP portátiles añadiendo subredes de red al clúster.
{: shortdesc}

Cuando pone una subred a disponibilidad de un clúster, las direcciones IP de esta subred se utilizan para la gestión de redes del clúster. Para evitar conflictos de direcciones IP, asegúrese de utilizar una subred con un solo clúster. No utilice una subred para varios clústeres o para otros fines externos a {{site.data.keyword.containerlong_notm}} al mismo tiempo.
{: important}

### Adición de direcciones IP portátiles mediante la solicitud de más subredes
{: #request}

Puede obtener más IP portátiles para los servicios de NLB creando una nueva subred en una cuenta de infraestructura de IBM Cloud (SoftLayer) y poniéndola a disposición del clúster especificado.
{:shortdesc}

Las direcciones IP públicas portátiles se facturan mensualmente. Si elimina direcciones IP públicas portátiles una vez suministrada la subred, todavía tendrá que pagar el cargo mensual, aunque solo las haya utilizado un breve periodo de tiempo.
{: note}

Antes de empezar:
-  Asegúrese de tener el [rol de plataforma **Operador** o **Administrador** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para el clúster.
- [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Para solicitar una subred:

1. Suministre una subred nueva.

    ```
    ibmcloud ks cluster-subnet-create --cluster <cluster_name_or_id> --size <subnet_size> --vlan <VLAN_ID>
    ```
    {: pre}

    <table>
    <caption>Descripción de los componentes de este mandato</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td>Sustituya <code>&lt;cluster_name_or_id&gt;</code> por el nombre o el ID del clúster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td>Sustituya <code>&lt;subnet_size&gt;</code> por el número de direcciones IP que desea crear en la subred portátil. Los valores aceptados son 8, 16, 32 o 64. <p class="note"> Cuando añade direcciones IP portátiles para la subred, se utilizan tres direcciones IP para establecer conexión por red clúster-interna. No puede utilizar estas tres direcciones IP para los equilibradores de carga de aplicación (ALB) de Ingress ni para crear servicios de equilibrador de carga de red (NLB). Por ejemplo, si solicita ocho direcciones IP públicas portátiles, puede utilizar cinco de ellas para exponer sus apps al público.</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td>Sustituya <code>&lt;VLAN_ID&gt;</code> con el ID de la VLAN pública o privada a la que desea asignar las direcciones IP públicas o privadas portátiles. Debe seleccionar una VLAN pública o privada a la que está conectado el nodo trabajador existente. Para revisar las VLAN públicas o privadas a las que están conectados los nodos trabajadores, ejecute <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code> y busque la sección <strong>Subnet VLANs</strong> en la salida. La subred se suministra en la misma zona en la que se encuentra la VLAN.</td>
    </tr>
    </tbody></table>

2. Compruebe que la subred se haya creado y añadido correctamente al clúster. El CIDR de subred se muestra en la sección **Subnet VLANs**.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

    En esta salida de ejemplo, se ha añadido una segunda subred a la VLAN pública `2234945`:
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR          Public   User-managed
    2234947   10.xxx.xx.xxx/29     false    false
    2234945   169.xx.xxx.xxx/29    true     false
    2234945   169.xx.xxx.xxx/29    true     false
    ```
    {: screen}

3. **Importante**: para habilitar la comunicación entre nodos trabajadores que están en distintas subredes en la misma VLAN, debe [habilitar el direccionamiento entre subredes en la misma VLAN](#subnet-routing).

<br />


### Adición de IP privadas portátiles mediante la adición de subredes gestionadas por el usuario a VLAN privadas
{: #subnet_user_managed}

Puede obtener más IP privadas portátiles para los servicios de equilibrador de carga de red (NLB) creando una subred desde una red local disponible para el clúster.
{:shortdesc}

¿Desea reutilizar las subredes portátiles existentes en la cuenta de infraestructura de IBM Cloud (SoftLayer) en su lugar? Consulte [Utilización de subredes de la infraestructura de IBM Cloud (SoftLayer) personalizadas o existentes para crear un clúster](#subnets_custom).
{: tip}

Requisitos:
- Las subredes gestionadas por el usuario solo se pueden añadir a VLAN privadas.
- El límite de longitud del prefijo de subred es de /24 a /30. Por ejemplo, `169.xx.xxx.xxx/24` especifica 253 direcciones IP privadas que se pueden utilizar, mientras que `169.xx.xxx.xxx/30` especifica 1 dirección IP privada que se puede utilizar.
- La primera dirección IP de la subred se debe utilizar como pasarela para la subred.

Antes de empezar:
- Configure el direccionamiento del tráfico de red de entrada y de salida de la subred externa.
- Confirme que tiene conectividad de VPN entre la pasarela de red del centro de datos local y el Virtual Router Appliance de red privada o el servicio de VPN strongSwan que se ejecuta en el clúster. Para obtener más información, consulte [Configuración de la conectividad de VPN](/docs/containers?topic=containers-vpn).
-  Asegúrese de tener el [rol de plataforma **Operador** o **Administrador** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para el clúster.
- [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)


Para añadir una subred desde una red local:

1. Vea el ID de la VLAN privada del clúster. Localice la sección **Subnet VLANs**. En el campo **User-managed**, identifique el ID de VLAN con _false_.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    Subnet VLANs
  VLAN ID   Subnet CIDR         Public   User-managed
  2234947   10.xxx.xx.xxx/29    false    false
  2234945   169.xx.xxx.xxx/29  true    false
    ```
    {: screen}

2. Añada la subred externa a la VLAN privada. Las direcciones IP privadas portátiles se añaden al mapa de configuración del clúster.

    ```
    ibmcloud ks cluster-user-subnet-add --cluster <cluster_name> --subnet-cidr <subnet_CIDR> --private-vlan <private_VLAN>
    ```
    {: pre}

    Ejemplo:

    ```
    ibmcloud ks cluster-user-subnet-add --cluster mycluster --subnet-cidr 10.xxx.xx.xxx/24 --private-vlan 2234947
    ```
    {: pre}

3. Verifique que se ha añadido la subred proporcionada por el usuario. El campo **User-managed** es _true_.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    En esta salida de ejemplo, se ha añadido una segunda subred a la VLAN privada `2234947`:
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    2234947   10.xxx.xx.xxx/24  false    true
    ```
    {: screen}

4. [Habilite el direccionamiento entre subredes en la misma VLAN](#subnet-routing).

5. Añada un [servicio de equilibrador de carga de red (NLB) privado](/docs/containers?topic=containers-loadbalancer) o habilite un [ALB de Ingress privado](/docs/containers?topic=containers-ingress#private_ingress) para acceder a la app a través de la red privada. Para utilizar una dirección IP privada de la subred que ha añadido, debe especificar una dirección IP del CIDR de la subred. De lo contrario, se elige una dirección IP aleatoria de las subredes de infraestructura de IBM Cloud (SoftLayer) o las subredes proporcionadas por el usuario en la VLAN privada.

<br />


## Gestión del direccionamiento de subred
{: #subnet-routing}

Si tiene varias VLAN para un clúster, varias subredes en la misma VLAN o un clúster multizona, debe habilitar la [función de direccionador virtual (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para la cuenta de infraestructura de IBM Cloud (SoftLayer) para que los nodos trabajadores puedan comunicarse entre sí en la red privada. Para habilitar VRF, [póngase en contacto con el representante de su cuenta de la infraestructura de IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si no puede o no desea habilitar VRF, habilite la [distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para llevar a cabo esta acción, necesita el [permiso de la infraestructura](/docs/containers?topic=containers-users#infra_access) **Red > Gestionar distribución de VLAN de red** o bien puede solicitar al propietario de la cuenta que lo habilite. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.

Revise los siguientes casos de ejemplo en los que también es necesaria la distribución de VLAN.

### Habilitación del direccionamiento entre subredes primarias en la misma VLAN
{: #vlan-spanning}

Cuando se crea un clúster, las subredes públicas y privadas primarias se suministran en las VLAN públicas y privadas. La subred pública primaria termina en `/28` y proporciona 14 IP públicas para los nodos trabajadores. La subred privada primaria termina en `/26` y proporciona IP privadas para un máximo de 62 nodos trabajadores.
{:shortdesc}

Puede superar las 14 IP iniciales públicas y 62 IP privadas iniciales para los nodos trabajadores si tiene un clúster grande o varios clústeres pequeños en la misma ubicación en la misma VLAN. Cuando una subred pública o privada alcanza el límite de nodos trabajadores, se solicita otra subred primaria en la misma VLAN.

Para asegurarse de que los nodos trabajadores de estas subredes primarias de la misma VLAN pueden comunicarse, debe activar la distribución de VLAN. Para obtener instrucciones, consulte [Habilitar o inhabilitar la distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning).

Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.
{: tip}

### Gestión del direccionamiento de subred para dispositivos de pasarela
{: #vra-routing}

Cuando se crea un clúster, se solicita una subred pública portátil y una subred privada portátil en las VLAN a las que está conectado el clúster. Estas subredes proporcionan direcciones IP para los servicios de equilibrador de carga de aplicaciones (ALB) de Ingress y de equilibrador de carga de red (NLB).
{: shortdesc}

Sin embargo, si tiene un dispositivo direccionador existente, como por ejemplo un [dispositivo direccionador virtual (VRA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra#about-the-vra), las subredes portátiles recién añadidas de las VLAN a las que está conectado el clúster no están configuradas en el direccionador. Para utilizar los NLB o ALB de Ingress, debe asegurarse de que los dispositivos de red pueden direccionar entre distintas subredes de la misma VLAN [habilitando la distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning).

Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.
{: tip}
