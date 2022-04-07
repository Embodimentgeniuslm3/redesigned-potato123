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



# クラスターからの永続ストレージの削除
{: #cleanup}

クラスターの永続ストレージをセットアップするときには、ストレージを要求する Kubernetes 永続ボリューム請求 (PVC)、ポッドにマウントされる、PVC で指定された Kubernetes 永続ボリューム (PV)、NFS ファイルやブロック・ストレージなどの IBM Cloud インフラストラクチャー (SoftLayer) インスタンスという 3 つの主なコンポーネントを作成します。 ストレージの作成方法によっては、3 つすべてを個別に削除しなければならない場合があります。
{:shortdesc}

## 永続ストレージのクリーンアップ
{: #storage_remove}

削除オプションについて

**クラスターを削除しました。 永続ストレージを削除するために他にも削除しなければならないものはありますか?**</br>
状況によります。 クラスターを削除すると、PVC と PV は削除されます。 ただし、関連するストレージ・インスタンスを IBM Cloud インフラストラクチャー (SoftLayer) で削除するかどうかは選択できます。 削除しないことを選択した場合、ストレージ・インスタンスは残ります。 また、クラスターを正常でない状態で削除した場合は、ストレージを削除することを選択していても、ストレージがまだ存在している可能性があります。 IBM Cloud インフラストラクチャー (SoftLayer) の[ストレージ・インスタンスを削除](#sl_delete_storage)する手順に従ってください。

**PVC を削除することで自分のストレージをすべて削除することはできますか?**</br>
場合によります。 [永続ストレージを動的に作成](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning)し、名前に `retain` が付いていないストレージ・クラスを選択した場合は、PVC を削除すると、PV および IBM Cloud インフラストラクチャー (SoftLayer) ストレージ・インスタンスも削除されます。

それ以外の場合はすべて、説明に従って PVC、PV、物理ストレージ・デバイスの状況を確認し、必要に応じて個々に削除してください。

**削除した後もストレージに対する料金を課金されますか?**</br>
何を削除したかと、課金タイプによって異なります。 PVC と PV を削除しても、IBM Cloud インフラストラクチャー (SoftLayer) アカウントのインスタンスを削除しなかった場合は、そのインスタンスがまだ存在するので課金されます。 課金されないようにするには、すべて削除する必要があります。 また、PVC で `billingType` を指定するときには、`hourly` か `monthly` を選択できます。 `monthly` を選択した場合、インスタンスは月単位で課金されます。 インスタンスを削除しても、その月の最後まで請求されます。


<p class="important">永続ストレージをクリーンアップすると、保管されているすべてのデータが削除されます。 データのコピーが必要な場合は、[ファイル・ストレージ](/docs/containers?topic=containers-file_storage#file_backup_restore)または[ブロック・ストレージ](/docs/containers?topic=containers-block_storage#block_backup_restore)のバックアップを作成してください。</p>

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

永続データをクリーンアップするには、次の手順を実行します。

1.  クラスターの PVC をリストし、PVC の **`NAME`**、**`STORAGECLASS`**、および PVC にバインドされていて **`VOLUME`** として表示されている PV の名前をメモします。
    ```
    kubectl get pvc
    ```
    {: pre}

    出力例:
    ```
    NAME                  STATUS    VOLUME                                     CAPACITY   ACCESSMODES   STORAGECLASS            AGE
    claim1-block-bronze   Bound     pvc-06886b77-102b-11e8-968a-f6612bb731fb   20Gi       RWO           ibmc-block-bronze       78d
    claim-file-bronze     Bound     pvc-457a2b96-fafc-11e7-8ff9-b6c8f770356c   4Gi        RWX           ibmc-file-bronze-retain 105d
    claim-file-silve      Bound     pvc-1efef0ba-0c48-11e8-968a-f6612bb731fb   24Gi       RWX           ibmc-file-silver        83d
    ```
    {: screen}

2. ストレージ・クラスの **`ReclaimPolicy`** と **`billingType`** を確認します。
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   回収ポリシーが `Delete` の場合は、PVC を削除すると、PV と物理ストレージも削除されます。 回収ポリシーが `Retain` の場合、またはストレージ・クラスを指定せずにストレージをプロビジョンした場合は、PVC を削除しても、PV と物理ストレージは削除されません。 PVC、PV、および物理ストレージを個別に削除する必要があります。

   ストレージの課金が月単位の場合は、課金サイクルの終了前にストレージを削除しても、その月の月額料金が課金されます。
   {: important}

3. PVC をマウントするすべてのポッドを削除します。
   1. PVC をマウントするポッドをリストします。
      ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
      {: pre}

      出力例:
      ```
      blockdepl-12345-prz7b:	claim1-block-bronze  
      ```
      {: screen}

      CLI 出力にポッドが返されない場合は、PVC を使用するポッドはありません。

   2. PVC を使用するポッドを削除します。 ポッドがデプロイメントの一部である場合は、デプロイメントを削除します。
      ```
      kubectl delete pod <pod_name>
      ```
      {: pre}

   3. ポッドが削除されたことを確認します。
      ```
      kubectl get pods
      ```
      {: pre}

4. PVC を削除します。
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}

5. PV の状況を確認します。 先ほど取得した PV の名前を、**`VOLUME`** として使用します。
   ```
   kubectl get pv <pv_name>
   ```
   {: pre}

   PVC を削除すると、PVC にバインドされている PV はリリースされます。 PV の状態は、ストレージをプロビジョンした方法に応じて、PV が自動的に削除された場合は `Deleting` 状態になり、PV を手動で削除する必要がある場合は `Released` 状態になります。 **注**: 自動的に削除される PV の場合、削除が終わる前に一時的に状況が `Released` と表示されることがあります。 数分後にコマンドを再実行し、PV が削除されたことを確認してください。

6. PV が削除されていない場合は、手動で PV を削除します。
   ```
   kubectl delete pv <pv_name>
   ```
   {: pre}

7. PV が削除されたことを確認します。
   ```
   kubectl get pv
   ```
   {: pre}

8. {: #sl_delete_storage}PV が指していた物理ストレージ・インスタンスをリストし、物理ストレージ・インスタンスの **`id`** をメモします。

   **ファイル・ストレージ:**
   ```
   ibmcloud sl file volume-list --columns id  --columns notes | grep <pv_name>
   ```
   {: pre}
   **ブロック・ストレージ:**
   ```
   ibmcloud sl block volume-list --columns id --columns notes | grep <pv_name>
   ```
   {: pre}

   クラスターを削除していて、PV の名前を取得できない場合は、`grep <pv_name>` を `grep <cluster_id>` に置き換えて、クラスターに関連付けられたすべてのストレージ・デバイスをリストしてください。
   {: tip}

   出力例:
   ```
   id         notes   
   12345678   {"plugin":"ibm-file-plugin-5b55b7b77b-55bb7","region":"us-south","cluster":"aa1a11a1a11b2b2bb22b22222c3c3333","type":"Endurance","ns":"default","pvc":"mypvc","pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7","storageclass":"ibmc-file-gold"}
   ```
   {: screen}

   **Notes** フィールドの情報について
   *  **`"plugin":"ibm-file-plugin-5b55b7b77b-55bb7"`**: クラスターが使用するストレージ・プラグイン。
   *  **`"region":"us-south"`**: クラスターが存在している地域。
   *  **`"cluster":"aa1a11a1a11b2b2bb22b22222c3c3333"`**: ストレージ・インスタンスに関連付けられているクラスター ID。
   *  **`"type":"Endurance"`**: ファイル・ストレージまたはブロック・ストレージのタイプ (`Endurance` または `Performance`)。
   *  **`"ns":"default"`**: ストレージ・インスタンスのデプロイ先の名前空間。
   *  **`"pvc":"mypvc"`**: ストレージ・インスタンスに関連付けられている PVC の名前。
   *  **`"pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7"`**: ストレージ・インスタンスに関連付けられている PV。
   *  **`"storageclass":"ibmc-file-gold"`**: ストレージ・クラスのタイプ (ブロンズ、シルバー、ゴールド、またはカスタム)。

9. 物理ストレージ・インスタンスを削除します。

   **ファイル・ストレージ:**
   ```
   ibmcloud sl file volume-cancel <filestorage_id>
   ```
   {: pre}

   **ブロック・ストレージ:**
   ```
   ibmcloud sl block volume-cancel <blockstorage_id>
   ```
   {: pre}

9. 物理ストレージ・インスタンスが削除されたことを確認します。 削除プロセスが完了するまで最大で数日かかることがあります。

   **ファイル・ストレージ:**
   ```
   ibmcloud sl file volume-list
   ```
   {: pre}
   **ブロック・ストレージ:**
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}
