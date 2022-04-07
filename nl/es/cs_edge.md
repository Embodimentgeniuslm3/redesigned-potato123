---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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


# Restricción del tráfico de red para los nodos trabajadores de extremo
{: #edge}

Los nodos trabajadores de extremo pueden mejorar la seguridad de su clúster de Kubernetes al permitir el acceso externo a un número inferior de nodos trabajadores y aislar la carga de trabajo en red en {{site.data.keyword.containerlong}}.
{:shortdesc}

Cuando estos nodos trabajadores se marcan solo para trabajo en red, las demás cargas de trabajo no pueden consumir la CPU ni la memoria del nodo trabajador ni interferir con la red.

Si tiene un clúster multizona y desea restringir el tráfico de red a los nodos trabajadores de extremo, debe haber al menos dos nodos trabajadores de extremo en cada zona para conseguir una alta disponibilidad de los pods del equilibrador de carga o Ingress. Cree una agrupación de nodos trabajadores de extremo que abarque todas las zonas del clúster y tenga como mínimo dos nodos trabajadores por zona.
{: tip}

## Cómo etiquetar nodos trabajadores como nodos extremos
{: #edge_nodes}

Añada la etiqueta `dedicated=edge` a dos o más nodos trabajadores en cada VLAN pública o privada en el clúster para garantizar que los equilibradores de carga de red (NLB) y los equilibradores de carga de aplicación (ALB) de Ingress están desplegados solo en los nodos trabajadores.
{:shortdesc}

En Kubernetes 1.14 y posteriores, puede desplegar NLB y ALB públicos y privados en los nodos de extremo. En Kubernetes 1.13 y anteriores, se pueden desplegar ALB públicos y privados y NLB públicos en nodos de extremo, pero los NLB privados solo deben desplegarse en nodos trabajadores que no sean de extremo en el clúster.
{: note}

Antes de empezar:

* Asegúrese de que tiene los siguientes [roles de {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform):
  * Cualquier rol de plataforma para el clúster
  * Rol de servicio **Escritor** o **Gestor** sobre todos los espacios de nombres
* [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

</br>Para etiquetar nodos trabajadores como nodos extremos:

1. [Cree una nueva agrupación de nodos trabajadores](/docs/containers?topic=containers-add_workers#add_pool) que abarque todas las zonas del clúster y que tenga al menos dos nodos trabajadores por zona. En el mandato `ibmcloud ks worker-pool-create`, incluya el distintivo `--labels dedicated=edge` para etiquetar todos los nodos trabajadores de la agrupación. Todos los nodos trabajadores de esta agrupación, incluidos los nodos trabajadores que añada posteriormente, se etiquetan como nodos de extremo.
  <p class="tip">Si desea utilizar una agrupación de nodos trabajadores existente, la agrupación debe abarcar todas las zonas del clúster y tener al menos dos trabajadores por zona. Puede etiquetar la agrupación de nodos trabajadores con `dedicated=edge` utilizando la [API de agrupación de trabajadores PATCH](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool). En el cuerpo de la solicitud, pase el JSON siguiente. Después de que la agrupación de nodos trabajadores se marque con `dedicated=edge`, todos los nodos trabajadores existentes y posteriores obtienen esta etiqueta y los equilibradores de carga e Ingress se despliegan en un nodo trabajador de extremo.
      <pre class="screen">
      {
        "labels": {"dedicated":"edge"},
        "state": "labels"
      }</pre></p>

2. Verifique que la agrupación de nodos trabajadores y los nodos trabajadores tengan la etiqueta `dedicated=edge`.
  * Para comprobar la agrupación de nodos trabajadores:
    ```
    ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID>
    ```
    {: pre}

  * Para comprobar los nodos trabajadores individuales, revise el campo **Labels** de la salida del mandato siguiente.
    ```
    kubectl describe node <worker_node_private_IP>
    ```
    {: pre}

3. Recupere todos los NLB y ALB existentes en el clúster.
  ```
  kubectl get services --all-namespaces | grep LoadBalancer
  ```
  {: pre}

  En la salida, anote el valor de **Namespace** y de **Name** de cada servicio de equilibrador de carga. Por ejemplo, en la siguiente salida, hay cuatro servicios de equilibrador de carga: un NLB público en el espacio de nombres `default` y un ALB privado y dos públicos en el espacio de nombres `kube-system`.
  ```
  NAMESPACE     NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                                     AGE
  default       webserver-lb                                     LoadBalancer   172.21.190.18    169.46.17.2     80:30597/TCP                                10m
  kube-system   private-crdf253b6025d64944ab99ed63bb4567b6-alb1  LoadBalancer   172.21.158.78    10.185.94.150   80:31015/TCP,443:31401/TCP,9443:32352/TCP   25d
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb1   LoadBalancer   172.21.84.248    169.48.228.78   80:30286/TCP,443:31363/TCP                  1h
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb2   LoadBalancer   172.21.229.73    169.46.17.6     80:31104/TCP,443:31138/TCP                  57m
  ```
  {: screen}

4. Utilizando la salida del paso anterior, ejecute el mandato siguiente para cada NLB y ALB. Este mandato vuelve a desplegar el NLB o el ALB en un nodo trabajador de extremo.

  Si el clúster ejecuta Kubernetes 1.14 o posterior, puede desplegar NLB y ALB públicos y privados en los nodos trabajadores de extremo. En Kubernetes 1.13 y anteriores, solo se pueden desplegar ALB públicos y privados y NLB públicos en los nodos de extremo, por lo que no debe volver a desplegar los servicios NLB privados.
  {: note}

  ```
  kubectl get service -n <namespace> <service_name> -o yaml | kubectl apply -f -
  ```
  {: pre}

  Salida de ejemplo:
  ```
  service "webserver-lb" configured
  ```
  {: screen}

5. Para verificar que las cargas de trabajo de red están restringidas a nodos de extremo, confirme que los pods de NLB y ALB están planificados en los nodos de extremo y no están planificados en nodos que no sean de extremo.

  * Pods de NLB:
    1. Confirme que los pods de NLB están desplegados en nodos de extremo. Busque la dirección IP externa del servicio de equilibrador de carga que aparece en la salida del paso 3. Sustituya los puntos (`.`) por guiones (`-`). Ejemplo para el NLB `webserver-lb` que tiene la dirección IP externa `169.46.17.2`:
      ```
      kubectl describe nodes -l dedicated=edge | grep "169-46-17-2"
      ```
      {: pre}

      Salida de ejemplo:
      ```
      ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-2z64r                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
      ```
      {: screen}
    2. Confirme que no se ha desplegado ningún pod de NLB en nodos que no sean de extremo. Ejemplo para el NLB `webserver-lb` que tiene la dirección IP externa `169.46.17.2`:
      ```
      kubectl describe nodes -l dedicated!=edge | grep "169-46-17-2"
      ```
      {: pre}
      * Si los pods de NLB se han desplegado correctamente en nodos de extremo, no se devuelve ningún pods de NLB. Los NLB se vuelven a planificar correctamente solo en nodos trabajadores de extremo.
      * Si se devuelven pods de NLB, continúe con el paso siguiente.

  * Pods de ALB:
    1. Confirme que todos los pods de ALB están desplegados en nodos de extremo. Busque la palabra clave `alb`. Cada ALB público y privado tiene dos pods. Ejemplo:
      ```
      kubectl describe nodes -l dedicated=edge | grep alb
      ```
      {: pre}

      Salida de ejemplo para un clúster con dos zonas en las que se ha habilitado un ALB privado predeterminado y dos ALB públicos predeterminados:
      ```
      kube-system                private-crdf253b6025d64944ab99ed63bb4567b6-alb1-d5dd478db-27pv4    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                private-crdf253b6025d64944ab99ed63bb4567b6-alb1-d5dd478db-7p9q6    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb1-5ff8cdff89-s77z6    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb1-5ff8cdff89-kvs9f    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb2-57df7c7b5b-tp6xw    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      kube-system                public-crdf253b6025d64944ab99ed63bb4567b6-alb2-57df7c7b5b-z5p2v    0 (0%)        0 (0%)      0 (0%)           0 (0%)
      ```
      {: screen}

    2. Confirme que no se ha desplegado ningún pod de ALB en nodos que no sean de extremo. Ejemplo:
      ```
      kubectl describe nodes -l dedicated!=edge | grep alb
      ```
      {: pre}
      * Si los pods de ALB se han desplegado correctamente en nodos de extremo, no se devuelve ningún pods de ALB. Los ALB se vuelven a planificar correctamente solo en nodos trabajadores de extremo.
      * Si se devuelven pods de ALB, continúe con el paso siguiente.

6. Si todavía hay pods de NLB o de ALB desplegados en nodos que no sean de extremo, puede suprimir los pods de modo que se vuelvan a desplegar en nodos de extremo. **Importante**: los pods se deben suprimir de uno en uno y se debe verificar que un pod se vuelve a planificar en un nodo de extremo antes de suprimir otros pods.
  1. Suprima una pod. Ejemplo del caso en el que los pods de NLB `webserver-lb` no se han planificado en un nodo de extremo:
    ```
    kubectl delete pod ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg
    ```
    {: pre}

  2. Verifique que el pod se vuelve a planificar en un nodo trabajador de extremo. La replanificación es automática, pero puede tardar unos minutos. Ejemplo para el NLB `webserver-lb` que tiene la dirección IP externa `169.46.17.2`:
    ```
    kubectl describe nodes -l dedicated=edge | grep "169-46-17-2"
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-wz6dg                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
    ibm-system                 ibm-cloud-provider-ip-169-46-17-2-76fcb4965d-2z64r                 5m (0%)       0 (0%)      10Mi (0%)        0 (0%)
    ```
    {: screen}

</br>Ha etiquetado nodos trabajadores de una agrupación de trabajadores con `dedicated=edge` y ha vuelto a desplegar todos los ALB y NLB existentes en nodos de extremo. Los siguientes ALB y NLB que se añaden al clúster también se desplegarán en un nodo de extremo en la agrupación de nodos trabajadores de extremo. A continuación, evite que el resto de las [cargas de trabajo se ejecuten en los nodos trabajadores de extremo](#edge_workloads) y [bloquee el tráfico entrante en los NodePorts de los nodos trabajadores](/docs/containers?topic=containers-network_policies#block_ingress).

<br />


## Restricción de la ejecución de cargas de trabajo en los nodos trabajadores de extremo
{: #edge_workloads}

Una ventaja de los nodos trabajadores extremos es que se puede especificar que solo ejecuten servicios de red.
{:shortdesc}

Utilizar la tolerancia `dedicated=edge` significa que todos los servicios de equilibrador de carga de red (NLB) y de equilibrador de carga de aplicación (ALB) de Ingress se despliegan únicamente en los nodos trabajadores etiquetados. Sin embargo, para evitar que se ejecuten otras cargas de trabajo en los nodos trabajadores de extremo y consuman recursos de los nodos trabajadores, se deben utilizar [antagonismos de Kubernetes ![Enlace de icono externo](../icons/launch-glyph.svg "Enlace de icono externo")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/).

Antes de empezar:
- Asegúrese de que tiene el [rol de servicio de {{site.data.keyword.Bluemix_notm}} IAM de **Gestor** sobre todos los espacios de nombres](/docs/containers?topic=containers-users#platform).
- [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

</br>Para evitar la ejecución de otras cargas de trabajo en los nodos trabajadores de extremo:

1. Aplique un antagonismo a todos los nodos trabajadores con la etiqueta `dedicated=edge` que impida que los pods se ejecuten en el nodo trabajador y que elimine los pods que no tengan la etiqueta `dedicated=edge` del nodo trabajador. Los pods que se eliminen se volverán a desplegar en otros nodos trabajadores con capacidad.
  ```
  kubectl taint node -l dedicated=edge dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
  {: pre}
  Ahora, solo se desplegarán en los nodos trabajadores de extremo los pods con la tolerancia `dedicated=edge`.

2. Verifique que los nodos de extremo tienen antagonismo.
  ```
  kubectl describe nodes -l dedicated=edge | grep "Taint|Hostname"
  ```
  {: pre}

  Salida de ejemplo:
  ```
  Taints:             dedicated=edge:NoExecute
    Hostname:    10.176.48.83
  Taints:             dedicated=edge:NoExecute
    Hostname:    10.184.58.7
  ```
  {: screen}

3. Si elige [habilitar la conservación de IP de origen para un servicio NLB 1.0](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations), asegúrese de que los pods de app estén planificados en los nodos trabajadores de extremo mediante la [adición de afinidad de nodo de extremo a los pods de app](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes). Los pods de app se deben planificar en los nodos de extremo para recibir solicitudes entrantes.

4. Para eliminar un antagonismo, ejecute el siguiente mandato.
    ```
    kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-
    ```
    {: pre}
