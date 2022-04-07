---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-16"

keywords: kubernetes, iks, helm, without tiller, private cluster tiller, integrations, helm chart

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


# Services unter Verwendung von Helm-Diagrammen hinzufügen
{: #helm}

Sie können Ihrem Cluster komplexe Kubernetes-Apps mithilfe von Helm-Diagrammen hinzufügen.
{: shortdesc}

**Was ist Helm und wozu wird es verwendet?** </br>
[Helm ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://helm.sh) ist ein Kubernetes-Paketmananager, der mithilfe von Helm-Diagrammen komplexe Kubernetes-Apps in Ihrem Cluster definiert, installiert und aktualisiert. Helm-Diagramme verpacken die Spezifikationen zum Generieren von YAML-Dateien für Kubernetes-Ressourcen, die Ihre App erstellen. Diese Kubernetes-Ressourcen werden automatisch in Ihrem Cluster angewendet und von Helm versioniert. Sie können Helm auch verwenden, um Ihre eigene App zu spezifizieren und zu packen und von Helm die YAML-Dateien für Ihre Kubernetes-Ressourcen generieren zu lassen.  

Zum Verwenden von Helm in Ihrem Cluster müssen Sie die Helm-CLI auf Ihrer lokalen Maschine installieren und den Helm-Server Tiller in jedem Cluster installieren, in dem Sie Helm verwenden möchten.

**Welche Helm-Diagramme werden in {{site.data.keyword.containerlong_notm}} unterstützt?** </br>
Eine Übersicht über verfügbare Helm-Diagramme finden Sie im Abschnitt [Helm-Diagrammkatalog ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/kubernetes/solutions/helm-charts). Die in diesem Katalog aufgeführten Helm-Diagramme sind wie folgt gruppiert:

- **iks-charts**: Helm-Diagramme, die für {{site.data.keyword.containerlong_notm}} genehmigt sind. Der Name des Repositorys wurde von `ibm` in `iks-charts` geändert. 
- **ibm-charts**: Helm-Diagramme, die für {{site.data.keyword.containerlong_notm}} und für {{site.data.keyword.Bluemix_notm}} Private-Cluster genehmigt wurden.
- **kubernetes**: Helm-Diagramme, die von der Kubernetes-Community bereitgestellt werden und von der Community-Governance als `stabil` eingestuft wurden. Diese Diagramme werden nicht daraufhin überprüft, ob sie in {{site.data.keyword.containerlong_notm}}- oder in {{site.data.keyword.Bluemix_notm}} Private-Clustern funktionieren.
- **kubernetes-incubator**: Helm-Diagramme, die von der Kubernetes-Community bereitgestellt werden und von der Community-Governance als `Inkubator` eingestuft wurden. Diese Diagramme werden nicht daraufhin überprüft, ob sie in {{site.data.keyword.containerlong_notm}}- oder in {{site.data.keyword.Bluemix_notm}} Private-Clustern funktionieren.

Helm-Diagramme aus den Repositorys **iks-charts** und **ibm-charts** sind vollständig in die {{site.data.keyword.Bluemix_notm}}-Unterstützungsorganisation integriert. Wenn Sie eine Frage oder ein Problem mit der Verwendung dieser Helm-Diagramme haben, können Sie einen der {{site.data.keyword.containerlong_notm}}-Unterstützungskanäle verwenden. Weitere Informationen finden Sie im Abschnitt [Hilfe und Unterstützung anfordern](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

**Welches sind die Voraussetzungen für die Verwendung von Helm und wie kann ich Helm in einem privaten Cluster verwenden?** </br>
Zum Bereitstellen von Helm-Diagrammen müssen Sie die Helm-CLI auf Ihrer lokalen Maschine installieren und den Helm-Server Tiller in Ihrem Cluster installieren. Das Image für Tiller ist in der öffentlichen Google-Container-Registry gespeichert. Für den Zugriff auf das Image während der Tiller-Installation muss Ihr Cluster öffentliche Netzkonnektivität zur öffentlichen Google-Container-Registry zulassen. Cluster, für die der öffentliche Serviceendpunkt aktiviert ist, können automatisch auf das Image zugreifen. Private Cluster, die durch eine angepasste Firewall geschützt werden, oder Cluster, für die nur der private Serviceendpunkt aktiviert ist, lassen keinen Zugriff auf das Tiller-Image zu. Stattdessen können Sie das [Image auf Ihre lokale Maschine extrahieren und durch eine Push-Operation in Ihren Namensbereich in {{site.data.keyword.registryshort_notm}} übertragen](#private_local_tiller) oder [Helm-Diagramme ohne Tiller installieren](#private_install_without_tiller).


## Helm in einem Cluster mit öffentlichem Zugriff einrichten
{: #public_helm_install}

Wenn für Ihren Cluster der öffentliche Serviceendpunkt aktiviert ist, können Sie den Helm-Server Tiller mithilfe des öffentlichen Image in der Google-Container-Registry installieren.
{: shortdesc}

Vorbereitende Schritte:
- [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Wenn Sie Tiller mit einem Kubernetes-Servicekonto und der Clusterrollenbindung im Namensbereich `kube-system` installieren möchten, müssen Sie über die Rolle [`cluster-admin`](/docs/containers?topic=containers-users#access_policies) verfügen.

Gehen Sie wie folgt vor, um Helm in einem Cluster mit öffentlichem Zugriff zu installieren:

1. Installieren Sie die <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm-CLI <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> auf Ihrer lokalen Maschine.

2. Überprüfen Sie, ob Sie Tiller bereits mit einem Kubernetes-Servicekonto in Ihrem Cluster installiert haben. 
   ```
   kubectl get serviceaccount --all-namespaces | grep tiller
   ```
   {: pre}
      
   Beispielausgabe, wenn Tiller installiert ist: 
   ```
   kube-system      tiller                               1         189d
   ```
   {: screen}
      
   Die Beispielausgabe enthält den Kubernetes-Namensbereich und den Namen des Servicekontos für Tiller. Wenn Tiller nicht mit einem Servicekonto in Ihrem Cluster installiert ist, wird keine CLI-Ausgabe zurückgegeben. 
      
3. **Wichtig**: Um die Clustersicherheit zu erhalten, konfigurieren Sie Tiller mit einem Servicekonto und einer Clusterrollenbindung in Ihrem Cluster. 
   - **Wenn Tiller mit einem Servicekonto installiert ist:** 
     1. Erstellen Sie eine Clusterrollenbindung für das Tiller-Servicekonto. Ersetzen Sie `<namespace>` durch den Namensbereich, in dem Tiller in Ihrem Cluster installiert ist.
        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=<namensbereich>:tiller -n <namensbereich>
        ```
        {: pre}
  
     2. Aktualisieren Sie Tiller. Ersetzen Sie `<tiller_service_account_name>` durch den Namen des Kubernetes-Servicekontos für Tiller, den Sie im vorherigen Schritt abgerufen haben.
        ```
        helm init --upgrade --service-account <name_des_tiller_servicekontos>
        ```
        {: pre}
           
     3. Überprüfen Sie, ob der Pod `tiller-deploy` in Ihrem Cluster den **Status** `Running` aufweist.
        ```
        kubectl get pods -n <namensbereich> -l app=helm
        ```
        {: pre}
           
        Beispielausgabe:

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
        tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}
        
   - **Wenn Tiller nicht mit einem Servicekonto installiert ist:** 
     1. Erstellen Sie ein Kubernetes-Servicekonto und eine Clusterrollenbindung für Tiller im Namensbereich `kube-system` Ihres Clusters.
        ```
        kubectl create serviceaccount tiller -n kube-system
        ```
        {: pre}
        
        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=kube-system:tiller -n kube-system
        ```
        {: pre}
           
     2. Überprüfen Sie, ob das Tiller-Servicekonto erstellt wurde.
        ```
        kubectl get serviceaccount -n kube-system tiller
        ```
        {: pre}
           
        Beispielausgabe: 
        ```
        NAME                                 SECRETS   AGE
    tiller                               1         2m
        ```
        {: screen}
          
     3. Initialisieren Sie die Helm-CLI und installieren Sie Tiller in Ihrem Cluster mit dem von Ihnen erstellten Servicekonto.
        ```
        helm init --service-account tiller
        ```
        {: pre}
           
     4. Überprüfen Sie, ob der Pod `tiller-deploy` in Ihrem Cluster den **Status** `Running` aufweist.
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        Beispielausgabe:
        ```
        NAME                            READY     STATUS    RESTARTS   AGE
        tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

4. Fügen Sie Ihrer Helm-Instanz die {{site.data.keyword.Bluemix_notm}}-Helm-Repositorys hinzu.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

5. Aktualisieren Sie die Repositorys, um die neuesten Versionen aller Helm-Diagramme abzurufen.
   ```
   helm repo update
   ```
   {: pre}

6. Listen Sie die aktuell in den {{site.data.keyword.Bluemix_notm}}-Repositorys verfügbaren Helm-Diagramme auf.
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

7. Ermitteln Sie das Helm-Diagramm, das Sie installieren wollen, und befolgen Sie die Anweisungen in der Datei `README` des Helm-Diagramms, um es in Ihrem Cluster zu installieren.


## Private Cluster: Tiller-Image durch Push-Operation in Ihren Namensbereich in IBM Cloud Container Registry übertragen
{: #private_local_tiller}

Sie können das Tiller-Image auf Ihre lokale Maschine extrahieren, durch eine Push-Operation in Ihren Namensbereich in {{site.data.keyword.registryshort_notm}} übertragen und Tiller mithilfe des Image in {{site.data.keyword.registryshort_notm}} in Ihrem privaten Cluster installieren.
{: shortdesc}

Wenn Sie ein Helm-Diagramm ohne Verwendung von Tiller installieren möchten, finden Sie weitere Informationen unter [Private Cluster: Helm-Diagramme ohne Tiller installieren](#private_install_without_tiller).
{: tip}

Vorbereitende Schritte:
- Installieren Sie Docker auf Ihrer lokalen Maschine. Wenn Sie die [{{site.data.keyword.Bluemix_notm}}-CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) installiert haben, ist Docker bereits installiert.
- [Installieren Sie das {{site.data.keyword.registryshort_notm}}-CLI-Plug-in und richten Sie einen Namensbereich ein](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install).
- Wenn Sie Tiller mit einem Kubernetes-Servicekonto und der Clusterrollenbindung im Namensbereich `kube-system` installieren möchten, müssen Sie über die Rolle [`cluster-admin`](/docs/containers?topic=containers-users#access_policies) verfügen.

Gehen Sie wie folgt vor, um Tiller mithilfe von {{site.data.keyword.registryshort_notm}} zu installieren:

1. Installieren Sie die <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm-CLI <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> auf Ihrer lokalen Maschine.
2. Stellen Sie eine Verbindung zu Ihrem privaten Cluster durch den VPN-Tunnel für die {{site.data.keyword.Bluemix_notm}}-Infrastruktur her, den Sie eingerichtet haben.
3. **Wichtig**: Zum Erhalten der Clustersicherheit erstellen Sie ein Servicekonto für Tiller im Namensbereich `kube-system` und eine Kubernetes-RBAC-Clusterrollenbindung für den Pod `tiller-deploy`, indem Sie die folgende YAML-Datei aus dem [{{site.data.keyword.Bluemix_notm}}-Repository `kube-samples` anwenden](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml).
    1. [Rufen Sie die YAML-Dateien für das Kubernetes-Servicekonto und die Clusterrollenbindung ab ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml).

    2. Erstellen Sie Kubernetes-Ressourcen in Ihrem Cluster.
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [Suchen Sie die Version von Tiller ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30), die Sie in Ihrem Cluster installieren möchten. Wenn Sie keine bestimmte Version benötigen, verwenden Sie die neueste Version.

5. Extrahieren Sie das Tiller-Image aus der öffentlichen Google Container Registry durch eine Pull-Operation auf Ihre lokale Maschine.
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   Beispielausgabe:
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [Übertragen Sie das Tiller-Image durch eine Push-Operation in {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing).

7. Für den Zugriff auf das Image {{site.data.keyword.registryshort_notm}} aus Ihrem Cluster heraus [kopieren Sie den geheimen Schlüssel für Image-Pull-Operationen in den Namensbereich `kube-system`](/docs/containers?topic=containers-images#copy_imagePullSecret).

8. Installieren Sie Tiller in Ihrem privaten Cluster, indem Sie das Image verwenden, das Sie in Ihrem Namensbereich in {{site.data.keyword.registryshort_notm}} gespeichert haben.
   ```
   helm init --tiller-image <region>.icr.io/<mein_namensbereich>/<mein_image>:<tag> --service-account tiller
   ```
   {: pre}

9. Fügen Sie Ihrer Helm-Instanz die {{site.data.keyword.Bluemix_notm}}-Helm-Repositorys hinzu.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

10. Aktualisieren Sie die Repositorys, um die neuesten Versionen aller Helm-Diagramme abzurufen.
    ```
    helm repo update
    ```
    {: pre}

11. Listen Sie die aktuell in den {{site.data.keyword.Bluemix_notm}}-Repositorys verfügbaren Helm-Diagramme auf.
    ```
    helm search iks-charts
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. Ermitteln Sie das Helm-Diagramm, das Sie installieren wollen, und befolgen Sie die Anweisungen in der Datei `README` des Helm-Diagramms, um es in Ihrem Cluster zu installieren.


## Private Cluster: Helm-Diagramme ohne Tiller installieren
{: #private_install_without_tiller}

Wenn Sie Tiller in Ihrem privaten Cluster nicht installieren wollen, können Sie die YAML-Dateien des Helm-Diagramms manuell erstellen und mithilfe von `kubectl`-Befehlen anwenden.
{: shortdesc}

Die Schritte in diesem Beispiel demonstrieren, wie Helm-Diagramme aus den Repositorys für {{site.data.keyword.Bluemix_notm}}-Helm-Diagramme in Ihrem privaten Cluster installiert werden. Wenn Sie ein Helm-Diagramm installieren wollen, das nicht in einem der Repositorys für {{site.data.keyword.Bluemix_notm}}-Helm-Diagramme gespeichert ist, müssen Sie die Anweisungen in diesem Abschnitt ausführen, um die YAML-Dateien für Ihr Helm-Diagramm zu erstellen. Darüber hinaus müssen Sie das Helm-Diagramm-Image aus der öffentlichen Container-Registry herunterladen, in Ihren Namensbereich in {{site.data.keyword.registryshort_notm}} durch eine Push-Operation übertragen und die Datei `values.yaml` aktualisieren, um das Image in {{site.data.keyword.registryshort_notm}} zu verwenden.
{: note}

1. Installieren Sie die <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm-CLI <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> auf Ihrer lokalen Maschine.
2. Stellen Sie eine Verbindung zu Ihrem privaten Cluster durch den VPN-Tunnel für die {{site.data.keyword.Bluemix_notm}}-Infrastruktur her, den Sie eingerichtet haben.
3. Fügen Sie Ihrer Helm-Instanz die {{site.data.keyword.Bluemix_notm}}-Helm-Repositorys hinzu.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

4. Aktualisieren Sie die Repositorys, um die neuesten Versionen aller Helm-Diagramme abzurufen.
   ```
   helm repo update
   ```
   {: pre}

5. Listen Sie die aktuell in den {{site.data.keyword.Bluemix_notm}}-Repositorys verfügbaren Helm-Diagramme auf.
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. Ermitteln Sie das Helm-Diagramm, das Sie installieren wollen, laden Sie das Helm-Diagramm auf Ihre lokale Maschine herunter und entpacken Sie die Dateien Ihres Helm-Diagramms. Das folgende Beispiel zeigt, wie das Helm-Diagramm für den Cluster-Autoscaler Version 1.0.3 heruntergeladen und die Dateien in einem Verzeichnis `cluster-autoscaler` entpackt werden.
   ```
   helm fetch iks-charts/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Wechseln Sie in das Verzeichnis, in dem Sie die Dateien des Helm-Diagramms entpackt haben.
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Erstellen Sie ein Verzeichnis `output` für die YAML-Dateien, die Sie mithilfe der Dateien in Ihrem Helm-Diagramm generieren.
   ```
   mkdir output
   ```
   {: pre}

9. Öffnen Sie die Datei `values.yaml` und nehmen Sie die Änderungen vor, die entsprechend den Installationsanweisungen für das Helm-Diagramm erforderlich sind.
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. Verwenden Sie Ihre lokale Helm-Installation, um alle Kubernetes-YAML-Dateien  für Ihr Helm-Diagramm zu erstellen. Die YAML-Dateien werden im Verzeichnis `output` gespeichert, das Sie zuvor erstellt haben.
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Beispielausgabe:
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. Stellen Sie alle YAML-Dateien in Ihrem privaten Cluster bereit.
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. Optional: Entfernen Sie alle YAML-Dateien aus dem Verzeichnis `output`.
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}

## Zugehörige Helm-Links
{: #helm_links}

Sehen Sie sich die folgenden Links an, um zusätzliche Informationen zu Helm zu finden.
{: shortdesc}

* Zeigen Sie verfügbare Helm-Diagramme an, die Sie in {{site.data.keyword.containerlong_notm}} verwenden können, im [Helm-Diagrammkatalog ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) an.
* Weitere Informationen zu den Helm-Befehlen zum Konfigurieren und Verwalten von Helm-Diagrammen finden Sie in der <a href="https://docs.helm.sh/helm/" target="_blank">Helm-Dokumentation <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.
* Über den folgenden Link finden Sie weitere Informationen zur Vorgehensweise bei der [Erhöhung der Bereitstellungsgeschwindigkeit mit Kubernetes-Helm-Diagrammen![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).
