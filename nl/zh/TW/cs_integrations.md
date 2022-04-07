---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-09"

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


# 整合服務
{: #integrations}

在 {{site.data.keyword.containerlong}} 中，您可以使用各種外部服務及型錄服務來與標準 Kubernetes 叢集搭配使用。
{:shortdesc}


## DevOps Services
{: #devops_services}
<table summary="此表格顯示可新增至叢集以新增額外 DevOps 功能的可用服務。列應該從左到右閱讀，第一欄為服務名稱，第二欄為服務說明。">
<caption>DevOps Services</caption>
<thead>
<tr>
<th>服務</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cfee_full_notm}}</td>
<td>在 Kubernetes 叢集之上部署及管理自己的 Cloud Foundry 平台來開發、包裝、部署及管理雲端原生應用程式，並運用 {{site.data.keyword.Bluemix_notm}} 生態系統將其他服務連結至應用程式。建立 {{site.data.keyword.cfee_full_notm}} 實例時，您必須藉由選擇工作者節點的機型和 VLAN 來配置 Kubernetes 叢集。叢集接著會佈建 {{site.data.keyword.containerlong_notm}}，而且 {{site.data.keyword.cfee_full_notm}} 會自動部署至叢集。如需如何設定 {{site.data.keyword.cfee_full_notm}} 的相關資訊，請參閱[入門指導教學](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started)。</td>
</tr>
<tr>
<td>Codeship</td>
<td>您可以使用 <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>，以持續整合及遞送容器。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">使用 Codeship Pro 將工作負載部署至 {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Grafeas</td>
<td>[Grafeas ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://grafeas.io) 是一種開放程式碼 CI/CD 服務，提供如何在軟體供應鏈處理程序期間擷取、儲存及交換 meta 資料的常見方法。例如，如果您將 Grafeas 整合至應用程式建置處理程序，則 Grafeas 可以儲存建置要求起始器的相關資訊、漏洞掃描結果及品質保證登出，讓您可以在將應用程式部署至正式作業時做出明智的決策。您可以在審核中使用此 meta 資料，或證明軟體供應鏈的規範。</td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 是 Kubernetes 套件管理程式。您可以建立新的 Helm 圖表或使用預先存在的 Helm 圖表，來定義、安裝及升級在 {{site.data.keyword.containerlong_notm}} 叢集裡執行的複式 Kubernetes 應用程式。<p>如需相關資訊，請參閱[在 {{site.data.keyword.containerlong_notm}} 中設定 Helm](/docs/containers?topic=containers-integrations#helm)。</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>使用工具鏈自動建置應用程式，並將容器部署至 Kubernetes 叢集。如需設定資訊，請參閱部落格 <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Istio on {{site.data.keyword.containerlong_notm}}</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 是一種開放程式碼服務，提供方法讓開發人員連接、保護、管理及監視雲端編排平台上微服務（也稱為服務網）的網路。Istio on {{site.data.keyword.containerlong}} 透過受管理附加程式，對叢集提供 Istio 單步驟安裝。只要按一下，您就可以取得所有 Istio 核心元件、額外的追蹤、監視和視覺化，以及啟動並執行 BookInfo 範例應用程式。若要開始使用，請參閱[使用受管理 Istio 附加程式（測試版）](/docs/containers?topic=containers-istio)。</td>
</tr>
<tr>
<td>Jenkins X</td>
<td>Jenkins X 是一種 Kubernetes 原生持續整合及持續交付平台，可用來自動執行建置處理程序。如需如何在 {{site.data.keyword.containerlong_notm}} 上安裝的相關資訊，請參閱 [Jenkins X 開放程式碼專案簡介 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/blogs/bluemix/2018/08/installing-jenkins-x-on-ibm-cloud-kubernetes-service/)。</td>
</tr>
<tr>
<td>Knative</td>
<td>[Knative ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/knative/docs) 是 IBM、Google、Pivotal、Red Hat、Cisco 及其他公司所開發的開放程式碼平台，目標是擴充 Kubernetes 功能，以協助您在 Kubernetes 叢集之上建立現代、以原始檔為主的容器化和無伺服器應用程式。此平台在各程式設計語言及架構之間使用一致的方式，以摘要建置、部署及管理 Kubernetes 中工作負載的作業負載，讓開發人員可以專注於最重要的事物：原始碼。如需相關資訊，請參閱[指導教學：使用受管理 Knative，以在 Kubernetes 叢集中執行無伺服器應用程式](/docs/containers?topic=containers-knative_tutorial#knative_tutorial)。</td>
</tr>
</tbody>
</table>

<br />



## 記載及監視服務
{: #health_services}
<table summary="此表格顯示可新增至叢集以新增額外記載及監視功能的可用服務。列應該從左到右閱讀，第一欄為服務名稱，第二欄為服務說明。">
<caption>記載及監視服務</caption>
<thead>
<tr>
<th>服務</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>使用 <a href="https://www.newrelic.com/coscale" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來監視工作者節點、容器、抄本集、抄寫控制器及服務。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">使用 CoScale 監視 {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Datadog</td>
<td>使用 <a href="https://www.datadoghq.com/" target="_blank">Dataloadog <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來監視您的叢集，並檢視基礎架構和應用程式效能度量值。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">使用 Datadog 監視 {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>透過 Grafana 分析日誌，來監視叢集裡所做的管理活動。如需服務的相關資訊，請參閱 [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla) 文件。如需您可追蹤之事件類型的相關資訊，請參閱 [Activity Tracker 事件](/docs/containers?topic=containers-at_events)。</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>將日誌管理功能新增至叢集，方法為將 LogDNA 作為協力廠商服務部署至工作者節點，以管理來自 Pod 容器的日誌。如需相關資訊，請參閱[使用 {{site.data.keyword.loganalysisfull_notm}} 搭配 LogDNA 來管理 Kubernetes 叢集日誌](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube)。</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>取得應用程式效能及性能的作業可見性，方法為將 Sysdig 作為協力廠商服務部署至工作者節點，以將度量轉遞至 {{site.data.keyword.monitoringlong}}。如需相關資訊，請參閱[分析在 Kubernetes 叢集裡部署之應用程式的度量](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)。</td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 透過自動探索及對映應用程式的 GUI，提供基礎架構及應用程式效能監視。Instana 會擷取向應用程式提出的每個要求，您可以用來進行疑難排解，並執行主要原因分析，以避免問題再次發生。如需進一步瞭解，請查看關於<a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">在 {{site.data.keyword.containerlong_notm}} 中部署 Instana <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 的部落格文章。</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus 是一個特別針對 Kubernetes 所設計的開放程式碼監視、記載及警示工具。Prometheus 會根據 Kubernetes 記載資訊來擷取叢集、工作者節點及部署性能的詳細資訊。針對叢集裡執行的每一個容器收集 CPU、記憶體、I/O 及網路活動。您可以使用自訂查詢或警示中所收集的資料，來監視叢集裡的效能及工作負載。

<p>若要使用 Prometheus，請遵循 <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">CoreOS 指示 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>使用 <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來檢視容器化應用程式的度量值和日誌。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">使用 Sematext 監視及記載容器 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Splunk</td>
<td>使用 Splunk Connect for Kubernetes，在 Splunk 中匯入並搜尋 Kubernetes 記載、物件及度量值資料。Splunk Connect for Kubernetes 包含可將 Fluentd 的 Splunk 支援部署部署至 Kubernetes 叢集的 Helm 圖表、傳送日誌和 meta 資料的 Splunk 建置「Fluentd HTTP 事件收集器 (HEC)」外掛程式，以及可擷取叢集度量值的度量值部署。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2019/02/solving-business-problems-with-splunk-on-ibm-cloud-kubernetes-service/" target="_blank">使用 Splunk on {{site.data.keyword.containerlong_notm}} 解決商業問題 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope 提供 Kubernetes 叢集內資源（包括服務、Pod、容器、處理程序、節點等項目）的視覺圖。Weave Scope 提供 CPU 及記憶體的互動式度量值，也提供工具來調整並執行至容器。
<p>如需相關資訊，請參閱[使用 Weave Scope 及 {{site.data.keyword.containerlong_notm}} 視覺化 Kubernetes 叢集資源](/docs/containers?topic=containers-integrations#weavescope)。</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## 安全服務
{: #security_services}

想要全面檢視如何整合 {{site.data.keyword.Bluemix_notm}} 安全服務與您的叢集嗎？請參閱[將端對端安全套用至雲端應用程式指導教學](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security)。
{: shortdesc}

<table summary="此表格顯示可新增至叢集以新增額外安全功能的可用服務。列應該從左到右閱讀，第一欄為服務名稱，第二欄為服務說明。">
<caption>安全服務</caption>
<thead>
<tr>
<th>服務</th>
<th>說明</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>要求使用者登入，以 [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) 新增應用程式的安全層次。若要鑑別針對您應用程式的 API HTTP/HTTPS 要求，您可以使用 [{{site.data.keyword.appid_short_notm}} 鑑別 Ingress 註釋](/docs/containers?topic=containers-ingress_annotation#appid-auth)，來整合 {{site.data.keyword.appid_short_notm}} 與 Ingress 服務。</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>作為<a href="/docs/services/va?topic=va-va_index" target="_blank">漏洞警告器</a>的補充，您可以使用 <a href="https://www.aquasec.com/" target="_blank">Aqua Security<img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>，透過減少容許應用程式執行的內容來改善容器部署的安全。如需相關資訊，請參閱<a href="https://blog.aquasec.com/securing-container-deployments-on-bluemix-with-aqua-security" target="_blank">使用 Aqua Security 保護 {{site.data.keyword.Bluemix_notm}} 上的容器部署 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>您可以使用 <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來儲存及管理應用程式的 SSL 憑證。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">使用 {{site.data.keyword.cloudcerts_long_notm}} 與 {{site.data.keyword.containerlong_notm}} 搭配來部署自訂網域 TLS 憑證 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
  <td>{{site.data.keyword.datashield_full}}（測試版）</td>
  <td>您可以使用 <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來加密資料記憶體。{{site.data.keyword.datashield_short}} 與 Intel® Software Guard Extensions (SGX) 及 Fortanix® 技術整合，以在使用時保護 {{site.data.keyword.Bluemix_notm}} 容器工作負載程式碼和資料。應用程式碼和資料在 CPU 強化區域（為工作者節點上保護應用程式重要層面的授信記憶體區域）中執行，有助於保持程式碼和資料的機密而且未經修改。</td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>設定您自己的安全 Docker 映像檔儲存庫，您可以在其中安全地儲存映像檔，以及在叢集使用者之間共用映像檔。如需相關資訊，請參閱 <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}} 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
  <td>{{site.data.keyword.keymanagementservicefull}}</td>
  <td>啟用 {{site.data.keyword.keymanagementserviceshort}} 來加密位於叢集裡的 Kubernetes 密碼。加密 Kubernetes 密碼，可防止未獲授權的使用者存取機密叢集資訊。<br>若要設定，請參閱<a href="/docs/containers?topic=containers-encryption#keyprotect">使用 {{site.data.keyword.keymanagementserviceshort}} 加密 Kubernetes 密碼</a>。<br>如需相關資訊，請參閱 <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>NeuVector</td>
<td>使用 <a href="https://neuvector.com/" target="_blank">Neutretor <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來保護容器與雲端原生防火牆。如需相關資訊，請參閱 <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Twistlock</td>
<td>作為<a href="/docs/services/va?topic=va-va_index" target="_blank">漏洞警告器</a>的補充，您可以使用 <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來管理防火牆、威脅保護及突發事件回應。如需相關資訊，請參閱 <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">{{site.data.keyword.containerlong_notm}} 上的 Twistlock <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
</tbody>
</table>

<br />



## 儲存服務
{: #storage_services}
<table summary="此表格顯示可新增至叢集以新增持續性儲存空間功能的可用服務。列應該從左到右閱讀，第一欄為服務名稱，第二欄為服務說明。">
<caption>儲存服務</caption>
<thead>
<tr>
<th>服務</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Velero</td>
  <td>您可以使用 <a href="https://github.com/heptio/velero" target="_blank">Heptio Velero <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>，來備份及還原叢集資源與持續性磁區。如需相關資訊，請參閱 Heptio Velero <a href="https://github.com/heptio/velero/blob/release-0.9/docs/use-cases.md" target="_blank">災難回復及叢集移轉的使用案例 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
  <td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) 是一種持續性的高效能 iSCSI 儲存空間，可以使用 Kubernetes 持續性磁區 (PV) 將其新增至應用程式。使用區塊儲存空間以在單一區域中部署有狀態應用程式，或作為單一 Pod 的高效能儲存空間。如需如何在叢集中佈建區塊儲存空間的相關資訊，請參閱[在 {{site.data.keyword.Bluemix_notm}} Block Storage 上儲存資料](/docs/containers?topic=containers-block_storage#block_storage)</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>與 {{site.data.keyword.cos_short}} 一起儲存的資料會加密及分散在多個地理位置，並使用 REST API 透過 HTTP 進行存取。您可以使用 [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) 來配置服務，以針對叢集裡的資料進行一次性或排定的備份。如需服務的一般資訊，請參閱 <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage" target="_blank">{{site.data.keyword.cos_short}} 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
  <tr>
  <td>{{site.data.keyword.Bluemix_notm}} File Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) 是一種持續性、快速且具彈性的網路連結的 NFS 型檔案儲存空間，可以使用 Kubernetes 持續性磁區將其新增至應用程式。您可以選擇具有 GB 大小及 IOPS 符合工作負載需求的預先定義儲存空間層級。如需如何在叢集中佈建檔案儲存空間的相關資訊，請參閱[在 {{site.data.keyword.Bluemix_notm}} File Storage 上儲存資料](/docs/containers?topic=containers-file_storage#file_storage)。</td>
  </tr>
  <tr>
    <td>Portworx</td>
    <td>[Portworx ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://portworx.com/products/introduction/) 是一個高可用性軟體定義儲存空間解決方案，可用來管理容器化資料庫及其他有狀態應用程式的持續性儲存空間，或在多個區域的 Pod 之間共用資料。您可以使用 Helm 圖表來安裝 Portworx，以及使用 Kubernetes 持續性磁區來佈建應用程式的儲存空間。如需如何在叢集中設定 Portworx 的相關資訊，請參閱[使用 Portworx 將資料儲存在軟體定義儲存空間 (SDS)](/docs/containers?topic=containers-portworx#portworx)。</td>
  </tr>
</tbody>
</table>

<br />


## 資料庫服務
{: #database_services}

<table summary="此表格顯示可新增至叢集以新增資料庫功能的可用服務。列應該從左到右閱讀，第一欄為服務名稱，第二欄為服務說明。">
<caption>資料庫服務</caption>
<thead>
<tr>
<th>服務</th>
<th>說明</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.blockchainfull_notm}} Platform 2.0 測試版</td>
    <td>在 {{site.data.keyword.containerlong_notm}} 上部署及管理自己的 {{site.data.keyword.blockchainfull_notm}} Platform。使用 {{site.data.keyword.blockchainfull_notm}} Platform 2.0，您可以管理 {{site.data.keyword.blockchainfull_notm}} 網路，或建立可結合其他 {{site.data.keyword.blockchainfull_notm}} 2.0 網路的組織。如需如何在 {{site.data.keyword.containerlong_notm}} 中設定 {{site.data.keyword.blockchainfull_notm}} 的相關資訊，請參閱[關於 {{site.data.keyword.blockchainfull_notm}} Platform 免費 2.0 測試版](/docs/services/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview)。</td>
  </tr>
<tr>
  <td>雲端資料庫</td>
  <td>您可以選擇各種 {{site.data.keyword.Bluemix_notm}} 資料庫服務（例如 {{site.data.keyword.composeForMongoDB_full}} 或 {{site.data.keyword.cloudantfull}}），以在叢集中部署高度可用且可擴充的資料庫解決方案。如需可用的雲端資料庫清單，請參閱 [{{site.data.keyword.Bluemix_notm}} 型錄 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/catalog?category=databases)。</td>
  </tr>
  </tbody>
  </table>


## 將 {{site.data.keyword.Bluemix_notm}} 服務新增至叢集裡
{: #adding_cluster}

新增 {{site.data.keyword.Bluemix_notm}} 服務以加強您的 Kubernetes 叢集，其在 Watson AI、資料、安全及物聯網 (IoT) 等方面有額外功能。
{:shortdesc}

您只能連結支援服務金鑰的服務。若要找出支援服務金鑰的服務清單，請參閱[啟用外部應用程式以使用 {{site.data.keyword.Bluemix_notm}} 服務](/docs/resources?topic=resources-externalapp#externalapp)。
{: note}

開始之前：
- 確定您具有下列角色：
    - 叢集的[**編輯者**或**管理者** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)。
    - 您要連結服務的名稱空間的 [**Writer** 或 **Manager** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)。
    - 您要使用之空間的[**開發人員** Cloud Foundry 角色](/docs/iam?topic=iam-mngcf#mngcf)
- [登入您的帳戶。將目標設為適當的地區及（如果適用的話）資源群組。設定叢集的環境定義](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

若要將 {{site.data.keyword.Bluemix_notm}} 服務新增至叢集裡，請執行下列動作：

1. [建立 {{site.data.keyword.Bluemix_notm}} 服務的實例](/docs/resources?topic=resources-externalapp#externalapp)。
    * 部分 {{site.data.keyword.Bluemix_notm}} 服務僅特定地區才有提供。唯有與您的叢集相同的地區中有提供某服務時，您才可以將該服務連結至您的叢集。此外，如果您要在華盛頓特區中建立服務實例，則必須使用 CLI。
    * 您必須在與叢集相同的資源群組中建立服務實例。只能在一個資源群組中建立一個資源，之後就無法進行變更。

2. 請檢查您所建立的服務類型，並記下服務實例**名稱**。
   - **Cloud Foundry 服務：**
     ```
    ibmcloud service list
    ```
     {: pre}

     輸出範例：
        ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **{{site.data.keyword.Bluemix_notm}}已啟用 IAM 的服務：**
     ```
    ibmcloud resource service-instances
    ```
     {: pre}

     輸出範例：
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   您也可以在儀表板中看到不同的服務類型：**Cloud Foundry 服務**和**服務**。

3. 識別您要用來新增服務的叢集名稱空間。請選擇下列選項。
     ```
     kubectl get namespaces
     ```
     {: pre}

4.  使用 `ibmcloud ks cluster-service-bind` [指令](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_bind)，將服務新增至叢集。對於已啟用 {{site.data.keyword.Bluemix_notm}} IAM 的服務，請務必使用您先前建立的 Cloud Foundry 別名。
    對於已啟用 IAM 的服務，您也可以使用預設的 **Writer** 服務存取角色，或使用 `--role` 旗標來指定服務存取角色。此指令會建立服務實例的服務金鑰，或者您可以包括 `--key` 旗標來使用現有服務金鑰認證。如果您使用 `--key` 旗標，則不要包括 `--role` 旗標。
    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
    ```
    {: pre}

    將服務順利新增至叢集之後，即會建立叢集密碼，以保留服務實例認證。在 etcd 中會將密碼自動加密，以保護您的資料。

    輸出範例：
    ```
    ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

5.  驗證 Kubernetes 密碼中的服務認證。
    1. 取得密碼的詳細資料，並記下**binding** 值。**binding** 值以 base64 編碼，並以 JSON 格式保留服務實例的認證。
       ```
       kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
       ```
       {: pre}

       輸出範例：
       ```
       apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
       ```
       {: screen}

    2. 將連結值解碼。
       ```
       echo "<binding>" | base64 -D
       ```
       {: pre}

       輸出範例：
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    3. 選用項目：將您在前一個步驟中所解碼的服務認證與您在 {{site.data.keyword.Bluemix_notm}} 儀表板中針對服務實例所找到的服務認證進行比較。

6. 既然您的服務已連結至叢集，您必須配置應用程式[存取 Kubernetes 密碼中的服務認證](#adding_app)。


## 從應用程式存取服務認證
{: #adding_app}

若要從應用程式存取 {{site.data.keyword.Bluemix_notm}} 服務實例，您必須把儲存在 Kubernetes 密碼中的服務認證提供給應用程式。
{: shortdesc}

服務實例的認證會以 base64 編碼，並以 JSON 格式儲存在您的密碼內。若要存取您密碼中的資料，請在下列選項之間進行選擇：
- [以磁區將密碼裝載至您的 Pod](#mount_secret)
- [在環境變數中參照該密碼](#reference_secret)
<br>
想要讓您的密碼更加安全嗎？請要求叢集管理者在您的叢集裡[啟用 {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect)，以加密新的及現有的密碼，例如儲存 {{site.data.keyword.Bluemix_notm}} 服務實例之認證的密碼。
{: tip}

開始之前：
-  確定您具有 `kube-system` 名稱空間的 [**Writer** 或 **Manager** {{site.data.keyword.Bluemix_notm}}IAM 服務角色](/docs/containers?topic=containers-users#platform)。
- [登入您的帳戶。將目標設為適當的地區及（如果適用的話）資源群組。設定叢集的環境定義](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。
- [將 {{site.data.keyword.Bluemix_notm}} 服務新增至叢集](#adding_cluster)。

### 以磁區將密碼裝載至您的 Pod
{: #mount_secret}

以磁區將密碼裝載至 Pod 時，名稱為 `binding` 的檔案會儲存在磁區裝載目錄中。JSON 格式的 `binding` 檔案包含存取 {{site.data.keyword.Bluemix_notm}} 服務所需的所有資訊和認證。
{: shortdesc}

1.  列出叢集裡的可用密碼，並記下您密碼的**名稱**。尋找類型為 **Opaque** 的密碼。如果有多個密碼，請與叢集管理者聯絡，以識別正確的服務密碼。

    ```
    kubectl get secrets
    ```
    {: pre}

    輸出範例：

    ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m

    ```
    {: screen}

2.  為您的 Kubernetes 部署建立 YAML 檔案，並以磁區將該密碼裝載至您的 Pod。
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: registry.bluemix.net/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>瞭解 YAML 檔案元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts.mountPath</code></td>
    <td>容器內裝載磁區之目錄的絕對路徑。</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>要裝載至 Pod 之磁區的名稱。</td>
    </tr>
    <tr>
    <td><code>secret.defaultMode</code></td>
    <td>密碼的讀取權和寫入權。請使用 `420` 來設定唯讀許可權。</td>
    </tr>
    <tr>
    <td><code>secret.secretName</code></td>
    <td>您在前一個步驟記下的密碼名稱。</td>
    </tr></tbody></table>

3.  建立 Pod，並以磁區裝載密碼。
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  驗證已建立 Pod。
    ```
    kubectl get pods
    ```
    {: pre}

    CLI 輸出範例：

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  存取服務認證。
    1. 登入 Pod。
       ```
       kubectl exec <pod_name> -it bash
       ```
       {: pre}

    2. 導覽至您稍早定義的磁區裝載路徑，並列出磁區裝載路徑中的檔案。
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}

       輸出範例：
       ```
       binding
       ```
       {: screen}

       `binding` 檔案包含您儲存在 Kubernetes 密碼中的服務認證。

    4. 檢視服務認證。認證會以 JSON 格式儲存為鍵值組。
       ```
       cat binding
       ```
       {: pre}

       輸出範例：
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. 配置應用程式以剖析 JSON 內容，並擷取您存取服務所需的資訊。


### 在環境變數中參照該密碼
{: #reference_secret}

您可以將 Kubernetes 密碼中的服務認證及其他鍵值組當成環境變數新增至部署中。
{: shortdesc}

1. 列出叢集裡的可用密碼，並記下您密碼的**名稱**。尋找類型為 **Opaque** 的密碼。如果有多個密碼，請與叢集管理者聯絡，以識別正確的服務密碼。

    ```
    kubectl get secrets
    ```
    {: pre}

    輸出範例：

    ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m
    ```
    {: screen}

2. 取得您密碼的詳細資料，以尋找您可以在 Pod 中作為環境變數參照的可能鍵值組。服務認證儲存在您密碼的 `binding` 索引鍵中。
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   輸出範例：
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. 為您的 Kubernetes 部署建立 YAML 檔案，並指定參照 `binding` 索引鍵的環境變數。
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: registry.bluemix.net/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>瞭解 YAML 檔案元件</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers.env.name</code></td>
     <td>環境變數的名稱。</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.name</code></td>
     <td>您在前一個步驟記下的密碼名稱。</td>
     </tr>
     <tr>
     <td><code>env.valueFrom.secretKeyRef.key</code></td>
     <td>此索引鍵為您密碼的一部分，您想在環境變數中參照它。如果要參照服務認證，您必須使用 <strong>binding</strong> 索引鍵。</td>
     </tr>
     </tbody></table>

4. 建立 Pod，其以環境變數參照您密碼的 `binding` 索引鍵。
   ```
    kubectl apply -f secret-test.yaml
    ```
   {: pre}

5. 驗證已建立 Pod。
   ```
   kubectl get pods
   ```
   {: pre}

   CLI 輸出範例：
   ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
   {: screen}

6. 驗證是否已正確設定環境變數。
   1. 登入 Pod。
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. 列出 Pod 中的所有環境變數。
      ```
      env
      ```
      {: pre}

      輸出範例：
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. 配置應用程式來讀取環境變數並剖析 JSON 內容，以擷取您存取該服務所需的資訊。

   Python 程式碼範例：
   ```
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

## 在 {{site.data.keyword.containerlong_notm}} 中設定 Helm
{: #helm}

[Helm ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://helm.sh) 是 Kubernetes 套件管理程式。您可以建立 Helm 圖表或使用預先存在的 Helm 圖表，來定義、安裝及升級在 {{site.data.keyword.containerlong_notm}} 叢集裡執行的複式 Kubernetes 應用程式。
{:shortdesc}

若要部署 Helm 圖表，您必須在本端機器上安裝 Helm CLI，並在叢集中安裝 Helm 伺服器 Tiller。Tiller 的映像檔儲存在公用 Google Container Registry 中。若要在 Tiller 安裝期間存取映像檔，您的叢集必須容許與公用 Google Container Registry 的公用網路連線功能。已啟用公用服務端點的叢集可以自動存取映像檔。受自訂防火牆保護的專用叢集或僅啟用專用服務端點的叢集，不容許存取 Tiller 映像檔。相反地，您可以[將映像檔取回至本端機器，並將映像檔推送至 {{site.data.keyword.registryshort_notm}} 中的名稱空間](#private_local_tiller)，或[在不使用 Tiller 的情況下安裝 Helm 圖表](#private_install_without_tiller)。
{: note}

### 在具有公用存取權的叢集中設定 Helm
{: #public_helm_install}

如果您的叢集已啟用公用服務端點，則可以使用 Google Container Registry 中的公用映像檔來安裝 Tiller。
{: shortdesc}

開始之前：[登入您的帳戶。將目標設為適當的地區及（如果適用的話）資源群組。設定叢集的環境定義](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。

1. 安裝 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。

2. **重要事項**：若要維護叢集安全，請在 `kube-system` 名稱空間中建立 Tiller 的服務帳戶，以及針對 `tiller-deploy` Pod 建立 Kubernetes RBAC 叢集角色連結，方法是套用 [{{site.data.keyword.Bluemix_notm}} `kube-samples` 儲存庫](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml)中的下列 `.yaml` 檔案。**附註**：如果要在 `kube-system` 名稱空間中，利用服務帳戶和叢集角色連結來安裝 Tiller，您必須具有 [`cluster-admin` 角色](/docs/containers?topic=containers-users#access_policies)。
    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: pre}

3. 使用您所建立的服務帳戶，來起始設定 Helm 並安裝 Tiller。

    ```
    helm init --service-account tiller
    ```
    {: pre}

4.  驗證安裝已成功。
    1.  驗證已建立 Tiller 服務帳戶。
        ```
        kubectl get serviceaccount -n kube-system tiller
        ```
        {: pre}

        輸出範例：

        ```
        NAME                                 SECRETS   AGE
        tiller                               1         2m
        ```
        {: screen}

    2.  驗證 `tiller - deploy` Pod 在叢集裡的 **Status** 為 `Running`。
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        輸出範例：

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
        tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

5. 將 {{site.data.keyword.Bluemix_notm}} Helm 儲存庫新增至 Helm 實例。
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

6. 更新儲存庫，以擷取所有 Helm 圖表的最新版本。
   ```
    helm repo update
    ```
   {: pre}

7. 列出 {{site.data.keyword.Bluemix_notm}} 儲存庫中目前可用的 Helm 圖表。
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

8. 識別您要安裝的 Helm 圖表，並遵循 Helm 圖表 `README` 中的指示，以在叢集中安裝 Helm 圖表。


### 專用叢集：將 Tiller 映像檔推送至 {{site.data.keyword.registryshort_notm}} 中的專用登錄
{: #private_local_tiller}

您可以將 Tiller 映像檔取回至本端機器、將它推送至 {{site.data.keyword.registryshort_notm}} 中的名稱空間，以及讓 Helm 使用 {{site.data.keyword.registryshort_notm}} 中的映像檔來安裝 Tiller。
{: shortdesc}

開始之前：
- 在本端機器上安裝 Docker。如果您安裝了 [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)，則已安裝 Docker。
- [安裝 {{site.data.keyword.registryshort_notm}} CLI 外掛程式並設定名稱空間](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install)。

若要使用 {{site.data.keyword.registryshort_notm}} 來安裝 Tiller，請執行下列動作：

1. 在本端機器上安裝 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。
2. 使用您設定的 {{site.data.keyword.Bluemix_notm}} 基礎架構 VPN 通道，來連接至專用叢集。
3. **重要事項**：若要維護叢集安全，請在 `kube-system` 名稱空間中建立 Tiller 的服務帳戶，以及針對 `tiller-deploy` Pod 建立 Kubernetes RBAC 叢集角色連結，方法是套用 [{{site.data.keyword.Bluemix_notm}} `kube-samples` 儲存庫](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml)中的下列 `.yaml` 檔案。**附註**：如果要在 `kube-system` 名稱空間中，利用服務帳戶和叢集角色連結來安裝 Tiller，您必須具有 [`cluster-admin` 角色](/docs/containers?topic=containers-users#access_policies)。
    
    1. [取得 Kubernetes 服務帳戶和叢集角色連結 YAML 檔案 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml)。

    2. 在叢集中建立 Kubernetes 資源。
       ```
       kubectl apply -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [尋找 Tiller 版本 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30)，您要在叢集中安裝該版本。如果您不需要特定版本，請使用最新版本。

5. 將 Tiller 映像檔取回至本端機器。
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   輸出範例：
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling from kubernetes-helm/tiller
   48ecbb6b270e: Pull complete
   d3fa0712c71b: Pull complete
   bf13a43b92e9: Pull complete
   b3f98be98675: Pull complete
   Digest: sha256:c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbb463aa6
   Status: Downloaded newer image for gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [將 Tiller 映像檔推送至 {{site.data.keyword.registryshort_notm}} 中的名稱空間](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing)。

7. [將映像檔取回密碼從 default 名稱空間複製到 `kube-system` 名稱空間，以存取 {{site.data.keyword.registryshort_notm}}](/docs/containers?topic=containers-images#copy_imagePullSecret)。

8. 使用您在 {{site.data.keyword.registryshort_notm}} 的名稱空間中儲存的映像檔，在專用叢集中安裝 Tiller。
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. 將 {{site.data.keyword.Bluemix_notm}} Helm 儲存庫新增至 Helm 實例。
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

10. 更新儲存庫，以擷取所有 Helm 圖表的最新版本。
    ```
    helm repo update
    ```
    {: pre}

11. 列出 {{site.data.keyword.Bluemix_notm}} 儲存庫中目前可用的 Helm 圖表。
    ```
    helm search ibm
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. 識別您要安裝的 Helm 圖表，並遵循 Helm 圖表 `README` 中的指示，以在叢集中安裝 Helm 圖表。

### 專用叢集：在不使用 Tiller 的情況下安裝 Helm 圖表
{: #private_install_without_tiller}

如果您不要在專用叢集中安裝 Tiller，則可以使用 `kubectl` 指令，手動建立 Helm 圖表 YAML 檔案，並套用這些檔案。
{: shortdesc}

此範例中的步驟顯示如何從專用叢集的 {{site.data.keyword.Bluemix_notm}} Helm 圖表儲存庫中安裝 Helm 圖表。如果您要安裝的 Helm 圖表未儲存在其中一個 {{site.data.keyword.Bluemix_notm}} Helm 圖表儲存庫中，則必須遵循此主題中的指示，來建立 Helm 圖表的 YAML 檔案。此外，您還必須從公用 Container Registry 下載 Helm 圖表映像檔，並將它推送至 {{site.data.keyword.registryshort_notm}} 中的名稱空間，然後更新 `values.yaml` 檔案以使用 {{site.data.keyword.registryshort_notm}} 中的映像檔。
{: note}

1. 在本端機器上安裝 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。
2. 使用您設定的 {{site.data.keyword.Bluemix_notm}} 基礎架構 VPN 通道，來連接至專用叢集。
3. 將 {{site.data.keyword.Bluemix_notm}} Helm 儲存庫新增至 Helm 實例。
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

4. 更新儲存庫，以擷取所有 Helm 圖表的最新版本。
   ```
    helm repo update
    ```
   {: pre}

5. 列出 {{site.data.keyword.Bluemix_notm}} 儲存庫中目前可用的 Helm 圖表。
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. 識別您要安裝的 Helm 圖表、將 Helm 圖表下載至本端機器，以及解壓縮 Helm 圖表的檔案。下列範例顯示如何下載 Cluster Autoscaler 1.0.3 版的 Helm 圖表，並解壓縮 `cluster-autoscaler` 目錄中的檔案。
   ```
   helm fetch ibm/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. 導覽至您要在其中解壓縮 Helm 圖表檔案的目錄。
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. 針對您使用 Helm 圖表中的檔案所產生的 YAML 檔案，建立 `output` 目錄。
   ```
   mkdir output
   ```
   {: pre}

9. 開啟 `values.yaml` 檔案，然後進行 Helm 圖表安裝指示所需的全部變更。
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. 使用本端 Helm 安裝，以建立 Helm 圖表的所有 Kubernetes YAML 檔案。YAML 檔案會儲存在您先前建立的 `output` 目錄中。
    ```
    helm template --values ./ibm-iks-cluster-autoscaler/values.yaml --output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    輸出範例：
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. 將所有 YAML 檔案部署至專用叢集。
    ```
    kubectl apply --recursive --filename ./output
    ```
   {: pre}

12. 選用項目：從 `output` 目錄中移除所有 YAML 檔案。
    ```
    kubectl delete --recursive --filename ./output
    ```
    {: pre}


### 相關的 Helm 鏈結
{: #helm_links}

* 若要使用 strongSwan Helm 圖表，請參閱[使用 strongSwan IPSec VPN 服務 Helm 圖表設定 VPN 連線功能](/docs/containers?topic=containers-vpn#vpn-setup)。
* 在主控台的 [Helm 圖表型錄 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) 中，檢視您可以與 {{site.data.keyword.Bluemix_notm}} 搭配使用的可用 Helm 圖表。
* 進一步瞭解 <a href="https://docs.helm.sh/helm/" target="_blank">Helm 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 中有關用來設定及管理 Helm 圖表的 Helm 指令。
* 進一步瞭解如何[使用 Kubernetes Helm 圖表來加快部署 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/)。

## 視覺化 Kubernetes 叢集資源
{: #weavescope}

Weave Scope 提供 Kubernetes 叢集內的資源（包括服務、Pod、容器等等）的視覺圖。Weave Scope 提供 CPU 及記憶體的互動式度量值，以及對容器執行 tail 及 exec 的工具。
{:shortdesc}

開始之前：

-   請記住不要在公用網際網路上公開叢集資訊。請完成下列步驟，以安全地部署 Weave Scope，並於本端從 Web 瀏覽器進行存取。
-   如果您還沒有叢集，請[建立標準叢集](/docs/containers?topic=containers-clusters#clusters_ui)。Weave Scope 可能需要大量 CPU，特別是應用程式。請搭配執行 Weave Scope 與較大的標準叢集，而非免費叢集。
-  確定您具有所有名稱空間的 [**Manager** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)。
-   [登入您的帳戶。將目標設為適當的地區及（如果適用的話）資源群組。設定叢集的環境定義](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)。


若要搭配使用 Weave Scope 與叢集，請執行下列動作：
2.  在叢集裡部署其中一個提供的 RBAC 許可權配置檔。

    若要啟用讀寫許可權，請執行下列指令：

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    若要啟用唯讀許可權，請執行下列動作：

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    輸出：

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  部署 Weave Scope 服務（只能透過叢集 IP 位址進行私密存取）。

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    輸出：

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  在您的電腦上執行埠轉遞指令，以開啟服務。下次存取 Weave Scope 時，您就可以執行此埠轉遞指令，而不需要再次完成先前的配置步驟。

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    輸出：

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  開啟瀏覽器，並前往 `http://localhost:4040`。若未部署預設元件，您會看到下圖。您可以選擇檢視拓蹼圖，或叢集裡的 Kubernetes 資源表格。

     <img src="images/weave_scope.png" alt="來自 Weave Scope 的拓蹼範例" style="width:357px;" />


[進一步瞭解 Weave Scope 特性 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.weave.works/docs/scope/latest/features/)。

<br />

