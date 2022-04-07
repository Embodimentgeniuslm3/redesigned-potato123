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


# Registrazione e monitoraggio di Ingress
{: #ingress_health}

Personalizza la registrazione e configura il monitoraggio per un supporto nella risoluzione dei problemi e per migliorare le prestazioni della tua configurazione Ingress.
{: shortdesc}

## Visualizzazione dei log Ingress
{: #ingress_logs}

Se vuoi risolvere i problemi o monitorare l'attività di Ingress, puoi esaminare i log Ingress.
{: shortdesc}

I log vengono raccolti automaticamente per i tuoi ALB Ingress Per visualizzare i log ALB, scegli tra due opzioni.
* [Crea una configurazione di registrazione per il servizio Ingress](/docs/containers?topic=containers-health#configuring) nel tuo cluster.
* Controlla i log dalla CLI. **Nota**: devi disporre almeno del [ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Lettore**](/docs/containers?topic=containers-users#platform) per lo spazio dei nomi `kube-system`.
    1. Ottieni l'ID di un pod per un ALB.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. Apri i log relativi a tale pod ALB.
        ```
        kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

</br>Il contenuto del log Ingress predefinito è formattato in JSON e visualizza i campi comuni che descrivono la sessione di connessione tra un client e la tua applicazione. Un log di esempio con i campi predefiniti è simile al seguente:

```
{"time_date": "2018-08-21T17:33:19+00:00", "client": "108.162.248.42", "host": "albhealth.multizone.us-south.containers.appdomain.cloud", "scheme": "http", "request_method": "GET", "request_uri": "/", "request_id": "c2bcce90cf2a3853694da9f52f5b72e6", "status": 200, "upstream_addr": "192.168.1.1:80", "upstream_status": 200, "request_time": 0.005, "upstream_response_time": 0.005, "upstream_connect_time": 0.000, "upstream_header_time": 0.005}
```
{: screen}

<table>
<caption>Descrizione dei campi nel formato di log Ingress predefinito</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei campi nel formato di log Ingress predefinito</th>
</thead>
<tbody>
<tr>
<td><code>"time_date": "$time_iso8601"</code></td>
<td>L'ora locale nel formato standard ISO 8601 quando viene scritto il log.</td>
</tr>
<tr>
<td><code>"client": "$remote_addr"</code></td>
<td>L'indirizzo IP del pacchetto di richieste che il client ha inviato alla tua applicazione. Questo IP può cambiare in base alle seguenti situazioni:<ul><li>Quando una richiesta client alla tua applicazione viene inviata al tuo cluster, la richiesta viene instradata a un pod per il servizio di programma di bilanciamento del carico che espone l'ALB. Se sullo stesso nodo di lavoro del pod del servizio del programma di bilanciamento del carico non esiste un pod dell'applicazione, il programma di bilanciamento inoltra la richiesta a un pod dell'applicazione su un nodo di lavoro diverso. L'indirizzo IP di origine del pacchetto di richieste viene modificato con l'indirizzo IP pubblico del nodo di lavoro su cui è in esecuzione il pod dell'applicazione.</li><li>Se la [conservazione dell'IP di origine è abilitata](/docs/containers?topic=containers-ingress#preserve_source_ip), viene invece registrato l'indirizzo IP originale della richiesta client alla tua applicazione.</li></ul></td>
</tr>
<tr>
<td><code>"host": "$http_host"</code></td>
<td>L'host, o il dominio secondario, tramite cui sono accessibili le tue applicazioni. Questo dominio secondario è configurato nei file di risorse Ingress per i tuoi ALB.</td>
</tr>
<tr>
<td><code>"scheme": "$scheme"</code></td>
<td>Il tipo di richiesta: <code>HTTP</code> o <code>HTTPS</code>.</td>
</tr>
<tr>
<td><code>"request_method": "$request_method"</code></td>
<td>Il metodo della chiamata di richiesta all'applicazione di back-end, come ad esempio <code>GET</code> o <code>POST</code>.</td>
</tr>
<tr>
<td><code>"request_uri": "$uri"</code></td>
<td>L'URI della richiesta originale al tuo percorso dell'applicazione. Gli ALB elaborano i percorsi su cui le applicazioni sono in ascolto sotto forma di prefissi. Alla ricezione di una richiesta da un client a un'applicazione, l'ALB controlla la risorsa Ingress per rilevare un percorso (come prefisso) che corrisponda al percorso nell'URI della richiesta.</td>
</tr>
<tr>
<td><code>"request_id": "$request_id"</code></td>
<td>Un identificativo richiesta univoco generato da 16 byte casuali.</td>
</tr>
<tr>
<td><code>"status": $status</code></td>
<td>Il codice di stato per la sessione di connessione.<ul>
<li><code>200</code>: la sessione è stata completata correttamente</li>
<li><code>400</code>: impossibile analizzare i dati del client</li>
<li><code>403</code>: accesso non consentito, ad esempio quando l'accesso è limitato per specifici indirizzi IP client</li>
<li><code>500</code>: errore server interno</li>
<li><code>502</code>: gateway non valido; ad esempio, se non è possibile selezionare o raggiungere un server upstream</li>
<li><code>503</code>: servizio non disponibile; ad esempio quando l'accesso è limitato dal numero di connessioni</li>
</ul></td>
</tr>
<tr>
<td><code>"upstream_addr": "$upstream_addr"</code></td>
<td>L'indirizzo IP e la porta o il percorso al socket del dominio UNIX del server upstream. Se vengono contattati diversi server durante l'elaborazione della richiesta, i loro indirizzi sono separati da virgole: <code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock"</code>. Se la richiesta viene reindirizzata internamente da un gruppo di server a un altro, gli indirizzi server da gruppi differenti sono separati da due punti: <code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock: 192.168.10.1:80, 192.168.10.2:80"</code>. Se l'ALB non riesce a selezionare un server, viene invece registrato il nome del gruppo di server.</td>
</tr>
<tr>
<td><code>"upstream_status": $upstream_status</code></td>
<td>Il codice di stato della risposta ottenuta dal server upstream per l'applicazione di back-end, come i codici di risposta HTTP standard. I codici di stato di diverse risposte sono separati da virgole e da due punti come indirizzi nella variabile <code>$upstream_addr</code>. Se l'ALB non riesce a selezionare un server, viene registrato il codice di stato 502 (Bad Gateway).</td>
</tr>
<tr>
<td><code>"request_time": $request_time</code></td>
<td>Il tempo di elaborazione della richiesta, misurato in secondi con una risoluzione di millisecondi. Questo tempo inizia quando l'ALB legge i primi byte della richiesta del client e si arresta quando l'ALB invia gli ultimi byte della risposta al client. Il log viene scritto subito dopo che il tempo di elaborazione della richiesta si arresta.</td>
</tr>
<tr>
<td><code>"upstream_response_time": $upstream_response_time</code></td>
<td>Il tempo impiegato dall'ALB per ricevere la risposta dal server upstream per l'applicazione di back-end, misurato in secondi con una risoluzione di millisecondi. I tempi di diverse risposte sono separati da virgole e da due punti come indirizzi nella variabile <code>$upstream_addr</code>.</td>
</tr>
<tr>
<td><code>"upstream_connect_time": $upstream_connect_time</code></td>
<td>Il tempo impiegato dall'ALB per stabilire una connessione con il server upstream per l'applicazione di back-end, misurato in secondi con una risoluzione di millisecondi. Se TLS/SSL è abilitato nella tua configurazione della risorsa Ingress, questo tempo include il tempo impiegato per l'handshake. I tempi di diverse connessioni sono separati da virgole e da due punti come indirizzi nella variabile <code>$upstream_addr</code> .</td>
</tr>
<tr>
<td><code>"upstream_header_time": $upstream_header_time</code></td>
<td>Il tempo impiegato dall'ALB per ricevere l'intestazione della risposta dal server upstream per l'applicazione di back-end, misurato in secondi con una risoluzione di millisecondi. I tempi di diverse connessioni sono separati da virgole e da due punti come indirizzi nella variabile <code>$upstream_addr</code> .</td>
</tr>
</tbody></table>

## Personalizzazione del formato e del contenuto del log Ingress
{: #ingress_log_format}

Puoi personalizzare il contenuto e il formato dei log raccolti per l'ALB Ingress.
{:shortdesc}

Per impostazione predefinita, il formato dei log Ingress è JSON e mostrano i campi log comuni. Tuttavia, puoi anche creare un formato di log personalizzato scegliendo quali componenti di log vengono inoltrati e come sono disposti nell'output del log

Prima di iniziare, assicurati di disporre del [ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Scrittore** o **Gestore**](/docs/containers?topic=containers-users#platform) per lo spazio dei nomi `kube-system`.

1. Modifica il file di configurazione per la risorsa della mappa di configurazione `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Aggiungi una sezione <code>data</code>. Aggiungi il campo `log-format` e, facoltativamente, il campo `log-format-escape-json`.

    ```
    apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    <table>
    <caption>Componenti del file YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione della configurazione del formato di log</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>Sostituisci <code>&lt;key&gt;</code> con il nome del componente di log e <code>&lt;log_variable&gt;</code> con una variabile per il componente di cui desideri raccogliere le voci di log. Puoi includere il testo e la punteggiatura che desideri nella voce di log, ad esempio le virgolette nei valori stringa e le virgole per separare i componenti di log. Ad esempio, se formatti il componente utilizzando <code>request: "$request"</code>, generi quanto segue in una voce del log: <code>request: "GET / HTTP/1.1"</code> . Per un elenco di tutte le variabili che puoi utilizzare, vedi l'<a href="http://nginx.org/en/docs/varindex.html">indice delle variabili NGINX</a>.<br><br>Per registrare un'intestazione aggiuntiva come ad esempio <em>x-custom-ID</em>, aggiungi la seguente coppia chiave-valore al contenuto del log personalizzato: <br><pre class="codeblock"><code>customID: $http_x_custom_id</code></pre> <br>I trattini (<code>-</code>) vengono convertiti in caratteri di sottolineatura (<code>_</code>) e <code>$http_</code> deve essere aggiunto come prefisso al nome intestazione personalizzato.</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>Facoltativo: per impostazione predefinita, i log vengono generati in formato testo. Per generare i log in formato JSON, aggiungi il campo <code>log-format-escape-json</code> e utilizza il valore <code>true</code>.</td>
    </tr>
    </tbody></table>

    Ad esempio, il tuo formato di log potrebbe contenere le seguenti variabili:
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    Una voce di log basata su questo formato sarà simile al seguente esempio:
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    Per creare un formato di log personalizzato basato sul formato predefinito per i log ALB, modifica la seguente sezione come necessario e aggiungila alla tua mappa di configurazione:
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. Salva il file di configurazione.

5. Verifica che le modifiche alla mappa di configurazione siano state applicate.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

4. Per visualizzare i log ALB di Ingress, scegli tra due opzioni.
    * [Crea una configurazione di registrazione per il servizio Ingress](/docs/containers?topic=containers-health#logging) nel tuo cluster.
    * Controlla i log dalla CLI.
        1. Ottieni l'ID di un pod per un ALB.
            ```
            kubectl get pods -n kube-system | grep alb
            ```
            {: pre}

        2. Apri i log relativi a tale pod ALB. Verifica che tutti i log seguano il formato aggiornato.
            ```
            kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
            ```
            {: pre}

<br />


## Monitoraggio dell'ALB Ingress
{: #ingress_monitoring}

Monitora i tuoi ALB distribuendo un exporter di metriche e un agent Prometheus nel tuo cluster.
{: shortdesc}

L'exporter di metriche ALB utilizza la direttiva NGINX, `vhost_traffic_status_zone`, per raccogliere i dati delle metriche dall'endpoint `/status/format/json` su ogni pod ALB Ingress. L'exporter di metriche riformatta automaticamente ogni campo di dati nel file JSON in una metrica leggibile da Prometheus. Quindi, un agent Prometheus recupera le metriche prodotte dall'exporter e le rende visibili su un dashboard Prometheus.

### Installazione del grafico Helm per l'exporter di metriche
{: #metrics-exporter}

Installa il grafico Helm per l'exporter di metriche per monitorare un ALB nel tuo cluster.
{: shortdesc}

I pod dell'exporter di metriche ALB devono essere distribuiti sugli stessi nodi di lavoro in cui sono distribuiti i tuoi ALB. Se i tuoi ALB vengono eseguiti su nodi di lavoro edge e tali nodi edge sono contaminati per evitare altre distribuzioni del carico di lavoro, i pod dell'exporter di metriche non possono essere pianificati. Devi rimuovere le contaminazioni eseguendo `kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-`.
{: note}

1.  **Importante**: [segui le istruzioni](/docs/containers?topic=containers-helm#public_helm_install) per installare il client Helm sulla tua macchina locale, installare il server Helm (tiller) con un account di servizio e aggiungere i repository Helm {{site.data.keyword.Bluemix_notm}}.

2. Installa il grafico Helm `ibmcloud-alb-metrics-exporter` sul tuo cluster. Questo grafico Helm distribuisce un exporter di metriche ALB e crea un account di servizio `alb-metrics-service-account` nello spazio dei nomi `kube-system`. Sostituisci <alb-ID> con l'ID dell'ALB per il quale vuoi raccogliere le metriche. Per visualizzare gli ID per gli ALB nel tuo cluster, esegui <code>ibmcloud ks albs --cluster &lt;cluster_name&gt;</code>.
  Devi distribuire un grafico per ogni ALB che vuoi monitorare.
  {: note}
  ```
  helm install iks-charts/ibmcloud-alb-metrics-exporter --name ibmcloud-alb-metrics-exporter --set metricsNameSpace=kube-system --set albId=<alb-ID>
  ```
  {: pre}

3. Controlla lo stato di distribuzione del grafico. Quando il grafico è pronto, il campo **STATO** vicino alla parte superiore dell'output ha un valore di `DEPLOYED`.
  ```
  helm status ibmcloud-alb-metrics-exporter
  ```
  {: pre}

4. Verifica che i pod `ibmcloud-alb-metrics-exporter` siano in esecuzione.
  ```
  kubectl get pods -n kube-system -o wide
  ```
  {:pre}

  Output di esempio:
  ```
  NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
  ...
  alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  ```
  {:screen}

5. Facoltativo: [installa l'agent Prometheus](#prometheus-agent) per raccogliere le metriche prodotte dall'exporter e rendere visibili le metriche su un dashboard Prometheus.

### Installazione del grafico Helm per l'agente Prometheus
{: #prometheus-agent}

Una volta installato l'[exporter di metriche](#metrics-exporter), puoi installare il grafico Helm per l'agente Prometheus per acquisire le metriche prodotte dall'exporter e renderle visibili su un dashboard Prometheus.
{: shortdesc}

1. Scarica il file TAR per il grafico Helm per l'exporter di metriche da https://icr.io/helm/iks-charts/charts/ibmcloud-alb-metrics-exporter-1.0.7.tgz

2. Passa alla sottocartella Prometheus.
  ```
  cd ibmcloud-alb-metrics-exporter-1.0.7.tar/ibmcloud-alb-metrics-exporter/subcharts/prometheus
  ```
  {: pre}

3. Installa il grafico Helm Prometheus nel tuo cluster. Sostituisci <ingress_subdomain> con il dominio secondario Ingress per il tuo cluster. L'URL per il dashboard Prometheus è una combinazione del dominio secondario Prometheus predefinito, `prom-dash`, e del tuo dominio secondario di Ingress, ad esempio `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`. Per trovare il dominio secondario Ingress per il tuo cluster, esegui <code>ibmcloud ks cluster-get --cluster &lt;cluster_name&gt;</code>.
  ```
  helm install --name prometheus . --set nameSpace=kube-system --set hostName=prom-dash.<ingress_subdomain>
  ```
  {: pre}

4. Controlla lo stato di distribuzione del grafico. Quando il grafico è pronto, il campo **STATO** vicino alla parte superiore dell'output ha un valore di `DEPLOYED`.
    ```
    helm status prometheus
    ```
    {: pre}

5. Verifica che il pod `prometheus` sia in esecuzione.
    ```
    kubectl get pods -n kube-system -o wide
    ```
    {:pre}

    Output di esempio:
    ```
    NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
    alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    prometheus-9fbcc8bc7-2wvbk                       1/1       Running     0          1m        172.30.xxx.xxx   10.xxx.xx.xxx
    ```
    {:screen}

6. In un browser, immetti l'URL per il dashboard Prometheus. Questo nome host ha il formato `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`. Si apre il dashboard Prometheus per il tuo ALB.

7. Esamina le ulteriori informazioni sulle metriche [ALB](#alb_metrics), [server](#server_metrics) e [upstream](#upstream_metrics) elencate nel dashboard.

### Metriche ALB
{: #alb_metrics}

L'`alb-metrics-exporter` riformatta automaticamente ogni campo di dati nel file JSON in una metrica leggibile da Prometheus. Le metriche ALB raccolgono dati sulle connessioni e sulle risposte gestite dall'ALB.
{: shortdesc}

Le metriche ALB sono nel formato `kube_system_<ALB-ID>_<METRIC-NAME> <VALUE>`. Ad esempio, se un ALB riceve 23 risposte con codici di stato di livello 2xx-, la metrica viene formattata come `kube_system_public_crf02710f54fcc40889c301bfd6d5b77fe_alb1_totalHandledRequest {.. metric="2xx"} 23` dove `metric` è l'etichetta di prometheus.

La seguente tabella elenca i nomi di metrica ALB supportati con le etichette della metrica nel formato `<ALB_metric_name>_<metric_label>`
<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Metriche ALB supportate</th>
</thead>
<tbody>
<tr>
<td><code>connections_reading</code></td>
<td>Il numero totale di connessioni client di lettura.</td>
</tr>
<tr>
<td><code>connections_accepted</code></td>
<td>Il numero totale di connessioni client accettate.</td>
</tr>
<tr>
<td><code>connections_active</code></td>
<td>Il numero totale di connessioni client attive.</td>
</tr>
<tr>
<td><code>connections_handled</code></td>
<td>Il numero totale di connessioni client gestite.</td>
</tr>
<tr>
<td><code>connections_requests</code></td>
<td>Il numero totale di connessioni client richieste.</td>
</tr>
<tr>
<td><code>connections_waiting</code></td>
<td>Il numero totale di connessioni client in attesa.</td>
</tr>
<tr>
<td><code>connections_writing</code></td>
<td>Il numero totale di connessioni client di scrittura.</td>
</tr>
<tr>
<td><code>totalHandledRequest_1xx</code></td>
<td>Il numero di risposte con i codici di stato 1xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_2xx</code></td>
<td>Il numero di risposte con i codici di stato 2xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_3xx</code></td>
<td>Il numero di risposte con i codici di stato 3xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_4xx</code></td>
<td>Il numero di risposte con i codici di stato 4xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_5xx</code></td>
<td>Il numero di risposte con i codici di stato 5xx.</td>
</tr>
<tr>
<td><code>totalHandledRequest_total</code></td>
<td>Il numero totale di richieste client ricevute dai client.</td>
  </tr></tbody>
</table>

### Metriche del server
{: #server_metrics}

L'`alb-metrics-exporter` riformatta automaticamente ogni campo di dati nel file JSON in una metrica leggibile da Prometheus. Le metriche del server raccolgono i dati sul dominio secondario definito in una risorsa Ingress; ad esempio, `dev.demostg1.stg.us.south.containers.appdomain.cloud`.
{: shortdesc}

Le metriche del server sono nel formato `kube_system_server_<ALB-ID>_<SUB-TYPE>_<SERVER-NAME>_<METRIC-NAME> <VALUE>`.

`<SERVER-NAME>_<METRIC-NAME>` sono formattati come etichette. Ad esempio, `albId="dev_demostg1_us-south_containers_appdomain_cloud",metric="out"`

Ad esempio, se il server ha inviato un totale di 22319 byte ai client, la metrica viene formattata come:
```
kube_system_server_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="dev_demostg1_us-south_containers_appdomain_cloud",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="out",pod_template_hash="3805183417"} 22319
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione del formato delle metriche del server</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>L'ID ALB. Nell'esempio precedente, l'ID ALB è <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>.</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>Il sottotipo di metrica. Ogni sottotipo corrisponde a uno o più nomi di metrica.
<ul>
<li><code>bytes</code> e <code>processing\_time</code> corrispondono alle metriche <code>in</code> e <code>out</code>.</li>
<li><code>cache</code> corrisponde alle metriche <code>bypass</code>, <code>expired</code>, <code>hit</code>, <code>miss</code>, <code>revalidated</code>, <code>scare</code>, <code>stale</code> e <code>updating</code>.</li>
<li><code>requests</code> corrisponde alle metriche <code>requestMsec</code>, <code>1xx</code>, <code>2xx</code>, <code>3xx</code>, <code>4xx</code>, <code>5xx</code> e <code>total</code>.</li></ul>
Nell'esempio precedente, il sottotipo è <code>bytes</code>.</td>
</tr>
<tr>
<td><code>&lt;SERVER-NAME&gt;</code></td>
<td>Il nome del server definito nella risorsa Ingress. Per mantenere la compatibilità con Prometheus, i punti (<code>.</code>) vengono sostituiti da caratteri di sottolineatura <code>(\_)</code>. Nell'esempio precedente, il nome del server è <code>dev_demostg1_stg_us_south_containers_appdomain_cloud</code>.</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>Il nome del tipo di metrica raccolto. Per un elenco di nomi di metrica, vedi la seguente tabella "Metriche del server supportate". Nell'esempio precedente, il nome di metrica è <code>out</code>.</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>Il valore della metrica raccolta. Nell'esempio precedente, il valore è <code>22319</code>.</td>
</tr>
</tbody></table>

La seguente tabella elenca i nomi di metrica del server supportati.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Metriche del server supportate</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>Il numero totale di byte ricevuti dai client.</td>
</tr>
<tr>
<td><code>out</code></td>
<td>Il numero totale di byte inviati ai client.</td>
</tr>
<tr>
<td><code>bypass</code></td>
<td>Il numero di volte in cui un elemento memorizzabile nella cache è stato recuperato dal server di origine perché non ha soddisfatto la soglia per essere nella cache (ad esempio, numero di richieste).</td>
</tr>
<tr>
<td><code>expired</code></td>
<td>Il numero di volte in cui un elemento è stato trovato nella cache ma non è stato selezionato perché scaduto.</td>
</tr>
<tr>
<td><code>hit</code></td>
<td>Il numero di volte in cui un elemento valido è stato selezionato dalla cache.</td>
</tr>
<tr>
<td><code>miss</code></td>
<td>Il numero di volte in cui nessun elemento di cache valido è stato trovato nella cache e il server ha recuperato l'elemento dal server di origine.</td>
</tr>
<tr>
<td><code>revalidated</code></td>
<td>Il numero di volte in cui un elemento scaduto nella cache è stato riconvalidato.</td>
</tr>
<tr>
<td><code>scarce</code></td>
<td>Il numero di volte in cui la cache ha rimosso gli elementi usati raramente o con priorità bassa per liberare la memoria.</td>
</tr>
<tr>
<td><code>stale</code></td>
<td>Il numero di volte in cui un elemento scaduto è stato trovato nella cache, ma poiché un'altra richiesta ha causato il recupero dell'elemento dal server di origine, l'elemento è stato selezionato dalla cache.</td>
</tr>
<tr>
<td><code>updating</code></td>
<td>Il numero di volte in cui il contenuto obsoleto è stato aggiornato.</td>
</tr>
<tr>
<td><code>requestMsec</code></td>
<td>La media dei tempi di elaborazione delle richieste in millisecondi.</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>Il numero di risposte con i codici di stato 1xx.</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>Il numero di risposte con i codici di stato 2xx.</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>Il numero di risposte con i codici di stato 3xx.</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>Il numero di risposte con i codici di stato 4xx.</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>Il numero di risposte con i codici di stato 5xx.</td>
</tr>
<tr>
<td><code>total</code></td>
<td>Il numero totale di risposte con i codici di stato.</td>
  </tr></tbody>
</table>

### Metriche di upstream
{: #upstream_metrics}

L'`alb-metrics-exporter` riformatta automaticamente ogni campo di dati nel file JSON in una metrica leggibile da Prometheus. Le metriche di upstream raccolgono i dati sul servizio di back-end definito in una risorsa Ingress.
{: shortdesc}

Le metriche di upstream sono formattate in due modi.
* Il [Tipo 1](#type_one) include il nome del servizio upstream.
* Il [Tipo 2](#type_two) include il nome del servizio upstream e l'indirizzo IP di uno specifico pod upstream.

#### Metriche di upstream di tipo 1
{: #type_one}

Le metriche di upstream di tipo 1 sono nel formato `kube_system_upstream_<ALB-ID>_<SUB-TYPE>_<UPSTREAM-NAME>_<METRIC-NAME> <VALUE>`.
{: shortdesc}

`<UPSTREAM-NAME>_<METRIC-NAME>` sono formattati come etichette. Ad esempio, `albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",metric="in"`

Ad esempio, se il servizio upstream ha ricevuto un totale di 1227 byte dall'ALB, la metrica viene formattata come:
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_bytes{albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc",app="alb-metrics-exporter",instance="172.30.140.68:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-8wfw4",metric="in",pod_template_hash="3805183417"} 1227
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione del formato delle metriche di upstream di tipo 1</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>L'ID ALB. Nell'esempio precedente, l'ID ALB è <code>public\_crf02710f54fcc40889c301bfd6d5b77fe\_alb1</code>.</td>
</tr>
<tr>
<td><code>&lt;SUB-TYPE&gt;</code></td>
<td>Il sottotipo di metrica. I valori supportati sono <code>bytes</code>, <code>processing\_time</code> e <code>requests</code>. Nell'esempio precedente, il sottotipo è <code>bytes</code>.</td>
</tr>
<tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>Il nome del servizio upstream definito nella risorsa Ingress. Per mantenere la compatibilità con Prometheus, i punti (<code>.</code>) vengono sostituiti da caratteri di sottolineatura <code>(\_)</code>. Nell'esempio precedente, il nome upstream è <code>default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc</code>.</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>Il nome del tipo di metrica raccolto. Per un elenco di nomi di metrica, vedi la seguente tabella "Metriche di upstream di tipo 1 supportate". Nell'esempio precedente, il nome di metrica è <code>in</code>.</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>Il valore della metrica raccolta. Nell'esempio precedente, il valore è <code>1227</code>.</td>
</tr>
</tbody></table>

La seguente tabella elenca i nomi di metrica di upstream di tipo 1 supportati.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Metriche di upstream di tipo 1 supportate</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>Il numero totale di byte ricevuti dal server ALB.</td>
</tr>
<tr>
<td><code>out</code></td>
<td>Il numero totale di byte inviati al server ALB.</td>
</tr>
<tr>
<td><code>1xx</code></td>
<td>Il numero di risposte con i codici di stato 1xx.</td>
</tr>
<tr>
<td><code>2xx</code></td>
<td>Il numero di risposte con i codici di stato 2xx.</td>
</tr>
<tr>
<td><code>3xx</code></td>
<td>Il numero di risposte con i codici di stato 3xx.</td>
</tr>
<tr>
<td><code>4xx</code></td>
<td>Il numero di risposte con i codici di stato 4xx.</td>
</tr>
<tr>
<td><code>5xx</code></td>
<td>Il numero di risposte con i codici di stato 5xx.</td>
</tr>
<tr>
<td><code>total</code></td>
<td>Il numero totale di risposte con i codici di stato.</td>
  </tr></tbody>
</table>

#### Metriche di upstream di tipo 2
{: #type_two}

Le metriche di upstream di tipo 2 sono nel formato `kube_system_upstream_<ALB-ID>_<METRIC-NAME>_<UPSTREAM-NAME>_<POD-IP> <VALUE>`.
{: shortdesc}

`<UPSTREAM-NAME>_<POD-IP>` sono formattati come etichette. Ad esempio, `albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",backend="172_30_75_6_80"`

Ad esempio, se il servizio upstream ha un tempo medio di elaborazione delle richieste (incluso l'upstream) di 40 millisecondi, la metrica viene formattata come:
```
kube_system_upstream_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_requestMsec{albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc",app="alb-metrics-exporter",backend="172_30_75_6_80",instance="172.30.75.3:9913",job="kubernetes-pods",kubernetes_namespace="kube-system",kubernetes_pod_name="alb-metrics-exporter-7d495d785c-swkls",pod_template_hash="3805183417"} 40
```

{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione del formato delle metriche di upstream di tipo 2</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>L'ID ALB. Nell'esempio precedente, l'ID ALB è <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>.</td>
</tr>
<td><code>&lt;UPSTREAM-NAME&gt;</code></td>
<td>Il nome del servizio upstream definito nella risorsa Ingress. Per mantenere la compatibilità con Prometheus, i punti (<code>.</code>) vengono sostituiti da caratteri di sottolineatura (<code>\_</code>). Nell'esempio precedente, il nome upstream è <code>demostg1\_stg\_us\_south\_containers\_appdomain\_cloud\_tea\_svc</code>.</td>
</tr>
<tr>
<td><code>&lt;POD\_IP&gt;</code></td>
<td>L'indirizzo IP e la porta di uno specifico pod del servizio upstream. Per mantenere la compatibilità con Prometheus, i punti (<code>.</code>) e i due punti (<code>:</code>) vengono sostituiti da caratteri di sottolineatura <code>(_)</code>. Nell'esempio precedente, l'ID del pod upstream è <code>172_30_75_6_80</code>.</td>
</tr>
<tr>
<td><code>&lt;METRIC_NAME&gt;</code></td>
<td>Il nome del tipo di metrica raccolto. Per un elenco di nomi di metrica, vedi la seguente tabella "Metriche di upstream di tipo 2 supportate". Nell'esempio precedente, il nome di metrica è <code>requestMsec</code>.</td>
</tr>
<tr>
<td><code>&lt;VALUE&gt;</code></td>
<td>Il valore della metrica raccolta. Nell'esempio precedente, il valore è <code>40</code>.</td>
</tr>
</tbody></table>

La seguente tabella elenca i nomi di metrica di upstream di tipo 2 supportati.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Metriche di upstream di tipo 2 supportate</th>
</thead>
<tbody>
<tr>
<td><code>requestMsec</code></td>
<td>La media dei tempi di elaborazione delle richieste, incluso l'upstream, in millisecondi.</td>
</tr>
<tr>
<td><code>responseMsec</code></td>
<td>La media dei soli tempi di elaborazione delle risposte upstream in millisecondi.</td>
  </tr></tbody>
</table>

<br />


## Aumento della dimensione della zona di memoria condivisa per la raccolta di metriche Ingress
{: #vts_zone_size}

Le zone di memoria condivise sono definite in modo che i processi del nodo di lavoro possano condividere informazioni come cache, persistenza della sessione e limiti di velocità. Una zona di memoria condivisa, detta zona di stato del traffico host virtuale, viene impostata per raccogliere i dati delle metriche per un ALB.
{:shortdesc}

Nella mappa di configurazione Ingress `ibm-cloud-provider-ingress-cm`, il campo `vts-status-zone-size` imposta la dimensione della zona di memoria condivisa per la raccolta dei dati delle metriche. Per impostazione predefinita, `vts-status-zone-size` è impostato su `10m`. Se hai un ambiente di grandi dimensioni che richiede più memoria per la raccolta delle metriche, puoi sovrascrivere il valore predefinito invece di usare un valore più grande seguendo questa procedura.

Prima di iniziare, assicurati di disporre del [ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Scrittore** o **Gestore**](/docs/containers?topic=containers-users#platform) per lo spazio dei nomi `kube-system`.

1. Modifica il file di configurazione per la risorsa della mappa di configurazione `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Modifica il valore di `vts-status-zone-size` da `10m` a un valore più grande.

   ```
   apiVersion: v1
   data:
     vts-status-zone-size: "10m"
   kind: ConfigMap
   metadata:
     name: ibm-cloud-provider-ingress-cm
     namespace: kube-system
   ```
   {: codeblock}

3. Salva il file di configurazione.

4. Verifica che le modifiche alla mappa di configurazione siano state applicate.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}
