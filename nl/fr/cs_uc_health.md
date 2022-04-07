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


# Cas d'utilisation d'{{site.data.keyword.cloud_notm}} dans le domaine de la santé
{: #cs_uc_health}

Ces cas d'utilisation mettent en évidence comment les charges de travail sur {{site.data.keyword.containerlong_notm}} bénéficient du cloud public. Elles disposent de calcul sécurisé sur du matériel bare metal, avec une rotation facile des clusters pour accélérer le développement, la migration depuis des machines virtuelles et le partage des données dans des bases de données cloud.
{: shortdesc}

## Un prestataire de soins de santé migre les charges de travail figurant sur des machines virtuelles inefficaces sur des conteneurs faciles à utiliser pour les systèmes de diagnostic et de gestion des patients
{: #uc_migrate}

Un responsable informatique travaillant pour un prestataire de soins de santé dispose de systèmes de diagnostic et de gestion des patients sur site. Ces systèmes passent par des cycles d'amélioration lents, ce qui peut conduire à des niveaux de services stagnants pour les patients.
{: shortdesc}

Pourquoi {{site.data.keyword.cloud_notm}} ? Pour améliorer le service aux patients, le prestataire envisage d'utiliser {{site.data.keyword.containerlong_notm}} et {{site.data.keyword.contdelivery_full}} afin de réduire les dépenses informatiques et accélérer le développement, le tout sur une plateforme sécurisée. Les systèmes SaaS (Software as a Service) à utilisation intensive du prestataire qui regroupent à la fois les systèmes d'enregistrements des patients et les applications de diagnostics métier, nécessitent des mises à jour fréquentes. Mais l'environnement sur site ne facilite pas un développement agile. Le prestataire souhaite également réduire les coûts de main d'oeuvre qui ne cessent d'augmenter par rapport à un budget de plus en plus réduit.

Technologies clés :
* [Clusters adaptés aux différents besoins en matière de stockage, d'UC et de mémoire RAM](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Mise à l'échelle horizontale](/docs/containers?topic=containers-app#highly_available_apps)
* [Sécurité et isolement de conteneur](/docs/containers?topic=containers-security#security)
* [Outils natifs DevOps, notamment des chaînes d'outils ouvertes dans {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)
* [Possibilité de connexion sans modifier le code d'application en utilisant {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-getting-started)

Pour commencer, les systèmes SaaS du prestataire sont conteneurisés et placés dans le cloud. A partir de cette première étape, le prestataire passe d'un matériel sursollicité dans un centre de données privé à un calcul personnalisé qui réduit les opérations informatiques et la maintenance, et économise l'énergie. Pour héberger les systèmes SaaS, des clusters Kubernetes sont facilement élaborés pour répondre aux besoins du prestataire en matière de stockage, UC et mémoire RAM. Un autre facteur permettant de réduire les coûts de personnel réside dans le fait qu'IBM gère Kubernetes, donc le prestataire peut se concentrer sur l'offre d'un meilleur service aux clients.

Un développement accéléré est un facteur de réussite pour le responsable informatique. Avec le passage au cloud public, les développeurs peuvent faire des expériences faciles avec le kit SDK Node.js, en intégrant les modifications dans les systèmes de développement et de test, réparties sur différents clusters. Ces insertions sont automatiques avec des chaînes d'outils ouvertes et {{site.data.keyword.contdelivery_full}}. Les mises à jour du système SaaS n'attendent plus dans des processus de construction lents et susceptibles de comporter des erreurs. Les développeurs peuvent fournir des mises à jour incrémentielles à leurs utilisateurs, tous les jours, voire plus fréquemment.  La consignation et la surveillance des systèmes SaaS, notamment pour connaître les interactions entre les rapports de front end et de back end des patients, sont rapidement intégrées dans le système. Les développeurs ne perdent plus de temps à construire des systèmes de consignation complexes, juste pour pouvoir identifier et résoudre les incidents des systèmes de production.

La sécurité avant tout : avec la technologie bare metal pour {{site.data.keyword.containerlong_notm}}, les charges de travail sensibles des patients disposent désormais d'un isolement standard mais dans le cadre de la flexibilité du cloud public. La technologie bare metal fournit une fonction de calcul sécurisé qui peut vérifier que le matériel sous-jacent ne fait pas l'objet de falsification. Sur cette base, Vulnerability Advisor offre des fonctionnalités d'analyse :
* Analyse de vulnérabilité des images
* Analyse des règles basée sur la norme ISO 27k
* Analyse des conteneurs de production
* Analyse des packages pour identifier les logiciels malveillants

La sécurisation des données des patients conduit à une meilleure satisfaction des patients.

**Contexte : Migration d'une charge de travail pour les prestataires de soins de santé**

* La dette technique, couplée à des cycles de mise en production longs, nuit aux systèmes de gestion et de diagnostic des patients essentiels du prestataire.
* Les applications de back-office et de front-office personnalisées du prestataire sont fournies sur site sur des images de machine virtuelle monolithiques.
* Les processus, Les méthodes et Les outils doivent être revus mais le prestataire ne sait pas très bien par où commencer.
* La dette technique ne cesse d'augmenter au lieu de diminuer, en raison de l'incapacité à diffuser des logiciels de qualité pour s'adapter aux exigences du marché.
* La sécurité est la préoccupation principale qui vient s'ajouter au poids de la distribution, ce qui contribue à augmenter les délais.
* Les budgets de dépenses d'investissement sont étroitement contrôlés et le service informatique a conscience qu'il n'a pas le budget ou le personnel nécessaire pour créer les environnements de test et de préproduction nécessaires avec les systèmes dont il dispose en interne.

**Modèle de solution**

Les services de calcul, de stockage et d'E-S à la demande s'exécutent dans le cloud public avec un accès sécurisé aux actifs d'entreprise sur site. Implémenter un processus CI/CD et d'autres éléments d'IBM Garage Method permet de réduire considérablement les cycles de livraison.

**Etape 1 : Sécurisation de la plateforme de calcul**
* Les applications qui gèrent les données ultra-sensibles des patients peuvent être redirigées pour être hébergées sur {{site.data.keyword.containerlong_notm}} qui s'exécute sur Bare Metal for Trusted Compute.
* La fonction de calcul sécurisé (Trusted Compute) peut vérifier que le matériel sous-jacent ne fait pas l'objet de falsification.
* Sur cette base, Vulnerability Advisor fournit des fonctionnalités d'analyse d'images, de règles, de conteneurs et de packages pour détecter les logiciels malveillants connus.
* Imposition uniforme d'une authentification gérée par des règles à vos services et API avec une simple annotation Ingress. Avec la sécurité déclarative, vous pouvez garantir l'authentification des utilisateurs et la validation de jeton en utilisant {{site.data.keyword.appid_short_notm}}.

**Etape 2 : Migration de type "lift-and-shift"**
* Migrer des images de machine virtuelle sur des images de conteneur qui s'exécutent sur {{site.data.keyword.containerlong_notm}} dans le cloud public.
* Fournir des méthodes et des tableaux de bord DevOps normalisés via Kubernetes.
* Activer la mise à l'échelle du calcul à la demande pour les charges de travail pour le traitement par lots et d'autres charges de travail back-office qui s'exécutent moins fréquemment.
* Utiliser {{site.data.keyword.SecureGatewayfull}} pour que les connexions soient toujours sécurisées sur le SGBD local.
* Les coûts liés à un centre de données privé ou aux dépenses locales sont considérablement réduits et remplacés par un modèle informatique à la demande qui évolue en fonction des exigences de la charge de travail.

**Etape 3 : Microservices et Garage Method**
* Réorganiser l'architecture des applications sous forme d'ensemble de microservices coopératifs. Cet ensemble s'exécute dans {{site.data.keyword.containerlong_notm}} qui est basé sur les zones fonctionnelles de l'application ayant le plus de problèmes de qualité.
* Utiliser {{site.data.keyword.cloudant}} avec les clés fournies par le client pour la mise en cache des données sur le cloud.
* Adopter des pratiques d'intégration et de distribution continues (CI/CD) de sorte que les développeurs créent une version et une édition de microservice en fonction de sa propre planification selon les besoins. {{site.data.keyword.contdelivery_full}} prévoit des chaînes d'outils de flux de travaux pour le processus CI/CD ainsi que la création d'images et l'analyse de vulnérabilité des images de conteneur.
* Adopter des pratiques de développement agile et itératif d'IBM Garage Method pour permettre la publication fréquente de nouvelles fonctions, nouveaux modules de correction et correctifs sans susciter d'indisponibilité.

**Solution technique**
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGatewayfull}}
* {{site.data.keyword.appid_short_notm}}

Pour les charges de travail les plus sensibles, les clusters peuvent être hébergés sur {{site.data.keyword.containerlong_notm}} for Bare Metal.  Il s'agit d'une plateforme de calcul sécurisé qui analyse automatiquement le matériel et le code d'exécution pour y détecter les vulnérabilités. En utilisant la technologie des conteneurs aux normes de l'industrie, les applications peuvent être redirigées dès le départ pour être rapidement hébergées sur {{site.data.keyword.containerlong_notm}} sans modifications d'architecture majeures. Cette délocalisation contribue à l'évolutivité immédiate.

Les applications peuvent être répliquées et mises à l'échelle à l'aide de règles définies et de l'orchestration automatique de Kubernetes. {{site.data.keyword.containerlong_notm}} fournit des ressources de calcul évolutives et les tableaux de bord DevOps associés pour créer, mettre à l'échelle et supprimer des applications et des services à la demande. En utilisant les objets de déploiement et d'exécution de Kubernetes, le prestataire peut surveiller et gérer les mises à niveau des applications en toute fiabilité.

{{site.data.keyword.SecureGatewayfull}} est utilisé pour créer un pipeline sécurisé vers les bases de données et les documents locaux pour les applications qui sont relocalisées pour s'exécuter dans {{site.data.keyword.containerlong_notm}}.

{{site.data.keyword.cloudant}} est une base de données NoSQL moderne qui convient à toute une gamme de cas d'utilisation axés sur les données, du stockage et des requêtes de données de type clé-valeur aux documents complexes. Pour diminuer les requêtes sur le SGBD de back-office, {{site.data.keyword.cloudant}} est utilisé pour mettre en cache les données de session de l'utilisateur couvrant plusieurs applications. Ces options améliorent l'utilisation et les performances de l'application de front end sur les différentes applications dans {{site.data.keyword.containerlong_notm}}.

Transférer les charges de travail de calcul sur {{site.data.keyword.cloud_notm}} ne suffit pas. Le prestataire doit également passer par une transformation des processus et des méthodes. En adoptant les pratiques d'IBM Garage Method, le prestataire peut implémenter un processus de livraison agile et itératif qui prend en charge les pratiques modernes de DevOps telles que l'intégration continue et la distribution continue (CI/CD).

Le processus CI/CD est en grande partie automatisé par le service de distribution continue d'IBM dans le cloud. Le prestataire peut définir des chaînes d'outils de flux de travaux pour préparer les images de conteneur, rechercher les vulnérabilités et déployer les images sur le cluster Kubernetes.

**Résultats**
* Faire passer les machines virtuelles monolithiques existantes sur des conteneurs hébergés dans le cloud constituait la première étape qui permettait au prestataire de faire des économies sur les coûts d'investissement et de se familiariser avec les pratiques modernes de DevOps.
* Réorganiser l'architecture des applications monolithiques sous forme d'ensemble de microservices à granularité fine a contribué à réduire considérablement le délai de livraison des correctifs, des corrections de bogue et des nouvelles fonctions.
* En parallèle, le prestataire a mis en place des itérations simples délimitées dans le temps pour maîtriser la dette technique existante.

## Un organisme de recherche à but non lucratif héberge des données sensibles alors qu'il augmente ses activités de recherche avec des partenaires
{: #uc_research}

Un responsable du développement au sein d'un organisme de recherche à but non lucratif sur les maladies dispose de chercheurs universitaires et professionnels qui ne peuvent pas partager facilement leurs données de recherche. Leur travail reste isolé dans plusieurs parties du globe en raison des règles de conformité régionales et des bases de données centralisées.
{: shortdesc}

Pourquoi {{site.data.keyword.cloud_notm}} ? {{site.data.keyword.containerlong_notm}} offre un calcul sécurisé permettant de traiter des données sensibles et performantes sur une plateforme ouverte. Cette plateforme globale est hébergée dans des régions situées à proximité. Donc elle est liée à des réglementations locales qui inspirent confiance aux patients et aux chercheurs quant à la protection de leurs données en local et fait la différence en obtenant des meilleurs résultats sur la santé.

Technologies clés :
* [Planification intelligente pour placer les charges de travail là où c'est nécessaire](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [{{site.data.keyword.cloudant}} pour conserver et synchroniser les données sur plusieurs applications](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)
* [Analyse de vulnérabilité et isolement des charges de travail](/docs/services/Registry?topic=va-va_index#va_index)
* [Outils natifs DevOps, notamment des chaînes d'outils ouvertes dans {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [{{site.data.keyword.openwhisk}} pour assainir les données et envoyer des notifications aux chercheurs en cas de changement de structure des données](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant#openwhisk_cloudant)

**Contexte : Hébergement et partage des données sur les maladies de manière sécurisée pour un organisme de recherche à but non lucratif**

* Différents groupes de chercheurs issus d'institutions distinctes n'ont pas de méthode unifiée de partage de données, ce qui freine leur collaboration.
* Les questions de sécurité s'ajoutent aux difficultés de collaboration avec pour conséquence des recherches beaucoup moins partagées.
* Les développeurs et les chercheurs sont disséminés dans le monde entier et confinés dans des cadres institutionnels, ce qui fait que PaaS (Platform as a Service) et SaaS (Software as a Service) sont les meilleures options pour chaque groupe d'utilisateurs.
* Les disparités régionales en terme de réglementation sanitaire nécessitent que certaines données et que le traitement des données ne sortent pas de cette région.

**Solution**

L'organisme de recherche à but non lucratif veut regrouper les données de recherche sur le cancer recueillies dans le monde entier. Donc il procède à la création d'une division dédiée aux solutions pour leurs chercheurs :
* INGESTION - Applications pour l'acquisition des données de recherche. Les chercheurs utilisent aujourd'hui des feuilles de calcul, des documents, des produits commerciaux et des bases de données exclusives ou locales pour enregistrer les résultats de recherche. Cette situation n'est pas près de changer avec la tentative de l'organisme à but non lucratif de centraliser l'analyse des données.
* ANONYMISATION - Applications permettant de rendre les données anonymes. L'index des stratégies de sécurité (SPI) doit être supprimé pour se conformer à la réglementation sanitaire régionale.
* ANALYSE - Applications d'analyse des données. Le modèle de base consiste à stocker les données dans un format standard, puis de les interroger et de les traiter à l'aide des technologies d'intelligence artificielle (IA) et d'apprentissage automatique (ML), de régressions simples, et ainsi de suite.

Les chercheurs doivent s'affilier à un cluster régional et les applications doivent acquérir et transformer les données et les rendre anonymes :
1. Synchronisation des données rendues anonymes sur des clusters régionaux ou livraison à un magasin de données centralisé
2. Traitement des données à l'aide d'outils d'apprentissage automatique tel que PyTorch sur des noeuds worker bare metal fournissant des processeurs graphiques (GPU)

**INGESTION** {{site.data.keyword.cloudant}} est utilisé au niveau de chaque cluster régional qui stocke les documents de données enrichies des chercheurs et peut être interrogé et traité selon les besoins. {{site.data.keyword.cloudant}} chiffre les données au repos et en transit, ce qui est conforme aux lois régionales de confidentialité des données.

{{site.data.keyword.openwhisk}} est utilisé pour créer des fonctions de traitement permettant d'acquérir des données de recherche et de les stocker sous forme de documents de données structurées dans {{site.data.keyword.cloudant}}. {{site.data.keyword.SecureGatewayfull}} fournit à {{site.data.keyword.openwhisk}} un moyen facile d'accéder à des données locales de manière sûre et en toute sécurité.

Les applications Web dans les clusters régionaux sont développées en nodeJS pour la saisie manuelle des données de résultats, de définition de schéma et d'affiliation des organismes de recherche. IBM Key Protect vous aide à sécuriser l'accès aux données {{site.data.keyword.cloudant}} et IBM Vulnerability Advisor analyse les conteneurs d'applications et les images à des fins de sécurité.

**ANONYMISATION** Chaque fois qu'un nouveau document de données est stocké dans {{site.data.keyword.cloudant}}, un événement est déclenché et une fonction de cloud rend les données anonymes et supprime SPI du document de données. Ces documents de données anonymisés sont stockés indépendamment des données "brutes" acquises et sont les seuls documents qui sont partagés entre les régions à des fins d'analyse.

**ANALYSE** Les infrastructures d'apprentissage automatique sont à calculs hyper intensifs et l'organisme à but non lucratif configure un cluster de traitement global de noeuds worker bare metal. Associé à ce cluster de traitement global figure une base de données {{site.data.keyword.cloudant}} agrégée de données anonymisées. Un travail cron déclenche à intervalles réguliers une fonction de cloud pour transférer des documents de données anonymisés depuis les centres régionaux vers l'instance {{site.data.keyword.cloudant}} du cluster de traitement global.

Le cluster de calcul exécute l'infrastructure d'apprentissage automatique PyTorch et les applications d'apprentissage automatique sont écrites en Python pour analyser les données agrégées. En plus des applications d'apprentissage automatique, les chercheurs dans le groupe collectif développent également leurs propres applications qui peuvent être publiées et exécutées sur le cluster global.

L'organisme à but non lucratif fournit également des applications qui s'exécutent sur des noeuds autres que bare metal du cluster global. Les applications affichent et extraient les données agrégées et la sortie de l'application d'apprentissage automatique. Ces applications sont accessibles via un noeud final public, sécurisé par la passerelle d'API dans le monde entier. Ensuite, les chercheurs et les analystes de données quelle que soit leur localisation peuvent télécharger des ensembles de données et effectuer leurs propres analyses.

**Hébergement des charges de travail de recherche sur {{site.data.keyword.containerlong_notm}}**

Les développeurs ont commencé par déployer leurs applications SaaS de partage des recherches dans des conteneurs avec {{site.data.keyword.containerlong_notm}}. Ils ont créé des clusters pour un environnement de développement permettant aux développeurs du monde entier de déployer conjointement des améliorations d'application rapides.

La sécurité d'abord : le responsable du développement a choisi la fonction de calcul sécurisé pour bare metal pour l'hébergement des clusters de recherche. Avec la technologie bare metal pour {{site.data.keyword.containerlong_notm}}, les charges de travail de recherche sensibles disposent désormais d'un isolement standard mais dans le cadre de la flexibilité du cloud public. La technologie bare metal fournit une fonction de calcul sécurisé qui peut vérifier que le matériel sous-jacent ne fait pas l'objet de falsification. Comme cet organisme à but non lucratif est en partenariat avec des laboratoires pharmaceutiques, la sécurité des applications est fondamentale. La concurrence est féroce et l'espionnage industriel est possible. Sur cette base sécuritaire, Vulnerability Advisor offre les fonctionnalités d'analyse suivantes :
* Analyse de vulnérabilité des images
* Analyse des règles basée sur la norme ISO 27k
* Analyse des conteneurs de production
* Analyse des packages pour identifier les logiciels malveillants

Des applications de recherche sécurisées conduisent à une participation accrue aux essais cliniques.

Pour atteindre une disponibilité globale, les systèmes de développement, de test et de production sont déployés dans le monde entier dans plusieurs centres de données. Pour garantir la haute disponibilité, ils utilisent une combinaison de clusters dans plusieurs régions géographiques ainsi que des clusters à zones multiples. Ils peuvent facilement déployer l'application de recherche sur les clusters de Francfort pour se conformer à la réglementation européenne locale. Ils déploient également l'application sur les clusters des Etats-Unis pour en assurer la disponibilité et la reprise localement. Ils répartissent également la charge de travail de recherche entre les clusters à zones multiples à Francfort pour garantir que l'application européenne est disponible et équilibrent la charge de travail de manière efficace. Comme les chercheurs transfèrent des données sensibles avec leur application de partage des recherches, les clusters de l'application sont hébergés dans des régions où des réglementations plus strictes sont en vigueur.

Les développeurs se concentrent sur les problèmes de domaine, en utilisant des outils existants : au lieu d'écrire un code d'apprentissage automatique (ML) unique, la logique ML est intégrée dans les applications par liaison de services {{site.data.keyword.cloud_notm}} à des clusters. Les développeurs sont également libérés des tâches de gestion d'infrastructure car IBM se charge de Kubernetes et des mises à niveau d'infrastructure, de la sécurité, etc.

**Solution**

Des kits de démarrage de calcul, de stockage et Node.js à la demande s'exécutent dans le cloud public avec un accès sécurisé aux données de recherche dans le monde entier, selon les besoins. Les calculs dans les clusters sont protégés contre toute falsification et isolés avec la technologie bare metal.

Solution technique :
* {{site.data.keyword.containerlong_notm}} avec fonction de calcul sécurisé
* {{site.data.keyword.openwhisk}}
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGatewayfull}}

**Etape 1 : Applications conteneurisées à l'aide de microservices**
* Utiliser le kit de démarrage Node.js d'IBM pour démarrer le développement rapidement.
* Structurer les applications sous forme d'ensemble de microservices coopératifs dans {{site.data.keyword.containerlong_notm}} basé sur les zones fonctionnelles de l'application et de ses dépendances.
* Déployer les applications de recherche dans des conteneurs dans {{site.data.keyword.containerlong_notm}}.
* Fournir des tableaux de bord DevOps normalisés via Kubernetes.
* Activer la mise à l'échelle du calcul à la demande pour le traitement par lots et d'autres charges de travail de recherche qui s'exécutent moins fréquemment.
* Utiliser {{site.data.keyword.SecureGatewayfull}} pour que les connexions aux bases de données locales existantes soient toujours sécurisées.

**Etape 2 : Utilisation de calculs sécurisés et performants**
* Les applications d'apprentissage automatique qui nécessitent du calcul à très hautes performances sont hébergées sur {{site.data.keyword.containerlong_notm}} sur des machines bare metal. Ce cluster d'apprentissage automatique est centralisé, de sorte que chaque cluster régional n'ait pas les frais de noeuds worker bare metal ; les déploiements Kubernetes sont également plus faciles.
* Les applications qui traitent les données cliniques sensibles peuvent être hébergées sur {{site.data.keyword.containerlong_notm}} sur Bare Metal for Trusted Compute.
* La fonction de calcul sécurisé (Trusted Compute) peut vérifier que le matériel sous-jacent ne fait pas l'objet de falsification. Sur cette base, Vulnerability Advisor fournit des fonctionnalités d'analyse d'images, de règles, de conteneurs et de packages pour détecter les logiciels malveillants connus.

**Etape 3 : Disponibilité globale garantie**
* Une fois que les développeurs ont construit et testé les applications dans leurs clusters de développement et de test, ils utilisent des chaînes d'outils IBM CI/CD pour déployer des applications dans des clusters à l'échelle mondiale.
* Les outils à haute disponibilité (HA) intégrés dans {{site.data.keyword.containerlong_notm}} équilibrent la charge de travail au sein de chaque région géographique, en incluant la réparation spontanée et l'équilibrage de charge.
* Avec les chaînes d'outils et les outils de déploiement Helm, les applications sont également déployées dans des clusters à l'échelle mondiale, de sorte que les charges de travail et les données soient conformes aux réglementations régionales en vigueur.

**Etape 4 : Partage de données**
* {{site.data.keyword.cloudant}} est une base de données NoSQL moderne qui convient à toute une gamme de cas d'utilisation axés sur les données, du stockage et des requêtes de données de type clé-valeur aux documents complexes.
* Pour diminuer les requêtes adressées aux bases de données régionales, {{site.data.keyword.cloudant}} est utilisé pour mettre en cache les données de session de l'utilisateur couvrant plusieurs applications.
* Cette option améliore l'utilisation et les performances de l'application de front end sur les différentes applications dans {{site.data.keyword.containerlong_notm}}.
* Alors que les applications de noeuds worker dans {{site.data.keyword.containerlong_notm}} analysent les données locales et stockent les résultats dans {{site.data.keyword.cloudant}}, {{site.data.keyword.openwhisk}} réagit à tout changement et assainit automatiquement les données sur les flux de données entrants.
* De la même manière, les notifications indiquant une avancée majeure des recherches dans une région peuvent être déclenchées via des transferts de données de sorte que tous les chercheurs puissent tirer parti de ces nouvelles données.

**Résultats**
* Avec les kits de démarrage, {{site.data.keyword.containerlong_notm}} et les outils IBM CI/CD, les développeurs du monde entier peuvent travailler avec différentes institutions et développer ensemble des applications de recherche avec des outils familiers et interopérables.
* Les microservices réduisent considérablement les délais de livraison des correctifs, des corrections de bogue et des nouvelles fonctions. Le développement initial est rapide et les mises à jour sont fréquentes.
* Les chercheurs ont accès aux données cliniques et peuvent les partager, en conformité avec les réglementations locales.
* Les patients qui participent aux programmes de recherche sur les maladies sont assurés que leurs données sont sécurisées et donnent des résultats, lorsqu'elles sont partagées avec d'importantes équipes de recherche.
