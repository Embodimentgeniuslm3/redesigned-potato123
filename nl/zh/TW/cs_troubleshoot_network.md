---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# 叢集網路的疑難排解
{: #cs_troubleshoot_network}

在您使用 {{site.data.keyword.containerlong}} 時，請考慮使用這些技術來進行叢集網路的疑難排解。
{: shortdesc}

透過 Ingress 連接至應用程式時發生困難嗎？請嘗試[除錯 Ingress](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress)。
{: tip}

疑難排解時，您可以使用 [{{site.data.keyword.containerlong_notm}}Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) 來執行測試，並從叢集收集相關的網路、Ingress 及 strongSwan 資訊。
{: tip}

## 無法透過網路負載平衡器 (NLB) 服務連接至應用程式
{: #cs_loadbalancer_fails}

{: tsSymptoms}
您已透過在叢集裡建立 NLB 服務，公然地公開應用程式。當您嘗試使用 NLB 的公用 IP 位址連接至應用程式時，連線失敗或逾時。

{: tsCauses}
NLB 服務可能因下列其中一個原因而未正常運作：

-   叢集是免費叢集或只有一個工作者節點的標準叢集。
-   尚未完整部署叢集。
-   NLB 服務的配置 Script 包含錯誤。

{: tsResolve}
若要對 NLB 服務進行疑難排解，請執行下列動作：

1.  確認您所設定的標準叢集已完整部署並且至少有兩個工作者節點，以確保 NLB 服務的高可用性。

  ```
        ibmcloud ks workers --cluster <cluster_name_or_ID>
        ```
  {: pre}

    在 CLI 輸出中，確定工作者節點的 **Status** 顯示 **Ready**，而且 **Machine Type** 顯示 **free** 以外的機型。

2. 若為 2.0 版 NLB：請確定您已完成 [NLB 2.0 必要條件](/docs/containers?topic=containers-loadbalancer#ipvs_provision)。

3. 檢查 NLB 服務配置檔的正確性。
    * 2.0 版 NLB：
        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myservice
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
        spec:
          type: LoadBalancer
          selector:
            <selector_key>:<selector_value>
          ports:
           - protocol: TCP
             port: 8080
          externalTrafficPolicy: Local
        ```
        {: screen}

        1. 確認您已將 **LoadBalancer** 定義為服務的類型。
        2. 確認您已包含 `service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"` 註釋。
        3. 在 LoadBalancer 服務的 `spec.selector` 區段中，確定 `<selector_key>` 及 `<selector_value>` 與您在部署 YAML 的 `spec.template.metadata.labels` 區段中所使用的鍵值組相同。如果標籤不相符，則 LoadBalancer 服務中的 **Endpoints** 區段會顯示 **<none>**，且無法從網際網路存取您的應用程式。
        4. 確認您已使用應用程式所接聽的**埠**。
        5. 確認您已將 `externalTrafficPolicy` 設為 `Local`。

    * 1.0 版 NLB：
        ```
        apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      type: LoadBalancer
      selector:
        <selector_key>:<selector_value>
      ports:
           - protocol: TCP
             port: 8080
        ```
        {: screen}

        1. 確認您已將 **LoadBalancer** 定義為服務的類型。
        2. 在 LoadBalancer 服務的 `spec.selector` 區段中，確定 `<selector_key>` 及 `<selector_value>` 與您在部署 YAML 的 `spec.template.metadata.labels` 區段中所使用的鍵值組相同。如果標籤不相符，則 LoadBalancer 服務中的 **Endpoints** 區段會顯示 **<none>**，且無法從網際網路存取您的應用程式。
        3. 確認您已使用應用程式所接聽的**埠**。

3.  檢閱 NLB 服務，並檢閱 **Events** 區段來尋找可能的錯誤。

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    尋找下列錯誤訊息：

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>若要使用 NLB 服務，您必須有至少包含兩個工作者節點的標準叢集。</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the NLB service request. Add a portable subnet to the cluster and try again</code></pre></br>此錯誤訊息指出未將可攜式公用 IP 位址配置給 NLB 服務。請參閱<a href="/docs/containers?topic=containers-subnets#subnets">將子網路新增至叢集</a>，以尋找如何要求叢集之可攜式公用 IP 位址的相關資訊。叢集可以使用可攜式公用 IP 位址之後，即會自動建立 NLB 服務。</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>您已使用 **`loadBalancerIP`** 區段定義負載平衡器 YAML 的可攜式公用 IP 位址，但在可攜式公用子網路中無法使用此可攜式公用 IP 位址。在配置 Script 的 **`loadBalancerIP`** 區段中，移除現有 IP 位址，並新增其中一個可用的可攜式公用 IP 位址。您也可以移除 Script 中的 **`loadBalancerIP`** 區段，以自動配置可用的可攜式公用 IP 位址。</li>
    <li><pre class="screen"><code>No available nodes for NLB services</code></pre>您沒有足夠的工作者節點可部署 NLB 服務。其中一個原因可能是您所部署的標準叢集有多個工作者節點，但佈建工作者節點失敗。
    </li>
    <ol><li>列出可用的工作者節點。</br><pre class="pre"><code>kubectl get nodes</code></pre></li>
    <li>如果找到至少兩個可用的工作者節點，則會列出工作者節點詳細資料。</br><pre class="pre"><code>ibmcloud ks worker-get --cluster &lt;cluster_name_or_ID&gt; --worker &lt;worker_ID&gt;</code></pre></li>
    <li>請確定 <code>kubectlget nodes</code> 及 <code>ibmcloud ks worker-get</code> 指令所傳回工作者節點的公用及專用 VLAN ID 相符。</li></ol></li></ul>

4.  如果使用自訂網域連接至 NLB 服務，請確定已將自訂網域對映至 NLB 服務的公用 IP 位址。
    1.  尋找 NLB 服務的公用 IP 位址。
        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  確認在「指標記錄 (PTR)」中已將自訂網域對映至 NLB 服務的可攜式公用 IP 位址。

<br />


## 無法透過 Ingress 連接至應用程式
{: #cs_ingress_fails}

{: tsSymptoms}
您已透過在叢集裡建立應用程式的 Ingress 資源，來公開應用程式。當您嘗試使用 Ingress 應用程式負載平衡器 (ALB) 的公用 IP 位址或子網域連接至應用程式時，連線失敗或逾時。

{: tsResolve}
首先，確認您的叢集已完整部署並且每個區域至少有 2 個工作者節點可用，以確保 ALB 的高可用性。
```
        ibmcloud ks workers --cluster <cluster_name_or_ID>
        ```
{: pre}

在 CLI 輸出中，確定工作者節點的 **Status** 顯示 **Ready**，而且 **Machine Type** 顯示 **free** 以外的機型。

* 如果您的標準叢集已完整部署並且每個區域至少有 2 個工作者節點可用，但沒有 **Ingress 子網域**可用，則請參閱[無法取得 Ingress ALB 的子網域](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit)。
* 若為其他問題，請遵循[除錯 Ingress](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress) 中的步驟，以對 Ingress 設定進行疑難排解。

<br />


## Ingress 應用程式負載平衡器 (ALB) 密碼問題
{: #cs_albsecret_fails}

{: tsSymptoms}
使用 `ibmcloud ks alb-cert-deploy` 指令將 Ingress 應用程式負載平衡器 (ALB) 密碼部署到叢集之後，當您在 {{site.data.keyword.cloudcerts_full_notm}} 中檢視憑證時，不會使用密碼名稱來更新 `Description` 欄位。

當您列出 ALB 密碼的相關資訊時，狀態顯示為 `*_failed`。例如，`create_failed`、`update_failed`、`delete_failed`。

{: tsResolve}
請檢閱下列原因，以了解 ALB 密碼為何失敗以及對應的疑難排解步驟：

<table>
<caption>疑難排解 Ingress 應用程式負載平衡器密碼</caption>
 <thead>
 <th>發生原因</th>
 <th>修正方式</th>
 </thead>
 <tbody>
 <tr>
 <td>您沒有必要的存取角色，無法下載及更新憑證資料。</td>
 <td>請洽詢帳戶「管理者」，以將下列 {{site.data.keyword.Bluemix_notm}} IAM 角色指派給您：<ul><li>{{site.data.keyword.cloudcerts_full_notm}} 實例的**管理員**及**撰寫者**服務角色。如需相關資訊，請參閱 {{site.data.keyword.cloudcerts_short}} 的<a href="/docs/services/certificate-manager?topic=certificate-manager-managing-service-access-roles#managing-service-access-roles">管理服務存取</a>。</li><li>叢集的<a href="/docs/containers?topic=containers-users#platform">**管理者**平台角色</a>。</li></ul></td>
 </tr>
 <tr>
 <td>建立、更新或移除時提供的憑證 CRN 與叢集不屬於相同的帳戶。</td>
 <td>請確定您提供的憑證 CRN 已匯入到部署在與叢集相同帳戶中的 {{site.data.keyword.cloudcerts_short}} 服務實例。</td>
 </tr>
 <tr>
 <td>建立時提供的憑證 CRN 不正確。</td>
 <td><ol><li>檢查您提供之憑證 CRN 字串的正確性。</li><li>如果發現憑證 CRN 是正確的，請嘗試更新密碼：<code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>如果這個指令導致 <code>update_failed</code> 狀態，則移除密碼：<code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>重新部署密碼：<code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>更新時提供的憑證 CRN 不正確。</td>
 <td><ol><li>檢查您提供之憑證 CRN 字串的正確性。</li><li>如果發現憑證 CRN 是正確的，則移除密碼：<code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>重新部署密碼：<code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>嘗試更新密碼：<code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>{{site.data.keyword.cloudcerts_long_notm}} 服務遭遇關閉時間。</td>
 <td>確定您的 {{site.data.keyword.cloudcerts_short}} 服務已啟動並執行。</td>
 </tr>
 <tr>
 <td>您所匯入的密碼與 IBM 所提供的 Ingress 密碼同名。</td>
 <td>重新命名您的密碼。您可以執行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress`，以檢查 IBM 提供的 Ingress 密碼名稱。</td>
 </tr>
 </tbody></table>

<br />


## 無法取得 Ingress ALB 的子網域，區域中未部署 ALB，或無法部署負載平衡器
{: #cs_subnet_limit}

{: tsSymptoms}
* 無 Ingress 子網域：執行 `ibmcloud ks cluster-get --cluster <cluster>` 時，叢集處於 `normal` 狀態，但沒有 **Ingress 子網域**可用。
* 區域中未部署 ALB：具有多區域叢集並執行 `ibmcloud ks albs --cluster <cluster>` 時，區域中未部署任何 ALB。例如，如果您在 3 個區域中具有工作者節點，則可能會看到如下的輸出，其中公用 ALB 未部署至第三個區域。
  ```
  ALB ID                                            Enabled    Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
  private-cr96039a75fddb4ad1a09ced6699c88888-alb1   false      disabled   private   -                dal10   ingress:411/ingress-auth:315   2294021
  private-cr96039a75fddb4ad1a09ced6699c88888-alb2   false      disabled   private   -                dal12   ingress:411/ingress-auth:315   2234947
  private-cr96039a75fddb4ad1a09ced6699c88888-alb3   false      disabled   private   -                dal13   ingress:411/ingress-auth:315   2234943
  public-cr96039a75fddb4ad1a09ced6699c88888-alb1    true       enabled    public    169.xx.xxx.xxx   dal10   ingress:411/ingress-auth:315   2294019
  public-cr96039a75fddb4ad1a09ced6699c88888-alb2    true       enabled    public    169.xx.xxx.xxx   dal12   ingress:411/ingress-auth:315   2234945
  ```
  {: screen}
* 無法部署負載平衡器：說明 `ibm-cloud-provider-vlan-ip-config` 配置映射時，可能會看到類似於下列範例輸出的錯誤訊息。
  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config
  ```
  {: pre}
  ```
  Warning  CreatingLoadBalancerFailed ... ErrorSubnetLimitReached: There are already the maximum number of subnets permitted in this VLAN.
  ```
  {: screen}

{: tsCauses}
在標準叢集裡，第一次在區域中建立叢集時，會在您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶中，自動為您在該區域中佈建公用 VLAN 及專用 VLAN。在該區域中，會在您指定的公用 VLAN 上要求 1 個公用可攜式子網路，並在您指定的專用 VLAN 上要求 1 個專用可攜式子網路。對於 {{site.data.keyword.containerlong_notm}}，VLAN 的限制為 40 個子網路。如果某個區域中叢集的 VLAN 已達到該限制，則佈建 **Ingress 子網域**會失敗，佈建該區域的公用 Ingress ALB 會失敗，或者您可能沒有可攜式的公用 IP 位址可用於建立網路負載平衡器 (NLB)。

若要檢視 VLAN 有多少子網路，請執行下列動作：
1.  從 [IBM Cloud 基礎架構 (SoftLayer) 主控台](https://cloud.ibm.com/classic?)，選取**網路** > **IP 管理** > **VLAN**。
2.  按一下您用來建立叢集之 VLAN 的 **VLAN 號碼**。檢閱 **Subnets** 區段，以查看是否有 40 個以上的子網路。

{: tsResolve}
如果您需要新的 VLAN，請[與 {{site.data.keyword.Bluemix_notm}} 支援中心聯絡](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans)，進行訂購。然後，[建立叢集](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)，而叢集使用這個新的 VLAN。


如果您有另一個可用的 VLAN，可以在現有叢集裡[設定 VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)。之後，您便可以將新的工作者節點新增至使用具有可用子網路之另一個 VLAN 的叢集。若要確認是否已啟用 VLAN Spanning，請使用 `ibmcloud ks vlan-spanning-get --region <region>` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。

如果並未使用 VLAN 中的所有子網路，則可以透過將 VLAN 上的子網路新增到叢集來重複使用這些子網路。
1. 檢查要使用的子網路是否可用。
  <p class="note">使用的基礎架構帳戶可能在多個 {{site.data.keyword.Bluemix_notm}} 帳戶之間共用。在這種情況下，即使執行 `ibmcloud ks subnets` 指令來查看 **Bound Cluster** 的子網路，也只能看到您的叢集的資訊。請洽詢基礎架構帳戶擁有者，以確定子網路可供使用，且其他任何帳戶或團隊不在使用中。</p>

2. 使用 [`ibmcloud ks cluster-subnet-add` 指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add)使現有子網路可供叢集使用。

3. 驗證已順利建立子網路並將其新增至叢集。子網路 CIDR 列在 **Subnet VLANs** 區段中。
    ```
    ibmcloud ks cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

    在此輸出範例中，第二個子網路已新增至 `2234945` 公用 VLAN：
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR          Public   User-managed
    2234947   10.xxx.xx.xxx/29     false    false
    2234945   169.xx.xxx.xxx/29    true     false
    2234945   169.xx.xxx.xxx/29    true     false
    ```
    {: screen}

4. 驗證新增的子網路中的可攜式 IP 位址是否用於叢集裡的 ALB 或負載平衡器。服務可能需要幾分鐘時間才能使用新增子網路中的可攜式 IP 位址。
  * 無 Ingress 子網域：執行 `ibmcloud ks cluster-get --cluster <cluster>` 以驗證是否移入了 **Ingress 子網域**。
  * 區域中未部署 ALB：執行 `ibmcloud ks albs --cluster <cluster>` 以驗證是否部署的是遺漏的 ALB。
  * 無法部署負載平衡器：執行 `kubectl get svc -n kube-system` 以驗證負載平衡器是否具有 **EXTERNAL-IP**。

<br />


## 透過 WebSocket 的連線會在 60 秒後結束
{: #cs_ingress_websocket}

{: tsSymptoms}
您的 Ingress 服務會公開一個使用 WebSocket 的應用程式。不過，當用戶端與 WebSocket 應用程式之間有 60 秒未傳送任何資料流量時，就會關閉其間的連線。

{: tsCauses}
在閒置 60 秒之後，可能會因下列其中一個原因而捨棄與 WebSocket 應用程式的連線：

* 您的網際網路連線具有不容忍長期連線的 Proxy 或防火牆。
* ALB 到 WebSocket 應用程式的逾時會終止連線。

{: tsResolve}
若要防止在閒置 60 秒後關閉連線，請執行下列動作：

1. 如果您透過 Proxy 或防火牆連接至 WebSocket 應用程式，請確定未將 Proxy 或防火牆配置為自動終止長期連線。

2. 若要保持連線處於作用中狀態，您可以增加逾時值，或在應用程式中設定活動訊號。
<dl><dt>變更逾時</dt>
<dd>在 ALB 配置中增加 `proxy-read-timeout` 值。例如，若要將逾時從 `60s` 變更為較大值（例如 `300s`），請將此[註釋](/docs/containers?topic=containers-ingress_annotation#connection)新增至 Ingress 資源檔：`ingress.bluemix.net/proxy-read-timeout: "serviceName=<service_name> timeout=300s"`。已變更叢集裡所有公用 ALB 的逾時。</dd>
<dt>設定活動訊號</dt>
<dd>如果您不要變更 ALB 的預設讀取逾時值，請設定 WebSocket 應用程式中的活動訊號。當您使用 [WAMP ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://wamp-proto.org/) 這類架構來設定活動訊號通訊協定時，應用程式的上游伺服器會定期在計時間隔傳送 "ping" 訊息，而且用戶端會回應 "pong" 訊息。請將活動訊號間隔設為 58 秒或以下，讓 "ping/pong" 資料流量保持連線開啟，再施行 60 秒的逾時值。</dd></dl>

<br />


## 來源 IP 保留在使用有污點的節點時失敗
{: #cs_source_ip_fails}

{: tsSymptoms}
您已在服務的配置檔中將 `externalTrafficPolicy` 變更為 `Local`，以啟用 [1.0 版負載平衡器](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)或 [Ingress ALB](/docs/containers?topic=containers-ingress#preserve_source_ip) 服務的來源 IP 保留。不過，沒有任何資料流量到達您應用程式的後端服務。

{: tsCauses}
當您啟用負載平衡器或 Ingress ALB 服務的來源 IP 保留時，會保留用戶端要求的來源 IP 位址。服務只會將資料流量轉遞至相同工作者節點上的應用程式 Pod，以確保要求封包的 IP 位址未變更。一般而言，負載平衡器或 Ingress ALB 服務 Pod 會部署至在其中部署應用程式 Pod 的相同工作者節點。不過，存在某些狀況，可能未在相同的工作者節點上排定服務 Pod 及應用程式 Pod。如果您在工作者節點上使用 [Kubernetes 污點 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)，即會防止沒有污點容忍的任何 Pod 在有污點的工作者節點上執行。來源 IP 保留可能無法根據您使用的污點類型來運作：

* **邊緣節點污點**：您已[新增 `dedicated=edge` 標籤](/docs/containers?topic=containers-edge#edge_nodes)至叢集裡每個公用 VLAN 的兩個以上工作者節點，以確保 Ingress 及負載平衡器 Pod 只會部署至那些工作者節點。然後，您也可以[為邊緣節點加上污點](/docs/containers?topic=containers-edge#edge_workloads)，以防止在邊緣節點上執行任何其他工作負載。不過，您未將邊緣節點親緣性規則及容忍新增至應用程式部署。您的應用程式 Pod 無法排定於與服務 Pod 相同的有污點節點，而且沒有任何資料流量會到達您應用程式的後端服務。

* **自訂污點**：您已在數個節點上使用自訂污點，因此只會將具有該污點容忍的應用程式 Pod 部署至那些節點。您已將親緣性規則及容忍新增至應用程式及負載平衡器或 Ingress 服務的部署，因此其 Pod 只會部署至這些節點。不過，在 `ibm-system` 名稱空間中自動建立的 `ibm-cloud-provider-ip` `keepalived` Pod，確保負載平衡器 Pod 及應用程式 Pod 一律排定至相同的工作者節點。這些 `keepalived` Pod 沒有您所使用之自訂污點的容忍。它們無法排定於應用程式 Pod 執行所在的相同有污點節點，而且沒有任何資料流量會到達您應用程式的後端服務。

{: tsResolve}
選擇下列其中一個選項，以解決問題：

* **邊緣節點污點**：若要確保您的負載平衡器及應用程式 Pod 部署至有污點的邊緣節點，請[將邊緣節點親緣性規則及容忍新增至應用程式部署](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes)。依預設，負載平衡器及 Ingress ALB Pod 會具有這些親緣性規則及容忍。

* **自訂污點**：移除 `keepalived` Pod 沒有其容忍的自訂污點。相反地，您可以[將工作者節點標示為邊緣節點，然後為這些邊緣節點加上污點](/docs/containers?topic=containers-edge)。

如果您完成上述其中一個選項，但仍未排定 `keepalived` Pod，則可以取得 `keepalived` Pod 的相關資訊：

1. 取得 `keepalived` Pod。
    ```
    kubectl get pods -n ibm-system
    ```
    {: pre}

2. 在輸出中，尋找**狀態**為 `Pending` 的 `ibm-cloud-provider-ip` Pod。範例：
    ```
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t     0/1       Pending   0          2m        <none>          <none>
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-8ptvg     0/1       Pending   0          2m        <none>          <none>
    ```
    {:screen}

3. 說明每個 `keepalived` Pod，並尋找**事件**區段。請解決列出的所有錯誤或警告訊息。
    ```
    kubectl describe pod ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t -n ibm-system
    ```
    {: pre}

<br />


## 無法使用 strongSwan Helm 圖表建立 VPN 連線功能
{: #cs_vpn_fails}

{: tsSymptoms}
當您執行 `kubectl exec $STRONGSWAN_POD -- ipsec status` 來檢查 VPN 連線功能時，並未看到 `ESTABLISHED` 狀態，或是 VPN Pod 處於 `ERROR` 狀況，或持續當機及重新啟動。

{: tsCauses}
您的 Helm 圖表配置檔有不正確的值、遺漏值或語法錯誤。

{: tsResolve}
當您嘗試使用 strongSwan Helm 圖表建立 VPN 連線功能時，第一次 VPN 狀態可能不是 `ESTABLISHED`。您可能需要檢查數種問題，並據此變更配置檔。若要對 strongSwan VPN 連線功能進行疑難排解，請執行下列動作：

1. [測試並驗證 strongSwan VPN 連線功能](/docs/containers?topic=containers-vpn#vpn_test)，方法是執行五個包含在 strongSwan 圖表定義中的 Helm 測試。

2. 如果您在執行 Helm 測試之後無法建立 VPN 連線功能，則可以執行 VPN Pod 映像檔內所包裝的 VPN 除錯工具。

    1. 設定 `STRONGSWAN_POD` 環境變數。

        ```
        export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. 執行除錯工具。

        ```
        kubectl exec  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        此工具會輸出好幾頁的資訊，因為它會針對一般網路問題執行各種不同的測試。開頭為 `ERROR`、`WARNING`、`VERIFY` 或 `CHECK` 的輸出行，指出 VPN 連線功能可能發生的錯誤。

    <br />


## 無法安裝新版 strongSwan Helm 圖表
{: #cs_strongswan_release}

{: tsSymptoms}
您可以修改 strongSwan Helm 圖表，並嘗試安裝新版本，方法是執行 `helm install -f config.yaml --name=vpn ibm/strongswan`。不過，您會看到下列錯誤：
```
Error: release vpn failed: deployments.extensions "vpn-strongswan" already exists
```
{: screen}

{: tsCauses}
此錯誤指出未完全解除安裝舊版 strongSwan 圖表。

{: tsResolve}

1. 刪除舊版圖表。
    ```
    helm delete --purge vpn
    ```
    {: pre}

2. 刪除舊版的部署。刪除部署及關聯的 Pod 最多需要 1 分鐘。
    ```
    kubectl delete deploy vpn-strongswan
    ```
    {: pre}

3. 驗證已刪除部署。部署 `vpn-strongswan` 未出現在清單中。
    ```
    kubectl get deployments
    ```
    {: pre}

4. 利用新的版本名稱來重新安裝更新的 strongSwan Helm 圖表。
    ```
    helm install -f config.yaml --name=vpn ibm/strongswan
    ```
    {: pre}

<br />


## 在您新增或刪除工作者節點之後，strongSwan VPN 連線功能失敗
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
您先前已使用 strongSwan IPSec VPN 服務建立了工作中 VPN 連線。不過，您在叢集上新增或刪除工作者節點之後，發生下列一個以上的症狀：

* 您沒有 `ESTABLISHED` 的 VPN 狀態
* 您無法從內部部署網路中存取新的工作者節點
* 您無法從在新的工作者節點上執行的 Pod 中存取遠端網路

{: tsCauses}
如果您已將工作者節點新增至工作者節點儲存區：

* 工作者節點是佈建在新的專用子網路上，現有 `localSubnetNAT` 或 `local.subnet` 設定並未透過 VPN 連線公開該子網路
* VPN 路徑無法新增至工作者節點，因為工作者節點有污點或標籤未內含在現有 `tolerations` 或 `nodeSelector` 設定中
* VPN Pod 在新的工作者節點上執行，但不容許該工作者節點的公用 IP 位址透過內部部署防火牆

如果您已刪除工作者節點：

* 該工作者節點是執行 VPN Pod 的唯一節點，這是因為現有 `tolerations` 或 `nodeSelector` 設定中對特定污點或標籤有限制

{: tsResolve}
更新 Helm 圖表值，以反映工作者節點變更：

1. 刪除現有 Helm 圖表。

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. 開啟 strongSwan VPN 服務的配置檔。

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. 檢查下列設定，並視需要變更設定，以反映已刪除或新增的工作者節點。

    如果您已新增工作者節點：

    <table>
    <caption>工作者節點設定</caption?
     <thead>
     <th>設定</th>
     <th>說明</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>新增的工作者可能部署在另一個新的專用子網路上，而不是其他工作者節點所在的其他現有子網路。如果是使用子網路 NAT 來重新對映叢集的專用本端 IP 位址，並且在新子網路上新增了工作者節點，請將新的子網路 CIDR 新增到此設定。</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>如果您先前將 VPN Pod 部署限制為具有特定標籤的工作者，請確定新增的工作者節點也具有該標籤。</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>如果新增的工作者節點有污點，則變更此設定，以容許 VPN Pod 在具有任何污點或特定污點的所有有污點的工作者節點上執行。</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>新增的工作者節點可能部署在另一個新的專用子網路上，而不是其他工作者節點所在的現有子網路。如果您的應用程式是由專用網路上的 NodePort 或 LoadBalancer 服務公開，且應用程式位於新增的工作者節點上，請將新的子網路 CIDR 新增至此設定。**附註**：如果您將值新增至 `local.subnet`，請檢查內部部署子網路的 VPN 設定，以查看它們是否也必須進行更新。</td>
     </tr>
     </tbody></table>

    如果您已刪除工作者節點：

    <table>
    <caption>工作者節點設定</caption>
     <thead>
     <th>設定</th>
     <th>說明</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>如果是使用子網路 NAT 來重新對映特定專用本端 IP 位址，請從此設定中移除來自舊工作者節點的任何 IP 位址。如果是使用子網路 NAT 來重新對映整個子網路，並且某個子網路上沒有任何工作者節點存在，請從此設定中移除該子網路 CIDR。</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>如果您先前將 VPN Pod 部署限制為單一工作者節點，且已刪除該工作者節點，請變更此設定，以容許 VPN Pod 在其他工作者節點上執行。</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>如果您所刪除的工作者節點沒有污點，但唯一剩下的工作者節點有污點，請變更此設定，以容許 VPN Pod 在具有任何污點或特定污點的工作者節點上執行。
     </td>
     </tr>
     </tbody></table>

4. 安裝含有更新值的新 Helm 圖表。

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. 檢查圖表部署狀態。圖表就緒時，輸出頂端附近的 **STATUS** 欄位會具有 `DEPLOYED` 值。

    ```
    helm status <release_name>
    ```
    {: pre}

6. 在某些情況下，您可能需要變更內部部署設定及防火牆設定，以符合您對 VPN 配置檔所做的變更。

7. 啟動 VPN。
    * 如果由叢集起始 VPN 連線（`ipsec.auto` 設為 `start`），請在內部部署閘道上啟動 VPN，然後在叢集上啟動 VPN。
    * 如果由內部部署閘道起始 VPN 連線（`ipsec.auto` 設為 `auto`），請在叢集上啟動 VPN，然後在內部部署閘道上啟動 VPN。

8. 設定 `STRONGSWAN_POD` 環境變數。

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. 檢查 VPN 的狀態。

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * 如果 VPN 連線的狀態為 `ESTABLISHED`，則表示 VPN 連線成功。不需執行進一步的動作。

    * 如果您仍有連線問題，請參閱[無法使用 strongSwan Helm 圖表建立 VPN 連線功能](#cs_vpn_fails)，進一步對您的 VPN 連線進行疑難排解。

<br />



## 無法擷取 Calico 網路原則
{: #cs_calico_fails}

{: tsSymptoms}
當您執行 `calicoctl get policy` 嘗試檢視叢集裡的 Calico 網路原則時，會收到下列其中一個非預期的結果或錯誤訊息：
- 空清單
- 舊 Calico 第 2 版原則的清單，而非第 3 版原則
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`

當您執行 `calicoctl get GlobalNetworkPolicy` 嘗試檢視叢集裡的 Calico 網路原則時，會收到下列其中一個非預期的結果或錯誤訊息：
- 空清單
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'v1'`
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`
- `Failed to get resources: Resource type 'GlobalNetworkPolicy' is not supported`

{: tsCauses}
若要使用 Calico 原則，則必須全部符合四個因素：您叢集的 Kubernetes 版本、Calico CLI 版本、Calico 配置檔語法，以及檢視原則指令。其中有一個以上的因素的版本不正確。

{: tsResolve}
您必須使用 3.3 版或更新版本的 Calico CLI、`calicoctl.cfg` 第 3 版配置檔語法，以及 `calicoctl get GlobalNetworkPolicy` 和 `calicoctl get NetworkPolicy` 指令。

若要確保符合所有 Calico 因素，請執行下列動作：

1. [安裝及配置 3.3 版或更新版本的 Calico CLI](/docs/containers?topic=containers-network_policies#cli_install)。
2. 確定您建立且要套用至您叢集的任何原則都會使用 [Calico 第 3 版語法 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/networkpolicy)。如果您在 Calico 第 2 版語法中有現有原則 `.yaml` 或 `.json` 檔案，則可以使用 [`calicoctl convert` 指令 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.3/reference/calicoctl/commands/convert) 將它轉換為 Calico 第 3 版語法。
3. 若要[檢視原則](/docs/containers?topic=containers-network_policies#view_policies)，請確保對於廣域原則，使用 `calicoctl get GlobalNetworkPolicy`，對於範圍限定為特定名稱空間的原則，使用 `calicoctl get NetworkPolicy --namespace <policy_namespace>`。

<br />


## 由於 VLAN ID 無效，無法新增工作者節點
{: #suspended}

{: tsSymptoms}
您的 {{site.data.keyword.Bluemix_notm}} 帳戶已暫停，或叢集裡的所有工作者節點已遭刪除。在重新啟動帳戶之後，當您嘗試調整工作者節點儲存區的大小，或重新平衡工作者節點儲存區時，無法新增工作者節點。您會看到與下列內容類似的錯誤訊息：

```
SoftLayerAPIError(SoftLayer_Exception_Public): Could not obtain network VLAN with id #123456.
```
{: screen}

{: tsCauses}
當帳戶暫時時，即會刪除帳戶內的工作者節點。如果叢集沒有工作者節點，則 IBM Cloud 基礎架構 (SoftLayer) 會收回關聯的公用及專用 VLAN。不過，叢集工作者節點儲存區在其 meta 資料中仍有先前的 VLAN ID，並會在您重新平衡儲存區或調整儲存區的大小時，使用這些無法使用的 ID。節點無法建立，因為 VLAN 不再與叢集相關聯。

{: tsResolve}

您可以[刪除現有的工作者節點儲存區](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_rm)，然後[建立新的工作者節點儲存區](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create)。

或者，您也可以保留現有的工作者節點儲存區，方法為訂購新的 VLAN，並使用這些 VLAN，在儲存區中建立新的工作者節點。

開始之前：[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  若要取得您需要其新 VLAN ID 的區域，請記下下列指令輸出中的**位置**。**附註**：如果您的叢集是多區域，則您需要每一個區域的 VLAN ID。

    ```
    ibmcloud ks clusters
    ```
    {: pre}

2.  [與 {{site.data.keyword.Bluemix_notm}} 支援中心聯絡](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans)，為叢集所在的每一個區域取得新的專用及公用 VLAN。

3.  記下每一個區域的新專用及公用 VLAN ID。

4.  記下工作者節點儲存區的名稱。

    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

5.  使用 `zone-network-set` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set)，以變更工作者節點儲存區網路 meta 資料。

    ```
    ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> -- worker-pools <worker-pool> --private-vlan <private_vlan_ID> --public-vlan <public_vlan_ID>
    ```
    {: pre}

6.  **僅限多區域叢集**：針對叢集裡的每一個區域重複**步驟 5**。

7.  重新平衡工作者節點儲存區或調整其大小，以新增使用新的 VLAN ID 的工作者節點，例如：

    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <worker_pool> --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

8.  驗證已建立工作者節點。

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <worker_pool>
    ```
    {: pre}

<br />



## 取得協助及支援
{: #network_getting_help}

叢集仍有問題？
{: shortdesc}

-  在終端機中，有 `ibmcloud` CLI 及外掛程式的更新可用時，就會通知您。請務必保持最新的 CLI，讓您可以使用所有可用的指令及旗標。
-   若要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，請[檢查 {{site.data.keyword.Bluemix_notm}} 狀態頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/status?selected=status)。
-   將問題張貼到 [{{site.data.keyword.containerlong_notm}} Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com)。如果您的 {{site.data.keyword.Bluemix_notm}} 帳戶未使用 IBM ID，請[要求邀請](https://bxcs-slack-invite.mybluemix.net/)以加入此 Slack。
    {: tip}
-   檢閱討論區，以查看其他使用者是否發生過相同的問題。使用討論區提問時，請標記您的問題，以便 {{site.data.keyword.Bluemix_notm}} 開發團隊能看到它。
    -   如果您在使用 {{site.data.keyword.containerlong_notm}} 開發或部署叢集或應用程式時有技術方面的問題，請將問題張貼到 [Stack Overflow ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers)，並使用 `ibm-cloud`、`kubernetes` 及 `containers` 來標記問題。
    -   若為服務及開始使用指示的相關問題，請使用 [IBM Developer Answers ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 討論區。請包含 `ibm-cloud` 及 `containers` 標籤。如需使用討論區的詳細資料，請參閱[取得協助](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)。
-   開立案例，以與「IBM 支援中心」聯絡。若要瞭解如何開立 IBM 支援中心案例，或是瞭解支援層次與案例嚴重性，請參閱[與支援中心聯絡](/docs/get-support?topic=get-support-getting-customer-support)。當您報告問題時，請包含您的叢集 ID。若要取得叢集 ID，請執行 `ibmcloud ks clusters`。您也可以使用 [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)，來收集及匯出叢集裡的相關資訊，以與 IBM 支援中心共用。
{: tip}

