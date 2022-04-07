---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

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


# Esercitazione: Utilizzo di Knative gestito per eseguire le applicazioni senza server nei cluster Kubernetes
{: #knative_tutorial}

Con questa esercitazione, puoi apprendere come installare Knative in un cluster Kubernetes in {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

**Cos'è Knative e perché dovrei utilizzarlo?**</br>
[Knative](https://github.com/knative/docs) è una piattaforma open source che è stata sviluppata da IBM, Google, Pivotal, Red Hat, Cisco e altri con l'obiettivo di estendere le funzionalità di Kubernetes per aiutarti a creare applicazioni senza server e inserite in contenitori moderne e basate sul sorgente in aggiunta al tuo cluster Kubernetes. La piattaforma è progettata per rispondere alle esigenze degli sviluppatori che oggi devono decidere quale tipo di applicazione vogliono eseguire nel cloud: applicazioni a 12 fattori, contenitori o funzioni. Ogni tipo di applicazione richiede una soluzione open source o proprietaria adattata a tali applicazioni: Cloud Foundry per le applicazioni a 12 fattori, Kubernetes per i contenitori e OpenWhisk e altri per le funzioni. In passato, gli sviluppatori dovevano decidere quale approccio volevano seguire, il che portava a un'assenza di flessibilità e a una complessità quando dovevano essere combinati diversi tipi di applicazioni.  

Knative utilizza un approccio congruente tra i linguaggi di programmazione e i framework per astrarre l'impegno operativo derivante dal creare, distribuire e gestire carichi di lavoro in Kubernetes in modo che gli sviluppatori possano concentrarsi su quello che conta di più per loro: il codice sorgente. Puoi utilizzare dei pacchetti di build collaudati con cui hai già dimestichezza, come ad esempio Cloud Foundry, Kaniko, Dockerfile, Bazel e altri. Mediante l'integrazione con Istio, Knative garantisce che i tuoi carichi di lavoro senza server e inseriti in contenitori possano essere esposti su internet, monitorati e controllati facilmente e che i tuoi dati siano crittografati durante il transito.

**Come funziona Knative?**</br>
Knative viene fornito con 3 componenti chiave, o _primitive_, che ti aiutano a creare, distribuire e gestire le tue applicazioni senza server nel tuo cluster Kubernetes:

- **Build:** la primitiva `Build` supporta la creazione di una serie di passi per creare la tua applicazione dal codice sorgente a un'immagine contenitore. Immagina di utilizzare un semplice template di creazione dove specifichi il repository di origine per trovare il tuo codice dell'applicazione e il registro contenitore dove desideri ospitare l'immagine. Con solo un singolo comando, puoi indicare a Knative di prendere questo template di build, eseguire il pull del codice sorgente, creare l'immagine ed eseguire il push dell'immagine nel tuo registro del contenitore in modo da poterla usare nel tuo contenitore.
- **Serving:** la primitiva `Serving` aiuta a distribuire le applicazioni senza server come servizi Knative e a ridimensionarli automaticamente, anche riducendoli fino a zero istanze. Utilizzando le funzionalità di gestione del traffico e di instradamento intelligente di Istio, puoi controllare quale traffico viene instradato a una specifica versione del tuo server, cosa che consente a uno sviluppatore di testare e distribuire una nuova versione dell'applicazione o di eseguire dei test A--B facilmente.
- **Eventing:** con la primitiva `Eventing`, puoi creare dei trigger o dei flussi di eventi che altri servizi possono sottoscrivere. Ad esempio, potresti voler preparare una nuova build della tua applicazione ogni volta che viene eseguito il push del codice nel tuo repository master GitHub. Potresti anche volere eseguire un'applicazione senza server solo se la temperatura scende sotto il punto di congelamento. La primitiva `Eventing` può essere integrata nella tua pipeline CI/CD per automatizzare la creazione e la distribuzione di applicazioni nel caso in cui si verifichi un evento specifico.

**Cos'è il componente aggiuntivo Managed Knative on {{site.data.keyword.containerlong_notm}} (sperimentale)?** </br>
Managed Knative on {{site.data.keyword.containerlong_notm}} è un componente aggiuntivo gestito che integra Knative e Istio direttamente con il tuo cluster Kubernetes. Le versioni di Knative e Istio nel componente aggiuntivo sono testate da IBM e supportate per l'utilizzo in {{site.data.keyword.containerlong_notm}}. Per ulteriori informazioni sui componenti aggiuntivi gestiti, vedi [Aggiunta di servizi utilizzando i componenti aggiuntivi gestiti](/docs/containers?topic=containers-managed-addons#managed-addons).

**Ci sono delle limitazioni?** </br>
Se hai installato il [controller di ammissione Container image security enforcer](/docs/services/Registry?topic=registry-security_enforce#security_enforce) nel tuo cluster, non puoi abilitare il componente aggiuntivo Knative gestito nel cluster.

Questo è di tuo gradimento? Attieniti a questa esercitazione per iniziare a lavorare con Knative in {{site.data.keyword.containerlong_notm}}.

## Obiettivi
{: #knative_objectives}

- Impara le basi su Knative e sulle primitive Knative.  
- Installa il componente aggiuntivo Istio gestito e Knative gestito nel tuo cluster.
- Distribuisci la tua prima applicazione senza server con Knative ed esponi l'applicazione su internet utilizzando la primitiva `Serving` di Knative.
- Esplora le funzionalità di ridimensionamento e revisione di Knative.

## Tempo richiesto
{: #knative_time}

30 minuti

## Destinatari
{: #knative_audience}

Questa esercitazione è progettata per gli sviluppatori interessati a imparare come utilizzare Knative per distribuire un'applicazione senza server in un cluster Kubernetes e per gli amministratori del cluster che vogliono imparare come configurare Knative in un cluster.

## Prerequisiti
{: #knative_prerequisites}

-  [Installa la CLI IBM Cloud, il plugin {{site.data.keyword.containerlong_notm}} e la CLI Kubernetes](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps). Assicurati di installare la versione della CLI `kubectl` che corrisponde alla versione Kubernetes del tuo cluster.
-  [Crea un cluster con almeno 3 nodi di lavoro, ciascuno dei quali con 4 core e 16 GB di memoria (`b3c.4x16`) o più](/docs/containers?topic=containers-clusters#clusters_cli). Ogni nodo di lavoro deve eseguire Kubernetes versione 1.12 o superiore.
-  Assicurati di disporre del [ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Scrittore** o **Gestore**](/docs/containers?topic=containers-users#platform) per {{site.data.keyword.containerlong_notm}}.
-  [Indirizza la CLI al tuo cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

## Lezione 1: Configurazione del componente aggiuntivo Knative gestito
{: #knative_setup}

Knative si basa su Istio per garantire che i tuoi carichi di lavoro senza server e inseriti nei contenitori possano essere esposti all'interno del cluster e su internet. Con Istio, puoi anche monitorare e controllare il traffico di rete tra i tuoi servizi e garantire che i tuoi dati siano crittografati durante il transito. Quando installi il componente aggiuntivo Knative gestito, viene anche installato automaticamente il componente aggiuntivo Istio gestito.
{: shortdesc}

1. Abilita il componente aggiuntivo Knative gestito nel tuo cluster. Quando abiliti Knative nel tuo cluster, Istio e tutti i componenti Knative vengono installati nel tuo cluster.
   ```
   ibmcloud ks cluster-addon-enable knative --cluster <cluster_name_or_ID> -y
   ```
   {: pre}

   Output di esempio:
   ```
   Enabling add-on knative for cluster knative...
   OK
   ```
   {: screen}

   Il completamento dell'installazione di tutti i componenti Knative potrebbe richiedere qualche minuto.

2. Verifica che Istio sia stato installato correttamente. Tutti i pod per i 9 servizi Istio e il pod per Prometheus devono essere in uno stato di `Running`.
   ```
   kubectl get pods --namespace istio-system
   ```
   {: pre}

   Output di esempio:
   ```
   NAME                                       READY     STATUS      RESTARTS   AGE
    istio-citadel-748d656b-pj9bw               1/1       Running     0          2m
    istio-egressgateway-6c65d7c98d-l54kg       1/1       Running     0          2m
    istio-galley-65cfbc6fd7-bpnqx              1/1       Running     0          2m
    istio-ingressgateway-f8dd85989-6w6nj       1/1       Running     0          2m
    istio-pilot-5fd885964b-l4df6               2/2       Running     0          2m
    istio-policy-56f4f4cbbd-2z2bk              2/2       Running     0          2m
    istio-sidecar-injector-646655c8cd-rwvsx    1/1       Running     0          2m
    istio-statsd-prom-bridge-7fdbbf769-8k42l   1/1       Running     0          2m
    istio-telemetry-8687d9d745-mwjbf           2/2       Running     0          2m
    prometheus-55c7c698d6-f4drj                1/1       Running     0          2m
   ```
   {: screen}

3. Facoltativo: se desideri utilizzare Istio per tutte le applicazioni nello spazio dei nomi `default`, aggiungi l'etichetta `istio-injection=enabled` allo spazio dei nomi. Ogni pod dell'applicazione senza server deve eseguire un sidecar proxy Envoy in modo che l'applicazione possa essere inclusa nella rete di servizi Istio. Questa etichetta consente a Istio di modificare automaticamente la specifica del template del pod nelle nuove distribuzioni di applicazione in modo che i pod vengano creati con i contenitori di sidecar proxy Envoy.
  ```
  kubectl label namespace default istio-injection=enabled
  ```
  {: pre}

4. Verifica che tutti i componenti Knative siano stati installati correttamente.
   1. Verifica che tutti i pod del componente `Serving` di Knative siano in uno stato `Running`.  
      ```
      kubectl get pods --namespace knative-serving
      ```
      {: pre}

      Output di esempio:
      ```
      NAME                          READY     STATUS    RESTARTS   AGE
      activator-598b4b7787-ps7mj    2/2       Running   0          21m
      autoscaler-5cf5cfb4dc-mcc2l   2/2       Running   0          21m
      controller-7fc84c6584-qlzk2   1/1       Running   0          21m
      webhook-7797ffb6bf-wg46v      1/1       Running   0          21m
      ```
      {: screen}

   2. Verifica che tutti i pod del componente `Build` di Knative siano in uno stato `Running`.  
      ```
      kubectl get pods --namespace knative-build
      ```
      {: pre}

      Output di esempio:
      ```
      NAME                                READY     STATUS    RESTARTS   AGE
      build-controller-79cb969d89-kdn2b   1/1       Running   0          21m
      build-webhook-58d685fc58-twwc4      1/1       Running   0          21m
      ```
      {: screen}

   3. Verifica che tutti i pod del componente `Eventing` di Knative siano in uno stato `Running`.
      ```
      kubectl get pods --namespace knative-eventing
      ```
      {: pre}

      Output di esempio:

      ```
      NAME                                            READY     STATUS    RESTARTS   AGE
      eventing-controller-847d8cf969-kxjtm            1/1       Running   0          22m
      in-memory-channel-controller-59dd7cfb5b-846mn   1/1       Running   0          22m
      in-memory-channel-dispatcher-67f7497fc-dgbrb    2/2       Running   1          22m
      webhook-7cfff8d86d-vjnqq                        1/1       Running   0          22m
      ```
      {: screen}

   4. Verifica che tutti i pod del componente `Sources` di Knative siano in uno stato `Running`.
      ```
      kubectl get pods --namespace knative-sources
      ```
      {: pre}

      Output di esempio:
      ```
      NAME                   READY     STATUS    RESTARTS   AGE
      controller-manager-0   1/1       Running   0          22m
      ```
      {: screen}

   5. Verifica che tutti i pod del componente `Monitoring` di Knative siano in uno stato `Running`.
      ```
      kubectl get pods --namespace knative-monitoring
      ```
      {: pre}

      Output di esempio:
      ```
      NAME                                  READY     STATUS                 RESTARTS   AGE
      elasticsearch-logging-0               1/1       Running                0          22m
      elasticsearch-logging-1               1/1       Running                0          21m
      grafana-79cf95cc7-mw42v               1/1       Running                0          21m
      kibana-logging-7f7b9698bc-m7v6r       1/1       Running                0          22m
      kube-state-metrics-768dfff9c5-fmkkr   4/4       Running                0          21m
      node-exporter-dzlp9                   2/2       Running                0          22m
      node-exporter-hp6gv                   2/2       Running                0          22m
      node-exporter-hr6vs                   2/2       Running                0          22m
      prometheus-system-0                   1/1       Running                0          21m
      prometheus-system-1                   1/1       Running                0          21m
      ```
      {: screen}

Ottimo! Con Knative e Istio debitamente configurati, puoi ora distribuire la tua prima applicazione senza server al tuo cluster.

## Lezione 2: Distribuisci un'applicazione senza server al tuo cluster
{: #deploy_app}

In questa lezione, distribuisci la tua prima applicazione [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) senza server in Go. Quando invii una richiesta alla tua applicazione di esempio, l'applicazione legge la variabile di ambiente `TARGET` e stampa `"Hello ${TARGET}!"`. Se questa variabile di ambiente è vuota, viene restituito `"Hello World!"`.
{: shortdesc}

1. Crea un file YAML per la tua prima applicazione `Hello World` senza server in Knative. Per distribuire un'applicazione con Knative, devi specificare una risorsa del servizio Knative. Un servizio è gestito dalla primitiva `Serving` di Knative ed è responsabile della gestione dell'intero ciclo di vita del carico di lavoro. Il servizio garantisce che ciascuna distribuzione abbia una revisione, un instradamento e una configurazione Knative. Quando aggiorni il servizio, una nuova versione dell'applicazione viene creata e aggiunta alla cronologia delle revisioni del servizio. Gli instradamenti Knative garantiscono che ciascuna revisione dell'applicazione sia associata a un endpoint di rete in modo che tu possa controllare quanto traffico di rete viene instradato a una specifica revisione. Le configurazioni Knative mantengono le impostazioni di una specifica revisione per consentirti così di poter sempre eseguire il rollback a una versione meno recente o di passare da una revisione all'altra. Per ulteriori informazioni sulle risorse `Serving` di Knative, vedi la [documentazione di Knative](https://github.com/knative/docs/tree/master/serving).
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Go Sample v1"
    ```
    {: codeblock}

    <table>
    <caption>Descrizione dei componenti del file YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>Il nome del tuo servizio Knative.</td>
    </tr>
    <tr>
    <td><code>metadata.namespace</td>
    <td>Lo spazio dei nomi Kubernetes in cui vuoi distribuire la tua applicazione come un servizio Knative. </td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>L'URL al registro del contenitore in cui è archiviata la tua immagine. In questo esempio, distribuisci un'applicazione Hello World di Knative archiviata nello spazio dei nomi <code>ibmcom</code> in Docker Hub. </td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>Un elenco delle variabili di ambiente che vuoi che abbia il tuo servizio Knative. In questo esempio, il valore della variabile di ambiente <code>TARGET</code> viene letto dall'applicazione di esempio e restituito quando invii una richiesta alla tua applicazione nel formato <code>"Hello ${TARGET}!"</code>. Se non viene fornito alcun valore, l'applicazione di esempio restituisce <code>"Hello World!"</code>.  </td>
    </tr>
    </tbody>
    </table>

2. Crea il servizio Knative nel tuo cluster. Quando crei il servizio, la primitiva `Serving` di Knative crea una revisione immutabile, un instradamento Knative, una regola di instradamento Ingress, un servizio Kubernetes , un pod Kubernetes e un programma di bilanciamento del carico per la tua applicazione. Alla tua applicazione viene assegnato un dominio secondario dal tuo dominio secondario Ingress nel formato `<knative_service_name>.<namespace>.<ingress_subdomain>` che puoi utilizzare per accedere all'applicazione da internet.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   Output di esempio:
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. Verifica che il tuo pod sia stato creato. Il tuo pod è composto da due contenitori. Un contenitore esegue la tua applicazione `Hello World` e l'altra applicazione è un sidecar che esegue gli strumenti di monitoraggio e registrazione Istio e Knative. Al tuo pod viene assegnato un numero di revisione `00001`.
   ```
   kubectl get pods
   ```
   {: pre}

   Output di esempio:
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00001-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
   ```
   {: screen}

4. Prova la tua applicazione `Hello World`.
   1. Ottieni il dominio predefinito assegnato al tuo servizio Knative. Se hai modificato il nome del tuo servizio Knative, o distribuito l'applicazione a uno spazio dei nomi differente, aggiorna questi valori nella tua query.
      ```
      kubectl get ksvc/kn-helloworld
      ```
      {: pre}

      Output di esempio:
      ```
      NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
      kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-rjmwt   kn-helloworld-rjmwt   True
      ```
      {: screen}

   2. Effettua una richiesta alla tua applicazione utilizzando il dominio secondario che hai richiamato nel passo precedente.
      ```
      curl -v <service_domain>
      ```
      {: pre}

      Output di esempio:
      ```
      * Rebuilt URL to: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud/
      *   Trying 169.46.XX.XX...
      * TCP_NODELAY set
      * Connected to kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud (169.46.XX.XX) port 80 (#0)
      > GET / HTTP/1.1
      > Host: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud
      > User-Agent: curl/7.54.0
      > Accept: */*
      >
      < HTTP/1.1 200 OK
      < Date: Thu, 21 Mar 2019 01:12:48 GMT
      < Content-Type: text/plain; charset=utf-8
      < Content-Length: 20
      < Connection: keep-alive
      < x-envoy-upstream-service-time: 17
      <
      Hello Go Sample v1!
      * Connection #0 to host kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud left intact
      ```
      {: screen}

5. Aspetta qualche minuto per lasciare che Knative riduca il tuo pod. Knative valuta il numero di pod che deve essere attivo in un dato momento per elaborare il carico di lavoro in entrata. Se non viene ricevuto traffico di rete, Knative riduce automaticamente i pod anche azzerandoli, come mostrato nel seguente esempio.

   Vuoi vedere in che modo Knative amplia i tuoi pod? Prova ad aumentare il carico di lavoro per la tua applicazione, ad esempio utilizzando strumenti quali il [semplice programma di test del carico basato sul cloud](https://loader.io/).
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   Se non vedi alcun pod `kn-helloworld`, Knative ha ridotto la tua applicazione a zero pod.

6. Aggiorna il tuo esempio di servizio Knative e immetti un valore differente per la variabile di ambiente `TARGET`.

   YAML di servizio di esempio:
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Mr. Smith"
    ```
    {: codeblock}

7. Applica la modifica al tuo servizio. Quando modifichi la configurazione, Knative crea automaticamente una nuova revisione, assegna un nuovo instradamento e, per impostazione predefinita, indica a Istio di instradare il traffico di rete in entrata alla revisione più recente.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

8. Effettua una nuova richiesta alla tua applicazione per verificare che la tua modifica sia stata applicata.
   ```
   curl -v <service_domain>
   ```

   Output di esempio:
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

9. Verifica che Knative abbia nuovamente ampliato il tuo pod per tenere conto del traffico di rete aumentato. Al tuo pod viene assegnato un numero di revisione `00002`. Puoi utilizzare il numero di revisione per fare riferimento a una specifica versione della tua applicazione, ad esempio quando vuoi indicare a Istio di suddividere il traffico in entrata tra due revisioni.
   ```
   kubectl get pods
   ```

   Output di esempio:
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00002-deployment-55db6bf4c5-2vftm   3/3       Running   0          16s
   ```
   {: screen}

10. Facoltativo: ripulisci il tuo servizio Knative.
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}

Ottimo. Hai distribuito correttamente la tua prima applicazione Knative al tuo cluster ed esplorato le funzionalità di revisione e ridimensionamento della primitiva `Serving` di Knative.


## Operazioni successive   
{: #whats-next}

- Prova questo [workshop di Knative ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/IBM/knative101/tree/master/workshop) per distribuire la tua prima applicazione Fibonacci `Node.js` al tuo cluster.
  - Esplora come utilizzare la primitiva `Build` di Knative per creare un'immagine da un Dockerfile in GitHub ed eseguire automaticamente il push dell'immagine al tuo spazio dei nomi in {{site.data.keyword.registrylong_notm}}.  
  - Impara come puoi configurare l'instradamento per il traffico di rete dal dominio secondario Ingress fornito da IBM al gateway Ingress Istio fornito da Knative.
  - Distribuisci una nuova versione della tua applicazione e utilizza Istio per controllare la quantità di traffico instradata a ciascuna versione dell'applicazione.
- Esplora gli esempi [`Eventing` di Knative ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/knative/docs/tree/master/eventing/samples).
- Scopri di più su Knative con la [documentazione di Knative ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/knative/docs).
