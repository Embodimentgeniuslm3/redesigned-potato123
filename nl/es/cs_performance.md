---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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


# Ajuste del rendimiento
{: #kernel}

Si tiene requisitos de optimización de rendimiento específicos, puede cambiar los valores predeterminados para algunos componentes de clúster en {{site.data.keyword.containerlong}}.
{: shortdesc}

Si decide cambiar los valores predeterminados, lo hace por su cuenta y riesgo. Usted es el responsable de ejecutar pruebas en cualquier configuración modificada y de cualquier interrupción potencial ocasionada por los valores modificados en el entorno.
{: important}

## Optimización del rendimiento del nodo trabajador
{: #worker}

Si tiene requisitos de optimización de rendimiento específicos, puede cambiar los valores predeterminados para los parámetros `sysctl` del kernel de Linux en los nodos trabajadores.
{: shortdesc}

Los nodos trabajadores se suministran automáticamente con el rendimiento optimizado del kernel, pero puede cambiar los valores predeterminados aplicando un objeto de [`DaemonSet` de Kubernetes
![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) al clúster. El conjunto de daemons altera los valores de todos los nodos trabajadores existentes y aplica los valores a los nuevos nodos trabajadores que se suministran en el clúster. Los pods no se ven afectados.

Debe tener el [rol de servicio **Gestor** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre todos los espacios de nombres para ejecutar `initContainer` con privilegios del ejemplo. Después de inicializar los contenedores para los despliegues, se descartarán los privilegios.
{: note}

1. Guarde el siguiente conjunto de daemons en un archivo denominado `worker-node-kernel-settings.yaml`. En la sección `spec.template.spec.initContainers`, añada los campos y valores de los parámetros de `sysctl` que desea ajustar. Este conjunto de daemons de ejemplo cambia el número máximo predeterminado de conexiones permitidas en el entorno mediante el valor `net.core.somaxconn` y el rango de puertos efímeros mediante el valor `net.ipv4.ip_local_port_range`.
    ```
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: kernel-optimization
      namespace: kube-system
      labels:
        tier: management
        app: kernel-optimization
    spec:
      selector:
        matchLabels:
          name: kernel-optimization
      template:
        metadata:
          labels:
            name: kernel-optimization
        spec:
          hostNetwork: true
          hostPID: true
          hostIPC: true
          initContainers:
            - mandato:
                - sh
                - -c
                - sysctl -w net.ipv4.ip_local_port_range="1025 65535"; sysctl -w net.core.somaxconn=32768;
              image: alpine:3.6
              imagePullPolicy: IfNotPresent
              name: sysctl
              resources: {}
              securityContext:
                privileged: true
                capabilities:
                  add:
                    - NET_ADMIN
              volumeMounts:
                - name: modifysys
                  mountPath: /sys
          containers:
            - resources:
                requests:
                  cpu: 0.01
              image: alpine:3.6
              name: sleepforever
              command: ["/bin/sh", "-c"]
              args:
                - >
                  while true; do
                    sleep 100000;
                  done
          volumes:
            - name: modifysys
              hostPath:
                path: /sys
    ```
    {: codeblock}

2. Aplique el conjunto de daemons a los nodos trabajadores. Los cambios se aplican inmediatamente.
    ```
    kubectl apply -f worker-node-kernel-settings.yaml
    ```
    {: pre}

<br />

Para revertir los parámetros de `sysctl` de los nodos trabajadores a los valores predeterminados establecidos por {{site.data.keyword.containerlong_notm}}:

1. Suprima el conjunto de daemons. Se eliminan los `initContainers` que han aplicado los valores personalizados.
    ```
    kubectl delete ds kernel-optimization
    ```
    {: pre}

2. [Rearranque todos los nodos trabajadores del clúster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot). Los nodos trabajadores vuelven a estar en línea con los valores predeterminados aplicados.

<br />


## Optimización del rendimiento de pod
{: #pod}

Si tiene demandas de carga de trabajo de rendimiento específicas, puede cambiar los valores predeterminados para los parámetros `sysctl` del kernel de Linux en los espacios de nombres de red de pod.
{: shortdesc}

Para optimizar los valores del kernel para los pods de app, puede insertar un parche [`initContainer` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) en el YAML de `pod/ds/rs/deployment` para cada despliegue. El `initContainer` se añade a cada despliegue de app que se encuentra en el espacio de nombres de red de pod para el que desea optimizar el rendimiento.

Antes de empezar, asegúrese de que tiene el [rol de servicio **Gestor** de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre todos los espacios de nombres para ejecutar `initContainer` con privilegios del ejemplo. Después de inicializar los contenedores para los despliegues, se descartarán los privilegios.

1. Guarde el siguiente parche de `initContainer` en un archivo denominado `pod-patch.yaml` y añada los campos y los valores de los parámetros de `sysctl` que desea ajustar. Este `initContainer` de ejemplo cambia el número máximo predeterminado de conexiones permitidas en el entorno mediante el valor `net.core.somaxconn` y el rango de puertos efímeros a través del valor `net.ipv4.ip_local_port_range`.
    ```
    spec:
      template:
        spec:
          initContainers:
          - mandato:
            - sh
            - -c
            - sysctl -e -w net.core.somaxconn=32768;  sysctl -e -w net.ipv4.ip_local_port_range="1025 65535";
            image: alpine:3.6
            imagePullPolicy: IfNotPresent
            name: sysctl
            resources: {}
            securityContext:
              privileged: true
    ```
    {: codeblock}

2. Aplique un parche a cada uno de los despliegues.
    ```
    kubectl patch deployment <deployment_name> --patch pod-patch.yaml
    ```
    {: pre}

3. Si ha cambiado el valor `net.core.somaxconn` en los valores de kernel, la mayoría de las apps pueden utilizar automáticamente el valor actualizado. Sin embargo, es posible que en algunas apps se deba cambiar manualmente el valor correspondiente en el código de la app para que coincida con el valor del kernel. Por ejemplo, si va a ajustar el rendimiento de un pod en el que se ejecuta una app NGINX, debe cambiar el valor del campo `backlog` en el código de la app NGINX para que coincida. Para obtener más información, consulte esta [publicación del blog sobre NGINX ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.nginx.com/blog/tuning-nginx/).

<br />


## Ajuste de recursos del proveedor de métricas de clúster
{: #metrics}

Las configuraciones del proveedor de métricas del clúster (`metrics-server` en Kubernetes 1.12 y posterior, o
`heapster` en versiones anteriores) están optimizadas para clústeres con 30 pods o menos por cada nodo trabajador. Si el clúster tiene más pods por cada nodo trabajador, es posible que el contenedor principal del proveedor de métricas
`metrics-server` o `heapster` para el pod se reinicie con frecuencia con un mensaje de error como
`OOMKilled`.

El pod del proveedor de métricas también tiene un contenedor `nanny` que escala las solicitudes de recursos y límites del contenedor principal de `metrics-server` o `heapster` en respuesta al número de nodos trabajadores del clúster. Puede cambiar los recursos predeterminados editando el mapa de configuración del proveedor de métricas.

Antes de empezar: [Inicie la sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Abra el archivo YAML del mapa de configuración del proveedor de métricas de clúster.
    *  Para `metrics-server`:
       ```
       kubectl get configmap metrics-server-config -n kube-system -o yaml
       ```
       {: pre}
    *  Para `heapster`:
       ```
       kubectl get configmap heapster-config -n kube-system -o yaml
       ```
       {: pre}
    Salida de ejemplo:
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
    kind: ConfigMap
    metadata:
      annotations:
        armada-service: cruiser-kube-addons
        version: --
      creationTimestamp: 2018-10-09T20:15:32Z
      labels:
        addonmanager.kubernetes.io/mode: EnsureExists
        kubernetes.io/cluster-service: "true"
      name: heapster-config
      namespace: kube-system
      resourceVersion: "526"
      selfLink: /api/v1/namespaces/kube-system/configmaps/heapster-config
      uid: 11a1aaaa-bb22-33c3-4444-5e55e555e555
    ```
    {: screen}

2.  Añada el campo `memoryPerNode` al mapa de configuración en la sección
`data.NannyConfiguration`. El valor predeterminado para `metrics-server` y `heapster` está establecido en `4Mi`.
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
        memoryPerNode: 5Mi
    kind: ConfigMap
    ...
    ```
    {: codeblock}

3.  Aplique los cambios.
    ```
    kubectl apply -f heapster-config.yaml
    ```
    {: pre}

4.  Supervise los pods del proveedor de métricas para ver si los contenedores siguen reiniciándose debido a un mensaje de error
`OOMKilled`. Si es así, repita estos pasos y aumente el tamaño de `memoryPerNode` hasta que el pod sea estable.

¿Desea ajustar más valores? Consulte los [documentos de configuración del redimensionador de complementos de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/autoscaler/tree/master/addon-resizer#addon-resizer-configuration) para obtener más ideas.
{: tip}
