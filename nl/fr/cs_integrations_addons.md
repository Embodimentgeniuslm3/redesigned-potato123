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

# Ajout de services à l'aide de modules complémentaires gérés
{: #managed-addons}

Ajoutez rapidement des technologies open source à votre cluster à l'aide de modules complémentaires gérés.
{: shortdesc}

**Que sont les modules complémentaires gérés ?** </br>
Les modules complémentaires {{site.data.keyword.containerlong_notm}} gérés permettent d'améliorer facilement votre cluster avec des fonctions open source, telles que Istio ou Knative. La version de l'outil open source que vous ajoutez à votre cluster est testée par IBM et approuvée pour être utilisée dans {{site.data.keyword.containerlong_notm}}.

**Comment la facturation et le support fonctionnent-ils pour les modules complémentaires gérés ?** </br>
Les modules complémentaires gérés sont entièrement intégrés dans l'organisation de support {{site.data.keyword.Bluemix_notm}}. Pour toute question ou problématique relative à l'utilisation des modules complémentaires gérés, vous pouvez utiliser l'un des canaux de support {{site.data.keyword.containerlong_notm}}. Pour plus d'informations, voir [Aide et assistance](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

Si l'outil que vous ajoutez à votre cluster entraîne des coûts, ceux-ci sont automatiquement intégrés et répertoriés dans le cadre de votre facturation {{site.data.keyword.containerlong_notm}}. Le cycle de facturation est déterminé par {{site.data.keyword.Bluemix_notm}} en fonction du moment où vous activez le module complémentaire dans votre cluster.

**Quelles sont les limitations à prendre en compte ?** </br>
Si vous avez installé le [contrôleur d'admission Container Image Security Enforcer](/docs/services/Registry?topic=registry-security_enforce#security_enforce) dans votre cluster, vous ne pouvez pas activer les modules complémentaires gérés dans votre cluster.

## Ajout de modules complémentaires gérés
{: #adding-managed-add-ons}

Pour activer un module complémentaire géré dans votre cluster, vous utilisez la [commande `ibmcloud ks cluster-addon-enable <addon_name> --cluster <cluster_name_or_ID>`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable). Lorsque vous activez le module complémentaire géré, une version prise en charge de l'outil, y compris toutes les ressources Kubernetes, est installée automatiquement dans votre cluster. Reportez-vous à la documentation de chacun des modules complémentaires gérés afin d'identifier les prérequis auxquels votre cluster doit satisfaire pour pourvoir les installer.

Pour plus d'informations sur les prérequis pour chaque module complémentaire, voir :

- [Istio](/docs/containers?topic=containers-istio#istio)
- [Knative](/docs/containers?topic=containers-serverless-apps-knative)

## Mise à jour des modules complémentaires gérés
{: #updating-managed-add-ons}

Les versions de chaque module complémentaire géré sont testées par {{site.data.keyword.Bluemix_notm}} et approuvées pour être utilisées dans {{site.data.keyword.containerlong_notm}}. Pour mettre à jour les composants d'un module complémentaire vers la dernière version prise en charge par {{site.data.keyword.containerlong_notm}}, procédez comme indiqué ci-après.
{: shortdesc}

1. Vérifiez si la version de vos modules complémentaires est la plus récente. Les modules complémentaires signalés par `* (<version> latest)` peuvent être mis à jour.
   ```
   ibmcloud ks cluster-addons --cluster <mycluster>
   ```
   {: pre}

   Exemple de sortie :
   ```
   OK
   Name      Version
   istio     1.1.5
   knative   0.5.2
   ```
   {: screen}

2. Sauvegardez les ressources, par exemple des fichiers de configuration de services ou d'applications, que vous avez créées ou modifiées dans l'espace de nom généré par le module complémentaire. Par exemple, le module complémentaire Istio utilise `istio-system`, et le module complémentaire Knative utilise `knative-serving`, `knative-monitoring`, `knative-eventing` et `knative-build`.
   Exemple de commande :
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

3. Sauvegardez dans un fichier YAML sur votre machine locale les ressources Kubernetes qui ont été générées automatiquement dans l'espace de nom du module complémentaire géré. Ces ressources sont générées à l'aide de définitions de ressource personnalisées (CRD).
   1. Procurez-vous les CRD pour votre module complémentaire à partir de l'espace de nom utilisé par ce dernier. Par exemple, pour le module complémentaire Istio, procurez-vous les CRD à partir de l'espace de nom `istio-system`.
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. Sauvegardez les ressources créées à partir de ces CRD.

4. Facultatif pour Knative : si vous avez modifié l'une quelconque des ressources suivantes, procurez-vous le fichier YAML et sauvegardez-les sur votre machine locale. Si vous avez modifié l'une quelconque de ces ressources et que vous souhaitez utiliser à la place celle qui est installée par défaut, vous pouvez supprimer la ressource. Au bout de quelques minutes, la ressource est recréée avec les valeurs par défaut installées.
  <table summary="Tableau des ressources Knative">
  <caption>Ressources Knative</caption>
  <thead><tr><th>Nom de ressource</th><th>Type de ressource</th><th>Espace de nom</th></tr></thead>
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

  Exemple de commande :
  ```
  kubectl get cm config-autoscaler -n knative-serving -o yaml > config-autoscaler.yaml
  ```
  {: pre}

5. Si vous avez activé les modules complémentaires `istio-sample-bookinfo` et `istio-extras`, désactivez-les.
   1. Désactivez le module complémentaire `istio-sample-bookinfo`.
      ```
      ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
      ```
      {: pre}

   2. Désactivez le module complémentaire `istio-extras`.
      ```
      ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
      ```
      {: pre}

6. Désactivez le module complémentaire.
   ```
   ibmcloud ks cluster-addon-disable <add-on_name> --cluster <cluster_name_or_ID> -f
   ```
   {: pre}

7. Avant de passer à l'étape suivante, vérifiez que les ressources issues du module complémentaire dans les espaces de nom du module complémentaire ou les espaces de nom de module complémentaire proprement dits sont retirés.
   * Par exemple, si vous mettez à jour le module complémentaire `istio-extras`, vous souhaiterez peut-être vérifier que les services `grafana`, `kiali` et `jaeger-*` sont retirés de l'espace de nom `istio-system`.
     ```
     kubectl get svc -n istio-system
     ```
     {: pre}
   * Par exemple, si vous mettez à jour le module complémentaire `knative`, vous souhaiterez peut-être vérifier que les espaces de nom `knative-serving`, `knative-monitoring`, `knative-eventing`, `knative-build` et `istio-system` sont supprimés. Les espaces de nom peuvent avoir le **statut** `Terminating` pendant quelques minutes avant d'être entièrement supprimés.
     ```
     kubectl get namespaces -w
     ```
     {: pre}

8. Choisissez la version de module complémentaire vers laquelle vous souhaitez effectuer une mise à jour.
   ```
   ibmcloud ks addon-versions
   ```
   {: pre}

9. Réactivez le module complémentaire. Utilisez l'option `--version` pour spécifier la version que vous souhaitez installer. Si aucune version n'est indiquée, la version par défaut est installée.
   ```
   ibmcloud ks cluster-addon-enable <add-on_name> --cluster <cluster_name_or_ID> --version <version>
   ```
   {: pre}

10. Appliquez les ressources CRD que vous avez sauvegardées à l'étape 2.
    ```
    kubectl apply -f <file_name> -n <namespace>
    ```
    {: pre}

11. Facultatif pour Knative : si vous avez sauvegardé l'une quelconque des ressources à l'étape 3, réappliquez-les. Exemple de commande :
    ```
    kubectl apply -f config-autoscaler.yaml -n knative-serving
    ```
    {: pre}

12. Facultatif pour Istio : réactivez les modules complémentaires `istio-extras` et `istio-sample-bookinfo`. Utilisez la même version pour ces modules complémentaires que pour le module complémentaire `istio`.
    1. Activez le module complémentaire `istio-extras`.
       ```
       ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

    2. Activez le module complémentaire `istio-sample-bookinfo`.
       ```
       ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

13. Facultatif pour Istio : si vous utilisez des sections TLS dans vos fichiers de configuration de passerelle, vous devez supprimer et recréer les passerelles de sorte que Envoy puisse accéder aux valeurs confidentielles. 
  ```
  kubectl delete gateway mygateway
  kubectl create -f mygateway.yaml
  ```
  {: pre}
