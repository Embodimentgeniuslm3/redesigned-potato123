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



# 配置叢集的子網路
{: #subnets}

將子網路新增至 Kubernetes 叢集，為網路負載平衡器 (NLB) 服務變更可用的可攜式公用或專用 IP 位址的儲存區。
{:shortdesc}



## {{site.data.keyword.containerlong_notm}} 中的網路連線功能概觀
{: #basics}

瞭解 {{site.data.keyword.containerlong_notm}} 叢集裡網路連線功能的基本概念。{{site.data.keyword.containerlong_notm}} 使用 VLAN、子網路及 IP 位址來提供叢集元件網路連線功能。
{: shortdesc}

### VLAN
{: #basics_vlans}

當您建立叢集時，叢集的工作者節點會自動連接至 VLAN。VLAN 會配置一組工作者節點及 Pod，彷彿它們已連接至相同的實體佈線，並提供一個通道，在工作者與 Pod 之間進行連線。
{: shortdesc}

<dl>
<dt>免費叢集的 VLAN</dt>
<dd>在免費叢集裡，叢集的工作者節點依預設會連接至 IBM 擁有的公用 VLAN 及專用 VLAN。由於 IBM 控制了 VLAN、子網路及 IP 位址，所以您無法建立多區域叢集，或將子網路新增至您的叢集，只能使用 NodePort 服務來公開您的應用程式。</dd>
<dt>標準叢集的 VLAN</dt>
<dd>在標準叢集裡，第一次在區域中建立叢集時，會在您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶中，自動為您在該區域中佈建公用 VLAN 及專用 VLAN。針對您在該區域建立的每個後續叢集，您必須指定要在該區域中使用的 VLAN 配對。您可以重複使用為您所建立的相同公用和專用 VLAN，因為多個叢集可以共用 VLAN。</br></br>您可以將工作者節點同時連接至公用 VLAN 和專用 VLAN，或僅連接至專用 VLAN。如果您只想要將工作者節點連接至專用 VLAN，則可以使用現有專用 VLAN 的 ID，或[建立專用 VLAN](/docs/cli/reference/ibmcloud?topic=cloud-cli-manage-classic-vlans#sl_vlan_create)，並在建立叢集期間使用該 ID。</dd></dl>

若要查看每個區域中針對您帳戶所佈建的 VLAN，請執行 `ibmcloud ks vlans --zone <zone>`。若要查看某個叢集佈建所在的 VLAN，請執行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> --showResources`，並尋找 **Subnet VLANs** 區段。

IBM Cloud 基礎架構 (SoftLayer) 管理在您於區域中建立第一個叢集時所自動供應的 VLAN。如果您讓 VLAN 變成未使用（例如移除 VLAN 中的所有工作者節點），IBM Cloud 基礎架構 (SoftLayer) 會收回此 VLAN。之後，如果您需要新建的 VLAN，請[與 {{site.data.keyword.Bluemix_notm}} 支援中心聯絡](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans)。

**我日後可以變更 VLAN 決策嗎？**</br>
您可以修改叢集裡的工作者節點儲存區，來變更 VLAN 設定。如需相關資訊，請參閱[變更工作者節點 VLAN 連線](/docs/containers?topic=containers-cs_network_cluster#change-vlans)。


### 子網路及 IP 位址
{: #basics_subnets}

除了工作者節點及 Pod 外，子網路也會自動佈建在 VLAN 上。子網路可藉由將 IP 位址指派給您的叢集元件，為它們提供網路連線功能。
{: shortdesc}

下列子網路會自動佈建在預設公用及專用 VLAN 上：

**公用 VLAN 子網路**
* 主要公用子網路會決定在建立叢集期間指派給工作者節點的公用 IP 位址。相同 VLAN 上的多個叢集可以共用一個主要公用子網路。
* 可攜式公用子網路只會連結至一個叢集，並提供該叢集 8 個公用 IP 位址。有 3 個 IP 保留給 IBM Cloud 基礎架構 (SoftLayer) 功能。1 個 IP 由預設公用 Ingress ALB 使用，而 4 個 IP 可以用來建立公用網路負載平衡器 (NLB) 服務或更多公用 ALB。可攜式公用 IP 是永久的固定 IP 位址，可用於透過網際網路存取 NLB 或 ALB。如果需要 4 個以上的 IP 用於 NLB 或 ALB，請參閱[新增可攜式 IP 位址](/docs/containers?topic=containers-subnets#adding_ips)。

**專用 VLAN 子網路**
* 主要專用子網路會決定在建立叢集期間指派給工作者節點的專用 IP 位址。相同 VLAN 上的多個叢集可以共用一個主要專用子網路。
* 可攜式專用子網路只會連結至一個叢集，並提供該叢集 8 個專用 IP 位址。有 3 個 IP 保留給 IBM Cloud 基礎架構 (SoftLayer) 功能。1 個 IP 由預設專用 Ingress ALB 使用，而 4 個 IP 可以用來建立專用網路負載平衡器 (NLB) 服務或更多專用 ALB。可攜式專用 IP 是永久的固定 IP 位址，可用於透過專用網路存取 NLB 或 ALB。如果需要 4 個以上的 IP 用於專用 NLB 或 ALB，請參閱[新增可攜式 IP 位址](/docs/containers?topic=containers-subnets#adding_ips)。

若要查看您帳戶中佈建的所有子網路，請執行 `ibmcloud ks subnets`。若要查看連結至某個叢集的可攜式公用及可攜式專用子網路，您可以執行 `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> --showResources`，並尋找 **Subnet VLANs** 區段。

在 {{site.data.keyword.containerlong_notm}} 中，VLAN 的限制為 40 個子網路。如果您達到此限制，首先請查看您是否可以[重複使用 VLAN 中的子網路來建立新的叢集](/docs/containers?topic=containers-subnets#subnets_custom)。如果您需要新的 VLAN，請[與 {{site.data.keyword.Bluemix_notm}} 支援中心聯絡](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans)，進行訂購。然後，[建立叢集](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)，而叢集使用這個新的 VLAN。
{: note}

**工作者節點的 IP 位址會變更嗎？**</br>
您的工作者節點獲指派叢集所使用之公用或專用 VLAN 上的 IP 位址。佈建工作者節點之後，IP 位址不會變更。例如，工作者節點 IP 位址會跨 `reload`、`reboot` 及 `update` 作業持續保存。此外，工作者節點的專用 IP 位址會用於大多數 `kubectl` 指令中的工作者節點身分。如果您變更工作者節點儲存區所使用的 VLAN，則該儲存區中佈建的新工作者節點會使用新的 VLAN 作為其 IP 位址。現有工作者節點 IP 位址不會變更，但您可以選擇移除使用舊 VLAN 的工作者節點。

### 網路分段
{: #basics_segmentation}

網路區隔說明將網路分成多個子網路的方式。在某個子網路中執行的應用程式無法查看或存取另一個子網路中的應用程式。如需網路分段選項及其與 VLAN 的關係的相關資訊，請參閱[此叢集安全主題](/docs/containers?topic=containers-security#network_segmentation)。
{: shortdesc}

不過，在數種狀況下，必須允許叢集裡的元件跨多個專用 VLAN 進行通訊。例如，如果您要建立多區域叢集、某個叢集有多個 VLAN，或在相同的 VLAN 上有多個子網路，則相同 VLAN 的不同子網路上的工作者節點或是不同 VLAN 上的工作者節點無法自動彼此通訊。您必須針對 IBM Cloud 基礎架構 (SoftLayer) 帳戶啟用「虛擬路由器功能 (VRF)」或 VLAN Spanning。

**何謂「虛擬路由器功能 (VRF)」和 VLAN Spanning？**</br>

<dl>
<dt>[虛擬路由器功能 (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)</dt>
<dd>VRF 可讓基礎架構帳戶中的所有 VLAN 和子網路彼此通訊。此外，還需要有 VRF，才能容許工作者節點與主節點透過專用服務端點進行通訊。若要啟用 VRF，[請與 IBM Cloud 基礎架構 (SoftLayer) 客戶代表聯絡](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion)。請注意，VRF 會刪除帳戶的 VLAN Spanning 選項，因為除非您配置閘道裝置來管理資料流量，否則所有 VLAN 都可以通訊。</dd>
<dt>[VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)</dt>
<dd>如果您無法或不想要啟用 VRF，請啟用 VLAN Spanning。若要執行此動作，您需要**網路 > 管理網路 VLAN Spanning** [基礎架構許可權](/docs/containers?topic=containers-users#infra_access)，或者您可以要求帳戶擁有者啟用它。若要確認是否已啟用 VLAN Spanning，請使用 `ibmcloud ks vlan-spanning-get --region <region>` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。
      請注意，如果您選擇啟用 VLAN Spanning 而非 VRF，則無法啟用專用服務端點。</dd>
</dl>

**VRF 或 VLAN Spanning 對網路分段的影響為何？**</br>

啟用 VRF 或 VLAN Spanning 時，任何已連接至相同 {{site.data.keyword.Bluemix_notm}} 帳戶中任何專用 VLAN 的系統都可以與工作者節點通訊。您可以套用 [Calico 專用網路原則](/docs/containers?topic=containers-network_policies#isolate_workers)，將叢集與專用網路上的其他系統隔離。{{site.data.keyword.containerlong_notm}} 也與所有 [IBM Cloud 基礎架構 (SoftLayer) 防火牆供應項目 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud-computing/bluemix/network-security) 相容。您可以使用自訂網路原則來設定 [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) 這類防火牆，以提供標準叢集的專用網路安全，以及偵測及重新修補網路侵入。

<br />



## 使用現有子網路建立叢集
{: #subnets_custom}

建立標準叢集時，會自動為您建立子網路。不過，您可以不要使用自動佈建的子網路，而使用 IBM Cloud 基礎架構 (SoftLayer) 帳戶中的現有可攜式子網路，或重複使用已刪除叢集裡的子網路。
{:shortdesc}

使用此選項，可在移除及建立叢集時保留穩定的靜態 IP 位址，或訂購更大的 IP 位址區塊。如果您要改用自己的內部部署網路子網路來取得叢集網路負載平衡器 (NLB) 服務的其他可攜式專用 IP 位址，請參閱[新增使用者管理的子網路至專用 VLAN 以新增可攜式專用 IP](#subnet_user_managed)。

可攜式公用 IP 位址是按月計費。如果您在佈建叢集之後移除可攜式公用 IP 位址，則仍須支付一個月的費用，即使您只是短時間使用也是一樣。
{: note}

開始之前：
- [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- 若要重複使用來自您不再需要之叢集的子網路，請刪除不需要的叢集。請立即建立新的叢集，因為如果您未重複使用子網路，這些子網路會在 24 小時內遭到刪除。

   ```
   ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
   ```
   {: pre}

</br>若要在 IBM Cloud 基礎架構 (SoftLayer) 組合中使用現有子網路，請執行下列動作：

1. 取得要使用之子網路的 ID，以及子網路所在 VLAN 的 ID。

    ```
    ibmcloud ks subnets
    ```
    {: pre}

    在此輸出範例中，子網路 ID 是 `1602829`，而 VLAN ID 是 `2234945`：
    ```
    Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public
    ```
    {: screen}

2. 使用所識別的 VLAN ID [在 CLI 中建立叢集](/docs/containers?topic=containers-clusters#clusters_cli_steps)。包含 `--no-subnet` 旗標，以防止自動建立新的可攜式公用 IP 子網路及新的可攜式專用 IP 子網路。

    ```
    ibmcloud ks cluster-create --zone dal10 --machine-type b3c.4x16 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name my_cluster
    ```
    {: pre}
    對於 `--zone` 旗標，如果您不記得 VLAN 所在的區域，可以執行 `ibmcloud ks vlans --zone <zone>` 來檢查 VLAN 是否位於特定區域中。
    {: tip}

3.  驗證已建立叢集。訂購工作者節點機器，並在您的帳戶中設定及佈建叢集，最多可能需要 15 分鐘。

    ```
    ibmcloud ks clusters
    ```
    {: pre}

    完全佈建叢集後，**狀態**會變更為 `deployed`。

    ```
    Name         ID                                   State      Created          Workers    Zone      Version     Resource Group Name
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3          dal10     1.13.6      Default
    ```
    {: screen}

4.  檢查工作者節點的狀態。

    ```
        ibmcloud ks workers --cluster <cluster_name_or_ID>
        ```
    {: pre}

    在繼續進行下一個步驟之前，工作者節點必須已備妥。**狀況**會變更為 `normal`，而**狀態**是 `Ready`。

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Zone     Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx  free           normal     Ready    dal10      1.13.6
    ```
    {: screen}

5.  指定子網路 ID，以將子網路新增至叢集。當您讓子網路可供叢集使用時，會為您建立 Kubernetes ConfigMap，其中包含您可以使用的所有可用可攜式公用 IP 位址。如果子網路 VLAN 所在的區域裡沒有 Ingress ALB 存在，則會自動使用一個可攜式公用 IP 位址及一個可攜式專用 IP 位址，來建立該區域的公用及專用 ALB。您可以使用來自該子網路的所有其他可攜式公用和專用 IP 位址，為您的應用程式建立 NLB 服務。

  ```
  ibmcloud ks cluster-subnet-add --cluster <cluster_name_or_id> --subnet-id <subnet_ID>
  ```
  {: pre}

  指令範例：
  ```
  ibmcloud ks cluster-subnet-add --cluster mycluster --subnet-id 807861
  ```
  {: screen}

6. **重若要事項**：若要為位於相同 VLAN 之不同子網路上的工作者節點，啟用它們之間的通訊，您必須[在相同 VLAN 上的子網路之間啟用遞送](#subnet-routing)。

<br />


## 管理現有的可攜式 IP 位址
{: #managing_ips}

依預設，4 個可攜式公用 IP 位址和 4 個可攜式專用 IP 位址可用於透過[建立網路負載平衡器 (NLB) 服務](/docs/containers?topic=containers-loadbalancer)或[建立其他 Ingress 應用程式負載平衡器 (ALB)](/docs/containers?topic=containers-ingress#scale_albs)，將單一應用程式公開至公用或專用網路。若要建立 NLB 或 ALB 服務，您必須至少有 1 個正確類型的可攜式 IP 位址可用。您可以檢視可用的可攜式 IP 位址，或釋放已使用的可攜式 IP 位址。
{: shortdesc}

### 檢視可用的可攜式公用 IP 位址
{: #review_ip}

若要列出叢集裡的所有可攜式 IP 位址（已使用及可用），您可以執行下列指令。
{: shortdesc}

```
kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
```
{: pre}

若要僅列出可用於建立公用 NLB 或更多公用 ALB 的可攜式公用 IP 位址，可以使用下列步驟：

開始之前：
-  確定您具有 `default` 名稱空間的[**撰寫者**或**管理員** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)。
- [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

若要列出可用的可攜式公用 IP 位址，請執行下列動作：

1.  建立名為 `myservice.yaml` 的 Kubernetes 服務配置檔，並且定義類型為 `LoadBalancer` 且具有虛擬 NLB IP 位址的服務。下列範例使用 IP 位址 1.1.1.1 作為 NLB IP 位址。將 `<zone>` 取代為您要在其中檢查可用 IP 的區域。

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
      annotations:
        service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "<zone>"
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  在叢集裡建立服務。

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  檢查服務。

    ```
    kubectl describe service myservice
    ```
    {: pre}

    建立此服務失敗，因為 Kubernetes 主節點在 Kubernetes ConfigMap 中找不到指定的 NLB IP 位址。執行此指令時，可能會看到錯誤訊息以及該叢集的可用公用 IP 位址清單。

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IP addresses are available: <list_of_IP_addresses>
    ```
    {: screen}

<br />


### 釋放已使用的 IP 位址
{: #free}

可以透過刪除正在使用可攜式 IP 位址的網路負載平衡器 (NLB) 服務或停用正在使用可攜式 IP 位址的 Ingress 應用程式負載平衡器 (ALB)，釋放已使用可攜式 IP 位址。
{:shortdesc}

開始之前：
-  確定您具有 `default` 名稱空間的[**撰寫者**或**管理員** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)。
- [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

若要刪除 NLB 或停用 ALB，請執行下列動作：

1. 列出叢集裡可用的服務。
    ```
    kubectl get services | grep LoadBalancer
    ```
    {: pre}

2. 移除使用公用或專用 IP 位址的 LoadBalancer 服務或停用使用該 IP 位址的 ALB。
  * 刪除 NLB：
    ```
    kubectl delete service <service_name>
    ```
    {: pre}
  * 停用 ALB：
    ```
    ibmcloud ks alb-configure --albID <ALB_ID> --disable
    ```
    {: pre}

<br />


## 新增可攜式 IP 位址
{: #adding_ips}

依預設，藉由[建立網路負載平衡器 (NLB) 服務](/docs/containers?topic=containers-loadbalancer)，即可使用 4 個可攜式公用 IP 位址和 4 個可攜式專用 IP 位址，將單一應用程式公開至公用或專用網路。若要建立超過 4 個公用或 4 個專用 NLB，您可以將網路子網路新增至叢集來取得其他可攜式 IP 位址。
{: shortdesc}

當您讓子網路可供叢集使用時，會使用這個子網路的 IP 位址來進行叢集網路連線。若要避免 IP 位址衝突，請確定一個子網路只搭配使用一個叢集。請不要同時將一個子網路用於多個叢集或 {{site.data.keyword.containerlong_notm}} 以外的其他用途。
{: important}

### 透過訂購更多子網路來新增可攜式 IP
{: #request}

您可以在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中建立新的子網路，並讓它可供您指定的叢集使用，而為 NLB 服務取得其他可攜式 IP。
{:shortdesc}

可攜式公用 IP 位址是按月計費。如果您在佈建子網路之後移除可攜式公用 IP 位址，則仍須支付一個月的費用，即使您只是短時間使用。
{: note}

開始之前：
-  確定您具有叢集的[**操作員**或**管理者** {{site.data.keyword.Bluemix_notm}} IAM 平台角色](/docs/containers?topic=containers-users#platform)。
- [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

若要訂購子網路，請執行下列動作：

1. 佈建新的子網路。

    ```
    ibmcloud ks cluster-subnet-create --cluster <cluster_name_or_id> --size <subnet_size> --vlan <VLAN_ID>
    ```
    {: pre}

    <table>
    <caption>瞭解這個指令的元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解這個指令的元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td>將 <code>&lt;cluster_name_or_id&gt;</code> 取代為叢集的名稱或 ID。</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td>將 <code>&lt;subnet_size&gt;</code> 取代為要在可攜式子網路中建立的 IP 位址數。接受值為 8、16、32 或 64。<p class="note"> 當您新增子網路的可攜式 IP 位址時，會使用三個 IP 位址來建立叢集內部網路。您無法將這三個 IP 位址用於 Ingress 應用程式負載平衡器 (ALB)，或使用它們來建立網路負載平衡器 (NLB) 服務。例如，如果您要求八個可攜式公用 IP 位址，則可以使用其中的五個將您的應用程式公開給大眾使用。</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td>將 <code>&lt;VLAN_ID&gt;</code> 取代為您要在其上配置可攜式公用或專用 IP 位址之公用或專用 VLAN 的 ID。您必須選取現有工作者節點所連接的公用或專用 VLAN。若要檢閱工作者節點連接mgln hapi 公用或專用 VLAN，請執行 <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code>，並在輸出中尋找 <strong>Subnet VLANs</strong> 區段。子網路佈建於 VLAN 所在的同一個區域中。</td>
    </tr>
    </tbody></table>

2. 驗證已順利建立子網路並將其新增至叢集。子網路 CIDR 列出在 **Subnet VLANs** 區段中。

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

3. **重要事項**：若要為位於相同 VLAN 之不同子網路上的工作者節點，啟用它們之間的通訊，您必須[在相同 VLAN 上的子網路之間啟用遞送](#subnet-routing)。

<br />


### 新增使用者管理的子網路至專用 VLAN 以新增可攜式專用 IP
{: #subnet_user_managed}

您可以讓來自內部部署網路的子網路可供您的叢集使用，為網路負載平衡器 (NLB) 服務取得其他可攜式專用 IP。
{:shortdesc}

想要改為在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中重複使用現有的可攜式子網路嗎？請參閱[使用自訂或現有 IBM Cloud 基礎架構 (SoftLayer) 子網路來建立叢集](#subnets_custom)。
{: tip}

需求：
- 使用者管理的子網路只能新增至專用 VLAN。
- 子網路字首長度限制為 /24 到 /30。例如，`169.xx.xxx.xxx/24` 指定 253 個可用的專用 IP 位址，而 `169.xx.xxx.xxx/30` 指定 1 個可用的專用 IP 位址。
- 子網路中的第一個 IP 位址必須用來作為子網路的閘道。

開始之前：
- 配置進出外部子網路的網路資料流量遞送。
- 確認您在內部部署資料中心網路閘道與專用網路 Virtual Router Appliance 或您叢集裡執行的 strongSwan VPN 服務之間，具有 VPN 連線功能。如需相關資訊，請參閱[設定 VPN 連線功能](/docs/containers?topic=containers-vpn)。
-  確定您具有叢集的[**操作員**或**管理者** {{site.data.keyword.Bluemix_notm}} IAM 平台角色](/docs/containers?topic=containers-users#platform)。
- [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)


若要從內部部署網路新增子網路，請執行下列動作：

1. 檢視叢集專用 VLAN 的 ID。找出 **Subnet VLANs** 區段。在 **User-managed** 欄位中，識別具有 _false_ 的 VLAN ID。

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    ```
    {: screen}

2. 將外部子網路新增至您的專用 VLAN。可攜式專用 IP 位址會新增至叢集的 ConfigMap。

    ```
    ibmcloud ks cluster-user-subnet-add --cluster <cluster_name> --subnet-cidr <subnet_CIDR> --private-vlan <private_VLAN>
    ```
    {: pre}

    範例：

    ```
    ibmcloud ks cluster-user-subnet-add --cluster mycluster --subnet-cidr 10.xxx.xx.xxx/24 --private-vlan 2234947
    ```
    {: pre}

3. 驗證已新增使用者提供的子網路。**User-managed** 欄位為 _true_。

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    在此輸出範例中，第二個子網路已新增至 `2234947` 專用 VLAN：
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    2234947   10.xxx.xx.xxx/24  false    true
    ```
    {: screen}

4. [在相同 VLAN 上的子網路之間啟用遞送](#subnet-routing)。

5. 新增[專用網路負載平衡器 (NLB) 服務](/docs/containers?topic=containers-loadbalancer)或啟用[專用 Ingress ALB](/docs/containers?topic=containers-ingress#private_ingress)，以透過專用網路存取應用程式。若要使用來自您新增之子網路中的專用 IP 位址，您必須指定來自子網路 CIDR 中的 IP 位址。否則，會從 IBM Cloud 基礎架構 (SoftLayer) 子網路或專用 VLAN 上使用者提供的子網路中，隨機選擇一個 IP 位址。

<br />


## 管理子網路遞送
{: #subnet-routing}

如果您的叢集具有多個 VLAN、同一個 VLAN 上有多個子網路，或者是您具有多區域叢集，則必須為您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶啟用[虛擬路由器功能 (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)，讓工作者節點可以在專用網路上彼此通訊。若要啟用 VRF，[請與 IBM Cloud 基礎架構 (SoftLayer) 客戶代表聯絡](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion)。如果您無法或不想要啟用 VRF，請啟用 [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)。若要執行此動作，您需要**網路 > 管理網路 VLAN Spanning** [基礎架構許可權](/docs/containers?topic=containers-users#infra_access)，或者您可以要求帳戶擁有者啟用它。若要確認是否已啟用 VLAN Spanning，請使用 `ibmcloud ks vlan-spanning-get --region <region>` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。

檢閱下列也需要 VLAN Spanning 的情境。

### 在相同 VLAN 上的主要子網路之間啟用遞送
{: #vlan-spanning}

當您建立叢集時，會在公用及專用 VLAN 上佈建主要公用及專用子網路。主要的公用子網路會以 `/28` 為結尾，並為工作者節點提供 14 個公用 IP。主要的專用子網路會以 `/26` 為結尾，並為最多 62 個工作者節點提供專用 IP。
{:shortdesc}

如果您在相同 VLAN 的相同位置中擁有大型叢集或數個較小的叢集，便可能會超出工作者節點的起始 14 個公用及 62 個專用 IP。當公用或專用子網路達到工作者節點的限制時，會訂購同一個 VLAN 中的另一個主要子網路。

若要確保相同 VLAN 上這些主要子網路中的工作者節點可以通訊，您必須開啟 VLAN Spanning。如需指示，請參閱[啟用或停用 VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)。

若要確認是否已啟用 VLAN Spanning，請使用 `ibmcloud ks vlan-spanning-get --region <region>` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。
{: tip}

### 管理閘道裝置的子網路遞送
{: #vra-routing}

建立叢集時，會在連接叢集的 VLAN 上訂購可攜式公用及可攜式專用子網路。這些子網路會提供 Ingress 應用程式負載平衡器 (ALB) 及網路負載平衡器 (NLB) 服務的 IP 位址。
{: shortdesc}

不過，如果您具有現有的路由器應用裝置，例如 [Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra#about-the-vra)，則不會在路由器上配置剛新增的可攜式子網路，這些子網路來自叢集連接到的那些 VLAN。若要使用 NLB 或 Ingress ALB，您必須藉由[啟用 VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)，來確保網路裝置可以在相同 VLAN 上的不同子網路之間遞送。

若要確認是否已啟用 VLAN Spanning，請使用 `ibmcloud ks vlan-spanning-get --region <region>` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)。
{: tip}
