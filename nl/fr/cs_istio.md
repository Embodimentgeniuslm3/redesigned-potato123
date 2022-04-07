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



# Utilisation du module complémentaire Istio géré (bêta)
{: #istio}

Istio sur {{site.data.keyword.containerlong}} offre une installation transparente d'Istio, des mises à jour automatiques et la gestion du cycle de vie des composants du plan de contrôle Istio, ainsi que l'intégration aux outils de surveillance et de consignation de la plateforme.
{: shortdesc}

En un seul clic, vous pouvez obtenir tous les composants de base d'Istio, des fonctions de suivi, de surveillance et de visualisation supplémentaires, ainsi que le modèle opérationnel d'application BookInfo. Istio sur {{site.data.keyword.containerlong_notm}} est proposé en tant que module complémentaire géré, de sorte qu'{{site.data.keyword.Bluemix_notm}} puisse conserver automatiquement tous vos composants Istio à jour.

## Description d'Istio sur {{site.data.keyword.containerlong_notm}}
{: #istio_ov}

### Qu'est-ce qu'Istio ?
{: #istio_ov_what_is}

[Istio ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/info/istio) est une plateforme ouverte de maillage de services, utilisée pour connecter, sécuriser, contrôler et observer des microservices sur des plateformes cloud, telles que Kubernetes dans {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Lorsque vous passez d'applications monolithiques à une architecture microservices distribuée, vous devez faire face à de nouveaux défis, tels que savoir contrôler le trafic de vos microservices, effectuer des lancements furtifs (dark launching) et des déploiements de contrôle de validité (canary rollout) de vos services, traiter les incidents, sécuriser la communication des services, observer les services et appliquer des politiques d'accès cohérentes à l'ensemble des services. Pour aborder ces difficultés, vous pouvez tirer parti d'un maillage de services. Un maillage de services fournit un réseau transparent indépendant du langage pour connecter, observer, sécuriser et contrôler la connectivité entre des microservices. Istio offre les connaissances et le contrôle du maillage de services afin de vous permettre de gérer le trafic réseau, d'équilibrer la charge sur les microservices, d'appliquer des politiques d'accès, de vérifier l'identité des services, etc.

Par exemple, l'utilisation d'Istio dans votre maillage de microservices peut vous aider à :
- Obtenir une meilleure visibilité des applications qui s'exécutent dans votre cluster
- Déployer des versions de contrôle de validité des applications et contrôler le trafic qu'elles reçoivent
- Activer le chiffrement automatique des données transférées entre les microservices
- Appliquer des limitations de débit et des règles de liste blanche et de liste noire s'appuyant sur des attributs

Un maillage de services Istio comprend un plan de données et un plan de contrôle. Le plan de données est constitué de proxys sidecar Envoy dans chaque pod d'application, qui assurent la communication entre les microservices. Le plan de contrôle est constitué de Pilot, de l'outil de télémétrie et de contrôle des règles d'accès Mixer, ainsi que de Citadel, qui appliquent des configurations Istio dans votre cluster. Pour plus d'informations sur chacun de ces composants, voir la [description du module complémentaire `istio`](#istio_components).

### Qu'est-ce qu'Istio sur {{site.data.keyword.containerlong_notm}} (bêta) ?
{: #istio_ov_addon}

Istio sur {{site.data.keyword.containerlong_notm}} est proposé en tant que module complémentaire géré qui intègre Istio directement dans votre cluster Kubernetes.
{: shortdesc}

Le module complémentaire Istio géré est classifié en tant que fonction bêta ; il peut être instable ou soumis à des changements fréquents. Les fonctions bêta, qui risquent de ne pas fournir le même niveau de performance ou de compatibilité que les fonctions de l'édition GA (General Availability), ne sont pas destinées à être utilisées dans un environnement de production.
{: note}

**Comment cela se présente dans mon cluster ?**</br>
Lorsque vous installez le module complémentaire Istio, les plans de contrôle et de données utilisent les réseaux locaux virtuels (VLAN) auxquels votre cluster est déjà connecté. Le trafic de la configuration circule sur le réseau privé au sein de votre cluster et ne nécessite pas l'ouverture de ports ou d'adresses supplémentaires dans votre pare-feu. Si vous exposez vos applications gérées par Istio avec une passerelle Istio, les demandes de trafic externe effectuées auprès des applications transitent via le VLAN public.

**Comment fonctionne le processus de mise à jour ?**</br>
La version Istio incluse dans le module complémentaire géré est testée par {{site.data.keyword.Bluemix_notm}} et approuvée en vue de son utilisation dans {{site.data.keyword.containerlong_notm}}. Pour mettre à jour vos composants Istio vers la version la plus récente d'Istio prise en charge par {{site.data.keyword.containerlong_notm}}, vous pouvez procéder comme indiqué dans la rubrique [Mise à jour de modules complémentaires gérés](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons).  

Si vous devez utiliser la version la plus récente d'Istio ou personnaliser votre installation Istio, vous pouvez installer la version open source d'Istio en suivant la procédure indiquée dans le [tutoriel de démarrage rapide avec {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/setup/kubernetes/quick-start-ibm/).
{: tip}

**Y a-t-il des limitations ?** </br>
Vous ne pouvez pas activer le module complémentaire Istio géré dans votre cluster si vous avez installé le [contrôleur d'admission Container Image Security Enforcer](/docs/services/Registry?topic=registry-security_enforce#security_enforce) dans votre cluster.

<br />


## Que puis-je installer ?
{: #istio_components}

Istio sur {{site.data.keyword.containerlong_notm}} est proposé sous la forme de trois modules complémentaires gérés dans votre cluster.
{: shortdesc}

<dl>
<dt>Istio (`istio`)</dt>
<dd>Installe les composants de base d'Istio, notamment Prometheus. Pour plus d'informations sur l'un des composants du plan de contrôle suivants, voir la [documentation Istio ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/concepts/what-is-istio/).
  <ul><li>`Envoy` passe par des proxys pour acheminer le trafic entrant et sortant de tous les services du maillage. Envoy est déployé en tant que conteneur sidecar dans le même pod que votre conteneur d'application.</li>
  <li>`Mixer` fournit des contrôle de règles et une collecte de télémétrie.<ul>
    <li>Les pods de télémétrie sont activés avec un noeud final Prometheus, qui rassemble toutes les données de télémétrie à partir des proxys sidecar Envoy et des services dans vos pods d'application.</li>
    <li>Les pods de règle appliquent le contrôle d'accès, notamment les limitations de débit et l'application de règles pour constituer une liste blanche et une liste noire.</li></ul>
  </li>
  <li>`Pilot` fournit la reconnaissance des services pour les composants sidecar Envoy et configure les règles de routage de gestion du trafic pour ces composants.</li>
  <li>`Citadel` fournit l'authentification et la gestion des identités pour l'authentification entre services et l'authentification des utilisateurs.</li>
  <li>`Galley` valide les modifications de configuration des autres composants du plan de contrôle Istio.</li>
</ul></dd>
<dt>Fonctions supplémentaires d'Istio (`istio-extras`)</dt>
<dd>Facultatif : installe [Grafana ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://grafana.com/), [Jaeger ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.jaegertracing.io/) et [Kiali ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.kiali.io/) pour fournir d'autres fonctions de surveillance, suivi et visualisation pour Istio.</dd>
<dt>Application modèle BookInfo (`istio-sample-bookinfo`)</dt>
<dd>Facultatif : déploie le [modèle d'application BookInfo pour Istio ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/examples/bookinfo/). Ce déploiement comprend la configuration de démonstration de base et les règles de destination par défaut pour que vous puissiez utiliser les fonctions d'Istio immédiatement.</dd>
</dl>

<br>
Vous pouvez toujours voir les modules complémentaires Istio activés dans votre cluster en exécutant la commande suivante :
```
ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
```
{: pre}

<br />


## Installation d'Istio sur {{site.data.keyword.containerlong_notm}}
{: #istio_install}

Installez les modules complémentaires gérés Istio dans un cluster existant.
{: shortdesc}

**Avant de commencer**</br>
* Vérifiez que vous disposez du [rôle de service **Writer** ou **Manager** d'{{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) pour {{site.data.keyword.containerlong_notm}}.
* [Créez ou utilisez un cluster standard existant avec au moins trois noeuds worker ayant chacun 4 coeurs et 16 Go de mémoire (`b3c.4x16`) ou plus](/docs/containers?topic=containers-clusters#clusters_ui). En outre, le cluster et les noeuds worker doivent au moins exécuter la version minimale prise en charge de Kubernetes, que vous pouvez consulter en exécutant la commande `ibmcloud ks addon-versions --addon istio`.
* [Ciblez l'interface CLI sur votre cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
* Si vous utilisez un cluster existant et que vous avez déjà installé Istio dans le cluster à l'aide de la charte Helm IBM ou via une autre méthode, [nettoyez cette installation Istio](#istio_uninstall_other).

### Installation des modules complémentaires Istio dans l'interface de ligne de commande
{: #istio_install_cli}

1. Activez le module complémentaire `istio`.
  ```
  ibmcloud ks cluster-addon-enable istio --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. Facultatif : activez le module complémentaire `istio-extras`.
  ```
  ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID>
  ```
  {: pre}

3. Facultatif : activez le module complémentaire `istio-sample-bookinfo`.
  ```
  ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID>
  ```
  {: pre}

4. Vérifiez que les modules complémentaires Istio gérés que vous avez installés sont activés dans ce cluster.
  ```
  ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Exemple de sortie :
  ```
  Name                      Version
  istio                     1.1.5
  istio-extras              1.1.5
  istio-sample-bookinfo     1.1.5
  ```
  {: screen}

5. Vous pouvez également consulter les composants individuels de chaque module complémentaire dans votre cluster.
  - Composants d'`istio` et `istio-extras` : vérifiez que les services Istio et les pods correspondants sont déployés.
    ```
    kubectl get svc -n istio-system
    ```
    {: pre}

    ```
    NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
    grafana                  ClusterIP      172.21.98.154    <none>          3000/TCP                                                       2m
    istio-citadel            ClusterIP      172.21.221.65    <none>          8060/TCP,9093/TCP                                              2m
    istio-egressgateway      ClusterIP      172.21.46.253    <none>          80/TCP,443/TCP                                                 2m
    istio-galley             ClusterIP      172.21.125.77    <none>          443/TCP,9093/TCP                                               2m
    istio-ingressgateway     LoadBalancer   172.21.230.230   169.46.56.125   80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                              8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP   2m
    istio-pilot              ClusterIP      172.21.171.29    <none>          15010/TCP,15011/TCP,8080/TCP,9093/TCP                          2m
    istio-policy             ClusterIP      172.21.140.180   <none>          9091/TCP,15004/TCP,9093/TCP                                    2m
    istio-sidecar-injector   ClusterIP      172.21.248.36    <none>          443/TCP                                                        2m
    istio-telemetry          ClusterIP      172.21.204.173   <none>          9091/TCP,15004/TCP,9093/TCP,42422/TCP                          2m
    jaeger-agent             ClusterIP      None             <none>          5775/UDP,6831/UDP,6832/UDP                                     2m
    jaeger-collector         ClusterIP      172.21.65.195    <none>          14267/TCP,14268/TCP                                            2m
    jaeger-query             ClusterIP      172.21.171.199   <none>          16686/TCP                                                      2m
    kiali                    ClusterIP      172.21.13.35     <none>          20001/TCP                                                      2m
    prometheus               ClusterIP      172.21.105.229   <none>          9090/TCP                                                       2m
    tracing                  ClusterIP      172.21.125.177   <none>          80/TCP                                                         2m
    zipkin                   ClusterIP      172.21.1.77      <none>          9411/TCP                                                       2m
    ```
    {: screen}

    ```
    kubectl get pods -n istio-system
    ```
    {: pre}

    ```
    NAME                                      READY   STATUS    RESTARTS   AGE
    grafana-76dcdfc987-94ldq                  1/1     Running   0          2m
    istio-citadel-869c7f9498-wtldz            1/1     Running   0          2m
    istio-egressgateway-69bb5d4585-qxxbp      1/1     Running   0          2m
    istio-galley-75d7b5bdb9-c9d9n             1/1     Running   0          2m
    istio-ingressgateway-5c8764db74-gh8xg     1/1     Running   0          2m
    istio-pilot-55fd7d886f-vv6fb              2/2     Running   0          2m
    istio-policy-6bb6f6ddb9-s4c8t             2/2     Running   0          2m
    istio-sidecar-injector-7d9845dbb7-r8nq5   1/1     Running   0          2m
    istio-telemetry-7695b4c4d4-tlvn8          2/2     Running   0          2m
    istio-tracing-55bbf55878-z4rd2            1/1     Running   0          2m
    kiali-77566cc66c-kh6lm                    1/1     Running   0          2m
    prometheus-5d5cb44877-lwrqx               1/1     Running   0          2m
    ```
    {: screen}

  - Composants d'`istio-sample-bookinfo` : vérifiez que les microservices de l'application BookInfo et les pods correspondants sont déployés.
    ```
    kubectl get svc -n default
    ```
    {: pre}

    ```
    NAME                      TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)          AGE
    details                   ClusterIP      172.21.19.104    <none>         9080/TCP         2m
    productpage               ClusterIP      172.21.168.196   <none>         9080/TCP         2m
    ratings                   ClusterIP      172.21.11.131    <none>         9080/TCP         2m
    reviews                   ClusterIP      172.21.117.164   <none>         9080/TCP         2m
    ```
    {: screen}

    ```
    kubectl get pods -n default
    ```
    {: pre}

    ```
    NAME                                     READY     STATUS      RESTARTS   AGE
    details-v1-6865b9b99d-7v9h8              2/2       Running     0          2m
    productpage-v1-f8c8fb8-tbsz9             2/2       Running     0          2m
    ratings-v1-77f657f55d-png6j              2/2       Running     0          2m
    reviews-v1-6b7f6db5c5-fdmbq              2/2       Running     0          2m
    reviews-v2-7ff5966b99-zflkv              2/2       Running     0          2m
    reviews-v3-5df889bcff-nlmjp              2/2       Running     0          2m
    ```
    {: screen}

### Installation des modules complémentaires Istio gérés dans l'interface utilisateur
{: #istio_install_ui}

1. Dans le [tableau de bord de votre cluster ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/clusters), cliquez sur le nom d'un cluster.

2. Cliquez sur l'onglet **Modules complémentaires**.

3. Sur la carte Istio, cliquez sur **Installer**.

4. La case à cocher **Istio** est déjà sélectionnée. Pour installer également les modules complémentaires Istio extras et l'application modèle BookInfo, sélectionnez les cases à cocher **Istio Extras** et **Istio Sample**.

5. Cliquez sur **Installer**.

6. Sur la carte Istio, vérifiez que les modules complémentaires que vous avez activés sont répertoriés.

Vous pouvez ensuite tester les fonctions d'Istio en expérimentant le [modèle d'application BookInfo](#istio_bookinfo).

<br />


## Test du modèle d'application BookInfo
{: #istio_bookinfo}

Le module complémentaire BookInfo (`istio-sample-bookinfo`) déploie le [modèle d'application BookInfo pour Istio ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/examples/bookinfo/) dans l'espace de nom `default`. Ce déploiement comprend la configuration de démonstration de base et les règles de destination par défaut pour que vous puissiez utiliser les fonctions d'Istio immédiatement.
{: shortdesc}

Les quatre microservices de l'application BookInfo comprennent :
* `productpage` appelle les microservices `details` et `reviews` pour alimenter la page.
* `details` contient les informations sur l'ouvrage.
* `reviews` contient les critiques du livre et appelle le microservice `ratings`.
* `ratings` contient les informations de classement du livre qui accompagnent une critique de livre.

Le microservice `reviews` comporte plusieurs versions :
* `v1` n'appelle pas le microservice `ratings`.
* `v2` appelle le microservice `ratings` et affiche le classement sous forme d'étoiles noires de 1 à 5.
* `v3` appelle le microservice `ratings` et affiche le classement sous forme d'étoiles rouges de 1 à 5.

Les fichiers YAML de déploiement de chacun de ces microservices sont modifiés pour les proxys sidecar Envoy soient pré-injectés sous forme de conteneurs dans les pods des microservices avant d'être déployés. Pour plus d'informations sur l'injection manuelle de composants sidecar, voir la [documentation Istio ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/setup/kubernetes/sidecar-injection/). L'application BookInfo est déjà exposé sur une adresse IP publique Ingress par une passerelle Istio. Bien que l'application BookInfo puisse vous aider à démarrer, cette application n'est pas conçue pour être utilisée en production.

Avant de commencer, [installez les modules complémentaires gérés `istio`, `istio-extras` et `istio-sample-bookinfo`](#istio_install) dans un cluster.

1. Obtenez l'adresse publique de votre cluster.
  1. Définissez l'hôte Ingress.
    ```
    export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
    ```
    {: pre}

  2. Définissez le port Ingress.
    ```
    export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
    ```
    {: pre}

  3. Créez une variable d'environnement `GATEWAY_URL` utilisant le port et l'hôte Ingress.
     ```
     export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
     ```
     {: pre}

2. Exécutez la commande curl sur la variable `GATEWAY_URL` pour vérifier que l'application BookInfo est en cours d'exécution. Une réponse `200` indique que l'application  BookInfo s'exécute correctement avec Istio.
   ```
   curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
   ```
   {: pre}

3.  Affichez la page Web BookInfo dans un navigateur.

    Mac OS ou Linux :
    ```
    open http://$GATEWAY_URL/productpage
    ```
    {: pre}

    Windows :
    ```
    start http://$GATEWAY_URL/productpage
    ```
    {: pre}

4. Essayez d'actualiser la page plusieurs fois. Différentes versions de la section reviews affichent des étoiles rouges, des étoiles noires, ou pas d'étoiles.

### Comprendre ce qui se passe
{: #istio_bookinfo_understanding}

Le modèle d'application BookInfo illustre comment trois composants de gestion du trafic d'Istio s'articulent pour acheminer le trafic entrant vers l'application.
{: shortdesc}

<dl>
<dt>`Gateway`</dt>
<dd>La [passerelle `bookinfo-gateway` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/) décrit un équilibreur de charge, le service `istio-ingressgateway` dans l'espace de nom `istio-system`, qui fait office de point d'entrée ingress pour le trafic HTTP/TCP de l'application BookInfo. Istio configure l'équilibreur de charge pour qu'il soit à l'écoute des demandes entrantes vers les applications gérées par Istio sur les ports définis dans le fichier de configuration de la passerelle.
</br></br>Pour voir le fichier de configuration de la passerelle BookInfo, exécutez la commande suivante :
<pre class="pre"><code>kubectl get gateway bookinfo-gateway -o yaml</code></pre></dd>

<dt>`VirtualService`</dt>
<dd>Le [`service virtuel` `bookinfo` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/) définit les règles qui régissent le mode de routage des demandes au sein du maillage de services en définissant les microservices en tant que `destinations`. Dans le service virtuel `bookinfo`, l'URI `/productpage` d'une demande est acheminé vers l'hôte `productpage` sur le port `9080`. Ainsi, toutes les demandes pour l'application BookInfo sont d'abord acheminées vers le microservice `productpage`, qui appelle ensuite les autres microservices de BookInfo.
</br></br>Pour voir la règle de service virtuel appliquée à BookInfo, exécutez la commande suivante :
<pre class="pre"><code>kubectl get virtualservice bookinfo -o yaml</code></pre></dd>

<dt>`DestinationRule`</dt>
<dd>Une fois que la passerelle a acheminé la demande selon la règle de service virtuel, les [`règles de destination` `details`, `productpage`, `ratings` et `reviews` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/) définissent des règles qui sont appliquées à la demande lorsqu'elle atteint un microservice. Par exemple, lorsque vous actualisez la page de produit de BookInfo, les modifications que vous voyez sont le résultat de l'appel aléatoire de différentes versions,`v1`, `v2` et `v3` du microservice `reviews`, par le microservice `productpage`. Les versions sont sélectionnées de manière aléatoire car la règle de destination `reviews` confère la même pondération aux sous-ensembles (`subsets`), ou aux versions nommées, du microservice. Ces sous-ensembles sont utilisés par les règles de service virtuel lorsque le trafic est acheminé vers des versions spécifiques du service.
</br></br>Pour voir les règles de destination appliquées à BookInfo, exécutez la commande suivante :
<pre class="pre"><code>kubectl describe destinationrules</code></pre></dd>
</dl>

</br>

Vous pouvez ensuite [exposer BookInfo en utilisant le sous-domaine Ingress fourni par IBM](#istio_expose_bookinfo) ou [consigner, surveiller, suivre et visualiser](#istio_health) le maillage de services de l'application BookInfo.

<br />


## Consignation, surveillance, suivi et visualisation d'Istio
{: #istio_health}

Pour consigner, surveiller, suivre et visualiser vos applications gérées par Istio sur {{site.data.keyword.containerlong_notm}}, vous pouvez lancer les tableaux de bord Grafana, Jaeger et Kiali qui sont installés dans le module complémentaire `istio-extras` ou déployer LogDNA et Sysdig en tant que services tiers sur vos noeuds worker.
{: shortdesc}

### Lancement des tableaux de bord Grafana, Jaeger et Kiali
{: #istio_health_extras}

Le module complémentaire Istio extras (`istio-extras`) installe [Grafana ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://grafana.com/), [Jaeger ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.jaegertracing.io/) et [Kiali ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.kiali.io/). Lancez les tableaux de bord de chacun de ces services pour offrir des fonctions de surveillance, de suivi et de visualisation supplémentaires pour Istio.
{: shortdesc}

Avant de commencer, [installez les modules complémentaires gérés `istio` et `istio-extras`](#istio_install) dans un cluster.

**Grafana**</br>
1. Démarrez l'acheminement du port Kubernetes pour le tableau de bord Grafana.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
  ```
  {: pre}

2. Pour ouvrir le tableau de bord Grafana d'Istio, accédez à l'URL suivante : http://localhost:3000/dashboard/db/istio-mesh-dashboard. Si vous avez installé le [module complémentaire BookInfo](#istio_bookinfo), le tableau de bord Istio affiche les métriques du trafic que vous avez généré lorsque vous avez actualisé plusieurs fois la page de produit. Pour plus d'informations sur l'utilisation du tableau de bord Grafana d'Istio, voir [Viewing the Istio Dashboard ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/tasks/telemetry/using-istio-dashboard/) dans la documentation open source d'Istio.

**Jaeger**</br>
1. Par défaut, Istio génère des intervalles de trace de 1 pour 100 demandes, ce qui correspond à une fréquence d'échantillonnage de 1 %. Vous devez envoyer au moins 100 demandes pour que la première trace soit visible. Pour envoyer 100 demandes au service `productpage` du [module complémentaire BookInfo](#istio_bookinfo), exécutez la commande suivante :
  ```
  for i in `seq 1 100`; do curl -s -o /dev/null http://$GATEWAY_URL/productpage; done
  ```
  {: pre}

2. Démarrez l'acheminement du port Kubernetes pour le tableau de bord Jaeger.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686 &
  ```
  {: pre}

3. Pour ouvrir l'interface utilisateur de Jaeger, accédez à l'URL suivante : http://localhost:16686.

4. Si vous avez installé le module complémentaire BookInfo, vous pouvez sélectionner `productpage` dans la liste **Service** et cliquer sur **Find Traces**. Les traces du trafic que vous avez générées lorsque vous avez actualisé plusieurs fois la page de produit s'affichent. Pour plus d'informations sur l'utilisation de Jaeger avec Istio, voir [Generating traces using the BookInfo sample ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/tasks/telemetry/distributed-tracing/#generating-traces-using-the-bookinfo-sample) dans la documentation open source d'Istio.

**Kiali**</br>
1. Démarrez l'acheminement du port Kubernetes pour le tableau de bord Kiali.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001 &
  ```
  {: pre}

2. Pour ouvrir l'interface utilisateur de Kiali, accédez à l'URL suivante : http://localhost:20001/kiali/console.

3. Entrez `admin` pour le nom d'utilisateur et la phrase passe. Pour plus d'informations sur l'utilisation de Kiali pour visualiser vos microservices gérés par Istio, voir [Generating a service graph ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://archive.istio.io/v1.0/docs/tasks/telemetry/kiali/#generating-a-service-graph) dans la documentation open source d'Istio.

### Configuration de la consignation avec {{site.data.keyword.la_full_notm}}
{: #istio_health_logdna}

Gérez en toute transparence les journaux de votre conteneur d'application et du conteneur de proxy sidecar Envoy dans chaque pod en déployant LogDNA sur vos noeuds worker pour transférer les journaux à {{site.data.keyword.loganalysislong}}.
{: shortdesc}

Pour utiliser [{{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about), déployez un agent de consignation sur tous les noeuds worker de votre cluster. Cet agent collecte les journaux avec l'extension `*.log` et les fichiers sans extension stockés dans le répertoire `/var/log` de votre pod à partir de tous les espaces de nom, y compris `kube-system`. Ces journaux comprennent les journaux de votre conteneur d'application et du conteneur de proxy sidecar Envoy dans chaque pod. L'agent transfère ensuite les journaux au service {{site.data.keyword.la_full_notm}}.

Pour commencer, configurez LogDNA pour votre cluster en suivant la procédure indiquée dans [Gestion des journaux de cluster Kubernetes avec {{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).




### Configuration de la surveillance avec {{site.data.keyword.mon_full_notm}}
{: #istio_health_sysdig}

Obtenez davantage de visibilité sur les performances et l'état de santé de vos applications gérées par Istio en déployant Sysdig sur vos noeuds worker afin de transférer des métriques à {{site.data.keyword.monitoringlong}}.
{: shortdesc}

Avec Istio sur {{site.data.keyword.containerlong_notm}}, le module complémentaire `istio` géré installe Prometheus sur votre cluster. Les pods `istio-mixer-telemetry` de votre cluster sont annotés avec un noeud final Prometheus pour que Prometheus puisse rassembler toutes les données télémétriques pour vos pods. Lorsque vous déployez un agent Sysdig sur tous les noeuds worker de votre cluster, Sysdig est déjà automatiquement activé pour détecter et décomposer les données à partir de ces noeuds finaux Prometheus afin de les afficher dans votre tableau de bord de surveillance {{site.data.keyword.Bluemix_notm}}.

Comme tout le travail de Prometheus est déjà réalisé, tout ce qui vous reste à faire est de déployer Sysdig dans votre cluster.

1. Configurez Sysdig en suivant la procédure indiquée dans [Analyzing metrics for an app that is deployed in a Kubernetes cluster](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster).

2. [Lancez l'interface utilisateur de Sysdig ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_step3).

3. Cliquez sur **Add new dashboard**.

4. Recherchez `Istio` et sélectionnez l'un des tableaux de bord Istio prédéfinis de Sysdig.

Pour plus d'informations sur les références liées aux métriques et aux tableaux de bord, la surveillance des composants internes d'Istio et la surveillance des déploiements A/B d'Istio et les déploiements de contrôle de validité, consultez l'article [How to monitor Istio, the Kubernetes service mesh ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://sysdig.com/blog/monitor-istio/). Recherchez la section intitulée "Monitoring Istio: reference metrics and dashboards".

<br />


## Configuration de l'injection de composants sidecar pour vos applications
{: #istio_sidecar}

Prêt à gérer vos propres applications avec Istio ? Avant de déployer votre application, vous devez d'abord déterminer si vous voulez injecter des proxys sidecar Envoy sur les pods d'application.
{: shortdesc}

Chaque pod d'application doit exécuter un proxy sidecar Envoy de sorte que les microservices puissent être inclus dans le maillage de services. Vous pouvez vérifier si les composants sidecar sont injectés dans chaque pod d'appplication automatiquement ou manuellement. Pour plus d'informations sur l'injection de composants sidecar, voir la [documentation Istio ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/setup/kubernetes/sidecar-injection/).

### Activation de l'injection automatique de composants sidecar
{: #istio_sidecar_automatic}

Lorsque l'injection automatique de composants sidecar est activée, un espace de nom est à l'écoute de tout nouveau déploiement et modifie automatiquement la spécification de modèle de pod de sorte que les pods d'application oient créés avec des conteneurs de proxy sidecar Envoy. Activez l'injection automatique de composants sidecar pour un espace de nom lorsque vous envisagez de déployer plusieurs applications que vous souhaitez intégrer à Istio dans cet espace de nom. Notez que l'injection automatique de composants sidecar n'est pas activée pour les espaces de nom par défaut dans le module complémentaire géré Istio.

Pour activer l'injection automatique de composants sidecar pour un espace de nom :

1. Obtenez le nom de l'espace de nom dans lequel vous voulez déployer les applications gérées par Istio.
  ```
  kubectl get namespaces
  ```
  {: pre}

2. Labellisez l'espace de nom avec `istio-injection=enabled`.
  ```
  kubectl label namespace <namespace> istio-injection=enabled
  ```
  {: pre}

3. Déployez les applications dans l'espace de nom labellisé ou redéployez les applications figurant déjà dans l'espace de nom.
  * Pour déployer une application dans l'espace de nom labellisé :
    ```
    kubectl apply <myapp>.yaml --namespace <namespace>
    ```
    {: pre}
  * Pour redéployer une application déjà déployée dans cet espace de nom, supprimez le pod d'application de sorte à ce qu'il soit redéployé avec le composant sidecar injecté.
    ```
    kubectl delete pod -l app=<myapp>
    ```
    {: pre}

5. Si vous n'avez pas créé de service pour exposer votre application, créez un service Kubernetes. Votre application doit être exposée par un service Kubernetes pour être incluse en tant que microservice dans le maillage de services Istio. Veillez à suivre la [configuration requise d'Istio pour les pods et les services ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/setup/kubernetes/spec-requirements/).

  1. Définissez un service pour l'application.
    ```
    apiVersion: v1
          kind: Service
          metadata:
            name: myappservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
       - protocol: TCP
             port: 8080
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML du service</th>
    </thead>
    <tbody>
    <tr>
    <td><code>selector</code></td>
    <td>Entrez la paire clé de libellé (<em>&lt;selector_key&gt;</em>) et valeur (<em>&lt;selector_value&gt;</em>) que vous désirez utiliser pour cibler les pods dans lesquels s'exécute votre application.</td>
     </tr>
     <tr>
     <td><code>port</code></td>
     <td>Port sur lequel le service est à l'écoute.</td>
     </tr>
     </tbody></table>

  2. Créez le service dans votre cluster. Vérifiez que le service se déploie dans le même espace de nom que l'application.
    ```
    kubectl apply -f myappservice.yaml -n <namespace>
    ```
    {: pre}

Les pods d'application sont désormais intégrés dans votre maillage de services Istio car ils disposent d'un conteneur sidecar Istio qui s'exécute aux côtés de votre conteneur d'application.

### Injection manuelle de composants sidecar
{: #istio_sidecar_manual}

Si vous ne souhaitez pas activer l'injection automatique de composants sidecar sur un espace de nom, vous pouvez effectuer l'injection manuellement dans un fichier YAML de déploiement. Injectez des composants sidecar manuellement lorsque les applications s'exécutent dans des espaces de nom aux côtés d'autres déploiements dans lesquels vous ne voulez pas que les composants sidecar soient ajoutés automatiquement.

Pour injecter manuellement des composants sidecar dans un déploiement :

1. Téléchargez le client `istioctl`.
  ```
  curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.1.5 sh -
  ```

2. Accédez au répertoire du package Istio.
  ```
  cd istio-1.1.5
  ```
  {: pre}

3. Injectez le composant sidecar Envoy dans le fichier YAML de déploiement de votre application.
  ```
  istioctl kube-inject -f <myapp>.yaml | kubectl apply -f -
  ```
  {: pre}

4. Déployez votre application.
  ```
  kubectl apply <myapp>.yaml
  ```
  {: pre}

5. Si vous n'avez pas créé de service pour exposer votre application, créez un service Kubernetes. Votre application doit être exposée par un service Kubernetes pour être incluse en tant que microservice dans le maillage de services Istio. Veillez à suivre la [configuration requise d'Istio pour les pods et les services ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/setup/kubernetes/spec-requirements/).

  1. Définissez un service pour l'application.
    ```
    apiVersion: v1
          kind: Service
          metadata:
            name: myappservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
       - protocol: TCP
             port: 8080
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML du service</th>
    </thead>
    <tbody>
    <tr>
    <td><code>selector</code></td>
    <td>Entrez la paire clé de libellé (<em>&lt;selector_key&gt;</em>) et valeur (<em>&lt;selector_value&gt;</em>) que vous désirez utiliser pour cibler les pods dans lesquels s'exécute votre application.</td>
     </tr>
     <tr>
     <td><code>port</code></td>
     <td>Port sur lequel le service est à l'écoute.</td>
     </tr>
     </tbody></table>

  2. Créez le service dans votre cluster. Vérifiez que le service se déploie dans le même espace de nom que l'application.
    ```
    kubectl apply -f myappservice.yaml -n <namespace>
    ```
    {: pre}

Les pods d'application sont désormais intégrés dans votre maillage de services Istio car ils disposent d'un conteneur sidecar Istio qui s'exécute aux côtés de votre conteneur d'application.

<br />


## Exposition d'applications gérées par Istio à l'aide d'un nom d'hôte fourni par IBM
{: #istio_expose}

Après avoir [configuré l'injection de proxy sidecar Envoy](#istio_sidecar) et déployé vos applications dans le maillage de services Istio, vous pouvez exposer vos applications gérées par Istio aux demandes publiques en utilisant le nom d'hôte fourni par IBM.
{: shortdesc}

Istio utilise des [passerelles ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/) et des [services virtuels![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/) pour contrôler le routage du trafic vers vos applications. Une passerelle configure un équilibreur de charge, `istio-ingressgateway`, qui fait office de point d'entrée pour vos applications gérées par Istio. Vous pouvez exposer vos applications gérés par Istio en enregistrant l'adresse IP externe de l'équilibreur de charge `istio-ingressgateway` avec une entrée DNS et un nom d'hôte. 

Vous pouvez d'abord tester l'[exemple d'exposition de l'application BookInfo](#istio_expose_bookinfo) ou [exposer au public vos propres applications gérées par Istio](#istio_expose_link).

### Exemple : Exposition de l'application BookInfo à l'aide d'un nom d'hôte fourni par IBM
{: #istio_expose_bookinfo}

Lorsque vous activez le module complémentaire BookInfo dans votre cluster, la passerelle Istio `bookinfo-gateway` est créée pour vous. Cette passerelle utilise le service virtuel et les règles de destination Istio pour configurer un équilibreur de charge, `istio-ingressgateway`, qui expose l'application BookInfo au public. Dans les étapes suivantes, vous allez créer un nom d'hôte pour l'adresse IP d'équilibreur de charge `istio-ingressgateway` par l'intermédiaire de laquelle vous pouvez accéder publiquement à l'application BookInfo.
{: shortdesc}

Avant de commencer, [activez le module complémentaire géré `istio-sample-bookinfo`](#istio_install) dans un cluster.

1. Procurez-vous l'adresse **EXTERNAL-IP** pour l'équilibreur de charge `istio-ingressgateway`.
  ```
  kubectl get svc -n istio-system
  ```
  {: pre}

  Dans l'exemple suivant, la valeur de **EXTERNAL-IP** est `168.1.1.1`.
  ```
  NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
  ...
  istio-ingressgateway     LoadBalancer   172.21.XXX.XXX   169.1.1.1       80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                            8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP  22m
  ```
  {: screen}

2. Enregistrez l'adresse IP en créant un nom d'hôte DNS.
  ```
  ibmcloud ks nlb-dns-create --cluster <cluster_name_or_id> --ip <LB_IP>
  ```
  {: pre}

3. Vérifiez que le nom d'hôte est créé.
  ```
  ibmcloud ks nlb-dnss --cluster <cluster_name_or_id>
  ```
  {: pre}

  Exemple de sortie :
  ```
  Hostname                                                                                IP(s)              Health Monitor   SSL Cert Status           SSL Cert Secret Name
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     ["168.1.1.1"]      None             created                   <certificate>
  ```
  {: screen}

4. Dans un navigateur Web, ouvrez la page de produit BookInfo.
  ```
  https://<host_name>/productpage
  ```
  {: codeblock}

5. Essayez d'actualiser la page plusieurs fois. Les demandes adressées à `http://<host_name>/productpage` sont reçues par l'ALB et transmises à l'équilibreur de charge de la passerelle Istio. Les différentes versions du microservice `reviews` sont toujours renvoyées de manière aléatoire car la passerelle Istio gère les règles de service virtuel et de routage de destination pour les microservices.

Pour plus d'informations sur la passerelle, les règles de service virtuel et les règles de destination pour l'application BookInfo, voir [Comprendre ce qui se passe](#istio_bookinfo_understanding). Pour plus d'informations sur l'enregistrement de noms d'hôte DNS dans {{site.data.keyword.containerlong_notm}}, voir [Enregistrement d'un nom d'hôte NLB](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname).

### Exposition publique d'applications gérées par Istio à l'aide d'un nom d'hôte fourni par IBM
{: #istio_expose_link}

Exposez publiquement vos applications gérées par Istio en créant une passerelle Istio, un service virtuel qui définit les règles de gestion de trafic pour vos services gérés par Istio et un nom d'hôte DNS pour l'adresse IP externe de l'équilibreur de charge `istio-ingressgateway`.
{: shortdesc}

**Avant de commencer :**
1. [Installez le module complémentaire géré `istio`](#istio_install) dans un cluster.
2. Installez le client `istioctl`.
  1. Téléchargez `istioctl`.
    ```
    curl -L https://git.io/getLatestIstio | sh -
    ```
  2. Accédez au répertoire du package Istio.
    ```
    cd istio-1.1.5
    ```
    {: pre}
3. [Configurez l'injection de composants sidecar pour vos microservices d'application, déployez les microservices d'application dans un espace de nom et créez des services Kubernetes pour les microservices d'application pour qu'ils soient inclus dans le maillage de services Istio](#istio_sidecar).

</br>
**Pour exposer publiquement vos applications gérées par Istio à l'aide d'un nom d'hôte :**

1. Créez une passerelle. Cet exemple de passerelle utilise le service d'équilibreur de charge `istio-ingressgateway` pour exposer le port 80 pour HTTP. Remplacez`<namespace>` par l'espace de nom dans lequel vos microservices gérés par Istio sont déployés. Si vos microservices sont à l'écoute sur un autre port que `80`, ajoutez ce port. Pour plus d'informations sur les composants du fichier YAML de la passerelle, voir la [documentation de référence Istio ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/).
  ```
  apiVersion: networking.istio.io/v1alpha3
  kind: Gateway
  metadata:
    name: my-gateway
    namespace: <namespace>
  spec:
    selector:
      istio: ingressgateway
    servers:
    - port:
        name: http
        number: 80
        protocol: HTTP
      hosts:
      - '*'
  ```
  {: codeblock}

2. Appliquez la passerelle dans l'espace de nom dans lequel sont déployés vos microservices gérés par Istio.
  ```
  kubectl apply -f my-gateway.yaml -n <namespace>
  ```
  {: pre}

3. Créez un service virtuel qui utilise la passerelle `my-gateway` et définit des règles de routage pour vos microservices d'application. Pour plus d'informations sur les composants du fichier YAML du service virtuel, voir la [documentation de référence Istio ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/).
  ```
  apiVersion: networking.istio.io/v1alpha3
  kind: VirtualService
  metadata:
    name: my-virtual-service
    namespace: <namespace>
  spec:
    gateways:
    - my-gateway
    hosts:
    - '*'
    http:
    - match:
      - uri:
          exact: /<service_path>
      route:
      - destination:
          host: <service_name>
          port:
            number: 80
  ```
  {: codeblock}

  <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
  </thead>
  <tbody>
  <tr>
  <td><code>namespace</code></td>
  <td>Remplacez <em>&lt;namespace&gt;</em> par l'espace de nom dans lequel sont déployés vos microservices gérés par Istio.</td>
  </tr>
  <tr>
  <td><code>gateways</code></td>
  <td><code>my-gateway</code> est spécifié pour que la passerelle puisse appliquer ces règles de routage de service virtuel à l'équilibreur de charge <code>istio-ingressgateway</code>.<td>
  </tr>
  <tr>
  <td><code>http.match.uri.exact</code></td>
  <td>Remplacez <em>&lt;service_path&gt;</em> par le chemin utilisé par le microservice de point d'entrée pour être à l'écoute. Par exemple, dans l'application BookInfo, le chemin est défini par <code>/productpage</code>.</td>
  </tr>
  <tr>
  <td><code>http.route.destination.host</code></td>
  <td>Remplacez <em>&lt;service_name&gt;</em> par le nom de votre microservice de point d'entrée. Par exemple, dans l'application BookInfo, <code>productpage</code> fait office de microservice de point d'entrée appelant les autres microservices d'application.</td>
  </tr>
  <tr>
  <td><code>http.route.destination.port.number</code></td>
  <td>Si votre microservice est à l'écoute sur un autre port, remplacez <em>&lt;80&gt;</em> par ce port.</td>
  </tr>
  </tbody></table>

4. Appliquez les règles de service virtuel dans l'espace de nom dans lequel est déployé votre microservice géré par Istio.
  ```
  kubectl apply -f my-virtual-service.yaml -n <namespace>
  ```
  {: pre}

5. Procurez-vous l'adresse **EXTERNAL-IP** pour l'équilibreur de charge `istio-ingressgateway`.
  ```
  kubectl get svc -n istio-system
  ```
  {: pre}

  Dans l'exemple suivant, la valeur de **EXTERNAL-IP** est `168.1.1.1`.
  ```
  NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
  ...
  istio-ingressgateway     LoadBalancer   172.21.XXX.XXX   169.1.1.1       80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                            8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP  22m
  ```
  {: screen}

6. Enregistrez l'adresse IP de l'équilibreur de charge `istio-ingressgateway` en créant un nom d'hôte DNS.
  ```
  ibmcloud ks nlb-dns-create --cluster <cluster_name_or_id> --ip <LB_IP>
  ```
  {: pre}

7. Vérifiez que le nom d'hôte est créé.
  ```
  ibmcloud ks nlb-dnss --cluster <cluster_name_or_id>
  ```
  {: pre}

  Exemple de sortie :
  ```
  Hostname                                                                                IP(s)              Health Monitor   SSL Cert Status           SSL Cert Secret Name
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     ["168.1.1.1"]      None             created                   <certificate>
  ```
  {: screen}

7. Dans un navigateur Web, vérifiez que ce trafic est acheminé vers vos microservices gérés par Istio en entrant l'URL du microservice d'application auquel accéder.
  ```
  http://<host_name>/<service_path>
  ```
  {: codeblock}

En résumé, vous avez créé une passerelle nommée `my-gateway`. Cette passerelle utilise le service d'équilibreur de charge `istio-ingressgateway` existant pour exposer votre application. L'équilibreur de charge `istio-ingressgateway` utilise les règles que vous avez définies dans le service virtuel `my-virtual-service` pour router le trafic vers votre application. Enfin, vous avez créé un nom d'hôte pour l'équilibreur de charge `istio-ingressgateway`. Toutes les demandes utilisateur adressées au nom d'hôte sont transmises à votre application selon vos règles de routage Istio. Pour plus d'informations sur l'enregistrement de noms d'hôte DNS dans {{site.data.keyword.containerlong_notm}}, y compris les informations sur la configuration des diagnostics d'intégrité personnalisés pour les noms d'hôte, voir [Enregistrement d'un nom d'hôte NLB](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname).

Vous avez besoin d'un contrôle plus fin sur le routage ? Pour créer des règles appliquées après le routage du trafic vers chaque microservice par l'équilibreur de charge, par exemple les règles d'envoi du trafic à différentes versions d'un microservice, vous pouvez créer et appliquer des [règles de destination (`DestinationRules`) ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/).
{: tip}

<br />


## Mise à jour d'Istio sur {{site.data.keyword.containerlong_notm}}
{: #istio_update}

La version Istio incluse dans le module complémentaire Istio géré est testée par {{site.data.keyword.Bluemix_notm}} et approuvée en vue de son utilisation dans {{site.data.keyword.containerlong_notm}}. Pour mettre à jour vos composants Istio vers la version la plus récente d'Istio prise en charge par {{site.data.keyword.containerlong_notm}}, voir la rubrique [Mise à jour de modules complémentaires gérés](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons).
{: shortdesc}

## Désinstallation d'Istio sur {{site.data.keyword.containerlong_notm}}
{: #istio_uninstall}

Si vous n'utilisez plus Istio, vous pouvez nettoyer les ressources Istio dans votre cluster en désinstallant les modules complémentaires Istio.
{:shortdesc}

Le module complémentaire `istio` est une dépendance pour les modules `istio-extras`, `istio-sample-bookinfo` et [`knative`](/docs/containers?topic=containers-serverless-apps-knative). Le module complémentaire `istio-extras` est une dépendance pour le module `istio-sample-bookinfo`.
{: important}

**Facultatif** : les ressources que vous avez créées ou modifiées dans l'espace de nom `istio-system` et toutes les ressources Kubernetes qui ont été générées automatiquement par des définitions de ressource personnalisées (CRD) sont retirées. Si vous souhaitez conserver ces ressources, sauvegardez-les avant de désinstaller les modules complémentaires `istio`.
1. Sauvegardez les ressources, par exemple des fichiers de configuration de services ou d'applications, que vous avez créées ou modifiées dans l'espace de nom `istio-system`.
   Exemple de commande :
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

2. Sauvegardez dans un fichier YAML sur votre machine locale les ressources Kubernetes qui ont été générées automatiquement par des CRD dans `istio-system`.
   1. Procurez-vous les CRD dans `istio-system`.
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. Sauvegardez les ressources créées à partir de ces CRD.

### Désinstallation des modules complémentaires Istio gérés dans l'interface de ligne de commande
{: #istio_uninstall_cli}

1. Désactivez le module complémentaire `istio-sample-bookinfo`.
  ```
  ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. Désactivez le module complémentaire `istio-extras`.
  ```
  ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
  ```
  {: pre}

3. Désactivez le module complémentaire `istio`.
  ```
  ibmcloud ks cluster-addon-disable istio --cluster <cluster_name_or_ID> -f
  ```
  {: pre}

4. Vérifiez que tous les modules complémentaires Istio sont désactivés dans ce cluster. Aucun module Istio n'est renvoyé dans la sortie.
  ```
  ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
  ```
  {: pre}

### Désinstallation des modules complémentaires Istio gérés dans l'interface utilisateur
{: #istio_uninstall_ui}

1. Dans le [tableau de bord de votre cluster ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/clusters), cliquez sur le nom d'un cluster.

2. Cliquez sur l'onglet **Modules complémentaires**.

3. Sur la carte Istio, cliquez sur l'icône de menu.

4. Désinstallez des modules complémentaires Istio individuels ou tous les modules complémentaires Istio.
  - Modules complémentaires Istio individuels :
    1. Cliquez sur **Manage**.
    2. Désélectionnez les cases à cocher correspondant aux modules complémentaires que vous souhaitez désactiver. Si vous désélectionnez un module, les autres modules nécessitant ce module comme dépendance peuvent être automatiquement désélectionnés.
    3. Cliquez sur **Manage**. Les modules complémentaires Istio sont désactivés et les ressources de ces modules sont supprimées de ce cluster.
  - Tous les modules complémentaires Istio :
    1. Cliquez sur **Désinstaller**. Tous les modules complémentaires Istio gérés sont désactivés dans ce cluster et toutes les ressources Istio dans ce cluster sont supprimées.

5. Sur la carte Istio, vérifiez que les modules complémentaires que vous avez désinstallés ne sont plus répertoriés.

<br />


### Désinstallation d'autres installations Istio dans votre cluster
{: #istio_uninstall_other}

Si vous aviez déjà installé Istio dans le cluster en utilisant la charte Helm IBM ou une autre méthode, nettoyez cette installation Istio avant d'activer les modules complémentaires Istio gérés dans le cluster. Pour vérifier si Istio figure déjà dans un cluster, exécutez la commande `kubectl get namespaces` et recherchez l'espace de nom `istio-system` dans la sortie.
{: shortdesc}

- Si vous avez installé Istio en utilisant la Charte Helm Istio d'{{site.data.keyword.Bluemix_notm}} :
  1. Désinstallez le déploiement Helm Istio.
    ```
    helm del istio --purge
    ```
    {: pre}

  2. Si vous avez utilisé Helm 2.9 ou version antérieure, supprimez la ressource job supplémentaire.
    ```
    kubectl -n istio-system delete job --all
    ```
    {: pre}

- Si vous avez installé Istio manuellement ou via la charte Helm de la communauté Istio, voir la [documentation sur la désinstallation d'Istio  ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/setup/kubernetes/quick-start/#uninstall-istio-core-components).
* Si vous avez déjà installé BookInfo dans le cluster, nettoyez les ressources correspondantes.
  1. Changez de répertoire pour accéder à l'emplacement du fichier Istio.
    ```
    cd <filepath>/istio-1.1.5
    ```
    {: pre}

  2. Supprimez tous les services, les pods et les déploiements BookInfo dans le cluster.
    ```
    samples/bookinfo/platform/kube/cleanup.sh
    ```
    {: pre}

<br />


## Etape suivante ?
{: #istio_next}

* Si vous désirez explorer davantage Istio, d'autres guides figurent dans la [Documentation Istio ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/).
* Suivez le cours [Cognitive Class: Getting started with Microservices with Istio and IBM Cloud Kubernetes Service ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cognitiveclass.ai/courses/get-started-with-microservices-istio-and-ibm-cloud-container-service/). **Remarque** : vous pouvez ignorer la section sur l'installation d'Istio de ce cours.
* Consultez cet article de bloque sur l'utilisation d'[Istio ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://itnext.io/vistio-visualize-your-istio-mesh-using-netflixs-vizceral-b075c402e18e) pour visualiser votre maillage de services Istio.
