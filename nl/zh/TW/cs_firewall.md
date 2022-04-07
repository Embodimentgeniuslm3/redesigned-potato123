---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

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
{:preview: .preview}



# 在防火牆中開啟必要埠及 IP 位址
{: #firewall}

請檢閱下列狀況，在下列狀況時，您可能需要在防火牆中為 {{site.data.keyword.containerlong}} 開啟特定的埠和 IP 位址。
{:shortdesc}

* 當組織網路原則導致無法透過 Proxy 或防火牆存取公用網際網路端點時，從本端系統[執行 `ibmcloud` 指令及 `ibmcloud ks` 指令](#firewall_bx)。
* 當組織網路原則導致無法透過 Proxy 或防火牆存取公用網際網路端點時，從本端系統[執行 `kubectl` 指令](#firewall_kubectl)。
* 當組織網路原則導致無法透過 Proxy 或防火牆存取公用網際網路端點時，從本端系統[執行 `calicoctl` 指令](#firewall_calicoctl)。
* 當已針對工作者節點設定防火牆，或是在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中自訂防火牆設定時，[容許 Kubernetes 主節點與工作者節點之間的通訊](#firewall_outbound)。
* [容許叢集透過專用網路上的防火牆存取資源](#firewall_private)。
* [容許叢集在 Calico 網路原則封鎖工作者節點輸出時存取資源](#firewall_calico_egress)。
* [從叢集外部存取 NodePort 服務、負載平衡器服務或 Ingress](#firewall_inbound)。
* [若要容許叢集存取在 {{site.data.keyword.Bluemix_notm}} 內部或外部或者在內部部署上執行且受防火牆保護的服務](#whitelist_workers)。

<br />


## 在防火牆的保護下執行 `ibmcloud` 及 `ibmcloud ks` 指令
{: #firewall_bx}

如果組織網路原則導致無法透過 Proxy 或防火牆從本端系統存取公用端點，則為了要執行 `ibmcloud ks` 及 `ibmcloud ks` 指令，您必須容許 {{site.data.keyword.Bluemix_notm}} 及 {{site.data.keyword.containerlong_notm}} 的 TCP 存取。
{:shortdesc}

1. 容許在防火牆的埠 443 上存取 `cloud.ibm.com`。
2. 透過此 API 端點登入 {{site.data.keyword.Bluemix_notm}} 來驗證您的連線。
  ```
  ibmcloud login -a https://cloud.ibm.com/
  ```
  {: pre}
3. 容許在防火牆的埠 443 上存取 `containers.cloud.ibm.com`。
4. 驗證連線。如果已正確配置存取，則會在輸出中顯示船。
   ```
   curl https://containers.cloud.ibm.com/v1/
   ```
   {: pre}

   輸出範例：
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


## 在防火牆的保護下執行 `kubectl` 指令
{: #firewall_kubectl}

如果組織網路原則導致無法透過 Proxy 或防火牆從本端系統存取公用端點，則若要執行 `kubectl` 指令，您必須容許叢集的 TCP 存取。
{:shortdesc}

建立叢集時，會從 20000-32767 內隨機指派服務端點 URL 中的埠。您可以針對任何可能建立的叢集選擇開啟埠範圍 20000-32767，或選擇容許存取特定現有叢集。

開始之前，請容許[執行 `ibmcloud ks` 指令](#firewall_bx)的存取。

若要容許存取特定叢集，請執行下列動作：

1. 登入 {{site.data.keyword.Bluemix_notm}} CLI。系統提示時，請輸入您的 {{site.data.keyword.Bluemix_notm}} 認證。如果您有聯合帳戶，請包含 `--sso` 選項。

   ```
   ibmcloud login [--sso]
   ```
   {: pre}

2. 如果叢集位於 `default` 以外的資源群組中，請將目標設為該資源群組。若要查看每一個叢集所屬的資源群組，請執行 `ibmcloud ks clusters`。**附註**：您必須至少具有資源群組的[**檢視者**角色](/docs/containers?topic=containers-users#platform)。
   ```
   ibmcloud target -g <resource_group_name>
   ```
   {: pre}

4. 取得叢集的名稱。

   ```
   ibmcloud ks clusters
   ```
   {: pre}

5. 擷取叢集的服務端點 URL。
 * 如果只移入**公用服務端點 URL**，請取得這個 URL。您已授權的叢集使用者可以在公用網路上透過此端點存取 Kubernetes 主節點。
 * 如果只移入**專用服務端點 URL**，請取得這個 URL。您已授權的叢集使用者可以在專用網路上透過此端點存取 Kubernetes 主節點。
 * 如果**公用服務端點 URL** 和**專用服務端點 URL** 都已移入，請取得這兩個 URL。您已授權的叢集使用者可以在公用網路上透過公用端點或在專用網路上透過專用端點來存取 Kubernetes 主節點。

  ```
   ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
   ```
  {: pre}

  輸出範例：
  ```
  ...
  Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
  Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
  ...
  ```
  {: screen}

6. 容許存取您在前一個步驟中取得的服務端點 URL 和埠。如果您的防火牆是 IP 型，您可以檢閱[本表格](#master_ips)來查看您容許存取服務端點 URL 時有哪些 IP 位址是開啟的。

7. 驗證連線。
  * 如果已啟用公用服務端點：
    ```
    curl --insecure <public_service_endpoint_URL>/version
    ```
    {: pre}

    指令範例：
    ```
    curl --insecure https://c3.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    輸出範例：
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
  * 如果已啟用了專用服務端點，您必須位於 {{site.data.keyword.Bluemix_notm}} 專用網路中或透過 VPN 連線與專用網路連線，以驗證與主節點的連線。請注意，您必須[透過專用負載平衡器公開主節點端點](/docs/containers?topic=containers-clusters#access_on_prem)，以便使用者可以透過 VPN 或 {{site.data.keyword.BluDirectLink}} 連接存取主節點。
    ```
    curl --insecure <private_service_endpoint_URL>/version
    ```
    {: pre}

    指令範例：
    ```
    curl --insecure https://c3-private.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

        輸出範例：
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

8. 選用項目：針對您需要公開的每一個叢集，重複這些步驟。

<br />


## 在防火牆的保護下執行 `calicoctl` 指令
{: #firewall_calicoctl}

如果組織網路原則導致無法透過 Proxy 或防火牆從本端系統存取公用端點，則若要執行 `calicoctl` 指令，您必須容許 Calico 指令的 TCP 存取。
{:shortdesc}

開始之前，請容許執行 [`ibmcloud` 指令](#firewall_bx)及 [`kubectl` 指令](#firewall_kubectl)的存取。

1. 從用來容許 [`kubectl` 指令](#firewall_kubectl)的主要 URL 中擷取 IP 位址。

2. 取得 etcd 的埠。

  ```
  kubectl get cm -n kube-system cluster-info -o yaml | grep etcd_host
  ```
  {: pre}

3. 容許透過主節點 URL IP 位址及 etcd 埠存取 Calico 原則。

<br />


## 容許叢集透過公用防火牆存取基礎架構資源及其他服務
{: #firewall_outbound}

讓您的叢集在公用防火牆的保護下存取基礎架構資源及服務，例如針對 {{site.data.keyword.containerlong_notm}} 地區、{{site.data.keyword.registrylong_notm}}、{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)、{{site.data.keyword.monitoringlong_notm}}、{{site.data.keyword.loganalysislong_notm}}、IBM Cloud 基礎架構 (SoftLayer) 專用 IP，以及持續性磁區要求的輸出。
{:shortdesc}

視您的叢集設定而定，您可以使用公用、專用或這兩個 IP 位址來存取服務。如果您叢集的工作者節點同時在公用及專用網路的防火牆後面的公用及專用網路 VLAN 上，則您必須開啟公用及專用 IP 位址兩者的連線。如果您叢集的工作者節點只在防火牆後面的專用 VLAN 上，則您只能開啟與專用 IP 位址的連線。
{: note}

1.  記下叢集裡所有工作者節點的公用 IP 位址。

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

2.  容許從來源 <em>&lt;each_worker_node_publicIP&gt;</em> 到目的地 TCP/UDP 埠範圍 20000-32767 和埠 443 以及下列 IP 位址和網路群組的送出網路資料流量。這些 IP 位址允許工作者節點與叢集主節點進行通訊。如果您擁有的企業防火牆阻止本端機器存取公用網際網路端點，請同時對本端機器執行此步驟，以便可以存取叢集主節點。

    對於地區內的所有區域，您必須容許對埠 443 的送出資料流量，以平衡引導處理程序期間的負載。例如，如果您的叢集是在美國南部，則必須容許從每個工作者節點的公用 IP 傳輸到所有區域的 IP 位址的埠 443。
    {: important}

    {: #master_ips}
    <table summary="表格中的第一列跨越兩個直欄。其餘的列應該從左到右閱讀，第一欄為伺服器區域，第二欄則為要符合的 IP 位址。">
  <caption>針對送出資料流量開啟的 IP 位址</caption>
          <thead>
          <th>地區</th>
          <th>區域</th>
          <th>公用 IP 位址</th>
          <th>專用 IP 位址</th>
          </thead>
        <tbody>
          <tr>
            <td>亞太地區北部</td>
            <td>che01<br>hkg02<br>seo01<br>sng01<br><br>tok02、tok04、tok05</td>
            <td><code>169.38.70.10</code><br><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><br><code>161.202.126.210、128.168.71.117、165.192.69.69</code></td>
            <td><code>166.9.40.7</code><br><code>166.9.42.7</code><br><code>166.9.44.5</code><br><code>166.9.40.8</code><br><br><code>166.9.40.6, 166.9.42.6, 166.9.44.4</code></td>
           </tr>
          <tr>
             <td>亞太地區南部</td>
             <td>mel01<br><br>syd01, syd04, syd05</td>
             <td><code>168.1.97.67</code><br><br><code>168.1.8.195、130.198.66.26、168.1.12.98、130.198.64.19</code></td>
             <td><code>166.9.54.10</code><br><br><code>166.9.52.14, 166.9.52.15, 166.9.54.11, 166.9.54.13, 166.9.54.12</code></td>
          </tr>
          <tr>
             <td>歐盟中部</td>
             <td>ams03<br>mil01<br>osl01<br>par01<br><br>fra02、fra04、fra05</td>
             <td><code>169.50.169.110、169.50.154.194</code><br><code>159.122.190.98、159.122.141.69</code><br><code>169.51.73.50</code><br><code>159.8.86.149、159.8.98.170</code><br><br><code>169.50.56.174、161.156.65.42、149.81.78.114</code></td>
             <td><code>166.9.28.17, 166.9.30.11</code><br><code>166.9.28.20, 166.9.30.12</code><br><code>166.9.32.8</code><br><code>166.9.28.19, 166.9.28.22</code><br><br><code>	166.9.28.23, 166.9.30.13, 166.9.32.9</code></td>
            </tr>
          <tr>
            <td>英國南部</td>
            <td>lon02、lon04、lon05、lon06</td>
            <td><code>159.122.242.78、158.175.111.42、158.176.94.26、159.122.224.242、158.175.65.170、158.176.95.146</code></td>
            <td><code>166.9.34.5, 166.9.34.6, 166.9.36.10, 166.9.36.11, 166.9.36.12, 166.9.36.13, 166.9.38.6, 166.9.38.7</code></td>
          </tr>
          <tr>
            <td>美國東部</td>
             <td>mon01<br>tor01<br><br>wdc04、wdc06、wdc07</td>
             <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><br><code>169.63.88.186、169.60.73.142、169.61.109.34、169.63.88.178、169.60.101.42、169.61.83.62</code></td>
             <td><code>166.9.20.11</code><br><code>166.9.22.8</code><br><br><code>166.9.20.12, 166.9.20.13, 166.9.22.9, 166.9.22.10, 166.9.24.4, 166.9.24.5</code></td>
          </tr>
          <tr>
            <td>美國南部</td>
            <td>hou02<br>mex01<br>sao01<br>sjc03<br>sjc04<br><br>dal10、dal12、dal13</td>
            <td><code>184.173.44.62</code><br><code>169.57.100.18</code><br><code>169.57.151.10</code><br><code>169.45.67.210</code><br><code>169.62.82.197</code><br><br><code>169.46.7.238、169.48.230.146、169.61.29.194、169.46.110.218、169.47.70.10、169.62.166.98、169.48.143.218、169.61.177.2、169.60.128.2</code></td>
            <td><code>166.9.15.74</code><br><code>166.9.15.76</code><br><code>166.9.12.143</code><br><code>166.9.12.144</code><br><code>166.9.15.75</code><br><br><code>166.9.12.140, 166.9.12.141, 166.9.12.142, 166.9.15.69, 166.9.15.70, 166.9.15.72, 166.9.15.71, 166.9.15.73, 166.9.16.183, 166.9.16.184, 166.9.16.185</code></td>
          </tr>
          </tbody>
        </table>

3.  {: #firewall_registry}若要容許工作者節點與 {{site.data.keyword.registrylong_notm}} 進行通訊，請容許送出的網路資料流量從工作者節點流至 [{{site.data.keyword.registrylong_notm}} 地區](/docs/services/Registry?topic=registry-registry_overview#registry_regions)：
  - `TCP port 443, port 4443 FROM <each_worker_node_publicIP> TO <registry_subnet>`
  -  將 <em>&lt;registry_subnet&gt;</em> 取代為您要容許資料流量的登錄子網路。全球登錄會儲存 IBM 提供的公用映像檔，地區登錄會儲存您自己的專用或公用映像檔。
        公證人功能需要有埠 4443，例如[驗證映像檔簽章](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)。<table summary="表格中的第一列跨越兩個直欄。其餘的列應該從左到右閱讀，第一欄為伺服器區域，第二欄則為要符合的 IP 位址。">
  <caption>針對「登錄」資料流量開啟的 IP 位址</caption>
    <thead>
      <th>{{site.data.keyword.containerlong_notm}} 地區</th>
      <th>登錄位址</th>
      <th>登錄公用子網路</th>
      <th>登錄專用 IP 位址</th>
    </thead>
    <tbody>
      <tr>
        <td>跨 <br>{{site.data.keyword.containerlong_notm}} 地區的全球登錄</td>
        <td><code>icr.io</code><br><br>
        已淘汰：<code>registry.bluemix.net</code></td>
        <td><code>169.60.72.144/28</code></br><code>169.61.76.176/28</code></br><code>169.62.37.240/29</code></br><code>169.60.98.80/29</code></br><code>169.63.104.232/29</code></td>
        <td><code>166.9.20.4</code></br><code>166.9.22.3</code></br><code>166.9.24.2</code></td>
      </tr>
      <tr>
        <td>亞太地區北部</td>
        <td><code>jp.icr.io</code><br><br>
        已淘汰：<code>registry.au-syd.bluemix.net</code></td>
        <td><code>161.202.146.86/29</code></br><code>128.168.71.70/29</code></br><code>165.192.71.222/29</code></td>
        <td><code>166.9.40.3</code></br><code>166.9.42.3</code></br><code>166.9.44.3</code></td>
      </tr>
      <tr>
        <td>亞太地區南部</td>
        <td><code>au.icr.io</code><br><br>
        已淘汰：<code>registry.au-syd.bluemix.net</code></td>
        <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></br><code>168.1.1.240/29</code></br><code>130.198.88.128/29</code></br><code>135.90.66.48/29</code></td>
        <td><code>166.9.52.2</code></br><code>166.9.54.2</code></br><code>166.9.56.3</code></td>
      </tr>
      <tr>
        <td>歐盟中部</td>
        <td><code>de.icr.io</code><br><br>
        已淘汰：<code>registry.eu-de.bluemix.net</code></td>
        <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></br><code>169.50.58.104/29</code></br><code>161.156.93.16/29</code></br><code>149.81.79.152/29</code></td>
        <td><code>166.9.28.12</code></br><code>166.9.30.9</code></br><code>166.9.32.5</code></td>
       </tr>
       <tr>
        <td>英國南部</td>
        <td><code>uk.icr.io</code><br><br>
        已淘汰：<code>registry.eu-gb.bluemix.net</code></td>
        <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></br><code>158.175.97.184/29</code></br><code>158.176.105.64/29</code></br><code>141.125.71.136/29</code></td>
        <td><code>166.9.36.9</code></br><code>166.9.38.5</code></br><code>166.9.34.4</code></td>
       </tr>
       <tr>
        <td>美國東部、美國南部</td>
        <td><code>us.icr.io</code><br><br>
        已淘汰：<code>registry.ng.bluemix.net</code></td>
        <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></br><code>169.61.234.224/29</code></br><code>169.61.135.160/29</code></br><code>169.61.46.80/29</code></td>
        <td><code>166.9.12.114</code></br><code>166.9.15.50</code></br><code>166.9.16.173</code></td>
       </tr>
      </tbody>
    </table>

4. 選用項目：容許從工作者節點到 {{site.data.keyword.monitoringlong_notm}}、{{site.data.keyword.loganalysislong_notm}}、Sysdig 及 LogDNA 服務的送出網路資料流量：
    *   **{{site.data.keyword.monitoringlong_notm}}**:
        <pre class="screen">TCP port 443, port 9095 FROM &lt;each_worker_node_public_IP&gt; TO &lt;monitoring_subnet&gt;</pre>
        將 <em>&lt;monitoring_subnet&gt;</em> 取代為您要容許資料流量的監視地區的子網路：
        <p><table summary="表格中的第一列跨越兩個直欄。其餘的列應該從左到右閱讀，第一欄為伺服器區域，第二欄則為要符合的 IP 位址。">
  <caption>針對監視資料流量開啟的 IP 位址</caption>
        <thead>
        <th>{{site.data.keyword.containerlong_notm}} 地區</th>
        <th>監視位址</th>
        <th>監視子網路</th>
        </thead>
      <tbody>
        <tr>
         <td>歐盟中部</td>
         <td><code>metrics.eu-de.bluemix.net</code></td>
         <td><code>158.177.65.80/30</code></td>
        </tr>
        <tr>
         <td>英國南部</td>
         <td><code>metrics.eu-gb.bluemix.net</code></td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>美國東部、美國南部、亞太地區北部、亞太地區南部</td>
          <td><code>metrics.ng.bluemix.net</code></td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
    *   **{{site.data.keyword.mon_full_notm}}**:
        <pre class="screen">TCP port 443, port 6443 FROM &lt;each_worker_node_public_IP&gt; TO &lt;sysdig_public_IP&gt;</pre>
        將 `<sysdig_public_IP>` 取代為 [Sysdig IP 位址](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-network#network)。
    *   **{{site.data.keyword.loganalysislong_notm}}**:
        <pre class="screen">TCP port 443, port 9091 FROM &lt;each_worker_node_public_IP&gt; TO &lt;logging_public_IP&gt;</pre>
        將 <em>&lt;logging_public_IP&gt;</em> 取代為您要容許資料流量的記載地區的所有位址：
        <p><table summary="表格中的第一列跨越兩個直欄。其餘的列應該從左到右閱讀，第一欄為伺服器區域，第二欄則為要符合的 IP 位址。">
<caption>針對記載資料流量開啟的 IP 位址</caption>
        <thead>
        <th>{{site.data.keyword.containerlong_notm}} 地區</th>
        <th>記載位址</th>
        <th>記載 IP 位址</th>
        </thead>
        <tbody>
          <tr>
            <td>美國東部、美國南部</td>
            <td><code>ingest.logging.ng.bluemix.net</code></td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
          </tr>
          <tr>
           <td>英國南部</td>
           <td><code>ingest.logging.eu-gb.bluemix.net</code></td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>歐盟中部</td>
           <td><code>ingest-eu-fra.logging.bluemix.net</code></td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>亞太地區南部、亞太地區北部</td>
           <td><code>ingest-au-syd.logging.bluemix.net</code></td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>
    *   **{{site.data.keyword.la_full_notm}}**:
        <pre class="screen">TCP port 443, port 80 FROM &lt;each_worker_node_public_IP&gt; TO &lt;logDNA_public_IP&gt;</pre>
        將 `<logDNA_public_IP>` 取代為 [LogDNA IP 位址](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-network#network)。

5. 如果您使用負載平衡器服務，請確定在公用和專用介面的工作者節點之間容許使用 VRRP 通訊協定的所有資料流量。{{site.data.keyword.containerlong_notm}} 使用 VRRP 通訊協定來管理公用及專用負載平衡器的 IP 位址。

6. {: #pvc}若要在專用叢集裡建立持續性磁區要求，請確定已使用下列 Kubernetes 版本或 {{site.data.keyword.Bluemix_notm}} 儲存空間外掛程式版本來設定您的叢集。這些版本啟用從叢集到持續性儲存空間實例的專用網路通訊。
    <table>
    <caption>專用叢集所需的 Kubernetes 或 {{site.data.keyword.Bluemix_notm}} 儲存空間外掛程式版本的概觀</caption>
    <thead>
      <th>儲存空間類型</th>
      <th>必要的版本</th>
   </thead>
   <tbody>
     <tr>
       <td>檔案儲存空間</td>
       <td>Kubernetes <code>1.13.4_1512</code>、<code>1.12.6_1544</code>、<code>1.11.8_1550</code>、<code>1.10.13_1551</code> 版或更新版本</td>
     </tr>
     <tr>
       <td>區塊儲存空間</td>
       <td>{{site.data.keyword.Bluemix_notm}} Block Storage 外掛程式版本 1.3.0 或更新版本</td>
     </tr>
     <tr>
       <td>物件儲存空間</td>
       <td><ul><li>{{site.data.keyword.cos_full_notm}} 外掛程式版本 1.0.3 或更新版本</li><li>以 HMAC 鑑別進行 {{site.data.keyword.cos_full_notm}} 服務設定</li></ul></td>
     </tr>
   </tbody>
   </table>

   如果您必須使用的 Kubernetes 版本或 {{site.data.keyword.Bluemix_notm}} 儲存空間外掛程式版本不支援透過專用網路的網路通訊，或者，如果您想要在不執行 HMAC 鑑別的情況下使用 {{site.data.keyword.cos_full_notm}}，則容許透過防火牆對 IBM Cloud 基礎架構 (SoftLayer) 及 {{site.data.keyword.Bluemix_notm}} Identity and Access Management 進行 Egress 存取：
   - 容許 TCP 埠 443 上的所有 Egress 網路資料流量。
   - 對於[**前端（公用）網路**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#frontend-public-network)及[**後端（專用）網路**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network)，容許存取您叢集所在之區域的 IBM Cloud 基礎架構 (SoftLayer) IP 範圍。若要尋找叢集的區域，請執行 `ibmcloud ks clusters`。


<br />


## 容許叢集透過專用防火牆存取資源
{: #firewall_private}

如果您的專用網路有防火牆，請容許工作者節點之間進行通訊，並讓您的叢集透過專用網路來存取基礎架構資源。
{:shortdesc}

1. 容許工作者節點之間的所有資料流量。
    1. 在公用及專用介面上，容許工作者節點之間的所有 TCP、UDP、VRRP 及 IPEncap 資料流量。{{site.data.keyword.containerlong_notm}} 使用 VRRP 通訊協定來管理專用負載平衡器的 IP 位址，以及使用 IPEncap 通訊協定來允許跨子網路的 Pod 至 Pod 資料流量。
    2. 如果您使用 Calico 原則，或者您在多區域叢集的每一個區域中都有防火牆，則防火牆可能會封鎖工作者節點之間的通訊。您必須使用工作者節點的埠、工作者節點的專用 IP 位址，或 Calico 工作者節點標籤，讓叢集裡的所有工作者節點都對彼此開啟。

2. 容許 IBM Cloud 基礎架構 (SoftLayer) 專用 IP 範圍，以便您可以在叢集裡建立工作者節點。
    1. 容許適當的 IBM Cloud 基礎架構 (SoftLayer) 專用 IP 範圍。請參閱[後端（專用）網路](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network)。
    2. 容許您所使用的所有[區域](/docs/containers?topic=containers-regions-and-zones#zones)的 IBM Cloud 基礎架構 (SoftLayer) 專用 IP 範圍。請注意，必須針對 `dal01`、`dal10` 和 `wdc04` 區域新增 IP，如果叢集位於歐洲地理位置，則還必須針對 `ams01` 區域新增 IP。請參閱[服務網路（在後端/專用網路上）](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#service-network-on-backend-private-network-)。

3. 開啟下列埠：
    - 容許出埠 TCP 及 UDP 連線從工作者節點到達埠 80 和 443，以容許工作者節點更新及重新載入。
    - 容許出埠 TCP 及 UDP 到達埠 2049，以容許將檔案儲存空間裝載為磁區。
    - 容許出埠 TCP 及 UDP 到達埠 3260，以進行與區塊儲存空間的通訊。
    - 容許入埠 TCP 及 UDP 連線到達埠 10250，以進行 Kubernetes 儀表板及指令（例如 `kubectl logs` 及 `kubectl exec`）。
    - 容許入埠及出埠連線到達 TCP 和 UDP 埠 53，以進行 DNS 存取。

4. 如果您在公用網路上也有防火牆，或者如果您有僅專用 VLAN 的叢集並且使用閘道裝置作為防火牆，您還必須容許在[容許叢集存取基礎架構資源和其他服務](#firewall_outbound)中指定的 IP 和埠。

<br />


## 容許叢集透過 Calico 輸出原則存取資源
{: #firewall_calico_egress}

如果您使用 [Calico 網路原則](/docs/containers?topic=containers-network_policies)作為防火牆來限制所有公用工作者節點輸出，則必須容許工作者節點存取主要 API 伺服器及 etcd 的本端 Proxy。
{: shortdesc}

1. [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)請包括 `--admin` 及 `--network` 選項與 `ibmcloud ks cluster-config` 指令。`--admin` 會下載金鑰，以存取基礎架構組合以及在工作者節點上執行 Calico 指令。`--network` 會下載 Calico 配置檔，以執行所有 Calico 指令。
  ```
  ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin --network
  ```
  {: pre}

2. 建立 Calico 網路原則，以容許從叢集到 172.20.0.1:2040 及 172.21.0.1:443 的公用資料流量（對於 API 伺服器本端 Proxy），以及從叢集到 172.20.0.1:2041 的公用資料流量（對於 etcd 本端 Proxy）。
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

3. 將原則套用至叢集。
    - Linux 及 OS X：

      ```
      calicoctl apply -f allow-master-local.yaml
      ```
      {: pre}

    - Windows：

      ```
      calicoctl apply -f filepath/allow-master-local.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

## 從叢集外部存取 NodePort、負載平衡器及 Ingress 服務
{: #firewall_inbound}

您可以容許對 NodePort、負載平衡器及 Ingress 服務進行送入存取。
{:shortdesc}

<dl>
  <dt>NodePort 服務</dt>
  <dd>開啟您在將服務部署至所有工作者節點的公用 IP 位址時所配置的埠，以容許接收資料流量。若要尋找埠，請執行 `kubectl get svc`。該埠在 20000-32000 的範圍內。<dd>
  <dt>負載平衡器服務</dt>
  <dd>開啟您在將服務部署至負載平衡器服務的公用 IP 位址時所配置的埠。</dd>
  <dt>Ingress</dt>
  <dd>針對 Ingress 應用程式負載平衡器的 IP 位址，開啟埠 80（適用於 HTTP）或埠 443（適用於 HTTPS）。</dd>
</dl>

<br />


## 在其他服務的防火牆內或在內部部署防火牆內將您的叢集列入白名單
{: #whitelist_workers}

如果您要存取在 {{site.data.keyword.Bluemix_notm}} 內部或外部或在內部部署上執行且受防火牆保護的服務，您可以將工作者節點的 IP 位址新增至該防火牆，以容許針對您叢集的出埠網路資料流量。例如，您可能想要從受防火牆保護的 {{site.data.keyword.Bluemix_notm}} 資料庫讀取資料，或在內部部署的防火牆內將您的工作者節點子網路列入白名單，以接受來自您叢集的網路資料流量。
{:shortdesc}

1.  [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

2. 取得工作者節點子網路或工作者節點 IP 位址。
  * **工作者節點子網路**：如果您預期經常變更叢集裡的工作者節點數目，例如，如果您啟用[叢集 autoscaler](/docs/containers?topic=containers-ca#ca)，則您可能不想針對每一個新的工作者節點都更新防火牆。反之，您可以將該叢集使用的 VLAN 子網路列入白名單。請記住，VLAN 子網路可能由其他叢集裡的工作者節點共用。
    <p class="note">{{site.data.keyword.containerlong_notm}} 為您的叢集佈建的**主要公用子網路**有提供 14 個可用的 IP 位址，並可由相同 VLAN 上的其他叢集共用。當您有超過 14 個工作者節點時，會訂購另一個子網路，因此您需要列入白名單的子網路可以變更。為了減少變更頻率，請建立具有較高 CPU 及記憶體資源之工作者節點特性的工作者節點儲存區，這樣您就不需要經常新增工作者節點。</p>
    1. 列出叢集裡的工作者節點。
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

    2. 從上一步的輸出中，記下叢集裡工作者節點的 **Public IP** 的所有唯一網路 ID（前 3 個八位元組）。如果要將僅專用叢集列入白名單，請改為記下 **Private IP**。在下列輸出中，唯一網路 ID 為 `169.xx.178` 和 `169.xx.210`。
        ```
ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w31   169.xx.178.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.13.6   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w34   169.xx.178.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.13.6  
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w32   169.xx.210.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.13.6   
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w33   169.xx.210.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.13.6  
        ```
        {: screen}
    3.  列出每個唯一網路 ID 的 VLAN 子網路。
        ```
        ibmcloud sl subnet list | grep -e <networkID1> -e <networkID2>
        ```
        {: pre}

    輸出範例：
    ```
        ID        identifier       type                 network_space   datacenter   vlan_id   IPs   hardware   virtual_servers
        1234567   169.xx.210.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal12        1122334   16    0          5   
        7654321   169.xx.178.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal10        4332211   16    0          6    
        ```
        {: screen}
    4.  擷取子網路位址。在輸出中，尋找 **IP** 數目。然後，使 `2` 的 `n` 次方等於 IP 的數目。比方說，如果 IP 的數目是 `16`，則 `2` 的 `4` (`n`) 次方就等於 `16`。現在，從 `32` 位元中減去 `n` 的值，就得出子網路 CIDR。例如，當 `n` 等於 `4` 時，CIDR 是 `28`（來自方程式 `32 - 4 = 28`）。將 **ID** 遮罩與 CIDR 值結合，即可得出完整子網路位址。在前一個輸出中，子網路位址如下：
        *   `169.xx.210.xxx/28`
        *   `169.xx.178.xxx/28`
  * **個別工作者節點 IP 位址**：如果您有少數工作者節點只執行一個應用程式且不需要調整，或如果您只想將一個工作者節點列入白名單，請列出叢集裡的所有工作者節點，並記下**公用 IP** 位址。如果您的工作者節點僅連接至專用網路，而您想要使用專用服務端點來連接至 {{site.data.keyword.Bluemix_notm}} 服務，請改為記下**專用 IP** 位址。請注意，只有這些工作者節點才會列入白名單。如果您刪除工作者節點或將工作者節點新增至叢集裡，您必須相應地更新防火牆。
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}
4.  針對出埠資料流量將子網路 CIDR 或 IP 位址新增至服務的防火牆，或針對入埠資料流量而新增至內部部署的防火牆。
5.  針對要列入白名單的每一個叢集重複這些步驟。
