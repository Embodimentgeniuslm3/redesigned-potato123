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


# Cas d'utilisation d'{{site.data.keyword.Bluemix_notm}} dans l'administration
{: #cs_uc_gov}

Ces cas d'utilisation mettent en évidence comment les charges de travail sur {{site.data.keyword.containerlong_notm}} bénéficient du cloud public. Ces charges de travail sont isolées avec la fonction de calcul sécurisé, se trouvent dans des régions globales pour la souveraineté des données, utilisent l'apprentissage automatique de Watson au lieu de nouveau code et se connectent à des bases de données locales.
{: shortdesc}

## L'administration régionale améliore la collaboration et la rapidité d'exécution avec des développeurs de la communauté qui combinent des données publiques et privées
{: #uc_data_mashup}

Un responsable du programme d'ouverture des données de l'administration (Open-Government Data Program) doit partager des données publiques avec la communauté et le secteur privé, mais  ces données sont verrouillées au sein d'un système monolithique local.
{: shortdesc}

Pourquoi {{site.data.keyword.Bluemix_notm}} ? Avec {{site.data.keyword.containerlong_notm}}, ce responsable peut fournir la valeur de transformation de la combinaison de données publiques et privées. De la même manière, le service fournit la plateforme de cloud public pour restructurer et exposer des microservices à partir d'applications monolithiques sur site. Le cloud public permet également à l'administration et aux partenariats publics d'utiliser des services de cloud externes et des outils open source facilitant le travail collaboratif.

Technologies clés :    
* [Clusters adaptés aux différents besoins en matière de stockage, d'UC et de mémoire RAM](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Outils natifs DevOps, notamment des chaînes d'outils ouvertes dans {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [Accès aux données publiques avec {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about)
* [Services IBM Cloud Analytics prêts à l'emploi](https://www.ibm.com/cloud/analytics)

**L'administration améliore la collaboration et la rapidité d'exécution avec des développeurs de la communauté qui combinent des données publiques et privées**
* L'avenir réside dans un modèle "d'administration ouverte", mais cette agence régionale ne peut pas franchir le pas avec les systèmes locaux dont elle dispose.
* Elle souhaite faire preuve d'innovation en encourageant le codéveloppement entre le secteur privé, les citoyens et les agences publiques.
* Des groupes hétérogènes de développeurs issus de l'administration ou d'organisations privées ne disposent pas d'une plateforme open source unifiée dans laquelle ils pourraient facilement partager des API et des données.
* Les données administratives sont verrouillées dans des systèmes locaux qui ne facilitent pas l'accès au public.

**Solution**

L'évolution vers une administration ouverte doit s'appuyer sur un système performant assurant à la fois la résilience, la continuité des opérations et la sécurité. Dans leur quête d'innovation et de codéveloppement, les agences et les citoyens dépendent de sociétés informatiques, de prestataires de service et de sociétés responsables de l'infrastructure pour "protéger et servir".

Pour freiner la bureaucratie et transformer ses relations avec les administrés, l'administration s'est tournée vers des normes ouvertes pour construire une plateforme collaborative :

* DONNEES OUVERTES – Stockage des données permettant aux citoyens, aux agences gouvernementales et aux entreprises d'accéder aux données, de les partager et de les améliorer en toute liberté
* API OUVERTES – Plateforme de développement dans laquelle les API sont créées et réutilisées par tous les partenaires de la communauté
* INNOVATION OUVERTE – Ensemble de services cloud qui permettent aux développeurs de se brancher sur l'innovation au lieu de la coder manuellement

Pour commencer, l'administration utilise {{site.data.keyword.cos_full_notm}} pour stocker ses données publiques dans le cloud. Ce mode de stockage peut être utilisé et réutilisé librement, partagé par tout le monde et fonctionner uniquement selon un système d'attributions et de partages. Les données sensibles peuvent être assainies avant d'être intégrées dans le cloud. Parallèlement, les contrôles d'accès sont configurés de sorte à limiter le nouveau stockage de données dans le cloud, où la communauté peut démontrer la faisabilité de l'amélioration des données disponibles existantes.

L'étape suivante de l'administration dans ses partenariats publics et privés a consisté à mettre en place une économie des API hébergée dans {{site.data.keyword.apiconnect_long}}. Là, les développeurs de la communauté et des entreprises, peuvent rendre les données facilement accessibles sous forme d'API. Leur objectif est d'avoir des API REST accessibles au public pour permettre l'interopérabilité et accélérer l'intégration des applications. Ils utilisent IBM {{site.data.keyword.SecureGateway}} pour se reconnecter aux sources de données privées sur site.

Enfin, les applications basées sur ces API partagées sont hébergées dans {{site.data.keyword.containerlong_notm}}, où il est facile de faire tourner des clusters. Les développeurs de la communauté, du secteur privé ou de l'administration peuvent facilement collaborer pour créer des applications. En bref, les développeurs doivent se focaliser sur le code au lieu de gérer l'infrastructure. C'est pourquoi ils ont choisi {{site.data.keyword.containerlong_notm}} car IBM simplifie la gestion de l'infrastructure :
* Gestion du maître Kubernetes, de l'infrastructure sous forme de services (IaaS) et de composants fonctionnels, comme Ingress et les composants de stockage
* Gestion de l'état de santé et de la reprise des noeuds worker
* Offre de calcul à l'échelle mondiale de sorte que les développeurs n'aient plus à soutenir l'infrastructure dans les régions du monde où ils nécessitent que leurs charges de travail et leurs données soient hébergées

Transférer les charges de travail de calcul sur {{site.data.keyword.Bluemix_notm}} ne suffit pas. L'administration doit également passer par une transformation des processus et des méthodes. En adoptant les pratiques d'IBM Garage Method, le fournisseur peut implémenter un processus de livraison agile et itératif qui prend en charge les pratiques modernes de DevOps telles que l'intégration continue et la distribution continue (CI/CD).

Le processus CI/CD est en grande partie automatisé avec {{site.data.keyword.contdelivery_full}} dans le cloud. Le fournisseur peut définir des chaînes d'outils de flux de travaux pour préparer les images de conteneur, rechercher les vulnérabilités et déployer les images sur le cluster Kubernetes.

**Modèle de solution**

Les outils de calcul, de stockage et d'API à la demande s'exécutent dans le cloud public avec un accès sécurisé aux sources de données locales en entrée et en sortie.

Solution technique :
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cos_full_notm}} et {{site.data.keyword.cloudant}}
* {{site.data.keyword.apiconnect_long}}
* IBM {{site.data.keyword.SecureGateway}}
* {{site.data.keyword.contdelivery_full}}

**Etape 1 : Stockage des données dans le cloud**
* {{site.data.keyword.cos_full_notm}} fournit un stockage des données d'historique accessible à tous sur le cloud public.
* Utiliser {{site.data.keyword.cloudant}} avec les clés fournies par les développeurs pour mettre en cache les données dans le cloud.
* Utiliser IBM {{site.data.keyword.SecureGateway}} pour que les connexions aux bases de données locales existantes soient toujours sécurisées.

**Etape 2 : Accès aux données au moyen d'API**
* Utiliser {{site.data.keyword.apiconnect_long}} pour la plateforme d'économie des API. Les API permettent au secteur public et au secteur privé de combiner des données dans leurs applications.
* Créer des clusters pour les applications publiques et privées, axées sur les API.
* Structurer les applications sous forme d'ensemble de microservices coopératifs qui s'exécutent dans {{site.data.keyword.containerlong_notm}}, basé sur les zones fonctionnelles des applications et de leurs dépendances.
* Déployer les applications dans des conteneurs qui s'exécutent dans {{site.data.keyword.containerlong_notm}}. Les outils à haute disponibilité (HA) intégrés dans {{site.data.keyword.containerlong_notm}} équilibrent les charges de travail, en incluant la réparation spontanée et l'équilibrage de charge.
* Fournir des tableaux de bord DevOps normalisés via Kubernetes, des outils open source avec lesquels tous les types de développeurs sont familiarisés.

**Etape 3 : Innovation avec IBM Garage et les services de cloud**
* Adopter les pratiques de développement agile et itératif d'IBM Garage Method pour permettre la publication fréquente de fonctions, modules de correction et correctifs sans susciter d'indisponibilité.
* Que les développeurs appartiennent au secteur public ou privé, {{site.data.keyword.contdelivery_full}} les aide à mettre à disposition rapidement une chaîne d'outils intégrée, en utilisant des modèles pouvant être personnalisés et partagés.
* Une fois que les développeurs ont construit et testé leurs applications dans leurs clusters de développement et de test, ils utilisent les chaînes d'outils d'{{site.data.keyword.contdelivery_full}} pour déployer des applications dans les clusters de production.
* Avec les outils d'intelligence artificielle (AI), d'apprentissage automatique (Machine Learning) et d'apprentissage en profondeur (Deep Learning) de Watson disponibles dans le catalogue d'{{site.data.keyword.Bluemix_notm}}, les développeurs se concentrent sur les problèmes de domaine. Au lieu d'un code d'apprentissage automatique (ML) unique personnalisé, la logique ML est intégrée dans les applications avec des liaisons de service.

**Résultats**
* Normalement, les partenariats publics ou privés qui prennent du temps font désormais tourner des applications en quelques semaines au lieu de mois entiers. Ces partenariats de développement peuvent maintenant fournir des fonctions et des correctifs d'erreur à un rythme pouvant aller jusqu'à 10 fois par semaine.
* Le développement est accéléré lorsque tous les participants utilisent des outils open source connus, tels que Kubernetes. Les longues courbes d'apprentissage ne constituent plus un blocage.
* Les citoyens et le secteur privé bénéficient de la transparence des activités, des informations et des plans. Les citoyens sont ainsi intégrés aux processus, aux services et aux aides gouvernementales.
* Les partenariats publics et privés ont maîtrisé des tâches herculéennes, telles que le suivi du virus Zika, la distribution intelligente d'électricité et l'analyse des statistiques sur la criminalité, sans oublier la formation des nouveaux cadres universitaires.

## Un port public important sécurise les échanges de données du port et les manifestes des transports maritimes en liaison avec des organismes publics et privés
{: #uc_port}

Les responsables informatiques d'une compagnie maritime privée et le port administré par les autorités publiques doivent connecter, fournir la visibilité et échanger en toute sécurité des informations portuaires. Mais il n'existe aucun système unifié pour connecter les informations portuaires publiques et les manifestes de transport maritime privés.
{: shortdesc}

Pourquoi {{site.data.keyword.Bluemix_notm}} ?  {{site.data.keyword.containerlong_notm}} permet à l'administration et aux partenaires publics d'utiliser des services de cloud externes et des outils open source facilitant le travail collaboratif. Les conteneurs offrent une plateforme pouvant être partagée où les autorités portuaires et la compagnie maritime ont l'assurance que les informations partagées sont hébergées sur une plateforme sécurisée. Par ailleurs, cette plateforme évolue lorsqu'ils passent des petits systèmes de développement et de test à des systèmes de production beaucoup plus importants. Des chaînes d'outils ouvertes contribuent à accélérer le développement en automatisant la réalisation, les tests et les déploiements.

Technologies clés :    
* [Clusters adaptés aux différents besoins en matière de stockage, d'UC et de mémoire RAM](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Sécurité et isolement de conteneur](/docs/containers?topic=containers-security#security)
* [Outils natifs DevOps, notamment des chaînes d'outils ouvertes dans {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**Contexte : Un port sécurise les échanges de données du port et les manifestes des compagnies maritimes en liaison avec des organismes publics et privés**

* Des groupes hétérogènes de développeurs issus des autorités portuaires ou de la compagnie maritime ne disposent pas d'une plateforme open source unifiée dans laquelle ils pourraient collaborer, ce qui ralentit les déploiements des mises à jour et des fonctionnalités.
* Les développeurs sont disséminés dans le monde entier et confinés dans des cadres institutionnels, ce qui fait que les logiciels open source et PaaS (Platform as a Service) constituent la meilleure solution.
* Les questions de sécurité sont devenues une préoccupation principale qui vient s'ajouter aux difficultés de collaboration qui affectent les fonctions et les mises à jour des logiciels, en particulier lorsque les applications sont en production.
* Les données JIT (juste-à-temps) impliquent que les systèmes internationaux doivent être à haute disponibilité pour réduire les retards dus aux opérations de transit. Les plannings des terminaux de transport maritime sont hyper contrôlés et inflexibles dans certains cas. L'utilisation du Web ne cesse d'augmenter, donc un système instable peut conduire à une mauvaise expérience utilisateur.

**Solution**

Les autorités portuaires et la compagnie maritime développent en commun un système commercial unifié pour soumettre sous forme électronique les informations de conformité pour le dédouanement des marchandises et les navires une seule fois, au lieu de passer par l'intermédiaire de plusieurs agences. Les applications de manifeste et de douane peuvent rapidement partager le contenu d'une cargaison particulière et s'assurer que tous les documents sont transférés par voie électronique et traités par les agences du port.

Ils créent alors un partenariat dédié aux solutions commerciales :
* DECLARATIONS - Application permettant d'inclure les manifestes de transport maritime et de traiter les documents de déclaration des douanes standard, et signaler les articles hors norme à des fins d'enquête et d'application de la loi
* DROITS DE DOUANE – Application pour calculer les droits de douane, facturer les frais du transporteur maritime par voie électronique et recevoir des paiements numériques
* REGLEMENTATION – Application flexible et configurable qui alimente les deux applications précédentes avec les politiques et réglementations qui changent constamment et affectent le traitement des importations, des exportations et des droits de douane

Les développeurs ont commencé par déployer leurs applications dans des conteneurs avec {{site.data.keyword.containerlong_notm}}. Ils ont créé des clusters pour un environnement de développement partagé leur permettant de déployer rapidement des améliorations pour leurs applications. Les conteneurs permettent à chaque équipe de développement d'utiliser le langage qui leur convient.

La sécurité d'abord : les responsables informatiques ont choisi la fonction de calcul sécurisé pour bare metal pour l'hébergement des clusters. Avec la technologie bare metal pour {{site.data.keyword.containerlong_notm}}, les charges de travail sensibles des douanes disposent désormais d'un isolement standard mais dans le cadre de la flexibilité du cloud public. La technologie bare metal fournit une fonction de calcul sécurisé qui peut vérifier que le matériel sous-jacent ne fait pas l'objet de falsification.

Comme la compagnie maritime envisage également d'utiliser d'autres ports, la sécurité des applications est fondamentale. Les manifestes de transport maritime et les informations douanières sont des documents hautement confidentiels. Sur cette base sécuritaire, Vulnerability Advisor offre les fonctionnalités d'analyse suivantes :
* Analyses de vulnérabilité des images
* Analyses des règles basée sur la norme ISO 27k
* Analyses des conteneurs de production
* Analyses des packages pour identifier les logiciels malveillants

En même temps, {{site.data.keyword.iamlong}} aide à contrôler les personnes ayant le niveau d'accès aux ressources.

Les développeurs se concentrent sur les problèmes de domaine en utilisant des outils existants : au lieu d'écrire un code unique pour les opérations de consignation et de surveillance, ils l'intègrent dans les applications via une liaison de services {{site.data.keyword.Bluemix_notm}} aux clusters. Les développeurs sont également libérés des tâches de gestion d'infrastructure car IBM se charge de Kubernetes et des mises à niveau d'infrastructure, de la sécurité, etc.

**Modèle de solution**

Des kits de démarrage de calcul, de stockage et Node.js à la demande s'exécutent dans le cloud public avec un accès sécurisé aux données de transport maritime dans le monde entier, selon les besoins. Les calculs dans les clusters sont protégés contre toute falsification et isolés avec la technologie bare metal.  

Solution technique :
* {{site.data.keyword.containerlong_notm}} avec fonction de calcul sécurisé
* {{site.data.keyword.openwhisk}}
* {{site.data.keyword.cloudant}}
* IBM {{site.data.keyword.SecureGateway}}

**Etape 1 : Applications conteneurisées à l'aide de microservices**
* Utiliser le kit de démarrage de Node.js d'IBM pour démarrer le développement rapidement.
* Structurer les applications sous forme d'ensemble de microservices coopératifs qui s'exécutent dans {{site.data.keyword.containerlong_notm}} basé sur les zones fonctionnelles de l'application et de ses dépendances.
* Déployer les applications de manifeste et d'expédition dans des conteneurs qui s'exécutent dans {{site.data.keyword.containerlong_notm}}.
* Fournir des tableaux de bord DevOps normalisés via Kubernetes.
* Utiliser IBM {{site.data.keyword.SecureGateway}} pour que les connexions aux bases de données locales existantes soient toujours sécurisées.

**Etape 2 : Disponibilité globale garantie**
* Une fois que les développeurs ont construit et testé leurs applications dans leurs clusters de développement et de test, ils utilisent les chaînes d'outils d'{{site.data.keyword.contdelivery_full}} et Helm pour déployer des applications spécifiques à chaque pays sur des clusters dans le monde entier.
* Les charges de travail et les données peuvent alors respecter les réglementations régionales en vigueur.
* Les outils à haute disponibilité (HA) intégrés dans {{site.data.keyword.containerlong_notm}} équilibrent la charge de travail au sein de chaque région géographique, en incluant la réparation spontanée et l'équilibrage de charge.

**Etape 3 : Partage des données**
* {{site.data.keyword.cloudant}} est une base de données NoSQL moderne qui convient à toute une gamme de cas d'utilisation axés sur les données, du stockage et des requêtes de données de type clé-valeur aux documents complexes.
* Pour diminuer les requêtes adressées aux bases de données régionales, {{site.data.keyword.cloudant}} est utilisé pour mettre en cache les données de session de l'utilisateur couvrant plusieurs applications.
* Cette configuration améliore l'utilisation et les performances de l'application de front-end sur les différentes applications dans {{site.data.keyword.containershort}}.
* Alors que les applications de noeuds worker dans {{site.data.keyword.containerlong_notm}} analysent les données locales et stockent les résultats dans {{site.data.keyword.cloudant}}, {{site.data.keyword.openwhisk}} réagit à tout changement et assainit automatiquement les données sur les flux de données entrants.
* De la même manière, les notifications d'expédition dans une région peuvent être déclenchées via des transferts de données de sorte que tous les clients en aval puissent accéder aux nouvelles données.

**Résultats**
* Avec les kits de démarrage d'IBM, {{site.data.keyword.containerlong_notm}} et les outils d'{{site.data.keyword.contdelivery_full}}, les développeurs du monde entier peuvent travailler en collaboration avec différents organismes et administrations. Ils peuvent développer ensemble des applications personnalisées, avec des outils familiers et interopérables.
* Les microservices réduisent considérablement les délais de livraison des correctifs, des corrections de bogue et des nouvelles fonctions. Le développement initial est rapide et les mises à jour sont fréquentes et peuvent intervenir jusqu'à 10 fois par semaine.
* Les clients de la compagnie maritime et les autorités gouvernementales ont accès aux données de manifeste et peuvent partager les données douanières tout en se conformant à la réglementation locale en vigueur.
* La compagnie maritime tire parti de l'amélioration de la gestion logistique dans la chaîne d'approvisionnement : coûts réduit et délais de dédouanement plus rapides.
* 99 % des déclarations sont numériques et 90 % des importations sont traitées sans intervention humaine.
