---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

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


# Cambio de puntos finales de servicio o de conexiones de VLAN
{: #cs_network_cluster}

Después de configurar inicialmente la red cuando [cree un clúster](/docs/containers?topic=containers-clusters), puede cambiar los puntos finales de servicio a través de los que se puede acceder al nodo maestro de Kubernetes o puede cambiar las conexiones de VLAN para los nodos trabajadores.
{: shortdesc}

## Configuración del punto final de servicio privado
{: #set-up-private-se}

En los clústeres que ejecutan Kubernetes versión 1.11 o posteriores, habilite o inhabilite el punto final de servicio privado para el clúster.
{: shortdesc}

El punto final de servicio privado hace que el maestro de Kubernetes sea de acceso privado. Los nodos trabajadores y los usuarios autorizados del clúster se pueden comunicar con el maestro de Kubernetes a través de la red privada. Para determinar si puede habilitar el punto final de servicio privado, consulte [Comunicación entre nodo trabajador y maestro y entre usuario y maestro](/docs/containers?topic=containers-plan_clusters#internet-facing). Tenga en cuenta que no puede inhabilitar el punto final de servicio privado después de habilitarlo.

¿Ha creado un clúster solo con un punto final de servicio privado antes de habilitar la cuenta para [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) y [puntos finales de servicio](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)? Intente [configurar el punto final de servicio público](#set-up-public-se) de modo que pueda utilizar el clúster hasta que se procesen los casos de soporte para actualizar la cuenta.
{: tip}

1. Habilite [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) en la cuenta de la infraestructura de IBM Cloud (SoftLayer).
2. [Habilite la cuenta de {{site.data.keyword.Bluemix_notm}} para que utilice puntos finales de servicio](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
3. Habilite el punto final de servicio privado.
   ```
   ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
4. Renueve el servidor de API del maestro de Kubernetes para que utilice el punto final de servicio privado. Puede seguir la indicación en la CLI o puede ejecutar manualmente el mandato siguiente.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}

5. [Cree un mapa de configuración](/docs/containers?topic=containers-update#worker-up-configmap) para controlar el número máximo de nodos trabajadores que pueden no estar disponibles a la vez en el clúster. Cuando actualiza los nodos trabajadores, el mapa de correlación ayuda a evitar tiempo de inactividad para las apps a medida que las apps se replanifican de forma ordenada en los nodos trabajadores disponibles.
6. Actualice todos los nodos trabajadores del clúster para que adopten la configuración de punto final de servicio privado.

   <p class="important">Con el siguiente mandato de actualización, los nodos trabajadores se vuelven a cargar para que adopten la configuración de punto final de servicio. Si no hay ninguna actualización de nodo trabajador disponible, debe [volver a cargar manualmente los nodos trabajadores](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli). Si los vuelve a cargar, asegúrese de delimitar, drenar y gestionar el orden para controlar el número máximo de nodos trabajadores que no están disponibles a la vez.</p>
   ```
   ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
   ```
   {: pre}

8. Si el clúster se encuentra en un entorno detrás de un cortafuegos:
  * [Permita a los usuarios autorizados del clúster ejecutar mandatos `kubectl` para acceder al maestro a través del punto final de servicio privado.](/docs/containers?topic=containers-firewall#firewall_kubectl)
  * [Permita el tráfico de red de salida a las IP privadas](/docs/containers?topic=containers-firewall#firewall_outbound) para los recursos de la infraestructura y para los servicios de {{site.data.keyword.Bluemix_notm}} que tiene previsto utilizar.

9. Opcional: Para utilizar solo el punto final de servicio privado, inhabilite el punto final de servicio público.
   ```
   ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## Configuración de punto final de servicio público
{: #set-up-public-se}

Habilite o inhabilite el punto final de servicio público para el clúster.
{: shortdesc}

El punto final de servicio público hace que el maestro de Kubernetes sea de acceso público. Los nodos trabajadores y los usuarios autorizados del clúster se pueden comunicar de forma segura con el maestro de Kubernetes a través de la red pública. Para obtener más información, consulte [Comunicación entre nodo trabajador y maestro y entre usuario y maestro](/docs/containers?topic=containers-plan_clusters#internet-facing).

**Pasos para habilitarlo**</br>
Si ya ha inhabilitado el punto final público, puede volver a habilitarlo.
1. Habilite el punto final de servicio público.
   ```
   ibmcloud ks cluster-feature-enable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
2. Renueve el servidor de API del maestro de Kubernetes para que utilice el punto final de servicio público. Puede seguir la indicación en la CLI o puede ejecutar manualmente el mandato siguiente.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}

   </br>

**Pasos para inhabilitar**</br>
Para inhabilitar el punto final de servicio público, primero debe habilitar el punto final de servicio privado para que los nodos trabajadores se puedan comunicar con el maestro de Kubernetes.
1. Habilite el punto final de servicio privado.
   ```
   ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
2. Renueve el servidor de API del maestro de Kubernetes para que utilice el punto final de servicio privado siguiendo la indicación de la CLI o ejecutando manualmente el mandato siguiente.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}
3. [Cree un mapa de configuración](/docs/containers?topic=containers-update#worker-up-configmap) para controlar el número máximo de nodos trabajadores que pueden no estar disponibles a la vez en el clúster. Cuando actualiza los nodos trabajadores, el mapa de correlación ayuda a evitar tiempo de inactividad para las apps a medida que las apps se replanifican de forma ordenada en los nodos trabajadores disponibles.

4. Actualice todos los nodos trabajadores del clúster para que adopten la configuración de punto final de servicio privado.

   <p class="important">Con el siguiente mandato de actualización, los nodos trabajadores se vuelven a cargar para que adopten la configuración de punto final de servicio. Si no hay ninguna actualización de nodo trabajador disponible, debe [volver a cargar manualmente los nodos trabajadores](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli). Si los vuelve a cargar, asegúrese de delimitar, drenar y gestionar el orden para controlar el número máximo de nodos trabajadores que no están disponibles a la vez.</p>
   ```
   ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
   ```
  {: pre}
5. Inhabilite el punto final de servicio público.
   ```
   ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}

## Cómo pasar de un punto final de servicio público a un punto final de servicio privado
{: #migrate-to-private-se}

En clústeres que ejecutan Kubernetes versión 1.11 o posteriores, habilite los nodos trabajadores para que se comuniquen con el maestro a través de la red privada en lugar de la red pública habilitando el punto final de servicio privado.
{: shortdesc}

Todos los clústeres que están conectados a una VLAN pública y a una privada utilizan de forma predeterminada el punto final de servicio público. Los nodos trabajadores y los usuarios autorizados del clúster se pueden comunicar de forma segura con el maestro de Kubernetes a través de la red pública. Para permitir que los nodos trabajadores se comuniquen con el maestro de Kubernetes a través de la red privada en lugar de hacerlo a través de la red pública, puede habilitar el punto final de servicio privado. A continuación, puede inhabilitar si lo desea el punto final de servicio público.
* Si habilita el punto final de servicio privado y mantiene el punto final de servicio público también habilitado, los nodos trabajadores siempre se comunican con el maestro a través de la red privada, pero los usuarios se pueden comunicar con el maestro a través de la red pública o privada.
* Si habilita el punto final de servicio privado pero inhabilita el punto final de servicio público, los nodos trabajadores y los usuarios deben comunicarse con el maestro a través de la red privada.

Tenga en cuenta que no puede inhabilitar el punto final de servicio privado después de habilitarlo.

1. Habilite [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) en la cuenta de la infraestructura de IBM Cloud (SoftLayer).
2. [Habilite la cuenta de {{site.data.keyword.Bluemix_notm}} para que utilice puntos finales de servicio](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
3. Habilite el punto final de servicio privado.
   ```
   ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
4. Renueve el servidor de API del maestro de Kubernetes para que utilice el punto final de servicio privado siguiendo la indicación de la CLI o ejecutando manualmente el mandato siguiente.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}
5. [Cree un mapa de configuración](/docs/containers?topic=containers-update#worker-up-configmap) para controlar el número máximo de nodos trabajadores que pueden no estar disponibles a la vez en el clúster. Cuando actualiza los nodos trabajadores, el mapa de correlación ayuda a evitar tiempo de inactividad para las apps a medida que las apps se replanifican de forma ordenada en los nodos trabajadores disponibles.

6.  Actualice todos los nodos trabajadores del clúster para que adopten la configuración de punto final de servicio privado.

    <p class="important">Con el siguiente mandato de actualización, los nodos trabajadores se vuelven a cargar para que adopten la configuración de punto final de servicio. Si no hay ninguna actualización de nodo trabajador disponible, debe [volver a cargar manualmente los nodos trabajadores](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli). Si los vuelve a cargar, asegúrese de delimitar, drenar y gestionar el orden para controlar el número máximo de nodos trabajadores que no están disponibles a la vez.</p>
    ```
    ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
    ```
    {: pre}

7. Opcional: Inhabilite el punto final de servicio público.
   ```
   ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## Cambio de las conexiones de VLAN de nodo trabajador
{: #change-vlans}

Cuando cree un clúster, debe elegir si desea conectar los nodos trabajadores a una VLAN privada y pública o solo a una VLAN privada. Los nodos trabajadores forman parte de agrupaciones de nodos trabajadores, que almacenan metadatos de red que incluyen las VLAN que se utilizarán para suministrar nodos trabajadores futuros en la agrupación. Es posible que desee cambiar posteriormente la configuración de la conectividad de VLAN del clúster, en casos como los siguientes.
{: shortdesc}

* Las VLAN de la agrupación de nodos trabajadores de una zona se han quedado sin capacidad y es necesario suministrar una nueva VLAN para que los nodos trabajadores de clúster la utilicen.
* Tiene un clúster con nodos trabajadores que están en VLAN públicas y privadas, pero desea cambiar a un [clúster solo privado](/docs/containers?topic=containers-plan_clusters#private_clusters).
* Tiene un clúster solo privado, pero desea que algunos nodos trabajadores, como por ejemplo una agrupación de nodos trabajadores de [nodos de extremo](/docs/containers?topic=containers-edge#edge) en la VLAN pública, expongan las apps en Internet.

¿Está intentando cambiar el punto final de servicio para la comunicación entre maestro y trabajador? Consulte los temas para configurar puntos finales de servicio [públicos](#set-up-public-se) y [privados](#set-up-private-se).
{: tip}

Antes de empezar:
* [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
* Si los nodos trabajadores son autónomos (no forman parte de una agrupación de nodos trabajadores), [actualícelos a agrupaciones de nodos trabajadores](/docs/containers?topic=containers-update#standalone_to_workerpool).

Para cambiar las VLAN que utiliza una agrupación de nodos trabajadores para suministrar nodos trabajadores:

1. Obtenga una lista de los nombres de las agrupaciones de nodos trabajadores del clúster.
  ```
  ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. Determine las zonas para una de las agrupaciones de nodos trabajadores. En la salida, busque el campo **Zones**.
  ```
  ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <pool_name>
  ```
  {: pre}

3. Para cada zona que haya encontrado en el paso anterior, obtenga una VLAN pública y una privada disponibles que sean compatibles entre sí.

  1. Examine las VLAN públicas y privadas disponibles que se muestran bajo **Type** en la salida.
     ```
     ibmcloud ks vlans --zone <zone>
     ```
     {: pre}

  2. Compruebe que las VLAN públicas y privadas de la zona son compatibles. Para que sean compatibles, el **Router** debe tener el mismo ID de pod. En esta salida de ejemplo, los ID de pod de **Router** coinciden: `01a` y `01a`. Si un ID de pod fuese `01a` y el otro fuese `02a`, no se podrían establecer estos ID de VLAN pública y privada para la agrupación de trabajadores.
     ```
     ID        Name   Number   Type      Router         Supports Virtual Workers
    229xxxx          1234     private   bcr01a.dal12   true
    229xxxx          5678     public    fcr01a.dal12   true
     ```
     {: screen}

  3. Si necesita solicitar una nueva VLAN pública o privada para la zona, puede hacerlo en la [consola de {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans) o puede utilizar el mandato siguiente. Recuerde que las VLAN deben ser compatibles, con ID de pod de **Router** coincidentes como en el paso anterior. Si va a crear un par de nuevas VLAN pública y privada, estas deben ser compatibles entre sí.
     ```
     ibmcloud sl vlan create -t [public|private] -d <zone> -r <compatible_router>
     ```
     {: pre}

  4. Anote los ID de las VLAN compatibles.

4. Configure una agrupación de nodos trabajadores con los nuevos metadatos de red de VLAN para cada zona. Puede crear una nueva agrupación de nodos trabajadores o puede modificar una existente.

  * **Cree una nueva agrupación de trabajadores**: Consulte [adición de nodos trabajadores mediante la creación de una nueva agrupación de nodos trabajadores](/docs/containers?topic=containers-add_workers#add_pool).

  * **Modifique una agrupación de nodos trabajadores existente**: Establezca los metadatos de red de la agrupación de nodos trabajadores de modo que utilicen la VLAN para cada zona. Los nodos trabajadores que ya se han creado en la agrupación siguen utilizando las VLAN anteriores, pero los nuevos nodos trabajadores de la agrupación utilizan los nuevos metadatos de VLAN que ha definido.

    * Ejemplo para añadir VLAN tanto pública como privada, como en el caso de que se pase de solo privada a privada y pública:
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

    * Ejemplo para añadir solo una VLAN privada, como en el caso de que se pase de VLAN pública y privada a solo privada cuando se dispone de una [cuenta habilitada para VRF que utiliza puntos finales de servicio](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started):
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

5. Añada nodos trabajadores a la agrupación de nodos trabajadores cambiando el tamaño de la agrupación.
   ```
   ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <pool_name> --size-per-zone <number_of_workers_per_zone>
   ```
   {: pre}

   Si desea eliminar los nodos trabajadores que utilizan los metadatos de red anteriores, cambie el número de nodos trabajadores por zona para duplicar la cantidad anterior de nodos trabajadores por zona. Más adelante en estos pasos, puede delimitar, drenar y eliminar los nodos trabajadores anteriores.
  {: tip}

6. Verifique que los nuevos nodos trabajadores se crean con la **IP pública** y la **IP privada** adecua das en la salida. Por ejemplo, si cambia la agrupación de nodos trabajadores de una VLAN pública y privada a solo privada, los nuevos nodos trabajadores solo tendrán una IP privada. Si cambia la agrupación de nodos trabajadores de solo privada a VLAN pública y privada, los nuevos nodos trabajadores tienen IP tanto públicas como privadas.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
   ```
   {: pre}

7. Opcional: Elimine los nodos trabajadores con los metadatos de red anteriores de la agrupación de nodos trabajadores.
  1. En la salida del paso anterior, anote el **ID** y la **IP privada** de los nodos trabajadores que desea eliminar de la agrupación de nodos trabajadores.
  2. Marque el nodo trabajador como no programable en un proceso que se conoce como acordonamiento. Cuando se acordona un nodo trabajador, deja de estar disponible para programar pods en el futuro.
     ```
     kubectl cordon <worker_private_ip>
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
     kubectl drain <worker_private_ip>
     ```
     {: pre}
     Este proceso puede tardar unos minutos.
  5. Elimine el nodo trabajador. Utilice el ID de nodo trabajador que ha recuperado anteriormente.
     ```
     ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_name_or_ID>
     ```
     {: pre}
  6. Verifique que se ha eliminado el nodo trabajador.
     ```
     ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
     ```
     {: pre}

8. Opcional: Puede repetir los pasos del 2 al 7 correspondientes a cada agrupación de nodos trabajadores del clúster. Después de completar estos pasos, todos los nodos trabajadores del clúster estarán configurados con las nuevas VLAN.

9. Los ALB predeterminados del clúster siguen enlazados a la VLAN antigua porque sus direcciones IP proceden de una subred de dicha VLAN. Como los ALB no se pueden mover entre distintas VLAN, puede [crear ALB en las nuevas VLAN e inhabilitar los ALB de las VLAN antiguas](/docs/containers?topic=containers-ingress#migrate-alb-vlan).
