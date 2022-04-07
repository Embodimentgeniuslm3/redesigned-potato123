---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, audit

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



# Evénements {{site.data.keyword.cloudaccesstrailshort}}
{: #at_events}

Vous pouvez afficher et gérer des activités initiées par l'utilisateur ou effectuer un audit de ces activités dans votre cluster {{site.data.keyword.containerlong_notm}} en utilisant le service {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

{{site.data.keyword.containershort_notm}} génère deux types d'événements {{site.data.keyword.cloudaccesstrailshort}} :

* **Evénements de gestion de cluster** :
    * Ces événements sont automatiquement générés et transmis à {{site.data.keyword.cloudaccesstrailshort}}.
    * Vous pouvez afficher ces événements via le **domaine de compte** {{site.data.keyword.cloudaccesstrailshort}}.

* **Evénements d'audit du serveur d'API Kubernetes** :
    * Ces événements sont automatiquement générés, mais vous devez configurer votre cluster pour les transférer au service {{site.data.keyword.cloudaccesstrailshort}}.
    * Vous pouvez configurer votre cluster pour l'envoi d'événements au **domaine de compte** {{site.data.keyword.cloudaccesstrailshort}} ou à un **domaine d'espace**. Pour plus d'informations, voir [Envoi de journaux d'audit](/docs/containers?topic=containers-health#api_forward).

Pour plus d'informations sur le fonctionnement de ce service, voir la [documentation {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started). Pour plus d'informations sur les actions de Kubernetes suivies, consultez la [documentation Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/home/).

Actuellement, {{site.data.keyword.containerlong_notm}} n'est pas configuré pour utiliser {{site.data.keyword.at_full}}. Pour gérer des événements de gestion de cluster et des journaux d'audit d'API Kubernetes, continuez d'utiliser {{site.data.keyword.cloudaccesstrailfull_notm}} avec LogAnalysis.
{: note}

## Recherche d'informations pour les événements
{: #kube-find}

Vous pouvez surveiller les activités de votre cluster en examinant les journaux dans le tableau de bord Kibana.
{: shortdesc}

Pour surveiller les activités d'administration :

1. Connectez-vous à votre compte {{site.data.keyword.Bluemix_notm}}.
2. Dans le catalogue, mettez à disposition une instance du service {{site.data.keyword.cloudaccesstrailshort}} dans le même compte que votre instance {{site.data.keyword.containerlong_notm}}.
3. Dans l'onglet **Gérer** du tableau de bord {{site.data.keyword.cloudaccesstrailshort}}, sélectionnez le compte ou le domaine d'espace.
  * **Journaux du compte** : les événements de gestion de cluster et d'audit de serveur d'API Kubernetes sont disponibles dans le **domaine du compte** pour la région {{site.data.keyword.Bluemix_notm}} où ces événements sont générés.
  * **Journaux d'espace** : si vous avez indiqué un espace lorsque vous avez configuré votre cluster pour transmettre les événements d'audit de serveur d'API Kubernetes, ces événements sont disponibles dans le **domaine d'espace** associé à l'espace Cloud Foundry dans lequel le service {{site.data.keyword.cloudaccesstrailshort}} est mis à disposition.
4. Cliquez sur **Afficher dans Kibana**.
5. Définissez la période pour laquelle vous désirez consulter les journaux. La valeur par défaut est 24 heures.
6. Pour affiner votre recherche, vous pouvez cliquer sur l'icône d'édition du paramètre `ActivityTracker_Account_Search_in_24h` et ajouter des zones dans la colonne **Zones disponibles**.

Pour laisser d'autres utilisateurs afficher les événements liés aux espaces et aux comptes, voir [octroi de droits pour afficher les événements de compte](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions).
{: tip}

## Suivi des événements de gestion de cluster
{: #cluster-events}

Consultez la liste suivante d'événements de gestion de cluster envoyés à {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
<tr>
<th>Action</th>
<th>Description</th></tr><tr>
<td><code>containers-kubernetes.account-credentials.set</code></td>
<td>Les données d'identification de l'infrastructure dans une région pour un groupe de ressources sont définies.</td></tr><tr>
<td><code>containers-kubernetes.account-credentials.unset</code></td>
<td>La définition des données d'identification de l'infrastructure dans une région pour un groupe de ressources est annulée.</td></tr><tr>
<td><code>containers-kubernetes.alb.create</code></td>
<td>Un équilibreur de charge d'application (ALB) Ingress est créé.</td></tr><tr>
<td><code>containers-kubernetes.alb.delete</code></td>
<td>Un équilibreur de charge d'application (ALB) Ingress est supprimé.</td></tr><tr>
<td><code>containers-kubernetes.alb.get</code></td>
<td>Les informations sur l'ALB Ingress sont consultées.</td></tr><tr>
<td><code>containers-kubernetes.apikey.reset</code></td>
<td>Une clé d'API est réinitialisée pour une région et un groupe de ressources.</td></tr><tr>
<td><code>containers-kubernetes.cluster.create</code></td>
<td>Un cluster est créé.</td></tr><tr>
<td><code>containers-kubernetes.cluster.delete</code></td>
<td>Un cluster est supprimé.</td></tr><tr>
<td><code>containers-kubernetes.cluster-feature.enable</code></td>
<td>Une fonction, telle que le calcul sécurisé pour les noeuds worker bare metal, est activée sur un cluster.</td></tr><tr>
<td><code>containers-kubernetes.cluster.get</code></td>
<td>Les informations sur le cluster sont consultées.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.create</code></td>
<td>Une configuration d'acheminement des journaux est créée.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.delete</code></td>
<td>Une configuration d'acheminement des journaux est supprimée.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.get</code></td>
<td>Les informations d'une configuration d'acheminement des journaux sont consultées.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.update</code></td>
<td>Une configuration d'acheminement des journaux est mise à jour.</td></tr><tr>
<td><code>containers-kubernetes.logging-config.refresh</code></td>
<td>Une configuration d'acheminement des journaux est actualisée.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.create</code></td>
<td>Un filtre de consignation est créé.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.delete</code></td>
<td>Un filtre de consignation est supprimé.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.get</code></td>
<td>Les informations sur un filtre de consignation sont consultées.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.update</code></td>
<td>Un filtre de consignation est mis à jour.</td></tr><tr>
<td><code>containers-kubernetes.logging-autoupdate.changed</code></td>
<td>Le programme de mise à jour automatique du module complémentaire de consignation est activé ou désactivé.</td></tr><tr>
<td><code>containers-kubernetes.mzlb.create</code></td>
<td>Un équilibreur de charge pour zones multiples est créé.</td></tr><tr>
<td><code>containers-kubernetes.mzlb.delete</code></td>
<td>Un équilibreur de charge pour zones multiples est supprimé.</td></tr><tr>
<td><code>containers-kubernetes.service.bind</code></td>
<td>Un service est lié à un cluster.</td></tr><tr>
<td><code>containers-kubernetes.service.unbind</code></td>
<td>Un service est dissocié d'un cluster.</td></tr><tr>
<td><code>containers-kubernetes.subnet.add</code></td>
<td>Un sous-réseau existant de l'infrastructure IBM Cloud (SoftLayer) est ajouté dans un cluster.</td></tr><tr>
<td><code>containers-kubernetes.subnet.create</code></td>
<td>Un sous-réseau est créé.</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.add</code></td>
<td>Un sous-réseau géré par l'utilisateur est ajouté dans un cluster.</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.delete</code></td>
<td>Un sous-réseau géré par l'utilisateur est retiré d'un cluster.</td></tr><tr>
<td><code>containers-kubernetes.version.update</code></td>
<td>La version Kubernetes d'un noeud de maître cluster est mise à jour.</td></tr><tr>
<td><code>containers-kubernetes.worker.create</code></td>
<td>Un noeud worker est créé.</td></tr><tr>
<td><code>containers-kubernetes.worker.delete</code></td>
<td>Un noeud worker est supprimé.</td></tr><tr>
<td><code>containers-kubernetes.worker.get</code></td>
<td>Les informations sur un noeud worker sont consultées.</td></tr><tr>
<td><code>containers-kubernetes.worker.reboot</code></td>
<td>Un noeud worker est réamorcé.</td></tr><tr>
<td><code>containers-kubernetes.worker.reload</code></td>
<td>Un noeud worker est rechargé.</td></tr><tr>
<td><code>containers-kubernetes.worker.update</code></td>
<td>Un noeud worker est mis à jour.</td></tr>
</table>

## Suivi des événements d'audit de Kubernetes
{: #kube-events}

Consultez le tableau suivant pour obtenir la liste des événements d'audit de serveur d'API Kubernetes envoyés à {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

Avant de commencer : veillez à ce que votre cluster soit configuré pour transférer les [événements d'audit d'API Kubernetes](/docs/containers?topic=containers-health#api_forward).

<table>
    <th>Action</th>
    <th>Description</th><tr>
    <td><code>bindings.create</code></td>
    <td>Une liaison est créée.</td></tr><tr>
    <td><code>certificatesigningrequests.create</code></td>
    <td>Une demande de signature de certificat est créée.</td></tr><tr>
    <td><code>certificatesigningrequests.delete</code></td>
    <td>Une demande de signature de certificat est supprimée.</td></tr><tr>
    <td><code>certificatesigningrequests.patch</code></td>
    <td>Un correctif est appliqué à une demande de signature de certificat.</td></tr><tr>
    <td><code>certificatesigningrequests.update</code></td>
    <td>Une demande de signature de certificat est mise à jour.</td></tr><tr>
    <td><code>clusterbindings.create</code></td>
    <td>Une liaison de rôle de cluster est créée.</td></tr><tr>
    <td><code>clusterbindings.deleted</code></td>
    <td>Une liaison de rôle de cluster est supprimée.</td></tr><tr>
    <td><code>clusterbindings.patched</code></td>
    <td>Un correctif est appliqué à une liaison de rôle de cluster.</td></tr><tr>
    <td><code>clusterbindings.updated</code></td>
    <td>Une liaison de rôle de cluster est mise à jour.</td></tr><tr>
    <td><code>clusterroles.create</code></td>
    <td>Un rôle de cluster est créé.</td></tr><tr>
    <td><code>clusterroles.deleted</code></td>
    <td>Un rôle de cluster est supprimé.</td></tr><tr>
    <td><code>clusterroles.patched</code></td>
    <td>Un correctif est appliqué à un rôle de cluster.</td></tr><tr>
    <td><code>clusterroles.updated</code></td>
    <td>Un rôle de cluster est mis à jour.</td></tr><tr>
    <td><code>configmaps.create</code></td>
    <td>Une mappe de configuration est créée.</td></tr><tr>
    <td><code>configmaps.delete</code></td>
    <td>Une mappe de configuration est supprimée.</td></tr><tr>
    <td><code>configmaps.patch</code></td>
    <td>Un correctif est appliqué à une mappe de configuration.</td></tr><tr>
    <td><code>configmaps.update</code></td>
    <td>Une mappe de configuration est mise à jour.</td></tr><tr>
    <td><code>controllerrevisions.create</code></td>
    <td>Une révision de contrôleur est créée.</td></tr><tr>
    <td><code>controllerrevisions.delete</code></td>
    <td>Une révision de contrôleur est supprimée.</td></tr><tr>
    <td><code>controllerrevisions.patch</code></td>
    <td>Un correctif est appliqué à une révision de contrôleur.</td></tr><tr>
    <td><code>controllerrevisions.update</code></td>
    <td>Une révision de contrôleur est mise à jour.</td></tr><tr>
    <td><code>daemonsets.create</code></td>
    <td>Un ensemble de démons est créé.</td></tr><tr>
    <td><code>daemonsets.delete</code></td>
    <td>Un ensemble de démons est supprimé.</td></tr><tr>
    <td><code>daemonsets.patch</code></td>
    <td>Un correctif a été appliqué à un ensemble de démons.</td></tr><tr>
    <td><code>daemonsets.update</code></td>
    <td>Un ensemble de démons est mis à jour.</td></tr><tr>
    <td><code>deployments.create</code></td>
    <td>Un déploiement est créé.</td></tr><tr>
    <td><code>deployments.delete</code></td>
    <td>Un déploiement est supprimé.</td></tr><tr>
    <td><code>deployments.patch</code></td>
    <td>Un correctif est appliqué à un déploiement.</td></tr><tr>
    <td><code>deployments.update</code></td>
    <td>Un déploiement est mis à jour.</td></tr><tr>
    <td><code>events.create</code></td>
    <td>Un événement est créé.</td></tr><tr>
    <td><code>events.delete</code></td>
    <td>Un événement est supprimé.</td></tr><tr>
    <td><code>events.patch</code></td>
    <td>Un correctif est appliqué à un événement.</td></tr><tr>
    <td><code>events.update</code></td>
    <td>Un événement est mis à jour.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.create</code></td>
    <td>Dans Kubernetes version 1.8, une configuration de type externaladmissionhookconfiguration est créée.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.delete</code></td>
    <td>Dans Kubernetes version 1.8, une configuration de type externaladmissionhookconfiguration est supprimée.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.patch</code></td>
    <td>Dans Kubernetes version 1.8, un correctif est appliqué à une configuration de type externaladmissionhookconfiguration.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.update</code></td>
    <td>Dans Kubernetes version 1.8, une configuration de type externaladmissionhookconfiguration est mise à jour.</td></tr><tr>
    <td><code>horizontalpodautoscalers.create</code></td>
    <td>Une règle de mise à l'échelle de pod horizontale est créée.</td></tr><tr>
    <td><code>horizontalpodautoscalers.delete</code></td>
    <td>Une règle de mise à l'échelle de pod horizontale est supprimée.</td></tr><tr>
    <td><code>horizontalpodautoscalers.patch</code></td>
    <td>Un correctif est appliqué à une règle de mise à l'échelle de pod horizontale.</td></tr><tr>
    <td><code>horizontalpodautoscalers.update</code></td>
    <td>Une règle de mise à l'échelle de pod horizontale est mise à jour.</td></tr><tr>
    <td><code>ingresses.create</code></td>
    <td>Un équilibreur de charge d'application (ALB) Ingress est créé.</td></tr><tr>
    <td><code>ingresses.delete</code></td>
    <td>Un équilibreur de charge d'application (ALB) Ingress est supprimé.</td></tr><tr>
    <td><code>ingresses.patch</code></td>
    <td>Un correctif est appliqué à un équilibreur de charge d'application (ALB) Ingress.</td></tr><tr>
    <td><code>ingresses.update</code></td>
    <td>Un équilibreur de charge d'application (ALB) Ingress est mis à jour.</td></tr><tr>
    <td><code>jobs.create</code></td>
    <td>Un travail est créé.</td></tr><tr>
    <td><code>jobs.delete</code></td>
    <td>Un travail est supprimé.</td></tr><tr>
    <td><code>jobs.patch</code></td>
    <td>Un correctif est appliqué à un travail.</td></tr><tr>
    <td><code>jobs.update</code></td>
    <td>Un travail est mis à jour.</td></tr><tr>
    <td><code>localsubjectaccessreviews.create</code></td>
    <td>Une révision d'accès de type LocalSubjectAccessReview est créée.</td></tr><tr>
    <td><code>limitranges.create</code></td>
    <td>Une limite de plage est créée.</td></tr><tr>
    <td><code>limitranges.delete</code></td>
    <td>Une limite de plage est supprimée.</td></tr><tr>
    <td><code>limitranges.patch</code></td>
    <td>Un correctif est appliqué à une limite de plage.</td></tr><tr>
    <td><code>limitranges.update</code></td>
    <td>Une limite de plage est mise à jour.</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.create</code></td>
    <td>Une configuration de webhook de mutation est créée. </td></tr><tr>
    <td><code>mutatingwebhookconfigurations.delete</code></td>
    <td>Une configuration de webhook de mutation est supprimée. </td></tr><tr>
    <td><code>mutatingwebhookconfigurations.patch</code></td>
    <td>Un correctif est appliqué à une configuration de webhook de mutation. </td></tr><tr>
    <td><code>mutatingwebhookconfigurations.update</code></td>
    <td>Une configuration de webhook de mutation est mise à jour. </td></tr><tr>
    <td><code>namespaces.create</code></td>
    <td>Un espace de nom est créé.</td></tr><tr>
    <td><code>namespaces.delete</code></td>
    <td>Un espace de nom est supprimé.</td></tr><tr>
    <td><code>namespaces.patch</code></td>
    <td>Un correctif est appliqué à un espace de nom.</td></tr><tr>
    <td><code>namespaces.update</code></td>
    <td>Un espace de nom est mis à jour.</td></tr><tr>
    <td><code>networkpolicies.create</code></td>
    <td>Une règle réseau est créée.</td></tr><tr><tr>
    <td><code>networkpolicies.delete</code></td>
    <td>Une règle réseau est supprimée.</td></tr><tr>
    <td><code>networkpolicies.patch</code></td>
    <td>Un correctif est appliqué à une règle réseau.</td></tr><tr>
    <td><code>networkpolicies.update</code></td>
    <td>Une règle réseau est mise à jour.</td></tr><tr>
    <td><code>nodes.create</code></td>
    <td>Un noeud est créé.</td></tr><tr>
    <td><code>nodes.delete</code></td>
    <td>Un noeud est supprimé.</td></tr><tr>
    <td><code>nodes.patch</code></td>
    <td>Un correctif est appliqué à un noeud.</td></tr><tr>
    <td><code>nodes.update</code></td>
    <td>Un noeud est mis à jour.</td></tr><tr>
    <td><code>persistentvolumeclaims.create</code></td>
    <td>Une réservation de volume persistant est créée.</td></tr><tr>
    <td><code>persistentvolumeclaims.delete</code></td>
    <td>Une réservation de volume persistant est supprimée.</td></tr><tr>
    <td><code>persistentvolumeclaims.patch</code></td>
    <td>Un correctif est appliqué à une réservation de volume persistant.</td></tr><tr>
    <td><code>persistentvolumeclaims.update</code></td>
    <td>Une réservation de volume persistant est mise à jour.</td></tr><tr>
    <td><code>persistentvolumes.create</code></td>
    <td>Un volume persistant est créé.</td></tr><tr>
    <td><code>persistentvolumes.delete</code></td>
    <td>Un volume persistant est supprimé.</td></tr><tr>
    <td><code>persistentvolumes.patch</code></td>
    <td>Un correctif est appliqué à un volume persistant.</td></tr><tr>
    <td><code>persistentvolumes.update</code></td>
    <td>Un volume persistant est mis à jour.</td></tr><tr>
    <td><code>poddisruptionbudgets.create</code></td>
    <td>Un objet poddisruptionbudget est créé.</td></tr><tr>
    <td><code>poddisruptionbudgets.delete</code></td>
    <td>Un objet poddisruptionbudget est supprimé.</td></tr><tr>
    <td><code>poddisruptionbudgets.patch</code></td>
    <td>Un correctif est appliqué à un objet poddisruptionbudget.</td></tr><tr>
    <td><code>poddisruptionbudgets.update</code></td>
    <td>Un objet poddisruptionbudget est mis à jour.</td></tr><tr>
    <td><code>podpresets.create</code></td>
    <td>Un paramètre de pod prédéfini est créé.</td></tr><tr>
    <td><code>podpresets.deleted</code></td>
    <td>Un paramètre de pod prédéfini est supprimé.</td></tr><tr>
    <td><code>podpresets.patched</code></td>
    <td>Un correctif est appliqué à un paramètre de pod prédéfini.</td></tr><tr>
    <td><code>podpresets.updated</code></td>
    <td>Un paramètre de pod prédéfini est mis à jour.</td></tr><tr>
    <td><code>pods.create</code></td>
    <td>Un pod est créé.</td></tr><tr>
    <td><code>pods.delete</code></td>
    <td>Un pod est supprimé.</td></tr><tr>
    <td><code>pods.patch</code></td>
    <td>Un correctif est appliqué à un pod.</td></tr><tr>
    <td><code>pods.update</code></td>
    <td>Un pod est mis à jour.</td></tr><tr>
    <td><code>podsecuritypolicies.create</code></td>
    <td>Une politique de sécurité de pod est créée.</td></tr><tr>
    <td><code>podsecuritypolicies.delete</code></td>
    <td>Une politique de sécurité de pod est supprimée.</td></tr><tr>
    <td><code>podsecuritypolicies.patch</code></td>
    <td>Un correctif est appliqué à une politique de sécurité de pod.</td></tr><tr>
    <td><code>podsecuritypolicies.update</code></td>
    <td>Une politique de sécurité de pod est mise à jour.</td></tr><tr>
    <td><code>podtemplates.create</code></td>
    <td>Un modèle de pod est créé.</td></tr><tr>
    <td><code>podtemplates.delete</code></td>
    <td>Un modèle de pod est supprimé.</td></tr><tr>
    <td><code>podtemplates.patch</code></td>
    <td>Un correctif est appliqué à un modèle de pod.</td></tr><tr>
    <td><code>podtemplates.update</code></td>
    <td>Un modèle de pod est mis à jour.</td></tr><tr>
    <td><code>replicasets.create</code></td>
    <td>Un jeu de répliques est créé.</td></tr><tr>
    <td><code>replicasets.delete</code></td>
    <td>Un jeu de répliques est supprimé.</td></tr><tr>
    <td><code>replicasets.patch</code></td>
    <td>Un correctif est appliqué à un jeu de répliques.</td></tr><tr>
    <td><code>replicasets.update</code></td>
    <td>Un jeu de répliques est mis à jour.</td></tr><tr>
    <td><code>replicationcontrollers.create</code></td>
    <td>Un contrôleur de réplication est créé.</td></tr><tr>
    <td><code>replicationcontrollers.delete</code></td>
    <td>Un contrôleur de réplication est supprimé.</td></tr><tr>
    <td><code>replicationcontrollers.patch</code></td>
    <td>Un correctif est appliqué à un contrôleur de réplication.</td></tr><tr>
    <td><code>replicationcontrollers.update</code></td>
    <td>Un contrôleur de réplication est mis à jour.</td></tr><tr>
    <td><code>resourcequotas.create</code></td>
    <td>Un quota de ressources est créé.</td></tr><tr>
    <td><code>resourcequotas.delete</code></td>
    <td>Un quota de ressources est supprimé.</td></tr><tr>
    <td><code>resourcequotas.patch</code></td>
    <td>Un correctif est appliqué à un quota de ressources.</td></tr><tr>
    <td><code>resourcequotas.update</code></td>
    <td>Un quota de ressources est mis à jour.</td></tr><tr>
    <td><code>rolebindings.create</code></td>
    <td>Une liaison de rôle est créée.</td></tr><tr>
    <td><code>rolebindings.deleted</code></td>
    <td>Une liaison de rôle est supprimée.</td></tr><tr>
    <td><code>rolebindings.patched</code></td>
    <td>Un correctif est appliqué à une liaison de rôle.</td></tr><tr>
    <td><code>rolebindings.updated</code></td>
    <td>Une liaison de rôle est mise à jour.</td></tr><tr>
    <td><code>roles.create</code></td>
    <td>Un rôle est créé.</td></tr><tr>
    <td><code>roles.deleted</code></td>
    <td>Un rôle est supprimé.</td></tr><tr>
    <td><code>roles.patched</code></td>
    <td>Un correctif est appliqué à un rôle.</td></tr><tr>
    <td><code>roles.updated</code></td>
    <td>Un rôle est mis à jour.</td></tr><tr>
    <td><code>secrets.create</code></td>
    <td>Une valeur confidentielle (secret) est créée.</td></tr><tr>
    <td><code>secrets.deleted</code></td>
    <td>Une valeur confidentielle (secret) est supprimée.</td></tr><tr>
    <td><code>secrets.get</code></td>
    <td>Une valeur confidentielle (secret) est consultée.</td></tr><tr>
    <td><code>secrets.patch</code></td>
    <td>Un correctif est appliqué à une valeur confidentielle (secret).</td></tr><tr>
    <td><code>secrets.updated</code></td>
    <td>Une valeur confidentielle (secret) est mise à jour.</td></tr><tr>
    <td><code>selfsubjectaccessreviews.create</code></td>
    <td>Une révision d'accès de type SelfSubjectAccessReview est créée.</td></tr><tr>
    <td><code>selfsubjectrulesreviews.create</code></td>
    <td>Une révision d'accès de type SelfSubjectRulesReview est créée.</td></tr><tr>
    <td><code>subjectaccessreviews.create</code></td>
    <td>Une révision d'accès de type SubjectAccessReview est créée.</td></tr><tr>
    <td><code>serviceaccounts.create</code></td>
    <td>Un compte de service est créé.</td></tr><tr>
    <td><code>serviceaccounts.deleted</code></td>
    <td>Un compte de service est supprimé.</td></tr><tr>
    <td><code>serviceaccounts.patch</code></td>
    <td>Un correctif est appliqué à un compte de service.</td></tr><tr>
    <td><code>serviceaccounts.updated</code></td>
    <td>Un compte de service est mis à jour.</td></tr><tr>
    <td><code>services.create</code></td>
    <td>Un service est créé.</td></tr><tr>
    <td><code>services.deleted</code></td>
    <td>Un service est supprimé.</td></tr><tr>
    <td><code>services.patch</code></td>
    <td>Un correctif est appliqué à un service.</td></tr><tr>
    <td><code>services.updated</code></td>
    <td>Un service est mis à jour.</td></tr><tr>
    <td><code>statefulsets.create</code></td>
    <td>Un ensemble avec état est créé.</td></tr><tr>
    <td><code>statefulsets.delete</code></td>
    <td>Un ensemble avec état est supprimé.</td></tr><tr>
    <td><code>statefulsets.patch</code></td>
    <td>Un correctif est appliqué à un ensemble avec état.</td></tr><tr>
    <td><code>statefulsets.update</code></td>
    <td>Un ensemble avec état est mis à jour.</td></tr><tr>
    <td><code>tokenreviews.create</code></td>
    <td>Une révision de jeton est créée.</td></tr><tr>
    <td><code>validatingwebhookconfigurations.create</code></td>
    <td>Une validation de configuration de webhook est créée. </td></tr><tr>
    <td><code>validatingwebhookconfigurations.delete</code></td>
    <td>Une validation de configuration de webhook est supprimée. </td></tr><tr>
    <td><code>validatingwebhookconfigurations.patch</code></td>
    <td>Un correctif est appliqué à une validation de configuration de webhook. </td></tr><tr>
    <td><code>validatingwebhookconfigurations.update</code></td>
    <td>Une validation de configuration de webhook est mise à jour. </td></tr>
</table>
