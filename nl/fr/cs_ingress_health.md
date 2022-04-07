---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# Consignation et surveillance d'Ingress
{: #ingress_health}

Personnalisez la consignation dans les journaux et configurez la surveillance pour vous aider à identifier et résoudre les erreurs et à améliorer les performances de la configuration de votre service Ingress.
{: shortdesc}

## Affichage des journaux Ingress
{: #ingress_logs}

Si vous souhaitez traiter les incidents liés à la ressource Ingress ou gérer l'activité d'Ingress, vous pouvez examiner les journaux Ingress.
{: shortdesc}

Les journaux sont automatiquement collectés pour vos équilibreurs de charge d'application (ALB) Ingress. Pour afficher les journaux de l'ALB, vous avez le choix entre deux options :
* [Créer une configuration de consignation pour le service Ingress](/docs/containers?topic=containers-health#configuring) dans votre cluster.
* Consulter les journaux à partir de l'interface de ligne de commande (CLI). **Remarque** : vous devez disposer au moins du [rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Lecteur**](/docs/containers?topic=containers-users#platform) pour l'espace de nom `kube-system`.
    1. Obtenez l'ID d'un pod pour un équilibreur de charge ALB.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. Ouvrez les journaux correspondant à ce pod d'ALB.
        ```
        kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

</br>Par défaut, le contenu des journaux est au format JSON et affiche des zones courantes qui décrivent la session de connexion entre un client et votre application. Voici un exemple de journal avec les zones par défaut :

```
{"time_date": "2018-08-21T17:33:19+00:00", "client": "108.162.248.42", "host": "albhealth.multizone.us-south.containers.appdomain.cloud", "scheme": "http", "request_method": "GET", "request_uri": "/", "request_id": "c2bcce90cf2a3853694da9f52f5b72e6", "status": 200, "upstream_addr": "192.168.1.1:80", "upstream_status": 200, "request_time": 0.005, "upstream_response_time": 0.005, "upstream_connect_time": 0.000, "upstream_header_time": 0.005}
```
{: screen}

<table>
<caption>Description des zones au format de journal Ingress par défaut</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des zones au format de journal Ingress par défaut</th>
</thead>
<tbody>
<tr>
<td><code>"time_date": "$time_iso8601"</code></td>
<td>Heure locale au format ISO 8601 standard au moment de l'écriture dans le journal.</td>
</tr>
<tr>
<td><code>"client": "$remote_addr"</code></td>
<td>Adresse IP du package de demande que le client a envoyé à votre application. Cette adresse IP change en fonction des situations suivantes :<ul><li>Lorsqu'une demande client vers votre application est envoyée à votre cluster, elle est acheminée à un pod pour le service d'équilibreur de charge qui expose l'équilibreur de charge d'application (ALB). S'il n'existe aucun pod d'application sur le même noeud worker que le pod de service d'équilibreur de charge, l'équilibreur de charge transmet la demande à un pod d'application sur un autre noeud worker. L'adresse IP source du package de demande est remplacée par l'adresse IP publique du noeud worker sur lequel s'exécute le pod d'application.</li><li>Si la [conservation de l'adresse IP source est activée](/docs/containers?topic=containers-ingress#preserve_source_ip), l'adresse IP d'origine de la demande du client à votre application est enregistrée à la place.</li></ul></td>
</tr>
<tr>
<td><code>"host": "$http_host"</code></td>
<td>Hôte ou sous-domaine, via lequel vos applications sont accessibles. Ce sous-domaine est configuré dans les fichiers de ressource Ingress de vos équilibreurs de charge d'application (ALB).</td>
</tr>
<tr>
<td><code>"scheme": "$scheme"</code></td>
<td>Type de demande : <code>HTTP</code> ou <code>HTTPS</code>.</td>
</tr>
<tr>
<td><code>"request_method": "$request_method"</code></td>
<td>Méthode d'appel de la demande à l'application de back end, par exemple <code>GET</code> ou <code>POST</code>.</td>
</tr>
<tr>
<td><code>"request_uri": "$uri"</code></td>
<td>URI de la demande d'origine vers votre chemin d'application. Les ALB traitent les chemins sur lesquels les applications sont à l'écoute en tant que préfixes. Lorsqu'un ALB reçoit une demande d'un client vers une application, il recherche un chemin (en tant que préfixe) dans la ressource Ingress, qui correspond au chemin dans l'URI.</td>
</tr>
<tr>
<td><code>"request_id": "$request_id"</code></td>
<td>Identificateur unique de la demande généré à partir de 16 octets aléatoires.</td>
</tr>
<tr>
<td><code>"status": $status</code></td>
<td>Code indiquant le statut de la session de connexion.<ul>
<li><code>200</code> : la session s'est terminée correctement</li>
<li><code>400</code> : les données du client n'ont pas pu être analysées</li>
<li><code>403</code> : accès interdit, par exemple lorsque l'accès est limité à certaines adresses IP client</li>
<li><code>500</code> : erreur de serveur interne</li>
<li><code>502</code> : passerelle incorrecte, par exemple si un serveur en amont est inaccessible ou qu'il est impossible de le sélectionner</li>
<li><code>503</code> : service non disponible, par exemple lorsque l'accès est limité à un certain nombre de connexions</li>
</ul></td>
</tr>
<tr>
<td><code>"upstream_addr": "$upstream_addr"</code></td>
<td>Adresse IP et port ou chemin vers le socket du domaine UNIX du serveur en amont. Si plusieurs serveurs sont contactés lors du traitement de la demande, leurs adresses sont séparées par des virgules : <code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock"</code>. Si la demande est redirigée en interne d'un groupe de serveurs à un autre, les adresses de serveur des différents groupes sont séparées par des points-virgules : <code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock : 192.168.10.1:80, 192.168.10.2:80"</code>. Si l'équilibreur de charge ALB ne parvient pas à sélectionner un serveur, le nom du groupe de serveurs est consigné à la place.</td>
</tr>
<tr>
<td><code>"upstream_status": $upstream_status</code></td>
<td>Code indiquant le statut de la réponse obtenue du serveur en amont pour l'application de back end, par exemple codes de réponse HTTP standard. Les codes de statut de plusieurs réponses sont séparés par des virgules ou des signes deux-points comme les adresses dans la variable <code>$upstream_addr</code>. Si l'ALB ne parvient pas à sélectionner un serveur, le code de statut 502 (Passerelle incorrecte) est consigné.</td>
</tr>
<tr>
<td><code>"request_time": $request_time</code></td>
<td>Temps de traitement de la demande, mesuré en secondes avec une résolution en millisecondes. Il démarre lorsque l'ALB lit les premiers octets de la demande du client et prend fin lorsque l'ALB envoie les derniers octets de la réponse au client. L'écriture dans le journal s'effectue immédiatement dès que le temps de traitement de la demande s'arrête.</td>
</tr>
<tr>
<td><code>"upstream_response_time": $upstream_response_time</code></td>
<td>Temps nécessaire à l'ALB pour recevoir la réponse du serveur en amont pour l'application de back end, mesuré en secondes avec une résolution en millisecondes. Les temps de plusieurs réponses sont séparés par des virgules ou des signes deux-points comme les adresses dans la variable <code>$upstream_addr</code>.</td>
</tr>
<tr>
<td><code>"upstream_connect_time": $upstream_connect_time</code></td>
<td>Temps nécessaire à l'ALB pour établir une connexion avec le serveur en amont pour l'application de back end, mesuré en secondes avec une résolution en millisecondes. Si TLS/SSL est activé dans la configuration de la ressource Ingress, ce temps comprend la durée d'établissement de la liaison. Les temps de plusieurs connexions sont séparés par des virgules ou des signes deux-points comme les adresses dans la variable <code>$upstream_addr</code>.</td>
</tr>
<tr>
<td><code>"upstream_header_time": $upstream_header_time</code></td>
<td>Temps nécessaire à l'ALB pour recevoir l'en-tête de réponse du serveur en amont pour l'application de back end, mesuré en secondes avec une résolution en millisecondes. Les temps de plusieurs connexions sont séparés par des virgules ou des signes deux-points comme les adresses dans la variable <code>$upstream_addr</code>.</td>
</tr>
</tbody></table>

## Personnalisation du contenu et du format des journaux Ingress
{: #ingress_log_format}

Vous pouvez personnaliser le contenu et le format des journaux collectés par l'équilibreur de charge d'application (ALB) Ingress.
{:shortdesc}

Par défaut, les journaux Ingress sont au format JSON et affichent des zones de journal courantes. Vous pouvez toutefois créer un format personnalisé pour les journaux en choisissant les composants de journaux qui sont acheminés et comment sont organisés ces composants dans la sortie du journal.

Avant de commencer, vérifiez que vous disposez du [rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Auteur** ou **Responsable**](/docs/containers?topic=containers-users#platform) pour l'espace de nom `kube-system`.

1. Editez le fichier de configuration correspondant à la ressource configmap `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Ajoutez une section <code>data</code>. Ajoutez la zone `log-format` et, éventuellement, la zone `log-format-escape-json`.

    ```
    apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    <table>
    <caption>Composants du fichier YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description de la configuration de la zone log-format</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>Remplacez <code>&lt;key&gt;</code> par le nom du composant de journal et <code>&lt;log_variable&gt;</code> par une variable pour le composant de journal que vous voulez collecter dans les entrées de journal. Vous pouvez inclure du texte et la ponctuation que vous voulez dans l'entrée du journal, par exemple entourer les valeurs de chaîne par des guillemets ou utiliser des virgules pour séparer les noms des composants de journal. Par exemple, le formatage d'un composant <code>request: "$request"</code> génère le résultat suivant dans une entrée de journal : <code>request: "GET / HTTP/1.1"</code>. Pour obtenir une liste de toutes les variables que vous pouvez utiliser, voir l'<a href="http://nginx.org/en/docs/varindex.html">index des variables NGINX</a>.<br><br>Pour consigner un en-tête supplémentaire, tel que <em>x-custom-ID</em>, ajoutez la paire clé-valeur suivante au contenu de journal personnalisé : <br><pre class="codeblock"><code>customID: $http_x_custom_id</code></pre> <br>Les traits d'union (<code>-</code>) sont convertis en traits de soulignement (<code>_</code>) et <code>$http_</code> doit être ajouté en préfixe au nom de l'en-tête personnalisé.</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>Facultatif : par défaut, les journaux sont générés au format texte. Pour générer des journaux au format JSON, ajoutez la zone <code>log-format-escape-json</code> et utilisez la valeur <code>true</code>.</td>
    </tr>
    </tbody></table>

    Par exemple, votre format de journal peut contenir les variables suivantes :
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    Selon ce format, une entrée de journal ressemblera à l'exemple suivant :
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    Pour créer un format de journal personnalisé basé sur le format par défaut des journaux d'ALB, modifiez la section suivante selon les besoins et ajoutez-la dans votre configmap :
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. Sauvegardez le fichier de configuration.

5. Vérifiez que les modifications apportées à configmap ont été appliquées.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

4. Pour afficher les journaux de l'ALB Ingress, vous avez le choix entre deux options :
    * [Créer une configuration de consignation pour le service Ingress](/docs/containers?topic=containers-health#logging) dans votre cluster.
    * Consulter les journaux à partir de l'interface de ligne de commande (CLI).
        1. Obtenez l'ID d'un pod pour un équilibreur de charge ALB.
            ```
            kubectl get pods -n kube-system | grep alb
            ```
            {: pre}

        2. Ouvrez les journaux correspondant à ce pod d'ALB. Vérifiez que les journaux sont conformes au nouveau format.
            ```
            kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
            ```
            {: pre}

<br />


## Surveillance de l'ALB Ingress
{: #ingress_monitoring}

Surveillez vos ALB en déployant dans votre cluster un exportateur de métriques et un agent Prometheus.
{: shortdesc}

L'exportateur de métriques d'ALB utilise la directive NGINX, `vhost_traffic_status_zone`, pour collecter des données de métriques à partir du noeud final `/status/format/json` sur chaque pod d'ALB Ingress. L'exportateur de métriques reformate automatiquement chaque zone de données dans le fichier JSON en métrique que peut lire Prometheus. Ensuite, un agent Prometheus récupère les métriques produites par l'exportateur et les rend visibles sur un tableau de bord Prometheus.

### Installation de la charge Helm de l'exportateur de métriques
{: #metrics-exporter}

Installez la charte Helm de l'exportateur de métriques pour surveiller un ALB dans votre cluster.
{: shortdesc}

Les pods d'exportateur de métriques d'ALB doivent se déployer sur les mêmes noeuds worker où sont déployés vos ALB. Si vos ALB s'exécutent sur des noeuds worker de périphérie, et que ces noeuds worker ont l'annotation taint pour empêcher d'autres déploiements de charge de travail, les pods d'exportateur de métriques ne peuvent pas être planifiés. Vous devez supprimer les annotations taint en exécutant la commande `kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-`.
{: note}

1.  **Important** : [Suivez les instructions](/docs/containers?topic=containers-helm#public_helm_install) d'installation du client Helm sur votre machine locale, installez le serveur Helm (Tiller) avec un compte de service et ajoutez les référentiels Helm {{site.data.keyword.Bluemix_notm}}.

2. Installez la charte Helm `ibmcloud-alb-metrics-exporter` dans votre cluster. Cette charte Helm déploie un exportateur de métriques d'ALB et crée un compte de service `alb-metrics-service-account` dans l'espace de nom `kube-system`. Remplacez <alb-ID> par l'ID de l'ALB pour lequel vous souhaitez collecter les métriques. Pour afficher les ID des ALB dans votre cluster, exécutez la commande <code>ibmcloud ks albs --cluster &lt;cluster_name&gt;</code>.
  Vous devez déployer une charte pour chaque ALB que vous désirez surveiller.
  {: note}
  ```
  helm install iks-charts/ibmcloud-alb-metrics-exporter --name ibmcloud-alb-metrics-exporter --set metricsNameSpace=kube-system --set albId=<alb-ID>
  ```
  {: pre}

3. Vérifiez le statut de déploiement de la charte. Lorsque la charte est prête, la zone **STATUS** vers le haut de la sortie a la valeur `DEPLOYED`.
  ```
  helm status ibmcloud-alb-metrics-exporter
  ```
  {: pre}

4. Vérifiez que les pods `ibmcloud-alb-metrics-exporter` sont en cours d'exécution.
  ```
  kubectl get pods -n kube-system -o wide
  ```
  {:pre}

  Exemple de sortie :
  ```
  NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
  ...
  alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  ```
  {:screen}

5. Facultatif : [Installez l'agent Prometheus](#prometheus-agent) pour récupérer les métriques produites par l'exportateur et les rendre visibles sur un tableau de bord Prometheus.

### Installation de la charte Helm de l'agent Prometheus
{: #prometheus-agent}

Après avoir installé l'[exportateur de métriques](#metrics-exporter), vous pouvez installer la charte de l'agent Prometheus pour récupérer les métriques produites par l'exportation et les rendre visibles sur un tableau de bord Prometheus.
{: shortdesc}

1. Téléchargez le fichier TAR pour la charte Helm de l'exportateur de métriques à partir du site https://icr.io/helm/iks-charts/charts/ibmcloud-alb-metrics-exporter-1.0.7.tgz

2. Accédez au sous-dossier Prometheus.
  ```
  cd ibmcloud-alb-metrics-exporter-1.0.7.tar/ibmcloud-alb-metrics-exporter/subcharts/prometheus
  ```
  {: pre}

3. Installez la charte Helm Prometheus sur votre cluster. Remplacez <ingress_subdomain> par le sous-domaine Ingress de votre cluster. L'URL d'accès au tableau de bord Prometheus comprend à la fois le sous-domaine Prometheus par défaut, `prom-dash`, et votre sous-domaine Ingress, par exemple, `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`. Pour obtenir le sous-domaine Ingress correspondant à votre cluster, exécutez la commande <code>ibmcloud ks cluster-get --cluster &lt;cluster_name&gt;</code>.
  ```
  helm install --name prometheus . --set nameSpace=kube-system --set hostName=prom-dash.<ingress_subdomain>
  ```
  {: pre}

4. Vérifiez le statut de déploiement de la charte. Lorsque la charte est prête, la zone **STATUS** près du début de la sortie a la valeur `DEPLOYED`.
    ```
    helm status prometheus
    ```
    {: pre}

5. Vérifiez que le pod `prometheus` est en cours d'exécution.
    ```
    kubectl get pods -n kube-system -o wide
    ```
    {:pre}

    Exemple de sortie :
    ```
    NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
    alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    prometheus-9fbcc8bc7-2wvbk                       1/1       Running     0          1m        172.30.xxx.xxx   10.xxx.xx.xxx
    ```
    {:screen}

6. Dans un navigateur, entrez l'URL d'accès au tableau de bord Prometheus. Ce nom d'hôte est au format `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`. Le tableau de bord Prometheus de votre ALB s'ouvre.

7. Consultez les informations sur les métriques d'[ALB](#alb_metrics), les métriques de [serveur](#server_metrics), et les métriques [en amont](#upstream_metrics) répertoriées dans le tableau de bord.

### Métriques d'ALB
{: #alb_metrics}

L'exportateur de métriques `alb-metrics-exporter` reformate automatiquement chaque zone de données dans le fichier JSON en métrique que peut lire Prometheus. Les métriques d'ALB collectent des données sur les connexions et les réponses traitées par l'ALB.
{: shortdesc}

Les métriques d'ALB sont au format `kube_system_<ALB-ID>_<METRIC-NAME> <VALUE>`. Par exemple, si un ALB reçoit 23 réponses avec des codes de statut 2xx-level, la métrique sera au format `kube_system_public_crf02710f54fcc40889c301bfd6d5b77fe_alb1_totalHandledRequest {.. metric="2xx"} 23`, où `metric` représente le libellé Prometheus.

Le tableau suivant présente la liste des noms de métriques d'ALB prises en charge avec les libellés de métrique au format `<ALB_metric_name>_<metric_label>`
<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Métriques d'ALB prises en charge</th>
</thead>
<tbody>
<tr>
<td><code>connections_reading</code></td>
<td>Nombre total de connexions client en lecture.</td>
</tr>
<tr>
<td><code>connections_accepted</code></td>
<td>Nombre total de connexions client acceptées.</td>
</tr>
<tr>
<td><code>connections_active</code></td>
<td>Nombre total de connexions client actives.</td>
</tr>
<tr>
<td><code>connections_handled</code></td>
<td>Nombre total de connexions client traitées.</td>
</tr>
<tr>
<td><code>connections_requests</code></td>
<td>Nombre total de connexions client demandées.</td>
</tr>
<tr>
<td><code>connections_waiting</code></td>
<td>Nombre total de connexions client en attente.</td>
</tr>
<tr>
<td><code>connections_writing</code></td>
<td>Nombre total de connexions clients en écriture.</td>
</tr>
<tr>
<td><code>totalHandledRequest_1xx</code></td>
<td>Nombre de réponses avec des codes de statut 1xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_2xx</code></td>
<td>Nombre de réponses avec des codes de statut 2xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_3xx</code></td>
<td>Nombre de réponses avec des codes de statut 3xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_4xx</code></td>
<td>Nombre de réponses avec des codes de statut 4xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_5xx</code></td>
<td>Nombre de réponses avec des codes de statut 5xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_total</code></td>
<td>Nombre total de demandes client reçues des clients.</td>
  </tr></tbody>
</table>

### Métriques de serveur
{: #server_metrics}

L'exportateur de métriques `alb-metrics-exporter` reformate automatiquement chaque zone de données dans le fichier JSON en métrique que peut lire Prometheus. Les métriques de serveur collectent des données sur le sous-domaine défini dans une ressource Ingress, par exemple, `dev.demostg1.stg.us.south.containers.appdomain.cloud`.
{: shortdesc}

Les métriques de serveur sont au format `kube_system_server_<ALB-ID>_<SUB-TYPE>_<SERVER-NAME>_<METRIC-NAME> <VALUE>`.

Les valeurs de `<SERVER-NAME>_<METRIC-NAME>` sont formatées sous forme de libellés. Par exemple, `albId="dev_demostg1_us-south_containers_appdomain_cloud",metric="out"`

Par exemple, si le serveur envoie 22319 octets au total aux clients, la métrique sera au format :
```
kube_system_server_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="dev_demostg1_us-south_containers_appdomain_cloud",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="out",pod_template_hash="3805183417"} 22319
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description du format des métriques de serveur</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ID de l'ALB. Dans l'exemple ci-dessus, l'ID de l'ALB est <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>.</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>Sous-type de métrique. Chaque sous-type correspond à un ou plusieurs noms de métrique.
<ul>
<li><code>bytes</code> et <code>processing\_time</code> correspondent aux métriques <code>in</code> et <code>out</code>.</li>
<li><code>cache</code> correspond aux métriques <code>bypass</code>, <code>expired</code>, <code>hit</code>, <code>miss</code>, <code>revalidated</code>, <code>scare</code>, <code>stale</code> et <code>updating</code>.</li>
<li><code>requests</code> correspond aux métriques <code>requestMsec</code>, <code>1xx</code>, <code>2xx</code>, <code>3xx</code>, <code>4xx</code>, <code>5xx</code> et <code>total</code>.</li></ul>
Dans l'exemple ci-dessus, le sous-type est <code>bytes</code>.</td>
</tr>
<tr>
<td><code>&lt;SERVER-NAME&gt;</code></td>
<td>Nom du serveur défini dans la ressource Ingress. Pour assurer la compatibilité avec Prometheus, les points (<code>.</code>) sont remplacés par des traits de soulignement <code>(\_)</code>. Dans l'exemple ci-dessus, le nom du serveur est <code>dev_demostg1_stg_us_south_containers_appdomain_cloud</code>.</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>Nom du type de métrique collectée. Pour obtenir une liste de noms de métrique, voir le tableau suivant : "Métriques de serveur prises en charge". Dans l'exemple ci-dessus, le nom de métrique est <code>out</code>.</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>Valeur de la métrique collectée. Dans l'exemple ci-dessus, la valeur est <code>22319</code>.</td>
</tr>
</tbody></table>

Le tableau suivant présente la liste des noms de métriques prises en charge.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Métriques de serveur prises en charge</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>Nombre total d'octets reçus des clients.</td>
</tr>
<tr>
<td><code>out</code></td>
<td>Nombre total d'octets envoyés aux clients.</td>
</tr>
<tr>
<td><code>bypass</code></td>
<td>Nombre de fois qu'un élément pouvant être mis en cache a été extrait du serveur d'origine car il ne respectait pas le seuil nécessaire pour figurer dans le cache (par exemple, le nombre de demandes).</td>
</tr>
<tr>
<td><code>expired</code></td>
<td>Nombre de fois qu'un élément en cache n'a pas été sélectionné car il était arrivé à expiration.</td>
</tr>
<tr>
<td><code>hit</code></td>
<td>Nombre de fois qu'un élément valide a été sélectionné dans le cache.</td>
</tr>
<tr>
<td><code>miss</code></td>
<td>Nombre de fois qu'aucun élément de cache valide n'a été trouvé dans le cache et que le serveur a extrait l'élément du serveur d'origine.</td>
</tr>
<tr>
<td><code>revalidated</code></td>
<td>Nombre de fois qu'un élément arrivé à expiration dans le cache a été revalidé.</td>
</tr>
<tr>
<td><code>scarce</code></td>
<td>Nombre de fois que le cache a supprimé des éléments rarement utilisés ou de faible priorité pour libérer un peu de mémoire.</td>
</tr>
<tr>
<td><code>stale</code></td>
<td>Nombre de fois qu'un élément arrivé à expiration a été trouvé en cache, mais comme une autre demande a entraîné l'extraction de cet élément par le serveur à partir du serveur d'origine, l'élément a été sélectionné dans le cache.</td>
</tr>
<tr>
<td><code>updating</code></td>
<td>Nombre de fois qu'un contenu périmé a été mis à jour.</td>
</tr>
<tr>
<td><code>requestMsec</code></td>
<td>Temps de traitement moyen de la demande en millisecondes.</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>Nombre de réponses avec des codes de statut 1xx.</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>Nombre de réponses avec des codes de statut 2xx.</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>Nombre de réponses avec des codes de statut 3xx.</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>Nombre de réponses avec des codes de statut 4xx.</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>Nombre de réponses avec des codes de statut 5xx.</td>
</tr>
<tr>
<td><code>total</code></td>
<td>Nombre total de réponses avec des codes de statut.</td>
  </tr></tbody>
</table>

### Métriques en amont
{: #upstream_metrics}

L'exportateur de métriques `alb-metrics-exporter` reformate automatiquement chaque zone de données dans le fichier JSON en métrique que peut lire Prometheus. Les métriques en amont collectent des données sur le service de back end défini dans une ressource Ingress.
{: shortdesc}

Les métriques en amont sont formatées de deux manières.
* [Type 1](#type_one) avec le nom du service en amont.
* [Type 2](#type_two) avec le nom du service en amont et une adresse IP de pod en amont spécifique.

#### Métriques en amont de type 1
{: #type_one}

Les métriques de type 1 en amont sont au format `kube_system_upstream_<ALB-ID>_<SUB-TYPE>_<UPSTREAM-NAME>_<METRIC-NAME> <VALUE>`.
{: shortdesc}

Les valeurs de `<UPSTREAM-NAME>_<METRIC-NAME>` sont formatées sous forme de libellés. Par exemple, `albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",metric="in"`

Par exemple, si le service en amont a reçu 1227 octets au total de l'ALB, la métrique est au format :
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="in",pod_template_hash="3805183417"} 1227
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description du format des métriques de type 1 en amont</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ID de l'ALB. Dans l'exemple ci-dessus, l'ID de l'ALB est <code>public\_crf02710f54fcc40889c301bfd6d5b77fe\_alb1</code>.</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>Sous-type de métrique. Les valeurs prises en charge sont <code>bytes</code>, <code>processing\_time</code> et <code>requests</code>. Dans l'exemple ci-dessus, le sous-type est <code>bytes</code>.</td>
</tr>
<tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>Nom du service en amont défini dans la ressource Ingress. Pour assurer la compatibilité avec Prometheus, les points (<code>.</code>) sont remplacés par des traits de soulignement <code>(\_)</code>. Dans l'exemple ci-dessus le nom du service en amont est <code>default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc</code>.</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>Nom du type de métrique collectée. Pour obtenir une liste de noms de métrique, voir le tableau suivant "Métriques de type 1 en amont prises en charge". Dans l'exemple ci-dessus, le nom de métrique est <code>in</code>.</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>Valeur de la métrique collectée. Dans l'exemple ci-dessus, la valeur est <code>1227</code>.</td>
</tr>
</tbody></table>

Le tableau suivant présente la liste des noms de métriques de type 1 en amont prises en charge.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Métriques de type 1 en amont prises en charge</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>Nombre total d'octets reçus du serveur d'ALB.</td>
</tr>
<tr>
<td><code>out</code></td>
<td>Nombre total d'octets envoyés au serveur d'ALB.</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>Nombre de réponses avec des codes de statut 1xx.</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>Nombre de réponses avec des codes de statut 2xx.</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>Nombre de réponses avec des codes de statut 3xx.</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>Nombre de réponses avec des codes de statut 4xx.</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>Nombre de réponses avec des codes de statut 5xx.</td>
</tr>
<tr>
<td><code>total</code></td>
<td>Nombre total de réponses avec des codes de statut.</td>
  </tr></tbody>
</table>

#### Métriques en amont de type 2
{: #type_two}

Les métriques de type 2 en amont sont au format `kube_system_upstream_<ALB-ID>_<METRIC-NAME>_<UPSTREAM-NAME>_<POD-IP> <VALUE>`.
{: shortdesc}

Les valeurs de `<UPSTREAM-NAME>_<POD-IP>` sont formatées sous forme de libellés. Par exemple, `albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",backend="172_30_75_6_80"`

Par exemple, si le service en amont a un temps moyen de traitement des demandes (y compris en amont) de 40 millisecondes, la métrique est au format :
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_requestMsec{albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",app="alb-metrics-exporter",backend="172_30_75_6_80",instance="172.30.75.3:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-swkls",pod_template_hash="3805183417"} 40
```

{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description du format de métrique de type 2 en amont</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>ID de l'ALB. Dans l'exemple ci-dessus, l'ID de l'ALB est <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>.</td>
</tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>Nom du service en amont défini dans la ressource Ingress. Pour assurer la compatibilité avec Prometheus, les points (<code>.</code>) sont remplacés par des traits de soulignement (<code>\_</code>). Dans l'exemple ci-dessus, le nom du service en amont est <code>demostg1\_stg\_us\_south\_containers\_appdomain\_cloud\_tea\_svc</code>.</td>
</tr>
<tr>
<td><code>&lt;POD\_IP&gt;</code></td>
<td>Adresse IP et port d'un pod de service en amont spécifique. Pour assurer la compatibilité avec Prometheus, les points (<code>.</code>) et les signes deux-points (<code>:</code>) sont remplacés par des traits de soulignement <code>(_)</code>. Dans l'exemple ci-dessus, l'adresse IP du pod en amont est <code>172_30_75_6_80</code>.</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>Nom du type de métrique collectée. Pour obtenir une liste de noms de métrique, voir le tableau suivant "Métriques de type 2 en amont prises en charge". Dans l'exemple ci-dessus, le nom de métrique est <code>requestMsec</code>.</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>Valeur de la métrique collectée. Dans l'exemple ci-dessus, la valeur est <code>40</code>.</td>
</tr>
</tbody></table>

Le tableau suivant présente la liste des noms de métriques de type 2 en amont prises en charge.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Métriques de type 2 en amont prises en charge</th>
</thead>
<tbody>
<tr>
<td><code>requestMsec</code></td>
<td>Temps moyen de traitement des demandes, y compris en amont, en millisecondes.</td>
</tr>
<tr>
<td><code>responseMsec</code></td>
<td>Temps de traitement moyen des réponses en amont uniquement, en millisecondes.</td>
  </tr></tbody>
</table>

<br />


## Augmentation de la taille de zone de mémoire partagée pour la collecte des données de mesure d'Ingress
{: #vts_zone_size}

Les zones de mémoire partagée sont définies pour que les processus de noeud worker puissent partager des informations de type cache, persistance de session et limites de débit. Une zone de mémoire partagée, désignée par zone d'état de trafic hôte virtuel (virtual host traffic status zone), est définie pour qu'Ingress collecte des données de mesure pour un équilibreur de charge d'application (ALB).
{:shortdesc}

Dans la ressource configmap `ibm-cloud-provider-ingress-cm` d'Ingress, la zone `vts-status-zone-size` définit la taille de la zone de mémoire partagée pour la collecte des données de mesure. Par défaut, la valeur de `vts-status-zone-size` est définie à `10m`. Si vous disposez d'un environnement plus important qui nécessite davantage de mémoire pour la collecte des données de mesure, vous pouvez remplacer la valeur par défaut par une valeur plus élevée en procédant comme suit :

Avant de commencer, vérifiez que vous disposez du [rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Auteur** ou **Responsable**](/docs/containers?topic=containers-users#platform) pour l'espace de nom `kube-system`.

1. Editez le fichier de configuration correspondant à la ressource configmap `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Modifiez la valeur de `vts-status-zone-size` en remplaçant `10m` par une valeur plus élevée.

   ```
   apiVersion: v1
   data:
     vts-status-zone-size: "10m"
   kind: ConfigMap
   metadata:
     name: ibm-cloud-provider-ingress-cm
     namespace: kube-system
   ```
   {: codeblock}

3. Sauvegardez le fichier de configuration.

4. Vérifiez que les modifications apportées à configmap ont été appliquées.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}
