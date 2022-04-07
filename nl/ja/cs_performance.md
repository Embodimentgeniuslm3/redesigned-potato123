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


# パフォーマンスのチューニング
{: #kernel}

特定のパフォーマンス最適化要件がある場合は、{{site.data.keyword.containerlong}} の 一部のクラスター・コンポーネント用のデフォルト設定を変更できます。
{: shortdesc}

デフォルトの設定を変更する場合は、お客様自身の責任で行ってください。 変更された設定に対してテストを実行すること、および環境内の設定変更が原因で発生する可能性がある障害についてはお客様が責任を持ちます。
{: important}

## ワーカー・ノードのパフォーマンスの最適化
{: #worker}

特定のパフォーマンス最適化要件がある場合は、ワーカー・ノードで、Linux カーネルの `sysctl` パラメーターのデフォルト設定を変更できます。
{: shortdesc}

ワーカー・ノードは、カーネル・パフォーマンスが最適になるようにして自動的にプロビジョンされますが、カスタム [Kubernetes `DaemonSet` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) オブジェクトをクラスターに適用してデフォルト設定を変更することができます。 デーモン・セットは、既存のすべてのワーカー・ノードの設定を変更し、クラスター内でプロビジョンされている新規ワーカー・ノードに設定を適用します。 ポッドは影響を受けません。

この例の特権的な `initContainer` を実行するには、すべての名前空間に対して[**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)を持っている必要があります。 デプロイメントのコンテナーが初期化された後、特権は除去されます。
{: note}

1. 以下のデーモン・セットを `worker-node-kernel-settings.yaml` という名前のファイルに保存します。 `spec.template.spec.initContainers` セクションで、調整する `sysctl` パラメーターのフィールドと値を追加します。 このサンプルのデーモン・セットでは、環境に許可するデフォルトの最大接続数を `net.core.somaxconn` 設定を使用して変更し、一時ポート範囲を `net.ipv4.ip_local_port_range` 設定を使用して変更しています。
    ```
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: kernel-optimization
      namespace: kube-system
      labels:
        tier: management
        app: kernel-optimization
    spec:
      selector:
        matchLabels:
          name: kernel-optimization
      template:
        metadata:
          labels:
            name: kernel-optimization
        spec:
          hostNetwork: true
          hostPID: true
          hostIPC: true
          initContainers:
            - command:
                - sh
                - -c
                - sysctl -w net.ipv4.ip_local_port_range="1025 65535"; sysctl -w net.core.somaxconn=32768;
              image: alpine:3.6
              imagePullPolicy: IfNotPresent
              name: sysctl
              resources: {}
              securityContext:
                privileged: true
                capabilities:
                  add:
                    - NET_ADMIN
              volumeMounts:
                - name: modifysys
                  mountPath: /sys
          containers:
            - resources:
                requests:
                  cpu: 0.01
              image: alpine:3.6
              name: sleepforever
              command: ["/bin/sh", "-c"]
              args:
                - >
                  while true; do
                    sleep 100000;
                  done
          volumes:
            - name: modifysys
              hostPath:
                path: /sys
    ```
    {: codeblock}

2. ワーカー・ノードにデーモン・セットを適用します。 この変更はすぐに適用されます。
    ```
    kubectl apply -f worker-node-kernel-settings.yaml
    ```
    {: pre}

<br />

ワーカー・ノードの `sysctl` パラメーターを {{site.data.keyword.containerlong_notm}} によって設定されたデフォルト値に戻すには、以下のようにします。

1. デーモン・セットを削除します。 カスタム設定を適用した `initContainers` が削除されます。
    ```
    kubectl delete ds kernel-optimization
    ```
    {: pre}

2. [クラスター内のすべてのワーカー・ノードをリブートします](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot)。 ワーカー・ノードは、デフォルト値が適用された状態でオンラインに戻ります。

<br />


## ポッドのパフォーマンスの最適化
{: #pod}

特定のパフォーマンス・ワークロードの要件がある場合は、ポッドのネットワーク名前空間で、Linux カーネルの `sysctl` パラメーターのデフォルト設定を変更します。
{: shortdesc}

アプリ・ポッドのカーネル設定を最適化するには、各デプロイメントの `pod/ds/rs/deployment` YAML に [`initContainer ` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) パッチを挿入します。 パフォーマンスを最適化したいポッドのネットワーク名前空間にある各アプリ・デプロイメントに `initContainer` が追加されます。

開始する前に、この例の特権的な `initContainer` を実行するために、すべての名前空間に対して[**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス役割](/docs/containers?topic=containers-users#platform)を持っていることを確認してください。 デプロイメントのコンテナーが初期化された後、特権は除去されます。

1. 以下の `initContainer` パッチを `pod-patch.yaml` という名前のファイルに保存し、調整する `sysctl` パラメーターのフィールドと値を追加します。 このサンプルの `initContainer` は、環境に許可するデフォルトの最大接続数を `net.core.somaxconn` 設定を使用して変更し、 一時ポート範囲を `net.ipv4.ip_local_port_range` 設定を使用して変更します。
    ```
    spec:
      template:
        spec:
          initContainers:
          - command:
            - sh
            - -c
            - sysctl -e -w net.core.somaxconn=32768;  sysctl -e -w net.ipv4.ip_local_port_range="1025 65535";
            image: alpine:3.6
            imagePullPolicy: IfNotPresent
            name: sysctl
            resources: {}
            securityContext:
              privileged: true
    ```
    {: codeblock}

2. 各デプロイメントにパッチを適用します。
    ```
    kubectl patch deployment <deployment_name> --patch pod-patch.yaml
    ```
    {: pre}

3. カーネル設定の `net.core.somaxconn` 値を変更した場合、ほとんどのアプリが、更新された値を自動的に使用します。 ただし、一部のアプリでは、カーネル値に一致するように、アプリ・コードの対応する値を手動で変更する必要がある場合があります。 例えば、NGINX アプリが実行されているポッドのパフォーマンスを調整する場合、NGINX アプリ・コードの `backlog` フィールドの値を一致するように変更する必要があります。 詳しくは、[NGINX に関するブログ投稿 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.nginx.com/blog/tuning-nginx/) を参照してください。

<br />


## クラスター・メトリック・プロバイダー・リソースの調整
{: #metrics}

クラスターのメトリック・プロバイダー (Kubernetes 1.12 以降では `metrics-server`、それより前のバージョンでは `heapster`) 構成は、ワーカー・ノード 1 つあたりのポッド数が 30 以下のクラスター用に最適化されています。 ワーカー・ノード 1 つあたりのポッド数がこれより多いクラスターでは、ポッドのメトリック・プロバイダー `metrics-server` または `heapster` メイン・コンテナーが、「`OOMKilled`」などのエラー・メッセージを出して頻繁に再始動する可能性があります。

メトリック・プロバイダー・ポッドには、クラスター内のワーカー・ノードの数に応じて `metrics-server` または `heapster` メイン・コンテナーのリソース要求および制限をスケーリングする `nanny` コンテナーもあります。 メトリック・プロバイダーの構成マップを編集して、デフォルトのリソースを変更できます。

開始前に、以下のことを行います。 [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  クラスターのメトリック・プロバイダーの構成マップ YAML を開きます。
    *  `metrics-server` の場合:
       ```
       kubectl get configmap metrics-server-config -n kube-system -o yaml
       ```
       {: pre}
    *  `heapster` の場合:
       ```
       kubectl get configmap heapster-config -n kube-system -o yaml
       ```
       {: pre}
    出力例:
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
    kind: ConfigMap
    metadata:
      annotations:
        armada-service: cruiser-kube-addons
        version: --
      creationTimestamp: 2018-10-09T20:15:32Z
      labels:
        addonmanager.kubernetes.io/mode: EnsureExists
        kubernetes.io/cluster-service: "true"
      name: heapster-config
      namespace: kube-system
      resourceVersion: "526"
      selfLink: /api/v1/namespaces/kube-system/configmaps/heapster-config
      uid: 11a1aaaa-bb22-33c3-4444-5e55e555e555
    ```
    {: screen}

2.  `memoryPerNode` フィールドを `data.NannyConfiguration` セクションの構成マップに追加します。 `metrics-server` と `heapster` の両方のデフォルト値は、`4Mi` に設定されています。
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
        memoryPerNode: 5Mi
    kind: ConfigMap
    ...
    ```
    {: codeblock}

3.  変更を適用します。
    ```
    kubectl apply -f heapster-config.yaml
    ```
    {: pre}

4.  メトリック・プロバイダーのポッドをモニターし、まだコンテナーが「`OOMKilled`」エラー・メッセージで再始動されるかどうかを確認します。 再始動された場合は、この手順を繰り返して、ポッドが安定するまで `memoryPerNode` のサイズを増やします。

さらに設定を調整する場合は、 [Kubernetes アドオン・サイズ変更機能の構成に関する資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/autoscaler/tree/master/addon-resizer#addon-resizer-configuration) で詳細を確認してください。
{: tip}
