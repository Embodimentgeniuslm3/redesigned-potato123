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


# 版本變更日誌
{: #changelog}

檢視可供 {{site.data.keyword.containerlong}} Kubernetes 叢集使用之主要、次要及修補程式更新的版本變更資訊。變更包含 Kubernetes 及 {{site.data.keyword.Bluemix_notm}} Provider 元件的更新。
{:shortdesc}

除非在變更日誌中另有說明，否則 {{site.data.keyword.containerlong_notm}} Provider 版本支援處於測試版的 Kubernetes API 和特性。會隨時變更的 Kubernetes Alpha 特性均已停用。

如需主要、次要和修補程式版本以及次要版本間之準備動作的相關資訊，請參閱 [Kubernetes 版本](/docs/containers?topic=containers-cs_versions)。
{: tip}

如需自舊版以來的變更相關資訊，請參閱下列變更日誌。
-  1.14 版[變更日誌](#114_changelog)。
-  1.13 版[變更日誌](#113_changelog)。
-  1.12 版[變更日誌](#112_changelog)。
-  **淘汰**：1.11 版[變更日誌](#111_changelog)。
-  不受支援版本之變更日誌的[保存檔](#changelog_archive)。

部分變更日誌適用於_工作者節點修正套件_，而且僅適用於工作者節點。您必須[套用這些修補程式](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)，以確保工作者節點的安全規範。這些工作者節點修正套件的版本可能比主要節點還要新，因為某些建置修正套件是專屬於工作者節點。其他變更日誌適用於_主節點修正套件_，而且僅適用於叢集主節點。可能未自動套用主要修正套件。您可以選擇[手動套用它們](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)。如需修補程式類型的相關資訊，請參閱[更新類型](/docs/containers?topic=containers-cs_versions#update_types)。
{: note}

</br>

## 1.14 版變更日誌
{: #114_changelog}

### 2019 年 6 月 4 日發佈的 1.14.2_1521 的變更日誌
{: #1142_1521}

下表顯示修補程式 1.14.2_1521 中包含的變更。
{: shortdesc}

<table summary="自 1.14.1_1519 版以來進行的變更">
<caption>自 1.14.1_1519 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集 DNS 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已修正錯誤，該錯誤可能導致 Kubernetes DNS 和 CoreDNS Pod 兩者在叢集 `create` 或 `update` 作業之後繼續執行。</td>
</tr>
<tr>
<td>叢集主節點 HA 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已更新配置，將主節點更新期間的間歇性主節點網路連線功能失敗減到最少。</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>請參閱 [etcd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coreos/etcd/releases/v3.3.13)。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>針對 [CVE-2018-10844 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，已更新映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.14.1-71</td>
<td>v1.14.2-100</td>
<td>已更新為支援 Kubernetes 1.14.2 版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.14.1</td>
<td>v1.14.2</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.2)。</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>0.3.1 版</td>
<td>0.3.3 版</td>
<td>請參閱 [Kubernetes Metrics Server 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3)。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>針對 [CVE-2018-10844 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，已更新映像。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 20 日發佈的工作者節點 FP1.14.1_1519 的變更日誌
{: #1141_1519}

下表顯示修補程式 1.14.1_1519 中包含的變更。
{: shortdesc}

<table summary="自 1.14.1_1518 版以來進行的變更">
<caption>自 1.14.1_1518 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 核心</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>下列各項的更新工作者節點映像檔及核心更新：[CVE-2018-12126 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 及 [CVE-2018-12130 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)。</td>
</tr>
<tr>
<td>Ubuntu 18.04 核心</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>下列各項的更新工作者節點映像檔及核心更新：[CVE-2018-12126 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 及 [CVE-2018-12130 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 13 日發佈的 1.14.1_1518 的變更日誌
{: #1141_1518}

下表顯示修補程式 1.14.1_1518 中包含的變更。
{: shortdesc}

<table summary="自 1.14.1_1516 版以來進行的變更">
<caption>自 1.14.1_1516 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點 HA Proxy</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>請參閱 [HAProxy 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。此更新解決 [CVE-2019-6706 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>Kubernetes API 伺服器審核原則配置已更新為不記載 `/openapi/v2*` 唯讀 URL。此外，Kubernetes 控制器管理程式配置將已簽署之 `kubelet` 憑證的有效期間從 1 年增加到 3 年。</td>
</tr>
<tr>
<td>OpenVPN 用戶端配置</td>
<td>N/A</td>
<td>N/A</td>
<td>`kube-system` 名稱空間中的 OpenVPN 用戶端 `vpn-*` Pod 現在會將 `dnsPolicy` 設為 `Default` 以避免 Pod 在叢集 DNS 關閉時失敗。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>e7182c7</td>
<td>13c7ef0</td>
<td>已更新 [CVE-2016-7076 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076) 及 [CVE-2017-1000368 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) 的映像檔。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 7 日發佈的 1.14.1_1516 的變更日誌
{: #1141_1516}

下表顯示修補程式 1.14.1_1516 中包含的變更。
{: shortdesc}

<table summary="自 1.13.5_1519 版以來進行的變更">
<caption>自 1.13.5_1519 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.4.4</td>
<td>v3.6.1</td>
<td>請參閱 [Calico 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.6/release-notes/)。</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.6</td>
<td>1.3.1</td>
<td>請參閱 [CoreDNS 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://coredns.io/2019/01/13/coredns-1.3.1-release/)。更新包括在叢集 DNS 服務上[度量埠的新增項目 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://coredns.io/plugins/metrics/)。<br><br>現在，CoreDNS 是唯一支援的叢集 DNS 提供者。如果將叢集從 Kubernetes 的較早版本更新到 1.14 版，並且使用的是 KubeDNS，則在叢集更新期間，KubeDNS 會自動移轉到 CoreDNS。如需相關資訊或要在更新之前測試 CoreDNS，請參閱[配置叢集 DNS 提供者](https://cloud.ibm.com/docs/containers?topic=containers-cluster_dns#cluster_dns)。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>9ff3fda</td>
<td>ed0dafc</td>
<td>已更新 [CVE-2019-1543 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 的映像檔。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.13.5-107 版</td>
<td>v1.14.1-71</td>
<td>已更新為支援 Kubernetes 1.14.1 版。此外，`calicoctl` 版本更新為 3.6.1。修正了對只有一個工作者節點可用於負載平衡器 Pod 的 2.0 版負載平衡器的更新。現在，專用負載平衡器支援在[專用邊緣工作者節點](/docs/containers?topic=containers-edge#edge)上執行。</td>
</tr>
<tr>
<td>IBM Pod 安全原則</td>
<td>N/A</td>
<td>N/A</td>
<td>[IBM Pod 安全原則](/docs/containers?topic=containers-psp#ibm_psp)更新為支援 Kubernetes [RunAsGroup ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#users-and-groups) 特性。</td>
</tr>
<tr>
<td>`kubelet` 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>將 `--pod-max-pids` 選項設定為 `14336`，以阻止單個 Pod 使用工作者節點上的所有程序 ID。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.13.5 版</td>
<td>v1.14.1</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.1) 和 [Kubernetes 1.14 部落格 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/blog/2019/03/25/kubernetes-1-14-release-announcement/)。<br><br>Kubernetes 預設的角色型存取控制 (RBAC) 原則不再授與[未經鑑別之使用者的探索及許可權檢查 API ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles) 存取權。這項變更僅適用於新的 1.14 版叢集。如果您從舊版本更新叢集，未經鑑別的使用者仍可存取探索及許可權檢查 API。</td>
</tr>
<tr>
<td>Kubernetes 許可控制器配置</td>
<td>N/A</td>
<td>N/A</td>
<td><ul>
<li>已針對叢集的 Kubernetes API 伺服器將 `NodeRestriction` 新增至 `--enable-admission-plugins` 選項，並將相關叢集資源配置為支援此安全加強功能。</li>
<li>已從叢集的 Kubernetes API 伺服器的 `--enable-admission-plugins` 選項移除 `Initializers`，且從 `--runtime-config` 選項中移除 `admissionregistration.k8s.io/v1alpha1=true`，因為不再支援這些 API。可以改為使用 [Kubernetes 許可 Webhook ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/)。</li></ul></td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.3.0</td>
<td>1.4.0</td>
<td>請參閱 [Kubernetes DNS Autoscaler 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.4.0)。</td>
</tr>
<tr>
<td>Kubernetes 特性關卡配置</td>
<td>N/A</td>
<td>N/A</td>
<td><ul>
  <li>已新增 `RuntimeClass=false`，以停用容器運行環境配置的選項。</li>
  <li>已移除 `ExperimentalCriticalPodAnnotation=true`，因為不再支援 `scheduler.alpha.kubernetes.io/critical-pod` Pod 註釋。可以改為使用 [Kubernetes Pod 優先順序 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/docs/containers?topic=containers-pod_priority#pod_priority)。</li></ul></td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>e132aa4</td>
<td>e7182c7</td>
<td>針對 [CVE-2019-11068 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068)，已更新映像檔。</td>
</tr>
</tbody>
</table>

<br />


## 1.13 版變更日誌
{: #113_changelog}

### 2019 年 6 月 4 日發佈的 1.13.6_1524 的變更日誌
{: #1136_1524}

下表顯示修補程式 1.13.6_1524 中包含的變更。
{: shortdesc}

<table summary="自 1.13.6_1522 版以來進行的變更">
<caption>自 1.13.6_1522 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集 DNS 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已修正錯誤，該錯誤可能導致 Kubernetes DNS 和 CoreDNS Pod 兩者在叢集 `create` 或 `update` 作業之後繼續執行。</td>
</tr>
<tr>
<td>叢集主節點 HA 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已更新配置，將主節點更新期間的間歇性主節點網路連線功能失敗減到最少。</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>請參閱 [etcd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coreos/etcd/releases/v3.3.13)。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>針對 [CVE-2018-10844 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，已更新映像。</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>0.3.1 版</td>
<td>0.3.3 版</td>
<td>請參閱 [Kubernetes Metrics Server 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3)。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>針對 [CVE-2018-10844 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，已更新映像。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 20 日發佈的工作者節點 FP1.13.6_1522 的變更日誌
{: #1136_1522}

下表顯示修補程式 1.13.6_1522 中包含的變更。
{: shortdesc}

<table summary="自 1.13.6_1521 版以來進行的變更">
<caption>自 1.13.6_1521 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 核心</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>下列各項的更新工作者節點映像檔及核心更新：[CVE-2018-12126 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 及 [CVE-2018-12130 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)。</td>
</tr>
<tr>
<td>Ubuntu 18.04 核心</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>下列各項的更新工作者節點映像檔及核心更新：[CVE-2018-12126 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 及 [CVE-2018-12130 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 13 日發佈的 1.13.6_1521 的變更日誌
{: #1136_1521}

下表顯示修補程式 1.13.6_1521 中包含的變更。
{: shortdesc}

<table summary="自 1.13.5_1519 版以來進行的變更">
<caption>自 1.13.5_1519 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點 HA Proxy</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>請參閱 [HAProxy 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。此更新解決 [CVE-2019-6706 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>已更新 [CVE-2019-1543 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 的映像檔。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.13.5-107 版</td>
<td>1.13.6-139 版</td>
<td>已更新為支援 Kubernetes 1.13.6 版。此外，也已修正 2.0 版負載平衡器的更新程序，針對負載平衡器 Pod 只有一個可用的工作者節點。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.13.5 版</td>
<td>1.13.6 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.6)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>Kubernetes API 伺服器審核原則配置已更新為不記載 `/openapi/v2*` 唯讀 URL。此外，Kubernetes 控制器管理程式配置將已簽署之 `kubelet` 憑證的有效期間從 1 年增加到 3 年。</td>
</tr>
<tr>
<td>OpenVPN 用戶端配置</td>
<td>N/A</td>
<td>N/A</td>
<td>`kube-system` 名稱空間中的 OpenVPN 用戶端 `vpn-*` Pod 現在會將 `dnsPolicy` 設為 `Default` 以避免 Pod 在叢集 DNS 關閉時失敗。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>已更新 [CVE-2016-7076 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076)、[CVE-2017-1000368 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) 及 [CVE-2019-11068 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068) 的映像檔。</td>
</tr>
</tbody>
</table>

### 2019 年 4 月 29 日發佈的工作者節點 FP1.13.5_1519 的變更日誌
{: #1135_1519}

下表顯示工作者節點 FP1.13.5_1519 中包含的變更。
{: shortdesc}

<table summary="自 1.13.5_1518 版以來進行的變更">
<caption>自 1.13.5_1518 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>已安裝 Ubuntu 套件的更新。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.5</td>
<td>1.2.6</td>
<td>請參閱 [containerd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.2.6)。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.13.5_1518（2019 年 4 月 15 日發行）的變更日誌
{: #1135_1518}

下表顯示工作者節點修正套件 1.13.5_1518 中所包括的變更。
{: shortdesc}

<table summary="自 1.13.5_1517 版以來進行的變更">
<caption>自 1.13.5_1517 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>更新為已安裝的 Ubuntu 套件，包括 [CVE-2019-3842 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html) 的 `systemd`。</td>
</tr>
</tbody>
</table>

### 1.13.5_1517（2019 年 4 月 8 日發行）的變更日誌
{: #1135_1517}

下表顯示修補程式 1.13.5_1517 中所包括的變更。
{: shortdesc}

<table summary="自 1.13.4_1516 版以來進行的變更">
<caption>自 1.13.4_1516 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>3.4.0 版</td>
<td>v3.4.4</td>
<td>請參閱 [Calico 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.4/releases/#v344)。此更新解決 [CVE-2019-9946 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946)。</td>
</tr>
<tr>
<td>叢集主節點 HA Proxy</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>請參閱 [HAProxy 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。此更新解決 [CVE-2018-0732 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732)、[CVE-2018-0734 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734)、[CVE-2018-0737 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)、[CVE-2018-5407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)、[CVE-2019-1543 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 及 [CVE-2019-1559 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.13.4-86 版</td>
<td>1.13.5-107 版</td>
<td>已更新為支援 Kubernetes 1.13.5 版及 Calico 3.4.4 版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.13.4 版</td>
<td>1.13.5 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.5)。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>已更新 [CVE-2017-12447 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447) 的映像檔。</td>
</tr>
<tr>
<td>Ubuntu 16.04 核心</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2019-9213 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的核心更新。</td>
</tr>
<tr>
<td>Ubuntu 18.04 核心</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2019-9213 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的核心更新。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.13.4_1516（2019 年 4 月 1 日發行）的變更日誌
{: #1134_1516}

下表顯示工作者節點修正套件 1.13.4_1516 中所包括的變更。
{: shortdesc}

<table summary="自 1.13.4_1515 版以來進行的變更">
<caption>自 1.13.4_1515 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作者節點資源使用率</td>
<td>N/A</td>
<td>N/A</td>
<td>增加 kubelet 及 containerd 的記憶體保留，以防止這些元件耗盡資源。如需相關資訊，請參閱[工作者節點資源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>

### 主節點修正套件 1.13.4_1515（2019 年 3 月 26 日發行）的變更日誌
{: #1134_1515}

下表顯示主節點修正套件 1.13.4_1515 中所包括的變更。
{: shortdesc}

<table summary="自 1.13.4_1513 版以來進行的變更">
<caption>自 1.13.4_1513 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集 DNS 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>修正 Kubernetes 1.11 版中的更新處理程序，以防止更新從叢集 DNS 提供者切換至 CoreDNS。在更新之後，您仍可以[將 CoreDNS 設為叢集 DNS 提供者](/docs/containers?topic=containers-cluster_dns#set_coredns)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>345</td>
<td>346</td>
<td>已更新 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的映像檔。</td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>166</td>
<td>167</td>
<td>修正用來管理 Kubernetes 密碼的間歇性 `context deadline exceeded` 及 `timeout` 錯誤。此外，還會修正可能未加密現有 Kubernetes 密碼的金鑰管理服務更新。此更新包括 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的修正程式。</td>
</tr>
<tr>
<td>適用於 {{site.data.keyword.Bluemix_notm}} 提供者的負載平衡器及負載平衡器監視器</td>
<td>143</td>
<td>146</td>
<td>已更新 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的映像檔。</td>
</tr>
</tbody>
</table>

### 1.13.4_1513（2019 年 3 月 20 日發行）的變更日誌
{: #1134_1513}

下表顯示修補程式 1.13.4_1513 中所包括的變更。
{: shortdesc}

<table summary="自 1.13.4_1510 版以來進行的變更">
<caption>自 1.13.4_1510 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集 DNS 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已修正在必須縮減未使用的叢集 DNS 時，可能導致主要叢集作業（例如，`refresh` 或 `update`）發生失敗的錯誤。</td>
</tr>
<tr>
<td>叢集主節點 HA Proxy 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已更新配置，以更妥善地處理連至叢集主節點時的間歇性連線失敗。</td>
</tr>
<tr>
<td>CoreDNS 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已更新 CoreDNS 配置，以在從 1.12 版更新叢集 Kubernetes 版本之後，支援[多個 Corefile ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://coredns.io/2017/07/23/corefile-explained/)。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.4</td>
<td>1.2.5</td>
<td>請參閱 [containerd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.2.5)。此更新包括 [CVE-2019-5736 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736) 的改良修正程式。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>已將 GPU 驅動程式更新為 [418.43 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.nvidia.com/object/unix.html)。此更新包括 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html) 的修正程式。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>344</td>
<td>345</td>
<td>已新增對[專用服務端點](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)的支援。</td>
</tr>
<tr>
<td>核心</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2019-6133 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) 的核心更新。</td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>136</td>
<td>166</td>
<td>已更新 [CVE-2018-16890 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890)、[CVE-2019-3822 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) 及 [CVE-2019-3823 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823) 的映像檔。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>已更新 [CVE-2018-10779 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779)、[CVE-2018-12900 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900)、[CVE-2018-17000 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000)、[CVE-2018-19210 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210)、[CVE-2019-6128 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128)、[CVE-2019-7663 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663) 的映像檔。</td>
</tr>
</tbody>
</table>

### 1.13.4_1510（2019 年 3 月 4 日發行）的變更日誌
{: #1134_1510}

下表顯示修補程式 1.13.4_1510 中所包括的變更。
{: shortdesc}

<table summary="自 1.13.2_1509 版以來進行的變更">
<caption>自 1.13.2_1509 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集 DNS 提供者</td>
<td>N/A</td>
<td>N/A</td>
<td>已增加 Kubernetes DNS 及 CoreDNS Pod 記憶體限制，從 `170Mi` 增加為 `400Mi`，以處理更多叢集服務。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>已更新 [CVE-2019-6454 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) 的映像檔。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.13.2-62 版</td>
<td>1.13.4-86 版</td>
<td>已更新為支援 Kubernetes 1.13.4 版。已針對 `externalTrafficPolicy` 設為 `local` 的 1.0 版負載平衡器，修正定期連線問題。已將負載平衡器 1.0 版及 2.0 版事件更新為使用最新的 {{site.data.keyword.Bluemix_notm}} 文件鏈結。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>342</td>
<td>344</td>
<td>已將映像檔的基本作業系統從 Fedora 變更為 Alpine。已更新 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>122</td>
<td>136</td>
<td>已將用戶端逾時增加至 {{site.data.keyword.keymanagementservicefull_notm}}。已更新 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.13.2 版</td>
<td>1.13.4 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.4)。此更新解決 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 及 [CVE-2019-1002100 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已新增 `ExperimentalCriticalPodAnnotation=true` 至 `--feature-gates` 選項。此設定可協助將 Pod 從已淘汰的 `scheduler.alpha.kubernetes.io/critical-pod` 註釋移轉至 [Kubernetes Pod 優先順序支援](/docs/containers?topic=containers-pod_priority#pod_priority)。</td>
</tr>
<tr>
<td>適用於 {{site.data.keyword.Bluemix_notm}} 提供者的負載平衡器及負載平衡器監視器</td>
<td>132</td>
<td>143</td>
<td>已更新 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>OpenVPN 用戶端及伺服器</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>已更新 [CVE-2019-1559 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) 的映像檔。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>已更新 [CVE-2019-6454 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) 的映像檔。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.13.2_1509（2019 年 2 月 27 日發行）的變更日誌
{: #1132_1509}

下表顯示工作者節點修正套件 1.13.2_1509 中所包括的變更。
{: shortdesc}

<table summary="自 1.13.2_1508 版以來進行的變更">
<caption>自 1.13.2_1508 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-19407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) 的核心更新。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.13.2_1508（2019 年 2 月 15 日發行）的變更日誌
{: #1132_1508}

下表顯示工作者節點修正套件 1.13.2_1508 中所包括的變更。
{: shortdesc}

<table summary="自 1.13.2_1507 版以來進行的變更">
<caption>自 1.13.2_1507 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點 HA Proxy 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已將 Pod 配置 `spec.priorityClassName` 值變更為 `system-node-critical`，並將 `spec.priority` 值設為 `2000001000`。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.2</td>
<td>1.2.4</td>
<td>請參閱 [containerd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.2.4)。此更新解決 [CVE-2019-5736 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736)。</td>
</tr>
<tr>
<td>Kubernetes `kubelet` 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已啟用 `ExperimentalCriticalPodAnnotation` 特性閘道，以防止重要的靜態 Pod 收回。將 `event-qps` 選項設為 `0`，以防止對事件建立進行速率限制。</td>
</tr>
</tbody>
</table>

### 1.13.2_1507（2019 年 2 月 5 日發行）的變更日誌
{: #1132_1507}

下表顯示修補程式 1.13.2_1507 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.4_1535 版以來進行的變更">
<caption>自 1.12.4_1535 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>3.3.1 版</td>
<td>3.4.0 版</td>
<td>請參閱 [Calico 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.4/releases/#v340)。</td>
</tr>
<tr>
<td>叢集 DNS 提供者</td>
<td>N/A</td>
<td>N/A</td>
<td>現在，CoreDNS 是新叢集的預設叢集 DNS 提供者。如果您將現有的叢集更新至 1.13 版，其使用 KubeDNS 作為叢集 DNS 提供者，則 KubeDNS 會繼續成為叢集 DNS 提供者。不過，您可以選擇[改用 CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set)。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.2.2</td>
<td>請參閱 [containerd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.2.2)。</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.2</td>
<td>1.2.6</td>
<td>請參閱 [CoreDNS 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coredns/coredns/releases/tag/v1.2.6)。此外，CoreDNS 配置更新為[支援多個 Corefile ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://coredns.io/2017/07/23/corefile-explained/)。</td>
</tr>
<tr>
<td>etcd</td>
<td>3.3.1 版</td>
<td>v3.3.11</td>
<td>請參閱 [etcd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coreos/etcd/releases/v3.3.11)。此外，etcd 的受支援密碼組合現在限制為具有高強度加密（128 位元或以上）的子集。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>已更新 [CVE-2019-3462 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 及 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.12.4-118 版</td>
<td>1.13.2-62 版</td>
<td>已更新為支援 Kubernetes 1.13.2 版。此外，`calicoctl` 版本已更新為 3.4.0。已修正 2.0 版負載平衡器對於工作者節點狀態變更所做的不必要配置更新。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>338</td>
<td>342</td>
<td>檔案儲存空間外掛程式的更新如下：
<ul><li>支援使用[磁區拓蹼感知排程](/docs/containers?topic=containers-file_storage#file-topology)來進行動態佈建。</li>
<li>如果儲存空間已刪除，則忽略持續性磁區要求 (PVC) 刪除錯誤。</li>
<li>新增失敗訊息註釋至失敗的 PVC。</li>
<li>最佳化儲存空間佈建程式控制器的主導者選擇和重新同步期間設定，並將佈建逾時值從 30 分鐘增加到 1 小時。</li>
<li>開始佈建之前檢查使用者許可權。</li>
<li>將 `CriticalAddonsOnly` 容忍新增至 `kube-system` 名稱空間中的 `ibm-file-plugin` 及 `ibm-storage-watcher` 部署。</li></ul></td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>111</td>
<td>122</td>
<td>已新增重試邏輯，以防止在 Kubernetes 密碼由 {{site.data.keyword.keymanagementservicefull_notm}} 進行管理時發生暫時失敗。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.12.4 版</td>
<td>1.13.2 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.2)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>Kubernetes API 伺服器審核原則配置已更新為包括記載 `cluster-admin` 要求的 meta 資料，以及記載工作負載 `create`、`update` 及 `patch` 要求的要求內文。</td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.2.0</td>
<td>1.3.0</td>
<td>請參閱 [Kubernetes DNS Autoscaler 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.3.0)。</td>
</tr>
<tr>
<td>OpenVPN 用戶端</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>已更新 [CVE-2018-0734 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 及 [CVE-2018-5407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) 的映像檔。已將 `CriticalAddonsOnly` 容忍新增至 `kube-system` 名稱空間中的 `vpn` 部署。此外，現在會從密碼（而非從 configmap）取得 Pod 配置。</td>
</tr>
<tr>
<td>OpenVPN 伺服器</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>已更新 [CVE-2018-0734 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 及 [CVE-2018-5407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) 的映像檔。</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864) 的安全修補程式。</td>
</tr>
</tbody>
</table>

<br />


## 1.12 版變更日誌
{: #112_changelog}

檢閱 1.12 版變更日誌。
{: shortdesc}

### 2019 年 6 月 4 日發佈的 1.12.9_1555 的變更日誌
{: #1129_1555}

下表顯示修補程式 1.12.9_1555 中包含的變更。
{: shortdesc}

<table summary="自 1.12.8_1553 版以來進行的變更">
<caption>自 1.12.8_1553 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集 DNS 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已修正錯誤，該錯誤可能導致 Kubernetes DNS 和 CoreDNS Pod 兩者在叢集 `create` 或 `update` 作業之後繼續執行。</td>
</tr>
<tr>
<td>叢集主節點 HA 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已更新配置，將主節點更新期間的間歇性主節點網路連線功能失敗減到最少。</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>請參閱 [etcd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coreos/etcd/releases/v3.3.13)。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>針對 [CVE-2018-10844 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，已更新映像。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.12.8-210 版</td>
<td>1.12.9-227 版</td>
<td>已更新為支援 Kubernetes 1.12.9 版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.12.8 版</td>
<td>1.12.9 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.9)。</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>0.3.1 版</td>
<td>0.3.3 版</td>
<td>請參閱 [Kubernetes Metrics Server 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3)。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>針對 [CVE-2018-10844 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，已更新映像。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 20 日發佈的工作者節點 FP1.12.8_1553 的變更日誌
{: #1128_1533}

下表顯示修補程式 1.12.8_1553 中包含的變更。
{: shortdesc}

<table summary="自 1.12.8_1553 版以來進行的變更">
<caption>自 1.12.8_1553 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 核心</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>下列各項的更新工作者節點映像檔及核心更新：[CVE-2018-12126 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 及 [CVE-2018-12130 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)。</td>
</tr>
<tr>
<td>Ubuntu 18.04 核心</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>下列各項的更新工作者節點映像檔及核心更新：[CVE-2018-12126 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 及 [CVE-2018-12130 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 13 日發佈的 1.12.8_1552 的變更日誌
{: #1128_1552}

下表顯示修補程式 1.12.8_1552 中包含的變更。
{: shortdesc}

<table summary="自 1.12.7_1550 版以來進行的變更">
<caption>自 1.12.7_1550 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點 HA Proxy</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>請參閱 [HAProxy 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。此更新解決 [CVE-2019-6706 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>已更新 [CVE-2019-1543 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 的映像檔。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.12.7-180 版</td>
<td>1.12.8-210 版</td>
<td>已更新為支援 Kubernetes 1.12.8 版。此外，也已修正 2.0 版負載平衡器的更新程序，針對負載平衡器 Pod 只有一個可用的工作者節點。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.12.7 版</td>
<td>1.12.8 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.8)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>Kubernetes API 伺服器審核原則配置已更新為不記載 `/openapi/v2*` 唯讀 URL。此外，Kubernetes 控制器管理程式配置將已簽署之 `kubelet` 憑證的有效期間從 1 年增加到 3 年。</td>
</tr>
<tr>
<td>OpenVPN 用戶端配置</td>
<td>N/A</td>
<td>N/A</td>
<td>`kube-system` 名稱空間中的 OpenVPN 用戶端 `vpn-*` Pod 現在會將 `dnsPolicy` 設為 `Default` 以避免 Pod 在叢集 DNS 關閉時失敗。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>已更新 [CVE-2016-7076 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076)、[CVE-2017-1000368 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) 及 [CVE-2019-11068 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068) 的映像檔。</td>
</tr>
</tbody>
</table>

### 2019 年 4 月 29 日發佈的工作者節點 FP1.12.7_1550 的變更日誌
{: #1127_1550}

下表顯示工作者節點 FP1.12.7_1550 中包含的變更。
{: shortdesc}

<table summary="自 1.12.7_1549 版以來進行的變更">
<caption>自 1.12.7_1549 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>已安裝 Ubuntu 套件的更新。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>請參閱 [containerd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.1.7)。</td>
</tr>
</tbody>
</table>


### 工作者節點修正套件 1.12.7_1549（2019 年 4 月 15 日發行）的變更日誌
{: #1127_1549}

下表顯示工作者節點修正套件 1.12.7_1549 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.7_1548 版以來進行的變更">
<caption>自 1.12.7_1548 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>更新為已安裝的 Ubuntu 套件，包括 [CVE-2019-3842 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html) 的 `systemd`。</td>
</tr>
</tbody>
</table>

### 1.12.7_1548（2019 年 4 月 8 日發行）的變更日誌
{: #1127_1548}

下表顯示修補程式 1.12.7_1548 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.6_1547 版以來進行的變更">
<caption>自 1.12.6_1547 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>3.3.1 版</td>
<td>3.3.6 版</td>
<td>請參閱 [Calico 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.3/releases/#v336)。此更新解決 [CVE-2019-9946 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946)。</td>
</tr>
<tr>
<td>叢集主節點 HA Proxy</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>請參閱 [HAProxy 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。此更新解決 [CVE-2018-0732 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732)、[CVE-2018-0734 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734)、[CVE-2018-0737 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)、[CVE-2018-5407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)、[CVE-2019-1543 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 及 [CVE-2019-1559 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.12.6-157 版</td>
<td>1.12.7-180 版</td>
<td>已更新為支援 Kubernetes 1.12.7 版及 Calico 3.3.6 版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.12.6 版</td>
<td>1.12.7 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.7)。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>已更新 [CVE-2017-12447 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447) 的映像檔。</td>
</tr>
<tr>
<td>Ubuntu 16.04 核心</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2019-9213 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的核心更新。</td>
</tr>
<tr>
<td>Ubuntu 18.04 核心</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2019-9213 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的核心更新。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.12.6_1547（2019 年 4 月 1 日發行）的變更日誌
{: #1126_1547}

下表顯示工作者節點修正套件 1.12.6_1547 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.6_1546 版以來進行的變更">
<caption>自 1.12.6_1546 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作者節點資源使用率</td>
<td>N/A</td>
<td>N/A</td>
<td>增加 kubelet 及 containerd 的記憶體保留，以防止這些元件耗盡資源。如需相關資訊，請參閱[工作者節點資源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>


### 主節點修正套件 1.12.6_1546（2019 年 3 月 26 日發行）的變更日誌
{: #1126_1546}

下表顯示主節點修正套件 1.12.6_1546 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.6_1544 版以來進行的變更">
<caption>自 1.12.6_1544 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>345</td>
<td>346</td>
<td>已更新 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的映像檔。</td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>166</td>
<td>167</td>
<td>修正用來管理 Kubernetes 密碼的間歇性 `context deadline exceeded` 及 `timeout` 錯誤。此外，還會修正可能未加密現有 Kubernetes 密碼的金鑰管理服務更新。此更新包括 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的修正程式。</td>
</tr>
<tr>
<td>適用於 {{site.data.keyword.Bluemix_notm}} 提供者的負載平衡器及負載平衡器監視器</td>
<td>143</td>
<td>146</td>
<td>已更新 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的映像檔。</td>
</tr>
</tbody>
</table>

### 1.12.6_1544（2019 年 3 月 20 日發行）的變更日誌
{: #1126_1544}

下表顯示修補程式 1.12.6_1544 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.6_1541 版以來進行的變更">
<caption>自 1.12.6_1541 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集 DNS 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已修正在必須縮減未使用的叢集 DNS 時，可能導致主要叢集作業（例如，`refresh` 或 `update`）發生失敗的錯誤。</td>
</tr>
<tr>
<td>叢集主節點 HA Proxy 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已更新配置，以更妥善地處理連至叢集主節點時的間歇性連線失敗。</td>
</tr>
<tr>
<td>CoreDNS 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>CoreDNS 配置已更新為支援[多個 Corefile ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://coredns.io/2017/07/23/corefile-explained/)。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>已將 GPU 驅動程式更新為 [418.43 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.nvidia.com/object/unix.html)。此更新包括 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html) 的修正程式。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>344</td>
<td>345</td>
<td>已新增對[專用服務端點](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)的支援。</td>
</tr>
<tr>
<td>核心</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2019-6133 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) 的核心更新。</td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>136</td>
<td>166</td>
<td>已更新 [CVE-2018-16890 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890)、[CVE-2019-3822 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) 及 [CVE-2019-3823 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823) 的映像檔。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>已更新 [CVE-2018-10779 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779)、[CVE-2018-12900 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900)、[CVE-2018-17000 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000)、[CVE-2018-19210 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210)、[CVE-2019-6128 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128)、[CVE-2019-7663 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663) 的映像檔。</td>
</tr>
</tbody>
</table>

### 1.12.6_1541（2019 年 3 月 4 日發行）的變更日誌
{: #1126_1541}

下表顯示修補程式 1.12.6_1541 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.5_1540 版以來進行的變更">
<caption>自 1.12.5_1540 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集 DNS 提供者</td>
<td>N/A</td>
<td>N/A</td>
<td>已增加 Kubernetes DNS 及 CoreDNS Pod 記憶體限制，從 `170Mi` 增加為 `400Mi`，以處理更多叢集服務。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>已更新 [CVE-2019-6454 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) 的映像檔。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.12.5-137 版</td>
<td>1.12.6-157 版</td>
<td>已更新為支援 Kubernetes 1.12.6 版。已針對 `externalTrafficPolicy` 設為 `local` 的 1.0 版負載平衡器，修正定期連線問題。已將負載平衡器 1.0 版及 2.0 版事件更新為使用最新的 {{site.data.keyword.Bluemix_notm}} 文件鏈結。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>342</td>
<td>344</td>
<td>已將映像檔的基本作業系統從 Fedora 變更為 Alpine。已更新 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>122</td>
<td>136</td>
<td>已將用戶端逾時增加至 {{site.data.keyword.keymanagementservicefull_notm}}。已更新 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.12.5 版</td>
<td>1.12.6 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.6)。此更新解決 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 及 [CVE-2019-1002100 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已新增 `ExperimentalCriticalPodAnnotation=true` 至 `--feature-gates` 選項。此設定可協助將 Pod 從已淘汰的 `scheduler.alpha.kubernetes.io/critical-pod` 註釋移轉至 [Kubernetes Pod 優先順序支援](/docs/containers?topic=containers-pod_priority#pod_priority)。</td>
</tr>
<tr>
<td>適用於 {{site.data.keyword.Bluemix_notm}} 提供者的負載平衡器及負載平衡器監視器</td>
<td>132</td>
<td>143</td>
<td>已更新 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>OpenVPN 用戶端及伺服器</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>已更新 [CVE-2019-1559 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) 的映像檔。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>已更新 [CVE-2019-6454 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) 的映像檔。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.12.5_1540（2019 年 2 月 27 日發行）的變更日誌
{: #1125_1540}

下表顯示工作者節點修正套件 1.12.5_1540 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.5_1538 版以來進行的變更">
<caption>自 1.12.5_1538 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-19407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) 的核心更新。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.12.5_1538（2019 年 2 月 15 日發行）的變更日誌
{: #1125_1538}

下表顯示工作者節點修正套件 1.12.5_1538 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.5_1537 版以來進行的變更">
<caption>自 1.12.5_1537 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點 HA Proxy 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已將 Pod 配置 `spec.priorityClassName` 值變更為 `system-node-critical`，並將 `spec.priority` 值設為 `2000001000`。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>請參閱 [containerd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.1.6)。此更新解決 [CVE-2019-5736 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736)。</td>
</tr>
<tr>
<td>Kubernetes `kubelet` 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已啟用 `ExperimentalCriticalPodAnnotation` 特性閘道，以防止重要的靜態 Pod 收回。</td>
</tr>
</tbody>
</table>

### 1.12.5_1537（2019 年 2 月 5 日發行）的變更日誌
{: #1125_1537}

下表顯示修補程式 1.12.5_1537 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.4_1535 版以來進行的變更">
<caption>自 1.12.4_1535 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>3.3.1 版</td>
<td>v3.3.11</td>
<td>請參閱 [etcd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coreos/etcd/releases/v3.3.11)。此外，etcd 的受支援密碼組合現在限制為具有高強度加密（128 位元或以上）的子集。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>已更新 [CVE-2019-3462 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 及 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.12.4-118 版</td>
<td>1.12.5-137 版</td>
<td>已更新為支援 Kubernetes 1.12.5 版。此外，`calicoctl` 版本已更新為 3.3.1。已修正 2.0 版負載平衡器對於工作者節點狀態變更所做的不必要配置更新。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>338</td>
<td>342</td>
<td>檔案儲存空間外掛程式的更新如下：
<ul><li>支援使用[磁區拓蹼感知排程](/docs/containers?topic=containers-file_storage#file-topology)來進行動態佈建。</li>
<li>如果儲存空間已刪除，則忽略持續性磁區要求 (PVC) 刪除錯誤。</li>
<li>新增失敗訊息註釋至失敗的 PVC。</li>
<li>最佳化儲存空間佈建程式控制器的主導者選擇和重新同步期間設定，並將佈建逾時值從 30 分鐘增加到 1 小時。</li>
<li>開始佈建之前檢查使用者許可權。</li></ul></td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>111</td>
<td>122</td>
<td>已新增重試邏輯，以防止在 Kubernetes 密碼由 {{site.data.keyword.keymanagementservicefull_notm}} 進行管理時發生暫時失敗。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.12.4 版</td>
<td>1.12.5 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.5)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>Kubernetes API 伺服器審核原則配置已更新為包括記載 `cluster-admin` 要求的 meta 資料，以及記載工作負載 `create`、`update` 及 `patch` 要求的要求內文。</td>
</tr>
<tr>
<td>OpenVPN 用戶端</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>已更新 [CVE-2018-0734 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 及 [CVE-2018-5407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) 的映像檔。此外，現在會從密碼（而非從 configmap）取得 Pod 配置。</td>
</tr>
<tr>
<td>OpenVPN 伺服器</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>已更新 [CVE-2018-0734 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 及 [CVE-2018-5407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) 的映像檔。</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864) 的安全修補程式。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.12.4_1535（2019 年 1 月 28 日發行）的變更日誌
{: #1124_1535}

下表顯示工作者節點修正套件 1.12.4_1535 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.4_1534 版以來進行的變更">
<caption>自 1.12.4_1534 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>更新為已安裝的 Ubuntu 套件，包括 [CVE-2019-3462 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 及 [USN-3863-1 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://usn.ubuntu.com/3863-1) 的 `apt`。</td>
</tr>
</tbody>
</table>


### 1.12.4_1534（2019 年 1 月 21 日發行）的變更日誌
{: #1124_1534}

下表顯示修補程式 1.12.3_1534 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.3_1533 版以來進行的變更">
<caption>自 1.12.3_1533 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.12.3-91 版</td>
<td>1.12.4-118 版</td>
<td>已更新為支援 Kubernetes 1.12.4 版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.12.3 版</td>
<td>1.12.4 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.4)。</td>
</tr>
<tr>
<td>Kubernetes Add-on Resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>請參閱 [Kubernetes Add-on Resizer 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4)。</td>
</tr>
<tr>
<td>Kubernetes 儀表板</td>
<td>1.8.3 版</td>
<td>1.10.1 版</td>
<td>請參閱 [Kubernetes 儀表板版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1)。此更新解決 [CVE-2018-18264 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264)。</td>
</tr>
<tr>
<td>GPU 安裝程式</td>
<td>390.12</td>
<td>410.79</td>
<td>已將已安裝的 GPU 驅動程式更新為 410.79。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.12.3_1533（2019 年 1 月 7 日發行）的變更日誌
{: #1123_1533}

下表顯示工作者節點修正套件 1.12.3_1533 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.3_1532 版以來進行的變更">
<caption>自 1.12.3_1532 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2017-5753、CVE-2018-18690 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog) 的核心更新。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.12.3_1532（2018 年 12 月 17 日發行）的變更日誌
{: #1123_1532}

下表顯示工作者節點修正套件 1.12.2_1532 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.3_1531 版以來進行的變更">
<caption>自 1.12.3_1531 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>已安裝 Ubuntu 套件的更新。</td>
</tr>
</tbody>
</table>


### 1.12.3_1531（2018 年 12 月 5 日發行）的變更日誌
{: #1123_1531}

下表顯示修補程式 1.12.3_1531 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.2_1530 版以來進行的變更">
<caption>自 1.12.2_1530 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.12.2-68 版</td>
<td>1.12.3-91 版</td>
<td>已更新為支援 Kubernetes 1.12.3 版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.12.2 版</td>
<td>1.12.3 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.3)。此更新解決了 [CVE-2018-1002105 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/issues/71411)。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.12.2_1530（2018 年 12 月 4 日發行）的變更日誌
{: #1122_1530}

下表顯示工作者節點修正套件 1.12.2_1530 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.2_1529 版以來進行的變更">
<caption>自 1.12.2_1529 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作者節點資源使用率</td>
<td>N/A</td>
<td>N/A</td>
<td>已新增 kukelet 和 containerd 的專用 cgroups，以防止這些元件耗盡資源。如需相關資訊，請參閱[工作者節點資源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>



### 1.12.2_1529（2018 年 11 月 27 日發行）的變更日誌
{: #1122_1529}

下表顯示修補程式 1.12.2_1529 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.2_1528 版以來進行的變更">
<caption>自 1.12.2_1528 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>3.2.1 版</td>
<td>3.3.1 版</td>
<td>請參閱 [Calico 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.3/releases/#v331)。此更新解決了 [Tigera Technical Advisory TTA-2018-001 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.projectcalico.org/security-bulletins/)。</td>
</tr>
<tr>
<td>叢集 DNS 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已修正下列錯誤：導致 Kubernetes DNS 及 CoreDNS Pod 兩者在叢集建立或更新作業之後執行。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.0 版</td>
<td>1.1.5 版</td>
<td>請參閱 [containerd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.1.5)。已更新 containerd 來修正可能[阻止 Pod 終止 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/issues/2744) 的死鎖。</td>
</tr>
<tr>
<td>OpenVPN 用戶端及伺服器</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>已更新 [CVE-2018-0732 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) 及 [CVE-2018-0737 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737) 的映像檔。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.12.2_1528（2018 年 11 月 19 日發行）的變更日誌
{: #1122_1528}

下表顯示工作者節點修正套件 1.12.2_1528 中所包括的變更。
{: shortdesc}

<table summary="自 1.12.2_1527 版以來進行的變更">
<caption>自 1.12.2_1527 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-7755 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) 的核心更新。</td>
</tr>
</tbody>
</table>


### 1.12.2_1527（2018 年 11 月 7 日發行）的變更日誌
{: #1122_1527}

下表顯示修補程式 1.12.2_1527 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.3_1533 版以來進行的變更">
<caption>自 1.11.3_1533 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>`kube-system` 名稱空間中的 Calico `calico-*` Pod 現在會針對所有容器設定 CPU 及記憶體資源要求。</td>
</tr>
<tr>
<td>叢集 DNS 提供者</td>
<td>N/A</td>
<td>N/A</td>
<td>Kubernetes DNS (KubeDNS) 仍是預設叢集 DNS 提供者。不過，您現在可以選擇[將叢集 DNS 提供者變更為 CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set)。</td>
</tr>
<tr>
<td>叢集度量值提供者</td>
<td>N/A</td>
<td>N/A</td>
<td>Kubernetes Metrics Server 取代 Kubernetes Heapster（自 Kubernetes 1.8 版後已淘汰）作為叢集度量值提供者。如需動作項目，請參閱 [`metrics-server` 準備動作](/docs/containers?topic=containers-cs_versions#metrics-server)。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.4</td>
<td>1.2.0</td>
<td>請參閱 [containerd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.2.0)。</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>N/A</td>
<td>1.2.2</td>
<td>請參閱 [CoreDNS 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coredns/coredns/releases/tag/v1.2.2)。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.11.3 版</td>
<td>1.12.2 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.2)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已新增三個新的 IBM Pod 安全原則及其關聯的叢集角色。如需相關資訊，請參閱[瞭解 IBM 叢集管理的預設資源](/docs/containers?topic=containers-psp#ibm_psp)。</td>
</tr>
<tr>
<td>Kubernetes 儀表板</td>
<td>1.8.3 版</td>
<td>1.10.0 版</td>
<td>請參閱 [Kubernetes 儀表板版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.0)。<br><br>
如果您透過 `kubectl proxy` 存取儀表板，則會移除登入頁面上的**跳過**按鈕。請改為[使用**記號**來登入](/docs/containers?topic=containers-app#cli_dashboard)。此外，您現在可以執行 `kubectl -n kube-system scale deploy kubernetes-dashboard --replicas=3`，來擴增「Kubernetes 儀表板」Pod 的數目。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>請參閱 [Kubernetes DNS 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/dns/releases/tag/1.14.13)。</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>N/A</td>
<td>0.3.1 版</td>
<td>請參閱 [Kubernetes Metrics Server 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.1)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.11.3-118 版</td>
<td>1.12.2-68 版</td>
<td>已更新為支援 Kubernetes 1.12 版。其他變更包括下列各項：
<ul><li>負載平衡器 Pod（`ibm-system` 名稱空間中的 `ibm-cloud-provider-ip-*`）現在會設定 CPU 及記憶體資源要求。</li>
<li>已新增 `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` 註釋，來指定負載平衡器服務要部署至其中的 VLAN。若要查看叢集裡的可用 VLAN，請執行 `ibmcloud ks vlans --zone <zone>`。</li>
<li>新的[負載平衡器 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) 以測試版形式提供。</li></ul></td>
</tr>
<tr>
<td>OpenVPN 用戶端配置</td>
<td>N/A</td>
<td>N/A</td>
<td>`kin-system` 名稱空間中的 OpenVPN 用戶端 `vpn-* pod` 現在會設定 CPU 及記憶體資源要求。</td>
</tr>
</tbody>
</table>

## 已淘汰：1.11 版變更日誌
{: #111_changelog}

檢閱 1.11 版變更日誌。
{: shortdesc}

Kubernetes 1.11 版已淘汰，自 2019 年 6 月 27 日（暫訂）起不再支援。針對每個 Kubernetes 版本更新，[檢閱潛在影響](/docs/containers?topic=containers-cs_versions#cs_versions)，然後立即[更新您的叢集](/docs/containers?topic=containers-update#update)為至少 1.12。
{: deprecated}

### 2019 年 6 月 4 日發佈的 1.11.10_1561 的變更日誌
{: #11110_1561}

下表顯示修補程式 1.11.10_1561 中包含的變更。
{: shortdesc}

<table summary="自 1.11.10_1559 版以來進行的變更">
<caption>自 1.11.10_1559 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點 HA 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已更新配置，將主節點更新期間的間歇性主節點網路連線功能失敗減到最少。</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>請參閱 [etcd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coreos/etcd/releases/v3.3.13)。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>針對 [CVE-2018-10844 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，已更新映像。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>針對 [CVE-2018-10844 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，已更新映像。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 20 日發佈的工作者節點 FP1.11.10_1559 的變更日誌
{: #11110_1559}

下表顯示修補程式套件 1.11.10_1559 中包含的變更。
{: shortdesc}

<table summary="自 1.11.10_1558 版以來進行的變更">
<caption>自 1.11.10_1558 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 核心</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>下列各項的更新工作者節點映像檔及核心更新：[CVE-2018-12126 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 及 [CVE-2018-12130 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)。</td>
</tr>
<tr>
<td>Ubuntu 18.04 核心</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>下列各項的更新工作者節點映像檔及核心更新：[CVE-2018-12126 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) 及 [CVE-2018-12130 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html)。</td>
</tr>
</tbody>
</table>

### 2019 年 5 月 13 日發佈的 1.11.10_1558 的變更日誌
{: #11110_1558}

下表顯示修補程式 1.11.10_1558 中包含的變更。
{: shortdesc}

<table summary="自 1.11.9_1556 版以來進行的變更">
<caption>自 1.11.9_1556 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點 HA Proxy</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>請參閱 [HAProxy 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。此更新解決 [CVE-2019-6706 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>已更新 [CVE-2019-1543 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 的映像檔。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.11.9-241 版</td>
<td>1.11.10-270 版</td>
<td>已更新為支援 Kubernetes 1.11.10 版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.11.9 版</td>
<td>1.11.10 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.10)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>Kubernetes API 伺服器審核原則配置已更新為不記載 `/openapi/v2*` 唯讀 URL。此外，Kubernetes 控制器管理程式配置將已簽署之 `kubelet` 憑證的有效期間從 1 年增加到 3 年。</td>
</tr>
<tr>
<td>OpenVPN 用戶端配置</td>
<td>N/A</td>
<td>N/A</td>
<td>`kube-system` 名稱空間中的 OpenVPN 用戶端 `vpn-*` Pod 現在會將 `dnsPolicy` 設為 `Default` 以避免 Pod 在叢集 DNS 關閉時失敗。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>已更新 [CVE-2016-7076 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076)、[CVE-2017-1000368 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) 及 [CVE-2019-11068 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068) 的映像檔。</td>
</tr>
</tbody>
</table>

### 2019 年 4 月 29 日發佈的工作者節點 FP1.11.9_1556 的變更日誌
{: #1119_1556}

下表顯示工作者節點 FP1.11.9_1556 中包含的變更。
{: shortdesc}

<table summary="自 1.11.9_1555 版以來進行的變更">
<caption>自 1.11.9_1555 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>已安裝 Ubuntu 套件的更新。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>請參閱 [containerd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.1.7)。</td>
</tr>
</tbody>
</table>


### 工作者節點修正套件 1.11.9_1555（2019 年 4 月 15 日發行）的變更日誌
{: #1119_1555}

下表顯示工作者節點修正套件 1.11.9_1555 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.9_1554 版以來進行的變更">
<caption>自 1.11.9_1554 以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>更新為已安裝的 Ubuntu 套件，包括 [CVE-2019-3842 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html) 的 `systemd`。</td>
</tr>
</tbody>
</table>

### 1.11.9_1554（2019 年 4 月 8 日發行）的變更日誌
{: #1119_1554}

下表顯示修補程式 1.11.9_1554 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.8_1553 版以來進行的變更">
<caption>自 1.11.8_1553 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>3.3.1 版</td>
<td>3.3.6 版</td>
<td>請參閱 [Calico 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.3/releases/#v336)。此更新解決 [CVE-2019-9946 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946)。</td>
</tr>
<tr>
<td>叢集主節點 HA Proxy</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>請參閱 [HAProxy 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。此更新解決 [CVE-2018-0732 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732)、[CVE-2018-0734 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734)、[CVE-2018-0737 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)、[CVE-2018-5407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)、[CVE-2019-1543 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 及 [CVE-2019-1559 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.11.8-219 版</td>
<td>1.11.9-241 版</td>
<td>已更新為支援 Kubernetes 1.11.9 版及 Calico 3.3.6 版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.11.8 版</td>
<td>1.11.9 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.9)。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>請參閱 [Kubernetes DNS 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/dns/releases/tag/1.14.13)。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>已更新 [CVE-2017-12447 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447) 的映像檔。</td>
</tr>
<tr>
<td>Ubuntu 16.04 核心</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2019-9213 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的核心更新。</td>
</tr>
<tr>
<td>Ubuntu 18.04 核心</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2019-9213 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的核心更新。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.11.8_1553（2019 年 4 月 1 日發行）的變更日誌
{: #1118_1553}

下表顯示工作者節點修正套件 1.11.8_1553 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.8_1552 版以來進行的變更">
<caption>自 1.11.8_1552 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作者節點資源使用率</td>
<td>N/A</td>
<td>N/A</td>
<td>增加 kubelet 及 containerd 的記憶體保留，以防止這些元件耗盡資源。如需相關資訊，請參閱[工作者節點資源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>

### 主節點修正套件 1.11.8_1552（2019 年 3 月 26 日發行）的變更日誌
{: #1118_1552}

下表顯示主節點修正套件 1.11.8_1552 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.8_1550 版以來進行的變更">
<caption>自 1.11.8_1550 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>345</td>
<td>346</td>
<td>已更新 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的映像檔。</td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>166</td>
<td>167</td>
<td>修正用來管理 Kubernetes 密碼的間歇性 `context deadline exceeded` 及 `timeout` 錯誤。此外，還會修正可能未加密現有 Kubernetes 密碼的金鑰管理服務更新。此更新包括 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的修正程式。</td>
</tr>
<tr>
<td>適用於 {{site.data.keyword.Bluemix_notm}} 提供者的負載平衡器及負載平衡器監視器</td>
<td>143</td>
<td>146</td>
<td>已更新 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的映像檔。</td>
</tr>
</tbody>
</table>

### 1.11.8_1550（2019 年 3 月 20 日發行）的變更日誌
{: #1118_1550}

下表顯示修補程式 1.11.8_1550 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.8_1547 版以來進行的變更">
<caption>自 1.11.8_1547 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點 HA Proxy 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已更新配置，以更妥善地處理連至叢集主節點時的間歇性連線失敗。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>已將 GPU 驅動程式更新為 [418.43 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.nvidia.com/object/unix.html)。此更新包括 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html) 的修正程式。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>344</td>
<td>345</td>
<td>已新增對[專用服務端點](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)的支援。</td>
</tr>
<tr>
<td>核心</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2019-6133 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) 的核心更新。</td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>136</td>
<td>166</td>
<td>已更新 [CVE-2018-16890 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890)、[CVE-2019-3822 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) 及 [CVE-2019-3823 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823) 的映像檔。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>已更新 [CVE-2018-10779 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779)、[CVE-2018-12900 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900)、[CVE-2018-17000 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000)、[CVE-2018-19210 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210)、[CVE-2019-6128 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128)、[CVE-2019-7663 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663) 的映像檔。</td>
</tr>
</tbody>
</table>

### 1.11.8_1547（2019 年 3 月 4 日發行）的變更日誌
{: #1118_1547}

下表顯示修補程式 1.11.8_1547 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.7_1546 版以來進行的變更">
<caption>自 1.11.7_1546 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>已更新 [CVE-2019-6454 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) 的映像檔。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.11.7-198 版</td>
<td>1.11.8-219 版</td>
<td>已更新為支援 Kubernetes 1.11.8 版。已針對 `externalTrafficPolicy` 設為 `local` 的負載平衡器，修正定期連線問題。已將負載平衡器事件更新為使用最新的 {{site.data.keyword.Bluemix_notm}} 文件鏈結。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>342</td>
<td>344</td>
<td>已將映像檔的基本作業系統從 Fedora 變更為 Alpine。已更新 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>122</td>
<td>136</td>
<td>已將用戶端逾時增加至 {{site.data.keyword.keymanagementservicefull_notm}}。已更新 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.11.7 版</td>
<td>1.11.8 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.8)。此更新解決 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 及 [CVE-2019-1002100 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已新增 `ExperimentalCriticalPodAnnotation=true` 至 `--feature-gates` 選項。此設定可協助將 Pod 從已淘汰的 `scheduler.alpha.kubernetes.io/critical-pod` 註釋移轉至 [Kubernetes Pod 優先順序支援](/docs/containers?topic=containers-pod_priority#pod_priority)。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>N/A</td>
<td>N/A</td>
<td>已增加 Kubernetes DNS Pod 記憶體限制，從 `170Mi` 增加為 `400Mi`，以處理更多叢集服務。</td>
</tr>
<tr>
<td>適用於 {{site.data.keyword.Bluemix_notm}} 提供者的負載平衡器及負載平衡器監視器</td>
<td>132</td>
<td>143</td>
<td>已更新 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>OpenVPN 用戶端及伺服器</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>已更新 [CVE-2019-1559 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) 的映像檔。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>已更新 [CVE-2019-6454 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) 的映像檔。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.11.7_1546（2019 年 2 月 27 日發行）的變更日誌
{: #1117_1546}

下表顯示工作者節點修正套件 1.11.7_1546 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.7_1546 版以來進行的變更">
<caption>自 1.11.7_1546 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-19407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) 的核心更新。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.11.7_1544（2019 年 2 月 15 日發行）的變更日誌
{: #1117_1544}

下表顯示工作者節點修正套件 1.11.7_1544 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.7_1543 版以來進行的變更">
<caption>自 1.11.7_1543 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點 HA Proxy 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已將 Pod 配置 `spec.priorityClassName` 值變更為 `system-node-critical`，並將 `spec.priority` 值設為 `2000001000`。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>請參閱 [containerd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.1.6)。此更新解決 [CVE-2019-5736 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736)。</td>
</tr>
<tr>
<td>Kubernetes `kubelet` 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已啟用 `ExperimentalCriticalPodAnnotation` 特性閘道，以防止重要的靜態 Pod 收回。</td>
</tr>
</tbody>
</table>

### 1.11.7_1543（2019 年 2 月 5 日發行）的變更日誌
{: #1117_1543}

下表顯示修補程式 1.11.7_1543 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.6_1541 版以來進行的變更">
<caption>自 1.11.6_1541 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>3.3.1 版</td>
<td>v3.3.11</td>
<td>請參閱 [etcd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coreos/etcd/releases/v3.3.11)。此外，etcd 的受支援密碼組合現在限制為具有高強度加密（128 位元或以上）的子集。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>已更新 [CVE-2019-3462 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 及 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.11.6-180 版</td>
<td>1.11.7-198 版</td>
<td>已更新為支援 Kubernetes 1.11.7 版。此外，`calicoctl` 版本已更新為 3.3.1。已修正 2.0 版負載平衡器對於工作者節點狀態變更所做的不必要配置更新。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>338</td>
<td>342</td>
<td>檔案儲存空間外掛程式的更新如下：
<ul><li>支援使用[磁區拓蹼感知排程](/docs/containers?topic=containers-file_storage#file-topology)來進行動態佈建。</li>
<li>如果儲存空間已刪除，則忽略持續性磁區要求 (PVC) 刪除錯誤。</li>
<li>新增失敗訊息註釋至失敗的 PVC。</li>
<li>最佳化儲存空間佈建程式控制器的主導者選擇和重新同步期間設定，並將佈建逾時值從 30 分鐘增加到 1 小時。</li>
<li>開始佈建之前檢查使用者許可權。</li></ul></td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>111</td>
<td>122</td>
<td>已新增重試邏輯，以防止在 Kubernetes 密碼由 {{site.data.keyword.keymanagementservicefull_notm}} 進行管理時發生暫時失敗。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.11.6 版</td>
<td>1.11.7 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.7)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>Kubernetes API 伺服器審核原則配置已更新為包括記載 `cluster-admin` 要求的 meta 資料，以及記載工作負載 `create`、`update` 及 `patch` 要求的要求內文。</td>
</tr>
<tr>
<td>OpenVPN 用戶端</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>已更新 [CVE-2018-0734 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 及 [CVE-2018-5407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) 的映像檔。此外，現在會從密碼（而非從 configmap）取得 Pod 配置。</td>
</tr>
<tr>
<td>OpenVPN 伺服器</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>已更新 [CVE-2018-0734 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 及 [CVE-2018-5407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) 的映像檔。</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864) 的安全修補程式。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.11.6_1541（2019 年 1 月 28 日發行）的變更日誌
{: #1116_1541}

下表顯示工作者節點修正套件 1.11.6_1541 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.6_1540 版以來進行的變更">
<caption>自 1.11.6_1540 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>更新為已安裝的 Ubuntu 套件，包括 [CVE-2019-3462 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) / [USN-3863-1 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://usn.ubuntu.com/3863-1) 的 `apt`。</td>
</tr>
</tbody>
</table>

### 1.11.6_1540（2019 年 1 月 21 日發行）的變更日誌
{: #1116_1540}

下表顯示修補程式 1.11.6_1540 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.5_1539 版以來進行的變更">
<caption>自 1.11.5_1539 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.11.5-152 版</td>
<td>1.11.6-180 版</td>
<td>已更新為支援 Kubernetes 1.11.6 版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.11.5 版</td>
<td>1.11.6 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.6)。</td>
</tr>
<tr>
<td>Kubernetes Add-on Resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>請參閱 [Kubernetes Add-on Resizer 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4)。</td>
</tr>
<tr>
<td>Kubernetes 儀表板</td>
<td>1.8.3 版</td>
<td>1.10.1 版</td>
<td>請參閱 [Kubernetes 儀表板版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1)。此更新解決 [CVE-2018-18264 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264)。<br><br>如果您透過 `kubectl proxy` 存取儀表板，則會移除登入頁面上的**跳過**按鈕。請改為[使用**記號**來登入](/docs/containers?topic=containers-app#cli_dashboard)。</td>
</tr>
<tr>
<td>GPU 安裝程式</td>
<td>390.12</td>
<td>410.79</td>
<td>已將已安裝的 GPU 驅動程式更新為 410.79。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.11.5_1539（2019 年 1 月 7 日發行）的變更日誌
{: #1115_1539}

下表顯示工作者節點修正套件 1.11.5_1539 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.5_1538 版以來進行的變更">
<caption>自 1.11.5_1538 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2017-5753、CVE-2018-18690 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog) 的核心更新。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.11.5_1538（2018 年 12 月 17 日發行）的變更日誌
{: #1115_1538}

下表顯示工作者節點修正套件 1.11.5_1538 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.5_1537 版以來進行的變更">
<caption>自 1.11.5_1537 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>已安裝 Ubuntu 套件的更新。</td>
</tr>
</tbody>
</table>

### 1.11.5_1537（2018 年 12 月 5 日發行）的變更日誌
{: #1115_1537}

下表顯示修補程式 1.11.5_1537 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.4_1536 版以來進行的變更">
<caption>自 1.11.4_1536 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.11.4-142 版</td>
<td>1.11.5-152 版</td>
<td>已更新為支援 Kubernetes 1.11.5 版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.11.4 版</td>
<td>1.11.5 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.5)。此更新解決了 [CVE-2018-1002105 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/issues/71411)。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.11.4_1536（2018 年 12 月 4 日發行）的變更日誌
{: #1114_1536}

下表顯示工作者節點修正套件 1.11.4_1536 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.4_1535 版以來進行的變更">
<caption>自 1.11.4_1535 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作者節點資源使用率</td>
<td>N/A</td>
<td>N/A</td>
<td>已新增 kukelet 和 containerd 的專用 cgroups，以防止這些元件耗盡資源。如需相關資訊，請參閱[工作者節點資源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>

### 1.11.4_1535（2018 年 11 月 27 日發行）的變更日誌
{: #1114_1535}

下表顯示修補程式 1.11.4_1535 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.3_1534 版以來進行的變更">
<caption>自 1.11.3_1534 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>3.2.1 版</td>
<td>3.3.1 版</td>
<td>請參閱 [Calico 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.3/releases/#v331)。此更新解決了 [Tigera Technical Advisory TTA-2018-001 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.projectcalico.org/security-bulletins/)。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.4 版</td>
<td>1.1.5 版</td>
<td>請參閱 [containerd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.1.5)。已更新 containerd 來修正可能[阻止 Pod 終止 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/issues/2744) 的死鎖。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.11.3-127 版</td>
<td>1.11.4-142 版</td>
<td>已更新為支援 Kubernetes 1.11.4 版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.11.3 版</td>
<td>1.11.4 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.4)。</td>
</tr>
<tr>
<td>OpenVPN 用戶端及伺服器</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>已更新 [CVE-2018-0732 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) 及 [CVE-2018-0737 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737) 的映像檔。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.11.3_1534（2018 年 11 月 19 日發行）的變更日誌
{: #1113_1534}

下表顯示工作者節點修正套件 1.11.3_1534 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.3_1533 版以來進行的變更">
<caption>自 1.11.3_1533 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-7755 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) 的核心更新。</td>
</tr>
</tbody>
</table>


### 1.11.3_1533（2018 年 11 月 7 日發行）的變更日誌
{: #1113_1533}

下表顯示修補程式 1.11.3_1533 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.3_1531 版以來進行的變更">
<caption>自 1.11.3_1531 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點 HA 更新</td>
<td>N/A</td>
<td>N/A</td>
<td>已修正叢集之高可用性 (HA) 主節點的更新，這些叢集會使用許可 Webhook，例如 `initializerconfigurations`、`mutatingwebhookconfigurations` 或 `validatingwebhookconfigurations`。例如，如需[容器映像檔安全強制執行](/docs/services/Registry?topic=registry-security_enforce#security_enforce)，您可能使用這些 Webhook 與 Helm 圖表搭配。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.11.3-100 版</td>
<td>1.11.3-127 版</td>
<td>已新增 `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` 註釋，來指定負載平衡器服務要部署至其中的 VLAN。若要查看叢集裡的可用 VLAN，請執行 `ibmcloud ks vlans --zone <zone>`。</td>
</tr>
<tr>
<td>已啟用 TPM 的核心</td>
<td>N/A</td>
<td>N/A</td>
<td>搭配 TPM 晶片進行「信任運算」的裸機工作者節點會使用預設 Ubuntu 核心，直到啟用信任為止。如果您在現有叢集上[啟用信任](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)，則需要[重新載入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)任何具有 TPM 晶片的現有裸機工作者節點。若要檢查裸機工作者節點是否具有 TPM 晶片，請在執行 `ibmcloud ks machine-types --zone` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)之後，檢閱**可信任**欄位。</td>
</tr>
</tbody>
</table>

### 主節點修正套件 1.11.3_1531（2018 年 11 月 1 日發行）的變更日誌
{: #1113_1531_ha-master}

下表顯示主節點修正套件 1.11.3_1531 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.3_1527 版以來進行的變更">
<caption>自 1.11.3_1527 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點</td>
<td>N/A</td>
<td>N/A</td>
<td>已更新叢集主節點配置來增加高可用性 (HA)。叢集現在具有三個使用高可用性 (HA) 配置所設定的 Kubernetes 主節點抄本，每一個主節點會部署在個別的實體主機上。此外，如果您的叢集是在具有多區域功能的區域中，則主節點會分散在各區域之中。<br>如需您必須採取的動作，請參閱[更新為高可用性叢集主節點](/docs/containers?topic=containers-cs_versions#ha-masters)。這些準備動作適用下列情況：<ul>
<li>如果您具有防火牆或自訂 Calico 網路原則。</li>
<li>如果您是在工作者節點上使用主機埠 `2040` 或 `2041`。</li>
<li>如果您已使用叢集主節點 IP 位址，對主節點進行叢集內存取。</li>
<li>如果您具有呼叫 Calico API 或 CLI (`calictl`) 的自動化，例如建立 Calico 原則。</li>
<li>如果您使用 Kubernetes 或 Calico 網路原則，來控制對主節點的 Pod Egress 存取。</li></ul></td>
</tr>
<tr>
<td>叢集主節點 HA Proxy</td>
<td>N/A</td>
<td>1.8.12-alpine</td>
<td>已在所有工作者節點上新增用戶端負載平衡的 `ibm-master-proxy-*` Pod，所以每一個工作者節點用戶端都可以將要求遞送至可用的 HA 主節點抄本。</td>
</tr>
<tr>
<td>etcd</td>
<td>3.2.18 版</td>
<td>3.3.1 版</td>
<td>請參閱 [etcd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coreos/etcd/releases/v3.3.1)。</td>
</tr>
<tr>
<td>加密 etcd 中的資料</td>
<td>N/A</td>
<td>N/A</td>
<td>先前，etcd 資料是儲存在主節點的 NFS 檔案儲存空間實例上，而此實例是在靜止時加密。現在，etcd 資料是儲存在主節點的本端磁碟上，並備份至 {{site.data.keyword.cos_full_notm}}。資料是在傳送至 {{site.data.keyword.cos_full_notm}} 期間和靜止時加密。不過，主節點的本端磁碟上的 etcd 資料不會加密。如果您想要將主節點的本端 etcd 資料加密，請[在您的叢集裡啟用 {{site.data.keyword.keymanagementservicelong_notm}}](/docs/containers?topic=containers-encryption#keyprotect)。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.11.3_1531（2018 年 10 月 26 日發行）的變更日誌
{: #1113_1531}

下表顯示工作者節點修正套件 1.11.3_1531 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.3_1525 版以來進行的變更">
<caption>自 1.11.3_1525 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>OS 岔斷處理</td>
<td>N/A</td>
<td>N/A</td>
<td>已將岔斷要求 (IRQ) 系統常駐程式取代為更高效能的岔斷處理程式。</td>
</tr>
</tbody>
</table>

### 主節點修正套件 1.11.3_1527（2018 年 10 月 15 日發行）的變更日誌
{: #1113_1527}

下表顯示主節點修正套件 1.11.3_1527 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.3_1524 版以來進行的變更">
<caption>自 1.11.3_1524 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已修正 `calico-node` 容器就緒探測，以便更妥善地處理節點失敗。</td>
</tr>
<tr>
<td>叢集更新</td>
<td>N/A</td>
<td>N/A</td>
<td>已修正從不受支援的版本更新主節點時的叢集附加程式更新問題。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.11.3_1525（2018 年 10 月 10 日發行）的變更日誌
{: #1113_1525}

下表顯示工作者節點修正套件 1.11.3_1525 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.3_1524 版以來進行的變更">
<caption>自 1.11.3_1524 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-14633、CVE-2018-17182 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog) 的核心更新。</td>
</tr>
<tr>
<td>非作用中階段作業逾時</td>
<td>N/A</td>
<td>N/A</td>
<td>基於法規遵循的原因，將非作用中階段作業逾時設為 5 分鐘。</td>
</tr>
</tbody>
</table>


### 1.11.3_1524（2018 年 10 月 2 日發行）的變更日誌
{: #1113_1524}

下表顯示修補程式 1.11.3_1524 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.3_1521 版以來進行的變更">
<caption>自 1.11.3_1521 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>1.1.3</td>
<td>1.1.4</td>
<td>請參閱 [containerd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.1.4)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.11.3-91 版</td>
<td>1.11.3-100 版</td>
<td>已更新負載平衡器錯誤訊息中的文件鏈結。</td>
</tr>
<tr>
<td>IBM 檔案儲存空間類別</td>
<td>N/A</td>
<td>N/A</td>
<td>已移除 IBM 檔案儲存空間類別中的重複 `reclaimPolicy` 參數。<br><br>
此外，現在，當您更新叢集主節點時，預設 IBM 檔案儲存空間類別會維持不變。如果您要變更預設儲存空間類別，請執行 `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`，並將 `<storageclass>` 取代為儲存空間類別的名稱。</td>
</tr>
</tbody>
</table>

### 1.11.3_1521（2018 年 9 月 20 日發行）的變更日誌
{: #1113_1521}

下表顯示修補程式 1.11.3_1521 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.2_1516 版以來進行的變更">
<caption>自 1.11.2_1516 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.11.2-71 版</td>
<td>1.11.3-91 版</td>
<td>已更新為支援 Kubernetes 1.11.3 版。</td>
</tr>
<tr>
<td>IBM 檔案儲存空間類別</td>
<td>N/A</td>
<td>N/A</td>
<td>已移除 IBM 檔案儲存空間類別中的 `mountOptions`，以使用工作者節點所提供的預設值。<br><br>
此外，現在，當您更新叢集主節點時，預設 IBM 檔案儲存空間類別會維持 `ibmc-file-bronze`。如果您要變更預設儲存空間類別，請執行 `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`，並將 `<storageclass>` 取代為儲存空間類別的名稱。</td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>N/A</td>
<td>N/A</td>
<td>已新增在叢集裡使用 Kubernetes 金鑰管理服務 (KMS) 提供者的能力，以支援 {{site.data.keyword.keymanagementservicefull}}。當您[在叢集裡啟用 {{site.data.keyword.keymanagementserviceshort}}](/docs/containers?topic=containers-encryption#keyprotect) 時，會加密您的所有 Kubernetes 密碼。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.11.2 版</td>
<td>1.11.3 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.3)。</td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>請參閱 [Kubernetes DNS Autoscaler 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0)。</td>
</tr>
<tr>
<td>日誌循環</td>
<td>N/A</td>
<td>N/A</td>
<td>已切換成使用 `systemd` 計時器，而非 `cronjobs`，以防止 `logrotate` 在未於 90 天內重新載入或更新的工作者節點上失敗。**附註**：在此次要版本的所有舊版中，Cron 工作失敗之後會填滿主要磁碟，因為日誌不會循環。在工作者節點作用 90 天但未進行更新或重新載入之後，Cron 工作就會失敗。如果日誌填滿整個主要磁碟，則工作者節點會進入失敗狀態。使用 `ibmcloud ks worker-reload` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)或 `ibmcloud ks worker-update` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)，即可修正工作者節點。</td>
</tr>
<tr>
<td>root 密碼有效期限</td>
<td>N/A</td>
<td>N/A</td>
<td>基於法規遵循的原因，工作者節點的 root 密碼會在 90 天後到期。如果您的自動化工具需要以 root 使用者身分登入工作者節點，或依賴以 root 使用者身分執行的 Cron 工作，則可以登入工作者節點並執行 `chage -M -1 root` 來停用密碼有效期限。**附註**：如果您的安全規範需求可避免以 root 使用者身分執行或移除密碼有效期限，則請不要停用有效期限。相反地，您可以至少每 90 天[更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)或[重新載入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)工作者節點一次。</td>
</tr>
<tr>
<td>工作者節點運行環境元件（`kubelet`、`kube-proxy`、`containerd`）</td>
<td>N/A</td>
<td>N/A</td>
<td>已移除主要磁碟上的運行環境元件的相依關係。主要磁碟滿載時，此加強功能可避免工作者節點失敗。</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>定期清除暫時性裝載裝置，以防止它們變成無界限。此動作解決 [Kubernetes 問題 57345 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/issues/57345)。</td>
</tr>
</tbody>
</table>

### 1.11.2_1516（2018 年 9 月 4 日發行）的變更日誌
{: #1112_1516}

下表顯示修補程式 1.11.2_1516 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.2_1514 版以來進行的變更">
<caption>自 1.11.2_1514 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>3.1.3 版</td>
<td>3.2.1 版</td>
<td>請參閱 [Calico 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.2/releases/#v321)。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.2</td>
<td>1.1.3</td>
<td>請參閱 [`containerd` 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.1.3)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.11.2-60 版</td>
<td>1.11.2-71 版</td>
<td>已變更雲端提供者配置，可更恰當地處理 `externalTrafficPolicy` 設為 `local` 之負載平衡器服務的更新。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已從 IBM 所提供檔案儲存空間類別的裝載選項移除預設 NFS 版本。主機的作業系統現在會與 IBM Cloud 基礎架構 (SoftLayer) NFS 伺服器協議 NFS 版本。若要手動設定特定的 NFS 版本，或變更主機作業系統所協議 PV 的 NFS 版本，請參閱[變更預設 NFS 版本](/docs/containers?topic=containers-file_storage#nfs_version_class)。</td>
</tr>
</tbody>
</table>

### 工作者節點修正套件 1.11.2_1514（2018 年 8 月 23 日發行）的變更日誌
{: #1112_1514}

下表顯示工作者節點修正套件 1.11.2_1514 中所包括的變更。
{: shortdesc}

<table summary="自 1.11.2_1513 版以來進行的變更">
<caption>自 1.11.2_1513 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>已更新 `systemd` 來修正 `cgroup` 洩漏。</td>
</tr>
<tr>
<td>核心</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-3620、CVE-2018-3646 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://usn.ubuntu.com/3741-1/) 的核心更新。</td>
</tr>
</tbody>
</table>

### 1.11.2_1513（2018 年 8 月 14 日發行）的變更日誌
{: #1112_1513}

下表顯示修補程式 1.11.2_1513 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.5_1518 版以來進行的變更">
<caption>自 1.10.5_1518 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>N/A</td>
<td>1.1.2</td>
<td>`containerd` 會將 Docker 取代為 Kubernetes 的新容器運行環境。請參閱 [`containerd` 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/containerd/containerd/releases/tag/v1.1.2)。如需您必須採取的動作，請參閱[更新為 `containerd` 作為容器運行環境](/docs/containers?topic=containers-cs_versions#containerd)。</td>
</tr>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>`containerd` 會將 Docker 取代為 Kubernetes 的新容器運行環境，來加強效能。如需您必須採取的動作，請參閱[更新為 `containerd` 作為容器運行環境](/docs/containers?topic=containers-cs_versions#containerd)。</td>
</tr>
<tr>
<td>etcd</td>
<td>3.2.14 版</td>
<td>3.2.18 版</td>
<td>請參閱 [etcd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coreos/etcd/releases/v3.2.18)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.10.5-118 版</td>
<td>1.11.2-60 版</td>
<td>已更新為支援 Kubernetes 1.11 版。此外，負載平衡器 Pod 現在使用新的 `ibm-app-cluster-critical` Pod 優先順序類別。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>334</td>
<td>338</td>
<td>已將 `incubator` 版本更新為 1.8。檔案儲存空間會佈建給您選取的特定區域。除非您使用多區域叢集，並且需要[新增地區及區域標籤](/docs/containers?topic=containers-kube_concepts#storage_multizone)，否則無法更新現有（靜態）PV 實例標籤。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.10.5 版</td>
<td>1.11.2 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.2)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已更新叢集 Kubernetes API 伺服器的 OpenID Connect 配置，以支援 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 存取群組。已針對叢集的 Kubernetes API 伺服器將 `Priority` 新增至 `--enable-admission-plugins` 選項，並已將叢集配置為支援 Pod 優先順序。如需相關資訊，請參閱：<ul><li>[{{site.data.keyword.Bluemix_notm}} IAM 存取群組](/docs/containers?topic=containers-users#rbac)</li>
<li>[配置 Pod 優先順序](/docs/containers?topic=containers-pod_priority#pod_priority)</li></ul></td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>1.5.2 版</td>
<td>1.5.4 版</td>
<td>已增加 `heapster-nanny` 容器的資源限制。請參閱 [Kubernetes Heapster 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/heapster/releases/tag/v1.5.4)。</td>
</tr>
<tr>
<td>記載配置</td>
<td>N/A</td>
<td>N/A</td>
<td>容器日誌目錄現在是 `/var/log/pods/`，而非前一個 `/var/lib/docker/containers/`。</td>
</tr>
</tbody>
</table>

<br />


## 保存
{: #changelog_archive}

不受支援的 Kubernetes 版本：
*  [1.10 版](#110_changelog)
*  [1.9 版](#19_changelog)
*  [1.8 版](#18_changelog)
*  [1.7 版](#17_changelog)

### 1.10 版變更日誌（自 2019 年 5 月 16 日起不再支援）
{: #110_changelog}

檢閱 1.10 版變更日誌。
{: shortdesc}

*   [2019 年 5 月 13 日發佈的工作者節點 FP1.10.13_1558 的變更日誌](#11013_1558)
*   [2019 年 4 月 29 日發佈的工作者節點 FP1.10.13_1557 的變更日誌](#11013_1557)
*   [工作者節點修正套件 1.10.13_1556（2019 年 4 月 15 日發行）的變更日誌](#11013_1556)
*   [1.10.13_1555（2019 年 4 月 8 日發行）的變更日誌](#11013_1555)
*   [工作者節點修正套件 1.10.13_1554（2019 年 4 月 1 日發行）的變更日誌](#11013_1554)
*   [主節點修正套件 1.10.13_1553（2019 年 3 月 26 日發行）的變更日誌](#11118_1553)
*   [1.10.13_1551（2019 年 3 月 20 日發行）的變更日誌](#11013_1551)
*   [1.10.13_1548（2019 年 3 月 4 日發行）的變更日誌](#11013_1548)
*   [工作者節點修正套件 1.10.12_1546（2019 年 2 月 27 日發行）的變更日誌](#11012_1546)
*   [工作者節點修正套件 1.10.12_1544（2019 年 2 月 15 日發行）的變更日誌](#11012_1544)
*   [1.10.12_1543（2019 年 2 月 5 日發行）的變更日誌](#11012_1543)
*   [工作者節點修正套件 1.10.12_1541（2019 年 1 月 28 日發行）的變更日誌](#11012_1541)
*   [1.10.12_1540（2019 年 1 月 21 日發行）的變更日誌](#11012_1540)
*   [工作者節點修正套件 1.10.11_1538（2019 年 1 月 7 日發行）的變更日誌](#11011_1538)
*   [工作者節點修正套件 1.10.11_1537（2018 年 12 月 17 日發行）的變更日誌](#11011_1537)
*   [1.10.11_1536（2018 年 12 月 4 日發行）的變更日誌](#11011_1536)
*   [工作者節點修正套件 1.10.8_1532（2018 年 11 月 27 日發行）的變更日誌](#1108_1532)
*   [工作者節點修正套件 1.10.8_1531（2018 年 11 月 19 日發行）的變更日誌](#1108_1531)
*   [1.10.8_1530（2018 年 11 月 7 日發行）的變更日誌](#1108_1530_ha-master)
*   [工作者節點修正套件 1.10.8_1528（2018 年 10 月 26 日發行）的變更日誌](#1108_1528)
*   [工作者節點修正套件 1.10.8_1525（2018 年 10 月 10 日發行）的變更日誌](#1108_1525)
*   [1.10.8_1524（2018 年 10 月 2 日發行）的變更日誌](#1108_1524)
*   [工作者節點修正套件 1.10.7_1521（2018 年 9 月 20 日發行）的變更日誌](#1107_1521)
*   [1.10.7_1520（2018 年 9 月 4 日發行）的變更日誌](#1107_1520)
*   [工作者節點修正套件 1.10.5_1519（2018 年 8 月 23 日發行）的變更日誌](#1105_1519)
*   [工作者節點修正套件 1.10.5_1518（2018 年 8 月 13 日發行）的變更日誌](#1105_1518)
*   [1.10.5_1517（2018 年 7 月 27 日發行）的變更日誌](#1105_1517)
*   [工作者節點修正套件 1.10.3_1514（2018 年 7 月 3 日發行）的變更日誌](#1103_1514)
*   [工作者節點修正套件 1.10.3_1513（2018 年 6 月 21 日發行）的變更日誌](#1103_1513)
*   [1.10.3_1512（2018 年 6 月 12 日發行）的變更日誌](#1103_1512)
*   [工作者節點修正套件 1.10.1_1510（2018 年 5 月 18 日發行）的變更日誌](#1101_1510)
*   [工作者節點修正套件 1.10.1_1509（2018 年 5 月 16 日發行）的變更日誌](#1101_1509)
*   [1.10.1_1508（2018 年 5 月 1 日發行）的變更日誌](#1101_1508)

#### 2019 年 5 月 13 日發佈的工作者節點 FP1.10.13_1558 的變更日誌
{: #11013_1558}

下表顯示工作者節點 FP1.10.13_1558 中包含的變更。
{: shortdesc}

<table summary="自 1.10.13_1557 版以來進行的變更">
<caption>自 1.10.13_1557 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點 HA Proxy</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>請參閱 [HAProxy 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。此更新解決 [CVE-2019-6706 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706)。</td>
</tr>
</tbody>
</table>

#### 2019 年 4 月 29 日發佈的工作者節點 FP1.10.13_1557 的變更日誌
{: #11013_1557}

下表顯示工作者節點 FP1.10.13_1557 中包含的變更。
{: shortdesc}

<table summary="自 1.10.13_1556 版以來進行的變更">
<caption>自 1.10.13_1556 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>已安裝 Ubuntu 套件的更新。</td>
</tr>
</tbody>
</table>


#### 工作者節點修正套件 1.10.13_1556（2019 年 4 月 15 日發行）的變更日誌
{: #11013_1556}

下表顯示工作者節點修正套件 1.10.13_1556 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.13_1555 版以來進行的變更">
<caption>自 1.10.13_1555 以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>更新為已安裝的 Ubuntu 套件，包括 [CVE-2019-3842 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html) 的 `systemd`。</td>
</tr>
</tbody>
</table>

#### 1.10.13_1555（2019 年 4 月 8 日發行）的變更日誌
{: #11013_1555}

下表顯示修補程式 1.10.13_1555 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.13_1554 版以來進行的變更">
<caption>自 1.10.13_1554 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點 HA Proxy</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>請參閱 [HAProxy 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.haproxy.org/download/1.9/src/CHANGELOG)。此更新解決 [CVE-2018-0732 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732)、[CVE-2018-0734 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734)、[CVE-2018-0737 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)、[CVE-2018-5407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)、[CVE-2019-1543 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) 及 [CVE-2019-1559 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559)。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>請參閱 [Kubernetes DNS 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/dns/releases/tag/1.14.13)。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>已更新 [CVE-2017-12447 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447) 的映像檔。</td>
</tr>
<tr>
<td>Ubuntu 16.04 核心</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2019-9213 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的核心更新。</td>
</tr>
<tr>
<td>Ubuntu 18.04 核心</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2019-9213 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) 的核心更新。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.13_1554（2019 年 4 月 1 日發行）的變更日誌
{: #11013_1554}

下表顯示工作者節點修正套件 1.10.13_1554 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.13_1553 版以來進行的變更">
<caption>自 1.10.13_1553 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作者節點資源使用率</td>
<td>N/A</td>
<td>N/A</td>
<td>增加 kubelet 及 containerd 的記憶體保留，以防止這些元件耗盡資源。如需相關資訊，請參閱[工作者節點資源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>


#### 主節點修正套件 1.10.13_1553（2019 年 3 月 26 日發行）的變更日誌
{: #11118_1553}

下表顯示主節點修正套件 1.10.13_1553 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.13_1551 版以來進行的變更">
<caption>自 1.10.13_1551 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>345</td>
<td>346</td>
<td>已更新 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的映像檔。</td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>166</td>
<td>167</td>
<td>修正用來管理 Kubernetes 密碼的間歇性 `context deadline exceeded` 及 `timeout` 錯誤。此外，還會修正可能未加密現有 Kubernetes 密碼的金鑰管理服務更新。此更新包括 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的修正程式。</td>
</tr>
<tr>
<td>適用於 {{site.data.keyword.Bluemix_notm}} 提供者的負載平衡器及負載平衡器監視器</td>
<td>143</td>
<td>146</td>
<td>已更新 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) 的映像檔。</td>
</tr>
</tbody>
</table>

#### 1.10.13_1551（2019 年 3 月 20 日發行）的變更日誌
{: #11013_1551}

下表顯示修補程式 1.10.13_1551 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.13_1548 版以來進行的變更">
<caption>自 1.10.13_1548 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點 HA Proxy 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已更新配置，以更妥善地處理連至叢集主節點時的間歇性連線失敗。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>已將 GPU 驅動程式更新為 [418.43 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.nvidia.com/object/unix.html)。此更新包括 [CVE-2019-9741 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html) 的修正程式。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>344</td>
<td>345</td>
<td>已新增對[專用服務端點](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)的支援。</td>
</tr>
<tr>
<td>核心</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2019-6133 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) 的核心更新。</td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>136</td>
<td>166</td>
<td>已更新 [CVE-2018-16890 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890)、[CVE-2019-3822 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) 及 [CVE-2019-3823 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823) 的映像檔。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>已更新 [CVE-2018-10779 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779)、[CVE-2018-12900 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900)、[CVE-2018-17000 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000)、[CVE-2018-19210 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210)、[CVE-2019-6128 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128)、[CVE-2019-7663 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663) 的映像檔。</td>
</tr>
</tbody>
</table>

#### 1.10.13_1548（2019 年 3 月 4 日發行）的變更日誌
{: #11013_1548}

下表顯示修補程式 1.10.13_1548 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.12_1546 版以來進行的變更">
<caption>自 1.10.12_1546 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>已更新 [CVE-2019-6454 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) 的映像檔。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.10.12-252 版</td>
<td>1.10.13-288 版</td>
<td>已更新為支援 Kubernetes 1.10.13 版。已針對 `externalTrafficPolicy` 設為 `local` 的負載平衡器，修正定期連線問題。已將負載平衡器事件更新為使用最新的 {{site.data.keyword.Bluemix_notm}} 文件鏈結。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>342</td>
<td>344</td>
<td>已將映像檔的基本作業系統從 Fedora 變更為 Alpine。已更新 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>122</td>
<td>136</td>
<td>已將用戶端逾時增加至 {{site.data.keyword.keymanagementservicefull_notm}}。已更新 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.10.12 版</td>
<td>1.10.13 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.13)。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>N/A</td>
<td>N/A</td>
<td>已增加 Kubernetes DNS Pod 記憶體限制，從 `170Mi` 增加為 `400Mi`，以處理更多叢集服務。</td>
</tr>
<tr>
<td>適用於 {{site.data.keyword.Bluemix_notm}} 提供者的負載平衡器及負載平衡器監視器</td>
<td>132</td>
<td>143</td>
<td>已更新 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>OpenVPN 用戶端及伺服器</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>已更新 [CVE-2019-1559 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) 的映像檔。</td>
</tr>
<tr>
<td>授信的運算代理程式</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>已更新 [CVE-2019-6454 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) 的映像檔。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.12_1546（2019 年 2 月 27 日發行）的變更日誌
{: #11012_1546}

下表顯示工作者節點修正套件 1.10.12_1546 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.12_1544 版以來進行的變更">
<caption>自 1.10.12_1544 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-19407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) 的核心更新。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.12_1544（2019 年 2 月 15 日發行）的變更日誌
{: #11012_1544}

下表顯示工作者節點修正套件 1.10.12_1544 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.12_1543 版以來進行的變更">
<caption>自 1.10.12_1543 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>18.06.1-ce</td>
<td>18.06.2-ce</td>
<td>請參閱 [Docker Community Edition 版本注意事項![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/docker/docker-ce/releases/tag/v18.06.2-ce)。此更新解決 [CVE-2019-5736 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736)。</td>
</tr>
<tr>
<td>Kubernetes `kubelet` 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已啟用 `ExperimentalCriticalPodAnnotation` 特性閘道，以防止重要的靜態 Pod 收回。</td>
</tr>
</tbody>
</table>

#### 1.10.12_1543（2019 年 2 月 5 日發行）的變更日誌
{: #11012_1543}

下表顯示修補程式 1.10.12_1543 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.12_1541 版以來進行的變更">
<caption>自 1.10.12_1541 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>3.3.1 版</td>
<td>v3.3.11</td>
<td>請參閱 [etcd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coreos/etcd/releases/v3.3.11)。此外，etcd 的受支援密碼組合現在限制為具有高強度加密（128 位元或以上）的子集。</td>
</tr>
<tr>
<td>GPU 裝置外掛程式及安裝程式</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>已更新 [CVE-2019-3462 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 及 [CVE-2019-6486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) 的映像檔。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>338</td>
<td>342</td>
<td>檔案儲存空間外掛程式的更新如下：
<ul><li>支援使用[磁區拓蹼感知排程](/docs/containers?topic=containers-file_storage#file-topology)來進行動態佈建。</li>
<li>如果儲存空間已刪除，則忽略持續性磁區要求 (PVC) 刪除錯誤。</li>
<li>新增失敗訊息註釋至失敗的 PVC。</li>
<li>最佳化儲存空間佈建程式控制器的主導者選擇和重新同步期間設定，並將佈建逾時值從 30 分鐘增加到 1 小時。</li>
<li>開始佈建之前檢查使用者許可權。</li></ul></td>
</tr>
<tr>
<td>金鑰管理服務提供者</td>
<td>111</td>
<td>122</td>
<td>已新增重試邏輯，以防止在 Kubernetes 密碼由 {{site.data.keyword.keymanagementservicefull_notm}} 進行管理時發生暫時失敗。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>Kubernetes API 伺服器審核原則配置已更新為包括記載 `cluster-admin` 要求的 meta 資料，以及記載工作負載 `create`、`update` 及 `patch` 要求的要求內文。</td>
</tr>
<tr>
<td>OpenVPN 用戶端</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>已更新 [CVE-2018-0734 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 及 [CVE-2018-5407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) 的映像檔。此外，現在會從密碼（而非從 configmap）取得 Pod 配置。</td>
</tr>
<tr>
<td>OpenVPN 伺服器</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>已更新 [CVE-2018-0734 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) 及 [CVE-2018-5407 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) 的映像檔。</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864) 的安全修補程式。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.12_1541（2019 年 1 月 28 日發行）的變更日誌
{: #11012_1541}

下表顯示工作者節點修正套件 1.10.12_1541 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.12_1540 版以來進行的變更">
<caption>自 1.10.12_1540 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>更新為已安裝的 Ubuntu 套件，包括 [CVE-2019-3462 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) 及 [USN-3863-1 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://usn.ubuntu.com/3863-1) 的 `apt`。</td>
</tr>
</tbody>
</table>

#### 1.10.12_1540（2019 年 1 月 21 日發行）的變更日誌
{: #11012_1540}

下表顯示修補程式 1.10.12_1540 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.11_1538 版以來進行的變更">
<caption>自 1.10.11_1538 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.10.11-219 版</td>
<td>1.10.12-252 版</td>
<td>已更新為支援 Kubernetes 1.10.12 版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.10.11 版</td>
<td>1.10.12 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.12)。</td>
</tr>
<tr>
<td>Kubernetes Add-on Resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>請參閱 [Kubernetes Add-on Resizer 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4)。</td>
</tr>
<tr>
<td>Kubernetes 儀表板</td>
<td>1.8.3 版</td>
<td>1.10.1 版</td>
<td>請參閱 [Kubernetes 儀表板版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1)。此更新解決 [CVE-2018-18264 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264)。<br><br>如果您透過 `kubectl proxy` 存取儀表板，則會移除登入頁面上的**跳過**按鈕。請改為[使用**記號**來登入](/docs/containers?topic=containers-app#cli_dashboard)。</td>
</tr>
<tr>
<td>GPU 安裝程式</td>
<td>390.12</td>
<td>410.79</td>
<td>已將已安裝的 GPU 驅動程式更新為 410.79。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.11_1538（2019 年 1 月 7 日發行）的變更日誌
{: #11011_1538}

下表顯示工作者節點修正套件 1.10.11_1538 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.11_1537 版以來進行的變更">
<caption>自 1.10.11_1537 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2017-5753、CVE-2018-18690 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog) 的核心更新。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.11_1537（2018 年 12 月 17 日發行）的變更日誌
{: #11011_1537}

下表顯示工作者節點修正套件 1.10.11_1537 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.11_1536 版以來進行的變更">
<caption>自 1.10.11_1536 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>已安裝 Ubuntu 套件的更新。</td>
</tr>
</tbody>
</table>


#### 1.10.11_1536（2018 年 12 月 4 日發行）的變更日誌
{: #11011_1536}

下表顯示修補程式 1.10.11_1536 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.8_1532 版以來進行的變更">
<caption>自 1.10.8_1532 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>3.2.1 版</td>
<td>3.3.1 版</td>
<td>請參閱 [Calico 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.3/releases/#v331)。此更新解決了 [Tigera Technical Advisory TTA-2018-001 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.projectcalico.org/security-bulletins/)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.10.8-197 版</td>
<td>1.10.11-219 版</td>
<td>已更新為支援 Kubernetes 1.10.11 版。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.10.8 版</td>
<td>1.10.11 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.11)。此更新解決了 [CVE-2018-1002105 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/issues/71411)。</td>
</tr>
<tr>
<td>OpenVPN 用戶端及伺服器</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>已更新 [CVE-2018-0732 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) 及 [CVE-2018-0737 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737) 的映像檔。</td>
</tr>
<tr>
<td>工作者節點資源使用率</td>
<td>N/A</td>
<td>N/A</td>
<td>已新增 kukelet 和 Docker 的專用 cgroups，以防止這些元件耗盡資源。如需相關資訊，請參閱[工作者節點資源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.8_1532（2018 年 11 月 27 日發行）的變更日誌
{: #1108_1532}

下表顯示工作者節點修正套件 1.10.8_1532 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.8_1531 版以來進行的變更">
<caption>自 1.10.8_1531 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>請參閱 [Docker 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.docker.com/engine/release-notes/#18061-ce)。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.8_1531（2018 年 11 月 19 日發行）的變更日誌
{: #1108_1531}

下表顯示工作者節點修正套件 1.10.8_1531 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.8_1530 版以來進行的變更">
<caption>自 1.10.8_1530 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-7755 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) 的核心更新。</td>
</tr>
</tbody>
</table>

#### 1.10.8_1530（2018 年 11 月 7 日發行）的變更日誌
{: #1108_1530_ha-master}

下表顯示修補程式 1.10.8_1530 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.8_1528 版以來進行的變更">
<caption>自 1.10.8_1528 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集主節點</td>
<td>N/A</td>
<td>N/A</td>
<td>已更新叢集主節點配置來增加高可用性 (HA)。叢集現在具有三個使用高可用性 (HA) 配置所設定的 Kubernetes 主節點抄本，每一個主節點會部署在個別的實體主機上。此外，如果您的叢集是在具有多區域功能的區域中，則主節點會分散在各區域之中。<br>如需您必須採取的動作，請參閱[更新為高可用性叢集主節點](/docs/containers?topic=containers-cs_versions#ha-masters)。這些準備動作適用下列情況：<ul>
<li>如果您具有防火牆或自訂 Calico 網路原則。</li>
<li>如果您是在工作者節點上使用主機埠 `2040` 或 `2041`。</li>
<li>如果您已使用叢集主節點 IP 位址，對主節點進行叢集內存取。</li>
<li>如果您具有呼叫 Calico API 或 CLI (`calictl`) 的自動化，例如建立 Calico 原則。</li>
<li>如果您使用 Kubernetes 或 Calico 網路原則，來控制對主節點的 Pod Egress 存取。</li></ul></td>
</tr>
<tr>
<td>叢集主節點 HA Proxy</td>
<td>N/A</td>
<td>1.8.12-alpine</td>
<td>已在所有工作者節點上新增用戶端負載平衡的 `ibm-master-proxy-*` Pod，所以每一個工作者節點用戶端都可以將要求遞送至可用的 HA 主節點抄本。</td>
</tr>
<tr>
<td>etcd</td>
<td>3.2.18 版</td>
<td>3.3.1 版</td>
<td>請參閱 [etcd 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/coreos/etcd/releases/v3.3.1)。</td>
</tr>
<tr>
<td>加密 etcd 中的資料</td>
<td>N/A</td>
<td>N/A</td>
<td>先前，etcd 資料是儲存在主節點的 NFS 檔案儲存空間實例上，而此實例是在靜止時加密。現在，etcd 資料是儲存在主節點的本端磁碟上，並備份至 {{site.data.keyword.cos_full_notm}}。資料是在傳送至 {{site.data.keyword.cos_full_notm}} 期間和靜止時加密。不過，主節點的本端磁碟上的 etcd 資料不會加密。如果您想要將主節點的本端 etcd 資料加密，請[在您的叢集裡啟用 {{site.data.keyword.keymanagementservicelong_notm}}](/docs/containers?topic=containers-encryption#keyprotect)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.10.8-172 版</td>
<td>1.10.8-197 版</td>
<td>已新增 `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` 註釋，來指定負載平衡器服務要部署至其中的 VLAN。若要查看叢集裡的可用 VLAN，請執行 `ibmcloud ks vlans --zone <zone>`。</td>
</tr>
<tr>
<td>已啟用 TPM 的核心</td>
<td>N/A</td>
<td>N/A</td>
<td>搭配 TPM 晶片進行「信任運算」的裸機工作者節點會使用預設 Ubuntu 核心，直到啟用信任為止。如果您在現有叢集上[啟用信任](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)，則需要[重新載入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)任何具有 TPM 晶片的現有裸機工作者節點。若要檢查裸機工作者節點是否具有 TPM 晶片，請在執行 `ibmcloud ks machine-types --zone` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)之後，檢閱**可信任**欄位。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.8_1528（2018 年 10 月 26 日發行）的變更日誌
{: #1108_1528}

下表顯示工作者節點修正套件 1.10.8_1528 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.8_1527 版以來進行的變更">
<caption>自 1.10.8_1527 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>OS 岔斷處理</td>
<td>N/A</td>
<td>N/A</td>
<td>已將岔斷要求 (IRQ) 系統常駐程式取代為更高效能的岔斷處理程式。</td>
</tr>
</tbody>
</table>

#### 主節點修正套件 1.10.8_1527（2018 年 10 月 15 日發行）的變更日誌
{: #1108_1527}

下表顯示主節點修正套件 1.10.8_1527 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.8_1527 版以來進行的變更">
<caption>自 1.10.8_1527 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已修正 `calico-node` 容器就緒探測，以便更妥善地處理節點失敗。</td>
</tr>
<tr>
<td>叢集更新</td>
<td>N/A</td>
<td>N/A</td>
<td>已修正從不受支援的版本更新主節點時的叢集附加程式更新問題。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.8_1525（2018 年 10 月 10 日發行）的變更日誌
{: #1108_1525}

下表顯示工作者節點修正套件 1.10.8_1525 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.8_1527 版以來進行的變更">
<caption>自 1.10.8_1527 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-14633、CVE-2018-17182 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog) 的核心更新。</td>
</tr>
<tr>
<td>非作用中階段作業逾時</td>
<td>N/A</td>
<td>N/A</td>
<td>基於法規遵循的原因，將非作用中階段作業逾時設為 5 分鐘。</td>
</tr>
</tbody>
</table>


#### 1.10.8_1524（2018 年 10 月 2 日發行）的變更日誌
{: #1108_1524}

下表顯示修補程式 1.10.8_1524 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.7_1520 版以來進行的變更">
<caption>自 1.10.7_1520 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>金鑰管理服務提供者</td>
<td>N/A</td>
<td>N/A</td>
<td>已新增在叢集裡使用 Kubernetes 金鑰管理服務 (KMS) 提供者的能力，以支援 {{site.data.keyword.keymanagementservicefull}}。當您[在叢集裡啟用 {{site.data.keyword.keymanagementserviceshort}}](/docs/containers?topic=containers-encryption#keyprotect) 時，會加密您的所有 Kubernetes 密碼。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.10.7 版</td>
<td>1.10.8 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.8)。</td>
</tr>
<tr>
<td>Kubernetes DNS Autoscaler</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>請參閱 [Kubernetes DNS Autoscaler 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.10.7-146 版</td>
<td>1.10.8-172 版</td>
<td>已更新為支援 Kubernetes 1.10.8 版。此外，已更新負載平衡器錯誤訊息中的文件鏈結。</td>
</tr>
<tr>
<td>IBM 檔案儲存空間類別</td>
<td>N/A</td>
<td>N/A</td>
<td>已移除 IBM 檔案儲存空間類別中的 `mountOptions`，以使用工作者節點所提供的預設值。已移除 IBM 檔案儲存空間類別中的重複 `reclaimPolicy` 參數。<br><br>
此外，現在，當您更新叢集主節點時，預設 IBM 檔案儲存空間類別會維持不變。如果您要變更預設儲存空間類別，請執行 `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`，並將 `<storageclass>` 取代為儲存空間類別的名稱。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.7_1521（2018 年 9 月 20 日發行）的變更日誌
{: #1107_1521}

下表顯示工作者節點修正套件 1.10.7_1521 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.7_1520 版以來進行的變更">
<caption>自 1.10.7_1520 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>日誌循環</td>
<td>N/A</td>
<td>N/A</td>
<td>已切換成使用 `systemd` 計時器，而非 `cronjobs`，以防止 `logrotate` 在未於 90 天內重新載入或更新的工作者節點上失敗。**附註**：在此次要版本的所有舊版中，Cron 工作失敗之後會填滿主要磁碟，因為日誌不會循環。在工作者節點作用 90 天但未進行更新或重新載入之後，Cron 工作就會失敗。如果日誌填滿整個主要磁碟，則工作者節點會進入失敗狀態。使用 `ibmcloud ks worker-reload` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)或 `ibmcloud ks worker-update` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)，即可修正工作者節點。</td>
</tr>
<tr>
<td>工作者節點運行環境元件（`kubelet`、`kube-proxy`、`docker`）</td>
<td>N/A</td>
<td>N/A</td>
<td>已移除主要磁碟上的運行環境元件的相依關係。主要磁碟滿載時，此加強功能可避免工作者節點失敗。</td>
</tr>
<tr>
<td>root 密碼有效期限</td>
<td>N/A</td>
<td>N/A</td>
<td>基於法規遵循的原因，工作者節點的 root 密碼會在 90 天後到期。如果您的自動化工具需要以 root 使用者身分登入工作者節點，或依賴以 root 使用者身分執行的 Cron 工作，則可以登入工作者節點並執行 `chage -M -1 root` 來停用密碼有效期限。**附註**：如果您的安全規範需求可避免以 root 使用者身分執行或移除密碼有效期限，則請不要停用有效期限。相反地，您可以至少每 90 天[更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)或[重新載入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)工作者節點一次。</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>定期清除暫時性裝載裝置，以防止它們變成無界限。此動作解決 [Kubernetes 問題 57345 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/issues/57345)。</td>
</tr>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>已停用預設 Docker 橋接器，因此 `172.17.0.0/16` IP 範圍現在用於專用路徑。如果您依賴直接在主機上執行 `docker` 指令或使用可裝載 Docker Socket 的 Pod 以在工作者節點中建置 Docker 容器，則請從下列選項中進行選擇。<ul><li>若要確保建置容器時的外部網路連線功能，請執行 `docker build . --network host`。</li>
<li>若要明確建立要在建置容器時使用的網路，請執行 `docker network create`，然後使用此網路。</li></ul>
**附註**：與 Docker Socket 或 Docker 具有直接相依關係嗎？[更新至 `containerd`（而非 `docker`）作為容器運行環境](/docs/containers?topic=containers-cs_versions#containerd)，因此您的叢集已準備好要執行 Kubernetes 1.11 版或更新版本。</td>
</tr>
</tbody>
</table>

#### 1.10.7_1520（2018 年 9 月 4 日發行）的變更日誌
{: #1107_1520}

下表顯示修補程式 1.10.7_1520 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.5_1519 版以來進行的變更">
<caption>自 1.10.5_1519 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>3.1.3 版</td>
<td>3.2.1 版</td>
<td>請參閱 Calico [版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.2/releases/#v321)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.10.5-118 版</td>
<td>1.10.7-146 版</td>
<td>已更新為支援 Kubernetes 1.10.7 版。此外，已變更雲端提供者配置，可更恰當地處理 `externalTrafficPolicy` 設為 `local` 之負載平衡器服務的更新。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>334</td>
<td>338</td>
<td>已將 incubator 版本更新為 1.8。檔案儲存空間會佈建給您選取的特定區域。除非您使用多區域叢集，並且需要新增地區及區域標籤，否則無法更新現有（靜態）PV 實例的標籤。<br><br> 已從 IBM 所提供檔案儲存空間類別的裝載選項移除預設 NFS 版本。主機的作業系統現在會與 IBM Cloud 基礎架構 (SoftLayer) NFS 伺服器協議 NFS 版本。若要手動設定特定的 NFS 版本，或變更主機作業系統所協議 PV 的 NFS 版本，請參閱[變更預設 NFS 版本](/docs/containers?topic=containers-file_storage#nfs_version_class)。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.10.5 版</td>
<td>1.10.7 版</td>
<td>請參閱 Kubernetes [版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.7)。</td>
</tr>
<tr>
<td>Kubernetes Heapster 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已增加 `heapster-nanny` 容器的資源限制。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.5_1519（2018 年 8 月 23 日發行）的變更日誌
{: #1105_1519}

下表顯示工作者節點修正套件 1.10.5_1519 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.5_1518 版以來進行的變更">
<caption>自 1.10.5_1518 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>已更新 `systemd` 來修正 `cgroup` 洩漏。</td>
</tr>
<tr>
<td>核心</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-3620、CVE-2018-3646 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://usn.ubuntu.com/3741-1/) 的核心更新。</td>
</tr>
</tbody>
</table>


#### 工作者節點修正套件 1.10.5_1518（2018 年 8 月 13 日發行）的變更日誌
{: #1105_1518}

下表顯示工作者節點修正套件 1.10.5_1518 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.5_1517 版以來進行的變更">
<caption>自 1.10.5_1517 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>已安裝 Ubuntu 套件的更新。</td>
</tr>
</tbody>
</table>

#### 1.10.5_1517（2018 年 7 月 27 日發行）的變更日誌
{: #1105_1517}

下表顯示修補程式 1.10.5_1517 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.3_1514 版以來進行的變更">
<caption>自 1.10.3_1514 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>3.1.1 版</td>
<td>3.1.3 版</td>
<td>請參閱 Calico [版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.1/releases/#v313)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.10.3-85 版</td>
<td>1.10.5-118 版</td>
<td>已更新為支援 Kubernetes 1.10.5 版。此外，LoadBalancer 服務 `create failure` 事件現在包括所有可攜式子網路錯誤。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>320</td>
<td>334</td>
<td>已將持續性磁區建立的逾時從 15 分鐘增加至 30 分鐘。已將預設計費類型變更為 `hourly`。已將裝載選項新增至預先定義的儲存空間類別。在 IBM Cloud 基礎架構 (SoftLayer) 帳戶的 NFS 檔案儲存空間實例中，已將 **Notes** 欄位變更為 JSON 格式，並已新增 PV 部署至其中的 Kubernetes 名稱空間。為了支援多區域叢集，已將區域及地區標籤新增至持續性磁區。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.10.3 版</td>
<td>1.10.5 版</td>
<td>請參閱 Kubernetes [版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.5)。</td>
</tr>
<tr>
<td>核心</td>
<td>N/A</td>
<td>N/A</td>
<td>對高效能網路工作負載的工作者節點網路設定做出少量改善。</td>
</tr>
<tr>
<td>OpenVPN 用戶端</td>
<td>N/A</td>
<td>N/A</td>
<td>在 `kube-system` 名稱空間中執行的 OpenVPN 用戶端 `vpn` 部署，現在是由 Kubernetes `addon-manager` 管理。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.3_1514（2018 年 7 月 3 日發行）的變更日誌
{: #1103_1514}

下表顯示工作者節點修正套件 1.10.3_1514 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.3_1513 版以來進行的變更">
<caption>自 1.10.3_1513 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>N/A</td>
<td>N/A</td>
<td>已最佳化高效能網路工作負載的 `sysctl`。</td>
</tr>
</tbody>
</table>


#### 工作者節點修正套件 1.10.3_1513（2018 年 6 月 21 日發行）的變更日誌
{: #1103_1513}

下表顯示工作者節點修正套件 1.10.3_1513 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.3_1512 版以來進行的變更">
<caption>自 1.10.3_1512 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>對於非加密的機型，當重新載入或更新工作者節點時，會取得全新的檔案系統來清理次要磁碟。</td>
</tr>
</tbody>
</table>

#### 1.10.3_1512（2018 年 6 月 12 日發行）的變更日誌
{: #1103_1512}

下表顯示修補程式 1.10.3_1512 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.3_1510 版以來進行的變更">
<caption>自 1.10.1_1510 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>1.10.1 版</td>
<td>1.10.3 版</td>
<td>請參閱 Kubernetes [版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.3)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已針對叢集的 Kubernetes API 伺服器將 `PodSecurityPolicy` 新增至 `--enable-admission-plugins` 選項，並已將叢集配置為支援 Pod 安全政策。如需相關資訊，請參閱[配置 Pod 安全原則](/docs/containers?topic=containers-psp)。</td>
</tr>
<tr>
<td>Kubelet 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>啟用 `--authentication-token-webhook` 選項，支援 API 載送及服務帳戶記號，以對 `kubelet` HTTPS 端點進行鑑別。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.10.1-52 版</td>
<td>1.10.3-85 版</td>
<td>已更新為支援 Kubernetes 1.10.3 版。</td>
</tr>
<tr>
<td>OpenVPN 用戶端</td>
<td>N/A</td>
<td>N/A</td>
<td>已將 `livenessProbe` 新增至 `kube-system` 名稱空間中執行的 OpenVPN 用戶端 `vpn` 部署。</td>
</tr>
<tr>
<td>核心更新</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>新的工作者節點映像檔，其含有 [CVE-2018-3639 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639) 的核心更新。</td>
</tr>
</tbody>
</table>



#### 工作者節點修正套件 1.10.1_1510（2018 年 5 月 18 日發行）的變更日誌
{: #1101_1510}

下表顯示工作者節點修正套件 1.10.1_1510 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.1_1509 版以來進行的變更">
<caption>自 1.10.1_1509 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>修正以處理您使用區塊儲存空間外掛程式時發生的錯誤。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.10.1_1509（2018 年 5 月 16 日發行）的變更日誌
{: #1101_1509}

下表顯示工作者節點修正套件 1.10.1_1509 中所包括的變更。
{: shortdesc}

<table summary="自 1.10.1_1508 版以來進行的變更">
<caption>自 1.10.1_1508 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>您在 `kubelet` 根目錄中儲存的資料，現在儲存在工作者節點機器的較大次要磁碟上。</td>
</tr>
</tbody>
</table>

#### 1.10.1_1508（2018 年 5 月 1 日發行）的變更日誌
{: #1101_1508}

下表顯示修補程式 1.10.1_1508 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.7_1510 版以來進行的變更">
<caption>自 1.9.7_1510 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>2.6.5 版</td>
<td>3.1.1 版</td>
<td>請參閱 Calico [版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.1/releases/#v311)。</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>1.5.0 版</td>
<td>1.5.2 版</td>
<td>請參閱 Kubernetes Heapster [版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2)。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.9.7 版</td>
<td>1.10.1 版</td>
<td>請參閱 Kubernetes [版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>針對叢集的 Kubernetes API 伺服器，已將 <code>StorageObjectInUseProtection</code> 新增至 <code>--enable-admission-plugins</code> 選項。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>請參閱 Kubernetes DNS [版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/dns/releases/tag/1.14.10)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.9.7-102 版</td>
<td>1.10.1-52 版</td>
<td>已更新成支援 Kubernetes 1.10 版。</td>
</tr>
<tr>
<td>GPU 支援</td>
<td>N/A</td>
<td>N/A</td>
<td>[圖形處理裝置 (GPU) 容器工作負載](/docs/containers?topic=containers-app#gpu_app)支援現在可用於排程及執行。如需可用的 GPU 機型清單，請參閱[工作者節點的硬體](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)。如需相關資訊，請參閱 Kubernetes 文件來[排程 GPU ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/)。</td>
</tr>
</tbody>
</table>

<br />


### 1.9 版變更日誌（自 2018 年 12 月 27 日起不再支援）
{: #19_changelog}

檢閱 1.9 版變更日誌。
{: shortdesc}

*   [工作者節點修正套件 1.9.11_1539（2018 年 12 月 17 日發行）的變更日誌](#1911_1539)
*   [工作者節點修正套件 1.9.11_1538（2018 年 12 月 4 日發行）的變更日誌](#1911_1538)
*   [工作者節點修正套件 1.9.11_1537（2018 年 11 月 27 日發行）的變更日誌](#1911_1537)
*   [1.9.11_1536（2018 年 11 月 19 日發行）的變更日誌](#1911_1536)
*   [工作者節點修正套件 1.9.10_1532（2018 年 11 月 7 日發行）的變更日誌](#1910_1532)
*   [工作者節點修正套件 1.9.10_1531（2018 年 10 月 26 日發行）的變更日誌](#1910_1531)
*   [主節點修正套件 1.9.10_1530（2018 年 10 月 15 日發行）的變更日誌](#1910_1530)
*   [工作者節點修正套件 1.9.10_1528（2018 年 10 月 10 日發行）的變更日誌](#1910_1528)
*   [1.9.10_1527（2018 年 10 月 2 日發行）的變更日誌](#1910_1527)
*   [工作者節點修正套件 1.9.10_1524（2018 年 9 月 20 日發行）的變更日誌](#1910_1524)
*   [1.9.10_1523（2018 年 9 月 4 日發行）的變更日誌](#1910_1523)
*   [工作者節點修正套件 1.9.9_1522（2018 年 8 月 23 日發行）的變更日誌](#199_1522)
*   [工作者節點修正套件 1.9.9_1521（2018 年 8 月 13 日發行）的變更日誌](#199_1521)
*   [1.9.9_1520（2018 年 7 月 27 日發行）的變更日誌](#199_1520)
*   [工作者節點修正套件 1.9.8_1517（2018 年 7 月 3 日發行）的變更日誌](#198_1517)
*   [工作者節點修正套件 1.9.8_1516（2018 年 6 月 21 日發行）的變更日誌](#198_1516)
*   [1.9.8_1515（2018 年 6 月 19 日發行）的變更日誌](#198_1515)
*   [工作者節點修正套件 1.9.7_1513（2018 年 6 月 11 日發行）的變更日誌](#197_1513)
*   [工作者節點修正套件 1.9.7_1512（2018 年 5 月 18 日發行）的變更日誌](#197_1512)
*   [工作者節點修正套件 1.9.7_1511（2018 年 5 月 16 日發行）的變更日誌](#197_1511)
*   [1.9.7_1510（2018 年 4 月 30 日發行）的變更日誌](#197_1510)

#### 工作者節點修正套件 1.9.11_1539（2018 年 12 月 17 日發行）的變更日誌
{: #1911_1539}

下表顯示工作者節點修正套件 1.9.11_1539 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.11_1538 版以來進行的變更">
<caption>自 1.9.11_1538 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>已安裝 Ubuntu 套件的更新。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.9.11_1538（2018 年 12 月 4 日發行）的變更日誌
{: #1911_1538}

下表顯示工作者節點修正套件 1.9.11_1538 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.11_1537 版以來進行的變更">
<caption>自 1.9.11_1537 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>工作者節點資源使用率</td>
<td>N/A</td>
<td>N/A</td>
<td>已新增 kukelet 和 Docker 的專用 cgroups，以防止這些元件耗盡資源。如需相關資訊，請參閱[工作者節點資源保留](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.9.11_1537（2018 年 11 月 27 日發行）的變更日誌
{: #1911_1537}

下表顯示工作者節點修正套件 1.9.11_1537 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.11_1536 版以來進行的變更">
<caption>自 1.9.11_1536 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>請參閱 [Docker 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.docker.com/engine/release-notes/#18061-ce)。</td>
</tr>
</tbody>
</table>

#### 1.9.11_1536（2018 年 11 月 19 日發行）的變更日誌
{: #1911_1536}

下表顯示修補程式 1.9.11_1536 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.10_1532 版以來進行的變更">
<caption>自 1.9.10_1532 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>2.6.5 版</td>
<td>2.6.12 版</td>
<td>請參閱 [Calico 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v2.6/releases/#v2612)。此更新解決了 [Tigera Technical Advisory TTA-2018-001 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.projectcalico.org/security-bulletins/)。</td>
</tr>
<tr>
<td>核心</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-7755 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) 的核心更新。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.9.10 版</td>
<td>1.9.11 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.11)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}}</td>
<td>1.9.10-219 版</td>
<td>1.9.11-249 版</td>
<td>已更新為支援 Kubernetes 1.9.11 版。</td>
</tr>
<tr>
<td>OpenVPN 用戶端及伺服器</td>
<td>2.4.4-r2</td>
<td>2.4.6-r3-IKS-8</td>
<td>已更新 [CVE-2018-0732 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) 及 [CVE-2018-0737 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737) 的映像檔。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.9.10_1532（2018 年 11 月 7 日發行）的變更日誌
{: #1910_1532}

下表顯示工作者節點修正套件 1.9.11_1532 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.10_1531 版以來進行的變更">
<caption>自 1.9.10_1531 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>已啟用 TPM 的核心</td>
<td>N/A</td>
<td>N/A</td>
<td>搭配 TPM 晶片進行「信任運算」的裸機工作者節點會使用預設 Ubuntu 核心，直到啟用信任為止。如果您在現有叢集上[啟用信任](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)，則需要[重新載入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)任何具有 TPM 晶片的現有裸機工作者節點。若要檢查裸機工作者節點是否具有 TPM 晶片，請在執行 `ibmcloud ks machine-types --zone` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)之後，檢閱**可信任**欄位。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.9.10_1531（2018 年 10 月 26 日發行）的變更日誌
{: #1910_1531}

下表顯示工作者節點修正套件 1.9.10_1531 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.10_1530 版以來進行的變更">
<caption>自 1.9.10_1530 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>OS 岔斷處理</td>
<td>N/A</td>
<td>N/A</td>
<td>已將岔斷要求 (IRQ) 系統常駐程式取代為更高效能的岔斷處理程式。</td>
</tr>
</tbody>
</table>

#### 主節點修正套件 1.9.10_1530（2018 年 10 月 15 日發行）的變更日誌
{: #1910_1530}

下表顯示工作者節點修正套件 1.9.10_1530 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.10_1527 版以來進行的變更">
<caption>自 1.9.10_1527 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>叢集更新</td>
<td>N/A</td>
<td>N/A</td>
<td>已修正從不受支援的版本更新主節點時的叢集附加程式更新問題。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.9.10_1528（2018 年 10 月 10 日發行）的變更日誌
{: #1910_1528}

下表顯示工作者節點修正套件 1.9.10_1528 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.10_1527 版以來進行的變更">
<caption>自 1.9.10_1527 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-14633、CVE-2018-17182 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog) 的核心更新。</td>
</tr>
<tr>
<td>非作用中階段作業逾時</td>
<td>N/A</td>
<td>N/A</td>
<td>基於法規遵循的原因，將非作用中階段作業逾時設為 5 分鐘。</td>
</tr>
</tbody>
</table>


#### 1.9.10_1527（2018 年 10 月 2 日發行）的變更日誌
{: #1910_1527}

下表顯示修補程式 1.9.10_1527 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.10_1523 版以來進行的變更">
<caption>自 1.9.10_1523 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.9.10-192 版</td>
<td>1.9.10-219 版</td>
<td>已更新負載平衡器錯誤訊息中的文件鏈結。</td>
</tr>
<tr>
<td>IBM 檔案儲存空間類別</td>
<td>N/A</td>
<td>N/A</td>
<td>已移除 IBM 檔案儲存空間類別中的 `mountOptions`，以使用工作者節點所提供的預設值。已移除 IBM 檔案儲存空間類別中的重複 `reclaimPolicy` 參數。<br><br>
此外，現在，當您更新叢集主節點時，預設 IBM 檔案儲存空間類別會維持不變。如果您要變更預設儲存空間類別，請執行 `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`，並將 `<storageclass>` 取代為儲存空間類別的名稱。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.9.10_1524（2018 年 9 月 20 日發行）的變更日誌
{: #1910_1524}

下表顯示工作者節點修正套件 1.9.10_1524 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.10_1523 版以來進行的變更">
<caption>自 1.9.10_1523 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>日誌循環</td>
<td>N/A</td>
<td>N/A</td>
<td>已切換成使用 `systemd` 計時器，而非 `cronjobs`，以防止 `logrotate` 在未於 90 天內重新載入或更新的工作者節點上失敗。**附註**：在此次要版本的所有舊版中，Cron 工作失敗之後會填滿主要磁碟，因為日誌不會循環。在工作者節點作用 90 天但未進行更新或重新載入之後，Cron 工作就會失敗。如果日誌填滿整個主要磁碟，則工作者節點會進入失敗狀態。使用 `ibmcloud ks worker-reload` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)或 `ibmcloud ks worker-update` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)，即可修正工作者節點。</td>
</tr>
<tr>
<td>工作者節點運行環境元件（`kubelet`、`kube-proxy`、`docker`）</td>
<td>N/A</td>
<td>N/A</td>
<td>已移除主要磁碟上的運行環境元件的相依關係。主要磁碟滿載時，此加強功能可避免工作者節點失敗。</td>
</tr>
<tr>
<td>root 密碼有效期限</td>
<td>N/A</td>
<td>N/A</td>
<td>基於法規遵循的原因，工作者節點的 root 密碼會在 90 天後到期。如果您的自動化工具需要以 root 使用者身分登入工作者節點，或依賴以 root 使用者身分執行的 Cron 工作，則可以登入工作者節點並執行 `chage -M -1 root` 來停用密碼有效期限。**附註**：如果您的安全規範需求可避免以 root 使用者身分執行或移除密碼有效期限，則請不要停用有效期限。相反地，您可以至少每 90 天[更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)或[重新載入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)工作者節點一次。</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>定期清除暫時性裝載裝置，以防止它們變成無界限。此動作解決 [Kubernetes 問題 57345 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/issues/57345)。</td>
</tr>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>已停用預設 Docker 橋接器，因此 `172.17.0.0/16` IP 範圍現在用於專用路徑。如果您依賴直接在主機上執行 `docker` 指令或使用可裝載 Docker Socket 的 Pod 以在工作者節點中建置 Docker 容器，則請從下列選項中進行選擇。<ul><li>若要確保建置容器時的外部網路連線功能，請執行 `docker build . --network host`。</li>
<li>若要明確建立要在建置容器時使用的網路，請執行 `docker network create`，然後使用此網路。</li></ul>
**附註**：與 Docker Socket 或 Docker 具有直接相依關係嗎？[更新至 `containerd`（而非 `docker`）作為容器運行環境](/docs/containers?topic=containers-cs_versions#containerd)，因此您的叢集已準備好要執行 Kubernetes 1.11 版或更新版本。</td>
</tr>
</tbody>
</table>

#### 1.9.10_1523（2018 年 9 月 4 日發行）的變更日誌
{: #1910_1523}

下表顯示修補程式 1.9.10_1523 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.9_1522 版以來進行的變更">
<caption>自 1.9.9_1522 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.9.9-167 版</td>
<td>1.9.10-192 版</td>
<td>已更新為支援 Kubernetes 1.9.10 版。此外，已變更雲端提供者配置，可更恰當地處理 `externalTrafficPolicy` 設為 `local` 之負載平衡器服務的更新。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>334</td>
<td>338</td>
<td>已將 incubator 版本更新為 1.8。檔案儲存空間會佈建給您選取的特定區域。除非您使用多區域叢集，並且需要新增地區及區域標籤，否則無法更新現有（靜態）PV 實例的標籤。<br><br>已從 IBM 所提供檔案儲存空間類別的裝載選項移除預設 NFS 版本。主機的作業系統現在會與 IBM Cloud 基礎架構 (SoftLayer) NFS 伺服器協議 NFS 版本。若要手動設定特定的 NFS 版本，或變更主機作業系統所協議 PV 的 NFS 版本，請參閱[變更預設 NFS 版本](/docs/containers?topic=containers-file_storage#nfs_version_class)。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.9.9 版</td>
<td>1.9.10 版</td>
<td>請參閱 Kubernetes [版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.10)。</td>
</tr>
<tr>
<td>Kubernetes Heapster 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已增加 `heapster-nanny` 容器的資源限制。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.9.9_1522（2018 年 8 月 23 日發行）的變更日誌
{: #199_1522}

下表顯示工作者節點修正套件 1.9.9_1522 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.9_1521 版以來進行的變更">
<caption>自 1.9.9_1521 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>已更新 `systemd` 來修正 `cgroup` 洩漏。</td>
</tr>
<tr>
<td>核心</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-3620、CVE-2018-3646 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://usn.ubuntu.com/3741-1/) 的核心更新。</td>
</tr>
</tbody>
</table>


#### 工作者節點修正套件 1.9.9_1521（2018 年 8 月 13 日發行）的變更日誌
{: #199_1521}

下表顯示工作者節點修正套件 1.9.9_1521 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.9_1520 版以來進行的變更">
<caption>自 1.9.9_1520 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>已安裝 Ubuntu 套件的更新。</td>
</tr>
</tbody>
</table>

#### 1.9.9_1520（2018 年 7 月 27 日發行）的變更日誌
{: #199_1520}

下表顯示修補程式 1.9.9_1520 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.8_1517 版以來進行的變更">
<caption>自 1.9.8_1517 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.9.8-141 版</td>
<td>1.9.9-167 版</td>
<td>已更新為支援 Kubernetes 1.9.9 版。此外，LoadBalancer 服務 `create failure` 事件現在包括所有可攜式子網路錯誤。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>320</td>
<td>334</td>
<td>已將持續性磁區建立的逾時從 15 分鐘增加至 30 分鐘。已將預設計費類型變更為 `hourly`。已將裝載選項新增至預先定義的儲存空間類別。在 IBM Cloud 基礎架構 (SoftLayer) 帳戶的 NFS 檔案儲存空間實例中，已將 **Notes** 欄位變更為 JSON 格式，並已新增 PV 部署至其中的 Kubernetes 名稱空間。為了支援多區域叢集，已將區域及地區標籤新增至持續性磁區。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.9.8 版</td>
<td>1.9.9 版</td>
<td>請參閱 Kubernetes [版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.9)。</td>
</tr>
<tr>
<td>核心</td>
<td>N/A</td>
<td>N/A</td>
<td>對高效能網路工作負載的工作者節點網路設定做出少量改善。</td>
</tr>
<tr>
<td>OpenVPN 用戶端</td>
<td>N/A</td>
<td>N/A</td>
<td>在 `kube-system` 名稱空間中執行的 OpenVPN 用戶端 `vpn` 部署，現在是由 Kubernetes `addon-manager` 管理。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.9.8_1517（2018 年 7 月 3 日發行）的變更日誌
{: #198_1517}

下表顯示工作者節點修正套件 1.9.8_1517 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.8_1516 版以來進行的變更">
<caption>自 1.9.8_1516 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>N/A</td>
<td>N/A</td>
<td>已最佳化高效能網路工作負載的 `sysctl`。</td>
</tr>
</tbody>
</table>


#### 工作者節點修正套件 1.9.8_1516（2018 年 6 月 21 日發行）的變更日誌
{: #198_1516}

下表顯示工作者節點修正套件 1.9.8_1516 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.8_1515 版以來進行的變更">
<caption>自 1.9.8_1515 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>對於非加密的機型，當重新載入或更新工作者節點時，會取得全新的檔案系統來清理次要磁碟。</td>
</tr>
</tbody>
</table>

#### 1.9.8_1515（2018 年 6 月 19 日發行）的變更日誌
{: #198_1515}

下表顯示修補程式 1.9.8_1515 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.7_1513 版以來進行的變更">
<caption>自 1.9.7_1513 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>1.9.7 版</td>
<td>1.9.8 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.8)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已針對叢集的 Kubernetes API 伺服器將 `PodSecurityPolicy` 新增至`--admission-control` 選項，並已將叢集配置為支援 Pod 安全原則。如需相關資訊，請參閱[配置 Pod 安全原則](/docs/containers?topic=containers-psp)。</td>
</tr>
<tr>
<td>IBM Cloud Provider</td>
<td>1.9.7-102 版</td>
<td>1.9.8-141 版</td>
<td>已更新為支援 Kubernetes 1.9.8 版。</td>
</tr>
<tr>
<td>OpenVPN 用戶端</td>
<td>N/A</td>
<td>N/A</td>
<td>已將 <code>livenessProbe</code> 新增至 <code>kube-system</code> 名稱空間中執行的 OpenVPN 用戶端 <code>vpn</code> 部署。</td>
</tr>
</tbody>
</table>


#### 工作者節點修正套件 1.9.7_1513（2018 年 6 月 11 日發行）的變更日誌
{: #197_1513}

下表顯示工作者節點修正套件 1.9.7_1513 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.7_1512 版以來進行的變更">
<caption>自 1.9.7_1512 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心更新</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>新的工作者節點映像檔，其含有 [CVE-2018-3639 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639) 的核心更新。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.9.7_1512（2018 年 5 月 18 日發行）的變更日誌
{: #197_1512}

下表顯示工作者節點修正套件 1.9.7_1512 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.7_1511 版以來進行的變更">
<caption>自 1.9.7_1511 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>修正以處理您使用區塊儲存空間外掛程式時發生的錯誤。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.9.7_1511（2018 年 5 月 16 日發行）的變更日誌
{: #197_1511}

下表顯示工作者節點修正套件 1.9.7_1511 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.7_1510 版以來進行的變更">
<caption>自 1.9.7_1510 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>您在 `kubelet` 根目錄中儲存的資料，現在儲存在工作者節點機器的較大次要磁碟上。</td>
</tr>
</tbody>
</table>

#### 1.9.7_1510（2018 年 4 月 30 日發行）的變更日誌
{: #197_1510}

下表顯示修補程式 1.9.7_1510 中所包括的變更。
{: shortdesc}

<table summary="自 1.9.3_1506 版以來進行的變更">
<caption>自 1.9.3_1506 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>1.9.3 版</td>
<td>1.9.7 版</td>
<td><p>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7)。此版本處理 [CVE-2017-1002101 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) 及 [CVE-2017-1002102 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) 漏洞。</p><p><strong>附註</strong>：現在，`secret`、`configMap`、`downwardAPI` 及預期的磁區已裝載為唯讀磁區。先前，應用程式可以將資料寫入至這些磁區，但系統可以自動回復資料。如果您的應用程式依賴先前不安全的行為，請相應地修改它們。</p></td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>針對叢集的 Kubernetes API 伺服器，已將 `admissionregistration.k8s.io/v1alpha1=true` 新增至 `--runtime-config` 選項。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.9.3-71 版</td>
<td>1.9.7-102 版</td>
<td>`NodePort` 及 `LoadBalancer` 服務現在支援[保留用戶端來源 IP](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)，方法為將 `service.spec.externalTrafficPolicy` 設為 `Local`。</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>針對較舊叢集，修正[邊緣節點](/docs/containers?topic=containers-edge#edge)容忍設定。</td>
</tr>
</tbody>
</table>

<br />


### 1.8 版變更日誌（不受支援）
{: #18_changelog}

檢閱 1.8 版變更日誌。
{: shortdesc}

*   [工作者節點修正套件 1.8.15_1521（2018 年 9 月 20 日發行）的變更日誌](#1815_1521)
*   [工作者節點修正套件 1.8.15_1520（2018 年 8 月 23 日發行）的變更日誌](#1815_1520)
*   [工作者節點修正套件 1.8.15_1519（2018 年 8 月 13 日發行）的變更日誌](#1815_1519)
*   [1.8.15_1518（2018 年 7 月 27 日發行）的變更日誌](#1815_1518)
*   [工作者節點修正套件 1.8.13_1516（2018 年 7 月 3 日發行）的變更日誌](#1813_1516)
*   [工作者節點修正套件 1.8.13_1515（2018 年 6 月 21 日發行）的變更日誌](#1813_1515)
*   [1.8.13_1514（2018 年 6 月 19 日發行）的變更日誌](#1813_1514)
*   [工作者節點修正套件 1.8.11_1512（2018 年 6 月 11 日發行）的變更日誌](#1811_1512)
*   [工作者節點修正套件 1.8.11_1511（2018 年 5 月 18 日發行）的變更日誌](#1811_1511)
*   [工作者節點修正套件 1.8.11_1510（2018 年 5 月 16 日發行）的變更日誌](#1811_1510)
*   [1.8.11_1509（2018 年 4 月 19 日發行）的變更日誌](#1811_1509)

#### 工作者節點修正套件 1.8.15_1521（2018 年 9 月 20 日發行）的變更日誌
{: #1815_1521}

<table summary="自 1.8.15_1520 版以來進行的變更">
<caption>自 1.8.15_1520 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>日誌循環</td>
<td>N/A</td>
<td>N/A</td>
<td>已切換成使用 `systemd` 計時器，而非 `cronjobs`，以防止 `logrotate` 在未於 90 天內重新載入或更新的工作者節點上失敗。**附註**：在此次要版本的所有舊版中，Cron 工作失敗之後會填滿主要磁碟，因為日誌不會循環。在工作者節點作用 90 天但未進行更新或重新載入之後，Cron 工作就會失敗。如果日誌填滿整個主要磁碟，則工作者節點會進入失敗狀態。使用 `ibmcloud ks worker-reload` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)或 `ibmcloud ks worker-update` [指令](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)，即可修正工作者節點。</td>
</tr>
<tr>
<td>工作者節點運行環境元件（`kubelet`、`kube-proxy`、`docker`）</td>
<td>N/A</td>
<td>N/A</td>
<td>已移除主要磁碟上的運行環境元件的相依關係。主要磁碟滿載時，此加強功能可避免工作者節點失敗。</td>
</tr>
<tr>
<td>root 密碼有效期限</td>
<td>N/A</td>
<td>N/A</td>
<td>基於法規遵循的原因，工作者節點的 root 密碼會在 90 天後到期。如果您的自動化工具需要以 root 使用者身分登入工作者節點，或依賴以 root 使用者身分執行的 Cron 工作，則可以登入工作者節點並執行 `chage -M -1 root` 來停用密碼有效期限。**附註**：如果您的安全規範需求可避免以 root 使用者身分執行或移除密碼有效期限，則請不要停用有效期限。相反地，您可以至少每 90 天[更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)或[重新載入](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)工作者節點一次。</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>定期清除暫時性裝載裝置，以防止它們變成無界限。此動作解決 [Kubernetes 問題 57345 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/issues/57345)。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.8.15_1520（2018 年 8 月 23 日發行）的變更日誌
{: #1815_1520}

<table summary="自 1.8.15_1519 版以來進行的變更">
<caption>自 1.8.15_1519 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>已更新 `systemd` 來修正 `cgroup` 洩漏。</td>
</tr>
<tr>
<td>核心</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>已更新工作者節點映像檔，其含有 [CVE-2018-3620、CVE-2018-3646 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://usn.ubuntu.com/3741-1/) 的核心更新。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.8.15_1519（2018 年 8 月 13 日發行）的變更日誌
{: #1815_1519}

<table summary="自 1.8.15_1518 版以來進行的變更">
<caption>自 1.8.15_1518 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 套件</td>
<td>N/A</td>
<td>N/A</td>
<td>已安裝 Ubuntu 套件的更新。</td>
</tr>
</tbody>
</table>

#### 1.8.15_1518（2018 年 7 月 27 日發行）的變更日誌
{: #1815_1518}

<table summary="自 1.8.13_1516 版以來進行的變更">
<caption>自 1.8.13_1516 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.8.13-176 版</td>
<td>1.8.15-204 版</td>
<td>已更新為支援 Kubernetes 1.8.15 版。此外，LoadBalancer 服務 `create failure` 事件現在包括所有可攜式子網路錯誤。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage 外掛程式</td>
<td>320</td>
<td>334</td>
<td>已將持續性磁區建立的逾時從 15 分鐘增加至 30 分鐘。已將預設計費類型變更為 `hourly`。已將裝載選項新增至預先定義的儲存空間類別。在 IBM Cloud 基礎架構 (SoftLayer) 帳戶的 NFS 檔案儲存空間實例中，已將 **Notes** 欄位變更為 JSON 格式，並已新增 PV 部署至其中的 Kubernetes 名稱空間。為了支援多區域叢集，已將區域及地區標籤新增至持續性磁區。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>1.8.13 版</td>
<td>1.8.15 版</td>
<td>請參閱 Kubernetes [版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.15)。</td>
</tr>
<tr>
<td>核心</td>
<td>N/A</td>
<td>N/A</td>
<td>對高效能網路工作負載的工作者節點網路設定做出少量改善。</td>
</tr>
<tr>
<td>OpenVPN 用戶端</td>
<td>N/A</td>
<td>N/A</td>
<td>在 `kube-system` 名稱空間中執行的 OpenVPN 用戶端 `vpn` 部署，現在是由 Kubernetes `addon-manager` 管理。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.8.13_1516（2018 年 7 月 3 日發行）的變更日誌
{: #1813_1516}

<table summary="自 1.8.13_1515 版以來進行的變更">
<caption>自 1.8.13_1515 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心</td>
<td>N/A</td>
<td>N/A</td>
<td>已最佳化高效能網路工作負載的 `sysctl`。</td>
</tr>
</tbody>
</table>


#### 工作者節點修正套件 1.8.13_1515（2018 年 6 月 21 日發行）的變更日誌
{: #1813_1515}

<table summary="自 1.8.13_1514 版以來進行的變更">
<caption>自 1.8.13_1514 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>對於非加密的機型，當重新載入或更新工作者節點時，會取得全新的檔案系統來清理次要磁碟。</td>
</tr>
</tbody>
</table>

#### 1.8.13_1514（2018 年 6 月 19 日發行）的變更日誌
{: #1813_1514}

<table summary="自 1.8.11_1512 版以來進行的變更">
<caption>自 1.8.11_1512 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>1.8.11	版</td>
<td>1.8.13 版</td>
<td>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.13)。</td>
</tr>
<tr>
<td>Kubernetes 配置</td>
<td>N/A</td>
<td>N/A</td>
<td>已針對叢集的 Kubernetes API 伺服器將 `PodSecurityPolicy` 新增至`--admission-control` 選項，並已將叢集配置為支援 Pod 安全原則。如需相關資訊，請參閱[配置 Pod 安全原則](/docs/containers?topic=containers-psp)。</td>
</tr>
<tr>
<td>IBM Cloud Provider</td>
<td>1.8.11-126 版</td>
<td>1.8.13-176 版</td>
<td>已更新為支援 Kubernetes 1.8.13 版。</td>
</tr>
<tr>
<td>OpenVPN 用戶端</td>
<td>N/A</td>
<td>N/A</td>
<td>已將 <code>livenessProbe</code> 新增至 <code>kube-system</code> 名稱空間中執行的 OpenVPN 用戶端 <code>vpn</code> 部署。</td>
</tr>
</tbody>
</table>


#### 工作者節點修正套件 1.8.11_1512（2018 年 6 月 11 日發行）的變更日誌
{: #1811_1512}

<table summary="自 1.8.11_1511 版以來進行的變更">
<caption>自 1.8.11_1511 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心更新</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>新的工作者節點映像檔，其含有 [CVE-2018-3639 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639) 的核心更新。</td>
</tr>
</tbody>
</table>


#### 工作者節點修正套件 1.8.11_1511（2018 年 5 月 18 日發行）的變更日誌
{: #1811_1511}

<table summary="自 1.8.11_1510 版以來進行的變更">
<caption>自 1.8.11_1510 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>修正以處理您使用區塊儲存空間外掛程式時發生的錯誤。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.8.11_1510（2018 年 5 月 16 日發行）的變更日誌
{: #1811_1510}

<table summary="自 1.8.11_1509 版以來進行的變更">
<caption>自 1.8.11_1509 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>您在 `kubelet` 根目錄中儲存的資料，現在儲存在工作者節點機器的較大次要磁碟上。</td>
</tr>
</tbody>
</table>


#### 1.8.11_1509（2018 年 4 月 19 日發行）的變更日誌
{: #1811_1509}

<table summary="自 1.8.8_1507 版以來進行的變更">
<caption>自 1.8.8_1507 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>1.8.8 版</td>
<td>1.8.11	版</td>
<td><p>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11)。此版本處理 [CVE-2017-1002101 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) 及 [CVE-2017-1002102 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) 漏洞。</p><p>現在，`secret`、`configMap`、`downwardAPI` 及預期的磁區已裝載為唯讀磁區。先前，應用程式可以將資料寫入至這些磁區，但系統可以自動回復資料。如果您的應用程式依賴先前不安全的行為，請相應地修改它們。</p></td>
</tr>
<tr>
<td>暫停容器映像檔</td>
<td>3.0</td>
<td>3.1</td>
<td>移除已繼承的孤立已休眠處理程序。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.8.8-86 版</td>
<td>1.8.11-126 版</td>
<td>`NodePort` 及 `LoadBalancer` 服務現在支援[保留用戶端來源 IP](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)，方法為將 `service.spec.externalTrafficPolicy` 設為 `Local`。</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>針對較舊叢集，修正[邊緣節點](/docs/containers?topic=containers-edge#edge)容忍設定。</td>
</tr>
</tbody>
</table>

<br />


### 1.7 版變更日誌（不受支援）
{: #17_changelog}

檢閱 1.7 版變更日誌。
{: shortdesc}

*   [工作者節點修正套件 1.7.16_1514（2018 年 6 月 11 日發行）的變更日誌](#1716_1514)
*   [工作者節點修正套件 1.7.16_1513（2018 年 5 月 18 日發行）的變更日誌](#1716_1513)
*   [工作者節點修正套件 1.7.16_1512（2018 年 5 月 16 日發行）的變更日誌](#1716_1512)
*   [1.7.16_1511（2018 年 4 月 19 日發行）的變更日誌](#1716_1511)

#### 工作者節點修正套件 1.7.16_1514（2018 年 6 月 11 日發行）的變更日誌
{: #1716_1514}

<table summary="自 1.7.16_1513 版以來進行的變更">
<caption>自 1.7.16_1513 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>核心更新</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>新的工作者節點映像檔，其含有 [CVE-2018-3639 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639) 的核心更新。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.7.16_1513（2018 年 5 月 18 日發行）的變更日誌
{: #1716_1513}

<table summary="自 1.7.16_1512 版以來進行的變更">
<caption>自 1.7.16_1512 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>修正以處理您使用區塊儲存空間外掛程式時發生的錯誤。</td>
</tr>
</tbody>
</table>

#### 工作者節點修正套件 1.7.16_1512（2018 年 5 月 16 日發行）的變更日誌
{: #1716_1512}

<table summary="自 1.7.16_1511 版以來進行的變更">
<caption>自 1.7.16_1511 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>您在 `kubelet` 根目錄中儲存的資料，現在儲存在工作者節點機器的較大次要磁碟上。</td>
</tr>
</tbody>
</table>

#### 1.7.16_1511（2018 年 4 月 19 日發行）的變更日誌
{: #1716_1511}

<table summary="自 1.7.4_1509 版以來進行的變更">
<caption>自 1.7.4_1509 版以來的變更</caption>
<thead>
<tr>
<th>元件</th>
<th>舊版</th>
<th>現行版本</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>1.7.4 版</td>
<td>1.7.16 版	</td>
<td><p>請參閱 [Kubernetes 版本注意事項 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16)。此版本處理 [CVE-2017-1002101 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) 及 [CVE-2017-1002102 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) 漏洞。</p><p>現在，`secret`、`configMap`、`downwardAPI` 及預期的磁區已裝載為唯讀磁區。先前，應用程式可以將資料寫入至這些磁區，但系統可以自動回復資料。如果您的應用程式依賴先前不安全的行為，請相應地修改它們。</p></td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>1.7.4-133 版</td>
<td>1.7.16-17 版</td>
<td>`NodePort` 及 `LoadBalancer` 服務現在支援[保留用戶端來源 IP](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)，方法為將 `service.spec.externalTrafficPolicy` 設為 `Local`。</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>針對較舊叢集，修正[邊緣節點](/docs/containers?topic=containers-edge#edge)容忍設定。</td>
</tr>
</tbody>
</table>
