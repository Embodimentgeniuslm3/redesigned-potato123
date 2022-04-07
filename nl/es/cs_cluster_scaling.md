---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, node scaling

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



# Escalado de clústeres
{: #ca}

Con el plugin {{site.data.keyword.containerlong_notm}} `ibm-iks-cluster-autoscaler`, puede escalar las agrupaciones de nodos trabajadores de un clúster automáticamente a fin de aumentar o reducir el número de nodos trabajadores de la agrupación en función de los requisitos de las cargas de trabajo planificadas. El plugin `ibm-iks-cluster-autoscaler` se basa en el [proyecto Cluster-Autoscaler de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler).
{: shortdesc}

¿Prefiere escalar automáticamente los pods? Consulte el apartado sobre [Escalado de apps](/docs/containers?topic=containers-app#app_scaling).
{: tip}

El programa de escalado automático de clústeres está disponible para los clústeres estándares configurados con conectividad de red pública. Si el clúster no puede acceder a la red pública, como por ejemplo un clúster privado detrás de un cortafuegos o un clúster con solo el punto final de servicio privado habilitado, no puede utilizar el programa de escalado automático de clústeres en su clúster.
{: important}

## Escalado hacia arriba (ampliar) y hacia abajo (reducir)
{: #ca_about}

El programa de escalado automático de clústeres explora periódicamente el clúster para ajustar el número de nodos trabajadores de las agrupaciones de nodos trabajadores que gestiona como respuesta a solicitudes de recursos de cargas de trabajo y a valores personalizados que configura el usuario, como por ejemplo intervalos de exploración. Cada minuto, el programa de escalado automático de clústeres comprueba las situaciones siguientes.
{: shortdesc}

*   **Pods pendientes de escalado hacia arriba**: Se considera que un pod está pendiente cuando no existen suficientes recursos de cálculo para planificar el pod en un nodo trabajador. Cuando el programa de escalado automático de clústeres detecta que hay pods pendientes, aumenta el número de nodos trabajadores de forma equitativa en las zonas para satisfacer las solicitudes de recursos de las cargas de trabajo.
*   **Nodos trabajadores infrautilizados a reducir**: De forma predeterminada, los nodos trabajadores que se ejecutan con menos del 50 % del total de recursos de cálculo solicitados durante 10 minutos o más y cuyas cargas de trabajo se pueden volver a planificar en otros nodos trabajadores se consideran infrautilizados. Si el programa de escalado automático de clústeres detecta nodos trabajadores infrautilizados, reduce el número de nodos trabajadores de uno en uno para que tenga solo los recursos de cálculo que necesita. Si lo desea, puede [personalizar](/docs/containers?topic=containers-ca#ca_chart_values) el umbral predeterminado de utilización de reducción del 50 % durante 10 minutos.

La exploración y el escalado se realizan a intervalos regulares a lo largo del tiempo, y, en función del número de nodos trabajadores, pueden tardar más tiempo en completarse, como por ejemplo 30 minutos.

El programa de escalado automático de clústeres ajusta el número de nodos trabajadores teniendo en cuenta las [solicitudes de recursos ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) que ha definido para los despliegues, no el uso real de los nodos trabajadores. Si sus pods y despliegues no solicitan cantidades adecuadas de recursos, debe ajustar sus archivos de configuración. El programa de escalado automático de clústeres no los puede ajustar automáticamente. Tenga también en cuenta que los nodos trabajadores utilizan algunos de sus recursos de cálculo para funcionalidad básica del clúster, [complementos](/docs/containers?topic=containers-update#addons) predeterminados y personalizados y [reservas de recursos](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).
{: note}

<br>
**¿Cómo funciona el escalado?**<br>
En general, el programa de escalado automático de clústeres calcula el número de nodos trabajadores que necesita el clúster para ejecutar su carga de trabajo. El escalado del clúster depende de muchos factores, incluidos los siguientes.
*   El tamaño de nodo trabajador mínimo y máximo por zona que se ha establecido.
*   Las solicitudes de recursos de pod pendientes y determinados metadatos que el usuario asocia con la carga de trabajo, como por ejemplo antiafinidad, etiquetas para colocar pods únicamente en determinados tipos de máquina o [presupuestos de interrupción de pod ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/).
*   Las agrupaciones de nodos trabajadores que gestiona el programa de escalado automático de clústeres, potencialmente en las zonas de un [clúster multizona](/docs/containers?topic=containers-ha_clusters#multizone).
*   Los [valores de diagrama de Helm personalizados](#ca_chart_values) que hay establecidos, como por ejemplo omitir nodos trabajadores para su supresión si utilizan almacenamiento local.

Para obtener más información, consulte, en las preguntas frecuentes sobre el programa de escalado automático de clústeres de Kubernetes, los temas [¿Cómo funciona el escalado hacia arriba? ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-up-work) y [¿Cómo funciona el escalado hacia abajo? ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-down-work).

<br>

**¿Puedo cambiar la forma en que funciona el escalado hacia arriba y hacia abajo?**<br>
Puede personalizar los valores o utilizar otros recursos de Kubernetes para influir en el funcionamiento del escalado hacia arriba y hacia abajo.
*   **Escalado hacia arriba**: [Personalice los valores del diagrama de Helm del programa de escalado automático de clústeres](#ca_chart_values) como por ejemplo `scanInterval`, `expander`, `skipNodes` o `maxNodeProvisionTime`. Revise las formas de [sobresuministrar a los nodos trabajadores](#ca_scaleup) para poder ampliar los nodos trabajadores antes de que una agrupación de trabajadores se quede sin recursos. También puede [configurar interrupciones de presupuesto de pod de Kubernetes y límites de prioridad de pods](#scalable-practices-apps) para influir en el funcionamiento del escalado hacia arriba.
*   **Escalado hacia abajo**: [Personalice los valores del diagrama de Helm del programa de escalado automático de clústeres](#ca_chart_values) como por ejemplo `scaleDownUnneededTime`, `scaleDownDelayAfterAdd`, `scaleDownDelayAfterDelete` o `scaleDownUtilizationThreshold`.

<br>
**¿Puedo establecer el tamaño mínimo por zona para aumentar de inmediato mi clúster a dicho tamaño?**<br>
No, el hecho de establecer `minSize` no activa automáticamente un aumento. El valor `minSize` es un umbral para que el programa de escalado automático del clúster no reduzca por debajo de un determinado número de nodos trabajadores por zona. Si el clúster aún no tiene dicho número por zona, el programa de escalado automático del clúster no lo aumenta hasta que tenga solicitudes de recursos de carga de trabajo que necesiten más recursos. Por ejemplo, si tiene una agrupación de nodos trabajadores con un nodo trabajador por tres zonas (tres nodos trabajadores en total) y establece el valor de `minSize` en `4` por zona, el programa de escalado automático del clúster no suministra inmediatamente tres nodos trabajadores adicionales por zona (12 nodos trabajadores en total). El aumento lo activan las solicitudes de recursos. Si crea una carga de trabajo que solicita los recursos de 15 nodos trabajadores, el programa de escalado automático del clúster aumenta la agrupación de nodos trabajadores para satisfacer esta solicitud. El valor `minSize` significa que el programa de escalado automático del clúster no reduce por debajo de cuatro nodos trabajadores por zona, aunque elimine la carga de trabajo que solicita esta cantidad.

<br>
**¿En qué se diferencia este comportamiento de las agrupaciones de nodos trabajadores que no gestiona el programa de escalado automático de clústeres?**<br>
Cuando [crea una agrupación de nodos trabajadores](/docs/containers?topic=containers-add_workers#add_pool), especifica el número de nodos trabajadores que tiene por zona. La agrupación de nodos trabajadores mantiene este número de nodos trabajadores hasta que el usuario [redimensiona](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) o [vuelve a equilibrar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance). La agrupación de nodos trabajadores no añade ni elimina nodos trabajadores automáticamente. Si tiene más pods de los que se pueden planificar, los pods permanecen en estado pendiente hasta que el usuario redimensiona la agrupación de nodos trabajadores.

Cuando se habilita el programa de escalado automático de clústeres para una agrupación de nodos trabajadores, se aumenta o se reduce el número de nodos trabajadores en respuesta a los valores de especificación de pod y a las solicitudes de recursos. No es necesario cambiar el tamaño de la agrupación de nodos trabajadores ni volver a equilibrar la agrupación de nodos trabajadores manualmente.

<br>
**¿Puedo ver un ejemplo de cómo aumenta o reduce el número de nodos trabajadores el programa de escalado automático de clústeres?**<br>
Examine la imagen siguiente para ver un ejemplo de aumento o reducción de número de nodos trabajadores del clúster.

_Figura: Escalado automático de un clúster._
![GIF de escalado automático de un clúster](images/cluster-autoscaler-x3.gif){: gif}

1.  El clúster tiene cuatro nodos trabajadores en dos agrupaciones de nodos trabajadores distribuidas en dos zonas. Cada agrupación tiene un nodo trabajador por zona, pero la **Agrupación de nodos trabajadores A** tiene el tipo de máquina `u2c.2x4` y la **Agrupación de nodos trabajadores B** tiene el tipo de máquina `b2c.4x16`. Los recursos totales de cálculo son de aproximadamente 10 núcleos (2 núcleos x 2 nodos trabajadores para la **Agrupación de nodos trabajadores A** y 4 núcleos x 2 nodos trabajadores para la **Agrupación de nodos trabajadores B**). Actualmente el clúster ejecuta una carga de trabajo que solicita 6 de estos 10 núcleos. Los recursos de cálculo adicionales se toman en cada nodo trabajador por medio de los [recursos reservados](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node) necesarios para ejecutar el clúster, los nodos trabajadores y cualquier complemento, como por ejemplo el programa de escalado automático de clústeres.
2.  El programa de escalado automático de clústeres está configurado para gestionar ambas agrupaciones de nodos trabajadores con el siguiente tamaño mínimo y máximo por zona:
    *  **Agrupación de nodos trabajadores A**: `minSize=1`, `maxSize=5`.
    *  **Agrupación de nodos trabajadores B**: `minSize=1`, `maxSize=2`.
3.  El usuario planifica despliegues que requieran 14 réplicas adicionales de pod de una app que solicita un núcleo de CPU por réplica. Se puede desplegar una réplica de pod en los recursos actuales, pero los otros 13 están pendientes.
4.  El programa de escalado automático de clústeres aumenta el número de nodos trabajadores dentro de estas restricciones para dar soporte a las solicitudes de recursos de las 13 réplicas de pod adicionales.
    *  **Agrupación de nodos trabajadores A**: Se añaden siete nodos trabajadores en un método round robin de la forma más uniforme posible entre las zonas. Los nodos trabajadores aumentan la capacidad de cálculo del clúster en aproximadamente 14 núcleos (2 núcleos x 7 nodos trabajadores).
    *  **Agrupación de nodos trabajadores B**: Se añaden dos nodos trabajadores de forma uniforme entre las zonas, con lo que se alcanza el `maxSize` de 2 nodos trabajadores por zona. Los nodos trabajadores aumentan la capacidad del clúster en aproximadamente 8 núcleos (4 núcleos x 2 nodos trabajadores).
5.  Las solicitudes de los 20 con 1 núcleo se distribuyen del siguiente modo entre los nodos trabajadores. Debido a que los nodos trabajadores tienen reservas de recursos, así como pods que se ejecutan para cubrir las características predeterminadas del clúster, los pods correspondientes a la carga de trabajo no pueden utilizar todos los recursos de cálculo disponibles de un nodo trabajador. Por ejemplo, aunque los nodos trabajadores `b2c.4x16` tienen cuatro núcleos, solo se pueden planificar en los nodos trabajadores tres pods que solicitan un mínimo de un núcleo cada uno.
    <table summary="Una tabla en la que se describe la distribución de la carga de trabajo en un clúster escalado.">
    <caption>Distribución de la carga de trabajo en un clúster escalado.</caption>
    <thead>
    <tr>
      <th>Agrupación de nodos trabajadores</th>
      <th>Zona</th>
      <th>Tipo</th>
      <th>Número de nodos trabajadores</th>
      <th>Número de pods</th>
    </tr>
    </thead>
    <tbody>
    <tr>
      <td>A</td>
      <td>dal10</td>
      <td>u2c.2x4</td>
      <td>Cuatro nodos</td>
      <td>Tres pods</td>
    </tr>
    <tr>
      <td>A</td>
      <td>dal12</td>
      <td>u2c.2x4</td>
      <td>Cinco nodos</td>
      <td>Cinco pods</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal10</td>
      <td>b2c.4x16</td>
      <td>Dos nodos</td>
      <td>Seis pods</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal12</td>
      <td>b2c.4x16</td>
      <td>Dos nodos</td>
      <td>Seis pods</td>
    </tr>
    </tbody>
    </table>
6.  Ya no se necesita la carga de trabajo adicional, por lo que suprime el despliegue. Después de un breve periodo de tiempo, el programa de escalado automático de clústeres detecta que el clúster ya no necesita todos sus recursos de cálculo y empieza a reduce el número de nodos trabajadores de uno en uno.
7.  Las agrupaciones de nodos trabajadores se escalan hacia abajo. El programa de escalado automático de clústeres realiza exploraciones a intervalos regulares para comprobar si hay solicitudes de recursos de pod pendientes y nodos trabajadores infrautilizados para escalar las agrupaciones de nodos trabajadores hacia arriba o hacia abajo.

## Seguir prácticas de despliegue escalable
{: #scalable-practices}

Para aprovechar al máximo el programa de escalado automático de clústeres utilice las siguientes estrategias para los nodos trabajadores y los despliegues de cargas de trabajo. Para obtener más información, consulte las [preguntas frecuentes sobre el programa de escalado automático de clústeres de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md).
{: shortdesc}

[Pruebe el programa de escalado automático de clústeres](#ca_helm) con algunas cargas de trabajo de prueba para obtener una idea de cómo [cómo funciona el escalado hacia arriba o hacia abajo](#ca_about), los [valores personalizados](#ca_chart_values) que puede configurar y otros aspectos que pueden interesarle, como por ejemplo el [sobresuministro](#ca_scaleup) de nodos trabajadores o la [limitación de apps](#ca_limit_pool). A continuación, limpie el entorno de prueba y planifique incluir estos valores personalizados y parámetros adicionales en una instalación nueva del programa de escalado automático de clústeres.

### ¿Puedo escalar automáticamente varias agrupaciones de trabajadores a la vez?
{: #scalable-practices-multiple}
Sí, después de instalar el diagrama de Helm, puede elegir las agrupaciones de trabajadores dentro del clúster que desea escalar automáticamente [en el mapa de configuración](#ca_cm). Sólo puede ejecutar un diagrama de Helm `ibm-iks-cluster-autoscaler` por clúster.
{: shortdesc}

### ¿Cómo puedo asegurarme de que el programa de escalado automático de clústeres responde a los recursos que necesita mi app?
{: #scalable-practices-resrequests}

El programa de escalado automático de clústeres escala el clúster en respuesta a las [solicitudes de recursos ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) de la carga de trabajo. Así, especifique [solicitudes de recursos ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) para todos los despliegues porque las solicitudes de recursos son lo que utiliza el programa de escalado automático de clústeres para calcular el número de nodos trabajadores que se necesitan para ejecutar la carga de trabajo. Tenga en cuenta que el escalado automático se basa en el uso de cálculo que solicitan las configuraciones de la carga de trabajo y no tiene en cuenta otros factores, como los costes de la máquina.
{: shortdesc}

### ¿Puedo reducir una agrupación de trabajadores a cero (0) nodos?
{: #scalable-practices-zero}

No, no puede establecer el programa de escalado automático de clústeres `minSize` a `0`. Además, a menos que [inhabilite](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure) todos los equilibradores de carga de la aplicación (ALB) públicos en cada zona del clúster, debe cambiar el valor de `minSize` a `2` nodos trabajadores por zona para que los pods de ALB se puedan distribuir para alcanzar una alta disponibilidad.
{: shortdesc}

### ¿Puedo optimizar mis despliegues para el escalado automático?
{: #scalable-practices-apps}

Sí, puede añadir varias características de Kubernetes al despliegue para ajustar la forma en que el programa de escalado automático de clústeres considera las solicitudes de recursos para el escalado.
{: shortdesc}
*   Utilice [presupuestos de interrupción de pods ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) para evitar las replanificaciones o las supresiones abruptas de sus pods.
*   Si utiliza la prioridad de pods, puede [editar el límite de corte de prioridad ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-cluster-autoscaler-work-with-pod-priority-and-preemption) para cambiar los tipos de prioridades que activan el escalado. De forma predeterminada, el límite de corte de prioridad es cero (`0`).

### ¿Puedo usar antagonismos y tolerancias con agrupaciones de trabajadores escaladas automáticamente?
{: #scalable-practices-taints}

Puesto que no se pueden aplicar antagonismos a nivel de agrupación de nodos trabajadores, no [especifique antagonismos en nodos trabajadores](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) para evitar resultados inesperados. Por ejemplo, cuando despliega una carga de trabajo que no toleran los nodos trabajadores antagónicos, los nodos trabajadores no se tienen en cuenta para el escalado hacia arriba y puede ser que se soliciten más nodos trabajadores aunque el clúster tenga suficiente capacidad. Sin embargo, los nodos trabajadores antagónicos se siguen identificando como infrautilizados si tienen menos del umbral (de forma predeterminada, el 50 %) de sus recursos utilizados y, por lo tanto, se tienen en cuenta para el escalado hacia abajo.
{: shortdesc}

### ¿Por qué están desequilibradas mis agrupaciones de trabajadores escaladas automáticamente?
{: #scalable-practices-unbalanced}

Durante un escalado hacia arriba, el programa de escalado automático de clústeres equilibra los nodos entre zonas, con una diferencia permitida de más o menos un (+/-1) nodo trabajador. Es posible que las cargas de trabajo pendientes no soliciten suficiente capacidad para hacer que cada zona esté equilibrada. En este caso, si desea equilibrar manualmente las agrupaciones de nodos trabajadores, [actualice el mapa de configuración del programa de escalado automático de clústeres](#ca_cm) para eliminar la agrupación de trabajadores desequilibrada. Luego ejecute el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) `ibmcloud ks worker-pool-rebalance` y vuelva a añadir la agrupación de nodos trabajadores al mapa de configuración del programa de escalado automático del clúster.
{: shortdesc}


### ¿Por qué no puedo cambiar el tamaño o volver a equilibrar mi agrupación de nodos trabajadores?
{: #scalable-practices-resize}

Cuando el programa de escalado automático de clústeres está habilitado para una agrupación de nodos trabajadores, no puede [redimensionar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) ni [volver a equilibrar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) sus agrupaciones de nodos trabajadores. Debe [editar el mapa de configuración](#ca_cm) para cambiar el tamaño mínimo o máximo de la agrupación de nodos trabajadores o debe inhabilitar el escalado automático de clústeres para dicha agrupación de nodos trabajadores. No utilice el [mandato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_rm) `ibmcloud ks worker-rm` para eliminar nodos trabajadores individuales de la agrupación de nodos trabajadores, lo que podría desequilibrar la agrupación de nodos trabajadores.
{: shortdesc}

Además, si no inhabilita las agrupaciones de nodos trabajadores antes de desinstalar el diagrama de Helm `ibm-iks-cluster-autoscaler`,
las agrupaciones de nodos trabajadores no se pueden redimensionar manualmente. Vuelva a instalar el diagrama de Helm `ibm-iks-cluster-autoscaler`,
[edite el mapa de configuración](#ca_cm) para inhabilitar la agrupación de nodos trabajadores e inténtelo de nuevo.

<br />


## Despliegue del diagrama de Helm del programa de escalado automático de clústeres en el clúster
{: #ca_helm}

Instale el plugin del programa de escalado automático de clústeres de {{site.data.keyword.containerlong_notm}} con un diagrama de Helm para escalar automáticamente las agrupaciones de nodos trabajadores en el clúster.
{: shortdesc}

**Antes de empezar**:

1.  [Instale los plugins necesarios y la CLI](/docs/cli?topic=cloud-cli-getting-started):
    *  CLI de {{site.data.keyword.Bluemix_notm}} (`ibmcloud`)
    *  Plugin {{site.data.keyword.containerlong_notm}} (`ibmcloud ks`)
    *  Plugin {{site.data.keyword.registrylong_notm}} (`ibmcloud cr`)
    *  Kubernetes (`kubectl`)
    *  Helm (`helm`)
2.  [Cree un clúster estándar](/docs/containers?topic=containers-clusters#clusters_ui) que ejecute **Kubernetes versión 1.12 o posterior**.
3.   [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
4.  Confirme que las credenciales de {{site.data.keyword.Bluemix_notm}} Identity and Access Management están almacenadas en el clúster. El programa de escalado automático de clústeres utiliza este secreto para autenticar las credenciales. Si falta el secreto, [créelo restableciendo las credenciales](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions).
    ```
    kubectl get secrets -n kube-system | grep storage-secret-store
    ```
    {: pre}
5.  El programa de escalado automático de clústeres solo puede escalar las agrupaciones de nodo trabajadores que tienen la etiqueta `ibm-cloud.kubernetes.io/worker-pool-id`.
    1.  Compruebe si la agrupación nodos trabajadores tiene la etiqueta necesaria.
        ```
        ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels
        ```
        {: pre}

        Salida de ejemplo de agrupación de nodos trabajadores con la etiqueta:
        ```
        Labels:             ibm-cloud.kubernetes.io/worker-pool-id=a1aa111111b22b22cc3c3cc444444d44-4d555e5
        ```
        {: screen}
    2.  Si la agrupación de nodos trabajadores no tiene la etiqueta necesaria, [añada una nueva agrupación de nodos trabajadores](/docs/containers?topic=containers-add_workers#add_pool) y utilice esta agrupación de nodos trabajadores con el programa de escalado automático de clústeres.


<br>
**Para instalar el plugin `ibm-iks-cluster-autoscaler` en el clúster**:

1.  [Siga las instrucciones](/docs/containers?topic=containers-helm#public_helm_install) para instalar el cliente **Helm versión 2.11 o posterior** en la máquina local e instale el servidor de Helm (tiller) con una cuenta de servicio en el clúster.
2.  Verifique que el tiller se ha instalado con una cuenta de servicio.

    ```
    kubectl get serviceaccount -n kube-system tiller
    ```
    {: pre}

    Salida de ejemplo:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}
3.  Añada y actualice el repositorio de Helm en el que se encuentra el diagrama de Helm del programa de escalado automático de clústeres.
    ```
    helm repo add iks-charts https://icr.io/helm/iks-charts
    ```
    {: pre}
    ```
    helm repo update
    ```
    {: pre}
4.  Instale el diagrama de Helm del programa de escalado automático de clústeres en el espacio de nombres `kube-system` de su clúster.

    Durante la instalación, puede [personalizar más los valores del programa de escalado automático de clústeres](#ca_chart_values), como por ejemplo la cantidad de tiempo que espera hasta escalar hacia arriba o hacia abajo los nodos trabajadores.
    {: tip}

    ```
    helm install iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Salida de ejemplo:
    ```
    NAME:   ibm-iks-cluster-autoscaler
    LAST DEPLOYED: Thu Nov 29 13:43:46 2018
    NAMESPACE: kube-system
    STATUS: DEPLOYED

    RESOURCES:
    ==> v1/ClusterRole
    NAME                       AGE
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Pod(related)

    NAME                                        READY  STATUS             RESTARTS  AGE
    ibm-iks-cluster-autoscaler-67c8f87b96-qbb6c  0/1    ContainerCreating  0         1s

    ==> v1/ConfigMap

    NAME              AGE
    iks-ca-configmap  1s

    ==> v1/ClusterRoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Role
    ibm-iks-cluster-autoscaler  1s

    ==> v1/RoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Service
    ibm-iks-cluster-autoscaler  1s

    ==> v1beta1/Deployment
    ibm-iks-cluster-autoscaler  1s

    ==> v1/ServiceAccount
    ibm-iks-cluster-autoscaler  1s

    NOTES:
    Thank you for installing: ibm-iks-cluster-autoscaler. Your release is named: ibm-iks-cluster-autoscaler

    Para obtener más información sobre cómo utilizar el programa de escalado automático de clústeres, consulte el archivo README.md del diagrama.
    ```
    {: screen}

5.  Compruebe que la instalación es correcta.

    1.  Compruebe que el pod del programa de escalado automático de clústeres tiene el estado **Running** (En ejecución).
        ```
        kubectl get pods --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        Salida de ejemplo:
        ```
        ibm-iks-cluster-autoscaler-8497bfc968-dbn7w   1/1       Running   0          9m
        ```
        {: screen}
    2.  Compruebe que se ha creado el servicio del programa de escalado automático de clústeres.
        ```
        kubectl get service --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        Salida de ejemplo:
        ```
        ibm-iks-cluster-autoscaler   ClusterIP   172.21.xxx.xx    <none>        8085/TCP        9m
        ```
        {: screen}

6.  Repita estos pasos para cada clúster donde desea suministrar el programa de escalado automático de clústeres.

7.  Para empezar a escalar sus agrupaciones de nodos trabajadores, consulte [Actualización de la configuración del programa de escalado automático de clústeres](#ca_cm).

<br />


## Actualización del mapa de configuración del programa de escalado automático de clústeres para habilitar el escalado
{: #ca_cm}

Actualice el mapa de configuración del programa de escalado automático de clústeres para habilitar el escalado automático de nodos trabajadores en las agrupaciones de nodos trabajadores en función de los valores mínimos y máximos que ha establecido.
{: shortdesc}

Después de editar el mapa de configuración para habilitar una agrupación de nodos trabajadores, el programa de escalado automático de clústeres escala el clúster en respuesta a las solicitudes de carga de trabajo. Por lo tanto, no puede [redimensionar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) ni [volver a equilibrar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) las agrupaciones de nodos trabajadores. La exploración y el escalado se realizan a intervalos regulares a lo largo del tiempo, y, en función del número de nodos trabajadores, pueden tardar más tiempo en completarse, como por ejemplo 30 minutos. Más adelante, si desea [eliminar el programa de escalado automático de clústeres](#ca_rm),
primero deberá inhabilitar cada agrupación de nodos trabajadores en el mapa de configuración.
{: note}

**Antes de empezar**:
*  [Instale el plugin `ibm-iks-cluster-autoscaler`](#ca_helm).
*  [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**Para actualizar el mapa de configuración y los valores del programa de escalado automático de clústeres**:

1.  Edite el archivo YAML del mapa de configuración del programa de escalado automático de clústeres.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system -o yaml
    ```
    {: pre}
    Salida de ejemplo:
    ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false}
        ]
    kind: ConfigMap
    metadata:
      creationTimestamp: 2018-11-29T18:43:46Z
      name: iks-ca-configmap
      namespace: kube-system
      resourceVersion: "2227854"
      selfLink: /api/v1/namespaces/kube-system/configmaps/iks-ca-configmap
      uid: b45d047b-f406-11e8-b7f0-82ddffc6e65e
    ```
    {: screen}
2.  Edite el mapa de configuración con los parámetros para definir cómo debe escalar la agrupación de nodos trabajadores el programa de escalado automático de clústeres. **Nota:** A menos que [inhabilite](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure) todos los equilibradores de carga de aplicación (ALB) públicos en cada zona del clúster estándar, debe cambiar el valor `minSize` por `2` por zona para que las pods de ALB se puedan propagar para alta disponibilidad.

    <table>
    <caption>Parámetros del mapa de configuración del programa de escalado automático de clústeres</caption>
    <thead>
    <th id="parameter-with-default">Parámetro con valor predeterminado</th>
    <th id="parameter-with-description">Descripción</th>
    </thead>
    <tbody>
    <tr>
    <td id="parameter-name" headers="parameter-with-default">`"name": "default"`</td>
    <td headers="parameter-name parameter-with-description">Sustituya `"default"` por el nombre o ID de la agrupación de nodos trabajadores que desea escalar. Para ver una lista de las agrupaciones de nodos trabajadores, ejecute `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`.<br><br>
    Para gestionar más de una agrupación de nodos trabajadores, copie la línea JSON en una línea separada por comas, como se indica a continuación. <pre class="codeblock">[
     {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
     {"name": "Pool2","minSize": 2,"maxSize": 5,"enabled":true}
    ]</pre><br><br>
    **Nota**: El programa de escalado automático de clústeres solo puede escalar las agrupaciones de nodo trabajadores que tienen la etiqueta `ibm-cloud.kubernetes.io/worker-pool-id`. Para comprobar si la agrupación de nodos trabajadores tiene la etiqueta necesaria, ejecute `ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels`. Si la agrupación de nodos trabajadores no tiene la etiqueta necesaria, [añada una nueva agrupación de nodos trabajadores](/docs/containers?topic=containers-add_workers#add_pool) y utilice esta agrupación de nodos trabajadores con el programa de escalado automático de clústeres.</td>
    </tr>
    <tr>
    <td id="parameter-minsize" headers="parameter-with-default">`"minSize": 1`</td>
    <td headers="parameter-minsize parameter-with-description">Especifique el número mínimo de nodos trabajadores por zona a los que el programa de escalado automático del clúster puede reducir la agrupación de nodos trabajadores. El valor debe ser `2` o superior para que las pods de ALB se puedan propagar para alta disponibilidad. Si [inhabilita](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure) todos los ALB públicos en cada zona de su clúster estándar, puede establecer el valor en `1`.
    <p class="note">El hecho de establecer `minSize` no activa automáticamente un aumento. El valor `minSize` es un umbral para que el programa de escalado automático del clúster no reduzca por debajo de un determinado número de nodos trabajadores por zona. Si el clúster aún no tiene dicho número por zona, el programa de escalado automático del clúster no lo aumenta hasta que tenga solicitudes de recursos de carga de trabajo que necesiten más recursos. Por ejemplo, si tiene una agrupación de nodos trabajadores con un nodo trabajador por tres zonas (tres nodos trabajadores en total) y establece el valor de `minSize` en `4` por zona, el programa de escalado automático del clúster no suministra inmediatamente tres nodos trabajadores adicionales por zona (12 nodos trabajadores en total). El aumento lo activan las solicitudes de recursos. Si crea una carga de trabajo que solicita los recursos de 15 nodos trabajadores, el programa de escalado automático del clúster aumenta la agrupación de nodos trabajadores para satisfacer esta solicitud. El valor `minSize` significa que el programa de escalado automático del clúster no reduce por debajo de cuatro nodos trabajadores por zona, aunque elimine la carga de trabajo que solicita esta cantidad. Para obtener más información, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#when-does-cluster-autoscaler-change-the-size-of-a-cluster).</p></td>
    </tr>
    <tr>
    <td id="parameter-maxsize" headers="parameter-with-default">`"maxSize": 2`</td>
    <td headers="parameter-maxsize parameter-with-description">Especifique el número máximo de nodos trabajadores por zona a los que el programa de escalado automático del clúster puede aumentar la agrupación de nodos trabajadores. El valor debe ser igual o mayor que el valor que establezca para `minSize`.</td>
    </tr>
    <tr>
    <td id="parameter-enabled" headers="parameter-with-default">`"enabled": false`</td>
    <td headers="parameter-enabled parameter-with-description">Establezca el valor en `true` para que el programa de escalado automático de clústeres gestione el escalado para la agrupación de nodos trabajadores. Establezca el valor en `false` para detener el escalado de la agrupación de nodos trabajadores por parte del programa de escalado automático de clústeres.<br><br>
    Más adelante, si desea [eliminar el programa de escalado automático de clústeres](#ca_rm),
primero deberá inhabilitar cada agrupación de nodos trabajadores en el mapa de configuración.</td>
    </tr>
    </tbody>
    </table>
3.  Guarde el archivo de configuración.
4.  Obtenga el pod del programa de escalado automático de clústeres.
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
5.  Revise la sección **`Events`** del pod del programa de escalado automático de clústeres para ver si hay un suceso **`ConfigUpdated`** para verificar que la correlación de configuración se ha actualizado correctamente. El mensaje de suceso correspondiente al mapa de configuración tiene el formato siguiente: `minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`.

    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    Salida de ejemplo:
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
    Type    Reason                 Age   From                     Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

## Personalización de los valores de configuración del diagrama de Helm del programa de escalado automático de clústeres
{: #ca_chart_values}

Personalice los valores del programa de escalado automático de clústeres, como por ejemplo el intervalo de tiempo que espera antes de aumentar o reducir el número de nodos trabajadores.
{: shortdesc}

**Antes de empezar**:
*  [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  [Instale el plugin `ibm-iks-cluster-autoscaler`](#ca_helm).

**Para los valores del programa de escalado automático de clústeres**:

1.  Revise los valores de configuración del diagrama de Helm del programa de escalado automático de clústeres. El programa de escalado automático de clústeres se suministra con valores predeterminados. Sin embargo, es posible que desee cambiar algunos valores, como por ejemplo los intervalos de escalado hacia abajo o de exploración, en función de la frecuencia con la que cambien las cargas de trabajo del clúster.
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}

    Salida de ejemplo:
    ```
    expander: least-waste
    image:
      pullPolicy: Always
      repository: icr.io/iks-charts/ibm-iks-cluster-autoscaler
      tag: dev1
    maxNodeProvisionTime: 120m
    resources:
      limits:
        cpu: 300m
        memory: 300Mi
      requests:
        cpu: 100m
        memory: 100Mi
    scaleDownDelayAfterAdd: 10m
    scaleDownDelayAfterDelete: 10m
    scaleDownUtilizationThreshold: 0.5
    scaleDownUnneededTime: 10m
    scanInterval: 1m
    skipNodes:
      withLocalStorage: true
      withSystemPods: true
    ```
    {: codeblock}

    <table>
    <caption>Valores de configuración del programa de escalado automático de clústeres</caption>
    <thead>
    <th>Parámetro</th>
    <th>Descripción</th>
    <th>Valor predeterminado</th>
    </thead>
    <tbody>
    <tr>
    <td>Parámetro `api_route`</td>
    <td>Establezca el [punto final de API de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cli_api) para la región en la que se encuentra el clúster.</td>
    <td>No hay valor predeterminado; utiliza la región de destino en la que se encuentra el clúster.</td>
    </tr>
    <tr>
    <td>Parámetro `expander`</td>
    <td>Especifique cómo determina el programa de escalado automático de clústeres la agrupación de nodo trabajadores se debe escalar si tiene varias agrupaciones de nodos trabajadores. Los valores posibles son:
    <ul><li>`random`: Selecciona aleatoriamente entre `most-pods` y `least-waste`.</li>
    <li>`most-pods`: Selecciona la agrupación de nodos trabajadores que puede planificar la mayor cantidad de pods cuando se escala hacia arriba. Utilice este método si va a utilizar `nodeSelector` para asegurarse de que los pods van a parar a nodos trabajadores específicos.</li>
    <li>`least-waste`: Selecciona la agrupación de nodos trabajadores que tiene menos CPU no utilizada después de aumentar el número de nodos. Si dos agrupaciones de nodos trabajadores utilizan la misma cantidad de recursos de CPU después de aumentar el número de nodos, se selecciona la agrupación de nodos trabajadores con menos memoria no utilizada.</li></ul></td>
    <td>random</td>
    </tr>
    <tr>
    <td>Parámetro `image.repository`</td>
    <td>Especifique la imagen de Docker que debe utilizar el programa de escalado automático de clústeres.</td>
    <td>`icr.io/iks-charts/ibm-iks-cluster-autoscaler`</td>
    </tr>
    <tr>
    <td>Parámetro `image.pullPolicy`</td>
    <td>Especifique cuándo se debe extraer la imagen de Docker. Los valores posibles son:
    <ul><li>`Always`: Extrae la imagen cada vez que se inicia el pod.</li>
    <li>`IfNotPresent`: Extrae la imagen solo si la imagen no está presente localmente.</li>
    <li>`Never`: Da por supuesto que la imagen existe localmente y nunca extrae la imagen.</li></ul></td>
    <td>Always</td>
    </tr>
    <tr>
    <td>Parámetro `maxNodeProvisionTime`</td>
    <td>Establezca el intervalo máximo de tiempo en minutos que se puede tardar en empezar a suministrar un nodo trabajador antes de que el programa de escalado automático de clústeres cancele la solicitud de escalado hacia arriba.</td>
    <td>`120m`</td>
    </tr>
    <tr>
    <td>Parámetro `resources.limits.cpu`</td>
    <td>Establezca la cantidad máxima de CPU de nodo trabajador que puede consumir el pod `ibm-iks-cluster-autoscaler`.</td>
    <td>`300m`</td>
    </tr>
    <tr>
    <td>Parámetro `resources.limits.memory`</td>
    <td>Establezca la cantidad máxima de memoria de nodo trabajador que puede consumir el pod `ibm-iks-cluster-autoscaler`.</td>
    <td>`300Mi`</td>
    </tr>
    <tr>
    <td>Parámetro `resources.requests.cpu`</td>
    <td>Establezca la cantidad mínima de CPU de nodo trabajador con la que empieza el pod `ibm-iks-cluster-autoscaler`.</td>
    <td>`100m`</td>
    </tr>
    <tr>
    <td>Parámetro `resources.requests.memory`</td>
    <td>Establezca la cantidad mínima de memoria de nodo trabajador con la que empieza el pod `ibm-iks-cluster-autoscaler`.</td>
    <td>`100Mi`</td>
    </tr>
    <tr>
    <td>Parámetro `scaleDownUnneededTime`</td>
    <td>Establezca el intervalo de tiempo en minutos que no se necesite un nodo trabajador antes de que se escale hacia abajo.</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>Parámetros `scaleDownDelayAfterAdd`, `scaleDownDelayAfterDelete`</td>
    <td>Establezca el intervalo de tiempo en minutos que el programa de escalado automático de clústeres espera para iniciar acciones de escalado después de escalar hacia arriba (`add`) o hacia abajo (`delete`).</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>Parámetro `scaleDownUtilizationThreshold`</td>
    <td>Establezca el umbral de utilización de nodo trabajador. Si la utilización del nodo trabajador queda por debajo del umbral, el nodo trabajador se tiene en cuenta para el escalado hacia abajo. La utilización del nodo trabajador se calcula como la suma de los recursos de CPU y de memoria que solicitan todos los pods que se ejecutan en el nodo trabajador dividido por la capacidad de recursos del nodo trabajador.</td>
    <td>`0.5`</td>
    </tr>
    <tr>
    <td>Parámetro `scanInterval`</td>
    <td>Establezca la frecuencia en minutos con la que el programa de escalado automático de clústeres explora el uso de la carga de trabajo que activa el escalado hacia arriba o hacia abajo.</td>
    <td>`1m`</td>
    </tr>
    <tr>
    <td>Parámetro `skipNodes.withLocalStorage`</td>
    <td>Si tiene el valor `true`, los nodos trabajadores que tienen pods que guardan datos en el almacenamiento local no se escalan hacia abajo.</td>
    <td>`true`</td>
    </tr>
    <tr>
    <td>Parámetro `skipNodes.withSystemPods`</td>
    <td>Si tiene el valor `true`, los nodos trabajadores que tienen pods `kube-system` no se escalan. No establezca el valor en `false` porque el escalado hacia abajo de pods `kube-system` puede dar lugar a resultados inesperados.</td>
    <td>`true`</td>
    </tr>
    </tbody>
    </table>
2.  Para cambiar alguno de los valores de configuración del programa de escalado automático de clústeres, actualice el diagrama de Helm con los nuevos valores. Incluya el distintivo `--recreate-pods` para que se vuelvan a crear los pods del programa de escalado automático de clústeres para coger los cambios de los valores personalizados.
    ```
    helm upgrade --set scanInterval=2m ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler -i --recreate-pods
    ```
    {: pre}

    Para restablecer los valores predeterminados del diagrama:
    ```
    helm upgrade --reset-values ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler --recreate-pods
    ```
    {: pre}
3.  Para verificar los cambios, revise de nuevo los valores del diagrama de Helm.
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}


## Limitación de apps para que se ejecuten solo en determinadas agrupaciones de trabajadores escaladas automáticamente
{: #ca_limit_pool}

Para limitar un despliegue de pod a una agrupación de trabajadores específica gestionada por el programa de escalado automático de clústeres, utilice etiquetas `nodeSelector` y `nodeAffinity`. Con `nodeAffinity`, tiene más control sobre cómo funciona el comportamiento de planificación para combinar pods con nodos trabajadores. Para obtener más información sobre la asignación de pods a nodos trabajadores, [consulte la documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/).
{: shortdesc}

**Antes de empezar**:
*  [Instale el plugin `ibm-iks-cluster-autoscaler`](#ca_helm).
*  [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**Para limitar los pods que se ejecutan en determinadas agrupaciones de trabajadores escaladas automáticamente**:

1.  Cree la agrupación de nodos trabajadores con la etiqueta que desea utilizar. Por ejemplo, la etiqueta podría ser `app: nginx`.
    ```
    ibmcloud ks worker-pool-create --name <name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_worker_nodes> --labels <key>=<value>
    ```
    {: pre}
2.  [Añada la agrupación de nodos trabajadores a la configuración del programa de escalado automático de clústeres](#ca_cm).
3.  En la plantilla de especificación del pod, haga coincidir `nodeSelector` o `nodeAffinity` con la etiqueta que ha utilizado en la agrupación de nodos trabajadores.

    Ejemplo de `nodeSelector`:
    ```
    ...
    spec:
          containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
      nodeSelector:
        app: nginx
    ...
    ```
    {: codeblock}

    Ejemplo de `nodeAffinity`:
    ```
    spec:
          containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: app
              operator: In
              values:
                - nginx
    ```
    {: codeblock}
4.  Despliegue el pod. Debido a la etiqueta coincidente, el pod se planifica en un nodo trabajador que se encuentra en la agrupación de nodos trabajadores etiquetada.
    ```
    kubectl apply -f pod.yaml
    ```
    {: pre}

<br />


## Escalado hacia arriba de nodos trabajadores antes de que la agrupación de nodos trabajadores no tenga recursos suficientes
{: #ca_scaleup}

Tal como se ha descrito en el tema [Visión general del funcionamiento del programa de escalado automático de clústeres](#ca_about) y en las [preguntas frecuentes sobre el programa de escalado automático de clústeres de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md), el programa de escalado automático de clústeres aumenta el número de nodos trabajadores de las agrupaciones de nodos trabajadores como respuesta a los recursos solicitados de la carga de trabajo frente a los recursos disponibles en la agrupación de nodos trabajadores. Sin embargo, es posible que desee que el programa de escalado automático de clústeres escale hacia arriba los nodos trabajadores antes de que la agrupación de nodos trabajadores se quede sin recursos. En este caso, la carga de trabajo no tiene que esperar tanto tiempo a que se suministren los nodos trabajadores porque la agrupación de nodos trabajadores ya se ha escalado hacia arriba para satisfacer las solicitudes de recursos.
{: shortdesc}

El programa de escalado automático de clústeres no da soporte al escalado temprano (sobresuministro) de agrupaciones de nodos trabajadores. Sin embargo, puede configurar otros recursos de Kubernetes para que trabajen con el programa de escalado automático de clústeres para conseguir un escalado temprano.

<dl>
  <dt><strong>Pods en pausa</strong></dt>
  <dd>Puede crear un despliegue que despliegue [contenedores en pausa ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://stackoverflow.com/questions/48651269/what-are-the-pause-containers) en pods con determinadas solicitudes de recursos y asigne al despliegue una prioridad de pod baja. Cuando cargas de trabajo de prioridad más alta necesiten estos recursos, el pod de pausa se anticipa y se convierte en un pod pendiente. Este suceso activa el escalado hacia arriba del programa de escalado automático de clústeres.<br><br>Para obtener más información sobre cómo configurar un despliegue de pod de pausa, consulte las [preguntas frecuentes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-can-i-configure-overprovisioning-with-cluster-autoscaler). Puede utilizar [este ejemplo de archivo de configuración de sobresuministro ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/IBM-Cloud/kube-samples/blob/master/ibm-ks-cluster-autoscaler/overprovisioning-autoscaler.yaml) para crear la clase de prioridad, la cuenta de servicio y los despliegues.<p class="note">Si utiliza este método, asegúrese de que entiende cómo funciona la [prioridad de pod](/docs/containers?topic=containers-pod_priority#pod_priority) y cómo establecer la prioridad de pod para los despliegues. Por ejemplo, si el pod en pausa no tiene suficientes recursos para un pod de prioridad más alta, el pod no se anticipa. La carga de trabajo de prioridad más alta permanece en espera, por lo que el programa de escalado automático de clústeres se activa para que escale hacia arriba. Sin embargo, en este caso la acción de escalado hacia arriba no es temprana porque la carga de trabajo que desea ejecutar no se puede planificar debido a una insuficiencia de recursos.</p></dd>

  <dt><strong>Escalado automático de pod horizontal (HPA)</strong></dt>
  <dd>Puesto que el escalado automático de pod horizontal se basa en el uso medio de CPU de los pods, el límite de uso de CPU establecido se alcanza antes de que la agrupación de trabajadores se quede sin recursos. Se solicitan más pods, lo que activa el programa de escalado automático de clústeres para escalar hacia arriba la agrupación de nodos trabajadores.<br><br>Para obtener más información sobre cómo configurar HPA, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/).</dd>
</dl>

<br />


## Actualización del diagrama de Helm del programa de escalado automático de clústeres
{: #ca_helm_up}

Puede actualizar el diagrama de Helm del programa de escalado automático de clústeres existente a la versión más reciente. Para comprobar la versión actual del diagrama de Helm, ejecute `helm ls | grep cluster-autoscaler`.
{: shortdesc}

¿Desea actualizar al diagrama de Helm más reciente desde la versión 1.0.2 o anterior? [Siga estas instrucciones](#ca_helm_up_102).
{: note}

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Actualice el repositorio de Helm para recuperar la última versión de todos los diagramas de Helm de este repositorio.
    ```
    helm repo update
    ```
    {: pre}

2.  Opcional: descargue el último diagrama de Helm en la máquina local. A continuación, extraiga el paquete y revise el archivo `release.md`
para ver la información de release más reciente.
    ```
    helm fetch iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

3.  Busque el nombre del diagrama de Helm del programa de escalado automático de clústeres que ha instalado en el clúster.
    ```
    helm ls | grep cluster-autoscaler
    ```
    {: pre}

    Salida de ejemplo:
    ```
    myhelmchart 	1       	Mon Jan  7 14:47:44 2019	DEPLOYED	ibm-ks-cluster-autoscaler-1.0.0  	kube-system
    ```
    {: screen}

4.  Actualice el diagrama de Helm del programa de escalado automático de clústeres a la versión más reciente.
    ```
    helm upgrade --force --recreate-pods <helm_chart_name>  iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

5.  Verifique que la sección del [mapa de configuración del programa de escalado automático de clústeres](#ca_cm) `workerPoolsConfig.json`
se ha establecido en `"enabled":true` para las agrupaciones de nodos trabajadores que desea escalar.
    ```
    kubectl describe cm iks-ca-configmap -n kube-system
    ```
    {: pre}

    Salida de ejemplo:
    ```
    Name:         iks-ca-configmap
    Namespace:    kube-system
    Labels:       <none>
    Annotations:  kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","data":{"workerPoolsConfig.json":"[\n {\"name\": \"docs\",\"minSize\": 1,\"maxSize\": 3,\"enabled\":true}\n]\n"},"kind":"ConfigMap",...

    Data
    ====
    workerPoolsConfig.json:
    ----
    [
     {"name": "docs","minSize": 1,"maxSize": 3,"enabled":true}
    ]

    Events:  <none>
    ```
    {: screen}

### Actualización al diagrama de Helm más reciente desde la versión 1.0.2 o anterior
{: #ca_helm_up_102}

La versión más reciente del diagrama de Helm del programa de escalado automático de clústeres requiere una eliminación completa de las versiones anteriormente instaladas del diagrama de Helm del programa de escalado automático de clústeres. Si ha instalado el diagrama de Helm versión 1.0.2 o anterior, desinstale dicha versión antes de instalar el diagrama de Helm más reciente del programa de escalado automático de clústeres.
{: shortdesc}

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Obtenga el mapa de configuración del programa de escalado automático de clústeres.
    ```
    kubectl get cm iks-ca-configmap -n kube-system -o yaml > iks-ca-configmap.yaml
    ```
    {: pre}
2.  Elimine todas las agrupaciones de nodos trabajadores del mapa de configuración estableciendo el valor `"enabled"` en `false`.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
3.  Si ha aplicado valores personalizados al diagrama de Helm, anote los valores personalizados.
    ```
    helm get values ibm-ks-cluster-autoscaler -a
    ```
    {: pre}
4.  Desinstale el diagrama de Helm actual.
    ```
    helm delete --purge ibm-ks-cluster-autoscaler
    ```
    {: pre}
5.  Actualice el repositorio del diagrama de Helm para obtener la versión más reciente del diagrama de Helm del programa de escalado automático de clústeres.
    ```
    helm repo update
    ```
    {: pre}
6.  Instale el diagrama de Helm más reciente del programa de escalado automático de clústeres. Aplique los valores personalizados que ha utilizado anteriormente con el distintivo `--set`, como por ejemplo `scanInterval=2m`.
    ```
    helm install  iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler [--set <custom_settings>]
    ```
    {: pre}
7.  Aplique el mapa de configuración del programa de escalado automático de clústeres que ha recuperado anteriormente para habilitar el escalado automático para las agrupaciones de nodos trabajadores.
    ```
    kubectl apply -f iks-ca-configmap.yaml
    ```
    {: pre}
8.  Obtenga el pod del programa de escalado automático de clústeres.
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
9.  Revise la sección **`Events`** del pod del programa de escalado automático de clústeres y busque un suceso **`ConfigUpdated`** para verificar que la correlación de configuración se ha actualizado correctamente. El mensaje de suceso correspondiente al mapa de configuración tiene el formato siguiente: `minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`.
    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    Salida de ejemplo:
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
    Type    Reason                 Age   From                     Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

<br />


## Eliminación del programa de escalado automático de clústeres
{: #ca_rm}

Si no desea escalar automáticamente las agrupaciones de nodos trabajadores, puede desinstalar el diagrama de Helm del programa de escalado automático de clústeres. Después de la eliminación, debe [redimensionar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) o [volver a equilibrar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) las agrupaciones de nodos trabajadores manualmente.
{: shortdesc}

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  En el [mapa de configuración del programa de escalado automático de clústeres](#ca_cm), elimine la agrupación de nodos trabajadores estableciendo el valor de `"enabled"` en `false`.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
    Salida de ejemplo:
    ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
         {"name":"mypool","minSize":1,"maxSize":5,"enabled":false}
        ]
    kind: ConfigMap
    ...
    ```
2.  Obtenga una lista de los diagramas de Helm existentes y anote el nombre del programa de escalado automático de clústeres.
    ```
    helm ls
    ```
    {: pre}
3.  Elimine el diagrama de Helm existente del clúster.
    ```
    helm delete --purge <ibm-iks-cluster-autoscaler_name>
    ```
    {: pre}
