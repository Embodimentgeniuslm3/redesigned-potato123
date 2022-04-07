---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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


# Déploiement d'applications sans serveur avec Knative
{: #serverless-apps-knative}

Apprenez à installer et utiliser Knative dans un cluster Kubernetes dans {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

**Qu'est-ce que Knative et pourquoi dois-je l'utiliser ?**</br>
[Knative](https://github.com/knative/docs) est une plateforme open source développée par IBM, Google, Pivotal, Red Hat, Cisco et d'autres firmes. L'objectif est d'étendre les capacités de Kubernetes pour vous aider à créer des applications modernes, conteneurisées, axées sur la source et sans serveur, en plus de votre cluster Kubernetes. Cette plateforme est conçue pour répondre aux besoins des développeurs qui doivent aujourd'hui décider du type d'application qu'ils souhaitent exécuter dans le cloud : applications à 12 facteurs, conteneurs ou fonctions. Chaque type d'application nécessite une solution propriétaire ou open source conçue sur mesure pour ces applications : Cloud Foundry pour les applications à 12 facteurs, Kubernetes pour les conteneurs, et OpenWhisk ou d'autres solutions pour les fonctions. Auparavant, les développeurs devaient déterminer l'approche qu'ils voulaient suivre, ce qui n'était pas très souple et pouvait s'avérer complexe lorsque différents types d'application devaient être combinés.  

Knative utilise une approche cohérente couvrant plusieurs infrastructures et langages de programmation pour permettre l'abstraction de la charge opérationnelle de construction, déploiement et gestion des charges de travail dans Kubernetes pour que les développeurs puissent se concentrer sur ce qui les intéresse le plus : le code source. Vous pouvez utiliser des packs de construction éprouvés que vous connaissez déjà, comme Cloud Foundry, Kaniko, Dockerfile, Bazel, etc. En s'intégrant à Istio, Knative garantit que vos charges de travail conteneurisées et sans serveur peuvent être facilement exposées sur Internet, surveillées et contrôlées et que vos données sont chiffrées lors de leur transfert.

**Comment fonctionne Knative ?**</br>
Knative est constitué de trois composants clés, ou _primitives_, qui vous aident à construire, déployer et gérer vos applications sans serveur dans votre cluster Kubernetes :

- **Build :** la primitive `Build` prend en charge la création d'une série d'étapes pour construire votre application à partir d'un code source dans une image de conteneur. Imaginez que vous utilisez un simple modèle de construction dans lequel vous spécifiez le référentiel de la source pour rechercher le code de votre application et le registre de conteneur dans lequel vous souhaitez héberger l'image. A l'aide d'une commande unique, vous pouvez demander à Knative d'utiliser ce modèle de construction, d'extraire le code source, de créer l'image et de l'insérer dans votre registre de conteneur pour que vous puissiez utiliser l'image dans votre conteneur.
- **Serving :** la primitive `Serving` vous aide à déployer des applications sans serveur sous forme de services Knative et vous permet d'effectuer leur mise à l'échelle automatiquement, même avec zéro instance. Pour exposer vos charges de travail sans serveur et conteneurisées, Knative utilise Istio. Lorsque vous installez un module complémentaire géré Knative, le module complémentaire géré Istio est automatiquement installé dans la foulée. En utilisant les capacités de gestion de trafic et de routage intelligent d'Istio, vous pouvez contrôler le trafic acheminé vers une version spécifique de votre service, ce qui facilite le travail du développeur pour tester et déployer une nouvelle version de l'application ou pour effectuer des tests A/B.
- **Eventing :** avec la primitive `Eventing`, vous pouvez créer des déclencheurs ou des flux d'événements auxquels peuvent s'abonner d'autres services. Par exemple, vous envisagerez peut-être de lancer une nouvelle version de votre application chaque fois qu'un code est inséré dans votre référentiel maître GitHub. Ou vous souhaitez exécuter une application sans serveur uniquement lorsque la température descend en dessous de zéro. Par exemple, la primitive `Eventing` peut être intégrée dans votre pipeline CI/CD pour automatiser la génération et le déploiement d'applications lorsqu'un événement spécifique se produit.

**Qu'est-ce que le module complémentaire Managed Knative on {{site.data.keyword.containerlong_notm}} (version expérimentale) ?** </br>
Managed Knative on {{site.data.keyword.containerlong_notm}} est un [module complémentaire géré](/docs/containers?topic=containers-managed-addons#managed-addons) qui intègre Knative et Istio directement avec votre cluster Kubernetes. Les versions de Knative et d'Istio dans le module complémentaire sont testées par IBM et prises en charge pour être utilisées dans {{site.data.keyword.containerlong_notm}}. Pour plus d'informations sur les modules complémentaires gérés, voir [Ajout de services à l'aide de modules complémentaires gérés](/docs/containers?topic=containers-managed-addons#managed-addons).

**Y a-t-il des limitations ?** </br>
Si vous avez installé le [contrôleur d'admission Container Image Security Enforcer](/docs/services/Registry?topic=registry-security_enforce#security_enforce) dans votre cluster, vous ne pouvez pas activer le module complémentaire géré Knative dans votre cluster.

## Configuration de Knative dans votre cluster
{: #knative-setup}

Knative s'appuie sur Istio pour garantir que vos charges de travail sans serveur et conteneurisées peuvent être exposées au sein du cluster et sur Internet. Avec Istio, vous pouvez également surveiller et contrôler le trafic réseau entre vos services et garantir que vos données sont chiffrées lors de leur transfert. Lorsque vous installez un module complémentaire géré Knative, le module complémentaire géré Istio est automatiquement installé dans la foulée.
{: shortdesc}

Avant de commencer :
-  [Installez l'interface de ligne de commande d'IBM Cloud, le plug-in {{site.data.keyword.containerlong_notm}} et l'interface de ligne de commande de Kubernetes](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps). Veillez à installer la version de l'interface CLI `kubectl` correspondant à la version Kubernetes de votre cluster.
-  [Créez un cluster standard avec au moins trois noeuds worker ayant chacun 4 coeurs et 16 Go de mémoire (`b3c.4x16`) ou plus](/docs/containers?topic=containers-clusters#clusters_ui). En outre, le cluster et les noeuds worker doivent au moins exécuter la version minimale prise en charge de Kubernetes, que vous pouvez consulter en exécutant la commande `ibmcloud ks addon-versions --addon knative`.
-  Vérifiez que vous disposez du [rôle de service **Writer** ou **Manager** d'{{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) pour {{site.data.keyword.containerlong_notm}}.
-  [Ciblez l'interface CLI sur votre cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
</br>

Pour installer Knative dans votre cluster :

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

2. Vérifiez que l'installation d'Istio a abouti. Tous les pods des neuf services Istio et le pod de Prometheus doivent avoir le statut `Running`.
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

4. Vérifiez que tous les pods du composant `Serving` de Knative sont à l'état `Running`.
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

## Utilisation des services Knative pour déployer une application sans serveur
{: #knative-deploy-app}

Une fois que vous avez configuré Knative dans votre cluster, vous pouvez déployer vos applications sans serveur en tant que service Knative.
{: shortdesc}

**Qu'est-ce qu'un service Knative ?** </br>
Pour déployer une application avec Knative, vous devez spécifier une ressource `Service` Knative. Un service Knative est géré par la primitive `Serving` de Knative et est chargé de gérer le cycle de vie complet de la charge de travail. Lorsque vous créez le service, la primitive `Serving` de Knative crée automatiquement une version pour votre application sans serveur et ajoute cette version à l'historique de révision du service. Une URL publique est affectée à votre application sans serveur à partir de votre sous-domaine Ingress, au format `<knative_service_name>.<namespace>.<ingress_subdomain>` Vous pouvez utiliser ce sous-domaine pour accéder à l'application depuis Internet. En outre, un nom d'hôte privé est affecté à votre application au format `<knative_service_name>.<namespace>.cluster.local` et vous pouvez l'utiliser pour accéder à votre application depuis le cluster. 

**Que se passe-t-il en coulisse lorsque je crée le service Knative ?**</br>
Lorsque vous créez un service Knative, votre application est automatiquement déployée en tant que pod Kubernetes dans votre cluster et exposée à l'aide d'un service Kubernetes. Pour affecter le nom d'hôte public, Knative utilise le sous-domaine Ingress et le certificat TLS fournis par IBM. Le trafic réseau entrant est routé en fonction des règles de routage Ingress fournies par IBM. 

**Comment puis-je déployer une nouvelle version de mon application ?**</br>
Lorsque vous mettez à jour votre service Knative, une nouvelle version de votre application sans serveur est créée. Les mêmes noms d'hôte privé et public que votre version précédente sont affectés à cette version. Par défaut, l'ensemble du trafic réseau entrant est routé vers la version la plus récente de votre application. Toutefois, vous pouvez également spécifier le pourcentage de trafic réseau entrant que vous souhaitez router vers une version d'application spécifique afin de pouvoir effectuer des tests A/B. Vous pouvez répartir le trafic réseau entrant sur deux versions d'application, la version en cours de votre application et la nouvelle version vers laquelle vous souhaitez effectuer le déploiement.   

**Puis-je utiliser mon propre domaine personnalisé et mon propre certificat TLS ?** </br>
Vous pouvez modifier l'objet ConfigMap de votre passerelle Ingress Istio et les règles de routage Ingress afin d'utiliser votre nom de domaine et votre certificat TLS personnalisés lorsque vous affectez un nom d'hôte à votre application sans serveur. Pour plus d'informations, voir [Configuration de noms de domaine et de certificats personnalisés](#knative-custom-domain-tls).

Pour déployer votre application sans serveur en tant que service Knative :

1. Créez un fichier YAML pour votre première application [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) sans serveur avec Knative. Lorsque vous envoyez une demande à votre modèle d'application, l'application lit la variable d'environnement `TARGET` et imprime `"Hello ${TARGET}!"`. Si cette variable d'environnement est vide, `"Hello World!"` est renvoyé.
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
    <td>Facultatif : Espace de nom Kubernetes dans lequel vous voulez déployer votre application en tant que service Knative. Par défaut, tous les services sont déployés sur l'espace de nom Kubernetes <code>default</code>. </td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>URL d'accès au registre de conteneur dans lequel est stockée votre image. Dans cet exemple, vous déployez l'application Knative Hello World qui est stockée dans l'espace de nom <code>ibmcom</code> dans Docker Hub. </td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>Facultatif : Liste de variables d'environnement que votre service Knative doit posséder. Dans cet exemple, la variable d'environnement <code>TARGET</code> est lue par le modèle d'application et renvoyée lorsque vous envoyez une demande à votre application au format <code>"Hello ${TARGET}!"</code>. Si aucune valeur n'est fournie, le modèle d'application renvoie <code>"Hello World!"</code>.  </td>
    </tr>
    </tbody>
    </table>

2. Créez le service Knative dans votre cluster.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   Exemple de sortie :
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. Vérifiez que votre service Knative a bien été créé. Dans votre sortie CLI, vous pouvez voir le domaine (**DOMAIN**) public qui est affecté à votre application sans serveur. Les colonnes **LATESTCREATED** et **LATESTREADY** affichent la version de votre application qui a été créée en dernier et qui est actuellement déployée au format `<knative_service_name>-<version>`. La version qui est affectée à votre application est une valeur de chaîne aléatoire. Dans cet exemple, la version de votre application sans serveur est `rjmwt`. Lorsque vous mettez à jour le service, une nouvelle version de votre application est créée et une nouvelle chaîne aléatoire lui est affectée.   
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

4. Essayez votre application `Hello World` en envoyant une demande à l'URL publique qui est affectée à votre application. 
   ```
   curl -v <public_app_url>
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

5. Répertoriez le nombre de pods qui ont été créés pour votre service Knative. Dans l'exemple de cette rubrique, un pod comprenant deux conteneurs est déployé. Un conteneur exécute votre application `Hello World` et l'autre est un composant sidecar qui exécute les outils de surveillance et de consignation d'Istio et de Knative.
   ```
   kubectl get pods
   ```
   {: pre}

   Exemple de sortie :
   ```
   NAME                                             READY     STATUS    RESTARTS   AGE
   kn-helloworld-rjmwt-deployment-55db6bf4c5-2vftm  2/2      Running   0          16s
   ```
   {: screen}

6. Patientez quelques minutes pour laisser Knative réduire votre pod. Knative évalue le nombre de pods qui doivent être opérationnels à moment donné pour traiter la charge de travail entrante. Si aucun trafic réseau n'est reçu, Knative réduit automatiquement vos pods, même jusqu'à zéro pod comme illustré dans cet exemple.

   Vous voulez voir comment Knative augmente vos pods ? Essayez d'augmenter la charge de travail de votre application, par exemple, en utilisant des outils comme [Simple Cloud-based Load Testing](https://loader.io/).
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   Si vous ne voyez pas de pod `kn-helloworld`, Knative réduit le nombre de pods de votre application à zéro.

7. Mettez à jour votre modèle de service Knative et saisissez une valeur différente pour la variable d'environnement `TARGET`.

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

8. Appliquez les modifications à votre service. Lorsque vous modifiez la configuration, Knative crée automatiquement une nouvelle version de votre application. 
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

9. Assurez-vous qu'une nouvelle version de votre application est déployée. Dans votre sortie CLI, vous pouvez voir la nouvelle version de votre application dans la colonne **LATESTCREATED**. Lorsque vous voyez la même version d'application dans la colonne **LATESTREADY**, la configuration de votre application est terminée et celle-ci est prête à recevoir le trafic réseau entrant sur l'URL public affectée. 
   ```
   kubectl get ksvc/kn-helloworld
   ```
   {: pre}

   Exemple de sortie :
   ```
   NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
   kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-ghyei   kn-helloworld-ghyei   True
   ```
   {: screen}

9. Effectuez une nouvelle demande à votre application pour vérifier que vos modifications ont été appliquées.
   ```
   curl -v <service_domain>
   ```

   Exemple de sortie :
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

10. Vérifiez que Knative a augmenté à nouveau le nombre de pods pour tenir compte de l'augmentation du trafic réseau.
    ```
    kubectl get pods
    ```

    Exemple de sortie :
    ```
    NAME                                              READY     STATUS    RESTARTS   AGE
    kn-helloworld-ghyei-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
    ```
    {: screen}

11. Facultatif : nettoyez votre service Knative.
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}


## Configuration des noms de domaine et des certificats personnalisés
{: #knative-custom-domain-tls}

Vous pouvez configurer Knative pour affecter des noms d'hôte à partir de votre propre domaine personnalisé que vous avez configuré avec TLS.
{: shortdesc}

Par défaut, un sous-domaine public est affecté à chaque application à partir de votre sous-domaine Ingress, au format `<knative_service_name>.<namespace>.<ingress_subdomain>`. Vous pouvez utiliser ce sous-domaine pour accéder à l'application depuis Internet. En outre, un nom d'hôte privé est affecté à votre application au format `<knative_service_name>.<namespace>.cluster.local` et vous pouvez l'utiliser pour accéder à votre application depuis le cluster. Si vous souhaitez affecter des noms d'hôte à partir d'un domaine personnalisé que vous possédez, vous pouvez modifier l'objet ConfigMap Knative pour utiliser le domaine personnalisé à la place. 

1. Créez un domaine personnalisé. Pour enregistrer votre domaine personnalisé, travaillez en collaboration avec votre fournisseur DNS (Domain Name Service) ou le [DNS IBM Cloud](/docs/infrastructure/dns?topic=dns-getting-started).
2. Configurez votre domaine pour acheminer le trafic réseau entrant vers la passerelle Ingress fournie par IBM. Sélectionnez l'une des options suivantes :
   - Définir un alias pour votre domaine personnalisé en spécifiant le domaine fourni par IBM sous forme d'enregistrement de nom canonique (CNAME). Pour identifier le domaine Ingress fourni par IBM, exécutez la commande `ibmcloud ks cluster-get --cluster <cluster_name>` et recherchez la zone **Sous-domaine Ingress**. L'utilisation de CNAME est privilégiée car IBM fournit des diagnostics d'intégrité automatiques sur le sous-domaine IBM et retire toutes les adresses IP défaillantes dans la réponse DNS.
   - Mappez votre domaine personnalisé à l'adresse IP privée portable de la passerelle Ingress en ajoutant l'adresse IP en tant qu'enregistrement. Pour trouver l'adresse IP publique de la passerelle Ingress, exécutez la commande `nslookup <ingress_subdomain>`.
3. Achetez un certificat TLS générique officiel pour votre domaine personnalisé. Si vous souhaitez acheter plusieurs certificats TLS, assurez-vous que le [nom usuel ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://support.dnsimple.com/articles/what-is-common-name/) est différent pour chaque certificat. 
4. Créez une valeur confidentielle Kubernetes pour votre certificat et votre clé.
   1. Codez le certificat et la clé en base 64 et sauvegardez la valeur codée en base 64 dans un nouveau fichier.
      ```
      openssl base64 -in tls.key -out tls.key.base64
      ```
      {: pre}

   2. Affichez la valeur codée en base 64 pour votre certificat et votre clé.
      ```
      cat tls.key.base64
      ```
      {: pre}

   3. Créez un fichier YAML de valeur confidentielle YAML en utilisant le certificat et la clé.
     ```
      apiVersion: v1
      kind: Secret
      metadata:
        name: mydomain-ssl
      type: Opaque
      data:
        tls.crt: <client_certificate>
        tls.key: <client_key>
      ```
      {: codeblock}

   4. Créez le certificat dans votre cluster.
    ```
      kubectl create -f secret.yaml
      ```
      {: pre}

5. Ouvrez la ressource Ingress `iks-knative-ingress` dans l'espace de nom `istio-system` de votre cluster pour commencer à l'éditer. 
   ```
   kubectl edit ingress iks-knative-ingress -n istio-system
   ```
   {: pre}

6. Modifiez les règles de routage par défaut pour votre Ingress.
   - Ajoutez votre domaine générique personnalisé à la section `spec.rules.host` de sorte que l'ensemble du trafic réseau entrant à partir de votre domaine personnalisé et de n'importe quel sous-domaine soit acheminé vers `istio-ingressgateway`.
   - Configurez tous les hôtes de votre domaine générique personnalisé pour qu'ils utilisent la valeur confidentielle TLS que vous avez créée précédemment dans la section `spec.tls.hosts`.

   Exemple Ingress :
   ```
   apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
     name: iks-knative-ingress
     namespace: istio-system
   spec:
     rules:
     - host: '*.mydomain'
       http:
         paths:
         - backend:
             serviceName: istio-ingressgateway
             servicePort: 80
           path: /
     tls:
     - hosts:
       - '*.mydomain'
       secretName: mydomain-ssl
   ```
   {: codeblock}

   Les sections `spec.rules.host` et `spec.tls.hosts` sont des listes et elles peuvent inclure plusieurs domaines et certificats TLS personnalisés.   {: tip}

7. Modifiez l'objet ConfigMap `config-domain` Knative pour utiliser votre domaine personnalisé afin d'affecter des noms d'hôte aux nouveaux services Knative. 
   1. Ouvrez l'objet ConfigMap `config-domain` pour commencer à l'éditer.
      ```
      kubectl edit configmap config-domain -n knative-serving
      ```
      {: pre}

   2. Spécifiez votre domaine personnalisé dans la section `data` de votre objet ConfigMap et retirez le domaine par défaut qui est défini pour votre cluster.
      - **Exemple d'affectation d'un nom d'hôte à partir de votre domaine personnalisé pour tous les services Knative** :
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        Lorsque vous ajoutez `""` à votre domaine personnalisé, tous les services Knative que vous créez se voient affecter un nom d'hôte issu de votre domaine personnalisé.   

      - **Exemple d'affectation d'un nom d'hôte à partir de votre domaine personnalisé pour certains services Knative sélectionnés** :
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: |
            selector:
              app: sample
          mycluster.us-south.containers.appdomain.cloud: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        Pour affecter un nom d'hôte à partir de votre domaine personnalisé uniquement à certains services Knative sélectionnés, ajoutez une clé de libellé `data.selector` à votre objet ConfigMap. Dans cet exemple, tous les services ayant le libellé `app: sample` se voient affecter un nom d'hôte issu de votre domaine personnalisé. Assurez-vous de disposer également d'un nom de domaine à affecter à toutes les autres applications qui n'ont pas le libellé `app: sample`. Dans cet exemple, le domaine fourni par défaut par IBM, `mycluster.us-south.containers.appdomain.cloud`, est utilisé. 
    3. Sauvegardez vos modifications.

Vos règles de routage Ingress et vos objets ConfigMap Knative étant tous configurés, vous pouvez créer des services Knative avec votre domaine et votre certificat TLS personnalisés. 

## Accès à un service Knative à partir d'un autre service Knative
{: #knative-access-service}

Vous pouvez accéder à votre service Knative à partir d'un autre service Knative en utilisant un appel d'API REST vers l'URL qui est affectée à votre service Knative.
{: shortdesc}

1. Répertoriez tous les services Knative dans votre cluster.
   ```
   kubectl get ksvc --all-namespaces
   ```
   {: pre}

2. Extrayez le domaine (**DOMAIN**) qui est affecté à votre service Knative. 
   ```
   kubect get ksvc/<knative_service_name>
   ```
   {: pre}

   Exemple de sortie :
   ```
   NAME        DOMAIN                                                            LATESTCREATED     LATESTREADY       READY   REASON
   myservice   myservice.default.mycluster.us-south.containers.appdomain.cloud   myservice-rjmwt   myservice-rjmwt   True
   ```
   {: screen}

3. Utilisez le nom de domaine pour implémenter un appel d'API REST afin d'accéder à votre service Knative. Cet appel d'API REST doit faire partie de l'application pour laquelle vous créez un service Knative. Si le service Knative auquel vous souhaitez accéder se voit affecter une adresse URL locale au format `<service_name>.<namespace>.svc.cluster.local`, Knative conserve la demande d'API REST dans le réseau interne du cluster. 

   Exemple de fragment de code en Go :
   ```go
   resp, err := http.Get("<knative_service_domain_name>")
   if err != nil {
       fmt.Fprintf(os.Stderr, "Error: %s\n", err)
   } else if resp.Body != nil {
       body, _ := ioutil.ReadAll(resp.Body)
       fmt.Printf("Response: %s\n", string(body))
   }
   ```
   {: codeblock}

## Paramètres courants pour le service Knative
{: #knative-service-settings}

Passez en revue les paramètres courants pour le service Knative qui peuvent vous être utiles lorsque vous développez votre application sans serveur.
{: shortdesc}

- [Définition du nombre minimal et du nombre maximal de pods](#knative-min-max-pods)
- [Spécification du nombre maximal de demandes par pod](#max-request-per-pod)
- [Création d'applications sans serveur privées uniquement](#knative-private-only)
- [Forcer le service Knative à extraire à nouveau une image de conteneur](#knative-repull-image)

### Définition du nombre minimal et du nombre maximal de pods
{: #knative-min-max-pods}

Vous pouvez spécifier le nombre minimal et le nombre maximal de pods que vous souhaitez exécuter pour vos applications en utilisant une annotation. Par exemple, si vous ne souhaitez pas que Knative réduise la taille de votre application à zéro instance, affectez la valeur 1 au nombre minimal de pods.
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: ...
spec:
  runLatest:
    configuration:
      revisionTemplate:
        metadata:
          annotations:
            autoscaling.knative.dev/minScale: "1"
            autoscaling.knative.dev/maxScale: "100"
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>Description des composants du fichier YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
</thead>
<tbody>
<tr>
<td><code>autoscaling.knative.dev/minScale</code></td>
<td>Entrez le nombre minimal de pods que vous voulez exécuter dans le cluster. Knative ne peut pas réduire la taille de votre application à un seuil inférieur au nombre que vous définissez, même si aucun trafic réseau n'est reçu par votre application. Le nombre par défaut de pods es zéro.</td>
</tr>
<tr>
<td><code>autoscaling.knative.dev/maxScale</code></td>
<td>Entrez le nombre maximal de pods que vous voulez exécuter dans le cluster. Knative ne peut pas augmenter la taille de votre application à un seuil supérieur au nombre que vous définissez, même si vous avez plus de demandes que vos instances d'application en cours ne peuvent gérer. </td>
</tr>
</tbody>
</table>

### Spécification du nombre maximal de demandes par pod
{: #max-request-per-pod}

Vous pouvez spécifier le nombre maximal de demandes qu'une instance d'application peut recevoir et traiter avant que Knative envisage d'augmenter vos instances d'application. Par exemple, si vous affectez la valeur 1 au nombre maximal de demandes, votre instance d'application peut recevoir une demande à la fois. Si un seconde demande arrive avant que la première ne soit entièrement traitée, Knative augmente une autre instance.

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image: myimage
          containerConcurrency: 1
....
```
{: codeblock}

<table>
<caption>Description des composants du fichier YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
</thead>
<tbody>
<tr>
<td><code>containerConcurrency</code></td>
<td>Entrez le nombre maximal de demandes qu'une instance d'application peut recevoir à la fois avant que Knative envisage d'augmenter vos instances d'application. </td>
</tr>
</tbody>
</table>

### Création d'applications sans serveur privées uniquement
{: #knative-private-only}

Par défaut, chaque service Knative se voit affecter une route publique à partir de votre sous-domaine Ingress Istio et une route privée au format `<service_name>.<namespace>.cluster.local`. Vous pouvez utiliser la route publique pour accéder à votre application à partir du réseau public. Si vous souhaitez conserver votre service privé, vous pouvez ajouter le libellé `serving.knative.dev/visibility` à votre service Knative. Ce libellé indique à Knative qu'il doit affecter un nom d'hôte privé à votre service uniquement.
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
  labels:
    serving.knative.dev/visibility: cluster-local
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>Description des composants du fichier YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
</thead>
<tbody>
<tr>
<td><code>serving.knative.dev/visibility</code></td>
  <td>Si vous ajoutez le libellé <code>serving.knative.dev/visibility: cluster-local</code>, votre service se voit affecter une route privée uniquement, au format <code>&lt;service_name&gt;.&lt;namespace&gt;.cluster.local</code>. Vous pouvez utiliser le nom d'hôte privé pour accéder à votre service à partir du cluster, mais vous ne pouvez pas accéder à votre service à partir du réseau public. </td>
</tr>
</tbody>
</table>

### Forcer le service Knative à extraire à nouveau une image de conteneur
{: #knative-repull-image}

L'implémentation en cours de Knative ne fournit pas un moyen standard de forcer le composant `Serving` de Knative à extraire à nouveau une image de conteneur. Pour extraire à nouveau une image à partir de votre registre, sélectionnez l'une des options suivantes:

- **Modifiez le modèle de révision (`revisionTemplate`**) d'un service Knative : Le modèle de révision (`revisionTemplate`) d'un service Knative permet de créer une révision de votre service Knative. Si vous modifiez ce modèle de révision et par exemple, vous ajoutez l'annotation `repullFlag`, Knative doit créer une nouvelle révision pour votre application. Dans le cadre de la création de la révision, Knative doit rechercher des mises à jour d'image de conteneur. Lorsque vous définissez `imagePullPolicy: Always`, Knative ne peut pas utiliser le cache d'image dans le cluster, mais en revanche, il doit extraire l'image de votre registre de conteneur. 
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: <service_name>
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           metadata:
             annotations:
               repullFlag: 123
           spec:
             container:
               image: <image_name>
               imagePullPolicy: Always
    ```
    {: codeblock}

    Vous devez modifier la valeur `repullFlag` à chaque fois que vous souhaitez créer une nouvelle révision de votre service qui extrait la version d'image la plus récente à partir de votre registre de conteneur. Prenez soin d'utiliser une valeur unique pour chaque révision afin d'éviter que Knative utilise une ancienne version d'image en raison de deux configurations de service Knative identiques.   
    {: note}

- **Utilisez des balises pour créer des images de conteneur uniques** : Vous pouvez utiliser des balises uniques pour chaque image de conteneur que vous créez, puis référencer cette image dans la configuration `container.image` de votre service Knative. Dans l'exemple suivant, `v1` est utilisé comme balise d'image. Pour forcer Knative à extraire une nouvelle image à partir de votre conteneur du registre, vous devez modifier la balise d'image. Par exemple, utilisez `v2` comme nouvelle balise d'image. 
  ```
  apiVersion: serving.knative.dev/v1alpha1
  kind: Service
  metadata:
    name: <service_name>
  spec:
    runLatest:
      configuration:
          spec:
            container:
              image: myapp:v1
              imagePullPolicy: Always
    ```
    {: codeblock}


## Liens connexes  
{: #knative-related-links}

- Testez le [workshop Knative ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/IBM/knative101/tree/master/workshop) pour déployer votre première application fibonacci `Node.js` dans votre cluster.
  - Découvrez comment utiliser la primitive Knative `Build` pour générer une image à partir d'un fichier Dockerfile dans GitHub et insérer automatiquement l'image dans votre espace de nom dans {{site.data.keyword.registrylong_notm}}.  
  - Apprenez à configurer le routage du trafic réseau du sous-domaine Ingress fourni par IBM vers la passerelle Ingress d'Istio fournie par Knative.
  - Déployez une nouvelle version de votre application et utilisez Istio pour contrôler la quantité de trafic acheminé vers chaque version de l'application.
- Explorez des exemples de [Knative `Eventing` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/knative/docs/tree/master/eventing/samples).
- Pour plus d'informations sur Knative, consultez la [documentation Knative ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/knative/docs).
