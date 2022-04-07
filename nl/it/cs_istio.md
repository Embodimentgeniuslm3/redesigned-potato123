---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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



# Utilizzo del componente aggiuntivo Istio gestito (beta)
{: #istio}

Istio su {{site.data.keyword.containerlong}} fornisce una facile installazione di Istio, aggiornamenti automatici e gestione del ciclo di vita dei componenti del piano di controllo Istio e integrazione con strumenti di registrazione e monitoraggio della piattaforma.
{: shortdesc}

Con un solo clic, puoi attivare tutti i componenti core di Istio, ulteriori funzioni di traccia, monitoraggio e visualizzazione e l'applicazione di esempio BookInfo. Istio su {{site.data.keyword.containerlong_notm}} viene offerto come componente aggiuntivo gestito, pertanto {{site.data.keyword.Bluemix_notm}} mantiene automaticamente aggiornati tutti i tuoi componenti Istio.

## Descrizione di Istio su {{site.data.keyword.containerlong_notm}}
{: #istio_ov}

### Cos'è Istio?
{: #istio_ov_what_is}

[Istio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/info/istio) è una piattaforma per la rete di servizi aperta che consente di connettere, proteggere, controllare e osservare i microservizi su piattaforme cloud come Kubernetes in {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Quando sposti le applicazioni monolitiche in un'architettura di microservizi distribuita, emergono una serie di nuove sfide tra cui controllare il traffico ai tuoi microservizi, eseguire rilasci graduali (dark launch) e distribuzioni canary dei tuoi servizi, gestire errori, proteggere le comunicazioni del servizio, osservare i servizi e applicare politiche di accesso coerenti in tutta la flotta di servizi. Per affrontare queste difficoltà, puoi sfruttare una rete di servizi. Una rete di servizi fornisce una rete trasparente e indipendente dal linguaggio per la connessione, l'osservazione, la protezione e il controllo della connettività tra i microservizi. Istio fornisce informazioni approfondite e controllo sulla rete di servizi per consentirti di gestire il traffico di rete, bilanciare il carico tra i microservizi, applicare politiche di accesso, verificare l'identità del servizio e molto altro.

Ad esempio, l'utilizzo di Istio nella tua rete di microservizi può aiutarti a:
- Ottenere una migliore visibilità delle applicazioni in esecuzione nel tuo cluster
- Distribuire versioni canary delle applicazioni e controllare il traffico che viene loro inviato
- Abilitare la crittografia automatica dei dati trasferiti tra i microservizi
- Applicare limiti di velocità e politiche di whitelist e blacklist basate su attributi

Una rete di servizi Istio è composta da un piano dati e un piano di controllo. Il piano dati è composto da sidecar del proxy Envoy in ogni pod dell'applicazione, che mediano le comunicazioni tra i microservizi. Il piano di controllo è costituito da Pilot, telemetria e politiche Mixer e Citadel, che applicano le configurazioni Istio nel tuo cluster. Per ulteriori informazioni su ciascuno di questi componenti, vedi la [descrizione del componente aggiuntivo `istio`](#istio_components).

### Cos'è Istio su {{site.data.keyword.containerlong_notm}} (beta)?
{: #istio_ov_addon}

Istio su {{site.data.keyword.containerlong_notm}} è offerto come componente aggiuntivo gestito che integra Istio direttamente con il tuo cluster Kubernetes.
{: shortdesc}

Il componente aggiuntivo gestito Istio è classificato come beta e potrebbe essere instabile o cambiare frequentemente. Inoltre, le funzioni beta potrebbero non fornire lo stesso livello di prestazioni o di compatibilità fornito dalle funzioni generalmente disponibili e non sono progettate per essere utilizzate in un ambiente di produzione.
{: note}

**Che forma assume nel mio cluster?**</br>
Quando installi il componente aggiuntivo Istio, i piani di controllo e dati di Istio utilizzano le VLAN a cui il tuo cluster è già connesso. Il traffico di configurazione transita sulla rete privata all'interno del tuo cluster e non richiede l'apertura di ulteriori porte o indirizzi IP nel tuo firewall. Se esponi le tue applicazioni gestite da Istio con un gateway Istio, le richieste di traffico esterno alle applicazioni transitano sulla VLAN pubblica.

**Come funziona il processo di aggiornamento?**</br>
La versione di Istio nel componente aggiuntivo gestito viene testata da {{site.data.keyword.Bluemix_notm}} e approvata per l'utilizzo in {{site.data.keyword.containerlong_notm}}. Per aggiornare i tuoi componenti Istio alla versione più recente di Istio supportata da {{site.data.keyword.containerlong_notm}}, puoi seguire la procedura indicata in [Aggiornamento di componenti aggiuntivi gestiti](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons).  

Se hai bisogno di utilizzare la versione più recente o di personalizzare la tua installazione di Istio, puoi installare la versione open source di Istio attenendoti alla procedura contenuta nell'[esercitazione introduttiva a {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/setup/kubernetes/quick-start-ibm/).
{: tip}

**Ci sono delle limitazioni?** </br>
Non puoi abilitare il componente aggiuntivo Istio gestito nel tuo cluster se hai installato il [controller di ammissione Container image security enforcer](/docs/services/Registry?topic=registry-security_enforce#security_enforce) nel tuo cluster.

<br />


## Cosa posso installare?
{: #istio_components}

Istio su {{site.data.keyword.containerlong_notm}} è offerto come tre componenti aggiuntivi gestiti nel tuo cluster.
{: shortdesc}

<dl>
<dt>Istio (`istio`)</dt>
<dd>Installa i componenti core di Istio, incluso Prometheus. Per ulteriori informazioni su uno qualsiasi dei seguenti componenti del piano di controllo, consulta la [documentazione di Istio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/concepts/what-is-istio/).
  <ul><li>`Envoy` trasmette tramite proxy il traffico in entrata e in uscita per tutti i servizi nella rete. Envoy viene distribuito come contenitore di sidecar nello stesso pod del tuo contenitore dell'applicazione.</li>
  <li>`Mixer` fornisce controlli di raccolta di telemetria e politiche.<ul>
    <li>I pod di telemetria sono abilitati con un endpoint Prometheus, che aggrega tutti i dati di telemetria dai sidecar del proxy Envoy e dai servizi nei tuoi pod dell'applicazione.</li>
    <li>I pod della politica applicano il controllo dell'accesso, incluso il limite di velocità e l'applicazione di politiche di whitelist e blacklist.</li></ul>
  </li>
  <li>`Pilot` fornisce il rilevamento di servizi per i sidecar Envoy e configura le regole di instradamento per la gestione del traffico per i sidecar.</li>
  <li>`Citadel` utilizza la gestione di identità e credenziali per fornire l'autenticazione da servizio a servizio e dell'utente finale.</li>
  <li>`Galley` convalida le modifiche di configurazione per gli altri componenti del piano di controllo Istio.</li>
</ul></dd>
<dt>Supplementi Istio (`istio-extras`)</dt>
<dd>Facoltativo: installa [Grafana ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://grafana.com/), [Jaeger ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.jaegertracing.io/) e [Kiali ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.kiali.io/) per fornire ulteriori funzioni di monitoraggio, traccia e visualizzazione per Istio.</dd>
<dt>Applicazione di esempio BookInfo (`istio-sample-bookinfo`)</dt>
<dd>Facoltativo: distribuisce l'[applicazione di esempio BookInfo per Istio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/examples/bookinfo/). Questa distribuzione include la configurazione della demo di base e le regole di destinazione predefinite che ti permettono di provare immediatamente le funzionalità di Istio.</dd>
</dl>

<br>
Puoi sempre vedere quali componenti aggiuntivi Istio sono abilitati nel tuo cluster immettendo il seguente comando:
```
ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
```
{: pre}

<br />


## Installazione di Istio su {{site.data.keyword.containerlong_notm}}
{: #istio_install}

Installa i componenti aggiuntivi gestiti da Istio in un cluster esistente.
{: shortdesc}

**Prima di iniziare**</br>
* Assicurati di disporre del [ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Scrittore** o **Gestore**](/docs/containers?topic=containers-users#platform) per {{site.data.keyword.containerlong_notm}}.
* [Crea o utilizza un cluster standard esistente con almeno 3 nodi di lavoro, ciascuno dei quali con 4 core e 16 GB di memoria (`b3c.4x16`) o più](/docs/containers?topic=containers-clusters#clusters_ui). Inoltre, il cluster e i nodi di lavoro devono eseguire almeno la versione minima supportata di Kubernetes, che puoi esaminare eseguendo `ibmcloud ks addon-versions --addon istio`.
* [Indirizza la CLI al tuo cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
* Se utilizzi un cluster esistente e in precedenza hai installato Istio nel cluster utilizzando il grafico Helm IBM o un altro metodo, [ripulisci tale installazione di Istio](#istio_uninstall_other).

### Installazione dei componenti aggiuntivi Istio gestiti nella CLI
{: #istio_install_cli}

1. Abilita il componente aggiuntivo `istio`.
  ```
  ibmcloud ks cluster-addon-enable istio --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. Facoltativo: abilita il componente aggiuntivo `istio-extras`.
  ```
  ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID>
  ```
  {: pre}

3. Facoltativo: abilita il componente aggiuntivo `istio-sample-bookinfo`.
  ```
  ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID>
  ```
  {: pre}

4. Verifica che i componenti aggiuntivi Istio gestiti che hai installato siano abilitati in questo cluster.
  ```
  ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Output di esempio:
  ```
  Name                      Version
  istio                     1.1.5
  istio-extras              1.1.5
  istio-sample-bookinfo     1.1.5
  ```
  {: screen}

5. Puoi anche controllare i singoli componenti di ciascun componente aggiuntivo nel tuo cluster.
  - Componenti di `istio` e `istio-extras`: assicurati che i servizi Istio e i pod corrispondenti siano stati distribuiti.
    ```
    kubectl get svc -n istio-system
    ```
    {: pre}

    ```
    NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
    grafana                  ClusterIP      172.21.98.154    <none>          3000/TCP                                                       2m
    istio-citadel            ClusterIP      172.21.221.65    <none>          8060/TCP,9093/TCP                                              2m
    istio-egressgateway      ClusterIP      172.21.46.253    <none>          80/TCP,443/TCP                                                 2m
    istio-galley             ClusterIP      172.21.125.77    <none>          443/TCP,9093/TCP                                               2m
    istio-ingressgateway     LoadBalancer   172.21.230.230   169.46.56.125   80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                              8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP   2m
    istio-pilot              ClusterIP      172.21.171.29    <none>          15010/TCP,15011/TCP,8080/TCP,9093/TCP                          2m
    istio-policy             ClusterIP      172.21.140.180   <none>          9091/TCP,15004/TCP,9093/TCP                                    2m
    istio-sidecar-injector   ClusterIP      172.21.248.36    <none>          443/TCP                                                        2m
    istio-telemetry          ClusterIP      172.21.204.173   <none>          9091/TCP,15004/TCP,9093/TCP,42422/TCP                          2m
    jaeger-agent             ClusterIP      None             <none>          5775/UDP,6831/UDP,6832/UDP                                     2m
    jaeger-collector         ClusterIP      172.21.65.195    <none>          14267/TCP,14268/TCP                                            2m
    jaeger-query             ClusterIP      172.21.171.199   <none>          16686/TCP                                                      2m
    kiali                    ClusterIP      172.21.13.35     <none>          20001/TCP                                                      2m
    prometheus               ClusterIP      172.21.105.229   <none>          9090/TCP                                                       2m
    tracing                  ClusterIP      172.21.125.177   <none>          80/TCP                                                         2m
    zipkin                   ClusterIP      172.21.1.77      <none>          9411/TCP                                                       2m
    ```
    {: screen}

    ```
    kubectl get pods -n istio-system
    ```
    {: pre}

    ```
    NAME                                      READY   STATUS    RESTARTS   AGE
    grafana-76dcdfc987-94ldq                  1/1     Running   0          2m
    istio-citadel-869c7f9498-wtldz            1/1     Running   0          2m
    istio-egressgateway-69bb5d4585-qxxbp      1/1     Running   0          2m
    istio-galley-75d7b5bdb9-c9d9n             1/1     Running   0          2m
    istio-ingressgateway-5c8764db74-gh8xg     1/1     Running   0          2m
    istio-pilot-55fd7d886f-vv6fb              2/2     Running   0          2m
    istio-policy-6bb6f6ddb9-s4c8t             2/2     Running   0          2m
    istio-sidecar-injector-7d9845dbb7-r8nq5   1/1     Running   0          2m
    istio-telemetry-7695b4c4d4-tlvn8          2/2     Running   0          2m
    istio-tracing-55bbf55878-z4rd2            1/1     Running   0          2m
    kiali-77566cc66c-kh6lm                    1/1     Running   0          2m
    prometheus-5d5cb44877-lwrqx               1/1     Running   0          2m
    ```
    {: screen}

  - Componenti di `istio-sample-bookinfo`: assicurati che i microservizi BookInfo e i pod corrispondenti siano stati distribuiti.
    ```
    kubectl get svc -n default
    ```
    {: pre}

    ```
    NAME                      TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)          AGE
    details                   ClusterIP      172.21.19.104    <none>         9080/TCP         2m
    productpage               ClusterIP      172.21.168.196   <none>         9080/TCP         2m
    ratings                   ClusterIP      172.21.11.131    <none>         9080/TCP         2m
    reviews                   ClusterIP      172.21.117.164   <none>         9080/TCP         2m
    ```
    {: screen}

    ```
    kubectl get pods -n default
    ```
    {: pre}

    ```
    NAME                                     READY     STATUS      RESTARTS   AGE
    details-v1-6865b9b99d-7v9h8              2/2       Running     0          2m
    productpage-v1-f8c8fb8-tbsz9             2/2       Running     0          2m
    ratings-v1-77f657f55d-png6j              2/2       Running     0          2m
    reviews-v1-6b7f6db5c5-fdmbq              2/2       Running     0          2m
    reviews-v2-7ff5966b99-zflkv              2/2       Running     0          2m
    reviews-v3-5df889bcff-nlmjp              2/2       Running     0          2m
    ```
    {: screen}

### Installazione dei componenti aggiuntivi Istio gestiti nell'IU
{: #istio_install_ui}

1. Nel tuo [dashboard del cluster ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/kubernetes/clusters), fai clic sul nome di un cluster.

2. Fai clic sulla scheda **Add-ons**.

3. Sulla scheda Istio, fai clic su **Install**.

4. La casella di spunta **Istio** è già selezionata. Per installare anche i supplementi Istio e l'applicazione di esempio BookInfo, seleziona le caselle di spunta **Istio Extras** e **Istio Sample**.

5. Fai clic su **Install**.

6. Sulla scheda Istio, verifica che siano elencati i componenti aggiuntivi che hai abilitato.

In seguito, puoi provare le funzionalità di Istio controllando l'[applicazione di esempio BookInfo](#istio_bookinfo).

<br />


## Prova dell'applicazione di esempio BookInfo
{: #istio_bookinfo}

Il componente aggiuntivo BookInfo (`istio-sample-bookinfo`) distribuisce l'[applicazione di esempio BookInfo per Istio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/examples/bookinfo/) nello spazio dei nomi `default`. Questa distribuzione include la configurazione della demo di base e le regole di destinazione predefinite che ti permettono di provare immediatamente le funzionalità di Istio.
{: shortdesc}

I quattro microservizi BookInfo includono:
* `productpage` richiama i microservizi `details` e `reviews` per popolare la pagina.
* `details` contiene informazioni sul libro.
* `reviews` contiene le recensioni del libro e richiama il microservizio `ratings`.
* `ratings` contiene informazioni sulla classificazione del libro che accompagnano una recensione.

Il microservizio `reviews` ha più versioni:
* `v1` non richiama il microservizio `ratings`.
* `v2` richiama il microservizio `ratings` e visualizza le valutazioni come da 1 a 5 stelle nere.
* `v3` richiama il microservizio `ratings` e visualizza le valutazioni come da 1 a 5 stelle rosse.

Gli YAML di distribuzione per ciascuno di questi microservizi vengono modificati in modo tale che i proxy sidecar Envoy siano pre-inseriti come contenitori nei pod dei microservizi prima che vengano distribuiti. Per ulteriori informazioni sull'inserimento manuale di sidecar, vedi la [documentazione di Istio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/setup/kubernetes/sidecar-injection/). L'applicazione BookInfo è inoltre già esposta su un indirizzo di ingresso IP pubblico mediante un gateway Istio. Sebbene l'applicazione BookInfo possa aiutarti a iniziare, non è pensata per l'utilizzo in produzione.

Prima di iniziare, [installa i componenti aggiuntivi gestiti `istio`, `istio-extras` e `istio-sample-bookinfo`](#istio_install) in un cluster.

1. Ottieni l'indirizzo pubblico per il tuo cluster.
  1. Imposta l'host ingress.
    ```
    export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
    ```
    {: pre}

  2. Imposta la porta ingress.
    ```
    export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
    ```
    {: pre}

  3. Crea una variabile di ambiente `GATEWAY_URL` che utilizza l'host e la porta ingress.
     ```
     export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
     ```
     {: pre}

2. Esegui il curl della variabile `GATEWAY_URL` per controllare che l'applicazione BookInfo sia in esecuzione. Una risposta `200` significa che l'applicazione BookInfo è in esecuzione correttamente con Istio.
   ```
   curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
   ```
   {: pre}

3.  Visualizza la pagina web di BookInfo in un browser.

    Mac OS o Linux:
    ```
    open http://$GATEWAY_URL/productpage
    ```
    {: pre}

    Windows:
    ```
    start http://$GATEWAY_URL/productpage
    ```
    {: pre}

4. Prova ad aggiornare la pagina diverse volte. Differenti versioni della sezione delle recensioni ruotano e visualizzano le stelle rosse, nere e nessuna stella.

### Informazioni su cosa è accaduto
{: #istio_bookinfo_understanding}

L'esempio di BookInfo mostra come tre componenti di gestione del traffico di Istio interagiscono per instradare il traffico in entrata verso l'applicazione.
{: shortdesc}

<dl>
<dt>`Gateway`</dt>
<dd>Il [Gateway ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/) `bookinfo-gateway` descrive un programma di bilanciamento del carico, il servizio `istio-ingressgateway` nello spazio dei nomi `istio-system`, che funge da punto di ingresso per il traffico HTTP/TCP per BookInfo. Istio configura il programma di bilanciamento del carico per l'ascolto delle richieste in entrata alle applicazioni gestite da Istio sulle porte definite nel file di configurazione del gateway.
</br></br>Per visualizzare il file di configurazione per il gateway BookInfo, immetti il seguente comando.
<pre class="pre"><code>kubectl get gateway bookinfo-gateway -o yaml</code></pre></dd>

<dt>`VirtualService`</dt>
<dd>Il [`VirtualService` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/) `bookinfo` definisce le regole che controllano il modo in cui vengono instradate le richieste all'interno della rete di servizi definendo i microservizi come destinazioni (`destinations`). Nel servizio virtuale `bookinfo`, l'URI `/productpage` di una richiesta viene instradato all'host `productpage` sulla porta `9080`. In questo modo, tutte le richieste all'applicazione BookInfo vengono instradate prima al microservizio `productpage`, che quindi richiama gli altri microservizi di BookInfo.
</br></br>Per visualizzare la regola del servizio virtuale che viene applicata a BookInfo, immetti il seguente comando.
<pre class="pre"><code>kubectl get virtualservice bookinfo -o yaml</code></pre></dd>

<dt>`DestinationRule`</dt>
<dd>Dopo che il gateway instrada la richiesta in base alla regola del servizio virtuale, le [`DestinationRules` `details`, `productpage`, `ratings` e `reviews` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/) definiscono le politiche che vengono applicate alla richiesta quando arriva a un microservizio. Ad esempio, quando aggiorni la pagina del prodotto BookInfo, le modifiche che vedi sono il risultato del microservizio `productpage` che richiama in modo casuale le diverse versioni, `v1`, `v2` e `v3`, del microservizio `reviews`. Le versioni sono selezionate in modo casuale poiché la regola di destinazione `reviews` attribuisce lo stesso peso ai `subsets`, o alle versioni denominate, del microservizio. Questi sottoinsiemi vengono utilizzati dalle regole del servizio virtuale quando il traffico viene indirizzato a specifiche versioni del servizio.
</br></br>Per visualizzare le regole di destinazione applicate a BookInfo, esegui il seguente comando.
<pre class="pre"><code>kubectl describe destinationrules</code></pre></dd>
</dl>

</br>

Successivamente, puoi [esporre BookInfo utilizzando il dominio secondario Ingress fornito da IBM](#istio_expose_bookinfo) o [registrare, monitorare, tracciare e visualizzare](#istio_health) la rete di servizi per l'applicazione BookInfo.

<br />


## Registrazione, monitoraggio, traccia e visualizzazione di Istio
{: #istio_health}

Per registrare, monitorare, tracciare e visualizzare le tue applicazioni gestite da Istio su {{site.data.keyword.containerlong_notm}}, puoi avviare i dashboard Grafana, Jaeger e Kiali installati nel componente aggiuntivo `istio-extras` o puoi distribuire LogDNA e Sysdig come servizi di terze parti sui tuoi nodi di lavoro.
{: shortdesc}

### Avvio dei dashboard Grafana, Jaeger e Kiali
{: #istio_health_extras}

Il componente aggiuntivo dei supplementi Istio (`istio-extras`) installa [Grafana ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://grafana.com/), [Jaeger ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.jaegertracing.io/) e [Kiali ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.kiali.io/). Avvia i dashboard per ciascuno di questi servizi per fornire ulteriori funzioni di monitoraggio, traccia e visualizzazione per Istio.
{: shortdesc}

Prima di iniziare, [installa i componenti aggiuntivi gestiti `istio` e `istio-extras`](#istio_install) in un cluster.

**Grafana**</br>
1. Avvia l'inoltro della porta Kubernetes per il dashboard Grafana.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
  ```
  {: pre}

2. Per aprire il dashboard Grafana di Istio, vai al seguente URL: http://localhost:3000/dashboard/db/istio-mesh-dashboard. Se hai installato il [componente aggiuntivo BookInfo](#istio_bookinfo), il dashboard di Istio mostra le metriche per il traffico che hai generato quando hai aggiornato alcune volte la pagina del prodotto. Per ulteriori informazioni sull'utilizzo del dashboard Grafana di Istio, vedi [Viewing the Istio Dashboard ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/tasks/telemetry/using-istio-dashboard/) nella documentazione open source di Istio.

**Jaeger**</br>
1. Per impostazione predefinita, Istio genera estensioni di traccia per 1 richiesta su 100, con una frequenza di campionamento pari all'1%. Devi inviare almeno 100 richieste prima che la prima traccia sia visibile. Per inviare 100 richieste al servizio `productpage` del [componente aggiuntivo BookInfo](#istio_bookinfo), esegui il seguente comando.
  ```
  for i in `seq 1 100`; do curl -s -o /dev/null http://$GATEWAY_URL/productpage; done
  ```
  {: pre}

2. Avvia l'inoltro della porta Kubernetes per il dashboard Jaeger.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686 &
  ```
  {: pre}

3. Per aprire l'IU Jaeger, vai al seguente URL: http://localhost:16686.

4. Se hai installato il componente aggiuntivo BookInfo, puoi selezionare `productpage` dall'elenco **Service** e fare clic su **Find Traces**. Vengono mostrate le tracce per il traffico che hai generato quando hai aggiornato alcune volte la pagina del prodotto. Per ulteriori informazioni sull'utilizzo di Jaeger con Istio, vedi [Generating traces using the BookInfo sample ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/tasks/telemetry/distributed-tracing/#generating-traces-using-the-bookinfo-sample) nella documentazione open source di Istio.

**Kiali**</br>
1. Avvia l'inoltro della porta Kubernetes per il dashboard Kiali.
  ```
  kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001 &
  ```
  {: pre}

2. Per aprire l'IU Kiali, vai al seguente URL: http://localhost:20001/kiali/console.

3. Immetti `admin` sia per il nome utente che per la passphrase. Per ulteriori informazioni sull'utilizzo di Kiali per visualizzare i tuoi microservizi gestiti da Istio, vedi [Generating a service graph ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://archive.istio.io/v1.0/docs/tasks/telemetry/kiali/#generating-a-service-graph) nella documentazione open source di Istio.

### Configurazione della registrazione con {{site.data.keyword.la_full_notm}}
{: #istio_health_logdna}

Gestisci facilmente i log per il tuo contenitore dell'applicazione e il contenitore di sidecar del proxy Envoy in ciascun pod distribuendo LogDNA ai tuoi nodi di lavoro per inoltrare i log a {{site.data.keyword.loganalysislong}}.
{: shortdesc}

Per utilizzare [{{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about), distribuisci un agent di registrazione a ogni nodo di lavoro presente nel tuo cluster. Questo agent raccoglie i log con l'estensione `*.log` e i file senza estensione memorizzati nella directory `/var/log` del tuo pod da tutti gli spazi dei nomi, incluso `kube-system`. Questi log includono i log provenienti dal tuo contenitore dell'applicazione e dal contenitore di sidecar del proxy Envoy in ciascun pod. L'agent inoltra quindi i log al servizio {{site.data.keyword.la_full_notm}}.

Per iniziare, configura LogDNA per il tuo cluster attenendoti alla procedura descritta in [Gestione dei log di cluster Kubernetes con {{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).




### Configurazione del monitoraggio con {{site.data.keyword.mon_full_notm}}
{: #istio_health_sysdig}

Ottieni visibilità operativa sulle prestazioni e sull'integrità delle tue applicazioni gestite da Istio distribuendo Sysdig ai tuoi nodi di lavoro per inoltrare le metriche a {{site.data.keyword.monitoringlong}}.
{: shortdesc}

Con Istio su {{site.data.keyword.containerlong_notm}}, il componente aggiuntivo `istio` gestito installa Prometheus nel tuo cluster. I pod `istio-mixer-telemetry` presenti nel tuo cluster sono annotati con un endpoint Prometheus in modo che Prometheus possa aggregare tutti i dati di telemetria per i tuoi pod. Quando distribuisci un agent Sysdig a ogni nodo di lavoro nel tuo cluster, Sysdig è già abilitato automaticamente a rilevare e recuperare i dati da questi endpoint Prometheus per visualizzarli nel tuo dashboard di monitoraggio {{site.data.keyword.Bluemix_notm}}.

Dal momento che tutto il lavoro di Prometheus è terminato, tutto ciò che ti resta da fare è distribuire Sysdig nel tuo cluster.

1. Configura Sysdig seguendo la procedura in [Analisi delle metriche per un'applicazione distribuita in un cluster Kubernetes](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster).

2. [Avvia l'IU Sysdig ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_step3).

3. Fai clic su **Add new dashboard**.

4. Cerca `Istio` e seleziona uno dei dashboard Istio predefiniti di Sysdig.

Per ulteriori informazioni sul riferimento a metriche e dashboard, monitoraggio dei componenti interni di Istio e monitoraggio delle distribuzioni A/B e delle distribuzioni canary di Istio, consulta [How to monitor Istio, the Kubernetes service mesh ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://sysdig.com/blog/monitor-istio/). Cerca la sezione denominata "Monitoring Istio: reference metrics and dashboards" del post del blog.

<br />


## Configurazione dell'inserimento di sidecar per le tue applicazioni
{: #istio_sidecar}

Sei pronto a gestire le tue applicazioni utilizzando Istio? Prima di distribuire la tua applicazione, devi prima decidere come inserire i sidecar del proxy Envoy nei pod dell'applicazione.
{: shortdesc}

Ogni pod dell'applicazione deve eseguire un sidecar del proxy Envoy in modo che i microservizi possano essere inclusi nella rete di servizi. Puoi assicurarti che i sidecar vengano inseriti automaticamente o manualmente in ogni pod dell'applicazione. Per ulteriori informazioni sull'inserimento di sidecar, vedi la [documentazione di Istio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/setup/kubernetes/sidecar-injection/).

### Abilitazione dell'inserimento automatico di sidecar
{: #istio_sidecar_automatic}

Quando l'inserimento automatico di sidecar è abilitato, uno spazio dei nomi ascolta eventuali nuove distribuzioni e modifica automaticamente la specifica del template di pod, cosicché i pod dell'applicazione vengano creati con contenitori di sidecar del proxy Envoy. Abilita l'inserimento automatico di sidecar per uno spazio dei nomi quando pianifichi di distribuire più applicazioni che vuoi integrare con Istio in tale spazio dei nomi. L'inserimento automatico di sidecar non è abilitato per tutti gli spazi dei nomi per impostazione predefinita nel componente aggiuntivo gestito da Istio.

Per abilitare l'inserimento automatico di sidecar per uno spazio dei nomi:

1. Ottieni il nome dello spazio dei nomi in cui vuoi distribuire le applicazioni gestite da Istio.
  ```
  kubectl get namespaces
  ```
  {: pre}

2. Etichetta lo spazio dei nomi come `istio-injection=enabled`.
  ```
  kubectl label namespace <namespace> istio-injection=enabled
  ```
  {: pre}

3. Distribuisci le applicazioni nello spazio dei nomi etichettato o ridistribuisci le applicazioni che si trovano già nello spazio dei nomi.
  * Per distribuire un'applicazione nello spazio dei nomi etichettato:
    ```
    kubectl apply <myapp>.yaml --namespace <namespace>
    ```
    {: pre}
  * Per ridistribuire un'applicazione già distribuita in tale spazio dei nomi, elimina il pod dell'applicazione in modo che venga ridistribuito con il sidecar inserito.
    ```
    kubectl delete pod -l app=<myapp>
    ```
    {: pre}

5. Se non hai creato un servizio per esporre la tua applicazione, crea un servizio Kubernetes. La tua applicazione deve essere esposta da un servizio Kubernetes per poter essere inclusa come microservizio nella rete di servizi Istio. Assicurati di attenerti ai [requisiti di Istio per pod e servizi ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/setup/kubernetes/spec-requirements/).

  1. Definisci un servizio per l'applicazione.
    ```
    apiVersion: v1
          kind: Service
          metadata:
            name: myappservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
       - protocol: TCP
               port: 8080
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YALM del servizio</th>
    </thead>
    <tbody>
    <tr>
    <td><code>selector</code></td>
    <td>Immetti la coppia di chiave (<em>&lt;selector_key&gt;</em>) e valore (<em>&lt;selector_value&gt;</em>) dell'etichetta che vuoi utilizzare per indirizzare i pod in cui viene eseguita la tua applicazione.</td>
     </tr>
     <tr>
     <td><code> port</code></td>
     <td>La porta su cui è in ascolto il servizio.</td>
     </tr>
     </tbody></table>

  2. Crea il servizio nel tuo cluster. Assicurati che il servizio venga distribuito nello stesso spazio dei nomi dell'applicazione.
    ```
    kubectl apply -f myappservice.yaml -n <namespace>
    ```
    {: pre}

I pod dell'applicazione sono ora integrati nella tua rete di servizi Istio perché hanno il contenitore di sidecar Istio che viene eseguito insieme al tuo contenitore dell'applicazione.

### Inserimento manuale di sidecar
{: #istio_sidecar_manual}

Se non vuoi abilitare l'inserimento automatico di sidecar su uno spazio dei nomi, puoi inserire manualmente il sidecar in un YAML di distribuzione. Inserisci i sidecar manualmente quando le applicazioni sono in esecuzione in spazi dei nomi insieme ad altre distribuzioni in cui non vuoi che i sidecar vengano inseriti automaticamente.

Per inserire manualmente i sidecar in una distribuzione:

1. Scarica il client `istioctl`.
  ```
  curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.1.5 sh -
  ```

2. Passa alla directory del pacchetto Istio.
  ```
  cd istio-1.1.5
  ```
  {: pre}

3. Inserisci il sidecar nel file YAML di distribuzione della tua applicazione.
  ```
  istioctl kube-inject -f <myapp>.yaml | kubectl apply -f -
  ```
  {: pre}

4. Distribuisci la tua applicazione.
  ```
  kubectl apply <myapp>.yaml
  ```
  {: pre}

5. Se non hai creato un servizio per esporre la tua applicazione, crea un servizio Kubernetes. La tua applicazione deve essere esposta da un servizio Kubernetes per poter essere inclusa come microservizio nella rete di servizi Istio. Assicurati di attenerti ai [requisiti di Istio per pod e servizi ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/setup/kubernetes/spec-requirements/).

  1. Definisci un servizio per l'applicazione.
    ```
    apiVersion: v1
          kind: Service
          metadata:
            name: myappservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
       - protocol: TCP
               port: 8080
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YALM del servizio</th>
    </thead>
    <tbody>
    <tr>
    <td><code>selector</code></td>
    <td>Immetti la coppia di chiave (<em>&lt;selector_key&gt;</em>) e valore (<em>&lt;selector_value&gt;</em>) dell'etichetta che vuoi utilizzare per indirizzare i pod in cui viene eseguita la tua applicazione.</td>
     </tr>
     <tr>
     <td><code> port</code></td>
     <td>La porta su cui è in ascolto il servizio.</td>
     </tr>
     </tbody></table>

  2. Crea il servizio nel tuo cluster. Assicurati che il servizio venga distribuito nello stesso spazio dei nomi dell'applicazione.
    ```
    kubectl apply -f myappservice.yaml -n <namespace>
    ```
    {: pre}

I pod dell'applicazione sono ora integrati nella tua rete di servizi Istio perché hanno il contenitore di sidecar Istio che viene eseguito insieme al tuo contenitore dell'applicazione.

<br />


## Esposizione delle applicazioni gestite da Istio attraverso un nome host fornito da IBM
{: #istio_expose}

Dopo aver [configurato l'inserimento di sidecar del proxy Envoy](#istio_sidecar) e distribuito le tue applicazioni nella rete di servizi Istio, puoi esporre le tue applicazioni gestite da Istio alle richieste pubbliche utilizzando un nome host fornito da IBM.
{: shortdesc}

Istio utilizza i [Gateway ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/) e i [Servizi virtuali ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/) per controllare il modo in cui il traffico viene instradato alle tue applicazioni. Un gateway configura un programma di bilanciamento del carico, `istio-ingressgateway`, che funge da punto di ingresso per le tue applicazioni gestite da Istio. Puoi esporre le tue applicazioni gestite da Istio registrando l'indirizzo IP esterno del programma di bilanciamento del carico `istio-ingressgateway` con una voce DNS e un nome host.

Puoi provare prima l'[esempio per esporre BookInfo](#istio_expose_bookinfo)o [esporre pubblicamente le tue applicazioni gestite da Istio](#istio_expose_link).

### Esempio: esposizione di BookInfo attraverso un nome host fornito da IBM
{: #istio_expose_bookinfo}

Quando abiliti il componente aggiuntivo BookInfo nel tuo cluster, viene creato automaticamente il gateway Istio `bookinfo-gateway`. Il gateway utilizza le regole del servizio virtuale e di destinazione di Istio per configurare un programma di bilanciamento del carico, `istio-ingressgateway`, che espone pubblicamente l'applicazione BookInfo. Nella seguente procedura, crei un nome host per l'indirizzo IP del programma di bilanciamento del carico `istio-ingressgateway` attraverso il quale puoi accedere pubblicamente a BookInfo.
{: shortdesc}

Prima di iniziare, [abilita il componente aggiuntivo gestito `istio-sample-bookinfo` ](#istio_install) in un cluster.

1. Ottieni l'indirizzo **EXTERNAL-IP** per il programma di bilanciamento del carico `istio-ingressgateway`.
  ```
  kubectl get svc -n istio-system
  ```
  {: pre}

  Nel seguente output di esempio, l'**EXTERNAL-IP** è `168.1.1.1`.
  ```
  NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
  ...
  istio-ingressgateway     LoadBalancer   172.21.XXX.XXX   169.1.1.1       80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                            8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP  22m
  ```
  {: screen}

2. Registra l'IP creando un nome host DNS.
  ```
  ibmcloud ks nlb-dns-create --cluster <cluster_name_or_id> --ip <LB_IP>
  ```
  {: pre}

3. Verifica che il nome host sia stato creato.
  ```
  ibmcloud ks nlb-dnss --cluster <cluster_name_or_id>
  ```
  {: pre}

  Output di esempio:
  ```
  Hostname                                                                                IP(s)              Health Monitor   SSL Cert Status           SSL Cert Secret Name
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     ["168.1.1.1"]      None             created                   <certificate>
  ```
  {: screen}

4. In un browser web, apri la pagina del prodotto BookInfo.
  ```
  https://<host_name>/productpage
  ```
  {: codeblock}

5. Prova ad aggiornare la pagina diverse volte. Le richieste a `http://<host_name>/productpage` vengono ricevute dall'ALB e inoltrate al programma di bilanciamento del carico del gateway Istio. Le diverse versioni del microservizio `reviews` vengono ancora restituite in modo casuale perché il gateway Istio gestisce le regole di instradamento di servizio virtuale e destinazione per i microservizi.

Per ulteriori informazioni su gateway, regole del servizio virtuale e regole di destinazione per l'applicazione BookInfo, vedi [Informazioni su cosa è accaduto](#istio_bookinfo_understanding). Per ulteriori informazioni sulla registrazione dei nomi host DNS in {{site.data.keyword.containerlong_notm}}, vedi [Registrazione di un nome host NLB](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname).

### Esposizione pubblica delle tue applicazioni gestite da Istio attraverso un nome host fornito da IBM
{: #istio_expose_link}

Esponi pubblicamente le tue applicazioni gestite da Istio, creando un gateway Istio, un servizio virtuale che definisce le regole di gestione del traffico per i tuoi servizi gestiti da Istio, e un nome host DNS per l'indirizzo IP esterno del programma di bilanciamento del carico `istio-ingressgateway`.
{: shortdesc}

**Prima di iniziare:**
1. [Installa il componente aggiuntivo gestito `istio`](#istio_install) in un cluster.
2. Installa il client `istioctl`.
  1. Scarica `istioctl`.
    ```
    curl -L https://git.io/getLatestIstio | sh -
    ```
  2. Passa alla directory del pacchetto Istio.
    ```
    cd istio-1.1.5
    ```
    {: pre}
3. [Configura l'inserimento di sidecar per i tuoi microservizi dell'applicazione, distribuisci i microservizi dell'applicazione in uno spazio dei nomi e crea servizi Kubernetes per i microservizi dell'applicazione in modo che possano essere inclusi nella rete di servizi Istio](#istio_sidecar).

</br>
**Per esporre pubblicamente le tue applicazioni gestite da Istio con un nome host:**

1. Crea un gateway. Questo gateway di esempio utilizza il servizio del programma di bilanciamento del carico `istio-ingressgateway` per esporre la porta 80 per HTTP. Sostituisci `<namespace>` con lo spazio dei nomi in cui vengono distribuiti i tuoi microservizi gestiti da Istio. Se i tuoi microservizi sono in ascolto su una porta diversa da `80`, aggiungi quella porta. Per ulteriori informazioni sui componenti YAML del gateway, vedi la [documentazione di riferimento di Istio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/).
  ```
  apiVersion: networking.istio.io/v1alpha3
  kind: Gateway
  metadata:
    name: my-gateway
    namespace: <namespace>
  spec:
    selector:
      istio: ingressgateway
    servers:
    - port:
        name: http
        number: 80
        protocol: HTTP
      hosts:
      - '*'
  ```
  {: codeblock}

2. Applica il gateway nello spazio dei nomi in cui vengono distribuiti i tuoi microservizi gestiti da Istio.
  ```
  kubectl apply -f my-gateway.yaml -n <namespace>
  ```
  {: pre}

3. Crea un servizio virtuale che utilizza il gateway `my-gateway` e definisce le regole di instradamento per i tuoi microservizi dell'applicazione. Per ulteriori informazioni sui componenti YAML del servizio virtuale, vedi la [documentazione di riferimento di Istio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/).
  ```
  apiVersion: networking.istio.io/v1alpha3
  kind: VirtualService
  metadata:
    name: my-virtual-service
    namespace: <namespace>
  spec:
    gateways:
    - my-gateway
    hosts:
    - '*'
    http:
    - match:
      - uri:
          exact: /<service_path>
      route:
      - destination:
          host: <service_name>
          port:
            number: 80
  ```
  {: codeblock}

  <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
  </thead>
  <tbody>
  <tr>
  <td><code>namespace</code></td>
  <td>Sostituisci <em>&lt;namespace&gt;</em> con lo spazio dei nomi in cui vengono distribuiti i tuoi microservizi gestiti da Istio.</td>
  </tr>
  <tr>
  <td><code>gateways</code></td>
  <td><code>my-gateway</code> viene specificato in modo che il gateway possa applicare queste regole di instradamento del servizio virtuale al programma di bilanciamento del carico <code>istio-ingressgateway</code>.<td>
  </tr>
  <tr>
  <td><code>http.match.uri.exact</code></td>
  <td>Sostituisci <em>&lt;service_path&gt;</em> con il percorso su cui è in ascolto il tuo microservizio di punto di ingresso. Ad esempio, nell'applicazione BookInfo, il percorso è definito come <code>/productpage</code>.</td>
  </tr>
  <tr>
  <td><code>http.route.destination.host</code></td>
  <td>Sostituisci <em>&lt;service_name&gt;</em> con il nome del tuo microservizio di punto di ingresso. Ad esempio, nell'applicazione BookInfo, <code>productpage</code> fungeva da microservizio di punto di ingresso che richiamava gli altri servizi dell'applicazione.</td>
  </tr>
  <tr>
  <td><code>http.route.destination.port.number</code></td>
  <td>Se il tuo microservizio è in ascolto su una porta diversa, sostituisci <em>&lt;80&gt;</em> con tale porta.</td>
  </tr>
  </tbody></table>

4. Applica le regole del servizio virtuale nello spazio dei nomi in cui viene distribuito il tuo microservizio gestito da Istio.
  ```
  kubectl apply -f my-virtual-service.yaml -n <namespace>
  ```
  {: pre}

5. Ottieni l'indirizzo **EXTERNAL-IP** per il programma di bilanciamento del carico `istio-ingressgateway`.
  ```
  kubectl get svc -n istio-system
  ```
  {: pre}

  Nel seguente output di esempio, l'**EXTERNAL-IP** è `168.1.1.1`.
  ```
  NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    AGE
  ...
  istio-ingressgateway     LoadBalancer   172.21.XXX.XXX   169.1.1.1       80:31380/TCP,443:31390/TCP,31400:31400/TCP,5011:31323/TCP,
                                                                            8060:32483/TCP,853:32628/TCP,15030:31601/TCP,15031:31915/TCP  22m
  ```
  {: screen}

6. Registra l'IP del programma di bilanciamento del carico `istio-ingressgateway` creando un nome host DNS.
  ```
  ibmcloud ks nlb-dns-create --cluster <cluster_name_or_id> --ip <LB_IP>
  ```
  {: pre}

7. Verifica che il nome host sia stato creato.
  ```
  ibmcloud ks nlb-dnss --cluster <cluster_name_or_id>
  ```
  {: pre}

  Output di esempio:
  ```
  Hostname                                                                                IP(s)              Health Monitor   SSL Cert Status           SSL Cert Secret Name
  mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud     ["168.1.1.1"]      None             created                   <certificate>
  ```
  {: screen}

7. In un browser web, verifica che il traffico venga instradato ai tuoi microservizi gestiti da Istio immettendo l'URL del microservizio dell'applicazione a cui accedere.
  ```
  http://<host_name>/<service_path>
  ```
  {: codeblock}

Nella revisione, hai creato un gateway denominato `my-gateway`. Questo gateway utilizza il servizio del programma di bilanciamento del carico `istio-ingressgateway` per esporre la tua applicazione. Il programma di bilanciamento del carico `istio-ingressgateway` utilizza le regole che hai definito nel servizio virtuale `my-virtual-service` per instradare il traffico alla tua applicazione. Infine, hai creato un nome host per il programma di bilanciamento del carico `istio-ingressgateway`. Tutte le richieste utente per il nome host vengono inoltrate alla tua applicazione secondo le tue regole di instradamento Istio. Per ulteriori informazioni sulla registrazione dei nomi host DNS in {{site.data.keyword.containerlong_notm}}, comprese le informazioni sulla configurazione di controlli di integrità personalizzati per i nomi host, vedi [Registrazione di un nome host NLB](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname).

Cerchi un controllo ancora più dettagliato sull'instradamento? Per creare regole da applicare dopo che il programma di bilanciamento del carico instrada il traffico verso ciascun microservizio, ad esempio le regole per l'invio del traffico a versioni diverse di un microservizio, puoi creare e applicare le [`DestinationRules` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/).
{: tip}

<br />


## Aggiornamento di Istio su {{site.data.keyword.containerlong_notm}}
{: #istio_update}

La versione di Istio del componente aggiuntivo gestito Istio viene testata da {{site.data.keyword.Bluemix_notm}} e approvata per l'utilizzo in {{site.data.keyword.containerlong_notm}}. Per aggiornare i tuoi componenti Istio alla versione più recente di Istio supportata da {{site.data.keyword.containerlong_notm}}, vedi [Aggiornamento di componenti aggiuntivi gestiti](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons).
{: shortdesc}

## Disinstallazione di Istio su {{site.data.keyword.containerlong_notm}}
{: #istio_uninstall}

Se hai finito di lavorare con Istio, puoi ripulire le risorse Istio nel tuo cluster disinstallando i relativi componenti aggiuntivi.
{:shortdesc}

Il componente aggiuntivo `istio` è una dipendenza per i componenti aggiuntivi `istio-extras`, `istio-sample-bookinfo` e [`knative`](/docs/containers?topic=containers-serverless-apps-knative). Il componente aggiuntivo `istio-extras` è una dipendenza per il componente aggiuntivo `istio-sample-bookinfo`.
{: important}

**Facoltativo**: le risorse create o modificate nello spazio dei nomi `istio-system` e tutte le risorse Kubernetes generate automaticamente dalle definizioni di risorse personalizzate (CRD) vengono rimosse. Se vuoi conservare queste risorse, salvale prima di disinstallare i componenti aggiuntivi `istio`.
1. Salva qualsiasi risorsa, quali i file di configurazione di un qualsiasi servizio o applicazione, tu abbia creato o modificato nello spazio dei nomi `istio-system`.
   Comando di esempio:
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

2. Salva le risorse Kubernetes generate automaticamente dalle CRD in `istio-system` per un file YAML sulla tua macchina locale.
   1. Ottieni le CRD in `istio-system`.
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. Salva le risorse create da queste definizioni di risorse personalizzate.

### Disinstallazione dei componenti aggiuntivi Istio gestiti nella CLI
{: #istio_uninstall_cli}

1. Disabilita il componente aggiuntivo `istio-sample-bookinfo`.
  ```
  ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. Disabilita il componente aggiuntivo `istio-extras`.
  ```
  ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
  ```
  {: pre}

3. Disabilita il componente aggiuntivo `istio`.
  ```
  ibmcloud ks cluster-addon-disable istio --cluster <cluster_name_or_ID> -f
  ```
  {: pre}

4. Verifica che tutti i componenti aggiuntivi Istio gestiti siano disabilitati in questo cluster. Nell'output non viene restituito alcun componente aggiuntivo Istio.
  ```
  ibmcloud ks cluster-addons --cluster <cluster_name_or_ID>
  ```
  {: pre}

### Disinstallazione dei componenti aggiuntivi Istio gestiti nell'IU
{: #istio_uninstall_ui}

1. Nel tuo [dashboard del cluster ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/kubernetes/clusters), fai clic sul nome di un cluster.

2. Fai clic sulla scheda **Add-ons**.

3. Sulla scheda Istio, fai clic sull'icona di menu.

4. Disinstalla i singoli o tutti i componenti aggiuntivi Istio.
  - Singoli componenti aggiuntivi Istio:
    1. Fai clic su **Gestisci**.
    2. Deseleziona le caselle di spunta per i componenti aggiuntivi che vuoi disabilitare. Se deselezioni un componente aggiuntivo, altri componenti aggiuntivi che richiedono tale componente aggiuntivo come dipendenza potrebbero essere automaticamente deselezionati.
    3. Fai clic su **Gestisci**. I componenti aggiuntivi Istio vengono disabilitati e le risorse per tali componenti aggiuntivi vengono rimosse da questo cluster.
  - Tutti i componenti aggiuntivi Istio:
    1. Fai clic su **Uninstall**. Tutti i componenti aggiuntivi Istio gestiti vengono disabilitati e tutte le risorse Istio vengono rimosse dal cluster.

5. Sulla scheda Istio, verifica che i componenti aggiuntivi che hai disinstallato non siano più elencati.

<br />


### Disinstallazione di altre installazioni di Istio nel tuo cluster
{: #istio_uninstall_other}

Se in precedenza hai installato Istio nel cluster utilizzando il grafico Helm IBM o un altro metodo, ripulisci tale installazione di Istio prima di abilitare i componenti aggiuntivi Istio gestiti nel cluster. Per verificare se Istio è già in un cluster, esegui `kubectl get namespaces` e cerca lo spazio dei nomi `istio-system` nell'output.
{: shortdesc}

- Se hai installato Istio utilizzando il grafico Helm Istio {{site.data.keyword.Bluemix_notm}}:
  1. Disinstalla la distribuzione Istio Helm.
    ```
    helm del istio --purge
    ```
    {: pre}

  2. Se hai utilizzato Helm 2.9 o precedenti, elimina la risorsa di lavoro supplementare.
    ```
    kubectl -n istio-system delete job --all
    ```
    {: pre}

- Se hai installato Istio manualmente o hai utilizzato il grafico Helm della community Istio, consulta la [documentazione di disinstallazione di Istio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/setup/kubernetes/quick-start/#uninstall-istio-core-components).
* Se hai precedentemente installato BookInfo nel cluster, ripulisci tali risorse.
  1. Modifica la directory con l'ubicazione del file Istio.
    ```
    cd <filepath>/istio-1.1.5
    ```
    {: pre}

  2. Elimina tutti i servizi, i pod e le distribuzioni di BookInfo nel cluster.
    ```
    samples/bookinfo/platform/kube/cleanup.sh
    ```
    {: pre}

<br />


## Operazioni successive
{: #istio_next}

* Per esplorare ulteriormente Istio, puoi trovare ulteriori guide nella [documentazione di Istio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/).
* Segui il corso [Cognitive Class: Getting started with Microservices with Istio and IBM Cloud Kubernetes Service ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cognitiveclass.ai/courses/get-started-with-microservices-istio-and-ibm-cloud-container-service/). **Nota**: puoi tralasciare la sezione relativa all'installazione di Istio di questo corso.
* Controlla questo post del blog relativo all'utilizzo di [Vistio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://itnext.io/vistio-visualize-your-istio-mesh-using-netflixs-vizceral-b075c402e18e) per visualizzare la tua rete di servizi Istio.
