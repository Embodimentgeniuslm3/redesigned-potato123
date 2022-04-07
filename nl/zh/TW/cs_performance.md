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


# 調整效能
{: #kernel}

如果您具有特定的效能最佳化需求，則可以在 {{site.data.keyword.containerlong}} 中變更部分叢集元件的預設值。
{: shortdesc}

如果您選擇變更預設值，則必須自行承擔風險。您負責針對任何變更的設定執行測試，以及負責處理環境中因變更的設定而造成的任何可能中斷情況。
{: important}

## 將工作者節點效能最佳化
{: #worker}

如果您具有特定的效能最佳化需求，則可以變更工作者節點上 Linux Kernel `sysctl` 參數的預設值。
{: shortdesc}

系統會以最佳化核心效能來自動佈建工作者節點，但您可以藉由將自訂 [Kubernetes `DaemonSet` ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) 物件套用至叢集來變更預設值。常駐程式集會變更所有現有工作者節點的設定，並將這些設定套用至叢集裡所佈建的任何新工作者節點。不影響任何 Pod。

您必須具有所有名稱空間的[**管理員** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)，才能執行範例特許 `initContainer`。在起始設定用於部署的容器之後，就會捨棄這些專用權。
{: note}

1. 將下列常駐程式集儲存在名為 `worker-node-kernel-settings.yaml` 的檔案中。在 `spec.template.spec.initContainers` 區段中，新增您要調整的 `sysctl` 參數的欄位和值。此範例常駐程式集會透過 `net.core.somaxconn` 設定來變更環境中允許的預設連線數上限，以及透過 `net.ipv4.ip_local_port_range` 設定來變更暫時埠範圍。
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

2. 將常駐程式集套用至工作者節點。會立即套用變更。
    ```
    kubectl apply -f worker-node-kernel-settings.yaml
    ```
    {: pre}

<br />

若要使工作者節點的 `sysctl` 參數回復至 {{site.data.keyword.containerlong_notm}} 所設定的預設值：

1. 刪除常駐程式集。即會移除套用自訂設定的 `initContainers`。
    ```
    kubectl delete ds kernel-optimization
    ```
    {: pre}

2. [將叢集裡的所有工作者節點重新開機](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot)。工作者節點已回到線上，並已套用預設值。

<br />


## 將 Pod 效能最佳化
{: #pod}

如果您具有特定的效能工作負載需求，則可以變更 Pod 網路名稱空間上 Linux Kernel `sysctl` 參數的預設值。
{: shortdesc}

若要將應用程式 Pod 的核心設定最佳化，您可以將 [`initContainer ` ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) 修補程式插入至每個部署的 `pod/ds/rs/deployment` YAML 中。`initContainer` 會新增至您要將效能最佳化的 Pod 網路名稱空間中的每個應用程式部署。

開始之前，請確定您具有所有名稱空間的[**管理員** {{site.data.keyword.Bluemix_notm}} IAM 服務角色](/docs/containers?topic=containers-users#platform)，才能執行範例特許 `initContainer`。在起始設定用於部署的容器之後，就會捨棄這些專用權。

1. 將下列 `initContainer` 修補程式儲存在名為 `pod-patch.yaml` 的檔案中，並新增您要調整的 `sysctl` 參數的欄位和值。此範例 `initContainer` 會透過 `net.core.somaxconn` 設定來變更環境中允許的預設連線數上限，以及透過 `net.ipv4.ip_local_port_range` 設定來變更暫時埠範圍。
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

2. 修補每一個部署。
    ```
    kubectl patch deployment <deployment_name> --patch pod-patch.yaml
    ```
    {: pre}

3. 如果您變更了核心設定中的 `net.core.somaxconn` 值，大部分應用程式都可以自動使用更新的值。然而，有些應用程式可能需要您手動變更應用程式碼中的對應值，以符合核心值。例如，如果您要調整 Pod 的效能，其中 NGINX 應用程式正在執行中，您必須變更 NGINX 應用程式碼中的 `backlog` 欄位的值使其相符。如需相關資訊，請參閱這個 [NGINX 部落格文章![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.nginx.com/blog/tuning-nginx/)。

<br />


## 調整叢集度量提供者資源
{: #metrics}

叢集的度量提供者（Kubernetes 1.12 以及更新版本中的 `metrics-server`，或舊版中的 `heapster`）配置會針對每個工作者節點具有 30 個或更少 Pod 的叢集進行最佳化。如果您的叢集的每個工作者節點具有更多的 Pod，則 Pod 的度量提供者 `metrics-server` 或 `heapster` 主要容器可能會頻繁地重新啟動，並出現 `OOMKilled` 這類錯誤訊息。

度量提供者 Pod 也有一個 `nanny` 容器，可以調整 `metrics-server` 或 `heapster` 主要容器的資源要求及限制，以回應叢集裡的工作者節點數目。您可以藉由編輯度量提供者的 ConfigMap 來變更預設資源。

開始之前：[登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  開啟叢集度量提供者 Configmap YAML。
    *  若為 `metrics-server`，請執行下列指令：
       ```
       kubectl get configmap metrics-server-config -n kube-system -o yaml
       ```
       {: pre}
    *  若為 `heapster`，請執行下列指令：
       ```
       kubectl get configmap heapster-config -n kube-system -o yaml
       ```
       {: pre}
    輸出範例：
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

2.  將 `memoryPerNode` 欄位新增至 `data.NannyConfiguration` 區段中的 ConfigMap。`metrics-server` 及 `heapster` 的預設值都會設為 `4Mi`。
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

3.  套用您的變更。
    ```
    kubectl apply -f heapster-config.yaml
    ```
    {: pre}

4.  監視度量提供者 Pod，以查看是否由於 `OOMKilled` 錯誤訊息而繼續重新啟動容器。若是如此，請重複這些步驟並增加 `memoryPerNode` 大小，直到 Pod 穩定為止。

想要調整其他設定嗎？請參閱 [Kubernetes 附加程式大小調整器配置文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/kubernetes/autoscaler/tree/master/addon-resizer#addon-resizer-configuration)，以取得其他構想。
{: tip}
