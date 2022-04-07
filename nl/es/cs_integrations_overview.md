---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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
{:preview: .preview}f


# Integraciones soportadas entre IBM Cloud y terceros
{: #supported_integrations}

Puede utilizar diversos servicios externos y servicios del catálogo con un clúster de Kubernetes estándar en {{site.data.keyword.containerlong}}.
{:shortdesc}

## Integraciones populares
{: #popular_services}

<table summary="En la tabla se muestran los servicios disponibles que puede añadir a su clúster y que son muy populares entre los usuarios de {{site.data.keyword.containerlong_notm}}. Las filas se leen de izquierda a derecha, con el nombre del servicio en la columna uno y una descripción del servicio en la columna dos.">
<caption>Servicios populares</caption>
<thead>
<tr>
<th>Servicio</th>
<th>Categoría</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Registros de actividad del clúster</td>
<td>Supervise la actividad administrativa realizada en el clúster mediante el análisis de registros a través de Grafana. Para obtener más información sobre el servicio, consulte la documentación de [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started). Para obtener más información sobre los tipos de sucesos de los que puede realizar un seguimiento, consulte [sucesos de Activity Tracker](/docs/containers?topic=containers-at_events).</td>
</tr>
<tr>
<td>{{site.data.keyword.appid_full}}</td>
<td>Autenticación</td>
<td>Añada un nivel de seguridad a sus apps con [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) requiriendo a los usuarios que inicien sesión. Para autenticar solicitudes HTTP/HTTPS de API o web para la app, puede integrar {{site.data.keyword.appid_short_notm}} con el servicio Ingress mediante la [anotación de Ingress de autenticación de {{site.data.keyword.appid_short_notm}}](/docs/containers?topic=containers-ingress_annotation#appid-auth).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} Block Storage</td>
<td>Almacenamiento en bloque</td>
<td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) es un almacenamiento iSCSI persistente y de alto rendimiento que puede añadir a las apps mediante volúmenes persistentes (PV) de Kubernetes. Utilice el almacenamiento en bloque para desplegar apps con estado en una sola zona o como almacenamiento de alto rendimiento para pods individuales. Para obtener más información sobre cómo suministrar almacenamiento en bloque en el clúster, consulte [Almacenamiento de datos en {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/containers?topic=containers-block_storage#block_storage)</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>Certificados TLS</td>
<td>Puede utilizar <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> para almacenar y gestionar certificados SSL para sus apps. Para obtener más información, consulte <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Uso de {{site.data.keyword.cloudcerts_long_notm}} con {{site.data.keyword.containerlong_notm}} para desplegar certificados TLS de dominio personalizados <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.registrylong}}</td>
<td>Imágenes de contenedor</td>
<td>Configure su propio repositorio de imágenes de Docker protegidas para almacenar y compartir de forma segura las imágenes entre los usuarios del clúster. Para obtener más información, consulte la <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">documentación de {{site.data.keyword.registrylong}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.</td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automatización de compilación</td>
<td>Automatice las compilaciones de las apps y los despliegues de los contenedores en clústeres de Kubernetes utilizando a cadena de herramientas. Para obtener más información sobre la configuración, consulte el blog sobre <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Despliegue de pods de Kubernetes en {{site.data.keyword.containerlong_notm}} mediante conductos de DevOps <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.datashield_full}} (Beta)</td>
<td>Cifrado de memoria</td>
<td>Puede utilizar <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> para cifrar la memoria de datos. {{site.data.keyword.datashield_short}} se integra con la tecnología Intel® Software Guard Extensions (SGX) y la tecnología Fortanix® para que el código de carga de trabajo del contenedor de {{site.data.keyword.Bluemix_notm}} esté protegido mientras se utiliza. El código de la app y los datos se ejecutan en enclaves de CPU, que son áreas de confianza de la memoria en el nodo trabajador que protegen aspectos críticos de la app, lo que ayuda a mantener la confidencialidad del código y de los datos y evita su modificación.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} File Storage</td>
<td>Almacenamiento de archivos</td>
<td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) es un almacenamiento de archivos basado en NFS persistente, rápido, flexible y conectado a la red que puede añadir a sus apps mediante volúmenes persistentes de Kubernetes. Puede elegir los niveles de almacenamiento predefinidos con tamaños de GB e IOPS que cumplan los requisitos de sus cargas de trabajo. Para obtener más información acerca de cómo suministrar almacenamiento de archivos en el clúster, consulte [Almacenamiento de datos en {{site.data.keyword.Bluemix_notm}} File Storage](/docs/containers?topic=containers-file_storage#file_storage).</td>
</tr>
<tr>
<td>{{site.data.keyword.keymanagementservicefull}}</td>
<td>Cifrado de datos</td>
<td>Cifre los secretos de Kubernetes que se encuentran en el clúster habilitando {{site.data.keyword.keymanagementserviceshort}}. El cifrado de los secretos de Kubernetes evita que los usuarios no autorizados accedan a la información confidencial del clúster.<br>Para configurarlo, consulte <a href="/docs/containers?topic=containers-encryption#keyprotect">Cifrado de secretos de Kubernetes mediante {{site.data.keyword.keymanagementserviceshort}}</a>.<br>Para obtener más información, consulte la <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">documentación de {{site.data.keyword.keymanagementserviceshort}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full}}</td>
<td>Registros de clúster y de app</td>
<td>Añada prestaciones de gestión de registros al clúster desplegando LogDNA como servicio de terceros en sus nodos trabajadores para gestionar registros de sus contenedores de pod. Para obtener más información, consulte [Gestión de registros de clúster de Kubernetes con {{site.data.keyword.loganalysisfull_notm}} con LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full}}</td>
<td>Métricas de clúster y de app</td>
<td>Obtenga visibilidad operativa sobre el rendimiento y el estado de las apps mediante el despliegue de Sysdig como servicio de terceros en sus nodos trabajadores para reenviar métricas a {{site.data.keyword.monitoringlong}}. Para obtener más información, consulte [Análisis de métricas para una app desplegada en un clúster de Kubernetes](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). </td>
</tr>
<tr>
<td>{{site.data.keyword.cos_full}}</td>
<td>Almacenamiento de objetos</td>
<td>Los datos que se almacenan con {{site.data.keyword.cos_short}} están cifrados y se dispersan entre varias ubicaciones geográficas. Se accede a estos datos sobre HTTP utilizando una API REST. Utilice [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) para configurar el servicio y hacer copias de seguridad puntuales o planificadas de los datos en los clústeres. Para obtener más información sobre el servicio, consulte la <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">documentación de {{site.data.keyword.cos_short}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.</td>
</tr>
<tr>
<td>Istio on {{site.data.keyword.containerlong_notm}}</td>
<td>Gestión de microservicios</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> es un servicio de código fuente abierto que ofrece a los desarrolladores una forma de conectarse, proteger, gestionar y supervisar una red de microservicios, también conocida como malla de servicios, en plataformas de orquestación de nube. Istio on {{site.data.keyword.containerlong}} proporciona un proceso de instalación en un paso de Istio en el clúster mediante un complemento gestionado. Con una sola pulsación, puede obtener todos los componentes principales de Istio, rastreo adicional, supervisión y visualización, y tener la app de ejemplo BookInfo activa y en ejecución. Para empezar, consulte [Utilización del complemento de Istio gestionado (beta)](/docs/containers?topic=containers-istio).</td>
</tr>
<tr>
<td>Knative</td>
<td>Apps sin servidor</td>
<td>[Knative ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/knative/docs) es una plataforma de código abierto que ha sido desarrollada por IBM, Google, Pivotal, Red Hat, Cisco y otros con el objetivo de ampliar las prestaciones de Kubernetes para ayudarle a crear apps modernas, centradas en contenedores y sin servidor sobre su clúster de Kubernetes. La plataforma utiliza un enfoque coherente entre lenguajes de programación e infraestructuras para facilitar la carga operativa derivada de crear, desplegar y gestionar cargas de trabajo en Kubernetes de modo que los desarrolladores puedan centrarse en lo que más les importa: el código fuente. Para obtener más información, consulte [Despliegue de apps sin servidor con Knative](/docs/containers?topic=containers-serverless-apps-knative). </td>
</tr>
<tr>
<td>Portworx</td>
<td>Almacenamiento para apps con estado</td>
<td>[Portworx ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://portworx.com/products/introduction/) es una solución de almacenamiento definida por software de alta disponibilidad que puede utilizar para gestionar el almacenamiento persistente para bases de datos contenerizadas y otras apps con estado, o para compartir datos entre pods de varias zonas. Puede instalar Portworx con un diagrama de Helm y suministrar almacenamiento para las apps mediante volúmenes persistentes de Kubernetes. Para obtener más información sobre cómo configurar Portworx en el clúster, consulte [Almacenamiento de datos en almacenamiento definido por software (SDS) con Portworx](/docs/containers?topic=containers-portworx#portworx).</td>
</tr>
<tr>
<td>Razee</td>
<td>Automatización de despliegues</td>
<td>[Razee ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://razee.io/) es un proyecto de código abierto que automatiza y gestiona el despliegue de recursos de Kubernetes en clústeres, entornos y proveedores de nube y le ayuda a visualizar información sobre el despliegue de sus recursos para que pueda supervisar el proceso de despliegue y detectar problemas con mayor rapidez. Para obtener más información sobre Razee y sobre cómo configurar Razee en el clúster para automatizar el proceso de despliegue, consulte la [documentación de Razee ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/razee-io/Razee).</td>
</tr>
</tbody>
</table>

<br />


## Servicios de DevOps
{: #devops_services}

<table summary="La tabla muestra los servicios disponibles que puede añadir a su clúster para añadir más prestaciones de DevOps. Las filas se leen de izquierda a derecha, con el nombre del servicio en la columna uno y una descripción del servicio en la columna dos.">
<caption>Servicios de DevOps</caption>
<thead>
<tr>
<th>Servicio</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cfee_full_notm}}</td>
<td>Despliegue y gestione su propia plataforma Cloud Foundry en la parte superior de un clúster de Kubernetes para desarrollar, empaquetar, desplegar y gestionar apps nativas de la nube y aprovechar el ecosistema de {{site.data.keyword.Bluemix_notm}} para enlazar servicios adicionales a las apps. Cuando cree una instancia de {{site.data.keyword.cfee_full_notm}}, debe configurar el clúster de Kubernetes eligiendo el tipo de máquina y las VLAN para los nodos trabajadores. A continuación, el clúster se suministrará con {{site.data.keyword.containerlong_notm}} y {{site.data.keyword.cfee_full_notm}} se desplegará automáticamente en el clúster. Para obtener más información sobre cómo configurar {{site.data.keyword.cfee_full_notm}}, consulte la [Guía de aprendizaje de iniciación](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started). </td>
</tr>
<tr>
<td>Codeship</td>
<td>Puede utilizar <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> para la integración y entrega continua de contenedores. Para obtener más información, consulte <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Utilización de Codeship Pro para desplegar cargas de trabajo en {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. </td>
</tr>
<tr>
<td>Grafeas</td>
<td>[Grafeas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://grafeas.io) es un servicio CI/CD de código abierto que proporciona un método común para recuperar, almacenar e intercambiar metadatos durante el proceso de la cadena de suministro de software. Por ejemplo, si integra Grafeas en el proceso de compilación de apps, Grafeas puede almacenar información sobre el iniciador de la solicitud de compilación, los resultados de la exploración de vulnerabilidades y el proceso de garantía de calidad para que pueda tomar una decisión informada sobre si se puede desplegar una app en producción. Puede utilizar estos metadatos en auditorías o para probar la conformidad de la cadena de suministro de software. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> es un gestor de paquetes de Kubernetes. Puede crear nuevos diagramas de Helm o utilizar diagramas de Helm preexistentes para definir, instalar y actualizar utilizar aplicaciones de Kubernetes complejas que se ejecutan en clústeres de {{site.data.keyword.containerlong_notm}}. <p>Para obtener más información, consulte [Configuración de Helm en {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automatice las compilaciones de las apps y los despliegues de los contenedores en clústeres de Kubernetes utilizando a cadena de herramientas. Para obtener más información sobre la configuración, consulte el blog sobre <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Despliegue de pods de Kubernetes en {{site.data.keyword.containerlong_notm}} mediante conductos de DevOps <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. </td>
</tr>
<tr>
<td>Istio on {{site.data.keyword.containerlong_notm}}</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> es un servicio de código fuente abierto que ofrece a los desarrolladores una forma de conectarse, proteger, gestionar y supervisar una red de microservicios, también conocida como malla de servicios, en plataformas de orquestación de nube. Istio on {{site.data.keyword.containerlong}} proporciona un proceso de instalación en un paso de Istio en el clúster mediante un complemento gestionado. Con una sola pulsación, puede obtener todos los componentes principales de Istio, rastreo adicional, supervisión y visualización, y tener la app de ejemplo BookInfo activa y en ejecución. Para empezar, consulte [Utilización del complemento de Istio gestionado (beta)](/docs/containers?topic=containers-istio).</td>
</tr>
<tr>
<td>Jenkins X</td>
<td>Jenkins X es una integración continua nativa de Kubernetes y una plataforma de entrega continua que se puede utilizar para automatizar el proceso de compilación. Para obtener más información sobre cómo instalarlo en {{site.data.keyword.containerlong_notm}}, consulte [Introducción al proyecto de código abierto Jenkins X ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/bluemix/2018/08/installing-jenkins-x-on-ibm-cloud-kubernetes-service/).</td>
</tr>
<tr>
<td>Knative</td>
<td>[Knative ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/knative/docs) es una plataforma de código abierto que ha sido desarrollada por IBM, Google, Pivotal, Red Hat, Cisco y otros con el objetivo de ampliar las prestaciones de Kubernetes para ayudarle a crear apps modernas, centradas en contenedores y sin servidor sobre su clúster de Kubernetes. La plataforma utiliza un enfoque coherente entre lenguajes de programación e infraestructuras para facilitar la carga operativa derivada de crear, desplegar y gestionar cargas de trabajo en Kubernetes de modo que los desarrolladores puedan centrarse en lo que más les importa: el código fuente. Para obtener más información, consulte [Despliegue de apps sin servidor con Knative](/docs/containers?topic=containers-serverless-apps-knative). </td>
</tr>
<tr>
<td>Razee</td>
<td>[Razee ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://razee.io/) es un proyecto de código abierto que automatiza y gestiona el despliegue de recursos de Kubernetes en clústeres, entornos y proveedores de nube y le ayuda a visualizar información sobre el despliegue de sus recursos para que pueda supervisar el proceso de despliegue y detectar problemas con mayor rapidez. Para obtener más información sobre Razee y sobre cómo configurar Razee en el clúster para automatizar el proceso de despliegue, consulte la [documentación de Razee ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/razee-io/Razee).</td>
</tr>
</tbody>
</table>

<br />


## Servicios de nube híbrida
{: #hybrid_cloud_services}

<table summary="En la tabla se muestran los servicios disponibles que puede utilizar para conectar su clúster a centros de datos locales. Las filas se leen de izquierda a derecha, con el nombre del servicio en la columna uno y una descripción del servicio en la columna dos.">
<caption>Servicios de nube híbrida</caption>
<thead>
<tr>
<th>Servicio</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.BluDirectLink}}</td>
    <td>[{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link) le permite crear una conexión privada directa entre entornos de red remotos y {{site.data.keyword.containerlong_notm}} sin tener que direccionar sobre Internet público. Las ofertas de {{site.data.keyword.Bluemix_notm}} Direct Link resultan útiles cuando se deben implementar cargas de trabajo híbridas, cargas de trabajo entre proveedores, transferencias de datos grandes o frecuentes o cargas de trabajo privadas. Para elegir una conexión de {{site.data.keyword.Bluemix_notm}} Direct Link y configurar una conexión de {{site.data.keyword.Bluemix_notm}} Direct Link, consulte [Iniciación a {{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-) en la documentación de {{site.data.keyword.Bluemix_notm}} Direct Link.</td>
  </tr>
<tr>
  <td>Servicio VPN IPSec de strongSwan</td>
  <td>Configure un [servicio VPN IPSec de strongSwan ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.strongswan.org/about.html) que se conecte de forma segura al clúster de Kubernetes con una red local. El servicio VPN IPSec de strongSwan proporciona un canal de comunicaciones de extremo a extremo seguro sobre Internet que está basado en la suite de protocolos
Internet Protocol Security (IPSec) estándar del sector. Para configurar una conexión segura entre el clúster y una red local, [configure y despliegue el servicio VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) directamente en un pod del clúster.</td>
  </tr>
  </tbody>
  </table>

<br />


## Servicios de registro y supervisión
{: #health_services}
<table summary="La tabla muestra los servicios disponibles que puede añadir al clúster para agregar funciones adicionales de registro y supervisión. Las filas se leen de izquierda a derecha; la columna un contiene el nombre del servicio y la dos contiene la descripción del servicio.">
<caption>Servicios de registro y supervisión</caption>
<thead>
<tr>
<th>Servicio</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>Supervise los nodos trabajadores, contenedores, conjuntos de réplicas, controladores de réplicas y servicios con <a href="https://www.newrelic.com/coscale" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. Para obtener más información, consulte <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Supervisión de {{site.data.keyword.containerlong_notm}} con CoScale <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. </td>
</tr>
<tr>
<td>Datadog</td>
<td>Supervise el clúster y visualice las métricas de rendimiento de las aplicaciones y la infraestructura con <a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. Para obtener más información, consulte <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Supervisión de {{site.data.keyword.containerlong_notm}} con Datadog <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Supervise la actividad administrativa realizada en el clúster mediante el análisis de registros a través de Grafana. Para obtener más información sobre el servicio, consulte la documentación de [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started). Para obtener más información sobre los tipos de sucesos de los que puede realizar un seguimiento, consulte [sucesos de Activity Tracker](/docs/containers?topic=containers-at_events).</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>Añada prestaciones de gestión de registros al clúster desplegando LogDNA como servicio de terceros en sus nodos trabajadores para gestionar registros de sus contenedores de pod. Para obtener más información, consulte [Gestión de registros de clúster de Kubernetes con {{site.data.keyword.loganalysisfull_notm}} con LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>Obtenga visibilidad operativa sobre el rendimiento y el estado de las apps mediante el despliegue de Sysdig como servicio de terceros en sus nodos trabajadores para reenviar métricas a {{site.data.keyword.monitoringlong}}. Para obtener más información, consulte [Análisis de métricas para una app desplegada en un clúster de Kubernetes](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> proporciona la infraestructura y la supervisión de rendimiento de app con una GUI que detecta y correlaciona automáticamente las apps. Instana captura las solicitudes de las apps, que puede utilizar para resolver problemas y realizar el análisis de la causa raíz para evitar que se repitan los problemas. Consulte la publicación sobre <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">despliegue de Instana en {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> para obtener más información.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus es una herramienta de supervisión, registro y generación de alertas diseñada para Kubernetes. Prometheus recupera información detallada acerca del clúster, los nodos trabajadores y el estado de despliegue basado en la información de registro de Kubernetes. Para cada contenedor en ejecución en el clúster, se recopila actividad de CPU, memoria, E/S y red. Los datos recopilados se pueden utilizar en consultas personalizadas o en alertas para supervisar el rendimiento y las cargas de trabajo del clúster.

<p>Para utilizar Prometheus, siga las <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">instrucciones de CoreOS <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>Visualice métricas y registros para sus aplicaciones contenerizadas utilizando <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. Para obtener más información, consulte <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Supervisión y registro de contenedores con Sematext <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. </td>
</tr>
<tr>
<td>Splunk</td>
<td>Importe y busque los datos de registro, de objetos y de métricas de Kubernetes en Splunk mediante Splunk Connect for Kubernetes. Splunk Connect for Kubernetes es una colección de diagramas de Helm que despliegan un despliegue soportado por Splunk de Fluentd en el clúster de Kubernetes, un plugin HEC (Fluentd HTTP Event Collector) creado por Splunk para enviar registros y metadatos y un despliegue de métricas que captura las métricas de clúster. Para obtener más información, consulte <a href="https://www.ibm.com/blogs/bluemix/2019/02/solving-business-problems-with-splunk-on-ibm-cloud-kubernetes-service/" target="_blank">Resolución de problemas de la empresa con Splunk en {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.</td>
</tr>
<tr>
<td>Weave Scope</td>
<td>[Weave Scope ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.weave.works/oss/scope/) proporciona un diagrama visual de los recursos de un clúster de Kubernetes, incluidos servicios, pods, contenedores, procesos, nodos, etc. Weave Scope ofrece métricas interactivas correspondientes a CPU y memoria y también herramientas para realizar seguimientos y ejecuciones en un contenedor.</li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## Servicios de seguridad
{: #security_services}

¿Desea obtener una visión completa de cómo integrar los servicios de seguridad de {{site.data.keyword.Bluemix_notm}} en su clúster? Consulte la [guía de aprendizaje sobre cómo aplicar seguridad de extremo a extremo a una aplicación de nube](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security).
{: shortdesc}

<table summary="La tabla muestra los servicios disponibles que puede añadir al clúster para agregar funciones adicionales de seguridad. Las filas se leen de izquierda a derecha; la columna uno contiene el nombre del servicio y la dos una descripción del servicio.">
<caption>Servicios de seguridad</caption>
<thead>
<tr>
<th>Servicio</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.appid_full}}</td>
    <td>Añada un nivel de seguridad a sus apps con [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) requiriendo a los usuarios que inicien sesión. Para autenticar solicitudes HTTP/HTTPS de API o web para la app, puede integrar {{site.data.keyword.appid_short_notm}} con el servicio Ingress mediante la [anotación de Ingress de autenticación de {{site.data.keyword.appid_short_notm}}](/docs/containers?topic=containers-ingress_annotation#appid-auth).</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>Como suplemento a <a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a>, puede utilizar <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> para mejorar la seguridad de los despliegues de contenedores reduciendo las acciones que su app puede realizar. Para obtener más información, consulte <a href="https://blog.aquasec.com/securing-container-deployments-on-bluemix-with-aqua-security" target="_blank">Protección de despliegues de contenedores en {{site.data.keyword.Bluemix_notm}} con Aqua Security <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>Puede utilizar <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> para almacenar y gestionar certificados SSL para sus apps. Para obtener más información, consulte <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Uso de {{site.data.keyword.cloudcerts_long_notm}} con {{site.data.keyword.containerlong_notm}} para desplegar certificados TLS de dominio personalizados <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. </td>
</tr>
<tr>
  <td>{{site.data.keyword.datashield_full}} (Beta)</td>
  <td>Puede utilizar <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> para cifrar la memoria de datos. {{site.data.keyword.datashield_short}} se integra con la tecnología Intel® Software Guard Extensions (SGX) y la tecnología Fortanix® para que el código de carga de trabajo del contenedor de {{site.data.keyword.Bluemix_notm}} esté protegido mientras se utiliza. El código de la app y los datos se ejecutan en enclaves de CPU, que son áreas de confianza de la memoria en el nodo trabajador que protegen aspectos críticos de la app, lo que ayuda a mantener la confidencialidad del código y de los datos y evita su modificación.</td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>Configure su propio repositorio de imágenes de Docker protegidas para almacenar y compartir de forma segura las imágenes entre los usuarios del clúster. Para obtener más información, consulte la <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">documentación de {{site.data.keyword.registrylong}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.keymanagementservicefull}}</td>
  <td>Cifre los secretos de Kubernetes que se encuentran en el clúster habilitando {{site.data.keyword.keymanagementserviceshort}}. El cifrado de los secretos de Kubernetes evita que los usuarios no autorizados accedan a la información confidencial del clúster.<br>Para configurarlo, consulte <a href="/docs/containers?topic=containers-encryption#keyprotect">Cifrado de secretos de Kubernetes mediante {{site.data.keyword.keymanagementserviceshort}}</a>.<br>Para obtener más información, consulte la <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">documentación de {{site.data.keyword.keymanagementserviceshort}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.</td>
</tr>
<tr>
<td>NeuVector</td>
<td>Proteja los contenedores con un cortafuegos nativo en la nube utilizando <a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. Para obtener más información, consulte <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. </td>
</tr>
<tr>
<td>Twistlock</td>
<td>Como suplemento a <a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a>, puede utilizar <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> para gestionar cortafuegos, la protección ante amenazas y la respuesta a incidentes. Para obtener más información, consulte <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock en {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Servicios de almacenamiento
{: #storage_services}
<table summary="La tabla muestra los servicios disponibles que puede añadir al clúster para agregar funciones adicionales de almacenamiento persistente. Las filas se leen de izquierda a derecha; la columna uno contiene el nombre del servicio y la dos una descripción del servicio.">
<caption>Servicios de almacenamiento</caption>
<thead>
<tr>
<th>Servicio</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Velero</td>
  <td>Utilice <a href="https://github.com/heptio/velero" target="_blank">Heptio Velero <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> para hacer copias de seguridad y restauración de volúmenes persistentes y recursos de clúster. Para obtener más información, consulte <a href="https://github.com/heptio/velero/blob/release-0.9/docs/use-cases.md" target="_blank">Casos de uso para migración de clústeres y recuperación ante desastres <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> de Heptio Velero.</td>
</tr>
<tr>
  <td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) es un almacenamiento iSCSI persistente y de alto rendimiento que puede añadir a las apps mediante volúmenes persistentes (PV) de Kubernetes. Utilice el almacenamiento en bloque para desplegar apps con estado en una sola zona o como almacenamiento de alto rendimiento para pods individuales. Para obtener más información sobre cómo suministrar almacenamiento en bloque en el clúster, consulte [Almacenamiento de datos en {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/containers?topic=containers-block_storage#block_storage)</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>Los datos que se almacenan con {{site.data.keyword.cos_short}} están cifrados y se dispersan entre varias ubicaciones geográficas. Se accede a estos datos sobre HTTP utilizando una API REST. Utilice [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) para configurar el servicio y hacer copias de seguridad puntuales o planificadas de los datos en los clústeres. Para obtener más información sobre el servicio, consulte la <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">documentación de {{site.data.keyword.cos_short}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.</td>
</tr>
  <tr>
  <td>{{site.data.keyword.Bluemix_notm}} File Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) es un almacenamiento de archivos basado en NFS persistente, rápido, flexible y conectado a la red que puede añadir a sus apps mediante volúmenes persistentes de Kubernetes. Puede elegir los niveles de almacenamiento predefinidos con tamaños de GB e IOPS que cumplan los requisitos de sus cargas de trabajo. Para obtener más información acerca de cómo suministrar almacenamiento de archivos en el clúster, consulte [Almacenamiento de datos en {{site.data.keyword.Bluemix_notm}} File Storage](/docs/containers?topic=containers-file_storage#file_storage).</td>
  </tr>
  <tr>
    <td>Portworx</td>
    <td>[Portworx ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://portworx.com/products/introduction/) es una solución de almacenamiento definida por software de alta disponibilidad que puede utilizar para gestionar el almacenamiento persistente para bases de datos contenerizadas y otras apps con estado, o para compartir datos entre pods de varias zonas. Puede instalar Portworx con un diagrama de Helm y suministrar almacenamiento para las apps mediante volúmenes persistentes de Kubernetes. Para obtener más información sobre cómo configurar Portworx en el clúster, consulte [Almacenamiento de datos en almacenamiento definido por software (SDS) con Portworx](/docs/containers?topic=containers-portworx#portworx).</td>
  </tr>
</tbody>
</table>

<br />


## Servicios de base de datos
{: #database_services}

<table summary="La tabla muestra los servicios disponibles que puede añadir al clúster para agregar funciones de bases de datos. Las filas se leen de izquierda a derecha; la columna uno contiene el nombre del servicio y la dos una descripción del servicio.">
<caption>Servicios de base de datos</caption>
<thead>
<tr>
<th>Servicio</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.blockchainfull_notm}} Platform 2.0 beta</td>
    <td>Despliegue y gestione su propia plataforma {{site.data.keyword.blockchainfull_notm}} en {{site.data.keyword.containerlong_notm}}. Con {{site.data.keyword.blockchainfull_notm}} Platform 2.0, puede alojar redes de {{site.data.keyword.blockchainfull_notm}} o crear organizaciones que puedan unirse a otras redes de {{site.data.keyword.blockchainfull_notm}} 2.0. Para obtener más información sobre cómo configurar {{site.data.keyword.blockchainfull_notm}} en {{site.data.keyword.containerlong_notm}}, consulte [Acerca de {{site.data.keyword.blockchainfull_notm}} Platform gratuita 2.0 beta](/docs/services/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview).</td>
  </tr>
<tr>
  <td>Bases de datos en la nube</td>
  <td>Puede elegir entre diversos servicios de base de datos de {{site.data.keyword.Bluemix_notm}}, como por ejemplo {{site.data.keyword.composeForMongoDB_full}} o {{site.data.keyword.cloudantfull}}, para desplegar soluciones de base de datos escalables y altamente disponibles en el clúster. Para ver una lista completa de bases de datos en la nube, consulte el [catálogo de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/catalog?category=databases).  </td>
  </tr>
  </tbody>
  </table>
