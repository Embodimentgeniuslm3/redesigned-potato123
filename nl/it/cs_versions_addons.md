---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, nginx, ingress controller

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


# Changelog Fluentd e ALB Ingress
{: #cluster-add-ons-changelog}

Il tuo cluster {{site.data.keyword.containerlong}} viene fornito con componenti quali Fluentd e Ingress ALB, che vengono aggiornati automaticamente da IBM. Puoi anche disabilitare gli aggiornamenti automatici per alcuni componenti e aggiornarli manualmente e separatamente dai nodi master e di lavoro. Fai riferimento alle tabelle delle seguenti sezioni per un riepilogo delle modifiche per ogni versione.
{: shortdesc}

Per ulteriori informazioni sulla gestione degli aggiornamenti per Fluentd e gli ALB Ingress, vedi [Aggiornamento dei componenti cluster](/docs/containers?topic=containers-update#components).

## Changelog ALB Ingress
{: #alb_changelog}

Visualizza le modifiche alla versione di build per gli ALB (application load balancer) Ingress nei tuoi cluster {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Quando il componente ALB Ingress viene aggiornato, i contenitori `nginx-ingress` e `ingress-auth` di tutti i pod ALB vengono aggiornati all'ultima versione di build. Per impostazione predefinita, gli aggiornamenti automatici del componente sono abilitati, ma puoi disabilitarli e aggiornare il componente manualmente. Per ulteriori informazioni, vedi [Aggiornamento dell'ALB (application load balancer) Ingress](/docs/containers?topic=containers-update#alb).

Fai riferimento alla seguente tabella per un riepilogo delle modifiche per ogni build del componente ALB Ingress.

<table summary="Panoramica delle modifiche della build del componente ALB (application load balancer) Ingress">
<caption>Changelog per il componente ALB (application load balancer) Ingress</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>Build `nginx-ingress` / `ingress-auth`</th>
<th>Data di rilascio</th>
<th>Modifiche senza interruzione del servizio</th>
<th>Modifiche con interruzione del servizio</th>
</tr>
</thead>
<tbody>
<tr>
<td>470/330</td>
<td>7 giugno 2019</td>
<td>Corregge le vulnerabilità del DB Berkeley per [CVE-2019-8457 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457).
</td>
<td>-</td>
</tr>
<tr>
<td>470/329</td>
<td>6 giugno 2019</td>
<td>Corregge le vulnerabilità del DB Berkeley per [CVE-2019-8457 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457).
</td>
<td>-</td>
</tr>
<tr>
<td>467/329</td>
<td>3 giugno 2019</td>
<td>Corregge le vulnerabilità GnuTLS per [CVE-2019-3829 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-3893 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3893), [CVE-2018-10844 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10845 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844) e [CVE-2018-10846 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846).
</td>
<td>-</td>
</tr>
<tr>
<td>462/329</td>
<td>28 maggio 2019</td>
<td>Corregge le vulnerabilità cURL per [CVE-2019-5435 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) e [CVE-2019-5436 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
<td>-</td>
</tr>
<tr>
<td>457/329</td>
<td>23 maggio 2019</td>
<td>Corregge le vulnerabilità Go per le scansioni di immagini.</td>
<td>-</td>
</tr>
<tr>
<td>423/329</td>
<td>13 maggio 2019</td>
<td>Migliora le prestazioni per l'integrazione con {{site.data.keyword.appid_full}}.</td>
<td>-</td>
</tr>
<tr>
<td>411 / 315</td>
<td>15 aprile 2019</td>
<td>Aggiorna il valore della scadenza del cookie {{site.data.keyword.appid_full_notm}} in modo che corrisponda al valore della scadenza del token di accesso.</td>
<td>-</td>
</tr>
<tr>
<td>411 / 306</td>
<td>22 marzo 2019</td>
<td>Aggiorna la versione di Go alla 1.12.1.</td>
<td>-</td>
</tr>
<tr>
<td>410 / 305</td>
<td>18 marzo 2019</td>
<td><ul>
<li>Corregge le vulnerabilità per le scansioni di immagini.</li>
<li>Migliora la registrazione per l'integrazione con {{site.data.keyword.appid_full_notm}}.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>408 / 304</td>
<td>05 marzo 2019</td>
<td>-</td>
<td>Corregge i bug nell'integrazione di autorizzazione correlata alla funzionalità di disconnessione, alla scadenza dei token e al callback delle autorizzazioni `OAuth`. Queste correzioni sono implementate solo se hai abilitato l'autorizzazione {{site.data.keyword.appid_full_notm}} utilizzando l'annotazione [`appid-auth`](/docs/containers?topic=containers-ingress_annotation#appid-auth). Per implementare queste correzioni, vengono aggiunte delle intestazioni aggiuntive, e questo aumenta la dimensione totale delle intestazioni. A seconda della dimensione delle tue intestazioni e della dimensione totale delle risposte, potresti aver bisogno di regolare le [annotazioni di buffer proxy](/docs/containers?topic=containers-ingress_annotation#proxy-buffer) che utilizzi.</td>
</tr>
<tr>
<td>406 / 301</td>
<td>19 febbraio 2019</td>
<td><ul>
<li>Aggiorna la versione di Go alla 1.11.5.</li>
<li>Corregge le vulnerabilità per le scansioni di immagini.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>404 / 300</td>
<td>31 gennaio 2019</td>
<td>Aggiorna la versione di Go alla 1.11.4.</td>
<td>-</td>
</tr>
<tr>
<td>403 / 295</td>
<td>23 gennaio 2019</td>
<td><ul>
<li>Aggiornamenti alla versione NGINX degli ALB alla 1.15.2.</li>
<li>I certificati TLS forniti da IBM sono ora rinnovati automaticamente 37 giorni prima che scadano invece di 7 giorni.</li>
<li>Aggiunge la funzionalità di disconnessione {{site.data.keyword.appid_full_notm}}: se `/logout` è presente in un percorso {{site.data.keyword.appid_full_notm}}, i cookie vengono rimossi e l'utente viene inviato nuovamente alla pagina di accesso.</li>
<li>Aggiunge un'intestazione alle richieste {{site.data.keyword.appid_full_notm}} per scopi di traccia interna.</li>
<li>Aggiorna la direttiva dell'ubicazione {{site.data.keyword.appid_short_notm}} in modo che l'annotazione `app-id` possa essere utilizzata insieme alle annotazioni `proxy-buffers`, `proxy-buffer-size` e `proxy-busy-buffer-size`.</li>
<li>Corregge un bug in modo che i log informativi non vengano etichettati come errori.</li>
</ul></td>
<td>Disabilita TLS 1.0 e 1.1 per impostazione predefinita. Se i client che si connettono alle tue applicazioni supportano TLS 1.2, non è richiesta alcuna azione. Se hai ancora dei client legacy che richiedono il supporto TLS 1.0 o 1.1, abilita manualmente le versioni TLS richieste attenendoti a [questa procedura](/docs/containers?topic=containers-ingress#ssl_protocols_ciphers). Per ulteriori informazioni su come visualizzare le versioni TLS utilizzate dai tuoi client per accedere alle applicazioni, vedi questo [post del blog {{site.data.keyword.Bluemix_notm}}](https://www.ibm.com/blogs/bluemix/2018/11/ibm-cloud-kubernetes-service-alb-update-tls-1-0-and-1-1-disabled-by-default/).</td>
</tr>
<tr>
<td>393 / 291</td>
<td>09 gennaio 2019</td>
<td>Aggiunge supporto per l'integrazione di più istanze {{site.data.keyword.appid_full_notm}}.</td>
<td>-</td>
</tr>
<tr>
<td>393 / 282</td>
<td>29 novembre 2018</td>
<td>Migliora le prestazioni per l'integrazione con {{site.data.keyword.appid_full_notm}}.</td>
<td>-</td>
</tr>
<tr>
<td>384 / 246</td>
<td>14 novembre 2018</td>
<td>Migliora le funzioni di registrazione e disconnessione per l'integrazione con {{site.data.keyword.appid_full_notm}}.</td>
<td>Sostituisce il certificato autofirmato per `*.containers.mybluemix.net` con il certificato firmato da LetsEncrypt che viene generato automaticamente e utilizzato dal cluster. Il certificato autofirmato `*.containers.mybluemix.net` viene rimosso.</td>
</tr>
<tr>
<td>350 / 192</td>
<td>05 novembre 2018</td>
<td>Aggiunge il supporto per abilitare e disabilitare gli aggiornamenti automatici del componente ALB Ingress.</td>
<td>-</td>
</tr>
</tbody>
</table>

## Changelog del Fluentd per la registrazione
{: #fluentd_changelog}

Visualizza le modifiche alla versione di build per il componente Fluentd per la registrazione nei tuoi cluster {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Per impostazione predefinita, gli aggiornamenti automatici del componente sono abilitati, ma puoi disabilitarli e aggiornare il componente manualmente. Per ulteriori informazioni, vedi [Gestione degli aggiornamenti automatici per Fluentd](/docs/containers?topic=containers-update#logging-up).

Fai riferimento alla seguente tabella per un riepilogo delle modifiche per ciascuna build del componente Fluentd.

<table summary="Panoramica delle modifiche della build per il componente Fluentd">
<caption>Changelog per il componente Fluentd</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>Build Fluentd</th>
<th>Data di rilascio</th>
<th>Modifiche senza interruzione del servizio</th>
<th>Modifiche con interruzione del servizio</th>
</tr>
</thead>
<tr>
<td>e7c10d74350dc64d4d92ba7f72bb4ff9219315d2</td>
<td>30 maggio 2019</td>
<td>Aggiorna la mappa di configurazione Fluentd in modo che ignori sempre i log dei pod degli spazi dei nomi IBM, anche quando il master Kubernetes non è disponibile.</td>
<td>-</td>
</tr>
<tr>
<td>c16fe1602ab65db4af0a6ac008f99ca2a526e6f6</td>
<td>21 maggio 2019</td>
<td>Corregge un bug in cui la metrica del nodo di lavoro non era stata visualizzata.</td>
<td>-</td>
</tr>
<tr>
<td>60fc11f7bd39d9c6cfed923c598bf6457b3f2037</td>
<td>10 maggio 2019</td>
<td>Aggiorna i pacchetti Ruby per [CVE-2019-8320 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320), [CVE-2019-8321 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321), [CVE-2019-8322 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322), [CVE-2019-8323 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323), [CVE-2019-8324 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) e [CVE-2019-8325 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325).</td>
<td>-</td>
</tr>
<tr>
<td>91a737f68f7d9e81b5d2223c910aaa7d7f91b76d</td>
<td>8 maggio 2019</td>
<td>Aggiorna i pacchetti Ruby per [CVE-2019-8320 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320), [CVE-2019-8321 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321), [CVE-2019-8322 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322), [CVE-2019-8323 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323), [CVE-2019-8324 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) e [CVE-2019-8325 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325).</td>
<td>-</td>
</tr>
<tr>
<td>d9af69e286986a05ed4a50469585b1cf978ddb1d</td>
<td>11 aprile 2019</td>
<td>Aggiorna il plug-in cAdvisor in modo che utilizzi TLS 1.2.</td>
<td>-</td>
</tr>
<tr>
<td>3100ddb62580a9f46ffdff7bab2ebec40b164de6</td>
<td>1 aprile 2019</td>
<td>Aggiorna l'account dei servizi Fluentd.</td>
<td>-</td>
</tr>
<tr>
<td>c85567b75bd7ad1c9428794cd63a8e239c3fd8f5</td>
<td>18 marzo 2019</td>
<td>Rimuove la dipendenza da cURL per [CVE-2019-8323 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323).</td>
<td>-</td>
</tr>
<tr>
<td>320ffdf87de068ee2f7f34c0e7a47a111e8d457b</td>
<td>18 febbraio 2019</td>
<td><ul>
<li>Aggiorna Fluend alla versione 1.3.</li>
<li>Rimuove Git dall'immagine Fluentd per [CVE-2018-19486 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19486).</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>972865196aefd3324105087878de12c518ed579f</td>
<td>1 gennaio 2019</td>
<td><ul>
<li>Abilita la codifica UTF-8 per il plug-in Fluentd `in_tail`.</li>
<li>Correzioni di bug secondari.</li>
</ul></td>
<td>-</td>
</tr>
</tbody>
</table>
