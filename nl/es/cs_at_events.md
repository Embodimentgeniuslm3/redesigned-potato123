---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, audit

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



# Sucesos de {{site.data.keyword.cloudaccesstrailshort}}
{: #at_events}

Puede ver, gestionar y auditar las actividades que ha realizado el usuario en el clúster de {{site.data.keyword.containerlong_notm}} mediante el servicio {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

{{site.data.keyword.containershort_notm}} genera dos tipos de sucesos de {{site.data.keyword.cloudaccesstrailshort}}:

* **Sucesos de gestión de clústeres**:
    * Estos sucesos se generan automáticamente y se reenvían a {{site.data.keyword.cloudaccesstrailshort}}.
    * Puede ver estos sucesos a través del **dominio de cuenta** de {{site.data.keyword.cloudaccesstrailshort}}.

* **Sucesos de auditoría de servidor de API de Kubernetes**:
    * Estos sucesos se generan automáticamente, pero debe configurar el clúster para reenviar estos sucesos al servicio {{site.data.keyword.cloudaccesstrailshort}}.
    * Puede configurar el clúster para que envíe sucesos al **dominio de cuenta** de {{site.data.keyword.cloudaccesstrailshort}} o a un **dominio de espacio**. Para obtener más información, consulte [Envío de registros de auditoría](/docs/containers?topic=containers-health#api_forward).

Para obtener más información sobre cómo funciona el servicio, consulte la [documentación de {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started). Para obtener más información sobre las acciones de Kubernetes de las que se realiza un seguimiento, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/home/).

{{site.data.keyword.containerlong_notm}} no está configurado actualmente para que utilice {{site.data.keyword.at_full}}. Para gestionar los sucesos de gestión de clústeres y los registros de auditoría de API de Kubernetes, siga utilizando {{site.data.keyword.cloudaccesstrailfull_notm}} con LogAnalysis.
{: note}

## Búsqueda de información sobre sucesos
{: #kube-find}

Puede supervisar las actividades del clúster consultando los registros en el panel de control de Kibana.
{: shortdesc}

Para supervisar la actividad administrativa:

1. Inicie sesión en su cuenta de {{site.data.keyword.Bluemix_notm}}.
2. Desde el catálogo, suministre una instancia del servicio {{site.data.keyword.cloudaccesstrailshort}} en la misma cuenta que la instancia de {{site.data.keyword.containerlong_notm}}.
3. En el separador **Gestión** del panel de control de {{site.data.keyword.cloudaccesstrailshort}}, seleccione la cuenta o el dominio de espacio.
  * **Registros de cuenta**: los sucesos de gestión de clúster y los sucesos de auditoría de servidor de API de Kubernetes están disponibles en el **dominio de cuenta** para la región de {{site.data.keyword.Bluemix_notm}} donde se generan los sucesos.
  * **Registros de espacio**: si ha especificado un espacio al configurar el clúster para reenviar sucesos de auditoría de servidor de API de Kubernetes, estos sucesos están disponibles en el **dominio de espacio** asociado al espacio de Cloud Foundry donde se ha suministrado el servicio {{site.data.keyword.cloudaccesstrailshort}}.
4. Pulse **Ver en Kibana**.
5. Establezca el intervalo de tiempo para el que desea ver los registros. El valor predeterminado es 24 horas.
6. Para reducir la búsqueda, puede pulsar el icono de edición correspondiente a `ActivityTracker_Account_Search_in_24h` y añadir campos en la columna **Campos disponibles**.

Para permitir que otros usuarios vean sucesos de cuenta y espacio, consulte [Concesión de permisos para ver sucesos de cuentas](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions).
{: tip}

## Seguimiento de sucesos de gestión de clústeres
{: #cluster-events}

Consulte la lista siguiente de sucesos de gestión de clústeres que se envían a {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
<tr>
<th>Acción</th>
<th>Descripción</th></tr><tr>
<td><code>containers-kubernetes.account-credentials.set</code></td>
<td>Se establecen las credenciales de infraestructura en una región para un grupo de recursos.</td></tr><tr>
<td><code>containers-kubernetes.account-credentials.unset</code></td>
<td>Se desestablecen las credenciales de infraestructura en una región para un grupo de recursos.</td></tr><tr>
<td><code>containers-kubernetes.alb.create</code></td>
<td>Se crea un ALB Ingress.</td></tr><tr>
<td><code>containers-kubernetes.alb.delete</code></td>
<td>Se suprime un ALB Ingress.</td></tr><tr>
<td><code>containers-kubernetes.alb.get</code></td>
<td>Se visualiza la información de ALB Ingress.</td></tr><tr>
<td><code>containers-kubernetes.apikey.reset</code></td>
<td>Se restablece una clave de API para una región y un grupo de recursos.</td></tr><tr>
<td><code>containers-kubernetes.cluster.create</code></td>
<td>Se crea un clúster.</td></tr><tr>
<td><code>containers-kubernetes.cluster.delete</code></td>
<td>Se suprime un clúster.</td></tr><tr>
<td><code>containers-kubernetes.cluster-feature.enable</code></td>
<td>Se habilita en un clúster una característica, como por ejemplo el cálculo de confianza para los nodos trabajadores nativos.</td></tr><tr>
<td><code>containers-kubernetes.cluster.get</code></td>
<td>Se visualiza información del clúster.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.create</code></td>
<td>Se crea una configuración de reenvío de registro.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.delete</code></td>
<td>Se suprime una configuración de reenvío de registro.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.get</code></td>
<td>Se visualiza información de una configuración de reenvío de registro.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.update</code></td>
<td>Se actualiza una configuración de reenvío de registro.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.refresh</code></td>
<td>Se renueva una configuración de reenvío de registro.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.create</code></td>
<td>Se crea un filtro de registro.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.delete</code></td>
<td>Se suprime un filtro de registro.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.get</code></td>
<td>Se visualiza información de un filtro de registro.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.update</code></td>
<td>Se actualiza un filtro de registro.</td></tr><tr>
<td><code>containers-kubernetes.logging-autoupdate.changed</code></td>
<td>Se habilita o inhabilita el actualizador automático del complemento de registro.</td></tr><tr>
<td><code>containers-kubernetes.mzlb.create</code></td>
<td>Se crea un equilibrador de carga multizona.</td></tr><tr>
<td><code>containers-kubernetes.mzlb.delete</code></td>
<td>Se suprime un equilibrador de carga multizona.</td></tr><tr>
<td><code>containers-kubernetes.service.bind</code></td>
<td>Se enlaza un servicio a un clúster.</td></tr><tr>
<td><code>containers-kubernetes.service.unbind</code></td>
<td>Se desenlaza un servicio de un clúster.</td></tr><tr>
<td><code>containers-kubernetes.subnet.add</code></td>
<td>Se añade a un clúster una subred de la infraestructura de IBM Cloud (SoftLayer) existente.</td></tr><tr>
<td><code>containers-kubernetes.subnet.create</code></td>
<td>Se crea una subred.</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.add</code></td>
<td>Se añade a un clúster una subred gestionada por el usuario.</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.delete</code></td>
<td>Se elimina de un clúster una subred gestionada por el usuario.</td></tr><tr>
<td><code>containers-kubernetes.version.update</code></td>
<td>Se actualiza la versión de Kubernetes de un nodo maestro de clúster.</td></tr><tr>
<td><code>containers-kubernetes.worker.create</code></td>
<td>Se crea un nodo trabajador.</td></tr><tr>
<td><code>containers-kubernetes.worker.delete</code></td>
<td>Se suprime un nodo trabajador.</td></tr><tr>
<td><code>containers-kubernetes.worker.get</code></td>
<td>Se visualiza información de un nodo trabajador.</td></tr><tr>
<td><code>containers-kubernetes.worker.reboot</code></td>
<td>Se reinicia un nodo trabajador.</td></tr><tr>
<td><code>containers-kubernetes.worker.reload</code></td>
<td>Se vuelve a cargar un nodo trabajador.</td></tr><tr>
<td><code>containers-kubernetes.worker.update</code></td>
<td>Se actualiza un nodo trabajador.</td></tr>
</table>

## Seguimiento de sucesos de auditoría de Kubernetes
{: #kube-events}

Consulte la tabla siguiente para ver una lista de los sucesos de auditoría del servidor de API de Kubernetes que se envían a {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

Antes de empezar: asegúrese de que el clúster está configurado para reenviar [Sucesos de auditoría de API de Kubernetes](/docs/containers?topic=containers-health#api_forward).

<table>
    <th>Acción</th>
    <th>Descripción</th><tr>
    <td><code>bindings.create</code></td>
    <td>Se crea un enlace.</td></tr><tr>
    <td><code>certificatesigningrequests.create</code></td>
    <td>Se crea una solicitud para firmar un certificado.</td></tr><tr>
    <td><code>certificatesigningrequests.delete</code></td>
    <td>Se suprime una solicitud para firmar un certificado.</td></tr><tr>
    <td><code>certificatesigningrequests.patch</code></td>
    <td>Se aplica un parche a una solicitud para firmar un certificado.</td></tr><tr>
    <td><code>certificatesigningrequests.update</code></td>
    <td>Se actualiza una solicitud para firmar un certificado.</td></tr><tr>
    <td><code>clusterbindings.create</code></td>
    <td>Se ha creado un enlace de rol de clúster.</td></tr><tr>
    <td><code>clusterbindings.deleted</code></td>
    <td>Se suprime un enlace de rol de clúster.</td></tr><tr>
    <td><code>clusterbindings.patched</code></td>
    <td>Se aplica un parche a un enlace de rol de clúster.</td></tr><tr>
    <td><code>clusterbindings.updated</code></td>
    <td>Se actualiza un enlace de rol de clúster.</td></tr><tr>
    <td><code>clusterroles.create</code></td>
    <td>Se crea un rol de clúster.</td></tr><tr>
    <td><code>clusterroles.deleted</code></td>
    <td>Se suprime un rol de clúster.</td></tr><tr>
    <td><code>clusterroles.patched</code></td>
    <td>Se aplica un parche a un rol de clúster.</td></tr><tr>
    <td><code>clusterroles.updated</code></td>
    <td>Se actualiza un rol de clúster.</td></tr><tr>
    <td><code>configmaps.create</code></td>
    <td>Se crea un mapa de configuración.</td></tr><tr>
    <td><code>configmaps.delete</code></td>
    <td>Se suprime un mapa de configuración.</td></tr><tr>
    <td><code>configmaps.patch</code></td>
    <td>Se aplica un parche a un mapa de configuración.</td></tr><tr>
    <td><code>configmaps.update</code></td>
    <td>Se actualiza un mapa de configuración.</td></tr><tr>
    <td><code>controllerrevisions.create</code></td>
    <td>Se crea una revisión de controlador.</td></tr><tr>
    <td><code>controllerrevisions.delete</code></td>
    <td>Se suprime una revisión de controlador.</td></tr><tr>
    <td><code>controllerrevisions.patch</code></td>
    <td>Se aplica un parche a una revisión de controlador.</td></tr><tr>
    <td><code>controllerrevisions.update</code></td>
    <td>Se actualiza una revisión de controlador.</td></tr><tr>
    <td><code>daemonsets.create</code></td>
    <td>Se crea un conjunto de daemons.</td></tr><tr>
    <td><code>daemonsets.delete</code></td>
    <td>Se suprime un conjunto de daemons.</td></tr><tr>
    <td><code>daemonsets.patch</code></td>
    <td>Se aplica un parche a un conjunto de daemons.</td></tr><tr>
    <td><code>daemonsets.update</code></td>
    <td>Se actualiza un conjunto de daemons.</td></tr><tr>
    <td><code>deployments.create</code></td>
    <td>Se crea un despliegue.</td></tr><tr>
    <td><code>deployments.delete</code></td>
    <td>Se suprime un despliegue.</td></tr><tr>
    <td><code>deployments.patch</code></td>
    <td>Se aplica un parche a un despliegue.</td></tr><tr>
    <td><code>deployments.update</code></td>
    <td>Se actualiza un despliegue.</td></tr><tr>
    <td><code>events.create</code></td>
    <td>Se crea un suceso.</td></tr><tr>
    <td><code>events.delete</code></td>
    <td>Se suprime un suceso.</td></tr><tr>
    <td><code>events.patch</code></td>
    <td>Se aplica un parche a un suceso.</td></tr><tr>
    <td><code>events.update</code></td>
    <td>Se actualiza un suceso.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.create</code></td>
    <td>En Kubernetes v1.8, se crea una configuración de hook de admisiones externo.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.delete</code></td>
    <td>En Kubernetes v1.8, se suprime una configuración de hook de admisiones externo.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.patch</code></td>
    <td>En Kubernetes v1.8, se aplica un parche a una configuración de hook de admisiones externo.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.update</code></td>
    <td>En Kubernetes v1.8, se actualiza una configuración de hook de admisiones externo.</td></tr><tr>
    <td><code>horizontalpodautoscalers.create</code></td>
    <td>Se crea una política de escalado automático de pod horizontal.</td></tr><tr>
    <td><code>horizontalpodautoscalers.delete</code></td>
    <td>Se suprime una política de escalado automático de pod horizontal.</td></tr><tr>
    <td><code>horizontalpodautoscalers.patch</code></td>
    <td>Se aplica un parche a una política de escalado automático de pod horizontal.</td></tr><tr>
    <td><code>horizontalpodautoscalers.update</code></td>
    <td>Se actualiza una política de escalado automático de pod horizontal.</td></tr><tr>
    <td><code>ingresses.create</code></td>
    <td>Se crea un ALB Ingress.</td></tr><tr>
    <td><code>ingresses.delete</code></td>
    <td>Se suprime un ALB Ingress.</td></tr><tr>
    <td><code>ingresses.patch</code></td>
    <td>Se aplica un parche a un ALB Ingress.</td></tr><tr>
    <td><code>ingresses.update</code></td>
    <td>Se actualiza un ALB Ingress.</td></tr><tr>
    <td><code>jobs.create</code></td>
    <td>Se crea un trabajo.</td></tr><tr>
    <td><code>jobs.delete</code></td>
    <td>Se suprime un trabajo.</td></tr><tr>
    <td><code>jobs.patch</code></td>
    <td>Se aplica un parche a un trabajo.</td></tr><tr>
    <td><code>jobs.update</code></td>
    <td>Se actualiza un trabajo.</td></tr><tr>
    <td><code>localsubjectaccessreviews.create</code></td>
    <td>Se crea una revisión de acceso de asunto local.</td></tr><tr>
    <td><code>limitranges.create</code></td>
    <td>Se crea un límite de rango.</td></tr><tr>
    <td><code>limitranges.delete</code></td>
    <td>Se suprime un límite de rango.</td></tr><tr>
    <td><code>limitranges.patch</code></td>
    <td>Se aplica un parche a un límite de rango.</td></tr><tr>
    <td><code>limitranges.update</code></td>
    <td>Se actualiza un límite de rango.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.create</code></td>
    <td>Se crea una configuración de webhook mutante.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.delete</code></td>
    <td>Se suprime una configuración de webhook mutante.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.patch</code></td>
    <td>Se aplica un parche a una configuración de webhook mutante.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.update</code></td>
    <td>Se actualiza una configuración de webhook mutante.</td></tr><tr>
    <td><code>namespaces.create</code></td>
    <td>Se crea un espacio de nombres.</td></tr><tr>
    <td><code>namespaces.delete</code></td>
    <td>Se suprime un espacio de nombres.</td></tr><tr>
    <td><code>namespaces.patch</code></td>
    <td>Se aplica un parche a un espacio de nombres.</td></tr><tr>
    <td><code>namespaces.update</code></td>
    <td>Se actualiza un espacio de nombres.</td></tr><tr>
    <td><code>networkpolicies.create</code></td>
    <td>Se crea una política de red.</td></tr><tr><tr>
    <td><code>networkpolicies.delete</code></td>
    <td>Se suprime una política de red.</td></tr><tr>
    <td><code>networkpolicies.patch</code></td>
    <td>Se aplica un parche a una política de red.</td></tr><tr>
    <td><code>networkpolicies.update</code></td>
    <td>Se actualiza una política de red.</td></tr><tr>
    <td><code>nodes.create</code></td>
    <td>Se ha creado un nodo.</td></tr><tr>
    <td><code>nodes.delete</code></td>
    <td>Se suprime un nodo.</td></tr><tr>
    <td><code>nodes.patch</code></td>
    <td>Se aplica un parche a un nodo.</td></tr><tr>
    <td><code>nodes.update</code></td>
    <td>Se actualiza un nodo.</td></tr><tr>
    <td><code>persistentvolumeclaims.create</code></td>
    <td>Se crea una reclamación de volumen persistente.</td></tr><tr>
    <td><code>persistentvolumeclaims.delete</code></td>
    <td>Se suprime una reclamación de volumen persistente.</td></tr><tr>
    <td><code>persistentvolumeclaims.patch</code></td>
    <td>Se aplica un parche a una reclamación de volumen persistente.</td></tr><tr>
    <td><code>persistentvolumeclaims.update</code></td>
    <td>Se actualiza una reclamación de volumen persistente.</td></tr><tr>
    <td><code>persistentvolumes.create</code></td>
    <td>Se crea un volumen persistente.</td></tr><tr>
    <td><code>persistentvolumes.delete</code></td>
    <td>Se suprime un volumen persistente.</td></tr><tr>
    <td><code>persistentvolumes.patch</code></td>
    <td>Se aplica un parche a un volumen persistente.</td></tr><tr>
    <td><code>persistentvolumes.update</code></td>
    <td>Se actualiza un volumen persistente.</td></tr><tr>
    <td><code>poddisruptionbudgets.create</code></td>
    <td>Se crea un presupuesto de interrupción de pod.</td></tr><tr>
    <td><code>poddisruptionbudgets.delete</code></td>
    <td>Se suprime un presupuesto de interrupción de pod.</td></tr><tr>
    <td><code>poddisruptionbudgets.patch</code></td>
    <td>Se aplica un parche a un presupuesto de interrupción de pod.</td></tr><tr>
    <td><code>poddisruptionbudgets.update</code></td>
    <td>Se actualiza un presupuesto de interrupción de pod.</td></tr><tr>
    <td><code>podpresets.create</code></td>
    <td>Se crea un pod preestablecido.</td></tr><tr>
    <td><code>podpresets.deleted</code></td>
    <td>Se suprime un pod preestablecido.</td></tr><tr>
    <td><code>podpresets.patched</code></td>
    <td>Se aplica un parche a un pod preestablecido.</td></tr><tr>
    <td><code>podpresets.updated</code></td>
    <td>Se actualiza un pod preestablecido.</td></tr><tr>
    <td><code>pods.create</code></td>
    <td>Se crea un pod.</td></tr><tr>
    <td><code>pods.delete</code></td>
    <td>Se suprime un pod.</td></tr><tr>
    <td><code>pods.patch</code></td>
    <td>Se aplica un parche a un pod.</td></tr><tr>
    <td><code>pods.update</code></td>
    <td>Se actualiza un pod.</td></tr><tr>
    <td><code>podsecuritypolicies.create</code></td>
    <td>Se crea una política de seguridad de pod.</td></tr><tr>
    <td><code>podsecuritypolicies.delete</code></td>
    <td>Se suprime una política de seguridad de pod.</td></tr><tr>
    <td><code>podsecuritypolicies.patch</code></td>
    <td>Se aplica un parche a una política de seguridad de pod.</td></tr><tr>
    <td><code>podsecuritypolicies.update</code></td>
    <td>Se actualiza una política de seguridad de pod.</td></tr><tr>
    <td><code>podtemplates.create</code></td>
    <td>Se crea una plantilla de pod.</td></tr><tr>
    <td><code>podtemplates.delete</code></td>
    <td>Se suprime una plantilla de pod.</td></tr><tr>
    <td><code>podtemplates.patch</code></td>
    <td>Se aplica un parche a una plantilla de pod.</td></tr><tr>
    <td><code>podtemplates.update</code></td>
    <td>Se actualiza una plantilla de pod.</td></tr><tr>
    <td><code>replicasets.create</code></td>
    <td>Se crea un conjunto de réplicas.</td></tr><tr>
    <td><code>replicasets.delete</code></td>
    <td>Se suprime un conjunto de réplicas.</td></tr><tr>
    <td><code>replicasets.patch</code></td>
    <td>Se aplica un parche a un conjunto de réplicas.</td></tr><tr>
    <td><code>replicasets.update</code></td>
    <td>Se actualiza un conjunto de réplicas.</td></tr><tr>
    <td><code>replicationcontrollers.create</code></td>
    <td>Se crea un controlador de réplicas.</td></tr><tr>
    <td><code>replicationcontrollers.delete</code></td>
    <td>Se suprime un controlador de réplicas.</td></tr><tr>
    <td><code>replicationcontrollers.patch</code></td>
    <td>Se aplica un parche a un controlador de réplicas.</td></tr><tr>
    <td><code>replicationcontrollers.update</code></td>
    <td>Se actualiza un controlador de réplicas.</td></tr><tr>
    <td><code>resourcequotas.create</code></td>
    <td>Se crea una cuota de recursos.</td></tr><tr>
    <td><code>resourcequotas.delete</code></td>
    <td>Se suprime una cuota de recursos.</td></tr><tr>
    <td><code>resourcequotas.patch</code></td>
    <td>Se aplica un parche a una cuota de recursos.</td></tr><tr>
    <td><code>resourcequotas.update</code></td>
    <td>Se actualiza una cuota de recursos.</td></tr><tr>
    <td><code>rolebindings.create</code></td>
    <td>Se crea un enlace de rol.</td></tr><tr>
    <td><code>rolebindings.deleted</code></td>
    <td>Se suprime un enlace de rol.</td></tr><tr>
    <td><code>rolebindings.patched</code></td>
    <td>Se aplica un parche a un enlace de rol.</td></tr><tr>
    <td><code>rolebindings.updated</code></td>
    <td>Se actualiza un enlace de rol.</td></tr><tr>
    <td><code>roles.create</code></td>
    <td>Se crea un rol.</td></tr><tr>
    <td><code>roles.deleted</code></td>
    <td>Se suprime un rol.</td></tr><tr>
    <td><code>roles.patched</code></td>
    <td>Se aplica un parche a un rol.</td></tr><tr>
    <td><code>roles.updated</code></td>
    <td>Se actualiza un rol.</td></tr><tr>
    <td><code>secrets.create</code></td>
    <td>Se crea un secreto.</td></tr><tr>
    <td><code>secrets.deleted</code></td>
    <td>Se suprime un secreto.</td></tr><tr>
    <td><code>secrets.get</code></td>
    <td>Se visualiza un secreto.</td></tr><tr>
    <td><code>secrets.patch</code></td>
    <td>Se aplica un parche a un secreto.</td></tr><tr>
    <td><code>secrets.updated</code></td>
    <td>Se actualiza un secreto.</td></tr><tr>
    <td><code>selfsubjectaccessreviews.create</code></td>
    <td>Se crea una revisión de acceso de asunto propio.</td></tr><tr>
    <td><code>selfsubjectrulesreviews.create</code></td>
    <td>Se crea una revisión de reglas de asuntos propios.</td></tr><tr>
    <td><code>subjectaccessreviews.create</code></td>
    <td>Se crea una revisión de acceso de asunto.</td></tr><tr>
    <td><code>serviceaccounts.create</code></td>
    <td>Se crea una cuenta de servicio.</td></tr><tr>
    <td><code>serviceaccounts.deleted</code></td>
    <td>Se suprime una cuenta de servicio.</td></tr><tr>
    <td><code>serviceaccounts.patch</code></td>
    <td>Se aplica un parche a una cuenta de servicio.</td></tr><tr>
    <td><code>serviceaccounts.updated</code></td>
    <td>Se actualiza una cuenta de servicio.</td></tr><tr>
    <td><code>services.create</code></td>
    <td>Se crea un servicio.</td></tr><tr>
    <td><code>services.deleted</code></td>
    <td>Se suprime un servicio.</td></tr><tr>
    <td><code>services.patch</code></td>
    <td>Se aplica un parche a un servicio.</td></tr><tr>
    <td><code>services.updated</code></td>
    <td>Se actualiza un servicio.</td></tr><tr>
    <td><code>statefulsets.create</code></td>
    <td>Se crea un conjunto con estado.</td></tr><tr>
    <td><code>statefulsets.delete</code></td>
    <td>Se suprime un conjunto con estado.</td></tr><tr>
    <td><code>statefulsets.patch</code></td>
    <td>Se aplica un parche a un conjunto con estado.</td></tr><tr>
    <td><code>statefulsets.update</code></td>
    <td>Se actualiza un conjunto con estado.</td></tr><tr>
    <td><code>tokenreviews.create</code></td>
    <td>Se crea una revisión de señal.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.create</code></td>
    <td>Se crea una validación de configuración de webhook.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.delete</code></td>
    <td>Se suprime una validación de configuración de webhook.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.patch</code></td>
    <td>Se aplica un parche a una validación de configuración de webhook.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.update</code></td>
    <td>Se actualiza una validación de configuración de webhook.</td></tr>
</table>
