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


# Fluentd 和 Ingress ALB 更改日志
{: #cluster-add-ons-changelog}

{{site.data.keyword.containerlong}} 集群随附 IBM 自动更新的组件，例如 Fluentd 和 Ingress ALB 组件。您还可以禁用某些组件的自动更新，而单独从主节点和工作程序节点手动更新这些组件。请参阅以下各部分中的表以获取每个版本的更改摘要。
{: shortdesc}

有关管理 Fluentd 和 Ingress ALB 的更新的更多信息，请参阅[更新集群组件](/docs/containers?topic=containers-update#components)。

## Ingress ALB 更改日志
{: #alb_changelog}

查看 {{site.data.keyword.containerlong_notm}} 集群中 Ingress 应用程序负载均衡器 (ALB) 的构建版本更改。
{:shortdesc}

更新 Ingress ALB 组件时，所有 ALB pod 中的 `nginx-ingress` 和 `ingress-auth` 容器都会更新为最新的构建版本。缺省情况下，会启用组件的自动更新，但您可以禁用自动更新，而手动更新组件。有关更多信息，请参阅[更新 Ingress 应用程序负载均衡器](/docs/containers?topic=containers-update#alb)。

请参阅下表以了解 Ingress ALB 组件的每个构建的更改摘要。

<table summary="Ingress 应用程序负载均衡器组件的构建更改概述">
<caption>Ingress 应用程序负载均衡器组件的更改日志</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>`nginx-ingress` / `ingress-auth` 构建</th>
<th>发布日期</th>
<th>非中断性更改</th>
<th>中断性更改</th>
</tr>
</thead>
<tbody>
<tr>
<td>470 / 330</td>
<td>2019 年 6 月 7 日</td>
<td>针对 [CVE-2019-8457 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457)，修复了 Berkeley DB 漏洞。
</td>
<td>-</td>
</tr>
<tr>
<td>470 / 329</td>
<td>2019 年 6 月 6 日</td>
<td>针对 [CVE-2019-8457 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457)，修复了 Berkeley DB 漏洞。
</td>
<td>-</td>
</tr>
<tr>
<td>467 / 329</td>
<td>2019 年 6 月 3 日</td>
<td>针对 [CVE-2019-3829 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-3893 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3893)、[CVE-2018-10844 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10845 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844) 和 [CVE-2018-10846 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)，修复了 GnuTLS 漏洞。
</td>
<td>-</td>
</tr>
<tr>
<td>462 / 329</td>
<td>2019 年 5 月 28 日</td>
<td>针对 [CVE-2019-5435 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 和 [CVE-2019-5436 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)，修复了 cURL 漏洞。</td>
<td>-</td>
</tr>
<tr>
<td>457 / 329</td>
<td>2019 年 5 月 23 日</td>
<td>针对映像扫描，修复了 Go 漏洞。</td>
<td>-</td>
</tr>
<tr>
<td>423 / 329</td>
<td>2019 年 5 月 13 日</td>
<td>提高了与 {{site.data.keyword.appid_full}} 集成的性能。</td>
<td>-</td>
</tr>
<tr>
<td>411 / 315</td>
<td>2019 年 4 月 15 日</td>
<td>更新了 {{site.data.keyword.appid_full_notm}} cookie 到期时间的值，使其与访问令牌到期时间的值相匹配。</td>
<td>-</td>
</tr>
<tr>
<td>411 / 306</td>
<td>2019 年 3 月 22 日</td>
<td>将 Go 版本更新为 1.12.1。</td>
<td>-</td>
</tr>
<tr>
<td>410 / 305</td>
<td>2019 年 3 月 18 日</td>
<td><ul>
<li>修复了映像扫描的漏洞。</li>
<li>改进了针对与 {{site.data.keyword.appid_full_notm}} 集成的登录功能。</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>408 / 304</td>
<td>2019 年 3 月 5 日</td>
<td>-</td>
<td>修复了与注销功能、令牌到期和 `OAuth` 授权回调相关的授权集成中的错误。仅当使用 [`appid-auth`](/docs/containers?topic=containers-ingress_annotation#appid-auth) 注释启用了 {{site.data.keyword.appid_full_notm}} 授权时，才会实现这些修订。要实现这些修订，将添加其他头，这会增加头的总大小。根据您自己的头大小以及响应的总大小，可能需要调整使用的任何[代理缓冲区注释](/docs/containers?topic=containers-ingress_annotation#proxy-buffer)。</td>
</tr>
<tr>
<td>406 / 301</td>
<td>2019 年 2 月 19 日</td>
<td><ul>
<li>将 Go 版本更新为 1.11.5。</li>
<li>修复了映像扫描的漏洞。</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>404 / 300</td>
<td>2019 年 1 月 31 日</td>
<td>将 Go 版本更新为 1.11.4。</td>
<td>-</td>
</tr>
<tr>
<td>403 / 295</td>
<td>2019 年 1 月 23 日</td>
<td><ul>
<li>将 NGINX 版本的 ALB 更新为 1.15.2。</li>
<li>现在，IBM 提供的 TLS 证书在到期前 37 天（而不是 7 天）会自动更新。</li>
<li>添加了 {{site.data.keyword.appid_full_notm}} 注销功能：如果 {{site.data.keyword.appid_full_notm}} 路径中存在 `/logout` 前缀，那么将除去 cookie 并将用户发送回登录页面。</li>
<li>向 {{site.data.keyword.appid_full_notm}} 请求添加了一个头，以用于内部跟踪。</li>
<li>更新了 {{site.data.keyword.appid_short_notm}} 位置伪指令，以便 `app-id` 注释可与 `proxy-buffers`、`proxy-buffer-size` 和 `proxy-busy-buffer-size` 注释一起使用。</li>
<li>修复了错误，使得参考日志不会标记为错误。</li>
</ul></td>
<td>缺省情况下已禁用 TLS 1.0 和 1.1。如果连接到应用程序的客户机支持 TLS 1.2，那么无需任何操作。如果仍然有需要 TLS 1.0 或 1.1 支持的旧客户机，那么必须通过执行[这些步骤](/docs/containers?topic=containers-ingress#ssl_protocols_ciphers)来启用所需的 TLS 版本。有关如何查看客户机用于访问应用程序的 TLS 版本的更多信息，请参阅此 [{{site.data.keyword.Bluemix_notm}} 博客帖子](https://www.ibm.com/blogs/bluemix/2018/11/ibm-cloud-kubernetes-service-alb-update-tls-1-0-and-1-1-disabled-by-default/)。</td>
</tr>
<tr>
<td>393 / 291</td>
<td>2019 年 1 月 9 日</td>
<td>添加了针对与多个 {{site.data.keyword.appid_full_notm}} 实例集成的支持。</td>
<td>-</td>
</tr>
<tr>
<td>393 / 282</td>
<td>2018 年 11 月 29 日</td>
<td>提高了与 {{site.data.keyword.appid_full_notm}} 集成的性能。</td>
<td>-</td>
</tr>
<tr>
<td>384 / 246</td>
<td>2018 年 11 月 14 日</td>
<td>改进了针对与 {{site.data.keyword.appid_full_notm}} 集成的登录和注销功能。</td>
<td>将 `*.containers.mybluemix.net` 的自签名证书替换为集群自动生成并由集群使用的 LetsEncrypt 签名证书。除去了 `*.containers.mybluemix.net` 自签名证书。</td>
</tr>
<tr>
<td>350 / 192</td>
<td>2018 年 11 月 5 日</td>
<td>添加了对启用和禁用 Ingress ALB 组件自动更新的支持。</td>
<td>-</td>
</tr>
</tbody>
</table>

## 用于日志记录的 Fluentd 的更改日志
{: #fluentd_changelog}

查看在 {{site.data.keyword.containerlong_notm}} 集群中用于日志记录的 Fluentd 组件的构建版本更改。
{:shortdesc}

缺省情况下，会启用组件的自动更新，但您可以禁用自动更新，而手动更新组件。有关更多信息，请参阅[管理 Fluentd 的自动更新](/docs/containers?topic=containers-update#logging-up)。

请参阅下表以了解 Fluentd 组件的每个构建的更改摘要。

<table summary="Fluentd 组件的构建更改概述">
<caption>Fluentd 组件的更改日志</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>Fluentd 构建</th>
<th>发布日期</th>
<th>非中断性更改</th>
<th>中断性更改</th>
</tr>
</thead>
<tr>
<td>e7c10d74350dc64d4d92ba7f72bb4ff9219315d2</td>
<td>2019 年 5 月 30 日</td>
<td>更新了 Fluentd 配置映射，以始终忽略 IBM 名称空间中的 pod 日志，即使 Kubernetes 主节点不可用时也是如此。</td>
<td>-</td>
</tr>
<tr>
<td>c16fe1602ab65db4af0a6ac008f99ca2a526e6f6</td>
<td>2019 年 5 月 21 日</td>
<td>修复了工作程序节点度量值不显示的错误。</td>
<td>-</td>
</tr>
<tr>
<td>60fc11f7bd39d9c6cfed923c598bf6457b3f2037</td>
<td>2019 年 5 月 10 日</td>
<td>针对 [CVE-2019-8320 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320)、[CVE-2019-8321 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321)、[CVE-2019-8322 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322)、[CVE-2019-8323 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323)、[CVE-2019-8324 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) 和 [CVE-2019-8325 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325)，更新了 Ruby 包。</td>
<td>-</td>
</tr>
<tr>
<td>91a737f68f7d9e81b5d2223c910aaa7d7f91b76d</td>
<td>2019 年 5 月 8 日</td>
<td>针对 [CVE-2019-8320 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320)、[CVE-2019-8321 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321)、[CVE-2019-8322 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322)、[CVE-2019-8323 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323)、[CVE-2019-8324 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) 和 [CVE-2019-8325 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325)，更新了 Ruby 包。</td>
<td>-</td>
</tr>
<tr>
<td>d9af69e286986a05ed4a50469585b1cf978ddb1d</td>
<td>2019 年 4 月 11 日</td>
<td>将 cAdvisor 插件更新为使用 TLS 1.2。</td>
<td>-</td>
</tr>
<tr>
<td>3100ddb62580a9f46ffdff7bab2ebec40b164de6</td>
<td>2019 年 4 月 1 日</td>
<td>更新了 Fluentd 服务帐户。</td>
<td>-</td>
</tr>
<tr>
<td>c85567b75bd7ad1c9428794cd63a8e239c3fd8f5</td>
<td>2019 年 3 月 18 日</td>
<td>针对 [CVE-2019-8323 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323)，除去了对 cURL 的依赖关系。</td>
<td>-</td>
</tr>
<tr>
<td>320ffdf87de068ee2f7f34c0e7a47a111e8d457b</td>
<td>2019 年 2 月 18 日</td>
<td><ul>
<li>将 Fluend 更新为 V1.3。</li>
<li>针对 [CVE-2018-19486 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19486)，从 Fluentd 映像中除去了 Git。</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>972865196aefd3324105087878de12c518ed579f</td>
<td>2019 年 1 月 1 日</td>
<td><ul>
<li>对 Fluentd `in_tail` 插件启用了 UTF-8 编码。</li>
<li>次要错误修订。</li>
</ul></td>
<td>-</td>
</tr>
</tbody>
</table>
