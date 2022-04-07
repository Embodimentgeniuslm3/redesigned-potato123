---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

keywords: kubernetes, iks, helm

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


# Integraciones de servicios de IBM Cloud y de terceros
{: #ibm-3rd-party-integrations}

Puede utilizar integraciones de servicios de infraestructura y de plataforma de {{site.data.keyword.Bluemix_notm}} y de terceros para añadir prestaciones al clúster.
{: shortdesc}

## Servicios de IBM Cloud
{: #ibm-cloud-services}

Revise la información siguiente para ver cómo se integran servicios de plataforma y de infraestructura de {{site.data.keyword.Bluemix_notm}} con {{site.data.keyword.containerlong_notm}} y cómo los puede utilizar en el clúster.
{: shortdesc}

### Servicios de plataforma de IBM Cloud
{: #platform-services}

Todos los servicios de plataforma de {{site.data.keyword.Bluemix_notm}} que dan soporte a claves de servicio se pueden integrar mediante [enlaces de servicio](/docs/containers?topic=containers-service-binding) de {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

El enlace de servicios es una forma rápida de crear credenciales de servicio para un servicio de {{site.data.keyword.Bluemix_notm}} y almacenar estas credenciales en un secreto de Kubernetes en el clúster. El secreto de Kubernetes se cifra automáticamente en etcd para proteger los datos. Las apps pueden utilizar las credenciales del secreto para acceder a la instancia de servicio de {{site.data.keyword.Bluemix_notm}}.

Los servicios que no admiten claves de servicio suelen proporcionar una API que puede utilizar directamente en la app.

Para ver una visión general de los servicios populares de {{site.data.keyword.Bluemix_notm}}, consulte [Integraciones populares](/docs/containers?topic=containers-supported_integrations#popular_services).

### Servicios de infraestructura de IBM Cloud
{: #infrastructure-services}

Puesto que {{site.data.keyword.containerlong_notm}} le permite crear un clúster de Kubernetes basado en la infraestructura de {{site.data.keyword.Bluemix_notm}}, algunos servicios de infraestructura, como por ejemplo servidores virtuales, servidores nativos o VLAN, están totalmente integrados en {{site.data.keyword.containerlong_notm}}. Estas instancias de servicios se crean y se trabaja con las mismas mediante la API, la CLI o la consola de {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Las soluciones de almacenamiento persistente soportadas, como por ejemplo {{site.data.keyword.Bluemix_notm}} File Storage, {{site.data.keyword.Bluemix_notm}} Block Storage o {{site.data.keyword.cos_full}}, se integran como controladores flex de Kubernetes y se pueden configurar mediante [diagramas de Helm](/docs/containers?topic=containers-helm). El diagrama de Helm configura automáticamente las clases de almacenamiento de Kubernetes, el proveedor de almacenamiento y el controlador de almacenamiento en el clúster. Puede utilizar las clases de almacenamiento para suministrar almacenamiento persistente mediante reclamaciones de volumen persistente (PVC).

Para proteger la red del clúster o para conectar con un centro de datos local, puede configurar una de las opciones siguientes:
- [Servicio VPN IPSec de strongSwan](/docs/containers?topic=containers-vpn#vpn-setup)
- [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)
- [Virtual Router Appliance (VRA)](/docs/containers?topic=containers-vpn#vyatta)
- [Fortigate Security Appliance (FSA)](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)

## Comunidad de Kubernetes e integraciones de código abierto
{: #kube-community-tools}

Puesto que es el propietario de los clústeres estándares que crea en {{site.data.keyword.containerlong_notm}}, puede optar por instalar soluciones de terceros para añadir funciones adicionales al clúster.
{: shortdesc}

Algunas tecnologías de código abierto, como Knative, Istio, LogDNA, Sysdig o Portworx, han sido probadas por IBM y se proporcionan como complementos gestionados, diagramas de Helm o servicios de {{site.data.keyword.Bluemix_notm}} con el soporte del proveedor de servicios en colaboración con IBM. Estas herramientas de código abierto están totalmente integradas en el sistema de facturación y de soporte de {{site.data.keyword.Bluemix_notm}}.

Puede instalar otras herramientas de código abierto en el clúster, pero es posible que estas herramientas no estén gestionadas o soportadas o que su funcionamiento en {{site.data.keyword.containerlong_notm}} no esté verificado.

### Integraciones soportadas en asociación
{: #open-source-partners}

Para obtener más información sobre los colaboradores de {{site.data.keyword.containerlong_notm}} y las ventajas de cada solución que proporcionan, consulte el apartado sobre [Socios de {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-service-partners).

### Complementos gestionados
{: #cluster-add-ons}
{{site.data.keyword.containerlong_notm}} integra varias integraciones populares de código abierto, como [Knative](/docs/containers?topic=containers-serverless-apps-knative) o [Istio](/docs/containers?topic=containers-istio), mediante [complementos gestionados](/docs/containers?topic=containers-managed-addons). Los complementos gestionados constituyen un sistema sencillo de instalar una herramienta de código abierto en el clúster que ha sido probada por IBM y que su uso en {{site.data.keyword.containerlong_notm}} ha sido aprobado.

Los complementos gestionados se integran completamente en la organización de soporte de {{site.data.keyword.Bluemix_notm}}. Si tiene alguna pregunta o algún problema con el uso de los complementos gestionados, puede utilizar uno de los canales de soporte de {{site.data.keyword.containerlong_notm}}. Para obtener más información, consulte [Obtención de ayuda y soporte](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

Si la herramienta que añade a su clúster incurre en costes, estos costes se integran automáticamente y se muestran como parte de la facturación mensual de {{site.data.keyword.Bluemix_notm}}. {{site.data.keyword.Bluemix_notm}} determina el ciclo de facturación según cuándo haya habilitado el complemento en el clúster.

### Otras integraciones de terceros
{: #kube-community-helm}

Puede instalar cualquier herramienta de origen abierto de terceros que se integre con Kubernetes. Por ejemplo, la comunidad de Kubernetes designa determinados diagramas de Helm como `estables` o como `incubadoras`. Tenga en cuenta que no se ha verificado el funcionamiento en {{site.data.keyword.containerlong_notm}} de estos diagramas o herramientas. Si la herramienta requiere una licencia, debe adquirirla para poder utilizar la herramienta. Para obtener una visión general de los diagramas de Helm disponibles de la comunidad de Kubernetes, consulte los repositorios `kubernetes` y `kubernetes-incubator` en el catálogo [diagramas de Helm ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/kubernetes/solutions/helm-charts).
{: shortdesc}

Los costes en los que incurra por utilizar una integración de código abierto de terceros no se incluyen en su factura mensual de {{site.data.keyword.Bluemix_notm}}.

La instalación de integraciones de código abierto de terceros o de diagramas de Helm desde la comunidad de Kubernetes puede cambiar la configuración predeterminada del clúster y puede llevar el clúster a un estado no soportado. Si tiene algún problema con el uso de cualquiera de estas herramientas, consulte directamente la comunidad de Kubernetes o el proveedor de servicios.
{: important}
