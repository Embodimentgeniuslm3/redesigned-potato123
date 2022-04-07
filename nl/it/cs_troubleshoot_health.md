---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Risoluzione dei problemi di registrazione e di monitoraggio
{: #cs_troubleshoot_health}

Mentre utilizzi {{site.data.keyword.containerlong}}, tieni presente queste tecniche per la risoluzione dei problemi con la registrazione e il monitoraggio.
{: shortdesc}

Se hai un problema più generale, prova a [eseguire il debug del cluster](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}

## I log non vengono visualizzati
{: #cs_no_logs}

{: tsSymptoms}
Quando accedi al dashboard Kibana, i tuoi log non vengono visualizzati.

{: tsResolve}
Controlla i seguenti motivi sul perché i tuoi log del cluster non vengono visualizzati e la procedura di risoluzione del problema corrispondente:

<table>
<caption>Risoluzione dei problemi con i log che non vengono visualizzati</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Perché sta succedendo</th>
      <th>Come porvi rimedio</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>Non è impostata alcuna configurazione di registrazione.</td>
    <td>Per poter inviare i log, devi creare una configurazione di registrazione. Per eseguire tale operazione, vedi <a href="/docs/containers?topic=containers-health#logging">Configurazione della registrazione cluster</a>.</td>
  </tr>
  <tr>
    <td>Il cluster non è in uno stato <code>Normale</code>.</td>
    <td>Per controllare lo stato del tuo cluster, vedi <a href="/docs/containers?topic=containers-cs_troubleshoot#debug_clusters">Debug dei cluster</a>.</td>
  </tr>
  <tr>
    <td>La quota di archiviazione dei log è stata raggiunta.</td>
    <td>Per aumentare i limiti della tua archiviazione di log, consulta la <a href="/docs/services/CloudLogAnalysis/troubleshooting?topic=cloudloganalysis-error_msgs">documentazione {{site.data.keyword.loganalysislong_notm}}</a>.</td>
  </tr>
  <tr>
    <td>Se hai specificato uno spazio durante la creazione del cluster, il proprietario dell'account non dispone delle autorizzazioni di Gestore, Sviluppatore o Revisore in quello spazio.</td>
      <td>Per modificare le autorizzazioni di accesso per il proprietario dell'account:
      <ol><li>Per appurare chi è il proprietario dell'account per il cluster, esegui <code>ibmcloud ks api-key-info --cluster &lt;cluster_name_or_ID&gt;</code>.</li>
      <li>Per concedere a tale proprietario dell'account le autorizzazioni di accesso allo spazio di gestore, sviluppatore o revisore {{site.data.keyword.containerlong_notm}}, consulta <a href="/docs/containers?topic=containers-users">Gestione dell'accesso al cluster</a>.</li>
      <li>Per aggiornare il token di registrazione dopo che sono state modificate le autorizzazioni, esegui <code>ibmcloud ks logging-config-refresh --cluster &lt;cluster_name_or_ID&gt;</code>.</li></ol></td>
    </tr>
    <tr>
      <td>Hai una configurazione di registrazione per la tua applicazione con un collegamento simbolico nel tuo percorso dell'applicazione.</td>
      <td><p>Per poter inviare i log, devi utilizzare un percorso assoluto nella tua configurazione della registrazione altrimenti i log non potranno essere letti. Se il tuo percorso viene montato nel tuo nodo di lavoro, potrebbe creare un collegamento simbolico.</p> <p>Esempio: se il percorso specificato è <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> ma vanno in <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>, i log non potranno essere letti.</p></td>
    </tr>
  </tbody>
</table>

Per verificare le modifiche che hai apportato durante la risoluzione dei problemi, puoi distribuire *Noisy*, un pod di esempio che produce diversi eventi di log, a un nodo di lavoro nel tuo cluster.

Prima di iniziare: [accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster:](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Crea il file di configurazione `deploy-noisy.yaml`.
    ```
    apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
      - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
      ```
      {: codeblock}

2. Esegui il file di configurazione nel contesto del cluster.
    ```
    kubectl apply -f noisy.yaml
    ```
    {:pre}

3. Dopo alcuni minuti, puoi visualizzare i tuoi log nel dashboard Kibana. Per accedere al dashboard Kibana, vai a uno dei seguenti URL e seleziona l'account {{site.data.keyword.Bluemix_notm}} in cui hai creato il cluster. Se hai specificato uno spazio durante la creazione del cluster, vai a tale spazio.
    - Stati Uniti Sud e Stati Uniti Est: `https://logging.ng.bluemix.net`
    - Regno Unito Sud: `https://logging.eu-gb.bluemix.net`
    - Europa centrale: `https://logging.eu-fra.bluemix.net`
    - Asia Pacifico Sud: `https://logging.au-syd.bluemix.net`

<br />


## Il dashboard Kubernetes non visualizza i grafici di utilizzo
{: #cs_dashboard_graphs}

{: tsSymptoms}
Quando accedi al dashboard Kubernetes, i grafici di utilizzo non vengono visualizzati.

{: tsCauses}
A volte, dopo un aggiornamento del cluster o un riavvio del nodo di lavoro, il pod `kube-dashboard` non viene aggiornato.

{: tsResolve}
Elimina il pod `kube-dashboard` per forzare un riavvio. Il pod viene ricreato con le politiche RBAC in modo da accedere a `heapster` per le informazioni di utilizzo.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## La quota di log è troppo bassa
{: #quota}

{: tsSymptoms}
Imposti una configurazione di registrazione nel tuo cluster per inoltrare i log a {{site.data.keyword.loganalysisfull}}. Quando visualizzi i log, vedi un messaggio di errore simile al seguente:

```
You have reached the daily quota that is allocated to the Bluemix space {Space GUID} for the IBM® Cloud Log Analysis instance {Instance GUID}. Your current daily allotment is XXX for Log Search storage, which is retained for a period of 3 days, during which it can be searched for in Kibana. This does not affect your log retention policy in Log Collection storage. To upgrade your plan so that you can store more data in Log Search storage per day, upgrade the Log Analysis service plan for this space. For more information about service plans and how to upgrade your plan, see Plans.
```
{: screen}

{: tsResolve}
Controlla i seguenti motivi per cui stai raggiungendo la tua quota di log e i corrispondenti passi di risoluzione del problema:

<table>
<caption>Risoluzione dei problemi di archiviazione dei log</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Perché sta succedendo</th>
      <th>Come porvi rimedio</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>Uno o più pod producono un numero elevato di log.</td>
    <td>Puoi liberare spazio di archiviazione di log evitando che i log provenienti da specifici pod vengano inoltrati. Crea un [filtro di registrazione](/docs/containers?topic=containers-health#filter-logs) per questi pod.</td>
  </tr>
  <tr>
    <td>Superi l'assegnazione giornaliera di 500 MB per l'archiviazione di log per il piano Lite.</td>
    <td>Innanzitutto, [calcola la quota di ricerca e l'utilizzo giornaliero](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota) del tuo dominio di log. Puoi quindi aumentare la tua quota di archiviazione di log [eseguendo l'upgrade del tuo piano del servizio {{site.data.keyword.loganalysisshort_notm}}](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan).</td>
  </tr>
  <tr>
    <td>Stai superando la quota di archiviazione di log per il tuo piano a pagamento corrente.</td>
    <td>Innanzitutto, [calcola la quota di ricerca e l'utilizzo giornaliero](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota) del tuo dominio di log. Puoi quindi aumentare la tua quota di archiviazione di log [eseguendo l'upgrade del tuo piano del servizio {{site.data.keyword.loganalysisshort_notm}}](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan).</td>
  </tr>
  </tbody>
</table>

<br />


## Le righe di log sono troppo lunghe
{: #long_lines}

{: tsSymptoms}
Imposti una configurazione di registrazione nel tuo cluster per inoltrare i log a {{site.data.keyword.loganalysisfull_notm}}. Quando visualizzi i log, vedi un lungo messaggio di log. Inoltre, in Kibana, potresti riuscire a vedere solo gli ultimi 600 - 700 caratteri del messaggio di log.

{: tsCauses}
Un lungo messaggio di log potrebbe essere troncato a causa della sua lunghezza prima di essere raccolto da Fluentd e, pertanto, il log potrebbe non essere analizzato correttamente da Fluentd prima di essere inoltrato a {{site.data.keyword.loganalysisshort_notm}}.

{: tsResolve}
Per limitare la lunghezza di riga, puoi configurare il tuo logger per avere una lunghezza massima per la `stack_trace` in ciascun log. Ad esempio, se stai utilizzando Log4j per il tuo logger, puoi utilizzare un [`EnhancedPatternLayout` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/EnhancedPatternLayout.html) per limitare la `stack_trace` a 15KB.

## Come ottenere aiuto e supporto
{: #health_getting_help}

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

