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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Traitement des incidents liés à la consignation et à la surveillance
{: #cs_troubleshoot_health}

Lorsque vous utilisez {{site.data.keyword.containerlong}}, tenez compte des techniques décrites ci-dessous pour identifier et résoudre les incidents liés à la consignation et à la surveillance.
{: shortdesc}

Si vous rencontrez un problème d'ordre plus général, expérimentez le [débogage de cluster](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}

## Les journaux ne s'affichent pas
{: #cs_no_logs}

{: tsSymptoms}
Lorsque vous accédez au tableau de bord Kibana, vos journaux ne s'affichent pas.

{: tsResolve}
Ci-dessous figurent les motifs pour lesquels les journaux de votre cluster peuvent ne pas apparaître, ainsi que les étapes de résolution correspondantes :

<table>
<caption>Traitement des incidents liés aux journaux qui ne s'affichent pas</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Motifs</th>
      <th>Procédure de résolution du problème</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>Aucune configuration de consignation n'a été définie.</td>
    <td>Pour que les journaux soient envoyés, vous devez créer une configuration de consignation. Pour cela, voir <a href="/docs/containers?topic=containers-health#logging">Configuration de la consignation de cluster</a>.</td>
  </tr>
  <tr>
    <td>L'état du cluster n'indique pas <code>Normal</code>.</td>
    <td>Pour vérifier l'état de votre cluster, voir <a href="/docs/containers?topic=containers-cs_troubleshoot#debug_clusters">Débogage des clusters</a>.</td>
  </tr>
  <tr>
    <td>Le quota de stockage des journaux a été atteint.</td>
    <td>Pour augmenter vos limites de stockage de journaux, reportez-vous à la <a href="/docs/services/CloudLogAnalysis/troubleshooting?topic=cloudloganalysis-error_msgs">documentation {{site.data.keyword.loganalysislong_notm}}</a>.</td>
  </tr>
  <tr>
    <td>Si vous avez spécifié un espace lors de la création du cluster, les droits d'accès Responsable, Développeur ou Auditeur n'ont pas été affectés au propriétaire du compte sur cet espace.</td>
      <td>Pour modifier les droits d'accès du propriétaire du compte :
      <ol><li>Pour identifier le propriétaire du compte pour le cluster, exécutez la commande <code>ibmcloud ks api-key-info --cluster &lt;cluster_name_or_ID&gt;</code>. </li>
      <li>Pour attribuer au propriétaire du compte les droits d'accès Responsable, Développeur ou Auditeur dans {{site.data.keyword.containerlong_notm}} sur cet espace, voir <a href="/docs/containers?topic=containers-users">Gestion de l'accès au cluster</a>.</li>
      <li>Pour actualiser le jeton de journalisation après une modification des droits d'accès, exécutez la commande <code>ibmcloud ks logging-config-refresh --cluster &lt;cluster_name_or_ID&gt;</code>.</li></ol></td>
    </tr>
    <tr>
      <td>Vous avez une configuration de consignation pour votre application avec un lien symbolique dans le chemin de votre application.</td>
      <td><p>Pour que les journaux soient envoyés, vous devez utiliser un chemin absolu dans votre configuration de consignation autrement vos journaux ne pourront pas être lus. Si votre chemin est monté sur votre noeud worker, il peut créer un lien symbolique. </p> <p>Exemple : si le chemin spécifié est <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> mais que les journaux sont acheminés vers <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>, les journaux ne pourront pas être lus.</p></td>
    </tr>
  </tbody>
</table>

Pour tester les modifications apportées lors de la résolution des incidents, vous pouvez déployer l'exemple de pod *Noisy*, lequel génère plusieurs événements de journal sur un noeud worker dans votre cluster.

Avant de commencer : [connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Créez le fichier de configuration `deploy-noisy.yaml`.
    ```
    apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
      - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
      ```
      {: codeblock}

2. Exécutez le fichier de configuration dans le contexte de cluster.
    ```
    kubectl apply -f noisy.yaml
    ```
    {:pre}

3. Au bout de quelques minutes, vos journaux s'affichent dans le tableau de bord Kibana. Pour accéder au tableau de bord Kibana, accédez à l'une des URL suivantes et sélectionnez le compte {{site.data.keyword.Bluemix_notm}} dans lequel vous avez créé le cluster. Si vous avez spécifié un espace lors de la création du cluster, accédez à la place à cet espace.
    - Sud et Est des Etats-Unis : `https://logging.ng.bluemix.net`
    - Sud du Royaume-Uni : `https://logging.eu-gb.bluemix.net`
    - Europe centrale : `https://logging.eu-fra.bluemix.net`
    - Asie-Pacifique sud : `https://logging.au-syd.bluemix.net`

<br />


## Le tableau de bord Kubernetes n'affiche pas de graphiques d'utilisation
{: #cs_dashboard_graphs}

{: tsSymptoms}
Lorsque vous accédez au tableau de bord Kubernetes, les graphiques d'utilisation ne s'affichent pas.

{: tsCauses}
Parfois, après la mise à jour d'un cluster ou le réamorçage d'un noeud worker, le pod `kube-dashboard` ne se met pas à jour.

{: tsResolve}
Supprimez le pod `kube-dashboard` pour forcer un redémarrage. Le pod est recréé avec des règles RBAC afin d'accéder à `heapster` pour obtenir les informations d'utilisation.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## Le quota de journaux est trop faible
{: #quota}

{: tsSymptoms}
Vous avez configuré une configuration de consignation dans votre cluster pour transférer les journaux à {{site.data.keyword.loganalysisfull}}. Lorsque vous les affichez, les journaux contiennent un message d'erreur semblable à ce qui suit :

```
You have reached the daily quota that is allocated to the Bluemix space {Space GUID} for the IBM® Cloud Log Analysis instance {Instance GUID}. Your current daily allotment is XXX for Log Search storage, which is retained for a period of 3 days, during which it can be searched for in Kibana. This does not affect your log retention policy in Log Collection storage. To upgrade your plan so that you can store more data in Log Search storage per day, upgrade the Log Analysis service plan for this space. For more information about service plans and how to upgrade your plan, see Plans.
```
{: screen}

{: tsResolve}
Examinez les motifs suivants pour savoir pourquoi vous avez atteint votre quota de journaux et les étapes à suivre pour y remédier :

<table>
<caption>Traitement des erreurs liées au stockage de journaux</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Motifs</th>
      <th>Procédure de résolution du problème</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>Un ou plusieurs pods produisent un nombre élevé de journaux.</td>
    <td>Vous pouvez libérer de l'espace de stockage pour les journaux en empêchant le transfert de journaux de pods spécifiques. Créez un [filtre de consignation](/docs/containers?topic=containers-health#filter-logs) pour ces pods.</td>
  </tr>
  <tr>
    <td>Vous dépassez les 500 Mo d'affectation quotidienne pour le stockage de journaux du plan Lite.</td>
    <td>[Calculez d'abord le quota de recherche et l'utilisation quotidienne](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota) du domaine de vos journaux. Vous pouvez ensuite augmenter votre quota de stockage de journaux en effectuant une [mise à niveau de votre plan de service {{site.data.keyword.loganalysisshort_notm}}](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan).</td>
  </tr>
  <tr>
    <td>Vous dépassez le quota de stockage de journaux prévu pour votre plan payant actuel.</td>
    <td>[Calculez d'abord le quota de recherche et l'utilisation quotidienne](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota) du domaine de vos journaux. Vous pouvez ensuite augmenter votre quota de stockage de journaux en effectuant une [mise à niveau de votre plan de service {{site.data.keyword.loganalysisshort_notm}}](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan).</td>
  </tr>
  </tbody>
</table>

<br />


## Les lignes des journaux sont trop longues
{: #long_lines}

{: tsSymptoms}
Vous avez configuré une configuration de consignation dans votre cluster pour transférer les journaux à {{site.data.keyword.loganalysisfull_notm}}. Lorsque vous affichez les journaux, vous voyez un message de journal très long. Par ailleurs, dans Kibana, vous ne pourrez voir que les 600 à 700 derniers caractères du message de journal.

{: tsCauses}
Un message de journal long est susceptible d'être tronqué en raison de sa longueur avant d'être collecté par Fluentd, par conséquent, le journal risque de ne pas être analysé correctement par Fluentd avant d'être transféré à {{site.data.keyword.loganalysisshort_notm}}.

{: tsResolve}
Pour limiter la longueur des lignes, vous pouvez configurer votre propre consignateur pour obtenir une longueur maximale dans la propriété `stack_trace` dans chaque journal. Par exemple, si vous utilisez un consignateur Log4j, vous pouvez utiliser un paramètre [`EnhancedPatternLayout` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/EnhancedPatternLayout.html) pour limiter la valeur de `stack_trace` à 15 ko.

## Aide et assistance
{: #health_getting_help}

Vous avez encore des problèmes avec votre cluster ?
{: shortdesc}

-  Dans le terminal, vous êtes averti des mises à jour disponibles pour l'interface de ligne de commande `ibmcloud` et les plug-ins. Veillez à maintenir votre interface de ligne de commande à jour pour pouvoir utiliser l'ensemble des commandes et des indicateurs.
-   Pour voir si {{site.data.keyword.Bluemix_notm}} est disponible, [vérifiez la page Statut d'{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/status?selected=status).
-   Publiez une question sur le site [{{site.data.keyword.containerlong_notm}} Slack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-container-service.slack.com).
    Si vous n'utilisez pas un ID IBM pour votre compte {{site.data.keyword.Bluemix_notm}}, [demandez une invitation](https://bxcs-slack-invite.mybluemix.net/) sur ce site Slack.
    {: tip}
-   Consultez les forums pour établir si d'autres utilisateurs ont rencontré le même problème. Lorsque vous utilisez les forums pour poser une question, balisez votre question de sorte que les équipes de développement {{site.data.keyword.Bluemix_notm}} la voient.
    -   Si vous avez des questions d'ordre technique sur le développement ou le déploiement de clusters ou d'applications à l'aide d'{{site.data.keyword.containerlong_notm}}, publiez-les sur le site [Stack Overflow ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) en leur adjoignant les balises `ibm-cloud`, `kubernetes` et `containers`.
    -   Pour toute question sur le service et les instructions de mise en route, utilisez le forum [IBM Developer Answers ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Incluez les balises `ibm-cloud` et `containers`.
    Voir [Comment obtenir de l'aide](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) pour plus d'informations sur l'utilisation des forums.
-   Contactez le support IBM en ouvrant un cas. Pour savoir comment ouvrir un cas de support IBM ou obtenir les niveaux de support et la gravité des cas, voir [Contacter le support](/docs/get-support?topic=get-support-getting-customer-support).
Lorsque vous signalez un problème, incluez l'ID de votre cluster. Pour identifier l'ID du cluster, exécutez la commande `ibmcloud ks clusters`. Vous pouvez également utiliser l'[outil de débogage et de diagnostic d'{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) pour regrouper et exporter des informations pertinentes de votre cluster à partager avec le support IBM.
{: tip}

