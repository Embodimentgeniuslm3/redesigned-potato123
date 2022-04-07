---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-26"

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



# Netzverkehr auf Edge-Workerknoten beschränken
{: #edge}

Mit Edge-Workerknoten kann die Sicherheit des Kubernetes-Clusters verbessert werden, indem der externe Zugriff auf Workerknoten beschränkt und die Netzarbeitslast in {{site.data.keyword.containerlong}} isoliert wird.
{:shortdesc}

Wenn diese Workerknoten nur für den Netzbetrieb markiert sind, können andere Arbeitslasten nicht die CPU oder den Speicher des entsprechenden Workerknotens nutzen und somit Auswirkungen auf den Netzbetrieb haben.

Wenn Sie über einen Mehrzonencluster verfügen und den Datenaustausch im Netz auf Edge-Workerknoten beschränken möchten, müssen in jeder Zone mindestens zwei Edge-Workerknoten für die Hochverfügbarkeit von Lastausgleichsfunktions- oder Ingress-Pods aktiviert werden. Erstellen Sie einen Worker-Pool für Edge-Workerknoten, der sich über alle Zonen im Cluster erstreckt, mit mindestens zwei Workerknoten pro Zone.
{: tip}

## Bezeichnung für Edge-Knoten zu Workerknoten hinzufügen
{: #edge_nodes}

Fügen Sie die Bezeichnung `dedicated=edge` zu mindestens zwei Workerknoten auf jedem öffentlichen VLAN in Ihrem Cluster hinzu, um sicherzustellen, dass Ingress- und LoadBalancer-Services nur für diese Workerknoten bereitgestellt werden.
{:shortdesc}

Vorbereitende Schritte:

1. Stellen Sie sicher, dass Sie die folgenden [{{site.data.keyword.Bluemix_notm}} IAM-Rollen](/docs/containers?topic=containers-users#platform) innehaben:
  * Beliebige Plattformrolle für den Cluster
  * Servicerolle **Schreibberechtigter** oder **Manager** für alle Namensbereiche
2. [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
3. Stellen Sie sicher, dass der Cluster mindestens über ein öffentliches VLAN verfügt. Edge-Workerknoten stehen nicht für Cluster mit ausschließlich privaten VLANs zur Verfügung.
4. [Erstellen Sie einen neuen Worker-Pool](/docs/containers?topic=containers-clusters#add_pool), der sich über alle Zonen im Cluster erstreckt und mindestens zwei Worker pro Zone aufweist.

Gehen Sie wie folgt vor, um eine Bezeichnung für Edge-Knoten zu Workerknoten hinzuzufügen:

1. Listen Sie die Workerknoten im Worker-Pool für Edge-Knoten auf. Verwenden Sie die Adresse unter **Private IP** zum Identifizieren des Knotens.

  ```
  ibmcloud ks workers --cluster <clustername_oder_-id> --worker-pool <edge-poolname>
  ```
  {: pre}

2. Ordnen Sie den Workerknoten die Bezeichnung `dedicated=edge` zu. Nachdem ein Workerknoten mit der Bezeichnung `dedicated=edge` markiert wurde, werden alle Ingress- und LoadBalancer-Services auf einem Edge-Workerknoten bereitgestellt.

  ```
  kubectl label nodes <knoten1-IP> <knoten2-IP> dedicated=edge
  ```
  {: pre}

3. Rufen Sie alle vorhandenen Lastausgleichsfunktionen und Ingress-Lastausgleichsfunktionen für Anwendungen (ALBs) im Cluster ab.

  ```
  kubectl get services --all-namespaces
  ```
  {: pre}

  Suchen Sie in der Ausgabe Services, für die als **Typ** der Wert **LoadBalancer** angegeben ist. Notieren Sie für jeden Lastausgleichsservice die Angaben unter **Namensbereich** und **Name**. Beispiel: In der folgenden Ausgabe sind drei Lastausgleichsservices vorhanden: die Lastausgleichsfunktion `webserver-lb` im Namensbereich `default` und die Ingress-Lastausgleichsfunktionen für Anwendungen `public-crdf253b6025d64944ab99ed63bb4567b6-alb1` und `public-crdf253b6025d64944ab99ed63bb4567b6-alb2` im Namensbereich `kube-system`.

  ```
  NAMESPACE     NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                      AGE
  default       kubernetes                                       ClusterIP      172.21.0.1       <none>          443/TCP                      1h
  default       webserver-lb                                     LoadBalancer   172.21.190.18    169.46.17.2     80:30597/TCP                 10m
  kube-system   heapster                                         ClusterIP      172.21.101.189   <none>          80/TCP                       1h
  kube-system   kube-dns                                         ClusterIP      172.21.0.10      <none>          53/UDP,53/TCP                1h
  kube-system   kubernetes-dashboard                             ClusterIP      172.21.153.239   <none>          443/TCP                      1h
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb1   LoadBalancer   172.21.84.248    169.48.228.78   80:30286/TCP,443:31363/TCP   1h
  kube-system   public-crdf253b6025d64944ab99ed63bb4567b6-alb2   LoadBalancer   172.21.229.73    169.46.17.6     80:31104/TCP,443:31138/TCP   57m
  ```
  {: screen}

4. Führen Sie unter Verwendung der Ausgabe des vorherigen Schritts den folgenden Befehl für jede Lastausgleichsfunktion und Ingress-Lastausgleichsfunktion für Anwendungen aus. Mit diesem Befehl wird die Lastausgleichsfunktion oder Ingress-Lastausgleichsfunktion für Anwendungen erneut auf einem Edge-Workerknoten bereitgestellt. Nur öffentliche Lastausgleichsfunktionen oder Lastausgleichsfunktionen für Anwendungen müssen erneut bereitgestellt werden.

  ```
  kubectl get service -n <namensbereich> <servicename> -o yaml | kubectl apply -f -
  ```
  {: pre}

  Beispielausgabe:

  ```
  service "my_loadbalancer" configured
  ```
  {: screen}

Sie haben Workerknoten mit der Bezeichnung `dedicated=edge` markiert und alle vorhandenen LoadBalancer- sowie Ingress-Services erneut auf den Workerknoten bereitgestellt. Verhindern Sie nun als nächsten Schritt, dass andere [Arbeitslasten auf Edge-Workerknoten ausgeführt werden](#edge_workloads) und [blockieren Sie eingehenden Datenverkehr an Knotenports auf Workerknoten](/docs/containers?topic=containers-network_policies#block_ingress).

<br />


## Verhindern, dass Workloads auf Edge-Workerknoten ausgeführt werden
{: #edge_workloads}

Ein Vorteil von Edge-Workerknoten besteht darin, dass sie so konfiguriert werden können, dass sie nur Netzservices (Networking Services) ausführen.
{:shortdesc}

Die Verwendung der Tolerierung `dedicated=edge` bedeutet, dass alle LoadBalancer- und Ingress-Services nur auf den markierten Workerknoten ausgeführt werden. Um jedoch zu verhindern, dass andere Workloads auf Edge-Workerknoten ausgeführt werden und deren Ressourcen verbrauchen, müssen Sie [Kubernetes-Taints ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) verwenden.

Vorbereitende Schritte:
- Stellen Sie sicher, dass Sie über die [{{site.data.keyword.Bluemix_notm}} IAM-Servicerolle **Manager** für alle Namensbereiche](/docs/containers?topic=containers-users#platform) verfügen.
- [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Gehen Sie wie folgt vor, um zu verhindern, dass andere Workloads auf Edge-Workerknoten ausgeführt werden:

1. Listen Sie alle Workerknoten mit der Bezeichnung `dedicated=edge` auf.

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated -l dedicated=edge
  ```
  {: pre}

2. Wenden Sie einen Taint auf alle Workerknoten an, der verhindert, dass Pods auf den entsprechenden Workerknoten ausgeführt werden und der Pods von den Workerknoten entfernt, die nicht die Bezeichnung `dedicated=edge` aufweisen. Die entfernten Pods werden auf anderen Workerknoten mit entsprechender Kapazität erneut bereitgestellt.

  ```
  kubectl taint node <knotenname> dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
  {: pre}
  Nun werden nur Pods mit der Tolerierung `dedicated=edge` auf Ihren Edge-Workerknoten bereitgestellt.

3. Wenn Sie die [Beibehaltung der Quellen-IP für einen Lastausgleichsservice der Version 1.0 aktivieren![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer), stellen Sie sicher, dass App-Pods auf den Edge-Workerknoten geplant werden, indem Sie [Edge-Knoten-Affinität zu App-Pods hinzufügen](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes). App-Pods müssen auf Edge-Knoten geplant werden, um eingehende Anforderungen empfangen zu können.

4. Zum Entfernen eines Taints führen Sie den folgenden Befehl aus.
    ```
    kubectl taint node <knotenname> dedicated:NoSchedule- dedicated:NoExecute-
    ```
    {: pre}
