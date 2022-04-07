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


# Cas d'utilisation d'{{site.data.keyword.cloud_notm}} dans la distribution
{: #cs_uc_retail}

Ces cas d'utilisation mettent en évidence comment les charges de travail sur {{site.data.keyword.containerlong_notm}} peuvent
tirer parti des fonctions d'analyse pour obtenir des connaissances sur le marché, les déploiements interrégionaux dans le monde entier et la gestion des stocks avec {{site.data.keyword.messagehub_full}} et le stockage d'objets.
{: shortdesc}

## Une enseigne partage des données à l'aide d'API avec des partenaires commerciaux mondiaux pour gérer les ventes omnicanal
{: #uc_data-share}

Un dirigeant dans ce secteur d'activité doit augmenter les canaux de distribution mais le système de distribution est confiné dans un centre de données local. La concurrence dispose de partenaires commerciaux mondiaux pour les ventes croisées et les permutations des ventes à plus haute valeur unitaire de leurs marchandises, dans les magasins et sur les sites en ligne.
{: shortdesc}

Pourquoi {{site.data.keyword.cloud_notm}} ? {{site.data.keyword.containerlong_notm}} offre un écosystème sur cloud public, dans lequel des conteneurs permettent à de nouveaux partenaires commerciaux et à d'autres acteurs externes de développer ensemble des applications et des données à l'aide d'API. Comme ce système de distribution est sur le cloud public, les API rationalisent le partage des données et relancent le développement de nouvelles applications. Les déploiements d'applications augmentent lorsque les développeurs expérimentent l'intégration facile et rapide de changements dans les systèmes de développement et de test avec des chaînes d'outils.

{{site.data.keyword.containerlong_notm}} et technologies clés :
* [Clusters adaptés aux différents besoins en matière de stockage, d'UC et de mémoire RAM](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [{{site.data.keyword.cos_full}} pour conserver et synchroniser les données sur plusieurs applications](/docs/tutorials?topic=solution-tutorials-pub-sub-object-storage#pub-sub-object-storage)
* [Outils natifs DevOps, notamment des chaînes d'outils ouvertes dans {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)

**Une enseigne partage des données à l'aide d'API avec des partenaires commerciaux mondiaux pour gérer les ventes omnicanal**

* L'enseigne doit faire face à une forte pression de la concurrence. Elle nécessite d'abord de masquer la complexité liée à l'introduction de nouveaux produits et de nouveaux canaux de distribution. Par exemple, elle a besoin de développer des produits sophistiqués. En même temps, elle nécessite une plus grande simplicité pour inciter les consommateurs à changer de marque.
* Cette capacité à changer de marque implique que l'écosystème de l'enseigne soit en connectivité avec des partenaires commerciaux. Le cloud peut ensuite apporter une valeur ajoutée aux partenaires commerciaux, aux consommateurs et à d'autres acteurs externes.
* Le lancement tous azimuts d'événements utilisateur, par exemple les soldes du Black Friday, est extrêmement contraignant pour les systèmes en ligne, poussant l'enseigne à surdimensionner l'infrastructure de calcul.
* Les développeurs de cette enseigne ont besoin de faire constamment évoluer les applications, mais les outils traditionnels ralentissent leur capacité à déployer fréquemment des mises à jour et des fonctions, notamment lors de leur collaboration avec les équipes des partenaires commerciaux.  

**Solution**

Une expérience d'achat plus intelligente est nécessaire pour fidéliser davantage de clients et augmenter la marge bénéficiaire brute. Le modèle de vente traditionnel de l'enseigne souffre de ne pas connaître le stock des partenaires commerciaux pour les ventes croisées et incitatives. Les clients recherchent plus de commodité et s'attendent à trouver rapidement les articles associés au même endroit, par exemple les pantalons et les tapis de yoga.

L'enseigne doit également fournir du contenu utile à ses clients, par exemple des informations sur les produits ou d'autres gammes de produit, des évaluations et une visibilité en temps réel du stock disponible. Et ces clients souhaitent en profiter en ligne et en magasin via leurs appareils mobiles personnels et les employés du magasin également équipés d'appareils mobiles.

La solution est constituée des composants principaux suivants :
* INVENTAIRE : application pour l'écosystème des partenaires commerciaux qui consolide et communique son stock, notamment lors de l'introduction de nouveaux produits, en incluant des API réutilisables par les partenaires commerciaux dans les applications B2B et de vente au détail
* VENTES CROISEES ET INCITATIVES : application qui met en avant les opportunités de ventes croisées et incitatives avec des API pouvant être utilisées dans différentes applications mobiles et d'e-commerce
* ENVIRONNEMENT DE DEVELOPPEMENT : les clusters Kubernetes pour les systèmes de développement, de test et de production augmentent la collaboration et le partage des données entre l'enseigne et ses partenaires commerciaux

Pour que l'enseigne puisse travailler avec des partenaires commerciaux mondiaux, les API de gestion de stock doivent être modifiées pour répondre aux préférences du marché et de langue propres à chaque région. {{site.data.keyword.containerlong_notm}} offre une couverture dans plusieurs régions, notamment en Amérique du Nord, en Europe, en Asie et en Australie, pour que les API puissent répercuter les besoins de chaque pays et assurer de faibles temps d'attente pour les appels d'API.

Une autre condition requise est de pouvoir partager les données d'inventaire avec les clients des partenaires commerciaux et de l'enseigne. Avec les API d'inventaire, les développeurs peuvent faire remonter les informations dans les applications, par exemple les applications mobiles d'inventaire ou les solutions Web d'e-commerce. Ils sont également chargés de construire et gérer le site d'e-commerce principal. En bref, ils doivent se focaliser sur le code au lieu de gérer l'infrastructure.

C'est pourquoi ils ont choisi {{site.data.keyword.containerlong_notm}} car IBM simplifie la gestion de l'infrastructure :
* Gestion du maître Kubernetes, de l'infrastructure sous forme de services (IaaS) et de composants fonctionnels, comme Ingress et les composants de stockage
* Gestion de l'état de santé et de la reprise des noeuds worker
* Calcul global, de sorte que les développeurs détiennent l'infrastructure matérielle dans les régions où leurs charges de travail et leurs données doivent résider

De plus, les fonctions de consignation et de surveillance pour les microservices d'API, notamment l'extraction de données personnelles de systèmes de back-end, s'intègrent facilement avec {{site.data.keyword.containerlong_notm}}. Les développeurs ne perdent plus de temps à construire des systèmes de consignation complexes, juste pour pouvoir identifier et résoudre les incidents des systèmes de production.

{{site.data.keyword.messagehub_full}} fait office de plateforme d'événements JIT (juste-à-temps) pour rapatrier les informations qui évoluent rapidement des systèmes d'inventaire des partenaires commerciaux sur {{site.data.keyword.cos_full}}.

**Modèle de solution**

Gestion du calcul, du stockage et des événements à la demande réalisée dans le cloud public avec accès au stock des partenaires dans le monde entier, selon les besoins

Solution technique :
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cos_full}}
* {{site.data.keyword.contdelivery_full}}
* IBM Cloud Logging and Monitoring
* {{site.data.keyword.appid_short_notm}}

**Etape 1 : Applications conteneurisées à l'aide de microservices**
* Structurer les applications sous forme d'ensemble de microservices coopératifs qui s'exécutent dans {{site.data.keyword.containerlong_notm}} basé sur les zones fonctionnelles de l'application et de ses dépendances.
* Déployer des applications dans des images de conteneur qui s'exécutent dans {{site.data.keyword.containerlong_notm}}.
* Fournir des tableaux de bord DevOps normalisés via Kubernetes.
* Activer la mise à l'échelle du calcul à la demande pour le traitement par lots et d'autres charges de travail d'inventaire qui s'exécutent moins fréquemment.

**Etape 2 : Disponibilité globale garantie**
* Les outils à haute disponibilité (HA) intégrés dans {{site.data.keyword.containerlong_notm}} équilibrent la charge de travail au sein de chaque région géographique, en incluant la réparation spontanée et l'équilibrage de charge.
* L'équilibrage de charge, les pare-feux et les systèmes DNS sont gérés par IBM Cloud Internet Services.
* Avec les chaînes d'outils et les outils de déploiement Helm, les applications sont également déployées dans des clusters à l'échelle mondiale, de sorte que les charges de travail et les données soient conformes aux réglementations régionales en vigueur, notamment pour tout ce qui relève de la personnalisation.

**Etape 3 : Conception des utilisateurs**
* {{site.data.keyword.appid_short_notm}} fournit des fonctions de connexion sans nécessiter de changement dans le code d'application.
* Une fois les utilisateurs connectés, vous pouvez utiliser {{site.data.keyword.appid_short_notm}} pour créer des profils et personnaliser l'expérience utilisateur de votre application.

**Etape 4 : Partage de données**
* {{site.data.keyword.cos_full}} associé à {{site.data.keyword.messagehub_full}} fournit un stockage de données d'historique en temps réel pour que les offres de ventes croisées correspondent au stock disponible chez les partenaires commerciaux.
* Les API permettent aux partenaires commerciaux de l'enseigne de partager des données dans leurs applications B2B et d'e-commerce.

**Etape 5 : Distribution continue**
* Le débogage d'API co-développées devient plus simple en ajoutant aux outils d'IBM Cloud Logging and Monitoring, des outils de cloud accessibles aux différents développeurs.
* {{site.data.keyword.contdelivery_full}} aide les développeurs à mettre en place rapidement une chaîne d'outils intégrée en utilisant des modèles pouvant être partagés et personnalisés avec les outils d'IBM, de tiers et des outils open source. Les compilations et les tests sont automatiques et le contrôle qualité est assuré par des fonctions d'analyse.
* Une fois que les développeurs ont construit et testé les applications dans leurs clusters de développement et de test, ils utilisent des chaînes d'outils IBM d'intégration continue et de distribution continue (CI/CD) pour déployer des applications dans des clusters à l'échelle mondiale.
* {{site.data.keyword.containerlong_notm}} facilite le transfert et la restauration d'applications. Des applications personnalisées sont déployées pour tester les campagnes via le routage intelligent et l'équilibrage de charge d'Istio.

**Résultats**
* Les microservices réduisent considérablement les délais de livraison des correctifs, des corrections de bogue et des nouvelles fonctions. Le développement initial à l'échelle mondiale est rapide et le rythme des mises à jour peut aller jusqu'à 40 fois par semaine.
* L'enseigne et ses partenaires commerciaux ont un accès immédiat au stock disponible et au calendrier de livraison, en utilisant des API.
* Avec {{site.data.keyword.containerlong_notm}} et les outils IBM d'intégration continue et de distribution continue (CI/CD), les versions A-B des applications sont prêtes à tester les campagnes.
* {{site.data.keyword.containerlong_notm}} fournit un calcul évolutif, de sorte que les charges de travail des API de ventes croisées et d'inventaire puissent augmenter lors des périodes de pointe de l'année, par exemple lors des vacances d'automne.

## Une épicerie traditionnelle parvient à augmenter la fréquentation des clients et les ventes en se tournant vers le numérique
{: #uc_grocer}

Un directeur du marketing doit parvenir à augmenter la fréquentation des clients dans les magasins de 20 % en ajoutant un atout majeur à sa chaîne de magasins. La concurrence des grandes enseignes et les commerces en ligne font baisser les ventes. Au même moment, le directeur du marketing décide de réduire le stock sans faire de démarque car la conservation de stock sur une longue durée bloque des millions en terme de capital.
{: shortdesc}

Pourquoi {{site.data.keyword.cloud_notm}} ? {{site.data.keyword.containerlong_notm}} facilite le lancement de calculs supplémentaires, ce qui permet aux développeurs d'ajouter rapidement des services Cloud Analytics pour connaître le comportement des ventes et l'adaptabilité au marché numérique.

Technologies clés :    
* [Mise à l'échelle horizontale pour accélérer le développement](/docs/containers?topic=containers-app#highly_available_apps)
* [Clusters adaptés aux différents besoins en matière de stockage, d'UC et de mémoire RAM](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Connaissances des tendances du marché avec Watson Discovery](https://www.ibm.com/watson/services/discovery/)
* [Outils natifs DevOps, notamment des chaînes d'outils ouvertes dans {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [Gestion des stocks avec {{site.data.keyword.messagehub_full}}](/docs/services/EventStreams?topic=eventstreams-about#about)

**Contexte : Une épicerie traditionnelle parvient à augmenter la fréquentation des clients et les ventes en se tournant vers le numérique**

* La pression de la concurrence des commerces en ligne et des grandes surfaces a déstabilisé les modèles d'épicerie classiques. Les ventes déclinent, comme en témoigne la baisse de fréquentation dans les magasins.
* Le programme de fidélité nécessite un coup de pouce avec une campagne moderne sur les bons de réduction imprimés lors du passage en caisse. Les développeurs doivent constamment faire évoluer les applications correspondantes, mais les outils traditionnels ralentissent leur capacité à déployer fréquemment des mises à jour et des fonctions.  
* Des stocks de marchandises de grande valeur ne bougent pas comme prévu, malgré la mode du "fooding" qui semble gagner du terrain sur les marchés des grandes métropoles.

**Solution**

L'épicier a besoin d'une application pour accélérer sa conversion et stimuler la fréquentation de son magasin afin de susciter de nouvelles ventes et mettre en place un programme de fidélisation des clients sur une plateforme d'analyse dans le cloud réutilisable. L'expérience ciblée en magasin peut être un événement avec un prestataire de service ou un vendeur pour fidéliser les clients ou en attirer de nouveaux, en fonction du succès de cet événement. Le magasin et le partenaire commercial offrent des avantages pour participer à l'événement et la possibilité d'acheter les produits dans le magasin ou auprès du partenaire commercial.  

Après l'événement, les clients sont orientés vers l'achat des produits nécessaires, et ils peuvent renouveler la démonstration eux-même par la suite. L'expérience client ciblée est mesurée avec des remboursements attractifs et l'inscription de nouveaux clients au programme de fidélité. La combinaison d'un événement marketing hyper-personnalisé et d'un outil pour assurer le suivi des achats en magasin peut faire en sorte que l'expérience ciblée soit suivie jusqu'à l'achat du produit. Toutes ces actions entraînent des conversions et une fréquentation plus élevée.

Prenons l'exemple d'un événement consistant à inviter un grand chef montrant comment réaliser une recette dans le magasin. Le magasin crée une motivation pour attirer du public. Par exemple, il offre un apéritif dans le restaurant du chef avec en plus la possibilité d'acheter les ingrédients nécessaires pour réaliser la recette (par exemple, une remise de 10 euros sur un panier d'achat de 100 euros).

La solution est constituée des composants principaux suivants :
1. ANALYSE D'INVENTAIRE : les événements en magasin (recettes, liste d'ingrédients et emplacement des produits) sont conçus pour épuiser le stock qui a du mal à s'écouler.
2. APPLICATION MOBILE DU PROGRAMME DE FIDELITE : cette application fournit le marketing ciblé avec des bons de réduction numériques, des listes de courses, les produits en stock (prix et disponibilité) sur une carte du magasin et le partage social.
3. SOCIAL MEDIA ANALYTICS fournit la personnalisation en détectant les produits préférés des clients en termes de tendances, gastronomie, chefs et ingrédients. Les analyses associent les tendances régionales à l'activité d'une personne sur Twitter, Pinterest et Instagram.
4. OUTILS DE DEVELOPPEMENT CONVIVIAUX : ces outils accélèrent le développement de fonctions et la correction d'erreurs.

Les systèmes de back-end de gestion des stocks pour l'inventaire des produits, le réapprovisionnement du magasin et la prévision des produits constituent une mine de renseignements, mais les fonctions d'analyse modernes peuvent libérer de nouvelles connaissances pour savoir comment écouler des produits haut de gamme. En combinant {{site.data.keyword.cloudant}} et IBM Streaming Analytics, le directeur du marketing peut trouver les ingrédients parfaits à faire correspondre à l'événement organisé dans le magasin pour attirer les clients.

{{site.data.keyword.messagehub_full}} fait office de plateforme d'événements JIT (juste-à-temps) pour rapatrier les informations qui évoluent rapidement dans les systèmes de gestion des stocks sur IBM Streaming Analytics.

Social Media Analytics avec Watson Discovery (connaissances liées à la personnalité et au ton) permet également d'introduire des tendances dans l'analyse d'inventaire pour améliorer la prévision des produits.

L'application mobile du programme de fidélité fournit des informations de personnalisation détaillées, notamment lorsque les consommateurs utilisent leurs fonctions de partage social, comme la publication de recettes.

En plus de l'application mobile, les développeurs sont chargés de construire et de gérer l'application du programme de fidélité existante qui est liée aux bons de réduction attribués au passage à la caisse. En bref, ils doivent se focaliser sur le code au lieu de gérer l'infrastructure. C'est pourquoi ils ont choisi {{site.data.keyword.containerlong_notm}} car IBM simplifie la gestion de l'infrastructure :
* Gestion du maître Kubernetes, de l'infrastructure sous forme de services (IaaS) et de composants fonctionnels, comme Ingress et les composants de stockage
* Gestion de l'état de santé et de la reprise des noeuds worker
* Calcul global pour que les développeurs ne soient plus chargés de la configuration de l'infrastructure dans les centres de données

**Modèle de solution**

Gestion du calcul, du stockage et des événements à la demande réalisée dans le cloud public avec accès aux systèmes ERP de back-end

Solution technique :
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cloudant}}
* IBM Streaming Analytics
* IBM Watson Discovery

**Etape 1 : Applications conteneurisées à l'aide de microservices**

* Structurer l'analyse des stocks et des applications mobiles sous forme de microservices et les déployer dans des conteneurs d'{{site.data.keyword.containerlong_notm}}.
* Fournir des tableaux de bord DevOps normalisés via Kubernetes.
* Effectuer une mise à l'échelle du calcul à la demande pour le traitement par lots et d'autres charges de travail de gestion des stocks qui s'exécutent moins fréquemment.

**Etape 2 : Analyse du stock et des tendances**
* {{site.data.keyword.messagehub_full}} fait office de plateforme d'événements JIT (juste-à-temps) pour rapatrier les informations qui évoluent rapidement dans les systèmes de gestion des stocks sur IBM Streaming Analytics.
* Les analyses de Social Media Analysis avec Watson Discovery et les données des systèmes de gestion des stocks sont intégrées avec IBM Streaming Analytics pour fournir des conseils en matière de marketing et de merchandising.

**Etape 3 : Promotions avec l'application mobile du programme de fidélité**
* Accélérer le développement d'application mobile avec le kit de démarrage et d'autres services d'IBM Mobile, tels qu'{{site.data.keyword.appid_full_notm}}.
* Les promotions sous forme de bons de réduction et d'autres prestations sont envoyées à l'application mobile des utilisateurs. Les promotions sont identifiées en utilisant l'analyse d'inventaire et l'analyse sociale avec d'autres systèmes de back-end.
* Le stockage des recettes en promotion sur l'application mobile et les conversions (remboursement de bons de réduction à la caisse) sont ensuite pris en compte dans les systèmes ERP pour effectuer d'autres analyses.

**Résultats**
* Avec {{site.data.keyword.containerlong_notm}}, les microservices réduisent considérablement les délais de livraison des correctifs, des corrections de bogue et des nouvelles fonctions. Le développement initial est rapide et les mises à jour sont fréquentes.
* La fréquentation des clients et les ventes ont augmenté dans les magasins devenus eux-mêmes un atout majeur de différenciation.
* En même temps, de nouvelles connaissances issues d'analyses sociales et cognitives ont contribué à réduire les dépenses d'exploitation (OpEx) liées aux stocks.
* Le partage social dans l'application mobile a également contribué à identifier et cibler de nouveaux clients.
