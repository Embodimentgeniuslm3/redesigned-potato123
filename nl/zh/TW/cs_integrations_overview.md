---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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
{:preview: .preview}f


# 支援的 IBM Cloud 和協力廠商整合
{: #supported_integrations}

在 {{site.data.keyword.containerlong}} 中，您可以使用各種外部服務及型錄服務來與標準 Kubernetes 叢集搭配使用。
{:shortdesc}

## 常用整合
{: #popular_services}

<table summary="該表格顯示了可以新增到叢集且 {{site.data.keyword.containerlong_notm}} 使用者常用的可用服務。每行從左到右閱讀，其中第一直欄是服務名稱，第二直欄是服務說明。">
<caption>常用服務</caption>
<thead>
<tr>
<th>服務</th>
<th>種類</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>叢集活動日誌</td>
<td>透過 Grafana 分析日誌，來監視叢集裡所做的管理活動。如需服務的相關資訊，請參閱 [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) 文件。如需您可追蹤之事件類型的相關資訊，請參閱 [Activity Tracker 事件](/docs/containers?topic=containers-at_events)。</td>
</tr>
<tr>
<td>{{site.data.keyword.appid_full}}</td>
<td>鑑別</td>
<td>要求使用者登入，以 [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) 新增應用程式的安全層次。若要鑑別針對您應用程式的 API HTTP/HTTPS 要求，您可以使用 [{{site.data.keyword.appid_short_notm}} 鑑別 Ingress 註釋](/docs/containers?topic=containers-ingress_annotation#appid-auth)，來整合 {{site.data.keyword.appid_short_notm}} 與 Ingress 服務。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} Block Storage</td>
<td>區塊儲存空間</td>
<td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) 是一種持續性的高效能 iSCSI 儲存空間，可以使用 Kubernetes 持續性磁區 (PV) 將其新增至應用程式。使用區塊儲存空間以在單一區域中部署有狀態應用程式，或作為單一 Pod 的高效能儲存空間。如需如何在叢集裡佈建區塊儲存空間的相關資訊，請參閱[在 {{site.data.keyword.Bluemix_notm}} Block Storage 上儲存資料](/docs/containers?topic=containers-block_storage#block_storage)</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>TLS 憑證</td>
<td>您可以使用 <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來儲存及管理應用程式的 SSL 憑證。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">使用 {{site.data.keyword.cloudcerts_long_notm}} 與 {{site.data.keyword.containerlong_notm}} 搭配來部署自訂網域 TLS 憑證 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.registrylong}}</td>
<td>容器映像檔</td>
<td>設定您自己的安全 Docker 映像檔儲存庫，您可以在其中安全地儲存映像檔，以及在叢集使用者之間共用映像檔。如需相關資訊，請參閱 <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}} 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>建置自動化</td>
<td>使用工具鏈自動建置應用程式，並將容器部署至 Kubernetes 叢集。如需設定的相關資訊，請參閱部落格 <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.datashield_full}}（測試版）</td>
<td>記憶體加密</td>
<td>您可以使用 <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來加密資料記憶體。{{site.data.keyword.datashield_short}} 與 Intel® Software Guard Extensions (SGX) 及 Fortanix® 技術整合，以在使用時保護 {{site.data.keyword.Bluemix_notm}} 容器工作負載程式碼和資料。應用程式碼及資料是在 CPU 強化的區域中執行，這些是工作者節點上的記憶體信任區域，能保護應用程式的重要方面，有助於保持程式碼和資料的機密性，並使其不會遭到修改。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} File Storage</td>
<td>檔案儲存空間</td>
<td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) 是一種持續性、快速且具彈性的網路連結的 NFS 型檔案儲存空間，可以使用 Kubernetes 持續性磁區將其新增至應用程式。您可以從具有符合您工作負載需求之 GB 大小及 IOPS 的預先定義儲存空間層級中進行選擇。如需如何在叢集裡佈建檔案儲存空間的相關資訊，請參閱[在 {{site.data.keyword.Bluemix_notm}} File Storage 上儲存資料](/docs/containers?topic=containers-file_storage#file_storage)。</td>
</tr>
<tr>
<td>{{site.data.keyword.keymanagementservicefull}}</td>
<td>資料加密</td>
<td>啟用 {{site.data.keyword.keymanagementserviceshort}} 來加密位於叢集裡的 Kubernetes 密碼。加密 Kubernetes 密碼，可防止未獲授權的使用者存取機密叢集資訊。<br>若要設定，請參閱<a href="/docs/containers?topic=containers-encryption#keyprotect">使用 {{site.data.keyword.keymanagementserviceshort}} 加密 Kubernetes 密碼</a>。<br>如需相關資訊，請參閱 <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full}}</td>
<td>叢集和應用程式日誌</td>
<td>將日誌管理功能新增至叢集，方法為將 LogDNA 作為協力廠商服務部署至工作者節點，以管理來自 Pod 容器的日誌。如需相關資訊，請參閱[使用 {{site.data.keyword.loganalysisfull_notm}} 搭配 LogDNA 來管理 Kubernetes 叢集日誌](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube)。</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full}}</td>
<td>叢集和應用程式度量</td>
<td>取得應用程式效能及性能的作業可見性，方法為將 Sysdig 作為協力廠商服務部署至工作者節點，以將度量轉遞至 {{site.data.keyword.monitoringlong}}。如需相關資訊，請參閱[分析在 Kubernetes 叢集裡部署之應用程式的度量](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster)。</td>
</tr>
<tr>
<td>{{site.data.keyword.cos_full}}</td>
<td>物件儲存空間</td>
<td>與 {{site.data.keyword.cos_short}} 一起儲存的資料會加密及分散在多個地理位置，並使用 REST API 透過 HTTP 進行存取。您可以使用 [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) 來配置服務，以針對叢集裡的資料進行一次性或排定的備份。如需服務的相關資訊，請參閱 <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">{{site.data.keyword.cos_short}} 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Istio on {{site.data.keyword.containerlong_notm}}</td>
<td>微服務管理</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 是一種開放程式碼服務，提供方法讓開發人員連接、保護、管理及監視雲端編排平台上微服務（也稱為服務網）的網路。Istio on {{site.data.keyword.containerlong}} 透過受管理附加程式，對叢集提供 Istio 單步驟安裝。只要按一下，您就可以取得所有 Istio 核心元件、額外的追蹤、監視和視覺化，以及啟動並執行 BookInfo 範例應用程式。若要開始使用，請參閱[使用受管理 Istio 附加程式（測試版）](/docs/containers?topic=containers-istio)。</td>
</tr>
<tr>
<td>Knative</td>
<td>無伺服器應用程式</td>
<td>[Knative ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/knative/docs) 是 IBM、Google、Pivotal、Red Hat、Cisco 及其他公司所開發的開放程式碼平台，目標是擴充 Kubernetes 功能，以協助您在 Kubernetes 叢集之上建立現代、以原始檔為主的容器化和無伺服器應用程式。此平台在各程式設計語言及架構之間使用一致的方式，以摘要建置、部署及管理 Kubernetes 中工作負載的作業負載，讓開發人員可以專注於最重要的事物：原始碼。如需相關資訊，請參閱[使用 Knative 部署無伺服器應用程式](/docs/containers?topic=containers-serverless-apps-knative)。</td>
</tr>
<tr>
<td>Portworx</td>
<td>用於有狀態應用程式的儲存空間</td>
<td>[Portworx ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://portworx.com/products/introduction/) 是一個高可用性軟體定義儲存空間解決方案，可用來管理容器化資料庫及其他有狀態應用程式的持續性儲存空間，或在多個區域的 Pod 之間共用資料。您可以使用 Helm 圖表來安裝 Portworx，以及使用 Kubernetes 持續性磁區來佈建應用程式的儲存空間。如需如何在叢集裡設定 Portworx 的相關資訊，請參閱[使用 Portworx 將資料儲存在軟體定義儲存空間 (SDS)](/docs/containers?topic=containers-portworx#portworx)。</td>
</tr>
<tr>
<td>Razee</td>
<td>部署自動化</td>
<td>[Razee ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://razee.io/) 是開放程式碼的專案，它會將各叢集、環境和雲端提供者之間的 Kubernetes 資源部署自動化並予以管理，並協助您視覺化資源的部署資訊，以便您可以監視推出處理程序，並更快速地找到部署問題。如需 Razee 的相關資訊，以及如何在叢集裡設定 Razee 來將部署處理程序自動化，請參閱 [Razee 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/razee-io/Razee)。</td>
</tr>
</tbody>
</table>

<br />


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
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 是 Kubernetes 套件管理程式。您可以建立新的 Helm 圖表或使用預先存在的 Helm 圖表，來定義、安裝及升級在 {{site.data.keyword.containerlong_notm}} 叢集裡執行的複式 Kubernetes 應用程式。<p>如需相關資訊，請參閱[在 {{site.data.keyword.containerlong_notm}} 中設定 Helm](/docs/containers?topic=containers-helm)。</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>使用工具鏈自動建置應用程式，並將容器部署至 Kubernetes 叢集。如需設定的相關資訊，請參閱部落格 <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
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
<td>[Knative ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/knative/docs) 是 IBM、Google、Pivotal、Red Hat、Cisco 及其他公司所開發的開放程式碼平台，目標是擴充 Kubernetes 功能，以協助您在 Kubernetes 叢集之上建立現代、以原始檔為主的容器化和無伺服器應用程式。此平台在各程式設計語言及架構之間使用一致的方式，以摘要建置、部署及管理 Kubernetes 中工作負載的作業負載，讓開發人員可以專注於最重要的事物：原始碼。如需相關資訊，請參閱[使用 Knative 部署無伺服器應用程式](/docs/containers?topic=containers-serverless-apps-knative)。</td>
</tr>
<tr>
<td>Razee</td>
<td>[Razee ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://razee.io/) 是開放程式碼的專案，它會將各叢集、環境和雲端提供者之間的 Kubernetes 資源部署自動化並予以管理，並協助您視覺化資源的部署資訊，以便您可以監視推出處理程序，並更快速地找到部署問題。如需 Razee 的相關資訊，以及如何在叢集裡設定 Razee 來將部署處理程序自動化，請參閱 [Razee 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/razee-io/Razee)。</td>
</tr>
</tbody>
</table>

<br />


## 混合式雲端服務
{: #hybrid_cloud_services}

<table summary="該表格顯示了可以將叢集連接至內部部署資料中心的可用服務。每行從左到右閱讀，其中第一直欄是服務名稱，第二直欄是服務說明。">
<caption>混合式雲端服務</caption>
<thead>
<tr>
<th>服務</th>
<th>說明</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.BluDirectLink}}</td>
    <td>[{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link) 容許您在遠端網路環境和 {{site.data.keyword.containerlong_notm}} 之間建立直接專用連線，而無需透過公用網際網路進行遞送。{{site.data.keyword.Bluemix_notm}} Direct Link 供應項目適用於您必須實作混合式工作負載、跨提供者工作負載、大型或頻繁的資料傳送，或是專用工作負載等情況。若要選擇 {{site.data.keyword.Bluemix_notm}} Direct Link 供應項目並設定 {{site.data.keyword.Bluemix_notm}} Direct Link 連線，請參閱 {{site.data.keyword.Bluemix_notm}} Direct Link 文件中的[開始使用 {{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-)。</td>
  </tr>
<tr>
  <td>strongSwan IPSec VPN 服務</td>
  <td>設定 [strongSwan IPSec VPN 服務 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.strongswan.org/about.html)，以將 Kubernetes 叢集與內部部署網路安全連接。在根據業界標準網際網路通訊協定安全 (IPSec) 通訊協定套組的網際網路上，strongsWan IPSec VPN 服務提供安全的端對端通訊通道。若要設定叢集與內部部署網路之間的安全連線，請直接在叢集的 Pod 中[配置及部署 strongSwan IPSec VPN 服務](/docs/containers?topic=containers-vpn#vpn-setup)。</td>
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
<td>透過 Grafana 分析日誌，來監視叢集裡所做的管理活動。如需服務的相關資訊，請參閱 [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) 文件。如需您可追蹤之事件類型的相關資訊，請參閱 [Activity Tracker 事件](/docs/containers?topic=containers-at_events)。</td>
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
<td>Prometheus 是一個針對 Kubernetes 所設計的開放程式碼監視、記載及警示工具。Prometheus 會根據 Kubernetes 記載資訊來擷取叢集、工作者節點及部署性能的詳細資訊。針對叢集裡執行中的每個容器，都會收集 CPU、記憶體、I/O 和網路活動。您可以使用自訂查詢或警示中所收集的資料，來監視叢集裡的效能及工作負載。

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
<td>[Weave Scope ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.weave.works/oss/scope/) 提供了 Kubernetes 叢集內資源（包括服務、Pod、容器、處理程序、節點等等）的視覺化圖表。Weave Scope 提供 CPU 及記憶體的互動式度量值，也提供工具來調整並執行至容器。
</li></ol>
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
  <tr>
    <td>{{site.data.keyword.appid_full}}</td>
    <td>要求使用者登入，以 [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started) 新增應用程式的安全層次。若要鑑別針對您應用程式的 API HTTP/HTTPS 要求，您可以使用 [{{site.data.keyword.appid_short_notm}} 鑑別 Ingress 註釋](/docs/containers?topic=containers-ingress_annotation#appid-auth)，來整合 {{site.data.keyword.appid_short_notm}} 與 Ingress 服務。</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>作為 <a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a> 的補充，您可以使用 <a href="https://www.aquasec.com/" target="_blank">Aqua Security<img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>，透過減少容許應用程式執行的內容來改善容器部署的安全。如需相關資訊，請參閱<a href="https://blog.aquasec.com/securing-container-deployments-on-bluemix-with-aqua-security" target="_blank">使用 Aqua Security 保護 {{site.data.keyword.Bluemix_notm}} 上的容器部署 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>您可以使用 <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來儲存及管理應用程式的 SSL 憑證。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">使用 {{site.data.keyword.cloudcerts_long_notm}} 與 {{site.data.keyword.containerlong_notm}} 搭配來部署自訂網域 TLS 憑證 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
  <td>{{site.data.keyword.datashield_full}}（測試版）</td>
  <td>您可以使用 <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來加密資料記憶體。{{site.data.keyword.datashield_short}} 與 Intel® Software Guard Extensions (SGX) 及 Fortanix® 技術整合，以在使用時保護 {{site.data.keyword.Bluemix_notm}} 容器工作負載程式碼和資料。應用程式碼及資料是在 CPU 強化的區域中執行，這些是工作者節點上的記憶體信任區域，能保護應用程式的重要方面，有助於保持程式碼和資料的機密性，並使其不會遭到修改。</td>
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
<td>作為 <a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a> 的補充，您可以使用 <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來管理防火牆、威脅保護及突發事件回應。如需相關資訊，請參閱 <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">{{site.data.keyword.containerlong_notm}} 上的 Twistlock <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
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
  <td>[{{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) 是一種持續性的高效能 iSCSI 儲存空間，可以使用 Kubernetes 持續性磁區 (PV) 將其新增至應用程式。使用區塊儲存空間以在單一區域中部署有狀態應用程式，或作為單一 Pod 的高效能儲存空間。如需如何在叢集裡佈建區塊儲存空間的相關資訊，請參閱[在 {{site.data.keyword.Bluemix_notm}} Block Storage 上儲存資料](/docs/containers?topic=containers-block_storage#block_storage)</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>與 {{site.data.keyword.cos_short}} 一起儲存的資料會加密及分散在多個地理位置，並使用 REST API 透過 HTTP 進行存取。您可以使用 [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) 來配置服務，以針對叢集裡的資料進行一次性或排定的備份。如需服務的相關資訊，請參閱 <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">{{site.data.keyword.cos_short}} 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
  <tr>
  <td>{{site.data.keyword.Bluemix_notm}} File Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) 是一種持續性、快速且具彈性的網路連結的 NFS 型檔案儲存空間，可以使用 Kubernetes 持續性磁區將其新增至應用程式。您可以從具有符合您工作負載需求之 GB 大小及 IOPS 的預先定義儲存空間層級中進行選擇。如需如何在叢集裡佈建檔案儲存空間的相關資訊，請參閱[在 {{site.data.keyword.Bluemix_notm}} File Storage 上儲存資料](/docs/containers?topic=containers-file_storage#file_storage)。</td>
  </tr>
  <tr>
    <td>Portworx</td>
    <td>[Portworx ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://portworx.com/products/introduction/) 是一個高可用性軟體定義儲存空間解決方案，可用來管理容器化資料庫及其他有狀態應用程式的持續性儲存空間，或在多個區域的 Pod 之間共用資料。您可以使用 Helm 圖表來安裝 Portworx，以及使用 Kubernetes 持續性磁區來佈建應用程式的儲存空間。如需如何在叢集裡設定 Portworx 的相關資訊，請參閱[使用 Portworx 將資料儲存在軟體定義儲存空間 (SDS)](/docs/containers?topic=containers-portworx#portworx)。</td>
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
  <td>可以在各種 {{site.data.keyword.Bluemix_notm}} 資料庫服務（如 {{site.data.keyword.composeForMongoDB_full}} 或 {{site.data.keyword.cloudantfull}}）之間進行選擇，以在叢集裡部署高度可用、可調整的資料庫解決方案。如需可用的雲端資料庫清單，請參閱 [{{site.data.keyword.Bluemix_notm}} 型錄 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/catalog?category=databases)。</td>
  </tr>
  </tbody>
  </table>
