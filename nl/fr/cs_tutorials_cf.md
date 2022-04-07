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


# Tutoriel : Migration d'une application de Cloud Foundry vers un cluster
{: #cf_tutorial}

Vous pouvez prendre une application que vous avez déjà déployée avec Cloud Foundry et déployer le même code dans un conteneur sur un cluster Kubernetes dans {{site.data.keyword.containerlong_notm}}.
{: shortdesc}


## Objectifs
{: #cf_objectives}

- Découvrir le processus général de déploiement d'applications dans des conteneurs sur un cluster Kubernetes.
- Créer un fichier Dockerfile à partir du code de votre application pour générer une image de conteneur.
- Déployer un conteneur à partir de cette image dans un cluster Kubernetes.

## Durée
{: #cf_time}

30 minutes

## Public
{: #cf_audience}

Ce tutoriel s'adresse aux développeurs d'applications Cloud Foundry.

## Prérequis
{: #cf_prereqs}

- [Créez un registre d'images privé dans {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-getting-started).
- [Créez un cluster](/docs/containers?topic=containers-clusters#clusters_ui).
- [Ciblez votre interface CLI sur le cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- Vérifiez que vous disposez des règles d'accès {{site.data.keyword.Bluemix_notm}} IAM pour {{site.data.keyword.containerlong_notm}} :
    - [N'importe quel rôle de plateforme](/docs/containers?topic=containers-users#platform)
    - [Rôle de service **Auteur** ou **Responsable**](/docs/containers?topic=containers-users#platform)
- [Familiarisez-vous avec la terminologie de Docker et Kubernetes](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology).


<br />



## Leçon 1 : Téléchargement du code de l'application
{: #cf_1}

Obtenez votre code prêt à l'emploi. Vous n'avez pas encore de code ? Vous pouvez télécharger le code de démarrage à utiliser dans ce tutoriel.
{: shortdesc}

1. Créez un répertoire nommé `cf-py` et accédez à ce répertoire. Dans ce répertoire, vous sauvegardez tous les fichiers requis pour construire l'image Docker et exécuter votre application.

  ```
  mkdir cf-py && cd cf-py
  ```
  {: pre}

2. Copiez le code de l'application et tous les fichiers associés dans le répertoire. Vous pouvez utiliser le code de votre propre application ou télécharger un conteneur boilerplate à partir du catalogue. Ce tutoriel utilise le conteneur boilerplate Flask Python. Vous pouvez toutefois utiliser les mêmes étapes de base avec un fichier Node.js, du code Java ou une application [Kitura](https://github.com/IBM-Cloud/Kitura-Starter).

    Pour télécharger le code d'application Flask Python :

    a. Dans le catalogue, à la section **Conteneurs boilerplate**, cliquez sur **Flask Python**. Ce conteneur boilerplate comprend un environnement d'exécution pour les applications Python 2 et Python 3.

    b. Entrez le nom de l'application `cf-py-<name>`, puis cliquez sur **Créer**. Pour accéder au code d'application du conteneur boilerplate, vous devez d'abord déployer l'application CF dans le cloud. Vous pouvez utiliser n'importe quel nom pour l'application. Si vous utilisez le nom de l'exemple, remplacez `<name>` par un identificateur unique, tel que `cf-py-msx`.

    **Attention** : n'utilisez pas d'informations personnelles dans les noms d'application, d'image de conteneur ou de ressource Kubernetes.

    A mesure que l'application est déployée, sont affichées des instructions pour le téléchargement, la modification et le redéploiement de votre application avec l'interface de ligne de commande.

    c. Dans l'étape 1 des instructions de la console, cliquez sur **TELECHARGER LE CODE DE DEMARRAGE**.

    d. Extrayez le fichier `.zip` et sauvegardez son contenu dans votre répertoire `cf-py`.

Votre code d'application est désormais prêt à être conteneurisé.


<br />



## Leçon 2 : Création d'une image Docker avec votre code d'application
{: #cf_2}

Créez un fichier Dockerfile incluant votre code d'application et les configurations nécessaires pour votre conteneur. Générez ensuite une image Docker depuis ce fichier Dockerfile et transférez-la à votre registre d'images privé.
{: shortdesc}

1. Dans le répertoire `cf-py` que vous avez créé dans la leçon précédente, créez un fichier `Dockerfile`, lequel constitue la base pour la création d'une image de conteneur. Vous pouvez créer le fichier Dockerfile à l'aide de l'éditeur CLI de votre choix ou d'un éditeur de texte sur votre ordinateur. L'exemple suivant illustre comment créer un fichier Dockerfile avec l'éditeur nano.

  ```
  Dockerfile nano
  ```
  {: pre}

2. Copiez le script suivant dans le fichier Dockerfile. Ce fichier s'applique spécifiquement à une application Python. Si vous utilisez un autre type de code, votre fichier Dockerfile doit inclure une autre image de base et peut nécessiter la définition d'autres zones.

  ```
  #Use the Python image from DockerHub as a base image
  FROM python:3-slim

  #Expose the port for your python app
  EXPOSE 5000

  #Copy all app files from the current directory into the app
  #directory in your container. Set the app directory
  #as the working directory
  WORKDIR /cf-py/
  COPY .  .

  #Install any requirements that are defined
  RUN pip install --no-cache-dir -r requirements.txt

  #Update the openssl package
  RUN apt-get update && apt-get install -y \
  openssl

  #Start the app.
  CMD ["python", "welcome.py"]
  ```
  {: codeblock}

3. Sauvegardez vos modifications dans l'éditeur nano en appuyant sur les touches `ctrl + o`. Confirmez vos modifications en appuyant sur `Entrée`. Quittez l'éditeur nano en appuyant sur les touches `ctrl + x`.

4. Générez une image Docker avec votre code d'application et transférez-la sur votre registre privé.

  ```
  ibmcloud cr build -t <region>.icr.io/namespace/cf-py .
  ```
  {: pre}

  <table>
  <caption>Description des composantes de cette commande</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Cette icône indique qu'il y a d'autres informations pour en savoir plus sur les composantes de cette commande."/> Description des composantes de cette commande</th>
  </thead>
  <tbody>
  <tr>
  <td>Paramètre</td>
  <td>Description</td>
  </tr>
  <tr>
  <td><code>build</code></td>
  <td>Commande build.</td>
  </tr>
  <tr>
  <td><code>-t registry.&lt;region&gt;.bluemix.net/namespace/cf-py</code></td>
  <td>Chemin de votre registre privé comprenant votre espace de nom unique et le nom de l'image. Dans cet exemple, nous avons conservé pour l'image le même nom que le répertoire de l'application, mais vous pouvez choisir un nom de votre choix pour l'image dans votre registre privé. Si vous n'êtes pas sûr du nom de votre espace de nom, exécutez la commande `ibmcloud cr namespaces` pour le retrouver.</td>
  </tr>
  <tr>
  <td><code>.</code></td>
  <td>Emplacement du fichier Dockerfile. Si vous exécutez la commande build depuis le répertoire où réside le fichier Dockerfile, entrez un point (.). Sinon, utilisez le chemin relatif du fichier Dockerfile.</td>
  </tr>
  </tbody>
  </table>

  L'image est créée dans votre registre privé. Vous pouvez exécuter la commande `ibmcloud cr images` pour vérifier que l'image a bien été créée.

  ```
  REPOSITORY                       NAMESPACE   TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS   
  us.icr.io/namespace/cf-py        namespace   latest   cb03170b2cb2   3 minutes ago   271 MB   OK
  ```
  {: screen}


<br />



## Leçon 3 : Déploiement d'un conteneur depuis votre image
{: #cf_3}

Déployez votre application sous forme de conteneur dans un cluster Kubernetes.
{: shortdesc}

1. Créez un fichier de configuration YAML nommé `cf-py.yaml` et mettez à jour `<registry_namespace>` avec le nom de votre registre d'images privé. Le fichier de configuration définit un déploiement de conteneur à partir de l'image que vous avez créée dans la leçon précédente et un service permettant d'exposer l'application au public.

  ```
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    labels:
      app: cf-py
    name: cf-py
    namespace: default
  spec:
    selector:
      matchLabels:
        app: cf-py
    replicas: 1
    template:
      metadata:
        labels:
          app: cf-py
      spec:
        containers:
        - image: us.icr.io/<registry_namespace>/cf-py:latest
          name: cf-py
  ---
  apiVersion: v1
  kind: Service
  metadata:
    name: cf-py-nodeport
    labels:
      app: cf-py
  spec:
    selector:
      app: cf-py
    type: NodePort
    ports:
     - port: 5000
       nodePort: 30872
  ```
  {: codeblock}

  <table>
  <caption>Description des composants du fichier YAML</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
  </thead>
  <tbody>
  <tr>
  <td><code>image</code></td>
  <td>Dans `us.icr.io/<registry_namespace>/cf-py:latest`, remplacez &lt;registry_namespace&gt; par l'espace de nom de votre registre d'images privé. Si vous n'êtes pas sûr du nom de votre espace de nom, exécutez la commande `ibmcloud cr namespaces` pour le retrouver.</td>
  </tr>
  <tr>
  <td><code>nodePort</code></td>
  <td>Exposez votre application en créant un service Kubernetes de type NodePort. Les ports de noeud (NodePort) sont compris entre 30000 et 32767. Vous utiliserez ce port pour tester votre application dans un navigateur par la suite.</td>
  </tr>
  </tbody></table>

2. Appliquez le fichier de configuration pour créer le déploiement et le service dans votre cluster.

  ```
  kubectl apply -f <filepath>/cf-py.yaml
  ```
  {: pre}

  Sortie :

  ```
  deployment "cf-py" configured
  service "cf-py-nodeport" configured
  ```
  {: screen}

3. Maintenant que la tâche de déploiement est terminée, vous pouvez tester votre application dans un navigateur. Extrayez les informations détaillées pour composer l'URL.

    a.  Identifiez l'adresse IP publique du noeud worker dans le cluster.

    ```
    ibmcloud ks workers --cluster <cluster_name>
    ```
    {: pre}

    Sortie :

    ```
    ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
    kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   u3c.2x4.encrypted   normal   Ready    dal10   1.13.6
    ```
    {: screen}

    b. Ouvrez un navigateur et accédez à l'application via l'URL `http://<public_IP_address>:<NodePort>`. En utilisant les valeurs de l'exemple, l'URL est `http://169.xx.xxx.xxx:30872`. Vous pouvez communiquer cette URL à un collègue pour la tester, ou bien l'entrer dans le navigateur de votre téléphone portable pour constater que l'application est réellement accessible au public.

    <img src="images/python_flask.png" alt="Capture d'écran de l'application de conteneur boilerplate Flask Python déployée." />

5.  [Lancez le tableau de bord Kubernetes](/docs/containers?topic=containers-app#cli_dashboard).

    Si vous sélectionnez votre cluster dans la [console {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/), vous pouvez utiliser le bouton **Tableau de bord Kubernetes** pour lancer votre tableau de bord en un seul clic.
    {: tip}

6. Dans l'onglet **Charges de travail**, vous pouvez examiner les ressources que vous avez créées.

Bien joué ! Votre application est déployée dans un conteneur.
