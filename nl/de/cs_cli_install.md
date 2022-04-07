---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-15"

keywords: kubernetes, iks, ibmcloud, ic, ks, kubectl

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




# CLI und API einrichten
{: #cs_cli_install}

Sie können die {{site.data.keyword.containerlong}}-CLI
oder -API zu Erstellen und Verwalten Ihrer Kubernetes-Cluster verwenden.
{:shortdesc}

## CLI installieren
{: #cs_cli_install_steps}

Installieren Sie die erforderlichen CLIs, um Ihre Kubernetes-Cluster in {{site.data.keyword.containerlong_notm}} zu erstellen und zu verwalten und um containerisierte Apps in Ihrem Cluster bereitzustellen.
{:shortdesc}

Diese Task enthält Informationen zur Installation dieser CLIs (Befehlszeilenschnittstellen) und Plug-ins:

- {{site.data.keyword.Bluemix_notm}}-CLI
- {{site.data.keyword.containerlong_notm}}-Plug-in
- {{site.data.keyword.registryshort_notm}}-Plug-in
- Kubernetes CLI-Version, die mit der Version `major.minor` Ihres Clusters übereinstimmt

Wenn Sie stattdessen nach der Erstellung des Clusters die {{site.data.keyword.Bluemix_notm}}-Konsole verwenden möchten, können Sie CLI-Befehle direkt über Ihren Web-Browser im [Kubernetes-Terminal](#cli_web) ausführen.
{: tip}

<br>
Gehen Sie wie folgt vor, um die Befehlszeilenschnittstellen (CLIs) zu installieren:

1. Installieren Sie die [{{site.data.keyword.Bluemix_notm}}-CLI ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli?topic=cloud-cli-ibmcloud-cli#idt-prereq). Zu dieser Installation gehören:
  - Die {{site.data.keyword.Bluemix_notm}}-Basis-CLI. Das Präfix zum Ausführen von Befehlen über die {{site.data.keyword.Bluemix_notm}}-CLI lautet `ibmcloud`.
  - Das {{site.data.keyword.containerlong_notm}}-Plug-in. Das Präfix zum Ausführen von Befehlen über die {{site.data.keyword.Bluemix_notm}}-CLI lautet `ibmcloud ks`.
  - Das {{site.data.keyword.registryshort_notm}}-Plug-in. Verwenden Sie dieses Plug-in zum Festlegen Ihres eigenen Namensbereichs in einer hoch verfügbaren, skalierbaren, privaten Multi-Tenant-Image-Registry, die von IBM gehostet wird, und zum Speichern und gemeinsamen Nutzen von Docker-Images mit anderen Benutzern. Docker-Images sind erforderlich, um Container in einem Cluster bereitzustellen. Das Präfix für die Ausführung von Registry-Befehlen ist `ibmcloud cr`.

  Beabsichtigen Sie, die CLI häufig einzusetzen? Dann probieren Sie aus, was im Abschnitt [Automatische Vervollständigung für die Shell für die {{site.data.keyword.Bluemix_notm}}-CLI aktivieren (nur Linux/MacOS)](/docs/cli/reference/ibmcloud?topic=cloud-cli-shell-autocomplete#shell-autocomplete-linux) beschrieben ist.
  {: tip}

2. Melden Sie sich an der {{site.data.keyword.Bluemix_notm}}-CLI an. Geben Sie Ihre
{{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wenn
Sie dazu aufgefordert werden.
  ```
  ibmcloud login
  ```
  {: pre}

  Falls Sie über eine föderierte ID verfügen, geben Sie `ibmcloud login --sso` ein, um sich an der Befehlszeilenschnittstelle von {{site.data.keyword.Bluemix_notm}} anzumelden. Geben Sie Ihren Benutzernamen ein und verwenden Sie die bereitgestellte URL in Ihrer CLI-Ausgabe, um Ihren einmaligen Kenncode abzurufen. Bei Verwendung einer föderierten ID schlägt die Anmeldung ohne die Option `--sso` fehl, mit der Option `--sso` ist sie erfolgreich.
  {: tip}

3. Stellen Sie sicher, dass die Plug-ins ordnungsgemäß installiert wurden.
  ```
  ibmcloud plugin list
  ```
  {: pre}

  Das {{site.data.keyword.containerlong_notm}}-Plug-in wird in den Ergebnissen mit **container-service**, das {{site.data.keyword.registryshort_notm}}-Plug-in mit **container-registry** angezeigt.

4. {: #kubectl}Um eine lokale Version des Kubernetes-Dashboards anzuzeigen und Apps in Ihren Clustern bereitzustellen, müssen Sie die [Kubernetes-CLI installieren ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). Das Präfix zum Ausführen von Befehlen über die Kubernetes-CLI lautet `kubectl`.

  Die aktuellste stabile Version von `kubectl` wird mit der {{site.data.keyword.Bluemix_notm}}-Basis-CLI installiert. Um jedoch mit Ihrem Cluster arbeiten zu können, müssen Sie stattdessen die Version `major.minor` der Kubernetes-CLI installieren, die mit der Version `major.minor` des Kubernetes-Clusters übereinstimmt, die Sie verwenden möchten. Wenn Sie eine `kubectl`-CLI-Version verwenden, die nicht wenigstens mit der Version `major.minor` Ihrer Cluster übereinstimmt, kann dies zu unerwarteten Ergebnissen führen. Stellen Sie sicher, dass Ihr Kubernetes-Cluster und die CLI-Versionen immer auf dem neuesten Stand sind.
  {: important}

  1. Laden Sie die Kubernetes-CLI-Version `major.minor` herunter, die mit der Version des Kubernetes-Clusters `major.minor` übereinstimmt, die Sie verwenden möchten. Die aktuelle Kubernetes-Standardversion für {{site.data.keyword.containerlong_notm}} ist Version 1.12.7.
    - **OS X**: [https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/darwin/amd64/kubectl ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/darwin/amd64/kubectl)
    - **Linux**: [https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/linux/amd64/kubectl ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/linux/amd64/kubectl)
    - **Windows**: Installieren Sie die Kubernetes-CLI im selben Verzeichnis wie die {{site.data.keyword.Bluemix_notm}}-CLI. Diese Konfiguration erspart Ihnen bei der späteren Ausführung von Befehlen einige Dateipfadänderungen. [https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/windows/amd64/kubectl.exe ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/windows/amd64/kubectl.exe)

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

Beginnen Sie anschließend damit, [Kubernetes-Cluster über die CLI mit {{site.data.keyword.containerlong_notm}} zu erstellen](/docs/containers?topic=containers-clusters#clusters_cli).

Referenzinformationen zu diesen CLIs finden Sie in der Dokumentation zu diesen Tools.

-   [`ibmcloud`-Befehle](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
-   [`ibmcloud ks`-Befehle](/docs/containers?topic=containers-cs_cli_reference#cs_cli_reference)
-   [`ibmcloud cr`-Befehle](/docs/services/Registry?topic=registry-registry_cli_reference#registry_cli_reference)
-   [`kubectl`-Befehle ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubectl.docs.kubernetes.io/)

<br />



## CLI in einem Container auf Ihrem Computer ausführen
{: #cs_cli_container}

Anstatt jede der CLIs einzeln auf Ihrem Computer zu installieren, können Sie die CLIs in einem Container installieren, der auf dem Computer ausgeführt wird.
{:shortdesc}

[Installieren Sie zunächst Docker ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.docker.com/community-edition#/download), um Images lokal zu erstellen und auszuführen. Wenn Sie Windows 8 oder älter verwenden, können Sie stattdessen [Docker Toolbox ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.docker.com/toolbox/toolbox_install_windows/) installieren.

1. Erstellen Sie ein Image von der bereitgestellten Dockerfile.

    ```
    docker build -t <imagename> https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/install-clis-container/Dockerfile
    ```
    {: pre}

2. Stellen Sie das Image lokal als Container bereit und hängen Sie ein Datenträger für den Zugriff auf lokale Dateien an.

    ```
    docker run -it -v /local/path:/container/volume <imagename>
    ```
    {: pre}

3. Beginnen Sie damit, die Befehle `ibmcloud ks` und `kubectl` über die interaktive Shell auszuführen. Wenn Sie Daten erstellen, die gespeichert werden sollen, speichern Sie diese auf dem angehängten Datenträger. Wenn Sie die Shell beenden, wird der Container gestoppt.

<br />



## CLI zur Ausführung von `kubectl` konfigurieren
{: #cs_cli_configure}

Sie können die Befehle, die mit der Kubernetes-Befehlszeilenschnittstelle zur Verfügung gestellt werden,
zum Verwalten von Clustern in {{site.data.keyword.Bluemix_notm}} verwenden.
{:shortdesc}

Alle in Kubernetes 1.12.7 verfügbaren `kubectl`-Befehle werden für die Verwendung mit Clustern in {{site.data.keyword.Bluemix_notm}} unterstützt. Wenn Sie einen Cluster erstellt haben, müssen Sie den Kontext für Ihre lokale Befehlszeilenschnittstelle (CLI) mit einer Umgebungsvariablen auf diesen Cluster festlegen. Anschließend können Sie die Kubernetes-`kubectl`-Befehle verwenden, um in {{site.data.keyword.Bluemix_notm}} mit Ihrem Cluster zu arbeiten.

Bevor Sie `kubectl`-Befehle ausführen können, müssen Sie Folgendes tun:
* [Installieren Sie die erforderlichen Befehlszeilenschnittellen](#cs_cli_install).
* [Erstellen Sie einen Cluster](/docs/containers?topic=containers-clusters#clusters_cli).
* Stellen Sie sicher, dass Sie über eine [Servicerolle](/docs/containers?topic=containers-users#platform) verfügen, die Ihnen die entsprechende RBAC-Rolle für Kubernetes erteilt, um mit Kubernetes-Ressourcen arbeiten zu können. Wenn Sie nur eine Servicerolle, jedoch keine Plattformrolle haben, muss Ihnen der Clusteradministrator den Namen und die ID des Clusters geben oder die Rolle **Anzeigeberechtigter** erteilen, damit Sie Cluster auflisten können.

Gehen Sie wie folgt vor, um `kubectl`-Befehle zu verwenden:

1.  Melden Sie sich an der {{site.data.keyword.Bluemix_notm}}-CLI an. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wenn Sie dazu aufgefordert werden. Zur Angabe einer {{site.data.keyword.Bluemix_notm}}-Region müssen Sie den [API-Endpunkt einschließen](/docs/containers?topic=containers-regions-and-zones#bluemix_regions).

    ```
    ibmcloud login
    ```
    {: pre}

    Falls Sie über eine föderierte ID verfügen, geben Sie `ibmcloud login --sso` ein, um sich an der Befehlszeilenschnittstelle von {{site.data.keyword.Bluemix_notm}} anzumelden. Geben Sie Ihren Benutzernamen ein und verwenden Sie die bereitgestellte URL in Ihrer CLI-Ausgabe, um Ihren einmaligen Kenncode abzurufen. Bei Verwendung einer föderierten ID schlägt die Anmeldung ohne die Option `--sso` fehl, mit der Option `--sso` ist sie erfolgreich.
    {: tip}

2.  Wählen Sie ein {{site.data.keyword.Bluemix_notm}}-Konto aus. Wenn Sie mehreren {{site.data.keyword.Bluemix_notm}}-Organisationen zugeordnet sind, wählen Sie die Organisation aus, in der der Cluster erstellt wurde. Cluster sind für eine Organisation spezifisch, jedoch von einem {{site.data.keyword.Bluemix_notm}}-Bereich unabhängig. Daher ist es nicht erforderlich, einen Bereich auszuwählen.

3.  Wenn Sie Cluster erstellen und mit ihnen in einer Ressourcengruppe arbeiten möchten, die nicht mit der Standardressourcengruppe identisch ist, geben Sie die entsprechende Ressourcengruppe als Ziel an. Um die Ressourcengruppen anzuzeigen, zu denen die einzelnen Cluster gehören, führen Sie `ibmcloud ks clusters` aus. **Hinweis:** Sie müssen über die [Zugriffsrechte eines **Anzeigeberechtigten**](/docs/containers?topic=containers-users#platform) für die Ressourcengruppe verfügen.
    ```
    ibmcloud target -g <ressourcengruppenname>
    ```
    {: pre}

4.  Um Kubernetes-Cluster in einer anderen als der zuvor ausgewählten {{site.data.keyword.Bluemix_notm}}-Region zu erstellen, wählen Sie die Region als Ziel aus.
    ```
    ibmcloud ks region-set
    ```
    {: pre}

5.  Listen Sie alle Cluster im Konto auf, um den Namen des Clusters zu ermitteln. Wenn Sie nur eine {{site.data.keyword.Bluemix_notm}} IAM-Servicerolle haben und Cluster nicht anzeigen können, bitten Sie Ihren Clusteradministrator um die IAM-Plattformrolle **Anzeigeberechtigter** oder um den Namen und die ID des Clusters.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

6.  Legen Sie den von Ihnen erstellten Cluster als Kontext für diese Sitzung fest. Führen Sie diese Konfigurationsschritte jedes Mal aus, wenn Sie mit Ihrem Cluster arbeiten.
    1.  Ermitteln Sie den Befehl zum Festlegen der Umgebungsvariablen und laden Sie die Kubernetes-Konfigurationsdateien herunter.

        ```
        ibmcloud ks cluster-config <clustername_oder_-id>
        ```
        {: pre}

        Wenn der Download der Konfigurationsdateien abgeschlossen ist, wird ein Befehl angezeigt, den Sie verwenden können, um den Pfad zu der lokalen Kubernetes-Konfigurationsdatei als Umgebungsvariable festzulegen.

        Beispiel:

        ```
        export KUBECONFIG=/Users/<benutzername>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  Kopieren Sie den Befehl, der in Ihrem Terminal angezeigt wird, um die Umgebungsvariable `KUBECONFIG` festzulegen.

        **Mac- oder Linux-Benutzer:** Anstatt den Befehl `ibmcloud ks cluster-config` auszuführen und die Umgebungsvariable `KUBECONFIG` zu kopieren, können Sie den Befehl `ibmcloud ks cluster-config --export <clustername>` ausführen. Abhängig von Ihrer Shell können Sie diese durch Ausführen des Befehls `eval $(ibmcloud ks cluster-config --export <clustername>)` einrichten.
        {: tip}

        **Windows PowerShell-Benutzer:** Anstatt den `SET`-Befehl aus der Ausgabe des Befehls `ibmcloud ks cluster-config` zu kopieren und einzufügen, müssen Sie die Umgebungsvariable `KUBECONFIG` festlegen, indem Sie zum Beispiel den folgenden Befehl ausführen: `$env:KUBECONFIG = "C:\Users\<benutzername>\.bluemix\plugins\container-service\clusters\mycluster\kube-config-prod-dal10-mycluster.yml"`.
        {:tip}

    3.  Stellen Sie sicher, dass die Umgebungsvariable `KUBECONFIG` richtig eingestellt ist.

        Beispiel:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Ausgabe:
        ```
        /Users/<benutzername>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

7.  Stellen Sie sicher, dass die `kubectl`-Befehle mit Ihrem Cluster ordnungsgemäß ausgeführt werden. Überprüfen Sie dazu die Serverversion der Kubernetes-CLI wie folgt.

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

Nun können Sie `kubectl`-Befehle zum Verwalten Ihrer Cluster in {{site.data.keyword.Bluemix_notm}} ausführen. Eine vollständige Liste von Befehlen finden Sie in der [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubectl.docs.kubernetes.io/).

**Tipp:** Wenn Sie Windows verwenden und die Kubernetes-CLI nicht in demselben Verzeichnis wie die {{site.data.keyword.Bluemix_notm}}-CLI installiert ist, müssen Sie die Verzeichnisse zu dem Pfad wechseln, in dem die Kubernetes-CLI installiert ist, um `kubectl`-Befehle erfolgreich ausführen zu können..


<br />


## CLI aktualisieren
{: #cs_cli_upgrade}

Es kann sinnvoll sein, die CLIs regelmäßig zu aktualisieren, um neue Funktionen verwenden zu können.
{:shortdesc}

Diese Task enthält Informationen zur Aktualisierung dieser CLIs (Befehlszeilenschnittstellen).

-   {{site.data.keyword.Bluemix_notm}}-CLI Version 0.8.0 oder höher
-   {{site.data.keyword.containerlong_notm}}-Plug-in
-   Kubernetes-CLI Version 1.12.7 oder höher
-   {{site.data.keyword.registryshort_notm}}-Plug-in

<br>
Gehen Sie wie folgt vor, um die Befehlszeilenschnittstellen (CLIs) zu aktualisieren:

1.  Aktualisieren Sie die {{site.data.keyword.Bluemix_notm}}-CLI. Laden Sie die [neueste Version ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli?topic=cloud-cli-ibmcloud-cli) herunter und führen Sie das Installationsprogramm aus.

2. Melden Sie sich an der {{site.data.keyword.Bluemix_notm}}-CLI an. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wenn Sie dazu aufgefordert werden. Zur Angabe einer {{site.data.keyword.Bluemix_notm}}-Region müssen Sie den [API-Endpunkt einschließen](/docs/containers?topic=containers-regions-and-zones#bluemix_regions).

    ```
    ibmcloud login
    ```
    {: pre}

     Falls Sie über eine föderierte ID verfügen, geben Sie `ibmcloud login --sso` ein, um sich an der Befehlszeilenschnittstelle von {{site.data.keyword.Bluemix_notm}} anzumelden. Geben Sie Ihren Benutzernamen ein und verwenden Sie die bereitgestellte URL in Ihrer CLI-Ausgabe, um Ihren einmaligen Kenncode abzurufen. Bei Verwendung einer föderierten ID schlägt die Anmeldung ohne die Option `--sso` fehl, mit der Option `--sso` ist sie erfolgreich.
     {: tip}

3.  Aktualisieren Sie das {{site.data.keyword.containerlong_notm}}-Plug-in.
    1.  Installieren Sie das Update aus dem {{site.data.keyword.Bluemix_notm}}-Plug-in-Repository.

        ```
        ibmcloud plugin update container-service 
        ```
        {: pre}

    2.  Überprüfen Sie die Plug-in-Installation, indem Sie den folgenden Befehl ausführen und die Liste der installierten Plug-ins überprüfen.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        Das
{{site.data.keyword.containerlong_notm}}-Plug-in wird in den Ergebnissen als 'container-service' angezeigt.

    3.  Initialisieren Sie die CLI.

        ```
        ibmcloud ks init
        ```
        {: pre}

4.  [Aktualisieren Sie die Kubernetes-CLI](#kubectl).

5.  Aktualisieren Sie das {{site.data.keyword.registryshort_notm}}-Plug-in.
    1.  Installieren Sie das Update aus dem {{site.data.keyword.Bluemix_notm}}-Plug-in-Repository.

        ```
        ibmcloud plugin update container-registry 
        ```
        {: pre}

    2.  Überprüfen Sie die Plug-in-Installation, indem Sie den folgenden Befehl ausführen und die Liste der installierten Plug-ins überprüfen.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        Das Registry-Plug-in wird in den Ergebnissen unter dem Namen 'container-registry' angezeigt.

<br />


## CLI deinstallieren
{: #cs_cli_uninstall}

Wenn Sie die Befehlszeilenschnittstelle (CLI) nicht mehr benötigen, können Sie sie deinstallieren.
{:shortdesc}

Diese Task enthält die Informationen zum Entfernen dieser CLIs (Befehlszeilenschnittstellen):


-   {{site.data.keyword.containerlong_notm}}-Plug-in
-   Kubernetes-CLI
-   {{site.data.keyword.registryshort_notm}}-Plug-in

Gehen Sie wie folgt vor, um die Befehlszeilenschnittstellen (CLIs) zu deinstallieren:

1.  Deinstallieren Sie das {{site.data.keyword.containerlong_notm}}-Plug-in.

    ```
    ibmcloud plugin uninstall container-service
    ```
    {: pre}

2.  Deinstallieren Sie das {{site.data.keyword.registryshort_notm}}-Plug-in.

    ```
    ibmcloud plugin uninstall container-registry
    ```
    {: pre}

3.  Stellen Sie sicher, dass die Plug-ins auch tatsächlich deinstalliert wurden, indem Sie den folgenden Befehl ausführen und die Liste der installierten Plug-ins überprüfen.

    ```
    ibmcloud plugin list
    ```
    {: pre}

    Die Plug-ins 'container-service' und 'container-registry' werden in den Ergebnissen nicht angezeigt.

<br />


## Kubernetes-Terminal in Ihrem Web-Browser verwenden (Beta)
{: #cli_web}

Das Kubernetes-Terminal ermöglicht Ihnen die Verwendung der {{site.data.keyword.Bluemix_notm}}-CLI, um Ihren Cluster direkt über Ihren Web-Browser zu verwalten.
{: shortdesc}

Das Kubernetes-Terminal wird als Beta-{{site.data.keyword.containerlong_notm}}-Add-on freigegeben und kann sich aufgrund von Benutzerfeedback und weiteren Tests noch ändern. Verwenden Sie diese Funktion nicht in Produktionsclustern, um unerwartete Nebeneffekte zu vermeiden.
{: important}

Wenn Sie Ihre Cluster mit dem Cluster-Dashboard in der {{site.data.keyword.Bluemix_notm}}-Konsole verwalten, aber recht bald zu fortgeschritteneren Konfigurationsänderungen übergehen möchten, können Sie CLI-Befehle jetzt direkt über Ihren Webbrowser im Kubernetes-Terminal ausführen. Das Kubernetes-Terminal wird mit der [{{site.data.keyword.Bluemix_notm}}-Basis-CLI ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli?topic=cloud-cli-ibmcloud-cli), dem {{site.data.keyword.containerlong_notm}}-Plug-in und dem {{site.data.keyword.registryshort_notm}}-Plug-in aktiviert. Zusätzlich ist der Terminal-Kontext bereits auf den Cluster festgelegt, mit dem Sie arbeiten. Sie können also Kubernetes-`kubectl`-Befehle ausführen, um mit Ihrem Cluster zu arbeiten.

Alle Dateien, die Sie lokal herunterladen und bearbeiten, wie z. B. YAML-Dateien, werden vorübergehend im Kubernetes-Terminal gespeichert und bleiben nicht über Sitzungen hinweg bestehen.
{: note}

Gehen Sie wie folgt vor, um das Kubernetes-Terminal zu installieren und zu starten:

1.  Melden Sie sich bei der [{{site.data.keyword.Bluemix_notm}}-Konsole](https://cloud.ibm.com/) an.
2.  Wählen Sie in der Menüleiste das Konto aus, das Sie verwenden möchten.
3.  Klicken Sie im Menü ![Menüsymbol](../icons/icon_hamburger.svg "Menüsymbol") auf **Kubernetes**.
4.  Klicken Sie auf der Seite **Cluster** auf den Cluster, auf den Sie zugreifen möchten.
5.  Klicken Sie auf der Seite mit den Clusterdetails auf die Schaltfläche **Terminal**.
6.  Klicken Sie auf **Installieren**. Es kann einige Minuten dauern, bis das Terminal-Add-on installiert ist.
7.  Klicken Sie nochmals auf die Schaltfläche **Terminal**. Das Terminal wird in Ihrem Browser geöffnet.

Beim nächsten Mal können Sie das Kubernetes-Terminal einfach durch Klicken auf die Schaltfläche **Terminal** starten.

<br />


## Clusterbereitstellungen mit der API automatisieren
{: #cs_api}

Mithilfe der {{site.data.keyword.containerlong_notm}}-API können Sie die Erstellung, Bereitstellung und Verwaltung Ihrer Kubernetes-Cluster automatisieren.
{:shortdesc}

Die {{site.data.keyword.containerlong_notm}}-API benötigt Headerinformationen, die Sie in Ihrer API-Anforderung bereitstellen müssen. Diese können je nach verwendeter API variieren. Um zu bestimmen, welche Headerinformationen für Ihre API benötigt werden, finden Sie weitere Informationen in der [{{site.data.keyword.containerlong_notm}}-API-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://us-south.containers.cloud.ibm.com/swagger-api).

Zur Authentifizierung bei {{site.data.keyword.containerlong_notm}} müssen Sie ein {{site.data.keyword.Bluemix_notm}} IAM-Token (IAM = Identity and Access Management) angeben, das mit Ihren {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweisen generiert wurde und das die {{site.data.keyword.Bluemix_notm}}-Konto-ID umfasst, unter der der Cluster erstellt wurde. Abhängig von der Art und Weise, in der Sie die Authentifizierung bei {{site.data.keyword.Bluemix_notm}} durchführen, können Sie zwischen den folgenden Optionen zur Automatisierung der Erstellung Ihres {{site.data.keyword.Bluemix_notm}} IAM-Tokens wählen.

Sie können auch die [swagger.json-Datei für APIs ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://containers.cloud.ibm.com/swagger-api-json) verwenden, um einen Client zu generieren, der mit der API als Teil Ihrer Automatisierungsarbeit interagieren kann.
{: tip}

<table summary="ID-Typen und Optionen mit dem Eingabeparameter in Spalte 1 und dem Wert in Spalte 2.">
<caption>ID-Typen und Optionen</caption>
<thead>
<th>{{site.data.keyword.Bluemix_notm}}-ID</th>
<th>Meine Optionen</th>
</thead>
<tbody>
<tr>
<td>Nicht föderierte ID</td>
<td><ul><li><strong>{{site.data.keyword.Bluemix_notm}}-Benutzername und -Kennwort:</strong> Sie können die Schritte im vorliegenden Abschnitt ausführen, um die Erstellung des {{site.data.keyword.Bluemix_notm}} IAM-Zugriffstokens vollständig zu automatisieren.</li>
<li><strong>{{site.data.keyword.Bluemix_notm}}-API-Schlüssel generieren:</strong> Alternativ zur Verwendung des {{site.data.keyword.Bluemix_notm}}-Benutzernamens und des zugehörigen Kennworts können Sie <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">auch {{site.data.keyword.Bluemix_notm}}-API-Schlüssel verwenden</a>. {{site.data.keyword.Bluemix_notm}}-API-Schlüssel sind von dem {{site.data.keyword.Bluemix_notm}}-Konto abhängig, für das sie generiert wurden. Sie können Ihren {{site.data.keyword.Bluemix_notm}}-API-Schlüssel nicht mit einer anderen Konto-ID in demselben {{site.data.keyword.Bluemix_notm}} IAM-Token kombinieren. Um auf Cluster zugreifen zu können, die mit einem anderen Konto als dem Konto erstellt wurden, auf dem der {{site.data.keyword.Bluemix_notm}}-API-Schlüssel basiert, müssen Sie sich bei dem Konto anmelden, um einen neuen API-Schlüssel zu generieren. </li></ul></tr>
<tr>
<td>Föderierte ID</td>
<td><ul><li><strong>{{site.data.keyword.Bluemix_notm}}-API-Schlüssel generieren:</strong> <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">{{site.data.keyword.Bluemix_notm}}-API-Schlüssel</a> sind von dem {{site.data.keyword.Bluemix_notm}}-Konto abhängig, für das sie generiert wurden. Sie können Ihren {{site.data.keyword.Bluemix_notm}}-API-Schlüssel nicht mit einer anderen Konto-ID in demselben {{site.data.keyword.Bluemix_notm}} IAM-Token kombinieren. Um auf Cluster zugreifen zu können, die mit einem anderen Konto als dem Konto erstellt wurden, auf dem der {{site.data.keyword.Bluemix_notm}}-API-Schlüssel basiert, müssen Sie sich bei dem Konto anmelden, um einen neuen API-Schlüssel zu generieren. </li><li><strong>Einmaligen Kenncode verwenden:</strong> Wenn Sie sich bei {{site.data.keyword.Bluemix_notm}} authentifizieren, indem Sie einen einmaligen Kenncode verwenden, dann können Sie die Erstellung Ihres {{site.data.keyword.Bluemix_notm}} IAM-Tokens nicht vollständig automatisieren, weil das Abrufen des einmaligen Kenncodes einen manuellen Eingriff beim Web-Browser erfordert. Um die Erstellung Ihres {{site.data.keyword.Bluemix_notm}} IAM-Tokens vollständig zu automatisieren, müssen Sie stattdessen einen {{site.data.keyword.Bluemix_notm}}-API-Schlüssel erstellen. </ul></td>
</tr>
</tbody>
</table>

1.  Erstellen Sie Ihr {{site.data.keyword.Bluemix_notm}} IAM-Zugriffstoken. Die Informationen des Hauptteils, der Bestandteil Ihrer Anforderung ist, kann abhängig von der {{site.data.keyword.Bluemix_notm}}-Authentifizierungsmethode, die verwendet wird, variieren.

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Eingabeparameter zum Abrufen von IAM-Tokens mit dem Eingabeparameter in Spalte 1 und dem Wert in Spalte 2.">
    <caption>Eingabeparameter zum Abrufen von IAM-Tokens.</caption>
    <thead>
        <th>Eingabeparameter</th>
        <th>Werte</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>Hinweis:</strong> <code>Yng6Yng=</code> entspricht der URL-codierten Autorisierung für den Benutzernamen <strong>bx</strong> und das Kennwort <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Hauptteil für {{site.data.keyword.Bluemix_notm}}-Benutzername und -Kennwort</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: Ihr {{site.data.keyword.Bluemix_notm}}-Benutzername.</li>
    <li>`password`: Ihr {{site.data.keyword.Bluemix_notm}}-Kennwort.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:`</br><strong>Hinweis</strong>: Fügen Sie den Schlüssel <code>uaa_client_secret</code> ohne angegebenen Wert hinzu.</li></ul></td>
    </tr>
    <tr>
    <td>Hauptteil für {{site.data.keyword.Bluemix_notm}}-API-Schlüssel</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: Ihr {{site.data.keyword.Bluemix_notm}}-API-Schlüssel.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Hinweis</strong>: Fügen Sie den Schlüssel <code>uaa_client_secret</code> ohne angegebenen Wert hinzu.</li></ul></td>
    </tr>
    <tr>
    <td>Hauptteil für einmaligen {{site.data.keyword.Bluemix_notm}}-Kenncode</td>
      <td><ul><li><code>grant_type: urn:ibm:params:oauth:grant-type:passcode</code></li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: Ihr einmaliger {{site.data.keyword.Bluemix_notm}}-Kenncode. Führen Sie den Befehl `ibmcloud login --sso` aus und befolgen Sie die Anweisungen in der CLI-Ausgabe, um den einmaligen Kenncode über Ihren Web-Browser abzurufen.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Hinweis</strong>: Fügen Sie den Schlüssel <code>uaa_client_secret</code> ohne angegebenen Wert hinzu.</li></ul>
    </td>
    </tr>
    </tbody>
    </table>

    Beispielausgabe:

    ```
    {
    "access_token": "<iam-zugriffstoken>",
    "refresh_token": "<iam-aktualisierungstoken>",
    "uaa_token": "<uaa-token>",
    "uaa_refresh_token": "<uaa-aktualisierungstoken>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    "scope": "ibm openid"
    }

    ```
    {: screen}

    Sie finden das {{site.data.keyword.Bluemix_notm}} IAM-Token im Feld **access_token** Ihrer API-Ausgabe. Notieren Sie das {{site.data.keyword.Bluemix_notm}} IAM-Token, um in den nächsten Schritten zusätzliche Headerinformationen abzurufen.

2.  Rufen Sie die ID des {{site.data.keyword.Bluemix_notm}}-Kontos ab, mit dem Sie arbeiten möchten. Ersetzen Sie `<iam_access_token>` durch das {{site.data.keyword.Bluemix_notm}} IAM-Token, das Sie im vorigen Schritt aus dem Feld **access_token** Ihrer API-Ausgabe abgerufen haben. In Ihrer API-Ausgabe können Sie die ID Ihres {{site.data.keyword.Bluemix_notm}}-Kontos im Feld **resources.metadata.guid** finden.

    ```
    GET https://accountmanagement.ng.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="Eingabeparameter zum Abrufen der {{site.data.keyword.Bluemix_notm}}-Konto-ID mit dem Eingabeparameter in Spalte 1 und dem Wert in Spalte 2.">
    <caption>Eingabeparameter zum Abrufen einer {{site.data.keyword.Bluemix_notm}}-Konto-ID.</caption>
    <thead>
  	<th>Eingabeparameter</th>
  	<th>Werte</th>
    </thead>
    <tbody>
  	<tr>
  		<td>Header</td>
      <td><ul><li><code>Content-Type: application/json</code></li>
        <li>`Authorization: bearer <iam_access_token>`</li>
        <li><code>Accept: application/json</code></li></ul></td>
  	</tr>
    </tbody>
    </table>

    Beispielausgabe:

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
            "guid": "<konto-id>",
            "url": "/v1/accounts/<konto-id>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

3.  Generieren Sie ein neues {{site.data.keyword.Bluemix_notm}} IAM-Token, das Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise und die Konto-ID enthält, mit der Sie arbeiten möchten.

    Wenn Sie einen {{site.data.keyword.Bluemix_notm}}-API-Schlüssel verwenden, müssen Sie die {{site.data.keyword.Bluemix_notm}}-Konto-ID verwenden, für die der API-Schlüssel erstellt wurde. Um auf Cluster in anderen Konten zugreifen zu können, müssen Sie sich bei dem betreffenden Konto anmelden und einen {{site.data.keyword.Bluemix_notm}}-API-Schlüssel erstellen, der auf diesem Konto basiert.
    {: note}

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Eingabeparameter zum Abrufen von IAM-Tokens mit dem Eingabeparameter in Spalte 1 und dem Wert in Spalte 2.">
    <caption>Eingabeparameter zum Abrufen von IAM-Tokens.</caption>
    <thead>
        <th>Eingabeparameter</th>
        <th>Werte</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`<p><strong>Hinweis:</strong> <code>Yng6Yng=</code> entspricht der URL-codierten Autorisierung für den Benutzernamen <strong>bx</strong> und das Kennwort <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Hauptteil für {{site.data.keyword.Bluemix_notm}}-Benutzername und -Kennwort</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: Ihr {{site.data.keyword.Bluemix_notm}}-Benutzername.</li>
    <li>`password`: Ihr {{site.data.keyword.Bluemix_notm}}-Kennwort.</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Hinweis</strong>: Fügen Sie den Schlüssel <code>uaa_client_secret</code> ohne angegebenen Wert hinzu.</li>
    <li>`bss_account`: Die {{site.data.keyword.Bluemix_notm}}-Konto-ID, die Sie im vorherigen Schritt abgerufen haben.</li></ul>
    </td>
    </tr>
    <tr>
    <td>Hauptteil für {{site.data.keyword.Bluemix_notm}}-API-Schlüssel</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: Ihr {{site.data.keyword.Bluemix_notm}}-API-Schlüssel.</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Hinweis</strong>: Fügen Sie den Schlüssel <code>uaa_client_secret</code> ohne angegebenen Wert hinzu.</li>
    <li>`bss_account`: Die {{site.data.keyword.Bluemix_notm}}-Konto-ID, die Sie im vorherigen Schritt abgerufen haben.</li></ul>
      </td>
    </tr>
    <tr>
    <td>Hauptteil für einmaligen {{site.data.keyword.Bluemix_notm}}-Kenncode</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:passcode`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: Ihr {{site.data.keyword.Bluemix_notm}}-Kenncode. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Hinweis</strong>: Fügen Sie den Schlüssel <code>uaa_client_secret</code> ohne angegebenen Wert hinzu.</li>
    <li>`bss_account`: Die {{site.data.keyword.Bluemix_notm}}-Konto-ID, die Sie im vorherigen Schritt abgerufen haben.</li></ul></td>
    </tr>
    </tbody>
    </table>

    Beispielausgabe:

    ```
    {
      "access_token": "<iam-token>",
      "refresh_token": "<iam-aktualisierungstoken>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    Sie finden das {{site.data.keyword.Bluemix_notm}} IAM-Token im Feld **access_token** und das Aktualisierungstoken im Feld **refresh_token** Ihrer API-Ausgabe.

4.  Listen Sie die verfügbaren {{site.data.keyword.containerlong_notm}}-Regionen auf und wählen Sie die Region aus, in der Sie arbeiten möchten. Verwenden Sie das IAM-Zugriffstoken und das Aktualisierungstoken aus dem vorherigen Schritt, um Ihre Headerinformationen zu erstellen.
    ```
    GET https://containers.cloud.ibm.com/v1/regions
    ```
    {: codeblock}

    <table summary="Eingabeparameter zum Abrufen von {{site.data.keyword.containerlong_notm}}-Regionen mit dem Eingabeparameter in Spalte 1 und dem Wert in Spalte 2.">
    <caption>Eingabeparameter zum Abrufen von {{site.data.keyword.containerlong_notm}}-Regionen.</caption>
    <thead>
    <th>Eingabeparameter</th>
    <th>Werte</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>`Authorization: bearer <iam_token>`</li>
    <li>`X-Auth-Refresh-Token: <refresh_token>`</li></ul></td>
    </tr>
    </tbody>
    </table>

    Beispielausgabe:
    ```
    {
    "regions": [
        {
            "name": "ap-north",
            "alias": "jp-tok",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "ap-south",
            "alias": "au-syd",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "eu-central",
            "alias": "eu-de",
            "cfURL": "api.eu-de.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "uk-south",
            "alias": "eu-gb",
            "cfURL": "api.eu-gb.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "us-east",
            "alias": "us-east",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "us-south",
            "alias": "us-south",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": true
        }
    ]
    }
    ```
    {: screen}

5.  Listen Sie alle Cluster in der {{site.data.keyword.containerlong_notm}}-Region auf, die Sie ausgewählt haben. Wenn Sie [Kubernetes-API-Anforderungen für Ihren Cluster ausführen](#kube_api), achten Sie darauf, die **id** und **region** Ihres Clusters zu vermerken.

     ```
     GET https://containers.cloud.ibm.com/v1/clusters
     ```
     {: codeblock}

     <table summary="Eingabeparameter zum Arbeiten mit der {{site.data.keyword.containerlong_notm}}-API mit dem Eingabeparameter in Spalte 1 und dem Wert in Spalte 2.">
     <caption>Eingabeparameter zum Arbeiten mit der {{site.data.keyword.containerlong_notm}}-API.</caption>
     <thead>
     <th>Eingabeparameter</th>
     <th>Werte</th>
     </thead>
     <tbody>
     <tr>
     <td>Header</td>
     <td><ul><li>`Authorization: bearer <iam_token>`</li>
       <li>`X-Auth-Refresh-Token: <refresh_token>`</li><li>`X-Region: <region>`</li></ul></td>
     </tr>
     </tbody>
     </table>

5.  Eine Liste von unterstützten APIs finden Sie in der [{{site.data.keyword.containerlong_notm}}-API-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://containers.cloud.ibm.com/swagger-api).

<br />


## Mit dem Cluster unter Verwendung der Kubernetes-API arbeiten
{: #kube_api}

Sie können die [Kubernetes-API ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/reference/using-api/api-overview/) für die Interaktion mit Ihrem Cluster in {{site.data.keyword.containerlong_notm}} verwenden.
{: shortdesc}

Für die folgenden Anweisungen ist ein öffentlicher Netzzugriff in Ihrem Cluster erforderlich, um eine Verbindung zum öffentlichen Serviceendpunkt Ihres Kubernetes-Masters herzustellen.
{: note}

1. Befolgen Sie die Schritte in [Clusterbereitstellungen mit der API automatisieren](#cs_api), um Ihr {{site.data.keyword.Bluemix_notm}} IAM-Zugriffstoken, das Aktualisierungstoken, die ID des Clusters, in dem Sie die Kubernetes-API-Anforderungen ausführen möchten, und die {{site.data.keyword.containerlong_notm}}-Region abzurufen, in der sich Ihr Cluster befindet.

2. Rufen Sie ein von {{site.data.keyword.Bluemix_notm}} IAM delegiertes Aktualisierungstoken ab.
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="Eingabeparameter zum Abrufen eines von IAM delegierten Aktualisierungstokens mit dem Eingabeparameter in Spalte 1 und dem Wert in Spalte 2.">
   <caption>Eingabeparameter zum Abrufen eines von IAM delegierten Aktualisierungstokens. </caption>
   <thead>
   <th>Eingabeparameter</th>
   <th>Werte</th>
   </thead>
   <tbody>
   <tr>
   <td>Header</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Berechtigung: Basic Yng6Yng=`</br><strong>Hinweis</strong>: <code>Yng6Yng=</code> entspricht der URL-codierten Autorisierung für den Benutzernamen <strong>bx</strong> und das Kennwort <strong>bx</strong>.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>Body</td>
   <td><ul><li>`delegated_refresh_token_expiry: 600`</li>
   <li>`receiver_client_ids: kube`</li>
   <li>`response_type: delegated_refresh_token` </li>
   <li>`refresh_token`: Ihr {{site.data.keyword.Bluemix_notm}} IAM-Aktualisierungstoken.</li>
   <li>`grant_type: refresh_token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   Beispielausgabe:
   ```
   {
    "delegated_refresh_token": <delegated_refresh_token>
   }
   ```
   {: screen}

3. Rufen Sie eine {{site.data.keyword.Bluemix_notm}} IAM-ID, das IAM-Zugriffstoken und das IAM-Aktualisierungstoken ab, indem Sie das delegierte Aktualisierungstoken aus dem vorherigen Schritt verwenden. In Ihrer API-Ausgabe finden Sie das IAM-ID-Token im Feld **id_token**, das IAM-Zugriffstoken im Feld **access_token** und das IAM-Aktualisierungstoken im Feld **refresh_token**.
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="Eingabeparameter zum Abrufen der IAM-ID und des IAM-Zugriffstokens mit dem Eingabeparameter in Spalte 1 und dem Wert in Spalte 2.">
   <caption>Eingabeparameter zum Abrufen der IAM-ID und des IAM-Zugriffstokens.</caption>
   <thead>
   <th>Eingabeparameter</th>
   <th>Werte</th>
   </thead>
   <tbody>
   <tr>
   <td>Header</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Autorisierung: Basic a3ViZTprdWJl`</br><strong>Hinweis</strong>: <code>a3ViZTprdWJl</code> entspricht der URL-codierten Autorisierung für den Benutzernamen <strong><code>kube</code></strong> und das Kennwort <strong><code>kube</code></strong>.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>Body</td>
   <td><ul><li>`refresh_token`: Ihr von {{site.data.keyword.Bluemix_notm}} IAM delegiertes Aktualisierungstoken. </li>
   <li>`grant_type: urn:ibm:params:oauth:grant-type:delegated-refresh-token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   Beispielausgabe:
   ```
   {
    "access_token": "<iam-zugriffstoken>",
    "id_token": "<iam-id-token>",
    "refresh_token": "<iam-aktualisierungstoken>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1553629664,
    "scope": "ibm openid containers-kubernetes"
   }
   ```
   {: screen}

4. Rufen Sie die öffentliche URL Ihres Kubernetes-Masters ab, indem Sie das IAM-Zugriffstoken, das IAM-ID-Token, das IAM-Aktualisierungstoken und die {{site.data.keyword.containerlong_notm}}-Region verwenden, in der sich Ihr Cluster befindet. Sie finden die URL in der **`publicServiceEndpointURL`** Ihrer API-Ausgabe.
   ```
   GET https://containers.cloud.ibm.com/v1/clusters/<cluster_ID>
   ```
   {: codeblock}

   <table summary="Eingabeparameter zum Abrufen des öffentlichen Serviceendpunkts für Ihren Kubernetes-Master mit dem Eingabeparameter in Spalte 1 und dem Wert in Spalte 2.">
   <caption>Eingabeparameter zum Abrufen des öffentlichen Serviceendpunkts für Ihren Kubernetes-Master.</caption>
   <thead>
   <th>Eingabeparameter</th>
   <th>Werte</th>
   </thead>
   <tbody>
   <tr>
   <td>Header</td>
     <td><ul><li>`Autorisierung`: Ihr {{site.data.keyword.Bluemix_notm}} IAM-Zugriffstoken.</li><li>`X-Auth-Refresh-Token`: Ihr {{site.data.keyword.Bluemix_notm}} IAM-Aktualisierungstoken.</li><li>`X-Region`: Die {{site.data.keyword.containerlong_notm}}-Region Ihres Clusters, die Sie mit der API `GET https://containers.cloud.ibm.com/v1/clusters` in [Clusterbereitstellungen mit der API automatisieren](#cs_api) abgerufen haben. </li></ul>
   </td>
   </tr>
   <tr>
   <td>Pfad</td>
   <td>`<cluster_ID>:` Die ID Ihres Clusters, die Sie mit der API `GET https://containers.cloud.ibm.com/v1/clusters` in [Clusterbereitstellungen mit der API automatisieren](#cs_api) abgerufen haben.      </td>
   </tr>
   </tbody>
   </table>

   Beispielausgabe:
   ```
   {
    "location": "Dallas",
    "dataCenter": "dal10",
    "multiAzCapable": true,
    "vlans": null,
    "worker_vlans": null,
    "workerZones": [
        "dal10"
    ],
    "id": "1abc123b123b124ab1234a1a12a12a12",
    "name": "mycluster",
    "region": "us-south",
    ...
    "publicServiceEndpointURL": "https://c7.us-south.containers.cloud.ibm.com:27078"
   }
   ```
   {: screen}

5. Führen Sie die Kubernetes-API-Anforderungen für Ihren Cluster aus, indem Sie das IAM-ID-Token verwenden, das Sie zuvor abgerufen haben. Listen Sie zum Beispiel die Version von Kubernetes auf, die in Ihrem Cluster ausgeführt wird.

   Wenn Sie die SSL-Zertifikatsprüfung in Ihrem API-Testframework aktiviert haben, müssen Sie diese Funktion inaktivieren.
{: tip}

   ```
   GET <publicServiceEndpointURL>/version
   ```
   {: codeblock}

   <table summary="Eingabeparameter zum Anzeigen der Kubernetes-Version, die in Ihrem Cluster ausgeführt wird, mit dem Eingabeparameter in Spalte 1 und dem Wert in Spalte 2. ">
   <caption>Eingabeparameter zum Anzeigen der Kubernetes-Version, die in Ihrem Cluster ausgeführt wird. </caption>
   <thead>
   <th>Eingabeparameter</th>
   <th>Werte</th>
   </thead>
   <tbody>
   <tr>
   <td>Header</td>
   <td>`Authorization: bearer <id_token>`</td>
   </tr>
   <tr>
   <td>Pfad</td>
   <td>`<publicServiceEndpointURL>`: Die **`publicServiceEndpointURL`** Ihres Kubernetes-Masters, die Sie im vorherigen Schritt abgerufen haben.      </td>
   </tr>
   </tbody>
   </table>

   Beispielausgabe:
   ```
   {
    "major": "1",
    "minor": "13",
    "gitVersion": "v1.13.4+IKS",
    "gitCommit": "c35166bd86eaa91d17af1c08289ffeab3e71e11e",
    "gitTreeState": "clean",
    "buildDate": "2019-03-21T10:08:03Z",
    "goVersion": "go1.11.5",
    "compiler": "gc",
    "platform": "linux/amd64"
   }
   ```
   {: screen}

6. Lesen Sie die [Kubernetes-API-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/reference/kubernetes-api/), um eine Liste der unterstützten APIs für die neueste Kubernetes-Version zu finden. Achten Sie darauf, die API-Dokumentation zu verwenden, die mit der Kubernetes-Version Ihres Clusters übereinstimmt. Wenn Sie nicht die aktuellste Kubernetes-Version verwenden, hängen Sie Ihre Version am Ende der URL an. Fügen Sie z. B. `v1.12` hinzu, um auf die API-Dokumentation für Version 1.12 zuzugreifen.


## {{site.data.keyword.Bluemix_notm}} IAM-Zugriffstoken aktualisieren und neue Aktualisierungstoken mit der API abrufen
{: #cs_api_refresh}

Alle {{site.data.keyword.Bluemix_notm}} IAM-Zugriffstoken (Identity and Access Management), die über die API ausgegeben werden, laufen nach einer Stunde ab. Sie müssen Ihr Zugriffstoken regelmäßig aktualisieren, um den Zugriff auf die {{site.data.keyword.Bluemix_notm}}-API zu sichern. Sie können dieselben Schritte ausführen, um ein neues Aktualisierungstoken abzurufen.
{:shortdesc}

Stellen Sie zunächst sicher, dass Sie über ein {{site.data.keyword.Bluemix_notm}} IAM-Aktualisierungstoken oder einen {{site.data.keyword.Bluemix_notm}}-API-Schlüssel verfügen, mit dem Sie ein neues Zugriffstoken anfordern können.
- **Aktualisierungstoken:** Führen Sie die Anweisungen unter [Clustererstellungs- und -verwaltungsprozess mit der {{site.data.keyword.Bluemix_notm}}-API automatisieren](#cs_api) aus.
- **API-Schlüssel:** Rufen Sie Ihren [{{site.data.keyword.Bluemix_notm}}-API-Schlüssel ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/) wie folgt ab.
   1. Klicken Sie in der Menüleiste auf **Verwalten** > **Zugriff (IAM)**.
   2. Klicken Sie auf die Seite **Benutzer** und wählen Sie sich selbst aus.
   3. Klicken Sie im Fenster **API-Schlüssel** auf die Option **IBM Cloud-API-Schlüssel erstellen**.
   4. Geben Sie einen **Namen** und eine **Beschreibung** für Ihren API-Schlüssel ein und klicken Sie auf **Erstellen**.
   4. Klicken Sie auf **Anzeigen**, um den API-Schlüssel anzuzeigen, der für Sie generiert wurde.
   5. Kopieren Sie den API-Schlüssel, um mit ihm Ihr neues {{site.data.keyword.Bluemix_notm}} IAM-Zugriffstoken abzurufen.

Führen Sie die folgenden Schritte aus, wenn Sie ein {{site.data.keyword.Bluemix_notm}} IAM-Token erstellen oder ein neues Aktualisierungstoken abrufen möchten.

1.  Generieren Sie ein neues {{site.data.keyword.Bluemix_notm}} IAM-Zugriffstoken, indem Sie das Aktualisierungstoken oder den {{site.data.keyword.Bluemix_notm}}-API-Schlüssel verwenden.
    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Eingabeparameter für das neue IAM-Token mit dem Eingabeparameter in Spalte 1 und dem Wert in Spalte 2.">
    <caption>Eingabeparameter für neue {{site.data.keyword.Bluemix_notm}} IAM-Token</caption>
    <thead>
    <th>Eingabeparameter</th>
    <th>Werte</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li>
      <li>`Autorisierung: Basic Yng6Yng=`</br></br><strong>Hinweis:</strong> <code>Yng6Yng=</code> entspricht der URL-codierten Autorisierung für den Benutzernamen <strong>bx</strong> und das Kennwort <strong>bx</strong>.</li></ul></td>
    </tr>
    <tr>
    <td>Hauptteil bei Verwendung des Aktualisierungstoken</td>
    <td><ul><li>`grant_type: refresh_token`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`refresh_token:` Ihr {{site.data.keyword.Bluemix_notm}} IAM-Aktualisierungstoken. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</li>
    <li>`bss_account:` Ihre {{site.data.keyword.Bluemix_notm}}-Konto-ID. </li></ul><strong>Hinweis:</strong> Fügen Sie den Schlüssel <code>uaa_client_secret</code> ohne angegebenen Wert hinzu.</td>
    </tr>
    <tr>
      <td>Hauptteil bei Verwendung des {{site.data.keyword.Bluemix_notm}}-API-Schlüssels</td>
      <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey:` Ihr {{site.data.keyword.Bluemix_notm}}-API-Schlüssel. </li>
    <li>`uaa_client_ID: cf`</li>
        <li>`uaa_client_secret:`</li></ul><strong>Hinweis:</strong> Fügen Sie den Schlüssel <code>uaa_client_secret</code> ohne angegebenen Wert hinzu.</td>
    </tr>
    </tbody>
    </table>

    API-Beispielausgabe:

    ```
    {
      "access_token": "<iam-token>",
    "refresh_token": "<iam-aktualisierungstoken>",
    "uaa_token": "<uaa-token>",
    "uaa_refresh_token": "<uaa-aktualisierungstoken>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    }

    ```
    {: screen}

    Sie finden Ihr neues {{site.data.keyword.Bluemix_notm}} IAM-Token im Feld **access_token** und das Aktualisierungstoken im Feld **refresh_token** Ihrer API-Ausgabe.

2.  Arbeiten Sie weiter mit der [{{site.data.keyword.containerlong_notm}}-API-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://containers.cloud.ibm.com/swagger-api), indem Sie das Token aus dem vorherigen Schritt verwenden.

<br />


## {{site.data.keyword.Bluemix_notm}} IAM-Zugriffstoken aktualisieren und neue Aktualisierungstoken mit der CLI abrufen
{: #cs_cli_refresh}

Wenn Sie eine neue CLI-Sitzung starten oder wenn in Ihrer aktuellen CLI-Sitzung 24 Stunden abgelaufen sind, müssen Sie den Kontext für Ihren Cluster festlegen, indem Sie `ibmcloud ks cluster-config <cluster_name>` ausführen. Wenn Sie den Kontext für Ihren Cluster mit diesem Befehl festlegen, wird die Datei `kubeconfig` für Ihren Kubernetes-Cluster heruntergeladen. Außerdem werden ein {{site.data.keyword.Bluemix_notm}} IAM-ID-Token (Identity and Access Management) und ein Aktualisierungstoken zur Authentifizierung ausgegeben.
{: shortdesc}

**ID-Token**: Alle IAM-ID-Token, die über die CLI ausgegeben werden, laufen nach einer Stunde ab. Wenn das ID-Token abläuft, wird das Aktualisierungstoken an den Token-Provider gesendet, um das ID-Token zu aktualisieren. Ihre Authentifizierung wird aktualisiert und Sie können wieder Befehle in Ihrem Cluster ausführen.

**Aktualisierungstoken**: Aktualisierungstokens laufen alle 30 Tage ab. Wenn das Aktualisierungstoken abgelaufen ist, kann das ID-Token nicht aktualisiert werden, und Sie können keine Befehle mehr in der CLI ausführen. Sie erhalten ein neues Aktualisierungstoken, indem Sie `ibmcloud ks cluster-config <cluster_name>` ausführen. Mit diesem Befehl wird auch Ihr ID-Token aktualisiert.
