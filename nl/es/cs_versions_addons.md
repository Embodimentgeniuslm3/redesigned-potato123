---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, nginx, ingress controller

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


# Registro de cambios de Fluentd y de ALB de Ingress
{: #cluster-add-ons-changelog}

El clúster de {{site.data.keyword.containerlong}} se suministra con componentes, como Fluentd y ALB de Ingress, que IBM actualiza automáticamente. También puede inhabilitar las actualizaciones automáticas de algunos componentes y actualizarlos manualmente de forma independiente de los nodos maestro y trabajador. Consulte las tablas de las secciones siguientes para ver un resumen de los cambios de cada versión.
{: shortdesc}

Para obtener más información sobre la gestión de actualizaciones para los ALB de Ingress y Fluentd, consulte [Actualización de los componentes de un clúster](/docs/containers?topic=containers-update#components).

## Registro de cambios de los ALB de Ingress
{: #alb_changelog}

Puede ver los cambios de la versión de compilación de los equilibradores de carga de aplicación (ALB) de Ingress en los clústeres de {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Cuando se actualiza el componente ALB de Ingress, se actualizan los contenedores
`nginx-ingress` e `ingress-auth` en todos los pods de ALB a la versión de compilación más reciente. De forma predeterminada, las actualizaciones automáticas del componente están habilitadas, pero puede inhabilitar las actualizaciones automáticas y actualizar manualmente el componente. Para obtener más información, consulte [Actualización del equilibrador de carga de aplicación de Ingress](/docs/containers?topic=containers-update#alb).

Consulte la tabla siguiente para ver un resumen de los cambios de cada compilación del componente ALB de Ingress.

<table summary="Visión general de los cambios de compilación del componente equilibrador de carga de aplicación de Ingress">
<caption>Registro de cambios del componente equilibrador de carga de aplicación de Ingress</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>Compilación de `nginx-ingress` / `ingress-auth`</th>
<th>Fecha de lanzamiento</th>
<th>Cambios no disruptivos</th>
<th>Cambios disruptivos</th>
</tr>
</thead>
<tbody>
<tr>
<td>470 / 330</td>
<td>7 de junio de 2019</td>
<td>Arregla las vulnerabilidades de BD de Berkeley para [CVE-2019-8457 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457).
</td>
<td>-</td>
</tr>
<tr>
<td>470 / 329</td>
<td>6 de junio de 2019</td>
<td>Arregla las vulnerabilidades de BD de Berkeley para [CVE-2019-8457 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457).
</td>
<td>-</td>
</tr>
<tr>
<td>467 / 329</td>
<td>3 de junio de 2019</td>
<td>Arregla las vulnerabilidades de GnuTLS para [CVE-2019-3829 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-3893 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3893), [CVE-2018-10844 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10845 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844) y [CVE-2018-10846 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846).
</td>
<td>-</td>
</tr>
<tr>
<td>462 / 329</td>
<td>28 de mayo de 2019</td>
<td>Arregla las vulnerabilidades de cURL para [CVE-2019-5435 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) y [CVE-2019-5436 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
<td>-</td>
</tr>
<tr>
<td>457 / 329</td>
<td>23 de mayo de 2019</td>
<td>Corrige las vulnerabilidades de Go para exploraciones de imágenes.</td>
<td>-</td>
</tr>
<tr>
<td>423 / 329</td>
<td>13 de mayo de 2019</td>
<td>Mejora el rendimiento de la integración con {{site.data.keyword.appid_full}}.</td>
<td>-</td>
</tr>
<tr>
<td>411 / 315</td>
<td>15 de abril de 2019</td>
<td>Actualiza el valor de caducidad de las cookies de {{site.data.keyword.appid_full_notm}} de forma que coincida con el valor de la caducidad de la señal de acceso.</td>
<td>-</td>
</tr>
<tr>
<td>411 / 306</td>
<td>22 de marzo de 2019</td>
<td>Actualiza la versión de Go a 1.12.1.</td>
<td>-</td>
</tr>
<tr>
<td>410 / 305</td>
<td>18 de marzo de 2019</td>
<td><ul>
<li>Corrige las vulnerabilidades de las exploraciones de imágenes.</li>
<li>Mejora el registro de la integración con {{site.data.keyword.appid_full_notm}}.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>408 / 304</td>
<td>5 de marzo de 2019</td>
<td>-</td>
<td>Corrige errores en la integración de autorización que está relacionada con la funcionalidad de cierre de sesión, caducidad de la señal y devolución de llamada de autorización de `OAuth`. Estos arreglos solo se implementan si ha habilitado la autorización de {{site.data.keyword.appid_full_notm}} mediante la anotación [`appid-auth`](/docs/containers?topic=containers-ingress_annotation#appid-auth). Para implementar estos arreglos, se añaden cabeceras adicionales, lo que aumenta el tamaño total de la cabecera. En función del tamaño de sus propias cabeceras y del tamaño total de las respuestas, es posible que tenga que ajustar las [anotaciones de almacenamiento intermedio de proxy](/docs/containers?topic=containers-ingress_annotation#proxy-buffer) que utilice.</td>
</tr>
<tr>
<td>406 / 301</td>
<td>19 de febrero de 2019</td>
<td><ul>
<li>Actualiza la versión de Go a 1.11.5.</li>
<li>Corrige las vulnerabilidades de las exploraciones de imágenes.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>404 / 300</td>
<td>31 de enero de 2019</td>
<td>Actualiza la versión de Go a 1.11.4.</td>
<td>-</td>
</tr>
<tr>
<td>403 / 295</td>
<td>23 de enero de 2019</td>
<td><ul>
<li>Actualiza la versión de NGINX de ALB a 1.15.2.</li>
<li>Ahora los certificados TLS proporcionados por IBM se renuevan automáticamente 37 días antes de que caduquen, en lugar de 7 días antes.</li>
<li>Añade la funcionalidad de cierre de sesión de {{site.data.keyword.appid_full_notm}}: Si el prefijo `/logout` existe en una vía de acceso de {{site.data.keyword.appid_full_notm}}, se eliminan cookies y se devuelve al usuario a la página de inicio de sesión.</li>
<li>Añade una cabecera a las solicitudes de {{site.data.keyword.appid_full_notm}} para fines de seguimiento internos.</li>
<li>Actualiza la directriz de ubicación de {{site.data.keyword.appid_short_notm}} de modo que se puede utilizar la anotación `app-id` con las anotaciones `proxy-buffers`, `proxy-buffer-size` y `proxy-busy-buffer-size`.</li>
<li>Corrige un error para que los registros de información no se etiqueten como errores.</li>
</ul></td>
<td>Inhabilita TLS 1.0 y 1.1 de forma predeterminada. Si los clientes que se conectan a las apps dan soporte a TLS 1.2, no se requiere ninguna acción. Si aún tiene clientes anteriores que necesitan soporte de TLS 1.0 o 1.1, habilite manualmente las versiones de TLS necesarias siguiendo [estos pasos](/docs/containers?topic=containers-ingress#ssl_protocols_ciphers). Para obtener más información sobre cómo ver las versiones de TLS que utilizan los clientes para acceder a las apps, consulte esta [publicación del blog de {{site.data.keyword.Bluemix_notm}}](https://www.ibm.com/blogs/bluemix/2018/11/ibm-cloud-kubernetes-service-alb-update-tls-1-0-and-1-1-disabled-by-default/).</td>
</tr>
<tr>
<td>393 / 291</td>
<td>9 de enero de 2019</td>
<td>Incorpora soporte para la integración con varias instancias de {{site.data.keyword.appid_full_notm}}.</td>
<td>-</td>
</tr>
<tr>
<td>393 / 282</td>
<td>29 de noviembre de 2018</td>
<td>Mejora el rendimiento de la integración con {{site.data.keyword.appid_full_notm}}.</td>
<td>-</td>
</tr>
<tr>
<td>384 / 246</td>
<td>14 de noviembre de 2018</td>
<td>Mejora las características de registro y de cierre de sesión de la integración con {{site.data.keyword.appid_full_notm}}.</td>
<td>Sustituye el certificado autofirmado para `*.containers.mybluemix.net` por el certificado firmado por LetsEncrypt que se genera automáticamente y que utiliza el clúster. Se ha eliminado el certificado autofirmado de `*.containers.mybluemix.net`.</td>
</tr>
<tr>
<td>350 / 192</td>
<td>05 de noviembre de 2018</td>
<td>Puede gestionar las actualizaciones automáticas del componente ALB de Ingress de las maneras siguientes.</td>
<td>-</td>
</tr>
</tbody>
</table>

## Registro de cambios de Fluentd para registro
{: #fluentd_changelog}

Puede ver los cambios de la versión del componente Fluentd para el registro en los clústeres de {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

De forma predeterminada, las actualizaciones automáticas del componente están habilitadas, pero puede inhabilitar las actualizaciones automáticas y actualizar manualmente el componente. Para obtener más información, consulte [Gestión de actualizaciones automáticas para Fluentd](/docs/containers?topic=containers-update#logging-up).

Consulte la tabla siguiente para ver un resumen de los cambios de cada compilación del componente Fluentd.

<table summary="Visión general de los cambios de compilación del componente Fluentd">
<caption>Registro de cambios del componente Fluentd</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>Compilación de Fluentd</th>
<th>Fecha de lanzamiento</th>
<th>Cambios no disruptivos</th>
<th>Cambios disruptivos</th>
</tr>
</thead>
<tr>
<td>e7c10d74350dc64d4d92ba7f72bb4ff9219315d2</td>
<td>30 de mayo de 2019</td>
<td>Actualiza el mapa de configuración de Fluentd de modo que siempre pase por alto los registros de pod procedentes de los espacios de nombres de IBM, incluso cuando el nodo maestro de Kubernetes no esté disponible.</td>
<td>-</td>
</tr>
<tr>
<td>c16fe1602ab65db4af0a6ac008f99ca2a526e6f6</td>
<td>21 de mayo de 2019</td>
<td>Arregla un error por el que no se visualizaban las métricas de nodo trabajador.</td>
<td>-</td>
</tr>
<tr>
<td>60fc11f7bd39d9c6cfed923c598bf6457b3f2037</td>
<td>10 de mayo de 2019</td>
<td>Actualiza los paquetes de Ruby para [CVE-2019-8320 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320), [CVE-2019-8321 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321), [CVE-2019-8322 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322), [CVE-2019-8323 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323), [CVE-2019-8324 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) y [CVE-2019-8325 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325).</td>
<td>-</td>
</tr>
<tr>
<td>91a737f68f7d9e81b5d2223c910aaa7d7f91b76d</td>
<td>8 de mayo de 2019</td>
<td>Actualiza los paquetes de Ruby para [CVE-2019-8320 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320), [CVE-2019-8321 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321), [CVE-2019-8322 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322), [CVE-2019-8323 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323), [CVE-2019-8324 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) y [CVE-2019-8325 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325).</td>
<td>-</td>
</tr>
<tr>
<td>d9af69e286986a05ed4a50469585b1cf978ddb1d</td>
<td>11 de abril de 2019</td>
<td>Actualiza el plugin cAdvisor para que utilice TLS 1.2.</td>
<td>-</td>
</tr>
<tr>
<td>3100ddb62580a9f46ffdff7bab2ebec40b164de6</td>
<td>1 de abril de 2019</td>
<td>Actualiza la cuenta de servicio de Fluentd.</td>
<td>-</td>
</tr>
<tr>
<td>c85567b75bd7ad1c9428794cd63a8e239c3fd8f5</td>
<td>18 de marzo de 2019</td>
<td>Elimina la dependencia de cURL para [CVE-2019-8323 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323).</td>
<td>-</td>
</tr>
<tr>
<td>320ffdf87de068ee2f7f34c0e7a47a111e8d457b</td>
<td>18 de febrero de 2019</td>
<td><ul>
<li>Actualiza Fluentd a la versión 1.3.</li>
<li>Elimina Git de la imagen de Fluentd para [CVE-2018-19486 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19486).</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>972865196aefd3324105087878de12c518ed579f</td>
<td>1 de enero de 2019</td>
<td><ul>
<li>Habilita la codificación UTF-8 para el plugin `in_tail` de Fluentd.</li>
<li>Arreglos de errores menores.</li>
</ul></td>
<td>-</td>
</tr>
</tbody>
</table>
