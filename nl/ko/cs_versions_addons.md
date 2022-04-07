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


# Fluentd 및 Ingress ALB 변경 로그
{: #cluster-add-ons-changelog}

{{site.data.keyword.containerlong}} 클러스터가 IBM에서 자동으로 업데이트하는 컴포넌트(예: Fluentd 및 Ingress ALB 컴포넌트)와 함께 제공됩니다. 일부 컴포넌트에 대한 자동 업데이트를 사용 안함으로 설정하고 마스터 및 작업자 노드와 별도로 수동으로 업데이트할 수도 있습니다. 각 버전의 변경사항에 대한 요약은 다음 절에 나와 있는 표를 참조하십시오.
{: shortdesc}

Fluentd 및 Ingress ALB에 대한 업데이트 관리와 관련된 자세한 정보는 [클러스터 컴포넌트 업데이트](/docs/containers?topic=containers-update#components)를 참조하십시오.

## Ingress ALB 변경 로그
{: #alb_changelog}

{{site.data.keyword.containerlong_notm}} 클러스터에 있는 Ingress 애플리케이션 로드 밸런서(ALB)에 대한 빌드 버전 변경사항을 확인하십시오.
{:shortdesc}

Ingress ALB 컴포넌트가 업데이트되면 모든 ALB 팟(Pod)에 있는 `nginx-ingress` 및 `ingress-auth` 컨테이너가 최신 빌드 버전으로 업데이트됩니다. 기본적으로 업데이트에 대한 자동 업데이트가 사용으로 설정되어 있지만, 자동 업데이트를 사용 안함으로 설정하고 추가 기능을 수동으로 업데이트할 수 있습니다. 자세한 정보는 [Ingress 애플리케이션 로드 밸런서 업데이트](/docs/containers?topic=containers-update#alb)를 참조하십시오.

Ingress ALB 컴포넌트의 빌드별 변경사항에 대한 요약은 다음 표를 참조하십시오.

<table summary="Ingress 애플리케이션 로드 밸런서 컴포넌트에 대한 빌드 변경사항 개요">
<caption>Ingress 애플리케이션 로드 밸런서 컴포넌트에 대한 변경 로그</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>`nginx-ingress` / `ingress-auth` 빌드</th>
<th>릴리스 날짜</th>
<th>비파괴적 변경사항</th>
<th>파괴적 변경사항</th>
</tr>
</thead>
<tbody>
<tr>
<td>470/330</td>
<td>2019년 6월 7일</td>
<td>[CVE-2019-8457 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457)의 Berkeley DB 취약점을 해결합니다.
</td>
<td>-</td>
</tr>
<tr>
<td>470/329</td>
<td>2019년 6월 6일</td>
<td>[CVE-2019-8457 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457)의 Berkeley DB 취약점을 해결합니다.
</td>
<td>-</td>
</tr>
<tr>
<td>467/329</td>
<td>2019년 6월 3일</td>
<td>[CVE-2019-3829 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-3893 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3893), [CVE-2018-10844 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10845 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844) 및 [CVE-2018-10846 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846)의 GnuTLS 취약점을 해결합니다.
</td>
<td>-</td>
</tr>
<tr>
<td>462/329</td>
<td>2019년 5월 28일</td>
<td>[CVE-2019-5435 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) 및 [CVE-2019-5436 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436)의 cURL 취약점을 해결합니다. </td>
<td>-</td>
</tr>
<tr>
<td>457/329</td>
<td>2019년 5월 23일</td>
<td>이미지 스캔의 Go 취약점을 해결합니다.</td>
<td>-</td>
</tr>
<tr>
<td>423/329</td>
<td>2019년 5월 13일</td>
<td>{{site.data.keyword.appid_full}}와의 통합에 대한 성능이 향상되었습니다.</td>
<td>-</td>
</tr>
<tr>
<td>411 / 315</td>
<td>2019년 4월 15일</td>
<td>액세스 토큰 만기 값과 일치하도록 {{site.data.keyword.appid_full_notm}} 쿠키 만기 값을 업데이트합니다.</td>
<td>-</td>
</tr>
<tr>
<td>411 / 306</td>
<td>2019년 3월 22일</td>
<td>Go 버전을 1.12.1로 업데이트합니다.</td>
<td>-</td>
</tr>
<tr>
<td>410 / 305</td>
<td>2019년 3월 18일</td>
<td><ul>
<li>이미지 스캔의 취약점을 해결합니다.</li>
<li>{{site.data.keyword.appid_full_notm}}와의 통합에 대한 로깅이 향상되었습니다.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>408 / 304</td>
<td>2019년 3월 5일</td>
<td>-</td>
<td>로그아웃 기능, 토큰 만료 및 `OAuth` 권한 콜백과 관련된 권한 통합의 버그를 해결합니다. 이 수정사항은 [`appid-auth`](/docs/containers?topic=containers-ingress_annotation#appid-auth) 어노테이션을 사용하여 {{site.data.keyword.appid_full_notm}} 권한을 사용으로 설정한 경우에만 구현됩니다. 이 수정사항을 구현하기 위해 총 헤더 크기를 증가시키는 추가 헤더가 추가되었습니다. 사용자 고유의 헤더 크기 및 총 응답 크기에 따라 사용하는 [ 프록시 버퍼 어노테이션](/docs/containers?topic=containers-ingress_annotation#proxy-buffer)을 조정해야 할 수 있습니다.</td>
</tr>
<tr>
<td>406 / 301</td>
<td>2019년 2월 19일</td>
<td><ul>
<li>Go 버전을 1.11.5로 업데이트합니다.</li>
<li>이미지 스캔의 취약점을 해결합니다.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>404 / 300</td>
<td>2019년 1월 31일</td>
<td>Go 버전을 1.11.4로 업데이트합니다.</td>
<td>-</td>
</tr>
<tr>
<td>403 / 295</td>
<td>2019년 1월 23일</td>
<td><ul>
<li>ALB의 NGINX 버전을 1.15.2로 업데이트합니다.</li>
<li>IBM에서 제공하는 TLS 인증서는 이제 7일이 아닌 만료되기 37일 전에 자동으로 갱신됩니다.</li>
<li>{{site.data.keyword.appid_full_notm}} 로그아웃 기능이 추가됨: `/logout` 접두부가 {{site.data.keyword.appid_full_notm}} 경로에 있으면 쿠키가 제거되고 사용자를 로그인 페이지로 다시 보냅니다.</li>
<li>내부 추적 용도로 {{site.data.keyword.appid_full_notm}} 요청에 헤더가 추가되었습니다.</li>
<li>`app-id` 어노테이션이 `proxy-buffers`, `proxy-buffer-size` 및 `proxy-busy-buffer-size` 어노테이션과 함께 사용될 수 있도록 {{site.data.keyword.appid_short_notm}} 위치 지시문을 업데이트합니다.</li>
<li>정보 로그가 오류로 레이블되지 않도록 버그를 해결합니다.</li>
</ul></td>
<td>기본적으로 TLS 1.0 및 1.1을 사용 안함으로 설정합니다. 앱에 연결하는 클라이언트가 TLS 1.2를 지원하면 어떠한 조치도 필요하지 않습니다. 여전히 TLS 1.0 또는 1.1 지원이 필요한 레거시 클라이언트가 있는 경우 [이 단계](/docs/containers?topic=containers-ingress#ssl_protocols_ciphers)를 수행하여 수동으로 필요한 TLS 버전을 사용으로 설정하십시오. 클라이언트가 애플리케이션에 액세스하는 데 사용하는 TLS 버전을 확인하는 방법에 대한 자세한 정보는 이 [{{site.data.keyword.Bluemix_notm}} 블로그 게시물](https://www.ibm.com/blogs/bluemix/2018/11/ibm-cloud-kubernetes-service-alb-update-tls-1-0-and-1-1-disabled-by-default/)을 참조하십시오.</td>
</tr>
<tr>
<td>393 / 291</td>
<td>2019년 1월 9일</td>
<td>다중 {{site.data.keyword.appid_full_notm}} 인스턴스와의 통합에 대한 지원이 추가되었습니다.</td>
<td>-</td>
</tr>
<tr>
<td>393 / 282</td>
<td>2018년 11월 29일</td>
<td>{{site.data.keyword.appid_full_notm}}와의 통합에 대한 성능이 향상되었습니다.</td>
<td>-</td>
</tr>
<tr>
<td>384 / 246</td>
<td>2018년 11월 14일</td>
<td>{{site.data.keyword.appid_full_notm}}와의 통합에 대한 로깅 및 로그아웃 기능이 향상되었습니다.</td>
<td>`*.containers.mybluemix.net`에 대한 자체 서명 인증서가 자동으로 생성되어 클러스터에서 사용되는 LetsEncrypt 서명 인증서로 대체되었습니다. `*.containers.mybluemix.net` 자체 서명 인증서는 제거되었습니다.</td>
</tr>
<tr>
<td>350 / 192</td>
<td>2018년 11월 5일</td>
<td>Ingress ALB 컴포넌트의 자동 업데이트를 사용 및 사용 안함으로 설정할 수 있는 지원이 추가되었습니다.</td>
<td>-</td>
</tr>
</tbody>
</table>

## 로깅을 위한 Fluentd
{: #fluentd_changelog}

{{site.data.keyword.containerlong_notm}} 클러스터에 로그인하기 위해 Fluentd 컴포넌트에 대한 빌드 버전 변경사항을 확인하십시오.
{:shortdesc}

기본적으로 업데이트에 대한 자동 업데이트가 사용으로 설정되어 있지만, 자동 업데이트를 사용 안함으로 설정하고 추가 기능을 수동으로 업데이트할 수 있습니다. 자세한 정보는 [Fluentd에 대한 자동 업데이트 관리](/docs/containers?topic=containers-update#logging-up)를 참조하십시오.

Fluentd 컴포넌트의 빌드별 변경사항에 대한 요약은 다음 표를 참조하십시오.

<table summary="Fluentd 컴포넌트에 대한 빌드 변경사항 개요">
<caption>Fluentd 컴포넌트에 대한 변경 로그</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>Fluentd 빌드</th>
<th>릴리스 날짜</th>
<th>비파괴적 변경사항</th>
<th>파괴적 변경사항</th>
</tr>
</thead>
<tr>
<td>e7c10d74350dc64d4d92ba7f72bb4ff9219315d2</td>
<td>2019년 5월 30일</td>
<td>Kubernetes 마스터를 사용할 수 없는 경우에도 IBM 네임스페이스에서 항상 팟(Pod) 로그를 무시하도록 Fluent 구성 맵을 업데이트합니다.</td>
<td>-</td>
</tr>
<tr>
<td>c16fe1602ab65db4af0a6ac008f99ca2a526e6f6</td>
<td>2019년 5월 21일</td>
<td>작업자 노드 메트릭이 표시되지 않는 버그를 해결합니다. </td>
<td>-</td>
</tr>
<tr>
<td>60fc11f7bd39d9c6cfed923c598bf6457b3f2037</td>
<td>2019년 5월 10일</td>
<td>[CVE-2019-8320 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320), [CVE-2019-8321 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321), [CVE-2019-8322 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322), [CVE-2019-8323 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323), [CVE-2019-8324 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) 및 [CVE-2019-8325 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325)의 Ruby 패키지를 업데이트합니다.</td>
<td>-</td>
</tr>
<tr>
<td>91a737f68f7d9e81b5d2223c910aaa7d7f91b76d</td>
<td>2019년 5월 8일</td>
<td>[CVE-2019-8320 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320), [CVE-2019-8321 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321), [CVE-2019-8322 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322), [CVE-2019-8323 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323), [CVE-2019-8324 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) 및 [CVE-2019-8325 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325)의 Ruby 패키지를 업데이트합니다.</td>
<td>-</td>
</tr>
<tr>
<td>d9af69e286986a05ed4a50469585b1cf978ddb1d</td>
<td>2019년 4월 11일</td>
<td>TLS 1.2를 사용하도록 cAdvisor 플러그인을 업데이트합니다.</td>
<td>-</td>
</tr>
<tr>
<td>3100ddb62580a9f46ffdff7bab2ebec40b164de6</td>
<td>2019년 4월 1일</td>
<td>Fluentd 서비스 계정을 업데이트합니다.</td>
<td>-</td>
</tr>
<tr>
<td>c85567b75bd7ad1c9428794cd63a8e239c3fd8f5</td>
<td>2019년 3월 18일</td>
<td>[CVE-2019-8323 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323)에 대한 cURL의 종속성을 제거합니다.</td>
<td>-</td>
</tr>
<tr>
<td>320ffdf87de068ee2f7f34c0e7a47a111e8d457b</td>
<td>2019년 2월 18일</td>
<td><ul>
<li>Fluend를 버전 1.3으로 업데이트합니다.</li>
<li>[CVE-2018-19486 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19486)에 대한 Fluentd 이미지에서 Git을 제거합니다.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>972865196aefd3324105087878de12c518ed579f</td>
<td>2019년 1월 1일</td>
<td><ul>
<li>Fluentd `in_tail` 플러그인에 대한 UTF-8 인코딩을 사용으로 설정합니다. </li>
<li>사소한 버그가 수정되었습니다.</li>
</ul></td>
<td>-</td>
</tr>
</tbody>
</table>
