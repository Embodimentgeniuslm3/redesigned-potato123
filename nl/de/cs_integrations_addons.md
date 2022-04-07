---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

keywords: kubernetes, iks, helm

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

# Services unter Verwendung verwalteter Add-ons hinzufügen
{: #managed-addons}

Fügen Sie Ihrem Cluster mit verwalteten Add-ons im Handumdrehen Open-Source-Technologien hinzu.
{: shortdesc}

**Was sind verwaltete Add-ons?** </br>
Verwaltete {{site.data.keyword.containerlong_notm}}-Add-ons sind eine einfache Möglichkeit, den Cluster mit Open-Source-Funktionen wie Istio oder Knative zu erweitern. Die Version des Open-Source-Tools, das Sie Ihrem Cluster hinzufügen, wird von IBM geprüft und für die Verwendung in {{site.data.keyword.containerlong_notm}} genehmigt.

**Wie funktioniert die Abrechnung und Unterstützung für verwaltete Add-ons?** </br>
Verwaltete Add-ons werden vollständig in die {{site.data.keyword.Bluemix_notm}}-Unterstützungsorganisation integriert. Wenn Sie eine Frage oder ein Problem mit der Verwendung der verwalteten Add-ons haben, können Sie einen der {{site.data.keyword.containerlong_notm}}-Unterstützungskanäle verwenden. Weitere Informationen finden Sie im Abschnitt [Hilfe und Unterstützung anfordern](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

Wenn das Tool, das Sie Ihrem Cluster hinzufügen, Kosten verursacht, werden diese automatisch integriert und als Teil Ihrer {{site.data.keyword.containerlong_notm}}-Abrechnung aufgeführt. Der Abrechnungszyklus wird von {{site.data.keyword.Bluemix_notm}} in Abhängigkeit vom Zeitpunkt der Aktivierung des Add-ons im Cluster bestimmt.

**Welche Einschränkungen muss ich berücksichtigen?** </br>
Wenn Sie den [Zugangscontroller zur Durchsetzung der Sicherheit von Container-Images](/docs/services/Registry?topic=registry-security_enforce#security_enforce) in Ihrem Cluster installiert haben, können Sie in Ihrem Cluster keine verwalteten Add-ons aktivieren.

## Verwaltete Add-ons hinzufügen
{: #adding-managed-add-ons}

Zum Aktivieren eines verwalteten Add-ons in Ihrem Cluster können Sie den Befehl [`ibmcloud ks cluster-addon-enable <addon_name> --cluster <clustername_oder_-id>`](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_addon_enable) verwenden. Wenn Sie das verwaltete Add-on aktivieren, wird eine unterstützte Version des Tools, einschließlich aller Kubernetes-Ressourcen, automatisch in Ihrem Cluster installiert. Lesen Sie die Dokumentation zu jedem verwalteten Add-on, um die Voraussetzungen zu ermitteln, die Ihr Cluster erfüllen muss, damit das verwaltete Add-on installiert werden kann.

Weitere Informationen zu den Voraussetzungen für die einzelnen Add-ons finden Sie unter:

- [Istio](/docs/containers?topic=containers-istio#istio)
- [Knative](/docs/containers?topic=containers-knative_tutorial#knative_tutorial)

## Verwaltete Add-ons aktualisieren
{: #updating-managed-add-ons}

Die Versionen der einzelnen verwalteten Add-ons werden von {{site.data.keyword.Bluemix_notm}} getestet und ihre Verwendung in {{site.data.keyword.containerlong_notm}} genehmigt. Verwenden Sie die folgenden Schritte, um die Komponenten eines Add-ons auf die aktuellste Version zu aktualisieren, die von {{site.data.keyword.containerlong_notm}} unterstützt wird.
{: shortdesc}

1. Überprüfen Sie, ob Ihre Add-ons die neueste Version aufweisen. Add-ons, die mit `* (<version> latest)` gekennzeichnet sind, können aktualisiert werden.
   ```
   ibmcloud ks cluster-addon-ls --cluster <mycluster>
   ```
   {: pre}

   Beispielausgabe:
   ```
   OK
   Name      Version
   istio     1.0.6 *(1.1.2 latest)
   knative   0.4.1
   ```
   {: screen}

2. Speichern Sie alle Ressourcen, wie z. B. Konfigurationsdateien für alle Services oder Apps, die Sie in dem Namensbereich erstellt oder geändert haben, der von dem Add-on generiert wurde. Das Istio-Add-on verwendet beispielsweise `istio-system` und das Knative-Add-on `knative-serving`, `knative-monitoring`, `knative-eventing` und `knative-build`. Beispielbefehl:
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

3. Speichern Sie die Kubernetes-Ressourcen, die automatisch im Namensbereich des verwalteten Add-ons generiert wurden, in einer YAML-Datei auf Ihrer lokalen Maschine. Diese Ressourcen werden mithilfe angepasster Ressourcendefinitionen (CRDs) generiert.
   1. Rufen Sie die CRDs für Ihr Add-on aus dem Namensbereich ab, den Ihr Add-on verwendet. Für das Istio-Add-on rufen Sie beispielsweise die CRDs aus dem Namensbereich `istio-system` ab.
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. Speichern Sie alle Ressourcen, die aus diesen CRDs erstellt wurden.

4. Optional für Knative: Wenn Sie eine der folgenden Ressourcen geändert haben, rufen Sie die YAML-Datei ab und speichern Sie sie auf Ihrer lokalen Maschine. Beachten Sie: Wenn Sie eine dieser Ressourcen geändert haben, aber stattdessen die installierte Standardeinstellung verwenden möchten, können Sie die Ressource löschen. Nach einigen Minuten wird die Ressource mit den installierten Standardwerten neu erstellt.
  <table summary="Tabelle der Knative-Ressourcen">
  <caption>Knative-Ressourcen</caption>
  <thead><tr><th>Ressourcenname</th><th>Ressourcentyp</th><th>Namensbereich</th></tr></thead>
  <tbody>
  <tr><td><code>config-autoscaler</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-controller</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-domain</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-gc</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-istio</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-logging</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-network</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-observability</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>kaniko</code></td><td>BuildTemplate</td><td><code>default</code></td></tr>
  <tr><td><code>iks-knative-ingress</code></td><td>Ingress</td><td><code>istio-system</code></td></tr>
  </tbody></table>

  Beispielbefehl:
  ```
  kubectl get cm config-autoscaler -n knative-serving -o yaml > config-autoscaler.yaml
  ```
  {: pre}

5. Wenn Sie die Add-ons `istio-sample-bookinfo` und `istio-extras` aktiviert haben, inaktivieren Sie sie.
   1. Inaktivieren Sie das Add-on `istio-sample-bookinfo`.
      ```
      ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <clustername_oder_-id>
      ```
      {: pre}

   2. Inaktivieren Sie das Add-on `istio-extras`.
      ```
      ibmcloud ks cluster-addon-disable istio-extras --cluster <clustername_oder_-id>
      ```
      {: pre}

6. Inaktivieren Sie das Add-on.
   ```
   ibmcloud ks cluster-addon-disable <add-on_name> --cluster <clustername_oder_-idclustername_oder_-id> -f
   ```
   {: pre}

7. Bevor Sie mit dem nächsten Schritt fortfahren, müssen Sie sicherstellen, dass entweder Ressourcen aus dem Add-on in den Add-on-Namensbereichen oder die Add-on-Namensbereiche selbst entfernt werden.
   * Wenn Sie beispielsweise das Add-on `istio-extras` aktualisieren, könnten Sie sicherstellen, dass die Services `grafana`, `kiali` und `jaeger-*` aus dem Namensbereich `istio-system` entfernt werden.
     ```
     kubectl get svc -n istio-system
     ```
     {: pre}
   * Wenn Sie beispielsweise das Add-on `knative` aktualisieren, könnten Sie sicherstellen, dass die Namensbereiche `knative-serving`, `knative-monitoring`, `knative-eventing`, `knative-build` und `istio-system` gelöscht werden. Die Namensbereiche könnten einige Minuten lang den **STATUS** `Beenden` haben, bevor sie ganz gelöscht werden.
     ```
     kubectl get namespaces -w
     ```
     {: pre}

8. Wählen Sie die Add-on-Version, auf die aktualisiert werden soll.
   ```
   ibmcloud ks addon-versions
   ```
   {: pre}

9. Aktivieren Sie das Add-on erneut. Verwenden Sie das Flag `-- version`, um die Version anzugeben, die Sie installieren wollen. Ist keine Version angegeben, wird die Standardversion installiert.
   ```
   ibmcloud ks cluster-addon-enable <add-on_name> --cluster <clustername_oder_-id> --version <version>
   ```
   {: pre}

10. Wenden Sie die CRD-Ressourcen an, die Sie in Schritt 2 gespeichert haben.
    ```
    kubectl apply -f <dateiname> -n <namensbereich>
    ```
    {: pre}

11. Wenn Sie eine der Ressourcen in Schritt 3 gespeichert haben, wenden Sie sie erneut an.
    Beispielbefehl:
    ```
    kubectl apply -f config-autoscaler.yaml -n knative-serving
    ```
    {: pre}

12. Optional für Istio: Aktivieren Sie die Add-ons `istio-extras` und `istio-sample-bookinfo` erneut. Verwenden Sie für diese Add-ons die gleiche Version wie für das `istio`-Add-on.
    1. Aktivieren Sie das Add-on `istio-extras`.
       ```
       ibmcloud ks cluster-addon-enable istio-extras --cluster <clustername_oder_-id> --version <version>
       ```
       {: pre}

    2. Aktivieren Sie das Add-on `istio-sample-bookinfo`.
       ```
       ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <clustername_oder_-id> --version <version>
       ```
       {: pre}
