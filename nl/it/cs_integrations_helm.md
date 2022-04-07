---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, helm, without tiller, private cluster tiller, integrations, helm chart

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



# Aggiunta di servizi attraverso i grafici Helm
{: #helm}

Puoi aggiungere applicazioni Kubernetes complesse al tuo cluster attraverso i grafici Helm.
{: shortdesc}

**Cos'è Helm e come lo uso?** </br>
[Helm ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://helm.sh) è un gestore pacchetti di Kubernetes che utilizza i grafici Helm per definire, installare e aggiornare applicazioni Kubernetes nel tuo cluster. I grafici Helm comprimono le specifiche per generare file YAML per le risorse Kubernetes che creano la tua applicazione. Queste risorse Kubernetes vengono applicate automaticamente nel tuo cluster e viene assegnata loro una versione da Helm. Puoi anche utilizzare Helm per specificare e comprimere la tua applicazione e permettere a Helm di generare i file YAML per le tue risorse Kubernetes.  

Per utilizzare Helm nel tuo cluster, devi installare la CLI Helm sulla tua macchina locale e il server Helm Tiller in ciascun cluster in cui intendi utilizzare Helm.

**Quali grafici Helm sono supportati in {{site.data.keyword.containerlong_notm}}?** </br>
Per una panoramica dei grafici Helm disponibili, vedi il [Catalogo dei grafici Helm ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/kubernetes/solutions/helm-charts). I grafici Helm elencati in questo catalogo sono raggruppati come segue:

- **iks-charts**: grafici Helm approvati per {{site.data.keyword.containerlong_notm}}. Il nome di questo repository è stato modificato da `ibm` a `iks-charts`.
- **ibm-charts**: grafici Helm approvati per i cluster privati {{site.data.keyword.containerlong_notm}} e
{{site.data.keyword.Bluemix_notm}}.
- **kubernetes**: grafici Helm forniti dalla community Kubernetes e considerati `stable` dalla governance della community. L'utilizzo di questi grafici nei cluster privati {{site.data.keyword.containerlong_notm}} o {{site.data.keyword.Bluemix_notm}} non è verificato.
- **kubernetes-incubator**: grafici Helm forniti dalla community Kubernetes e considerati `incubator` dalla governance della community. L'utilizzo di questi grafici nei cluster privati {{site.data.keyword.containerlong_notm}} o {{site.data.keyword.Bluemix_notm}} non è verificato.

I grafici Helm dei repository **iks-charts** e **ibm-charts** sono pienamente integrati nell'organizzazione di supporto {{site.data.keyword.Bluemix_notm}}. Se hai domande o riscontri un problema nell'utilizzo dei grafici Helm, puoi utilizzare uno dei canali di supporto {{site.data.keyword.containerlong_notm}}. Per ulteriori informazioni, vedi [Come ottenere aiuto e supporto](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

**Quali sono i prerequisiti per l'utilizzo di Helm? Posso utilizzare Helm in un cluster privato?** </br>
Per distribuire i grafici Helm, devi installare la CLI Helm sulla tua macchina locale e installare il server Helm Tiller nel tuo cluster. L'immagine per Tiller è memorizzata nel Google Container Registry pubblico. Per accedere all'immagine durante l'installazione di Tiller, il tuo cluster deve consentire la connettività di rete pubblica al Google Container Registry pubblico. I cluster che abilitano l'endpoint del servizio pubblico possono accedere automaticamente all'immagine. I cluster privati protetti con un firewall personalizzato, o i cluster che hanno abilitato solo l'endpoint del servizio privato, non consentono l'accesso all'immagine Tiller. Puoi invece [estrarre l'immagine nella tua macchina locale e inviarla al tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}}](#private_local_tiller) oppure [installare i grafici Helm senza utilizzare Tiller](#private_install_without_tiller).


## Configurazione di Helm in un cluster con accesso pubblico
{: #public_helm_install}

Se il tuo cluster ha abilitato l'endpoint del servizio pubblico, puoi installare il server Helm Tiller attraverso l'immagine pubblica memorizzata in Google Container Registry.
{: shortdesc}

Prima di iniziare:
- [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Per installare Tiller con un account di servizio Kubernetes e il bind del ruolo cluster nello spazio dei nomi `kube-system`, accertati di avere il [ruolo `cluster-admin`](/docs/containers?topic=containers-users#access_policies).

Per installare Helm in un cluster con accesso pubblico:

1. Installa la <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI Helm <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> sulla tua macchina locale.

2. Controlla se hai già installato Tiller con un account di servizio Kubernetes nel tuo cluster.
   ```
   kubectl get serviceaccount --all-namespaces | grep tiller
   ```
   {: pre}

   Output di esempio con Tiller installato:
   ```
   kube-system      tiller                               1         189d
   ```
   {: screen}

   L'output di esempio include lo spazio dei nomi Kubernetes e il nome dell'account di servizio per Tiller. Se Tiller non è installato con un account di servizio nel tuo cluster, non viene restituito alcun output della CLI.

3. **Importante**: per preservare la sicurezza del cluster, configura Tiller con un account di servizio e un bind del ruolo cluster nel tuo cluster.
   - **Se Tiller viene installato con un account di servizio:**
     1. Crea un bind del ruolo cluster per l'account di servizio Tiller. Sostituisci `<namespace>` con lo spazio dei nomi in cui Tiller è installato nel tuo cluster.
        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=<namespace>:tiller -n <namespace>
        ```
        {: pre}

     2. Aggiorna Tiller. Sostituisci `<tiller_service_account_name>` con il nome dell'account di servizio Kubernetes per Tiller che hai richiamato nel passo precedente.
        ```
        helm init --upgrade --service-account <tiller_service_account_name>
        ```
        {: pre}

     3. Verifica che il pod `tiller-deploy` abbia uno **Status** di `Running` nel tuo cluster.
        ```
        kubectl get pods -n <namespace> -l app=helm
        ```
        {: pre}

        Output di esempio:

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

   - **Se Tiller non è installato con un account di servizio:**
     1. Crea un account di servizio Kubernetes e un bind del ruolo cluster per Tiller nello spazio dei nomi `kube-system` del tuo cluster.
        ```
        kubectl create serviceaccount tiller -n kube-system
        ```
        {: pre}

        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=kube-system:tiller -n kube-system
        ```
        {: pre}

     2. Verifica che l'account di servizio Tiller sia stato creato.
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

     3. Inizializza la CLI di Helm e installa Tiller nel tuo cluster con l'account di servizio che hai creato.
        ```
        helm init --service-account tiller
        ```
        {: pre}

     4. Verifica che il pod `tiller-deploy` abbia uno **Status** di `Running` nel tuo cluster.
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        Output di esempio:
        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

4. Aggiungi i repository Helm {{site.data.keyword.Bluemix_notm}} alla tua istanza Helm.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

5. Aggiorna i repository per richiamare le versioni più recenti di tutti i grafici Helm.
   ```
   helm repo update
   ```
   {: pre}

6. Elenca i grafici Helm attualmente disponibili nei repository {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

7. Identifica il grafico Helm che vuoi installare e segui le istruzioni nel file `README` del grafico Helm per installarlo nel tuo cluster.


## Cluster privati: esecuzione del push dell'immagine di Tiller nel tuo spazio dei nomi in IBM Cloud Container Registry
{: #private_local_tiller}

Puoi estrarre l'immagine di Tiller nella tua macchina locale, eseguirne il push nel tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}} e installare Tiller nel tuo cluster privato utilizzando l'immagine archiviata in {{site.data.keyword.registryshort_notm}}.
{: shortdesc}

Se desideri installare un grafico Helm senza utilizzare Tiller, vedi [Cluster privati: installazione dei grafici Helm senza utilizzare Tiller](#private_install_without_tiller).
{: tip}

Prima di iniziare:
- Installa Docker sulla tua macchina locale. Se hai installato la [CLI {{site.data.keyword.Bluemix_notm}}](/docs/cli?topic=cloud-cli-getting-started), Docker è già installato.
- [Installa il plugin della CLI {{site.data.keyword.registryshort_notm}} e configura uno spazio dei nomi](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install).
- Per installare Tiller con un account di servizio Kubernetes e il bind del ruolo cluster nello spazio dei nomi `kube-system`, accertati di avere il [ruolo `cluster-admin`](/docs/containers?topic=containers-users#access_policies).

Per installare Tiller utilizzando {{site.data.keyword.registryshort_notm}}:

1. Installa la <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI Helm <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> sulla tua macchina locale.
2. Connettiti al tuo cluster privato utilizzando il tunnel VPN dell'infrastruttura {{site.data.keyword.Bluemix_notm}} che hai configurato.
3. **Importante**: per mantenere la sicurezza del cluster, crea un account di servizio per Tiller nello spazio dei nomi `kube-system` e un bind del ruolo cluster RBAC Kubernetes per il pod `tiller-deploy` applicando il seguente file YAML dal [repository {{site.data.keyword.Bluemix_notm}} `kube-samples`](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml).
    1. [Ottieni il file YALM per l'account di servizio e il bind del ruolo cluster di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml).

    2. Crea le risorse Kubernetes nel tuo cluster.
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [Trova la versione di Tiller ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30) che vuoi installare nel tuo cluster. Se non hai bisogno di una versione specifica, usa quella più recente.

5. Estrai l'immagine Tiller da Google Container Registry nella tua macchina locale.
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   Output di esempio:
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [Invia l'immagine Tiller al tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing).

7. Per accedere all'immagine in {{site.data.keyword.registryshort_notm}} dal tuo cluster,[copia il segreto di pull dell'immagine dallo spazio dei nomi predefinito allo
spazio dei nomi kube-system](/docs/containers?topic=containers-images#copy_imagePullSecret).

8. Installa Tiller nel tuo cluster privato utilizzando l'immagine che hai memorizzato nel tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}}.
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. Aggiungi i repository Helm {{site.data.keyword.Bluemix_notm}} alla tua istanza Helm.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

10. Aggiorna i repository per richiamare le versioni più recenti di tutti i grafici Helm.
    ```
    helm repo update
    ```
    {: pre}

11. Elenca i grafici Helm attualmente disponibili nei repository {{site.data.keyword.Bluemix_notm}}.
    ```
    helm search iks-charts
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. Identifica il grafico Helm che vuoi installare e segui le istruzioni nel file `README` del grafico Helm per installarlo nel tuo cluster.


## Cluster privati: installazione dei grafici Helm senza utilizzare Tiller
{: #private_install_without_tiller}

Se non vuoi installare Tiller nel tuo cluster privato, puoi creare manualmente i file YAML del grafico Helm e applicarli utilizzando i comandi `kubectl`.
{: shortdesc}

I passi in questo esempio mostrano come installare i grafici Helm dai repository di grafici Helm {{site.data.keyword.Bluemix_notm}} nel tuo cluster privato. Se vuoi installare un grafico Helm che non è memorizzato in uno dei repository di grafici Helm {{site.data.keyword.Bluemix_notm}}, devi seguire le istruzioni presentate in questo argomento per creare i file YAML per il tuo grafico. Inoltre, devi scaricare l'immagine del grafico Helm dal registro del contenitore pubblico, estrarla nel tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}} e aggiornare il file `values.yaml` per utilizzare l'immagine in {{site.data.keyword.registryshort_notm}}.
{: note}

1. Installa la <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI Helm <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> sulla tua macchina locale.
2. Connettiti al tuo cluster privato utilizzando il tunnel VPN dell'infrastruttura {{site.data.keyword.Bluemix_notm}} che hai configurato.
3. Aggiungi i repository Helm {{site.data.keyword.Bluemix_notm}} alla tua istanza Helm.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

4. Aggiorna i repository per richiamare le versioni più recenti di tutti i grafici Helm.
   ```
   helm repo update
   ```
   {: pre}

5. Elenca i grafici Helm attualmente disponibili nei repository {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. Identifica il grafico Helm che vuoi installare, scarica il grafico Helm nella tua macchina locale e decomprimi i file del tuo grafico Helm. Il seguente esempio mostra come scaricare il grafico Helm per il cluster autoscaler versione 1.0.3 e decomprimere i file in una directory `cluster-autoscaler`.
   ```
   helm fetch iks-charts/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Passa alla directory in cui hai decompresso i file del grafico Helm.
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Crea una directory `output` per i file YAML che generi utilizzando i file nel tuo grafico Helm.
   ```
   mkdir output
   ```
   {: pre}

9. Apri il file `values.yaml` e apporta tutte le modifiche richieste dalle istruzioni di installazione del grafico Helm.
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. Utilizza la tua installazione Helm locale per creare tutti i file YALM di Kubernetes per il tuo grafico Helm. I file YAML vengono memorizzati nella directory `output` che hai creato in precedenza.
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Output di esempio:
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. Distribuisci tutti i file YAML nel tuo cluster privato.
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. Facoltativo: rimuovi tutti i file YAML dalla directory `output`.
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}

## Link Helm correlati
{: #helm_links}

Consulta i seguenti link per ulteriori informazioni su Helm.
{: shortdesc}

* Visualizza i grafici Helm disponibili che puoi utilizzare in {{site.data.keyword.containerlong_notm}} nel [Catalogo dei grafici Helm![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/kubernetes/solutions/helm-charts).
* Ulteriori informazioni sui comandi Helm che puoi utilizzare per configurare e gestire i grafici Helm sono disponibili nella <a href="https://docs.helm.sh/helm/" target="_blank">documentazione di Helm <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.
* Ulteriori informazioni su come puoi [aumentare la velocità di distribuzione con i grafici Helm Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).
