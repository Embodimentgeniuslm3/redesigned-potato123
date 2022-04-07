---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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


# Configura il provider DNS del cluster
{: #cluster_dns}

A ogni servizio nel tuo cluster {{site.data.keyword.containerlong}} viene assegnato un nome DNS (Domain Name System) che il provider DNS del cluster registra per risolvere le richieste DNS. A seconda della versione Kubernetes del tuo cluster, puoi scegliere tra DNS Kubernetes (KubeDNS) o [CoreDNS ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://coredns.io/). Per ulteriori informazioni sul DNS per i servizi e i pod, consulta [la documentazione di Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/).
{: shortdesc}

**Quali provider DNS del cluster sono supportati dalle diverse versioni Kubernetes?**<br>

| Versione Kubernetes | Predefinito per i nuovi cluster | Descrizione |
|---|---|---|
| 1.14 e successive | CoreDNS | Se un cluster utilizza KubeDNS e viene aggiornato alla versione 1.14 o successiva da una versione precedente, il provider DNS del cluster viene migrato automaticamente da KubeDNS a CoreDNS durante l'aggiornamento. Non puoi riportare il provider DNS del cluster a KubeDNS. |
| 1.13 | CoreDNS | I cluster che vengono aggiornati alla versione 1.13 da una versione precedente mantengono qualsiasi provider DNS utilizzato al momento dell'aggiornamento. Se vuoi utilizzarne uno diverso, [cambia il provider DNS](#dns_set). |
| 1.12 | KubeDNS | Per utilizzare invece CoreDNS, [cambia il provider DNS](#set_coredns). |
| 1.11 e precedenti | KubeDNS | Non puoi cambiare il provider DNS in CoreDNS. |
{: caption="Provider DNS predefinito del cluster per versione Kubernetes" caption-side="top"}

**Quali sono i vantaggi dell'utilizzo di CoreDNS al posto di KubeDNS?**<br>
CoreDNS è il provider DNS di cluster supportato predefinito per Kubernetes versione 1.13 e successive e, recentemente, è diventato un [Graduated Project CNCF (Cloud Native Computing Foundation)![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.cncf.io/projects/). I graduated project vengono testati approfonditamente, hanno una protezione avanzata e sono pronti per l'adozione a livello di produzione su larga scala.

Come osservato nell'[annuncio di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/blog/2018/12/03/kubernetes-1-13-release-announcement/), CoreDNS è un server DNS autorevole di uso generico, che fornisce un'integrazione con Kubernetes compatibile con le versioni precedenti, ma ampliabile. Poiché CoreDNS è un unico eseguibile e un unico processo, ha meno dipendenze e parti mobili che potrebbero avere problemi rispetto al precedente provider DNS del cluster. Il progetto è scritto con lo stesso linguaggio del progetto Kubernetes, `Go`, che aiuta a proteggere la memoria. Infine, CoreDNS supporta casi di utilizzo più flessibili rispetto a KubeDNS, perché permette di creare voci DNS personalizzate quali le [configurazioni comuni dei documenti CoreDNS![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://coredns.io/manual/toc/#setups).

## Ridimensionamento automatico del provider DNS del cluster
{: #dns_autoscale}

Per impostazione predefinita, il provider DNS del cluster {{site.data.keyword.containerlong_notm}} include una distribuzione per ridimensionare automaticamente i pod DNS in risposta al numero di nodi di lavoro e core all'interno del cluster. Puoi ottimizzare i parametri dell'autoscaler DNS modificando la mappa di configurazione del ridimensionamento automatico DNS. Ad esempio, se le tue applicazioni fanno un uso elevato del provider DNS del cluster, potresti dover aumentare il numero minimo di pod DNS per supportare l'applicazione. Per ulteriori informazioni, consulta la [documentazione Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/).
{: shortdesc}

Prima di iniziare: [accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Verifica che la distribuzione del provider DNS del cluster sia disponibile. Nel tuo cluster, potresti avere installato l'autoscaler per KubeDNS, CoreDNS o per entrambi i provider DNS. Se sono installati entrambi gli autoscaler DNS, trova quello in uso controllando la colonna **AVAILABLE** nell'output della tua CLI. La distribuzione in uso è elencata con un'unica distribuzione disponibile.
    ```
    kubectl get deployment -n kube-system | grep dns-autoscaler
    ```
    {: pre}

    Output di esempio:
    ```
    NAME                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    coredns-autoscaler     0         0         0            0           32d
    kube-dns-autoscaler    1         1         1            1           260d
    ```
    {: screen}
2.  Ottieni il nome della mappa di configurazione per i parametri dell'autoscaler DNS.
    ```
    kubectl get configmap -n kube-system
    ```
    {: pre}    
3.  Modifica le impostazioni predefinite per l'autoscaler DNS. Cerca il campo `data.linear`, che ha come valore predefinito un pod DNS ogni 16 nodi di lavoro o 256 core, con un minimo di due pod DNS, indipendentemente dalle dimensioni del cluster (`preventSinglePointFailure: true`). Per ulteriori informazioni, consulta la [documentazione Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/#tuning-autoscaling-parameters).
    ```
    kubectl edit configmap -n kube-system <dns-autoscaler>
    ```
    {: pre}

    Output di esempio:
    ```
    apiVersion: v1
    data:
      linear: '{"coresPerReplica":256,"nodesPerReplica":16,"preventSinglePointFailure":true}'
    kind: ConfigMap
    metadata:
    ...
    ```
    {: screen}

## Personalizzazione del provider DNS del cluster
{: #dns_customize}

Puoi personalizzare il tuo provider DNS del cluster {{site.data.keyword.containerlong_notm}} modificando la mappa di configurazione DNS. Ad esempio, potresti voler configurare gli `stubdomains` e i server dei nomi upstream per risolvere i servizi che puntano a host esterni. Inoltre, se utilizzi CoreDNS, puoi configurare più [Corefile ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://coredns.io/2017/07/23/corefile-explained/) all'interno della mappa di configurazione di CoreDNS. Per ulteriori informazioni, consulta la [documentazione Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/).
{: shortdesc}

Prima di iniziare: [accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Verifica che la distribuzione del provider DNS del cluster sia disponibile. Nel tuo cluster, potresti avere installato il provider di cluster DNS per KubeDNS, CoreDNS o entrambi i provider DNS. Se sono installati entrambi i provider DNS, trova quello in uso controllando la colonna **AVAILABLE** nell'output della tua CLI. La distribuzione in uso è elencata con un'unica distribuzione disponibile.
    ```
    kubectl get deployment -n kube-system -l k8s-app=kube-dns
    ```
    {: pre}

    Output di esempio:
    ```
    NAME                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    coredns                0         0         0            0           89d
    kube-dns-amd64         2         2         2            2           89d
    ```
    {: screen}
2.  Modifica le impostazioni predefinite per la mappa di configurazione di CoreDNS o KubeDNS.

    *   **Per CoreDNS**: utilizza un Corefile nella sezione `data` della mappa di configurazione per personalizzare gli `stubdomains` e i server dei nomi upstream. Per ulteriori informazioni, consulta la [documentazione Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns).
        ```
        kubectl edit configmap -n kube-system coredns
        ```
        {: pre}

        **Output di esempio per CoreDNS**:
          ```
          apiVersion: v1
          kind: ConfigMap
          metadata:
            name: coredns
            namespace: kube-system
          data:
            Corefile: |
              abc.com:53 {
                  errors
                  cache 30
                  proxy . 1.2.3.4
              }
              .:53 {
                  errors
                  health
                  kubernetes cluster.local in-addr.arpa ip6.arpa {
                     pods insecure
                     upstream 172.16.0.1
                     fallthrough in-addr.arpa ip6.arpa
                  }
                  prometheus :9153
                  proxy . /etc/resolv.conf
                  cache 30
                  loop
                  reload
                  loadbalance
              }
          ```
          {: screen}

          Hai molte personalizzazioni che vuoi organizzare? In Kubernetes versione 1.12.6_1543 e successive, puoi aggiungere più Corefile alla mappa di configurazione di CoreDNS. Per ulteriori informazioni, consulta la [documentazione sull'importazione di Corefile ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://coredns.io/plugins/import/).
          {: tip}

    *   **Per KubeDNS**: configura gli `stubdomains` e i server dei nomi upstream nella sezione `data` della mappa di configurazione. Per ulteriori informazioni, consulta la [documentazione Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#kube-dns).
        ```
        kubectl edit configmap -n kube-system kube-dns
        ```
        {: pre}

        **Output di esempio per KubeDNS**:
        ```
        apiVersion: v1
        kind: ConfigMap
        metadata:
          name: kube-dns
          namespace: kube-system
        data:
          stubDomains: |
          {"abc.com": ["1.2.3.4"]}
        ```
        {: screen}

## Impostazione del provider DNS del cluster su CoreDNS o KubeDNS
{: #dns_set}

Se hai un cluster {{site.data.keyword.containerlong_notm}} che esegue Kubernetes versione 1.12 o 1.13, puoi scegliere di utilizzare il DNS Kubernetes (KubeDNS) o CoreDNS come provider DNS del cluster.
{: shortdesc}

I cluster che eseguono altre versioni Kubernetes non possono impostare il provider DNS del cluster. Le versioni 1.11 e precedenti supportano solo KubeDNS e le versioni 1.14 e successive supportano solo CoreDNS.
{: note}

**Prima di iniziare**:
1.  [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2.  Determina il provider DNS del cluster corrente. Nel seguente esempio, KubeDNS è il provider DNS corrente del cluster.
    ```
    kubectl cluster-info
    ```
    {: pre}

    Output di esempio:
    ```
    ...
    KubeDNS is running at https://c2.us-south.containers.cloud.ibm.com:20190/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
    ...
    ```
    {: screen}
3.  In base al provider DNS utilizzato dal tuo cluster, segui la procedura per cambiare i provider DNS.
    *  [Passa all'uso di CoreDNS](#set_coredns).
    *  [Passa all'uso di KubeDNS](#set_kubedns).

### Configurazione di CoreDNS come provider DNS del cluster
{: #set_coredns}

Configura CoreDNS invece di KubeDNS come provider DNS del cluster.
{: shortdesc}

1.  Se hai personalizzato la mappa di configurazione del provider KubeDNS o dell'autoscaler KubeDNS, trasferisci eventuali personalizzazioni alle mappe di configurazione di CoreDNS.
    *   Per la mappa di configurazione `kube-dns` nello spazio dei nomi `kube-system`, trasferisci qualsiasi [personalizzazione DNS ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/) alla mappa di configurazione `coredns` nello spazio dei nomi `kube-system`. La sintassi differisce per le mappe di configurazione `kube-dns` e `coredns`. Per un esempio, consulta la [documentazione Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns-configuration-equivalent-to-kube-dns).
    *   Per la mappa di configurazione `kube-dns-autoscaler` nello spazio dei nomi `kube-system`, trasferisci qualsiasi [personalizzazione dell'autoscaler DNS ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/) alla mappa di configurazione `coredns-autoscaler` nello spazio dei nomi `kube-system`. La sintassi di personalizzazione è la stessa per entrambi.
2.  Riduci la distribuzione dell'autoscaler KubeDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=0 kube-dns-autoscaler
    ```
    {: pre}
3.  Controlla e attendi che i pod vengano eliminati.
    ```
    kubectl get pods -n kube-system -l k8s-app=kube-dns-autoscaler
    ```
    {: pre}
4.  Riduci la distribuzione di KubeDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=0 kube-dns-amd64
    ```
    {: pre}
5.  Amplia la distribuzione dell'autoscaler CoreDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=1 coredns-autoscaler
    ```
    {: pre}
6.  Etichetta e annota il servizio DNS del cluster per CoreDNS.
    ```
    kubectl label service --overwrite -n kube-system kube-dns kubernetes.io/name=CoreDNS
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/port=9153
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/scrape=true
    ```
    {: pre}
7.  **Facoltativo**: se prevedi di utilizzare Prometheus per raccogliere le metriche dai pod CoreDNS, devi aggiungere una porta di metriche al servizio `kube-dns` da cui stai passando.
    ```
    kubectl -n kube-system patch svc kube-dns --patch '{"spec": {"ports": [{"name":"metrics","port":9153,"protocol":"TCP"}]}}' --type strategic
    ```
    {: pre}



### Configurazione di KubeDNS come provider DNS del cluster
{: #set_kubedns}

Configura KubeDNS invece di CoreDNS come provider DNS del cluster.
{: shortdesc}

1.  Se hai personalizzato la mappa di configurazione del provider CoreDNS o dell'autoscaler CoreDNS, trasferisci eventuali personalizzazioni alle mappe di configurazione di KubeDNS.
    *   Per la mappa di configurazione `coredns` nello spazio dei nomi `kube-system`, trasferisci qualsiasi [personalizzazione DNS ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/) alla mappa di configurazione `kube-dns` nello spazio dei nomi `kube-system`. La sintassi differisce tra le mappe di configurazione `kube-dns` e `coredns`. Per un esempio, consulta la [documentazione Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns-configuration-equivalent-to-kube-dns).
    *   Per la mappa di configurazione `coredns-autoscaler` nello spazio dei nomi `kube-system`, trasferisci qualsiasi [personalizzazione dell'autoscaler DNS ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/) alla mappa di configurazione `kube-dns-autoscaler` nello spazio dei nomi `kube-system`. La sintassi di personalizzazione è la stessa per entrambi.
2.  Riduci la distribuzione dell'autoscaler CoreDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=0 coredns-autoscaler
    ```
    {: pre}
3.  Controlla e attendi che i pod vengano eliminati.
    ```
    kubectl get pods -n kube-system -l k8s-app=coredns-autoscaler
    ```
    {: pre}
4.  Riduci la distribuzione di CoreDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=0 coredns
    ```
    {: pre}
5.  Amplia la distribuzione dell'autoscaler KubeDNS.
    ```
    kubectl scale deployment -n kube-system --replicas=1 kube-dns-autoscaler
    ```
    {: pre}
6.  Etichetta e annota il servizio DNS del cluster per KubeDNS.
    ```
    kubectl label service --overwrite -n kube-system kube-dns kubernetes.io/name=KubeDNS
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/port-
    ```
    {: pre}
    ```
    kubectl annotate service --overwrite -n kube-system kube-dns prometheus.io/scrape-
    ```
    {: pre}
7.  **Facoltativo**: se hai utilizzato Prometheus per raccogliere le metriche dai pod CoreDNS, il tuo servizio `kube-dns` aveva una porta di metriche. Tuttavia, KubeDNS non ha bisogno di includere questa porta di metriche, per cui puoi rimuovere la porta dal servizio.
    ```
    kubectl -n kube-system patch svc kube-dns --patch '{"spec": {"ports": [{"name":"dns","port":53,"protocol":"UDP"},{"name":"dns-tcp","port":53,"protocol":"TCP"}]}}' --type merge
    ```
    {: pre}
