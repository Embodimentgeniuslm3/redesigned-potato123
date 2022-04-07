---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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


# Changelog versione
{: #changelog}

Visualizza le informazioni sulle modifiche alla versione per gli aggiornamenti principali, secondari e patch disponibili per i tuoi cluster Kubernetes {{site.data.keyword.containerlong}}. Le modifiche includono gli aggiornamenti ai componenti Kubernetes e {{site.data.keyword.Bluemix_notm}} Provider.
{:shortdesc}

Se non diversamente indicato nei changelog, la versione del provider {{site.data.keyword.containerlong_notm}} abilita le funzioni e le API Kubernetes che sono in beta. Le funzioni alpha di Kubernetes, che sono soggette a modifica, sono disabilitate.

Per ulteriori informazioni sulle versioni principali, secondarie e patch e sulle azioni di preparazione tra le versioni secondarie, vedi [Versioni Kubernetes](/docs/containers?topic=containers-cs_versions).
{: tip}

Per informazioni sulle modifiche dalla versione precedente, vedi i seguenti changelog.
-  [Changelog](#114_changelog) versione 1.14.
-  [Changelog](#113_changelog) versione 1.13.
-  [Changelog](#112_changelog) versione 1.12.
-  **Obsoleto**: [changelog](#111_changelog) versione 1.11.
-  [Archivio](#changelog_archive) di changelog per le versioni non supportate.

Alcuni changelog sono per i _fix pack del nodo di lavoro_ e si applicano solo ai nodi di lavoro. Devi [applicare queste patch](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) per garantire la conformità di sicurezza per i tuoi nodi di lavoro. Questi fix pack di nodo di lavoro possono essere a una versione superiore rispetto al master perché alcuni fix pack di build sono specifici per i nodi di lavoro. Altri changelog sono per i _fix pack del master_ e si applicano solo al master del cluster. I fix pack del master potrebbero non essere applicati automaticamente. Puoi scegliere di [applicarli manualmente](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update). Per ulteriori informazioni sui tipi di patch, vedi [Tipi di aggiornamento](/docs/containers?topic=containers-cs_versions#update_types).
{: note}

</br>

## Changelog versione 1.14
{: #114_changelog}

### Changelog per 1.14.2_1521, rilasciato il 4 giugno 2019
{: #1142_1521}

La seguente tabella mostra le modifiche incluse nella patch 1.14.2_1521.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.14.1_1519">
<caption>Modifiche intervenute dalla versione 1.14.1_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione del DNS del cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato corretto un bug che poteva lasciare entrambi i pod CoreDNS e DNS Kubernetes in esecuzione dopo le operazioni di `create` o `update` del cluster.</td>
</tr>
<tr>
<td>Configurazione HA del master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione è stata aggiornata per ridurre al minimo gli errori di connettività della rete del master intermittenti durante un aggiornamento del master.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Vedi le [note sulla release etcd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Immagine aggiornata per [CVE-2018-10844 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), e [CVE-2019-5436 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.14.1-71</td>
<td>v1.14.2-100</td>
<td>Aggiornato per supportare la release Kubernetes 1.14.2.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.14.1</td>
<td>v1.14.2</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.2).</td>
</tr>
<tr>
<td>Server delle metriche di Kubernetes</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>Vedi le [note sulla release del server delle metriche Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Immagine aggiornata per [CVE-2018-10844 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), e [CVE-2019-5436 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.14.1_1519, rilasciato il 20 maggio 2019
{: #1141_1519}

La seguente tabella mostra le modifiche incluse nella patch 1.14.1_1519.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.14.1_1518">
<caption>Modifiche intervenute dalla versione 1.14.1_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel Ubuntu 16.04</td>
<td>4.4.0-145 generica</td>
<td>4.4.0-148 generica</td>
<td>Immagini del nodo di lavoro aggiornate con l'aggiornamento del kernel per [CVE-2018-12126 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Kernel Ubuntu 18.04</td>
<td>4.15.0-47 generica</td>
<td>4.15.0-50 generica</td>
<td>Immagini del nodo di lavoro aggiornate con l'aggiornamento del kernel per [CVE-2018-12126 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Changelog per 1.14.1_1518, rilasciato il 13 maggio 2019
{: #1141_1518}

La seguente tabella mostra le modifiche incluse nella patch 1.14.1_1518.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.14.1_1516">
<caption>Modifiche intervenute dalla versione 1.14.1_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy HA del master cluster</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Vedi le [note sulla release di HAProxy ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.haproxy.org/download/1.9/src/CHANGELOG). L'aggiornamento risolve [CVE-2019-6706 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione della politica di controllo del server API Kubernetes è stata aggiornata per non registrare l'URL di sola lettura `/openapi/v2*`. Inoltre, la configurazione del gestore controller Kubernetes ha aumentato la durata di validità dei certificati `kubelet` firmati da 1 a 3 anni.</td>
</tr>
<tr>
<td>Configurazione del client OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>Il pod `vpn-*` del client OpenVPN nello spazio dei nomi `kube-system` ora imposta `dnsPolicy` su `Default` per evitare il malfunzionamento del pod quando il DNS del cluster è inattivo.</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>e7182c7</td>
<td>13c7ef0</td>
<td>Immagine aggiornata per [CVE-2016-7076 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076) e [CVE-2017-1000368 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368).</td>
</tr>
</tbody>
</table>

### Changelog per 1.14.1_1516, rilasciato il 7 maggio 2019
{: #1141_1516}

La seguente tabella mostra le modifiche incluse nella patch 1.14.1_1516.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.13.5_1519">
<caption>Modifiche intervenute dalla versione 1.13.5_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.4.4</td>
<td>v3.6.1</td>
<td>Vedi le [note sulla release Calico![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.6/release-notes/).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.6</td>
<td>1.3.1</td>
<td>Vedi le [note sulla release CoreDNS ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://coredns.io/2019/01/13/coredns-1.3.1-release/). L'aggiornamento include l'aggiunta di una [porta di metriche ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://coredns.io/plugins/metrics/) sul servizio DNS del cluster. <br><br>CoreDNS è ora il solo provider DNS del cluster supportato. Se aggiorni un cluster a Kubernetes versione 1.14 da una versione precedente e hai utilizzato KubeDNS, KubeDNS viene migrato automaticamente a CoreDNS durante l'aggiornamento del cluster. Per ulteriori informazioni o per testare CoreDNS prima di eseguire l'aggiornamento, vedi [Configura il provider DNS del cluster](https://cloud.ibm.com/docs/containers?topic=containers-cluster_dns#cluster_dns).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>9ff3fda</td>
<td>ed0dafc</td>
<td>Immagine aggiornata per [CVE-2019-1543 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.5-107</td>
<td>v1.14.1-71</td>
<td>Aggiornato per supportare la release Kubernetes 1.14.1. Inoltre, la versione `calicoctl` è stata aggiornata alla 3.6.1. Gli aggiornamenti sono stati corretti ai programmi di bilanciamento del carico versione 2.0 con solo un singolo nodo di lavoro disponibile per i pod del programma di bilanciamento del carico. I programmi di bilanciamento del carico privato ora supportano l'esecuzione sui [nodi di lavoro edge privati](/docs/containers?topic=containers-edge#edge).</td>
</tr>
<tr>
<td>Politiche di sicurezza del pod IBM</td>
<td>N/D</td>
<td>N/D</td>
<td>[Le politiche di sicurezza del pod IBM](/docs/containers?topic=containers-psp#ibm_psp) sono aggiornate per supportare la funzione [RunAsGroup ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#users-and-groups) Kubernetes,</td>
</tr>
<tr>
<td>Configurazione `kubelet`</td>
<td>N/D</td>
<td>N/D</td>
<td>Imposta l'opzione `--pod-max-pids` su `14336` per evitare che un singolo pod utilizzi tutti gli ID processo su un nodo di lavoro.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.14.1</td>
<td>Vedi le [note sulla release Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.1) e il [blog di Kubernetes 1.14 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/blog/2019/03/25/kubernetes-1-14-release-announcement/).<br><br>Le politiche RBAC (role-based access control) predefinite di Kubernetes non concedono più l'accesso alle [API di rilevamento e controllo delle autorizzazioni agli utenti non autenticati ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles). Questa modifica si applica solo ai nuovi cluster versione 1.14. Se aggiorni un cluster da una versione precedente, gli utenti non autenticati hanno ancora accesso alle API di rilevamento e controllo delle autorizzazioni.</td>
</tr>
<tr>
<td>Configurazione dei controller di ammissione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td><ul>
<li>È stata aggiunta `NodeRestriction` all'opzione `--enable-admission-plugins` per il server API Kubernetes del cluster e sono state configurate le risorse cluster correlate per supportare questo miglioramento della sicurezza.</li>
<li>È stata eseguita la rimozione di `Initializers` dall'opzione `--enable-admission-plugins` e di `admissionregistration.k8s.io/v1alpha1=true` dall'opzione `--runtime-config` per il server API Kubernetes del cluster perché queste API non sono più supportate. Puoi invece utilizzare i [webhook di ammissione Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/).</li></ul></td>
</tr>
<tr>
<td>DNS autoscaler di Kubernetes</td>
<td>1.3.0</td>
<td>1.4.0</td>
<td>Vedi le [note sulla release di DNS autoscaler di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.4.0).</td>
</tr>
<tr>
<td>Configurazione dei gate delle funzioni Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td><ul>
  <li>È stata aggiunta `RuntimeClass=false` per disabilitare la selezione della configurazione di runtime del contenitore.</li>
  <li>È stata rimossa `ExperimentalCriticalPodAnnotation=true` perché l'annotazione del pod `scheduler.alpha.kubernetes.io/critical-pod` non è più supportata. Puoi invece utilizzare [priorità di pod Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/docs/containers?topic=containers-pod_priority#pod_priority).</li></ul></td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>e132aa4</td>
<td>e7182c7</td>
<td>Immagine aggiornata per [CVE-2019-11068 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

<br />


## Changelog versione 1.13
{: #113_changelog}

### Changelog per 1.13.6_1524, rilasciato il 4 giugno 2019
{: #1136_1524}

La seguente tabella mostra le modifiche incluse nella patch 1.13.6_1524.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.13.6_1522">
<caption>Modifiche intervenute dalla versione 1.13.6_1522</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione del DNS del cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato corretto un bug che poteva lasciare entrambi i pod CoreDNS e DNS Kubernetes in esecuzione dopo le operazioni di `create` o `update` del cluster.</td>
</tr>
<tr>
<td>Configurazione HA del master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione è stata aggiornata per ridurre al minimo gli errori di connettività della rete del master intermittenti durante un aggiornamento del master.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Vedi le [note sulla release etcd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Immagine aggiornata per [CVE-2018-10844 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), e [CVE-2019-5436 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Server delle metriche di Kubernetes</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>Vedi le [note sulla release del server delle metriche Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Immagine aggiornata per [CVE-2018-10844 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), e [CVE-2019-5436 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.13.6_1522, rilasciato il 20 maggio 2019
{: #1136_1522}

La seguente tabella mostra le modifiche incluse nella patch 1.13.6_1522.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.13.6_1521">
<caption>Modifiche intervenute dalla versione 1.13.6_1521</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel Ubuntu 16.04</td>
<td>4.4.0-145 generica</td>
<td>4.4.0-148 generica</td>
<td>Immagini del nodo di lavoro aggiornate con l'aggiornamento del kernel per [CVE-2018-12126 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Kernel Ubuntu 18.04</td>
<td>4.15.0-47 generica</td>
<td>4.15.0-50 generica</td>
<td>Immagini del nodo di lavoro aggiornate con l'aggiornamento del kernel per [CVE-2018-12126 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Changelog per 1.13.6_1521, rilasciato il 13 maggio 2019
{: #1136_1521}

La seguente tabella mostra le modifiche incluse nella patch 1.13.6_1521.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.13.5_1519">
<caption>Modifiche intervenute dalla versione 1.13.5_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy HA del master cluster</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Vedi le [note sulla release di HAProxy ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.haproxy.org/download/1.9/src/CHANGELOG). L'aggiornamento risolve [CVE-2019-6706 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Immagine aggiornata per [CVE-2019-1543 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.5-107</td>
<td>v1.13.6-139</td>
<td>Aggiornato per supportare la release Kubernetes 1.13.6. È stato inoltre corretto il processo di aggiornamento per il programma di bilanciamento del carico 2.0 con un solo nodo di lavoro disponibile per i pod del programma di bilanciamento del carico.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.13.6</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.6).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione della politica di controllo del server API Kubernetes è stata aggiornata per non registrare l'URL di sola lettura `/openapi/v2*`. Inoltre, la configurazione del gestore controller Kubernetes ha aumentato la durata di validità dei certificati `kubelet` firmati da 1 a 3 anni.</td>
</tr>
<tr>
<td>Configurazione del client OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>Il pod `vpn-*` del client OpenVPN nello spazio dei nomi `kube-system` ora imposta `dnsPolicy` su `Default` per evitare il malfunzionamento del pod quando il DNS del cluster è inattivo.</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Immagine aggiornata per [CVE-2016-7076 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) e [CVE-2019-11068 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.13.5_1519, rilasciato il 29 aprile 2019
{: #1135_1519}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.13.5_1519.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.13.5_1518">
<caption>Modifiche intervenute dalla versione 1.13.5_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.5</td>
<td>1.2.6</td>
<td>Vedi le [note sulla release di containerd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.2.6).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.13.5_1518, rilasciato il 15 aprile 2019
{: #1135_1518}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.13.5_1518.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.13.5_1517">
<caption>Modifiche intervenute dalla versione 1.13.5_1517</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati, incluso `systemd` per [CVE-2019-3842 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Changelog per 1.13.5_1517, rilasciato l'8 aprile 2019
{: #1135_1517}

La seguente tabella mostra le modifiche incluse nella patch 1.13.5_1517.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.13.4_1516">
<caption>Modifiche intervenute dalla versione 1.13.4_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.4.0</td>
<td>v3.4.4</td>
<td>Vedi le [note sulla release Calico![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.4/releases/#v344). L'aggiornamento risolve [CVE-2019-9946 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Proxy HA del master cluster</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Vedi le [note sulla release di HAProxy ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.haproxy.org/download/1.9/src/CHANGELOG). L'aggiornamento risolve [CVE-2018-0732 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) e [CVE-2019-1559 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.4-86</td>
<td>v1.13.5-107</td>
<td>Aggiornato per supportare le release Kubernetes 1.13.5 e Calico 3.4.4.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.4</td>
<td>v1.13.5</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.5).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Immagine aggiornata per [CVE-2017-12447 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Kernel Ubuntu 16.04</td>
<td>4.4.0-143 generica</td>
<td>4.4.0-145 generica</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2019-9213 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Kernel Ubuntu 18.04</td>
<td>4.15.0-46 generica</td>
<td>4.15.0-47 generica</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2019-9213 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.13.4_1516, rilasciato il 1° aprile 2019
{: #1134_1516}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.13.4_1516.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.13.4_1515">
<caption>Modifiche intervenute dalla versione 1.13.4_1515</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilizzo delle risorse del nodo di lavoro</td>
<td>N/D</td>
<td>N/D</td>
<td>Riserve di memoria aumentate per kubelet e containerd per impedire che questi componenti esauriscano le risorse. Per ulteriori informazioni, vedi [Riserve di risorse del nodo di lavoro](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del master 1.13.4_1515, rilasciato il 26 marzo 2019
{: #1134_1515}

La seguente tabella mostra le modifiche incluse nel fix pack del master 1.13.4_1515.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.13.4_1513">
<caption>Modifiche intervenute dalla versione 1.13.4_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione del DNS del cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato corretto il processo di aggiornamento da Kubernetes versione 1.11 per impedire che l'aggiornamento esegua la commutazione dal provider DNS del cluster a CoreDNS. Puoi ancora [configurare CoreDNS come provider DNS del cluster](/docs/containers?topic=containers-cluster_dns#set_coredns) dopo l'aggiornamento.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>345</td>
<td>346</td>
<td>Immagine aggiornata per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>166</td>
<td>167</td>
<td>Corregge gli errori `context deadline exceeded` e `timeout` intermittenti per la gestione dei segreti Kubernetes. Corregge inoltre gli aggiornamenti al KMS (key management service) che potrebbero lasciare come non crittografati i segreti Kubernetes esistenti. L'aggiornamento include una correzione per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Programma di bilanciamento del carico e monitoraggio del programma di bilanciamento del carico per il {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Immagine aggiornata per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Changelog per 1.13.4_1513, rilasciato il 20 marzo 2019
{: #1134_1513}

La seguente tabella mostra le modifiche incluse nella patch 1.13.4_1513.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.13.4_1510">
<caption>Modifiche intervenute dalla versione 1.13.4_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione del DNS del cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato corretto un bug che poteva causare il malfunzionamento di operazioni del master cluster, quali `refresh` o `update`, quando il DNS del cluster non utilizzato deve essere ridotto.</td>
</tr>
<tr>
<td>Configurazione proxy HA del master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione è stata aggiornata per gestire meglio gli errori di connessione intermittenti al master cluster.</td>
</tr>
<tr>
<td>Configurazione CoreDNS</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione di CoreDNS è stata aggiornata per supportare [più Corefile ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://coredns.io/2017/07/23/corefile-explained/) dopo l'aggiornamento della versione del cluster Kubernetes da 1.12.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.4</td>
<td>1.2.5</td>
<td>Vedi le [note sulla release di containerd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.2.5). L'aggiornamento include una correzione migliorata per [CVE-2019-5736 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Sono stati aggiornati i driver GPU per [418.43 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.nvidia.com/object/unix.html). L'aggiornamento include una correzione per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>344</td>
<td>345</td>
<td>È stato aggiunto il supporto per gli [endpoint del servizio privato](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2019-6133 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>136</td>
<td>166</td>
<td>Immagine aggiornata per [CVE-2018-16890 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) e [CVE-2019-3823 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Immagine aggiornata per [CVE-2018-10779 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) e [CVE-2019-7663 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Changelog per 1.13.4_1510, rilasciato il 4 marzo 2019
{: #1134_1510}

La seguente tabella mostra le modifiche incluse nella patch 1.13.4_1510.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.13.2_1509">
<caption>Modifiche intervenute dalla versione 1.13.2_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provider DNS del cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>Limite di memoria del pod di DNS Kubernetes e CoreDNS aumentato da `170Mi` a `400Mi` per gestire più servizi cluster.</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Immagini aggiornate per [CVE-2019-6454 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.2-62</td>
<td>v1.13.4-86</td>
<td>Aggiornato per supportare la release Kubernetes 1.13.4. Sono stati corretti i problemi di connettività periodici per i programmi di bilanciamento del carico versione 1,0 che impostano `externalTrafficPolicy` su `local`. Sono stati aggiornati gli eventi del programma di bilanciamento del carico versione 1.0 e 2.0 per utilizzare i link alla documentazione {{site.data.keyword.Bluemix_notm}} più recenti.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>342</td>
<td>344</td>
<td>È stato modificato il sistema operativo di base per l'immagine da Fedora a Alpine. Immagine aggiornata per [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>122</td>
<td>136</td>
<td>Il timeout del client è stato aumentato a {{site.data.keyword.keymanagementservicefull_notm}}. Immagine aggiornata per [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.2</td>
<td>v1.13.4</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.4). L'aggiornamento risolve [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) e [CVE-2019-1002100 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>All'opzione `--feature-gates` è stata aggiunta l'impostazione `ExperimentalCriticalPodAnnotation=true`. Questa impostazione aiuta a migrare i pod dall'annotazione `scheduler.alpha.kubernetes.io/critical-pod` dichiarata obsoleta al [supporto della priorità del pod Kubernetes](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Programma di bilanciamento del carico e monitoraggio del programma di bilanciamento del carico per il {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Immagine aggiornata per [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Client e server OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Immagine aggiornata per [CVE-2019-1559 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Immagine aggiornata per [CVE-2019-6454 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.13.2_1509, rilasciato il 27 febbraio 2019
{: #1132_1509}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.13.2_1509.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.13.2_1508">
<caption>Modifiche intervenute dalla versione 1.13.2_1508</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-19407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.13.2_1508, rilasciato il 15 febbraio 2019
{: #1132_1508}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.13.2_1508.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.13.2_1507">
<caption>Modifiche intervenute dalla versione 1.13.2_1507</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione proxy HA del master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>Il valore `spec.priorityClassName` della configurazione del pod è stato modificato in `system-node-critical` e il valore `spec.priority` è stato impostato su `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.2</td>
<td>1.2.4</td>
<td>Vedi le [note sulla release di containerd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.2.4). L'aggiornamento risolve [CVE-2019-5736 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configurazione `kubelet` di Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato abilitato il gate della funzione `ExperimentalCriticalPodAnnotation` per evitare la rimozione del pod statico critico. Imposta l'opzione `event-qps` su `0` per evitare la creazione di eventi che limitano la frequenza.</td>
</tr>
</tbody>
</table>

### Changelog per 1.13.2_1507, rilasciato il 5 febbraio 2019
{: #1132_1507}

La seguente tabella mostra le modifiche incluse nella patch 1.13.2_1507.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.4_1535">
<caption>Modifiche intervenute dalla versione 1.12.4_1535</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.4.0</td>
<td>Vedi le [note sulla release Calico![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.4/releases/#v340).</td>
</tr>
<tr>
<td>Provider DNS del cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>CoreDNS è ora il provider DNS del cluster predefinito per i nuovi cluster. Se aggiorni un cluster esistente alla 1.13 che utilizza KubeDNS come provider DNS del cluster, KubeDNS continua a essere il provider DNS del cluster. Tuttavia, puoi scegliere di [utilizzare invece CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.2.2</td>
<td>Vedi le [note sulla release di containerd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.2.2).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.2</td>
<td>1.2.6</td>
<td>Vedi le [note sulla release CoreDNS ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coredns/coredns/releases/tag/v1.2.6). Inoltre, la configurazione CoreDNS è stata aggiornata per [supportare più Corefile![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://coredns.io/2017/07/23/corefile-explained/).</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Vedi le [note sulla release etcd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coreos/etcd/releases/v3.3.11). Inoltre, le suite di cifratura supportate per etcd sono ora limitate a un sottoinsieme con crittografia molto elevata (128 bit o più).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Immagini aggiornate per [CVE-2019-3462 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) e [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.4-118</td>
<td>v1.13.2-62</td>
<td>Aggiornato per supportare la release Kubernetes 1.13.2. Inoltre, la versione `calicoctl` è stata aggiornata alla 3.4.0. Sono stati corretti gli aggiornamenti della configurazione non necessari ai programmi di bilanciamento del carico versione 2.0 sulle modifiche di stato del nodo di lavoro.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>338</td>
<td>342</td>
<td>Il plugin di archiviazione file è stato aggiornato nel seguente modo:
<ul><li>Supporta il provisioning dinamico con una [pianificazione che rileva la topologia dei volumi](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignora gli errori di eliminazione dell'attestazione del volume persistente (o PVC, persistent volume claim) se l'archiviazione è già stata eliminata.</li>
<li>Aggiunge un'annotazione di messaggio di errore alle PVC non riuscite.</li>
<li>Ottimizza le impostazioni di periodo di risincronizzazione e di designazione del leader del controller del provisioner di archiviazione e aumenta il timeout del provisioning da 30 minuti a 1 ora.</li>
<li>Controlla le autorizzazioni utente prima di avviare il provisioning.</li>
<li>Aggiunge una tolleranza `CriticalAddonsOnly` alle distribuzioni `ibm-file-plugin` e `ibm-storage-watcher` nello spazio dei nomi `kube-system`.</li></ul></td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>111</td>
<td>122</td>
<td>È stata aggiunta una logica di nuovi tentativi per evitare errori temporanei quando i segreti Kubernetes sono gestiti da {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.13.2</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.2).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione della politica di controllo del server API Kubernetes è stata aggiornata per includere la registrazione dei metadati per le richieste `cluster-admin` e la registrazione del corpo delle richieste `create`, `update` e `patch` del carico di lavoro.</td>
</tr>
<tr>
<td>DNS autoscaler di Kubernetes</td>
<td>1.2.0</td>
<td>1.3.0</td>
<td>Vedi le [note sulla release di DNS autoscaler di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.3.0).</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Immagine aggiornata per [CVE-2018-0734 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). È stata aggiunta la tolleranza `CriticalAddonsOnly` alla distribuzione `vpn` nello spazio dei nomi `kube-system`. Inoltre, la configurazione del pod viene ora ottenuta da un segreto invece che da una mappa di configurazione.</td>
</tr>
<tr>
<td>Server OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Immagine aggiornata per [CVE-2018-0734 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Patch di sicurezza per [CVE-2018-16864 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

<br />


## Changelog versione 1.12
{: #112_changelog}

Esamina il changelog della versione 1.12.
{: shortdesc}

### Changelog per 1.12.9_1555, rilasciato il 4 giugno 2019
{: #1129_1555}

La seguente tabella mostra le modifiche incluse nella patch 1.12.9_1555.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.8_1553">
<caption>Modifiche intervenute dalla versione 1.12.8_1553</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione del DNS del cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato corretto un bug che poteva lasciare entrambi i pod CoreDNS e DNS Kubernetes in esecuzione dopo le operazioni di `create` o `update` del cluster.</td>
</tr>
<tr>
<td>Configurazione HA del master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione è stata aggiornata per ridurre al minimo gli errori di connettività della rete del master intermittenti durante un aggiornamento del master.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Vedi le [note sulla release etcd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Immagine aggiornata per [CVE-2018-10844 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), e [CVE-2019-5436 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.8-210</td>
<td>v1.12.9-227</td>
<td>Aggiornato per supportare la release Kubernetes 1.12.9.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.8</td>
<td>v1.12.9</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.9).</td>
</tr>
<tr>
<td>Server delle metriche di Kubernetes</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>Vedi le [note sulla release del server delle metriche Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Immagine aggiornata per [CVE-2018-10844 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), e [CVE-2019-5436 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.12.8_1553, rilasciato il 20 maggio 2019
{: #1128_1533}

La seguente tabella mostra le modifiche incluse nella patch 1.12.8_1553.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.8_1553">
<caption>Modifiche intervenute dalla versione 1.12.8_1553</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel Ubuntu 16.04</td>
<td>4.4.0-145 generica</td>
<td>4.4.0-148 generica</td>
<td>Immagini del nodo di lavoro aggiornate con l'aggiornamento del kernel per [CVE-2018-12126 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Kernel Ubuntu 18.04</td>
<td>4.15.0-47 generica</td>
<td>4.15.0-50 generica</td>
<td>Immagini del nodo di lavoro aggiornate con l'aggiornamento del kernel per [CVE-2018-12126 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Changelog per 1.12.8_1552, rilasciato il 13 maggio 2019
{: #1128_1552}

La seguente tabella mostra le modifiche incluse nella patch 1.12.8_1552.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.7_1550">
<caption>Modifiche intervenute dalla versione 1.12.7_1550</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy HA del master cluster</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Vedi le [note sulla release di HAProxy ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.haproxy.org/download/1.9/src/CHANGELOG). L'aggiornamento risolve [CVE-2019-6706 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Immagine aggiornata per [CVE-2019-1543 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.7-180</td>
<td>v1.12.8-210</td>
<td>Aggiornato per supportare la release Kubernetes 1.12.8. È stato inoltre corretto il processo di aggiornamento per il programma di bilanciamento del carico 2.0 con un solo nodo di lavoro disponibile per i pod del programma di bilanciamento del carico.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.7</td>
<td>v1.12.8</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.8).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione della politica di controllo del server API Kubernetes è stata aggiornata per non registrare l'URL di sola lettura `/openapi/v2*`. Inoltre, la configurazione del gestore controller Kubernetes ha aumentato la durata di validità dei certificati `kubelet` firmati da 1 a 3 anni.</td>
</tr>
<tr>
<td>Configurazione del client OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>Il pod `vpn-*` del client OpenVPN nello spazio dei nomi `kube-system` ora imposta `dnsPolicy` su `Default` per evitare il malfunzionamento del pod quando il DNS del cluster è inattivo.</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Immagine aggiornata per [CVE-2016-7076 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) e [CVE-2019-11068 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.12.7_1550, rilasciato il 29 aprile 2019
{: #1127_1550}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.12.7_1550.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.7_1549">
<caption>Modifiche intervenute dalla versione 1.12.7_1549</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>Vedi le [note sulla release di containerd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.1.7).</td>
</tr>
</tbody>
</table>


### Changelog per il fix pack del nodo di lavoro 1.12.7_1549, rilasciato il 15 aprile 2019
{: #1127_1549}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.12.7_1549.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.7_1548">
<caption>Modifiche intervenute dalla versione 1.12.7_1548</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati, incluso `systemd` per [CVE-2019-3842 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Changelog per 1.12.7_1548, rilasciato l'8 aprile 2019
{: #1127_1548}

La seguente tabella mostra le modifiche incluse nella patch 1.12.7_1548.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.6_1547">
<caption>Modifiche intervenute dalla versione 1.12.6_1547</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>Vedi le [note sulla release Calico![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.3/releases/#v336). L'aggiornamento risolve [CVE-2019-9946 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Proxy HA del master cluster</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Vedi le [note sulla release di HAProxy ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.haproxy.org/download/1.9/src/CHANGELOG). L'aggiornamento risolve [CVE-2018-0732 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) e [CVE-2019-1559 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.6-157</td>
<td>v1.12.7-180</td>
<td>Aggiornato per supportare le release Kubernetes 1.12.7 e Calico 3.3.6.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.6</td>
<td>v1.12.7</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.7).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Immagine aggiornata per [CVE-2017-12447 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Kernel Ubuntu 16.04</td>
<td>4.4.0-143 generica</td>
<td>4.4.0-145 generica</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2019-9213 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Kernel Ubuntu 18.04</td>
<td>4.15.0-46 generica</td>
<td>4.15.0-47 generica</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2019-9213 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.12.6_1547, rilasciato il 1° aprile 2019
{: #1126_1547}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.12.6_1547.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.6_1546">
<caption>Modifiche intervenute dalla versione 1.12.6_1546</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilizzo delle risorse del nodo di lavoro</td>
<td>N/D</td>
<td>N/D</td>
<td>Riserve di memoria aumentate per kubelet e containerd per impedire che questi componenti esauriscano le risorse. Per ulteriori informazioni, vedi [Riserve di risorse del nodo di lavoro](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>


### Changelog per il fix pack del master 1.12.6_1546, rilasciato il 26 marzo 2019
{: #1126_1546}

La seguente tabella mostra le modifiche incluse nel fix pack del master 1.12.6_1546.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.6_1544">
<caption>Modifiche intervenute dalla versione 1.12.6_1544</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>345</td>
<td>346</td>
<td>Immagine aggiornata per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>166</td>
<td>167</td>
<td>Corregge gli errori `context deadline exceeded` e `timeout` intermittenti per la gestione dei segreti Kubernetes. Corregge inoltre gli aggiornamenti al KMS (key management service) che potrebbero lasciare come non crittografati i segreti Kubernetes esistenti. L'aggiornamento include una correzione per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Programma di bilanciamento del carico e monitoraggio del programma di bilanciamento del carico per il {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Immagine aggiornata per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Changelog per 1.12.6_1544, rilasciato il 20 marzo 2019
{: #1126_1544}

La seguente tabella mostra le modifiche incluse nella patch 1.12.6_1544.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.6_1541">
<caption>Modifiche intervenute dalla versione 1.12.6_1541</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione del DNS del cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato corretto un bug che poteva causare il malfunzionamento di operazioni del master cluster, quali `refresh` o `update`, quando il DNS del cluster non utilizzato deve essere ridotto.</td>
</tr>
<tr>
<td>Configurazione proxy HA del master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione è stata aggiornata per gestire meglio gli errori di connessione intermittenti al master cluster.</td>
</tr>
<tr>
<td>Configurazione CoreDNS</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione di CoreDNS è stata aggiornata per supportare [più Corefile ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://coredns.io/2017/07/23/corefile-explained/).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Sono stati aggiornati i driver GPU per [418.43 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.nvidia.com/object/unix.html). L'aggiornamento include una correzione per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>344</td>
<td>345</td>
<td>È stato aggiunto il supporto per gli [endpoint del servizio privato](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2019-6133 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>136</td>
<td>166</td>
<td>Immagine aggiornata per [CVE-2018-16890 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) e [CVE-2019-3823 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Immagine aggiornata per [CVE-2018-10779 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) e [CVE-2019-7663 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Changelog per 1.12.6_1541, rilasciato il 4 marzo 2019
{: #1126_1541}

La seguente tabella mostra le modifiche incluse nella patch 1.12.6_1541.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.5_1540">
<caption>Modifiche intervenute dalla versione 1.12.5_1540</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provider DNS del cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>Limite di memoria del pod di DNS Kubernetes e CoreDNS aumentato da `170Mi` a `400Mi` per gestire più servizi cluster.</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Immagini aggiornate per [CVE-2019-6454 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.5-137</td>
<td>v1.12.6-157</td>
<td>Aggiornato per supportare la release Kubernetes 1.12.6. Sono stati corretti i problemi di connettività periodici per i programmi di bilanciamento del carico versione 1,0 che impostano `externalTrafficPolicy` su `local`. Sono stati aggiornati gli eventi del programma di bilanciamento del carico versione 1.0 e 2.0 per utilizzare i link alla documentazione {{site.data.keyword.Bluemix_notm}} più recenti.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>342</td>
<td>344</td>
<td>È stato modificato il sistema operativo di base per l'immagine da Fedora a Alpine. Immagine aggiornata per [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>122</td>
<td>136</td>
<td>Il timeout del client è stato aumentato a {{site.data.keyword.keymanagementservicefull_notm}}. Immagine aggiornata per [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.5</td>
<td>v1.12.6</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.6). L'aggiornamento risolve [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) e [CVE-2019-1002100 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>All'opzione `--feature-gates` è stata aggiunta l'impostazione `ExperimentalCriticalPodAnnotation=true`. Questa impostazione aiuta a migrare i pod dall'annotazione `scheduler.alpha.kubernetes.io/critical-pod` dichiarata obsoleta al [supporto della priorità del pod Kubernetes](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Programma di bilanciamento del carico e monitoraggio del programma di bilanciamento del carico per il {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Immagine aggiornata per [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Client e server OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Immagine aggiornata per [CVE-2019-1559 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Immagine aggiornata per [CVE-2019-6454 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.12.5_1540, rilasciato il 27 febbraio 2019
{: #1125_1540}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.12.5_1540.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.5_1538">
<caption>Modifiche intervenute dalla versione 1.12.5_1538</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-19407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.12.5_1538, rilasciato il 15 febbraio 2019
{: #1125_1538}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.12.5_1538.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.5_1537">
<caption>Modifiche intervenute dalla versione 1.12.5_1537</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione proxy HA del master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>Il valore `spec.priorityClassName` della configurazione del pod è stato modificato in `system-node-critical` e il valore `spec.priority` è stato impostato su `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>Vedi le [note sulla release di containerd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.1.6). L'aggiornamento risolve [CVE-2019-5736 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configurazione `kubelet` di Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato abilitato il gate della funzione `ExperimentalCriticalPodAnnotation` per evitare la rimozione del pod statico critico.</td>
</tr>
</tbody>
</table>

### Changelog per 1.12.5_1537, rilasciato il 5 febbraio 2019
{: #1125_1537}

La seguente tabella mostra le modifiche incluse nella patch 1.12.5_1537.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.4_1535">
<caption>Modifiche intervenute dalla versione 1.12.4_1535</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Vedi le [note sulla release etcd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coreos/etcd/releases/v3.3.11). Inoltre, le suite di cifratura supportate per etcd sono ora limitate a un sottoinsieme con crittografia molto elevata (128 bit o più).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Immagini aggiornate per [CVE-2019-3462 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) e [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.4-118</td>
<td>v1.12.5-137</td>
<td>Aggiornato per supportare la release Kubernetes 1.12.5. Inoltre, la versione `calicoctl` è stata aggiornata alla 3.3.1. Sono stati corretti gli aggiornamenti della configurazione non necessari ai programmi di bilanciamento del carico versione 2.0 sulle modifiche di stato del nodo di lavoro.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>338</td>
<td>342</td>
<td>Il plugin di archiviazione file è stato aggiornato nel seguente modo:
<ul><li>Supporta il provisioning dinamico con una [pianificazione che rileva la topologia dei volumi](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignora gli errori di eliminazione dell'attestazione del volume persistente (o PVC, persistent volume claim) se l'archiviazione è già stata eliminata.</li>
<li>Aggiunge un'annotazione di messaggio di errore alle PVC non riuscite.</li>
<li>Ottimizza le impostazioni di periodo di risincronizzazione e di designazione del leader del controller del provisioner di archiviazione e aumenta il timeout del provisioning da 30 minuti a 1 ora.</li>
<li>Controlla le autorizzazioni utente prima di avviare il provisioning.</li></ul></td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>111</td>
<td>122</td>
<td>È stata aggiunta una logica di nuovi tentativi per evitare errori temporanei quando i segreti Kubernetes sono gestiti da {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.12.5</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.5).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione della politica di controllo del server API Kubernetes è stata aggiornata per includere la registrazione dei metadati per le richieste `cluster-admin` e la registrazione del corpo delle richieste `create`, `update` e `patch` del carico di lavoro.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Immagine aggiornata per [CVE-2018-0734 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Inoltre, la configurazione del pod viene ora ottenuta da un segreto invece che da una mappa di configurazione.</td>
</tr>
<tr>
<td>Server OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Immagine aggiornata per [CVE-2018-0734 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Patch di sicurezza per [CVE-2018-16864 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.12.4_1535, rilasciato il 28 gennaio 2019
{: #1124_1535}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.12.4_1535.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.4_1534">
<caption>Modifiche intervenute dalla versione 1.12.4_1534</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati, incluso `apt` per [CVE-2019-3462 ![icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) e [USN-3863-1 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>


### Changelog per 1.12.4_1534, rilasciato il 21 gennaio 2019
{: #1124_1534}

La seguente tabella mostra le modifiche incluse nella patch 1.12.3_1534.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.3_1533">
<caption>Modifiche intervenute dalla versione 1.12.3_1533</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.3-91</td>
<td>v1.12.4-118</td>
<td>Aggiornato per supportare la release Kubernetes 1.12.4.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.3</td>
<td>v1.12.4</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.4).</td>
</tr>
<tr>
<td>Add-On Resizer di Kubernetes</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Vedi le [note sulla release dell'Add-On Resizer di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Dashboard Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>Vedi le [note sulla release del dashboard Kubernetes
![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). L'aggiornamento risolve [CVE-2018-18264 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).</td>
</tr>
<tr>
<td>Programma di installazione GPU</td>
<td>390.12</td>
<td>410.79</td>
<td>I driver GPU installati sono stati aggiornati alla 410.79.</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.12.3_1533, rilasciato il 7 gennaio 2019
{: #1123_1533}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.12.3_1533.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.3_1532">
<caption>Modifiche intervenute dalla versione 1.12.3_1532</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2017-5753, CVE-2018-18690 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.12.3_1532, rilasciato il 17 dicembre 2018
{: #1123_1532}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.12.2_1532.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.3_1531">
<caption>Modifiche intervenute dalla versione 1.12.3_1531</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati.</td>
</tr>
</tbody>
</table>


### Changelog per 1.12.3_1531, rilasciato il 5 dicembre 2018
{: #1123_1531}

La seguente tabella mostra le modifiche incluse nella patch 1.12.3_1531.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.2_1530">
<caption>Modifiche intervenute dalla versione 1.12.2_1530</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.2-68</td>
<td>v1.12.3-91</td>
<td>Aggiornato per supportare la release Kubernetes 1.12.3.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.2</td>
<td>v1.12.3</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.3). L'aggiornamento risolve [CVE-2018-1002105 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.12.2_1530, rilasciato il 4 dicembre 2018
{: #1122_1530}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.12.2_1530.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.2_1529">
<caption>Modifiche intervenute dalla versione 1.12.2_1529</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilizzo delle risorse del nodo di lavoro</td>
<td>N/D</td>
<td>N/D</td>
<td>Sono stati aggiunti dei gruppi di controllo (cgroup) dedicati per kubelet e containerd per impedire che questi componenti esauriscano le risorse. Per ulteriori informazioni, vedi [Riserve di risorse del nodo di lavoro](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>



### Changelog per 1.12.2_1529, rilasciato il 27 novembre 2018
{: #1122_1529}

La seguente tabella mostra le modifiche incluse nella patch 1.12.2_1529.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.2_1528">
<caption>Modifiche intervenute dalla versione 1.12.2_1528</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>Vedi le [note sulla release Calico![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.3/releases/#v331). L'aggiornamento risolve [Tigera Technical Advisory TTA-2018-001 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Configurazione del DNS del cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato corretto un bug che poteva comportare l'esecuzione dei pod di DNS Kubernetes e di CoreDNS dopo le operazioni di creazione o aggiornamento del cluster.</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.2.0</td>
<td>v1.1.5</td>
<td>Vedi le [note sulla release di containerd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.1.5). Containerd è stato aggiornato per correggere un deadlock che può [impedire ai pod di terminare ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/issues/2744).</td>
</tr>
<tr>
<td>Client e server OpenVPN</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Immagine aggiornata per [CVE-2018-0732 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) e [CVE-2018-0737 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.12.2_1528, rilasciato il 19 novembre 2018
{: #1122_1528}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.12.2_1528.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.12.2_1527">
<caption>Modifiche intervenute dalla versione 1.12.2_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-7755 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>


### Changelog per 1.12.2_1527, rilasciato il 7 novembre 2018
{: #1122_1527}

La seguente tabella mostra le modifiche incluse nella patch 1.12.2_1527.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.3_1533">
<caption>Modifiche intervenute dalla versione 1.11.3_1533</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione Calico</td>
<td>N/D</td>
<td>N/D</td>
<td>I pod di Calico `calico-*` nello spazio dei nomi `kube-system` ora impostano le richieste di risorse di CPU e memoria per tutti i contenitori.</td>
</tr>
<tr>
<td>Provider DNS del cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>DNS Kubernetes (KubeDNS) rimane il provider DNS predefinito del cluster. Tuttavia, ora hai la possibilità di [modificare il provider DNS del cluster su CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>Provider di metriche del cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>Il server delle metriche di Kubernetes sostituisce Kubernetes Heapster (obsoleto a partire da Kubernetes versione 1.8) come provider di metriche del cluster. Per gli elementi di azione, vedi [l'azione di preparazione di `metrics-server`](/docs/containers?topic=containers-cs_versions#metrics-server).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.4</td>
<td>1.2.0</td>
<td>Vedi le [note sulla release di containerd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.2.0).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>N/D</td>
<td>1.2.2</td>
<td>Vedi le [note sulla release CoreDNS ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coredns/coredns/releases/tag/v1.2.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.12.2</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.2).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Sono state aggiunte tre nuove politiche di sicurezza per i pod IBM e i relativi ruoli cluster associati. Per ulteriori informazioni, vedi [Descrizione delle risorse predefinite per la gestione cluster IBM](/docs/containers?topic=containers-psp#ibm_psp).</td>
</tr>
<tr>
<td>Dashboard Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.0</td>
<td>Vedi le [note sulla release del dashboard Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.0).<br><br>
Se accedi al dashboard tramite `kubectl proxy`, il pulsante **SKIP** nella pagina di accesso viene rimosso. [Utilizza invece un **Token** per eseguire l'accesso](/docs/containers?topic=containers-app#cli_dashboard). Inoltre, puoi ora aumentare il numero di pod del dashboard Kubernetes eseguendo `kubectl -n kube-system scale deploy kubernetes-dashboard --replicas=3`.</td>
</tr>
<tr>
<td>DNS Kubernetes</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>Vedi le [note sulla release di DNS Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Server delle metriche di Kubernetes</td>
<td>N/D</td>
<td>v0.3.1</td>
<td>Vedi le [note sulla release del server delle metriche Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.1).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-118</td>
<td>v1.12.2-68</td>
<td>Aggiornato per supportare la release Kubernetes 1.12. Ulteriori modifiche includono quanto segue:
<ul><li>I pod del programma di bilanciamento del carico (`ibm-cloud-provider-ip-*` nello spazio dei nomi `ibm-system`) ora impostano le richieste di risorse di CPU e memoria.</li>
<li>È stata aggiunta l'annotazione `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` per specificare la VLAN in cui viene distribuito il servizio del programma di bilanciamento del carico. Per visualizzare le VLAN disponibili nel tuo cluster, esegui `ibmcloud ks vlans --zone <zone>`.</li>
<li>È disponibile un nuovo [programma di bilanciamento del carico 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) come versione beta.</li></ul></td>
</tr>
<tr>
<td>Configurazione del client OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>Il client OpenVPN `vpn-* pod` nello spazio dei nomi `kube-system` ora imposta le richieste di risorse di CPU e memoria.</td>
</tr>
</tbody>
</table>

## Obsoleto: Changelog versione 1.11
{: #111_changelog}

Esamina il changelog della versione 1.11.
{: shortdesc}

Kubernetes versione 1.11 è stato dichiarato obsoleto e non sarà più supportato a partire dal 27 giugno 2019 (data ancora non definitiva). [Esamina il potenziale impatto](/docs/containers?topic=containers-cs_versions#cs_versions) di ciascun aggiornamento della versione di Kubernetes e [aggiorna quindi i tuoi cluster](/docs/containers?topic=containers-update#update) immediatamente almeno alla 1.12.
{: deprecated}

### Changelog per 1.11.10_1561, rilasciato il 4 giugno 2019
{: #11110_1561}

La seguente tabella mostra le modifiche incluse nella patch 1.11.10_1561.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.10_1559">
<caption>Modifiche intervenute dalla versione 1.11.10_1559</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione HA del master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione è stata aggiornata per ridurre al minimo gli errori di connettività della rete del master intermittenti durante un aggiornamento del master.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Vedi le [note sulla release etcd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Immagine aggiornata per [CVE-2018-10844 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), e [CVE-2019-5436 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Immagine aggiornata per [CVE-2018-10844 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), e [CVE-2019-5436 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.11.10_1559, rilasciato il 20 maggio 2019
{: #11110_1559}

La seguente tabella mostra le modifiche incluse nel patch pack 1.11.10_1559.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.10_1558">
<caption>Modifiche intervenute dalla versione 1.11.10_1558</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel Ubuntu 16.04</td>
<td>4.4.0-145 generica</td>
<td>4.4.0-148 generica</td>
<td>Immagini del nodo di lavoro aggiornate con l'aggiornamento del kernel per [CVE-2018-12126 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Kernel Ubuntu 18.04</td>
<td>4.15.0-47 generica</td>
<td>4.15.0-50 generica</td>
<td>Immagini del nodo di lavoro aggiornate con l'aggiornamento del kernel per [CVE-2018-12126 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Changelog per 1.11.10_1558, rilasciato il 13 maggio 2019
{: #11110_1558}

La seguente tabella mostra le modifiche incluse nella patch 1.11.10_1558.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.9_1556">
<caption>Modifiche intervenute dalla versione 1.11.9_1556</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy HA del master cluster</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Vedi le [note sulla release di HAProxy ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.haproxy.org/download/1.9/src/CHANGELOG). L'aggiornamento risolve [CVE-2019-6706 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Immagine aggiornata per [CVE-2019-1543 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.9-241</td>
<td>v1.11.10-270</td>
<td>Aggiornato per supportare la release Kubernetes 1.11.10.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.9</td>
<td>v1.11.10</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.10).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione della politica di controllo del server API Kubernetes è stata aggiornata per non registrare l'URL di sola lettura `/openapi/v2*`. Inoltre, la configurazione del gestore controller Kubernetes ha aumentato la durata di validità dei certificati `kubelet` firmati da 1 a 3 anni.</td>
</tr>
<tr>
<td>Configurazione del client OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>Il pod `vpn-*` del client OpenVPN nello spazio dei nomi `kube-system` ora imposta `dnsPolicy` su `Default` per evitare il malfunzionamento del pod quando il DNS del cluster è inattivo.</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Immagine aggiornata per [CVE-2016-7076 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) e [CVE-2019-11068 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.11.9_1556, rilasciato il 29 aprile 2019
{: #1119_1556}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.11.9_1556.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.9_1555">
<caption>Modifiche intervenute dalla versione 1.11.9_1555</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>Vedi le [note sulla release di containerd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.1.7).</td>
</tr>
</tbody>
</table>


### Changelog per il fix pack del nodo di lavoro 1.11.9_1555, rilasciato il 15 aprile 2019
{: #1119_1555}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.11.9_1555.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.9_1554">
<caption>Modifiche intervenute dalla 1.11.9_1554</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati, incluso `systemd` per [CVE-2019-3842 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Changelog per 1.11.9_1554, rilasciato l'8 aprile 2019
{: #1119_1554}

La seguente tabella mostra le modifiche incluse nella patch 1.11.9_1554.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.8_1553">
<caption>Modifiche intervenute dalla versione 1.11.8_1553</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>Vedi le [note sulla release Calico![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.3/releases/#v336). L'aggiornamento risolve [CVE-2019-9946 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Proxy HA del master cluster</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Vedi le [note sulla release di HAProxy ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.haproxy.org/download/1.9/src/CHANGELOG). L'aggiornamento risolve [CVE-2018-0732 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) e [CVE-2019-1559 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.8-219</td>
<td>v1.11.9-241</td>
<td>Aggiornato per supportare le release Kubernetes 1.11.9 e Calico 3.3.6.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.8</td>
<td>v1.11.9</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.9).</td>
</tr>
<tr>
<td>DNS Kubernetes</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>Vedi le [note sulla release di DNS Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Immagine aggiornata per [CVE-2017-12447 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Kernel Ubuntu 16.04</td>
<td>4.4.0-143 generica</td>
<td>4.4.0-145 generica</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2019-9213 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Kernel Ubuntu 18.04</td>
<td>4.15.0-46 generica</td>
<td>4.15.0-47 generica</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2019-9213 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.11.8_1553, rilasciato il 1° aprile 2019
{: #1118_1553}

La seguente tabella mostra le modifiche include nella correzione del nodo di lavoro 1.11.8_1553.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.8_1552">
<caption>Modifiche intervenute dalla versione 1.11.8_1552</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilizzo delle risorse del nodo di lavoro</td>
<td>N/D</td>
<td>N/D</td>
<td>Riserve di memoria aumentate per kubelet e containerd per impedire che questi componenti esauriscano le risorse. Per ulteriori informazioni, vedi [Riserve di risorse del nodo di lavoro](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del master 1.11.8_1552, rilasciato il 26 marzo 2019
{: #1118_1552}

La seguente tabella mostra le modifiche incluse nel fix pack del master 1.11.8_1552.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.8_1550">
<caption>Modifiche intervenute dalla versione 1.11.8_1550</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>345</td>
<td>346</td>
<td>Immagine aggiornata per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>166</td>
<td>167</td>
<td>Corregge gli errori `context deadline exceeded` e `timeout` intermittenti per la gestione dei segreti Kubernetes. Corregge inoltre gli aggiornamenti al KMS (key management service) che potrebbero lasciare come non crittografati i segreti Kubernetes esistenti. L'aggiornamento include una correzione per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Programma di bilanciamento del carico e monitoraggio del programma di bilanciamento del carico per il {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Immagine aggiornata per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Changelog per 1.11.8_1550, rilasciato il 20 marzo 2019
{: #1118_1550}

La seguente tabella mostra le modifiche incluse nella patch 1.11.8_1550.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.8_1547">
<caption>Modifiche intervenute dalla versione 1.11.8_1547</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione proxy HA del master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione è stata aggiornata per gestire meglio gli errori di connessione intermittenti al master cluster.</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Sono stati aggiornati i driver GPU per [418.43 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.nvidia.com/object/unix.html). L'aggiornamento include una correzione per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>344</td>
<td>345</td>
<td>È stato aggiunto il supporto per gli [endpoint del servizio privato](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2019-6133 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>136</td>
<td>166</td>
<td>Immagine aggiornata per [CVE-2018-16890 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) e [CVE-2019-3823 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Immagine aggiornata per [CVE-2018-10779 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) e [CVE-2019-7663 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Changelog per 1.11.8_1547, rilasciato il 4 marzo 2019
{: #1118_1547}

La seguente tabella mostra le modifiche incluse nella patch 1.11.8_1547.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.7_1546">
<caption>Modifiche intervenute dalla versione 1.11.7_1546</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Immagini aggiornate per [CVE-2019-6454 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.7-198</td>
<td>v1.11.8-219</td>
<td>Aggiornato per supportare la release Kubernetes 1.11.8. Sono stati corretti i problemi di connettività periodici per i programmi di bilanciamento del carico che impostano `externalTrafficPolicy` su `local`. Sono stati aggiornati gli eventi del programma di bilanciamento del carico per utilizzare i link alla documentazione {{site.data.keyword.Bluemix_notm}} più recenti.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>342</td>
<td>344</td>
<td>È stato modificato il sistema operativo di base per l'immagine da Fedora a Alpine. Immagine aggiornata per [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>122</td>
<td>136</td>
<td>Il timeout del client è stato aumentato a {{site.data.keyword.keymanagementservicefull_notm}}. Immagine aggiornata per [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.7</td>
<td>v1.11.8</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.8). L'aggiornamento risolve [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) e [CVE-2019-1002100 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>All'opzione `--feature-gates` è stata aggiunta l'impostazione `ExperimentalCriticalPodAnnotation=true`. Questa impostazione aiuta a migrare i pod dall'annotazione `scheduler.alpha.kubernetes.io/critical-pod` dichiarata obsoleta al [supporto della priorità del pod Kubernetes](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>DNS Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Limite di memoria del pod di DNS Kubernetes aumentato da `170Mi` a `400Mi` per gestire più servizi cluster.</td>
</tr>
<tr>
<td>Programma di bilanciamento del carico e monitoraggio del programma di bilanciamento del carico per il {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Immagine aggiornata per [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Client e server OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Immagine aggiornata per [CVE-2019-1559 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Immagine aggiornata per [CVE-2019-6454 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.11.7_1546, rilasciato il 27 febbraio 2019
{: #1117_1546}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.11.7_1546.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.7_1546">
<caption>Modifiche intervenute dalla versione 1.11.7_1546</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-19407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.11.7_1544, rilasciato il 15 febbraio 2019
{: #1117_1544}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.11.7_1544.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.7_1543">
<caption>Modifiche intervenute dalla versione 1.11.7_1543</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione proxy HA del master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>Il valore `spec.priorityClassName` della configurazione del pod è stato modificato in `system-node-critical` e il valore `spec.priority` è stato impostato su `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>Vedi le [note sulla release di containerd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.1.6). L'aggiornamento risolve [CVE-2019-5736 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configurazione `kubelet` di Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato abilitato il gate della funzione `ExperimentalCriticalPodAnnotation` per evitare la rimozione del pod statico critico.</td>
</tr>
</tbody>
</table>

### Changelog per 1.11.7_1543, rilasciato il 5 febbraio 2019
{: #1117_1543}

La seguente tabella mostra le modifiche incluse nella patch 1.11.7_1543.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.6_1541">
<caption>Modifiche intervenute dalla versione 1.11.6_1541</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Vedi le [note sulla release etcd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coreos/etcd/releases/v3.3.11). Inoltre, le suite di cifratura supportate per etcd sono ora limitate a un sottoinsieme con crittografia molto elevata (128 bit o più).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Immagini aggiornate per [CVE-2019-3462 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) e [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.6-180</td>
<td>v1.11.7-198</td>
<td>Aggiornato per supportare la release Kubernetes 1.11.7. Inoltre, la versione `calicoctl` è stata aggiornata alla 3.3.1. Sono stati corretti gli aggiornamenti della configurazione non necessari ai programmi di bilanciamento del carico versione 2.0 sulle modifiche di stato del nodo di lavoro.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>338</td>
<td>342</td>
<td>Il plugin di archiviazione file è stato aggiornato nel seguente modo:
<ul><li>Supporta il provisioning dinamico con una [pianificazione che rileva la topologia dei volumi](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignora gli errori di eliminazione dell'attestazione del volume persistente (o PVC, persistent volume claim) se l'archiviazione è già stata eliminata.</li>
<li>Aggiunge un'annotazione di messaggio di errore alle PVC non riuscite.</li>
<li>Ottimizza le impostazioni di periodo di risincronizzazione e di designazione del leader del controller del provisioner di archiviazione e aumenta il timeout del provisioning da 30 minuti a 1 ora.</li>
<li>Controlla le autorizzazioni utente prima di avviare il provisioning.</li></ul></td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>111</td>
<td>122</td>
<td>È stata aggiunta una logica di nuovi tentativi per evitare errori temporanei quando i segreti Kubernetes sono gestiti da {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.6</td>
<td>v1.11.7</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.7).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione della politica di controllo del server API Kubernetes è stata aggiornata per includere la registrazione dei metadati per le richieste `cluster-admin` e la registrazione del corpo delle richieste `create`, `update` e `patch` del carico di lavoro.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Immagine aggiornata per [CVE-2018-0734 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Inoltre, la configurazione del pod viene ora ottenuta da un segreto invece che da una mappa di configurazione.</td>
</tr>
<tr>
<td>Server OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Immagine aggiornata per [CVE-2018-0734 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Patch di sicurezza per [CVE-2018-16864 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.11.6_1541, rilasciato il 28 gennaio 2019
{: #1116_1541}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.11.6_1541.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.6_1540">
<caption>Modifiche intervenute dalla versione 1.11.6_1540</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati, incluso `apt` per [CVE-2019-3462 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) / [USN-3863-1 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>

### Changelog per 1.11.6_1540, rilasciato il 21 gennaio 2019
{: #1116_1540}

La seguente tabella mostra le modifiche incluse nella patch 1.11.6_1540.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.5_1539">
<caption>Modifiche intervenute dalla versione 1.11.5_1539</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.5-152</td>
<td>v1.11.6-180</td>
<td>Aggiornato per supportare la release Kubernetes 1.11.6.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.5</td>
<td>v1.11.6</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.6).</td>
</tr>
<tr>
<td>Add-On Resizer di Kubernetes</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Vedi le [note sulla release dell'Add-On Resizer di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Dashboard Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>Vedi le [note sulla release del dashboard Kubernetes
![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). L'aggiornamento risolve [CVE-2018-18264 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).<br><br>Se accedi al dashboard tramite kubectl proxy, il pulsante SKIP nella pagina di accesso viene rimosso. [Utilizza invece un Token per eseguire l'accesso](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td>Programma di installazione GPU</td>
<td>390.12</td>
<td>410.79</td>
<td>I driver GPU installati sono stati aggiornati alla 410.79.</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.11.5_1539, rilasciato il 7 gennaio 2019
{: #1115_1539}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.11.5_1539.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.5_1538">
<caption>Modifiche intervenute dalla versione 1.11.5_1538</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2017-5753, CVE-2018-18690 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.11.5_1538, rilasciato il 17 dicembre 2018
{: #1115_1538}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.11.5_1538.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.5_1537">
<caption>Modifiche intervenute dalla versione 1.11.5_1537</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati.</td>
</tr>
</tbody>
</table>

### Changelog per 1.11.5_1537, rilasciato il 5 dicembre 2018
{: #1115_1537}

La seguente tabella mostra le modifiche incluse nella patch 1.11.5_1537.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.4_1536">
<caption>Modifiche intervenute dalla versione 1.11.4_1536</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.4-142</td>
<td>v1.11.5-152</td>
<td>Aggiornato per supportare la release Kubernetes 1.11.5.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.4</td>
<td>v1.11.5</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.5). L'aggiornamento risolve [CVE-2018-1002105 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.11.4_1536, rilasciato il 4 dicembre 2018
{: #1114_1536}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.11.4_1536.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.4_1535">
<caption>Modifiche intervenute dalla versione 1.11.4_1535</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilizzo delle risorse del nodo di lavoro</td>
<td>N/D</td>
<td>N/D</td>
<td>Sono stati aggiunti dei gruppi di controllo (cgroup) dedicati per kubelet e containerd per impedire che questi componenti esauriscano le risorse. Per ulteriori informazioni, vedi [Riserve di risorse del nodo di lavoro](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Changelog per 1.11.4_1535, rilasciato il 27 novembre 2018
{: #1114_1535}

La seguente tabella mostra le modifiche incluse nella patch 1.11.4_1535.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.3_1534">
<caption>Modifiche intervenute dalla versione 1.11.3_1534</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>Vedi le [note sulla release Calico![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.3/releases/#v331). L'aggiornamento risolve [Tigera Technical Advisory TTA-2018-001 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.1.4</td>
<td>v1.1.5</td>
<td>Vedi le [note sulla release di containerd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.1.5). Containerd è stato aggiornato per correggere un deadlock che può [impedire ai pod di terminare ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/issues/2744).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-127</td>
<td>v1.11.4-142</td>
<td>Aggiornato per supportare la release Kubernetes 1.11.4.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.11.4</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.4).</td>
</tr>
<tr>
<td>Client e server OpenVPN</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Immagine aggiornata per [CVE-2018-0732 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) e [CVE-2018-0737 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.11.3_1534, rilasciato il 19 novembre 2018
{: #1113_1534}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.11.3_1534.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.3_1533">
<caption>Modifiche intervenute dalla versione 1.11.3_1533</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-7755 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>


### Changelog per 1.11.3_1533, rilasciato il 7 novembre 2018
{: #1113_1533}

La seguente tabella mostra le modifiche incluse nella patch 1.11.3_1533.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.3_1531">
<caption>Modifiche intervenute dalla versione 1.11.3_1531</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Aggiornamento HA del master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato corretto l'aggiornamento ai master altamente disponibili (HA) per i cluster che utilizzano i webhook di ammissione come `initializerconfigurations`, `mutatingwebhookconfigurations` o `validatingwebhookconfigurations`. Potresti utilizzare questi webhook con i grafici Helm, ad esempio per [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-100</td>
<td>v1.11.3-127</td>
<td>È stata aggiunta l'annotazione `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` per specificare la VLAN in cui viene distribuito il servizio del programma di bilanciamento del carico. Per visualizzare le VLAN disponibili nel tuo cluster, esegui `ibmcloud ks vlans --zone <zone>`.</td>
</tr>
<tr>
<td>Kernel abilitato a TPM</td>
<td>N/D</td>
<td>N/D</td>
<td>I nodi di lavoro bare metal con chip TPM per Trusted Compute utilizzano il kernel Ubuntu predefinito fino a quando non viene abilitata l'attendibilità. Se [abiliti l'attendibilità](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) su un cluster esistente, devi [ricaricare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) tutti i nodi di lavoro bare metal esistenti con i chip TPM. Per verificare se un nodo di lavoro bare metal ha un chip TPM, controlla il campo **Trustable** dopo l'esecuzione del [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-types --zone`.</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del master 1.11.3_1531, rilasciato l'1 novembre 2018
{: #1113_1531_ha-master}

La seguente tabella mostra le modifiche incluse nel fix pack del master 1.11.3_1531.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.3_1527">
<caption>Modifiche intervenute dalla versione 1.11.3_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>È stata aggiornata la configurazione del master cluster per aumentare l'alta disponibilità (HA). I cluster ora dispongono di tre repliche del master Kubernetes che vengono impostate con una configurazione altamente disponibile (HA), con ciascun master distribuito su host fisici separati. Inoltre, se il tuo cluster si trova in una zona che supporta il multizona, i master vengono estesi tra le zone.<br>Per le azioni che devi eseguire, vedi [Aggiornamento a master cluster altamente disponibili](/docs/containers?topic=containers-cs_versions#ha-masters). Queste azioni di preparazione si applicano:<ul>
<li>Se hai un firewall o politiche di rete Calico personalizzate.</li>
<li>Se utilizzi le porte host `2040` o `2041` sui tuoi nodi di lavoro.</li>
<li>Se hai utilizzato l'indirizzo IP del master cluster per l'accesso in cluster al master.</li>
<li>Se disponi dell'automazione che richiama l'API o la CLI Calico (`calicoctl`), ad esempio per creare le politiche Calico.</li>
<li>Se utilizzi le politiche di rete di Kubernetes o Calico per controllare l'accesso in uscita dei pod al master.</li></ul></td>
</tr>
<tr>
<td>Proxy HA del master cluster</td>
<td>N/D</td>
<td>1.8.12-alpine</td>
<td>È stato aggiunto un pod `ibm-master-proxy-*` per il bilanciamento del carico lato client su tutti i nodi di lavoro, in modo che ogni client del nodo di lavoro possa instradare le richieste a una replica del master HA disponibile.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>Vedi le [note sulla release etcd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coreos/etcd/releases/v3.3.1).</td>
</tr>
<tr>
<td>Crittografia dei dati in etcd</td>
<td>N/D</td>
<td>N/D</td>
<td>In precedenza, i dati etcd erano archiviati sull'istanza di archiviazione file NFS del master che veniva crittografata quando inattiva. Ora, i dati etcd vengono archiviati sul disco locale del master e sottoposti a backup in {{site.data.keyword.cos_full_notm}}. I dati vengono crittografati durante il transito su {{site.data.keyword.cos_full_notm}} e quando inattivi. Tuttavia, i dati etcd sul disco locale del master non vengono crittografati. Se vuoi che i dati etcd locali del master vengano crittografati, [abilita {{site.data.keyword.keymanagementservicelong_notm}} nel tuo cluster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.11.3_1531, rilasciato il 26 ottobre 2018
{: #1113_1531}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.11.3_1531.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.3_1525">
<caption>Modifiche intervenute dalla versione 1.11.3_1525</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Gestione delle interruzioni del sistema operativo</td>
<td>N/D</td>
<td>N/D</td>
<td>Il daemon di sistema di richieste di interruzione (IRQ) è stato sostituito con un gestore interruzioni più performante.</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del master 1.11.3_1527, rilasciato il 15 ottobre 2018
{: #1113_1527}

La seguente tabella mostra le modifiche incluse nel fix pack del master 1.11.3_1527.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.3_1524">
<caption>Modifiche intervenute dalla versione 1.11.3_1524</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione Calico</td>
<td>N/D</td>
<td>N/D</td>
<td>È stata corretta l'analisi di disponibilità del contenitore `calico-node` per gestire meglio gli errori del nodo.</td>
</tr>
<tr>
<td>Aggiornamento cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato risolto il problema con l'aggiornamento dei componenti aggiuntivi del cluster quando il master viene aggiornato da una versione non supportata.</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.11.3_1525, rilasciato il 10 ottobre 2018
{: #1113_1525}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.11.3_1525.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.3_1524">
<caption>Modifiche intervenute dalla versione 1.11.3_1524</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-14633, CVE-2018-17182 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Timeout di sessione inattiva</td>
<td>N/D</td>
<td>N/D</td>
<td>Imposta il timeout della sessione inattiva su 5 minuti per motivi di conformità.</td>
</tr>
</tbody>
</table>


### Changelog per 1.11.3_1524, rilasciato il 2 ottobre 2018
{: #1113_1524}

La seguente tabella mostra le modifiche incluse nella patch 1.11.3_1524.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.3_1521">
<caption>Modifiche intervenute dalla versione 1.11.3_1521</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>1.1.3</td>
<td>1.1.4</td>
<td>Vedi le [note sulla release di containerd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.1.4).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-91</td>
<td>v1.11.3-100</td>
<td>È stato aggiornato il link alla documentazione nei messaggi di errore del programma di bilanciamento del carico.</td>
</tr>
<tr>
<td>Classi di archiviazione file IBM</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato rimosso il parametro duplicato `reclaimPolicy` nelle classi di archiviazione file IBM.<br><br>
Inoltre, adesso quando aggiorni il master del cluster, la classe di archiviazione file IBM predefinita rimane invariata. Se desideri modificare la classe di archiviazione predefinita, esegui `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` e sostituisci `<storageclass>` con il nome della classe di archiviazione.</td>
</tr>
</tbody>
</table>

### Changelog per 1.11.3_1521, rilasciato il 20 ottobre 2018
{: #1113_1521}

La seguente tabella mostra le modifiche incluse nella patch 1.11.3_1521.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.2_1516">
<caption>Modifiche intervenute dalla versione 1.11.2_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.2-71</td>
<td>v1.11.3-91</td>
<td>Aggiornato per supportare la release Kubernetes 1.11.3.</td>
</tr>
<tr>
<td>Classi di archiviazione file IBM</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato rimosso `mountOptions` nelle classi di archiviazione file IBM per utilizzare l'impostazione predefinita fornita dal nodo di lavoro.<br><br>
Inoltre, adesso quando aggiorni il master del cluster, la classe di archiviazione file IBM predefinita rimane `ibmc-file-bronze`. Se desideri modificare la classe di archiviazione predefinita, esegui `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` e sostituisci `<storageclass>` con il nome della classe di archiviazione.</td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>N/D</td>
<td>N/D</td>
<td>È stata aggiunta la possibilità di utilizzare il provider KMS (key management service) di Kubernetes nel cluster, per supportare {{site.data.keyword.keymanagementservicefull}}. Quando [abiliti {{site.data.keyword.keymanagementserviceshort}} nel cluster](/docs/containers?topic=containers-encryption#keyprotect), tutti i tuoi segreti Kubernetes vengono crittografati.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.2</td>
<td>v1.11.3</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.3).</td>
</tr>
<tr>
<td>DNS autoscaler di Kubernetes</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>Vedi le [note sulla release di DNS autoscaler di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0).</td>
</tr>
<tr>
<td>Rotazione dei log</td>
<td>N/D</td>
<td>N/D</td>
<td>Si è passato all'utilizzo dei timer `systemd` anziché `cronjobs` per evitare che `logrotate` non funzioni sui nodi di lavoro che non vengono ricaricati o aggiornati entro 90 giorni. **Nota**: in tutte le versioni precedenti per questa release secondaria, il disco primario si riempie dopo che il lavoro cron ha esito negativo perché i log non vengono ruotati. Il lavoro cron ha esito negativo dopo che il nodo di lavoro resta attivo per 90 giorni senza essere aggiornato o ricaricato. Se i log riempiono l'intero disco primario, il nodo di lavoro entra in uno stato non riuscito. Il nodo di lavoro può essere corretto utilizzando il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` o il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>Scadenza della password root</td>
<td>N/D</td>
<td>N/D</td>
<td>Le password root per i nodi di lavoro scadono dopo 90 giorni per motivi di conformità. Se i tuoi strumenti di automazione devono accedere al nodo di lavoro come root o fanno affidamento su lavori cron eseguiti come root, puoi disabilitare la scadenza della password accedendo al nodo di lavoro ed eseguendo `chage -M -1 root`. **Nota**: se hai requisiti di conformità di sicurezza che impediscono l'esecuzione come root o la rimozione della scadenza della password, non disabilitare la scadenza. Puoi invece [aggiornare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) o [ricaricare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) i tuoi nodi di lavoro almeno ogni 90 giorni.</td>
</tr>
<tr>
<td>Componenti di runtime del nodo di lavoro (`kubelet`, `kube-proxy`, `containerd`)</td>
<td>N/D</td>
<td>N/D</td>
<td>Sono state rimosse le dipendenze dei componenti di runtime sul disco primario. Questo miglioramento impedisce il malfunzionamento dei nodi di lavoro quando il disco primario viene riempito.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/D</td>
<td>N/D</td>
<td>Elimina periodicamente le unità di montaggio temporanee per evitare che diventino illimitate. Questa azione risolve il [problema di Kubernetes 57345 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
</tbody>
</table>

### Changelog per 1.11.2_1516, rilasciato il 4 settembre 2018
{: #1112_1516}

La seguente tabella mostra le modifiche incluse nella patch 1.11.2_1516.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.2_1514">
<caption>Modifiche intervenute dalla versione 1.11.2_1514</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>Vedi le [note sulla release Calico![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.2/releases/#v321).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.2</td>
<td>1.1.3</td>
<td>Vedi le [note sulla release `containerd` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.1.3).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.2-60</td>
<td>v1.11.2-71</td>
<td>È stata modificata la configurazione del provider cloud per gestire meglio gli aggiornamenti per i servizi del programma di bilanciamento del carico con `externalTrafficPolicy` impostata su `local`.</td>
</tr>
<tr>
<td>Configurazione del plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>N/D</td>
<td>N/D</td>
<td>È stata rimossa la versione NFS predefinita dalle opzioni di montaggio nelle classi di archiviazione file fornite da IBM. Il sistema operativo dell'host ora negozia la versione NFS con il server NFS dell'infrastruttura IBM Cloud (SoftLayer). Per impostare manualmente una specifica versione NFS, o modificare la versione NFS del tuo PV che era stato negoziato dal sistema operativo dell'host, vedi [Modifica della versione NFS predefinita](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.11.2_1514, rilasciato il 23 agosto 2018
{: #1112_1514}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.11.2_1514.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.11.2_1513">
<caption>Modifiche intervenute dalla versione 1.11.2_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>`systemd` è stato aggiornato per correggere la perdita `cgroup`.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-3620,CVE-2018-3646 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>

### Changelog per 1.11.2_1513, rilasciato il 14 agosto 2018
{: #1112_1513}

La seguente tabella mostra le modifiche incluse nella patch 1.11.2_1513.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.5_1518">
<caption>Modifiche intervenute dalla versione 1.10.5_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>N/D</td>
<td>1.1.2</td>
<td>`containerd` sostituisce Docker come nuovo runtime del contenitore per Kubernetes. Vedi le [note sulla release `containerd` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/containerd/containerd/releases/tag/v1.1.2). Per le azioni che devi eseguire, vedi [Aggiornamento a `containerd` come runtime del contenitore](/docs/containers?topic=containers-cs_versions#containerd).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/D</td>
<td>N/D</td>
<td>`containerd` sostituisce Docker come nuovo runtime del contenitore per Kubernetes, per migliorare le prestazioni. Per le azioni che devi eseguire, vedi [Aggiornamento a `containerd` come runtime del contenitore](/docs/containers?topic=containers-cs_versions#containerd).</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.14</td>
<td>v3.2.18</td>
<td>Vedi le [note sulla release etcd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coreos/etcd/releases/v3.2.18).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.5-118</td>
<td>v1.11.2-60</td>
<td>Aggiornato per supportare la release Kubernetes 1.11. Inoltre, i pod del programma di bilanciamento del carico ora usano la nuova classe di priorità del pod `ibm-app-cluster-critical`.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>334</td>
<td>338</td>
<td>La versione di `incubator` è stata aggiornata alla 1.8. Il provisioning dell'archiviazione file viene eseguito alla specifica zona da te selezionata. Non puoi aggiornare delle etichette di un'istanza PV (statica) esistente a meno che tu non stia utilizzando un cluster multizona e debba [aggiungere le etichette di regione e zona](/docs/containers?topic=containers-kube_concepts#storage_multizone).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.11.2</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.2).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione OpenID Connect è stata aggiornata per il server API Kubernetes del cluster per supportare i gruppi di accesso {{site.data.keyword.Bluemix_notm}} IAM (Identity Access and Management). È stata aggiunta `Priority` all'opzione `--enable-admission-plugins` per il server API Kubernetes del cluster ed è stato configurato il cluster per supportare la priorità dei pod. Per ulteriori informazioni, consulta:
<ul><li>[Gruppi di accesso {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#rbac)</li>
<li>[Configurazione della priorità dei pod](/docs/containers?topic=containers-pod_priority#pod_priority)</li></ul></td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.2</td>
<td>v.1.5.4</td>
<td>Sono stati aumentati i limiti di risorsa per il contenitore `heapster-nanny`. Vedi le [note sulla release di Kubernetes Heapster![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/heapster/releases/tag/v1.5.4).</td>
</tr>
<tr>
<td>Configurazione della registrazione</td>
<td>N/D</td>
<td>N/D</td>
<td>La directory dei log del contenitore è ora `/var/log/pods/` invece della precedente `/var/lib/docker/containers/`.</td>
</tr>
</tbody>
</table>

<br />


## Archivio
{: #changelog_archive}

Versioni Kubernetes non supportate:
*  [Versione 1.10](#110_changelog)
*  [Versione 1.9](#19_changelog)
*  [Versione 1.8](#18_changelog)
*  [Versione 1.7](#17_changelog)

### Changelog della versione 1.10 (non supportata dal 16 maggio 2019)
{: #110_changelog}

Esamina i changelog della versione 1.10.
{: shortdesc}

*   [Changelog per il fix pack del nodo di lavoro 1.10.13_1558, rilasciato il 13 maggio 2019](#11013_1558)
*   [Changelog per il fix pack del nodo di lavoro 1.10.13_1557, rilasciato il 29 aprile 2019](#11013_1557)
*   [Changelog per il fix pack del nodo di lavoro 1.10.13_1556, rilasciato il 15 aprile 2019](#11013_1556)
*   [Changelog per 1.10.13_1555, rilasciato l'8 aprile 2019](#11013_1555)
*   [Changelog per il fix pack del nodo di lavoro 1.10.13_1554, rilasciato il 1° aprile 2019](#11013_1554)
*   [Changelog per il fix pack del master 1.10.13_1553, rilasciato il 26 marzo 2019](#11118_1553)
*   [Changelog per 1.10.13_1551, rilasciato il 20 marzo 2019](#11013_1551)
*   [Changelog per 1.10.13_1548, rilasciato il 4 marzo 2019](#11013_1548)
*   [Changelog per il fix pack del nodo di lavoro 1.10.12_1546, rilasciato il 27 febbraio 2019](#11012_1546)
*   [Changelog per il fix pack del nodo di lavoro 1.10.12_1544, rilasciato il 15 febbraio 2019](#11012_1544)
*   [Changelog per 1.10.12_1543, rilasciato il 5 febbraio 2019](#11012_1543)
*   [Changelog per il fix pack del nodo di lavoro 1.10.12_1541, rilasciato il 28 gennaio 2019](#11012_1541)
*   [Changelog per 1.10.12_1540, rilasciato il 21 gennaio 2019](#11012_1540)
*   [Changelog per il fix pack del nodo di lavoro 1.10.11_1538, rilasciato il 7 gennaio 2019](#11011_1538)
*   [Changelog per il fix pack del nodo di lavoro 1.10.11_1537, rilasciato il 17 dicembre 2018](#11011_1537)
*   [Changelog per 1.10.11_1536, rilasciato il 4 dicembre 2018](#11011_1536)
*   [Changelog per il fix pack del nodo di lavoro 1.10.8_1532, rilasciato il 27 novembre 2018](#1108_1532)
*   [Changelog per il fix pack del nodo di lavoro 1.10.8_1531, rilasciato il 19 novembre 2018](#1108_1531)
*   [Changelog per 1.10.8_1530, rilasciato il 7 novembre 2018](#1108_1530_ha-master)
*   [Changelog per il fix pack del nodo di lavoro 1.10.8_1528, rilasciato il 26 ottobre 2018](#1108_1528)
*   [Changelog per il fix pack del nodo di lavoro 1.10.8_1525, rilasciato il 10 ottobre 2018](#1108_1525)
*   [Changelog per 1.10.8_1524, rilasciato il 2 ottobre 2018](#1108_1524)
*   [Changelog per il fix pack del nodo di lavoro 1.10.7_1521, rilasciato il 20 settembre 2018](#1107_1521)
*   [Changelog per 1.10.7_1520, rilasciato il 4 settembre 2018](#1107_1520)
*   [Changelog per il fix pack del nodo di lavoro 1.10.5_1519, rilasciato il 23 agosto 2018](#1105_1519)
*   [Changelog per il fix pack del nodo di lavoro 1.10.5_1518, rilasciato il 13 agosto 2018](#1105_1518)
*   [Changelog per 1.10.5_1517, rilasciato il 27 luglio 2018](#1105_1517)
*   [Changelog per il fix pack del nodo di lavoro 1.10.3_1514, rilasciato il 3 luglio 2018](#1103_1514)
*   [Changelog per il fix pack del nodo di lavoro 1.10.3_1513, rilasciato il 21 giugno 2018](#1103_1513)
*   [Changelog per 1.10.3_1512, rilasciato il 12 giugno 2018](#1103_1512)
*   [Changelog per il fix pack del nodo di lavoro 1.10.1_1510, rilasciato il 18 maggio 2018](#1101_1510)
*   [Changelog per il fix pack del nodo di lavoro 1.10.1_1509, rilasciato il 16 maggio 2018](#1101_1509)
*   [Changelog per 1.10.1_1508, rilasciato il 01 maggio 2018](#1101_1508)

#### Changelog per il fix pack del nodo di lavoro 1.10.13_1558, rilasciato il 13 maggio 2019
{: #11013_1558}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.13_1558.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.13_1557">
<caption>Modifiche intervenute dalla versione 1.10.13_1557</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy HA del master cluster</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Vedi le [note sulla release di HAProxy ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.haproxy.org/download/1.9/src/CHANGELOG). L'aggiornamento risolve [CVE-2019-6706 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.13_1557, rilasciato il 29 aprile 2019
{: #11013_1557}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.13_1557.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.13_1556">
<caption>Modifiche intervenute dalla 1.10.13_1556</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati.</td>
</tr>
</tbody>
</table>


#### Changelog per il fix pack del nodo di lavoro 1.10.13_1556, rilasciato il 15 aprile 2019
{: #11013_1556}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.13_1556.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.13_1555">
<caption>Modifiche intervenute dalla 1.10.13_1555</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati, incluso `systemd` per [CVE-2019-3842 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

#### Changelog per 1.10.13_1555, rilasciato l'8 aprile 2019
{: #11013_1555}

La seguente tabella mostra le modifiche incluse nella patch 1.10.13_1555.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.13_1554">
<caption>Modifiche intervenute dalla versione 1.10.13_1554</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy HA del master cluster</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Vedi le [note sulla release di HAProxy ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.haproxy.org/download/1.9/src/CHANGELOG). L'aggiornamento risolve [CVE-2018-0732 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) e [CVE-2019-1559 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>DNS Kubernetes</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>Vedi le [note sulla release di DNS Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Immagine aggiornata per [CVE-2017-12447 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Kernel Ubuntu 16.04</td>
<td>4.4.0-143 generica</td>
<td>4.4.0-145 generica</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2019-9213 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Kernel Ubuntu 18.04</td>
<td>4.15.0-46 generica</td>
<td>4.15.0-47 generica</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2019-9213 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.13_1554, rilasciato il 1° aprile 2019
{: #11013_1554}

La seguente tabella mostra le modifiche include nella correzione del nodo di lavoro 1.10.13_1554.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.13_1553">
<caption>Modifiche dalla versione 1.10.13_1553</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilizzo delle risorse del nodo di lavoro</td>
<td>N/D</td>
<td>N/D</td>
<td>Riserve di memoria aumentate per kubelet e containerd per impedire che questi componenti esauriscano le risorse. Per ulteriori informazioni, vedi [Riserve di risorse del nodo di lavoro](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>


#### Changelog per il fix pack del master 1.10.13_1553, rilasciato il 26 marzo 2019
{: #11118_1553}

La seguente tabella mostra le modifiche incluse nel fix pack del master 1.10.13_1553.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.13_1551">
<caption>Modifiche intervenute dalla versione 1.10.13_1551</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>345</td>
<td>346</td>
<td>Immagine aggiornata per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>166</td>
<td>167</td>
<td>Corregge gli errori `context deadline exceeded` e `timeout` intermittenti per la gestione dei segreti Kubernetes. Corregge inoltre gli aggiornamenti al KMS (key management service) che potrebbero lasciare come non crittografati i segreti Kubernetes esistenti. L'aggiornamento include una correzione per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Programma di bilanciamento del carico e monitoraggio del programma di bilanciamento del carico per il {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Immagine aggiornata per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

#### Changelog per 1.10.13_1551, rilasciato il 20 marzo 2019
{: #11013_1551}

La seguente tabella mostra le modifiche incluse nella patch 1.10.13_1551.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.13_1548">
<caption>Modifiche intervenute dalla versione 1.10.13_1548</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione proxy HA del master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione è stata aggiornata per gestire meglio gli errori di connessione intermittenti al master cluster.</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Sono stati aggiornati i driver GPU per [418.43 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.nvidia.com/object/unix.html). L'aggiornamento include una correzione per [CVE-2019-9741 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>344</td>
<td>345</td>
<td>È stato aggiunto il supporto per gli [endpoint del servizio privato](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2019-6133 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>136</td>
<td>166</td>
<td>Immagine aggiornata per [CVE-2018-16890 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) e [CVE-2019-3823 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Immagine aggiornata per [CVE-2018-10779 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) e [CVE-2019-7663 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

#### Changelog per 1.10.13_1548, rilasciato il 4 marzo 2019
{: #11013_1548}

La seguente tabella mostra le modifiche incluse nella patch 1.10.13_1548.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.12_1546">
<caption>Modifiche intervenute dalla versione 1.10.12_1546</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Immagini aggiornate per [CVE-2019-6454 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.12-252</td>
<td>v1.10.13-288</td>
<td>Aggiornato per supportare la release Kubernetes 1.10.13. Sono stati corretti i problemi di connettività periodici per i programmi di bilanciamento del carico che impostano `externalTrafficPolicy` su `local`. Sono stati aggiornati gli eventi del programma di bilanciamento del carico per utilizzare i link alla documentazione {{site.data.keyword.Bluemix_notm}} più recenti.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>342</td>
<td>344</td>
<td>È stato modificato il sistema operativo di base per l'immagine da Fedora a Alpine. Immagine aggiornata per [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>122</td>
<td>136</td>
<td>Il timeout del client è stato aumentato a {{site.data.keyword.keymanagementservicefull_notm}}. Immagine aggiornata per [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.12</td>
<td>v1.10.13</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.13).</td>
</tr>
<tr>
<td>DNS Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Limite di memoria del pod di DNS Kubernetes aumentato da `170Mi` a `400Mi` per gestire più servizi cluster.</td>
</tr>
<tr>
<td>Programma di bilanciamento del carico e monitoraggio del programma di bilanciamento del carico per il {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Immagine aggiornata per [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Client e server OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Immagine aggiornata per [CVE-2019-1559 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agent Trusted Compute</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Immagine aggiornata per [CVE-2019-6454 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.12_1546, rilasciato il 27 febbraio 2019
{: #11012_1546}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.12_1546.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.12_1544">
<caption>Modifiche intervenute dalla versione 1.10.12_1544</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-19407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.12_1544, rilasciato il 15 febbraio 2019
{: #11012_1544}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.12_1544.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.12_1543">
<caption>Modifiche intervenute dalla versione 1.10.12_1543</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>18.06.1-ce</td>
<td>18.06.2-ce</td>
<td>Vedi le [note sulla release di Docker Community Edition ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/docker/docker-ce/releases/tag/v18.06.2-ce). L'aggiornamento risolve [CVE-2019-5736 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configurazione `kubelet` di Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato abilitato il gate della funzione `ExperimentalCriticalPodAnnotation` per evitare la rimozione del pod statico critico.</td>
</tr>
</tbody>
</table>

#### Changelog per 1.10.12_1543, rilasciato il 5 febbraio 2019
{: #11012_1543}

La seguente tabella mostra le modifiche incluse nella patch 1.10.12_1543.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.12_1541">
<caption>Modifiche intervenute dalla versione 1.10.12_1541</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Vedi le [note sulla release etcd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coreos/etcd/releases/v3.3.11). Inoltre, le suite di cifratura supportate per etcd sono ora limitate a un sottoinsieme con crittografia molto elevata (128 bit o più).</td>
</tr>
<tr>
<td>Plugin del dispositivo GPU e programma di installazione</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Immagini aggiornate per [CVE-2019-3462 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) e [CVE-2019-6486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>338</td>
<td>342</td>
<td>Il plugin di archiviazione file è stato aggiornato nel seguente modo:
<ul><li>Supporta il provisioning dinamico con una [pianificazione che rileva la topologia dei volumi](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignora gli errori di eliminazione dell'attestazione del volume persistente (o PVC, persistent volume claim) se l'archiviazione è già stata eliminata.</li>
<li>Aggiunge un'annotazione di messaggio di errore alle PVC non riuscite.</li>
<li>Ottimizza le impostazioni di periodo di risincronizzazione e di designazione del leader del controller del provisioner di archiviazione e aumenta il timeout del provisioning da 30 minuti a 1 ora.</li>
<li>Controlla le autorizzazioni utente prima di avviare il provisioning.</li></ul></td>
</tr>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>111</td>
<td>122</td>
<td>È stata aggiunta una logica di nuovi tentativi per evitare errori temporanei quando i segreti Kubernetes sono gestiti da {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>La configurazione della politica di controllo del server API Kubernetes è stata aggiornata per includere la registrazione dei metadati per le richieste `cluster-admin` e la registrazione del corpo delle richieste `create`, `update` e `patch` del carico di lavoro.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Immagine aggiornata per [CVE-2018-0734 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Inoltre, la configurazione del pod viene ora ottenuta da un segreto invece che da una mappa di configurazione.</td>
</tr>
<tr>
<td>Server OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Immagine aggiornata per [CVE-2018-0734 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Patch di sicurezza per [CVE-2018-16864 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.12_1541, rilasciato il 28 gennaio 2019
{: #11012_1541}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.12_1541.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.12_1540">
<caption>Modifiche intervenute dalla versione 1.10.12_1540</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati, incluso `apt` per [CVE-2019-3462 ![icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) e [USN-3863-1 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>

#### Changelog per 1.10.12_1540, rilasciato il 21 gennaio 2019
{: #11012_1540}

La seguente tabella mostra le modifiche incluse nella patch 1.10.12_1540.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.11_1538">
<caption>Modifiche intervenute dalla versione 1.10.11_1538</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.11-219</td>
<td>v1.10.12-252</td>
<td>Aggiornato per supportare la release Kubernetes 1.10.12.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.11</td>
<td>v1.10.12</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.12).</td>
</tr>
<tr>
<td>Add-On Resizer di Kubernetes</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Vedi le [note sulla release dell'Add-On Resizer di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Dashboard Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>Vedi le [note sulla release del dashboard Kubernetes
![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). L'aggiornamento risolve [CVE-2018-18264 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).<br><br>Se accedi al dashboard tramite kubectl proxy, il pulsante SKIP nella pagina di accesso viene rimosso. [Utilizza invece un Token per eseguire l'accesso](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td>Programma di installazione GPU</td>
<td>390.12</td>
<td>410.79</td>
<td>I driver GPU installati sono stati aggiornati alla 410.79.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.11_1538, rilasciato il 7 gennaio 2019
{: #11011_1538}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.11_1538.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.11_1537">
<caption>Modifiche intervenute dalla versione 1.10.11_1537</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2017-5753, CVE-2018-18690 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.11_1537, rilasciato il 17 dicembre 2018
{: #11011_1537}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.11_1537.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.11_1536">
<caption>Modifiche intervenute dalla versione 1.10.11_1536</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati.</td>
</tr>
</tbody>
</table>


#### Changelog per 1.10.11_1536, rilasciato il 4 dicembre 2018
{: #11011_1536}

La seguente tabella mostra le modifiche incluse nella patch 1.10.11_1536.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.8_1532">
<caption>Modifiche intervenute dalla versione 1.10.8_1532</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>Vedi le [note sulla release Calico![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.3/releases/#v331). L'aggiornamento risolve [Tigera Technical Advisory TTA-2018-001 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.8-197</td>
<td>v1.10.11-219</td>
<td>Aggiornato per supportare la release Kubernetes 1.10.11.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.8</td>
<td>v1.10.11</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.11). L'aggiornamento risolve [CVE-2018-1002105 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
<tr>
<td>Client e server OpenVPN</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Immagine aggiornata per [CVE-2018-0732 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) e [CVE-2018-0737 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
<tr>
<td>Utilizzo delle risorse del nodo di lavoro</td>
<td>N/D</td>
<td>N/D</td>
<td>Sono stati aggiunti dei gruppi di controllo (cgroup) dedicati per kubelet e docker per impedire che questi componenti esauriscano le risorse. Per ulteriori informazioni, vedi [Riserve di risorse del nodo di lavoro](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.8_1532, rilasciato il 27 novembre 2018
{: #1108_1532}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.8_1532.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.8_1531">
<caption>Modifiche intervenute dalla versione 1.10.8_1531</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>Vedi le [note sulla release Docker ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.docker.com/engine/release-notes/#18061-ce).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.8_1531, rilasciato il 19 novembre 2018
{: #1108_1531}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.8_1531.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.8_1530">
<caption>Modifiche intervenute dalla versione 1.10.8_1530</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-7755 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>

#### Changelog per 1.10.8_1530, rilasciato il 7 novembre 2018
{: #1108_1530_ha-master}

La seguente tabella mostra le modifiche incluse nella patch 1.10.8_1530.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.8_1528">
<caption>Modifiche intervenute dalla versione 1.10.8_1528</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Master cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>È stata aggiornata la configurazione del master cluster per aumentare l'alta disponibilità (HA). I cluster ora dispongono di tre repliche del master Kubernetes che vengono impostate con una configurazione altamente disponibile (HA), con ciascun master distribuito su host fisici separati. Inoltre, se il tuo cluster si trova in una zona che supporta il multizona, i master vengono estesi tra le zone.<br>Per le azioni che devi eseguire, vedi [Aggiornamento a master cluster altamente disponibili](/docs/containers?topic=containers-cs_versions#ha-masters). Queste azioni di preparazione si applicano:<ul>
<li>Se hai un firewall o politiche di rete Calico personalizzate.</li>
<li>Se utilizzi le porte host `2040` o `2041` sui tuoi nodi di lavoro.</li>
<li>Se hai utilizzato l'indirizzo IP del master cluster per l'accesso in cluster al master.</li>
<li>Se disponi dell'automazione che richiama l'API o la CLI Calico (`calicoctl`), ad esempio per creare le politiche Calico.</li>
<li>Se utilizzi le politiche di rete di Kubernetes o Calico per controllare l'accesso in uscita dei pod al master.</li></ul></td>
</tr>
<tr>
<td>Proxy HA del master cluster</td>
<td>N/D</td>
<td>1.8.12-alpine</td>
<td>È stato aggiunto un pod `ibm-master-proxy-*` per il bilanciamento del carico lato client su tutti i nodi di lavoro, in modo che ogni client del nodo di lavoro possa instradare le richieste a una replica del master HA disponibile.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>Vedi le [note sulla release etcd ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/coreos/etcd/releases/v3.3.1).</td>
</tr>
<tr>
<td>Crittografia dei dati in etcd</td>
<td>N/D</td>
<td>N/D</td>
<td>In precedenza, i dati etcd erano archiviati sull'istanza di archiviazione file NFS del master che veniva crittografata quando inattiva. Ora, i dati etcd vengono archiviati sul disco locale del master e sottoposti a backup in {{site.data.keyword.cos_full_notm}}. I dati vengono crittografati durante il transito su {{site.data.keyword.cos_full_notm}} e quando inattivi. Tuttavia, i dati etcd sul disco locale del master non vengono crittografati. Se vuoi che i dati etcd locali del master vengano crittografati, [abilita {{site.data.keyword.keymanagementservicelong_notm}} nel tuo cluster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.8-172</td>
<td>v1.10.8-197</td>
<td>È stata aggiunta l'annotazione `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` per specificare la VLAN in cui viene distribuito il servizio del programma di bilanciamento del carico. Per visualizzare le VLAN disponibili nel tuo cluster, esegui `ibmcloud ks vlans --zone <zone>`.</td>
</tr>
<tr>
<td>Kernel abilitato a TPM</td>
<td>N/D</td>
<td>N/D</td>
<td>I nodi di lavoro bare metal con chip TPM per Trusted Compute utilizzano il kernel Ubuntu predefinito fino a quando non viene abilitata l'attendibilità. Se [abiliti l'attendibilità](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) su un cluster esistente, devi [ricaricare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) tutti i nodi di lavoro bare metal esistenti con i chip TPM. Per verificare se un nodo di lavoro bare metal ha un chip TPM, controlla il campo **Trustable** dopo l'esecuzione del [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-types --zone`.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.8_1528, rilasciato il 26 ottobre 2018
{: #1108_1528}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.8_1528.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.8_1527">
<caption>Modifiche intervenute dalla versione 1.10.8_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Gestione delle interruzioni del sistema operativo</td>
<td>N/D</td>
<td>N/D</td>
<td>Il daemon di sistema di richieste di interruzione (IRQ) è stato sostituito con un gestore interruzioni più performante.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del master 1.10.8_1527, rilasciato il 15 ottobre 2018
{: #1108_1527}

La seguente tabella mostra le modifiche incluse nel fix pack del master 1.10.8_1527.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.8_1524">
<caption>Modifiche intervenute dalla versione 1.10.8_1524</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configurazione Calico</td>
<td>N/D</td>
<td>N/D</td>
<td>È stata corretta l'analisi di disponibilità del contenitore `calico-node` per gestire meglio gli errori del nodo.</td>
</tr>
<tr>
<td>Aggiornamento cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato risolto il problema con l'aggiornamento dei componenti aggiuntivi del cluster quando il master viene aggiornato da una versione non supportata.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.8_1525, rilasciato il 10 ottobre 2018
{: #1108_1525}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.8_1525.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.8_1524">
<caption>Modifiche intervenute dalla versione 1.10.8_1524</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-14633, CVE-2018-17182 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Timeout di sessione inattiva</td>
<td>N/D</td>
<td>N/D</td>
<td>Imposta il timeout della sessione inattiva su 5 minuti per motivi di conformità.</td>
</tr>
</tbody>
</table>


#### Changelog per 1.10.8_1524, rilasciato il 2 ottobre 2018
{: #1108_1524}

La seguente tabella mostra le modifiche incluse nella patch 1.10.8_1524.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.7_1520">
<caption>Modifiche intervenute dalla versione 1.10.7_1520</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provider KMS (Key Management Service)</td>
<td>N/D</td>
<td>N/D</td>
<td>È stata aggiunta la possibilità di utilizzare il provider KMS (key management service) di Kubernetes nel cluster, per supportare {{site.data.keyword.keymanagementservicefull}}. Quando [abiliti {{site.data.keyword.keymanagementserviceshort}} nel cluster](/docs/containers?topic=containers-encryption#keyprotect), tutti i tuoi segreti Kubernetes vengono crittografati.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.7</td>
<td>v1.10.8</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.8).</td>
</tr>
<tr>
<td>DNS autoscaler di Kubernetes</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>Vedi le [note sulla release di DNS autoscaler di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.7-146</td>
<td>v1.10.8-172</td>
<td>Aggiornato per supportare la release Kubernetes 1.10.8. Inoltre, è stato aggiornato il link alla documentazione nei messaggi di errore del programma di bilanciamento del carico.</td>
</tr>
<tr>
<td>Classi di archiviazione file IBM</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato rimosso `mountOptions` nelle classi di archiviazione file IBM per utilizzare l'impostazione predefinita fornita dal nodo di lavoro. È stato rimosso il parametro duplicato `reclaimPolicy` nelle classi di archiviazione file IBM.<br><br>
Inoltre, adesso quando aggiorni il master del cluster, la classe di archiviazione file IBM predefinita rimane invariata. Se desideri modificare la classe di archiviazione predefinita, esegui `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` e sostituisci `<storageclass>` con il nome della classe di archiviazione.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.7_1521, rilasciato il 20 settembre 2018
{: #1107_1521}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.7_1521.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.7_1520">
<caption>Modifiche intervenute dalla versione 1.10.7_1520</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Rotazione dei log</td>
<td>N/D</td>
<td>N/D</td>
<td>Si è passato all'utilizzo dei timer `systemd` anziché `cronjobs` per evitare che `logrotate` non funzioni sui nodi di lavoro che non vengono ricaricati o aggiornati entro 90 giorni. **Nota**: in tutte le versioni precedenti per questa release secondaria, il disco primario si riempie dopo che il lavoro cron ha esito negativo perché i log non vengono ruotati. Il lavoro cron ha esito negativo dopo che il nodo di lavoro resta attivo per 90 giorni senza essere aggiornato o ricaricato. Se i log riempiono l'intero disco primario, il nodo di lavoro entra in uno stato non riuscito. Il nodo di lavoro può essere corretto utilizzando il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` o il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>Componenti di runtime del nodo di lavoro (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/D</td>
<td>N/D</td>
<td>Sono state rimosse le dipendenze dei componenti di runtime sul disco primario. Questo miglioramento impedisce il malfunzionamento dei nodi di lavoro quando il disco primario viene riempito.</td>
</tr>
<tr>
<td>Scadenza della password root</td>
<td>N/D</td>
<td>N/D</td>
<td>Le password root per i nodi di lavoro scadono dopo 90 giorni per motivi di conformità. Se i tuoi strumenti di automazione devono accedere al nodo di lavoro come root o fanno affidamento su lavori cron eseguiti come root, puoi disabilitare la scadenza della password accedendo al nodo di lavoro ed eseguendo `chage -M -1 root`. **Nota**: se hai requisiti di conformità di sicurezza che impediscono l'esecuzione come root o la rimozione della scadenza della password, non disabilitare la scadenza. Puoi invece [aggiornare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) o [ricaricare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) i tuoi nodi di lavoro almeno ogni 90 giorni.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/D</td>
<td>N/D</td>
<td>Elimina periodicamente le unità di montaggio temporanee per evitare che diventino illimitate. Questa azione risolve il [problema di Kubernetes 57345 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato disabilitato il bridge Docker predefinito in modo che per gli instradamenti privati venga utilizzato ora l'intervallo di IP `172.17.0.0/16`. Se ti affidi alla creazione di contenitori Docker nei nodi di lavoro eseguendo direttamente i comandi `docker` sull'host o utilizzando un pod che monta il socket Docker, scegli tra le seguenti opzioni.<ul><li>Per garantire la connettività di rete esterna quando crei il contenitore, esegui `docker build . --network host`.</li>
<li>Per creare esplicitamente una rete da utilizzare quando crei il contenitore, esegui `docker network create` e utilizza quindi questa rete.</li></ul>
**Nota**: hai dipendenze sul socket Docker o direttamente su Docker? [Aggiorna a `containerd` anziché a `docker` come runtime del contenitore](/docs/containers?topic=containers-cs_versions#containerd) in modo che i tuoi cluster siano preparati per eseguire Kubernetes versione 1.11 o successive.</td>
</tr>
</tbody>
</table>

#### Changelog per 1.10.7_1520, rilasciato il 4 settembre 2018
{: #1107_1520}

La seguente tabella mostra le modifiche incluse nella patch 1.10.7_1520.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.5_1519">
<caption>Modifiche intervenute dalla versione 1.10.5_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>Consulta le [note sulla release Calico ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.2/releases/#v321).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.5-118</td>
<td>v1.10.7-146</td>
<td>Aggiornato per supportare la release Kubernetes 1.10.7. È stata inoltre modificata la configurazione del provider cloud per gestire meglio gli aggiornamenti per i servizi del programma di bilanciamento del carico con `externalTrafficPolicy` impostata su `local`.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>334</td>
<td>338</td>
<td>La versione di incubator è stata aggiornata alla 1.8. Il provisioning dell'archiviazione file viene eseguito alla specifica zona da te selezionata. Non puoi aggiornare delle etichette di un'istanza PV (statica) esistente a meno che tu non stia utilizzando un cluster multizona e debba aggiungere le etichette di regione e zona.<br><br> È stata rimossa la versione NFS predefinita dalle opzioni di montaggio nelle classi di archiviazione file fornite da IBM. Il sistema operativo dell'host ora negozia la versione NFS con il server NFS dell'infrastruttura IBM Cloud (SoftLayer). Per impostare manualmente una specifica versione NFS, o modificare la versione NFS del tuo PV che era stato negoziato dal sistema operativo dell'host, vedi [Modifica della versione NFS predefinita](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.10.7</td>
<td>Vedi le [note sulla release Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.7).</td>
</tr>
<tr>
<td>Configurazione di Kubernetes Heapster</td>
<td>N/D</td>
<td>N/D</td>
<td>Sono stati aumentati i limiti di risorsa per il contenitore `heapster-nanny`.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.5_1519, rilasciato il 23 agosto 2018
{: #1105_1519}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.5_1519.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.5_1518">
<caption>Modifiche intervenute dalla versione 1.10.5_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>`systemd` è stato aggiornato per correggere la perdita `cgroup`.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-3620,CVE-2018-3646 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>


#### Changelog per il fix pack del nodo di lavoro 1.10.5_1518, rilasciato il 13 agosto 2018
{: #1105_1518}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.5_1518.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.5_1517">
<caption>Modifiche intervenute dalla versione 1.10.5_1517</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati.</td>
</tr>
</tbody>
</table>

#### Changelog per 1.10.5_1517, rilasciato il 27 luglio 2018
{: #1105_1517}

La seguente tabella mostra le modifiche incluse nella patch 1.10.5_1517.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.3_1514">
<caption>Modifiche intervenute dalla versione 1.10.3_1514</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.1</td>
<td>v3.1.3</td>
<td>Consulta le [note sulla release Calico ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.1/releases/#v313).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.3-85</td>
<td>v1.10.5-118</td>
<td>Aggiornato per supportare la release Kubernetes 1.10.5. Inoltre, gli eventi `create failure` del servizio LoadBalancer ora includono qualsiasi errore di sottorete portatile.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>320</td>
<td>334</td>
<td>Il timeout per la creazione di volumi persistenti è stato aumentato da 15 a 30 minuti. Il tipo di fatturazione predefinito è stato modificato in oraria (`hourly`). Sono state aggiunte delle opzioni di montaggio alle classi di archiviazione predefinite. Nell'istanza di archiviazione file NFS nel tuo account dell'infrastruttura IBM Cloud (SoftLayer), il campo **Note** è stato modificato in formato JSON ed è stato aggiunto lo spazio dei nomi Kubernetes a cui viene distribuito il PV. Per supportare i cluster multizona, sono state aggiunte le etichette di zona e regione ai volumi persistenti.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.3</td>
<td>v1.10.5</td>
<td>Vedi le [note sulla release Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.5).</td>
</tr>
<tr>
<td>Kernel</td>
<td>N/D</td>
<td>N/D</td>
<td>Miglioramenti secondari alle impostazioni di rete dei nodi di lavoro per i carichi di lavoro di rete ad alte prestazioni.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>La distribuzione del client OpenVPN `vpn` che viene eseguita nello spazio dei nomi `kube-system` è ora gestita dall'`addon-manager` Kubernetes.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.3_1514, rilasciato il 3 luglio 2018
{: #1103_1514}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.3_1514.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.3_1513">
<caption>Modifiche intervenute dalla versione 1.10.3_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>N/D</td>
<td>N/D</td>
<td>`sysctl` ottimizzato per i carichi di lavoro di rete ad alte prestazioni.</td>
</tr>
</tbody>
</table>


#### Changelog per il fix pack del nodo di lavoro 1.10.3_1513, rilasciato il 21 giugno 2018
{: #1103_1513}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.3_1513.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.3_1512">
<caption>Modifiche intervenute dalla versione 1.10.3_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/D</td>
<td>N/D</td>
<td>Per i tipi di macchina non crittografati, il disco secondario viene ripulito ottenendo un file system nuovo quando ricarichi o aggiorni il nodo di lavoro.</td>
</tr>
</tbody>
</table>

#### Changelog per 1.10.3_1512, rilasciato il 12 giugno 2018
{: #1103_1512}

La seguente tabella mostra le modifiche incluse nella patch 1.10.3_1512.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.1_1510">
<caption>Modifiche intervenute dalla versione 1.10.1_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.10.1</td>
<td>v1.10.3</td>
<td>Vedi le [note sulla release Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.3).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>È stata aggiunta `PodSecurityPolicy` all'opzione `--enable-admission-plugins` per il server API Kubernetes del cluster ed è stato configurato il cluster per supportare le politiche di sicurezza del pod. Per ulteriori informazioni, vedi [Configurazione delle politiche di sicurezza del pod](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>Configurazione di kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>È stata abilitata l'opzione `--authentication-token-webhook` per supportare i token di account di servizio e di connessione API per l'autenticazione presso l'endpoint HTTPS `kubelet`.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.1-52</td>
<td>v1.10.3-85</td>
<td>Aggiornato per supportare la release Kubernetes 1.10.3.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato aggiunto `livenessProbe` alla distribuzione `vpn` del client OpenVPN che viene eseguita nello spazio dei nomi `kube-system`.</td>
</tr>
<tr>
<td>Aggiornamento kernel</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Nuove immagini di nodo di lavoro con l'aggiornamento kernel per [CVE-2018-3639 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>



#### Changelog per il fix pack del nodo di lavoro 1.10.1_1510, rilasciato il 18 maggio 2018
{: #1101_1510}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.1_1510.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.1_1509">
<caption>Modifiche intervenute dalla versione 1.10.1_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Correzione per risolvere un bug che si è verificato se hai utilizzato il plugin dell'archiviazione blocchi.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.10.1_1509, rilasciato il 16 maggio 2018
{: #1101_1509}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.10.1_1509.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.10.1_1508">
<caption>Modifiche intervenute dalla versione 1.10.1_1508</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>I dati che archivi nella directory root `kubelet` sono ora salvati nel disco secondario più grande della tua macchina del nodo di lavoro.</td>
</tr>
</tbody>
</table>

#### Changelog per 1.10.1_1508, rilasciato il 01 maggio 2018
{: #1101_1508}

La seguente tabella mostra le modifiche incluse nella patch 1.10.1_1508.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.7_1510">
<caption>Modifiche intervenute dalla versione 1.9.7_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v3.1.1</td>
<td>Consulta le [note sulla release Calico ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.1/releases/#v311).</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.0</td>
<td>v1.5.2</td>
<td>Vedi le [note sulla release Kubernetes Heapster ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.10.1</td>
<td>Vedi le [note sulla release Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiunto <code>StorageObjectInUseProtection</code> all'opzione <code>--enable-admission-plugins</code> del server dell'API Kubernetes del cluster.</td>
</tr>
<tr>
<td>DNS Kubernetes</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>Consulta le [note sulla release della DNS Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/dns/releases/tag/1.14.10).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.7-102</td>
<td>v1.10.1-52</td>
<td>Aggiornato per supportare la release Kubernetes 1.10.</td>
</tr>
<tr>
<td>Supporto GPU</td>
<td>N/D</td>
<td>N/D</td>
<td>Il supporto per i [carichi di lavoro del contenitore GPU (graphics processing unit)](/docs/containers?topic=containers-app#gpu_app) è ora disponibile per la pianificazione e l'esecuzione. Per un elenco di tipi di macchina GPU disponibili, consulta [Hardware per i nodi di lavoro](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes). Per ulteriori informazioni, consulta la documentazione Kubernetes in [Schedule GPUs ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).</td>
</tr>
</tbody>
</table>

<br />


### Changelog della versione 1.9 (non supportata dal 27 dicembre 2018)
{: #19_changelog}

Esamina i changelog della versione 1.9.
{: shortdesc}

*   [Changelog per il fix pack del nodo di lavoro 1.9.11_1539, rilasciato il 17 dicembre 2018](#1911_1539)
*   [Changelog per il fix pack del nodo di lavoro 1.9.11_1538, rilasciato il 4 dicembre 2018](#1911_1538)
*   [Changelog per il fix pack del nodo di lavoro 1.9.11_1537, rilasciato il 27 novembre 2018](#1911_1537)
*   [Changelog per 1.9.11_1536, rilasciato il 19 novembre 2018](#1911_1536)
*   [Changelog per il fix pack del nodo di lavoro 1.9.10_1532, rilasciato il 7 novembre 2018](#1910_1532)
*   [Changelog per il fix pack del nodo di lavoro 1.9.10_1531, rilasciato il 26 ottobre 2018](#1910_1531)
*   [Changelog per il fix pack del master 1.9.10_1530, rilasciato il 15 ottobre 2018](#1910_1530)
*   [Changelog per il fix pack del nodo di lavoro 1.9.10_1528, rilasciato il 10 ottobre 2018](#1910_1528)
*   [Changelog per 1.9.10_1527, rilasciato il 2 ottobre 2018](#1910_1527)
*   [Changelog per il fix pack del nodo di lavoro 1.9.10_1524, rilasciato il 20 settembre 2018](#1910_1524)
*   [Changelog per 1.9.10_1523, rilasciato il 4 settembre 2018](#1910_1523)
*   [Changelog per il fix pack del nodo di lavoro 1.9.9_1522, rilasciato il 23 agosto 2018](#199_1522)
*   [Changelog per il fix pack del nodo di lavoro 1.9.9_1521, rilasciato il 13 agosto 2018](#199_1521)
*   [Changelog per 1.9.9_1520, rilasciato il 27 luglio 2018](#199_1520)
*   [Changelog per il fix pack del nodo di lavoro 1.9.8_1517, rilasciato il 3 luglio 2018](#198_1517)
*   [Changelog per il fix pack del nodo di lavoro 1.9.8_1516, rilasciato il 21 giugno 2018](#198_1516)
*   [Changelog per 1.9.8_1515, rilasciato il 19 giugno 2018](#198_1515)
*   [Changelog per il fix pack del nodo di lavoro 1.9.7_1513, rilasciato l'11 giugno 2018](#197_1513)
*   [Changelog per il fix pack del nodo di lavoro 1.9.7_1512, rilasciato il 18 maggio 2018](#197_1512)
*   [Changelog per il fix pack del nodo di lavoro 1.9.7_1511, rilasciato il 16 maggio 2018](#197_1511)
*   [Changelog per 1.9.7_1510, rilasciato il 30 aprile 2018](#197_1510)

#### Changelog per il fix pack del nodo di lavoro 1.9.11_1539, rilasciato il 17 dicembre 2018
{: #1911_1539}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.11_1539.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.11_1538">
<caption>Modifiche intervenute dalla versione 1.9.11_1538</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.9.11_1538, rilasciato il 4 dicembre 2018
{: #1911_1538}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.11_1538.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.11_1537">
<caption>Modifiche intervenute dalla versione 1.9.11_1537</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilizzo delle risorse del nodo di lavoro</td>
<td>N/D</td>
<td>N/D</td>
<td>Sono stati aggiunti dei gruppi di controllo (cgroup) dedicati per kubelet e docker per impedire che questi componenti esauriscano le risorse. Per ulteriori informazioni, vedi [Riserve di risorse del nodo di lavoro](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.9.11_1537, rilasciato il 27 novembre 2018
{: #1911_1537}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.11_1537.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.11_1536">
<caption>Modifiche intervenute dalla versione 1.9.11_1536</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>Vedi le [note sulla release Docker ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.docker.com/engine/release-notes/#18061-ce).</td>
</tr>
</tbody>
</table>

#### Changelog per 1.9.11_1536, rilasciato il 19 novembre 2018
{: #1911_1536}

La seguente tabella mostra le modifiche incluse nella patch 1.9.11_1536.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.10_1532">
<caption>Modifiche intervenute dalla versione 1.9.10_1532</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v2.6.12</td>
<td>Vedi le [note sulla release Calico![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v2.6/releases/#v2612). L'aggiornamento risolve [Tigera Technical Advisory TTA-2018-001 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-7755 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.10</td>
<td>v1.9.11</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.11).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.10-219</td>
<td>v1.9.11-249</td>
<td>Aggiornato per supportare la release Kubernetes 1.9.11.</td>
</tr>
<tr>
<td>Client e server OpenVPN</td>
<td>2.4.4-r2</td>
<td>2.4.6-r3-IKS-8</td>
<td>Immagine aggiornata per [CVE-2018-0732 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) e [CVE-2018-0737 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.9.10_1532, rilasciato il 7 novembre 2018
{: #1910_1532}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.11_1532.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.10_1531">
<caption>Modifiche intervenute dalla versione 1.9.10_1531</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel abilitato a TPM</td>
<td>N/D</td>
<td>N/D</td>
<td>I nodi di lavoro bare metal con chip TPM per Trusted Compute utilizzano il kernel Ubuntu predefinito fino a quando non viene abilitata l'attendibilità. Se [abiliti l'attendibilità](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) su un cluster esistente, devi [ricaricare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) tutti i nodi di lavoro bare metal esistenti con i chip TPM. Per verificare se un nodo di lavoro bare metal ha un chip TPM, controlla il campo **Trustable** dopo l'esecuzione del [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-types --zone`.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.9.10_1531, rilasciato il 26 ottobre 2018
{: #1910_1531}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.10_1531.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.10_1530">
<caption>Modifiche intervenute dalla versione 1.9.10_1530</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Gestione delle interruzioni del sistema operativo</td>
<td>N/D</td>
<td>N/D</td>
<td>Il daemon di sistema di richieste di interruzione (IRQ) è stato sostituito con un gestore interruzioni più performante.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del master 1.9.10_1530, rilasciato il 15 ottobre 2018
{: #1910_1530}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.10_1530.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.10_1527">
<caption>Modifiche intervenute dalla versione 1.9.10_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Aggiornamento cluster</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato risolto il problema con l'aggiornamento dei componenti aggiuntivi del cluster quando il master viene aggiornato da una versione non supportata.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.9.10_1528, rilasciato il 10 ottobre 2018
{: #1910_1528}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.10_1528.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.10_1527">
<caption>Modifiche intervenute dalla versione 1.9.10_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-14633, CVE-2018-17182 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Timeout di sessione inattiva</td>
<td>N/D</td>
<td>N/D</td>
<td>Imposta il timeout della sessione inattiva su 5 minuti per motivi di conformità.</td>
</tr>
</tbody>
</table>


#### Changelog per 1.9.10_1527, rilasciato il 2 ottobre 2018
{: #1910_1527}

La seguente tabella mostra le modifiche incluse nella patch 1.9.10_1527.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.10_1523">
<caption>Modifiche intervenute dalla versione 1.9.10_1523</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.10-192</td>
<td>v1.9.10-219</td>
<td>È stato aggiornato il link alla documentazione nei messaggi di errore del programma di bilanciamento del carico.</td>
</tr>
<tr>
<td>Classi di archiviazione file IBM</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato rimosso `mountOptions` nelle classi di archiviazione file IBM per utilizzare l'impostazione predefinita fornita dal nodo di lavoro. È stato rimosso il parametro duplicato `reclaimPolicy` nelle classi di archiviazione file IBM.<br><br>
Inoltre, adesso quando aggiorni il master del cluster, la classe di archiviazione file IBM predefinita rimane invariata. Se desideri modificare la classe di archiviazione predefinita, esegui `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` e sostituisci `<storageclass>` con il nome della classe di archiviazione.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.9.10_1524, rilasciato il 20 settembre 2018
{: #1910_1524}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.10_1524.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.10_1523">
<caption>Modifiche intervenute dalla versione 1.9.10_1523</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Rotazione dei log</td>
<td>N/D</td>
<td>N/D</td>
<td>Si è passato all'utilizzo dei timer `systemd` anziché `cronjobs` per evitare che `logrotate` non funzioni sui nodi di lavoro che non vengono ricaricati o aggiornati entro 90 giorni. **Nota**: in tutte le versioni precedenti per questa release secondaria, il disco primario si riempie dopo che il lavoro cron ha esito negativo perché i log non vengono ruotati. Il lavoro cron ha esito negativo dopo che il nodo di lavoro resta attivo per 90 giorni senza essere aggiornato o ricaricato. Se i log riempiono l'intero disco primario, il nodo di lavoro entra in uno stato non riuscito. Il nodo di lavoro può essere corretto utilizzando il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` o il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>Componenti di runtime del nodo di lavoro (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/D</td>
<td>N/D</td>
<td>Sono state rimosse le dipendenze dei componenti di runtime sul disco primario. Questo miglioramento impedisce il malfunzionamento dei nodi di lavoro quando il disco primario viene riempito.</td>
</tr>
<tr>
<td>Scadenza della password root</td>
<td>N/D</td>
<td>N/D</td>
<td>Le password root per i nodi di lavoro scadono dopo 90 giorni per motivi di conformità. Se i tuoi strumenti di automazione devono accedere al nodo di lavoro come root o fanno affidamento su lavori cron eseguiti come root, puoi disabilitare la scadenza della password accedendo al nodo di lavoro ed eseguendo `chage -M -1 root`. **Nota**: se hai requisiti di conformità di sicurezza che impediscono l'esecuzione come root o la rimozione della scadenza della password, non disabilitare la scadenza. Puoi invece [aggiornare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) o [ricaricare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) i tuoi nodi di lavoro almeno ogni 90 giorni.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/D</td>
<td>N/D</td>
<td>Elimina periodicamente le unità di montaggio temporanee per evitare che diventino illimitate. Questa azione risolve il [problema di Kubernetes 57345 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato disabilitato il bridge Docker predefinito in modo che per gli instradamenti privati venga utilizzato ora l'intervallo di IP `172.17.0.0/16`. Se ti affidi alla creazione di contenitori Docker nei nodi di lavoro eseguendo direttamente i comandi `docker` sull'host o utilizzando un pod che monta il socket Docker, scegli tra le seguenti opzioni.<ul><li>Per garantire la connettività di rete esterna quando crei il contenitore, esegui `docker build . --network host`.</li>
<li>Per creare esplicitamente una rete da utilizzare quando crei il contenitore, esegui `docker network create` e utilizza quindi questa rete.</li></ul>
**Nota**: hai dipendenze sul socket Docker o direttamente su Docker? [Aggiorna a `containerd` anziché a `docker` come runtime del contenitore](/docs/containers?topic=containers-cs_versions#containerd) in modo che i tuoi cluster siano preparati per eseguire Kubernetes versione 1.11 o successive.</td>
</tr>
</tbody>
</table>

#### Changelog per 1.9.10_1523, rilasciato il 4 settembre 2018
{: #1910_1523}

La seguente tabella mostra le modifiche incluse nella patch 1.9.10_1523.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.9_1522">
<caption>Modifiche intervenute dalla versione 1.9.9_1522</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.9-167</td>
<td>v1.9.10-192</td>
<td>Aggiornato per supportare la release Kubernetes 1.9.10. È stata inoltre modificata la configurazione del provider cloud per gestire meglio gli aggiornamenti per i servizi del programma di bilanciamento del carico con `externalTrafficPolicy` impostata su `local`.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>334</td>
<td>338</td>
<td>La versione di incubator è stata aggiornata alla 1.8. Il provisioning dell'archiviazione file viene eseguito alla specifica zona da te selezionata. Non puoi aggiornare delle etichette di un'istanza PV (statica) esistente a meno che tu non stia utilizzando un cluster multizona e debba aggiungere le etichette di regione e zona.<br><br>È stata rimossa la versione NFS predefinita dalle opzioni di montaggio nelle classi di archiviazione file fornite da IBM. Il sistema operativo dell'host ora negozia la versione NFS con il server NFS dell'infrastruttura IBM Cloud (SoftLayer). Per impostare manualmente una specifica versione NFS, o modificare la versione NFS del tuo PV che era stato negoziato dal sistema operativo dell'host, vedi [Modifica della versione NFS predefinita](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.9</td>
<td>v1.9.10</td>
<td>Vedi le [note sulla release Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.10).</td>
</tr>
<tr>
<td>Configurazione di Kubernetes Heapster</td>
<td>N/D</td>
<td>N/D</td>
<td>Sono stati aumentati i limiti di risorsa per il contenitore `heapster-nanny`.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.9.9_1522, rilasciato il 23 agosto 2018
{: #199_1522}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.9_1522.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.9_1521">
<caption>Modifiche intervenute dalla versione 1.9.9_1521</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>`systemd` è stato aggiornato per correggere la perdita `cgroup`.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-3620,CVE-2018-3646 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>


#### Changelog per il fix pack del nodo di lavoro 1.9.9_1521, rilasciato il 13 agosto 2018
{: #199_1521}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.9_1521.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.9_1520">
<caption>Modifiche intervenute dalla versione 1.9.9_1520</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati.</td>
</tr>
</tbody>
</table>

#### Changelog per 1.9.9_1520, rilasciato il 27 luglio 2018
{: #199_1520}

La seguente tabella mostra le modifiche incluse nella patch 1.9.9_1520.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.8_1517">
<caption>Modifiche intervenute dalla versione 1.9.8_1517</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.8-141</td>
<td>v1.9.9-167</td>
<td>Aggiornato per supportare la release Kubernetes 1.9.9. Inoltre, gli eventi `create failure` del servizio LoadBalancer ora includono qualsiasi errore di sottorete portatile.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>320</td>
<td>334</td>
<td>Il timeout per la creazione di volumi persistenti è stato aumentato da 15 a 30 minuti. Il tipo di fatturazione predefinito è stato modificato in oraria (`hourly`). Sono state aggiunte delle opzioni di montaggio alle classi di archiviazione predefinite. Nell'istanza di archiviazione file NFS nel tuo account dell'infrastruttura IBM Cloud (SoftLayer), il campo **Note** è stato modificato in formato JSON ed è stato aggiunto lo spazio dei nomi Kubernetes a cui viene distribuito il PV. Per supportare i cluster multizona, sono state aggiunte le etichette di zona e regione ai volumi persistenti.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.8</td>
<td>v1.9.9</td>
<td>Vedi le [note sulla release Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.9).</td>
</tr>
<tr>
<td>Kernel</td>
<td>N/D</td>
<td>N/D</td>
<td>Miglioramenti secondari alle impostazioni di rete dei nodi di lavoro per i carichi di lavoro di rete ad alte prestazioni.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>La distribuzione del client OpenVPN `vpn` che viene eseguita nello spazio dei nomi `kube-system` è ora gestita dall'`addon-manager` Kubernetes.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.9.8_1517, rilasciato il 3 luglio 2018
{: #198_1517}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.8_1517.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.8_1516">
<caption>Modifiche intervenute dalla versione 1.9.8_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>N/D</td>
<td>N/D</td>
<td>`sysctl` ottimizzato per i carichi di lavoro di rete ad alte prestazioni.</td>
</tr>
</tbody>
</table>


#### Changelog per il fix pack del nodo di lavoro 1.9.8_1516, rilasciato il 21 giugno 2018
{: #198_1516}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.8_1516.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.8_1515">
<caption>Modifiche intervenute dalla versione 1.9.8_1515</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/D</td>
<td>N/D</td>
<td>Per i tipi di macchina non crittografati, il disco secondario viene ripulito ottenendo un file system nuovo quando ricarichi o aggiorni il nodo di lavoro.</td>
</tr>
</tbody>
</table>

#### Changelog per 1.9.8_1515, rilasciato il 19 giugno 2018
{: #198_1515}

La seguente tabella mostra le modifiche incluse nella patch 1.9.8_1515.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.7_1513">
<caption>Modifiche intervenute dalla versione 1.9.7_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.9.8</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.8).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>È stata aggiunta `PodSecurityPolicy` all'opzione `--admission-control` per il server API Kubernetes del cluster ed è stato configurato il cluster per supportare le politiche di sicurezza del pod. Per ulteriori informazioni, vedi [Configurazione delle politiche di sicurezza del pod](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>IBM Cloud Provider</td>
<td>v1.9.7-102</td>
<td>v1.9.8-141</td>
<td>Aggiornato per supportare la release Kubernetes 1.9.8.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato aggiunto <code>livenessProbe</code> alla distribuzione <code>vpn</code> del client OpenVPN che viene eseguita nello spazio dei nomi <code>kube-system</code>.</td>
</tr>
</tbody>
</table>


#### Changelog per il fix pack del nodo di lavoro 1.9.7_1513, rilasciato l'11 giugno 2018
{: #197_1513}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.7_1513.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.7_1512">
<caption>Modifiche intervenute dalla versione 1.9.7_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Aggiornamento kernel</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Nuove immagini di nodo di lavoro con l'aggiornamento kernel per [CVE-2018-3639 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.9.7_1512, rilasciato il 18 maggio 2018
{: #197_1512}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.7_1512.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.7_1511">
<caption>Modifiche intervenute dalla versione 1.9.7_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Correzione per risolvere un bug che si è verificato se hai utilizzato il plugin dell'archiviazione blocchi.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.9.7_1511, rilasciato il 16 maggio 2018
{: #197_1511}

La seguente tabella mostra le modifiche incluse nel fix pack del nodo di lavoro 1.9.7_1511.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.7_1510">
<caption>Modifiche intervenute dalla versione 1.9.7_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>I dati che archivi nella directory root `kubelet` sono ora salvati nel disco secondario più grande della tua macchina del nodo di lavoro.</td>
</tr>
</tbody>
</table>

#### Changelog per 1.9.7_1510, rilasciato il 30 aprile 2018
{: #197_1510}

La seguente tabella mostra le modifiche incluse nella patch 1.9.7_1510.
{: shortdesc}

<table summary="Modifiche intervenute dalla versione 1.9.3_1506">
<caption>Modifiche intervenute dalla versione 1.9.3_1506</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.3</td>
<td>v1.9.7	</td>
<td><p>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7). Questa release risolve le vulnerabilità [CVE-2017-1002101 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) e [CVE-2017-1002102 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p><strong>Nota</strong>: ora `secret`, `configMap`, `downwardAPI` e i volumi previsti sono montati come di sola lettura. In precedenza, le applicazioni potevano scrivere i dati in questi volumi ma il sistema poteva ripristinare i dati automaticamente. Se le tue applicazioni si basano sul comportamento non sicuro precedente, modificale.</p></td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiunto `admissionregistration.k8s.io/v1alpha1=true` all'opzione `--runtime-config` del server dell'API Kubernetes del cluster.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.3-71</td>
<td>v1.9.7-102</td>
<td>I servizi `NodePort` e `LoadBalancer` ora supportano [la conservazione dell'IP di origine client](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) impostando `service.spec.externalTrafficPolicy` su `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corregge la configurazione di tolleranza del [nodo edge](/docs/containers?topic=containers-edge#edge) per i cluster meno recenti.</td>
</tr>
</tbody>
</table>

<br />


### Changelog versione 1.8 (non supportato)
{: #18_changelog}

Esamina i changelog della versione 1.8.
{: shortdesc}

*   [Changelog per il fix pack del nodo di lavoro 1.8.15_1521, rilasciato il 20 settembre 2018](#1815_1521)
*   [Changelog per il fix pack del nodo di lavoro 1.8.15_1520, rilasciato il 23 agosto 2018](#1815_1520)
*   [Changelog per il fix pack del nodo di lavoro 1.8.15_1519, rilasciato il 13 agosto 2018](#1815_1519)
*   [Changelog per 1.8.15_1518, rilasciato il 27 luglio 2018](#1815_1518)
*   [Changelog per il fix pack del nodo di lavoro 1.8.13_1516, rilasciato il 3 luglio 2018](#1813_1516)
*   [Changelog per il fix pack del nodo di lavoro 1.8.13_1515, rilasciato il 21 giugno 2018](#1813_1515)
*   [Changelog 1.8.13_1514, rilasciato il 19 giugno 2018](#1813_1514)
*   [Changelog per il fix pack del nodo di lavoro 1.8.11_1512, rilasciato l'11 giugno 2018](#1811_1512)
*   [Changelog per il fix pack del nodo di lavoro 1.8.11_1511, rilasciato il 18 maggio 2018](#1811_1511)
*   [Changelog per il fix pack del nodo di lavoro 1.8.11_1510, rilasciato il 16 maggio 2018](#1811_1510)
*   [Changelog per 1.8.11_1509, rilasciato il 19 aprile 2018](#1811_1509)

#### Changelog per il fix pack del nodo di lavoro 1.8.15_1521, rilasciato il 20 settembre 2018
{: #1815_1521}

<table summary="Modifiche intervenute dalla versione 1.8.15_1520">
<caption>Modifiche intervenute dalla versione 1.8.15_1520</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Rotazione dei log</td>
<td>N/D</td>
<td>N/D</td>
<td>Si è passato all'utilizzo dei timer `systemd` anziché `cronjobs` per evitare che `logrotate` non funzioni sui nodi di lavoro che non vengono ricaricati o aggiornati entro 90 giorni. **Nota**: in tutte le versioni precedenti per questa release secondaria, il disco primario si riempie dopo che il lavoro cron ha esito negativo perché i log non vengono ruotati. Il lavoro cron ha esito negativo dopo che il nodo di lavoro resta attivo per 90 giorni senza essere aggiornato o ricaricato. Se i log riempiono l'intero disco primario, il nodo di lavoro entra in uno stato non riuscito. Il nodo di lavoro può essere corretto utilizzando il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` o il [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>Componenti di runtime del nodo di lavoro (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/D</td>
<td>N/D</td>
<td>Sono state rimosse le dipendenze dei componenti di runtime sul disco primario. Questo miglioramento impedisce il malfunzionamento dei nodi di lavoro quando il disco primario viene riempito.</td>
</tr>
<tr>
<td>Scadenza della password root</td>
<td>N/D</td>
<td>N/D</td>
<td>Le password root per i nodi di lavoro scadono dopo 90 giorni per motivi di conformità. Se i tuoi strumenti di automazione devono accedere al nodo di lavoro come root o fanno affidamento su lavori cron eseguiti come root, puoi disabilitare la scadenza della password accedendo al nodo di lavoro ed eseguendo `chage -M -1 root`. **Nota**: se hai requisiti di conformità di sicurezza che impediscono l'esecuzione come root o la rimozione della scadenza della password, non disabilitare la scadenza. Puoi invece [aggiornare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) o [ricaricare](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) i tuoi nodi di lavoro almeno ogni 90 giorni.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/D</td>
<td>N/D</td>
<td>Elimina periodicamente le unità di montaggio temporanee per evitare che diventino illimitate. Questa azione risolve il [problema di Kubernetes 57345 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.8.15_1520, rilasciato il 23 agosto 2018
{: #1815_1520}

<table summary="Modifiche intervenute dalla versione 1.8.15_1519">
<caption>Modifiche intervenute dalla versione 1.8.15_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>`systemd` è stato aggiornato per correggere la perdita `cgroup`.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Le immagini del nodo di lavoro sono state aggiornate con l'aggiornamento kernel per [CVE-2018-3620,CVE-2018-3646 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.8.15_1519, rilasciato il 13 agosto 2018
{: #1815_1519}

<table summary="Modifiche intervenute dalla versione 1.8.15_1518">
<caption>Modifiche intervenute dalla versione 1.8.15_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacchetti Ubuntu</td>
<td>N/D</td>
<td>N/D</td>
<td>Aggiornamenti ai pacchetti Ubuntu installati.</td>
</tr>
</tbody>
</table>

#### Changelog per 1.8.15_1518, rilasciato il 27 luglio 2018
{: #1815_1518}

<table summary="Modifiche intervenute dalla versione 1.8.13_1516">
<caption>Modifiche intervenute dalla versione 1.8.13_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.13-176</td>
<td>v1.8.15-204</td>
<td>Aggiornato per supportare la release Kubernetes 1.8.15. Inoltre, gli eventi `create failure` del servizio LoadBalancer ora includono qualsiasi errore di sottorete portatile.</td>
</tr>
<tr>
<td>Plugin {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>320</td>
<td>334</td>
<td>Il timeout per la creazione di volumi persistenti è stato aumentato da 15 a 30 minuti. Il tipo di fatturazione predefinito è stato modificato in oraria (`hourly`). Sono state aggiunte delle opzioni di montaggio alle classi di archiviazione predefinite. Nell'istanza di archiviazione file NFS nel tuo account dell'infrastruttura IBM Cloud (SoftLayer), il campo **Note** è stato modificato in formato JSON ed è stato aggiunto lo spazio dei nomi Kubernetes a cui viene distribuito il PV. Per supportare i cluster multizona, sono state aggiunte le etichette di zona e regione ai volumi persistenti.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.8.13</td>
<td>v1.8.15</td>
<td>Vedi le [note sulla release Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.15).</td>
</tr>
<tr>
<td>Kernel</td>
<td>N/D</td>
<td>N/D</td>
<td>Miglioramenti secondari alle impostazioni di rete dei nodi di lavoro per i carichi di lavoro di rete ad alte prestazioni.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>La distribuzione del client OpenVPN `vpn` che viene eseguita nello spazio dei nomi `kube-system` è ora gestita dall'`addon-manager` Kubernetes.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.8.13_1516, rilasciato il 3 luglio 2018
{: #1813_1516}

<table summary="Modifiche intervenute dalla versione 1.8.13_1515">
<caption>Modifiche intervenute dalla versione 1.8.13_1515</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>N/D</td>
<td>N/D</td>
<td>`sysctl` ottimizzato per i carichi di lavoro di rete ad alte prestazioni.</td>
</tr>
</tbody>
</table>


#### Changelog per il fix pack del nodo di lavoro 1.8.13_1515, rilasciato il 21 giugno 2018
{: #1813_1515}

<table summary="Modifiche intervenute dalla versione 1.8.13_1514">
<caption>Modifiche intervenute dalla versione 1.8.13_1514</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/D</td>
<td>N/D</td>
<td>Per i tipi di macchina non crittografati, il disco secondario viene ripulito ottenendo un file system nuovo quando ricarichi o aggiorni il nodo di lavoro.</td>
</tr>
</tbody>
</table>

#### Changelog 1.8.13_1514, rilasciato il 19 giugno 2018
{: #1813_1514}

<table summary="Modifiche intervenute dalla versione 1.8.11_1512">
<caption>Modifiche intervenute dalla versione 1.8.11_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.11</td>
<td>v1.8.13</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.13).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>È stata aggiunta `PodSecurityPolicy` all'opzione `--admission-control` per il server API Kubernetes del cluster ed è stato configurato il cluster per supportare le politiche di sicurezza del pod. Per ulteriori informazioni, vedi [Configurazione delle politiche di sicurezza del pod](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>IBM Cloud Provider</td>
<td>v1.8.11-126</td>
<td>v1.8.13-176</td>
<td>Aggiornato per supportare la release Kubernetes 1.8.13.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>N/D</td>
<td>N/D</td>
<td>È stato aggiunto <code>livenessProbe</code> alla distribuzione <code>vpn</code> del client OpenVPN che viene eseguita nello spazio dei nomi <code>kube-system</code>.</td>
</tr>
</tbody>
</table>


#### Changelog per il fix pack del nodo di lavoro 1.8.11_1512, rilasciato l'11 giugno 2018
{: #1811_1512}

<table summary="Modifiche intervenute dalla versione 1.8.11_1511">
<caption>Modifiche intervenute dalla versione 1.8.11_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Aggiornamento kernel</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Nuove immagini di nodo di lavoro con l'aggiornamento kernel per [CVE-2018-3639 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>


#### Changelog per il fix pack del nodo di lavoro 1.8.11_1511, rilasciato il 18 maggio 2018
{: #1811_1511}

<table summary="Modifiche intervenute dalla versione 1.8.11_1510">
<caption>Modifiche intervenute dalla versione 1.8.11_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Correzione per risolvere un bug che si è verificato se hai utilizzato il plugin dell'archiviazione blocchi.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.8.11_1510, rilasciato il 16 maggio 2018
{: #1811_1510}

<table summary="Modifiche intervenute dalla versione 1.8.11_1509">
<caption>Modifiche intervenute dalla versione 1.8.11_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>I dati che archivi nella directory root `kubelet` sono ora salvati nel disco secondario più grande della tua macchina del nodo di lavoro.</td>
</tr>
</tbody>
</table>


#### Changelog per 1.8.11_1509, rilasciato il 19 aprile 2018
{: #1811_1509}

<table summary="Modifiche intervenute dalla versione 1.8.8_1507">
<caption>Modifiche intervenute dalla versione 1.8.8_1507</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td><p>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11). Questa release risolve le vulnerabilità [CVE-2017-1002101 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) e [CVE-2017-1002102 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p>Ora `secret`, `configMap`, `downwardAPI` e i volumi previsti sono montati come di sola lettura. In precedenza, le applicazioni potevano scrivere i dati in questi volumi ma il sistema poteva ripristinare i dati automaticamente. Se le tue applicazioni si basano sul comportamento non sicuro precedente, modificale.</p></td>
</tr>
<tr>
<td>Sospensione immagine contenitore</td>
<td>3.0</td>
<td>3.1</td>
<td>Rimuove i processi zombie orfani ereditati.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>I servizi `NodePort` e `LoadBalancer` ora supportano [la conservazione dell'IP di origine client](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) impostando `service.spec.externalTrafficPolicy` su `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corregge la configurazione di tolleranza del [nodo edge](/docs/containers?topic=containers-edge#edge) per i cluster meno recenti.</td>
</tr>
</tbody>
</table>

<br />


### Changelog versione 1.7 (non supportato)
{: #17_changelog}

Esamina i changelog della versione 1.7.
{: shortdesc}

*   [Changelog per il fix pack del nodo di lavoro 1.7.16_1514, rilasciato l'11 giugno 2018](#1716_1514)
*   [Changelog per il fix pack del nodo di lavoro 1.7.16_1513, rilasciato il 18 maggio 2018](#1716_1513)
*   [Changelog per il fix pack del nodo di lavoro 1.7.16_1512, rilasciato il 16 maggio 2018](#1716_1512)
*   [Changelog per 1.7.16_1511, rilasciato il 19 aprile 2018](#1716_1511)

#### Changelog per il fix pack del nodo di lavoro 1.7.16_1514, rilasciato l'11 giugno 2018
{: #1716_1514}

<table summary="Modifiche intervenute dalla versione 1.7.16_1513">
<caption>Modifiche intervenute dalla versione 1.7.16_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Aggiornamento kernel</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Nuove immagini di nodo di lavoro con l'aggiornamento kernel per [CVE-2018-3639 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.7.16_1513, rilasciato il 18 maggio 2018
{: #1716_1513}

<table summary="Modifiche intervenute dalla versione 1.7.16_1512">
<caption>Modifiche intervenute dalla versione 1.7.16_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Correzione per risolvere un bug che si è verificato se hai utilizzato il plugin dell'archiviazione blocchi.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.7.16_1512, rilasciato il 16 maggio 2018
{: #1716_1512}

<table summary="Modifiche intervenute dalla versione 1.7.16_1511">
<caption>Modifiche intervenute dalla versione 1.7.16_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>I dati che archivi nella directory root `kubelet` sono ora salvati nel disco secondario più grande della tua macchina del nodo di lavoro.</td>
</tr>
</tbody>
</table>

#### Changelog per 1.7.16_1511, rilasciato il 19 aprile 2018
{: #1716_1511}

<table summary="Modifiche intervenute dalla versione 1.7.4_1509">
<caption>Modifiche intervenute dalla versione 1.7.4_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td><p>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16). Questa release risolve le vulnerabilità [CVE-2017-1002101 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) e [CVE-2017-1002102 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p>Ora `secret`, `configMap`, `downwardAPI` e i volumi previsti sono montati come di sola lettura. In precedenza, le applicazioni potevano scrivere i dati in questi volumi ma il sistema poteva ripristinare i dati automaticamente. Se le tue applicazioni si basano sul comportamento non sicuro precedente, modificale.</p></td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>I servizi `NodePort` e `LoadBalancer` ora supportano [la conservazione dell'IP di origine client](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) impostando `service.spec.externalTrafficPolicy` su `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corregge la configurazione di tolleranza del [nodo edge](/docs/containers?topic=containers-edge#edge) per i cluster meno recenti.</td>
</tr>
</tbody>
</table>
