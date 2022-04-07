---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

keywords: kubernetes, iks, ImagePullBackOff, registry, image, failed to pull image,

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Resolución de problemas con clústeres y nodos trabajadores
{: #cs_troubleshoot_clusters}

Si utiliza {{site.data.keyword.containerlong}}, tenga en cuenta estas técnicas para resolver problemas relacionados con los clústeres y los nodos trabajadores.
{: shortdesc}

<p class="tip">Si tiene un problema más general, pruebe la [depuración del clúster](/docs/containers?topic=containers-cs_troubleshoot).<br>Además, al resolver problemas, puede utilizar la [herramienta de diagnósticos y de depuración de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) para ejecutar pruebas y recopilar información pertinente del clúster.</p>

## No se ha podido crear un clúster o gestionar nodos trabajadores debido a errores de permiso
{: #cs_credentials}

{: tsSymptoms}
Intenta gestionar nodos trabajadores para un clúster nuevo o existente ejecutando uno de los mandatos siguientes.
* Suministrar nodos trabajadores: `ibmcloud ks cluster-create`, `ibmcloud ks worker-pool-rebalance` o `ibmcloud ks worker-pool-resize`
* Volver a cargar nodos trabajadores: `ibmcloud ks worker-reload` o `ibmcloud ks worker-update`
* Reiniciar nodos trabajadores: `ibmcloud ks worker-reboot`
* Suprimir nodos trabajadores: `ibmcloud ks cluster-rm`, `ibmcloud ks worker-rm`, `ibmcloud ks worker-pool-rebalance` o `ibmcloud ks worker-pool-resize`

Pero recibe un mensaje de error parecido al siguiente.

```
No se ha podido conectar con la cuenta de IBM infraestructura de Cloud (SoftLayer).
Para crear un clúster estándar, debe tener una cuenta de Pago según uso enlazada a una cuenta de infraestructura de IBM Cloud (SoftLayer) o debe haber utilizado la CLI de {{site.data.keyword.containerlong_notm}} para configurar las claves de API de la infraestructura de {{site.data.keyword.Bluemix_notm}}.
```
{: screen}

```
Excepción de infraestructura de {{site.data.keyword.Bluemix_notm}}: El elemento 'Item' se debe solicitar con permiso.
```
{: screen}

```
No se ha encontrado el nodo trabajador. Revise los permisos de infraestructura de {{site.data.keyword.Bluemix_notm}}.
```
{: screen}

```
Excepción de infraestructura de {{site.data.keyword.Bluemix_notm}}: El usuario no tiene los permisos necesarios de la infraestructura de {{site.data.keyword.Bluemix_notm}} para añadir servidores
```
{: screen}

```
La solicitud de intercambio de señales IAM ha fallado: no se puede crear la señal del portal de IMS, ya que no hay ninguna cuenta de IMS enlazada a la cuenta de BSS seleccionada
```
{: screen}

```
El clúster no se ha podido configurar con el registro. Asegúrese de que tiene el rol de administrador para {{site.data.keyword.registrylong_notm}}.
```
{: screen}

{: tsCauses}
A las credenciales de infraestructura que se han establecido para la región y al grupo de recursos le faltan los [permisos de infraestructura](/docs/containers?topic=containers-access_reference#infra) adecuados. Los permisos de infraestructura del usuario se suelen guardar como una [clave de API](/docs/containers?topic=containers-users#api_key) para la región y el grupo de recursos. Menos habitualmente, si utiliza un [tipo de cuenta de {{site.data.keyword.Bluemix_notm}} diferente](/docs/containers?topic=containers-users#understand_infra), es posible que tenga [establecer manualmente las credenciales de la infraestructura](/docs/containers?topic=containers-users#credentials). Si utiliza una infraestructura de IBM Cloud (SoftLayer) distinta para suministrar recursos de infraestructura, es posible que tenga también [clústeres huérfanos](#orphaned) en la cuenta.

{: tsResolve}
El propietario de la cuenta debe configurar correctamente las credenciales de la cuenta de infraestructura. Las credenciales dependen del tipo de cuenta de infraestructura que esté utilizando.

Antes de empezar, [inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

1.  Identifique las credenciales de usuario que se utilizan para los permisos de la infraestructura de la región y del grupo de recursos.
    1.  Compruebe la clave de API para una región y un grupo de recursos del clúster.
        ```
        ibmcloud ks api-key-info --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Salida de ejemplo:
        ```
        Getting information about the API key owner for cluster <cluster_name>...
        OK
        Name                Email   
        <user_name>         <name@email.com>
        ```
        {: screen}
    2.  Compruebe si la cuenta de la infraestructura para la región y el grupo de recursos se ha establecido manualmente de modo que utilice otra cuenta de infraestructura de IBM Cloud (SoftLayer).
        ```
        ibmcloud ks credential-get --region <us-south>
        ```
        {: pre}

        **Salida de ejemplo si las credenciales se han establecidas de modo que utilicen otra cuenta**. En este caso, las credenciales de infraestructura del usuario se utilizan para la región y el grupo de recursos que ha seleccionado como destino, incluso si las credenciales de un usuario diferente se almacenan en la clave de API que ha recuperado en el paso anterior.
        ```
        OK
        Infrastructure credentials for user name <1234567_name@email.com> set for resource group <resource_group_name>.
        ```
        {: screen}

        **Salida de ejemplo si las credenciales no se han establecidas de modo que utilicen otra cuenta**. En este caso, el propietario de la clave de API que ha recuperado en el paso anterior tiene las credenciales de infraestructura que se utilizan para la región y el grupo de recursos.
        ```
        FAILED
        No credentials set for resource group <resource_group_name>.: The user credentials could not be found. (E0051)
        ```
        {: screen}
2.  Valide los permisos de infraestructura que tiene el usuario.
    1.  Obtenga una lista de los permisos de infraestructura recomendados y necesarios para la región y el grupo de recursos.
        ```
        ibmcloud ks infra-permissions-get --region <region>
        ```
        {: pre}
    2.  Asegúrese de que el [propietario de las credenciales de infraestructura para la clave de API o la cuenta definida manualmente tiene los permisos correctos](/docs/containers?topic=containers-users#owner_permissions).
    3.  Si es necesario, puede modificar la [clave de API](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset) o el propietario de las credenciales de infraestructura [establecido manualmente](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) para la región y el grupo de recursos.
3.  Pruebe que los permisos modificados permiten a los usuarios autorizados realizar operaciones de infraestructura para el clúster.
    1.  Por ejemplo, intente suprimir un nodo trabajador.
        ```
        ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}
    2.  Compruebe si el nodo trabajador se ha eliminado.
        ```
        ibmcloud ks worker-get --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}

        Salida de ejemplo si la eliminación del nodo trabajador se ejecuta correctamente. La operación `worker-get` falla porque el nodo trabajador se ha suprimido. Los permisos de infraestructura se han configurado correctamente.
        ```
        FAILED
        The specified worker node could not be found. (E0011)
        ```
        {: screen}

    3.  Si el nodo trabajador no se ha eliminado, revise los campos [**State** y **Status**](/docs/containers?topic=containers-cs_troubleshoot#debug_worker_nodes) y el apartado sobre [problemas comunes con nodos trabajadores](/docs/containers?topic=containers-cs_troubleshoot#common_worker_nodes_issues) para continuar con la depuración.
    4.  Si define las credenciales manualmente y todavía no puede ver los nodos trabajadores del clúster en la cuenta de infraestructura, compruebe si [el clúster está huérfano](#orphaned).

<br />


## El cortafuegos impide ejecutar mandatos de CLI
{: #ts_firewall_clis}

{: tsSymptoms}
Cuando ejecuta los mandatos `ibmcloud`, `kubectl` o `calicoctl` desde la CLI, fallan.

{: tsCauses}
Puede que tenga políticas de red corporativas que impidan el acceso desde el sistema local a los puntos finales públicos mediante proxies o cortafuegos.

{: tsResolve}
[Permita el acceso TCP para que funciones los mandatos de CLI](/docs/containers?topic=containers-firewall#firewall_bx). Esta tarea requiere el [rol de plataforma **Administrador** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para el clúster.


## No puedo acceder a los recursos de mi clúster
{: #cs_firewall}

{: tsSymptoms}
Cuando los nodos trabajadores del clúster no se pueden comunicar en la red privada, podría ver distintos síntomas.

- Mensaje de error de ejemplo cuando ejecuta `kubectl exec`, `attach`, `logs`, `proxy` o `port-forward`:
  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

- Mensaje de error de ejemplo cuando `kubectl proxy` se ejecuta correctamente, pero el panel de control de Kubernetes no está disponible:
  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}

- Mensaje de error de ejemplo cuando `kubectl proxy` falla o cuando falla la conexión con el servicio:
  ```
  Conexión rechazada
  ```
  {: screen}

  ```
  La conexión ha excedido el tiempo de espera
  ```
  {: screen}

  ```
  No se puede conectar con el servidor: net/http: tiempo de espera excedido de reconocimiento de TLS
  ```
  {: screen}


{: tsCauses}
Para acceder a los recursos del clúster, los nodos trabajadores deben poder comunicarse en la red privada. Es posible que tenga Vyatta u otro cortafuegos configurado o que se hayan personalizado los valores existentes del cortafuegos en la cuenta de infraestructura de IBM Cloud (SoftLayer). {{site.data.keyword.containerlong_notm}} requiere que determinadas direcciones IP y puertos estén abiertos para permitir la comunicación entre el nodo trabajador y el maestro de Kubernetes y viceversa. Si los nodos trabajadores están distribuidos en varias zonas, debe permitir la comunicación de red privada habilitando la distribución de VLAN. Es posible que no se puede establecer comunicación entre nodos trabajadores si los nodos trabajadores están bloqueados en un bucle de recarga.

{: tsResolve}
1. Obtenga una lista de los nodos trabajadores del clúster y verifique que los nodos trabajadores no están bloqueados en el estado `Reloading` (recargando).
   ```
   ibmcloud ks workers --cluster <cluster_name_or_id>
   ```
   {: pre}

2. Si tiene un clúster multizona y su cuenta no está habilitada para VRF, verifique que [ha habilitado la distribución de VLAN](/docs/containers?topic=containers-subnets#subnet-routing) para su cuenta.
3. Si tiene Vyatta o valores de cortafuegos personalizados, asegúrese de que [ha abierto los puertos necesarios](/docs/containers?topic=containers-firewall#firewall_outbound) para permitir que el clúster acceda a los recursos y servicios de la infraestructura.

<br />



## No se puede ver o trabajar con un clúster
{: #cs_cluster_access}

{: tsSymptoms}
* No puede encontrar un clúster. Cuando ejecuta `ibmcloud ks clusters`, el clúster no aparece en la lista de salida.
* No puede trabajar con un clúster. Cuando ejecuta `ibmcloud ks cluster-config` u otros mandatos específicos de clúster, no se encuentra el clúster.


{: tsCauses}
En {{site.data.keyword.Bluemix_notm}}, cada recurso debe estar en un grupo de recursos. Por ejemplo, es posible que el clúster `mycluster` exista en el grupo de recursos `default`. Cuando el propietario de la cuenta le da acceso a los recursos asignándole un rol de plataforma de {{site.data.keyword.Bluemix_notm}} IAM, el acceso puede ser para un recurso específico o para el grupo de recursos. Cuando se le da acceso a un recurso específico, no tiene acceso al grupo de recursos. En este caso, no es necesario que tenga como destino un grupo de recursos para trabajar con los clústeres a los que tiene acceso. Si tiene como destino un grupo de recursos distinto al del grupo en el que se encuentra el clúster, las acciones sobre dicho clúster pueden fallar. Por el contrario, si se le da acceso a un recurso como parte de su acceso a un grupo de recursos, debe elegir como destino un grupo de recursos para poder trabajar con un clúster de dicho grupo. Si no elige como destino de la sesión de CLI el grupo de recursos en el que se encuentra el clúster, las acciones sobre dicho clúster pueden fallar.

Si no encuentra o no puede trabajar con un clúster, es posible que esté experimentando uno de los problemas siguientes:
* Tiene acceso al clúster y al grupo de recursos en el que se encuentra el clúster, pero la sesión de CLI no tiene como destino el grupo de recursos en el que se encuentra el clúster.
* Tiene acceso al clúster, pero no como parte del grupo de recursos en el que se encuentra el clúster. La sesión de CLI tiene como destino este u otro grupo de recursos.
* No tiene acceso al clúster.

{: tsResolve}
Para comprobar sus permisos de acceso de usuario:

1. Obtenga una lista de todos sus permisos de usuario.
    ```
    ibmcloud iam user-policies <your_user_name>
    ```
    {: pre}

2. Compruebe si tiene acceso al clúster y al grupo de recursos en el que se encuentra el clúster.
    1. Busque una política que tenga el valor **Resource Group Name** del grupo de recursos del clúster y el valor **Memo** para `Policy applies to the resource group`. Si tiene esta política, tiene acceso al grupo de recursos. Por ejemplo, esta política indica que un usuario tiene acceso al grupo de recursos `test-rg`:
        ```
        Policy ID:   3ec2c069-fc64-4916-af9e-e6f318e2a16c
        Roles:       Viewer
        Resources:
                     Resource Group ID     50c9b81c983e438b8e42b2e8eca04065
                     Resource Group Name   test-rg
                     Memo                  Policy applies to the resource group
        ```
        {: screen}
    2. Busque una política que tenga el valor **Resource Group Name** del grupo de recursos del clúster, para **Service Name** tenga el valor `containers-kubernetes` o ningún valor y tenga el valor **Memo** para `Policy applies to the resource(s) within the resource group`. Si tiene esta política, tiene acceso a los clústeres o a todos los recursos dentro del grupo de recursos. Por ejemplo, esta política indica que un usuario tiene acceso a los clústeres del grupo de recursos `test-rg`:
        ```
        Policy ID:   e0ad889d-56ba-416c-89ae-a03f3cd8eeea
        Roles:       Administrator
        Resources:
                     Resource Group ID     a8a12accd63b437bbd6d58fb6a462ca7
                     Resource Group Name   test-rg
                     Service Name          containers-kubernetes
                     Service Instance
                     Region
                     Resource Type
                     Resource
                     Memo                  Policy applies to the resource(s) within the resource group
        ```
        {: screen}
    3. Si tiene estas dos políticas, salte al primer punto del paso 4. Si no tiene la política del paso 2a, pero sí tiene la política del paso 2b, salte al segundo punto del paso 4. Si no tiene ninguna de estas políticas, continúe en el paso 3.

3. Compruebe si tiene acceso al clúster, pero no como parte del acceso al grupo de recursos en el que se encuentra el clúster.
    1. Busque una política que no tenga ningún valor junto a los campos **Policy ID** y **Roles**. Si tiene esta política, tiene acceso al clúster como parte del acceso a toda la cuenta. Por ejemplo, esta política indica que un usuario tiene acceso a todos los recursos de la cuenta:
        ```
        Policy ID:   8898bdfd-d520-49a7-85f8-c0d382c4934e
        Roles:       Administrator, Manager
        Resources:
                     Service Name
                     Service Instance
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    2. Busque una política que tenga para **Service Name** el valor `containers-kubernetes` y para **Service Instance** el valor del ID del clúster. Para encontrar el ID de un clúster, ejecute `ibmcloud ks cluster-get --cluster <cluster_name>`. Por ejemplo, esta política indica que un usuario tiene acceso a un clúster específico:
        ```
        Policy ID:   140555ce-93ac-4fb2-b15d-6ad726795d90
        Roles:       Administrator
        Resources:
                     Service Name       containers-kubernetes
                     Service Instance   df253b6025d64944ab99ed63bb4567b6
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    3. Si tiene alguna de estas políticas, salte al segundo punto del paso 4. Si no tiene ninguna de estas políticas, salte al tercer punto del paso 4.

4. En función de las políticas de acceso, elija una de las opciones siguientes.
    * Si tiene acceso al clúster y al grupo de recursos en el que se encuentra el clúster:
      1. Elija como destino el grupo de recursos. **Nota**: no se puede trabajar con clústeres en otros grupos de recursos hasta que no se elimine como destino este grupo de recursos.
          ```
          ibmcloud target -g <resource_group>
          ```
          {: pre}

      2. Elija el clúster como destino.
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

    * Si tiene acceso al clúster pero no al grupo de recursos en el que se encuentra el clúster:
      1. No elija como destino un grupo de recursos. Si ya ha elegido como destino un grupo de recursos, elimínelo como destino:
        ```
        ibmcloud target --unset-resource-group
        ```
        {: pre}

      2. Elija el clúster como destino.
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

    * Si no tiene acceso al clúster:
        1. Solicite al propietario de la cuenta que le asigne un [rol de plataforma de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para dicho clúster.
        2. No elija como destino un grupo de recursos. Si ya ha elegido como destino un grupo de recursos, elimínelo como destino:
          ```
          ibmcloud target --unset-resource-group
          ```
          {: pre}
        3. Elija el clúster como destino.
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

<br />


## El acceso al nodo trabajador con SSH falla
{: #cs_ssh_worker}

{: tsSymptoms}
No puede acceder a un nodo trabajador mediante una conexión SSH.

{: tsCauses}
SSH mediante contraseña no está disponible en los nodos trabajadores.

{: tsResolve}
Utilice un [`DaemonSet` de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) para las acciones que tenga que ejecutar en cada nodo, o bien utilice trabajos para las acciones únicas que deba ejecutar.

<br />


## El ID de instancia nativa no es coherente con los registros del nodo trabajador
{: #bm_machine_id}

{: tsSymptoms}
Si utiliza mandatos `ibmcloud ks worker` con el nodo trabajador nativo, aparece un mensaje parecido al siguiente.

```
ID de instancia incoherente con los registros del nodo trabajador
```
{: screen}

{: tsCauses}
El ID de máquina puede ser incoherente con el registro del nodo trabajador de {{site.data.keyword.containerlong_notm}} si la máquina experimenta problemas de hardware. Cuando IBM Cloud infraestructura (SoftLayer) soluciona este problema, es posible que un componente cambie dentro del sistema que el servicio no identifica.

{: tsResolve}
Para que {{site.data.keyword.containerlong_notm}} pueda volver a identificar la máquina, [vuelva a cargar el nodo trabajador nativo](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload). **Nota**: al volver a cargar, también se actualiza la [versión de parche](/docs/containers?topic=containers-changelog) de la máquina.

También puede [suprimir el nodo trabajador nativo](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm). **Nota**: las instancias nativas se facturan mensualmente.

<br />


## No se ha podido modificar o suprimir la infraestructura en un clúster huérfano
{: #orphaned}

{: tsSymptoms}
No puede ejecutar mandatos relacionados con la infraestructura en el clúster, como por ejemplo:
* Adición o eliminación de nodos trabajadores
* Recarga o rearranque de nodos trabajadores
* Redimensionamiento de agrupaciones de nodos trabajadores
* Actualización del clúster

No puede ver los nodos trabajadores del clúster en la cuenta de infraestructura de IBM Cloud (SoftLayer). Sin embargo, puede actualizar y gestionar otros clústeres de la cuenta.

Además, ha verificado que tiene las [credenciales de infraestructura adecuadas](#cs_credentials).

{: tsCauses}
Es posible que el clúster se suministre en una cuenta de infraestructura de IBM Cloud (SoftLayer) que ya no esté enlazada a la cuenta {{site.data.keyword.containerlong_notm}}. El clúster está huérfano. Puesto que los recursos se encuentran en otra cuenta, no tiene las credenciales de la infraestructura para modificar los recursos.

Tenga en cuenta el caso de ejemplo siguiente para entender cómo se pueden quedar huérfanas los clústeres.
1.  Tiene una cuenta de pago según uso de {{site.data.keyword.Bluemix_notm}}.
2.  Crea un clúster llamado `Cluster1`. Los nodos trabajadores y otros recursos de la infraestructura se suministran en la cuenta de infraestructura que viene con su cuenta de pago según uso.
3.  Más adelante, averigua que su equipo utiliza una cuenta de infraestructura de IBM Cloud (SoftLayer) heredada o compartida. Utiliza el mandato `ibmcloud ks credential-set` para cambiar las credenciales de la infraestructura de IBM Cloud (SoftLayer) para utilizar la cuenta del equipo.
4.  Crea otro clúster llamado `Cluster2`. Los nodos trabajadores y otros recursos de la infraestructura se suministran en la cuenta de infraestructura del equipo.
5.  Observa que `Cluster1` necesita una actualización del nodo trabajador, o una recarga del nodo trabajador o simplemente desea limpiarlo antes de suprimirlo. Sin embargo, como `Cluster1` se ha suministrado en otra cuenta de infraestructura, no puede modificar sus recursos de la infraestructura. `Cluster1` es huérfano.
6.  Sigue los pasos de resolución de la sección siguiente, pero no restablece las credenciales de la infraestructura en la cuenta de equipo. Puede suprimir `Cluster1`, pero ahora `Cluster2` es huérfano.
7.  Vuelve a cambiar las credenciales de la infraestructura por la cuenta de equipo que creó `Cluster2`. Ahora, ya no tiene un clúster huérfano.

<br>

{: tsResolve}
1.  Compruebe qué cuenta de infraestructura utiliza la región en la que está actualmente el clúster para suministrar clústeres.
    1.  Inicie sesión en la [consola de clústeres de {{site.data.keyword.containerlong_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/kubernetes/clusters).
    2.  En la tabla, seleccione el clúster.
    3.  En el separador **Visión general**, compruebe si hay un campo **Usuario de infraestructura**. Este campo le ayuda a determinar si la cuenta de {{site.data.keyword.containerlong_notm}} utiliza una cuenta de infraestructura distinta de la predeterminada.
        * Si no ve el campo **Usuario de infraestructura**, tiene una cuenta de Pago según uso enlazada que utiliza las mismas credenciales para las cuentas de infraestructura y de plataforma. Es posible que el clúster que no se puede modificar se suministre en una cuenta de infraestructura distinta.
        * Si ve un campo **Usuario de infraestructura**, significa que utiliza una cuenta de infraestructura distinta de la que se ha suministrado con la cuenta de Pago según uso. Estas credenciales distintas se aplican a todos los clústeres de la región. Es posible que el clúster que no se puede modificar se suministre en su cuenta de pago según uso o en una cuenta de infraestructura distinta.
2.  Compruebe qué cuenta de infraestructura se ha utilizado para suministrar el clúster.
    1.  En el separador **Nodos trabajadores**, seleccione un nodo trabajador y anote su **ID**.
    2.  Abra el menú ![Icono de menú](../icons/icon_hamburger.svg "Icono de menú") y pulse **Infraestructura clásica**.
    3.  En el panel de navegación de infraestructura, pulse **Dispositivos > Lista de dispositivos**.
    4.  Busque el ID de nodo trabajador que ha anotado anteriormente.
    5.  Si no encuentra el ID del nodo trabajador, significa que el nodo trabajador no se suministra en esta cuenta de infraestructura. Cambie a otra cuenta de infraestructura y vuélvalo a intentar.
3.  Utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) `ibmcloud ks credential-set` para cambiar las credenciales de infraestructura por la cuenta en la que se han suministrado los nodos trabajadores del clúster, que ha encontrado en el paso anterior.
    Si ya no tiene acceso y no puede obtener las credenciales de la infraestructura, debe abrir un caso de soporte de {{site.data.keyword.Bluemix_notm}} para eliminar el clúster huérfano.
    {: note}
4.  [Suprima el clúster](/docs/containers?topic=containers-remove).
5.  Si lo desea, restablezca las credenciales de la infraestructura de la cuenta anterior. Tenga en cuenta que si ha creado clústeres con una cuenta de infraestructura distinta de la cuenta a la que conmuta, es posible que deje huérfanos dichos clústeres.
    * Para establecer las credenciales en otra cuenta de infraestructura, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) `ibmcloud ks credential-set`.
    * Para utilizar las credenciales predeterminadas que se suministran con la cuenta de pago según uso de {{site.data.keyword.Bluemix_notm}}, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset) `ibmcloud ks credential-unset --region <region>`.

<br />


## Tiempo de espera excedido de los mandatos `kubectl`
{: #exec_logs_fail}

{: tsSymptoms}
Si ejecuta mandatos como `kubectl exec`, `kubectl attach`, `kubectl proxy`, `kubectl port-forward` o `kubectl logs`, ve el siguiente mensaje.

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
La conexión OpenVPN entre el nodo maestro y los nodos trabajadores no está funcionando correctamente.

{: tsResolve}
1. Si tiene varias VLAN para un clúster, varias subredes en la misma VLAN o un clúster multizona, debe habilitar la [función de direccionador virtual (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para la cuenta de infraestructura de IBM Cloud (SoftLayer) para que los nodos trabajadores puedan comunicarse entre sí en la red privada. Para habilitar VRF, [póngase en contacto con el representante de su cuenta de la infraestructura de IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si no puede o no desea habilitar VRF, habilite la [distribución de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para llevar a cabo esta acción, necesita el [permiso de la infraestructura](/docs/containers?topic=containers-users#infra_access) **Red > Gestionar distribución de VLAN de red** o bien puede solicitar al propietario de la cuenta que lo habilite. Para comprobar si la distribución de VLAN ya está habilitada, utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.
2. Reinicie el pod de cliente de OpenVPN.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. Si sigue viendo el mismo mensaje de error, entonces el nodo trabajador en el que está el pod de VPN podría estar en un estado no saludable. Para reiniciar el pod de VPN y volverlo a planificar en un nodo trabajador diferente, [acordone, drene y rearranque el nodo trabajador](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) en el que está el pod de VPN.

<br />


## Al enlazar un servicio a un clúster se produce un error de nombre
{: #cs_duplicate_services}

{: tsSymptoms}
Cuando ejecuta `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`, aparece el siguiente mensaje.

```
Se han encontrado varios servicios con el mismo nombre.
Ejecute 'ibmcloud service list' para ver las instancias disponibles del servicio Bluemix...
```
{: screen}

{: tsCauses}
Varias instancias de servicio pueden tener el mismo nombre en distintas regiones.

{: tsResolve}
Utilice el GUID de servicio en lugar del nombre de instancia de servicio en el mandato `ibmcloud ks cluster-service-bind`.

1. [Inicie una sesión en la región de {{site.data.keyword.Bluemix_notm}} que incluye la instancia de servicio que desea enlazar.](/docs/containers?topic=containers-regions-and-zones#bluemix_regions)

2. Obtenga el GUID de la instancia de servicio.
  ```
  ibmcloud service show <service_instance_name> --guid
  ```
  {: pre}

  Salida:
  ```
  Invocando 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. Vuelva a enlazar el servicio al clúster.
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_GUID>
  ```
  {: pre}

<br />


## Al enlazar un servicio a un clúster se produce un error de no encontrado
{: #cs_not_found_services}

{: tsSymptoms}
Cuando ejecuta `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`, aparece el siguiente mensaje.

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'ibmcloud service list'. (E0023)
```
{: screen}

{: tsCauses}
Para enlazar servicios a un clúster, debe tener el rol de usuario desarrollador de Cloud Foundry para el espacio de nombres donde se ha suministrado la instancia de servicio. Además, debe tener acceso de editor de {{site.data.keyword.Bluemix_notm}} IAM a {{site.data.keyword.containerlong}}. Para acceder a la instancia de servicio, debe haber iniciado una sesión en el espacio en el que se ha suministrado la instancia de servicio.

{: tsResolve}

**Como usuario:**

1. Inicie una sesión en {{site.data.keyword.Bluemix_notm}}.
   ```
   ibmcloud login
   ```
   {: pre}

2. Defina como destino la organización y el espacio donde se haya suministrado la instancia de servicio.
   ```
   ibmcloud target -o <org> -s <space>
   ```
   {: pre}

3. Liste las instancias de servicio para verificar que está en el espacio adecuado.
   ```
   ibmcloud service list
   ```
   {: pre}

4. Intente enlazar de nuevo el servicio. Si recibe el mismo error, póngase en contacto con el administrador de la cuenta y verifique que tiene los permisos suficientes para enlazar a los servicios (consulte los siguientes pasos de administración de la cuenta).

**Como administrador de la cuenta:**

1. Verifique que el usuario que experimenta este problema tiene [permisos de editor para {{site.data.keyword.containerlong}}](/docs/iam?topic=iam-iammanidaccser#edit_existing).

2. Verifique que el usuario con este problema tiene el [rol de desarrollador de Cloud Foundry para el espacio](/docs/iam?topic=iam-mngcf#update_cf_access) en el que se ha suministrado el servicio.

3. Si los permisos son los correctos, intente asignar un permiso diferente y, a continuación, vuelva a asignar los permisos necesarios.

4. Espere unos minutos y, a continuación, deje que el usuario intente enlazar el servicio de nuevo.

5. Si esto no resuelve el problema, entonces los permisos de {{site.data.keyword.Bluemix_notm}} IAM no están sincronizados y no podrá resolver el problema usted mismo. [Póngase en contacto con el equipo de soporte de IBM](/docs/get-support?topic=get-support-getting-customer-support) abriendo un caso de soporte. Asegúrese de proporcionar el ID de clúster, el ID de usuario y el ID de instancia de servicio.
   1. Recupere el ID de clúster.
      ```
      ibmcloud ks clusters
      ```
      {: pre}

   2. Recupere el ID de la instancia de servicio.
      ```
      ibmcloud service show <service_name> --guid
      ```
      {: pre}


<br />


## Al enlazar un servicio a un clúster se produce el error de que el servicio no admite las claves de servicio
{: #cs_service_keys}

{: tsSymptoms}
Cuando ejecuta `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`, aparece el siguiente mensaje.

```
Este servicio no admite la creación de claves
```
{: screen}

{: tsCauses}
Algunos servicios de {{site.data.keyword.Bluemix_notm}}, como {{site.data.keyword.keymanagementservicelong}} no admiten la creación de credenciales de servicio, también conocidas como claves de servicio. Si no se admiten las claves de servicio, el servicio no se puede enlazar a un clúster. Para consultar una lista de servicios que admiten la creación de claves de servicio, consulte [Habilitación de apps externas para utilizar servicios de {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).

{: tsResolve}
Para integrar servicios que no admiten claves de servicio, compruebe si el servicio proporciona una API que puede utilizar para acceder al servicio directamente desde la app. Por ejemplo, si desea utilizar {{site.data.keyword.keymanagementservicelong}}, consulte la [Referencia de API ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/kms?language=curl).

<br />


## Después de que un nodo trabajador se actualice o recargue, aparecen pods y nodos duplicados
{: #cs_duplicate_nodes}

{: tsSymptoms}
Cuando ejecuta `kubectl get nodes`, ve nodos trabajadores duplicados con el estado **`NotReady`**. Los nodos trabajadores con **`NotReady`** tienen direcciones IP públicas, mientras que los nodos trabajadores con **`Ready`** tienen direcciones IP privadas.

{: tsCauses}
Clústeres antiguos listaban nodos trabajadores según la dirección IP pública del clúster. Ahora los nodos trabajadores aparecen listados por la dirección IP privada del clúster. Cuando se vuelve a cargar o se actualiza un nodo, la dirección IP se modifica, pero la referencia a la dirección IP pública permanece.

{: tsResolve}
El servicio no se interrumpe debido a estos duplicados, pero puede eliminar las referencias a los nodos trabajadores antiguos del servidor de API.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## El acceso a un pod en un nodo trabajador nuevo falla con un tiempo de espera excedido
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
Ha suprimido un nodo trabajador del clúster y luego ha añadido un nodo trabajador. Cuando se despliega un pod o un servicio de Kubernetes, el recurso no puede acceder al nodo trabajador recién creado y la conexión supera el tiempo de espera.

{: tsCauses}
Si suprime un nodo trabajador del clúster y luego añade un nodo trabajador, es posible que el nuevo nodo trabajador se asigne a la dirección IP privada del nodo trabajador suprimido. Calico utiliza esta dirección IP privada como código y sigue intentando acceder al nodo suprimido.

{: tsResolve}
Actualice manualmente la referencia a la dirección IP privada de modo que apunte al nodo correcto.

1.  Confirme que tiene dos nodos trabajadores con la misma dirección **IP privada**. Anote la **IP privada** y el **ID** del nodo trabajador suprimido.

  ```
  ibmcloud ks workers --cluster <cluster_name_or_id>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Zone   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       normal    Ready    dal10      1.13.6
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       deleted    -       dal10      1.13.6
  ```
  {: screen}

2.  Instale la [CLI de Calico](/docs/containers?topic=containers-network_policies#adding_network_policies).
3.  Obtenga una lista de los nodos trabajadores disponibles en Calico. Sustituya <path_to_file> por la vía de acceso local al archivo de configuración de Calico.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Suprima el nodo trabajador duplicado en Calico. Sustituya NODE_ID por el ID del nodo trabajador.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  Rearranque el nodo trabajador que no se ha suprimido.

  ```
  ibmcloud ks worker-reboot --cluster <cluster_name_or_id> --worker <worker_id>
  ```
  {: pre}


El nodo suprimido ya no aparece en Calico.

<br />




## Las pods no se pueden desplegar debido a una política de seguridad de pod
{: #cs_psp}

{: tsSymptoms}
Después de crear un pod o de ejecutar `kubectl get events` para comprobar un despliegue de pod, ve un mensaje de error similar al siguiente.

```
no se puede validar con ninguna política de seguridad de pod
```
{: screen}

{: tsCauses}
[El controlador de admisiones `PodSecurityPolicy`](/docs/containers?topic=containers-psp) comprueba la autorización del usuario o de la cuenta de servicio, como un despliegue o tiller de Helm, que ha intentado crear el pod. Si no hay ninguna política de seguridad de pod que dé soporte a la cuenta de usuario o al servicio, el controlador de admisiones `PodSecurityPolicy` impide que se creen los pods.

Si ha suprimido uno de los recursos de la política de seguridad de pod para [la gestión de clústeres de {{site.data.keyword.IBM_notm}}](/docs/containers?topic=containers-psp#ibm_psp), es posible que experimente problemas similares.

{: tsResolve}
Asegúrese de que el usuario o la cuenta de servicio tenga autorización de la política de seguridad de pod. Es posible que tenga que [modificar una política existente](/docs/containers?topic=containers-psp#customize_psp).

Si ha suprimido un recurso de gestión de clústeres de {{site.data.keyword.IBM_notm}}, renueve el nodo maestro de Kubernetes para restaurarlo.

1.  [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2.  Renueve el nodo maestro de Kubernetes para restaurarlo.

    ```
    ibmcloud ks apiserver-refresh
    ```
    {: pre}


<br />




## El clúster permanece en un estado pendiente
{: #cs_cluster_pending}

{: tsSymptoms}
Al desplegar el clúster, se queda en estado pendiente y no se inicia.

{: tsCauses}
Si acaba de crear el clúster, es posible que los nodos trabajadores aún se estén configurando. Si ya esperado algo de tiempo, es posible que tenga una VLAN no válida.

{: tsResolve}

Puede probar una de las siguientes soluciones:
  - Compruebe el estado del clúster ejecutando `ibmcloud ks clusters`. Luego, compruebe que los nodos trabajadores están desplegados ejecutando `ibmcloud ks workers --cluster <cluster_name>`.
  - Compruebe si la VLAN es válida. Para ser válida, una VLAN debe estar asociada con infraestructura que pueda alojar un trabajador con almacenamiento en disco local. Puede [ver una lista de las VLAN](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans) ejecutando `ibmcloud ks vlans --zone <zone>`; si la VLAN no aparece en la lista, significa que no es válida. Elija una VLAN diferente.

<br />


## Un error al crear el clúster impide que se extraigan imágenes del registro
{: #ts_image_pull_create}

{: tsSymptoms}
Al crear el clúster, ha recibido un mensaje parecido al siguiente:


```
El clúster o puede extraer imágenes de dominios 'icr.io' de IBM Cloud Container Registry porque no se ha podido crear una política de acceso de IAM. Asegúrese de que tiene el rol de plataforma de Administrador de IAM sobre IBM Cloud Container Registry. A continuación, cree un secreto de extracción de imágenes con credenciales de IAM en el registro con el mandato 'ibmcloud ks cluster-pull-secret-apply'.
```
{: screen}

{: tsCauses}
Durante la creación del clúster, se crea un ID de servicio para el clúster y se le asigna la política de acceso al servicio de **Lector** sobre {{site.data.keyword.registrylong_notm}}. A continuación, se genera una clave de API para este ID de servicio y se guarda en un [secreto de extracción de imágenes](/docs/containers?topic=containers-images#cluster_registry_auth) para autorizar al clúster a extraer imágenes de {{site.data.keyword.registrylong_notm}}.

Para asignar correctamente la política de acceso al servicio de **Lector** al ID de servicio durante la creación del clúster, debe tener la política de acceso de plataforma de **Administrador** sobre {{site.data.keyword.registrylong_notm}}.

{: tsResolve}

Pasos:
1.  Asegúrese de que el propietario de la cuenta le asigne el rol de **Administrador** sobre {{site.data.keyword.registrylong_notm}}.
    ```
    ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
    ```
    {: pre}
2.  [Utilice el mandato `ibmcloud ks cluster-pull-secret-apply`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply) para volver a crear un secreto de extracción de imágenes con las credenciales de registro adecuadas.

<br />


## No se ha podido extraer una imagen del registro con `ImagePullBackOff` o errores de autorización
{: #ts_image_pull}

{: tsSymptoms}

Cuando despliega una carga de trabajo que extrae una imagen de {{site.data.keyword.registrylong_notm}}, sus pods fallan con el estado **`ImagePullBackOff`**.

```
kubectl get pods
```
{: pre}

```
NAME         READY     STATUS             RESTARTS   AGE
<pod_name>   0/1       ImagePullBackOff   0          2m
```
{: screen}

Cuando se describe el pod, se ven errores de autenticación similares a los siguientes.

```
kubectl describe pod <pod_name>
```
{: pre}

```
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

```
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

{: tsCauses}
El clúster utiliza una clave de API o una señal que se almacena en un [secreto
de extracción de imágenes](/docs/containers?topic=containers-images#cluster_registry_auth) para autorizar al clúster a extraer las imágenes de {{site.data.keyword.registrylong_notm}}. De forma predeterminada, los nuevos clústeres tienen secretos de extracción de imágenes que utilizan claves de API para que el clúster pueda extraer imágenes de cualquier registro regional icr.io para contenedores que desplegados en el espacio de nombres default de Kubernetes. Si el clúster tiene un secreto de extracción de imágenes que utiliza una señal, el acceso predeterminado a {{site.data.keyword.registrylong_notm}} se limita aún más a determinados registros regionales que utilizan los dominios de <region>.registry.bluemix.net en desuso.

{: tsResolve}

1.  Verifique que utiliza el nombre y el código correctos de la imagen en el archivo YAML de despliegue.
    ```
    ibmcloud cr images
    ```
    {: pre}
2.  Obtenga el archivo de configuración de pod de un pod que falla y busque la sección `imagePullSecrets`.
    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ...
    imagePullSecrets:
    - name: bluemix-default-secret
    - name: bluemix-default-secret-regional
    - name: bluemix-default-secret-international
    - name: default-us-icr-io
    - name: default-uk-icr-io
    - name: default-de-icr-io
    - name: default-au-icr-io
    - name: default-jp-icr-io
    - name: default-icr-io
    ...
    ```
    {: screen}
3.  Si no se muestra ningún secreto de extracción de imágenes en la lista, configure el secreto de extracción de imágenes en el espacio de nombres.
    1.  [Copie los secretos de extracción de imágenes del espacio de nombres de Kubernetes `default` en el espacio de nombres en el que desea desplegar la carga de trabajo](/docs/containers?topic=containers-images#copy_imagePullSecret).
    2.  [Añada el secreto de extracción de imágenes a la cuenta de servicio para este espacio de nombres de Kubernetes](/docs/containers?topic=containers-images#store_imagePullSecret) para que todos los pods del espacio de nombres puedan utilizar las credenciales secretas de obtención de imágenes.
4.  Si aparecen en la lista secretos de extracción de imágenes, determine el tipo de credenciales que utiliza para acceder al registro de contenedor.
    *   **En desuso**: Si el nombre del secreto contiene `bluemix`, significa que utiliza una señal de registro para autenticarse con nombres de dominio `registry.<region>.bluemix.net` en desuso. Continúe en el apartado [Resolución de problemas de secretos de extracción de imágenes que utilizan señales](#ts_image_pull_token).
    *   Si el nombre del secreto contiene `icr`, significa que utiliza una clave de API para autenticarse con los nombres de dominio `icr.io`. Continúe en el apartado [Resolución de problemas de secretos de extracción de imágenes que utilizan claves de API](#ts_image_pull_apikey).
    *   Si tiene ambos tipos de secretos, utilice los dos métodos de autenticación. Utilice los nombres de dominio `icr.io` en los archivos YAML de despliegue correspondientes a la imagen de contenedor. Continúe en el apartado [Resolución de problemas de secretos de extracción de imágenes que utilizan claves de API](#ts_image_pull_apikey).

<br>
<br>

**Resolución de problemas de secretos de extracción de imágenes que utilizan claves de API**</br>
{: #ts_image_pull_apikey}

Si la configuración de pod tiene un secreto de extracción de imágenes que utiliza una clave de API, compruebe que las credenciales de clave de API se han configurado correctamente.
{: shortdesc}

En los pasos siguientes se presupone que la clave de API almacena las credenciales de un ID de servicio. Si configura el secreto de extracción de imágenes de modo que utilice una clave de API de un usuario individual, debe verificar los permisos y las credenciales de {{site.data.keyword.Bluemix_notm}} IAM de dicho usuario.
{: note}

1.  Localice el ID de servicio que utiliza la clave de API para el secreto de extracción de imágenes en la **Descripción**. El ID de servicio que se crea con el clúster indica `ID for <cluster_name>` y se utiliza en el espacio de nombres de Kubernetes `default`. Si ha creado otro ID de servicio, como por ejemplo para acceder a otro espacio de nombres de Kubernetes o para modificar los permisos de {{site.data.keyword.Bluemix_notm}} IAM, ha personalizado la descripción.
    ```
    ibmcloud iam service-ids
    ```
    {: pre}

    Salida de ejemplo:
    ```
    UUID                Name               Created At              Last Updated            Description                                                                                                                                                                                         Locked     
    ServiceId-aa11...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   ID for <cluster_name>                                                                                                                                         false   
    ServiceId-bb22...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>                                                                                                                                         false    
    ```
    {: screen}
2.  Verifique que el ID de servicio tiene asignado como mínimo una [Política de rol de acceso al servicio para {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-user#create) de **Lector** de {{site.data.keyword.Bluemix_notm}} IAM. Si el ID de servicio no tiene el rol de servicio de **Lector**, [edite las políticas de IAM](/docs/iam?topic=iam-serviceidpolicy#access_edit). Si las políticas son correctas, continúe con el paso siguiente para ver si las credenciales son válidas.
    ```
    ibmcloud iam service-policies <service_ID_name>
    ```
    {: pre}

    Salida de ejemplo:
    ```              
    Policy ID:   a111a111-b22b-333c-d4dd-e555555555e5   
    Roles:       Reader   
    Resources:                            
                  Service Name       container-registry      
                  Service Instance         
                  Región                  
                  Resource Type      namespace      
                  Resource           <registry_namespace>  
    ```
    {: screen}  
3.  Compruebe si las credenciales del secreto de extracción de imágenes son válidas.
    1.  Obtenga la configuración del secreto de extracción de imágenes. Si el pod no está en el espacio de nombres `default`, incluya el distintivo `-n`.
        ```
        kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
        ```
        {: pre}
    2.  En la salida, copie el valor codificado en base64 del campo `.dockercfg`.
        ```
        apiVersion: v1
        kind: Secret
        data:
          .dockercfg: eyJyZWdp...==
        ...
        ```
        {: screen}
    3.  Decodifique la serie base64. Por ejemplo, en OS X puede ejecutar el mandato siguiente.
        ```
        echo -n "<base64_string>" | base64 --decode
        ```
        {: pre}

        Salida de ejemplo:
        ```
        {"auths":{"<region>.icr.io":{"username":"iamapikey","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
        ```
        {: screen}
    4.  Compare el nombre de dominio de registro regional del secreto de extracción de imágenes con el nombre de dominio que ha especificado en la imagen del contenedor. De forma predeterminada, los clústeres nuevos tienen secretos de extracción de imágenes para cada nombre de dominio de registro regional para los contenedores que se ejecutan en el espacio de nombres `default` de Kubernetes. Sin embargo, si ha modificado los valores predeterminados o si está utilizando un espacio de nombres de Kubernetes distinto, es posible que no tenga un secreto de extracción de imágenes para el registro regional. [Copie un secreto de extracción de imágenes](/docs/containers?topic=containers-images#copy_imagePullSecret) para el nombre de dominio de registro regional.
    5.  Inicie una sesión en el registro desde la máquina local utilizando los valores de `username` y `password` del secreto de extracción de imágenes. Si no puede iniciar la sesión, es posible que tenga que arreglar el ID de servicio.
        ```
        docker login -u iamapikey -p <password_string> <region>.icr.io
        ```
        {: pre}
        1.  Vuelva a crear el ID de servicio de clúster, las políticas de {{site.data.keyword.Bluemix_notm}} IAM, la clave de API y los secretos de extracción de imágenes para contenedores que se ejecuten en el espacio de nombres `default` de Kubernetes.
            ```
            ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
            ```
            {: pre}
        2.  Vuelva a crear el despliegue en el espacio de nombres de Kubernetes `default`. Si sigue viendo un mensaje de error de autorización, repita los pasos 1-5 con los nuevos secretos de extracción de imágenes. Si aún no puede iniciar la sesión, [póngase en contacto con el equipo de IBM en Slack o abra un caso de soporte de {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
    6.  Si inicia la sesión correctamente, extraiga una imagen localmente. Si el mandato falla con un error de `acceso denegado`,
la cuenta de registro se encuentra en una cuenta de {{site.data.keyword.Bluemix_notm}} distinta de la cuenta en el que se encuentra el clúster. [Cree un secreto de extracción de imágenes para acceder a imágenes de otra cuenta](/docs/containers?topic=containers-images#other_registry_accounts). Si puede extraer una imagen a su máquina local, significa que la clave de API tiene los derechos adecuados, pero que la configuración de API en el clúster no es correcta. No puede resolver este problema. [Póngase en contacto con el equipo de IBM en Slack o abra un caso de soporte de {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
        ```
        docker pull <region>icr.io/<namespace>/<image>:<tag>
        ```
        {: pre}

<br>
<br>

**En desuso: Resolución de problemas de secretos de extracción de imágenes que utilizan señales**</br>
{: #ts_image_pull_token}

Si la configuración de pod tiene un secreto de extracción de imágenes que utiliza una señal, compruebe que las credenciales de la señal sean válidas.
{: shortdesc}

Este método de utilizar una señal para autorizar el acceso del clúster a {{site.data.keyword.registrylong_notm}} recibe soporte para los nombres de dominio `registry.bluemix.net`, pero está en desuso. En su lugar, [utilice el método de clave de API](/docs/containers?topic=containers-images#cluster_registry_auth) para autorizar el acceso del clúster a los nuevos nombres de dominio de registro de `icr.io`.
{: deprecated}

1.  Obtenga la configuración del secreto de extracción de imágenes. Si el pod no está en el espacio de nombres `default`, incluya el distintivo `-n`.
    ```
    kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
    ```
    {: pre}
2.  En la salida, copie el valor codificado en base64 del campo `.dockercfg`.
    ```
    apiVersion: v1
    kind: Secret
    data:
      .dockercfg: eyJyZWdp...==
    ...
    ```
    {: screen}
3.  Decodifique la serie base64. Por ejemplo, en OS X puede ejecutar el mandato siguiente.
    ```
    echo -n "<base64_string>" | base64 --decode
    ```
    {: pre}

    Salida de ejemplo:
    ```
    {"auths":{"registry.<region>.bluemix.net":{"username":"token","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
    ```
    {: screen}
4.  Compare el nombre de dominio de registro con el nombre de dominio que ha especificado en la imagen del contenedor. Por ejemplo, si el secreto de extracción de imágenes autoriza el acceso al dominio `registry.ng.bluemix.net` pero
ha especificado una imagen que se almacena en `registry.eu-de.bluemix.net`, debe [crear una señal que utilizará en un secreto de extracción de imágenes](/docs/containers?topic=containers-images#token_other_regions_accounts) para `registry.eu-de.bluemix.net`.
5.  Inicie una sesión en el registro desde la máquina local utilizando los valores de `username` y `password` del secreto de extracción de imágenes. Si no puede iniciar la sesión, la señal tiene un problema que no puede resolver. [Póngase en contacto con el equipo de IBM en Slack o abra un caso de soporte de {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
    ```
    docker login -u token -p <password_string> registry.<region>.bluemix.net
    ```
    {: pre}
6.  Si inicia la sesión correctamente, extraiga una imagen localmente. Si el mandato falla con un error de `acceso denegado`,
la cuenta de registro se encuentra en una cuenta de {{site.data.keyword.Bluemix_notm}} distinta de la cuenta en el que se encuentra el clúster. [Cree un secreto de extracción de imágenes para acceder a imágenes de otra cuenta](/docs/containers?topic=containers-images#token_other_regions_accounts). Si el mandato se ejecuta correctamente, [póngase en contacto con el equipo de IBM en Slack o abra un caso de soporte de {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
    ```
    docker pull registry.<region>.bluemix.net/<namespace>/<image>:<tag>
    ```
    {: pre}

<br />


## Los pods permanecen en estado pendiente
{: #cs_pods_pending}

{: tsSymptoms}
Cuando se ejecuta `kubectl get pods`, puede ver que los pods permanecen en el estado **Pendiente**.

{: tsCauses}
Si acaba de crear el clúster de Kubernetes, es posible que los nodos trabajadores aún se estén configurando.

Si este clúster ya existe:
*  Quizá no tiene suficiente capacidad en el clúster para desplegar el pod.
*  Es posible que el pod haya superado una solicitud o límite de recursos.

{: tsResolve}
Esta tarea requiere el [rol de plataforma de **Administrador**](/docs/containers?topic=containers-users#platform) de {{site.data.keyword.Bluemix_notm}} IAM para el clúster y el [rol de servicio de **Gestor**](/docs/containers?topic=containers-users#platform) sobre todos los espacios de nombres.

Si acaba de crear el clúster de Kubernetes, ejecute el siguiente mandato y espere que se hayan inicializado los nodos trabajadores.

```
kubectl get nodes
```
{: pre}

Si este clúster ya existe, compruebe la capacidad del clúster.

1.  Establezca el proxy con el número de puerto predeterminado.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Abra el panel de control de Kubernetes.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  Compruebe si tiene suficiente capacidad en el clúster para desplegar el pod.

4.  Si no tiene suficiente capacidad en el clúster, cambie el tamaño de la agrupación de nodos trabajadores para añadir más nodos.

    1.  Revise los tamaños y los tipos de máquina actuales de las agrupaciones de nodos trabajadores para decidir cuál va a redimensionar.

        ```
        ibmcloud ks worker-pools
        ```
        {: pre}

    2.  Cambie el tamaño de las agrupaciones de nodos trabajadores para añadir más nodos a cada zona que abarque la agrupación.

        ```
        ibmcloud ks worker-pool-resize --worker-pool <worker_pool> --cluster <cluster_name_or_ID> --size-per-zone <workers_per_zone>
        ```
        {: pre}

5.  Opcional: compruebe las solicitudes de recursos de pod.

    1.  Confirme que los valores de `resources.requests` no son mayores que la capacidad del nodo trabajador. Por ejemplo, si la solicitud de pod es `cpu: 4000m`, o 4 núcleos, pero el tamaño de nodo trabajador es de sólo 2 núcleos, el pod no se puede desplegar.

        ```
        kubectl get pod <pod_name> -o yaml
        ```
        {: pre}

    2.  Si la solicitud supera la capacidad disponible, [añada una nueva agrupación de trabajadores](/docs/containers?topic=containers-add_workers#add_pool) con nodos trabajadores que puedan cumplir la solicitud.

6.  Si los pods continúan en estado **pending** después de que se despliegue por completo el nodo trabajador, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) para solucionar el estado pendiente del pod.

<br />


## Los contenedores no se inician
{: #containers_do_not_start}

{: tsSymptoms}
Los pods se despliegan correctamente en los clústeres, pero los contenedores no inician.

{: tsCauses}
Es posible que los contenedores no se inicien cuando se alcanza la cuota de registro.

{: tsResolve}
[Libere almacenamiento en {{site.data.keyword.registryshort_notm}}.](/docs/services/Registry?topic=registry-registry_quota#registry_quota_freeup)

<br />


## Los pods no se pueden reiniciar repetidamente o se eliminan inesperadamente
{: #pods_fail}

{: tsSymptoms}
Su pod estaba en buen estado pero inesperadamente se ha eliminado o ha quedado atascado en un bucle de reinicio.

{: tsCauses}
Es posible que sus contenedores superen sus límites de recursos, o que sus pods hayan sido sustituidos por pods de prioridad más alta.

{: tsResolve}
Para ver si un contenedor se ha eliminado debido a un límite de recursos:
<ol><li>Obtenga el nombre del pod. Si ha utilizado una etiqueta, puede incluirla para filtrar los resultados.<pre class="pre"><code>kubectl get pods --selector='app=wasliberty'</code></pre></li>
<li>Describa el pod y busque el **Recuento de reinicios**.<pre class="pre"><code>kubectl describe pod</code></pre></li>
<li>Si el pod se ha reiniciado muchas veces durante un breve periodo de tiempo, capte su estado. <pre class="pre"><code>kubectl get pod -o go-template={{range.status.containerStatuses}}{{"Container Name: "}}{{.name}}{{"\r\nLastState: "}}{{.lastState}}{{end}}</code></pre></li>
<li>Revise el motivo. Por ejemplo, `OOM Killed` significa "sin memoria," lo que indica que el contenedor ha caído debido a un límite de recursos.</li>
<li>Añada capacidad al clúster para disponer de suficientes recursos.</li></ol>

<br>

Para ver si su pod ha sido sustituido por pods de prioridad más alta:
1.  Obtenga el nombre del pod.

    ```
    kubectl get pods
    ```
    {: pre}

2.  Revise el YAML de su pod.

    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

3.  Compruebe el campo `priorityClassName`.

    1.  Si no hay ningún valor del campo `priorityClassName`, significa que su pod tiene la clase de prioridad `globalDefault`. Si el administrador del clúster no ha establecido una clase de prioridad `globalDefault`, el valor predeterminado es cero (0) o la prioridad más baja. Cualquier pod con una clase de prioridad más alta tiene preferencia sobre su pod o puede eliminarlo.

    2.  Si hay un valor para el campo `priorityClassName`, obtenga la clase de prioridad.

        ```
        kubectl get priorityclass <priority_class_name> -o yaml
        ```
        {: pre}

    3.  Observe el campo `value` para comprobar la prioridad del pod.

4.  Obtenga una lista de las clases de prioridad del clúster.

    ```
    kubectl get priorityclasses
    ```
    {: pre}

5.  Para cada clase de prioridad, obtenga el archivo YAML y observe el campo `value`.

    ```
    kubectl get priorityclass <priority_class_name> -o yaml
    ```
    {: pre}

6.  Compare el valor de la clase de prioridad de su pod con los otros valores de clase de prioridad para ver si la prioridad es más alta o más baja.

7.  Repita los pasos del 1 al 3 para los otros pods del clúster, para comprobar qué clase de prioridad están utilizando. Si la clase de prioridad de estos otros pods es más alta que la de su pod, su pod no se suministra a menos que haya recursos suficientes para su pod y para cada pod con prioridad más alta.

8.  Póngase en contacto con el administrador del clúster para añadir más capacidad al clúster y confirmar que se han asignado las clases de prioridad adecuadas.

<br />


## No se puede instalar un diagrama de Helm con valores de configuración actualizados
{: #cs_helm_install}

{: tsSymptoms}
Cuando intenta instalar un diagrama de Helm actualizado ejecutando `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>`, obtiene el mensaje de error `Error: failed to download "ibm/<chart_name>"`.

{: tsCauses}
El URL del repositorio de {{site.data.keyword.Bluemix_notm}} en la instancia de Helm podría ser incorrecto.

{: tsResolve}
Para resolver el problema del diagrama de Helm:

1. Obtenga una lista de los repositorios disponibles actualmente en la instancia de Helm.

    ```
    helm repo list
    ```
    {: pre}

2. En la salida, verifique que el URL del repositorio de {{site.data.keyword.Bluemix_notm}}, `ibm`, es `https://icr.io/helm/iks-charts`.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://icr.io/helm/iks-charts
    ```
    {: screen}

    * Si el URL es incorrecto:

        1. Elimine el repositorio de {{site.data.keyword.Bluemix_notm}}.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. Añada el repositorio de {{site.data.keyword.Bluemix_notm}} de nuevo.

            ```
            helm repo add iks-charts https://icr.io/helm/iks-charts
            ```
            {: pre}

    * Si el URL es correcto, obtenga las últimas actualizaciones del repositorio.

        ```
        helm repo update
        ```
        {: pre}

3. Instale el diagrama de Helm con las actualizaciones.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

<br />


## No puedo instalar el tiller de Helm o desplegar contenedores desde imágenes públicas de mi clúster
{: #cs_tiller_install}

{: tsSymptoms}

Cuando intenta instalar el tiller de Helm o desea desplegar imágenes de registros públicos, como DockerHub, la instalación falla con un error similar al siguiente:

```
Failed to pull image "gcr.io/kubernetes-helm/tiller:v2.12.0": rpc error: code = Unknown desc = failed to resolve image "gcr.io/kubernetes-helm/tiller:v2.12.0": no available registry endpoint:
```
{: screen}

{: tsCauses}
Es posible que haya configurado un cortafuegos personalizado, que haya especificado políticas de Calico personalizadas o que haya creado un clúster solo privado utilizando el punto final de servicio privado que bloquea la conectividad de red pública con el registro de contenedor en el que se almacena la imagen.

{: tsResolve}
- Si tiene un cortafuegos personalizado o ha definidos políticas de Calico, permita el tráfico de red de salida y de entrada entre los nodos trabajadores y el registro de contenedor en el que se ha guardado la imagen. Si la imagen se ha guardado en {{site.data.keyword.registryshort_notm}}, revise los puertos necesarios en el apartado [Cómo permitir que el clúster acceda a recursos de la infraestructura que no sean servicios](/docs/containers?topic=containers-firewall#firewall_outbound).
- Si ha creado un clúster privado habilitando solo el punto final de servicio privado, puede [habilitar el punto final de servicio público](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable) para el clúster. Si desea instalar diagramas de Helm en un clúster privado sin abrir una conexión pública, puede instalar Helm [con Tiller](/docs/containers?topic=containers-helm#private_local_tiller) o [sin Tiller](/docs/containers?topic=containers-helm#private_install_without_tiller).

<br />


## Obtención de ayuda y soporte
{: #clusters_getting_help}

¿Sigue teniendo problemas con su clúster?
{: shortdesc}

-  En el terminal, se le notifica cuando están disponibles las actualizaciones de la CLI y los plugins de `ibmcloud`. Asegúrese de mantener actualizada la CLI para poder utilizar todos los mandatos y distintivos disponibles.
-   Para ver si {{site.data.keyword.Bluemix_notm}} está disponible, [consulte la página de estado de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/status?selected=status).
-   Publique una pregunta en [Slack de {{site.data.keyword.containerlong_notm}}![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-container-service.slack.com).
    Si no utiliza un ID de IBM para la cuenta de {{site.data.keyword.Bluemix_notm}}, [solicite una invitación](https://bxcs-slack-invite.mybluemix.net/) a este Slack.
    {: tip}
-   Revise los foros para ver si otros usuarios se han encontrado con el mismo problema. Cuando utiliza los foros para formular una pregunta, etiquete la pregunta para que la puedan ver los equipos de desarrollo de {{site.data.keyword.Bluemix_notm}}.
    -   Si tiene preguntas técnicas sobre el desarrollo o despliegue de clústeres o apps con {{site.data.keyword.containerlong_notm}}, publique su pregunta en [Stack Overflow ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) y etiquete su pregunta con `ibm-cloud`, `kubernetes` y `containers`.
    -   Para formular preguntas sobre el servicio y obtener instrucciones de iniciación, utilice el foro [IBM Developer Answers ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Incluya las etiquetas `ibm-cloud` y `containers`.
    Consulte [Obtención de ayuda](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) para obtener más detalles sobre cómo utilizar los foros.
-   Póngase en contacto con el soporte de IBM abriendo un caso. Para obtener información sobre cómo abrir un caso de soporte de IBM, o sobre los niveles de soporte y las gravedades de los casos, consulte [Cómo contactar con el servicio de soporte](/docs/get-support?topic=get-support-getting-customer-support).
Al informar de un problema, incluya el ID de clúster. Para obtener el ID de clúster, ejecute `ibmcloud ks clusters`. También puede utilizar la [herramienta de diagnósticos y de depuración de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) para recopilar y exportar la información pertinente del clúster que se va a compartir con el servicio de soporte de IBM.
{: tip}

