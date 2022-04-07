---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-05"

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





# Creazione di un token {{site.data.keyword.registryshort_notm}} per un registro di immagini di {{site.data.keyword.Bluemix_dedicated_notm}}
{: #cs_dedicated_tokens}

Crea un token senza scadenza per un registro di immagini che hai utilizzato per i gruppi singoli o scalabili con i cluster in {{site.data.keyword.containerlong}}.
{:shortdesc}

1.  Richiedi un token di registro permanente per la sessione corrente. Questo token concede l'accesso alle immagini nello spazio dei nomi corrente.
    ```
    ibmcloud cr token-add --description "<description>" --non-expiring -q
    ```
    {: pre}

2.  Verifica il segreto Kubernetes.

    ```
    kubectl describe secrets
    ```
    {: pre}

    Puoi utilizzare questo segreto per lavorare con {{site.data.keyword.containerlong}}.

3.  Crea il segreto Kubernetes per memorizzare le informazioni sul token.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Descrizione dei componenti di questo comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace &lt;kubernetes_namespace&gt;</code></td>
    <td>Obbligatoria. Lo spazio dei nomi Kubernetes del cluster in cui vuoi utilizzare il segreto e a cui distribuire i contenitori. Esegui <code>kubectl get namespaces</code> per elencare tutti gli spazi dei nomi nel tuo cluster.</td>
    </tr>
    <tr>
    <td><code>&lt;secret_name&gt;</code></td>
    <td>Obbligatoria. Il nome che vuoi utilizzare per imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server=&lt;registry_url&gt;</code></td>
    <td>Obbligatoria. L'URL del registro di immagini in cui è configurato il tuo spazio dei nomi: <code>registry.&lt;dedicated_domain&gt;</code></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username=token</code></td>
    <td>Obbligatoria. Non modificare questo valore.</td>
    </tr>
    <tr>
    <td><code>--docker-password=&lt;token_value&gt;</code></td>
    <td>Obbligatoria. Il valore del tuo token di registro che hai richiamato in precedenza.</td>
    </tr>
    <tr>
    <td><code>--docker-email=&lt;docker-email&gt;</code></td>
    <td>Obbligatoria. Se ne hai uno, immetti il tuo indirizzo e-mail Docker. Se non hai uno, immetti un indirizzo e-mail fittizio, come ad esempio a@b.c. Questa e-mail è obbligatoria per creare un segreto Kubernetes, ma non viene utilizzata dopo la creazione.</td>
    </tr>
    </tbody></table>

4.  Crea un a pod che fa riferimento all'imagePullSecret.

    1.  Apri il tuo editor di testo preferito e crea uno script di configurazione del pod denominato mypod.yaml.
    2.  Definisci il pod e l'imagePullSecret che vuoi utilizzare per accedere al registro. Per utilizzare un'immagine privata da uno spazio dei nomi:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<dedicated_domain>/<my_namespace>/<my_image>:<tag>
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>Descrizione dei componenti del file YAML</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
        </thead>
        <tbody>
        <tr>
        <td><code>&lt;pod_name&gt;</code></td>
        <td>Il nome del pod che vuoi creare.</td>
        </tr>
        <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>Il nome del contenitore che vuoi distribuire al tuo cluster.</td>
        </tr>
        <tr>
        <td><code>&lt;my_namespace&gt;</code></td>
        <td>Lo spazio dei nomi in cui è memorizzata la tua immagine. Per elencare gli spazi dei nomi disponibili, esegui `ibmcloud cr namespace-list`.</td>
        </tr>
        <td><code>&lt;my_image&gt;</code></td>
        <td>Il nome dell'immagine che vuoi utilizzare. Per elencare le immagini disponibili in un account {{site.data.keyword.Bluemix_notm}}, esegui <code>ibmcloud cr image-list</code>.</td>
        </tr>
        <tr>
        <td><code>&lt;tag&gt;</code></td>
        <td>La versione dell'immagine che vuoi utilizzare. Se non si specifica una tag, viene utilizzata l'immagine contrassegnata con <strong>latest</strong> per impostazione predefinita.</td>
        </tr>
        <tr>
        <td><code>&lt;secret_name&gt;</code></td>
        <td>Il nome dell'imagePullSecret che hai creato in precedenza.</td>
        </tr>
        </tbody></table>

    3.  Salva le modifiche.

    4.  Crea la distribuzione nel tuo cluster.

          ```
          kubectl apply -f mypod.yaml -n <namespace>
          ```
          {: pre}
