---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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


# Distribuzione di applicazioni senza server con Knative
{: #serverless-apps-knative}

Apprendi come installare e utilizzare Knative in un cluster Kubernetes in {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

**Cos'è Knative e perché potrei usarlo?**</br>
[Knative](https://github.com/knative/docs) è una piattaforma open source sviluppata da IBM, Google, Pivotal, Red Hat, Cisco e altri, con l'obiettivo di estendere le funzionalità di Kubernetes per aiutarti a creare applicazioni moderne senza server, inserite in contenitori e basate sul sorgente, sul tuo cluster Kubernetes. La piattaforma è progettata per rispondere alle esigenze degli sviluppatori che oggi devono decidere quale tipo di applicazione vogliono eseguire nel cloud: applicazioni a 12 fattori, contenitori o funzioni. Ogni tipo di applicazione richiede una soluzione open source o proprietaria studiata per tali applicazioni: Cloud Foundry per le applicazioni a 12 fattori, Kubernetes per i contenitori e OpenWhisk e altri per le funzioni. In passato, gli sviluppatori dovevano decidere quale approccio volevano seguire, il che portava a un'assenza di flessibilità e a una complessità quando dovevano essere combinati diversi tipi di applicazioni.  

Knative utilizza un approccio congruente tra i linguaggi di programmazione e i framework per astrarre il carico operativo derivante dal creare, distribuire e gestire carichi di lavoro in Kubernetes in modo che gli sviluppatori possano concentrarsi su quello che conta di più per loro: il codice sorgente. Puoi utilizzare dei pacchetti di build collaudati con cui hai già dimestichezza, come ad esempio Cloud Foundry, Kaniko, Dockerfile, Bazel e altri. Mediante l'integrazione con Istio, Knative garantisce che i tuoi carichi di lavoro senza server e inseriti in contenitori possano essere esposti su internet, monitorati e controllati facilmente e che i tuoi dati siano crittografati durante il transito.

**Come funziona Knative?**</br>
Knative viene fornito con tre componenti chiave, o _primitive_, che ti aiutano a creare, distribuire e gestire le tue applicazioni senza server nel tuo cluster Kubernetes:

- **Build:** la primitiva `Build` supporta la creazione di una serie di passi per creare la tua applicazione dal codice sorgente a un'immagine contenitore. Immagina di utilizzare un semplice template di build dove specifichi il repository di origine per trovare il tuo codice dell'applicazione e il registro contenitore dove desideri ospitare l'immagine. Con un solo comando, puoi indicare a Knative di prendere questo template di build, eseguire il pull del codice sorgente, creare l'immagine ed eseguirne il push nel registro del tuo contenitore, in modo da poterla usare nel tuo contenitore.
- **Serving:** la primitiva `Serving` aiuta a distribuire le applicazioni senza server come servizi Knative e a ridimensionarli automaticamente, anche riducendoli fino a zero istanze. Per esporre i tuoi carichi di lavoro senza server inseriti in contenitori, Knative utilizza Istio. Quando installi il componente aggiuntivo Knative gestito, viene anche installato automaticamente il componente aggiuntivo Istio gestito. Utilizzando le funzionalità di gestione del traffico e di instradamento intelligente di Istio, puoi controllare quale traffico viene instradato a una specifica versione del tuo servizio, il che consente a uno sviluppatore di testare e distribuire una nuova versione dell'applicazione o di eseguire dei test A-B facilmente.
- **Eventing:** con la primitiva `Eventing`, puoi creare dei trigger o dei flussi di eventi che altri servizi possono sottoscrivere. Ad esempio, potresti voler preparare una nuova build della tua applicazione ogni volta che viene eseguito il push del codice nel tuo repository master GitHub. Potresti anche volere eseguire un'applicazione senza server solo se la temperatura scende sotto il punto di congelamento. Ad esempio, la primitiva `Eventing` può essere integrata nella tua pipeline CI/CD per automatizzare la creazione e la distribuzione di applicazioni nel caso in cui si verifichi un evento specifico.

**Cos'è il componente aggiuntivo Managed Knative on {{site.data.keyword.containerlong_notm}} (sperimentale)?** </br>
Managed Knative on {{site.data.keyword.containerlong_notm}} è un [componente aggiuntivo gestito](/docs/containers?topic=containers-managed-addons#managed-addons) che integra Knative e Istio direttamente con il tuo cluster Kubernetes. Le versioni di Knative e Istio nel componente aggiuntivo sono testate da IBM e supportate per l'utilizzo in {{site.data.keyword.containerlong_notm}}. Per ulteriori informazioni sui componenti aggiuntivi gestiti, vedi [Aggiunta di servizi utilizzando i componenti aggiuntivi gestiti](/docs/containers?topic=containers-managed-addons#managed-addons).

**Ci sono delle limitazioni?** </br>
Se hai installato il [controller di ammissione Container image security enforcer](/docs/services/Registry?topic=registry-security_enforce#security_enforce) nel tuo cluster, non puoi abilitare il componente aggiuntivo Knative gestito nel cluster.

## Configurazione Knative nel tuo cluster
{: #knative-setup}

Knative si basa su Istio per garantire che i tuoi carichi di lavoro senza server e inseriti nei contenitori possano essere esposti all'interno del cluster e su internet. Con Istio, puoi anche monitorare e controllare il traffico di rete tra i tuoi servizi e garantire che i tuoi dati siano crittografati durante il transito. Quando installi il componente aggiuntivo Knative gestito, viene anche installato automaticamente il componente aggiuntivo Istio gestito.
{: shortdesc}

Prima di iniziare:
-  [Installa la CLI IBM Cloud, il plugin {{site.data.keyword.containerlong_notm}} e la CLI Kubernetes](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps). Assicurati di installare la versione della CLI `kubectl` che corrisponde alla versione Kubernetes del tuo cluster.
-  [Crea un cluster standard con almeno 3 nodi di lavoro, ciascuno dei quali con 4 core e 16 GB di memoria (`b3c.4x16`) o più](/docs/containers?topic=containers-clusters#clusters_ui). Inoltre, il cluster e i nodi di lavoro devono eseguire almeno la versione minima supportata di Kubernetes, verificabile eseguendo `addon-versions --addon knative`.
-  Assicurati di disporre del [ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Scrittore** o **Gestore**](/docs/containers?topic=containers-users#platform) per {{site.data.keyword.containerlong_notm}}.
-  [Indirizza la CLI al tuo cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
</br>

Per installare Knative nel tuo cluster:

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

2. Verifica che Istio sia stato installato correttamente. Lo stato di tutti i pod per i nove servizi Istio e il pod per Prometheus deve essere `Running`.
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

4. Verifica che tutti i pod del componente `Serving` di Knative siano in uno stato `Running`.
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

## Utilizzo dei servizi Knative per la distribuzione di un'applicazione senza server
{: #knative-deploy-app}

Una volta configurato Knative nel tuo cluster, puoi distribuire la tua applicazione senza server come servizio Knative.
{: shortdesc}

**Cos'è un servizio Knative?** </br>
Per distribuire un'applicazione con Knative, devi specificare una risorsa `Service` Knative. Un servizio Knative è gestito dalla primitiva `Serving` di Knative ed è a capo della gestione dell'intero ciclo di vita del carico di lavoro. Quando crei il servizio, la primitiva`Serving` di Knative crea automaticamente una versione per la tua applicazione senza server e la aggiunge alla cronologia delle revisioni del servizio. Alla tua applicazione senza server viene assegnato dal tuo dominio secondario Ingress un URL pubblico in formato `<knative_service_name>.<namespace>.<ingress_subdomain>` che puoi utilizzare per accedere all'applicazione da internet. Inoltre, alla tua applicazione viene assegnato un nome host privato in formato `<knative_service_name>.<namespace>.cluster.local`, che puoi utilizzare per accedere alla tua applicazione dall'interno del cluster.

**Cosa accade dietro le quinte quando creo un servizio Knative?**</br>
Quando crei un servizio Knative, la tua applicazione viene distribuita automaticamente come pod Kubernetes nel tuo cluster ed esposta utilizzando un servizio Kubernetes. Per assegnare il nome host pubblico, Knative utilizza il certificato TLS e il sottodominio Ingress fornito da IBM. Il traffico di rete in entrata viene instradato in base alle regole di instradamento Ingress predefinite fornite da IBM.

**Come posso distribuire una nuova versione della mia applicazione?**</br>
Quando aggiorni il tuo servizio Knative, viene creata una nuova versione della tua applicazione senza server. A questa versione vengono assegnati gli stessi nomi host pubblico e privato della tua versione precedente. Per impostazione predefinita, tutto il traffico di rete in entrata viene instradato all'ultima versione della tua applicazione. Tuttavia, puoi specificare anche la percentuale di traffico di rete in entrata che vuoi instradare a una versione specifica dell'applicazione, in modo da poter eseguire i test A-B. Puoi suddividere il traffico di rete in entrata tra due versioni dell'applicazione alla volta: la versione corrente della tua applicazione e la nuova versione che vuoi aggiungere.  

**Posso usare il mio certificato TLS e il mio dominio personalizzato?** </br>
Puoi modificare la mappa di configurazione del tuo gateway Ingress Istio e le regole di instradamento Ingress per utilizzare il tuo nome dominio personalizzato e il tuo certificato TLS quando assegni un nome host alla tua applicazione senza server. Per ulteriori informazioni, vedi [Configurazione di certificati e nomi dominio personalizzati](#knative-custom-domain-tls).

Per distribuire la tua applicazione senza server come servizio Knative:

1. Crea un file YAML per la tua prima applicazione [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) senza server in Go con Knative. Quando invii una richiesta alla tua applicazione di esempio, l'applicazione legge la variabile di ambiente `TARGET` e stampa `"Hello ${TARGET}!"`. Se questa variabile di ambiente è vuota, viene restituito `"Hello World!"`.
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
    <td>Facoltativo: lo spazio dei nomi Kubernetes in cui vuoi distribuire la tua applicazione come servizio Knative. Per impostazione predefinita, tutti i servizi vengono distribuiti nello spazio dei nomi Kubernetes <code>default</code>. </td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>L'URL al registro del contenitore in cui è archiviata la tua immagine. In questo esempio, distribuisci un'applicazione Hello World di Knative archiviata nello spazio dei nomi <code>ibmcom</code> in Docker Hub. </td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>Facoltativo: un elenco delle variabili di ambiente che vuoi che abbia il tuo servizio Knative. In questo esempio, il valore della variabile di ambiente <code>TARGET</code> viene letto dall'applicazione di esempio e restituito quando invii una richiesta alla tua applicazione nel formato <code>"Hello ${TARGET}!"</code>. Se non viene fornito alcun valore, l'applicazione di esempio restituisce <code>"Hello World!"</code>.  </td>
    </tr>
    </tbody>
    </table>

2. Crea il servizio Knative nel tuo cluster.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   Output di esempio:
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. Verifica che il tuo servizio Knative sia stato creato. Nell'output della tua CLI, puoi visualizzare il **DOMAIN** pubblico assegnato alla tua applicazione senza server. Le colonne **LATESTCREATED** e **LATESTREADY** mostrano la versione della tua applicazione creata per ultima e attualmente distribuita in formato `<knative_service_name>-<version>`. La versione assegnata alla tua applicazione è un valore stringa casuale. In questo esempio, la versione della tua applicazione senza server è `rjmwt`. Quando aggiorni il servizio, viene creata una nuova versione della tua applicazione a cui viene assegnata una nuova stringa casuale per la versione.  
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

4. Prova la tua applicazione `Hello World` inviando una richiesta all'URL pubblico che le è stato assegnato.
   ```
   curl -v <public_app_url>
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

5. Elenca il numero di pod creati per il tuo servizio Knative. Nell'esempio di questo argomento, viene distribuito un unico pod formato da due contenitori. Un contenitore esegue la tua applicazione `Hello World` e l'altra applicazione è un sidecar che esegue gli strumenti di monitoraggio e registrazione Istio e Knative.
   ```
   kubectl get pods
   ```
   {: pre}

   Output di esempio:
   ```
   NAME                                             READY     STATUS    RESTARTS   AGE
   kn-helloworld-rjmwt-deployment-55db6bf4c5-2vftm  2/2      Running   0          16s
   ```
   {: screen}

6. Aspetta qualche minuto per lasciare che Knative riduca il tuo pod. Knative valuta il numero di pod che deve essere attivo in un dato momento per elaborare il carico di lavoro in entrata. Se non viene ricevuto traffico di rete, Knative riduce automaticamente i pod anche azzerandoli, come mostrato nel seguente esempio.

   Vuoi vedere in che modo Knative amplia i tuoi pod? Prova ad aumentare il carico di lavoro per la tua applicazione, ad esempio utilizzando strumenti quali il [semplice programma di test del carico basato sul cloud](https://loader.io/).
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   Se non vedi alcun pod `kn-helloworld`, Knative ha ridotto la tua applicazione a zero pod.

7. Aggiorna il tuo esempio di servizio Knative e immetti un valore differente per la variabile di ambiente `TARGET`.

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

8. Applica la modifica al tuo servizio. Quando modifichi la configurazione, Knative crea automaticamente una nuova versione per la tua applicazione.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

9. Verifica che sia stata distribuita una nuova versione della tua applicazione. Nell'output della tua CLI, puoi vedere la nuova versione della tua applicazione nella colonna **LATESTCREATED**. Se vedi la stessa versione dell'applicazione nella colonna **LATESTREADY**, la tua applicazione è completamente configurata e pronta a ricevere il traffico di rete in entrata sull'URL pubblico assegnato.
   ```
   kubectl get ksvc/kn-helloworld
   ```
   {: pre}

   Output di esempio:
   ```
   NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
   kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-ghyei   kn-helloworld-ghyei   True
   ```
   {: screen}

9. Effettua una nuova richiesta alla tua applicazione per verificare che la tua modifica sia stata applicata.
   ```
   curl -v <service_domain>
   ```

   Output di esempio:
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

10. Verifica che Knative abbia nuovamente ampliato il tuo pod per tenere conto del traffico di rete aumentato.
    ```
    kubectl get pods
    ```

    Output di esempio:
    ```
    NAME                                              READY     STATUS    RESTARTS   AGE
    kn-helloworld-ghyei-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
    ```
    {: screen}

11. Facoltativo: ripulisci il tuo servizio Knative.
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}


## Configurazione di certificati e nomi dominio personalizzati
{: #knative-custom-domain-tls}

Puoi configurare Knative in modo da assegnare nomi host dal tuo dominio personalizzato configurato con TLS.
{: shortdesc}

Per impostazione predefinita, a ciascuna applicazione viene assegnato dal tuo dominio secondario Ingress un dominio secondario pubblico in formato `<knative_service_name>.<namespace>.<ingress_subdomain>` che puoi utilizzare per accedere all'applicazione da Internet. Inoltre, alla tua applicazione viene assegnato un nome host privato in formato `<knative_service_name>.<namespace>.cluster.local`, che puoi utilizzare per accedere alla tua applicazione dall'interno del cluster. Se desideri assegnare nomi host da un tuo dominio personalizzato, puoi modificare la mappa di configurazione Knative in modo da utilizzare, invece, il dominio personalizzato.

1. Crea un dominio personalizzato. Per registrare il tuo dominio personalizzato, utilizza il provider DNS (Domain Name Service) o il [DNS IBM Cloud](/docs/infrastructure/dns?topic=dns-getting-started).
2. Configura il tuo dominio per instradare il traffico di rete in entrata al gateway Ingress fornito da IBM. Scegli tra queste opzioni:
   - Definisci un alias per il tuo dominio personalizzato specificando il dominio fornito da IBM come un record di nome canonico (CNAME). Per trovare il dominio Ingress fornito da IBM, esegui `ibmcloud ks cluster-get --cluster <cluster_name>` e cerca il campo **Dominio secondario Ingress**. L'utilizzo di un CNAME è preferito perché IBM fornisce dei controlli dell'integrità automatici sul dominio secondario IBM e rimuove gli eventuali IP malfunzionanti dalla risposta DNS.
   - Associa il tuo dominio personalizzato all'indirizzo IP pubblico portatile del gateway Ingress aggiungendo l'indirizzo IP come record. Per trovare l'indirizzo IP pubblico del gateway Ingress, esegui `nslookup <ingress_subdomain>`.
3. Acquista un certificato TLS jolly ufficiale per il tuo dominio personalizzato. Se vuoi acquistare più certificati TLS, assicurati che il [CN ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://support.dnsimple.com/articles/what-is-common-name/) sia diverso per ciascun certificato.
4. Crea un segreto Kubernetes per la tua chiave e il tuo certificato.
   1. Codifica il certificato e la chiave in base64 e salva il valore con codifica base64 in un nuovo file.
      ```
      openssl base64 -in tls.key -out tls.key.base64
      ```
      {: pre}

   2. Visualizza il valore con codifica base64 per il certificato e la chiave.
      ```
      cat tls.key.base64
      ```
      {: pre}

   3. Crea un file YAML segreto utilizzando il certificato e la chiave.
      ```
      apiVersion: v1
      kind: Secret
      metadata:
        name: mydomain-ssl
      type: Opaque
      data:
        tls.crt: <client_certificate>
        tls.key: <client_key>
      ```
      {: codeblock}

   4. Crea il certificato nel tuo cluster.
      ```
      kubectl create -f secret.yaml
      ```
      {: pre}

5. Apri la risorsa Ingress `iks-knative-ingress` nello spazio dei nomi `istio-system` del tuo cluster per iniziare a modificarla.
   ```
   kubectl edit ingress iks-knative-ingress -n istio-system
   ```
   {: pre}

6. Modifica le regole di instradamento predefinite per il tuo Ingress.
   - Aggiungi il tuo dominio jolly personalizzato alla sezione `spec.rules.host` in modo che tutto il traffico di rete in entrata proveniente dal tuo dominio personalizzato e da qualsiasi dominio secondario venga instradato all'`istio-ingressGateway`.
   - Configura tutti gli host del tuo dominio jolly personalizzato per utilizzare il segreto TLS creato in precedenza nella sezione `spec.tls.hosts`.

   Esempio di Ingress:
   ```
   apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
     name: iks-knative-ingress
     namespace: istio-system
   spec:
     rules:
     - host: '*.mydomain'
       http:
         paths:
         - backend:
             serviceName: istio-ingressgateway
             servicePort: 80
           path: /
     tls:
     - hosts:
       - '*.mydomain'
       secretName: mydomain-ssl
   ```
   {: codeblock}

   Le sezioni `spec.rules.host` e `spec.tls.hosts` sono degli elenchi e possono includere più certificati TLS e domini personalizzati.
   {: tip}

7. Modifica la mappa di configurazione `config-domain` Knative per utilizzare il tuo dominio personalizzato per assegnare nomi host ai nuovi servizi Knative.
   1. Apri la mappa di configurazione `config-domain`per iniziare a modificarla.
      ```
      kubectl edit configmap config-domain -n knative-serving
      ```
      {: pre}

   2. Specifica il tuo dominio personalizzato nella sezione `data` della tua mappa di configurazione e rimuovi il dominio predefinito impostato per il tuo cluster.
      - **Esempio di assegnazione di un nome host dal tuo dominio personalizzato per tutti i servizi Knative**:
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        Aggiungendo `""` al tuo dominio personalizzato, a tutti i servizi Knative che hai creato viene assegnato un nome host dal tuo dominio personalizzato.  

      - **Esempio di assegnazione di un nome host dal tuo dominio personalizzato per la selezione di servizi Knative**:
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: |
            selector:
              app: sample
          mycluster.us-south.containers.appdomain.cloud: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        Per assegnare un nome host dal tuo dominio personalizzato per la selezione di soli servizi Knative, aggiungi una chiave e un valore etichetta `data.selector` alla tua mappa di configurazione. In questo esempio, a tutti i servizi con etichetta `app: sample` viene assegnato un nome host dal tuo dominio personalizzato. Assicurati di avere anche un nome dominio da assegnare a tutte le altre applicazioni che non hanno l'etichetta `app: sample`. In questo esempio, viene utilizzato il dominio predefinito fornito da IBM `mycluster.us-south.containers.appdomain.cloud`.
    3. Salva le modifiche.

Una volta configurate tutte le tue regole di instradamento Ingress e le mappe di configurazione Knative, puoi creare servizi Knative con il tuo dominio personalizzato e il certificato TLS.

## Accesso a un servizio Knative da un altro servizio Knative
{: #knative-access-service}

Puoi accedere al tuo servizio Knative da un altro servizio Knative utilizzando una chiamata API REST all'URL assegnato al tuo servizio Knative.
{: shortdesc}

1. Elenca tutti i servizi Knative del tuo cluster.
   ```
   kubectl get ksvc --all-namespaces
   ```
   {: pre}

2. Richiama il **DOMAIN** assegnato al tuo servizio Knative.
   ```
   kubect get ksvc/<knative_service_name>
   ```
   {: pre}

   Output di esempio:
   ```
   NAME        DOMAIN                                                            LATESTCREATED     LATESTREADY       READY   REASON
   myservice   myservice.default.mycluster.us-south.containers.appdomain.cloud   myservice-rjmwt   myservice-rjmwt   True
   ```
   {: screen}

3. Utilizza il nome dominio per implementare una chiamata API REST per accedere al tuo servizio Knative. Questa API REST deve appartenere all'applicazione per cui crei un servizio Knative. Se al servizio Knative a cui vuoi accedere è assegnato un URL locale in formato `<service_name>.<namespace>.svc.cluster.local`, Knative mantiene la richiesta API REST all'interno della rete interna al cluster.

   Frammento di codice di esempio in Go:
   ```go
   resp, err := http.Get("<knative_service_domain_name>")
   if err != nil {
       fmt.Fprintf(os.Stderr, "Error: %s\n", err)
   } else if resp.Body != nil {
       body, _ := ioutil.ReadAll(resp.Body)
       fmt.Printf("Response: %s\n", string(body))
   }
   ```
   {: codeblock}

## Impostazioni comuni del servizio Knative
{: #knative-service-settings}

Esamina le impostazioni comuni del servizio Knative che potresti trovare utili quando sviluppi la tua applicazione senza server.
{: shortdesc}

- [Impostazione del numero minimo e massimo di pod](#knative-min-max-pods)
- [Indicazione del numero massimo di richieste per pod](#max-request-per-pod)
- [Creazione di applicazioni senza server solo private](#knative-private-only)
- [Come forzare il servizio Knative a rieseguire il pull dell'immagine di un contenitore](#knative-repull-image)

### Impostazione del numero minimo e massimo di pod
{: #knative-min-max-pods}

Puoi specificare il numero minimo e massimo di pod che vuoi eseguire per le tue applicazioni attraverso un'annotazione. Ad esempio, se non vuoi che Knative riduca il numero delle istanze della tua applicazione a zero, imposta il numero minimo di pod su 1.
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: ...
spec:
  runLatest:
    configuration:
      revisionTemplate:
        metadata:
          annotations:
            autoscaling.knative.dev/minScale: "1"
            autoscaling.knative.dev/maxScale: "100"
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>Descrizione dei componenti del file YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
</thead>
<tbody>
<tr>
<td><code>autoscaling.knative.dev/minScale</code></td>
<td>Immetti il numero minimo di pod che vuoi eseguire nel tuo cluster. Knative non può ridurre i pod della tua applicazione oltre il numero che hai impostato, anche se la tua applicazione non riceve nessun traffico di rete. Il numero predefinito di pod è uguale a zero.  </td>
</tr>
<tr>
<td><code>autoscaling.knative.dev/maxScale</code></td>
<td>Immetti il numero massimo di pod che vuoi eseguire nel tuo cluster. Knative non può aumentare i pod della tua applicazione oltre il numero che hai impostato, anche se hai più richieste di quante ne possono gestire le istanze correnti della tua applicazione.</td>
</tr>
</tbody>
</table>

### Indicazione del numero massimo di richieste per pod
{: #max-request-per-pod}

Puoi specificare il numero massimo di richieste che un'istanza dell'applicazione può ricevere ed elaborare prima che Knative valuti la possibilità di ampliare le istanze dell'applicazione. Ad esempio, se imposti il numero massimo di richieste su 1, l'istanza della tua applicazione può ricevere una sola richiesta alla volta. Se arriva una seconda richiesta prima del completamento dell'elaborazione della prima, Knative amplia un'altra istanza.

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image: myimage
          containerConcurrency: 1
....
```
{: codeblock}

<table>
<caption>Descrizione dei componenti del file YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
</thead>
<tbody>
<tr>
<td><code>containerConcurrency</code></td>
<td>Immetti il numero massimo di richieste che un'istanza dell'applicazione può ricevere alla volta prima che Knative valuti la possibilità di ampliare le istanze dell'applicazione.</td>
</tr>
</tbody>
</table>

### Creazione di applicazioni senza server solo private
{: #knative-private-only}

Per impostazione predefinita, a ogni servizio Knative viene assegnato un percorso pubblico dal dominio secondario Istio Ingress e un percorso privato in formato `<service_name>.<namespace>.cluster.local`. Puoi utilizzare il percorso pubblico per accedere alla tua applicazione dalla rete pubblica. Se vuoi mantenere il tuo servizio privato, puoi aggiungere l'etichetta `serving.knative.dev/visibility` al tuo servizio Knative. Questa etichetta indica a Knative di assegnare un nome host privato solo al tuo servizio.
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
  labels:
    serving.knative.dev/visibility: cluster-local
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>Descrizione dei componenti del file YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
</thead>
<tbody>
<tr>
<td><code>serving.knative.dev/visibility</code></td>
  <td>Se aggiungi l'etichetta <code>serving.knative.dev/visibility: cluster-local</code>, al tuo servizio viene assegnato solo un percorso privato informato <code>&lt;service_name&gt;.&lt;namespace&gt;.cluster.local</code>. Puoi utilizzare il nome host privato per accedere al tuo servizio dal cluster, ma non puoi accedere al tuo servizio dalla rete pubblica.  </td>
</tr>
</tbody>
</table>

### Come forzare il servizio Knative a rieseguire il pull dell'immagine di un contenitore
{: #knative-repull-image}

L'implementazione corrente di Knative non fornisce un modo standard per forzare il tuo componente `Serving` Knative a rieseguire il pull di un'immagine del contenitore. Per rieseguire il pull di un'immagine dal tuo registro, scegli tra le seguenti opzioni:

- **Modifica il `revisionTemplate` del servizio Knative**: il `revisionTemplate` di un servizio Knative viene utilizzato per creare una revisione del tuo servizio Knative. Se modifichi questo modello di revisione e, ad esempio, aggiungi l'annotazione `repullFlag`, Knative deve creare una nuova revisione della tua applicazione. Nell'ambito della creazione della revisione, Knative deve cercare aggiornamenti delle immagini del contenitore. Quando imposti `imagePullPolicy: Always`, Knative non può utilizzare la cache di immagini nel cluster, ma deve invece eseguire il pull dell'immagine dal registro del tuo contenitore.
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: <service_name>
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           metadata:
             annotations:
               repullFlag: 123
           spec:
             container:
               image: <image_name>
               imagePullPolicy: Always
    ```
    {: codeblock}

    Devi modificare il valore `repullFlag` ogni volta che vuoi creare una nuova revisione del servizio che esegue il pull della versione dell'immagine più recente dal registro del tuo contenitore. Assicurati di utilizzare un valore univoco per ogni revisione, al fine di evitare che Knative utilizzi una versione obsoleta dell'immagine, a causa di due configurazioni identiche del servizio Knative.  
    {: note}

- **Utilizza delle tag per creare immagini contenitore univoche**: puoi utilizzare tag univoche per ogni immagine del contenitore creata e fare riferimento a questa immagine nella configurazione `container.image` del tuo servizio Knative. Nel seguente esempio, come tag di immagine viene utilizzato `v1`. Per forzare Knative a eseguire il pull di una nuova immagine dal registro del tuo contenitore, devi modificare la tag dell'immagine. Ad esempio, utilizza `v2` come nuova tag dell'immagine.
  ```
  apiVersion: serving.knative.dev/v1alpha1
  kind: Service
  metadata:
    name: <service_name>
  spec:
    runLatest:
      configuration:
          spec:
            container:
              image: myapp:v1
              imagePullPolicy: Always
    ```
    {: codeblock}


## Link correlati  
{: #knative-related-links}

- Prova questo [workshop di Knative ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/IBM/knative101/tree/master/workshop) per distribuire la tua prima applicazione Fibonacci `Node.js` al tuo cluster.
  - Esplora come utilizzare la primitiva `Build` di Knative per creare un'immagine da un Dockerfile in GitHub ed eseguire automaticamente il push dell'immagine al tuo spazio dei nomi in {{site.data.keyword.registrylong_notm}}.  
  - Impara come puoi configurare l'instradamento per il traffico di rete dal dominio secondario Ingress fornito da IBM al gateway Ingress Istio fornito da Knative.
  - Distribuisci una nuova versione della tua applicazione e utilizza Istio per controllare la quantità di traffico instradata a ciascuna versione dell'applicazione.
- Esplora gli esempi [`Eventing` di Knative ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/knative/docs/tree/master/eventing/samples).
- Scopri di più su Knative con la [documentazione di Knative ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/knative/docs).
