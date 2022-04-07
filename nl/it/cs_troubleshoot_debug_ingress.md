---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, nginx, ingress controller

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Debug di Ingress
{: #cs_troubleshoot_debug_ingress}

Mentre utilizzi {{site.data.keyword.containerlong}}, valuta le seguenti tecniche per procedure generiche sulla risoluzione dei problemi e sull'esecuzione del debug di Ingress.
{: shortdesc}

Hai esposto pubblicamente la tua applicazione creando una risorsa Ingress per la tua applicazione nel tuo cluster. Tuttavia, quando provi a connettere la tua applicazione tramite il dominio secondario o l'indirizzo IP pubblico di ALB, la connessione non riesce o va in timeout. La procedura nelle successive sezioni può aiutarti ad eseguire il debug della tua impostazione Ingress.

Assicurati di definire un host in una sola risorsa Ingress. Se un host è definito in più risorse Ingress, l'ALB potrebbe non inoltrare correttamente il traffico e potresti riscontrare degli errori.
{: tip}

Prima di iniziare, assicurati di disporre delle seguenti [politiche di accesso {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform):
  - Ruolo della piattaforma **Editor** o **Amministratore** per il cluster
  - Ruolo del servizio **Scrittore** o **Gestore**

## Passo 1: Esegui i test Ingress nel {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool

Mentre risolvi i problemi, puoi utilizzare il {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool per eseguire dei test Ingress e raccogliere informazioni Ingress pertinenti dal tuo cluster. Per utilizzare lo strumento di debug, installa il [grafico Helm `ibmcloud-iks-debug` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/kubernetes/helm/iks-charts/ibmcloud-iks-debug):
{: shortdesc}


1. [Configura Helm nel tuo cluster, crea un account di servizio per Tiller e aggiungi il repository `ibm` alla tua istanza Helm](/docs/containers?topic=containers-helm).

2. Installa il grafico Helm nel tuo cluster.
  ```
  helm install iks-charts/ibmcloud-iks-debug --name debug-tool
  ```
  {: pre}


3. Avvia un server proxy per visualizzare l'interfaccia dello strumento di debug.
  ```
  kubectl proxy --port 8080
  ```
  {: pre}

4. In un browser web, apri l'URL dell'interfaccia dello strumento di debug: http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page

5. Seleziona il gruppo di test **ingress**. Alcuni test controllano la presenza di potenziali avvertenze, errori o problemi e alcuni testi si limitano a raccogliere informazioni a cui puoi fare riferimento mentre risolvi i problemi. Per ulteriori informazioni sulla funzione di ciascun test, fai clic sull'icona delle informazioni accanto al nome del test.

6. Fai clic su **Run**.

7. Controlla i risultati di ciascun test.
  * Se qualche test non riesce, fai clic sull'icona di informazioni accanto al nome del test nella colonna di sinistra per informazioni su come risolvere il problema.
  * Puoi anche utilizzare i risultati dei test che si limitano a raccogliere informazioni mentre esegui il debug del tuo servizio Ingress nelle successive sezioni:

## Passo 2: controlla i messaggi di errore nella distribuzione Ingress e nei log dei pod ALB
{: #errors}

Inizia controllando l'eventuale presenza di messaggi di errore negli eventi di distribuzione di risorse Ingress o nei log dei pod ALB. Questi messaggi di errore ti possono aiutare a trovare le cause principali dei malfunzionamenti e di eseguire un ulteriore debug della tua impostazione di Ingress nelle sezioni successive.
{: shortdesc}

1. Controlla la tua distribuzione delle risorse Ingress e cerca messaggi di avvertenza o errore.
    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    Nella sezione **Events** dell'output, potresti vedere dei messaggi di avvertenza relativi a valori non validi nella tua risorsa Ingress o in specifiche annotazioni che hai utilizzato. Controlla la [documentazione relativa alla configurazione delle risorse Ingress](/docs/containers?topic=containers-ingress#public_inside_4) oppure la [documentazione relativa alle annotazioni](/docs/containers?topic=containers-ingress_annotation).

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster.us-south.containers.appdomain.cloud
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}
{: #check_pods}
2. Controlla lo stato dei tuoi pod ALB.
    1. Ottieni i pod ALB che sono in esecuzione nel tuo cluster.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. Assicurati che tutti i pod siano in esecuzione controllando la colonna **STATUS**.

    3. Se un pod non è in esecuzione (`Running`), puoi disabilitarlo e riabilitarlo. Nei seguenti comandi, sostituisci `<ALB_ID>` con l'ID dell'ALB del pod. Ad esempio, se il pod che non è in esecuzione ha il nome `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1-5d6d86fbbc-kxj6z`, l'ID ALB è `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1`.
        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --disable
        ```
        {: pre}

        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --enable
        ```
        {: pre}

3. Verifica i log del tuo ALB.
    1.  Ottieni gli ID dei pod ALB che sono in esecuzione nel tuo cluster.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    3. Ottieni i log per il contenitore `nginx-ingress` su ciascun pod ALB.
        ```
        kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

    4. Ricerca messaggi di errore nei log dell'ALB.

## Passo 3: esegui il ping del dominio secondario ALB e degli indirizzi IP pubblici
{: #ping}

Controlla la disponibilità del tuo dominio secondario Ingress e degli indirizzi IP pubblici di ALB.
{: shortdesc}

1. Ottieni gli indirizzi IP su cui sono in ascolto i tuoi ALB pubblici.
    ```
    ibmcloud ks albs --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Output di esempio per un cluster multizona con nodi di lavoro in `dal10` e `dal13`:

    ```
    ALB ID                                            Enabled   Status     Type      ALB IP          Zone    Build                          ALB VLAN ID
    private-cr24a9f2caf6554648836337d240064935-alb1   false     disabled   private   -               dal13   ingress:411/ingress-auth:315   2294021
    private-cr24a9f2caf6554648836337d240064935-alb2   false     disabled   private   -               dal10   ingress:411/ingress-auth:315   2234947
    public-cr24a9f2caf6554648836337d240064935-alb1    true      enabled    public    169.62.196.238  dal13   ingress:411/ingress-auth:315   2294019
    public-cr24a9f2caf6554648836337d240064935-alb2    true      enabled    public    169.46.52.222   dal10   ingress:411/ingress-auth:315   2234945
    ```
    {: screen}

    * Se un ALB pubblico non ha alcun indirizzo IP, vedi [L'ALB Ingress non viene distribuito in una zona](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit).

2. Controlla l'integrità dei tuoi IP ALB.

    * Per i cluster a zona singola e multizona: esegui il ping dell'indirizzo IP di ciascun ALB pubblico per garantire che ciascun ALB sia in grado di ricevere correttamente i pacchetti. Se stai utilizzando degli ALB privati, puoi eseguire il ping dei loro indirizzi IP solo dalla rete privata.
        ```
        ping <ALB_IP>
        ```
        {: pre}

        * Se la CLI restituisce un timeout e hai un firewall personalizzato che sta proteggendo i tuoi di nodi di lavoro, assicurati di consentire ICMP nel tuo [firewall](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall).
        * Se non c'è alcun firewall che sta bloccando i ping e l'esecuzione dei ping continua comunque fino a una condizione di timeout, [controlla lo stato dei tuoi pod ALB](#check_pods).

    * Solo cluster multizona: puoi utilizzare il controllo dell'integrità MZLB per determinare lo stato dei tuoi IP ALB. Per ulteriori informazioni sull'MZLB, vedi [Programma di bilanciamento del carico multizona (o MZLB, multizone load balancer)](/docs/containers?topic=containers-ingress#planning). Il controllo dell'integrità MZLB è disponibile solo per i cluster che hanno il nuovo dominio secondario Ingress nel formato `<cluster_name>.<region_or_zone>.containers.appdomain.cloud`. Se il tuo cluster utilizza ancora il formato meno recente di `<cluster_name>.<region>.containers.mybluemix.net`, [converti il tuo cluster a zona singola in multizona](/docs/containers?topic=containers-add_workers#add_zone). Al tuo cluster viene assegnato un dominio secondario con il nuovo formato ma può anche continuare a utilizzare il formato di dominio secondario meno recente. In alternativa, puoi ordinare un nuovo cluster a cui viene automaticamente assegnato il nuovo formato di dominio secondario.

    Il seguente comando HTTP cURL utilizza l'host `albhealth`, che è configurato da {{site.data.keyword.containerlong_notm}} per restituire lo stato `healthy` o `unhealthy` per un IP ALB.
        ```
        curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
        ```
        {: pre}

        Output di esempio:
        ```
        healthy
        ```
        {: screen}
        Se uno o più degli IP restituiscono `unhealthy`, [controlla lo stato dei tuoi pod ALB](#check_pods).

3. Ottieni il dominio secondario Ingress fornito da IBM.
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress
    ```
    {: pre}

    Output di esempio:
    ```
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    ```
    {: screen}

4. Assicurati che gli IP per ciascun ALB pubblico che hai ottenuto nel passo 2 di questa sezione siano registrati presso il dominio secondario Ingresso fornito da IBM del tuo cluster. Ad esempio, in un cluster multizona, l'IP ALB pubblico in ciascuna zona dove hai dei nodi di lavoro deve essere registrato sotto lo stesso nome host.

    ```
    kubectl get ingress -o wide
    ```
    {: pre}

    Output di esempio:
    ```
    NAME                HOSTS                                                    ADDRESS                        PORTS     AGE
    myingressresource   mycluster-12345.us-south.containers.appdomain.cloud      169.46.52.222,169.62.196.238   80        1h
    ```
    {: screen}

## Passo 4: controlla le associazioni di dominio e la configurazione della risorsa Ingress
{: #ts_ingress_config}

1. Se utilizzi un dominio personalizzato, verifica di aver utilizzato il tuo provider DNS per associare il dominio personalizzato al dominio secondario fornito da IBM oppure all'indirizzo IP pubblico di ALB. Nota: l'utilizzo di un CNAME è preferito perché IBM fornisce dei controlli dell'integrità automatici sul dominio secondario IBM e rimuove gli eventuali IP malfunzionanti dalla risposta DNS.
    * Dominio secondario fornito da IBM: controlla che il tuo dominio personalizzato sia associato al dominio secondario fornito da IBM del cluster nel record CNAME (Canonical Name).
        ```
        host www.my-domain.com
        ```
        {: pre}

        Output di esempio:
        ```
        www.my-domain.com is an alias for mycluster-12345.us-south.containers.appdomain.cloud
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
        ```
        {: screen}

    * Indirizzo IP pubblico: controlla che il tuo dominio personalizzato sia associato all'indirizzo IP pubblico portatile di ALB nel record A. Gli IP devono corrispondere agli IP ALB pubblici che hai ottenuto nel passo 1 della [sezione precedente](#ping).
        ```
        host www.my-domain.com
        ```
        {: pre}

        Output di esempio:
        ```
        www.my-domain.com has address 169.46.52.222
        www.my-domain.com has address 169.62.196.238
        ```
        {: screen}

2. Controlla i file di configurazione delle risorse Ingress per il tuo cluster.
    ```
    kubectl get ingress -o yaml
    ```
    {: pre}

    1. Assicurati di definire un host in una sola risorsa Ingress. Se un host è definito in più risorse Ingress, l'ALB potrebbe non inoltrare correttamente il traffico e potresti riscontrare degli errori.

    2. Controlla che il dominio secondario e il certificato TLS siano corretti. Per trovare il dominio secondario Ingress e il certificato TLS forniti da IBM, esegui `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`.

    3.  Assicurati che la tua applicazione sia in ascolto sullo stesso percorso configurato nella sezione **percorso** del tuo Ingress. Se la tua applicazione è configurata per essere in ascolto sul percorso root, utilizza `/` come percorso. Se il traffico in entrata a questo percorso deve essere instradato a un percorso differente su cui la tua applicazione è in ascolto, utilizza l'annotazione di [percorsi di riscrittura](/docs/containers?topic=containers-ingress_annotation#rewrite-path).

    4. Modifica il tuo YAML di configurazione delle risorse come necessario. Quando chiudi l'editor, le tue modifiche vengono salvate e applicate automaticamente.
        ```
        kubectl edit ingress <myingressresource>
        ```
        {: pre}

## Rimozione di un ALB dal DNS per il debug
{: #one_alb}

Se non puoi accedere alla tua applicazione tramite uno specifico IP ALB, puoi rimuovere temporaneamente l'ALB dalla produzione disabilitandone la registrazione DNS. Puoi quindi utilizzare l'indirizzo IP dell'ALB per eseguire i test di debug su detto ALB.

Supponiamo ad esempio che hai un cluster multizona in 2 zone e 2 ALB pubblici hanno gli indirizzi IP `169.46.52.222` e `169.62.196.238`. Anche se il controllo dell'integrità sta restituendo uno stato di integro per l'ALB della seconda zona, la tua applicazione non è raggiungibile direttamente per suo tramite. Decidi di rimuovere l'indirizzo IP di tale ALB, `169.62.196.238`, dalla produzione per l'esecuzione del debug. l'IP ALB della prima zona, `169.46.52.222`, viene registrato presso il tuo dominio e continua a instradare il traffico mentre tu esegui il debug dell'ALB della seconda zona.

1. Ottieni il nome dell'ALB con l'indirizzo IP irraggiungibile.
    ```
    ibmcloud ks albs --cluster <cluster_name> | grep <ALB_IP>
    ```
    {: pre}

    Ad esempio, l'IP irraggiungibile `169.62.196.238` appartiene all'ALB `public-cr24a9f2caf6554648836337d240064935-alb1`:
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
    public-cr24a9f2caf6554648836337d240064935-alb1    false     disabled   private   169.62.196.238   dal13   ingress:411/ingress-auth:315   2294021
    ```
    {: screen}

2. Utilizzando il nome ALB dal passo precedente, ottieni i nomi dei pod ALB. Il seguente comando utilizza il nome ALB di esempio dal passo precedente:
    ```
    kubectl get pods -n kube-system | grep public-cr24a9f2caf6554648836337d240064935-alb1
    ```
    {: pre}

    Output di esempio:
    ```
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq   2/2       Running   0          24m
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-trqxc   2/2       Running   0          24m
    ```
    {: screen}

3. Disabilita il controllo dell'integrità che viene eseguito per tutti i pod ALB. Ripeti questa procedura per ciascun pod ALB che hai ottenuto nel passo precedente. I comandi e l'output di esempio in questi passi utilizzano il primo pod, `public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq`.
    1. Accedi al pod ALB e controlla la riga `server_name` nel file di configurazione NGINX.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        Output di esempio che conferma che il pod ALB è configurato con il corretto nome host di controllo dell'integrità, `albhealth.<domain>`:
        ```
        server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud;
        ```
        {: screen}

    2. Per rimuovere l'IP disabilitando il controllo dell'integrità, inserisci `#` davanti al `server_name`. Quando l'host virtuale `albhealth.mycluster-12345.us-south.containers.appdomain.cloud` è disabilitato per l'ALB, il controllo dell'integrità automatizzato rimuove automaticamente l'IP dalla risposta DNS.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- sed -i -e 's*server_name*#server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

    3. Verifica che la modifica sia stata applicata.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        Output di esempio:
        ```
        #server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud
        ```
        {: screen}

    4. Per rimuovere l'IP dalla registrazione DNS, ricarica la configurazione NGINX.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- nginx -s reload
        ```
        {: pre}

    5. Ripeti questa procedura per ciascun pod ALB.

4. Ora, quando provi ad eseguire il cURL dell'host `albhealth` per controllare l'integrità dell'IP ALB, il controllo non riesce.
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

    Output:
    ```
    <html>
        <head>
            <title>404 Non trovato</title>
        </head>
        <body bgcolor="white"><center>
            <h1>404 Non trovato</h1>
        </body>
    </html>
    ```
    {: screen}

5. Verifica che l'indirizzo IP ALB sia rimosso dalla registrazione DNS per il tuo dominio controllando il server Cloudflare. Nota: l'aggiornamento della registrazione DNS potrebbe impiegare alcuni minuti.
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    Output di esempio che conferma che solo l'IP ALB integro, `169.46.52.222`, rimane nella registrazione DNS e che l'IP ALB non integro, `169.62.196.238`, è stato rimosso:
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    ```
    {: screen}

6. Ora che l'IP ALB è stato rimosso dalla produzione, puoi servirtene per eseguire i test di debug sulla tua applicazione. Per eseguire un test delle comunicazioni alla tua applicazione tramite questo IP, puoi eseguire il seguente comando cURL, sostituendo i valori di esempio con i tuoi valori:
    ```
    curl -X GET --resolve mycluster-12345.us-south.containers.appdomain.cloud:443:169.62.196.238 https://mycluster-12345.us-south.containers.appdomain.cloud/
    ```
    {: pre}

    * Se tutto è configurato correttamente, ottieni la risposta prevista dalla tua applicazione.
    * Se in risposta ottieni un errore, ci potrebbe essere un errore nella tua applicazione oppure in una configurazione che si applica solo a questo specifico ALB. Controlla il tuo codice dell'applicazione, i tuoi [file di configurazione delle risorse Ingress](/docs/containers?topic=containers-ingress#public_inside_4) o qualsiasi altra configurazione che hai applicato solo a questo ALB.

7. Dopo aver terminato il debug, ripristina il controllo dell'integrità sui pod ALB. Ripeti questa procedura per ciascun pod ALB.
  1. Accedi al pod ALB e rimuovi `#` dal `server_name`.
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- sed -i -e 's*#server_name*server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
    ```
    {: pre}

  2. Ricarica la configurazione NGINX in modo che sia applicato il ripristino del controllo dell'integrità.
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- nginx -s reload
    ```
    {: pre}

9. Ora, quando esegui il cURL dell'host `albhealth` per controllare l'integrità dell'IP ALB, il controllo restituisce `healthy`.
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

10. Verifica che l'indirizzo IP ALB sia stato ripristinato nella registrazione DNS per il tuo dominio controllando il server Cloudflare. Nota: l'aggiornamento della registrazione DNS potrebbe impiegare alcuni minuti.
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    Output di esempio:
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
    ```
    {: screen}

<br />


## Come ottenere aiuto e supporto
{: #ingress_getting_help}

Stai ancora avendo problemi con il tuo cluster?
{: shortdesc}

-  Nel terminale, ricevi una notifica quando sono disponibili degli aggiornamenti ai plugin e alla CLI `ibmcloud`. Assicurati di mantenere la tua CLI aggiornata in modo che tu possa utilizzare tutti i comandi e gli indicatori disponibili.
-   Per vedere se {{site.data.keyword.Bluemix_notm}} è disponibile, [controlla la pagina sugli stati {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/status?selected=status).
-   Pubblica una domanda in [{{site.data.keyword.containerlong_notm}} Slack ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-container-service.slack.com).
    Se non stai utilizzando un ID IBM per il tuo account {{site.data.keyword.Bluemix_notm}}, [richiedi un invito](https://bxcs-slack-invite.mybluemix.net/) a questo Slack.
    {: tip}
-   Rivedi i forum per controllare se altri utenti hanno riscontrato gli stessi problemi. Quando utilizzi i forum per fare una domanda, contrassegna con una tag la tua domanda in modo che sia visualizzabile dai team di sviluppo {{site.data.keyword.Bluemix_notm}}.
    -   Se hai domande tecniche sullo sviluppo o la distribuzione di cluster o applicazioni con
{{site.data.keyword.containerlong_notm}}, inserisci la tua domanda in
[Stack Overflow ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) e contrassegnala con le tag `ibm-cloud`, `kubernetes` e `containers`.
    -   Per domande sul servizio e istruzioni introduttive, utilizza il forum
[IBM Developer Answers ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Includi le tag `ibm-cloud`
e `containers`.
    Consulta [Come ottenere supporto](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) per ulteriori dettagli sull'utilizzo dei forum.
-   Contatta il supporto IBM aprendo un caso. Per informazioni su come aprire un caso di supporto IBM o sui livelli di supporto e sulla gravità dei casi, consulta [Come contattare il supporto](/docs/get-support?topic=get-support-getting-customer-support).
Quando riporti un problema, includi il tuo ID del cluster. Per ottenere il tuo ID del cluster, esegui `ibmcloud ks clusters`. Puoi anche utilizzare il [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) per raccogliere ed esportare informazioni pertinenti dal tuo cluster da condividere con il supporto IBM.
{: tip}

