---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-16"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Debugging für Ingress
{: #cs_troubleshoot_debug_ingress}

Ziehen Sie bei der Verwendung von {{site.data.keyword.containerlong}} die folgenden Verfahren für die allgemeine Fehlerbehebung und das Debugging für Ingress in Betracht.
{: shortdesc}

Sie haben Ihre App öffentlich zugänglich gemacht, indem Sie eine Ingress-Ressource für Ihre App in Ihrem Cluster erstellt haben. Wenn Sie jedoch versuchen, über die öffentliche IP-Adresse oder Unterdomäne der ALB eine Verbindung zu Ihrer App herzustellen, schlägt die Verbindung fehl oder es wird ein Zeitlimit überschritten. Die Schritte in den folgenden Abschnitten können Ihnen beim Debugging Ihrer Ingress-Konfiguration helfen.

Stellen Sie sicher, dass Sie einen Host in nur einer Ingress-Ressource definieren. Wenn ein Host in mehreren Ingress-Ressourcen definiert ist, leitet die Lastausgleichsfunktion für Anwendungen den Datenverkehr möglicherweise nicht ordnungsgemäß weiter und es können Fehler auftreten.
{: tip}

Stellen Sie zunächst sicher, dass Sie über die folgenden [{{site.data.keyword.Bluemix_notm}} IAM-Zugriffsrichtlinien](/docs/containers?topic=containers-users#platform) verfügen:
  - Plattformrolle **Editor** oder **Administrator** für den Cluster
  - Servicerolle **Schreibberechtigter** oder **Manager**

## Schritt 1: Ingress-Tests in {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool durchführen

Bei der Fehlerbehebung können Sie {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool verwenden, um Ingress-Tests durchzuführen und relevante Ingress-Informationen aus Ihrem Cluster zu erfassen. Zur Verwendung des Debug-Tools installieren Sie das [Helm-Diagramm `ibmcloud-iks-debug` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm/ibmcloud-iks-debug):
{: shortdesc}


1. [Richten Sie Helm in Ihrem Cluster ein, erstellen Sie ein Servicekonto für Tiller und fügen Sie Ihrer Helm-Instanz das Repository `ibm` hinzu](/docs/containers?topic=containers-integrations#helm).

2. Installieren Sie das Helm-Diagramm in Ihrem Cluster.
  ```
  helm install iks-charts/ibmcloud-iks-debug --name debug-tool
  ```
  {: pre}


3. Starten Sie einen Proxy-Server, um die Debug-Tool-Schnittstelle anzuzeigen.
  ```
  kubectl proxy --port 8080
  ```
  {: pre}

4. Öffnen Sie in einem Webbrowser die URL der Debug-Tool-Schnittstelle: http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page

5. Wählen Sie die Testgruppe **ingress** aus. Einige Tests prüfen auf potenzielle Warnungen, Fehler oder Probleme, während andere Tests nur Informationen zusammenstellen, auf die Sie bei der Fehlersuche zurückgreifen können. Weitere Informationen zur Funktion der einzelnen Test erhalten Sie, wenn Sie auf das Informationssymbol neben dem Namen des Tests klicken.

6. Klicken Sie auf **Run** (Ausführen).

7. Prüfen Sie die Ergebnisse jedes Tests.
  * Wenn ein Test einen Fehler meldet, klicken Sie auf das Informationssymbol neben dem Namen des Tests in der linken Spalte, um Informationen zur Lösung des Problems anzuzeigen.
  * Sie können auch die Ergebnisse von Tests, die nur Informationen zusammenstellen, beim Debugging für Ihren Ingress-Service in folgenden Abschnitten verwenden.

## Schritt 2: Ingress-Bereitstellung und Pod-Protokolle der Lastausgleichsfunktion für Anwendungen (ALB) auf Fehlernachrichten überprüfen
{: #errors}

Beginnen Sie, indem Sie in den Ereignissen der Ingress-Ressourcenbereitstellung und in den ALB-Pod-Protokollen nach Fehlernachrichten suchen. Diese Fehlernachrichten können Ihnen bei der Suche nach den Ursachen für Ausfälle und bei der weiteren Fehlerbehebung für Ihre Ingress-Konfiguration in den nächsten Abschnitten helfen.
{: shortdesc}

1. Überprüfen Sie Ihre Ingress-Bereitstellung und suchen Sie nach möglichen Warnungen und Fehlernachrichten.
    ```
    kubectl describe ingress <meiningress>
    ```
    {: pre}

    Im Abschnitt **Events** der Ausgabe sehen Sie möglicherweise Nachrichten zu ungültigen Werten in Ihrer Ingress-Ressource oder in bestimmten Annotationen, die Sie verwendet haben. Weitere Informationen finden Sie in der [Dokumentation zur Ingress-Ressourcenkonfiguration](/docs/containers?topic=containers-ingress#public_inside_4) oder in der [Dokumentation zu Annotationen](/docs/containers?topic=containers-ingress_annotation).

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster.us-south.containers.appdomain.cloud
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}
{: #check_pods}
2. Überprüfen Sie den Status Ihrer ALB-Pods.
    1. Rufen Sie die ALB-Pods ab, die in Ihrem Cluster ausgeführt werden.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. Stellen Sie sicher, dass alle Pods ausgeführt werden, und überprüfen Sie dazu die Spalte **STATUS**.

    3. Wenn sich ein Pod nicht im Status `Aktiv` befindet, können Sie die ALB inaktivieren und erneut aktivieren. Ersetzen Sie `<ALB_ID>` in den folgenden Befehlen durch die ID der ALB des Pods. Beispiel: Wenn der Pod, der nicht aktiv ist, den Namen `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1-5d6d86fbbc-kxj6z` trägt, lautet die ALB-ID `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1`.
        ```
        ibmcloud ks alb-configure --albID <ALB-ID> --disable
        ```
        {: pre}

        ```
        ibmcloud ks alb-configure --albID <ALB-ID> --enable
        ```
        {: pre}

3. Überprüfen Sie die Protokolle für Ihre Lastausgleichsfunktion für Anwendungen.
    1.  Rufen Sie die IDs der ALB-Pods ab, die in Ihrem Cluster ausgeführt werden.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    3. Rufen Sie die Protokolle für den Container `nginx-ingress` auf jedem ALB-Pod ab.
        ```
        kubectl logs <ingress-pod-id> nginx-ingress -n kube-system
        ```
        {: pre}

    4. Suchen Sie nach Fehlernachrichten in den Protokollen der Lastausgleichsfunktion für Anwendungen.

## Schritt 3: ALB-Unterdomäne und öffentliche IP-Adressen mit Ping überprüfen
{: #ping}

Überprüfen Sie die Verfügbarkeit Ihrer Ingress-Unterdomäne und der öffentlichen IP-Adressen der ALBs.
{: shortdesc}

1. Rufen Sie die IP-Adressen ab, an denen Ihre öffentlichen ALBs empfangsbereit sind.
    ```
    ibmcloud ks albs --cluster <clustername_oder_-id>
    ```
    {: pre}

    Beispielausgabe für einen Mehrzonencluster mit Workerknoten in `dal10` und `dal13`:

    ```
    ALB ID                                            Status     Type      ALB IP           Zone    Build
    private-cr24a9f2caf6554648836337d240064935-alb1   disabled   private   -                dal13   ingress:350/ingress-auth:192   
    private-cr24a9f2caf6554648836337d240064935-alb2   disabled   private   -                dal10   ingress:350/ingress-auth:192   
    public-cr24a9f2caf6554648836337d240064935-alb1    enabled    public    169.62.196.238   dal13   ingress:350/ingress-auth:192   
    public-cr24a9f2caf6554648836337d240064935-alb2    enabled    public    169.46.52.222    dal10   ingress:350/ingress-auth:192  
    ```
    {: screen}

    * Wenn eine öffentliche ALB über keine IP-Adresse verfügt, finden Sie weitere Informationen hierzu unter [Ingress-ALB wird in einer Zone nicht bereitgestellt](/docs/containers?topic=containers-cs_troubleshoot_network#cs_multizone_subnet_limit).

2. Überprüfen Sie den Zustand Ihrer ALB-IPs.

    * Für Einzelzonencluster und Mehrzonencluster: Überprüfen Sie die IP-Adresse jeder öffentlichen ALB, um sicherzustellen, dass jede ALB erfolgreich Pakete empfangen kann. Wenn Sie private ALBs verwenden, können Sie deren IP-Adressen nur über das private Netz mit Ping überprüfen.
        ```
        ping <ip_der_lastausgleichsfunktion_für_anwendungen>
        ```
        {: pre}

        * Wenn die CLI ein Zeitlimit zurückgibt und Sie über eine angepasste Firewall verfügen, die Ihre Workerknoten schützt, stellen Sie sicher, dass Ihre [Firewall](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall) ICMP zulässt.
        * Wenn keine Firewall vorhanden ist, die die Pingsignale blockiert, und die Pingsignale weiterhin das Zeitlimit überschreiten, müssen Sie den [Status der ALB-Pods überprüfen](#check_pods).

    * Nur bei Mehrzonenclustern: Sie können die MZLB-Zustandsprüfung verwenden, um den Status Ihrer ALB-IPs zu ermitteln. Weitere Informationen zur MZLB finden Sie unter [Lastausgleichsfunktion für mehrere Zonen (MZLB)](/docs/containers?topic=containers-ingress#planning). Der MZLB-Statusmonitor ist nur für Cluster verfügbar, die über die neue Ingress-Unterdomäne im Format `<cluster_name>.<region_or_zone>.containers.appdomain.cloud` verfügen. Wenn Ihr Cluster noch das ältere Format von `<cluster_name>.<region>.containers.mybluemix.net` verwendet, müssen Sie Ihren [Einzelzonencluster in einen Mehrzonencluster konvertieren](/docs/containers?topic=containers-clusters#add_zone). Ihrem Cluster wird eine Unterdomäne mit dem neuen Format zugewiesen, er kann aber auch weiterhin das ältere Unterdomänenformat verwenden. Alternativ können Sie einen neuen Cluster bestellen, dem automatisch das neue Unterdomänenformat zugeordnet wird.

    Der folgende HTTP-cURL-Befehl verwendet den Host `albhealth`, der von {{site.data.keyword.containerlong_notm}} so konfiguriert wird, dass er entweder den Status `healthy` oder den Status `unhealthy` für eine ALB-IP zurückgibt.
        ```
        curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
        ```
        {: pre}

        Beispielausgabe:
        ```
        healthy
        ```
        {: screen}
        Wenn ein oder mehrere IPs `unhealthy` zurückgeben, müssen Sie [den Status der ALB-Pods überprüfen](#check_pods).

3. Rufen Sie die von IBM bereitgestellte Ingress-Unterdomäne ab.
    ```
    ibmcloud ks cluster-get --cluster <clustername_oder_-id> | grep Ingress
    ```
    {: pre}

    Beispielausgabe:
    ```
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <geheimer_tls-schlüssel>
    ```
    {: screen}

4. Stellen Sie sicher, dass die IPs für jede öffentliche ALB, die Sie in Schritt 2 dieses Abschnitts erhalten haben, bei der von IBM bereitgestellten Ingress-Unterdomäne Ihres Clusters registriert sind. In einem Mehrzonencluster muss beispielsweise die öffentliche ALB-IP in jeder Zone, in der Sie Workerknoten haben, unter demselben Hostnamen registriert werden.

    ```
    kubectl get ingress -o wide
    ```
    {: pre}

    Beispielausgabe:
    ```
    NAME                HOSTS                                                    ADDRESS                        PORTS     AGE
    myingressresource   mycluster-12345.us-south.containers.appdomain.cloud      169.46.52.222,169.62.196.238   80        1h
    ```
    {: screen}

## Schritt 4: Domänenzuordnungen und Ingress-Ressourcenkonfiguration überprüfen
{: #ts_ingress_config}

1. Wenn Sie eine angepasste Domäne verwenden, stellen Sie sicher, dass Sie Ihren DNS-Provider verwendet haben, um die von IBM bereitgestellte Unterdomäne oder die öffentliche IP-Adresse der ALB zuzuordnen. Dabei ist zu beachten, dass die Verwendung eines CNAME bevorzugt wird, weil IBM automatische Zustandsprüfungen für die IBM Unterdomäne ermöglicht und alle fehlgeschlagenen IPs aus der DNS-Antwort entfernt.
    * Von IBM bereitgestellte Unterdomäne: Überprüfen Sie, ob Ihre angepasste Domäne der von IBM bereitgestellten Unterdomäne des Clusters im CNAME-Datensatz (kanonischer Name) zugeordnet ist.
        ```
        host www.my-domain.com
        ```
        {: pre}

        Beispielausgabe:
        ```
        www.my-domain.com is an alias for mycluster-12345.us-south.containers.appdomain.cloud
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
        ```
        {: screen}

    * Öffentliche IP-Adresse: Prüfen Sie, ob Ihre angepasste Domäne der portierbaren öffentlichen IP-Adresse der ALB im A-Datensatz zugeordnet ist. Die IPs müssen mit den öffentlichen ALB-IPs übereinstimmen, die Sie in Schritt 1 des [vorherigen Abschnitts](#ping) erhalten haben.
        ```
        host www.my-domain.com
        ```
        {: pre}

        Beispielausgabe:
        ```
        www.my-domain.com has address 169.46.52.222
        www.my-domain.com has address 169.62.196.238
        ```
        {: screen}

2. Überprüfen Sie die Ingress-Ressourcenkonfigurationsdateien für Ihren Cluster.
    ```
    kubectl get ingress -o yaml
    ```
    {: pre}

    1. Stellen Sie sicher, dass Sie einen Host in nur einer Ingress-Ressource definieren. Wenn ein Host in mehreren Ingress-Ressourcen definiert ist, leitet die Lastausgleichsfunktion für Anwendungen den Datenverkehr möglicherweise nicht ordnungsgemäß weiter und es können Fehler auftreten.

    2. Prüfen Sie, ob die Unterdomäne und das TLS-Zertifikat korrekt sind. Um die von IBM bereitgestellte Ingress-Unterdomäne und das TLS-Zertifikat zu ermitteln, führen Sie den Befehl `ibmcloud ks cluster-get --cluster <clustername_oder_-id>` aus.

    3.  Stellen Sie sicher, dass Ihre App denselben Pfad überwacht, der im Abschnitt **path** von Ingress konfiguriert ist. Wenn Ihre App so eingerichtet ist, dass sie den Rootpfad überwacht, verwenden Sie `/` als Pfad. Wenn der für diesen Pfad eingehende Datenverkehr an einen anderen Pfad weitergeleitet werden muss, an dem Ihre App empfangsbereit ist, verwenden Sie die Annotation [rewrite-path](/docs/containers?topic=containers-ingress_annotation#rewrite-path).

    4. Bearbeiten Sie die YAML-Datei für die Ressourcenkonfiguration nach Bedarf. Wenn Sie den Editor schließen, werden Ihre Änderungen gespeichert und automatisch angewendet.
        ```
        kubectl edit ingress <meine_ingress-ressource>
        ```
        {: pre}

## ALB für das Debugging aus DNS entfernen
{: #one_alb}

Wenn Sie nicht über eine bestimmte ALB-IP auf Ihre App zugreifen können, können Sie die ALB vorübergehend aus der Produktion nehmen, indem Sie ihre DNS-Registrierung inaktivieren. Anschließend können Sie die IP-Adresse der ALB verwenden, um Debugging-Tests für diese ALB auszuführen.

Beispiel: Angenommen, Sie haben einen Mehrzonencluster in zwei Zonen und die beiden öffentlichen ALBs haben die IP-Adressen `169.46.52.222` und `169.62.196.238`. Obwohl die Zustandsprüfung für den ALB der zweiten Zone einen einwandfreien Zustand (healthy) meldet, ist Ihre App nicht direkt über die App erreichbar. Sie entscheiden, die IP-Adresse der ALB `169.62.196.238` zu Debugging-Zwecken aus der Produktion zu nehmen. Die ALB-IP der ersten Zone (`169.46.52.222`) wird bei Ihrer Domäne registriert und leitet den Datenverkehr weiter, während Sie das Debugging für die ALB der zweiten Zone durchführen.

1. Rufen Sie den Namen der ALB mit der nicht erreichbaren IP-Adresse ab.
    ```
    ibmcloud ks albs --cluster <clustername> | grep <ALB-IP>
    ```
    {: pre}

    Beispiel: Die nicht erreichbare IP `169.62.196.238` gehört zu der ALB `public-cr24a9f2caf6554648836337d240064935-alb1`:
    ```
    ALB ID                                            Status     Type      ALB IP           Zone   Build
    public-cr24a9f2caf6554648836337d240064935-alb1    enabled    public    169.62.196.238   dal13   ingress:350/ingress-auth:192
    ```
    {: screen}

2. Verwenden Sie den ALB-Namen aus dem vorherigen Schritt, um die Namen der ALB-Pods abzurufen. Der folgende Befehl verwendet den ALB-Beispielnamen aus dem vorherigen Schritt:
    ```
    kubectl get pods -n kube-system | grep public-cr24a9f2caf6554648836337d240064935-alb1
    ```
    {: pre}

    Beispielausgabe:
    ```
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq   2/2       Running   0          24m
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-trqxc   2/2       Running   0          24m
    ```
    {: screen}

3. Inaktivieren Sie die Zustandsprüfung, die für alle ALB-Pods ausgeführt wird. Wiederholen Sie diese Schritte für jeden ALB-Pod, den Sie im vorherigen Schritt erhalten haben. Die Beispielbefehle und die Ausgabe in diesen Schritten verwenden den ersten Pod: `public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq`.
    1. Melden Sie sich beim ALB-Pod an und überprüfen Sie die Zeile `server_name` in der NGINX-Konfigurationsdatei.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        Beispielausgabe, die bestätigt, dass der ALB-Pod mit dem korrekten Hostnamen für die Zustandsprüfung konfiguriert ist (`albhealth.<domäne>`):
        ```
        server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud;
        ```
        {: screen}

    2. Um die IP zu entfernen, indem Sie die Zustandsprüfung inaktivieren, fügen Sie `#` vor `server_name` ein. Wenn der virtuelle Host `albhealth.mycluster-12345.us-south.containers.appdomain.cloud` für die ALB inaktiviert wird, entfernt die automatische Zustandsprüfung automatisch die IP aus der DNS-Antwort.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- sed -i -e 's*server_name*#server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

    3. Stellen Sie sicher, dass die Änderung angewendet wurde.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        Beispielausgabe:
        ```
        #server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud
        ```
        {: screen}

    4. Um die IP aus der DNS-Registrierung zu entfernen, laden Sie die NGINX-Konfiguration erneut.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- nginx -s reload
        ```
        {: pre}

    5. Wiederholen Sie diese Schritte für jeden ALB-Pod.

4. Wenn Sie jetzt versuchen, den cURL-Befehl für den Host `albhealth` auszuführen, um eine Zustandsprüfung der ALB-IP vorzunehmen, schlägt die Überprüfung fehl.
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

    Ausgabe:
    ```
    <html>
        <head>
            <title>404 Not Found</title>
        </head>
        <body bgcolor="white"><center>
            <h1>404 Not Found</h1>
        </body>
    </html>
    ```
    {: screen}

5. Stellen Sie sicher, dass die ALB-IP-Adresse aus der DNS-Registrierung für Ihre Domäne entfernt wurde, indem Sie den Cloudflare-Server überprüfen. Beachten Sie, dass die Aktualisierung der DNS-Registrierung einige Minuten in Anspruch nehmen kann.
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    Beispielausgabe, die bestätigt, dass nur die einwandfreie ALB-IP `169.46.52.222` in der DNS-Registrierung verbleibt und dass die nicht einwandfreie ALB-IP `169.62.196.238` entfernt wurde:
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    ```
    {: screen}

6. Da die ALB-IP nun aus der Produktion genommen wurde, können Sie sie zur Ausführung der Debugging-Tests für Ihre App verwenden. Um die Kommunikation mit Ihrer App über diese IP zu testen, können Sie den folgenden cURL-Befehl ausführen und dabei die Beispielwerte durch eigene Werte ersetzen:
    ```
    curl -X GET --resolve mycluster-12345.us-south.containers.appdomain.cloud:443:169.62.196.238 https://mycluster-12345.us-south.containers.appdomain.cloud/
    ```
    {: pre}

    * Wenn alles ordnungsgemäß konfiguriert ist, erhalten Sie die erwartete Antwort von Ihrer App.
    * Wenn die Antwort einen Fehler enthält, liegt möglicherweise ein Fehler in Ihrer App oder in einer Konfiguration vor, die nur für diese spezielle ALB gültig ist. Überprüfen Sie Ihren App-Code, Ihre [Ingress-Ressourcenkonfigurationsdateien](/docs/containers?topic=containers-ingress#public_inside_4) oder alle anderen Konfigurationen, die Sie nur auf diese ALB angewendet haben.

7. Wenn Sie das Debugging beendet haben, stellen Sie die Zustandsprüfung für die ALB-Pods wieder her. Wiederholen Sie diese Schritte für jeden ALB-Pod.
  1. Melden Sie sich beim ALB-Pod an und entfernen Sie das Zeichen `#` in der Zeile `server_name`.
    ```
    kubectl exec -ti <podname> -n kube-system -c nginx-ingress -- sed -i -e 's*#server_name*server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
    ```
    {: pre}

  2. Laden Sie die NGINX-Konfiguration erneut, sodass die Wiederherstellung der Zustandsprüfung angewendet wird.
    ```
    kubectl exec -ti <podname> -n kube-system -c nginx-ingress -- nginx -s reload
    ```
    {: pre}

9. Wenn Sie jetzt den cURL-Befehl für den Host `albhealth` ausführen, um eine Zustandsprüfung der ALB-IP vorzunehmen, gibt die Überprüfung `healthy` zurück.
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

10. Stellen Sie sicher, dass die ALB-IP-Adresse in der DNS-Registrierung für Ihre Domäne wiederhergestellt wurde, indem Sie den Cloudflare-Server überprüfen. Beachten Sie, dass die Aktualisierung der DNS-Registrierung einige Minuten in Anspruch nehmen kann.
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    Beispielausgabe:
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
    ```
    {: screen}

<br />


## Hilfe und Unterstützung anfordern
{: #ingress_getting_help}

Haben Sie noch immer Probleme mit Ihrem Cluster?
{: shortdesc}

-  Sie werden im Terminal benachrichtigt, wenn Aktualisierungen für die `ibmcloud`-CLI und -Plug-ins verfügbar sind. Halten Sie Ihre CLI stets aktuell, sodass Sie alle verfügbaren Befehle und Flags verwenden können.
-   [Überprüfen Sie auf der {{site.data.keyword.Bluemix_notm}}-Statusseite ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/status?selected=status), ob {{site.data.keyword.Bluemix_notm}} verfügbar ist.
-   Veröffentlichen Sie eine Frage im [{{site.data.keyword.containerlong_notm}}-Slack ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibm-container-service.slack.com).
    Wenn Sie keine IBM ID für Ihr {{site.data.keyword.Bluemix_notm}}-Konto verwenden, [fordern Sie eine Einladung](https://bxcs-slack-invite.mybluemix.net/) zu diesem Slack an.
    {: tip}
-   Suchen Sie in entsprechenden Foren, ob andere Benutzer auf das gleiche Problem gestoßen sind. Versehen Sie Ihre Fragen in den Foren mit Tags, um sie für das Entwicklungsteam von {{site.data.keyword.Bluemix_notm}} erkennbar zu machen.
    -   Wenn Sie technische Fragen zur Entwicklung oder Bereitstellung von Clustern oder Apps mit {{site.data.keyword.containerlong_notm}} haben, veröffentlichen Sie Ihre Frage auf [Stack Overflow ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) und versehen Sie sie mit den Tags `ibm-cloud`, `kubernetes` und `containers`.
    -   Verwenden Sie bei Fragen zum Service und zu ersten Schritten das Forum [IBM Developer Answers ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Geben Sie die Tags `ibm-cloud` und `containers` an.
    Weitere Details zur Verwendung der Foren finden Sie unter [Hilfe anfordern](/docs/get-support?topic=get-support-getting-customer-support#using-avatar).
-   Wenden Sie sich an den IBM Support, indem Sie einen Fall öffnen. Informationen zum Öffnen eines IBM Supportfalls oder zu Supportstufen und zu Prioritätsstufen von Fällen finden Sie unter [Support kontaktieren](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
Geben Sie beim Melden eines Problems Ihre Cluster-ID an. Führen Sie den Befehl `ibmcloud ks clusters` aus, um Ihre Cluster-ID abzurufen. Sie können außerdem [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) verwenden, um relevante Informationen aus Ihrem Cluster zu erfassen und zu exportieren, um sie dem IBM Support zur Verfügung zu stellen.
{: tip}

