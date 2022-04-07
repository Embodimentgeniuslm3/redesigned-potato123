---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

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


# Configurazione della connettività VPN
{: #vpn}

Con la connettività VPN, puoi collegare in modo sicuro le applicazioni di un cluster Kubernetes su {{site.data.keyword.containerlong}} a una rete in loco. Puoi anche collegare le applicazioni esterne al tuo cluster a un'applicazione in esecuzione nel tuo cluster.
{:shortdesc}

Per collegare i tuoi nodi di lavoro e applicazioni a un data center in loco, puoi configurare una delle seguenti opzioni.

- **Servizio VPN IPSec strongSwan**: puoi configurare un [Servizio VPN IPSec strongSwan![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.strongswan.org/about.html) che collega in modo sicuro il tuo cluster Kubernetes a una rete in loco. Il servizio VPN IPSec strongSwan fornisce un canale di comunicazione end-to-end protetto su Internet basato sulla suite di protocolli IPSec (Internet Protocol Security) standard del settore. Per configurare una connessione protetta tra il tuo cluster e una rete in loco, [configura e distribuisci il servizio VPN IPSec strongSwan](#vpn-setup) direttamente in un pod nel tuo cluster.

- **{{site.data.keyword.BluDirectLink}}**: [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link) ti consente di creare una connessione diretta e privata tra i tuoi ambienti di rete remoti e {{site.data.keyword.containerlong_notm}} senza instradamento su internet pubblico. Le offerte {{site.data.keyword.Bluemix_notm}} Direct Link sono utili quando devi implementare carichi di lavoro ibridi, carichi di lavoro tra provider, trasferimenti di dati di grandi dimensioni o frequenti oppure carichi di lavoro privati. Per scegliere un'offerta {{site.data.keyword.Bluemix_notm}} Direct Link e configurare una connessione {{site.data.keyword.Bluemix_notm}} Direct Link, vedi [Introduzione a IBM Cloud {{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-) nella documentazione di {{site.data.keyword.Bluemix_notm}} Direct Link.

- **VRA (Virtual Router Appliance) o FSA (Fortigate Security Appliance)**: Potresti scegliere di configurare una [VRA (Vyatta)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) o una [FSA](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations) per configurare un endpoint VPN IPSec. Questa opzione è utile quando hai un cluster più grande, vuoi accedere a più cluster su una singola VPN o hai bisogno di una VPN basata sugli instradamenti. Per configurare una VRA, vedi [Configurazione della connettività VPN con VRA](#vyatta).

## Utilizzo del grafico Helm del servizio VPN IPSec strongSwan
{: #vpn-setup}

Utilizza un grafico Helm per configurare e distribuire il servizio VPN IPSec strongSwan all'interno di un pod Kubernetes.
{:shortdesc}

Poiché strongSwan è integrato con il tuo cluster, non hai bisogno di un dispositivo gateway esterno. Quando viene stabilita la connettività VPN, gli instradamenti vengono configurati automaticamente su tutti i nodi di lavoro nel cluster. Questi instradamenti consentono una connettività a due vie tramite il tunnel VPN tra i pod su un qualsiasi nodo di lavoro e il sistema remoto. Ad esempio, il seguente diagramma mostra in che modo un'applicazione in {{site.data.keyword.containerlong_notm}} può comunicare con un server in loco tramite una connessione VPN strongSwan:

<img src="images/cs_vpn_strongswan.png" width="700" alt="Esponi un'applicazione in {{site.data.keyword.containerlong_notm}} utilizzando un NLB (network load balancer)" style="width:700px; border-style: none"/>

1. Un'applicazione nel tuo cluster, `myapp`, riceve una richiesta da un servizio Ingress o LoadBalancer e deve connettersi in modo sicuro alla tua rete in loco.

2. La richiesta al data center in loco viene inoltrata al pod VPN strongSwan IPSec. L'indirizzo IP di destinazione viene utilizzato per determinare quali pacchetti di rete inviare al pod VPN strongSwan IPSec.

3. La richiesta viene crittografata e inviata sul tunnel VPN nel data center in loco.

4. La richiesta in entrata passa attraverso il firewall in loco e viene consegnata all'endpoint del tunnel VPN (router) in cui viene decrittografata.

5. L'endpoint del tunnel VPN (router) inoltra la richiesta al mainframe o al server in loco a seconda dell'indirizzo IP di destinazione specificato nel passo 2. I dati necessari vengono reinviati tramite la connessione VPN a `myapp` seguendo lo stesso processo.

## Considerazioni sul servizio VPN strongSwan
{: #strongswan_limitations}

Prima di utilizzare il grafico Helm strongSwan, esamina le considerazioni e le limitazioni riportate di seguito.
{: shortdesc}

* Il grafico Helm strongSwan richiede che l'endpoint VPN remoto abiliti l'attraversamento NAT. L'attraversamento NAT richiede la porta UDP 4500 in aggiunta alla porta UDP IPSec predefinita 500. Entrambe le porte UDP devono essere consentite attraverso qualsiasi firewall configurato.
* Il grafico Helm strongSwan non supporta le VPN IPSec basate sugli instradamenti.
* Il grafico Helm strongSwan supporta le VPN IPSec che utilizzano chiavi precondivise ma non supporta le VPN IPSec che richiedono i certificati.
* Il grafico Helm strongSwan non consente a più cluster e ad altre risorse IaaS di condividere una singola connessione VPN.
* Il grafico Helm strongSwan viene eseguito come un pod Kubernetes all'interno del cluster. Le prestazioni della VPN sono influenzate dall'utilizzo della memoria e della rete di Kubernetes e di altri pod che sono in esecuzione nel cluster. Se hai un ambiente critico per le prestazioni, considera l'utilizzo di una soluzione VPN eseguita esternamente al cluster su hardware dedicato.
* Il grafico Helm strongSwan esegue un singolo pod VPN come endpoint del tunnel IPSec. Se si verifica un malfunzionamento del pod, il cluster lo riavvia. Tuttavia, potresti riscontrare un breve tempo di inattività mentre il nuovo pod viene avviato e la connessione VPN viene ristabilita. Se hai bisogno di un ripristino a seguito di un errore più rapido oppure una soluzione ad alta disponibilità più elaborata, considera l'utilizzo di una soluzione VPN eseguita esternamente al cluster su hardware dedicato.
* Il grafico Helm strongSwan non fornisce le metriche o il monitoraggio del traffico di rete che transita sulla connessione VPN. Per un elenco degli strumenti di monitoraggio supportati, consulta [Servizi di registrazione e monitoraggio](/docs/containers?topic=containers-supported_integrations#health_services).

I tuoi utenti del cluster possono utilizzare il servizio VPN strongSwan per stabilire una connessione al tuo master Kubernetes tramite l'endpoint del servizio privato. Tuttavia, le comunicazioni con il master Kubernetes sull'endpoint del servizio privato devono passare per l'intervallo di indirizzi IP <code>166.X.X.X</code>, che non è instradabile da una connessione VPN. Puoi esporre l'endpoint del servizio privato del master per i tuoi utenti del cluster [utilizzando un NLB (network load balancer) privato](/docs/containers?topic=containers-clusters#access_on_prem). L'NLB privato espone l'endpoint del servizio privato del master come un indirizzo IP del cluster `172.21.x.x` interno a cui il pod VPN strongSwan può accedere. Se abiliti solo l'endpoint del servizio privato, puoi utilizzare il dashboard Kubernetes o abilitare temporaneamente l'endpoint del servizio pubblico per creare l'NLB privato.
{: tip}

<br />


## Configurazione della VPN strongSwan in un cluster multizona
{: #vpn_multizone}

I cluster multizona forniscono l'alta disponibilità per le applicazioni nel caso in cui si verificasse un'interruzione rendendo le istanze applicazione disponibili sui nodi di lavoro in più zone. Tuttavia, la configurazione del servizio VPN strongSwan in un cluster multizona è più complessa della configurazione di strongSwan in un cluster a zona singola.
{: shortdesc}

Prima di configurare strongSwan in un cluster multizona, prova a distribuire un grafico Helm strongSwan in un cluster a zona singola. Quando stabilisci inizialmente una connessione VPN tra un cluster a zona singola e una rete in loco, puoi determinare più facilmente le impostazioni del firewall di rete remota che sono importanti per una configurazione strongSwan multizona:
* Alcuni endpoint VPN remoti hanno impostazioni quali `leftid` o `rightid` nel file `ipsec.conf`. Se hai queste impostazioni, controlla se devi impostare il `leftid` sull'indirizzo IP del tunnel IPSec VPN.
*	Se la connessione è in entrata al cluster dalla rete remota, controlla se l'endpoint VPN remoto può ristabilire la connessione VPN a un indirizzo IP differente in caso di malfunzionamento del programma di bilanciamento del carico in una zona.

Per iniziare con uno strongSwan in un cluster multizona, scegli una delle seguenti opzioni:
* Se puoi utilizzare una connessione VPN in uscita, puoi scegliere di configurare solo una distribuzione VPN StrongSwan. Vedi [Configurazione di una singola connessione VPN in uscita da un cluster multizona](#multizone_one_outbound).
* Se hai bisogno di una connessione VPN in entrata, le impostazioni di configurazione che puoi utilizzare variano a seconda del fatto che tu possa configurare l'endpoint VPN remoto per ristabilire la connessione VPN a un altro IP di programma di bilanciamento del carico pubblico quando viene rilevata un'interruzione.
  * Se l'endpoint VPN remoto può ristabilire automaticamente la connessione VPN a un altro IP, puoi scegliere di configurare solo una singola distribuzione VPN strongSwan. Vedi [Configurazione di una singola connessione VPN in entrata a un cluster multizona](#multizone_one_inbound).
  * Se l'endpoint VPN remoto non può ristabilire automaticamente la connessione VPN a un altro IP, devi distribuire un servizio VPN strongSwan in entrata separato in ciascuna zona. Vedi [Configurazione di una connessione VPN in ciascuna zona di un cluster multizona](#multizone_multiple).

Prova a configurare il tuo ambiente in modo da aver bisogno solo di una singola distribuzione VPN strongSwan per una connessione VPN in uscita o in entrata al tuo cluster multizona. Se devi configurare delle VPN strongSwan separate in ciascuna zona, assicurati di pianificare come gestire questa complessità aggiunta e questo utilizzo di risorse aumentato.
{: note}

### Configurazione di una singola connessione VPN in uscita da un cluster multizona
{: #multizone_one_outbound}

La soluzione più semplice per configurare il servizio VPN strongSwan in un cluster multizona consiste nell'utilizzare una singola connessione VPN in uscita che fluttua tra diversi nodi di lavoro in tutte le zone di disponibilità nel tuo cluster.
{: shortdesc}

Quando la connessione VPN è in uscita dal cluster multizona, è necessaria solo una singola distribuzione strongSwan. Se un nodo di lavoro viene rimosso o se riscontra dei tempi di inattività, `kubelet` ripianifica il pod VPN su un nuovo nodo di lavoro. Se una zona di disponibilità riscontra un'interruzione, `kubelet` ripianifica il pod VPN su un nuovo nodo di lavoro in una zona differente.

1. [Configura un grafico Helm VPN strongSwan](/docs/containers?topic=containers-vpn#vpn_configure). Quando ti attieni alla procedura indicata in tale sezione, assicurati di specificare le seguenti impostazioni:
    - `ipsec.auto`: modifica in `start`. Le connessioni sono in uscita dal cluster.
    - `loadBalancerIP`: non specificare un indirizzo IP. Lascia questa impostazione vuota.
    - `zoneLoadBalancer`: specifica un indirizzo IP di programma di bilanciamento del carico pubblico per ciascuna zona dove hai dei nodi di lavoro. [Puoi verificare se visualizzare i tuoi indirizzi IP pubblici disponibili](/docs/containers?topic=containers-subnets#review_ip) o [liberare un indirizzo IP utilizzato](/docs/containers?topic=containers-subnets#free). Poiché il pod VPN strongSwan può essere pianificato su un nodo di lavoro in qualsiasi zona, questo elenco di IP garantisce che possa essere utilizzato un IP di programma di bilanciamento del carico in qualsiasi zona dove è pianificato il pod VPN.
    - `connectUsingLoadBalancerIP`: imposta su `true`. Quando il pod VPN strongSwan è pianificato su un nodo di lavoro, il servizio strongSwan seleziona l'indirizzo IP del programma di bilanciamento del carico che si trova nella stessa zona e utilizza questo IP per stabilire la connessione in uscita.
    - `local.id`: specifica un valore fisso che è supportato dal tuo endpoint VPN remoto. Se l'endpoint VPN remoto richiede che tu imposti l'opzione `local.id` (valore `leftid` in `ipsec.conf`) sull'indirizzo IP pubblico del tunnel IPSec VPN, imposta `local.id` su `%loadBalancerIP`. Questo valore configura automaticamente il valore `leftid` in `ipsec.conf` sull'indirizzo IP del programma di bilanciamento del carico utilizzato per la connessione.

2. Nel tuo firewall di rete remota, consenti le connessioni VPN IPSec in entrata dagli indirizzi IP pubblici che hai elencato nell'impostazione `zoneLoadBalancer`.

3. Configura l'endpoint VPN remoto per consentire una connessione VPN in entrata da ciascuno dei possibili IP di programma di bilanciamento del carico che hai elencato nell'impostazione `zoneLoadBalancer`.

### Configurazione d una singola connessione VPN in entrata da un cluster multizona
{: #multizone_one_inbound}

Quando hai bisogno di connessioni VPN in entrata e l'endpoint VPN remoto può ristabilire automaticamente la connessione VPN a un IP diverso quando viene rilevato un malfunzionamento, puoi utilizzare una singola connessione VPN in entrata che fluttua tra i diversi nodi di lavoro in tutte le zone di disponibilità nel tuo cluster.
{: shortdesc}

L'endpoint VPN remoto può stabilire la connessione VPN a uno qualsiasi dei programmi di bilanciamento del carico strongSwan in una qualsiasi delle zone. La richiesta in entrata viene inviata al pod VPN indipendentemente da quale sia la zona in cui si trova il pod VPN. Le risposte dal pod VPN vengono restituite tramite il programma di bilanciamento del carico originale all'endpoint VPN remoto. Questa opzione garantisce l'alta disponibilità perché `kubelet` ripianifica il pod VPN su un nuovo nodo di lavoro, se un nodo di lavoro viene rimosso o se riscontra dei tempi di inattività. Inoltre, se una zona di disponibilità riscontra un'interruzione, l'endpoint VPN remoto può ristabilire la connessione VPN all'indirizzo IP del programma di bilanciamento del carico in una zona differente in modo che sia ancora possibile raggiungere il pod VPN.

1. [Configura un grafico Helm VPN strongSwan](/docs/containers?topic=containers-vpn#vpn_configure). Quando ti attieni alla procedura indicata in tale sezione, assicurati di specificare le seguenti impostazioni:
    - `ipsec.auto`: modifica in `add`. Le connessioni sono in entrata al cluster.
    - `loadBalancerIP`: non specificare un indirizzo IP. Lascia questa impostazione vuota.
    - `zoneLoadBalancer`: specifica un indirizzo IP di programma di bilanciamento del carico pubblico per ciascuna zona dove hai dei nodi di lavoro. [Puoi verificare se visualizzare i tuoi indirizzi IP pubblici disponibili](/docs/containers?topic=containers-subnets#review_ip) o [liberare un indirizzo IP utilizzato](/docs/containers?topic=containers-subnets#free).
    - `local.id`: se l'endpoint VPN remoto richiede che tu imposti l'opzione `local.id` (valore `leftid` in `ipsec.conf`) sull'indirizzo IP pubblico del tunnel IPSec VPN, imposta `local.id` su `%loadBalancerIP`. Questo valore configura automaticamente il valore `leftid` in `ipsec.conf` sull'indirizzo IP del programma di bilanciamento del carico utilizzato per la connessione.

2. Nel tuo firewall di rete remota, consenti le connessioni VPN IPSec in uscita agli indirizzi IP pubblici che hai elencato nell'impostazione `zoneLoadBalancer`.

### Configurazione di una connessione VPN in entrata in ciascuna zona di un cluster multizona
{: #multizone_multiple}

Quando hai bisogno di connessioni PVN in entrata e l'endpoint VPN remoto non può ristabilire la connessione VPN a un IP differente, devi distribuire un servizio VPN strongSwan separato in ciascuna zona.
{: shortdesc}

L'endpoint VPN remoto deve essere aggiornato per stabilire una connessione VPN separata a un programma di bilanciamento del carico in ciascuna delle zone. Devi inoltre configurare le impostazioni specifiche per la zona sull'endpoint VPN remoto in modo che ciascuna di queste connessioni VPN sia univoca. Assicurati che queste connessioni VPN in entrata multiple rimangano sempre attive.

Dopo che hai distribuito ciascun grafico Helm, ogni distribuzione VPN strongSwan viene avviata come un servizio di programma di bilanciamento del carico Kubernetes nella zona corretta. Le richieste in entrata a tale IP pubblico sono inoltrate al pod VPN che è anche allocato nella stessa zona. Se la zona riscontra un'interruzione, le connessioni VPN stabilite nelle altre zone non ne risentono.

1. [Configura un grafico Helm VPN strongSwan](/docs/containers?topic=containers-vpn#vpn_configure) per ciascuna zona. Quando ti attieni alla procedura indicata in tale sezione, assicurati di specificare le seguenti impostazioni:
    - `loadBalancerIP`: specifica un indirizzo IP di programma di bilanciamento del carico pubblico disponibile che sia nella zona dove distribuisci questo servizio strongSwan. [Puoi verificare se visualizzare i tuoi indirizzi IP pubblici disponibili](/docs/containers?topic=containers-subnets#review_ip) o [liberare un indirizzo IP utilizzato](/docs/containers?topic=containers-subnets#free).
    - `zoneSelector`: specifica la zona dove desideri che sia pianificato il pod VPN.
    - Potrebbero essere necessarie delle ulteriori impostazioni, come `zoneSpecificRoutes`, `remoteSubnetNAT`, `localSubnetNAT` o `enableSingleSourceIP`, a seconda di quali siano le risorse che devono essere accessibili sulla VPN. Per ulteriori dettagli, vedi il prossimo passo.

2. Configurare le impostazioni specifiche per la zona su entrambi i lati del tunnel VPN per garantire che ciascuna connessione VPN sia univoca. A seconda di quali risorse devono essere accessibili sulla VPN, disponi di due opzioni per rendere le connessioni distinguibili:
    * Se i pod nel cluster devono accedere ai servizi sulla rete in loco remota:
      - `zoneSpecificRoutes`: imposta su `true`. Questa impostazione limita la connessione VPN a una singola zona nel cluster. I pod in una zona specifica utilizzano solo la connessione VPN configurata per quella specifica zona. Questa soluzione riduce il numero di pod strongSwan richiesti per supportare più VPN in un cluster multizona, migliora le prestazioni della VPN perché il traffico della VPN fluisce solo ai nodi di lavoro che si trovano nella zona corrente e assicura che la connettività VPN per ciascuna zona non sia influenzata dalla connettività VPN, dai pod per cui si è verificato un arresto anomalo o dalle interruzioni che si sono verificate in altre zone. Tieni presente che non è necessario configurare `remoteSubnetNAT`. Più VPN che utilizzano l'impostazione `zoneSpecificRoutes` possono avere la stessa sottorete remota (`remote.subnet`) perché l'instradamento è configurato in base a ogni singola zona.
      - `enableSingleSourceIP`: imposta su `true` e imposta la sottorete locale (`local.subnet`) su un singolo indirizzo IP /32. Questa combinazione di impostazioni nasconde tutti gli indirizzi IP privati del cluster dietro un singolo indirizzo IP /32. Questo indirizzo /32 univoco consente alla rete in loco remota di inviare risposte sulla connessione VPN corretta al pod corretto nel cluster che ha avviato la richiesta. Tieni presente che il singolo indirizzo IP /32 configurato per l'opzione (`local.subnet`) deve essere univoco in ciascuna configurazione VPN strongSwan.
    * Se le applicazioni nella rete in loco remota devono accedere ai servizi nel cluster:    
      - `localSubnetNAT`: assicurati che un'applicazione nella rete remota in loco possa selezionare una connessione VPN specifica per inviare e ricevere traffico nel cluster. In ciascuna configurazione Helm strongSwan, utilizza `localSubnetNAT` per identificare in modo univoco le risorse del cluster a cui può accedere l'applicazione in loco remota. Poiché vengono stabilite più VPN dalla rete in loco remota al cluster, devi aggiungere la logica all'applicazione sulla rete in loco in modo che possa selezionare quale VPN utilizzare quando accede ai servizi nel cluster. Tieni presente che i servizi nel cluster sono accessibili tramite molteplici sottoreti differenti, a seconda di quello che hai configurato per `localSubnetNAT` in ciascuna configurazione VPN strongSwan.
      - ` remoteSubnetNAT` assicurati che un pod nel tuo cluster utilizzi la stessa connessione VPN per restituire il traffico alla rete remota. In ogni file di distribuzione strongSwan, associa la sottorete in loco remota a una sottorete univoca utilizzando l'impostazione `remoteSubnetNAT`. Il traffico che viene ricevuto da un pod nel cluster da una `remoteSubnetNAT` specifica per la VPN viene restituito alla stessa `remoteSubnetNAT` specifica per la VPN e quindi su quella stessa connessione VPN.

3. Configura il software dell'endpoint VPN remoto per stabilire una connessione VPN separata al programma di bilanciamento del carico in ciascuna zona.

<br />


## Configurazione del grafico Helm strongSwan
{: #vpn_configure}

Prima di installare il grafico Helm strongSwan, devi decidere la configurazione di strongSwan.
{: shortdesc}

Prima di iniziare:
* Installa un gateway VPN IPSec nel tuo data center in loco.
* Assicurati di disporre del [ruolo del servizio {{site.data.keyword.Bluemix_notm}} IAM **Scrittore** o **Gestore**](/docs/containers?topic=containers-users#platform) per lo spazio dei nomi `default`.
* [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
  * **Nota**: nei cluster standard sono consentite tutte le configurazioni strongSwan. Se utilizzi un cluster gratuito, puoi scegliere solo una connessione VPN in uscita nel [Passo 3](#strongswan_3). Le connessioni VPN in entrata richiedono un programma di bilanciamento del carico nel cluster e i programmi di bilanciamento del carico non sono disponibili per i cluster gratuiti.

### Passo 1: Ottieni il grafico Helm strongSwan
{: #strongswan_1}

Installa Helm e ottieni il grafico Helm strongSwan per visualizzare le possibili configurazioni.
{: shortdesc}

1.  [Segui le istruzioni](/docs/containers?topic=containers-helm#public_helm_install) per installare il client Helm sulla tua macchina locale, installare il server Helm (tiller) con un account di servizio e aggiungere il repository Helm {{site.data.keyword.Bluemix_notm}}. Nota: è necessario Helm versione 2.8 o successive.

2.  Verifica che tiller sia installato con un account di servizio.

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    Output di esempio:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. Salva le impostazioni di configurazione predefinite per il grafico Helm strongSwan in un file YAML locale.

    ```
    helm inspect values iks-charts/strongswan > config.yaml
    ```
    {: pre}

4. Apri il file `config.yaml`.

### Passo 2: Configura le impostazioni IPSec di base
{: #strongswan_2}

Per controllare lo stabilimento della connessione VPN, modifica le seguenti impostazioni IPSec di base.
{: shortdesc}

Per ulteriori informazioni su ciascuna impostazione, leggi la documentazione fornita nel file `config.yaml` per il grafico Helm.
{: tip}

1. Se il tuo endpoint del tunnel VPN in loco non supporta `ikev2` come protocollo per inizializzare la connessione, modifica il valore di `ipsec.keyexchange` in `ikev1`.
2. Imposta `ipsec.esp` su un elenco degli algoritmi di autenticazione e crittografia ESP utilizzati dal tuo endpoint del tunnel VPN in loco per la connessione.
    * Se `ipsec.keyexchange` è impostata su `ikev1`, devi specificare questa impostazione.
    * Se `ipsec.keyexchange` è impostata su `ikev2`, questa impostazione è facoltativa.
    * Se lasci vuota questa impostazione, per la connessione vengono utilizzati gli algoritmi strongSwan `aes128-sha1,3des-sha1` predefiniti.
3. Imposta `ipsec.ike` su un elenco degli algoritmi di autenticazione e crittografia IKE/ISAKMP SA utilizzati dal tuo endpoint del tunnel VPN in loco per la connessione. Gli algoritmi devono essere specifici nel formato `encryption-integrity[-prf]-dhgroup`.
    * Se `ipsec.keyexchange` è impostata su `ikev1`, devi specificare questa impostazione.
    * Se `ipsec.keyexchange` è impostata su `ikev2`, questa impostazione è facoltativa.
    * Se lasci vuota questa impostazione, per la connessione vengono utilizzati gli algoritmi strongSwan `aes128-sha1-modp2048,3des-sha1-modp1536` predefiniti.
4. Modifica il valore di `local.id` in qualsiasi stringa che vuoi usare per identificare il lato cluster Kubernetes locale utilizzato dal tuo endpoint del tunnel VPN. Il valore predefinito è `ibm-cloud`. Alcune implementazioni VPN richiedono che tu utilizzi l'indirizzo IP pubblico per l'endpoint locale.
5. Modifica il valore di `remote.id` in qualsiasi stringa che vuoi usare per identificare il lato in loco remoto utilizzato dal tuo endpoint del tunnel VPN. Il valore predefinito è `on-prem`. Alcune implementazioni VPN richiedono che tu utilizzi l'indirizzo IP pubblico per l'endpoint remoto.
6. Modifica il valore di `preshared.secret` nel segreto precondiviso utilizzato dal tuo gateway dell'endpoint del tunnel VPN in loco per la connessione. Questo valore è memorizzato in `ipsec.secrets`.
7. Facoltativo: imposta `remote.privateIPtoPing` su qualsiasi indirizzo IP privato nella sottorete remota di cui eseguire il ping come parte del test di convalida della connettività Helm.

### Passo 3: Seleziona la connessione VPN in entrata o in uscita
{: #strongswan_3}

Quando configuri una connessione VPN strongSwan, scegli se la connessione VPN è in entrata al cluster o in uscita dal cluster.
{: shortdesc}

<dl>
<dt>In entrata</dt>
<dd>L'endpoint VPN in loco dalla rete remota avvia la connessione VPN e il cluster è in ascolto per la connessione.</dd>
<dt>In uscita</dt>
<dd>Il cluster avvia la connessione VPN e l'endpoint VPN in loco dalla rete remota è in ascolto per la connessione.</dd>
</dl>

Se utilizzi un cluster gratuito, puoi scegliere solo una connessione VPN in uscita. Le connessioni VPN in entrata richiedono un programma di bilanciamento del carico nel cluster e i programmi di bilanciamento del carico non sono disponibili per i cluster gratuiti.

Per stabilire una connessione VPN in entrata, modifica le seguenti impostazioni:
1. Verifica che `ipsec.auto` sia impostata su `add`.
2. Facoltativo: imposta `loadBalancerIP` su un indirizzo IP pubblico portatile per il servizio VPN strongSwan. La specifica di un indirizzo IP è utile quando hai bisogno di un indirizzo IP stabile, ad esempio quando devi designare quali indirizzi IP sono consentiti attraverso un firewall in loco. Il cluster deve avere almeno un indirizzo IP del programma di bilanciamento del carico pubblico. [Puoi verificare se visualizzare i tuoi indirizzi IP pubblici disponibili](/docs/containers?topic=containers-subnets#review_ip) o [liberare un indirizzo IP utilizzato](/docs/containers?topic=containers-subnets#free).
    * Se lasci vuota questa impostazione, viene utilizzato uno degli indirizzi IP pubblici portatili disponibili.
    * Devi anche configurare l'indirizzo IP pubblico che selezioni per l'endpoint VPN, o l'indirizzo IP pubblico ad esso assegnato, sull'endpoint VPN in loco.

Per stabilire una connessione VPN in uscita, modifica le seguenti impostazioni:
1. Modifica `ipsec.auto` in `start`.
2. Imposta `remote.gateway` sull'indirizzo IP pubblico per l'endpoint VPN in loco nella rete remota.
3. Scegli una delle seguenti opzioni per l'indirizzo IP per l'endpoint VPN del cluster:
    * **Indirizzo IP pubblico del gateway privato del cluster**: se i tuoi nodi di lavoro sono connessi solo a una VLAN privata, la richiesta VPN in uscita viene instradata tramite il gateway privato per raggiungere Internet. L'indirizzo IP pubblico del gateway privato viene utilizzato per la connessione VPN.
    * **Indirizzo IP pubblico del nodo di lavoro dove è in esecuzione il pod strongSwan**: se il nodo di lavoro dove è in esecuzione il pod strongSwan è connesso a una VLAN pubblica, l'indirizzo IP pubblico del nodo di lavoro viene utilizzato per la connessione VPN.
        <br>
        * Se il pod strongSwan viene eliminato e ripianificato su un nodo di lavoro differente nel cluster, l'indirizzo IP pubblico della VPN cambia. L'endpoint VPN in loco della rete remota deve consentire lo stabilimento della connessione VPN dall'indirizzo IP pubblico di uno qualsiasi dei nodi di lavoro del cluster.
        * Se l'endpoint VPN remoto non è in grado di gestire le connessioni VPN da più indirizzi IP pubblici, limita i nodi a cui viene distribuito il pod VPN strongSwan. Imposta `nodeSelector` sugli indirizzi IP degli specifici nodi di lavoro o un'etichetta di nodo di lavoro. Ad esempio, il valore `kubernetes.io/hostname: 10.232.xx.xx` consente la distribuzione del pod VPN solo su tale nodo di lavoro. Il valore `strongswan: vpn` limita il pod VPN all'esecuzione su qualsiasi nodo di lavoro con quell'etichetta. Puoi utilizzare qualsiasi etichetta di nodo di lavoro. Per consentire l'utilizzo di nodi di lavoro differenti con distribuzioni di grafico helm differenti, utilizza `strongswan: <release_name>`. Per l'alta disponibilità, seleziona almeno due nodi di lavoro.
    * **Indirizzo IP pubblico del servizio strongSwan**: per stabilire la connessione utilizzando l'indirizzo IP del servizio VPN strongSwan, imposta `connectUsingLoadBalancerIP` su `true`. L'indirizzo IP del servizio strongSwan è un indirizzo IP pubblico portatile che puoi specificare nell'impostazione `loadBalancerIP` oppure un indirizzo IP pubblico portatile disponibile che viene assegnato automaticamente al servizio.
        <br>
        * Se scegli di selezionare un indirizzo IP utilizzando l'impostazione `loadBalancerIP`, il cluster deve avere almeno un indirizzo IP del programma di bilanciamento del carico pubblico disponibile. [Puoi verificare se visualizzare i tuoi indirizzi IP pubblici disponibili](/docs/containers?topic=containers-subnets#review_ip) o [liberare un indirizzo IP utilizzato](/docs/containers?topic=containers-subnets#free).
        * Tutti i nodi di lavoro del cluster devono trovarsi sulla stessa VLAN pubblica. In caso contrario, devi utilizzare l'impostazione `nodeSelector` per assicurarti che il pod VPN venga distribuito in un nodo di lavoro sulla stessa VLAN pubblica di `loadBalancerIP`.
        * Se `connectUsingLoadBalancerIP` è impostata su `true` e `ipsec.keyexchange` è impostata su `ikev1`, devi impostare `enableServiceSourceIP` su `true`.

### Passo 4: Accesso alle risorse del cluster sulla connessione VPN
{: #strongswan_4}

Determina le risorse cluster che devono essere accessibili dalla rete remota sulla connessione VPN.
{: shortdesc}

1. Aggiungi i CIDR di una o più sottoreti del cluster all'impostazione `local.subnet`. Devi configurare i CIDR della sottorete locale sull'endpoint VPN in loco. Questo elenco può includere le seguenti sottoreti:  
    * CIDR della sottorete del pod Kubernetes: `172.30.0.0/16`. Le comunicazioni bidirezionali sono abilitate tra tutti i pod del cluster e uno qualsiasi degli host nelle sottoreti di rete remota che elenchi nell'impostazione `remote.subnet`. Se devi evitare che qualsiasi host `remote.subnet` acceda ai pod del cluster per motivi di sicurezza, non aggiungere la sottorete pod Kubernetes all'impostazione `local.subnet`.
    * CIDR della sottorete del servizio Kubernetes: `172.21.0.0/16`. Gli indirizzi IP del servizio forniscono un modo per esporre più pod dell'applicazione distribuiti su diversi nodi di lavoro dietro un singolo IP.
    * Se le tue applicazioni vengono esposte da un servizio NodePort sulla rete privata o un ALB Ingress privato, aggiungi il CIDR di sottorete privata del nodo di lavoro. Richiama i primi tre ottetti del tuo indirizzo IP privato del tuo nodo di lavoro eseguendo `ibmcloud ks worker <cluster_name>`. Ad esempio, se è `10.176.48.xx`, prendi nota di `10.176.48`. Ottieni quindi il CIDR della sottorete privata del nodo di lavoro eseguendo questo comando, sostituendo `<xxx.yyy.zz>` con l'ottetto che hai richiamato precedentemente: `ibmcloud sl subnet list | grep <xxx.yyy.zzz>`. **Nota**: se un nodo di lavoro viene aggiunto a una nuova sottorete privata, devi aggiungere il nuovo CIDR della sottorete privata sia all'impostazione `local.subnet` che all'endpoint VPN in loco. Bisogna quindi riavviare la connessione VPN.
    * Se hai delle applicazioni esposte dai servizi LoadBalancer sulla rete privata, aggiungi i CIDR della sottorete gestita dall'utente privata del cluster. Per trovare questi valori, esegui `ibmcloud ks cluster-get --cluster <cluster_name> --showResources`. Nella sezione **VLAN**, ricerca i CIDR che hanno un valore **Pubblico** di `false`. **Nota**: se `ipsec.keyexchange` è impostata su `ikev1`, puoi specificare solo una sottorete. Puoi tuttavia utilizzare l'impostazione `localSubnetNAT` per combinare più sottoreti del cluster in una singola sottorete.

2. Facoltativo: riassocia le sottoreti del cluster utilizzando l'impostazione `localSubnetNAT`. NAT (Network Address Translation) per le sottoreti fornisce una soluzione temporanea per i conflitti di sottorete tra la rete del cluster e la rete remota in loco. Puoi utilizzare NAT per riassociare le sottoreti IP locali private del cluster, la sottorete del pod (172.30.0.0/16) o la sottorete del servizio pod (172.21.0.0/16) a una sottorete privata diversa. Il tunnel VPN vede le sottoreti IP riassociate invece delle sottoreti originali. La riassociazione avviene prima che i pacchetti vengano inviati attraverso il tunnel VPN e dopo che i pacchetti arrivano dal tunnel VPN. Puoi esporre contemporaneamente entrambe le sottoreti riassociate e non riassociate sulla VPN. Per abilitare NAT, puoi aggiungere un'intera sottorete o singoli indirizzi IP.
    * Se aggiungi un'intera sottorete nel formato `10.171.42.0/24=10.10.10.0/24`, per la riassociazione si applica un rapporto di 1 a 1: tutti gli indirizzi IP nella sottorete della rete interna vengono associati alla sottorete della rete esterna e viceversa.
    * Se aggiungi singoli indirizzi IP nel formato `10.171.42.17/32=10.10.10.2/32,10.171.42.29/32=10.10.10.3/32`, solo quegli indirizzi IP interni vengono associati agli indirizzi IP esterni specificati.

3. Facoltativo per i grafici Helm strongSwan versione 2.2.0 e successive: nascondi tutti gli indirizzi IP del cluster dietro un singolo indirizzo IP impostando `enableSingleSourceIP` su `true`. Questa opzione fornisce una delle configurazioni più sicure per la connessione VPN perché non è consentita alcuna connessione dalla rete remota che vada nuovamente nel cluster.
    <br>
    * Questa impostazione richiede che tutto il flusso di dati sulla connessione VPN sia in uscita indipendentemente dal fatto che la connessione VPN sia stabilita dal cluster o dalla rete remota.
    * `local.subnet` deve essere impostato su una singola sottorete /32.

4. Facoltativo per i grafici Helm strongSwan versione 2.2.0 e successive: abilita il servizio strongSwan per instradare le richieste in entrata dalla rete remota al servizio che esiste esternamente al cluster utilizzando impostazione `localNonClusterSubnet`.
    <br>
    * Il servizio non cluster deve esistere sulla stessa rete privata o su una rete privata che sia raggiungibile dai nodi di lavoro.
    * Il nodo di lavoro non cluster non può avviare il traffico alla rete remota tramite la connessione VPN, ma il nodo non cluster può essere la destinazione di richieste in entrata dalla rete remota.
    * Devi elencare i CIDR delle sottoreti non cluster nell'impostazione `local.subnet`.

### Passo 5: Accesso alle risorse di rete remota sulla connessione VPN
{: #strongswan_5}

Determina quali risorse di rete remota devono essere accessibili dal cluster sulla connessione VPN.
{: shortdesc}

1. Aggiungi i CIDR di una o più sottoreti private in loco all'impostazione `remote.subnet`. **Nota**: se `ipsec.keyexchange` è impostata su `ikev1`, puoi specificare solo una sottorete.
2. Facoltativo per i grafici Helm strongSwan versione 2.2.0 e successive: riassocia le sottoreti di rete remota utilizzando l'impostazione `remoteSubnetNAT`. NAT (Network Address Translation) per le sottoreti fornisce una soluzione temporanea per i conflitti di sottorete tra la rete del cluster e la rete remota in loco. Puoi utilizzare NAT per riassociare le sottoreti IP della rete remota a una sottorete privata differente. La riassociazione avviene prima che i pacchetti vengano inviati attraverso il tunnel VPN. I pod nel cluster vedono le sottoreti IP riassociate invece delle sottoreti originali. Prima che i pod ritrasmettano i dati attraverso il tunnel VPN, la sottorete IP riassociata viene riportata alla sottorete effettiva utilizzata dalla rete remota. Puoi esporre contemporaneamente entrambe le sottoreti riassociate e non riassociate sulla VPN.

### Passo 6 (facoltativo): Abilita il monitoraggio con l'integrazione del webhook Slack
{: #strongswan_6}

Per monitorare lo stato della VPN strongSwan, puoi configurare un webhook per pubblicare automaticamente messaggi di connettività VPN in un canale Slack.
{: shortdesc}

1. Accedi al tuo spazio di lavoro Slack.

2. Vai alla [pagina dell'applicazione Incoming WebHooks ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://slack.com/apps/A0F7XDUAZ-incoming-webhooks).

3. Fai clic su **Request to Install**. Se questa applicazione non è elencata nella tua configurazione Slack, contatta il proprietario del tuo spazio di lavoro Slack.

4. Una volta approvata la tua richiesta di installazione, fai clic su **Add Configuration**.

5. Scegli un canale Slack o crea un nuovo canale in cui inviare i messaggi VPN.

6. Copia l'URL webhook che viene generato. Il formato dell'URL è simile al seguente:
  ```
  https://hooks.slack.com/services/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
  ```
  {: screen}

7. Per verificare che il webhook Slack sia installato, invia un messaggio di prova al tuo URL webhook eseguendo questo comando:
    ```
    curl -X POST -H 'Content-type: application/json' -d '{"text":"VPN test message"}' <webhook_URL>
    ```
    {: pre}

8. Vai al canale Slack che hai scelto per verificare che il messaggio di prova abbia esito positivo.

9. Nel file `config.yaml` relativo al grafico Helm, configura il webhook per monitorare la tua connessione VPN.
    1. Modifica `monitoring.enable` in `true`.
    2. Aggiungi gli indirizzi IP privati o gli endpoint HTTP nella sottorete remota che vuoi che siano sempre raggiungibili tramite la connessione VPN a `monitoring.privateIPs` o `monitoring.httpEndpoints`. Ad esempio, potresti aggiungere l'IP dall'impostazione `remote.privateIPtoPing` a `monitoring.privateIPs`.
    3. Aggiungi l'URL webhook a `monitoring.slackWebhook`.
    4. Modifica le altre impostazioni `monitoring` facoltative in base alle necessità.

### Passo 7: Distribuisci il grafico Helm
{: #strongswan_7}

Distribuisci il grafico Helm strongSwan nel tuo cluster con le configurazioni che hai scelto in precedenza.
{: shortdesc}

1. Se devi configurare delle impostazioni più avanzate, attieniti alla documentazione fornita per ciascuna impostazione nel grafico Helm.

3. Salva il file `config.yaml` aggiornato.

4. Installa il grafico Helm nel tuo cluster con il file `config.yaml` aggiornato.

    Se hai più distribuzioni VPN in un singolo cluster, puoi evitare conflitti di denominazione e differenziare le distribuzioni scegliendo nomi di release più descrittivi rispetto a `vpn`. Per evitare il troncamento del nome della release, limita il nome a 35 caratteri o meno.
    {: tip}

    ```
    helm install -f config.yaml --name=vpn iks-charts/strongswan
    ```
    {: pre}

5. Controlla lo stato di distribuzione del grafico. Quando il grafico è pronto, il campo **STATO** vicino alla parte superiore dell'output ha un valore di `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

6. Una volta che il grafico è stato distribuito, verifica che le impostazioni aggiornate nel file `config.yaml` siano state utilizzate.

    ```
    helm get values vpn
    ```
    {: pre}

## Test e verifica della connettività VPN strongSwan
{: #vpn_test}

Dopo aver distribuito il tuo grafico Helm, verifica la connettività VPN.
{:shortdesc}

1. Se la VPN nel gateway in loco non è attiva, avviala.

2. Imposta la variabile di ambiente `STRONGSWAN_POD`.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

3. Controlla lo stato della VPN. Uno stato di `ESTABLISHED` significa che la connessione VPN ha avuto esito positivo.

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    Output di esempio:

    ```
    Security Associations (1 up, 0 connecting):
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    * Quando tenti di stabilire la connettività VPN con il grafico Helm strongSwan per la prima volta, è probabile che lo stato della VPN non sia `ESTABLISHED`. Potresti dover verificare le impostazioni dell'endpoint VPN in loco e modificare il file di configurazione più volte prima che la connessione abbia esito positivo:
        1. Esegui `helm delete --purge <release_name>`
        2. Correggi i valori errati nel file di configurazione.
        3. Esegui `helm install -f config.yaml --name=<release_name> ibm/strongswan`
      Puoi anche eseguire ulteriori controlli nel passo successivo.

    * Se il pod VPN è in uno stato `ERROR` o continua ad arrestarsi e a riavviarsi, potrebbe essere dovuto alla convalida dei parametri delle impostazioni `ipsec.conf` nella mappa di configurazione del grafico.
        1. Controlla eventuali errori di convalida nei log del pod strongSwan eseguendo `kubectl logs $STRONGSWAN_POD`.
        2. Se sono presenti degli errori di convalida, esegui `helm delete --purge <release_name>`
        3. Correggi i valori errati nel file di configurazione.
        4. Esegui `helm install -f config.yaml --name=<release_name> ibm/strongswan`

4. Puoi testare ulteriormente la connettività VPN eseguendo i cinque test Helm inclusi nella definizione del grafico strongSwan.

    ```
    helm test vpn
    ```
    {: pre}

    * Se tutti i test hanno esito positivo, la tua connessione VPN strongSwan è configurata correttamente.
    * Se un test non riesce, vai al passo successivo.

5. Visualizza l'output di un test non riuscito nei log del pod di test.

    ```
    kubectl logs <test_program>
    ```
    {: pre}

    Alcuni dei test hanno requisiti che sono impostazioni facoltative nella configurazione VPN. Se alcuni test non riescono, gli errori potrebbero essere accettabili a seconda che queste impostazioni facoltative siano state specificate o meno. Fai riferimento alla seguente tabella per informazioni su ciascun test e sul perché potrebbe non riuscire.
    {: note}

    {: #vpn_tests_table}
    <table>
    <caption>Descrizione dei test di connettività VPN Helm</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei test di connettività VPN Helm</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td>Convalida la sintassi del file <code>ipsec.conf</code> che viene generato dal file <code>config.yaml</code>. Questo test potrebbe non riuscire a causa di valori non corretti nel file <code>config.yaml</code>.</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>Verifica che la connessione VPN abbia lo stato <code>ESTABLISHED</code>. Questo test potrebbe non riuscire per i seguenti motivi:<ul><li>Differenze tra i valori nel file <code>config.yaml</code> e le impostazioni dell'endpoint VPN in loco.</li><li>Se il cluster è in modalità "ascolto" (<code>ipsec.auto</code> è impostata su <code>add</code>), la connessione non viene stabilita sul lato in loco.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td>Esegue il ping dell'indirizzo IP pubblico <code>remote.gateway</code> che hai configurato nel file <code>config.yaml</code>. Se la connessione VPN ha lo stato <code>ESTABLISHED</code>, puoi ignorare il risultato di questo test. Se la connessione VPN non ha lo stato <code>ESTABLISHED</code>, questo test potrebbe non riuscire per i seguenti motivi:<ul><li>Non hai specificato un indirizzo IP del gateway VPN in loco. Se <code>ipsec.auto</code> è impostata su <code>start</code>, l'indirizzo IP <code>remote.gateway</code> è obbligatorio.</li><li>I pacchetti ICMP (ping) vengono bloccati da un firewall.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>Esegue il ping dell'indirizzo IP privato <code>remote.privateIPtoPing</code> del gateway VPN in loco dal pod VPN nel cluster. Questo test potrebbe non riuscire per i seguenti motivi:<ul><li>Non hai specificato un indirizzo IP <code>remote.privateIPtoPing</code>. Se non hai specificato un indirizzo IP intenzionalmente, questo errore è accettabile.</li><li>Non hai specificato il CIDR della sottorete pod del cluster, <code>172.30.0.0/16</code>, nell'elenco <code>local.subnet</code>.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>Esegue il ping dell'indirizzo IP privato <code>remote.privateIPtoPing</code> del gateway VPN in loco dal nodo di lavoro nel cluster. Questo test potrebbe non riuscire per i seguenti motivi:<ul><li>Non hai specificato un indirizzo IP <code>remote.privateIPtoPing</code>. Se non hai specificato un indirizzo IP intenzionalmente, questo errore è accettabile.</li><li>Non hai specificato il CIDR della sottorete privata del nodo di lavoro del cluster nell'elenco <code>local.subnet</code>.</li></ul></td>
    </tr>
    </tbody></table>

6. Elimina il grafico Helm corrente.

    ```
    helm delete --purge vpn
    ```
    {: pre}

7. Apri il file `config.yaml` e correggi i valori errati.

8. Salva il file `config.yaml` aggiornato.

9. Installa il grafico Helm nel tuo cluster con il file `config.yaml` aggiornato. Le proprietà aggiornate sono memorizzate in una mappa di configurazione per il tuo grafico.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

10. Controlla lo stato di distribuzione del grafico. Quando il grafico è pronto, il campo **STATO** vicino alla parte superiore dell'output ha un valore di `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

11. Una volta che il grafico è stato distribuito, verifica che le impostazioni aggiornate nel file `config.yaml` siano state utilizzate.

    ```
    helm get values vpn
    ```
    {: pre}

12. Elimina i pod di test correnti.

    ```
    kubectl get pods -a -l app=strongswan-test
    ```
    {: pre}

    ```
    kubectl delete pods -l app=strongswan-test
    ```
    {: pre}

13. Esegui di nuovo i test.

    ```
    helm test vpn
    ```
    {: pre}

<br />


## Limitazione del traffico VPN strongSwan per spazio dei nomi o nodo di lavoro
{: #limit}

Se hai un cluster a singolo tenant o hai un cluster a più tenant in cui le risorse del cluster sono condivise tra i tenant, puoi [limitare il traffico VPN per ogni distribuzione di strongSwan ai pod presenti in determinati spazi dei nomi](#limit_namespace). Se hai un cluster a più tenant in cui le risorse del cluster sono dedicate ai tenant, puoi [limitare il traffico VPN per ogni distribuzione strongSwan ai nodi di lavoro dedicati a ciascun tenant](#limit_worker).
{: shortdesc}

### Limitazione del traffico VPN strongSwan per spazio dei nomi
{: #limit_namespace}

Se hai un cluster a singolo tenant o un cluster a più tenant, puoi limitare il traffico VPN ai pod che si trovano solo in determinati spazi dei nomi.
{: shortdesc}

Ad esempio, supponiamo che desideri che i pod presenti solo in uno spazio dei nomi specifico, `my-secure-namespace`, inviino e ricevano i dati tramite la VPN. Non vuoi che i pod in altri spazi dei nomi, come ad esempio `kube-system`, `ibm-system` o `default`, accedano alla tua rete in loco. Per limitare il traffico VPN solo a `my-secure-namespace`, puoi creare politiche di rete globali di Calico.

Prima di utilizzare questa soluzione, esamina le considerazioni e le limitazioni riportate di seguito.
* Non devi distribuire il grafico Helm strongSwan nello spazio dei nomi specificato. Il pod VPN strongSwan e la serie di daemon di instradamento essere distribuiti in `kube-system` o in qualsiasi altro spazio dei nomi. Se la VPN strongSwan non viene distribuita nello spazio dei nomi specificato, il test Helm `vpn-strongswan-ping-remote-ip-1` avrà esito negativo. Questo errore è previsto ed accettabile. Il test esegue il ping dell'indirizzo IP privato `remote.privateIPtoPing` del gateway VPN in loco da un pod che non si trova nello spazio dei nomi che ha l'accesso diretto alla sottorete remota. Tuttavia, il pod VPN è ancora in grado di inoltrare il traffico ai pod negli spazi dei nomi che hanno instradamenti alla sottorete remota e il traffico può ancora scorrere correttamente. Lo stato della VPN è ancora `ESTABLISHED` e i pod nello spazio dei nomi specificato possono connettersi tramite la VPN.

* Le politiche di rete globali di Calico nei seguenti passi non impediscono ai pod Kubernetes che utilizzano la rete host di inviare e ricevere dati tramite la VPN. Quando un pod è configurato con la rete host, l'applicazione in esecuzione nel pod può restare in ascolto sulle interfacce di rete del nodo di lavoro su cui si trova. Questi pod di rete host possono esistere in qualsiasi spazio dei nomi. Per determinare quali pod dispongono della rete host, esegui `kubectl get pods --all-namespaces -o wide` e cerca tutti i pod che non hanno un indirizzo IP di pod `172.30.0.0/16`. Se vuoi impedire ai pod di rete host di inviare e ricevere dati tramite la VPN, puoi impostare le seguenti opzioni nel tuo file di distribuzione `values.yaml`: `local.subnet: 172.30.0.0/16` e `enablePodSNAT: false`. Queste impostazioni di configurazione espongono tutti i pod Kubernetes sulla connessione VPN alla rete remota. Tuttavia, solo i pod che si trovano nello spazio dei nomi sicuro specificato sono raggiungibili tramite la VPN.

Prima di iniziare:
* [Distribuisci il grafico Helm strongSwan](#vpn_configure) e [assicurati che la connettività VPN funzioni correttamente](#vpn_test).
* [Installa e configura la CLI Calico](/docs/containers?topic=containers-network_policies#cli_install).

Per limitare il traffico VPN a un determinato spazio dei nomi:

1. Crea una politica di rete globale Calico denominata `allow-non-vpn-outbound.yaml`. Questa politica consente a tutti gli spazi dei nomi di continuare a inviare il traffico in uscita a tutte le destinazioni, ad eccezione della sottorete remota a cui accede la VPN strongSwan. Sostituisci `<remote.subnet>` con la `remote.subnet` che hai specificato nel file di configurazione `values.yaml` di Helm. Per specificare più sottoreti remote, vedi la [documentazione di Calico ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/globalnetworkpolicy).
    ```yaml
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: allow-non-vpn-outbound
    spec:
      selector: has(projectcalico.org/namespace)
      egress:
      - action: Allow
        destination:
          notNets:
          - <remote.subnet>
      order: 900
      types:
      - Egress
    ```
    {: codeblock}

2. Applica la politica.

    ```
    calicoctl apply -f allow-non-vpn-outbound.yaml --config=filepath/calicoctl.cfg
    ```
    {: pre}

3. Crea un'altra politica di rete globale Calico denominata `allow-vpn-from-namespace.yaml`. Questa politica consente solo a uno spazio dei nomi specificato di inviare il traffico in uscita alla sottorete remota a cui accede la VPN strongSwan. Sostituisci `<namespace>` con lo spazio dei nomi che può accedere alla VPN e `<remote.subnet>` con la `remote.subnet` che hai specificato nel file di configurazione `values.yaml` di Helm. Per specificare più spazi dei nomi o sottoreti remote, vedi la [documentazione di Calico ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/globalnetworkpolicy).
    ```yaml
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: allow-vpn-from-namespace
    spec:
      selector: projectcalico.org/namespace == "<namespace>"
      egress:
      - action: Allow
        destination:
          nets:
          - <remote.subnet>
      order: 900
      types:
      - Egress
    ```
    {: codeblock}

4. Applica la politica.

    ```
    calicoctl apply -f allow-vpn-from-namespace.yaml --config=filepath/calicoctl.cfg
    ```
    {: pre}

5. Verifica che le politiche di rete globali siano state create nel tuo cluster.
    ```
    calicoctl get GlobalNetworkPolicy -o wide --config=filepath/calicoctl.cfg
    ```
    {: pre}

### Limitazione del traffico VPN strongSwan per nodo di lavoro
{: #limit_worker}

Se hai più distribuzioni VPN strongSwan in un cluster a più tenant, puoi limitare il traffico VPN per ogni distribuzione a specifici nodi di lavoro dedicati a ciascun tenant.
{: shortdesc}

Quando distribuisci un grafico Helm strongSwan, viene creata una distribuzione VPN strongSwan. I pod VPN strongSwan vengono distribuiti su qualsiasi nodo di lavoro non contaminato. Inoltre, viene creata una serie di daemon Kubernetes. Questa serie di daemon configura automaticamente gli instradamenti su tutti i nodi di lavoro non contaminati nel cluster a ognuna delle sottoreti remote. Il pod VPN strongSwan utilizza gli instradamenti sui nodi di lavoro per inoltrare le richieste alla sottorete remota nella rete in loco.

Gli instradamenti non vengono configurati sui nodi contaminati a meno che non specifichi la contaminazione nell'impostazione `tolerations` nel file `value.yaml`. Contaminando i nodi di lavoro, puoi impedire che gli instradamenti VPN vengano configurati su questi nodi di lavoro. Quindi, puoi specificare la contaminazione nell'impostazione `tolerations` solo per la distribuzione VPN che vuoi consentire sul nodo di lavoro contaminato. In questo modo, i pod VPN strongSwan per la distribuzione del grafico Helm di un tenant utilizzano solo gli instradamenti sui nodi di lavoro di quel tenant per instradare il traffico attraverso la connessione VPN alla sottorete remota.

Prima di utilizzare questa soluzione, esamina le considerazioni e le limitazioni riportate di seguito.
* Per impostazione predefinita, Kubernetes posiziona i pod dell'applicazione su qualsiasi nodo di lavoro non contaminato disponibile. Per assicurarti che questa soluzione funzioni correttamente, ciascun tenant deve prima garantire di distribuire i propri pod dell'applicazione solo ai nodi di lavoro contaminati per il tenant corretto. Inoltre, ciascun nodo di lavoro contaminato deve avere anche una tolleranza per consentire ai pod dell'applicazione di essere posizionati sul nodo. Per ulteriori informazioni sulle contaminazioni e tolleranze, vedi la [documentazione di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/).
* Le risorse del cluster potrebbero non essere utilizzate in modo ottimale perché nessuno dei tenant può posizionare i pod dell'applicazione sui nodi non contaminati condivisi.

I seguenti passi per limitare il traffico VPN strongSwan per nodo di lavoro utilizzano questo scenario di esempio: supponiamo che tu abbia un cluster {{site.data.keyword.containerlong_notm}} a più tenant con sei nodi di lavoro. Il cluster supporta il tenant A e il tenant B. Contamina i nodi di lavoro nei seguenti modi:
* Due nodi di lavoro vengono contaminati in modo che solo i pod del tenant A siano pianificati sui nodi di lavoro.
* Due nodi di lavoro vengono contaminati in modo che solo i pod del tenant B siano pianificati sui nodi di lavoro.
* Due nodi di lavoro non vengono contaminati perché sono richiesti almeno 2 nodi di lavoro per i pod VPN strongSwan e l'IP del programma di bilanciamento del carico sui cui eseguirli.

Per limitare il traffico VPN ai nodi contaminati per ciascun tenant:

1. Per limitare il traffico VPN solo ai nodi di lavoro dedicati al tenant A in questo esempio, specifichi la seguente `toleration` nel file `values.yaml` per il grafico Helm strongSwan del tenant A:
    ```
    tolerations:
     - key: dedicated
       operator: "Equal"
       value: "tenantA"
       effect: "NoSchedule"
    ```
    {: codeblock}
    Questa tolleranza consente alla serie di daemon di instradamento di essere eseguita sui due nodi di lavoro che hanno la contaminazione `dedicated="tenantA"` e sui due nodi di lavoro non contaminati. I pod VPN strongSwan per questa distribuzione vengono eseguiti sui due nodi di lavoro non contaminati.

2. Per limitare il traffico VPN solo ai nodi di lavoro dedicati al tenant B in questo esempio, specifichi la seguente `toleration` nel file `values.yaml` per il grafico Helm strongSwan del tenant B:
    ```
    tolerations:
     - key: dedicated
       operator: "Equal"
       value: "tenantB"
       effect: "NoSchedule"
    ```
    {: codeblock}
    Questa tolleranza consente alla serie di daemon di instradamento di essere eseguita sui due nodi di lavoro che hanno la contaminazione `dedicated="tenantB"` e sui due nodi di lavoro non contaminati. I pod VPN strongSwan per questa distribuzione vengono eseguiti anche sui due nodi di lavoro non contaminati.

<br />


## Aggiornamento del grafico Helm strongSwan
{: #vpn_upgrade}

Assicurati che il tuo grafico Helm strongSwan sia aggiornato.
{:shortdesc}

Per aggiornare il tuo grafico Helm strongSwan all'ultima versione:

  ```
  helm upgrade -f config.yaml <release_name> ibm/strongswan
  ```
  {: pre}

## Disabilitazione del servizio VPN IPSec strongSwan
{: vpn_disable}

Puoi disabilitare la connessione VPN eliminando il grafico Helm.
{:shortdesc}

  ```
  helm delete --purge <release_name>
  ```
  {: pre}

<br />


## Utilizzo di una VRA (Virtual Router Appliance)
{: #vyatta}

La VRA ([)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) fornisce il sistema operativo Vyatta 5600 più recente per i server bare metal x86. Puoi utilizzare una VRA come un gateway VPN per connetterti in modo sicuro ad una rete in loco.
{:shortdesc}

Tutto il traffico di rete pubblico e privato che entra o esce dalle VLAN del cluster viene instradato tramite una VRA. Puoi utilizzare la VRA come un endpoint VPN per creare un tunnel IPSec crittografato tra i server nell'infrastruttura IBM Cloud (SoftLayer) e le risorse in loco. Ad esempio, il seguente diagramma mostra in che modo un'applicazione su un nodo di lavoro solamente privato in {{site.data.keyword.containerlong_notm}} può comunicare con un server in loco tramite una connessione VPN VRA:

<img src="images/cs_vpn_vyatta.png" width="725" alt="Esponi un'applicazione in {{site.data.keyword.containerlong_notm}} utilizzando un programma di bilanciamento del carico" style="width:725px; border-style: none"/>

1. Un'applicazione nel tuo cluster, `myapp2`, riceve una richiesta da un servizio Ingress o LoadBalancer e deve connettersi in modo sicuro alla tua rete in loco.

2. Poiché `myapp2` si trova su un nodo di lavoro che si trova solo su una VLAN privata, la VRA funge da connessione protetta tra i nodi di lavoro e la rete in loco. La VRA usa l'indirizzo IP di destinazione per determinare quali pacchetti di rete inviare alla rete in loco.

3. La richiesta viene crittografata e inviata sul tunnel VPN nel data center in loco.

4. La richiesta in entrata passa attraverso il firewall in loco e viene consegnata all'endpoint del tunnel VPN (router) in cui viene decrittografata.

5. L'endpoint del tunnel VPN (router) inoltra la richiesta al mainframe o al server in loco a seconda dell'indirizzo IP di destinazione specificato nel passo 2. I dati necessari vengono reinviati tramite la connessione VPN a `myapp2` seguendo lo stesso processo.

Per configurare una VRA (Virtual Router Appliance):

1. [Ordina una VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started).

2. [Configura la VLAN privata nella VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans).

3. Per abilitare una connessione VPN utilizzando la VRA, [configura VRRP nella VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#high-availability-vpn-with-vrrp).

Se hai un'applicazione router esistente e aggiungi quindi un cluster, le nuove sottoreti portatili ordinate per il cluster non vengono configurate sull'applicazione router. Per utilizzare i servizi di rete, devi abilitare l'instradamento tra le sottoreti sulla stessa VLAN [abilitando lo spanning della VLAN](/docs/containers?topic=containers-subnets#subnet-routing). Per controllare se lo spanning della VLAN è già abilitato, usa il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.
{: important}
