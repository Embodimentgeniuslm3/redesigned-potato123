---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-05"

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





# {{site.data.keyword.registryshort_notm}}-Token für eine {{site.data.keyword.Bluemix_dedicated_notm}}-Image-Registry erstellen
{: #cs_dedicated_tokens}

Erstellen Sie ein Token ohne Ablaufdatum für eine Image-Registry, die Sie für einzelne und skalierbare Gruppen mit Clustern in {{site.data.keyword.containerlong}} verwendet haben.
{:shortdesc}

1.  Fordern Sie ein permanentes Registry-Token für die aktuelle Sitzung an. Dieses Token gewährt Zugriff auf die Images im aktuellen Namensbereich.
    ```
    ibmcloud cr token-add --description "<beschreibung>" --non-expiring -q
    ```
    {: pre}

2.  Überprüfen Sie den geheimen Kubernetes-Schlüssel.

    ```
    kubectl describe secrets
    ```
    {: pre}

    Sie können diesen geheimen Schlüssel verwenden, um mit {{site.data.keyword.containerlong}} zu arbeiten.

3.  Erstellen Sie den geheimen Kubernetes-Schlüssel, um Ihre Tokeninformationen zu speichern.

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
    <td><code>--namespace &lt;kubernetes-namensbereich&gt;</code></td>
    <td>Erforderlich. Der Kubernetes-Namensbereich Ihres Clusters, in dem Sie den geheimen Schlüssel verwenden und Container bereitstellen möchten. Führen Sie <code>kubectl get namespaces</code>, um alle Namensbereiche in Ihrem Cluster aufzulisten.</td>
    </tr>
    <tr>
    <td><code>&lt;name_des_geheimen_schlüssels&gt;</code></td>
    <td>Erforderlich. Der Name, den Sie für Ihr imagePullSecret verwenden möchten.</td>
    </tr>
    <tr>
    <td><code>--docker-server=&lt;registry_url&gt;</code></td>
    <td>Erforderlich. Die URL der Image-Registry, in der Ihr Namensbereich eingerichtet ist: <code>Registry.&lt;dedizierte_domäne&gt;</code></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username=token</code></td>
    <td>Erforderlich. Ändern Sie diesen Wert nicht.</td>
    </tr>
    <tr>
    <td><code>--docker-password=&lt;token_value&gt;</code></td>
    <td>Erforderlich. Der Wert des zuvor abgerufenen Registry-Tokens.</td>
    </tr>
    <tr>
    <td><code>--docker-email=&lt;docker-email&gt;</code></td>
    <td>Erforderlich. Falls Sie über eine Docker-E-Mail-Adresse verfügen, geben Sie diese ein. Ist dies nicht der Fall, geben Sie eine fiktive E-Mail-Adresse ein, z. B. a@b.c. Die Angabe der E-Mail-Adresse ist obligatorisch, um einen geheimen Kubernetes-Schlüssel zu erstellen, wird aber nach der Erstellung nicht weiter genutzt.</td>
    </tr>
    </tbody></table>

4.  Erstellen Sie einen Pod, der das imagePullSecret referenziert.

    1.  Öffnen Sie Ihren bevorzugten Texteditor und erstellen Sie ein Podkonfigurationsscript namens 'mypod.yaml'.
    2.  Definieren Sie den Pod und das imagePullSecret, das Sie für den Zugriff auf die Registry verwenden möchten. Gehen Sie wie folgt vor, um ein privates Image aus einem Namensbereich zu verwenden:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <podname>
        spec:
          containers:
            - name: <containername>
              image: registry.<dedizierte_domäne>/<mein_namensbereich>/<mein_image>:<tag>
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
        <td><code>&lt;podname&gt;</code></td>
        <td>Der Name des Pod, den Sie erstellen möchten.</td>
        </tr>
        <tr>
        <td><code>&lt;containername&gt;</code></td>
        <td>Der Name des Containers, den Sie in Ihrem Cluster bereitstellen möchten.</td>
        </tr>
        <tr>
        <td><code>&lt;mein_namensbereich&gt;</code></td>
        <td>Der Namensbereich, in dem das Image gespeichert ist. Führen Sie den Befehl `ibmcloud cr namespace-list` aus, um die verfügbaren Namensbereiche aufzulisten.</td>
        </tr>
        <td><code>&lt;mein_image&gt;</code></td>
        <td>Der Name des Images, das Sie verwenden möchten. Führen Sie den Befehl <code>ibmcloud cr image-list</code> aus, um die verfügbaren Images in einem {{site.data.keyword.Bluemix_notm}}-Konto aufzulisten.</td>
        </tr>
        <tr>
        <td><code>&lt;tag&gt;</code></td>
        <td>Die Version des Images, das Sie verwenden möchten. Ist kein Tag angegeben, wird standardmäßig das Image mit dem Tag <strong>latest</strong> verwendet.</td>
        </tr>
        <tr>
        <td><code>&lt;name_des_geheimen_schlüssels&gt;</code></td>
        <td>Der Name des imagePullSecret, das Sie zuvor erstellt haben.</td>
        </tr>
        </tbody></table>

    3.  Speichern Sie Ihre Änderungen.

    4.  Erstellen Sie die Bereitstellung in Ihrem Cluster.

          ```
          kubectl apply -f mypod.yaml -n <namespace>
          ```
          {: pre}
