---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, node scaling

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
{:gif: data-image-type='gif'}



# Mise à l'échelle des clusters
{: #ca}

Avec le plug-in `ibm-iks-cluster-autoscaler` d'{{site.data.keyword.containerlong_notm}}, vous pouvez mettre à l'échelle automatiquement les pools worker dans votre cluster pour augmenter ou diminuer le nombre de noeuds worker dans le pool en fonction de la taille requise pour vos charges de travail planifiées. Le plug-in `ibm-iks-cluster-autoscaler` est basé sur le [projet Kubernetes Cluster-Autoscaler ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler).
{: shortdesc}

Vous préférez plutôt effectuer la mise à l'échelle automatique de vos pods ? Consultez la rubrique [Mise à l'échelle des applications](/docs/containers?topic=containers-app#app_scaling).
{: tip}

Le programme de mise à l'échelle automatique de cluster est disponible pour les clusters standard configurés avec une connectivité de réseau public. Si votre cluster n'a pas accès au réseau public, par exemple s'il s'agit d'un cluster privé derrière un pare-feu ou d'un cluster avec uniquement le noeud final de service privé activé, vous ne pouvez pas utiliser le programme de mise à l'échelle automatique de cluster.
{: important}

## Explication de la mise à l'échelle par augmentation ou par diminution
{: #ca_about}

Le programme de mise à l'échelle automatique de cluster analyse régulièrement le cluster pour ajuster le nombre de noeuds worker dans les pools worker qu'il gère en réponse à vos demandes de ressources de charge de travail et d'éventuels paramètres personnalisés que vous configurez, par exemple les intervalles d'analyse. Toutes les minutes, le programme vérifie les situations suivantes.
{: shortdesc}

*   **Pods en attente d'une augmentation de capacité** : un pod est considéré en attente lorsque il n'y a pas de ressources de calcul suffisantes pour qu'il soit planifié sur un noeud worker. Lorsque le programme de mise à l'échelle automatique de cluster détecte des pods en attente, il augmente le nombre de noeuds worker uniformément sur les zones pour satisfaire les demandes de ressources des charges de travail.
*   **Noeuds worker sous-utilisés à réduire** : par défaut, les noeuds worker qui s'exécutent avec moins de 50 % du total des ressources de calcul demandées sur une plage d'au moins 10 minutes et qui peuvent replanifier leurs charges de travail sur d'autres noeuds worker sont considérés comme sous-utilisés. Si le programme de mise à l'échelle automatique de cluster détecte des noeuds worker sous-utilisés, il réduit leur nombre un par un de sorte à ce que vous disposiez uniquement des ressources de calcul dont vous avez besoin. Si vous le souhaitez, vous pouvez [personnaliser](/docs/containers?topic=containers-ca#ca_chart_values) le seuil d'utilisation par défaut de 50% sur une durée de 10 minutes.

L'analyse et l'augmentation ou la diminution de capacité se produit à intervalles réguliers. Selon le nombre de noeuds worker, ces opérations peuvent prendre plus de temps, par exemple 30 minutes.

Le programme de mise à l'échelle automatique de cluster ajuste le nombre de noeuds worker en considérant les [demandes de ressources ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) que vous définissez pour vos déploiements, et non pas l'utilisation réelle des noeuds worker. Si vos pods et vos déploiements ne demandent pas les quantités de ressources appropriées, vous devez ajuster leurs fichiers de configuration. Le programme de mise à l'échelle automatique de cluster ne peut pas le faire à votre place. En outre, n'oubliez pas que les noeuds worker utilisent une partie de leurs ressources de calcul pour des fonctionnalités de cluster de base, des [modules complémentaires](/docs/containers?topic=containers-update#addons) par défaut et personnalisés, ainsi que pour des [réserves de ressources](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).
{: note}

<br>
**A quoi ressemble la mise à l'échelle (qu'il s'agisse d'une augmentation ou d'une réduction de capacité) ?**<br>
En général, le programme de mise à l'échelle automatique de cluster calcule le nombre de noeuds worker dont votre cluster a besoin pour exécuter sa charge de travail. La mise à l'échelle du cluster dépend de plusieurs facteurs, notamment :
*   La taille minimale et maximale des noeuds worker par zone que vous avez définies.
*   Vos demandes de ressources de pod en attente et certaines métadonnées que vous associez à la charge de travail, notamment l'anti-affinité, les libellés pour placer les pods uniquement sur certains types de machine, ou les objets [pod disruption budgets![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/).
*   Les pools worker gérés par le programme de mise à l'échelle automatique de cluster, éventuellement sur plusieurs zones d'un [cluster à zones multiples](/docs/containers?topic=containers-ha_clusters#multizone).
*   Les [valeurs de charte Helm personnalisées](#ca_chart_values) qui sont définies, par exemple, la non prise en compte des noeuds worker pour suppression s'ils utilisent le stockage local.

Pour plus d'informations, voir la foire aux questions (FAQ) de Kubernetes Cluster Autoscaler pour [How does scale-up work? ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-up-work) et [How does scale-down work? ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-down-work).

<br>

**Puis-je modifier le fonctionnement de la mise à l'échelle par augmentation et par réduction ?**<br>
Vous pouvez personnaliser des paramètres ou utiliser d'autres ressources Kubernetes pour affecter le fonctionnement de la mise à l'échelle par augmentation et par diminution.
*   **Scale-up** : [Personnalisez les valeurs de charte Helm de mise à l'échelle automatique pour le cluster](#ca_chart_values), par exemple, `scanInterval`, `expander`, `skipNodes` ou `maxNodeProvisionTime`. Passez en revue les moyens d'effectuer un [provisionnement excessif des noeuds worker](#ca_scaleup) afin de pouvoir effectuer une mise à l'échelle par augmentation des noeuds worker avant qu'un pool worker soit à court de ressources. Vous pouvez également [configurer des interruptions de budget de pod Kubernetes et des limites de priorité de pod](#scalable-practices-apps) afin d'affecter le fonctionnement de la mise à l'échelle par augmentation.
*   **Scale-down** : [Personnalisez les valeurs de charte Helm de mise à l'échelle automatique pour le cluster](#ca_chart_values), par exemple, `scaleDownUnneededTime`, `scaleDownDelayAfterAdd`, `scaleDownDelayAfterDelete` ou `scaleDownUtilizationThreshold`.

<br>
**Puis-je définir la taille minimale par zone pour faire passer immédiatement mon cluster à cette taille ?**<br>
Non, le fait de définir une taille minimale (`minSize`) ne déclenche pas automatiquement un changement de taille par augmentation. La taille minimale (`minSize`) est un seuil défini pour que le programme de mise à l'échelle automatique de cluster ne procède pas à une mise à l'échelle inférieure à un certain nombre de noeuds worker par zone. Si votre cluster ne dispose pas encore de ce nombre de noeuds par zone, le programme de mise à l'échelle automatique de cluster n'effectue pas de mise à l'échelle par augmentation tant qu'aucune de vos demandes de ressource de charge de travail ne réclame davantage de ressources. Par exemple, si vous disposez d'un pool de noeuds worker avec un noeud worker pour trois zones (trois noeuds worker au total) et vous affectez au paramètre `minSize` la valeur `4` pour chaque zone, le programme de mise à l'échelle automatique de cluster ne met pas immédiatement à disposition trois autres noeuds worker par zone (12 noeuds worker au total). En revanche, la mise à l'échelle par augmentation est déclenchée par les demandes de ressource. Si vous créez une charge de travail qui demande les ressources de 15 noeuds worker, le programme de mise à l'échelle automatique de cluster procède à une mise à l'échelle par augmentation du pool de noeuds worker pour répondre à la demande. Le paramètre `minSize` signifie que le programme de mise à l'échelle automatique de cluster ne procède pas à une mise à l'échelle par réduction au-dessous de quatre noeuds worker pour chaque zone même si vous retirez la charge de travail qui demande la quantité. 

<br>
**En quoi ce comportement est-il différent des pools worker qui ne sont pas gérés par le programme de mise à l'échelle automatique de cluster ?**<br>
Lorsque vous [créez un pool worker](/docs/containers?topic=containers-add_workers#add_pool), vous spécifiez le nombre de noeuds worker dont il dispose par zone. Le pool worker conserve ce nombre de noeuds tant que vous n'avez pas effectué de [redimensionnement](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) ou de [rééquilibrage](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) dessus. Le pool worker n'ajoute ou ne supprime pas de noeuds worker pour vous. Si vous disposez d'autres pods pouvant être planifiés, les pods restent à l'état en attente jusqu'à ce que vous redimensionniez le pool worker.

Lorsque vous activez le programme de mise à l'échelle automatique de cluster pour un pool worker, les noeuds worker sont augmentés ou diminués, en réponse aux paramètres de spécification et aux demandes de ressources de vos pods. Vous n'avez pas besoin de redimensionner ou de rééquilibrer le pool worker manuellement.

<br>
**Puis-je voir un exemple de mise à l'échelle automatique de cluster ?**<br>
Examinez l'image suivante pour obtenir un exemple de mise à l'échelle.

_Figure : Mise à l'échelle automatique d'un cluster._
![Mise à l'échelle automatique d'un cluster (GIF)](images/cluster-autoscaler-x3.gif){: gif}

1.  Le cluster comporte quatre noeuds worker dans deux pools worker répartis sur deux zones. Chaque pool contient un noeud worker par zone, mais le type de machine du **Pool worker A** est `u2c.2x4` et le type de machine du **Pool worker B** est `b2c.4x16`. Le nombre total de vos ressources de calcul est d'environ 10 coeurs (2 coeurs x 2 noeuds worker pour le **Pool worker A** et 4 coeurs x 2 noeuds worker pour le **Pool worker B**). Votre cluster exécute actuellement une charge de travail qui nécessite 6 de ces 10 coeurs. D'autres ressources de calcul sont prélevées sur chaque noeud worker par des [ressources réservées](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node) qui sont nécessaires pour exécuter le cluster, les noeuds worker et d'autres modules complémentaires, tels que le programme de mise à l'échelle automatique de cluster.
2.  Le programme de mise à l'échelle automatique de cluster est configuré pour gérer les deux pools worker avec la taille minimale et la taille maximale par zone suivantes :
    *  **Pool worker A** : `minSize=1`, `maxSize=5`.
    *  **Pool worker B** : `minSize=1`, `maxSize=2`.
3.  Vous planifiez des déploiements qui nécessitent 14 répliques de pod supplémentaires d'une application qui sollicite un coeur d'UC par réplique. Une réplique de pod peut être déployée sur les ressources actuelles, mais les 13 autres sont en attente.
4.  Le programme de mise à l'échelle automatique du cluster augmente le nombre de vos noeuds worker avec ces contraintes pour la prise en charge des demandes de ressources pour ces 13 autres répliques de pod.
    *  **Pool de noeuds worker A** : Sept noeuds worker sont ajoutés à tour de rôle aussi équitablement possible sur les différentes zones. Les noeuds worker augmentent la capacité de calcul du cluster d'environ 14 coeurs (2 coeurs x 7 noeuds worker).
    *  **Pool de noeuds worker B** : Deux noeuds worker sont ajoutés de manière équitable entre les zones, atteignant la taille maximale (`maxSize`) de 2 noeuds worker par zone. Les noeuds worker augmentent la capacité du cluster d'environ 8 coeurs (4 coeurs x 2 noeuds worker).
5.  Les 20 pods avec des demandes correspondant à 1 coeur sont répartis comme suit dans les noeuds worker. Comme les noeuds worker ont des réserves de ressources, ainsi que des pods qui s'exécutent pour les fonctions de cluster par défaut, les pods de votre charge de travail ne peuvent pas utiliser toutes les ressources de calcul disponibles d'un noeud worker. Par exemple, même si les noeuds worker de type `b2c.4x16` ont quatre coeurs, seuls trois pods qui demandent un coeur minimum chacun peuvent être planifiés sur les noeuds worker.
    <table summary="Tableau présentant la répartition d'une charge de travail dans un cluster mis à l'échelle.">
    <caption>Répartition de la charge de travail dans un cluster mis à l'échelle.</caption>
    <thead>
    <tr>
      <th>Pool worker</th>
      <th>Zone</th>
      <th>Type</th>
      <th>Nb de noeuds worker</th>
      <th>Nb de pods</th>
    </tr>
    </thead>
    <tbody>
    <tr>
      <td>A</td>
      <td>dal10</td>
      <td>u2c.2x4</td>
      <td>Quatre noeuds</td>
      <td>Trois pods</td>
    </tr>
    <tr>
      <td>A</td>
      <td>dal12</td>
      <td>u2c.2x4</td>
      <td>Cinq noeuds</td>
      <td>Cinq pods</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal10</td>
      <td>b2c.4x16</td>
      <td>Deux noeuds</td>
      <td>Six pods</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal12</td>
      <td>b2c.4x16</td>
      <td>Deux noeuds</td>
      <td>Six pods</td>
    </tr>
    </tbody>
    </table>
6.  Comme vous n'avez plus besoin de charge de travail supplémentaire, vous supprimez le déploiement. Après un court laps de temps, le programme de mise à l'échelle automatique de cluster détecte que votre cluster n'a plus besoin de toutes ses ressources de calcul et réduit le nombre de noeuds worker un par un.
7.  Vos pools worker sont réduits. Le programme de mise à l'échelle automatique de cluster effectue des analyses à intervalles réguliers pour rechercher des demandes de ressources de pods en attente et des noeuds worker sous-utilisés pour augmenter ou diminuer vos pools worker.

## Pratiques de déploiement évolutives
{: #scalable-practices}

Tirez le meilleur parti du programme de mise à l'échelle automatique de cluster en utilisant les stratégies suivantes pour votre noeud worker et les stratégies de déploiement de charge de travail. Pour plus d'informations voir la [foire aux questions (FAQ) de Kubernetes Cluster Autoscaler ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md).
{: shortdesc}

[Essayez le programme de mise à l'échelle automatique de cluster](#ca_helm) avec un petit nombre de charges de travail de test afin de vous faire une bonne idée de la fonction dont [la mise à l'échelle par augmentation et la mise à l'échelle par réduction fonctionnent](#ca_about), des [valeurs personnalisées](#ca_chart_values) que vous pourriez souhaiter configurer, ainsi que d'autres aspects qui pourraient vous intéresser, par exemple le [provisionnement excessif](#ca_scaleup) de noeuds worker ou la [limitation d'applications](#ca_limit_pool). Ensuite, nettoyez votre environnement de test et prévoyez d'inclure ces valeurs personnalisées et d'autres paramètres avec une toute nouvelle installation du programme de mise à l'échelle automatique de cluster.

### Puis-je effectuer une mise à l'échelle automatique de plusieurs pools worker en même temps ?
{: #scalable-practices-multiple}
Oui, après avoir installé la charte Helm, vous pouvez choisir les pools worker du cluster sur lesquels doit porter la mise à l'échelle automatique [dans configmap](#ca_cm). Vous ne pouvez exécuter qu'une charte Helm `ibm-iks-cluster-autoscaler` par cluster.
{: shortdesc}

### Comment puis-je être certain que le programme de mise à l'échelle automatique de cluster répond aux besoins en ressources de mon application ?
{: #scalable-practices-resrequests}

Le programme de mise à l'échelle automatique de cluster ajuste votre cluster en réponse aux [demandes de ressources ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) de votre charge de travail. Ainsi, spécifiez les [demandes de ressources ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) de tous vos déploiements car ces demandes seront utilisées par le programme de mise à l'échelle automatique de cluster pour calculer le nombre de noeuds worker nécessaires pour exécuter la charge de travail. N'oubliez pas que la mise à l'échelle automatique est basée sur l'utilisation de calcul demandé par vos configurations de charge de travail, et ne tient pas compte d'autres facteurs comme le coût des machines.
{: shortdesc}

### Puis-je réduire un pool worker à zéro (0) noeud ?
{: #scalable-practices-zero}

Non, vous ne pouvez pas affecter au paramètre `minSize` du programme de mise à l'échelle automatique de cluster la valeur `0`. De plus, à moins de [désactiver](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure) tous les équilibreurs de charge d'application (ALB) publics dans chaque zone de votre cluster, vous devez remplacer par `2` la valeur du paramètre `minSize` pour chaque zone de sorte que les pods d'ALB soient dispersés pour assurer une haute disponibilité.
{: shortdesc}

### Puis-je optimiser mes déploiements pour la mise à l'échelle automatique ?
{: #scalable-practices-apps}

Oui, vous pouvez ajouter plusieurs fonctions Kubernetes à votre déploiement afin d'ajuster la façon dont le programme de mise à l'échelle automatique de cluster prend en compte vos demandes de mise à l'échelle pour vos ressources.
{: shortdesc}
*   Utilisez des objets [pod disruption budgets ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) pour éviter des replanifications ou des suppressions brutales de vos pods.
*   Si vous utilisez des priorités de pod, vous pouvez [modifier la limite de priorité ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-cluster-autoscaler-work-with-pod-priority-and-preemption) pour modifier les types de priorité qui déclenchent une mise à l'échelle au niveau supérieur. Par défaut, la limite de priorité est zéro (`0`).

### Puis-je utiliser des annotations taint et des tolérances avec des pools worker mis à l'échelle automatiquement ?
{: #scalable-practices-taints}

Comme les annotations taint ne sont pas applicables au niveau du pool worker, n'utilisez pas de [noeuds worker avec taint](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) pour éviter d'obtenir des résultats imprévisibles. Par exemple, lorsque vous déployez une charge de travail qui n'est pas tolérée par les noeuds worker avec taint, l'augmentation des noeuds worker n'est pas envisagée et d'autres noeuds worker peuvent être commandés même si le cluster dispose d'une capacité suffisante. Cependant, les noeuds worker avec taint restent identifiés comme sous-utilisés si l'utilisation de leurs ressources est inférieure au seuil défini (par défaut 50 %) et sont donc pris en compte dans la diminution.
{: shortdesc}

### Pourquoi mes pools worker qui ont fait l'objet d'une mise à l'échelle automatique ne sont pas équilibrés ?
{: #scalable-practices-unbalanced}

Lors d'une mise à l'échelle par augmentation, le programme de mise à l'échelle automatique équilibre les noeuds dans les zones, avec une différence autorisée de plus ou moins un (+/- 1) noeud worker. Vos charges de travail en attente risquent de ne pas demander suffisamment de capacité pour que chaque zone soit équilibrée. Dans ce cas, si vous voulez équilibrer manuellement les pools worker, [mettez à jour le fichier configmap du programme de mise à l'échelle automatique de cluster](#ca_cm) pour retirer les pools worker déséquilibrés. Exécutez ensuite la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) `ibmcloud ks worker-pool-rebalance` et remettez le pool de noeuds worker dans le fichier configmap du programme de mise à l'échelle automatique de cluster.
{: shortdesc}


### Pourquoi ne puis-je pas redimensionner ou rééquilibrer mon pool worker ?
{: #scalable-practices-resize}

Lorsque le programme de mise à l'échelle automatique de cluster est activé pour un pool worker, vous ne pouvez pas [redimensionner](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) ou [rééquilibrer](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) vos pools worker. Vous devez [modifier le fichier configmap](#ca_cm) pour changer les tailles minimale et maximale du pool worker, ou désactiver la mise à l'échelle automatique de cluster pour ce pool worker. N'utilisez pas la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_rm) `ibmcloud ks worker-rm` pour supprimer des noeuds worker individuels de votre pool de noeuds worker, car celui-ci pourrait être déséquilibré.
{: shortdesc}

Par ailleurs, si vous ne désactivez pas les pools worker avant de désinstaller la charte Helm `ibm-iks-cluster-autoscaler`, les pools worker ne peuvent pas être redimensionnés manuellement. Réinstallez la charte Helm `ibm-iks-cluster-autoscaler`, [modifiez le fichier configmap](#ca_cm) pour désactiver le pool worker, et réessayez.

<br />


## Déploiement de la charte Helm du programme de mise à l'échelle automatique de cluster dans votre cluster
{: #ca_helm}

Installez le plug-in du programme de mise à l'échelle automatique de cluster d'{{site.data.keyword.containerlong_notm}} avec une charte Helm pour mettre à l'échelle automatiquement vos pools worker dans votre cluster.
{: shortdesc}

**Avant de commencer** :

1.  [Installez l'interface de ligne de commande et les plug-in requis](/docs/cli?topic=cloud-cli-getting-started) :
    *  Interface de ligne de commande {{site.data.keyword.Bluemix_notm}} (`ibmcloud`)
    *  Plug-in {{site.data.keyword.containerlong_notm}} (`ibmcloud ks`)
    *  Plug-in {{site.data.keyword.registrylong_notm}} (`ibmcloud cr`)
    *  Kubernetes (`kubectl`)
    *  Helm (`helm`)
2.  [Créez un cluster standard](/docs/containers?topic=containers-clusters#clusters_ui) qui exécute **Kubernetes version 1.12 ou ultérieure**.
3.   [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
4.  Confirmez que vos données d'identification pour {{site.data.keyword.Bluemix_notm}} Identity and Access Management sont stockées dans le cluster. Le programme de mise à l'échelle automatique de cluster utilise cette valeur confidentielle pour l'authentification des données d'identification.
    Si la valeur confidentielle est manquante, [créez-la en redéfinissant des données d'identification](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions).
    ```
    kubectl get secrets -n kube-system | grep storage-secret-store
    ```
    {: pre}
5.  Le programme de mise à l'échelle automatique de cluster peut fonctionner uniquement pour les pools worker ayant le libellé `ibm-cloud.kubernetes.io/worker-pool-id`.
    1.  Vérifiez si votre pool de noeuds worker comporte le libellé requis.
        ```
        ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels
        ```
        {: pre}

        Exemple de sortie d'un pool worker disposant de ce libellé :
        ```
        Labels:             ibm-cloud.kubernetes.io/worker-pool-id=a1aa111111b22b22cc3c3cc444444d44-4d555e5
        ```
        {: screen}
    2.  Si votre pool worker ne dispose pas du libellé requis, [ajoutez un nouveau pool worker](/docs/containers?topic=containers-add_workers#add_pool) et utilisez ce nouveau pool avec le programme de mise à l'échelle automatique de cluster.


<br>
**Pour installer le plug-in `ibm-iks-cluster-autoscaler` dans votre cluster** :

1.  [Suivez les instructions](/docs/containers?topic=containers-helm#public_helm_install) pour installer le client **Helm version 2.11 ou ultérieure** sur votre machine locale et installer le serveur Helm (Tiller) avec un compte de service dans votre cluster.
2.  Vérifiez que Tiller est installé avec un compte de service.

    ```
    kubectl get serviceaccount -n kube-system tiller
    ```
    {: pre}

    Exemple de sortie :

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}
3.  Ajoutez et mettez à jour le référentiel Helm où se trouve la charte Helm du programme de mise à l'échelle automatique de cluster.
    ```
    helm repo add iks-charts https://icr.io/helm/iks-charts
    ```
    {: pre}
    ```
    helm repo update
    ```
    {: pre}
4.  Installez la charte Helm du programme de mise à l'échelle automatique de cluster dans l'espace de nom `kube-system` de votre cluster.

    Durant l'installation, vous pouvez [personnaliser davantage les paramètres du programme de mise à l'échelle automatique de cluster](#ca_chart_values), par exemple la durée d'attente avant d'augmenter ou de réduire le nombre de noeuds worker.
    {: tip}

    ```
    helm install iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Exemple de sortie :
    ```
    NAME:   ibm-iks-cluster-autoscaler
    LAST DEPLOYED: Thu Nov 29 13:43:46 2018
    NAMESPACE: kube-system
    STATUS: DEPLOYED

    RESOURCES:
    ==> v1/ClusterRole
    NAME                       AGE
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Pod(related)

    NAME                                        READY  STATUS             RESTARTS  AGE
    ibm-iks-cluster-autoscaler-67c8f87b96-qbb6c  0/1    ContainerCreating  0         1s

    ==> v1/ConfigMap

    NAME              AGE
    iks-ca-configmap  1s

    ==> v1/ClusterRoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Role
    ibm-iks-cluster-autoscaler  1s

    ==> v1/RoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Service
    ibm-iks-cluster-autoscaler  1s

    ==> v1beta1/Deployment
    ibm-iks-cluster-autoscaler  1s

    ==> v1/ServiceAccount
    ibm-iks-cluster-autoscaler  1s

    NOTES:
    Thank you for installing: ibm-iks-cluster-autoscaler. Your release is named: ibm-iks-cluster-autoscaler

    For more information about using the cluster autoscaler, refer to the chart README.md file.
    ```
    {: screen}

5.  Vérifiez que l'installation a abouti.

    1.  Vérifiez que le pod du programme de mise à l'échelle automatique de cluster est à l'état **Running**.
        ```
        kubectl get pods --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        Exemple de sortie :
        ```
        ibm-iks-cluster-autoscaler-8497bfc968-dbn7w   1/1       Running   0          9m
        ```
        {: screen}
    2.  Vérifiez que le service du programme de mise à l'échelle automatique de cluster est créé.
        ```
        kubectl get service --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        Exemple de sortie :
        ```
        ibm-iks-cluster-autoscaler   ClusterIP   172.21.xxx.xx    <none>        8085/TCP        9m
        ```
        {: screen}

6.  Répétez ces étapes pour tous les clusters dans lesquels vous souhaitez mettre à disposition le programme de mise à l'échelle automatique de cluster.

7.  Pour commencer la mise à l'échelle de vos pools worker, voir [Mise à jour de la configuration du programme de mise à l'échelle automatique de cluster](#ca_cm).

<br />


## Mise à jour de la mappe de configuration du programme de mise à l'échelle automatique de cluster pour activer la mise à l'échelle
{: #ca_cm}

Mettez à jour la mappe de configuration (configmap) du programme de mise à l'échelle automatique de cluster pour activer la mise à l'échelle automatique des noeuds worker dans vos pools worker en fonction des valeurs minimales et maximales que vous avez définies.
{: shortdesc}

Après avoir édité la mappe de configuration pour activer un pool de noeuds worker, le programme de mise à l'échelle automatique de cluster commence à ajuster votre cluster en réponse aux demandes de votre charge de travail. Par conséquent, vous ne pouvez pas [redimensionner](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) ou [rééquilibrer](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) vos pools worker. L'analyse et l'augmentation ou la diminution de capacité se produit à intervalles réguliers. Selon le nombre de noeuds worker, ces opérations peuvent prendre plus de temps, par exemple 30 minutes. Par la suite, si vous souhaitez [supprimer le programme de mise à l'échelle automatique de cluster](#ca_rm), vous devez d'abord désactiver chaque pool worker dans la mappe de configuration.
{: note}

**Avant de commencer** :
*  [Installez le plug-in `ibm-iks-cluster-autoscaler`](#ca_helm).
*  [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**Pour mettre à jour la mappe de configuration (configmap) et les valeurs du programme de mise à l'échelle automatique de cluster** :

1.  Editez le fichier YAML configmap du programme de mise à l'échelle automatique de cluster.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system -o yaml
    ```
    {: pre}
    Exemple de sortie :
    ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false}
        ]
    kind: ConfigMap
    metadata:
      creationTimestamp: 2018-11-29T18:43:46Z
      name: iks-ca-configmap
      namespace: kube-system
      resourceVersion: "2227854"
      selfLink: /api/v1/namespaces/kube-system/configmaps/iks-ca-configmap
      uid: b45d047b-f406-11e8-b7f0-82ddffc6e65e
    ```
    {: screen}
2.  Editez le fichier configmap avec les paramètres permettant de définir comment le programme de mise à l'échelle automatique de cluster procède pour mettre à l'échelle le pool de noeuds worker de votre cluster. **Remarque :** A moins d'avoir [désactivé](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure) tous les équilibreurs de charge d'application (ALB) publics dans chaque zone de votre cluster standard, vous devez remplacer par `2` la valeur de `minSize` pour chaque zone de sorte que les pods d'ALB soient dispersés afin d'assurer une haute disponibilité. 

    <table>
    <caption>Paramètres de la mappe de configuration du programme de mise à l'échelle automatique de cluster</caption>
    <thead>
    <th id="parameter-with-default">Paramètre avec la valeur par défaut</th>
    <th id="parameter-with-description">Description</th>
    </thead>
    <tbody>
    <tr>
    <td id="parameter-name" headers="parameter-with-default">`"name": "default"`</td>
    <td headers="parameter-name parameter-with-description">Remplacez `"default"` par le nom ou l'ID du pool worker que vous souhaitez mettre à l'échelle. Pour afficher la liste des pools worker, exécutez la commande `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`.<br><br>
    Pour gérer plusieurs pools worker, copiez la ligne JSON sur une ligne séparée par des virgules, comme suit. <pre class="codeblock">[
     {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
     {"name": "Pool2","minSize": 2,"maxSize": 5,"enabled":true}
    ]</pre><br><br>
    **Remarque** : Le programme de mise à l'échelle automatique de cluster ne peut fonctionner qu'avec des pools worker avec le libellé `ibm-cloud.kubernetes.io/worker-pool-id`. Pour vérifiez si votre pool de noeuds worker comporte le libellé requis, exécutez la commande `ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels`. Si votre pool worker ne dispose pas du libellé requis, [ajoutez un nouveau pool worker](/docs/containers?topic=containers-add_workers#add_pool) et utilisez ce nouveau pool avec le programme de mise à l'échelle automatique de cluster.</td>
    </tr>
    <tr>
    <td id="parameter-minsize" headers="parameter-with-default">`"minSize": 1`</td>
    <td headers="parameter-minsize parameter-with-description">Indiquez le nombre minimal de noeuds worker par zone auquel le programme de mise à l'échelle automatique de cluster peut réduire le pool de noeuds worker. Cette valeur doit être supérieure ou égale à `2` pour que vos pods d'ALB puissent être dispersés pour assurer la haute disponibilité. Si vous avez [désactivé](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure) tous les ALB publics dans chaque zone de votre cluster standard, vous pouvez a affecter la valeur `1`.
    <p class="note">Le fait de définir une taille minimale (`minSize`) ne déclenche pas automatiquement un changement de taille par augmentation. La taille minimale (`minSize`) est un seuil défini pour que le programme de mise à l'échelle automatique de cluster ne procède pas à une mise à l'échelle inférieure à un certain nombre de noeuds worker par zone. Si votre cluster ne dispose pas encore de ce nombre de noeuds par zone, le programme de mise à l'échelle automatique de cluster n'effectue pas de mise à l'échelle par augmentation tant qu'aucune de vos demandes de ressource de charge de travail ne réclame davantage de ressources. Par exemple, si vous disposez d'un pool de noeuds worker avec un noeud worker pour trois zones (trois noeuds worker au total) et vous affectez au paramètre `minSize` la valeur `4` pour chaque zone, le programme de mise à l'échelle automatique de cluster ne met pas immédiatement à disposition trois autres noeuds worker par zone (12 noeuds worker au total). En revanche, la mise à l'échelle par augmentation est déclenchée par les demandes de ressource. Si vous créez une charge de travail qui demande les ressources de 15 noeuds worker, le programme de mise à l'échelle automatique de cluster procède à une mise à l'échelle par augmentation du pool de noeuds worker pour répondre à la demande. Le paramètre `minSize` signifie que le programme de mise à l'échelle automatique de cluster ne procède pas à une mise à l'échelle par réduction au-dessous de quatre noeuds worker pour chaque zone même si vous retirez la charge de travail qui demande la quantité. Pour plus d'informations, voir la [documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#when-does-cluster-autoscaler-change-the-size-of-a-cluster).</p></td>
    </tr>
    <tr>
    <td id="parameter-maxsize" headers="parameter-with-default">`"maxSize": 2`</td>
    <td headers="parameter-maxsize parameter-with-description">Indiquez le nombre maximal de noeuds worker par zone auquel le programme de mise à l'échelle automatique de cluster peut faire passer le pool de noeuds worker. Cette valeur doit être supérieure ou égale à la valeur que vous avez définie pour le paramètre `minSize`.</td>
    </tr>
    <tr>
    <td id="parameter-enabled" headers="parameter-with-default">`"enabled": false`</td>
    <td headers="parameter-enabled parameter-with-description">Définissez cette valeur sur `true` pour que le programme de mise à l'échelle automatique du cluster gère la mise à l'échelle du pool worker. Définissez cette valeur sur `false` pour arrêter la mise à l'échelle du pool worker par le programme de mise à l'échelle automatique de cluster.<br><br>
    Par la suite, si vous souhaitez [supprimer le programme de mise à l'échelle automatique de cluster](#ca_rm), vous devez d'abord désactiver chaque pool worker dans la mappe de configuration.</td>
    </tr>
    </tbody>
    </table>
3.  Sauvegardez le fichier de configuration.
4.  Obtenez le pod du programme de mise à l'échelle automatique de cluster.
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
5.  Examinez la section **`Events`** du pod du programme de mise à l'échelle automatique de cluster pour un événement **`ConfigUpdated`** afin de vérifier que la mappe de configuration a été mise à jour. Le message d'événement de votre mappe de configuration est au format suivant :`minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`.

    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    Exemple de sortie :
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
		Type     Reason         Age   From                                        Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

## Personnalisation des valeurs de configuration de la charte Helm du programme de mise à l'échelle automatique de cluster
{: #ca_chart_values}

Personnalisez les paramètres du programme de mise à l'échelle automatique de cluster, par exemple la durée d'attente avant d'augmenter ou de réduire des noeuds worker.
{: shortdesc}

**Avant de commencer** :
*  [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  [Installez le plug-in `ibm-iks-cluster-autoscaler`](#ca_helm).

**Pour mettre à jour les valeurs du programme de mise à l'échelle automatique de cluster** :

1.  Examinez les valeurs de configuration de la charte Helm du programme de mise à l'échelle automatique de cluster. Ce programme est fourni avec des paramètres par défaut. Cependant, vous souhaiterez peut-être modifier certaines valeurs telles que les intervalles d'analyse ou de réduction, en fonction de la fréquence des modifications que vous apportez à vos charges de travail.
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}

    Exemple de sortie :
    ```
    expander: least-waste
    image:
      pullPolicy: Always
      repository: icr.io/iks-charts/ibm-iks-cluster-autoscaler
      tag: dev1
    maxNodeProvisionTime: 120m
    resources:
      limits:
        cpu: 300m
        memory: 300Mi
      requests:
        cpu: 100m
        memory: 100Mi
    scaleDownDelayAfterAdd: 10m
    scaleDownDelayAfterDelete: 10m
    scaleDownUtilizationThreshold: 0.5
    scaleDownUnneededTime: 10m
    scanInterval: 1m
    skipNodes:
      withLocalStorage: true
      withSystemPods: true
    ```
    {: codeblock}

    <table>
    <caption>Valeurs de configuration du programme de mise à l'échelle automatique de cluster</caption>
    <thead>
    <th>Paramètre</th>
    <th>Description</th>
    <th>Valeur par défaut</th>
    </thead>
    <tbody>
    <tr>
    <td>Paramètre `api_route`</td>
    <td>Définissez le [noeud final d'API {{site.data.keyword.containerlong_notm}} ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cli_api) pour la région dans laquelle se trouve votre cluster. </td>
    <td>Aucune valeur par défaut. Utilise la région ciblée dans laquelle se trouve votre cluster.</td>
    </tr>
    <tr>
    <td>Paramètre `expander`</td>
    <td>Indiquez comment le programme de mise à l'échelle automatique de cluster détermine le pool worker à mettre à l'échelle si vous disposez de plusieurs pools de ce type. Les valeurs possibles sont :
    <ul><li>`random` : sélection de manière aléatoire entre `most-pods` et `least-waste`.</li>
    <li>`most-pods` : sélection du pool worker en mesure de planifier le plus de pods lors d'une augmentation d'échelle. Utilisez cette méthode avec `nodeSelector` pour vous assurer que les pods atterrissent sur des noeuds worker spécifiques.</li>
    <li>`least-waste` : sélection du pool de noeuds worker ayant le moins d'UC inutilisée après une mise à l'échelle par augmentation. Si deux pools de noeuds worker utilisent la même quantité de ressources d'UC après la mise à l'échelle par augmentation, le pool de noeuds worker avec le moins de mémoire inutilisée est sélectionné. </li></ul></td>
    <td>random</td>
    </tr>
    <tr>
    <td>Paramètre `image.repository`</td>
    <td>Indiquez l'image Docker à utiliser pour le programme de mise à l'échelle automatique de cluster.</td>
    <td>`icr.io/iks-charts/ibm-iks-cluster-autoscaler`</td>
    </tr>
    <tr>
    <td>Paramètre `image.pullPolicy`</td>
    <td>Indiquez à quel moment extraire l'image Docker. Les valeurs possibles sont :
    <ul><li>`Always` : extrait l'image chaque fois que le pod est démarré.</li>
    <li>`IfNotPresent` : extrait l'image uniquement si elle n'est pas déjà présente en local.</li>
    <li>`Never` : suppose que l'image existe en local et par conséquent ne l'extrait jamais.</li></ul></td>
    <td>Always</td>
    </tr>
    <tr>
    <td>Paramètre `maxNodeProvisionTime`</td>
    <td>Définissez la durée maximale en minutes que peut prendre un noeud worker pour commencer le provisionnement avant l'annulation de la demande d'augmentation par le programme de mise à l'échelle automatique du cluster.</td>
    <td>`120m`</td>
    </tr>
    <tr>
    <td>Paramètre `resources.limits.cpu`</td>
    <td>Définissez la quantité maximale d'UC de noeud worker que peut consommer le pod `ibm-iks-cluster-autoscaler`.</td>
    <td>`300m`</td>
    </tr>
    <tr>
    <td>Paramètre `resources.limits.memory`</td>
    <td>Définissez la quantité maximale de mémoire de noeud worker que peut consommer le pod `ibm-iks-cluster-autoscaler`.</td>
    <td>`300Mi`</td>
    </tr>
    <tr>
    <td>Paramètre `resources.requests.cpu`</td>
    <td>Définissez la quantité minimale d'UC de noeud worker dont dispose le pod `ibm-iks-cluster-autoscaler` au démarrage.</td>
    <td>`100m`</td>
    </tr>
    <tr>
    <td>Paramètre `resources.requests.memory`</td>
    <td>Définissez la quantité minimale de mémoire de noeud worker dont dispose le pod `ibm-iks-cluster-autoscaler` au démarrage.</td>
    <td>`100Mi`</td>
    </tr>
    <tr>
    <td>Paramètre `scaleDownUnneededTime`</td>
    <td>Définissez la durée en minutes pendant laquelle le noeud worker doit être inutile avant de faire l'objet d'une réduction.</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>Paramètres `scaleDownDelayAfterAdd` et `scaleDownDelayAfterDelete`</td>
    <td>Définissez la durée d'attente en minutes du programme de mise à l'échelle automatique de cluster avant de relancer des actions de mise à l'échelle après une augmentation (`add`) ou une réduction (`delete`).</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>Paramètre `scaleDownUtilizationThreshold`</td>
    <td>Définissez le seuil d'utilisation du noeud worker. Si cette valeur passe au-dessous du seuil, la réduction du noeud worker est envisagée. L'utilisation du noeud worker est calculée à partir de la somme des ressources d'UC et de mémoire demandées par tous les pods qui s'exécutent sur le noeud worker divisée par la capacité en ressources du noeud worker.</td>
    <td>`0.5`</td>
    </tr>
    <tr>
    <td>Paramètre `scanInterval`</td>
    <td>Définissez la fréquence en minutes que prend le programme de mise à l'échelle automatique de cluster pour analyser l'utilisation de la charge de travail qui déclenche une augmentation ou une réduction d'échelle.</td>
    <td>`1m`</td>
    </tr>
    <tr>
    <td>Paramètre `skipNodes.withLocalStorage`</td>
    <td>Lorsque la valeur de ce paramètre est définie sur `true`, les noeuds worker qui comportent des pods qui sauvegardent des données sur du stockage local ne font pas l'objet d'une réduction d'échelle.</td>
    <td>`true`</td>
    </tr>
    <tr>
    <td>Paramètre `skipNodes.withSystemPods`</td>
    <td>Lorsque la valeur de ce paramètre est définie sur `true`, les noeuds worker qui comportent des pods `kube-system` ne sont pas réduits. Ne définissez pas la valeur sur `false` car la réduction des pods `kube-system` peut avoir des résultats imprévisibles.</td>
    <td>`true`</td>
    </tr>
    </tbody>
    </table>
2.  Pour modifier une valeur de configuration du programme de mise à l'échelle automatique de cluster, mettez à jour la charte Helm avec la nouvelle valeur. Ajoutez l'option `--recreate-pods` de sorte que les pods du programme de mise à l'échelle automatique de cluster existants soient recréés pour récupérer les modifications de paramètre personnalisées.
    ```
    helm upgrade --set scanInterval=2m ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler -i --recreate-pods
    ```
    {: pre}

    Pour restaurer les valeurs par défaut de la charte :
    ```
    helm upgrade --reset-values ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler --recreate-pods
    ```
    {: pre}
3.  Pour vérifiez vos modifications, réexaminez les valeurs de la charte Helm.
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}


## Limitation d'exécution des applications uniquement à certains pools worker mis à l'échelle automatiquement
{: #ca_limit_pool}

Pour limiter un déploiement de pod à un pool de noeuds worker spécifique qui est géré par le programme de mise à l'échelle automatique de cluster, utilisez des libellés et le paramètre `nodeSelector` ou `nodeAffinity`.
Le paramètre `nodeAffinity` vous permet de mieux contrôler la façon dont le comportement de planification fonctionne pour faire correspondre des pods et des noeuds worker. Pour plus d'informations sur l'affectation de pods à des noeuds worker, [voir la documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/).
{: shortdesc}

**Avant de commencer** :
*  [Installez le plug-in `ibm-iks-cluster-autoscaler`](#ca_helm).
*  [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**Pour limiter l'exécution des pods à certains pools worker mis à l'échelle automatiquement** :

1.  Créez le pool worker avec le libellé de votre choix. Par exemple, votre libellé peut être `app: nginx`.
    ```
    ibmcloud ks worker-pool-create --name <name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_worker_nodes> --labels <key>=<value>
    ```
    {: pre}
2.  [Ajoutez le pool worker dans la configuration du programme de mise à l'échelle automatique de cluster](#ca_cm).
3.  Dans le modèle de spécification (spec) de votre pod, faites correspondre `nodeSelector` ou `nodeAffinity` au libellé que vous avez utilisé dans votre pool de noeuds worker.
    

    Exemple de paramètre `nodeSelector` :
    ```
    ...
    spec:
          containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
      nodeSelector:
        app: nginx
    ...
    ```
    {: codeblock}

    Exemple de paramètre `nodeAffinity` :
    ```
    spec:
          containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: app
              operator: In
              values:
                - nginx
    ```
    {: codeblock}
4.  Déployez le pod. En raison du libellé correspondant, le pod est planifié sur un noeud worker qui se trouve dans le pool worker avec ce libellé.
    ```
    kubectl apply -f pod.yaml
    ```
    {: pre}

<br />


## Augmentation des noeuds worker avant que le pool worker soit à court de ressources
{: #ca_scaleup}

Comme indiqué à la rubrique [Description du fonctionnement du programme de mise à l'échelle automatique de cluster](#ca_about) et dans la [foire aux questions (FAQ) de Kubernetes Cluster Autoscaler ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md), le programme de mise à l'échelle automatique de cluster augmente la capacité de vos pools worker en réponse aux ressources demandées de la charge de travail par rapport aux ressources disponibles du pool de noeuds worker. Cependant, vous préférez peut-être que ce programme augmente vos noeuds worker avant que le pool worker soit à court de ressources. Dans ce cas, votre charge de travail n'a pas besoin d'attendre que les noeuds worker soient mis à disposition car le pool worker est déjà mis à l'échelle pour répondre aux demandes de ressources.
{: shortdesc}

Le programme de mise à l'échelle automatique de cluster ne prend pas en charge la mise à l'échelle anticipée (provisionnement excessif) des pools worker. Cependant, vous pouvez configurer d'autres ressources Kubernetes pour utiliser ce programme et réaliser une mise à l'échelle anticipée.

<dl>
  <dt><strong>Pods de pause</strong></dt>
  <dd>Vous pouvez créer un déploiement pour déployer des [conteneurs de pause ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://stackoverflow.com/questions/48651269/what-are-the-pause-containers) dans des pods avec des demandes de ressources spécifiques, et affecter une priorité de pod faible à ce déploiement. Lorsque ces ressources sont requises par les charges de travail de priorité plus élevée, le pod de pause est préempté et devient un pod en attente. Cet événement déclenche le programme de mise à l'échelle automatique de cluster pour augmenter la capacité.<br><br>Pour plus d'informations sur la configuration d'un déploiement de pod de pause, voir [Kubernetes FAQ ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-can-i-configure-overprovisioning-with-cluster-autoscaler). Vous pouvez utiliser [cet exemple de fichier de configuration de provisionnement excessif![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/IBM-Cloud/kube-samples/blob/master/ibm-ks-cluster-autoscaler/overprovisioning-autoscaler.yaml) pour créer la classe de priorité, le compte de service et les déploiements.<p class="note">Si vous utilisez cette méthode, assurez-vous de comprendre comment fonctionne la [priorité de pod](/docs/containers?topic=containers-pod_priority#pod_priority) et comment définir la priorité de pod pour vos déploiements. Par exemple, si le pod de pause ne dispose pas de ressources suffisantes pour un pod de priorité élevée, le pod n'est pas préempté. La charge de travail de priorité plus élevée reste en attente, provoquant le déclenchement du programme de mise à l'échelle automatique de cluster pour augmenter la capacité. Pourtant dans ce cas précis, l'action de mise à l'échelle par augmentation n'est pas anticipée car la charge de travail que vous souhaitez exécuter ne peut pas être planifiée en raison d'un nombre de ressources insuffisant. </p></dd>

  <dt><strong>Mise à l'échelle automatique de pod horizontale (HPA)</strong></dt>
  <dd>Comme la mise à l'échelle automatique de pod horizontale est basée sur l'utilisation d'UC moyenne des pods, la limite d'utilisation d'UC que vous avez définie est atteinte avant même que les ressources du pool de noeuds worker ne soient effectivement épuisées. Plus de pods sont demandés, ce qui déclenche le programme de mise à l'échelle automatique de cluster pour augmenter le pool worker.<br><br>Pour plus d'informations sur la configuration de HPA, voir la [documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/).</dd>
</dl>

<br />


## Mise à jour de la charte Helm du programme de mise à l'échelle automatique de cluster
{: #ca_helm_up}

Vous pouvez mettre à jour la charte Helm du programme de mise à l'échelle automatique de cluster existante pour passer à la version la plus récente. Pour vérifier la version de votre charte Helm, exécutez la commande `helm ls | grep cluster-autoscaler`.
{: shortdesc}

Mise à jour vers la dernière version de la charte Helm à partir de la version 1.0.2 ou antérieure ? [Suivez ces instructions](#ca_helm_up_102).
{: note}

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Mettez à jour le référentiel Helm pour extraire la dernière version de toutes les chartes Helm figurant dans ce référentiel.
    ```
    helm repo update
    ```
    {: pre}

2.  Facultatif : téléchargez la charte Helm la plus récente sur votre machine locale. Ensuite, extrayez le package et consultez le fichier `release.md` pour trouver les informations relatives à la dernière édition.
    ```
    helm fetch iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

3.  Recherchez le nom de la charte Helm correspondant au programme de mise à l'échelle automatique de cluster que vous avez installée dans votre cluster.
    ```
    helm ls | grep cluster-autoscaler
    ```
    {: pre}

    Exemple de sortie :
    ```
    myhelmchart 	1       	Mon Jan  7 14:47:44 2019	DEPLOYED	ibm-ks-cluster-autoscaler-1.0.0  	kube-system
    ```
    {: screen}

4.  Mettez à jour la charte Helm du programme de mise à l'échelle automatique de cluster à la dernière version.
    ```
    helm upgrade --force --recreate-pods <helm_chart_name>  iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

5.  Vérifiez que la section `workerPoolsConfig.json` de la [mappe de configuration du programme de mise à l'échelle automatique de cluster](#ca_cm) est définie sur `"enabled": true` pour les pools worker que vous souhaitez mettre à l'échelle.
    ```
    kubectl describe cm iks-ca-configmap -n kube-system
    ```
    {: pre}

    Exemple de sortie :
    ```
    Name:         iks-ca-configmap
    Namespace:    kube-system
    Labels:       <none>
    Annotations:  kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","data":{"workerPoolsConfig.json":"[\n {\"name\": \"docs\",\"minSize\": 1,\"maxSize\": 3,\"enabled\":true}\n]\n"},"kind":"ConfigMap",...

    Data
    ====
    workerPoolsConfig.json:
    ----
    [
     {"name": "docs","minSize": 1,"maxSize": 3,"enabled":true}
    ]

    Events:  <none>
    ```
    {: screen}

### Mise à jour vers la dernière version de la charte Helm à partir de la version 1.0.2 ou antérieure
{: #ca_helm_up_102}

La version la plus récente de la charte Helm du programme de mise à l'échelle automatique de cluster nécessite la suppression complète des versions déjà installées de la charte. Si vous avez installé la charte Helm de version 1.0.2 ou antérieure, désinstallez d'abord cette version avant d'installer la dernière version de la charte Helm du programme de mise à l'échelle automatique de cluster.
{: shortdesc}

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Obtenez la mappe de configuration (configmap) de votre programme de mise à l'échelle automatique de cluster.
    ```
    kubectl get cm iks-ca-configmap -n kube-system -o yaml > iks-ca-configmap.yaml
    ```
    {: pre}
2.  Retirez tous les pools worker de la mappe de configuration en définissant la valeur `"enabled"` sur `false`.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
3.  Si vous avez appliqué des paramètres personnalisés à la charte Helm, notez ces paramètres.
    ```
    helm get values ibm-ks-cluster-autoscaler -a
    ```
    {: pre}
4.  Désinstallez la charte Helm actuelle.
    ```
    helm delete --purge ibm-ks-cluster-autoscaler
    ```
    {: pre}
5.  Mettez à jour le référentiel de la charte Helm pour obtenir la dernière version de la charte Helm du programme de mise à l'échelle automatique de cluster.
    ```
    helm repo update
    ```
    {: pre}
6.  Installez la charte Helm du programme de mise à l'échelle automatique de cluster la plus récente. Appliquez les éventuels paramètres personnalisés que vous avez utilisés précédemment avec l'indicateur `--set`, par exemple `scanInterval=2m`.
    ```
    helm install  iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler [--set <custom_settings>]
    ```
    {: pre}
7.  Appliquez la mappe de configuration du programme de mise à l'échelle automatique de cluster que vous avez récupérée précédemment afin d'activer la mise à l'échelle automatique de vos pools worker.
    ```
    kubectl apply -f iks-ca-configmap.yaml
    ```
    {: pre}
8.  Obtenez le pod du programme de mise à l'échelle automatique de cluster.
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
9.  Examinez la section **`Events`** du pod du programme de mise à l'échelle automatique de cluster et recherchez un événement **`ConfigUpdated`** afin de vérifier que la mappe de configuration a été mise à jour. Le message d'événement de votre mappe de configuration est au format suivant :`minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`.
    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    Exemple de sortie :
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
		Type     Reason         Age   From                                        Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

<br />


## Suppression du programme de mise à l'échelle automatique de cluster
{: #ca_rm}

Si vous ne souhaitez plus recourir à la mise à l'échelle automatique de vos pools worker, vous pouvez désinstaller la charte Helm du programme de mise à l'échelle automatique de cluster. Après la suppression, vous devez [redimensionner](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) ou [rééquilibrer](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) vos pools worker manuellement.
{: shortdesc}

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Dans la [mappe de configuration (configmap) du programme de mise à l'échelle automatique de cluster](#ca_cm), supprimez le pool worker en définissant la valeur de `"enabled"` sur `false`.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
    Exemple de sortie :
    ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
         {"name":"mypool","minSize":1,"maxSize":5,"enabled":false}
        ]
    kind: ConfigMap
    ...
    ```
2.  Répertoriez vos chartes Helm existantes et notez le nom du programme de mise à l'échelle automatique de cluster.
    ```
    helm ls
    ```
    {: pre}
3.  Supprimez de votre cluster les chartes Helm existantes.
    ```
    helm delete --purge <ibm-iks-cluster-autoscaler_name>
    ```
    {: pre}
