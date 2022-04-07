---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

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


# Einführung in {{site.data.keyword.containerlong_notm}}
{: #container_index}

Mit {{site.data.keyword.containerlong}} sind Sie im Handumdrehen einsatzbereit, indem Sie hoch verfügbare Apps in Docker-Containern bereitstellen, die in Kubernetes-Clustern ausgeführt werden.
{:shortdesc}

Ein Container ist ein Standardverfahren zum Packen von Apps mit allen zugehörigen Abhängigkeiten, sodass sich die Apps nahtlos zwischen Umgebungen verlagern lassen. Im Unterschied zu virtuellen Maschinen ist in Containern das Betriebssystem nicht enthalten. Nur App-Code, Laufzeit, Systemtools, Bibliotheken und Einstellungen werden in Container gepackt. Container sind schlanker, leichter portierbar und effizienter als virtuelle Maschinen.

## Einführung in Cluster
{: #clusters_gs_index}

Sie möchten also eine App in einem Container bereitstellen? Dann müssen Sie als Erstes einen Kubernetes-Cluster erstellen. Kubernetes ist ein Orchestrierungstool für Container. Mit Kubernetes können Entwickler hoch verfügbare Apps im Handumdrehen bereitstellen, indem sie sich die Leistungsstärke und Flexibilität von Clustern zunutze machen.
{:shortdesc}

Und was ist ein Cluster? Ein Cluster ist eine Gruppe von Ressourcen, Knoten, Netzen und Speichereinheiten, die die Hochverfügbarkeit von Apps sicherstellen. Nachdem Sie den Cluster erstellt haben, können Sie Ihre Apps in Containern bereitstellen.

Rufen Sie vor Beginn den für Sie geeigneten [{{site.data.keyword.Bluemix_notm}}-Kontotyp](https://cloud.ibm.com/registration) ab:
* **Belastbar (nutzungsabhängig oder Abonnement)**: Sie können einen kostenlosen Cluster erstellen. Sie können auch Ressourcen der IBM Cloud-Infrastruktur (SoftLayer) für die Erstellung und Verwendung in Standardclustern bereitstellen.
* **Lite**: Es ist nicht möglich, einen kostenlosen oder einen Standardcluster zu erstellen. [Aktualisieren Sie Ihr Konto](/docs/account?topic=account-accountfaqs#changeacct) in ein belastbares Konto.

Gehen Sie wie folgt vor, um einen kostenlosen Cluster zu erstellen:

1.  Wählen Sie im [{{site.data.keyword.Bluemix_notm}}-**Katalog** ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/catalog?category=containers) den Eintrag **{{site.data.keyword.containershort_notm}}** aus und klicken Sie auf **Erstellen**. Es wird eine Seite für die Clusterkonfiguration geöffnet. Standardmäßig ist **Kostenloser Cluster** ausgewählt.

2.  Geben Sie dem Cluster einen eindeutigen Namen.

3.  Klicken Sie auf **Cluster erstellen**. Es wird ein Worker-Pool erstellt, der einen Workerknoten enthält. Die Bereitstellung des Workerknotens kann einige Minuten dauern, der Fortschritt wird jedoch auf der Registerkarte **Workerknoten** angezeigt. Wenn der Status `Bereit` lautet, können Sie anfangen, mit dem Cluster zu arbeiten!

<br>

Hervorragend! Sie haben Ihren ersten Kubernetes-Cluster erstellt. Im Folgenden finden Sie einige Details zu Ihrem kostenlosen Cluster:

*   **Maschinentyp**: Der kostenlose Cluster verfügt über einen in einem Worker-Pool gruppierten virtuellen Workerknoten mit zwei CPUs, 4 GB Speicher und einer einzelnen 100-GB-SAN-Platte, die für die Apps zur Verfügung steht. Wenn Sie einen Standardcluster erstellen, können Sie zwischen physischen (Bare-Metal-Maschinen) oder virtuellen Maschinen sowie verschiedenen Maschinengrößen wählen.
*   **Verwalteter Master**: Der Workerknoten wird durch einen dedizierten und hoch verfügbaren {{site.data.keyword.IBM_notm}}-eigenen Kubernetes-Master zentral überwacht und verwaltet, der alle Kubernetes-Ressourcen im Cluster steuert und überwacht. Sie können sich auf Ihren Workerknoten und die darin bereitgestellten Apps konzentrieren, ohne sich Gedanken über die Verwaltung des Masters zu machen.
*   **Infrastrukturressourcen**: Die für die Ausführung des Clusters erforderlichen Ressourcen, z. B. VLANs und IP-Adressen, werden in einem {{site.data.keyword.IBM_notm}}-eigenen Konto der IBM Cloud-Infrastruktur (SoftLayer) verwaltet. Wenn Sie einen Standardcluster erstellen, verwalten Sie diese Ressourcen in Ihrem eigenen Konto der IBM Cloud-Infrastruktur (SoftLayer). Weitere Informationen zu diesen Ressourcen und den [erforderlichen Berechtigungen](/docs/containers?topic=containers-users#infra_access) erhalten Sie, wenn Sie einen Standardcluster erstellen.
*   **Weitere Optionen:** Kostenlose Cluster werden innerhalb der von Ihnen ausgewählten Region bereitgestellt, die Zone können Sie allerdings nicht wählen. Erstellen Sie einen Standardcluster, um Kontrolle über Zone, Vernetzung und persistenten Speicher zu haben. [Weitere Informationen zu den Vorteilen von kostenlosen und Standardclustern](/docs/containers?topic=containers-cs_ov#cluster_types).

<br>

**Womit möchten Sie fortfahren?**</br>
Probieren Sie verschiedene Dinge mit dem kostenlosen Cluster aus, bevor dieser abläuft.

* Arbeiten Sie das [erste {{site.data.keyword.containerlong_notm}}-Lernprogramm](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial) durch, um einen Kubernetes-Cluster zu erstellen, die CLI zu installieren oder das Kubernetes-Terminal zu verwenden, eine private Registry zu erstellen, Ihre Clusterumgebung einzurichten oder Ihrem Cluster einen Service hinzuzufügen.
* Behalten Sie Ihren Schwung mit dem [zweiten {{site.data.keyword.containerlong_notm}}-Lernprogramm](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial) über die Bereitstellung von Apps im Cluster bei.
* [Erstellen Sie einen Standardcluster](/docs/containers?topic=containers-clusters#clusters_ui) mit mehreren Knoten für eine höhere Verfügbarkeit.


