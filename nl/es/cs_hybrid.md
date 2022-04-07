---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Nube híbrida
{: #hybrid_iks_icp}

Si tiene una cuenta privada de {{site.data.keyword.Bluemix}}, puede utilizarla con determinados servicios de {{site.data.keyword.Bluemix_notm}}, incluido {{site.data.keyword.containerlong}}. Para obtener más información, consulte el blog sobre [Experiencia híbrida en nube pública de IBM y privada de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://ibm.biz/hybridJune2018).
{: shortdesc}

Si ya conoce las [ofertas de {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-cs_ov#differentiation) y ha desarrollado la estrategia de Kubernetes sobre qué [cargas de trabajo ejecutar en la nube](/docs/containers?topic=containers-strategy#cloud_workloads). Ahora, puede conectar su nube pública y privada mediante el servicio VPN de strongSwan o {{site.data.keyword.BluDirectLink}}.

* El [servicio VPN de strongSwan](#hybrid_vpn) conecta de forma segura el clúster de Kubernetes con una red local a través de un canal de comunicación seguro de extremo a extremo a través de Internet que se basa en la suite de protocolos de Internet Protocol Security (IPSec) estándar del sector.
* Con [{{site.data.keyword.Bluemix_notm}} Direct Link](#hybrid_dl), puede crear una conexión directa y privada entre los entornos de red remotos y {{site.data.keyword.containerlong_notm}} sin direccionamiento a través de Internet público.

Después de conectar la nube pública y privada, puede [reutilizar los paquetes privados para contenedores públicos](#hybrid_ppa_importer).

## Conexión de la nube pública y privada con la VPN de strongSwan
{: #hybrid_vpn}

Establezca conectividad de VPN entre el clúster de Kubernetes público y la instancia privada de {{site.data.keyword.Bluemix}} para permitir la comunicación bidireccional.
{: shortdesc}

1.  Cree un clúster estándar con {{site.data.keyword.containerlong}} en {{site.data.keyword.Bluemix_notm}} público o utilice uno existente. Para crear un clúster, elija entre las opciones siguientes:
    - [Cree un clúster estándar desde la consola o la CLI](/docs/containers?topic=containers-clusters#clusters_ui).
    - [Utilice Cloud Automation Manager (CAM) para crear un clúster utilizando una plantilla predefinida ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_deploy_IKS.html). Cuando se despliega un clúster con CAM, el tiller de Helm se instala automáticamente.

2.  En el clúster de {{site.data.keyword.containerlong_notm}}, [siga las instrucciones para configurar el servicio VPN IPSec de strongSwan](/docs/containers?topic=containers-vpn#vpn_configure).

    *  Para el [Paso 2](/docs/containers?topic=containers-vpn#strongswan_2), tenga en cuenta que:

       * El valor de `local.id` que defina en el clúster de {{site.data.keyword.containerlong_notm}} debe coincidir con el valor que establezca después como `remote.id` en el clúster privado de {{site.data.keyword.Bluemix}}.
       * El valor de `remote.id` que defina en el clúster de {{site.data.keyword.containerlong_notm}} debe coincidir con el valor que establezca después como `local.id` en el clúster privado de {{site.data.keyword.Bluemix}}.
       * El valor de `preshared.secret` que defina en el clúster de {{site.data.keyword.containerlong_notm}} debe coincidir con el valor que establezca después como `preshared.secret` en el clúster privado de {{site.data.keyword.Bluemix}}.

    *  Para el [Paso 3](/docs/containers?topic=containers-vpn#strongswan_3), configure strongSwan para una conexión VPN **entrante**.

       ```
       ipsec.auto: add
       loadBalancerIP: <portable_public_IP>
       ```
       {: codeblock}

3.  Anote la dirección IP pública portátil que ha establecido como `loadbalancerIP`.

    ```
    kubectl get svc vpn-strongswan
    ```
    {: pre}

4.  [Cree un clúster en {{site.data.keyword.Bluemix_notm}} privado ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/installing/installing.html).

5.  En el clúster privado de {{site.data.keyword.Bluemix_notm}}, despliegue el servicio VPN IPSec de strongSwan.

    1.  [Complete las soluciones alternativas de VPN IPSec de strongSwan ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_strongswan.html).

    2.  [Configure el diagrama de Helm de VPN de strongSwan ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/app_center/create_release.html) en el clúster privado.

        *  En los parámetros de configuración, defina el campo **Remote gateway** en el valor de la dirección IP pública portátil que ha establecido como `loadbalancerIP` del clúster de {{site.data.keyword.containerlong_notm}}.

           ```
           Operation at startup: start
           ...
           Remote gateway: <portable_public_IP>
           ...
           ```
           {: codeblock}

        *  Recuerde que el valor de `local.id` privado debe coincidir con el valor de `remote.id` público, el valor de `remote.id` privado debe coincidir con el valor de `local.id` público y los valores de `preshared.secret` público y privado deben coincidir.

        Ahora, puede iniciar una conexión desde el clúster privado de {{site.data.keyword.Bluemix_notm}} al clúster de {{site.data.keyword.containerlong_notm}}.

7.  [Pruebe la conexión VPN](/docs/containers?topic=containers-vpn#vpn_test) entre los clústeres.

8.  Repita estos pasos para cada clúster que desee conectar.

**¿Qué es lo siguiente?**

*   [Ejecute las imágenes de software para las que tenga licencia en los clústeres públicos](#hybrid_ppa_importer).
*   Para gestionar varios clústeres de Kubernetes de nube, como por ejemplo entre {{site.data.keyword.Bluemix_notm}} Público y {{site.data.keyword.Bluemix_notm}} Privado, consulte [IBM Multicloud Manager ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).


## Conexión de la nube pública y privada con {{site.data.keyword.Bluemix_notm}} Direct Link
{: #hybrid_dl}

Con [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link), puede crear una conexión directa y privada entre los entornos de red remotos y {{site.data.keyword.containerlong_notm}} sin direccionamiento a través de Internet público.
{: shortdesc}

Para conectar la nube pública y la instancia local de {{site.data.keyword.Bluemix}} Private, puede utilizar una de estas cuatro ofertas:
* {{site.data.keyword.Bluemix_notm}} Direct Link Connect
* {{site.data.keyword.Bluemix_notm}} Direct Link Exchange
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated Hosting

Para elegir una conexión de {{site.data.keyword.Bluemix_notm}} Direct Link y configurar una conexión de {{site.data.keyword.Bluemix_notm}} Direct Link, consulte [Iniciación a {{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-) en la documentación de {{site.data.keyword.Bluemix_notm}} Direct Link.

**¿Qué es lo siguiente?**</br>
* [Ejecute las imágenes de software para las que tenga licencia en los clústeres públicos](#hybrid_ppa_importer).
* Para gestionar varios clústeres de Kubernetes de nube, como por ejemplo entre {{site.data.keyword.Bluemix_notm}} Público y {{site.data.keyword.Bluemix_notm}} Privado, consulte [IBM Multicloud Manager ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).

<br />


## Ejecución de imágenes de {{site.data.keyword.Bluemix_notm}} privado en contenedores públicos de Kubernetes
{: #hybrid_ppa_importer}

Puede ejecutar determinados productos bajo licencia de IBM empaquetados para {{site.data.keyword.Bluemix_notm}} privado en un clúster de {{site.data.keyword.Bluemix_notm}} público.  
{: shortdesc}

El software bajo licencia está disponible en [IBM Passport Advantage ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www-01.ibm.com/software/passportadvantage/index.html). Para utilizar este software en un clúster de {{site.data.keyword.Bluemix_notm}} público, debe descargar el software, extraer la imagen y cargar la imagen en el espacio de nombres en {{site.data.keyword.registryshort}}. Independientemente del entorno en el que tenga previsto utilizar el software, debe obtener primero la licencia necesaria para el producto.

En la tabla siguiente se ofrece una visión general de los productos disponibles de {{site.data.keyword.Bluemix_notm}} privado que puede utilizar en el clúster de {{site.data.keyword.Bluemix_notm}} público.

| Nombre del producto | Versión | Número de pieza |
| --- | --- | --- |
| IBM Db2 Direct Advanced Edition Server | 11.1 | CNU3TML |
| IBM Db2 Advanced Enterprise Server Edition Server | 11.1 | CNU3SML |
| IBM MQ Advanced | 9.1.0.0, 9.1.1,0, 9.1.2.0 | - |
| IBM WebSphere Application Server Liberty | 16.0.0.3 | Imagen de Docker Hub |
{: caption="Tabla. Productos de {{site.data.keyword.Bluemix_notm}} privado soportados que se pueden utilizar en {{site.data.keyword.Bluemix_notm}} público." caption-side="top"}

Antes de empezar:
- [Instale el plugin de la CLI de {{site.data.keyword.registryshort}} (`ibmcloud cr`)](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#cli_namespace_registry_cli_install).
- [Configure un espacio de nombres en {{site.data.keyword.registryshort}}](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#registry_namespace_setup) o recupere el espacio de nombres existente ejecutando `ibmcloud cr namespaces`.
- [Defina el clúster como destino de la CLI `kubectl`](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- [Instale la CLI de Helm y configure el servicio tiller en el clúster](/docs/containers?topic=containers-helm#public_helm_install).

Para desplegar una imagen de {{site.data.keyword.Bluemix_notm}} privado en {{site.data.keyword.Bluemix_notm}} público:

1.  Siga los pasos de la [documentación de {{site.data.keyword.registryshort}}](/docs/services/Registry?topic=registry-ts_index#ts_ppa) para descargar el software abajo licencia de IBM Passport Advantage, enviar por push la imagen al espacio de nombres e instalar el diagrama de Helm en el clúster.

    **Para IBM WebSphere Application Server Liberty**:

    1.  En lugar de obtener la imagen de IBM Passport Advantage, utilice la [imagen de Docker Hub ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://hub.docker.com/_/websphere-liberty/). Para obtener instrucciones sobre cómo obtener una licencia de producción, consulte [Actualización de la imagen de Docker Hub a una imagen de producción ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/WASdev/ci.docker/tree/master/ga/production-upgrade).

    2.  Siga las [instrucciones
del diagrama de Helm de Liberty ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_icp_helm.html).

2.  Verifique que el campo **STATUS** del diagrama de Helm muestra `DEPLOYED`. Si no es así, espere unos minutos e inténtelo de nuevo.
    ```
    helm status <helm_chart_name>
    ```
    {: pre}

3.  Consulte la documentación específica del producto para obtener más información sobre cómo configurar y utilizar el producto con el clúster.

    - [IBM Db2 Direct Advanced Edition Server ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.licensing.doc/doc/c0070181.html)
    - [IBM MQ Advanced ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.html)
    - [IBM WebSphere Application Server Liberty ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/as_ditamaps/was900_welcome_liberty.html)
