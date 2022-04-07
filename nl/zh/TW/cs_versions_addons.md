---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, nginx, ingress controller

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


# Fluentd 和 Ingress ALB 變更日誌
{: #cluster-add-ons-changelog}

{{site.data.keyword.containerlong}} 叢集隨附 IBM 自動更新的元件，例如 Fluentd 和 Ingress ALB 元件。您還可以停用某些元件的自動更新，而個別從主節點和工作者節點手動更新這些元件。請參閱下列各節中的表格，以取得每一個版本的變更摘要。
{: shortdesc}

如需管理 Fluentd 和 Ingress ALB 的更新的相關資訊，請參閱[更新叢集元件](/docs/containers?topic=containers-update#components)。

## Ingress ALB 變更日誌
{: #alb_changelog}

檢視 {{site.data.keyword.containerlong_notm}} 叢集裡 Ingress 應用程式負載平衡器 (ALB) 的建置版本變更。
{:shortdesc}

更新 Ingress ALB 元件時，所有 ALB Pod 中的 `nginx-ingress` 和 `ingress-auth` 容器都會更新至最新的建置版本。依預設，會啟用自動更新元件，但您可以停用自動更新並手動更新元件。如需相關資訊，請參閱[更新 Ingress 應用程式負載平衡器](/docs/containers?topic=containers-update#alb)。

請參閱下表格以瞭解 Ingress ALB 元件的每個建置的變更摘要。

<table summary="Ingress 應用程式負載平衡器元件的建置變更概觀">
<caption>Ingress 應用程式負載平衡器元件的變更日誌</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>`nginx-ingress` / `ingress-auth` 建置</th>
<th>發行日期</th>
<th>連續的變更</th>
<th>非連續的變更</th>
</tr>
</thead>
<tbody>
<tr>
<td>470 / 330</td>
<td>2019 年 6 月 7 日</td>
<td>已修正 [CVE-2019-8457 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457) 的 Berkeley DB 漏洞。</td>
<td>-</td>
</tr>
<tr>
<td>470 / 329</td>
<td>2019 年 6 月 6 日</td>
<td>已修正 [CVE-2019-8457 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457) 的 Berkeley DB 漏洞。</td>
<td>-</td>
</tr>
<tr>
<td>467 / 329</td>
<td>2019 年 6 月 3 日</td>
<td>針對 [CVE-2019-3829 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-3893 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3893)、[CVE-2018-10844 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10845 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844) 和 [CVE-2018-10846 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)，修正 GnuTLS 漏洞。
</td>
<td>-</td>
</tr>
<tr>
<td>462 / 329</td>
<td>2019 年 5 月 28 日</td>
<td>針對 [CVE-2019-5435 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，修正 cURL 漏洞。</td>
<td>-</td>
</tr>
<tr>
<td>457 / 329</td>
<td>2019 年 5 月 23 日</td>
<td>針對映像檔掃描，修正 Go 漏洞。</td>
<td>-</td>
</tr>
<tr>
<td>423 / 329</td>
<td>2019 年 5 月 13 日</td>
<td>提高了與 {{site.data.keyword.appid_full}} 整合的效能。</td>
<td>-</td>
</tr>
<tr>
<td>411 / 315</td>
<td>2019 年 4 月 15 日</td>
<td>更新 {{site.data.keyword.appid_full_notm}} Cookie 有效期限的值，讓它符合存取記號有效期限的值。</td>
<td>-</td>
</tr>
<tr>
<td>411 / 306</td>
<td>2019 年 3 月 22 日</td>
<td>將 Go 版本更新為 1.12.1。</td>
<td>-</td>
</tr>
<tr>
<td>410 / 305</td>
<td>2019 年 3 月 18 日</td>
<td><ul>
<li>修正映像檔掃描的漏洞。</li>
<li>改善與 {{site.data.keyword.appid_full_notm}} 整合的記載。</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>408 / 304</td>
<td>2019 年 3 月 05 日</td>
<td>-</td>
<td>修正與登出功能、記號到期和 `OAuth` 授權回呼相關的授權整合中的錯誤。只有當您使用 [`appid-auth`](/docs/containers?topic=containers-ingress_annotation#appid-auth) 註釋來啟用 {{site.data.keyword.appid_full_notm}} 授權時，才會實作這些修正程式。若要實作這些修正程式，則會新增其他標頭，這會增加標頭大小總計。視您自己的標頭大小及回應大小總計而定，您可能需要調整所使用的任何 [Proxy 緩衝區註釋](/docs/containers?topic=containers-ingress_annotation#proxy-buffer)。</td>
</tr>
<tr>
<td>406 / 301</td>
<td>2019 年 2 月 19 日</td>
<td><ul>
<li>將 Go 版本更新為 1.11.5。</li>
<li>修正映像檔掃描的漏洞。</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>404 / 300</td>
<td>2019 年 1 月 31 日</td>
<td>將 Go 版本更新為 1.11.4。</td>
<td>-</td>
</tr>
<tr>
<td>403 / 295</td>
<td>2019 年 1 月 23 日</td>
<td><ul>
<li>將 ALB 的 NGINX 版本更新為 1.15.2。</li>
<li>IBM 提供的 TLS 憑證現在會在到期前 37 天（而非 7 天）自動更新。</li>
<li>新增 {{site.data.keyword.appid_full_notm}} 登出功能：如果 {{site.data.keyword.appid_full_notm}} 路徑中有 `/logout` 字首，則會移除 Cookie，且會將使用者傳送回登入頁面。</li>
<li>將標頭新增至 {{site.data.keyword.appid_full_notm}} 要求，以進行內部追蹤。</li>
<li>更新了 {{site.data.keyword.appid_short_notm}} 位置指引，以便 `app-id` 註釋可與 `proxy-buffers`、`proxy-buffer-size` 和 `proxy-busy-buffer-size` 註釋一起使用。</li>
<li>修正錯誤，讓參考資訊日誌不會被標示為錯誤。</li>
</ul></td>
<td>依預設，停用 TLS 1.0 及 1.1。如果連接至您應用程式的用戶端支援 TLS 1.2，則不需要採取任何動作。如果您仍然有需要 TLS 1.0 或 1.1 支援的舊版用戶端，請遵循[下列步驟](/docs/containers?topic=containers-ingress#ssl_protocols_ciphers)，手動啟用所需的 TLS 版本。如需如何查看您用戶端使用哪個 TLS 版本來存取您應用程式的相關資訊，請參閱此 [{{site.data.keyword.Bluemix_notm}} 部落格文章](https://www.ibm.com/blogs/bluemix/2018/11/ibm-cloud-kubernetes-service-alb-update-tls-1-0-and-1-1-disabled-by-default/)。</td>
</tr>
<tr>
<td>393 / 291</td>
<td>2019 年 1 月 09 日</td>
<td>新增與多個 {{site.data.keyword.appid_full_notm}} 實例整合的支援。</td>
<td>-</td>
</tr>
<tr>
<td>393 / 282</td>
<td>2018 年 11 月 29 日</td>
<td>提高與 {{site.data.keyword.appid_full_notm}} 整合的效能。</td>
<td>-</td>
</tr>
<tr>
<td>384 / 246</td>
<td>2018 年 11 月 14 日</td>
<td>改善與 {{site.data.keyword.appid_full_notm}} 整合的記載和登出特性。</td>
<td>將 `*.containers.mybluemix.net` 的自簽憑證取代為自動產生並供叢集使用的 LetsEncrypt 簽署憑證。`*.containers.mybluemix.net` 自簽憑證會被移除。</td>
</tr>
<tr>
<td>350 / 192</td>
<td>2018 年 11 月 5 日</td>
<td>新增對啟用和停用 Ingress ALB 元件自動更新的支援。</td>
<td>-</td>
</tr>
</tbody>
</table>

## 用於記載的 Fluentd 的變更日誌
{: #fluentd_changelog}

檢視在 {{site.data.keyword.containerlong_notm}} 叢集裡用於記載的 Fluentd 元件的建置版本變更。
{:shortdesc}

依預設，會啟用自動更新元件，但您可以停用自動更新並手動更新元件。如需相關資訊，請參閱[管理 Fluentd 的自動更新](/docs/containers?topic=containers-update#logging-up)。

請參閱下表格以瞭解 Fluentd 元件的每個建置的變更摘要。

<table summary="Fluentd 元件的建置變更概觀">
<caption>Fluentd 元件的變更日誌</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>Fluentd 建置</th>
<th>發行日期</th>
<th>連續的變更</th>
<th>非連續的變更</th>
</tr>
</thead>
<tr>
<td>e7c10d74350dc64d4d92ba7f72bb4ff9219315d2</td>
<td>2019 年 5 月 30 日</td>
<td>更新了 Fluentd 配置對映，以始終忽略 IBM 名稱空間中的 Pod 日誌，即使 Kubernetes 主節點無法使用時也是如此。</td>
<td>-</td>
</tr>
<tr>
<td>c16fe1602ab65db4af0a6ac008f99ca2a526e6f6</td>
<td>2019 年 5 月 21 日</td>
<td>修正工作者節點度量不顯示的錯誤。</td>
<td>-</td>
</tr>
<tr>
<td>60fc11f7bd39d9c6cfed923c598bf6457b3f2037</td>
<td>2019 年 5 月 10 日</td>
<td>已更新 [CVE-2019-8320 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320)、[CVE-2019-8321 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321)、[CVE-2019-8322 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322)、[CVE-2019-8323 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323)、[CVE-2019-8324 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) 及 [CVE-2019-8325 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325) 的 Ruby 套件。</td>
<td>-</td>
</tr>
<tr>
<td>91a737f68f7d9e81b5d2223c910aaa7d7f91b76d</td>
<td>2019 年 5 月 8 日</td>
<td>已更新 [CVE-2019-8320 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320)、[CVE-2019-8321 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321)、[CVE-2019-8322 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322)、[CVE-2019-8323 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323)、[CVE-2019-8324 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) 及 [CVE-2019-8325 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325) 的 Ruby 套件。</td>
<td>-</td>
</tr>
<tr>
<td>d9af69e286986a05ed4a50469585b1cf978ddb1d</td>
<td>2019 年 4 月 11 日</td>
<td>將 cAdvisor 外掛程式更新為使用 TLS 1.2。</td>
<td>-</td>
</tr>
<tr>
<td>3100ddb62580a9f46ffdff7bab2ebec40b164de6</td>
<td>2019 年 4 月 1 日</td>
<td>更新了 Fluentd 服務帳戶。</td>
<td>-</td>
</tr>
<tr>
<td>c85567b75bd7ad1c9428794cd63a8e239c3fd8f5</td>
<td>2019 年 3 月 18 日</td>
<td>針對 [CVE-2019-8323 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323)，移除了對 cURL 的相依關係。</td>
<td>-</td>
</tr>
<tr>
<td>320ffdf87de068ee2f7f34c0e7a47a111e8d457b</td>
<td>2019 年 2 月 18 日</td>
<td><ul>
<li>將 Fluend 更新為 1.3 版。</li>
<li>針對 [CVE-2018-19486 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19486)，從 Fluentd 映像檔中移除了 Git。</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>972865196aefd3324105087878de12c518ed579f</td>
<td>2019 年 1 月 1 日</td>
<td><ul>
<li>對 Fluentd `in_tail` 外掛程式啟用了 UTF-8 編碼。</li>
<li>次要錯誤修正程式。</li>
</ul></td>
<td>-</td>
</tr>
</tbody>
</table>
