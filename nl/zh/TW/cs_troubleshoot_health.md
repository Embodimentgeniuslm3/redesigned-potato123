---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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


# 記載及監視的疑難排解
{: #cs_troubleshoot_health}

在您使用 {{site.data.keyword.containerlong}} 時，請考慮使用這些技術來進行記載及監視問題的疑難排解。
{: shortdesc}

如果您有更一般性的問題，請嘗試[叢集除錯](/docs/containers?topic=containers-cs_troubleshoot)。
{: tip}

## 日誌未出現
{: #cs_no_logs}

{: tsSymptoms}
存取 Kibana 儀表板時，未顯示任何日誌。

{: tsResolve}
請檢閱下列原因，以了解對應日誌為何未出現以及對應的疑難排解步驟：

<table>
<caption>疑難排解未顯示的日誌</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>發生原因</th>
      <th>修正方式</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>未設定任何記載配置。</td>
    <td>若要傳送日誌，您必須建立記載配置。若要這麼做，請參閱<a href="/docs/containers?topic=containers-health#logging">配置叢集記載</a>。</td>
  </tr>
  <tr>
    <td>叢集未處於 <code>Normal</code> 狀況。</td>
    <td>若要檢查叢集的狀況，請參閱<a href="/docs/containers?topic=containers-cs_troubleshoot#debug_clusters">叢集除錯</a>。</td>
  </tr>
  <tr>
    <td>已達到日誌儲存空間配額。</td>
    <td>若要增加日誌儲存空間限制，請參閱 <a href="/docs/services/CloudLogAnalysis/troubleshooting?topic=cloudloganalysis-error_msgs">{{site.data.keyword.loganalysislong_notm}} 文件</a>。</td>
  </tr>
  <tr>
    <td>如果您在建立叢集時指定了空間，則帳戶擁有者對該空間沒有「管理員」、「開發人員」或「審核員」許可權。</td>
      <td>若要變更帳戶擁有者的存取權，請執行下列動作：<ol><li>若要找出叢集的帳戶擁有者是誰，請執行 <code>ibmcloud ks api-key-info --cluster &lt;cluster_name_or_ID&gt;</code>。</li>
      <li>若要授與該帳戶擁有者對空間的「管理員」、「開發人員」或「審核員」等 {{site.data.keyword.containerlong_notm}} 存取許可權，請參閱<a href="/docs/containers?topic=containers-users">管理叢集存取</a>。</li>
      <li>若要在許可權變更之後重新整理記載記號，請執行 <code>ibmcloud ks logging-config-refresh --cluster &lt;cluster_name_or_ID&gt;</code>。</li></ol></td>
    </tr>
    <tr>
      <td>您的應用程式記載配置的應用程式路徑中有符號鏈結。</td>
      <td><p>若要傳送日誌，您必須在記載配置中使用絕對路徑，否則無法讀取日誌。如果路徑裝載到工作者節點，該節點可能會建立符號鏈結。</p> <p>範例：如果指定的路徑是 <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code>，但日誌進入 <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>，則無法讀取日誌。</p></td>
    </tr>
  </tbody>
</table>

若要測試您在疑難排解期間所做的變更，可以將 *Noisy*（這是可以產生數個日誌事件的範例 Pod）部署至叢集裡的工作者節點。

開始之前：[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. 建立 `deploy-noisy.yaml` 配置檔。
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: noisy
    spec:
      containers:
      - name: noisy
        image: ubuntu:16.04
        command: ["/bin/sh"]
        args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
        imagePullPolicy: "Always"
      ```
      {: codeblock}

2. 在叢集的環境定義中執行配置檔。
    ```
        kubectl apply -f noisy.yaml
        ```
    {:pre}

3. 幾分鐘之後，您可以在 Kibana 儀表板中檢視日誌。若要存取 Kibana 儀表板，請移至下列其中一個 URL，然後選取您建立叢集所在的 {{site.data.keyword.Bluemix_notm}} 帳戶。如果您在建立叢集時指定了空間，請改為移至該空間。
    - 美國南部及美國東部：`https://logging.ng.bluemix.net`
    - 英國南部：`https://logging.eu-gb.bluemix.net`
    - 歐盟中部：`https://logging.eu-fra.bluemix.net`
    - 亞太地區南部：`https://logging.au-syd.bluemix.net`

<br />


## Kubernetes 儀表板未顯示使用率圖形
{: #cs_dashboard_graphs}

{: tsSymptoms}
存取 Kubernetes 儀表板時，未顯示任何使用率圖形。

{: tsCauses}
有時，在叢集更新或工作者節點重新開機後，`kube-dashboard` Pod 不會更新。

{: tsResolve}
刪除 `kube-dashboard` Pod，以強制重新啟動。系統會以 RBAC 原則重建 Pod，存取 `heapster` 以取得使用率資訊。

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## 日誌配額太低
{: #quota}

{: tsSymptoms}
您可以在叢集裡設定記載配置，將日誌轉遞至 {{site.data.keyword.loganalysisfull}}。檢視日誌時，您會看到類似於下列內容的錯誤訊息：

```
您已達到配置給 IBM® Cloud Log Analysis 實例 {Instance GUID} 之 Bluemix 空間 {Space GUID} 的每日配額。您的現行每日配額是日誌搜尋儲存空間 XXX，這會保留 3 天的期間，在此期間內可以在 Kibana 中搜尋它。這不會影響「日誌收集」儲存空間中的日誌保留原則。若要升級您的方案，以便可以每天在「日誌搜尋」儲存空間中儲存更多資料，請升級此空間的 Log Analysis 服務方案。如需服務方案及如何升級方案的相關資訊，請參閱「方案」。
```
{: screen}

{: tsResolve}
請檢閱下列原因，以了解為何達到日誌配額以及對應的疑難排解步驟：

<table>
<caption>日誌儲存問題疑難排解</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>發生原因</th>
      <th>修正方式</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>一個以上 Pod 產生大量日誌。</td>
    <td>您可以防止轉遞來自特定 Pod 的日誌，藉以釋放日誌儲存空間。為這些 Pod 建立一個[記載過濾器](/docs/containers?topic=containers-health#filter-logs)。</td>
  </tr>
  <tr>
    <td>您會超過精簡方案 500 MB 的每日日誌儲存空間配額。</td>
    <td>首先，針對您的日誌網域，[計算搜尋配額及每日用量](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota)。然後，您可以透過[升級 {{site.data.keyword.loganalysisshort_notm}} 服務方案](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan)的方式，來增加您的日誌儲存空間配額。</td>
  </tr>
  <tr>
    <td>您超出現行付款方案的日誌儲存空間配額。</td>
    <td>首先，針對您的日誌網域，[計算搜尋配額及每日用量](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota)。然後，您可以透過[升級 {{site.data.keyword.loganalysisshort_notm}} 服務方案](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan)的方式，來增加您的日誌儲存空間配額。</td>
  </tr>
  </tbody>
</table>

<br />


## 日誌行太長
{: #long_lines}

{: tsSymptoms}
在叢集裡設定記載配置，將日誌轉遞至 {{site.data.keyword.loganalysisfull_notm}}。檢視日誌時，您會看到一條較長的日誌訊息。此外，在 Kibana 中，您可能只能看到日誌訊息的最後 600 - 700 個字元。

{: tsCauses}
在 Fluentd 收集日誌訊息之前，該日誌訊息可能就已經因為其長度過長而遭截斷，因此，在轉遞至 {{site.data.keyword.loganalysisshort_notm}} 之前，Fluentd 可能未正確剖析該日誌。

{: tsResolve}
若要限制行長度，您可以將自己的日誌程式配置為在每個日誌中具有 `stack_trace` 長度上限。例如，若您的日誌程式使用 Log4j，您可以使用 [`EnhancedPatternLayout`![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/EnhancedPatternLayout.html)，將 `stack_trace` 限制為 15KB。

## 取得協助及支援
{: #health_getting_help}

叢集仍有問題？
{: shortdesc}

-  在終端機中，有 `ibmcloud` CLI 及外掛程式的更新可用時，就會通知您。請務必保持最新的 CLI，讓您可以使用所有可用的指令及旗標。
-   若要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，請[檢查 {{site.data.keyword.Bluemix_notm}} 狀態頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/status?selected=status)。
-   將問題張貼到 [{{site.data.keyword.containerlong_notm}} Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com)。    如果您的 {{site.data.keyword.Bluemix_notm}} 帳戶未使用 IBM ID，請[要求邀請](https://bxcs-slack-invite.mybluemix.net/)以加入此 Slack。
    {: tip}
-   檢閱討論區，以查看其他使用者是否發生過相同的問題。使用討論區提問時，請標記您的問題，以便 {{site.data.keyword.Bluemix_notm}} 開發團隊能看到它。
    -   如果您在使用 {{site.data.keyword.containerlong_notm}} 開發或部署叢集或應用程式時有技術方面的問題，請將問題張貼到 [Stack Overflow ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers)，並使用 `ibm-cloud`、`kubernetes` 及 `containers` 來標記問題。
    -   若為服務及開始使用指示的相關問題，請使用 [IBM Developer Answers ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 討論區。請包含 `ibm-cloud` 及 `containers` 標籤。如需使用討論區的詳細資料，請參閱[取得協助](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)。
-   開立案例，以與「IBM 支援中心」聯絡。若要瞭解如何開立 IBM 支援中心案例，或是瞭解支援層次與案例嚴重性，請參閱[與支援中心聯絡](/docs/get-support?topic=get-support-getting-customer-support)。當您報告問題時，請包含您的叢集 ID。若要取得叢集 ID，請執行 `ibmcloud ks clusters`。您也可以使用 [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)，來收集及匯出叢集裡的相關資訊，以與 IBM 支援中心共用。
{: tip}

