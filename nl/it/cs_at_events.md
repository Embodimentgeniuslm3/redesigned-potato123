---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, audit

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



# Eventi {{site.data.keyword.cloudaccesstrailshort}}
{: #at_events}

Puoi visualizzare, gestire e controllare le attività avviate dall'utente nel tuo cluster {{site.data.keyword.containerlong_notm}} utilizzando il servizio {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

{{site.data.keyword.containershort_notm}} genera due tipi di eventi di {{site.data.keyword.cloudaccesstrailshort}}:

* **Eventi di gestione del cluster**:
    * Questi eventi vengono generati e inoltrati automaticamente a {{site.data.keyword.cloudaccesstrailshort}}.
    * Puoi visualizzare questi eventi attraverso il **dominio account** di {{site.data.keyword.cloudaccesstrailshort}}.

* **Eventi di controllo del server API Kubernetes**:
    * Questi eventi vengono generati automaticamente, ma devi configurare il cluster per inoltrarli al servizio {{site.data.keyword.cloudaccesstrailshort}}.
    * Puoi configurare il cluster per inviare gli eventi al **dominio account** o a un **dominio spazio** di {{site.data.keyword.cloudaccesstrailshort}}. Per ulteriori informazioni, vedi [Invio dei log di controllo](/docs/containers?topic=containers-health#api_forward).

Per ulteriori informazioni su come funziona il servizio, consulta la [documentazione di {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started). Per ulteriori informazioni sulle azioni Kubernetes che vengono tracciate, vedi la [documentazione Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/home/).

{{site.data.keyword.containerlong_notm}} non è attualmente configurato per l'utilizzo di {{site.data.keyword.at_full}}. Per gestire gli eventi di gestione cluster e i log di controllo dell'API Kubernetes, continua a utilizzare {{site.data.keyword.cloudaccesstrailfull_notm}} con LogAnalysis.
{: note}

## Ricerca di informazioni per gli eventi
{: #kube-find}

Puoi monitorare le attività nel tuo cluster controllando i log nel dashboard Kibana.
{: shortdesc}

Per monitorare l'attività amministrativa:

1. Accedi al tuo account {{site.data.keyword.Bluemix_notm}}.
2. Dal catalogo, esegui il provisioning di un'istanza del servizio {{site.data.keyword.cloudaccesstrailshort}} nello stesso account della tua istanza di {{site.data.keyword.containerlong_notm}}.
3. Nella scheda **Gestisci** del dashboard {{site.data.keyword.cloudaccesstrailshort}}, seleziona il dominio di account o spazio.
  * **Log di account**: gli eventi di gestione del cluster e gli eventi di controllo del server API Kubernetes sono disponibili nel **dominio account** relativo alla regione {{site.data.keyword.Bluemix_notm}} in cui vengono generati gli eventi.
  * **Log dello spazio**: se hai specificato uno spazio quando hai configurato il tuo cluster per l'inoltro degli eventi di controllo del server API Kubernetes, questi eventi sono disponibili nel **dominio spazio** associato allo spazio Cloud Foundry in cui è stato eseguito il provisioning del servizio {{site.data.keyword.cloudaccesstrailshort}}.
4. Fai clic su **Visualizza in Kibana**.
5. Imposta l'intervallo di tempo per il quale desideri visualizzare i log. Il valore predefinito è 24 ore.
6. Per restringere la ricerca, puoi fare clic sull'icona di modifica per `ActivityTracker_Account_Search_in_24h` e aggiungere campi nella colonna **Campi disponibili**.

Per consentire ad altri utenti di visualizzare eventi di account e di spazio, vedi [Concessione di autorizzazioni per visualizzare gli eventi di account](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions).
{: tip}

## Traccia degli eventi di gestione del cluster
{: #cluster-events}

Controlla il seguente elenco degli eventi di gestione del cluster che vengono inviati a {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
<tr>
<th>Azione</th>
<th>Descrizione</th></tr><tr>
<td><code>containers-kubernetes.account-credentials.set</code></td>
<td>Sono state impostate le credenziali dell'infrastruttura in una regione per un gruppo di risorse.</td></tr><tr>
<td><code>containers-kubernetes.account-credentials.unset</code></td>
<td>È stata annullata l'impostazione delle credenziali dell'infrastruttura in una regione per un gruppo di risorse.</td></tr><tr>
<td><code>containers-kubernetes.alb.create</code></td>
<td>È stato creato un ALB Ingress.</td></tr><tr>
<td><code>containers-kubernetes.alb.delete</code></td>
<td>È stato eliminato un ALB Ingress.</td></tr><tr>
<td><code>containers-kubernetes.alb.get</code></td>
<td>Sono state visualizzate informazioni sull'ALB Ingress.</td></tr><tr>
<td><code>containers-kubernetes.apikey.reset</code></td>
<td>È stata reimpostata una chiave API per una regione e un gruppo di risorse.</td></tr><tr>
<td><code>containers-kubernetes.cluster.create</code></td>
<td>È stato creato un cluster.</td></tr><tr>
<td><code>containers-kubernetes.cluster.delete</code></td>
<td>È stato eliminato un cluster.</td></tr><tr>
<td><code>containers-kubernetes.cluster-feature.enable</code></td>
<td>Una funzione, ad esempio il Trusted Compute per i nodi di lavoro bare metal, è stata abilitata su un cluster.</td></tr><tr>
<td><code>containers-kubernetes.cluster.get</code></td>
<td>Sono state visualizzate informazioni sul cluster.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.create</code></td>
<td>È stata creata una configurazione di inoltro log.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.delete</code></td>
<td>È stata eliminata una configurazione di inoltro log.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.get</code></td>
<td>Sono state visualizzate informazioni relative a una configurazione di inoltro log.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.update</code></td>
<td>È stata aggiornata una configurazione di inoltro log.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.refresh</code></td>
<td>È stato eseguito un aggiornamento di una configurazione di inoltro log.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.create</code></td>
<td>È stato creato un filtro di registrazione.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.delete</code></td>
<td>È stato eliminato un filtro di registrazione.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.get</code></td>
<td>Sono state visualizzate informazioni relative al filtro di registrazione.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.update</code></td>
<td>È stato aggiornato un filtro di registrazione.</td></tr><tr>
<td><code>containers-kubernetes.logging-autoupdate.changed</code></td>
<td>Il programma di aggiornamento automatico del componente aggiuntivo di registrazione è stato abilitato o disabilitato.</td></tr><tr>
<td><code>containers-kubernetes.mzlb.create</code></td>
<td>È stato creato un programma di bilanciamento del carico multizona.</td></tr><tr>
<td><code>containers-kubernetes.mzlb.delete</code></td>
<td>È stato eliminato un programma di bilanciamento del carico multizona.</td></tr><tr>
<td><code>containers-kubernetes.service.bind</code></td>
<td>È stato eseguito il bind di un servizio a un cluster.</td></tr><tr>
<td><code>containers-kubernetes.service.unbind</code></td>
<td>È stato annullato il bind di un servizio a un cluster.</td></tr><tr>
<td><code>containers-kubernetes.subnet.add</code></td>
<td>Una sottorete dell'infrastruttura IBM Cloud (SoftLayer) esistente è stata aggiunta a un cluster.</td></tr><tr>
<td><code>containers-kubernetes.subnet.create</code></td>
<td>È stata creata una sottorete.</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.add</code></td>
<td>Una sottorete gestita dall'utente è stata aggiunta a un cluster.</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.delete</code></td>
<td>Una sottorete gestita dall'utente è stata rimossa da un cluster.</td></tr><tr>
<td><code>containers-kubernetes.version.update</code></td>
<td>È stata aggiornata la versione Kubernetes di un nodo master del cluster.</td></tr><tr>
<td><code>containers-kubernetes.worker.create</code></td>
<td>È stato creato un nodo di lavoro.</td></tr><tr>
<td><code>containers-kubernetes.worker.delete</code></td>
<td>È stato eliminato un nodo di lavoro.</td></tr><tr>
<td><code>containers-kubernetes.worker.get</code></td>
<td>Sono state visualizzate informazioni relative a un nodo di lavoro.</td></tr><tr>
<td><code>containers-kubernetes.worker.reboot</code></td>
<td>È stato riavviato un nodo di lavoro.</td></tr><tr>
<td><code>containers-kubernetes.worker.reload</code></td>
<td>È stato ricaricato un nodo di lavoro.</td></tr><tr>
<td><code>containers-kubernetes.worker.update</code></td>
<td>È stato aggiornato un nodo di lavoro.</td></tr>
</table>

## Traccia degli eventi di controllo Kubernetes
{: #kube-events}

Controlla la seguente tabella per un elenco degli eventi di controllo del server API Kubernetes che vengono inviati a {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

Prima di iniziare: assicurati che il cluster sia configurato per inoltrare gli [eventi di controllo API Kubernetes](/docs/containers?topic=containers-health#api_forward).

<table>
    <th>Azione</th>
    <th>Descrizione</th><tr>
    <td><code>bindings.create</code></td>
    <td>È stato creato un bind.</td></tr><tr>
    <td><code>certificatesigningrequests.create</code></td>
    <td>È stata creata una richiesta per firmare un certificato.</td></tr><tr>
    <td><code>certificatesigningrequests.delete</code></td>
    <td>È stata eliminata una richiesta per firmare un certificato.</td></tr><tr>
    <td><code>certificatesigningrequests.patch</code></td>
    <td>È stata corretta una richiesta per firmare un certificato.</td></tr><tr>
    <td><code>certificatesigningrequests.update</code></td>
    <td>È stata aggiornata una richiesta per firmare un certificato.</td></tr><tr>
    <td><code>clusterbindings.create</code></td>
    <td>È stato creato un bind del ruolo cluster.</td></tr><tr>
    <td><code>clusterbindings.deleted</code></td>
    <td>È stato eliminato un bind del ruolo cluster.</td></tr><tr>
    <td><code>clusterbindings.patched</code></td>
    <td>È stato corretto un bind del ruolo cluster.</td></tr><tr>
    <td><code>clusterbindings.updated</code></td>
    <td>È stato aggiornato un bind del ruolo cluster.</td></tr><tr>
    <td><code>clusterroles.create</code></td>
    <td>È stato creato un ruolo cluster.</td></tr><tr>
    <td><code>clusterroles.deleted</code></td>
    <td>È stato eliminato un ruolo cluster.</td></tr><tr>
    <td><code>clusterroles.patched</code></td>
    <td>È stato corretto un ruolo cluster.</td></tr><tr>
    <td><code>clusterroles.updated</code></td>
    <td>È stato aggiornato un ruolo cluster.</td></tr><tr>
    <td><code>configmaps.create</code></td>
    <td>È stata creata una mappa di configurazione.</td></tr><tr>
    <td><code>configmaps.delete</code></td>
    <td>È stata eliminata una mappa di configurazione.</td></tr><tr>
    <td><code>configmaps.patch</code></td>
    <td>È stata corretta una mappa di configurazione.</td></tr><tr>
    <td><code>configmaps.update</code></td>
    <td>È stata aggiornata una mappa di configurazione.</td></tr><tr>
    <td><code>controllerrevisions.create</code></td>
    <td>È stata creata una revisione del controller.</td></tr><tr>
    <td><code>controllerrevisions.delete</code></td>
    <td>È stata eliminata una revisione del controller.</td></tr><tr>
    <td><code>controllerrevisions.patch</code></td>
    <td>È stata corretta una revisione del controller.</td></tr><tr>
    <td><code>controllerrevisions.update</code></td>
    <td>È stata aggiornata una revisione del controller.</td></tr><tr>
    <td><code>daemonsets.create</code></td>
    <td>È stata creata una serie di daemon.</td></tr><tr>
    <td><code>daemonsets.delete</code></td>
    <td>È stata eliminata una serie di daemon.</td></tr><tr>
    <td><code>daemonsets.patch</code></td>
    <td>È stata corretta una serie di daemon.</td></tr><tr>
    <td><code>daemonsets.update</code></td>
    <td>È stata aggiornata una serie di daemon.</td></tr><tr>
    <td><code>deployments.create</code></td>
    <td>È stata creata una distribuzione.</td></tr><tr>
    <td><code>deployments.delete</code></td>
    <td>È stata eliminata una distribuzione.</td></tr><tr>
    <td><code>deployments.patch</code></td>
    <td>È stata corretta una distribuzione.</td></tr><tr>
    <td><code>deployments.update</code></td>
    <td>È stata aggiornata una distribuzione.</td></tr><tr>
    <td><code>events.create</code></td>
    <td>È stato creato un evento.</td></tr><tr>
    <td><code>events.delete</code></td>
    <td>È stato eliminato un evento.</td></tr><tr>
    <td><code>events.patch</code></td>
    <td>È stato corretto un evento.</td></tr><tr>
    <td><code>events.update</code></td>
    <td>È stato aggiornato un evento.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.create</code></td>
    <td>In Kubernetes v1.8, è stata creata una configurazione hook di ammissioni esterne.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.delete</code></td>
    <td>In Kubernetes v1.8, è stata eliminata una configurazione hook di ammissioni esterne.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.patch</code></td>
    <td>In Kubernetes v1.8, è stata corretta una configurazione hook di ammissioni esterne.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.update</code></td>
    <td>In Kubernetes v1.8, è stata aggiornata una configurazione hook di ammissioni esterne.</td></tr><tr>
    <td><code>horizontalpodautoscalers.create</code></td>
    <td>È stata creata una politica di ridimensionamento automatico pod orizzontale.</td></tr><tr>
    <td><code>horizontalpodautoscalers.delete</code></td>
    <td>È stata eliminata una politica di ridimensionamento automatico pod orizzontale.</td></tr><tr>
    <td><code>horizontalpodautoscalers.patch</code></td>
    <td>È stata corretta una politica di ridimensionamento automatico pod orizzontale.</td></tr><tr>
    <td><code>horizontalpodautoscalers.update</code></td>
    <td>È stata aggiornata una politica di ridimensionamento automatico pod orizzontale.</td></tr><tr>
    <td><code>ingresses.create</code></td>
    <td>È stato creato un ALB Ingress.</td></tr><tr>
    <td><code>ingresses.delete</code></td>
    <td>È stato eliminato un ALB Ingress.</td></tr><tr>
    <td><code>ingresses.patch</code></td>
    <td>È stato corretto un ALB Ingress.</td></tr><tr>
    <td><code>ingresses.update</code></td>
    <td>È stato aggiornato un ALB Ingress.</td></tr><tr>
    <td><code>jobs.create</code></td>
    <td>È stato creato un lavoro.</td></tr><tr>
    <td><code>jobs.delete</code></td>
    <td>È stato eliminato un lavoro.</td></tr><tr>
    <td><code>jobs.patch</code></td>
    <td>È stato corretto un lavoro.</td></tr><tr>
    <td><code>jobs.update</code></td>
    <td>È stato aggiornato un lavoro.</td></tr><tr>
    <td><code>localsubjectaccessreviews.create</code></td>
    <td>È stata creata una revisione di accesso locale del soggetto.</td></tr><tr>
    <td><code>limitranges.create</code></td>
    <td>È stato creato un limite di intervallo.</td></tr><tr>
    <td><code>limitranges.delete</code></td>
    <td>È stato eliminato un limite di intervallo.</td></tr><tr>
    <td><code>limitranges.patch</code></td>
    <td>È stato corretto un limite di intervallo.</td></tr><tr>
    <td><code>limitranges.update</code></td>
    <td>È stato aggiornato un limite di intervallo.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.create</code></td>
    <td>Viene creata una configurazione webhook oggetto di variazioni.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.delete</code></td>
    <td>Viene eliminata una configurazione webhook oggetto di variazioni.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.patch</code></td>
    <td>Viene corretta una configurazione webhook oggetto di variazioni.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.update</code></td>
    <td>Viene aggiornata una configurazione webhook oggetto di variazioni.</td></tr><tr>
    <td><code>namespaces.create</code></td>
    <td>È stato creato uno spazio dei nomi.</td></tr><tr>
    <td><code>namespaces.delete</code></td>
    <td>È stato eliminato uno spazio dei nomi.</td></tr><tr>
    <td><code>namespaces.patch</code></td>
    <td>È stato corretto uno spazio dei nomi.</td></tr><tr>
    <td><code>namespaces.update</code></td>
    <td>È stato aggiornato uno spazio dei nomi.</td></tr><tr>
    <td><code>networkpolicies.create</code></td>
    <td>È stata creata una politica di rete.</td></tr><tr><tr>
    <td><code>networkpolicies.delete</code></td>
    <td>È stata eliminata una politica di rete.</td></tr><tr>
    <td><code>networkpolicies.patch</code></td>
    <td>È stata corretta una politica di rete.</td></tr><tr>
    <td><code>networkpolicies.update</code></td>
    <td>È stata aggiornata una politica di rete.</td></tr><tr>
    <td><code>nodes.create</code></td>
    <td>È stato creato un nodo.</td></tr><tr>
    <td><code>nodes.delete</code></td>
    <td>È stato eliminato un nodo.</td></tr><tr>
    <td><code>nodes.patch</code></td>
    <td>È stato corretto un nodo.</td></tr><tr>
    <td><code>nodes.update</code></td>
    <td>È stato aggiornato un nodo.</td></tr><tr>
    <td><code>persistentvolumeclaims.create</code></td>
    <td>È stata creata un'attestazione del volume persistente.</td></tr><tr>
    <td><code>persistentvolumeclaims.delete</code></td>
    <td>È stata eliminata un'attestazione del volume persistente.</td></tr><tr>
    <td><code>persistentvolumeclaims.patch</code></td>
    <td>È stata corretta un'attestazione del volume persistente.</td></tr><tr>
    <td><code>persistentvolumeclaims.update</code></td>
    <td>È stata aggiornata un'attestazione del volume persistente.</td></tr><tr>
    <td><code>persistentvolumes.create</code></td>
    <td>È stato creato un volume persistente.</td></tr><tr>
    <td><code>persistentvolumes.delete</code></td>
    <td>È stato eliminato un volume persistente.</td></tr><tr>
    <td><code>persistentvolumes.patch</code></td>
    <td>È stato corretto un volume persistente.</td></tr><tr>
    <td><code>persistentvolumes.update</code></td>
    <td>È stato aggiornato un volume persistente.</td></tr><tr>
    <td><code>poddisruptionbudgets.create</code></td>
    <td>È stato creato un PDB (Pod Disruption Budget).</td></tr><tr>
    <td><code>poddisruptionbudgets.delete</code></td>
    <td>È stato eliminato un PDB (Pod Disruption Budget).</td></tr><tr>
    <td><code>poddisruptionbudgets.patch</code></td>
    <td>È stato corretto un PDB (Pod Disruption Budget).</td></tr><tr>
    <td><code>poddisruptionbudgets.update</code></td>
    <td>È stato aggiornato un PDB (Pod Disruption Budget).</td></tr><tr>
    <td><code>podpresets.create</code></td>
    <td>È stato creato un pod preimpostato.</td></tr><tr>
    <td><code>podpresets.deleted</code></td>
    <td>È stato eliminato un pod preimpostato.</td></tr><tr>
    <td><code>podpresets.patched</code></td>
    <td>È stato corretto un pod preimpostato.</td></tr><tr>
    <td><code>podpresets.updated</code></td>
    <td>È stato aggiornato un pod preimpostato.</td></tr><tr>
    <td><code>pods.create</code></td>
    <td>È stato creato un pod.</td></tr><tr>
    <td><code>pods.delete</code></td>
    <td>È stato eliminato un pod.</td></tr><tr>
    <td><code>pods.patch</code></td>
    <td>È stato corretto un pod.</td></tr><tr>
    <td><code>pods.update</code></td>
    <td>È stato aggiornato un pod.</td></tr><tr>
    <td><code>podsecuritypolicies.create</code></td>
    <td>È stata creata una politica di sicurezza dei pod.</td></tr><tr>
    <td><code>podsecuritypolicies.delete</code></td>
    <td>È stata eliminata una politica di sicurezza dei pod.</td></tr><tr>
    <td><code>podsecuritypolicies.patch</code></td>
    <td>È stata corretta una politica di sicurezza dei pod.</td></tr><tr>
    <td><code>podsecuritypolicies.update</code></td>
    <td>È stata aggiornata una politica di sicurezza dei pod.</td></tr><tr>
    <td><code>podtemplates.create</code></td>
    <td>È stato creato un template di pod.</td></tr><tr>
    <td><code>podtemplates.delete</code></td>
    <td>È stato eliminato un template di pod.</td></tr><tr>
    <td><code>podtemplates.patch</code></td>
    <td>È stato corretto un template di pod.</td></tr><tr>
    <td><code>podtemplates.update</code></td>
    <td>È stato aggiornato un template di pod.</td></tr><tr>
    <td><code>replicasets.create</code></td>
    <td>È stata creata una serie di repliche.</td></tr><tr>
    <td><code>replicasets.delete</code></td>
    <td>È stata eliminata una serie di repliche.</td></tr><tr>
    <td><code>replicasets.patch</code></td>
    <td>È stata corretta una serie di repliche.</td></tr><tr>
    <td><code>replicasets.update</code></td>
    <td>È stata aggiornata una serie di repliche.</td></tr><tr>
    <td><code>replicationcontrollers.create</code></td>
    <td>È stato creato un controller di replica.</td></tr><tr>
    <td><code>replicationcontrollers.delete</code></td>
    <td>È stato eliminato un controller di replica.</td></tr><tr>
    <td><code>replicationcontrollers.patch</code></td>
    <td>È stato corretto un controller di replica.</td></tr><tr>
    <td><code>replicationcontrollers.update</code></td>
    <td>È stato aggiornato un controller di replica.</td></tr><tr>
    <td><code>resourcequotas.create</code></td>
    <td>È stata creata una quota della risorsa.</td></tr><tr>
    <td><code>resourcequotas.delete</code></td>
    <td>È stata eliminata una quota della risorsa.</td></tr><tr>
    <td><code>resourcequotas.patch</code></td>
    <td>È stata corretta una quota della risorsa.</td></tr><tr>
    <td><code>resourcequotas.update</code></td>
    <td>È stata aggiornata una quota della risorsa.</td></tr><tr>
    <td><code>rolebindings.create</code></td>
    <td>È stato creato un bind del ruolo.</td></tr><tr>
    <td><code>rolebindings.deleted</code></td>
    <td>È stato eliminato un bind del ruolo.</td></tr><tr>
    <td><code>rolebindings.patched</code></td>
    <td>È stato corretto un bind del ruolo.</td></tr><tr>
    <td><code>rolebindings.updated</code></td>
    <td>È stato aggiornato un bind del ruolo.</td></tr><tr>
    <td><code>roles.create</code></td>
    <td>È stato creato un ruolo.</td></tr><tr>
    <td><code>roles.deleted</code></td>
    <td>È stato eliminato un ruolo.</td></tr><tr>
    <td><code>roles.patched</code></td>
    <td>È stato corretto un ruolo.</td></tr><tr>
    <td><code>roles.updated</code></td>
    <td>È stato aggiornato un ruolo.</td></tr><tr>
    <td><code>secrets.create</code></td>
    <td>È stato creato un segreto.</td></tr><tr>
    <td><code>secrets.deleted</code></td>
    <td>È stato eliminato un segreto.</td></tr><tr>
    <td><code>secrets.get</code></td>
    <td>È stato visualizzato un segreto.</td></tr><tr>
    <td><code>secrets.patch</code></td>
    <td>È stato corretto un segreto.</td></tr><tr>
    <td><code>secrets.updated</code></td>
    <td>È stato aggiornato un segreto.</td></tr><tr>
    <td><code>selfsubjectaccessreviews.create</code></td>
    <td>È stata creata una revisione dell'accesso del soggetto autonomo.</td></tr><tr>
    <td><code>selfsubjectrulesreviews.create</code></td>
    <td>È stata creata una revisione delle regole del soggetto autonomo.</td></tr><tr>
    <td><code>subjectaccessreviews.create</code></td>
    <td>È stata creata una revisione dell'accesso del soggetto.</td></tr><tr>
    <td><code>serviceaccounts.create</code></td>
    <td>È stato creato un account di servizio.</td></tr><tr>
    <td><code>serviceaccounts.deleted</code></td>
    <td>È stato eliminato un account di servizio.</td></tr><tr>
    <td><code>serviceaccounts.patch</code></td>
    <td>È stato corretto un account di servizio.</td></tr><tr>
    <td><code>serviceaccounts.updated</code></td>
    <td>È stato aggiornato un account di servizio.</td></tr><tr>
    <td><code>services.create</code></td>
    <td>È stato creato un servizio.</td></tr><tr>
    <td><code>services.deleted</code></td>
    <td>È stato eliminato un servizio.</td></tr><tr>
    <td><code>services.patch</code></td>
    <td>È stato corretto un servizio.</td></tr><tr>
    <td><code>services.updated</code></td>
    <td>È stato aggiornato un servizio.</td></tr><tr>
    <td><code>statefulsets.create</code></td>
    <td>È stata creata una serie con stato.</td></tr><tr>
    <td><code>statefulsets.delete</code></td>
    <td>È stata eliminata una serie con stato.</td></tr><tr>
    <td><code>statefulsets.patch</code></td>
    <td>È stata corretta una serie con stato.</td></tr><tr>
    <td><code>statefulsets.update</code></td>
    <td>È stata aggiornata una serie con stato.</td></tr><tr>
    <td><code>tokenreviews.create</code></td>
    <td>È stata creata una revisione token.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.create</code></td>
    <td>Viene creata una convalida della configurazione webhook.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.delete</code></td>
    <td>Viene eliminata una convalida della configurazione webhook.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.patch</code></td>
    <td>Viene corretta una convalida della configurazione webhook.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.update</code></td>
    <td>Viene aggiornata una convalida della configurazione webhook.</td></tr>
</table>
