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


# Guía de aprendizaje: Utilización de políticas de red de Calico para bloquear el tráfico
{: #policy_tutorial}

De forma predeterminada, los servicios Kubernetes NodePort, LoadBalancer e Ingress hacen que la app esté disponible en todas las interfaces de red de clúster públicas y privadas. La política predeterminada de Calico `allow-node-port-dnat` permite el tráfico de entrada procedente de los servicios de NodePort, equilibrador de carga de red (NLB) y equilibrador de carga de aplicación (ALB) de Ingress a los pods de la app que dichos servicios exponen. Kubernetes utiliza la conversión de direcciones de red de destino (DNAT) para reenviar las solicitudes de servicio a los pods adecuados.
{: shortdesc}

Sin embargo, por motivos de seguridad, es posible que tenga que permitir el tráfico a los servicios de red solo desde determinadas direcciones IP de origen. Puede utilizar [políticas Pre-DNAT de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/getting-started/bare-metal/policy/pre-dnat) para crear una lista blanca o una lista negra del tráfico procedente o destinado a determinadas direcciones IP. Las políticas Pre-DNAT impiden que el tráfico especificado llegue a sus apps porque se aplican antes de que Kubernetes utilice DNAT normal para reenviar el tráfico a los pods. Cuando se crean políticas Pre-DNAT de Calico, se elige si las direcciones IP de origen deben estar en la lista blanca o en la lista negra. Para la mayoría de los casos, la lista blanca proporciona la configuración más segura porque todo el tráfico se bloquea excepto el tráfico de las direcciones IP de origen conocidas y permitidas. La creación de una lista negra suele ser útil solo en ciertos casos como, por ejemplo, para evitar un ataque desde un pequeño conjunto de direcciones IP.

En este caso de ejemplo, el usuario adopta el rol de administrador de una empresa de relaciones públicas (PR) y observa un tráfico poco habitual que afecta a sus apps. Las lecciones de esta guía de aprendizaje le guían a través de la creación de una app de servidor web de ejemplo, la exposición la app mediante un servicio equilibrador de carga de red (NLB) y protegiendo la app de tráfico no deseado con políticas de Calico de lista blanca y lista negra.

## Objetivos
{: #policies_objectives}

- Aprender a bloquear todo el tráfico entrante a todos los puertos de nodo creando una política de Pre-DNAT de orden superior.
- Aprender a permitir el acceso de las direcciones IP de origen de la lista blanca a la IP pública de NLB y al puerto creando una política Pre-DNAT de orden inferior. Las políticas de orden inferior tienen preferencia sobre las políticas de orden superior.
- Aprender a bloquear el acceso de las direcciones IP de origen de la lista negra a la IP pública de NLB y al puerto creando una política Pre-DNAT de orden inferior.

## Tiempo necesario
{: #policies_time}

1 hora

## Público
{: #policies_audience}

Esta guía de aprendizaje está destinada a los desarrolladores de software y administradores de la red que gestionan el tráfico de red a una app.

## Requisitos previos
{: #policies_prereqs}

- [Cree un clúster](/docs/containers?topic=containers-clusters#clusters_ui).
- [Defina su clúster como destino de la CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- [Instale y configure la CLI de Calico](/docs/containers?topic=containers-network_policies#cli_install).
- Asegúrese de tener las políticas de acceso de {{site.data.keyword.Bluemix_notm}} IAM siguientes para
{{site.data.keyword.containerlong_notm}}:
    - [Cualquier rol de plataforma](/docs/containers?topic=containers-users#platform)
    - [El rol de servicio de **Escritor** o de **Gestor**](/docs/containers?topic=containers-users#platform)

<br />


## Lección 1: Desplegar una app y exponerla mediante un NLB
{: #lesson1}

En la primera lección se muestra cómo se expone la app desde varias direcciones IP y puertos y de dónde procede el tráfico público que entra en el clúster.
{: shortdesc}

Empiece desplegando una app de servidor web de ejemplo para utilizarla en la guía de aprendizaje. El servidor web `echoserver` muestra datos sobre la conexión que se establece con el clúster desde el cliente y le deja probar el acceso al clúster de la empresa PR. A continuación, exponga la app creando un servicio de equilibrador de carga de red (NLB) 1.0. Un servicio de NLB 1.0 hace que la app esté disponible a través de la dirección IP de servicio del NLB y de los puertos de nodo de los nodos trabajadores.

¿Desea utilizar un equilibrador de carga de aplicación (ALB) de Ingress? En lugar de crear un NLB en los pasos 3 y 4, [cree un servicio para la app de servidor web](/docs/containers?topic=containers-ingress#public_inside_1) y [cree un recurso de Ingress para la app de servidor web](/docs/containers?topic=containers-ingress#public_inside_4). Luego obtenga las direcciones IP públicas de los ALB ejecutando `ibmcloud ks albs --cluster <cluster_name>` y utilice estas IP a lo largo de la guía de aprendizaje en lugar de la `<loadbalancer_IP>.`
{: tip}

En la siguiente imagen se muestra cómo la app de servidor web se expone a Internet mediante el puerto de nodo público y el NLB público al final de la Lección 1:

<img src="images/cs_tutorial_policies_Lesson1.png" width="450" alt="Al final de la Lección 1, la app de servidor web se expone en internet a través del puerto de internet y el NLB público." style="width:450px; border-style: none"/>

1. Despliegue la app de servidor web de ejemplo. Cuando se establece una conexión con la app de servidor web, la app responde con las cabeceras HTTP que ha recibido en la conexión.
    ```
    kubectl run webserver --image=k8s.gcr.io/echoserver:1.10 --replicas=3
    ```
    {: pre}

2. Verifique que los pods de la app de servidor web tienen en **STATUS** el valor `En ejecución`.
    ```
    kubectl get pods -o wide
    ```
    {: pre}

    Salida de ejemplo:
    ```
    NAME                         READY     STATUS    RESTARTS   AGE       IP               NODE
    webserver-855556f688-6dbsn   1/1       Running   0          1m        172.30.xxx.xxx   10.176.48.78
    webserver-855556f688-76rkp   1/1       Running   0          1m        172.30.xxx.xxx   10.176.48.78
    webserver-855556f688-xd849   1/1       Running   0          1m        172.30.xxx.xxx   10.176.48.78
    ```
    {: screen}

3. Para exponer la app a internet público, cree un archivo de configuración de servicio de NLB 1.0 denominado `webserver-lb.yaml` en un editor de texto.
    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: webserver
      name: webserver-lb
    spec:
      type: LoadBalancer
      selector:
        run: webserver
      ports:
      - name: webserver-port
        port: 80
        protocol: TCP
        targetPort: 8080
    ```
    {: codeblock}

4. Despliegue el NLB.
    ```
    kubectl apply -f filepath/webserver-lb.yaml
    ```
    {: pre}

5. Verifique que puede acceder públicamente a la app expuesta por el NLB desde el sistema.

    1. Obtenga la dirección **EXTERNAL-IP** pública del NLB.
        ```
        kubectl get svc -o wide
        ```
        {: pre}

        Salida de ejemplo:
        ```
        NAME           CLUSTER-IP       EXTERNAL-IP        PORT(S)        AGE       SELECTOR
        webserver-lb   172.21.xxx.xxx   169.xx.xxx.xxx     80:31024/TCP   2m        run=webserver
        ```
        {: screen}

    2. Cree un archivo de texto de hoja de apuntes y copie la IP del NLB en el archivo de texto. La hoja de apuntes le ayuda a utilizar valores rápidamente en las lecciones siguientes.

    3. Verifique que puede acceder públicamente a la IP externa correspondiente al NLB.
        ```
        curl --connect-timeout 10 <loadbalancer_IP>:80
        ```
        {: pre}

        La información de salida del siguiente ejemplo confirma que el NLB expone la app en la dirección IP pública del NLB `169.1.1.1`. El pod de la app `webserver-855556f688-76rkp` ha recibido la solicitud curl:
        ```
        Hostname: webserver-855556f688-76rkp
        Pod Information:
            -no pod information available-
        Server values:
            server_version=nginx: 1.13.3 - lua: 10008
        Request Information:
            client_address=1.1.1.1
            method=GET
            real path=/
            query=
            request_version=1.1
            request_scheme=http
            request_uri=http://169.1.1.1:8080/
        Request Headers:
            accept=*/*
            host=169.1.1.1
            user-agent=curl/7.54.0
        Request Body:
            -no body in request-
        ```
        {: screen}

6. Verifique que puede acceder públicamente a la app expuesta por el node desde el sistema. Un servicio de NLB hace que la app esté disponible a través de la dirección IP de servicio del NLB y de los puertos de nodo de los nodos trabajadores.

    1. Obtenga el puerto de nodo que el NLB ha asignado a los nodos trabajadores. El puerto de nodo se encuentra en el rango comprendido entre 30000 y 32767.
        ```
        kubectl get svc -o wide
        ```
        {: pre}

        En la siguiente información de salida de ejemplo, el puerto de nodo es `31024`:
        ```
        NAME           CLUSTER-IP       EXTERNAL-IP        PORT(S)        AGE       SELECTOR
        webserver-lb   172.21.xxx.xxx   169.xx.xxx.xxx     80:31024/TCP   2m        run=webserver
        ```
        {: screen}  

    2. Obtenga la dirección **IP pública** de un nodo trabajador.
        ```
        ibmcloud ks workers --cluster <cluster_name>
        ```
        {: pre}

        Salida de ejemplo:
        ```
        ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
        kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.xx.xxx.xxx   10.176.48.67   u3c.2x4.encrypted   normal   Ready    dal10   1.13.6_1513*   
        kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w2   169.xx.xxx.xxx   10.176.48.79   u3c.2x4.encrypted   normal   Ready    dal10   1.13.6_1513*   
        kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w3   169.xx.xxx.xxx   10.176.48.78   u3c.2x4.encrypted   normal   Ready    dal10   1.13.6_1513*   
        ```
        {: screen}

    3. Copie la IP pública del nodo trabajador y el puerto de nodo en la hoja de apuntes de texto para utilizarla en lecciones posteriores.

    4. Verifique que puede acceder a la dirección IP pública del nodo trabajador a través del puerto de nodo.
        ```
        curl  --connect-timeout 10 <worker_IP>:<NodePort>
        ```
        {: pre}

        La información de salida de ejemplo siguiente confirma que la solicitud a la app se ha recibido a través de la dirección IP privada `10.1.1.1` para el nodo trabajador y el puerto de nodo `31024`. El pod de la app `webserver-855556f688-xd849` ha recibido la solicitud curl:
        ```
        Hostname: webserver-855556f688-xd849
        Pod Information:
            -no pod information available-
        Server values:
            server_version=nginx: 1.13.3 - lua: 10008
        Request Information:
            client_address=1.1.1.1
            method=GET
            real path=/
            query=
            request_version=1.1
            request_scheme=http
            request_uri=http://10.1.1.1:8080/
        Request Headers:
            accept=*/*
            host=10.1.1.1:31024
            user-agent=curl/7.60.0
        Request Body:
            -no body in request-
        ```
        {: screen}

En este punto, la app está expuesta desde varias direcciones IP y puertos. La mayoría de estas direcciones IP son internas del clúster y solo se puede acceder a ellas a través de la red privada. Solo el puerto de nodo público y el puerto del NLB público están expuestos a Internet público.

A continuación, puede empezar a crear y aplicar políticas de Calico para bloquear el tráfico público.

## Lección 2: Bloquear todo el tráfico de entrada a todos los puertos de nodo
{: #lesson2}

Para proteger el clúster de la empresa PR, debe bloquear el acceso público tanto al servicio de NLB como a los puertos de nodo que exponen la app. Empiece bloqueando el acceso a los puertos de nodo.
{: shortdesc}

En la imagen siguiente se muestra cómo se permite el tráfico al NLB, pero no a los puertos de nodo al final de la Lección 2:

<img src="images/cs_tutorial_policies_Lesson2.png" width="425" alt="Al final de la Lección 2, la app de servidor web quedará expuesta a Internet solo mediante el NLB público." style="width:425px; border-style: none"/>

1. En un editor de texto, cree una política Pre-DNAT de orden superior denominada `deny-nodeports.yaml` para denegar el tráfico TCP y UDP entrante procedente de cualquier IP de origen a todos los puertos de nodo.
    ```
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: deny-nodeports
    spec:
      applyOnForward: true
      preDNAT: true
      ingress:
      - action: Deny
            destination:
              ports:
          - 30000:32767
            protocol: TCP
            source: {}
      - action: Deny
            destination:
              ports:
          - 30000:32767
        protocol: UDP
        source: {}
      selector: ibm.role=='worker_public'
      order: 1100
      types:
      - Ingress
    ```
    {: codeblock}

2. Aplique la política.
    - Linux:

      ```
      calicoctl apply -f filepath/deny-nodeports.yaml
      ```
      {: pre}

    - Windows y OS X:

      ```
      calicoctl apply -f filepath/deny-nodeports.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}
  Salida de ejemplo:
  ```
  Successfully applied 1 'GlobalNetworkPolicy' resource(s)
  ```
  {: screen}

3. Utilizando los valores de la hoja de apuntes, verifique que no puede acceder públicamente a la dirección IP pública del nodo trabajador y al puerto de nodo.
    ```
    curl  --connect-timeout 10 <worker_IP>:<NodePort>
    ```
    {: pre}

    La conexión excede el tiempo de espera porque la política de Calico que ha creado está bloqueando el tráfico a los puertos de nodo.
    ```
    curl: (28) Connection timed out after 10016 milliseconds
    ```
    {: screen}

4. Cambie el valor externalTrafficPolicy de LoadBalancer que ha creado en la lección anterior, `Cluster`, por `Local`. `Local` garantiza que la IP de origen del sistema se conserva cuando ejecuta curl sobre la dirección IP externa de LoadBalancer en el paso siguiente.
    ```
    kubectl patch svc webserver -p '{"spec":{"externalTrafficPolicy":"Local"}}'
    ```
    {: pre}

5. Utilizando el valor de la hoja de apuntes, verifique que aún puede acceder públicamente a la dirección IP externa del NLB.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}

    Salida de ejemplo:
    ```
    Hostname: webserver-855556f688-76rkp
    Pod Information:
        -no pod information available-
    Server values:
        server_version=nginx: 1.13.3 - lua: 10008
    Request Information:
        client_address=1.1.1.1
        method=GET
        real path=/
        query=
        request_version=1.1
        request_scheme=http
        request_uri=http://<loadbalancer_IP>:8080/
    Request Headers:
        accept=*/*
        host=<loadbalancer_IP>
        user-agent=curl/7.54.0
    Request Body:
        -no body in request-
    ```
    {: screen}
    En la sección `Request Information` de la información de salida, la dirección IP de origen es, por ejemplo, `client_address=1.1.1.1`. La dirección IP de origen es la dirección IP pública del sistema que está utilizando para ejecutar curl. De lo contrario, si se está conectando a internet a través de un proxy o VPN, es posible que el proxy o VPN esté ocultando la dirección IP real del sistema. En cualquier caso, el servicio de NLB ve la dirección IP de origen del sistema como la dirección IP del cliente.

6. Copie la dirección IP de origen del sistema (`client_address=1.1.1.1` en la información de salida del paso anterior) en la hoja de apuntes para utilizarla en lecciones posteriores.

¡Estupendo! En este punto, la app está expuesta a internet público solo desde el puerto público del NLB. El tráfico a los puertos de nodo públicos está bloqueado. El clúster se bloquea parcialmente ante tráfico no deseado.

A continuación, puede crear y aplicar políticas de Calico al tráfico de la lista blanca desde determinadas IP de origen.

## Lección 3: Permitir el tráfico de entrada desde una IP de la lista blanca al NLB
{: #lesson3}

Supongamos que ahora decide bloquear por completo el tráfico al clúster de la empresa PR y probar el acceso colocando en la lista blanca la dirección IP de su propio sistema.
{: shortdesc}

En primer lugar, además de los puertos de nodo, debe bloquear todo el tráfico entrante al NLB que expone la app. A continuación, puede crear una política que contenga la dirección IP de su sistema en la lista blanca. Al final de la Lección 3, todo el tráfico dirigido a los puertos de nodo públicos y al NLB se bloqueará y solo se permitirá el tráfico desde la IP del sistema de la lista blanca:

<img src="images/cs_tutorial_policies_L3.png" width="550" alt="La app de servidor web se expone sólo mediante el NLB público a la IP del sistema." style="width:500px; border-style: none"/>

1. En un editor de texto, cree una política Pre-DNAT de orden superior denominada `deny-lb-port-80.yaml` para denegar todo el tráfico TCP y UDP entrante procedente de cualquier IP de origen a la dirección IP y puerto del NLB. Sustituya `<loadbalancer_IP>` por la dirección IP pública del NLB de la hoja de apuntes.

    ```
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: deny-lb-port-80
    spec:
      applyOnForward: true
      preDNAT: true
      ingress:
      - action: Deny
        destination:
          nets:
          - <loadbalancer_IP>/32
          ports:
          - 80
        protocol: TCP
        source: {}
      - action: Deny
        destination:
          nets:
          - <loadbalancer_IP>/32
          ports:
          - 80
        protocol: UDP
        source: {}
      selector: ibm.role=='worker_public'
      order: 800
      types:
      - Ingress
    ```
    {: codeblock}

2. Aplique la política.
    - Linux:

      ```
      calicoctl apply -f filepath/deny-lb-port-80.yaml
      ```
      {: pre}

    - Windows y OS X:

      ```
      calicoctl apply -f filepath/deny-lb-port-80.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

3. Utilizando el valor de la hoja de apuntes, verifique que ahora no puede acceder a la dirección IP pública del NLB. La conexión excede el tiempo de espera porque la política de Calico que ha creado está bloqueando el tráfico hacia el NLB.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}

4. En un editor de texto, cree una política de Pre-DNAT de orden inferior denominada `whitelist.yaml` para permitir el tráfico desde la IP del sistema a la dirección IP y puerto del NLB. Utilizando los valores de la hoja de apuntes, sustituya `<loadbalancer_IP>` por la dirección IP pública del NLB y `<client_address>` por la dirección IP pública de la IP de origen de su sistema. Si no recuerda la IP del sistema, puede ejecutar `curl ifconfig.co`.
    ```
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: whitelist
    spec:
      applyOnForward: true
      preDNAT: true
      ingress:
      - action: Allow
        destination:
          nets:
          - <loadbalancer_IP>/32
          ports:
          - 80
        protocol: TCP
        source:
          nets:
          - <client_address>/32
      selector: ibm.role=='worker_public'
      order: 500
      types:
      - Ingress
    ```
    {: codeblock}

5. Aplique la política.
    - Linux:

      ```
      calicoctl apply -f filepath/whitelist.yaml
      ```
      {: pre}

    - Windows y OS X:

      ```
      calicoctl apply -f filepath/whitelist.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}
  Ahora la dirección IP de su sistema está en la lista blanca.

6. Utilizando el valor de la hoja de apuntes, verifique que ahora puede acceder a la dirección IP pública del NLB.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}

7. Si tiene acceso a otro sistema que tenga otra dirección IP, intente acceder al NLB desde dicho sistema.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}
    La conexión excede el tiempo de espera porque la dirección IP del sistema no está en la lista blanca.

En este punto, todo el tráfico dirigido al NLB y a los puertos de nodo públicos está bloqueado. Solo se permite el tráfico procedente de la IP del sistema de la lista blanca.

## Lección 4: Denegar el tráfico de entrada al NLB procedente de las IP de la lista negra
{: #lesson4}

En la lección anterior, ha bloqueado todo el tráfico y solo ha colocado en la lista blanca unas pocas IP. Este caso de ejemplo funciona bien para fines de prueba cuando se desea limitar el acceso a unas pocas direcciones IP de origen controladas. Sin embargo, la empresa PR tiene apps que tienen que estar ampliamente disponibles para el público. Debe asegurarse de que se permita todo el tráfico, excepto el tráfico inusual que ve desde unas pocas direcciones IP. La creación de una lista negra resulta útil en un escenario como este, porque puede ayudarle a evitar un ataque desde un pequeño conjunto de direcciones IP.
{: shortdesc}

En esta lección, probará la creación de una lista negra que bloquee el tráfico procedente de la dirección IP de origen de su propio sistema. Al final de la Lección 4, todo el tráfico dirigido a los puertos de nodo públicos se bloqueará y se permitirá todo el tráfico dirigido al NLB. Solo se bloqueará el tráfico procedente de la IP del sistema en la lista negra al NLB:

<img src="images/cs_tutorial_policies_L4.png" width="550" alt="La app de webserver se expone a internet mediante el NLB público. Solo se bloquea el tráfico procedente de la IP del sistema." style="width:550px; border-style: none"/>

1. Limpie las políticas de la lista blanca que ha creado en la lección anterior.
    - Linux:
      ```
      calicoctl delete GlobalNetworkPolicy deny-lb-port-80
      ```
      {: pre}
      ```
      calicoctl delete GlobalNetworkPolicy whitelist
      ```
      {: pre}

    - Windows y OS X:
      ```
      calicoctl delete GlobalNetworkPolicy deny-lb-port-80 --config=filepath/calicoctl.cfg
      ```
      {: pre}
      ```
      calicoctl delete GlobalNetworkPolicy whitelist --config=filepath/calicoctl.cfg
      ```
      {: pre}

    Ahora, todo el tráfico TCP y UDP entrante procedente cualquier IP de origen a la dirección IP y puerto del NLB vuelve a estar permitido.

2. Para denegar todo el tráfico TCP y UDP de entrada procedente de la dirección IP de origen de su sistema a la dirección IP y puerto del NLB, cree una política pre-DNAT de orden inferior denominada `blacklist.yaml` en un editor de texto. Utilizando los valores de la hoja de apuntes, sustituya `<loadbalancer_IP>` por la dirección IP pública del NLB y `<client_address>` por la dirección IP pública de la IP de origen de su sistema.
  ```
  apiVersion: projectcalico.org/v3
  kind: GlobalNetworkPolicy
  metadata:
    name: blacklist
  spec:
    applyOnForward: true
    preDNAT: true
    ingress:
    - action: Deny
      destination:
        nets:
        - <loadbalancer_IP>/32
        ports:
        - 80
      protocol: TCP
      source:
        nets:
        - <client_address>/32
    - action: Deny
      destination:
        nets:
        - <loadbalancer_IP>/32
        ports:
        - 80
      protocol: UDP
      source:
        nets:
        - <client_address>/32
    selector: ibm.role=='worker_public'
    order: 500
    types:
    - Ingress
  ```
  {: codeblock}

3. Aplique la política.
    - Linux:

      ```
      calicoctl apply -f filepath/blacklist.yaml
      ```
      {: pre}

    - Windows y OS X:

      ```
      calicoctl apply -f filepath/blacklist.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}
  Ahora la dirección IP de su sistema está en la lista negra.

4. Utilizando el valor de la hoja de apuntes, verifique desde su sistema que no puede acceder a la dirección IP del NLB porque la IP de su sistema está en la lista negra.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}
    En este punto, todo el tráfico dirigido a los puertos de nodo públicos está bloqueado y se permite todo el tráfico dirigido al NLB público. Solo se bloquea el tráfico procedente de la IP del sistema en la lista negra al NLB.

¡Buen trabajo! Ha controlado correctamente el tráfico de entrada en la app utilizando las políticas Pre-DNAT de Calico para crear listas negras de direcciones IP de origen.

## Lección 5: Registrar tráfico bloqueado desde las IP de la lista negra al NLB
{: #lesson5}

En la lección anterior, ha incluido en la lista negra el tráfico desde la dirección IP del sistema al NLB. En esta lección puede aprender a registrar las solicitudes de tráfico denegadas.
{: shortdesc}

En nuestro ejemplo de ejemplo, la empresa PR donde trabaja quiere configurar un seguimiento de registro para cualquier tráfico poco habitual que una de las políticas de red deniega continuamente. Para supervisar la amenaza de seguridad potencial, configura el registro para que registre todas las veces que la política de lista negra deniega un intento de acción en la IP de NLB.

1. Cree una NetworkPolicy de Calico denominada `log-denied-packets`. Esta política de registro utiliza el mismo selector que la política `blacklist`, que añade esta política a la cadena de reglas de Iptables de Calico. Si utiliza un número de orden inferior, como por ejemplo `300`, puede asegurarse de que esta regla se añade a la cadena de reglas de Iptables antes de la política de lista negra. Los paquetes procedentes de su IP los registra esta política antes de que intenten coincidir con la regla de política `blacklist` y sean denegados.
  ```
  apiVersion: projectcalico.org/v3
  kind: GlobalNetworkPolicy
  metadata:
    name: log-denied-packets
  spec:
    applyOnForward: true
    preDNAT: true
    ingress:
    - action: Log
      destination:
        nets:
        - <loadbalancer_IP>/32
        ports:
        - 80
      protocol: TCP
      source:
        nets:
        - <client_address>/32
    - action: Deny
      destination:
        nets:
        - <loadbalancer_IP>/32
        ports:
        - 80
      protocol: UDP
      source:
        nets:
        - <client_address>/32
    selector: ibm.role=='worker_public'
    order: 300
    types:
    - Ingress
  ```
  {: codeblock}

2. Aplique la política.
  ```
  calicoctl apply -f log-denied-packets.yaml --config=<filepath>/calicoctl.cfg
  ```
  {: pre}

3. Genere entradas de registro enviando solicitudes de la IP del sistema a la IP de NLB. Estos paquetes de solicitud se registran antes de ser denegados.
  ```
  curl --connect-timeout 10 <loadbalancer_IP>:80
  ```
  {: pre}

4. Busque entradas de registro escritas en la vía de acceso `/var/log/syslog`. La entrada de registro se parecerá a lo siguiente.
  ```
  Sep 5 14:34:40 <worker_hostname> kernel: [158271.044316] calico-packet: IN=eth1 OUT= MAC=08:00:27:d5:4e:57:0a:00:27:00:00:00:08:00 SRC=192.XXX.XX.X DST=192.XXX.XX.XX LEN=60 TOS=0x00 PREC=0x00 TTL=64 ID=52866 DF PROTO=TCP SPT=42962 DPT=22 WINDOW=29200 RES=0x00 SYN URGP=0
  ```
  {: screen}

¡Bien! Ha configurado el registro de forma que se pueda supervisar más fácilmente el tráfico de lista negra.

Si desea limpiar la lista negra y las políticas de registro:
1. Limpie la política de lista negra.
    - Linux:
      ```
      calicoctl delete GlobalNetworkPolicy blacklist
      ```
      {: pre}

    - Windows y OS X:
      ```
      calicoctl delete GlobalNetworkPolicy blacklist --config=filepath/calicoctl.cfg
      ```
      {: pre}

2. Limpie la política de registro.
    - Linux:
      ```
      calicoctl delete GlobalNetworkPolicy log-denied-packets
      ```
      {: pre}

    - Windows y OS X:
      ```
      calicoctl delete GlobalNetworkPolicy log-denied-packets --config=filepath/calicoctl.cfg
      ```
      {: pre}

## ¿Qué es lo siguiente?
{: #whats_next}

* Obtenga más información sobre [cómo controlar el tráfico con políticas de red](/docs/containers?topic=containers-network_policies).
* Para ver más ejemplos de políticas de red de Calico que controlan el tráfico procedente y destinado a su clúster, puede consultar la [demostración de política inicial ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/stars-policy/) y la [política de red avanzada ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/advanced-policy).
