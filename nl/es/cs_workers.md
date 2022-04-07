---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

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



# Adición de nodos trabajadores y de zonas a clústeres
{: #add_workers}

Para aumentar la disponibilidad de las apps, puede añadir nodos trabajadores a una zona existente o a varias zonas existentes en el clúster. Para ayudar a proteger las apps frente a anomalías de zona, puede añadir zonas al clúster.
{:shortdesc}

Cuando se crea un clúster, los nodos trabajadores se suministran en una agrupación de nodos trabajadores. Después de la creación del clúster, puede añadir más nodos trabajadores a una agrupación cambiando el tamaño de la agrupación o añadiendo más agrupaciones de nodos trabajadores. De forma predeterminada, la agrupación de nodos trabajadores existe en una zona. Los clústeres que tienen una agrupación de nodos trabajadores en una sola zona se denominan clústeres de una sola zona. Cuando se añaden más zonas al clúster, la agrupación de nodos trabajadores existe entre las zonas. Los clústeres que tienen una agrupación de trabajadores distribuida entre más de una zona se denominan clústeres multizona.

Si tiene un clúster multizona, mantenga equilibrados los recursos de los nodos trabajadores. Asegúrese de que todas las agrupaciones de nodos trabajadores estén distribuidas entre las mismas zonas y añada o elimine nodos trabajadores cambiando el tamaño de las agrupaciones en lugar de añadir nodos individuales.
{: tip}

Antes de comenzar, asegúrese de tener el [rol de plataforma **Operador** o **Administrador** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform). A continuación, elija una de las secciones siguientes:
  * [Añadir nodos trabajadores cambiando el tamaño de una agrupación de nodos trabajadores existente en el clúster](#resize_pool)
  * [Añadir nodos trabajadores añadiendo al clúster una agrupación de nodos trabajadores](#add_pool)
  * [Añadir una zona al clúster y replicar los nodos trabajadores de las agrupaciones de nodos trabajadores entre varias zonas](#add_zone)
  * [En desuso: añada al clúster un nodo trabajador autónomo](#standalone)

Después de configurar la agrupación de nodos trabajadores, puede [configurar el programa de escalado automático de clústeres](/docs/containers?topic=containers-ca#ca) para añadir o eliminar automáticamente nodos trabajadores de las agrupaciones de nodos trabajadores en función de las solicitudes de recursos de la carga de trabajo.
{:tip}

## Adición de nodos trabajadores mediante el redimensionando de una agrupación de nodos trabajadores existente
{: #resize_pool}

Puede añadir o reducir el número de nodos trabajadores del clúster redimensionando una agrupación de nodos trabajadores existente, independientemente de si la agrupación de nodos trabajadores está en una zona o está distribuida entre varias zonas.
{: shortdesc}

Por ejemplo, supongamos que tiene un clúster con una agrupación de nodos trabajadores que tiene tres nodos trabajadores por zona.
* Si el clúster está en una sola zona y existe en `dal10`, la agrupación de nodos trabajadores tiene tres nodos trabajadores en `dal10`. El clúster tiene un total de tres nodos trabajadores.
* Si el clúster es multizona y existe en `dal10` y en `dal12`, la agrupación de nodos trabajadores tiene tres nodos trabajadores en `dal10` y tres nodos trabajadores en `dal12`. El clúster tiene un total de seis nodos trabajadores.

Para las agrupaciones de nodos trabajadores nativas, tenga en cuenta que la facturación es mensual. El redimensionamiento de la agrupación afectará a sus costes mensuales.
{: tip}

Para redimensionar la agrupación de nodos trabajadores, cambie el número de nodos trabajadores que la agrupación de nodos trabajadores despliega en cada zona:

1. Obtenga el nombre de la agrupación de nodos trabajadores que desea redimensionar.
    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

2. Redimensione la agrupación de nodos trabajadores designando el número de nodos trabajadores que desea desplegar en cada zona. El valor mínimo es 1.
    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <pool_name>  --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

3. Verifique que se ha cambiado el tamaño de la agrupación de nodos trabajadores.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
    ```
    {: pre}

    Salida de ejemplo correspondiente a una agrupación de nodos trabajadores que está en dos zonas, `dal10` y `dal12`, y que se redimensiona a dos nodos trabajadores por zona:
    ```
    ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal10   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w9   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    kube-dal12-crb20b637238ea471f8d4a8b881aae4962-w10  169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          normal   Ready   dal12   1.8.6_1504
    ```
    {: screen}

## Adición de nodos trabajadores mediante la creación de una nueva agrupación de nodos trabajadores
{: #add_pool}

Puede añadir nodos trabajadores a un clúster creando una nueva agrupación de nodos trabajadores.
{:shortdesc}

1. Recupere las **Worker Zones** (zonas de trabajo) del clúster y elija la zona donde desea desplegar los nodos trabajadores en la agrupación de nodos trabajadores. Si tiene un clúster de una sola zona, debe utilizar la zona que puede ver en el campo **Worker Zones**. Para clústeres multizona, puede elegir cualquiera de las
**Worker Zones** existentes del clúster, o añadir una de las [ubicaciones metropolitanas multizona](/docs/containers?topic=containers-regions-and-zones#zones) de la región en la que se encuentra el clúster. Para obtener una lista de las zonas disponibles, ejecute `ibmcloud ks zones`.
   ```
   ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
   ```
   {: pre}

   Salida de ejemplo:
   ```
   ...
   Worker Zones: dal10, dal12, dal13
   ```
   {: screen}

2. Para cada zona, obtenga una lista de las VLAN privadas y públicas disponibles. Anote las VLAN privadas y públicas que desea utilizar. Si no tiene una VLAN privada o pública, la VLAN se crea automáticamente cuando añade una zona a la agrupación de nodos trabajadores.
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3.  Para cada zona, revise los [tipos de máquina disponibles para los nodos trabajadores](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes).

    ```
    ibmcloud ks machine-types <zone>
    ```
    {: pre}

4. Cree una agrupación de nodos trabajadores. Incluya la opción `--labels` para etiquetar automáticamente los nodos trabajadores que están en la agrupación con la etiqueta `key=value`. Si suministra una agrupación de nodos trabajadores nativos, especifique `--hardware dedicated`.
   ```
   ibmcloud ks worker-pool-create --name <pool_name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_workers_per_zone> --hardware <dedicated_or_shared> --labels <key=value>
   ```
   {: pre}

5. Verifique que la agrupación de nodos trabajadores se ha creado.
   ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
   ```
   {: pre}

6. De forma predeterminada, la adición de una agrupación de nodos trabajadores crea una agrupación sin zonas. Para desplegar nodos trabajadores en una zona, debe añadir las zonas que ha recuperado anteriormente a la agrupación de nodos trabajadores. Si desea distribuir los nodos trabajadores en varias zonas, repita este mandato para cada zona.
   ```
   ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
   ```
   {: pre}

7. Verifique que los nodos trabajadores se suministran en la zona que ha añadido. Los nodos trabajadores están listos cuando el estado cambia de **provision_pending** a **normal**.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
   ```
   {: pre}

   Salida de ejemplo:
   ```
   ID                                                 Public IP        Private IP      Machine Type      State    Status  Zone    Version
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w7   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   kube-dal10-crb20b637238ea471f8d4a8b881aae4962-w8   169.xx.xxx.xxx   10.xxx.xx.xxx   b3c.4x16          provision_pending   Ready   dal10   1.8.6_1504
   ```
   {: screen}

## Adición de nodos trabajadores mediante la adición de una zona a una agrupación de nodos trabajadores
{: #add_zone}

Puede distribuir el clúster entre varias zonas dentro de una región mediante la adición de una zona a su agrupación de nodos trabajadores existente.
{:shortdesc}

Cuando se añade una zona a una agrupación de nodos trabajadores, los nodos trabajadores definidos en la agrupación de nodos trabajadores se suministran en la nueva zona y se tienen en cuenta para la futura planificación de la carga de trabajo. {{site.data.keyword.containerlong_notm}} añade automáticamente a cada nodo trabajador la etiqueta `failure-domain.beta.kubernetes.io/region` correspondiente a la región y la etiqueta `failure-domain.beta.kubernetes.io/zone` correspondiente a la zona. El planificador de Kubernetes utiliza estas etiquetas para distribuir los pods entre zonas de la misma región.

Si tiene varias agrupaciones de nodos trabajadores en el clúster, añada la zona a todas ellas para que los nodos trabajadores se distribuya uniformemente en el clúster.

Antes de empezar:
*  Para añadir una zona a la agrupación de nodos trabajadores, la agrupación de nodos trabajadores debe estar en una [zona con soporte multizona](/docs/containers?topic=containers-regions-and-zones#zones). Si la agrupación de nodos trabajadores no está en una zona con soporte multizona, tenga en cuenta la posibilidad de [crear una nueva agrupación de nodos trabajadores](#add_pool).
*  Si tiene varias VLAN para un clúster, varias subredes en la misma VLAN o un clúster multizona, debe habilitar la [función de direccionador virtual (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para la cuenta de infraestructura de IBM Cloud (SoftLayer) para que los nodos trabajadores puedan comunicarse entre sí en la red privada. Para habilitar VRF, [póngase en contacto con el representante de su cuenta de la infraestructura de IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si no puede o no desea habilitar VRF, habilite la [distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para llevar a cabo esta acción, necesita el [permiso de la infraestructura](/docs/containers?topic=containers-users#infra_access) **Red > Gestionar distribución de VLAN de red** o bien puede solicitar al propietario de la cuenta que lo habilite. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.

Para añadir una zona con nodos trabajadores a la agrupación de nodos trabajadores:

1. Obtenga una lista de las zonas disponibles y elija la zona que desea añadir a la agrupación de nodos trabajadores. La zona que elija debe ser una zona con soporte multizona.
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. Obtenga una lista de las VLAN de dicha zona. Si no tiene una VLAN privada o pública, la VLAN se crea automáticamente cuando añade una zona a la agrupación de nodos trabajadores.
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. Obtenga una lista de las agrupaciones de nodos trabajadores de su clúster y anote sus nombres.
   ```
   ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
   ```
   {: pre}

4. Añada la zona a la agrupación de nodos trabajadores. Si tiene varias agrupaciones de nodos trabajadores, añada la zona a todas las agrupaciones de nodos trabajadores de forma que el clúster quede equilibrado en todas las zonas. Sustituya `<pool1_id_or_name,pool2_id_or_name,...>` por los nombres de todas sus agrupaciones de nodos trabajadores en una lista separada por comas.

    Debe existir una VLAN privada y pública para que pueda añadir una zona a varias agrupaciones de nodos trabajadores. Si no tiene una VLAN privada y pública en dicha zona, añada primero la zona a una agrupación de nodos trabajadores para que se cree una VLAN privada y pública. A continuación, puede añadir la zona a otras agrupaciones de nodos trabajadores especificando la VLAN privada y pública que se ha creado automáticamente.
    {: note}

   Si desea utilizar distintas VLAN para distintas agrupaciones de nodos trabajadores, repita este mandato para cada VLAN y sus agrupaciones de nodos trabajadores correspondientes. Los nuevos nodos trabajadores se añaden a las VLAN que especifique, pero las VLAN de los nodos trabajadores existentes no se modifican.
   {: tip}
   ```
   ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool1_name,pool2_name,...> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
   ```
   {: pre}

5. Verifique que la zona se ha añadido al clúster. Busque la zona añadidos en el campo **Worker zones** de la información de salida. Observe que el número total de nodos trabajadores del campo **Workers** ha aumentado ya que se han suministrado nuevos nodos trabajadores en la zona que se ha añadido.
  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Salida de ejemplo:
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
  Resource Group ID:              a8a12accd63b437bbd6d58fb6a462ca7
  Resource Group Name:            Default
  ```
  {: screen}

## En desuso: Adición de nodos trabajadores autónomos
{: #standalone}

Si tiene un clúster que se ha creado antes de la introducción de las agrupaciones de nodos trabajadores, puede utilizar los mandatos en desuso para añadir nodos trabajadores autónomos.
{: deprecated}

Si tiene un clúster que se ha creado después de que se hayan incorporado las agrupaciones de nodos trabajadores, no puede añadir nodos trabajadores autónomos. En su lugar, puede [crear una agrupación de nodos trabajadores](#add_pool), [redimensionar la agrupación de nodos trabajadores existente](#resize_pool) o [añadir una zona a una agrupación de nodos trabajadores](#add_zone) para añadir nodos trabajadores al clúster.
{: note}

1. Obtenga una lista de las zonas disponibles y elija la zona en la desea añadir nodos trabajadores.
   ```
   ibmcloud ks zones
   ```
   {: pre}

2. Obtenga una lista de las VLAN disponibles en dicha zona y anote sus ID.
   ```
   ibmcloud ks vlans --zone <zone>
   ```
   {: pre}

3. Obtenga una lista de los tipos de máquina de dicha zona.
   ```
   ibmcloud ks machine-types --zone <zone>
   ```
   {: pre}

4. Añada nodos trabajadores autónomos al clúster. Para los tipos de máquina nativos, especifique `dedicated`.
   ```
   ibmcloud ks worker-add --cluster <cluster_name_or_ID> --workers <number_of_worker_nodes> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --machine-type <machine_type> --hardware <shared_or_dedicated>
   ```
   {: pre}

5. Verifique que los nodos trabajadores se han creado.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## Adición de etiquetas a agrupaciones de nodos trabajadores existentes
{: #worker_pool_labels}

Puede asignar una etiqueta a una agrupación de nodos trabajadores cuando [cree la agrupación de nodos trabajadores](#add_pool) o más tarde, siguiendo estos pasos. Después de que se haya etiquetado una agrupación de nodos trabajadores, todos los nodos trabajadores existentes y posteriores obtienen esta etiqueta. Puede utilizar etiquetas para desplegar cargas de trabajo específicas solo en los nodos trabajadores de la agrupación de nodos trabajadores, como por ejemplo los [nodos de extremo para el tráfico de red del equilibrador de carga](/docs/containers?topic=containers-edge).
{: shortdesc}

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Obtenga una lista de las agrupaciones de nodos trabajadores del clúster.
    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}
2.  Para etiquetar la agrupación de nodos trabajadores con una etiqueta `key=value`, utilice la [API de agrupación de nodos trabajadores PATCH ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool). Formatee el cuerpo de la solicitud como en el siguiente ejemplo JSON.
    ```
    {
      "labels": {"key":"value"},
      "state": "labels"
    }
    ```
    {: codeblock}
3.  Verifique que la agrupación de nodos trabajadores y el nodo trabajador tienen la etiqueta `key=value` que ha asignado.
    *   Para comprobar las agrupaciones de nodos trabajadores:
        ```
        ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
        ```
        {: pre}
    *   Para comprobar los nodos trabajadores:
        1.  Obtenga una lista de los nodos trabajadores de la agrupación de nodos trabajadores y anote la **IP privada**.
            ```
            ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
            ```
            {: pre}
        2.  Revise el **Labels** de la salida.
            ```
            kubectl describe node <worker_node_private_IP>
            ```
            {: pre}

Después de etiquetar la agrupación de nodos trabajadores, puede utilizar la [etiqueta en los despliegues de la app](/docs/containers?topic=containers-app#label) para que las cargas de trabajo se ejecuten solo en estos nodos trabajadores, o puede definirlos con [antagonismo ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) para evitar que los despliegues se ejecuten en estos nodos trabajadores.

<br />


## Recuperación automática para los nodos trabajadores
{: #planning_autorecovery}

Los componentes críticos, como `containerd`, `kubelet`, `kube-proxy` y `calico`, deben ser funcionales para tener un nodo trabajador de Kubernetes en buen estado. Con el tiempo, estos componentes se pueden estropear dejando así el nodo trabajador en estado fuera de servicio. Los nodos trabajadores averiados reducen la capacidad total del clúster y pueden provocar tiempo de inactividad en la app.
{:shortdesc}

Puede [configurar comprobaciones de estado del nodo trabajador y habilitar la recuperación automática](/docs/containers?topic=containers-health#autorecovery). Si la recuperación automática detecta un nodo trabajador erróneo basado en las comprobaciones configuradas, desencadena una acción correctiva, como una recarga del sistema operativo, en el nodo trabajador. Para obtener más información sobre cómo funciona la recuperación automática, consulte el [blog sobre recuperación automática ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).
