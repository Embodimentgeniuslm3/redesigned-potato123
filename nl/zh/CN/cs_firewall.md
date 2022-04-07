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



# 在防火墙中打开必需的端口和 IP 地址
{: #firewall}

查看以下情况，在这些情况下，您可能需要在防火墙中为 {{site.data.keyword.containerlong}} 打开特定端口和 IP 地址。
{:shortdesc}

* 在企业网络策略阻止通过代理或防火墙访问公用因特网端点时从本地系统[运行 `ibmcloud` 和 `ibmcloud ks` 命令](#firewall_bx)。
* 在企业网络策略阻止通过代理或防火墙访问公用因特网端点时从本地系统[运行 `kubectl` 命令](#firewall_kubectl)。
* 在企业网络策略阻止通过代理或防火墙访问公用因特网端点时从本地系统[运行 `calicoctl` 命令](#firewall_calicoctl)。
* 为工作程序节点设置防火墙或在 IBM Cloud infrastructure (SoftLayer) 帐户中定制防火墙设置时，[允许 Kubernetes 主节点与工作程序节点之间进行通信](#firewall_outbound)。
* [允许集群通过专用网络上的防火墙访问资源](#firewall_private)。
* [允许集群在 Calico 网络策略阻止工作程序节点流出流量时访问资源](#firewall_calico_egress)。
* [从集群外部访问 NodePort 服务、LoadBalancer 服务或 Ingress](#firewall_inbound)。
* [允许集群访问在 {{site.data.keyword.Bluemix_notm}} 内部或外部或者内部部署中运行且受防火墙保护的服务](#whitelist_workers)。

<br />


## 从防火墙后运行 `ibmcloud` 和 `ibmcloud ks` 命令
{: #firewall_bx}

如果企业网络策略阻止通过代理或防火墙从本地系统访问公共端点，那么要运行 `ibmcloud` 和 `ibmcloud ks` 命令，必须允许对 {{site.data.keyword.Bluemix_notm}} 和 {{site.data.keyword.containerlong_notm}} 的 TCP 访问。
{:shortdesc}

1. 允许在防火墙中的端口 443 上访问 `cloud.ibm.com`。
2. 通过此 API 端点登录到 {{site.data.keyword.Bluemix_notm}} 来验证连接。
  ```
  ibmcloud login -a https://cloud.ibm.com/
  ```
  {: pre}
3. 允许在防火墙中的端口 443 上访问 `containers.cloud.ibm.com`。
4. 验证连接。如果正确配置了访问权，那么会在输出中显示船。
   ```
   curl https://containers.cloud.ibm.com/v1/
   ```
   {: pre}

   输出示例：
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


## 从防火墙后运行 `kubectl` 命令
{: #firewall_kubectl}

如果企业网络策略阻止通过代理或防火墙从本地系统访问公共端点，那么要运行 `kubectl` 命令，必须允许集群的 TCP 访问。
{:shortdesc}

创建集群时，服务端点 URL 中的端口是从 20000-32767 中随机分配的。您可以选择为可能创建的任何集群打开端口范围 20000-32767，也可以选择允许对特定现有集群进行访问。

开始之前，允许访问以[运行 `ibmcloud ks` 命令](#firewall_bx)。

要允许访问特定集群：

1. 登录到 {{site.data.keyword.Bluemix_notm}} CLI。根据提示，输入您的 {{site.data.keyword.Bluemix_notm}} 凭证。如果您有联合帐户，请包括 `--sso` 选项。

   ```
    ibmcloud login [--sso]
    ```
   {: pre}

2. 如果集群位于非 `default` 资源组中，请将该资源组设定为目标。要查看每个集群所属的资源组，请运行 `ibmcloud ks clusters`。**注**：对于该资源组，您必须至少具有[**查看者**角色](/docs/containers?topic=containers-users#platform)。
   ```
   ibmcloud target -g <resource_group_name>
   ```
   {: pre}

4. 获取集群的名称。

   ```
   ibmcloud ks clusters
   ```
   {: pre}

5. 检索集群的服务端点 URL。
 * 如果仅填充了**公共服务端点 URL**，请获取此 URL。授权集群用户可以通过公用网络上的此端点来访问 Kubernetes 主节点。
 * 如果仅填充了**专用服务端点 URL**，请获取此 URL。授权集群用户可以通过专用网络上的此端点来访问 Kubernetes 主节点。
 * 如果同时填充了**公共服务端点 URL** 和**专用服务端点 URL**，请获取这两个 URL。授权集群用户可以通过公用网络上的公共端点或专用网络上的专用端点来访问 Kubernetes 主节点。

  ```
   ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
   ```
  {: pre}

  输出示例：
  ```
  ...
  Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
  Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
  ...
  ```
  {: screen}

6. 允许访问在上一步中获得的服务端点 URL 和端口。如果防火墙是基于 IP 的，那么可以通过查看[此表](#master_ips)来了解允许访问服务端点 URL 时打开的 IP 地址。

7. 验证连接。
  * 如果启用了公共服务端点：
    ```
    curl --insecure <public_service_endpoint_URL>/version
    ```
    {: pre}

    示例命令：
    ```
       curl --insecure https://c3.<region>.containers.cloud.ibm.com:31142/version
   ```
    {: pre}

    输出示例：
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
  * 如果启用了专用服务端点，您必须位于 {{site.data.keyword.Bluemix_notm}} 专用网络中或通过 VPN 连接与专用网络连接，以验证与主节点的连接。请注意，您必须[通过专用负载均衡器公开主节点端点](/docs/containers?topic=containers-clusters#access_on_prem)，以便用户可以通过 VPN 或 {{site.data.keyword.BluDirectLink}} 连接访问主节点。
    ```
    curl --insecure <private_service_endpoint_URL>/version
    ```
    {: pre}

    示例命令：
    ```
    curl --insecure https://c3-private.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    输出示例：
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

8. 可选：对您需要显示的每个集群重复上述步骤。

<br />


## 从防火墙后运行 `calicoctl` 命令
{: #firewall_calicoctl}

如果企业网络策略阻止通过代理或防火墙从本地系统访问公共端点，那么要运行 `calicoctl` 命令，必须允许 Calico 命令的 TCP 访问。
{:shortdesc}

开始之前，允许访问以运行 [`ibmcloud` 命令](#firewall_bx)和 [`kubectl` 命令](#firewall_kubectl)。

1. 从用于允许 [`kubectl` 命令](#firewall_kubectl)的主 URL 中检索 IP 地址。

2. 获取 etcd 的端口。

  ```
  kubectl get cm -n kube-system cluster-info -o yaml | grep etcd_host
  ```
  {: pre}

3. 允许通过主 URL IP 地址和 etcd 端口访问 Calico 策略。

<br />


## 允许集群通过公共防火墙访问基础架构资源和其他服务
{: #firewall_outbound}

支持集群从公共防火墙后访问基础架构资源和服务，例如针对 {{site.data.keyword.containerlong_notm}} 区域、{{site.data.keyword.registrylong_notm}}、{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)、{{site.data.keyword.monitoringlong_notm}}、{{site.data.keyword.loganalysislong_notm}}、IBM Cloud Infrastructure (SoftLayer) 专用 IP 以及用于持久卷声明的 Egress。
{:shortdesc}

根据集群设置，可以使用公共和/或专用 IP 地址来访问服务。如果集群在位于公用和专用网络的防火墙后的公用和专用 VLAN 上都有工作程序节点，那么必须同时打开公共和专用 IP 地址的连接。如果集群仅在防火墙后的专用 VLAN 上有工作程序节点，那么可以仅打开与专用 IP 地址的连接。
{: note}

1.  记下用于集群中所有工作程序节点的公共 IP 地址。

    ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
    {: pre}

2.  允许从源 <em>&lt;each_worker_node_publicIP&gt;</em> 到目标 TCP/UDP 端口范围 20000-32767 和端口 443 以及以下 IP 地址和网络组的出站网络流量。这些 IP 地址允许工作程序节点与集群主节点进行通信。如果您拥有的企业防火墙阻止本地计算机访问公用因特网端点，请同时对本地计算机执行此步骤，以便可以访问集群主节点。

    针对区域内的所有专区，必须允许出局流量流至端口 443，以便在引导过程中均衡负载。例如，如果集群位于美国南部，那么必须允许流量从每个工作程序节点的公共 IP 流至所有专区的 IP 地址的端口 443。
    {: important}

    {: #master_ips}
    <table summary="表中第一行跨两列。其他行应从左到右阅读，其中第一列是服务器专区，第二列是要匹配的 IP 地址。">
  <caption>要为出局流量打开的 IP 地址</caption>
          <thead>
          <th>区域</th>
          <th>专区</th>
          <th>公共 IP 地址</th>
          <th>专用 IP 地址</th>
          </thead>
        <tbody>
          <tr>
            <td>亚太地区北部</td>
            <td>che01<br>hkg02<br>seo01<br>sng01<br><br>tok02、tok04、tok05</td>
            <td><code>169.38.70.10</code><br><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><br><code>161.202.126.210、128.168.71.117、165.192.69.69</code></td>
            <td><code>166.9.40.7</code><br><code>166.9.42.7</code><br><code>166.9.44.5</code><br><code>166.9.40.8</code><br><br><code>166.9.40.6、166.9.42.6、166.9.44.4</code></td>
           </tr>
          <tr>
             <td>亚太地区南部</td>
             <td>mel01<br><br>syd01、syd04、syd05</td>
             <td><code>168.1.97.67</code><br><br><code>168.1.8.195、130.198.66.26、168.1.12.98、130.198.64.19</code></td>
             <td><code>166.9.54.10</code><br><br><code>166.9.52.14、166.9.52.15、166.9.54.11、166.9.54.13、166.9.54.12</code></td>
          </tr>
          <tr>
             <td>欧洲中部</td>
             <td>ams03<br>mil01<br>osl01<br>par01<br><br>fra02、fra04、fra05</td>
             <td><code>169.50.169.110, 169.50.154.194</code><br><code>159.122.190.98, 159.122.141.69</code><br><code>169.51.73.50</code><br><code>159.8.86.149, 159.8.98.170</code><br><br><code>169.50.56.174、161.156.65.42、149.81.78.114</code></td>
             <td><code>166.9.28.17、166.9.30.11</code><br><code>166.9.28.20、166.9.30.12</code><br><code>166.9.32.8</code><br><code>166.9.28.19、166.9.28.22</code><br><br><code>	166.9.28.23、166.9.30.13、166.9.32.9</code></td>
            </tr>
          <tr>
            <td>英国南部</td>
            <td>lon02、lon04、lon05、lon06</td>
            <td><code>159.122.242.78、158.175.111.42、158.176.94.26、159.122.224.242、158.175.65.170、158.176.95.146</code></td>
            <td><code>166.9.34.5、166.9.34.6、166.9.36.10、166.9.36.11、166.9.36.12、166.9.36.13、166.9.38.6、166.9.38.7</code></td>
          </tr>
          <tr>
            <td>美国东部</td>
             <td>mon01<br>tor01<br><br>wdc04、wdc06、wdc07</td>
             <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><br><code>169.63.88.186、169.60.73.142、169.61.109.34、169.63.88.178、169.60.101.42、169.61.83.62</code></td>
             <td><code>166.9.20.11</code><br><code>166.9.22.8</code><br><br><code>166.9.20.12、166.9.20.13、166.9.22.9、166.9.22.10、166.9.24.4、166.9.24.5</code></td>
          </tr>
          <tr>
            <td>美国南部</td>
            <td>hou02<br>mex01<br>sao01<br>sjc03<br>sjc04<br><br>dal10、dal12、dal13</td>
            <td><code>184.173.44.62</code><br><code>169.57.100.18</code><br><code>169.57.151.10</code><br><code>169.45.67.210</code><br><code>169.62.82.197</code><br><br><code>169.46.7.238、169.48.230.146、169.61.29.194、169.46.110.218、169.47.70.10、169.62.166.98、169.48.143.218、169.61.177.2、169.60.128.2</code></td>
            <td><code>166.9.15.74</code><br><code>166.9.15.76</code><br><code>166.9.12.143</code><br><code>166.9.12.144</code><br><code>166.9.15.75</code><br><br><code>166.9.12.140、166.9.12.141、166.9.12.142、166.9.15.69、166.9.15.70、166.9.15.72、166.9.15.71、166.9.15.73、166.9.16.183、166.9.16.184、166.9.16.185</code></td>
          </tr>
          </tbody>
        </table>

3.  {: #firewall_registry}要允许工作程序节点与 {{site.data.keyword.registrylong_notm}} 进行通信，请允许出站网络流量从工作程序节点流至 [{{site.data.keyword.registrylong_notm}} 区域](/docs/services/Registry?topic=registry-registry_overview#registry_regions)：
  - `TCP port 443, port 4443 FROM <each_worker_node_publicIP> TO <registry_subnet>`
  -  将 <em>&lt;registry_subnet&gt;</em> 替换为要允许流量流至的注册表子网。全局注册表存储 IBM 提供的公共映像，区域注册表存储您自己的专用或公共映像。对于公证功能，端口 4443 是必需的，例如[验证映像签名](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)。<table summary="表中第一行跨两列。其他行应从左到右阅读，其中第一列是服务器专区，第二列是要匹配的 IP 地址。">
  <caption>要为注册表流量打开的 IP 地址</caption>
    <thead>
      <th>{{site.data.keyword.containerlong_notm}} 区域</th>
      <th>注册表地址</th>
      <th>注册表公用子网</th>
      <th>注册表专用 IP 地址</th>
    </thead>
    <tbody>
      <tr>
        <td>跨 <br>{{site.data.keyword.containerlong_notm}} 区域的全局注册表</td>
        <td><code>icr.io</code><br><br>
        不推荐：<code>registry.bluemix.net</code></td>
        <td><code>169.60.72.144/28</code></br><code>169.61.76.176/28</code></br><code>169.62.37.240/29</code></br><code>169.60.98.80/29</code></br><code>169.63.104.232/29</code></td>
        <td><code>166.9.20.4</code></br><code>166.9.22.3</code></br><code>166.9.24.2</code></td>
      </tr>
      <tr>
        <td>亚太地区北部</td>
        <td><code>jp.icr.io</code><br><br>
        不推荐：<code>registry.au-syd.bluemix.net</code></td>
        <td><code>161.202.146.86/29</code></br><code>128.168.71.70/29</code></br><code>165.192.71.222/29</code></td>
        <td><code>166.9.40.3</code></br><code>166.9.42.3</code></br><code>166.9.44.3</code></td>
      </tr>
      <tr>
        <td>亚太地区南部</td>
        <td><code>au.icr.io</code><br><br>
        不推荐：<code>registry.au-syd.bluemix.net</code></td>
        <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></br><code>168.1.1.240/29</code></br><code>130.198.88.128/29</code></br><code>135.90.66.48/29</code></td>
        <td><code>166.9.52.2</code></br><code>166.9.54.2</code></br><code>166.9.56.3</code></td>
      </tr>
      <tr>
        <td>欧洲中部</td>
        <td><code>de.icr.io</code><br><br>
        不推荐：<code>registry.eu-de.bluemix.net</code></td>
        <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></br><code>169.50.58.104/29</code></br><code>161.156.93.16/29</code></br><code>149.81.79.152/29</code></td>
        <td><code>166.9.28.12</code></br><code>166.9.30.9</code></br><code>166.9.32.5</code></td>
       </tr>
       <tr>
        <td>英国南部</td>
        <td><code>uk.icr.io</code><br><br>
        不推荐：<code>registry.eu-gb.bluemix.net</code></td>
        <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></br><code>158.175.97.184/29</code></br><code>158.176.105.64/29</code></br><code>141.125.71.136/29</code></td>
        <td><code>166.9.36.9</code></br><code>166.9.38.5</code></br><code>166.9.34.4</code></td>
       </tr>
       <tr>
        <td>美国东部和美国南部</td>
        <td><code>us.icr.io</code><br><br>
        不推荐：<code>registry.ng.bluemix.net</code></td>
        <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></br><code>169.61.234.224/29</code></br><code>169.61.135.160/29</code></br><code>169.61.46.80/29</code></td>
        <td><code>166.9.12.114</code></br><code>166.9.15.50</code></br><code>166.9.16.173</code></td>
       </tr>
      </tbody>
    </table>

4. 可选：允许出局网络流量从工作程序节点流至 {{site.data.keyword.monitoringlong_notm}}、{{site.data.keyword.loganalysislong_notm}}、Sysdig 和 LogDNA 服务：
    *   **{{site.data.keyword.monitoringlong_notm}}**:
        <pre class="screen">TCP port 443, port 9095 FROM &lt;each_worker_node_public_IP&gt; TO &lt;monitoring_subnet&gt;</pre>
        将 <em>&lt;monitoring_subnet&gt;</em> 替换为要允许流量流至的监视区域的子网：
        <p><table summary="表中第一行跨两列。其他行应从左到右阅读，其中第一列是服务器专区，第二列是要匹配的 IP 地址。">
  <caption>要为监视流量打开的 IP 地址</caption>
        <thead>
        <th>{{site.data.keyword.containerlong_notm}} 区域</th>
        <th>监视地址</th>
        <th>监视子网</th>
        </thead>
      <tbody>
        <tr>
         <td>欧洲中部</td>
         <td><code>metrics.eu-de.bluemix.net</code></td>
         <td><code>158.177.65.80/30</code></td>
        </tr>
        <tr>
         <td>英国南部</td>
         <td><code>metrics.eu-gb.bluemix.net</code></td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>美国东部、美国南部、亚太地区北部和亚太地区南部</td>
          <td><code>metrics.ng.bluemix.net</code></td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
    *   **{{site.data.keyword.mon_full_notm}}**:
        <pre class="screen">TCP port 443, port 6443 FROM &lt;each_worker_node_public_IP&gt; TO &lt;sysdig_public_IP&gt;</pre>
        将 `<sysdig_public_IP>` 替换为 [Sysdig IP 地址](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-network#network)。
    *   **{{site.data.keyword.loganalysislong_notm}}**:
        <pre class="screen">TCP port 443, port 9091 FROM &lt;each_worker_node_public_IP&gt; TO &lt;logging_public_IP&gt;</pre>
        将 <em>&lt;logging_public_IP&gt;</em> 替换为要允许流量流至的日志记录区域的所有地址：
        <p><table summary="表中第一行跨两列。其他行应从左到右阅读，其中第一列是服务器专区，第二列是要匹配的 IP 地址。">
<caption>要对日志记录流量打开的 IP 地址</caption>
        <thead>
        <th>{{site.data.keyword.containerlong_notm}} 区域</th>
        <th>日志记录地址</th>
        <th>日志记录 IP 地址</th>
        </thead>
        <tbody>
          <tr>
            <td>美国东部和美国南部</td>
            <td><code>ingest.logging.ng.bluemix.net</code></td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
          </tr>
          <tr>
           <td>英国南部</td>
           <td><code>ingest.logging.eu-gb.bluemix.net</code></td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>欧洲中部</td>
           <td><code>ingest-eu-fra.logging.bluemix.net</code></td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>亚太南部、亚太北部</td>
           <td><code>ingest-au-syd.logging.bluemix.net</code></td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>
    *   **{{site.data.keyword.la_full_notm}}**:
        <pre class="screen">TCP port 443, port 80 FROM &lt;each_worker_node_public_IP&gt; TO &lt;logDNA_public_IP&gt;</pre>
        将 `<logDNA_public_IP>` 替换为 [LogDNA IP 地址](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-network#network)。

5. 如果使用负载均衡器服务，请确保在工作程序节点的公共和专用接口上允许所有使用 VRRP 协议的所有流量。{{site.data.keyword.containerlong_notm}} 使用 VRRP 协议来管理公共和专用负载均衡器的 IP 地址。

6. {: #pvc}要在专用集群中创建持久卷声明，请确保集群设置为使用以下 Kubernetes 版本或 {{site.data.keyword.Bluemix_notm}} 存储插件版本。这些版本支持从集群到持久性存储器实例的专用网络通信。
    <table>
    <caption>专用集群的必需 Kubernetes 或 {{site.data.keyword.Bluemix_notm}} 存储插件版本概述</caption>
    <thead>
      <th>存储器类型</th>
      <th>必需版本</th>
   </thead>
   <tbody>
     <tr>
       <td>文件存储器</td>
       <td>Kubernetes V<code>1.13.4_1512</code>、<code>1.12.6_1544</code>、<code>1.11.8_1550</code>、<code>1.10.13_1551</code> 或更高版本</td>
     </tr>
     <tr>
       <td>块存储器</td>
       <td>{{site.data.keyword.Bluemix_notm}} Block Storage V1.3.0 或更高版本插件</td>
     </tr>
     <tr>
       <td>对象存储器</td>
       <td><ul><li>{{site.data.keyword.cos_full_notm}} V1.0.3 或更高版本插件</li><li>{{site.data.keyword.cos_full_notm}} 服务设置为使用 HMAC 认证</li></ul></td>
     </tr>
   </tbody>
   </table>

   如果必须使用不支持通过专用网络进行网络通信的 Kubernetes 版本或 {{site.data.keyword.Bluemix_notm}} 存储插件版本，或者如果要使用不采用 HMAC 认证的 {{site.data.keyword.cos_full_notm}}，请允许通过防火墙对 IBM Cloud Infrastructure (SoftLayer) 和 {{site.data.keyword.Bluemix_notm}} Identity and Access Management 的流出访问：
   - 允许 TCP 端口 443 上的所有流出网络流量。
   - 允许访问[**前端（公用）网络**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#frontend-public-network)和[**后端（专用）网络**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network)中集群所在的专区的 IBM Cloud Infrastructure (SoftLayer) IP 范围。要查找集群的专区，请运行 `ibmcloud ks clusters`。


<br />


## 允许集群通过专用防火墙访问资源
{: #firewall_private}

如果您在专用网络上有一个防火墙，那么允许工作程序节点之间的通信，并且使集群通过专用网络访问基础架构资源。
{:shortdesc}

1. 允许工作程序节点之间的所有流量。
    1. 允许公共接口和专用接口上的工作程序节点之间的所有 TCP、UDP、VRRP 和 IPEncap 流量。{{site.data.keyword.containerlong_notm}} 使用 VRRP 协议来管理专用负载均衡器的 IP 地址，而使用 IPEncap 协议来允许子网上 pod 之间的流量。
    2. 如果使用 Calico 策略，或者如果在多专区集群的每个专区中有防火墙，那么防火墙可能会阻止工作程序节点之间的通信。必须使用工作程序端口、工作程序专用 IP 地址或 Calico 工作程序节点标签，使集群中的所有工作程序节点相互开放。

2. 允许 IBM Cloud Infrastructure (SoftLayer) 专用 IP 范围，从而可在集群中创建工作程序节点。
    1. 允许相应的 IBM Cloud Infrastructure (SoftLayer) 专用 IP 范围。请参阅[后端（专用）网络](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network)。
    2. 针对使用的所有[专区](/docs/containers?topic=containers-regions-and-zones#zones)，允许 IBM Cloud Infrastructure (SoftLayer) 专用 IP 范围。请注意，必须针对 `dal01`、`dal10` 和 `wdc04` 专区添加 IP，如果集群位于欧洲地理位置，那么还必须针对 `ams01` 专区添加 IP。请参阅[服务网络（在后端/专用网络上）](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#service-network-on-backend-private-network-)。

3. 打开以下端口：
    - 允许从工作程序到端口 80 和 443 的出站 TCP 和 UDP 连接，从而允许工作程序节点更新和重新装入。
    - 允许到端口 2049 的出站 TCP 和 UDP，从而允许作为卷安装文件存储器。
    - 允许到端口 3260 的出站 TCP 和 UDP，以便与块存储器进行通信。
    - 针对 Kubernetes 仪表板和命令，允许到端口 10250 的入站 TCP 和 UDP 连接，例如，`kubectl logs` 和 `kubectlexec`。
    - 允许到 TCP 和 UDP 端口 53 的入站和出站连接以用于 DNS 访问。

4. 如果您在公用网络上也有防火墙，或者如果您有仅专用 VLAN 的集群并且使用网关设备作为防火墙，那么还必须允许在[允许集群访问基础架构资源和其他服务](#firewall_outbound)中指定的 IP 和端口。

<br />


## 允许集群通过 Calico 流出策略访问资源
{: #firewall_calico_egress}

如果使用 [Calico 网络策略](/docs/containers?topic=containers-network_policies)充当防火墙来限制所有公共工作程序的流出流量，那么必须允许工作程序访问主 API 服务器和 etcd 的本地代理。
{: shortdesc}

1. [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)在 `ibmcloud ks cluster-config` 命令中包含 `--admin` 和 `--network` 选项。`--admin` 用于下载密钥，以用于访问基础架构产品服务组合以及在工作程序节点上运行 Calico 命令。`--network` 用于下载 Calico 配置文件以运行所有 Calico 命令。
  ```
  ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin --network
  ```
  {: pre}

2. 创建 Calico 网络策略，以允许来自集群的公共流量流至 172.20.0.1:2040 和 172.21.0.1:443（对于 API 服务器本地代理）以及流至 172.20.0.1:2041（对于 etcd 本地代理）。
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

3. 将策略应用于集群。
    - Linux 和 OS X：

      ```
      calicoctl apply -f allow-master-local.yaml
      ```
      {: pre}

    - Windows：

      ```
      calicoctl apply -f filepath/allow-master-local.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

## 从集群外部访问 NodePort、LoadBalancer 和 Ingress 服务
{: #firewall_inbound}

您可以允许入站访问 NodePort、LoadBalancer 和 Ingress 服务。
{:shortdesc}

<dl>
  <dt>NodePort 服务</dt>
  <dd>打开将服务部署到允许流量流到的所有工作程序节点的公共 IP 地址时所配置的端口。要查找该端口，请运行 `kubectl get svc`。端口在 20000-32000 范围内。<dd>
  <dt>LoadBalancer 服务</dt>
  <dd>打开将服务部署到 LoadBalancer 服务的公共 IP 地址时所配置的端口。</dd>
  <dt>Ingress</dt>
  <dd>针对 Ingress 应用程序负载均衡器的 IP 地址打开端口 80（对于 HTTP）或端口 443（对于 HTTPS）。</dd>
</dl>

<br />


## 在其他服务的防火墙或在内部部署防火墙中将集群列入白名单
{: #whitelist_workers}

如果要访问在 {{site.data.keyword.Bluemix_notm}} 内部或外部或者内部部署中运行且受防火墙保护的服务，可以在该防火墙中添加工作程序节点的 IP 地址，以允许出站网络流量流至集群。例如，您可能希望从受防火墙保护的 {{site.data.keyword.Bluemix_notm}} 数据库中读取数据，或者希望在内部部署防火墙中将工作程序节点子网列入白名单，以允许来自集群的网络流量。
{:shortdesc}

1.  [登录到您的帐户。如果适用，请将相应的资源组设定为目标。为集群设置上下文。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

2. 获取工作程序节点子网或工作程序节点 IP 地址。
  * **工作程序节点子网**：如果您预期会频繁更改集群中的工作程序节点数，例如如果启用了[集群自动缩放器](/docs/containers?topic=containers-ca#ca)，那么您可能并不希望为每个新的工作程序节点更新防火墙。您可以改为将集群使用的 VLAN 子网列入白名单。请记住，VLAN 子网可能由其他集群中的工作程序节点共享。
    <p class="note">{{site.data.keyword.containerlong_notm}} 为集群供应的**主公用子网**随附 14 个可用 IP 地址，可以由同一 VLAN 上的其他集群共享。当您拥有的工作程序节点超过 14 个时，将订购另一个子网，因此需要列入白名单的子网可能会更改。为了减少更改频率，请创建具有较高 CPU 和内存资源的工作程序节点类型模板的工作程序池，这样就不需要经常添加工作程序节点。</p>
    1. 列出集群中的工作程序节点。
      ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
      {: pre}

    2. 从上一步的输出中，记下集群中工作程序节点的 **Public IP** 的所有唯一网络标识（前 3 个八位元）。如果要将仅专用集群列入白名单，请改为记下 **Private IP**。在以下输出中，唯一网络标识为 `169.xx.178` 和 `169.xx.210`。
        ```
ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w31   169.xx.178.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.13.6   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w34   169.xx.178.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.13.6  
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w32   169.xx.210.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.13.6   
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w33   169.xx.210.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.13.6  
        ```
        {: screen}
    3.  列出每个唯一网络标识的 VLAN 子网。
        ```
        ibmcloud sl subnet list | grep -e <networkID1> -e <networkID2>
        ```
        {: pre}

    输出示例：
    ```
        ID        identifier       type                 network_space   datacenter   vlan_id   IPs   hardware   virtual_servers
        1234567   169.xx.210.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal12        1122334   16    0          5   
        7654321   169.xx.178.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal10        4332211   16    0          6    
        ```
        {: screen}
    4.  检索子网地址。在输出中，找到 **IPs** 的数量。然后，使 `2` 的 `n` 次幂等于 IP 数。例如，如果 IP 数为 `16`，那么 `2` 的 `4` (`n`) 次幂等于 `16`。现在，从 `32` 位中减去值 `n` 可得到子网 CIDR。例如，`n` 等于 `4` 时，CIDR 为 `28`（公式为 `32 - 4 = 28`）。将 **identifier** 掩码与 CIDR 值组合在一起，即可获得完整的子网地址。在先前的输出中，子网地址为：
        *   `169.xx.210.xxx/28`
        *   `169.xx.178.xxx/28`
  * **单个工作程序节点 IP 地址**：如果您有一小部分工作程序节点仅运行一个应用程序且不需要缩放，或者如果您只想将一个工作程序节点列入白名单，请列出集群中的所有工作程序节点，并记下 **Public IP** 地址。如果工作程序节点仅连接到专用网络，并且您希望使用专用服务端点连接到 {{site.data.keyword.Bluemix_notm}} 服务，请改为记下 **Private IP** 地址。请注意，仅将这些工作程序节点列入白名单。如果删除工作程序节点或将工作程序节点添加到集群，那么必须相应地更新防火墙。
    ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
    {: pre}
4.  将子网 CIDR 或 IP 地址添加到服务的防火墙以用于出站流量，或添加到内部部署防火墙以用于入站流量。
5.  针对要列入白名单的每个集群，重复上述步骤。
