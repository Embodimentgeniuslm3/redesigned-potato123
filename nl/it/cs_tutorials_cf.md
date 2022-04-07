---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# Esercitazione: Migrazione di un'applicazione da Cloud Foundry a un cluster
{: #cf_tutorial}

Puoi prendere un'applicazione che hai precedentemente distribuito utilizzando Cloud Foundry e distribuire lo stesso codice presente in un contenitore in un cluster Kubernetes in {{site.data.keyword.containerlong_notm}}.
{: shortdesc}


## Obiettivi
{: #cf_objectives}

- Imparare il processo generale di distribuzione delle applicazioni presenti nei contenitori in un cluster Kubernetes.
- Creare un Dockerfile dal tuo codice applicazione per creare un'immagine del contenitore.
- Distribuire un contenitore proveniente da tale immagine in un cluster Kubernetes.

## Tempo richiesto
{: #cf_time}

30 minuti

## Destinatari
{: #cf_audience}

Questa esercitazione è progettata per gli sviluppatori dell'applicazione Cloud Foundry.

## Prerequisiti
{: #cf_prereqs}

- [Crea un registro delle immagini privato in {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-getting-started).
- [Crea un
cluster](/docs/containers?topic=containers-clusters#clusters_ui).
- [Indirizza la tua CLI al cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- Assicurati di disporre delle seguenti politiche di accesso {{site.data.keyword.Bluemix_notm}} IAM per {{site.data.keyword.containerlong_notm}}:
    - [Qualsiasi ruolo della piattaforma](/docs/containers?topic=containers-users#platform)
    - Il ruolo del servizio [**Scrittore** o **Gestore**](/docs/containers?topic=containers-users#platform)
- [Acquisisci informazioni sulla terminologia Docker e Kubernetes](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology).


<br />



## Lezione 1: Scarica il codice dell'applicazione
{: #cf_1}

Tieni il tuo codice pronto per l'utilizzo. Non hai ancora il codice? Puoi scaricare il codice iniziale da utilizzare in questa esercitazione.
{: shortdesc}

1. Crea una directory denominata `cf-py` e passa ad essa. In questa directory, salva tutti i file richiesti per creare l'immagine Docker e per eseguire la tua applicazione.

  ```
  mkdir cf-py && cd cf-py
  ```
  {: pre}

2. Copia il codice dell'applicazione e tutti i file correlati nella directory. Puoi utilizzare il tuo codice oppure scaricare il contenitore tipo dal catalogo. Questa esercitazione utilizza il contenitore tipo Python Flask. Tuttavia, puoi utilizzare la stessa procedura di base con un'applicazione Node.js, Java o [Kitura](https://github.com/IBM-Cloud/Kitura-Starter).

    Per scaricare il codice dell'applicazione Python Flask:

    a. Nel catalogo, in **Boilerplates**, fai clic su **Python Flask**. Questo contenitore tipo include un ambiente di runtime per le applicazioni Python 2 e Python 3.

    b. Immetti il nome applicazione `cf-py-<name>` e fai clic su **CREA**. Per accedere al codice dell'applicazione per il contenitore tipo, devi innanzitutto distribuire l'applicazione CF nel cloud. Puoi utilizzare qualsiasi nome per l'applicazione. Se utilizzi il nome dall'esempio, sostituisci `<name>` con un identificativo univoco, come ad esempio `cf-py-msx`.

    **Attenzione**: non utilizzare informazioni personali nei nomi dell'applicazione, del contenitore delle immagini o della risorsa Kubernetes.

    Quando viene distribuita l'applicazione, vengono visualizzate le istruzioni per scaricare, modificare e ridistribuire la tua applicazione con l'interfaccia riga di comando.

    c. Dal passo 1 nelle istruzioni della console, fai clic su **DOWNLOAD STARTER CODE**.

    d. Estrai il file `.zip` e salvane il contenuto nella tua directory `cf-py`.

Il tuo codice applicazione è pronto per essere inserito nel contenitore!


<br />



## Lezione 2: Creazione di un'immagine Docker con il tuo codice applicazione
{: #cf_2}

Crea un Dockerfile che includa il tuo codice applicazione e le configurazioni necessarie per il tuo contenitore. Quindi, crea un'immagine Docker da tale Dockerfile e inseriscila nel tuo registro delle immagini privato.
{: shortdesc}

1. Nella directory `cf-py` che hai creato nella lezione precedente, crea un `Dockerfile`, ossia la base per la creazione dell'immagine di un contenitore. Puoi creare il Dockerfile utilizzando il tuo editor CLI preferito o un editor di testo sul tuo
computer. Il seguente esempio mostra come creare un file Dockerfile con l'editor nano.

  ```
  Dockerfile nano
  ```
  {: pre}

2. Copia il seguente script nel Dockerfile. Questo Dockerfile si applica in modo specifico ad un'applicazione Python. Se stai utilizzando un altro tipo di codice, il tuo Dockerfile deve includere un'immagine di base diversa e potrebbe richiedere la definizione di altri campi.

  ```
  #Use the Python image from DockerHub as a base image
  FROM python:3-slim

  #Expose the port for your python app
  EXPOSE 5000

  #Copy all app files from the current directory into the app
  #directory in your container. Set the app directory
  #as the working directory
  WORKDIR /cf-py/
  COPY .  .

  #Install any requirements that are defined
  RUN pip install --no-cache-dir -r requirements.txt

  #Update the openssl package
  RUN apt-get update && apt-get install -y \
  openssl

  #Start the app.
  CMD ["python", "welcome.py"]
  ```
  {: codeblock}

3. Salva le modifiche nell'editor nano premendo `ctrl + o`. Conferma le modifiche premendo `invio`. Esci dall'editor nano premendo `ctrl + x`.

4. Crea un'immagine Docker che includa il tuo codice applicazione e inseriscila nel tuo registro privato.

  ```
  ibmcloud cr build -t <region>.icr.io/namespace/cf-py .
  ```
  {: pre}

  <table>
  <caption>Descrizione dei componenti di questo comando</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Questa icona indica che sono presenti informazioni aggiuntive sui componenti di questo comando."/> Descrizione dei componenti di questo comando</th>
  </thead>
  <tbody>
  <tr>
  <td>Parametro</td>
  <td>Descrizione</td>
  </tr>
  <tr>
  <td><code>build</code></td>
  <td>Il comando build.</td>
  </tr>
  <tr>
  <td><code>-t registry.&lt;region&gt;.bluemix.net/namespace/cf-py</code></td>
  <td>Il tuo percorso privato, che include il tuo spazio dei nomi univoco e il nome dell'immagine. Per questo esempio, viene utilizzato lo stesso nome sia per l'immagine che per la directory dell'applicazione, ma puoi scegliere qualsiasi nome per l'immagine nel tuo registro privato. Se non sei sicuro di quale sia il tuo spazio dei nomi, esegui il comando `ibmcloud cr namespaces` per trovarlo.</td>
  </tr>
  <tr>
  <td><code>.</code></td>
  <td>L'ubicazione del Dockerfile. Se stai eseguendo il comando build dalla directory che
contiene il Dockerfile, immetti un punto (.). Altrimenti, utilizza il percorso relativo al Dockerfile.</td>
  </tr>
  </tbody>
  </table>

  L'immagine viene creata nel tuo registro privato. Puoi eseguire il comando `ibmcloud cr images` per verificare che l'immagine sia stata creata.

  ```
  REPOSITORY                       NAMESPACE   TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS
  us.icr.io/namespace/cf-py        namespace   latest   cb03170b2cb2   3 minutes ago   271 MB   OK
  ```
  {: screen}


<br />



## Lezione 3: Distribuzione di un contenitore dalla propria immagine
{: #cf_3}

Distribuisci la tua applicazione come un contenitore in un cluster Kubernetes.
{: shortdesc}

1. Crea un file YAML di configurazione denominato `cf-py.yaml` e aggiorna `<registry_namespace>` con il nome del tuo registro delle immagini privato. Questo file di configurazione definisce una distribuzione del contenitore dall'immagine che hai creato nella lezione precedente e un servizio per esporre l'applicazione al pubblico.

  ```
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    labels:
      app: cf-py
    name: cf-py
    namespace: default
  spec:
    selector:
      matchLabels:
        app: cf-py
    replicas: 1
    template:
      metadata:
        labels:
          app: cf-py
      spec:
        containers:
        - image: us.icr.io/<registry_namespace>/cf-py:latest
          name: cf-py
  ---
  apiVersion: v1
  kind: Service
  metadata:
    name: cf-py-nodeport
    labels:
      app: cf-py
  spec:
    selector:
      app: cf-py
    type: NodePort
    ports:
     - port: 5000
       nodePort: 30872
  ```
  {: codeblock}

  <table>
  <caption>Descrizione dei componenti del file YAML</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
  </thead>
  <tbody>
  <tr>
  <td><code>image</code></td>
  <td>In `us.icr.io/<registry_namespace>/cf-py:latest`, sostituisci &lt;registry_namespace&gt; con lo spazio dei nomi del tuo registro delle immagini privato. Se non sei sicuro di quale sia il tuo spazio dei nomi, esegui il comando `ibmcloud cr namespaces` per trovarlo.</td>
  </tr>
  <tr>
  <td><code>nodePort</code></td>
  <td>Esponi la tua applicazione creando un servizio Kubernetes di tipo NodePort. L'intervallo di NodePort è compreso tra 30000 e 32767. Utilizza questa porta per verificare la tua applicazione in un browser in un secondo momento.</td>
  </tr>
  </tbody></table>

2. Applica il file di configurazione per creare la distribuzione e il servizio nel tuo cluster.

  ```
  kubectl apply -f <filepath>/cf-py.yaml
  ```
  {: pre}

  Output:

  ```
  deployment "cf-py" configured
  service "cf-py-nodeport" configured
  ```
  {: screen}

3. Ora che tutto il lavoro di distribuzione è stato effettuato, puoi verificare la tua applicazione in un browser. Ottieni i dettagli dal formato dell'URL.

    a.  Ottieni l'indirizzo IP pubblico per il nodo di lavoro nel cluster.

    ```
    ibmcloud ks workers --cluster <cluster_name>
    ```
    {: pre}

    Output:

    ```
    ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
    kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   u3c.2x4.encrypted   normal   Ready    dal10   1.13.6
    ```
    {: screen}

    b. Apri un browser e controlla l'applicazione con il seguente URL: `http://<public_IP_address>:<NodePort>`. Con i valori di esempio, l'URL è `http://169.xx.xxx.xxx:30872`. Puoi dare questo URL a un collaboratore come prova o immetterlo nel browser del cellulare, in modo da poter controllare che l'applicazione sia realmente disponibile pubblicamente.

    <img src="images/python_flask.png" alt="Un'acquisizione schermo dell'applicazione Python Flask del contenitore tipo distribuita." />

5.  [Avvia il dashboard Kubernetes](/docs/containers?topic=containers-app#cli_dashboard).

    Se selezioni il tuo cluster nella [console {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/), puoi utilizzare il pulsante **Dashboard Kubernetes** per avviare il tuo dashboard con un clic.
    {: tip}

6. Nella scheda **Carichi di lavoro**, puoi visualizzare le risorse che hai creato.

Ottimo lavoro! La tua applicazione è stata distribuita al contenitore!
