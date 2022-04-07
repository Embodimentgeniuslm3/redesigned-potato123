---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# Eliminación de clústeres
{: #remove}

Los clústeres gratuitos y estándares que se han creado con una cuenta facturable se deben eliminar manualmente cuando ya no sean necesarios, de manera que dichos clústeres ya no consuman recursos.
{:shortdesc}

<p class="important">
No se crean copias de seguridad del clúster ni de los datos del almacén persistente. Cuando se suprime un clúster, puede optar por suprimir el almacenamiento persistente. El almacenamiento persistente que se ha suministrado mediante una clase de almacenamiento `delete` se suprime de forma permanente en la infraestructura de IBM Cloud (SoftLayer) si elige suprimir el almacenamiento persistente. Si ha suministrado el almacenamiento persistente mediante una clase de almacenamiento `retain` y ha elegido suprimir el almacenamiento, se suprimen el clúster, el PV y la PVC, pero la instancia de almacenamiento persistente de la cuenta de infraestructura de IBM Cloud (SoftLayer) permanece.</br>
</br>Cuando se elimina un clúster, también se eliminan todas las subredes suministradas de forma automática al crear el clúster con el mandato `ibmcloud ks cluster-subnet-create`. Sin embargo, si ha añadido de forma manual subredes existentes al clúster con el mandato `ibmcloud ks cluster-subnet-add`, estas subredes no se eliminan de su cuenta (SoftLayer) de la infraestructura IBM Cloud y podrá reutilizarlas en otros clústeres.</p>

Antes de empezar:
* Anote el ID de clúster. Podría necesitar el ID de clúster para investigar y eliminar recursos (Softlayer) de la infraestructura IBM Cloud que no se suprimen de forma automática al suprimir su clúster.
* Si desea suprimir los datos en el almacenamiento persistente, [debe comprender las opciones de supresión](/docs/containers?topic=containers-cleanup#cleanup).
* Asegúrese de tener el [rol de plataforma **Administrador** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform).

Para eliminar un clúster:

1. Opcional: desde la CLI, guarde una copia de todos los datos del clúster en un archivo YAML local.
  ```
  kubectl get all --all-namespaces -o yaml
  ```
  {: pre}

2. Elimine el clúster.
  - Desde la consola de {{site.data.keyword.Bluemix_notm}}
    1.  Seleccione el clúster y pulse **Suprimir** en el menú **Más acciones...**.

  - Desde la CLI de {{site.data.keyword.Bluemix_notm}}
    1.  Liste los clústeres disponibles.

        ```
        ibmcloud ks clusters
        ```
        {: pre}

    2.  Suprima el clúster.

        ```
        ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
        ```
        {: pre}

    3.  Siga las indicaciones y decidir si desea suprimir los recursos del clúster (contenedores, pods, servicios enlazados, almacenamiento persistente y secretos).
      - **Almacenamiento persistente**: El almacenamiento persistente proporciona alta disponibilidad a los datos. Si ha creado una reclamación de volumen persistente utilizando una [compartición de archivo existente](/docs/containers?topic=containers-file_storage#existing_file), no puede suprimir la compartición de archivo al suprimir el clúster. Suprima manualmente la compartición de archivo desde el portafolio de infraestructura de IBM Cloud (SoftLayer).

          Debido al ciclo de facturación mensual, una reclamación de volumen persistente no se puede suprimir el último día del mes. Si suprime la reclamación de volumen persistente el último día del mes, la supresión permanece pendiente hasta el principio del siguiente mes.
          {: note}

Pasos siguientes:
- Después de que deje de aparecer en la lista de clústeres disponibles, al ejecutar el mandato `ibmcloud ks clusters`, podrá reutilizar el nombre de un clúster eliminado.
- Si ha conservado las subredes, puede [reutilizarlas en un nuevo clúster](/docs/containers?topic=containers-subnets#subnets_custom) o suprimirlas de forma manual más tarde de su portafolio de infraestructura de IBM Cloud (SoftLayer).
- Si ha conservado el almacenamiento persistente, puede [suprimir el almacenamiento](/docs/containers?topic=containers-cleanup#cleanup) más tarde a través el panel de control de infraestructura IBM Cloud (SoftLayer) en la consola de {{site.data.keyword.Bluemix_notm}}.
