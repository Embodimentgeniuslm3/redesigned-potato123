---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-15"

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



# Argomenti popolari di {{site.data.keyword.containerlong_notm}}
{: #cs_popular_topics}

Resta al passo con cosa succede in {{site.data.keyword.containerlong}}. Ulteriori informazioni sulle nuove funzioni da esplorare, su un suggerimento per la sperimentazione o su alcuni argomenti popolari che gli altri sviluppatori stanno trovando utili al momento.
{:shortdesc}



## Argomenti popolari nel mese di aprile 2019
{: #apr19}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel mese di aprile 2019</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>15 aprile 2019</td>
<td>[Registrazione di un nome host NLB (network load balancer)](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname)</td>
<td>Dopo che hai configurato gli NLB (network load balancer) pubblici per esporre le tue applicazioni a Internet, puoi procedere alla creazione delle voci DNS per gli IP dell'NLB creando i nomi host. {{site.data.keyword.Bluemix_notm}} si occupa di generare e gestire il certificato SSL jolly per il nome host per tuo conto. Puoi anche configurare i monitoraggi TCP/HTTP(S) per controllare lo stato di integrità degli indirizzi IP dell'NLB dietro ciascun nome host.</td>
</tr>
<tr>
<td>8 aprile 2019</td>
<td>[Terminale Kubernetes nel tuo browser web (beta)](/docs/containers?topic=containers-cs_cli_install#cli_web)</td>
<td>Se utilizzi il dashboard cluster nella console {{site.data.keyword.Bluemix_notm}} per gestire i tuoi cluster ma desideri apportare velocemente modifiche della configurazione più avanzate, puoi ora eseguire i comandi CLI direttamente dal tuo browser web nel terminale Kubernetes. Nella pagina dei dettagli per un cluster, avvia il terminale Kubernetes facendo clic sul pulsante **Terminale**. Nota che il terminale Kubernetes è rilasciato come un componente aggiuntivo beta e non è destinato a essere utilizzato nei cluster di produzione.</td>
</tr>
<tr>
<td>4 aprile 2019</td>
<td>[Master altamente disponibili ora a Sydney](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>Quando [crei un cluster](/docs/containers?topic=containers-clusters#clusters_ui) in una località metropolitana multizona, di cui ora fa parte anche Sydney, le repliche master Kubernetes vengono distribuite tra le zone per un'alta disponibilità.</td>
</tr>
</tbody></table>

## Argomenti popolari nel mese di marzo 2019
{: #mar19}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel mese di marzo 2019</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>21 marzo 2019</td>
<td>Presentazione degli endpoint del servizio privati per il tuo master cluster Kubernetes</td>
<td>Per impostazione predefinita, {{site.data.keyword.containerlong_notm}} configura il tuo cluster con l'accesso a una VLAN pubblica e a una VLAN privata. In precedenza, se volevi un [cluster di sola VLAN privata](/docs/containers?topic=containers-plan_clusters#private_clusters), dovevi configurare un'applicazione gateway per connettere i nodi di lavoro del cluster al master. Ora puoi utilizzare l'endpoint del servizio privato. Con l'endpoint del servizio privato abilitato, tutto il traffico tra i nodi di lavoro e il master si verifica sulla rete privata, senza che occorra un dispositivo di applicazione gateway. Oltre a questa sicurezza aumentata, il traffico in entrata e in uscita sulla rete privata è [illimitato e non addebitato ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/bandwidth). Puoi continuare a mantenere l'endpoint del servizio pubblico per proteggere l'accesso al tuo master Kubernetes su internet, ad esempio per eseguire i comandi `kubectl` senza essere sulla rete privata.<br><br>
Per utilizzare gli endpoint del servizio privato, devi abilitare la [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) e gli [endpoint del servizio](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started) per il tuo account dell'infrastruttura IBM Cloud (SoftLayer) . Il tuo cluster deve eseguire Kubernetes versione 1.11 o superiore. Se il tuo cluster esegue una versione Kubernetes meno recente, [esegui l'aggiornamento almeno alla 1.11](/docs/containers?topic=containers-update#update). Per ulteriori informazioni, consulta i seguenti link:<ul>
<li>[Descrizione delle comunicazioni tra master e nodi di lavoro con gli endpoint del servizio](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master)</li>
<li>[Configurazione dell'endpoint del servizio privato](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)</li>
<li>[Passaggio dagli endpoint del servizio pubblici a quelli privati](/docs/containers?topic=containers-cs_network_cluster#migrate-to-private-se)</li>
<li>Se hai un firewall sulla rete privata, [aggiunta di indirizzi IP privati per {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.registrylong_notm}} e altri servizi {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-firewall#firewall_outbound)</li>
</ul>
<p class="important">Se passi a un cluster di endpoint del servizio solo privato, assicurati che il tuo cluster possa ancora comunicare con gli altri servizi {{site.data.keyword.Bluemix_notm}} che utilizzi. [Archiviazione SDS (software-defined storage) Portworx](/docs/containers?topic=containers-portworx#portworx) e [cluster autoscaler](/docs/containers?topic=containers-ca#ca) non supportano l'endpoint del servizio solo privato. Utilizza invece un cluster con endpoint del servizio sia pubblici che privati. L'[archiviazione file basata su NFS](/docs/containers?topic=containers-file_storage#file_storage) è supportata se il tuo cluster esegue Kubernetes versione 1.13.4_1513, 1.12.6_1544, 1.11.8_1550, 1.10.13_1551 o successive.</p>
</td>
</tr>
<tr>
<td>07 marzo 2019</td>
<td>[Il cluster autoscaler passa da beta a GA](/docs/containers?topic=containers-ca#ca)</td>
<td>Il cluster autoscaler è ora generalmente disponibile (GA, generally available). Installa il plugin Helm e inizia a ridimensionare i pool di nodi di lavoro nel tuo cluster automaticamente per aumentare o ridurre il numero di nodi di lavoro in base alle esigenze di dimensionamento dei tuoi carichi di lavoro pianificati.<br><br>
Hai bisogno di aiuto o hai un feedback sul cluster autoscaler? Se sei un utente esterno, [esegui la registrazione per Slack pubblico ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://bxcs-slack-invite.mybluemix.net/) e pubblica nel canale [#cluster-autoscaler ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-container-service.slack.com/messages/CF6APMLBB). Se sei un dipendente IBM, pubblica nel canale [Slack interno![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-argonauts.slack.com/messages/C90D3KZUL).</td>
</tr>
</tbody></table>




## Argomenti popolari nel febbraio 2019
{: #feb19}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel febbraio 2019</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>25 febbraio</td>
<td>Controllo più granulare sul pull di immagini da {{site.data.keyword.registrylong_notm}}</td>
<td>Quando distribuisci i tuoi carichi di lavoro nei cluster {{site.data.keyword.containerlong_notm}}, i tuoi contenitori possono ora eseguire il pull di immagini dai [nuovi nomi dominio `icr.io` per {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-registry_overview#registry_regions). Inoltre, puoi utilizzare politiche di accesso dettagliate in {{site.data.keyword.Bluemix_notm}} IAM per controllare l'accesso alle tue immagini. Per ulteriori informazioni, vedi [Informazioni su come il tuo cluster è autorizzato a estrarre immagini](/docs/containers?topic=containers-images#cluster_registry_auth).</td>
</tr>
<tr>
<td>21 febbraio</td>
<td>[Zona disponibile in Messico](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)</td>
<td>Il Messico (`mex01`) è una nuova zona per i cluster nella regione Stati Uniti Sud. Se hai un firewall, assicurati di [aprire le porte del firewall](/docs/containers?topic=containers-firewall#firewall_outbound) per questa e le altre zone all'interno della regione in cui si trova il tuo cluster.</td>
</tr>
<tr><td>06 febbraio 2019</td>
<td>[Istio su {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-istio)</td>
<td>Istio su {{site.data.keyword.containerlong_notm}} fornisce un'installazione senza soluzione di continuità di Istio, aggiornamenti e gestione del ciclo di vita automatici dei componenti del piano di controllo Istio e un'integrazione con gli strumenti di registrazione e monitoraggio della piattaforma. Con un solo clic, puoi attivare tutti i componenti core di Istio, ulteriori funzioni di traccia, monitoraggio e visualizzazione e l'applicazione di esempio BookInfo. Istio su {{site.data.keyword.containerlong_notm}} viene offerto come componente aggiuntivo gestito, pertanto {{site.data.keyword.Bluemix_notm}} mantiene automaticamente aggiornati tutti i tuoi componenti Istio.</td>
</tr>
<tr>
<td>06 febbraio 2019</td>
<td>[Knative su {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-knative_tutorial)</td>
<td>Knative è una piattaforma open source che estende le funzionalità di Kubernetes per aiutarti a creare applicazioni senza server e inserite in contenitori moderne e basate sul sorgente sul tuo cluster Kubernetes. Managed Knative on {{site.data.keyword.containerlong_notm}} è un componente aggiuntivo gestito che integra Knative e Istio direttamente con il tuo cluster Kubernetes. Le versioni di Knative e Istio nel componente aggiuntivo sono testate da IBM e supportate per l'utilizzo in {{site.data.keyword.containerlong_notm}}.</td>
</tr>
</tbody></table>


## Argomenti popolari nel mese di gennaio 2019
{: #jan19}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel gennaio 2019</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>30 gennaio</td>
<td>Spazi dei nomi Kubernetes e ruoli di accesso del servizio {{site.data.keyword.Bluemix_notm}} IAM</td>
<td>{{site.data.keyword.containerlong_notm}} ora supporta i [ruoli di accesso del servizio](/docs/containers?topic=containers-access_reference#service) {{site.data.keyword.Bluemix_notm}} IAM. Questi ruoli di accesso del servizio si allineano a [RBAC Kubernetes](/docs/containers?topic=containers-users#role-binding) per autorizzare gli utenti a eseguire azioni `kubectl` nel cluster per gestire risorse Kubernetes quali i pod o le distribuzioni. Puoi inoltre [limitare l'accesso utente a uno specifico spazio dei nomi Kubernetes](/docs/containers?topic=containers-users#platform) all'interno del cluster utilizzando i ruoli di accesso del servizio {{site.data.keyword.Bluemix_notm}} IAM. I [ruoli di accesso della piattaforma](/docs/containers?topic=containers-access_reference#iam_platform) sono ora utilizzati per autorizzare gli utenti ad eseguire azioni `ibmcloud ks` per gestire la tua infrastruttura del cluster, come ad esempio i nodi di lavoro.<br><br>I ruoli di accesso del servizio {{site.data.keyword.Bluemix_notm}} IAM vengono aggiunti automaticamente ai cluster e agli account {{site.data.keyword.containerlong_notm}} esistenti basati sulle autorizzazioni che avevano in precedenza i tuoi utenti con i ruoli di accesso della piattaforma IAM. Andando avanti, puoi utilizzare IAM per creare gruppi di accesso, aggiungere utenti ai gruppi di accesso e assegnare ai gruppi i ruoli di accesso del servizio o della piattaforma. Per ulteriori informazioni, consulta il blog [Introducing service roles and namespaces in IAM for more granular control of cluster access ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/).</td>
</tr>
<tr>
<td>8 gennaio</td>
<td>[Anteprima del cluster autoscaler beta](/docs/containers?topic=containers-ca#ca)</td>
<td>Ridimensiona automaticamente i pool di nodi di lavoro nel tuo cluster per aumentare o ridurre il numero di nodi di lavoro nel pool di nodi di lavoro in base alle esigenze di dimensionamento dei tuoi carichi di lavoro pianificati. Per testare questo beta, devi richiedere l'accesso alla whitelist.</td>
</tr>
<tr>
<td>8 gennaio</td>
<td>[{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)</td>
<td>A volte trovi difficile ottenere tutti i file YAML e le altre informazioni di cui hai bisogno per un ausilio nella risoluzione di un problema? Prova {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool per una GUI (graphical user interface) che ti aiuta a raccogliere informazioni su cluster, distribuzione e rete.</td>
</tr>
</tbody></table>

## Argomenti popolari nel dicembre 2018
{: #dec18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel dicembre 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>11 dicembre</td>
<td>Supporto SDS con Portworx</td>
<td>Gestisci l'archiviazione persistente per i tuoi database inseriti in contenitori e altre applicazioni con stato oppure condividi i dati tra i pod su più zone con Portworx. [Portworx ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://portworx.com/products/introduction/) è una soluzione SDS (software-defined storage) altamente disponibile che gestisce l'archiviazione blocchi locale dei tuoi nodi di lavoro per creare un livello di archiviazione persistente unificato per le tue applicazioni. Utilizzando la replica di volume di ciascun volume a livello di contenitore su più nodi di lavoro, Portworx garantisce la persistenza dei dati e l'accessibilità dei dati nelle zone. Per ulteriori informazioni, vedi [Archiviazione di dati su SDS (software-defined storage) con Portworx](/docs/containers?topic=containers-portworx#portworx).    </td>
</tr>
<tr>
<td>6 dicembre</td>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>Ottieni visibilità operativa sulle prestazioni e sull'integrità delle tue applicazioni distribuendo Sysdig come servizio di terze parti ai tuoi nodi di lavoro per inoltrare le metriche a {{site.data.keyword.monitoringlong}}. Per ulteriori informazioni, vedi [Analizza le metriche per un'applicazione distribuita in un cluster Kubernetes](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). **Nota**: se utilizzi {{site.data.keyword.mon_full_notm}} con i cluster che eseguono Kubernetes versione 1.11 o successive, non vengono raccolte tutte le metriche del contenitore perché Sysdig attualmente non supporta `containerd`.</td>
</tr>
<tr>
<td>4 dicembre</td>
<td>[Riserve di risorse del nodo di lavoro](/docs/containers?topic=containers-plan_clusters#resource_limit_node)</td>
<td>Stai distribuendo così tante applicazioni che ti preoccupi di superare la capacità del tuo cluster? Le riserve di risorse di nodo di lavoro e le rimozioni di Kubernetes proteggono la capacità di calcolo del tuo cluster in modo che il tuo cluster disponga di risorse sufficienti per continuare a funzionare. Ti basta aggiungere qualche altro nodo di lavoro e i tuoi pod iniziano automaticamente a essere ripianificati su di essi. Le risorse di risorse di nodo di lavoro sono aggiornate nelle [modifiche delle patch di versione](/docs/containers?topic=containers-changelog#changelog) più recenti.</td>
</tr>
</tbody></table>

## Argomenti popolari nel novembre 2018
{: #nov18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel novembre 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>29 novembre</td>
<td>[Zona disponibile a Chennai](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Benvenuta Chennai, India come nuova zona per i cluster nella regione Asia Pacifico Nord. Se hai un firewall, assicurati di [aprire le porte del firewall](/docs/containers?topic=containers-firewall#firewall) per questa e le altre zone all'interno della regione in cui si trova il tuo cluster.</td>
</tr>
<tr>
<td>27 novembre</td>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>Aggiungi funzionalità di gestione dei log al tuo cluster distribuendo LogDNA come servizio di terze parti ai tuoi nodi di lavoro per gestire i log dai contenitori di pod. Per ulteriori informazioni, vedi [Gestione dei log di cluster Kubernetes con {{site.data.keyword.loganalysisfull_notm}} with LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>7 novembre</td>
<td>NLB (network load balancer) 2.0 (beta)</td>
<td>Puoi ora scegliere tra [NLB 1.0 o 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) per esporre in modo protetto le tue applicazioni cluster al pubblico.</td>
</tr>
<tr>
<td>7 novembre</td>
<td>Kubernetes versione 1.12 disponibile</td>
<td>Adesso, puoi aggiornare o creare i cluster che eseguono [Kubernetes versione 1.12](/docs/containers?topic=containers-cs_versions#cs_v112). I cluster della versione 1.12 vengono forniti con master Kubernetes altamente disponibili per impostazione predefinita.</td>
</tr>
<tr>
<td>7 novembre</td>
<td>Master altamente disponibili</td>
<td>Sono disponibili master altamente disponibili per i cluster che eseguono Kubernetes versione 1.10. Tutti i vantaggi descritti nella voce precedente per i cluster della versione 1.11 si applicano ai cluster della versione 1.10, così come i [passi di preparazione](/docs/containers?topic=containers-cs_versions#110_ha-masters) che devi eseguire.</td>
</tr>
<tr>
<td>1 novembre</td>
<td>Master altamente disponibili nei cluster che eseguono Kubernetes versione 1.11</td>
<td>In una singola zona, il tuo master è altamente disponibile e include repliche su host fisici separati per il server API, etcd, il programma di pianificazione e il gestore controller Kubernetes per proteggerti in caso di interruzione, come durante un aggiornamento del cluster. Se il tuo cluster si trova in una zona che supporta il multizona, il tuo master altamente disponibile viene esteso anche tra le zone per fornire protezione in caso di un malfunzionamento della zona.<br>Per le azioni che devi eseguire, vedi [Aggiornamento a master cluster altamente disponibili](/docs/containers?topic=containers-cs_versions#ha-masters). Queste azioni di preparazione si applicano:<ul>
<li>Se hai un firewall o politiche di rete Calico personalizzate.</li>
<li>Se utilizzi le porte host `2040` o `2041` sui tuoi nodi di lavoro.</li>
<li>Se hai utilizzato l'indirizzo IP del master cluster per l'accesso in cluster al master.</li>
<li>Se disponi dell'automazione che richiama l'API o la CLI Calico (`calicoctl`), ad esempio per creare le politiche Calico.</li>
<li>Se utilizzi le politiche di rete di Kubernetes o Calico per controllare l'accesso in uscita dei pod al master.</li></ul></td>
</tr>
</tbody></table>

## Argomenti popolari nell'ottobre 2018
{: #oct18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nell'ottobre 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>25 ottobre</td>
<td>[Zona disponibile a Milano](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Benvenuta Milano, Italia come nuova zona per i cluster a pagamento nella regione Europa Centrale. Precedentemente, Milano era disponibile solo per i cluster gratuiti. Se hai un firewall, assicurati di [aprire le porte del firewall](/docs/containers?topic=containers-firewall#firewall) per questa e le altre zone all'interno della regione in cui si trova il tuo cluster.</td>
</tr>
<tr>
<td>22 ottobre</td>
<td>[Nuova ubicazione multizona di Londra, `lon05`](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>L'ubicazione metropolitana multizona di Londra sostituisce la zona `lon02` con la nuova zona `lon05`, in cui sono disponibili più risorse di infrastruttura rispetto a `lon02`. Crea nuovi cluster multizona con `lon05`. I cluster esistenti con `lon02` sono supportati, ma i nuovi cluster multizona devono utilizzare invece `lon05`.</td>
</tr>
<tr>
<td>05 ottobre</td>
<td>Integrazione con {{site.data.keyword.keymanagementservicefull}} (beta)</td>
<td>Puoi crittografare i segreti Kubernetes presenti nel tuo cluster [abilitando {{site.data.keyword.keymanagementserviceshort}} (beta)](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
<tr>
<td>04 ottobre</td>
<td>[{{site.data.keyword.registrylong}} è ora integrato con {{site.data.keyword.Bluemix_notm}} IAM (Identity and Access Management)](/docs/services/Registry?topic=registry-iam#iam)</td>
<td>Puoi utilizzare {{site.data.keyword.Bluemix_notm}} IAM per controllare l'accesso alle tue risorse del registro, come il pull, il push e la creazione di immagini. Quando crei un cluster, crei anche un token del registro in modo che il cluster possa lavorare con il tuo registro. Pertanto, hai bisogno del ruolo di gestione della piattaforma **Amministratore** del registro a livello di account per creare un cluster. Per abilitare {{site.data.keyword.Bluemix_notm}} IAM per il tuo account del registro, vedi [Abilitazione dell'applicazione delle politiche per gli utenti esistenti](/docs/services/Registry?topic=registry-user#existing_users).</td>
</tr>
<tr>
<td>01 ottobre</td>
<td>[Gruppi di risorse](/docs/containers?topic=containers-users#resource_groups)</td>
<td>Puoi utilizzare i gruppi di risorse per separare le tue risorse {{site.data.keyword.Bluemix_notm}} in pipeline, dipartimenti o altri raggruppamenti per aiutare ad assegnare l'accesso e misurare l'utilizzo. Adesso, {{site.data.keyword.containerlong_notm}} supporta la creazione di cluster nel gruppo `predefinito` o in un qualsiasi altro gruppo di risorse che crei.</td>
</tr>
</tbody></table>

## Argomenti popolari a settembre 2018
{: #sept18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel settembre 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>25 settembre</td>
<td>[Nuove zone disponibili](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Ora hai ancora più opzioni su dove distribuire le tue applicazioni!
<ul><li>Benvenuta San Jose come due nuove zone nella regione Stati Uniti Sud, `sjc03` e `sjc04`. Se hai un firewall, assicurati di [aprire le porte del firewall](/docs/containers?topic=containers-firewall#firewall) per questa zona e le altre all'interno della regione in cui si trova il tuo cluster.</li>
<li>Con le due nuove zone `tok04` e `tok05`, puoi ora [creare cluster multizona](/docs/containers?topic=containers-plan_clusters#multizone) a Tokyo nella regione Asia Pacifico Nord.</li></ul></td>
</tr>
<tr>
<td>05 settembre</td>
<td>[Zona disponibile a Oslo](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Benvenuta Oslo, Norvegia come nuova zona nella regione Europa Centrale. Se hai un firewall, assicurati di [aprire le porte del firewall](/docs/containers?topic=containers-firewall#firewall) per questa zona e le altre all'interno della regione in cui si trova il tuo cluster.</td>
</tr>
</tbody></table>

## Argomenti popolari ad agosto 2018
{: #aug18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes ad agosto 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>31 agosto</td>
<td>{{site.data.keyword.cos_full_notm}} è ora integrato con {{site.data.keyword.containerlong}}</td>
<td>Utilizza le attestazioni del volume persistente (o PVC, persistent volume claim) native di Kubernetes per eseguire il provisioning di {{site.data.keyword.cos_full_notm}} al tuo cluster. L'impiego ottimale di {{site.data.keyword.cos_full_notm}} è per i carichi di lavoro ad alta intensità di lettura e se si desidera archiviare i dati in più zone in un cluster multizona. Inizia [creando una istanza del servizio {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#create_cos_service) e [installando il plugin {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#install_cos) sul tuo cluster. </br></br>Non sei sicuro di quale soluzione di archiviazione potrebbe essere quella giusta per te? Inizia [qui](/docs/containers?topic=containers-storage_planning#choose_storage_solution) per analizzare i tuoi dati e scegliere la soluzione di archiviazione appropriata per i tuoi dati. </td>
</tr>
<tr>
<td>14 agosto</td>
<td>Aggiorna i tuoi cluster a Kubernetes versione 1.11 per assegnare la priorità dei pod</td>
<td>Dopo che hai aggiornato il tuo cluster a [Kubernetes versione 1.11](/docs/containers?topic=containers-cs_versions#cs_v111), puoi avvalerti di nuove funzionalità, come ad esempio delle prestazioni del runtime del contenitore aumentate con `containerd` oppure l'[assegnazione della priorità dei pod](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
</tbody></table>

## Argomenti popolari nel luglio 2018
{: #july18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel luglio 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>30 luglio</td>
<td>[Usa il tuo controller Ingress](/docs/containers?topic=containers-ingress#user_managed)</td>
<td>Hai requisiti di sicurezza particolarmente specifici o altri requisiti personalizzati per il controller Ingress del tuo cluster? Se sì, potresti voler eseguire il tuo controller Ingress invece di quello predefinito.</td>
</tr>
<tr>
<td>10 luglio</td>
<td>Introduzione dei cluster multizona</td>
<td>Vuoi migliorare la disponibilità del cluster? Ora puoi estendere il tuo cluster tra più zone in aree metropolitane selezionate. Per ulteriori informazioni, vedi [Creazione di cluster multizona in {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-plan_clusters#multizone).</td>
</tr>
</tbody></table>

## Argomenti popolari nel giugno 2018
{: #june18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel giugno 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>13 giugno</td>
<td>Il nome del comando CLI `bx` è stato modificato nel comando CLI `ic`</td>
<td>Quando scarichi la versione più recente della CLI di {{site.data.keyword.Bluemix_notm}}, esegui i comandi utilizzando il prefisso `ic` invece di `bx`. Ad esempio, elenca i tuoi cluster eseguendo `ibmcloud ks clusters`.</td>
</tr>
<tr>
<td>12 giugno</td>
<td>[Politiche di sicurezza del pod](/docs/containers?topic=containers-psp)</td>
<td>Per i cluster su cui è in esecuzione Kubernetes 1.10.3 o successive, puoi configurare le politiche di sicurezza del
pod per autorizzare chi può creare e aggiornare i pod in {{site.data.keyword.containerlong_notm}}.</td>
</tr>
<tr>
<td>06 giugno</td>
<td>[Supporto TLS per il dominio secondario jolly Ingress fornito da IBM](/docs/containers?topic=containers-ingress#wildcard_tls)</td>
<td>Per i cluster creati dopo il 6 giugno 2018, il certificato TLS del dominio secondario jolly Ingress fornito da IBM è ora un certificato jolly e può essere usato per il dominio secondario jolly registrato. Per i cluster creati prima del 6 giugno 2018, il certificato TLS verrà aggiornato a un certificato jolly quando verrà rinnovato il certificato TLS corrente.</td>
</tr>
</tbody></table>

## Argomenti popolari nel maggio 2018
{: #may18}


<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel maggio 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>24 maggio</td>
<td>[Nuovo formato del dominio secondario Ingress](/docs/containers?topic=containers-ingress)</td>
<td>I cluster creati dopo il 24 maggio vengono assegnati a un dominio secondario Ingress in un nuovo formato: <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>. Quando utilizzi Ingress per esporre le tue applicazioni, puoi utilizzare il nuovo dominio secondario per accedere alle tue applicazioni da Internet.</td>
</tr>
<tr>
<td>14 maggio</td>
<td>[Aggiornamento: distribuisci i carichi di lavoro nel bare metal GPU nel mondo](/docs/containers?topic=containers-app#gpu_app)</td>
<td>Se hai un [tipo di macchina GPU (graphics processing unit) bare metal](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node) nel tuo cluster, puoi pianificare le applicazioni intensive in modo matematico. Il nodo di lavoro GPU può elaborare il carico di lavoro della tua applicazione tramite la CPU e la GPU per aumentare le prestazioni.</td>
</tr>
<tr>
<td>03 maggio</td>
<td>[Container Image Security Enforcement (beta)](/docs/services/Registry?topic=registry-security_enforce#security_enforce)</td>
<td>Il tuo team ha bisogno di un piccolo ulteriore aiuto per sapere quale immagine trasmettere nei tuoi contenitori dell'applicazione? Prova Container Image Security Enforcement beta per verificare le immagini del contenitore prima di distribuirle. Disponibile per i cluster che eseguono Kubernetes 1.9 o successive.</td>
</tr>
<tr>
<td>01 maggio</td>
<td>[Distribuisci il dashboard Kubernetes dalla console](/docs/containers?topic=containers-app#cli_dashboard)</td>
<td>Hai mai voluto accedere al dashboard Kubernetes con un clic? Controlla il pulsante **Dashboard Kubernetes** nella console {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
</tbody></table>




## Argomenti popolari nell'aprile 2018
{: #apr18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nell'aprile 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>17 aprile</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>Installa il [plugin](/docs/containers?topic=containers-block_storage#install_block) di {{site.data.keyword.Bluemix_notm}} Block Storage per salvare i dati persistenti nell'archiviazione blocchi. Quindi puoi [creare una nuova](/docs/containers?topic=containers-block_storage#add_block) archiviazione blocchi o [utilizzarne una esistente](/docs/containers?topic=containers-block_storage#existing_block) per il tuo cluster.</td>
</tr>
<tr>
<td>13 aprile</td>
<td>[Nuova esercitazione per la migrazione delle applicazioni Cloud Foundry nei cluster](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)</td>
<td>Hai un'applicazione Cloud Foundry? Impara a distribuire lo stesso codice proveniente da tale applicazione ad un'applicazione che viene eseguita in un cluster Kubernetes.</td>
</tr>
<tr>
<td>05 aprile</td>
<td>[Filtraggio dei log](/docs/containers?topic=containers-health#filter-logs)</td>
<td>Filtra specifici log per impedire che vengano inoltrati. I log possono essere filtrati per uno specifico spazio dei nomi, nome contenitore, livello di log e stringa di messaggio.</td>
</tr>
</tbody></table>

## Argomenti popolari nel marzo 2018
{: #mar18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel marzo 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>16 marzo</td>
<td>[Provisioning di un cluster bare metal con Trusted Compute](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)</td>
<td>Crea un cluster bare metal che esegue [Kubernetes versione 1.9](/docs/containers?topic=containers-cs_versions#cs_v19) o successive e abilita Trusted Compute per verificare possibili tentativi di intrusione nei tuoi nodi di lavoro.</td>
</tr>
<tr>
<td>14 marzo</td>
<td>[Accesso sicuro con {{site.data.keyword.appid_full}}](/docs/containers?topic=containers-supported_integrations#appid)</td>
<td>Migliora le tue applicazioni in esecuzione in {{site.data.keyword.containerlong_notm}} richiedendo agli utenti di effettuare l'accesso.</td>
</tr>
<tr>
<td>13 marzo</td>
<td>[Zona disponibile a San Paolo](/docs/containers?topic=containers-regions-and-zones)</td>
<td>San Paolo, in Brasile, è una nuova zona nella regione Stati Uniti Sud. Se hai un firewall, assicurati di [aprire le porte del firewall](/docs/containers?topic=containers-firewall#firewall) per questa zona e le altre all'interno della regione in cui si trova il tuo cluster.</td>
</tr>
</tbody></table>

## Argomenti popolari nel febbraio 2018
{: #feb18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel febbraio 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>27 febbraio</td>
<td>Immagini HVM (hardware virtual machine) per i nodi di lavoro</td>
<td>Aumenta le prestazioni I/O dei tuoi carichi di lavoro con le immagini HVM. Esegui l'attivazione su ciascun nodo di lavoro esistente utilizzando il [comando](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) `ibmcloud ks worker-reload` oppure il [comando](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>26 febbraio</td>
<td>[Ridimensionamento automatico di KubeDNS](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>KubeDNS adesso si ridimensiona con l'espandersi del tuo cluster. Puoi regolare le razioni di ridimensionamento utilizzando il seguente comando: `kubectl -n kube-system edit cm kube-dns-autoscaler`.</td>
</tr>
<tr>
<td>23 febbraio</td>
<td>Visualizza la console web per la [registrazione](/docs/containers?topic=containers-health#view_logs) e le [metriche](/docs/containers?topic=containers-health#view_metrics)</td>
<td>Visualizza facilmente dati di log e metriche sul tuo cluster e i suoi componenti con un'IU web migliorata. Per l'accesso, vedi la pagina dei dettagli del tuo cluster.</td>
</tr>
<tr>
<td>20 febbraio</td>
<td>Immagini crittografate e [contenuti firmati e attendibili](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)</td>
<td>In {{site.data.keyword.registryshort_notm}}, puoi firmare e crittografare le immagini per garantirne l'integrità durante la memorizzazione nello spazio dei nomi del tuo registro. Esegui le tue istanze del contenitore utilizzando solo contenuti attendibili.</td>
</tr>
<tr>
<td>19 febbraio</td>
<td>[Configura la VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup)</td>
<td>Distribuisci rapidamente il grafico Helm della VPN IPSec strongSwan per connettere in modo sicuro il cluster {{site.data.keyword.containerlong_notm}} al tuo data center in loco senza una VRA (Virtual Router Appliance).</td>
</tr>
<tr>
<td>14 febbraio</td>
<td>[Zona disponibile a Seoul](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Giusto in tempo per le Olimpiadi, distribuisci un cluster Kubernetes a Seoul nella regione Asia Pacifico Nord. Se hai un firewall, assicurati di [aprire le porte del firewall](/docs/containers?topic=containers-firewall#firewall) per questa zona e le altre all'interno della regione in cui si trova il tuo cluster.</td>
</tr>
<tr>
<td>08 febbraio</td>
<td>[Aggiorna Kubernetes 1.9](/docs/containers?topic=containers-cs_versions#cs_v19)</td>
<td>Rivedi le modifiche da apportare ai tuoi cluster prima di aggiornare a Kubernetes 1.9.</td>
</tr>
</tbody></table>

## Argomenti popolari nel gennaio 2018
{: #jan18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel gennaio 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<td>25 gennaio</td>
<td>[Registro globale disponibile](/docs/services/Registry?topic=registry-registry_overview#registry_regions)</td>
<td>Con il {{site.data.keyword.registryshort_notm}}, puoi utilizzare il registro globale `registry.bluemix.net` per estrarre le immagini pubbliche fornite da IBM.</td>
</tr>
<tr>
<td>23 gennaio</td>
<td>[Zone disponibili a Singapore e Montreal, CA](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Singapore e Montreal sono le zone disponibili nelle regioni Asia Pacifico Nord e Stati Uniti Est di {{site.data.keyword.containerlong_notm}}. Se hai un firewall, assicurati di [aprire le porte del firewall](/docs/containers?topic=containers-firewall#firewall) per queste zone e le altre all'interno della regione in cui si trova il tuo cluster.</td>
</tr>
<tr>
<td>08 gennaio</td>
<td>[Disponibili varietà avanzate](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)</td>
<td>I tipi di macchine virtuali della serie 2 includono l'archiviazione SSD locale e la crittografia del disco. [Sposta i tuoi carichi di lavoro](/docs/containers?topic=containers-update#machine_type) a queste varietà per migliorare prestazioni e stabilità.</td>
</tr>
</tbody></table>

## Utilizza la chat per parlare con sviluppatori con interessi simili su Slack
{: #slack}

Puoi vedere di cosa parlano gli altri utenti e porre le tue domande in [{{site.data.keyword.containerlong_notm}} Slack. ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-container-service.slack.com)
{:shortdesc}

Se non stai utilizzando un ID IBM per il tuo account {{site.data.keyword.Bluemix_notm}}, [richiedi un invito](https://bxcs-slack-invite.mybluemix.net/) a questo Slack.
{: tip}
