---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

keywords: kubernetes, iks, helm, without tiller, private cluster tiller, integrations, helm chart

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



# Ajout de services à l'aide d'une liaison de service IBM Cloud
{: #service-binding}

Ajoutez des services {{site.data.keyword.Bluemix_notm}} pour améliorer votre cluster Kubernetes en ajoutant des fonctionnalités supplémentaires dans des domaines tels que Watson AI, les données, la sécurité et Internet of Things (IoT).
{:shortdesc}

**Quels types de services puis-je lier à mon cluster ?** </br>
Lorsque vous ajoutez des services {{site.data.keyword.Bluemix_notm}} à votre cluster, vous avez le choix entre les services qui sont activés pour {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) et les services qui sont basés sur Cloud Foundry. Les services activés par IAM offrent une contrôle d'accès plus fin et peuvent être gérés dans un groupe de ressources {{site.data.keyword.Bluemix_notm}}. Les services Cloud Foundry doivent être ajoutés à une organisation et à un espace Cloud Foundry et ne peuvent pas être ajoutés à un groupe de ressources. Pour contrôler l'accès à votre instance de service Cloud Foundry, vous utilisez des rôles Cloud Foundry. Pour plus d'informations sur les services activés par IAM et les services Cloud Foundry, voir [Qu'est-ce qu'une ressource ?](/docs/resources?topic=resources-resource#resource).

Pour obtenir la liste des services {{site.data.keyword.Bluemix_notm}} pris en charge, voir le [catalogue {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/catalog).

**Qu'est-ce qu'une liaison de service {{site.data.keyword.Bluemix_notm}} ?**</br>
Une liaison de service est un moyen de créer rapidement des données d'identification de service pour un service {{site.data.keyword.Bluemix_notm}} et de stocker ces données dans une valeur confidentielle Kubernetes dans votre cluster. Pour lier un service à votre cluster, vous devez d'abord mettre à disposition une instance du service. Vous utilisez ensuite la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind) `ibmcloud ks cluster-service-bind` pour créer les données d'identification de service et la valeur confidentielle Kubernetes. La valeur confidentielle est chiffrée automatiquement dans etcd pour protéger vos données.

Vous voulez sécuriser davantage vos valeurs confidentielles ? Demandez à l'administrateur de votre cluster d'[activer {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) dans votre cluster pour chiffrer les valeurs confidentielles qu'elles soient nouvelles ou existantes, par exemple la valeur confidentielle dans laquelle sont stockées les données d'identification de vos instances de service {{site.data.keyword.Bluemix_notm}}.
{: tip}

**Puis-je utiliser tous les services {{site.data.keyword.Bluemix_notm}} dans mon cluster ?**</br>
Vous pouvez utiliser une liaison de service uniquement pour vos services qui prennent en charge les clés de service de sorte que les données d'identification de service puissent être créées et stockées automatiquement dans une valeur confidentielle Kubernetes. Pour trouver une liste de services compatibles avec les clés de service, voir [Utilisation des services {{site.data.keyword.Bluemix_notm}} avec des applications externes](/docs/resources?topic=resources-externalapp#externalapp).

Les services qui ne prennent pas en charge les clés de service fournissent généralement une API que vous pouvez utiliser dans votre application. La méthode de liaison de service ne configure pas automatiquement l'accès API pour votre application. Prenez soin d'examiner la documentation d'API de votre service et d'implémenter l'API dans votre application.

## Ajout de services IBM Cloud à des clusters
{: #bind-services}

Utilisez une liaison de service {{site.data.keyword.Bluemix_notm}} pour créer automatiquement des données d'identification de service pour vos services {{site.data.keyword.Bluemix_notm}} et stockez ces données d'identification dans une valeur confidentielle Kubernetes.
{: shortdesc}

Avant de commencer :
- Vérifiez que vous disposez des rôles suivants :
    - [Rôle d'accès de plateforme {{site.data.keyword.Bluemix_notm}} IAM **Editeur** ou **Administrateur**](/docs/containers?topic=containers-users#platform) pour le cluster dans lequel vous souhaitez lier un service
    - [Rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Auteur** ou **Responsable**](/docs/containers?topic=containers-users#platform) pour l'espace de nom Kubernetes dans lequel vous souhaitez lier le service
    - Pour les services Cloud Foundry : [Rôle Cloud Foundry **Développeur**](/docs/iam?topic=iam-mngcf#mngcf) pour l'espace dans lequel vous souhaitez mettre à disposition le service
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Pour ajouter un service {{site.data.keyword.Bluemix_notm}} dans votre cluster :

1. [Créez une instance du service {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).
    * Certains services {{site.data.keyword.Bluemix_notm}} ne sont disponibles que dans des régions éligibles. Vous pouvez lier un service à votre cluster uniquement si ce service est disponible dans la même région que votre cluster. En outre, si vous souhaitez créer une instance de service dans la zone Washington DC, vous devez utiliser l'interface CLI.
    * **Pour les services activés par IAM** : vous devez créer l'instance de service dans le même groupe de ressources que votre cluster. Un service peut être créé dans un seul groupe de ressources que vous ne pouvez plus modifier par la suite.

2. Vérifiez le type de service que vous avez créé et notez le **nom** de l'instance de service.
   - **Services Cloud Foundry :**
     ```
     ibmcloud service list
     ```
     {: pre}

     Exemple de sortie :
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **Services activés par {{site.data.keyword.Bluemix_notm}} IAM :**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}

     Exemple de sortie :
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   Vous pouvez également voir les différents types de service dans votre tableau de bord {{site.data.keyword.Bluemix_notm}} sous **Services Cloud Foundry** et **Services**.

3. Identifiez l'espace de nom du cluster que vous désirez ajouter à votre service.
   ```
   kubectl get namespaces
   ```
   {: pre}

4. Liez le service à votre cluster pour créer les données d'identification de service pour votre service et stockez ces données d'identification dans une valeur confidentielle Kubernetes. Si vous disposez de données d'identification de service existantes, utilisez l'option `--key` pour spécifier ces données d'identification. Pour les services activés par IAM, les données d'identification sont créées automatiquement avec le rôle d'accès au service **Auteur**, mais vous pouvez utiliser l'option `--role` pour spécifier un autre rôle d'accès au service. Si vous utilisez l'option `--key`, n'incluez pas l'option `--role`.
   ```
   ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
   ```
   {: pre}

   Lorsque la création des données d'identification de service aboutit, une valeur confidentielle Kubernetes portant le nom `binding-<service_instance_name>` est créée.  

   Exemple de sortie :
   ```
   ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
   ```
   {: screen}

5. Vérifiez les données d'identification du service dans votre valeur confidentielle Kubernetes.
   1. Obtenez les détails de la valeur confidentielle et notez la valeur de **binding**. La valeur de **binding** est codée en base64 et contient les données d'identification pour votre instance de service au format JSON.
      ```
      kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
      ```
      {: pre}

      Exemple de sortie :
      ```
      apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
      ```
      {: screen}

   2. Décodez la valeur de binding.
      ```
      echo "<binding>" | base64 -D
      ```
      {: pre}

      Exemple de sortie :
      ```
      {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

   3. Facultatif : comparez les données d'identification du service que vous avez décodées à l'étape précédente aux données d'identification du service que vous trouvez pour votre instance de service dans le tableau de bord {{site.data.keyword.Bluemix_notm}}.

6. Votre service étant désormais lié à votre cluster, vous devez configurer votre application pour [accéder aux données d'identification du service dans la valeur confidentielle Kubernetes](#adding_app).


## Accès aux données d'identification à partir de vos applications
{: #adding_app}

Pour accéder à une instance de service {{site.data.keyword.Bluemix_notm}} à partir de votre application, vous devez rendre les données d'identification stockées dans la valeur confidentielle Kubernetes accessibles à votre application.
{: shortdesc}

Les données d'identification d'une instance de service sont codées en base64 et stockées au format JSON dans votre valeur confidentielle. Pour accéder aux données figurant dans votre valeur confidentielle, choisissez l'une des options suivantes :
- [Monter la valeur confidentielle sous forme de volume sur votre pod](#mount_secret)
- [Référencer la valeur confidentielle dans les variables d'environnement](#reference_secret)
<br>

Avant de commencer :
-  Vérifiez que vous disposez du [rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Auteur** ou **Responsable**](/docs/containers?topic=containers-users#platform) pour l'espace de nom `kube-system`.
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- [Ajoutez un service {{site.data.keyword.Bluemix_notm}} à votre cluster](#bind-services).

### Montage de la valeur confidentielle sous forme de volume dans votre pod
{: #mount_secret}

Lorsque vous montez la valeur confidentielle sous forme de volume dans votre pod, un fichier nommé `binding` est stocké dans le répertoire de montage du volume. Le fichier `binding` au format JSON comprend toutes les informations et les données d'identification dont vous avez besoin pour accéder au service {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

1.  Répertoriez les valeurs confidentielles disponibles dans votre cluster et notez le **nom** de la vôtre. Recherchez une valeur confidentielle de type **Opaque**. S'il existe plusieurs valeurs confidentielles, contactez l'administrateur de votre cluster pour identifier la valeur confidentielle appropriée.
    ```
    kubectl get secrets
    ```
    {: pre}

    Exemple de sortie :
    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
    ```
    {: screen}

2.  Créez un fichier YAML pour votre déploiement Kubernetes et montez la valeur confidentielle sous forme de volume sur votre pod.
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: icr.io/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>Description des composants du fichier YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>Chemin absolu du répertoire où est monté le volume dans le conteneur.</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>Nom du volume à monter sur votre pod.</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>Droits d'accès en lecture et écriture à la valeur confidentielle. Utilisez `420` pour définir les droits en lecture seule. </td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>Nom de la valeur confidentielle que vous avez notée à l'étape précédente.</td>
    </tr></tbody></table>

3.  Créez le pod et montez la valeur confidentielle sous forme de volume.
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  Vérifiez que le pod a bien été créé.
    ```
    kubectl get pods
    ```
    {: pre}

    Exemple de sortie d'interface CLI :

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  Accédez aux données d'identification pour le service.
    1. Connectez-vous à votre pod.
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. Accédez au chemin de montage du volume que vous avez défini précédemment et répertoriez les fichiers figurant dans ce chemin.
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       Exemple de sortie :
       ```
       binding
       ```
       {: screen}

       Le fichier `binding` comprend les données d'identification pour le service que vous avez stockées dans la valeur confidentielle Kubernetes.

    4. Affichez les données d'identification du service. Ces données sont stockées sous forme de paires clé-valeur au format JSON.
       ```
       cat binding
       ```
       {: pre}

       Exemple de sortie :
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. Configurez votre application pour analyser le contenu JSON et extraire les informations dont vous avez besoin pour accéder à votre service.

### Référencement de la valeur confidentielle dans les variables d'environnement
{: #reference_secret}

Vous pouvez ajouter les données d'identification du service et d'autres paires clé-valeur figurant dans votre valeur confidentielle Kubernetes sous forme de variables d'environnement dans votre déploiement.
{: shortdesc}

1. Répertoriez les valeurs confidentielles disponibles dans votre cluster et notez le **nom** de la vôtre. Recherchez une valeur confidentielle de type **Opaque**. S'il existe plusieurs valeurs confidentielles, contactez l'administrateur de votre cluster pour identifier la valeur confidentielle appropriée.
   ```
   kubectl get secrets
   ```
   {: pre}

   Exemple de sortie :
   ```
   NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
   ```
   {: screen}

2. Obtenez les détails de votre valeur confidentielle pour rechercher les paires clé-valeur potentielles que vous pouvez référencer sous forme de variables d'environnement dans votre pod. Les données d'identification du service sont stockées dans la clé `binding` de votre valeur confidentielle.
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   Exemple de sortie :
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Créez un fichier YAML pour votre déploiement Kubernetes et indiquez une variable d'environnement qui référence la clé `binding`.
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: icr.io/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>Description des composants du fichier YAML</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>Nom de votre variable d'environnement.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>Nom de la valeur confidentielle que vous avez notée à l'étape précédente.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>Clé faisant partie de votre valeur confidentielle que vous voulez référencer dans votre variable d'environnement. Pour référencer les données d'identification du service, vous devez utiliser la clé <strong>binding</strong>.  </td>
     </tr>
     </tbody></table>

4. Créez le pod qui référence la clé `binding` de votre valeur confidentielle sous forme de variable d'environnement.
   ```
   kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. Vérifiez que le pod a bien été créé.
   ```
   kubectl get pods
   ```
   {: pre}

   Exemple de sortie d'interface CLI :
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. Vérifiez que la variable d'environnement est définie correctement.
   1. Connectez-vous à votre pod.
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. Répertoriez toutes les variables d'environnement dans le pod.
      ```
      env
      ```
      {: pre}

      Exemple de sortie :
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. Configurez votre application pour lire la variable d'environnement et analyser le contenu JSON pour extraire les informations dont vous avez besoin pour accéder à votre service.

   Exemple de code en Python :
   ```python
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

8. Facultatif : par précaution, ajoutez le traitement d'erreurs à votre application au cas où la variable d'environnement `BINDING` ne serait pas correctement définie.

   Exemple de code en Java :
   ```java
   if (System.getenv("BINDING") == null) {
    throw new RuntimeException("Environment variable 'SECRET' is not set!");
   }
   ```
   {: codeblock}

   Exemple de code en Node.js :
   ```js
   if (!process.env.BINDING) {
    console.error('ENVIRONMENT variable "BINDING" is not set!');
    process.exit(1);
   }
   ```
   {: codeblock}
