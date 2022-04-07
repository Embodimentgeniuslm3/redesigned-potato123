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


# クラスターの削除
{: #remove}

有料アカウントで作成したフリー・クラスターと標準クラスターが不要になったら、リソースが消費されないように手動で削除する必要があります。
{:shortdesc}

<p class="important">
永続ストレージ内のクラスターやデータのバックアップは作成されません。 クラスターを削除するときに、永続ストレージも削除することができます。 永続ストレージを削除する場合、`delete` ストレージ・クラスを使用してプロビジョンした永続ストレージは、IBM Cloud インフラストラクチャー (SoftLayer) で完全に削除されます。 `retain` ストレージ・クラスを使用してプロビジョンした永続ストレージを削除する場合、クラスター、PV、および PVC は削除されますが、IBM Cloud インフラストラクチャー (SoftLayer) アカウント内の永続ストレージ・インスタンスは残ります。</br>
</br>クラスターを削除するときには、クラスターの作成時に自動的にプロビジョニングされたサブネットと、`ibmcloud ks cluster-subnet-create` コマンドを使用して作成したサブネットもすべて削除します。 ただし、`ibmcloud ks cluster-subnet-add コマンド`を使用して既存のサブネットをクラスターに手動で追加した場合、これらのサブネットは IBM Cloud インフラストラクチャー (SoftLayer) アカウントから削除されず、他のクラスターで再利用できます。</p>

開始前に、以下のことを行います。
* クラスター ID をメモします。 クラスターで自動的に削除されない IBM Cloud インフラストラクチャー (SoftLayer) 関連リソースを調査および削除するために、クラスター ID が必要になる場合があります。
* 永続ストレージ内のデータを削除する場合は、[削除オプションについて理解します](/docs/containers?topic=containers-cleanup#cleanup)。
* [**管理者**の {{site.data.keyword.Bluemix_notm}} IAM プラットフォーム役割](/docs/containers?topic=containers-users#platform)があることを確認してください。

クラスターを削除するには、以下のようにします。

1. オプション: CLI から、クラスター内のすべてのデータのコピーをローカルの YAML ファイルに保存します。
  ```
  kubectl get all --all-namespaces -o yaml
  ```
  {: pre}

2. クラスターを削除します。
  - {{site.data.keyword.Bluemix_notm}} コンソールから
    1.  クラスターを選択して、**「その他のアクション...」**メニューから**「削除」**をクリックします。

  - {{site.data.keyword.Bluemix_notm}} CLI から
    1.  使用可能なクラスターをリストします。

        ```
        ibmcloud ks clusters
        ```
        {: pre}

    2.  クラスターを削除します。

        ```
        ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
        ```
        {: pre}

    3.  プロンプトに従って、コンテナー、ポッド、バインドされたサービス、永続ストレージ、およびシークレットを含むクラスター・リソースを削除するかどうかを選択します。
      - **永続ストレージ**: 永続ストレージでは、データの高可用性を確保できます。 [既存のファイル共有](/docs/containers?topic=containers-file_storage#existing_file)を使用して永続ボリューム請求を作成した場合は、クラスターを削除するときにファイル共有を削除できません。 このファイル共有は後で IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオから手動で削除する必要があります。

          毎月の課金サイクルの規定で、永続ボリューム請求を月の最終日に削除することはできません。 永続ボリューム請求を月の末日に削除した場合、削除は翌月初めまで保留状態になります。
          {: note}

次のステップ:
- 削除したクラスターの名前は、`ibmcloud ks clusters` コマンドを実行しても使用可能なクラスターのリストに表示されなくなったら、再利用できます。
- サブネットを残した場合は、[それらを新しいクラスターで再利用](/docs/containers?topic=containers-subnets#subnets_custom)することも、後で IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオから手動で削除することもできます。
- 永続ストレージを残した場合は、後で {{site.data.keyword.Bluemix_notm}} コンソールの IBM Cloud インフラストラクチャー (SoftLayer) ダッシュボードを使用して[ストレージを削除](/docs/containers?topic=containers-cleanup#cleanup)できます。
