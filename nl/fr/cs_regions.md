---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

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

# Emplacements
{: #regions-and-zones}

Vous pouvez déployer des clusters {{site.data.keyword.containerlong}} dans le monde entier. Lorsque vous créez un cluster Kubernetes, ses ressources restent dans l'emplacement où vous avez déployé le cluster. Vous pouvez accéder à {{site.data.keyword.containerlong_notm}} via un noeud final d'API global afin de gérer votre cluster.
{:shortdesc}

![Emplacements {{site.data.keyword.containerlong_notm}}](images/locations.png)

_Emplacements {{site.data.keyword.containerlong_notm}}_

Les ressources {{site.data.keyword.Bluemix_notm}} utilisées doivent être organisées en régions accessibles via des [noeuds finaux propres aux régions](#bluemix_regions). Utilisez le [noeud final global](#endpoint) à la place.
{: deprecated}

## Emplacements {{site.data.keyword.containerlong_notm}}
{: #locations}

Les ressources {{site.data.keyword.Bluemix_notm}} sont organisées dans une hiérarchie d'emplacements géographiques. {{site.data.keyword.containerlong_notm}} est disponible dans un sous-ensemble de ces emplacements, y compris les six régions compatibles avec plusieurs zones présentes dans le monde entier. Les clusters gratuits ne sont disponibles que dans certains emplacements. D'autres services {{site.data.keyword.Bluemix_notm}} peuvent être disponibles partout ou dans un emplacement spécifique.
{: shortdesc}

### Emplacements disponibles
{: #available-locations}

Pour répertorier les emplacements {{site.data.keyword.containerlong_notm}} disponibles, utilisez la commande `ibmcloud ks supported-locations`.
{: shortdesc}

L'image suivante permet d'illustrer la façon dont les emplacements {{site.data.keyword.containerlong_notm}} sont organisés :

![Organisation des emplacements {{site.data.keyword.containerlong_notm}}](images/cs_regions_hierarchy.png)

<table summary="Le tableau présente l'organisation des emplacements {{site.data.keyword.containerlong_notm}}. La lecture des lignes s'effectue de gauche à droite, avec le type d'emplacement dans la première colonne, un exemple de type dans la deuxième colonne et une description dans la troisième colonne.">
<caption>Organisation des emplacements {{site.data.keyword.containerlong_notm}}.</caption>
  <thead>
  <th>Type</th>
  <th>Exemple</th>
  <th>Description</th>
  </thead>
  <tbody>
    <tr>
      <td>Zone géographique</td>
      <td>Amérique du Nord (`na`)</td>
      <td>Groupe organisationnel basé sur des continents géographiques.</td>
    </tr>
    <tr>
      <td>Pays</td>
      <td>Canada (`ca`)</td>
      <td>Pays d'un emplacement dans la zone géographique.</td>
    </tr>
    <tr>
      <td>Agglomération</td>
      <td>Mexico City (`mex-cty`), Dallas (`dal`)</td>
      <td>Nom d'une ville dans laquelle se trouve au moins un centre de données (zone). Une agglomération peut être compatible avec plusieurs zones et comporter plusieurs centres de données compatibles avec plusieurs zones, par exemple, Dallas, ou comporter des centres de données à zone unique, par exemple, Mexico City. Si vous créez un cluster dans une agglomération compatible avec plusieurs zones, le maître et les noeuds worker Kubernetes peuvent être répartis entre les zones pour assurer la haute disponibilité.</td>
    </tr>
    <tr>
      <td>Centre de données (zone)</td>
      <td>Dallas 12 (`dal12`)</td>
      <td>Emplacement physique de l'infrastructure de calcul, de réseau et de stockage et des systèmes de refroidissement et des appareils électriques hébergeant les services et les applications. Les clusters peuvent être répartis entre plusieurs centres de données, ou zones, dans une architecture à zones multiples pour assurer la haute disponibilité. Les zones sont isolées les unes des autres pour garantir qu'aucun point de défaillance unique ne sera partagé.</td>
    </tr>
  </tbody>
  </table>

### Emplacements à zone unique et à zones multiples dans {{site.data.keyword.containerlong_notm}}
{: #zones}

Les tableaux suivants répertorient les emplacements à zone unique et à zones multiples qui sont disponibles dans {{site.data.keyword.containerlong_notm}}. Notez que dans certaines agglomérations, vous pouvez mettre à disposition un cluster en tant que cluster à zone unique ou à zones multiples. En outre, les clusters gratuits sont disponibles uniquement dans certaines zones géographiques en tant que clusters à zone unique dotés d'un noeud worker.
{: shortdesc}

* **Zones multiples** : si vous créez un cluster dans une agglomération à zones multiples, les répliques de votre maître Kubernetes à haute disponibilité sont automatiquement réparties entre les différentes zones. Vous avez la possibilité de répartir vos noeuds worker entre les zones pour protéger vos applications en cas de défaillance d'une zone.
* **Zone unique** : si vous créez un cluster dans un centre de données unique, vous pouvez créer plusieurs noeuds worker, mais vous ne pouvez pas les répartir sur plusieurs zones. Le maître à haute disponibilité comprend trois répliques sur des hôtes distincts, mais n'est pas réparti entre les zones.

Pour déterminer rapidement si une zone est compatible avec plusieurs zones, vous pouvez exécuter la commande `ibmcloud ks supported-locations` et rechercher la valeur dans la colonne `Multizone Metro`.
{: tip}


Les ressources {{site.data.keyword.Bluemix_notm}} utilisées doivent être organisées en régions accessibles via des [noeuds finaux propres aux régions](#bluemix_regions). Les tableaux répertorient les précédentes régions à titre d'information. Dorénavant, pour pouvez utiliser le [noeud final global](#endpoint) pour passer à une architecture qui est moins basée sur les régions.
{: deprecated}

**Emplacements d'agglomération à zones multiples**

<table summary="Le tableau présente les emplacements des agglomérations à zones multiples qui sont disponibles dans {{site.data.keyword.containerlong_notm}}. La lecture des lignes s'effectue de gauche à droite. La première colonne contient la zone géographique dans laquelle se trouve l'emplacement, la deuxième colonne contient le pays de l'emplacement, la troisième colonne contient l'agglomération de l'emplacement, la quatrième colonne contient le centre de données et la cinquième colonne contient la région dépréciée dans laquelle l'emplacement était organisé.">
<caption>Emplacements des agglomérations à zones multiples disponibles dans {{site.data.keyword.containerlong_notm}}.</caption>
  <thead>
  <th>Zone géographique</th>
  <th>Pays</th>
  <th>Agglomération</th>
  <th>Centre de données</th>
  <th>Région dépréciée</th>
  </thead>
  <tbody>
    <tr>
      <td>Asie-Pacifique</td>
      <td>Australie</td>
      <td>Sydney</td>
      <td>syd01, syd04, syd05</td>
      <td>Asie-Pacifique Sud (`ap-south`, `au-syd`)</td>
    </tr>
    <tr>
      <td>Asie-Pacifique</td>
      <td>Japon</td>
      <td>Tokyo</td>
      <td>tok02, tok04, tok05</td>
      <td>Asie-Pacifique Nord (`ap-north`, `jp-tok`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>Allemagne</td>
      <td>Francfort</td>
      <td>fra02, fra04, fra05</td>
      <td>Europe centrale (`eu-central`, `eu-de`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>Royaume-uni</td>
      <td>Londres</td>
      <td>lon04, lon05`*`, lon06</td>
      <td>Sud du Royaume-Uni (`uk-south`, `eu-gb`)</td>
    </tr>
    <tr>
      <td>Amérique du Nord</td>
      <td>Etats-Unis</td>
      <td>Dallas</td>
      <td>dal10, dal12, dal13</td>
      <td>Sud des Etats-Unis (`us-south`)</td>
    </tr>
    <tr>
      <td>Amérique du Nord</td>
      <td>Etats-Unis</td>
      <td>Washington, D.C.</td>
      <td>wdc04, wdc06, wdc07</td>
      <td>Est des Etats-Unis (`us-east`)</td>
    </tr>
  </tbody>
  </table>

**Emplacements de centre de données à zone unique**

<table summary="Le tableau présente les emplacements de centre de données à zone unique qui sont disponibles dans {{site.data.keyword.containerlong_notm}}. La lecture des lignes s'effectue de gauche à droite. La première colonne contient la zone géographique dans laquelle se trouve l'emplacement, la deuxième colonne contient le pays de l'emplacement, la troisième colonne contient l'agglomération de l'emplacement, la quatrième colonne contient le centre de données et la cinquième colonne contient la région dépréciée dans laquelle l'emplacement était organisé.">
<caption>Emplacements de centre de données à zone unique disponibles dans {{site.data.keyword.containerlong_notm}}.</caption>
  <thead>
  <th>Zone géographique</th>
  <th>Pays</th>
  <th>Agglomération</th>
  <th>Centre de données</th>
  <th>Région dépréciée</th>
  </thead>
  <tbody>
    <tr>
      <td>Asie-Pacifique</td>
      <td>Australie</td>
      <td>Melbourne</td>
      <td>mel01</td>
      <td>Asie-Pacifique Sud (`ap-south`, `au-syd`)</td>
    </tr>
    <tr>
      <td>Asie-Pacifique</td>
      <td>Australie</td>
      <td>Sydney</td>
      <td>syd01, syd04, syd05</td>
      <td>Asie-Pacifique Sud (`ap-south`, `au-syd`)</td>
    </tr>
    <tr>
      <td>Asie-Pacifique</td>
      <td>Chine</td>
      <td>Hong Kong<br>RAS de République populaire de Chine</td>
      <td>hkg02</td>
      <td>Asie-Pacifique Nord (`ap-north`, `jp-tok`)</td>
    </tr>
    <tr>
      <td>Asie-Pacifique</td>
      <td>Inde</td>
      <td>Chennai</td>
      <td>che01</td>
      <td>Asie-Pacifique Nord (`ap-north`, `jp-tok`)</td>
    </tr>
    <tr>
      <td>Asie-Pacifique</td>
      <td>Japon</td>
      <td>Tokyo</td>
      <td>tok02, tok04, tok05</td>
      <td>Asie-Pacifique Nord (`ap-north`, `jp-tok`)</td>
    </tr>
    <tr>
      <td>Asie-Pacifique</td>
      <td>Corée</td>
      <td>Séoul</td>
      <td>seo01</td>
      <td>Asie-Pacifique Nord (`ap-north`, `jp-tok`)</td>
    </tr>
    <tr>
      <td>Asie-Pacifique</td>
      <td>Singapour</td>
      <td>Singapour</td>
      <td>sng01</td>
      <td>Asie-Pacifique Nord (`ap-north`, `jp-tok`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>France</td>
      <td>Paris</td>
      <td>par01</td>
      <td>Europe centrale (`eu-central`, `eu-de`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>Allemagne</td>
      <td>Francfort</td>
      <td>fra02, fra04, fra05</td>
      <td>Europe centrale (`eu-central`, `eu-de`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>Italie</td>
      <td>Milan</td>
      <td>mil01</td>
      <td>Europe centrale (`eu-central`, `eu-de`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>Pays-Bas</td>
      <td>Amsterdam</td>
      <td>ams03</td>
      <td>Europe centrale (`eu-central`, `eu-de`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>Norvège</td>
      <td>Oslo</td>
      <td>osl</td>
      <td>Europe centrale (`eu-central`, `eu-de`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>Royaume-uni</td>
      <td>Londres</td>
      <td>lon02`*`, lon04, lon05`*`, lon06</td>
      <td>Sud du Royaume-Uni (`uk-south`, `eu-gb`)</td>
    </tr>
    <tr>
      <td>Amérique du Nord</td>
      <td>Canada</td>
      <td>Montréal</td>
      <td>mon01</td>
      <td>Est des Etats-Unis (`us-east`)</td>
    </tr>
    <tr>
      <td>Amérique du Nord</td>
      <td>Canada</td>
      <td>Toronto</td>
      <td>tor01</td>
      <td>Est des Etats-Unis (`us-east`)</td>
    </tr>
    <tr>
      <td>Amérique du Nord</td>
      <td>Mexique</td>
      <td>Mexico City</td>
      <td>mex01</td>
      <td>Sud des Etats-Unis (`us-south`)</td>
    </tr>
    <tr>
      <td>Amérique du Nord</td>
      <td>Etats-Unis</td>
      <td>Dallas</td>
      <td>dal10, dal12, dal13</td>
      <td>Sud des Etats-Unis (`us-south`)</td>
    </tr>
    <tr>
      <td>Amérique du Nord</td>
      <td>Etats-Unis</td>
      <td>San Jose</td>
      <td>sjc03, sjc04</td>
      <td>Sud des Etats-Unis (`us-south`)</td>
    </tr>
    <tr>
      <td>Amérique du Nord</td>
      <td>Etats-Unis</td>
      <td>Washington, D.C.</td>
      <td>wdc04, wdc06, wdc07</td>
      <td>Est des Etats-Unis (`us-east`)</td>
    </tr>
    <tr>
      <td>Amérique du Sud</td>
      <td>Brésil</td>
      <td>São Paulo</td>
      <td>sao01</td>
      <td>Sud des Etats-Unis (`us-south`)</td>
    </tr>
  </tbody>
  </table>

`*` lon05 remplace lon02. Les nouveaux clusters doivent utiliser lon05, et les maîtres à haute disponibilité répartis entre les zones ne sont pris en charge que dans lon05.
{: note}

### Clusters à zone unique
{: #regions_single_zone}

Dans un cluster à zone unique, les ressources de votre cluster restent dans la zone dans laquelle le cluster est déployé. L'image suivante met en évidence les relations entre les composants d'un cluster à zone unique avec l'emplacement Toronto, Canada `tor01`.
{: shortdesc}

<img src="/images/location-cluster-resources.png" width="650" alt="Présentation de l'emplacement des ressources de votre cluster" style="width:650px; border-style: none"/>

_Présentation de l'emplacement des ressources de votre cluster à zone unique_

1.  Les ressources de votre cluster, notamment le maître et les noeuds worker, sont situées dans le centre de données où vous avez déployé le cluster. Lorsque vous initiez des actions d'orchestration de conteneurs locaux, par exemple des commandes `kubectl`, les informations s'échangent entre le maître et vos noeuds worker dans la même zone.

2.  Si vous configurez d'autres ressources de cluster, par exemple du stockage, des ressources réseau, du calcul ou des applications qui s'exécutent dans des pods, les ressources et leurs données restent dans la zone dans laquelle vous avez déployé votre cluster.

3.  Lorsque vous initiez des actions de gestion de cluster, par exemple l'exécution de commandes `ibmcloud ks`, les informations de base sur le cluster (par exemple le nom, l'ID, l'utilisateur, la commande) sont acheminées au moyen d'un noeud final régional via le noeud final global. Les noeuds finaux régionaux se situent dans l'agglomération à zones multiples la plus proche. Dans cet exemple, l'agglomération est Washington, D.C.

### Clusters à zones multiples
{: #regions_multizone}

Dans un cluster à zones multiples, les ressources de votre cluster sont réparties sur plusieurs zones pour assurer la haute disponibilité.
{: shortdesc}

1.  Les noeuds worker sont répartis sur plusieurs zones d'une agglomération pour offrir une plus grande disponibilité à votre cluster. Les répliques du maître Kubernetes sont également réparties sur les zones. Lorsque vous initiez des actions d'orchestration de conteneurs locaux, par exemple avec des commandes `kubectl`, les informations s'échangent entre le maître et vos noeuds worker via le noeud final global.

2.  D'autres ressources de cluster, comme par exemple du stockage, des ressources réseau, du calcul ou des applications s'exécutant dans des pods, varient par leur manière de se déployer dans les zones de votre cluster à zones multiples. Pour plus d'informations, consultez les rubriques suivantes :
    *   Configuration de [stockage de fichiers](/docs/containers?topic=containers-file_storage#add_file) et de [stockage par blocs](/docs/containers?topic=containers-block_storage#add_block) dans les clusters à zones multiples, ou [Choix d'une solution de stockage persistant à zones multiples](/docs/containers?topic=containers-storage_planning#persistent_storage_overview).
    *   [Activation de l'accès public ou privé à une application à l'aide d'un service d'équilibreur de charge de réseau (NLB) dans un cluster à zones multiples](/docs/containers?topic=containers-loadbalancer#multi_zone_config)
    *   [Gestion de trafic réseau à l'aide d'Ingress](/docs/containers?topic=containers-ingress#planning)
    *   [Augmentation de la disponibilité de votre application](/docs/containers?topic=containers-app#increase_availability)

3.  Lorsque vous initiez des actions de gestion de cluster, par exemple l'exécution de [commandes `ibmcloud ks`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli), les informations de base sur le cluster (par exemple le nom, l'ID, l'utilisateur, la commande) sont acheminées via le noeud final global.

### Clusters gratuits
{: #regions_free}

Les clusters gratuits sont limités à certains emplacements.
{: shortdesc}

**Création d'un cluster gratuit dans l'interface CLI** : avant de créer un cluster gratuit, vous devez cibler une région en exécutant la commande `ibmcloud ks region-set`. Votre cluster est créé dans une agglomération au sein de la région que vous ciblez : l'agglomération de Sydney dans `ap-south`, l'agglomération de Francfort dans `eu-central`, l'agglomération de Londres dans `uk-south` ou l'agglomération de Dallas dans `us-south`. Notez que vous ne pouvez pas spécifier une zone dans l'agglomération.

**Création d'un cluster gratuit dans la console {{site.data.keyword.Bluemix_notm}}** : lorsque vous utilisez la console, vous pouvez sélectionner une zone géographique et une agglomération dans la zone géographique. Vous pouvez sélectionner l'agglomération de Dallas en Amérique du Nord, les agglomérations de Francfort et de Londres en Europe ou l'agglomération de Sydney en Asie-Pacifique. Votre cluster est créé dans une zone au sein de l'agglomération que vous choisissez.

<br />


## Accès au noeud final global
{: #endpoint}

Vous pouvez organiser vos ressources entre les services {{site.data.keyword.Bluemix_notm}} en utilisant des emplacements {{site.data.keyword.Bluemix_notm}} (également appelés régions). Par exemple, vous pouvez créer un cluster Kubernetes en utilisant une image Docker privée stockée dans le service {{site.data.keyword.registryshort_notm}} au même emplacement. Pour accéder à ces ressources, vous pouvez utiliser les noeuds finaux globaux et effectuer un filtrage par emplacement.
{:shortdesc}

### Connexion à {{site.data.keyword.Bluemix_notm}}
{: #login-ic}

Lorsque vous vous connectez à la ligne de commande {{site.data.keyword.Bluemix_notm}} (`ibmcloud`), vous êtes invité à sélectionner une région. Toutefois, cette région n'affecte pas le noeud final (`ibmcloud ks`) du plug-in {{site.data.keyword.containerlong_notm}}, qui continue d'utiliser le noeud final global. Notez que vous devez tout de même cibler le groupe de ressources dans lequel se trouve votre cluster s'il ne s'agit pas du groupe de ressources par défaut.
{: shortdesc}

Pour vous connecter au noeud final d'API global {{site.data.keyword.Bluemix_notm}} et cibler le groupe de ressources dans lequel se trouve votre cluster :
```
ibmcloud login -a https://cloud.ibm.com -g <nondefault_resource_group_name>
```
{: pre}

### Connexion à {{site.data.keyword.containerlong_notm}}
{: #login-iks}

Lorsque vous vous connectez à {{site.data.keyword.Bluemix_notm}}, vous pouvez accéder à {{site.data.keyword.containershort_notm}}. Pour vous aider à démarrer, consultez les rubriques suivantes relatives à l'utilisation de l'API et de l'interface CLI {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

**Interface CLI {{site.data.keyword.containerlong_notm}} CLI** :
* [Configuration de votre interface CLI pour utiliser le plug-in `ibmcloud ks`](/docs/containers?topic=containers-cs_cli_install#cs_cli_install)
* [Configuration d votre interface CLI pour vous connecter à un cluster spécifique et exécuter des commandes `kubectl`](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Par défaut, vous êtes connecté au noeud final {{site.data.keyword.containerlong_notm}} global, `https://containers.cloud.ibm.com`.

Lorsque vous utilisez la nouvelle fonctionnalité globale dans l'interface CLI {{site.data.keyword.containerlong_notm}}, tenez compte des modifications suivantes dans la fonctionnalité existante basée sur les régions :

* Affichage de liste de ressources :
  * Lorsque vous répertoriez des ressources, par exemple avec les commandes `ibmcloud ks clusters`, `ibmcloud ks subnets` ou `ibmcloud ks zones`, les ressources de tous les emplacements sont renvoyées. Pour filtrer des resources par emplacement spécifique, vous pouvez ajouter l'indicateur `--locations` dans certaines commandes. Par exemple, si vous filtrez des clusters pour l'agglomération `dal`, les clusters à zones multiples de cette agglomération et les clusters à zone unique de centres de données (zones) situés dans cette agglomération sont renvoyés. Si vous filtrez des clusters pour le centre de données `dal10` (zone), les clusters à zones multiples ayant un noeud worker dans cette zone et les clusters à zone unique situés dans cette zone sont renvoyés. Notez que vous pouvez transmettre un emplacement ou une liste d'emplacements séparés par des virgules.
    Exemple de filtrage par emplacement :
    ```
    ibmcloud ks clusters --locations dal
    ```
    {: pre}
  * D'autres commandes ne renvoient pas les ressources de tous les emplacements. Pour exécuter les commandes `credential-set/unset/get`, `api-key-reset` et `vlan-spanning-get`, vous devez spécifier une région à l'aide de l'indicateur `--region`.

* Gestion de ressources :
  * Lorsque vous utilisez le noeud final global, vous pouvez gérer les ressources pour lesquels vous disposez de droits d'accès dans n'importe quel endroit, même si vous définissez une région en exécutant la commande `ibmcloud ks region-set` et que la ressource que vous souhaitez gérer figure dans une autre région.
  * Si vous avez des clusters de même nom dans des régions différentes, vous pouvez utiliser l'ID de cluster lorsque vous exécutez des commandes ou définir une région à l'aide de la commande `ibmcloud ks region-set` et utiliser le nom de cluster lorsque vous exécutez des commandes.

* Fonctionnalité existante :
  * Si vous devez répertorier et gérer les ressources d'une seule région, vous pouvez utiliser la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_init) `ibmcloud ks init` pour cibler un noeud final régional à la place du noeud final global.
    Exemple relatif au ciblage du noeud final régional du Sud des Etats-Unis :
    ```
    ibmcloud ks init --host https://us-south.containers.cloud.ibm.com
    ```
    {: pre}
  * Pour utiliser la fonctionnalité globale, vous pouvez réutiliser la commande `ibmcloud ks init` afin de cibler le noeud final global. Exemple relatif au nouveau ciblage du noeud final global :
    ```
    ibmcloud ks init --host https://containers.cloud.ibm.com
    ```
    {: pre}

</br></br>
**API {{site.data.keyword.containerlong_notm}}** :
* [Initiation à l'API](/docs/containers?topic=containers-cs_cli_install#cs_api).
* [Documentation sur les commandes d'API](https://containers.cloud.ibm.com/global/swagger-global-api/).
* Générez un client de l'API à utiliser par automatisation en utilisant l'[API `swagger.json`](https://containers.cloud.ibm.com/global/swagger-global-api/swagger.json).

Pour interagir avec l'API {{site.data.keyword.containerlong_notm}}, entrez le type de commande et ajoutez `global/v1/command` au noeud final.

Exemple d'API globale `GET /clusters` :
```
GET https://containers.cloud.ibm.com/global/v1/clusters
```
{: codeblock}

</br>

Si vous devez spécifier une région dans l'appel d'API, retirez le paramètre `/global` du chemin et spécifiez le nom de région dans l'en-tête `X-Region`. Pour afficher la liste des régions disponibles, exécutez la commande `ibmcloud ks regions`.

<br />



## Déprécié : Région et structure de zone {{site.data.keyword.Bluemix_notm}} précédentes
{: #bluemix_regions}

Auparavant, vos ressources {{site.data.keyword.Bluemix_notm}} étaient organisées en régions. Les régions constituent un outil conceptuel permettant d'organiser les zones et peuvent inclure des zones (centres de données) dans différents pays et différentes zones géographiques. Le tableau suivant mappe les précédentes régions {{site.data.keyword.Bluemix_notm}}, régions {{site.data.keyword.containerlong_notm}} et zones {{site.data.keyword.containerlong_notm}}. Les zones compatibles avec plusieurs zones sont en gras.
{: shortdesc}

Les noeuds finaux propres à une région sont dépréciés. Utilisez le [noeud final global](#endpoint) à la place. Si vous devez utiliser des noeuds finaux régionaux, [affectez à la variable d'environnement `IKS_BETA_VERSION` dans le plug-in {{site.data.keyword.containerlong_notm}} la valeur `0.2`](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_beta).
{: deprecated}

| Région {{site.data.keyword.containerlong_notm}} | Régions {{site.data.keyword.Bluemix_notm}} correspondantes | Zones disponibles dans la région |
| --- | --- | --- |
| Asie-Pacifique nord (clusters standard uniquement) | Tokyo | che01, hkg02, seo01, sng01, **tok02, tok04, tok05** |
| Asie-Pacifique sud | Sydney | mel01, **syd01, syd04, syd05** |
| Europe centrale | Francfort | ams03, **fra02, fra04, fra05**, mil01, osl01, par01 |
| Sud du Royaume-Uni | Londres | lon02, **lon04, lon05, lon06** |
| Est des Etats-Unis (clusters standard uniquement) | Washington DC | mon01, tor01, **wdc04, wdc06, wdc07** |
| Sud des Etats-Unis | Dallas | **dal10, dal12, dal13**, mex01, sjc03, sjc04, sao01 |
{: caption="Régions {{site.data.keyword.containershort_notm}} et {{site.data.keyword.Bluemix_notm}} correspondantes, avec des zones. Les zones compatibles avec plusieurs zones sont en gras." caption-side="top"}

En utilisant des régions {{site.data.keyword.containerlong_notm}}, vous pouvez créer des clusters  Kubernetes ou y accéder dans une région différente de la région {{site.data.keyword.Bluemix_notm}} où vous êtes connecté. Les noeuds finaux de régions {{site.data.keyword.containerlong_notm}} se réfèrent spécifiquement au service {{site.data.keyword.containerlong_notm}}, et non pas à {{site.data.keyword.Bluemix_notm}} dans son ensemble.

Vous souhaiterez peut-être vous connecter à une autre région {{site.data.keyword.containerlong_notm}} pour les raisons suivantes :
  * Vous avez créé des services {{site.data.keyword.Bluemix_notm}} ou des images Docker privées dans une région et vous souhaitez les utiliser avec {{site.data.keyword.containerlong_notm}} dans une autre région.
  * Vous souhaitez accéder à un cluster dans une région différente de la région {{site.data.keyword.Bluemix_notm}} par défaut à laquelle vous êtes connecté.

Pour changer rapidement de région, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_region-set) `ibmcloud ks region-set`.
