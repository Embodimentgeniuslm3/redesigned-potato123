---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, clusters, worker nodes, worker pools, delete

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


# 클러스터 제거
{: #remove}

청구 가능 계정으로 작성된 무료 및 표준 클러스터가 더 이상 필요하지 않은 경우에는 해당 클러스터가 더 이상 리소스를 이용하지 않도록 이를 수동으로 제거해야 합니다.
{:shortdesc}

<p class="important">
클러스터 또는 지속적 스토리지의 데이터에 대한 백업이 작성되지 않았습니다. 클러스터를 삭제할 때 지속적 스토리지를 삭제하도록 선택할 수 있습니다. 지속적 스토리지를 삭제하도록 선택하면 `delete` 스토리지 클래스를 사용하여 프로비저닝한 지속적 스토리지는 IBM Cloud 인프라(SoftLayer)에서 영구적으로 삭제됩니다. `retain` 스토리지 클래스를 사용하여 지속적 스토리지를 프로비저닝하고 스토리지를 삭제하도록 선택한 경우, 클러스터, PV 및 PVC는 삭제되지만 IBM Cloud 인프라(SoftLayer) 계정의 지속적인 스토리지 인스턴스는 유지됩니다.</br>
</br>클러스터를 제거하면 클러스터를 작성할 때 자동으로 프로비저닝된 서브넷과 `ibmcloud ks cluster-subnet-create` 명령을 사용하여 작성된 서브넷도 역시 제거됩니다. 그러나 `ibmcloud ks cluster-subnet-add` 명령을 사용하여 클러스터에 기존 서브넷을 수동으로 추가한 경우, 이러한 서브넷은 IBM Cloud 인프라(SoftLayer) 계정에서 제거되지 않으며 다른 클러스터에서 이를 재사용할 수 있습니다.</p>

시작하기 전에:
* 클러스터 ID를 기록해 두십시오. 클러스터와 함께 자동으로 삭제되지 않는 관련 IBM Cloud 인프라(SoftLayer) 리소스를 조사하여 이를 제거하려면 클러스터 ID가 필요할 수 있습니다.
* 지속적 스토리지의 데이터를 삭제하려면 [삭제 옵션을 잘 숙지](/docs/containers?topic=containers-cleanup#cleanup)하십시오.
* [**관리자** {{site.data.keyword.Bluemix_notm}} IAM 플랫폼 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.

클러스터를 제거하려면 다음을 수행하십시오.

1. 선택사항: CLI에서 클러스터에 있는 모든 데이터의 사본을 로컬 YAML 파일에 저장하십시오. 
  ```
  kubectl get all --all-namespaces -o yaml
  ```
  {: pre}

2. 클러스터를 제거하십시오. 
  - {{site.data.keyword.Bluemix_notm}} 콘솔에서
    1.  클러스터를 선택하고 **추가 조치...** 메뉴에서 **삭제**를 클릭하십시오.

  - {{site.data.keyword.Bluemix_notm}} CLI에서
    1.  사용 가능한 클러스터를 나열하십시오.

        ```
        ibmcloud ks clusters
        ```
        {: pre}

    2.  클러스터를 삭제하십시오.

        ```
        ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
        ```
        {: pre}

    3.  프롬프트에 따라 클러스터 리소스(컨테이너, 팟(Pod), 바인딩된 서비스, 지속적 스토리지, 시크릿 등) 삭제 여부를 선택하십시오.
      - **지속적 스토리지**: 지속적 스토리지는 데이터에 대한 고가용성을 제공합니다. [기존 파일 공유](/docs/containers?topic=containers-file_storage#existing_file)를 사용하여 지속적 볼륨 클레임을 작성한 경우 클러스터를 삭제할 때 파일 공유를 삭제할 수 없습니다. 나중에 IBM Cloud 인프라(SoftLayer) 포트폴리오에서 파일 공유를 수동으로 삭제해야 합니다.

          월별 비용 청구 주기로 인해, 매월 말일에는 지속적 볼륨 클레임(PVC)을 삭제할 수 없습니다. 해당 월의 말일에 지속적 볼륨 클레임을 삭제하는 경우 다음 달이 시작될 때까지 삭제는 보류 상태를 유지합니다.
          {: note}

다음 단계:
- 제거된 클러스터의 이름이 `ibmcloud ks clusters` 명령 실행 시 사용 가능한 클러스터 목록에 더 이상 나열되지 않으면 이 이름을 재사용할 수 있습니다.
- 서브넷을 보존한 경우에는 [이를 새 클러스터에서 다시 사용](/docs/containers?topic=containers-subnets#subnets_custom)하거나 나중에 IBM Cloud 인프라(SoftLayer) 포트폴리오에서 수동으로 삭제할 수 있습니다.
- 지속적 스토리지를 보유한 경우에는 {{site.data.keyword.Bluemix_notm}} 콘솔의 IBM Cloud 인프라(SoftLayer) 대시보드를 통해 나중에 [스토리지를 삭제](/docs/containers?topic=containers-cleanup#cleanup)할 수 있습니다.
