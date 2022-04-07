---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-09"

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


# Lernprogramm: Kubernetes-Cluster erstellen
{: #cs_cluster_tutorial}

Mit diesem Lernprogramm können Sie einen Kubernetes-Cluster in {{site.data.keyword.containerlong}} bereitstellen und verwalten. Hier finden Sie Informationen dazu, wie die Bereitstellung, der Betrieb, das Skalieren und die Überwachung von containerisierten Apps in einem Cluster automatisiert werden kann.
{:shortdesc}

In der vorliegenden Reihe von Lernprogrammen werden Sie erfahren, wie eine fiktive Public-Relations-Firma Kubernetes-Funktionalität verwendet, um eine containerisierte App in {{site.data.keyword.Bluemix_notm}} bereitzustellen. Diese PR-Firma nutzt {{site.data.keyword.toneanalyzerfull}} zur Analyse ihrer Pressemitteilungen und zum Empfang von Feedback.


## Ziele
{: #tutorials_objectives}

Im ersten Lernprogramm fungieren Sie als Netzadministrator der PR-Firma. Sie konfigurieren einen angepassten Kubernetes-Cluster, der zur Bereitstellung und zum Testen einer Hello World-Version der App in {{site.data.keyword.containerlong_notm}} verwendet wird.
{:shortdesc}

-   Erstellen Sie einen Cluster mit einem Worker-Pool, der einen (1) Workerknoten hat.
-   Installieren Sie die Befehlszeilenschnittstellen zum Ausführen von [Kubernetes-Befehlen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubectl.docs.kubernetes.io/) und zum Verwalten von Docker-Images in {{site.data.keyword.registrylong_notm}}.
-   Erstellen Sie ein privates Image-Repository in {{site.data.keyword.registrylong_notm}} zum Speichern der Images.
-   Fügen Sie den {{site.data.keyword.toneanalyzershort}}-Service zum Cluster hinzu, sodass alle Apps im Cluster diesen Service verwenden können.


## Erforderlicher Zeitaufwand
{: #tutorials_time}

40 Minuten


## Zielgruppe
{: #tutorials_audience}

Dieses Lernprogramm ist für Softwareentwickler und Netzadministratoren konzipiert, die zum ersten Mal einen Kubernetes-Cluster erstellen.
{: shortdesc}

## Voraussetzungen
{: #tutorials_prereqs}

-  Überprüfen Sie die Schritte, die Sie zur [Vorbereitung der Erstellung eines Clusters](/docs/containers?topic=containers-clusters#cluster_prepare) ausführen müssen.
-  Stellen Sie sicher, dass Sie über die folgenden Zugriffsrichtlinien verfügen:
    - Die [{{site.data.keyword.Bluemix_notm}} IAM-Plattformrolle **Administrator**](/docs/containers?topic=containers-users#platform) für {{site.data.keyword.containerlong_notm}}
    - Die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Schreibberechtigter** oder **Manager**](/docs/containers?topic=containers-users#platform) für {{site.data.keyword.containerlong_notm}}


## Lerneinheit 1: Cluster erstellen und CLI einrichten
{: #cs_cluster_tutorial_lesson1}

Erstellen Sie Ihren Cluster in der {{site.data.keyword.Bluemix_notm}}-Konsole und installieren Sie die erforderlichen CLIs.
{: shortdesc}

**Gehen Sie wie folgt vor, um den Cluster zu erstellen:**

Da die Bereitstellung des Clusters einige Minuten dauern kann, erstellen Sie den Cluster, bevor Sie die CLIs installieren.

1.  [Erstellen Sie in der {{site.data.keyword.Bluemix_notm}}-Konsole ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/kubernetes/catalog/cluster/create) einen kostenlosen oder einen Standardcluster mit einem (1) Worker-Pool, in dem sich ein (1) Workerknoten befindet.

    Sie können auch einen [Cluster in der CLI](/docs/containers?topic=containers-clusters#clusters_cli) erstellen.
    {: tip}

Installieren Sie bei der Bereitstellung des Clusters die folgenden CLIs, die für die Verwaltung von Clustern verwendet werden:
-   {{site.data.keyword.Bluemix_notm}}-CLI
-   {{site.data.keyword.containerlong_notm}}-Plug-in
-   Kubernetes-CLI
-   {{site.data.keyword.registryshort_notm}}-Plug-in

Wenn Sie stattdessen nach der Erstellung des Clusters die {{site.data.keyword.Bluemix_notm}}-Konsole verwenden möchten, können Sie CLI-Befehle direkt über Ihren Web-Browser im [Kubernetes-Terminal](/docs/containers?topic=containers-cs_cli_install#cli_web) ausführen.
{: tip}

</br>
**Gehen Sie wie folgt vor, um die CLIs (Befehlszeilenschnittstellen) und die zugehörigen Voraussetzungen zu installieren:**

1. Installieren Sie die [{{site.data.keyword.Bluemix_notm}}-CLI ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli?topic=cloud-cli-ibmcloud-cli#idt-prereq). Zu dieser Installation gehören:
  - Die {{site.data.keyword.Bluemix_notm}}-Basis-CLI. Das Präfix zum Ausführen von Befehlen über die {{site.data.keyword.Bluemix_notm}}-CLI lautet `ibmcloud`.
  - Das {{site.data.keyword.containerlong_notm}}-Plug-in. Das Präfix zum Ausführen von Befehlen über die {{site.data.keyword.Bluemix_notm}}-CLI lautet `ibmcloud ks`.
  - Das {{site.data.keyword.registryshort_notm}}-Plug-in. Mit diesem Plug-in können Sie ein privates Image-Repository in {{site.data.keyword.registryshort_notm}} einrichten und verwalten. Das Präfix für die Ausführung von Registry-Befehlen ist `ibmcloud cr`.

2. Melden Sie sich an der {{site.data.keyword.Bluemix_notm}}-CLI an. Geben Sie Ihre
{{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wenn
Sie dazu aufgefordert werden.
  ```
  ibmcloud login
  ```
  {: pre}

  Wenn Sie über eine eingebundene ID verfügen, dann geben Sie das Flag `--sso` an, um sich anzumelden. Geben Sie Ihren Benutzernamen ein und verwenden Sie die bereitgestellte URL in Ihrer CLI-Ausgabe, um Ihren einmaligen Kenncode abzurufen.
  {: tip}

3. Folgen Sie den Eingabeaufforderungen, um ein Konto auszuwählen.

5. Stellen Sie sicher, dass die Plug-ins ordnungsgemäß installiert wurden.
  ```
  ibmcloud plugin list
  ```
  {: pre}

  Das {{site.data.keyword.containerlong_notm}}-Plug-in wird in den Ergebnissen mit **container-service**, das {{site.data.keyword.registryshort_notm}}-Plug-in mit **container-registry** angezeigt.

6. Um die Apps in Ihren Clustern bereitzustellen, müssen Sie die [Kubernetes-CLI installieren ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). Das Präfix zum Ausführen von Befehlen über die Kubernetes-CLI lautet `kubectl`.

  1. Laden Sie die Kubernetes-CLI-Version `major.minor` herunter, die mit der Version des Kubernetes-Clusters `major.minor` übereinstimmt, die Sie verwenden möchten. Die aktuelle Kubernetes-Standardversion für {{site.data.keyword.containerlong_notm}} ist Version 1.12.7.
    - **OS X**: [https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/darwin/amd64/kubectl ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/darwin/amd64/kubectl)
    - **Linux**: [https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/linux/amd64/kubectl ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/linux/amd64/kubectl)
    - **Windows**: [https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/windows/amd64/kubectl.exe ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/windows/amd64/kubectl.exe)

  2. Wenn Sie mit OS X oder Linux arbeiten, dann führen Sie die folgenden Schritte aus.

    1. Verschieben Sie die ausführbare Datei in das Verzeichnis `/usr/local/bin`.
      ```
      mv /filepath/kubectl /usr/local/bin/kubectl
      ```
      {: pre}

    2. Stellen Sie sicher, dass `/usr/local/bin` in der Variablen `PATH` Ihres Systems aufgelistet wird. Die Variable `PATH` enthält alle Verzeichnisse, in denen Ihr Betriebssystem ausführbare Dateien finden kann. Die Verzeichnisse, die in der Variablen `PATH` aufgelistet sind, erfüllen jedoch eine andere Funktion. Im Verzeichnis `/usr/local/bin` werden ausführbare Dateien für Software gespeichert, die nicht Bestandteil des Betriebssystem ist und vom Systemadministrator manuell installiert wurde.
      ```
      echo $PATH
      ```
      {: pre}
      CLI-Beispielausgabe:
      ```
      /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
      ```
      {: screen}

    3. Konvertieren Sie die Datei in eine ausführbare Datei.
      ```
      chmod +x /usr/local/bin/kubectl
      ```
      {: pre}

Gute Arbeit! Sie haben die CLIs für die folgenden Lerneinheiten und Lernprogramme erfolgreich installiert. Als Nächstes werden Sie die Clusterumgebung einrichten und den {{site.data.keyword.toneanalyzershort}}-Service hinzufügen.


## Lerneinheit 2: Private Registry einrichten
{: #cs_cluster_tutorial_lesson2}

Richten Sie in {{site.data.keyword.registryshort_notm}} ein privates Image-Repository ein und fügen Sie Ihrem Cluster geheime Schlüssel hinzu, sodass die App auf den {{site.data.keyword.toneanalyzershort}}-Service zugreifen kann.
{: shortdesc}

1.  Wenn sich Ihr Cluster in einer anderen Ressourcengruppe als `default` befindet, geben Sie diese Ressourcengruppe als Ziel an. Um die Ressourcengruppen anzuzeigen, zu denen die einzelnen Cluster gehören, führen Sie `ibmcloud ks clusters` aus.
   ```
   ibmcloud target -g <ressourcengruppenname>
   ```
   {: pre}

2.  Richten Sie Ihr eigenes privates Image-Repository in {{site.data.keyword.registryshort_notm}} ein, um Docker-Images sicher zu speichern und mit allen Benutzern des Clusters gemeinsam nutzen zu können. In {{site.data.keyword.Bluemix_notm}} ist ein privates Image-Repository durch einen Namensbereich gekennzeichnet. Der Namensbereich dient der Erstellung einer eindeutigen URL zu Ihrem Image-Repository, das Entwickler für den Zugriff auf private Docker-Images verwenden können.

    Erfahren Sie mehr über das [Sichern der persönlichen Daten](/docs/containers?topic=containers-security#pi) bei der Arbeit mit Container-Images.

    Im vorliegenden Beispiel will das PR-Unternehmen lediglich ein Image-Repository in {{site.data.keyword.registryshort_notm}} erstellen und wählt daher `pr-unternehmen` als Namensbereich aus, um alle Images im Konto zu gruppieren. Ersetzen Sie &lt;namensbereich&gt; durch einen Namensbereich Ihrer Wahl, der sich nicht auf das Lernprogramm bezieht.

    ```
    ibmcloud cr namespace-add <namensbereich>
    ```
    {: pre}

3.  Vergewissern Sie sich zuerst, dass die Bereitstellung Ihres Workerknotens abgeschlossen ist, bevor Sie mit dem nächsten Schritt fortfahren.

    ```
    ibmcloud ks workers --cluster <clustername_oder_-id>
    ```
    {: pre}

    Wenn die Bereitstellung Ihres Workerknotens abgeschlossen ist, wechselt der Status zu **Bereit**. Sie können nun mit dem Binden von {{site.data.keyword.Bluemix_notm}}-Services beginnen.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Zone   Version
    kube-mil01-pafe24f557f070463caf9e31ecf2d96625-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   free           normal   Ready    mil01      1.12.7
    ```
    {: screen}

## Lerneinheit 3: Clusterumgebung einrichten
{: #cs_cluster_tutorial_lesson3}

Legen Sie in Ihrer CLI (Befehlszeilenschnittstelle) den Kontext für Ihren Kubernetes-Cluster fest.
{: shortdesc}

Jedes Mal, wenn Sie sich an der {{site.data.keyword.containerlong}}-CLI anmelden, um mit Clustern zu arbeiten, müssen Sie diese Befehle ausführen, um den Pfad zu der Konfigurationsdatei des Clusters als Sitzungsvariable festzulegen. Anhand dieser Variablen sucht die Kubernetes-CLI eine lokale Konfigurationsdatei und Zertifikate, die zum Herstellen der Verbindung zum Cluster in {{site.data.keyword.Bluemix_notm}} erforderlich sind.

1.  Ermitteln Sie den Befehl zum Festlegen der Umgebungsvariablen und laden Sie die Kubernetes-Konfigurationsdateien herunter.

    ```
    ibmcloud ks cluster-config --cluster <clustername_oder_-id>
    ```
    {: pre}

    Wenn der Download der Konfigurationsdateien abgeschlossen ist, wird ein Befehl angezeigt, den Sie verwenden können, um den Pfad zu der lokalen Kubernetes-Konfigurationsdatei als Umgebungsvariable festzulegen.

    Beispiel für OS X:

    ```
    export KUBECONFIG=/Users/<benutzername>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

2.  Kopieren Sie den Befehl, der in Ihrem Terminal angezeigt wird, um die Umgebungsvariable `KUBECONFIG` festzulegen.

    **Windows PowerShell-Benutzer:** Anstatt den `SET`-Befehl aus der Ausgabe des Befehls `ibmcloud ks cluster-config` zu kopieren und einzufügen, müssen Sie die Umgebungsvariable `KUBECONFIG` festlegen, indem Sie zum Beispiel den folgenden Befehl ausführen: `$env:KUBECONFIG = "C:\Users\<benutzername>\.bluemix\plugins\container-service\clusters\mycluster\kube-config-prod-dal10-mycluster.yml"`.
    {: note}

3.  Stellen Sie sicher, dass die Umgebungsvariable `KUBECONFIG` richtig eingestellt ist.

    Beispiel für OS X:

    ```
    echo $KUBECONFIG
    ```
    {: pre}

    Ausgabe:

    ```
    /Users/<benutzername>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

4.  Stellen Sie sicher, dass die `kubectl`-Befehle mit Ihrem Cluster ordnungsgemäß ausgeführt werden. Überprüfen Sie dazu die Serverversion der Kubernetes-CLI wie folgt.

    ```
    kubectl version  --short
    ```
    {: pre}

    Beispielausgabe:

    ```
    Client Version: v1.12.7
    Server Version: v1.12.7
    ```
    {: screen}

## Lerneinheit 4: Einen Service zum Cluster hinzufügen
{: #cs_cluster_tutorial_lesson4}

Mit den {{site.data.keyword.Bluemix_notm}}-Services können Sie bereits entwickelte Funktionen in Ihren Apps nutzen. Alle an den Cluster gebundenen {{site.data.keyword.Bluemix_notm}}-Services können von allen Apps genutzt werden, die in diesem Cluster bereitgestellt sind. Wiederholen Sie die folgenden Schritte für jeden {{site.data.keyword.Bluemix_notm}}-Service, den Sie in Verbindung mit Ihren Apps verwenden wollen.
{: shortdesc}

1.  Fügen Sie den {{site.data.keyword.toneanalyzershort}}-Service zu Ihrem {{site.data.keyword.Bluemix_notm}}-Konto hinzu. Ersetzen Sie <servicename> durch einen Namen für Ihre Serviceinstanz.

    Wenn Sie den {{site.data.keyword.toneanalyzershort}}-Service zu Ihrem Konto hinzufügen, wird eine Nachricht angezeigt, dass der Service nicht kostenlos ist. Wenn Sie Ihren API-Aufruf einschränken, entstehen im Rahmen dieses Lernprogramms keine Kosten durch den {{site.data.keyword.watson}}-Service. [Informieren Sie sich über die Preisinformationen für den {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}-Service ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/catalog/services/tone-analyzer).
    {: note}

    ```
    ibmcloud service create tone_analyzer standard <servicename>
    ```
    {: pre}

2.  Binden Sie die {{site.data.keyword.toneanalyzershort}}-Instanz an den Kubernetes-Standardnamensbereich (`default`) für den Cluster. Später können Sie Ihren eigenen Namensbereich erstellen, um Benutzerzugriff auf Kubernetes-Ressourcen zu verwalten. Aber für den Moment müssen Sie den Namensbereich `default` verwenden. Kubernetes-Namensbereiche unterscheiden sich von dem Registry-Namensbereich, den Sie davor erstellt haben.

    ```
    ibmcloud ks cluster-service-bind <clustername> default <servicename>
    ```
    {: pre}

    Ausgabe:

    ```
    ibmcloud ks cluster-service-bind pr_firm_cluster default mytoneanalyzer
    Binding service instance to namespace...
    OK
    Namespace:	default
    Secret name:	binding-mytoneanalyzer
    ```
    {: screen}

3.  Stellen Sie sicher, dass der geheime Kubernetes-Schlüssel im Namensbereich Ihres Clusters erstellt wurde. Jeder {{site.data.keyword.Bluemix_notm}}-Service ist durch eine JSON-Datei definiert, die vertrauliche Informationen enthält, wie zum Beispiel den {{site.data.keyword.Bluemix_notm}} IAM-API-Schlüssel (Identity and Access Management) und die URL, über die der Container Zugriff erhält. Kubernetes verwendet geheime Schlüssel, um diese Informationen sicher zu speichern. Im vorliegenden Beispiel enthält der geheime Schlüssel den API-Schlüssel für den Zugriff auf die {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}-Instanz, die in Ihrem Konto bereitgestellt wird.

    ```
    kubectl get secrets --namespace=default
    ```
    {: pre}

    Ausgabe:

    ```
    NAME                       TYPE                                  DATA      AGE
    binding-mytoneanalyzer     Opaque                                1         1m
    bluemix-default-secret     kubernetes.io/dockercfg               1         1h
    default-token-kf97z        kubernetes.io/service-account-token   3         1h
    ```
    {: screen}

</br>
Ganz hervorragend! Der Cluster ist konfiguriert und Ihre lokale Umgebung ist jetzt so vorbereitet, dass Sie mit der Bereitstellung von Apps im Cluster beginnen können.

## Womit möchten Sie fortfahren?
{: #tutorials_next}

* Testen Sie Ihr Wissen und [nehmen Sie an einem Quiz teil ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibmcloud-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php)!
* Machen Sie sich mit dem [Lernprogramm: Apps in Kubernetes-Clustern bereitstellen](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial) vertraut, um die App der PR-Firma in dem von Ihnen erstellten Cluster bereitzustellen.
