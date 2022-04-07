---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-09"

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


# Integración de servicios
{: #integrations}

Puede utilizar diversos servicios externos y servicios del catálogo con un clúster de Kubernetes estándar en {{site.data.keyword.containerlong}}.
{:shortdesc}


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
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> es un gestor de paquetes de Kubernetes. Puede crear nuevos diagramas de Helm o utilizar diagramas de Helm preexistentes para definir, instalar y actualizar utilizar aplicaciones de Kubernetes complejas que se ejecutan en clústeres de {{site.data.keyword.containerlong_notm}}. <p>Para obtener más información, consulte [Configuración de Helm en {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-integrations#helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automatice las compilaciones de las apps y los despliegues de los contenedores en clústeres de Kubernetes utilizando a cadena de herramientas. Para obtener información sobre la configuración, consulte el blog sobre <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Despliegue de pods de Kubernetes en {{site.data.keyword.containerlong_notm}} mediante conductos DevOps <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. </td>
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
<td>[Knative ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/knative/docs) es una plataforma de código abierto que ha sido desarrollada por IBM, Google, Pivotal, Red Hat, Cisco y otros con el objetivo de ampliar las prestaciones de Kubernetes para ayudarle a crear apps modernas, centradas en contenedores y sin servidor sobre su clúster de Kubernetes. La plataforma utiliza un enfoque coherente entre lenguajes de programación e infraestructuras para facilitar la carga operativa derivada de crear, desplegar y gestionar cargas de trabajo en Kubernetes de modo que los desarrolladores puedan centrarse en lo que más les importa: el código fuente. Para obtener más información, consulte la [Guía de aprendizaje: Utilización de Knative gestionado para ejecutar apps sin servidor en clústeres de Kubernetes](/docs/containers?topic=containers-knative_tutorial#knative_tutorial). </td>
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
<td>Supervise la actividad administrativa realizada en el clúster mediante el análisis de registros a través de Grafana. Para obtener más información sobre el servicio, consulte la documentación de [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla). Para obtener más información sobre los tipos de sucesos de los que puede realizar un seguimiento, consulte [sucesos de Activity Tracker](/docs/containers?topic=containers-at_events).</td>
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
<td>Prometheus es una herramienta de supervisión, registro y generación de alertas diseñada específicamente para Kubernetes. Prometheus recupera información detallada acerca del clúster, los nodos trabajadores y el estado de despliegue basado en la información de registro de Kubernetes. Para cada contenedor en ejecución en el clúster se recopila actividad de CPU, memoria, E/S y red. Los datos recopilados se pueden utilizar en consultas personalizadas o en alertas para supervisar el rendimiento y las cargas de trabajo del clúster.

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
<td>Weave Scope proporciona un diagrama visual de los recursos de un clúster de Kubernetes, incluidos servicios, pods, contenedores, procesos, nodos, etc. Weave Scope ofrece métricas interactivas correspondientes a CPU y memoria y también herramientas para realizar seguimientos y ejecuciones en un contenedor.<p>Para obtener más información, consulte [Visualización de recursos de clúster de Kubernetes con Weave Scope y {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-integrations#weavescope).</p></li></ol>
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
  <tr id="appid">
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
  <td>Los datos que se almacenan con {{site.data.keyword.cos_short}} están cifrados y se dispersan entre varias ubicaciones geográficas. Se accede a estos datos sobre HTTP utilizando una API REST. Utilice [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) para configurar el servicio y hacer copias de seguridad puntuales o planificadas de los datos en los clústeres. Para obtener información general sobre el servicio, consulte la <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage" target="_blank">documentación de {{site.data.keyword.cos_short}} <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.</td>
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
  <td>Puede elegir entre una variedad de servicios de base de datos de {{site.data.keyword.Bluemix_notm}}, como por ejemplo {{site.data.keyword.composeForMongoDB_full}} o {{site.data.keyword.cloudantfull}}, para desplegar soluciones de base de datos escalables y altamente disponibles en el clúster. Para ver una lista completa de bases de datos en la nube, consulte el [catálogo de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/catalog?category=databases).  </td>
  </tr>
  </tbody>
  </table>


## Adición de servicios de {{site.data.keyword.Bluemix_notm}} a clústeres
{: #adding_cluster}

Añada servicios de {{site.data.keyword.Bluemix_notm}} para mejorar el clúster de Kubernetes con prestaciones adicionales en áreas como, por ejemplo, Watson AI, datos, seguridad e Internet de las cosas (IoT).
{:shortdesc}

Sólo puede enlazar servicios que admitan claves de servicio. Para consultar una lista de servicios que admiten claves de servicio, consulte [Habilitación de apps externas para utilizar servicios de {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).
{: note}

Antes de empezar:
- Asegúrese de que tiene los roles siguientes:
    - [Rol de servicio de {{site.data.keyword.Bluemix_notm}} IAM **Editor** o **Administrador**](/docs/containers?topic=containers-users#platform) para el clúster.
    - [Rol de servicio de {{site.data.keyword.Bluemix_notm}} IAM de **Escritor** o de **Gestor**](/docs/containers?topic=containers-users#platform) sobre el espacio de nombres al que quiere vincular el servicio
    - El [rol **Desarrollador** de Cloud Foundry](/docs/iam?topic=iam-mngcf#mngcf) para el espacio que desea utilizar
- [Inicie una sesión en su cuenta. Elija como destino la región adecuada y, si procede, el grupo de recursos. Establezca el contexto para el clúster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Para añadir un servicio {{site.data.keyword.Bluemix_notm}} al clúster:

1. [Cree una instancia del servicio {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).
    * Algunos servicios de {{site.data.keyword.Bluemix_notm}} solo están disponibles en determinadas regiones. Puede enlazar un servicio con el clúster sólo si el servicio está disponible en la misma región que el clúster. Además, si desea crear una instancia de servicio en la zona de Washington DC, debe utilizar la CLI.
    * Debe crear la instancia de servicio en el mismo grupo de recursos que el clúster. Un recurso solo se puede crear en un grupo de recursos que no se puede cambiar después.

2. Compruebe el tipo de servicio que ha creado y tome nota del **Nombre** de la instancia de servicio.
   - **Servicios de Cloud Foundry:**
     ```
     ibmcloud service list
     ```
     {: pre}

     Salida de ejemplo:
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **Servicios habilitados para {{site.data.keyword.Bluemix_notm}} IAM:**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}

     Salida de ejemplo:
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   También puede ver los distintos tipos de servicio en el panel de control como **Servicios de Cloud Foundry** y **Servicios**.

3. Identifique el espacio de nombres del clúster que desea utilizar para añadir el servicio. Puede elegir entre las siguientes opciones.
     ```
     kubectl get namespaces
     ```
     {: pre}

4.  Añada el servicio al clúster con el [mandato `ibmcloud ks cluster-service-bind`](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_bind). Para los servicios habilitados para {{site.data.keyword.Bluemix_notm}} IAM, asegúrese de utilizar el alias de Cloud Foundry que ha creado anteriormente. Para los servicios habilitados para IAM, también puede utilizar el rol de acceso al servicio de **Escritor** predeterminado, o bien puede especificar el rol de acceso al servicio con el distintivo `--role`. El mandato crea una clave de servicio para la instancia de servicio, o bien puede incluir el distintivo `--key` para utilizar las credenciales de clave de servicio existentes. Si utiliza el distintivo `--key`, no incluya el distintivo `--role`.
    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
    ```
    {: pre}

    Cuando el servicio se haya añadido correctamente al clúster, se crea un secreto de clúster que contiene las credenciales de la instancia de servicio. Los secretos se cifran automáticamente en etcd para proteger los datos.

    Salida de ejemplo:
    ```
    ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

5.  Verifique las credenciales de servicio en el secreto de Kubernetes.
    1. Obtenga los detalles del secreto y anote el valor de **binding**. El valor de **binding** está codificado en base64 y contiene las credenciales para la instancia de servicio en formato JSON.
       ```
       kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
       ```
       {: pre}

       Salida de ejemplo:
       ```
       apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
       ```
       {: screen}

    2. Descodifique el valor de binding.
       ```
       echo "<binding>" | base64 -D
       ```
       {: pre}

       Salida de ejemplo:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    3. Opcional: compare las credenciales de servicio que ha descodificado en el paso anterior con las credenciales de servicio que encuentre para la instancia de servicio en el panel de control de {{site.data.keyword.Bluemix_notm}}.

6. Ahora que el servicio está enlazado con el clúster, debe configurar la app para que [acceda a las credenciales de servicio en el secreto de Kubernetes](#adding_app).


## Acceso a las credenciales de servicio desde las apps
{: #adding_app}

Para acceder a una instancia de servicio de {{site.data.keyword.Bluemix_notm}} desde la app, debe permitir que la app acceda a las credenciales de servicio almacenadas en el secreto de Kubernetes.
{: shortdesc}

Las credenciales de una instancia de servicio están codificadas como base64 y se almacenan en el secreto en formato JSON. Para acceder a los datos del secreto, elija una de las opciones siguientes:
- [Monte el secreto como un volumen en el pod](#mount_secret)
- [Haga referencia al secreto en las variables de entorno](#reference_secret)
<br>
¿Desea proteger aún más sus secretos? Solicite al administrador del clúster que [habilite {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) en el clúster para cifrar los secretos nuevos y existentes, como el secreto que almacena las credenciales de las instancias de servicio de {{site.data.keyword.Bluemix_notm}}.
{: tip}

Antes de empezar:
-  Asegúrese de que tiene el [rol de servicio **Escritor** o **Gestor** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre el espacio de nombres `kube-system`.
- [Inicie una sesión en su cuenta. Elija como destino la región adecuada y, si procede, el grupo de recursos. Establezca el contexto para el clúster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- [Añada un servicio de {{site.data.keyword.Bluemix_notm}} a su clúster](#adding_cluster).

### Montaje del secreto como un volumen en el pod
{: #mount_secret}

Cuando monta el secreto como volumen en el pod, un archivo denominado `binding` se almacena en el directorio de montaje del volumen. El archivo `binding` en formato JSON incluye toda la información y las credenciales que necesita para acceder al servicio de {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

1.  Liste los secretos disponibles en el clúster y anote el **nombre** del secreto. Busque un secreto de tipo **opaco**. Si existen varios secretos, póngase en contacto con el administrador del clúster para identificar el secreto correcto del servicio.

    ```
    kubectl get secrets
    ```
    {: pre}

    Salida de ejemplo:

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m

    ```
    {: screen}

2.  Cree un archivo YAML para el despliegue de Kubernetes y monte el secreto como un volumen en el pod.
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: registry.bluemix.net/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>Visión general de los componentes del archivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>La vía de acceso absoluta del directorio en el que el que está montado el volumen dentro del contenedor.</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>El nombre del volumen que va a montar en el pod.</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>Los permisos de lectura y escritura en el secreto. Utilice `420` para establecer permisos de solo lectura. </td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>El nombre del secreto que ha anotado en el paso anterior.</td>
    </tr></tbody></table>

3.  Cree el pod y monte el secreto como un volumen.
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  Verifique que se ha creado el pod.
    ```
    kubectl get pods
    ```
    {: pre}

    Ejemplo de salida de CLI:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  Acceda a las credenciales de servicio.
    1. Inicie una sesión en el pod.
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. Vaya a la vía de acceso de montaje de volumen que ha definido anteriormente y liste los archivos en la vía de acceso de montaje del volumen.
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       Salida de ejemplo:
       ```
       binding
       ```
       {: screen}

       El archivo `binding` incluye las credenciales de servicio que ha almacenado en el secreto de Kubernetes.

    4. Consulte las credenciales de servicio. Las credenciales se almacenan como pares de valor de clave en formato JSON.
       ```
       cat binding
       ```
       {: pre}

       Salida de ejemplo:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. Configure la app para analizar el contenido JSON y recuperar la información que necesita para acceder al servicio.


### Cómo hacer referencia al secreto en las variables de entorno
{: #reference_secret}

Puede añadir las credenciales de servicio y otros pares de valores de clave del secreto de Kubernetes como variables de entorno en el despliegue.
{: shortdesc}

1. Liste los secretos disponibles en el clúster y anote el **nombre** del secreto. Busque un secreto de tipo **opaco**. Si existen varios secretos, póngase en contacto con el administrador del clúster para identificar el secreto correcto del servicio.

    ```
    kubectl get secrets
    ```
    {: pre}

    Salida de ejemplo:

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
    ```
    {: screen}

2. Obtenga los detalles del secreto para encontrar pares potenciales de valores de clave a los que pueda hacer referencia como variables de entorno en el pod. Las credenciales de servicio se almacenan en la clave `binding` del secreto.
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   Salida de ejemplo:
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Cree un archivo YAML para el despliegue de Kubernetes y especifique una variable de entorno que haga referencia a la clave `binding`.
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: registry.bluemix.net/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>Visión general de los componentes del archivo YAML</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>El nombre de la variable de entorno.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>El nombre del secreto que ha anotado en el paso anterior.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>La clave que forma parte de su secreto y a la que desea hacer referencia en la variable de entorno. Para hacer referencia a las credenciales de servicio, debe utilizar la clave <strong>binding</strong>.  </td>
     </tr>
     </tbody></table>

4. Cree el pod que hace referencia a la clave `binding` del secreto como variable de entorno.
   ```
   kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. Verifique que se ha creado el pod.
   ```
   kubectl get pods
   ```
   {: pre}

   Ejemplo de salida de CLI:
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. Compruebe que la variable de entorno se haya establecido correctamente.
   1. Inicie una sesión en el pod.
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. Enumere todas las variables de entorno en el pod.
      ```
      env
      ```
      {: pre}

      Salida de ejemplo:
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. Configure la app para leer la variable de entorno y analizar el contenido JSON para recuperar la información que necesita para acceder al servicio.

   Código de ejemplo en Python:
   ```
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

## Configuración de Helm en {{site.data.keyword.containerlong_notm}}
{: #helm}

[Helm ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://helm.sh) es un gestor de paquetes de Kubernetes. Puede crear diagramas de Helm o utilizar diagramas de Helm preexistentes para definir, instalar y actualizar utilizar aplicaciones de Kubernetes complejas que se ejecuten en clústeres de {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Para desplegar diagramas de Helm, debe instalar la CLI de Helm en la máquina local e instalar el tiller del servidor de Helm en el clúster. La imagen de Tiller se almacena en el registro de contenedores de Google público. Para acceder a la imagen durante la instalación de Tiller, el clúster debe permitir la conectividad de red pública con el registro público de contenedores de Google. Los clústeres que tienen habilitado el punto final de servicio público puede acceder automáticamente a la imagen. Los clústeres privados que están protegidos con un cortafuegos personalizado, o clústeres que solo han habilitado el punto final de servicio privado, no permiten el acceso a la imagen de Tiller. Puede [extraer la imagen en su máquina local y enviar la imagen al espacio de nombres en {{site.data.keyword.registryshort_notm}}](#private_local_tiller), o bien [instalar los diagramas de Helm sin utilizar tiller](#private_install_without_tiller).
{: note}

### Configuración de Helm en un clúster con acceso público
{: #public_helm_install}

Si el clúster ha habilitado el punto final de servicio público, puede instalar Tiller utilizando la imagen pública del registro de contenedores de Google.
{: shortdesc}

Antes de empezar: [Inicie la sesión en su cuenta. Elija como destino la región adecuada y, si procede, el grupo de recursos. Establezca el contexto para el clúster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

1. Instale la <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI de Helm <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.

2. **Importante**: para mantener la seguridad del clúster, cree una cuenta de servicio para Tiller en el espacio de nombres `kube-system` y un enlace de rol de clúster RBAC de Kubernetes para el pod `tiller-deploy` mediante la aplicación del siguiente archivo `.yaml` desde el repositorio de [{{site.data.keyword.Bluemix_notm}} `kube-samples`](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml). **Nota**: Para instalar Tiller con la cuenta de servicio y el enlace de rol de clúster en el espacio de nombres `kube-system`, debe tener el [rol `cluster-admin`](/docs/containers?topic=containers-users#access_policies).
    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: pre}

3. Inicialice Helm e instale tiller con la cuenta de servicio que acaba de crear.

    ```
    helm init --service-account tiller
    ```
    {: pre}

4.  Compruebe que la instalación es correcta.
    1.  Verifique que se ha creado la cuenta de servicio de Tiller.
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

    2.  Verifique que el pod `tiller-deploy` tiene el **Estado** `En ejecución` en el clúster.
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        Salida de ejemplo:

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

5. Añada los repositorios de Helm de {{site.data.keyword.Bluemix_notm}} a la instancia de Helm.
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

6. Actualice los repositorios para recuperar las versiones más recientes de todos los diagramas de Helm.
   ```
   helm repo update
   ```
   {: pre}

7. Obtenga una lista de los diagramas de Helm disponibles actualmente en los repositorios de {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

8. Identifique el diagrama de Helm que desea instalar y siga las instrucciones del archivo `README` del diagrama de Helm para instalar el diagrama de Helm en el clúster.


### Clústeres privados: Envío de la imagen de Tiller al registro privado en {{site.data.keyword.registryshort_notm}}
{: #private_local_tiller}

Puede extraer la imagen de Tiller en la máquina local, enviarla a su espacio de nombres en {{site.data.keyword.registryshort_notm}} y dejar que Helm instale Tiller utilizando la imagen de {{site.data.keyword.registryshort_notm}}.
{: shortdesc}

Antes de empezar:
- Instale Docker en la máquina local. Si ha instalado la [CLI de {{site.data.keyword.Bluemix_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli), Docker ya está instalado.
- [Instale el plugin de la CLI de {{site.data.keyword.registryshort_notm}} y configure un espacio de nombres](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install).

Para instalar Tiller mediante {{site.data.keyword.registryshort_notm}}:

1. Instale la <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI de Helm <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> en la máquina local.
2. Conéctese a su clúster privado mediante el túnel VPN de la infraestructura de {{site.data.keyword.Bluemix_notm}} que ha configurado.
3. **Importante**: para mantener la seguridad del clúster, cree una cuenta de servicio para Tiller en el espacio de nombres `kube-system` y un enlace de rol de clúster RBAC de Kubernetes para el pod `tiller-deploy` mediante la aplicación del siguiente archivo `.yaml` desde el repositorio de [{{site.data.keyword.Bluemix_notm}} `kube-samples`](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml). **Nota**: Para instalar Tiller con la cuenta de servicio y el enlace de rol de clúster en el espacio de nombres `kube-system`, debe tener el [rol `cluster-admin`](/docs/containers?topic=containers-users#access_policies).
    1. [Obtenga la cuenta del servicio de Kubernetes y los archivos YAML de vinculación de roles del clúster ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml).

    2. Cree los recursos de Kubernetes en el clúster.
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [Localice la versión de Tiller ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30)] que desea instalar en el clúster. Si no necesita una versión específica, utilice la última.

5. Extraiga la imagen de Tiller en la máquina local.
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   Salida de ejemplo:
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [Envíe la imagen de Tiller al espacio de nombres en {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing).

7. [Copie el secreto de extracción de imágenes para acceder a {{site.data.keyword.registryshort_notm}} desde el espacio de nombres predeterminados al espacio de nombres `kube-system`](/docs/containers?topic=containers-images#copy_imagePullSecret).

8. Instale Tiller en el clúster privado mediante la imagen que ha almacenado en el espacio de nombres en {{site.data.keyword.registryshort_notm}}.
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. Añada los repositorios de Helm de {{site.data.keyword.Bluemix_notm}} a la instancia de Helm.
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

10. Actualice los repositorios para recuperar las versiones más recientes de todos los diagramas de Helm.
    ```
    helm repo update
    ```
    {: pre}

11. Obtenga una lista de los diagramas de Helm disponibles actualmente en los repositorios de {{site.data.keyword.Bluemix_notm}}.
    ```
    helm search ibm
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. Identifique el diagrama de Helm que desea instalar y siga las instrucciones del archivo `README` del diagrama de Helm para instalar el diagrama de Helm en el clúster.

### Clústeres privados: Instalación de diagramas de Helm sin utilizar Tiller
{: #private_install_without_tiller}

Si no desea instalar Tiller en el clúster privado, puede crear manualmente los archivos YAML del diagrama de Helm y aplicarlos mediante mandatos `kubectl`.
{: shortdesc}

En los pasos de este ejemplo se muestra cómo instalar diagramas de Helm desde los repositorios de diagramas de Helm de {{site.data.keyword.Bluemix_notm}} en el clúster privado. Si desea instalar un diagrama de Helm que no esté almacenado en uno de los repositorios de diagramas de Helm de {{site.data.keyword.Bluemix_notm}}, debe seguir las instrucciones de este tema para crear los archivos YAML para el diagrama de Helm. Además, debe descargar la imagen del diagrama de Helm del registro de contenedor público, enviarla al espacio de nombres de {{site.data.keyword.registryshort_notm}} y actualizar el archivo `values.yaml` para que utilice la imagen de {{site.data.keyword.registryshort_notm}}.
{: note}

1. Instale la <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI de Helm <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> en la máquina local.
2. Conéctese a su clúster privado mediante el túnel VPN de la infraestructura de {{site.data.keyword.Bluemix_notm}} que ha configurado.
3. Añada los repositorios de Helm de {{site.data.keyword.Bluemix_notm}} a la instancia de Helm.
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

4. Actualice los repositorios para recuperar las versiones más recientes de todos los diagramas de Helm.
   ```
   helm repo update
   ```
   {: pre}

5. Obtenga una lista de los diagramas de Helm disponibles actualmente en los repositorios de {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. Identifique el diagrama de Helm que desea instalar, descargue el diagrama de Helm en la máquina local y desempaquete los archivos de su diagrama de Helm. En el ejemplo siguiente se muestra cómo descargar el diagrama de Helm para el programa de escalado automático de clústeres 1.0.3 y desempaquetar los archivos en el directorio `cluster-autoscaler`.
   ```
   helm fetch ibm/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Vaya al directorio en el que ha desempaquetado los archivos del diagrama de Helm.
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Cree un directorio `output` para los archivos YAML que genere utilizando los archivos de su diagrama de Helm.
   ```
   mkdir output
   ```
   {: pre}

9. Abra el archivo `values.yaml` y realice los cambios necesarios según las instrucciones de instalación del diagrama de Helm.
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. Utilice la instalación de Helm local para crear todos los archivos YAML de Kubernetes para el diagrama de Helm. Los archivos YAML se almacenan en el directorio `output` que ha creado anteriormente.
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Salida de ejemplo:
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. Despliegue todos los archivos YAML en el clúster privado.
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. Opcional: Elimine todos los archivos YAML del directorio `output`.
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}


### Enlaces de Helm relacionados
{: #helm_links}

* Para utilizar el diagrama de Helm de strongSwan, consulte [Configuración de la conectividad de VPN con el diagrama de Helm del servicio VPN IPSec de strongSwan](/docs/containers?topic=containers-vpn#vpn-setup).
* Consulte los diagramas de Helm disponibles que puede utilizar con {{site.data.keyword.Bluemix_notm}} en [catálogo de diagramas de Helm ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) en la consola.
* Obtenga más información sobre los mandatos Helm que se utilizan para configurar y gestionar diagramas de Helm en la <a href="https://docs.helm.sh/helm/" target="_blank">documentación de Helm <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.
* Obtenga más información sobre cómo [aumentar la velocidad del despliegue con diagramas de Helm de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).

## Visualización de recursos de un clúster de Kubernetes
{: #weavescope}

Weave Scope proporciona un diagrama visual de los recursos de un clúster de Kubernetes, incluidos servicios, pods, contenedores, etc. Weave Scope ofrece métricas interactivas correspondientes a CPU y memoria y herramientas para realizar seguimientos y ejecuciones en un contenedor.
{:shortdesc}

Antes de empezar:

-   Recuerde no exponer la información del clúster en Internet público. Siga estos pasos para desplegar de forma segura Weave Scope y para acceder al mismo localmente desde un navegador web.
-   Si aún no tiene uno, [cree un clúster estándar](/docs/containers?topic=containers-clusters#clusters_ui). Weave Scope puede consumir mucha CPU, especialmente la app. Ejecute Weave Scope con clústeres de pago grandes, no con clústeres gratuitos.
-  Asegúrese de tener el [rol de servicio de {{site.data.keyword.Bluemix_notm}} IAM de **Gestor**](/docs/containers?topic=containers-users#platform) sobre todos los espacios de nombres.
-   [Inicie una sesión en su cuenta. Elija como destino la región adecuada y, si procede, el grupo de recursos. Establezca el contexto para el clúster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).


Para utilizar Weave Scope con una clúster:
2.  Despliegue una de los archivos de configuración de permisos RBAC del clúster.

    Para habilitar los permisos de lectura/escritura:

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    Para habilitar los permisos de solo lectura:

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    Salida:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Despliegue el servicio Weave Acope, al que puede acceder de forma privada con la dirección IP del clúster.

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    Salida:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  Ejecute un mandato de reenvío de puerto para que abrir el servicio en el sistema. La próxima vez que acceda a Weave Scope, podrá ejecutar este mandato sin realizar de nuevo los pasos de configuración anteriores.

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    Salida:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  Abra el navegador en `http://localhost:4040`. Sin los componentes predeterminados desplegados, verá el siguiente diagrama. Seleccione la vista de diagramas o tablas de topología de los recursos de Kubernetes en el clúster.

     <img src="images/weave_scope.png" alt="Ejemplo de topología de Weave Scope" style="width:357px;" />


[Más información sobre las características de Weave Scope ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.weave.works/docs/scope/latest/features/).

<br />

