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


# バージョンの変更ログ
{: #changelog}

{{site.data.keyword.containerlong}} Kubernetes クラスターに利用可能なメジャー、マイナー、パッチの更新に関するバージョン変更の情報を表示します。 変更には、Kubernetes および {{site.data.keyword.Bluemix_notm}} Provider コンポーネントへの更新が含まれます。
{:shortdesc}

変更ログに特に記載がない限り、{{site.data.keyword.containerlong_notm}} プロバイダー・バージョンでは、ベータ版の Kubernetes API および機能が有効です。変更される可能性のある Kubernetes アルファ機能は無効になっています。

メジャー・バージョン、マイナー・バージョン、パッチ・バージョンについてと、マイナー・バージョン間の準備アクションについて詳しくは、[Kubernetes のバージョン](/docs/containers?topic=containers-cs_versions)を参照してください。
{: tip}

前のバージョンからの変更点については、以下の変更ログを参照してください。
-  バージョン 1.14 [変更ログ](#114_changelog)。
-  バージョン 1.13 [変更ログ](#113_changelog)。
-  バージョン 1.12 [変更ログ](#112_changelog)。
-  **非推奨**: バージョン 1.11 [変更ログ](#111_changelog)。
-  非サポートのバージョンの変更ログの[アーカイブ](#changelog_archive)。

一部の変更ログは_ワーカー・ノード・フィックスパック_ に関するものであり、ワーカー・ノードにのみ適用されます。 [これらのパッチを適用して](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)、ワーカー・ノードのセキュリティー・コンプライアンスを確実にする必要があります。 このようなワーカー・ノード・フィックスパックのバージョンはマスターよりも高くなることがあります。ワーカー・ノードに固有のビルド・フィックスパックがあるからです。 他の変更ログは_マスター・フィックスパック_ に関するものであり、クラスター・マスターにのみ適用されます。 マスターのフィックスパックは自動的に適用されない場合があります。 [これらを手動で適用](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)することもできます。 パッチのタイプについて詳しくは、[更新タイプ](/docs/containers?topic=containers-cs_versions#update_types)を参照してください。
{: note}

</br>

## バージョン 1.14 変更ログ
{: #114_changelog}

### 1.14.2_1521 (2019 年 6 月 4 日リリース) の変更ログ
{: #1142_1521}

以下の表に、パッチ 1.14.2_1521 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.14.1_1519 からの変更点">
<caption>バージョン 1.14.1_1519 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター DNS 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスターの `create` 操作または `update` 操作の後に Kubernetes DNS ポッドと CoreDNS ポッドの両方が実行されたままになることがあるバグが修正されました。</td>
</tr>
<tr>
<td>クラスター・マスター HA 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>マスター更新時に断続的に発生するマスター・ネットワーク接続障害を最小限に抑えるように構成が更新されました。</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>[etcd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coreos/etcd/releases/v3.3.13) を参照してください。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>[CVE-2018-10844 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435)、および [CVE-2019-5436 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.14.1-71</td>
<td>v1.14.2-100</td>
<td>Kubernetes 1.14.2 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.14.1</td>
<td>v1.14.2</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.2) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>[Kubernetes Metrics Server リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3) を参照してください。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>[CVE-2018-10844 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435)、および [CVE-2019-5436 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.14.1_1519 (2019 年 5 月 20 日リリース) の変更ログ
{: #1141_1519}

以下の表に、パッチ 1.14.1_1519 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.14.1_1518 からの変更点">
<caption>バージョン 1.14.1_1518 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 カーネル</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>[CVE-2018-12126 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html)、および [CVE-2018-12130 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>Ubuntu 18.04 カーネル</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>[CVE-2018-12126 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html)、および [CVE-2018-12130 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

### 1.14.1_1518 (2019 年 5 月 13 日リリース) の変更ログ
{: #1141_1518}

以下の表に、パッチ 1.14.1_1518 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.14.1_1516 からの変更点">
<caption>バージョン 1.14.1_1516 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター HA プロキシー</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>[HAProxy リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.haproxy.org/download/1.9/src/CHANGELOG) を参照してください。 更新により、[CVE-2019-6706 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706) が解決されます。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>Kubernetes API サーバーの監査ポリシー構成が更新され、`/openapi/v2*` 読み取り専用 URL がログに記録されなくなりました。さらに、Kubernetes コントローラー・マネージャーの構成により、署名済み `kubelet` 証明書の有効期間が 1 年から 3 年に延長されました。</td>
</tr>
<tr>
<td>OpenVPN クライアント構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kube-system` 名前空間の OpenVPN クライアントの `vpn-*` ポッドについて、クラスター DNS がダウンしたときにポッドが失敗しないように `dnsPolicy` が `Default` に設定されるようになりました。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>e7182c7</td>
<td>13c7ef0</td>
<td>[CVE-2016-7076 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076) および [CVE-2017-1000368 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### 1.14.1_1516 (2019 年 5 月 7 日リリース) の変更ログ
{: #1141_1516}

以下の表に、パッチ 1.14.1_1516 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.13.5_1519 からの変更点">
<caption>バージョン 1.13.5_1519 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.4.4</td>
<td>v3.6.1</td>
<td>[Calico リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.6/release-notes/) を参照してください。</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.6</td>
<td>1.3.1</td>
<td>[CoreDNS リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://coredns.io/2019/01/13/coredns-1.3.1-release/) を参照してください。 更新には、クラスター DNS サービスの[メトリック・ポート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://coredns.io/plugins/metrics/) の追加が含まれます。<br><br>CoreDNS がサポートされる唯一のクラスター DNS プロバイダーになりました。クラスターを旧バージョンから Kubernetes バージョン 1.14 に更新し、KubeDNS を使用していた場合、KubeDNS はクラスター更新時に自動的に CoreDNS に移行されます。詳細および更新前に CoreDNS をテストする方法については、[クラスター DNS プロバイダーの構成](https://cloud.ibm.com/docs/containers?topic=containers-cluster_dns#cluster_dns)を参照してください。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>9ff3fda</td>
<td>ed0dafc</td>
<td>[CVE-2019-1543 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.5-107</td>
<td>v1.14.1-71</td>
<td>Kubernetes 1.14.1 リリースをサポートするように更新されました。さらに、`calicoctl` バージョンが 3.6.1 に更新されました。ロード・バランサー・ポッドに使用可能なワーカー・ノードが 1 つのみのバージョン 2.0 ロード・バランサーに対する更新が修正されました。プライベート・ロード・バランサーは、[プライベート・エッジ・ワーカー・ノード](/docs/containers?topic=containers-edge#edge)での実行をサポートするようになりました。</td>
</tr>
<tr>
<td>IBM ポッド・セキュリティー・ポリシー</td>
<td>該当なし</td>
<td>該当なし</td>
<td>Kubernetes [RunAsGroup ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#users-and-groups) 機能をサポートするように [IBM ポッド・セキュリティー・ポリシー](/docs/containers?topic=containers-psp#ibm_psp)が更新されました。</td>
</tr>
<tr>
<td>`kubelet` 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>1 つのポッドでワーカー・ノード上のすべてのプロセス ID が消費されないように、`--pod-max-pids` オプションが `14336` に設定されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.14.1</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.1) および [Kubernetes 1.14 のブログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/blog/2019/03/25/kubernetes-1-14-release-announcement/) を参照してください。<br><br>Kubernetes のデフォルトの役割ベースのアクセス制御 (RBAC) ポリシーで、[非認証ユーザーに対するディスカバリー API および許可確認 API ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles) へのアクセスが認可されなくなりました。この変更は、新規バージョン 1.14 のクラスターにのみ適用されます。以前のバージョンからクラスターを更新した場合、非認証ユーザーはディスカバリー API および許可確認 API に引き続きアクセスできます。</td>
</tr>
<tr>
<td>Kubernetes アドミッション・コントローラーの構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td><ul>
<li>クラスターの Kubernetes API サーバーの `--enable-admission-plugins` オプションに `NodeRestriction` が追加され、このセキュリティー強化をサポートするようにクラスター・リソースが構成されました。</li>
<li>クラスターの Kubernetes API サーバーについて、`--enable-admission-plugins` オプションから `Initializers` が削除され、`--runtime-config` オプションから `admissionregistration.k8s.io/v1alpha1=true` が削除されました。これらの API がサポートされなくなったためです。代わりに、[Kubernetes アドミッション Webhook ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/) を使用できます。</li></ul></td>
</tr>
<tr>
<td>Kubernetes DNS autoscaler</td>
<td>1.3.0</td>
<td>1.4.0</td>
<td>[Kubernetes DNS autoscaler リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.4.0) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 機能ゲートの構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td><ul>
  <li>コンテナー・ランタイム構成の選択を無効にするために、`RuntimeClass=false` が追加されました。</li>
  <li>`scheduler.alpha.kubernetes.io/critical-pod` ポッド・アノテーションがサポートされなくなったため、`ExperimentalCriticalPodAnnotation=true` が削除されました。代わりに、[Kubernetes のポッドの優先度 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/docs/containers?topic=containers-pod_priority#pod_priority) を使用できます。</li></ul></td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>e132aa4</td>
<td>e7182c7</td>
<td>[CVE-2019-11068 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

<br />


## バージョン 1.13 変更ログ
{: #113_changelog}

### 1.13.6_1524 (2019 年 6 月 4 日リリース) の変更ログ
{: #1136_1524}

以下の表に、パッチ 1.13.6_1524 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.13.6_1522 からの変更点">
<caption>バージョン 1.13.6_1522 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター DNS 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスターの `create` 操作または `update` 操作の後に Kubernetes DNS ポッドと CoreDNS ポッドの両方が実行されたままになることがあるバグが修正されました。</td>
</tr>
<tr>
<td>クラスター・マスター HA 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>マスター更新時に断続的に発生するマスター・ネットワーク接続障害を最小限に抑えるように構成が更新されました。</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>[etcd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coreos/etcd/releases/v3.3.13) を参照してください。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>[CVE-2018-10844 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435)、および [CVE-2019-5436 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>[Kubernetes Metrics Server リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3) を参照してください。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>[CVE-2018-10844 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435)、および [CVE-2019-5436 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.13.6_1522 (2019 年 5 月 20 日リリース) の変更ログ
{: #1136_1522}

以下の表に、パッチ 1.13.6_1522 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.13.6_1521 からの変更点">
<caption>バージョン 1.13.6_1521 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 カーネル</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>[CVE-2018-12126 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html)、および [CVE-2018-12130 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>Ubuntu 18.04 カーネル</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>[CVE-2018-12126 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html)、および [CVE-2018-12130 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

### 1.13.6_1521 (2019 年 5 月 13 日リリース) の変更ログ
{: #1136_1521}

以下の表に、パッチ 1.13.6_1521 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.13.5_1519 からの変更点">
<caption>バージョン 1.13.5_1519 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター HA プロキシー</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>[HAProxy リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.haproxy.org/download/1.9/src/CHANGELOG) を参照してください。 更新により、[CVE-2019-6706 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706) が解決されます。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>[CVE-2019-1543 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.5-107</td>
<td>v1.13.6-139</td>
<td>Kubernetes 1.13.6 リリースをサポートするように更新されました。ロード・バランサー・ポッドに使用可能なワーカー・ノードが 1 つのみのバージョン 2.0 ロード・バランサーに対する更新プロセスが修正されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.13.6</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.6) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>Kubernetes API サーバーの監査ポリシー構成が更新され、`/openapi/v2*` 読み取り専用 URL がログに記録されなくなりました。さらに、Kubernetes コントローラー・マネージャーの構成により、署名済み `kubelet` 証明書の有効期間が 1 年から 3 年に延長されました。</td>
</tr>
<tr>
<td>OpenVPN クライアント構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kube-system` 名前空間の OpenVPN クライアントの `vpn-*` ポッドについて、クラスター DNS がダウンしたときにポッドが失敗しないように `dnsPolicy` が `Default` に設定されるようになりました。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>[CVE-2016-7076 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076)、[CVE-2017-1000368 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368)、および [CVE-2019-11068 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.13.5_1519 (2019 年 4 月 29 日リリース) の変更ログ
{: #1135_1519}

以下の表に、ワーカー・ノードのフィックスパック 1.13.5_1519 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.13.5_1518 からの変更点">
<caption>バージョン 1.13.5_1518 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>インストール済み Ubuntu パッケージが更新されました。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.5</td>
<td>1.2.6</td>
<td>[containerd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.2.6) を参照してください。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.13.5_1518 (2019 年 4 月 15 日リリース) の変更ログ
{: #1135_1518}

以下の表に、ワーカー・ノードのフィックスパック 1.13.5_1518 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.13.5_1517 からの変更点">
<caption>バージョン 1.13.5_1517 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>[CVE-2019-3842 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html) の `systemd` を含むインストール済み Ubuntu パッケージの更新。</td>
</tr>
</tbody>
</table>

### 1.13.5_1517 (2019 年 4 月 8 日リリース) の変更ログ
{: #1135_1517}

以下の表に、パッチ 1.13.5_1517 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.13.4_1516 からの変更点">
<caption>バージョン 1.13.4_1516 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.4.0</td>
<td>v3.4.4</td>
<td>[Calico リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.4/releases/#v344) を参照してください。 更新により、[CVE-2019-9946 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946) が解決されます。</td>
</tr>
<tr>
<td>クラスター・マスター HA プロキシー</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>[HAProxy リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.haproxy.org/download/1.9/src/CHANGELOG) を参照してください。 更新により、[CVE-2018-0732 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732)、[CVE-2018-0734 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734)、[CVE-2018-0737 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)、[CVE-2018-5407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)、[CVE-2019-1543 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543)、および [CVE-2019-1559 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) が解決されます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.4-86</td>
<td>v1.13.5-107</td>
<td>Kubernetes 1.13.5 リリースおよび Calico 3.4.4 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.4</td>
<td>v1.13.5</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.5) を参照してください。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>[CVE-2017-12447 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>Ubuntu 16.04 カーネル</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>[CVE-2019-9213 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>Ubuntu 18.04 カーネル</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>[CVE-2019-9213 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.13.4_1516 (2019 年 4 月 1 日リリース) の変更ログ
{: #1134_1516}

以下の表に、ワーカー・ノードのフィックスパック 1.13.4_1516 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.13.4_1515 からの変更点">
<caption>バージョン 1.13.4_1515 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>ワーカー・ノードのリソースの使用方法</td>
<td>該当なし</td>
<td>該当なし</td>
<td>kubelet と containerd がリソース不足にならないように、これらのコンポーネント用のメモリー予約が増加されました。 詳しくは、[ワーカー・ノードのリソースの予約](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)を参照してください。</td>
</tr>
</tbody>
</table>

### マスター・フィックスパック 1.13.4_1515 (2019 年 3 月 26 日リリース) の変更ログ
{: #1134_1515}

以下の表に、マスターのフィックスパック 1.13.4_1515 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.13.4_1513 からの変更点">
<caption>バージョン 1.13.4_1513 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター DNS 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスター DNS プロバイダーの CoreDNS への切り替えによって更新されないように、Kubernetes バージョン 1.11 からの更新プロセスが修正されました。 引き続き更新後に、[CoreDNS をクラスター DNS プロバイダーとしてセットアップ](/docs/containers?topic=containers-cluster_dns#set_coredns)できます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>345</td>
<td>346</td>
<td>[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>166</td>
<td>167</td>
<td>Kubernetes シークレットを管理するための `context deadline exceeded` および `timeout` で断続的に発生するエラーが修正されました。 また、既存の Kubernetes シークレットが暗号化されないままになることがある鍵管理サービスの更新も修正されました。 更新には、[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) の修正が含まれます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} プロバイダー用のロード・バランサーとロード・バランサー・モニター</td>
<td>143</td>
<td>146</td>
<td>[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### 1.13.4_1513 (2019 年 3 月 20 日リリース) の変更ログ
{: #1134_1513}

以下の表に、パッチ 1.13.4_1513 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.13.4_1510 からの変更点">
<caption>バージョン 1.13.4_1510 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター DNS 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>使用されていないクラスター DNS のスケールダウンが必要なときに`リフレッシュ`や`更新`などのクラスター・マスター操作が失敗するバグが修正されました。</td>
</tr>
<tr>
<td>クラスター・マスター HA プロキシー構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>断続的に発生するクラスター・マスターへの接続エラーへの対応を向上させるために構成が更新されました。</td>
</tr>
<tr>
<td>CoreDNS 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>1.12 からクラスター Kubernetes バージョンを更新すると、[複数の Corefile ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://coredns.io/2017/07/23/corefile-explained/) をサポートするように CoreDNS 構成が更新されます。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.4</td>
<td>1.2.5</td>
<td>[containerd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.2.5) を参照してください。 更新には、[CVE-2019-5736 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736) の改善された修正が含まれます。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>GPU ドライバーが [418.43 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.nvidia.com/object/unix.html) に更新されました。 更新には、[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html) の修正が含まれます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>344</td>
<td>345</td>
<td>[プライベート・サービス・エンドポイント](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)のサポートが追加されました。</td>
</tr>
<tr>
<td>カーネル</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>[CVE-2019-6133 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>136</td>
<td>166</td>
<td>[CVE-2018-16890 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890)、[CVE-2019-3822 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822)、および [CVE-2019-3823 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>[CVE-2018-10779 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779)、[CVE-2018-12900 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900)、[CVE-2018-17000 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000)、[CVE-2018-19210 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210)、[CVE-2019-6128 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128)、および [CVE-2019-7663 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### 1.13.4_1510 (2019 年 3 月 4 日リリース) の変更ログ
{: #1134_1510}

以下の表に、パッチ 1.13.4_1510 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.13.2_1509 からの変更点">
<caption>バージョン 1.13.2_1509 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスターの DNS プロバイダー</td>
<td>該当なし</td>
<td>該当なし</td>
<td>より多くのクラスター・サービスを処理するために、Kubernetes DNS および CoreDNS のポッドのメモリー制限が `170 Mi` から `400 Mi` に増えました。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>[CVE-2019-6454 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.13.2-62</td>
<td>v1.13.4-86</td>
<td>Kubernetes 1.13.4 リリースをサポートするように更新されました。 `externalTrafficPolicy` を `local` に設定したバージョン 1.0 のロード・バランサーに定期的に発生していた接続の問題が修正されました。 最新の {{site.data.keyword.Bluemix_notm}} ドキュメント・リンクを使用するようにロード・バランサーのバージョン 1.0 および 2.0 のイベントが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>342</td>
<td>344</td>
<td>イメージの基本オペレーティング・システムが Fedora から Alpine に変更されました。 [CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>122</td>
<td>136</td>
<td>クライアントのタイムアウトが {{site.data.keyword.keymanagementservicefull_notm}} に増やされました。 [CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.2</td>
<td>v1.13.4</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.4) を参照してください。 更新により、[CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) および [CVE-2019-1002100 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100) が解決されます。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`--feature-gates` オプションに `ExperimentalCriticalPodAnnotation=true` が追加されました。 この設定は、ポッドを非推奨の `scheduler.alpha.kubernetes.io/critical-pod` アノテーションから [Kubernetes ポッドの優先度のサポート](/docs/containers?topic=containers-pod_priority#pod_priority)に移行するのに役立ちます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} プロバイダー用のロード・バランサーとロード・バランサー・モニター</td>
<td>132</td>
<td>143</td>
<td>[CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>OpenVPN クライアントとサーバー</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>[CVE-2019-1559 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>[CVE-2019-6454 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.13.2_1509 (2019 年 2 月 27 日リリース) の変更ログ
{: #1132_1509}

以下の表に、ワーカー・ノードのフィックスパック 1.13.2_1509 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.13.2_1508 からの変更点">
<caption>バージョン 1.13.2_1508 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>[CVE-2018-19407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.13.2_1508 (2019 年 2 月 15 日リリース) の変更ログ
{: #1132_1508}

以下の表に、ワーカー・ノードのフィックスパック 1.13.2_1508 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.13.2_1507 からの変更点">
<caption>バージョン 1.13.2_1507 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター HA プロキシー構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ポッド構成値 `spec.priorityClassName` が `system-node-critical` に変更され、`spec.priority` の値が `2000001000` に設定されました。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.2</td>
<td>1.2.4</td>
<td>[containerd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.2.4) を参照してください。 更新により、[CVE-2019-5736 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736) が解決されます。</td>
</tr>
<tr>
<td>Kubernetes の `kubelet` 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`ExperimentalCriticalPodAnnotation` 機能ゲートで静的ポッドの重大な強制除去の発生を防止できるようになりました。 `event-qps`オプションを `0` に設定して、速度を制限するイベント生成を防止できます。</td>
</tr>
</tbody>
</table>

### 1.13.2_1507 (2019 年 2 月 5 日リリース) の変更ログ
{: #1132_1507}

以下の表に、パッチ 1.13.2_1507 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.4_1535 からの変更点">
<caption>バージョン 1.12.4_1535 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.4.0</td>
<td>[Calico リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.4/releases/#v340) を参照してください。</td>
</tr>
<tr>
<td>クラスターの DNS プロバイダー</td>
<td>該当なし</td>
<td>該当なし</td>
<td>新しいクラスターのデフォルトのクラスター DNS プロバイダーは CoreDNS になりました。 KubeDNS をクラスター DNS プロバイダーとして使用している既存のクラスターを 1.13 に更新した場合は、クラスター DNS プロバイダーは KubeDNS のままです。 しかし、[CoreDNS を代わりに使用する](/docs/containers?topic=containers-cluster_dns#dns_set)こともできます。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.2.2</td>
<td>[containerd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.2.2) を参照してください。</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.2</td>
<td>1.2.6</td>
<td>[CoreDNS リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coredns/coredns/releases/tag/v1.2.6) を参照してください。 さらに、[複数の Corefile ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://coredns.io/2017/07/23/corefile-explained/) をサポートするように、CoreDNS 構成が更新されました。</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>[etcd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coreos/etcd/releases/v3.3.11) を参照してください。 さらに、etcd でサポートされる暗号スイートが、強力な暗号化 (128 ビット以上) を使用するサブセットだけに制限されるようになりました。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>[CVE-2019-3462 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) および [CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.4-118</td>
<td>v1.13.2-62</td>
<td>Kubernetes 1.13.2 リリースをサポートするように更新されました。 さらに、`calicoctl` バージョンが 3.4.0 に更新されました。 ワーカー・ノードの状況変化時にバージョン 2.0 ロード・バランサーに対して行われていた不要な構成更新が修正されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>338</td>
<td>342</td>
<td>ファイル・ストレージ・プラグインが次のように更新されました。
<ul><li>[ボリューム・トポロジー対応スケジューリング](/docs/containers?topic=containers-file_storage#file-topology)による動的プロビジョニングをサポートします。</li>
<li>ストレージが既に削除されている場合は、永続ボリューム請求 (PVC) 削除エラーを無視します。</li>
<li>失敗した PVC に失敗メッセージのアノテーションを追加します。</li>
<li>ストレージ・プロビジョナー・コントローラーのリーダー選択と再同期期間の設定を最適化し、プロビジョニング・タイムアウトを 30 分から 1 時間に増やします。</li>
<li>プロビジョニングを開始する前にユーザー許可を確認します。</li>
<li>`kube-system` 名前空間の `ibm-file-plugin` デプロイメントと `ibm-storage-watcher` デプロイメントに `CriticalAddonsOnly` 耐性を追加します。</li></ul></td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>111</td>
<td>122</td>
<td>Kubernetes シークレットが {{site.data.keyword.keymanagementservicefull_notm}} によって管理されている場合の一時的な失敗を回避するための再試行ロジックが追加されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.13.2</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.2) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>Kubernetes API サーバーの監査ポリシー構成が更新され、`cluster-admin` 要求のメタデータのロギングと、ワークロードの`作成`、`更新`、および`パッチ`要求の本体のロギングが含まれるようになりました。</td>
</tr>
<tr>
<td>Kubernetes DNS autoscaler</td>
<td>1.2.0</td>
<td>1.3.0</td>
<td>[Kubernetes DNS autoscaler リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.3.0) を参照してください。</td>
</tr>
<tr>
<td>OpenVPN クライアント</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) および [CVE-2018-5407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) への対策としてイメージが更新されました。 `kube-system` 名前空間の `vpn` デプロイメントに `CriticalAddonsOnly` 耐性が追加されました。 さらに、ポッド構成が構成マップからではなくシークレットから取得されるようになりました。</td>
</tr>
<tr>
<td>OpenVPN サーバー</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) および [CVE-2018-5407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864) のセキュリティー・パッチ。</td>
</tr>
</tbody>
</table>

<br />


## バージョン 1.12 変更ログ
{: #112_changelog}

バージョン 1.12 の変更ログをまとめます。
{: shortdesc}

### 1.12.9_1555 (2019 年 6 月 4 日リリース) の変更ログ
{: #1129_1555}

以下の表に、パッチ 1.12.9_1555 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.8_1553 からの変更点">
<caption>バージョン 1.12.8_1553 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター DNS 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスターの `create` 操作または `update` 操作の後に Kubernetes DNS ポッドと CoreDNS ポッドの両方が実行されたままになることがあるバグが修正されました。</td>
</tr>
<tr>
<td>クラスター・マスター HA 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>マスター更新時に断続的に発生するマスター・ネットワーク接続障害を最小限に抑えるように構成が更新されました。</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>[etcd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coreos/etcd/releases/v3.3.13) を参照してください。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>[CVE-2018-10844 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435)、および [CVE-2019-5436 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.8-210</td>
<td>v1.12.9-227</td>
<td>Kubernetes 1.12.9 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.8</td>
<td>v1.12.9</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.9) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>[Kubernetes Metrics Server リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3) を参照してください。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>[CVE-2018-10844 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435)、および [CVE-2019-5436 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.12.8_1553 (2019 年 5 月 20 日リリース) の変更ログ
{: #1128_1533}

以下の表に、パッチ 1.12.8_1553 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.8_1553 からの変更点">
<caption>バージョン 1.12.8_1553 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 カーネル</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>[CVE-2018-12126 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html)、および [CVE-2018-12130 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>Ubuntu 18.04 カーネル</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>[CVE-2018-12126 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html)、および [CVE-2018-12130 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

### 1.12.8_1552 (2019 年 5 月 13 日リリース) の変更ログ
{: #1128_1552}

以下の表に、パッチ 1.12.8_1552 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.7_1550 からの変更点">
<caption>バージョン 1.12.7_1550 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター HA プロキシー</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>[HAProxy リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.haproxy.org/download/1.9/src/CHANGELOG) を参照してください。 更新により、[CVE-2019-6706 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706) が解決されます。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>[CVE-2019-1543 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.7-180</td>
<td>v1.12.8-210</td>
<td>Kubernetes 1.12.8 リリースをサポートするように更新されました。 ロード・バランサー・ポッドに使用可能なワーカー・ノードが 1 つのみのバージョン 2.0 ロード・バランサーに対する更新プロセスが修正されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.7</td>
<td>v1.12.8</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.8) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>Kubernetes API サーバーの監査ポリシー構成が更新され、`/openapi/v2*` 読み取り専用 URL がログに記録されなくなりました。さらに、Kubernetes コントローラー・マネージャーの構成により、署名済み `kubelet` 証明書の有効期間が 1 年から 3 年に延長されました。</td>
</tr>
<tr>
<td>OpenVPN クライアント構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kube-system` 名前空間の OpenVPN クライアントの `vpn-*` ポッドについて、クラスター DNS がダウンしたときにポッドが失敗しないように `dnsPolicy` が `Default` に設定されるようになりました。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>[CVE-2016-7076 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076)、[CVE-2017-1000368 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368)、および [CVE-2019-11068 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.12.7_1550 (2019 年 4 月 29 日リリース) の変更ログ
{: #1127_1550}

以下の表に、ワーカー・ノードのフィックスパック 1.12.7_1550 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.7_1549 からの変更点">
<caption>バージョン 1.12.7_1549 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>インストール済み Ubuntu パッケージが更新されました。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>[containerd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.1.7) を参照してください。</td>
</tr>
</tbody>
</table>


### ワーカー・ノード・フィックスパック 1.12.7_1549 (2019 年 4 月 15 日リリース) の変更ログ
{: #1127_1549}

以下の表に、ワーカー・ノードのフィックスパック 1.12.7_1549 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.7_1548 からの変更点">
<caption>バージョン 1.12.7_1548 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>[CVE-2019-3842 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html) の `systemd` を含むインストール済み Ubuntu パッケージの更新。</td>
</tr>
</tbody>
</table>

### 1.12.7_1548 (2019 年 4 月 8 日リリース) の変更ログ
{: #1127_1548}

以下の表に、パッチ 1.12.7_1548 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.6_1547 からの変更点">
<caption>バージョン 1.12.6_1547 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>[Calico リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.3/releases/#v336) を参照してください。 更新により、[CVE-2019-9946 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946) が解決されます。</td>
</tr>
<tr>
<td>クラスター・マスター HA プロキシー</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>[HAProxy リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.haproxy.org/download/1.9/src/CHANGELOG) を参照してください。 更新により、[CVE-2018-0732 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732)、[CVE-2018-0734 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734)、[CVE-2018-0737 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)、[CVE-2018-5407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)、[CVE-2019-1543 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543)、および [CVE-2019-1559 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) が解決されます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.6-157</td>
<td>v1.12.7-180</td>
<td>Kubernetes 1.12.7 リリースおよび Calico 3.3.6 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.6</td>
<td>v1.12.7</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.7) を参照してください。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>[CVE-2017-12447 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>Ubuntu 16.04 カーネル</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>[CVE-2019-9213 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>Ubuntu 18.04 カーネル</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>[CVE-2019-9213 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.12.6_1547 (2019 年 4 月 1 日リリース) の変更ログ
{: #1126_1547}

以下の表に、ワーカー・ノードのフィックスパック 1.12.6_1547 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.6_1546 からの変更点">
<caption>バージョン 1.12.6_1546 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>ワーカー・ノードのリソースの使用方法</td>
<td>該当なし</td>
<td>該当なし</td>
<td>kubelet と containerd がリソース不足にならないように、これらのコンポーネント用のメモリー予約が増加されました。 詳しくは、[ワーカー・ノードのリソースの予約](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)を参照してください。</td>
</tr>
</tbody>
</table>


### マスター・フィックスパック 1.12.6_1546 (2019 年 3 月 26 日リリース) の変更ログ
{: #1126_1546}

以下の表に、マスターのフィックスパック 1.12.6_1546 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.6_1544 からの変更点">
<caption>バージョン 1.12.6_1544 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>345</td>
<td>346</td>
<td>[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>166</td>
<td>167</td>
<td>Kubernetes シークレットを管理するための `context deadline exceeded` および `timeout` で断続的に発生するエラーが修正されました。 また、既存の Kubernetes シークレットが暗号化されないままになることがある鍵管理サービスの更新も修正されました。 更新には、[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) の修正が含まれます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} プロバイダー用のロード・バランサーとロード・バランサー・モニター</td>
<td>143</td>
<td>146</td>
<td>[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### 1.12.6_1544 (2019 年 3 月 20 日リリース) の変更ログ
{: #1126_1544}

以下の表に、パッチ 1.12.6_1544 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.6_1541 からの変更点">
<caption>バージョン 1.12.6_1541 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター DNS 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>使用されていないクラスター DNS のスケールダウンが必要なときに`リフレッシュ`や`更新`などのクラスター・マスター操作が失敗するバグが修正されました。</td>
</tr>
<tr>
<td>クラスター・マスター HA プロキシー構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>断続的に発生するクラスター・マスターへの接続エラーへの対応を向上させるために構成が更新されました。</td>
</tr>
<tr>
<td>CoreDNS 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>[複数の Corefile ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://coredns.io/2017/07/23/corefile-explained/) をサポートするように CoreDNS 構成が更新されました。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>GPU ドライバーが [418.43 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.nvidia.com/object/unix.html) に更新されました。 更新には、[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html) の修正が含まれます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>344</td>
<td>345</td>
<td>[プライベート・サービス・エンドポイント](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)のサポートが追加されました。</td>
</tr>
<tr>
<td>カーネル</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>[CVE-2019-6133 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>136</td>
<td>166</td>
<td>[CVE-2018-16890 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890)、[CVE-2019-3822 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822)、および [CVE-2019-3823 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>[CVE-2018-10779 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779)、[CVE-2018-12900 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900)、[CVE-2018-17000 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000)、[CVE-2018-19210 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210)、[CVE-2019-6128 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128)、および [CVE-2019-7663 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### 1.12.6_1541 (2019 年 3 月 4 日リリース) の変更ログ
{: #1126_1541}

以下の表に、パッチ 1.12.6_1541 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.5_1540 からの変更点">
<caption>バージョン 1.12.5_1540 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスターの DNS プロバイダー</td>
<td>該当なし</td>
<td>該当なし</td>
<td>より多くのクラスター・サービスを処理するために、Kubernetes DNS および CoreDNS のポッドのメモリー制限が `170 Mi` から `400 Mi` に増えました。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>[CVE-2019-6454 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.5-137</td>
<td>v1.12.6-157</td>
<td>Kubernetes 1.12.6 リリースをサポートするように更新されました。 `externalTrafficPolicy` を `local` に設定したバージョン 1.0 のロード・バランサーに定期的に発生していた接続の問題が修正されました。 最新の {{site.data.keyword.Bluemix_notm}} ドキュメント・リンクを使用するようにロード・バランサーのバージョン 1.0 および 2.0 のイベントが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>342</td>
<td>344</td>
<td>イメージの基本オペレーティング・システムが Fedora から Alpine に変更されました。 [CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>122</td>
<td>136</td>
<td>クライアントのタイムアウトが {{site.data.keyword.keymanagementservicefull_notm}} に増やされました。 [CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.5</td>
<td>v1.12.6</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.6) を参照してください。 更新により、[CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) および [CVE-2019-1002100 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100) が解決されます。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`--feature-gates` オプションに `ExperimentalCriticalPodAnnotation=true` が追加されました。 この設定は、ポッドを非推奨の `scheduler.alpha.kubernetes.io/critical-pod` アノテーションから [Kubernetes ポッドの優先度のサポート](/docs/containers?topic=containers-pod_priority#pod_priority)に移行するのに役立ちます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} プロバイダー用のロード・バランサーとロード・バランサー・モニター</td>
<td>132</td>
<td>143</td>
<td>[CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>OpenVPN クライアントとサーバー</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>[CVE-2019-1559 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>[CVE-2019-6454 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.12.5_1540 (2019 年 2 月 27 日リリース) の変更ログ
{: #1125_1540}

以下の表に、ワーカー・ノードのフィックスパック 1.12.5_1540 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.5_1538 からの変更点">
<caption>バージョン 1.12.5_1538 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>[CVE-2018-19407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.12.5_1538 (2019 年 2 月 15 日リリース) の変更ログ
{: #1125_1538}

以下の表に、ワーカー・ノードのフィックスパック 1.12.5_1538 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.5_1537 からの変更点">
<caption>バージョン 1.12.5_1537 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター HA プロキシー構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ポッド構成値 `spec.priorityClassName` が `system-node-critical` に変更され、`spec.priority` の値が `2000001000` に設定されました。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>[containerd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.1.6) を参照してください。 更新により、[CVE-2019-5736 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736) が解決されます。</td>
</tr>
<tr>
<td>Kubernetes の `kubelet` 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`ExperimentalCriticalPodAnnotation` 機能ゲートで静的ポッドの重大な強制除去の発生を防止できるようになりました。</td>
</tr>
</tbody>
</table>

### 1.12.5_1537 (2019 年 2 月 5 日リリース) の変更ログ
{: #1125_1537}

以下の表に、パッチ 1.12.5_1537 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.4_1535 からの変更点">
<caption>バージョン 1.12.4_1535 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>[etcd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coreos/etcd/releases/v3.3.11) を参照してください。 さらに、etcd でサポートされる暗号スイートが、強力な暗号化 (128 ビット以上) を使用するサブセットだけに制限されるようになりました。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>[CVE-2019-3462 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) および [CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.4-118</td>
<td>v1.12.5-137</td>
<td>Kubernetes 1.12.5 リリースをサポートするように更新されました。 さらに、`calicoctl` バージョンが 3.3.1 に更新されました。 ワーカー・ノードの状況変化時にバージョン 2.0 ロード・バランサーに対して行われていた不要な構成更新が修正されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>338</td>
<td>342</td>
<td>ファイル・ストレージ・プラグインが次のように更新されました。
<ul><li>[ボリューム・トポロジー対応スケジューリング](/docs/containers?topic=containers-file_storage#file-topology)による動的プロビジョニングをサポートします。</li>
<li>ストレージが既に削除されている場合は、永続ボリューム請求 (PVC) 削除エラーを無視します。</li>
<li>失敗した PVC に失敗メッセージのアノテーションを追加します。</li>
<li>ストレージ・プロビジョナー・コントローラーのリーダー選択と再同期期間の設定を最適化し、プロビジョニング・タイムアウトを 30 分から 1 時間に増やします。</li>
<li>プロビジョニングを開始する前にユーザー許可を確認します。</li></ul></td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>111</td>
<td>122</td>
<td>Kubernetes シークレットが {{site.data.keyword.keymanagementservicefull_notm}} によって管理されている場合の一時的な失敗を回避するための再試行ロジックが追加されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.12.5</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.5) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>Kubernetes API サーバーの監査ポリシー構成が更新され、`cluster-admin` 要求のメタデータのロギングと、ワークロードの`作成`、`更新`、および`パッチ`要求の本体のロギングが含まれるようになりました。</td>
</tr>
<tr>
<td>OpenVPN クライアント</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) および [CVE-2018-5407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) への対策としてイメージが更新されました。 さらに、ポッド構成が構成マップからではなくシークレットから取得されるようになりました。</td>
</tr>
<tr>
<td>OpenVPN サーバー</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) および [CVE-2018-5407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864) のセキュリティー・パッチ。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.12.4_1535 (2019 年 1 月 28 日リリース) の変更ログ
{: #1124_1535}

以下の表に、ワーカー・ノードのフィックスパック 1.12.4_1535 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.4_1534 からの変更点">
<caption>バージョン 1.12.4_1534 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>[CVE-2019-3462 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) および [USN-3863-1 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://usn.ubuntu.com/3863-1) の `apt` を含むインストール済み Ubuntu パッケージの更新。</td>
</tr>
</tbody>
</table>


### 1.12.4_1534 (2019 年 1 月 21 日リリース) の変更ログ
{: #1124_1534}

以下の表に、パッチ 1.12.3_1534 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.3_1533 からの変更点">
<caption>バージョン 1.12.3_1533 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.3-91</td>
<td>v1.12.4-118</td>
<td>Kubernetes 1.12.4 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.3</td>
<td>v1.12.4</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.4) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes アドオン・サイズ変更機能</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>[Kubernetes アドオン・サイズ変更機能のリリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes ダッシュボード</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>[Kubernetes ダッシュボードのリリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1) を参照してください。 更新により、[CVE-2018-18264 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264) が解決されます。</td>
</tr>
<tr>
<td>GPU インストーラー</td>
<td>390.12</td>
<td>410.79</td>
<td>インストール済み GPU ドライバーが 410.79 に更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.12.3_1533 (2019 年 1 月 7 日リリース) の変更ログ
{: #1123_1533}

以下の表に、ワーカー・ノードのフィックスパック 1.12.3_1533 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.3_1532 からの変更点">
<caption>バージョン 1.12.3_1532 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>[CVE-2017-5753、CVE-2018-18690 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.12.3_1532 (2018 年 12 月 17 日リリース) の変更ログ
{: #1123_1532}

以下の表に、ワーカー・ノードのフィックスパック 1.12.2_1532 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.3_1531 からの変更点">
<caption>バージョン 1.12.3_1531 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>インストール済み Ubuntu パッケージが更新されました。</td>
</tr>
</tbody>
</table>


### 1.12.3_1531 (2018 年 12 月 5 日リリース) の変更ログ
{: #1123_1531}

以下の表に、パッチ 1.12.3_ 1531 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.2_1530 からの変更点">
<caption>バージョン 1.12.2_1530 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.12.2-68</td>
<td>v1.12.3-91</td>
<td>Kubernetes 1.12.3 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.2</td>
<td>v1.12.3</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.3) を参照してください。 更新により、[CVE-2018-1002105 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/issues/71411) が解決されます。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.12.2_1530 (2018 年 12 月 4 日リリース) の変更ログ
{: #1122_1530}

以下の表に、ワーカー・ノードのフィックスパック 1.12.2_1530 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.2_1529 からの変更点">
<caption>バージョン 1.12.2_1529 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>ワーカー・ノードのリソースの使用方法</td>
<td>該当なし</td>
<td>該当なし</td>
<td>kubelet と containerd がリソース不足にならないように、これらのコンポーネント用に専用の cgroup が追加されました。 詳しくは、[ワーカー・ノードのリソースの予約](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)を参照してください。</td>
</tr>
</tbody>
</table>



### 1.12.2_1529 (2018 年 11 月 27 日リリース) の変更ログ
{: #1122_1529}

以下の表に、パッチ 1.12.2_1529 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.2_1528 からの変更点">
<caption>バージョン 1.12.2_1528 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>[Calico リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.3/releases/#v331) を参照してください。 更新により、[Tigera Technical Advisory TTA-2018-001 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.projectcalico.org/security-bulletins/) が解決されます。</td>
</tr>
<tr>
<td>クラスター DNS 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスターの作成/更新の操作後に Kubernetes DNS ポッドと CoreDNS ポッドの両方が実行されるバグが修正されました。</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.2.0</td>
<td>v1.1.5</td>
<td>[containerd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.1.5) を参照してください。 [ポッドの終了を妨げるデッドロック ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/issues/2744) が起きないように containerd が更新されました。</td>
</tr>
<tr>
<td>OpenVPN クライアントとサーバー</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>[CVE-2018-0732 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) および [CVE-2018-0737 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.12.2_1528 (2018 年 11 月 19 日リリース) の変更ログ
{: #1122_1528}

以下の表に、ワーカー・ノードのフィックスパック 1.12.2_1528 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.12.2_1527 からの変更点">
<caption>バージョン 1.12.2_1527 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>[CVE-2018-7755 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>


### 1.12.2_1527 (2018 年 11 月 7 日リリース) の変更ログ
{: #1122_1527}

以下の表に、パッチ 1.12.2_1527 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.3_1533 からの変更点">
<caption>バージョン 1.11.3_1533 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kube-system` 名前空間の Calico の `calico-*` ポッドについて、すべてのコンテナーの CPU とメモリーのリソース要求を設定するようになりました。</td>
</tr>
<tr>
<td>クラスターの DNS プロバイダー</td>
<td>該当なし</td>
<td>該当なし</td>
<td>デフォルトのクラスターの DNS プロバイダーは Kubernetes DNS (KubeDNS) のままです。 しかし、[クラスターの DNS プロバイダーを CoreDNS に変更する](/docs/containers?topic=containers-cluster_dns#dns_set)オプションを選択できるようになりました。</td>
</tr>
<tr>
<td>クラスター・メトリック・プロバイダー</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスター・メトリック・プロバイダーとして、Kubernetes Heapster (Kubernetes バージョン 1.8 以降では非推奨) が Kubernetes Metrics Server に置き換えられました。 アクション項目については、[`metrics-server` 準備アクション](/docs/containers?topic=containers-cs_versions#metrics-server)を参照してください。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.4</td>
<td>1.2.0</td>
<td>[containerd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.2.0) を参照してください。</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>該当なし</td>
<td>1.2.2</td>
<td>[CoreDNS リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coredns/coredns/releases/tag/v1.2.2) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.12.2</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.2) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>3 つの新しい IBM ポッド・セキュリティー・ポリシーと、関連するクラスター役割が追加されました。 詳しくは、[IBM クラスター管理のためのデフォルトのリソースについて](/docs/containers?topic=containers-psp#ibm_psp)を参照してください。</td>
</tr>
<tr>
<td>Kubernetes ダッシュボード</td>
<td>v1.8.3</td>
<td>v1.10.0</td>
<td>[Kubernetes ダッシュボードのリリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.0) を参照してください。<br><br>
`kubectl proxy` でダッシュボードにアクセスすると、ログイン・ページ上の**「スキップ (SKIP)」**ボタンが削除されます。 代わりに、[**「トークン (Token)」**を使用してログインしてください](/docs/containers?topic=containers-app#cli_dashboard)。 さらに、`kubectl -n kube-system scale deploy kubernetes-dashboard --replicas=3` を実行して、Kubernetes ダッシュボード・ポッドの数を増やせるようになりました。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>[Kubernetes DNS リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/dns/releases/tag/1.14.13) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>該当なし</td>
<td>v0.3.1</td>
<td>[Kubernetes Metrics Server リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.1) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-118</td>
<td>v1.12.2-68</td>
<td>Kubernetes 1.12 リリースをサポートするように更新されました。 その他の変更には、次のようなものがあります。
<ul><li>ロード・バランサー・ポッド (`ibm-system` 名前空間の `ibm-cloud-provider-ip-*` ポッド) で、CPU とメモリーのリソース要求を設定するようになりました。</li>
<li>ロード・バランサー・サービスのデプロイ先の VLAN を指定する `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` アノテーションが追加されました。 クラスター内の使用可能な VLAN を表示するには、`ibmcloud ks vlans --zone <zone>` を実行します。</li>
<li>新しい[ロード・バランサー 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) がベータ版として提供されています。</li></ul></td>
</tr>
<tr>
<td>OpenVPN クライアント構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kube-system` 名前空間の OpenVPN クライアントの `vpn-* pod` について、CPU とメモリーのリソース要求を設定するようになりました。</td>
</tr>
</tbody>
</table>

## 非推奨: バージョン 1.11 変更ログ
{: #111_changelog}

バージョン 1.11 の変更ログをまとめます。
{: shortdesc}

Kubernetes バージョン 1.11 は非推奨バージョンであり、2019 年 6 月 27 日 (暫定) に非サポート・バージョンになります。各 Kubernetes バージョンの更新が[与える可能性のある影響を確認](/docs/containers?topic=containers-cs_versions#cs_versions)したうえで、少なくとも 1.12 にただちに[クラスターを更新](/docs/containers?topic=containers-update#update)してください。
{: deprecated}

### 1.11.10_1561 (2019 年 6 月 4 日リリース) の変更ログ
{: #11110_1561}

以下の表に、パッチ 1.11.10_1561 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.10_1559 からの変更点">
<caption>バージョン 1.11.10_1559 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター HA 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>マスター更新時に断続的に発生するマスター・ネットワーク接続障害を最小限に抑えるように構成が更新されました。</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>[etcd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coreos/etcd/releases/v3.3.13) を参照してください。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>[CVE-2018-10844 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435)、および [CVE-2019-5436 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>[CVE-2018-10844 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844)、[CVE-2018-10845 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845)、[CVE-2018-10846 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)、[CVE-2019-3829 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829)、[CVE-2019-3836 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836)、[CVE-2019-9893 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893)、[CVE-2019-5435 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435)、および [CVE-2019-5436 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.11.10_1559 (2019 年 5 月 20 日リリース) の変更ログ
{: #11110_1559}

以下の表に、パッチ・パック 1.11.10_1559 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.10_1558 からの変更点">
<caption>バージョン 1.11.10_1558 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu 16.04 カーネル</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>[CVE-2018-12126 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html)、および [CVE-2018-12130 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>Ubuntu 18.04 カーネル</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>[CVE-2018-12126 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html)、[CVE-2018-12127 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html)、および [CVE-2018-12130 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

### 1.11.10_1558 (2019 年 5 月 13 日リリース) の変更ログ
{: #11110_1558}

以下の表に、パッチ 1.11.10_1558 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.9_1556 からの変更点">
<caption>バージョン 1.11.9_1556 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター HA プロキシー</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>[HAProxy リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.haproxy.org/download/1.9/src/CHANGELOG) を参照してください。 更新により、[CVE-2019-6706 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706) が解決されます。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>[CVE-2019-1543 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.9-241</td>
<td>v1.11.10-270</td>
<td>Kubernetes 1.11.10 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.9</td>
<td>v1.11.10</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.10) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>Kubernetes API サーバーの監査ポリシー構成が更新され、`/openapi/v2*` 読み取り専用 URL がログに記録されなくなりました。さらに、Kubernetes コントローラー・マネージャーの構成により、署名済み `kubelet` 証明書の有効期間が 1 年から 3 年に延長されました。</td>
</tr>
<tr>
<td>OpenVPN クライアント構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kube-system` 名前空間の OpenVPN クライアントの `vpn-*` ポッドについて、クラスター DNS がダウンしたときにポッドが失敗しないように `dnsPolicy` が `Default` に設定されるようになりました。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>[CVE-2016-7076 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076)、[CVE-2017-1000368 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368)、および [CVE-2019-11068 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.11.9_1556 (2019 年 4 月 29 日リリース) の変更ログ
{: #1119_1556}

以下の表に、ワーカー・ノードのフィックスパック 1.11.9_1556 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.9_1555 からの変更点">
<caption>バージョン 1.11.9_1555 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>インストール済み Ubuntu パッケージが更新されました。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>[containerd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.1.7) を参照してください。</td>
</tr>
</tbody>
</table>


### ワーカー・ノード・フィックスパック 1.11.9_1555 (2019 年 4 月 15 日リリース) の変更ログ
{: #1119_1555}

以下の表に、ワーカー・ノードのフィックスパック 1.11.9_1555 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.9_1554 からの変更点">
<caption>1.11.9_1554 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>[CVE-2019-3842 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html) の `systemd` を含むインストール済み Ubuntu パッケージの更新。</td>
</tr>
</tbody>
</table>

### 1.11.9_1554 (2019 年 4 月 8 日リリース) の変更ログ
{: #1119_1554}

以下の表に、パッチ 1.11.9_1554 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.8_1553 からの変更点">
<caption>バージョン 1.11.8_1553 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>[Calico リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.3/releases/#v336) を参照してください。 更新により、[CVE-2019-9946 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946) が解決されます。</td>
</tr>
<tr>
<td>クラスター・マスター HA プロキシー</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>[HAProxy リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.haproxy.org/download/1.9/src/CHANGELOG) を参照してください。 更新により、[CVE-2018-0732 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732)、[CVE-2018-0734 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734)、[CVE-2018-0737 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)、[CVE-2018-5407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)、[CVE-2019-1543 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543)、および [CVE-2019-1559 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) が解決されます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.8-219</td>
<td>v1.11.9-241</td>
<td>Kubernetes 1.11.9 リリースおよび Calico 3.3.6 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.8</td>
<td>v1.11.9</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.9) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>[Kubernetes DNS リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/dns/releases/tag/1.14.13) を参照してください。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>[CVE-2017-12447 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>Ubuntu 16.04 カーネル</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>[CVE-2019-9213 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>Ubuntu 18.04 カーネル</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>[CVE-2019-9213 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.11.8_1553 (2019 年 4 月 1 日リリース) の変更ログ
{: #1118_1553}

以下の表に、ワーカー・ノードのフィックスパック 1.11.8_1553 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.8_1552 からの変更点">
<caption>バージョン 1.11.8_1552 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>ワーカー・ノードのリソースの使用方法</td>
<td>該当なし</td>
<td>該当なし</td>
<td>kubelet と containerd がリソース不足にならないように、これらのコンポーネント用のメモリー予約が増加されました。 詳しくは、[ワーカー・ノードのリソースの予約](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)を参照してください。</td>
</tr>
</tbody>
</table>

### マスター・フィックスパック 1.11.8_1552 (2019 年 3 月 26 日リリース) の変更ログ
{: #1118_1552}

以下の表に、マスターのフィックスパック 1.11.8_1552 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.8_1550 からの変更点">
<caption>バージョン 1.11.8_1550 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>345</td>
<td>346</td>
<td>[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>166</td>
<td>167</td>
<td>Kubernetes シークレットを管理するための `context deadline exceeded` および `timeout` で断続的に発生するエラーが修正されました。 また、既存の Kubernetes シークレットが暗号化されないままになることがある鍵管理サービスの更新も修正されました。 更新には、[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) の修正が含まれます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} プロバイダー用のロード・バランサーとロード・バランサー・モニター</td>
<td>143</td>
<td>146</td>
<td>[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### 1.11.8_1550 (2019 年 3 月 20 日リリース) の変更ログ
{: #1118_1550}

以下の表に、パッチ 1.11.8_1550 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.8_1547 からの変更点">
<caption>バージョン 1.11.8_1547 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター HA プロキシー構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>断続的に発生するクラスター・マスターへの接続エラーへの対応を向上させるために構成が更新されました。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>GPU ドライバーが [418.43 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.nvidia.com/object/unix.html) に更新されました。 更新には、[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html) の修正が含まれます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>344</td>
<td>345</td>
<td>[プライベート・サービス・エンドポイント](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)のサポートが追加されました。</td>
</tr>
<tr>
<td>カーネル</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>[CVE-2019-6133 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>136</td>
<td>166</td>
<td>[CVE-2018-16890 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890)、[CVE-2019-3822 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822)、および [CVE-2019-3823 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>[CVE-2018-10779 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779)、[CVE-2018-12900 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900)、[CVE-2018-17000 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000)、[CVE-2018-19210 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210)、[CVE-2019-6128 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128)、および [CVE-2019-7663 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### 1.11.8_1547 (2019 年 3 月 4 日リリース) の変更ログ
{: #1118_1547}

以下の表に、パッチ 1.11.8_1547 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.7_1546 からの変更点">
<caption>バージョン 1.11.7_1546 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>[CVE-2019-6454 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.7-198</td>
<td>v1.11.8-219</td>
<td>Kubernetes 1.11.8 リリースをサポートするように更新されました。 `externalTrafficPolicy` を `local` に設定したロード・バランサーに定期的に発生していた接続の問題が修正されました。 最新の {{site.data.keyword.Bluemix_notm}} ドキュメント・リンクを使用するようにロード・バランサーのイベントが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>342</td>
<td>344</td>
<td>イメージの基本オペレーティング・システムが Fedora から Alpine に変更されました。 [CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>122</td>
<td>136</td>
<td>クライアントのタイムアウトが {{site.data.keyword.keymanagementservicefull_notm}} に増やされました。 [CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.7</td>
<td>v1.11.8</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.8) を参照してください。 更新により、[CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) および [CVE-2019-1002100 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100) が解決されます。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`--feature-gates` オプションに `ExperimentalCriticalPodAnnotation=true` が追加されました。 この設定は、ポッドを非推奨の `scheduler.alpha.kubernetes.io/critical-pod` アノテーションから [Kubernetes ポッドの優先度のサポート](/docs/containers?topic=containers-pod_priority#pod_priority)に移行するのに役立ちます。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>該当なし</td>
<td>該当なし</td>
<td>より多くのクラスター・サービスを処理するために、Kubernetes DNS ポッドのメモリー制限が `170 Mi` から `400 Mi` に増えました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} プロバイダー用のロード・バランサーとロード・バランサー・モニター</td>
<td>132</td>
<td>143</td>
<td>[CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>OpenVPN クライアントとサーバー</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>[CVE-2019-1559 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>[CVE-2019-6454 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.11.7_1546 (2019 年 2 月 27 日リリース) の変更ログ
{: #1117_1546}

以下の表に、ワーカー・ノードのフィックスパック 1.11.7_1546 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.7_1546 からの変更点">
<caption>バージョン 1.11.7_1546 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>[CVE-2018-19407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.11.7_1544 (2019 年 2 月 15 日リリース) の変更ログ
{: #1117_1544}

以下の表に、ワーカー・ノードのフィックスパック 1.11.7_1544 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.7_1543 からの変更点">
<caption>バージョン 1.11.7_1543 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター HA プロキシー構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ポッド構成値 `spec.priorityClassName` が `system-node-critical` に変更され、`spec.priority` の値が `2000001000` に設定されました。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>[containerd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.1.6) を参照してください。 更新により、[CVE-2019-5736 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736) が解決されます。</td>
</tr>
<tr>
<td>Kubernetes の `kubelet` 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`ExperimentalCriticalPodAnnotation` 機能ゲートで静的ポッドの重大な強制除去の発生を防止できるようになりました。</td>
</tr>
</tbody>
</table>

### 1.11.7_1543 (2019 年 2 月 5 日リリース) の変更ログ
{: #1117_1543}

以下の表に、パッチ 1.11.7_1543 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.6_1541 からの変更点">
<caption>バージョン 1.11.6_1541 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>[etcd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coreos/etcd/releases/v3.3.11) を参照してください。 さらに、etcd でサポートされる暗号スイートが、強力な暗号化 (128 ビット以上) を使用するサブセットだけに制限されるようになりました。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>[CVE-2019-3462 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) および [CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.6-180</td>
<td>v1.11.7-198</td>
<td>Kubernetes 1.11.7 リリースをサポートするように更新されました。 さらに、`calicoctl` バージョンが 3.3.1 に更新されました。 ワーカー・ノードの状況変化時にバージョン 2.0 ロード・バランサーに対して行われていた不要な構成更新が修正されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>338</td>
<td>342</td>
<td>ファイル・ストレージ・プラグインが次のように更新されました。
<ul><li>[ボリューム・トポロジー対応スケジューリング](/docs/containers?topic=containers-file_storage#file-topology)による動的プロビジョニングをサポートします。</li>
<li>ストレージが既に削除されている場合は、永続ボリューム請求 (PVC) 削除エラーを無視します。</li>
<li>失敗した PVC に失敗メッセージのアノテーションを追加します。</li>
<li>ストレージ・プロビジョナー・コントローラーのリーダー選択と再同期期間の設定を最適化し、プロビジョニング・タイムアウトを 30 分から 1 時間に増やします。</li>
<li>プロビジョニングを開始する前にユーザー許可を確認します。</li></ul></td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>111</td>
<td>122</td>
<td>Kubernetes シークレットが {{site.data.keyword.keymanagementservicefull_notm}} によって管理されている場合の一時的な失敗を回避するための再試行ロジックが追加されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.6</td>
<td>v1.11.7</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.7) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>Kubernetes API サーバーの監査ポリシー構成が更新され、`cluster-admin` 要求のメタデータのロギングと、ワークロードの`作成`、`更新`、および`パッチ`要求の本体のロギングが含まれるようになりました。</td>
</tr>
<tr>
<td>OpenVPN クライアント</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) および [CVE-2018-5407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) への対策としてイメージが更新されました。 さらに、ポッド構成が構成マップからではなくシークレットから取得されるようになりました。</td>
</tr>
<tr>
<td>OpenVPN サーバー</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) および [CVE-2018-5407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864) のセキュリティー・パッチ。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.11.6_1541 (2019 年 1 月 28 日リリース) の変更ログ
{: #1116_1541}

以下の表に、ワーカー・ノードのフィックスパック 1.11.6_1541 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.6_1540 からの変更点">
<caption>バージョン 1.11.6_1540 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>[CVE-2019-3462 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) / [USN-3863-1 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://usn.ubuntu.com/3863-1) の `apt` を含むインストール済み Ubuntu パッケージの更新。</td>
</tr>
</tbody>
</table>

### 1.11.6_1540 (2019 年 1 月 21 日リリース) の変更ログ
{: #1116_1540}

以下の表に、パッチ 1.11.6_1540 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.5_1539 からの変更点">
<caption>バージョン 1.11.5_1539 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.5-152</td>
<td>v1.11.6-180</td>
<td>Kubernetes 1.11.6 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.5</td>
<td>v1.11.6</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.6) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes アドオン・サイズ変更機能</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>[Kubernetes アドオン・サイズ変更機能のリリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes ダッシュボード</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>[Kubernetes ダッシュボードのリリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1) を参照してください。 更新により、[CVE-2018-18264 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264) が解決されます。<br><br>`kubectl proxy` でダッシュボードにアクセスすると、ログイン・ページ上の**「スキップ (SKIP)」**ボタンが削除されます。 代わりに、[**「トークン (Token)」**を使用してログインしてください](/docs/containers?topic=containers-app#cli_dashboard)。</td>
</tr>
<tr>
<td>GPU インストーラー</td>
<td>390.12</td>
<td>410.79</td>
<td>インストール済み GPU ドライバーが 410.79 に更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.11.5_1539 (2019 年 1 月 7 日リリース) の変更ログ
{: #1115_1539}

以下の表に、ワーカー・ノードのフィックスパック 1.11.5_1539 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.5_1538 からの変更点">
<caption>バージョン 1.11.5_1538 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>[CVE-2017-5753、CVE-2018-18690 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.11.5_1538 (2018 年 12 月 17 日リリース) の変更ログ
{: #1115_1538}

以下の表に、ワーカー・ノードのフィックスパック 1.11.5_1538 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.5_1537 からの変更点">
<caption>バージョン 1.11.5_1537 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>インストール済み Ubuntu パッケージが更新されました。</td>
</tr>
</tbody>
</table>

### 1.11.5_1537 (2018 年 12 月 5 日リリース) の変更ログ
{: #1115_1537}

以下の表に、パッチ 1.11.5_1537 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.4_1536 からの変更点">
<caption>バージョン 1.11.4_1536 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.4-142</td>
<td>v1.11.5-152</td>
<td>Kubernetes 1.11.5 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.4</td>
<td>v1.11.5</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.5) を参照してください。 更新により、[CVE-2018-1002105 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/issues/71411) が解決されます。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.11.4_1536 (2018 年 12 月 4 日リリース) の変更ログ
{: #1114_1536}

以下の表に、ワーカー・ノードのフィックスパック 1.11.4_1536 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.4_1535 からの変更点">
<caption>バージョン 1.11.4_1535 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>ワーカー・ノードのリソースの使用方法</td>
<td>該当なし</td>
<td>該当なし</td>
<td>kubelet と containerd がリソース不足にならないように、これらのコンポーネント用に専用の cgroup が追加されました。 詳しくは、[ワーカー・ノードのリソースの予約](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)を参照してください。</td>
</tr>
</tbody>
</table>

### 1.11.4_1535 (2018 年 11 月 27 日リリース) の変更ログ
{: #1114_1535}

以下の表に、パッチ 1.11.4_1535 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.3_1534 からの変更点">
<caption>バージョン 1.11.3_1534 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>[Calico リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.3/releases/#v331) を参照してください。 更新により、[Tigera Technical Advisory TTA-2018-001 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.projectcalico.org/security-bulletins/) が解決されます。</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.1.4</td>
<td>v1.1.5</td>
<td>[containerd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.1.5) を参照してください。 [ポッドの終了を妨げるデッドロック ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/issues/2744) が起きないように containerd が更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-127</td>
<td>v1.11.4-142</td>
<td>Kubernetes 1.11.4 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.11.4</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.4) を参照してください。</td>
</tr>
<tr>
<td>OpenVPN クライアントとサーバー</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>[CVE-2018-0732 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) および [CVE-2018-0737 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.11.3_1534 (2018 年 11 月 19 日リリース) の変更ログ
{: #1113_1534}

以下の表に、ワーカー・ノードのフィックスパック 1.11.3_1534 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.3_1533 からの変更点">
<caption>バージョン 1.11.3_1533 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>[CVE-2018-7755 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>


### 1.11.3_1533 (2018 年 11 月 7 日リリース) の変更ログ
{: #1113_1533}

以下の表に、パッチ 1.11.3_1533 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.3_1531 からの変更点">
<caption>バージョン 1.11.3_1531 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター HA の更新</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`initializerconfigurations`、`mutatingwebhookconfigurations`、`validatingwebhookconfigurations` などのアドミッション Webhook を使用するクラスターの高可用性 (HA) マスターに対する更新を修正しました。 これらの Webhook を、[Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce) などのために Helm チャートで使用することもできます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-100</td>
<td>v1.11.3-127</td>
<td>ロード・バランサー・サービスのデプロイ先の VLAN を指定する `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` アノテーションが追加されました。 クラスター内の使用可能な VLAN を表示するには、`ibmcloud ks vlans --zone <zone>` を実行します。</td>
</tr>
<tr>
<td>TPM 対応カーネル</td>
<td>該当なし</td>
<td>該当なし</td>
<td>トラステッド・コンピューティング用の TPM チップを搭載したベアメタルのワーカー・ノードでは、トラストを有効にするまで、デフォルトの Ubuntu カーネルが使用されます。 既存のクラスターで[トラストを有効にした](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)場合には、TPM チップを搭載した既存のベアメタルのワーカー・ノードを[再ロード](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)する必要があります。 ベアメタルのワーカー・ノードに TPM チップが搭載されているかどうかを確認するには、 `ibmcloud ks machine-types --zone` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)の実行後に、**Trustable** フィールドを確認します。</td>
</tr>
</tbody>
</table>

### マスター・フィックスパック 1.11.3_1531 (2018 年 11 月 1 日リリース) の変更ログ
{: #1113_1531_ha-master}

以下の表に、マスターのフィックスパック 1.11.3_1531 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.3_1527 からの変更点">
<caption>バージョン 1.11.3_1527 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスター・マスターの構成が更新され、高可用性 (HA) が向上しました。 現在のクラスターは、3 台の Kubernetes マスター・レプリカがそれぞれ別の物理ホスト上にデプロイされる高可用性 (HA) 構成でセットアップされるようになりました。 さらに、クラスターが複数ゾーン対応ゾーンにある場合は、それらのマスターがゾーン間に分散されます。<br>実行する必要があるアクションについては、[高可用性クラスター・マスターへの更新](/docs/containers?topic=containers-cs_versions#ha-masters)を参照してください。 これらの準備アクションは、以下の場合に適用されます。<ul>
<li>ファイアウォールまたはカスタム Calico ネットワーク・ポリシーがある場合。</li>
<li>ワーカー・ノードでホスト・ポート `2040` または `2041` を使用している場合。</li>
<li>マスターへのクラスター内アクセス用にクラスターのマスター IP アドレスを使用していた場合。</li>
<li>Calico ポリシーの作成などのために、Calico API または CLI (`calicoctl`) を呼び出す自動化機能がある場合。</li>
<li>Kubernetes または Calico ネットワーク・ポリシーを使用して、ポッドからマスターへの発信アクセスを制御している場合。</li></ul></td>
</tr>
<tr>
<td>クラスター・マスター HA プロキシー</td>
<td>該当なし</td>
<td>1.8.12-alpine</td>
<td>すべてのワーカー・ノードにクライアント・サイド・ロード・バランシングのための `ibm-master-proxy-*` ポッドが追加されたので、各ワーカー・ノード・クライアントが、使用可能な HA マスター・レプリカに要求をルーティングできるようになりました。</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>[etcd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coreos/etcd/releases/v3.3.1) を参照してください。</td>
</tr>
<tr>
<td>etcd のデータの暗号化</td>
<td>該当なし</td>
<td>該当なし</td>
<td>以前は、etcd データは、保存時に暗号化が行われるマスターの NFS ファイル・ストレージ・インスタンスに保管されていました。 現在は、etcd データはマスターのローカル・ディスクに保管され、{{site.data.keyword.cos_full_notm}} にバックアップされます。 {{site.data.keyword.cos_full_notm}} に転送中のデータも保存されたデータも暗号化されています。 しかし、マスターのローカル・ディスク上の etcd データは暗号化されません。 マスターのローカル etcd データを暗号化する場合は、[クラスター内で {{site.data.keyword.keymanagementservicelong_notm}} を有効にします](/docs/containers?topic=containers-encryption#keyprotect)。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.11.3_1531 (2018 年 10 月 26 日リリース) の変更ログ
{: #1113_1531}

以下の表に、ワーカー・ノードのフィックスパック 1.11.3_1531 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.3_1525 からの変更点">
<caption>バージョン 1.11.3_1525 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>OS 割り込み処理</td>
<td>該当なし</td>
<td>該当なし</td>
<td>割り込み要求 (IRQ) システム・デーモンが、さらにパフォーマンスの高い割り込みハンドラーに置き換えられました。</td>
</tr>
</tbody>
</table>

### マスター・フィックスパック 1.11.3_1527 (2018 年 10 月 15 日リリース) の変更ログ
{: #1113_1527}

以下の表に、マスターのフィックスパック 1.11.3_1527 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.3_1524 からの変更点">
<caption>バージョン 1.11.3_1524 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ノードの障害を処理しやすくするために `calico-node` コンテナーの Readiness Probe が修正されました。</td>
</tr>
<tr>
<td>クラスター更新</td>
<td>該当なし</td>
<td>該当なし</td>
<td>サポートされないバージョンからのマスター更新時に、クラスター・アドオンの更新で発生していた問題が修正されました。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.11.3_1525 (2018 年 10 月 10 日リリース) の変更ログ
{: #1113_1525}

以下の表に、ワーカー・ノードのフィックスパック 1.11.3_1525 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.3_1524 からの変更点">
<caption>バージョン 1.11.3_1524 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>[CVE-2018-14633、CVE-2018-17182 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>非アクティブ・セッションのタイムアウト</td>
<td>該当なし</td>
<td>該当なし</td>
<td>コンプライアンスの理由により非アクティブ・セッションのタイムアウトが 5 分に設定されました。</td>
</tr>
</tbody>
</table>


### 1.11.3_1524 (2018 年 10 月 2 日リリース) の変更ログ
{: #1113_1524}

以下の表に、パッチ 1.11.3_1524 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.3_1521 からの変更点">
<caption>バージョン 1.11.3_1521 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>1.1.3</td>
<td>1.1.4</td>
<td>[containerd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.1.4) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.3-91</td>
<td>v1.11.3-100</td>
<td>ロード・バランサーのエラー・メッセージ内の文書リンクが更新されました。</td>
</tr>
<tr>
<td>IBM ファイル・ストレージ・クラス</td>
<td>該当なし</td>
<td>該当なし</td>
<td>IBM ファイル・ストレージ・クラスの重複 `reclaimPolicy` パラメーターが削除されました。<br><br>
また、クラスター・マスターの更新時に、デフォルトの IBM ファイル・ストレージ・クラスが変更されなくなりました。 デフォルトのストレージ・クラスを変更する場合は、`kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` を実行して、`<storageclass>` をストレージ・クラスの名前に置き換えます。</td>
</tr>
</tbody>
</table>

### 1.11.3_1521 (2018 年 9 月 20 日リリース) の変更ログ
{: #1113_1521}

以下の表に、パッチ 1.11.3_1521 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.2_1516 からの変更点">
<caption>バージョン 1.11.2_1516 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.2-71</td>
<td>v1.11.3-91</td>
<td>Kubernetes 1.11.3 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>IBM ファイル・ストレージ・クラス</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ワーカー・ノードで提供されるデフォルトを使用するように、IBM ファイル・ストレージ・クラスの `mountOptions` が削除されました。<br><br>
また、クラスター・マスターの更新時に、デフォルトの IBM ファイル・ストレージ・クラスが `ibmc-file-bronze` のままになりました。 デフォルトのストレージ・クラスを変更する場合は、`kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` を実行して、`<storageclass>` をストレージ・クラスの名前に置き換えます。</td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>該当なし</td>
<td>該当なし</td>
<td>{{site.data.keyword.keymanagementservicefull}} をサポートするため、クラスターで Kubernetes 鍵管理サービス (KMS) プロバイダーを使用する機能が追加されました。 [クラスターで {{site.data.keyword.keymanagementserviceshort}} を有効にする](/docs/containers?topic=containers-encryption#keyprotect)と、すべての Kubernetes シークレットが暗号化されます。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.2</td>
<td>v1.11.3</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.3) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes DNS autoscaler</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>[Kubernetes DNS autoscaler リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0) を参照してください。</td>
</tr>
<tr>
<td>ログの循環</td>
<td>該当なし</td>
<td>該当なし</td>
<td>90 日内に再ロードまたは更新されないワーカー・ノードで `logrotate` が失敗することを防止するため、`cronjobs` の代わりに `systemd` タイマーを使用するように切り替えられました。 **注**: このマイナー・リリースの前のすべてのバージョンでは、ログが循環されないため、cron ジョブが失敗した後に 1 次ディスクが満杯になります。 ワーカー・ノードが更新も再ロードもされずに 90 日間アクティブだった場合、cron ジョブは失敗します。 ログで 1 次ディスクが満杯になった場合、ワーカー・ノードは失敗状態になります。 ワーカー・ノードを修正するには、`ibmcloud ks worker-reload` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)または `ibmcloud ks worker-update` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)を使用できます。</td>
</tr>
<tr>
<td>ルート・パスワードの有効期限</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ワーカー・ノードのルート・パスワードは、コンプライアンスの理由により 90 日後に有効期限が切れます。 自動化ツールがワーカー・ノードに root としてログインする必要がある場合や、root として実行される cron ジョブを使用している場合は、ワーカー・ノードにログインして `chage -M -1 root` を実行することによって、パスワード有効期限を無効にすることができます。 **注**: セキュリティー・コンプライアンスに関する要件で、root として実行したり、パスワード有効期限を削除したりすることを避ける必要がある場合は、有効期限を無効にしないでください。 代わりに、少なくとも 90 日ごとにワーカー・ノードを[更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)または[再ロード](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)してください。</td>
</tr>
<tr>
<td>ワーカー・ノード・ランタイム・コンポーネント (`kubelet`、`kube-proxy`、`containerd`)</td>
<td>該当なし</td>
<td>該当なし</td>
<td>1 次ディスクでのランタイム・コンポーネントの従属関係が削除されました。 この機能強化によって、1 次ディスクが満杯になった場合にワーカー・ノードが失敗することが回避されます。</td>
</tr>
<tr>
<td>systemd</td>
<td>該当なし</td>
<td>該当なし</td>
<td>一時的なマウント装置が無制限に増えることを防止するため、それらを定期的に削除します。 このアクションは、[Kubernetes 問題 57345 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/issues/57345) に対処するためのものです。</td>
</tr>
</tbody>
</table>

### 1.11.2_1516 (2018 年 9 月 4 日リリース) の変更ログ
{: #1112_1516}

以下の表に、パッチ 1.11.2_1516 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.2_1514 からの変更点">
<caption>バージョン 1.11.2_1514 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>[Calico リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.2/releases/#v321) を参照してください。</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.2</td>
<td>1.1.3</td>
<td>[`containerd` リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.1.3) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.11.2-60</td>
<td>v1.11.2-71</td>
<td>`externalTrafficPolicy` が `local` に設定されて、ロード・バランサー・サービスの更新をより良好に処理できるようにクラウド・プロバイダー構成が変更されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>IBM 提供のファイル・ストレージ・クラスのマウント・オプションから、デフォルトの NFS バージョンが削除されました。 ホストのオペレーティング・システムによって、NFS バージョンが、IBM Cloud インフラストラクチャー (SoftLayer) の NFS サーバーとネゴシエーションされるようになりました。 特定の NFS バージョンを手動で設定する場合、またはホストのオペレーティング・システムによってネゴシエーションされた PV の NFS バージョンを変更する場合は、[デフォルトの NFS バージョンの変更](/docs/containers?topic=containers-file_storage#nfs_version_class)を参照してください。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.11.2_1514 (2018 年 8 月 23 日リリース) の変更ログ
{: #1112_1514}

以下の表に、ワーカー・ノードのフィックスパック 1.11.2_1514 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.11.2_1513 からの変更点">
<caption>バージョン 1.11.2_1513 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>`cgroup` リークを修正するように `systemd` が更新されました。</td>
</tr>
<tr>
<td>カーネル</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>[CVE-2018-3620、CVE-2018-3646 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://usn.ubuntu.com/3741-1/) に関連するカーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

### 1.11.2_1513 (2018 年 8 月 14 日リリース) の変更ログ
{: #1112_1513}

以下の表に、パッチ 1.11.2_1513 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.5_1518 からの変更点">
<caption>バージョン 1.10.5_1518 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>該当なし</td>
<td>1.1.2</td>
<td>`containerd` は、Kubernetes の新しいコンテナー・ランタイムとして Docker を置き換えるものです。 [`containerd` リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/containerd/containerd/releases/tag/v1.1.2) を参照してください。 実行する必要があるアクションについては、[コンテナー・ランタイムとしての `containerd` への更新](/docs/containers?topic=containers-cs_versions#containerd)を参照してください。</td>
</tr>
<tr>
<td>Docker</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`containerd` は、パフォーマンスを向上させるために、Kubernetes の新しいコンテナー・ランタイムとして Docker を置き換えるものです。 実行する必要があるアクションについては、[コンテナー・ランタイムとしての `containerd` への更新](/docs/containers?topic=containers-cs_versions#containerd)を参照してください。</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.14</td>
<td>v3.2.18</td>
<td>[etcd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coreos/etcd/releases/v3.2.18) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.5-118</td>
<td>v1.11.2-60</td>
<td>Kubernetes 1.11 リリースをサポートするように更新されました。 さらに、ロード・バランサー・ポッドで、新しいポッドの優先度クラス `ibm-app-cluster-critical` が使用されるようになりました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>334</td>
<td>338</td>
<td>`incubator` のバージョンが 1.8 に更新されました。 ファイル・ストレージは、選択した特定のゾーンにプロビジョンされます。 複数ゾーン・クラスターを使用しており、[地域とゾーンのラベルを追加](/docs/containers?topic=containers-kube_concepts#storage_multizone)する必要がある場合を除いて、既存の (静的) PV インスタンスのラベルを更新することはできません。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.11.2</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.2) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスターの Kubernetes API サーバーの OpenID Connect 構成が、{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) アクセス・グループをサポートするように更新されました。 クラスターの Kubernetes API サーバーの `--enable-admission-plugins` オプションに `Priority` が追加され、ポッドの優先度をサポートするようにクラスターが構成されるようになりました。 詳しくは、以下を参照してください。
<ul><li>[{{site.data.keyword.Bluemix_notm}}IAM アクセス・グループ](/docs/containers?topic=containers-users#rbac)</li>
<li>[ポッドの優先度の構成](/docs/containers?topic=containers-pod_priority#pod_priority)</li></ul></td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.2</td>
<td>v.1.5.4</td>
<td>`heapster-nanny` コンテナーのリソース限度が増やされました。 [Kubernetes Heapster リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/heapster/releases/tag/v1.5.4) を参照してください。</td>
</tr>
<tr>
<td>ロギング構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>コンテナー・ログ・ディレクトリーが、以前の `/var/lib/docker/containers/` から `/var/log/pods/` になりました。</td>
</tr>
</tbody>
</table>

<br />


## アーカイブ
{: #changelog_archive}

サポートされない Kubernetes バージョン:
*  [バージョン 1.10](#110_changelog)
*  [バージョン 1.9](#19_changelog)
*  [バージョン 1.8](#18_changelog)
*  [バージョン 1.7](#17_changelog)

### バージョン 1.10 の変更ログ (2019 年 5 月 16 日付けでサポート対象外)
{: #110_changelog}

バージョン 1.10 の変更ログをまとめます。
{: shortdesc}

*   [ワーカー・ノード・フィックスパック 1.10.13_1558 (2019 年 5 月 13 日リリース) の変更ログ](#11013_1558)
*   [ワーカー・ノード・フィックスパック 1.10.13_1557 (2019 年 4 月 29 日リリース) の変更ログ](#11013_1557)
*   [ワーカー・ノード・フィックスパック 1.10.13_1556 (2019 年 4 月 15 日リリース) の変更ログ](#11013_1556)
*   [1.10.13_1555 (2019 年 4 月 8 日リリース) の変更ログ](#11013_1555)
*   [ワーカー・ノード・フィックスパック 1.10.13_1554 (2019 年 4 月 1 日リリース) の変更ログ](#11013_1554)
*   [マスター・フィックスパック 1.10.13_1553 (2019 年 3 月 26 日リリース) の変更ログ](#11118_1553)
*   [1.10.13_1551 (2019 年 3 月 20 日リリース) の変更ログ](#11013_1551)
*   [1.10.13_1548 (2019 年 3 月 4 日リリース) の変更ログ](#11013_1548)
*   [ワーカー・ノード・フィックスパック 1.10.12_1546 (2019 年 2 月 27 日リリース) の変更ログ](#11012_1546)
*   [ワーカー・ノード・フィックスパック 1.10.12_1544 (2019 年 2 月 15 日リリース) の変更ログ](#11012_1544)
*   [1.10.12_1543 (2019 年 2 月 5 日リリース) の変更ログ](#11012_1543)
*   [ワーカー・ノード・フィックスパック 1.10.12_1541 (2019 年 1 月 28 日リリース) の変更ログ](#11012_1541)
*   [1.10.12_1540 (2019 年 1 月 21 日リリース) の変更ログ](#11012_1540)
*   [ワーカー・ノード・フィックスパック 1.10.11_1538 (2019 年 1 月 7 日リリース) の変更ログ](#11011_1538)
*   [ワーカー・ノード・フィックスパック 1.10.11_1537 (2018 年 12 月 17 日リリース) の変更ログ](#11011_1537)
*   [1.10.11_1536 (2018 年 12 月 4 日リリース) の変更ログ](#11011_1536)
*   [ワーカー・ノード・フィックスパック 1.10.8_1532 (2018 年 11 月 27 日リリース) の変更ログ](#1108_1532)
*   [ワーカー・ノード・フィックスパック 1.10.8_1531 (2018 年 11 月 19 日リリース) の変更ログ](#1108_1531)
*   [1.10.8_1530 (2018 年 11 月 7 日リリース) の変更ログ](#1108_1530_ha-master)
*   [ワーカー・ノード・フィックスパック 1.10.8_1528 (2018 年 10 月 26 日リリース) の変更ログ](#1108_1528)
*   [ワーカー・ノード・フィックスパック 1.10.8_1525 (2018 年 10 月 10 日リリース) の変更ログ](#1108_1525)
*   [1.10.8_1524 (2018 年 10 月 2 日リリース) の変更ログ](#1108_1524)
*   [ワーカー・ノード・フィックスパック 1.10.7_1521 (2018 年 9 月 20 日リリース) の変更ログ](#1107_1521)
*   [1.10.7_1520 (2018 年 9 月 4 日リリース) の変更ログ](#1107_1520)
*   [ワーカー・ノード・フィックスパック 1.10.5_1519 (2018 年 8 月 23 日リリース) の変更ログ](#1105_1519)
*   [ワーカー・ノード・フィックスパック 1.10.5_1518 (2018 年 8 月 13 日リリース) の変更ログ](#1105_1518)
*   [1.10.5_1517 (2018 年 7 月 27 日リリース) の変更ログ](#1105_1517)
*   [ワーカー・ノード・フィックスパック 1.10.3_1514 (2018 年 7 月 3 日リリース) の変更ログ](#1103_1514)
*   [ワーカー・ノード・フィックスパック 1.10.3_1513 (2018 年 6 月 21 日リリース) の変更ログ](#1103_1513)
*   [1.10.3_1512 (2018 年 6 月 12 日リリース) の変更ログ](#1103_1512)
*   [ワーカー・ノード・フィックスパック 1.10.1_1510 (2018 年 5 月 18 日リリース) の変更ログ](#1101_1510)
*   [ワーカー・ノード・フィックスパック 1.10.1_1509 (2018 年 5 月 16 日リリース) の変更ログ](#1101_1509)
*   [1.10.1_1508 (2018 年 5 月 1 日リリース) の変更ログ](#1101_1508)

#### ワーカー・ノード・フィックスパック 1.10.13_1558 (2019 年 5 月 13 日リリース) の変更ログ
{: #11013_1558}

以下の表に、ワーカー・ノードのフィックスパック 1.10.13_1558 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.13_1557 からの変更点">
<caption>バージョン 1.10.13_1557 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター HA プロキシー</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>[HAProxy リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.haproxy.org/download/1.9/src/CHANGELOG) を参照してください。 更新により、[CVE-2019-6706 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706) が解決されます。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.13_1557 (2019 年 4 月 29 日リリース) の変更ログ
{: #11013_1557}

以下の表に、ワーカー・ノードのフィックスパック 1.10.13_1557 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.13_1556 からの変更点">
<caption>1.10.13_1556 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>インストール済み Ubuntu パッケージが更新されました。</td>
</tr>
</tbody>
</table>


#### ワーカー・ノード・フィックスパック 1.10.13_1556 (2019 年 4 月 15 日リリース) の変更ログ
{: #11013_1556}

以下の表に、ワーカー・ノードのフィックスパック 1.10.13_1556 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.13_1555 からの変更点">
<caption>1.10.13_1555 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>[CVE-2019-3842 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html) の `systemd` を含むインストール済み Ubuntu パッケージの更新。</td>
</tr>
</tbody>
</table>

#### 1.10.13_1555 (2019 年 4 月 8 日リリース) の変更ログ
{: #11013_1555}

以下の表に、パッチ 1.10.13_1555 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.13 からの変更点">
<caption>バージョン 1.10.13_1554 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター HA プロキシー</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>[HAProxy リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.haproxy.org/download/1.9/src/CHANGELOG) を参照してください。 更新により、[CVE-2018-0732 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732)、[CVE-2018-0734 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734)、[CVE-2018-0737 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737)、[CVE-2018-5407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407)、[CVE-2019-1543 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543)、および [CVE-2019-1559 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) が解決されます。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>[Kubernetes DNS リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/dns/releases/tag/1.14.13) を参照してください。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>[CVE-2017-12447 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>Ubuntu 16.04 カーネル</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>[CVE-2019-9213 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>Ubuntu 18.04 カーネル</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>[CVE-2019-9213 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.13_1554 (2019 年 4 月 1 日リリース) の変更ログ
{: #11013_1554}

以下の表に、ワーカー・ノードのフィックスパック 1.10.13_1554 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.13_1553 からの変更点">
<caption>バージョン 1.10.13_1553 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>ワーカー・ノードのリソースの使用方法</td>
<td>該当なし</td>
<td>該当なし</td>
<td>kubelet と containerd がリソース不足にならないように、これらのコンポーネント用のメモリー予約が増加されました。 詳しくは、[ワーカー・ノードのリソースの予約](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)を参照してください。</td>
</tr>
</tbody>
</table>


#### マスター・フィックスパック 1.10.13_1553 (2019 年 3 月 26 日リリース) の変更ログ
{: #11118_1553}

以下の表に、マスターのフィックスパック 1.10.13_1553 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.13_1551 からの変更点">
<caption>バージョン 1.10.13_1551 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>345</td>
<td>346</td>
<td>[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>166</td>
<td>167</td>
<td>Kubernetes シークレットを管理するための `context deadline exceeded` および `timeout` で断続的に発生するエラーが修正されました。 また、既存の Kubernetes シークレットが暗号化されないままになることがある鍵管理サービスの更新も修正されました。 更新には、[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) の修正が含まれます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} プロバイダー用のロード・バランサーとロード・バランサー・モニター</td>
<td>143</td>
<td>146</td>
<td>[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

#### 1.10.13_1551 (2019 年 3 月 20 日リリース) の変更ログ
{: #11013_1551}

以下の表に、パッチ 1.10.13_1551 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.13_1548 からの変更点">
<caption>バージョン 1.10.13_1548 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター HA プロキシー構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>断続的に発生するクラスター・マスターへの接続エラーへの対応を向上させるために構成が更新されました。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>GPU ドライバーが [418.43 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.nvidia.com/object/unix.html) に更新されました。 更新には、[CVE-2019-9741 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html) の修正が含まれます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>344</td>
<td>345</td>
<td>[プライベート・サービス・エンドポイント](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)のサポートが追加されました。</td>
</tr>
<tr>
<td>カーネル</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>[CVE-2019-6133 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>136</td>
<td>166</td>
<td>[CVE-2018-16890 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890)、[CVE-2019-3822 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822)、および [CVE-2019-3823 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>[CVE-2018-10779 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779)、[CVE-2018-12900 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900)、[CVE-2018-17000 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000)、[CVE-2018-19210 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210)、[CVE-2019-6128 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128)、および [CVE-2019-7663 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

#### 1.10.13_1548 (2019 年 3 月 4 日リリース) の変更ログ
{: #11013_1548}

以下の表に、パッチ 1.10.13_1548 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.12_1546 からの変更点">
<caption>バージョン 1.10.12_1546 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>[CVE-2019-6454 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.12-252</td>
<td>v1.10.13-288</td>
<td>Kubernetes 1.10.13 リリースをサポートするように更新されました。 `externalTrafficPolicy` を `local` に設定したロード・バランサーに定期的に発生していた接続の問題が修正されました。 最新の {{site.data.keyword.Bluemix_notm}} ドキュメント・リンクを使用するようにロード・バランサーのイベントが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>342</td>
<td>344</td>
<td>イメージの基本オペレーティング・システムが Fedora から Alpine に変更されました。 [CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>122</td>
<td>136</td>
<td>クライアントのタイムアウトが {{site.data.keyword.keymanagementservicefull_notm}} に増やされました。 [CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.12</td>
<td>v1.10.13</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.13) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>該当なし</td>
<td>該当なし</td>
<td>より多くのクラスター・サービスを処理するために、Kubernetes DNS ポッドのメモリー制限が `170 Mi` から `400 Mi` に増えました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} プロバイダー用のロード・バランサーとロード・バランサー・モニター</td>
<td>132</td>
<td>143</td>
<td>[CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>OpenVPN クライアントとサーバー</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>[CVE-2019-1559 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>トラステッド・コンピューティング・エージェント</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>[CVE-2019-6454 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.12_1546 (2019 年 2 月 27 日リリース) の変更ログ
{: #11012_1546}

以下の表に、ワーカー・ノードのフィックスパック 1.10.12_1546 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.12_1544 からの変更点">
<caption>バージョン 1.10.12_1544 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>[CVE-2018-19407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.12_1544 (2019 年 2 月 15 日リリース) の変更ログ
{: #11012_1544}

以下の表に、ワーカー・ノードのフィックスパック 1.10.12_1544 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.12_1543 からの変更点">
<caption>バージョン 1.10.12_1543 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>18.06.1-ce</td>
<td>18.06.2-ce</td>
<td>[Docker Community Edition リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/docker/docker-ce/releases/tag/v18.06.2-ce) を参照してください。 更新により、[CVE-2019-5736 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736) が解決されます。</td>
</tr>
<tr>
<td>Kubernetes の `kubelet` 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`ExperimentalCriticalPodAnnotation` 機能ゲートで静的ポッドの重大な強制除去の発生を防止できるようになりました。</td>
</tr>
</tbody>
</table>

#### 1.10.12_1543 (2019 年 2 月 5 日リリース) の変更ログ
{: #11012_1543}

以下の表に、パッチ 1.10.12_1543 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.12_1541 からの変更点">
<caption>バージョン 1.10.12_1541 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>[etcd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coreos/etcd/releases/v3.3.11) を参照してください。 さらに、etcd でサポートされる暗号スイートが、強力な暗号化 (128 ビット以上) を使用するサブセットだけに制限されるようになりました。</td>
</tr>
<tr>
<td>GPU デバイスのプラグインおよびインストーラー</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>[CVE-2019-3462 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) および [CVE-2019-6486 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>338</td>
<td>342</td>
<td>ファイル・ストレージ・プラグインが次のように更新されました。
<ul><li>[ボリューム・トポロジー対応スケジューリング](/docs/containers?topic=containers-file_storage#file-topology)による動的プロビジョニングをサポートします。</li>
<li>ストレージが既に削除されている場合は、永続ボリューム請求 (PVC) 削除エラーを無視します。</li>
<li>失敗した PVC に失敗メッセージのアノテーションを追加します。</li>
<li>ストレージ・プロビジョナー・コントローラーのリーダー選択と再同期期間の設定を最適化し、プロビジョニング・タイムアウトを 30 分から 1 時間に増やします。</li>
<li>プロビジョニングを開始する前にユーザー許可を確認します。</li></ul></td>
</tr>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>111</td>
<td>122</td>
<td>Kubernetes シークレットが {{site.data.keyword.keymanagementservicefull_notm}} によって管理されている場合の一時的な失敗を回避するための再試行ロジックが追加されました。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>Kubernetes API サーバーの監査ポリシー構成が更新され、`cluster-admin` 要求のメタデータのロギングと、ワークロードの`作成`、`更新`、および`パッチ`要求の本体のロギングが含まれるようになりました。</td>
</tr>
<tr>
<td>OpenVPN クライアント</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) および [CVE-2018-5407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) への対策としてイメージが更新されました。 さらに、ポッド構成が構成マップからではなくシークレットから取得されるようになりました。</td>
</tr>
<tr>
<td>OpenVPN サーバー</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>[CVE-2018-0734 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) および [CVE-2018-5407 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>[CVE-2018-16864 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864) のセキュリティー・パッチ。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.12_1541 (2019 年 1 月 28 日リリース) の変更ログ
{: #11012_1541}

以下の表に、ワーカー・ノードのフィックスパック 1.10.12_1541 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.12_1540 からの変更点">
<caption>バージョン 1.10.12_1540 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>[CVE-2019-3462 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) および [USN-3863-1 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://usn.ubuntu.com/3863-1) の `apt` を含むインストール済み Ubuntu パッケージの更新。</td>
</tr>
</tbody>
</table>

#### 1.10.12_1540 (2019 年 1 月 21 日リリース) の変更ログ
{: #11012_1540}

以下の表に、パッチ 1.10.12_1540 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.11_1538 からの変更点">
<caption>バージョン 1.10.11_1538 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.11-219</td>
<td>v1.10.12-252</td>
<td>Kubernetes 1.10.12 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.11</td>
<td>v1.10.12</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.12) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes アドオン・サイズ変更機能</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>[Kubernetes アドオン・サイズ変更機能のリリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes ダッシュボード</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>[Kubernetes ダッシュボードのリリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1) を参照してください。 更新により、[CVE-2018-18264 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264) が解決されます。<br><br>`kubectl proxy` でダッシュボードにアクセスすると、ログイン・ページ上の**「スキップ (SKIP)」**ボタンが削除されます。 代わりに、[**「トークン (Token)」**を使用してログインしてください](/docs/containers?topic=containers-app#cli_dashboard)。</td>
</tr>
<tr>
<td>GPU インストーラー</td>
<td>390.12</td>
<td>410.79</td>
<td>インストール済み GPU ドライバーが 410.79 に更新されました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.11_1538 (2019 年 1 月 7 日リリース) の変更ログ
{: #11011_1538}

以下の表に、ワーカー・ノードのフィックスパック 1.10.11_1538 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.11_1537 からの変更点">
<caption>バージョン 1.10.11_1537 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>[CVE-2017-5753、CVE-2018-18690 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.11_1537 (2018 年 12 月 17 日リリース) の変更ログ
{: #11011_1537}

以下の表に、ワーカー・ノードのフィックスパック 1.10.11_1537 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.11_1536 からの変更点">
<caption>バージョン 1.10.11_1536 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>インストール済み Ubuntu パッケージが更新されました。</td>
</tr>
</tbody>
</table>


#### 1.10.11_1536 (2018 年 12 月 4 日リリース) の変更ログ
{: #11011_1536}

以下の表に、パッチ 1.10.11_1536 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.8_1532 からの変更点">
<caption>バージョン 1.10.8_1532 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>[Calico リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.3/releases/#v331) を参照してください。 更新により、[Tigera Technical Advisory TTA-2018-001 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.projectcalico.org/security-bulletins/) が解決されます。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.8-197</td>
<td>v1.10.11-219</td>
<td>Kubernetes 1.10.11 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.8</td>
<td>v1.10.11</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.11) を参照してください。 更新により、[CVE-2018-1002105 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/issues/71411) が解決されます。</td>
</tr>
<tr>
<td>OpenVPN クライアントとサーバー</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>[CVE-2018-0732 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) および [CVE-2018-0737 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737) への対策としてイメージが更新されました。</td>
</tr>
<tr>
<td>ワーカー・ノードのリソースの使用方法</td>
<td>該当なし</td>
<td>該当なし</td>
<td>kubelet と docker がリソース不足にならないように、これらのコンポーネント用に専用の cgroup が追加されました。 詳しくは、[ワーカー・ノードのリソースの予約](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)を参照してください。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.8_1532 (2018 年 11 月 27 日リリース) の変更ログ
{: #1108_1532}

以下の表に、ワーカー・ノードのフィックスパック 1.10.8_1532 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.8_1531 からの変更点">
<caption>バージョン 1.10.8_1531 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>[Docker リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.docker.com/engine/release-notes/#18061-ce) を参照してください。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.8_1531 (2018 年 11 月 19 日リリース) の変更ログ
{: #1108_1531}

以下の表に、ワーカー・ノードのフィックスパック 1.10.8_1531 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.8_1530 からの変更点">
<caption>バージョン 1.10.8_1530 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>[CVE-2018-7755 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

#### 1.10.8_1530 (2018 年 11 月 7 日リリース) の変更ログ
{: #1108_1530_ha-master}

以下の表に、パッチ 1.10.8_1530 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.8_1528 からの変更点">
<caption>バージョン 1.10.8_1528 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター・マスター</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスター・マスターの構成が更新され、高可用性 (HA) が向上しました。 現在のクラスターは、3 台の Kubernetes マスター・レプリカがそれぞれ別の物理ホスト上にデプロイされる高可用性 (HA) 構成でセットアップされるようになりました。 さらに、クラスターが複数ゾーン対応ゾーンにある場合は、それらのマスターがゾーン間に分散されます。<br>実行する必要があるアクションについては、[高可用性クラスター・マスターへの更新](/docs/containers?topic=containers-cs_versions#ha-masters)を参照してください。 これらの準備アクションは、以下の場合に適用されます。<ul>
<li>ファイアウォールまたはカスタム Calico ネットワーク・ポリシーがある場合。</li>
<li>ワーカー・ノードでホスト・ポート `2040` または `2041` を使用している場合。</li>
<li>マスターへのクラスター内アクセス用にクラスターのマスター IP アドレスを使用していた場合。</li>
<li>Calico ポリシーの作成などのために、Calico API または CLI (`calicoctl`) を呼び出す自動化機能がある場合。</li>
<li>Kubernetes または Calico ネットワーク・ポリシーを使用して、ポッドからマスターへの発信アクセスを制御している場合。</li></ul></td>
</tr>
<tr>
<td>クラスター・マスター HA プロキシー</td>
<td>該当なし</td>
<td>1.8.12-alpine</td>
<td>すべてのワーカー・ノードにクライアント・サイド・ロード・バランシングのための `ibm-master-proxy-*` ポッドが追加されたので、各ワーカー・ノード・クライアントが、使用可能な HA マスター・レプリカに要求をルーティングできるようになりました。</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>[etcd リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/coreos/etcd/releases/v3.3.1) を参照してください。</td>
</tr>
<tr>
<td>etcd のデータの暗号化</td>
<td>該当なし</td>
<td>該当なし</td>
<td>以前は、etcd データは、保存時に暗号化が行われるマスターの NFS ファイル・ストレージ・インスタンスに保管されていました。 現在は、etcd データはマスターのローカル・ディスクに保管され、{{site.data.keyword.cos_full_notm}} にバックアップされます。 {{site.data.keyword.cos_full_notm}} に転送中のデータも保存されたデータも暗号化されています。 しかし、マスターのローカル・ディスク上の etcd データは暗号化されません。 マスターのローカル etcd データを暗号化する場合は、[クラスター内で {{site.data.keyword.keymanagementservicelong_notm}} を有効にします](/docs/containers?topic=containers-encryption#keyprotect)。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.8-172</td>
<td>v1.10.8-197</td>
<td>ロード・バランサー・サービスのデプロイ先の VLAN を指定する `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` アノテーションが追加されました。 クラスター内の使用可能な VLAN を表示するには、`ibmcloud ks vlans --zone <zone>` を実行します。</td>
</tr>
<tr>
<td>TPM 対応カーネル</td>
<td>該当なし</td>
<td>該当なし</td>
<td>トラステッド・コンピューティング用の TPM チップを搭載したベアメタルのワーカー・ノードでは、トラストを有効にするまで、デフォルトの Ubuntu カーネルが使用されます。 既存のクラスターで[トラストを有効にした](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)場合には、TPM チップを搭載した既存のベアメタルのワーカー・ノードを[再ロード](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)する必要があります。 ベアメタルのワーカー・ノードに TPM チップが搭載されているかどうかを確認するには、 `ibmcloud ks machine-types --zone` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)の実行後に、**Trustable** フィールドを確認します。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.8_1528 (2018 年 10 月 26 日リリース) の変更ログ
{: #1108_1528}

以下の表に、ワーカー・ノードのフィックスパック 1.10.8_1528 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.8_1527 からの変更点">
<caption>バージョン 1.10.8_1527 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>OS 割り込み処理</td>
<td>該当なし</td>
<td>該当なし</td>
<td>割り込み要求 (IRQ) システム・デーモンが、さらにパフォーマンスの高い割り込みハンドラーに置き換えられました。</td>
</tr>
</tbody>
</table>

#### マスター・フィックスパック 1.10.8_1527 (2018 年 10 月 15 日リリース) の変更ログ
{: #1108_1527}

以下の表に、マスターのフィックスパック 1.10.8_1527 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.8_1524 からの変更点">
<caption>バージョン 1.10.8_1524 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ノードの障害を処理しやすくするために `calico-node` コンテナーの Readiness Probe が修正されました。</td>
</tr>
<tr>
<td>クラスター更新</td>
<td>該当なし</td>
<td>該当なし</td>
<td>サポートされないバージョンからのマスター更新時に、クラスター・アドオンの更新で発生していた問題が修正されました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.8_1525 (2018 年 10 月 10 日リリース) の変更ログ
{: #1108_1525}

以下の表に、ワーカー・ノードのフィックスパック 1.10.8_1525 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.8_1524 からの変更点">
<caption>バージョン 1.10.8_1524 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>[CVE-2018-14633、CVE-2018-17182 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>非アクティブ・セッションのタイムアウト</td>
<td>該当なし</td>
<td>該当なし</td>
<td>コンプライアンスの理由により非アクティブ・セッションのタイムアウトが 5 分に設定されました。</td>
</tr>
</tbody>
</table>


#### 1.10.8_1524 (2018 年 10 月 2 日リリース) の変更ログ
{: #1108_1524}

以下の表に、パッチ 1.10.8_1524 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.7_1520 からの変更点">
<caption>バージョン 1.10.7_1520 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>鍵管理サービス・プロバイダー</td>
<td>該当なし</td>
<td>該当なし</td>
<td>{{site.data.keyword.keymanagementservicefull}} をサポートするため、クラスターで Kubernetes 鍵管理サービス (KMS) プロバイダーを使用する機能が追加されました。 [クラスターで {{site.data.keyword.keymanagementserviceshort}} を有効にする](/docs/containers?topic=containers-encryption#keyprotect)と、すべての Kubernetes シークレットが暗号化されます。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.7</td>
<td>v1.10.8</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.8) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes DNS autoscaler</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>[Kubernetes DNS autoscaler リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.7-146</td>
<td>v1.10.8-172</td>
<td>Kubernetes 1.10.8 リリースをサポートするように更新されました。 また、ロード・バランサーのエラー・メッセージ内の文書リンクが更新されました。</td>
</tr>
<tr>
<td>IBM ファイル・ストレージ・クラス</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ワーカー・ノードで提供されるデフォルトを使用するように、IBM ファイル・ストレージ・クラスの `mountOptions` が削除されました。 IBM ファイル・ストレージ・クラスの重複 `reclaimPolicy` パラメーターが削除されました。<br><br>
また、クラスター・マスターの更新時に、デフォルトの IBM ファイル・ストレージ・クラスが変更されなくなりました。 デフォルトのストレージ・クラスを変更する場合は、`kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` を実行して、`<storageclass>` をストレージ・クラスの名前に置き換えます。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.7_1521 (2018 年 9 月 20 日リリース) の変更ログ
{: #1107_1521}

以下の表に、ワーカー・ノードのフィックスパック 1.10.7_1521 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.7_1520 からの変更点">
<caption>バージョン 1.10.7_1520 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>ログの循環</td>
<td>該当なし</td>
<td>該当なし</td>
<td>90 日内に再ロードまたは更新されないワーカー・ノードで `logrotate` が失敗することを防止するため、`cronjobs` の代わりに `systemd` タイマーを使用するように切り替えられました。 **注**: このマイナー・リリースの前のすべてのバージョンでは、ログが循環されないため、cron ジョブが失敗した後に 1 次ディスクが満杯になります。 ワーカー・ノードが更新も再ロードもされずに 90 日間アクティブだった場合、cron ジョブは失敗します。 ログで 1 次ディスクが満杯になった場合、ワーカー・ノードは失敗状態になります。 ワーカー・ノードを修正するには、`ibmcloud ks worker-reload` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)または `ibmcloud ks worker-update` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)を使用できます。</td>
</tr>
<tr>
<td>ワーカー・ノード・ランタイム・コンポーネント (`kubelet`、`kube-proxy`、`docker`)</td>
<td>該当なし</td>
<td>該当なし</td>
<td>1 次ディスクでのランタイム・コンポーネントの従属関係が削除されました。 この機能強化によって、1 次ディスクが満杯になった場合にワーカー・ノードが失敗することが回避されます。</td>
</tr>
<tr>
<td>ルート・パスワードの有効期限</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ワーカー・ノードのルート・パスワードは、コンプライアンスの理由により 90 日後に有効期限が切れます。 自動化ツールがワーカー・ノードに root としてログインする必要がある場合や、root として実行される cron ジョブを使用している場合は、ワーカー・ノードにログインして `chage -M -1 root` を実行することによって、パスワード有効期限を無効にすることができます。 **注**: セキュリティー・コンプライアンスに関する要件で、root として実行したり、パスワード有効期限を削除したりすることを避ける必要がある場合は、有効期限を無効にしないでください。 代わりに、少なくとも 90 日ごとにワーカー・ノードを[更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)または[再ロード](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)してください。</td>
</tr>
<tr>
<td>systemd</td>
<td>該当なし</td>
<td>該当なし</td>
<td>一時的なマウント装置が無制限に増えることを防止するため、それらを定期的に削除します。 このアクションは、[Kubernetes 問題 57345 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/issues/57345) に対処するためのものです。</td>
</tr>
<tr>
<td>Docker</td>
<td>該当なし</td>
<td>該当なし</td>
<td>デフォルトの Docker ブリッジが無効にされたため、`172.17.0.0/16` IP 範囲がプライベート経路で使用されるようになりました。 ホストで `docker` コマンドを直接実行することによって、または Docker ソケットをマウントするポッドを使用することによって、ワーカー・ノードで Docker コンテナーを作成している場合は、以下のオプションから選択してください。<ul><li>コンテナー作成時に外部ネットワーク接続を確実にするため、`docker build . --network host` を実行します。</li>
<li>コンテナー作成時に使用するネットワークを明示的に作成するため、`docker network create` を実行して、このネットワークを使用します。</li></ul>
**注**: Docker ソケットまたは Docker に直接依存していますか? [コンテナー・ランタイムとして `docker` の代わりに `containerd` を使用するように更新](/docs/containers?topic=containers-cs_versions#containerd)して、クラスターで Kubernetes バージョン 1.11 以降を実行できるようにしてください。</td>
</tr>
</tbody>
</table>

#### 1.10.7_1520 (2018 年 9 月 4 日リリース) の変更ログ
{: #1107_1520}

以下の表に、パッチ 1.10.7_1520 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.5_1519 からの変更点">
<caption>バージョン 1.10.5_1519 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>Calico [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.2/releases/#v321) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.5-118</td>
<td>v1.10.7-146</td>
<td>Kubernetes 1.10.7 リリースをサポートするように更新されました。 さらに、`externalTrafficPolicy` が `local` に設定されて、ロード・バランサー・サービスの更新をより良好に処理できるようにクラウド・プロバイダー構成が変更されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>334</td>
<td>338</td>
<td>incubator のバージョンが 1.8 に更新されました。 ファイル・ストレージは、選択した特定のゾーンにプロビジョンされます。 複数ゾーン・クラスターを使用しており、地域とゾーンのラベルを追加する必要がある場合を除いて、既存の (静的) PV インスタンスのラベルを更新することはできません。<br><br> IBM 提供のファイル・ストレージ・クラスのマウント・オプションから、デフォルトの NFS バージョンが削除されました。 ホストのオペレーティング・システムによって、NFS バージョンが、IBM Cloud インフラストラクチャー (SoftLayer) の NFS サーバーとネゴシエーションされるようになりました。 特定の NFS バージョンを手動で設定する場合、またはホストのオペレーティング・システムによってネゴシエーションされた PV の NFS バージョンを変更する場合は、[デフォルトの NFS バージョンの変更](/docs/containers?topic=containers-file_storage#nfs_version_class)を参照してください。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.10.7</td>
<td>Kubernetes [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.7) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes Heapster 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`heapster-nanny` コンテナーのリソース限度が増やされました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.5_1519 (2018 年 8 月 23 日リリース) の変更ログ
{: #1105_1519}

以下の表に、ワーカー・ノードのフィックスパック 1.10.5_1519 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.5_1518 からの変更点">
<caption>バージョン 1.10.5_1518 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>`cgroup` リークを修正するように `systemd` が更新されました。</td>
</tr>
<tr>
<td>カーネル</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>[CVE-2018-3620、CVE-2018-3646 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://usn.ubuntu.com/3741-1/) に関連するカーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>


#### ワーカー・ノード・フィックスパック 1.10.5_1518 (2018 年 8 月 13 日リリース) の変更ログ
{: #1105_1518}

以下の表に、ワーカー・ノードのフィックスパック 1.10.5_1518 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.5_1517 からの変更点">
<caption>バージョン 1.10.5_1517 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>インストール済み Ubuntu パッケージが更新されました。</td>
</tr>
</tbody>
</table>

#### 1.10.5_1517 (2018 年 7 月 27 日リリース) の変更ログ
{: #1105_1517}

以下の表に、パッチ 1.10.5_1517 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.3_1514 からの変更点">
<caption>バージョン 1.10.3_1514 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.1</td>
<td>v3.1.3</td>
<td>Calico [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.1/releases/#v313) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.3-85</td>
<td>v1.10.5-118</td>
<td>Kubernetes 1.10.5 リリースをサポートするように更新されました。 また、LoadBalancer サービス `create failure` イベントで、ポータブル・サブネット・エラーが含まれるようになりました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>320</td>
<td>334</td>
<td>永続ボリューム作成のタイムアウトが 15 分から 30 分に増やされました。 デフォルトの請求タイプが `hourly` に変更されました。 事前定義ストレージ・クラスにマウント・オプションが追加されました。 IBM Cloud インフラストラクチャー (SoftLayer) アカウントの NFS ファイル・ストレージ・インスタンスで、**「メモ」**フィールドが JSON 形式に変更され、PV がデプロイされる Kubernetes 名前空間が追加されました。 マルチゾーン・クラスターをサポートするため、永続ボリュームにゾーンと地域のラベルが追加されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.3</td>
<td>v1.10.5</td>
<td>Kubernetes [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.5) を参照してください。</td>
</tr>
<tr>
<td>カーネル</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ハイパフォーマンス・ネットワーキング・ワークロードのために、ワーカー・ノード・ネットワーク設定がわずかに改善されました。</td>
</tr>
<tr>
<td>OpenVPN クライアント</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kube-system` 名前空間で実行される OpenVPN クライアント `vpn` デプロイメントは、Kubernetes `addon-manager` によって管理されるようになりました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.3_1514 (2018 年 7 月 3 日リリース) の変更ログ
{: #1103_1514}

以下の表に、ワーカー・ノードのフィックスパック 1.10.3_1514 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.3_1513 からの変更点">
<caption>バージョン 1.10.3_1513 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ハイパフォーマンス・ネットワーキング・ワークロードのために、`sysctl` が最適化されました。</td>
</tr>
</tbody>
</table>


#### ワーカー・ノード・フィックスパック 1.10.3_1513 (2018 年 6 月 21 日リリース) の変更ログ
{: #1103_1513}

以下の表に、ワーカー・ノードのフィックスパック 1.10.3_1513 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.3_1512 からの変更点">
<caption>バージョン 1.10.3_1512 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>該当なし</td>
<td>該当なし</td>
<td>非暗号化マシン・タイプの場合、ワーカー・ノードを再ロードまたは更新するときに新しいファイル・システムを取得することによって、2 次ディスクがクリーンアップされるようになりました。</td>
</tr>
</tbody>
</table>

#### 1.10.3_1512 (2018 年 6 月 12 日リリース) の変更ログ
{: #1103_1512}

以下の表に、パッチ 1.10.3_1512 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.1_1510 からの変更点">
<caption>バージョン 1.10.1_1510 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.10.1</td>
<td>v1.10.3</td>
<td>Kubernetes [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.3) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスターの Kubernetes API サーバーの `--enable-admission-plugins` オプションに `PodSecurityPolicy` が追加され、ポッドのセキュリティー・ポリシーをサポートするようにクラスターが構成されるようになりました。 詳しくは、[ポッド・セキュリティー・ポリシーの構成](/docs/containers?topic=containers-psp) を参照してください。</td>
</tr>
<tr>
<td>Kubelet 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kubelet` HTTPS エンドポイントを認証する API ベアラー・トークンとサービス・アカウント・トークンをサポートするため、`--authentication-token-webhook` オプションが使用可能になりました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.10.1-52</td>
<td>v1.10.3-85</td>
<td>Kubernetes 1.10.3 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>OpenVPN クライアント</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kube-system` 名前空間で実行される OpenVPN クライアント `vpn` デプロイメントに `livenessProbe` が追加されました。</td>
</tr>
<tr>
<td>カーネル更新</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>[CVE-2018-3639 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639) に対して、カーネル更新を使用して新規ワーカー・ノード・イメージが導入されました。</td>
</tr>
</tbody>
</table>



#### ワーカー・ノード・フィックスパック 1.10.1_1510 (2018 年 5 月 18 日リリース) の変更ログ
{: #1101_1510}

以下の表に、ワーカー・ノードのフィックスパック 1.10.1_1510 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.1_1509 からの変更点">
<caption>バージョン 1.10.1_1509 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ブロック・ストレージ・プラグインを使用したときに発生するバグに対応するための修正です。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.10.1_1509 (2018 年 5 月 16 日リリース) の変更ログ
{: #1101_1509}

以下の表に、ワーカー・ノードのフィックスパック 1.10.1_1509 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.10.1_1508 からの変更点">
<caption>バージョン 1.10.1_1508 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kubelet` ルート・ディレクトリーに保管したデータが、ワーカー・ノード・マシンのより大きい 2 次ディスクに保存されるようになりました。</td>
</tr>
</tbody>
</table>

#### 1.10.1_1508 (2018 年 5 月 1 日リリース) の変更ログ
{: #1101_1508}

以下の表に、パッチ 1.10.1_1508 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.7_1510 からの変更点">
<caption>バージョン 1.9.7_1510 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v3.1.1</td>
<td>Calico [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.1/releases/#v311) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.0</td>
<td>v1.5.2</td>
<td>Kubernetes Heapster [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.10.1</td>
<td>Kubernetes [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスターの Kubernetes API サーバーの <code>--enable-admission-plugins</code> オプションに <code>StorageObjectInUseProtection</code> が追加されました。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>Kubernetes DNS [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/dns/releases/tag/1.14.10) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.7-102</td>
<td>v1.10.1-52</td>
<td>Kubernetes 1.10 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>GPU サポート</td>
<td>該当なし</td>
<td>該当なし</td>
<td>スケジューリングおよび実行で、[グラフィックス・プロセッシング・ユニット (GPU) コンテナー・ワークロード](/docs/containers?topic=containers-app#gpu_app)がサポートされるようになりました。 使用可能な GPU マシン・タイプのリストについては、[ワーカー・ノード用のハードウェア](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes)を参照してください。 詳しくは、[GPU のスケジュール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/) に関する Kubernetes の資料を参照してください。</td>
</tr>
</tbody>
</table>

<br />


### バージョン 1.9 の変更ログ (2018 年 12 月 27 日付けでサポート対象外)
{: #19_changelog}

バージョン 1.9 の変更ログをまとめます。
{: shortdesc}

*   [ワーカー・ノード・フィックスパック 1.9.11_1539 (2018 年 12 月 17 日リリース) の変更ログ](#1911_1539)
*   [ワーカー・ノード・フィックスパック 1.9.11_1538 (2018 年 12 月 4 日リリース) の変更ログ](#1911_1538)
*   [ワーカー・ノード・フィックスパック 1.9.11_1537 (2018 年 11 月 27 日リリース) の変更ログ](#1911_1537)
*   [1.9.11_1536 (2018 年 11 月 19 日リリース) の変更ログ](#1911_1536)
*   [ワーカー・ノード・フィックス 1.9.10_1532 (2018 年 11 月 7 日リリース) の変更ログ](#1910_1532)
*   [ワーカー・ノード・フィックスパック 1.9.10_1531 (2018 年 10 月 26 日リリース) の変更ログ](#1910_1531)
*   [マスター・フィックスパック 1.9.10_1530 (2018 年 10 月 15 日リリース) の変更ログ](#1910_1530)
*   [ワーカー・ノード・フィックスパック 1.9.10_1528 (2018 年 10 月 10 日リリース) の変更ログ](#1910_1528)
*   [1.9.10_1527 (2018 年 10 月 2 日リリース) の変更ログ](#1910_1527)
*   [ワーカー・ノード・フィックスパック 1.9.10_1524 (2018 年 9 月 20 日リリース) の変更ログ](#1910_1524)
*   [1.9.10_1523 (2018 年 9 月 4 日リリース) の変更ログ](#1910_1523)
*   [ワーカー・ノード・フィックスパック 1.9.9_1522 (2018 年 8 月 23 日リリース) の変更ログ](#199_1522)
*   [ワーカー・ノード・フィックスパック 1.9.9_1521 (2018 年 8 月 13 日リリース) の変更ログ](#199_1521)
*   [1.9.9_1520 (2018 年 7 月 27 日リリース) の変更ログ](#199_1520)
*   [ワーカー・ノード・フィックスパック 1.9.8_1517 (2018 年 7 月 3 日リリース) の変更ログ](#198_1517)
*   [ワーカー・ノード・フィックスパック 1.9.8_1516 (2018 年 6 月 21 日リリース) の変更ログ](#198_1516)
*   [1.9.8_1515 (2018 年 6 月 19 日リリース) の変更ログ](#198_1515)
*   [ワーカー・ノード・フィックスパック 1.9.7_1513 (2018 年 6 月 11 日リリース) の変更ログ](#197_1513)
*   [ワーカー・ノード・フィックスパック 1.9.7_1512 (2018 年 5 月 18 日リリース) の変更ログ](#197_1512)
*   [ワーカー・ノード・フィックスパック 1.9.7_1511 (2018 年 5 月 16 日リリース) の変更ログ](#197_1511)
*   [1.9.7_1510 (2018 年 4 月 30 日リリース) の変更ログ](#197_1510)

#### ワーカー・ノード・フィックスパック 1.9.11_1539 (2018 年 12 月 17 日リリース) の変更ログ
{: #1911_1539}

以下の表に、ワーカー・ノードのフィックスパック 1.9.11_1539 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.11_1538 からの変更点">
<caption>バージョン 1.9.11_1538 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>インストール済み Ubuntu パッケージが更新されました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.9.11_1538 (2018 年 12 月 4 日リリース) の変更ログ
{: #1911_1538}

以下の表に、ワーカー・ノードのフィックスパック 1.9.11_1538 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.11_1537 からの変更点">
<caption>バージョン 1.9.11_1537 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>ワーカー・ノードのリソースの使用方法</td>
<td>該当なし</td>
<td>該当なし</td>
<td>kubelet と docker がリソース不足にならないように、これらのコンポーネント用に専用の cgroup が追加されました。 詳しくは、[ワーカー・ノードのリソースの予約](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)を参照してください。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.9.11_1537 (2018 年 11 月 27 日リリース) の変更ログ
{: #1911_1537}

以下の表に、ワーカー・ノードのフィックスパック 1.9.11_1537 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.11_1536 からの変更点">
<caption>バージョン 1.9.11_1536 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>[Docker リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.docker.com/engine/release-notes/#18061-ce) を参照してください。</td>
</tr>
</tbody>
</table>

#### 1.9.11_1536 (2018 年 11 月 19 日リリース) の変更ログ
{: #1911_1536}

以下の表に、パッチ 1.9.11_1536 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.10_1532 からの変更点">
<caption>バージョン 1.9.10_1532 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v2.6.12</td>
<td>[Calico リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v2.6/releases/#v2612) を参照してください。 更新により、[Tigera Technical Advisory TTA-2018-001 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.projectcalico.org/security-bulletins/) が解決されます。</td>
</tr>
<tr>
<td>カーネル</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>[CVE-2018-7755 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog) への対策としてカーネル更新が適用され、ワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.10</td>
<td>v1.9.11</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.11) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.10-219</td>
<td>v1.9.11-249</td>
<td>Kubernetes 1.9.11 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>OpenVPN クライアントとサーバー</td>
<td>2.4.4-r2</td>
<td>2.4.6-r3-IKS-8</td>
<td>[CVE-2018-0732 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) および [CVE-2018-0737 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737) への対策としてイメージが更新されました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックス 1.9.10_1532 (2018 年 11 月 7 日リリース) の変更ログ
{: #1910_1532}

以下の表に、ワーカー・ノードのフィックスパック 1.9.11_1532 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.10_1531 からの変更点">
<caption>バージョン 1.9.10_1531 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>TPM 対応カーネル</td>
<td>該当なし</td>
<td>該当なし</td>
<td>トラステッド・コンピューティング用の TPM チップを搭載したベアメタルのワーカー・ノードでは、トラストを有効にするまで、デフォルトの Ubuntu カーネルが使用されます。 既存のクラスターで[トラストを有効にした](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)場合には、TPM チップを搭載した既存のベアメタルのワーカー・ノードを[再ロード](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)する必要があります。 ベアメタルのワーカー・ノードに TPM チップが搭載されているかどうかを確認するには、 `ibmcloud ks machine-types --zone` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)の実行後に、**Trustable** フィールドを確認します。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.9.10_1531 (2018 年 10 月 26 日リリース) の変更ログ
{: #1910_1531}

以下の表に、ワーカー・ノードのフィックスパック 1.9.10_1531 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.10_1530 からの変更点">
<caption>バージョン 1.9.10_1530 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>OS 割り込み処理</td>
<td>該当なし</td>
<td>該当なし</td>
<td>割り込み要求 (IRQ) システム・デーモンが、さらにパフォーマンスの高い割り込みハンドラーに置き換えられました。</td>
</tr>
</tbody>
</table>

#### マスター・フィックスパック 1.9.10_1530 (2018 年 10 月 15 日リリース) の変更ログ
{: #1910_1530}

以下の表に、ワーカー・ノードのフィックスパック 1.9.10_1530 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.10_1527 からの変更点">
<caption>バージョン 1.9.10_1527 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>クラスター更新</td>
<td>該当なし</td>
<td>該当なし</td>
<td>サポートされないバージョンからのマスター更新時に、クラスター・アドオンの更新で発生していた問題が修正されました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.9.10_1528 (2018 年 10 月 10 日リリース) の変更ログ
{: #1910_1528}

以下の表に、ワーカー・ノードのフィックスパック 1.9.10_1528 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.10_1527 からの変更点">
<caption>バージョン 1.9.10_1527 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>[CVE-2018-14633、CVE-2018-17182 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog) に対して、カーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
<tr>
<td>非アクティブ・セッションのタイムアウト</td>
<td>該当なし</td>
<td>該当なし</td>
<td>コンプライアンスの理由により非アクティブ・セッションのタイムアウトが 5 分に設定されました。</td>
</tr>
</tbody>
</table>


#### 1.9.10_1527 (2018 年 10 月 2 日リリース) の変更ログ
{: #1910_1527}

以下の表に、パッチ 1.9.10_1527 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.10_1523 からの変更点">
<caption>バージョン 1.9.10_1523 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.10-192</td>
<td>v1.9.10-219</td>
<td>ロード・バランサーのエラー・メッセージ内の文書リンクが更新されました。</td>
</tr>
<tr>
<td>IBM ファイル・ストレージ・クラス</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ワーカー・ノードで提供されるデフォルトを使用するように、IBM ファイル・ストレージ・クラスの `mountOptions` が削除されました。 IBM ファイル・ストレージ・クラスの重複 `reclaimPolicy` パラメーターが削除されました。<br><br>
また、クラスター・マスターの更新時に、デフォルトの IBM ファイル・ストレージ・クラスが変更されなくなりました。 デフォルトのストレージ・クラスを変更する場合は、`kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` を実行して、`<storageclass>` をストレージ・クラスの名前に置き換えます。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.9.10_1524 (2018 年 9 月 20 日リリース) の変更ログ
{: #1910_1524}

以下の表に、ワーカー・ノードのフィックスパック 1.9.10_1524 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.10_1523 からの変更点">
<caption>バージョン 1.9.10_1523 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>ログの循環</td>
<td>該当なし</td>
<td>該当なし</td>
<td>90 日内に再ロードまたは更新されないワーカー・ノードで `logrotate` が失敗することを防止するため、`cronjobs` の代わりに `systemd` タイマーを使用するように切り替えられました。 **注**: このマイナー・リリースの前のすべてのバージョンでは、ログが循環されないため、cron ジョブが失敗した後に 1 次ディスクが満杯になります。 ワーカー・ノードが更新も再ロードもされずに 90 日間アクティブだった場合、cron ジョブは失敗します。 ログで 1 次ディスクが満杯になった場合、ワーカー・ノードは失敗状態になります。 ワーカー・ノードを修正するには、`ibmcloud ks worker-reload` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)または `ibmcloud ks worker-update` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)を使用できます。</td>
</tr>
<tr>
<td>ワーカー・ノード・ランタイム・コンポーネント (`kubelet`、`kube-proxy`、`docker`)</td>
<td>該当なし</td>
<td>該当なし</td>
<td>1 次ディスクでのランタイム・コンポーネントの従属関係が削除されました。 この機能強化によって、1 次ディスクが満杯になった場合にワーカー・ノードが失敗することが回避されます。</td>
</tr>
<tr>
<td>ルート・パスワードの有効期限</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ワーカー・ノードのルート・パスワードは、コンプライアンスの理由により 90 日後に有効期限が切れます。 自動化ツールがワーカー・ノードに root としてログインする必要がある場合や、root として実行される cron ジョブを使用している場合は、ワーカー・ノードにログインして `chage -M -1 root` を実行することによって、パスワード有効期限を無効にすることができます。 **注**: セキュリティー・コンプライアンスに関する要件で、root として実行したり、パスワード有効期限を削除したりすることを避ける必要がある場合は、有効期限を無効にしないでください。 代わりに、少なくとも 90 日ごとにワーカー・ノードを[更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)または[再ロード](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)してください。</td>
</tr>
<tr>
<td>systemd</td>
<td>該当なし</td>
<td>該当なし</td>
<td>一時的なマウント装置が無制限に増えることを防止するため、それらを定期的に削除します。 このアクションは、[Kubernetes 問題 57345 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/issues/57345) に対処するためのものです。</td>
</tr>
<tr>
<td>Docker</td>
<td>該当なし</td>
<td>該当なし</td>
<td>デフォルトの Docker ブリッジが無効にされたため、`172.17.0.0/16` IP 範囲がプライベート経路で使用されるようになりました。 ホストで `docker` コマンドを直接実行することによって、または Docker ソケットをマウントするポッドを使用することによって、ワーカー・ノードで Docker コンテナーを作成している場合は、以下のオプションから選択してください。<ul><li>コンテナー作成時に外部ネットワーク接続を確実にするため、`docker build . --network host` を実行します。</li>
<li>コンテナー作成時に使用するネットワークを明示的に作成するため、`docker network create` を実行して、このネットワークを使用します。</li></ul>
**注**: Docker ソケットまたは Docker に直接依存していますか? [コンテナー・ランタイムとして `docker` の代わりに `containerd` を使用するように更新](/docs/containers?topic=containers-cs_versions#containerd)して、クラスターで Kubernetes バージョン 1.11 以降を実行できるようにしてください。</td>
</tr>
</tbody>
</table>

#### 1.9.10_1523 (2018 年 9 月 4 日リリース) の変更ログ
{: #1910_1523}

以下の表に、パッチ 1.9.10_1523 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.9_1522 からの変更点">
<caption>バージョン 1.9.9_1522 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.9-167</td>
<td>v1.9.10-192</td>
<td>Kubernetes 1.9.10 リリースをサポートするように更新されました。 さらに、`externalTrafficPolicy` が `local` に設定されて、ロード・バランサー・サービスの更新をより良好に処理できるようにクラウド・プロバイダー構成が変更されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>334</td>
<td>338</td>
<td>incubator のバージョンが 1.8 に更新されました。 ファイル・ストレージは、選択した特定のゾーンにプロビジョンされます。 複数ゾーン・クラスターを使用しており、地域とゾーンのラベルを追加する必要がある場合を除いて、既存の (静的) PV インスタンスのラベルを更新することはできません。<br><br>IBM 提供のファイル・ストレージ・クラスのマウント・オプションから、デフォルトの NFS バージョンが削除されました。 ホストのオペレーティング・システムによって、NFS バージョンが、IBM Cloud インフラストラクチャー (SoftLayer) の NFS サーバーとネゴシエーションされるようになりました。 特定の NFS バージョンを手動で設定する場合、またはホストのオペレーティング・システムによってネゴシエーションされた PV の NFS バージョンを変更する場合は、[デフォルトの NFS バージョンの変更](/docs/containers?topic=containers-file_storage#nfs_version_class)を参照してください。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.9</td>
<td>v1.9.10</td>
<td>Kubernetes [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.10) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes Heapster 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`heapster-nanny` コンテナーのリソース限度が増やされました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.9.9_1522 (2018 年 8 月 23 日リリース) の変更ログ
{: #199_1522}

以下の表に、ワーカー・ノードのフィックスパック 1.9.9_1522 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.9_1521 からの変更点">
<caption>バージョン 1.9.9_1521 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>`cgroup` リークを修正するように `systemd` が更新されました。</td>
</tr>
<tr>
<td>カーネル</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>[CVE-2018-3620、CVE-2018-3646 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://usn.ubuntu.com/3741-1/) に関連するカーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>


#### ワーカー・ノード・フィックスパック 1.9.9_1521 (2018 年 8 月 13 日リリース) の変更ログ
{: #199_1521}

以下の表に、ワーカー・ノードのフィックスパック 1.9.9_1521 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.9_1520 からの変更点">
<caption>バージョン 1.9.9_1520 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>インストール済み Ubuntu パッケージが更新されました。</td>
</tr>
</tbody>
</table>

#### 1.9.9_1520 (2018 年 7 月 27 日リリース) の変更ログ
{: #199_1520}

以下の表に、パッチ 1.9.9_1520 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.8_1517 からの変更点">
<caption>バージョン 1.9.8_1517 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.8-141</td>
<td>v1.9.9-167</td>
<td>Kubernetes 1.9.9 リリースをサポートするように更新されました。 また、LoadBalancer サービス `create failure` イベントで、ポータブル・サブネット・エラーが含まれるようになりました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>320</td>
<td>334</td>
<td>永続ボリューム作成のタイムアウトが 15 分から 30 分に増やされました。 デフォルトの請求タイプが `hourly` に変更されました。 事前定義ストレージ・クラスにマウント・オプションが追加されました。 IBM Cloud インフラストラクチャー (SoftLayer) アカウントの NFS ファイル・ストレージ・インスタンスで、**「メモ」**フィールドが JSON 形式に変更され、PV がデプロイされる Kubernetes 名前空間が追加されました。 マルチゾーン・クラスターをサポートするため、永続ボリュームにゾーンと地域のラベルが追加されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.8</td>
<td>v1.9.9</td>
<td>Kubernetes [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.9) を参照してください。</td>
</tr>
<tr>
<td>カーネル</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ハイパフォーマンス・ネットワーキング・ワークロードのために、ワーカー・ノード・ネットワーク設定がわずかに改善されました。</td>
</tr>
<tr>
<td>OpenVPN クライアント</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kube-system` 名前空間で実行される OpenVPN クライアント `vpn` デプロイメントは、Kubernetes `addon-manager` によって管理されるようになりました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.9.8_1517 (2018 年 7 月 3 日リリース) の変更ログ
{: #198_1517}

以下の表に、ワーカー・ノードのフィックスパック 1.9.8_1517 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.8_1516 からの変更点">
<caption>バージョン 1.9.8_1516 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ハイパフォーマンス・ネットワーキング・ワークロードのために、`sysctl` が最適化されました。</td>
</tr>
</tbody>
</table>


#### ワーカー・ノード・フィックスパック 1.9.8_1516 (2018 年 6 月 21 日リリース) の変更ログ
{: #198_1516}

以下の表に、ワーカー・ノードのフィックスパック 1.9.8_1516 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.8_1515 からの変更点">
<caption>バージョン 1.9.8_1515 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>該当なし</td>
<td>該当なし</td>
<td>非暗号化マシン・タイプの場合、ワーカー・ノードを再ロードまたは更新するときに新しいファイル・システムを取得することによって、2 次ディスクがクリーンアップされるようになりました。</td>
</tr>
</tbody>
</table>

#### 1.9.8_1515 (2018 年 6 月 19 日リリース) の変更ログ
{: #198_1515}

以下の表に、パッチ 1.9.8_1515 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.7_1513 からの変更点">
<caption>バージョン 1.9.7_1513 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.9.8</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.8) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスターの Kubernetes API サーバーの `--admission-control` オプションに `PodSecurityPolicy` が追加され、ポッドのセキュリティー・ポリシーをサポートするようにクラスターが構成されるようになりました。 詳しくは、[ポッド・セキュリティー・ポリシーの構成](/docs/containers?topic=containers-psp) を参照してください。</td>
</tr>
<tr>
<td>IBM Cloud Provider</td>
<td>v1.9.7-102</td>
<td>v1.9.8-141</td>
<td>Kubernetes 1.9.8 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>OpenVPN クライアント</td>
<td>該当なし</td>
<td>該当なし</td>
<td><code>kube-system</code> 名前空間で実行される OpenVPN クライアント <code>vpn</code> デプロイメントに <code>livenessProbe</code> が追加されました。</td>
</tr>
</tbody>
</table>


#### ワーカー・ノード・フィックスパック 1.9.7_1513 (2018 年 6 月 11 日リリース) の変更ログ
{: #197_1513}

以下の表に、ワーカー・ノードのフィックスパック 1.9.7_1513 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.7_1512 からの変更点">
<caption>バージョン 1.9.7_1512 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル更新</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>[CVE-2018-3639 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639) に対して、カーネル更新を使用して新規ワーカー・ノード・イメージが導入されました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.9.7_1512 (2018 年 5 月 18 日リリース) の変更ログ
{: #197_1512}

以下の表に、ワーカー・ノードのフィックスパック 1.9.7_1512 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.7_1511 からの変更点">
<caption>バージョン 1.9.7_1511 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ブロック・ストレージ・プラグインを使用したときに発生するバグに対応するための修正です。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.9.7_1511 (2018 年 5 月 16 日リリース) の変更ログ
{: #197_1511}

以下の表に、ワーカー・ノードのフィックスパック 1.9.7_1511 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.7_1510 からの変更点">
<caption>バージョン 1.9.7_1510 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kubelet` ルート・ディレクトリーに保管したデータが、ワーカー・ノード・マシンのより大きい 2 次ディスクに保存されるようになりました。</td>
</tr>
</tbody>
</table>

#### 1.9.7_1510 (2018 年 4 月 30 日リリース) の変更ログ
{: #197_1510}

以下の表に、パッチ 1.9.7_1510 に含まれる変更内容を示します。
{: shortdesc}

<table summary="バージョン 1.9.3_1506 からの変更点">
<caption>バージョン 1.9.3_1506 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.3</td>
<td>v1.9.7	</td>
<td><p>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7) を参照してください。 このリリースで、[CVE-2017-1002101 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) および [CVE-2017-1002102 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) の脆弱性に対応しています。</p><p><strong>注</strong>: `secret`、`configMap`、`downwardAPI`、および投影ボリュームは、読み取り専用でマウントされるようになりました。 これまでは、システムによって自動的に元の状態に戻されることがあるこれらのボリュームに、アプリがデータを書き込むことができました。 アプリが以前の非セキュアな動作に依存している場合は、適切に変更してください。</p></td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスターの Kubernetes API サーバーの `--runtime-config` オプションに `admissionregistration.k8s.io/v1alpha1=true` が追加されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.3-71</td>
<td>v1.9.7-102</td>
<td>`NodePort` と `LoadBalancer` サービスは、`service.spec.externalTrafficPolicy` を `Local` に設定すると、[クライアント・ソース IP の保持](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)をサポートするようになりました。</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>以前のクラスターの[エッジ・ノード](/docs/containers?topic=containers-edge#edge)耐障害性セットアップを修正します。</td>
</tr>
</tbody>
</table>

<br />


### バージョン 1.8 変更ログ (サポート対象外)
{: #18_changelog}

バージョン 1.8 の変更ログをまとめます。
{: shortdesc}

*   [ワーカー・ノード・フィックスパック 1.8.15_1521 (2018 年 9 月 20 日リリース) の変更ログ](#1815_1521)
*   [ワーカー・ノード・フィックスパック 1.8.15_1520 (2018 年 8 月 23 日リリース) の変更ログ](#1815_1520)
*   [ワーカー・ノード・フィックスパック 1.8.15_1519 (2018 年 8 月 13 日リリース) の変更ログ](#1815_1519)
*   [1.8.15_1518 (2018 年 7 月 27 日リリース) の変更ログ](#1815_1518)
*   [ワーカー・ノード・フィックスパック 1.8.13_1516 (2018 年 7 月 3 日リリース) の変更ログ](#1813_1516)
*   [ワーカー・ノード・フィックスパック 1.8.13_1515 (2018 年 6 月 21 日リリース) の変更ログ](#1813_1515)
*   [1.8.13_1514 (2018 年 6 月 19 日リリース) の変更ログ](#1813_1514)
*   [ワーカー・ノード・フィックスパック 1.8.11_1512 (2018 年 6 月 11 日リリース) の変更ログ](#1811_1512)
*   [ワーカー・ノード・フィックスパック 1.8.11_1511 (2018 年 5 月 18 日リリース) の変更ログ](#1811_1511)
*   [ワーカー・ノード・フィックスパック 1.8.11_1510 (2018 年 5 月 16 日リリース) の変更ログ](#1811_1510)
*   [1.8.11_1509 (2018 年 4 月 19 日リリース) の変更ログ](#1811_1509)

#### ワーカー・ノード・フィックスパック 1.8.15_1521 (2018 年 9 月 20 日リリース) の変更ログ
{: #1815_1521}

<table summary="バージョン 1.8.15_1520 からの変更点">
<caption>バージョン 1.8.15_1520 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>ログの循環</td>
<td>該当なし</td>
<td>該当なし</td>
<td>90 日内に再ロードまたは更新されないワーカー・ノードで `logrotate` が失敗することを防止するため、`cronjobs` の代わりに `systemd` タイマーを使用するように切り替えられました。 **注**: このマイナー・リリースの前のすべてのバージョンでは、ログが循環されないため、cron ジョブが失敗した後に 1 次ディスクが満杯になります。 ワーカー・ノードが更新も再ロードもされずに 90 日間アクティブだった場合、cron ジョブは失敗します。 ログで 1 次ディスクが満杯になった場合、ワーカー・ノードは失敗状態になります。 ワーカー・ノードを修正するには、`ibmcloud ks worker-reload` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)または `ibmcloud ks worker-update` [コマンド](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)を使用できます。</td>
</tr>
<tr>
<td>ワーカー・ノード・ランタイム・コンポーネント (`kubelet`、`kube-proxy`、`docker`)</td>
<td>該当なし</td>
<td>該当なし</td>
<td>1 次ディスクでのランタイム・コンポーネントの従属関係が削除されました。 この機能強化によって、1 次ディスクが満杯になった場合にワーカー・ノードが失敗することが回避されます。</td>
</tr>
<tr>
<td>ルート・パスワードの有効期限</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ワーカー・ノードのルート・パスワードは、コンプライアンスの理由により 90 日後に有効期限が切れます。 自動化ツールがワーカー・ノードに root としてログインする必要がある場合や、root として実行される cron ジョブを使用している場合は、ワーカー・ノードにログインして `chage -M -1 root` を実行することによって、パスワード有効期限を無効にすることができます。 **注**: セキュリティー・コンプライアンスに関する要件で、root として実行したり、パスワード有効期限を削除したりすることを避ける必要がある場合は、有効期限を無効にしないでください。 代わりに、少なくとも 90 日ごとにワーカー・ノードを[更新](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)または[再ロード](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)してください。</td>
</tr>
<tr>
<td>systemd</td>
<td>該当なし</td>
<td>該当なし</td>
<td>一時的なマウント装置が無制限に増えることを防止するため、それらを定期的に削除します。 このアクションは、[Kubernetes 問題 57345 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/issues/57345) に対処するためのものです。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.8.15_1520 (2018 年 8 月 23 日リリース) の変更ログ
{: #1815_1520}

<table summary="バージョン 1.8.15_1519 からの変更点">
<caption>バージョン 1.8.15_1519 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>`cgroup` リークを修正するように `systemd` が更新されました。</td>
</tr>
<tr>
<td>カーネル</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>[CVE-2018-3620、CVE-2018-3646 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://usn.ubuntu.com/3741-1/) に関連するカーネル更新を使用してワーカー・ノード・イメージが更新されました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.8.15_1519 (2018 年 8 月 13 日リリース) の変更ログ
{: #1815_1519}

<table summary="バージョン 1.8.15_1518 からの変更点">
<caption>バージョン 1.8.15_1518 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ubuntu パッケージ</td>
<td>該当なし</td>
<td>該当なし</td>
<td>インストール済み Ubuntu パッケージが更新されました。</td>
</tr>
</tbody>
</table>

#### 1.8.15_1518 (2018 年 7 月 27 日リリース) の変更ログ
{: #1815_1518}

<table summary="バージョン 1.8.13_1516 からの変更点">
<caption>バージョン 1.8.13_1516 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.13-176</td>
<td>v1.8.15-204</td>
<td>Kubernetes 1.8.15 リリースをサポートするように更新されました。 また、LoadBalancer サービス `create failure` イベントで、ポータブル・サブネット・エラーが含まれるようになりました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} File Storage プラグイン</td>
<td>320</td>
<td>334</td>
<td>永続ボリューム作成のタイムアウトが 15 分から 30 分に増やされました。 デフォルトの請求タイプが `hourly` に変更されました。 事前定義ストレージ・クラスにマウント・オプションが追加されました。 IBM Cloud インフラストラクチャー (SoftLayer) アカウントの NFS ファイル・ストレージ・インスタンスで、**「メモ」**フィールドが JSON 形式に変更され、PV がデプロイされる Kubernetes 名前空間が追加されました。 マルチゾーン・クラスターをサポートするため、永続ボリュームにゾーンと地域のラベルが追加されました。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.8.13</td>
<td>v1.8.15</td>
<td>Kubernetes [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.15) を参照してください。</td>
</tr>
<tr>
<td>カーネル</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ハイパフォーマンス・ネットワーキング・ワークロードのために、ワーカー・ノード・ネットワーク設定がわずかに改善されました。</td>
</tr>
<tr>
<td>OpenVPN クライアント</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kube-system` 名前空間で実行される OpenVPN クライアント `vpn` デプロイメントは、Kubernetes `addon-manager` によって管理されるようになりました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.8.13_1516 (2018 年 7 月 3 日リリース) の変更ログ
{: #1813_1516}

<table summary="バージョン 1.8.13_1515 からの変更点">
<caption>バージョン 1.8.13_1515 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ハイパフォーマンス・ネットワーキング・ワークロードのために、`sysctl` が最適化されました。</td>
</tr>
</tbody>
</table>


#### ワーカー・ノード・フィックスパック 1.8.13_1515 (2018 年 6 月 21 日リリース) の変更ログ
{: #1813_1515}

<table summary="バージョン 1.8.13_1514 からの変更点">
<caption>バージョン 1.8.13_1514 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>該当なし</td>
<td>該当なし</td>
<td>非暗号化マシン・タイプの場合、ワーカー・ノードを再ロードまたは更新するときに新しいファイル・システムを取得することによって、2 次ディスクがクリーンアップされるようになりました。</td>
</tr>
</tbody>
</table>

#### 1.8.13_1514 (2018 年 6 月 19 日リリース) の変更ログ
{: #1813_1514}

<table summary="バージョン 1.8.11_1512 からの変更点">
<caption>バージョン 1.8.11_1512 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.11</td>
<td>v1.8.13</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.13) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスターの Kubernetes API サーバーの `--admission-control` オプションに `PodSecurityPolicy` が追加され、ポッドのセキュリティー・ポリシーをサポートするようにクラスターが構成されるようになりました。 詳しくは、[ポッド・セキュリティー・ポリシーの構成](/docs/containers?topic=containers-psp) を参照してください。</td>
</tr>
<tr>
<td>IBM Cloud Provider</td>
<td>v1.8.11-126</td>
<td>v1.8.13-176</td>
<td>Kubernetes 1.8.13 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>OpenVPN クライアント</td>
<td>該当なし</td>
<td>該当なし</td>
<td><code>kube-system</code> 名前空間で実行される OpenVPN クライアント <code>vpn</code> デプロイメントに <code>livenessProbe</code> が追加されました。</td>
</tr>
</tbody>
</table>


#### ワーカー・ノード・フィックスパック 1.8.11_1512 (2018 年 6 月 11 日リリース) の変更ログ
{: #1811_1512}

<table summary="バージョン 1.8.11_1511 からの変更点">
<caption>バージョン 1.8.11_1511 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル更新</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>[CVE-2018-3639 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639) に対して、カーネル更新を使用して新規ワーカー・ノード・イメージが導入されました。</td>
</tr>
</tbody>
</table>


#### ワーカー・ノード・フィックスパック 1.8.11_1511 (2018 年 5 月 18 日リリース) の変更ログ
{: #1811_1511}

<table summary="バージョン 1.8.11_1510 からの変更点">
<caption>バージョン 1.8.11_1510 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ブロック・ストレージ・プラグインを使用したときに発生するバグに対応するための修正です。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.8.11_1510 (2018 年 5 月 16 日リリース) の変更ログ
{: #1811_1510}

<table summary="バージョン 1.8.11_1509 からの変更点">
<caption>バージョン 1.8.11_1509 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kubelet` ルート・ディレクトリーに保管したデータが、ワーカー・ノード・マシンのより大きい 2 次ディスクに保存されるようになりました。</td>
</tr>
</tbody>
</table>


#### 1.8.11_1509 (2018 年 4 月 19 日リリース) の変更ログ
{: #1811_1509}

<table summary="バージョン 1.8.8_1507 からの変更点">
<caption>バージョン 1.8.8_1507 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td><p>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11) を参照してください。 このリリースで、[CVE-2017-1002101 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) および [CVE-2017-1002102 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) の脆弱性に対応しています。</p><p>`secret`、`configMap`、`downwardAPI`、投影ボリュームは、読み取り専用としてマウントされるようになります。 これまでは、システムによって自動的に元の状態に戻されることがあるこれらのボリュームに、アプリがデータを書き込むことができました。 アプリが以前の非セキュアな動作に依存している場合は、適切に変更してください。</p></td>
</tr>
<tr>
<td>一時停止コンテナーのイメージ</td>
<td>3.0</td>
<td>3.1</td>
<td>継承したオーファン・ゾンビ・プロセスを削除します。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>`NodePort` と `LoadBalancer` サービスは、`service.spec.externalTrafficPolicy` を `Local` に設定すると、[クライアント・ソース IP の保持](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)をサポートするようになりました。</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>以前のクラスターの[エッジ・ノード](/docs/containers?topic=containers-edge#edge)耐障害性セットアップを修正します。</td>
</tr>
</tbody>
</table>

<br />


### バージョン 1.7 変更ログ (サポート対象外)
{: #17_changelog}

バージョン 1.7 の変更ログをまとめます。
{: shortdesc}

*   [ワーカー・ノード・フィックスパック 1.7.16_1514 (2018 年 6 月 11 日リリース) の変更ログ](#1716_1514)
*   [ワーカー・ノード・フィックスパック 1.7.16_1513 (2018 年 5 月 18 日リリース) の変更ログ](#1716_1513)
*   [ワーカー・ノード・フィックスパック 1.7.16_1512 (2018 年 5 月 16 日リリース) の変更ログ](#1716_1512)
*   [1.7.16_1511 (2018 年 4 月 19 日リリース) の変更ログ](#1716_1511)

#### ワーカー・ノード・フィックスパック 1.7.16_1514 (2018 年 6 月 11 日リリース) の変更ログ
{: #1716_1514}

<table summary="バージョン 1.7.16_1513 からの変更点">
<caption>バージョン 1.7.16_1513 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>カーネル更新</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>[CVE-2018-3639 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639) に対して、カーネル更新を使用して新規ワーカー・ノード・イメージが導入されました。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.7.16_1513 (2018 年 5 月 18 日リリース) の変更ログ
{: #1716_1513}

<table summary="バージョン 1.7.16_1512 からの変更点">
<caption>バージョン 1.7.16_1512 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ブロック・ストレージ・プラグインを使用したときに発生するバグに対応するための修正です。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.7.16_1512 (2018 年 5 月 16 日リリース) の変更ログ
{: #1716_1512}

<table summary="バージョン 1.7.16_1511 からの変更点">
<caption>バージョン 1.7.16_1511 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kubelet` ルート・ディレクトリーに保管したデータが、ワーカー・ノード・マシンのより大きい 2 次ディスクに保存されるようになりました。</td>
</tr>
</tbody>
</table>

#### 1.7.16_1511 (2018 年 4 月 19 日リリース) の変更ログ
{: #1716_1511}

<table summary="バージョン 1.7.4_1509 からの変更点">
<caption>バージョン 1.7.4_1509 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td><p>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16) を参照してください。 このリリースで、[CVE-2017-1002101 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) および [CVE-2017-1002102 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) の脆弱性に対応しています。</p><p>`secret`、`configMap`、`downwardAPI`、投影ボリュームは、読み取り専用としてマウントされるようになります。 これまでは、システムによって自動的に元の状態に戻されることがあるこれらのボリュームに、アプリがデータを書き込むことができました。 アプリが以前の非セキュアな動作に依存している場合は、適切に変更してください。</p></td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>`NodePort` と `LoadBalancer` サービスは、`service.spec.externalTrafficPolicy` を `Local` に設定すると、[クライアント・ソース IP の保持](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations)をサポートするようになりました。</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>以前のクラスターの[エッジ・ノード](/docs/containers?topic=containers-edge#edge)耐障害性セットアップを修正します。</td>
</tr>
</tbody>
</table>
