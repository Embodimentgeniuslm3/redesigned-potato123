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


# 您使用 {{site.data.keyword.containerlong_notm}} 的責任
{: #responsibilities_iks}

瞭解使用 {{site.data.keyword.containerlong}} 時，您所擁有的叢集管理責任及條款。
{:shortdesc}

## 叢集管理責任
{: #responsibilities}

IBM 提供企業雲端平台，供您部署應用程式與 {{site.data.keyword.Bluemix_notm}} DevOps、AI、資料及安全服務。您可以選擇如何在雲端中設定、整合及操作應用程式和服務。
{:shortdesc}

<table summary="此表格顯示 IBM 及您的責任。列應該從左到右閱讀，第一欄為代表每個責任的圖示，第二欄為說明。">
<caption>IBM 及您的責任</caption>
  <thead>
  <th colspan=2>依類型的責任</th>
  </thead>
  <tbody>
    <tr>
    <td align="center"><img src="images/icon_clouddownload.svg" alt="箭頭指向下方的雲端圖示"/><br>雲端基礎架構</td>
    <td>
    **IBM 責任**：
    <ul><li>在安全的 IBM 擁有的基礎架構帳戶中，部署每個叢集的完整受管理、高可用性專用主節點。</li>
    <li>在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中佈建工作者節點。</li>
    <li>設定叢集管理元件，例如 VLAN 及負載平衡器。</li>
    <li>滿足更多基礎架構的要求，例如，新增及移除工作者節點、建立預設子網路，以及佈建儲存空間磁區以回應持續性磁區要求。</li>
    <li>整合已訂購的基礎架構資源以自動與叢集架構一起運作，並可用於已部署的應用程式及工作負載。</li></ul>
    <br><br>
    **您的責任**：
    <ul><li>使用提供的 API、CLI 或主控台工具來調整[運算](/docs/containers?topic=containers-clusters#clusters)及[儲存空間](/docs/containers?topic=containers-storage_planning#storage_planning)容量，並且調整[網路配置](/docs/containers?topic=containers-cs_network_cluster#cs_network_cluster)來符合您工作負載的需求。</li></ul><br><br>
    </td>
     </tr>
     <tr>
     <td align="center"><img src="images/icon_tools.svg" alt="板手的圖示"/><br>受管理叢集</td>
     <td>
     **IBM 責任**：
     <ul><li>提供一組工具來自動化叢集管理，例如 {{site.data.keyword.containerlong_notm}} [API ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://containers.cloud.ibm.com/global/swagger-global-api/)、[CLI 外掛程式](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)及[主控台 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/kubernetes/clusters)。</li>
     <li>自動套用 Kubernetes 主節點修補程式作業系統、版本及安全更新項目。讓主要和次要更新項目可供您套用。</li>
     <li>更新及回復叢集內的作業 {{site.data.keyword.containerlong_notm}} 及 Kubernetes 元件，例如 Ingress 應用程式負載平衡器及檔案儲存空間外掛程式。</li>
     <li>備份並回復 etcd 中的資料，例如 Kubernetes 工作負載配置檔。</li>
     <li>建立叢集時，設定主節點與工作者節點之間的 OpenVPN 連線。</li>
     <li>在各種介面中監視及報告主節點及工作者節點的性能。</li>
     <li>提供工作者節點主要、次要及修補程式作業系統、版本及安全更新項目。</li>
     <li>滿足自動化要求，以更新及回復工作者節點。提供選用的[工作者節點自動回復](/docs/containers?topic=containers-health#autorecovery)。</li>
     <li>提供[叢集 Autoscaler](/docs/containers?topic=containers-ca#ca) 這類工具來延伸叢集基礎架構。</li>
     </ul>
     <br><br>
     **您的責任**：
     <ul>
     <li>使用 API、CLI 或主控台工具來[套用](/docs/containers?topic=containers-update#update)提供的主要及次要 Kubernetes 主節點更新項目，以及主要、次要和修補程式工作者節點更新項目。</li>
     <li>使用 API、CLI 或主控台工具來[回復](/docs/containers?topic=containers-cs_troubleshoot#cs_troubleshoot)您的基礎架構資源，或設定並配置選用的[工作者節點自動回復](/docs/containers?topic=containers-health#autorecovery)。</li></ul>
     <br><br></td>
      </tr>
    <tr>
      <td align="center"><img src="images/icon_locked.svg" alt="鎖的圖示"/><br>高度安全的環境</td>
      <td>
      **IBM 責任**：
      <ul>
      <li>維護與[各種業界規範標準](/docs/containers?topic=containers-faqs#standards)相對應的控制項，例如 PCI DSS。</li>
      <li>監視、隔離及回復叢集主節點。</li>
      <li>提供 Kubernetes 主節點 API 伺服器、etcd、排程器及控制器管理程式元件的高可用性抄本，來防範主節點運作中斷。</li>
      <li>自動套用主要安全修補程式更新項目，並提供工作者節點安全修補程式更新項目。</li>
      <li>啟用特定安全設定，例如工作者節點上的加密磁碟。</li>
      <li>停用工作者節點的某些不安全動作，例如不允許使用者透過 SSH 連接至主機。</li>
      <li>使用 TLS 加密主節點與工作者節點之間的通訊。</li>
      <li>為工作者節點作業系統提供符合 CIS 標準的 Linux 映像檔。</li>
      <li>持續監視主節點及工作者節點映像檔，以偵測漏洞及安全規範問題。</li>
      <li>為工作者節點佈建兩個本端 SSD、AES 256 位元加密資料分割區。</li>
      <li>提供叢集網路連線功能的選項，例如公用及專用服務端點。</li>
      <li>提供運算隔離的選項，例如專用虛擬機器、裸機及具有授信運算的裸機。</li>
      <li>整合 Kubernetes 角色型存取控制 (RBAC) 與 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)。</li>
      </ul>
      <br><br>
      **您的責任**：
      <ul>
      <li>使用 API、CLI 或主控台工具，將提供的[安全修補程式更新項目](/docs/containers?topic=containers-changelog#changelog)套用至工作者節點。</li>
      <li>選擇如何設定[叢集網路](/docs/containers?topic=containers-plan_clusters)，並配置進一步的[安全設定](/docs/containers?topic=containers-security#security)，以符合您工作負載的安全及規範需求。適用的話，請配置[防火牆](/docs/containers?topic=containers-firewall#firewall)。</li></ul>
      <br><br></td>
      </tr>

      <tr>
        <td align="center"><img src="images/icon_code.svg" alt="程式碼方括弧的圖示"/><br>應用程式編排</td>
        <td>
        **IBM 責任**：
        <ul>
        <li>佈建已安裝 Kubernetes 元件的叢集，讓您可以存取 Kubernetes API。</li>
        <li>提供許多受管理附加程式，以延伸應用程式的功能，例如 [Istio](/docs/containers?topic=containers-istio#istio) 及 [Knative](/docs/containers?topic=containers-serverless-apps-knative)。為您簡化維護，因為 IBM 為受管理附加程式提供安裝及更新項目。</li>
        <li>提供叢集與精選協力廠商夥伴關係技術的整合，例如：{{site.data.keyword.la_short}}、{{site.data.keyword.mon_short}} 和 Portworx）的整合。</li>
        <li>提供自動化，以啟用與其他 {{site.data.keyword.Bluemix_notm}} 服務的服務連結。</li>
        <li>使用映像檔取回密碼來建立叢集，讓 `default` Kubernetes 名稱空間中的部署可以從 {{site.data.keyword.registrylong_notm}} 取回映像檔。</li>
        <li>提供儲存空間類別及外掛程式，以支援與應用程式搭配使用的持續性磁區。</li>
        <li>建立已保留子網路 IP 位址的叢集，以用來在外部公開應用程式。</li>
        <li>支援原生 Kubernetes 公用和專用負載平衡器及 Ingress 路徑，以在外部公開服務。</li>
        </ul>
        <br><br>
        **您的責任**：
        <ul>
        <li>使用提供的工具和特性來[配置及部署](/docs/containers?topic=containers-app#app)；[設定許可權](/docs/containers?topic=containers-users#users)；[與其他服務整合](/docs/containers?topic=containers-supported_integrations#supported_integrations)；[在外部提供](/docs/containers?topic=containers-cs_network_planning#cs_network_planning)；[監視性能](/docs/containers?topic=containers-health#health)；[儲存、備份及還原資料](/docs/containers?topic=containers-storage_planning#storage_planning)；以及管理[高可用性](/docs/containers?topic=containers-ha#ha)和具復原力的工作負載。</li>
        </ul>
        </td>
        </tr>
  </tbody>
  </table>

<br />


## {{site.data.keyword.containerlong_notm}} 濫用
{: #terms}

客戶不得誤用 {{site.data.keyword.containerlong_notm}}。
{:shortdesc}

誤用包括：

*   任何不合法的活動
*   散佈或執行惡意軟體
*   危害 {{site.data.keyword.containerlong_notm}} 或干擾任何人使用 {{site.data.keyword.containerlong_notm}}
*   危害或干擾任何人使用任何其他服務或系統
*   未經授權即存取任何服務或系統
*   未經授權即修改任何服務或系統
*   違反他人權利

如需整體使用條款，請參閱[雲端服務條款](/docs/overview/terms-of-use?topic=overview-terms#terms)。
