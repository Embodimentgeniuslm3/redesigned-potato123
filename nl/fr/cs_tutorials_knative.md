---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

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


# Tutoriel : Utilisation du module complémentaire géré Knative pour exécuter des applications sans serveur dans les clusters Kubernetes
{: #knative_tutorial}

Avec ce tutoriel, vous allez découvrir comment installer Knative sur un cluster Kubernetes dans {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

**Qu'est-ce que Knative et pourquoi dois-je l'utiliser ?**</br>
[Knative](https://github.com/knative/docs) est une plateforme open source développée par IBM, Google, Pivotal, Red Hat, Cisco et d'autres firmes avec pour objectif l'extension des capacités de Kubernetes pour vous aider à créer des applications modernes, conteneurisées, axées sur la source et sans serveur, en plus de votre cluster Kubernetes. Cette plateforme est conçue pour répondre aux besoins des développeurs qui doivent aujourd'hui décider du type d'application qu'ils souhaitent exécuter dans le cloud : applications à 12 facteurs, conteneurs ou fonctions. Chaque type d'application nécessite une solution propriétaire ou open source conçue sur mesure pour ces applications : Cloud Foundry pour les applications à 12 facteurs, Kubernetes pour les conteneurs, et OpenWhisk ou d'autres solutions pour les fonctions. Auparavant, les développeurs devaient déterminer l'approche qu'ils voulaient suivre, ce qui n'était pas très souple et pouvait s'avérer complexe lorsque différents types d'application devaient être combinés.  

Knative utilise une approche cohérente couvrant plusieurs infrastructures et langages de programmation pour permettre l'abstraction de la charge opérationnelle de construction, déploiement et gestion des charges de travail dans Kubernetes pour que les développeurs puissent se concentrer sur ce qui les intéresse le plus : le code source. Vous pouvez utiliser des packs de construction éprouvés que vous connaissez déjà, comme Cloud Foundry, Kaniko, Dockerfile, Bazel, etc. En s'intégrant à Istio, Knative garantit que vos charges de travail conteneurisées et sans serveur peuvent être facilement exposées sur Internet, surveillées et contrôlées et que vos données sont chiffrées lors de leur transfert.

**Comment fonctionne Knative ?**</br>
Knative est constitué de 3 composants clés, ou _primitives_, qui vous aident à construire, déployer et gérer vos applications sans serveur dans votre cluster Kubernetes :

- **Build :** la primitive `Build` prend en charge la création d'une série d'étapes pour construire votre application à partir d'un code source dans une image de conteneur. Imaginez l'utilisation d'un simple modèle de construction dans lequel vous spécifiez le référentiel de la source pour rechercher le code de votre application et le registre de conteneur dans lequel vous souhaitez héberger l'image. A l'aide d'une commande unique, vous pouvez demander à Knative d'utiliser ce modèle de construction, d'extraire le code source, de créer l'image et de l'insérer dans votre registre de conteneur pour que vous puissiez utiliser l'image dans votre conteneur.
- **Serving :** la primitive `Serving` vous aide à déployer des applications sans serveur sous forme de services Knative et vous permet d'effectuer leur mise à l'échelle automatiquement, même avec zéro instance. En utilisant les capacités de gestion de trafic et de routage intelligent d'Istio, vous pouvez contrôler le trafic acheminé vers une version spécifique de votre service, ce qui facilite le travail du développeur pour tester et déployer une nouvelle version de l'application ou pour effectuer des tests A/B.
- **Eventing :** avec la primitive `Eventing`, vous pouvez créer des déclencheurs ou des flux d'événements auxquels peuvent s'abonner d'autres services. Par exemple, vous envisagerez peut-être de lancer une nouvelle version de votre application chaque fois qu'un code est inséré dans votre référentiel maître GitHub. Ou vous souhaitez exécuter une application sans serveur uniquement lorsque la température descend en dessous de zéro. La primitive `Eventing` peut être intégrée dans votre pipeline CI/CD pour automatiser la génération et le déploiement d'applications lorsqu'un événement spécifique se produit.

**Qu'est-ce que le module complémentaire Managed Knative on {{site.data.keyword.containerlong_notm}} (version expérimentale) ?** </br>
Managed Knative on {{site.data.keyword.containerlong_notm}} est un module complémentaire géré qui intègre Knative et Istio directement avec votre cluster Kubernetes. Les versions de Knative et d'Istio dans le module complémentaire sont testées par IBM et prises en charge pour être utilisées dans {{site.data.keyword.containerlong_notm}}. Pour plus d'informations sur les modules complémentaires gérés, voir [Ajout de services à l'aide de modules complémentaires gérés](/docs/containers?topic=containers-managed-addons#managed-addons).

**Y a-t-il des limitations ?** </br>
Si vous avez installé le [contrôleur d'admission Container Image Security Enforcer](/docs/services/Registry?topic=registry-security_enforce#security_enforce) dans votre cluster, vous ne pouvez pas activer le module complémentaire géré Knative dans votre cluster.

Ça vous tente ? Suivez ce tutoriel pour expérimenter Knative dans {{site.data.keyword.containerlong_notm}}.

## Objectifs
{: #knative_objectives}

- Familiarisez-vous avec les concepts de base et les primitives de Knative.  
- Installez les modules complémentaires gérés Knative et Istio dans votre cluster.
- Déployez votre première application sans serveur avec Knative et exposez-la sur Internet à l'aide de la primitive `Serving`.
- Découvrez la mise à l'échelle et les fonctions de révision de Knative.

## Durée
{: #knative_time}

30 minutes

## Public
{: #knative_audience}

Ce tutoriel est destiné aux développeurs qui souhaitent apprendre comment utiliser Knative pour déployer une application sans serveur dans un cluster Kubernetes, et aux administrateurs de cluster qui souhaitent apprendre à configurer Knative dans un cluster.

## Prérequis
{: #knative_prerequisites}

-  [Installez l'interface de ligne de commande d'IBM Cloud, le plug-in {{site.data.keyword.containerlong_notm}} et l'interface de ligne de commande de Kubernetes](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps). Veillez à installer la version de l'interface CLI `kubectl` correspondant à la version Kubernetes de votre cluster.
-  [Créez un cluster avec au moins trois noeuds worker ayant chacun 4 coeurs et 16 Go de mémoire (`b3c.4x16`) ou plus](/docs/containers?topic=containers-clusters#clusters_cli). Chaque noeud worker doit exécuter Kubernetes version 1.12 ou plus. 
-  Vérifiez que vous disposez du [rôle de service **Writer** ou **Manager** d'{{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) pour {{site.data.keyword.containerlong_notm}}.
-  [Ciblez l'interface CLI sur votre cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

## Leçon 1 : Configuration du module complémentaire géré Knative
{: #knative_setup}

Knative s'appuie sur Istio pour garantir que vos charges de travail sans serveur et conteneurisées peuvent être exposées au sein du cluster et sur Internet. Avec Istio, vous pouvez également surveiller et contrôler le trafic réseau entre vos services et garantir que vos données sont chiffrées lors de leur transfert. Lorsque vous installez un module complémentaire géré Knative, le module complémentaire géré Istio est automatiquement installé dans la foulée.
{: shortdesc}

1. Activez le module complémentaire géré Knative dans votre cluster. Lorsque vous activez Knative dans votre cluster, Istio et tous les composants Knative sont installés dans votre cluster.
   ```
   ibmcloud ks cluster-addon-enable knative --cluster <cluster_name_or_ID> -y
   ```
   {: pre}

   Exemple de sortie :
   ```
   Enabling add-on knative for cluster knative...
   OK
   ```
   {: screen}

   L'installation de tous les composants Knative peut prendre quelques minutes.

2. Vérifiez que l'installation d'Istio a abouti. Tous les pods des 9 services Istio et le pod de Prometheus doivent avoir le statut `Running`.
   ```
   kubectl get pods --namespace istio-system
   ```
   {: pre}

   Exemple de sortie :
   ```
   NAME                                       READY     STATUS      RESTARTS   AGE
    istio-citadel-748d656b-pj9bw               1/1       Running     0          2m
    istio-egressgateway-6c65d7c98d-l54kg       1/1       Running     0          2m
    istio-galley-65cfbc6fd7-bpnqx              1/1       Running     0          2m
    istio-ingressgateway-f8dd85989-6w6nj       1/1       Running     0          2m
    istio-pilot-5fd885964b-l4df6               2/2       Running     0          2m
    istio-policy-56f4f4cbbd-2z2bk              2/2       Running     0          2m
    istio-sidecar-injector-646655c8cd-rwvsx    1/1       Running     0          2m
    istio-statsd-prom-bridge-7fdbbf769-8k42l   1/1       Running     0          2m
    istio-telemetry-8687d9d745-mwjbf           2/2       Running     0          2m
    prometheus-55c7c698d6-f4drj                1/1       Running     0          2m
   ```
   {: screen}

3. Facultatif : si vous souhaitez utiliser Istio pour toutes les applications dans l'espace de nom `default`, ajoutez le libellé `istio-injection=enabled` à l'espace de nom. Chaque pod d'application sans serveur doit exécuter un proxy sidecar Envoy de sorte que les applications puissent être incluses dans le maillage de services Istio. Ce libellé permet à Istio de modifier automatiquement la spécification de modèle de pod dans de nouveaux déploiements d'application de sorte que les pods soient créés avec des conteneurs de proxy sidecar Envoy. 
  ```
  kubectl label namespace default istio-injection=enabled
  ```
  {: pre}

4. Vérifiez que tous les composants Knative sont installés correctement.
   1. Vérifiez que tous les pods du composant `Serving` de Knative sont à l'état `Running`.  
      ```
      kubectl get pods --namespace knative-serving
      ```
      {: pre}

      Exemple de sortie :
      ```
      NAME                          READY     STATUS    RESTARTS   AGE
      activator-598b4b7787-ps7mj    2/2       Running   0          21m
      autoscaler-5cf5cfb4dc-mcc2l   2/2       Running   0          21m
      controller-7fc84c6584-qlzk2   1/1       Running   0          21m
      webhook-7797ffb6bf-wg46v      1/1       Running   0          21m
      ```
      {: screen}

   2. Vérifiez que tous les pods du composant `Build` de Knative sont à l'état `Running`.  
      ```
      kubectl get pods --namespace knative-build
      ```
      {: pre}

      Exemple de sortie :
      ```
      NAME                                READY     STATUS    RESTARTS   AGE
      build-controller-79cb969d89-kdn2b   1/1       Running   0          21m
      build-webhook-58d685fc58-twwc4      1/1       Running   0          21m
      ```
      {: screen}

   3. Vérifiez que tous les pods du composant `Eventing` de Knative sont à l'état `Running`.
      ```
      kubectl get pods --namespace knative-eventing
      ```
      {: pre}

      Exemple de sortie :

      ```
      NAME                                            READY     STATUS    RESTARTS   AGE
      eventing-controller-847d8cf969-kxjtm            1/1       Running   0          22m
      in-memory-channel-controller-59dd7cfb5b-846mn   1/1       Running   0          22m
      in-memory-channel-dispatcher-67f7497fc-dgbrb    2/2       Running   1          22m
      webhook-7cfff8d86d-vjnqq                        1/1       Running   0          22m
      ```
      {: screen}

   4. Vérifiez que tous les pods du composant `Sources` de Knative sont à l'état `Running`.
      ```
      kubectl get pods --namespace knative-sources
      ```
      {: pre}

      Exemple de sortie :
      ```
      NAME                   READY     STATUS    RESTARTS   AGE
      controller-manager-0   1/1       Running   0          22m
      ```
      {: screen}

   5. Vérifiez que tous les pods du composant `Monitoring` de Knative sont à l'état `Running`.
      ```
      kubectl get pods --namespace knative-monitoring
      ```
      {: pre}

      Exemple de sortie :
      ```
      NAME                                  READY     STATUS                 RESTARTS   AGE
      elasticsearch-logging-0               1/1       Running                0          22m
      elasticsearch-logging-1               1/1       Running                0          21m
      grafana-79cf95cc7-mw42v               1/1       Running                0          21m
      kibana-logging-7f7b9698bc-m7v6r       1/1       Running                0          22m
      kube-state-metrics-768dfff9c5-fmkkr   4/4       Running                0          21m
      node-exporter-dzlp9                   2/2       Running                0          22m
      node-exporter-hp6gv                   2/2       Running                0          22m
      node-exporter-hr6vs                   2/2       Running                0          22m
      prometheus-system-0                   1/1       Running                0          21m
      prometheus-system-1                   1/1       Running                0          21m
      ```
      {: screen}

Parfait ! Avec Knative et Istio configurés, vous pouvez désormais déployer votre première application sans serveur dans votre cluster.

## Leçon 2 : Déploiement d'une application sans serveur dans votre cluster
{: #deploy_app}

Dans cette leçon, vous allez déployer votre première [application `Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) sans serveur dans Go. Lorsque vous envoyez une demande à votre modèle d'application, l'application lit la variable d'environnement `TARGET` et imprime `"Hello ${TARGET}!"`. Si cette variable d'environnement est vide, `"Hello World!"` est renvoyé.
{: shortdesc}

1. Créez un fichier YAML pour votre première application `Hello World` sans serveur dans Knative. Pour déployer une application avec Knative, vous devez spécifier une ressource de service Knative. Un service est géré par la primitive `Serving` de Knative et chargé de gérer le cycle de vie complet de la charge de travail. Le service garantit que chaque déploiement comporte une révision Knative, une route et une configuration. Lorsque vous mettez à jour le service, une nouvelle version de l'application est créée et ajoutée dans l'historique de révision du service. Les routes Knative garantissent que chaque révision de l'application est mappée à un noeud final de réseau pour que vous puissiez contrôler la quantité de trafic acheminé vers une révision spécifique. Les configurations Knative renferment les paramètres d'une révision spécifique pour que vous puissiez toujours revenir à une révision antérieur ou passer d'une révision à une autre. Pour plus d'informations sur les ressources Knative `Serving`, voir la [documentation Knative](https://github.com/knative/docs/tree/master/serving).
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Go Sample v1"
    ```
    {: codeblock}

    <table>
    <caption>Description des composants du fichier YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>Nom de votre service Knative.</td>
    </tr>
    <tr>
    <td><code>metadata.namespace</td>
    <td>Espace de nom Kubernetes dans lequel vous voulez déployer votre application sous forme de service Knative. </td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>URl d'accès au registre de conteneur dans lequel est stockée votre image. Dans cet exemple, vous déployez l'application Knative Hello World qui est stockée dans l'espace de nom <code>ibmcom</code> dans Docker Hub. </td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>Liste de variables d'environnement que vous voulez que votre service Knative possède. Dans cet exemple, la variable d'environnement <code>TARGET</code> est lue par le modèle d'application et renvoyée lorsque vous envoyez une demande à votre application au format <code>"Hello ${TARGET}!"</code>. Si aucune valeur n'est fournie, le modèle d'application renvoie <code>"Hello World!"</code>.  </td>
    </tr>
    </tbody>
    </table>

2. Créez le service Knative dans votre cluster. Lorsque vous créez ce service, la primitive Knative `Serving` crée une révision non modifiable, une route Knative, une règle de routage Ingress, un service Kubernetes, un pod Kubernetes et un équilibreur de charge pour votre application. Un sous-domaine est affecté à votre application à partir de votre sous-domaine Ingress, au format `<knative_service_name>.<namespace>.<ingress_subdomain>`. Vous pouvez utiliser ce sous-domaine pour accéder à l'application depuis Internet. 
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   Exemple de sortie :
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. Vérifiez que votre pod a bien été créé. Votre pod est constitué de deux conteneurs. Un conteneur exécute votre application `Hello World` et l'autre est un composant sidecar qui exécute les outils de surveillance et de consignation d'Istio et de Knative. Le numéro de révision `00001` est attribué à votre pod.
   ```
   kubectl get pods
   ```
   {: pre}

   Exemple de sortie :
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00001-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
   ```
   {: screen}

4. Testez votre application `Hello World`.
   1. Obtenez le domaine par défaut affecté à votre service Knative. Si vous avez modifié le nom de votre service Knative, ou déployé l'application dans un espace de nom différent, mettez à jour ces valeurs dans votre requête.
      ```
      kubectl get ksvc/kn-helloworld
      ```
      {: pre}

      Exemple de sortie :
      ```
      NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
      kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-rjmwt   kn-helloworld-rjmwt   True
      ```
      {: screen}

   2. Effectuez une demande à votre application en utilisant le sous-domaine que vous avez récupéré à l'étape précédente.
      ```
      curl -v <service_domain>
      ```
      {: pre}

      Exemple de sortie :
      ```
      * Rebuilt URL to: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud/
      *   Trying 169.46.XX.XX...
      * TCP_NODELAY set
      * Connected to kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud (169.46.XX.XX) port 80 (#0)
      > GET / HTTP/1.1
      > Host: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud
      > User-Agent: curl/7.54.0
      > Accept: */*
      >
      < HTTP/1.1 200 OK
      < Date: Thu, 21 Mar 2019 01:12:48 GMT
      < Content-Type: text/plain; charset=utf-8
      < Content-Length: 20
      < Connection: keep-alive
      < x-envoy-upstream-service-time: 17
      <
      Hello Go Sample v1!
      * Connection #0 to host kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud left intact
      ```
      {: screen}

5. Patientez quelques minutes pour laisser Knative réduire votre pod. Knative évalue le nombre de pods qui doivent être opérationnels à moment donné pour traiter la charge de travail entrante. Si aucun trafic réseau n'est reçu, Knative réduit automatiquement vos pods, même jusqu'à zéro pod comme illustré dans cet exemple.

   Vous voulez voir comment Knative augmente vos pods ? Essayez d'augmenter la charge de travail de votre application, par exemple, en utilisant des outils comme [Simple Cloud-based Load Testing](https://loader.io/).
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   Si vous ne voyez pas de pod `kn-helloworld`, Knative réduit le nombre de pods de votre application à zéro.

6. Mettez à jour votre modèle de service Knative et saisissez une valeur différente pour la variable d'environnement `TARGET`.

   Exemple de fichier YAML de service :
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Mr. Smith"
    ```
    {: codeblock}

7. Appliquez les modifications à votre service. Lorsque vous modifiez la configuration, Knative crée automatiquement une nouvelle révision, affecte une nouvelle route et, par défaut, demande à Istio d'acheminer le trafic réseau entrant vers la révision la plus récente.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

8. Effectuez une nouvelle demande à votre application pour vérifier que vos modifications ont été appliquées.
   ```
   curl -v <service_domain>
   ```

   Exemple de sortie :
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

9. Vérifiez que Knative a augmenté à nouveau le nombre de pods pour tenir compte de l'augmentation du trafic réseau. Le numéro de révision `00002` est attribué à votre pod. Vous pouvez utiliser ce numéro de révision pour référencer une version spécifique de votre application, par exemple lorsque vous voulez demander à Istio de répartir le trafic entrant entre les deux révisions.
   ```
   kubectl get pods
   ```

   Exemple de sortie :
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00002-deployment-55db6bf4c5-2vftm   3/3       Running   0          16s
   ```
   {: screen}

10. Facultatif : nettoyez votre service Knative.
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}

Génial ! Vous venez de déployer votre première application Knative dans votre cluster et de découvrir les fonctions de révision et de mise à l'échelle de la primitive Knative `Serving`.


## Etape suivante ?   
{: #whats-next}

- Testez le [workshop Knative ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/IBM/knative101/tree/master/workshop) pour déployer votre première application fibonacci `Node.js` dans votre cluster.
  - Découvrez comment utiliser la primitive Knative `Build` pour générer une image à partir d'un fichier Dockerfile dans GitHub et insérer automatiquement l'image dans votre espace de nom dans {{site.data.keyword.registrylong_notm}}.  
  - Apprenez à configurer le routage du trafic réseau du sous-domaine Ingress fourni par IBM vers la passerelle Ingress d'Istio fournie par Knative.
  - Déployez une nouvelle version de votre application et utilisez Istio pour contrôler la quantité de trafic acheminé vers chaque version de l'application.
- Explorez des exemples de [Knative `Eventing` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/knative/docs/tree/master/eventing/samples).
- Pour plus d'informations sur Knative, consultez la [documentation Knative ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/knative/docs).
