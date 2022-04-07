---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, node scaling

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
{:gif: data-image-type='gif'}



# 클러스터 스케일링
{: #ca}

{{site.data.keyword.containerlong_notm}} `ibm-iks-cluster-autoscaler` 플러그인을 사용하면 클러스터의 작업자 풀을 자동으로 스케일링하여 스케줄된 워크로드의 크기 결정 요구사항에 따라 작업자 풀의 작업자 노드 수를 늘리거나 줄일 수 있습니다. `ibm-iks-cluster-autoscaler` 플러그인은 [Kubernetes 클러스터 오토스케일러 프로젝트 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)를 기반으로 합니다.
{: shortdesc}

대신 팟(Pod)을 오토스케일링하시겠습니까? [앱 스케일링](/docs/containers?topic=containers-app#app_scaling)을 확인하십시오.
{: tip}

클러스터 오토스케일러는 공용 네트워크 연결로 설정된 표준 클러스터에서 사용할 수 있습니다. 클러스터가 방화벽으로 보호되는 사설 클러스터 또는 개인 서비스 엔드포인트가 사용으로 설정된 클러스터와 같이 공용 네트워크에 액세스할 수 없는 경우, 클러스터에서 클러스터 오토스케일러를 사용할 수 없습니다.
{: important}

## 스케일링 업 및 스케일링 다운 이해
{: #ca_about}

클러스터 오토스케일러는 주기적으로 클러스터를 스캔하여 워크로드 리소스 요청 및 구성하는 사용자 정의 설정(예: 스캔 간격)에 대한 응답으로 관리하는 작업자 풀의 작업자 노드 수를 조정합니다. 매 분마다 클러스터 오토스케일러는 다음 상황을 확인합니다.
{: shortdesc}

*   **보류 중인 팟(Pod) 스케일링 업**: 작업자 노드에 팟(Pod)을 스케줄하는 데 충분하지 않은 컴퓨팅 리소스가 존재하는 경우 팟(Pod)은 보류 중으로 간주됩니다. 클러스터 오토스케일러가 보류 중인 팟(Pod)을 감지하면 오토스케일러는 워크로드 리소스 요청을 충족하기 위해 구역 전체에 작업자 노드를 균등하게 스케일링 업합니다.
*   **스케일링 다운할, 사용률이 낮은 작업자 노드**: 기본적으로, 10분 이상 요청된 총 컴퓨팅 리소스의 50% 미만으로 실행되며 워크로드를 다른 작업자 노드로 재스케줄할 수 있는 작업자 노드는 사용률이 낮은 것으로 간주됩니다. 클러스터 오토스케일러가 사용률이 낮은 작업자 노드를 감지하면 필요한 컴퓨팅 리소스만 있도록 작업자 노드를 한 번에 하나씩 스케일링 다운합니다. 필요한 경우 10분 동안 50% 의 기본 스케일링 다운 사용률 임계값을 [사용자 정의](/docs/containers?topic=containers-ca#ca_chart_values)할 수 있습니다.

시간이 경과함에 따라 정기적으로 스캔과 스케일링 업 및 다운이 수행되며 작업자 노드의 수에 따라 완료하는 데 더 오랜 시간(예: 30분)이 소요될 수 있습니다.

클러스터 오토스케일러는 실제 작업자 노드 사용량이 아닌 배치에 정의한 [리소스 요청 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)을 고려하여 작업자 노드의 수를 조정합니다. 팟(Pod)과 배치에서 적절한 양의 리소스를 요청하지 않는 경우 구성 파일을 조정해야 합니다. 클러스터 오토스케일러가 구성 파일을 조정할 수 없습니다. 작업자 노드는 기본 클러스터 기능, 기본 및 사용자 정의 [추가 기능](/docs/containers?topic=containers-update#addons), [리소스 예약](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)에 자신의 컴퓨팅 리소스 중 일부를 사용한다는 점 또한 기억하십시오.
{: note}

<br>
**스케일링 업 및 다운은 어떤 모습입니까?**<br>
일반적으로 클러스터 오토스케일러는 클러스터에서 워크로드를 실행하는 데 필요한 작업자 노드의 수를 계산합니다. 클러스터 스케일링 업 및 다운은 다음 항목을 비롯한 많은 요인에 따라 달라집니다. 
*   설정한 구역당 최소 및 최대 작업자 노드 크기.
*   보류 중인 팟(Pod) 리소스 요청 및 워크로드와 연관시키는 특정 메타데이터(예: 반친화성), 특정 시스템 유형에만 팟(Pod)을 배치하는 레이블 또는 [팟(Pod) 중단 비용![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/).
*   클러스터 오토스케일러가 관리하는 작업자 풀(잠재적으로 [다중 구역 클러스터](/docs/containers?topic=containers-ha_clusters#multizone)의 구역 전체에 걸쳐 있음).
*   설정된 [사용자 정의 Helm 차트 값](#ca_chart_values)(예: 로컬 스토리지를 사용하는 경우 삭제를 위해 작업자 노드를 건너뜀).

자세한 정보는 [스케일링 업은 어떻게 작동합니까? ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-up-work) 및 [스케일링 다운은 어떻게 작동합니까? ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-scale-down-work)에 대한 Kubernetes 클러스터 오토스케일러 FAQ를 참조하십시오.

<br>

**스케일링 업 및 스케일링 다운 작동 방식을 변경할 수 있습니까?**<br>
설정을 사용자 정의하거나 기타 Kubernetes 리소스를 사용하여 스케일링 업 및 스케일링 다운 작동 방식에 영향을 줄 수 있습니다.
*   **스케일링 업**: [클러스터 오토스케일러 Helm 차트 값 사용자 정의](#ca_chart_values)(예: `scanInterval`, `expander`, `skipNodes` 또는 `maxNodeProvisionTime`). 작업자 풀에 리소스가 부족해지기 전에 작업자 노드를 스케일링 업할 수 있도록 [작업자 노드를 과잉 프로비저닝](#ca_scaleup)하는 방법을 검토하십시오. 또한 [Kubernetes 팟(Pod) 예산 중단 및 팟(Pod) 우선순위 컷오프를 설정](#scalable-practices-apps)하여 스케일링 업 작동 방식에 영향을 줄 수도 있습니다.
*   **스케일링 다운**: [클러스터 오토스케일러 Helm 차트 값 사용자 정의](#ca_chart_values)(예: `scaleDownUnneededTime`, `scaleDownDelayAfterAdd`, `scaleDownDelayAfterDelete` 또는 `scaleDownUtilizationThreshold`).

<br>
**내 클러스터를 즉시 스케일링 업하기 위해 구역당 최소 크기를 설정할 수 있습니까?**<br>
아니오, `minSize` 설정이 스케일링 업을 자동으로 트리거하지는 않습니다. `minSize`는 클러스터 오토스케일러가 각 구역을 특정 작업자 노드 수보다 적게 스케일링하지 않도록 하는 임계값입니다. 자신의 클러스터에 있는 구역당 작업자 노드 수가 아직 이보다 모자란 경우 클러스터 오토스케일러는 추가 리소스를 필요로 하는 워크로드 리소스가 생길 때까지 스케일링 업을 수행하지 않습니다. 예를 들어, 세 구역 각각에 하나의 작업자 노드가 있는 작업자 풀이 있으며(총 세 개의 작업자 노드) `minSize`를 구역당 `4`로 설정하는 경우 클러스터 오토스케일러는 각 구역에서 즉시 세 작업자 노드를 추가로 프로비저닝하지 않습니다(총 12개의 작업자 노드). 대신 이 스케일링 업은 리소스 요청에 의해 트리거됩니다. 15개 작업자 노드의 리소스를 요청하는 워크로드를 작성하는 경우 클러스터 오토스케일러는 이 요청을 만족시키기 위해 작업자 풀을 스케일링 업합니다. `minSize`는 이제 구역당 네 개의 작업자 노드를 요청하는 워크로드를 제거하는 경우에도 클러스터 오토스케일러가 작업자 노드를 이보다 적게 스케일링 다운하지 않음을 의미합니다. 

<br>
**이 작동은 클러스터 오토스케일러가 관리하지 않는 작업자 풀과 어떻게 다릅니까?**<br>
[작업자 풀을 작성](/docs/containers?topic=containers-add_workers#add_pool)할 때 해당 구역당 작업자 노드의 수를 지정합니다. 사용자가 작업자 노드의 수를 [크기 조정](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) 또는 [리밸런싱](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)할 때까지 작업자 풀은 작업자 노드의 수를 유지보수합니다. 작업자 풀은 사용자의 작업자 노드를 추가하거나 제거하지 않습니다. 스케줄될 수 있는 팟(Pod)보다 많은 팟(Pod)이 있는 경우 팟(Pod)은 작업자 풀 크기를 조정할 때까지 보류 상태로 유지됩니다.

작업자 풀에 대해 클러스터 오토스케일러를 사용으로 설정한 경우, 작업자 노드는 팟(Pod) 스펙 설정 및 리소스 요청에 대한 응답으로 스케줄링 업 또는 다운됩니다. 작업자 풀을 수동으로 크기 조정하거나 리밸런싱할 필요가 없습니다.

<br>
**클러스터 오토스케일러의 스케줄링 업 및 다운 방법에 대한 예제를 볼 수 있습니까?**<br>
클러스터 스케일링 업 및 다운 예제는 다음 이미지를 고려하십시오.

_Figure: 클러스터를 자동으로 스케일링 업 및 다운합니다._
![클러스터 스케일링 업 및 다운 GIF](images/cluster-autoscaler-x3.gif){: gif}

1.  클러스터에는 두 개의 구역에 걸쳐 분산되어 있는 두 개의 작업자 풀에 4개의 작업자 노드가 있습니다. 각 풀에는 구역당 하나의 작업자 노드가 있지만 **작업자 풀 A**에는 `u2c.2x4` 시스템 유형이 있으며 **작업자 풀 B**에는 `b2c.4x16` 시스템 유형이 있습니다. 총 컴퓨팅 리소스는 약 10개의 코어(**작업자 풀 A**에 해당하는 2개의 코어 x 2개의 작업자 노드, **작업자 풀 B**에 해당하는 4개의 코어 x 2개의 작업자 노드)입니다. 현재 클러스터는 이러한 10개의 코어 중 6개 코어를 요청하는 워크로드를 실행하고 있습니다. 추가 컴퓨팅 리소스는 각 작업자 노드에서 클러스터, 작업자 노드, 기타 추가 기능(클러스터 오토스케일러 등)을 실행하는 데 필요한 [예약된 리소스](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node)가 사용합니다. 
2.  클러스터 오토스케일러는 다음과 같은 최소 및 최대 구역당 크기를 사용하는 두 개의 작업자 풀을 모두 관리하도록 구성되어 있습니다.
    *  **작업자 풀 A**: `minSize=1`, `maxSize=5`.
    *  **작업자 풀 B**: `minSize=1`, `maxSize=2`.
3.  복제본당 CPU 코어 1개를 요청하는 앱의 추가 팟(Pod) 복제본을 14개 필요로 하는 배치를 스케줄합니다. 하나의 팟(Pod) 복제본이 현재 리소스에 배치될 수 있지만 나머지 13개는 보류 중입니다.
4.  클러스터 오토스케일러는 추가 13개의 팟(Pod) 복제본 리소스 요청을 지원하기 위해 이러한 제한조건 내에서 작업자 노드를 스케줄링 업합니다.
    *  **작업자 풀 A**: 일곱 개의 작업자 노드가 구역 전체에서 가능한 한 균등하게 라운드 로빈 방법으로 추가됩니다. 작업자 노드는 대략 14개의 코어(2개의 코어 x 7개의 작업자 노드)로 클러스터 컴퓨팅 용량을 늘립니다.
    *  **작업자 풀 B**: 두 개의 작업자 노드가 구역 전체에서 균등하게 추가되어, `maxSize`인 구역당 두 개의 작업자 노드에 도달합니다. 작업자 노드는 대략 8개의 코어(4개의 코어 x 2개의 작업자 노드)로 클러스터 용량을 늘립니다.
5.  코어 1개의 요청이 있는 20개의 팟(Pod)은 작업자 노드에서 다음과 같이 분배됩니다. 작업자 노드에는 팟(Pod)뿐만 아니라 기본 클러스터 기능을 처리하기 위해 실행되는 예약된 리소스도 있으므로, 워크로드를 위한 팟(Pod)이 작업자 노드의 모든 사용 가능한 컴퓨팅 리소스를 사용할 수는 없습니다. 예를 들면 `b2c.4x16` 작업자 노드에는 네 개의 코어가 있지만, 각각 최소 한 개의 코어를 요청하는 세 개의 팟(Pod)만 작업자 노드에 스케줄할 수 있습니다.
    <table summary="스케일링된 클러스터에서의 워크로드 분배를 설명하는 표입니다.">
    <caption>스케일링된 클러스터에서 워크로드 분배.</caption>
    <thead>
    <tr>
      <th>작업자 풀</th>
      <th>구역</th>
      <th>유형</th>
      <th># 작업자 노드</th>
      <th># 팟(Pod)</th>
    </tr>
    </thead>
    <tbody>
    <tr>
      <td>A</td>
      <td>dal10</td>
      <td>u2c.2x4</td>
      <td>네 개의 노드</td>
      <td>세 개의 팟(Pod)</td>
    </tr>
    <tr>
      <td>A</td>
      <td>dal12</td>
      <td>u2c.2x4</td>
      <td>다섯 개의 노드</td>
      <td>다섯 개의 팟(Pod)</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal10</td>
      <td>b2c.4x16</td>
      <td>두 개의 노드</td>
      <td>여섯 개의 팟(Pod)</td>
    </tr>
    <tr>
      <td>B</td>
      <td>dal12</td>
      <td>b2c.4x16</td>
      <td>두 개의 노드</td>
      <td>여섯 개의 팟(Pod)</td>
    </tr>
    </tbody>
    </table>
6.  더 이상 추가 워크로드가 필요하지 않으므로 배치를 삭제합니다. 잠시 후, 클러스터 오토스케일러는 클러스터가 더 이상 모든 컴퓨팅 리소스를 필요로 하지 않음을 발견하고 한 번에 하나씩 작업자 노드를 스케일링 다운하기 시작합니다. 
7.  작업자 풀이 스케일링 다운되었습니다. 클러스터 오토스케일러는 정기적으로 스캔하여 보류 중인 팟(Pod) 리소스 요청 및 사용률이 낮은 작업자 풀을 확인하여 작업자 풀을 스케일링 업 또는 다운합니다.

## 확장 가능한 배치 사례 준수
{: #scalable-practices}

작업자 노드 및 앱 워크로드 배치 전략에 대해 다음 전략을 사용하여 클러스터 오토스케일러를 최대한 활용하십시오. 자세한 정보는 [Kubernetes 클러스터 오토스케일러 FAQ ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md)를 참조하십시오.
{: shortdesc}

[스케일링 업 및 스케일링 다운 작동](#ca_about) 방식, 구성할 수 있는 [사용자 정의 값](#ca_chart_values) 및 원하는 기타 측면(예: [과잉 프로비저닝](#ca_scaleup) 작업자 노드 또는 [앱 제한](#ca_limit_pool))을 알아보기 위해 일부 테스트 워크로드가 포함된 [클러스터 오토스케일러를 사용해 봅니다](#ca_helm). 그런 다음 테스트 환경을 정리하고 클러스터 오토스케일러의 새 설치와 함께 사용자 정의 값 및 추가 설정을 포함하도록 계획합니다.

### 한 번에 여러 작업자 풀을 오토스케일링할 수 있습니까?
{: #scalable-practices-multiple}
예, Helm 차트를 설치한 후 [Configmap](#ca_cm)을 오토스케일링할 클러스터 내의 작업자 풀을 선택할 수 있습니다. 클러스터당 하나의 `ibm-iks-cluster-autoscaler`Helm 차트만 실행할 수 있습니다.
{: shortdesc}

### 어떻게 하면 클러스터 오토스케일러가 내 앱에서 필요로 하는 항목에 대해 대응하도록 할 수 있습니까?
{: #scalable-practices-resrequests}

클러스터 오토스케일러는 워크로드 [리소스 요청 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)에 대한 응답으로 클러스터를 스케일링합니다. 따라서 리소스 요청은 클러스터 오토스케일러가 워크로드를 실행하는 데 필요한 작업자 노드 수를 계산하는 데 사용하므로 모든 배치에 대해 [리소스 요청 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)을 지정하십시오. 오토스케일링 기능은 워크로드 구성에서 요청하는 컴퓨팅 사용량을 기준으로 하며 시스템 비용과 같은 기타 요인을 고려하지 않습니다.
{: shortdesc}

### 작업자 풀을 0개의 노드로 스케일링 다운할 수 있습니까?
{: #scalable-practices-zero}

아니오, 클러스터 오토스케일러 `minSize`를 `0`으로 설정할 수는 없습니다. 또한, 클러스터의 각 구역에서 모든 공용 애플리케이션 로드 밸런서(ALB)를 [사용 안함](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)으로 설정하지 않는 한, ALB 팟(Pod)이 고가용성을 위해 분산될 수 있도록 `minSize`를 구역당 `2`개의 작업자 노드로 변경해야 합니다.
{: shortdesc}

### 오토스케일링을 위해 내 배치를 최적화할 수 있습니까?
{: #scalable-practices-apps}

예, 클러스터 오토스케일러가 스케일링을 위해 리소스 요청을 고려하는 방식을 조정하기 위해 여러 Kubernetes 기능을 배치에 추가할 수 있습니다.
{: shortdesc}
*   [팟(Pod) 중단 비용 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/)을 사용하여 갑작스런 팟(Pod)의 재스케줄링 또는 삭제를 방지하십시오.
*   팟(Pod) 우선순위를 사용하는 경우 [우선순위 컷오프를 편집 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-does-cluster-autoscaler-work-with-pod-priority-and-preemption)하여 우선순위 트리거 스케일링 업의 유형을 변경하십시오. 기본적으로, 우선순위 컷오프는 제로(`0`)입니다.

### 오토스케일링된 작업자 풀로 오염 및 결함을 사용할 수 있습니까?
{: #scalable-practices-taints}

작업자 풀 레벨에서 오염을 적용할 수 없으므로 예기치 않은 결과를 방지하기 위해 [작업자 노드를 오염](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)시키지 마십시오. 예를 들어, 오염된 작업자 노드가 허용하지 않는 워크로드를 배치하는 경우 작업자 노드는 스케일링 업을 고려하지 않으며 클러스터의 용량이 충분하더라도 추가 작업자 노드를 주문할 수 있습니다. 그러나 오염된 작업자 노드는 사용되는 리소스의 임계값(기본값: 50%) 미만이면 여전히 사용률이 낮은 것으로 식별되므로 스케일링 다운이 고려됩니다.
{: shortdesc}

### 오토스케일링된 작업자 풀이 불균형 상태인 이유가 무엇입니까?
{: #scalable-practices-unbalanced}

스케일링 업 중에 클러스터 오토스케일러는 구역 간의 노드를 밸런싱하며 허용된 차이는 더하기 또는 빼기 1(+/- 1) 작업자 노드입니다. 보류 중인 워크로드는 각 구역의 밸런싱을 유지하기 위해 충분한 용량을 요청하지 않을 수 있습니다. 이 경우, 작업자 풀을 수동으로 밸런싱하려는 경우에는 [클러스터 오토스케일러 configmap을 업데이트](#ca_cm)하여 불균형 상태의 작업자 풀을 제거하십시오. 그런 다음 `ibmcloud ks worker-pool-rebalance` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)을 실행하고 작업자 풀을 클러스터 오토스케일러 configmap에 다시 추가하십시오.
{: shortdesc}


### 내 작업자 풀을 크기 조정하거나 리밸런싱할 수 없는 이유는 무엇입니까?
{: #scalable-practices-resize}

클러스터 오토스케일러가 작업자 풀에 사용으로 설정되면 작업자 풀을 [크기 조정](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) 또는 [리밸런싱](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)할 수 없습니다. [configmap를 편집](#ca_cm)하여 작업자 풀 최소 또는 최대 크기를 변경하거나 해당 작업자 풀에 대한 클러스터 Auto-Scaling을 사용 안함으로 설정해야 합니다. 작업자 풀에서 개별 작업자 노드를 제거하기 위해 `ibmcloud ks worker-rm` [명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_rm)을 사용하지 마십시오. 작업자 풀의 불균형 상태를 초래할 수 있습니다.
{: shortdesc}

또한 `ibm-iks-cluster-autoscaler` Helm 차트를 설치 제거하기 전에 작업자 풀을 사용 안함으로 설정하지 않으면 수동으로 작업자 풀의 크기를 조정할 수 없습니다. `ibm-iks-cluster-autoscaler` Helm 차트를 다시 설치하고 [configmap을 편집](#ca_cm)하여 작업자 풀을 사용 안함으로 설정한 후 다시 시도하십시오.

<br />


## 클러스터에 클러스터 오토스케일러 Helm 차트 배치
{: #ca_helm}

Helm 차트를 사용하여 {{site.data.keyword.containerlong_notm}} 클러스터 오토스케일러 플러그인을 설치하여 클러스터에서 작업자 풀을 오토스케일링하십시오.
{: shortdesc}

**시작하기 전에**:

1.  [필수 CLI 및 플러그인을 설치](/docs/cli?topic=cloud-cli-getting-started)하십시오.
    *  {{site.data.keyword.Bluemix_notm}} CLI(`ibmcloud`)
    *  {{site.data.keyword.containerlong_notm}} 플러그인(`ibmcloud ks`)
    *  {{site.data.keyword.registrylong_notm}} 플러그인(`ibmcloud cr`)
    *  Kubernetes(`kubectl`)
    *  Helm(`helm`)
2.  **Kubernetes 버전 1.12 이상**을 실행하는 [표준 클러스터를 작성](/docs/containers?topic=containers-clusters#clusters_ui)하십시오.
3.   [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
4.  {{site.data.keyword.Bluemix_notm}} Identity and Access Management 인증 정보가 클러스터에 저장되어 있는지 확인하십시오. 클러스터 오토스케일러는 이 시크릿을 사용하여 인증 정보를 인증합니다. 시크릿이 누락된 경우에는 [인증 정보를 재설정하여 이를 작성](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions)하십시오.
    ```
    kubectl get secrets -n kube-system | grep storage-secret-store
    ```
    {: pre}
5.  클러스터 오토스케일러는 `ibm-cloud.kubernetes.io/worker-pool-id` 레이블이 있는 작업자 풀만 스케일링할 수 있습니다.
    1.  작업자 풀에 필수 레이블이 있는지 확인하십시오.
        ```
        ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels
        ```
        {: pre}

        레이블이 있는 작업자 풀의 예제 출력은 다음과 같습니다.
        ```
        Labels:             ibm-cloud.kubernetes.io/worker-pool-id=a1aa111111b22b22cc3c3cc444444d44-4d555e5
        ```
        {: screen}
    2.  작업자 풀에 필수 레이블이 없으면 [새 작업자 풀을 추가](/docs/containers?topic=containers-add_workers#add_pool)하여 이 작업자 풀을 클러스터 오토스케일러와 함께 사용하십시오.


<br>
**클러스터에 `ibm-iks-cluster-autoscaler` 플러그인을 설치하려면 다음을 수행하십시오.**

1.  [지시사항에 따라](/docs/containers?topic=containers-helm#public_helm_install) 로컬 시스템에 **Helm 버전 2.11 이상** 클라이언트를 설치하고 클러스터에 서비스 계정이 있는 Helm 서버(tiller)를 설치하십시오.
2.  Tiller가 서비스 계정으로 설치되어 있는지 확인하십시오.

    ```
    kubectl get serviceaccount -n kube-system tiller
    ```
    {: pre}

    출력 예:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}
3.  클러스터 오토스케일러 Helm 차트가 있는 Helm 저장소를 추가하고 업데이트하십시오.
    ```
    helm repo add iks-charts https://icr.io/helm/iks-charts
    ```
    {: pre}
    ```
    helm repo update
    ```
    {: pre}
4.  클러스터의 `kube-system` 네임스페이스에 클러스터 오토스케일러 Helm 차트를 설치하십시오.

    설치 중에, 작업자 노드를 스케일링 업 또는 다운하기 전에 대기하는 시간과 같은 [클러스터 오토스케일러 설정을 추가로 사용자 정의](#ca_chart_values)할 수도 있습니다.
    {: tip}

    ```
    helm install iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler
    ```
    {: pre}

    출력 예:
    ```
    NAME:   ibm-iks-cluster-autoscaler
    LAST DEPLOYED: Thu Nov 29 13:43:46 2018
    NAMESPACE: kube-system
    STATUS: DEPLOYED

    RESOURCES:
    ==> v1/ClusterRole
    NAME                       AGE
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Pod(related)

    NAME                                        READY  STATUS             RESTARTS  AGE
    ibm-iks-cluster-autoscaler-67c8f87b96-qbb6c  0/1    ContainerCreating  0         1s

    ==> v1/ConfigMap

    NAME              AGE
    iks-ca-configmap  1s

    ==> v1/ClusterRoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Role
    ibm-iks-cluster-autoscaler  1s

    ==> v1/RoleBinding
    ibm-iks-cluster-autoscaler  1s

    ==> v1/Service
    ibm-iks-cluster-autoscaler  1s

    ==> v1beta1/Deployment
    ibm-iks-cluster-autoscaler  1s

    ==> v1/ServiceAccount
    ibm-iks-cluster-autoscaler  1s

    NOTES:
    Thank you for installing: ibm-iks-cluster-autoscaler. Your release is named: ibm-iks-cluster-autoscaler

    클러스터 오토스케일러 사용에 대한 자세한 정보는 차트 README.md 파일을 참조하십시오.
    ```
    {: screen}

5.  설치가 완료되었는지 확인하십시오.

    1.  클러스터 오토스케일러 팟(Pod)이 **실행 중** 상태인지 확인하십시오.
        ```
        kubectl get pods --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        출력 예:
        ```
        ibm-iks-cluster-autoscaler-8497bfc968-dbn7w   1/1       Running   0          9m
        ```
        {: screen}
    2.  클러스터 오토스케일러 서비스가 작성되었는지 확인하십시오.
        ```
        kubectl get service --namespace=kube-system | grep ibm-iks-cluster-autoscaler
        ```
        {: pre}
        출력 예:
        ```
        ibm-iks-cluster-autoscaler   ClusterIP   172.21.xxx.xx    <none>        8085/TCP        9m
        ```
        {: screen}

6.  클러스터 오토스케일러를 프로비저닝할 모든 클러스터에 대해 이러한 단계를 반복하십시오.

7.  작업자 풀 스케일링을 시작하려면 [클러스터 오토스케일러 구성 업데이트](#ca_cm)를 참조하십시오.

<br />


## 클러스터 오토스케일러 configmap를 업데이트하여 스케일링 사용
{: #ca_cm}

설정한 최소값 및 최대값을 기준으로 작업자 풀에서 작업자 노드를 자동으로 스케일링할 수 있도록 클러스터 오토스케일러 configmap을 업데이트합니다.
{: shortdesc}

ConfigMap을 편집하여 작업자 풀을 사용으로 설정하면 클러스터 오토스케일러가 워크로드 요청에 대응하여 클러스터를 스케일링하기 시작합니다. 이와 같이 작업자 풀을 [크기 조정](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) 또는 [리밸런싱](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)할 수 없습니다. 시간이 경과함에 따라 정기적으로 스캔과 스케일링 업 및 다운이 수행되며 작업자 노드의 수에 따라 완료하는 데 더 오랜 시간(예: 30분)이 소요될 수 있습니다. 나중에 [클러스터 오토스케일러를 제거](#ca_rm)하려면 먼저 configmap의 각 작업자 풀을 사용 안함으로 설정해야 합니다.
{: note}

**시작하기 전에**:
*  [`ibm-iks-cluster-autoscaler` 플러그인을 설치](#ca_helm)하십시오.
*  [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**클러스터 오토스케일러 configmap 및 값을 업데이트하려면 다음을 수행하십시오.**

1.  클러스터 오토스케일러 configmap YAML 파일을 편집하십시오.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system -o yaml
    ```
    {: pre}
    출력 예:
    ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false}
        ]
    kind: ConfigMap
    metadata:
      creationTimestamp: 2018-11-29T18:43:46Z
      name: iks-ca-configmap
      namespace: kube-system
      resourceVersion: "2227854"
      selfLink: /api/v1/namespaces/kube-system/configmaps/iks-ca-configmap
      uid: b45d047b-f406-11e8-b7f0-82ddffc6e65e
    ```
    {: screen}
2.  매개변수를 사용하여 configmap을 편집하여 클러스터 오토스케일러가 클러스터 작업자 풀을 스케일링하는 방법을 정의하십시오. **참고:** 표준 클러스터의 각 구역에서 모든 공용 애플리케이션 로드 밸런서(ALB)를 [사용 안함으로 설정](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)하지 않는 한, ALB 팟(Pod)이 고가용성을 위해 분산될 수 있도록 `minSize`를 구역당 `2`개로 변경해야 합니다. 

    <table>
    <caption>클러스터 오토스케일러 configmap 매개변수</caption>
    <thead>
    <th id="parameter-with-default">기본값이 있는 매개변수</th>
    <th id="parameter-with-description">설명</th>
    </thead>
    <tbody>
    <tr>
    <td id="parameter-name" headers="parameter-with-default">`"name": "default"`</td>
    <td headers="parameter-name parameter-with-description">`"default"`를 스케일링하려는 작업자 풀의 이름 또는 ID로 대체하십시오. 작업자 풀을 나열하려면 `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`를 실행하십시오.<br><br>
    둘 이상의 작업자 풀을 관리하려면 다음과 같이 JSON 행을 쉼표로 구분된 행으로 복사하십시오. <pre class="codeblock">[
     {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
     {"name": "Pool2","minSize": 2,"maxSize": 5,"enabled":true}
    ]</pre><br><br>
    **참고**: 클러스터 오토스케일러는 `ibm-cloud.kubernetes.io/worker-pool-id` 레이블이 있는 작업자 풀만 스케일링할 수 있습니다. 작업자 풀에 필수 레이블이 있는지 확인하려면 `ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <worker_pool_name_or_ID> | grep Labels`를 실행하십시오. 작업자 풀에 필수 레이블이 없으면 [새 작업자 풀을 추가](/docs/containers?topic=containers-add_workers#add_pool)하여 이 작업자 풀을 클러스터 오토스케일러와 함께 사용하십시오.</td>
    </tr>
    <tr>
    <td id="parameter-minsize" headers="parameter-with-default">`"minSize": 1`</td>
    <td headers="parameter-minsize parameter-with-description">클러스터 오토스케일러가 작업자 풀을 스케일링 다운할 수 있는 구역당 최소 작업자 노드 수를 지정하십시오. 고가용성을 위해 ALB 팟(Pod)이 분산될 수 있도록 값은 `2` 이상이어야 합니다. 표준 클러스터의 각 구역에서 모든 공용 ALB를 [사용 안함으로 설정](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)한 경우에는 이 값을 `1`로 설정할 수 있습니다.
    <p class="note">`minSize` 설정이 스케일링 업을 자동으로 트리거하지는 않습니다. `minSize`는 클러스터 오토스케일러가 각 구역을 특정 작업자 노드 수보다 적게 스케일링하지 않도록 하는 임계값입니다. 자신의 클러스터에 있는 구역당 작업자 노드 수가 아직 이보다 모자란 경우 클러스터 오토스케일러는 추가 리소스를 필요로 하는 워크로드 리소스가 생길 때까지 스케일링 업을 수행하지 않습니다. 예를 들어, 세 구역 각각에 하나의 작업자 노드가 있는 작업자 풀이 있으며(총 세 개의 작업자 노드) `minSize`를 구역당 `4`로 설정하는 경우 클러스터 오토스케일러는 각 구역에서 즉시 세 작업자 노드를 추가로 프로비저닝하지 않습니다(총 12개의 작업자 노드). 대신 이 스케일링 업은 리소스 요청에 의해 트리거됩니다. 15개 작업자 노드의 리소스를 요청하는 워크로드를 작성하는 경우 클러스터 오토스케일러는 이 요청을 만족시키기 위해 작업자 풀을 스케일링 업합니다. `minSize`는 이제 구역당 네 개의 작업자 노드를 요청하는 워크로드를 제거하는 경우에도 클러스터 오토스케일러가 작업자 노드를 이보다 적게 스케일링 다운하지 않음을 의미합니다. 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#when-does-cluster-autoscaler-change-the-size-of-a-cluster)를 참조하십시오. </p></td>
    </tr>
    <tr>
    <td id="parameter-maxsize" headers="parameter-with-default">`"maxSize": 2`</td>
    <td headers="parameter-maxsize parameter-with-description">클러스터 오토스케일러가 작업자 풀을 스케일링 업할 수 있는 구역당 최대 작업자 노드 수를 지정하십시오. 값은 `minSize`에 설정한 값 이상이어야 합니다.</td>
    </tr>
    <tr>
    <td id="parameter-enabled" headers="parameter-with-default">`"enabled": false`</td>
    <td headers="parameter-enabled parameter-with-description">클러스터 오토스케일러에서 작업자 풀의 스케일링을 관리하려면 이 값을 `true`로 설정합니다. 클러스터 오토스케일러에서 작업자 풀의 스케일링을 중지하려면 이 값을 `false`로 설정합니다.<br><br>
    나중에 [클러스터 오토스케일러를 제거](#ca_rm)하려면 먼저 configmap의 각 작업자 풀을 사용 안함으로 설정해야 합니다.</td>
    </tr>
    </tbody>
    </table>
3.  구성 파일을 저장하십시오.
4.  클러스터 오토스케일러 팟(Pod)을 가져오십시오.
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
5.  **`ConfigUpdated`** 이벤트에 대한 클러스터 오토스케일러의 **`이벤트`** 섹션을 검토하여 configmap이 업데이트되었는지 확인하십시오. configmap에 대한 이벤트 메시지는 다음과 같은 형식입니다. `minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`

    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    출력 예:
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
    Type    Reason                 Age   From                     Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

## 클러스터 오토스케일러 Helm 차트 구성 값 사용자 정의
{: #ca_chart_values}

작업자 노드를 스케일링 업 또는 다운하기 전에 대기하는 시간과 같은 클러스터 오토스케일러 설정을 사용자 정의합니다.
{: shortdesc}

**시작하기 전에**:
*  [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  [`ibm-iks-cluster-autoscaler` 플러그인을 설치](#ca_helm)하십시오.

**클러스터 오토스케일러 값을 업데이트하려면 다음을 수행하십시오.**

1.  클러스터 오토스케일러 Helm 차트 구성 값을 검토하십시오. 클러스터 오토스케일러에는 기본 설정이 포함되어 있습니다. 그러나 클러스터 워크로드를 변경하는 빈도에 따라 스케일링 다운 또는 스캔 간격과 같은 일부 값을 변경할 수 있습니다.
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}

    출력 예:
    ```
    expander: least-waste
    image:
      pullPolicy: Always
      repository: icr.io/iks-charts/ibm-iks-cluster-autoscaler
      tag: dev1
    maxNodeProvisionTime: 120m
    resources:
      limits:
        cpu: 300m
        memory: 300Mi
      requests:
        cpu: 100m
        memory: 100Mi
    scaleDownDelayAfterAdd: 10m
    scaleDownDelayAfterDelete: 10m
    scaleDownUtilizationThreshold: 0.5
    scaleDownUnneededTime: 10m
    scanInterval: 1m
    skipNodes:
      withLocalStorage: true
      withSystemPods: true
    ```
    {: codeblock}

    <table>
    <caption>클러스터 오토스케일러 구성 값</caption>
    <thead>
    <th>매개변수</th>
    <th>설명</th>
    <th>기본값</th>
    </thead>
    <tbody>
    <tr>
    <td>`api_route` 매개변수</td>
    <td>클러스터가 있는 지역에 대한 [{{site.data.keyword.containerlong_notm}} API 엔드포인트](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cli_api)를 설정하십시오.</td>
    <td>기본값이 없습니다. 클러스터가 있는 대상 지역을 사용합니다.</td>
    </tr>
    <tr>
    <td>`expander` 매개변수</td>
    <td>여러 개의 작업자 풀이 있는 경우 클러스터 오토스케일러가 스케일링할 작업자 풀을 판별하는 방법을 지정합니다. 가능한 값은 다음과 같습니다.
    <ul><li>`random`: `most-pods` 및 `least-waste` 간에 무작위로 선택합니다.</li>
    <li>`most-pods`: 스케일링 업 시 가장 많은 팟(Pod)을 스케줄할 수 있는 작업자 풀을 선택합니다. `nodeSelector`를 사용하여 특정 작업자 노드에 팟(Pod)이 있는지 확인하려는 경우 이 방법을 사용하십시오.</li>
    <li>`least-waste`: 스케일링 업 후 사용되지 않는 CPU가 가장 적은 작업자 풀을 선택합니다. 스케일링 업 후 두 작업자 풀의 CPU 리소스 양이 동일한 경우에는 사용되지 않는 메모리가 가장 적은 작업자 풀이 선택됩니다. </li></ul></td>
    <td>랜덤</td>
    </tr>
    <tr>
    <td>`image.repository` 매개변수</td>
    <td>사용할 클러스터 오토스케일러 Docker 이미지를 지정하십시오.</td>
    <td>`icr.io/iks-charts/ibm-iks-cluster-autoscaler`</td>
    </tr>
    <tr>
    <td>`image.pullPolicy` 매개변수</td>
    <td>Docker 이미지를 가져올 시기를 지정합니다. 가능한 값은 다음과 같습니다.
    <ul><li>`Always`: 팟(Pod)이 시작될 때마다 이미지를 가져옵니다.</li>
    <li>`IfNotPresent`: 이미지가 이미 로컬에 존재하지 않는 경우에만 이미지를 가져옵니다.</li>
    <li>`Never`: 이미지가 로컬로 존재하고 이미지를 가져오지 않는다고 가정합니다.</li></ul></td>
    <td>Always</td>
    </tr>
    <tr>
    <td>`maxNodeProvisionTime` 매개변수</td>
    <td>클러스터 오토스케일러가 스케일링 업 요청을 취소하기 전에 작업자 노드가 프로비저닝을 시작하는 데 걸릴 수 있는 최대 시간(분)을 설정합니다.</td>
    <td>`120m`</td>
    </tr>
    <tr>
    <td>`resources.limits.cpu` 매개변수</td>
    <td>`ibm-iks-cluster-autoscaler` 팟(Pod)을 이용할 수 있는 최대 작업자 노드 CPU 양을 설정합니다.</td>
    <td>`300m`</td>
    </tr>
    <tr>
    <td>`resources.limits.memory` 매개변수</td>
    <td>`ibm-iks-cluster-autoscaler` 팟(Pod)을 이용할 수 있는 최대 작업자 노드 메모리 양을 설정합니다.</td>
    <td>`300Mi`</td>
    </tr>
    <tr>
    <td>`resources.requests.cpu` 매개변수</td>
    <td>`ibm-iks-cluster-autoscaler` 팟(Pod)을 시작할 최소 작업자 노드 CPU 양을 설정합니다.</td>
    <td>`100m`</td>
    </tr>
    <tr>
    <td>`resources.requests.memory` 매개변수</td>
    <td>`ibm-iks-cluster-autoscaler` 팟(Pod)을 시작할 최소 작업자 노드 메모리 양을 설정합니다.</td>
    <td>`100Mi`</td>
    </tr>
    <tr>
    <td>`scaleDownUnneededTime` 매개변수</td>
    <td>작업자 노드가 스케일링 다운되기 전에 작업자 노드가 필요하지 않아야 하는 시간(분)을 설정합니다.</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>`scaleDownDelayAfterAdd`, `scaleDownDelayAfterDelete` 매개변수</td>
    <td>스케일링 업(`add`) 또는 스케일링 다운(`delete`) 후에 클러스터 오토스케일러가 스케일링 조치를 다시 시작하기 위해 대기하는 시간(분)을 설정합니다.</td>
    <td>`10m`</td>
    </tr>
    <tr>
    <td>`scaleDownUtilizationThreshold` 매개변수</td>
    <td>작업자 노드 활용 임계값을 설정합니다. 작업자 노드 사용률이 임계값 미만이면 작업자 노드는 스케일링 다운된 것으로 간주됩니다. 작업자 노드 사용률은 작업자 노드에서 실행되는 모든 팟(Pod)에서 요청한 CPU 및 메모리 리소스의 합계를 작업자 노드 리소스 용량으로 나눈 값으로 계산됩니다. </td>
    <td>`0.5`</td>
    </tr>
    <tr>
    <td>`scanInterval` 매개변수</td>
    <td>클러스터 오토스케일러가 스케일링 업 또는 다운을 트리거하는 워크로드 사용량을 스캔하는 빈도를 분 단위로 설정합니다.</td>
    <td>`1m`</td>
    </tr>
    <tr>
    <td>`skipNodes.withLocalStorage` 매개변수</td>
    <td>`true`로 설정하면 로컬 스토리지에 데이터를 저장하고 있는 팟(Pod)이 있는 작업자 노드는 스케일링 다운되지 않습니다.</td>
    <td>`true`</td>
    </tr>
    <tr>
    <td>`skipNodes.withSystemPods` 매개변수</td>
    <td>`true`로 설정하면 `kube-system` 팟(Pod)이 있는 작업자 노드는 스케일링 다운되지 않습니다. `kube-system` 팟(Pod)을 스케일링 다운하면 예기치 않은 결과가 발생하므로 이 값을 `false`로 설정하지 마십시오.</td>
    <td>`true`</td>
    </tr>
    </tbody>
    </table>
2.  클러스터 오토스케일러 구성 값을 변경하려면 새 값으로 Helm 차트를 업데이트하십시오. 기존 클러스터 오토스케일러 팟(Pod)이 사용자 정의 설정 변경사항을 선택하기 위해 다시 작성될 수 있도록 `--recreate-pods` 플래그를 포함하십시오.
    ```
    helm upgrade --set scanInterval=2m ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler -i --recreate-pods
    ```
    {: pre}

    차트를 기본값으로 재설정하려면 다음을 수행하십시오.
    ```
    helm upgrade --reset-values ibm-iks-cluster-autoscaler iks-charts/ibm-iks-cluster-autoscaler --recreate-pods
    ```
    {: pre}
3.  변경사항을 확인하려면 Helm 차트 값을 다시 검토하십시오.
    ```
    helm get values ibm-iks-cluster-autoscaler -a
    ```
    {: pre}


## 오토스케일링된 특정 작업자 풀에서만 실행되도록 앱 제한
{: #ca_limit_pool}

팟(Pod) 배치를 클러스터 오토스케일러가 관리하는 특정 작업자 풀로 제한하려면 레이블과 `nodeSelector` 또는 `nodeAffinity`를 사용하십시오. `nodeAffinity`를 사용하면 스케줄링 작업이 팟(Pod)과 작업자 노드를 대응시키는 방식을 제어하는 기능이 향상됩니다. 팟(Pod)을 작업자 노드에 지정하는 데 대한 자세한 정보는 [Kubernetes 문서를 참조 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/)하십시오.
{: shortdesc}

**시작하기 전에**:
*  [`ibm-iks-cluster-autoscaler` 플러그인을 설치](#ca_helm)하십시오.
*  [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**오토스케일링된 특정 작업자 풀에서 실행되도록 앱 제한**:

1.  사용할 레이블이 있는 작업자 풀을 작성하십시오. 예를 들면, 레이블이 `app: nginx`와 같을 수 있습니다.
    ```
    ibmcloud ks worker-pool-create --name <name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_worker_nodes> --labels <key>=<value>
    ```
    {: pre}
2.  [클러스터 오토스케일러 구성에 작업자 풀을 추가](#ca_cm)하십시오.
3.  팟(Pod) 스펙 템플리트에서 `nodeSelector` 또는 `nodeAffinity`를 작업자 풀에서 사용한 레이블에 대응시키십시오. 

    `nodeSelector`의 예:
    ```
    ...
    spec:
      containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
      nodeSelector:
        app: nginx
    ...
    ```
    {: codeblock}

    `nodeAffinity`의 예:
    ```
    spec:
      containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: app
                operator: In
                values:
                - nginx
    ```
    {: codeblock}
4.  팟(Pod)을 배치하십시오. 일치하는 레이블로 인해 팟(Pod)은 레이블 지정된 작업자 풀에 있는 작업자 노드에 스케줄됩니다.
    ```
    kubectl apply -f pod.yaml
    ```
    {: pre}

<br />


## 작업자 풀에 리소스가 부족하기 전에 작업자 노드 스케일링 업
{: #ca_scaleup}

[클러스터 오토스케일러 작동 방법 이해](#ca_about) 주제와 [Kubernetes 클러스터 오토스케일러 FAQ ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md)에 설명되어 있는 바와 같이, 클러스터 오토스케일러는 작업자 풀의 사용 가능한 리소스에 대한 워크로드의 리소스 요청에 대응하여 작업자 풀을 스케일링 업합니다. 그러나, 작업자 풀에 리소스가 부족해지기 전에 클러스터 오토스케일러가 작업자 노드를 스케일링 업하도록 할 수 있습니다. 이 경우 작업자 풀이 리소스 요청을 충족하도록 이미 스케일링 업되었기 때문에 워크로드는 작업자 노드가 프로비저닝되는 동안 오래 대기할 필요가 없습니다.
{: shortdesc}

클러스터 오토스케일러는 작업자 풀의 초기 스케일링(과잉 프로비저닝)을 지원하지 않습니다. 그러나 다른 Kubernetes 리소스를 구성하여 초기 스케일링을 달성하도록 클러스터 오토스케일러에 대한 작업을 수행할 수 있습니다.

<dl>
  <dt><strong>일시정지 팟(Pod)</strong></dt>
  <dd>특정 리소스 요청이 있는 팟(Pod)에 [일시정지 컨테이너 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://stackoverflow.com/questions/48651269/what-are-the-pause-containers)에 배치하는 배치를 작성하고 해당 배치에 낮은 팟(Pod) 우선순위를 지정할 수 있습니다. 이러한 리소스가 우선순위가 높은 워크로드에 필요하면 일시중지 팟(Pod)이 선취되어 보류 중인 팟(Pod)이 됩니다. 이 이벤트는 클러스터 오토스케일러가 스케일링 업하도록 트리거합니다.<br><br>일시정지 팟(Pod) 배치 설정에 대한 자세한 정보는 [Kubernetes FAQ ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-can-i-configure-overprovisioning-with-cluster-autoscaler)를 참조하십시오. [구성 파일을 과잉 프로비저닝하는 이 예 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/IBM-Cloud/kube-samples/blob/master/ibm-ks-cluster-autoscaler/overprovisioning-autoscaler.yaml)를 사용하여 우선순위 클래스, 서비스 계정 및 배치를 작성할 수 있습니다.<p class="note">이 방법을 사용하는 경우 [팟(Pod) 우선순위](/docs/containers?topic=containers-pod_priority#pod_priority)가 작동하는 방법을 이해하고 배치에 팟(Pod)을 설정하는 방법을 확인하십시오. 예를 들어, 일시정지 팟(Pod)에 상위 우선순위에 필요한 충분한 리소스가 없는 경우 해당 팟(Pod)은 선취되지 않습니다. 우선순위가 높은 워크로드는 보류 중으로 유지되므로 클러스터 오토스케일러가 스케일링 업되도록 트리거됩니다. 그러나 이 경우, 충분하지 않은 리소스로 인해 실행할 워크로드를 스케줄할 수 없으므로 스케일링 업 조치가 이른 것은 아닙니다. </p></dd>

  <dt><strong>수평 팟(Pod) Auto-Scaling(HPA)</strong></dt>
  <dd>수평 팟(Pod) 오토스케일링 기능은 팟(Pod)의 평균 CPU 사용량을 기준으로 하므로 작업자 풀에 리소소가 부족해지기 전에 설정한 CPU 사용량 한계에 도달합니다. 추가 팟(Pod)이 요청되면 클러스터 오토스케일러가 작업자 풀을 스케일링 업하도록 트리거합니다.<br><br>HPA 설정에 대한 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/)를 참조하십시오. </dd>
</dl>

<br />


## 클러스터 오토스케일러 Helm 차트 업데이트
{: #ca_helm_up}

기존 클러스터 오토스케일러 Helm 차트를 최신 버전으로 업데이트할 수 있습니다. 현재 Helm 차트 버전을 확인하려면 `helm ls | grep cluster-autoscaler`를 실행하십시오.
{: shortdesc}

버전 1.0.2 이하에서 최신 Helm 차트로 업데이트 [다음 지시사항을 따르십시오](#ca_helm_up_102).
{: note}

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Helm 저장소를 업데이트하여 이 저장소에 있는 모든 Helm 차트의 최신 버전을 검색하십시오.
    ```
    helm repo update
    ```
    {: pre}

2.  선택사항: 최신 Helm 차트를 로컬 머신에 다운로드하십시오. 그런 다음, 패키지의 압축을 풀고 `release.md` 파일을 검토하여 최신 릴리스 정보를 찾으십시오.
    ```
    helm fetch iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

3.  클러스터에 설치한 클러스터 오토스케일러 Helm 차트의 이름을 찾으십시오.
    ```
    helm ls | grep cluster-autoscaler
    ```
    {: pre}

    출력 예:
    ```
    myhelmchart 	1       	Mon Jan  7 14:47:44 2019	DEPLOYED	ibm-ks-cluster-autoscaler-1.0.0  	kube-system
    ```
    {: screen}

4.  클러스터 오토스케일러 Helm 차트를 최신 버전으로 업데이트하십시오.
    ```
    helm upgrade --force --recreate-pods <helm_chart_name>  iks-charts/ibm-iks-cluster-autoscaler
    ```
    {: pre}

5.  스케일링할 작업자 풀에 대한 [cluster autoscaler configmap](#ca_cm) `workerPoolsConfig.json` 섹션이 `"enabled": true`로 설정되어 있는지 확인하십시오.
    ```
    kubectl describe cm iks-ca-configmap -n kube-system
    ```
    {: pre}

    출력 예:
    ```
    Name:         iks-ca-configmap
    Namespace:    kube-system
    Labels:       <none>
    Annotations:  kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","data":{"workerPoolsConfig.json":"[\n {\"name\": \"docs\",\"minSize\": 1,\"maxSize\": 3,\"enabled\":true}\n]\n"},"kind":"ConfigMap",...

    Data
    ====
    workerPoolsConfig.json:
    ----
    [
     {"name": "docs","minSize": 1,"maxSize": 3,"enabled":true}
    ]

    Events:  <none>
    ```
    {: screen}

### 버전 1.0.2 이하에서 최신 Helm 차트로 업데이트
{: #ca_helm_up_102}

클러스터 오토스케일러의 최신 Helm 차트 버전은 이전에 설치된 클러스터 오토스케일러 Helm 차트 버전의 완전한 제거가 필요합니다. Helm 차트 버전 1.0.2 이하 버전을 설치한 경우, 클러스터 오토스케일러의 최신 Helm 차트를 설치하기 전에 먼저 해당 버전을 설치 제거하십시오.
{: shortdesc}

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  클러스터 오토스케일러 configmap을 가져오십시오.
    ```
    kubectl get cm iks-ca-configmap -n kube-system -o yaml > iks-ca-configmap.yaml
    ```
    {: pre}
2.  `"enabled"` 값을 `false`로 설정하여 configmap에서 모든 작업자 풀을 제거하십시오.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
3.  Helm 차트에 사용자 정의 설정을 적용한 경우 사용자 정의 설정을 기록해 두십시오.
    ```
    helm get values ibm-ks-cluster-autoscaler -a
    ```
    {: pre}
4.  현재 Helm 차트를 설치 제거하십시오.
    ```
    helm delete --purge ibm-ks-cluster-autoscaler
    ```
    {: pre}
5.  Helm 차트 저장소를 업데이트하여 최신 클러스터 오토스케일러 Helm 차트 버전을 가져오십시오.
    ```
    helm repo update
    ```
    {: pre}
6.  최신 클러스터 오토스케일러 Helm 차트를 설치하십시오. 이전에 사용한 모든 사용자 정의 설정을 `--set` 플래그(예: `scanInterval=2m`)로 적용하십시오.
    ```
    helm install  iks-charts/ibm-iks-cluster-autoscaler --namespace kube-system --name ibm-iks-cluster-autoscaler [--set <custom_settings>]
    ```
    {: pre}
7.  이전에 검색한 클러스터 오토스케일러 configmap을 적용하여 작업자 풀에 대한 오토스케일러를 사용으로 설정하십시오.
    ```
    kubectl apply -f iks-ca-configmap.yaml
    ```
    {: pre}
8.  클러스터 오토스케일러 팟(Pod)을 가져오십시오.
    ```
    kubectl get pods -n kube-system
    ```
    {: pre}
9.  클러스터 오토스케일러의 **`이벤트`** 섹션을 검토하고 **`ConfigUpdated`** 이벤트를 찾아 configmap이 업데이트되었는지 확인하십시오. configmap에 대한 이벤트 메시지는 다음과 같은 형식입니다. `minSize:maxSize:PoolName:<SUCCESS|FAILED>:error message`
    ```
    kubectl describe pod -n kube-system <cluster_autoscaler_pod>
    ```
    {: pre}

    출력 예:
    ```
		Name:               ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6
		Namespace:          kube-system
		...
		Events:
    Type    Reason                 Age   From                     Message
		----     ------         ----  ----                                        -------

		Normal  ConfigUpdated  3m    ibm-iks-cluster-autoscaler-857c4d9d54-gwvc6  {"1:3:default":"SUCCESS:"}
    ```
    {: screen}

<br />


## 클러스터 오토스케일러 제거
{: #ca_rm}

작업자 풀을 자동으로 스케일링하지 않으려는 경우, 클러스터 오토스케일러 Helm 차트를 설치 제거할 수 있습니다. 제거 후 작업자 풀을 수동으로 [크기 조정](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) 또는 [리밸런싱](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)해야 합니다.
{: shortdesc}

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  [클러스터 오토스케일러 configmap](#ca_cm)에서 `"enabled"` 값을 `false`로 설정하여 작업자 풀을 제거하십시오.
    ```
    kubectl edit cm iks-ca-configmap -n kube-system
    ```
    {: pre}
    출력 예:
    ```
    apiVersion: v1
    data:
      workerPoolsConfig.json: |
        [
         {"name": "default","minSize": 1,"maxSize": 2,"enabled":false},
         {"name":"mypool","minSize":1,"maxSize":5,"enabled":false}
        ]
    kind: ConfigMap
    ...
    ```
2.  기존 Helm 차트를 나열하고 클러스터 오토스케일러의 이름을 기록해 두십시오.
    ```
    helm ls
    ```
    {: pre}
3.  클러스터에서 기존 Helm 차트를 제거하십시오.
    ```
    helm delete --purge <ibm-iks-cluster-autoscaler_name>
    ```
    {: pre}
