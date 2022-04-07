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


# Cas d'utilisation d'{{site.data.keyword.cloud_notm}} pour les services financiers
{: #cs_uc_finance}

Ces cas d'utilisation mettent en évidence comment les charges de travail sur {{site.data.keyword.containerlong_notm}} peuvent
tirer parti d'un calcul hautes performances à haute disponibilité, d'une rotation facile des clusters pour accélérer le développement et de l'intelligence artificielle (IA) avec {{site.data.keyword.ibmwatson}}.
{: shortdesc}

## Une société hypothécaire réduit les coûts et accélère la conformité aux réglementations
{: #uc_mortgage}

Un responsable de la gestion des risques d'une société hypothécaire résidentielle traite 70 millions d'enregistrements par jour mais le système local s'est avéré à la fois lent et inexact. Les dépenses informatiques ont monté en flèche car le matériel est vite devenu obsolète sans avoir été rentabilisé. En attendant que le matériel soit remplacé, la société a pris du retard en terme de conformité aux réglementations.
{: shortdesc}

Pourquoi {{site.data.keyword.Bluemix_notm}} ? Pour améliorer l'analyse des risques, cette société s'est tournée vers les services {{site.data.keyword.containerlong_notm}} et IBM Cloud Analytic pour réduire les coûts, augmenter la disponibilité à l'échelle internationale et enfin accélérer la conformité aux réglementations. Avec {{site.data.keyword.containerlong_notm}} dans plusieurs régions, ses applications d'analyse peuvent être conteneurisées et déployées dans le monde entier, améliorant ainsi la disponibilité et répondant aux réglementations locales. Ces déploiements sont accélérés grâce à des outils open source familiers, déjà intégrés dans {{site.data.keyword.containerlong_notm}}.

{{site.data.keyword.containerlong_notm}} et technologies clés :
* [Mise à l'échelle horizontale](/docs/containers?topic=containers-app#highly_available_apps)
* [Plusieurs régions pour assurer une haute disponibilité](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)
* [Clusters adaptés aux différents besoins en matière de stockage, d'UC et de mémoire RAM](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Sécurité et isolement de conteneur](/docs/containers?topic=containers-security#security)
* [{{site.data.keyword.cloudant}} pour conserver et synchroniser les données sur plusieurs applications](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)

**Solution**

Les applications d'analyse de cette société ont été d'abord conteneurisées puis placées dans le cloud. En un éclair, le casse-tête lié au matériel a disparu. Des clusters Kubernetes ont pu facilement être conçus pour répondre aux besoins de la société en termes d'UC, de mémoire RAM, de stockage et de sécurité à hautes performances. Et lorsque les applications d'analyse utilisées changent, il est possible d'ajouter ou de réduire le traitement sans avoir à investir des sommes folles dans le matériel. Avec la mise à l'échelle horizontale d'{{site.data.keyword.containerlong_notm}}, les applications de la société ont pu s'adapter à l'évolution du nombre d'enregistrements, accélérant ainsi la production de rapports réglementaires. {{site.data.keyword.containerlong_notm}} fournit des ressources de calcul élastiques, à hautes performances et sécurisées dans le monde entier, permettant d'utiliser la pleine capacité des ressources informatiques modernes.

A présent, ces applications reçoivent d'importants volumes de données depuis un entrepôt de données sur {{site.data.keyword.cloudant}}. Le stockage en cloud dans {{site.data.keyword.cloudant}} assure une meilleure disponibilité qu'avec un stockage verrouillé dans un système local. Comme la disponibilité est un facteur essentiel, les applications sont déployées dans différents centres de données dans le monde entier, pour assurer la reprise après incident et réduire les temps d'attente.

La société a pu accélérer sont traitement en matière d'analyse des risques et de conformité. Ses fonctions d'analyse prédictive et d'analyse des risques, telles que les calculs Monte Carlo, sont désormais constamment mises à jour au moyen de déploiements agiles et itératifs. L'orchestration de conteneurs est traitée par un maître Kubernetes géré de sorte que les coûts d'exploitation sont également réduits. Et enfin, l'analyse des risques pour les hypothèques est plus réactive à l'évolution rapide du marché.

**Contexte : Utilisation de modèles financiers et de modèles de conformité pour les hypothèques résidentielles**

* Les besoins accrus pour assurer une meilleure gestion des risques financiers ont conduit à une augmentation de la surveillance réglementaire. Les mêmes besoins font que l'analyse associée des processus et des informations à fournir en matière de gestion des risques tend vers des rapports plus granulaires, mieux intégrés et plus nombreux en matière de réglementation.
* Les grilles de calcul à hautes performances constituent les composants d'infrastructure essentiels pour la création de modèles financiers.

Le problème de la société à l'heure actuelle est de faire face au volume et au délai de livraison.

Son environnement actuel date de plus de 7 ans. Il s'agit d'un environnement local avec une capacité limitée en termes de calcul, de stockage et d'E-S.
Les actualisations de serveur sont coûteuses et leur exécution prend beaucoup de temps.
Les mises à jour de logiciels et d'applications suivent un processus informel et ne sont pas reproductibles.
Le calcul à hautes performances réel est difficile à programmer. L'API est trop complexe pour les nouveaux développeurs qui viennent d'arriver et nécessitent des connaissances qui ne sont documentées nulle part.
Et l'exécution des mises à niveau des applications principales prend 6 à 9 mois.

**Modèle de solution : Services d'E-S, de stockage et de calcul à la demande s'exécutant dans un cloud public avec accès sécurisé aux actifs de l'entreprise locaux selon les besoins**

* Stockage de documents sécurisé et évolutif prenant en charge les analyses de documents structurés et non structurés
* Actifs et applications d'entreprise existants de type "Lift and shift" qui permettent en même temps l'intégration à des systèmes locaux qui ne seront pas migrés
* Solutions de déploiement plus rapides et implémentation de processus de surveillance et de processus DevOps standard pour traiter les bogues affectant l'exactitude des informations dans les rapports

**Solution détaillée**

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.cos_full_notm}}
* {{site.data.keyword.sqlquery_notm}} (Spark)
* {{site.data.keyword.cloudant}}
* {{site.data.keyword.SecureGateway}}

{{site.data.keyword.containerlong_notm}} fournit des ressources de calcul évolutives et les tableaux de bord DevOps associés pour créer, mettre à l'échelle et supprimer des applications et des services à la demande. Avec des conteneurs conformes aux normes de l'industrie, les applications peuvent être redirigées dès le départ pour être hébergées sur {{site.data.keyword.containerlong_notm}} sans modifications d'architecture majeures.

Cette solution offre des avantages immédiats en termes d'évolutivité. En utilisant la gamme enrichie d'objets de déploiement et d'exécution de Kubernetes, la société hypothécaire est en mesure de surveiller et gérer les mises à niveau des applications en toute fiabilité. Elle peut également mettre à l'échelle et répliquer les applications qui utilisent des règles définies et l'orchestration automatique de Kubernetes.

{{site.data.keyword.SecureGateway}} est utilisé pour créer un pipeline sécurisé vers les bases de données et les documents locaux pour les applications qui sont relocalisées pour s'exécuter dans {{site.data.keyword.containerlong_notm}}.

{{site.data.keyword.cos_full_notm}} est conçu pour tout le stockage brut de documents et de données à mesure des avancées. Pour les simulations Monte Carlo, un pipeline de flux de travaux est mis en place dans lequel les données de simulation figurent dans des fichiers structurés qui sont stockés dans {{site.data.keyword.cos_full_notm}}. Un déclencheur permettant de démarrer la simulation met à l'échelle les services de calcul dans {{site.data.keyword.containerlong_notm}} pour diviser les données des fichiers en compartiments de N événements pour la simulation. {{site.data.keyword.containerlong_notm}} s'adapte automatiquement aux exécutions des N services associés et consigne les résultats intermédiaires dans {{site.data.keyword.cos_full_notm}}. Ces résultats sont traités par un autre ensemble de services de calcul {{site.data.keyword.containerlong_notm}} pour produire les résultats finaux.

{{site.data.keyword.cloudant}} est une base de données NoSQL moderne qui convient à de nombreux cas d'utilisation axés sur les données, du stockage et des requêtes de données de type clé-valeur aux documents complexes. Pour gérer le volume grandissant des règles de rapports de gestion et de réglementations, la société hypothécaire utilise {{site.data.keyword.cloudant}} pour stocker les documents associés aux données de réglementation brutes qu'elle reçoit. Des processus de calcul sur {{site.data.keyword.containerlong_notm}} sont déclenchés pour compiler, traiter et publier les données dans divers formats de rapports. Les résultats intermédiaires communs aux rapports sont stockés sous forme de documents {{site.data.keyword.cloudant}} de sorte que des processus utilisant des modèles puissent être utilisés pour produire les rapports nécessaires.

**Résultats**

* Des simulations financières complexes sont réalisées en utilisant 25 % du temps qui était auparavant nécessaire avec les systèmes locaux existants.
* La durée de déploiement s'est améliorée pour passer de 6 à 9 mois à 1 à 3 semaines en moyenne. Ce progrès a été rendu possible car {{site.data.keyword.containerlong_notm}} permet d'utiliser un processus contrôlé et rigoureux pour augmenter les conteneurs d'application et les remplacer par de nouvelles versions. Les bogues reportés peuvent être corrigés rapidement, en traitant, par exemple, des problèmes liés à l'exactitude des données.
* Les coûts des rapports réglementaires ont été réduits avec un ensemble cohérent et évolutif de services de stockage et de calcul fournis par {{site.data.keyword.containerlong_notm}} et {{site.data.keyword.cloudant}}.
* Avec le temps, les applications d'origine qui étaient initialement transférées sur le cloud ont été réorganisées en microservices coopératifs s'exécutant sur {{site.data.keyword.containerlong_notm}}. Cette action a contribué à accélérer le développement et la durée de déploiement et a permis d'innover davantage en raison d'une relative facilité en matière d'expérimentation. Des applications innovantes sont sorties avec de nouvelles versions de microservices pour tirer parti des conditions du marché et du secteur d'activité (c'est ce que l'on appelle des applications et des microservices conjoncturels).

## Une société technique de paiements rationalise la productivité des développeurs, en déployant des outils d'intelligence artificielle (IA) pour leurs partenaires 4 fois plus vite
{: #uc_payment_tech}

Un responsable du développement dispose de développeurs qui utilisent des outils traditionnels locaux qui ralentissent le prototypage en attendant l'achat du matériel.
{: shortdesc}

Pourquoi {{site.data.keyword.Bluemix_notm}} ? {{site.data.keyword.containerlong_notm}} accélère les calculs en utilisant une technologie open source standard. Une fois que la société est passée à {{site.data.keyword.containerlong_notm}}, les développeurs ont eu accès à des outils DevOps conviviaux, comme des conteneurs portables et faciles à partager.

Les développeurs peuvent désormais expérimenter facilement l'intégration rapide de changements dans des systèmes de développement et de test avec des chaînes d'outils ouvertes. Les outils de développement de logiciels traditionnels sont radicalement transformés lorsqu'ils ajoutent des services de cloud dotés d'intelligence artificielle (IA) aux applications en un seul clic.

Technologies clés :
* [Clusters adaptés aux différents besoins en matière de stockage, d'UC et de mémoire RAM](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)
* [Prévention de la fraude avec {{site.data.keyword.watson}} AI](https://www.ibm.com/cloud/watson-studio)
* [Outils natifs DevOps, notamment des chaînes d'outils ouvertes dans {{site.data.keyword.contdelivery_full}}](https://www.ibm.com/cloud/garage/toolchains/)
* [SDK for Node.js](/docs/runtimes/nodejs?topic=Nodejs-nodejs_runtime#nodejs_runtime)
* [Possibilité de connexion sans modifier le code d'application en utilisant {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-getting-started)

**Contexte : Rationalisation de la productivité des développeurs et déploiement d'outils IA pour les partenaires 4 fois plus vite**

* Les interruptions sont de plus en plus fréquentes dans le secteur des paiements qui connaît une croissance spectaculaire pour les particuliers et les entreprises. Mais les mises à jour des outils de paiement demeurent particulièrement lentes.
* Des fonctions cognitives sont nécessaires pour identifier plus rapidement les transactions frauduleuses avec de nouveaux moyens.
* Avec le nombre croissant des partenaires et de leurs transactions, la circulation des outils augmente, mais le budget d'infrastructure de la société doit être réduit, en maximisant l'efficacité des ressources.
* La dette technique augmente faute de diminuer en raison de l'incapacité de lancer des logiciels de qualité pour pouvoir répondre aux exigences du marché.
* Les budgets de dépenses d'investissement sont étroitement contrôlés et le service informatique a conscience qu'il n'a pas le budget ou le personnel nécessaire pour créer les environnements de test et de préproduction avec les systèmes dont il dispose en interne.
* La sécurité tend à devenir la préoccupation majeure et vient s'ajouter aux problèmes de livraison, provoquant ainsi des retards supplémentaires.

**Solution**

Le responsable du développement doit faire face à de nombreux défis dans le secteur dynamique des paiements. Les réglementations, le comportement des consommateurs, la fraude, la concurrence et les infrastructures du marché évoluent rapidement. Un développement rapide est essentiel pour faire partie de l'univers des paiements à venir.

Le modèle de gestion consiste à fournir des outils de paiement aux partenaires commerciaux, pour qu'ils puissent aider ces institutions financières et d'autres organisations à fournir des expériences en matière de paiement numérique avec une sécurité renforcée.

La solution adaptée consiste à aider les développeurs et leurs partenaires commerciaux :
* DE L'AVANT-GUICHET AUX OUTILS DE PAIEMENT : systèmes de redevance, suivi des paiements même transfrontaliers, conformité aux réglementations, données biométriques, versements, etc.
* FONCTIONS SPECIFIQUES A LA REGLEMENTATION : chaque pays comporte des réglementations uniques de sorte que l'ensemble d'outils global peut sembler similaire mais apporte des avantages propres aux différents pays
* OUTILS DE DEVELOPPEMENT CONVIVIAUX qui accélèrent le déploiement des fonctions et des corrections d'erreurs
* DETECTION DES FRAUDES EN TANT QUE SERVICE (FDaaS) avec {{site.data.keyword.watson}} AI pour garder une longueur d'avance sur les actions frauduleuses fréquentes qui ne cessent d'augmenter.

**Modèle de solution**

Calcul à la demande, outils DevOps et intelligence artificielle qui s'exécutent dans un cloud public avec accès aux systèmes de paiement de back-end. Implémentation d'un processus d'intégration continue et de distribution continue (CI/CD) pour raccourcir considérablement les cycles de livraison.

Solution technique :
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.contdelivery_full}}
* IBM Cloud Logging and Monitoring
* {{site.data.keyword.ibmwatson_notm}} for Financial Services
* {{site.data.keyword.appid_full_notm}}

La société a commencé par conteneuriser les machines virtuelles des outils de paiement et à les placer dans le cloud. En un éclair, le casse-tête lié au matériel a disparu. Des clusters Kubernetes ont pu facilement être conçus pour répondre aux besoins de la société en termes d'UC, de mémoire RAM, de stockage et de sécurité. Et lorsque les outils de paiements doivent évoluer, elle a pu ajouter ou réduire les calculs sans acquisition coûteuse et lente de matériel.

Avec la mise à l'échelle horizontale d'{{site.data.keyword.containerlong_notm}}, les applications de la société s'adaptent à l'évolution du nombre de partenaires, accélérant ainsi la croissance. {{site.data.keyword.containerlong_notm}} fournit des ressources de calcul élastiques sécurisées dans le monde entier permettant d'utiliser la pleine capacité des ressources informatiques modernes.

Un développement accéléré est un facteur de réussite pour le responsable. Avec l'utilisation de conteneurs modernes, les développeurs peuvent conduire des tests dans les langages de leur choix et intégrer les changements dans des systèmes de développement et de test, répartis sur différents clusters. Ces intégrations sont automatiques avec des chaînes d'outils ouvertes et {{site.data.keyword.contdelivery_full}}. Les mises à jour d'outils ne sont plus confinées dans des processus de construction lents et susceptibles de comporter des erreurs. Les développeurs peuvent fournir des mises à jour incrémentielles à leurs outils, tous les jours, voire plus fréquemment.

Les fonctions de consignation et de surveillance des outils, notamment avec l'utilisation de l'intelligence artificielle de {{site.data.keyword.watson}}, sont rapidement intégrées dans le système. Les développeurs ne perdent plus de temps à construire des systèmes de consignation complexes, juste pour pouvoir identifier et résoudre les incidents de leurs systèmes de production. Un facteur clé pour réduire les coûts de personnel réside dans le fait qu'IBM gère Kubernetes, de sorte que les développeurs puissent se concentrer sur de meilleurs outils de paiement.

La sécurité avant tout : avec la technologie bare metal pour {{site.data.keyword.containerlong_notm}}, les outils de paiement sensibles disposent désormais d'un isolement standard mais dans le cadre de la flexibilité du cloud public. La technologie Bare metal fournit une fonction de calcul sécurisé qui peut vérifier que le matériel sous-jacent ne fait pas l'objet de falsification. Des analyses pour détecter les vulnérabilités et les logiciels malveillants sont exécutées en permanence.

**Etape 1 : Méthode "Lift and shift" pour sécuriser les calculs**
* Les applications qui gèrent des données ultra-sensibles peuvent être redirigées pour être hébergées sur {{site.data.keyword.containerlong_notm}} qui s'exécute sur Bare Metal for Trusted Compute. La fonction de calcul sécurisé (Trusted Compute) peut vérifier que le matériel sous-jacent ne fait pas l'objet de falsification.
* Migration des images de machine virtuelle sur des images de conteneur qui s'exécutent sur {{site.data.keyword.containerlong_notm}} dans {{site.data.keyword.Bluemix_notm}} public.
* Sur cette base, Vulnerability Advisor fournit des fonctionnalités d'analyse d'images, de règles, de conteneurs et de vulnérabilités liées aux packages pour détecter les logiciels malveillants connus.
* Les coûts liés à un centre de données privé ou aux dépenses locales sont considérablement réduits et remplacés par un modèle informatique à la demande qui évolue en fonction des exigences de la charge de travail.
* Imposition uniforme d'une authentification gérée par des règles à vos services et API avec une simple annotation Ingress. Avec la sécurité déclarative, vous pouvez garantir l'authentification des utilisateurs et la validation de jeton en utilisant {{site.data.keyword.appid_short_notm}}.

**Etape 2 : Opérations et connexions à des systèmes de paiement de back end existants**
* Utiliser IBM {{site.data.keyword.SecureGateway}} pour que les connexions soient toujours sécurisées sur les systèmes d'outils locaux.
* Fournir des méthodes et des tableaux de bord DevOps normalisés via Kubernetes.
* Une fois que les développeurs ont construit et testé des applications dans des clusters de développement et de test, ils utilisent les chaînes d'outils d'{{site.data.keyword.contdelivery_full}} pour déployer des applications dans les clusters d'{{site.data.keyword.containerlong_notm}} dans le monde entier.
* Les outils à haute disponibilité (HA) intégrés dans {{site.data.keyword.containerlong_notm}} équilibrent la charge de travail au sein de chaque région géographique, en incluant la réparation spontanée et l'équilibrage de charge.

**Etape 3 : Analyse et prévention des fraudes**
* Déployer IBM {{site.data.keyword.watson}} for Financial Services pour la prévention et la détection des fraudes.
* A l'aide des chaînes d'outils et des outils de déploiement Helm, les applications sont également déployées sur les clusters {{site.data.keyword.containerlong_notm}} dans le monde entier. Les charges de travail et les données respectent alors les réglementations régionales en vigueur.

**Résultats**
* Faire passer les machines virtuelles monolithiques existantes sur des conteneurs hébergés dans le cloud constituait la première étape qui permettait au responsable du développement de faire des économies sur les coûts d'investissement et d'exploitation.
* Avec une gestion de l'infrastructure assurée par IBM, l'équipe de développement est libre de fournir des mises à jour 10 fois par jour.
* En parallèle, le fournisseur a mis en place des itérations simples délimitées dans le temps pour maîtriser la dette technique existante.
* Avec le nombre de transactions traitées, les opérations peuvent être mises à l'échelle de manière exponentielle.
* En même temps, la nouvelle méthode d'analyse des fraudes de {{site.data.keyword.watson}} a contribué à accélérer la détection et la prévention, avec 4 fois moins de fraudes par rapport à la moyenne régionale.
