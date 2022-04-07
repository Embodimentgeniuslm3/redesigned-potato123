---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

keywords: kubernetes, iks, clusters, worker nodes, worker pools, delete

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


# Creazione dei cluster
{: #clusters}

Crea un cluster Kubernetes in {{site.data.keyword.containerlong}}.
{: shortdesc}

Sei ancora nella fase iniziale? Prova l'[esercitazione relativa alla creazione di un cluster Kubernetes](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial). Non sai quale configurazione del cluster scegliere? Vedi [Pianificazione della configurazione di rete del tuo cluster](/docs/containers?topic=containers-plan_clusters).
{: tip}

Hai già creato un cluster e stai solo cercando dei comandi di esempio rapidi? Prova questi esempi.
*  **Cluster gratuito**:
   ```
   ibmcloud ks cluster-create --name my_cluster
   ```
   {: pre}
*  **Cluster standard, macchina virtuale condivisa**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **Cluster standard, bare metal**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type mb2c.4x32 --hardware dedicated --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **Cluster standard, macchina virtuale con endpoint del servizio pubblici e privati in account abilitati per VRF**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-service-endpoint --private-service-endpoint --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **Cluster standard che utilizza solo VLAN private e l'endpoint del servizio privato**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --private-service-endpoint --private-vlan <private_VLAN_ID> --private-only
   ```
   {: pre}

<br />


## Preparazione alla creazione di cluster a livello di account
{: #cluster_prepare}

Prepara il tuo account {{site.data.keyword.Bluemix_notm}} per {{site.data.keyword.containerlong_notm}}. Si tratta di preparazioni che, una volta eseguite dall'amministratore dell'account, potresti non dover modificare ogni volta che crei un cluster. Tuttavia, ogni volta che crei un cluster, puoi comunque verificare che lo stato corrente a livello di account sia quello che desideri.
{: shortdesc}

1. [Crea o aggiorna il tuo account a un account fatturabile (Pagamento a consumo o Sottoscrizione {{site.data.keyword.Bluemix_notm}})](https://cloud.ibm.com/registration/).

2. [Configura una chiave API {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-users#api_key) nelle regioni in cui vuoi creare i cluster. Assegna la chiave API con le autorizzazioni appropriate per creare i cluster:
  * Ruolo **Super utente** per l'infrastruttura IBM Cloud (SoftLayer).
  * Ruolo di gestione della piattaforma **Amministratore** per {{site.data.keyword.containerlong_notm}} a livello di account.
  * Ruolo di gestione della piattaforma **Amministratore** per {{site.data.keyword.registrylong_notm}} a livello di account. Se il tuo account è antecedente al 4 ottobre 2018, devi [abilitare le politiche {{site.data.keyword.Bluemix_notm}} IAM per {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-user#existing_users). Con le politiche IAM, puoi controllare l'accesso a risorse come gli spazi dei nomi del registro.

  Sei il proprietario dell'account? Hai già le autorizzazioni necessarie! Quando crei un cluster, la chiave API per tale regione e gruppo di risorse viene impostata con le tue credenziali.
  {: tip}

3. Assicurati di avere il ruolo della piattaforma **Amministratore** per {{site.data.keyword.containerlong_notm}}. Per consentire al tuo cluster di estrarre immagini dal registro privato, ti occorre anche il ruolo della piattaforma **Amministratore** per {{site.data.keyword.registrylong_notm}}.
  1. Dalla barra dei menu della [console {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/), fai clic su **Gestisci > Accesso (IAM)**.
  2. Fai clic sulla pagina **Utenti** e quindi, dalla tabella, seleziona il tuo nome utente.
  3. Dalla scheda **Politiche di accesso**, conferma che il tuo **Ruolo** sia **Amministratore**. Puoi essere l'**Amministratore** per tutte le risorse nell'account o almeno per {{site.data.keyword.containershort_notm}}. **Nota**: se disponi del ruolo **Amministratore** per {{site.data.keyword.containershort_notm}} in un solo gruppo di risorse o una sola regione anziché nell'intero account, devi avere almeno il ruolo di **Visualizzatore** a livello di account per visualizzare le VLAN dell'account.
  <p class="tip">Accertati che l'amministratore del tuo account non ti assegni il ruolo della piattaforma **Amministratore** anche come ruolo di servizio. Devi assegnare i ruoli della piattaforma e del servizio separatamente.</p>

4. Se il tuo account utilizza più gruppi di risorse, scopri la strategia del tuo account per la [gestione dei gruppi di risorse](/docs/containers?topic=containers-users#resource_groups).
  * Il cluster viene creato nel gruppo di risorse che specifichi quando accedi a {{site.data.keyword.Bluemix_notm}}. Se non specifichi un gruppo di risorse, viene utilizzato automaticamente il gruppo di risorse predefinito.
  * Se vuoi creare un cluster in un gruppo di risorse diverso da quello predefinito, devi disporre almeno del ruolo **Visualizzatore** per il gruppo di risorse. Se non disponi di alcun ruolo per il gruppo di risorse ma sei comunque un **Amministratore** per il servizio all'interno del gruppo di risorse, il tuo cluster viene creato nel gruppo di risorse predefinito.
  * Non puoi modificare il gruppo di risorse di un cluster. Inoltre, se hai bisogno di utilizzare il [comando](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind) `ibmcloud ks cluster-service-bind` per l'[integrazione con un servizio {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-service-binding#bind-services), tale servizio deve appartenere allo stesso gruppo di risorse del cluster. I servizi che non utilizzano gruppi di risorse, come {{site.data.keyword.registrylong_notm}}, o che non hanno bisogno di un bind del servizio, come {{site.data.keyword.la_full_notm}}, funzionano anche se il cluster si trova in un gruppo di risorse differente.
  * Se intendi utilizzare [{{site.data.keyword.monitoringlong_notm}} per le metriche](/docs/containers?topic=containers-health#view_metrics), assegna al cluster un nome univoco tra tutti i gruppi di risorse e tutte le regioni nel tuo account per evitare conflitti di denominazione delle metriche.
  * I cluster gratuiti vengono creati nel gruppo di risorse `default`.

5. **Cluster standard**: pianifica la [configurazione di rete](/docs/containers?topic=containers-plan_clusters) del tuo cluster in modo che soddisfi le esigenze dei tuoi carichi di lavoro e del tuo ambiente. Quindi configura la rete della tua infrastruttura IBM Cloud (SoftLayer) in modo da consentire la comunicazione nodo di lavoro-master e utente-master:
  * Per utilizzare solo l'endpoint del servizio privato o gli endpoint del servizio pubblici e privati (esecuzione di carichi di lavoro con connessione Internet o estensione del tuo data center in loco):
    1. Abilita [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) nel tuo account dell'infrastruttura IBM Cloud (SoftLayer).
    2. [Abilita il tuo account {{site.data.keyword.Bluemix_notm}} per l'utilizzo degli endpoint del servizio](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
    <p class="note">Il master Kubernetes è accessibile tramite l'endpoint del servizio privato se gli utenti del cluster autorizzati si trovano nella tua rete privata {{site.data.keyword.Bluemix_notm}} o sono connessi alla rete privata tramite una [connessione VPN](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) o [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). Tuttavia, le comunicazioni con il master Kubernetes sull'endpoint del servizio privato devono passare attraverso l'intervallo di indirizzi IP <code>166.X.X.X</code>, che non è instradabile da una connessione VPN o tramite {{site.data.keyword.Bluemix_notm}} Direct Link. Puoi esporre l'endpoint del servizio privato del master per gli utenti del tuo cluster utilizzando un NLB (network load balancer) privato. L'NLB privato espone l'endpoint del servizio privato del master come intervallo di indirizzi IP <code>10.X.X.X</code> interno a cui gli utenti possono accedere con la connessione VPN o {{site.data.keyword.Bluemix_notm}} Direct Link. Se abiliti solo l'endpoint del servizio privato, puoi utilizzare il dashboard Kubernetes o abilitare temporaneamente l'endpoint del servizio pubblico per creare l'NLB privato. Per ulteriori informazioni, vedi [Accesso ai cluster attraverso l'endpoint del servizio privato](/docs/containers?topic=containers-clusters#access_on_prem).</p>

  * Per utilizzare solo l'endpoint del servizio pubblico (esecuzione di carichi di lavoro con connessione Internet):
    1. Abilita lo [spanning delle VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) per il tuo account dell'infrastruttura IBM Cloud (SoftLayer) in modo che i tuoi nodi di lavoro possano comunicare tra loro sulla rete privata. Per eseguire questa azione, ti serve l'[autorizzazione dell'infrastruttura](/docs/containers?topic=containers-users#infra_access) **Rete > Gestisci il VLAN Spanning di rete** oppure puoi richiedere al proprietario dell'account di abilitarlo. Per controllare se lo spanning della VLAN è già abilitato, utilizza il [comando `ibmcloud ks vlan-spanning-get --region <region>`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get).
  * Per utilizzare un dispositivo gateway (estensione del tuo data center in loco):
    1. Abilita lo [spanning delle VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) per il tuo account dell'infrastruttura IBM Cloud (SoftLayer) in modo che i tuoi nodi di lavoro possano comunicare tra loro sulla rete privata. Per eseguire questa azione, ti serve l'[autorizzazione dell'infrastruttura](/docs/containers?topic=containers-users#infra_access) **Rete > Gestisci il VLAN Spanning di rete** oppure puoi richiedere al proprietario dell'account di abilitarlo. Per controllare se lo spanning della VLAN è già abilitato, utilizza il [comando `ibmcloud ks vlan-spanning-get --region <region>`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get).
    2. Configurare un dispositivo gateway. Ad esempio, puoi scegliere di configurare un [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) o un [Fortigate Security Appliance](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations) in modo che funga da firewall che consenta il traffico necessario e blocchi il traffico non desiderato.
    3. [Apri le porte e gli indirizzi IP privati](/docs/containers?topic=containers-firewall#firewall_outbound) per ciascuna regione, in modo che il master e i nodi di lavoro possano comunicare e per i servizi {{site.data.keyword.Bluemix_notm}} che intendi utilizzare.

<br />


## Preparazione alla creazione di cluster a livello di cluster
{: #prepare_cluster_level}

Dopo aver configurato il tuo account per la creazione dei cluster, prepara la configurazione del tuo cluster. Queste preparazioni che hanno un impatto sul cluster ogni volta che ne crei uno.
{: shortdesc}

1. Decidi tra un [cluster gratuito o standard](/docs/containers?topic=containers-cs_ov#cluster_types). Puoi creare 1 cluster gratuito per provare alcune delle funzionalità per 30 giorni oppure puoi creare cluster standard completamente personalizzabili con le tue scelte di isolamento hardware. Crea un cluster standard per ottenere maggiori vantaggi e maggior controllo sulle prestazioni del tuo cluster.

2. Pianifica la configurazione dei cluster standard.
  * Decidi se creare un cluster a [zona singola](/docs/containers?topic=containers-ha_clusters#single_zone) o [multizona](/docs/containers?topic=containers-ha_clusters#multizone). Nota che i cluster multizona sono disponibili solo in determinate ubicazioni.
  * Scegli il tipo di [hardware e isolamento](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) che vuoi per i nodi di lavoro del tuo cluster, compresa la decisione tra macchine virtuali o bare metal.

3. Per i cluster standard, puoi [stimare i costi](/docs/billing-usage?topic=billing-usage-cost#cost) nella console {{site.data.keyword.Bluemix_notm}}. Per ulteriori informazioni sugli addebiti che potrebbero non essere inclusi nello stimatore, vedi [Prezzi e fatturazione](/docs/containers?topic=containers-faqs#charges).

4. Se crei il cluster in un ambiente dietro un firewall, come nel caso dei cluster che estendono il tuo data center in loco, [consenti il traffico di rete in uscita verso gli IP pubblici e privati](/docs/containers?topic=containers-firewall#firewall_outbound) per i servizi {{site.data.keyword.Bluemix_notm}} che intendi utilizzare.

<br />


## Creazione di un cluster gratuito
{: #clusters_free}

Puoi utilizzare il tuo cluster gratuito per acquisire familiarità con il funzionamento di {{site.data.keyword.containerlong_notm}}. Con i cluster gratuiti, puoi comprendere la terminologia, completare un'esercitazione e ottenere le indicazioni necessarie prima di passare ai cluster standard a livello di produzione. Non preoccuparti, ottieni comunque un cluster gratuito anche se disponi di un account fatturabile.
{: shortdesc}

I cluster gratuiti includono un nodo di lavoro configurato con 2vCPU e 4 GB di memoria e hanno una durata di vita di 30 giorni. Trascorso tale periodo di tempo, il cluster scade e verrà eliminato insieme ai suoi dati. {{site.data.keyword.Bluemix_notm}} non esegue il backup dei dati eliminati e non possono essere ripristinati. Assicurati di eseguire il backup dei dati importanti.
{: note}

### Creazione di un cluster gratuito nella console
{: #clusters_ui_free}

1. Nel [catalogo ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/catalog?category=containers), seleziona **{{site.data.keyword.containershort_notm}}** per creare un cluster.
2. Seleziona il piano del cluster **gratuito**.
3. Seleziona un'area geografica in cui distribuire il tuo cluster.
4. Seleziona un'ubicazione metropolitana nell'area geografica. Il tuo cluster viene creato in una zona appartenente a quest'area.
5. Assegna un nome al tuo cluster. Il nome deve iniziare con una lettera, può contenere lettere, numeri e un trattino (-) e non può contenere più di 35 caratteri. Utilizza un nome che sia univoco tra le regioni.
6. Fai clic su **Crea cluster**. Per impostazione predefinita, viene creato un pool di nodi di lavoro con un nodo di lavoro. Puoi vedere lo stato di avanzamento della distribuzione del nodo di lavoro nella scheda **Nodi di lavoro**. Quando la distribuzione è terminata, puoi vedere se il tuo cluster è pronto nella scheda **Panoramica**. Nota che anche se il cluster è pronto, alcune parti del cluster utilizzate da altri servizi, come i segreti di pull dell'immagine di registro, potrebbero essere ancora in elaborazione.

  A ogni nodo di lavoro viene assegnato un ID e nome dominio univoco che non deve essere modificato manualmente dopo la creazione del cluster. La modifica dell'ID o del nome dominio impedisce al master Kubernetes di gestire il tuo cluster.
  {: important}
7. Una volta che il tuo cluster è stato creato, puoi [iniziare a utilizzarlo configurando la sessione della tua CLI](#access_cluster).

### Creazione di un cluster gratuito nella CLI
{: #clusters_cli_free}

Prima di iniziare, installa la CLI {{site.data.keyword.Bluemix_notm}} e il [plugin {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_cli_install#cs_cli_install).

1. Accedi alla CLI {{site.data.keyword.Bluemix_notm}}.
  1. Esegui l'accesso e immetti le tue credenziali {{site.data.keyword.Bluemix_notm}} quando richiesto.
     ```
     ibmcloud login
     ```
     {: pre}

     Se disponi di un ID federato, utilizza `ibmcloud login --sso` per accedere alla CLI {{site.data.keyword.Bluemix_notm}}. Immetti il tuo nome utente e usa l'URL fornito nell'output della CLI per richiamare la tua passcode monouso. Sai che hai un ID federato quando l'accesso ha esito negativo senza `--sso` e positivo con l'opzione `--sso`.
     {: tip}

  2. Se disponi di più account {{site.data.keyword.Bluemix_notm}}, seleziona l'account in cui vuoi creare il tuo cluster Kubernetes.

  3. Per creare un cluster gratuito in una regione specifica, devi specificare la regione. Puoi creare un cluster gratuito in `ap-south`, `eu-central`, `uk-south` o `us-south`. Il cluster viene creato in una zona all'interno di tale regione.
     ```
     ibmcloud ks region-set
     ```
     {: pre}

2. Crea un cluster.
  ```
  ibmcloud ks cluster-create --name my_cluster
  ```
  {: pre}

3. Verifica che la creazione del cluster sia stata richiesta. L'ordine della macchina dei nodi di lavoro può richiedere alcuni minuti.
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    Quando è stato terminato il provisioning del tuo cluster, lo stato del tuo cluster viene modificato in **distribuito**.
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

4. Controlla lo stato del nodo di lavoro.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Quando il nodo di lavoro è pronto, la condizione passa a Normale (**normal**) e lo stato a Pronto (**Ready**). Quando lo stato del nodo è **Ready**, puoi accedere al cluster. Nota che anche se il cluster è pronto, alcune parti del cluster utilizzate da altri servizi come i segreti Ingress o i segreti di pull dell'immagine di registro potrebbero essere ancora in elaborazione.
    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   free           normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    A ogni nodo di lavoro viene assegnato un ID e nome dominio univoco che non deve essere modificato manualmente dopo la creazione del cluster. La modifica dell'ID o del nome dominio impedisce al master Kubernetes di gestire il tuo cluster.
    {: important}

5. Una volta che il tuo cluster è stato creato, puoi [iniziare a utilizzarlo configurando la sessione della tua CLI](#access_cluster).

<br />


## Creazione di un cluster standard
{: #clusters_standard}

Utilizza la CLI {{site.data.keyword.Bluemix_notm}} o la console {{site.data.keyword.Bluemix_notm}} per creare un cluster completamente personalizzabile con l'isolamento hardware e l'opzione di accesso alle funzioni prescelti, come più nodi di lavoro per un ambiente altamente disponibile.
{: shortdesc}

### Creazione di un cluster standard nella console
{: #clusters_ui}

1. Nel [catalogo ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/catalog?category=containers), seleziona **{{site.data.keyword.containershort_notm}}** per creare un cluster.

2. Seleziona un gruppo di risorse in cui creare il tuo cluster.
  **Nota**:
    * Un cluster può essere creato in un solo gruppo di risorse e, una volta che è stato creato, non puoi modificare il suo gruppo di risorse.
    * Per creare i cluster in un gruppo di risorse diverso da quello predefinito, devi disporre almeno del [ruolo **Visualizzatore**](/docs/containers?topic=containers-users#platform) per il gruppo di risorse.

3. Seleziona un'area geografica in cui distribuire il tuo cluster.

4. Fornisci al tuo cluster un nome univoco. Il nome deve iniziare con una lettera, può contenere lettere, numeri e un trattino (-) e non può contenere più di 35 caratteri. Utilizza un nome che sia univoco tra le regioni. Il nome cluster e la regione in cui viene distribuito il cluster formano il nome dominio completo per il dominio secondario Ingress. Per garantire che il dominio secondario Ingress sia univoco all'interno di una regione, è possibile che il nome cluster venga troncato e vi venga aggiunto un valore casuale all'interno del nome dominio Ingress.
 **Nota**: la modifica dell'ID univoco o del nome dominio assegnato durante la creazione non consente al master di gestire il tuo cluster.

5. Seleziona la disponibilità a **Zona singola** o **Multizona**. In un cluster multizona, il nodo master viene distribuito in una zona con supporto multizona e le risorse del tuo cluster vengono distribuite tra più zone.

6. Immetti i dettagli dell'area metropolitana e della zona.
  * Cluster multizona:
    1. Seleziona un'ubicazione metropolitana. Per prestazioni ottimali, seleziona l'ubicazione metropolitana fisicamente più vicina a te. Le tue scelte potrebbero essere limitate in base all'area geografica.
    2. Seleziona le zone specifiche in cui desideri ospitare il tuo cluster. Devi selezionare almeno una zona ma puoi selezionarne quante ne desideri. Se selezioni più di una zona, i nodi di lavoro vengono estesi tra le zone che hai scelto e ciò ti fornisce una disponibilità più elevata. Se selezioni solo una zona, puoi [aggiungere le zone al tuo cluster](/docs/containers?topic=containers-add_workers#add_zone) una volta creato.
  * Cluster a zona singola: seleziona una zona in cui vuoi ospitare il tuo cluster. Per prestazioni ottimali, seleziona la zona fisicamente più vicina a te. Le tue scelte potrebbero essere limitate in base all'area geografica.

7. Per ogni zona, scegli le VLAN.
  * Per creare un cluster in cui puoi eseguire carichi di lavoro con connessione Internet:
    1. Seleziona una VLAN pubblica e una VLAN privata dal tuo account dell'infrastruttura IBM Cloud (SoftLayer) per ciascuna zona. I nodi di lavoro comunicano tra di loro attraverso la VLAN privata e possono comunicare con il master Kubernetes attraverso la VLAN pubblica o privata. Se non disponi di una VLAN pubblica o privata in questa zona, vengono create automaticamente una VLAN pubblica e una VLAN privata. Puoi utilizzare la stessa VLAN per più cluster.
  * Per creare un cluster che estenda il tuo data center in loco solo sulla rete privata o che estenda il tuo data center in loco con la possibilità di aggiungere un accesso pubblico limitato in seguito o che estenda il tuo data center in loco e fornisca un accesso pubblico limitato attraverso un dispositivo gateway:
    1. Seleziona una VLAN privata dal tuo account dell'infrastruttura IBM Cloud (SoftLayer) per ciascuna zona. I nodi di lavoro comunicano tra loro utilizzando la VLAN privata. Se non disponi di una VLAN privata in una zona, viene creata automaticamente una VLAN privata. Puoi utilizzare la stessa VLAN per più cluster.
    2. Per la VLAN pubblica, seleziona **Nessuna**.

8. Per **Endpoint del servizio master**, scegli la modalità di comunicazione tra nodi di lavoro e master Kubernetes.
  * Per creare un cluster in cui puoi eseguire carichi di lavoro con connessione Internet:
    * Se la VRF e gli endpoint del servizio sono abilitati nel tuo account {{site.data.keyword.Bluemix_notm}}, seleziona**Endpoint sia privati che pubblici**.
    * Se non puoi o non vuoi abilitare VRF, seleziona **Solo endpoint pubblico**.
  * Per creare un cluster che estenda solo il tuo data center in loco o un cluster che estenda il tuo data center in loco con la possibilità di aggiungere un accesso pubblico limitato con nodi di lavoro edge, seleziona **Endpoint sia privati che pubblici** o **Solo endpoint privato**. Assicurarsi di aver abilitato VRF e gli endpoint del servizio nel tuo account {{site.data.keyword.Bluemix_notm}}. Nota che se abiliti solo l'endpoint del servizio privato, devi [esporre l'endpoint del master attraverso un NLB (network load balancer) privato](#access_on_prem), in modo che gli utenti possano accedere al master tramite VPN o con una connessione {{site.data.keyword.BluDirectLink}}.
  * Per creare un cluster che estenda il tuo data center in loco e fornisca un accesso pubblico limitato con un dispositivo gateway, seleziona **Solo endpoint pubblico**.

9. Configura il tuo pool di nodi di lavoro predefinito. I pool di nodi di lavoro sono gruppi di nodi di lavoro che condividono la stessa configurazione. Puoi sempre aggiungere altri pool di nodi di lavoro al tuo cluster in un secondo momento.
  1. Scegli la versione del server API Kubernetes per il nodo master del cluster e i nodi di lavoro.
  2. Filtra i tipi di nodo di lavoro selezionando un tipo di macchina. Il tipo virtuale ha una fatturazione oraria e il tipo bare metal ha una fatturazione mensile.
    - **Bare metal**: il provisioning dei server bare metal, con fatturazione mensile, viene eseguito manualmente mediante l'infrastruttura IBM Cloud (SoftLayer) dopo il tuo ordine e il completamento di questo processo può richiedere più di un giorno lavorativo. Bare metal è più adatto per le applicazioni ad alte prestazioni che richiedono più risorse e controllo host. Puoi anche scegliere di abilitare [Trusted Compute](/docs/containers?topic=containers-security#trusted_compute) per verificare possibili tentativi di manomissione dei tuoi nodi di lavoro. Trusted Compute è disponibile per alcuni tipi di macchina bare metal selezionati. Ad esempio, le varietà GPU `mgXc` non supportano Trusted Compute. Se non abiliti l'attendibilità durante la creazione del cluster ma vuoi farlo in seguito, puoi usare il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Dopo aver abilitato l'attendibilità, non puoi disabilitarla successivamente.
    Assicurati di voler eseguire il provisioning di una macchina bare metal. Poiché viene fatturata mensilmente, se la annulli immediatamente dopo un ordine effettuato per errore, ti viene comunque addebitato l'intero mese.
    {:tip}
    - **Virtuale - Condiviso**: le risorse dell'infrastruttura, come l'hypervisor e l'hardware fisico, sono condivise tra te e altri clienti IBM, ma ciascun nodo di lavoro è accessibile solo a te. Sebbene questa opzione sia meno costosa e sufficiente nella maggior parte dei casi, potresti voler verificare i requisiti di prestazioni e infrastruttura con le politiche aziendali.
    - **Virtuale - dedicato**: i tuoi nodi di lavoro sono ospitati sull'infrastruttura dedicata al tuo account. Le tue risorse fisiche sono completamente isolate.
  3. Seleziona un tipo. Il tipo definisce la quantità di CPU virtuale, di memoria e di spazio disco configurata in ogni nodo di lavoro e resa disponibile ai contenitori. I tipi di macchine bare metal e virtuali disponibili variano in base alla zona in cui distribuisci il cluster. Una volta creato il tuo cluster, puoi aggiungere tipi di macchina diversi aggiungendo un nodo di lavoro o un pool al cluster.
  4. Specifica il numero di nodi di lavoro di cui hai bisogno nel cluster. Il numero di nodi di lavoro che hai immesso viene replicato tra il numero di zone che hai selezionato. Ciò significa che se hai 2 zone e selezioni 3 nodi di lavoro, viene eseguito il provisioning di 6 nodi e 3 nodi sono attivi in ciascuna zona.

10. Fai clic su **Crea cluster**. Un pool di nodi di lavoro viene creato con il numero di nodi di lavoro che hai specificato. Puoi vedere lo stato di avanzamento della distribuzione del nodo di lavoro nella scheda **Nodi di lavoro**. Quando la distribuzione è terminata, puoi vedere se il tuo cluster è pronto nella scheda **Panoramica**. Nota che anche se il cluster è pronto, alcune parti del cluster utilizzate da altri servizi come i segreti Ingress o i segreti di pull dell'immagine di registro potrebbero essere ancora in elaborazione.

  A ogni nodo di lavoro viene assegnato un ID e nome dominio univoco che non deve essere modificato manualmente dopo la creazione del cluster. La modifica dell'ID o del nome dominio impedisce al master Kubernetes di gestire il tuo cluster.
  {: important}

11. Una volta che il tuo cluster è stato creato, puoi [iniziare a utilizzarlo configurando la sessione della tua CLI](#access_cluster).

### Creazione di un cluster standard nella CLI
{: #clusters_cli_steps}

Prima di iniziare, installa la CLI {{site.data.keyword.Bluemix_notm}} e il [plugin {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_cli_install#cs_cli_install).

1. Accedi alla CLI {{site.data.keyword.Bluemix_notm}}.
  1. Esegui l'accesso e immetti le tue credenziali {{site.data.keyword.Bluemix_notm}} quando richiesto.
     ```
     ibmcloud login
     ```
     {: pre}

     Se disponi di un ID federato, utilizza `ibmcloud login --sso` per accedere alla CLI {{site.data.keyword.Bluemix_notm}}. Immetti il tuo nome utente e usa l'URL fornito nell'output della CLI per richiamare la tua passcode monouso. Sai che hai un ID federato quando l'accesso ha esito negativo senza `--sso` e positivo con l'opzione `--sso`.
     {: tip}

  2. Se disponi di più account {{site.data.keyword.Bluemix_notm}}, seleziona l'account in cui vuoi creare il tuo cluster Kubernetes.

  3. Per creare i cluster in un gruppo di risorse diverso da quello predefinito, specifica tale gruppo di risorse. **Nota**:
      * Un cluster può essere creato in un solo gruppo di risorse e, una volta che è stato creato, non puoi modificare il suo gruppo di risorse.
      * Devi disporre almeno del [ruolo **Visualizzatore**](/docs/containers?topic=containers-users#platform) per il gruppo di risorse.

      ```
      ibmcloud target -g <resource_group_name>
      ```
      {: pre}

2. Esamina le zone disponibili. Nell'output del seguente comando, il **Tipo di ubicazione** delle zone è `dc`. Per estendere il tuo cluster tra le zone, devi crearlo in una [una zona che supporta il multizona](/docs/containers?topic=containers-regions-and-zones#zones). Le zone multizona hanno una valore metropolitano nella colonna **Metropolitana multizona**.
    ```
    ibmcloud ks supported-locations
    ```
    {: pre}
    Quando selezioni una zona che si trova fuori dal tuo paese, tieni presente che potresti aver bisogno dell'autorizzazione legale prima che i dati possano essere archiviati fisicamente in un paese straniero.
    {: note}

3. Esaminare i tipi di nodo di lavoro disponibili in tale zona. Il tipo di nodo di lavoro specifica gli host di calcolo virtuali o fisici disponibili per ciascun nodo di lavoro.
  - **Virtuale**: il provisioning delle macchine virtuali, con fatturazione oraria, viene eseguito su hardware condiviso o dedicato.
  - **Fisica**: il provisioning dei server bare metal, con fatturazione mensile, viene eseguito manualmente mediante l'infrastruttura IBM Cloud (SoftLayer) dopo il tuo ordine e il completamento di questo processo può richiedere più di un giorno lavorativo. Bare metal è più adatto per le applicazioni ad alte prestazioni che richiedono più risorse e controllo host.
  - **Macchine fisiche con Trusted Compute**: puoi anche scegliere di abilitare [Trusted Compute](/docs/containers?topic=containers-security#trusted_compute) per verificare possibili tentativi di manomissione dei tuoi nodi di lavoro. Trusted Compute è disponibile per alcuni tipi di macchina bare metal selezionati. Ad esempio, le varietà GPU `mgXc` non supportano Trusted Compute. Se non abiliti l'attendibilità durante la creazione del cluster ma vuoi farlo in seguito, puoi usare il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Dopo aver abilitato l'attendibilità, non puoi disabilitarla successivamente.
  - **Tipi di macchina**: per stabilire quale tipo di macchina distribuire, esamina le combinazioni di core, memoria e archiviazione dell'[hardware del nodo di lavoro disponibile](/docs/containers?topic=containers-planning_worker_nodes#shared_dedicated_node). Una volta creato il tuo cluster, puoi aggiungere diversi tipi di macchina fisica o virtuale [aggiungendo un pool di nodi di lavoro](/docs/containers?topic=containers-add_workers#add_pool).

     Assicurati di voler eseguire il provisioning di una macchina bare metal. Poiché viene fatturata mensilmente, se la annulli immediatamente dopo un ordine effettuato per errore, ti viene comunque addebitato l'intero mese.
     {:tip}

  ```
  ibmcloud ks machine-types --zone <zone>
  ```
  {: pre}

4. Controlla se esistono già delle VLAN per la zona nell'infrastruttura IBM Cloud (SoftLayer) per questo account.
  ```
  ibmcloud ks vlans --zone <zone>
  ```
  {: pre}

  Output di esempio:
  ```
  ID        Name                Number   Type      Router  
        1519999   vlan   1355     private   bcr02a.dal10  
        1519898   vlan   1357     private   bcr02a.dal10 
        1518787   vlan   1252     public   fcr02a.dal10 
        1518888   vlan   1254     public    fcr02a.dal10
  ```
  {: screen}
  * Per creare un cluster in cui puoi eseguire carichi di lavoro con connessione Internet, verifica se esiste una VLAN pubblica e privata. Se esiste già una VLAN pubblica e privata, annota i router corrispondenti. I router della VLAN privata iniziano sempre con <code>bcr</code> (router back-end) e i router
della VLAN pubblica iniziano sempre con <code>fcr</code> (router front-end). Quando crei un cluster e specifichi le VLAN private e pubbliche, la combinazione di numero e lettera dopo tali prefissi deve corrispondere. Nell'output di esempio, le VLAN private
possono essere utilizzate con una qualsiasi VLAN pubblica perché i router includono tutti
`02a.dal10`.
  * Per creare un cluster che estenda il tuo data center in loco solo sulla rete privata o che estenda il tuo data center in loco con la possibilità di aggiungere un accesso pubblico limitato in seguito tramite nodi di lavoro edge o che estenda il tuo data center in loco e fornisca un accesso pubblico limitato attraverso un dispositivo gateway, verifica se esiste una VLAN privata. Se esiste, annotane l'ID.

5. Esegui il comando `cluster-create`. Per impostazione predefinita, i dischi del nodo di lavoro sono crittografati con AES a 256 bit e il costo del cluster viene addebitato in base alle ore di utilizzo.
  * Per creare un cluster in cui puoi eseguire carichi di lavoro con connessione Internet:
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers <number> --name <cluster_name> --kube-version <major.minor.patch> [--private-service-endpoint] [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * Per creare un cluster che estenda il tuo data center in loco sulla rete privata, con la possibilità di aggiungere un accesso pubblico limitato in seguito attraverso nodi di lavoro edge:
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --private-service-endpoint [--public-service-endpoint] [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}
  * Per creare un cluster che estenda il tuo data center in loco e fornisca un accesso pubblico limitato attraverso un dispositivo gateway:
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --public-service-endpoint [--disable-disk-encrypt] [--trusted]
    ```
    {: pre}

    <table>
    <caption>Componenti di cluster-create</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>Il comando per creare un cluster nella tua organizzazione {{site.data.keyword.Bluemix_notm}}.</td>
    </tr>
    <tr>
    <td><code>--zone <em>&lt;zone&gt;</em></code></td>
    <td>Specifica l'ID zona {{site.data.keyword.Bluemix_notm}} in cui vuoi creare il cluster che hai scelto nel passo 4.</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>Specifica il tipo di macchina che hai scelto nel passo 5.</td>
    </tr>
    <tr>
    <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
    <td>Specifica il livello di isolamento hardware per il tuo nodo di lavoro. Utilizza dedicato per avere risorse fisiche dedicate disponibili solo per te o condiviso per consentire la condivisione delle risorse fisiche con altri clienti IBM. L'impostazione predefinita è shared. Questo valore è facoltativo per i cluster standard VM. Per i tipi di macchina bare metal, specifica `dedicated`.</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>Se hai già una VLAN pubblica configurata nel tuo account dell'infrastruttura IBM Cloud (SoftLayer) per quella zona, immetti l'ID della VLAN pubblica individuato nel passo 4.<p>I router della VLAN privata iniziano sempre con <code>bcr</code> (router back-end) e i router
della VLAN pubblica iniziano sempre con <code>fcr</code> (router front-end). Quando crei un cluster e specifichi le VLAN private e pubbliche, la combinazione di numero e lettera dopo tali prefissi deve corrispondere.</p></td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>Se hai già una VLAN privata configurata nel tuo account dell'infrastruttura IBM Cloud (SoftLayer) per quella zona, immetti l'ID della VLAN privata individuato nel passo 4. Se non hai una VLAN privata nel tuo account, non utilizzare questa opzione. {{site.data.keyword.containerlong_notm}} crea automaticamente una VLAN privata per te.<p>I router della VLAN privata iniziano sempre con <code>bcr</code> (router back-end) e i router
della VLAN pubblica iniziano sempre con <code>fcr</code> (router front-end). Quando crei un cluster e specifichi le VLAN private e pubbliche, la combinazione di numero e lettera dopo tali prefissi deve corrispondere.</p></td>
    </tr>
    <tr>
    <td><code>--private-only </code></td>
    <td>Crea il cluster solo con VLAN private. Se includi questa opzione, non includere l'opzione <code>--public-vlan</code>.</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>Specifica un nome per il cluster. Il nome deve iniziare con una lettera, può contenere lettere, numeri e un trattino (-) e non può contenere più di 35 caratteri. Utilizza un nome che sia univoco tra le regioni. Il nome cluster e la regione in cui viene distribuito il cluster formano il nome dominio completo per il dominio secondario Ingress. Per garantire che il dominio secondario Ingress sia univoco all'interno di una regione, è possibile che il nome cluster venga troncato e vi venga aggiunto un valore casuale all'interno del nome dominio Ingress.
</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>Specifica il numero di nodi di lavoro da includere nel cluster. Se non viene specificata l'opzione <code>--workers</code>, viene creato 1 nodo di lavoro.</td>
    </tr>
    <tr>
    <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
    <td>La versione di Kubernetes per il nodo master del cluster. Questo valore è facoltativo. Quando la versione non viene specificata, il cluster viene creato con l'impostazione predefinita delle versioni di Kubernetes supportate. Per visualizzare le versioni disponibili, esegui <code>ibmcloud ks versions</code>.
</td>
    </tr>
    <tr>
    <td><code>--private-service-endpoint</code></td>
    <td>**In [account abilitati per VRF](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)**: abilita l'endpoint del servizio privato in modo che il tuo master Kubernetes e i nodi di lavoro possano comunicare sulla VLAN privata. Inoltre, puoi scegliere di abilitare l'endpoint del servizio pubblico utilizzando l'indicatore `--public-service-endpoint` per accedere al tuo cluster su Internet. Se abiliti solo l'endpoint del servizio privato, devi essere connesso alla VLAN privata per poter comunicare con il tuo master Kubernetes. Dopo aver abilitato un endpoint del servizio privato, non puoi disabilitarlo in seguito.<br><br>Dopo aver creato il cluster, puoi ottenere l'endpoint eseguendo `ibmcloud ks cluster-get --cluster<cluster_name_or_ID>`.</td>
    </tr>
    <tr>
    <td><code>--public-service-endpoint</code></td>
    <td>Abilita l'endpoint del servizio pubblico in modo che il tuo master Kubernetes sia accessibile tramite la rete pubblica, ad esempio per eseguire i comandi `kubectl` dal tuo terminale e che il tuo master Kubernetes e i tuoi nodi di lavoro possano comunicare sulla VLAN pubblica. Puoi disabilitare in seguito l'endpoint del servizio pubblico se vuoi un cluster solo privato.<br><br>Dopo aver creato il cluster, puoi ottenere l'endpoint eseguendo `ibmcloud ks cluster-get --cluster<cluster_name_or_ID>`.</td>
    </tr>
    <tr>
    <td><code>--disable-disk-encrypt</code></td>
    <td>I nodi di lavoro sono dotati di [crittografia disco](/docs/containers?topic=containers-security#encrypted_disk) AES a 256 bit per impostazione predefinita. Se vuoi disabilitare la crittografia, includi questa opzione.</td>
    </tr>
    <tr>
    <td><code>--trusted</code></td>
    <td>**Cluster bare metal**: abilita [Trusted Compute](/docs/containers?topic=containers-security#trusted_compute) per verificare i tentativi di intrusione nei tuoi nodi di lavoro bare metal. Trusted Compute è disponibile per alcuni tipi di macchina bare metal selezionati. Ad esempio, le varietà GPU `mgXc` non supportano Trusted Compute. Se non abiliti l'attendibilità durante la creazione del cluster ma vuoi farlo in seguito, puoi usare il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Dopo aver abilitato l'attendibilità, non puoi disabilitarla successivamente.</td>
    </tr>
    </tbody></table>

6. Verifica che la creazione del cluster sia stata richiesta. Per le macchine virtuali, possono essere necessari alcuni minuti per l'ordine delle macchine del nodo di lavoro e per la configurazione e il provisioning del cluster nel tuo account. Il provisioning delle macchine fisiche bare metal viene eseguito tramite l'interazione manuale con l'infrastruttura Cloud IBM (SoftLayer) e il completamento di questo processo può richiedere più di un giorno lavorativo.
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    Quando è stato terminato il provisioning del tuo cluster, lo stato del tuo cluster viene modificato in **distribuito**.
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.6      Default
    ```
    {: screen}

7. Controlla lo stato dei nodi di lavoro.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Quando i nodi di lavoro sono pronti, la condizione passa a **normal** e lo stato è **Ready**. Quando lo stato del nodo è **Ready**, puoi accedere al cluster. Nota che anche se il cluster è pronto, alcune parti del cluster utilizzate da altri servizi come i segreti Ingress o i segreti di pull dell'immagine di registro potrebbero essere ancora in elaborazione. Nota che se hai creato il tuo cluster solo con una VLAN privata, ai tuoi nodi di lavoro non viene assegnato nessun indirizzo **IP pubblico**.

    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   standard       normal   Ready    mil01       1.13.6      Default
    ```
    {: screen}

    A ogni nodo di lavoro viene assegnato un ID e nome dominio univoco che non deve essere modificato manualmente dopo la creazione del cluster. La modifica dell'ID o del nome dominio impedisce al master Kubernetes di gestire il tuo cluster.
    {: important}

8. Una volta che il tuo cluster è stato creato, puoi [iniziare a utilizzarlo configurando la sessione della tua CLI](#access_cluster).

<br />


## Accesso al tuo cluster
{: #access_cluster}

Una volta che il tuo cluster è stato creato, puoi iniziare a utilizzarlo configurando la sessione della tua CLI.
{: shortdesc}

### Accesso ai cluster attraverso l'endpoint del servizio pubblico
{: #access_internet}

Per utilizzare il tuo cluster, imposta il cluster che hai creato come contesto per una sessione CLI per l'esecuzione di comandi `kubectl`.
{: shortdesc}

1. Se la tua rete è protetta da un firewall aziendale, consenti l'accesso alle porte e agli endpoint delle API {{site.data.keyword.containerlong_notm}} e {{site.data.keyword.Bluemix_notm}}.
  1. [Consenti l'accesso agli endpoint pubblici per l'API `ibmcloud` e l'API `ibmcloud ks` nel tuo firewall](/docs/containers?topic=containers-firewall#firewall_bx).
  2. [Consenti agli utenti del cluster autorizzati di eseguire i comandi `kubectl`](/docs/containers?topic=containers-firewall#firewall_kubectl) per accedere al master solo tramite l'endpoint del servizio pubblico o privato o tramite gli endpoint del servizio pubblico e privato.
  3. [Consenti agli utenti del cluster autorizzati di eseguire i comandi `calicotl`](/docs/containers?topic=containers-firewall#firewall_calicoctl) per gestire le politiche di rete Calico nel tuo cluster.

2. Configura il cluster che hai creato come il contesto per questa sessione. Completa questi passi di configurazione ogni volta che lavori con il tuo cluster.

  Se invece desideri utilizzare la console {{site.data.keyword.Bluemix_notm}}, puoi eseguire i comandi CLI direttamente dal tuo browser Web nel [terminale Kubernetes](/docs/containers?topic=containers-cs_cli_install#cli_web).
  {: tip}
  1. Richiama il comando per impostare la variabile di ambiente e scaricare i file di configurazione Kubernetes.
      ```
      ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
      ```
      {: pre}
      Quando il download dei file di configurazione è terminato, viene visualizzato un comando che puoi utilizzare per impostare il percorso al file di configurazione di Kubernetes locale come una variabile di ambiente.

      Esempio per OS X:
      ```
      export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}
  2. Copia e incolla il comando visualizzato nel tuo terminale per impostare la variabile di ambiente `KUBECONFIG`.
  3. Verifica che la variabile di ambiente `KUBECONFIG` sia impostata correttamente.
      Esempio per OS X:
      ```
      echo $KUBECONFIG
      ```
      {: pre}

      Output:
      ```
      /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}

3. Avvia il tuo dashboard Kubernetes con la porta predefinita `8001`.
  1. Imposta il proxy con il numero di porta predefinito.
      ```
      kubectl proxy
      ```
      {: pre}

      ```
      Inizio di utilizzo su 127.0.0.1:8001
      ```
      {: screen}

  2.  Apri il seguente URL in un browser web per visualizzare il dashboard Kubernetes.
      ```
      http://localhost:8001/ui
      ```
      {: codeblock}

### Accesso ai cluster attraverso l'endpoint del servizio privato
{: #access_on_prem}

Il master Kubernetes è accessibile tramite l'endpoint del servizio privato se gli utenti del cluster autorizzati si trovano nella tua rete privata {{site.data.keyword.Bluemix_notm}} o sono connessi alla rete privata tramite una [connessione VPN](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) o [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). Tuttavia, le comunicazioni con il master Kubernetes sull'endpoint del servizio privato devono passare attraverso l'intervallo di indirizzi IP <code>166.X.X.X</code>, che non è instradabile da una connessione VPN o tramite {{site.data.keyword.Bluemix_notm}} Direct Link. Puoi esporre l'endpoint del servizio privato del master per gli utenti del tuo cluster utilizzando un NLB (network load balancer) privato. L'NLB privato espone l'endpoint del servizio privato del master come intervallo di indirizzi IP <code>10.X.X.X</code> interno a cui gli utenti possono accedere con la connessione VPN o {{site.data.keyword.Bluemix_notm}} Direct Link. Se abiliti solo l'endpoint del servizio privato, puoi utilizzare il dashboard Kubernetes o abilitare temporaneamente l'endpoint del servizio pubblico per creare l'NLB privato.
{: shortdesc}

1. Se la tua rete è protetta da un firewall aziendale, consenti l'accesso alle porte e agli endpoint delle API {{site.data.keyword.containerlong_notm}} e {{site.data.keyword.Bluemix_notm}}.
  1. [Consenti l'accesso agli endpoint pubblici per l'API `ibmcloud` e l'API `ibmcloud ks` nel tuo firewall](/docs/containers?topic=containers-firewall#firewall_bx).
  2. [Consenti agli utenti del cluster autorizzati di eseguire i comandi `kubectl`](/docs/containers?topic=containers-firewall#firewall_kubectl). Nota che non puoi testare la connessione al tuo cluster nel passo 6, finché non esponi l'endpoint del servizio privato del master al cluster utilizzando un NLB privato.

2. Ottieni l'URL e la porta dell'endpoint del servizio privato per il tuo cluster.
  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  In questo output di esempio, l'**URL dell'endpoint del servizio privato** è `https://c1.private.us-east.containers.cloud.ibm.com:25073`.
  ```
  Name:                           setest
  ID:                             b8dcc56743394fd19c9f3db7b990e5e3
  State:                          normal
  Created:                        2019-04-25T16:03:34+0000
  Location:                       wdc04
  Master URL:                     https://c1.private.us-east.containers.cloud.ibm.com:25073
  Public Service Endpoint URL:    -
  Private Service Endpoint URL:   https://c1.private.us-east.containers.cloud.ibm.com:25073
  Master Location:                Washington D.C.
  ...
  ```
  {: screen}

3. Crea un file YAML denominato `kube-api-via-nlb.yaml`. Questo file YAML crea un servizio `LoadBalancer` privato ed espone l'endpoint del servizio privato attraverso tale NLB. Sostituisci `<private_service_endpoint_port>` con la porta individuata nel passo precedente.
  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: kube-api-via-nlb
    annotations:
      service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: private
    namespace: default
  spec:
    type: LoadBalancer
    ports:
    - protocol: TCP
      port: <private_service_endpoint_port>
      targetPort: <private_service_endpoint_port>
  ---
  kind: Endpoints
  apiVersion: v1
  metadata:
    name: kube-api-via-nlb
  subsets:
    - addresses:
        - ip: 172.20.0.1
      ports:
        - port: 2040
  ```
  {: codeblock}

4. Per creare l'NLB privato, è necessario essere connessi al master del cluster. Poiché non puoi ancora connetterti attraverso l'endpoint del servizio privato da una VPN o da {{site.data.keyword.Bluemix_notm}} Direct Link, devi connettersi al master del cluster e creare l'NLB utilizzando l'endpoint del servizio pubblico o il dashboard Kubernetes.
  * Se hai abilitati solo l'endpoint del servizio privato, puoi utilizzare il dashboard Kubernetes per creare l'NLB. Il dashboard instrada automaticamente tutte le richieste all'endpoint del servizio privato del master.
    1.  Accedi alla [console {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/).
    2.  Dalla barra dei menu, seleziona l'account che vuoi utilizzare.
    3.  Dal menu ![Icona Menu](../icons/icon_hamburger.svg "Icona Menu"), fai clic su **Kubernetes**.
    4.  Nella pagina **Cluster**, fai clic sul cluster a cui vuoi accedere.
    5.  Dalla pagina dei dettagli del cluster, fai clic su **Dashboard Kubernetes**.
    6.  Fai clic su **+ Crea**.
    7.  Seleziona **Crea da file**, carica il file `kube-api-via-nlb.yaml` e fai clic su **Carica**.
    8.  Nella pagina **Panoramica**, verifica che venga creato il servizio `kube-api-via-nlb`. Nella colonna **Endpoint esterni**, annotare l'indirizzo `10.x.x.x`. Questo indirizzo IP espone l'endpoint del servizio privato per il master Kubernetes sulla porta che hai specificato nel tuo file YAML.

  * Se hai abilitato anche l'endpoint del servizio pubblico, hai già accesso al master.
    1. Crea l'NLB e l'endpoint.
      ```
      kubectl apply -f kube-api-via-nlb.yaml
      ```
      {: pre}
    2. Verifica che l'NLB `kube-api-via-nlb` venga creato. Nell'output, annota l'indirizzo `10.x.x.x.x` **EXTERNAL-IP**. Questo indirizzo IP espone l'endpoint del servizio privato per il master Kubernetes sulla porta che hai specificato nel tuo file YAML.
      ```
      kubectl get svc -o wide
      ```
      {: pre}

      In questo output di esempio, l'indirizzo IP per l'endpoint del servizio privato del master Kubernetes è `10.186.92.42`.
      ```
      NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)          AGE   SELECTOR
      kube-api-via-nlb         LoadBalancer   172.21.150.118   10.186.92.42     443:32235/TCP    10m   <none>
      ...
      ```
      {: screen}

  <p class="note">Se vuoi connettere il master attraverso il [servizio VPN strongSwan](/docs/containers?topic=containers-vpn#vpn-setup), annota invece l'**IP cluster** `172.21.x.x` da utilizzare nel passo successivo. Poiché il pod VPN strongSwan viene eseguito all'interno del tuo cluster, può accedere all'NLB attraverso l'indirizzo IP del servizio IP del cluster interno. Nel tuo file `config.yaml` per il grafico Helm strongSwan, verifica che il CIDR della sottorete del servizio Kubernetes, `172.21.0.0/16`, sia elencato nell'impostazione `local.subnet` .</p>

5. Sulle macchine client in cui tu o i tuoi utenti eseguite i comandi `kubectl`, aggiungi l'indirizzo IP dell'NLB e l'URL dell'endpoint del servizio privato al file `/etc/hosts`. Non includere le porte nell'indirizzo IP e nell'URL, né `https://` nell'URL.
  * Per gli utenti OSX e Linux:
    ```
    sudo nano /etc/hosts
    ```
    {: pre}
  * Per gli utenti Windows:
    ```
    notepad C:\Windows\System32\drivers\etc\hosts
    ```
    {: pre}

    A seconda delle autorizzazioni della tua macchina locale, potresti dover eseguire Blocco note come amministratore, per modificare il file degli host.
    {: tip}

  Testo di esempio da aggiungere:
  ```
  10.186.92.42      c1.private.us-east.containers.cloud.ibm.com
  ```
  {: codeblock}

6. Verifica di essere connesso alla rete privata attraverso una connessione VPN o {{site.data.keyword.Bluemix_notm}} Direct Link.

7. Verifica che i comandi `kubectl` siano eseguiti correttamente con il tuo cluster attraverso l'endpoint del servizio privato, controllando la versione del server della CLI Kubernetes.
  ```
  kubectl version --short
  ```
  {: pre}

  Output di esempio:
  ```
  Client Version: v1.13.6
  Server Version: v1.13.6
  ```
  {: screen}

<br />


## Passi successivi
{: #next_steps}

Quando il cluster è attivo e in esecuzione, puoi controllare le seguenti attività:
- Se hai creato il cluster in una zona con supporto multizona, [estendi i nodi di lavoro aggiungendo una zona al tuo cluster](/docs/containers?topic=containers-add_workers).
- [Distribuisci un'applicazione nel tuo cluster.](/docs/containers?topic=containers-app#app_cli)
- [Configura il tuo registro privato in {{site.data.keyword.Bluemix_notm}} per memorizzare e condividere le immagini Docker con altri utenti.](/docs/services/Registry?topic=registry-getting-started)
- [Configura il cluster autoscaler](/docs/containers?topic=containers-ca#ca) per aggiungere o rimuovere automaticamente i nodi di lavoro dai pool di nodi di lavoro in base alle tue richieste di risorse del carico di lavoro.
- Controlla chi può creare i pod nel tuo cluster con le [politiche di sicurezza del pod](/docs/containers?topic=containers-psp).
- Abilita i componenti aggiuntivi gestiti [Istio](/docs/containers?topic=containers-istio) e [Knative](/docs/containers?topic=containers-serverless-apps-knative) per estendere le funzionalità del tuo cluster.

Quindi, puoi verificare i seguenti passi di configurazione della rete per la configurazione del tuo cluster:

### Esecuzione dei carichi di lavoro delle applicazioni con connessione Internet in un cluster
{: #next_steps_internet}

* Isola i carichi di lavoro di rete nei [nodi di lavoro edge](/docs/containers?topic=containers-edge).
* Esponi le tue applicazioni con [servizi di rete pubblici](/docs/containers?topic=containers-cs_network_planning#public_access).
* Controlla il traffico pubblico verso i servizi di rete che espongono le tue applicazioni creando[politiche pre-DNAT di Calico](/docs/containers?topic=containers-network_policies#block_ingress), quali le politiche di whitelist e blacklist.
* Connetti il tuo cluster con servizi in reti private al di fuori del tuo account {{site.data.keyword.Bluemix_notm}} configurando un[servizio VPN IPSec strongSwan](/docs/containers?topic=containers-vpn).

### Estensione del tuo data center in loco a un cluster e autorizzazione di un accesso pubblico limitato attraverso nodi edge e politiche di rete di Calico
{: #next_steps_calico}

* Connetti il tuo cluster con servizi in reti private al di fuori del tuo account {{site.data.keyword.Bluemix_notm}} configurando [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) o il [servizio VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup). {{site.data.keyword.Bluemix_notm}} Direct Link consente la comunicazione tra applicazioni e servizi del tuo cluster e una rete locale sulla rete privata, mentre strongSwan consente la comunicazione attraverso un tunnel VPN criptato sulla rete pubblica.
* Isola i carichi di lavoro della rete pubblica creando un [pool di nodi di lavoro edge](/docs/containers?topic=containers-edge) con i nodi di lavoro collegati a VLAN pubbliche e private.
* Esponi le tue applicazioni con [servizi di rete pubblici](/docs/containers?topic=containers-cs_network_planning#private_access).
* [Crea politiche di rete host di Calico](/docs/containers?topic=containers-network_policies#isolate_workers) per bloccare l'accesso pubblico ai pod, isolare il tuo cluster sulla rete privata e consentire l'accesso ad altri servizi {{site.data.keyword.Bluemix_notm}}.

### Estensione del tuo data center in loco a un cluster e autorizzazione di un accesso pubblico limitato attraverso un dispositivo Gateway
{: #next_steps_gateway}

* Se configuri anche un firewall gateway per la rete privata, devi [consentire le comunicazioni tra i nodi di lavoro e permettere al tuo cluster di accedere alle risorse dell'infrastruttura sulla rete privata](/docs/containers?topic=containers-firewall#firewall_private).
* Per connettere in modo sicuro i tuoi nodi di lavoro e le applicazioni a reti private al di fuori del tuo account {{site.data.keyword.Bluemix_notm}}, configura un endpoint VPN IPSec nel tuo dispositivo gateway. Quindi, [configura il servizio VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) nel tuo cluster per utilizzare l'endpoint VPN sul tuo gateway o [configura la connettività VPN direttamente con una VRA](/docs/containers?topic=containers-vpn#vyatta).
* Esponi le tue applicazioni con [servizi di rete pubblici](/docs/containers?topic=containers-cs_network_planning#private_access).
* [Apri le porte e gli indirizzi IP richiesti](/docs/containers?topic=containers-firewall#firewall_inbound) del firewall del tuo dispositivo gateway per consentire il traffico in entrata ai servizi di rete.

### Estensione del tuo data center in loco a un cluster solo sulla rete privata
{: #next_steps_extend}

* Se hai un firewall sulla rete privata, [consenti le comunicazioni tra i nodi di lavoro e permetti al tuo cluster di accedere alle risorse dell'infrastruttura sulla rete privata](/docs/containers?topic=containers-firewall#firewall_private).
* Connetti il tuo cluster con servizi in reti private al di fuori del tuo account {{site.data.keyword.Bluemix_notm}} configurando [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link).
* Esponi le tue applicazioni sulla rete privata con [servizi di rete privati](/docs/containers?topic=containers-cs_network_planning#private_access).
