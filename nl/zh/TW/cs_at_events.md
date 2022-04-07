---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, audit

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



# {{site.data.keyword.cloudaccesstrailshort}} 事件
{: #at_events}

您可以使用 {{site.data.keyword.cloudaccesstrailshort}} 服務，在 {{site.data.keyword.containerlong_notm}} 叢集裡檢視、管理及審核使用者起始的活動。
{: shortdesc}

{{site.data.keyword.containershort_notm}} 會產生兩種類型的 {{site.data.keyword.cloudaccesstrailshort}} 事件：

* **叢集管理事件**：
    * 這些事件會自動產生並轉遞至 {{site.data.keyword.cloudaccesstrailshort}}。
    * 您可以透過 {{site.data.keyword.cloudaccesstrailshort}} **帳戶網域**來檢視這些事件。

* **Kubernetes API 伺服器審核事件**：
    * 這些事件會自動產生，但您必須配置叢集，以將這些事件轉遞至 {{site.data.keyword.cloudaccesstrailshort}} 服務。
    * 您可以配置叢集，以將事件傳送至 {{site.data.keyword.cloudaccesstrailshort}} **帳戶網域**或傳送至**空間網域**。如需相關資訊，請參閱[傳送審核日誌](/docs/containers?topic=containers-health#api_forward)。

如需服務運作方式的相關資訊，請參閱 [{{site.data.keyword.cloudaccesstrailshort}} 文件](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started)。如需所追蹤 Kubernetes 動作的相關資訊，請檢閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/home/)。

{{site.data.keyword.containerlong_notm}} 目前未配置為使用 {{site.data.keyword.at_full}}。若要管理叢集管理事件及 Kubernetes API 審核日誌，請繼續使用 {{site.data.keyword.cloudaccesstrailfull_notm}} with LogAnalysis。
{: note}

## 尋找事件的資訊
{: #kube-find}

您可以在 Kibana 儀表板中查看日誌，以監視叢集裡的活動。
{: shortdesc}

若要監視管理活動，請執行下列動作：

1. 登入 {{site.data.keyword.Bluemix_notm}} 帳戶。
2. 從型錄中，在與 {{site.data.keyword.containerlong_notm}} 實例相同的帳戶中佈建 {{site.data.keyword.cloudaccesstrailshort}} 服務實例。
3. 在 {{site.data.keyword.cloudaccesstrailshort}} 儀表板的**管理**標籤上，選取帳戶或空間網域。
  * **帳戶日誌**：叢集管理事件及 Kubernetes API 伺服器審核事件位於產生事件之 {{site.data.keyword.Bluemix_notm}} 地區的**帳戶網域**中。
  * **空間日誌**：當您將叢集配置為轉遞 Kubernetes API 伺服器審核事件時，如果指定了空間，則這些事件位於**空間網域**中，而此空間網域與佈建 {{site.data.keyword.cloudaccesstrailshort}} 服務的 Cloud Foundry 空間相關聯。
4. 按一下**在 Kibana 中檢視**。
5. 設定您要檢視其日誌的時間範圍。預設值為 24 小時。
6. 若要縮小搜尋範圍，您可以按一下 `ActivityTracker_Account_Search_in_24h` 的編輯圖示，並在**可用欄位**直欄中新增欄位。

若要讓其他使用者檢視帳戶及空間事件，請參閱[授與許可權來查看帳戶事件](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions)。
{: tip}

## 追蹤叢集管理事件
{: #cluster-events}

請查看下列清單，其中列出傳送至 {{site.data.keyword.cloudaccesstrailshort}} 的叢集管理事件。
{: shortdesc}

<table>
<tr>
<th>動作</th>
<th>說明</th></tr><tr>
<td><code>containers-kubernetes.account-credentials.set</code></td>
<td>已設定資源群組之地區中的基礎架構認證。</td></tr><tr>
<td><code>containers-kubernetes.account-credentials.unset</code></td>
<td>已取消設定資源群組之地區中的基礎架構認證。</td></tr><tr>
<td><code>containers-kubernetes.alb.create</code></td>
<td>已建立 Ingress ALB。</td></tr><tr>
<td><code>containers-kubernetes.alb.delete</code></td>
<td>已刪除 Ingress ALB。</td></tr><tr>
<td><code>containers-kubernetes.alb.get</code></td>
<td>已檢視 Ingress ALB 資訊。</td></tr><tr>
<td><code>containers-kubernetes.apikey.reset</code></td>
<td>已重設地區及資源群組的 API 金鑰。</td></tr><tr>
<td><code>containers-kubernetes.cluster.create</code></td>
<td>已建立叢集。</td></tr><tr>
<td><code>containers-kubernetes.cluster.delete</code></td>
<td>已刪除叢集。</td></tr><tr>
<td><code>containers-kubernetes.cluster-feature.enable</code></td>
<td>已在叢集上啟用適用於裸機工作者節點的「授信運算」這類特性。</td></tr><tr>
<td><code>containers-kubernetes.cluster.get</code></td>
<td>已檢視叢集資訊。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.create</code></td>
<td>已建立日誌轉遞配置。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.delete</code></td>
<td>已刪除日誌轉遞配置。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.get</code></td>
<td>已檢視日誌轉遞配置的資訊。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.update</code></td>
<td>已更新日誌轉遞配置。</td></tr><tr>
<td><code>containers-kubernetes.logging-config.refresh</code></td>
<td>已重新整理日誌轉遞配置。</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.create</code></td>
<td>已建立記載過濾器。</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.delete</code></td>
<td>已刪除記載過濾器。</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.get</code></td>
<td>已檢視記載過濾器的資訊。</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.update</code></td>
<td>已更新記載過濾器。</td></tr><tr>
<td><code>containers-kubernetes.logging-autoupdate.changed</code></td>
<td>已啟用或停用記載附加程式自動更新程式。</td></tr><tr>
<td><code>containers-kubernetes.mzlb.create</code></td>
<td>已建立多區域負載平衡器。</td></tr><tr>
<td><code>containers-kubernetes.mzlb.delete</code></td>
<td>已刪除多區域負載平衡器。</td></tr><tr>
<td><code>containers-kubernetes.service.bind</code></td>
<td>服務已連結至叢集。</td></tr><tr>
<td><code>containers-kubernetes.service.unbind</code></td>
<td>已取消服務與叢集的連結。</td></tr><tr>
<td><code>containers-kubernetes.subnet.add</code></td>
<td>現有的 IBM Cloud 基礎架構 (SoftLayer) 子網路已新增至叢集。</td></tr><tr>
<td><code>containers-kubernetes.subnet.create</code></td>
<td>已建立子網路。</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.add</code></td>
<td>使用者管理的子網路已新增至叢集。</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.delete</code></td>
<td>已從叢集移除使用者管理的子網路。</td></tr><tr>
<td><code>containers-kubernetes.version.update</code></td>
<td>已更新叢集主要節點的 Kubernetes 版本。</td></tr><tr>
<td><code>containers-kubernetes.worker.create</code></td>
<td>已建立工作者節點。</td></tr><tr>
<td><code>containers-kubernetes.worker.delete</code></td>
<td>已刪除工作者節點。</td></tr><tr>
<td><code>containers-kubernetes.worker.get</code></td>
<td>已檢視工作者節點的資訊。</td></tr><tr>
<td><code>containers-kubernetes.worker.reboot</code></td>
<td>已重新啟動工作者節點。</td></tr><tr>
<td><code>containers-kubernetes.worker.reload</code></td>
<td>已重新載入工作者節點。</td></tr><tr>
<td><code>containers-kubernetes.worker.update</code></td>
<td>已更新工作者節點。</td></tr>
</table>

## 追蹤 Kubernetes 審核事件
{: #kube-events}

請查看下列表格，以取得傳送至 {{site.data.keyword.cloudaccesstrailshort}} 之 Kubernetes API 伺服器審核事件的清單。
{: shortdesc}

開始之前：請確定您的叢集已配置為轉遞 [Kubernetes API 審核事件](/docs/containers?topic=containers-health#api_forward)。

<table>
    <th>動作</th>
    <th>說明</th><tr>
    <td><code>bindings.create</code></td>
    <td>已建立連結。</td></tr><tr>
    <td><code>certificatesigningrequests.create</code></td>
    <td>已建立簽署憑證要求。</td></tr><tr>
    <td><code>certificatesigningrequests.delete</code></td>
    <td>已刪除簽署憑證要求。</td></tr><tr>
    <td><code>certificatesigningrequests.patch</code></td>
    <td>已修補簽署憑證要求。</td></tr><tr>
    <td><code>certificatesigningrequests.update</code></td>
    <td>已更新簽署憑證的要求。</td></tr><tr>
    <td><code>clusterbindings.create</code></td>
    <td>已建立叢集角色連結。</td></tr><tr>
    <td><code>clusterbindings.deleted</code></td>
    <td>已刪除叢集角色連結。</td></tr><tr>
    <td><code>clusterbindings.patched</code></td>
    <td>已修補叢集角色連結。</td></tr><tr>
    <td><code>clusterbindings.updated</code></td>
    <td>已更新叢集角色連結。</td></tr><tr>
    <td><code>clusterroles.create</code></td>
    <td>已建立叢集角色。</td></tr><tr>
    <td><code>clusterroles.deleted</code></td>
    <td>已刪除叢集角色。</td></tr><tr>
    <td><code>clusterroles.patched</code></td>
    <td>已修補叢集角色。</td></tr><tr>
    <td><code>clusterroles.updated</code></td>
    <td>已更新叢集角色。</td></tr><tr>
    <td><code>configmaps.create</code></td>
    <td>已建立配置對映。</td></tr><tr>
    <td><code>configmaps.delete</code></td>
    <td>已刪除配置對映。</td></tr><tr>
    <td><code>configmaps.patch</code></td>
    <td>已修補配置對映。</td></tr><tr>
    <td><code>configmaps.update</code></td>
    <td>已更新配置對映。</td></tr><tr>
    <td><code>controllerrevisions.create</code></td>
    <td>已建立控制器修訂。</td></tr><tr>
    <td><code>controllerrevisions.delete</code></td>
    <td>已刪除控制器修訂。</td></tr><tr>
    <td><code>controllerrevisions.patch</code></td>
    <td>已修補控制器修訂。</td></tr><tr>
    <td><code>controllerrevisions.update</code></td>
    <td>已更新控制器修訂。</td></tr><tr>
    <td><code>daemonsets.create</code></td>
    <td>已建立常駐程式集。</td></tr><tr>
    <td><code>daemonsets.delete</code></td>
    <td>已刪除常駐程式集。</td></tr><tr>
    <td><code>daemonsets.patch</code></td>
    <td>已修補常駐程式集。</td></tr><tr>
    <td><code>daemonsets.update</code></td>
    <td>已更新常駐程式集。</td></tr><tr>
    <td><code>deployments.create</code></td>
    <td>已建立部署。</td></tr><tr>
    <td><code>deployments.delete</code></td>
    <td>已刪除部署。</td></tr><tr>
    <td><code>deployments.patch</code></td>
    <td>已修補部署。</td></tr><tr>
    <td><code>deployments.update</code></td>
    <td>已更新部署。</td></tr><tr>
    <td><code>events.create</code></td>
    <td>已建立事件。</td></tr><tr>
    <td><code>events.delete</code></td>
    <td>已刪除事件。</td></tr><tr>
    <td><code>events.patch</code></td>
    <td>已修補事件。</td></tr><tr>
    <td><code>events.update</code></td>
    <td>已更新事件。</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.create</code></td>
    <td>在 Kubernetes 1.8 版中，已建立外部許可連結鉤配置。</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.delete</code></td>
    <td>在 Kubernetes 1.8 版中，已刪除外部許可連結鉤配置。</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.patch</code></td>
    <td>在 Kubernetes 1.8 版中，已修補外部許可連結鉤配置。</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.update</code></td>
    <td>在 Kubernetes 1.8 版中，已更新外部許可連結鉤配置。</td></tr><tr>
    <td><code>horizontalpodautoscalers.create</code></td>
    <td>已建立水平 Pod 自動調整原則。</td></tr><tr>
    <td><code>horizontalpodautoscalers.delete</code></td>
    <td>已刪除水平 Pod 自動調整原則。</td></tr><tr>
    <td><code>horizontalpodautoscalers.patch</code></td>
    <td>已修補水平 Pod 自動調整原則。</td></tr><tr>
    <td><code>horizontalpodautoscalers.update</code></td>
    <td>已更新水平 Pod 自動調整原則。</td></tr><tr>
    <td><code>ingresses.create</code></td>
    <td>已建立 Ingress ALB。</td></tr><tr>
    <td><code>ingresses.delete</code></td>
    <td>已刪除 Ingress ALB。</td></tr><tr>
    <td><code>ingresses.patch</code></td>
    <td>已修補 Ingress ALB。</td></tr><tr>
    <td><code>ingresses.update</code></td>
    <td>已更新 Ingress ALB。</td></tr><tr>
    <td><code>jobs.create</code></td>
    <td>已建立工作。</td></tr><tr>
    <td><code>jobs.delete</code></td>
    <td>已刪除工作。</td></tr><tr>
    <td><code>jobs.patch</code></td>
    <td>已修補工作。</td></tr><tr>
    <td><code>jobs.update</code></td>
    <td>已更新工作。</td></tr><tr>
    <td><code>localsubjectaccessreviews.create</code></td>
    <td>已建立本端主題存取檢閱。</td></tr><tr>
    <td><code>limitranges.create</code></td>
    <td>已建立範圍限制。</td></tr><tr>
    <td><code>limitranges.delete</code></td>
    <td>已刪除範圍限制。</td></tr><tr>
    <td><code>limitranges.patch</code></td>
    <td>已修補範圍限制。</td></tr><tr>
    <td><code>limitranges.update</code></td>
    <td>已更新範圍限制。</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.create</code></td>
    <td>已建立更改 Webhook 配置。</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.delete</code></td>
    <td>已刪除更改 Webhook 配置。</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.patch</code></td>
    <td>已修補更改 Webhook 配置。</td></tr><tr>
    <td><code>mutatingwebhookconfigurations.update</code></td>
    <td>已更新更改 Webhook 配置。</td></tr><tr>
    <td><code>namespaces.create</code></td>
    <td>已建立名稱空間。</td></tr><tr>
    <td><code>namespaces.delete</code></td>
    <td>已刪除名稱空間。</td></tr><tr>
    <td><code>namespaces.patch</code></td>
    <td>已修補名稱空間。</td></tr><tr>
    <td><code>namespaces.update</code></td>
    <td>已更新名稱空間。</td></tr><tr>
    <td><code>networkpolicies.create</code></td>
    <td>已建立網路原則。</td></tr><tr><tr>
    <td><code>networkpolicies.delete</code></td>
    <td>已刪除網路原則。</td></tr><tr>
    <td><code>networkpolicies.patch</code></td>
    <td>已修補網路原則。</td></tr><tr>
    <td><code>networkpolicies.update</code></td>
    <td>已更新網路原則。</td></tr><tr>
    <td><code>nodes.create</code></td>
    <td>已建立節點。</td></tr><tr>
    <td><code>nodes.delete</code></td>
    <td>已刪除節點。</td></tr><tr>
    <td><code>nodes.patch</code></td>
    <td>已修補節點。</td></tr><tr>
    <td><code>nodes.update</code></td>
    <td>已更新節點。</td></tr><tr>
    <td><code>persistentvolumeclaims.create</code></td>
    <td>已建立持續性磁區要求。</td></tr><tr>
    <td><code>persistentvolumeclaims.delete</code></td>
    <td>已刪除持續性磁區要求。</td></tr><tr>
    <td><code>persistentvolumeclaims.patch</code></td>
    <td>已修補持續性磁區要求。</td></tr><tr>
    <td><code>persistentvolumeclaims.update</code></td>
    <td>已更新持續性磁區要求。</td></tr><tr>
    <td><code>persistentvolumes.create</code></td>
    <td>已建立持續性磁區。</td></tr><tr>
    <td><code>persistentvolumes.delete</code></td>
    <td>已刪除持續性磁區。</td></tr><tr>
    <td><code>persistentvolumes.patch</code></td>
    <td>已修補持續性磁區。</td></tr><tr>
    <td><code>persistentvolumes.update</code></td>
    <td>已更新持續性磁區。</td></tr><tr>
    <td><code>poddisruptionbudgets.create</code></td>
    <td>已建立 Pod 中斷預算。</td></tr><tr>
    <td><code>poddisruptionbudgets.delete</code></td>
    <td>已刪除 Pod 中斷預算。</td></tr><tr>
    <td><code>poddisruptionbudgets.patch</code></td>
    <td>已修補 Pod 中斷預算。</td></tr><tr>
    <td><code>poddisruptionbudgets.update</code></td>
    <td>已更新 Pod 中斷預算。</td></tr><tr>
    <td><code>podpresets.create</code></td>
    <td>已建立 Pod 預設。</td></tr><tr>
    <td><code>podpresets.deleted</code></td>
    <td>已刪除 Pod 預設。</td></tr><tr>
    <td><code>podpresets.patched</code></td>
    <td>已修補 Pod 預設。</td></tr><tr>
    <td><code>podpresets.updated</code></td>
    <td>已更新 Pod 預設。</td></tr><tr>
    <td><code>pods.create</code></td>
    <td>已建立 Pod。</td></tr><tr>
    <td><code>pods.delete</code></td>
    <td>已刪除 Pod。</td></tr><tr>
    <td><code>pods.patch</code></td>
    <td>已修補 Pod。</td></tr><tr>
    <td><code>pods.update</code></td>
    <td>已更新 Pod。</td></tr><tr>
    <td><code>podsecuritypolicies.create</code></td>
    <td>已建立 Pod 安全原則。</td></tr><tr>
    <td><code>podsecuritypolicies.delete</code></td>
    <td>已刪除 Pod 安全原則。</td></tr><tr>
    <td><code>podsecuritypolicies.patch</code></td>
    <td>已修補 Pod 安全原則。</td></tr><tr>
    <td><code>podsecuritypolicies.update</code></td>
    <td>已更新 Pod 安全原則。</td></tr><tr>
    <td><code>podtemplates.create</code></td>
    <td>已建立 Pod 範本。</td></tr><tr>
    <td><code>podtemplates.delete</code></td>
    <td>已刪除 Pod 範本。</td></tr><tr>
    <td><code>podtemplates.patch</code></td>
    <td>已修補 Pod 範本。</td></tr><tr>
    <td><code>podtemplates.update</code></td>
    <td>已更新 Pod 範本。</td></tr><tr>
    <td><code>replicasets.create</code></td>
    <td>已建立抄本集。</td></tr><tr>
    <td><code>replicasets.delete</code></td>
    <td>已刪除抄本集。</td></tr><tr>
    <td><code>replicasets.patch</code></td>
    <td>已修補抄本集。</td></tr><tr>
    <td><code>replicasets.update</code></td>
    <td>已更新抄本集。</td></tr><tr>
    <td><code>replicationcontrollers.create</code></td>
    <td>已建立抄寫控制器。</td></tr><tr>
    <td><code>replicationcontrollers.delete</code></td>
    <td>已刪除抄寫控制器。</td></tr><tr>
    <td><code>replicationcontrollers.patch</code></td>
    <td>已修補抄寫控制器。</td></tr><tr>
    <td><code>replicationcontrollers.update</code></td>
    <td>已更新抄寫控制器。</td></tr><tr>
    <td><code>resourcequotas.create</code></td>
    <td>已建立資源配額。</td></tr><tr>
    <td><code>resourcequotas.delete</code></td>
    <td>已刪除資源配額。</td></tr><tr>
    <td><code>resourcequotas.patch</code></td>
    <td>已修補資源配額。</td></tr><tr>
    <td><code>resourcequotas.update</code></td>
    <td>已更新資源配額。</td></tr><tr>
    <td><code>rolebindings.create</code></td>
    <td>已建立角色連結。</td></tr><tr>
    <td><code>rolebindings.deleted</code></td>
    <td>已刪除角色連結。</td></tr><tr>
    <td><code>rolebindings.patched</code></td>
    <td>已修補角色連結。</td></tr><tr>
    <td><code>rolebindings.updated</code></td>
    <td>已更新角色連結。</td></tr><tr>
    <td><code>roles.create</code></td>
    <td>已建立角色。</td></tr><tr>
    <td><code>roles.deleted</code></td>
    <td>已刪除角色。</td></tr><tr>
    <td><code>roles.patched</code></td>
    <td>已修補角色。</td></tr><tr>
    <td><code>roles.updated</code></td>
    <td>已更新角色。</td></tr><tr>
    <td><code>secrets.create</code></td>
    <td>已建立密碼。</td></tr><tr>
    <td><code>secrets.deleted</code></td>
    <td>已刪除密碼。</td></tr><tr>
    <td><code>secrets.get</code></td>
    <td>已檢視密碼。</td></tr><tr>
    <td><code>secrets.patch</code></td>
    <td>已修補密碼。</td></tr><tr>
    <td><code>secrets.updated</code></td>
    <td>已更新密碼。</td></tr><tr>
    <td><code>selfsubjectaccessreviews.create</code></td>
    <td>已建立自我主題存取檢閱。</td></tr><tr>
    <td><code>selfsubjectrulesreviews.create</code></td>
    <td>已建立自我主題規則檢閱。</td></tr><tr>
    <td><code>subjectaccessreviews.create</code></td>
    <td>已建立主題存取檢閱。</td></tr><tr>
    <td><code>serviceaccounts.create</code></td>
    <td>已建立服務帳戶。</td></tr><tr>
    <td><code>serviceaccounts.deleted</code></td>
    <td>已刪除服務帳戶。</td></tr><tr>
    <td><code>serviceaccounts.patch</code></td>
    <td>已修補服務帳戶。</td></tr><tr>
    <td><code>serviceaccounts.updated</code></td>
    <td>已更新服務帳戶。</td></tr><tr>
    <td><code>services.create</code></td>
    <td>已建立服務。</td></tr><tr>
    <td><code>services.deleted</code></td>
    <td>已刪除服務。</td></tr><tr>
    <td><code>services.patch</code></td>
    <td>已修補服務。</td></tr><tr>
    <td><code>services.updated</code></td>
    <td>已更新服務。</td></tr><tr>
    <td><code>statefulsets.create</code></td>
    <td>已建立有狀態集合。</td></tr><tr>
    <td><code>statefulsets.delete</code></td>
    <td>已刪除有狀態集合。</td></tr><tr>
    <td><code>statefulsets.patch</code></td>
    <td>已修補有狀態集合。</td></tr><tr>
    <td><code>statefulsets.update</code></td>
    <td>已更新有狀態集合。</td></tr><tr>
    <td><code>tokenreviews.create</code></td>
    <td>已建立記號檢閱。</td></tr><tr>
    <td><code>validatingwebhookconfigurations.create</code></td>
    <td>已建立 Webhook 配置驗證。</td></tr><tr>
    <td><code>validatingwebhookconfigurations.delete</code></td>
    <td>已刪除 Webhook 配置驗證。</td></tr><tr>
    <td><code>validatingwebhookconfigurations.patch</code></td>
    <td>已修補 Webhook 配置驗證。</td></tr><tr>
    <td><code>validatingwebhookconfigurations.update</code></td>
    <td>已更新 Webhook 配置驗證。</td></tr>
</table>
