---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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

# 版本注意事項
{: #iks-release}

使用版本注意事項可瞭解對 {{site.data.keyword.containerlong}} 文件的最新變更，這些變更依月份分組。
{:shortdesc}

## 2019 年 6 月
{: #jun19}

<table summary="此表格顯示版本注意事項。列應該從左到右閱讀，第一欄為日期，第二欄為特性的標題，第三欄為說明。">
<caption>2019 年 6 月的 {{site.data.keyword.containerlong_notm}} 文件更新</caption>
<thead>
<th>日期</th>
<th>說明</th>
</thead>
<tbody>
<tr>
  <td>2019 年 6 月 7 日</td>
  <td><ul>
  <li><strong>透過專用服務端點存取 Kubernetes 主節點</strong>：已新增[步驟](/docs/containers?topic=containers-clusters#access_on_prem)，用於透過專用負載平衡器公開專用服務端點。完成這些步驟後，授權叢集使用者可以透過 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 連線來存取 Kubernetes 主節點。</li>
  <li><strong>{{site.data.keyword.BluDirectLink}}</strong>：向 [VPN 連線功能](/docs/containers?topic=containers-vpn)和[混合式雲端](/docs/containers?topic=containers-hybrid_iks_icp)頁面已新增 {{site.data.keyword.Bluemix_notm}} Direct Link，用於在遠端網路環境和 {{site.data.keyword.containerlong_notm}} 之間建立直接專用連線功能，而無需透過公用網際網路進行遞送。</li>
  <li><strong>Ingress ALB 變更日誌</strong>：已將 [ALB `ingress-auth` 映像檔更新至建置 330](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog)。</li>
  <li><strong>OpenShift 測試版</strong>：[已新增一個課程](/docs/containers?topic=containers-openshift_tutorial#openshift_logdna_sysdig)，用於說明如何修改 {{site.data.keyword.la_full_notm}} 和 {{site.data.keyword.mon_full_notm}} 附加程式的特許安全環境定義限制的應用程式部署。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 6 月 6 日</td>
  <td><ul>
  <li><strong>Fluentd 變更日誌</strong>：已新增 [Fluentd 版本變更日誌](/docs/containers?topic=containers-cluster-add-ons-changelog#fluentd_changelog)。</li>
  <li><strong>Ingress ALB 變更日誌</strong>：已將 [ALB `nginx-ingress` 映像檔更新至建置 470](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog)。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 6 月 5 日</td>
  <td><ul>
  <li><strong>CLI 參考資料</strong>：已更新 [CLI 參考資料頁面](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)，以反映 {{site.data.keyword.containerlong_notm}} CLI 外掛程式 [0.3.34 版](/docs/containers?topic=containers-cs_cli_changelog)的多項變更。</li>
  <li><strong>新推出！Red Hat OpenShift on IBM Cloud 叢集（測試版）</strong>：透過 Red Hat OpenShift on IBM Cloud 測試版，可以建立 {{site.data.keyword.containerlong_notm}} 叢集，其中包含隨 OpenShift 容器編排平台軟體一起安裝的工作者節點。您將取得叢集基礎架構環境的受管理 {{site.data.keyword.containerlong_notm}} 的所有優點，以及在 Red Hat Enterprise Linux 上執行以用於應用程式部署的 [OpenShift 工具和型錄 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.openshift.com/container-platform/3.11/welcome/index.html)。首先，請參閱[指導教學：建立 Red Hat OpenShift on IBM Cloud 叢集（測試版）](/docs/containers?topic=containers-openshift_tutorial)。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 6 月 4 日</td>
  <td><ul>
  <li><strong>版本變更日誌</strong>：已更新 [1.14.2_1521](/docs/containers?topic=containers-changelog#1142_1521)、[1.13.6_1524](/docs/containers?topic=containers-changelog#1136_1524)、[1.12.9_1555](/docs/containers?topic=containers-changelog#1129_1555) 和 [1.11.10_1561](/docs/containers?topic=containers-changelog#11110_1561) 修補程式版次的變更日誌。</li></ul>
  </td>
</tr>
<tr>
  <td>2019 年 6 月 3 日</td>
  <td><ul>
  <li><strong>自帶 Ingress 控制器</strong>：已更新[步驟](/docs/containers?topic=containers-ingress#user_managed)，以反映對預設社群控制器的變更，以及需要對多區域叢集裡的控制器 IP 位址進行性能檢查。</li>
  <li><strong>{{site.data.keyword.cos_full_notm}}</strong>：已更新[步驟](/docs/containers?topic=containers-object_storage#install_cos)，以安裝使用或不使用 Helm 伺服器 Tiller 的 {{site.data.keyword.cos_full_notm}} 外掛程式。</li>
  <li><strong>Ingress ALB 變更日誌</strong>：已將 [ALB `nginx-ingress` 映像檔更新至建置 467](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog)。</li>
  <li><strong>Kustomize</strong>：已新增[透過 Kustomize 在多個環境中重複使用 Kubernetes 配置檔](/docs/containers?topic=containers-app#kustomize)的範例。</li>
  <li><strong>Razee</strong>：向支援的整合已新增 [Razee ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/razee-io/Razee)，以直觀視覺化叢集裡的部署資訊以及自動部署 Kubernetes 資源。</li></ul>
  </td>
</tr>
</tbody></table>

## 2019 年 5 月
{: #may19}

<table summary="此表格顯示版本注意事項。列應該從左到右閱讀，第一欄為日期，第二欄為特性的標題，第三欄為說明。">
<caption>2019 年 5 月的 {{site.data.keyword.containerlong_notm}} 文件更新</caption>
<thead>
<th>日期</th>
<th>說明</th>
</thead>
<tbody>
<tr>
  <td>2019 年 5 月 30 日</td>
  <td><ul>
  <li><strong>CLI 參考資料</strong>：已更新 [CLI 參考資料頁面](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)，以反映 {{site.data.keyword.containerlong_notm}} CLI 外掛程式 [0.3.33 版](/docs/containers?topic=containers-cs_cli_changelog)的多項變更。</li>
  <li><strong>儲存空間的疑難排解</strong>：<ul>
  <li>已新增有關 [PVC 擱置狀態](/docs/containers?topic=containers-cs_troubleshoot_storage#file_pvc_pending)的檔案儲存空間和區塊儲存空間疑難排解主題。</li>
  <li>已新增有關[應用程式無法存取或寫入 PVC](/docs/containers?topic=containers-cs_troubleshoot_storage#block_app_failures) 時的區塊儲存空間疑難排解主題。</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 28 日</td>
  <td><ul>
  <li><strong>叢集附加程式變更日誌</strong>：已將 [ALB `nginx-ingress` 映像檔更新至建置 462](/docs/containers?topic=containers-cluster-add-ons-changelog)。</li>
  <li><strong>登錄的疑難排解</strong>：已新增[疑難排解主題](/docs/containers?topic=containers-cs_troubleshoot_clusters#ts_image_pull_create)，用於說明在叢集建立期間叢集由於發生錯誤而無法從 {{site.data.keyword.registryfull}} 取回映像檔的情況。</li>
  <li><strong>儲存空間的疑難排解</strong>：<ul>
  <li>已新增有關[除錯持續性儲存空間故障](/docs/containers?topic=containers-cs_troubleshoot_storage#debug_storage)的主題。</li>
  <li>已新增有關[由於遺漏許可權，PVC 建立失敗](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions)的疑難排解主題。</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 23 日</td>
  <td><ul>
  <li><strong>CLI 參考資料</strong>：已更新 [CLI 參考資料頁面](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)，以反映 {{site.data.keyword.containerlong_notm}} CLI 外掛程式 [0.3.28 版](/docs/containers?topic=containers-cs_cli_changelog)的多項變更。</li>
  <li><strong>叢集附加程式變更日誌</strong>：已將 [ALB `nginx-ingress` 映像檔更新至建置 457](/docs/containers?topic=containers-cluster-add-ons-changelog)。</li>
  <li><strong>叢集和工作者狀態</strong>：已更新[記載和監視頁面](/docs/containers?topic=containers-health#states)，以新增有關叢集和工作者節點狀態的參考表格。</li>
  <li><strong>叢集規劃和建立</strong>：現在，可以在下列頁面中尋找有關規劃、建立和移除叢集以及網路規劃的資訊：
    <ul><li>[規劃叢集網路設定](/docs/containers?topic=containers-plan_clusters)</li>
    <li>[規劃叢集的高可用性](/docs/containers?topic=containers-ha_clusters)</li>
    <li>[規劃工作者節點設定](/docs/containers?topic=containers-planning_worker_nodes)</li>
    <li>[建立叢集](/docs/containers?topic=containers-clusters)</li>
    <li>[將工作者節點及區域新增至叢集](/docs/containers?topic=containers-add_workers)</li>
    <li>[移除叢集](/docs/containers?topic=containers-remove)</li>
    <li>[變更服務端點或 VLAN 連線](/docs/containers?topic=containers-cs_network_cluster)</li></ul>
  </li>
  <li><strong>叢集版本更新</strong>：已更新[不支援的版本原則](/docs/containers?topic=containers-cs_versions)，以指出如果主節點版本比最舊的受支援版本低三個以上的次要版本，則無法更新叢集。可以透過在列出叢集時檢閱**狀態**欄位來檢視叢集是否**不受支援**。</li>
  <li><strong>Istio</strong>：已更新 [Istio 頁面](/docs/containers?topic=containers-istio)，以移除 Istio 在僅連接至專用 VLAN 的叢集裡不運作的限制。已新增[更新受管理附加程式主題](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons)的步驟，以在 Istio 管理的附加程式更新完成後，重新建立使用 TLS 區段的 Istio 閘道。</li>
  <li><strong>熱門主題</strong>：已將熱門主題取代為此版本注意事項頁面，此頁面包含 {{site.data.keyword.containershort_notm}} 特定的新特性和更新。如需 {{site.data.keyword.Bluemix_notm}} 產品的最新資訊，請查看[公告](https://www.ibm.com/cloud/blog/announcements)。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 20 日</td>
  <td><ul>
  <li><strong>版本變更日誌</strong>：已新增[工作者節點修正套件變更日誌](/docs/containers?topic=containers-changelog)。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 16 日</td>
  <td><ul>
  <li><strong>CLI 參考資料</strong>：已更新 [CLI 參考資料頁面](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)，以新增用於 `logging-collect` 指令的 COS 端點，並且說明 `apiserver-refresh` 用於重新啟動 Kubernetes 主節點元件。</li>
  <li><strong>不受支援：Kubernetes 1.10 版</strong>：[Kubernetes 1.10 版](/docs/containers?topic=containers-cs_versions#cs_v114)現在不受支援。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 15 日</td>
  <td><ul>
  <li><strong>預設 Kubernetes 版本</strong>：預設 Kubernetes 版本現在為 1.13.6。</li>
  <li><strong>服務限制</strong>：已新增[服務限制主題](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#tech_limits)。</li>
  </ul></td>
</tr>
<tr>
  <td>2019 年 5 月 13 日</td>
  <td><ul>
  <li><strong>版本變更日誌</strong>：已新增可用於 [1.14.1_1518](/docs/containers?topic=containers-changelog#1141_1518)、[1.13.6_1521](/docs/containers?topic=containers-changelog#1136_1521)、[1.12.8_1552](/docs/containers?topic=containers-changelog#1128_1552)、[1.11.10_1558](/docs/containers?topic=containers-changelog#11110_1558) 和 [1.10.13_1558](/docs/containers?topic=containers-changelog#11013_1558) 的新修補程式更新。</li>
  <li><strong>工作者節點特性</strong>：已移除每種[雲端狀態 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/status?component=containers-kubernetes&selected=status) 48 個以上核心的所有[虛擬機器工作者節點特性](/docs/containers?topic=containers-planning_worker_nodes#vm)。您仍可以佈建使用 48 個以上核心的[Bare Metal Server 工作者節點](/docs/containers?topic=containers-plan_clusters#bm)。</li></ul></td>
</tr>
<tr>
  <td>2019 年 5 月 8 日</td>
  <td><ul>
  <li><strong>API</strong>：已新增[廣域 API Swagger 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://containers.cloud.ibm.com/global/swagger-global-api/#/) 的鏈結。</li>
  <li><strong>Cloud Object Storage</strong>：在 {{site.data.keyword.containerlong_notm}} 叢集裡[已新增有關 Cloud Object Storage 的疑難排解手冊](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_pvc_pending)。</li>
  <li><strong>Kubernetes 策略</strong>：已新增有關[在將應用程式移至 {{site.data.keyword.containerlong_notm}} 之前，最好具備哪些知識和技術技能？](/docs/containers?topic=containers-strategy#knowledge)的主題。</li>
  <li><strong>Kubernetes 1.14 版</strong>：已新增經過認證的 [Kubernetes 1.14 版](/docs/containers?topic=containers-cs_versions#cs_v114)。</li>
  <li><strong>參考主題</strong>：已更新[使用者存取權](/docs/containers?topic=containers-access_reference)和 [CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) 參考頁面中各種服務連結、`logging` 和 `nlb` 作業的資訊。</li></ul></td>
</tr>
<tr>
  <td>2019 年 5 月 7 日</td>
  <td><ul>
  <li><strong>叢集 DNS 提供者</strong>：[說明 CoreDNS 的優點](/docs/containers?topic=containers-cluster_dns)，現在執行 Kubernetes 1.14 和更高版本的叢集僅支援 CoreDNS。</li>
  <li><strong>邊緣節點</strong>：已新增對[邊緣節點](/docs/containers?topic=containers-edge)的專用負載平衡器支援。</li>
  <li><strong>免費叢集</strong>：說明支援[免費叢集](/docs/containers?topic=containers-regions-and-zones#regions_free)的位置。</li>
  <li><strong>新推出！整合</strong>：新增並重組有關 [{{site.data.keyword.Bluemix_notm}} 服務和協力廠商整合](/docs/containers?topic=containers-ibm-3rd-party-integrations)、[常用整合](/docs/containers?topic=containers-supported_integrations)和[合作夥伴關係](/docs/containers?topic=containers-service-partners)的資訊。</li>
  <li><strong>新推出！Kubernetes 1.14 版</strong>：建立叢集或將叢集更新為 [Kubernetes 1.14](/docs/containers?topic=containers-cs_versions#cs_v114)。</li>
  <li><strong>淘汰使用 Kubernetes 1.11 版</strong>：在不支援 [Kubernetes 1.11](/docs/containers?topic=containers-cs_versions#cs_v111) 之前，[更新](/docs/containers?topic=containers-update)執行 Kubernetes 1.11 的所有叢集。</li>
  <li><strong>許可權</strong>：已新增常見問題 - [我能為叢集使用者提供哪些存取原則？](/docs/containers?topic=containers-faqs#faq_access)</li>
  <li><strong>工作者節點儲存區</strong>：已新增有關如何[將標籤套用於現有工作者節點儲存區](/docs/containers?topic=containers-add_workers#worker_pool_labels)的指示。</li>
  <li><strong>參考主題</strong>：為支援新特性，例如 Kubernetes 1.14，已更新[變更日誌](/docs/containers?topic=containers-changelog#changelog)參考頁面。</li></ul></td>
</tr>
<tr>
  <td>2019 年 5 月 1 日</td>
  <td><strong>指派基礎架構存取權</strong>：已修訂[指派開立支援案例的 IAM 許可權的步驟](/docs/containers?topic=containers-users#infra_access)。</td>
</tr>
</tbody></table>


