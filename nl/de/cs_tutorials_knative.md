---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

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


# Lernprogramm: Verwaltetes Knative-Add-on zum Ausführen serverunabhängiger Apps in Kubernetes-Clustern verwenden
{: #knative_tutorial}

Mithilfe dieses Lernprogramms können Sie sich mit der Installation von Knative in einem Kubernetes-Cluster in {{site.data.keyword.containerlong_notm}} vertraut machen.
{: shortdesc}

**Was ist Knative und wozu wird es verwendet?**</br>
[Knative](https://github.com/knative/docs) ist eine Open-Source-Plattform, die von IBM, Google, Pivotal, Red Hat, Cisco und anderen dazu entwickelt wurde, die Funktionalität von Kubernetes zu erweitern, um Sie bei der Erstellung moderner, quellenzentrierter, containerisierter und serverunabhängiger Apps auf der Basis eines Kubernetes-Clusters zu unterstützen. Die Plattform ist auf die Anforderungen von Entwicklern ausgerichtet, die heute entscheiden müssen, welchen Typ von App sie in der Cloud ausführen wollen: 12-Faktoren-Apps, Container oder Funktionen. Jeder Typ von App erfordert eine Open-Source-Lösung oder eine proprietäre Lösung, die auf diese Apps zugeschnitten ist: Cloud Foundry für 12-Faktoren-Apps, Kubernetes für Container sowie OpenWhisk und andere für Funktionen. In der Vergangenheit mussten Entwickler entscheiden, welchem Ansatz sie folgen wollten, was sich als unflexibel und komplex erwies, wenn unterschiedliche Typen von Apps kombiniert werden mussten.  

Knative nutzt ein konsistentes, Programmiersprachen und Frameworks übergreifendes Konzept, um den Betriebsaufwand für die Builderstellung, Bereitstellung und Verwaltung von Workloads in Kubernetes zusammenzufassen, sodass Entwickler sich auf das Wichtigste konzentrieren können: den Quellcode. Sie können bewährte Build-Packs verwenden, mit denen Sie bereits vertraut sind, wie zum Beispiel Cloud Foundry, Kaniko, Dockerfile, Bazel und andere. Durch die Integration in Istio stellt Knative sicher, dass sich Ihre serverunabhängigen und containerisierten Workloads problemlos über das Internet zugänglich machen, überwachen und steuern lassen und dass Ihre Daten bei der Übertragung verschlüsselt werden.

**Wie funktioniert Knative?**</br>
Knative wird mit drei Hauptkomponenten oder _Basiskomponenten_ ('Primitives') bereitgestellt, die Sie beim Erstellen (Build), Bereitstellen (Deploy) und Verwalten Ihrer serverunabhängigen Apps in Ihrem Kubernetes-Cluster unterstützen.

- **Build:** Die Basiskomponente `Build` unterstützt das Erstellen einer Reihe von Schritten, um Ihre App aus Quellcode als Container-Image zu erstellen. Stellen Sie sich vor, Sie verwenden eine einfache Buildvorlage, in der Sie das Quellenrepository zum Auffinden Ihres App-Codes und die gewünschte Container-Registry für das Hosting des Image angeben. Durch einen einzigen Befehl können Sie Knative anweisen, diese Buildvorlage zu verwenden, den Quellcode zu extrahieren, das Image zu erstellen und das Image durch eine Push-Operation in Ihre Container-Registry einzufügen, sodass Sie das Image in Ihrem Container verwenden können.
- **Serving:** Die Basiskomponente `Serving` unterstützt Sie bei der Bereitstellung serverunabhängiger Apps als Knative-Services sowie bei der automatischen Skalierung dieser Services, sogar bis auf null Instanzen herab. Durch die Nutzung der Istio-Funktionalität für Datenverkehrsmanagement und intelligentes Routing können Sie steuern, welcher Datenverkehr an eine bestimmte Version Ihres Service geleitet wird, sodass ein Entwickler eine neue App-Version ohne großen Aufwand testen und implementieren kann oder A-B-Tests durchführen kann.
- **Eventing:** Mit der Basiskomponente `Eventing` können Sie Auslöser (Trigger) oder Ereignisströme erstellen, die andere Services abonnieren können. Sie wollen zum Beispiel immer dann einen neuen Build Ihrer App starten, wenn Code durch eine Push-Operation in Ihr GitHub-Master-Repository übertragen wird. Oder Sie möchten eine serverunabhängige App nur ausführen, wenn die Temperatur unter den Gefrierpunkt fällt. Die Basiskomponente `Eventing` kann in Ihre CI/CD-Pipeline integriert werden, um den Build und die Bereitstellung von Apps zu automatisieren, wenn ein bestimmtes Ereignis auftritt.

**Was ist das (experimentelle) Managed Knative on {{site.data.keyword.containerlong_notm}}-Add-on?** </br>
Managed Knative on {{site.data.keyword.containerlong_notm}} ist ein verwaltetes Add-on, das Knative und Istio direkt in Ihren Kubernetes-Cluster integriert. Die Knative- und die Istio-Version in dem Add-on werden von IBM getestet und für die Verwendung in {{site.data.keyword.containerlong_notm}} unterstützt. Weitere Informationen zu verwalteten Add-ons finden Sie unter [Services unter Verwendung verwalteter Add-ons hinzufügen](/docs/containers?topic=containers-managed-addons#managed-addons).

**Gibt es Einschränkungen?** </br>
Wenn Sie den [Zugangscontroller zur Durchsetzung der Sicherheit von Container-Images](/docs/services/Registry?topic=registry-security_enforce#security_enforce) in Ihrem Cluster installiert haben, können Sie das verwaltete Knative-Add-on in Ihrem Cluster nicht aktivieren.

Klingt das für Sie interessant? Arbeiten Sie dieses Lernprogramm durch, um in die Verwendung von Knative in {{site.data.keyword.containerlong_notm}} einzusteigen.

## Ziele
{: #knative_objectives}

- Sie machen sich mit den Grundlagen von Knative und mit den Knative-Basiskomponenten (Primitives) vertraut.  
- Sie installieren das verwaltete Knative-Add-on und das verwaltete Istio-Add-on in Ihrem Cluster.
- Sie stellen Ihre erste serverunabhängige App mit Knative bereit und machen die App über das Internet mithilfe der Knative-Basiskomponente `Serving` zugänglich.
- Sie erkunden die Skalierungs- und Revisionsfunktionen von Knative.

## Erforderlicher Zeitaufwand
{: #knative_time}

30 Minuten

## Zielgruppe
{: #knative_audience}

Dieses Lernprogramm wurde für Entwickler konzipiert, die sich informieren möchten, wie eine serverunabhängige App mithilfe von Knative in einem Kubernetes-Cluster bereitgestellt wird. Clusteradministratoren können sich damit vertraut machen, wie Knative in einem Cluster eingerichtet wird.

## Voraussetzungen
{: #knative_prerequisites}

-  [Installieren Sie die IBM Cloud-CLI, das {{site.data.keyword.containerlong_notm}}-Plug-in und die Kubernetes-CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps). Stellen Sie sicher, dass Sie die CLI-Version von `kubectl` installieren, die der Kubernetes-Version Ihres Clusters entspricht.
-  [Erstellen Sie einen Cluster mit mindestens drei Workerknoten, die jeweils vier Cores und 16 GB Hauptspeicher (`b3c.4x16`) oder mehr haben](/docs/containers?topic=containers-clusters#clusters_cli). Auf jedem Workerknoten muss Kubernetes Version 1.12 oder höher ausgeführt werden.
-  Stellen Sie sicher, dass Sie über die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Schreibberechtigter** oder **Manager**](/docs/containers?topic=containers-users#platform) für {{site.data.keyword.containerlong_notm}} verfügen.
-  [Definieren Sie Ihren Cluster als Ziel der CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

## Lerneinheit 1: Verwaltetes Knative-Add-on einrichten
{: #knative_setup}

Knative baut auf Istio auf, um sicherzustellen, dass Ihre serverunabhängigen und containerisierten Workloads im Cluster und über das Internet zugänglich gemacht werden können. Mit Istio können Sie den Netzverkehr zwischen Ihren Services auch überwachen und steuern sowie sicherstellen, dass Ihre Daten während der Übertragung verschlüsselt sind. Wenn Sie das verwaltete Knative-Add-on installieren, wird das verwaltete Istio-Add-on automatisch mitinstalliert.
{: shortdesc}

1. Aktivieren Sie das verwaltete Knative-Add-on in Ihrem Cluster. Wenn Sie Knative in Ihrem Cluster aktivieren, werden Istio und alle Knative-Komponenten in Ihrem Cluster installiert.
   ```
   ibmcloud ks cluster-addon-enable knative --cluster <clustername_oder_-id> -y
   ```
   {: pre}

   Beispielausgabe:
   ```
   Enabling add-on knative for cluster knative...
   OK
   ```
   {: screen}

   Die Installation aller Knative-Komponenten kann einige Minuten dauern.

2. Überprüfen Sie, ob Istio erfolgreich installiert wurde. Alle Pods für die neun Istio-Services und der Pod für Prometheus müssen den Status `Running` (Aktiv) haben.
   ```
   kubectl get pods --namespace istio-system
   ```
   {: pre}

   Beispielausgabe:
   ```
   NAME                                       READY     STATUS      RESTARTS   AGE
    istio-citadel-748d656b-pj9bw               1/1       Running     0          2m
    istio-egressgateway-6c65d7c98d-l54kg       1/1       Running     0          2m
    istio-galley-65cfbc6fd7-bpnqx              1/1       Running     0          2m
    istio-ingressgateway-f8dd85989-6w6nj       1/1       Running     0          2m
    istio-pilot-5fd885964b-l4df6               2/2       Running     0          2m
    istio-policy-56f4f4cbbd-2z2bk              2/2       Running     0          2m
    istio-sidecar-injector-646655c8cd-rwvsx    1/1       Running     0          2m
    istio-statsd-prom-bridge-7fdbbf769-8k42l   1/1       Running     0          2m
    istio-telemetry-8687d9d745-mwjbf           2/2       Running     0          2m
    prometheus-55c7c698d6-f4drj                1/1       Running     0          2m
   ```
   {: screen}

3. Optional: Wenn Sie Istio für alle Apps im Standardnamensbereich (`default`) verwenden möchten, fügen Sie dem Namensbereich die Bezeichnung `istio-injection=enabled` hinzu. Jeder serverunabhängige App-Pod muss einen Envoy-Prox-Sidecar ausführen, sodass die App in das Istio-Servicenetz eingeschlossen werden kann. Diese Bezeichnung ermöglicht Istio das automatische Ändern der Pod-Vorlagenspezifikation bei neuen App-Bereitstellungen, sodass Pods mit Envoy-Proxy-Sidecar-Containern erstellt werden.
  ```
  kubectl label namespace default istio-injection=enabled
  ```
  {: pre}

4. Überprüfen Sie, ob alle Knative-Komponenten erfolgreich installiert wurden.
   1. Überprüfen Sie, ob alle Pods der Knative-Komponente `Serving` den Status `Running` (Aktiv) haben.  
      ```
      kubectl get pods --namespace knative-serving
      ```
      {: pre}

      Beispielausgabe:
      ```
      NAME                          READY     STATUS    RESTARTS   AGE
      activator-598b4b7787-ps7mj    2/2       Running   0          21m
      autoscaler-5cf5cfb4dc-mcc2l   2/2       Running   0          21m
      controller-7fc84c6584-qlzk2   1/1       Running   0          21m
      webhook-7797ffb6bf-wg46v      1/1       Running   0          21m
      ```
      {: screen}

   2. Überprüfen Sie, ob alle Pods der Knative-Komponente `Build` den Status `Running` (Aktiv) haben.  
      ```
      kubectl get pods --namespace knative-build
      ```
      {: pre}

      Beispielausgabe:
      ```
      NAME                                READY     STATUS    RESTARTS   AGE
      build-controller-79cb969d89-kdn2b   1/1       Running   0          21m
      build-webhook-58d685fc58-twwc4      1/1       Running   0          21m
      ```
      {: screen}

   3. Überprüfen Sie, ob alle Pods der Knative-Komponente `Eventing` den Status `Running` (Aktiv) haben.
      ```
      kubectl get pods --namespace knative-eventing
      ```
      {: pre}

      Beispielausgabe:

      ```
      NAME                                            READY     STATUS    RESTARTS   AGE
      eventing-controller-847d8cf969-kxjtm            1/1       Running   0          22m
      in-memory-channel-controller-59dd7cfb5b-846mn   1/1       Running   0          22m
      in-memory-channel-dispatcher-67f7497fc-dgbrb    2/2       Running   1          22m
      webhook-7cfff8d86d-vjnqq                        1/1       Running   0          22m
      ```
      {: screen}

   4. Überprüfen Sie, ob alle Pods der Knative-Komponente `Sources` (Quellen) den Status `Running` (Aktiv) haben.
      ```
      kubectl get pods --namespace knative-sources
      ```
      {: pre}

      Beispielausgabe:
      ```
      NAME                   READY     STATUS    RESTARTS   AGE
      controller-manager-0   1/1       Running   0          22m
      ```
      {: screen}

   5. Überprüfen Sie, ob alle Pods der Knative-Komponente `Monitoring` (Überwachung) den Status `Running` (Aktiv) haben.
      ```
      kubectl get pods --namespace knative-monitoring
      ```
      {: pre}

      Beispielausgabe:
      ```
      NAME                                  READY     STATUS                 RESTARTS   AGE
      elasticsearch-logging-0               1/1       Running                0          22m
      elasticsearch-logging-1               1/1       Running                0          21m
      grafana-79cf95cc7-mw42v               1/1       Running                0          21m
      kibana-logging-7f7b9698bc-m7v6r       1/1       Running                0          22m
      kube-state-metrics-768dfff9c5-fmkkr   4/4       Running                0          21m
      node-exporter-dzlp9                   2/2       Running                0          22m
      node-exporter-hp6gv                   2/2       Running                0          22m
      node-exporter-hr6vs                   2/2       Running                0          22m
      prometheus-system-0                   1/1       Running                0          21m
      prometheus-system-1                   1/1       Running                0          21m
      ```
      {: screen}

Super! Knative und Istio sind jetzt vollständig eingerichtet. Daher können Sie nun Ihre erste serverunabhängige App auf dem Cluster bereitstellen.

## Lerneinheit 2: Serverunabhängige App auf dem Cluster bereitstellen
{: #deploy_app}

In dieser Lerneinheit stellen Sie Ihre erste serverunabhängige App [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) in Go bereit. Wenn Sie eine Anforderung an Ihre Beispielapp senden, liest die App die Umgebungsvariable `TARGET` und gibt `"Hello ${TARGET}!"` aus. Ist diese Umgebungsvariable leer, wird `"Hello World!"` zurückgegeben.
{: shortdesc}

1. Erstellen Sie eine YAML-Datei für Ihre erste serverunabhängige App `Hello World` in Knative. Zum Bereitstellen einer App mit Knative müssen Sie eine Knative-Serviceressource angeben. Ein Service wird von der Knative-Basiskomponente `Serving` verwaltet und ist für die Verwaltung des gesamten Lebenszyklus der Workload verantwortlich. Der Service stellt sicher, dass jede Bereitstellung eine Knative-Revision, eine Route und eine Konfiguration hat. Wenn Sie den Service aktualisieren, wird eine neue Version der App erstellt und dem Revisionsprotokoll des Service hinzugefügt. Knative-Routen stellen sicher, dass jede Revision der App einem Netzendpunkt zugeordnet wird, sodass Sie die Weiterleitung (Routing) von Netzverkehr an eine bestimmte Revision steuern können. Knative-Konfigurationen enthalten die Einstellungen einer bestimmten Revision, sodass immer ein Rollback auf eine ältere Revision oder ein Wechsel zwischen Revisionen möglich ist. Weitere Informationen zu Knative-Ressourcen der Komponente `Serving` finden Sie in der [Knative-Dokumentation](https://github.com/knative/docs/tree/master/serving).
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Go Sample v1"
    ```
    {: codeblock}

    <table>
    <caption>Erklärung der Komponenten der YAML-Datei</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>Der Name Ihres Knative-Service.</td>
    </tr>
    <tr>
    <td><code>metadata.namespace</td>
    <td>Der Kubernetes-Namensbereich, in dem Sie Ihre App als Knative-Service bereitstellen wollen. </td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>Die URL zu der Container-Registry, in der Ihr Image gespeichert ist. In diesem Beispiel stellen Sie eine Knative-App 'Hello World' bereit, die im Namensbereich <code>ibmcom</code> in Docker Hub gespeichert ist. </td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>Eine Liste der Umgebungsvariablen, die Ihr Knative-Service haben soll. In diesem Beispiel wird der Wert der Umgebungsvariablen <code>TARGET</code> von der Beispielapp gelesen und zurückgegeben, wenn Sie eine Anforderung an Ihre App im Format <code>"Hello ${TARGET}!"</code> senden. Wenn kein Wert angegeben ist, gibt die Beispielapp den Wert <code>"Hello World!"</code> zurück.  </td>
    </tr>
    </tbody>
    </table>

2. Erstellen Sie den Knative-Service in Ihrem Cluster. Wenn Sie den Service erstellen, erstellt die Knative-Basiskomponente `Serving` eine unveränderliche Revision, eine Knative-Route, eine Ingress-Routing-Regel, einen Kubernetes-Service, einen Kubernetes-Pod und eine Lastausgleichsfunktion (Load Balancer) für Ihre App. Ihrer App wird eine Unterdomäne aus Ihrer Ingress-Unterdomäne im Format `<knative-servicename>.<namensbereich>.<ingress-unterdomäne>` zugeordnet, durch die Sie über das Internet auf die App zugreifen können.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   Beispielausgabe:
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. Überprüfen Sie, ob Ihr Pod erstellt wurde. Ihr Pod besteht aus zwei Containern. Der eine Container führt Ihre App `Hello World` aus und der andere Container ist ein Sidecar, der Istio- und Knative-Tools zur Überwachung und Protokollierung ausführt. Ihrem Pod wurde die Revisionsnummer `00001` zugeordnet.
   ```
   kubectl get pods
   ```
   {: pre}

   Beispielausgabe:
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00001-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
   ```
   {: screen}

4. Probieren Sie Ihre App `Hello World` aus.
   1. Rufen Sie die Standarddomäne ab, die Ihrem Knative-Service zugeordnet ist. Wenn Sie den Namen Ihres Knative-Service geändert oder die App in einem anderen Namensbereich bereitgestellt haben, aktualisieren Sie diese Werte in Ihrer Abfrage.
      ```
      kubectl get ksvc/kn-helloworld
      ```
      {: pre}

      Beispielausgabe:
      ```
      NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
      kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-rjmwt   kn-helloworld-rjmwt   True
      ```
      {: screen}

   2. Senden Sie eine Anforderung an Ihre App, indem Sie die Unterdomäne verwenden, die Sie im vorherigen Schritt abgerufen haben.
      ```
      curl -v <servicedomäne>
      ```
      {: pre}

      Beispielausgabe:
      ```
      * Rebuilt URL to: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud/
      *   Trying 169.46.XX.XX...
      * TCP_NODELAY set
      * Connected to kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud (169.46.XX.XX) port 80 (#0)
      > GET / HTTP/1.1
      > Host: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud
      > User-Agent: curl/7.54.0
      > Accept: */*
      >
      < HTTP/1.1 200 OK
      < Date: Thu, 21 Mar 2019 01:12:48 GMT
      < Content-Type: text/plain; charset=utf-8
      < Content-Length: 20
      < Connection: keep-alive
      < x-envoy-upstream-service-time: 17
      <
      Hello Go Sample v1!
      * Connection #0 to host kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud left intact
      ```
      {: screen}

5. Warten Sie einige Minuten, um Knative ein Scale-down Ihrer Pods durchführen zu lassen. Knative berechnet die Anzahl der Pods, die gleichzeitig betriebsbereit sein müssen, um eingehende Workloads verarbeiten zu können. Wenn kein Netzverkehr empfangen wird, verringert Knative die Anzahl Ihrer Pods durch automatische Scale-downs sogar bis auf null Pods, wie in diesem Beispiel gezeigt.

   Möchten Sie sehen, wie Knative ein Scale-up Ihrer Pods durchführt? Versuchen Sie, die Workload für Ihre App zu erhöhen, indem Sie zum Beispiel Tools wie das [einfache Testprogramm für cloudbasierte Auslastungen](https://loader.io/) verwenden.
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   Wenn keine Pods `kn-helloworld` angezeigt werden, hat Knative die Anzahl der Pods Ihrer App durch Scale-down auf null reduziert.

6. Aktualisieren Sie Ihr Knative-Servicebeispiel und geben Sie einen anderen Wert für die Umgebungsvariable `TARGET` an.

   YAML-Beispiel für den Service:
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Mr. Smith"
    ```
    {: codeblock}

7. Wenden Sie die Änderung auf Ihren Service an. Wenn Sie die Konfiguration ändern, erstellt Knative automatisch eine neue Revision, ordnet eine neue Route zu und weist Istio standardmäßig an, eingehenden Netzverkehr an die letzte Revision zu leiten.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

8. Senden Sie eine neue Anforderung an Ihr App, um zu prüfen, ob Ihre Änderung angewendet wurde.
   ```
   curl -v <servicedomäne>
   ```

   Beispielausgabe:
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

9. Überprüfen Sie, ob Knative die Anzahl Ihrer Pods durch Scale-up wieder erhöht hat, um dem erhöhten Netzverkehr Rechnung zu tragen. Ihrem Pod wurde die Revisionsnummer `00002` zugeordnet. Durch die Revisionsnummer können Sie auf eine bestimmte Version Ihrer App verweisen, zum Beispiel wenn Sie Istio anweisen wollen, eingehenden Datenverkehr zwischen zwei Revisionen aufzuteilen.
   ```
   kubectl get pods
   ```

   Beispielausgabe:
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00002-deployment-55db6bf4c5-2vftm   3/3       Running   0          16s
   ```
   {: screen}

10. Optional: Bereinigen Sie Ihren Knative-Service.
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}

Klasse! Sie haben Ihre erste Knative-App erfolgreich in Ihrem Cluster bereitgestellt und die Revisions- und Skalierungsfunktionalität der Knative-Basiskomponente `Serving` ausprobiert.


## Womit möchten Sie fortfahren?   
{: #whats-next}

- Probieren Sie diesen [Knative-Workshop ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/IBM/knative101/tree/master/workshop) aus, um Ihre erste `Node.js`-Fibonacci-App in Ihrem Cluster bereitzustellen.
  - Erkunden Sie, wie Sie mit der Knative-Basiskomponente `Build` ein Image aus einer Dockerfile in GitHub erstellen und das Image automatisch durch eine Push-Operation in Ihren Namensbereich in {{site.data.keyword.registrylong_notm}} übertragen.  
  - Erfahren Sie, wie Sie das Routing für Netzverkehr aus der von IBM zur Verfügung gestellten Ingress-Domäne zu dem von Knative bereitgestellten Istio-Ingress-Gateway einrichten können.
  - Implementieren Sie eine neue Version Ihrer App und steuern Sie mithilfe von Istio die Menge an Datenverkehr, die an jede App-Version geleitet wird.
- Untersuchen Sie Beispiele für die [Knative-Basiskomponente `Eventing` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/knative/docs/tree/master/eventing/samples).
- Lesen Sie weitere Informationen zu Knative in der [Knative-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/knative/docs).
