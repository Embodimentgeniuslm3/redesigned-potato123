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


# Container auf Grundlage von Images erstellen
{: #images}

Ein Docker-Image ist die Basis für jeden Container, den Sie mit {{site.data.keyword.containerlong}} erstellen.
{:shortdesc}

Ein Image wird auf der Grundlage einer Dockerfile erstellt. Hierbei handelt es sich um eine Datei, die Anweisungen zum Erstellen des Image enthält. Eine Dockerfile referenziert möglicherweise in ihren Anweisungen Buildartefakte, die separat gespeichert sind, z. B. eine App, die Konfiguration der App und ihre Abhängigkeiten.

## Image-Registrys planen
{: #planning_images}

Images werden in der Regel in einer Registry gespeichert, auf die der Zugriff entweder öffentlich (extern) möglich ist (öffentliche Registry) oder aber so eingerichtet ist, dass der Zugriff auf eine kleine Gruppe von Benutzern beschränkt ist (private Registry).
{:shortdesc}

Öffentliche Registrys wie Docker Hub sind gut dafür geeignet, sich mit Docker und Kubernetes vertraut zu machen und die erste containerisierte App in einem Cluster zu erstellen. Was Unternehmensanwendungen betrifft, so sollten Sie jedoch eine private Registry (wie z. B. die von {{site.data.keyword.registryshort_notm}} bereitgestellte Registry) verwenden,
um zu verhindern, dass Ihre Images durch nicht berechtigte Benutzer verwendet oder unbefugt Änderungen an ihnen vorgenommen werden. Private Registrys müssen vom Clusteradministrator eingerichtet werden, damit sichergestellt ist, dass die Berechtigungsnachweise für den Zugriff auf die private Registry den Clusterbenutzern zur Verfügung stehen.


Für die Bereitstellung Ihrer Apps im Cluster können Sie mehrere Registrys mit {{site.data.keyword.containerlong_notm}} verwenden.

|Registry|Beschreibung|Vorteil|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started)|Mit dieser Option können Sie Ihr eigenes geschütztes Docker-Image-Repository in {{site.data.keyword.registryshort_notm}} einrichten, in dem Sie Images sicher speichern und gemeinsam mit den Clusterbenutzern nutzen können.|<ul><li>Ermöglicht die Verwaltung des Zugriffs auf Images in Ihrem Konto.</li><li>Ermöglicht die Verwendung der von {{site.data.keyword.IBM_notm}} bereitgestellten Images und Beispielapps wie zum Beispiel {{site.data.keyword.IBM_notm}} Liberty als übergeordnetes Image, zu dem eigener App-Code hinzugefügt werden kann.</li><li>Automatisches Scannen der Images durch Vulnerability Advisor auf potenzielle Sicherheitslücken und Bereitstellung betriebssystemspezifischer Empfehlungen, um diese zu schließen.</li></ul>|
|Beliebige andere private Registry|Ermöglicht die Verbindung einer beliebigen vorhandenen privaten Registry mit Ihrem Cluster durch Erstellung eines [geheimen Schlüssels für Image-Pull-Operationen (imagePullSecret) ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/containers/images/). Dieser dient dazu, die URL zur Ihrer Registry und die Berechtigungsnachweise sicher in einem geheimen Kubernetes-Schlüssel zu speichern.|<ul><li>Ermöglicht die Verwendung vorhandener privater Registrys unabhängig von ihrer jeweiligen Quelle (Docker Hub, organisationseigene Registrys oder andere private Cloud-Registrys).</li></ul>|
|[Öffentlicher Docker Hub ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://hub.docker.com/){: #dockerhub}|Verwenden Sie diese Option, um vorhandene öffentliche Images von Docker Hub direkt in Ihrer [Kubernetes-Bereitstellung![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) zu verwenden, wenn keine Dockerfile-Änderungen erforderlich sind. <p>**Hinweis:** Berücksichtigen Sie, dass diese Option möglicherweise den Sicherheitsanforderungen Ihrer Organisation (wie beispielsweise bei der Zugriffsverwaltung, der Ermittlung von Sicherheitslücken oder der Vertraulichkeit von App-Daten) nicht entspricht.</p>|<ul><li>Für Ihren Cluster ist keine zusätzliche Konfiguration erforderlich.</li><li>Enthält eine Vielzahl von Open-Source-Anwendungen.</li></ul>|
{: caption="Optionen für öffentliche und für private Image-Registrys" caption-side="top"}

Nachdem Sie eine Image-Registry eingerichtet haben, können Clusterbenutzer diese Images zur Bereitstellung ihrer Apps im Cluster verwenden.

Erfahren Sie mehr über das [Sichern der persönlichen Daten](/docs/containers?topic=containers-security#pi) bei der Arbeit mit Container-Images.

<br />


## Vertrauenswürdige Inhalte für Container-Images konfigurieren
{: #trusted_images}

Sie können Container aus vertrauenswürdigen Images erstellen, die signiert und in {{site.data.keyword.registryshort_notm}} gespeichert sind, und Bereitstellungen aus nicht signierten oder gefährdeten Images verhindern.
{:shortdesc}

1.  [Images für vertrauenswürdige Inhalte signieren](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent). Nachdem Sie eine Vertrauensbeziehung für Ihre Images konfiguriert haben, können Sie vertrauenswürdige Inhalte und Unterzeichner, die Images per Push-Operation in Ihre Registry übertragen können, verwalten.
2.  Um eine Richtlinie umzusetzen, die besagt, dass nur signierte Images zum Erstellen von Containern in Ihrem Cluster verwendet werden können, [fügen Sie Container Image Security Enforcement (beta)](/docs/services/Registry?topic=registry-security_enforce#security_enforce) hinzu.
3.  Stellen Sie Ihre App bereit.
    1. [Bereitstellung im Kubernetes-Standardnamensbereich (`default`)](#namespace).
    2. [Bereitstellung in einem anderen Kubernetes-Namensbereich oder aus einer anderen {{site.data.keyword.Bluemix_notm}}-Region bzw. einem anderen Konto](#other).

<br />


## Container aus einem {{site.data.keyword.registryshort_notm}}-Image im Kubernetes-Standardnamensbereich (`default`) bereitstellen
{: #namespace}

Sie können Container aus einem von IBM bereitgestellten öffentlichen Image oder aus einem privaten Image, das in Ihrem Namensbereich {{site.data.keyword.registryshort_notm}}-Namensbereich gespeichert ist, in Ihrem Cluster bereitstellen. Weitere Informationen dazu, wie Ihr Cluster auf Registry-Images zugreift, finden Sie unter [Berechtigung des Clusters zum Extrahieren von Images aus {{site.data.keyword.registrylong_notm}}](#cluster_registry_auth).
{:shortdesc}

Vorbereitende Schritte:
1. [Richten Sie einen Namensbereich in {{site.data.keyword.registryshort_notm}} ein und übertragen Sie Images per Push-Operation an diesen Namensbereich](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2. [Erstellen Sie einen Cluster](/docs/containers?topic=containers-clusters#clusters_cli).
3. Wenn Sie einen vorhandenen Cluster verwenden, der vor dem **25. Februar 2019** erstellt wurde, [aktualisieren Sie Ihren Cluster für die Verwendung des API-Schlüssels `imagePullSecret`](#imagePullSecret_migrate_api_key).
4. [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Gehen Sie wie folgt vor, um einen Container im **Standardnamensbereich** Ihres Clusters bereitzustellen:

1.  Erstellen Sie eine Bereitstellungskonfigurationsdatei mit dem Namen `mydeployment.yaml`.
2.  Definieren Sie die Bereitstellung und das zu verwendende Image aus Ihrem Namensbereich in {{site.data.keyword.registryshort_notm}}.

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app_name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - name: <app_name>
            image: <region>.icr.io/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    Ersetzen Sie die Image-URL-Variablen durch die Informationen für Ihr Image:
    *  **`<app_name>`**: Der Name Ihrer App. 
    *  **`<region>`**: Der regionale {{site.data.keyword.registryshort_notm}}-API-Endpunkt für die Registry-Domäne. Zum Auflisten der Domäne für die Region, in der Sie angemeldet sind, führen Sie `ibmcloud cr api` aus.
    *  **`<namespace>`**: Der Registry-Namensbereich. Zum Abrufen der Informationen zu Ihrem Namensbereich führen Sie den Befehl `ibmcloud cr namespace-list` aus.
    *  **`<my_image>:<tag>`**: Das Image und der Tag, das bzw. die zum Erstellen des Containers verwendet werden soll. Zum Abrufen der in Ihrer Registry verfügbaren Images führen Sie den Befehl `ibmcloud cr images` aus.

3.  Erstellen Sie die Bereitstellung in Ihrem Cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

<br />


## Informationen über das Autorisieren Ihres Clusters zum Extrahieren von Bildern aus einer Registry
{: #cluster_registry_auth}

Zum Extrahieren von Images aus einer Registry verwendet der {{site.data.keyword.containerlong_notm}}-Cluster einen speziellen Typ eines geheimen Kubernetes-Schlüssels, einen geheimen Schlüssel für Image-Pull-Operationen (`imagePullSecret`). Dieser geheime Schlüssel für Image-Pull-Operationen speichert die Berechtigungsnachweise für den Zugriff auf eine Container-Registry. Die Container-Registry kann Ihr Namensbereich in {{site.data.keyword.registrylong_notm}}, ein Namensbereich in {{site.data.keyword.registrylong_notm}}, der einem anderen {{site.data.keyword.Bluemix_notm}}-Konto angehört, oder eine andere private Registry wie z. B. Docker sein. Ihr Cluster ist so konfiguriert, dass er Images aus Ihrem Namensbereich in {{site.data.keyword.registrylong_notm}} extrahiert und Container aus diesen Images im Kubernetes-Standardnamensbereich (`default`) in Ihrem Cluster bereitstellt. Wenn Sie Images in andere Kubernetes-Clusternamensbereiche oder andere Registrys extrahieren müssen, müssen Sie den geheimen Schlüssel für Image-Pull-Operationen konfigurieren.
{:shortdesc}

**Wie wird mein Cluster so konfiguriert, dass Images aus dem Kubernetes-Standardnamensbereich (`default`) extrahiert werden?**<br>
Wenn Sie einen Cluster erstellen, hat der Cluster eine {{site.data.keyword.Bluemix_notm}} IAM-Service-ID, der eine Richtlinie für die IAM-Servicezugriffsrolle **Leseberechtigter** für {{site.data.keyword.registrylong_notm}} zugeordnet wird. Die Service-ID-Berechtigungsnachweise werden als Identität durch einen nicht ablaufenden API-Schlüssel dargestellt, der in geheimen Schlüsseln für Image-Pull-Operationen (imagePullSecrets) in Ihrem Cluster gespeichert wird. Die geheimen Schlüssel für Image-Pull-Operationen werden dem Kubernetes-Namensbereich `default` und der Liste der geheimen Schlüssel im Servicekonto `default` für diesen Namensbereich hinzugefügt. Durch die Verwendung von geheimen Schlüsseln für Image-Pull-Operationen können Ihre Bereitstellungen Images in Ihrer [globalen und regionalen Registry](/docs/services/Registry?topic=registry-registry_overview#registry_regions) extrahieren (Lesezugriff), um Container im Kubernetes-Namensbereich `default` zu erstellen. Die globale Registry dient der sicheren Speicherung öffentlicher, von IBM bereitgestellter Images, auf die Sie in allen Ihren Bereitstellungen verweisen können, sodass es nicht notwendig ist, unterschiedliche Verweise für Images zu verwenden, die in den einzelnen regionalen Registrys gespeichert sind. Die regionale Registry dient der sicheren Speicherung Ihrer eigenen privaten Docker-Images.

**Kann ich den Pull-Zugriff auf eine bestimmte regionale Registry einschränken?**<br>
Ja, Sie können die [vorhandene IAM-Richtlinie der Service-ID bearbeiten](/docs/iam?topic=iam-serviceidpolicy#access_edit), die die Servicezugriffsrolle **Leseberechtigter** auf diese regionale Registry oder auf eine Registry-Ressource wie z. B. einen Namensbereich beschränkt. Bevor Sie Registry-IAM-Richtlinien anpassen können, müssen Sie [{{site.data.keyword.Bluemix_notm}} IAM-Richtlinien für {{site.data.keyword.registrylong_notm}} aktivieren](/docs/services/Registry?topic=registry-user#existing_users).

  Sollen die Berechtigungsnachweise der Registrys noch sicherer werden? Wenden Sie sich zur [Aktivierung von {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) im Cluster an Ihren Clusteradministrator, damit geheime Kubernetes-Schlüssel im Cluster verschlüsselt werden, zum Beispiel `imagePullSecret` zum Speichern der Berechtigungsnachweise von Registrys.
  {: tip}

**Kann ich Images in anderen Kubernetes-Namensbereichen als `default` extrahieren?**<br>
Standardmäßig nicht. Unter Verwendung des Standard-Cluster-Setups können Sie Container aus beliebigen Images, die in Ihrem {{site.data.keyword.registrylong_notm}}-Namensbereich gespeichert sind, im Kubernetes-Standardnamensbereich (`default`) Ihres Clusters bereitstellen. Zur Verwendung dieser Images in anderen Kubernetes-Namensbereichen oder in anderen {{site.data.keyword.Bluemix_notm}}-Konten haben Sie die [Option, einen geheimen Schlüssel für Image-Pull-Operationen zu kopieren oder einen eigenen zu erstellen](#other).

**Kann ich Images aus einem anderen {{site.data.keyword.Bluemix_notm}}-Konto extrahieren?**<br>
Ja, Sie können einen API-Schlüssel in dem {{site.data.keyword.Bluemix_notm}}-Konto erstellen, das Sie vorhaben zu benutzen. Anschließend erstellen Sie einen geheimen Schlüssel für Image-Pull-Operationen, der diese API-Schlüsselberechtigungsnachweise in jedem Cluster und Clusternamensbereich speichert, aus Sie extrahieren möchten. [Folgen Sie diesem Beispiel, bei dem ein API-Schlüssel für eine autorisierte Service-ID verwendet wird](#other_registry_accounts).

Informationen zur Verwendung einer Nicht-{{site.data.keyword.Bluemix_notm}}-Registry wie Docker finden Sie unter [Zugriff auf Images in anderen privaten Registrys](#private_images).

**Muss der API-Schlüssel für eine Service-ID verwendet werden? Was passiert, wenn ich die Begrenzung für Service-IDs für mein Konto erreiche?**<br>
Das Standard-Cluster-Setup erstellt eine Service-ID zum Speichern von {{site.data.keyword.Bluemix_notm}} IAM-API-Schlüsselberechtigungsnachweisen im geheimen Schlüssel für Image-Pull-Operationen. Sie können jedoch auch einen API-Schlüssel für einen einzelnen Benutzer erstellen und diese Berechtigungsnachweise in einem geheimen Schlüssel für Image-Pull-Operationen speichern. Wenn Sie die [IAM-Begrenzung für Service-IDs](/docs/iam?topic=iam-iam_limits#iam_limits) erreichen, wird Ihr Cluster ohne die Service-ID und ohne geheimen Schlüssel für Image-Pull-Operationen erstellt und kann standardmäßig keine Images aus den `icr.io`-Registry-Domänen extrahieren. Sie müssen einen [eigenen geheimen Schlüssel für Image-Pull-Operationen erstellen](#other_registry_accounts), jedoch unter Verwendung eines API-Schlüssels für einen einzelnen Benutzer und nicht mittels einer {{site.data.keyword.Bluemix_notm}} IAM-Service-ID.

**Mein geheimer Schlüssel für Image-Pull-Operationen verwendet ein Registry-Token. Funktioniert ein Token dennoch?**<br>

Die vorherige Methode zur Berechtigung des Clusterzugriffs auf {{site.data.keyword.registrylong_notm}} durch automatisches Erstellen eines [Tokens](/docs/services/Registry?topic=registry-registry_access#registry_tokens) und Speichern des Tokens in einem geheimen Schlüssel für Image-Pull-Operationen wird unterstützt, ist jedoch veraltet.
{: deprecated}

Tokens autorisieren den Zugriff auf die veralteten `registry.bluemix.net`-Registry-Domänen, während API-Schlüssel den Zugriff auf die `icr.io`-Registry-Domänen autorisieren. Während der Übergangszeit von der Token- zur API-Schlüssel-basierten Authentifizierung werden sowohl Token- als auch API-Schlüssel-basierte geheime Schlüssel für Image-Pull-Operationen für eine bestimmte Zeit erstellt. Wenn sowohl Token- als auch API-Schlüssel-basierte geheime Schlüssel für Image-Pull-Operationen vorhanden sind, kann Ihr Cluster Images entweder aus `registry.bluemix.net`- oder aus `icr.io`-Domänen im Kubernetes-Standardnamensbereich (`default`) extrahieren.

Bevor die veralteten Tokens und `registry.bluemix.net`-Domänen nicht mehr unterstützt werden, aktualisieren Sie die geheimen Schlüssel für Image-Pull-Operationen Ihres Clusters, sodass sie die API-Schlüssel-Methode für den [Kubernetes-Standardnamensbereich (`default`) und [alle anderen Namensbereiche oder Konten](#other)(#imagePullSecret_migrate_api_key)] verwenden, die Sie möglicherweise nutzen. Anschließend aktualisieren Sie Ihre Bereitstellungen, sodass sie aus den `icr.io`-Registry-Domänen extrahieren.

<br />


## Vorhandene Cluster zur Verwendung von API-Schlüsseln in geheimen Schlüsseln für Image-Pull-Operationen aktualisieren
{: #imagePullSecret_migrate_api_key}

Neue {{site.data.keyword.containerlong_notm}}-Cluster speichern einen API-Schlüssel in einem [geheimen Schlüssel für Image-Pull-Operationen, um den Zugriff auf {{site.data.keyword.registrylong_notm}} zu autorisieren](#cluster_registry_auth). Mit diesen geheimen Schlüsseln für Image-Pull-Operationen können Sie Container aus Images bereitstellen, die in den Registry-Domänen `icr.io` gespeichert sind. Cluster, die vor dem **25. Februar 2019** erstellt wurden, müssen Sie aktualisieren, um einen API-Schlüssel anstelle eines Registry-Tokens im geheimen Schlüssel für Image-Pull-Operationen zu speichern.
{: shortdesc}

**Vorbereitende Schritte**:
*   [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*   Stellen Sie sicher, dass Sie über die folgenden Berechtigungen verfügen:
    *   {{site.data.keyword.Bluemix_notm}} IAM-Plattformrolle **Operator oder Administrator** für {{site.data.keyword.containerlong_notm}}. Der Kontoeigner kann Ihnen die Rolle erteilen, indem er Folgendes ausführt:
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name containers-kubernetes --roles Administrator,Operator
        ```
        {: pre}
    *   {{site.data.keyword.Bluemix_notm}} IAM-Plattformrolle **Administrator** für {{site.data.keyword.registrylong_notm}}, über alle Regionen und Ressourcengruppen. Der Kontoeigner kann Ihnen die Rolle erteilen, indem er Folgendes ausführt:
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
        ```
        {: pre}

**Gehen Sie wie folgt vor, um den geheimen Schlüssel für Image-Pull-Operationen für Ihren Cluster im Kubernetes-Standardnamensbereich (`default`) zu aktualisieren**:
1.  Rufen Sie Ihre Cluster-ID ab.
    ```
    ibmcloud ks clusters
    ```
    {: pre}
2.  Führen Sie den folgenden Befehl aus, um eine Service-ID für den Cluster zu erstellen, der Service-ID die IAM-Servicerolle **Leseberechtigter** für {{site.data.keyword.registrylong_notm}} zuzuweisen, einen API-Schlüssel zur Darstellung der Berechtigungsnachweise der Service-ID zu erstellen und den API-Schlüssel in einem geheimen Schlüssel für Image-Pull-Operationen im Cluster zu speichern. Der geheime Schlüssel für Image-Pull-Operationen befindet sich im Kubernetes-Namensbereich `default`.
    ```
    ibmcloud ks cluster-pull-secret-apply --cluster <clustername_oder_-id>
    ```
    {: pre}

    Wenn Sie diesen Befehl ausführen, wird die Erstellung von IAM-Berechtigungsnachweisen und geheimen Schlüsseln für Image-Pull-Operationen eingeleitet, deren Ausführung einige Zeit dauern kann. Sie können Container, die ein Image aus {{site.data.keyword.registrylong_notm}}-Domänen `icr.io` extrahieren, erst bereitstellen, wenn die geheimen Schlüssel für Image-Pull-Operationen erstellt wurden.
    {: important}

3.  Überprüfen Sie, ob die geheimen Schlüssel (Secrets) für Image-Pull-Operationen in Ihrem Cluster erstellt wurden. Beachten Sie, dass Sie für jede {{site.data.keyword.registrylong_notm}}-Region einen separaten geheimen Schlüssel für Image-Pull-Operationen haben.
    ```
    kubectl get secrets
    ```
    {: pre}
    Beispielausgabe:
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
4.  Aktualisieren Sie Ihre Containerbereitstellungen, um Images aus dem Domänennamen `icr.io` zu extrahieren.
5.  Optional: Wenn Sie eine Firewall haben, stellen Sie sicher, dass Sie [ausgehenden Netzdatenverkehr an die Registry-Teilnetze für die Domänen zulassen,](/docs/containers?topic=containers-firewall#firewall_outbound) die Sie verwenden.

**Womit möchten Sie fortfahren?**
*   Zum Extrahieren von Images in anderen Kubernetes-Namensbereichen als `default` oder aus anderen {{site.data.keyword.Bluemix_notm}}-Konten [kopieren Sie einen anderen geheimen Schlüssel für Image-Pull-Operationen oder erstellen einen solchen](/docs/containers?topic=containers-images#other).
*   Zum Einschränken des Zugriffs geheimer Schlüssel für Image-Pull-Operationen auf bestimmte Registry-Ressourcen, wie zum Beispiel Namensbereiche oder Regionen:
    1.  Stellen Sie sicher, dass [{{site.data.keyword.Bluemix_notm}} IAM-Richtlinien für {{site.data.keyword.registrylong_notm}} aktiviert sind](/docs/services/Registry?topic=registry-user#existing_users).
    2.  [Bearbeiten Sie die {{site.data.keyword.Bluemix_notm}} IAM-Richtlinien](/docs/iam?topic=iam-serviceidpolicy#access_edit) für die Service-ID oder [erstellen Sie einen anderen geheimen Schlüssel für Image-Pull-Operationen](/docs/containers?topic=containers-images#other_registry_accounts).

<br />


## Geheimen Schlüssel für Image-Pull-Operationen für den Zugriff auf andere Kubernetes-Clusternamensbereiche, andere {{site.data.keyword.Bluemix_notm}}-Konten oder externe private Registrys verwenden
{: #other}

Richten Sie Ihren eigenen geheimen Schlüssel für Image-Pull-Operationen in Ihrem Cluster ein, um Container in anderen Kubernetes-Namensbereichen als `default` bereitzustellen, Images zu verwenden, die in anderen {{site.data.keyword.Bluemix_notm}}-Konten gespeichert sind, oder Images zu verwenden, die in externen privaten Registrys gespeichert sind. Weiterhin können Sie einen geheimen Schlüssel für Image-Pull-Operationen erstellen, um IAM-Zugriffsrichtlinien anzuwenden, die Berechtigungen auf bestimmte Repositorys für Registry-Images, Namensbereiche oder Aktionen (z. B. `push` oder `pull`) einschränken.
{:shortdesc}

Geheime Schlüssel für Image-Pull-Operationen sind nur für die Kubernetes-Namensbereiche gültig, für die sie erstellt wurden. Wiederholen Sie diese Schritte für jeden Namensbereich, in dem Sie Container bereitstellen möchten. Für Images aus [DockerHub](#dockerhub) sind keine geheimen Schlüssel für Image-Pull-Operationen erforderlich.
{: tip}

Vorbereitende Schritte:

1.  [Richten Sie einen Namensbereich in {{site.data.keyword.registryshort_notm}} ein und übertragen Sie Images per Push-Operation an diesen Namensbereich](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2.  [Erstellen Sie einen Cluster](/docs/containers?topic=containers-clusters#clusters_cli).
3.  Wenn Sie über einen vorhandenen Cluster verfügen, der vor dem **<25. Februar 2019>** erstellt wurde, [aktualisieren Sie Ihren Cluster zur Verwendung von API-Schlüsseln in geheimen Schlüsseln für Image-Pull-Operationen](#imagePullSecret_migrate_api_key).
4.  [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

<br/>
Wählen Sie zur Verwendung Ihres eigenen geheimen Schlüssels für Image-Pull-Operationen eine der folgenden Optionen aus:
- [Kopieren Sie den geheimen Schlüssel für Image-Pull-Operationen](#copy_imagePullSecret) aus dem Kubernetes-Standardnamensbereich (default) in andere Namensbereiche in Ihrem Cluster.
- [Erstellen Sie IAM-API-Schlüsselberechtigungsnachweise und speichern Sie sie in einem geheimen Schlüssel für Image-Pull-Operationen](#other_registry_accounts), um auf Images in anderen {{site.data.keyword.Bluemix_notm}}-Konten zuzugreifen oder um IAM-Richtlinien zur Einschränkung des Zugriffs auf bestimmte Registry-Domänen oder Namensbereiche anzuwenden.
- [Erstellen Sie einen geheimen Schlüssel für Image-Pull-Operationen, um auf Images in externen privaten Registrys zuzugreifen](#private_images).

<br/>
Wenn Sie bereits einen geheimen Schlüssel für Image-Pull-Operationen in Ihrem Namensbereich erstellt haben, den Sie in Ihrer Bereitstellung verwenden möchten, finden Sie weitere Informationen unter [Container mithilfe des erstellten geheimen Schlüssels für Image-Pull-Operationen (`imagePullSecret`) bereitstellen](#use_imagePullSecret).

### Vorhandenen geheimen Schlüssel für Image-Pull-Operationen kopieren
{: #copy_imagePullSecret}

Sie können einen geheimen Schlüssel für Image-Pull-Operationen, z. B. den, der automatisch für den Kubernetes-Namensbereich `default` erstellt wird, in andere Namensbereiche Ihres Clusters kopieren. Wenn Sie andere {{site.data.keyword.Bluemix_notm}} IAM-API-Schlüsselberechtigungsnachweise für diesen Namensbereich verwenden möchten, z. B. um den Zugriff auf bestimmte Namensbereiche einzuschränken oder um Images mit Pull-Operation aus anderen {{site.data.keyword.Bluemix_notm}}-Konten zu extrahieren, [erstellen Sie stattdessen einen geheimen Schlüssel für Image-Pull-Operationen](#other_registry_accounts).
{: shortdesc}

1.  Listen Sie die Kubernetes-Namensbereiche in Ihrem Cluster auf oder erstellen Sie einen Namensbereich, der verwendet werden soll.
    ```
    kubectl get namespaces
    ```
    {: pre}

    Beispielausgabe:
    ```
    default          Active    79d
    ibm-cert-store   Active    79d
    ibm-system       Active    79d
    istio-system     Active    34d
    kube-public      Active    79d
    kube-system      Active    79d
    ```
    {: screen}

    Gehen Sie wie folgt vor, um einen Namensbereich zu erstellen:
    ```
    kubectl create namespace <name_des_namensbereichs>
    ```
    {: pre}
2.  Listen Sie die vorhandenen geheimen Schlüssel für Image-Pull-Operationen (Secrets) im Kubernetes-Namensbereich `default` für {{site.data.keyword.registrylong_notm}} auf.
    ```
    kubectl get secrets -n default | grep icr
    ```
    {: pre}
    Beispielausgabe:
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d
    default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
3.  Kopieren Sie jeden geheimen Schlüssel für Image-Pull-Operationen aus dem Namensbereich `default` in den Namensbereich Ihrer Wahl. Die neuen geheimen Schlüssel für Image-Pull-Operationen erhalten Namen der Form `<namensbereich>-icr-<region>-io`.
    ```
    kubectl get secret default-us-icr-io -o yaml | sed 's/default/<neuer_namensbereich>/g' | kubectl -n <neuer_namensbereich> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-uk-icr-io -o yaml | sed 's/default/<neuer_namensbereich>/g' | kubectl -n <neuer_namensbereich> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-de-icr-io -o yaml | sed 's/default/<neuer_namensbereich>/g' | kubectl -n <neuer_namensbereich> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-au-icr-io -o yaml | sed 's/default/<neuer_namensbereich>/g' | kubectl -n <neuer_namensbereich> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-jp-icr-io -o yaml | sed 's/default/<neuer_namensbereich>/g' | kubectl -n <neuer_namensbereich> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-icr-io -o yaml | sed 's/default/<neuer_namensbereich>/g' | kubectl -n <neuer_namensbereich> create -f -
    ```
    {: pre}
4.  Überprüfen Sie, ob die geheimen Schlüssel erfolgreich erstellt wurden.
    ```
    kubectl get secrets -n <name_des_namensbereichs>
    ```
    {: pre}
5.  [Sie haben die Möglichkeit, den geheimen Schlüssel für Image-Pull-Operationen einem Kubernetes-Servicekonto hinzuzufügen, sodass jeder Pod in dem Namensbereich den geheimen Schlüssel für Image-Pull-Operationen verwenden kann, wenn Sie einen Container bereitstellen](#use_imagePullSecret).

### Geheimen Schlüssel für Image-Pull-Operationen mit unterschiedlichen IAM-API-Schlüsselberechtigungsnachweisen erstellen, um mehr Kontrolle oder Zugriff auf Images in anderen {{site.data.keyword.Bluemix_notm}}-Konten zu erhalten
{: #other_registry_accounts}

Sie können {{site.data.keyword.Bluemix_notm}} IAM-Zugriffsrichtlinien an Benutzer oder an eine Service-ID zuweisen, um die Berechtigungen für bestimmte Repositorys für Registry-Images, Namensbereiche oder Aktionen (wie `push` oder `pull`) einzuschränken. Anschließend erstellen Sie einen API-Schlüssel und speichern diese Registry-Berechtigungsnachweise in einem geheimen Schlüssel für Image-Pull-Operationen für Ihren Cluster.
{: shortdesc}

Wenn Sie beispielsweise auf Images in anderen {{site.data.keyword.Bluemix_notm}}-Konten zugreifen möchten, erstellen Sie einen API-Schlüssel, der die {{site.data.keyword.registryshort_notm}}-Berechtigungsnachweise eines Benutzers oder einer Service-ID in diesem Konto speichert. Anschließend speichern Sie Im Konto Ihres Clusters die API-Schlüsselberechtigungsnachweise in einem geheimen Schlüssel für Image-Pull-Operationen für jeden Cluster und Clusternamensbereich.

Mit den folgenden Schritten wird ein API-Schlüssel erstellt, der die Berechtigungsnachweise einer {{site.data.keyword.Bluemix_notm}} IAM-Service-ID speichert. Anstatt eine Service-ID zu verwenden, können Sie auch einen API-Schlüssel für eine Benutzer-ID erstellen, die eine {{site.data.keyword.Bluemix_notm}} IAM-Servicezugriffsrichtlinie für {{site.data.keyword.registryshort_notm}} hat. Stellen Sie jedoch sicher, dass der Benutzer eine funktionsfähige ID ist, oder treffen Sie Vorkehrungen für den Fall, dass der Benutzer ausscheidet, sodass der Cluster weiterhin auf die Registry zugreifen kann.
{: note}

1.  Listen Sie die Kubernetes-Namensbereiche in Ihrem Cluster auf oder erstellen Sie einen Namensbereich, der verwendet werden soll, wenn Sie Container aus Ihren Registry-Images bereitstellen wollen.
    ```
    kubectl get namespaces
    ```
    {: pre}

    Beispielausgabe:
    ```
    default          Active    79d
    ibm-cert-store   Active    79d
    ibm-system       Active    79d
    istio-system     Active    34d
    kube-public      Active    79d
    kube-system      Active    79d
    ```
    {: screen}

    Gehen Sie wie folgt vor, um einen Namensbereich zu erstellen:
    ```
    kubectl create namespace <name_des_namensbereichs>
    ```
    {: pre}
2.  Erstellen Sie eine {{site.data.keyword.Bluemix_notm}} IAM-Service-ID für Ihren Cluster, die für die IAM-Richtlinien und die API-Schlüsselberechtigungsnachweise in dem geheimen Schlüssel für Image-Pull-Operationen verwendet wird. Stellen Sie sicher, dass Sie die Service-ID mit einer Beschreibung versehen, die Ihnen später beim Abrufen der Service-ID hilft, zum Beispiel indem Sie den Clusternamen und den Namenbereich mitangeben.
    ```
    ibmcloud iam service-id-create <clustername>-<kube-namensbereich>-id --description "Service-ID für IBM Cloud Container Registry in Kubernetes-Cluster <clustername> namespace <kube-namensbereich>"
    ```
    {: pre}
3.  Erstellen Sie eine angepasste {{site.data.keyword.Bluemix_notm}} IAM-Richtlinie für Ihre Cluster-Service-ID, die Zugriff auf {{site.data.keyword.registryshort_notm}} erteilt.
    ```
    ibmcloud iam service-policy-create <cluster-service-id> --roles <servicezugriffsrolle> --service-name container-registry [--region <iam-region>] [--resource-type namespace --resource <registry-namensbereich>]
    ```
    {: pre}
    <table>
    <caption>Erklärung der Bestandteile dieses Befehls</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Bestandteile dieses Befehls</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster-service-id&gt;</em></code></td>
    <td>Erforderlich. Ersetzen Sie diese Variable durch die Service-ID `<clustername>-<kube-namensbereich>-id`, die Sie zuvor für Ihren Kubernetes-Cluster erstellt haben.</td>
    </tr>
    <tr>
    <td><code>--service-name <em>container-registry</em></code></td>
    <td>Erforderlich. Geben Sie `container-registry` so ein, dass die IAM-Richtlinie für {{site.data.keyword.registrylong_notm}} gilt.</td>
    </tr>
    <tr>
    <td><code>--roles <em>&lt;servicezugriffsrolle&gt;</em></code></td>
    <td>Erforderlich. Geben Sie die [Servicezugriffsrolle für {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-iam#service_access_roles) ein, auf die Sie den Service-ID-Zugriff festlegen wollen. Mögliche Werte: `Reader` (Leseberechtigter), `Writer` (Schreibberechtigter) und `Manager`.</td>
    </tr>
    <tr>
    <td><code>--region <em>&lt;iam-region&gt;</em></code></td>
    <td>Optional. Wenn Sie den Bereich der Zugriffsrichtlinie auf bestimmte IAM-Regionen festlegen wollen, geben Sie die Regionen in einer durch Kommas getrennten Liste ein. Mögliche Werte: `au-syd`, `eu-gb`, `eu-de`, `jp-tok`, `us-south` und `global`.</td>
    </tr>
    <tr>
    <td><code>--resource-type <em>namensbereich</em> --resource <em>&lt;registry-namensbereich&gt;</em></code></td>
    <td>Optional. Wenn Sie den Zugriff nur auf Images in bestimmten [{{site.data.keyword.registrylong_notm}}-Namensbereichen](/docs/services/Registry?topic=registry-registry_overview#registry_namespaces) beschränken wollen, geben Sie den Wert `namensbereich` für den Ressourcentyp ein und geben Sie den Wert für `<registry-namensbereich>` an. Führen Sie zum Auflisten von Registry-Namensbereichen den Befehl `ibmcloud cr namespaces` aus.</td>
    </tr>
    </tbody></table>
4.  Erstellen Sie einen API-Schlüssel für die Service-ID. Benennen Sie den API-Schlüssel ähnlich wie Ihre Service-ID und schließen Sie die Service-ID ein, die Sie zuvor erstellt haben: ``<clustername>-<kube-namensbereich>-id`. Stellen Sie sicher, dass Sie dem API-Schlüssel eine Beschreibung beifügen, die Ihnen später beim Abrufen des Schlüssels hilft.
    ```
    ibmcloud iam service-api-key-create <clustername>-<kube-namensbereich>-key <clustername>-<kube-namensbereich>-id --description "API-Schlüssel für Service-ID <service-id> in Kubernetes-Cluster <clustername> Namensbereich <kube-namensbereich>"
    ```
    {: pre}
5.  Ermitteln Sie Ihren Wert für **API Key** in der Ausgabe des vorherigen Befehls.
    ```
    Please preserve the API key! It cannot be retrieved after it's created.

    Name          <clustername>-<kube-namensbereich>-key   
    Description   key_for_registry_for_serviceid_for_kubernetes_cluster_multizone_namespace_test   
    Bound To      crn:v1:bluemix:public:iam-identity::a/1bb222bb2b33333ddd3d3333ee4ee444::serviceid:ServiceId-ff55555f-5fff-6666-g6g6-777777h7h7hh   
    Created At    2019-02-01T19:06+0000   
    API Key       i-8i88ii8jjjj9jjj99kkkkkkkkk_k9-llllll11mmm1   
    Locked        false   
    UUID          ApiKey-222nn2n2-o3o3-3o3o-4p44-oo444o44o4o4   
    ```
    {: screen}
6.  Erstellen Sie einen geheimen Kubernetes-Schlüssel für Image-Pull-Operationen zum Speichern der API-Schlüsselberechtigungsnachweise im Namensbereich des Clusters. Wiederholen Sie diesen Schritt für jede Domäne `icr.io`, für jeden Kubernetes-Namensbereich und für jeden Cluster, in denen Sie Images aus der Registry mit den IAM-Berechtigungsnachweisen dieser Service-ID extrahieren wollen.
    ```
    kubectl --namespace <kubernetes-namensbereich> create secret docker-registry <name_des_geheimen_schlüssels> --docker-server=<registry-url> --docker-username=iamapikey --docker-password=<api-schlüsselwert> --docker-email=<docker-e-mail>
    ```
    {: pre}

    <table>
    <caption>Erklärung der Bestandteile dieses Befehls</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Bestandteile dieses Befehls</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes-namensbereich&gt;</em></code></td>
    <td>Erforderlich. Geben Sie den Kubernetes-Namensbereich Ihres Clusters an, den Sie für den Service-ID-Namen verwendet haben.</td>
    </tr>
    <tr>
    <td><code><em>&lt;name_des_geheimen_schlüssels&gt;</em></code></td>
    <td>Erforderlich. Geben Sie einen Namen für Ihren geheimen Schlüssel für Image-Pull-Operationen ein.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry-url&gt;</em></code></td>
    <td>Erforderlich. Setzen Sie die URL auf die Image-Registry, in der Ihr Registry-Namensbereich eingerichtet ist. Verfügbare Registry-Domänen:<ul>
    <li>Asien-Pazifik (Norden - Tokio): `jp.icr.io`</li>
    <li>Asien-Pazifik (Süden - Sydney): `au.icr.io`</li>
    <li>Mitteleuropa (Frankfurt): `de.icr.io`</li>
    <li>Vereinigtes Königreich (Süden - London): `uk.icr.io`</li>
    <li>Vereinigte Staaten (Süden - Dallas): `us.icr.io`</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username iamapikey</code></td>
    <td>Erforderlich. Geben Sie den Benutzernamen für die Anmeldung bei Ihrer privaten Registry ein. Für {{site.data.keyword.registryshort_notm}} wird der Benutzername auf den Wert <strong><code>iamapikey</code></strong> gesetzt.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;tokenwert&gt;</em></code></td>
    <td>Erforderlich. Geben Sie den Wert für Ihren API-Schlüssel (**API Key**) ein, den Sie zuvor ermittelt haben.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-e-mail&gt;</em></code></td>
    <td>Erforderlich. Falls Sie über eine Docker-E-Mail-Adresse verfügen, geben Sie diese ein. Ist dies nicht der Fall, geben Sie eine fiktive E-Mail-Adresse ein, zum Beispiel: `a@b.c`. Diese E-Mail-Adresse ist erforderlich, um einen geheimen Kubernetes-Schlüssel zu erstellen, wird aber nach der Erstellung nicht weiter genutzt.</td>
    </tr>
    </tbody></table>
7.  Überprüfen Sie, ob der geheime Schlüssel erfolgreich erstellt wurde. Ersetzen Sie <em>&lt;kubernetes-namensbereich&gt;</em> durch den Namensbereich, in dem Sie den geheimen Schlüssel für Image-Pull-Operationen erstellt haben.

    ```
    kubectl get secrets --namespace <kubernetes-namensbereich>
    ```
    {: pre}
8.  [Sie haben die Möglichkeit, den geheimen Schlüssel für Image-Pull-Operationen einem Kubernetes-Servicekonto hinzuzufügen, sodass jeder Pod in dem Namensbereich den geheimen Schlüssel für Image-Pull-Operationen verwenden kann, wenn Sie einen Container bereitstellen](#use_imagePullSecret).

### Zugriff auf Images in anderen privaten Registrys
{: #private_images}

Wenn bereits eine private Registry vorhanden ist, müssen Sie die Berechtigungsnachweise für die Registry in einem geheimen Kubernetes-Schlüssel für Image-Pull-Operationen speichern und diesen geheimen Schlüssel in Ihrer Konfigurationsdatei referenzieren.
{:shortdesc}

Vorbereitende Schritte:

1.  [Erstellen Sie einen Cluster](/docs/containers?topic=containers-clusters#clusters_cli).
2.  [Geben Sie als Ziel Ihrer CLI Ihren Cluster an](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Gehen Sie wie folgt vor, um einen geheimen Schlüssel für Image-Pull-Operationen zu erstellen:

1.  Erstellen Sie den geheimen Kubernetes-Schlüssel, um Ihre Berechtigungsnachweise für die private Registry zu speichern.

    ```
    kubectl --namespace <kubernetes-namensbereich> create secret docker-registry <name_des_geheimen_schlüssels>  --docker-server=<registry-url> --docker-username=<docker-benutzername> --docker-password=<docker-kennwort> --docker-email=<docker-e-mail>
    ```
    {: pre}

    <table>
    <caption>Erklärung der Bestandteile dieses Befehls</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Bestandteile dieses Befehls</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes-namensbereich&gt;</em></code></td>
    <td>Erforderlich. Der Kubernetes-Namensbereich Ihres Clusters, in dem Sie den geheimen Schlüssel verwenden und Container bereitstellen möchten. Führen Sie <code>kubectl get namespaces</code> aus, um alle Namensbereiche in Ihrem Cluster aufzulisten.</td>
    </tr>
    <tr>
    <td><code><em>&lt;name_des_geheimen_schlüssels&gt;</em></code></td>
    <td>Erforderlich. Der Name, den Sie für Ihren geheimen Schlüssel für Image-Pull-Operationen (<code>imagePullSecret</code>) verwenden wollen.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry-url&gt;</em></code></td>
    <td>Erforderlich. Die URL der Registry, in der Ihre privaten Images gespeichert sind.</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker-benutzername&gt;</em></code></td>
    <td>Erforderlich. Der Benutzername für die Anmeldung bei Ihrer privaten Registry.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;tokenwert&gt;</em></code></td>
    <td>Erforderlich. Der Wert des zuvor abgerufenen Registry-Tokens.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-e-mail&gt;</em></code></td>
    <td>Erforderlich. Falls Sie über eine Docker-E-Mail-Adresse verfügen, geben Sie diese ein. Ist dies nicht der Fall, geben Sie eine fiktive E-Mail-Adresse ein, zum Beispiel: `a@b.c`. Diese E-Mail-Adresse ist erforderlich, um einen geheimen Kubernetes-Schlüssel zu erstellen, wird aber nach der Erstellung nicht weiter genutzt.</td>
    </tr>
    </tbody></table>

2.  Überprüfen Sie, ob der geheime Schlüssel erfolgreich erstellt wurde. Ersetzen Sie <em>&lt;kubernetes-namensbereich&gt;</em> durch den Namen des Namensbereichs, in dem Sie den geheimen Schlüssel für Image-Pull-Operationen (`imagePullSecret`) erstellt haben.

    ```
    kubectl get secrets --namespace <kubernetes-namensbereich>
    ```
    {: pre}

3.  [Erstellen Sie einen Pod, der den geheimen Schlüssel für Image-Pull-Operationen referenziert](#use_imagePullSecret).

<br />


## Geheimen Schlüssel für Image-Pull-Operationen zum Bereitstellen von Containern verwenden
{: #use_imagePullSecret}

Sie können einen geheimen Schlüssel für Image-Pull-Operationen in Ihrer Pod-Bereitstellung definieren oder den geheimen Schlüssel für Image-Pull-Operationen in Ihrem Kubernetes-Servicekonto speichern, sodass er für alle Bereitstellungen, bei denen kein Servicekonto angegeben wird, verfügbar ist.
{: shortdesc}

Wählen Sie eine der beiden folgenden Optionen aus:
* [Verweis auf den geheimen Schlüssel für Image-Pull-Operationen in Ihrer Pod-Bereitstellung:](#pod_imagePullSecret) Verwenden Sie diese Option, wenn Sie nicht allen Pods in Ihrem Namensbereich standardmäßig Zugriff auf Ihre Registry geben möchten.
* [Speichern des geheimen Schlüssels für Image-Pull-Operationen im Kubernetes-Servicekonto](#store_imagePullSecret): Verwenden Sie diese Option, um Zugriff auf Images in Ihrer Registry für alle Bereitstellungen in den ausgewählten Kubernetes-Namensbereichen zu erteilen.

Vorbereitende Schritte:
* [Erstellen Sie einen geheimen Schlüssel für Image-Pull-Operationen](#other), um auf andere Images in anderen Registrys oder Kubernetes-Namensbereichen als `default` zuzugreifen.
* [Geben Sie als Ziel Ihrer CLI Ihren Cluster an](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

### Verweis auf den geheimen Schlüssel für Image-Pull-Operationen in Ihrer Pod-Bereitstellung
{: #pod_imagePullSecret}

Wenn Sie auf den geheimen Schlüssel für Image-Pull-Operationen in einer Pod-Bereitstellung verweisen, ist der geheime Schlüssel für Image-Pull-Operationen nur für diesen Pod gültig und kann nicht mit anderen Pods im Namensbereich gemeinsam verwendet werden.
{:shortdesc}

1.  Erstellen Sie eine Podkonfigurationsdatei mit dem Namen `mypod.yaml`.
2.  Definieren Sie den Pod und den geheimen Schlüssel für Image-Pull-Operationen für den Zugriff auf Images in {{site.data.keyword.registrylong_notm}}.

    Gehen Sie wie folgt vor, um auf ein privates Image zuzugreifen:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <containername>
          image: <region>.icr.io/<name_des_namensbereichs>/<imagename>:<tag>
      imagePullSecrets:
        - name: <name_des_geheimen_schlüssels>
    ```
    {: codeblock}

    Gehen Sie wie folgt vor, um auf ein öffentliches {{site.data.keyword.Bluemix_notm}}-Image zuzugreifen:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <containername>
          image: icr.io/<imagename>:<tag>
      imagePullSecrets:
        - name: <name_des_geheimen_schlüssels>
    ```
    {: codeblock}

    <table>
    <caption>Erklärung der Komponenten der YAML-Datei</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;containername&gt;</em></code></td>
    <td>Der Name des Containers, den Sie in Ihrem Cluster bereitstellen möchten.</td>
    </tr>
    <tr>
    <td><code><em>&lt;name_des_namensbereichs&gt;</em></code></td>
    <td>Der Namensbereich, in dem das Image gespeichert ist. Führen Sie den Befehl `ibmcloud cr namespace-list` aus, um die verfügbaren Namensbereiche aufzulisten.</td>
    </tr>
    <tr>
    <td><code><em>&lt;imagename&gt;</em></code></td>
    <td>Der Name des zu verwendenden Images. Führen Sie den Befehl `ibmcloud cr image-list` aus, um die verfügbaren Images in einem {{site.data.keyword.Bluemix_notm}}-Konto aufzulisten.</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>Die Version des Images, das Sie verwenden möchten. Ist kein Tag angegeben, wird standardmäßig das Image mit dem Tag <strong>latest</strong> verwendet.</td>
    </tr>
    <tr>
    <td><code><em>&lt;name_des_geheimen_schlüssels&gt;</em></code></td>
    <td>Der Name des geheimen Schlüssels für Image-Pull-Operationen, den Sie zuvor erstellt haben.</td>
    </tr>
    </tbody></table>

3.  Speichern Sie Ihre Änderungen.
4.  Erstellen Sie die Bereitstellung in Ihrem Cluster.
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### Geheimen Schlüssel für Image-Pull-Operationen im Kubernetes-Servicekonto für den ausgewählten Namensbereich speichern
{:#store_imagePullSecret}

Jeder Namensbereich hat ein Kubernetes-Servicekonto namens `default`. Sie können diesem Servicekonto den geheimen Schlüssel für Image-Pull-Operationen hinzufügen, um Zugriff auf Images in Ihrer Registry zu erteilen. Bereitstellungen, bei denen kein Servicekonto angegeben ist, verwenden automatisch das Servicekonto `default` für diesen Namensbereich.
{:shortdesc}

1. Prüfen Sie, ob bereits ein geheimer Schlüssel für Image-Pull-Operationen für Ihr Standardservicekonto vorhanden ist.
   ```
   kubectl describe serviceaccount default -n <name_des_namensbereichs>
   ```
   {: pre}
   Wenn `<none>` im Eintrag **Image pull secrets** angezeigt wird, ist kein geheimer Schlüssel für Image-Pull-Operationen vorhanden.  
2. Fügen Sie Ihrem Standardservicekonto den geheimen Schlüssel für Image-Pull-Operationen hinzu.
   - **Gehen Sie wie folgt vor, um den geheimen Schlüssel für Image-Pull-Operationen hinzuzufügen, wenn kein solcher definiert ist:**
       ```
       kubectl patch -n <name_des_namensbereichs> serviceaccount/default -p '{"imagePullSecrets":[{"name": "<name_des_geheimen_Schlüssels_für_image-pull-operationen>"}]}'
       ```
       {: pre}
   - **Gehen Sie wie folgt vor, um den geheimen Schlüssel für Image-Pull-Operationen hinzuzufügen, wenn bereits ein solcher definiert ist:**
       ```
       kubectl patch -n <name_des_namensbereichs> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"<name_des_geheimen_Schlüssels_für_image-pull-operationen>"}}]'
       ```
       {: pre}
3. Überprüfen Sie, ob Ihr geheimer Schlüssel für Image-Pull-Operationen Ihrem Standardservicekonto hinzugefügt wurde.
   ```
   kubectl describe serviceaccount default -n <name_des_namensbereichs>
   ```
   {: pre}

   Beispielausgabe:
   ```
   Name:                default
   Namespace:           <name_des_namensbereichs>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  <name_des_geheimen_schlüssels_für_image-pull-operationen>
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. Stellen Sie einen Container aus einem Image in Ihrer Registry bereit.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <containername>
         image: registry.<region>.bluemix.net/<name_des_namensbereichs>/<imagename>:<tag>
   ```
   {: codeblock}

5. Erstellen Sie die Bereitstellung im Cluster.
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


## Veraltet: Registry-Token zum Bereitstellen von Containern aus einem {{site.data.keyword.registrylong_notm}}-Image verwenden
{: #namespace_token}

Sie können Container aus einem von IBM bereitgestellten öffentlichen Image oder einem privaten Image, das in Ihrem Namensbereich in {{site.data.keyword.registryshort_notm}} gespeichert ist, in Ihrem Cluster bereitstellen. Vorhandene Cluster verwenden ein [Registry-Token](/docs/services/Registry?topic=registry-registry_access#registry_tokens), das in einem geheimen Schlüssel für Image-Pull-Operationen (`imagePullSecret`) des Clusters gespeichert ist, um den Zugriff zum Extrahieren von Images aus den Domänennamen mit `registry.bluemix.net` zu autorisieren.
{:shortdesc}

Wenn Sie einen Cluster erstellen, werden automatisch nicht ablaufende Registry-Tokens und geheime Schlüssel für die [nächstgelegene regionale Registry sowie die globale Registry](/docs/services/Registry?topic=registry-registry_overview#registry_regions) erstellt. Die globale Registry dient der sicheren Speicherung öffentlicher, von IBM bereitgestellter Images, auf die Sie in allen Ihren Bereitstellungen verweisen können, sodass es nicht notwendig ist, unterschiedliche Verweise für Images zu verwenden, die in den einzelnen regionalen Registrys gespeichert sind. Die regionale Registry dient der sicheren Speicherung Ihrer eigenen privaten Docker-Images. Mithilfe der Tokens wird Lesezugriff auf alle Namensbereiche autorisiert, die Sie in {{site.data.keyword.registryshort_notm}} einrichten, sodass Sie mit diesen öffentlichen Images (in globalen Registrys) und privaten Images (in regionalen Registrys) arbeiten können.

Jedes Token muss in einem geheimen Schlüssel für Image-Pull-Operationen (`imagePullSecret`) von Kubernetes gespeichert sein, damit ein Kubernetes-Cluster darauf zugreifen kann, wenn Sie eine containerisierte App bereitstellen. Bei der Erstellung Ihres Clusters werden die Tokens für die globale Registry (mit den von IBM bereitgestellten öffentlichen Images) von {{site.data.keyword.containerlong_notm}} automatisch in geheimen Schlüsseln für Image-Pull-Operationen (imagePullSecrets) in Kubernetes gespeichert. Die geheimen Schlüssel für Image-Pull-Operationen werden dem Kubernetes-Namensbereich `default`, dem Namensbereich `kube-system` und der Liste der geheimen Schlüssel im Servicekonto `default` für diese Namensbereiche hinzugefügt.

Diese Methode der Verwendung eines Tokens für die Autorisierung des Clusterzugriffs auf {{site.data.keyword.registrylong_notm}} wird für Domänennamen mit `registry.bluemix.net` unterstützt, ist jedoch veraltet. [Verwenden Sie stattdessen die Methode mit dem API-Schlüssel](#cluster_registry_auth), um den Clusterzugriff auf die neuen Registry-Domänennamen mit `icr.io` zu autorisieren.
{: deprecated}

Abhängig von der Position des Image und der Position des Containers müssen Sie Container durch die folgenden unterschiedlichen Schritte bereitstellen.
*   [Container im Kubernetes-Namensbereich `default` mit einem Image bereitstellen, das sich in derselben Region wie Ihr Cluster befindet](#token_default_namespace)
*   [Container in einem anderen Kubernetes-Namensbereich als `default` bereitstellen](#token_copy_imagePullSecret)
*   [Container mit einem Image bereitstellen, das sich in einer anderen Region oder einem anderen {{site.data.keyword.Bluemix_notm}}-Konto als Ihr Cluster befindet](#token_other_regions_accounts)
*   [Container mit einem Image bereitstellen, das aus einer privaten, nicht von IBM bereitgestellten Registry kommt](#private_images)

Bei dieser anfänglichen Konfiguration können Sie Container aus allen Images, die in einem Namensbereich in Ihrem {{site.data.keyword.Bluemix_notm}}-Konto verfügbar sind, im Namenbereich **default** Ihres Clusters bereitstellen. Um einen Container in anderen Namensbereichen Ihres Clusters bereitstellen oder ein Image verwenden zu können, das in einer anderen {{site.data.keyword.Bluemix_notm}}-Region oder in einem anderen {{site.data.keyword.Bluemix_notm}}-Konto gespeichert ist, müssen Sie einen [eigenen geheimen Schlüssel für Image-Pull-Operationen für Ihren Cluster erstellen](#other).
{: note}

### Veraltet: Images im Kubernetes-Namensbereich `default` mit einem Registry-Token bereitstellen
{: #token_default_namespace}

Mit einem Registry-Token, das im geheimen Schlüssel für Image-Pull-Operationen gespeichert ist, können Sie einen Container aus einem beliebigen Image, das in Ihrer regionalen {{site.data.keyword.registrylong_notm}} verfügbar ist, im Namensbereich **default** Ihres Clusters bereitstellen.
{: shortdesc}

Vorbereitende Schritte:
1. [Richten Sie einen Namensbereich in {{site.data.keyword.registryshort_notm}} ein und übertragen Sie Images per Push-Operation an diesen Namensbereich](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2. [Erstellen Sie einen Cluster](/docs/containers?topic=containers-clusters#clusters_cli).
3. [Geben Sie als Ziel Ihrer CLI Ihren Cluster an](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Um einen Container im Standardnamensbereich (**default**) Ihres Clusters bereitzustellen, müssen Sie eine Konfigurationsdatei erstellen.

1.  Erstellen Sie eine Bereitstellungskonfigurationsdatei mit dem Namen `mydeployment.yaml`.
2.  Definieren Sie die Bereitstellung und das Image aus Ihrem Namensbereich, das Sie verwenden möchten, in {{site.data.keyword.registryshort_notm}}.

    Gehen Sie wie folgt vor, um ein privates Image aus einem Namensbereich in {{site.data.keyword.registryshort_notm}} zu verwenden:

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <app-name>-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: <app-name>
      template:
        metadata:
          labels:
            app: <app-name>
        spec:
          containers:
          - name: <app-name>
            image: registry.<region>.bluemix.net/<namensbereich>/<mein_image>:<tag>
    ```
    {: codeblock}

    **Tipp:** Führen Sie `ibmcloud cr namespace-list` aus, um Ihre Namensbereichsinformationen abzurufen.

3.  Erstellen Sie die Bereitstellung in Ihrem Cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Tipp:** Sie können auch eine vorhandene Konfigurationsdatei bereitstellen, z. B. eines der von IBM bereitgestellten öffentlichen Images. In diesem Beispiel wird das Image **ibmliberty** in der Region 'Vereinigte Staaten (Süden)' verwendet.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}

### Veraltet: Tokenbasierten geheimen Schlüssel für Image-Pull-Operationen aus dem Namenbereich 'default' in andere Namensbereiche in Ihrem Cluster kopieren
{: #token_copy_imagePullSecret}

Sie können den geheimen Schlüssel für Image-Pull-Operationen mit Registry-Token-Berechtigungsnachweisen, der automatisch für den Kubernetes-Namensbereich `default` erstellt wird, in andere Namensbereiche in Ihrem Cluster kopieren.
{: shortdesc}

1. Listen Sie verfügbare Namensbereiche in Ihrem Cluster auf.
   ```
   kubectl get namespaces
   ```
   {: pre}

   Beispielausgabe:
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. Optional: Erstellen Sie einen Namensbereich in Ihrem Cluster.
   ```
   kubectl create namespace <name_des_namensbereichs>
   ```
   {: pre}

3. Kopieren Sie die geheimen Schlüssel für Image-Pull-Operationen aus dem Namensbereich `default` in den Namensbereich Ihrer Wahl. Die neuen geheimen Schlüssel für Image-Pull-Operationen erhalten Namen der Form `bluemix-<name_des_namensbereichs>-secret-regional` und `bluemix-<name_des_namensbereichs>-secret-international`.
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<name_des_namensbereichs>/g' | kubectl -n <name_des_namensbereichs> create -f -
   ```
   {: pre}

   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<name_des_namensbereichs>/g' | kubectl -n <name_des_namensbereichs> create -f -
   ```
   {: pre}

4.  Überprüfen Sie, ob die geheimen Schlüssel erfolgreich erstellt wurden.
    ```
    kubectl get secrets --namespace <name_des_namensbereichs>
    ```
    {: pre}

5. [Stellen Sie einen Container mit dem geheimen Schlüssel für Image-Pull-Operationen (`imagePullSecret`](#use_imagePullSecret)) in Ihrem Namensbereich bereit.


### Veraltet: Tokenbasierten geheimen Schlüssel für Image-Pull-Operationen für den Zugriff auf Images in anderen {{site.data.keyword.Bluemix_notm}}-Regionen und -Konten erstellen
{: #token_other_regions_accounts}

Um auf Images in anderen {{site.data.keyword.Bluemix_notm}}-Regionen oder -Konten zuzugreifen, müssen Sie ein Registry-Token erstellen und Ihre Berechtigungsnachweise in einem geheimen Schlüssel für Image-Pull-Operationen speichern.
{: shortdesc}

1.  Falls Sie kein Token haben, [erstellen Sie ein Token für die Registry, auf die Sie zugreifen möchten.](/docs/services/Registry?topic=registry-registry_access#registry_tokens_create)
2.  Listen Sie die Tokens in Ihrem {{site.data.keyword.Bluemix_notm}}-Konto auf.

    ```
    ibmcloud cr token-list
    ```
    {: pre}

3.  Notieren Sie sich die Token-ID, die Sie verwenden möchten.
4.  Rufen Sie den Wert für Ihr Token ab. Ersetzen Sie <em>&lt;token-id&gt;</em> durch die ID des Tokens, das sie im vorherigen Schritt abgerufen haben.

    ```
    ibmcloud cr token-get <token-id>
    ```
    {: pre}

    Ihr Tokenwert wird im Feld **Token** Ihrer CLI-Ausgabe angezeigt.

5.  Erstellen Sie den geheimen Kubernetes-Schlüssel, um Ihre Tokeninformationen zu speichern.

    ```
    kubectl --namespace <kubernetes-namensbereich> create secret docker-registry <name_des_geheimen_schlüssels>  --docker-server=<registry-url> --docker-username=token --docker-password=<tokenwert> --docker-email=<docker-e-mail>
    ```
    {: pre}

    <table>
    <caption>Erklärung der Bestandteile dieses Befehls</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Bestandteile dieses Befehls</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes-namensbereich&gt;</em></code></td>
    <td>Erforderlich. Der Kubernetes-Namensbereich Ihres Clusters, in dem Sie den geheimen Schlüssel verwenden und Container bereitstellen möchten. Führen Sie <code>kubectl get namespaces</code> aus, um alle Namensbereiche in Ihrem Cluster aufzulisten.</td>
    </tr>
    <tr>
    <td><code><em>&lt;name_des_geheimen_schlüssels&gt;</em></code></td>
    <td>Erforderlich. Der Name, den Sie für Ihren geheimen Schlüssel für Image-Pull-Operationen verwenden wollen.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry-url&gt;</em></code></td>
    <td>Erforderlich. Die URL der Image-Registry, in der Ihr Namensbereich eingerichtet ist.<ul><li>Für Namensbereiche, die in den Regionen 'Vereinigte Staaten (Süden)' und 'Vereinigte Staaten (Osten)' eingerichtet sind: <code>registry.ng.bluemix.net</code></li><li>Für Namensbereiche, die in der Region 'Vereinigtes Königreich (Süden)' eingerichtet sind: <code>registry.eu-gb.bluemix.net</code></li><li>Für Namensbereiche, die in der Region 'Mitteleuropa (Frankfurt)' eingerichtet sind: <code>registry.eu-de.bluemix.net</code></li><li>Für Namensbereiche, die in der Region 'Australien (Sydney)' eingerichtet sind: <code>registry.au-syd.bluemix.net</code></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker-benutzername&gt;</em></code></td>
    <td>Erforderlich. Der Benutzername für die Anmeldung bei Ihrer privaten Registry. Für {{site.data.keyword.registryshort_notm}} wird der Benutzername auf den Wert <strong><code>token</code></strong> gesetzt.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;tokenwert&gt;</em></code></td>
    <td>Erforderlich. Der Wert des zuvor abgerufenen Registry-Tokens.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-e-mail&gt;</em></code></td>
    <td>Erforderlich. Falls Sie über eine Docker-E-Mail-Adresse verfügen, geben Sie diese ein. Ist dies nicht der Fall, geben Sie eine fiktive E-Mail-Adresse ein, z. B. a@b.c. Die Angabe der E-Mail-Adresse ist obligatorisch, um einen geheimen Kubernetes-Schlüssel zu erstellen, wird aber nach der Erstellung nicht weiter genutzt.</td>
    </tr>
    </tbody></table>

6.  Überprüfen Sie, ob der geheime Schlüssel erfolgreich erstellt wurde. Ersetzen Sie <em>&lt;kubernetes-namensbereich&gt;</em> durch den Namensbereich, in dem Sie den geheimen Schlüssel für Image-Pull-Operationen erstellt haben.

    ```
    kubectl get secrets --namespace <kubernetes-namensbereich>
    ```
    {: pre}

7.  [Stellen Sie einen Container mit dem geheimen Schlüssel für Image-Pull-Operationen](#use_imagePullSecret) in Ihrem Namensbereich bereit.
