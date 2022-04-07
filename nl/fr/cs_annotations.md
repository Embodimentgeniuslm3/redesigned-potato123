---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, ingress

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



# Personnalisation d'Ingress avec des annotations
{: #ingress_annotation}

Pour ajouter des fonctionnalités à l'équilibreur de charge d'application (ALB) Ingress, vous pouvez indiquer des annotations sous forme de métadonnées dans une ressource Ingress.
{: shortdesc}

Avant d'utiliser des annotations, assurez-vous d'avoir configuré correctement votre service Ingress en suivant la procédure indiquée dans [Equilibrage de charge HTTPS avec des équilibreurs de charge d'application Ingress](/docs/containers?topic=containers-ingress). Lorsque vous avez configuré l'ALB Ingress avec une configuration de base, vous pouvez ensuite en développer les fonctionnalités en ajoutant des annotations dans le fichier de ressources Ingress.
{: note}

<table>
<caption>Annotations générales</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotations générales</th>
<th>Nom</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#custom-errors">Actions d'erreur personnalisées</a></td>
<td><code>custom-errors, custom-error-actions</code></td>
<td>Indiquer les actions personnalisées que peut entreprendre l'ALB pour des erreurs spécifiques à HTTP.</td>
</tr>
<tr>
<td><a href="#location-snippets">Fragments d'emplacement</a></td>
<td><code>location-snippets</code></td>
<td>Ajouter la configuration d'un bloc d'emplacement personnalisé pour un service.</td>
</tr>
<tr>
<td><a href="#alb-id">Routage de l'équilibreur de charge d'application (ALB) privé</a></td>
<td><code>ALB-ID</code></td>
<td>Router les demandes entrantes vers vos applications avec un équilibreur de charge ALB privé.</td>
</tr>
<tr>
<td><a href="#server-snippets">Fragments de serveur</a></td>
<td><code>server-snippets</code></td>
<td>Ajouter la configuration d'un bloc de serveur personnalisé.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotations de connexion</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Annotations de connexion</th>
 <th>Nom</th>
 <th>Description</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">Personnalisation des paramètres connect-timeouts et read-timeouts</a></td>
  <td><code>proxy-connect-timeout, proxy-read-timeout</code></td>
  <td>Définir la durée d'attente de connexion de l'équilibreur de charge ALB à l'application de back end et la durée de lecture avant de considérer que l'application est indisponible.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">Demandes keepalive</a></td>
  <td><code>keepalive-requests</code></td>
  <td>Définir le nombre maximum de demandes pouvant être traitées via une connexion persistante.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">Délai d'expiration de connexion persistante (keepalive)</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>Définir la durée maximale d'ouverture d'une connexion persistante sur le serveur.</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">Proxy vers le prochain serveur en amont</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>Définir à quel moment l'équilibreur de charge ALB transmet une demande au prochain serveur en amont.</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">Affinité de session avec des cookies</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>Acheminer systématiquement le trafic réseau entrant vers le même serveur en amont à l'aide d'un cookie d'association (sticky cookie).</td>
  </tr>
  <tr>
  <td><a href="#upstream-fail-timeout">Expiration du délai imparti pour les tentatives de connexion infructueuses en amont</a></td>
  <td><code>upstream-fail-timeout</code></td>
  <td>Définir la durée allouée aux tentatives de connexion au serveur de l'équilibreur de charge d'application (ALB) avant de considérer que le serveur est indisponible.</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">Connexions persistantes en amont</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>Définir le nombre maximum de connexions persistantes inactives pour un serveur en amont.</td>
  </tr>
  <tr>
  <td><a href="#upstream-max-fails">Nombre max. de tentatives de connexions infructueuses en amont</a></td>
  <td><code>upstream-max-fails</code></td>
  <td>Définir le nombre maximal de tentatives infructueuses de communication avec le serveur avant de considérer que le serveur est indisponible.</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>Annotations d'authentification HTTPS et TLS/SSL</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>Annotations d'authentification HTTPS et TLS/SSL</th>
  <th>Nom</th>
  <th>Description</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#custom-port">Ports HTTP et HTTPS personnalisés</a></td>
  <td><code>custom-port</code></td>
  <td>Modifier les ports par défaut pour le trafic réseau HTTP (port 80) et HTTPS (port 443).</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">Redirection HTTP vers HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>Rediriger les demandes HTTP non sécurisées sur votre domaine vers HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#hsts">Dispositif de sécurité HSTS (HTTP Strict Transport Security)</a></td>
  <td><code>hsts</code></td>
  <td>Définir le navigateur pour accéder au domaine uniquement à l'aide de HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">Authentification mutuelle</a></td>
  <td><code>mutual-auth</code></td>
  <td>Configurer l'authentification mutuelle pour l'équilibreur de charge ALB.</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">Support des services SSL</a></td>
  <td><code>ssl-services</code></td>
  <td>Autoriser le support des services SSL à chiffrer le trafic vers vos applications en amont nécessitant HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#tcp-ports">Ports TCP</a></td>
  <td><code>tcp-ports</code></td>
  <td>Accéder à une application via un port TCP non standard.</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>Annotations de routage de chemin</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotations de routage de chemin</th>
<th>Nom</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-external-service">Services externes</a></td>
<td><code>proxy-external-service</code></td>
<td>Ajouter des définitions de chemin à des services externes, tels qu'un service hébergé dans {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td><a href="#location-modifier">Modificateur d'emplacement</a></td>
<td><code>location-modifier</code></td>
<td>Modifier le mode utilisé par l'équilibreur de charge ALB pour faire correspondre l'URI de la demande au chemin de l'application.</td>
</tr>
<tr>
<td><a href="#rewrite-path">Chemins de redirection</a></td>
<td><code>rewrite-path</code></td>
<td>Router le trafic réseau entrant vers un autre chemin sur lequel votre application de back end est à l'écoute.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotations de mémoire tampon de proxy</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Annotations de mémoire tampon de proxy</th>
 <th>Nom</th>
 <th>Description</th>
 </thead>
 <tbody>
 <tr>
<td><a href="#large-client-header-buffers">Mémoires tampons d'en-têtes de demande client volumineux</a></td>
<td><code>large-client-header-buffers</code></td>
<td>Définir le nombre et la taille maximum des mémoires tampons qui lisent des en-têtes de demande client volumineux</td>
</tr>
 <tr>
 <td><a href="#proxy-buffering">Mise en mémoire tampon des données de réponse client</a></td>
 <td><code>proxy-buffering</code></td>
 <td>Désactiver la mise en mémoire tampon d'une réponse client sur l'équilibreur de charge ALB lors de l'envoi de la réponse au client.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">Tampons de proxy</a></td>
 <td><code>proxy-buffers</code></td>
 <td>Définir le nombre et la taille des mémoires tampons qui lisent une réponse pour une connexion unique provenant du serveur relayé via un proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">Taille de mémoire tampon de proxy</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>Définir la taille de la mémoire tampon qui lit la première partie de la réponse reçue du serveur relayé via un proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">Taille des mémoires tampons de proxy occupées</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>Définir la taille des mémoires tampons de proxy pouvant être occupées.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>Annotations de demande et de réponse</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotations de demande et de réponse</th>
<th>Nom</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#add-host-port">Ajout d'un port de serveur à l'en-tête d'hôte</a></td>
<td><code>add-host-port</code></td>
<td>Ajouter le port du serveur à l'hôte pour les demandes de routage.</td>
</tr>
<tr>
<td><a href="#client-max-body-size">Taille du corps de demande client</a></td>
<td><code>client-max-body-size</code></td>
<td>Définir la taille maximale du corps que le client peut envoyer dans le cadre d'une demande.</td>
</tr>
<tr>
<td><a href="#proxy-add-headers">En-tête de réponse ou de demande client supplémentaire</a></td>
<td><code>proxy-add-headers, response-add-headers</code></td>
<td>Ajouter des informations d'en-tête à une demande client avant d'acheminer la demande vers votre application de back end ou à une réponse client avant d'envoyer la réponse au client.</td>
</tr>
<tr>
<td><a href="#response-remove-headers">Retrait d'en-tête de réponse client</a></td>
<td><code>response-remove-headers</code></td>
<td>Retirer les informations d'en-tête d'une réponse client avant d'acheminer la réponse vers le client.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotations de limites de service</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotations de limites de service</th>
<th>Nom</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">Limites de débit globales</a></td>
<td><code>global-rate-limit</code></td>
<td>Limiter le débit de traitement des demandes et le nombre de connexions par une clé définie pour tous les services.</td>
</tr>
<tr>
<td><a href="#service-rate-limit">Limites de débit de service</a></td>
<td><code>service-rate-limit</code></td>
<td>Limiter le débit de traitement des demandes et le nombre de connexions par une clé définie pour des services spécifiques.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotations d'authentification utilisateur</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotations d'authentification utilisateur</th>
<th>Nom</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#appid-auth">Authentification {{site.data.keyword.appid_short}}</a></td>
<td><code>appid-auth</code></td>
<td>Utiliser {{site.data.keyword.appid_full}} pour l'authentification auprès de votre application.</td>
</tr>
</tbody></table>

<br>

## Annotations générales
{: #general}

### Actions d'erreur personnalisées (`custom-errors`, `custom-error-actions`)
{: #custom-errors}

Indiquez les actions personnalisées que peut entreprendre l'ALB pour des erreurs spécifiques à HTTP.
{: shortdesc}

**Description**</br>
Pour traiter des erreurs spécifiques à HTTP prévisibles, vous pouvez configurer des actions d'erreur personnalisées pour l'ALB.

* L'annotation `custom-errors` définit le nom du service, l'erreur HTTP à traiter et le nom de l'action d'erreur que l'ALB effectue lorsqu'il détecte l'erreur HTTP indiquée pour le service.
* L'annotation `custom-error-actions` définit les actions d'erreur personnalisées dans les fragments de code NGINX.

Par exemple, dans l'annotation `custom-errors`, vous pouvez configurer l'ALB pour traiter les erreurs HTTP `401` pour le service `app1` en renvoyant une action d'erreur nommée `/errorAction401`. Ensuite, dans l'annotation `custom-error-actions`, vous pouvez définir un fragment de code nommé `/errorAction401` pour que l'ALB renvoie une page d'erreur personnalisée au client. </br>

Vous pouvez également utiliser l'annotation `custom-errors` pour rediriger le client vers un service d'erreur que vous gérez. Vous devez définir le chemin d'accès à ce service d'erreur dans la section `paths` du fichier de la ressource Ingress.

**Exemple de fichier YAML de ressource Ingress**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/custom-errors: "serviceName=<app1> httpError=<401> errorActionName=</errorAction401>;serviceName=<app2> httpError=<403> errorActionName=</errorPath>"
    ingress.bluemix.net/custom-error-actions: |
         errorActionName=</errorAction401>
         #Example custom error snippet
         proxy_pass http://example.com/forbidden.html;
         <EOS>
  spec:
    tls:
    - hosts:
      - mydomain
      secretName: mysecret
    rules:
    - host: mydomain
      http:
        paths:
        - path: /path1
          backend:
            serviceName: app1
            servicePort: 80
        - path: /path2
          backend:
            serviceName: app2
            servicePort: 80
        - path: </errorPath>
          backend:
            serviceName: <error-svc>
            servicePort: 80
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>app1</em>&gt;</code> par le nom du service Kubernetes auquel s'applique l'action d'erreur personnalisée. L'action d'erreur personnalisée s'applique uniquement aux chemins d'accès utilisant ce même service en amont. Si vous ne définissez pas de nom de service, les actions d'erreur personnalisées s'appliquent aux chemins d'accès de tous les services.</td>
</tr>
<tr>
<td><code>httpError</code></td>
<td>Remplacez <code>&lt;<em>401</em>&gt;</code> par le code de l'erreur HTTP que vous voulez traiter avec une action d'erreur personnalisée.</td>
</tr>
<tr>
<td><code>errorActionName</code></td>
<td>Remplacez <code>&lt;<em>/errorAction401</em>&gt;</code> par le nom d'une action d'erreur personnalisée à entreprendre ou par le chemin d'accès à un service d'erreur.<ul>
<li>Si vous indiquez le nom d'une action d'erreur personnalisée, vous devez définir cette action d'erreur dans un fragment de code dans l'annotation <code>custom-error-actions</code>. Dans l'exemple de fichier YAML, <code>app1</code> utilise l'action <code>/errorAction401</code>, qui est définie dans le fragment dans l'annotation <code>custom-error-actions</code>.</li>
<li>Si vous spécifiez le chemin d'accès à un service d'erreur, vous devez indiquer ce chemin d'accès et le nom du service d'erreur dans la section <code>paths</code>. Dans l'exemple de fichier YAML, <code>app2</code> utilise le chemin <code>/errorPath</code>, qui est défini à la fin de la section <code>paths</code>.</li></ul></td>
</tr>
<tr>
<td><code>ingress.bluemix.net/custom-error-actions</code></td>
<td>Définissez une action d'erreur personnalisée que l'ALB effectue pour le service et l'erreur HTTP que vous avez indiqués. Utilisez un fragment de code NGINX et indiquez <code>&lt;EOS&gt;</code> à la fin de chaque fragment. Dans l'exemple de fichier YAML, l'ALB transfère au client une page d'erreur personnalisée, <code>http://example.com/forbidden.html</code>, lorsque l'erreur <code>401</code> se produit pour le service <code>app1</code>.</td>
</tr>
</tbody></table>

<br />


### Fragments d'emplacement (`location-snippets`)
{: #location-snippets}

Ajoutez la configuration d'un bloc d'emplacement personnalisé pour un service.
{:shortdesc}

**Description**</br>
Un bloc de serveur est une directive NGINX qui définit la configuration du serveur virtuel de l'équilibreur de charge d'application (ALB). Un bloc d'emplacement est une directive NGINX définie dans le bloc de serveur. Les blocs d'emplacement définissent comment Ingress traite l'URI de la demande ou la partie de la requête indiquée après le nom de domaine ou l'adresse IP et le port.

Lorsqu'un bloc de serveur reçoit une demande, le bloc d'emplacement correspond à l'URI d'accès à un chemin et la demande est transférée à l'adresse IP du pod où est déployée l'application. En utilisant l'annotation `location-snippets`, vous pouvez modifier la manière dont le bloc d'emplacement transfère les demandes à des services particuliers.

Pour modifier le bloc de serveur dans son ensemble à la place, voir l'annotation [`server-snippets`](#server-snippets).

Pour afficher les blocs de serveur et les blocs d'emplacement dans le fichier de configuration NGINX, exécutez la commande suivante pour l'un de vos pods d'ALB : `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**Exemple de fichier YAML de ressource Ingress**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/location-snippets: |
      serviceName=<myservice1>
      # Example location snippet
      proxy_request_buffering off;
      rewrite_log on;
      proxy_set_header "x-additional-test-header" "location-snippet-header";
      <EOS>
      serviceName=<myservice2>
      proxy_set_header Authorization "";
      <EOS>
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service que vous avez créé pour votre application.</td>
</tr>
<tr>
<td>Fragment d'emplacement</td>
<td>Fournissez le fragment de configuration que vous souhaitez utiliser pour le service spécifié. L'exemple de fragment pour le service <code>myservice1</code> configure le bloc d'emplacement pour désactiver la mise en mémoire tampon de la demande proxy, activer la réécriture des journaux et définir des en-têtes supplémentaires lors du transfert d'une demande au service. L'exemple de fragment pour le service <code>myservice2</code> définit un en-tête <code>Authorization</code> vide. Tous les fragments d'emplacement doivent se terminer par la valeur <code>&lt;EOS&gt;</code>.</td>
</tr>
</tbody></table>

<br />


### Routage d'équilibreur de charge d'application (ALB) privé (`ALB-ID`)
{: #alb-id}

Routez les demandes entrantes vers vos applications avec un équilibreur de charge ALB privé.
{:shortdesc}

**Description**</br>
Sélectionner un équilibreur de charge ALB privé pour acheminer les demandes entrantes au lieu de l'équilibreur de charge ALB public.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/ALB-ID: "<private_ALB_ID_1>;<private_ALB_ID_2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>ID de votre équilibreur de charge d'application (ALB) privé. Pour identifier l'ID de l'ALB privé, exécutez la commande <code>ibmcloud ks albs --cluster &lt;my_cluster&gt;</code>.<p>
Si vous disposez d'un cluster à zones multiples avec plusieurs AB privés activés, vous pouvez fournir une liste d'ID ALB séparés par un point-virgule (<code>;</code>). Par exemple : <code>ingress.bluemix.net/ALB-ID: &lt;private_ALB_ID_1&gt;;&lt;private_ALB_ID_2&gt;;&lt;private_ALB_ID_3&gt;</code></p>
</td>
</tr>
</tbody></table>

<br />


### Fragments de serveur (`server-snippets`)
{: #server-snippets}

Ajoutez la configuration d'un bloc de serveur personnalisé.
{:shortdesc}

**Description**</br>
Un bloc de serveur est une directive NGINX qui définit la configuration du serveur virtuel de l'équilibreur de charge d'application (ALB). En fournissant un fragment de configuration personnalisé dans l'annotation `server-snippets`, vous pouvez modifier la manière dont l'ALB traite les demandes au niveau su serveur.

Pour afficher les blocs de serveur et les blocs d'emplacement dans le fichier de configuration NGINX, exécutez la commande suivante pour l'un de vos pods d'ALB : `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**Exemple de fichier YAML de ressource Ingress**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/server-snippets: |
      # Example snippet
      location = /health {
      return 200 'Healthy';
      add_header Content-Type text/plain;
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td>Fragment de serveur</td>
<td>Fournissez le fragment de configuration que vous souhaitez utiliser. Ce fragment de code indique un bloc d'emplacement pour définir les demandes <code>/health</code>. Le bloc d'emplacement est configuré pour renvoyer une réponse saine et ajouter un en-tête lorsqu'il transfère une demande.</td>
</tr>
</tbody>
</table>

Vous pouvez utiliser l'annotation `server-snippets` pour ajouter un en-tête pour toutes les réponses de service au niveau d'un serveur :
{: tip}

```
annotations :
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: codeblock}

<br />


## Annotations de connexion
{: #connection}

Avec les annotations de connexion, vous pouvez modifier le mode de connexion de l'ALB à l'application de back end et aux serveurs en amont et définir des délais d'attente ou un nombre maximum de connexions actives avant de considérer l'application ou le serveur comme étant indisponible.
{: shortdesc}

### Personnalisation des paramètres connect-timeout et read-timeout (`proxy-connect-timeout`, `proxy-read-timeout`)
{: #proxy-connect-timeout}

Définissez la durée d'attente de connexion de l'équilibreur de charge ALB à l'application de back end et la durée de lecture avant que l'application soit considérée comme indisponible.
{:shortdesc}

**Description**</br>
Lorsqu'une demande client est envoyée à l'équilibreur de charge d'application (ALB) Ingress, une connexion à l'application de back end est ouverte par l'ALB. Par défaut, l'ALB patiente pendant 60 secondes pour la réception d'une réponse de l'application de back end. Si l'application de back end ne répond pas au cours de ce délai de 60 secondes, la demande de connexion est abandonnée et l'application de back end est considérée comme indisponible.

Une fois que l'ALB est connecté à l'application de back end, les données de réponse sont lues depuis cette application par l'ALB. Au cours de cette opération de lecture, l'ALB patiente jusqu'à 60 secondes entre deux opérations de lecture pour recevoir des données de l'application de back end. Si l'application de back end n'envoie pas les données au cours de ce délai de 60 secondes, la connexion avec l'application de back end est fermée et cette dernière est considérée comme indisponible.

Sur un proxy, les paramètres connect-timeout et read-timeout sont définis par défaut sur 60 secondes et, en principe, ne doivent pas être modifiés.

Si la disponibilité de votre application est imprévisible ou si celle-ci est lente à répondre en raison de charges de travail élevées, vous envisagerez peut-être d'augmenter la valeur du paramètre connect-timeout ou read-timeout. Notez que l'augmentation du délai d'attente a une incidence sur les performances de l'équilibreur de charge ALB puisque la connexion à l'application de back end doit rester ouverte jusqu'à expiration de ce délai.

D'un autre côté, vous pouvez réduire le délai d'attente afin d'améliorer les performances sur l'équilibreur de charge ALB. Assurez-vous que votre application de back end est capable de traiter les demandes dans le délai d'attente imparti, même lorsque les charges de travail sont plus importantes.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-connect-timeout: "serviceName=<myservice> timeout=<connect_timeout>"
   ingress.bluemix.net/proxy-read-timeout: "serviceName=<myservice> timeout=<read_timeout>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>&lt;connect_timeout&gt;</code></td>
<td>Délai d'attente en secondes ou en minutes pour la connexion à l'application de back end, par exemple <code>65s</code> ou <code>1m</code>. La valeur d'un paramètre connect-timeout ne peut pas excéder 75 secondes.</td>
</tr>
<tr>
<td><code>&lt;read_timeout&gt;</code></td>
<td>Délai d'attente en secondes ou en minutes avant la lecture de l'application de back end, par exemple <code>65s</code> ou <code>2m</code>.
</tr>
</tbody></table>

<br />


### Demandes Keepalive (`keepalive-requests`)
{: #keepalive-requests}

**Description**</br>
Définir le nombre maximum de demandes pouvant être traitées via une connexion persistante.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/keepalive-requests: "serviceName=<myservice> requests=<max_requests>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: <myservice>
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application. Ce paramètre est facultatif. Si aucun service n'est spécifié, la configuration est appliquée à tous les services du sous-domaine Ingress. Si le paramètre est fourni, les demandes keepalive sont définies pour le service indiqué. Si le paramètre n'est pas fourni, les demandes keepalive sont définies au niveau serveur de <code>nginx.conf</code> pour tous les services pour lesquels aucune demande keepalive n'est configurée.</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>Remplacez <code>&lt;<em>max_requests</em>&gt;</code> par le nombre maximal de demandes pouvant être traitées dans le cadre d'une connexion persistante (keepalive).</td>
</tr>
</tbody></table>

<br />


### Délai d'expiration d'une connexion persistante (`keepalive-timeout`)
{: #keepalive-timeout}

**Description**</br>
Définir la durée maximale d'ouverture d'une connexion persistante sur le serveur.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/keepalive-timeout: "serviceName=<myservice> timeout=<time>s"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application. Ce paramètre est facultatif. Si le paramètre est fourni, le délai d'expiration de la connexion persistante (keepalive-timeout) est défini pour le service indiqué. Si le paramètre n'est pas fourni, le délai d'expiration de la connexion persistante est défini au niveau serveur de <code>nginx.conf</code> pour tous les services pour lesquels aucun délai d'expiration de connexion persistante n'est configuré.</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Remplacez <code>&lt;<em>time</em>&gt;</code> par la durée, en secondes. Exemple :<code>timeout=20s</code>. La valeur <code>0</code> désactive les connexions client persistantes.</td>
</tr>
</tbody></table>

<br />


### Proxy vers le prochain serveur en amont (`proxy-next-upstream-config`)
{: #proxy-next-upstream-config}

Définissez à quel moment l'équilibreur de charge ALB transmet une demande au prochain serveur en amont.
{:shortdesc}

**Description**</br>
L'équilibreur de charge d'application (ALB) Ingress fait office de proxy entre l'application client et votre application. Certaines configurations d'application nécessitent plusieurs serveurs en amont qui traitent les demandes client entrantes à partir de l'équilibreur de charge ALB. Parfois, le serveur proxy utilisé par l'équilibreur de charge ALB ne parvient pas à établir une connexion à un serveur en amont utilisé par l'application. L'équilibreur de charge ALB peut alors essayer d'établir une connexion avec le prochain serveur en amont pour lui transmettre la demande à la place. Vous pouvez utiliser l'annotation `proxy-next-upstream-config` pour définir dans quels cas, pour combien de temps et combien de fois l'équilibreur de charge ALB peut tenter de transmettre une demande au prochain serveur en amont.

Un délai d'attente est toujours configuré lorsque vous utilisez l'annotation `proxy-next-upstream-config`, par conséquent, n'ajoutez pas le paramètre `timeout=true` dans cette annotation.
{: note}

**Exemple de fichier YAML de ressource Ingress**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-next-upstream-config: "serviceName=<myservice1> retries=<tries> timeout=<time> error=true http_502=true; serviceName=<myservice2> http_403=true non_idempotent=true"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 80
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>Remplacez <code>&lt;<em>tries</em>&gt;</code> par le nombre maximal de tentatives de l'équilibreur de charge ALB pour transmettre une demande au prochain serveur en amont. Ce nombre inclut la demande d'origine. Pour désactiver cette limitation, utilisez <code>0</code>. Si vous n'indiquez pas de valeur, la valeur par défaut <code>0</code> est utilisée.
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Remplacez <code>&lt;<em>time</em>&gt;</code> par la durée maximale (en secondes) allouée à l'équilibreur de charge ALB pour tenter de transmettre une demande au prochain serveur en amont. Par exemple, pour définir une durée de 30 secondes, entrez <code>30s</code>. Pour désactiver cette limitation, utilisez <code>0</code>. Si vous n'indiquez pas de valeur, la valeur par défaut <code>0</code> est utilisée.
</td>
</tr>
<tr>
<td><code>error</code></td>
<td>Si la valeur est <code>true</code>, l'équilibreur de charge ALB transmet une demande au prochain serveur en amont en cas d'erreur lors de l'établissement d'une connexion avec le premier serveur en amont, en lui transférant la demande ou en lisant l'en-tête de réponse.
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>Si la valeur est <code>true</code>, l'équilibreur de charge ALB transmet une demande au prochain serveur en amont lorsque le premier serveur en amont renvoie une réponse vide ou non valide.
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>Si la valeur est <code>true</code>, l'équilibreur de charge ALB transmet une demande au prochain serveur en amont lorsque le premier serveur en amont renvoie une réponse avec le code 502. Vous pouvez considérer les codes de réponse HTTP suivants : <code>500</code>, <code>502</code>, <code>503</code>, <code>504</code>, <code>403</code>, <code>404</code>, <code>429</code>.
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>Si la valeur est <code>true</code>, l'équilibreur de charge ALB peut transmettre des demandes avec une méthode 'non-idempotent' au prochain serveur en amont. Par défaut, l'équilibreur de charge ALB ne transmet pas ces demandes au prochain serveur en amont.
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>Pour empêcher l'équilibreur de charge ALB de transmettre les demandes au prochain serveur en amont, définissez ce paramètre avec la valeur <code>true</code>.
</td>
</tr>
</tbody></table>

<br />


### Affinité de session avec des cookies (`sticky-cookie-services`)
{: #sticky-cookie-services}

Utilisez l'annotation sticky cookie pour ajouter une affinité de session à votre équilibreur de charge ALB et toujours acheminer le trafic réseau entrant au même serveur en amont.
{:shortdesc}

**Description**</br>
Pour la haute disponibilité, certaines configurations d'application nécessitent de déployer plusieurs serveurs en amont qui prennent en charge les demandes client entrantes. Lorsqu'un client se connecte à votre application de back end, vous pouvez utiliser l'affinité de session pour qu'un client soit servi par le même serveur en amont pendant toute la durée d'une session ou pendant la durée d'exécution d'une tâche. Vous pouvez configurer votre équilibreur de charge ALB pour assurer une affinité de session en acheminant toujours le trafic réseau entrant au même serveur en amont.

Chaque client qui se connecte à votre application de back end est affecté par l'équilibreur de charge d'application (ALB) à l'un des serveurs en amont disponibles. L'équilibreur de charge ALB crée un cookie de session stocké dans l'application du client et inclus dans les informations d'en-tête de chaque demande entre l'équilibreur de charge ALB et le client. Les informations contenues dans le cookie garantissent la prise en charge de toutes les demandes par le même serveur en amont pendant toute la session.

Recourir à des sessions permanentes (sticky) peut s'avérer plus complexe et réduire votre disponibilité. Par exemple, vous pouvez avoir un serveur HTTP qui conserve certains états de session pour une connexion initiale de sorte que le service HTTP n'accepte que les demandes suivantes avec la même valeur d'état pour la session. Cependant cela évite la mise à l'échelle horizontale facile du service HTTP. Envisagez l'utilisation d'une base de données externe, comme Redis ou Memcached, pour stocker la valeur de session de la demande HTTP pour pouvoir conserver l'état de la session sur plusieurs serveurs.
{: note}

Lorsque vous incluez plusieurs services, utilisez un point-virgule (;) pour les séparer.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/sticky-cookie-services: "serviceName=<myservice1> name=<cookie_name1> expires=<expiration_time1> path=<cookie_path1> hash=<hash_algorithm1>;serviceName=<myservice2> name=<cookie_name2> expires=<expiration_time2> path=<cookie_path2> hash=<hash_algorithm2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>Remplacez <code>&lt;<em>cookie_name</em>&gt;</code> par le nom d'un cookie d'association (sticky cookie) créé lors d'une session.</td>
</tr>
<tr>
<td><code>expires</code></td>
<td>Remplacez <code>&lt;<em>expiration_time</em>&gt;</code> par le délai en secondes (s), minutes (m) ou heures (h) avant expiration du cookie d'association. Ce délai est indépendant de l'activité de l'utilisateur. Une fois le cookie arrivé à expiration, il est supprimé par le navigateur Web du client et n'est plus envoyé à l'équilibreur de charge ALB. Par exemple, pour définir un délai d'expiration d'1 seconde, d'1 minute ou d'1 heure, entrez <code>1s</code>, <code>1m</code> ou <code>1h</code>.</td>
</tr>
<tr>
<td><code>path</code></td>
<td>Remplacez <code>&lt;<em>cookie_path</em>&gt;</code> par le chemin ajouté au sous-domaine Ingress et qui indique pour quels domaines et sous-domaines le cookie est envoyé à l'équilibreur de charge ALB. Par exemple, si votre domaine Ingress est <code>www.myingress.com</code> et que vous souhaitez envoyer le cookie dans chaque demande client, vous devez définir <code>path=/</code>. Si vous souhaitez envoyer le cookie uniquement pour <code>www.myingress.com/myapp</code> et tous ses sous-domaines, vous devez définir <code>path=/myapp</code>.</td>
</tr>
<tr>
<td><code>hash</code></td>
<td>Remplacez <code>&lt;<em>hash_algorithm</em>&gt;</code> par l'algorithme de hachage qui protège les informations dans le cookie. Seul le type <code>sha1</code> est pris en charge. SHA1 crée une somme hachée basée sur les informations contenues dans le cookie et l'ajoute au cookie. Le serveur peut déchiffrer les informations contenues dans le cookie et vérifier l'intégrité des données.</td>
</tr>
</tbody></table>

<br />


### Expiration du délai imparti pour les tentatives de connexion infructueuses en amont (`upstream-fail-timeout`)
{: #upstream-fail-timeout}

Définissez la durée autorisée des tentatives de connexion de l'équilibreur de charge d'application (ALB) au serveur.
{:shortdesc}

**Description**</br>
Définissez la durée allouée aux tentatives de l'équilibreur de charge d'application (ALB) pour se connecter à un serveur avant de considérer que ce serveur est indisponible. Pour qu'un serveur soit considéré comme indisponible, l'ALB doit atteindre le nombre maximal de tentatives de connexion infructueuses défini par l'annotation [`upstream-max-fails`](#upstream-max-fails) dans la durée impartie. Cette durée détermine également combien de temps le serveur est considéré comme étant indisponible.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-fail-timeout: "serviceName=<myservice> fail-timeout=<fail_timeout>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName (facultatif)</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
</tr>
<tr>
<td><code>fail-timeout</code></td>
<td>Remplacez <code>&lt;<em>fail_timeout</em>&gt;</code> par la durée allouée aux tentatives de l'équilibreur de charge d'application (ALB) pour se connecter à un serveur avant de considérer ce serveur comme indisponible. La valeur par défaut est <code>10s</code>. La durée doit être exprimée en secondes.</td>
</tr>
</tbody></table>

<br />


### Connexions persistantes en amont (`upstream-keepalive`)
{: #upstream-keepalive}

Définissez le nombre maximum de connexions persistantes inactives pour un serveur en amont.
{:shortdesc}

**Description**</br>
Définir le nombre maximum de connexions persistantes inactives à un serveur en amont d'un service donné. Le serveur en amont accepte, par défaut, de 64 connexions persistantes inactives.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-keepalive: "serviceName=<myservice> keepalive=<max_connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
</tr>
<tr>
<td><code>keepalive</code></td>
<td>Remplacez <code>&lt;<em>max_connections</em>&gt;</code> par le nombre maximal de connexions persistantes inactives dans le serveur en amont. Valeur par défaut : <code>64</code>. La valeur <code>0</code> désactive les connexions persistantes en amont pour le service spécifié.</td>
</tr>
</tbody></table>

<br />


### Nombre max. de tentatives infructueuses en amont (`upstream-max-fails`)
{: #upstream-max-fails}

Définissez le nombre maximal de tentatives de communication infructueuses avec le serveur.
{:shortdesc}

**Description**</br>
Définissez le nombre maximal de tentatives de connexion infructueuses au serveur par l'ALB avant de considérer que ce serveur est indisponible. Pour que le serveur soit considéré comme indisponible, l'ALB doit atteindre le nombre maximal compris dans la durée impartie définie par l'annotation [`upstream-fail-timeout`](#upstream-fail-timeout). La durée pendant laquelle le serveur est considéré comme indisponible est également définie par l'annotation `upstream-fail-timeout`.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-max-fails: "serviceName=<myservice> max-fails=<max_fails>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName (facultatif)</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
</tr>
<tr>
<td><code>max-fails</code></td>
<td>Remplacez <code>&lt;<em>max_fails</em>&gt;</code> par le nombre maximal de tentatives infructueuses que peut effectuer l'équilibreur de charge d'application (ALB) pour communiquer avec le serveur. La valeur par défaut est <code>1</code>. La valeur <code>0</code> désactive l'annotation.</td>
</tr>
</tbody></table>

<br />


## Annotations d'authentification HTTPS et TLS/SSL
{: #https-auth}

Avec les annotations d'authentification HTTPS et TLS/SSL, vous pouvez configurer votre ALB pour le trafic HTTPS, modifier les ports par défaut HTTPS, activer le chiffrement SSL pour le trafic envoyé à vos applications de back end ou configurer l'authentification mutuelle.
{: shortdesc}

### Ports HTTP et HTTPS personnalisés (`custom-port`)
{: #custom-port}

Modifiez les ports par défaut pour le trafic réseau HTTP (port 80) et HTTPS (port 443).
{:shortdesc}

**Description**</br>
Par défaut, l'équilibreur de charge d'application (ALB) Ingress est configuré pour écouter le trafic réseau HTTP entrant sur le port 80 et pour le trafic réseau HTTPS entrant sur le port 443. Vous pouvez modifier les ports par défaut pour renforcer la sécurité de votre domaine ALB ou pour activer uniquement un port HTTPS.

Pour activer l'authentification mutuelle sur un port, [configurez l'ALB pour ouvrir le port valide](/docs/containers?topic=containers-ingress#opening_ingress_ports), puis spécifiez ce port dans l'annotation [`mutual-auth`](#mutual-auth). N'utilisez pas l'annotation `custom-port` pour spécifier un port pour l'authentification mutuelle.
{: note}

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/custom-port: "protocol=<protocol1> port=<port1>;protocol=<protocol2> port=<port2>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>Entrez <code>http</code> ou <code>https</code> pour changer le port par défaut pour le trafic réseau HTTP ou HTTPS entrant.</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>Entrez le numéro de port à utiliser pour le trafic réseau HTTP ou HTTPS entrant.  <p class="note">Lorsqu'un port personnalisé est indiqué pour HTTP ou HTTPS, les ports par défaut ne sont plus valides à la fois pour HTTP et HTTPS. Par exemple, pour remplacer le port par défaut pour HTTPS par 8443, mais utiliser le port par défaut pour HTTP, vous devez définir des ports personnalisés pour les deux ports : <code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>.</p></td>
 </tr>
</tbody></table>


**Utilisation**</br>
1. Passez en revue les ports ouverts pour votre équilibreur de charge ALB.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  La sortie de votre interface CLI sera similaire à ceci :
  ```
  NAME                                             TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. Ouvrez la mappe de configuration ALB.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. Ajoutez les ports HTTP et HTTPS différents des valeurs par défaut à la mappe de configuration (ConfigMap). Remplacez `<port>` par le port HTTP ou HTTPS que vous souhaitez ouvrir.
  <p class="note">Par défaut, les ports 80 et 443 sont ouverts. Pour conserver les ports 80 et 443 ouverts, vous devez également les inclure en plus des autres ports TCP que vous indiquez dans la zone `public-ports`. Si vous avez activé un équilibreur de charge ALB privé, vous devez également spécifier tous les ports que vous souhaitez garder ouverts dans la zone `private-ports`. Pour plus d'informations, voir [Ouverture de ports dans l'équilibreur de charge d'application (ALB) Ingress](/docs/containers?topic=containers-ingress#opening_ingress_ports).</p>
  ```
  apiVersion: v1
  kind: ConfigMap
  data:
    public-ports: <port1>;<port2>
  metadata:
    creationTimestamp: 2017-08-22T19:06:51Z
    name: ibm-cloud-provider-ingress-cm
    namespace: kube-system
    resourceVersion: "1320"
    selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
    uid: <uid>
  ```
  {: codeblock}

4. Vérifiez que votre équilibreur de charge ALB est reconfiguré avec d'autres ports que les ports par défaut.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  La sortie de votre interface CLI sera similaire à ceci :
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                           AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. Configurez Ingress pour utiliser les ports différents des ports par défaut lorsque vous routez le trafic réseau entrant vers vos services. Utilisez l'annotation de l'exemple de fichier YAML dans cette référence.

6. Mettez à jour la configuration de votre équilibreur de charge ALB.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Ouvrez le navigateur Web de votre choix pour accéder à votre application. Exemple : `https://<ibmdomain>:<port>/<service_path>/`

<br />


### Redirection HTTP vers HTTPS (`redirect-to-https`)
{: #redirect-to-https}

Convertissez les demandes client HTTP non sécurisées en demandes HTTPS.
{:shortdesc}

**Description**</br>
Configurez votre équilibreur de charge d'application (ALB) Ingress pour sécuriser votre domaine avec le certificat TLS fourni par IBM ou votre certificat TLS personnalisé. Certains utilisateurs peuvent tenter d'accéder à vos applications en utilisant une demande `http` non sécurisée vers votre domaine ALB (par exemple, `http://www.myingress.com` au lieu de d'utiliser `https`). Vous pouvez utiliser l'annotation redirect pour convertir systématiquement en HTTPS les demandes HTTP non sécurisées. Si vous n'utilisez pas cette annotation, les demandes HTTP non sécurisées ne sont pas converties par défaut en demandes HTTPS et peuvent exposer au public des informations confidentielles non chiffrées.


La redirection de demandes HTTP en demandes HTTPS est désactivée par défaut.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/redirect-to-https: "True"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<br />


### Dispositif de sécurité HTTP Strict Transport Security (HSTS) (`hsts`)
{: #hsts}

**Description**</br>
Le dispositif de sécurité HSTS oblige le navigateur à n'accéder à un domaine qu'en utilisant HTTPS. Même si l'utilisateur saisit ou suit un lien HTTP normal, le navigateur met à niveau la connexion systématiquement vers HTTPS.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/hsts: enabled=true maxAge=<31536000> includeSubdomains=true
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          ```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>Utilisez <code>true</code> pour activer HSTS.</td>
</tr>
<tr>
<td><code>maxAge</code></td>
<td>Remplacez <code>&lt;<em>31536000</em>&gt;</code> par un nombre entier représentant le nombre de secondes nécessaire à un navigateur pour mettre en cache les demandes d'envoi directement sur HTTPS. La valeur par défaut est <code>31536000</code>, ce qui équivaut à 1 an.</td>
</tr>
<tr>
<td><code>includeSubdomains</code></td>
<td>Utilisez <code>true</code> pour demander au navigateur que la politique HSTS s'applique également à tous les sous-domaines du domaine en cours. La valeur par défaut est <code>true</code>. </td>
</tr>
</tbody></table>

<br />


### Authentification mutuelle (`mutual-auth`)
{: #mutual-auth}

Configurez l'authentification mutuelle pour l'équilibreur de charge d'application (ALB).
{:shortdesc}

**Description**</br>
Configurer l'authentification mutuelle du trafic en aval pour l'équilibreur de charge d'application (ALB) Ingress. Le client externe authentifie le serveur et le serveur authentifie également le client à l'aide de certificats. L'authentification mutuelle est également appelée authentification basée sur des certificats ou authentification bidirectionnelle.

Utilisez l'annotation `mutual-auth` pour la terminaison SSL entre le client et l'ALB Ingress. Utilisez l'annotation [`ssl-services`](#ssl-services) pour la terminaison SSL entre l'ALB Ingress et l'application de back end.

L'annotation d'authentification mutuelle valide les certificats client. Pour transférer des certificats client dans un en-tête pour que les applications gèrent les autorisations, vous pouvez utiliser l'annotation [`proxy-add-headers` ](#proxy-add-headers) suivante : `"ingress.bluemix.net/proxy-add-headers": "serviceName=router-set {\n X-Forwarded-Client-Cert $ssl_client_escaped_cert;\n}\n"`
{: tip}

**Prérequis**</br>

* Vous devez disposer d'une valeur confidentielle d'authentification mutuelle valide qui contient l'élément `ca.crt` requis. Pour créer une valeur confidentielle pour l'authentification mutuelle, consultez la procédure indiquée à la fin de cette section.
* Pour activer l'authentification mutuelle sur un autre port que le port 443, [configurez l'équilibreur de charge ALB de manière à ouvrir le port valide](/docs/containers?topic=containers-ingress#opening_ingress_ports), puis spécifiez ce port dans cette annotation. N'utilisez pas l'annotation `custom-port` pour spécifier un port pour l'authentification mutuelle.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/mutual-auth: "secretName=<mysecret> port=<port> serviceName=<servicename1>,<servicename2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>Remplacez <code>&lt;<em>mysecret</em>&gt;</code> par le nom de la ressource confidentielle.</td>
</tr>
<tr>
<td><code>port</code></td>
<td>Remplacez <code>&lt;<em>port</em>&gt;</code> par le numéro de port de l'équilibreur de charge ALB.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>servicename</em>&gt;</code> par le nom d'une ou de plusieurs ressources Ingress. Ce paramètre est facultatif.</td>
</tr>
</tbody></table>

**Pour créer une valeur confidentielle pour l'authentification mutuelle :**

1. Procurez-vous un certificat d'autorité de certification et une clé auprès de votre fournisseur de certificat. Si vous disposez de votre propre domaine, achetez un certificat TLS officiel pour votre domaine. Vérifiez que l'élément [CN ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://support.dnsimple.com/articles/what-is-common-name/) est différent pour chaque certificat.
    A des fins de test, vous pouvez créer un certificat autosigné en utilisant OpenSSL. Pour plus d'informations, voir ce [tutoriel sur les certificats SSL autosignés ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.akadia.com/services/ssh_test_certificate.html) ou ce [tutoriel sur l'authentification mutuelle qui comprend la création de votre propre autorité de certification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/).
    {: tip}
2. [Convertissez le certificat en base 64 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.base64encode.org/).
3. Créez un fichier YAML de valeur confidentielle à l'aide de la valeur cert.
   ```
   apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       ca.crt: <ca_certificate>
   ```
   {: codeblock}
4. Créez le certificat en tant que valeur confidentielle Kubernetes.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### Support des services SSL (`ssl-services`)
{: #ssl-services}

Autorisez les demandes HTTPS et chiffrez le trafic vers vos applications en amont.
{:shortdesc}

**Description**</br>
Lorsque la configuration de votre ressource Ingress comporte une section TLS, l'équilibreur de charge ALB Ingress peut traiter les demandes URL sécurisées par HTTPS vers votre application. Par défaut, l'ALB interrompt la terminaison TLS et déchiffre la demande avant d'utiliser le protocole HTTP pour transférer le trafic vers vos applications. Si vous disposez d'applications qui nécessitent le protocole HTTPS et que vous avez besoin que le trafic soit chiffré, utilisez l'annotation `ssl-services`. Avec l'annotation `ssl-services`, l'ALB met fin à la connexion TLS, puis crée une nouvelle connexion SSL entre l'ALB et le pod d'application. Le trafic est à nouveau chiffré avant d'être envoyé aux pods en amont.

Si votre application de back end peut traiter TLS et que vous souhaitez renforcer la sécurité, vous pouvez ajouter l'authentification unidirectionnelle ou mutuelle en fournissant un certificat inclus dans une valeur confidentielle (secret).

Utilisez l'annotation `ssl-services` pour la terminaison SSL entre l'ALB Ingress et l'application de back end. Utilisez l'[annotation `mutual-auth`](#mutual-auth) pour la terminaison SSL entre le client et l'ALB Ingress.
{: tip}

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: <myingressname>
  annotations:
    ingress.bluemix.net/ssl-services: ssl-service=<myservice1> ssl-secret=<service1-ssl-secret>;ssl-service=<myservice2> ssl-secret=<service2-ssl-secret>
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          ```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>ssl-service</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service qui nécessite HTTPS. Le trafic est chiffré de l'équilibreur de charge ALB jusqu'au service de cette application.</td>
</tr>
<tr>
<td><code>ssl-secret</code></td>
<td>Si votre application de back end peut traiter TLS et que vous souhaitez renforcer la sécurité, remplacez <code>&lt;<em>service-ssl-secret</em>&gt;</code> par la valeur confidentielle de l'authentification unidirectionnelle ou mutuelle du service.<ul><li>Si vous fournissez une valeur confidentielle d'authentification unidirectionnelle, cette valeur doit contenir le certificat <code>trusted.crt</code> du serveur en amont. Pour créer une valeur confidentielle pour l'authentification unidirectionnelle, consultez la procédure indiquée à la fin de cette section.</li><li>Si vous fournissez une valeur confidentielle d'authentification mutuelle, cette valeur doit contenir les éléments <code>client.crt</code> et <code>client.key</code> requis que votre application attend du client. Pour créer une valeur confidentielle pour l'authentification mutuelle, consultez la procédure indiquée à la fin de cette section.</li></ul><p class="important">Si vous ne fournissez pas de valeur confidentielle, les connexions non sécurisées sont autorisées. Vous pouvez décider d'omettre une valeur confidentielle pour tester la connexion et si vous ne disposez pas d'un certificat ou si votre certificat a expiré et que vous voulez autoriser les connexions non sécurisées.</p></td>
</tr>
</tbody></table>


**Pour créer une valeur confidentielle d'authentification unidirectionnelle :**

1. Procurez-vous la clé et le certificat de l'autorité de certification (CA) de votre serveur en amont et un certificat client SSL. L'ALB d'IBM s'appuie sur NGINX qui nécessite le certificat racine, le certificat intermédiaire et le certificat de back end. Pour plus d'informations, voir la [documentation NGINX ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.nginx.com/nginx/admin-guide/security-controls/securing-http-traffic-upstream/).
2. [Convertissez le certificat en base 64 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.base64encode.org/).
3. Créez un fichier YAML de valeur confidentielle à l'aide de la valeur cert.
   ```
   apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       trusted.crt: <ca_certificate>
   ```
   {: codeblock}

   Pour imposer aussi l'authentification mutuelle pour le trafic en amont, vous pouvez fournir les éléments `client.crt` et `client.key` en plus de `trusted.crt` dans la section data.
   {: tip}

4. Créez le certificat en tant que valeur confidentielle Kubernetes.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

</br>
**Pour créer une valeur confidentielle pour l'authentification mutuelle :**

1. Procurez-vous un certificat d'autorité de certification et une clé auprès de votre fournisseur de certificat. Si vous disposez de votre propre domaine, achetez un certificat TLS officiel pour votre domaine. Vérifiez que l'élément [CN ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://support.dnsimple.com/articles/what-is-common-name/) est différent pour chaque certificat.
    A des fins de test, vous pouvez créer un certificat autosigné en utilisant OpenSSL. Pour plus d'informations, voir ce [tutoriel sur les certificats SSL autosignés ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.akadia.com/services/ssh_test_certificate.html) ou ce [tutoriel sur l'authentification mutuelle qui comprend la création de votre propre autorité de certification ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/).
    {: tip}
2. [Convertissez le certificat en base 64 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.base64encode.org/).
3. Créez un fichier YAML de valeur confidentielle à l'aide de la valeur cert.
   ```
   apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       ca.crt: <ca_certificate>
   ```
   {: codeblock}
4. Créez le certificat en tant que valeur confidentielle Kubernetes.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### Ports TCP (`tcp-ports`)
{: #tcp-ports}

Accédez à une application via un port TCP non standard.
{:shortdesc}

**Description**</br>
Utiliser cette annotation pour une application qui s'exécute sur une charge de travail de flux TCP.

<p class="note">L'équilibreur de charge ALB opère en mode passe-système et achemine le trafic aux applications de back end. La terminaison SSL n'est pas prise en charge dans ce cas. La connexion TLS est maintenue et fonctionne sans aucun changement.</p>

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/tcp-ports: "serviceName=<myservice> ingressPort=<ingress_port> servicePort=<service_port>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes auquel accéder via un port TCP non standard.</td>
</tr>
<tr>
<td><code>ingressPort</code></td>
<td>Remplacez <code>&lt;<em>ingress_port</em>&gt;</code> par le port TCP sur lequel accéder à votre application.</td>
</tr>
<tr>
<td><code>servicePort</code></td>
<td>Ce paramètre est facultatif. Lorsqu'il est fourni, le port est remplacé par cette valeur avant l'envoi du trafic à l'application de back end. Sinon, le port reste identique au port Ingress. Si vous ne souhaitez pas définir ce paramètre, vous pouvez le retirer de votre configuration. </td>
</tr>
</tbody></table>


**Utilisation**</br>
1. Passez en revue les ports ouverts pour votre équilibreur de charge ALB.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  La sortie de votre interface CLI sera similaire à ceci :
  ```
  NAME                                             TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. Ouvrez la mappe de configuration ALB.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. Ajoutez les ports TCP à la mappe de configuration. Remplacez `<port>` par les ports TCP que vous souhaitez ouvrir.
  Par défaut, les ports 80 et 443 sont ouverts. Pour conserver les ports 80 et 443 ouverts, vous devez également les inclure en plus des autres ports TCP que vous indiquez dans la zone `public-ports`. Si vous avez activé un équilibreur de charge ALB privé, vous devez également spécifier tous les ports que vous souhaitez garder ouverts dans la zone `private-ports`. Pour plus d'informations, voir [Ouverture de ports dans l'équilibreur de charge d'application (ALB) Ingress](/docs/containers?topic=containers-ingress#opening_ingress_ports).
  {: note}
  ```
  apiVersion: v1
  kind: ConfigMap
  data:
    public-ports: 80;443;<port1>;<port2>
  metadata:
    creationTimestamp: 2017-08-22T19:06:51Z
    name: ibm-cloud-provider-ingress-cm
    namespace: kube-system
    resourceVersion: "1320"
    selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
    uid: <uid>
   ```
  {: codeblock}

4. Vérifiez que votre équilibreur de charge ALB est reconfiguré avec les ports TCP.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}
  La sortie de votre interface CLI sera similaire à ceci :
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                           AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. Configurez l'ALB pour accéder à votre application via un port TCP non standard. Utilisez l'annotation `tcp-ports` de l'exemple de fichier YAML dans cette référence.

6. Créez votre ressource ALB ou mettez à jour la configuration de votre équilibreur de charge ALB existant.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Exécutez une commande curl sur le sous-domaine Ingress pour accéder à votre application. Exemple : `curl <domain>:<ingressPort>`

<br />


## Annotations de routage de chemin
{: #path-routing}

L'équilibreur de charge d'application (ALB) Ingress achemine le trafic sur les chemins utilisés par les applications de back end pour être à l'écoute. Avec les annotations de routage de chemin, vous pouvez configurer le mode de routage du trafic vers vos applications utilisé par l'ALB.
{: shortdesc}

### Services externes (`proxy-external-service`)
{: #proxy-external-service}

Ajoutez des définitions de chemin d'accès à des services externes, tels des services hébergés dans {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

**Description**</br>
Ajoutez des définitions de chemin d'accès à des services externes. N'utilisez cette annotation que si votre application opère sur un service externe au lieu d'un service de back end. Lorsque vous utilisez cette annotation pour créer une route de service externe, seules les annotations `client-max-body-size`, `proxy-read-timeout`, `proxy-connect-timeout` et `proxy-buffering` sont prises en charge conjointement. Aucune autre annotation n'est prise en charge en conjonction avec `proxy-external-service`.

Vous ne pouvez pas spécifier plusieurs hôtes correspondant à un service et un chemin d'accès uniques.
{: note}

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-external-service: "path=<mypath> external-svc=https:<external_service> host=<mydomain>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>path</code></td>
<td>Remplacez <code>&lt;<em>mypath</em>&gt;</code> par le chemin sur lequel le service externe est à l'écoute.</td>
</tr>
<tr>
<td><code>external-svc</code></td>
<td>Remplacez <code>&lt;<em>external_service</em>&gt;</code> par le service externe à appeler. Par exemple, <code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>.</td>
</tr>
<tr>
<td><code>host</code></td>
<td>Remplacez <code>&lt;<em>mydomain</em>&gt;</code> par le domaine hôte du service externe.</td>
</tr>
</tbody></table>

<br />


### Modificateur d'emplacement (`location-modifier`)
{: #location-modifier}

Modifiez le mode utilisé par l'équilibreur de charge d'application (ALB) pour faire correspondre l'URI de la demande avec le chemin de l'application.
{:shortdesc}

**Description**</br>
Par défaut, les équilibreurs de charge ALB traitent les chemins sur lesquels les applications sont à l'écoute en tant que préfixes. Lorsqu'un équilibreur de charge ALB reçoit une demande destinée à une application, il recherche un chemin (sous forme de préfixe) dans la ressource Ingress qui correspond au début de l'URI de la demande. Si une correspondance est trouvée, la demande est transmise à l'adresse IP du pod sur lequel est déployée l'application.

L'annotation `location-modifier` modifie le mode utilisé par l'équilibreur de charge ALB pour rechercher des correspondances en modifiant l'emplacement de configuration des blocs. Le bloc d'emplacement détermine comment sont traitées les demandes correspondant au chemin de l'application.

Cette annotation est obligatoire pour traiter les chemins d'expression régulière (regex).
{: note}

**Modificateurs pris en charge**</br>
<table>
<caption>Modificateurs pris en charge</caption>
<col width="10%">
<col width="90%">
<thead>
<th>Modificateur</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><code>=</code></td>
<td>Avec le modificateur représenté par un signe égal, l'équilibreur de charge ALB sélectionne uniquement les correspondances exactes. Lorsqu'une correspondance exacte est trouvée, la recherche s'arrête et le chemin correspondant est sélectionné.<br>Par exemple, si votre application est à l'écoute sur <code>/tea</code>, l'équilibreur de charge ALB sélectionne uniquement les chemins <code>/tea</code> exacts lorsqu'il établit une correspondance entre une demande et votre application.</td>
</tr>
<tr>
<td><code>~</code></td>
<td>Avec le modificateur représenté par un tilde, l'équilibreur de charge ALB traite les chemins comme des chemins d'expression régulière sensibles à la casse lors de la correspondance.<br>Par exemple, si votre application est à l'écoute sur <code>/coffee</code>, l'équilibreur de charge ALB peut sélectionner les chemins <code>/ab/coffee</code> ou <code>/123/coffee</code> lorsqu'il établit une correspondance entre une demande et votre application même si ces chemins ne sont pas explicitement définis pour votre application.</td>
</tr>
<tr>
<td><code>~\*</code></td>
<td>Avec le modificateur représenté par un tilde suivi d'un astérisque, l'équilibreur de charge ALB traite les chemins comme des chemins d'expression régulière insensibles à la casse lors de la correspondance.<br>Par exemple, si votre application est à l'écoute sur <code>/coffee</code>, l'équilibreur de charge ALB peut sélectionner les chemins <code>/ab/Coffee</code> ou <code>/123/COFFEE</code> lorsqu'il établit une correspondance entre une demande et votre application même si ces chemins ne sont pas explicitement définis pour votre application.</td>
</tr>
<tr>
<td><code>^~</code></td>
<td>Avec le modificateur représenté par un caret suivi d'un tilde, l'équilibreur de charge ALB sélectionne la meilleure correspondance (hors expression régulière) au lieu d'un chemin d'accès d'expression régulière.</td>
</tr>
</tbody>
</table>


**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/location-modifier: "modifier='<location_modifier>' serviceName=<myservice1>;modifier='<location_modifier>' serviceName=<myservice2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>modifier</code></td>
<td>Remplacez <code>&lt;<em>location_modifier</em>&gt;</code> par le modificateur d'emplacement à utiliser pour le chemin. Les modificateurs pris en charge sont : <code>'='</code>, <code>'~'</code>, <code>'~\*'</code> et <code>'^~'</code>. Vous devez mettre les modificateurs entre guillemets simples.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
</tr>
</tbody></table>

<br />


### Chemins de redirection (`rewrite-path`)
{: #rewrite-path}

Routez le trafic réseau entrant sur un chemin de domaine d'un équilibreur de charge ALB vers un autre chemin utilisé par votre application de back end en mode écoute.
{:shortdesc}

**Description**</br>
Votre domaine d'équilibreur de charge d'application (ALB) Ingress achemine le trafic réseau entrant sur `mykubecluster.us-south.containers.appdomain.cloud/beans` vers votre application. Votre application est en mode écoute sur `/coffee` au lieu de `/beans`. Pour acheminer le trafic réseau entrant vers votre application, ajoutez l'annotation rewrite au fichier de configuration de votre ressource Ingress. L'annotation rewrite garantit que le trafic réseau entrant sur `/beans` est réacheminé vers votre application en utilisant le chemin `/coffee`. Lorsque vous incluez plusieurs services, utilisez uniquement un point-virgule (;) non précédé ni suivi d'un espace pour les séparer.

**Exemple de fichier YAML de ressource Ingress**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/rewrite-path: "serviceName=<myservice1> rewrite=<target_path1>;serviceName=<myservice2> rewrite=<target_path2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /beans
        backend:
          serviceName: myservice1
          servicePort: 80
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>Remplacez <code>&lt;<em>target_path</em>&gt;</code> par le chemin sur lequel votre application est à l'écoute. Le trafic réseau entrant sur le domaine ALB est acheminé au service Kubernetes en utilisant ce chemin. La plupart des applications ne sont pas à l'écoute sur un chemin spécifique, mais utilisent le chemin racine et un port spécifique. Dans l'exemple de <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code>, le chemin de redirection est <code>/coffee</code>. <p class= "note">si vous appliquez ce fichier et que l'URL indique le code réponse <code>404</code>, votre application de back end est peut-être à l'écoute sur un chemin qui se termine par une barre oblique (`/`). Essayez d'ajouter `/` à la fin de cette zone de redirection, puis appliquez à nouveau le fichier et relancez l'URL.</p></td>
</tr>
</tbody></table>

<br />


## Annotations de mémoire tampon de proxy
{: #proxy-buffer}

L'équilibreur de charge d'application (ALB) Ingress fait office de proxy entre votre application de back end et le navigateur Web du client. Avec les annotations de mémoire tampon de proxy, vous pouvez configurer comment les données sont mises en mémoire tampon sur votre ALB lors de l'envoi ou de la réception de paquets de données.  
{: shortdesc}

### Mémoires tampons d'en-têtes de demande client volumineux (`large-client-header-buffers`)
{: #large-client-header-buffers}

Définissez le nombre et la taille maximum des mémoires tampons qui lisent des en-têtes de demande client volumineux.
{:shortdesc}

**Description**</br>
Les mémoires tampons qui lisent des en-têtes de demande client volumineux sont attribuées uniquement à la demande : si une connexion passe à l'état keepalive après le traitement de la fin de la demande (end-of-request), ces mémoires tampons sont libérées. Par défaut, il y a `4` mémoires tampons et leur taille est égale à `8K` octets. Si une ligne de demande dépasse la taille maximale d'une mémoire tampon, le message d'erreur HTTP `414 Request-URI Too Large` est renvoyé au client. Par ailleurs, si la zone d'en-tête de demande dépasse la taille maximale d'une mémoire tampon, l'erreur `400 Bad Request` est renvoyée au client. Vous pouvez ajuster le nombre et la taille maximum des mémoires tampons utilisées pour la lecture des en-têtes de demande client volumineux.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/large-client-header-buffers: "number=<number> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>Nombre maximal de mémoires tampons à attribuer pour lire les en-têtes de demande client volumineux. Par exemple, pour le définir à 4, définissez <code>4</code>.</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>Taille maximale des mémoires tampons utilisées pour la lecture des en-têtes de demande client volumineux. Par exemple, pour la définir à 16 kilooctets, indiquez <code>16k</code>. La taille doit se terminer par <code>k</code> pour kilooctet ou <code>m</code> pour mégaoctet.</td>
 </tr>
</tbody></table>

<br />


### Mise en mémoire tampon des données de réponse client (`proxy-buffering`)
{: #proxy-buffering}

Utilisez l'annotation de mémoire tampon pour désactiver le stockage de données de réponse sur l'équilibreur de charge d'application (ALB) tandis que les données sont envoyées au client.
{:shortdesc}

**Description**</br>
L'équilibreur de charge d'application (ALB) Ingress fait office de proxy entre votre application de back end et le navigateur Web du client. Lorsqu'une réponse est envoyée de l'application de back end au client, les données de réponse sont placées par défaut en mémoire tampon sur l'ALB par défaut. L'équilibreur de charge ALB met en proxy la réponse client et commence à envoyer la réponse au client à la cadence du client. Une fois que toutes les données de l'application de back end ont été reçues par l'équilibreur de charge ALB, la connexion à l'application de back end est clôturée. La connexion de l'équilibreur de charge ALB au client reste ouverte jusqu'à ce que le client ait reçu toutes les données.

Si la mise en mémoire tampon des données de réponse sur l'équilibreur de charge d'application (ALB) est désactivée, les données sont envoyées immédiatement de l'ALB au client. Le client doit être capable de traiter les données entrantes au rythme de l'ALB. Si le client est trop lent, la connexion en amont reste ouverte jusqu'à ce que le client puisse rattraper son retard. 

La mise en mémoire tampon des données de réponse sur l'équilibreur de charge ALB est activée par défaut.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-buffering: "enabled=<false> serviceName=<myservice1>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>Pour désactiver la mise en mémoire tampon des données de réponse sur l'équilibreur de charge ALB, définissez cette option avec <code>false</code>.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code><em>&lt;myservice1&gt;</em></code> par le nom du service Kubernetes que vous avez créé pour votre application. S'il y a plusieurs services, séparez-les par un point-virgule (;). Cette zone est facultative. Si vous n'indiquez pas de nom de service, cette annotation est utilisée par tous les services.</td>
</tr>
</tbody></table>

<br />


### Mémoires tampons de proxy (`proxy-buffers`)
{: #proxy-buffers}

Configurez le nombre et la taille des mémoires tampons de proxy pour l'équilibreur de charge d'application (ALB).
{:shortdesc}

**Description**</br>
Définir le nombre et la taille des mémoires tampons qui lisent une réponse pour une connexion unique provenant du serveur relayé via un proxy. Si aucun service n'est spécifié, la configuration est appliquée à tous les services du sous-domaine Ingress. Par exemple, si une configuration telle que `serviceName=SERVICE number=2 size=1k` est spécifiée, 1k est appliqué au service. Si une configuration telle que `number=2 size=1k` est spécifiée, 1k est appliqué à tous les services du sous-domaine Ingress.</br>
<p class="tip">Si vous obtenez le message d'erreur `upstream sent too big header while reading response header from upstream`, le serveur en amont de votre système de back end a envoyé une taille d'en-tête dépassant la taille limite par défaut. Augmentez la taille des paramètres `proxy-buffers` et [`proxy-buffer-size`](#proxy-buffer-size).</p>

**Exemple de fichier YAML de ressource Ingress**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffers: "serviceName=<myservice> number=<number_of_buffers> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service auquel appliquer proxy-buffers.</td>
</tr>
<tr>
<td><code>number</code></td>
<td>Remplacez <code>&lt;<em>number_of_buffers</em>&gt;</code> par un nombre. Par exemple, <code>2</code>.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Remplacez <code>&lt;<em>size</em>&gt;</code> par la taille de chaque mémoire tampon, en kilooctets (k ou K). Par exemple, <code>1K</code>.</td>
</tr>
</tbody>
</table>

<br />


### Taille de mémoire tampon de proxy (`proxy-buffer-size`)
{: #proxy-buffer-size}

Définissez la taille de la mémoire tampon de proxy qui lit la première partie de la réponse.
{:shortdesc}

**Description**</br>
Définir la taille de la mémoire tampon qui lit la première partie de la réponse reçue du serveur relayé via un proxy. Cette partie de la réponse contient généralement un petit en-tête de réponse. Si aucun service n'est spécifié, la configuration est appliquée à tous les services du sous-domaine Ingress. Par exemple, si une configuration telle que `serviceName=SERVICE size=1k` est spécifiée, 1k est appliqué au service. Si une configuration telle que `size=1k` est spécifiée, 1k est appliqué à tous les services du sous-domaine Ingress.

Si vous obtenez le message d'erreur `upstream sent too big header while reading response header from upstream`, le serveur en amont de votre système de back end a envoyé une taille d'en-tête dépassant la taille limite par défaut. Augmentez la taille des paramètres `proxy-buffer-size` et [`proxy-buffers`](#proxy-buffers).
{: tip}

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffer-size: "serviceName=<myservice> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service auquel appliquer proxy-buffers-size.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Remplacez <code>&lt;<em>size</em>&gt;</code> par la taille de chaque mémoire tampon, en kilooctets (k ou K). Par exemple, <code>1K</code>. Pour calculer la taille adaptée, vous pouvez consulter [cet article de blogue ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.getpagespeed.com/server-setup/nginx/tuning-proxy_buffer_size-in-nginx). </td>
</tr>
</tbody></table>

<br />


### Taille des mémoires tampons de proxy occupées (`proxy-busy-buffers-size`)
{: #proxy-busy-buffers-size}

Configurez la taille des mémoires tampons de proxy pouvant être occupées.
{:shortdesc}

**Description**</br>
Limiter la taille des mémoires tampons pouvant envoyer une réponse au client alors que la réponse n'a pas été encore complètement lue. Dans l'intervalle, le reste des mémoires tampons peut lire la réponse et, au besoin, placer en mémoire tampon une partie de la réponse dans un fichier temporaire. Si aucun service n'est spécifié, la configuration est appliquée à tous les services du sous-domaine Ingress. Par exemple, si une configuration telle que `serviceName=SERVICE size=1k` est spécifiée, 1k est appliqué au service. Si une configuration telle que `size=1k` est spécifiée, 1k est appliqué à tous les services du sous-domaine Ingress.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-busy-buffers-size: "serviceName=<myservice> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
         ```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service auquel appliquer proxy-busy-buffers-size.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Remplacez <code>&lt;<em>size</em>&gt;</code> par la taille de chaque mémoire tampon, en kilooctets (k ou K). Par exemple, <code>1K</code>.</td>
</tr>
</tbody></table>

<br />


## Annotations de demande et de réponse
{: #request-response}

Utilisez les annotations de demande et de réponse pour ajouter et retirer des informations d'en-tête dans les demandes du client et du serveur et pour modifier la taille du corps que le client peut envoyer.
{: shortdesc}

### Ajout d'un port de serveur à l'en-tête d'hôte (`add-host-port`)
{: #add-host-port}

Ajoutez un port de serveur à la demande du client avant la transmission de la demande à votre application de back end.
{: shortdesc}

**Description**</br>
Ajouter l'élément `:server_port` à l'en-tête d'hôte d'une demande client avant de transférer la demande à votre application de back end.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/add-host-port: "enabled=<true> serviceName=<myservice>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>Pour activer le paramètre server_port pour le sous-domaine, définissez cette option avec la valeur <code>true</code>.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code><em>&lt;myservice&gt;</em></code> par le nom du service Kubernetes que vous avez créé pour votre application. S'il y a plusieurs services, séparez-les par un point-virgule (;). Cette zone est facultative. Si vous n'indiquez pas de nom de service, cette annotation est utilisée par tous les services.</td>
</tr>
</tbody></table>

<br />


### En-tête de réponse ou de demande client supplémentaire (`proxy-add-headers`, `response-add-headers`)
{: #proxy-add-headers}

Ajoutez des informations d'en-tête à une demande client avant d'envoyer la demande vers l'application de back end ou à une réponse client avant d'envoyer la réponse au client.
{:shortdesc}

**Description**</br>
L'équilibreur de charge d'application (ALB) Ingress fait office de proxy entre l'application client et votre application. Les demandes client envoyées à l'équilibreur de charge ALB sont traitées (relayées via un proxy) et placées dans une nouvelle demande, envoyée ensuite à votre application de back end. De la même manière, les réponses de l'application de back end sont envoyées à l'équilibreur de charge ALB sont traitées (relayées via un proxy) et placées dans une nouvelle réponse qui est ensuite envoyée au client. Lorsqu'une demande ou une réponse passe par un proxy, les informations d'en-tête HTTP initialement envoyées par le client ou l'application de back end, par exemple, le nom d'utilisateur, sont retirées.

Si votre application de back end a besoin des informations d'en-tête HTTP, vous pouvez utiliser l'annotation `proxy-add-headers` pour ajouter des informations d'en-tête à la demande client avant son envoi par l'équilibreur de charge ALB à l'application de back end. Si l'application Web du client nécessite des informations d'en-tête HTTP, vous pouvez utiliser l'annotation `response-add-headers` pour ajouter les informations d'en-tête à la réponse avant son envoi par l'équilibreur de charge ALB à l'application Web du client.<br>

Par exemple, vous devrez peut-être ajouter les informations d'en-tête X-Forward suivantes à la demande avant son transfert vers votre application :
```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```
{: screen}

Pour ajouter les informations d'en-tête X-Forward à la demande envoyée à votre application, utilisez l'annotation `proxy-add-headers` comme suit :
```
ingress.bluemix.net/proxy-add-headers: |
  serviceName=<myservice1> {
  Host $host;
  X-Real-IP $remote_addr;
  X-Forwarded-Proto $scheme;
  X-Forwarded-For $proxy_add_x_forwarded_for;
  }
```
{: codeblock}

</br>

L'annotation `response-add-headers` ne prend pas en charge les en-têtes globaux pour tous les services. Pour ajouter un en-tête pour les réponses de tous les services au niveau du serveur, vous pouvez utiliser l'[annotation `server-snippets`](#server-snippets) :
{: tip}
```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: pre}
</br>

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-add-headers: |
      serviceName=<myservice1> {
      <header1> <value1>;
      <header2> <value2>;
      }
      serviceName=<myservice2> {
      <header3> <value3>;
      }
    ingress.bluemix.net/response-add-headers: |
      serviceName=<myservice1> {
      <header1>:<value1>;
      <header2>:<value2>;
      }
      serviceName=<myservice2> {
      <header3>:<value3>;
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>Clé des informations d'en-tête à ajouter à la demande ou à la réponse du client.</td>
</tr>
<tr>
<td><code>&lt;value&gt;</code></td>
<td>Valeur des informations d'en-tête à ajouter à la demande ou à la réponse du client.</td>
</tr>
</tbody></table>

<br />


### Retrait d'en-tête de réponse client (`response-remove-headers`)
{: #response-remove-headers}

Retirez de l'application de back end les informations d'en-tête incluses dans la réponse client, avant l'envoi de la réponse au client.
{:shortdesc}

**Description**</br>
L'équilibreur de charge d'application (ALB) Ingress fait office de proxy entre votre application de back end et le navigateur Web du client. Les réponses client de l'application de back end qui sont envoyées à l'équilibreur de charge ALB (relayées via proxy) sont traitées et placées dans une nouvelle réponse qui est alors envoyée de l'ALB au navigateur Web du client. Même si le fait de relayer une réponse via un proxy permet de retirer les informations d'en-tête HTTP initialement envoyées à partir de l'application de back end, il se peut que les en-têtes propres à l'application de back end ne soient pas tous retirés par ce processus. Supprimez les informations d'en-tête d'une réponse client avant que celle-ci ne soit envoyée de l'équilibreur de charge ALB au navigateur Web du client.

**Exemple de fichier YAML de ressource Ingress**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/response-remove-headers: |
      serviceName=<myservice1> {
      "<header1>";
      "<header2>";
      }
      serviceName=<myservice2> {
      "<header3>";
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>Clé de l'en-tête à retirer de la réponse du client.</td>
</tr>
</tbody></table>

<br />


### Taille du corps de demande client (`client-max-body-size`)
{: #client-max-body-size}

Définissez la taille maximale du corps que le client peut envoyer dans le cadre d'une demande.
{:shortdesc}

**Description**</br>
Dans le but de maintenir les performances attendues, la valeur définie pour la taille maximale du corps de la demande client est 1 mégaoctet. Lorsqu'une demande client dont la taille du corps dépasse la limite est envoyée à l'équilibreur de charge ALB Ingress et que le client n'autorise pas la scission des données, l'équilibreur de charge ALB renvoie une réponse HTTP 413 (Entité de demande trop volumineuse) au client. Une connexion entre le client et l'équilibreur de charge ALB n'est pas possible tant que le corps de la demande n'a pas été réduit. Lorsque le client autorise le fractionnement des données en plusieurs blocs, les données sont divisées en packages de 1 mégaoctet et envoyées à l'équilibreur de charge ALB.

Vous souhaiterez peut-être augmenter la taille maximale du corps car vous attendez des demandes client avec une taille de corps supérieure à 1 mégaoctet. Par exemple, vous voulez que votre client puisse télécharger des fichiers volumineux. L'augmentation de la taille de corps de demande maximale peut avoir une incidence sur les performances de votre équilibreur de charge ALB puisque la connexion au client doit rester ouverte jusqu'à ce que la demande ait été reçue.

Certains navigateurs Web du client ne peuvent pas afficher correctement le message de réponse HTTP 413.
{: note}

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/client-max-body-size: "serviceName=<myservice> size=<size>; size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Facultatif : pour appliquer la taille de corps maximale d'un client à un service spécifique, remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service. Si vous n'indiquez pas de nom de service, la taille est appliquée à tous les services. Dans l'exemple de fichier YAML, le format <code>"serviceName=&lt;myservice&gt; size=&lt;size&gt;; size=&lt;size&gt;"</code> applique la première taille au service <code>myservice</code> et applique la seconde taille à tous les autres services.</li>
</tr>
<td><code>&lt;size&gt;</code></td>
<td>Taille maximale du corps de réponse client. Par exemple, pour définir la taille maximale à 200 mégaoctets, indiquez <code>200m</code>. Vous pouvez définir la taille 0 pour désactiver la vérification de la taille du corps de la demande client.</td>
</tr>
</tbody></table>

<br />


## Annotations de limites de service
{: #service-limit}

Avec les annotations de limites de service, vous pouvez modifier le taux de traitement des demandes par défaut et le nombre de connexions en provenance d'une adresse IP unique.
{: shortdesc}

### Limites de débit globales (`global-rate-limit`)
{: #global-rate-limit}

Limitez le débit de traitement des demandes et le nombre de connexions par une clé définie pour tous les services.
{:shortdesc}

**Description**</br>
Pour tous les services, cette annotation permet de limiter le débit de traitement des demandes et le nombre de connexions par une clé définie émanant d'une même adresse IP pour tous les chemins des back end sélectionnés.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/global-rate-limit: "key=<key> rate=<rate> conn=<number-of-connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>key</code></td>
<td>Les valeurs prises en charge sont `location`, les en-têtes `$http_` et `$uri`. Pour définir une limite globale pour les demandes entrantes en fonction de la zone ou du service, utilisez `key=location`. Pour définir une limite globale pour les demandes entrantes en fonction de l'en-tête, utilisez `X-USER-ID key=$http_x_user_id`.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>Remplacez <code>&lt;<em>rate</em>&gt;</code> par le débit de traitement. Entrez une valeur correspondant à un débit par seconde (r/s) ou à un débit par minute (r/m). Exemple : <code>50r/m</code>.</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>Remplacez <code>&lt;<em>number-of-connections</em>&gt;</code> par le nombre de connexions.</td>
</tr>
</tbody></table>

<br />


### Limites de débit de service (`service-rate-limit`)
{: #service-rate-limit}

Limitez le débit de traitement des demandes et le nombre de connexions pour des services spécifiques.
{:shortdesc}

**Description**</br>
Pour des services spécifiques, cette annotation permet de limiter le débit de traitement des demandes et le nombre de connexions par une clé définie émanant d'une même adresse IP pour tous les chemins des back end sélectionnés.

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/service-rate-limit: "serviceName=<myservice> key=<key> rate=<rate> conn=<number_of_connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service pour lequel vous désirez limiter le débit de traitement.</li>
</tr>
<tr>
<td><code>key</code></td>
<td>Les valeurs prises en charge sont `location`, les en-têtes `$http_` et `$uri`. Pour définir une limite globale pour les demandes entrantes en fonction de la zone ou du service, utilisez `key=location`. Pour définir une limite globale pour les demandes entrantes en fonction de l'en-tête, utilisez `X-USER-ID key=$http_x_user_id`.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>Remplacez <code>&lt;<em>rate</em>&gt;</code> par le débit de traitement. Pour définir un débit par seconde, utilisez r/s : <code>10r/s</code>. Pour définir un débit par minute, utilisez r/m : <code>50r/m</code>.</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>Remplacez <code>&lt;<em>number-of-connections</em>&gt;</code> par le nombre de connexions.</td>
</tr>
</tbody></table>

<br />


## Annotations d'authentification utilisateur
{: #user-authentication}

Utilisez les annotations d'authentification utilisateur si vous souhaitez utiliser {{site.data.keyword.appid_full_notm}} pour l'authentification auprès de vos applications.
{: shortdesc}

### Authentification {{site.data.keyword.appid_short_notm}} (`appid-auth`)
{: #appid-auth}

Utiliser {{site.data.keyword.appid_full_notm}} pour l'authentification auprès de votre application.
{:shortdesc}

**Description**</br>
Authentifier les demandes Web ou HTTP/HTTPS d'API avec {{site.data.keyword.appid_short_notm}}.

Si vous définissez la valeur Web pour le type de demande, une demande Web qui contient un jeton d'accès {{site.data.keyword.appid_short_notm}} est validée. En cas d'échec de la validation de jeton, la demande Web est rejetée. Si la demande ne contient pas de jeton d'accès, elle est redirigée vers la page de connexion {{site.data.keyword.appid_short_notm}}. Pour que l'authentification Web {{site.data.keyword.appid_short_notm}} fonctionne, les cookies doivent être activés dans le navigateur de l'utilisateur.

Si vous définissez la valeur api pour le type de demande, une demande API qui contient un jeton d'accès {{site.data.keyword.appid_short_notm}} est validée. Si la demande ne contient pas de jeton d'accès, le message d'erreur 401: Unauthorized est renvoyé à l'utilisateur.

Pour des raisons de sécurité, l'authentification {{site.data.keyword.appid_short_notm}} ne prend en charge que les systèmes de back end avec TLS/SSL activé.
{: note}

**Exemple de fichier YAML de ressource Ingress**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/appid-auth: "bindSecret=<bind_secret> namespace=<namespace> requestType=<request_type> serviceName=<myservice> [idToken=false]"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>bindSecret</code></td>
<td>Remplacez <em><code>&lt;bind_secret&gt;</code></em> par la valeur confidentielle (secret) Kubernetes qui stocke la valeur confidentielle de liaison pour votre instance de service {{site.data.keyword.appid_short_notm}}.</td>
</tr>
<tr>
<td><code>espace de nom</code></td>
<td>Remplacez <em><code>&lt;namespace&gt;</code></em> par l'espace de nom de la valeur confidentielle de liaison. Par défaut, cette zone correspond à l'espace de nom `default`.</td>
</tr>
<tr>
<td><code>requestType</code></td>
<td>Remplacez <code><em>&lt;request_type&gt;</em></code> par le type de demande que vous voulez envoyer à {{site.data.keyword.appid_short_notm}}. Les valeurs admises sont `web` ou `api`. La valeur par défaut est `api`.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code><em>&lt;myservice&gt;</em></code> par le nom du service Kubernetes que vous avez créé pour votre application. Cette zone est obligatoire. Si aucun nom de service n'est inclus, l'annotation est activée pour tous les services.  Si un nom de service est inclus, l'annotation est activée uniquement pour ce service. S'il y a plusieurs services, séparez-les par une virgule (,).</td>
</tr>
<tr>
<td><code>idToken=false</code></td>
<td>Facultatif : le client Liberty OIDC ne peut pas analyser simultanément le jeton d'accès et le jeton d'identité. Lorsque vous utilisez Liberty, affectez la valeur false à ce paramètre pour que le jeton d'identité ne soit pas envoyé au serveur Liberty.</td>
</tr>
</tbody></table>

**Utilisation**</br>

Comme l'application utilise {{site.data.keyword.appid_short_notm}} pour l'authentification, vous devez mettre à disposition une instance {{site.data.keyword.appid_short_notm}}, configurer cette instance avec des URI de redirection valides et générer une valeur confidentielle de liaison en reliant l'instance à votre cluster.

1. Choisissez une instance existante ou créer une nouvelle instance {{site.data.keyword.appid_short_notm}}.
  * Pour utiliser une instance existante, vérifiez que le nom de l'instance de service ne contient pas d'espace. Pour supprimer les espaces, sélectionnez le menu des autres options en regard du nom de votre instance de service et sélectionnez **Renommer le service**.
  * Pour mettre à disposition une [nouvelle instance {{site.data.keyword.appid_short_notm}}](https://cloud.ibm.com/catalog/services/app-id) :
      1. Remplacez la valeur de **Nom du service** par le nom unique que vous avez choisi pour l'instance de service. Le nom de l'instance de service ne doit pas contenir d'espace.
      2. Choisissez la même région que celle dans laquelle est déployé votre cluster.
      3. Cliquez sur **Créer**.

2. Ajoutez des URL de redirection pour votre application. Une URL de redirection est l'URL du point de terminaison de rappel de votre application. Pour éviter les attaques par hameçonnage, App ID valide l'URL de la demande par rapport à la liste blanche des URL de redirection.
  1. Dans la console de gestion {{site.data.keyword.appid_short_notm}}, accédez à **Gérer l'authentification**.
  2. Dans l'onglet **Fournisseurs d'identité**, assurez-vous qu'un fournisseur d'identité a été sélectionné. Dans le cas contraire, l'utilisateur sera authentifié mais un jeton d'accès pour accès anonyme à l'application sera émis.
  3. Dans la zone **Paramètres d'authentification**, ajoutez des URL de redirection à votre application au format `http://<hostname>/<app_path>/appid_callback` ou `https://<hostname>/<app_path>/appid_callback`.

    {{site.data.keyword.appid_full_notm}} offre une fonction de déconnexion : si `/logout` figure dans votre chemin {{site.data.keyword.appid_full_notm}}, les cookies sont supprimés et l'utilisateur est redirigé vers la page de connexion. Pour utiliser cette fonction, vous devez ajouter `/appid_logout` à votre domaine sous la forme `https://<hostname>/<app_path>/appid_logout` et inclure cette URL dans la liste des URL de redirection.
    {: note}

3. Effectuez la liaison de l'instance de service {{site.data.keyword.appid_short_notm}} à votre cluster. La commande crée une clé de service pour l'instance de service, ou vous pouvez inclure l'indicateur `--key` pour utiliser les données d'identification de la clé de service existante.
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>]
  ```
  {: pre}
  Une fois que le service a été correctement ajouté à votre cluster, une valeur confidentielle de cluster est créée pour héberger les données d'identification de votre instance de service. Exemple de sortie d'interface CLI :
  ```
  ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service appid1
    Binding service instance to namespace...
    OK
    Namespace:    mynamespace
    Secret name:  binding-<service_instance_name>
  ```
  {: screen}

4. Obtenez la valeur confidentielle créée dans l'espace de nom de votre cluster.
  ```
  kubectl get secrets --namespace=<namespace>
  ```
  {: pre}

5. Utilisez la valeur confidentielle de liaison et l'espace de nom du cluster pour ajouter l'annotation `appid-auth` dans votre ressource Ingress.


