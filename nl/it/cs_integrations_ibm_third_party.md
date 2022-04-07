---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

keywords: kubernetes, iks, helm

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


# Servizi IBM Cloud e integrazioni di terze parti
{: #ibm-3rd-party-integrations}

Puoi utilizzare i servizi di infrastruttura e piattaforma {{site.data.keyword.Bluemix_notm}} e altre integrazioni di terze parti per aggiungere ulteriori funzionalità al tuo cluster.
{: shortdesc}

## Servizi IBM Cloud
{: #ibm-cloud-services}

Esamina le seguenti informazioni per vedere come i servizi di piattaforma e infrastruttura {{site.data.keyword.Bluemix_notm}} vengono integrati con {{site.data.keyword.containerlong_notm}} e come puoi utilizzarli nel tuo cluster.
{: shortdesc}

### Servizi di piattaforma IBM Cloud
{: #platform-services}

Tutti i servizi di piattaforma {{site.data.keyword.Bluemix_notm}} che supportano le chiavi del servizio possono essere integrati utilizzando il [bind del servizio](/docs/containers?topic=containers-service-binding) {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Il bind del servizio ti consente di creare rapidamente le credenziali per un servizio {{site.data.keyword.Bluemix_notm}} e archiviarle in un segreto Kubernetes nel tuo cluster. Il segreto Kubernetes viene crittografato automaticamente in etcd per proteggere i tuoi dati. Le tue applicazioni possono utilizzare le credenziali contenute nel segreto per accedere alla tua istanza del servizio {{site.data.keyword.Bluemix_notm}}.

I servizi che non supportano le chiavi del servizio di solito forniscono un'API che puoi utilizzare direttamente nella tua applicazione.

Per trovare una panoramica dei servizi {{site.data.keyword.Bluemix_notm}}, vedi [Integrazioni popolari](/docs/containers?topic=containers-supported_integrations#popular_services).

### Servizi di infrastruttura IBM Cloud
{: #infrastructure-services}

Poiché {{site.data.keyword.containerlong_notm}} ti consente di creare un cluster Kubernetes basato sull'infrastruttura {{site.data.keyword.Bluemix_notm}}, alcuni servizi di infrastruttura, quali Virtual Server, Bare Metal Server o VLAN, sono completamente integrati in {{site.data.keyword.containerlong_notm}}. Crei e lavori con queste istanze del servizio utilizzando l'API, la CLI o la console {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Le soluzioni di archiviazione persistente supportate, come {{site.data.keyword.Bluemix_notm}} File Storage, {{site.data.keyword.Bluemix_notm}} Block Storage o {{site.data.keyword.cos_full}} sono integrate come driver flessibili di Kubernetes e possono essere configurate utilizzando i [grafici Helm](/docs/containers?topic=containers-helm). Il grafico Helm configura automaticamente le classi di archiviazione Kubernetes, il provider di archiviazione e il driver di archiviazione nel tuo cluster. Puoi utilizzare le classi di archiviazione per eseguire il provisioning dell'archiviazione persistente utilizzando le attestazioni del volume persistente (o PVC, persistent volume claim).

Per proteggere la tua rete cluster o per connetterti a un data center in loco, puoi configurare una delle seguenti opzioni:
- [Servizio VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup)
- [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)
- [VRA (Virtual Router Appliance)](/docs/containers?topic=containers-vpn#vyatta)
- [FSA (Fortigate Security Appliance)](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)

## Community Kubernetes e integrazioni open source
{: #kube-community-tools}

Poiché sei proprietario dei cluster standard che crei in {{site.data.keyword.containerlong_notm}}, puoi scegliere di installare soluzioni di terze parti per aggiungere ulteriori funzionalità al tuo cluster.
{: shortdesc}

Alcune tecnologie open source, come Knative, Istio, LogDNA, Sysdig o Portworx, sono testate da IBM e fornite come componenti aggiuntivi gestiti, grafici Helm o servizi {{site.data.keyword.Bluemix_notm}} che vengono gestiti dal provider dei servizi in collaborazione con IBM. Questi strumenti open source sono completamente integrati nel sistema di fatturazione e supporto {{site.data.keyword.Bluemix_notm}}.

Puoi installare altri strumenti open source nel tuo cluster, ma questi strumenti potrebbero non essere gestiti, supportati o verificati per funzionare con {{site.data.keyword.containerlong_notm}}.

### Integrazioni gestite in collaborazione
{: #open-source-partners}

Per ulteriori informazioni sui partner {{site.data.keyword.containerlong_notm}} e sui vantaggi di ciascuna soluzione che forniscono, vedi [Partner di {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-service-partners).

### Componenti aggiuntivi gestiti
{: #cluster-add-ons}
{{site.data.keyword.containerlong_notm}} integra le popolari integrazioni open source, quali [Knative](/docs/containers?topic=containers-serverless-apps-knative) o [Istio](/docs/containers?topic=containers-istio), utilizzando i [componenti aggiuntivi gestiti](/docs/containers?topic=containers-managed-addons). I componenti aggiuntivi gestiti rappresentano un modo semplice per installare nel tuo cluster uno strumento open source che viene testato da IBM e approvato per l'utilizzo in {{site.data.keyword.containerlong_notm}}.

I componenti aggiuntivi gestiti sono pienamente integrati nell'organizzazione di supporto {{site.data.keyword.Bluemix_notm}}. Se hai domande o riscontri un problema nell'utilizzo dei componenti aggiuntivi gestiti, puoi utilizzare uno dei canali di supporto {{site.data.keyword.containerlong_notm}}. Per ulteriori informazioni, vedi [Come ottenere aiuto e supporto](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

Se lo strumento che aggiungi al tuo cluster comporta dei costi, questi costi vengono automaticamente integrati ed elencati come parte della fatturazione mensile di {{site.data.keyword.Bluemix_notm}}. {{site.data.keyword.Bluemix_notm}} determina il ciclo di fatturazione sulla base della data di abilitazione del componente aggiuntivo nel tuo cluster.

### Altre integrazioni di terze parti
{: #kube-community-helm}

Puoi installare qualsiasi strumento open source di terze parti che si integri con Kubernetes. Ad esempio, la community Kubernetes designa alcuni grafici Helm come `stable` o `incubator`. Nota che l'utilizzo di questi grafici o strumenti in {{site.data.keyword.containerlong_notm}} non è verificato. Se lo strumento richiede una licenza, devi acquistare una licenza prima di utilizzare lo strumento. Per una panoramica dei grafici Helm disponibili dalla community Kubernetes, vedi i repository `kubernetes` e `kubernetes-incubator` nel catalogo dei [grafici Helm ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/kubernetes/solutions/helm-charts).
{: shortdesc}

Eventuali costi sostenuti utilizzando un'integrazione open source di terze parti non sono inclusi nella tua fattura mensile di {{site.data.keyword.Bluemix_notm}}.

L'installazione di integrazioni open source di terze parti o di grafici Helm dalla community Kubernetes potrebbe modificare la configurazione predefinita del cluster e portare il tuo cluster in uno stato non supportato. Se riscontri un problema con l'utilizzo di uno di questi strumenti, consulta direttamente la community Kubernetes o il provider dei servizi.
{: important}
