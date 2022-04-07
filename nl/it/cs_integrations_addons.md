---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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

# Aggiunta di servizi tramite componenti aggiuntivi gestiti
{: #managed-addons}

Aggiunta rapida di tecnologie open source al tuo cluster con componenti aggiuntivi gestiti.
{: shortdesc}

**Cosa sono i componenti aggiuntivi gestiti?** </br>
I componenti aggiuntivi {{site.data.keyword.containerlong_notm}} gestiti rappresentano un modo facile per migliorare il tuo cluster con funzionalità open source, quali Istio o Knative. La versione dello strumento open source che aggiungi al tuo cluster viene testata da IBM e approvata perl'uso in {{site.data.keyword.containerlong_notm}}.

**Come funzionano la fatturazione e il supporto dei componenti aggiuntivi gestiti?** </br>
I componenti aggiuntivi gestiti sono pienamente integrati nell'organizzazione di supporto {{site.data.keyword.Bluemix_notm}}. Se hai domande o riscontri un problema nell'utilizzo dei componenti aggiuntivi gestiti, puoi utilizzare uno dei canali di supporto {{site.data.keyword.containerlong_notm}}. Per ulteriori informazioni, vedi [Come ottenere aiuto e supporto](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

Se lo strumento che aggiungi al tuo cluster comporta dei costi, questi vengono automaticamente integrati ed elencati all'interno della come parte della tua fatturazione {{site.data.keyword.containerlong_notm}}. {{site.data.keyword.Bluemix_notm}} determina il ciclo di fatturazione sulla base della data di abilitazione del componente aggiuntivo nel tuo cluster.

**Di quali limitazioni devo tenere conto?** </br>
Se hai installato il [controller di ammissione Container image security enforcer](/docs/services/Registry?topic=registry-security_enforce#security_enforce) nel tuo cluster, non puoi abilitarvi alcun componente aggiuntivo gestito.

## Aggiunta di componenti aggiuntivi gestiti
{: #adding-managed-add-ons}

Per abilitare un componente aggiuntivo gestito nel tuo cluster, utilizza il [comando `ibmcloud ks cluster-addon-enable <addon_name> --cluster <cluster_name_or_ID>`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable). Quando abiliti il componente aggiuntivo gestito, viene automaticamente installata nel tuo cluster una versione supportata dello strumento, comprendente tutte le risorse Kubernetes. Fai riferimento alla documentazione di ciascun componente aggiuntivo gestito per trovare i prerequisiti che il tuo cluster deve soddisfare per installarlo.

Per ulteriori informazioni sui prerequisiti per ciascun componente aggiuntivo, vedi:

- [Istio](/docs/containers?topic=containers-istio#istio)
- [Knative](/docs/containers?topic=containers-serverless-apps-knative)

## Aggiornamento di componenti aggiuntivi gestiti
{: #updating-managed-add-ons}

Le versioni di ciascun componente aggiuntivo gestito vengono testate da {{site.data.keyword.Bluemix_notm}} e approvate per l'uso in {{site.data.keyword.containerlong_notm}}. Per aggiornare i componenti di un componente aggiuntivo alla versione più recente supportata da {{site.data.keyword.containerlong_notm}}, utilizza la seguente procedura.
{: shortdesc}

1. Controlla se i tuoi componenti aggiuntivi sono aggiornati all'ultima versione. È possibile aggiornare qualsiasi componente aggiuntivo contrassegnato con `* (<version> più recente)`.
   ```
   ibmcloud ks cluster-addons --cluster <mycluster>
   ```
   {: pre}

   Output di esempio:
   ```
   OK
   Name      Version
   istio     1.1.5
   knative   0.5.2
   ```
   {: screen}

2. Salva qualsiasi risorsa, quale i file di configurazione di un qualsiasi servizio o applicazione, che hai creato o modificato nello spazio dei nomi generato dal componente aggiuntivo. Ad esempio, il componente aggiuntivo Istio utilizza `istio-system` e il componente aggiuntivo Knative utilizza `knative-serving`, `knative-monitoring`, `knative-eventing` e `knative-build`.
   Comando di esempio:
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

3. Salva le risorse Kubernetes generate automaticamente nello spazio dei nomi del componente aggiuntivo gestito in un file YAML sulla tua macchina locale. Queste risorse vengono generate attraverso definizioni di risorse personalizzate.
   1. Ottieni le definizioni di risorse personalizzate per il tuo componente aggiuntivo dallo spazio dei nomi utilizzato da tale componente. Ad esempio, per il componente aggiuntivo Istio, ottieni le definizioni di risorse personalizzate dallo spazio dei nomi `istio-system`.
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. Salva le risorse create da queste definizioni di risorse personalizzate.

4. Facoltativo per Knative: se hai modificato una qualsiasi delle seguenti risorse, prendi il file YAML e salvalo nella tua macchina locale. Se hai modificato una qualsiasi di queste risorse, ma desideri invece utilizzare i valori predefiniti installati, puoi eliminare la risorsa. Dopo alcuni minuti, la risorsa viene ricreata con i valori predefiniti installati.
  <table summary="Tabella di risorse Knative">
  <caption>Risorse Knative</caption>
  <thead><tr><th>Nome della risorsa</th><th>Tipo di risorsa</th><th>Spazio dei nomi</th></tr></thead>
  <tbody>
  <tr><td><code>config-autoscaler</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-controller</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-domain</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-gc</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-istio</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-logging</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-network</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-observability</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>kaniko</code></td><td>BuildTemplate</td><td><code>default</code></td></tr>
  <tr><td><code>iks-knative-ingress</code></td><td>Ingress</td><td><code>istio-system</code></td></tr>
  </tbody></table>

  Comando di esempio:
  ```
  kubectl get cm config-autoscaler -n knative-serving -o yaml > config-autoscaler.yaml
  ```
  {: pre}

5. Se hai abilitato i componenti aggiuntivi `istio-sample-bookinfo` e `istio-extras`, disabilitali.
   1. Disabilita il componente aggiuntivo `istio-sample-bookinfo`.
      ```
      ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
      ```
      {: pre}

   2. Disabilita il componente aggiuntivo `istio-extras`.
      ```
      ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
      ```
      {: pre}

6. Disabilita il componente aggiuntivo.
   ```
   ibmcloud ks cluster-addon-disable <add-on_name> --cluster <cluster_name_or_ID> -f
   ```
   {: pre}

7. Prima di continuare con il prossimo passo, verifica che le risorse del componente aggiuntivo degli spazi dei nomi dei componenti aggiuntivi o gli stessi spazi dei nomi dei componenti aggiuntivi siano stati rimossi.
   * Ad esempio, se aggiorni il componente aggiuntivo `istio-extras`, potresti verificare che i servizi `grafana`, `kiali` e `jaeger-*` siano stati rimossi dallo spazio dei nomi `istio-system`.
     ```
     kubectl get svc -n istio-system
     ```
     {: pre}
   * Ad esempio, se aggiorni il componente aggiuntivo `knative`, potresti verificare che gli spazi dei nomi `knative-serving`,
`knative-monitoring`, `knative-eventing`, `knative-build` e `istio-system` siano stati
eliminati. Lo **STATUS** degli spazi dei nomi potrebbe essere `Terminating` per alcuni minuti prima che vengano eliminati.
     ```
     kubectl get namespaces -w
     ```
     {: pre}

8. Scegli a quale versione desideri aggiornare i componenti aggiuntivi.
   ```
   ibmcloud ks addon-versions
   ```
   {: pre}

9. Riabilita il componente aggiuntivo. Utilizza l'indicatore `--version` per specificare la versione che desideri installare. Se non viene specificata alcuna versione, viene installata quella predefinita.
   ```
   ibmcloud ks cluster-addon-enable <add-on_name> --cluster <cluster_name_or_ID> --version <version>
   ```
   {: pre}

10. Applica le risorse delle definizioni di risorse personalizzate che hai salvato nel passo 2.
    ```
    kubectl apply -f <file_name> -n <namespace>
    ```
    {: pre}

11. Facoltativo per Knative: se hai salvato una qualsiasi delle risorse nel passo 3, riapplicale.
    Comando di esempio:
    ```
    kubectl apply -f config-autoscaler.yaml -n knative-serving
    ```
    {: pre}

12. Facoltativo per Istio: riabilita i componenti aggiuntivi `istio-extras` e `istio-sample-bookinfo`. Utilizza la stessa versione sia per questi componenti aggiuntivi, sia per il componente aggiuntivo `istio`.
    1. Abilita il componente aggiuntivo `istio-extras`.
       ```
       ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

    2. Abilita il componente aggiuntivo `istio-sample-bookinfo`.
       ```
       ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

13. Facoltativo per Istio: se utilizzi le sezioni TLS nei tuoi file di configurazione del gateway, devi eliminare e ricreare i gateway in modo che Envoy possa accedere ai segreti.
  ```
  kubectl delete gateway mygateway
  kubectl create -f mygateway.yaml
  ```
  {: pre}
