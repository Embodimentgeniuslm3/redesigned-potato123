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


# Cas d'utilisation d'{{site.data.keyword.cloud_notm}} dans le domaine des transports
{: #cs_uc_transport}

Ces cas d'utilisation mettent en évidence comment les charges de travail sur {{site.data.keyword.containerlong_notm}} peuvent
tirer parti des chaînes d'outils pour effectuer rapidement des mises à jour d'application et des déploiements interrégionaux dans le monde entier. En même temps, ces charges de travail peuvent se connecter aux systèmes de back-end existants, utiliser Watson AI pour la personnalisation et accéder aux données IoT avec {{site.data.keyword.messagehub_full}}.

{: shortdesc}

## Une compagnie maritime augmente la disponibilité des systèmes dans le monde entier pour son écosystème de partenaires commerciaux
{: #uc_shipping}

Un responsable informatique dispose de systèmes de routage et de planification de transport au niveau mondial, avec lesquels des partenaires peuvent interagir. Les partenaires nécessitent des informations à la minute près pour ces systèmes qui ont accès aux données de terminaux IoT. Mais ces systèmes n'étaient pas en mesure de s'adapter à l'échelle internationale avec une haute disponibilité suffisante.
{: shortdesc}

Pourquoi {{site.data.keyword.cloud_notm}} ? {{site.data.keyword.containerlong_notm}} adapte les applications conteneurisées en garantissant 99,999 % de disponibilité pour répondre aux exigences croissantes. Les déploiements d'application ont lieu 40 fois par jour lorsque les développeurs expérimentent l'application rapide et facile des changements dans des environnements de développement et de test. La plateforme IoT facilite l'accès aux données IoT.

Technologies clés :    
* [Ecosystème de partenaires commerciaux dans plusieurs régions ](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [Mise à l'échelle horizontale](/docs/containers?topic=containers-app#highly_available_apps)
* [Chaînes d'outils ouvertes dans {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [Services de cloud pour l'innovation](https://www.ibm.com/cloud/products/#analytics)
* [{{site.data.keyword.messagehub_full}} pour transférer des données d'événement dans les applications](/docs/services/EventStreams?topic=eventstreams-about#about)

**Contexte : Une compagnie maritime augmente la disponibilité des systèmes dans le monde entier pour son écosystème de partenaires commerciaux**

* Les différences régionales en terme de logistique des transports ne facilitent pas l'intégration d'un nombre croissant de partenaires dans plusieurs pays. Parmi les exemples figurent les réglementations particulières et la logistique de transit, nécessitant que la compagnie maintienne des enregistrements cohérents au passage des frontières.
* Les données JIT (juste-à-temps) impliquent une haute disponibilité des systèmes internationaux pour réduire les retards dus aux opérations de transit. Les plannings des terminaux de transport maritime sont hyper contrôlés et inflexibles dans certains cas. L'utilisation du Web ne cesse d'augmenter, donc un système instable peut conduire à une mauvaise expérience utilisateur.
* Les développeurs ont besoin de faire constamment évoluer les applications, mais les outils traditionnels ralentissent leur capacité à déployer fréquemment des mises à jour et des fonctions.  

**Solution**

La compagnie maritime doit assurer une certaine cohésion dans la gestion de ses plannings et inventaires, ainsi que pour le traitement des formalités douanières. Elle peut ensuite partager la localisation de la cargaison et son contenu, ainsi que le calendrier de livraison avec ses clients. Elle élimine toute approximation quant à l'arrivée d'une marchandise (par exemple un appareil, des vêtements ou un produit) de sorte que leurs clients puissent communiquer ces informations à leurs propres clients.

La solution est constituée des composants principaux suivants :
1. Diffusion des données des terminaux IoT en temps réel pour chaque conteneur expédié : manifestes et emplacement
2. Partage numérique des formalités douanières avec les ports et les partenaires de transit, y compris le contrôle d'accès
3. Application destinée aux clients de la compagnie qui permet d'agréger et de communiquer les informations d'arrivée des marchandises expédiées, y compris des API pour que ces clients puissent réutiliser les données d'expédition dans leurs propres applications et leurs applications B2B

Pour que la compagnie maritime soit en mesure de travailler avec des partenaires mondiaux, les systèmes de routage et de planification nécessitent des modifications locales pour s'adapter à la langue, aux réglementations de chaque région et à la logistique spécifique à chaque port. {{site.data.keyword.containerlong_notm}} offre une couverture globale dans plusieurs régions, notamment en Amérique du Nord, en Europe, en Asie et en Australie, pour que les applications répercutent les besoins de ses partenaires, dans chaque pays.

Les terminaux IoT envoient les données distribuées par {{site.data.keyword.messagehub_full}} aux applications des ports régionaux et aux magasins de données contenant les manifestes de douanes et de conteneurs associés. {{site.data.keyword.messagehub_full}} est le point d'arrivée des événements IoT. Les événements sont distribués en fonction de la connectivité gérée offerte par Watson IoT Platform pour {{site.data.keyword.messagehub_full}}.

Une fois que les événements sont dans {{site.data.keyword.messagehub_full}}, ils sont conservés pour être utilisés immédiatement dans les applications de transit des ports et à n'importe quel moment par la suite. Les applications qui nécessitent les temps d'attente les plus faibles bénéficient en temps réel des données du flux d'événements ({{site.data.keyword.messagehub_full}}). D'autres applications à venir, comme les outils d'analyse, peuvent choisir leur mode de consommation des données par lots à partir du magasin d'événements avec {{site.data.keyword.cos_full}}.

Comme les données d'expédition sont partagées avec les clients de la société, les développeurs s'assurent que ces clients peuvent utiliser des API pour faire remonter les données d'expédition dans leurs propres applications. Parmi les exemples d'applications de ce type figurent les applications mobiles de suivi ou les solutions Web d'e-commerce. Les développeurs sont également chargés de construire et gérer les applications des ports régionaux qui rassemblent et diffusent les enregistrements douaniers et les manifestes de transport maritime. En bref, ils doivent se focaliser sur le code au lieu de gérer l'infrastructure. C'est pourquoi ils ont choisi {{site.data.keyword.containerlong_notm}} car IBM simplifie la gestion de l'infrastructure :
* Gestion du maître Kubernetes, de l'infrastructure sous forme de services (IaaS) et de composants fonctionnels, comme Ingress et les composants de stockage
* Gestion de l'état de santé et de la reprise des noeuds worker
* Calcul global, pour que les développeurs ne soient plus chargés de l'infrastructure dans les différentes régions où doivent résider leurs charges de travail et leurs données.

Pour atteindre une disponibilité globale, les systèmes de développement, de test et de production ont été déployés dans le monde entier dans plusieurs centres de données. Pour garantir la haute disponibilité, ils utilisent une combinaison de plusieurs clusters répartis dans différentes régions géographiques ainsi que des clusters à zones multiples. Ils peuvent facilement déployer l'application du port pour répondre aux besoins métier :
* à Francfort avec des clusters permettant la conformité à la réglementation européenne locale
* aux Etats-Unis avec des clusters pour assurer la disponibilité locale et la reprise en cas d'incident

Ils ont également réparti la charge de travail entre des clusters à zones multiples à Francfort pour garantir que la version européenne de l'application est disponible et équilibre la charge de travail de manière efficace. Comme chaque région échange par téléchargement des données particulières avec l'application du port, les clusters de l'application sont hébergés dans les régions où les temps d'attente sont faibles.

Pour les développeurs, une grande partie du processus d'intégration continue et de distribution continue (CI/CD) peut être automatisée avec {{site.data.keyword.contdelivery_full}}. La compagnie peut définir des chaînes d'outils de flux de travaux pour préparer les images de conteneur, rechercher les vulnérabilités et déployer les images sur le cluster Kubernetes.

**Modèle de solution**

Gestion du calcul, du stockage et des événements à la demande réalisée dans le cloud public avec accès au données d'expédition dans le monde entier, selon les besoins

Solution technique :
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.messagehub_full}}
* {{site.data.keyword.cos_full}}
* {{site.data.keyword.contdelivery_full}}

**Etape 1 : Applications conteneurisées à l'aide de microservices**

* Intégrer des applications dans un ensemble de microservices coopératifs dans {{site.data.keyword.containerlong_notm}} selon les zones fonctionnelles de l'application et des dépendances associées.
* Déployer les applications dans des conteneurs dans {{site.data.keyword.containerlong_notm}}.
* Fournir des tableaux de bord DevOps normalisés via Kubernetes.
* Activer la mise à l'échelle du calcul à la demande pour le traitement par lots et d'autres charges de travail d'inventaire qui s'exécutent moins fréquemment.
* Utiliser {{site.data.keyword.messagehub_full}} pour gérer les flux de données des terminaux IoT.

**Etape 2 : Disponibilité globale garantie**
* Les outils à haute disponibilité (HA) intégrés dans {{site.data.keyword.containerlong_notm}} équilibrent la charge de travail au sein de chaque région géographique, en incluant la réparation spontanée et l'équilibrage de charge.
* L'équilibrage de charge, les pare-feux et les systèmes DNS sont gérés par IBM Cloud Internet Services.
* Avec les chaînes d'outils et les outils de déploiement Helm, les applications sont également déployées dans des clusters à l'échelle mondiale, de sorte que les charges de travail et les données soient conformes aux réglementations régionales en vigueur.

**Etape 3 : Partage de données**
* {{site.data.keyword.cos_full}} associé à {{site.data.keyword.messagehub_full}} fournit en temps réel un stockage des données chronologique.
* Les API permettent aux clients de la société de livraison de partager des données dans leurs applications.

**Etape 4 : Distribution en continu**
* {{site.data.keyword.contdelivery_full}} aide les développeurs à mettre en place rapidement une chaîne d'outils intégrée, en utilisant des modèles pouvant être partagés et personnalisés avec les outils d'IBM, de tiers et des outils open source. Les compilations et les tests sont automatiques et le contrôle qualité est assuré par des fonctions d'analyse.
* Une fois que les développeurs ont construit et testé les applications dans leurs clusters de développement et de test, ils utilisent des chaînes d'outils IBM CI/CD pour déployer des applications dans des clusters à l'échelle mondiale.
* {{site.data.keyword.containerlong_notm}} facilite le transfert et la restauration d'applications. Des applications personnalisées sont déployées pour répondre aux exigences régionales via le routage intelligent et l'équilibrage de charge d'Istio.

**Résultats**

* Avec {{site.data.keyword.containerlong_notm}} et les outils CI/CD d'IBM, les versions régionales des applications sont hébergées à proximité des unités physiques à partir desquelles elles collectent leurs données.
* Les microservices réduisent considérablement les délais de livraison des correctifs, des corrections de bogue et des nouvelles fonctions. Le développement initial est rapide et les mises à jour sont fréquentes.
* Les clients de la compagnie maritime ont un accès en temps réel aux emplacements de la cargaison, aux calendriers de livraison et même aux documents d'enregistrements approuvés des autorités portuaires.
* Les partenaires de transit dans les différents terminaux de transport sont conscient des manifestes et des détails d'expédition, ce qui améliore la logistique sur place et permet d'éviter les retards.
* Indépendamment de ce scénario, [Maersk et IBM ont formé une joint-venture](https://www.ibm.com/press/us/en/pressrelease/53602.wss) pour améliorer les chaînes d'approvisionnement internationales avec Blockchain.

## Une compagnie aérienne met en place un site de prestations en ressources humaines (RH) en moins de trois semaines
{: #uc_airline}

Un directeur des ressources humaines a besoin d'un nouveau site RH avec un agent conversationnel innovant, mais la plateforme utilisée et les outils de développement impliquent des délais relativement longs pour la mise en oeuvre des applications. Dans ce cas de figure, il y a beaucoup d'attente à la clé pour l'acquisition du matériel.
{: shortdesc}

Pourquoi {{site.data.keyword.cloud_notm}} ? {{site.data.keyword.containerlong_notm}} facilite le lancement des calculs. Les développeurs peuvent ensuite expérimenter facilement l'intégration rapide de changements dans des systèmes de développement et de test avec des chaînes d'outils ouvertes. Les outils de développement de logiciels traditionnels sont encore plus performants lorsqu'ils fonctionnent avec IBM Watson Assistant. Le nouveau site de prestations a pu voir le jour en moins de 3 semaines.

Technologies clés :    
* [Clusters adaptés aux différents besoins en matière de stockage, d'UC et de mémoire RAM](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Service d'un agent conversationnel optimisé par Watson](https://developer.ibm.com/code/patterns/create-cognitive-banking-chatbot/)
* [Outils natifs DevOps, notamment des chaînes d'outils ouvertes dans {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**Contexte : Construction et déploiement rapides d'un site de prestations RH en moins de 3 semaines**
* L'augmentation du nombre d'employés et les changements en matière de politiques RH indiquent qu'un nouveau site complet est nécessaire pour les inscriptions annuelles.
* Des fonctions interactives, comme par exemple un agent conversationnel, sont envisagées pour aider à transmettre les nouvelles politiques RH aux employés.
* En raison de l'augmentation du nombre d'employés, la fréquentation du site augmente mais le budget consacré à l'infrastructure reste stable.
* L'équipe RH fait l'objet de pressions pour aller plus vite : elle doit déployer rapidement les nouvelles fonctions du site et publier fréquemment les changements de dernière minutes liés aux prestations.
* La période des inscriptions dure deux semaines et donc toute indisponibilité de la nouvelle application n'est pas admise.

**Solution**

La compagnie aérienne veut instaurer une culture ouverte en mettant les personnes au premier plan. Le responsable RH a tout à fait conscience que le fait de récompenser et de conserver des talents a un impact non négligeable sur la rentabilité de la compagnie aérienne. Par conséquent, la diffusion annuelle des prestations constitue un aspect essentiel pour encourager une culture d'entreprise centrée sur les employés.

Elle a besoin d'une solution qui profite à la fois aux développeurs et à ses utilisateurs :
* DEVELOPPEMENT FRONT-END POUR LES PRESTATIONS EXISTANTES : assurances, programmes de formation, bien-être, etc.
* FONCTIONS SPECIFIQUES A UNE REGION : chaque pays dispose de politiques RH particulières donc le site global doit paraître similaire tout en intégrant les prestations spécifiques à chaque région
* OUTILS DE DEVELOPPEMENT CONVIVIAUX accélérant le déploiement de fonctions et la correction des erreurs
* AGENT CONVERSATIONNEL pour fournir des conversations authentiques sur les prestations et répondre efficacement aux demandes et aux questions des utilisateurs.

Solution technique :
* {{site.data.keyword.containerlong_notm}}
* IBM Watson Assistant et Tone Analyzer
* {{site.data.keyword.contdelivery_full}}
* IBM Logging and Monitoring
* {{site.data.keyword.SecureGatewayfull}}
* {{site.data.keyword.appid_full_notm}}

Un développement accéléré est un atout majeur pour le responsable RH. L'équipe commence par conteneuriser les applications et à les placer dans le cloud. Avec l'utilisation de conteneurs modernes, les développeurs peuvent facilement expérimenter le kit de développement de logiciels (SDK) Node.js et intégrer les changements dans des systèmes de développement et de test, répartis sur différents clusters. Ces intégrations sont automatiques avec des chaînes d'outils ouvertes et {{site.data.keyword.contdelivery_full}}. Les mises à jour de ce site RH ne sont plus confinées dans des processus de construction lents et susceptibles de comporter des erreurs. Désormais il est possible d'effectuer des mises à jour incrémentielles du site tous les jours, voire plus fréquemment.  En outre, la consignation et la surveillance du site RH est rapidement intégrée, notamment pour le mode d'extraction de données personnalisées du site à partir de systèmes de prestations de back-end. Les développeurs ne perdent plus de temps à construire des systèmes de consignation complexes, juste pour pouvoir identifier et résoudre les incidents des systèmes de production. Les développeurs n'ont pas besoin de devenir des experts en matière de sécurité de cloud, ils peuvent facilement imposer l'authentification axée sur des règles en utilisant {{site.data.keyword.appid_full_notm}}.

Avec {{site.data.keyword.containerlong_notm}}, ils passent d'un matériel sursollicité dans un centre de données privé à un calcul personnalisé qui réduit les opérations informatiques et la maintenance, et économise l'énergie. Pour héberger le site RH, ils peuvent aisément concevoir des clusters Kubernetes afin de satisfaire leurs besoins en termes d'UC, de mémoire RAM et de stockage. Un autre facteur permettant de réduire les coûts de personnel réside dans le fait qu'IBM gère Kubernetes, de sorte que les développeurs puissent se concentrer à apporter une meilleure expérience des employés dans le cadre de l'inscription aux prestations.

{{site.data.keyword.containerlong_notm}} fournit des ressources de calcul évolutives et les tableaux de bord DevOps associés pour créer, mettre à l'échelle et supprimer des applications et des services à la demande. A l'aide de la technologie des conteneurs aux normes de l'industrie, les applications peuvent être facilement développées et partagées entre plusieurs environnements de développement, de test et de production. Cette configuration contribue à l'évolutivité immédiate. A l'aide d'une gamme enrichie d'objets de déploiement et d'exécution Kubernetes, l'équipe RH peut surveiller et gérer les mises à niveau des applications en toute fiabilité. Elle peut également répliquer et mettre à l'échelle les applications à l'aide de règles définies et de l'orchestration automatique de Kubernetes.

**Etape 1 : Conteneurs, microservices et Garage Method**
* Les applications sont générées dans un ensemble de microservices coopératifs qui s'exécutent dans {{site.data.keyword.containerlong_notm}}. L'architecture représente les zones fonctionnelles de l'application présentant le plus de problèmes de qualité.
* Déployer les applications dans des conteneurs dans {{site.data.keyword.containerlong_notm}}, avec une analyse en continu avec IBM Vulnerability Advisor.
* Fournir des tableaux de bord DevOps normalisés via Kubernetes.
* Adopter les pratiques de développement agile et itératif essentielles d'IBM Garage Method pour permettre la publication fréquente de nouvelles fonctions, nouveaux modules de correction et correctifs sans susciter d'indisponibilité.

**Etape 2 : Connexions aux systèmes de prestations de back-end**
* {{site.data.keyword.SecureGatewayfull}} est utilisé pour créer un tunnel sécurisé vers les systèmes locaux hébergeant les systèmes de prestations.  
* La combinaison des données locales et d'{{site.data.keyword.containerlong_notm}} permet à l'équipe RH d'accéder aux données sensibles tout en respectant la réglementation en vigueur.
* Les discussions avec l'agent conversationnel entrent dans les politiques RH, permettant au site de prestations d'identifier les offres les plus populaires et les moins populaires, y compris les améliorations à apporter aux initiatives peu performantes.

**Etape 3 : Agent conversationnel et personnalisation**
* IBM Watson Assistant fournit des outils pour mettre en place rapidement un agent conversationnel pouvant fournir aux utilisateurs les informations adéquates sur les prestations.
* Watson Tone Analyzer s'assure que les clients sont satisfaits des discussions avec l'agent conversationnel et fait appel à une intervention humaine si nécessaire.

**Etape 4 : Livraison en continu dans le monde entier**
* {{site.data.keyword.contdelivery_full}} aide les développeurs à mettre en place rapidement une chaîne d'outils intégrée, en utilisant des modèles pouvant être partagés et personnalisés avec les outils d'IBM, de tiers et des outils open source. Les compilations et les tests sont automatiques et le contrôle qualité est assuré par des fonctions d'analyse.
* Une fois que les développeurs ont construit et testé les applications dans leurs clusters de développement et de test, ils utilisent des chaînes d'outils IBM CI/CD pour déployer des applications dans des clusters de production à l'échelle mondiale.
* {{site.data.keyword.containerlong_notm}} facilite le transfert et la restauration d'applications. Des applications personnalisées sont déployées pour répondre aux exigences régionales via le routage intelligent et l'équilibrage de charge d'Istio.
* Les outils à haute disponibilité (HA) intégrés dans {{site.data.keyword.containerlong_notm}} équilibrent la charge de travail au sein de chaque région géographique, en incluant la réparation spontanée et l'équilibrage de charge.

**Résultats**
* Avec des outils comme l'agent conversationnel, l'équipe RH a prouvé au personnel que l'innovation faisait réellement partie de la culture d'entreprise et ne se limitait pas à de belles paroles.
* L'authenticité associée à la personnalisation du site a répondu aux nouvelles attentes du personnel de la compagnie aérienne d'aujourd'hui.
* Les mises à jour de dernière minute du site RH, en particulier celles qui ont été mises en place suite aux discussions des employés avec l'agent conversationnel, sont rapidement opérationnelles car les développeurs intègrent désormais les changements au moins 10 fois par jour.
* Avec une gestion de l'infrastructure assurée par IBM, l'équipe de développement a pu fournir le site en moins de 3 semaines.
