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



# 방화벽에서 필수 포트와 IP 주소 열기
{: #firewall}

{{site.data.keyword.containerlong}}를 위해 방화벽에서 특정 포트 및 IP 주소를 열어야 할 수 있는 다음 상황을 검토하십시오.
{:shortdesc}

* 회사 네트워크 정책으로 인해 프록시 또는 방화벽을 통해 공용 인터넷 엔드포인트에 액세스하지 못할 때 로컬 시스템에서 [`ibmcloud` 및 `ibmcloud ks` 명령을 실행](#firewall_bx)하려는 경우
* 회사 네트워크 정책으로 인해 프록시 또는 방화벽을 통해 공용 인터넷 엔드포인트에 액세스하지 못할 때 로컬 시스템에서 [`kubectl` 명령을 실행](#firewall_kubectl)하려는 경우
* 회사 네트워크 정책으로 인해 프록시 또는 방화벽을 통해 공용 인터넷 엔드포인트에 액세스하지 못할 때 로컬 시스템에서 [`calicoctl` 명령을 실행](#firewall_calicoctl)하려는 경우
* 작업자 노드에 대한 방화벽이 설정되었거나 IBM Cloud 인프라(SoftLayer) 계정에서 방화벽 설정이 사용자 정의되었을 때 [Kubernetes 마스터와 작업자 노드 간의 통신을 허용](#firewall_outbound)하려는 경우
* [클러스터가 사설 네트워크의 방화벽을 통해 리소스에 액세스할 수 있도록 허용](#firewall_private)하려는 경우
* [클러스터가 Calico 네트워크 정책이 작업자 노드 유출을 차단할 때 리소스에 액세스할 수 있도록 허용](#firewall_calico_egress)하려는 경우
* [클러스터의 외부에서 NodePort 서비스, 로드 밸런서 서비스 또는 Ingress에 액세스](#firewall_inbound)하려는 경우
* [방화벽으로 보호되는 {{site.data.keyword.Bluemix_notm}} 또는 온프레미스 내부 또는 외부에서 실행하는 서비스에 액세스할 수 있도록 허용하는 경우](#whitelist_workers)

<br />


## 방화벽 뒤에서 `ibmcloud` 및 `ibmcloud ks` 명령 실행
{: #firewall_bx}

회사 네트워크 정책으로 인해 로컬 시스템에서 프록시 또는 방화벽을 통해 공용 엔드포인트에 액세스하지 못하는 경우, `ibmcloud` 및 `ibmcloud ks` 명령을 실행하려면 {{site.data.keyword.Bluemix_notm}} 및 {{site.data.keyword.containerlong_notm}}에 대한 TCP 액세스를 허용해야 합니다.
{:shortdesc}

1. 방화벽의 포트 443에서 `cloud.ibm.com`에 대한 액세스를 허용하십시오.
2. 이 API 엔드포인트를 통해 {{site.data.keyword.Bluemix_notm}}에 로그인하여 연결을 확인하십시오.
  ```
  ibmcloud login -a https://cloud.ibm.com/
  ```
  {: pre}
3. 방화벽의 포트 443에서 `containers.cloud.ibm.com`에 대한 액세스를 허용하십시오.
4. 연결을 확인하십시오. 액세스가 올바르게 구성되면 출력에 배가 표시됩니다.
   ```
   curl https://containers.cloud.ibm.com/v1/
   ```
   {: pre}

   출력 예:
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


## 방화벽 뒤에서 `kubectl` 명령 실행
{: #firewall_kubectl}

회사 네트워크 정책으로 인해 로컬 시스템에서 프록시 또는 방화벽을 통해 공용 엔드포인트에 액세스하지 못하는 경우, `kubectl` 명령을 실행하려면 클러스터에 대한 TCP 액세스를 허용해야 합니다.
{:shortdesc}

클러스터가 작성될 때 서비스 엔드포인트 URL의 포트는 20000 - 32767 내에서 무작위로 지정됩니다. 작성될 수 있는 모든 클러스터에 포트 범위 20000 - 32767을 공개할 수도 있고 기존의 특정 클러스터에만 액세스를 허용할 수도 있습니다.

시작하기 전에 [run `ibmcloud ks` 명령](#firewall_bx)에 대한 액세스를 허용하십시오.

특정 클러스터에 대한 액세스를 허용하려면 다음을 수행하십시오.

1. {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 프롬프트가 표시되면
{{site.data.keyword.Bluemix_notm}} 인증 정보를 입력하십시오. 연합 계정이 있는 경우, `--sso` 옵션을 포함하십시오.

   ```
   ibmcloud login [--sso]
   ```
   {: pre}

2. 클러스터가 `default` 외의 리소스 그룹에 속해 있는 경우에는 해당 리소스 그룹을 대상으로 지정하십시오. 각 클러스터가 속하는 리소스 그룹을 보려면 `ibmcloud ks clusters`를 실행하십시오. **참고**: 해당 리소스 그룹에 대해 [**Viewer** 역할](/docs/containers?topic=containers-users#platform) 이상의 역할을 갖고 있어야 합니다.
   ```
   ibmcloud target -g <resource_group_name>
   ```
   {: pre}

4. 클러스터의 이름을 가져오십시오.

   ```
   ibmcloud ks clusters
   ```
   {: pre}

5. 클러스터에 대한 서비스 엔드포인트 URL을 검색하십시오.
 * **공용 서비스 엔드포인트 URL**만 채워진 경우 이 URL을 가져오십시오. 권한 부여된 클러스터 사용자는 공용 네트워크에서 이 엔드포인트를 통해 Kubernetes 마스터에 액세스할 수 있습니다.
 * **개인 서비스 엔드포인트 URL**만 채워진 경우 이 URL을 가져오십시오. 권한 부여된 클러스터 사용자는 사설 네트워크에서 이 엔드포인트를 통해 Kubernetes 마스터에 액세스할 수 있습니다.
 * **공용 서비스 엔드포인트 URL** 및 **개인 서비스 엔드포인트 URL**이 모두 채워진 경우 URL 둘 다 가져오십시오. 권한 부여된 클러스터 사용자는 공용 네트워크의 공용 엔드포인트 또는 사설 네트워크의 개인 엔드포인트를 통해 Kubernetes 마스터에 액세스할 수 있습니다.

  ```
   ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  출력 예:
  ```
  ...
  Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
  Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
  ...
  ```
  {: screen}

6. 이전 단계에서 가져온 서비스 엔드포인트 URL 및 포트에 액세스를 허용하십시오. 방화벽이 IP 기반인 경우 [이 표](#master_ips)를 검토하여 서비스 엔드포인트 URL에 대한 액세스를 허용할 때 열리는 IP 주소를 확인할 수 있습니다.

7. 연결을 확인하십시오.
  * 공용 서비스 엔드포인트가 사용으로 설정된 경우:
    ```
    curl --insecure <public_service_endpoint_URL>/version
    ```
    {: pre}

    명령 예:
    ```
       curl --insecure https://c3.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    출력 예:
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
  * 개인 서비스 엔드포인트가 사용으로 설정된 경우, {{site.data.keyword.Bluemix_notm}} 사설 네트워크를 사용하거나 VPN 연결을 통해 사설 네트워크에 연결하여 마스터에 대한 연결을 확인해야 합니다. 사용자가 VPN 또는 {{site.data.keyword.BluDirectLink}} 연결을 통해 마스터에 액세스할 수 있도록 [사설 로드 밸런서를 통해 마스터 엔드포인트를 노출](/docs/containers?topic=containers-clusters#access_on_prem)해야 한다는 점을 유의하십시오.
    ```
    curl --insecure <private_service_endpoint_URL>/version
    ```
    {: pre}

    명령 예:
    ```
    curl --insecure https://c3-private.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    출력 예:
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

8. 선택사항: 노출해야 하는 각 클러스터에 대해 이러한 단계를 반복하십시오.

<br />


## 방화벽 뒤에서 `calicoctl` 명령 실행
{: #firewall_calicoctl}

회사 네트워크 정책으로 인해 로컬 시스템에서 프록시 또는 방화벽을 통해 공용 엔드포인트에 액세스하지 못하는 경우, `calicoctl` 명령을 실행하려면 Calico 명령에 대한 TCP 액세스를 허용해야 합니다.
{:shortdesc}

시작하기 전에 [`ibmcloud` 명령](#firewall_bx) 및 [`kubectl` 명령](#firewall_kubectl) 실행에 대한 액세스를 허용하십시오.

1. [`kubectl` 명령](#firewall_kubectl)을 허용하는 데 사용한 마스터 URL에서 IP 주소를 검색하십시오.

2. etcd의 포트를 가져오십시오.

  ```
  kubectl get cm -n kube-system cluster-info -o yaml | grep etcd_host
  ```
  {: pre}

3. 마스터 URL IP 주소와 etcd 포트를 통해 Calico 정책에 대한 액세스를 허용하십시오.

<br />


## 클러스터가 공용 방화벽을 통해 인프라 리소스 및 기타 서비스에 액세스하도록 허용
{: #firewall_outbound}

클러스터가 공용 방화벽 뒤에서 인프라 리소스와 서비스(예: {{site.data.keyword.containerlong_notm}} 지역, {{site.data.keyword.registrylong_notm}}, {{site.data.keyword.Bluemix_notm}} Identity and Access Management(IAM), {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, IBM Cloud 인프라(SoftLayer) 사설 IP 및 지속적 볼륨 클레임을 위한 egress에 액세스할 수 있게 하십시오.
{:shortdesc}

클러스터 설정에 따라 공용, 개인 또는 IP 주소 둘 다 사용하여 서비스에 액세스합니다. 공용 및 사설 네트워크 모두에 대해 방화벽으로 보호되는 공용 및 사설 VLAN 모두에 작업자 노드가 있는 클러스터가 있는 경우 공인과 사설 IP 주소 모두에 대한 연결을 열어야 합니다. 클러스터에 방화벽으로 보호되는 사설 VLAN에만 작업자 노드가 있는 경우에는 사설 IP 주소에만 연결을 열 수 있습니다.
{: note}

1.  클러스터의 모든 작업자 노드에 대한 공인 IP 주소를 기록해 두십시오.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

2.  소스 <em>&lt;each_worker_node_publicIP&gt;</em>에서 대상 TCP/UDP 포트 범위 20000-32767 및 포트 443으로의 발신 네트워크 트래픽과 다음 IP 주소 및 네트워크 그룹을 허용하십시오. 이러한 IP 주소는 작업자 노드가 클러스터 마스터와 통신할 수 있도록 합니다. 로컬 시스템이 공용 인터넷 엔드포인트에 액세스하지 못하도록 하는 회사 방화벽이 있는 경우에는 클러스터 마스터에 액세스할 수 있도록 로컬 시스템에 대해서도 이 단계를 수행하십시오. 

    부트스트랩 프로세스 중에 로드 밸런싱을 수행하려면 지역 내의 모든 구역에 대해 포트 443에 대한 발신 트래픽을 허용해야 합니다. 예를 들어, 클러스터가 미국 남부에 있는 경우에는 각 작업자 노드의 공용 IP에서 모든 구역의 IP 주소의 포트 443으로 트래픽을 허용해야 합니다.
    {: important}

    {: #master_ips}
    <table summary="표에서 첫 번째 행은 두 열 모두에 걸쳐 있습니다. 나머지 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며, 서버 구역은 1열에 있고 일치시킬 IP 주소는 2열에 있습니다.">
      <caption>발신 트래픽을 위해 열리는 IP 주소</caption>
          <thead>
          <th>지역</th>
          <th>구역</th>
          <th>공인 IP 주소</th>
          <th>사설 IP 주소</th>
          </thead>
        <tbody>
          <tr>
            <td>AP 북부</td>
            <td>che01<br>hkg02<br>seo01<br>sng01<br><br>tok02, tok04, tok05</td>
            <td><code>169.38.70.10</code><br><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><br><code>161.202.126.210, 128.168.71.117, 165.192.69.69</code></td>
            <td><code>166.9.40.7</code><br><code>166.9.42.7</code><br><code>166.9.44.5</code><br><code>166.9.40.8</code><br><br><code>166.9.40.6, 166.9.42.6, 166.9.44.4</code></td>
           </tr>
          <tr>
             <td>AP 남부</td>
             <td>mel01<br><br>syd01, syd04, syd05</td>
             <td><code>168.1.97.67</code><br><br><code>168.1.8.195, 130.198.66.26, 168.1.12.98, 130.198.64.19</code></td>
             <td><code>166.9.54.10</code><br><br><code>166.9.52.14, 166.9.52.15, 166.9.54.11, 166.9.54.13, 166.9.54.12</code></td>
          </tr>
          <tr>
             <td>중앙 유럽</td>
             <td>ams03<br>mil01<br>osl01<br>par01<br><br>fra02, fra04, fra05</td>
             <td><code>169.50.169.110, 169.50.154.194</code><br><code>159.122.190.98, 159.122.141.69</code><br><code>169.51.73.50</code><br><code>159.8.86.149, 159.8.98.170</code><br><br><code>169.50.56.174, 161.156.65.42, 149.81.78.114</code></td>
             <td><code>166.9.28.17, 166.9.30.11</code><br><code>166.9.28.20, 166.9.30.12</code><br><code>166.9.32.8</code><br><code>166.9.28.19, 166.9.28.22</code><br><br><code>	166.9.28.23, 166.9.30.13, 166.9.32.9</code></td>
            </tr>
          <tr>
            <td>영국 남부</td>
            <td>lon02, lon04, lon05, lon06</td>
            <td><code>159.122.242.78, 158.175.111.42, 158.176.94.26, 159.122.224.242, 158.175.65.170, 158.176.95.146</code></td>
            <td><code>166.9.34.5, 166.9.34.6, 166.9.36.10, 166.9.36.11, 166.9.36.12, 166.9.36.13, 166.9.38.6, 166.9.38.7</code></td>
          </tr>
          <tr>
            <td>미국 동부</td>
             <td>mon01<br>tor01<br><br>wdc04, wdc06, wdc07</td>
             <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><br><code>169.63.88.186, 169.60.73.142, 169.61.109.34, 169.63.88.178, 169.60.101.42, 169.61.83.62</code></td>
             <td><code>166.9.20.11</code><br><code>166.9.22.8</code><br><br><code>166.9.20.12, 166.9.20.13, 166.9.22.9, 166.9.22.10, 166.9.24.4, 166.9.24.5</code></td>
          </tr>
          <tr>
            <td>미국 남부</td>
            <td>hou02<br>mex01<br>sao01<br>sjc03<br>sjc04<br><br>dal10,dal12,dal13</td>
            <td><code>184.173.44.62</code><br><code>169.57.100.18</code><br><code>169.57.151.10</code><br><code>169.45.67.210</code><br><code>169.62.82.197</code><br><br><code>169.46.7.238, 169.48.230.146, 169.61.29.194, 169.46.110.218, 169.47.70.10, 169.62.166.98, 169.48.143.218, 169.61.177.2, 169.60.128.2</code></td>
            <td><code>166.9.15.74</code><br><code>166.9.15.76</code><br><code>166.9.12.143</code><br><code>166.9.12.144</code><br><code>166.9.15.75</code><br><br><code>166.9.12.140, 166.9.12.141, 166.9.12.142, 166.9.15.69, 166.9.15.70, 166.9.15.72, 166.9.15.71, 166.9.15.73, 166.9.16.183, 166.9.16.184, 166.9.16.185</code></td>
          </tr>
          </tbody>
        </table>

3.  {: #firewall_registry}작업자 노드와 {{site.data.keyword.registrylong_notm}}가 통신할 수 있도록 하려면 작업자 노드에서 [{{site.data.keyword.registrylong_notm}} 지역](/docs/services/Registry?topic=registry-registry_overview#registry_regions)으로의 발신 네트워크 트래픽을 허용하십시오. 
  - `TCP port 443, port 4443 FROM <each_worker_node_publicIP> TO <registry_subnet>`
  -  <em>&lt;registry_subnet&gt;</em>을 트래픽을 허용하려는 레지스트리 서브넷으로 대체하십시오. 글로벌 레지스트리는 IBM 제공 공용 이미지를 저장하고 지역 레지스트리는 사용자의 개인용 이미지 또는 공용 이미지를 저장합니다. 포트 4443은 [이미지 서명 확인](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)과 같은 공증 기능에 필요합니다. <table summary="표에서 첫 번째 행은 두 열 모두에 걸쳐 있습니다. 나머지 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며, 서버 구역은 1열에 있고 일치시킬 IP 주소는 2열에 있습니다.">
  <caption>레지스트리 트래픽을 위해 열리는 IP 주소</caption>
    <thead>
      <th>{{site.data.keyword.containerlong_notm}} 지역</th>
      <th>레지스트리 주소</th>
      <th>레지스트리 공용 서브넷</th>
      <th>레지스트리 사설 IP 주소</th>
    </thead>
    <tbody>
      <tr>
        <td><br>{{site.data.keyword.containerlong_notm}} 지역 전체의 글로벌 레지스트리</td>
        <td><code>icr.io</code><br><br>
        더 이상 사용되지 않음: <code>registry.bluemix.net</code></td>
        <td><code>169.60.72.144/28</code></br><code>169.61.76.176/28</code></br><code>169.62.37.240/29</code></br><code>169.60.98.80/29</code></br><code>169.63.104.232/29</code></td>
        <td><code>166.9.20.4</code></br><code>166.9.22.3</code></br><code>166.9.24.2</code></td>
      </tr>
      <tr>
        <td>AP 북부</td>
        <td><code>jp.icr.io</code><br><br>
        더 이상 사용되지 않음: <code>registry.au-syd.bluemix.net</code></td>
        <td><code>161.202.146.86/29</code></br><code>128.168.71.70/29</code></br><code>165.192.71.222/29</code></td>
        <td><code>166.9.40.3</code></br><code>166.9.42.3</code></br><code>166.9.44.3</code></td>
      </tr>
      <tr>
        <td>AP 남부</td>
        <td><code>au.icr.io</code><br><br>
        더 이상 사용되지 않음: <code>registry.au-syd.bluemix.net</code></td>
        <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></br><code>168.1.1.240/29</code></br><code>130.198.88.128/29</code></br><code>135.90.66.48/29</code></td>
        <td><code>166.9.52.2</code></br><code>166.9.54.2</code></br><code>166.9.56.3</code></td>
      </tr>
      <tr>
        <td>중앙 유럽</td>
        <td><code>de.icr.io</code><br><br>
        더 이상 사용되지 않음: <code>registry.eu-de.bluemix.net</code></td>
        <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></br><code>169.50.58.104/29</code></br><code>161.156.93.16/29</code></br><code>149.81.79.152/29</code></td>
        <td><code>166.9.28.12</code></br><code>166.9.30.9</code></br><code>166.9.32.5</code></td>
       </tr>
       <tr>
        <td>영국 남부</td>
        <td><code>uk.icr.io</code><br><br>
        더 이상 사용되지 않음: <code>registry.eu-gb.bluemix.net</code></td>
        <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></br><code>158.175.97.184/29</code></br><code>158.176.105.64/29</code></br><code>141.125.71.136/29</code></td>
        <td><code>166.9.36.9</code></br><code>166.9.38.5</code></br><code>166.9.34.4</code></td>
       </tr>
       <tr>
        <td>미국 동부, 미국 남부</td>
        <td><code>us.icr.io</code><br><br>
        더 이상 사용되지 않음: <code>registry.ng.bluemix.net</code></td>
        <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></br><code>169.61.234.224/29</code></br><code>169.61.135.160/29</code></br><code>169.61.46.80/29</code></td>
        <td><code>166.9.12.114</code></br><code>166.9.15.50</code></br><code>166.9.16.173</code></td>
       </tr>
      </tbody>
    </table>

4. 선택사항: 작업자 노드에서 {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, Sysdig 및 서비스로의 발신 네트워크 트래픽을 허용하십시오.
    *   **{{site.data.keyword.monitoringlong_notm}}**:
        <pre class="screen">TCP port 443, port 9095 FROM &lt;each_worker_node_public_IP&gt; TO &lt;monitoring_subnet&gt;</pre>
        <em>&lt;monitoring_subnet&gt;</em>을 트래픽을 허용하려는 모니터링 지역에 대한 서브넷으로 대체하십시오.
        <p><table summary="표에서 첫 번째 행은 두 열 모두에 걸쳐 있습니다. 나머지 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며, 서버 구역은 1열에 있고 일치시킬 IP 주소는 2열에 있습니다.">
  <caption>모니터링 트래픽을 위해 열리는 IP 주소</caption>
        <thead>
        <th>{{site.data.keyword.containerlong_notm}} 지역</th>
        <th>모니터링 주소</th>
        <th>서브넷 모니터링</th>
        </thead>
      <tbody>
        <tr>
         <td>중앙 유럽</td>
         <td><code>metrics.eu-de.bluemix.net</code></td>
         <td><code>158.177.65.80/30</code></td>
        </tr>
        <tr>
         <td>영국 남부</td>
         <td><code>metrics.eu-gb.bluemix.net</code></td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>미국 동부, 미국 남부, AP 북부, AP 남부</td>
          <td><code>metrics.ng.bluemix.net</code></td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
    *   **{{site.data.keyword.mon_full_notm}}**:
        <pre class="screen">TCP port 443, port 6443 FROM &lt;each_worker_node_public_IP&gt; TO &lt;sysdig_public_IP&gt;</pre>
        `<sysdig_public_IP>`를 [Sysdig IP 주소](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-network#network)로 대체하십시오.
    *   **{{site.data.keyword.loganalysislong_notm}}**:
        <pre class="screen">TCP port 443, port 9091 FROM &lt;each_worker_node_public_IP&gt; TO &lt;logging_public_IP&gt;</pre>
<em>&lt;logging_public_IP&gt;</em>를 트래픽을 허용할 로깅 지역에 대한 모든 주소로 대체하십시오.
        <p><table summary="표에서 첫 번째 행은 두 열 모두에 걸쳐 있습니다. 나머지 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며, 서버 구역은 1열에 있고 일치시킬 IP 주소는 2열에 있습니다.">
<caption>로깅 트래픽을 위해 열리는 IP 주소</caption>
        <thead>
        <th>{{site.data.keyword.containerlong_notm}} 지역</th>
        <th>로깅 주소</th>
        <th>로깅 IP 주소</th>
        </thead>
        <tbody>
          <tr>
            <td>미국 동부, 미국 남부</td>
            <td><code>ingest.logging.ng.bluemix.net</code></td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
          </tr>
          <tr>
           <td>영국 남부</td>
           <td><code>ingest.logging.eu-gb.bluemix.net</code></td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>중앙 유럽</td>
           <td><code>ingest-eu-fra.logging.bluemix.net</code></td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>AP 남부, AP 북부</td>
           <td><code>ingest-au-syd.logging.bluemix.net</code></td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>
    *   **{{site.data.keyword.la_full_notm}}**:
        <pre class="screen">TCP port 443, port 80 FROM &lt;each_worker_node_public_IP&gt; TO &lt;logDNA_public_IP&gt;</pre>
        `<logDNA_public_IP>`를 [LogDNA IP 주소](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-network#network)로 대체하십시오.

5. 로드 밸런서 서비스를 사용하는 경우에는 VRRP 프로토콜을 사용하는 모든 트래픽이 공용 및 사설 인터페이스의 작업자 노드 간에 허용되는지 확인하십시오. {{site.data.keyword.containerlong_notm}}는 VRRP 프로토콜을 사용하여 공용 및 사설 로드 밸런서의 IP 주소를 관리합니다.

6. {: #pvc}사설 클러스터에서 지속적 볼륨 클레임을 작성하려면 클러스터가 다음 Kubernetes 버전 또는 {{site.data.keyword.Bluemix_notm}} 스토리지 플러그인 버전으로 설정되어 있는지 확인하십시오. 이러한 버전을 사용하면 클러스터에서 지속적 스토리지 인스턴스로의 사설 네트워크 통신을 사용할 수 있습니다.
    <table>
    <caption>사설 클러스터를 위한 필수 Kubernetes 또는 {{site.data.keyword.Bluemix_notm}} 스토리지 플러그인 버전 개요</caption>
    <thead>
      <th>스토리지 유형</th>
      <th>필수 버전</th>
   </thead>
   <tbody>
     <tr>
       <td>파일 스토리지</td>
       <td>Kubernetes 버전 <code>1.13.4_1512</code>, <code>1.12.6_1544</code>, <code>1.11.8_1550</code>, <code>1.10.13_1551</code> 이상</td>
     </tr>
     <tr>
       <td>블록 스토리지</td>
       <td>{{site.data.keyword.Bluemix_notm}} Block Storage 플러그인 버전 1.3.0 이상</td>
     </tr>
     <tr>
       <td>오브젝트 스토리지</td>
       <td><ul><li>{{site.data.keyword.cos_full_notm}} 플러그인 버전 1.0.3 이상</li><li>HMAC 인증을 사용하여 {{site.data.keyword.cos_full_notm}} 서비스 설정</li></ul></td>
     </tr>
   </tbody>
   </table>

   사설 네트워크를 통한 네트워크 통신을 지원하지 않는 Kubernetes 버전 또는 {{site.data.keyword.Bluemix_notm}} 스토리지 플러그인 버전을 사용해야 하는 경우 또는 HMAC 인증없이 {{site.data.keyword.cos_full_notm}}를 사용하려는 경우에는 방화벽을 통해 IBM Cloud 인프라(SoftLayer) 및 {{site.data.keyword.Bluemix_notm}} Identity and Access Management에 대한 egress 액세스를 다음과 같이 허용하십시오.
   - TCP 포트 443에서 모든 egress 네트워크 트래픽을 허용하십시오.
   - [**프론트 엔드(공용) 네트워크**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#frontend-public-network) 및 [**백엔드(사설) 네트워크**](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network) 둘 다에 대해 클러스터가 있는 구역의 IBM Cloud 인프라(SoftLayer) IP 범위에 대한 액세스를 허용하십시오. 클러스터의 구역을 찾으려면 `ibmcloud ks clusters`를 실행하십시오.


<br />


## 클러스터가 사설 방화벽을 거쳐 리소스에 액세스할 수 있도록 허용
{: #firewall_private}

사설 네트워크에 방화벽이 있는 경우에는 작업자 노드 간의 통신을 허용하고 클러스터가 사설 네트워크에서 인프라 리소스에 액세스할 수 있도록 허용하십시오.
{:shortdesc}

1. 작업자 노드 간에 모든 트래픽을 허용하십시오.
    1. 공용 및 사설 인터페이스에서 작업자 노드 간에 모든 TCP, UDP, VRRP 및 IPEncap 트래픽을 허용하십시오. {{site.data.keyword.containerlong_notm}}는 사설 로드 밸런서의 IP 주소를 관리하는 데 VRRP 프로토콜을 사용하고 서브넷 간 팟(Pod) 대 팟(Pod) 트래픽을 허용하는 데 IPEncap 프로토콜을 사용합니다.
    2. Calico 정책을 사용 중이거나 다중 구역 클러스터의 각 구역에 방화벽이 있는 경우에는 방화벽이 작업자 노드 간의 통신을 차단할 수 있습니다. 작업자의 포트, 작업자의 사설 IP 주소 또는 Calico 작업자 노드 레이블을 사용하여 서로 간에 클러스터의 모든 작업자 노드를 열어야 합니다.

2. 클러스터에서 작업자 노드를 작성할 수 있도록 IBM Cloud 인프라(SoftLayer) 사설 IP 범위를 허용하십시오.
    1. 적합한 IBM Cloud 인프라(SoftLayer) 사설 IP 범위를 허용하십시오. [백엔드(사설) 네트워크](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#backend-private-network)를 참조하십시오.
    2. 사용 중인 모든 [구역](/docs/containers?topic=containers-regions-and-zones#zones)에 대해 IBM Cloud 인프라(SoftLayer) 사설 IP 범위를 허용하십시오. `dal01`, `dal10`, `wdc04` 구역(클러스터가 유럽 지역에 있는 경우에는 `ams01` 구역까지)의 IP를 추가해야 한다는 점을 유의하십시오. [(백엔드/사설 네트워크의) 서비스 네트워크](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#service-network-on-backend-private-network-)를 참조하십시오.

3. 다음 포트를 여십시오.
    - 작업자 노드 업데이트와 재로드가 허용되도록 작업자에서 포트 80 및 443으로 아웃바운드 TCP 및 UDP 연결을 허용하십시오.
    - 볼륨으로서 파일 스토리지의 마운트가 허용되도록 포트 2049로의 아웃바운드 TCP 및 UDP를 허용하십시오.
    - 블록 스토리지와의 통신에 대해 포트 3260으로의 아웃바운드 TCP 및 UDP를 허용합니다.
    - Kubernetes 대시보드와 명령(예: `kubectl logs` 및 `kubectl exec`)에 대해 포트 10250으로의 인바운드 TCP 및 UDP 연결을 허용하십시오.
    - DNS 액세스용 TCP 및 UDP 포트 53에 대한 인바운드와 아웃바운드 연결을 허용하십시오.

4. 공용 네트워크에도 방화벽이 있는 경우, 또는 사설 VLAN 전용 클러스터가 있으며 게이트웨이 디바이스를 방화벽으로 사용 중인 경우에는 [클러스터가 인프라 리소스 및 기타 서비스에 액세스할 수 있도록 허용](#firewall_outbound)에 지정된 IP 및 포트도 허용해야 합니다. 

<br />


## 클러스터가 Calico 유출 정책을 통해 액세스할 수 있도록 허용
{: #firewall_calico_egress}

모든 공용 작업자 유출을 제한하기 위해 방화벽의 역할을 수행하도록 [Calico 네트워크 정책](/docs/containers?topic=containers-network_policies)을 사용하는 경우 작업자가 마스터 API 서버 및 etcd에 대한 로컬 프록시에 액세스하도록 허용해야 합니다.
{: shortdesc}

1. [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) `--admin` 및 `--network` 옵션을 `ibmcloud ks cluster-config` 명령에 포함하십시오. `--admin`은 인프라 포트폴리오에 액세스하고 작업자 노드에서 Calico 명령을 실행하기 위한 키를 다운로드합니다. `--network`는 모든 Calico 명령을 실행하기 위한 Calico 구성 파일을 다운로드합니다.
  ```
  ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin --network
  ```
  {: pre}

2. 클러스터에서 172.20.0.1:2040 및 172.21.0.1:443(API 서버용)과 172.20.0.1:2041(etcd 로컬 프록시용)로 공용 트래픽을 허용하는 Calico 네트워크 정책을 작성하십시오.
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

3. 클러스터에 정책을 적용하십시오.
    - Linux 및 OS X:

      ```
      calicoctl apply -f allow-master-local.yaml
      ```
      {: pre}

    - Windows:

      ```
      calicoctl apply -f filepath/allow-master-local.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

## 클러스터 외부에서 NodePort, 로드 밸런서 및 Ingress 서비스에 액세스
{: #firewall_inbound}

NodePort, 로드 밸런서 및 Ingress 서비스에 대한 수신 액세스를 허용할 수 있습니다.
{:shortdesc}

<dl>
  <dt>NodePort 서비스</dt>
  <dd>모든 작업자 노드가 트래픽을 허용할 수 있도록 서비스를 공인 IP 주소에 배치할 때 구성한 포트를 여십시오. 포트를 찾으려면 `kubectl get svc`를 실행하십시오. 포트는 20000 - 32000 범위에 있습니다.<dd>
  <dt>로드 밸런서 서비스</dt>
  <dd>서비스를 로드 밸런스 서비스의 공인 IP 주소에 배치할 때 구성한 포트를 여십시오.</dd>
  <dt>Ingress</dt>
  <dd>Ingress 애플리케이션 로드 밸런서의 IP 주소에 대해 HTTP의 경우 포트 80을 열고 HTTPS의 경우 포트 443을 여십시오.</dd>
</dl>

<br />


## 다른 서비스의 방화벽 또는 온프레미스 방화벽에서 클러스터를 화이트리스트로 지정
{: #whitelist_workers}

방화벽으로 보호되는 {{site.data.keyword.Bluemix_notm}} 또는 온프레미스 내부 또는 외부에서 실행하는 서비스에 액세스하려는 경우, 해당 방화벽에서 작업자 노드의 IP 주소를 추가하여 클러스터에 대한 아웃바운드 네트워크 트래픽을 허용할 수 있습니다. 예를 들어, 방화벽으로 보호되는 {{site.data.keyword.Bluemix_notm}} 데이터베이스에서 데이터를 읽거나 온프레미스 방화벽에서 작업자 노드 서브넷을 화이트리스트로 지정하여 클러스터에서 네트워크 트래픽을 허용할 수 있습니다.
{:shortdesc}

1.  [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

2. 작업자 노드 서브넷 또는 작업자 노드 IP 주소를 가져오십시오.
  * **작업자 노드 서브넷**: 클러스터의 작업자 노드 수를 자주 변경하려는 경우(예: [클러스터 오토스케일러](/docs/containers?topic=containers-ca#ca) 사용), 새 작업자 노드마다 방화벽을 업데이트하지 않을 수 있습니다. 대신 클러스터에서 사용하는 VLAN 서브넷을 화이트리스트로 지정할 수 있습니다. 다른 클러스터의 작업자 노드에 의해 VLAN 서브넷이 공유될 수 있다는 점에 유의하십시오.
    <p class="note">클러스터에 대해 {{site.data.keyword.containerlong_notm}}가 프로비저닝한 **기본 공용 서브넷**은 14개의 사용 가능한 IP 주소와 함께 제공되며 동일한 VLAN의 다른 클러스터에서 공유할 수 있습니다. 14개 이상의 작업자 노드가 있으면 다른 서브넷이 주문되므로 화이트리스트에 필요한 서브넷을 변경할 수 있습니다. 변경 빈도를 줄이려면 작업자 노드를 자주 추가할 필요가 없도록 고성능 CPU와 메모리 리소스의 작업자 노드 특성이 있는 작업자 풀을 작성하십시오.</p>
    1. 클러스터의 작업자 노드를 나열합니다.
      ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

    2. 이전 단계의 출력에서 클러스터의 작업자 노드에 대한 **Public IP**의 고유 네트워크 ID(처음 세 옥텟)을 모두 기록해 두십시오. 사설 전용 클러스터를 화이트리스트 지정하려는 경우에는 **Private IP**를 대신 기록해 두십시오. 다음 출력에서 고유 네트워크 ID는 `169.xx.178` 및 `169.xx.210`입니다.
        ```
ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w31   169.xx.178.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.13.6   
        kube-dal10-crb2f60e9735254ac8b20b9c1e38b649a5-w34   169.xx.178.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal10   1.13.6  
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w32   169.xx.210.101   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.13.6   
        kube-dal12-crb2f60e9735254ac8b20b9c1e38b649a5-w33   169.xx.210.102   10.xxx.xx.xxx   b3c.4x16.encrypted   normal   Ready    dal12   1.13.6  
        ```
        {: screen}
    3.  각 고유 네트워크 ID에 대한 VLAN 서브넷을 나열하십시오.
        ```
        ibmcloud sl subnet list | grep -e <networkID1> -e <networkID2>
        ```
        {: pre}

        출력 예:
        ```
        ID        identifier       type                 network_space   datacenter   vlan_id   IPs   hardware   virtual_servers
        1234567   169.xx.210.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal12        1122334   16    0          5   
        7654321   169.xx.178.xxx   ADDITIONAL_PRIMARY   PUBLIC          dal10        4332211   16    0          6    
        ```
        {: screen}
    4.  서브넷 주소를 검색하십시오. 출력에서 **IPs**의 수를 찾으십시오. 그런 다음 `2`를 IPs와 같은 `n`의 거듭제곱으로 올리십시오. 예를 들어 IPs의 수가 `16`이면 `16`과 같도록 `2`를 `4`(`n`)의 거듭제곱으로 올립니다. 이제 `32`비트에서 `n`의 값을 빼서 서브넷 CIDR을 얻으십시오. 예를 들어, `n`이 `4`인 경우 CIDR은 `28`(`32 - 4 = 28` 방정식에서)입니다. **ID** 표시를 CIDR 값과 결합하여 전체 서브넷 주소를 얻으십시오. 이전 출력에서 서브넷 주소는 다음과 같습니다.
        *   `169.xx.210.xxx/28`
        *   `169.xx.178.xxx/28`
  * **개별 작업자 노드 IP 주소**: 하나의 앱만 실행하고 확장할 필요가 없는 작업자 노드의 수가 적은 경우 또는 하나의 작업자 노드만 화이트리스트로 지정하려는 경우 클러스터의 모든 작업자 노드를 나열하여 **공인 IP** 주소를 기록해 두십시오. 작업자 노드가 사설 네트워크에만 연결되어 있고 개인 서비스 엔드포인트를 사용하여 {{site.data.keyword.Bluemix_notm}} 서비스에 연결하려면 대신 **사설 IP** 주소를 기록해 두십시오. 이러한 작업자 노드는 화이트리스트로 지정됩니다. 작업자 노드를 삭제하거나 클러스터에 작업자 노드를 추가하면 이에 따라 방화벽을 업데이트해야 합니다.
    ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}
4.  아웃바운드 트래픽의 경우 서브넷 CIDR 또는 IP 주소를 서비스의 방화벽에 추가하고, 인바운드 트래픽의 경우 온프레미스 방화벽에 서브넷 CIDR 또는 IP 주소를 추가하십시오.
5.  화이트리스트로 지정할 각각의 클러스터에 대해 이러한 단계를 반복하십시오.
