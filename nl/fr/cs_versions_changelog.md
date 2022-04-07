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


# Journal des modifications de version
{: #changelog}

Affichez des informations sur les modifications de version correspondant à des mises à jour de version principale, de version secondaire ou de correctif disponibles pour vos clusters Kubernetes {{site.data.keyword.containerlong}}. Les modifications comprennent des mises à jour pour les composants de Kubernetes et d'{{site.data.keyword.Bluemix_notm}} Provider.
{:shortdesc}

Sauf indication contraire dans les journaux de modifications, la version du fournisseur {{site.data.keyword.containerlong_notm}} active les API et les fonctions Kubernetes version bêta. Les fonctions alpha Kubernetes, qui sont susceptibles d'être modifiées, sont désactivées. 

Pour plus d'informations sur les versions principales et secondaires, les versions de correctifs et les actions de préparation entre les versions secondaires, voir [Versions Kubernetes](/docs/containers?topic=containers-cs_versions).
{: tip}

Pour plus d'informations sur les modifications apportées depuis la version précédente, voir les journaux de modifications suivants.
-  [Journal des modifications](#114_changelog) - Version 1.14.
-  [Journal des modifications](#113_changelog) - Version 1.13.
-  [Journal des modifications](#112_changelog) - Version 1.12.
-  **Déprécié ** : [Journal des modifications](#111_changelog) - Version 1.11.
-  [Archive](#changelog_archive) des journaux de modifications des versions non prises en charge.

Certains journaux de modifications concernent les _groupes de correctifs de noeud worker_ et s'appliquent uniquement aux noeuds worker. Vous devez [appliquer ces correctifs](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) pour garantir la conformité de vos noeuds worker aux normes de sécurité. Ces groupes de correctifs peuvent avoir une version supérieure à celle du maître car certains groupes de correctifs de génération sont spécifiques à des noeuds worker. D'autres journaux de modifications pour les _groupes de correctifs du maître_ et s'appliquent uniquement au noeud du maître cluster. Les groupes de correctifs du maître ne s'appliquent pas forcément automatiquement. Vous pouvez choisir de les [appliquer manuellement](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update). Pour plus d'informations sur les types de correctif, voir [Types de mise à jour](/docs/containers?topic=containers-cs_versions#update_types).
{: note}

</br>

## Journal des modifications - Version 1.14
{: #114_changelog}

### Journal des modifications pour la version 1.14.2_1521, publié le 4 mars 2019
{: #1142_1521}

Le tableau suivant présente les modifications incluses dans le correctif 1.14.2_1521 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.7.4_1509">
<caption>Modifications effectuées depuis la version 1.14.1_1519</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration du DNS de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Correction d'un bogue pouvant laisser les pods Kubernetes DNS et CoreDNS s'exécuter après des opérations `create` ou `update` de cluster. </td>
</tr>
<tr>
<td>Configuration à haute disponibilité de maître cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration afin de réduire les échecs de connectivité de réseau maître durant une mise à jour de maître. </td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Voir les [notes sur l'édition d'etcd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-10844 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) et [CVE-2019-5436 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.14.1-71</td>
<td>v1.14.2-100</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.14.2. </td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.14.1</td>
<td>v1.14.2</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.2).</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>Voir les [notes sur l'édition de Kubernetes Metrics Server![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-10844 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) et [CVE-2019-5436 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.14.1_1519, publié le 20 mai 2019
{: #1141_1519}

Le tableau suivant présente les modifications incluses dans le correctif 1.14.1_1519 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.14.1_1518">
<caption>Modifications effectuées depuis la version 1.14.1_1518</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau Ubuntu 16.04</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2018-12126 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) et [CVE-2018-12130 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Noyau Ubuntu 18.04</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2018-12126 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) et [CVE-2018-12130 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.14.1_1518, publié le 13 mai 2019
{: #1141_1518}

Le tableau suivant présente les modifications incluses dans le correctif 1.14.1_1518 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.14.1_1516">
<caption>Modifications effectuées depuis la version 1.14.1_1516</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy du maître cluster HA</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Voir les [notes sur l'édition de HAProxy ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La mise à jour résout la vulnérabilité [CVE-2019-6706 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration de règle d'audit du serveur d'API Kubernetes pour ne pas consigner l'URL en lecture seule `/openapi/v2*`. De plus, la configuration du gestionnaire de contrôleur Kubernetes a augmenté la durée de validité des certificats `kubelet` signés, de 1 à 3 ans. </td>
</tr>
<tr>
<td>Configuration du client OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Le client OpenVPN `vpn-*` dans l'espace de nom `kube-system` affecte désormais à `dnsPolicy` la valeur `Default` pour empêcher l'échec du pod lorsque le DNS de cluster est indisponible. </td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>e7182c7</td>
<td>13c7ef0</td>
<td>Mise à jour d'une image pour traiter les vulnérabilités [CVE-2016-7076 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076) et [CVE-2017-1000368 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.14.1_1516, publié le 7 mai 2019
{: #1141_1516}

Le tableau suivant présente les modifications incluses dans le correctif 1.14.1_1516 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.13.5_1519">
<caption>Modifications effectuées depuis la version 1.13.5_1519</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.4.4</td>
<td>v3.6.1</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.6/release-notes/). </td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.6</td>
<td>1.3.1</td>
<td>Voir les [notes sur l'édition de CoreDNS![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://coredns.io/2019/01/13/coredns-1.3.1-release/). La mise à jour inclut l'ajout d'une [port de métriques![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://coredns.io/plugins/metrics/) sur le service DNS de cluster. <br><br>CoreDNS est désormais le seul fournisseur DNS de cluster pris en charge. Si vous mettez à jour un cluster vers la version 1.14 de Kubernetes à partir d'une version précédente et si vous avez utilisé le fournisseur KubeDNS, ce dernier est automatiquement vers CoreDNS durant la mise à jour du cluster. Pour plus d'informations ou pour tester CoreDNS avant d'effectuer la mise à jour, voir [Configuration du fournisseur DNS de cluster](https://cloud.ibm.com/docs/containers?topic=containers-cluster_dns#cluster_dns).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>9ff3fda</td>
<td>ed0dafc</td>
<td>Image mise à jour pour la vulnérabilité [CVE-2019-1543 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.5-107</td>
<td>v1.14.1-71</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.14.1. Par ailleurs, `calicoctl` est mis à jour vers la version 3.6.1. Résolution des mises à jour vers les équilibreurs de charge version 2.0 avec un seul noeud worker disponible pour les pods d'équilibreur de charge. Les équilibreurs de charge privés prennent désormais être exécutés sur des [noeuds worker de périphérie privés](/docs/containers?topic=containers-edge#edge).</td>
</tr>
<tr>
<td>Politiques de sécurité de pod IBM</td>
<td>N/A</td>
<td>N/A</td>
<td>Les [politiques de sécurité de pod IBM](/docs/containers?topic=containers-psp#ibm_psp) sont mises à jour pour prendre en charge la fonction [RunAsGroup ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#users-and-groups). </td>
</tr>
<tr>
<td>Configuration de `kubelet`</td>
<td>N/A</td>
<td>N/A</td>
<td>Affectez à l'option `--pod-max-pids` la valeur `14336` pour empêcher un seul pod de consommer tous les ID de processus sur un noeud worker.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.14.1</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.1) et le [blogue Kubernetes 1.14 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/blog/2019/03/25/kubernetes-1-14-release-announcement/).<br><br>Les politiques de contrôle d'accès à base de rôles (RBAC) par défaut de Kubernetes n'octroient plus d'accès aux [API de reconnaissance et de vérification des droits aux utilisateurs authentifiés![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles). Cette modification ne s'applique qu'aux clusters version 1.14. Si vous mettez à jour un cluster à partir d'une version antérieure, les utilisateurs non authentifiés ont toujours accès aux API de reconnaissance et de vérification des droits. </td>
</tr>
<tr>
<td>Configuration des contrôleurs d'admission Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td><ul>
<li>Ajout de `NodeRestriction` à l'option `--enable-admission-plugins` pour le serveur d'API Kubernetes du cluster et configuration des ressources de cluster connexes afin de prendre en charge cette amélioration de sécurité. </li>
<li>Retrait de `Initializers` de l'option `--enable-admission-plugins` et de `admissionregistration.k8s.io/v1alpha1=true` de l'option `--runtime-config` pour le serveur d'API Kubernetes du cluster car ces API ne sont plus prises en charge. En revanche, vous pouvez utiliser les [webhooks d'admission Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/).</li></ul></td>
</tr>
<tr>
<td>Mise à l'échelle automatique de DNS avec Kubernetes</td>
<td>1.3.0</td>
<td>1.4.0</td>
<td>Voir les [notes sur l'édition de la fonction Kubernetes de mise à l'échelle automatique (autoscaler) de DNS ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.4.0).</td>
</tr>
<tr>
<td>Configuration de Kubernetes feature gates</td>
<td>N/A</td>
<td>N/A</td>
<td><ul>
  <li>Ajout de `RuntimeClass=false` pour désactiver la sélection de la configuration d'exécution de conteneur. </li>
  <li>Retrait de `ExperimentalCriticalPodAnnotation=true` car l'annotation de pod `scheduler.alpha.kubernetes.io/critical-pod` n'est plus prise en charge. En revanche, vous pouvez utiliser la [priorité de pod Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/docs/containers?topic=containers-pod_priority#pod_priority).</li></ul></td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>e132aa4</td>
<td>e7182c7</td>
<td>Image mise à jour pour la vulnérabilité [CVE-2019-11068 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

<br />


## Journal des modifications - Version 1.13
{: #113_changelog}

### Journal des modifications pour la version 1.13.6_1524, publié le 4 juin 2019
{: #1136_1524}

Le tableau suivant présente les modifications incluses dans le correctif 1.13.6_1524 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.13.6_1522">
<caption>Modifications effectuées depuis la version 1.13.6_1522</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration du DNS de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Correction d'un bogue pouvant laisser les pods Kubernetes DNS et CoreDNS s'exécuter après des opérations `create` ou `update` de cluster. </td>
</tr>
<tr>
<td>Configuration à haute disponibilité de maître cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration afin de réduire les échecs de connectivité de réseau maître durant une mise à jour de maître. </td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Voir les [notes sur l'édition d'etcd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-10844 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) et [CVE-2019-5436 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>Voir les [notes sur l'édition de Kubernetes Metrics Server![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-10844 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) et [CVE-2019-5436 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.13.6_1522, publié le 20 mai 2019
{: #1136_1522}

Le tableau suivant présente les modifications incluses dans le correctif 1.13.6_1522 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.13.6_1521">
<caption>Modifications effectuées depuis la version 1.13.6_1521</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau Ubuntu 16.04</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2018-12126 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) et [CVE-2018-12130 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Noyau Ubuntu 18.04</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2018-12126 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) et [CVE-2018-12130 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.13.6_1521, publié le 13 mai 2019
{: #1136_1521}

Le tableau suivant présente les modifications incluses dans le correctif 1.13.6_1521 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.13.5_1519">
<caption>Modifications effectuées depuis la version 1.13.5_1519</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy du maître cluster HA</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Voir les [notes sur l'édition de HAProxy ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La mise à jour résout la vulnérabilité [CVE-2019-6706 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Image mise à jour pour la vulnérabilité [CVE-2019-1543 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.5-107</td>
<td>v1.13.6-139</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.13.6. En outre, résolution du processus de mise à jour pour l'équilibreur de charge version 2.0 qui ne possède un seul noeud worker disponible pour les pods d'équilibreur de charge. </td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.13.6</td>
<td>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.6). </td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration de règle d'audit du serveur d'API Kubernetes pour ne pas consigner l'URL en lecture seule `/openapi/v2*`. De plus, la configuration du gestionnaire de contrôleur Kubernetes a augmenté la durée de validité des certificats `kubelet` signés, de 1 à 3 ans. </td>
</tr>
<tr>
<td>Configuration du client OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Le client OpenVPN `vpn-*` dans l'espace de nom `kube-system` affecte désormais à `dnsPolicy` la valeur `Default` pour empêcher l'échec du pod lorsque le DNS de cluster est indisponible. </td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2016-7076 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) et [CVE-2019-11068 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.13.5_1519, publié le 29 avril 2019
{: #1135_1519}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.13.5_1519.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.13.5_1518">
<caption>Modifications effectuées depuis la version 1.13.5_1518</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour des packages Ubuntu installés.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.5</td>
<td>1.2.6</td>
<td>Voir les [notes sur l'édition de containerd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.2.6).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.13.5_1518, publié le 15 avril 2019
{: #1135_1518}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.13.5_1518.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.13.5_1517">
<caption>Modifications effectuées depuis la version 1.13.5_1517</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour pour les packages Ubuntu installés, notamment `systemd` pour traiter les vulnérabilités [CVE-2019-3842 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.13.5_1517, publié le 8 avril 2019
{: #1135_1517}

Le tableau suivant présente les modifications incluses dans le correctif 1.13.5_1517.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.13.4_1516">
<caption>Modifications depuis la version 1.13.4_1516</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.4.0</td>
<td>v3.4.4</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.4/releases/#v344). La mise à jour résout la vulnérabilité [CVE-2019-9946 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Proxy du maître cluster HA</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Voir les [notes sur l'édition de HAProxy ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La mise à jour résout les vulnérabilités [CVE-2018-0732 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) et [CVE-2019-1559 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.4-86</td>
<td>v1.13.5-107</td>
<td>Mise à jour pour prendre en charge les éditions Kubernetes 1.13.5 et Calico 3.4.4.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.4</td>
<td>v1.13.5</td>
<td>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.5).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2017-12447 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Noyau Ubuntu 16.04</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2019-9213 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Noyau Ubuntu 18.04</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2019-9213 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.13.4_1516, publié le 1er avril 2019
{: #1134_1516}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.13.4_1516.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.13.4_1515">
<caption>Modifications depuis la version 1.13.4_1515</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilisation des ressources de noeud worker</td>
<td>N/A</td>
<td>N/A</td>
<td>Augmentation des réserves de mémoire pour kubelet et containerd afin d'empêcher ces composants d'être à court de ressources. Pour plus d'informations, voir [Réserves de ressources de noeud worker](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Journal des modifications du maître - Groupe de correctifs 1.13.4_1515, publié le 26 mars 2019
{: #1134_1515}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs du maître 1.13.4_1515.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.13.4_1513">
<caption>Modifications depuis la version 1.13.4_1513</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration du DNS de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Processus de mise à jour corrigé à partir de Kubernetes version 1.11 afin d'empêcher la mise à jour de faire passer le fournisseur DNS de cluster à CoreDNS. Vous pourrez toujours [configurer CoreDNS comme fournisseur DNS de cluster](/docs/containers?topic=containers-cluster_dns#set_coredns) après la mise à jour.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>345</td>
<td>346</td>
<td>Image mise à jour pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>166</td>
<td>167</td>
<td>Résout les erreurs de type `context deadline exceeded` et `timeout` sporadiques lors de la gestion des secrets Kubernetes. De plus, résout les mises à jour apportées au service de gestion de clés susceptibles de laisser les secrets Kubernetes existants non chiffrés. La mise à jour comprend un correctif pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Equilibreur de charge et moniteur d'équilibrage de charge pour {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Image mise à jour pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.13.4_1513, publié le 20 mars 2019
{: #1134_1513}

Le tableau suivant présente les modifications incluses dans le correctif 1.13.4_1513.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.13.4_1510">
<caption>Modifications depuis la version 1.13.4_1510</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration du DNS de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Correction d'un bogue qui pouvait entraîner l'échec d'opérations du maître cluster, par exemple une actualisation (`refresh`) ou une mise à jour (`update`), lorsque le DNS de cluster non utilisé devait être réduit.</td>
</tr>
<tr>
<td>Configuration de proxy à haute disponibilité de maître cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration pour mieux traiter les échecs de connexion intermittente avec le maître cluster.</td>
</tr>
<tr>
<td>Configuration du serveur CoreDNS</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration du serveur CoreDNS pour prendre en charge [plusieurs fichiers Corefiles ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://coredns.io/2017/07/23/corefile-explained/) après une mise à jour de la version du cluster Kubernetes à partir de la version 1.12.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.4</td>
<td>1.2.5</td>
<td>Voir les [notes sur l'édition de containerd![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.2.5). La mise à jour comprend une amélioration du correctif pour la vulnérabilité [CVE-2019-5736 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Mise à jour des pilotes GPU à la version [418.43 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.nvidia.com/object/unix.html). La mise à jour comprend un correctif pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>344</td>
<td>345</td>
<td>Prise en charge des [noeuds finaux de service privé](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Noyau</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour traiter la vulnérabilité [CVE-2019-6133 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>136</td>
<td>166</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-16890 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) et [CVE-2019-3823 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-10779 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) et [CVE-2019-7663 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.13.4_1510, publié le 4 mars 2019
{: #1134_1510}

Le tableau suivant présente les modifications incluses dans le correctif 1.13.4_1510.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.13.2_1509">
<caption>Modifications depuis la version 1.13.2_1509</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Fournisseur DNS de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Augmentation de la limite de mémoire de pod pour les serveurs CoreDNS et DNS Kubernetes de `170Mi` à `400Mi` afin de traiter plus de services de cluster.</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Mise à jour des images pour traiter la vulnérabilité [CVE-2019-6454 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.2-62</td>
<td>v1.13.4-86</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.13.4. Correction de problèmes de connectivité périodique des équilibreurs de charge de version 1.0 qui définissaient le paramètre `externalTrafficPolicy` avec la valeur `local`. Mise à jour des événements d'équilibreur de charge de version 1.0 et 2.0 pour utiliser les liens à jour de la documentation {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>342</td>
<td>344</td>
<td>Modification de l'image de base du système d'exploitation pour passer de Fedora à Alpine. Image mise à jour pour traiter la vulnérabilité [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>122</td>
<td>136</td>
<td>Augmentation du délai d'attente du client dans {{site.data.keyword.keymanagementservicefull_notm}}. Image mise à jour pour traiter la vulnérabilité [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.2</td>
<td>v1.13.4</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.4). La mise à jour résout les vulnérabilités [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) et [CVE-2019-1002100 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout du paramètre `ExperimentalCriticalPodAnnotation=true` à l'option `--feature-gates`. Ce paramètre permet de migrer les pods de l'annotation `scheduler.alpha.kubernetes.io/critical-pod` dépréciée pour [prendre en charge la priorité de pod de Kubernetes](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Equilibreur de charge et moniteur d'équilibrage de charge pour {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Serveur et client OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2019-1559 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2019-6454 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.13.2_1509, publié le 27 février 2019
{: #1132_1509}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.13.2_1509.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.13.2_1508">
<caption>Modifications depuis la version 1.13.2_1508</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour traiter la vulnérabilité [CVE-2018-19407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.13.2_1508, publié le 15 février 2019
{: #1132_1508}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.13.2_1508.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.13.2_1507">
<caption>Modifications depuis la version 1.13.2_1507</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration de proxy à haute disponibilité de maître cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Remplacement de la valeur `spec.priorityClassName` de la configuration de pod par `system-node-critical` et définition de la valeur de `spec.priority` sur `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.2</td>
<td>1.2.4</td>
<td>Voir les [notes sur l'édition de containerd![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.2.4). La mise à jour résout la vulnérabilité [CVE-2019-5736 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configuration du `kubelet` de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Activation de l'indicateur feature gate `ExperimentalCriticalPodAnnotation` pour éviter l'expulsion critique de pod statique. Définissez l'option `event-qps` avec la valeur `0` pour empêcher la création d'événement de limitation de débit.</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.13.2_1507, publié le 5 février 2019
{: #1132_1507}

Le tableau suivant présente les modifications incluses dans le correctif 1.13.2_1507.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.4_1535">
<caption>Modifications depuis la version 1.12.4_1535</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.4.0</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.4/releases/#v340).</td>
</tr>
<tr>
<td>Fournisseur DNS de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Le serveur CoreDNS est désormais le fournisseur de DNS de cluster par défaut pour les nouveaux clusters. Si vous effectuez une mise à jour du cluster existant à la version 1.13 qui utilise KubeDNS comme fournisseur de DNS de cluster, KubeDNS reste le fournisseur de DNS de cluster. Cependant, vous pouvez choisir d'[utiliser CoreDNS à la place](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.2.2</td>
<td>Voir les [notes sur l'édition de containerd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.2.2).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.2</td>
<td>1.2.6</td>
<td>Voir les [notes sur l'édition de CoreDNS![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coredns/coredns/releases/tag/v1.2.6). En outre, la configuration de CoreDNS est mise à jour pour [prendre en charge plusieurs fichiers Corefiles ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://coredns.io/2017/07/23/corefile-explained/).</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Voir les [notes sur l'édition d'etcd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coreos/etcd/releases/v3.3.11). Par ailleurs, les suites de chiffrement prises en charge sur etcd sont désormais limitées à un sous-ensemble avec un niveau de chiffrement élevé (128 bits ou plus).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Images mises à jour pour traiter les vulnérabilités [CVE-2019-3462 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) et [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.4-118</td>
<td>v1.13.2-62</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.13.2. Par ailleurs, `calicoctl` est mis à jour à la version 3.4.0. Correction de mises à jour de configuration inutiles vers les équilibreurs de charge de version 2.0 sur les modifications de statut de noeud worker.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>338</td>
<td>342</td>
<td>Le plug-in de stockage de fichiers est mis à jour comme suit :
<ul><li>Prise en charge du provisionnement dynamique avec [planification tenant compte de la topologie de volume](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Les erreurs de suppression de réservation de volume persistant (PVC) sont ignorées si le stockage est déjà supprimé.</li>
<li>Ajout d'une annotation de message d'échec pour les PVC ayant échoué.</li>
<li>Optimisation des paramètres d'élection du leader du contrôleur du service de mise à disposition de stockage et de période de resynchronisation et augmentation du délai de mise à disposition de 30 minutes à 1 heure.</li>
<li>Vérification des droits d'utilisateur avant de commencer la mise à disposition.</li>
<li>Ajout d'un paramètre de tolérance `CriticalAddonsOnly` dans les déploiements de `ibm-file-plugin` et `ibm-storage-watcher` dans l'espace de nom `kube-system`.</li></ul></td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>111</td>
<td>122</td>
<td>Ajout d'une logique de nouvelle tentative pour éviter les échecs temporaires lorsque les valeurs confidentielles (secrets) de Kubernetes sont gérées par {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.13.2</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.2).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration de règle d'audit du serveur d'API Kubernetes pour inclure les métadonnées de journalisation pour les demandes `cluster-admin` et consigner le corps des demandes `create`, `update` et `patch` de charge de travail.</td>
</tr>
<tr>
<td>Mise à l'échelle automatique de DNS avec Kubernetes</td>
<td>1.2.0</td>
<td>1.3.0</td>
<td>Voir les [notes sur l'édition de la fonction Kubernetes de mise à l'échelle automatique (autoscaler) de DNS ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.3.0).</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Mise à jour d'une image pour traiter les vulnérabilités [CVE-2018-0734 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) et [CVE-2018-5407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Ajout d'un paramètre de tolérance `CriticalAddonsOnly` au déploiement de `vpn` dans l'espace de nom `kube-system`. Par ailleurs, la configuration de pod est désormais obtenue à partir d'une valeur confidentielle (secret) au lieu d'une mappe de configuration (configmap).</td>
</tr>
<tr>
<td>Serveur OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Mise à jour d'une image pour traiter les vulnérabilités [CVE-2018-0734 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) et [CVE-2018-5407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Correctif de sécurité pour traiter la vulnérabilité [CVE-2018-16864 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

<br />


## Journal des modifications - Version 1.12
{: #112_changelog}

Passez en revue le journal des modifications de la version 1.12.
{: shortdesc}

### Journal des modifications pour la version 1.12.9_1555, publié le 4 juin 2019
{: #1129_1555}

Le tableau suivant présente les modifications incluses dans le correctif 1.12.9_1555 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.8_1553">
<caption>Modifications effectuées depuis la version 1.12.8_1553</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration du DNS de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Correction d'un bogue pouvant laisser les pods Kubernetes DNS et CoreDNS s'exécuter après des opérations `create` ou `update` de cluster. </td>
</tr>
<tr>
<td>Configuration à haute disponibilité de maître cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration afin de réduire les échecs de connectivité de réseau maître durant une mise à jour de maître. </td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Voir les [notes sur l'édition d'etcd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-10844 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) et [CVE-2019-5436 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.8-210</td>
<td>v1.12.9-227</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.12.9. </td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.8</td>
<td>v1.12.9</td>
<td>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.9).</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>Voir les [notes sur l'édition de Kubernetes Metrics Server![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-10844 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) et [CVE-2019-5436 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.12.8_1553, publié le 20 mai 2019
{: #1128_1533}

Le tableau suivant présente les modifications incluses dans le correctif 1.12.8_1553 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.8_1553">
<caption>Modifications effectuées depuis la version 1.12.8_1553</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau Ubuntu 16.04</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2018-12126 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) et [CVE-2018-12130 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Noyau Ubuntu 18.04</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2018-12126 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) et [CVE-2018-12130 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.12.8_1552, publié le 13 mai 2019
{: #1128_1552}

Le tableau suivant présente les modifications incluses dans le correctif 1.12.8_1552 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.7_1550">
<caption>Modifications effectuées depuis la version 1.12.7_1550</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy du maître cluster HA</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Voir les [notes sur l'édition de HAProxy ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La mise à jour résout la vulnérabilité [CVE-2019-6706 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Image mise à jour pour la vulnérabilité [CVE-2019-1543 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.7-180</td>
<td>v1.12.8-210</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.12.8. En outre, résolution du processus de mise à jour pour l'équilibreur de charge version 2.0 qui ne possède un seul noeud worker disponible pour les pods d'équilibreur de charge. </td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.7</td>
<td>v1.12.8</td>
<td>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.8). </td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration de règle d'audit du serveur d'API Kubernetes pour ne pas consigner l'URL en lecture seule `/openapi/v2*`. De plus, la configuration du gestionnaire de contrôleur Kubernetes a augmenté la durée de validité des certificats `kubelet` signés, de 1 à 3 ans. </td>
</tr>
<tr>
<td>Configuration du client OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Le client OpenVPN `vpn-*` dans l'espace de nom `kube-system` affecte désormais à `dnsPolicy` la valeur `Default` pour empêcher l'échec du pod lorsque le DNS de cluster est indisponible. </td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2016-7076 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) et [CVE-2019-11068 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.12.7_1550, publié le 29 avril 2019
{: #1127_1550}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.12.7_1550 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.7_1549">
<caption>Modifications effectuées depuis la version 1.12.7_1549</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour des packages Ubuntu installés.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>Voir les [notes sur l'édition de containerd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.1.7).</td>
</tr>
</tbody>
</table>


### Journal des modifications de noeud worker - Groupe de correctifs 1.12.7_1549, publié le 15 avril 2019
{: #1127_1549}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.12.7_1549.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.7_1548">
<caption>Modifications depuis la version 1.12.7_1548</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour pour les packages Ubuntu installés, notamment `systemd` pour traiter les vulnérabilités [CVE-2019-3842 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.12.7_1548, publié le 8 avril 2019
{: #1127_1548}

Le tableau suivant présente les modifications incluses dans le correctif 1.12.7_1548.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.6_1547">
<caption>Modifications depuis la version 1.12.6_1547</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.3/releases/#v336). La mise à jour résout la vulnérabilité [CVE-2019-9946 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Proxy du maître cluster HA</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Voir les [notes sur l'édition de HAProxy ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La mise à jour résout les vulnérabilités [CVE-2018-0732 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) et [CVE-2019-1559 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.6-157</td>
<td>v1.12.7-180</td>
<td>Mise à jour pour prendre en charge les éditions Kubernetes 1.12.7 et Calico 3.3.6.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.6</td>
<td>v1.12.7</td>
<td>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.7).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2017-12447 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Noyau Ubuntu 16.04</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2019-9213 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Noyau Ubuntu 18.04</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2019-9213 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.12.6_1547, publié le 1er avril 2019
{: #1126_1547}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.12.6_1547.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.6_1546">
<caption>Modifications depuis la version 1.12.6_1546</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilisation des ressources de noeud worker</td>
<td>N/A</td>
<td>N/A</td>
<td>Augmentation des réserves de mémoire pour kubelet et containerd afin d'empêcher ces composants d'être à court de ressources. Pour plus d'informations, voir [Réserves de ressources de noeud worker](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>


### Journal des modifications du maître - Groupe de correctifs 1.12.6_1546, publié le 26 mars 2019
{: #1126_1546}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs du maître 1.12.6_1546.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.6_1544">
<caption>Modifications depuis la version 1.12.6_1544</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>345</td>
<td>346</td>
<td>Image mise à jour pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>166</td>
<td>167</td>
<td>Résout les erreurs de type `context deadline exceeded` et `timeout` sporadiques lors de la gestion des secrets Kubernetes. De plus, résout les mises à jour apportées au service de gestion de clés susceptibles de laisser les secrets Kubernetes existants non chiffrés. La mise à jour comprend un correctif pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Equilibreur de charge et moniteur d'équilibrage de charge pour {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Image mise à jour pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.12.6_1544, publié le 20 mars 2019
{: #1126_1544}

Le tableau suivant présente les modifications incluses dans le correctif 1.12.6_1544.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.6_1541">
<caption>Modifications depuis la version 1.12.6_1541</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration du DNS de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Correction d'un bogue qui pouvait entraîner l'échec d'opérations du maître cluster, par exemple une actualisation (`refresh`) ou une mise à jour (`update`), lorsque le DNS de cluster non utilisé devait être réduit.</td>
</tr>
<tr>
<td>Configuration de proxy à haute disponibilité de maître cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration pour mieux traiter les échecs de connexion intermittente avec le maître cluster.</td>
</tr>
<tr>
<td>Configuration du serveur CoreDNS</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration du serveur CoreDNS pour prendre en charge [plusieurs fichiers Corefiles ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://coredns.io/2017/07/23/corefile-explained/).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Mise à jour des pilotes GPU à la version [418.43 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.nvidia.com/object/unix.html). La mise à jour comprend un correctif pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>344</td>
<td>345</td>
<td>Prise en charge des [noeuds finaux de service privé](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Noyau</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour traiter la vulnérabilité [CVE-2019-6133 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>136</td>
<td>166</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-16890 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) et [CVE-2019-3823 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-10779 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) et [CVE-2019-7663 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.12.6_1541, publié le 4 mars 2019
{: #1126_1541}

Le tableau suivant présente les modifications incluses dans le correctif 1.12.6_1541.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.5_1540">
<caption>Modifications depuis la version 1.12.5_1540</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Fournisseur DNS de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Augmentation de la limite de mémoire de pod pour les serveurs CoreDNS et DNS Kubernetes de `170Mi` à `400Mi` afin de traiter plus de services de cluster.</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Mise à jour des images pour traiter la vulnérabilité [CVE-2019-6454 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.5-137</td>
<td>v1.12.6-157</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.12.6. Correction de problèmes de connectivité périodique des équilibreurs de charge de version 1.0 qui définissaient le paramètre `externalTrafficPolicy` avec la valeur `local`. Mise à jour des événements d'équilibreur de charge de version 1.0 et 2.0 pour utiliser les liens à jour de la documentation {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>342</td>
<td>344</td>
<td>Modification de l'image de base du système d'exploitation pour passer de Fedora à Alpine. Image mise à jour pour traiter la vulnérabilité [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>122</td>
<td>136</td>
<td>Augmentation du délai d'attente du client dans {{site.data.keyword.keymanagementservicefull_notm}}. Image mise à jour pour traiter la vulnérabilité [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.5</td>
<td>v1.12.6</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.6). La mise à jour résout les vulnérabilités [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) et [CVE-2019-1002100 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout du paramètre `ExperimentalCriticalPodAnnotation=true` à l'option `--feature-gates`. Ce paramètre permet de migrer les pods de l'annotation `scheduler.alpha.kubernetes.io/critical-pod` dépréciée pour [prendre en charge la priorité de pod de Kubernetes](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Equilibreur de charge et moniteur d'équilibrage de charge pour {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Serveur et client OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2019-1559 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2019-6454 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.12.5_1540, publié le 27 février 2019
{: #1125_1540}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.12.5_1540.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.5_1538">
<caption>Modifications depuis la version 1.12.5_1538</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour traiter la vulnérabilité [CVE-2018-19407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.12.5_1538, publié le 15 février 2019
{: #1125_1538}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.12.5_1538.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.5_1537">
<caption>Modifications depuis la version 1.12.5_1537</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration de proxy à haute disponibilité de maître cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Remplacement de la valeur `spec.priorityClassName` de la configuration de pod par `system-node-critical` et définition de la valeur de `spec.priority` sur `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>Voir les [notes sur l'édition de containerd![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.1.6). La mise à jour résout la vulnérabilité [CVE-2019-5736 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configuration du `kubelet` de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Activation de l'indicateur feature gate `ExperimentalCriticalPodAnnotation` pour éviter l'expulsion critique de pod statique.</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.12.5_1537, publié le 5 février 2019
{: #1125_1537}

Le tableau suivant présente les modifications incluses dans le correctif 1.12.5_1537.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.4_1535">
<caption>Modifications depuis la version 1.12.4_1535</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Voir les [notes sur l'édition d'etcd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coreos/etcd/releases/v3.3.11). Par ailleurs, les suites de chiffrement prises en charge sur etcd sont désormais limitées à un sous-ensemble avec un niveau de chiffrement élevé (128 bits ou plus).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Images mises à jour pour traiter les vulnérabilités [CVE-2019-3462 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) et [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.4-118</td>
<td>v1.12.5-137</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.12.5. Par ailleurs, `calicoctl` est mis à jour à la version 3.3.1. Correction de mises à jour de configuration inutiles vers les équilibreurs de charge de version 2.0 sur les modifications de statut de noeud worker.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>338</td>
<td>342</td>
<td>Le plug-in de stockage de fichiers est mis à jour comme suit :
<ul><li>Prise en charge du provisionnement dynamique avec [planification tenant compte de la topologie de volume](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Les erreurs de suppression de réservation de volume persistant (PVC) sont ignorées si le stockage est déjà supprimé.</li>
<li>Ajout d'une annotation de message d'échec pour les PVC ayant échoué.</li>
<li>Optimisation des paramètres d'élection du leader du contrôleur du service de mise à disposition de stockage et de période de resynchronisation et augmentation du délai de mise à disposition de 30 minutes à 1 heure.</li>
<li>Vérification des droits d'utilisateur avant de commencer la mise à disposition.</li></ul></td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>111</td>
<td>122</td>
<td>Ajout d'une logique de nouvelle tentative pour éviter les échecs temporaires lorsque les valeurs confidentielles (secrets) de Kubernetes sont gérées par {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.12.5</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.5).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration de règle d'audit du serveur d'API Kubernetes pour inclure les métadonnées de journalisation pour les demandes `cluster-admin` et consigner le corps des demandes `create`, `update` et `patch` de charge de travail.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Mise à jour d'une image pour traiter les vulnérabilités [CVE-2018-0734 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) et [CVE-2018-5407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Par ailleurs, la configuration de pod est désormais obtenue à partir d'une valeur confidentielle (secret) au lieu d'une mappe de configuration (configmap).</td>
</tr>
<tr>
<td>Serveur OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Mise à jour d'une image pour traiter les vulnérabilités [CVE-2018-0734 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) et [CVE-2018-5407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Correctif de sécurité pour traiter la vulnérabilité [CVE-2018-16864 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.12.4_1535, publié le 28 janvier 2019
{: #1124_1535}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.12.4_1535.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.4_1534">
<caption>Modifications depuis la version 1.12.4_1534</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour pour les packages Ubuntu installés, notamment `apt` pour traiter les vulnérabilités [CVE-2019-3462 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) et [USN-3863-1 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>


### Journal des modifications pour la version 1.12.4_1534, publié le 21 janvier 2019
{: #1124_1534}

Le tableau suivant présente les modifications incluses dans le correctif 1.12.3_1534.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.4_1533">
<caption>Modifications depuis la version 1.12.3_1533</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.3-91</td>
<td>v1.12.4-118</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.12.4.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.3</td>
<td>v1.12.4</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.4).</td>
</tr>
<tr>
<td>Image Kubernetes add-on resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Voir les [notes sur l'édition de Kubernetes pour l'image add-on resizer ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Tableau de bord Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>Voir les [notes sur l'édition sur le tableau de bord Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). La mise à jour résout la vulnérabilité [CVE-2018-18264 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).</td>
</tr>
<tr>
<td>Programme d'installation de GPU</td>
<td>390.12</td>
<td>410.79</td>
<td>Mise à jour des pilotes GPU installés à la version 410.79.</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.12.3_1533, publié le 7 janvier 2019
{: #1123_1533}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.12.3_1533.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.3_1532">
<caption>Modifications depuis la version 1.12.3_1532</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour traiter les vulnérabilités [CVE-2017-5753, CVE-2018-18690 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.12.3_1532, publié le 17 décembre 2018
{: #1123_1532}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.12.2_1532.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.3_1531">
<caption>Modifications depuis la version 1.12.3_1531</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour des packages Ubuntu installés.</td>
</tr>
</tbody>
</table>


### Journal des modifications pour la version 1.12.3_1531, publié le 5 décembre 2018
{: #1123_1531}

Le tableau suivant présente les modifications incluses dans le correctif 1.12.3_1531.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.2_1530">
<caption>Modifications depuis la version 1.12.2_1530</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.2-68</td>
<td>v1.12.3-91</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.12.3.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.2</td>
<td>v1.12.3</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.3). La mise à jour résout la vulnérabilité [CVE-2018-1002105 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.12.2_1530, publié le 4 décembre 2018
{: #1122_1530}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.12.2_1530.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.2_1529">
<caption>Modifications depuis la version 1.12.2_1529</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilisation des ressources de noeud worker</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de groupes cgroups pour le kubelet et containerd pour empêcher ces composants d'être à court de ressources. Pour plus d'informations, voir [Réserves de ressources de noeud worker](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>



### Journal des modifications pour la version 1.12.2_1529, publié le 27 novembre 2018
{: #1122_1529}

Le tableau suivant présente les modifications incluses dans le correctif 1.12.2_1529.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.2_1528">
<caption>Modifications depuis la version 1.12.2_1528</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.3/releases/#v331). La mise à jour résout [Tigera Technical Advisory TTA-2018-001 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Configuration du DNS de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Correction d'un bogue pouvant entraîner l'exécution des pods Kubernetes DNS et CoreDNS après la création du cluster ou des opérations de mise à jour.</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.2.0</td>
<td>v1.1.5</td>
<td>Voir les [notes sur l'édition de containerd![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.1.5). Mise à jour de containerd pour corriger un interblocage pouvant [empêcher les pods de s'arrêter ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/issues/2744).</td>
</tr>
<tr>
<td>Serveur et client OpenVPN</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Mise à jour d'une image pour [CVE-2018-0732 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) et [CVE-2018-0737 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.12.2_1528, publié le 19 novembre 2018
{: #1122_1528}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.12.2_1528.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.12.2_1527">
<caption>Modifications depuis la version 1.12.2_1527</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour [CVE-2018-7755 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>


### Journal des modifications pour la version 1.12.2_1527, publié le 7 novembre 2018
{: #1122_1527}

Le tableau suivant présente les modifications incluses dans le correctif 1.12.2_1527.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.3_1533">
<caption>Modifications depuis la version 1.11.3_1533</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration de Calico</td>
<td>N/A</td>
<td>N/A</td>
<td>Les pods `calico-*` de Calico dans l'espace de nom `kube-system` définissent désormais les demandes de ressources d'UC et de mémoire pour tous les conteneurs.</td>
</tr>
<tr>
<td>Fournisseur DNS de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Kubernetes DNS (KubeDNS) reste le fournisseur DNS de cluster par défaut. Vous avez toutefois la possibilité de [remplacer le fournisseur DNS de cluster par CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>Fournisseur de métriques de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Le serveur Kubernetes Metrics Server remplace Kubernetes Heapster (déprécié depuis la version 1.8 de Kubernetes) comme fournisseur de métriques de cluster. Pour les éléments action, voir [l'action de préparation `metrics-server`](/docs/containers?topic=containers-cs_versions#metrics-server).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.4</td>
<td>1.2.0</td>
<td>Voir les [notes sur l'édition de containerd![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.2.0).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>N/A</td>
<td>1.2.2</td>
<td>Voir les [notes sur l'édition de CoreDNS![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coredns/coredns/releases/tag/v1.2.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.12.2</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.2).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de trois nouvelles politiques de sécurité de pod IBM et des rôles de cluster associés. Pour plus d'informations, voir [Description des ressources par défaut pour la gestion de cluster IBM](/docs/containers?topic=containers-psp#ibm_psp).</td>
</tr>
<tr>
<td>Tableau de bord Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.0</td>
<td>Voir les [notes sur l'édition du tableau de bord Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.0).<br><br>
Si vous accédez au tableau de bord via `kubectl proxy`, le bouton **SKIP** sur la page de connexion a été retiré. [Utilisez un **jeton** pour vous connecter](/docs/containers?topic=containers-app#cli_dashboard) à la place. De plus, vous pouvez désormais augmenter le nombre de pods du tableau de bord Kubernetes en exécutant la commande `kubectl -n kube-system scale deploy kubernetes-dashboard --replicas=3`.</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>Voir les [notes sur l'édition de Kubernetes DNS![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>N/A</td>
<td>v0.3.1</td>
<td>Voir les [notes sur l'édition de Kubernetes Metrics Server![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.1).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-118</td>
<td>v1.12.2-68</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.12. Autres modifications incluses :
<ul><li>Les pods d'équilibreur de charge (`ibm-cloud-provider-ip-*` dans l'espace de nom `ibm-system`) définissent désormais les demandes de ressources d'UC et de mémoire.</li>
<li>L'annotation `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` a été ajoutée pour spécifier le réseau local virtuel (VLAN) sur lequel est déployé le service d'équilibreur de charge. Pour voir les VLAN disponibles dans votre cluster, exécutez la commande `ibmcloud ks vlans --zone <zone>`.</li>
<li>Un nouvel [équilibreur de charge 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) est disponible en version bêta.</li></ul></td>
</tr>
<tr>
<td>Configuration du client OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Le client OpenVPN `vpn-* pod` dans l'espace de nom `kube-system` définit désormais les demandes de ressources d'UC et de mémoire.</td>
</tr>
</tbody>
</table>

## Déprécié : Journal des modifications - Version 1.11
{: #111_changelog}

Passez en revue le journal des modifications de la version 1.11.
{: shortdesc}

La version 1.11 de Kubernetes est dépréciée et n'est plus prise en charge à compter du 27 juin 2019 (date provisoire). [Consultez l'impact potentiel](/docs/containers?topic=containers-cs_versions#cs_versions) de chaque mise à jour de version Kubernetes, puis [mettez à jour vos clusters](/docs/containers?topic=containers-update#update) pour passer immédiatement à la version 1.12 ou ultérieure.
{: deprecated}

### Journal des modifications pour la version 1.11.10_1561, publié le 4 juin 2019
{: #11110_1561}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.10_1561 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.10_1559">
<caption>Modifications effectuées depuis la version 1.11.10_1559</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration à haute disponibilité de maître cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration afin de réduire les échecs de connectivité de réseau maître durant une mise à jour de maître. </td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Voir les [notes sur l'édition d'etcd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-10844 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) et [CVE-2019-5436 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-10844 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) et [CVE-2019-5436 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.11.10_1559, publié le 20 mai 2019
{: #11110_1559}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs 1.11.10_1559 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.10_1558">
<caption>Modifications effectuées depuis la version 1.11.10_1558</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau Ubuntu 16.04</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2018-12126 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) et [CVE-2018-12130 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Noyau Ubuntu 18.04</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2018-12126 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) et [CVE-2018-12130 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.11.10_1558, publié le 13 mai 2019
{: #11110_1558}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.10_1558 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.9_1556">
<caption>Modifications effectuées depuis la version 1.11.9_1556</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy du maître cluster HA</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Voir les [notes sur l'édition de HAProxy ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La mise à jour résout la vulnérabilité [CVE-2019-6706 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Image mise à jour pour la vulnérabilité [CVE-2019-1543 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.9-241</td>
<td>v1.11.10-270</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.11.10. </td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.9</td>
<td>v1.11.10</td>
<td>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.10). </td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration de règle d'audit du serveur d'API Kubernetes pour ne pas consigner l'URL en lecture seule `/openapi/v2*`. De plus, la configuration du gestionnaire de contrôleur Kubernetes a augmenté la durée de validité des certificats `kubelet` signés, de 1 à 3 ans. </td>
</tr>
<tr>
<td>Configuration du client OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Le client OpenVPN `vpn-*` dans l'espace de nom `kube-system` affecte désormais à `dnsPolicy` la valeur `Default` pour empêcher l'échec du pod lorsque le DNS de cluster est indisponible. </td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2016-7076 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) et [CVE-2019-11068 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.11.9_1556, publié le 29 avril 2019
{: #1119_1556}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.11.9_1556 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.9_1555">
<caption>Modifications effectuées depuis la version 1.11.9_1555</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour des packages Ubuntu installés.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>Voir les [notes sur l'édition de containerd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.1.7).</td>
</tr>
</tbody>
</table>


### Journal des modifications de noeud worker - Groupe de correctifs 1.11.9_1555, publié le 15 avril 2019
{: #1119_1555}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.11.9_1555.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.9_1544">
<caption>Modifications depuis la 1.11.9_1554</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour pour les packages Ubuntu installés, notamment `systemd` pour traiter les vulnérabilités [CVE-2019-3842 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.11.9_1554, publié le 8 avril 2019
{: #1119_1554}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.9_1554.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.8_1553">
<caption>Modifications depuis la version 1.11.8_1553</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.3/releases/#v336). La mise à jour résout la vulnérabilité [CVE-2019-9946 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Proxy du maître cluster HA</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Voir les [notes sur l'édition de HAProxy ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La mise à jour résout les vulnérabilités [CVE-2018-0732 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) et [CVE-2019-1559 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.8-219</td>
<td>v1.11.9-241</td>
<td>Mise à jour pour prendre en charge les éditions Kubernetes 1.11.9 et Calico 3.3.6.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.8</td>
<td>v1.11.9</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.9).</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>Voir les [notes sur l'édition de Kubernetes DNS![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2017-12447 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Noyau Ubuntu 16.04</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2019-9213 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Noyau Ubuntu 18.04</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2019-9213 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.11.8_1553, publié le 1er avril 2019
{: #1118_1553}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.11.8_1553.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.8_1552">
<caption>Modifications depuis la version 1.11.8_1552</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilisation des ressources de noeud worker</td>
<td>N/A</td>
<td>N/A</td>
<td>Augmentation des réserves de mémoire pour kubelet et containerd afin d'empêcher ces composants d'être à court de ressources. Pour plus d'informations, voir [Réserves de ressources de noeud worker](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Journal des modifications du maître - Groupe de correctifs 1.11.8_1552, publié le 26 mars 2019
{: #1118_1552}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs du maître 1.11.8_1552.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.8_1550">
<caption>Modifications depuis la version 1.11.8_1550</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>345</td>
<td>346</td>
<td>Image mise à jour pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>166</td>
<td>167</td>
<td>Résout les erreurs de type `context deadline exceeded` et `timeout` sporadiques lors de la gestion des secrets Kubernetes. De plus, résout les mises à jour apportées au service de gestion de clés susceptibles de laisser les secrets Kubernetes existants non chiffrés. La mise à jour comprend un correctif pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Equilibreur de charge et moniteur d'équilibrage de charge pour {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Image mise à jour pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.11.8_1550, publié le 20 mars 2019
{: #1118_1550}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.8_1550.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.8_1547">
<caption>Modifications depuis la version 1.11.8_1547</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration de proxy à haute disponibilité de maître cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration pour mieux traiter les échecs de connexion intermittente avec le maître cluster.</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Mise à jour des pilotes GPU à la version [418.43 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.nvidia.com/object/unix.html). La mise à jour comprend un correctif pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>344</td>
<td>345</td>
<td>Prise en charge des [noeuds finaux de service privé](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Noyau</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour traiter la vulnérabilité [CVE-2019-6133 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>136</td>
<td>166</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-16890 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) et [CVE-2019-3823 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-10779 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) et [CVE-2019-7663 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.11.8_1547, publié le 4 mars 2019
{: #1118_1547}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.8_1547.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.7_1546">
<caption>Modifications depuis la version 1.11.7_1546</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Mise à jour des images pour traiter la vulnérabilité [CVE-2019-6454 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.7-198</td>
<td>v1.11.8-219</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.11.8. Correction de problèmes de connectivité périodique des équilibreurs de charge qui définissaient le paramètre `externalTrafficPolicy` avec la valeur `local`. Mise à jour des événements d'équilibreur de charge pour utiliser les liens à jour de la documentation {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>342</td>
<td>344</td>
<td>Modification de l'image de base du système d'exploitation pour passer de Fedora à Alpine. Image mise à jour pour traiter la vulnérabilité [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>122</td>
<td>136</td>
<td>Augmentation du délai d'attente du client dans {{site.data.keyword.keymanagementservicefull_notm}}. Image mise à jour pour traiter la vulnérabilité [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.7</td>
<td>v1.11.8</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.8). La mise à jour résout les vulnérabilités [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) et [CVE-2019-1002100 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout du paramètre `ExperimentalCriticalPodAnnotation=true` à l'option `--feature-gates`. Ce paramètre permet de migrer les pods de l'annotation `scheduler.alpha.kubernetes.io/critical-pod` dépréciée pour [prendre en charge la priorité de pod de Kubernetes](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>N/A</td>
<td>N/A</td>
<td>Augmentation de la limite de mémoire de pod pour le serveur Kubernetes DNS de `170Mi` à `400Mi` afin de traiter plus de services de cluster.</td>
</tr>
<tr>
<td>Equilibreur de charge et moniteur d'équilibrage de charge pour {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Serveur et client OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2019-1559 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2019-6454 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.11.7_1546, publié le 27 février 2019
{: #1117_1546}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.11.7_1546.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.7_1546">
<caption>Modifications depuis la version 1.11.7_1546</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour traiter la vulnérabilité [CVE-2018-19407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.11.7_1544, publié le 15 février 2019
{: #1117_1544}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.11.7_1544.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.7_1543">
<caption>Modifications depuis la version 1.11.7_1543</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration de proxy à haute disponibilité de maître cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Remplacement de la valeur `spec.priorityClassName` de la configuration de pod par `system-node-critical` et définition de la valeur de `spec.priority` sur `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>Voir les [notes sur l'édition de containerd![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.1.6). La mise à jour résout la vulnérabilité [CVE-2019-5736 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configuration du `kubelet` de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Activation de l'indicateur feature gate `ExperimentalCriticalPodAnnotation` pour éviter l'expulsion critique de pod statique.</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.11.7_1543, publié le 5 février 2019
{: #1117_1543}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.7_1543.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.6_1541">
<caption>Modifications depuis la version 1.11.6_1541</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Voir les [notes sur l'édition d'etcd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coreos/etcd/releases/v3.3.11). Par ailleurs, les suites de chiffrement prises en charge sur etcd sont désormais limitées à un sous-ensemble avec un niveau de chiffrement élevé (128 bits ou plus).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Images mises à jour pour traiter les vulnérabilités [CVE-2019-3462 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) et [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.6-180</td>
<td>v1.11.7-198</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.11.7. Par ailleurs, `calicoctl` est mis à jour à la version 3.3.1. Correction de mises à jour de configuration inutiles vers les équilibreurs de charge de version 2.0 sur les modifications de statut de noeud worker.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>338</td>
<td>342</td>
<td>Le plug-in de stockage de fichiers est mis à jour comme suit :
<ul><li>Prise en charge du provisionnement dynamique avec [planification tenant compte de la topologie de volume](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Les erreurs de suppression de réservation de volume persistant (PVC) sont ignorées si le stockage est déjà supprimé.</li>
<li>Ajout d'une annotation de message d'échec pour les PVC ayant échoué.</li>
<li>Optimisation des paramètres d'élection du leader du contrôleur du service de mise à disposition de stockage et de période de resynchronisation et augmentation du délai de mise à disposition de 30 minutes à 1 heure.</li>
<li>Vérification des droits d'utilisateur avant de commencer la mise à disposition.</li></ul></td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>111</td>
<td>122</td>
<td>Ajout d'une logique de nouvelle tentative pour éviter les échecs temporaires lorsque les valeurs confidentielles (secrets) de Kubernetes sont gérées par {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.6</td>
<td>v1.11.7</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.7).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration de règle d'audit du serveur d'API Kubernetes pour inclure les métadonnées de journalisation pour les demandes `cluster-admin` et consigner le corps des demandes `create`, `update` et `patch` de charge de travail.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Mise à jour d'une image pour traiter les vulnérabilités [CVE-2018-0734 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) et [CVE-2018-5407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Par ailleurs, la configuration de pod est désormais obtenue à partir d'une valeur confidentielle (secret) au lieu d'une mappe de configuration (configmap).</td>
</tr>
<tr>
<td>Serveur OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Mise à jour d'une image pour traiter les vulnérabilités [CVE-2018-0734 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) et [CVE-2018-5407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Correctif de sécurité pour traiter la vulnérabilité [CVE-2018-16864 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.11.6_1541, publié le 28 janvier 2019
{: #1116_1541}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.11.6_1541.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.6_1540">
<caption>Modifications depuis la version 1.11.6_1540</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour pour les packages Ubuntu installés, notamment `apt` pour traiter les vulnérabilités [CVE-2019-3462 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) / [USN-3863-1 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.11.6_1540, publié le 21 janvier 2019
{: #1116_1540}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.6_1540.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.5_1539">
<caption>Modifications depuis la version 1.11.5_1539</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.5-152</td>
<td>v1.11.6-180</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.11.6.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.5</td>
<td>v1.11.6</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.6).</td>
</tr>
<tr>
<td>Image Kubernetes add-on resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Voir les [notes sur l'édition de Kubernetes pour l'image add-on resizer ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Tableau de bord Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>Voir les [notes sur l'édition sur le tableau de bord Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). La mise à jour résout la vulnérabilité [CVE-2018-18264 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).<br><br>Si vous accédez au tableau de bord via `kubectl proxy`, le bouton **SKIP** sur la page de connexion a été retiré. [Utilisez un **jeton** pour vous connecter](/docs/containers?topic=containers-app#cli_dashboard) à la place.</td>
</tr>
<tr>
<td>Programme d'installation de GPU</td>
<td>390.12</td>
<td>410.79</td>
<td>Mise à jour des pilotes GPU installés à la version 410.79.</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.11.5_1539, publié le 7 janvier 2019
{: #1115_1539}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.11.5_1539.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.5_1538">
<caption>Modifications depuis la version 1.11.5_1538</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour traiter les vulnérabilités [CVE-2017-5753, CVE-2018-18690 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.11.5_1538, publié le 17 décembre 2018
{: #1115_1538}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.11.5_1538.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.5_1537">
<caption>Modifications depuis la version 1.11.5_1537</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour des packages Ubuntu installés.</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.11.5_1537, publié le 5 décembre 2018
{: #1115_1537}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.5_1537.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.4_1536">
<caption>Modifications depuis la version 1.11.4_1536</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.4-142</td>
<td>v1.11.5-152</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.11.5.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.4</td>
<td>v1.11.5</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.5). La mise à jour résout la vulnérabilité [CVE-2018-1002105 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.11.4_1536, publié le 4 décembre 2018
{: #1114_1536}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.11.4_1536.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.4_1535">
<caption>Modifications depuis la version 1.11.4_1535</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilisation des ressources de noeud worker</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de groupes cgroups pour le kubelet et containerd pour empêcher ces composants d'être à court de ressources. Pour plus d'informations, voir [Réserves de ressources de noeud worker](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.11.4_1535, publié le 27 novembre 2018
{: #1114_1535}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.4_1535.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.3_1534">
<caption>Modifications depuis la version 1.11.3_1534</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.3/releases/#v331). La mise à jour résout [Tigera Technical Advisory TTA-2018-001 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.1.4</td>
<td>v1.1.5</td>
<td>Voir les [notes sur l'édition de containerd![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.1.5). Mise à jour de containerd pour corriger un interblocage pouvant [empêcher les pods de s'arrêter ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/issues/2744).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-127</td>
<td>v1.11.4-142</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.11.4.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.11.4</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.4).</td>
</tr>
<tr>
<td>Serveur et client OpenVPN</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Mise à jour d'une image pour [CVE-2018-0732 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) et [CVE-2018-0737 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.11.3_1534, publié le 19 novembre 2018
{: #1113_1534}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.11.3_1534.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.3_1533">
<caption>Modifications depuis la version 1.11.3_1533</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour [CVE-2018-7755 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>


### Journal des modifications pour la version 1.11.3_1533, publié le 7 novembre 2018
{: #1113_1533}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.3_1533.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.3_1531">
<caption>Modifications depuis la version 1.11.3_1531</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Mise à jour des maîtres cluster à haute disponibilité (HA)</td>
<td>N/A</td>
<td>N/A</td>
<td>Correction de la mise à jour des maîtres à haute disponibilité (HA) pour les clusters qui utilisent des webhooks d'admission, tels qu'`initializerconfigurations`, `mutatingwebhookconfigurations` ou `validatingwebhookconfigurations`. Vous pouvez utiliser ces webhooks avec des chartes Helm comme pour [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-100</td>
<td>v1.11.3-127</td>
<td>Ajout de l'annotation `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` pour spécifier le réseau local virtuel (VLAN) sur lequel est déployé le service d'équilibreur de charge. Pour voir les VLAN disponibles dans votre cluster, exécutez la commande `ibmcloud ks vlans --zone <zone>`.</td>
</tr>
<tr>
<td>Noyau avec puce TPM activée</td>
<td>N/A</td>
<td>N/A</td>
<td>Les noeuds worker bare metal avec des puces TPM pour la fonction de calcul sécurisé utilisent le noyau Ubuntu par défaut jusqu'à ce que la fonction de confiance (trust) soit activée. Si vous [activez la fonction de confiance](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) sur un cluster existant, vous devez [recharger](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) tous les noeuds worker bare metal existants avec des puces TPM. Pour vérifier si un noeud worker bare metal comporte une puce TPM, vérifiez la zone **Trustable** après avoir exécuté la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-types --zone`.</td>
</tr>
</tbody>
</table>

### Journal des modifications du maître - Groupe de correctifs 1.11.3_1531, publié le 1er novembre 2018
{: #1113_1531_ha-master}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs du maître 1.11.3_1531.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.3_1527">
<caption>Modifications depuis la version 1.11.3_1527</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Maître cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration du noeud du maître cluster pour accroître la haute disponibilité (HA). Désormais, les clusters ont trois répliques du maître Kubernetes conçues avec une configuration haute disponibilité (HA), chaque maître étant déployé sur des hôtes physiques distincts. Par ailleurs, si votre cluster se trouve dans une zone compatible avec plusieurs zones, les maîtres sont répartis sur les différentes zones.<br>Pour connaître les actions que vous devez effectuer, voir [Mise à jour des maîtres cluster pour la haute disponibilité](/docs/containers?topic=containers-cs_versions#ha-masters). Ces actions de préparation sont applicables dans les cas suivants :<ul>
<li>Si vous disposez d'un pare-feu ou de règles réseau Calico personnalisées.</li>
<li>Si vous utilisez les ports d'hôte `2040` ou `2041` sur vos noeuds worker.</li>
<li>Si vous avez utilisé l'adresse IP du noeud du maître cluster pour accéder au maître depuis le cluster.</li>
<li>Si vous disposez d'un processus automatique pour appeler l'API ou l'interface de ligne de commande Calico (`calicoctl`), par exemple pour créer des règles Calico.</li>
<li>Si vous utilisez des règles réseau Kubernetes ou Calico pour contrôler l'accès du trafic sortant du pod vers le maître.</li></ul></td>
</tr>
<tr>
<td>Proxy du maître cluster HA</td>
<td>N/A</td>
<td>1.8.12-alpine</td>
<td>Ajout d'un pod `ibm-master-proxy-*` pour l'équilibrage de charge côté client sur tous les noeuds worker, de sort que chaque client de noeud worker puisse acheminer les demandes à une réplique de maître HA disponible.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>Voir les [notes sur l'édition d'etcd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coreos/etcd/releases/v3.3.1).</td>
</tr>
<tr>
<td>Chiffrement des données dans etcd</td>
<td>N/A</td>
<td>N/A</td>
<td>Auparavant, les données etcd étaient stockées sur une instance de stockage de fichiers NFS d'un maître chiffrée au repos. Désormais, les données etcd sont stockées sur le disque local du maître et sauvegardées dans {{site.data.keyword.cos_full_notm}}. Les données sont chiffrées lors du transit vers {{site.data.keyword.cos_full_notm}} et au repos. Cependant, les données etcd sur le disque local du maître ne sont pas chiffrées. Si vous souhaitez que les données etcd locales de votre maître soient chiffrées, [activez {{site.data.keyword.keymanagementservicelong_notm}} dans votre cluster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.11.3_1531, publié le 26 octobre 2018
{: #1113_1531}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.11.3_1531.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.3_1525">
<caption>Modifications depuis la version 1.11.3_1525</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Traitement des interruptions du système d'exploitation</td>
<td>N/A</td>
<td>N/A</td>
<td>Remplacement du démon du système IRQ (Interrupt Request) avec un gestionnaire d'interruption plus performant.</td>
</tr>
</tbody>
</table>

### Journal des modifications du maître - Groupe de correctifs 1.11.3_1527, publié le 15 octobre 2018
{: #1113_1527}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs du maître 1.11.3_1527.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.3_1524">
<caption>Modifications depuis la version 1.11.3_1524</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration de Calico</td>
<td>N/A</td>
<td>N/A</td>
<td>La sonde de conteneur Readiness Probe `calico-node` a été corrigée pour mieux traiter les défaillances de noeuds.</td>
</tr>
<tr>
<td>Mise à jour de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Correction du problème de mise à jour des modules complémentaires de cluster lorsque le maître est mis à jour à partir d'une version non prise en charge.</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.11.3_1525, publié le 10 octobre 2018
{: #1113_1525}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.11.3_1525.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.3_1524">
<caption>Modifications depuis la version 1.11.3_1524</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Mise à jour des images de noeud worker avec la mise à jour du noyau pour [CVE-2018-14633, CVE-2018-17182 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Délai d'inactivité de session</td>
<td>N/A</td>
<td>N/A</td>
<td>Le délai d'inactivité de session a été fixé à 5 minutes pour des raisons de conformité.</td>
</tr>
</tbody>
</table>


### Journal des modifications pour la version 1.11.3_1524, publié le 2 octobre 2018
{: #1113_1524}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.3_1524.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.3_1521">
<caption>Modifications depuis la version 1.11.3_1521</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>1.1.3</td>
<td>1.1.4</td>
<td>Voir les [notes sur l'édition de containerd![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.1.4).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-91</td>
<td>v1.11.3-100</td>
<td>Mise à jour du lien vers la documentation dans les messages d'erreur de l'équilibreur de charge.</td>
</tr>
<tr>
<td>Classes de stockage de fichiers d'IBM</td>
<td>N/A</td>
<td>N/A</td>
<td>Suppression du paramètre `reclaimPolicy` en double dans les classes de stockage de fichiers d'IBM.<br><br>
Désormais, lorsque vous mettez à jour le noeud du maître cluster, la classe de stockage de fichiers d'IBM reste inchangée. Si vous souhaitez modifier la classe de stockage par défaut, exécutez la commande `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` et remplacez `<storageclass>` par le nom de la classe de stockage.</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.11.3_1521, publié le 20 septembre 2018
{: #1113_1521}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.3_1521.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.2_1516">
<caption>Modifications depuis la version 1.11.2_1516</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.2-71</td>
<td>v1.11.3-91</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.11.3.</td>
</tr>
<tr>
<td>Classes de stockage de fichiers d'IBM</td>
<td>N/A</td>
<td>N/A</td>
<td>Suppression de `mountOptions` dans les classes de stockage de fichiers d'IBM afin d'utiliser la valeur par défaut fournie par le noeud worker.<br><br>
Désormais, lorsque vous mettez à jour le noeud du maître cluster, la classe de stockage de fichiers d'IBM reste `ibmc-file-bronze`. Si vous souhaitez modifier la classe de stockage par défaut, exécutez la commande `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` et remplacez `<storageclass>` par le nom de la classe de stockage.</td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>N/A</td>
<td>N/A</td>
<td>Possibilité d'utiliser le fournisseur de service de gestion des clés (KMS) de Kubernetes dans le cluster pour prendre en charge {{site.data.keyword.keymanagementservicefull}}. Lorsque vous [activez {{site.data.keyword.keymanagementserviceshort}} dans votre cluster](/docs/containers?topic=containers-encryption#keyprotect), toutes vos valeurs confidentielles (secrets) Kubernetes sont chiffrées.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.2</td>
<td>v1.11.3</td>
<td>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.3).</td>
</tr>
<tr>
<td>Mise à l'échelle automatique de DNS avec Kubernetes</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>Voir les [notes sur l'édition de la fonction Kubernetes de mise à l'échelle automatique (autoscaler) de DNS ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0).</td>
</tr>
<tr>
<td>Rotation des journaux</td>
<td>N/A</td>
<td>N/A</td>
<td>Passage à l'utilisation de fichiers timer `systemd` au lieu de travaux `cronjobs` pour empêcher l'échec de la rotation des journaux (`logrotate`) sur les noeuds worker qui ne sont pas rechargés ou mis à niveau dans les 90 jours. **Remarque** : dans toutes les versions antérieures à cette version secondaire, le disque principal se remplit après l'échec d'un travail cron car il est impossible de faire tourner les journaux. Le travail cron échoue lorsque le noeud worker est actif pendant une durée de 90 jours sans être mis à jour ou rechargé. Si les journaux remplissent la totalité du disque principal, le noeud worker passe à l'état d'échec. Le noeud worker peut être corrigé en exécutant la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` ou la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`. </td>
</tr>
<tr>
<td>Expiration du mot de passe root</td>
<td>N/A</td>
<td>N/A</td>
<td>Les mots de passe root des noeuds worker expirent au bout de 90 jours pour des raisons de conformité. Si vos outils d'automatisation doivent se connecter au noeud worker en tant que root ou dépendent de travaux cron qui s'exécutent en tant que root, vous pouvez désactiver l'expiration du mot de passe en vous connectant au noeud worker et en exécutant la commande `chage -M -1 root`. **Remarque** : si vos exigences de conformité en matière de sécurité vous empêchent toute exécution en tant que root ou l'annulation de l'expiration du mot de passe, ne désactivez pas l'expiration. Vous pouvez à la place [mettre à jour](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) ou [recharger](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) vos noeuds worker au moins tous les 90 jours.</td>
</tr>
<tr>
<td>Composants d'exécution de noeud worker (`kubelet`, `kube-proxy`, `containerd`)</td>
<td>N/A</td>
<td>N/A</td>
<td>Suppression des dépendances des composants d'exécution sur le disque principal. Cette amélioration empêche l'échec des noeuds worker lorsque le disque principal est plein.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>Nettoyage régulier des unités de montage transitoires pour leur éviter de devenir illimitées. Cette action corrige l'erreur [Kubernetes 57345 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.11.2_1516, publié le 4 septembre 2018
{: #1112_1516}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.2_1516.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.2_1514">
<caption>Modifications depuis la version 1.11.2_1514</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.2/releases/#v321).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.2</td>
<td>1.1.3</td>
<td>Voir les [notes sur l'édition de `containerd` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.1.3).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.2-60</td>
<td>v1.11.2-71</td>
<td>La configuration du fournisseur de cloud a changé pour mieux gérer les mises à jour des services d'équilibreur de charge avec l'élément `externalTrafficPolicy` défini sur `local`.</td>
</tr>
<tr>
<td>Configuration du plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>N/A</td>
<td>N/A</td>
<td>La version NFS par défaut a été supprimée des options de montage dans les classes de stockage de fichiers fournies par IBM. Désormais, le système d'exploitation de l'hôte négocie la version NFS avec le serveur NFS de l'infrastructure IBM Cloud (SoftLayer). Pour définir manuellement une version NFS spécifique ou modifier la version NFS de votre volume persistant qui a été négociée par le système d'exploitation de l'hôte, voir [Modification de la version NFS par défaut](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.11.2_1514, publié le 23 août 2018
{: #1112_1514}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.11.2_1514.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.11.2_1513">
<caption>Modifications depuis la version 1.11.2_1513</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Mise à jour de `systemd` pour corriger la fuite liée à `cgroup`.</td>
</tr>
<tr>
<td>Noyau</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Mise à jour des images de noeud worker avec la mise à jour du noyau pour [CVE-2018-3620, CVE-2018-3646 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.11.2_1513, publié le 14 août 2018
{: #1112_1513}

Le tableau suivant présente les modifications incluses dans le correctif 1.11.2_1513.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.5_1518">
<caption>Modifications depuis la version 1.10.5_1518</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>N/A</td>
<td>1.1.2</td>
<td>`containerd` est le nouvel environnement d'exécution de conteneur de Kubernetes qui remplace Docker. Voir les [notes sur l'édition de `containerd` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/containerd/containerd/releases/tag/v1.1.2). Pour connaître les actions que vous devez effectuer, voir [Mise à jour vers l'environnement d'exécution de conteneur `containerd`](/docs/containers?topic=containers-cs_versions#containerd).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>`containerd` est le nouvel environnement d'exécution de conteneur de Kubernetes qui remplace Docker afin d'améliorer les performances. Pour connaître les actions que vous devez effectuer, voir [Mise à jour vers l'environnement d'exécution de conteneur `containerd`](/docs/containers?topic=containers-cs_versions#containerd).</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.14</td>
<td>v3.2.18</td>
<td>Voir les [notes sur l'édition d'etcd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coreos/etcd/releases/v3.2.18).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.5-118</td>
<td>v1.11.2-60</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.11. En outre, les pods d'équilibreur de charge utilisent désormais la nouvelle classe de priorité de pod `ibm-app-cluster-critical`.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>334</td>
<td>338</td>
<td>Mise à jour d'`incubator` à la version 1.8. Le stockage de fichiers est mis à disposition dans la zone spécifique que vous sélectionnez. Vous ne pouvez pas mettre à jour des libellés d'une instance de volume persistant (statique) existante, à moins d'utiliser un cluster à zones multiples et de nécessiter [l'ajout de libellés de région et de zone](/docs/containers?topic=containers-kube_concepts#storage_multizone).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.11.2</td>
<td>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.2).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration OpenID Connect pour le serveur d'API Kubernetes du cluster pour prendre en charge les groupes d'accès Identity Access and Management (IAM) d'{{site.data.keyword.Bluemix_notm}}. Ajout de `Priority` dans l'option `--enable-admission-plugins` pour le serveur d'API Kubernetes du cluster et configuration du cluster pour prendre en charge la priorité des pods. Pour plus d'informations, voir :
<ul><li>[Groupes d'accès {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#rbac)</li>
<li>[Configuration de la priorité de pod](/docs/containers?topic=containers-pod_priority#pod_priority)</li></ul></td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.2</td>
<td>v.1.5.4</td>
<td>Augmentation des limites de ressources pour le conteneur `heapster-nanny`. Voir les [notes sur l'édition de Kubernetes Heapster ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/heapster/releases/tag/v1.5.4).</td>
</tr>
<tr>
<td>Configuration de consignation</td>
<td>N/A</td>
<td>N/A</td>
<td>Le répertoire de consignation du conteneur est désormais `/var/log/pods/` au lieu de `/var/lib/docker/containers/`.</td>
</tr>
</tbody>
</table>

<br />


## Archive
{: #changelog_archive}

Versions de Kubernetes non prises en charge :
*  [Version 1.10](#110_changelog)
*  [Version 1.9](#19_changelog)
*  [Version 1.8](#18_changelog)
*  [Version 1.7](#17_changelog)

### Journal des modifications pour la version 1.10 (non prise en charge depuis le 16 mai 2019)
{: #110_changelog}

Passez en revue le journal des modifications version 1.10.
{: shortdesc}

*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.13_1558, publié le 13 mai 2019](#11013_1558)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.13_1557, publié le 29 avril 2019](#11013_1557)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.13_1556, publié le 15 avril 2019](#11013_1556)
*   [Journal des modifications pour la version 1.10.13_1555, publié le 8 avril 2019](#11013_1555)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.13_1554, publié le 1er avril 2019](#11013_1554)
*   [Journal des modifications du maître - Groupe de correctifs 1.10.13_1553, publié le 26 mars 2019](#11118_1553)
*   [Journal des modifications pour la version 1.10.13_1551, publié le 20 mars 2019](#11013_1551)
*   [Journal des modifications pour la version 1.10.13_1548, publié le 4 mars 2019](#11013_1548)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.12_1546, publié le 27 février 2019](#11012_1546)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.12_1544, publié le 15 février 2019](#11012_1544)
*   [Journal des modifications pour la version 1.10.12_1543, publié le 5 février 2019](#11012_1543)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.12_1541, publié le 28 janvier 2019](#11012_1541)
*   [Journal des modifications pour la version 1.10.12_1540, publié le 21 janvier 2019](#11012_1540)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.11_1538, publié le 7 janvier 2019](#11011_1538)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.11_1537, publié le 17 décembre 2018](#11011_1537)
*   [Journal des modifications pour la version 1.10.11_1536, publié le 4 décembre 2018](#11011_1536)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.8_1532, publié le 27 novembre 2018](#1108_1532)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.8_1531, publié le 19 novembre 2018](#1108_1531)
*   [Journal des modifications pour la version 1.10.8_1530, publié le 7 novembre 2018](#1108_1530_ha-master)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.8_1528, publié le 26 octobre 2018](#1108_1528)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.8_1525, publié le 10 octobre 2018](#1108_1525)
*   [Journal des modifications pour la version 1.10.8_1524, publié le 2 octobre 2018](#1108_1524)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.7_1521, publié le 20 septembre 2018](#1107_1521)
*   [Journal des modifications pour la version 1.10.7_1520, publié le 4 septembre 2018](#1107_1520)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.5_1519, publié le 23 août 2018](#1105_1519)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.5_1518, publié le 13 août 2018](#1105_1518)
*   [Journal des modifications pour la version 1.10.5_1517, publié le 27 juillet 2018](#1105_1517)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.3_1514, publié le 3 juillet 2018](#1103_1514)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.3_1513, publié le 21 juin 2018](#1103_1513)
*   [Journal des modifications pour la version 1.10.3_1512, publié le 12 juin 2018](#1103_1512)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.1_1510, publié le 18 mai 2018](#1101_1510)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.10.1_1509, publié le 16 mai 2018](#1101_1509)
*   [Journal des modifications pour la version 10. 1_1508, publié le 1er mai 2018](#1101_1508)

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.13_1558, publié le 13 mai 2019
{: #11013_1558}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.13_1558 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.13_1557">
<caption>Modifications effectuées depuis la version 1.10.13_1557</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy du maître cluster HA</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Voir les [notes sur l'édition de HAProxy ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La mise à jour résout la vulnérabilité [CVE-2019-6706 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.13_1557, publié le 29 avril 2019
{: #11013_1557}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.13_1557 :
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.13_1556">
<caption>Modifications effectuées depuis la version 1.10.13_1556</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour des packages Ubuntu installés.</td>
</tr>
</tbody>
</table>


#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.13_1556, publié le 15 avril 2019
{: #11013_1556}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.13_1556.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.13_1555">
<caption>Modifications depuis la 1.10.13_1555</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour pour les packages Ubuntu installés, notamment `systemd` pour traiter les vulnérabilités [CVE-2019-3842 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.10.13_1555, publié le 8 avril 2019
{: #11013_1555}

Le tableau suivant présente les modifications incluses dans le correctif 1.10.13_1555.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.13_1554">
<caption>Modifications depuis la version 1.10.13_1554</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy du maître cluster HA</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Voir les [notes sur l'édition de HAProxy ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.haproxy.org/download/1.9/src/CHANGELOG). La mise à jour résout les vulnérabilités [CVE-2018-0732 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) et [CVE-2019-1559 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>Voir les [notes sur l'édition de Kubernetes DNS![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2017-12447 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Noyau Ubuntu 16.04</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2019-9213 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Noyau Ubuntu 18.04</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Images de noeud worker mises à jour avec une mise à jour du noyau pour [CVE-2019-9213 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.13_1554, publié le 1er avril 2019
{: #11013_1554}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.13_1554.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.13_1553">
<caption>Modifications depuis la version 1.10.13_1553</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilisation des ressources de noeud worker</td>
<td>N/A</td>
<td>N/A</td>
<td>Augmentation des réserves de mémoire pour kubelet et containerd afin d'empêcher ces composants d'être à court de ressources. Pour plus d'informations, voir [Réserves de ressources de noeud worker](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>


#### Journal des modifications du maître - Groupe de correctifs 1.10.13_1553, publié le 26 mars 2019
{: #11118_1553}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs du maître 1.10.13_1553.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.13_1551">
<caption>Modifications depuis la version 1.10.13_1551</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>345</td>
<td>346</td>
<td>Image mise à jour pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>166</td>
<td>167</td>
<td>Résout les erreurs de type `context deadline exceeded` et `timeout` sporadiques lors de la gestion des secrets Kubernetes. De plus, résout les mises à jour apportées au service de gestion de clés susceptibles de laisser les secrets Kubernetes existants non chiffrés. La mise à jour comprend un correctif pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Equilibreur de charge et moniteur d'équilibrage de charge pour {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Image mise à jour pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.10.13_1551, publié le 20 mars 2019
{: #11013_1551}

Le tableau suivant présente les modifications incluses dans le correctif 1.10.13_1551.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.13_1548">
<caption>Modifications depuis la version 1.10.13_1548</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration de proxy à haute disponibilité de maître cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration pour mieux traiter les échecs de connexion intermittente avec le maître cluster.</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Mise à jour des pilotes GPU à la version [418.43 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.nvidia.com/object/unix.html). La mise à jour comprend un correctif pour la vulnérabilité [CVE-2019-9741 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>344</td>
<td>345</td>
<td>Prise en charge des [noeuds finaux de service privé](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Noyau</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour traiter la vulnérabilité [CVE-2019-6133 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>136</td>
<td>166</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-16890 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) et [CVE-2019-3823 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Image mise à jour pour traiter les vulnérabilités [CVE-2018-10779 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) et [CVE-2019-7663 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.10.13_1548, publié le 4 mars 2019
{: #11013_1548}

Le tableau suivant présente les modifications incluses dans le correctif 1.10.13_1548.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.12_1546">
<caption>Modifications depuis la version 1.10.12_1546</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Mise à jour des images pour traiter la vulnérabilité [CVE-2019-6454 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.12-252</td>
<td>v1.10.13-288</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.10.13. Correction de problèmes de connectivité périodique des équilibreurs de charge qui définissaient le paramètre `externalTrafficPolicy` avec la valeur `local`. Mise à jour des événements d'équilibreur de charge pour utiliser les liens à jour de la documentation {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>342</td>
<td>344</td>
<td>Modification de l'image de base du système d'exploitation pour passer de Fedora à Alpine. Image mise à jour pour traiter la vulnérabilité [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>122</td>
<td>136</td>
<td>Augmentation du délai d'attente du client dans {{site.data.keyword.keymanagementservicefull_notm}}. Image mise à jour pour traiter la vulnérabilité [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.12</td>
<td>v1.10.13</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.13).</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>N/A</td>
<td>N/A</td>
<td>Augmentation de la limite de mémoire de pod pour le serveur Kubernetes DNS de `170Mi` à `400Mi` afin de traiter plus de services de cluster.</td>
</tr>
<tr>
<td>Equilibreur de charge et moniteur d'équilibrage de charge pour {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Serveur et client OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2019-1559 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agent de calcul sécurisé</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Image mise à jour pour traiter la vulnérabilité [CVE-2019-6454 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.12_1546, publié le 27 février 2019
{: #11012_1546}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.12_1546.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.12_1544">
<caption>Modifications depuis la version 1.10.12_1544</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour traiter la vulnérabilité [CVE-2018-19407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.12_1544, publié le 15 février 2019
{: #11012_1544}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.12_1544.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.12_1543">
<caption>Modifications depuis la version 1.10.12_1543</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>18.06.1-ce</td>
<td>18.06.2-ce</td>
<td>Voir les [notes sur l'édition de Docker Community Edition ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/docker/docker-ce/releases/tag/v18.06.2-ce). La mise à jour résout la vulnérabilité [CVE-2019-5736 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configuration du `kubelet` de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Activation de l'indicateur feature gate `ExperimentalCriticalPodAnnotation` pour éviter l'expulsion critique de pod statique.</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.10.12_1543, publié le 5 février 2019
{: #11012_1543}

Le tableau suivant présente les modifications incluses dans le correctif 1.10.12_1543.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.12_1541">
<caption>Modifications depuis la version 1.10.12_1541</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Voir les [notes sur l'édition d'etcd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coreos/etcd/releases/v3.3.11). Par ailleurs, les suites de chiffrement prises en charge sur etcd sont désormais limitées à un sous-ensemble avec un niveau de chiffrement élevé (128 bits ou plus).</td>
</tr>
<tr>
<td>Plug-in et programme d'installation d'unité GPU</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Images mises à jour pour traiter les vulnérabilités [CVE-2019-3462 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) et [CVE-2019-6486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>338</td>
<td>342</td>
<td>Le plug-in de stockage de fichiers est mis à jour comme suit :
<ul><li>Prise en charge du provisionnement dynamique avec [planification tenant compte de la topologie de volume](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Les erreurs de suppression de réservation de volume persistant (PVC) sont ignorées si le stockage est déjà supprimé.</li>
<li>Ajout d'une annotation de message d'échec pour les PVC ayant échoué.</li>
<li>Optimisation des paramètres d'élection du leader du contrôleur du service de mise à disposition de stockage et de période de resynchronisation et augmentation du délai de mise à disposition de 30 minutes à 1 heure.</li>
<li>Vérification des droits d'utilisateur avant de commencer la mise à disposition.</li></ul></td>
</tr>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>111</td>
<td>122</td>
<td>Ajout d'une logique de nouvelle tentative pour éviter les échecs temporaires lorsque les valeurs confidentielles (secrets) de Kubernetes sont gérées par {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration de règle d'audit du serveur d'API Kubernetes pour inclure les métadonnées de journalisation pour les demandes `cluster-admin` et consigner le corps des demandes `create`, `update` et `patch` de charge de travail.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Mise à jour d'une image pour traiter les vulnérabilités [CVE-2018-0734 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) et [CVE-2018-5407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Par ailleurs, la configuration de pod est désormais obtenue à partir d'une valeur confidentielle (secret) au lieu d'une mappe de configuration (configmap).</td>
</tr>
<tr>
<td>Serveur OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Mise à jour d'une image pour traiter les vulnérabilités [CVE-2018-0734 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) et [CVE-2018-5407 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Correctif de sécurité pour traiter la vulnérabilité [CVE-2018-16864 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.12_1541, publié le 28 janvier 2019
{: #11012_1541}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.12_1541.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.12_1540">
<caption>Modifications depuis la version 1.10.12_1540</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour pour les packages Ubuntu installés, notamment `apt` pour traiter les vulnérabilités [CVE-2019-3462 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) et [USN-3863-1 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.10.12_1540, publié le 21 janvier 2019
{: #11012_1540}

Le tableau suivant présente les modifications incluses dans le correctif 1.10.12_1540.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.11_1538">
<caption>Modifications depuis la version 1.10.11_1538</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.11-219</td>
<td>v1.10.12-252</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.10.12.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.11</td>
<td>v1.10.12</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.12).</td>
</tr>
<tr>
<td>Image Kubernetes add-on resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Voir les [notes sur l'édition de Kubernetes pour l'image add-on resizer ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Tableau de bord Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>Voir les [notes sur l'édition sur le tableau de bord Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). La mise à jour résout la vulnérabilité [CVE-2018-18264 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).<br><br>Si vous accédez au tableau de bord via `kubectl proxy`, le bouton **SKIP** sur la page de connexion a été retiré. [Utilisez un **jeton** pour vous connecter](/docs/containers?topic=containers-app#cli_dashboard) à la place.</td>
</tr>
<tr>
<td>Programme d'installation de GPU</td>
<td>390.12</td>
<td>410.79</td>
<td>Mise à jour des pilotes GPU installés à la version 410.79.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.11_1538, publié le 7 janvier 2019
{: #11011_1538}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.11_1538.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.11_1537">
<caption>Modifications depuis la version 1.10.11_1537</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour traiter les vulnérabilités [CVE-2017-5753, CVE-2018-18690 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.11_1537, publié le 17 décembre 2018
{: #11011_1537}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.11_1537.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.11_1536">
<caption>Modifications depuis la version 1.10.11_1536</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour des packages Ubuntu installés.</td>
</tr>
</tbody>
</table>


#### Journal des modifications pour la version 1.10.11_1536, publié le 4 décembre 2018
{: #11011_1536}

Le tableau suivant présente les modifications incluses dans le correctif 1.10.11_1536.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.8_1532">
<caption>Modifications depuis la version 1.10.8_1532</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.3/releases/#v331). La mise à jour résout [Tigera Technical Advisory TTA-2018-001 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.8-197</td>
<td>v1.10.11-219</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.10.11.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.8</td>
<td>v1.10.11</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.11). La mise à jour résout la vulnérabilité [CVE-2018-1002105 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
<tr>
<td>Serveur et client OpenVPN</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Mise à jour d'une image pour [CVE-2018-0732 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) et [CVE-2018-0737 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
<tr>
<td>Utilisation des ressources de noeud worker</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de groupes cgroups pour le kubelet et docker pour empêcher ces composants d'être à court de ressources. Pour plus d'informations, voir [Réserves de ressources de noeud worker](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.8_1532, publié le 27 novembre 2018
{: #1108_1532}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.8_1532.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.8_1531">
<caption>Modifications depuis la version 1.10.8_1531</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>Voir les [notes sur l'édition de Docker ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.docker.com/engine/release-notes/#18061-ce).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.8_1531, publié le 19 novembre 2018
{: #1108_1531}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.8_1531.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.8_1530">
<caption>Modifications depuis la version 1.10.8_1530</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour [CVE-2018-7755 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.10.8_1530, publié le 7 novembre 2018
{: #1108_1530_ha-master}

Le tableau suivant présente les modifications incluses dans le correctif 1.10.8_1530.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.8_1528">
<caption>Modifications depuis la version 1.10.8_1528</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Maître cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Mise à jour de la configuration du noeud du maître cluster pour accroître la haute disponibilité (HA). Désormais, les clusters ont trois répliques du maître Kubernetes conçues avec une configuration haute disponibilité (HA), chaque maître étant déployé sur des hôtes physiques distincts. Par ailleurs, si votre cluster se trouve dans une zone compatible avec plusieurs zones, les maîtres sont répartis sur les différentes zones.<br>Pour connaître les actions que vous devez effectuer, voir [Mise à jour des maîtres cluster pour la haute disponibilité](/docs/containers?topic=containers-cs_versions#ha-masters). Ces actions de préparation sont applicables dans les cas suivants :<ul>
<li>Si vous disposez d'un pare-feu ou de règles réseau Calico personnalisées.</li>
<li>Si vous utilisez les ports d'hôte `2040` ou `2041` sur vos noeuds worker.</li>
<li>Si vous avez utilisé l'adresse IP du noeud du maître cluster pour accéder au maître depuis le cluster.</li>
<li>Si vous disposez d'un processus automatique pour appeler l'API ou l'interface de ligne de commande Calico (`calicoctl`), par exemple pour créer des règles Calico.</li>
<li>Si vous utilisez des règles réseau Kubernetes ou Calico pour contrôler l'accès du trafic sortant du pod vers le maître.</li></ul></td>
</tr>
<tr>
<td>Proxy du maître cluster HA</td>
<td>N/A</td>
<td>1.8.12-alpine</td>
<td>Ajout d'un pod `ibm-master-proxy-*` pour l'équilibrage de charge côté client sur tous les noeuds worker, de sort que chaque client de noeud worker puisse acheminer les demandes à une réplique de maître HA disponible.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>Voir les [notes sur l'édition d'etcd ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/coreos/etcd/releases/v3.3.1).</td>
</tr>
<tr>
<td>Chiffrement des données dans etcd</td>
<td>N/A</td>
<td>N/A</td>
<td>Auparavant, les données etcd étaient stockées sur une instance de stockage de fichiers NFS d'un maître chiffrée au repos. Désormais, les données etcd sont stockées sur le disque local du maître et sauvegardées dans {{site.data.keyword.cos_full_notm}}. Les données sont chiffrées lors du transit vers {{site.data.keyword.cos_full_notm}} et au repos. Cependant, les données etcd sur le disque local du maître ne sont pas chiffrées. Si vous souhaitez que les données etcd locales de votre maître soient chiffrées, [activez {{site.data.keyword.keymanagementservicelong_notm}} dans votre cluster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.8-172</td>
<td>v1.10.8-197</td>
<td>Ajout de l'annotation `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` pour spécifier le réseau local virtuel (VLAN) sur lequel est déployé le service d'équilibreur de charge. Pour voir les VLAN disponibles dans votre cluster, exécutez la commande `ibmcloud ks vlans --zone <zone>`.</td>
</tr>
<tr>
<td>Noyau avec puce TPM activée</td>
<td>N/A</td>
<td>N/A</td>
<td>Les noeuds worker bare metal avec des puces TPM pour la fonction de calcul sécurisé utilisent le noyau Ubuntu par défaut jusqu'à ce que la fonction de confiance (trust) soit activée. Si vous [activez la fonction de confiance](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) sur un cluster existant, vous devez [recharger](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) tous les noeuds worker bare metal existants avec des puces TPM. Pour vérifier si un noeud worker bare metal comporte une puce TPM, vérifiez la zone **Trustable** après avoir exécuté la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-types --zone`.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.8_1528, publié le 26 octobre 2018
{: #1108_1528}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.8_1528.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.8_1527">
<caption>Modifications depuis la version 1.10.8_1527</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Traitement des interruptions du système d'exploitation</td>
<td>N/A</td>
<td>N/A</td>
<td>Remplacement du démon du système IRQ (Interrupt Request) avec un gestionnaire d'interruption plus performant.</td>
</tr>
</tbody>
</table>

#### Journal des modifications du maître - Groupe de correctifs 1.10.8_1527, publié le 15 octobre 2018
{: #1108_1527}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs du maître 1.10.8_1527.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.8_1524">
<caption>Modifications depuis la version 1.10.8_1524</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration de Calico</td>
<td>N/A</td>
<td>N/A</td>
<td>La sonde de conteneur Readiness Probe `calico-node` a été corrigée pour mieux traiter les défaillances de noeuds.</td>
</tr>
<tr>
<td>Mise à jour de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Correction du problème de mise à jour des modules complémentaires de cluster lorsque le maître est mis à jour à partir d'une version non prise en charge.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.8_1525, publié le 10 octobre 2018
{: #1108_1525}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.8_1525.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.8_1524">
<caption>Modifications depuis la version 1.10.8_1524</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Mise à jour des images de noeud worker avec la mise à jour du noyau pour [CVE-2018-14633, CVE-2018-17182 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Délai d'inactivité de session</td>
<td>N/A</td>
<td>N/A</td>
<td>Le délai d'inactivité de session a été fixé à 5 minutes pour des raisons de conformité.</td>
</tr>
</tbody>
</table>


#### Journal des modifications pour la version 1.10.8_1524, publié le 2 octobre 2018
{: #1108_1524}

Le tableau suivant présente les modifications incluses dans le correctif 1.10.8_1524.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.7_1520">
<caption>Modifications depuis la version 1.10.7_1520</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Fournisseur de service de gestion des clés (KMS)</td>
<td>N/A</td>
<td>N/A</td>
<td>Possibilité d'utiliser le fournisseur de service de gestion des clés (KMS) de Kubernetes dans le cluster pour prendre en charge {{site.data.keyword.keymanagementservicefull}}. Lorsque vous [activez {{site.data.keyword.keymanagementserviceshort}} dans votre cluster](/docs/containers?topic=containers-encryption#keyprotect), toutes vos valeurs confidentielles (secrets) Kubernetes sont chiffrées.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.7</td>
<td>v1.10.8</td>
<td>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.8).</td>
</tr>
<tr>
<td>Mise à l'échelle automatique de DNS avec Kubernetes</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>Voir les [notes sur l'édition de la fonction Kubernetes de mise à l'échelle automatique (autoscaler) de DNS ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.7-146</td>
<td>v1.10.8-172</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.10.8. Avec en plus la mise à jour du lien vers la documentation dans les messages d'erreur de l'équilibreur de charge.</td>
</tr>
<tr>
<td>Classes de stockage de fichiers d'IBM</td>
<td>N/A</td>
<td>N/A</td>
<td>Suppression de `mountOptions` dans les classes de stockage de fichiers d'IBM afin d'utiliser la valeur par défaut fournie par le noeud worker. Suppression du paramètre `reclaimPolicy` en double dans les classes de stockage de fichiers d'IBM.<br><br>
Désormais, lorsque vous mettez à jour le noeud du maître cluster, la classe de stockage de fichiers d'IBM reste inchangée. Si vous souhaitez modifier la classe de stockage par défaut, exécutez la commande `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` et remplacez `<storageclass>` par le nom de la classe de stockage.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.7_1521, publié le 20 septembre 2018
{: #1107_1521}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.7_1521.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.7_1520">
<caption>Modifications depuis la version 1.10.7_1520</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Rotation des journaux</td>
<td>N/A</td>
<td>N/A</td>
<td>Passage à l'utilisation de fichiers timer `systemd` au lieu de travaux `cronjobs` pour empêcher l'échec de la rotation des journaux (`logrotate`) sur les noeuds worker qui ne sont pas rechargés ou mis à niveau dans les 90 jours. **Remarque** : dans toutes les versions antérieures à cette version secondaire, le disque principal se remplit après l'échec d'un travail cron car il est impossible de faire tourner les journaux. Le travail cron échoue lorsque le noeud worker est actif pendant une durée de 90 jours sans être mis à jour ou rechargé. Si les journaux remplissent la totalité du disque principal, le noeud worker passe à l'état d'échec. Le noeud worker peut être corrigé en exécutant la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` ou la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`. </td>
</tr>
<tr>
<td>Composants d'exécution de noeud worker (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/A</td>
<td>N/A</td>
<td>Suppression des dépendances des composants d'exécution sur le disque principal. Cette amélioration empêche l'échec des noeuds worker lorsque le disque principal est plein.</td>
</tr>
<tr>
<td>Expiration du mot de passe root</td>
<td>N/A</td>
<td>N/A</td>
<td>Les mots de passe root des noeuds worker expirent au bout de 90 jours pour des raisons de conformité. Si vos outils d'automatisation doivent se connecter au noeud worker en tant que root ou dépendent de travaux cron qui s'exécutent en tant que root, vous pouvez désactiver l'expiration du mot de passe en vous connectant au noeud worker et en exécutant la commande `chage -M -1 root`. **Remarque** : si vos exigences de conformité en matière de sécurité vous empêchent toute exécution en tant que root ou l'annulation de l'expiration du mot de passe, ne désactivez pas l'expiration. Vous pouvez à la place [mettre à jour](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) ou [recharger](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) vos noeuds worker au moins tous les 90 jours.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>Nettoyage régulier des unités de montage transitoires pour leur éviter de devenir illimitées. Cette action corrige l'erreur [Kubernetes 57345 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>Désactivation du pont Docker par défaut pour que la plage d'adresses IP `172.17.0.0/16` soit désormais utilisée pour les routes privées. Si vos conteneurs Docker doivent être construits en exécutant des commandes `docker` directement sur l'hôte ou en utilisant un pod qui monte le socket Docker, choisissez l'une des options suivantes.<ul><li>Pour assurer la connectivité avec le réseau externe lorsque vous construisez le conteneur, exécutez la commande `docker build . --network host`.</li>
<li>Pour créer de manière explicite un réseau à utiliser lorsque vous construisez le conteneur, exécutez la commande `docker network create`, puis utilisez ce réseau.</li></ul>
**Remarque** : vous avez des dépendances sur le socket Docker ou directement sur Docker ? [Effectuez une mise à jour pour passer à l'environnement d'exécution de conteneur `containerd` au lieu de `docker`](/docs/containers?topic=containers-cs_versions#containerd) pour que vos clusters soient prêts à exécuter Kubernetes version 1.11 ou ultérieure.</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.10.7_1520, publié le 4 septembre 2018
{: #1107_1520}

Le tableau suivant présente les modifications incluses dans le correctif 1.10.7_1520.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.5_1519">
<caption>Modifications depuis la version 1.10.5_1519</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.2/releases/#v321).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.5-118</td>
<td>v1.10.7-146</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.10.7. En outre, la configuration du fournisseur de cloud a changé pour mieux gérer les mises à jour des services d'équilibreur de charge avec l'élément `externalTrafficPolicy` défini sur `local`.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>334</td>
<td>338</td>
<td>Mise à jour d'incubateur à la version 1.8. Le stockage de fichiers est mis à disposition dans la zone spécifique que vous sélectionnez. Vous ne pouvez pas mettre à jour des libellés d'une instance de volume persistant (statique) existante, à moins d'utiliser un cluster à zones multiples et de nécessiter l'ajout de libellés de région et de zone.<br><br> La version NFS par défaut a été supprimée des options de montage dans les classes de stockage de fichiers fournies par IBM. Désormais, le système d'exploitation de l'hôte négocie la version NFS avec le serveur NFS de l'infrastructure IBM Cloud (SoftLayer). Pour définir manuellement une version NFS spécifique ou modifier la version NFS de votre volume persistant qui a été négociée par le système d'exploitation de l'hôte, voir [Modification de la version NFS par défaut](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.10.7</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.7).</td>
</tr>
<tr>
<td>Configuration de Kubernetes Heapster</td>
<td>N/A</td>
<td>N/A</td>
<td>Augmentation des limites de ressources pour le conteneur `heapster-nanny`.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.5_1519, publié le 23 août 2018
{: #1105_1519}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.5_1519.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.5_1518">
<caption>Modifications depuis la version 1.10.5_1518</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Mise à jour de `systemd` pour corriger la fuite liée à `cgroup`.</td>
</tr>
<tr>
<td>Noyau</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Mise à jour des images de noeud worker avec la mise à jour du noyau pour [CVE-2018-3620, CVE-2018-3646 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>


#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.5_1518, publié le 13 août 2018
{: #1105_1518}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.5_1518.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.5_1517">
<caption>Modifications depuis la version 1.10.5_1517</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour des packages Ubuntu installés.</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.10.5_1517, publié le 27 juillet 2018
{: #1105_1517}

Le tableau suivant présente les modifications incluses dans le correctif 1.10.5_1517.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.3_1514">
<caption>Modifications depuis la version 1.10.3_1514</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.1</td>
<td>v3.1.3</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.1/releases/#v313).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.3-85</td>
<td>v1.10.5-118</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.10.5. En outre, les événements `create failure` du service LoadBalancer incluent désormais toutes les erreurs des sous-réseaux portables.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>320</td>
<td>334</td>
<td>Le délai de création d'un volume persistant a augmenté pour passer de 15 à 30 minutes. Le type de facturation par défaut a été changé pour passer à une facturation à l'heure (`hourly`). Des options de montage ont été ajoutées aux classes de stockage prédéfinies. Dans l'instance de stockage de fichiers NFS dans votre compte d'infrastructure IBM Cloud (SoftLayer), la zone **Notes** est désormais au format JSON et l'espace de nom Kubernetes dans lequel le volume persistant est déployé a été ajouté. Pour prendre en charge les clusters à zones multiples, des libellés de zone et de région ont été ajoutés pour les volumes persistants.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.3</td>
<td>v1.10.5</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.5).</td>
</tr>
<tr>
<td>Noyau</td>
<td>N/A</td>
<td>N/A</td>
<td>De légères améliorations ont été apportées aux paramètres réseau des noeuds worker pour les charges de travail réseau à hautes performances.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Le déploiement du `vpn` du client OpenVPN qui s'exécute dans l'espace de nom `kube-system` est désormais géré par le gestionnaire `addon-manager` de Kubernetes.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.3_1514, publié le 3 juillet 2018
{: #1103_1514}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.3_1514.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.3_1513">
<caption>Modifications depuis la version 1.10.3_1513</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>N/A</td>
<td>N/A</td>
<td>Optimisation de `sysctl` pour les charges de travail réseau hautes performances.</td>
</tr>
</tbody>
</table>


#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.3_1513, publié le 21 juin 2018
{: #1103_1513}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.3_1513.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.3_1512">
<caption>Modifications depuis la version 1.10.3_1512</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>Pour les types de machine non chiffrés, le disque secondaire est nettoyé en obtenant un nouveau système de fichiers lors du rechargement ou de la mise à jour du noeud worker.</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.10.3_1512, publié le 12 juin 2018
{: #1103_1512}

Le tableau suivant présente les modifications incluses dans le correctif 1.10.3_1512.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.1_1510">
<caption>Modifications depuis la version 1.10.1_1510</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.10.1</td>
<td>v1.10.3</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.3).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de `PodSecurityPolicy` dans l'option `--enable-admission-plugins` pour le serveur d'API Kubernetes du cluster et configuration du cluster pour prendre en charge les politiques de sécurité de pod. Pour plus d'informations, voir [Configuration de politiques de sécurité de pod](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>Configuration de Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>L'option `--authentication-token-webhook` a été activée pour prendre en charge les jetons porteur pour API et les jetons de compte de service pour l'authentification avec le noeud final HTTPS `kubelet`.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.1-52</td>
<td>v1.10.3-85</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.10.3.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de `livenessProbe` dans le déploiement du `vpn` du client OpenVPN qui s'exécute dans l'espace de nom `kube-system`.</td>
</tr>
<tr>
<td>Mise à jour du noyau</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Mise à jour des images de noeud worker avec la mise à jour du noyau pour [CVE-2018-3639 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>



#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.1_1510, publié le 18 mai 2018
{: #1101_1510}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.1_1510.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.1_1509">
<caption>Modifications depuis la version 1.10.1_1509</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correctif pour résoudre un bogue qui se produisait lorsque vous utilisiez le plug-in de stockage par blocs.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.10.1_1509, publié le 16 mai 2018
{: #1101_1509}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.10.1_1509.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.10.1_1508">
<caption>Modifications depuis la version 1.10.1_1508</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Les données que vous stockez dans le répertoire racine `kubelet` sont désormais sauvegardées dans un disque secondaire plus volumineux de la machine du noeud worker.</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 10. 1_1508, publié le 1er mai 2018
{: #1101_1508}

Le tableau suivant présente les modifications incluses dans le correctif 1.10.1_1508.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.7_1510">
<caption>Modifications depuis la version 1.9.7_1510</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v3.1.1</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.1/releases/#v311).</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.0</td>
<td>v1.5.2</td>
<td>Voir les [notes sur l'édition de Kubernetes Heapster ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.10.1</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de <code>StorageObjectInUseProtection</code> à l'option <code>--enable-admission-plugins</code> pour le serveur d'API Kubernetes du cluster.</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>Voir les [notes sur l'édition de Kubernetes DNS ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/dns/releases/tag/1.14.10).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.7-102</td>
<td>v1.10.1-52</td>
<td>Mis à jour pour prendre en charge la version Kubernetes 1.10.</td>
</tr>
<tr>
<td>Prise en charge de GPU</td>
<td>N/A</td>
<td>N/A</td>
<td>La prise en charge des [charges de travail de conteneur d'unité de traitement graphique (GPU)](/docs/containers?topic=containers-app#gpu_app) est désormais disponible pour la planification et l'exécution. Pour obtenir la liste des types de machine GPU disponibles, voir [Matériel pour les noeuds worker](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes). Pour plus d'informations, voir la documentation Kubernetes relative à la [planification d'unités GPU ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).</td>
</tr>
</tbody>
</table>

<br />


### Journal des modifications pour la version 1.9 (non prise en charge depuis le 27 décembre 2018)
{: #19_changelog}

Passez en revue le journal des modifications version 1.9.
{: shortdesc}

*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.11_1539, publié le 17 décembre 2018](#1911_1539)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.11_1538, publié le 4 décembre 2018](#1911_1538)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.11_1537, publié le 27 novembre 2018](#1911_1537)
*   [Journal des modifications pour la version 1.9.11_1536, publié le 19 novembre 2018](#1911_1536)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.10_1532, publié le 7 novembre 2018](#1910_1532)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.10_1531, publié le 26 octobre 2018](#1910_1531)
*   [Journal des modifications du maître - Groupe de correctifs 1.9.10_1530, publié le 15 octobre 2018](#1910_1530)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.10_1528, publié le 10 octobre 2018](#1910_1528)
*   [Journal des modifications pour la version 1.9.10_1527, publié le 2 octobre 2018](#1910_1527)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.10_1524, publié le 20 septembre 2018](#1910_1524)
*   [Journal des modifications pour la version 1.9.10_1523, publié le 4 septembre 2018](#1910_1523)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.9_1522, publié le 23 août 2018](#199_1522)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.9_1521, publié le 13 août 2018](#199_1521)
*   [Journal des modifications pour la version 1.9.9_1520, publié le 27 juillet 2018](#199_1520)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.8_1517, publié le 3 juillet 2018](#198_1517)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.8_1516, publié le 21 juin 2018](#198_1516)
*   [Journal des modifications pour la version 1.9.8_1515, publié le 19 juin 2018](#198_1515)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.7_1513, publié le 11 juin 2018](#197_1513)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.7_1512, publié le 18 mai 2018](#197_1512)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.9.7_1511, publié le 16 mai 2018](#197_1511)
*   [Journal des modifications pour la version 1.9.7_1510, publié le 30 avril 2018](#197_1510)

#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.11_1539, publié le 17 décembre 2018
{: #1911_1539}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.11_1539.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.11_1538">
<caption>Modifications depuis la version 1.9.11_1538</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour des packages Ubuntu installés.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.11_1538, publié le 4 décembre 2018
{: #1911_1538}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.11_1538.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.11_1537">
<caption>Modifications depuis la version 1.9.11_1537</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilisation des ressources de noeud worker</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de groupes cgroups pour le kubelet et docker pour empêcher ces composants d'être à court de ressources. Pour plus d'informations, voir [Réserves de ressources de noeud worker](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.11_1537, publié le 27 novembre 2018
{: #1911_1537}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.11_1537.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.11_1536">
<caption>Modifications depuis la version 1.9.11_1536</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>Voir les [notes sur l'édition de Docker ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.docker.com/engine/release-notes/#18061-ce).</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.9.11_1536, publié le 19 novembre 2018
{: #1911_1536}

Le tableau suivant présente les modifications incluses dans le correctif 1.9.11_1536.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.10_1532">
<caption>Modifications depuis la version 1.9.10_1532</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v2.6.12</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v2.6/releases/#v2612). La mise à jour résout [Tigera Technical Advisory TTA-2018-001 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Noyau</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Mise à jour des images de noeud worker avec une mise à jour du noyau pour [CVE-2018-7755 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.10</td>
<td>v1.9.11</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.11).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.10-219</td>
<td>v1.9.11-249</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.9.11.</td>
</tr>
<tr>
<td>Serveur et client OpenVPN</td>
<td>2.4.4-r2</td>
<td>2.4.6-r3-IKS-8</td>
<td>Mise à jour d'une image pour [CVE-2018-0732 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) et [CVE-2018-0737 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.10_1532, publié le 7 novembre 2018
{: #1910_1532}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.11_1532.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.10_1531">
<caption>Modifications depuis la version 1.9.10_1531</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau avec puce TPM activée</td>
<td>N/A</td>
<td>N/A</td>
<td>Les noeuds worker bare metal avec des puces TPM pour la fonction de calcul sécurisé utilisent le noyau Ubuntu par défaut jusqu'à ce que la fonction de confiance (trust) soit activée. Si vous [activez la fonction de confiance](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) sur un cluster existant, vous devez [recharger](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) tous les noeuds worker bare metal existants avec des puces TPM. Pour vérifier si un noeud worker bare metal comporte une puce TPM, vérifiez la zone **Trustable** après avoir exécuté la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-types --zone`.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.10_1531, publié le 26 octobre 2018
{: #1910_1531}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.10_1531.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.10_1530">
<caption>Modifications depuis la version 1.9.10_1530</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Traitement des interruptions du système d'exploitation</td>
<td>N/A</td>
<td>N/A</td>
<td>Remplacement du démon du système IRQ (Interrupt Request) avec un gestionnaire d'interruption plus performant.</td>
</tr>
</tbody>
</table>

#### Journal des modifications du maître - Groupe de correctifs 1.9.10_1530, publié le 15 octobre 2018
{: #1910_1530}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.10_1530.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.10_1527">
<caption>Modifications depuis la version 1.9.10_1527</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Mise à jour de cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Correction du problème de mise à jour des modules complémentaires de cluster lorsque le maître est mis à jour à partir d'une version non prise en charge.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.10_1528, publié le 10 octobre 2018
{: #1910_1528}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.10_1528.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.10_1527">
<caption>Modifications depuis la version 1.9.10_1527</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Mise à jour des images de noeud worker avec la mise à jour du noyau pour [CVE-2018-14633, CVE-2018-17182 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Délai d'inactivité de session</td>
<td>N/A</td>
<td>N/A</td>
<td>Le délai d'inactivité de session a été fixé à 5 minutes pour des raisons de conformité.</td>
</tr>
</tbody>
</table>


#### Journal des modifications pour la version 1.9.10_1527, publié le 2 octobre 2018
{: #1910_1527}

Le tableau suivant présente les modifications incluses dans le correctif 1.9.10_1527.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.10_1523">
<caption>Modifications depuis la version 1.9.10_1523</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.10-192</td>
<td>v1.9.10-219</td>
<td>Mise à jour du lien vers la documentation dans les messages d'erreur de l'équilibreur de charge.</td>
</tr>
<tr>
<td>Classes de stockage de fichiers d'IBM</td>
<td>N/A</td>
<td>N/A</td>
<td>Suppression de `mountOptions` dans les classes de stockage de fichiers d'IBM afin d'utiliser la valeur par défaut fournie par le noeud worker. Suppression du paramètre `reclaimPolicy` en double dans les classes de stockage de fichiers d'IBM.<br><br>
Désormais, lorsque vous mettez à jour le noeud du maître cluster, la classe de stockage de fichiers d'IBM reste inchangée. Si vous souhaitez modifier la classe de stockage par défaut, exécutez la commande `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` et remplacez `<storageclass>` par le nom de la classe de stockage.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.10_1524, publié le 20 septembre 2018
{: #1910_1524}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.10_1524.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.10_1523">
<caption>Modifications depuis la version 1.9.10_1523</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Rotation des journaux</td>
<td>N/A</td>
<td>N/A</td>
<td>Passage à l'utilisation de fichiers timer `systemd` au lieu de travaux `cronjobs` pour empêcher l'échec de la rotation des journaux (`logrotate`) sur les noeuds worker qui ne sont pas rechargés ou mis à niveau dans les 90 jours. **Remarque** : dans toutes les versions antérieures à cette version secondaire, le disque principal se remplit après l'échec d'un travail cron car il est impossible de faire tourner les journaux. Le travail cron échoue lorsque le noeud worker est actif pendant une durée de 90 jours sans être mis à jour ou rechargé. Si les journaux remplissent la totalité du disque principal, le noeud worker passe à l'état d'échec. Le noeud worker peut être corrigé en exécutant la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` ou la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`. </td>
</tr>
<tr>
<td>Composants d'exécution de noeud worker (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/A</td>
<td>N/A</td>
<td>Suppression des dépendances des composants d'exécution sur le disque principal. Cette amélioration empêche l'échec des noeuds worker lorsque le disque principal est plein.</td>
</tr>
<tr>
<td>Expiration du mot de passe root</td>
<td>N/A</td>
<td>N/A</td>
<td>Les mots de passe root des noeuds worker expirent au bout de 90 jours pour des raisons de conformité. Si vos outils d'automatisation doivent se connecter au noeud worker en tant que root ou dépendent de travaux cron qui s'exécutent en tant que root, vous pouvez désactiver l'expiration du mot de passe en vous connectant au noeud worker et en exécutant la commande `chage -M -1 root`. **Remarque** : si vos exigences de conformité en matière de sécurité vous empêchent toute exécution en tant que root ou l'annulation de l'expiration du mot de passe, ne désactivez pas l'expiration. Vous pouvez à la place [mettre à jour](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) ou [recharger](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) vos noeuds worker au moins tous les 90 jours.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>Nettoyage régulier des unités de montage transitoires pour leur éviter de devenir illimitées. Cette action corrige l'erreur [Kubernetes 57345 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>Désactivation du pont Docker par défaut pour que la plage d'adresses IP `172.17.0.0/16` soit désormais utilisée pour les routes privées. Si vos conteneurs Docker doivent être construits en exécutant des commandes `docker` directement sur l'hôte ou en utilisant un pod qui monte le socket Docker, choisissez l'une des options suivantes.<ul><li>Pour assurer la connectivité avec le réseau externe lorsque vous construisez le conteneur, exécutez la commande `docker build . --network host`.</li>
<li>Pour créer de manière explicite un réseau à utiliser lorsque vous construisez le conteneur, exécutez la commande `docker network create`, puis utilisez ce réseau.</li></ul>
**Remarque** : vous avez des dépendances sur le socket Docker ou directement sur Docker ? [Effectuez une mise à jour pour passer à l'environnement d'exécution de conteneur `containerd` au lieu de `docker`](/docs/containers?topic=containers-cs_versions#containerd) pour que vos clusters soient prêts à exécuter Kubernetes version 1.11 ou ultérieure.</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.9.10_1523, publié le 4 septembre 2018
{: #1910_1523}

Le tableau suivant présente les modifications incluses dans le correctif 1.9.10_1523.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.9_1522">
<caption>Modifications depuis la version 1.9.9_1522</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.9-167</td>
<td>v1.9.10-192</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.9.10. En outre, la configuration du fournisseur de cloud a changé pour mieux gérer les mises à jour des services d'équilibreur de charge avec l'élément `externalTrafficPolicy` défini sur `local`.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>334</td>
<td>338</td>
<td>Mise à jour d'incubateur à la version 1.8. Le stockage de fichiers est mis à disposition dans la zone spécifique que vous sélectionnez. Vous ne pouvez pas mettre à jour des libellés d'une instance de volume persistant (statique) existante, à moins d'utiliser un cluster à zones multiples et de nécessiter l'ajout de libellés de région et de zone.<br><br>La version NFS par défaut a été supprimée des options de montage dans les classes de stockage de fichiers fournies par IBM. Désormais, le système d'exploitation de l'hôte négocie la version NFS avec le serveur NFS de l'infrastructure IBM Cloud (SoftLayer). Pour définir manuellement une version NFS spécifique ou modifier la version NFS de votre volume persistant qui a été négociée par le système d'exploitation de l'hôte, voir [Modification de la version NFS par défaut](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.9</td>
<td>v1.9.10</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.10).</td>
</tr>
<tr>
<td>Configuration de Kubernetes Heapster</td>
<td>N/A</td>
<td>N/A</td>
<td>Augmentation des limites de ressources pour le conteneur `heapster-nanny`.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.9_1522, publié le 23 août 2018
{: #199_1522}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.9_1522.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.9_1521">
<caption>Modifications depuis la version 1.9.9_1521</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Mise à jour de `systemd` pour corriger la fuite liée à `cgroup`.</td>
</tr>
<tr>
<td>Noyau</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Mise à jour des images de noeud worker avec la mise à jour du noyau pour [CVE-2018-3620, CVE-2018-3646 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>


#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.9_1521, publié le 13 août 2018
{: #199_1521}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.9_1521.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.9_1520">
<caption>Modifications depuis la version 1.9.9_1520</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour des packages Ubuntu installés.</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.9.9_1520, publié le 27 juillet 2018
{: #199_1520}

Le tableau suivant présente les modifications incluses dans le correctif 1.9.9_1520.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.8_1517">
<caption>Modifications depuis la version 1.9.8_1517</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.8-141</td>
<td>v1.9.9-167</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.9.9. En outre, les événements `create failure` du service LoadBalancer incluent désormais toutes les erreurs des sous-réseaux portables.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>320</td>
<td>334</td>
<td>Le délai de création d'un volume persistant a augmenté pour passer de 15 à 30 minutes. Le type de facturation par défaut a été changé pour passer à une facturation à l'heure (`hourly`). Des options de montage ont été ajoutées aux classes de stockage prédéfinies. Dans l'instance de stockage de fichiers NFS dans votre compte d'infrastructure IBM Cloud (SoftLayer), la zone **Notes** est désormais au format JSON et l'espace de nom Kubernetes dans lequel le volume persistant est déployé a été ajouté. Pour prendre en charge les clusters à zones multiples, des libellés de zone et de région ont été ajoutés pour les volumes persistants.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.8</td>
<td>v1.9.9</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.9).</td>
</tr>
<tr>
<td>Noyau</td>
<td>N/A</td>
<td>N/A</td>
<td>De légères améliorations ont été apportées aux paramètres réseau des noeuds worker pour les charges de travail réseau à hautes performances.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Le déploiement du `vpn` du client OpenVPN qui s'exécute dans l'espace de nom `kube-system` est désormais géré par le gestionnaire `addon-manager` de Kubernetes.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.8_1517, publié le 3 juillet 2018
{: #198_1517}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.8_1517.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.8_1516">
<caption>Modifications depuis la version 1.9.8_1516</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>N/A</td>
<td>N/A</td>
<td>Optimisation de `sysctl` pour les charges de travail réseau hautes performances.</td>
</tr>
</tbody>
</table>


#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.8_1516, publié le 21 juin 2018
{: #198_1516}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.8_1516.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.8_1515">
<caption>Modifications depuis la version 1.9.8_1515</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>Pour les types de machine non chiffrés, le disque secondaire est nettoyé en obtenant un nouveau système de fichiers lors du rechargement ou de la mise à jour du noeud worker.</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.9.8_1515, publié le 19 juin 2018
{: #198_1515}

Le tableau suivant présente les modifications incluses dans le correctif 1.9.8_1515.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.7_1513">
<caption>Modifications depuis la version 1.9.7_1513</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.9.8</td>
<td>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.8).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de `PodSecurityPolicy` dans l'option `--admission-control` pour le serveur d'API Kubernetes du cluster et configuration du cluster pour prendre en charge les politiques de sécurité de pod. Pour plus d'informations, voir [Configuration de politiques de sécurité de pod](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>IBM Cloud Provider</td>
<td>v1.9.7-102</td>
<td>v1.9.8-141</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.9.8.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de <code>livenessProbe</code> dans le déploiement du <code>vpn</code> du client OpenVPN qui s'exécute dans l'espace de nom <code>kube-system</code>.</td>
</tr>
</tbody>
</table>


#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.7_1513, publié le 11 juin 2018
{: #197_1513}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.7_1513.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.7_1512">
<caption>Modifications depuis la version 1.9.7_1512</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Mise à jour du noyau</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Mise à jour des images de noeud worker avec la mise à jour du noyau pour [CVE-2018-3639 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.7_1512, publié le 18 mai 2018
{: #197_1512}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.7_1512.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.7_1511">
<caption>Modifications depuis la version 1.9.7_1511</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correctif pour résoudre un bogue qui se produisait lorsque vous utilisiez le plug-in de stockage par blocs.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.9.7_1511, publié le 16 mai 2018
{: #197_1511}

Le tableau suivant présente les modifications incluses dans le groupe de correctifs de noeud worker 1.9.7_1511.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.7_1510">
<caption>Modifications depuis la version 1.9.7_1510</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Les données que vous stockez dans le répertoire racine `kubelet` sont désormais sauvegardées dans un disque secondaire plus volumineux de la machine du noeud worker.</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.9.7_1510, publié le 30 avril 2018
{: #197_1510}

Le tableau suivant présente les modifications incluses dans le correctif 1.9.7_1510.
{: shortdesc}

<table summary="Modifications effectuées depuis la version 1.9.3_1506">
<caption>Modifications depuis la version 1.9.3_1506</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.3</td>
<td>v1.9.7	</td>
<td><p>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7). Cette édition traite les vulnérabilités [CVE-2017-1002101 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) et [CVE-2017-1002102 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p><strong>Remarque</strong> : désormais les éléments `secret`, `configMap`, `downwardAPI` et les volumes projetés sont montés en lecture seule. Auparavant, les applications pouvaient écrire des données dans ces volumes, mais le système pouvait les rétablir automatiquement. Si vos applications s'appuient sur ce comportement peu fiable en matière de sécurité, modifiez-les en conséquence.</p></td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de `admissionregistration.k8s.io/v1alpha1=true` à l'option `--runtime-config` pour le serveur d'API Kubernetes du cluster.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.3-71</td>
<td>v1.9.7-102</td>
<td>Les services `NodePort` et `LoadBalancer` prennent désormais en charge la [conservation de l'adresse IP source du client](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) en définissant l'élément `service.spec.externalTrafficPolicy` sur `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Correction de la configuration de tolérance des [noeuds de périphérie](/docs/containers?topic=containers-edge#edge) pour les clusters plus anciens.</td>
</tr>
</tbody>
</table>

<br />


### Journal des modifications pour la version 1.8 (non prise en charge)
{: #18_changelog}

Passez en revue le journal des modifications version 1.8.
{: shortdesc}

*   [Journal des modifications de noeud worker - Groupe de correctifs 1.8.15_1521, publié le 20 septembre 2018](#1815_1521)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.8.15_1520, publié le 23 août 2018](#1815_1520)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.8.15_1519, publié le 13 août 2018](#1815_1519)
*   [Journal des modifications pour la version 1.8.15_1518, publié le 27 juillet 2018](#1815_1518)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.8.13_1516, publié le 3 juillet 2018](#1813_1516)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.8.13_1515, publié le 21 juin 2018](#1813_1515)
*   [Journal des modifications pour la version 1.8.13_1514, publié le 19 juin 2018](#1813_1514)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.8.11_1512, publié le 11 juin 2018](#1811_1512)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.8.11_1511, publié le 18 mai 2018](#1811_1511)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.8.11_1510, publié le 16 mai 2018](#1811_1510)
*   [Journal des modifications pour la version 1.8.11_1509, publié le 19 avril 2018](#1811_1509)

#### Journal des modifications de noeud worker - Groupe de correctifs 1.8.15_1521, publié le 20 septembre 2018
{: #1815_1521}

<table summary="Modifications effectuées depuis la version 1.8.15_1520">
<caption>Modifications depuis la version 1.8.15_1520</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Rotation des journaux</td>
<td>N/A</td>
<td>N/A</td>
<td>Passage à l'utilisation de fichiers timer `systemd` au lieu de travaux `cronjobs` pour empêcher l'échec de la rotation des journaux (`logrotate`) sur les noeuds worker qui ne sont pas rechargés ou mis à niveau dans les 90 jours. **Remarque** : dans toutes les versions antérieures à cette version secondaire, le disque principal se remplit après l'échec d'un travail cron car il est impossible de faire tourner les journaux. Le travail cron échoue lorsque le noeud worker est actif pendant une durée de 90 jours sans être mis à jour ou rechargé. Si les journaux remplissent la totalité du disque principal, le noeud worker passe à l'état d'échec. Le noeud worker peut être corrigé en exécutant la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` ou la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`. </td>
</tr>
<tr>
<td>Composants d'exécution de noeud worker (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/A</td>
<td>N/A</td>
<td>Suppression des dépendances des composants d'exécution sur le disque principal. Cette amélioration empêche l'échec des noeuds worker lorsque le disque principal est plein.</td>
</tr>
<tr>
<td>Expiration du mot de passe root</td>
<td>N/A</td>
<td>N/A</td>
<td>Les mots de passe root des noeuds worker expirent au bout de 90 jours pour des raisons de conformité. Si vos outils d'automatisation doivent se connecter au noeud worker en tant que root ou dépendent de travaux cron qui s'exécutent en tant que root, vous pouvez désactiver l'expiration du mot de passe en vous connectant au noeud worker et en exécutant la commande `chage -M -1 root`. **Remarque** : si vos exigences de conformité en matière de sécurité vous empêchent toute exécution en tant que root ou l'annulation de l'expiration du mot de passe, ne désactivez pas l'expiration. Vous pouvez à la place [mettre à jour](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) ou [recharger](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) vos noeuds worker au moins tous les 90 jours.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>Nettoyage régulier des unités de montage transitoires pour leur éviter de devenir illimitées. Cette action corrige l'erreur [Kubernetes 57345 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.8.15_1520, publié le 23 août 2018
{: #1815_1520}

<table summary="Modifications effectuées depuis la version 1.8.15_1519">
<caption>Modifications depuis la version 1.8.15_1519</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Mise à jour de `systemd` pour corriger la fuite liée à `cgroup`.</td>
</tr>
<tr>
<td>Noyau</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Mise à jour des images de noeud worker avec la mise à jour du noyau pour [CVE-2018-3620, CVE-2018-3646 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.8.15_1519, publié le 13 août 2018
{: #1815_1519}

<table summary="Modifications effectuées depuis la version 1.8.15_1518">
<caption>Modifications depuis la version 1.8.15_1518</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Packages Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Mises à jour des packages Ubuntu installés.</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.8.15_1518, publié le 27 juillet 2018
{: #1815_1518}

<table summary="Modifications effectuées depuis la version 1.8.13_1516">
<caption>Modifications depuis la version 1.8.13_1516</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.13-176</td>
<td>v1.8.15-204</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.8.15. En outre, les événements `create failure` du service LoadBalancer incluent désormais toutes les erreurs des sous-réseaux portables.</td>
</tr>
<tr>
<td>Plug-in {{site.data.keyword.Bluemix_notm}} File Storage</td>
<td>320</td>
<td>334</td>
<td>Le délai de création d'un volume persistant a augmenté pour passer de 15 à 30 minutes. Le type de facturation par défaut a été changé pour passer à une facturation à l'heure (`hourly`). Des options de montage ont été ajoutées aux classes de stockage prédéfinies. Dans l'instance de stockage de fichiers NFS dans votre compte d'infrastructure IBM Cloud (SoftLayer), la zone **Notes** est désormais au format JSON et l'espace de nom Kubernetes dans lequel le volume persistant est déployé a été ajouté. Pour prendre en charge les clusters à zones multiples, des libellés de zone et de région ont été ajoutés pour les volumes persistants.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.8.13</td>
<td>v1.8.15</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.15).</td>
</tr>
<tr>
<td>Noyau</td>
<td>N/A</td>
<td>N/A</td>
<td>De légères améliorations ont été apportées aux paramètres réseau des noeuds worker pour les charges de travail réseau à hautes performances.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Le déploiement du `vpn` du client OpenVPN qui s'exécute dans l'espace de nom `kube-system` est désormais géré par le gestionnaire `addon-manager` de Kubernetes.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.8.13_1516, publié le 3 juillet 2018
{: #1813_1516}

<table summary="Modifications effectuées depuis la version 1.8.13_1515">
<caption>Modifications depuis la version 1.8.13_1515</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noyau</td>
<td>N/A</td>
<td>N/A</td>
<td>Optimisation de `sysctl` pour les charges de travail réseau hautes performances.</td>
</tr>
</tbody>
</table>


#### Journal des modifications de noeud worker - Groupe de correctifs 1.8.13_1515, publié le 21 juin 2018
{: #1813_1515}

<table summary="Modifications effectuées depuis la version 1.8.13_1514">
<caption>Modifications depuis la version 1.8.13_1514</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>Pour les types de machine non chiffrés, le disque secondaire est nettoyé en obtenant un nouveau système de fichiers lors du rechargement ou de la mise à jour du noeud worker.</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.8.13_1514, publié le 19 juin 2018
{: #1813_1514}

<table summary="Modifications effectuées depuis la version 1.8.11_1512">
<caption>Modifications depuis la version 1.8.11_1512</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.11</td>
<td>v1.8.13</td>
<td>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.13).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de `PodSecurityPolicy` dans l'option `--admission-control` pour le serveur d'API Kubernetes du cluster et configuration du cluster pour prendre en charge les politiques de sécurité de pod. Pour plus d'informations, voir [Configuration de politiques de sécurité de pod](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>IBM Cloud Provider</td>
<td>v1.8.11-126</td>
<td>v1.8.13-176</td>
<td>Mise à jour pour prendre en charge l'édition Kubernetes 1.8.13.</td>
</tr>
<tr>
<td>Client OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de <code>livenessProbe</code> dans le déploiement du <code>vpn</code> du client OpenVPN qui s'exécute dans l'espace de nom <code>kube-system</code>.</td>
</tr>
</tbody>
</table>


#### Journal des modifications de noeud worker - Groupe de correctifs 1.8.11_1512, publié le 11 juin 2018
{: #1811_1512}

<table summary="Modifications effectuées depuis la version 1.8.11_1511">
<caption>Modifications depuis la version 1.8.11_1511</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Mise à jour du noyau</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Mise à jour des images de noeud worker avec la mise à jour du noyau pour [CVE-2018-3639 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>


#### Journal des modifications de noeud worker - Groupe de correctifs 1.8.11_1511, publié le 18 mai 2018
{: #1811_1511}

<table summary="Modifications effectuées depuis la version 1.8.11_1510">
<caption>Modifications depuis la version 1.8.11_1510</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correctif pour résoudre un bogue qui se produisait lorsque vous utilisiez le plug-in de stockage par blocs.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.8.11_1510, publié le 16 mai 2018
{: #1811_1510}

<table summary="Modifications effectuées depuis la version 1.8.11_1509">
<caption>Modifications depuis la version 1.8.11_1509</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Les données que vous stockez dans le répertoire racine `kubelet` sont désormais sauvegardées dans un disque secondaire plus volumineux de la machine du noeud worker.</td>
</tr>
</tbody>
</table>


#### Journal des modifications pour la version 1.8.11_1509, publié le 19 avril 2018
{: #1811_1509}

<table summary="Modifications effectuées depuis la version 1.8.8_1507">
<caption>Modifications depuis la version 1.8.8_1507</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td><p>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11). Cette édition traite les vulnérabilités [CVE-2017-1002101 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) et [CVE-2017-1002102 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p>Désormais les éléments `secret`, `configMap`, `downwardAPI` et les volumes projetés sont montés en mode lecture seule. Auparavant, les applications pouvaient écrire des données dans ces volumes, mais le système pouvait les rétablir automatiquement. Si vos applications s'appuient sur ce comportement peu fiable en matière de sécurité, modifiez-les en conséquence.</p></td>
</tr>
<tr>
<td>Image de conteneur Pause</td>
<td>3.0</td>
<td>3.1</td>
<td>Supprime les processus zombie orphelins hérités.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>Les services `NodePort` et `LoadBalancer` prennent désormais en charge la [conservation de l'adresse IP source du client](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) en définissant l'élément `service.spec.externalTrafficPolicy` sur `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Correction de la configuration de tolérance des [noeuds de périphérie](/docs/containers?topic=containers-edge#edge) pour les clusters plus anciens.</td>
</tr>
</tbody>
</table>

<br />


### Journal des modifications pour la version 1.7 (non prise en charge)
{: #17_changelog}

Passez en revue le journal des modifications version 1.7.
{: shortdesc}

*   [Journal des modifications de noeud worker - Groupe de correctifs 1.7.16_1514, publié le 11 juin 2018](#1716_1514)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.7.16_1513, publié le 18 mai 2018](#1716_1513)
*   [Journal des modifications de noeud worker - Groupe de correctifs 1.7.16_1512, publié le 16 mai 2018](#1716_1512)
*   [Journal des modifications pour la version 1.7.16_1511, publié le 19 avril 2018](#1716_1511)

#### Journal des modifications de noeud worker - Groupe de correctifs 1.7.16_1514, publié le 11 juin 2018
{: #1716_1514}

<table summary="Modifications effectuées depuis la version 1.7.16_1513">
<caption>Modifications depuis la version 1.7.16_1513</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Mise à jour du noyau</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Mise à jour des images de noeud worker avec la mise à jour du noyau pour [CVE-2018-3639 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.7.16_1513, publié le 18 mai 2018
{: #1716_1513}

<table summary="Modifications effectuées depuis la version 1.7.16_1512">
<caption>Modifications depuis la version 1.7.16_1512</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correctif pour résoudre un bogue qui se produisait lorsque vous utilisiez le plug-in de stockage par blocs.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.7.16_1512, publié le 16 mai 2018
{: #1716_1512}

<table summary="Modifications effectuées depuis la version 1.7.16_1511">
<caption>Modifications depuis la version 1.7.16_1511</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Les données que vous stockez dans le répertoire racine `kubelet` sont désormais sauvegardées dans un disque secondaire plus volumineux de la machine du noeud worker.</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.7.16_1511, publié le 19 avril 2018
{: #1716_1511}

<table summary="Modifications effectuées depuis la version 1.7.4_1509">
<caption>Modifications depuis la version 1.7.4_1509</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td><p>Voir les [Notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16). Cette édition traite les vulnérabilités [CVE-2017-1002101 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) et [CVE-2017-1002102 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p>Désormais les éléments `secret`, `configMap`, `downwardAPI` et les volumes projetés sont montés en mode lecture seule. Auparavant, les applications pouvaient écrire des données dans ces volumes, mais le système pouvait les rétablir automatiquement. Si vos applications s'appuient sur ce comportement peu fiable en matière de sécurité, modifiez-les en conséquence.</p></td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>Les services `NodePort` et `LoadBalancer` prennent désormais en charge la [conservation de l'adresse IP source du client](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) en définissant l'élément `service.spec.externalTrafficPolicy` sur `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Correction de la configuration de tolérance des [noeuds de périphérie](/docs/containers?topic=containers-edge#edge) pour les clusters plus anciens.</td>
</tr>
</tbody>
</table>
