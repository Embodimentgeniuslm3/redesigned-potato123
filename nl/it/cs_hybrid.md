---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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



# Cloud ibrido
{: #hybrid_iks_icp}

Se hai un account {{site.data.keyword.Bluemix}} privato, puoi usarlo con i servizi {{site.data.keyword.Bluemix_notm}} di selezione, incluso {{site.data.keyword.containerlong}}. Per ulteriori informazioni, consulta il blog sull'argomento [Hybrid experience across {{site.data.keyword.Bluemix_notm}} Private and IBM Public Cloud![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://ibm.biz/hybridJune2018).
{: shortdesc}

Capisci le offerte [{{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-cs_ov#differentiation) e hai sviluppato la tua strategia Kubernetes per stabilire quali [carichi di lavoro eseguire su cloud](/docs/containers?topic=containers-strategy#cloud_workloads). Ora puoi connettere il tuo cloud pubblico e privato attraverso il servizio VPN strongSwan o {{site.data.keyword.BluDirectLink}}.

* Il servizio [VPN strongSwan](#hybrid_vpn) collega in maniera sicura il tuo cluster Kubernetes con una rete in loco, attraverso un canale di comunicazione end-to-end protetto su Internet, basato sulla suite di protocolli IPSec (Internet Protocol Security) standard del settore.
* Con [{{site.data.keyword.Bluemix_notm}} Direct Link](#hybrid_dl), puoi creare una connessione privata diretta tra i tuoi ambienti di rete remoti e {{site.data.keyword.containerlong_notm}} senza instradamento sull'Internet pubblico.

Una volta che connetti il tuo cloud pubblico e privato, puoi [riusare i tuoi pacchetti privati per i contenitori pubblici](#hybrid_ppa_importer).

## Connessione del tuo cloud pubblico e privato con la VPN strongSwan
{: #hybrid_vpn}

Stabilisci una connettività VPN tra il tuo cluster Kubernetes pubblico e la tua istanza {{site.data.keyword.Bluemix}} privato per consentire una comunicazione bidirezionale.
{: shortdesc}

1.  Crea un cluster standard con {{site.data.keyword.containerlong}} in {{site.data.keyword.Bluemix_notm}} pubblico oppure usane uno esistente. Per creare un cluster, scegli tra le seguenti opzioni:
    - [Crea un cluster standard dalla console o dalla CLI](/docs/containers?topic=containers-clusters#clusters_ui).
    - [Usa CAM (Cloud Automation Manager) per creare un cluster utilizzando un template predefinito![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_deploy_IKS.html). Quando distribuisci un cluster con CAM, viene installato automaticamente il tiller Helm.

2.  Nel tuo cluster {{site.data.keyword.containerlong_notm}}, [attieniti alle istruzioni per configurare il servizio VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn_configure).

    *  Per il [Passo 2](/docs/containers?topic=containers-vpn#strongswan_2), nota che:

       * Il `local.id` che imposti nel tuo cluster {{site.data.keyword.containerlong_notm}} deve corrispondere a quello che imposti successivamente come `remote.id` nel tuo cluster {{site.data.keyword.Bluemix}} privato.
       * Il `remote.id` che imposti nel tuo cluster {{site.data.keyword.containerlong_notm}} deve corrispondere a quello che imposti successivamente come `local.id` nel tuo cluster {{site.data.keyword.Bluemix}} privato.
       * Il `preshared.secret` che imposti nel tuo cluster {{site.data.keyword.containerlong_notm}} deve corrispondere a quello che imposti successivamente come `preshared.secret` nel tuo cluster {{site.data.keyword.Bluemix}} privato.

    *  Per il [Passo 3](/docs/containers?topic=containers-vpn#strongswan_3), configura strongSwan per una connessione VPN **in entrata**.

       ```
       ipsec.auto: add
       loadBalancerIP: <portable_public_IP>
       ```
       {: codeblock}

3.  Prendi nota dell'indirizzo IP pubblico portatile che imposti come `loadbalancerIP`.

    ```
    kubectl get svc vpn-strongswan
    ```
    {: pre}

4.  [Crea un cluster in {{site.data.keyword.Bluemix_notm}} privato![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/installing/installing.html).

5.  Nel tuo cluster {{site.data.keyword.Bluemix_notm}} privato, distribuisci il servizio VPN IPSec strongSwan.

    1.  [Completa le soluzioni temporanee della VPN IPSec strongSwan ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_strongswan.html).

    2.  [Configura il grafico Helm VPN strongSwan![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/app_center/create_release.html) nel tuo cluster privato.

        *  Nei parametri di configurazione, imposta il campo **Remote gateway** sul valore dell'indirizzo IP pubblico portatile che hai impostato come `loadbalancerIP` del tuo cluster {{site.data.keyword.containerlong_notm}}.

           ```
           Operation at startup: start
           ...
           Remote gateway: <portable_public_IP>
           ...
           ```
           {: codeblock}

        *  Ricordati che il `local.id` privato deve corrispondere al `remote.id`pubblico, il `remote.id` privato deve corrispondere al `local.id` pubblico e i valori `preshared.secret` per pubblico e privato devono corrispondere.

        Ora puoi avviare una connessione dal cluster {{site.data.keyword.Bluemix_notm}} privato al cluster {{site.data.keyword.containerlong_notm}}.

7.  [Verifica la connessione VPN](/docs/containers?topic=containers-vpn#vpn_test) tra i cluster.

8.  Ripeti questi passi per ciascun cluster che vuoi collegare.

**Operazioni successive**

*   [Esegui le tue immagini software su licenza in cluster pubblici](#hybrid_ppa_importer).
*   Per gestire più cluster cloud Kubernetes, come ad esempio tra {{site.data.keyword.Bluemix_notm}} Public e {{site.data.keyword.Bluemix_notm}} Private, consulta [IBM Multicloud Manager ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).


## Connessione del tuo cloud pubblico e privato con {{site.data.keyword.Bluemix_notm}} Direct Link
{: #hybrid_dl}

Con [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link), puoi creare una connessione privata diretta tra i tuoi ambienti di rete remoti e {{site.data.keyword.containerlong_notm}} senza instradamento sull'Internet pubblico.
{: shortdesc}

Per collegare il tuo cloud pubblico e la tua istanza {{site.data.keyword.Bluemix}} privato, puoi utilizzare una delle quattro offerte di seguito indicate:
* {{site.data.keyword.Bluemix_notm}} Direct Link Connect
* {{site.data.keyword.Bluemix_notm}} Direct Link Exchange
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated Hosting

Per scegliere un'offerta {{site.data.keyword.Bluemix_notm}} Direct Link e configurare una connessione {{site.data.keyword.Bluemix_notm}} Direct Link, vedi [Introduzione a {{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-) nella documentazione di {{site.data.keyword.Bluemix_notm}} Direct Link.

**Operazioni successive**</br>
* [Esegui le tue immagini software su licenza in cluster pubblici](#hybrid_ppa_importer).
* Per gestire più cluster cloud Kubernetes, come ad esempio tra {{site.data.keyword.Bluemix_notm}} Public e {{site.data.keyword.Bluemix_notm}} Private, consulta [IBM Multicloud Manager ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).

<br />


## Esecuzione delle immagini di {{site.data.keyword.Bluemix_notm}} privato in contenitori Kubernetes pubblici
{: #hybrid_ppa_importer}

Puoi eseguire i prodotti IBM su licenza di selezione forniti per {{site.data.keyword.Bluemix_notm}} privato in un cluster in {{site.data.keyword.Bluemix_notm}} pubblico.  
{: shortdesc}

Il software su licenza è disponibile in [IBM Passport Advantage ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www-01.ibm.com/software/passportadvantage/index.html). Per usare questo software in un cluster in {{site.data.keyword.Bluemix_notm}} pubblico, devi scaricarlo, estrarre l'immagine e caricarla nel tuo spazio dei nomi in {{site.data.keyword.registryshort}}. Indipendentemente dall'ambiente in cui intendi usare il software, devi ottenere innanzitutto la licenza richiesta per il prodotto.

La seguente tabella è una panoramica dei prodotti {{site.data.keyword.Bluemix_notm}} privato disponibili che puoi usare nel tuo cluster in {{site.data.keyword.Bluemix_notm}} pubblico.

| Nome prodotto | Versione | Numero parte |
| --- | --- | --- |
| IBM Db2 Direct Advanced Edition Server | 11.1 | CNU3TML |
| IBM Db2 Advanced Enterprise Server Edition Server | 11.1 | CNU3SML |
| IBM MQ Advanced | 9.1.0.0, 9.1.1,0, 9.1.2.0 | - |
| IBM WebSphere Application Server Liberty | 16.0.0.3 | immagine Docker Hub |
{: caption="Tabella. Prodotti {{site.data.keyword.Bluemix_notm}} privato supportati da usare in {{site.data.keyword.Bluemix_notm}} pubblico." caption-side="top"}

Prima di iniziare:
- [Installa il plugin della CLI {{site.data.keyword.registryshort}} (`ibmcloud cr`)](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#cli_namespace_registry_cli_install).
- [Configura lo spazio dei nomi in {{site.data.keyword.registryshort}}](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#registry_namespace_setup) o richiama il tuo spazio dei nomi esistente eseguendo `ibmcloud cr namespaces`.
- [Indirizza la tua CLI `kubectl` al tuo cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- [Installa la CLI Helm e imposta tiller nel tuo cluster](/docs/containers?topic=containers-helm#public_helm_install).

Per distribuire un'immagine di {{site.data.keyword.Bluemix_notm}} privato in un cluster in {{site.data.keyword.Bluemix_notm}} pubblico:

1.  Segui le procedure descritte nella [documentazione di {{site.data.keyword.registryshort}}](/docs/services/Registry?topic=registry-ts_index#ts_ppa) per scaricare il software su licenza da IBM Passport Advantage, inviare l'immagine al tuo spazio dei nomi e installare il grafico Helm nel tuo cluster.

    **Per IBM WebSphere Application Server Liberty**:

    1.  Invece di ottenere l'immagine da IBM Passport Advantage, usa l'[immagine Docker Hub ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://hub.docker.com/_/websphere-liberty/). Per istruzioni su come ottenere una licenza, vedi [Upgrading the image from Docker Hub to a production image![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/WASdev/ci.docker/tree/master/ga/production-upgrade).

    2.  Segui le [istruzioni del grafico Helm Liberty![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_icp_helm.html).

2.  Verifica che lo **STATO** del grafico Helm mostri `DISTRIBUITO`. Se mostra un valore differente, attendi alcuni minuti e ritenta.
    ```
    helm status <helm_chart_name>
    ```
    {: pre}

3.  Consulta la documentazione specifica del prodotto per ulteriori informazioni su come configurare e usare il prodotto con il tuo cluster.

    - [IBM Db2 Direct Advanced Edition Server ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.licensing.doc/doc/c0070181.html)
    - [IBM MQ Advanced ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.html)
    - [IBM WebSphere Application Server Liberty ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/as_ditamaps/was900_welcome_liberty.html)
