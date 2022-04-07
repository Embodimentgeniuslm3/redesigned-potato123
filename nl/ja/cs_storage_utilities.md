---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, local persistent storage

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



# IBM Cloud ストレージ・ユーティリティー
{: #utilities}

## IBM Cloud Block Storage Attacher プラグイン (ベータ) のインストール
{: #block_storage_attacher}

{{site.data.keyword.Bluemix_notm}} Block Storage Attacher プラグインを使用して、未フォーマットおよび未マウントのロー・ブロック・ストレージをクラスター内のワーカー・ノードに接続します。  
{: shortdesc}

例えば、[Portworx](/docs/containers?topic=containers-portworx) などのソフトウェア定義ストレージ・ソリューション (SDS) を使用してデータを保管することを希望しているが、SDS の使用向けに最適化されている、追加のローカル・ディスクを備えたベア・メタル・ワーカー・ノードの使用を希望していない場合があります。 非 SDS ワーカー・ノードにローカル・ディスクを追加するには、{{site.data.keyword.Bluemix_notm}} インフラストラクチャー・アカウント内でブロック・ストレージ・デバイスを手動で作成して、{{site.data.keyword.Bluemix_notm}} Block Volume Attacher を使用してそのストレージを非 SDS ワーカー・ノードに接続する必要があります。

{{site.data.keyword.Bluemix_notm}} Block Volume Attacher プラグインは、デーモン・セットの一部としてクラスター内のすべてのワーカー・ノード上にポッドを作成して、後でブロック・ストレージ・デバイスを非 SDS ワーカー・ノードに接続するために使用する Kubernetes ストレージ・クラスをセットアップします。

{{site.data.keyword.Bluemix_notm}} Block Volume Attacher プラグインの更新方法と削除方法については、 [IBM Cloud Object Storage プラグインの更新](#update_block_attacher)と [IBM Cloud Object Storage プラグインの削除](#remove_block_attacher)を参照してください。
{: tip}

1.  [こちらの手順に従って](/docs/containers?topic=containers-helm#public_helm_install)、Helm クライアントをローカル・マシンにインストールして、サービス・アカウントを使用して Helm サーバー (tiller) をクラスター内にインストールします。

2.  tiller がサービス・アカウントでインストールされていることを確認します。

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    出力例:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. Helm リポジトリーを更新して、このリポジトリーにあるすべての Helm チャートの最新バージョンを取得します。
   ```
   helm repo update
   ```
   {: pre}

4. {{site.data.keyword.Bluemix_notm}} Block Volume Attacher プラグインをインストールします。 このプラグインをインストールすると、事前定義されたブロック・ストレージ・クラスがクラスターに追加されます。
   ```
   helm install iks-charts/ibm-block-storage-attacher --name block-attacher
   ```
   {: pre}

   出力例:
   ```
   NAME:   block-volume-attacher
   LAST DEPLOYED: Thu Sep 13 22:48:18 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/ClusterRoleBinding
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   ==> v1beta1/DaemonSet
   NAME                             DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-attacher  0        0        0      0           0          <none>         1s

   ==> v1/StorageClass
   NAME                 PROVISIONER                AGE
   ibmc-block-attacher  ibm.io/ibmc-blockattacher  1s

   ==> v1/ServiceAccount
   NAME                             SECRETS  AGE
   ibmcloud-block-storage-attacher  1        1s

   ==> v1beta1/ClusterRole
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-attacher.   Your release is named: block-volume-attacher

   Please refer Chart README.md file for attaching a block storage
   Please refer Chart RELEASE.md to see the release details/fixes
   ```
   {: screen}

5. {{site.data.keyword.Bluemix_notm}} Block Volume Attacher デーモン・セットが正常にインストールされていることを確認します。
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   出力例:
   ```
   ibmcloud-block-storage-attacher-z7cv6           1/1       Running            0          19m
   ```
   {: screen}

   正常にインストールされている場合は、1 つ以上の **ibmcloud-block-storage-attacher** ポッドが表示されます。 ポッドの数は、クラスター内のワーカー・ノードの数と等しくなります。 すべてのポッドが **Running** 状態である必要があります。

6. {{site.data.keyword.Bluemix_notm}} Block Volume Attacher のストレージ・クラスが正常に作成されていることを確認します。
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   出力例:
   ```
   ibmc-block-attacher       ibm.io/ibmc-blockattacher   11m
   ```
   {: screen}

### IBM Cloud Block Storage Attacher プラグインの更新
{: #update_block_attacher}

既存の {{site.data.keyword.Bluemix_notm}} Block Storage Attacher プラグインを最新バージョンにアップグレードできます。
{: shortdesc}

1. Helm リポジトリーを更新して、このリポジトリーにあるすべての Helm チャートの最新バージョンを取得します。
   ```
   helm repo update
   ```
   {: pre}

2. オプション: 最新の Helm チャートをローカル・マシンにダウンロードします。 そして、パッケージを解凍し、`release.md` ファイルを参照して最新リリース情報を確認します。
   ```
   helm fetch iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

3. {{site.data.keyword.Bluemix_notm}} Block Storage Attacher プラグインの Helm チャートの名前を確認します。
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   出力例:
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

4. {{site.data.keyword.Bluemix_notm}} Block Storage Attacher を最新バージョンにアップグレードします。
   ```
   helm upgrade --force --recreate-pods <helm_chart_name> ibm-block-storage-attacher
   ```
   {: pre}

### IBM Cloud Block Volume Attacher プラグインの削除
{: #remove_block_attacher}

{{site.data.keyword.Bluemix_notm}} Block Storage Attacher プラグインをクラスター内でプロビジョンして使用することを希望しない場合は、Helm チャートをアンインストールできます。
{: shortdesc}

1. {{site.data.keyword.Bluemix_notm}} Block Storage Attacher プラグインの Helm チャートの名前を確認します。
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   出力例:
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

2. Helm チャートを削除することで、{{site.data.keyword.Bluemix_notm}} Block Storage Attacher プラグインを削除します。
   ```
   helm delete <helm_chart_name> --purge
   ```
   {: pre}

3. {{site.data.keyword.Bluemix_notm}} Block Storage Attacher プラグインのポッドが削除されたことを確認します。
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   CLI 出力にポッドが表示されていなければ、ポッドは正常に削除されています。

4. {{site.data.keyword.Bluemix_notm}} Block Storage Attacher のストレージ・クラスが削除されたことを確認します。
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   ストレージ・クラスが正常に削除された場合は、CLI 出力にストレージ・クラスが表示されません。

## フォーマットされていないブロック・ストレージの自動プロビジョニングと、ワーカー・ノードからストレージへのアクセスの許可
{: #automatic_block}

{{site.data.keyword.Bluemix_notm}} Block Volume Attacher プラグインを使用して、同じ構成の未フォーマットおよび未マウントのロー・ブロック・ストレージをクラスター内のすべてのワーカー・ノードに追加できます。
{: shortdesc}

{{site.data.keyword.Bluemix_notm}} Block Volume Attacher プラグインに含まれている `mkpvyaml` コンテナーはスクリプトを実行するように構成されており、このスクリプトによって、クラスター内のすべてのワーカー・ノードが検出されて、{{site.data.keyword.Bluemix_notm}} インフラストラクチャー・ポータル内にロー・ブロック・ストレージが作成されて、これらのワーカー・ノードがこのストレージにアクセスすることが許可されます。

異なるブロック・ストレージ構成を追加するには、ワーカー・ノードのサブセットのみにブロック・ストレージを追加します。または、プロビジョニング・プロセスをより細かく制御するには、[ブロック・ストレージを手動で追加](#manual_block)します。
{: tip}


1. {{site.data.keyword.Bluemix_notm}} にログインして、ご使用のクラスターが含まれているリソース・グループをターゲットにします。
   ```
   ibmcloud login
   ```
   {: pre}

2.  {{site.data.keyword.Bluemix_notm}} ストレージ・ユーティリティーのリポジトリーを複製します。
    ```
    git clone https://github.com/IBM/ibmcloud-storage-utilities.git
    ```
    {: pre}

3. `block-storage-utilities` ディレクトリーに移動します。
   ```
   cd ibmcloud-storage-utilities/block-storage-provisioner
   ```
   {: pre}

4. `yamlgen.yaml` ファイルを開いて、クラスター内のすべてのワーカー・ノードに追加するブロック・ストレージ構成を指定します。
   ```
   #
   # Can only specify 'performance' OR 'endurance' and associated clause
   #
   cluster:  <cluster_name>    #  Enter the name of your cluster
   region:   <region>          #  Enter the IBM Cloud Kubernetes Service region where you created your cluster
   type:  <storage_type>       #  Enter the type of storage that you want to provision. Choose between "performance" or "endurance"
   offering: storage_as_a_service
   # performance:
   #    - iops:  <iops>
   endurance:
     - tier:  2                #   [0.25|2|4|10]
   size:  [ 20 ]               #   Enter the size of your storage in gigabytes
   ```
   {: codeblock}

   <table>
   <caption>YAML ファイルの構成要素について</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
   </thead>
   <tbody>
   <tr>
   <td><code>クラスター</code></td>
   <td>ロー・ブロック・ストレージを追加するクラスターの名前を入力します。 </td>
   </tr>
   <tr>
   <td><code>地域 (region)</code></td>
   <td>クラスターを作成した {{site.data.keyword.containerlong_notm}} の地域を入力します。 <code>[bxcs] cluster-get --cluster &lt;cluster_name_or_ID&gt;</code> を実行して、クラスターの地域を確認します。  </td>
   </tr>
   <tr>
   <td><code>type</code></td>
   <td>プロビジョンするストレージのタイプを入力します。 <code>performance</code> または <code>endurance</code> のいずれかを選択します。 詳しくは、[ブロック・ストレージ構成の決定](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)を参照してください。  </td>
   </tr>
   <tr>
   <td><code>performance.iops</code></td>
   <td>`performance` ストレージをプロビジョンする場合は、IOPS の数を入力します。 詳しくは、[ブロック・ストレージ構成の決定](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)を参照してください。 `endurance` ストレージをプロビジョンする場合は、このセクションを削除するか、各行頭に `#` を追加してこのセクションをコメント化します。
   </tr>
   <tr>
   <td><code>endurance.tier</code></td>
   <td>`endurance` ストレージをプロビジョンする場合は、ギガバイトあたりの IOPS の数を入力します。 例えば、`ibmc-block-bronze` ストレージ・クラス内の定義に従ってブロック・ストレージをプロビジョンする場合は、2 を入力します。詳しくは、[ブロック・ストレージ構成の決定](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)を参照してください。 `performance` ストレージをプロビジョンする場合は、このセクションを削除するか、各行頭に `#` を追加してこのセクションをコメント化します。 </td>
   </tr>
   <tr>
   <td><code>size</code></td>
   <td>ストレージのサイズをギガバイト単位で入力します。 サポートされているストレージ層サイズについては、[ブロック・ストレージ構成の決定](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)を参照してください。 </td>
   </tr>
   </tbody>
   </table>  

5. IBM Cloud インフラストラクチャー (SoftLayer) のユーザー名と API キーを取得します。 このユーザー名と API キーは、`mkpvyaml` スクリプトによってクラスターにアクセスするために使用されます。
   1. [{{site.data.keyword.Bluemix_notm}} コンソール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/) にログインします。
   2. メニュー ![メニュー・アイコン](../icons/icon_hamburger.svg "メニュー・アイコン") から、**「インフラストラクチャー」**を選択します。
   3. メニュー・バーから、**「アカウント」** > **「ユーザー」** > **「ユーザー・リスト」**を選択します。
   4. 取得するユーザー名と API キーを持つユーザーを確認します。
   5. **「生成」**をクリックして API キーを生成するか、**「表示」**をクリックして既存の API キーを表示します。 ポップアップ・ウィンドウが開いて、インフラストラクチャーのユーザー名と API キーが表示されます。

6. 資格情報を環境変数に保管します。
   1. 環境変数を追加します。
      ```
      export SL_USERNAME=<infrastructure_username>
      ```
      {: pre}

      ```
      export SL_API_KEY=<infrastructure_api_key>
      ```
      {: pre}

   2. 環境変数が正常に作成されたことを確認します。
      ```
      printenv
      ```
      {: pre}

7.  `mkpvyaml` コンテナーをビルドして実行します。 このコンテナーをイメージから実行すると、`mkpvyaml.py` スクリプトが実行されます。 このスクリプトによって、クラスター内のすべてのワーカー・ノードにブロック・ストレージ・デバイスが追加されて、各ワーカー・ノードがこのブロック・ストレージ・デバイスにアクセスすることが許可されます。 このスクリプトの終了時に生成される `pv-<cluster_name>.yaml` という YAML ファイルは、後でクラスター内に永続ボリュームを作成するために使用できます。
    1.  `mkpvyaml` コンテナーをビルドします。
        ```
        docker build -t mkpvyaml .
        ```
        {: pre}
        出力例:
        ```
        Sending build context to Docker daemon   29.7kB
        Step 1/16 : FROM ubuntu:18.10
        18.10: Pulling from library/ubuntu
        5940862bcfcd: Pull complete
        a496d03c4a24: Pull complete
        5d5e0ccd5d0c: Pull complete
        ba24b170ddf1: Pull complete
        Digest: sha256:20b5d52b03712e2ba8819eb53be07612c67bb87560f121cc195af27208da10e0
        Status: Downloaded newer image for ubuntu:18.10
         ---> 0bfd76efee03
        Step 2/16 : RUN apt-get update
         ---> Running in 85cedae315ce
        Get:1 http://security.ubuntu.com/ubuntu cosmic-security InRelease [83.2 kB]
        Get:2 http://archive.ubuntu.com/ubuntu cosmic InRelease [242 kB]
        ...
        Step 16/16 : ENTRYPOINT [ "/docker-entrypoint.sh" ]
         ---> Running in 9a6842f3dbe3
        Removing intermediate container 9a6842f3dbe3
         ---> 7926f5384fc7
        Successfully built 7926f5384fc7
        Successfully tagged mkpvyaml:latest
        ```
        {: screen}
    2.  このコンテナーを実行して、`mkpvyaml.py` スクリプトを実行します。
        ```
        docker run --rm -v `pwd`:/data -v ~/.bluemix:/config -e SL_API_KEY=$SL_API_KEY -e SL_USERNAME=$SL_USERNAME mkpvyaml
        ```
        {: pre}

        出力例:
        ```
        Unable to find image 'portworx/iks-mkpvyaml:latest' locally
        latest: Pulling from portworx/iks-mkpvyaml
        72f45ff89b78: Already exists
        9f034a33b165: Already exists
        386fee7ab4d3: Already exists
        f941b4ac6aa8: Already exists
        fe93e194fcda: Pull complete
        f29a13da1c0a: Pull complete
        41d6e46c1515: Pull complete
        e89af7a21257: Pull complete
        b8a7a212d72e: Pull complete
        5e07391a6f39: Pull complete
        51539879626c: Pull complete
        cdbc4e813dcb: Pull complete
        6cc28f4142cf: Pull complete
        45bbaad87b7c: Pull complete
        05b0c8595749: Pull complete
        Digest: sha256:43ac58a8e951994e65f89ed997c173ede1fa26fb41e5e764edfd3fc12daf0120
        Status: Downloaded newer image for portworx/iks-mkpvyaml:latest

        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087291
        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087293
        kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1
                  ORDER ID =  30085655
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  Order ID =  30087291 has created VolId =  12345678
                  Granting access to volume: 12345678 for HostIP: 10.XXX.XX.XX
                  Order ID =  30087293 has created VolId =  87654321
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Vol 53002831 is not yet ready ...
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Order ID =  30085655 has created VolId =  98712345
                  Granting access to volume: 98712345 for HostIP: 10.XXX.XX.XX
        Output file created as :  pv-<cluster_name>.yaml
        ```
        {: screen}

7. [ブロック・ストレージ・デバイスをワーカー・ノードに接続します](#attach_block)。

## 特定のワーカー・ノードへのブロック・ストレージの手動追加
{: #manual_block}

この方法を選択するのは、異なるブロック・ストレージ構成を追加する場合、ワーカー・ノードのサブセットのみにブロック・ストレージを追加する場合、またはプロビジョニング・プロセスをより細かく制御する場合です。
{: shortdesc}

1. クラスター内のワーカー・ノードをリスト表示して、ブロック・ストレージ・デバイスを追加する非 SDS ワーカー・ノードのプライベート IP アドレスとゾーンを書き留めます。
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

2. [ブロック・ストレージ構成の決定](/docs/containers?topic=containers-block_storage#block_predefined_storageclass)のステップ 3 と 4 を参照して、非 SDS ワーカー・ノードに追加するブロック・ストレージ・デバイスのタイプ、サイズ、および IOPS 数を選択します。    

3. 非 SDS ワーカー・ノードと同じゾーン内にブロック・ストレージ・デバイスを作成します。

   **GB あたり 2 IOPS の 20 GB エンデュランス・ブロック・ストレージのプロビジョニング例:**
   ```
   ibmcloud sl block volume-order --storage-type endurance --size 20 --tier 2 --os-type LINUX --datacenter dal10
   ```
   {: pre}

   **100 IOPS の 20 GB パフォーマンス・ブロック・ストレージのプロビジョニング例:**
   ```
   ibmcloud sl block volume-order --storage-type performance --size 20 --iops 100 --os-type LINUX --datacenter dal10
   ```
   {: pre}

4. ブロック・ストレージ・デバイスが作成されたことを確認して、ボリュームの **`id`** を書き留めます。 **注:** ブロック・ストレージ・デバイスがすぐに表示されない場合は、数分間お待ちください。 その後、このコマンドを再実行してください。
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}

   出力例:
   ```
   id         username          datacenter   storage_type                capacity_gb   bytes_used   ip_addr         lunId   active_transactions
   123456789  IBM02SL1234567-8  dal10        performance_block_storage   20            -            161.12.34.123   0       0   
   ```
   {: screen}

5. ボリュームの詳細を確認して、**`Target IP`** と **`LUN Id`** を書き留めます。
   ```
   ibmcloud sl block volume-detail <volume_ID>
   ```
   {: pre}

   出力例:
   ```
   Name                       Value
   ID                         1234567890
   User name                  IBM123A4567890-1
   Type                       performance_block_storage
   Capacity (GB)              20
   LUN Id                     0
   IOPs                       100
   Datacenter                 dal10
   Target IP                  161.12.34.123
   # of Active Transactions   0
   Replicant Count            0
   ```
   {: screen}

6. 非 SDS ワーカー・ノードがブロック・ストレージ・デバイスにアクセスすることを許可します。 `<volume_ID>` を、既に取得したブロック・ストレージ・デバイスのボリューム ID に置き換えて、`<private_worker_IP>` を、このデバイスを接続する非 SDS ワーカー・ノードのプライベート IP アドレスに置き換えてください。

   ```
   ibmcloud sl block access-authorize <volume_ID> -p <private_worker_IP>
   ```
   {: pre}

   出力例:
   ```
   The IP address 123456789 was authorized to access <volume_ID>.
   ```
   {: screen}

7. 非 SDS ワーカー・ノードが正常にアクセスを許可されたことを確認して、**`host_iqn`**、**`username`**、および **`password`** を書き留めます。
   ```
   ibmcloud sl block access-list <volume_ID>
   ```
   {: pre}

   出力例:
   ```
   ID          name                 type   private_ip_address   source_subnet   host_iqn                                      username   password           allowed_host_id
   123456789   <private_worker_IP>  IP     <private_worker_IP>  -               iqn.2018-09.com.ibm:ibm02su1543159-i106288771   IBM02SU1543159-I106288771   R6lqLBj9al6e2lbp   1146581   
   ```
   {: screen}

   正常にアクセスが許可された場合は、**`host_iqn`**、**`username`**、および **`password`** が割り当てられています。

8. [ブロック・ストレージ・デバイスをワーカー・ノードに接続します](#attach_block)。


## 非 SDS ワーカー・ノードへのロー・ブロック・ストレージの接続
{: #attach_block}

ブロック・ストレージ・デバイスを非 SDS ワーカー・ノードに接続するには、{{site.data.keyword.Bluemix_notm}} Block Volume Attacher のストレージ・クラスとブロック・ストレージ・デバイスの詳細を使用して、永続ボリューム (PV) を作成する必要があります。
{: shortdesc}

**始める前に**:
- 未フォーマットおよび未マウントのロー・ブロック・ストレージを非 SDS ワーカー・ノードに対して[自動的に](#automatic_block)または[手動で](#manual_block)作成済みであることを確認します。
- [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**ロー・ブロック・ストレージを非 SDS ワーカー・ノードに接続するには、以下のようにします**。
1. PV の作成を準備します。  
   - **`mkpvyaml` コンテナーを使用した場合:**
     1. `pv-<cluster_name>.yaml` ファイルを開きます。
        ```
        nano pv-<cluster_name>.yaml
        ```
        {: pre}

     2. PV の構成を確認します。

   - **ブロック・ストレージを手動で追加した場合:**
     1. `pv.yaml` ファイルを作成します。 次のコマンドによって、`nano` エディターを使用してこのファイルが作成されます。
        ```
        nano pv.yaml
        ```
        {: pre}

     2. ブロック・ストレージ・デバイスの詳細を PV に追加します。
        ```
        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: <pv_name>
          annotations:
            ibm.io/iqn: "<IQN_hostname>"
            ibm.io/username: "<username>"
            ibm.io/password: "<password>"
            ibm.io/targetip: "<targetIP>"
            ibm.io/lunid: "<lunID>"
            ibm.io/nodeip: "<private_worker_IP>"
            ibm.io/volID: "<volume_ID>"
        spec:
          capacity:
            storage: <size>
          accessModes:
            - ReadWriteOnce
          hostPath:
              path: /
          storageClassName: ibmc-block-attacher
        ```
        {: codeblock}

        <table>
        <caption>YAML ファイルの構成要素について</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
        </thead>
        <tbody>
      	<tr>
          <td><code>metadata.name</code></td>
      	<td>PV の名前を入力します。</td>
      	</tr>
        <tr>
        <td><code>ibm.io/iqn</code></td>
        <td>前に取得した IQN ホスト名を入力します。 </td>
        </tr>
        <tr>
        <td><code>ibm.io/username</code></td>
        <td>既に取得した IBM Cloud インフラストラクチャー (SoftLayer) のユーザー名を入力します。 </td>
        </tr>
        <tr>
        <td><code>ibm.io/password</code></td>
        <td>既に取得した IBM Cloud インフラストラクチャー (SoftLayer) のパスワードを入力します。 </td>
        </tr>
        <tr>
        <td><code>ibm.io/targetip</code></td>
        <td>既に取得したターゲット IP を入力します。 </td>
        </tr>
        <tr>
        <td><code>ibm.io/lunid</code></td>
        <td>既に取得したブロック・ストレージ・デバイスの LUN ID を入力します。 </td>
        </tr>
        <tr>
        <td><code>ibm.io/nodeip</code></td>
        <td>ブロック・ストレージ・デバイスへのアクセスを既に許可した、ブロック・ストレージ・デバイスを接続するワーカー・ノードのプライベート IP アドレスを入力します。 </td>
        </tr>
        <tr>
          <td><code>ibm.io/volID</code></td>
        <td>既に取得したブロック・ストレージ・ボリュームの ID を入力します。 </td>
        </tr>
        <tr>
        <td><code>storage</code></td>
        <td>既に作成したブロック・ストレージ・デバイスのサイズを入力します。 例えば、ブロック・ストレージ・デバイスが 20 ギガバイトの場合は、<code>20Gi</code> と入力します。  </td>
        </tr>
        </tbody>
        </table>
2. ブロック・ストレージ・デバイスを非 SDS ワーカー・ノードに接続するための PV を作成します。
   - **`mkpvyaml` コンテナーを使用した場合:**
     ```
     kubectl apply -f pv-<cluster_name>.yaml
     ```
     {: pre}

   - **ブロック・ストレージを手動で追加した場合:**
     ```
     kubectl apply -f  block-volume-attacher/pv.yaml
     ```
     {: pre}

3. ブロック・ストレージがワーカー・ノードに正常に接続されたことを確認します。
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   出力例:
   ```
   Name:            kube-wdc07-cr398f790bc285496dbeb8e9137bc6409a-w1-pv1
   Labels:          <none>
   Annotations:     ibm.io/attachstatus=attached
                    ibm.io/dm=/dev/dm-1
                    ibm.io/iqn=iqn.2018-09.com.ibm:ibm02su1543159-i106288771
                    ibm.io/lunid=0
                    ibm.io/mpath=3600a09803830445455244c4a38754c66
                    ibm.io/nodeip=10.176.48.67
                    ibm.io/password=R6lqLBj9al6e2lbp
                    ibm.io/targetip=161.26.98.114
                    ibm.io/username=IBM02SU1543159-I106288771
                    kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","kind":"PersistentVolume","metadata":{"annotations":{"ibm.io/iqn":"iqn.2018-09.com.ibm:ibm02su1543159-i106288771","ibm.io/lunid":"0"...
   Finalizers:      []
   StorageClass:    ibmc-block-attacher
   Status:          Available
   Claim:
   Reclaim Policy:  Retain
   Access Modes:    RWO
   Capacity:        20Gi
   Node Affinity:   <none>
   Message:
   Source:
       Type:          HostPath (bare host directory volume)
       Path:          /
       HostPathType:
   Events:            <none>
   ```
   {: screen}

   ブロック・ストレージ・デバイスが正常に接続された場合は、**ibm.io/dm** がデバイス ID (`/dev/dm/1` など) に設定され、CLI 出力の **Annotations** セクションに **ibm.io/attachstatus=attached** と表示されます。

ボリュームを切り離す場合は、PV を削除します。 切り離されたボリュームは、特定のワーカー・ノードによるアクセスが引き続き許可されて、異なるボリュームを同じワーカー・ノードに接続するために {{site.data.keyword.Bluemix_notm}} Block Volume Attacher のストレージ・クラスを使用して新しい PV を作成したときに再接続されます。 元の切り離されたボリュームが再接続されることを回避するには、`ibmcloud sl block access-revoke` コマンドを使用して、ワーカー・ノードが切り離されたボリュームにアクセスすることを禁止してください。 ボリュームを切り離しても、そのボリュームは IBM Cloud インフラストラクチャー (SoftLayer) アカウントから削除されません。 ボリュームの課金をキャンセルするには、[該当ストレージを IBM Cloud インフラストラクチャー (SoftLayer) アカウントから手動で削除する必要があります](/docs/containers?topic=containers-cleanup)。
{: note}


