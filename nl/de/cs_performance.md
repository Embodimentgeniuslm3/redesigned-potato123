---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-05"

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

# Leistung optimieren
{: #kernel}

Wenn Sie bestimmte Voraussetzungen für die Leistungsoptimierung haben, können Sie die Standardeinstellungen für manche Clusterkomponenten in {{site.data.keyword.containerlong}} ändern.
{: shortdesc}

Wenn Sie die Standardeinstellungen ändern, tun Sie dies auf eigenes Risiko. Sie sind für die Ausführung der erforderlichen Tests für alle geänderten Einstellungen und für eventuelle Unterbrechungen, die durch die geänderten Einstellungen in Ihrer Umgebung verursacht werden, verantwortlich.
{: important}

## Leistung des Workerknotens optimieren
{: #worker}

Wenn Sie bestimmte Voraussetzungen für die Leistungsoptimierung haben, können Sie die Standardeinstellungen für die `sysctl`-Parameter für den Linux-Kernel auf Workerknoten ändern.
{: shortdesc}

Workerknoten werden automatisch mit optimierter Kerneloptimierung eingerichtet, aber Sie können die Standardeinstellungen ändern, indem Sie ein angepasstes [Kubernetes-Objekt des Typs `DaemonSet` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) auf Ihren Cluster anwenden. Die Dämongruppe (DaemonSet) ändert die Einstellungen für alle vorhandenen Workerknoten und wendet die Einstellungen auf alle neuen Workerknoten an, die im Cluster eingerichtet werden. Es sind keine Pods betroffen.

Sie müssen die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Manager**](/docs/containers?topic=containers-users#platform) für alle Namensbereiche innehaben, um das Beispiel für den privilegierten `initContainer` ausführen zu können. Nachdem die Container für die Bereitstellungen initialisiert wurden, werden die Privilegien gelöscht.
{: note}

1. Speichern Sie die folgende Dämongruppe (DaemonSet) in einer Datei mit dem Namen `worker-node-kernel-settings.yaml`. Fügen Sie im Abschnitt `spec.template.spec.initContainers` die Felder und Werte für die `sysctl`-Parameter hinzu, die Sie optimieren möchten. Dieses Beispiel für eine Dämongruppe ändert den Standardwert für die Anzahl Verbindungen, die maximal in der Umgebung zulässig sind, über die Einstellung `net.core.somaxconn` und den Bereich ephemerer Ports über die Einstellung `net.ipv4.ip_local_port_range`.
    ```
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: kernel-optimization
      namespace: kube-system
      labels:
        tier: management
        app: kernel-optimization
    spec:
      selector:
        matchLabels:
          name: kernel-optimization
      template:
        metadata:
          labels:
            name: kernel-optimization
        spec:
          hostNetwork: true
          hostPID: true
          hostIPC: true
          initContainers:
            - command:
                - sh
                - -c
                - sysctl -w net.ipv4.ip_local_port_range="1025 65535"; sysctl -w net.core.somaxconn=32768;
              image: alpine:3.6
              imagePullPolicy: IfNotPresent
              name: sysctl
              resources: {}
              securityContext:
                privileged: true
                capabilities:
                  add:
                    - NET_ADMIN
              volumeMounts:
                - name: modifysys
                  mountPath: /sys
          containers:
            - resources:
                requests:
                  cpu: 0.01
              image: alpine:3.6
              name: sleepforever
              command: ["/bin/sh", "-c"]
              args:
                - >
                  while true; do
                    sleep 100000;
                  done
          volumes:
            - name: modifysys
              hostPath:
                path: /sys
    ```
    {: codeblock}

2. Wenden Sie die Dämongruppe auf Ihre Workerknoten an. Die Änderungen werden sofort angewendet.
    ```
    kubectl apply -f worker-node-kernel-settings.yaml
    ```
    {: pre}

<br />

Gehen Sie wie folgt vor, um die `sysctl`-Parameter der Workerknoten auf die Standardwerte zurückzusetzen, die von {{site.data.keyword.containerlong_notm}} festgelegt wurden:

1. Löschen Sie die Dämongruppe. Die `initContainer`, die die angepassten Einstellungen angewendet haben, werden entfernt.
    ```
    kubectl delete ds kernel-optimization
    ```
    {: pre}

2. [Führen Sie für alle Workerknoten im Cluster einen Warmstart durch](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reboot). Die Workerknoten werden mit den angewendeten Standardwerten wieder gestartet.

<br />


## Podleistung optimieren
{: #pod}

Wenn Sie bestimmte Anforderungen an die Workloadleistung haben, können Sie die Standardeinstellungen für die `sysctl`-Parameter für den Linux-Kernel in Pod-Netznamensbereichen ändern.
{: shortdesc}

Um die Kerneleinstellungen für App-Pods zu optimieren, können Sie einen [`initContainer-Patch ` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) in die YAML-Datei `pod/ds/rs/deployment` für jede Bereitstellung einfügen. Der `initContainer` wird jeder App-Bereitstellung hinzugefügt, die sich in dem Pod-Netznamensbereich befindet, für den Sie die Leistung optimieren möchten.

Sie müssen zunächst sicherstellen, dass Sie die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Manager**](/docs/containers?topic=containers-users#platform) für alle Namensbereiche innehaben, um das Beispiel für den privilegierten `initContainer` ausführen zu können. Nachdem die Container für die Bereitstellungen initialisiert wurden, werden die Privilegien gelöscht.

1. Speichern Sie das folgende `initContainer`-Patch in einer Datei mit dem Namen `pod-patch.yaml` und fügen Sie die Felder und Werte für die `sysctl`-Parameter hinzu, die Sie optimieren möchten. Dieses Beispiel für den `initContainer` ändert den Standardwert für die Anzahl Verbindungen, die maximal in der Umgebung zulässig sind, über die Einstellung `net.core.somaxconn` und den Bereich ephemerer Ports über die Einstellung `net.ipv4.ip_local_port_range`.
    ```
    spec:
      template:
        spec:
          initContainers:
          - command:
            - sh
            - -c
            - sysctl -e -w net.core.somaxconn=32768;  sysctl -e -w net.ipv4.ip_local_port_range="1025 65535";
            image: alpine:3.6
            imagePullPolicy: IfNotPresent
            name: sysctl
            resources: {}
            securityContext:
              privileged: true
    ```
    {: codeblock}

2. Wenden Sie Patches auf alle Ihre Bereitstellungen an.
    ```
    kubectl patch deployment <name_der_bereitstellung> --patch pod-patch.yaml
    ```
    {: pre}

3. Wenn Sie den Wert für `net.core.somaxconn` in den Kerneleinstellungen geändert haben, können die meisten Apps automatisch den aktualisierten Wert verwenden. Einige Apps erfordern jedoch unter Umständen, dass Sie den entsprechenden Wert in Ihrem App-Code manuell ändern, damit er mit dem Kernelwert übereinstimmt. Wenn Sie beispielsweise die Leistung eines Pods optimieren, in dem eine NGINX-App ausgeführt wird, müssen Sie den Wert des Felds `backlog` im NGINX-App-Code ändern, damit er übereinstimmt. Weitere Informationen finden Sie in diesem [Blogbeitrag zu NGINX![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.nginx.com/blog/tuning-nginx/).

<br />


## Ressourcen des Providers von Clustermetriken anpassen
{: #metrics}

Die Konfigurationen Ihres Provider von Clustermetriken (`metrics-server` in Kubernetes 1.12 und höher oder `heapster` in früheren Versionen) sind für Cluster mit 30 oder weniger Pods pro Workerknoten optimiert. Wenn Ihr Cluster mehr Pods pro Workerknoten aufweist, kann es sein, dass der Hauptcontainer `metrics-server` oder `heapster` des Providers von Metriken für den Pod regelmäßig mit einer Fehlernachricht wie `OOMKilled` neu gestartet wird.

Der Pod des Providers von Metriken verfügt auch über einen Container `nanny`, der die Ressourcenanforderungen und -limits des Hauptcontainers `metrics-server` oder `heapster` in Antwort auf die Anzahl von Workerknoten im Cluster skaliert. Sie können die Standardressourcen ändern, indem Sie die Konfigurationszuordnung des Providers von Metriken bearbeiten.

Vorbereitende Schritte: [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Öffnen Sie die Konfigurationszuordnungs-YAML des Providers von Clustermetriken.
    *  Für `metrics-server`:
       ```
       kubectl get configmap metrics-server-config -n kube-system -o yaml
       ```
       {: pre}
    *  Für `heapster`:
       ```
       kubectl get configmap heapster-config -n kube-system -o yaml
       ```
       {: pre}
    Beispielausgabe:
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
    kind: ConfigMap
    metadata:
      annotations:
        armada-service: cruiser-kube-addons
        version: --
      creationTimestamp: 2018-10-09T20:15:32Z
      labels:
        addonmanager.kubernetes.io/mode: EnsureExists
        kubernetes.io/cluster-service: "true"
      name: heapster-config
      namespace: kube-system
      resourceVersion: "526"
      selfLink: /api/v1/namespaces/kube-system/configmaps/heapster-config
      uid: 11a1aaaa-bb22-33c3-4444-5e55e555e555
    ```
    {: screen}

2.  Fügen Sie das Feld `memoryPerNode` der Konfigurationszuordnung im Abschnitt `data.NannyConfiguration` hinzu. Der Standardwert sowohl für `metrics-server` als auch für `heapster` ist auf `4Mi` gesetzt.
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
        memoryPerNode: 5Mi
    kind: ConfigMap
    ...
    ```
    {: codeblock}

3.  Wenden Sie Ihre Änderungen an.
    ```
    kubectl apply -f heapster-config.yaml
    ```
    {: pre}

4.  Überwachen Sie die Pods des Providers von Metriken, um zu sehen, ob Container weiterhin aufgrund einer Fehlernachricht `OOMKilled` erneut gestartet werden. Ist dies der Fall, wiederholen Sie diese Schritte und setzen Sie den Wert für `memoryPerNode` nach oben, bis der Pod stabil läuft.

Möchten Sie weitere Einstellungen anpassen? Ideen und Informationen finden Sie in der [Dokumentation zur Konfiguration von Kubernetes Add-on Resizer ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/autoscaler/tree/master/addon-resizer#addon-resizer-configuration).
{: tip}
