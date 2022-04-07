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
{:download: .download}
{:preview: .preview}


# Configurazione delle politiche di sicurezza del pod
{: #psp}

Con le [politiche di sicurezza del pod (PSP, pod security policy) ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/), puoi
configurare le politiche per autorizzare chi può creare e aggiornare i pod in {{site.data.keyword.containerlong}}.

**Come imposto le politiche di sicurezza dei pod?**</br>
Come amministratore cluster, vuoi controllare cosa succede nel tuo cluster, specialmente le azioni che influiscono sulla sicurezza o sulla disponibilità del cluster. Le politiche di sicurezza del pod ti consentono di controllare l'uso di contenitori privilegiati, di spazi dei nomi root, di reti host e di porte, di tipi di volume, di file system host, di autorizzazioni Linux come ID di gruppo o di sola lettura e altro.

Con il controller di ammissione `PodSecurityPolicy`, non potrai creare alcun pod fino a quando non [autorizzerai le politiche](#customize_psp). L'impostazione delle politiche di sicurezza del pod può avere effetti secondari involontari, assicurati quindi di eseguire il test di una distribuzione una volta modificata la politica. Per distribuire le applicazioni, tutti gli account utente e di servizio devono essere autorizzati dalle politiche di sicurezza del pod necessarie per distribuire i pod. Ad esempio, se installi le applicazioni utilizzando [Helm](/docs/containers?topic=containers-helm#public_helm_install), il componente tiller di Helm crea i pod e quindi devi disporre della corretta autorizzazione della politica di sicurezza del pod.

Stai provando a controllare quali utenti hanno accesso a {{site.data.keyword.containerlong_notm}}? Vedi [Assegnazione dell'accesso al cluster](/docs/containers?topic=containers-users#users) per impostare le autorizzazioni {{site.data.keyword.Bluemix_notm}} IAM e dell'infrastruttura.
{: tip}

**Ci sono politiche configurate per impostazione predefinita? Cosa posso aggiungere?**</br>
Per impostazione predefinita, {{site.data.keyword.containerlong_notm}} configura il controller di ammissione `PodSecurityPolicy` con le [risorse per la gestione cluster {{site.data.keyword.IBM_notm}}](#ibm_psp) che non puoi eliminare o modificare. Inoltre, non puoi disabilitare il controller di ammissione.

Per impostazione predefinita, le azioni pod non possono essere bloccate. Due risorse RBAC (role-based access control) nel cluster autorizzano invece tutti gli amministratori, gli utenti, i servizi e i nodi a creare pod privilegiati e non privilegiati. Sono incluse ulteriori risorse RBAC per la portabilità con i pacchetti di {{site.data.keyword.Bluemix_notm}} Privato utilizzati per le [distribuzioni ibride](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp).

Se vuoi impedire che determinati utenti creino o aggiornino i pod, puoi [modificare queste risorse RBAC oppure crearne di tue](#customize_psp).

**Come funziona l'autorizzazione della politica?**</br>
Quando come utente crei direttamente un pod senza utilizzare un controller, ad esempio una distribuzione, le tue credenziali vengono convalidate rispetto alle politiche di sicurezza del pod che hai autorizzato ad utilizzare. Se nessuna politica supporta i requisiti di sicurezza del pod, quest'ultimo non viene creato.

Quando crei un pod utilizzando un controller della risorsa, ad esempio una distribuzione, Kubernetes convalida le credenziali dell'account di servizio del pod rispetto alle politiche di sicurezza del pod che l'account di servizio è autorizzato ad utilizzare. Se nessuna politica supporta i requisiti di sicurezza del pod, il controller ha esito positivo, ma il pod non viene creato.

Per i messaggi di errore comuni, vedi [Distribuzione dei pod non riuscita a causa di una politica di sicurezza del pod](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_psp).

**Perché posso ancora creare dei pod privilegiati quando non sono parte del bind del ruolo cluster `privileged-psp-user`?**<br>
Altri bind del ruolo cluster o bind del ruolo con ambito delimitato agli spazi dei nomi possono darti altre politiche di sicurezza del pod che ti autorizzano a creare pod privilegiati. Inoltre, per impostazione predefinita, gli amministratori del cluster hanno accesso a tutte le risorse, comprese le politiche di sicurezza del pod e, pertanto, possono aggiungersi alle PSP o creare delle risorse privilegiate.

## Personalizzazione delle politiche di sicurezza del pod
{: #customize_psp}

Per impedire azioni pod non autorizzate, puoi modificare le risorse della politica di sicurezza del pod esistenti o crearne di tue. Per personalizzare le politiche devi essere un amministratore cluster.
{: shortdesc}

**Quali politiche esistenti posso modificare?**</br>
Per impostazione predefinita, il tuo cluster contiene le seguenti risorse RBAC che consentono agli amministratori
cluster, agli utenti autenticati, agli account di servizio e ai nodi di utilizzare le politiche di sicurezza
del pod `ibm-privileged-psp` e `ibm-restricted-psp`. Queste politiche consentono
agli utenti di creare e aggiornare i pod privilegiati e non privilegiati (limitato).

| Nome | Spazio dei nomi | Tipo | Scopo |
|---|---|---|---|
| `privileged-psp-user` | Tutti | `ClusterRoleBinding` | Consente agli amministratori cluster, agli utenti autenticati, agli account di servizio e ai nodi di utilizzare la politica di sicurezza del pod `ibm-privileged-psp`. |
| `restricted-psp-user` | Tutti | `ClusterRoleBinding` | Consente agli amministratori cluster, agli utenti autenticati, agli account di servizio e ai nodi di utilizzare la politica di sicurezza del pod `ibm-restricted-psp`. |
{: caption="Risorse RBAC predefinite che puoi modificare" caption-side="top"}

Puoi modificare questi ruoli RBAC per rimuovere o aggiungere gli amministratori, gli utenti, i servizi o i nodi alla politica.

Prima di iniziare:
*  [Accedi al tuo account. Se applicabile, specifica il gruppo di risorse appropriato. Imposta il contesto per il tuo cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  Comprendi l'utilizzo dei ruoli RBAC. Per ulteriori informazioni, vedi [Autorizzazione di utenti con ruoli personalizzati di RBAC Kubernetes](/docs/containers?topic=containers-users#rbac) o la [documentazione Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview).
* Assicurati di disporre del [ruolo di accesso del servizio {{site.data.keyword.Bluemix_notm}} IAM **Gestore**](/docs/containers?topic=containers-users#platform) per tutti gli spazi dei nomi.

Quando modifichi la configurazione predefinita, puoi impedire l'esecuzione di azioni cluster importanti, ad esempio le distribuzioni pod o gli aggiornamenti cluster. Verifica le tue modifiche in un cluster non di produzione che non viene utilizzato dagli altri team.
{: important}

**Per modificare le risorse RBAC**:
1.  Richiama il nome del bind del ruolo del cluster RBAC.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  Scarica il bind del ruolo del cluster come un file `.yaml` che puoi modificare in locale.

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml > privileged-psp-user.yaml
    ```
    {: pre}

    Potresti voler salvare una copia della politica esistente in modo che tu possa ripristinarla nel caso in cui la politica modificata produca risultati non previsti.
    {: tip}

    **Esempio di file di bind del ruolo del cluster**:

    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRoleBinding
    metadata:
      creationTimestamp: 2018-06-20T21:19:50Z
      name: privileged-psp-user
      resourceVersion: "1234567"
      selfLink: /apis/rbac.authorization.k8s.io/v1/clusterrolebindings/privileged-psp-user
      uid: aa1a1111-11aa-11a1-aaaa-1a1111aa11a1
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: ibm-privileged-psp-user
    subjects:
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:masters
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:nodes
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:serviceaccounts
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:authenticated
    ```
    {: codeblock}

3.  Modifica il file `.yaml` del bind del ruolo del cluster. Per comprendere cosa puoi modificare, esamina la [documentazione Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/). Azioni di esempio:

    *   **Account di servizio**: potresti voler autorizzare gli account di servizio in modo che le distribuzioni possano verificarsi solo in specifici spazi dei nomi. Ad esempio, se dedichi la politica per consentire azioni all'interno dello spazio dei nomi `kube-system`, possono verificarsi molte azioni importanti come ad esempio gli aggiornamenti cluster. Tuttavia, le azioni negli altri spazi dei nomi non sono più autorizzate.

        Per delimitare l'ambito della politica per consentire le azioni in uno specifico spazio dei nomi, modifica `system:serviceaccounts` in `system:serviceaccount:<namespace>`.
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:serviceaccount:kube-system
        ```
        {: codeblock}

    *   **Utenti**: potresti voler rimuovere l'autorizzazione per tutti gli utenti autenticati per distribuire i pod con accesso privilegiato. Rimuovi la voce `system:authenticated` riportata di seguito.
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:authenticated
        ```
        {: codeblock}

4.  Crea la risorsa di bind del ruolo del cluster modificata nel tuo cluster.

    ```
    kubectl apply -f privileged-psp-user.yaml
    ```
    {: pre}

5.  Verifica che la risorsa sia stata modificata.

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml
    ```
    {: pre}

</br>
**Per eliminare le risorse RBAC**:
1.  Richiama il nome del bind del ruolo del cluster RBAC.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  Elimina il ruolo RBAC che desideri rimuovere.
    ```
    kubectl delete clusterrolebinding privileged-psp-user
    ```
    {: pre}

3.  Verifica che il bind del ruolo del cluster RBAC non esista più nel tuo cluster.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

</br>
**Per creare una tua politica di sicurezza del pod**:</br>
per creare una tua risorsa di politica di sicurezza del pod e autorizzare gli utenti con RBAC, esamina la [documentazione di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/).

Assicurati di aver modificato le politiche esistenti in modo che la nuova politica che hai creato non vada in conflitto con quella esistente. Ad esempio, la politica esistente consente agli utenti di creare e aggiornare pod privilegiati. Se crei una politica che non consente agli utenti di creare o aggiornare pod privilegiati, il conflitto tra la nuova politica e quella esistente potrebbe causare risultati imprevisti.

## Descrizione delle risorse predefinite per la gestione cluster {{site.data.keyword.IBM_notm}}
{: #ibm_psp}

Il tuo cluster Kubernetes in {{site.data.keyword.containerlong_notm}} contiene le seguenti politiche di sicurezza del
pod e risorse RBAC correlate per consentire a {{site.data.keyword.IBM_notm}} di gestire correttamente il tuo cluster.
{: shortdesc}

Le risorse `PodSecurityPolicy` predefinite fanno riferimento alle politiche di sicurezza del pod impostate da {{site.data.keyword.IBM_notm}}.

**Attenzione**: non devi eliminare o modificare queste risorse.

| Nome | Spazio dei nomi | Tipo | Scopo |
|---|---|---|---|
| `ibm-anyuid-hostaccess-psp` | Tutti | `PodSecurityPolicy` | Politica per la creazione di pod con accesso completo all'host. |
| `ibm-anyuid-hostaccess-psp-user` | Tutti | `ClusterRole` | Ruolo cluster che consente l'uso della politica di sicurezza del pod `ibm-anyuid-hostaccess-psp`. |
| `ibm-anyuid-hostpath-psp` | Tutti | `PodSecurityPolicy` | Politica per la creazione di pod con accesso al percorso host. |
| `ibm-anyuid-hostpath-psp-user` | Tutti | `ClusterRole` | Ruolo cluster che consente l'uso della politica di sicurezza del pod `ibm-anyuid-hostpath-psp`. |
| `ibm-anyuid-psp` | Tutti | `PodSecurityPolicy` | Politica per la creazione di qualsiasi pod eseguibile UID/GID. |
| `ibm-anyuid-psp-user` | Tutti | `ClusterRole` | Ruolo cluster che consente l'uso della politica di sicurezza del pod `ibm-anyuid-psp`. |
| `ibm-privileged-psp` | Tutti | `PodSecurityPolicy` | Politica per la creazione del pod privilegiato. |
| `ibm-privileged-psp-user` | Tutti | `ClusterRole` | Ruolo cluster che consente l'uso della politica di sicurezza del pod `ibm-privileged-psp`. |
| `ibm-privileged-psp-user` | `kube-system` | `RoleBinding` | Consente agli amministratori cluster, agli account di servizio e ai nodi di utilizzare la politica di sicurezza del pod `ibm-privileged-psp` nello spazio dei nomi `kube-system`. |
| `ibm-privileged-psp-user` | `ibm-system` | `RoleBinding` | Consente agli amministratori cluster, agli account di servizio e ai nodi di utilizzare la politica di sicurezza del pod `ibm-privileged-psp` nello spazio dei nomi `ibm-system`. |
| `ibm-privileged-psp-user` | `kubx-cit` | `RoleBinding` | Consente agli amministratori cluster, agli account di servizio e ai nodi di utilizzare la politica di sicurezza del pod `ibm-privileged-psp` nello spazio dei nomi `kubx-cit`. |
| `ibm-restricted-psp` | Tutti | `PodSecurityPolicy` | Politica per la creazione di pod non privilegiati o limitati. |
| `ibm-restricted-psp-user` | Tutti | `ClusterRole` | Ruolo cluster che consente l'uso della politica di sicurezza del pod `ibm-restricted-psp`. |
{: caption="Risorse delle politiche di sicurezza del pod IBM che non devi modificare" caption-side="top"}
