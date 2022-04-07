---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, node scaling

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
{:gif: data-image-type='gif'}



# Ridimensionamento dei cluster
{: #ca}

Con il plugin `ibm-iks-cluster-autoscaler` di {{site.data.keyword.containerlong_notm}}, puoi ridimensionare automaticamente i pool di nodi di lavoro nel tuo cluster per aumentare o ridurre il numero di nodi di lavoro nel pool in base alle esigenze di dimensionamento dei tuoi carichi di lavoro pianificati. Il plugin `ibm-iks-cluster-autoscaler` è basato sul progetto [Kubernetes Cluster-Autoscaler ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler).
{: shortdesc}

Vuoi invece ridimensionare automaticamente i tuoi pod? Consulta [Ridimensionamento delle applicazioni](/docs/containers?topic=containers-app#app_scaling).
{: tip}

Il cluster autoscaler (ossia, il componente di ridimensionamento automatico del cluster) è disponibile per i cluster standard configurati con la connettività di rete pubblica. Se il tuo cluster non può accedere alla rete pubblica, perché è, ad esempio, un cluster privato dietro un firewall o un cluster con solo l'endpoint del servizio privato abilitato, non puoi utilizzare il cluster autoscaler nel tuo cluster.
{: important}

## Descrizione del ridimensionamento e dell'ampliamento
{: #ca_about}

Il cluster autoscaler esegue periodicamente la scansione del cluster per regolare il numero di nodi di lavoro all'interno dei pool di nodi di lavoro che gestisce in risposta alle tue richieste di risorse del carico di lavoro e alle eventuali impostazioni personalizzate da te configurate, ad esempio gli intervalli di scansione. Ogni minuto, il cluster autoscaler verifica le seguenti situazioni.
{: shortdesc}

*   **Pod in sospeso da ampliare**: un pod è considerato in sospeso quando esistono risorse di calcolo insufficienti per pianificare il pod su un nodo di lavoro. Quando il cluster autoscaler rileva pod in sospeso, amplia i nodi di lavoro in modo uniforme tra le zone per soddisfare le richieste di risorse del carico di lavoro.
*   **Nodi di lavoro sottoutilizzati da ridurre**: per impostazione predefinita, i nodi di lavoro eseguiti con meno del 50% delle risorse di calcolo totali richieste per 10 minuti o più e che possono ripianificare i loro carichi di lavoro su altri nodi di lavoro sono considerati sottoutilizzati. Se il cluster autoscaler rileva nodi di lavoro sottoutilizzati, riduce i tuoi nodi di lavoro uno alla volta in modo che tu disponga solo delle risorse di calcolo di cui hai bisogno. Se lo desideri, puoi [personalizzare](/docs/containers?topic=containers-ca#ca_chart_values) la soglia di utilizzo della riduzione del 50% per 10 minuti.

La scansione e l'ampliamento e riduzione avvengono a intervalli regolari nel tempo e, a seconda del numero di nodi di lavoro, queste operazioni potrebbero richiedere più tempo, ad esempio 30 minuti.

Il cluster autoscaler regola il numero di nodi di lavoro considerando le [richieste di risorse ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) che definisci per le tue distribuzioni e non l'effettivo utilizzo del nodo di lavoro. Se i tuoi pod e le tue distribuzioni non richiedono quantità adeguate di risorse, devi regolare i relativi file di configurazione. Il cluster autoscaler non può regolarli per te.  Inoltre, tieni presente che i nodi di lavoro utilizzano alcune delle loro risorse di calcolo per le funzionalità di base del cluster, i [componenti aggiuntivi](/docs/containers?topic=containers-update#addons) predefiniti e personalizzati e le [riserve di risorse](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).
{: note}

<br>
**Come si presentano l'ampliamento e la riduzione?**<br>
In generale, il cluster autoscaler calcola il numero di nodi di lavoro di cui il tuo cluster ha bisogno per eseguire il proprio carico di lavoro. L'ampliamento o la riduzione del cluster dipendono da diversi fattori, compresi i seguenti.
*   La dimensione minima e massima del nodo di lavoro per zona da te impostata.
*   Le tue richieste di risorse del pod in sospeso e determinati metadati che associ al carico di lavoro, come anti-affinità, etichette per posizionare i pod solo su alcuni tipi di macchine o [PDB (pod disruption budget)![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/).
*   I pool di nodi di lavoro gestiti dal cluster autoscaler, potenzialmente tra le zone in un [cluster multizona](/docs/containers?topic=containers-ha_clusters#multizone).
*   I [valori personalizzati del grafico Helm](#ca_chart_values) impostati, quale ad esempio la scelta di ignorare i nodi di lavoro per l'eliminazione se utilizzano l'archiviazione locale.

Per ulteriori informazioni, consulta le domande frequenti (FAQ) su Kubernetes Cluster Autoscaler su [Come si amplia il lavoro? ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-up-work) e [Come si ridimensiona il lavoro? ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-down-work).

<br>

**Posso modificare le modalità di ampliamento e ridimensionamento del lavoro?**<br>
Puoi personalizzare le impostazioni o utilizzare altre risorse Kubernetes per influire sulle modalità di ampliamento e ridimensionamento del lavoro.
*   **Ampliamento**: [Personalizza i valori del grafico Helm del cluster autoscaler](#ca_chart_values) quali `scanInterval`, `expander`, `skipNodes` o `maxNodeProvisionTime`. Rivedi le modalità di esecuzione di un [provisioning in eccesso dei nodi di lavoro](#ca_scaleup) in modo da poter ampliate i nodi di lavoro prima che un pool di nodi di lavori esaurisca le risorse. Puoi anche [configurare PDB (pod disruption budget) Kubernetes e limiti di priorità dei pod](#scalable-practices-apps) per influire sul funzionamento dell'ampliamento dei lavori.
*   **Ridimensionamento**: [Personalizza i valori del grafico Helm del cluster autoscaler](#ca_chart_values) quali `scaleDownUnneededTime`, `scaleDownDelayAfterAdd`, `scaleDownDelayAfterDelete` o `scaleDownUtilizationThreshold`.

<br>
**Posso impostare la dimensione minima per ogni zona per ampliare immediatamente il mio cluster a tale dimensione?**<br>
No, l'impostazione di una dimensione minima (`minSize`) non attiva automaticamente un ampliamento, La dimensione minima (`minSize`) è una soglia che serve a fare in modo che il cluster autoscaler non esegua il ridimensionamento al di sotto di un certo numero di nodi di lavoro per ogni zona. Se il tuo cluster non ha ancora tale numero per ogni zona, il cluster autoscaler non esegue alcun ampliamento finché non hai richieste di risorse del carico di lavoro che richiedono ulteriori risorse. Ad esempio, se hai un pool di nodi di lavoro con un singolo nodo di lavoro per ogni tre zone (tre nodi di lavoro in totale) e imposti la dimensione minima (`minSize`) su `4` per ogni zona, il cluster autoscaler non esegue immediatamente il provisioning di tre nodi di lavoro aggiuntivi per ogni zona (12 nodi di lavoro in totale). L'ampliamento viene invece attivato dalle richieste di risorse. Se crei un carico di lavoro che richiede le risorse di 15 nodi di lavoro, il cluster autoscaler amplia il pool di nodi di lavoro per soddisfare questa richiesta. Ora, la `minSize` indica che il cluster autoscaler non esegue un ridimensionamento al di sotto di quattro nodi di lavoro per ogni zona anche se rimuovi il carico di lavoro che richiede tale quantità.

<br>
**In che modo differisce questo comportamento dai pool di nodi di lavoro che non sono gestiti dal cluster autoscaler?**<br>
Quando [crei un pool di nodi di lavoro](/docs/containers?topic=containers-add_workers#add_pool), specifichi il numero di nodi di lavoro per zona inclusi nel pool. Il pool di nodi di lavoro mantiene quel numero di nodi di lavoro finché non lo [ridimensioni](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) o [ribilanci](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance). Il pool di nodi di lavoro non aggiunge o rimuove automaticamente i nodi di lavoro. Se hai più pod di quanti possano essere pianificati, i pod rimangono in sospeso fino a quando non ridimensioni il pool di nodi di lavoro.

Quando abiliti il cluster autoscaler per un pool di nodi di lavoro, i nodi di lavoro vengono ampliati o ridotti in risposta alle tue impostazioni delle specifiche del pod e alle tue richieste di risorse. Non hai bisogno di ridimensionare o ribilanciare manualmente il pool di nodi di lavoro.

<br>
**Posso vedere un esempio di come il cluster autoscaler esegue l'ampliamento e la riduzione?**<br>
Considera la seguente immagine per un esempio di ampliamento e riduzione del cluster.

_Figura: ampliamento e riduzione automatici di un cluster._
![GIF di ampliamento e riduzione automatici di un cluster](images/cluster-autoscaler-x3.gif){: gif}

1.  Il cluster ha quattro nodi di lavoro in due pool di nodi di lavoro che vengono estesi tra le zone. Ogni pool ha un nodo di lavoro per zona, ma il **Pool di nodi di lavoro A** ha una macchina di tipo `u2c.2x4` e il **Pool di nodi di lavoro B** ha una macchina di tipo `b2c.4x16`. Le tue risorse di calcolo totali sono circa 10 core (2 core x 2 nodi di lavoro per il **Pool di nodi di lavoro A** e 4 core x 2 nodi di lavoro per il **Pool di nodi di lavoro B**). Il tuo cluster esegue attualmente un carico di lavoro che richiede 6 di questi 10 core. Ulteriori risorse di calcolo vengono utilizzate su ciascun nodo di lavoro dalle [risorse riservate](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node) necessarie per eseguire il cluster, i nodi di lavoro e qualsiasi componente aggiuntivo come, ad esempio, il cluster autoscaler.
2.  Il cluster autoscaler è configurato per gestire entrambi i pool di nodi di lavoro con le seguenti dimensioni minime e massime per zona:
    *  **Pool di nodi di lavoro A**: `minSize=1`, `maxSize=5`.
    *  **Pool di nodi di lavoro B**: `minSize=1`, `maxSize=2`.
3.  Pianifichi le distribuzioni che richiedono 14 repliche di pod aggiuntive di un'applicazione che richiede un core di CPU per ogni replica. Una replica di pod può essere distribuita sulle risorse correnti, mentre le altre 13 sono in sospeso.
4.  Il cluster autoscaler amplia i tuoi nodi di lavoro entro questi vincoli per supportare le ulteriori 13 repliche di pod richieste dalla risorsa.
    *  **Pool di nodi di lavoro A**: sette nodi di lavoro vengono aggiunti in un metodo round-robin il più uniformemente possibile tra le zone. I nodi di lavoro aumentano la capacità di calcolo del cluster di circa 14 core (2 core x 7 nodi di lavoro).
    *  **Pool di nodi di lavoro B**: due nodi di lavoro vengono aggiunti uniformemente tra le zone, raggiungendo il valore `maxSize` di 2 nodi di lavoro per ogni zona. I nodi di lavoro aumentano la capacità del cluster di circa 8 core (4 core x 2 nodi di lavoro).
5.  I 20 pod con richieste di 1 core vengono distribuiti come segue in tutti i nodi di lavoro. Poiché i nodi di lavoro hanno riserve di risorse e pod eseguiti per ricoprire le funzioni cluster predefinite, i pod per il tuo carico di lavoro non possono utilizzare tutte le risorse di calcolo disponibili di un nodo di lavoro. Ad esempio, sebbene i nodi di lavoro `b2c.4x16` abbiano quattro core, solo tre pod che richiedono un minimo di un core ciascuno possono essere pianificati sui nodi di lavoro.
    <table summary="Una tabella che descrive la distribuzione del carico di lavoro nel cluster ridimensionato.">
    <caption>Distribuzione del carico di lavoro nel cluster ridimensionato.</caption>
    <thead>
    <tr>
      <th>Pool di nodi di lavoro</th>
      <th>Zona</th>
      <th>Tipo</th>
      <th>Numero di nodi di lavoro</th>
      <th>Numero di pod</th>
    </tr>
    </thead>
    <tbody>
    <tr>
      <td>A</td>
      <td>dal10</td>
      <td>u2c.2x4</td>
      <td>Quattro nodi</td>
      <td>Tre pod</td>
    </tr>
    <tr>
      <td>A</td>
      <td>dal12</td>
      <td>u2c.2x4</td>
      <td>Cinque nodi</td>
      <td>Cinque pod</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal10</td>
      <td>b2c.4x16</td>
      <td>Due nodi</td>
      <td>Sei pod</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal12</td>
      <td>b2c.4x16</td>
      <td>Due nodi</td>
      <td>Sei pod</td>
    </tr>
    </tbody>
    </table>
6.  Non hai più bisogno del carico di lavoro aggiuntivo, quindi elimini la distribuzione. Dopo un breve periodo di tempo, il cluster autoscaler del cluster rileva che il tuo cluster non ha più bisogno di tutte le sue risorse di calcolo e riduce i nodi di lavoro uno alla volta.
7.  I tuoi pool di nodi di lavoro vengono ridotti. Il cluster autoscaler esegue la scansione a intervalli regolari per verificare la presenza di richieste di risorse pod in sospeso e di nodi di lavoro sottoutilizzati per ampliare o ridurre i tuoi pool di nodi di lavoro.

## Attuazione di procedure di distribuzione scalabili
{: #scalable-practices}

Ottieni il massimo dal cluster autoscaler utilizzando le seguenti strategie per il tuo nodo di lavoro e strategie di distribuzione del carico di lavoro. Per ulteriori informazioni, consulta le [domande frequenti (FAQ) su Kubernetes Cluster Autoscaler ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md).
{: shortdesc}

[Prova il cluster autoscaler](#ca_helm) con alcuni carichi di lavoro di test per farti un'idea della modalità di [ampliamento
e ridimensionamento](#ca_about), quali [valori personalizzati](#ca_chart_values) potresti voler configurare e qualsiasi altra cosa potresti volere, quale il [provisioning in eccesso](#ca_scaleup) dei nodi di lavoro o la [limitazione delle applicazioni](#ca_limit_pool). Quindi ripulisci l'ambiente di test ed effettua una pianificazione in modo da includere i valori personalizzati e le impostazioni aggiuntive con una nuova installazione del cluster autoscaler.

### Posso ridimensionare automaticamente più pool di nodi di lavoro alla volta?
{: #scalable-practices-multiple}
Sì, una volta che installi il grafico Helm, puoi scegliere i pool di nodi di lavoro del cluster da ridimensionare automaticamente [nella mappa di configurazione](#ca_cm). Puoi eseguire un solo grafico Helm
`ibm-iks-cluster-autoscaler` per ciascun cluster.
{: shortdesc}

### Come posso accertarmi che il cluster autoscaler risponda alle risorse di cui ha bisogno la mia applicazione?
{: #scalable-practices-resrequests}

L'autoscaler ridimensiona il tuo cluster in risposta alle tue [richieste di risorse ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) del carico di lavoro. Pertanto, devi specificare le [richieste di risorse ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) per tutte le tue distribuzioni, poiché tali richieste sono quelle utilizzate dal cluster autoscaler per calcolare il numero di nodi di lavoro necessari per eseguire il carico di lavoro. Tieni presente che il ridimensionamento automatico si basa sull'utilizzo del calcolo richiesto dalle tue configurazioni del carico di lavoro e non prende in considerazione altri fattori come i costi della macchina.
{: shortdesc}

### Posso ridimensionare un pool di nodi di lavoro fino a zero (0) nodi?
{: #scalable-practices-zero}

No, non puoi impostare il cluster autoscaler `minSize` su `0`. Inoltre, a meno che non [disabiliti](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure) tutti gli ALB (application load balancer) pubblici in ciascuna zona del tuo cluster, devi modificare il valore di `minSize` impostandolo su `2` nodi di lavoro per ogni zona, in modo che i pod ALB possano essere diffusi per l'alta disponibilità.
{: shortdesc}

### Posso ottimizzare le mie distribuzioni per il ridimensionamento automatico?
{: #scalable-practices-apps}

Sì, puoi aggiungere varie funzioni Kubernetes alla tua distribuzione per adattare il modo in cui il cluster autoscaler considera le tue richieste di risorsa per il ridimensionamento.
{: shortdesc}
*   Utilizza i [PDB (pod disruption budget) ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) per impedire improvvise ripianificazioni o eliminazioni dei tuoi pod.
*   Se utilizzi la priorità dei pod, puoi [modificare il limite di priorità ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-cluster-autoscaler-work-with-pod-priority-and-preemption) per modificare quali tipi di priorità attivano l'ampliamento. Per impostazione predefinita, il limite di priorità è zero (`0`).

### Posso utilizzare contaminazioni e tolleranze con i pool di nodi di lavoro ridimensionati automaticamente?
{: #scalable-practices-taints}

Poiché non è possibile applicare contaminazioni a livello di pool di nodi di lavoro, non [contaminare i nodi di lavoro](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) per evitare risultati imprevisti. Ad esempio, quando distribuisci un carico di lavoro che non è tollerato dai nodi di lavoro contaminati, i nodi di lavoro non vengono considerati per l'ampliamento e potrebbero essere ordinati altri nodi di lavoro anche se il cluster ha una capacità sufficiente. Tuttavia, se hanno una quantità di risorse utilizzate inferiore alla soglia (per impostazione predefinita, il 50%), i nodi di lavoro contaminati vengono ancora identificati come sottoutilizzati e dunque considerati per il ridimensionamento.
{: shortdesc}

### Perché i miei pool di nodi di lavoro ridimensionati automaticamente non sono bilanciati?
{: #scalable-practices-unbalanced}

Durante un ampliamento, il cluster autoscaler bilancia i nodi tra le zone, con una differenza consentita di un nodo di lavoro in più o in meno (+/- 1). I tuoi carichi di lavoro in sospeso potrebbero non richiedere una capacità sufficiente per rendere bilanciata ogni zona. In questo caso, se vuoi bilanciare manualmente i pool di nodi di lavoro, [aggiorna la mappa di configurazione del cluster autoscaler](#ca_cm) per rimuovere il pool di nodi di lavoro non bilanciato. Esegui quindi il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) `ibmcloud ks worker-pool-rebalance` e aggiungi nuovamente il pool di nodi di lavoro alla mappa di configurazione del cluster autoscaler.
{: shortdesc}


### Perché non riesco a ridimensionare o ribilanciare il mio pool di nodi di lavoro?
{: #scalable-practices-resize}

Quando il cluster autoscaler è abilitato per un pool di nodi di lavoro, non puoi [ridimensionare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) o [ribilanciare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) i tuoi pool di nodi di lavoro. Devi [modificare la mappa di configurazione](#ca_cm) per cambiare le dimensioni minime e massime del pool di nodi di lavoro oppure disabilitare il ridimensionamento automatico del cluster per tale pool. Non utilizzare il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_rm) `ibmcloud ks worker-rm` per rimuovere singoli nodi di lavoro dal tuo pool, in quanto potrebbe sbilanciare il pool di nodi di lavoro.
{: shortdesc}

Inoltre, se non disabiliti i pool di nodi di lavoro prima di disinstallare il grafico Helm `ibm-iks-cluster-autoscaler`, i pool non potranno essere ridimensionati manualmente. Reinstalla il grafico Helm `ibm-iks-cluster-autoscaler`, [modifica la mappa di configurazione](#ca_cm) per disabilitare il pool di nodi di lavoro e prova di nuovo.

<br />


## Distribuzione del grafico Helm del cluster autoscaler nel tuo cluster
{: #ca_helm}

Installa il plugin del cluster autoscaler di {{site.data.keyword.containerlong_notm}} con un grafico Helm per ridimensionare automaticamente i pool di nodi di lavoro nel tuo cluster.
{: shortdesc}

**Prima di iniziare**:

1.  [Installa la CLI e i plugin richiesti](/docs/cli?topic=cloud-cli-getting-started):
    *  CLI {{site.data.keyword.Bluemix_notm}} (`ibmcloud`)
    *  Plugin {{site.data.keyword.containerlong_notm}} (`ibmcloud ks`)
    *  Plugin {{site.data.keyword.registrylong_notm}} (`ibmcloud cr`)
    *  Kubernetes (`kubectl`)
    *  Helm (`helm`)
2.  [Crea un cluster standard](/docs/containers?topic=containers-clusters#clusters_ui) che esegue **Kubernetes versione 1.12 o successiva**.
3.   [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
4.  Conferma che le tue credenziali di {{site.data.keyword.Bluemix_notm}} Identity and Access Management siano memorizzate nel cluster. Il cluster autoscaler utilizza questo segreto per autenticare le credenziali. Se il segreto manca, [crealo reimpostando le credenziali](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions).
    ```
    kubectl get secrets -n kube-system | grep storage-secret-store
    ```
    {: pre}
5.  Il cluster autoscaler può ridimensionare solo i pool di nodi di lavoro che hanno l'etichetta `ibm-cloud.kubernetes.io/worker-pool-id`.
    1.  Controlla se il tuo pool di nodi di lavoro ha l'etichetta richiesta.
        ```
        ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels
        ```
        {: pre}

        Output di esempio di un pool di nodi di lavoro con l'etichetta:
        ```
        Labels:             ibm-cloud.kubernetes.io/worker-pool-id=a1aa111111b22b22cc3c3cc444444d44-4d555e5
        ```
        {: screen}
    2.  Se il tuo pool di nodi di lavoro non ha l'etichetta richiesta, [aggiungi un nuovo pool di nodi di lavoro](/docs/containers?topic=containers-add_workers#add_pool) e utilizza questo pool con il cluster autoscaler.


<br>
**Per installare il plugin `ibm-iks-cluster-autoscaler` nel tuo cluster**:

1.  [Segui le istruzioni](/docs/containers?topic=containers-helm#public_helm_install) per installare il client **Helm versione 2.11 o successiva** sulla tua macchina locale e per installare il server Helm (tiller) con un account di servizio nel tuo cluster.
2.  Verifica che tiller sia installato con un account di servizio.

    ```
    kubectl get serviceaccount -n kube-system tiller
    ```
    {: pre}

    Output di esempio:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}
3.  Aggiungi e aggiorna il repository Helm in cui si trova il grafico Helm del cluster autoscaler.
    ```
    helm repo add iks-charts https://icr.io/helm/iks-charts
    ```
    {: pre}
    ```
    helm repo update
    ```
    {: pre}
4.  Installa il grafico Helm del cluster autoscaler nello spazio dei nomi `kube-system` del tuo cluster.

    Durante l'installazione, puoi [personalizzare ulteriormente le impostazioni del cluster autoscaler](#ca_chart_values), come il tempo di attesa obbligatorio che precede l'ampliamento o il ridimensionamento dei nodi di lavoro.
    {: tip}

    ```
    helm install iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Output di esempio:
    ```
    NAME:   ibm-iks-cluster-autoscaler
    LAST DEPLOYED: Thu Nov 29 13:43:46 2018
    NAMESPACE: kube-system
    STATUS: DEPLOYED

    RESOURCES:
    ==> v1/ClusterRole
    NAME                       AGE
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Pod(related)

    NAME                                        READY  STATUS             RESTARTS  AGE
    ibm-iks-cluster-autoscaler-67c8f87b96-qbb6c  0/1    ContainerCreating  0         1s

    ==> v1/ConfigMap

    NAME              AGE
    iks-ca-configmap  1s

    ==> v1/ClusterRoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Role
    ibm-iks-cluster-autoscaler  1s

    ==> v1/RoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Service
    ibm-iks-cluster-autoscaler  1s

    ==> v1beta1/Deployment
    ibm-iks-cluster-autoscaler  1s

    ==> v1/ServiceAccount
    ibm-iks-cluster-autoscaler  1s

    NOTES:
    Thank you for installing: ibm-iks-cluster-autoscaler. Your release is named: ibm-iks-cluster-autoscaler

    For more information about using the cluster autoscaler, refer to the chart README.md file.
    ```
    {: screen}

5.  Verifica che l'installazione sia riuscita correttamente.

    1.  Verifica che il pod del cluster autoscaler sia in stato di esecuzione (**Running**).
        ```
        kubectl get pods --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        Output di esempio:
        ```
        ibm-iks-cluster-autoscaler-8497bfc968-dbn7w   1/1       Running   0          9m
        ```
        {: screen}
    2.  Verifica che il servizio di cluster autoscaler sia stato creato.
        ```
        kubectl get service --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        Output di esempio:
        ```
        ibm-iks-cluster-autoscaler   ClusterIP   172.21.xxx.xx    <none>        8085/TCP        9m
        ```
        {: screen}

6.  Ripeti questa procedura per ogni cluster in cui desideri eseguire il provisioning del cluster autoscaler.

7.  Per iniziare a ridimensionare i tuoi pool di nodi di lavoro, vedi [Aggiornamento della configurazione del cluster autoscaler](#ca_cm).

<br />


## Aggiornamento della mappa di configurazione del cluster autoscaler per abilitare il ridimensionamento
{: #ca_cm}

Aggiorna la mappa di configurazione del cluster autoscaler per abilitare il ridimensionamento automatico dei nodi di lavoro nei tuoi pool di nodi di lavoro in base ai valori minimi e massimi da te impostati.
{: shortdesc}

Dopo aver modificato la mappa di configurazione per abilitare un pool di nodi di lavoro, il cluster autoscaler ridimensiona il cluster in risposta alle tue richieste di carico di lavoro. Pertanto, non puoi [ridimensionare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) o [ribilanciare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) i tuoi pool di nodi di lavoro. La scansione e l'ampliamento e riduzione avvengono a intervalli regolari nel tempo e, a seconda del numero di nodi di lavoro, queste operazioni potrebbero richiedere più tempo, ad esempio 30 minuti. In seguito, se vuoi [rimuovere il cluster autoscaler](#ca_rm), devi prima disabilitare ogni pool di nodi di lavoro nella mappa di configurazione.
{: note}

**Prima di iniziare**:
*  [Installa il plugin `ibm-iks-cluster-autoscaler`](#ca_helm).
*  [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**Per aggiornare la mappa di configurazione e i valori del cluster autoscaler**:

1.  Modifica il file YAML della mappa di configurazione del cluster autoscaler.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system -o yaml
    ```
    {: pre}
    Output di esempio:
    ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false}
        ]
    kind: ConfigMap
    metadata:
      creationTimestamp: 2018-11-29T18:43:46Z
      name: iks-ca-configmap
      namespace: kube-system
      resourceVersion: "2227854"
      selfLink: /api/v1/namespaces/kube-system/configmaps/iks-ca-configmap
      uid: b45d047b-f406-11e8-b7f0-82ddffc6e65e
    ```
    {: screen}
2.  Modifica la mappa di configurazione con i parametri per definire il modo in cui il cluster autoscaler ridimensiona il tuo pool di nodi di lavoro del cluster. **Nota:** a meno che non [disabiliti](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure) tutti gli ALB (application load balancer) pubblici in ciascuna zona del tuo cluster standard, devi modificare la `minSize` impostandola su `2` per ogni zona in modo che i pod ALB possano essere diffusi per l'alta disponibilità.

    <table>
    <caption>Parametri della mappa di configurazione del cluster autoscaler</caption>
    <thead>
    <th id="parameter-with-default">Parametro con valore predefinito</th>
    <th id="parameter-with-description">Descrizione</th>
    </thead>
    <tbody>
    <tr>
    <td id="parameter-name" headers="parameter-with-default">`"name": "default"`</td>
    <td headers="parameter-name parameter-with-description">Sostituisci `"default"` con il nome o l'ID del pool di nodi di lavoro che vuoi ridimensionare. Per elencare i pool di nodi di lavoro, esegui `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`.<br><br>
    Per gestire più di un pool di nodi di lavoro, copia la riga JSON in una riga separata da virgole, come nel seguente esempio. <pre class="codeblock">[
     {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
     {"name": "Pool2","minSize": 2,"maxSize": 5,"enabled":true}
    ]</pre><br><br>
    **Nota**: il cluster autoscaler può ridimensionare solo i pool di nodi di lavoro che hanno l'etichetta `ibm-cloud.kubernetes.io/worker-pool-id`. Per controllare se il tuo pool di nodi di lavoro ha l'etichetta richiesta, esegui `ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels`. Se il tuo pool di nodi di lavoro non ha l'etichetta richiesta, [aggiungi un nuovo pool di nodi di lavoro](/docs/containers?topic=containers-add_workers#add_pool) e utilizza questo pool con il cluster autoscaler.</td>
    </tr>
    <tr>
    <td id="parameter-minsize" headers="parameter-with-default">`"minSize": 1`</td>
    <td headers="parameter-minsize parameter-with-description">Specifica il numero minimo di nodi di lavoro per ogni zona a cui il cluster autoscaler può ridurre il pool di nodi di lavoro. Il valore deve essere `2` o superiore in modo che i tuoi pod ALB possano essere diffusi per l'alta disponibilità. Se hai [disabilitato](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure) tutti gli ALB pubblici in ciascuna zona del tuo cluster standard, puoi impostare il valore su `1`.
    <p class="note">L'impostazione di una `minSize` non attiva automaticamente un ampliamento. La dimensione minima (`minSize`) è una soglia che serve a fare in modo che il cluster autoscaler non esegua il ridimensionamento al di sotto di un certo numero di nodi di lavoro per ogni zona. Se il tuo cluster non ha ancora tale numero per ogni zona, il cluster autoscaler non esegue alcun ampliamento finché non hai richieste di risorse del carico di lavoro che richiedono ulteriori risorse. Ad esempio, se hai un pool di nodi di lavoro con un singolo nodo di lavoro per ogni tre zone (tre nodi di lavoro in totale) e imposti la dimensione minima (`minSize`) su `4` per ogni zona, il cluster autoscaler non esegue immediatamente il provisioning di tre nodi di lavoro aggiuntivi per ogni zona (12 nodi di lavoro in totale). L'ampliamento viene invece attivato dalle richieste di risorse. Se crei un carico di lavoro che richiede le risorse di 15 nodi di lavoro, il cluster autoscaler amplia il pool di nodi di lavoro per soddisfare questa richiesta. Ora, la `minSize` indica che il cluster autoscaler non esegue un ridimensionamento al di sotto di quattro nodi di lavoro per ogni zona anche se rimuovi il carico di lavoro che richiede tale quantità.Per ulteriori informazioni, vedi la [documentazione di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#when-does-cluster-autoscaler-change-the-size-of-a-cluster).</p></td>
    </tr>
    <tr>
    <td id="parameter-maxsize" headers="parameter-with-default">`"maxSize": 2`</td>
    <td headers="parameter-maxsize parameter-with-description">Specifica il numero massimo di nodi di lavoro per ogni zona a cui il cluster autoscaler può ampliare il pool di nodi di lavoro. Il valore deve essere uguale o maggiore del valore che hai impostato per `minSize`.</td>
    </tr>
    <tr>
    <td id="parameter-enabled" headers="parameter-with-default">`"enabled": false`</td>
    <td headers="parameter-enabled parameter-with-description">Imposta il valore su `true` per fare in modo che il cluster autoscaler gestisca il ridimensionamento per il pool di nodi di lavoro. Imposta il valore su `false` per fare in modo che il cluster autoscaler non ridimensioni il pool di nodi di lavoro.<br><br>
    In seguito, se vuoi [rimuovere il cluster autoscaler](#ca_rm), devi prima disabilitare ogni pool di nodi di lavoro nella mappa di configurazione.</td>
    </tr>
    </tbody>
    </table>
3.  Salva il file di configurazione.
4.  Ottieni il pod del cluster autoscaler.
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
5.  Esamina la sezione **`Events`** del pod del cluster autoscaler e controlla l'evento **`ConfigUpdated`** per verificare che la mappa di configurazione sia stata aggiornata correttamente. Il formato del messaggio di evento della tua mappa di configurazione è il seguente: `minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`.

    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    Output di esempio:
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
    Type    Reason                 Age   From                     Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

## Personalizzazione dei valori di configurazione del grafico Helm del cluster autoscaler
{: #ca_chart_values}

Personalizza le impostazioni del cluster autoscaler come la quantità di tempo che deve attendere prima di ampliare o ridurre i nodi di lavoro.
{: shortdesc}

**Prima di iniziare**:
*  [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  [Installa il plugin `ibm-iks-cluster-autoscaler`](#ca_helm).

**Per aggiornare i valori del cluster autoscaler**:

1.  Esamina i valori di configurazione del grafico Helm del cluster autoscaler. Il cluster autoscaler viene fornito con le impostazioni predefinite. Tuttavia, puoi modificare alcuni valori, ad esempio gli intervalli di riduzione o di scansione, a seconda della frequenza con cui modifichi i tuoi carichi di lavoro del cluster.
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}

    Output di esempio:
    ```
    expander: least-waste
    image:
      pullPolicy: Always
      repository: icr.io/iks-charts/ibm-iks-cluster-autoscaler
      tag: dev1
    maxNodeProvisionTime: 120m
    resources:
      limits:
        cpu: 300m
        memory: 300Mi
      requests:
        cpu: 100m
        memory: 100Mi
    scaleDownDelayAfterAdd: 10m
    scaleDownDelayAfterDelete: 10m
    scaleDownUtilizationThreshold: 0.5
    scaleDownUnneededTime: 10m
    scanInterval: 1m
    skipNodes:
      withLocalStorage: true
      withSystemPods: true
    ```
    {: codeblock}

    <table>
    <caption>Valori di configurazione del cluster autoscaler</caption>
    <thead>
    <th>Parametro</th>
    <th>Descrizione</th>
    <th>Valore predefinito</th>
    </thead>
    <tbody>
    <tr>
    <td>Parametro `api_route`</td>
    <td>Imposta l'[endpoint API {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cli_api) per la regione in cui si trova il tuo cluster.</td>
    <td>Nessun valore predefinito; utilizza la regione di destinazione in cui si trova il tuo cluster.</td>
    </tr>
    <tr>
    <td>Parametro `expander`</td>
    <td>Specifica in che modo il cluster autoscaler determina quale pool di nodi di lavoro ridimensionare se disponi di più pool. I valori possibili sono:
    <ul><li>`random`: seleziona in modo casuale tra `most-pods` e `least-waste`.</li>
    <li>`most-pods`: seleziona il pool di nodi di lavoro in grado di pianificare la maggior parte dei pod durante l'ampliamento. Scegli questo metodo se utilizzi `nodeSelector` per assicurarti che i pod vengano posizionati su specifici nodi di lavoro.</li>
    <li>`least-waste`: seleziona il pool di nodi di lavoro che ha meno CPU inutilizzata dopo l'ampliamento. Se due pool di nodi di lavoro utilizzano la stessa quantità di risorse CPU dopo l'ampliamento, viene selezionato il pool di nodi di lavoro con meno memoria inutilizzata.</li></ul></td>
    <td>random</td>
    </tr>
    <tr>
    <td>Parametro `image.repository`</td>
    <td>Specifica l'immagine Docker del cluster autoscaler da utilizzare.</td>
    <td>`icr.io/iks-charts/ibm-iks-cluster-autoscaler`</td>
    </tr>
    <tr>
    <td>Parametro `image.pullPolicy`</td>
    <td>Specifica quando estrarre l'immagine Docker. I valori possibili sono:
    <ul><li>`Always`: estrae l'immagine ogni volta che il pod viene avviato.</li>
    <li>`IfNotPresent`: estrae l'immagine solo se l'immagine non è già presente localmente.</li>
    <li>`Never`: suppone che l'immagine esista localmente e non estrae mai l'immagine.</li></ul></td>
    <td>Always</td>
    </tr>
    <tr>
    <td>Parametro `maxNodeProvisionTime`</td>
    <td>Imposta la quantità massima di tempo in minuti che un nodo di lavoro può richiedere per iniziare il provisioning prima che il cluster autoscaler annulli la richiesta di ampliamento.</td>
    <td>`120m`</td>
    </tr>
    <tr>
    <td>Parametro `resources.limits.cpu`</td>
    <td>Imposta la quantità massima di CPU del nodo di lavoro che può essere consumata dal pod `ibm-iks-cluster-autoscaler`.</td>
    <td>`300m`</td>
    </tr>
    <tr>
    <td>Parametro `resources.limits.memory`</td>
    <td>Imposta la quantità massima di memoria del nodo di lavoro che può essere consumata dal pod `ibm-iks-cluster-autoscaler`.</td>
    <td>`300Mi`</td>
    </tr>
    <tr>
    <td>Parametro `resources.requests.cpu`</td>
    <td>Imposta la quantità minima di CPU del nodo di lavoro con cui inizia il pod `ibm-iks-cluster-autoscaler`.</td>
    <td>`100m`</td>
    </tr>
    <tr>
    <td>Parametro `resources.requests.memory`</td>
    <td>Imposta la quantità minima di memoria del nodo di lavoro con cui inizia il pod `ibm-iks-cluster-autoscaler`.</td>
    <td>`100Mi`</td>
    </tr>
    <tr>
    <td>Parametro `scaleDownUnneededTime`</td>
    <td>Imposta la quantità di tempo in minuti in cui un nodo di lavoro non deve essere necessario prima che possa essere ridotto.</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>Parametri `scaleDownDelayAfterAdd`, `scaleDownDelayAfterDelete`</td>
    <td>Imposta la quantità di tempo in minuti che il cluster autoscaler attende per avviare nuovamente le azioni di ridimensionamento dopo l'ampliamento (`add`) o la riduzione (`delete`).</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>Parametro `scaleDownUtilizationThreshold`</td>
    <td>Imposta la soglia di utilizzo del nodo di lavoro. Se l'utilizzo del nodo di lavoro scende sotto la soglia, il nodo di lavoro viene considerato idoneo per la riduzione. L'utilizzo del nodo di lavoro viene calcolato come la somma delle risorse di CPU e memoria richieste da tutti i pod eseguiti sul nodo di lavoro divisa per la capacità delle risorse del nodo di lavoro.</td>
    <td>`0,5`</td>
    </tr>
    <tr>
    <td>Parametro `scanInterval`</td>
    <td>Imposta la frequenza in minuti in cui il cluster autoscaler esegue la scansione dell'utilizzo del carico di lavoro che attiva l'ampliamento o la riduzione.</td>
    <td>`1m`</td>
    </tr>
    <tr>
    <td>Parametro `skipNodes.withLocalStorage`</td>
    <td>Se impostato su `true`, i nodi di lavoro che dispongono di pod che stanno salvando i dati nell'archiviazione locale non vengono ridotti.</td>
    <td>`true`</td>
    </tr>
    <tr>
    <td>Parametro `skipNodes.withSystemPods`</td>
    <td>Se impostato su `true`, i nodi di lavoro che dispongono di pod `kube-system` non vengono ridotti. Non impostare il valore su `false` perché la riduzione dei pod `kube-system` potrebbe comportare risultati imprevisti.</td>
    <td>`true`</td>
    </tr>
    </tbody>
    </table>
2.  Per modificare uno qualsiasi dei valori di configurazione del cluster autoscaler, aggiorna il grafico Helm con i nuovi valori. Includi l'indicatore `--recreate-pods`, cosicché gli eventuali pod cluster autoscaler esistenti vengano ricreati per acquisire le modifiche alle impostazioni personalizzate.
    ```
    helm upgrade --set scanInterval=2m ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler -i --recreate-pods
    ```
    {: pre}

    Per ripristinare il grafico ai valori predefiniti:
    ```
    helm upgrade --reset-values ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler --recreate-pods
    ```
    {: pre}
3.  Per verificare le tue modifiche, esamina nuovamente i valori del grafico Helm.
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}


## Limitazione dell'esecuzione di applicazioni solo su determinati pool di nodi di lavoro ridimensionati automaticamente
{: #ca_limit_pool}

Per limitare una distribuzione di pod a uno specifico pool di nodi di lavoro gestito dal cluster autoscaler, utilizza le etichette e `nodeSelector` o `nodeAffinity`. Con `nodeAffinity`, hai più controllo sulla modalità di funzionamento della pianificazione per mettere in corrispondenza i pod con i nodi di lavoro. Per ulteriori informazioni sull'assegnazione di pod ai nodi di lavoro, [vedi la documentazione di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/).
{: shortdesc}

**Prima di iniziare**:
*  [Installa il plugin `ibm-iks-cluster-autoscaler`](#ca_helm).
*  [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**Per limitare l'esecuzione dei pod su determinati pool di nodi di lavoro ridimensionati automaticamente**:

1.  Crea il pool di nodi di lavoro con l'etichetta che vuoi utilizzare. Ad esempio, la tua etichetta potrebbe essere `app: nginx`.
    ```
    ibmcloud ks worker-pool-create --name <name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_worker_nodes> --labels <key>=<value>
    ```
    {: pre}
2.  [Aggiungi il pool di nodi di lavoro alla configurazione di cluster autoscaler](#ca_cm).
3.  Nel tuo template delle specifiche del pod, metti in corrispondenza `nodeSelector` o `nodeAffinity` con l'etichetta che hai utilizzato nel tuo pool di nodi di lavoro.

    Esempio di `nodeSelector`:
    ```
    ...
    spec:
          containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
      nodeSelector:
        app: nginx
    ...
    ```
    {: codeblock}

    Esempio di `nodeAffinity`:
    ```
    spec:
          containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: app
                  operator: In
                  values:
                - nginx
    ```
    {: codeblock}
4.  Distribuisci il pod. A causa dell'etichetta corrispondente, il pod viene pianificato su un nodo di lavoro che si trova nel pool di nodi di lavoro etichettato.
    ```
    kubectl apply -f pod.yaml
    ```
    {: pre}

<br />


## Ampliamento dei nodi di lavoro prima che il pool di nodi abbia risorse insufficienti
{: #ca_scaleup}

Come descritto nell'argomento [Informazioni su come funziona il cluster autoscaler](#ca_about) e nelle [domande frequenti (FAQ) su Kubernetes Cluster Autoscaler ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md), il cluster autoscaler amplia i tuoi pool di nodi di lavoro in risposta alle tue risorse richieste del carico di lavoro rispetto alle risorse disponibili del pool di nodi di lavoro  Tuttavia, potresti volere che il cluster autoscaler esegua l'ampliamento dei nodi di lavoro prima che il pool di nodi di lavoro esaurisca le risorse. In questo caso, il tuo carico di lavoro non deve attendere il provisioning dei nodi di lavoro perché il pool di nodi di lavoro è già stato ampliato per soddisfare le richieste di risorse.
{: shortdesc}

Il cluster autoscaler non supporta il ridimensionamento anticipato (overprovisioning) dei pool di nodi di lavoro. Tuttavia, puoi configurare altre risorse Kubernetes per lavorare con il cluster autoscaler per ottenere un ridimensionamento anticipato.

<dl>
  <dt><strong>Pod di pausa</strong></dt>
  <dd>Puoi creare una distribuzione che distribuisca i [contenitori di pausa ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://stackoverflow.com/questions/48651269/what-are-the-pause-containers) nei pod con richieste di risorse specifiche e assegnare alla distribuzione una priorità di pod bassa. Quando queste risorse sono necessarie per carichi di lavoro con priorità più elevata, il pod di pausa viene prerilasciato e diventa un pod in sospeso. Questo evento attiva il cluster autoscaler per eseguire l'ampliamento.<br><br>Per ulteriori informazioni sulla configurazione di una distribuzione di pod di pausa, vedi [FAQ Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-can-i-configure-overprovisioning-with-cluster-autoscaler). Puoi utilizzare [questo file di configurazione del provisioning in eccesso di esempio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/IBM-Cloud/kube-samples/blob/master/ibm-ks-cluster-autoscaler/overprovisioning-autoscaler.yaml) per creare la classe di priorità, l'account di servizio e le distribuzioni.<p class="note">Se utilizzi questo metodo, assicurati di comprendere il funzionamento della [priorità dei pod](/docs/containers?topic=containers-pod_priority#pod_priority) e come impostarla per le tue distribuzioni. Ad esempio, se il pod di pausa non dispone di risorse sufficienti per un pod con priorità più alta, il pod non viene prerilasciato. Il carico di lavoro con priorità più alta rimane in sospeso, quindi il cluster autoscaler viene attivato per l'ampliamento. Tuttavia, in questo caso, l'azione di ampliamento non è anticipata, poiché il carico di lavoro che desideri eseguire non può essere pianificato a causa delle risorse insufficienti.</p></dd>

  <dt><strong>Ridimensionamento automatico pod orizzontale (HPA)</strong></dt>
  <dd>Poiché il ridimensionamento automatico pod orizzontale si basa sull'utilizzo medio della CPU dei pod, il limite di utilizzo della CPU che hai impostato viene raggiunto prima che il pool di nodi di lavoro esaurisca le risorse. Sono richiesti più pod, il che attiva quindi il cluster autoscaler per l'ampliamento del pool di nodi di lavoro.<br><br>Per ulteriori informazioni sulla configurazione di HPA, consulta la [documentazione di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/).</dd>
</dl>

<br />


## Aggiornamento del grafico Helm del cluster autoscaler
{: #ca_helm_up}

Puoi aggiornare il grafico Helm esistente del cluster autoscaler alla versione più recente. Per verificare la tua versione corrente del grafico Helm, esegui `helm ls | grep cluster-autoscaler`.
{: shortdesc}

Vuoi eseguire l'aggiornamento al grafico Helm più recente dalla versione 1.0.2 o precedenti? [Segui queste istruzioni](#ca_helm_up_102).
{: note}

Prima di iniziare: [accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Aggiorna il repository Helm per richiamare la versione più recente di tutti i grafici Helm in questo repository.
    ```
    helm repo update
    ```
    {: pre}

2.  Facoltativo: scarica il grafico helm più recente sulla tua macchina locale. Estrai quindi il pacchetto ed esamina il file `release.md` per trovare le informazioni di release più aggiornate.
    ```
    helm fetch iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

3.  Trova il nome del grafico Helm del cluster autoscaler che hai installato nel tuo cluster.
    ```
    helm ls | grep cluster-autoscaler
    ```
    {: pre}

    Output di esempio:
    ```
    myhelmchart 	1       	Mon Jan  7 14:47:44 2019	DEPLOYED	ibm-ks-cluster-autoscaler-1.0.0  	kube-system
    ```
    {: screen}

4.  Aggiorna il grafico Helm del cluster autoscaler alla versione più recente.
    ```
    helm upgrade --force --recreate-pods <helm_chart_name>  iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

5.  Verifica che la sezione `workerPoolsConfig.json` della [mappa di configurazione del cluster autoscaler](#ca_cm) sia impostata su `"enabled": true` per i pool di nodi di lavoro che vuoi ridimensionare.
    ```
    kubectl describe cm iks-ca-configmap -n kube-system
    ```
    {: pre}

    Output di esempio:
    ```
    Name:         iks-ca-configmap
    Namespace:    kube-system
    Labels:       <none>
    Annotations:  kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","data":{"workerPoolsConfig.json":"[\n {\"name\": \"docs\",\"minSize\": 1,\"maxSize\": 3,\"enabled\":true}\n]\n"},"kind":"ConfigMap",...

    Data
    ====
    workerPoolsConfig.json:
    ----
    [
     {"name": "docs","minSize": 1,"maxSize": 3,"enabled":true}
    ]

    Events:  <none>
    ```
    {: screen}

### Aggiornamento al grafico Helm più recente dalla versione 1.0.2 o precedenti
{: #ca_helm_up_102}

La versione più recente del grafico Helm del cluster autoscaler richiede una rimozione completa delle versioni precedentemente installate. Se hai installato il grafico Helm versione 1.0.2 o precedenti, disinstalla tale versione prima di installare l'ultimo grafico Helm del cluster autoscaler.
{: shortdesc}

Prima di iniziare: [accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Ottieni la mappa di configurazione del cluster autoscaler.
    ```
    kubectl get cm iks-ca-configmap -n kube-system -o yaml > iks-ca-configmap.yaml
    ```
    {: pre}
2.  Rimuovi tutti i pool di nodi di lavoro dalla mappa di configurazione impostando il valore `"enabled"` su `false`.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
3.  Se hai applicato le impostazioni personalizzate al grafico Helm, prendi nota delle tue impostazioni personalizzate.
    ```
    helm get values ibm-ks-cluster-autoscaler -a
    ```
    {: pre}
4.  Disinstalla il tuo grafico Helm corrente.
    ```
    helm delete --purge ibm-ks-cluster-autoscaler
    ```
    {: pre}
5.  Aggiorna il repository di grafici Helm per ottenere la versione più recente per il grafico Helm del cluster autoscaler.
    ```
    helm repo update
    ```
    {: pre}
6.  Installa l'ultimo grafico Helm del cluster autoscaler. Applica eventuali impostazioni personalizzate che hai utilizzato in precedenza con l'indicatore `--set`, ad esempio `scanInterval=2m`.
    ```
    helm install  iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler [--set <custom_settings>]
    ```
    {: pre}
7.  Applica la mappa di configurazione del cluster autoscaler che hai precedentemente richiamato per abilitare il ridimensionamento automatico per i tuoi pool di nodi di lavoro.
    ```
    kubectl apply -f iks-ca-configmap.yaml
    ```
    {: pre}
8.  Ottieni il pod del cluster autoscaler.
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
9.  Esamina la sezione **`Events`** del pod del cluster autoscaler e cerca un evento **`ConfigUpdated`** per verificare che la mappa di configurazione sia stata aggiornata correttamente. Il formato del messaggio di evento della tua mappa di configurazione è il seguente: `minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`.
    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    Output di esempio:
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
    Type    Reason                 Age   From                     Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

<br />


## Rimozione del cluster autoscaler
{: #ca_rm}

Se non vuoi ridimensionare automaticamente i tuoi pool di nodi di lavoro, puoi disinstallare il grafico Helm del cluster autoscaler. Dopo la rimozione, devi [ridimensionare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) o [ribilanciare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) automaticamente i tuoi pool di nodi di lavoro.
{: shortdesc}

Prima di iniziare: [accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Nella [mappa di configurazione del cluster autoscaler](#ca_cm), rimuovi il pool di nodi di lavoro impostando il valore `"enabled"` su `false`.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
    Output di esempio:
    ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
         {"name":"mypool","minSize":1,"maxSize":5,"enabled":false}
        ]
    kind: ConfigMap
    ...
    ```
2.  Elenca i tuoi grafici Helm esistenti e prendi nota del nome del cluster autoscaler.
    ```
    helm ls
    ```
    {: pre}
3.  Rimuovi il grafico Helm esistente dal tuo cluster.
    ```
    helm delete --purge <ibm-iks-cluster-autoscaler_name>
    ```
    {: pre}
