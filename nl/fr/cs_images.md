---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-04"

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



# Génération de conteneurs à partir d'images
{: #images}

Une image Docker est la base de chaque conteneur que vous créez avec {{site.data.keyword.containerlong}}.
{:shortdesc}

L'image est créée depuis un Dockerfile, lequel est un fichier contenant des instructions pour générer l'image. Un Dockerfile peut référencer dans ses instructions des artefacts de génération stockés séparément, comme une application, sa configuration, et ses dépendances.

## Planification de registres d'images
{: #planning_images}

Les images sont généralement stockées dans un registre d'images pouvant être accessible au public (registre public)  ou, au contraire, dont l'accès est limité à un petit groupe d'utilisateurs (registre privé).
{:shortdesc}

Des registres publics, tel que Docker Hub, peuvent être utilisés pour vous familiariser avec Docker et Kubernetes pour créer votre première application conteneurisée dans un cluster. Dans le cas d'applications d'entreprise, utilisez un registre privé, tel que celui fourni dans {{site.data.keyword.registryshort_notm}}, pour empêcher l'utilisation et la modification de vos images par des utilisateurs non habilités. Les registres privés sont mis en place par l'administrateur du cluster pour garantir que les données d'identification pour accéder au registre privé sont disponibles aux utilisateurs du cluster.


Vous pouvez utiliser plusieurs registres avec {{site.data.keyword.containerlong_notm}} pour déployer des applications dans votre cluster.

|Registre|Description|Avantage|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started)|Cette option vous permet de mettre en place votre propre référentiel d'images Docker sécurisé dans {{site.data.keyword.registryshort_notm}} où vous pourrez stocker en sécurité vos images et les partager avec les autres utilisateurs du cluster.|<ul><li>Gestion de l'accès aux images dans votre compte.</li><li>Utilisation d'images et de modèles d'application fournis par {{site.data.keyword.IBM_notm}}, comme {{site.data.keyword.IBM_notm}} Liberty,  en tant qu'image parent à laquelle vous ajouterez votre propre code d'application.</li><li>Analyse automatique des images pour détection de vulnérabilités potentielles par Vulnerability Advisor et soumission de recommandations spécifiques au système d'exploitation pour les corriger.</li></ul>|
|Tout autre registre privé|Connexion de n'importe quel registre privé existant à votre cluster en créant une [valeur confidentielle d'extraction d'image ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/containers/images/). Cette valeur est utilisée pour enregistrer de manière sécurisée l'URL de votre registre et les données d'identification dans une valeur confidentielle Kubernetes.|<ul><li>Utilisation de registres privés existants indépendamment de leur source (Docker Hub, registres dont l'organisation est propriétaire, autres registres Cloud privés).</li></ul>|
|[Docker Hub public![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://hub.docker.com/){: #dockerhub}|Utilisez cette option pour utiliser des images publiques Docker Hub existantes directement dans votre [déploiement Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) lorsqu'il n'y a pas besoin d'effectuer des modifications dans Dockerfile. <p>**Remarque :** gardez à l'esprit que cette option peut ne pas satisfaire les exigences de sécurité de votre organisation (par exemple, en matière de gestion des accès, d'analyse des vulnérabilités ou de protection des données confidentielles de l'application).</p>|<ul><li>Aucune configuration supplémentaire du cluster n'est nécessaire.</li><li>Inclut un certain nombre d'applications en code source ouvert.</li></ul>|
{: caption="Options de registre d'images public et privé" caption-side="top"}

Une fois que vous avez configuré un registre d'images, les utilisateurs du cluster peuvent utiliser les images pour déployer des applications dans le cluster.

Découvrez comment [sécuriser vos informations personnelles](/docs/containers?topic=containers-security#pi) lorsque vous utilisez des images de conteneur.

<br />


## Configuration de contenu sécurisé pour des images de conteneur
{: #trusted_images}

Vous pouvez générer des conteneurs à partir d'images sécurisées qui sont signées et stockées dans {{site.data.keyword.registryshort_notm}} et empêcher les déploiements à partir d'images non signées ou vulnérables.
{:shortdesc}

1.  [Signez les images pour le contenu sécurisé](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent). Après avoir défini la fonction de confiance (trust) pour vos images, vous pouvez gérer du contenu sécurisé et les signataires peuvent envoyer des images par commande push dans votre registre.
2.  Pour imposer une règle stipulant que seules des images signées peuvent être utilisées pour générer des conteneurs dans votre cluster, [ajoutez Container Image Security Enforcement (bêta)](/docs/services/Registry?topic=registry-security_enforce#security_enforce).
3.  Déployez votre application.
    1. [Déploiement dans l'espace de nom Kubernetes `default`](#namespace).
    2. [Déploiement dans un autre espace de nom Kubernetes ou à partir d'une autre région ou d'un autre compte {{site.data.keyword.Bluemix_notm}}](#other).

<br />


## Déploiement de conteneurs à partir d'une image {{site.data.keyword.registryshort_notm}} dans l'espace de nom Kubernetes `default`
{: #namespace}

Vous pouvez déployer dans votre cluster des conteneurs depuis une image fournie par IBM ou depuis une image privée stockée dans votre espace de nom {{site.data.keyword.registryshort_notm}}. Pour plus d'informations sur la façon dont votre cluster accède aux images de registre, voir [Comment votre cluster est-il autorisé à extraire des images d'{{site.data.keyword.registrylong_notm}}](#cluster_registry_auth).
{:shortdesc}

Avant de commencer :
1. [Configurez un espace de nom dans {{site.data.keyword.registryshort_notm}} et envoyez des images dans cet espace de nom](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2. [Créez un cluster](/docs/containers?topic=containers-clusters#clusters_ui).
3. Si vous utilisez un cluster créé avant le **25 février 2019**, [mettez à jour votre cluster pour utiliser la valeur `imagePullSecret` de la clé d'API](#imagePullSecret_migrate_api_key).
4. [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Pour déployer un conteneur dans l'espace de nom **default** de votre cluster :

1.  Créez un fichier de configuration de déploiement nommé `mydeployment.yaml`.
2.  Définissez le déploiement et l'image de votre espace de nom que vous désirez utiliser dans {{site.data.keyword.registryshort_notm}}.

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app_name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - name: <app_name>
            image: <region>.icr.io/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    Remplacez les variables d'URL de l'image par les informations correspondant à votre image :
    *  **`<app_name>`** : nom de votre application.
    *  **`<region>`** : noeud final d'API {{site.data.keyword.registryshort_notm}} régional pour le domaine de registre. Afin de répertorier le domaine pour la région à laquelle vous êtes connecté, exécutez la commande `ibmcloud cr api`.
    *  **`<namespace>`** : espace de nom de registre. Pour obtenir des informations sur votre espace de nom, exécutez la commande `ibmcloud cr namespace-list`.
    *  **`<my_image>:<tag>`** : image et balise que vous souhaitez utiliser pour créer le conteneur. Pour obtenir les images disponibles dans votre registre, exécutez la commande `ibmcloud cr images`.

3.  Créez le déploiement dans votre cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

<br />


## Comment autoriser votre cluster à extraire des images d'un registre ?
{: #cluster_registry_auth}

Pour extraire des images d'un registre, votre cluster {{site.data.keyword.containerlong_notm}} utilise un type spécial de secret Kubernetes, un `imagePullSecret`. Cette valeur confidentielle d'extraction d'image stocke les données d'identification permettant d'accéder à un registre de conteneur. Le registre de conteneur peut être votre espace de nom dans {{site.data.keyword.registrylong_notm}}, un espace de nom dans {{site.data.keyword.registrylong_notm}} qui appartient à un autre compte {{site.data.keyword.Bluemix_notm}} ou n'importe quel autre registre privé, tel que Docker. Votre cluster est configuré pour extraire des images de votre espace de nom dans {{site.data.keyword.registrylong_notm}} et déployer des conteneurs à partir de ces images dans l'espace de nom kubernetes `default` dans votre cluster. Si vous devez extraire des images dans d'autres espaces de nom Kubernetes de cluster ou d'autres registres, vous devez configurer la valeur confidentielle d'extraction d'image.
{:shortdesc}

**Comment mon cluster est-il configuré pour extraire des images de l'espace de nom Kubernetes `default` ?**<br>
Lorsque vous créez un cluster, il dispose d'un ID de service {{site.data.keyword.Bluemix_notm}} IAM associé à une règle de rôle d'accès au service **Reader** IAM dans {{site.data.keyword.registrylong_notm}}. Les données d'identification de l'ID de service sont représentées dans une clé d'API qui n'expire jamais et qui est stockée dans des valeurs confidentielles d'extraction d'image dans votre cluster. Ces valeurs confidentielles sont ajoutées dans l'espace de nom Kubernetes `default` et la liste de ces valeurs dans le compte de service `default` correspondant à cet espace de nom. En utilisant des valeurs confidentielles d'extraction d'image, vos déploiements peuvent extraire des images (avec accès en lecture seule) dans votre [registre global et régional](/docs/services/Registry?topic=registry-registry_overview#registry_regions) pour générer des conteneurs dans l'espace de nom Kubernetes `default`. Le registre global stocke de manière sécurisée des images publiques fournies par IBM auxquelles vous pouvez vous référer dans vos déploiements au lieu d'utiliser des références différentes pour les images stockées dans chaque registre régional. Le registre régional stocke vos propres images Docker privées de manière sécurisée.

**Puis-je restreindre l'accès pour l'extraction à un registre régional particulier ?**<br>
Oui, vous pouvez [editer la règle IAM existante de l'ID de service](/docs/iam?topic=iam-serviceidpolicy#access_edit) qui limite le rôle d'accès au service **Lecteur** à ce registre régional ou à une ressource de registre, comme par exemple, un espace de nom. Avant de personnaliser des règles IAM de registre, vous devez [activer les règles {{site.data.keyword.Bluemix_notm}} IAM pour {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-user#existing_users).

  Vous voulez sécuriser davantage les données d'identification de votre registre ? Demandez à l'administrateur de votre cluster d'[activer {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) dans votre cluster pour chiffrer les valeurs confidentielles Kubernetes, telles que `imagePullSecret` qui stocke les données d'identification de votre registre.
  {: tip}

**Puis-je extraire des images dans des espaces de nom Kubernetes autres que `default` ?**<br>
Par défaut, non. Avec la configuration de cluster par défaut, vous pouvez déployer des conteneurs depuis n'importe quelle image qui est stockée dans votre espace de nom {{site.data.keyword.registrylong_notm}} dans l'espace de nom Kubernetes `default` de votre cluster. Pour utiliser ces images dans d'autres espaces de nom Kubernetes ou d'autres comptes {{site.data.keyword.Bluemix_notm}}, [vous pouvez copier ou créer votre propre valeur confidentielle d'extraction d'image](#other).

**Puis-je extraire des images d'un autre compte {{site.data.keyword.Bluemix_notm}} ?**<br>
Oui, créez une clé d'API dans le compte {{site.data.keyword.Bluemix_notm}} que vous souhaitez utiliser. Ensuite, créez une valeur confidentielle d'extraction d'image qui stocke ces données d'identification de clé d'API dans chaque cluster et espace de nom de cluster à partir desquels vous souhaitez effectuer les extractions. [Suivez cet exemple qui utilise une clé d'API d'ID de service autorisé](#other_registry_accounts).

Pour utiliser un registre non {{site.data.keyword.Bluemix_notm}}, tel que Docker, voir [Accès aux images stockées dans d'autres registres privés](#private_images).

**La clé d'API doit-elle exister pour un ID de service ? Que se passe-t-il si j'atteins la limite d'ID de service pour mon compte ?**<br>
La configuration de cluster par défaut crée un ID de service pour stocker les données d'identification de clé d'API {{site.data.keyword.Bluemix_notm}} IAM dans la valeur confidentielle d'extraction d'image. Cependant, vous pouvez également créer une clé d'API pour un utilisateur individuel et stocker ces données d'identification dans une valeur confidentielle d'extraction d'image. Si vous atteignez la [limite IAM d'ID de service](/docs/iam?topic=iam-iam_limits#iam_limits), votre cluster est créé sans l'ID de service et la valeur confidentielle d'extraction d'image et ne peut pas extraire des images des domaines de registre `icr.io` par défaut. Vous devez [créer votre propre valeur confidentielle d'extraction d'image](#other_registry_accounts), mais en utilisant une clé d'API pour un utilisateur individuel, tel qu'un ID fonctionnel, pas un ID de service {{site.data.keyword.Bluemix_notm}} IAM.

**La valeur confidentielle d'extraction d'image de mon cluster utilise un jeton de registre. Un jeton fonctionne-t-il encore ?**<br>

La méthode précédente qui consistait à autoriser l'accès d'un cluster à {{site.data.keyword.registrylong_notm}} par la création automatique d'un [jeton](/docs/services/Registry?topic=registry-registry_access#registry_tokens) et en stockant ce jeton dans une valeur confidentielle d'extraction d'image est prise en charge mais dépréciée.
{: deprecated}

Les jetons autorisent l'accès aux domaines de registre `registry.bluemix.net` obsolètes, tandis que les clés d'API autorisent l'accès aux domaines de registre `icr.io`. Durant la période de transition de l'authentification basée sur les jetons à l'authentification basée sur clés d'API, les jetons et les valeurs confidentielles d'extraction d'image basées sur les clés d'API sont créés pour un certain temps. Avec des jetons et des valeurs confidentielles d'extraction d'image basées sur des clés d'API, votre cluster peut extraire des images des domaines `registry.bluemix.net` ou `icr.io` dans l'espace de nom Kubernetes `default`.

Avant que les jetons obsolètes et les domaines `registry.bluemix.net` ne soient plus pris en charge, mettez à jour vos valeurs confidentielles d'extraction d'image de cluster qui utilisent la méthode de clé d'API pour l'[espace de nom Kubernetes `default`](#imagePullSecret_migrate_api_key) et [n'importe quels autres espaces de nom ou comptes](#other) que vous êtes susceptible d'utiliser. Ensuite, mettez à jour vos déploiements pour effectuer des extractions à partir des domaines de registre `icr.io`.

**Une fois que j'ai copié ou créé une valeur confidentielle d'extraction d'image dans un autre espace de nom Kubernetes, ai-je terminé ?**<br>
Pas tout à fait. Vos conteneurs doivent être autorisés à extraire des images à l'aide de la valeur confidentielle que vous avez créée. Vous pouvez ajouter la valeur confidentielle d'extraction d'image au compte de service pour l'espace de nom ou faire référence à la valeur confidentielle dans chaque déploiement. Pour obtenir des instructions, voir [Utilisation de valeur confidentielle d'extraction d'image pour déployer des conteneurs](/docs/containers?topic=containers-images#use_imagePullSecret).

<br />


## Mise à jour de clusters existants pour utiliser la valeur confidentielle d'extraction d'image de la clé d'API
{: #imagePullSecret_migrate_api_key}

Les nouveaux clusters {{site.data.keyword.containerlong_notm}} stockent une clé d'API dans une [valeur confidentielle d'extraction d'image pour autoriser l'accès à {{site.data.keyword.registrylong_notm}}](#cluster_registry_auth). Avec ces valeurs confidentielles d'extraction d'image, vous pouvez déployer des conteneurs à partir d'images stockées dans les domaines de registre `icr.io`. Pour les clusters créés avant le **25 février 2019**, vous devez mettre à jour votre cluster pour stocker une clé d'API au lieu d'un jeton de registre dans la valeur confidentielle d'extraction d'image.
{: shortdesc}

**Avant de commencer** :
*   [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*   Vérifiez que vous disposez des droits suivants :
    *   Rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM **Opérateur ou Administrateur** pour {{site.data.keyword.containerlong_notm}}. Le propriétaire de compte peut vous accorder le rôle en exécutant la commande suivante :
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name containers-kubernetes --roles Administrator,Operator
        ```
        {: pre}
    *   Rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM **Administrateur** pour {{site.data.keyword.registrylong_notm}}, sur toutes les régions et tous les groupes de ressources. Le propriétaire de compte peut vous accorder le rôle en exécutant la commande suivante :
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
        ```
        {: pre}

**Pour mettre à jour votre valeur confidentielle d'extraction d'image dans l'espace de nom Kubernetes `default`** :
1.  Obtenez l'ID de votre cluster.
    ```
    ibmcloud ks clusters
    ```
    {: pre}
2.  Exécutez la commande suivante pour créer un ID de service pour le cluster, affecter l'ID de service à un rôle de service IAM **Lecteur** pour {{site.data.keyword.registrylong_notm}}, créer une clé d'API pour représenter les données d'identification de l'ID de service et stocker la clé d'API dans une valeur confidentielle d'extraction d'image Kubernetes dans le cluster. Cette valeur confidentielle d'extraction d'image se trouve dans l'espace de nom Kubernetes `default`.
    ```
    ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Lorsque vous exécutez cette commande, la création de données d'identification IAM et de valeurs confidentielles d'extraction d'image est initiée et peut prendre un certain temps. Vous ne pouvez pas déployer de conteneurs qui extraient une image des domaines {{site.data.keyword.registrylong_notm}} `icr.io` tant que les valeurs confidentielles d'extraction d'image ne sont pas créées.
    {: important}

3.  Vérifiez que les valeurs confidentielles d'extraction d'image sont créées dans votre cluster. Notez que vous disposez de valeurs confidentielles distinctes pour chaque région {{site.data.keyword.registrylong_notm}}.
    ```
    kubectl get secrets
    ```
    {: pre}
    Exemple de sortie :
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
4.  Mettez à jour vos déploiements de conteneur pour extraire des images du nom de domaine `icr.io`.
5.  Facultatif : si vous disposez d'un pare-feu, veillez à [autoriser le trafic réseau sortant vers les sous-réseaux de registre](/docs/containers?topic=containers-firewall#firewall_outbound) pour les domaines que vous utilisez.

**Etape suivante ?**
*   Pour extraire des images dans d'autres espaces de nom Kubernetes que `default` ou à partir d'autres comptes {{site.data.keyword.Bluemix_notm}}, [copiez ou créez une autre valeur confidentielle d'extraction d'image](/docs/containers?topic=containers-images#other).
*   Pour limiter l'accès de la valeur confidentielle d'extraction d'image à des ressources de registre particulières, par exemple des espaces de nom ou des régions :
    1.  Vérifiez que les [règles {{site.data.keyword.Bluemix_notm}} IAM pour {{site.data.keyword.registrylong_notm}} sont activées](/docs/services/Registry?topic=registry-user#existing_users).
    2.  [Editez les règles {{site.data.keyword.Bluemix_notm}} IAM](/docs/iam?topic=iam-serviceidpolicy#access_edit) pour l'ID de service, ou [créez une autre valeur confidentielle d'extraction d'image](/docs/containers?topic=containers-images#other_registry_accounts).

<br />


## Utilisation d'une valeur confidentielle d'extraction d'image pour accéder à d'autres espaces de nom Kubernetes de cluster, d'autres comptes {{site.data.keyword.Bluemix_notm}} ou à des registres privés externes
{: #other}

Configurez votre propre valeur confidentielle d'extraction d'image pour déployer des conteneurs dans d'autres espaces de nom Kubernetes que `default`, utiliser des images stockées dans d'autres comptes {{site.data.keyword.Bluemix_notm}} ou utiliser des images stockées dans des registres privés externes. Par ailleurs, vous pouvez créer votre propre valeur confidentielle d'extraction d'image pour appliquer des règles d'accès IAM limitant les droits à des espaces de nom d'image de registre ou des actions (telles que `push` ou `pull`) spécifiques.
{:shortdesc}

Une fois que vous avez créé la valeur confidentielle d'extraction d'image, vous conteneurs doivent utiliser la valeur confidentielle pour être autorisés à extraire une image du registre. Vous pouvez ajouter la valeur confidentielle d'extraction d'image au compte de service pour l'espace de nom ou faire référence à la valeur confidentielle dans chaque déploiement. Pour obtenir des instructions, voir [Utilisation de valeur confidentielle d'extraction d'image pour déployer des conteneurs](/docs/containers?topic=containers-images#use_imagePullSecret).

Les valeurs confidentielles d'extraction d'image sont valides uniquement pour les espaces de nom Kubernetes pour lesquels elles ont été créées. Répétez ces étapes pour chaque espace de nom dans lequel vous désirez déployer des conteneurs. Les images de [DockerHub](#dockerhub) ne nécessitent pas de valeurs confidentielles d'extraction d'image.
{: tip}

Avant de commencer :

1.  [Configurez un espace de nom dans {{site.data.keyword.registryshort_notm}} et envoyez des images dans cet espace de nom](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2.  [Créez un cluster](/docs/containers?topic=containers-clusters#clusters_ui).
3.  Si vous utilisez un cluster créé avant le **25 février 2019**, [mettez à jour votre cluster pour utiliser la valeur confidentielle d'extraction d'image de clé d'API](#imagePullSecret_migrate_api_key).
4.  [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

<br/>
Pour utiliser votre propre valeur confidentielle d'extraction d'image, choisissez parmi les options suivantes :
- [Copier la valeur confidentielle d'extraction d'image](#copy_imagePullSecret) de l'espace de nom Kubernetes default dans d'autres espaces de nom de votre cluster.
- [Créer de nouvelles données d'identification de clé d'API IAM et les stocker dans une valeur confidentielle d'extraction d'image](#other_registry_accounts) pour accéder à des images dans d'autres comptes {{site.data.keyword.Bluemix_notm}} ou our appliquer des règles IAM limitant l'accès à certains domaines ou espaces de nom de registre.
- [Créer une valeur confidentielle d'extraction d'image pour accéder aux images dans des registres privés externes](#private_images).

<br/>
Si vous avez déjà créé une valeur confidentielle d'extraction d'image dans votre espace de nom et que vous voulez utiliser cette valeur dans votre déploiement, voir [Déploiement de conteneurs en utilisant la valeur confidentielle `imagePullSecret` créée](#use_imagePullSecret).

### Copie d'une valeur confidentielle d'extraction d'image
{: #copy_imagePullSecret}

Vous pouvez copier une valeur confidentielle d'extraction d'image, telle que celle qui est créée automatiquement pour l'espace de nom Kubernetes `default` dans d'autres espaces de nom de votre cluster. Si vous souhaitez utiliser d'autres données d'identification de clé d'API {{site.data.keyword.Bluemix_notm}} IAM pour cet espace de nom, par exemple, pour limiter l'accès à certains espaces de nom ou pour extraire des images d'autres comptes {{site.data.keyword.Bluemix_notm}}, [créez une valeur confidentielle d'extraction d'image](#other_registry_accounts) à la place.
{: shortdesc}

1.  Répertoriez les espaces de nom Kubernetes disponibles dans votre cluster ou créez un espace de nom à utiliser.
    ```
    kubectl get namespaces
    ```
    {: pre}

    Exemple de sortie :
    ```
    default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
    ```
    {: screen}

    Pour créer un espace de nom :
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  Répertoriez les valeurs confidentielles d'extraction d'image (secrets) dans les espaces de nom Kubernetes `default` pour {{site.data.keyword.registrylong_notm}}.
    ```
    kubectl get secrets -n default | grep icr
    ```
    {: pre}
    Exemple de sortie :
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
3.  Copiez chaque valeur confidentielle d'extraction d'image de l'espace de nom `default` vers les espaces de nom de votre choix. Les nouvelles valeurs confidentielles d'extraction d'image sont nommées `<namespace_name>-icr-<region>-io`.
    ```
    kubectl get secret default-us-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-uk-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-de-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-au-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-jp-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
4.  Vérifiez que la création des valeurs confidentielles a abouti.
    ```
    kubectl get secrets -n <namespace_name>
    ```
    {: pre}
5.  [Ajoutez une valeur confidentielle d'extraction d'image à un compte de service Kubernetes pour qu'un pod de l'espace de nom puisse utiliser cette valeur confidentielle lorsque vous déployez un conteneur](#use_imagePullSecret).

### Création d'une valeur confidentielle d'extraction d'image avec d'autres données d'identification de clé d'API IAM pour plus de contrôle ou pour accéder à des images dans d'autres comptes {{site.data.keyword.Bluemix_notm}}
{: #other_registry_accounts}

Vous pouvez affecter des règles d'accès {{site.data.keyword.Bluemix_notm}} IAM à des utilisateurs ou à un ID de service pour limiter les droits à des espaces de nom d'images de registre ou des actions (telles que `push` ou `pull`) spécifiques. Ensuite, créez une clé d'API et stockez ces données d'identification de registre dans une valeur confidentielle d'extraction d'image pour votre cluster.
{: shortdesc}

Par exemple, pour accéder à des images dans d'autres comptes {{site.data.keyword.Bluemix_notm}}, créez une clé d'API qui stocke les données d'identification {{site.data.keyword.registryshort_notm}} d'un utilisateur ou d'un ID de service dans ce compte. Ensuite, dans le compte de votre cluster, sauvegardez les données d'identification de la clé d'API dans une valeur confidentielle d'extraction d'image pour chaque cluster et espace de nom de cluster.

La procédure suivante permet de créer une clé d'API qui stocke les données d'identification d'un ID de service {{site.data.keyword.Bluemix_notm}} IAM. Au lieu d'utiliser un ID de service, vous envisagerez peut-être de créer une clé d'API pour un ID utilisateur disposant d'une règle d'accès au service {{site.data.keyword.Bluemix_notm}} IAM pour {{site.data.keyword.registryshort_notm}}. Cependant, assurez-vous que l'utilisateur correspond à un ID fonctionnel ou dispose d'un plan au cas où il partirait, afin que le cluster puisse toujours accéder au registre.
{: note}

1.  Répertoriez les espaces de nom Kubernetes disponibles dans votre cluster ou créez un espace de nom à utiliser pour y déployer les conteneurs à partir de vos images de registre.
    ```
    kubectl get namespaces
    ```
    {: pre}

    Exemple de sortie :
    ```
    default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
    ```
    {: screen}

    Pour créer un espace de nom :
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  Créez un ID de service {{site.data.keyword.Bluemix_notm}} IAM pour votre cluster qui sera utilisé pour les règles IAM et les données d'identification de clé d'API dans la valeur confidentielle d'extraction d'image. Veillez à indiquer une description pour l'ID de service, qui vous aidera à retrouver cet ID par la suite, par exemple en y incluant le nom du cluster et de l'espace de nom.
    ```
    ibmcloud iam service-id-create <cluster_name>-<kube_namespace>-id --description "Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
3.  Créez une règle {{site.data.keyword.Bluemix_notm}} IAM personnalisée pour l'ID de service de votre cluster qui autorise l'accès à {{site.data.keyword.registryshort_notm}}.
    ```
    ibmcloud iam service-policy-create <cluster_service_ID> --roles <service_access_role> --service-name container-registry [--region <IAM_region>] [--resource-type namespace --resource <registry_namespace>]
    ```
    {: pre}
    <table>
    <caption>Description des composantes de cette commande</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de cette commande</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster_service_ID&gt;</em></code></td>
    <td>Obligatoire. Remplacez cette valeur par l'ID de service `<cluster_name>-<kube_namespace>-id` que vous avez créé précédemment pour votre cluster Kubernetes.</td>
    </tr>
    <tr>
    <td><code>--service-name <em>container-registry</em></code></td>
    <td>Obligatoire. Entrez `container-registry` pour que la règle IAM s'applique à {{site.data.keyword.registrylong_notm}}.</td>
    </tr>
    <tr>
    <td><code>--roles <em>&lt;service_access_role&gt;</em></code></td>
    <td>Obligatoire. Entrez le [rôle d'accès au service pour {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-iam#service_access_roles) auquel vous voulez définir la portée d'accès de l'ID de service. Les valeurs possibles sont `Reader`, `Writer` et `Manager`.</td>
    </tr>
    <tr>
    <td><code>--region <em>&lt;IAM_region&gt;</em></code></td>
    <td>Facultatif. Pour définir la portée de la règle d'accès à certaines régions IAM, entrez les régions dans une liste en les séparant par des virgules. Les valeurs possibles sont `au-syd`, `eu-gb`, `eu-de`, `jp-tok`, `us-south` et `global`.</td>
    </tr>
    <tr>
    <td><code>--resource-type <em>namespace</em> --resource <em>&lt;registry_namespace&gt;</em></code></td>
    <td>Facultatif. Si vous souhaitez limiter l'accès uniquement aux images figurant dans certains [espaces de nom {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-registry_overview#registry_namespaces), entrez `namespace` comme type de ressource et spécifiez l'espace de nom du registre (`<registry_namespace>`). Pour répertorier les espaces de nom du registre, exécutez la commande `ibmcloud cr namespaces`.</td>
    </tr>
    </tbody></table>
4.  Créez une clé d'API pour l'ID de service. Nommez la clé d'API comme votre ID de service et incluez l'ID de service que vous avez créé précédemment, ``<cluster_name>-<kube_namespace>-id`. Veillez à indiquer une description pour la clé d'API pour vous aider à la retrouver par la suite.
    ```
    ibmcloud iam service-api-key-create <cluster_name>-<kube_namespace>-key <cluster_name>-<kube_namespace>-id --description "API key for service ID <service_id> in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
5.  Récupérez la valeur de votre **clé d'API** dans la sortie de la commande précédente.
    ```
    Conservez la clé d'API ! Elle ne peut plus être récupérée après avoir été créée.

    Name          <cluster_name>-<kube_namespace>-key   
    Description   key_for_registry_for_serviceid_for_kubernetes_cluster_multizone_namespace_test   
    Bound To      crn:v1:bluemix:public:iam-identity::a/1bb222bb2b33333ddd3d3333ee4ee444::serviceid:ServiceId-ff55555f-5fff-6666-g6g6-777777h7h7hh   
    Created At    2019-02-01T19:06+0000   
    API Key       i-8i88ii8jjjj9jjj99kkkkkkkkk_k9-llllll11mmm1   
    Locked        false   
    UUID          ApiKey-222nn2n2-o3o3-3o3o-4p44-oo444o44o4o4   
    ```
    {: screen}
6.  Créez une valeur confidentielle d'extraction d'image Kubernetes pour stocker les données d'identification de la clé d'API dans l'espace de nom du cluster. Répétez cette étape pour chaque domaine `icr.io`, espace de nom Kubernetes et cluster dont vous voulez extraire des images du registre avec les données d'identification IAM de cet ID de service.
    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name> --docker-server=<registry_URL> --docker-username=iamapikey --docker-password=<api_key_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Description des composantes de cette commande</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de cette commande</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obligatoire. Spécifiez l'espace de nom Kubernetes de votre cluster que vous avez utilisé pour le nom d'ID de service.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obligatoire. Entrez le nom de votre valeur confidentielle d'extraction d'image.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obligatoire. Définissez l'URL d'accès au registre d'images dans lequel est configuré votre espace de nom de registre. Domaines de registre disponibles :<ul>
    <li>Asie-Pacifique nord (Tokyo) : `jp.icr.io`</li>
    <li>Asie-Pacifique sud (Sydney) : `au.icr.io`</li>
    <li>Europe centrale (Francfort) : `de.icr.io`</li>
    <li>Sud du Royaume-Uni (Londres) : `uk.icr.io`</li>
    <li>Sud des Etats-Unis (Dallas) : `us.icr.io`</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username iamapikey</code></td>
    <td>Obligatoire. Entrez le nom d'utilisateur pour vous connecter à votre registre privé. Pour {{site.data.keyword.registryshort_notm}}, le nom d'utilisateur est défini par la valeur <strong><code>iamapikey</code></strong>.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obligatoire. Entrez la valeur de votre **clé d'API** que vous avez récupérée précédemment.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obligatoire. Si vous en avez une, entrez votre adresse e-mail Docker. A défaut, entrez une adresse e-mail fictive, par exemple `a@b.c`. Cette adresse e-mail est obligatoire pour créer une valeur confidentielle (secret) Kubernetes, mais n'est plus utilisée une fois la valeur créée.</td>
    </tr>
    </tbody></table>
7.  Vérifiez que la création de la valeur confidentielle a abouti. Remplacez <em>&lt;kubernetes_namespace&gt;</em> par l'espace de nom dans lequel vous avez créé la valeur confidentielle d'extraction d'image.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}
8.  [Ajoutez une valeur confidentielle d'extraction d'image à un compte de service Kubernetes pour qu'un pod de l'espace de nom puisse utiliser cette valeur confidentielle lorsque vous déployez un conteneur](#use_imagePullSecret).

### Accès aux images stockées dans d'autres registres privés
{: #private_images}

Si vous disposez déjà d'un registre privé, vous devez stocker les données d'identification du registre dans une valeur confidentielle d'extraction d'image Kubernetes et référencez cette valeur dans votre fichier de configuration.
{:shortdesc}

Avant de commencer :

1.  [Créez un cluster](/docs/containers?topic=containers-clusters#clusters_ui).
2.  [Ciblez votre interface CLI sur votre cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Pour créer une valeur confidentielle d'extraction d'image :

1.  Créez la valeur confidentielle Kubernetes pour stocker vos données d'identification de registre privé.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Description des composantes de cette commande</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de cette commande</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obligatoire. Espace de nom Kubernetes de votre cluster dans lequel vous désirez utiliser la valeur confidentielle et déployer des conteneurs. Exécutez la commande <code>kubectl get namespaces</code> pour répertorier tous les espaces de nom dans votre cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obligatoire. Nom que vous désirez utiliser pour l'élément <code>imagePullSecret</code>.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obligatoire. URL du registre dans lequel sont stockées vos images privées.</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Obligatoire. Nom d'utilisateur pour connexion à votre registre privé.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obligatoire. Valeur de votre jeton de registre extraite auparavant.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obligatoire. Si vous en avez une, entrez votre adresse e-mail Docker. A défaut, entrez une adresse e-mail fictive, par exemple `a@b.c`. Cette adresse e-mail est obligatoire pour créer une valeur confidentielle (secret) Kubernetes, mais n'est plus utilisée une fois la valeur créée.</td>
    </tr>
    </tbody></table>

2.  Vérifiez que la création de la valeur confidentielle a abouti. Remplacez <em>&lt;kubernetes_namespace&gt;</em> par le nom de l'espace de nom dans lequel vous avez créé la valeur confidentielle `imagePullSecret`.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [Créez un pod faisant référence à la valeur confidentielle d'extraction d'image](#use_imagePullSecret).

<br />


## Utilisation de valeur confidentielle d'extraction d'image pour déployer des conteneurs
{: #use_imagePullSecret}

Vous pouvez définir une valeur confidentielle d'extraction d'image dans votre déploiement de pod ou stocker la valeur confidentielle d'extraction d'image dans votre compte de service Kubernetes de sorte à ce qu'elle soit disponible pour tous les déploiements sans compte de service spécifié.
{: shortdesc}

Sélectionnez l'une des options suivantes :
* [Référencer la valeur confidentielle d'extraction d'image dans votre déploiement de pod](#pod_imagePullSecret) : utilisez cette option si, par défaut, vous ne souhaitez pas octroyer l'accès à votre registre pour tous les pods de votre espace de nom.
* [Stocker la valeur confidentielle d'extraction d'image dans le compte de service Kubernetes](#store_imagePullSecret) : utilisez cette option pour octroyer l'accès aux images de votre registre pour tous les déploiements dans les espaces de nom Kubernetes sélectionnés.

Avant de commencer :
* [Créez une valeur confidentielle d'extraction d'image](#other) pour accéder aux images dans d'autres registres ou dans d'autres espaces de nom Kubernetes que `default`.
* [Ciblez votre interface CLI sur votre cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

### Référencer la valeur confidentielle d'extraction d'image dans votre déploiement de pod
{: #pod_imagePullSecret}

Lorsque vous référencez la valeur confidentielle d'extraction d'image dans un déploiement de pod, cette valeur n'est valide que pour ce pod et ne peut pas être partagée entre les pods de cet espace de nom.
{:shortdesc}

1.  Créez un fichier de configuration de pod nommé `mypod.yaml`.
2.  Définissez le pod et la valeur confidentielle d'extraction d'image pour accéder aux images dans {{site.data.keyword.registrylong_notm}}.

    Pour accéder à une image privée :
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    Pour accéder à une image publique {{site.data.keyword.Bluemix_notm}} :
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: icr.io/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    <table>
    <caption>Description des composants du fichier YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>Nom du conteneur à déployer dans votre cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>Espace de nom sous lequel l'image est stockée. Pour répertorier les espaces de nom disponibles, exécutez la commande `ibmcloud cr namespace-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>Nom de l'image à utiliser. Pour répertorier les images disponibles dans un compte {{site.data.keyword.Bluemix_notm}}, exécutez la commande `ibmcloud cr image-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>Version de l'image que vous désirez utiliser. Si aucune étiquette n'est spécifiée, celle correspondant à <strong>latest</strong> (la plus récente) est utilisée par défaut.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Nom de la valeur confidentielle d'extraction d'image que vous avez créée précédemment.</td>
    </tr>
    </tbody></table>

3.  Sauvegardez vos modifications.
4.  Créez le déploiement dans votre cluster.
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### Stocker une valeur confidentielle d'extraction d'image dans le compte de service Kubernetes pour l'espace de nom sélectionné
{:#store_imagePullSecret}

Tous les espaces de nom ont un compte de service Kubernetes nommé `default`. Vous pouvez ajouter la  valeur confidentielle d'extraction d'image dans ce compte de service pour octroyer l'accès aux images de votre registre. Les déploiements pour lesquels aucun compte de service n'est spécifié utilisent automatiquement le compte de service `default` pour cet espace de nom.
{:shortdesc}

1. Vérifiez s'il existe déjà une valeur confidentielle d'extraction d'image pour le compte de service default.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   Lorsque `<none>` est affiché dans l'entrée **ImagePullSecrets**, il n'existe aucune valeur confidentielle d'extraction d'image.  
2. Ajoutez la  valeur confidentielle d'extraction d'image dans le compte de service default.
   - **Pour ajouter la  valeur confidentielle d'extraction d'image lorsqu'aucune valeur de ce type n'est définie :**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "<image_pull_secret_name>"}]}'
       ```
       {: pre}
   - **Pour ajouter la  valeur confidentielle d'extraction d'image lorsqu'une valeur de ce type est déjà définie :**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"<image_pull_secret_name>"}}]'
       ```
       {: pre}
3. Vérifiez que la  valeur confidentielle d'extraction d'image a été ajoutée dans le compte de service default.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   Exemple de sortie :
   ```
   Name:                default
   Namespace:           <namespace_name>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  <image_pull_secret_name>
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. Déployez un conteneur à partir d'une image dans votre registre.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <container_name>
         image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
   ```
   {: codeblock}

5. Créez le déploiement dans le cluster.
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


## Déprécié : Utilisation d'un jeton de registre pour déployer des conteneurs à partir d'une image {{site.data.keyword.registrylong_notm}}
{: #namespace_token}

Vous pouvez déployer dans votre cluster des conteneurs depuis une image fournie par IBM ou depuis une image privée stockée dans votre espace de nom dans {{site.data.keyword.registryshort_notm}}. Les clusters existants utilisent un [jeton](/docs/services/Registry?topic=registry-registry_access#registry_tokens) de registre stocké dans la valeur confidentielle d'extraction d'image `imagePullSecret` d'un cluster pour autoriser l'accès permettant d'extraire des images dans les noms de domaine `registry.bluemix.net`.
{:shortdesc}

Lorsque vous créez un cluster, des jetons de registre sans date d'expiration et des valeurs confidentielles sont créés automatiquement pour le [registre régional le plus proche, tout comme pour le registre global](/docs/services/Registry?topic=registry-registry_overview#registry_regions). Le registre global stocke de manière sécurisée des images publiques fournies par IBM auxquelles vous pouvez vous référer dans vos déploiements au lieu d'utiliser des références différentes pour les images stockées dans chaque registre régional. Le registre régional stocke vos propres images Docker privées de manière sécurisée. Les jetons sont utilisés pour autoriser un accès en lecture seule aux espaces de nom que vous avez configurés dans {{site.data.keyword.registryshort_notm}} pour pouvoir utiliser ces images publiques (registre global) et privées (registre régional).

Chaque jeton doit être stocké dans un élément Kubernetes `imagePullSecret` de sorte à être accessible à un cluster Kubernetes lorsque vous déployez une application conteneurisée. Lorsque votre cluster est créé, {{site.data.keyword.containerlong_notm}} stocke automatiquement les jetons pour le registre global (images publiques fournies par IBM) et pour les registres régionaux dans des valeurs confidentielles Kubernetes pour extraction d'images. Les valeurs confidentielles d'extraction d'image sont ajoutées à l'espace de nom Kubernetes `default`, à l'espace de nom `kube-system` et la liste des valeurs confidentielles dans le compte de service `default` correspondant à ces espaces de nom.

Cette méthode consistant à utiliser un jeton pour autoriser l'accès du cluster à {{site.data.keyword.registrylong_notm}} est prise en charge pour les noms de domaine `registry.bluemix.net` mais elle est dépréciée. [Utilisez à la place la méthode de clé d'API](#cluster_registry_auth) pour autoriser l'accès au cluster aux nouveaux noms de domaine de registre `icr.io`.
{: deprecated}

En fonction de l'emplacement de l'image et du conteneur, vous devez déployer les conteneurs en suivant différentes étapes.
*   [Déployer un conteneur dans l'espace de nom Kubernetes `default` avec une image qui se trouve dans la même région que votre cluster](#token_default_namespace)
*   [Déployer un conteneur dans un autre espace de nom Kubernetes que `default`](#token_copy_imagePullSecret)
*   [Déployer un conteneur avec une image qui se trouve dans une autre région ou un autre compte {{site.data.keyword.Bluemix_notm}} que votre cluster](#token_other_regions_accounts)
*   [Déployer un conteneur avec une image provenant d'un registre privé qui n'est pas un registre IBM](#private_images)

Avec cette configuration initiale, vous pouvez déployer des conteneurs depuis n'importe quelle image disponible dans un espace de nom dans votre compte {{site.data.keyword.Bluemix_notm}} vers l'espace de nom nommé **default** de votre cluster. Pour déployer un conteneur dans d'autres espaces de nom de votre cluster ou utiliser une image stockée dans une autre région {{site.data.keyword.Bluemix_notm}} ou un autre compte {{site.data.keyword.Bluemix_notm}}, vous devez [créer votre propre valeur confidentielle d'extraction d'image pour votre cluster](#other).
{: note}

### Déprécié : Déployer des images vers les espaces de nom `default` avec un jeton de registre
{: #token_default_namespace}

Avec le jeton de registre stocké dans la valeur confidentielle d'extraction d'image, vous pouvez déployer un conteneur à partir de n'importe quelle image disponible dans votre registre régional {{site.data.keyword.registrylong_notm}} dans l'espace de nom **default** de votre cluster.
{: shortdesc}

Avant de commencer :
1. [Configurez un espace de nom dans {{site.data.keyword.registryshort_notm}} et envoyez des images dans cet espace de nom](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2. [Créez un cluster](/docs/containers?topic=containers-clusters#clusters_ui).
3. [Ciblez votre interface CLI sur votre cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Pour déployer un conteneur dans l'espace de nom **default** de votre cluster, créez un fichier de configuration.

1.  Créez un fichier de configuration de déploiement nommé `mydeployment.yaml`.
2.  Définissez le déploiement et l'image de votre espace de nom que vous désirez utiliser dans {{site.data.keyword.registryshort_notm}}.

    Pour utiliser une image privée d'un espace de nom dans {{site.data.keyword.registryshort_notm}} :

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app_name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - name: <app_name>
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **Astuce :** pour extraire vos informations d'espace de nom, exécutez la commande `ibmcloud cr namespace-list`.

3.  Créez le déploiement dans votre cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Astuce :** vous pouvez également déployer un fichier de configuration existant, tel que l'une des images publiques fournies par IBM. Cet exemple utilise l'image **ibmliberty** dans la région Sud des Etats-Unis.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}

### Déprécié : Copier la valeur confidentielle d'extraction d'image de l'espace de nom default dans d'autres espaces de nom de votre cluster
{: #token_copy_imagePullSecret}

Vous pouvez copier la valeur confidentielle d'extraction d'image contenant les données d'identification du jeton de registre, qui est créée automatiquement pour l'espace de nom Kubernetes `default` dans d'autres espaces de nom de votre cluster.
{: shortdesc}

1. Répertoriez les espaces de nom disponibles dans votre cluster.
   ```
   kubectl get namespaces
   ```
   {: pre}

   Exemple de sortie :
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. Facultatif : créez un espace de nom dans votre cluster.
   ```
   kubectl create namespace <namespace_name>
   ```
   {: pre}

3. Copiez les valeurs confidentielles d'extraction d'image de l'espace de nom `default` vers les espaces de nom de votre choix. Les nouvelles valeurs confidentielles d'extraction d'image sont nommées `bluemix-<namespace_name>-secret-regional` et `bluemix-<namespace_name>-secret-international`.
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  Vérifiez que la création des valeurs confidentielles a abouti.
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. [Déployez un conteneur à l'aide de la valeur confidentielle `imagePullSecret`](#use_imagePullSecret) dans votre espace de nom.


### Déprécié : Créer une valeur confidentielle d'extraction d'image basée sur un jeton pour accéder à des images dans d'autres régions et comptes {{site.data.keyword.Bluemix_notm}}
{: #token_other_regions_accounts}

Pour accéder à des images figurant dans d'autres régions ou comptes {{site.data.keyword.Bluemix_notm}}, vous devez créer un jeton de registre et sauvegarder vos données d'identification dans une valeur confidentielle d'extraction d'image.
{: shortdesc}

1.  Si vous ne possédez pas de jeton, [créez un jeton pour le registre auquel vous souhaitez accéder. ](/docs/services/Registry?topic=registry-registry_access#registry_tokens_create)
2.  Répertoriez les jetons dans votre compte {{site.data.keyword.Bluemix_notm}}.

    ```
    ibmcloud cr token-list
    ```
    {: pre}

3.  Notez l'ID du jeton que vous désirez utiliser.
4.  Extrayez la valeur de ce jeton. Remplacez <em>&lt;token_ID&gt;</em> par l'ID du jeton que vous avez récupéré à l'étape précédente.

    ```
    ibmcloud cr token-get <token_id>
    ```
    {: pre}

    La valeur de ce jeton est affichée dans la zone **Token** de la sortie de votre interface CLI.

5.  Créez la valeur confidentielle Kubernetes pour stocker votre information de jeton.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Description des composantes de cette commande</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de cette commande</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obligatoire. Espace de nom Kubernetes de votre cluster dans lequel vous désirez utiliser la valeur confidentielle et déployer des conteneurs. Exécutez la commande <code>kubectl get namespaces</code> pour répertorier tous les espaces de nom dans votre cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obligatoire. Nom que vous désirez utiliser comme valeur confidentielle d'extraction d'image .</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obligatoire. URL du registre d'images où votre espace de nom est configuré.<ul><li>Pour les espaces de nom configurés dans les régions Sud et Est des Etats-Unis  : <code>registry.ng.bluemix.net</code></li><li>Pour les espaces de nom configurés dans la région Sud du Royaume-Uni : <code>registry.eu-gb.bluemix.net</code></li><li>Pour les espaces de nom configurés dans la région Europe centrale (Francfort) : <code>registry.eu-de.bluemix.net</code></li><li>Pour les espaces de nom configurés en Australie (Sydney) : <code>registry.au-syd.bluemix.net</code></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Obligatoire. Nom d'utilisateur pour connexion à votre registre privé. Pour {{site.data.keyword.registryshort_notm}}, le nom d'utilisateur est défini avec <strong><code>token</code></strong>.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obligatoire. Valeur de votre jeton de registre extraite auparavant.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obligatoire. Si vous en avez une, entrez votre adresse e-mail Docker. A défaut, indiquez une adresse e-mail fictive (par exemple, a@b.c). Cette adresse e-mail est obligatoire pour créer une valeur confidentielle (secret) Kubernetes, mais n'est plus utilisée une fois la valeur créée.</td>
    </tr>
    </tbody></table>

6.  Vérifiez que la création de la valeur confidentielle a abouti. Remplacez <em>&lt;kubernetes_namespace&gt;</em> par l'espace de nom dans lequel vous avez créé la valeur confidentielle d'extraction d'image.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  [Déployez un conteneur à l'aide de la valeur confidentielle d'extraction d'image](#use_imagePullSecret) dans votre espace de nom.
