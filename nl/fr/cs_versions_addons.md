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


# Journal des modifications de l'équilibreur de charge d'application Ingress et de Fluentd
{: #cluster-add-ons-changelog}

Votre cluster {{site.data.keyword.containerlong}} est livré avec des composants, par exemple Fluentd et ALB Ingress, qui sont mis à jour automatiquement par IBM. Vous pouvez également désactiver les mises à jour automatiques pour certains composants et effectuer manuellement leur mise à jour indépendamment du maître et des noeuds worker. Consultez les tableaux des sections suivantes pour obtenir un récapitulatif des modifications de chaque version.
{: shortdesc}

Pour plus d'informations sur la gestion des mises à jour pour Fluentd et les ALB Ingress, voir [Mise à jour des composants de cluster](/docs/containers?topic=containers-update#components).

## Journal des modifications des ALB Ingress
{: #alb_changelog}

Affichez les modifications de version pour les équilibreurs de charge d'application (ALB) Ingress dans vos clusters {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Lorsque le composant ALB Ingress est mis à jour, les conteneurs `nginx-ingress` et `ingress-auth` figurant dans tous les pods d'ALB sont mis à jour vers la dernière version. Par défaut, les mises à jour automatiques du composant sont activées mais vous pouvez les désactiver pour effectuer manuellement la mise à jour du composant. Pour plus d'informations, voir la section sur la [mise à jour de l'équilibreur de charge d'application (ALB) Ingress](/docs/containers?topic=containers-update#alb).

Consultez le tableau suivant pour obtenir un récapitulatif des modifications de chaque version du composant ALB Ingress. 

<table summary="Présentation des modifications de version du composant d'équilibreur de charge d'application (ALB) Ingress">
<caption>Journal des modifications du composant d'équilibreur de charge d'application Ingress</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>Version `nginx-ingress` / `ingress-auth`</th>
<th>Date de publication</th>
<th>Modifications sans interruption</th>
<th>Modifications nécessitant une interruption</th>
</tr>
</thead>
<tbody>
<tr>
<td>470 / 330</td>
<td>7 juin 2019</td>
<td>Correction des vulnérabilités de BD Berkeley pour [CVE-2019-8457 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457).
</td>
<td>-</td>
</tr>
<tr>
<td>470 / 329</td>
<td>6 juin 2019</td>
<td>Correction des vulnérabilités de BD Berkeley pour [CVE-2019-8457 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457).
</td>
<td>-</td>
</tr>
<tr>
<td>467 / 329</td>
<td>3 juin 2019</td>
<td>Correction des vulnérabilités GnuTLS pour [CVE-2019-3829 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-3893 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3893), [CVE-2018-10844 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10845 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844) et [CVE-2018-10846 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846).
</td>
<td>-</td>
</tr>
<tr>
<td>462 / 329</td>
<td>28 mai 2019</td>
<td>Correction des vulnérabilités cURL pour [CVE-2019-5435 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) et [CVE-2019-5436 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
<td>-</td>
</tr>
<tr>
<td>457 / 329</td>
<td>23 mai 2019</td>
<td>Correction de vulnérabilités Go pour les analyses d'image.</td>
<td>-</td>
</tr>
<tr>
<td>423 / 329</td>
<td>13 mai 2019</td>
<td>Amélioration des performances de l'intégration d'{{site.data.keyword.appid_full}}.</td>
<td>-</td>
</tr>
<tr>
<td>411 / 315</td>
<td>15 avril 2019</td>
<td>Mise à jour de la valeur de l'expiration du cookie {{site.data.keyword.appid_full_notm}} de sorte qu'elle corresponde à la valeur de l'expiration du jeton d'accès.</td>
<td>-</td>
</tr>
<tr>
<td>411 / 306</td>
<td>22 mars 2019</td>
<td>Mise à jour de la version Go à 1.12.1.</td>
<td>-</td>
</tr>
<tr>
<td>410 / 305</td>
<td>18 mars 2019</td>
<td><ul>
<li>Correction de vulnérabilités pour les analyses d'image.</li>
<li>Amélioration de la fonction de consignation pour l'intégration d'{{site.data.keyword.appid_full_notm}}.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>408 / 304</td>
<td>5 mars 2019</td>
<td>-</td>
<td>Correction de bogues dans l'intégration d'autorisation concernant la fonctionnalité de déconnexion, l'expiration de jeton et le rappel d'autorisation `OAuth`. Ces corrections sont appliquées uniquement si vous avez activé l'autorisation {{site.data.keyword.appid_full_notm}} en utilisant l'annotation [`appid-auth`](/docs/containers?topic=containers-ingress_annotation#appid-auth). Pour implémenter ces corrections, des en-têtes supplémentaires sont ajoutés, ce qui augmente la taille totale des en-têtes. En fonction de la taille de vos propres en-têtes et de la taille totale des réponses, il vous faudra peut-être ajuster les [annotations de mémoire tampon de proxy](/docs/containers?topic=containers-ingress_annotation#proxy-buffer) que vous utilisez.</td>
</tr>
<tr>
<td>406 / 301</td>
<td>19 fév. 2019</td>
<td><ul>
<li>Mise à jour de la version Go à 1.11.5.</li>
<li>Correction de vulnérabilités pour les analyses d'image.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>404 / 300</td>
<td>31 jan. 2019</td>
<td>Mise à jour de la version Go à 1.11.4.</td>
<td>-</td>
</tr>
<tr>
<td>403 / 295</td>
<td>23 jan. 2019</td>
<td><ul>
<li>Mise à jour de la version NGINX des équilibreurs de charge d'application (ALB) à 1.15.2.</li>
<li>Les certificats TLS fournis par IBM sont désormais renouvelés 37 jours avant leur date d'expiration et non plus 7 jours avant.</li>
<li>Ajout de la fonctionnalité de déconnexion {{site.data.keyword.appid_full_notm}} : si le préfixe `/logout` figure dans un chemin {{site.data.keyword.appid_full_notm}}, les cookies sont supprimés et l'utilisateur est redirigé vers la page de connexion.</li>
<li>Ajout d'un en-tête aux demandes {{site.data.keyword.appid_full_notm}} pour assurer un suivi interne.</li>
<li>Mise à jour de la directive d'emplacement {{site.data.keyword.appid_short_notm}} pour pouvoir utiliser l'annotation `app-id` avec les annotations `proxy-buffers`, `proxy-buffer-size` et `proxy-busy-buffer-size`.</li>
<li>Correction d'un bogue pour que les journaux d'information ne soient pas labellisés comme des erreurs.</li>
</ul></td>
<td>Désactivation de TLS 1.0 et 1.1 par défaut. Si les clients qui se connectent à vos applications prennent en charge TLS 1.2, aucune action n'est requise. Si vous avez toujours des clients qui nécessitent la prise en charge de TLS 1.0 ou 1.1, activez manuellement les versions TLS requises en suivant [ces étapes](/docs/containers?topic=containers-ingress#ssl_protocols_ciphers). Pour plus d'informations sur l'affichage des versions TLS utilisées par vos clients pour accéder à vos applications, voir cet [article du blogue {{site.data.keyword.Bluemix_notm}}](https://www.ibm.com/blogs/bluemix/2018/11/ibm-cloud-kubernetes-service-alb-update-tls-1-0-and-1-1-disabled-by-default/).</td>
</tr>
<tr>
<td>393 / 291</td>
<td>9 jan. 2019</td>
<td>Ajout de la prise en charge de l'intégration à plusieurs instances {{site.data.keyword.appid_full_notm}}.</td>
<td>-</td>
</tr>
<tr>
<td>393 / 282</td>
<td>29 nov. 2018</td>
<td>Amélioration des performances de l'intégration d'{{site.data.keyword.appid_full_notm}}.</td>
<td>-</td>
</tr>
<tr>
<td>384 / 246</td>
<td>14 nov. 2018</td>
<td>Amélioration des fonctions de connexion et de déconnexion pour l'intégration d'{{site.data.keyword.appid_full_notm}}.</td>
<td>Le certificat autosigné pour `*.containers.mybluemix.net` est remplacé par le certificat signé LetsEncrypt automatiquement généré et utilisé par le cluster. Le certificat autosigné `*.containers.mybluemix.net` est retiré.</td>
</tr>
<tr>
<td>350 / 192</td>
<td>5 nov. 2018</td>
<td>Ajout de la prise en charge relative à l'activation et à la désactivation des mises à jour automatiques pour le composant ALB Ingress.</td>
<td>-</td>
</tr>
</tbody>
</table>

## Fluentd pour la consignation du journal des modifications
{: #fluentd_changelog}

Affichez les modifications de version du composant Fluentd pour la consignation dans vos clusters {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Par défaut, les mises à jour automatiques du composant sont activées mais vous pouvez les désactiver pour effectuer manuellement la mise à jour du composant. Pour plus d'informations, voir [Gestion des mises à jour automatiques pour Fluentd](/docs/containers?topic=containers-update#logging-up).

Consultez le tableau suivant pour obtenir un récapitulatif des modifications de chaque version du composant Fluentd. 

<table summary="Présentation des modifications de version du composant Fluentd">
<caption>Journal des modifications du composant Fluentd</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>Version Fluentd</th>
<th>Date de publication</th>
<th>Modifications sans interruption</th>
<th>Modifications nécessitant une interruption</th>
</tr>
</thead>
<tr>
<td>e7c10d74350dc64d4d92ba7f72bb4ff9219315d2</td>
<td>30 mai 2019</td>
<td>Mise à jour de l'objet ConfigMap Fluent pour ignorer systématiquement les journaux de pod des espaces de nom IBM, même lorsque le maître Kubernetes n'est pas disponible. </td>
<td>-</td>
</tr>
<tr>
<td>c16fe1602ab65db4af0a6ac008f99ca2a526e6f6</td>
<td>21 mai 2019</td>
<td>Correction d'un bogue à cause duquel les métriques de noeud worker ne s'affichent pas. </td>
<td>-</td>
</tr>
<tr>
<td>60fc11f7bd39d9c6cfed923c598bf6457b3f2037</td>
<td>10 mai 2019</td>
<td>Mise à jour des packages Ruby pour [CVE-2019-8320 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320), [CVE-2019-8321 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321), [CVE-2019-8322 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322), [CVE-2019-8323 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323), [CVE-2019-8324 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) et [CVE-2019-8325 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325).</td>
<td>-</td>
</tr>
<tr>
<td>91a737f68f7d9e81b5d2223c910aaa7d7f91b76d</td>
<td>8 mai 2019</td>
<td>Mise à jour des packages Ruby pour [CVE-2019-8320 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320), [CVE-2019-8321 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321), [CVE-2019-8322 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322), [CVE-2019-8323 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323), [CVE-2019-8324 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) et [CVE-2019-8325 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325).</td>
<td>-</td>
</tr>
<tr>
<td>d9af69e286986a05ed4a50469585b1cf978ddb1d</td>
<td>11 avril 2019</td>
<td>Mise à jour du plug-in cAdvisor pour utiliser TLS 1.2.</td>
<td>-</td>
</tr>
<tr>
<td>3100ddb62580a9f46ffdff7bab2ebec40b164de6</td>
<td>1er avril 2019</td>
<td>Mise à jour du compte de service Fluentd. </td>
<td>-</td>
</tr>
<tr>
<td>c85567b75bd7ad1c9428794cd63a8e239c3fd8f5</td>
<td>18 mars 2019</td>
<td>Retrait de la dépendance sur cURL pour [CVE-2019-8323 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323).</td>
<td>-</td>
</tr>
<tr>
<td>320ffdf87de068ee2f7f34c0e7a47a111e8d457b</td>
<td>18 février 2019</td>
<td><ul>
<li>Mise à jour de Fluend vers la version 1.3.</li>
<li>Retrait de Git de l'image Fluentd pour [CVE-2018-19486 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19486).</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>972865196aefd3324105087878de12c518ed579f</td>
<td>1er janvier 2019</td>
<td><ul>
<li>Activation du codage UTF-8 pour le plug-in Fluentd `in_tail`. </li>
<li>Correctifs de bogues mineurs.
</li>
</ul></td>
<td>-</td>
</tr>
</tbody>
</table>
