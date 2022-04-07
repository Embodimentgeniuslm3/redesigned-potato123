---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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
{:note: .note}


# Informazioni sulla versione e azioni di aggiornamento
{: #cs_versions}

## Tipi di versione Kubernetes
{: #version_types}

{{site.data.keyword.containerlong}} supporta contemporaneamente più versioni di Kubernetes. Quando viene rilasciata una versione più recente (n), sono supportate fino a 2 versioni precedenti (n-2). Le versioni che sono più di 2 precedenti rispetto all'ultima (n-3) sono prima dichiarate obsolete e quindi non più supportate.
{:shortdesc}

**Versioni Kubernetes supportate**:
*   Più recente: 1.14.2 
*   Predefinita: 1.13.6
*   Altro: 1.12.9

**Versioni Kubernetes obsolete e non supportate**:
*   Obsoleta: 1.11
*   Non supportate: 1.5, 1.7, 1.8, 1.9, 1.10 

</br>

**Versioni obsolete**: quando i cluster sono in esecuzione su una versione Kubernetes obsoleta, hai un minimo di 30 giorni per controllare ed effettuare l'aggiornamento a una versione di Kubernetes supportata prima che la versione non sia più supportata. Durante il periodo di deprecazione, il tuo cluster è ancora funzionante, ma potrebbe richiedere aggiornamenti a una release supportata per correggere le vulnerabilità di sicurezza. Ad esempio, puoi aggiungere e ricaricare i nodi di lavoro, ma non puoi creare nuovi cluster che utilizzano la versione obsoleta quando la data non supportata dista 30 giorni o meno.

**Versioni non supportate**: se i tuoi cluster eseguono una versione Kubernetes che non è supportata, controlla i seguenti potenziali impatti degli aggiornamenti e, quindi, [aggiorna il cluster](/docs/containers?topic=containers-update#update) immediatamente per continuare a ricevere il supporto e gli importanti aggiornamenti della sicurezza. I cluster non supportati non possono aggiungere o ricaricare i nodi di lavoro esistenti. Puoi appurare se il tuo cluster non è supportato (**unsupported**) esaminando il campo **State** nell'output del comando `ibmcloud ks clusters` oppure nella [console {{site.data.keyword.containerlong_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/kubernetes/clusters).

Se attendi finché il tuo cluster arriva a tre o più versioni secondarie dalla versione più vecchia supportata, non puoi aggiornarlo. Invece, [crea un nuovo cluster](/docs/containers?topic=containers-clusters#clusters), [distribuisci le tue applicazioni](/docs/containers?topic=containers-app#app) nel nuovo cluster ed [elimina](/docs/containers?topic=containers-remove) il cluster non supportato.<br><br>Per evitare questo problema, aggiorna i cluster obsoleti a una versione supportata che non sia precedente alle ultime tre versioni, ad esempio da 1.11 a 1.12, poi esegui l'aggiornamento all'ultima versione, la 1.14. Se i nodi di lavoro eseguono una versione che precede di tre o quattro versioni quella del master, potresti osservare un malfunzionamento dei pod, che assumono stati quali `MatchNodeSelector`, `CrashLoopBackOff` o `ContainerCreating`, finché non aggiorni i nodi di lavoro alla stessa versione del master. Dopo che effettui l'aggiornamento a una versione supportata, il tuo cluster può riprendere le normali operazioni e continuare a ricevere supporto.
{: important}

</br>

Per controllare la versione del server di un cluster, immetti il seguente comando.
```
kubectl version  --short | grep -i server
```
{: pre}

Output di esempio:
```
Server Version: v1.13.6+IKS
```
{: screen}


## Tipi di aggiornamento
{: #update_types}

Per il tuo cluster Kubernetes sono disponibili tre tipi di aggiornamento: principale, secondario e patch.
{:shortdesc}

|Tipo di aggiornamento|Esempi di etichette di versione|Aggiornato da|Impatto
|-----|-----|-----|-----|
|Principale|1.x.x|Tu|Modifiche di funzionamento per i cluster, inclusi script o distribuzioni|
|Secondario|x.9.x|Tu|Modifiche di funzionamento per i cluster, inclusi script o distribuzioni|
|Patch|x.x.4_1510|IBM e tu|Patch Kubernetes e altri aggiornamenti del componente {{site.data.keyword.Bluemix_notm}} Provider come patch di sicurezza e del sistema operativo. IBM aggiorna i master automaticamente, ma tu applichi le patch ai nodi di lavoro. Vedi ulteriori informazioni sulle patch nella seguente sezione.|
{: caption="Impatti degli aggiornamenti di Kubernetes" caption-side="top"}

Quando gli aggiornamenti diventano disponibili, ricevi una notifica quando visualizzi le informazioni relative ai nodi di lavoro, ad esempio con i comandi `ibmcloud ks workers --cluster <cluster>` o `ibmcloud ks worker-get --cluster <cluster> --worker <worker>`.
-  **Aggiornamenti principali e secondari (1.x)**: per prima cosa, [aggiorna il tuo nodo master](/docs/containers?topic=containers-update#master) e poi [aggiorna i nodi di lavoro](/docs/containers?topic=containers-update#worker_node). I nodi di lavoro non possono eseguire una versione principale o secondaria di Kubernetes che sia superiore ai master.
   - Non puoi aggiornare un master Kubernetes a più di tre versioni secondarie in avanti. Ad esempio, se la versione corrente del tuo master è 1.11 e vuoi aggiornarlo alla 1.14, devi prima aggiornarlo alla 1.12.
   - Se utilizzi una versione della CLI `kubectl` che corrisponde almeno alla versione `major.minor` dei tuoi cluster, potresti riscontrare risultati imprevisti. Assicurati di mantenere aggiornate le [versioni della CLI](/docs/containers?topic=containers-cs_cli_install#kubectl) e dei cluster Kubernetes.
-  **Aggiornamenti patch (x.x.4_1510)**: le modifiche tra le patch sono documentate in [Changelog versione](/docs/containers?topic=containers-changelog). Le patch master sono applicate automaticamente ma avvii tu gli aggiornamenti delle patch di nodi di lavoro. I nodi di lavoro possono anche eseguire versioni patch superiori ai master. Man mano che gli aggiornamenti diventano disponibili, ricevi una notifica quando visualizzi informazioni sui nodi master e di lavoro nella console {{site.data.keyword.Bluemix_notm}} o nella CLI, ad esempio con i seguenti comandi: `ibmcloud ks clusters`, `cluster-get`, `workers` o `worker-get`.
   - **Patch di nodi di lavoro**: controlla mensilmente se sono disponibili aggiornamenti e utilizza il [comando `ibmcloud ks worker-update`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) o il [comando `ibmcloud ks worker-reload`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) per applicare queste patch di sicurezza e del sistema operativo. Durante un aggiornamento o un caricamento, viene ricreata l'immagine della macchina del nodo di lavoro e i dati vengono eliminati se non sono [archiviati all'esterno del nodo di lavoro](/docs/containers?topic=containers-storage_planning#persistent_storage_overview).
   - **Patch master**: le patch master vengono applicate automaticamente nel corso di diversi giorni, pertanto una versione della patch master potrebbe essere disponibile prima che venga applicata al tuo master. L'automazione degli aggiornamenti ignora anche i cluster che si trovano in uno stato non integro o che hanno operazioni attualmente in corso. Occasionalmente, IBM potrebbe disabilitare gli aggiornamenti automatici per uno specifico fix pack del master, come indicato nel changelog, ad esempio una patch che è necessaria solo se un master viene aggiornato da una versione secondaria a un'altra. In questi casi, puoi scegliere di utilizzare tranquillamente il [comando `ibmcloud ks cluster-update`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update) manualmente, senza attendere l'applicazione dell'automazione degli aggiornamenti.

</br>

{: #prep-up}
Queste informazioni riassumono gli aggiornamenti che potrebbero avere un impatto sulle applicazioni distribuite quando aggiorni un cluster a una nuova versione dalla versione precedente.
-  [Azioni di preparazione](#cs_v114) per la versione 1.14.
-  [Azioni di preparazione](#cs_v113) per la versione 1.13.
-  [Azioni di preparazione](#cs_v112) per la versione 1.12.
-  **Obsoleta**: [azioni di preparazione](#cs_v111) per la versione 1.11.
-  [Archivio](#k8s_version_archive) di versioni non supportate.

<br/>

Per un elenco completo delle modifiche, esamina le seguenti informazioni:
* [Changelog Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).
* [Changelog versione IBM](/docs/containers?topic=containers-changelog).

</br>

## Cronologia delle release
{: #release-history}

La seguente tabella registra la cronologia delle release della versione {{site.data.keyword.containerlong_notm}}. Puoi utilizzare queste informazioni per finalità di pianificazione, ad esempio per stimare i termini temporali generali di quando una specifica release potrebbe diventare non supportata. Dopo che la community Kubernetes ha rilasciato un aggiornamento della versione, il team di IBM inizia un processo di consolidamento e verifica della release per gli ambienti {{site.data.keyword.containerlong_notm}}. Le date di disponibilità e di cessazione del supporto della release dipendono dai risultati di questi test, dagli aggiornamenti della community, dalle patch di sicurezza e delle modifiche della tecnologia tra le versioni. Pianifica di tenere la versione del tuo master cluster e dei tuoi nodi di lavoro aggiornata in base alla politica di supporto della versione `n-2`.
{: shortdesc}

{{site.data.keyword.containerlong_notm}} è stato inizialmente generalmente disponibile con Kubernetes versione 1.5. Le date di release o di cessazione del supporto previste sono soggette a variazioni. Per andare ai passi di preparazione dell'aggiornamento della versione, fai clic sul numero di versione.

Le date contrassegnate con un simbolo che sembra un pugnale (`†`) non sono definitive e sono soggette a variazioni.
{: important}

<table summary="Questa tabella mostra la cronologia delle release per {{site.data.keyword.containerlong_notm}}.">
<caption>Cronologia delle release per {{site.data.keyword.containerlong_notm}}.</caption>
<col width="20%" align="center">
<col width="20%">
<col width="30%">
<col width="30%">
<thead>
<tr>
<th>Supportata?</th>
<th>Versione</th>
<th>{{site.data.keyword.containerlong_notm}}<br>data di rilascio</th>
<th>{{site.data.keyword.containerlong_notm}}<br>data di cessazione del supporto</th>
</tr>
</thead>
<tbody>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="Questa versione è supportata."/></td>
  <td>[1.14](#cs_v114)</td>
  <td>7 maggio 2019</td>
  <td>Marzo 2020 `†`</td>
</tr>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="Questa versione è supportata."/></td>
  <td>[1.13](#cs_v113)</td>
  <td>05 febbraio 2019</td>
  <td>Dicembre 2019 `†`</td>
</tr>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="Questa versione è supportata."/></td>
  <td>[1.12](#cs_v112)</td>
  <td>07 novembre 2018</td>
  <td>Settembre 2019 `†`</td>
</tr>
<tr>
  <td><img src="images/warning-filled.png" align="left" width="32" style="width:32px;" alt="Questa versione è obsoleta."/></td>
  <td>[1.11](#cs_v111)</td>
  <td>14 agosto 2018</td>
  <td>27 giugno 2019 `†`</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="Questa versione non è supportata."/></td>
  <td>[1.10](#cs_v110)</td>
  <td>01 maggio 2018</td>
  <td>16 maggio 2019</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="Questa versione non è supportata."/></td>
  <td>[1.9](#cs_v19)</td>
  <td>08 febbraio 2018</td>
  <td>27 dicembre 2018</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="Questa versione non è supportata."/></td>
  <td>[1.8](#cs_v18)</td>
  <td>08 novembre 2017</td>
  <td>22 settembre 2018</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="Questa versione non è supportata."/></td>
  <td>[1.7](#cs_v17)</td>
  <td>19 settembre 2017</td>
  <td>21 giugno 2018</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="Questa versione non è supportata."/></td>
  <td>1.6</td>
  <td>N/D</td>
  <td>N/D</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="Questa versione non è supportata."/></td>
  <td>[1.5](#cs_v1-5)</td>
  <td>23 maggio 2017</td>
  <td>04 aprile 2018</td>
</tr>
</tbody>
</table>

<br />


## Versione 1.14
{: #cs_v114}

<p><img src="images/certified_kubernetes_1x14.png" style="padding-right: 10px;" align="left" alt="Questo badge indica la certificazione Kubernetes versione 1.14 per {{site.data.keyword.containerlong_notm}}."/> {{site.data.keyword.containerlong_notm}} è un prodotto certificato Kubernetes per la versione 1.14 secondo il programma Kubernetes Software Conformance Certification di CNCF. _Kubernetes® è un marchio registrato della Linux Foundation negli Stati Uniti e in altri paesi e viene utilizzato in seguito a una licenza dalla Linux Foundation._</p>

Riesamina le modifiche che potresti dover apportare quando esegui l'aggiornamento dalla precedente versione di Kubernetes alla versione 1.14.
{: shortdesc}

Kubernetes 1.14 introduce nuove funzionalità da esplorare. Prova il nuovo [progetto `kustomize` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes-sigs/kustomize) che puoi utilizzare per facilitare la scrittura, la personalizzazione e il riuso delle configurazioni YAML delle risorse Kubernetes. Oppure consulta la nuova [documentazione della CLI `kubectl` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubectl.docs.kubernetes.io/).
{: tip}

### Aggiorna prima del master
{: #114_before}

La seguente tabella mostra le azioni che devi eseguire prima di aggiornare il master Kubernetes.
{: shortdesc}

<table summary="Aggiornamenti Kubernetes per la versione 1.14">
<caption>Modifiche da apportare prima di aggiornare il master a Kubernetes 1.14</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Modifica della struttura della directory del log dei pod CRI</td>
<td>L'interfaccia di runtime del contenitore (CRI) ha modificato la struttura della directory del log dei pod da `/var/log/pods/<UID>` a `/var/log/pods/<NAMESPACE_NAME_UID>`. Se le tue applicazioni escludono Kubernetes e la CRI per accedere direttamente ai log dei pod sui nodi di lavoro, aggiornarle in modo che gestiscano entrambe le strutture delle directory. Questa modifica non influisce sull'accesso ai log dei pod tramite Kubernetes, ad esempio tramite `kubectl logs`.</td>
</tr>
<tr>
<td>I controlli dell'integrazione non seguono più i reindirizzamenti</td>
<td>Le analisi di attività e disponibilità del controllo dell'integrità che utilizzano un `HTTPGetAction` non seguono più i reindirizzamenti ai nomi host diversi dalla richiesta di analisi originale. Questi reindirizzamenti non locali restituiscono invece una risposta `Success` e viene generato un evento con motivazione `ProbeWarning` per indicare che il reindirizzamento è stato ignorato. Se in precedenza per eseguire i controlli dell'integrità per i vari endpoint dei nomi host ti affidavi al reindirizzamento, devi eseguire la logica del controllo di integrità al di fuori del `kubelet`. Ad esempio, invece di reindirizzare la richiesta di analisi potresti delegare l'endpoint esterno.</td>
</tr>
<tr>
<td>Non supportato: provider DNS del cluster KubeDNS</td>
<td>CoreDNS è ora l'unico provider DNS del cluster supportato per i cluster che eseguono Kubernetes versione 1.14 e successive. Se aggiorni alla versione 1.14 un cluster esistente che utilizza KubeDNS come provider DNS del cluster, durante l'aggiornamento KubeDNS viene migrato automaticamente a CoreDNS. Quindi, prima di aggiornare il cluster, valuta la possibilità di [configurare CoreDNS come provider DNS del cluster](/docs/containers?topic=containers-cluster_dns#set_coredns) e verificarlo.<br><br>CoreDNS supporta la [specifica del DNS del cluster ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/dns/blob/master/docs/specification.md#25---records-for-external-name-services) per immettere un nome dominio come campo `ExternalName` del servizio Kubernetes. Il provider di DNS del cluster precedente, KubeDNS, non rispetta la specifica DNS del cluster e, pertanto, consente gli indirizzi IP per `ExternalName`. Se qualche servizio Kubernetes sta utilizzando indirizzi IP invece di DNS, devi aggiornare l'`ExternalName` a DNS per la continuazione del funzionamento.</td>
</tr>
<tr>
<td>Non supportato: funzione alpha `Initializers` di Kubernetes</td>
<td>La funzione alpha `Initializers` di Kubernetes, la versione API `admissionregistration.k8s.io/v1alpha1`, il plug-in del controller di ammissione `Initializers` e l'utilizzo del campo API `metadata.initializers` sono stati rimossi. Se utilizzi `Initializers`, passa all'uso di [webhook di ammissione Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/) ed elimina gli eventuali oggetti API `InitializerConfiguration` esistenti prima di aggiornare il cluster.</td>
</tr>
<tr>
<td>Non supportato: contaminazioni alfa nodi</td>
<td>L'utilizzo delle contaminazioni `node.alpha.kubernetes.io/notReady` e `node.alpha.kubernetes.io/unreachable` non è più supportato. Se ti affidi a queste contaminazioni, aggiorna le tue applicazioni in modo che utilizzino invece le contaminazioni `node.kubernetes.io/not-ready` e `node.kubernetes.io/unreachable`.</td>
</tr>
<tr>
<td>Non supportato: documenti Swagger API Kubernetes</td>
<td>I documenti dell'API degli schemi `swagger/*`, `/swagger.json` e `/swagger-2.0.0.pb-v1` sono stati rimossi e sostituiti dai documenti sull'API dello schema `/openapi/v2`. I documenti su Swagger sono diventati obsoleti nel momento in cui sono stati resi disponibili i documenti OpenAPI in Kubernetes versione 1.10. Inoltre, il server API Kubernetes ora aggrega solo gli schemi OpenAPI dagli endpoint `/openapi/v2` dei server API aggregati. Il fallback da aggregare da `/swagger.json` è stato rimosso. Se hai installato applicazioni che forniscono estensioni API Kubernetes, assicurati che supportino i documenti sull'API dello schema `/openapi/v2`.</td>
</tr>
<tr>
<td>Non supportato e obsoleto: Seleziona metriche</td>
<td>Esamina le metriche [Kubernetes obsolete e rimosse ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.14.md#removed-and-deprecated-metrics). Se utilizzi una qualsiasi di queste metriche obsolete, passa alla metrica sostitutiva disponibile.</td>
</tr>
</tbody>
</table>

### Aggiorna dopo il master
{: #114_after}

La seguente tabella mostra le azioni che devi eseguire dopo aver aggiornato il master Kubernetes.
{: shortdesc}

<table summary="Aggiornamenti Kubernetes per la versione 1.14">
<caption>Modifiche da apportare dopo che aggiorni il master a Kubernetes 1.14</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Non supportato: `kubectl --show-all`</td>
<td>L'indicatore `--show-all` e l'indicatore abbreviato `-a` non sono più supportati. Se i tuoi script si basano su questi indicatori, aggiornali.</td>
</tr>
<tr>
<td>Politiche RBAC predefinite di Kubernetes per gli utenti autenticati</td>
<td>Le politiche RBAC (Role-based Access Control) predefinite di Kubernetes non concedono più l'accesso alle [API di rilevamento e controllo delle autorizzazioni agli utenti non autenticati ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles). Questa modifica si applica solo ai nuovi cluster versione 1.14. Se aggiorni un cluster da una versione precedente, gli utenti non autenticati hanno ancora accesso alle API di rilevamento e controllo delle autorizzazioni. Se vuoi aggiornare il cluster al valore predefinito più sicuro per gli utenti non autenticati, rimuovi il gruppo `system:unauthenticated` del bind dei ruoli del cluster `system:basic-user` e `system:discovery`.</td>
</tr>
<tr>
<td>Obsoleto: query Prometheus che utilizzano le etichette `pod_name` e `container_name`</td>
<td>Aggiorna le query Prometheus che rispondono alle etichette `pod_name` o `container_name` in modo che utilizzino invece le etichette `pod` o `container`. Un esempio di query che potrebbero utilizzare queste etichette obsolete include le metriche di analisi kubelet. Le etichette `pod_name` e `container_name` obsolete diventano non supportate nella release successiva di Kubernetes.</td>
</tr>
</tbody>
</table>

<br />


## Versione 1.13
{: #cs_v113}

<p><img src="images/certified_kubernetes_1x13.png" style="padding-right: 10px;" align="left" alt="Questo badge indica la certificazione Kubernetes versione 1.13 per {{site.data.keyword.containerlong_notm}}."/> {{site.data.keyword.containerlong_notm}} è un prodotto certificato Kubernetes per la versione 1.13 secondo il programma Kubernetes Software Conformance Certification di CNCF. _Kubernetes® è un marchio registrato della Linux Foundation negli Stati Uniti e in altri paesi e viene utilizzato in seguito a una licenza dalla Linux Foundation._</p>

Riesamina le modifiche che potresti dover apportare quando esegui l'aggiornamento dalla precedente versione di Kubernetes alla versione 1.13.
{: shortdesc}

### Aggiorna prima del master
{: #113_before}

La seguente tabella mostra le azioni che devi eseguire prima di aggiornare il master Kubernetes.
{: shortdesc}

<table summary="Aggiornamenti Kubernetes per la versione 1.13">
<caption>Modifiche da apportare prima di aggiornare il master a Kubernetes 1.13</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>N/D</td>
<td></td>
</tr>
</tbody>
</table>

### Aggiorna dopo il master
{: #113_after}

La seguente tabella mostra le azioni che devi eseguire dopo aver aggiornato il master Kubernetes.
{: shortdesc}

<table summary="Aggiornamenti Kubernetes per la versione 1.13">
<caption>Modifiche da apportare dopo aver aggiornato il master a Kubernetes 1.13</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoreDNS disponibile come nuovo provider DNS del cluster predefinito</td>
<td>CoreDNS è ora il provider DNS predefinito per i nuovi cluster in Kubernetes 1.13 e versioni successive. Se aggiorni un cluster esistente alla 1.13 che utilizza KubeDNS come provider DNS del cluster, KubeDNS continua a essere il provider DNS del cluster. Tuttavia, puoi scegliere di [utilizzare invece CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set).
<br><br>CoreDNS supporta la [specifica del DNS del cluster ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/dns/blob/master/docs/specification.md#25---records-for-external-name-services) per immettere un nome dominio come campo `ExternalName` del servizio Kubernetes. Il provider di DNS del cluster precedente, KubeDNS, non rispetta la specifica DNS del cluster e, pertanto, consente gli indirizzi IP per `ExternalName`. Se qualche servizio Kubernetes sta utilizzando indirizzi IP invece di DNS, devi aggiornare l'`ExternalName` a DNS per la continuazione del funzionamento.</td>
</tr>
<tr>
<td>Output `kubectl` per `Deployment` e `StatefulSet`</td>
<td>L'output `kubectl` per `Deployment` e `StatefulSet` ora include una colonna `Ready` ed è più leggibile. Se i tuoi script si basano sul comportamento precedente, aggiornarli.</td>
</tr>
<tr>
<td>Output `kubectl` per `PriorityClass`</td>
<td>L'output `kubectl` per `PriorityClass` ora include una colonna `Value`. Se i tuoi script si basano sul comportamento precedente, aggiornarli.</td>
</tr>
<tr>
<td>`kubectl get componentstatuses`</td>
<td>Il comando `kubectl get componentstatuses` non notifica correttamente l'integrità di alcuni componenti master Kubernetes perché tali componenti non sono più accessibili dal server API Kubernetes ora che `localhost` e le porte non sicure (HTTP) sono disabilitate. Dopo l'introduzione dei master altamente disponibili in Kubernetes versione 1.10, ogni master Kubernetes è configurato con più istanze `apiserver`, `controller-manager`, `scheduler` ed `etcd`. Esamina invece l'integrità del cluster controllando la [console {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/kubernetes/landing) oppure utilizzando il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get) `ibmcloud ks cluster-get`.</td>
</tr>
<tr>
<tr>
<td>Non supportato: `kubectl run-container`</td>
<td>Il comando `kubectl run-container` è stato rimosso. Utilizza invece il comando `kubectl run`.</td>
</tr>
<tr>
<td>`kubectl rollout undo`</td>
<td>Quando esegui `kubectl rollout undo` per una revisione che non esiste, viene restituito un errore. Se i tuoi script si basano sul comportamento precedente, aggiornarli.</td>
</tr>
<tr>
<td>Obsoleto: annotazione `scheduler.alpha.kubernetes.io/critical-pod`</td>
<td>L'annotazione `scheduler.alpha.kubernetes.io/critical-pod` è ora obsoleta. Modifica i pod che si basano su questa annotazione per utilizzare invece la [priorità dei pod](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
</tbody>
</table>

### Aggiorna dopo i nodi di lavoro
{: #113_after_workers}

La seguente tabella mostra le azioni che devi eseguire dopo che hai aggiornato i tuoi nodi di lavoro.
{: shortdesc}

<table summary="Aggiornamenti Kubernetes per la versione 1.13">
<caption>Modifiche da apportare dopo che hai aggiornato i tuoi nodi di lavoro a Kubernetes 1.13</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd `cri` stream server</td>
<td>In containerd versione 1.2, il server di stream del plugin `cri` ora è in ascolto su una porta casuale, `http://localhost:0`. Questa modifica supporta il proxy di streaming `kubelet` e fornisce un'interfaccia di streaming più sicura per le operazioni `exec` e `logs` di contenitore. In precedenza, il server di stream `cri` era in ascolto sull'interfaccia di rete privata del nodo di lavoro utilizzando la porta 10010. Se le tue applicazioni utilizzano il plugin `cri` del contenitore e si basano sulla modalità di funzionamento precedente, eseguine l'aggiornamento.</td>
</tr>
</tbody>
</table>

<br />


## Versione 1.12
{: #cs_v112}

<p><img src="images/certified_kubernetes_1x12.png" style="padding-right: 10px;" align="left" alt="Questo badge indica la certificazione Kubernetes versione 1.12 per il servizio IBM Cloud Container."/> {{site.data.keyword.containerlong_notm}} è un prodotto certificato Kubernetes per la versione 1.12 con il programma Kubernetes Software Conformance Certification di CNCF. _Kubernetes® è un marchio registrato della Linux Foundation negli Stati Uniti e in altri paesi e viene utilizzato in seguito a una licenza dalla Linux Foundation._</p>

Riesamina le modifiche che potresti dover apportare quando esegui l'aggiornamento dalla precedente versione di Kubernetes alla versione 1.12.
{: shortdesc}

### Aggiorna prima del master
{: #112_before}

La seguente tabella mostra le azioni che devi eseguire prima di aggiornare il master Kubernetes.
{: shortdesc}

<table summary="Aggiornamenti Kubernetes per la versione 1.12">
<caption>Modifiche da apportare prima di aggiornare il master a Kubernetes 1.12</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Server delle metriche di Kubernetes</td>
<td>Se attualmente hai il `metric-server` di Kubernetes distribuito nel tuo cluster, devi rimuovere il `metric-server` prima di aggiornare il cluster a Kubernetes 1.12. Questa rimozione impedisce i conflitti con il `metric-server` che viene distribuito durante l'aggiornamento.</td>
</tr>
<tr>
<td>Bind del ruolo per l'account di servizio `default` `kube-system`</td>
<td>L'account di servizio di `default` `kube-system` non ha più l'accesso **cluster-admin** all'API Kubernetes. Se distribuisci funzioni o componenti aggiuntivi come [Helm](/docs/containers?topic=containers-helm#public_helm_install) che richiedono l'accesso ai processi nel tuo cluster, configura un [account di servizio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/). Se hai bisogno di tempo per creare e configurare singoli account di servizio con le autorizzazioni appropriate, puoi concedere temporaneamente il ruolo **cluster-admin** con il seguente bind del ruolo cluster: `kubectl create clusterrolebinding kube-system:default --clusterrole=cluster-admin --serviceaccount=kube-system:default`</td>
</tr>
</tbody>
</table>

### Aggiorna dopo il master
{: #112_after}

La seguente tabella mostra le azioni che devi eseguire dopo aver aggiornato il master Kubernetes.
{: shortdesc}

<table summary="Aggiornamenti Kubernetes per la versione 1.12">
<caption>Modifiche da apportare dopo aver aggiornato il master a Kubernetes 1.12</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>API per Kubernetes</td>
<td>L'API Kubernetes sostituisce le API obsolete nel seguente modo:
<ul><li><strong>apps/v1</strong>: l'API Kubernetes `apps/v1` sostituisce le API `apps/v1beta1` e `apps/v1alpha`. L'API `apps/v1` sostituisce anche l'API `extensions/v1beta1` per le risorse `daemonset`, `deployment`, `replicaset` e `statefulset`. Il progetto Kubernetes sta deprecando ed eliminando gradualmente il supporto per le precedenti API dall'`apiserver` Kubernetes e dal client `kubectl`.</li>
<li><strong>networking.k8s.io/v1</strong>: l'API `networking.k8s.io/v1` sostituisce l'API `extensions/v1beta1` per le risorse NetworkPolicy.</li>
<li><strong>policy/v1beta1</strong>: l'API `policy/v1beta1` sostituisce l'API `extensions/v1beta1` per le risorse `podsecuritypolicy`.</li></ul>
<br><br>Aggiorna tutti i campi `apiVersion` del tuo YAML per utilizzare l'API Kubernetes appropriata prima che le API dichiarate obsolete diventino non supportate. Inoltre, consulta la [documentazione di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) per controllare le modifiche relative ad `apps/v1`, come le seguenti.
<ul><li>Dopo la creazione di una distribuzione, il campo `.spec.selector` è immutabile.</li>
<li>Il campo `.spec.rollbackTo` è obsoleto. Utilizza invece il comando `kubectl rollout undo`.</li></ul></td>
</tr>
<tr>
<td>CoreDNS disponibile come provider DNS del cluster</td>
<td>Il progetto Kubernetes è in fase di transizione per supportare CoreDNS anziché il DNS Kubernetes (KubeDNS) corrente. Nella versione 1.12, il DNS predefinito del cluster rimane KubeDNS, ma puoi [scegliere di utilizzare CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>`kubectl apply --force`</td>
<td>Adesso, quando forzi un'azione di applicazione (`kubectl apply --force`) sulle risorse che non possono essere aggiornate, come i campi immutabili nei file YAML, le risorse vengono invece ricreate. Se i tuoi script si basano sul comportamento precedente, aggiornarli.</td>
</tr>
<tr>
<td>`kubectl get componentstatuses`</td>
<td>Il comando `kubectl get componentstatuses` non notifica correttamente l'integrità di alcuni componenti master Kubernetes perché tali componenti non sono più accessibili dal server API Kubernetes ora che `localhost` e le porte non sicure (HTTP) sono disabilitate. Dopo l'introduzione dei master altamente disponibili in Kubernetes versione 1.10, ogni master Kubernetes è configurato con più istanze `apiserver`, `controller-manager`, `scheduler` ed `etcd`. Esamina invece l'integrità del cluster controllando la [console {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/kubernetes/landing) oppure utilizzando il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get) `ibmcloud ks cluster-get`.</td>
</tr>
<tr>
<td>`kubectl logs --interactive`</td>
<td>L'indicatore `--interactive` non è più supportato per `kubectl logs`. Aggiorna qualsiasi automazione che utilizza questo indicatore.</td>
</tr>
<tr>
<td>`kubectl patch`</td>
<td>Se il comando `patch` non produce alcuna modifica (una patch ridondante), il comando non esce più con un codice di ritorno `1`. Se i tuoi script si basano sul comportamento precedente, aggiornarli.</td>
</tr>
<tr>
<td>`kubectl version -c`</td>
<td>L'indicatore abbreviato `-c` non è più supportato. Utilizza invece l'indicatore completo `--client`. Aggiorna qualsiasi automazione che utilizza questo indicatore.</td>
</tr>
<tr>
<td>`kubectl wait`</td>
<td>Se non vengono trovati selettori corrispondenti, il comando ora stampa un messaggio di errore ed esce con un codice di ritorno `1`. Se i tuoi script si basano sul comportamento precedente, aggiornarli.</td>
</tr>
<tr>
<td>kubelet cAdvisor port</td>
<td>L'interfaccia utente web di [Container Advisor (cAdvisor) ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/google/cadvisor) che kubelet utilizzava avviando `--cadvisor-port` viene rimossa da Kubernetes 1.12. Se hai ancora bisogno di eseguire cAdvisor, [distribuisci cAdvisor come serie di daemon ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/google/cadvisor/tree/master/deploy/kubernetes).<br><br>Nella serie di daemon, specifica la sezione delle porte in modo che sia possibile raggiungere cAdvisor tramite `http://node-ip:4194`, come nel seguente esempio. I pod cAdvisor non funzionano finché i nodi di lavoro non vengono aggiornati alla 1.12, perché le versioni precedenti di kubelet usano la porta host 4194 per cAdvisor.
<pre class="screen"><code>ports:
          - name: http
            containerPort: 8080
            hostPort: 4194
            protocol: TCP</code></pre></td>
</tr>
<tr>
<td>Dashboard Kubernetes</td>
<td>Se accedi al dashboard tramite `kubectl proxy`, il pulsante **SKIP** nella pagina di accesso viene rimosso. [Utilizza invece un **Token** per eseguire l'accesso](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td id="metrics-server">Server delle metriche di Kubernetes</td>
<td>Il server delle metriche di Kubernetes sostituisce Kubernetes Heapster (obsoleto a partire da Kubernetes versione 1.8) come provider di metriche del cluster. Se esegui più di 30 pod per nodo di lavoro nel tuo cluster, [regola la configurazione di `metrics-server` per le prestazioni](/docs/containers?topic=containers-kernel#metrics).
<p>Il dashboard Kubernetes non funziona con il `metrics-server`. Se vuoi visualizzare le metriche in un dashboard, scegli tra le seguenti opzioni.</p>
<ul><li>[Configura Grafana per analizzare le metriche](/docs/services/cloud-monitoring/tutorials?topic=cloud-monitoring-container_service_metrics#container_service_metrics) utilizzando il Dashboard di monitoraggio del cluster.</li>
<li>Distribuisci [Heapster ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/heapster) nel tuo cluster.
<ol><li>Copia i file [YAML ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/heapster-rbac.yaml) `heapster-rbac`, [YAML ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/standalone/heapster-service.yaml) `heapster-service` e [YAML ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/standalone/heapster-controller.yaml) `heapster-controller`.</li>
<li>Modifica il file YAML `heapster-controller` sostituendo le seguenti stringhe.
<ul><li>Sostituisci `{{ nanny_memory }}` con `90Mi`</li>
<li>Sostituisci `{{ base_metrics_cpu }}` con `80m`</li>
<li>Sostituisci `{{ metrics_cpu_per_node }}` con `0.5m`</li>
<li>Sostituisci `{{ base_metrics_memory }}` con `140Mi`</li>
<li>Sostituisci `{{ metrics_memory_per_node }}` con `4Mi`</li>
<li>Sostituisci `{{ heapster_min_cluster_size }}` con `16`</li></ul></li>
<li>Applica i file YAML `heapster-rbac`, `heapster-service` e `heapster-controller` al tuo cluster eseguendo il comando `kubectl apply -f`.</li></ol></li></ul></td>
</tr>
<tr>
<td>API Kubernetes `rbac.authorization.k8s.io/v1`</td>
<td>L'API Kubernetes `rbac.authorization.k8s.io/v1` (supportata a partire da Kubernetes 1.8) sostituirà l'API `rbac.authorization.k8s.io/v1alpha1` e `rbac.authorization.k8s.io/v1beta1`. Non puoi più creare oggetti RBAC come i ruoli o i bind del ruolo con l'API `v1alpha` non supportata. Gli oggetti RBAC esistenti vengono convertiti nell'API `v1`.</td>
</tr>
</tbody>
</table>

<br />


## Obsoleto: Versione 1.11
{: #cs_v111}

<p><img src="images/certified_kubernetes_1x11.png" style="padding-right: 10px;" align="left" alt="Questo badge indica la certificazione Kubernetes versione 1.11 per il servizio IBM Cloud Container."/> {{site.data.keyword.containerlong_notm}} è un prodotto certificato Kubernetes per la versione 1.11 con il programma Kubernetes Software Conformance Certification di CNCF. _Kubernetes® è un marchio registrato della Linux Foundation negli Stati Uniti e in altri paesi e viene utilizzato in seguito a una licenza dalla Linux Foundation._</p>

Riesamina le modifiche che potresti dover apportare quando esegui l'aggiornamento dalla precedente versione di Kubernetes alla versione 1.11.
{: shortdesc}

Kubernetes versione 1.11 è stato dichiarato obsoleto e non sarà più supportato a partire dal 27 giugno 2019 (data ancora non definitiva). [Esamina il potenziale impatto](/docs/containers?topic=containers-cs_versions#cs_versions) di ciascun aggiornamento della versione di Kubernetes e [aggiorna quindi i tuoi cluster](/docs/containers?topic=containers-update#update) immediatamente almeno alla 1.12.
{: deprecated}

Prima di poter aggiornare correttamente un cluster da Kubernetes versione 1.9 o precedente alla versione 1.11, devi seguire la procedura illustrata in [Preparazione dell'aggiornamento a Calico v3](#111_calicov3).
{: important}

### Aggiorna prima del master
{: #111_before}

La seguente tabella mostra le azioni che devi eseguire prima di aggiornare il master Kubernetes.
{: shortdesc}

<table summary="Aggiornamenti Kubernetes per la versione 1.11">
<caption>Modifiche da apportare prima di aggiornare il master a Kubernetes 1.11</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione dell'alta disponibilità (HA) del master cluster</td>
<td>È stata aggiornata la configurazione del master cluster per aumentare l'alta disponibilità (HA). I cluster ora dispongono di tre repliche del master Kubernetes che vengono configurate con ciascun master distribuito su host fisici separati. Inoltre, se il tuo cluster si trova in una zona che supporta il multizona, i master vengono estesi tra le zone.<br><br>Per le azioni che devi eseguire, vedi [Aggiornamento a master cluster altamente disponibili](#ha-masters). Queste azioni di preparazione si applicano:<ul>
<li>Se hai un firewall o politiche di rete Calico personalizzate.</li>
<li>Se utilizzi le porte host `2040` o `2041` sui tuoi nodi di lavoro.</li>
<li>Se hai utilizzato l'indirizzo IP del master cluster per l'accesso in cluster al master.</li>
<li>Se disponi dell'automazione che richiama l'API o la CLI Calico (`calicoctl`), ad esempio per creare le politiche Calico.</li>
<li>Se utilizzi le politiche di rete di Kubernetes o Calico per controllare l'accesso in uscita dei pod al master.</li></ul></td>
</tr>
<tr>
<td>Nuovo runtime del contenitore Kubernetes `containerd`</td>
<td><p class="important">`containerd` sostituisce Docker come nuovo runtime del contenitore per Kubernetes. Per le azioni che devi eseguire, vedi [Aggiornamento a `containerd` come runtime del contenitore](#containerd).</p></td>
</tr>
<tr>
<td>Crittografia dei dati in etcd</td>
<td>In precedenza, i dati etcd erano archiviati sull'istanza di archiviazione file NFS del master che veniva crittografata quando inattiva. Ora, i dati etcd vengono archiviati sul disco locale del master e sottoposti a backup in {{site.data.keyword.cos_full_notm}}. I dati vengono crittografati durante il transito su {{site.data.keyword.cos_full_notm}} e quando inattivi. Tuttavia, i dati etcd sul disco locale del master non vengono crittografati. Se vuoi che i dati etcd locali del master vengano crittografati, [abilita {{site.data.keyword.keymanagementservicelong_notm}} nel tuo cluster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
<tr>
<td>Propagazione del montaggio del volume del contenitore Kubernetes</td>
<td>Il valore predefinito per il campo [`mountPropagation` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/storage/volumes/#mount-propagation) per un `VolumeMount` del contenitore è stato modificato da `HostToContainer` a `None`. Questa modifica ripristina la modalità di funzionamento che esisteva in Kubernetes versione 1.9 e precedente. Se le tue specifiche del pod si basano su `HostToContainer` come valore predefinito, eseguine l'aggiornamento.</td>
</tr>
<tr>
<td>Deserializzatore JSON del server API Kubernetes</td>
<td>Il deserializzatore JSON del server API Kubernetes è ora sensibile a maiuscole/minuscole. Questa modifica ripristina la modalità di funzionamento che esisteva in Kubernetes versione 1.7 e precedente. Se le tue definizioni di risorsa JSON utilizzano maiuscole/minuscole non corrette, procedi ad aggiornarle. <br><br>Sono interessate solo le richieste dirette del server API Kubernetes. La CLI `kubectl` ha continuato a implementare chiavi sensibile a maiuscole/minuscole in Kubernetes versione 1.7 e successive; quindi, se gestisci le tue risorse strettamente con `kubectl`, non sei interessato.</td>
</tr>
</tbody>
</table>

### Aggiorna dopo il master
{: #111_after}

La seguente tabella mostra le azioni che devi eseguire dopo aver aggiornato il master Kubernetes.
{: shortdesc}

<table summary="Aggiornamenti Kubernetes per la versione 1.11">
<caption>Modifiche da apportare dopo aver aggiornato il master a Kubernetes 1.11</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione della registrazione cluster</td>
<td>Il componente aggiuntivo del cluster `fluentd` viene aggiornato automaticamente con la versione 1.11, anche quando `logging-autoupdate` è disabilitata.<br><br>
La directory del log del contenitore è stata modificata da `/var/lib/docker/` a `/var/log/pods/`. Se utilizzi una tua soluzione di registrazione che monitora la directory precedente, procedi a un aggiornamento in modo conforme.</td>
</tr>
<tr>
<td>L'endpoint API per {{site.data.keyword.Bluemix_notm}} IAM (Identity and Access Management)</td>
<td>I cluster che eseguono Kubernetes versione 1.11 o successiva supportano i [gruppi di accesso](/docs/iam?topic=iam-groups#groups) e gli [ID servizio](/docs/iam?topic=iam-serviceids#serviceids) IAM. Ora puoi utilizzare queste funzioni per [autorizzare l'accesso al tuo cluster](/docs/containers?topic=containers-users#users).</td>
</tr>
<tr>
<td>Aggiorna la configurazione di Kubernetes</td>
<td>La configurazione di OpenID Connect per il server API Kubernetes del cluster è stata aggiornata per supportare i gruppi di accesso di {{site.data.keyword.Bluemix_notm}} IAM (Identity and Access Management). Come risultato, devi aggiornare la configurazione Kubernetes del tuo cluster dopo l'aggiornamento del master Kubernetes v1.11 eseguendo `ibmcloud ks cluster-config --cluster <cluster_name_or_ID>`. Con questo comando, la configurazione viene applicata ai bind del ruolo nello spazio dei nomi `default`.<br><br>Se non aggiorni la configurazione, le azioni del cluster non riescono con il seguente messaggio di errore: `You must be logged in to the server (Unauthorized).`</td>
</tr>
<tr>
<td>Dashboard Kubernetes</td>
<td>Se accedi al dashboard tramite `kubectl proxy`, il pulsante **SKIP** nella pagina di accesso viene rimosso. [Utilizza invece un **Token** per eseguire l'accesso](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td>`kubectl` CLI</td>
<td>La CLI `kubectl` per Kubernetes versione 1.11 richiede le API `apps/v1`. Come risultato, la CLI `kubectl` v1.11 non funziona per i cluster che eseguono Kubernetes versione 1.8 o precedenti. Utilizza la versione della CLI `kubectl` che corrisponde alla versione del server API Kubernetes del tuo cluster.</td>
</tr>
<tr>
<td>`kubectl auth can-i`</td>
<td>Ora, quando un utente non è autorizzato, il comando `kubectl auth can-i` non riesce con `exit code 1`. Se i tuoi script si basano sulla modalità di funzionamento precedente, aggiornali.</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>Ora, quando si eliminano risorse utilizzando criteri di selezione quali le etichette, il comando `kubectl delete` ignora gli errori `not found` per impostazione predefinita. Se i tuoi script si basano sul comportamento precedente, aggiornarli.</td>
</tr>
<tr>
<td>Funzione `sysctls` di Kubernetes</td>
<td>L'annotazione `security.alpha.kubernetes.io/sysctls` viene ora ignorata. Kubernetes ha invece aggiunto dei campi agli oggetti `PodSecurityPolicy` e `Pod` per specificare e controllare `sysctls`. Per ulteriori informazioni, vedi [Using sysctls in Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/sysctl-cluster/). <br><br>Dopo che hai aggiornato i nodi di lavoro e il master del cluster, aggiorna i tuoi oggetti `PodSecurityPolicy` e `Pod` per utilizzare i nuovi campi `sysctls`.</td>
</tr>
</tbody>
</table>

### Aggiornamento a master cluster altamente disponibili in Kubernetes 1.11
{: #ha-masters}

Per i cluster che eseguono Kubernetes versione 1.10.8_1530, 1.11.3_1531, o successiva, la configurazione principale del cluster viene aggiornata per aumentare l'alta disponibilità (HA). I cluster ora dispongono di tre repliche del master Kubernetes che vengono configurate con ciascun master distribuito su host fisici separati. Inoltre, se il tuo cluster si trova in una zona che supporta il multizona, i master vengono estesi tra le zone.
{: shortdesc}

Puoi controllare se il tuo cluster ha una configurazione master ad alta disponibilità controllando l'URL master del cluster nella console oppure eseguendo `ibmcloud ks cluster-get --cluster <cluster_name_or_ID`. Se l'URL master ha un nome host come ad esempio ` https://c2.us-south.containers.cloud.ibm.com:xxxxx` e non un indirizzo IP come ad esempio ` https://169.xx.xx.xx:xxxxx`, il cluster ha una configurazione master HA. Potresti ottenere una configurazione master HA a causa di un aggiornamento di patch master automatico oppure applicando un aggiornamento manualmente. In entrambi i casi, devi comunque esaminare i seguenti elementi per assicurati che la tua rete cluster sia impostata per trarre il massimo vantaggio dalla configurazione.

* Se hai un firewall o politiche di rete Calico personalizzate.
* Se utilizzi le porte host `2040` o `2041` sui tuoi nodi di lavoro.
* Se hai utilizzato l'indirizzo IP del master cluster per l'accesso in cluster al master.
* Se disponi dell'automazione che richiama l'API o la CLI Calico (`calicoctl`), ad esempio per creare le politiche Calico.
* Se utilizzi le politiche di rete di Kubernetes o Calico per controllare l'accesso in uscita dei pod al master.

<br>
**Aggiornamento del tuo firewall o delle politiche di rete host Calico personalizzate per i master HA**:</br>
{: #ha-firewall}
Se utilizzi un firewall o delle politiche di rete host Calico personalizzate per controllare l'uscita dai tuoi nodi di lavoro, consenti il traffico in uscita alle porte e agli indirizzi IP per tutte le zone all'interno della regione in cui si trova il tuo cluster. Vedi [Concessione al cluster dell'accesso alle risorse dell'infrastruttura e ad altri servizi](/docs/containers?topic=containers-firewall#firewall_outbound).

<br>
**Riserva delle porte host `2040` e `2041` sui tuoi nodi di lavoro**: </br>
{: #ha-ports}
Per consentire l'accesso al master cluster in una configurazione HA, devi lasciare disponibili le porte host `2040` e `2041` su tutti i nodi di lavoro.
* Aggiorna tutti i pod con `hostPort` impostata su `2040` o `2041` per utilizzare porte diverse.
* Aggiorna tutti i pod con `hostNetwork` impostata su `true` che è in ascolto sulle porte `2040` o `2041` per utilizzare porte diverse.

Per verificare se i tuoi pod stanno attualmente utilizzando le porte `2040` o `2041`, specifica il tuo cluster e immetti il seguente comando.

```
kubectl get pods --all-namespaces -o yaml | grep -B 3 "hostPort: 204[0,1]"
```
{: pre}

Se già hai una configurazione master HA, vedi i risultati per `ibm-master-proxy-*` nello spazio dei nomi `kube-system`, come nel seguente esempio. Se vengono restituiti altri pod, aggiorna le relative porte.

```
name: ibm-master-proxy-static
ports:
- containerPort: 2040
  hostPort: 2040
  name: apiserver
  protocol: TCP
- containerPort: 2041
  hostPort: 2041
...
```
{: screen}


<br>
**Utilizzo dell'IP o del dominio del cluster del servizio `kubernetes` per l'accesso in cluster al master**:</br>
{: #ha-incluster}
Per accedere al master cluster in una configurazione HA all'interno del cluster, utilizza una delle seguenti opzioni:
* L'indirizzo IP del cluster del servizio `kubernetes`, che per impostazione predefinita è: `https://172.21.0.1`
* Il nome di dominio del servizio `kubernetes`, che per impostazione predefinita è: `https://kubernetes.default.svc.cluster.local`

Se in precedenza hai utilizzato l'indirizzo IP del master cluster, questo metodo continua a funzionare. Tuttavia, per una migliore disponibilità, esegui l'aggiornamento per utilizzare l'indirizzo IP o il nome di dominio del cluster del servizio `kubernetes`.

<br>
**Configurazione di Calico per l'accesso fuori dal cluster al master con configurazione HA**:</br>
{: #ha-outofcluster}
I dati archiviati nella mappa di configurazione `calico-config` nello spazio dei nomi `kube-system` vengono modificati per supportare la configurazione del master HA. In particolare, il valore `etcd_endpoints` supporta ora solo l'accesso in cluster. L'utilizzo di questo valore per configurare la CLI di Calico per l'accesso dall'esterno del cluster non funziona più.

Utilizza invece i dati archiviati nella mappa di configurazione `cluster-info` nello spazio dei nomi `kube-system`. In particolare, utilizza i valori `etcd_host` ed `etcd_port` per configurare l'endpoint per la [CLI Calico](/docs/containers?topic=containers-network_policies#cli_install) per accedere al master con configurazione HA dall'esterno del cluster.

<br>
**Aggiornamento delle politiche di rete di Kubernetes o Calico**:</br>
{: #ha-networkpolicies}
Devi eseguire ulteriori azioni se utilizzi le [politiche di rete di Kubernetes o Calico](/docs/containers?topic=containers-network_policies#network_policies) per controllare l'accesso in uscita dei pod al master cluster e se attualmente utilizzi:
*  L'IP del cluster del servizio Kubernetes, che puoi ottenere eseguendo `kubectl get service kubernetes -o yaml | grep clusterIP`.
*  Il nome di dominio del servizio Kubernetes, che per impostazione predefinita è `https://kubernetes.default.svc.cluster.local`.
*  L'IP del master cluster, che puoi ottenere eseguendo `kubectl cluster-info | grep Kubernetes`.

I seguenti passi descrivono come aggiornare le tue politiche di rete di Kubernetes. Per aggiornare le politiche di rete di Calico, ripeti questi passi con alcune modifiche minori alla sintassi della politica e con `calicoctl` per cercare le politiche per gli impatti.
{: note}

Prima di iniziare: [accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Ottieni l'indirizzo IP del tuo master cluster.
    ```
    kubectl cluster-info | grep Kubernetes
    ```
    {: pre}

2.  Cerca le tue politiche di rete Kubernetes per gli impatti. Se non viene restituito alcun YAML, il tuo cluster non subisce alcun impatto e non devi apportare ulteriori modifiche.
    ```
    kubectl get networkpolicies --all-namespaces -o yaml | grep <cluster-master-ip>
    ```
    {: pre}

3.  Esamina il file YAML. Ad esempio, se il tuo cluster utilizza la seguente politica di rete Kubernetes per consentire ai pod nello spazio dei nomi `default` di accedere al master cluster tramite l'IP del cluster o l'IP del master cluster del servizio `kubernetes`, dovrai aggiornare la politica.
    ```
    apiVersion: networking.k8s.io/v1
    kind: NetworkPolicy
    metadata:
      name: all-master-egress
      namespace: default
    spec:
      egress:
      # Allow access to cluster master using kubernetes service cluster IP address
      # or domain name or cluster master IP address.
      - ports:
        - protocol: TCP
        to:
        - ipBlock:
            cidr: 161.202.126.210/32
      # Allow access to Kubernetes DNS in order to resolve the kubernetes service
      # domain name.
      - ports:
        - protocol: TCP
          port: 53
        - protocol: UDP
          port: 53
      podSelector: {}
      policyTypes:
      - Egress
    ```
    {: screen}

4.  Rivedi la politica di rete Kubernetes per consentire l'uscita all'indirizzo IP del proxy master nel cluster `172.20.0.1`. Per ora, mantieni l'indirizzo IP del master cluster. Ad esempio, il precedente esempio di politica di rete cambia come riportato di seguito.

    Se in precedenza hai configurato le tue politiche di uscita per aprire solo il singolo indirizzo IP e la porta per il singolo master Kubernetes, ora utilizza l'intervallo di indirizzi IP del proxy master nel cluster 172.20.0.1/32 e la porta 2040.
    {: tip}

    ```
    apiVersion: networking.k8s.io/v1
    kind: NetworkPolicy
    metadata:
      name: all-master-egress
      namespace: default
    spec:
      egress:
      # Allow access to cluster master using kubernetes service cluster IP address
      # or domain name.
      - ports:
        - protocol: TCP
        to:
        - ipBlock:
            cidr: 172.20.0.1/32
        - ipBlock:
            cidr: 161.202.126.210/32
      # Allow access to Kubernetes DNS in order to resolve the kubernetes service domain name.
      - ports:
        - protocol: TCP
          port: 53
        - protocol: UDP
          port: 53
      podSelector: {}
      policyTypes:
      - Egress
    ```
    {: screen}

5.  Applica la politica di rete rivista al tuo cluster.
    ```
    kubectl apply -f all-master-egress.yaml
    ```
    {: pre}

6.  Dopo aver completato tutte le [azioni di preparazione](#ha-masters) (inclusi questi passi), [aggiorna il tuo master cluster](/docs/containers?topic=containers-update#master) al fix pack del master HA.

7.  Al termine dell'aggiornamento, rimuovi l'indirizzo IP del master cluster dalla politica di rete. Ad esempio, dalla politica di rete precedente, rimuovi le seguenti righe e quindi riapplica la politica.

    ```
    - ipBlock:
        cidr: 161.202.126.210/32
    ```
    {: screen}

    ```
    kubectl apply -f all-master-egress.yaml
    ```
    {: pre}

### Aggiornamento a `containerd` come runtime del contenitore
{: #containerd}

Per i cluster che eseguono Kubernetes versione 1.11 o successive, `containerd` sostituisce Docker come nuovo runtime del contenitore per Kubernetes per migliorare le prestazioni. Se i tuoi pod si basano su Docker come runtime del contenitore Kubernetes, devi aggiornarli per gestire `containerd` come runtime del contenitore. Per ulteriori informazioni, vedi l'[annuncio relativo a containerd di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/blog/2018/05/24/kubernetes-containerd-integration-goes-ga/).
{: shortdesc}

**Come faccio a sapere se le mie applicazioni si basano su `docker` invece di `containerd`?**<br>
Esempi di volte in cui ti potresti basare su Docker come runtime del contenitore:
*  Se accedi all'API o al Docker Engine direttamente utilizzando contenitori privilegiati, aggiorna i tuoi pod per supportare `containerd` come runtime. Ad esempio, potresti richiamare direttamente il socket Docker per avviare i contenitori o per eseguire altre operazioni di Docker. Il socket Docker è cambiato da `/var/run/docker.sock` a `/run/containerd/containerd.sock`. Il protocollo utilizzato nel socket `containerd` è leggermente diverso da quello usato in Docker. Prova ad aggiornare la tua applicazione al socket `containerd`. Se vuoi continuare a utilizzare il socket Docker, prova a utilizzare [Docker-inside-Docker (DinD) ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://hub.docker.com/_/docker/).
*  Alcuni componenti aggiuntivi di terze parti, come gli strumenti di registrazione e monitoraggio, che installi nel tuo cluster potrebbero basarsi su Docker Engine. Contatta il tuo provider per verificare che gli strumenti siano compatibili con containerd. Possibili casi di utilizzo includono:
   - Il tuo strumento di registrazione potrebbe utilizzare la directory `/var/log/pods/<pod_uuid>/<container_name>/*.log` del contenitore `stderr/stdout` per accedere ai log. In Docker, questa directory è un collegamento simbolico a `/var/data/cripersistentstorage/containers/<container_uuid>/<container_uuid>-json.log` mentre in `containerd` accedi alla directory direttamente senza un collegamento simbolico.
   - Il tuo strumento di monitoraggio accede direttamente al socket Docker. Il socket Docker è cambiato da `/var/run/docker.sock` a `/run/containerd/containerd.sock`.

<br>

**Oltre a basarmi sul runtime, devo eseguire altre azioni di preparazione?**<br>

**Strumento manifest**: se hai delle immagini multipiattaforma create con lo strumento sperimentale `docker manifest` [![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.docker.com/edge/engine/reference/commandline/manifest/) prima di Docker versione 18.06, non puoi eseguire il pull dell'immagine da DockerHub utilizzando `containerd`.

Quando controlli gli eventi pod, potresti vedere un errore simile al seguente:
```
failed size validation
```
{: screen}

Per utilizzare un'immagine creata utilizzando lo strumento manifest con `containerd`, scegli dalle seguenti opzioni:

*  Ricrea l'immagine con lo [strumento manifest ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/estesp/manifest-tool).
*  Ricrea l'immagine con lo strumento `docker-manifest` dopo che hai eseguito l'aggiornamento a Docker versione 18.06 o successive.

<br>

**Cosa non è influenzato? Devo modificare il modo in cui distribuisco i miei contenitori?**<br>
In generale, i tuoi processi di distribuzione dei contenitori non cambiano. Puoi continuare a utilizzare un Dockerfile per definire un'immagine Docker e creare un contenitore Docker per le tue applicazioni. Se utilizzi i comandi `docker` per creare immagini ed eseguirne il push a un registro, puoi continuare a usare invece i comandi `docker` o `ibmcloud cr`.

### Preparazione dell'aggiornamento a Calico v3
{: #111_calicov3}

Se stai aggiornando un cluster da Kubernetes versione 1.9 o precedente alla versione 1.11, prepara l'aggiornamento di Calico v3 prima di aggiornare il master. Durante l'aggiornamento master a Kubernetes v1.11, non vengono pianificati nuovi pod o nuove politiche di rete Kubernetes o Calico. La quantità di tempo dell'aggiornamento impedisce nuove variazioni di pianificazione. I cluster piccoli possono impiegare pochi minuti, con pochi minuti extra per ogni 10 nodi. I pod e le politiche di rete esistenti continuano ad essere in esecuzione.
{: shortdesc}

Se stai aggiornando un cluster da Kubernetes versione 1.10 alla versione 1.11, puoi ignorare questi passi perché li hai completati quando hai eseguito l'aggiornamento alla 1.10.
{: note}

Prima di iniziare, il master del cluster e tutti i nodi di lavoro devono eseguire Kubernetes versione 1.8 o 1.9 e deve essere presente almeno un nodo di lavoro.

1.  Verifica che i tuoi pod Calico siano integri.
    ```
    kubectl get pods -n kube-system -l k8s-app=calico-node -o wide
    ```
    {: pre}

2.  Se tutti i pod non sono nello stato **In esecuzione**, elimina il pod e attendi finché non è in uno stato **In esecuzione** prima di continuare. Se il pod non ritorna a uno stato **In esecuzione**:
    1.  Controlla le **Condizioni** e lo **Stato** del nodo di lavoro.
        ```
        ibmcloud ks workers --cluster <cluster_name_or_ID>
        ```
        {: pre}
    2.  Se lo stato del nodo di lavoro non è **Normale**, segui la procedura per il [Debug dei nodi di lavoro](/docs/containers?topic=containers-cs_troubleshoot#debug_worker_nodes). Ad esempio, uno stato **Critico** o **Sconosciuto** viene spesso risolto [ricaricando il nodo di lavoro](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload).

3.  Se generi automaticamente le politiche o altre risorse Calico, aggiorna il tuo strumento di automazione per generare queste risorse con la [sintassi Calico v3 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/).

4.  Se utilizzi [strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) per la connettività VPN, il grafico Helm di strongSwan 2.0.0 non funziona con Calico v3 o Kubernetes 1.11. [Aggiorna strongSwan](/docs/containers?topic=containers-vpn#vpn_upgrade) al grafico Helm 2.1.0, che è compatibile con le versioni precedenti Calico 2.6 e Kubernetes 1.7, 1.8 e 1.9.

5.  [Aggiorna il tuo master del cluster a Kubernetes v1.11](/docs/containers?topic=containers-update#master).

<br />


## Archivio
{: #k8s_version_archive}

Trova una panoramica delle versioni Kubernetes che non sono supportate in {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

### Versione 1.10 (non supportata)
{: #cs_v110}

A partire dal 16 maggio 2019, i cluster {{site.data.keyword.containerlong_notm}} che eseguono [Kubernetes versione 1.10](/docs/containers?topic=containers-changelog#changelog_archive) non sono supportati. I cluster della versione 1.10 non possono ricevere aggiornamenti di sicurezza o supporto a meno che non vengano aggiornati alla versione successiva più recente.
{: shortdesc}

[Esamina il potenziale impatto](/docs/containers?topic=containers-cs_versions#cs_versions) di ciascun aggiornamento della versione di Kubernetes e [aggiorna quindi i tuoi cluster](/docs/containers?topic=containers-update#update) a [Kubernetes versione 1.12](#cs_v112) poiché Kubernetes 1.1 è obsoleto.

### Versione 1.9 (non supportata)
{: #cs_v19}

A partire dal 27 dicembre 2018, i cluster {{site.data.keyword.containerlong_notm}} che eseguono [Kubernetes versione 1.9](/docs/containers?topic=containers-changelog#changelog_archive) non sono supportati. I cluster della versione 1.9 non possono ricevere aggiornamenti di sicurezza o supporto a meno che non vengano aggiornati alla versione successiva più recente.
{: shortdesc}

[Esamina il potenziale impatto](/docs/containers?topic=containers-cs_versions#cs_versions) di ciascun aggiornamento della versione di Kubernetes e [aggiorna quindi i tuoi cluster](/docs/containers?topic=containers-update#update), prima a [Kubernetes versione 1.11 (obsoleta)](#cs_v111), quindi, immediatamente a [Kubernetes versione 1.12](#cs_v112).

### Versione 1.8 (non supportata)
{: #cs_v18}

A partire dal 22 settembre 2018, i cluster {{site.data.keyword.containerlong_notm}} che eseguono [Kubernetes versione 1.8](/docs/containers?topic=containers-changelog#changelog_archive) non sono supportati. I cluster della versione 1.8 non possono ricevere aggiornamenti di sicurezza o supporto.
{: shortdesc}

Per continuare ad eseguire le tue applicazioni in {{site.data.keyword.containerlong_notm}}, [crea un nuovo cluster](/docs/containers?topic=containers-clusters#clusters) e [distribuisci le tue applicazioni](/docs/containers?topic=containers-app#app) al nuovo cluster.

### Versione 1.7 (non supportata)
{: #cs_v17}

A partire dal 21 giugno 2018, i cluster {{site.data.keyword.containerlong_notm}} che eseguono [Kubernetes versione 1.7](/docs/containers?topic=containers-changelog#changelog_archive) non sono supportati. I cluster della versione 1.7 non possono ricevere aggiornamenti di sicurezza o supporto.
{: shortdesc}

Per continuare ad eseguire le tue applicazioni in {{site.data.keyword.containerlong_notm}}, [crea un nuovo cluster](/docs/containers?topic=containers-clusters#clusters) e [distribuisci le tue applicazioni](/docs/containers?topic=containers-app#app) al nuovo cluster.

### Versione 1.5 (non supportata)
{: #cs_v1-5}

A partire dal 4 aprile 2018, i cluster {{site.data.keyword.containerlong_notm}} che eseguono [Kubernetes versione 1.5](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.5.md) non sono supportati. I cluster della versione 1.5 non possono ricevere aggiornamenti di sicurezza o supporto.
{: shortdesc}

Per continuare ad eseguire le tue applicazioni in {{site.data.keyword.containerlong_notm}}, [crea un nuovo cluster](/docs/containers?topic=containers-clusters#clusters) e [distribuisci le tue applicazioni](/docs/containers?topic=containers-app#app) al nuovo cluster.
