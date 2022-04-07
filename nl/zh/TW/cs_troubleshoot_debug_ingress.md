---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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
{:preview: .preview}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# 除錯 Ingress
{: #cs_troubleshoot_debug_ingress}

在您使用 {{site.data.keyword.containerlong}} 時，請考慮使用這些技術來進行一般 Ingress 疑難排解及除錯。
{: shortdesc}

您已透過在叢集裡建立應用程式的 Ingress 資源，來公開應用程式。不過，當您嘗試透過 ALB 的公用 IP 位址或子網域連接至應用程式時，連線會失敗或逾時。下列各節中的步驟可協助您除錯 Ingress 設定。

確定您只在一個 Ingress 資源中定義主機。如果在多個 Ingress 資源中定義一個主機，則 ALB 可能不會適當地轉遞資料流量，而且您可能會遭遇錯誤。
{: tip}

開始之前，請確定您具有下列 [{{site.data.keyword.Bluemix_notm}} IAM 存取原則](/docs/containers?topic=containers-users#platform)：
  - 叢集的**編輯者**或**管理者**平台角色
  - **撰寫者**或**管理員**服務角色

## 步驟 1：在 {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool 中執行 Ingress 測試

疑難排解時，您可以使用 {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool 來執行 Ingress 測試，並從叢集收集相關的 Ingress 資訊。若要使用除錯工具，請安裝 [`ibmcloud-iks-debug` Helm 圖表 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/kubernetes/helm/iks-charts/ibmcloud-iks-debug)：
{: shortdesc}


1. [在叢集裡設定 Helm，建立 Tiller 的服務帳戶，並將 `ibm` 儲存庫新增至 Helm 實例](/docs/containers?topic=containers-helm)。

2. 將 Helm 圖表安裝至您的叢集。
        
  ```
  helm install iks-charts/ibmcloud-iks-debug --name debug-tool
  ```
  {: pre}


3. 啟動 Proxy 伺服器以顯示除錯工具介面。
  ```
  kubectl proxy --port 8080
  ```
  {: pre}

4. 在 Web 瀏覽器中，開啟除錯工具介面 URL： http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page

5. 選取測試的 **ingress** 群組。有些測試會檢查潛在警告、錯誤或問題，有些測試僅收集您在疑難排解時可以參照的資訊。如需每個測試的功能相關資訊，請按一下測試名稱旁邊的資訊圖示。

6. 按一下**執行**。

7. 檢查每個測試的結果。
  * 如果有任何測試失敗，請按一下左手邊直欄中測試名稱旁的資訊圖示，以取得如何解決問題的相關資訊。
  * 您也可以使用測試結果，其只在您於下列各節中對 Ingress 服務進行除錯時收集資訊。

## 步驟 2：檢查 Ingress 部署及 ALB Pod 日誌中的錯誤訊息
{: #errors}

從檢查 Ingress 資源部署事件及 ALB Pod 日誌中的錯誤訊息開始。這些錯誤訊息可協助您找到失敗的主要原因，並在下列各節進一步除錯 Ingress 設定。
{: shortdesc}

1. 檢查 Ingress 資源部署，並尋找警告或錯誤訊息。
    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    在輸出的 **Events** 區段中，您可能會看到關於您使用之 Ingress 資源或某些註釋中含有無效值的警告訊息。請檢查 [Ingress 資源配置文件](/docs/containers?topic=containers-ingress#public_inside_4)或[註釋文件](/docs/containers?topic=containers-ingress_annotation)。

    ```
        Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends      ----                                             ----  --------
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
2. 檢查 ALB Pod 的狀態。
    1. 取得在叢集裡執行的 ALB Pod。
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. 檢查 **STATUS** 直欄，確定所有 Pod 都正在執行。

    3. 如果 Pod 不是 `Running`，則您可以停用及重新啟用 ALB。在下列指令中，將 `<ALB_ID>` 取代為 Pod 之 ALB 的 ID。例如，如果未執行的 Pod 具有名稱 `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1-5d6d86fbbc-kxj6z`，則 ALB ID 為 `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1`。
        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --disable
        ```
        {: pre}

        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --enable
        ```
        {: pre}

3. 檢查 ALB 的日誌。
    1.  取得在叢集裡執行的 ALB Pod ID。
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    3. 取得每個 ALB Pod 上 `nginx-ingress` 容器的日誌。
        ```
        kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

    4. 尋找 ALB 日誌中的錯誤訊息。

## 步驟 3：連線測試 ALB 子網域及公用 IP 位址
{: #ping}

請檢查 Ingress 子網域及 ALB 之公用 IP 位址的可用性。
{: shortdesc}

1. 取得公用 ALB 所接聽的 IP 位址。
    ```
    ibmcloud ks albs --cluster <cluster_name_or_ID>
    ```
    {: pre}

    `dal10` 及 `dal13` 中具有工作者節點之多區域叢集的範例輸出：

    ```
    ALB ID                                            Enabled   Status     Type      ALB IP          Zone    Build                          ALB VLAN ID
    private-cr24a9f2caf6554648836337d240064935-alb1   false     disabled   private   -               dal13   ingress:411/ingress-auth:315   2294021
    private-cr24a9f2caf6554648836337d240064935-alb2   false     disabled   private   -               dal10   ingress:411/ingress-auth:315   2234947
    public-cr24a9f2caf6554648836337d240064935-alb1    true      enabled    public    169.62.196.238  dal13   ingress:411/ingress-auth:315   2294019
    public-cr24a9f2caf6554648836337d240064935-alb2    true      enabled    public    169.46.52.222   dal10   ingress:411/ingress-auth:315   2234945
    ```
    {: screen}

    * 如果公用 ALB 沒有 IP 位址，請參閱 [Ingress ALB 未在區域中部署](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit)。

2. 檢查 ALB IP 的性能。

    * 對於單一區域叢集及多區域叢集：連線測試每個公用 ALB 的 IP 位址，以確保每個 ALB 都能夠順利接收封包。如果您使用專用 ALB，則只能從專用網路連線測試其 IP 位址。
        ```
        ping <ALB_IP>
        ```
        {: pre}

        * 如果 CLI 傳回逾時，而且您有保護工作者節點的自訂防火牆，請確定[防火牆](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall)中容許 ICMP。
        * 如果沒有任何防火牆封鎖連線測試，但連線測試仍然執行逾時，則請[檢查 ALB Pod 的狀態](#check_pods)。

    * 僅限多區域叢集：您可以使用 MZLB 性能檢查來判斷您 ALB IP 的狀態。如需 MZLB 的相關資訊，請參閱[多區域負載平衡器 (MZLB)](/docs/containers?topic=containers-ingress#planning)。MZLB 性能檢查僅適用於格式為 `<cluster_name>.<region_or_zone>.containers.appdomain.cloud` 之新 Ingress 子網域的叢集。如果您的叢集仍然使用舊格式 `<cluster_name>.<region>.containers.mybluemix.net`，則請[將單一區域叢集轉換成多區域](/docs/containers?topic=containers-add_workers#add_zone)。您的叢集獲指派具有新格式的子網域，但也可以繼續使用較舊的子網域格式。或者，您可以訂購自動獲指派新子網域格式的新叢集。

    下列 HTTP cURL 指令使用 `albhealth` 主機，其由 {{site.data.keyword.containerlong_notm}} 配置成傳回 ALB IP 的 `healthy` 或 `unhealthy` 狀態。
        ```
                curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
        ```
        {: pre}

        輸出範例：
        ```
        healthy
        ```
        {: screen}
            如果有一個以上的 IP 傳回 `unhealthy`，則請[檢查 ALB Pod 的狀態](#check_pods)。

3. 取得 IBM 提供的 Ingress 子網域。
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress
    ```
    {: pre}

    輸出範例：
    ```
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    ```
    {: screen}

4. 確定已向叢集的 IBM 所提供 Ingress 子網域，登錄您在本節步驟 2 取得之每個公用 ALB 的 IP。例如，在多區域叢集裡，您必須在相同主機名稱下登錄具有工作者節點之每個區域中的公用 ALB IP。

    ```
    kubectl get ingress -o wide
    ```
    {: pre}

    輸出範例：
        ```
    NAME                HOSTS                                                    ADDRESS                        PORTS     AGE
    myingressresource   mycluster-12345.us-south.containers.appdomain.cloud      169.46.52.222,169.62.196.238   80        1h
    ```
    {: screen}

## 步驟 4：檢查網域對映及 Ingress 資源配置
{: #ts_ingress_config}

1. 如果您使用自訂網域，請驗證您已使用 DNS 提供者將自訂網域對映至 IBM 提供的子網域或 ALB 的公用 IP 位址。請注意，最好使用 CNAME，因為 IBM 會在 IBM 子網域上提供自動性能檢查，並從 DNS 回應移除任何失敗的 IP。
    * IBM 提供的子網域：確認已將自訂網域對映至「標準名稱記錄 (CNAME)」中叢集的 IBM 所提供子網域。
        ```
        host www.my-domain.com
        ```
        {: pre}

        輸出範例：
        ```
        www.my-domain.com is an alias for mycluster-12345.us-south.containers.appdomain.cloud
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
        ```
        {: screen}

    * 公用 IP 位址：確認已將自訂網域對映至 A 記錄中 ALB 的可攜式公用 IP 位址。IP 應該符合您在[前一節](#ping)步驟 1 中所取得的公用 ALB IP。
        ```
        host www.my-domain.com
        ```
        {: pre}

        輸出範例：
    ```
        www.my-domain.com has address 169.46.52.222
        www.my-domain.com has address 169.62.196.238
        ```
        {: screen}

2. 檢查叢集的 Ingress 資源配置檔。
    ```
    kubectl get ingress -o yaml
    ```
    {: pre}

    1. 確定您只在一個 Ingress 資源中定義主機。如果在多個 Ingress 資源中定義一個主機，則 ALB 可能不會適當地轉遞資料流量，而且您可能會遭遇錯誤。

    2. 確認子網域及 TLS 憑證正確無誤。若要尋找 IBM 提供的 Ingress 子網域及 TLS 憑證，請執行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`。

    3.  確定應用程式接聽與 Ingress 之 **path** 區段中配置相同的路徑。如果您的應用程式設定成接聽根路徑，請使用 `/` 作為路徑。如果必須將此路徑的送入資料流量遞送至應用程式所接聽的不同路徑，請使用 [rewrite paths](/docs/containers?topic=containers-ingress_annotation#rewrite-path) 註釋。

    4. 視需要編輯資源配置 YAML。當您關閉編輯器時，即會儲存並自動套用您的變更。
        ```
        kubectl edit ingress <myingressresource>
        ```
        {: pre}

## 從 DNS 移除 ALB 以進行除錯
{: #one_alb}

如果您無法透過特定的 ALB IP 來存取應用程式，則可以停用其 DNS 登錄，以從正式作業暫時移除 ALB。然後，您可以使用 ALB 的 IP 位址，對該 ALB 執行除錯測試。

例如，假設您在 2 個區域中有某個多區域叢集，而且 2 個公用 ALB 都有 IP 位址 `169.46.52.222` 及 `169.62.196.238`。雖然性能檢查針對第二個區域的 ALB 傳回性能良好，但是無法直接透過它連接您的應用程式。您決定從正式作業移除該 ALB 的 IP 位址 `169.62.196.238` 以進行除錯。第一個區域的 ALB IP `169.46.52.222` 會向您的網域登錄，並在除錯第二個區域的 ALB 時繼續遞送資料流量。

1. 取得具有無法連接之 IP 位址的 ALB 名稱。
    ```
    ibmcloud ks albs --cluster <cluster_name> | grep <ALB_IP>
    ```
    {: pre}

    例如，無法連接的 IP `169.62.196.238` 屬於 ALB `public-cr24a9f2caf6554648836337d240064935-alb1`：
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
    public-cr24a9f2caf6554648836337d240064935-alb1    false     disabled   private   169.62.196.238   dal13   ingress:411/ingress-auth:315   2294021
    ```
    {: screen}

2. 使用前一個步驟中的 ALB 名稱，取得 ALB Pod 的名稱。下列指令使用前一個步驟中的範例 ALB 名稱：
    ```
    kubectl get pods -n kube-system | grep public-cr24a9f2caf6554648836337d240064935-alb1
    ```
    {: pre}

    輸出範例：
        ```
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq   2/2       Running   0          24m
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-trqxc   2/2       Running   0          24m
    ```
    {: screen}

3. 停用針對所有 ALB Pod 執行的性能檢查。針對您在前一個步驟中取得的每個 ALB Pod，重複這些步驟。這些步驟中的指令範例及輸出會使用第一個 Pod `public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq`。
    1. 登入 ALB Pod，並檢查 NGINX 配置檔中的 `server_name` 行。
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        確認 ALB Pod 的輸出範例已配置正確的性能檢查主機名稱 `albhealth.<domain>`：
        ```
        server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud;
        ```
        {: screen}

    2. 若要停用性能檢查來移除 IP，請在 `server_name` 前面插入 `#`。停用 ALB 的 `albhealth.mycluster-12345.us-south.containers.appdomain.cloud` 虛擬主機時，自動化性能檢查會從 DNS 回應中自動移除 IP。
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- sed -i -e 's*server_name*#server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

    3. 驗證已套用變更。
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        輸出範例：
        ```
        #server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud
        ```
        {: screen}

    4. 若要從 DNS 登錄移除 IP，請重新載入 NGINX 配置。
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- nginx -s reload
        ```
        {: pre}

    5. 針對每個 ALB Pod，重複這些步驟。

4. 現在，當您嘗試對 `albhealth` 主機進行 cURL 處理來執行 ALB IP 的性能檢查時，檢查會失敗。
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

    輸出：
    ```
    <html>
        <head>
            <title>404 找不到</title>
        </head>
        <body bgcolor="white"><center>
            <h1>404 找不到</h1>
        </body>
    </html>
    ```
    {: screen}

5. 檢查 Cloudflare 伺服器，以驗證已從網域的 DNS 登錄移除 ALB IP 位址。請注意，DNS 登錄可能需要幾分鐘的時間才能更新。
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    確認只有性能良好的 ALB IP `169.46.52.222` 保留在 DNS 登錄中，並已移除性能不佳的 ALB IP `169.62.196.238` 的範例輸出：
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    ```
    {: screen}

6. 現在，已從正式作業移除 ALB IP，您可以透過它對應用程式執行除錯測試。若要透過此 IP 來測試與應用程式的通訊，您可以執行下列 cURL 指令，並將範例值取代為您自己的值：
    ```
    curl -X GET --resolve mycluster-12345.us-south.containers.appdomain.cloud:443:169.62.196.238 https://mycluster-12345.us-south.containers.appdomain.cloud/
    ```
    {: pre}

    * 如果已正確配置所有項目，則會從應用程式取得預期回應。
    * 如果您在回應中收到錯誤，則您的應用程式或只套用至此特定 ALB 的配置中可能發生錯誤。請檢查您的應用程式碼、[Ingress 資源配置檔](/docs/containers?topic=containers-ingress#public_inside_4)，或您只套用至此 ALB 的任何其他配置。

7. 完成除錯之後，請在 ALB Pod 上還原性能檢查。針對每個 ALB Pod，重複這些步驟。
  1. 登入 ALB Pod，並移除 `server_name` 中的 `#`。
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- sed -i -e 's*#server_name*server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
    ```
    {: pre}

  2. 重新載入 NGINX 配置，以套用性能檢查還原。
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- nginx -s reload
    ```
    {: pre}

9. 現在，當您對 `albhealth` 主機進行 cURL 處理來執行 ALB IP 的性能檢查時，檢查會傳回 `healthy`。
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

10. 檢查 Cloudflare 伺服器，以驗證已在網域的 DNS 登錄中還原 ALB IP 位址。請注意，DNS 登錄可能需要幾分鐘的時間才能更新。
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    輸出範例：
        ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
    ```
    {: screen}

<br />


## 取得協助及支援
{: #ingress_getting_help}

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

