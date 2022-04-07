---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, multi az, multi-az, szr, mzr

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


# 規劃叢集網路設定
{: #plan_clusters}

為 {{site.data.keyword.containerlong}} 叢集設計網路設定，以符合工作負載和環境的需求。
{: shortdesc}

在 {{site.data.keyword.containerlong_notm}} 叢集裡，容器化應用程式在稱為工作者節點的計算主機上管理。工作者節點是由 Kubernetes 主節點管理。工作者節點與 Kubernetes 主節點、其他服務、網際網路或其他專用網路之間的通訊設定取決於如何設定 IBM Cloud 基礎架構 (SoftLayer) 網路。

是否首次建立叢集？請首先試用[指導教學](/docs/containers?topic=containers-cs_cluster_tutorial)，然後在準備好規劃正式作業就緒型叢集後回到這裡。
{: tip}

若要規劃叢集網路設定，請首先[瞭解叢集網路基本資訊](#plan_basics)。然後，可以檢閱適用於以環境為基礎之情境的三個潛在叢集網路設定，包括[執行面對網際網路的應用程式工作負載](#internet-facing)、[使用有限公用存取權延伸內部部署資料中心](#limited-public)和[僅在專用網路上延伸內部部署資料中心](#private_clusters)。

## 瞭解叢集網路基本資訊
{: #plan_basics}

建立叢集時，必須選擇網路連線功能設定，以便特定叢集元件可以相互通訊，並且能與叢集外部的網路或服務進行通訊。
{: shortdesc}

* [工作者節點到工作者節點的通訊](#worker-worker)：所有工作者節點都必須能夠在專用網路上相互通訊。在許多情況下，必須容許跨多個專用 VLAN 進行通訊，使其在不同 VLAN 和不同區域上的工作者節點相互連接。
* [工作者節點到主節點的通訊以及使用者到主節點的通訊](#workeruser-master)：工作者節點和授權叢集使用者可以透過 TLS 在公用網路上或透過專用服務端點在專用網路上安全地與 Kubernetes 主節點進行通訊。
* [工作者節點與其他 {{site.data.keyword.Bluemix_notm}} 服務或內部部署網路的通訊](#worker-services-onprem)：容許工作者節點安全地與其他 {{site.data.keyword.Bluemix_notm}} 服務（例如，{{site.data.keyword.registrylong}}）以及與內部部署網路進行通訊。
* [與在工作者節點上執行的應用程式的外部通訊](#external-workers)：容許向叢集發出公用或專用要求，以及從叢集向公用端點發出要求。

### 工作者節點到工作者節點的通訊
{: #worker-worker}

建立叢集時，叢集的工作者節點會自動連接至專用 VLAN，並且可以選擇連接至公用 VLAN。VLAN 會將一群組工作者節點和 Pod 視為連線功能到相同實體佈線那樣進行配置，並為工作者節點之間的連線功能提供頻道。
{: shortdesc}

**工作者節點的 VLAN 連線**</br>
所有工作者節點都必須連接至專用 VLAN，以便每個工作者節點都可以向其他工作者節點傳送資訊以及接收來自其他工作者節點的資訊。建立具有同時連接至公用 VLAN 的工作者節點的叢集時，工作者節點可以透過公用 VLAN 和專用 VLAN（如果啟用了專用服務端點）自動與 Kubernetes 主節點進行通訊。公用 VLAN 還會提供公用網路連線功能，以便可以將叢集裡的應用程式公開到網際網路。但是，如果需要保護應用程式不被公用介面存取，則有若干選項可用於保護叢集，例如使用 Calico 網路原則或將外部網路工作負載隔離到邊緣工作者節點。
* 免費叢集：在免費叢集裡，依預設叢集的工作者節點會連接至 IBM 擁有的公用 VLAN 和專用 VLAN。由於 IBM 控制了 VLAN、子網路及 IP 位址，所以您無法建立多區域叢集，或將子網路新增至您的叢集，只能使用 NodePort 服務來公開您的應用程式。</dd>
* 標準叢集：在標準叢集裡，首次在某個區域中建立叢集時，會自動在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中佈建該區域中的公用 VLAN 和專用 VLAN。如果指定工作者節點必須僅連接至專用 VLAN，則會將自動佈建該區域中的專用 VLAN。配對於在該區域中建立的每個後續叢集，可以指定要使用的 VLAN 配對。您可以重複使用為您所建立的相同公用及專用 VLAN，因為多個叢集可以共用 VLAN。

如需 VLAN、子網路和 IP 位址的相關資訊，請參閱 [{{site.data.keyword.containerlong_notm}} 中的網路連線功能概觀](/docs/containers?topic=containers-subnets#basics)。

**工作者節點跨子網路和 VLAN 進行通訊**</br>
在各種狀況下，必須允許叢集裡的元件跨多個專用 VLAN 進行通訊。例如，如果您要建立多區域叢集、某個叢集有多個 VLAN，或在相同的 VLAN 上有多個子網路，則相同 VLAN 的不同子網路上的工作者節點或是不同 VLAN 上的工作者節點無法自動彼此通訊。您必須針對 IBM Cloud 基礎架構 (SoftLayer) 帳戶啟用虛擬遞送及轉遞 (VRF) 或 VLAN Spanning。

* [虛擬路由和轉遞 (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)：VRF 可讓基礎架構帳戶中的所有專用 VLAN 和子網路相互通訊。此外，需要 VRF 來容許工作者節點與主節點透過專用服務端點進行通訊，以及與支援專用服務端點的其他 {{site.data.keyword.Bluemix_notm}} 實例進行通訊。若要啟用 VRF，請執行 `ibmcloud account update --service-endpoint-enable true`。此指令輸出將提示您開立支援案例，以便為帳戶啟用 VRF 和服務端點。VRF 會使帳戶的 VLAN Spanning 選項失效，因為所有 VLAN 都能夠進行通訊。</br></br>已啟用 VRF 後，任何連接至相同 {{site.data.keyword.Bluemix_notm}} 帳戶中的任何專用 VLAN 的系統都可以與叢集工作者節點進行通訊。您可以套用 [Calico 專用網路原則](/docs/containers?topic=containers-network_policies#isolate_workers)，將叢集與專用網路上的其他系統隔離。</dd>
* [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)：如果無法或者不希望啟用 VRF（例如，如果不需要主節點在專用網路上可供存取，或者如果是使用閘道裝置透過公用 VLAN 來存取主節點），請啟用 VLAN Spanning。例如，如果您有現有閘道裝置，然後新增了叢集，則不會在該閘道裝置上配置為叢集訂購的新可攜式子網路，但 VLAN Spanning 會啟用子網路之間的路由。若要啟用 VLAN Spanning，您需要**網路 > 管理網路 VLAN Spanning**[基礎架構許可權](/docs/containers?topic=containers-users#infra_access)，或者可以要求帳戶擁有者來啟用 VLAN Spanning 。若要檢查是否已啟用 VLAN Spanning，請使用 `ibmcloud ks vlan-spanning-get` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。如果選擇啟用 VLAN Spanning 而不是 VRF，則無法啟用專用服務端點。

</br>

### 工作者節點到主節點的通訊以及使用者到主節點的通訊
{: #workeruser-master}

必須設定通訊通道，以便工作者節點可以與 Kubernetes 主節點建立連線。您可以啟用僅限公用服務端點、公用和專用服務端點或僅限專用服務端點，來容許工作者節點與 Kubernetes 主節點進行通訊。
{: shortdesc}

為了保護透過公用和專用服務端點進行的通訊，在建立叢集時，{{site.data.keyword.containerlong_notm}} 會自動設定 Kubernetes 主節點與工作者節點之間的 OpenVPN 連線。工作者節點安全地透過 TLS 憑證與主節點交談，而且主節點透過 OpenVPN 連線與工作者節點交談。

**僅限公用服務端點**</br>
如果您不希望或無法為帳戶啟用 VRF，則工作者節點可以透過公用服務端點在公用 VLAN 上自動連接至 Kubernetes 主節點。
* 工作者節點與主節點之間的通訊，是使用公用服務端點，透過公用網路安全地建立。
* 授權叢集使用者只能透過公用服務端點公開存取主節點。例如，叢集使用者可以透過網際網路安全地存取 Kubernetes 主節點，以執行 `kubectl` 指令。

**公用和專用服務端點**</br>
若要讓主節點可供叢集使用者公開或專用存取，您可以啟用公用和專用服務端點。在 {{site.data.keyword.Bluemix_notm}} 帳戶中需要 VRF，且您必須啟用帳戶以使用服務端點。若要啟用 VRF 及服務端點，請執行 `ibmcloud account update --service-endpoint-enable true`。
* 如果工作者節點連接至公用和專用 VLAN，則工作者節點與主節點之間的通訊將使用專用服務端點以透過專用網路建立通訊，以及使用公用服務端點以透過公用網路建立通訊。透過公用端點遞送一半的工作者節點到主節點資料流量，並透過專用端點遞送另一半，可防範主節點到工作者節點通訊發生公用或專用網路的中斷。如果工作者節點僅連接至專用 VLAN，則工作者節點與主節點之間的通訊將僅使用專用服務端點透過專用網路建立。
* 授權叢集使用者可透過公用服務端點公開存取主節點。如果授權叢集使用者位於 {{site.data.keyword.Bluemix_notm}} 專用網路中，或透過 VPN 連線或 {{site.data.keyword.Bluemix_notm}} Direct Link 連接至專用網路，則可以透過專用服務端點私下存取主節點。請注意，您必須[透過專用負載平衡器公開主節點端點](/docs/containers?topic=containers-clusters#access_on_prem)，以便使用者可以透過 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 連線存取主節點。

**僅專用服務端點**</br>
若要使主節點僅可供專用存取，可以啟用專用服務端點。在 {{site.data.keyword.Bluemix_notm}} 帳戶中需要 VRF，且您必須啟用帳戶以使用服務端點。若要啟用 VRF 及服務端點，請執行 `ibmcloud account update --service-endpoint-enable true`。請注意，使用僅專用服務端點不會發生任何計費或計量的頻寬計費。
* 工作者節點與主節點之間的通訊，是使用專用服務端點，透過專用網路建立。
* 如果授權叢集使用者位於 {{site.data.keyword.Bluemix_notm}} 專用網路中，或者透過 VPN 連線或 DirectLink 連接至專用網路，則可以私下存取主節點。請注意，您必須[透過專用負載平衡器公開主節點端點](/docs/containers?topic=containers-clusters#access_on_prem)，以便使用者可以透過 VPN 或 DirectLink 連線存取主節點。

</br>

### 工作者節點與其他 {{site.data.keyword.Bluemix_notm}} 服務或內部部署網路的通訊
{: #worker-services-onprem}

容許工作者節點安全地與其他 {{site.data.keyword.Bluemix_notm}} 服務（例如，{{site.data.keyword.registrylong}}）以及與內部部署網路進行通訊。
{: shortdesc}

**與其他 {{site.data.keyword.Bluemix_notm}} 服務透過專用或公用網路進行通訊**</br>
工作者節點可以透過 IBM Cloud 基礎架構 (SoftLayer) 專用網路，自動、安全地與支援專用服務端點的其他 {{site.data.keyword.Bluemix_notm}} 服務（例如 {{site.data.keyword.registrylong}}）進行通訊。如果 {{site.data.keyword.Bluemix_notm}} 服務不支援專用服務端點，則工作者節點必須連接至公用 VLAN，這樣才能透過公用網路安全地與服務進行通訊。

如果使用 Calico 網路原則來鎖定叢集裡的公用網路，則可能需要容許存取要在 Calico 原則中使用的服務的公用和專用 IP 位址。如果使用的是閘道裝置，例如虛擬路由器應用裝置 (Vyatta)，則必須[容許存取要使用的服務的專用 IP 位址](/docs/containers?topic=containers-firewall#firewall_outbound)（這是要在閘道裝置防火牆中使用的服務）。
{: note}

**使用 {{site.data.keyword.BluDirectLink}} 透過專用網路與內部部署資料中心內的資源進行通訊**</br>
若要將叢集與內部部署資料中心（例如 {{site.data.keyword.icpfull_notm}}）相連接，可以設定 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)。藉由 {{site.data.keyword.Bluemix_notm}} Direct Link，您可以在遠端網路環境和 {{site.data.keyword.containerlong_notm}} 之間建立直接專用連線，而無需透過公用網際網路遞送。

**使用 strongSwan IPSec VPN 連線透過公用網路與內部部署資料中心內的資源進行通訊**
* 連接至公用和專用 VLAN 的工作者節點：直接在叢集裡設定 [strongSwan IPSec VPN 服務 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.strongswan.org/about.html)。在根據業界標準網際網路通訊協定安全 (IPSec) 通訊協定套組的網際網路上，strongsWan IPSec VPN 服務提供安全的端對端通訊通道。若要設定叢集與內部部署網路之間的安全連線，請直接在叢集的 Pod 中[配置及部署 strongSwan IPSec VPN 服務](/docs/containers?topic=containers-vpn#vpn-setup)。
* 僅連接至專用 VLAN 的工作者節點：在閘道裝置上設定 IPSec VPN 端點，例如虛擬路由器應用裝置 (Vyatta)。然後，在叢集裡[配置 strongSwan IPSec VPN 服務](/docs/containers?topic=containers-vpn#vpn-setup)，以在閘道上使用 VPN 端點。如果您不想要使用 strongSwan，則可以[使用 VRA 直接設定 VPN 連線功能](/docs/containers?topic=containers-vpn#vyatta)。


</br>

### 與在工作者節點上執行的應用程式的外部通訊
{: #external-workers}

容許從叢集外部向在工作者節點上執行的應用程式發出公用或專用資料流量要求。
{: shortdesc}

**流至叢集應用程式的專用資料流量**</br>
在叢集裡部署應用程式後，您可能希望使應用程式僅可供位於叢集所在專用網路上的使用者和服務存取。專用負載平衡適用於讓叢集外的要求使用您的應用程式，而不將應用程式公開給一般大眾使用。您也可以先使用專用負載平衡來測試應用程式的存取、要求遞送及其他配置，稍後再使用公用網路服務將應用程式公開給大眾使用。若要容許從叢集外部向應用程式發出專用資料流量要求，可以建立專用 Kubernetes 網路連線功能服務，例如專用 NodePort、NLB 和 Ingress ALB。然後可以使用 Calico DNAT 前原則來阻止流至專用網路連線功能服務的公用 NodePort 的資料流量。如需相關資訊，請參閱[規劃專用外部負載平衡](/docs/containers?topic=containers-cs_network_planning#private_access)。

**流至叢集應用程式的公用資料流量**</br>
若要使應用程式可供從公用網際網路進行外部存取，可以建立公用 NodePort、網路負載平衡器 (NLB) 和 Ingress 應用程式負載平衡器 (ALB)。公用網路服務使用公用 IP 位址及選用公用 URL 來提供應用程式，以連接至此公用網路介面。公開應用程式時，具有您為應用程式設定的公用服務 IP 位址或 URL 的任何人都可以將要求傳送至應用程式。然後，可以使用 Calico DNAT 前原則來控制流至公用網路連線功能服務的資料流量，例如僅將來自特定來源 IP 位址或 CIDR 的資料流量列入白名單，而阻塞其他所有資料流量。如需相關資訊，請參閱[規劃公用外部負載平衡](/docs/containers?topic=containers-cs_network_planning#private_access)。

要獲得額外的安全，請將網路連線功能工作負載隔離到邊緣工作者節點。邊緣工作者節點可以藉由容許可由外部存取連接至公用 VLAN 的較少工作者節點，以及隔離網路工作負載，來增進叢集的安全。[將工作者節點標註為邊緣節點](/docs/containers?topic=containers-edge#edge_nodes)時，NLB 和 ALB Pod 會僅部署到這些指定的工作者節點。若也要防止在邊緣節點上執行其他工作負載，您可以[為邊緣節點加上污點](/docs/containers?topic=containers-edge#edge_workloads)。
在 Kubernetes 1.14 版和更高版本中，可以將公用和專用 NLB 和 ALB 部署到邊緣節點。例如，如果工作者節點僅連接至專用 VLAN，但您需要允許對叢集裡的應用程式進行公用存取，則可以建立邊緣工作者節點儲存區，其中邊緣節點連接至公用和專用 VLAN。可以將公用 NLB 和 ALB 部署到這些邊緣節點，以確保只有這些工作者節點可處理公用連線。

如果工作者節點僅連接至專用 VLAN，並且您使用閘道裝置來提供工作者節點和叢集主節點之間的通訊，則還可以將該裝置配置為公用或專用防火牆。若要容許從叢集外部向應用程式發出公用或專用資料流量要求，可以建立公用或專用 NodePort、NLB 和 Ingress ALB。然後，必須在閘道裝置防火牆中[開啟必要的埠和 IP 位址](/docs/containers?topic=containers-firewall#firewall_inbound)，以允許透過公用或專用網路送至這些服務的入埠資料流量。
{: note}

<br />


## 情境：在叢集裡執行面對網際網路的應用程式工作負載
{: #internet-facing}

在此情境中，您希望在叢集裡執行的工作負載可供來自網際網路的要求存取，以便一般使用者可以存取應用程式。您希望使用的選項是在叢集裡隔離公用存取，並控制允許向叢集發出哪些公用要求。此外，工作者節點對要連接到叢集的任何 {{site.data.keyword.Bluemix_notm}} 服務都具有自動存取權。
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_internet.png" alt="執行網際網路適用的工作負載的叢集的架構圖"/>
 <figcaption>執行網際網路適用的工作負載的叢集的架構</figcaption>
</figure>
</p>

若要達成此設定，請將工作者節點連接至公用和專用 VLAN 來建立叢集。

如果您建立具有公用和專用 VLAN 的叢集，則您以後無法從該叢集移除所有公用 VLAN。從叢集移除所有公用 VLAN 會導致數個叢集元件停止運作。請改為建立僅連接至專用 VLAN 的新工作者節點儲存區。
{: note}

可以選擇容許透過公用和專用網路或僅透過公用網路進行工作者節點到主節點的通訊以及使用者到主節點的通訊。
* 公用和專用服務端點：帳戶必須已啟用了 VRF 並讓您能使用服務端點。工作者節點到主節點之間的通訊透過專用服務端點在專用網路上建立，以及透過為公用服務端點在公用網路上建立。授權叢集使用者可透過公用服務端點公開存取主節點。
* 公用服務端點：如果您不希望或無法為帳戶啟用 VRF，則工作者節點和授權叢集使用者可以透過公用服務端點在公用網路上自動連接至 Kubernetes 主節點。

您的工作者節點可以自動且安全地與其他 {{site.data.keyword.Bluemix_notm}} 服務通訊，而那些服務需要透過您的 IBM Cloud 基礎架構 (SoftLayer) 專用網路支援專用服務端點。如果 {{site.data.keyword.Bluemix_notm}} 服務不支援專用服務端點，則工作者節點可以透過公用網路安全地與服務進行通訊。您可以鎖定工作者節點的公用或專用介面，方法是使用 Calico 網路原則進行公用網路或專用網路隔離。您可能需要容許存取您要用在這些 Calico 隔離原則之服務的公用及專用 IP 位址。

若要將叢集裡的應用程式公開到網際網路，可以建立公用網路負載平衡器 (NLB) 或 Ingress 應用程式負載平衡器 (ALB) 服務。透過建立標註為邊緣節點的工作者節點的儲存區，可以提高叢集的安全。公用網路服務的 Pod 會部署到邊緣節點，以便將外部資料流量工作負載隔離到叢集裡的僅少數幾個工作者節點。您可以對於公開您應用程式的網路服務，進一步控制其公用資料流量，方法是建立 Calico DNAT 前的原則，例如白名單及黑名單原則。

如果工作者節點需要存取 {{site.data.keyword.Bluemix_notm}} 帳戶外部的專用網路中的服務，則可以在叢集裡配置並部署 strongSwan IPSec VPN 服務，或者利用 {{site.data.keyword.Bluemix_notm}} Direct Link 服務來連接至這些網路。

準備好要開始針對這個情境使用叢集了嗎？規劃[高可用性](/docs/containers?topic=containers-ha_clusters)及[工作者節點](/docs/containers?topic=containers-planning_worker_nodes)設定之後，請參閱[建立叢集](/docs/containers?topic=containers-clusters#cluster_prepare)。

<br />


## 情境：將內部部署資料中心延伸到專用網路上的叢集，並新增有限公用存取權
{: #limited-public}

在此情境中，您希望在叢集裡執行的工作負載可供內部部署資料中心內的服務、資料庫或其他資源存取。但是，您可能需要提供對叢集的有限公用存取權，並且希望確保在叢集裡控制和隔離任何公用存取。例如，您可能需要工作者節點存取不支援專用服務端點而必須透過公用網路進行存取的 {{site.data.keyword.Bluemix_notm}} 服務。或者，您可能需要提供對叢集裡執行的應用程式的有限公用存取權。
{: shortdesc}

若要實現此叢集設定，可以[使用邊緣節點和 Calico 網路原則](#calico-pc)或[使用閘道裝置](#vyatta-gateway)來建立防火牆。

### 使用邊緣節點和 Calico 網路原則
{: #calico-pc}

透過將邊緣節點用作公用閘道，並將 Calico 網路原則用作公用防火牆，容許與叢集建立有限公用連線功能。
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_calico.png" alt="使用邊緣節點和 Calico 網路原則進行安全公用存取的叢集的架構圖"/>
 <figcaption>使用邊緣節點和 Calico 網路原則進行安全公用存取的叢集的架構</figcaption>
</figure>
</p>

使用此設定時，可透過將工作者節點僅連接至專用 VLAN 來建立叢集。帳戶必須已啟用了 VRF 並讓您能使用專用服務端點。

如果授權叢集使用者位於 {{site.data.keyword.Bluemix_notm}} 專用網路，或透過 [VPN 連線](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking)或 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) 連接至專用網路，則可以透過專用服務端點私下存取 Kubernetes 主節點。然而，透過專用服務端點與 Kubernetes 主節點的通訊，必須經過 <code>166.X.X.X</code> IP 位址範圍，此範圍無法從 VPN 連線或透過 {{site.data.keyword.Bluemix_notm}} Direct Link 遞送。您可以為叢集使用者，使用專用網路負載平衡器 (NLV) 公開主節點的專用服務端點。專用 NLB 會將主節點的專用服務端點公開為內部 <code>10.X.X.X</code> IP 位址範圍，使用者可以用 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 連線來存取此範圍。如果您只啟用專用服務端點，可以使用 Kubernetes 儀表板或暫時啟用公用服務端點以建立專用 NLB。

接下來，可以建立連接至公用和專用 VLAN 並標註為邊緣節點的工作者節點的儲存區。邊緣節點透過僅容許外部存取少數幾個工作者節點，並將網路連線功能工作負載隔離到這些工作者節點，可以提高叢集的安全。

您的工作者節點可以自動且安全地與其他 {{site.data.keyword.Bluemix_notm}} 服務通訊，而那些服務需要透過您的 IBM Cloud 基礎架構 (SoftLayer) 專用網路支援專用服務端點。如果 {{site.data.keyword.Bluemix_notm}} 服務不支援專用服務端點，則連接至公用 VLAN 的邊緣節點可以透過公用網路安全地與服務進行通訊。您可以鎖定工作者節點的公用或專用介面，方法是使用 Calico 網路原則進行公用網路或專用網路隔離。您可能需要容許存取您要用在這些 Calico 隔離原則之服務的公用及專用 IP 位址。

若要提供對您叢集內應用程式的專用存取，您可以建立專用網路負載平衡器 (NLB) 或 Ingress 應用程式負載平衡器 (ALB)，將應用程式只公開給專用網路。透過建立 Calico DNAT 前原則（例如，用於阻止工作者節點上的公用 NodePort 的原則），可以阻止流至用於公開應用程式的這些網路服務的所有公用資料流量。如果您需要提供對您叢集內應用程式的有限公用存取，可以建立公用 NLB 或 ALB 來公開您的應用程式。然後，必須將應用程式部署到這些邊緣節點，以便 NLB 或 ALB 可以將公用資料流量定向到應用程式 Pod。您可以對於公開您應用程式的網路服務，進一步控制其公用資料流量，方法是建立 Calico DNAT 前的原則，例如白名單及黑名單原則。專用和公用網路服務的 Pod 會部署到邊緣節點，以便將外部資料流量工作負載限制為僅流至叢集裡的少數幾個工作者節點。  

若要安全地存取 {{site.data.keyword.Bluemix_notm}} 外部和其他內部部署網路上的服務，可以在叢集裡配置並部署 strongSwan IPSec VPN 服務。strongSwan 負載平衡器 Pod 會部署到邊緣儲存區中的一個工作者節點，其中 Pod 會透過加密 VPN 通道在公用網路上建立與內部部署網路的安全連線。或者，可以使用 {{site.data.keyword.Bluemix_notm}} Direct Link 服務僅透過專用網路將叢集連接至內部部署資料中心。

準備好要開始針對這個情境使用叢集了嗎？規劃[高可用性](/docs/containers?topic=containers-ha_clusters)及[工作者節點](/docs/containers?topic=containers-planning_worker_nodes)設定之後，請參閱[建立叢集](/docs/containers?topic=containers-clusters#cluster_prepare)。

</br>

### 使用閘道裝置
{: #vyatta-gateway}

透過將閘道裝置（例如，虛擬路由器應用裝置 (Vyatta)）配置為公用閘道和防火牆，容許與叢集建立有限公用連線功能。
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_gateway.png" alt="使用閘道裝置進行安全公用存取的叢集的架構圖"/>
 <figcaption>使用閘道裝置進行安全公用存取的叢集的架構</figcaption>
</figure>
</p>

如果僅在專用 VLAN 上設定了工作者節點，並且您不希望或無法為帳戶啟用 VRF，則必須配置閘道裝置，以透過公用網路在工作者節點與主節點之間提供網路連線功能。例如，可以選擇設定[虛擬路由器應用裝置](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)或 [Fortigate Security Appliance](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)。

可以使用自訂網路原則來設定閘道裝置，以便為叢集提供專用網路安全，偵測網路侵入並進行補救。在公用網路上設定防火牆時，必須為每個地區開啟必要的埠和專用 IP 位址，以便主節點和工作者節點可以通訊。如果還為專用網路配置了此防火牆，您還必須開啟必要的埠和專用 IP 位址，以容許工作者節點之間的通訊，並且使叢集透過專用網路存取基礎架構資源。此外，還必須為帳戶啟用 VLAN Spanning，以便子網路可以在相同 VLAN 上以及不同 VLAN 之間進行路由。

若要將工作者節點和應用程式安全地連接至內部部署網路或 {{site.data.keyword.Bluemix_notm}} 外部的服務，請在閘道裝置上設定 IPSec VPN 端點，並在叢集裡設定 strongSwan IPSec VPN 服務以使用閘道 VPN 端點。如果不想使用 strongSwan，則可以使用 VRA 直接設定 VPN 連線功能。

工作者節點可以透過閘道裝置與其他 {{site.data.keyword.Bluemix_notm}} 服務和 {{site.data.keyword.Bluemix_notm}} 外部的公用服務安全地進行通訊。可以將防火牆配置為僅容許存取要使用的服務的公用和專用 IP 位址。

若要提供對您叢集內應用程式的專用存取，您可以建立專用網路負載平衡器 (NLB) 或 Ingress 應用程式負載平衡器 (ALB)，將應用程式只公開給專用網路。如果您需要提供對您叢集內應用程式的有限公用存取，可以建立公用 NLB 或 ALB 來公開您的應用程式。由於所有資料流量都流經閘道裝置防火牆，因此可以透過在防火牆中開啟服務的埠和 IP 位址來允許流至用於公開應用程式的網路服務的入埠資料流量，以控制流至這些服務的公用和專用資料流量。

準備好要開始針對這個情境使用叢集了嗎？規劃[高可用性](/docs/containers?topic=containers-ha_clusters)及[工作者節點](/docs/containers?topic=containers-planning_worker_nodes)設定之後，請參閱[建立叢集](/docs/containers?topic=containers-clusters#cluster_prepare)。

<br />


## 情境：將內部部署資料中心延伸到專用網路上的叢集
{: #private_clusters}

在此情境中，您希望在 {{site.data.keyword.containerlong_notm}} 叢集裡執行工作負載。但是，您希望這些工作負載僅可供內部部署資料中心內的服務、資料庫或其他資源（例如，{{site.data.keyword.icpfull_notm}}）存取。叢集工作負載可能需要存取支援透過專用網路進行通訊的其他若干 {{site.data.keyword.Bluemix_notm}} 服務，例如 {{site.data.keyword.cos_full_notm}}。
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_extend.png" alt="在專用網路上連接至內部部署資料中心的叢集的架構圖"/>
 <figcaption>在專用網路上連接至內部部署資料中心的叢集的架構</figcaption>
</figure>
</p>

若要實現此設定，可透過將工作者節點僅連接至專用 VLAN 來建立叢集。若要僅透過專用服務端點在專用網路上提供叢集主節點和工作者節點之間的連線功能，帳戶必須已啟用 VRF 並讓您能使用服務端點。由於已啟用了 VRF 時專用網路上的任何資源都可看到叢集，因此可以透過套用 Calico 專用網路原則，將叢集與專用網路上的其他系統相隔離。

如果授權叢集使用者位於 {{site.data.keyword.Bluemix_notm}} 專用網路，或透過 [VPN 連線](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking)或 [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) 連接至專用網路，則可以透過專用服務端點私下存取 Kubernetes 主節點。然而，透過專用服務端點與 Kubernetes 主節點的通訊，必須經過 <code>166.X.X.X</code> IP 位址範圍，此範圍無法從 VPN 連線或透過 {{site.data.keyword.Bluemix_notm}} Direct Link 遞送。您可以為叢集使用者，使用專用網路負載平衡器 (NLV) 公開主節點的專用服務端點。專用 NLB 會將主節點的專用服務端點公開為內部 <code>10.X.X.X</code> IP 位址範圍，使用者可以用 VPN 或 {{site.data.keyword.Bluemix_notm}} Direct Link 連線來存取此範圍。如果您只啟用專用服務端點，可以使用 Kubernetes 儀表板或暫時啟用公用服務端點以建立專用 NLB。

工作者節點可以自動、安全地與支援專用服務端點的其他 {{site.data.keyword.Bluemix_notm}} 服務（例如，{{site.data.keyword.registrylong}}）透過 IBM Cloud 基礎架構 (SoftLayer) 專用網路進行通訊。例如，{{site.data.keyword.cloudant_short_notm}} 的所有標準方案實例的專用硬體環境都支援專用服務端點。如果 {{site.data.keyword.Bluemix_notm}} 服務不支援專用服務端點，則叢集無法存取該服務。

若要提供對叢集裡應用程式的專用存取權，可以建立專用網路負載平衡器 (NLB) 或 Ingress 應用程式負載平衡器 (ALB)。這些 Kubernetes 網路服務僅將應用程式公開到專用網路，以便連線到該 NLB IP 所在子網路的任何內部部署系統都可以存取應用程式。

準備好要開始針對這個情境使用叢集了嗎？規劃[高可用性](/docs/containers?topic=containers-ha_clusters)及[工作者節點](/docs/containers?topic=containers-planning_worker_nodes)設定之後，請參閱[建立叢集](/docs/containers?topic=containers-clusters#cluster_prepare)。
