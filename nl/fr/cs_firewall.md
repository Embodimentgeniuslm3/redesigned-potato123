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



# Ouverture des ports et adresses IP requis dans votre pare-feu
{: #firewall}

Examinez ces situations pour lesquelles vous aurez peut-être à ouvrir des ports et des adresses IP spécifiques dans vos pare-feux pour {{site.data.keyword.containerlong}} :
{:shortdesc}

* [Pour exécuter des commandes `ibmcloud` et `ibmcloud ks`](#firewall_bx) depuis votre système local lorsque les règles réseau de l'entreprise empêchent l'accès aux noeud finaux de l'Internet public via des proxys ou des pare-feux.
* [Pour exécuter des commandes `kubectl`](#firewall_kubectl) depuis votre système local lorsque les règles réseau de l'entreprise empêchent l'accès à des noeuds finaux de l'Internet public via des proxys ou des pare-feux.
* [Pour exécuter des commandes `calicoctl`](#firewall_calicoctl) depuis votre système local lorsque les règles réseau de l'entreprise empêchent l'accès à des noeuds finaux de l'Internet public via des proxys ou des pare-feux.
* [Pour autoriser la communication entre le maître Kubernetes et les noeuds worker ](#firewall_outbound)lorsqu'un pare-feu a été mis en place pour les noeuds worker ou que les paramètres du pare-feu ont été personnalisés dans votre compte d'infrastructure IBM Cloud (SoftLayer).
* [Pour autoriser le cluster à accéder aux ressources via un pare-feu sur le réseau privé](#firewall_private).
* [Pour autoriser le cluster à accéder aux ressources lorsque les règles réseau Calico bloquent la sortie du noeud worker](#firewall_calico_egress).
* [Pour accéder au service NodePort, au service d'équilibreur de charge ou au service Ingress de l'extérieur du cluster](#firewall_inbound).
* [Pour autoriser le cluster à accéder aux services qui s'exécutent en interne ou en externe dans {{site.data.keyword.Bluemix_notm}} ou sur site, et qui sont protégés par un pare-feu](#whitelist_workers).

<br />


## Exécution de commandes `ibmcloud` et `ibmcloud ks` derrière un pare-feu
{: #firewall_bx}

Si les règles réseau de l'entreprise empêchent l'accès depuis votre système à des noeuds finaux publics via des proxys ou des pare-feux, pour exécuter des commandes `ibmcloud` et `ibmcloud ks`, vous devez autoriser l'accès TCP pour {{site.data.keyword.Bluemix_notm}} et {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

1. Autorisez l'accès à `cloud.ibm.com` sur le port 443 dans votre pare-feu.
2. Vérifiez votre connexion en vous connectant à {{site.data.keyword.Bluemix_notm}} via ce noeud final d'API.
  ```
  ibmcloud login -a https://cloud.ibm.com/
  ```
  {: pre}
3. Autorisez l'accès à `containers.cloud.ibm.com` sur le port 443 dans votre pare-feu.
4. Vérifiez votre connexion. Si l'accès est configuré correctement, des navires sont affichés dans la sortie.
   ```
   curl https://containers.cloud.ibm.com/v1/
   ```
   {: pre}

   Exemple de sortie :
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


## Exécution de commandes `kubectl` derrière un pare-feu
{: #firewall_kubectl}

Si les règles réseau de l'entreprise empêchent l'accès depuis votre système à des noeuds finaux publics via des proxys ou des pare-feux, pour exécuter des commandes `kubectl`, vous devez autoriser l'accès TCP pour le cluster.
{:shortdesc}

Lorsqu'un cluster est créé, le port indiqué dans les URL de noeud final de service est affecté aléatoirement sur une plage de 20000 à 32767. Vous pouvez choisir d'ouvrir la plage de ports 20000 à 32767 pour n'importe quel cluster pouvant être créé ou bien autoriser l'accès pour un cluster existant spécifique.

Avant de commencer, autorisez l'accès pour [exécuter des commandes `ibmcloud ks`](#firewall_bx).

Pour autoriser l'accès à un cluster spécifique :

1. Connectez-vous à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. A l'invite, entrez vos données d'identification {{site.data.keyword.Bluemix_notm}}. Si vous disposez d'un compte fédéré, incluez l'option `--sso`.

   ```
   ibmcloud login [--sso]
   ```
   {: pre}

2. Si le cluster se trouve dans un autre groupe de ressources que `default`, ciblez ce groupe de ressources. Pour voir à quel groupe de ressources appartient chaque cluster, exécutez la commande `ibmcloud ks clusters`. **Remarque** : vous devez au moins disposer du [rôle **Afficheur**](/docs/containers?topic=containers-users#platform) pour le groupe de ressources.
   ```
   ibmcloud target -g <resource_group_name>
   ```
   {: pre}

4. Obtenez le nom de votre cluster.

   ```
   ibmcloud ks clusters
   ```
   {: pre}

5. Extrayez les URL de noeud final de service pour votre cluster.
 * Si seule l'**URL du noeud final de service public** est indiquée, extrayez cette URL. Vos utilisateurs de cluster autorisés peuvent accéder au maître Kubernetes via ce noeud final sur le réseau public.
 * Si seule l'**URL du noeud final de service privé** est indiquée, extrayez cette URL. Vos utilisateurs de cluster autorisés peuvent accéder au maître Kubernetes via ce noeud final sur le réseau privé.
 * Si l'**URL du noeud final de service public** et l'**URL du noeud final de service privé** sont indiquées, extrayez ces deux URL. Vos utilisateurs de cluster autorisés peuvent accéder au maître Kubernetes via le noeud final public sur le réseau public et via le noeud final privé sur le réseau privé.

  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Exemple de sortie :
  ```
  ...
  Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
  Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
  ...
  ```
  {: screen}

6. Autorisez l'accès aux URL de noeud final de service et aux ports que vous avez obtenus à l'étape précédente. Si vous disposez d'un pare-feu basé sur les adresses IP, vous pouvez voir quelles sont les adresses IP ouvertes lorsque vous autorisez l'accès aux URL de noeud final de service, en consultant [ce tableau](#master_ips).

7. Vérifiez votre connexion.
  * Si le noeud final de service public est activé :
    ```
    curl --insecure <public_service_endpoint_URL>/version
    ```
    {: pre}

    Exemple de commande :
    ```
    curl --insecure https://c3.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    Exemple de sortie :
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
  * Si le noeud final de service privé est activé, vous devez être dans votre réseau privé {{site.data.keyword.Bluemix_notm}} ou vous connecter au réseau privé via une connexion VPN pour vérifier votre connexion au maître.
    Notez que vous devez [exposer le noeud final maître via un équilibreur de charge privé](/docs/containers?topic=containers-clusters#access_on_prem) de sorte que les utilisateurs puissent accéder au maître via un VPN ou une connexion {{site.data.keyword.BluDirectLink}}.
    ```
    curl --insecure <private_service_endpoint_URL>/version
    ```
    {: pre}

    Exemple de commande :
    ```
    curl --insecure https://c3-private.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    Exemple de sortie :
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

8. Facultatif : répétez ces étapes pour chaque cluster que vous avec besoin d'exposer.

<br />


## Exécution de commandes `calicoctl` derrière un pare-feu
{: #firewall_calicoctl}

Si les règles réseau de l'entreprise empêchent l'accès depuis votre système à des noeuds finaux publics via des proxys ou des pare-feux, pour exécuter des commandes `calicoctl`, vous devez autoriser l'accès TCP pour les commandes Calico.
{:shortdesc}

Avant de commencer, autorisez l'accès pour exécuter des commandes [`ibmcloud`](#firewall_bx) et [`kubectl`](#firewall_kubectl).

1. Extrayez l'adresse IP de l'URL maître que vous avez utilisée pour autoriser les commandes [`kubectl`](#firewall_kubectl).

2. Obtenez le port pour etcd.

  ```
  kubectl get cm -n kube-system cluster-info -o yaml | grep etcd_host
  ```
  {: pre}

3. Autorisez l'accès aux règles Calico via l'adresse IP de l'URL du maître et le port etcd.

<br />


## Autorisation accordée au cluster d'accéder aux ressources de l'infrastructure et à d'autres services via un pare-feu public
{: #firewall_outbound}

Autorisez votre cluster à accéder aux services et aux ressources d'infrastructure derrière un pare-feu public, par exemple pour les régions {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.registrylong_notm}}, {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM), {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, les adresses IP privées de l'infrastructure IBM Cloud (SoftLayer) et la sortie (egress) des réservations de volume persistant.
{:shortdesc}

Selon la configuration de votre cluster, vous accédez aux services en utilisant des adresses IP publiques, des adresses IP privées ou ces deux types d'adresse IP. Si vous disposez d'un cluster avec des noeuds worker figurant à la fois sur des VLAN public et privé derrière un pare-feu pour ces deux types de réseau, vous devez ouvrir la connexion pour les adresses IP publiques et privées. Si votre cluster comprend des noeuds worker uniquement sur le VLAN privé derrière un pare-feu, vous pouvez ouvrir la connexion uniquement aux adresses IP privées.
{: note}

1.  Notez l'adresse IP publique pour tous vos noeuds worker dans le cluster.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

2.  Autorisez le trafic entrant depuis la source <em>&lt;each_worker_node_publicIP&gt;</em> vers la plage de ports de destination TCP/UDP 20000 à 32767 et le port 443, et les adresses IP et groupes de réseau suivants. Ces adresses IP permettent aux noeuds worker de communiquer avec le maître cluster. Si un pare-feu d'entreprise empêche votre machine locale d'accéder à des noeuds finaux de l'Internet public, effectuez cette étape également pour votre machine locale afin de pouvoir accéder au maître cluster.

    Vous devez autoriser le trafic sortant vers le port 443 pour toutes les zones de la région afin d'équilibrer la charge lors du processus d'amorçage. Par exemple, si votre cluster se trouve au Sud des Etats-Unis, vous devez autoriser le trafic depuis les adresses IP publiques de chacun de vos noeuds worker sur le port 443 de l'adresse IP pour toutes les zones.
    {: important}

    {: #master_ips}
    <table summary="La première ligne du tableau est répartie sur deux colonnes. La lecture des autres lignes s'effectue de gauche à droite, avec la zone du serveur dans la première colonne et les adresses IP correspondantes dans la deuxième.">
      <caption>Adresses IP à ouvrir pour le trafic sortant</caption>
          <thead>
          <th>Région</th>
          <th>Zone</th>
          <th>Adresse IP publique</th>
          <th>Adresse IP privée</th>
          </thead>
        <tbody>
          <tr>
            <td>Asie-Pacifique nord</td>
            <td>che01<br>hkg02<br>seo01<br>sng01<br><br>tok02, tok04, tok05</td>
            <td><code>169.38.70.10</code><br><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><br><code>161.202.126.210, 128.168.71.117, 165.192.69.69</code></td>
            <td><code>166.9.40.7</code><br><code>166.9.42.7</code><br><code>166.9.44.5</code><br><code>166.9.40.8</code><br><br><code>166.9.40.6, 166.9.42.6, 166.9.44.4</code></td>
           </tr>
          <tr>
             <td>Asie-Pacifique sud</td>
             <td>mel01<br><br>syd01, syd04, syd05</td>
             <td><code>168.1.97.67</code><br><br><code>168.1.8.195, 130.198.66.26, 168.1.12.98, 130.198.64.19</code></td>
             <td><code>166.9.54.10</code><br><br><code>166.9.52.14, 166.9.52.15, 166.9.54.11, 166.9.54.13, 166.9.54.12</code></td>
          </tr>
          <tr>
             <td>Europe centrale</td>
             <td>ams03<br>mil01<br>osl01<br>par01<br><br>fra02, fra04, fra05</td>
             <td><code>169.50.169.110, 169.50.154.194</code><br><code>159.122.190.98, 159.122.141.69</code><br><code>169.51.73.50</code><br><code>159.8.86.149, 159.8.98.170</code><br><br><code>169.50.56.174, 161.156.65.42, 149.81.78.114</code></td>
             <td><code>166.9.28.17, 166.9.30.11</code><br><code>166.9.28.20, 166.9.30.12</code><br><code>166.9.32.8</code><br><code>166.9.28.19, 166.9.28.22</code><br><br><code>	166.9.28.23, 166.9.30.13, 166.9.32.9</code></td>
            </tr>
          <tr>
            <td>Sud du Royaume-Uni</td>
            <td>lon02, lon04, lon05, lon06</td>
            <td><code>159.122.242.78, 158.175.111.42, 158.176.94.26, 159.122.224.242, 158.175.65.170, 158.176.95.146</code></td>
            <td><code>166.9.34.5, 166.9.34.6, 166.9.36.10, 166.9.36.11, 166.9.36.12, 166.9.36.13, 166.9.38.6, 166.9.38.7</code></td>
          </tr>
          <tr>
            <td>Est des Etats-Unis</td>
             <td>mon01<br>tor01<br><br>wdc04, wdc06, wdc07</td>
             <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><br><code>169.63.88.186, 169.60.73.142, 169.61.109.34, 169.63.88.178, 169.60.101.42, 169.61.83.62</code></td>
             <td><code>166.9.20.11</code><br><code>166.9.22.8</code><br><br><code>166.9.20.12, 166.9.20.13, 166.9.22.9, 166.9.22.10, 166.9.24.4, 166.9.24.5</code></td>
          </tr>
          <tr>
            <td>Sud des Etats-Unis</td>
            <td>hou02<br>mex01<br>sao01<br>sjc03<br>sjc04<br><br>dal10,dal12,dal13</td>
            <td><code>184.173.44.62</code><br><code>169.57.100.18</code><br><code>169.57.151.10</code><br><code>169.45.67.210</code><br><code>169.62.82.197</code><br><br><code>169.46.7.238, 169.48.230.146, 169.61.29.194, 169.46.110.218, 169.47.70.10, 169.62.166.98, 169.48.143.218, 169.61.177.2, 169.60.128.2</code></td>
            <td><code>166.9.15.74</code><br><code>166.9.15.76</code><br><code>166.9.12.143</code><br><code>166.9.12.144</code><br><code>166.9.15.75</code><br><br><code>166.9.12.140, 166.9.12.141, 166.9.12.142, 166.9.15.69, 166.9.15.70, 166.9.15.72, 166.9.15.71, 166.9.15.73, 166.9.16.183, 166.9.16.184, 166.9.16.185</code></td>
          </tr>
          </tbody>
        </table>

3.  {: #firewall_registry}Pour permettre aux noeuds worker à communiquer avec {{site.data.keyword.registrylong_notm}}, autorisez le trafic réseau sortant provenant des noeuds worker vers les [régions {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-registry_overview#registry_regions) : 
  - `TCP port 443, port 4443 FROM <each_worker_node_publicIP> TO <registry_subnet>`
  -  Remplacez <em>&lt;registry_subnet&gt;</em> par le sous-réseau du registre vers lequel vous souhaitez autoriser le trafic. Le registre global héberge des images publiques fournies par IBM et les registres régionaux vos propres images privées ou publiques. Le port 4443 est requis pour les fonctions notariales, telles que la [vérification des signatures d'image](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent). <table summary="La première ligne du tableau est répartie sur deux colonnes. La lecture des autres lignes s'effectue de gauche à droite, avec la zone du serveur dans la première colonne et les adresses IP correspondantes dans la deuxième.">
  <caption>Adresses IP à ouvrir pour le trafic du registre</caption>
    <thead>
      <th>Région {{site.data.keyword.containerlong_notm}}</th>
      <th>Adresse du registre</th>
      <th>Sous-réseaux publics du registre</th>
      <th>Adresses IP privées du registre</th>
    </thead>
    <tbody>
      <tr>
        <td>Registre global entre les régions <br>d'{{site.data.keyword.containerlong_notm}}</td>
        <td><code>icr.io</code><br><br>
        Déprécié : <code>registry.bluemix.net</code></td>
        <td><code>169.60.72.144/28</code></br><code>169.61.76.176/28</code></br><code>169.62.37.240/29</code></br><code>169.60.98.80/29</code></br><code>169.63.104.232/29</code></td>
        <td><code>166.9.20.4</code></br><code>166.9.22.3</code></br><code>166.9.24.2</code></td>
      </tr>
      <tr>
        <td>Asie-Pacifique nord</td>
        <td><code>jp.icr.io</code><br><br>
        Déprécié : <code>registry.au-syd.bluemix.net</code></td>
        <td><code>161.202.146.86/29</code></br><code>128.168.71.70/29</code></br><code>165.192.71.222/29</code></td>
        <td><code>166.9.40.3</code></br><code>166.9.42.3</code></br><code>166.9.44.3</code></td>
      </tr>
      <tr>
        <td>Asie-Pacifique sud</td>
        <td><code>au.icr.io</code><br><br>
        Déprécié : <code>registry.au-syd.bluemix.net</code></td>
        <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></br><code>168.1.1.240/29</code></br><code>130.198.88.128/29</code></br><code>135.90.66.48/29</code></td>
        <td><code>166.9.52.2</code></br><code>166.9.54.2</code></br><code>166.9.56.3</code></td>
      </tr>
      <tr>
        <td>Europe centrale</td>
        <td><code>de.icr.io</code><br><br>
        Déprécié : <code>registry.eu-de.bluemix.net</code></td>
        <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></br><code>169.50.58.104/29</code></br><code>161.156.93.16/29</code></br><code>149.81.79.152/29</code></td>
        <td><code>166.9.28.12</code></br><code>166.9.30.9</code></br><code>166.9.32.5</code></td>
       </tr>
       <tr>
        <td>Sud du Royaume-Uni</td>
        <td><code>uk.icr.io</code><br><br>
        Déprécié : <code>registry.eu-gb.bluemix.net</code></td>
        <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></br><code>158.175.97.184/29</code></br><code>158.176.105.64/29</code></br><code>141.125.71.136/29</code></td>
        <td><code>166.9.36.9</code></br><code>166.9.38.5</code></br><code>166.9.34.4</code></td>
       </tr>
       <tr>
        <td>Est des Etats-Unis, Sud des Etats-Unis</td>
        <td><code>us.icr.io</code><br><br>
        Déprécié : <code>registry.ng.bluemix.net</code></td>
        <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></br><code>169.61.234.224/29</code></br><code>169.61.135.160/29</code></br><code>169.61.46.80/29</code></td>
        <td><code>166.9.12.114</code></br><code>166.9.15.50</code></br><code>166.9.16.173</code></td>
       </tr>
      </tbody>
    </table>

4. Facultatif : autorisez le trafic réseau sortant depuis les noeuds worker vers les services {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, Sysdig et LogDNA :
    *   **{{site.data.keyword.monitoringlong_notm}}**:
        <pre class="screen">TCP port 443, port 9095 FROM &lt;each_worker_node_public_IP&gt; TO &lt;monitoring_subnet&gt;</pre>
        Remplacez <em>&lt;monitoring_subnet&gt;</em> par les sous-réseaux des régions de surveillance auxquels vous désirez autoriser le trafic :
        <p><table summary="La première ligne du tableau est répartie sur deux colonnes. La lecture des autres lignes s'effectue de gauche à droite, avec la zone du serveur dans la première colonne et les adresses IP correspondantes dans la deuxième.">
  <caption>Adresses IP à ouvrir pour gérer le trafic</caption>
        <thead>
        <th>Région {{site.data.keyword.containerlong_notm}}</th>
        <th>Adresse de surveillance</th>
        <th>Sous-réseaux de surveillance</th>
        </thead>
      <tbody>
        <tr>
         <td>Europe centrale</td>
         <td><code>metrics.eu-de.bluemix.net</code></td>
         <td><code>158.177.65.80/30</code></td>
        </tr>
        <tr>
         <td>Sud du Royaume-Uni</td>
         <td><code>metrics.eu-gb.bluemix.net</code></td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>Est des Etats-Unis, Sud des Etats-Unis, Asie-Pacifique nord et Asie-Pacifique sud</td>
          <td><code>metrics.ng.bluemix.net</code></td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
    *   **{{site.data.keyword.mon_full_notm}}**:
        <pre class="screen">TCP port 443, port 6443 FROM &lt;each_worker_node_public_IP&gt; TO &lt;sysdig_public_IP&gt;</pre>
        Remplacez `<sysdig_public_IP>` par les [adresses IP de Sysdig](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-network#network).
    *   **{{site.data.keyword.loganalysislong_notm}}**:
        <pre class="screen">TCP port 443, port 9091 FROM &lt;each_worker_node_public_IP&gt; TO &lt;logging_public_IP&gt;</pre>
        Remplacez <em>&lt;logging_public_IP&gt;</em> par toutes les adresses des régions de consignation auxquelles vous voulez autoriser le trafic :
        <p><table summary="La première ligne du tableau est répartie sur deux colonnes. La lecture des autres lignes s'effectue de gauche à droite, avec la zone du serveur dans la première colonne et les adresses IP correspondantes dans la deuxième.">
<caption>Adresses IP à ouvrir pour consigner le trafic</caption>
        <thead>
        <th>Région {{site.data.keyword.containerlong_notm}}</th>
        <th>Adresse de consignation</th>
        <th>Adresses IP de consignation</th>
        </thead>
        <tbody>
          <tr>
            <td>Est des Etats-Unis, Sud des Etats-Unis</td>
            <td><code>ingest.logging.ng.bluemix.net</code></td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
          </tr>
          <tr>
           <td>Sud du Royaume-Uni</td>
           <td><code>ingest.logging.eu-gb.bluemix.net</code></td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>Europe centrale</td>
           <td><code>ingest-eu-fra.logging.bluemix.net</code></td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>Asie-Pacifique sud, Asie-Pacifique nord</td>
           <td><code>ingest-au-syd.logging.bluemix.net</code></td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>
    *   **{{site.data.keyword.la_full_notm}}**:
        <pre class="screen">TCP port 443, port 80 FROM &lt;each_worker_node_public_IP&gt; TO &lt;logDNA_public_IP&gt;</pre>
        Remplacez `<logDNA_public_IP>` par les [adresses IP de LogDNA](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-network#network).

5. Si vous utilisez des services d'équilibreur de charge, vérifiez que tout le trafic utilisant le protocole VRRP est autorisé entre les noeuds worker sur les interfaces publiques et privées. {{site.data.keyword.containerlong_notm}} utilise le protocole VRRP pour gérer les adresses IP des équilibreurs de charge publics et privés.

6. {: #pvc}Pour créer des réservations de volume persistant dans un cluster privé, veillez à configurer votre cluster avec la version Kubernetes ou les versions de plug-in de stockage d'{{site.data.keyword.Bluemix_notm}} suivantes. Ces versions activent la communication sur le réseau privé de votre cluster à vos instances de stockage persistant.
    <table>
    <caption>Présentation des versions de Kubernetes ou de plug-in de stockage {{site.data.keyword.Bluemix_notm}} requises pour les clusters privés</caption>
    <thead>
      <th>Type de stockage</th>
      <th>Version requise</th>
   </thead>
   <tbody>
     <tr>
       <td>Stockage de fichiers</td>
       <td>Version Kubernetes <code>1.13.4_1512</code>, <code>1.12.6_1544</code>, <code>1.11.8_1550</code>, <code>1.10.13_1551</code> ou ultérieure</td>
     </tr>
     <tr>
       <td>Stockage par blocs</td>
       <td>Plug-in {{site.data.keyword.Bluemix_notm}} Block Storage version 1.3.0 ou ultérieure</td>
     </tr>
     <tr>
       <td>Stockage d'objets</td>
       <td><ul><li>Plug-in {{site.data.keyword.cos_full_notm}} version 1.0.3 ou ultérieure</li><li>Service {{site.data.keyword.cos_full_notm}} configuré avec l'authentification HMAC</li></ul></td>
     </tr>
   </tbody>
   </table>

   Si vous devez utiliser une version de Kubernetes ou de plug-in de stockage {{site.data.keyword.Bluemix_notm}} qui ne prend pas en charge la communication réseau sur le réseau privé, ou si vous souhaitez utiliser {{site.data.keyword.cos_full_notm}} sans l'authentification HMAC, autorisez l'accès sortant via votre pare-feu à l'infrastructure IBM Cloud (SoftLayer) et {{site.data.keyword.Bluemix_notm}} Identity and Access Management :
   - Autorisez tout le trafic réseau sortant sur le port TCP 443.
   - Autorisez l'accès à la plage d'adresses IP de l'infrastructure IBM Cloud (SoftLayer) pour la zone où se trouve votre cluster pour le [**réseau (public) de front end**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#frontend-public-network) et le [**réseau (privé) de back end**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network). Pour savoir dans quelle zone se trouve votre cluster, exécutez la commande `ibmcloud ks clusters`.


<br />


## Autorisation accordée au cluster d'accéder aux ressources via un pare-feu privé
{: #firewall_private}

Si vous disposez d'un pare-feu sur le réseau privé, autorisez la communication entre les noeuds worker et laissez votre cluster accéder aux ressources de l'infrastructure via le réseau privé.
{:shortdesc}

1. Autorisez tout le trafic entre les noeuds worker.
    1. Autorisez tout le trafic TCP, UDP, VRRP et IPEncap entre les noeuds worker sur les interfaces publiques et privées. {{site.data.keyword.containerlong_notm}} utilise le protocole VRRP pour gérer les adresses IP des équilibreurs de charge privés et le protocole IPEncap pour autoriser le trafic de pod à pod dans les sous-réseaux.
    2. Si vous utilisez des règles Calico ou si vous disposez de pare-feux dans chaque zone d'un cluster à zones multiples, un pare-feu peut bloquer la communication entre les noeuds worker. Vous devez ouvrir tous les noeuds worker dans le cluster pour qu'ils communiquent entre eux en utilisant des ports de noeuds worker, des adresses IP privées de noeuds worker ou le libellé de noeud worker Calico.

2. Autorisez les plages d'adresses IP privées de l'infrastructure IBM Cloud (SoftLayer) pour pouvoir créer des noeuds worker dans votre cluster.
    1. Autorisez les plages d'adresses IP privées de l'infrastructure IBM Cloud (SoftLayer) appropriées. Voir [Réseau (privé) de back end](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network).
    2. Autorisez les plages d'adresses IP privées de l'infrastructure IBM Cloud (SoftLayer) pour toutes les [zones](/docs/containers?topic=containers-regions-and-zones#zones) que vous utilisez. Notez que vous devez ajouter des adresses IP pour les zones `dal01`, `dal10`, `wdc04`, et si votre cluster se trouve en Europe, pour la zone `ams01`. Voir [Réseau de service (sur réseau back end/privé)](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#service-network-on-backend-private-network-).

3. Ouvrez les ports suivants :
    - Autorisez les connexions TCP et UDP sortantes à partir des noeuds worker sur les ports 80 et 443 pour permettre les mises à jour et les rechargements des noeuds worker.
    - Autorisez les connexions TCP et UDP sortantes sur le port 2049 pour permettre le montage du stockage de fichiers sous forme de volumes.
    - Autorisez les connexions TCP et UDP sortantes sur le port 3260 pour la communication avec du stockage par blocs.
    - Autorisez les connexions TCP et UDP entrantes sur le port 10250 pour le tableau de bord Kubernetes et les commandes telles que `kubectl logs` et `kubectl exec`.
    - Autorisez les connexions TCP et UDP entrantes et sortantes sur le port 53 pour l'accès DNS.

4. Si vous disposez également d'un pare-feu sur le réseau public, ou d'un cluster avec uniquement un VLAN privé et que vous utilisez un dispositif de passerelle comme pare-feu, vous devez également autoriser les adresses IP et les ports spécifiés dans [Autorisation accordée au cluster d'accéder aux ressources de l'infrastructure et à d'autres services](#firewall_outbound).

<br />


## Autorisation accordée au cluster d'accéder aux ressources via des règles sortantes Calico
{: #firewall_calico_egress}

Si vous utilisez des [règles réseau Calico](/docs/containers?topic=containers-network_policies) qui agissent en tant que pare-feu pour limiter toutes les sorties de noeud worker publiques, vous devez autoriser vos noeuds worker à accéder aux proxy locaux pour le serveur d'API maître et etcd.
{: shortdesc}

1. [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) Incluez les options `--admin` et `--network` avec la commande `ibmcloud ks cluster-config`. L'option `--admin` permet de télécharger les clés pour accéder à votre portefeuille d'infrastructure et exécuter des commandes Calico sur vos noeuds worker. L'option `--network` permet de télécharger le fichier de configuration pour exécuter toutes les commandes Calico.
  ```
  ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin --network
  ```
  {: pre}

2. Créez une politique de réseau Calico qui autorise le trafic public à partir de votre cluster vers 172.20.0.1:2040 et 172.21.0.1:443 pour le proxy local de serveur d'API, et 172.20.0.1:2041 pour le proxy local etcd.
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

3. Appliquez la politique au cluster.
    - Linux et OS X :

      ```
      calicoctl apply -f allow-master-local.yaml
      ```
      {: pre}

    - Windows :

      ```
      calicoctl apply -f filepath/allow-master-local.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

## Accès au service NodePort, au service d'équilibreur de charge et au service Ingress de l'extérieur du cluster
{: #firewall_inbound}

Vous pouvez autoriser l'accès entrant au service NodePort, au service d'équilibreur de charge et au service Ingress.
{:shortdesc}

<dl>
  <dt>Service NodePort</dt>
  <dd>Ouvrez le port que vous avez configuré lorsque vous avez déployé le service sur les adresses IP publiques pour tous les noeuds worker vers lesquels autoriser le trafic. Pour identifier le port, exécutez la commande `kubectl get svc`. Le port est compris dans une plage de 20000 à 32000.<dd>
  <dt>Service d'équilibreur de charge</dt>
  <dd>Ouvrez le port que vous avez configuré lorsque vous avez déployé le service sur l'adresse IP publique du service d'équilibreur de charge.</dd>
  <dt>Ingress</dt>
  <dd>Ouvrez le port 80 pour HTTP ou le port 443 pour HTTPS vers l'adresse IP de l'équilibreur de charge d'application Ingress.</dd>
</dl>

<br />


## Placement de votre cluster en liste blanche dans les pare-feux d'autres services ou dans les pare-feux locaux
{: #whitelist_workers}

Si vous voulez accéder aux services qui s'exécutent en interne ou en externe dans {{site.data.keyword.Bluemix_notm}} ou sur site, et qui sont protégés par un pare-feu, vous pouvez ajouter les adresses IP de vos noeuds worker dans ce pare-feu pour autoriser le trafic réseau sortant vers votre cluster. Par exemple, vous envisagerez peut-être de lire des données dans une base de données {{site.data.keyword.Bluemix_notm}} qui est protégée par un pare-feu, ou de mettre les sous-réseaux de vos noeuds worker sur liste blanche dans un pare-feu local pour autoriser le trafic réseau provenant de votre cluster.
{:shortdesc}

1.  [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

2. Obtenez les sous-réseaux ou les adresses IP des noeuds worker.
  * **Sous-réseaux de noeuds worker** : si vous prévoyez de changer fréquemment le nombre de noeuds worker dans votre cluster, par exemple si vous activez le [programme de mise à l'échelle automatique de cluster (cluster autoscaler)](/docs/containers?topic=containers-ca#ca), vous souhaiterez sans doute éviter d'avoir à mettre à jour votre pare-feu pour chaque nouveau noeud worker. Vous pouvez à la place mettre sur liste blanche les sous-réseaux de VLAN utilisés par le cluster. N'oubliez pas qu'un sous-réseau de VLAN peut être partagé par des noeuds worker dans d'autres clusters.
    <p class="note">Les **sous-réseaux publics principaux** mis à disposition par {{site.data.keyword.containerlong_notm}} pour votre cluster comprennent 14 adresses IP disponibles et peuvent être partagés par d'autres clusters sur le même VLAN. Lorsque vous avez plus de 14 noeuds worker, un autre sous-réseau est commandé. Par conséquent, les sous-réseaux que vous devez mettre en liste blanche peuvent changer. Pour réduire la fréquence des modifications, créez des pools de noeuds worker avec des modèles comprenant des ressources supérieures en termes d'UC et de mémoire, pour que vous n'ayez pas à ajouter des noeuds worker aussi souvent.</p>
    1. Affichez la liste des noeuds worker présents dans votre cluster.
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

    2. Dans la sortie de l'étape précédente, notez tous les ID de réseau uniques (les 3 premiers octets) de l'**adresse IP publique** des noeuds worker dans votre cluster.
Si vous souhaitez mettre sur liste blanche un cluster privé uniquement, notez l'**adresse IP privée** à la place. Dans la sortie suivante, les ID de réseau uniques sont `169.xx.178` et `169.xx.210` :
        ```
        ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w31   169.xx.178.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.13.6   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w34   169.xx.178.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.13.6  
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w32   169.xx.210.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.13.6   
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w33   169.xx.210.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.13.6  
        ```
        {: screen}
    3.  Répertoriez les sous-réseaux de VLAN correspondant à chaque ID de réseau unique.
        ```
        ibmcloud sl subnet list | grep -e <networkID1> -e <networkID2>
        ```
        {: pre}

        Exemple de sortie :
        ```
        ID        identifier       type                 network_space   datacenter   vlan_id   IPs   hardware   virtual_servers
        1234567   169.xx.210.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal12        1122334   16    0          5   
        7654321   169.xx.178.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal10        4332211   16    0          6    
        ```
        {: screen}
    4.  Récupérez l'adresse du sous-réseau. Dans la sortie, recherchez le nombre d'adresses **IP**. Puis, augmentez la puissance `2` à un nombre `n` égal au nombre d'adresses IP. Par exemple, si le nombre d'adresses IP est `16`, `2` est augmenté à la puissance `4` (`n`) pour obtenir `16`. Obtenez ensuite le CIDR du sous-réseau en soustrayant la valeur `n` de `32` bits. Par exemple, lorsque le nombre `n` est égal à `4`, le CIDR correspond à `28` (d'après l'équation `32 - 4 = 28`). Combinez le masque **identifier** avec la valeur de CIDR pour obtenir l'adresse complète du sous-réseau. Dans la sortie précédente, les adresses de sous-réseau sont :
        *   `169.xx.210.xxx/28`
        *   `169.xx.178.xxx/28`
  * **Adresses IP de noeuds worker individuels** : si vous avez un nombre peu élevé de noeuds worker qui n'exécutent qu'une application et n'ont pas besoin de mise à l'échelle, ou si vous voulez mettre sur liste blanche un seul noeud worker, répertoriez tous les noeuds worker figurant dans votre cluster et notez les adresses **IP publiques**. Si vos noeuds worker sont connectés uniquement à un réseau privé et que vous voulez vous connecter aux services {{site.data.keyword.Bluemix_notm}} en utilisant le noeud final de service privé, notez les adresses **IP privées** à la place. Vous constatez que seuls ces noeuds worker sont placés en liste blanche. Si vous supprimez ou ajoutez des noeuds worker dans votre cluster, vous devez mettre à jour votre pare-feu en conséquence.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}
4.  Ajoutez la valeur CIDR ou les adresses IP au pare-feu de votre service pour le trafic sortant ou à votre pare-feu local pour le trafic entrant.
5.  Répétez ces étapes pour chaque cluster à mettre sur liste blanche.
