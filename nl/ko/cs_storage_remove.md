---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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



# 클러스터에서 지속적 스토리지 제거
{: #cleanup}

클러스터에서 지속적 스토리지를 설정하는 경우에는 스토리지를 요청하는 Kubernetes 지속적 볼륨 클레임(PVC), 팟(Pod)에 마운트되고 PVC에서 설명되는 Kubernetes 지속적 볼륨(PV), 그리고 IBM Cloud 인프라(SoftLayer) 인스턴스(예: NFS 파일 또는 블록 스토리지)의 3개의 기본 컴포넌트가 있습니다. 스토리지를 작성한 방법에 따라 3개 모두를 별도로 삭제해야 할 수 있습니다.
{:shortdesc}

## 지속적 스토리지 정리
{: #storage_remove}

삭제 옵션 이해:

**내 클러스터를 삭제했습니다. 지속적 스토리지를 제거하려면 다른 것도 삭제해야 합니까?**</br>
경우에 따라 다릅니다. 클러스터를 삭제하면 PVC 및 PV 역시 삭제됩니다. 그러나 사용자는 IBM Cloud 인프라(SoftLayer)에서 연관된 스토리지 인스턴스의 제거 여부를 선택합니다. 이를 제거하지 않기로 선택한 경우에는 스토리지 인스턴스가 계속 존재합니다. 또한 비정상 상태에서 클러스터를 삭제한 경우, 제거를 선택했어도 스토리지가 아직 존재할 수 있습니다. 지시사항, 특히 IBM Cloud 인프라(SoftLayer)에서 [스토리지 인스턴스를 삭제](#sl_delete_storage)하는 단계를 따르십시오.

**PVC를 삭제하여 내 모든 스토리지를 제거할 수 있습니까?**</br>
종종 가능합니다. [지속적 스토리지를 동적으로 작성](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)하며 해당 이름에 `retain`이 없는 스토리지 클래스를 선택하는 경우, PVC를 삭제하면 PV 및 IBM Cloud 인프라(SoftLayer) 스토리지 인스턴스도 삭제됩니다.

기타 모든 경우에는 지시사항에 따라 실제 스토리지 디바이스 및 PVC, PV의 상태를 확인한 후에 필요하면 이를 별도로 삭제하십시오.

**삭제 후에 스토리지에 대해 여전히 비용을 지불합니까?**</br>
청구 유형 및 삭제 대상에 따라 다릅니다. PVC 및 PV를 삭제하지만 IBM Cloud 인프라(SoftLayer) 계정의 인스턴스는 삭제하지 않은 경우, 해당 인스턴스는 계속 존재하게 되며 이에 대한 비용을 지불하게 됩니다. 비용 청구를 막으려면 이들을 모두 삭제해야 합니다. 또한 PVC에서 `billingType`을 지정하는 경우에는 `hourly` 또는 `monthly`를 선택할 수 있습니다. `monthly`를 선택하면 인스턴스의 비용이 매월 청구됩니다. 인스턴스를 삭제하면 해당 월의 나머지에 대해 비용이 청구됩니다.


<p class="important">지속적 스토리지를 정리하면 그 안에 저장된 모든 데이터가 삭제됩니다. 데이터의 사본이 필요한 경우에는 [파일 스토리지](/docs/containers?topic=containers-file_storage#file_backup_restore) 또는 [블록 스토리지](/docs/containers?topic=containers-block_storage#block_backup_restore)에 대한 백업을 작성하십시오.</p>

시작하기 전에: [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

지속적 데이터를 정리하려면 다음을 수행하십시오.

1.  클러스터의 PVC를 나열하고 PVC의 **`NAME`**, **`STORAGECLASS`**, 그리고 PVC에 바인드되고 **`VOLUME`**으로 표시되는 PV의 이름을 기록해 두십시오.
    ```
    kubectl get pvc
    ```
    {: pre}

    출력 예:
    ```
    NAME                  STATUS    VOLUME                                     CAPACITY   ACCESSMODES   STORAGECLASS            AGE
    claim1-block-bronze   Bound     pvc-06886b77-102b-11e8-968a-f6612bb731fb   20Gi       RWO           ibmc-block-bronze       78d
    claim-file-bronze     Bound     pvc-457a2b96-fafc-11e7-8ff9-b6c8f770356c   4Gi        RWX           ibmc-file-bronze-retain 105d
    claim-file-silve      Bound     pvc-1efef0ba-0c48-11e8-968a-f6612bb731fb   24Gi       RWX           ibmc-file-silver        83d
    ```
    {: screen}

2. 스토리지 클래스에 대한 **`ReclaimPolicy`** 및 **`billingType`**을 검토하십시오.
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   재확보 정책에서 `Delete`를 표시하는 경우에는 PVC를 제거할 때 PV 및 실제 스토리지가 제거됩니다. 재확보 정책에서 `Retain`을 표시하거나 사용자가 스토리지 클래스 없이 스토리지를 프로비저닝한 경우에는 PVC를 제거할 때 PV 및 실제 스토리지가 제거되지 않습니다. 사용자가 PVC, PV 및 실제 스토리지를 개별적으로 제거해야 합니다.

   스토리지가 매월 비용 청구되는 경우에는 비용 청구 주기가 종료되기 전에 스토리지를 제거해도 여전히 해당 월 전체에 대해 비용이 청구됩니다.
   {: important}

3. PVC를 마운트하는 팟(Pod)을 제거하십시오.
   1. PVC를 마운트하는 팟(Pod)을 나열하십시오.
      ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
      {: pre}

      출력 예:
      ```
      blockdepl-12345-prz7b:	claim1-block-bronze  
      ```
      {: screen}

      팟(Pod)이 CLI 출력에서 리턴되지 않으면 PVC를 사용하는 팟(Pod)이 없는 것입니다.

   2. PVC를 사용하는 팟(Pod)을 제거하십시오. 팟(Pod)이 배치의 일부인 경우에는 배치를 제거하십시오.
      ```
      kubectl delete pod <pod_name>
      ```
      {: pre}

   3. 팟(Pod)이 제거되었는지 확인하십시오.
      ```
      kubectl get pods
      ```
      {: pre}

4. PVC를 제거하십시오.
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}

5. PV의 상태를 검토하십시오. 이전에 검색한 PV의 이름을 **`VOLUME`**으로 사용하십시오.
   ```
   kubectl get pv <pv_name>
   ```
   {: pre}

   PVC를 제거하면 PVC에 바인딩된 PV가 릴리스됩니다. 스토리지를 프로비저닝하는 방법에 따라, PV는 `Deleting` 상태(PV가 자동 삭제되는 경우) 또는 `Released` 상태(PV를 수동 삭제해야 하는 경우)가 됩니다. **참고**: 자동 삭제되는 PV의 경우에는 삭제되기 전에 상태가 잠시 `Released`로 표시될 수 있습니다. 잠시 후에 명령을 다시 실행하면 PV가 제거되었는지 여부를 볼 수 있습니다.

6. PV가 삭제되지 않은 경우에는 PV를 수동으로 제거하십시오.
   ```
   kubectl delete pv <pv_name>
   ```
   {: pre}

7. PV가 제거되었는지 확인하십시오.
   ```
   kubectl get pv
   ```
   {: pre}

8. {: #sl_delete_storage}PV가 지시하는 실제 스토리지 인스턴스를 나열하고 실제 스토리지 인스턴스의 **`id`**를 기록해 두십시오.

   **파일 스토리지: **
   ```
   ibmcloud sl file volume-list --columns id  --columns notes | grep <pv_name>
   ```
   {: pre}
   **블록 스토리지: **
   ```
   ibmcloud sl block volume-list --columns id --columns notes | grep <pv_name>
   ```
   {: pre}

   클러스터를 제거했으며 PV의 이름을 검색할 수 없는 경우에는 `grep <pv_name>`을 `grep <cluster_id>`으로 대체하여 클러스터와 연관된 모든 스토리지 디바이스를 나열하십시오.
   {: tip}

   출력 예:
   ```
   id         notes   
   12345678   {"plugin":"ibm-file-plugin-5b55b7b77b-55bb7","region":"us-south","cluster":"aa1a11a1a11b2b2bb22b22222c3c3333","type":"Endurance","ns":"default","pvc":"mypvc","pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7","storageclass":"ibmc-file-gold"}
   ```
   {: screen}

   **참고** 필드 정보 이해:
   *  **`"plugin":"ibm-file-plugin-5b55b7b77b-55bb7"`**: 클러스터가 사용하는 스토리지 플러그인입니다.
   *  **`"region":"us-south"`**: 클러스터가 있는 지역입니다.
   *  **`"cluster":"aa1a11a1a11b2b2bb22b22222c3c3333"`**: 스토리지 인스턴스와 연관된 클러스터 ID입니다.
   *  **`"type":"Endurance"`**: 파일 또는 블록 스토리지의 유형(`Endurance` 또는 `Performance`)입니다.
   *  **`"ns":"default"`**: 스토리지 인스턴스가 배치되는 네임스페이스입니다.
   *  **`"pvc":"mypvc"`**: 스토리지 인스턴스와 연관된 PVC의 이름입니다.
   *  **`"pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7"`**: 스토리지 인스턴스와 연관된 PV입니다.
   *  **`"storageclass":"ibmc-file-gold"`**: 스토리지 클래스의 유형은 브론즈, 실버, 골드 또는 사용자 정의입니다.

9. 실제 스토리지 인스턴스를 제거하십시오.

   **파일 스토리지: **
   ```
   ibmcloud sl file volume-cancel <filestorage_id>
   ```
   {: pre}

   **블록 스토리지: **
   ```
   ibmcloud sl block volume-cancel <blockstorage_id>
   ```
   {: pre}

9. 실제 스토리지 인스턴스가 제거되었는지 확인하십시오. 삭제 프로세스는 완료되는 데 수 일이 걸릴 수 있습니다.

   **파일 스토리지: **
   ```
   ibmcloud sl file volume-list
   ```
   {: pre}
   **블록 스토리지: **
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}
