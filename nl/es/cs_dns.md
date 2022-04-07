---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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


# Configuración del proveedor de DNS de clúster
{: #cluster_dns}

Se asigna un nombre DNS (Sistema de nombres de dominio) a cada servicio del clúster de {{site.data.keyword.containerlong}}, que el proveedor de DNS del clúster registra para resolver solicitudes de DNS. En función de la versión de Kubernetes de su clúster, puede elegir entre un DNS Kubernetes DNS (KubeDNS) o [CoreDNS ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://coredns.io/). Para obtener más información sobre DNS para servicios y pods, consulte
[la documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/).
{: shortdesc}

**¿Qué versiones de Kubernetes dan soporte a cada proveedor de DNS de clúster?**<br>

| Versión de Kubernetes | Valor predeterminado para clústeres nuevos | Descripción |
|---|---|---|
| 1.14 y posteriores | CoreDNS | Si un clúster utiliza KubeDNS y se actualiza a la versión 1.14 o posterior desde una versión anterior, el proveedor de DNS de clúster se migra automáticamente desde KubeDNS a CoreDNS durante la actualización del clúster. No puede volver a cambiar el proveedor de DNS del clúster por KubeDNS. |
| 1.13 | CoreDNS | Los clústeres que se actualizan a 1.13 desde una versión anterior mantienen el proveedor de DNS que utilizaban en el momento de la actualización. Si desea utilizar uno distinto, [cambie el proveedor de DNS](#dns_set). |
| 1.12 | KubeDNS | Para utilizar CoreDNS en su lugar, [cambie el proveedor de DNS](#set_coredns). |
| 1.11 y anteriores | KubeDNS | No puede cambiar el proveedor de DNS por CoreDNS. |
{: caption="Proveedor de DNS de clúster predeterminado por versión de Kubernetes" caption-side="top"}

**¿Cuáles son las ventajas de utilizar CoreDNS en lugar de KubeDNS?**<br>
CoreDNS es el proveedor de DNS de clúster soportado predeterminado para Kubernetes versión 1.13 y posteriores, y recientemente se ha convertido en un [proyecto Cloud Native Computing Foundation (CNCF) certificado ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.cncf.io/projects/). Un proyecto certificado se ha puesto a prueba a fondo, se ha reforzado y está listo para su adopción a gran escala y a nivel de producción.

Tal como se indica en el [anuncio de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/blog/2018/12/03/kubernetes-1-13-release-announcement/), CoreDNS es un servidor DNS de finalidad general y autorizado que proporciona una integración compatible con versiones anteriores, aunque extensible, con Kubernetes. Debido a que CoreDNS es un solo proceso ejecutable, tiene menos dependencias y partes móviles que puedan experimentar problemas que el proveedor de DNS de clúster anterior. El proyecto también se ha escrito en el mismo lenguaje que el proyecto Kubernetes, `Go`, lo que ayuda a proteger la memoria. Por último, CoreDNS da soporte a casos prácticos más flexibles que KubeDNS porque puede crear entradas de DNS personalizadas, como por ejemplo [configuraciones comunes de la documentación de CoreDNS ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://coredns.io/manual/toc/#setups).

## Escalado automático del proveedor de DNS de clúster
{: #dns_autoscale}

De forma predeterminada, el proveedor de DNS de clúster de {{site.data.keyword.containerlong_notm}} incluye un despliegue para escalar automáticamente los pods de DNS como respuesta al número de nodos trabajadores y núcleos dentro del clúster. Puede ajustar los parámetros del programa de escalado automático de DNS editando el mapa de configuración de escalado automático de DNS. Por ejemplo, si las apps hacen un uso intensivo del proveedor de DNS de clúster, es posible que tenga que aumentar el número mínimo de pods de DNS para dar soporte a la app. Para obtener más información, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/).
{: shortdesc}

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Verifique que el despliegue del proveedor de DNS del clúster está disponible. Es posible que tenga el programa de escalado automático de KubeDNS, el de CoreDNS o el de ambos proveedores de DNS instalados en el clúster. Si se han instalado ambos programas de escalado automático de DNS, busque el que se está utilizando en la columna **AVAILABLE** de la salida de la CLI. El despliegue que se está utilizando se muestra con un despliegue disponible.
    ```
    kubectl get deployment -n kube-system | grep dns-autoscaler
    ```
    {: pre}

    Salida de ejemplo:
    ```
    NAME                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    coredns-autoscaler     0         0         0            0           32d
    kube-dns-autoscaler    1         1         1            1           260d
    ```
    {: screen}
2.  Obtenga el nombre del mapa de configuración para ver los parámetros del programa de escalado automático de DNS.
    ```
    kubectl get configmap -n kube-system
    ```
    {: pre}    
3.  Edite los valores predeterminados correspondientes al programa de escalado automático de DNS. Busque el campo `data.linear`, cuyo valor predeterminado es un pod DNS por 16 nodos trabajadores o 256 núcleos, con un mínimo de dos DNS, independientemente del tamaño del clúster (`preventSinglePointFailure: true`). Para obtener más información, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/#tuning-autoscaling-parameters).
    ```
    kubectl edit configmap -n kube-system <dns-autoscaler>
    ```
    {: pre}

    Salida de ejemplo:
    ```
    apiVersion: v1
    data:
      linear: '{"coresPerReplica":256,"nodesPerReplica":16,"preventSinglePointFailure":true}'
    kind: ConfigMap
    metadata:
    ...
    ```
    {: screen}

## Personalización del proveedor de DNS de clúster
{: #dns_customize}

Puede personalizar el proveedor de DNS de clúster de {{site.data.keyword.containerlong_notm}} editando el mapa de configuración de DNS. Por ejemplo, supongamos que desea configurar `dominios auxiliares` y servidores de nombres en sentido ascendente para resolver servicios que apuntan a hosts externos. Además, si utiliza CoreDNS, puede configurar varios [Corefiles ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://coredns.io/2017/07/23/corefile-explained/) dentro del mapa de configuración de CoreDNS. Para obtener más información, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/).
{: shortdesc}

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Verifique que el despliegue del proveedor de DNS del clúster está disponible. Es posible que tenga el proveedor de DNS de KubeDNS, el de CoreDNS o ambos proveedores de DNS instalados en el clúster. Si se han instalado ambos proveedores de DNS, busque el que se está utilizando en la columna **AVAILABLE** de la salida de la CLI. El despliegue que se está utilizando se muestra con un despliegue disponible.
    ```
    kubectl get deployment -n kube-system -l k8s-app=kube-dns
    ```
    {: pre}

    Salida de ejemplo:
    ```
    NAME                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    coredns                0         0         0            0           89d
    kube-dns-amd64         2         2         2            2           89d
    ```
    {: screen}
2.  Edite los valores predeterminados correspondientes al mapa de configuración de CoreDNS o de KubeDNS.

    *   **Para CoreDNS**: Utilice un archivo Corefile en la sección `data` del mapa de configuración para personalizar los `dominios auxiliares` y los servidores de nombres en sentido ascendente. Para obtener más información, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns).
        ```
        kubectl edit configmap -n kube-system coredns
        ```
        {: pre}

        **Salida de ejemplo de CoreDNS**:
          ```
          apiVersion: v1
          kind: ConfigMap
          metadata:
            name: coredns
            namespace: kube-system
          data:
            Corefile: |
              abc.com:53 {
                  errors
                  cache 30
                  proxy . 1.2.3.4
              }
              .:53 {
                  errors
                  health
                  kubernetes cluster.local in-addr.arpa ip6.arpa {
                     pods insecure
                     upstream 172.16.0.1
                     fallthrough in-addr.arpa ip6.arpa
                  }
                  prometheus :9153
                  proxy . /etc/resolv.conf
                  cache 30
                  loop
                  reload
                  loadbalance
              }
          ```
          {: screen}

          ¿Tiene muchas personalizaciones que desea organizar? En Kubernetes versión 1.12.6_1543 y posteriores, puede añadir varios archivos Corefiles al mapa de configuración de CoreDNS. Para obtener más información, consulte la [documentación de importación de Corefile ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://coredns.io/plugins/import/).
          {: tip}

    *   **Para KubeDNS**: Configure `dominios auxiliares` y servidores de nombres en sentido ascendente en la sección `data` del mapa de configuración. Para obtener más información, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#kube-dns).
        ```
        kubectl edit configmap -n kube-system kube-dns
        ```
        {: pre}

        **Salida de ejemplo de KubeDNS**:
        ```
        apiVersion: v1
        kind: ConfigMap
        metadata:
          name: kube-dns
          namespace: kube-system
        data:
          stubDomains: |
          {"abc.com": ["1.2.3.4"]}
        ```
        {: screen}

## Establecimiento del proveedor de DNS del clúster en CoreDNS o en KubeDNS
{: #dns_set}

Si tiene un clúster de {{site.data.keyword.containerlong_notm}} que ejecuta Kubernetes versión 1.12 o 1.13, puede elegir entre utilizar un DNS de Kubernetes (KubeDNS) o un CoreDNS como proveedor de DNS del clúster.
{: shortdesc}

Los clústeres que ejecutan otras versiones de Kubernetes no pueden establecer el proveedor de DNS del clúster. La versión 1.11 y anteriores solo dan soporte a KubeDNS, y la versión 1.14 y posteriores solo dan soporte a CoreDNS.
{: note}

**Antes de empezar**:
1.  [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2.  Determine el proveedor de DNS de clúster actual. En el ejemplo siguiente, KubeDNS es el proveedor de DNS de clúster actual.
    ```
    kubectl cluster-info
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ...
    KubeDNS está en ejecución en https://c2.us-south.containers.cloud.ibm.com:20190/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
    ...
    ```
    {: screen}
3.  En función del proveedor de DNS que utilice el clúster, siga los pasos para cambiar los proveedores de DNS.
    *  [Cambie para utilizar CoreDNS](#set_coredns).
    *  [Cambie para utilizar KubeDNS](#set_kubedns).

### Configuración de CoreDNS como proveedor de DNS de clúster
{: #set_coredns}

Configure CoreDNS en lugar de KubeDNS como proveedor de DNS de clúster.
{: shortdesc}

1.  Si ha personalizado el mapa de configuración del proveedor de KubeDNS o el mapa de configuración del programa de escalado automático de KubeDNS, transfiera las personalizaciones a los mapas de configuración de CoreDNS.
    *   Para el mapa de configuración `kube-dns` del espacio de nombres `kube-system`, transfiera las [personalizaciones de DNS ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/) al mapa de configuración `coredns` del espacio de nombres `kube-system`. La sintaxis difiere entre los mapas de configuración de `kube-dns` y `coredns`. Para ver un ejemplo, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns-configuration-equivalent-to-kube-dns).
    *   Para el mapa de configuración `kube-dns-autoscaler` del espacio de nombres `kube-system`, transfiera las [personalizaciones del programa de escalado automático de DNS ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/) al mapa de configuración `coredns-autoscaler` del espacio de nombres `kube-system`. La sintaxis de personalización es la misma en ambos.
2.  Reduzca la escala del despliegue del programa de escalado automático de KubeDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=0 kube-dns-autoscaler
    ```
    {: pre}
3.  Compruebe y espere a que se supriman los pods.
    ```
    kubectl get pods -n kube-system -l k8s-app=kube-dns-autoscaler
    ```
    {: pre}
4.  Reduzca la escala del despliegue de KubeDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=0 kube-dns-amd64
    ```
    {: pre}
5.  Aumente la escala del despliegue del programa de escalado automático de CoreDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=1 coredns-autoscaler
    ```
    {: pre}
6.  Etiquete y anote el servicio DNS de clúster para CoreDNS.
    ```
    kubectl label service --overwrite -n kube-system kube-dns kubernetes.io/name=CoreDNS
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/port=9153
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/scrape=true
    ```
    {: pre}
7.  **Opcional**: Si tiene previsto utilizar Prometheus para recopilar métricas de los pods CoreDNS, debe añadir un puerto de métricas al servicio `kube-dns` desde el que está realizando el cambio.
    ```
    kubectl -n kube-system patch svc kube-dns --patch '{"spec": {"ports": [{"name":"metrics","port":9153,"protocol":"TCP"}]}}' --type strategic
    ```
    {: pre}



### Configuración de KubeDNS como proveedor de DNS de clúster
{: #set_kubedns}

Configure KubeDNS en lugar de CoreDNS como proveedor de DNS de clúster.
{: shortdesc}

1.  Si ha personalizado el mapa de configuración del proveedor de CoreDNS o el mapa de configuración del programa de escalado automático de CoreDNS, transfiera las personalizaciones a los mapas de configuración de KubeDNS.
    *   Para el mapa de configuración `coredns` del espacio de nombres `kube-system`, transfiera las [personalizaciones de DNS ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/) al mapa de configuración `kube-dns` del espacio de nombres `kube-system`. La sintaxis difiere entre los mapas de configuración de `kube-dns` y `coredns`. Para ver un ejemplo, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns-configuration-equivalent-to-kube-dns).
    *   Para el mapa de configuración `coredns-autoscaler` del espacio de nombres `kube-system`, transfiera las [personalizaciones del programa de escalado automático de DNS ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/) al mapa de configuración `kube-dns-autoscaler` del espacio de nombres `kube-system`. La sintaxis de personalización es la misma en ambos.
2.  Reduzca la escala del despliegue del programa de escalado automático de CoreDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=0 coredns-autoscaler
    ```
    {: pre}
3.  Compruebe y espere a que se supriman los pods.
    ```
    kubectl get pods -n kube-system -l k8s-app=coredns-autoscaler
    ```
    {: pre}
4.  Reduzca la escala del despliegue de CoreDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=0 coredns
    ```
    {: pre}
5.  Aumente la escala del despliegue del programa de escalado automático de KubeDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=1 kube-dns-autoscaler
    ```
    {: pre}
6.  Etiquete y anote el servicio DNS de clúster para KubeDNS.
    ```
    kubectl label service --overwrite -n kube-system kube-dns kubernetes.io/name=KubeDNS
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/port-
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/scrape-
    ```
    {: pre}
7.  **Opcional**: Si ha utilizado Prometheus para recopilar métricas de los pods CoreDNS, el servicio `kube-dns` tenía un puerto de métricas. Sin embargo, KubeDNS no necesita incluir este puerto de métricas, de modo que puede eliminar el puerto del servicio.
    ```
    kubectl -n kube-system patch svc kube-dns --patch '{"spec": {"ports": [{"name":"dns","port":53,"protocol":"UDP"},{"name":"dns-tcp","port":53,"protocol":"TCP"}]}}' --type merge
    ```
    {: pre}
