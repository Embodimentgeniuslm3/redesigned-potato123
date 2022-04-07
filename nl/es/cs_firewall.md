---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

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



# Apertura de los puertos y direcciones IP necesarios en el cortafuegos
{: #firewall}

Revise estas situaciones en las que puede tener que abrir puertos específicos y direcciones IP en el cortafuegos para {{site.data.keyword.containerlong}}.
{:shortdesc}

* [Para ejecutar mandatos `ibmcloud` e `ibmcloud ks`](#firewall_bx) desde el sistema local cuando las políticas de red corporativas impiden el acceso a los puntos finales de internet pública mediante proxies o cortafuegos.
* [Para ejecutar mandatos `kubectl`](#firewall_kubectl) desde el sistema local cuando las políticas de red corporativas impiden el acceso a los puntos finales de internet pública mediante proxies o cortafuegos.
* [Para ejecutar mandatos `calicoctl`](#firewall_calicoctl) desde el sistema local cuando las políticas de red corporativas impiden el acceso a los puntos finales de internet pública mediante proxies o cortafuegos.
* [Para permitir la comunicación entre el maestro de Kubernetes y los nodos trabajadores](#firewall_outbound) cuando hay un cortafuegos configurado para los nodos trabajadores o los valores del cortafuegos están personalizados en su cuenta de infraestructura de IBM Cloud (SoftLayer).
* [Para permitir que el clúster acceda a los recursos a través de un cortafuegos en la red privada](#firewall_private).
* [Para permitir que el clúster acceda a los recursos cuando las políticas de red de Calico bloque la salida del nodo trabajador](#firewall_calico_egress).
* [Para acceder al servicio NodePort, equilibrador de carga o Ingress desde fuera del clúster](#firewall_inbound).
* [Para permitir al clúster acceder a servicios que se ejecutan dentro o fuera de {{site.data.keyword.Bluemix_notm}} o que son locales y están protegidos por un cortafuegos](#whitelist_workers).

<br />


## Ejecución de mandatos `ibmcloud` e `ibmcloud ks` desde detrás de un cortafuegos
{: #firewall_bx}

Si las políticas de red corporativas impiden el acceso desde el sistema local a los puntos finales públicos mediante proxies o cortafuegos, para ejecutar mandatos `ibmcloud` e `ibmcloud ks` debe permitir el acceso TCP para {{site.data.keyword.Bluemix_notm}} y {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

1. Permita el acceso a `cloud.ibm.com` en el puerto 443 del cortafuegos.
2. Verifique la conexión iniciando una sesión en {{site.data.keyword.Bluemix_notm}} a través de este punto final de API.
  ```
  ibmcloud login -a https://cloud.ibm.com/
  ```
  {: pre}
3. Permita el acceso a `containers.cloud.ibm.com` en el puerto 443 del cortafuegos.
4. Compruebe la conexión. Si el acceso está configurado correctamente, los envíos se visualizan en la salida.
   ```
   curl https://containers.cloud.ibm.com/v1/
   ```
   {: pre}

   Salida de ejemplo:
   ```
                                     )___(
                              _______/__/_
                     ___     /===========|   ___
    ____       __   [\\\]___/____________|__[///]   __
    \   \_____[\\]__/___________________________\__[//]___
     \                                                    |
      \                                                  /
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ```
   {: screen}

<br />


## Ejecución de mandatos `kubectl` desde detrás de un cortafuegos
{: #firewall_kubectl}

Si las políticas de red corporativas impiden el acceso desde el sistema local a los puntos finales públicos mediante proxies o cortafuegos, para ejecutar mandatos `kubectl` debe permitir el acceso TCP para el clúster.
{:shortdesc}

Cuando se crea un clúster, el puerto de los URL de punto final de servicio se asignan aleatoriamente entre 20000 y 32767. Puede elegir abrir el rango de puerto 20000-32767 para cualquier clúster que se pueda crear o permitir el acceso a un clúster existente específico.

Antes de empezar, permita el acceso para [ejecutar mandatos `ibmcloud ks`](#firewall_bx).

Para permitir el acceso a un clúster específico:

1. Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Escriba
sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite. Si tiene una cuenta federada, incluya la opción `--sso`.

   ```
   ibmcloud login [--sso]
   ```
   {: pre}

2. Si el clúster está en un grupo de recursos distinto de `default`, elija como destino dicho grupo de recursos. Para ver el grupo de recursos al que pertenece cada clúster, ejecute `ibmcloud ks clusters`. **Nota**: Debe tener al menos el [rol de **Visor**](/docs/containers?topic=containers-users#platform) sobre el grupo de recursos.
   ```
   ibmcloud target -g <resource_group_name>
   ```
   {: pre}

4. Obtenga el nombre del clúster.

   ```
   ibmcloud ks clusters
   ```
   {: pre}

5. Recupere los URL de punto final de servicio para el clúster.
 * Si solo está especificado el **URL de punto final de servicio público**, obtenga este URL. Los usuarios autorizados del clúster pueden acceder al nodo maestro de Kubernetes a través de este punto final en la red pública.
 * Si solo está especificado el **URL de punto final de servicio privado**, obtenga este URL. Los usuarios autorizados del clúster pueden acceder al nodo maestro de Kubernetes a través de este punto final en la red privada.
 * Si tanto el **URL de punto final de servicio público** como el **URL de punto final de servicio privado** están especificados, obtenga los dos URL. Los usuarios autorizados del clúster pueden acceder al nodo maestro de Kubernetes a través del punto final público de la red pública o del punto final privado de la red privada.

  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ...
  Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
  Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
  ...
  ```
  {: screen}

6. Permita el acceso a los URL de punto final de servicio y a los puertos que ha obtenido en el paso anterior. Si el cortafuegos está basado en IP, puede consultar las direcciones IP que están abiertas cuando se permite el acceso a los URL de punto final de servicio en [esta tabla](#master_ips).

7. Compruebe la conexión.
  * Si el punto final de servicio público está habilitado:
    ```
    curl --insecure <public_service_endpoint_URL>/version
    ```
    {: pre}

    Mandato de ejemplo:
    ```
    curl --insecure https://c3.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    Salida de ejemplo:
    ```
    {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
    ```
    {: screen}
  * Si el punto final de servicio privado está habilitado, debe estar en la red privada de {{site.data.keyword.Bluemix_notm}} o debe conectarse a la red privada a través de una conexión VPN para verificar la conexión con el nodo maestro. Tenga en cuenta que debe [exponer el punto final maestro a través de un equilibrador de carga privado](/docs/containers?topic=containers-clusters#access_on_prem) para que los usuarios puedan acceder al nodo maestro a través de una conexión VPN o {{site.data.keyword.BluDirectLink}}.
    ```
    curl --insecure <private_service_endpoint_URL>/version
    ```
    {: pre}

    Mandato de ejemplo:
    ```
    curl --insecure https://c3-private.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    Salida de ejemplo:
    ```
    {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
    ```
    {: screen}

8. Opcional: repita estos pasos para cada clúster que necesite exponer.

<br />


## Ejecución de mandatos `calicoctl` desde detrás de un cortafuegos
{: #firewall_calicoctl}

Si las políticas de red corporativas impiden el acceso desde el sistema local a los puntos finales públicos mediante proxies o cortafuegos, para ejecutar mandatos `calicoctl` debe permitir el acceso TCP para los mandatos Calico.
{:shortdesc}

Antes de empezar, permita el acceso para ejecutar mandatos [`ibmcloud`](#firewall_bx) y mandatos [`kubectl`](#firewall_kubectl).

1. Recupere la dirección IP del URL maestro que ha utilizado para permitir los mandatos [`kubectl`](#firewall_kubectl).

2. Obtenga el puerto para etcd.

  ```
  kubectl get cm -n kube-system cluster-info -o yaml | grep etcd_host
  ```
  {: pre}

3. Permita el acceso para las políticas de Calico mediante la dirección IP del URL maestro y el puerto etcd.

<br />


## Cómo permitir al clúster acceder a recursos de infraestructura y otros servicios a través de un cortafuegos público
{: #firewall_outbound}

Permita que el clúster acceda a servicios y recursos de infraestructura desde detrás de un cortafuegos público, como por ejemplo para las regiones de {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.registrylong_notm}}, {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM), {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, IP privadas de infraestructura de IBM Cloud (SoftLayer) y de salida para reclamaciones de volumen persistente.
{:shortdesc}

En función de la configuración del clúster, puede acceder a los servicios utilizando las direcciones IP públicas, privadas o ambas direcciones IP. Si tiene un clúster con nodos trabajadores en las VLAN públicas y privadas detrás de un cortafuegos para redes tanto públicas como privadas, debe abrir la conexión para direcciones IP públicas y privadas. Si el clúster solo tiene nodos trabajadores en la VLAN privada detrás de un cortafuegos, puede abrir la conexión solo a las direcciones IP privadas.
{: note}

1.  Anote la dirección IP pública de todos sus nodos trabajadores del clúster.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

2.  Permita el tráfico de red de salida del origen <em>&lt;each_worker_node_publicIP&gt;</em> al rango de puertos TCP/UDP de destino 20000-32767 y al puerto 443, y a las siguientes direcciones IP y grupos de red. Estas direcciones IP permiten que los nodos trabajadores se comuniquen con el nodo maestro del clúster. Si tiene un cortafuegos corporativo que impide a la máquina local acceder a puntos finales de Internet pública, realice este paso también para la máquina local para poder acceder al nodo maestro del clúster.

    También debe permitir el tráfico de salida al puerto 443 y a todas las zonas entre una región y otra para equilibrar la carga durante el proceso de arranque. Por ejemplo, si el clúster está en EE. UU. sur, debe permitir el tráfico procedente desde las IP públicas de cada uno de los nodos trabajadores al puerto 443 de la dirección IP para todas las zonas.
    {: important}

    {: #master_ips}
    <table summary="La primera fila de la tabla abarca ambas columnas. El resto de las filas deben leerse de izquierda a derecha, con la zona de servidor en la columna una y las direcciones IP coincidentes en la columna dos.">
      <caption>Direcciones IP para abrir para el tráfico saliente</caption>
          <thead>
          <th>Región</th>
          <th>Zona</th>
          <th>Dirección IP pública</th>
          <th>Dirección IP privada</th>
          </thead>
        <tbody>
          <tr>
            <td>AP norte</td>
            <td>che01<br>hkg02<br>seo01<br>sng01<br><br>tok02, tok04, tok05</td>
            <td><code>169.38.70.10</code><br><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><br><code>161.202.126.210, 128.168.71.117, 165.192.69.69</code></td>
            <td><code>166.9.40.7</code><br><code>166.9.42.7</code><br><code>166.9.44.5</code><br><code>166.9.40.8</code><br><br><code>166.9.40.6, 166.9.42.6, 166.9.44.4</code></td>
           </tr>
          <tr>
             <td>AP sur</td>
             <td>mel01<br><br>syd01, syd04, syd05</td>
             <td><code>168.1.97.67</code><br><br><code>168.1.8.195, 130.198.66.26, 168.1.12.98, 130.198.64.19</code></td>
             <td><code>166.9.54.10</code><br><br><code>166.9.52.14, 166.9.52.15, 166.9.54.11, 166.9.54.13, 166.9.54.12</code></td>
          </tr>
          <tr>
             <td>UE central</td>
             <td>ams03<br>mil01<br>osl01<br>par01<br><br>fra02, fra04, fra05</td>
             <td><code>169.50.169.110, 169.50.154.194</code><br><code>159.122.190.98, 159.122.141.69</code><br><code>169.51.73.50</code><br><code>159.8.86.149, 159.8.98.170</code><br><br><code>169.50.56.174, 161.156.65.42, 149.81.78.114</code></td>
             <td><code>166.9.28.17, 166.9.30.11</code><br><code>166.9.28.20, 166.9.30.12</code><br><code>166.9.32.8</code><br><code>166.9.28.19, 166.9.28.22</code><br><br><code>	166.9.28.23, 166.9.30.13, 166.9.32.9</code></td>
            </tr>
          <tr>
            <td>RU sur</td>
            <td>lon02, lon04, lon05, lon06</td>
            <td><code>159.122.242.78, 158.175.111.42, 158.176.94.26, 159.122.224.242, 158.175.65.170, 158.176.95.146</code></td>
            <td><code>166.9.34.5, 166.9.34.6, 166.9.36.10, 166.9.36.11, 166.9.36.12, 166.9.36.13, 166.9.38.6, 166.9.38.7</code></td>
          </tr>
          <tr>
            <td>EE. UU. este</td>
             <td>mon01<br>tor01<br><br>wdc04, wdc06, wdc07</td>
             <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><br><code>169.63.88.186, 169.60.73.142, 169.61.109.34, 169.63.88.178, 169.60.101.42, 169.61.83.62</code></td>
             <td><code>166.9.20.11</code><br><code>166.9.22.8</code><br><br><code>166.9.20.12, 166.9.20.13, 166.9.22.9, 166.9.22.10, 166.9.24.4, 166.9.24.5</code></td>
          </tr>
          <tr>
            <td>EE. UU. sur</td>
            <td>hou02<br>mex01<br>sao01<br>sjc03<br>sjc04<br><br>dal10,dal12,dal13</td>
            <td><code>184.173.44.62</code><br><code>169.57.100.18</code><br><code>169.57.151.10</code><br><code>169.45.67.210</code><br><code>169.62.82.197</code><br><br><code>169.46.7.238, 169.48.230.146, 169.61.29.194, 169.46.110.218, 169.47.70.10, 169.62.166.98, 169.48.143.218, 169.61.177.2, 169.60.128.2</code></td>
            <td><code>166.9.15.74</code><br><code>166.9.15.76</code><br><code>166.9.12.143</code><br><code>166.9.12.144</code><br><code>166.9.15.75</code><br><br><code>166.9.12.140, 166.9.12.141, 166.9.12.142, 166.9.15.69, 166.9.15.70, 166.9.15.72, 166.9.15.71, 166.9.15.73, 166.9.16.183, 166.9.16.184, 166.9.16.185</code></td>
          </tr>
          </tbody>
        </table>

3.  {: #firewall_registry}Para permitir que los nodos trabajadores se comuniquen con {{site.data.keyword.registrylong_notm}}, permita el tráfico de salida procedente de los nodos trabajadores a las [regiones de {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-registry_overview#registry_regions):
  - `TCP port 443, port 4443 FROM <each_worker_node_publicIP> TO <registry_subnet>`
  -  Sustituya <em>&lt;registry_subnet&gt;</em> por la subred de registro a la que desea permitir el tráfico. El registro global almacena imágenes públicas proporcionadas por IBM y los registros regionales almacenan sus propias imágenes privadas o públicas. El puerto 4443 se necesita para funciones de administración, como [verificar firmas de imágenes](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent). <table summary="La primera fila de la tabla abarca ambas columnas. El resto de las filas deben leerse de izquierda a derecha, con la zona de servidor en la columna una y las direcciones IP coincidentes en la columna dos.">
  <caption>Direcciones IP para abrir para el tráfico de registro</caption>
    <thead>
      <th>Región de {{site.data.keyword.containerlong_notm}}</th>
      <th>Dirección de registro</th>
      <th>Subredes públicas de registro</th>
      <th>Direcciones IP privadas de registro</th>
    </thead>
    <tbody>
      <tr>
        <td>Registro global a través de regiones de <br>{{site.data.keyword.containerlong_notm}}</td>
        <td><code>icr.io</code><br><br>
        En desuso: <code>registry.bluemix.net</code></td>
        <td><code>169.60.72.144/28</code></br><code>169.61.76.176/28</code></br><code>169.62.37.240/29</code></br><code>169.60.98.80/29</code></br><code>169.63.104.232/29</code></td>
        <td><code>166.9.20.4</code></br><code>166.9.22.3</code></br><code>166.9.24.2</code></td>
      </tr>
      <tr>
        <td>AP norte</td>
        <td><code>jp.icr.io</code><br><br>
        En desuso: <code>registry.au-syd.bluemix.net</code></td>
        <td><code>161.202.146.86/29</code></br><code>128.168.71.70/29</code></br><code>165.192.71.222/29</code></td>
        <td><code>166.9.40.3</code></br><code>166.9.42.3</code></br><code>166.9.44.3</code></td>
      </tr>
      <tr>
        <td>AP sur</td>
        <td><code>au.icr.io</code><br><br>
        En desuso: <code>registry.au-syd.bluemix.net</code></td>
        <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></br><code>168.1.1.240/29</code></br><code>130.198.88.128/29</code></br><code>135.90.66.48/29</code></td>
        <td><code>166.9.52.2</code></br><code>166.9.54.2</code></br><code>166.9.56.3</code></td>
      </tr>
      <tr>
        <td>UE central</td>
        <td><code>de.icr.io</code><br><br>
        En desuso: <code>registry.eu-de.bluemix.net</code></td>
        <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></br><code>169.50.58.104/29</code></br><code>161.156.93.16/29</code></br><code>149.81.79.152/29</code></td>
        <td><code>166.9.28.12</code></br><code>166.9.30.9</code></br><code>166.9.32.5</code></td>
       </tr>
       <tr>
        <td>RU sur</td>
        <td><code>uk.icr.io</code><br><br>
        En desuso: <code>registry.eu-gb.bluemix.net</code></td>
        <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></br><code>158.175.97.184/29</code></br><code>158.176.105.64/29</code></br><code>141.125.71.136/29</code></td>
        <td><code>166.9.36.9</code></br><code>166.9.38.5</code></br><code>166.9.34.4</code></td>
       </tr>
       <tr>
        <td>EE. UU. este, EE. UU. sur</td>
        <td><code>us.icr.io</code><br><br>
        En desuso: <code>registry.ng.bluemix.net</code></td>
        <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></br><code>169.61.234.224/29</code></br><code>169.61.135.160/29</code></br><code>169.61.46.80/29</code></td>
        <td><code>166.9.12.114</code></br><code>166.9.15.50</code></br><code>166.9.16.173</code></td>
       </tr>
      </tbody>
    </table>

4. Opcional: Permita el tráfico de red de salida de los nodos trabajadores a los servicios {{site.data.keyword.monitoringlong_notm}},
{{site.data.keyword.loganalysislong_notm}}, Sysdig y LogDNA:
    *   **{{site.data.keyword.monitoringlong_notm}}**:
        <pre class="screen">TCP port 443, port 9095 FROM &lt;each_worker_node_public_IP&gt; TO &lt;monitoring_subnet&gt;</pre>
        Sustituya <em>&lt;monitoring_subnet&gt;</em> por las subredes correspondientes a las regiones de supervisión a las que desea permitir el tráfico:
        <p><table summary="La primera fila de la tabla abarca ambas columnas. El resto de las filas deben leerse de izquierda a derecha, con la zona de servidor en la columna una y las direcciones IP coincidentes en la columna dos.">
  <caption>Direcciones IP para abrir para el tráfico de supervisión</caption>
        <thead>
        <th>Región de {{site.data.keyword.containerlong_notm}}</th>
        <th>Dirección de supervisión</th>
        <th>Subredes de supervisión</th>
        </thead>
      <tbody>
        <tr>
         <td>UE central</td>
         <td><code>metrics.eu-de.bluemix.net</code></td>
         <td><code>158.177.65.80/30</code></td>
        </tr>
        <tr>
         <td>RU sur</td>
         <td><code>metrics.eu-gb.bluemix.net</code></td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>EE. UU. este, EE. UU. sur, AP norte, AP sur</td>
          <td><code>metrics.ng.bluemix.net</code></td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
    *   **{{site.data.keyword.mon_full_notm}}**:
        <pre class="screen">TCP port 443, port 6443 FROM &lt;each_worker_node_public_IP&gt; TO &lt;sysdig_public_IP&gt;</pre>
        Sustituya `<sysdig_public_IP>` por las [direcciones IP de Sysdig](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-network#network).
    *   **{{site.data.keyword.loganalysislong_notm}}**:
        <pre class="screen">TCP port 443, port 9091 FROM &lt;each_worker_node_public_IP&gt; TO &lt;logging_public_IP&gt;</pre>
        Sustituya <em>&lt;logging_public_IP&gt;</em> por todas las direcciones de las regiones de registro a las que desea permitir el tráfico:
        <p><table summary="La primera fila de la tabla abarca ambas columnas. El resto de las filas deben leerse de izquierda a derecha, con la zona de servidor en la columna una y las direcciones IP coincidentes en la columna dos.">
<caption>Direcciones IP para abrir para el tráfico de creación de registros</caption>
        <thead>
        <th>Región de {{site.data.keyword.containerlong_notm}}</th>
        <th>Dirección de registro</th>
        <th>Direcciones IP de registro</th>
        </thead>
        <tbody>
          <tr>
            <td>EE. UU. este, EE. UU. sur</td>
            <td><code>ingest.logging.ng.bluemix.net</code></td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
          </tr>
          <tr>
           <td>RU sur</td>
           <td><code>ingest.logging.eu-gb.bluemix.net</code></td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>UE central</td>
           <td><code>ingest-eu-fra.logging.bluemix.net</code></td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>AP sur, AP norte</td>
           <td><code>ingest-au-syd.logging.bluemix.net</code></td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>
    *   **{{site.data.keyword.la_full_notm}}**:
        <pre class="screen">TCP port 443, port 80 FROM &lt;each_worker_node_public_IP&gt; TO &lt;logDNA_public_IP&gt;</pre>
        Sustituya `<logDNA_public_IP>` por las [direcciones IP de LogDNA](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-network#network).

5. Si utiliza los servicios del equilibrador de carga, asegúrese de que todo el tráfico que utiliza el protocolo VRRP esté permitido entre los nodos trabajadores en las interfaces públicas y privadas. {{site.data.keyword.containerlong_notm}} utiliza el protocolo VRRP para gestionar direcciones IP para equilibradores de carga públicos y privados.

6. {: #pvc}Para crear reclamaciones de volúmenes persistentes en un clúster privado, asegúrese de que el clúster se haya configurado con la versión de Kubernetes o las versiones del plugin de almacenamiento de {{site.data.keyword.Bluemix_notm}} siguientes. Estas versiones permiten la comunicación de red privada entre el clúster y las instancias de almacenamiento persistente.
    <table>
    <caption>Visión general de las versiones necesarias de Kubernetes o del plugin de almacenamiento de {{site.data.keyword.Bluemix_notm}} para clústeres privados</caption>
    <thead>
      <th>Tipo de almacenamiento</th>
      <th>Versión necesaria</th>
   </thead>
   <tbody>
     <tr>
       <td>Almacenamiento de archivos</td>
       <td>Kubernetes versión <code>1.13.4_1512</code>, <code>1.12.6_1544</code>, <code>1.11.8_1550</code>, <code>1.10.13_1551</code> o posterior</td>
     </tr>
     <tr>
       <td>Almacenamiento en bloque</td>
       <td>Plugin {{site.data.keyword.Bluemix_notm}} Block Storage versión 1.3.0 o posteriores</td>
     </tr>
     <tr>
       <td>Almacenamiento de objetos</td>
       <td><ul><li>Plugin {{site.data.keyword.cos_full_notm}} versión 1.0.3 o posteriores</li><li>Servicio {{site.data.keyword.cos_full_notm}} configurado con autenticación HMAC</li></ul></td>
     </tr>
   </tbody>
   </table>

   Si debe utilizar una versión de Kubernetes o una versión del plugin de almacenamiento de {{site.data.keyword.Bluemix_notm}} que no da soporte a la comunicación de red a través de la red privada o si desea utilizar {{site.data.keyword.cos_full_notm}} sin autenticación HMAC, permita el acceso de salida a través de su cortafuegos a la infraestructura de IBM Cloud (SoftLayer) y a {{site.data.keyword.Bluemix_notm}} Identity and Access Management:
   - Permita todo el tráfico de red de salida en el puerto TCP 443.
   - Permita el acceso al rango de IP de la infraestructura de IBM Cloud (SoftLayer) correspondiente a la zona en la que se encuentra el clúster tanto para la [**red frontal (pública)**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#frontend-public-network) como para la [**red de fondo (privada)**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network). Para buscar la zona del clúster, ejecute `ibmcloud ks clusters`.


<br />


## Cómo permitir que el clúster acceda a recursos a través de un cortafuegos privado
{: #firewall_private}

Si tiene un cortafuegos en la red privada, permita la comunicación entre los nodos trabajadores y deje que el clúster acceda a los recursos de la infraestructura a través de la red privada.
{:shortdesc}

1. Permita todo el tráfico entre nodos trabajadores.
    1. Permita todo el tráfico TCP, UDP, VRRP e IPEncap entre los nodos trabajadores en las interfaces públicas y privadas. {{site.data.keyword.containerlong_notm}} utiliza el protocolo VRRP para gestionar direcciones IP para equilibradores de carga privados y el protocolo IPEncap para permitir el tráfico de pod a pod entre subredes.
    2. Si utiliza políticas de Calico, o si tiene cortafuegos en cada zona de un clúster multizona, un cortafuegos puede bloquear la comunicación entre nodos trabajadores. Debe abrir todos los nodos trabajadores del clúster entre sí utilizando los puertos de los trabajadores, las direcciones IP privadas de los trabajadores o la etiqueta de nodo trabajador de Calico.

2. Permita los rangos de IP privados de la infraestructura de IBM Cloud (SoftLayer) para poder crear nodos trabajadores en el clúster.
    1. Permita los rangos de IP privados adecuados de la infraestructura de IBM Cloud (SoftLayer). Consulte [Red (privada) de fondo](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network).
    2. Permita los rangos de IP privados de la infraestructura de IBM Cloud (SoftLayer) para todas las [zonas](/docs/containers?topic=containers-regions-and-zones#zones) que está utilizando. Tenga en cuenta que debe añadir IP para las zonas `dal01`, `dal10`, `wdc04` y, si el clúster se encuentra en la zona geográfica de Europa, para la zona `ams01`. Consulte [Red de servicio (en red de fondo/privada)](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#service-network-on-backend-private-network-).

3. Abra los puertos siguientes:
    - Permita las conexiones TCP y UDP de salida de los nodos trabajadores a los puertos 80 y 443 para permitir actualizaciones y recargas de nodos trabajadores.
    - Permita TCP y UDP de salida al puerto 2049 para permitir el almacenamiento de archivos de montaje como volúmenes.
    - Permita TCP de salida y UDP al puerto 3260 para la comunicación con el almacenamiento en bloque.
    - Permita las conexiones TCP y UDP de entrada al puerto 10250 para el panel de control de Kubernetes y los mandatos como `kubectl logs` y `kubectl exec`.
    - Permita las conexiones de entrada y salida al puerto 53 TCP y UDP para el acceso de DNS.

4. Si también tiene un cortafuegos en la red pública, o si tiene un clúster sólo de VLAN privada y está utilizando un dispositivo de pasarela como cortafuegos, también debe permitir las IP y los puertos especificados en [Cómo permitir al clúster acceder a recursos de infraestructura y otros servicios](#firewall_outbound).

<br />


## Cómo permitir que el clúster acceda a recursos a través de políticas de salida de Calico
{: #firewall_calico_egress}

Si utiliza [Políticas de red de Calico](/docs/containers?topic=containers-network_policies) para actuar como un cortafuegos para restringir cualquier salida de trabajador pública, debe permitir a los trabajadores acceder a los proxies locales del servidor de API maestro y de etcd.
{: shortdesc}

1. [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) Incluya las opciones `--admin` y `--network`
con el mandato `ibmcloud ks cluster-config`. `--admin` descarga las claves para acceder a su portafolio de infraestructura y ejecutar mandatos de Calico en los nodos trabajadores. `--network` descarga el archivo de configuración de Calico para ejecutar todos los mandatos de Calico.
  ```
  ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin --network
  ```
  {: pre}

2. Cree una política de red Calico que permita el tráfico público de su clúster a 172.20.0.1:2040 y 172.21.0.1:443 para el proxy local del servidor de API, y 172.20.0.1:2041 para el proxy local etcd.
  ```
  apiVersion: projectcalico.org/v3
  kind: GlobalNetworkPolicy
  metadata:
    name: allow-master-local
  spec:
    egress:
    - action: Allow
      destination:
        ports:
        - 2040:2041
        nets:
        - 172.20.0.1/32
        protocol: UDP
    - action: Allow
      destination:
        ports:
        - 2040:2041
        nets:
        - 172.20.0.1/32
        protocol: TCP
    - action: Allow
      destination:
        ports:
        - 443
        nets:
        - 172.21.0.1/32
        protocol: UDP
    - action: Allow
      destination:
        ports:
        - 443
        nets:
        - 172.21.0.1/32
        protocol: TCP
    order: 1500
    selector: ibm.role == 'worker_public'
    types:
    - Egress
  ```
  {: codeblock}

3. Aplique la política al clúster.
    - Linux y OS X:

      ```
      calicoctl apply -f allow-master-local.yaml
      ```
      {: pre}

    - Windows:

      ```
      calicoctl apply -f filepath/allow-master-local.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

## Acceso a servicios NodePort, de equilibrador de carga e Ingress desde fuera del clúster
{: #firewall_inbound}

Puede permitir el acceso a servicios NodePort, de equilibrador de carga e Ingress.
{:shortdesc}

<dl>
  <dt>Servicio NodePort</dt>
  <dd>Abra el puerto que ha configurado cuando ha desplegado el servicio a las direcciones IP públicas para permitir el tráfico a todos los nodos trabajadores. Para encontrar el puerto, ejecute `kubectl get svc`. El puerto está en el rango 20000-32000.<dd>
  <dt>Servicio equilibrador de carga</dt>
  <dd>Abra el puerto que ha configurado cuando ha desplegado el servicio a la dirección IP pública del servicio de equilibrador de carga.</dd>
  <dt>Ingress</dt>
  <dd>Abra el puerto 80 para HTTP o el puerto 443 para HTTPS a la dirección IP del equilibrador de carga de aplicación de Ingress.</dd>
</dl>

<br />


## Colocación del clúster en la lista blanca de los cortafuegos de otros servicios o de los cortafuegos locales
{: #whitelist_workers}

Si desea acceder a servicios que se ejecutan dentro o fuera de {{site.data.keyword.Bluemix_notm}} o que son locales y que están protegidos por un cortafuegos, puede añadir las direcciones IP de los nodos trabajadores en dicho cortafuegos para permitir el tráfico de red de salida dirigido al clúster. Por ejemplo, es posible que desee leer datos de una base de datos de {{site.data.keyword.Bluemix_notm}} que esté protegida por un cortafuegos o colocar sus subredes de nodos trabajadores en la lista blanca de un cortafuegos local para permitir el tráfico de red procedente del clúster.
{:shortdesc}

1.  [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

2. Obtenga las subredes de nodos trabajadores o las direcciones IP de los nodos trabajadores.
  * **Subredes de nodos trabajadores**: Si tiene previsto cambiar el número de nodos trabajadores del clúster con frecuencia, por ejemplo si habilita el [programa de escalado automático de clústeres](/docs/containers?topic=containers-ca#ca), es posible que no desee actualizar el cortafuegos para cada nuevo nodo trabajador. Puede, en su lugar, colocar las subredes de VLAN que utiliza el clúster en la lista blanca. Tenga en cuenta que es posible que la subred de VLAN se comparta con nodos trabajadores de otros clústeres.
    <p class="note">Las **subredes públicas primarias** que {{site.data.keyword.containerlong_notm}} suministra para el clúster vienen con 14 direcciones IP disponibles y se pueden compartir con otros clústeres de la misma VLAN. Si tiene más de 14 nodos trabajadores, se solicita otra subred. de modo que las subredes que debe colocar en la lista blanca pueden cambiar. Para reducir la frecuencia de cambios, cree agrupaciones de nodos trabajadores con tipos de nodos trabajadores con más recursos de CPU y memoria para que no tenga que añadir nodos trabajadores con tanta frecuencia.</p>
    1. Obtenga una lista de los nodos trabajadores del clúster.
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

    2. En la salida del paso anterior, anote todos los ID de red exclusivos (primeros 3 octetos) de la **IP pública** para los nodos trabajadores del clúster. Si desea colocar en la lista blanca un clúster solo privado, anote en su lugar la **IP privada**. En la salida siguiente, los ID de red exclusivos son `169.xx.178` y `169.xx.210`.
        ```
        ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w31   169.xx.178.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.13.6   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w34   169.xx.178.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.13.6  
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w32   169.xx.210.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.13.6   
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w33   169.xx.210.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.13.6  
        ```
        {: screen}
    3.  Obtenga una lista de las subredes VLAN para cada ID de red exclusivo.
        ```
        ibmcloud sl subnet list | grep -e <networkID1> -e <networkID2>
        ```
        {: pre}

        Salida de ejemplo:
        ```
        ID        identifier       type                 network_space   datacenter   vlan_id   IPs   hardware   virtual_servers
        1234567   169.xx.210.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal12        1122334   16    0          5   
        7654321   169.xx.178.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal10        4332211   16    0          6    
        ```
        {: screen}
    4.  Recupere la dirección de subred. En la salida, busque el número de **IP**. Luego eleve `2` a la `n` potencia, igual al número de IP. Por ejemplo, si el número de IP es `16`, se eleva `2` a la `4` potencia (`n`), lo que es igual a `16`. A continuación, obtenga el CIDR de la subred restando el valor de `n` de `32` bits. Por ejemplo, si `n` es igual a `4`, el CIDR es `28` (resultado de la ecuación `32 - 4 = 28`). Combine la máscara **identifier** con el valor de CIDR para obtener la dirección completa de la subred. En la salida anterior, las direcciones de subred son las siguientes:
        *   `169.xx.210.xxx/28`
        *   `169.xx.178.xxx/28`
  * **Direcciones IP de nodos trabajadores individuales**: Si tiene un pequeño número de nodos trabajadores que solo ejecutan una app y no necesitan escalarse, o si solo desea colocar en la lista blanca un nodo trabajador, obtenga una lista de todos los nodos trabajadores del clúster y anote las direcciones **IP públicas**. Si los nodos trabajadores solo están conectados a una red privada y desea conectarse a servicios de {{site.data.keyword.Bluemix_notm}} utilizando el punto final de servicio privado, anote en su lugar las direcciones **IP privadas**. Tenga en cuenta que solo estos nodos trabajadores se colocan en la lista blanca. Si suprime nodos trabajadores o añade nodos trabajadores al clúster, debe actualizar el cortafuegos en consecuencia.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}
4.  Añada el CIDR de subred o las direcciones IP al cortafuegos del servicio para el tráfico de salida o al cortafuegos local para el tráfico de entrada.
5.  Repita estos pasos para cada clúster que desee colocar en la lista blanca.
