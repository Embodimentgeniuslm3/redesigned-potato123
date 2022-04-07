---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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



# Aggiunta di servizi attraverso il bind del servizio IBM Cloud
{: #service-binding}

Aggiungi i servizi {{site.data.keyword.Bluemix_notm}} per migliorare il tuo cluster Kubernetes con funzionalità supplementari in aree quali l'intelligenza artificiale Watson, i dati, la sicurezza e IoT (Internet of Things).
{:shortdesc}

**Per quali tipi di servizi posso eseguire il bind al mio cluster?** </br>
Quando aggiungi servizi {{site.data.keyword.Bluemix_notm}} al tuo cluster, puoi scegliere tra quelli che supportano {{site.data.keyword.Bluemix_notm}} IAM (Identity and Access Management) e quelli basati su Cloud Foundry. I servizi che supportano IAM offrono un controllo di accesso più granulare e possono essere gestiti in un gruppo di risorse {{site.data.keyword.Bluemix_notm}}. I servizi Cloud Foundry devono essere aggiunti a un'organizzazione e uno spazio Cloud Foundry e non possono essere aggiunti a un gruppo di risorse. Per controllare l'accesso alla tua istanza di servizio Cloud Foundry, utilizzi ruoli Cloud Foundry. Per ulteriori informazioni sui servizi che supportano IAM e sui servizi Cloud Foundry, vedi [Che cosa è una risorsa?](/docs/resources?topic=resources-resource#resource).

Per trovare un elenco dei servizi {{site.data.keyword.Bluemix_notm}} supportati, vedi il [catalogo {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/catalog).

**Cosa è un bind del servizio {{site.data.keyword.Bluemix_notm}}?**</br>
Il bind del servizio ti consente di creare rapidamente le credenziali per un servizio {{site.data.keyword.Bluemix_notm}} e archiviarle in un segreto Kubernetes nel tuo cluster. Per eseguire il bind di un servizio al tuo cluster, devi innanzitutto eseguire il provisioning di un'istanza del servizio. Quindi, utilizzi il [comando `ibmcloud ks cluster-service-bind`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind) per creare le credenziali del servizio e il segreto Kubernetes. Il segreto Kubernetes viene crittografato automaticamente in etcd per proteggere i tuoi dati.

Vuoi rendere i tuoi segreti ancora più sicuri? Chiedi al tuo amministratore cluster di [abilitare {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) nel tuo cluster per crittografare i segreti nuovi ed esistenti, come ad esempio il segreto che memorizza le credenziali delle tue istanze del servizio {{site.data.keyword.Bluemix_notm}}.
{: tip}

**Posso utilizzare tutti i servizi {{site.data.keyword.Bluemix_notm}} nel mio cluster?**</br>
Puoi utilizzare il bind dei servizi solo per i servizi che supportano chiavi del servizio, cosicché le credenziali del servizio possano essere create e archiviate automaticamente in un segreto
Kubernetes. Per trovare un elenco dei servizi che supportano le chiavi del servizio, vedi [Abilitazione di applicazioni esterne a utilizzare servizi {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).

I servizi che non supportano le chiavi del servizio di solito forniscono un'API che puoi utilizzare nella tua applicazione. Il metodo di bind del servizio non configura automaticamente l'accesso API per la tua applicazione. Assicurati di rivedere la documentazione API del tuo servizio e implementare l'interfaccia API nella tua applicazione.

## Aggiunta dei servizi di IBM Cloud ai cluster
{: #bind-services}

Utilizza il bind del servizio {{site.data.keyword.Bluemix_notm}} per creare automaticamente credenziali del servizio per i tuoi servizi {{site.data.keyword.Bluemix_notm}} e archiviarle in un segreto Kubernetes.
{: shortdesc}

Prima di iniziare:
- Assicurati di avere i seguenti ruoli:
    - [Ruolo di accesso alla piattaforma {{site.data.keyword.Bluemix_notm}} IAM
Editor o Amministratore](/docs/containers?topic=containers-users#platform) per il cluster in cui desideri eseguire il bind di un servizio
    - [Ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Scrittore** o **Gestore**](/docs/containers?topic=containers-users#platform) per lo spazio dei nomi Kubernetes in cui desideri eseguire il bind del servizio
    - Per i servizi Cloud Foundry: [ruolo Cloud Foundry **Sviluppatore**](/docs/iam?topic=iam-mngcf#mngcf) per lo spazio in cui desideri eseguire il provisioning del servizio
- [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Per aggiungere un servizio {{site.data.keyword.Bluemix_notm}} al tuo cluster:

1. [Crea un'istanza del servizio {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).
    * Alcuni servizi {{site.data.keyword.Bluemix_notm}} sono disponibili solo in determinate regioni. Puoi eseguire il bind di un servizio al tuo cluster solo se il servizio è disponibile nella stessa regione del tuo cluster. Inoltre, se vuoi creare un'istanza del servizio nella zona Washington DC, devi utilizzare la CLI.
    * **Per i servizi che supportano IAM**: devi creare l'istanza del servizio nello stesso gruppo di risorse del tuo cluster. Un servizio può essere creato in un solo gruppo di risorse che non puoi modificare in seguito.

2. Controlla il tipo di servizio che hai creato e prendi nota del nome (**Name**) dell'istanza del servizio.
   - **Servizi Cloud Foundry:**
     ```
     ibmcloud service list
     ```
     {: pre}

     Output di esempio:
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **Servizi abilitati a {{site.data.keyword.Bluemix_notm}} IAM:**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}

     Output di esempio:
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   Puoi anche vedere i diversi tipi di servizio nel tuo dashboard {{site.data.keyword.Bluemix_notm}} come **Servizi Cloud Foundry** e **Servizi**.

3. Identifica lo spazio dei nomi del cluster che desideri utilizzare per l'aggiunta del tuo servizio.
   ```
   kubectl get namespaces
   ```
   {: pre}

4. Esegui il bind del servizio per il tuo cluster, al fine di creare le credenziali del tuo servizio e archiviarle in un segreto Kubernetes. Se disponi di credenziali del servizio esistenti, utilizza l'indicatore `--key` per specificarle. Per i servizi che supportano IAM, le credenziali vengono create automaticamente con il ruolo di accesso al servizio **Scrittore**, ma puoi utilizzare
l'indicatore `--role` per specificarne uno differente. Se utilizzi l'indicatore `--key`, non includere l'indicatore `--role`.
   ```
   ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
   ```
   {: pre}

   Quando la creazione delle credenziali del servizio ha esito positivo, viene creato un segreto Kubernetes denominato `binding-<service_instance_name>`.  

   Output di esempio:
   ```
   ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
   ```
   {: screen}

5. Verifica le credenziali del servizio nel tuo segreto Kubernetes.
   1. Ottieni i dettagli del segreto e prendi nota del valore **binding**. Il valore **binding** ha una codifica base64 e contiene le credenziali per la tua istanza del servizio in formato JSON.
      ```
      kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
      ```
      {: pre}

      Output di esempio:
      ```
      apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
      ```
      {: screen}

   2. Decodifica il valore di bind.
      ```
      echo "<binding>" | base64 -D
      ```
      {: pre}

      Output di esempio:
      ```
      {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

   3. Facoltativo: confronta le credenziali del servizio che hai decodificato nel passo precedente con le credenziali del servizio che trovi per la tua istanza del servizio nel dashboard {{site.data.keyword.Bluemix_notm}}.

6. Ora che è stato eseguito il bind del tuo servizio al tuo cluster, devi configurare la tua applicazione per [accedere alle credenziali del servizio nel segreto Kubernetes](#adding_app).


## Accesso alle credenziali del servizio dalle tue applicazioni
{: #adding_app}

Per accedere ad un'istanza del servizio {{site.data.keyword.Bluemix_notm}} dalla tua applicazione, devi rendere le credenziali del servizio archiviate nel segreto di Kubernetes disponibili per la tua applicazione.
{: shortdesc}

Le credenziali di un'istanza del servizio hanno una codifica base64 e sono archiviate all'interno del tuo segreto in formato JSON. Per accedere ai dati nel tuo segreto, scegli tra le seguenti opzioni:
- [Monta il segreto come un volume al tuo pod](#mount_secret)
- [Fai riferimento al segreto nelle variabili di ambiente](#reference_secret)
<br>

Prima di iniziare:
-  Assicurati di disporre del [ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Scrittore** o **Gestore**](/docs/containers?topic=containers-users#platform) per lo spazio dei nomi `kube-system`.
- [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- [Aggiungi un servizio {{site.data.keyword.Bluemix_notm}} al tuo cluster](#bind-services).

### Montaggio del segreto come un volume al tuo pod
{: #mount_secret}

Quando monti un segreto come un volume al tuo pod, un file denominato `binding` viene archiviato nella directory di montaggio del volume. Il file `binding` in formato JSON include tutte le informazioni e le credenziali di cui hai bisogno per accedere al servizio {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

1.  Elenca i segreti disponibili nel tuo cluster e prendi nota del **nome** del tuo segreto. Cerca un segreto di tipo **Opaque**. Se sono presenti più segreti, contatta il tuo amministratore cluster per identificare il segreto del servizio
corretto.
    ```
    kubectl get secrets
    ```
    {: pre}

    Output di esempio:
    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
    ```
    {: screen}

2.  Crea un file YAML per la tua distribuzione Kubernetes e monta il segreto come un volume al tuo pod.
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: icr.io/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>Descrizione dei componenti del file YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>Il percorso assoluto della directory in cui viene montato il volume nel contenitore.</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>Il nome del volume per montare il tuo pod.</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>Le autorizzazioni di lettura e scrittura sul segreto. Utilizza `420` per impostare le autorizzazioni di sola lettura. </td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>Il nome del segreto di cui hai preso nota nel passo precedente.</td>
    </tr></tbody></table>

3.  Crea il pod e monta il segreto come un volume.
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  Verifica che il pod sia stato creato.
    ```
    kubectl get pods
    ```
    {: pre}

    Output CLI di esempio:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  Accedi alle credenziali del servizio.
    1. Accedi al tuo pod.
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. Vai al tuo percorso di montaggio del volume che hai definito in precedenza ed elenca i file in esso contenuti.
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       Output di esempio:
       ```
       binding
       ```
       {: screen}

       Il file `binding` include le credenziali del servizio che hai archiviato nel segreto Kubernetes.

    4. Visualizza le credenziali del servizio. Le credenziali vengono archiviate come coppie chiave-valore in formato JSON.
       ```
       cat binding
       ```
       {: pre}

       Output di esempio:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. Configura la tua applicazione per analizzare il contenuto JSON e richiamare le informazioni di cui hai bisogno per accedere al tuo servizio.

### Riferimento al segreto nelle variabili di ambiente
{: #reference_secret}

Puoi aggiungere le credenziali del servizio e altre coppie chiave-valore dal tuo segreto Kubernetes come variabili di ambiente alla tua distribuzione.
{: shortdesc}

1. Elenca i segreti disponibili nel tuo cluster e prendi nota del **nome** del tuo segreto. Cerca un segreto di tipo **Opaque**. Se sono presenti più segreti, contatta il tuo amministratore cluster per identificare il segreto del servizio
corretto.
   ```
   kubectl get secrets
   ```
   {: pre}

   Output di esempio:
   ```
   NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
   ```
   {: screen}

2. Ottieni i dettagli del tuo segreto per trovare le potenziali coppie chiave-valore a cui puoi fare riferimento come variabili di ambiente nel tuo pod. Le credenziali del servizio sono archiviate nella chiave `binding` del tuo segreto.
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   Output di esempio:
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Crea un file YAML per la tua distribuzione Kubernetes e specifica una variabile di ambiente che fa riferimento alla chiave `binding`.
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: icr.io/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>Descrizione dei componenti del file YAML</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>Il nome della tua variabile di ambiente.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>Il nome del segreto di cui hai preso nota nel passo precedente.</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>La chiave che fa parte del tuo segreto e a cui vuoi fare riferimento nella tua variabile di ambiente. Per fare riferimento alle credenziali del servizio, devi utilizzare la chiave <strong>binding</strong> .  </td>
     </tr>
     </tbody></table>

4. Crea il pod che fa riferimento alla chiave `binding` del tuo segreto come una variabile di ambiente.
   ```
   kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. Verifica che il pod sia stato creato.
   ```
   kubectl get pods
   ```
   {: pre}

   Output CLI di esempio:
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. Verifica che la variabile di ambiente sia impostata correttamente.
   1. Accedi al tuo pod.
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. Elenca tutte le variabili di ambiente nel pod.
      ```
      env
      ```
      {: pre}

      Output di esempio:
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. Configura la tua applicazione per leggere la variabile di ambiente e per analizzare il contenuto JSON per richiamare le informazioni di cui hai bisogno per accedere al tuo servizio.

   Codice di esempio in Python:
   ```python
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

8. Facoltativo: come precauzione, aggiungi la gestione degli errori alla tua applicazione nel caso in cui la variabile di ambiente `BINDING` non sia impostata correttamente.

   Codice di esempio in Java:
   ```java
   if (System.getenv("BINDING") == null) {
    throw new RuntimeException("Environment variable 'SECRET' is not set!");
   }
   ```
   {: codeblock}

   Codice di esempio in Node.js:
   ```js
   if (!process.env.BINDING) {
    console.error('ENVIRONMENT variable "BINDING" is not set!');
    process.exit(1);
   }
   ```
   {: codeblock}
