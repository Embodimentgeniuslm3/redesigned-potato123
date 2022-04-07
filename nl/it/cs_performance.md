---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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


# Ottimizzazione delle prestazioni
{: #kernel}

Se hai specifici requisiti di ottimizzazione delle prestazioni, puoi modificare le impostazioni predefinite per alcuni componenti cluster in {{site.data.keyword.containerlong}}.
{: shortdesc}

Se scegli di modificare le impostazioni predefinite, lo fai a tuo proprio rischio. Sei responsabile dell'esecuzione di test sulle eventuali impostazioni modificate e per qualsiasi eventuale interruzione causata dalle impostazioni modificate nel tuo ambiente.
{: important}

## Ottimizzazione delle prestazioni dei nodi di lavoro
{: #worker}

Se hai specifici requisiti di ottimizzazione delle prestazioni, puoi modificare le impostazioni predefinite per i parametri `sysctl` del kernel Linux sui nodi di lavoro.
{: shortdesc}

I nodi di lavoro vengono automaticamente forniti con prestazioni kernel ottimizzate, ma puoi modificare le impostazioni predefinite applicando un oggetto [`DaemonSet` Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) personalizzato al tuo cluster. La serie di daemon modifica le impostazioni per tutti i nodi di lavoro esistenti e applica le impostazioni a qualsiasi nuovo nodo di lavoro di cui viene eseguito il provisioning nel cluster. Nessun pod è interessato.

Devi disporre del [ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Gestore**](/docs/containers?topic=containers-users#platform) per tutti gli spazi dei nomi per eseguire l'`initContainer` privilegiato di esempio. Dopo che i contenitori per le distribuzioni sono stati inizializzati, i privilegi vengono eliminati.
{: note}

1. Salva la seguente serie di daemon in un file denominato `worker-node-kernel-settings.yaml`. Nella sezione `spec.template.spec.initContainers`, aggiungi i campi e i valori per i parametri `sysctl` che vuoi ottimizzare. Questa serie di daemon di esempio modifica il numero massimo predefinito di connessioni consentite nell'ambiente tramite l'impostazione `net.core.somaxconn` e l'intervallo di porte temporanee tramite l'impostazione `net.ipv4.ip_local_port_range`.
    ```
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: kernel-optimization
      namespace: kube-system
      labels:
        tier: management
        app: kernel-optimization
    spec:
      selector:
        matchLabels:
          name: kernel-optimization
      template:
        metadata:
          labels:
            name: kernel-optimization
        spec:
          hostNetwork: true
          hostPID: true
          hostIPC: true
          initContainers:
            - command:
                - sh
                - -c
                - sysctl -w net.ipv4.ip_local_port_range="1025 65535"; sysctl -w net.core.somaxconn=32768;
              image: alpine:3.6
              imagePullPolicy: IfNotPresent
              name: sysctl
              resources: {}
              securityContext:
                privileged: true
                capabilities:
                  add:
                    - NET_ADMIN
              volumeMounts:
                - name: modifysys
                  mountPath: /sys
          containers:
            - resources:
                requests:
                  cpu: 0.01
              image: alpine:3.6
              name: sleepforever
              command: ["/bin/sh", "-c"]
              args:
                - >
                  while true; do
                    sleep 100000;
                  done
          volumes:
            - name: modifysys
              hostPath:
                path: /sys
    ```
    {: codeblock}

2. Applica la serie di daemon ai tuoi nodi di lavoro. Le modifiche vengono applicate immediatamente.
    ```
    kubectl apply -f worker-node-kernel-settings.yaml
    ```
    {: pre}

<br />

Per ripristinare i parametri `sysctl` dei tuoi nodi di lavoro ai valori predefiniti impostati da {{site.data.keyword.containerlong_notm}}:

1. Elimina la serie di daemon. Gli `initContainer` che applicavano le impostazioni personalizzate vengono rimossi.
    ```
    kubectl delete ds kernel-optimization
    ```
    {: pre}

2. [Riavvia tutti i nodi di lavoro nel cluster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot). I nodi di lavoro tornano online con i valori predefiniti applicati.

<br />


## Ottimizzazione delle prestazioni dei pod
{: #pod}

Se hai richieste di carico di lavoro specifiche per le prestazioni, puoi modificare le impostazioni predefinite per i parametri `sysctl` del kernel Linux sugli spazi dei nomi della rete di pod.
{: shortdesc}

Per ottimizzare le impostazioni del kernel per i pod dell'applicazione, puoi inserire una patch [`initContainer ` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) nello YALM `pod/ds/rs/deployment` per ciascuna distribuzione. L'`initContainer` viene aggiunto a ciascuna distribuzione dell'applicazione che si trova nello spazio dei nomi della rete di pod per cui vuoi ottimizzare le prestazioni.

Prima di iniziare, assicurati di disporre del [ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Gestore**](/docs/containers?topic=containers-users#platform) per tutti gli spazi dei nomi per eseguire l'`initContainer` privilegiato di esempio. Dopo che i contenitori per le distribuzioni sono stati inizializzati, i privilegi vengono eliminati.

1. Salva la seguente patch `initContainer` in un file denominato `pod-patch.yaml` e aggiungi i campi e i valori per i parametri `sysctl` che vuoi ottimizzare. Questo `initContainer` di esempio modifica il numero massimo predefinito di connessioni consentite nell'ambiente tramite l'impostazione `net.core.somaxconn` e l'intervallo di porte temporanee tramite l'impostazione `net.ipv4.ip_local_port_range`.
    ```
    spec:
      template:
        spec:
          initContainers:
          - command:
            - sh
            - -c
            - sysctl -e -w net.core.somaxconn=32768;  sysctl -e -w net.ipv4.ip_local_port_range="1025 65535";
            image: alpine:3.6
            imagePullPolicy: IfNotPresent
            name: sysctl
            resources: {}
            securityContext:
              privileged: true
    ```
    {: codeblock}

2. Applica la patch a ciascuna delle tue distribuzioni.
    ```
    kubectl patch deployment <deployment_name> --patch pod-patch.yaml
    ```
    {: pre}

3. Se hai modificato il valore `net.core.somaxconn` nelle impostazioni del kernel, la maggior parte delle applicazioni può utilizzare automaticamente il valore aggiornato. Tuttavia, alcune applicazioni potrebbero richiedere che tu modifichi manualmente il valore corrispondente nel codice dell'applicazione in modo che corrisponda al valore del kernel. Ad esempio, se stai ottimizzando le prestazioni di un pod in cui è in esecuzione un'applicazione NGINX, devi modificare il valore del campo `backlog` nel codice dell'applicazione NGINX in modo che corrisponda. Per ulteriori informazioni, vedi questo [post del blog di NGINX ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.nginx.com/blog/tuning-nginx/).

<br />


## Regolazione delle risorse del provider di metriche del cluster
{: #metrics}

Le configurazione del provider di metriche del tuo cluster (`metrics-server` in Kubernetes 1.12 e successive o `heapster` nelle versioni precedenti) sono ottimizzate per i cluster con 30 o meno pod per ogni nodo di lavoro. Se il tuo cluster ha più pod per nodo di lavoro, il contenitore principale `metrics-server` o `heapster` del provider di metriche per il pod potrebbe riavviarsi frequentemente con un messaggio di errore come `OOMKilled`.

Il pod del provider di metriche ha anche un contenitore `nanny` che ridimensiona le richieste e i limiti delle risorse del contenitore principale `metrics-server` o `heapster` in risposta al numero di nodi di lavoro nel cluster. Puoi modificare le risorse predefinite modificando la mappa di configurazione del provider di metriche.

Prima di iniziare: [accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Apri il file YALM di mappa di configurazione del provider di metriche del cluster.
    *  Per `metrics-server`:
       ```
       kubectl get configmap metrics-server-config -n kube-system -o yaml
       ```
       {: pre}
    *  Per `heapster`:
       ```
       kubectl get configmap heapster-config -n kube-system -o yaml
       ```
       {: pre}
    Output di esempio:
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
    kind: ConfigMap
    metadata:
      annotations:
        armada-service: cruiser-kube-addons
        version: --
      creationTimestamp: 2018-10-09T20:15:32Z
      labels:
        addonmanager.kubernetes.io/mode: EnsureExists
        kubernetes.io/cluster-service: "true"
      name: heapster-config
      namespace: kube-system
      resourceVersion: "526"
      selfLink: /api/v1/namespaces/kube-system/configmaps/heapster-config
      uid: 11a1aaaa-bb22-33c3-4444-5e55e555e555
    ```
    {: screen}

2.  Aggiungi il campo `memoryPerNode` alla mappa di configurazione nella sezione `data.NannyConfiguration`. Il valore predefinito per `metrics-server` e per `heapster` è impostato su `4Mi`.
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
        memoryPerNode: 5Mi
    kind: ConfigMap
    ...
    ```
    {: codeblock}

3.  Applica le tue modifiche.
    ```
    kubectl apply -f heapster-config.yaml
    ```
    {: pre}

4.  Monitora i pod del provider di metriche per vedere se i contenitori continuano a essere riavviati a causa di un messaggio di errore `OOMKilled`. In tal caso, ripeti questi passi e aumenta la dimensione `memoryPerNode` finché il pod non è stabile.

Vuoi ottimizzare altre impostazioni? Consulta la [documentazione Add-on resizer configuration di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/autoscaler/tree/master/addon-resizer#addon-resizer-configuration) per ulteriori idee.
{: tip}
