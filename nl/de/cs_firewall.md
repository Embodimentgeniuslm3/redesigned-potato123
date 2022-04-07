---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

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



# Erforderliche Ports und IP-Adressen in Ihrer Firewall öffnen
{: #firewall}

Überprüfen Sie die hier aufgeführten Situationen, in denen Sie möglicherweise bestimmte Ports und IP-Adressen in Ihren Firewalls {{site.data.keyword.containerlong}} öffnen müssen, um folgende Aktionen zu ermöglichen:
{:shortdesc}

* [Ausführen von `ibmcloud`- und `ibmcloud ks`-Befehlen](#firewall_bx) aus Ihrem lokalen System, wenn die Netzrichtlinien des Unternehmens den Zugriff auf öffentliche Internetendpunkte über Proxys oder Firewalls verhindern.
* [Ausführen von `kubectl`-Befehlen](#firewall_kubectl) aus Ihrem lokalen System, wenn die Netzrichtlinien des Unternehmens den Zugriff auf öffentliche Internetendpunkte über Proxys oder Firewalls verhindern.
* [Ausführen von `calicoctl`-Befehlen](#firewall_calicoctl) aus Ihrem lokalen System, wenn die Netzrichtlinien des Unternehmens den Zugriff auf öffentliche Internetendpunkte über Proxys oder Firewalls verhindern.
* [Zulassen der Kommunikation zwischen dem Kubernetes-Master und den Workerknoten](#firewall_outbound), wenn entweder eine Firewall für die Workerknoten eingerichtet wurde oder wenn die Firewalleinstellungen in Ihrem IBM Cloud-Infrastrukturkonto (SoftLayer) angepasst wurden.
* [Zulassen, dass der Cluster über eine Firewall im privaten Netz auf die Ressourcen zugreift](#firewall_private).
* [Zulassen, dass der Cluster auf Ressourcen zugreift, wenn Calico-Netzrichtlinien den Workerknoten-Egress blockieren](#firewall_calico_egress).
* [Zugreifen auf den NodePort-Service, Lastausgleichsservice oder Ingress-Service von außerhalb des Clusters](#firewall_inbound).
* [Zulassen, dass der Cluster auf Services zugreift, die innerhalb oder außerhalb von {{site.data.keyword.Bluemix_notm}} oder lokal ausgeführt werden und durch eine Firewall geschützt sind](#whitelist_workers).

<br />


## `ibmcloud`- und `ibmcloud ks`-Befehle von hinter einer Firewall ausführen
{: #firewall_bx}

Falls Unternehmensnetzrichtlinien den Zugriff von Ihrem lokalen System auf öffentliche Endpunkte über Proxys oder Firewalls verhindern, müssen Sie TCP-Zugriff für {{site.data.keyword.Bluemix_notm}} und {{site.data.keyword.containerlong_notm}} zulassen, um `ibmcloud`- und `ibmcloud ks`-Befehle auszuführen.
{:shortdesc}

1. Ermöglichen Sie Zugriff auf `cloud.ibm.com` an Port 443 in Ihrer Firewall.
2. Überprüfen Sie Ihre Verbindung, indem Sie sich über diesen API-Endpunkt bei {{site.data.keyword.Bluemix_notm}} anmelden.
  ```
  ibmcloud login -a https://cloud.ibm.com/
  ```
  {: pre}
3. Ermöglichen Sie Zugriff auf `containers.cloud.ibm.com` an Port 443 in Ihrer Firewall.
4. Überprüfen Sie Ihre Verbindung. Ist der Zugriff korrekt konfiguriert, werden in der Ausgabe Schiffe angezeigt.
   ```
   curl https://containers.cloud.ibm.com/v1/
   ```
   {: pre}

   Beispielausgabe:
   ```
                                     )___(
                              _______/__/_
                     ___     /===========|   ___
    ____       __   [\\\]___/____________|__[///]   __
    \   \_____[\\]__/___________________________\__[//]___
     \                                                    |
      \                                                  /
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ```
   {: screen}

<br />


## `kubectl`-Befehle von hinter einer Firewall ausführen
{: #firewall_kubectl}

Falls Unternehmensnetzrichtlinien den Zugriff von Ihrem lokalen System auf öffentliche Endpunkte über Proxys oder Firewalls verhindern, müssen Sie TCP-Zugriff für den Cluster zulassen, um `kubectl`-Befehle auszuführen.
{:shortdesc}

Wenn ein Cluster erstellt wird, wird der Port in den Serviceendpunkt-URLs zufällig mit einer Nummer zwischen 20000-32767 zugewiesen. Sie können entweder den Portbereich 20000-32767 für alle Cluster öffnen, die eventuell erstellt werden, oder Sie können den Zugriff für einen spezifischen vorhandenen Cluster gewähren.

Vorab müssen Sie zulassen, dass [`ibmcloud ks`-Befehle ausgeführt werden](#firewall_bx).

Gehen Sie wie folgt vor, um Zugriff auf einen bestimmten Cluster zu gewähren:

1. Melden Sie sich an der {{site.data.keyword.Bluemix_notm}}-CLI an. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wenn Sie dazu aufgefordert werden. Wenn Sie über ein föderiertes Konto verfügen, schließen Sie die Option `--sso` ein.

   ```
   ibmcloud login [--sso]
   ```
   {: pre}

2. Wenn sich der Cluster in einer anderen Ressourcengruppe als `default` befindet, geben Sie diese Ressourcengruppe als Ziel an. Um die Ressourcengruppen anzuzeigen, zu denen die einzelnen Cluster gehören, führen Sie `ibmcloud ks clusters` aus. **Hinweis:** Sie müssen mindestens über die [Plattformrolle **Anzeigeberechtigter**](/docs/containers?topic=containers-users#platform) für die Ressourcengruppe verfügen.
   ```
   ibmcloud target -g <ressourcengruppenname>
   ```
   {: pre}

4. Rufen Sie den Namen Ihres Clusters ab.

   ```
   ibmcloud ks clusters
   ```
   {: pre}

5. Rufen Sie die Serviceendpunkt-URLs für Ihren Cluster ab.
 * Wenn nur das Feld **Public Service Endpoint URL** einen Wert enthält, nehmen Sie diese URL. Ihre berechtigten Clusterbenutzer können auf den Kubernetes-Master über diesen Endpunkt im öffentlichen Netz zugreifen.
 * Wenn nur das Feld **Private Service Endpoint URL** einen Wert enthält, nehmen Sie diese URL. Ihre berechtigten Clusterbenutzer können auf den Kubernetes-Master über diesen Endpunkt im privaten Netz zugreifen.
 * Wenn die beiden Felder **Public Service Endpoint URL** und **Private Service Endpoint URL** Werte enthalten, nehmen Sie beide URLs. Ihre berechtigten Clusterbenutzer können auf den Kubernetes-Master über den öffentlichen Endpunkt im öffentlichen Netz oder über den privaten Endpunkt im privaten Netz zugreifen.

  ```
  ibmcloud ks cluster-get --cluster <clustername_oder_-id>
  ```
  {: pre}

  Beispielausgabe:
  ```
  ...
  Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
  Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
  ...
  ```
  {: screen}

6. Ermöglichen Sie den Zugriff auf die Serviceendpunkt-URLs und Ports, die Sie im vorherigen Schritt abgerufen haben. Wenn Ihre Firewall IP-basiert arbeitet, können Sie die IP-Adressen, die geöffnet werden, wenn Sie den Zugriff auf die Serviceendpunkt-URLs ermöglichen, [in dieser Tabelle](#master_ips) ermitteln.

7. Überprüfen Sie Ihre Verbindung.
  * Wenn der öffentliche Serviceendpunkt aktiviert ist:
    ```
    curl --insecure <öffentliche_serviceendpunkt-url>/version
    ```
    {: pre}

    Beispielbefehl:
    ```
    curl --insecure https://c3.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    Beispielausgabe:
    ```
    {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
    ```
    {: screen}
  * Wenn der private Serviceendpunkt aktiviert ist, müssen Sie sich in Ihrem privaten {{site.data.keyword.Bluemix_notm}}-Netz befinden, oder eine Verbindung zu dem privaten Netz über eine VPN-Verbindung herstellen, um die Verbindung zum Master zu prüfen:
    ```
    curl --insecure <private_serviceendpunkt-url>/version
    ```
    {: pre}

    Beispielbefehl:
    ```
    curl --insecure https://c3-private.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    Beispielausgabe:
    ```
    {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
    ```
    {: screen}

8. Optional: Wiederholen Sie diese Schritte für jeden Cluster, den Sie zugänglich machen müssen.

<br />


## `calicoctl`-Befehle von hinter einer Firewall ausführen
{: #firewall_calicoctl}

Falls Unternehmensnetzrichtlinien den Zugriff von Ihrem lokalen System auf öffentliche Endpunkte über Proxys oder Firewalls verhindern, müssen Sie TCP-Zugriff für die Calico-Befehle zulassen, um `calicoctl`-Befehle auszuführen.
{:shortdesc}

Vorab müssen Sie zulassen, dass [`ibmcloud`-Befehle](#firewall_bx) und [`kubectl`-Befehle](#firewall_kubectl) ausgeführt werden.

1. Rufen Sie die IP-Adresse aus der Master-URL ab, die Sie verwendet haben, um die [`kubectl`-Befehle](#firewall_kubectl) zuzulassen.

2. Rufen Sie den Port für 'etcd' ab.

  ```
  kubectl get cm -n kube-system cluster-info -o yaml | grep etcd_host
  ```
  {: pre}

3. Ermöglichen Sie Zugriff für die Calico-Richtlinien über die IP-Adresse und den 'etcd'-Port der Master-URL.

<br />


## Zugriff des Clusters auf Infrastrukturressourcen und andere Services über eine öffentliche Firewall ermöglichen
{: #firewall_outbound}

Ermöglichen Sie Ihrem Cluster den Zugriff auf Infrastrukturressourcen und Services von hinter einer öffentlichen Firewall, z. B. für {{site.data.keyword.containerlong_notm}}-Regionen, {{site.data.keyword.registrylong_notm}}, {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM), {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, private IP-Adressen der IBM Cloud-Infrastruktur (SoftLayer) und Egress für Persistent Volume Claims (PVCs).
{:shortdesc}

Abhängig von Ihrem Cluster-Setup greifen Sie auf die Services über die öffentlichen IP-Adressen, über die privaten IP-Adressen oder über beide Typen von IP-Adressen zu. Wenn Sie einen Cluster mit Workerknoten im öffentlichen VLAN und im privaten VLAN hinter einer Firewall für das öffentliche und das private Netz haben, müssen Sie die Verbindung sowohl für öffentliche als auch für private IP-Adressen öffnen. Wenn Ihr Cluster nur Workerknoten im privaten VLAN hinter einer Firewall hat, können Sie die Verbindung nur zu den privaten IP-Adressen öffnen.
{: note}

1.  Notieren Sie die öffentlichen IP-Adressen für alle Workerknoten im Cluster.

    ```
    ibmcloud ks workers --cluster <clustername_oder_-id>
    ```
    {: pre}

2.  Ermöglichen Sie Netzverkehr von der Quelle <em>&lt;öffentliche_IP_für_jeden_workerknoten&gt;</em> zum Ziel-TCP/UDP-Portbereich 20000-32767 und zum Port 443 sowie für die folgenden IP-Adressen und Netzgruppen. Wenn Ihre Unternehmensfirewall verhindert, dass Ihr lokales System auf öffentliche Internetendpunkte zugreifen kann, führen Sie diesen Schritt sowohl für Ihre Quellen-Workerknoten als auch für Ihr lokales System aus.

    Sie müssen den ausgehenden Datenverkehr am Port 443 für alle Zonen in der Region zulassen, um die Arbeitslast während des Bootstrap-Prozesses auszugleichen. Wenn sich Ihr Cluster beispielsweise in den USA (Süden) befindet, dann müssen Sie den Datenverkehr von den öffentlichen IPs jedes Ihrer Workerknoten an Port 443 der IP-Adresse für alle Zonen zulassen.
    {: important}

    {: #master_ips}
    <table summary="Die erste Zeile in der Tabelle erstreckt sich über beide Spalten. Die verbleibenden Zeilen enthalten von links nach rechts die jeweilige Serverzone in der ersten Spalte und die entsprechenden IP-Adressen in der zweiten Spalte.">
      <caption>Zu öffnende IP-Adressen für abgehenden Datenverkehr</caption>
          <thead>
          <th>Region</th>
          <th>Zone</th>
          <th>Öffentliche IP-Adresse</th>
          <th>Private IP-Adresse</th>
          </thead>
        <tbody>
          <tr>
            <td>Asien-Pazifik (Norden)</td>
            <td>che01<br>hkg02<br>seo01<br>sng01<br><br>tok02, tok04, tok05</td>
            <td><code>169.38.70.10</code><br><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><br><code>161.202.126.210, 128.168.71.117, 165.192.69.69</code></td>
            <td><code>166.9.40.7</code><br><code>166.9.42.7</code><br><code>166.9.44.5</code><br><code>166.9.40.8</code><br><br><code>166.9.40.6, 166.9.42.6, 166.9.44.4</code></td>
           </tr>
          <tr>
             <td>Asien-Pazifik (Süden)</td>
             <td>mel01<br><br>syd01, syd04, syd05</td>
             <td><code>168.1.97.67</code><br><br><code>168.1.8.195, 130.198.66.26, 168.1.12.98, 130.198.64.19</code></td>
             <td><code>166.9.54.10</code><br><br><code>166.9.52.14, 166.9.52.15, 166.9.54.11, 166.9.54.13, 166.9.54.12</code></td>
          </tr>
          <tr>
             <td>Mitteleuropa</td>
             <td>ams03<br>mil01<br>osl01<br>par01<br><br>fra02, fra04, fra05</td>
             <td><code>169.50.169.110, 169.50.154.194</code><br><code>159.122.190.98, 159.122.141.69</code><br><code>169.51.73.50</code><br><code>159.8.86.149, 159.8.98.170</code><br><br><code>169.50.56.174, 161.156.65.42, 149.81.78.114</code></td>
             <td><code>166.9.28.17, 166.9.30.11</code><br><code>166.9.28.20, 166.9.30.12</code><br><code>166.9.32.8</code><br><code>166.9.28.19, 166.9.28.22</code><br><br><code>	166.9.28.23, 166.9.30.13, 166.9.32.9</code></td>
            </tr>
          <tr>
            <td>Vereinigtes Königreich (Süden)</td>
            <td>lon02, lon04, lon05, lon06</td>
            <td><code>159.122.242.78, 158.175.111.42, 158.176.94.26, 159.122.224.242, 158.175.65.170, 158.176.95.146</code></td>
            <td><code>166.9.34.5, 166.9.34.6, 166.9.36.10, 166.9.36.11, 166.9.36.12, 166.9.36.13, 166.9.38.6, 166.9.38.7</code></td>
          </tr>
          <tr>
            <td>Vereinigte Staaten (Osten)</td>
             <td>mon01<br>tor01<br><br>wdc04, wdc06, wdc07</td>
             <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><br><code>169.63.88.186, 169.60.73.142, 169.61.109.34, 169.63.88.178, 169.60.101.42, 169.61.83.62</code></td>
             <td><code>166.9.20.11</code><br><code>166.9.22.8</code><br><br><code>166.9.20.12, 166.9.20.13, 166.9.22.9, 166.9.22.10, 166.9.24.4, 166.9.24.5</code></td>
          </tr>
          <tr>
            <td>Vereinigte Staaten (Süden)</td>
            <td>hou02<br>mex01<br>sao01<br>sjc03<br>sjc04<br><br>dal10,dal12,dal13</td>
            <td><code>184.173.44.62</code><br><code>169.57.100.18</code><br><code>169.57.151.10</code><br><code>169.45.67.210</code><br><code>169.62.82.197</code><br><br><code>169.46.7.238, 169.48.230.146, 169.61.29.194, 169.46.110.218, 169.47.70.10, 169.62.166.98, 169.48.143.218, 169.61.177.2, 169.60.128.2</code></td>
            <td><code>166.9.15.74</code><br><code>166.9.15.76</code><br><code>166.9.12.143</code><br><code>166.9.12.144</code><br><code>166.9.15.75</code><br><br><code>166.9.12.140, 166.9.12.141, 166.9.12.142, 166.9.15.69, 166.9.15.70, 166.9.15.72, 166.9.15.71, 166.9.15.73, 166.9.16.183, 166.9.16.184, 166.9.16.185</code></td>
          </tr>
          </tbody>
        </table>

3.  Lassen Sie ausgehenden Netzverkehr von den Workerknoten an [{{site.data.keyword.registrylong_notm}}-Regionen](/docs/services/Registry?topic=registry-registry_overview#registry_regions) zu:
  - `TCP-Port 443, Port 4443 VON <each_worker_node_publicIP> ZU <registry_subnet>`
  -  Ersetzen <em>&lt;registry-teilnetz&gt;</em> durch das Registry-Teilnetz, zu dem Sie Datenverkehr zulassen wollen. In der globalen Registry werden von IBM bereitgestellte öffentliche Images gespeichert. Ihre eigenen privaten oder öffentlichen Images werden in regionalen Registrys gespeichert. Port 4443 ist für notarielle Funktionen erforderlich, wie zum Beispiel die [Überprüfung von Imagesignaturen](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent). <table summary="Die erste Zeile in der Tabelle erstreckt sich über beide Spalten. Die verbleibenden Zeilen enthalten von links nach rechts die jeweilige Serverzone in der ersten Spalte und die entsprechenden IP-Adressen in der zweiten Spalte.">
  <caption>Zu öffnende IP-Adressen für Registry-Datenverkehr</caption>
    <thead>
      <th>{{site.data.keyword.containerlong_notm}}-Region</th>
      <th>Registry-Adresse</th>
      <th>Öffentliche Registry-Teilnetze</th>
      <th>Private Registry-IP-Adressen</th>
    </thead>
    <tbody>
      <tr>
        <td>Über <br>{{site.data.keyword.containerlong_notm}}-Regionen hinweg verfügbare globale Registry</td>
        <td><strong>Öffentlich:</strong> <code>icr.io</code><br>
        Veraltet: <code>registry.bluemix.net</code><br><br>
        <strong>Privat:</strong> <code>z1-1.private.icr.io<br>z2-1.private.icr.io<br>z3-1.private.icr.io</code></td>
        <td><code>169.60.72.144/28</code></br><code>169.61.76.176/28</code></br><code>169.62.37.240/29</code></br><code>169.60.98.80/29</code></br><code>169.63.104.232/29</code></td>
        <td><code>166.9.20.4</code></br><code>166.9.22.3</code></br><code>166.9.24.2</code></td>
      </tr>
      <tr>
        <td>Asien-Pazifik (Norden)</td>
        <td><strong>Öffentlich:</strong> <code>jp.icr.io</code><br>
        Veraltet: <code>registry.au-syd.bluemix.net</code><br><br>
        <strong>Privat:</strong> <code>z1-1.private.jp.icr.io<br>z2-1.private.jp.icr.io<br>z3-1.private.jp.icr.io</code></td>
        <td><code>161.202.146.86/29</code></br><code>128.168.71.70/29</code></br><code>165.192.71.222/29</code></td>
        <td><code>166.9.40.3</code></br><code>166.9.42.3</code></br><code>166.9.44.3</code></td>
      </tr>
      <tr>
        <td>Asien-Pazifik (Süden)</td>
        <td><strong>Öffentlich:</strong> <code>au.icr.io</code><br>
        Veraltet: <code>registry.au-syd.bluemix.net</code><br><br>
        <strong>Privat:</strong> <code>z1-1.private.au.icr.io<br>z2-1.private.au.icr.io<br>z3-1.private.au.icr.io</code></td>
        <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></br><code>168.1.1.240/29</code></br><code>130.198.88.128/29</code></br><code>135.90.66.48/29</code></td>
        <td><code>166.9.52.2</code></br><code>166.9.54.2</code></br><code>166.9.56.3</code></td>
      </tr>
      <tr>
        <td>Mitteleuropa</td>
        <td><strong>Öffentlich:</strong> <code>de.icr.io</code><br>
        Veraltet: <code>registry.eu-de.bluemix.net</code><br><br>
        <strong>Privat:</strong> <code>z1-1.private.de.icr.io<br>z2-1.private.de.icr.io<br>z3-1.private.de.icr.io</code></td>
        <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></br><code>169.50.58.104/29</code></br><code>161.156.93.16/29</code></br><code>149.81.79.152/29</code></td>
        <td><code>166.9.28.12</code></br><code>166.9.30.9</code></br><code>166.9.32.5</code></td>
       </tr>
       <tr>
        <td>Vereinigtes Königreich (Süden)</td>
        <td><strong>Öffentlich:</strong> <code>uk.icr.io</code><br>
        Veraltet: <code>registry.eu-gb.bluemix.net</code><br><br>
        <strong>Privat:</strong> <code>z1-1.private.uk.icr.io<br>z2-1.private.uk.icr.io<br>z3-1.private.uk.icr.io</code></td>
        <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></br><code>158.175.97.184/29</code></br><code>158.176.105.64/29</code></br><code>141.125.71.136/29</code></td>
        <td><code>166.9.36.9</code></br><code>166.9.38.5</code></br><code>166.9.34.4</code></td>
       </tr>
       <tr>
        <td>Vereinigte Staaten (Osten), Vereinigte Staaten (Süden)</td>
        <td><strong>Öffentlich:</strong> <code>us.icr.io</code><br>
        Veraltet: <code>registry.ng.bluemix.net</code><br><br>
        <strong>Privat:</strong> <code>z1-1.private.us.icr.io<br>z2-1.private.us.icr.io<br>z3-1.private.us.icr.io</code></td>
        <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></br><code>169.61.234.224/29</code></br><code>169.61.135.160/29</code></br><code>169.61.46.80/29</code></td>
        <td><code>166.9.12.114</code></br><code>166.9.15.50</code></br><code>166.9.16.173</code></td>
       </tr>
      </tbody>
    </table>

4. Optional: Lassen Sie den ausgehenden Netzverkehr von den Workerknoten an {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, Sysdig und LogDNA-Services zu:
    *   **{{site.data.keyword.monitoringlong_notm}}**:
        <pre class="screen">TCP-Port 443, Port 9095 VON &lt;jede_öffentliche_workerknoten-ip&gt; ZU &lt;überwachungsteilnetz&gt;</pre>
        Ersetzen Sie <em>&lt;überwachungsteilnetz&gt;</em> durch die Teilnetze für die Überwachungsregionen, an die der Datenverkehr zugelassen werden soll:
        <p><table summary="Die erste Zeile in der Tabelle erstreckt sich über beide Spalten. Die verbleibenden Zeilen enthalten von links nach rechts die jeweilige Serverzone in der ersten Spalte und die entsprechenden IP-Adressen in der zweiten Spalte.">
  <caption>Zu öffnende IP-Adressen für die Überwachung von Datenverkehr</caption>
        <thead>
        <th>{{site.data.keyword.containerlong_notm}}-Region</th>
        <th>Überwachungsadresse</th>
        <th>Überwachungsteilnetze</th>
        </thead>
      <tbody>
        <tr>
         <td>Mitteleuropa</td>
         <td><code>metrics.eu-de.bluemix.net</code></td>
         <td><code>158.177.65.80/30</code></td>
        </tr>
        <tr>
         <td>Vereinigtes Königreich (Süden)</td>
         <td><code>metrics.eu-gb.bluemix.net</code></td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>Vereinigte Staaten (Osten), Vereinigte Staaten (Süden), Asien-Pazifik (Norden), Asien-Pazifik (Süden)</td>
          <td><code>metrics.ng.bluemix.net</code></td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
    *   **{{site.data.keyword.mon_full_notm}}**:
        <pre class="screen">TCP-Port 443, Port 6443 VON &lt;jede_öffentliche_workerknoten-ip&gt; ZU &lt;öffentliche_sysdig-ip&gt;</pre>
        Ersetzen Sie `<öffentliche_sysdig-ip>` durch die [Sysdig-IP-Adressen](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-network#network).
    *   **{{site.data.keyword.loganalysislong_notm}}**:
        <pre class="screen">TCP-Port 443, Port 9091 VON &lt;jede_öffentliche_workerknoten-ip&gt; ZU &lt;öffentliche_ip_der_protokollierung&gt;</pre>
        Ersetzen Sie <em>&lt;öffentliche_ip_der_protokollierung&gt;</em> durch alle Adressen für die Protokollierungsregionen, an die der Datenverkehr als zulässig definiert werden soll:
        <p><table summary="Die erste Zeile in der Tabelle erstreckt sich über beide Spalten. Die verbleibenden Zeilen enthalten von links nach rechts die jeweilige Serverzone in der ersten Spalte und die entsprechenden IP-Adressen in der zweiten Spalte.">
<caption>Zu öffnende IP-Adressen für die Protokollierung von Datenverkehr</caption>
        <thead>
        <th>{{site.data.keyword.containerlong_notm}}-Region</th>
        <th>Protokollierungsadresse</th>
        <th>IP-Adressen für die Protokollierung</th>
        </thead>
        <tbody>
          <tr>
            <td>Vereinigte Staaten (Osten), Vereinigte Staaten (Süden)</td>
            <td><code>ingest.logging.ng.bluemix.net</code></td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
          </tr>
          <tr>
           <td>Vereinigtes Königreich (Süden)</td>
           <td><code>ingest.logging.eu-gb.bluemix.net</code></td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>Mitteleuropa</td>
           <td><code>ingest-eu-fra.logging.bluemix.net</code></td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>Asien-Pazifik (Süden), Asien-Pazifik (Norden)</td>
           <td><code>ingest-au-syd.logging.bluemix.net</code></td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>
    *   **{{site.data.keyword.la_full_notm}}**:
        <pre class="screen">TCP-Port 443, Port 80 VON &lt;öffentliche_ip_jedes_workerknotens&gt; ZU &lt;öffentliche_logDNA-ip&gt;</pre>
        Ersetzen Sie `<öffentliche_logDNA-ip>` durch die [LogDNA-IP-Adressen](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-network#network).

5. Wenn Sie Services der Lastausgleichsfunktion verwenden, müssen Sie sicherstellen, dass der gesamte Datenverkehr, der das VRRP-Protokoll verwendet, zwischen Workerknoten auf öffentlichen und privaten Schnittstellen zulässig ist. {{site.data.keyword.containerlong_notm}} verwendet das VRRP-Protokoll, um IP-Adressen für öffentliche und private Lastausgleichsfunktionen zu verwalten.

6. {: #pvc}Stellen Sie zum Erstellen von Persistent Volume Claims (PVCs) in einem privaten Cluster sicher, dass Ihr Cluster mit der folgenden Kubernetes-Version bzw. den folgenden Versionen des {{site.data.keyword.Bluemix_notm}}-Speicher-Plug-ins eingerichtet ist. Diese Versionen ermöglichen die Kommunikation über das private Netz aus Ihrem Cluster zu Instanzen des persistenten Speichers (PV-Instanzen).
    <table>
    <caption>Übersicht über erforderliche Kubernetes- oder {{site.data.keyword.Bluemix_notm}}-Speicher-Plug-in-Versionen für private Cluster</caption>
    <thead>
      <th>Speichertyp</th>
      <th>Erforderliche Version</th>
   </thead>
   <tbody>
     <tr>
       <td>Dateispeicher</td>
       <td>Kubernetes Version <code>1.13.4_1512</code>, <code>1.12.6_1544</code>, <code>1.11.8_1550</code>, <code>1.10.13_1551</code> oder höher</td>
     </tr>
     <tr>
       <td>Blockspeicher</td>
       <td>{{site.data.keyword.Bluemix_notm}} Block Storage-Plug-in Version 1.3.0 oder höher</td>
     </tr>
     <tr>
       <td>Objektspeicher</td>
       <td><ul><li>{{site.data.keyword.cos_full_notm}}-Plug-in Version 1.0.3 oder höher</li><li>{{site.data.keyword.cos_full_notm}}-Service mit HMAC-Authentifizierung</li></ul></td>
     </tr>
   </tbody>
   </table>

   Wenn Sie eine Kubernetes-Version oder eine Version des {{site.data.keyword.Bluemix_notm}}-Speicher-Plug-ins verwenden müssen, die die Netzkommunikation über das private Netz nicht unterstützt, oder wenn Sie {{site.data.keyword.cos_full_notm}} ohne HMAC-Authentifizierung verwenden wollen, lassen Sie den Egress-Zugriff durch Ihre Firewall auf die IBM Cloud-Infrastruktur (SoftLayer) und auf {{site.data.keyword.Bluemix_notm}} Identity and Access Management zu:
   - Lassen Sie Egress-Netzverkehr über TCP-Port 443 zu.
   - Lassen Sie den Zugriff auf den IP-Bereich der IBM Cloud-Infrastruktur (SoftLayer) für die Zone, in der sich Ihr Cluster befindet, sowohl für das [**Front-End-Netz (öffentlich)**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#frontend-public-network) als auch für das [**Back-End-Netz (privat)**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network) zu. Zum Ermitteln der Zone Ihres Clusters führen Sie den Befehl `ibmcloud ks clusters` aus.


<br />


## Zugriff des Clusters auf Ressourcen über eine private Firewall ermöglichen
{: #firewall_private}

Wenn Sie über eine Firewall im privaten Netz verfügen, lassen Sie die Kommunikation zwischen den Workerknoten zu und lassen Sie Ihren Cluster über das private Netz auf die Infrastrukturressourcen zugreifen.
{:shortdesc}

1. Lassen Sie den gesamten Datenverkehr zwischen den Workerknoten zu.
    1. Lassen den gesamten TCP-, UDP-, VRRP- und IPEncap-Datenverkehr zwischen Workerknoten auf öffentlichen und privaten Schnittstelle zu. Von {{site.data.keyword.containerlong_notm}} wird das VRRP-Protokoll zum Verwalten von IP-Adressen für private Lastausgleichsfunktionen und das IPEncap-Protokoll für den Datenverkehr zwischen den Pods in den Teilnetzen verwendet.
    2. Wenn Sie Calico-Richtlinien verwenden oder wenn Sie über Firewalls in jeder Zone eines Mehrzonenclusters verfügen, kann eine Firewall möglicherweise die Kommunikation zwischen den Workerknoten blockieren. Sie müssen alle Workerknoten im Cluster füreinander öffnen, indem Sie die Worker-Ports, die privaten IP-Adressen der Worker oder die Calico-Workerknotenbezeichnung verwenden.

2. Lassen Sie für die IBM Cloud-Infrastruktur (SoftLayer) private IP-Bereiche zu, sodass Sie Workerknoten in Ihrem Cluster erstellen können.
    1. Lassen Sie für die entsprechende IBM Cloud-Infrastruktur (SoftLayer) private IP-Bereiche zu. Weitere Informationen finden Sie unter [Back-End-Netz (privat)](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network).
    2. Lassen Sie für die IBM Cloud-Infrastruktur (SoftLayer) private IP-Bereiche für alle [Zonen](/docs/containers?topic=containers-regions-and-zones#zones) zu, die Sie verwenden. Beachten Sie, dass Sie IPs für die Zonen `dal01` und `wdc04` hinzufügen müssen. Weitere Informationen finden Sie unter [Servicenetz (im Back-End-Netz/privatem Netz)](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#service-network-on-backend-private-network-).

3. Öffnen Sie die folgenden Ports:
    - Abgehende TCP- und UDP-Verbindungen von den Workern zu den Ports 80 und 443 zulassen, um das Aktualisieren und erneute Laden von Workerknoten zu ermöglichen.
    - Abgehende TCP- und UDP-Verbindungen zu Port 2049 zulassen, um das Anhängen von Dateispeicher als Datenträger zu ermöglichen.
    - Abgehende TCP- und UDP-Verbindungen zu Port 3260 zulassen, um die Kommunikation mit Blockspeicher zu ermöglichen.
    - Eingehende TCP- und UDP-Verbindungen zu Port 10250 für das Kubernetes-Dashboard und Kubernetes-Befehle wie `kubectl logs` und `kubectl exec` zulassen.
    - Eingehende und abgehende TCP- und UDP-Verbindungen zu TCP- und UDP-Port 53 für den DNS-Zugriff zulassen.

4. Wenn Sie auch über eine Firewall im öffentlichen Netz verfügen oder wenn Sie über einen Cluster mit ausschließlich privatem VLAN verfügen und eine Gateway-Appliance als Firewall verwenden, müssen Sie auch die IPs und Ports zulassen, die im Abschnitt [Zugriff des Clusters auf Infrastrukturressourcen und andere Services ermöglichen](#firewall_outbound) angegeben sind.

<br />


## Zugriff des Clusters auf Ressourcen über Calico-Richtlinien für ausgehenden Netzverkehr zulassen
{: #firewall_calico_egress}

Wenn Sie [Calico-Netzrichtlinien](/docs/containers?topic=containers-network_policies) als Firewall verwenden, um den gesamten öffentlichen Worker-Egress einzuschränken, müssen Sie Ihren Workern den Zugriff auf die lokalen Proxys für den Master-API-Server und etcd ermöglichen.
{: shortdesc}

1. [Melden Sie sich an Ihrem Konto an. Geben Sie, sofern anwendbar, die richtige Ressourcengruppe als Ziel an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) Schließen Sie die Optionen `--admin` und `--network` in den Befehl `ibmcloud ks cluster-config` ein. Durch `--admin` werden die Schlüssel für den Zugriff auf Ihr Infrastrukturportfolio und zum Ausführen von Calico-Befehlen auf Ihren Workerknoten heruntergeladen. Durch `--network` wird die Calico-Konfigurationsdatei für die Ausführung aller Calico-Befehle heruntergeladen.
  ```
  ibmcloud ks cluster-config --cluster <clustername_oder_-id> --admin --network
  ```
  {: pre}

2. Erstellen Sie eine Calico-Netzrichtlinie, die den öffentlichen Datenverkehr von Ihrem Cluster an 172.20.0.1:2040 und 172.21.0.1:443 für den lokalen Proxy des API-Servers bzw. 172.20.0.1:2041 für den lokalen etcd-Proxy zulässt.
  ```
  apiVersion: projectcalico.org/v3
  kind: GlobalNetworkPolicy
  metadata:
    name: allow-master-local
  spec:
    egress:
    - action: Allow
      destination:
        ports:
        - 2040:2041
        nets:
        - 172.20.0.1/32
        protocol: UDP
    - action: Allow
      destination:
        ports:
        - 2040:2041
        nets:
        - 172.20.0.1/32
        protocol: TCP
    - action: Allow
      destination:
        ports:
        - 443
        nets:
        - 172.21.0.1/32
        protocol: UDP
    - action: Allow
      destination:
        ports:
        - 443
        nets:
        - 172.21.0.1/32
        protocol: TCP
    order: 1500
    selector: ibm.role == 'worker_public'
    types:
    - Egress
  ```
  {: codeblock}

3. Wenden Sie die Richtlinie auf den Cluster an.
    - Linux und OS X:

      ```
      calicoctl apply -f allow-master-local.yaml
      ```
      {: pre}

    - Windows:

      ```
      calicoctl apply -f filepath/allow-master-local.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

## Auf NodePort-, LoadBalancer- und Ingress-Services von außerhalb des Clusters zugreifen
{: #firewall_inbound}

Sie können eingehenden Zugriff auf NodePort-, LoadBalancer- und Ingress-Services zulassen.
{:shortdesc}

<dl>
  <dt>NodePort-Service</dt>
  <dd>Öffnen Sie den Port, den Sie bei der Bereitstellung des Service an den öffentlichen IP-Adressen für alle Workerknoten konfiguriert haben, mit denen Datenverkehr möglich sein soll. Führen Sie `kubectl get svc` aus, um den Port zu suchen. Der Port liegt im Bereich 20000-32000.<dd>
  <dt>Lastausgleichsservice</dt>
  <dd>Öffnen Sie den Port, den Sie bei der Bereitstellung des LoadBalancer-Service an der öffentlichen IP-Adresse konfiguriert haben.</dd>
  <dt>Ingress</dt>
  <dd>Öffnen Sie Port 80 für HTTP oder Port 443 für HTTPS an der IP-Adresse für die Ingress-Lastausgleichsfunktion für Anwendungen.</dd>
</dl>

<br />


## Cluster in den Firewalls anderer Services oder in lokalen Firewalls in einer Whitelist angeben
{: #whitelist_workers}

Wenn Sie auf Services zugreifen wollen, die innerhalb oder außerhalb von {{site.data.keyword.Bluemix_notm}} oder lokal am Standort ausgeführt und durch eine Firewall geschützt werden, können Sie die IP-Adressen Ihrer Workerknoten in dieser Firewall hinzufügen, um ausgehenden Netzverkehr zu Ihrem Cluster zu ermöglichen. Beispiel: Sie wollen möglicherweise Daten aus einer {{site.data.keyword.Bluemix_notm}}-Datenbank lesen, die durch eine Firewall geschützt wird, oder die Teilnetze Ihrer Workerknoten in einer Whitelist der lokalen Firewall angeben, um Netzverkehr aus Ihrem Cluster zuzulassen.
{:shortdesc}

1.  [Melden Sie sich an Ihrem Konto an. Geben Sie, sofern anwendbar, die richtige Ressourcengruppe als Ziel an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

2. Rufen Sie die Teilnetze oder IP-Adressen der Workerknoten ab.
  * **Workerknotenteilnetze:** Wenn Sie davon ausgehen, dass sich die Anzahl der Workerknoten in Ihrem Cluster häufig ändert, zum Beispiel wenn Sie den [Cluster-Autoscaler](/docs/containers?topic=containers-ca#ca) aktivieren, ist es vielleicht nicht sinnvoll, die Firewall für jeden neuen Workerknoten zu aktualisieren. Stattdessen können Sie die VLAN-Teilnetze, die der Cluster verwendet, in einer Whilelist angeben. Beachten Sie dabei, dass das VLAN-Teilnetz möglicherweise mit Workerknoten in anderen Clustern gemeinsam genutzt wird.
    <p class="note">Die **primären öffentlichen Teilnetze**, die {{site.data.keyword.containerlong_notm}} für Ihren Cluster bereitstellt, enthalten standardmäßig 14 verfügbare IP-Adressen und können mit anderen Clustern im selben VLAN gemeinsam genutzt werden. Wenn Sie mehr als 14 Workerknoten haben, wird ein weiteres Teilnetz bestellt, sodass sich die Teilnetze, die Sie in der Whitelist angeben müssen, ändern können. Zur Verringerung der Änderungshäufigkeit können Sie Worker-Pools mit Typen von Workerknoten mit höheren Kapazitäten an CPU- und Speicherressourcen erstellen, sodass Sie nicht oft Workerknoten hinzufügen müssen.</p>
    1. Listen Sie die Workerknoten in Ihrem Cluster auf.
      ```
      ibmcloud ks workers --cluster <clustername_oder_-id>
      ```
      {: pre}

    2. Notieren Sie in der Ausgabe des Befehls im vorherigen Schritt alle eindeutigen Netz-IDs (erste 3 Oktette) der Spalte **Public IP** für die Workerknoten in Ihrem Cluster.<staging> Wenn Sie einen nur privaten Cluster in einer Whitelist angeben wollen, notieren Sie stattdessen die Netz-IDs der Spalte **Private IP**.<staging> In der folgenden Ausgabe sind die öffentlichen Netz-IDs `169.xx.178` und `169.xx.210`.
        ```
        ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w31   169.xx.178.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.12.7   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w34   169.xx.178.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.12.7  
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w32   169.xx.210.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.12.7   
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w33   169.xx.210.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.12.7  
        ```
        {: screen}
    3.  Listen Sie die VLAN-Teilnetze für jede eindeutige Netz-ID auf.
        ```
        ibmcloud sl subnet list | grep -e <netz-id1> -e <netz-id2>
        ```
        {: pre}

        Beispielausgabe:
        ```
        ID        identifier       type                 network_space   datacenter   vlan_id   IPs   hardware   virtual_servers
        1234567   169.xx.210.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal12        1122334   16    0          5   
        7654321   169.xx.178.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal10        4332211   16    0          6    
        ```
        {: screen}
    4.  Ermitteln Sie die Teilnetzadresse. Suchen Sie in der Ausgabe die Anzahl von **IPs**. Bilden Sie die Potenz `2` hoch `n`, die gleich der Anzahl IPs ist. Beispiel: Wenn die Anzahl der IPs `16` ist, dann entspricht dies der Potenz `2` hoch `4` (`n`) gleich `16`. Ermitteln Sie jetzt das Teilnetz-CIDR, indem Sie den Wert `n` von `32` Bit subtrahieren. Beispiel: Wenn `n` gleich `4` ist, dann ist der CIDR-Wert gleich `28` (nach der Gleichung: `32 - 4 = 28`). Kombinieren Sie die **Netz-ID-Maske** mit dem CIDR-Wert, um die vollständige Teilnetzadresse zu erhalten. In der vorherigen Ausgabe werden die folgenden Teilnetzadressen angegeben:
        *   `169.xx.210.xxx/28`
        *   `169.xx.178.xxx/28`
  * **Einzelne IP-Adressen für Workerknoten:** Wenn Sie eine kleine Anzahl von Workerknoten haben, die nur eine App ausführen und nicht skaliert werden müssen, oder wenn Sie nur einen Workerknoten in einer Whitelist angeben wollen, listen Sie alle Workerknoten in Ihrem Cluster auf und notieren die Adressen der Spalte **Public IP**. Wenn Ihre Workerknoten nur mit einem privaten Netz verbunden sind und Sie eine Verbindung zu {{site.data.keyword.Bluemix_notm}}-Services über den privaten Serviceendpunkt herstellen wollen, notieren Sie stattdessen die IP-Adressen der Spalte **Private IP**. Beachten Sie, dass nur diese Workerknoten in der Whitelist aufgeführt werden. Wenn Sie die Workerknoten löschen oder dem Cluster Workerknoten hinzufügen, müssen Sie Ihre Firewall entsprechend aktualisieren.
    ```
    ibmcloud ks workers --cluster <clustername_oder_-id>
    ```
    {: pre}
4.  Fügen Sie das Teilnetz-CIDR oder die IP-Adressen in der Firewall Ihres Service für ausgehenden Datenverkehr oder in der lokalen Firewall Ihres Standorts für eingehenden Datenverkehr hinzu.
5.  Wiederholen Sie diese Schritte für jeden Cluster, den Sie auf die Whitelist setzen wollen.
