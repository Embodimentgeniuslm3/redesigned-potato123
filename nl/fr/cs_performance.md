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


# Optimisation des performances
{: #kernel}

Si vous avez des exigences spécifiques en termes d'optimisation des performances, vous pouvez modifier les paramètres par défaut de certains composants de cluster dans {{site.data.keyword.containerlong}}.
{: shortdesc}

Si vous choisissez de modifier les paramètres par défaut, vous le faites à vos propres risques. Vous êtes responsable de l'exécution des tests sur les paramètres modifiés et de toute interruption potentielle qui serait provoquée par les paramètres modifiés dans votre environnement.
{: important}

## Optimisation des performances de noeud worker
{: #worker}

Si vous avez des exigences spécifiques en termes d'optimisation des performances, vous pouvez modifier les valeurs par défaut des paramètres `sysctl` du noyau Linux sur les noeuds worker.
{: shortdesc}

Les noeuds worker sont automatiquement mis à disposition avec des performances de noyau optimisées, mais vous pouvez modifier les paramètres par défaut en appliquant un objet [Kubernetes `DaemonSet` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) personnalisé dans votre cluster. Cet objet modifie les paramètres de tous les noeuds worker existants et appliquent les paramètres à tous les nouveaux noeuds worker mis à disposition dans le cluster. Aucun pod n'est affecté.

Vous devez disposer du [rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Responsable**](/docs/containers?topic=containers-users#platform) pour tous les espaces de nom pour exécuter l'exemple de commande `initContainer` avec privileged. Une fois que les conteneurs pour les déploiements sont initialisés, les privilèges sont supprimés.
{: note}

1. Sauvegardez l'ensemble de démons (DaemonSet) suivant dans un fichier nommé `worker-node-kernel-settings.yaml`. Dans la section `spec.template.spec.initContainers`, ajoutez les zones et les valeurs pour les paramètres `sysctl` que vous désirez optimiser. Cet exemple d'ensemble de démons modifie le nombre maximal par défaut de connexions autorisées dans l'environnement via le paramètre `net.core.somaxconn` et la plage de ports éphémères via le paramètre `net.ipv4.ip_local_port_range`.
    ```
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: kernel-optimization
      namespace: kube-system
      labels:
        tier: management
        app: kernel-optimization
    spec:
      selector:
        matchLabels:
          name: kernel-optimization
      template:
        metadata:
          labels:
            name: kernel-optimization
        spec:
          hostNetwork: true
          hostPID: true
          hostIPC: true
          initContainers:
            - command:
                - sh
                - -c
                - sysctl -w net.ipv4.ip_local_port_range="1025 65535"; sysctl -w net.core.somaxconn=32768;
              image: alpine:3.6
              imagePullPolicy: IfNotPresent
              name: sysctl
              resources: {}
              securityContext:
                privileged: true
                capabilities:
                  add:
                    - NET_ADMIN
              volumeMounts:
                - name: modifysys
                  mountPath: /sys
          containers:
            - resources:
                requests:
                  cpu: 0.01
              image: alpine:3.6
              name: sleepforever
              command: ["/bin/sh", "-c"]
              args:
                - >
                  while true; do
                    sleep 100000;
                  done
          volumes:
            - name: modifysys
              hostPath:
                path: /sys
    ```
    {: codeblock}

2. Appliquez l'ensemble de démons à vos noeuds worker. Les modifications sont appliquées immédiatement.
    ```
    kubectl apply -f worker-node-kernel-settings.yaml
    ```
    {: pre}

<br />

Pour rétablir les valeurs par défaut des paramètres `sysctl` de vos noeuds worker, définies par {{site.data.keyword.containerlong_notm}} :

1. Supprimez l'ensemble de démons. Les éléments `initContainers` qui appliquaient les paramètres personnalisés sont supprimés.
    ```
    kubectl delete ds kernel-optimization
    ```
    {: pre}

2. [Réamorcez tous les noeuds worker dans le cluster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot). Les noeuds worker sont à nouveau en ligne dès que les valeurs par défaut sont appliquées.

<br />


## Optimisation des performances de pod
{: #pod}

Si vous avez des exigences spécifiques en termes de charges de travail pour les performances, vous pouvez modifier les paramètres par défaut `sysctl` du noyau Linux dans les espaces de nom de réseau de pods.
{: shortdesc}

Pour optimiser les paramètres du noyau pour les pods d'application, vous pouvez insérer un correctif [`initContainer ` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) dans le fichier YAML `pod/ds/rs/deployment` pour chaque déploiement. L'élément `initContainer` est ajouté à chaque déploiement d'application figurant dans l'espace de nom du réseau de pods dont vous souhaitez optimiser les performances.

Avant de commencer, vérifiez que vous disposez du [rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Responsable**](/docs/containers?topic=containers-users#platform) pour tous les espaces de nom pour exécuter l'exemple de commande `initContainer` avec privileged. Une fois que les conteneurs pour les déploiements sont initialisés, les privilèges sont supprimés.

1. Sauvegardez le correctif `initContainer` suivant dans un fichier nommé `pod-patch.yaml` et ajoutez les zones et les valeurs pour les paramètres `sysctl` que vous désirez optimiser. Cet exemple d'élément `initContainer` modifie le nombre maximal par défaut de connexions autorisées dans l'environnement via le paramètre `net.core.somaxconn` et la plage de ports éphémères via le paramètre `net.ipv4.ip_local_port_range`.
    ```
    spec:
      template:
        spec:
          initContainers:
          - command:
            - sh
            - -c
            - sysctl -e -w net.core.somaxconn=32768;  sysctl -e -w net.ipv4.ip_local_port_range="1025 65535";
            image: alpine:3.6
            imagePullPolicy: IfNotPresent
            name: sysctl
            resources: {}
            securityContext:
              privileged: true
    ```
    {: codeblock}

2. Appliquez le correctif à chacun de vos déploiements.
    ```
    kubectl patch deployment <deployment_name> --patch pod-patch.yaml
    ```
    {: pre}

3. Si vous avez modifié la valeur `net.core.somaxconn` dans les paramètres du noyau, la plupart des applications peuvent utiliser automatiquement la valeur mise à jour. En revanche, certaines applications pourront nécessiter la modification manuelle de la valeur correspondante dans le code de votre application pour qu'elle corresponde à la valeur du noyau. Par exemple si vous optimisez les performances d'un pod dans lequel s'exécute une application NGINX, vous devez modifier la valeur de la zone `backlog` dans le code d'application NGINX pour qu'elle corresponde. Pour plus d'informations, reportez-vous à cet [article du blogue NGINX ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.nginx.com/blog/tuning-nginx/).

<br />


## Ajustement des ressources du fournisseur de métriques du cluster
{: #metrics}

Les configurations du fournisseur de métriques de votre cluster (`metrics-server` dans Kubernetes 1.12 et versions ultérieures, ou `heapster` dans les versions antérieures) sont optimisées pour les clusters comportant un nombre inférieur ou égal à 30 pods par noeud worker. Si votre cluster comporte plus de pods par noeud worker, le conteneur principal du fournisseur de métriques `metrics-server` ou `heapster` pour le pod peut redémarrer fréquemment avec un message d'erreur de ce type : `OOMKilled`.

Le pod du fournisseur de métriques comporte également un conteneur `nanny` pour la mise à l'échelle des demandes et des limites de ressources du conteneur principal de `metrics-server` ou `heapster` en réponse au nombre de noeuds worker dans le cluster. Vous pouvez modifier les ressources par défaut en éditant l'élément configmap du fournisseur de métriques.

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Ouvrez le fichier YAML correspondant à l'élément configmap du fournisseur de métriques du cluster.
    *  Pour `metrics-server` :
       ```
       kubectl get configmap metrics-server-config -n kube-system -o yaml
       ```
       {: pre}
    *  Pour `heapster` :
       ```
       kubectl get configmap heapster-config -n kube-system -o yaml
       ```
       {: pre}
    Exemple de sortie :
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
    kind: ConfigMap
    metadata:
      annotations:
        armada-service: cruiser-kube-addons
        version: --
      creationTimestamp: 2018-10-09T20:15:32Z
      labels:
        addonmanager.kubernetes.io/mode: EnsureExists
        kubernetes.io/cluster-service: "true"
      name: heapster-config
      namespace: kube-system
      resourceVersion: "526"
      selfLink: /api/v1/namespaces/kube-system/configmaps/heapster-config
      uid: 11a1aaaa-bb22-33c3-4444-5e55e555e555
    ```
    {: screen}

2.  Ajoutez la zone `memoryPerNode` à l'élément configmap dans la section `data.NannyConfiguration`. La valeur par défaut pour `metrics-server` et `heapster` est définie sur `4Mi`.
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
        memoryPerNode: 5Mi
    kind: ConfigMap
    ...
    ```
    {: codeblock}

3.  Appliquez vos modifications.
    ```
    kubectl apply -f heapster-config.yaml
    ```
    {: pre}

4.  Surveillez les pods du fournisseur de métriques pour voir si les conteneurs continuent à redémarrer en raison d'un message d'erreur `OOMKilled`. Dans ce cas, répétez ces étapes en augmentant la taille de `memoryPerNode` jusqu'à ce que le pod soit stable.

Vous voulez régler d'autres paramètres ? Consultez la [documentation sur la configuration du composant Addon Resizer de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/autoscaler/tree/master/addon-resizer#addon-resizer-configuration) pour en savoir plus.
{: tip}
