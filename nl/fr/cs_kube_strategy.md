---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-03"

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


# Définition de votre stratégie Kubernetes
{: #strategy}

{{site.data.keyword.containerlong}} vous permet de procéder au déploiement rapide et sécurisé de charges de travail de conteneur pour vos applications dans un environnement de production. Découvrez davantage d'informations, ainsi, lors de la planification de votre stratégie de cluster, vous pourrez optimiser votre configuration afin de tirer le meilleur parti des fonctions automatisées de déploiement, de mise à l'échelle et de gestion d'orchestration [Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/).
{:shortdesc}

## Déplacement de vos charges de travail vers {{site.data.keyword.Bluemix_notm}}
{: #cloud_workloads}

Nombreuses sont les raisons qui vous incitent à déplacer vos charges de travail {{site.data.keyword.Bluemix_notm}} : réduire le coût total de possession, augmenter la haute disponibilité pour vos applications dans un environnement sécurisé et conforme, mettre à l'échelle par augmentation et par diminution afin de répondre aux demandes d'utilisateur, et bien plus encore. {{site.data.keyword.containerlong_notm}} combine une technologie de conteneur et des outils open source, tels que Kubernetes, afin de vous permettre de créer une application native en cloud capable de migrer entre différents environnements de cloud et d'éviter ainsi le blocage des fournisseurs.
{:shortdesc}

Mais comment faire pour accéder au cloud ? Quelles sont vos options ? Et comment gérer vos charges de travail lorsque vous y parvenez ?

Utilisez cette page pour découvrir certaines stratégies pour vos déploiements Kubernetes sur {{site.data.keyword.containerlong_notm}}. Et n'hésitez pas à dialoguer avec votre équipe sur [Slack. ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-container-service.slack.com)

Pas encore sur Slack ? [Demandez une invitation !](https://bxcs-slack-invite.mybluemix.net/)
{: tip}

### Que puis-je déplacer vers {{site.data.keyword.Bluemix_notm}} ?
{: #move_to_cloud}

With {{site.data.keyword.Bluemix_notm}} vous offre la possibilité de créer des clusters Kubernetes dans des [environnements de cloud hors site, sur site ou hybrides](/docs/containers?topic=containers-cs_ov#differentiation). Le tableau ci-après fournit quelques exemples de types de charge de travail généralement déplacés par les utilisateurs vers les divers types de cloud. Vous pouvez également choisir une approche hybride où des clusters sont exécutés dans les deux environnements.
{: shortdesc}

| Charge de travail | {{site.data.keyword.containershort_notm}} hors site |Sur site |
| --- | --- | --- |
| Outils d'activation DevOps | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> | |
| Développement et test d'applications | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> | |
| Des déplacements majeurs sont demandés pour les applications et elles doivent rapidement faire l'objet d'une mise à l'échelle | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> | |
| Applications métier, telles que CRM, HCM, ERP et E-commerce | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> | |
| Outils de collaboration et de réseau social, tels que la messagerie électronique | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> | |
| Charges de travail Linux et x86 | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> | |
| Ressource bare metal et de calcul GPU | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> |
| Charges de travail compatibles PCI et HIPAA | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> |
| Applications existantes avec des contraintes et des dépendances de plateforme et d'infrastructure | | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> |
| Applications de propriété avec des conceptions strictes, des règles d'octroi de licence ou des réglementations sévères | | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> |
| Mise à l'échelle d'applications dans le cloud public et synchronisation des données sur une base de données privée sur site | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />  | <img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" /> |
{: caption="Les implémentations d'{{site.data.keyword.Bluemix_notm}} prennent en charge vos charges de travail" caption-side="top"}

**Prêt à exécuter vos charges de travail hors site dans l{{site.data.keyword.containerlong_notm}} ?**</br>
Parfait ! Vous êtes déjà dans notre documentation sur le cloud public. Poursuivez votre lecture afin de découvrir d'autres idées de stratégies ou prenez une longueur d'avance en [créant un cluster dès maintenant](/docs/containers?topic=containers-getting-started).

**Le cloud sur site vous intéresse ?**</br>
Explorez la [documentation sur {{site.data.keyword.Bluemix_notm}} Private ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.1/kc_welcome_containers.html). Si vous avez déjà réalisé d'importants investissements en matière de technologie IBM et possédez par exemple WebSphere Application Server et Liberty, vous pouvez optimiser votre stratégie de modernisation d'{{site.data.keyword.Bluemix_notm}} Private à l'aide de différents outils.

**Vous souhaitez exécuter des charges de travail dans des clouds sur site et hors site ?**</br>
Commencez par configurer un compte {{site.data.keyword.Bluemix_notm}} Private. Ensuite, reportez-vous à la rubrique [Utilisation d'{{site.data.keyword.containerlong_notm}} avec {{site.data.keyword.Bluemix_notm}} Private](/docs/containers?topic=containers-hybrid_iks_icp) pour connecter votre environnement {{site.data.keyword.Bluemix_notm}} Private à un cluster dans {{site.data.keyword.Bluemix_notm}} Public. Pour gérer plusieurs clusters Kubernetes en cloud, par exemple, dans {{site.data.keyword.Bluemix_notm}} Public et {{site.data.keyword.Bluemix_notm}} Private, voir [IBM Multicloud Manager ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).

### Quel type d'application puis-je exécuter dans {{site.data.keyword.containerlong_notm}} ?
{: #app_types}

Votre application conteneurisée doit pouvoir être exécutée sur un système d'application pris en charge, Ubuntu 16.64, 18.64. Vous souhaitez également prendre en compte le caractère avec état de votre application.
{: shortdesc}

<dl>
<dt>Applications sans état</dt>
  <dd><p>Les applications sans état sont privilégiées pour des environnements natifs en cloud, tels que Kubernetes. Elles sont faciles à migrer et à mettre à l'échelle car elles déclarent des dépendances, stockent des configurations indépendamment du code et traitent les services de sauvegarde, tels que les bases de données, comme des ressources connectées et non comme des ressources couplées à l'application. Les pods d'application n'ont pas besoin de stockage de données persistant ni d'adresse IP de réseau stable, et par conséquent, ils peuvent être arrêtés, replanifiés et mis à l'échelle en fonction des demandes de charge de travail. L'application utilise un service DaaS (Database-as-a-Service) pour les données persistantes et les services NodePort, d'équilibreur de charge ou Ingress pour exposer la charge de travail sur une adresse IP stable.</p></dd>
<dt>Applications avec état</dt>
  <dd><p>Comparées aux applications sans état, les applications avec état sont beaucoup plus compliquées à configurer, à gérer et à mettre à l'échelle car les pods requièrent des données persistantes et une identité de réseau stable. Les applications avec état sont souvent des bases de données ou d'autres charges de travail distribuées à forte consommation de données dans lesquelles le traitement est plus efficace à proximité des données proprement dites.</p>
  <p>Si vous souhaitez déployer une application avec état, vous devez configurer un stockage persistant et monter un volume persistant sur le pod qui est contrôlé par un objet StatefulSet. Vous pouvez choisir d'ajouter un stockage de [fichiers](/docs/containers?topic=containers-file_storage#file_statefulset), un stockage [par blocs](/docs/containers?topic=containers-block_storage#block_statefulset) ou un stockage d'[objets](/docs/containers?topic=containers-object_storage#cos_statefulset) comme stockage persistant pour votre ensemble avec état. Vous pouvez également installer [Portworx](/docs/containers?topic=containers-portworx) sur vos noeuds worker bare metal et utiliser Portworx comme solution de stockage défini par logiciel (SDS) à haute disponibilité afin de gérer du stockage persistant pour vos applications avec état. Pour plus d'informations sur le fonctionnement des ensembles avec état, voir la [documentation Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/).</p></dd>
</dl>

### Quelles sont les instructions de développement d'applications natives en cloud sans état ?
{: #12factor}

Consultez la méthodologie indépendante du langage utilisé décrite sur le site [Twelve-Factor App ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://12factor.net/) pour savoir comment développer votre application en fonction de 12 facteurs, résumés comme suit.
{: shortdesc}

1.  **Base de code** : Utilisez une base de code dans un système de contrôle de version pour vos déploiements. Lorsque vous extrayez une image pour votre déploiement de conteneur, spécifiez une balise d'image de test au lieu d'utiliser la dernière (valeur `latest`).
2.  **Dépendances** : Déclarez et isolez explicitement des dépendances externes.
3.  **Configuration** : Stockez une configuration spécifique au déploiement dans les variables d'environnement et non dans le code.
4.  **Services de sauvegarde**: Traitez les services de sauvegarde, tels que des magasins de données ou des files d'attente de messages, comme des ressources connectées ou remplaçables.
5.  **Etapes d'application** : Créez des étapes distinctes, telles que `build`, `release`, `run`, avec un cloisonnement strict entre chacune d'elles.
6.  **Processus** : Exécutez un ou plusieurs processus sans état qui ne partagent rien et utilisez le [stockage persistant](/docs/containers?topic=containers-storage_planning) pour sauvegarder les données.
7.  **Liaison de ports** : Les liaisons de port sont autonomes et fournissent un noeud final de service sur un hôte et un port bien définis.
8.  **Accès concurrents** : Gérez et mettez à l'échelle votre application via des instances de processus telles que les répliques et la mise à l'échelle horizontale. Définissez des demandes et des limites de ressource pour vos déploiements. Notez que les règles réseau Calico ne limitent pas la bande passante. En revanche, vous pouvez utiliser [Istio](/docs/containers?topic=containers-istio).
9.  **Supprimable** : Concevez votre application pour qu'elle soit supprimable, avec un minimum de démarrage, un arrêt approprié et une tolérance aux arrêts de traitement brutaux. N'oubliez pas que les conteneurs, les pods et même les noeuds worker sont censés être supprimables, par conséquent, planifiez votre application en conséquence.
10.  **Parité développement et production** : Configurez un pipeline d'[intégration continue](https://www.ibm.com/cloud/garage/content/code/practice_continuous_integration/) et de [distribution continue](https://www.ibm.com/cloud/garage/content/deliver/practice_continuous_delivery/) pour votre application, avec un minimum de différences entre l'application en phase de développement et l'application en phase de production. 
11.  **Journaux** : Traitez les journaux comme des flux d'événements : L'environnement externe ou d'hébergement traite et achemine les fichiers journaux. **Important** : dans {{site.data.keyword.containerlong_notm}}, les journaux ne sont pas activés par défaut. Pour les activer, voir [Configuration de l'acheminement des journaux](/docs/containers?topic=containers-health#configuring).
12.  **Processus d'administration** : conservez les scripts d'administration ponctuels avec votre application et exécutez-les en tant  qu'[objet de travail Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) pour faire en sorte que les scripts d'administration soient exécutés avec le même environnement que l'application proprement dite. Pour l'orchestration de packages plus volumineux que vous souhaitez exécuter dans vos clusters Kubernetes, pensez à utiliser un gestionnaire de package, tel que [Helm ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://helm.sh/).

### J'ai déjà une application. Comment puis-je la faire migrer vers {{site.data.keyword.containerlong_notm}} ?
{: #migrate_containerize}

Vous pouvez effectuer les étapes générales suivantes pour conteneuriser votre application :
{: shortdesc}

1.  Servez-vous du site [Twelve-Factor App ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://12factor.net/) comme guide pour isoler des dépendances, séparer des processus en services distinctes et réduire le caractère avec état de votre application le plus possible.
2.  Recherchez une image de base appropriée à utiliser. Vous pouvez utiliser des images disponibles pour le public à partir de [Docker Hub ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://hub.docker.com/), des [images IBM publiques](/docs/services/Registry?topic=registry-public_images#public_images), ou créer et gérer votre propre image dans votre {{site.data.keyword.registryshort_notm}}.
3.  Ajoutez à votre image Docker uniquement ce qui est nécessaire pour exécuter l'application.
4.  Au lieu de vous en remettre au stockage local, prévoyez d'utiliser du stockage persistant ou des solutions database-as-a-service en cloud pour sauvegarder les données de votre application.
5.  A terme, restructurez vos processus d'application en microservices.

Pour plus d'informations, voir les tutoriels suivants :
*  [Migration d'une application de Cloud Foundry vers un cluster](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)
*  [Déplacement d'une application basée sur une machine virtuelle vers Kubernetes](/docs/tutorials?topic=solution-tutorials-vm-to-containers-and-kubernetes)



Consultez également les rubriques ci-après pour plus d'informations sur le déplacement de charges de travail, telles que les environnements Kubernetes, la haute disponibilité, la reconnaissance de service et les déploiements.

<br />


### Quelles sont les connaissances et les compétences techniques qu'il est recommandé de posséder avant de déplacer ses applications vers {{site.data.keyword.containerlong_notm}} ?
{: #knowledge}

Kubernetes est conçu pour fournir des fonctions à deux personnes, l'administrateur de cluster et le développeur d'applications. Chaque personne utilise différentes compétences techniques pour exécuter et développer des applications sur un cluster.
{: shortdesc}

**Quelles sont les principales tâches et les compétences techniques d'un administrateur de cluster ?** </br>
En tant qu'administrateur de cluster, vous êtes chargé de configurer, faire fonctionner, sécuriser et gérer l'infrastructure {{site.data.keyword.Bluemix_notm}} de votre cluster. En général, les tâches sont les suivantes :

- Dimensionner le cluster afin de fournir suffisamment de capacité pour vos charges de travail. 
- Concevoir un cluster afin d'atteindre les normes de haute disponibilité, de reprise après incident et de conformité de votre entreprise. 
- Sécuriser le cluster en configurant des droits utilisateur et en limitant les actions au sein du cluster afin de protéger vos ressources de calcul, votre réseau et vos données. 
- Planifier et gérer la communication réseau entre les composants d'infrastructure afin de garantir la sécurité, la segmentation et la conformité du réseau. 
- Planifier des options de stockage permanent afin de répondre aux exigences en matière d'hébergement de données et de protection des données. 

La personne chargée de l'administration de cluster doit avoir des connaissances approfondies en matière de réseau, de stockage, de sécurité et de conformité. En général, dans une entreprise, ces connaissances sont réparties entre plusieurs spécialistes, tels que les ingénieurs système, les administrateurs système, les ingénieurs réseau, les architectes réseau, les responsables informatiques ou les spécialistes sécurité et conformité. Pensez à affecter le rôle d'administration de cluster à plusieurs personnes de votre entreprise de sorte que celle-ci possède les connaissances requises pour faire fonctionner correctement le cluster. 

**Quelles sont les principales tâches et les compétences techniques d'un développeur d'applications ?** </br>
En tant que développeur, vous concevez, créez, sécurisez, déployez, testez, exécutez et surveillez des applications conteneurisées natives cloud dans un cluster Kubernetes. Pour créer et exécuter ces applications, vous devez bien connaître les concepts de microservices, les instructions relatives aux [applications à 12 facteurs](#12factor), les [principes Docker et de conteneurisation](https://www.docker.com/) et les [options de déploiement Kubernetes](/docs/containers?topic=containers-app#plan_apps) disponibles. Si vous souhaitez déployer des applications sans serveur, familiarisez-vous avec [Knative](/docs/containers?topic=containers-cs_network_planning).

Kubernetes et {{site.data.keyword.containerlong_notm}} fournissent plusieurs options pour [exposer une application et maintenir une application privée](/docs/containers?topic=containers-cs_network_planning), [ajouter du stockage persistant](/docs/containers?topic=containers-storage_planning), [intégrer d'autres services](/docs/containers?topic=containers-ibm-3rd-party-integrations) et [sécuriser vos charges de travail et protéger des données sensibles](/docs/containers?topic=containers-security#container). Avant de déplacer votre application vers un cluster dans {{site.data.keyword.containerlong_notm}}, vérifiez que vous pouvez exécuter votre application en tant qu'application conteneurisée sur le système d'exploitation Ubuntu 16.64, 18.64 pris en charge et que Kubernetes et {{site.data.keyword.containerlong_notm}} fournissent les fonctions dont votre charge de travail a besoin. 

**Les administrateurs de cluster et les développeurs interagissent-ils ?** </br>
Oui. Les administrateurs de cluster et les développeurs doivent interagir fréquemment afin que les administrateurs de cluster puissent comprendre les besoins en charge de travail et fournir cette capacité dans le cluster et pour que les développeurs puissent prendre connaissance des limitations, intégrations et principes de sécurité disponibles dont ils doivent tenir compte dans leur processus de développement d'application. 

## Dimensionnement de votre cluster Kubernetes pour permettre la prise en charge de votre charge de travail
{: #sizing}

Déterminer le nombre de noeuds worker dont vous avez besoin dans votre cluster pour permettre la prise en charge de votre charge de travail n'est pas une science exacte. Il peut s'avérer nécessaire de tester différentes configurations et de procéder à des adaptations. C'est une bonne chose que vous utilisiez {{site.data.keyword.containerlong_notm}}, car ce composant vous permet d'ajouter et de retirer des noeuds worker en fonction des demandes de charge de travail.
{: shortdesc}

Pour commencer le dimensionnement de votre cluster, posez-vous les questions suivantes :

### Combien de ressources mon application nécessite-t-elle ?
{: #sizing_resources}

Tout d'abord, commençons par votre utilisation de charge de travail de projet ou existante.

1.  Calculez l'utilisation moyenne de la mémoire et de l'unité centrale de votre charge de travail. Par exemple, vous pouvez visualiser le gestionnaire de tâches sur une machine Windows ou exécutez la commande `top` sur une machine Mac ou Linux. Vous pouvez également utiliser un service de métriques et exécuter des rapports afin de calculer l'utilisation de la charge de travail.
2.  Anticipez le nombre de demandes que votre charge de travail doit servir afin de pouvoir déterminer le nombre de répliques d'application souhaité pour traiter la charge de travail. Ainsi, vous pouvez concevoir une instance d'application qui traite 1 000 demandes par minute et anticiper le fait que votre charge de travail doit servir 10 000 demandes par minute. Dans ce cas, vous pouvez décider de créer 12 répliques d'application, 10 pour le traitement du volume anticipé et 2 autres pour le traitement de la capacité de surcharge.

### Outre mon application, quels sont les autres éléments susceptibles d'utiliser les ressources du cluster ?
{: #sizing_other}

A présent, ajoutons d'autres fonctions que vous pourriez utiliser.



1.  Déterminez si votre application extrait des images volumineuses ou en grand nombre, ce qui peut nécessiter un stockage local sur le noeud worker.
2.  Décidez si vous souhaitez [intégrer des services](/docs/containers?topic=containers-supported_integrations#supported_integrations) dans votre cluster, par exemple, [Helm](/docs/containers?topic=containers-helm#public_helm_install) ou [Prometheus ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus). Ces services intégrés et modules complémentaires lancent des pods qui consomment des ressources de cluster.

### Quel type de disponibilité est souhaitable pour ma charge de travail ?
{: #sizing_availability}

N'oubliez pas que vous souhaitez disposer d'une charge de travail la plus élevée possible !

1.  Planifiez votre stratégie pour des [clusters hautement disponibles](/docs/containers?topic=containers-ha_clusters#ha_clusters), par exemple, faites un choix entre des clusters à zone unique ou à zones multiples.
2.  Passez en revue des [déploiements hautement disponibles](/docs/containers?topic=containers-app#highly_available_apps) afin de vous aider à décider de quelle façon rendre votre application disponible.

### De combien de noeuds worker ai-je besoin pour gérer ma charge de travail ?
{: #sizing_workers}

Maintenant que nous savons précisément à quoi ressemble votre charge de travail, mappons l'utilisation estimée sur vos configurations de cluster disponibles.

1.  Estimez la capacité maximale de noeud worker, laquelle dépend du type de cluster dont vous disposez. Vous ne voulez pas épuiser la capacité de noeud worker au cas où une surcharge ou un tout autre événement temporaire se produirait.
    *  **Clusters à zone unique** : prévoyez au moins 3 noeuds worker dans votre cluster. De plus, vous souhaitez 1 noeud supplémentaire de capacité d'unité centrale et de mémoire disponible dans le cluster.
    *  **Clusters à zones multiples** : prévoyez au moins 2 noeuds worker par zone, par conséquent, 6 noeuds sur 3 zones au total. En outre, planifiez la capacité totale du cluster à au moins 150 % de votre capacité requise totale de charge de travail, de sorte que si la zone 1 tombe en panne, vous disposiez de ressources disponibles pour la gestion de la charge de travail.
2.  Alignez la taille d'application et la capacité de noeud worker avec l'une des [versions de noeud worker disponibles](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes). Pour voir les versions disponibles dans une zone, exécutez `ibmcloud ks machine-types <zone>`.
    *   **Ne surchargez pas les noeuds worker** : pour éviter que les pods rivalisent pour l'unité centrale ou s'exécutent de manière inefficace, vous devez connaître les ressources requises par vos applications afin de pouvoir planifier le nombre de noeuds worker dont vous avez besoin. Par exemple, si vos applications requièrent un nombre de ressources inférieur au nombre de ressources disponibles sur le noeud worker, vous pouvez limiter le nombre de pods que vous déployez sur un noeud worker. Maintenez votre noeud worker à environ 75 % de capacité afin de laisser de l'espace pour les autres pods que vous pourriez avoir besoin de planifier. Si vos applications requièrent un nombre de ressources supérieur au nombre de ressources disponibles sur votre noeud worker, utilisez une autre version de noeud worker susceptible de répondre à ces exigences. Vous savez que vos noeuds worker sont surchargés lorsqu'ils rapportent fréquemment un état `NotReady` ou qu'ils rejettent des pods en raison du manque de mémoire ou de ressources supplémentaires.
    *   **Versions de noeud worker plus grandes ou plus petites** : les noeuds plus grands peuvent être plus économiques que les noeuds plus petits, en particulier pour les charges de travail qui sont conçues pour gagner en efficacité lorsqu'elles sont exécutées sur une machine à hautes performances. Toutefois, si un noeud worker de grande taille tombe en panne, vous devez faire en sorte que votre cluster dispose de suffisamment de capacité pour replanifier en douceur tous les pods de charge de travail sur d'autres noeuds worker du cluster. Un noeud worker plus petit peut vous aider à effectuer une mise à l'échelle plus en douceur.
    *   **Répliques de votre application** : pour déterminer le nombre de noeuds worker dont vous avez besoin, vous pouvez également prendre en compte le nombre de répliques de votre application que vous souhaitez exécuter. Par exemple, si vous savez que votre charge de travail requiert 32 coeurs d'unité centrale et que vous planifiez d'exécuter 16 répliques de votre application, chaque pod de réplique a besoin de 2 coeurs d'unité centrale. Si vous souhaitez n'exécuter qu'un seul pod par d'application par noeud worker, vous pouvez commander un nombre approprié de noeuds worker pour votre type de cluster afin de prendre en charge cette configuration.
3.  Exécutez des tests de performance pour continuer à affiner le nombre de noeuds worker dont vous besoin dans votre cluster, avec des exigences représentatives en termes de temps d'attente, d'évolutivité, de jeu de données et de charge de travail.
4.  Pour les charges de travail qui doivent être augmentées ou diminuées en fonction des demandes de ressource, configurez le [programme de mise à l'échelle automatique horizontale de pod](/docs/containers?topic=containers-app#app_scaling) et le [programme de mise à l'échelle automatique de pool worker de cluster](/docs/containers?topic=containers-ca#ca).

<br />


## Structuration de votre environnement Kubernetes
{: #kube_env}

Votre {{site.data.keyword.containerlong_notm}} est lié à un seul portefeuille de l'infrastructure IBM Cloud (SoftLayer). Dans votre compte, vous pouvez créer des clusters qui sont composés d'un maître et de plusieurs noeuds worker. IBM gère le maître et vous pouvez créer une combinaison de pools worker qui regroupent des machines individuelles ayant la même version ou les mêmes spécifications de mémoire et d'unité centrale. Au sein du cluster, vous pouvez organiser davantage des ressources par espaces de nom et libellés. Choisissez la bonne combinaison de clusters, types de machine et stratégies d'organisation de sorte que vos équipes et vos charges de travail puissent obtenir les ressources dont ils ont besoin.
{:shortdesc}

### Quel type de cluster et quels types de machine dois-je me procurer ?
{: #env_flavors}

**Types de cluster** : décidez si vous souhaitez une [configuration de plusieurs clusters, de cluster à zone unique ou  de cluster à zones multiples](/docs/containers?topic=containers-ha_clusters#ha_clusters). Les clusters à zones multiples sont disponibles dans [la totalité des six régions {{site.data.keyword.Bluemix_notm}} au niveau mondial](/docs/containers?topic=containers-regions-and-zones#zones). De plus, gardez à l'esprit que les noeuds worker varient d'une zone à l'autre.

**Types de noeuds worker** : en général, des machines physiques bare metal sont plus appropriées pour exécuter des charges de travail intensives, tandis que des machines virtuelles sur du matériel partagé ou partagé constituent une solution économique pour vos activités de test et de développement. Avec les noeuds worker bare metal, votre cluster dispose d'une vitesse réseau de 10Gbps et de coeurs HT offrant une capacité de traitement plus élevée. Les machines virtuelles sont livrées avec une vitesse réseau de 1 Gbps et des coeurs standard qui n'offrent pas la fonction HT. [Vérifiez l'isolement et les versions machine disponibles](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes).

### Dois-je utiliser plusieurs clusters ou simplement ajouter d'autres noeuds worker à un cluster existant ?
{: #env_multicluster}

Le nombre de clusters que vous créez dépend de votre charge de travail, des règles et de la réglementation en vigueur dans votre entreprise et de ce que vous souhaitez faire avec vos ressources informatiques. Vous pouvez également passer en revue les informations de sécurité dans la rubrique [Isolement et sécurité des conteneurs](/docs/containers?topic=containers-security#container).

**Plusieurs clusters** : vous devez configurer [un équilibreur de charge global](/docs/containers?topic=containers-ha_clusters#multiple_clusters) et copier et appliquer les mêmes fichiers YAML de configuration dans chacun d'eux afin d'équilibrer les charges de travail sur les clusters. Par conséquent, gérer plusieurs clusters est généralement plus complexe, mais cela peut vous permettre d'atteindre des objectifs importants, tels que ceux décrits ci-après.
*  Respecter les politiques de sécurité qui exigent d'isoler des charges de travail.
*  Tester le fonctionnement de votre application dans différentes versions de Kubernetes ou d'autres logiciels de cluster, tels que Calico.
*  Créer un cluster avec votre application dans une autre région pour obtenir de meilleures performances pour les utilisateurs situés dans cette zone géographique.
*  Configurer un accès utilisateur au niveau des instances de cluster au lieu de personnaliser et gérer plusieurs politiques de contrôle d'accès à base de rôles pour contrôler l'accès dans un cluster au niveau des espaces de nom.

**Moins de clusters ou clusters uniques** : gérer moins de clusters peut vous permettre de réduire vos efforts opérationnels et les coûts par cluster pour les ressources fixes. Au lieu de créer davantage de clusters, vous pouvez ajouter des pools worker pour différents types de machine de ressources informatiques disponibles pour les composants d'application et de service que vous souhaitez utiliser. Lorsque vous développez l'application, les ressources qu'elle utilise sont dans la même zone ou étroitement connectées dans une zone multiple, par conséquent, vous pouvez émettre des hypothèses sur les temps d'attente, la bande passante ou les incidents en rapport avec ces éléments. Toutefois, il est encore plus important vous d'organiser votre cluster à l'aide d'espaces de nom, de quotas de ressources et de libellés.

### Comment puis-je configurer mes ressources dans le cluster ?
{: #env_resources}

<dl>
<dt>Prenez en compte la capacité de votre noeud worker.</dt>
  <dd>Pour tirer le meilleur parti des performances de votre noeud worker, prenez en compte les points suivants :
  <ul><li><strong>Maintenir la puissance de vos coeurs</strong>: chaque machine comporte un certain nombre de coeurs. En fonction de la charge de travail de votre application, définissez une limite pour le nombre de pods par coeur, par exemple, 10.</li>
  <li><strong>Eviter de surcharger les noeuds</strong> : de même, ce n'est pas parce qu'un noeud peut contenir plus de 100 pods que cela doit effectivement être le cas. En fonction de la charge de travail de votre application, définissez une limite pour le nombre de pods par noeud, par exemple, 40.</li>
  <li><strong>Ne pas épuiser la bande passante de votre cluster</strong> : gardez à l'esprit que la bande passante du réseau lors de la mise à l'échelle des machines virtuelles est d'environ 1000 mégabits par seconde. Si vous avez besoin de centaines de noeuds worker dans un cluster, scindez ce dernier en plusieurs clusters comportant moins de noeuds ou commandez des noeuds bare metal.</li>
  <li><strong>Trier vos services</strong> : déterminez le nombre de services dont vous avez besoin pour votre charge de travail avant de procéder au déploiement. Les règles de mise en réseau et d'acheminement de port sont placées dans Iptables. Si vous prévoyez un nombre plus élevé de services, par exemple, plus de 5 000 services, scindez le cluster en plusieurs clusters.</li></ul></dd>
<dt>Mettez à disposition différents types de machines pour une combinaison de ressources informatiques.</dt>
  <dd>Tout le monde aime avoir le choix, non ? Avec {{site.data.keyword.containerlong_notm}}, vous pouvez déployer [une combinaison de différents types de machine](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) : cela va des machines bare metal, appropriées pour des charges de travail intensives, aux machines virtuelles qui offrent une mise à l'échelle rapide. Utilisez des libellés ou des espaces de nom pour organiser des déploiements sur vos machines. Lorsque vous créez un déploiement, limitez-le de sorte que le pod de votre application ne soit déployé que sur des machines disposant de la bonne combinaison de ressources. Par exemple, vous souhaiterez peut-être limiter une application de base de données à une machine bare metal avec une quantité significative de stockage sur disque local, par exemple, `md1c.28x512.4x4tb`.</dd>
<dt>Configurez plusieurs espaces de nom lorsque plusieurs équipes et projets se partagent le cluster.</dt>
  <dd><p>Les espaces de nom sont en quelque sorte un cluster au sein du cluster. Ils permettent de répartir les ressources de cluster à l'aide de [quotas de ressources ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/policy/resource-quotas/) et de [limites par défaut ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/). Lorsque vous créez de nouveaux espaces de nom, prenez soin de configurer les [règles RBAC](/docs/containers?topic=containers-users#rbac) appropriées pour contrôler les accès. Pour plus d'informations, voir [Share a cluster with namespaces ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/) dans la documentation Kubernetes.</p>
  <p>Si vous disposez d'un cluster de petite taille, de deux douzaines d'utilisateurs et de ressources qui sont similaires (par exemple, différentes versions d'un même logiciel), vous n'aurez probablement pas besoin de plusieurs espaces de nom. Vous pourrez utiliser des libellés à la place.</p></dd>
<dt>Définissez des quotas de ressources de sorte que les utilisateurs de votre cluster soient obligés d'utiliser des demandes et des limites de ressources</dt>
  <dd>Pour faire en sorte que chaque équipe dispose des ressources nécessaires pour déployer des services et exécuter des applications dans le cluster, vous devez établir des [quotas de ressources](https://kubernetes.io/docs/concepts/policy/resource-quotas/) pour tous les espaces de nom. Ces quotas de ressources déterminent les contraintes de déploiement d'un espace de nom, telles que le nombre de ressources Kubernetes que vous pouvez déployer, ainsi que la quantité d'UC et de mémoire pouvant être consommée par ces ressources. Après avoir défini un quota, les utilisateurs doivent inclure des limites et des demandes de ressources dans leurs déploiements.</dd>
<dt>Organisez vos objets Kubernetes à l'aide de libellés</dt>
  <dd><p>Pour organiser et sélectionner vos ressources Kubernetes, telles que des `pods` ou des `noeuds`, [utilisez des libellés Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/). Par défaut, {{site.data.keyword.containerlong_notm}} applique des libellés, notamment `arch`, `os`, `region`, `zone` et `machine-type`.</p>
  <p>Les libellés sont notamment utilisés pour [limiter le trafic réseau à des noeuds worker de périphérie](/docs/containers?topic=containers-edge), [déployer une application sur une machine GPU](/docs/containers?topic=containers-app#gpu_app) et [limiter les charges de travail de vos applications![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/) afin d'utiliser des noeuds worker qui répondent à un certain type de machine ou capacités SDS, par exemple, des noeuds worker bare metal. Pour voir quels sont les libellés déjà appliqués à une ressource, utilisez la commande <code>kubectl get</code> avec l'option <code>--show-labels</code>. Par exemple :</p>
  <p><pre class="pre"><code>kubectl get node &lt;node_ID&gt; --show-labels</code></pre></p>
  Pour appliquer des libellés à des noeuds worker, [créez votre pool de noeuds worker](/docs/containers?topic=containers-add_workers#add_pool) avec des libellés ou [mettez à jour un pool de noeuds worker existant](/docs/containers?topic=containers-add_workers#worker_pool_labels)</dd>
</dl>




<br />


## Procédure permettant de rendre vos ressources hautement disponibles
{: #kube_ha}

Bien qu'aucun système ne soit complètement infaillible, vous pouvez prendre des mesures pour augmenter la haute disponibilité de vos applications et services dans {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Prenez connaissance d'autres informations relatives aux procédures permettant de rendre des ressources hautement disponibles.
* [Réduisez les points de défaillance potentiels](/docs/containers?topic=containers-ha#ha).
* [Créez des clusters à zones multiples](/docs/containers?topic=containers-ha_clusters#ha_clusters).
* [Planifiez des déploiements hautement disponibles](/docs/containers?topic=containers-app#highly_available_apps) qui utilisent des fonctions, telles que les jeux de répliques et l'anti-affinité de pod dans les zones multiples.
* [Exécutez des conteneurs basés sur des images dans un registre public de type cloud](/docs/containers?topic=containers-images).
* [Prévoyez le stockage des données](/docs/containers?topic=containers-storage_planning#persistent_storage_overview). En particulier pour les clusters à zones multiples, envisagez d'utiliser un service de cloud, tel que [{{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) ou [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about).
* Pour les clusters à zones multiples, activez un [service d'équilibrage de charge](/docs/containers?topic=containers-loadbalancer#multi_zone_config) ou l'[équilibreur de charge pour zones multiples](/docs/containers?topic=containers-ingress#ingress) Ingress afin d'exposer vos applications au public.

<br />


## Configuration de la reconnaissance de service
{: #service_discovery}

Chacun de vos pods dans votre cluster Kubernetes possède une adresse IP. Mais lorsque vous déployez une application sur votre cluster, vous ne voulez pas vous en remettre à l'adresse IP du pod pour la reconnaissance de service et la mise en réseau. Les pods sont retirés et remplacés fréquemment et dynamiquement. A la place, utilisez un service Kubernetes, qui représente un groupe de pods et fournit un point d'entrée stable via l'adresse IP virtuelle du service, que l'on appelle `son adresse IP de cluster`. Pour plus d'informations, voir la documentation Kubernetes relatives à la [reconnaissance des services ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/services-networking/service/#discovering-services).
{:shortdesc}

### Puis-je personnaliser le fournisseur DNS de cluster Kubernetes ?
{: #services_dns}

Lorsque vous créez des services et des pods, un nom DNS leur est affecté de sorte que vos conteneurs d'applications puissent utiliser l'adresse IP du service DNS pour résoudre les noms DNS. Vous pouvez personnaliser le DND de pod pour spécifier des serveurs de noms, des recherches et des options de liste d'objets. Pour plus d'informations, voir [Configuration du fournisseur DNS de cluster](/docs/containers?topic=containers-cluster_dns#cluster_dns).
{: shortdesc}



### Comment faire pour m'assurer que mes services sont connectés aux déploiements qui conviennent et qu'ils sont prêts ?
{: #services_connected}

Pour la plupart des services, ajoutez un sélecteur à votre fichier `.yaml` de service de telle manière qu'il soit appliqué aux pods qui exécutent vos applications via ce libellé. Souvent, lorsque votre application démarre pour la première fois, vous ne voulez pas qu'elle traite des demandes immédiatement. Ajoutez une sonde Readiness Probe à votre déploiement de sorte que le trafic soit envoyé uniquement à un pod qui est considéré comme prêt. Pour voir un exemple de déploiement avec un service qui utilise des libellés et définit une zone Readiness Probe, reportez-vous au contenu suivant : [NGINX YAML](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml).
{: shortdesc}

Parfois, vous ne souhaitez pas que le service utilise un libellé. Vous pouvez par exemple disposer d'une base de données externe ou souhaiter faire pointer le service vers un autre service situé dans un autre espace de nom au sein du cluster. Dans ce cas, vous devez ajouter manuellement un objet de noeud final et le lier au service.


### Comment contrôler le trafic réseau entre les services qui s'exécutent dans mon cluster ?
{: #services_network_traffic}

Par défaut, les pods peuvent communiquer avec d'autres pods dans le cluster, mais vous pouvez bloquer le trafic vers certains pods ou espaces de nom à l'aide de règles réseau. En outre, si vous exposez votre application de façon externe en utilisant un service NodePort, LoadBalancer ou Ingress, vous souhaiterez peut-être configurer des règles réseau avancées pour bloquer le trafic. Dans {{site.data.keyword.containerlong_notm}}, vous pouvez utiliser Calico pour gérer des [règles réseau Kubernetes et Calico afin de contrôler le trafic](/docs/containers?topic=containers-network_policies#network_policies).

Si vous disposez d'une grande variété de microservices qui s'exécutent sur les plateformes pour lesquelles vous devez connecter, gérer et sécuriser le trafic réseau, pensez à utiliser un outil de maillage de service, tel que le [module complémentaire Istio géré](/docs/containers?topic=containers-istio).

Vous pouvez également [configurer des noeuds de périphérie](/docs/containers?topic=containers-edge#edge) pour augmenter la sécurité et l'isolement de votre cluster en limitant la charge de travail de mise en réseau à certains noeuds worker définis.



### Comment puis-je exposer mes services sur Internet ?
{: #services_expose_apps}

Vous pouvez créer trois types de services pour la mise en réseau externe : NodePort, LoadBalancer et Ingress. Pour plus d'informations, voir [Planification des services de mise en réseau](/docs/containers?topic=containers-cs_network_planning#external).

Lorsque vous déterminez le nombre d'objets `Service` dont vous avez besoin dans votre cluster, gardez à l'esprit que Kubernetes utilise `iptables` pour gérer les règles de mise en réseau et d'acheminement de port. Si vous exécutez un nombre important de services dans votre cluster, par exemple, 5000, les performances peuvent être dégradées.



## Déploiement de charges de travail d'application sur des clusters
{: #deployments}

Avec Kubernetes, vous déclarez le nombre de types d'objets dans les fichiers de configuration YAML, par exemple, des pods, des déploiements et des travaux. Ces objets décrivent par exemple les applications conteneurisées qui sont en cours d'exécution, les ressources utilisées par ces applications et les règles qui gèrent leur comportement en matière de redémarrage, de mise à jour, de réplication, etc. Pour plus d'informations, voir les documents Kubernetes relatifs aux [meilleures pratiques de configuration ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/configuration/overview/).
{: shortdesc}

### Je croyais que je devais placer mon application dans un conteneur. C'est quoi cette histoire de pods ?
{: #deploy_pods}

Un [pod ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/pods/pod/) est la plus petite unité déployable pouvant être gérée par Kubernetes. Vous placez votre conteneur (ou un groupe de conteneurs) dans un pod et utilisez le fichier de configuration de pod pour indiquer au pod comment configurer le conteneur et partager des ressources avec d'autres pods. Tout les conteneurs que vous placez dans un pod sont exécutés dans un contexte partagé, ce qui signifie qu'ils partagent la même machine virtuelle ou physique.
{: shortdesc}

**Eléments à placer dans un conteneur** : lorsque vous pensez aux composants de votre application, demandez-vous s'ils ont des besoins en ressources radicalement différents, par exemple, en termes d'unité centrale et de mémoire. Certains composants peuvent-ils s'exécuter dans les meilleures conditions possibles, où il serait acceptable de les mettre hors service pendant un petit moment pendant que des ressources sont détournées vers d'autres zones. Un autre composant est-il destiné au client, auquel cas, il doit impérativement rester actif ? Répartissez-les dans des conteneurs distincts. Vous pouvez toujours les déployer sur le même pod de sorte qu'ils s'exécutent ensemble de manière synchronisée.

**Eléments à placer dans un pod** : il n'est pas nécessaire que les conteneurs pour votre application se trouvent dans le même pod. En fait, si vous disposez d'un composant qui est avec état et difficile à mettre à l'échelle, par exemple un service de base de données, placez-le dans un autre pod que vous pouvez planifier sur un noeud worker avec davantage de ressources pour gérer la charge de travail. Si vos conteneurs fonctionnent correctement s'ils s'exécutent sur différents noeuds worker, utilisez plusieurs pods. S'ils doivent se trouver sur la même machine et être mis à l'échelle ensemble, regroupez-les dans le même pod.

### Si je peux simplement utiliser un pod, pourquoi ai-je besoin de tous ces différents types d'objets ?
{: #deploy_objects}

Créer un fichier YAML de pod est facile à faire. Vous pouvez en écrire un avec juste quelques lignes, comme suit :

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
{: codeblock}

Mais, vous ne voulez pas vous arrêter là. Si le noeud exécuté par votre pod tombe en panne, ce dernier tombe également en panne et il n'est pas replanifié. A la place, utilisez un  [déploiement![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) pour prendre en charge la replanification de pod, les jeux de répliques et les mises à jour en continu. Un déploiement de base est pratiquement aussi facile à réaliser qu'un pod. Au lieu de définir le conteneur dans la spécification (`spec`) proprement dite, vous spécifiez des répliques (`replicas`) et un modèle (`template`) dans la spécification (`spec`) de déploiement. Le modèle possède sa propre spécification (`spec`) pour les conteneurs qu'il contient, par exemple :

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```
{: codeblock}

Vous pouvez continuer d'ajouter des fonctions, par exemple, l'anti-affinité de pod ou des limites de ressource, le tout dans le même fichier YAML.

Pour plus d'informations sur les différentes fonctions que vous pouvez ajouter à votre déploiement, voir [Création du fichier YAML pour le déploiement de votre application](/docs/containers?topic=containers-app#app_yaml).
{: tip}

### Comment puis-je organiser mes déploiements pour faciliter leur mise à jour et leur gestion ?
{: #deploy_organize}

Maintenant que nous savez précisément ce que vous devez ajouter à votre déploiement, vous vous demandez sûrement comment faire pour gérer tous ces fichiers YAML ? Sans parler des objets qu'ils créent dans votre environnement Kubernetes !

Quelques conseils pour organiser les fichiers YAML de déploiement
*  Utilisez un système de contrôle des versions, par exemple Git.
*  Regroupez des objets Kubernetes connexes dans un seul fichier YAML. Par exemple, si vous créez un `déploiement`, vous pouvez être amené à ajouter également le fichier `service` au fichier YAML. Séparez les objets par trois tirets (`---`) comme illustré ci-dessous :
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    ...
    ---
    apiVersion: v1
    kind: Service
    metadata:
    ...
    ```
    {: codeblock}
*  Vous pouvez utiliser la commande `kubectl apply -f` pour une application à la totalité d'un répertoire et non simplement à un fichier.
*  Utilisez le [projet `kustomize` ](/docs/containers?topic=containers-app#kustomize) qui peut vous aider à écrire, personnaliser et réutiliser vos configurations YAML de ressource Kubernetes. 

Dans le fichier YAML, vous pouvez utiliser des libellés ou des annotations comme métadonnées pour gérer vos déploiements.

**Libellés** : les [libellés ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) sont des paires `key:value` qui peuvent être associées à des objets Kubernetes, tels que des pods et des déploiements. Ils peuvent être tout ce que vous voulez et sont utiles pour sélectionner des objets en fonction des informations de libellé. Les libellés fournissent la base du regroupement d'objets. Voici quelques idées de libellé :
* `app: nginx`
* `version: v1`
* `env: dev`

**Annotations** : les [annotations ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) sont semblables à des libellés dans la mesure où elles sont également des paires `key:value`. Elles sont plutôt recommandées pour des informations non identifiantes qui peuvent être optimisées par des outils ou des bibliothèques, par exemple, pour des informations supplémentaires relatives à la provenance d'un objet, à l'utilisation de l'objet, à des pointeurs vers des référentiels de suivi connexes ou une règle concernant l'objet. Vous ne sélectionnez pas des objets basés sur des annotations.

### Que puis-je faire d'autre pour préparer mon application au déploiement ?
{: #deploy_prep}

Beaucoup de choses ! Reportez-vous à la rubrique [Préparation de votre application conteneurisée pour une exécution dans des clusters](/docs/containers?topic=containers-app#plan_apps). Cette rubrique contient des informations sur les sujets suivants :
*  Types d'applications que vous pouvez exécuter dans kubernetes, y compris des astuces pour les applications avec état et sans état.
*  Migration d'applications vers kubernetes.
*  Dimensionnement de votre cluster en fonction de vos exigences de charge de travail.
*  Configuration de ressources d'application supplémentaires, par exemple, les services IBM, le stockage, la consignation et la surveillance.
*  Utilisation de variables dans votre déploiement.
*  Contrôle de l'accès à votre application.

<br />


## Conditionnement de votre application
{: #packaging}

Si vous souhaitez exécuter votre application dans plusieurs clusters, plusieurs environnements privés et publics, ou même dans plusieurs fournisseurs de cloud, vous vous demandez peut-être comment faire pour que votre stratégie de déploiement fonctionnent dans tous ces environnements. Grâce à {{site.data.keyword.Bluemix_notm}} et à d'autres outils open source, vous pouvez conditionner votre application de manière à automatiser les déploiements.
{: shortdesc}

<dl>
<dt>Automatisez votre infrastructure</dt>
  <dd>Vous pouvez utiliser l'outil [Terraform](/docs/terraform?topic=terraform-getting-started#getting-started) open source pour automatiser la mise à disposition de l'infrastructure {{site.data.keyword.Bluemix_notm}}, y compris les clusters Kubernetes. Suivez ce tutoriel pour [planifier, créer et mettre à jour des environnements de déploiement](/docs/tutorials?topic=solution-tutorials-plan-create-update-deployments#plan-create-update-deployments). Après avoir créé un cluster, vous pouvez également configurer un [programme de mise à l'échelle automatique de cluster {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ca) de sorte que votre pool worker puisse augmenter et réduire le nombre de noeuds worker en fonction des demandes de ressources de votre charge de travail.</dd>
<dt>Configurez un pipeline d'intégration continue et de distribution continue (CI/CD)</dt>
  <dd>Avec vos fichiers de configuration d'application organisés dans un système de gestion de contrôle de source tel que Git, vous pouvez créer votre pipeline afin de tester et déployer du code dans différents environnements, par exemple, `test` et `prod`. Suivez [ce tutoriel sur le déploiement continu dans Kubernetes](/docs/tutorials?topic=solution-tutorials-continuous-deployment-to-kubernetes#continuous-deployment-to-kubernetes).</dd>
<dt>Conditionnez vos fichiers de configuration d'application</dt>
  <dd>Grâce au [gestionnaire de package Kubernetes Helm ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://helm.sh/docs/), vous pouvez spécifier toutes les ressources Kubernetes dont votre application a besoin dans une charte Helm. Ensuite, vous pouvez utiliser Helm pour créer les fichiers de configuration YAML et déployer ces fichiers dans votre cluster. Vous pouvez également [intégrer des chartes Helm fournies par {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) afin d'étendre les capacités de votre cluster, par exemple, avec un plug-in de stockage par blocs.<p class="tip">Recherchez-vous simplement un moyen de créer facilement des modèles de fichier YAML ? Certaines personnes utilisent Helm pour cela, mais vous pouvez aussi essayer d'autres outils de la communauté, tels que [`ytt` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://get-ytt.io/).</p></dd>
</dl>

<br />


## Procédure permettant de maintenir à jour votre application
{: #updating}

Vous faites beaucoup d'efforts pour préparer la réception de la version suivante de votre application. Vous pouvez utiliser les outils de mise à jour d'{{site.data.keyword.Bluemix_notm}} et de Kubernetes pour faire en sorte que votre application s'exécute dans un environnement de cluster sécurisé, et pour déployer différentes versions de votre application.
{: shortdesc}

### Comment puis-je maintenir mon cluster dans un état pris en charge ?
{: #updating_kube}

Assurez-vous que votre cluster exécute toujours une [version de Kubernetes qui est prise en charge](/docs/containers?topic=containers-cs_versions#cs_versions). Lorsqu'une nouvelle version mineure de Kubernetes est publiée, une version plus ancienne est rapidement dépréciée et n'est plus prise en charge. Pour plus d'informations, voir [Mise à jour du maître Kubernetes](/docs/containers?topic=containers-update#master) et des [noeuds worker](/docs/containers?topic=containers-update#worker_node).

### Quelles stratégies de mise à jour d'application puis-je utiliser ?
{: #updating_apps}

Pour mettre à jour votre application, vous pouvez choisir parmi différentes stratégies, telles que celles décrites ci-après. Vous pouvez commencer par un déploiement en continu ou un basculement instantané avant de passer à un déploiement cobaye plus complexe.

<dl>
<dt>Déploiement en continu</dt>
  <dd>Vous pouvez utiliser une fonctionnalité native Kubernetes pour créer un déploiement `v2` et pour remplacer progressivement votre précédent déploiement `v1`. Cette approche nécessite que les applications soient rétrocompatibles afin de permettre aux utilisateurs qui reçoivent la version d'application `v2` de ne subir aucune modification avec rupture. Pour plus d'informations, voir [Gestion des déploiements en continu pour mettre à jour vos applications](/docs/containers?topic=containers-app#app_rolling).</dd>
<dt>Basculement instantané</dt>
  <dd>Egalement appelé déploiement Blue-Green, un basculement instantané nécessite de doubler les ressources informatiques de sorte que deux versions d'une application s'exécutent en même temps. Avec cette approche, vous pouvez faire passer vos utilisateurs à la version plus récente quasiment en temps réel. Prenez soin d'utiliser des sélecteurs de libellé de service (par exemple `version: green` et `version: blue`) pour être certain que les demandes sont envoyées à la version d'application appropriée. Vous pouvez créer le nouveau déploiement `version: green`, attendre qu'il soit prêt, puis supprimer le déploiement `version: blue`. Ou, vous pouvez effectuer une [mise à jour en continu](/docs/containers?topic=containers-app#app_rolling), mais affecter la valeur `0%` au paramètre `maxUnavailable` et la valeur `100%` au paramètre `maxSurge`.
<dt>Déploiement A/B ou cobaye</dt>
  <dd>Une stratégie de mise à jour plus complexe, un déploiement cobaye, est lorsque vous choisissez un pourcentage d'utilisateurs, par exemple, 5 % et que vous les envoyez à la nouvelle version d'application. Vous collectez dans vos outils de consignation et de surveillance des métriques relatives aux performances de la nouvelle version d'application, vous effectuez des tests A/B, puis vous déployez la mise à jour à d'autres utilisateurs. Comme pour tous les déploiements, spécifier un libellé pour l'application (par exemple, `version: stable` et `version: canary`) est essentiel. Pour gérer des déploiements cobaye, vous devrez peut-être [installer le maillage de service complémentaire Istio géré](/docs/containers?topic=containers-istio#istio),  [configurer la surveillance Sysdig pour votre cluster](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster), puis utiliser le maillage de service Istio pour les tests A/B, comme indiqué [dans cet article de blogue![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://sysdig.com/blog/monitor-istio/). Ou bien, utilisez Knative pour des déploiements cobaye. </dd>
</dl>

<br />


## Surveillance des performances de votre cluster
{: #monitoring_health}

Une consignation et une surveillance efficaces de votre cluster et de vos applications vous permettent de mieux comprendre votre environnement afin d'optimiser l'utilisation de vos ressources et de traiter les problèmes qui peuvent survenir. Pour configurer des solutions de consignation et de surveillance pour votre cluster, voir [Consignation et surveillance](/docs/containers?topic=containers-health#health).
{: shortdesc}

Lorsque vous configurez votre consignation et votre surveillance, tenez compte des considérations suivantes :

<dl>
<dt>Collecte de journaux et de métriques pour déterminer l'état de santé du cluster</dt>
  <dd>Kubernetes comprend un serveur de métriques destiné à vous aider à déterminer les performances de base du cluster. Vous pouvez passer en revue ces métriques dans votre [tableau de bord Kubernetes](/docs/containers?topic=containers-app#cli_dashboard) ou dans un terminal en exécutant des commandes `kubectl top (pods | nodes)`. Vous pouvez inclure ces commandes dans votre automatisation.<br><br>
  Transférez les journaux vers un outil d'analyse de journal de manière à pouvoir analyser vos journaux ultérieurement. Définissez la prolixité et le niveau de consignation afin d'éviter de stocker plus de journaux que nécessaire. Les journaux peuvent rapidement consommer beaucoup de stockage, ce qui peut avoir un impact sur vos applications et peut rendre l'analyse de journal plus difficile.</dd>
<dt>Test des performances d'application</dt>
  <dd>Après avoir configuré la consignation et la surveillance, effectuez des tests de performance. Dans un environnement de test, créez volontairement une grande variété de scénarios non idéaux, par exemple en supprimant tous les noeuds worker d'une zone afin de répliquer la défaillance d'une zone. Passez en revue les journaux et les métriques pour voir de quelle façon votre application récupère.</dd>
<dt>Préparation pour des audits</dt>
  <dd>En plus des journaux d'application et des métriques de cluster, vous souhaitez configurer le suivi des activités de manière à obtenir un enregistrement auditable décrivant qui a effectué quelles actions sur le cluster et dans Kubernetes. Pour plus d'informations, voir [{{site.data.keyword.cloudaccesstrailshort}}](/docs/containers?topic=containers-at_events#at_events).</dd>
</dl>
