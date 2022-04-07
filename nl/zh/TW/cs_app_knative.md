---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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


# 使用 Knative 部署無伺服器應用程式
{: #serverless-apps-knative}

瞭解如何在 {{site.data.keyword.containerlong_notm}} 的 Kubernetes 叢集裡安裝和使用 Knative。
{: shortdesc}

**什麼是 Knative？為什麼要使用 Knative？**</br>
[Knative](https://github.com/knative/docs) 是一種開放程式碼平台，由 IBM、Google、Pivotal、Red Hat、Cisco 等公司聯合開發。目標是擴充 Kubernetes 的功能，以協助您在 Kubernetes 叢集上，建立以原始檔為主的現代容器化和無伺服器應用程式。此平台專門設計用來因應開發人員的需求，現今的開發人員必須決定他們要在雲端執行哪種類型的應用程式：12 因子應用程式、容器或函數。每種應用程式都需要一個針對自己量身自訂的開放程式碼或專有解決方案：對於 12 因子應用程式，需要 Cloud Foundry；對於容器，需要 Kubernetes；對於函數，需要 OpenWhisk 及其他解決方案。過去，開發人員必須決定他們想要遵循什麼方法，這導致在必須結合不同類型的應用程式時，不僅靈活度不夠，複雜性也較高。  

Knative 在程式設計語言及架構方面都採用一致的方法，將在 Kubernetes 中建置、部署及管理工作負載的作業負擔抽象化，以便開發人員能專注在對他們而言最重要的事情：原始碼。您可以使用您已經熟悉的已認證建置套件，例如 Cloud Foundry、Kaniko、Dockerfile、Bazel 等等。透過與 Istio 整合，Knative 確保可輕易監視、控制以及在網際網路上公開無伺服器及容器化工作負載，並確保您的資料在傳送期間都有加密。

**Knative 如何運作？**</br>
Knative 隨附三個關鍵元件（或_基本元素_），可協助您建置、部署及管理您 Kubernetes 叢集裡的無伺服器應用程式：

- **Build：**`Build` 基本元素支援建立一組步驟，以將應用程式從原始碼建置到容器映像檔。試想一下，您使用一個簡單的建置範本，在其中可指定用於尋找應用程式程式碼的來源儲存庫以及要在其中管理映像檔的容器登錄。只需使用一個指令，即可指示 Knative 採用此建置範本，取回原始碼，建立映像檔並將其推送到容器登錄，以便可以在容器中使用該映像檔。
- **Serving：**`Serving` 基本元素可協助您將無伺服器應用程式部署為 Knative 服務，並自動調整它們，甚至縮減到 0 個實例。為了公開無伺服器容器化工作負載，Knative 將使用 Istio。當您安裝受管理的 Knative 附加程式時，也會自動安裝受管理的 Istio 附加程式。
透過使用 Istio 的資料流量管理及智慧型遞送功能，您可以控制哪些資料流量遞送至服務的特定版本，方便開發人員測試及推出新的應用程式版本或執行 A-B 測試。
- **Eventing：**使用 `Eventing` 基本元素，您可以建立觸發程式或要讓其他服務訂閱的事件串流。例如，您可能想要在每次程式碼推送至您的 GitHub 主節點儲存庫時，開始應用程式的新建置。或者，您只想要在溫度低於冰點時，才執行無伺服器應用程式。例如，`Eventing` 基本元素可整合至您的 CI/CD 管線，以在發生特定事件時自動建置及部署應用程式。

**何謂 Managed Knative on {{site.data.keyword.containerlong_notm}}（實驗）附加程式？** </br>Managed Knative on {{site.data.keyword.containerlong_notm}} 是一個[受管理附加程式](/docs/containers?topic=containers-managed-addons#managed-addons)，用於將 Knative 和 Istio 直接與 Kubernetes 叢集整合。附加程式中的 Knative 和 Istio 版本已經過 IBM 測試，可在 {{site.data.keyword.containerlong_notm}} 中使用。如需受管理附加程式的相關資訊，請參閱[使用受管理附加程式新增服務](/docs/containers?topic=containers-managed-addons#managed-addons)。

**有任何限制嗎？** </br> 如果您在叢集裡已安裝 [container image security enforcer admission controller](/docs/services/Registry?topic=registry-security_enforce#security_enforce)，便無法在叢集裡啟用受管理的 Knative 附加程式。

## 在叢集裡設定 Knative
{: #knative-setup}

Knative 建置在 Istio 頂端，確保您的無伺服器及容器化工作負載可以在叢集內及網際網路上公開。使用 Istio，您也可以監視及控制您伺服器之間的網路資料流量，並確保您的資料在傳輸期間已加密。當您安裝受管理的 Knative 附加程式時，也會自動安裝受管理的 Istio 附加程式。
{: shortdesc}

開始之前：
-  [安裝 IBM Cloud CLI、{{site.data.keyword.containerlong_notm}} 外掛程式及 Kubernetes CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)。請務必安裝符合叢集之 Kubernetes 版本的 `kubectl` CLI 版本。
-  [建立具有至少 3 個工作者節點的標準叢集，每個節點有 4 個核心數和 16 GB 記憶體 (`b3c.4x16`) 或更高配置](/docs/containers?topic=containers-clusters#clusters_ui)。此外，叢集和工作者節點必須至少執行最低受支援的 Kubernetes 版本，您可以透過執行 `ibmcloud ks addon-versions --addon knative` 來檢查版本。
-  確定您具有 {{site.data.keyword.containerlong_notm}} 的[**撰寫者**或**管理員** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)。
-  [將 CLI 的目標設為叢集](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。
</br>

若要在叢集裡安裝 Knative，請執行下列動作：

1. 在叢集裡啟用受管理的 Knative 附加程式。在叢集裡啟用 Knative 時，會在叢集裡安裝 Istio 及所有 Knative 元件。
   ```
   ibmcloud ks cluster-addon-enable knative --cluster <cluster_name_or_ID> -y
   ```
   {: pre}

   輸出範例：
   ```
   Enabling add-on knative for cluster knative...
   OK
   ```
   {: screen}

   安裝所有 Knative 元件可能需要幾分鐘的時間才能完成。

2. 驗證是否已順利安裝 Istio。適用於 9 個 Istio 服務的所有 Pod，以及適用於 Prometheus 的 Pod 都必須處於 `Running` 狀態。
   ```
   kubectl get pods --namespace istio-system
   ```
   {: pre}

   輸出範例：
   ```
    NAME                                       READY     STATUS      RESTARTS   AGE
    istio-citadel-748d656b-pj9bw               1/1       Running     0          2m
    istio-egressgateway-6c65d7c98d-l54kg       1/1       Running     0          2m
    istio-galley-65cfbc6fd7-bpnqx              1/1       Running     0          2m
    istio-ingressgateway-f8dd85989-6w6nj       1/1       Running     0          2m
    istio-pilot-5fd885964b-l4df6               2/2       Running     0          2m
    istio-policy-56f4f4cbbd-2z2bk              2/2       Running     0          2m
    istio-sidecar-injector-646655c8cd-rwvsx    1/1       Running     0          2m
    istio-statsd-prom-bridge-7fdbbf769-8k42l   1/1       Running     0          2m
    istio-telemetry-8687d9d745-mwjbf           2/2       Running     0          2m
    prometheus-55c7c698d6-f4drj                1/1       Running     0          2m
    ```
   {: screen}

3. 選用項目：如果您要在 `default` 名稱空間中對所有應用程式使用 Istio，請將 `istio-injection=enabled` 標籤新增至名稱空間。每個無伺服器應用程式 Pod 都必須執行 Envoy Proxy Sidecar，才能將應用程式包含在 Istio 服務網中。此標籤容許 Istio 自動修改新應用程式部署中的 Pod 範本規格，以使用 Envoy Proxy Sidecar 容器來建立 Pod。
   ```
    kubectl label namespace default istio-injection=enabled
    ```
   {: pre}

4. 驗證 Knative `Serving` 元件的所有 Pod 都處於 `Running` 狀態。
   ```
      kubectl get pods --namespace knative-serving
      ```
   {: pre}

   輸出範例：
   ```
NAME                          READY     STATUS    RESTARTS   AGE
      activator-598b4b7787-ps7mj    2/2       Running   0          21m
      autoscaler-5cf5cfb4dc-mcc2l   2/2       Running   0          21m
      controller-7fc84c6584-qlzk2   1/1       Running   0          21m
      webhook-7797ffb6bf-wg46v      1/1       Running   0          21m
      ```
   {: screen}

## 使用 Knative 服務部署無伺服器應用程式
{: #knative-deploy-app}

在叢集裡設定 Knative 後，可以將無伺服器應用程式部署為 Knative 服務。
{: shortdesc}

**什麼是 Knative 服務？**</br>
若要使用 Knative 部署應用程式，必須指定 Knative `服務`資源。Knative 服務由 Knative `Serving` 基本元素所管理，且負責管理工作負載的整個生命週期。建立服務時，Knative `Serving` 基本元素會自動為無伺服器應用程式建立一個版本，並將此版本新增到服務的修訂歷程。會從 Ingress 子網域中指派一個公用 URL 給您的無伺服器應用程式，格式為 `<knative_service_name>.<namespace>.<ingress_subdomain>`，您可透過網際網路存取該應用程式。此外，專用主機名稱已指派給您的應用程式，格式為 `<knative_service_name>.<namespace>.cluster.local`，您可以用來從叢集內存取應用程式。

**建立 Knative 服務時，幕後會執行哪些操作？**</br>
建立 Knative 服務時，應用程式會自動部署為叢集裡的 Kubernetes Pod，並使用 Kubernetes 服務進行公開。為指派公用主機名稱，Knative 將使用 IBM 提供的 Ingress 子網域和 TLS 憑證。送入網路資料流量根據 IBM 提供的預設 Ingress 遞送規則進行遞送。

**如何應用新版本應用程式？**</br>
更新 Knative 服務時，會建立新版本的無伺服器應用程式。為此版本指派的公用和專用主機名稱與先前版本的相同。依預設，所有送入網路資料流量都會遞送到最新版本的應用程式。但是，您還可以指定想要遞送到特定應用程式版本的送入網路資料流量的百分比，以便可以執行 A-B 測試。可以同時在兩個應用程式版本（應用程式的現行版本和要前進至的新版本）之間分割送入網路資料流量。  

**可以使用自帶自訂網域和 TLS 憑證嗎？** </br>
可以變更 Istio Ingress 閘道的配置對映和 Ingress 遞送規則，以在為無伺服器應用程式指派主機名稱時使用自訂網域名稱和 TLS 憑證。如需相關資訊，請參閱[設定自訂網域名稱和憑證](#knative-custom-domain-tls)。

若要將無伺服器應用程式部署為 Knative 服務，請執行下列動作：

1. 在 Knative 中，使用 Go 為第一個無伺服器 [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) 應用程式建立 YAML 檔案。當您將要求傳送至您的範例應用程式時，應用程式會讀取環境變數 `TARGET`，並顯示出 `"Hello ${TARGET}!"`. 如果此環境變數是空的，則 `"Hello World!"` 會傳回。

   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Go Sample v1"
    ```
    {: codeblock}

    <table>
    <caption>瞭解 YAML 檔案元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>Knative 服務的名稱。</td>
    </tr>
    <tr>
    <td><code>metadata.namespace</td>
    <td>選用：要在其中將應用程式部署為 Knative 服務的 Kubernetes 名稱空間。依預設，所有服務都會部署到 <code>default</code> Kubernetes 名稱空間。</td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>在其中儲存映像檔之容器登錄的 URL。在此範例中，您部署 Knative Hello World 應用程式，其儲存在 Docker Hub 中的 <code>ibmcom</code> 名稱空間中。</td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>選用：想要 Knative 服務具有的環境變數的清單。在本範例中，範例應用程式會讀取環境變數 <code>TARGET</code> 的值，然後在您傳送要求至應用程式時傳回，格式為 <code>"Hello ${TARGET}!"</code>. 如果未提供任何值，則範例應用程式會傳回 <code>"Hello World!"</code>.  </td>
    </tr>
    </tbody>
    </table>

2. 在叢集裡建立 Knative 服務。
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   輸出範例：
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. 驗證 Knative 服務是否已建立。在 CLI 輸出中，可以檢視指派給無伺服器應用程式的公用 **DOMAIN**。**LATESTCREATED** 和 **LATESTREADY** 直欄顯示前次建立的應用程式版本和現行部署的版本，格式為 `<knative_service_name>-<version>`。指派給應用程式的版本是隨機字串值。在此範例中，無伺服器應用程式的版本為 `rjmwt`。更新服務時，會建立新版本的應用程式，並為該版本指派新的隨機字串。  
   ```
      kubectl get ksvc/kn-helloworld
      ```
   {: pre}

   輸出範例：
   ```
NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
      kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-rjmwt   kn-helloworld-rjmwt   True
      ```
   {: screen}

4. 透過向指派給應用程式的公用 URL 傳送要求，試用 `Hello World` 應用程式。
   ```
   curl -v <public_app_url>
   ```
   {: pre}

   輸出範例：
   ```
   * Rebuilt URL to: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud/
   *   Trying 169.46.XX.XX...
   * TCP_NODELAY set
   * Connected to kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud (169.46.XX.XX) port 80 (#0)
   > GET / HTTP/1.1
   > Host: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud
   > User-Agent: curl/7.54.0
   > Accept: */*
   >
   < HTTP/1.1 200 OK
      < Date: Thu, 21 Mar 2019 01:12:48 GMT
      < Content-Type: text/plain; charset=utf-8
      < Content-Length: 20
      < Connection: keep-alive
      < x-envoy-upstream-service-time: 17
      <
      Hello Go Sample v1!* Connection #0 to host kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud left intact
   ```
   {: screen}

5. 列出為 Knative 服務建立的 Pod 數。在本主題的範例中，部署了一個 Pod，其中包含兩個容器。一個容器執行 `Hello World` 應用程式，另一個容器則是執行 Istio 及 Knative 監視與記載工具的邊車容器。
   ```
   kubectl get pods
   ```
   {: pre}

   輸出範例：
   ```
   NAME                                             READY     STATUS    RESTARTS   AGE
   kn-helloworld-rjmwt-deployment-55db6bf4c5-2vftm  2/2      Running   0          16s
   ```
   {: screen}

6. 請等待幾分鐘，讓 Knative 縮減您的 Pod。Knative 會評估一次必須啟動多少數量的 Pod 才能處理送入的工作負載。如果沒有收到任何網路資料流量，則 Knative 會自動縮減您的 Pod，甚至到 0 個 Pod，如本範例所示。

   想要瞭解 Knative 如何擴增您的 Pod？嘗試增加應用程式的工作負載，例如，使用像[簡單雲端型負載測試器](https://loader.io/)之類的工具。
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   如果您沒有看到任何 `kn-helloworld` Pod，表示 Knative 將您的應用程式縮減到 0 個 Pod。

7. 更新您的 Knative 服務範例，並為 `TARGET` 環境變數輸入不同的值。

   範例服務 YAML：
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Mr. Smith"
    ```
   {: codeblock}

8. 將變更套用至您的服務。變更配置時，Knative 會自動建立新版本的應用程式。
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

9. 驗證是否部署了新版本的應用程式。在 CLI 輸出中，可以在 **LATESTCREATED** 直欄中看到應用程式的新版本。在 **LATESTREADY** 直欄中看到相同的應用程式版本時，說明應用程式已全部設定並已備妥，可在指派的公用 URL 上接收送入網路資料流量。
   ```
      kubectl get ksvc/kn-helloworld
      ```
   {: pre}

   輸出範例：
   ```
   NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
   kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-ghyei   kn-helloworld-ghyei   True
   ```
   {: screen}

9. 對您的應用程式提出新的要求，以驗證是否已套用您的變更。
   ```
   curl -v <service_domain>
   ```

   輸出範例：
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

10. 驗證 Knative 是否再次擴增您的 Pod，以因應增加的網路資料流量。
    ```
    kubectl get pods
    ```

    輸出範例：
    ```
    NAME                                              READY     STATUS    RESTARTS   AGE
    kn-helloworld-ghyei-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
    ```
    {: screen}

11. 選用項目：清除您的 Knative 服務。
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}


## 設定自訂網域名稱和憑證
{: #knative-custom-domain-tls}

您可以配置 Knative，以從配置為使用 TLS 的您自己的自訂網域中指派主機名稱。
{: shortdesc}

依預設，會從 Ingress 子網域指派公用子網域給每個應用程式，格式為 `<knative_service_name>.<namespace>.<ingress_subdomain>`，您可透過網際網路存取該應用程式。此外，專用主機名稱已指派給您的應用程式，格式為 `<knative_service_name>.<namespace>.cluster.local`，您可以用來從叢集內存取應用程式。如果要從您擁有的自訂網域指派主機名稱，則可以變更 Knative 配置對映以改為使用自訂網域。

1. 建立自訂網域。若要登錄自訂網域，請使用您的網域名稱服務 (DNS) 提供者或 [ DNS](/docs/infrastructure/dns?topic=dns-getting-started)。
2. 配置網域以將送入網路資料流量遞送到 IBM 提供的 Ingress 閘道。請從下列選項中進行選擇：
   - 將 IBM 提供的網域指定為「標準名稱記錄 (CNAME)」，以定義自訂網域的別名。若要尋找 IBM 提供的 Ingress 網域，請執行 `ibmcloud ks cluster-get --cluster <cluster_name>`，並尋找 **Ingress subdomain** 欄位。最好使用 CNAME，因為 IBM 會對 IBM 子網域提供自動性能檢查，並從 DNS 回應移除任何失敗的 IP。
   - 透過將 Ingress 閘道的可攜式公用 IP 位址新增為記錄，將自訂網域對映到該 IP 位址。若要尋找 Ingress 閘道的公用 IP 位址，請執行 `nslookup <ingress_subdomain>`。
3. 購買用於自訂網域的正式萬用字元 TLS 憑證。如果要購買多個 TLS 憑證，請確保每個憑證的 [CN ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://support.dnsimple.com/articles/what-is-common-name/) 都是不同的。
4. 建立用於憑證和秘密金鑰的 Kubernetes 密碼。
   1. 將憑證及金鑰編碼為 Base-64，並將 Base-64 編碼值儲存在新的檔案中。
      ```
      openssl base64 -in tls.key -out tls.key.base64
      ```
      {: pre}

   2. 檢視憑證及金鑰的 Base-64 編碼值。
      ```
      cat tls.key.base64
      ```
      {: pre}

   3. 使用憑證和密鑰建立密碼 YAML 檔案。
      ```
      apiVersion: v1
      kind: Secret
      metadata:
        name: mydomain-ssl
      type: Opaque
      data:
        tls.crt: <client_certificate>
        tls.key: <client_key>
      ```
      {: codeblock}

   4. 在叢集裡建立憑證。
      ```
      kubectl create -f secret.yaml
      ```
      {: pre}

5. 開啟叢集的 `istio-system` 名稱空間中的 `iks-knative-ingress` Ingress 資源，以開始對其進行編輯。
   ```
   kubectl edit ingress iks-knative-ingress -n istio-system
   ```
   {: pre}

6. 變更 Ingress 的預設遞送規則。
   - 將自訂萬用字元網域新增到 `spec.rules.host` 區段，以便將來自自訂網域和任何子網域的所有送入網路資料流量遞送到 `istio-ingressgateway`。
   - 配置自訂萬用字元網域的所有主機，以使用先前在 `spec.tls.hosts` 區段中建立的 TLS 密碼。

   範例 Ingress：
   ```
   apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
     name: iks-knative-ingress
     namespace: istio-system
   spec:
     rules:
     - host: '*.mydomain'
       http:
         paths:
         - backend:
             serviceName: istio-ingressgateway
             servicePort: 80
           path: /
     tls:
     - hosts:
       - '*.mydomain'
       secretName: mydomain-ssl
   ```
   {: codeblock}

   `spec.rules.host` 和 `spec.tls.hosts` 區段是清單，可以包含多個自訂網域和 TLS 憑證。
   {: tip}

7. 修改 Knative `config-domain` 配置對映，以使用自訂網域將主機名稱指派給新的 Knative 服務。
   1. 開啟 `config-domain` 配置對映以開始對其進行編輯。
      ```
      kubectl edit configmap config-domain -n knative-serving
      ```
      {: pre}

   2. 在配置對映的 `data` 區段中指定自訂網域，並移除為叢集設定的預設網域。
      - **從自訂網域中為所有 Knative 服務指派主機名稱的範例**：
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        透過將 `""` 新增到自訂網域，會從自訂網域中為建立的所有 Knative 服務指派主機名稱。  

      - **從自訂網域中為所選 Knative 服務指派主機名稱的範例**：
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: |
            selector:
              app: sample
          mycluster.us-south.containers.appdomain.cloud: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        若要從自訂網域中僅為所選 Knative 服務指派主機名稱，請將 `data.selector` 標籤鍵和值新增到配置對映。在此範例中，將從自訂網域中為具有標籤 `app: sample` 的所有服務指派主機名稱。確保還具有要指派給沒有 `app: sample` 標籤的其他所有應用程式的網域名稱。在此範例中，使用 IBM 提供的預設網域 `mycluster.us-south.containers.appdomain.cloud`。
    3. 儲存變更。

Ingress 遞送規則和 Knative 配置對映全部設定後，可以使用自訂網域和 TLS 憑證來建立 Knative 服務。

## 從一個 Knative 服務存取另一個 Knative 服務
{: #knative-access-service}

可以透過對指派給 Knative 服務的 URL 進行 REST API 呼叫，從其他 Knative 服務存取您的 Knative 服務。
{: shortdesc}

1. 列出叢集裡的所有 Knative 服務。
   ```
   kubectl get ksvc --all-namespaces
   ```
   {: pre}

2. 擷取指派給 Knative 服務的 **DOMAIN**。
   ```
   kubect get ksvc/<knative_service_name>
   ```
   {: pre}

   輸出範例：
   ```
   NAME        DOMAIN                                                            LATESTCREATED     LATESTREADY       READY   REASON
   myservice   myservice.default.mycluster.us-south.containers.appdomain.cloud   myservice-rjmwt   myservice-rjmwt   True
   ```
   {: screen}

3. 使用網域名稱來實作 REST API 呼叫以存取 Knative 服務。此 REST API 呼叫必須是為其建立 Knative 服務的應用程式的一部分。如果要存取的 Knative 服務指派有本端 URL（格式為 `<service_name>.<namespace>.svc.cluster.local`），則 Knative 會在叢集內部網路中保留 REST API 要求。

   使用 Go 撰寫的程式碼範例 Snippet：
   ```go
   resp, err := http.Get("<knative_service_domain_name>")
   if err != nil {
       fmt.Fprintf(os.Stderr, "Error: %s\n", err)
   } else if resp.Body != nil {
       body, _ := ioutil.ReadAll(resp.Body)
       fmt.Printf("Response: %s\n", string(body))
   }
   ```
   {: codeblock}

## 常用 Knative 服務設定
{: #knative-service-settings}

檢閱常用 Knative 服務設定，在開發無伺服器應用程式時，您可能會發現這些設定非常有用。
{: shortdesc}

- [設定最小和最大 Pod 數](#knative-min-max-pods)
- [指定每個 Pod 的最大要求數](#max-request-per-pod)
- [建立僅限專用無伺服器應用程式](#knative-private-only)
- [強制 Knative 服務重新取回容器映像檔](#knative-repull-image)

### 設定最小和最大 Pod 數
{: #knative-min-max-pods}

可以使用註釋指定要為應用程式執行的最小和最大 Pod 數。例如，如果您不想要 Knative 將應用程式縮減為零個實例，請將最小 Pod 數設定為 1。
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: ...
spec:
  runLatest:
    configuration:
      revisionTemplate:
        metadata:
          annotations:
            autoscaling.knative.dev/minScale: "1"
            autoscaling.knative.dev/maxScale: "100"
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>瞭解 YAML 檔案元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
</thead>
<tbody>
<tr>
<td><code>autoscaling.knative.dev/minScale</code></td>
<td>輸入要在叢集裡執行的最小 Pod 數。Knative 無法將應用程式縮減為低於設定的數目，即使應用程式未收到任何網路資料流量時也是如此。預設 Pod 數為零。</td>
</tr>
<tr>
<td><code>autoscaling.knative.dev/maxScale</code></td>
<td>輸入要在叢集裡執行的最大 Pod 數。Knative 無法將應用程式擴增為高於設定的數目，即使您有現行應用程式實例可以處理的更多要求。</td>
</tr>
</tbody>
</table>

### 指定每個 Pod 的最大要求數
{: #max-request-per-pod}

可以指定在 Knative 考慮擴增應用程式實例之前，應用程式實例可以接收和處理的最大要求數。例如，如果將最大要求數設定為 1，則應用程式實例一次可以接收一個要求。如果第二個要求在第一個要求完全處理之前到達，則 Knative 會擴增一個實例。

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image: myimage
          containerConcurrency: 1
....
```
{: codeblock}

<table>
<caption>瞭解 YAML 檔案元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
</thead>
<tbody>
<tr>
<td><code>containerConcurrency</code></td>
<td>輸入在 Knative 考慮擴增應用程式實例之前，應用程式實例一次可以接收的最大要求數。</td>
</tr>
</tbody>
</table>

### 建立僅限專用無伺服器應用程式
{: #knative-private-only}

依預設，會從 Istio Ingress 子網域中為每個 Knative 服務指派公用路徑，另外還會指派專用路徑，格式為 `<service_name>.<namespace>.cluster.local`。可以使用公用路徑透過公用網路來存取應用程式。如果要使服務保持專用，可以將 `serving.knative.dev/visibility` 標籤新增到 Knative 服務。此標籤指示 Knative 僅將專用主機名稱指派給您的服務。
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
  labels:
    serving.knative.dev/visibility: cluster-local
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>瞭解 YAML 檔案元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
</thead>
<tbody>
<tr>
<td><code>serving.knative.dev/visibility</code></td>
  <td>如果新增 <code>serving.knative.dev/visibility: cluster-local</code> 標籤，則將僅為服務指派格式為 <code>&lt;service_name&gt;.&lt;namespace&gt;.cluster.local</code> 的專用路徑。可以使用專用主機名稱從叢集內存取服務，但無法透過公用網路存取服務。</td>
</tr>
</tbody>
</table>

### 強制 Knative 服務重新取回容器映像檔
{: #knative-repull-image}

Knative 的現行實作未提供強制 Knative `Serving` 元件重新取回容器映像檔的標準方法。若要從登錄中重新取回映像檔，請在下列選項之間進行選擇：

- **修改 Knative 服務 `revisionTemplate`**：Knative 服務的 `revisionTemplate` 用於建立 Knative 服務的修訂。如果修改了此修訂範本，例如新增 `repullFlag` 註釋，則 Knative 必須為應用程式建立新的修訂。在建立修訂的過程中，Knative 必須檢查容器映像檔更新。設定 `imagePullPolicy: Always` 時，Knative 無法在叢集裡使用映像檔快取，而是必須從容器登錄中取回映像檔。
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: <service_name>
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           metadata:
             annotations:
               repullFlag: 123
           spec:
             container:
               image: <image_name>
               imagePullPolicy: Always
    ```
    {: codeblock}

    每次要建立服務的新修訂時，都必須變更 `repulFlag` 值，以從容器登錄中取回最新的映像檔版本。請確保對每個修訂使用唯一值，以避免由於兩個完全相同的 Knative 服務配置而導致 Knative 使用舊映像檔版本。  
    {: note}

- **使用標籤來建立唯一容器映像檔**：可以對建立的每個容器映像檔使用唯一標籤，並在 Knative 服務 `container.image` 配置中參照此映像檔。在下列範例中，`v1` 用作映像檔標籤。若要強制 Knative 從容器登錄中取回新映像檔，必須變更映像檔標籤。例如，使用 `v2` 作為新的映像檔標籤。
  ```
  apiVersion: serving.knative.dev/v1alpha1
  kind: Service
  metadata:
    name: <service_name>
  spec:
    runLatest:
      configuration:
          spec:
            container:
              image: myapp:v1
              imagePullPolicy: Always
    ```
    {: codeblock}


## 相關鏈結  
{: #knative-related-links}

- 請試用此 [Knative 研討會 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/IBM/knative101/tree/master/workshop)，將您的第一個 `Node.js` fibonacci 應用程式部署至您的叢集。
  - 探索如何使用 Knative `Build` 基本元素，從 GitHub 中的 Dockerfile 建置映像檔，並自動將映像檔推送至 {{site.data.keyword.registrylong_notm}} 中的名稱空間。  
  - 瞭解如何將網路資料流量的遞送從 IBM 提供的 Ingress 子網域設定為 Knative 所提供的 Istio Ingress 閘道。
  - 推出應用程式的新版本，並使用 Istio 來控制遞送至每個應用程式版本的資料流量數量。
- 探索 [Knative `Eventing` ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/knative/docs/tree/master/eventing/samples) 範例。
- 使用 [Knative 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/knative/docs) 進一步瞭解 Knative。
