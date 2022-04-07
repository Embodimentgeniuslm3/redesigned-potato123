---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

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


# 指導教學：使用受管理的 Knative 在 Kubernetes 叢集中執行無伺服器應用程式
{: #knative_tutorial}

使用此指導教學，您可以瞭解如何在 {{site.data.keyword.containerlong_notm}} 的 Kubernetes 叢集中安裝 Knative。
{: shortdesc}

**何謂 Knative 及使用原因為何？**</br>
[Knative](https://github.com/knative/docs) 是 IBM、Google、Pivotal、Red Hat、Cisco 及其他人為了擴充 Kubernetes 的功能而開發的開放程式碼平台，以協助您在 Kubernetes 叢集上，建立現代、以程式碼為中心的容器化及無伺服器應用程式。平台專門設計用來因應開發人員的需求，他們今日必須決定要在雲端中執行哪種類型的應用程式：12 因素應用程式、容器或功能。每一種類型的應用程式都需要有針對這些應用程式自訂的開放程式碼或專有的解決方案：Cloud Foundry 適用於 12 因素應用程式、Kubernetes 適用於容器以及 OpenWhisk 和其他則適用於功能。過去，開發人員必須決定他們想要遵循什麼方法，導致在必須結合不同類型的應用程式時，不僅靈活度不夠，複雜性也較高。  

Knative 在程式設計語言及架構方面都採用一致的方法，來簡化建置、部署及管理 Kubernetes 中工作負載的作業負擔，以便開發人員專注在對他們而言最重要的事情上：原始碼。您可以使用已熟悉的已認證建置套件，例如，Cloud Foundry、Kaniko、Dockerfile、Bazel 及其他。透過與 Istio 整合，Knative 確保可輕易監視、控制以及在網際網路上公開無伺服器及容器化工作負載，並確保您的資料在傳送期間都有加密。

**Knative 如何運作？**</br>
Knative 隨附 3 個關鍵元件（或_基本元素_），可協助您建置、部署及管理您 Kubernetes 叢集中的無伺服器應用程式：

- **Build：**`Build` 基本元素支援建立一組步驟，以將應用程式從原始碼建置到容器映像檔。假設使用一個簡單的建置範本，您在其中指定原始檔儲存庫，以尋找您的應用程式碼以及要在其中管理映像檔的容器登錄。只要一個指令，便可指示 Knative 採用此建置範本、取回原始碼、建立映像檔，以及將映像檔推送至您的容器登錄，以便您能夠在您的容器中使用該映像檔。
- **Serving：**`Serving` 基本元素可協助您將無伺服器應用程式部署為 Knative 服務，並自動調整它們，甚至縮減到 0 個實例。透過使用 Istio 的資料流量管理及智慧型遞送功能，您可以控制哪些資料流量遞送至服務的特定版本，方便開發人員測試及推出新的應用程式版本或執行 A-B 測試。
- **Eventing：**使用 `Eventing` 基本元素，您可以建立觸發程式或要讓其他服務訂閱的事件串流。例如，您可能想要在每次程式碼推送至您的 GitHub 主節點儲存庫時，開始應用程式的新建置。或者，您只想要在溫度低於冰點時，才執行無伺服器應用程式。`Eventing` 基本元素可整合至您的 CI/CD 管線，以在發生特定事件時自動建置及部署應用程式。

**何謂 Managed Knative on {{site.data.keyword.containerlong_notm}}（實驗）附加程式？** </br>Managed Knative on {{site.data.keyword.containerlong_notm}} 是受管理的附加程式，可將 Knative 及 Istio 直接與您的 Kubernetes 叢集整合。附加程式中的 Knative 及 Istio 版本由 IBM 進行測試，並支援在 {{site.data.keyword.containerlong_notm}} 中使用。如需受管理附加程式的相關資訊，請參閱[使用受管理附加程式新增服務](/docs/containers?topic=containers-managed-addons#managed-addons)。

**有任何限制嗎？** </br> 如果您在叢集中已安裝 [container image security enforcer admission controller](/docs/services/Registry?topic=registry-security_enforce#security_enforce)，便無法在叢集中啟用受管理的 Knative 附加程式。

聽起來不錯？請遵循本指導教學，開始在 {{site.data.keyword.containerlong_notm}} 中使用 Knative。

## 目標
{: #knative_objectives}

- 瞭解 Knative 及 Knative 基本元素的基本觀念。  
- 在叢集中安裝受管理的 Knative 及受管理的 Istio 附加程式。
- 使用 Knative `Serving` 基本元素，部署第一個使用 Knative 的無伺服器應用程式，並將該應用程式公開在網際網路上。
- 探索 Knative 調整和修訂功能。

## 所需時間
{: #knative_time}

30 分鐘

## 適用對象
{: #knative_audience}

本指導教學是為了開發人員（有興趣學習如何使用 Knative 在 Kubernetes 叢集中部署無伺服器應用程式）以及叢集管理者（想要學習如何在叢集中設定 Knative）而設計。

## 必要條件
{: #knative_prerequisites}

-  [安裝 IBM Cloud CLI、{{site.data.keyword.containerlong_notm}} 外掛程式及 Kubernetes CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps)。請務必安裝符合叢集之 Kubernetes 版本的 `kubectl` CLI 版本。
-  [建立具有至少 3 個工作者節點的叢集，每個工作者節點具有 4 個核心及 16 GB 記憶體 (`b3c.4x16`) 或更多](/docs/containers?topic=containers-clusters#clusters_cli)。每個工作者節點都必須執行 Kubernetes 1.12 版或更新版本。
-  請確定您具有 {{site.data.keyword.containerlong_notm}} 的[**撰寫者**或**管理員** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)。
-  [將 CLI 的目標設為叢集](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

## 課程 1：設定受管理的 Knative 附加程式
{: #knative_setup}

Knative 建置在 Istio 頂端，確保您的無伺服器及容器化工作負載可以在叢集內及網際網路上公開。使用 Istio，您也可以監視及控制您伺服器之間的網路資料流量，並確保您的資料在傳輸期間已加密。當您安裝受管理的 Knative 附加程式時，也會自動安裝受管理的 Istio 附加程式。
{: shortdesc}

1. 在叢集中啟用受管理的 Knative 附加程式。在叢集中啟用 Knative 時，會在叢集中安裝 Istio 及所有 Knative 元件。
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

4. 驗證是否已順利安裝所有 Knative 元件。
   1. 驗證 Knative `Serving` 元件的所有 Pod 都處於 `Running` 狀態。  
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

   2. 驗證 Knative `Build` 元件的所有 Pod 都處於 `Running` 狀態。  
      ```
      kubectl get pods --namespace knative-build
      ```
      {: pre}

      輸出範例：
      ```
      NAME                                READY     STATUS    RESTARTS   AGE
      build-controller-79cb969d89-kdn2b   1/1       Running   0          21m
      build-webhook-58d685fc58-twwc4      1/1       Running   0          21m
      ```
      {: screen}

   3. 驗證 Knative `Eventing` 元件的所有 Pod 都處於 `Running` 狀態。
      ```
      kubectl get pods --namespace knative-eventing
      ```
      {: pre}

      輸出範例：

      ```
      NAME                                            READY     STATUS    RESTARTS   AGE
      eventing-controller-847d8cf969-kxjtm            1/1       Running   0          22m
      in-memory-channel-controller-59dd7cfb5b-846mn   1/1       Running   0          22m
      in-memory-channel-dispatcher-67f7497fc-dgbrb    2/2       Running   1          22m
      webhook-7cfff8d86d-vjnqq                        1/1       Running   0          22m
      ```
      {: screen}

   4. 驗證 Knative `Sources` 元件的所有 Pod 都處於 `Running` 狀態。
      ```
      kubectl get pods --namespace knative-sources
      ```
      {: pre}

      輸出範例：
      ```
      NAME                   READY     STATUS    RESTARTS   AGE
      controller-manager-0   1/1       Running   0          22m
      ```
      {: screen}

   5. 驗證 Knative `Monitoring` 元件的所有 Pod 都處於 `Running` 狀態。
      ```
      kubectl get pods --namespace knative-monitoring
      ```
      {: pre}

      輸出範例：
      ```
      NAME                                  READY     STATUS                 RESTARTS   AGE
      elasticsearch-logging-0               1/1       Running                0          22m
      elasticsearch-logging-1               1/1       Running                0          21m
      grafana-79cf95cc7-mw42v               1/1       Running                0          21m
      kibana-logging-7f7b9698bc-m7v6r       1/1       Running                0          22m
      kube-state-metrics-768dfff9c5-fmkkr   4/4       Running                0          21m
      node-exporter-dzlp9                   2/2       Running                0          22m
      node-exporter-hp6gv                   2/2       Running                0          22m
      node-exporter-hr6vs                   2/2       Running                0          22m
      prometheus-system-0                   1/1       Running                0          21m
      prometheus-system-1                   1/1       Running                0          21m
      ```
      {: screen}

太棒了！全部設定完 Knative 及 Istio 之後，您現在可以將您的第一個無伺服器應用程式部署至您的叢集。

## 課程 2：將無伺服器應用程式部署至您的叢集
{: #deploy_app}

在本課程中，您將在 Go 中部署您的第一個無伺服器 [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) 應用程式。當您將要求傳送至您的範例應用程式時，應用程式會讀取環境變數 `TARGET`，並顯示出 `"Hello ${TARGET}!"`. 如果此環境變數是空的，則 `"Hello World!"` 會傳回。
{: shortdesc}

1. 在 Knative 中，為您的第一個無伺服器 `Hello World` 應用程式建立 YAML 檔案。若要使用 Knative 部署應用程式，您必須指定 Knative Service 資源。服務由 Knative `Serving` 基本元素所管理，且負責管理工作負載的整個生命週期。服務可確保每個部署都有一個 Knative 修訂版、一個路徑和一個配置。當您更新服務時，會建立應用程式的新版本，並新增至服務的修訂歷程。Knative 路徑可確保每個應用程式修訂都對映至一個網路端點，以便您可以控制遞送至特定修訂的網路資料流量。Knative 配置保留特定修訂的設定，以便您一律可以回復到舊版修訂，或在修訂之間切換。如需 Knative `Serving` 資源的相關資訊，請參閱 [Knative 文件](https://github.com/knative/docs/tree/master/serving)。
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
    <td>Kubernetes 名稱空間，您要在其中將您的應用程式部署為 Knative 服務。</td>
    </tr>
    <tr>
    <td><code>spec.container.image</code></td>
    <td>在其中儲存映像檔之容器登錄的 URL。在此範例中，您部署 Knative Hello World 應用程式，其儲存在 Docker Hub 中的 <code>ibmcom</code> 名稱空間中。</td>
    </tr>
    <tr>
    <td><code>spec.container.env</code></td>
    <td>您想要 Knative 服務擁有的環境變數清單。在本範例中，範例應用程式會讀取環境變數 <code>TARGET</code> 的值，然後在您傳送要求至應用程式時傳回，格式為 <code>"Hello ${TARGET}!"</code>. 如果未提供任何值，則範例應用程式會傳回 <code>"Hello World!"</code>.  </td>
    </tr>
    </tbody>
    </table>

2. 在叢集中建立 Knative 服務。當您建立服務時，Knative `Serving` 基本元素會針對您的應用程式，建立不可變的修訂、Knative 路徑、Ingress 遞送規則、Kubernetes 服務、Kubernetes Pod 以及負載平衡器。會從 Ingress 子網域指派一個子網域給您的應用程式，格式為：`<knative_service_name>.<namespace>.<ingress_subdomain>`，您可用來從網際網路存取應用程式。
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   輸出範例：
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. 驗證是否已建立 Pod。您的 Pod 由兩個容器所組成。一個容器執行 `Hello World` 應用程式，另一個容器則是執行 Istio 及 Knative 監視與記載工具的邊車容器。`00001` 修訂號碼已指派給您的 Pod。
   ```
   kubectl get pods
   ```
   {: pre}

   輸出範例：
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00001-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
   ```
   {: screen}

4. 試用 `Hello World` 應用程式。
   1. 取得指派給您 Knative 服務的預設網域。如果您已變更您 Knative 服務的名稱，或已將應用程式部署至不同的名稱空間，請更新您查詢中的這些值。
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

   2. 使用您在前一個步驟中擷取的子網域來要求您的應用程式。
      ```
      curl -v <service_domain>
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
      Hello Go Sample v1!
      * Connection #0 to host kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud left intact
      ```
      {: screen}

5. 請等待幾分鐘，讓 Knative 縮減您的 Pod。Knative 會評估一次必須啟動多少數量的 Pod 才能處理送入的工作負載。如果沒有收到任何網路資料流量，則 Knative 會自動縮減您的 Pod，甚至到 0 個 Pod，如本範例所示。

   想要瞭解 Knative 如何擴增您的 Pod？嘗試增加應用程式的工作負載，例如，使用像[簡單雲端型負載測試器](https://loader.io/)之類的工具。
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   如果您沒有看到任何 `kn-helloworld` Pod，表示 Knative 將您的應用程式縮減到 0 個 Pod。

6. 更新您的 Knative 服務範例，並為 `TARGET` 環境變數輸入不同的值。

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

7. 將變更套用至您的服務。變更配置時，Knative 會自動建立新的修訂、指派新的路徑，以及依預設指示 Istio 將送入的網路資料流量遞送至最新修訂。
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

8. 對您的應用程式提出新的要求，以驗證是否已套用您的變更。
   ```
   curl -v <service_domain>
   ```

   輸出範例：
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

9. 驗證 Knative 是否再次擴增您的 Pod，以因應增加的網路資料流量。`00002` 修訂號碼已指派給您的 Pod。比方說，當您想要指示 Istio 在兩個修訂之間分割送入的資料流量時，便可使用修訂號碼來參照特定版本的應用程式。
   ```
   kubectl get pods
   ```

   輸出範例：
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00002-deployment-55db6bf4c5-2vftm   3/3       Running   0          16s
   ```
   {: screen}

10. 選用項目：清除您的 Knative 服務。
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}

真棒！您已順利將第一個 Knative 應用程式部署至叢集，並已探索 Knative `Serving` 基本元素的修訂和調整功能。


## 下一步為何？   
{: #whats-next}

- 請試用此 [Knative 研討會 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/IBM/knative101/tree/master/workshop)，將您的第一個 `Node.js` fibonacci 應用程式部署至您的叢集。
  - 探索如何使用 Knative `Build` 基本元素，從 GitHub 中的 Dockerfile 建置映像檔，並自動將映像檔推送至 {{site.data.keyword.registrylong_notm}} 中的名稱空間。  
  - 瞭解如何將網路資料流量的遞送從 IBM 提供的 Ingress 子網域設定為 Knative 所提供的 Istio Ingress 閘道。
  - 推出應用程式的新版本，並使用 Istio 來控制遞送至每個應用程式版本的資料流量數量。
- 探索 [Knative `Eventing` ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/knative/docs/tree/master/eventing/samples) 範例。
- 使用 [Knative 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/knative/docs) 進一步瞭解 Knative。
