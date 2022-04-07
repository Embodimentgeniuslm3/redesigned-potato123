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



# Personalizzazione di Ingress con le annotazioni
{: #ingress_annotation}

Per aggiungere funzionalità al tuo ALB (application load balancer) Ingress, puoi specificare delle annotazioni sotto forma di metadati in una risorsa Ingress.
{: shortdesc}

Prima di utilizzare le annotazioni, assicurati di aver impostato correttamente la configurazione del tuo servizio Ingress seguendo la procedura in [Bilanciamento del carico HTTPS con ALB (application load balancer) Ingress](/docs/containers?topic=containers-ingress). Una volta impostato l'ALB Ingress con una configurazione di base, puoi espanderne le capacità aggiungendo le annotazioni al file di risorse Ingress.
{: note}

<table>
<caption>Annotazioni generali</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotazioni generali</th>
<th>Nome</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td><a href="#custom-errors">Azioni di errore personalizzate</a></td>
<td><code>custom-errors, custom-error-actions</code></td>
<td>Indica le azioni personalizzate che l'ALB può effettuare per specifici errori HTTP.</td>
</tr>
<tr>
<td><a href="#location-snippets">Frammenti ubicazione</a></td>
<td><code>location-snippets</code></td>
<td>Aggiungi una configurazione del blocco di ubicazione personalizzata per un servizio.</td>
</tr>
<tr>
<td><a href="#alb-id">Instradamento ALB privato</a></td>
<td><code>ALB-ID</code></td>
<td>Instrada le richieste in entrata alle tue applicazioni con un ALB privato.</td>
</tr>
<tr>
<td><a href="#server-snippets">Frammenti server</a></td>
<td><code>server-snippets</code></td>
<td>Aggiungi una configurazione del blocco server personalizzata.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotazioni di connessione</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Annotazioni di connessione</th>
 <th>Nome</th>
 <th>Descrizione</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">Timeout di lettura e connessione personalizzati</a></td>
  <td><code>proxy-connect-timeout, proxy-read-timeout</code></td>
  <td>Imposta il tempo in cui l'ALB attende di connettersi e leggere dall'applicazione di back-end prima che questa venga considerata non disponibile.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">Richieste di keepalive</a></td>
  <td><code>keepalive-requests</code></td>
  <td>Imposta il numero massimo di richieste che possono essere offerte attraverso una connessione keepalive.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">Timeout di keepalive</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>Imposta il tempo massimo per cui una connessione keepalive rimane aperta sul server.</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">Upstream successivo del proxy</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>Imposta quando l'ALB può passare una richiesta al server upstream successivo.</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">Affinità di sessione con i cookie</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>Instrada sempre il tuo traffico di rete in entrata allo stesso server upstream utilizzando un cookie permanente.</td>
  </tr>
  <tr>
  <td><a href="#upstream-fail-timeout">Timeout di errore upstream</a></td>
  <td><code>upstream-fail-timeout</code></td>
  <td>Imposta il periodo di tempo durante il quale l'ALB può tentare di connettersi al server prima che quest'ultimo venga considerato non disponibile.</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">Keepalive upstream</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>Imposta il numero massimo di connessioni keepalive inattive per un server upstream.</td>
  </tr>
  <tr>
  <td><a href="#upstream-max-fails">Numero massimo di errori upstream</a></td>
  <td><code>upstream-max-fails</code></td>
  <td>Imposta il numero massimo di tentativi non riusciti di comunicare con il server prima che quest'ultimo venga considerato non disponibile.</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>Annotazioni autenticazione HTTPS e TLS/SSL</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>Annotazioni autenticazione HTTPS e TLS/SSL</th>
  <th>Nome</th>
  <th>Descrizione</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#custom-port">Porte HTTP e HTTPS personalizzate</a></td>
  <td><code>custom-port</code></td>
  <td>Modifica le porte predefinite per il traffico di rete HTTP (porta 80) e HTTPS (porta 443).</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">Reindirizzamenti HTTP a HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>Reindirizza le richieste HTTP nel tuo dominio a HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#hsts">HSTS (HTTP Strict Transport Security)</a></td>
  <td><code>hsts</code></td>
  <td>Imposta il browser per accedere al dominio solo tramite HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">Autenticazione reciproca</a></td>
  <td><code>mutual-auth</code></td>
  <td>Configura l'autenticazione reciproca per l'ALB.</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">Supporto dei servizi SSL</a></td>
  <td><code>ssl-services</code></td>
  <td>Consenti il supporto dei servizi SSL per crittografare il traffico verso le tue applicazioni upstream che richiedono HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#tcp-ports">Porte TCP</a></td>
  <td><code>tcp-ports</code></td>
  <td>Accedi a un'applicazione tramite una porta TCP non standard.</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>Annotazioni di instradamento del percorso</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotazioni di instradamento del percorso</th>
<th>Nome</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-external-service">Servizi esterni</a></td>
<td><code>proxy-external-service</code></td>
<td>Aggiungi definizioni di percorso a servizi esterni, ad esempio il servizio ospitato in {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td><a href="#location-modifier">Modificatore ubicazione</a></td>
<td><code>location-modifier</code></td>
<td>Modifica il modo in cui l'ALB mette in corrispondenza l'URI della richiesta con il percorso dell'applicazione.</td>
</tr>
<tr>
<td><a href="#rewrite-path">Percorsi di riscrittura</a></td>
<td><code>rewrite-path</code></td>
<td>Instrada il traffico di rete in entrata a un percorso diverso su cui è in ascolto la tua applicazione di back-end.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotazioni buffer proxy</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Annotazioni buffer proxy</th>
 <th>Nome</th>
 <th>Descrizione</th>
 </thead>
 <tbody>
 <tr>
<td><a href="#large-client-header-buffers">Buffer di intestazione client di grandi dimensioni</a></td>
<td><code>large-client-header-buffers</code></td>
<td>Imposta il numero e la dimensione massima dei buffer che leggono intestazioni delle richieste client di grandi dimensioni.</td>
</tr>
 <tr>
 <td><a href="#proxy-buffering">Buffer dei dati della risposta client</a></td>
 <td><code>proxy-buffering</code></td>
 <td>Disabilita il buffer di una risposta client sull'ALB durante l'invio della risposta al client.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">Buffer proxy</a></td>
 <td><code>proxy-buffers</code></td>
 <td>Imposta il numero e la dimensione dei buffer che leggono una risposta per una singola connessione dal server proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">Dimensione buffer proxy</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>Imposta la dimensione del buffer che legge la prima parte della risposta ricevuta dal server proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">Dimensione buffer occupati del proxy</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>Imposta la dimensione dei buffer del proxy che può essere occupata.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>Annotazioni richiesta e risposta</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotazioni richiesta e risposta</th>
<th>Nome</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td><a href="#add-host-port">Aggiungi porta server all'intestazione host</a></td>
<td><code>add-host-port</code></td>
<td>Aggiunge la porta server all'host per l'instradamento delle richieste.</td>
</tr>
<tr>
<td><a href="#client-max-body-size">Dimensione corpo richiesta client</a></td>
<td><code>client-max-body-size</code></td>
<td>Imposta la dimensione massima del corpo che il client può inviare come parte di una richiesta.</td>
</tr>
<tr>
<td><a href="#proxy-add-headers">Intestazione della risposta o della richiesta del client aggiuntiva</a></td>
<td><code>proxy-add-headers, response-add-headers</code></td>
<td>Aggiungi le informazioni di intestazione a una richiesta client prima di inoltrare la richiesta alla tua applicazione di back-end o a una risposta client prima di inviare la risposta al client.</td>
</tr>
<tr>
<td><a href="#response-remove-headers">Rimozione intestazione risposta client</a></td>
<td><code>response-remove-headers</code></td>
<td>Rimuovi le informazioni sull'intestazione da una risposta client prima di inoltrarla al client.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotazioni limite del servizio</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotazioni limite del servizio</th>
<th>Nome</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">Limiti di velocità globali</a></td>
<td><code>global-rate-limit</code></td>
<td>Limita la velocità di elaborazione delle richieste e il numero di connessioni per una chiave definita per tutti i servizi.</td>
</tr>
<tr>
<td><a href="#service-rate-limit">Limiti di velocità del servizio</a></td>
<td><code>service-rate-limit</code></td>
<td>Limita la velocità di elaborazione delle richieste e il numero di connessioni per una chiave definita per servizi specifici.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotazioni di autenticazione utente</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotazioni di autenticazione utente</th>
<th>Nome</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td><a href="#appid-auth">Autenticazione {{site.data.keyword.appid_short}}</a></td>
<td><code>appid-auth</code></td>
<td>Utilizza {{site.data.keyword.appid_full}} per eseguire l'autenticazione con la tua applicazione.</td>
</tr>
</tbody></table>

<br>

## Annotazioni generali
{: #general}

### Azioni di errore personalizzate (`custom-errors`, `custom-error-actions`)
{: #custom-errors}

Indica le azioni personalizzate che l'ALB può effettuare per specifici errori HTTP.
{: shortdesc}

**Descrizione**</br>
Per gestire specifici errori HTTP che potrebbero verificarsi, puoi configurare azioni di errore personalizzate che verranno eseguite dall'ALB.

* L'annotazione `custom-errors` definisce il nome del servizio, l'errore HTTP da gestire e il nome dell'azione di errore che l'ALB esegue quando rileva l'errore HTTP specificato per il servizio.
* L'annotazione `custom-error-actions` definisce le azioni di errore personalizzate nei frammenti di codice NGINX.

Ad esempio, nell'annotazione `custom-errors`, puoi configurare l'ALB per gestire gli errori HTTP `401` per `app1` restituendo un'azione di errore personalizzata denominata `/errorAction401`. Quindi, nell'annotazione `custom-error-actions`, puoi definire un frammento di codice denominato `/errorAction401` in modo che l'ALB restituisca una pagina di errore personalizzata al client.</br>

Puoi anche utilizzare l'annotazione `custom-errors` per reindirizzare il client a un servizio di errore da te gestito. Devi definire il percorso di questo servizio di errore nella sezione `paths` dei file di risorse Ingress.

**YAML risorsa Ingress di esempio**</br>

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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>app1</em>&gt;</code> con il nome del servizio Kubernetes a cui si applica l'errore personalizzato. L'errore personalizzato si applica solo ai percorsi specifici che utilizzano questo stesso servizio upstream. Se non imposti un nome di servizio, gli errori personalizzati vengono applicati a tutti i percorsi di servizio.</td>
</tr>
<tr>
<td><code>httpError</code></td>
<td>Sostituisci <code>&lt;<em>401</em>&gt;</code> con il codice di errore HTTP che vuoi gestire con un'azione di errore personalizzata.</td>
</tr>
<tr>
<td><code>errorActionName</code></td>
<td>Sostituisci <code>&lt;<em>/errorAction401</em>&gt;</code> con il nome di un'azione di errore personalizzata da eseguire o con il percorso di un servizio di errore.<ul>
<li>Se specifichi il nome di un'azione di errore personalizzata, devi definire tale azione di errore in un frammento di codice nell'annotazione <code>custom-error-actions</code>. Nello YAML di esempio, <code>app1</code> utilizza <code>/errorAction401</code>, che è definito nel frammento nell'annotazione <code>custom-error-actions</code>.</li>
<li>Se specifichi il percorso di un servizio di errore, devi specificare tale percorso di errore e il nome del servizio di errore nella sezione <code>paths</code>. Nello YAML di esempio, <code>app2</code> utilizza <code>/errorPath</code>, che è definito alla fine della sezione <code>paths</code>.</li></ul></td>
</tr>
<tr>
<td><code>ingress.bluemix.net/custom-error-actions</code></td>
<td>Definisci un'azione di errore personalizzata che l'ALB esegue per il servizio e l'errore HTTP che hai specificato. Utilizza un frammento di codice NGINX e termina ogni frammento con <code>&lt;EOS&gt;</code>. Nello YAML di esempio, l'ALB passa una pagina di errore personalizzata, <code>http://example.com/forbidden.html</code>, al client quando si verifica un errore <code>401</code> per <code>app1</code>.</td>
</tr>
</tbody></table>

<br />


### Frammenti ubicazione (`location-snippets`)
{: #location-snippets}

Aggiungi una configurazione del blocco di ubicazione personalizzata per un servizio.
{:shortdesc}

**Descrizione**</br>
Un blocco server è una direttiva NGINX che definisce la configurazione per il server virtuale ALB. Un blocco di ubicazione è una direttiva NGINX definita all'interno del blocco server. I blocchi di ubicazione definiscono in che modo Ingress elabora l'URI della richiesta o la parte della richiesta specificata dopo il nome dominio o l'indirizzo IP e la porta.

Quando un blocco server riceve una richiesta, il blocco di ubicazione mette in corrispondenza l'URI con un percorso e la richiesta viene inoltrata all'indirizzo IP del pod in cui è distribuita l'applicazione. Utilizzando l'annotazione `location-snippets`, puoi modificare in che modo il blocco di ubicazione inoltra le richieste a determinati servizi.

Per modificare invece il blocco server nel suo insieme, vedi l'annotazione [`server-snippets`](#server-snippets).

Per visualizzare i blocchi di server e ubicazione nel file di configurazione NGINX, esegui il seguente comando per uno dei tuoi pod ALB: `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**YAML risorsa Ingress di esempio**</br>

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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio che hai creato per la tua applicazione.</td>
</tr>
<tr>
<td>Frammento ubicazione</td>
<td>Fornisci il frammento di configurazione che vuoi utilizzare per il servizio specificato. Il frammento di esempio per il servizio <code>myservice1</code> configura il blocco di ubicazione per disattivare il buffer della richiesta proxy, attivare le riscritture di log e impostare le intestazioni aggiuntive quando inoltra una richiesta al servizio. Il frammento di esempio per il servizio <code>myservice2</code> imposta un'intestazione <code>Authorization</code> vuota. Ogni frammento di ubicazione deve terminare con il valore <code>&lt;EOS&gt;</code>.</td>
</tr>
</tbody></table>

<br />


### Instradamento ALB privato (`ALB-ID`)
{: #alb-id}

Instrada le richieste in entrata alle tue applicazioni con un ALB privato.
{:shortdesc}

**Descrizione**</br>
Scegli un ALB privato con cui instradare le richieste in entrata al posto dell'ALB pubblico.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>L'ID per il tuo ALB privato. Per trovare l'ID ALB privato, esegui <code>ibmcloud ks albs --cluster &lt;my_cluster&gt;</code>.<p>
Se hai un cluster multizona con più di un ALB privato abilitato, puoi fornire un elenco degli ID ALB separati da <code>;</code>. Ad esempio: <code>ingress.bluemix.net/ALB-ID: &lt;private_ALB_ID_1&gt;;&lt;private_ALB_ID_2&gt;;&lt;private_ALB_ID_3&gt;</code></p>
</td>
</tr>
</tbody></table>

<br />


### Frammenti server (`server-snippets`)
{: #server-snippets}

Aggiungi una configurazione del blocco server personalizzata.
{:shortdesc}

**Descrizione**</br>
Un blocco server è una direttiva NGINX che definisce la configurazione per il server virtuale ALB. Fornendo un frammento di configurazione personalizzato nell'annotazione `server-snippets`, puoi modificare il modo in cui l'ALB gestisce le richieste a livello di server.

Per visualizzare i blocchi di server e ubicazione nel file di configurazione NGINX, esegui il seguente comando per uno dei tuoi pod ALB: `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**YAML risorsa Ingress di esempio**</br>

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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td>Frammento server</td>
<td>Fornisci il frammento di configurazione che vuoi utilizzare. Questo frammento di esempio specifica un blocco di ubicazione per gestire le richieste <code>/health</code>. Il blocco di ubicazione è configurato per restituire una risposta integra e aggiungere un'intestazione quando inoltra una richiesta.</td>
</tr>
</tbody>
</table>

Puoi utilizzare l'annotazione `server-snippets` per aggiungere un'intestazione per tutte le risposte del servizio a livello di server:
{: tip}

```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: codeblock}

<br />


## Annotazioni di connessione
{: #connection}

Con le annotazioni di connessione, puoi modificare il modo in cui l'ALB si connette all'applicazione di back-end e ai server upstream e impostare i timeout o un numero massimo di connessioni keepalive prima che l'applicazione o il server siano considerati non disponibili.
{: shortdesc}

### Timeout di lettura e connessione personalizzati (`proxy-connect-timeout`, `proxy-read-timeout`)
{: #proxy-connect-timeout}

Imposta il tempo in cui l'ALB attende di connettersi e leggere dall'applicazione di back-end prima che questa venga considerata non disponibile.
{:shortdesc}

**Descrizione**</br>
Quando una richiesta client viene inviata all'ALB Ingress, l'ALB apre una connessione all'applicazione di back-end. Per impostazione predefinita, l'ALB attende 60 secondi per ricevere una risposta dall'applicazione di back-end. Se l'applicazione di back-end non risponde entro 60 secondi, la richiesta di connessione viene annullata e l'applicazione di back-end viene considerata non disponibile.

Una volta connesso all'applicazione di back-end, l'ALB legge i dati della risposta da tale applicazione. Durante questa operazione di lettura, l'ALB attende un massimo di 60 secondi tra due operazioni di lettura per ricevere i dati dall'applicazione di back-end. Se l'applicazione di back-end non invia i dati entro 60 secondi, il collegamento all'applicazione di back-end viene chiuso e l'applicazione non viene considerata disponibile.

Un timeout di connessione e timeout di lettura di 60 secondi è il timeout predefinito su un proxy e, in genere, non deve essere modificato.

Se la disponibilità della tua applicazione non è stazionaria o la tua applicazione è lenta a rispondere a causa di elevati carichi di lavoro, potresti voler aumentare i timeout di collegamento o di lettura. Tieni presente che l'aumento del timeout influisce sulle prestazioni dell'ALB in quanto la connessione all'applicazione di back-end deve rimanere aperta fino al raggiungimento del timeout.

D'altra parte, puoi ridurre il timeout per migliorare le prestazioni sull'ALB. Assicurati che la tua applicazione di back-end possa gestire le richieste nel timeout specificato, anche durante elevati carichi di lavoro.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>&lt;connect_timeout&gt;</code></td>
<td>Il numero di secondi o di minuti da attendere per il collegamento all'applicazione di back-end, ad esempio <code>65s</code> o <code>1m</code>. Un timeout di connessione non può superare i 75 secondi.</td>
</tr>
<tr>
<td><code>&lt;read_timeout&gt;</code></td>
<td>Il numero di secondi o di minuti da attendere prima che venga letta l'applicazione di back-end, ad esempio <code>65s</code> o <code>2m</code>.
</tr>
</tbody></table>

<br />


### Richieste di keepalive (`keepalive-requests`)
{: #keepalive-requests}

**Descrizione**</br>
Imposta il numero massimo di richieste che possono essere offerte attraverso una connessione keepalive.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione. Questo parametro è facoltativo. La configurazione viene applicata a tutti i servizi nel dominio secondario Ingress, a meno che non venga specificato un servizio. Se il parametro viene fornito, le richieste di keepalive vengono impostate per il servizio specificato. Se il parametro non viene fornito, le richieste di keepalive vengono impostate al livello server del <code>nginx.conf</code> per tutti i servizi per i quali non sono configurate le richieste di keepalive.</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>Sostituisci <code>&lt;<em>max_requests</em>&gt;</code> con il numero massimo di richieste che possono essere offerte attraverso una connessione keepalive.</td>
</tr>
</tbody></table>

<br />


### Timeout di keepalive (`keepalive-timeout`)
{: #keepalive-timeout}

**Descrizione**</br>
Imposta il tempo massimo per cui una connessione keepalive rimane aperta sul server.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione. Questo parametro è facoltativo. Se il parametro viene fornito, il timeout di keepalive viene impostato per il servizio specificato. Se il parametro non viene fornito, il timeout di keepalive viene impostato al livello server del <code>nginx.conf</code> per tutti i servizi per i quali non è configurato il timeout di keepalive.</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Sostituisci <code>&lt;<em>time</em>&gt;</code> con una quantità di tempo in secondi. Esempio: <code>timeout=20s</code>. Un valore <code>0</code> disabilita le connessioni client keepalive.</td>
</tr>
</tbody></table>

<br />


### Upstream successivo del proxy (`proxy-next-upstream-config`)
{: #proxy-next-upstream-config}

Imposta quando l'ALB può passare una richiesta al server upstream successivo.
{:shortdesc}

**Descrizione**</br>
L'ALB Ingress funge da proxy tra l'applicazione client e la tua applicazione. Alcune configurazioni di applicazione richiedono più server upstream che gestiscono le richieste client in entrata dall'ALB. A volte, il server proxy utilizzato dall'ALB non può stabilire una connessione con un server upstream utilizzato dall'applicazione L'ALB può quindi tentare di stabilire una connessione con il server upstream successivo per passare ad esso la richiesta. Puoi utilizzare l'annotazione `proxy-next-upstream-config` per impostare in quali casi, per quanto tempo e per quante volte l'ALB può tentare di passare una richiesta al server upstream successivo.

Il timeout viene sempre configurato quando utilizzi `proxy-next-upstream-config`, quindi non aggiungere `timeout=true` a questa annotazione.
{: note}

**YAML risorsa Ingress di esempio**</br>

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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>Sostituisci <code>&lt;<em>tries</em>&gt;</code> con il numero massimo di volte in cui l'ALB tenterà di passare una richiesta al server upstream successivo. Questo numero include la richiesta originale. Per disattivare questa limitazione, utilizza <code>0</code>. Se non specifichi un valore, verrà utilizzato il valore predefinito <code>0</code>.
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Sostituisci <code>&lt;<em>time</em>&gt;</code> con la quantità di tempo massima, in secondi, in cui l'ALB tenta di passare una richiesta al server upstream successivo. Ad esempio, per impostare un tempo di 30 secondi, immetti <code>30s</code>. Per disattivare questa limitazione, utilizza <code>0</code>. Se non specifichi un valore, verrà utilizzato il valore predefinito <code>0</code>.
</td>
</tr>
<tr>
<td><code>error</code></td>
<td>Se impostato su <code>true</code>, l'ALB passa una richiesta al server upstream successivo quando si verifica un errore mentre si stabilisce una connessione con il primo server upstream, si passa una richiesta ad esso oppure si legge l'intestazione della risposta.
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>Se impostato su <code>true</code>, l'ALB passa una richiesta al server upstream successivo quando il primo server upstream restituisce una risposta vuota o non valida.
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>Se impostato su <code>true</code>, l'ALB passa una richiesta al server upstream successivo quando il primo server upstream restituisce una risposta con il codice 502. Puoi designare i seguenti codici di risposta HTTP: <code>500</code>, <code>502</code>, <code>503</code>, <code>504</code>, <code>403</code>, <code>404</code>, <code>429</code>.
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>Se impostato su <code>true</code>, l'ALB può passare le richieste con un metodo non-idempotent al server upstream successivo. Per impostazione predefinita, l'ALB non passa queste richieste al server upstream successivo.
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>Per impedire che l'ALB passi le richieste al server upstream successivo, deve essere impostato su <code>true</code>.
</td>
</tr>
</tbody></table>

<br />


### Affinità di sessione con i cookie (`sticky-cookie-services`)
{: #sticky-cookie-services}

Utilizza l'annotazione cookie permanente per aggiungere l'affinità di sessione al tuo ALB e instradare sempre il traffico di rete in entrata allo stesso server upstream.
{:shortdesc}

**Descrizione**</br>
Per l'alta disponibilità, alcune configurazioni di applicazione richiedono di distribuire più server upstream che gestiscono le richieste client in entrata. Quando un client si collega alla tua applicazione di back-end, puoi utilizzare l'affinità di sessione in modo che un client sia servito dallo stesso server upstream per la durata di una sessione o per il tempo necessario per completare un'attività. Puoi configurare il tuo ALB per garantire l'affinità di sessione indirizzando sempre il traffico di rete in entrata allo stesso server upstream.

Ad ogni client che si collega alla tua applicazione di back-end, l'ALB assegna uno dei server upstream disponibili. L'ALB crea un cookie di sessione che viene memorizzato nell'applicazione del client e che viene incluso nelle informazioni di intestazione di ogni richiesta tra l'ALB e il client. Le informazioni nel cookie garantiscono che tutte le richieste vengano gestite dallo stesso server upstream nella sessione.

Affidarsi a sessioni permanenti può aggiungere complessità e ridurre la disponibilità. Ad esempio, potresti avere un server HTTP che mantiene uno stato di sessione per una connessione iniziale in modo che il servizio HTTP accetti solo richieste successive con lo stesso valore dello stato di sessione. Tuttavia, ciò impedisce un facile adattamento orizzontale del servizio HTTP. Considera l'utilizzo di un database esterno, come Redis o Memcached, per memorizzare il valore della sezione di richiesta HTTP in modo da poter mantenere lo stato della sessione su più server.
{: note}

Quando includi più servizi, utilizza un punto e virgola (;) per separarli.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>Sostituisci <code>&lt;<em>cookie_name</em>&gt;</code> con il nome di un cookie permanente creato durante una sessione.</td>
</tr>
<tr>
<td><code>expires</code></td>
<td>Sostituisci <code>&lt;<em>expiration_time</em>&gt;</code> con il tempo in secondi (s), minuti (m) o ore (h) prima che scada il cookie permanente. Il tempo non è dipendente dall'attività utente. Una volta scaduto, il cookie viene eliminato dal browser web del client e non viene più inviato all'ALB. Ad esempio, per impostare un orario di scadenza di 1 secondo, 1 minuto o 1 ora, immetti <code>1s</code>, <code>1m</code> o <code>1h</code>.</td>
</tr>
<tr>
<td><code>path</code></td>
<td>Sostituisci <code>&lt;<em>cookie_path</em>&gt;</code> con il percorso che viene aggiunto al dominio secondario Ingress e che indica per quali domini e domini secondari il cookie viene inviato all'ALB. Ad esempio, se il tuo dominio Ingress è <code>www.myingress.com</code> e desideri inviare il cookie in ogni richiesta client, devi impostare <code>path=/</code>. Se desideri inviare il cookie solo per <code>www.myingress.com/myapp</code> e a tutti i relativi domini secondari, devi impostare <code>path=/myapp</code>.</td>
</tr>
<tr>
<td><code>hash</code></td>
<td>Sostituisci <code>&lt;<em>hash_algorithm</em>&gt;</code> con l'algoritmo hash che protegge le informazioni nel cookie. È
supportato solo <code>sha1</code>. SHA1 crea un riepilogo hash in base alle informazioni nel cookie e lo accoda ad esso. Il server può decrittografare le informazioni nel cookie e verificare l'integrità dei dati.</td>
</tr>
</tbody></table>

<br />


### Timeout di errore upstream (`upstream-fail-timeout`)
{: #upstream-fail-timeout}

Imposta il periodo di tempo durante il quale l'ALB può tentare di connettersi al server.
{:shortdesc}

**Descrizione**</br>
Imposta il periodo di tempo durante il quale l'ALB può tentare di connettersi a un server prima che quest'ultimo venga considerato non disponibile. Affinché un server venga considerato non disponibile, l'ALB deve raggiungere il numero massimo di tentativi di connessione non riuscita impostato dall'annotazione [`upstream-max-fails`](#upstream-max-fails) all'interno del periodo di tempo impostato. Questo periodo di tempo determina anche per quanto tempo il server viene considerato non disponibile.

**YAML risorsa Ingress di esempio**</br>
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
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName(Optional)</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
</tr>
<tr>
<td><code>fail-timeout</code></td>
<td>Sostituisci <code>&lt;<em>fail_timeout</em>&gt;</code> con il periodo di tempo durante il quale l'ALB può tentare di connettersi al server prima che quest'ultimo venga considerato non disponibile. Il valore predefinito è <code>10s</code>. Il tempo deve essere espresso in secondi.</td>
</tr>
</tbody></table>

<br />


### Keepalive upstream (`upstream-keepalive`)
{: #upstream-keepalive}

Imposta il numero massimo di connessioni keepalive inattive per un server upstream.
{:shortdesc}

**Descrizione**</br>
Imposta il numero massimo di connessioni keepalive inattive al server upstream di un determinato servizio. Il server upstream ha 64 connessioni keepalive inattive per impostazione predefinita.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
</tr>
<tr>
<td><code>keepalive</code></td>
<td>Sostituisci <code>&lt;<em>max_connections</em>&gt;</code> con il numero massimo di connessioni keepalive inattive a un server upstream. Il valore predefinito è <code>64</code>. Un valore <code>0</code> disabilita le connessioni keepalive upstream per il servizio specificato.</td>
</tr>
</tbody></table>

<br />


### Numero massimo di errori upstream (`upstream-max-fails`)
{: #upstream-max-fails}

Imposta il numero massimo di tentativi non riusciti per comunicare con il server.
{:shortdesc}

**Descrizione**</br>
Imposta il numero massimo di volte in cui l'ALB può riuscire a non connettersi al server prima che quest'ultimo venga considerato non disponibile. Affinché il server venga considerato non disponibile, l'ALB deve raggiungere il numero massimo impostato entro il periodo di tempo impostato dall'annotazione [`upstream-fail-timeout`](#upstream-fail-timeout). Il periodo di tempo durante il quale il server viene considerato non disponibile viene impostato anche dall'annotazione `upstream-fail-timeout`.

**YAML risorsa Ingress di esempio**</br>
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
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName(Optional)</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
</tr>
<tr>
<td><code>max-fails</code></td>
<td>Sostituisci <code>&lt;<em>max_fails</em>&gt;</code> con il numero massimo di tentativi non riusciti che possono essere eseguiti dall'ALB per comunicare con il server. Il valore predefinito è <code>1</code>. Un valore pari a <code>0</code> disabilita l'annotazione.</td>
</tr>
</tbody></table>

<br />


## Annotazioni autenticazione HTTPS e TLS/SSL
{: #https-auth}

Con le annotazioni di autenticazione HTTPS e TLS/SSL, puoi configurare il tuo ALB per il traffico HTTPS, modificare le porte HTTPS predefinite, abilitare la crittografia SSL per il traffico inviato alle tue applicazioni di back-end o configurare l'autenticazione reciproca.
{: shortdesc}

### Porte HTTP e HTTPS personalizzate (`custom-port`)
{: #custom-port}

Modifica le porte predefinite per il traffico di rete HTTP (porta 80) e HTTPS (porta 443).
{:shortdesc}

**Descrizione**</br>
Per impostazione predefinita, l'ALB Ingress è configurato per l'ascolto del traffico di rete HTTP in entrata sulla porta 80 e del traffico di rete HTTPS in entrata sulla porta 443. Puoi modificare le porte predefinite per aggiungere sicurezza al tuo dominio ALB o per abilitare solo una porta HTTPS.

Per abilitare l'autenticazione reciproca su una porta, [configura l'ALB per aprire la porta valida](/docs/containers?topic=containers-ingress#opening_ingress_ports) e specifica quindi tale porta nell'[annotazione `mutual-auth`](#mutual-auth). Non utilizzare l'annotazione `custom-port` per specificare una porta per l'autenticazione reciproca.
{: note}

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>Immetti <code>http</code> o <code>https</code> per modificare la porta predefinita per il traffico di rete HTTP o HTTPS in entrata.</td>
 </tr>
 <tr>
 <td><code>&lt; port&gt;</code></td>
 <td>Immetti il numero di porta da usare per il traffico di rete HTTP o HTTPS in entrata.  <p class="note">Quando viene specificata una porta personalizzata per HTTP o HTTPS, le porte predefinite non sono più valide. Ad esempio, per modificare la porta predefinita per HTTPS in 8443, ma utilizzare la porta predefinita per HTTP, devi impostare le porte personalizzate per entrambe: <code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>.</p></td>
 </tr>
</tbody></table>


**Utilizzo**</br>
1. Controlla le porte aperte per il tuo ALB.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  L'output della CLI sarà simile al seguente:
  ```
  NAME                                             TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. Apri la mappa di configurazione di ALB.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. Aggiungi le porte HTTP e HTTPS non predefinite alla mappa di configurazione. Sostituisci `<port>` con la porta HTTP o HTTPS che desideri aprire.
  <p class="note">Per impostazione predefinita, sono aperte le porte 80 e 443. Se vuoi mantenere le porte 80 e 443 aperte, devi includerle in aggiunta a tutte le altre porte TCP che specifichi nel campo `public-ports`. Se hai abilitato un ALB privato, devi inoltre specificare tutte le porte che vuoi mantenere aperte nel campo `private-ports`. Per ulteriori informazioni, consulta [Apertura delle porte nell'ALB Ingress](/docs/containers?topic=containers-ingress#opening_ingress_ports).</p>
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

4. Verifica che il tuo ALB sia stato riconfigurato con le porte non predefinite.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  L'output della CLI sarà simile al seguente:
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                           AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. Configura il tuo Ingress in modo che utilizzi le porte non predefinite quando instrada il traffico di rete ai tuoi servizi. Utilizza l'annotazione del file YAML di esempio indicata in questo riferimento.

6. Aggiorna la configurazione del tuo ALB.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Apri il tuo browser web preferito per accedere alla tua applicazione. Esempio: `https://<ibmdomain>:<port>/<service_path>/`

<br />


### Reindirizzamenti HTTP a HTTPS (`redirect-to-https`)
{: #redirect-to-https}

Converti le richieste client http non sicure in https.
{:shortdesc}

**Descrizione**</br>
Configura il tuo ALB Ingress per proteggere il tuo dominio con il certificato TLS fornito da IBM o con il tuo certificato TLS personalizzato. Alcuni utenti potrebbero tentare di accedere alle tue applicazioni utilizzando una richiesta `http` non sicura al tuo dominio ALB, ad esempio `http://www.myingress.com`, invece di utilizzare `https`. Puoi utilizzare l'annotazione di reindirizzamento per convertire sempre le richieste HTTP non sicure in HTTPS. Se non utilizzi questa annotazione, le richieste HTTP non sicure non vengono convertite in richieste HTTPS per impostazione predefinita e potrebbero esporre al pubblico informazioni riservate non crittografate.


Il reindirizzamento delle richieste HTTP in HTTPS è disabilitato per impostazione predefinita.

**YAML risorsa Ingress di esempio**</br>
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


### HSTS (HTTP Strict Transport Security) (`hsts`)
{: #hsts}

**Descrizione**</br>
HSTS indica al browser di accedere a un dominio solo tramite HTTPS. Anche se l'utente inserisce o segue un link HTTP semplice, il browser aggiorna rigorosamente la connessione a HTTPS.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>Utilizza <code>true</code> per abilitare HSTS.</td>
</tr>
<tr>
<td><code>maxAge</code></td>
<td>Sostituisci <code>&lt;<em>31536000</em>&gt;</code> con un numero intero che rappresenta il numero di secondi in cui un browser memorizzerà nella cache l'invio di richieste direttamente a HTTPS. Il valore predefinito è <code>31536000</code>, che equivale a 1 anno.</td>
</tr>
<tr>
<td><code>includeSubdomains</code></td>
<td>Utilizza <code>true</code> per indicare al browser che la politica HSTS si applica anche a tutti i domini secondari del dominio corrente. Il valore predefinito è <code>true</code>. </td>
</tr>
</tbody></table>

<br />


### Autenticazione reciproca (`mutual-auth`)
{: #mutual-auth}

Configura l'autenticazione reciproca per l'ALB.
{:shortdesc}

**Descrizione**</br>
Configura l'autenticazione reciproca per l'ALB Ingress. Il client esterno autentica il server e anche il server autentica il client utilizzando i certificati. L'autenticazione reciproca è nota anche come autenticazione basata su certificato o autenticazione bidirezionale.

Utilizza l'annotazione `mutual-auth` per la terminazione SSL tra il client e l'ALB Ingress. Utilizza l'[annotazione `ssl-services`](#ssl-services) per la terminazione SSL tra l'ALB Ingress e l'applicazione di back-end.

L'annotazione di autenticazione reciproca convalida i certificati client. Per inoltrare i certificati client in un'intestazione affinché le applicazioni gestiscano l'autorizzazione, puoi utilizzare la seguente
[annotazione `proxy-add-headers`](#proxy-add-headers):
`"ingress.bluemix.net/proxy-add-headers": "serviceName=router-set {\n X-Forwarded-Client-Cert $ssl_client_escaped_cert;\n}\n"`
{: tip}

**Prerequisiti**</br>

* Devi disporre di un segreto di autenticazione reciproca valido che contenga la `ca.crt` richiesta. Per creare un segreto di autenticazione reciproca, vedi la procedura alla fine di questa sezione.
* Per abilitare l'autenticazione reciproca su una porta diversa da 443, [configura l'ALB per aprire la porta valida](/docs/containers?topic=containers-ingress#opening_ingress_ports) e specifica quindi tale porta in questa annotazione. Non utilizzare l'annotazione `custom-port` per specificare una porta per l'autenticazione reciproca.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>Sostituisci <code>&lt;<em>mysecret</em>&gt;</code> con un nome per la risorsa del segreto.</td>
</tr>
<tr>
<td><code> port</code></td>
<td>Sostituisci <code>&lt;<em>port</em>&gt;</code> con il numero di porta ALB.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>servicename</em>&gt;</code> con il nome di una o più risorse Ingress. Questo parametro è facoltativo.</td>
</tr>
</tbody></table>

**Per creare un segreto di autenticazione reciproca:**

1. Genera un certificato e una chiave di autorità di certificazione (CA, certificate authority) dal tuo provider di certificati. Se disponi del tuo proprio dominio, acquista un certificato TLS ufficiale per il dominio. Assicurati che il [CN ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://support.dnsimple.com/articles/what-is-common-name/) sia diverso per ciascun certificato.
    A scopo di test, puoi creare un certificato autofirmato utilizzando OpenSSL. Per ulteriori informazioni, vedi questa [esercitazione sul certificato SSL autofirmato ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.akadia.com/services/ssh_test_certificate.html) o questa [esercitazione sull'autenticazione reciproca che include la creazione della tua propria CA ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/).
    {: tip}
2. [Converti il certificato in base 64 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.base64encode.org/).
3. Crea un file YAML del segreto utilizzando il certificato.
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
4. Crea il certificato come segreto Kubernetes.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### Supporto dei servizi SSL (`ssl-services`)
{: #ssl-services}

Consenti le richieste HTTPS e crittografa il traffico alle tue applicazioni upstream.
{:shortdesc}

**Descrizione**</br>
Quando la tua configurazione della risorsa Ingress contiene una sezione TLS, l'ALB Ingress può gestire le richieste URL protette da HTTPS nella tua applicazione. Per impostazione predefinita, l'ALB termina la terminazione TLS e decrittografa la richiesta prima di utilizzare il protocollo HTTP per inoltrare il traffico alle tue applicazioni. Se disponi di applicazioni che richiedono il protocollo HTTPS e che il traffico sia crittografato, utilizza l'annotazione `ssl-services`. Con l'annotazione `ssl-services`, l'ALB termina la connessione TLS esterna, quindi crea una nuova connessione SSL tra l'ALB e il pod dell'applicazione. Il traffico viene nuovamente crittografato prima di essere inviato ai pod upstream.

Se la tua applicazione di back-end può gestire TLS e vuoi aggiungere ulteriore sicurezza, puoi aggiungere l'autenticazione unidirezionale o reciproca fornendo un certificato contenuto in un segreto.

Utilizza l'annotazione `ssl-services` per la terminazione SSL tra l'ALB Ingress e l'applicazione di back-end. Utilizza l'[annotazione `mutual-auth`](#mutual-auth) per la terminazione SSL tra il client e l'ALB Ingress.
{: tip}

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>ssl-service</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio che richiede HTTPS. Il traffico viene crittografato dall'ALB a questo servizio dell'applicazione.</td>
</tr>
<tr>
<td><code>ssl-secret</code></td>
<td>Se la tua applicazione di back-end può gestire TLS e vuoi aggiungere ulteriore sicurezza, sostituisci <code>&lt;<em>service-ssl-secret</em>&gt;</code> con il segreto di autenticazione unidirezionale o reciproca per il servizio.<ul><li>Se fornisci un segreto di autenticazione unidirezionale, il valore deve contenere il <code>trusted.crt</code> dal server upstream. Per creare un segreto unidirezionale, vedi la procedura alla fine di questa sezione.</li><li>Se fornisci un segreto di autenticazione reciproca, il valore deve contenere i <code>client.crt</code> e <code>client.key</code> richiesti che la tua applicazione si aspetta dal client. Per creare un segreto di autenticazione reciproca, vedi la procedura alla fine di questa sezione.</li></ul><p class="important">Se non fornisci un segreto, sono consentite connessioni non sicure. Puoi scegliere di omettere un segreto se vuoi testare la connessione e non hai dei certificati pronti oppure se i tuoi certificati sono scaduti e vuoi consentire connessioni non sicure.</p></td>
</tr>
</tbody></table>


**Per creare un segreto di autenticazione unidirezionale:**

1. Ottieni la chiave e il certificato di autorità di certificazione (CA, certificate authority) dal tuo server upstream e un certificato client SSL. L'ALB IBM è basato su NGINX, che richiede il certificato root, il certificato intermedio e il certificato di back-end. Per ulteriori informazioni, vedi la [documentazione di NGINX ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.nginx.com/nginx/admin-guide/security-controls/securing-http-traffic-upstream/).
2. [Converti il certificato in base 64 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.base64encode.org/).
3. Crea un file YAML del segreto utilizzando il certificato.
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

   Per applicare l'autenticazione reciproca anche per il traffico upstream, puoi fornire un `client.crt` e una `client.key` in aggiunta al `trusted.crt` nella sezione dei dati.
   {: tip}

4. Crea il certificato come segreto Kubernetes.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

</br>
**Per creare un segreto di autenticazione reciproca:**

1. Genera un certificato e una chiave di autorità di certificazione (CA, certificate authority) dal tuo provider di certificati. Se disponi del tuo proprio dominio, acquista un certificato TLS ufficiale per il dominio. Assicurati che il [CN ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://support.dnsimple.com/articles/what-is-common-name/) sia diverso per ciascun certificato.
    A scopo di test, puoi creare un certificato autofirmato utilizzando OpenSSL. Per ulteriori informazioni, vedi questa [esercitazione sul certificato SSL autofirmato ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.akadia.com/services/ssh_test_certificate.html) o questa [esercitazione sull'autenticazione reciproca che include la creazione della tua propria CA ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/).
    {: tip}
2. [Converti il certificato in base 64 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.base64encode.org/).
3. Crea un file YAML del segreto utilizzando il certificato.
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
4. Crea il certificato come segreto Kubernetes.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### Porte TCP (`tcp-ports`)
{: #tcp-ports}

Accedi a un'applicazione tramite una porta TCP non standard.
{:shortdesc}

**Descrizione**</br>
Utilizza questa annotazione per un'applicazione che esegue un carico di lavoro di flussi TCP.

<p class="note">L'ALB funziona in modalità pass-through e inoltra il traffico alle applicazioni di back-end. In questo caso, la terminazione SSL non è supportata. La connessione TLS non viene terminata e ne viene eseguito il pass-through senza alcuna variazione.</p>

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes a cui accedere tramite una porta TCP non standard.</td>
</tr>
<tr>
<td><code>ingressPort</code></td>
<td>Sostituisci <code>&lt;<em>ingress_port</em>&gt;</code> con la porta TCP con cui vuoi accedere alla tua applicazione.</td>
</tr>
<tr>
<td><code>servicePort</code></td>
<td>Questo parametro è facoltativo. Se fornito, la porta viene sostituita con questo valore prima che il traffico venga inviato all'applicazione di back-end. Altrimenti, la porta rimane la stessa della porta Ingress. Se non vuoi impostare questo parametro, puoi rimuoverlo dalla tua configurazione. </td>
</tr>
</tbody></table>


**Utilizzo**</br>
1. Controlla le porte aperte per il tuo ALB.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  L'output della CLI sarà simile al seguente:
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. Apri la mappa di configurazione di ALB.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. Aggiungi le porte TCP alla mappa di configurazione. Sostituisci `<port>` con le porte TCP che desideri aprire.
  Per impostazione predefinita, sono aperte le porte 80 e 443. Se vuoi mantenere le porte 80 e 443 aperte, devi includerle in aggiunta a tutte le altre porte TCP che specifichi nel campo `public-ports`. Se hai abilitato un ALB privato, devi inoltre specificare tutte le porte che vuoi mantenere aperte nel campo `private-ports`. Per ulteriori informazioni, consulta [Apertura delle porte nell'ALB Ingress](/docs/containers?topic=containers-ingress#opening_ingress_ports).
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

4. Verifica che il tuo ALB sia stato riconfigurato con le porte TCP.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}
  L'output della CLI sarà simile al seguente:
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                               AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. Configura l'ALB per accedere alla tua applicazione attraverso una porta TCP non standard. Utilizza l'annotazione `tcp-ports` del file YAML di esempio indicata in questo riferimento.

6. Crea la tua risorsa ALB oppure aggiorna la tua configurazione ALB esistente.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Esegui il curl del dominio secondario Ingress per accedere alla una applicazione. Esempio: `curl <domain>:<ingressPort>`

<br />


## Annotazioni di instradamento del percorso
{: #path-routing}

L'ALB Ingress instrada il traffico ai percorsi su cui sono in ascolto le applicazioni di back-end. Con le annotazioni di istradamento del percorso, puoi configurare il modo in cui l'ALB instrada il traffico verso le tue applicazioni.
{: shortdesc}

### Servizi esterni (`proxy-external-service`)
{: #proxy-external-service}

Aggiungi definizioni di percorso a servizi esterni, ad esempio i servizi ospitati in {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

**Descrizione**</br>
Aggiungi le definizioni di percorso ai servizi esterni. Utilizza questa annotazione solo quando la tua applicazione funziona su un servizio esterno anziché su un servizio di back-end. Quando utilizzi questa annotazione per creare una rotta al servizio esterno, sono supportate insieme solo le annotazioni `client-max-body-size`, `proxy-read-timeout`, `proxy-connect-timeout` e `proxy-buffering`. Tutte le altre annotazioni non sono supportate insieme a `proxy-external-service`.

Non puoi specificare più host per un singolo servizio e percorso.
{: note}

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>path</code></td>
<td>Sostituisci <code>&lt;<em>mypath</em>&gt;</code> con il percorso su cui è in ascolto il servizio esterno.</td>
</tr>
<tr>
<td><code>external-svc</code></td>
<td>Sostituisci <code>&lt;<em>external_service</em>&gt;</code> con il servizio esterno da richiamare. Ad esempio, <code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>.</td>
</tr>
<tr>
<td><code>host</code></td>
<td>Sostituisci <code>&lt;<em>mydomain</em>&gt;</code> con il dominio host del servizio esterno.</td>
</tr>
</tbody></table>

<br />


### Modificatore ubicazione (`location-modifier`)
{: #location-modifier}

Modifica il modo in cui l'ALB mette in corrispondenza l'URI della richiesta con il percorso dell'applicazione.
{:shortdesc}

**Descrizione**</br>
Per impostazione predefinita, gli ALB elaborano i percorsi su cui le applicazioni sono in ascolto sotto forma di prefissi. Alla ricezione di una richiesta a un'applicazione, l'ALB controlla la risorsa Ingress per rilevare un percorso (come prefisso) che corrisponda all'inizio dell'URI della richiesta. Se viene trovata una corrispondenza, la richiesta viene inoltrata all'indirizzo IP del pod in cui viene distribuita l'applicazione.

L'annotazione `location-modifier` cambia il modo in cui l'ALB cerca le corrispondenze modificando la configurazione del blocco di ubicazione. Il blocco di ubicazione determina come vengono gestite le richieste per il percorso dell'applicazione.

Per gestire i percorsi di espressioni regolari (regex), questa annotazione è obbligatoria.
{: note}

**Modificatori supportati**</br>
<table>
<caption>Modificatori supportati</caption>
<col width="10%">
<col width="90%">
<thead>
<th>Modificatore</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td><code>=</code></td>
<td>Il modificatore con segno di uguale fa sì che l'ALB selezioni solo le corrispondenze esatte. Quando viene trovata una corrispondenza esatta, la ricerca si ferma e viene selezionato il percorso corrispondente.<br>Ad esempio, se la tua applicazione è in ascolto su <code>/tea</code>, l'ALB seleziona solo i percorsi <code>/tea</code> esatti quando fa corrispondere una richiesta alla tua applicazione.</td>
</tr>
<tr>
<td><code>~</code></td>
<td>il modificatore con tilde fa sì che l'ALB elabori i percorsi come percorsi regex sensibili al maiuscolo/minuscolo durante la corrispondenza.<br>Ad esempio, se la tua applicazione è in ascolto su <code>/coffee</code>, l'ALB può selezionare i percorsi <code>/ab/coffee</code> o <code>/123/coffee</code> quando fa corrispondere una richiesta alla tua applicazione anche se i percorsi non sono esplicitamente impostati per la tua applicazione.</td>
</tr>
<tr>
<td><code>~\*</code></td>
<td>Il modificatore con tilde seguita da un asterisco fa sì che l'ALB elabori i percorsi come percorsi regex non sensibili al maiuscolo/minuscolo durante la corrispondenza.<br>Ad esempio, se la tua applicazione è in ascolto su <code>/coffee</code>, l'ALB può selezionare i percorsi <code>/ab/Coffee</code> o <code>/123/COFFEE</code> quando fa corrispondere una richiesta alla tua applicazione anche se i percorsi non sono esplicitamente impostati per la tua applicazione.</td>
</tr>
<tr>
<td><code>^~</code></td>
<td>Il modificatore con accento circonflesso seguito da una tilde fa sì che l'ALB selezioni la migliore corrispondenza non regex anziché un percorso regex.</td>
</tr>
</tbody>
</table>


**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>modifier</code></td>
<td>Sostituisci <code>&lt;<em>location_modifier</em>&gt;</code> con il modificatore ubicazione che vuoi utilizzare per il percorso. I modificatori supportati sono <code>'='</code>, <code>'~'</code>, <code>'~\*'</code> e <code>'^~'</code>. Devi racchiudere i modificatori tra virgolette singole.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
</tr>
</tbody></table>

<br />


### Percorsi di riscrittura (`rewrite-path`)
{: #rewrite-path}

Instrada il traffico di rete in entrata in un percorso di dominio dell'ALB a un percorso diverso su cui è in ascolto la tua applicazione di back-end.
{:shortdesc}

**Descrizione**</br>
Il tuo dominio ALB Ingress instrada il traffico di rete in entrata su `mykubecluster.us-south.containers.appdomain.cloud/beans` alla tua applicazione. La tua applicazione è in ascolto su `/coffee`, invece di `/beans`. Per inoltrare il traffico di rete in entrata alla tua applicazione, aggiungi l'annotazione di riscrittura al file di configurazione della tua risorsa Ingress. L'annotazione di riscrittura garantisce che il traffico di rete in entrata su `/beans` venga inoltrato alla tua applicazione utilizzando il percorso `/coffee`. Quando includi più servizi, utilizza solo un carattere punto e virgola (;) non preceduto o seguito da uno spazio per
separarli.

**YAML risorsa Ingress di esempio**</br>

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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>Sostituisci <code>&lt;<em>target_path</em>&gt;</code> con il percorso su cui è in ascolto la tua applicazione. Il traffico di rete in entrata nel dominio dell'ALB viene inoltrato al servizio Kubernetes utilizzando tale percorso. La maggior parte delle applicazioni non è
in ascolto su uno specifico percorso, ma utilizza il percorso root e una porta specifica. Nell'esempio per <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code>, il percorso di riscrittura è <code>/coffee</code>. <p class= "note">Se applichi questo file e l'URL mostra una risposta <code>404</code>, la tua applicazione di back-end potrebbe essere in ascolto su un percorso che termina con `/`. Prova ad aggiungere un simbolo `/` finale a questo campo di riscrittura, quindi riapplica il file e prova di nuovo l'URL.</p></td>
</tr>
</tbody></table>

<br />


## Annotazioni buffer proxy
{: #proxy-buffer}

L'ALB Ingress funge da proxy tra la tua applicazione di back-end e il browser web del client. Con le annotazioni di buffer proxy, puoi configurare il modo in cui i dati vengono memorizzati nel buffer sul tuo ALB durante l'invio o la ricezione di pacchetti di dati.  
{: shortdesc}

### Buffer di intestazione client di grandi dimensioni (`large-client-header-buffers`)
{: #large-client-header-buffers}

Imposta il numero e la dimensione massima dei buffer che leggono intestazioni delle richieste client di grandi dimensioni.
{:shortdesc}

**Descrizione**</br>
I buffer che leggono le intestazioni delle richieste client di grandi dimensioni vengono assegnati solo in base alla domanda: se una connessione viene trasferita nello stato keepalive dopo l'elaborazione di fine richiesta,
questi buffer vengono rilasciati. Per impostazione predefinita, ci sono `4` buffer e la dimensione buffer è uguale a `8K` byte. Se una riga della richiesta supera la dimensione massima impostata di un buffer, al client viene restituito l'errore HTTP `414 Request-URI Too Large`. Inoltre, se un campo di intestazione della richiesta supera la dimensione massima impostata di un buffer, al client viene restituito l'errore `400 Bad Request`. Puoi regolare il numero e la dimensione massima dei buffer utilizzati per la lettura di intestazioni di richieste client di grandi dimensioni.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>Il numero massimo di buffer che devono essere assegnati per leggere l'intestazione della richiesta client di grandi dimensioni. Ad esempio, per impostarlo su 4, definisci <code>4</code>.</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>La dimensione massima dei buffer che leggono l'intestazione della richiesta client di grandi dimensioni. Ad esempio, per impostarla su 16 kilobyte, definisci <code>16k</code>. La dimensione deve terminare con una <code>k</code> per kilobyte o una <code>m</code> per megabyte.</td>
 </tr>
</tbody></table>

<br />


### Buffer dei dati della risposta client (`proxy-buffering`)
{: #proxy-buffering}

Utilizza l'annotazione del buffer per disabilitare l'archiviazione dei dati della risposta sull'ALB mentre i dati vengono inviati al client.
{:shortdesc}

**Descrizione**</br>
L'ALB Ingress funge da proxy tra la tua applicazione di back-end e il browser web del client. Quando una risposta viene inviata dall'applicazione di back-end al client, i dati della risposta vengono memorizzati nel buffer sull'ALB per impostazione predefinita. L'ALB trasmette tramite proxy la risposta client e inizia a inviare la risposta al client al ritmo del client. Una volta che l'ALB ha ricevuto tutti i dati dall'applicazione di back-end, la connessione all'applicazione di back-end viene chiusa. La connessione dall'ALB al client rimane aperta finché il client non riceve tutti i dati.

Se il buffer dei dati della risposta sull'ALB è disabilitato, i dati vengono immediatamente inviati dall'ALB al client. Il client deve essere in grado di gestire i dati in entrata al ritmo dell'ALB. Se il client è troppo lento, la connessione di upstream rimane aperta fino a quando il client non riesce a rimettersi al passo.

Il buffer dei dati della risposta sull'ALB è abilitato per impostazione predefinita.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>Per disabilitare il buffer dei dati della risposta nell'ALB, imposta su <code>false</code>.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code><em>&lt;myservice1&gt;</em></code> con il nome del servizio Kubernetes che hai creato per la tua applicazione. Separa più servizi con un punto e virgola (;). Questo campo è facoltativo. Se non specifichi un nome di servizio, tutti i servizi utilizzano questa annotazione.</td>
</tr>
</tbody></table>

<br />


### Buffer proxy (`proxy-buffers`)
{: #proxy-buffers}

Configura il numero e la dimensione dei buffer del proxy per l'ALB.
{:shortdesc}

**Descrizione**</br>
Imposta il numero e la dimensione dei buffer che leggono una risposta per una singola connessione dal server proxy. La configurazione viene applicata a tutti i servizi nel dominio secondario Ingress, a meno che non venga specificato un servizio. Ad esempio, se si specifica una configurazione come `serviceName=SERVICE number=2 size=1k`, al servizio viene applicato 1k. Se si specifica una configurazione quale `number=2 size=1k`, 1k viene applicato a tutti i servizi nel dominio secondario Ingress.</br>
<p class="tip">Se ricevi il messaggio di errore `upstream sent too big header while reading response header from upstream`, il server upstream nel tuo back-end ha inviato una dimensione di intestazione che è maggiore del limite predefinito. Aumenta la dimensione sia per `proxy-buffers` che per [`proxy-buffer-size`](#proxy-buffer-size).</p>

**YAML risorsa Ingress di esempio**</br>

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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome di un servizio a cui applicare proxy-buffers.</td>
</tr>
<tr>
<td><code>number</code></td>
<td>Sostituisci <code>&lt;<em>number_of_buffers</em>&gt;</code> con un numero, ad esempio <code>2</code>.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Sostituisci <code>&lt;<em>size</em>&gt;</code> con la dimensione di ciascun buffer in kilobyte (k o K), ad esempio <code>1K</code>.</td>
</tr>
</tbody>
</table>

<br />


### Dimensione buffer proxy (`proxy-buffer-size`)
{: #proxy-buffer-size}

Configura la dimensione del buffer del proxy che legge la prima parte della risposta.
{:shortdesc}

**Description**</br>
Imposta la dimensione del buffer che legge la prima parte della risposta ricevuta dal server proxy. Questa parte della risposta di solito contiene una piccola intestazione di risposta. La configurazione viene applicata a tutti i servizi nel dominio secondario Ingress, a meno che non venga specificato un servizio. Ad esempio, se si specifica una configurazione come `serviceName=SERVICE size=1k`, al servizio viene applicato 1k. Se si specifica una configurazione come `size=1k`, 1k viene applicato a tutti i servizi nel dominio secondario Ingress.

Se ricevi il messaggio di errore `upstream sent too big header while reading response header from upstream`, il server upstream nel tuo back-end ha inviato una dimensione di intestazione che è maggiore del limite predefinito. Aumenta la dimensione sia per `proxy-buffer-size` che per [`proxy-buffers`](#proxy-buffers).
{: tip}

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome di un servizio a cui applicare proxy-buffers-size.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Sostituisci <code>&lt;<em>size</em>&gt;</code> con la dimensione di ciascun buffer in kilobyte (k o K), ad esempio <code>1K</code>. Per calcolare la dimensione corretta, puoi consultare [questo post del blog ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.getpagespeed.com/server-setup/nginx/tuning-proxy_buffer_size-in-nginx). </td>
</tr>
</tbody></table>

<br />


### Dimensione buffer occupati del proxy (`proxy-busy-buffers-size`)
{: #proxy-busy-buffers-size}

Configura la dimensione dei buffer del proxy che può essere occupata.
{:shortdesc}

**Descrizione**</br>
Limita la dimensione di tutti i buffer che stanno inviando una risposta al client quando la risposta non è stata ancora completamente letta. Nel frattempo, il resto dei buffer può leggere la risposta e, se necessario, eseguire il buffer di una parte della risposta in un file temporaneo. La configurazione viene applicata a tutti i servizi nel dominio secondario Ingress, a meno che non venga specificato un servizio. Ad esempio, se si specifica una configurazione come `serviceName=SERVICE size=1k`, al servizio viene applicato 1k. Se si specifica una configurazione come `size=1k`, 1k viene applicato a tutti i servizi nel dominio secondario Ingress.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome di un servizio a cui applicare proxy-busy-buffers-size.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Sostituisci <code>&lt;<em>size</em>&gt;</code> con la dimensione di ciascun buffer in kilobyte (k o K), ad esempio <code>1K</code>.</td>
</tr>
</tbody></table>

<br />


## Annotazioni richiesta e risposta
{: #request-response}

Utilizza le annotazioni di richiesta e risposta per aggiungere o rimuovere le informazioni di intestazione dalle richieste del client e del server e per modificare la dimensione del corpo che il client può inviare.
{: shortdesc}

### Aggiungi porta server all'intestazione host (`add-host-port`)
{: #add-host-port}

Aggiungi una porta del server alla richiesta del client prima che la richiesta venga inoltrata alla tua applicazione di back-end.
{: shortdesc}

**Descrizione**</br>
Aggiungi `:server_port` all'intestazione host di una richiesta client prima di inoltrarla alla tua applicazione di back-end.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>Per abilitare l'impostazione di server_port per il dominio secondario, impostala su <code>true</code>.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code><em>&lt;myservice&gt;</em></code> con il nome del servizio Kubernetes che hai creato per la tua applicazione. Separa più servizi con un punto e virgola (;). Questo campo è facoltativo. Se non specifichi un nome di servizio, tutti i servizi utilizzano questa annotazione.</td>
</tr>
</tbody></table>

<br />


### Intestazione della risposta o della richiesta del client aggiuntiva (`proxy-add-headers`, `response-add-headers`)
{: #proxy-add-headers}

Aggiungi ulteriori informazioni di intestazione a una richiesta client prima che venga inviata all'applicazione di back-end o a una risposta client prima che venga inviata al client.
{:shortdesc}

**Descrizione**</br>
L'ALB Ingress funge da proxy tra l'applicazione client e la tua applicazione di back-end. Le richieste client inviate all'ALB vengono elaborate (tramite proxy) e inserite in una nuova richiesta che viene quindi inviata all'applicazione di back-end. Allo stesso modo, le risposte dell'applicazione di back-end inviate all'ALB vengono elaborate (tramite proxy) e inserite in una nuova richiesta che viene quindi inviata al client. La trasmissione tramite proxy di una richiesta o una risposta rimuove le informazioni dell'intestazione HTTP, come il nome utente, che erano state inizialmente inviate dal client o dall'applicazione di back-end.

Se la tua applicazione di back-end richiede le informazioni dell'intestazione HTTP, puoi utilizzare l'annotazione `proxy-add-headers` per aggiungere le informazioni di intestazione alla richiesta client prima che venga inoltrata dall'ALB all'applicazione di back-end. Se l'applicazione web client richiede le informazioni dell'intestazione HTTP, puoi utilizzare l'annotazione `response-add-headers` per aggiungere le informazioni di intestazione alla risposta prima che venga inoltrata dall'ALB all'applicazione web client.<br>

Ad esempio, potresti dover aggiungere le seguenti informazioni dell'intestazione X-Forward alla richiesta prima che venga inoltrata alla tua applicazione:
```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```
{: screen}

Per aggiungere le informazioni dell'intestazione X-Forward alla richiesta inviata alla tua applicazione, utilizza l'annotazione `proxy-add-headers` nel seguente modo:
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

L'annotazione `response-add-headers` non supporta le intestazioni globali per tutti i servizi. Per aggiungere un'intestazione per tutte le risposte del servizio a livello di server, puoi utilizzare l'[annotazione `server-snippets`](#server-snippets):
{: tip}
```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: pre}
</br>

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>La chiave delle informazioni sull'intestazione da aggiungere alla richiesta o alla risposta client.</td>
</tr>
<tr>
<td><code>&lt;value&gt;</code></td>
<td>Il valore delle informazioni sull'intestazione da aggiungere alla richiesta o alla risposta client.</td>
</tr>
</tbody></table>

<br />


### Rimozione intestazione risposta client (`response-remove-headers`)
{: #response-remove-headers}

Rimuovi le informazioni di intestazione incluse nella risposta client dall'applicazione di back-end prima che la risposta venga inviata al client.
{:shortdesc}

**Descrizione**</br>
L'ALB Ingress funge da proxy tra la tua applicazione di back-end e il browser web del client. Le risposte client dell'applicazione di back-end inviate all'ALB vengono elaborate (tramite proxy) e inserite in una nuova risposta che viene quindi inviata dall'ALB al browser web del client. Sebbene la trasmissione tramite proxy di una risposta rimuova le informazioni sull'intestazione http che erano state inizialmente inviate dall'applicazione di back-end, questo processo potrebbe non rimuovere tutte le intestazione specifiche dell'applicazione di back-end. Rimuovi le informazioni di intestazione da una risposta client prima che la risposta venga inoltrata dall'ALB al browser web del client.

**YAML risorsa Ingress di esempio**</br>

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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>La chiave dell'intestazione da rimuovere dalla risposta client.</td>
</tr>
</tbody></table>

<br />


### Dimensione corpo richiesta client (`client-max-body-size`)
{: #client-max-body-size}

Imposta la dimensione massima del corpo che il client può inviare come parte di una richiesta.
{:shortdesc}

**Descrizione**</br>
Per mantenere le prestazioni previste, la dimensione massima del corpo della richiesta client è impostata su 1 megabyte. Quando all'ALB Ingress viene inviata una richiesta client con una dimensione del corpo superiore al limite e il client non consente la divisione dei dati, l'ALB restituisce una risposta HTTP 413 (Request Entity Too Large) al client. Una connessione tra il client e l'ALB non è possibile finché non viene ridotta la dimensione del corpo della richiesta. Quando il client consente di suddividere i dati in più blocchi, i dati vengono suddivisi in pacchetti da 1 megabyte e inviati all'ALB.

Potresti voler ridurre la dimensione del corpo massima perché ti attendi richieste client con una dimensione del corpo maggiore di 1 megabyte. Ad esempio, vuoi che il tuo client sia in grado di caricare file di grandi dimensioni. L'aumento della dimensione massima del corpo della richiesta potrebbe influire sulle prestazioni del tuo ALB poiché la connessione al client deve rimanere aperta fino a quando la richiesta non viene ricevuta.

Alcuni browser web del client non possono visualizzare correttamente il messaggio di risposta HTTP 413.
{: note}

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Facoltativo: per applicare una dimensione massima del corpo del client a un servizio specifico, sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio. Se non specifichi un nome del servizio, la dimensione viene applicata a tutti i servizi. Nello YAML di esempio, il formato <code>"serviceName=&lt;myservice&gt; size=&lt;size&gt;; size=&lt;size&gt;"</code> applica la prima dimensione al servizio <code>myservice</code> e applica la seconda dimensione a tutti gli altri servizi.</li>
</tr>
<td><code>&lt;size&gt;</code></td>
<td>La dimensione massima del corpo della risposta client. Ad esempio, per impostare la dimensione massima su 200 megabyte, definisci <code>200m</code>. Puoi impostare la dimensione su 0 per disabilitare il controllo della dimensione del corpo della richiesta client.</td>
</tr>
</tbody></table>

<br />


## Annotazioni limite del servizio
{: #service-limit}

Con le annotazioni di limite del servizio, puoi modificare la velocità di elaborazione delle richieste predefinita e il numero di connessioni che possono provenire da un singolo indirizzo IP.
{: shortdesc}

### Limiti di velocità globali (`global-rate-limit`)
{: #global-rate-limit}

Limita la velocità di elaborazione delle richieste e il numero di connessioni per una chiave definita per tutti i servizi.
{:shortdesc}

**Descrizione**</br>
Per tutti i servizi, limita la velocità di elaborazione delle richieste e il numero di connessioni per una chiave definita provenienti da un singolo indirizzo IP per tutti i percorsi dei
back-end selezionati.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>key</code></td>
<td>I valori supportati sono `location`, intestazioni `$http_` e `$uri`. Per impostare un limite globale per le richieste in entrata in base alla zona o al servizio, utilizza `key=location`. Per impostare un limite globale per le richieste in entrata in base all'intestazione, utilizza `X-USER-ID key=$http_x_user_id`.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>Sostituisci <code>&lt;<em>rate</em>&gt;</code> con la velocità di elaborazione. Immetti un valore come una velocità al secondo (r/s) o al minuto (r/m). Esempio: <code>50r/m</code>.</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>Sostituisci <code>&lt;<em>number-of-connections</em>&gt;</code> con il numero di connessioni.</td>
</tr>
</tbody></table>

<br />


### Limiti di velocità del servizio (`service-rate-limit`)
{: #service-rate-limit}

Limita la velocità di elaborazione delle richieste e il numero di connessioni per i servizi specifici.
{:shortdesc}

**Descrizione**</br>
Per servizi specifici, limita la velocità di elaborazione delle richieste e il numero di connessioni per una chiave definita provenienti da un singolo indirizzo IP per tutti i percorsi dei back-end selezionati.

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio per cui vuoi limitare la velocità di elaborazione.</li>
</tr>
<tr>
<td><code>key</code></td>
<td>I valori supportati sono `location`, intestazioni `$http_` e `$uri`. Per impostare un limite globale per le richieste in entrata in base alla zona o al servizio, utilizza `key=location`. Per impostare un limite globale per le richieste in entrata in base all'intestazione, utilizza `X-USER-ID key=$http_x_user_id`.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>Sostituisci <code>&lt;<em>rate</em>&gt;</code> con la velocità di elaborazione. Per definire una velocità al secondo, utilizza r/s: <code>10r/s</code>. Per definire una velocità al minuto, utilizza r/m: <code>50r/m</code>.</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>Sostituisci <code>&lt;<em>number-of-connections</em>&gt;</code> con il numero di connessioni.</td>
</tr>
</tbody></table>

<br />


## Annotazioni di autenticazione utente
{: #user-authentication}

Utilizza l'annotazione di autenticazione utente se vuoi usare {{site.data.keyword.appid_full_notm}} per eseguire l'autenticazione con le tue applicazioni.
{: shortdesc}

### Autenticazione {{site.data.keyword.appid_short_notm}} (`appid-auth`)
{: #appid-auth}

Utilizza {{site.data.keyword.appid_full_notm}} per eseguire l'autenticazione con la tua applicazione.
{:shortdesc}

**Descrizione**</br>
Esegui l'autenticazione di richieste HTTP/HTTPS web o API con {{site.data.keyword.appid_short_notm}}.

Se imposti il tipo di richiesta su web, viene convalidata una richiesta web che contiene un token di accesso {{site.data.keyword.appid_short_notm}}. Se la convalida del token non riesce, la richiesta web viene rifiutata. Se la richiesta non contiene un token di accesso, verrà reindirizzata alla pagina di accesso di {{site.data.keyword.appid_short_notm}}. Perché l'autenticazione web {{site.data.keyword.appid_short_notm}} funzioni, è necessario abilitare i cookie nel browser dell'utente.

Se imposti il tipo di richiesta su api, viene convalidata una richiesta API che contiene un token di accesso {{site.data.keyword.appid_short_notm}}. Se la richiesta non contiene un token di accesso, l'utente riceverà il messaggio di errore 401: Unauthorized

Per motivi di sicurezza, l'autenticazione {{site.data.keyword.appid_short_notm}} supporta solo i back-end con TLS/SSL abilitato.
{: note}

**YAML risorsa Ingress di esempio**</br>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>bindSecret</code></td>
<td>Sostituisci <em><code>&lt;bind_secret&gt;</code></em> con il segreto Kubernetes che memorizza il segreto di bind per la tua istanza di servizio {{site.data.keyword.appid_short_notm}}.</td>
</tr>
<tr>
<td><code>namespace</code></td>
<td>Sostituisci <em><code>&lt;namespace&gt;</code></em> con lo spazio dei nomi del segreto di bind. Il valore predefinito di questo campo è lo spazio dei nomi `default`.</td>
</tr>
<tr>
<td><code>requestType</code></td>
<td>Sostituisci <code><em>&lt;request_type&gt;</em></code> con il tipo di richiesta che vuoi inviare a {{site.data.keyword.appid_short_notm}}. I valori accettati sono `web` o `api`. Il valore predefinito è `api`.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code><em>&lt;myservice&gt;</em></code> con il nome del servizio Kubernetes che hai creato per la tua applicazione. Questo campo è obbligatorio. Se non si include un nome di servizio, l'annotazione viene abilitata per tutti i servizi.  Se si include un nome di servizio, l'annotazione viene abilitata solo per tale servizio. Separa più servizi con una virgola (,).</td>
</tr>
<tr>
<td><code>idToken=false</code></td>
<td>Facoltativo: il client OIDC Liberty non è in grado di analizzare contemporaneamente sia il token di accesso che quello di identità. Quando lavori con Liberty, imposta questo valore su false in modo che il token di identità non venga inviato al server Liberty.</td>
</tr>
</tbody></table>

**Utilizzo**</br>

Poiché l'applicazione utilizza {{site.data.keyword.appid_short_notm}} per l'autenticazione, devi eseguire il provisioning di un'istanza {{site.data.keyword.appid_short_notm}}, configurare l'istanza con URI di reindirizzamento validi e generare un segreto di bind eseguendo il bind dell'istanza al tuo cluster.

1. Scegli un'istanza esistente oppure crea una nuova istanza {{site.data.keyword.appid_short_notm}}.
  * Per utilizzare un'istanza esistente, assicurati che il nome dell'istanza del servizio non contenga spazi. Per rimuovere gli spazi, seleziona il menu di opzioni aggiuntive accanto al nome della tua istanza di servizio e seleziona **Ridenomina servizio**.
  * Per eseguire il provisioning di una [nuova istanza {{site.data.keyword.appid_short_notm}}](https://cloud.ibm.com/catalog/services/app-id):
      1. Sostituisci il **Nome servizio** inserito automaticamente con il tuo nome univoco per l'istanza di servizio. Il nome dell'istanza del servizio non può contenere spazi.
      2. Scegli la stessa regione in cui è distribuito il tuo cluster.
      3. Fai clic su **Crea**.

2. Aggiungi gli URL di reindirizzamento per la tua applicazione. Un URL di reindirizzamento è l'endpoint di callback della tua applicazione. Per impedire attacchi di phishing, l'ID dell'applicazione convalida l'URL richiesto con la whitelist degli URL di reindirizzamento.
  1. Nella console di gestione {{site.data.keyword.appid_short_notm}}, passa a **Gestisci autenticazione**.
  2. Nella scheda **Provider di identità**, assicurati di avere un Provider di identità selezionato. Se non vengono selezionati provider di identità, verrà emesso un token di accesso per l'accesso anonimo all'applicazione.
  3. Nella scheda **Impostazioni di autenticazione**, aggiungi gli URL di reindirizzamento per la tua applicazione nel formato `http://<hostname>/<app_path>/appid_callback` o `https://<hostname>/<app_path>/appid_callback`.

    {{site.data.keyword.appid_full_notm}} offre una funzione di disconnessione: se `/logout` è presente nel tuo percorso di {{site.data.keyword.appid_full_notm}}, i cookie vengono rimossi e l'utente viene rimandato alla pagina di accesso. Per utilizzare questa funzione, devi aggiungere `/appid_logout` al tuo dominio nel formato `https://<hostname>/<app_path>/appid_logout` e includere questo URL nell'elenco degli URL di reindirizzamento.
    {: note}

3. Esegui il bind dell'istanza di servizio {{site.data.keyword.appid_short_notm}} al tuo cluster. Il comando crea una chiave di servizio per l'istanza del servizio oppure puoi includere l'indicatore `--key` per utilizzare le credenziali della chiave di servizio esistente.
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>]
  ```
  {: pre}
  Dopo che
il servizio è stato correttamente associato al tuo cluster, viene creato un segreto cluster che contiene le credenziali
della tua istanza del servizio. Output CLI di esempio:
  ```
  ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service appid1
    Binding service instance to namespace...
    OK
    Namespace:    mynamespace
    Secret name:  binding-<service_instance_name>
  ```
  {: screen}

4. Richiama il segreto creato nel tuo spazio dei nomi del cluster.
  ```
  kubectl get secrets --namespace=<namespace>
  ```
  {: pre}

5. Utilizza il segreto di bind e lo spazio dei nomi del cluster per aggiungere l'annotazione `appid-auth` alla tua risorsa Ingress.


