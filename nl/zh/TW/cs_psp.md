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


# 配置 Pod 安全原則
{: #psp}

使用 [pod 安全原則 (PSP) ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)，可以配置原則來授權誰可以在 {{site.data.keyword.containerlong}} 中建立和更新 Pod。

**為什麼要設定 Pod 安全原則？**</br>
當您作為叢集管理者時，您要控制叢集裡發生的情況，特別是影響叢集的安全或準備情形的動作。Pod 安全原則可協助您控制特許容器、root 名稱空間、主機網路及埠、磁區類型、主機檔案系統、Linux 許可權（例如唯讀或群組 ID）等等的使用。

使用 `PodSecurityPolicy` 許可控制器時，需要先[授權原則](#customize_psp)，然後才能建立 Pod。設定 Pod 安全原則可能會有非預期的副作用，因此請務必在變更原則之後測試部署。若要部署應用程式，部署 Pod 所需的 Pod 安全原則必須全部授權使用者及服務帳戶。例如，如果您使用 [Helm](/docs/containers?topic=containers-helm#public_helm_install) 來安裝應用程式，則 Helm Tiller 元件會建立 Pod，因此，您必須有正確的 Pod 安全原則授權。

要嘗試控制哪些使用者可以存取 {{site.data.keyword.containerlong_notm}} 嗎？請參閱[指派叢集存取](/docs/containers?topic=containers-users#users)，以設定 {{site.data.keyword.Bluemix_notm}} IAM 及基礎架構許可權。
{: tip}

**依預設會設定任何原則嗎？我可以新增哪些原則？**</br>
依預設，{{site.data.keyword.containerlong_notm}} 會使用您無法刪除或修改的 [{{site.data.keyword.IBM_notm}} 叢集管理的資源](#ibm_psp)來配置 `PodSecurityPolicy` 許可控制器。您也無法停用許可控制器。

依預設，不會鎖定 Pod 動作。相反地，叢集裡的兩個角色型存取控制 (RBAC) 資源會授權所有管理者、使用者、服務及節點來建立特許及非特許 Pod。為了可攜性，包括其他 RBAC 資源，搭配用於[混合式部署](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp)的「{{site.data.keyword.Bluemix_notm}} 專用」套件。

如果您要防止特定使用者建立或更新 Pod，則可以[修改這些 RBAC 資源或建立自己的 RBAC 資源](#customize_psp)。

**原則授權如何運作？**</br>
當您作為使用者時，可直接建立 Pod，但使用控制器（例如部署）無法建立 Pod，則會根據您獲授權使用的 Pod 安全原則來驗證您的認證。如果原則不支援 Pod 安全需求，則不會建立 Pod。

當您使用資源控制器（例如部署）來建立 Pod 時，Kubernetes 會根據服務帳戶獲授權使用的 Pod 安全原則來驗證 Pod 的服務帳戶認證。如果原則不支援 Pod 安全需求，則控制器會順利，但不會建立 Pod。

如需一般錯誤訊息，請參閱[因為 Pod 安全原則，Pod 無法部署](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_psp)。

**我不屬於 `privileged-psp-user` 叢集角色連結，但為什麼我仍可以建立特許 Pod？**<br>
其他叢集角色連結或作用域限定為名稱空間的角色連結可能會為您提供其他 Pod 安全原則，用於授權您建立特許 Pod。此外，依預設，叢集管理者有權存取所有資源，包括 Pod 安全原則，因此可以將其自身新增到 PSP 或建立特許資源。

## 自訂 Pod 安全原則
{: #customize_psp}

若要防止未獲授權的 Pod 動作，您可以修改現有 Pod 安全原則資源或建立自己的 Pod 安全原則資源。您必須是叢集管理者，才能自訂原則。
{: shortdesc}

**我可以修改哪些現有原則？**</br>
依預設，您的叢集包含下列 RBAC 資源，讓叢集管理者、已鑑別使用者、服務帳戶及節點可使用 `ibm-privileged-psp` 及 `ibm-restricted-psp` Pod 安全原則。這些原則容許使用者建立與更新特許及非特許（受限）Pod。

|名稱| 名稱空間 |類型| 用途 |
|---|---|---|---|
| `privileged-psp-user` |全部       | `ClusterRoleBinding` | 讓叢集管理者、已鑑別使用者、服務帳戶及節點可使用 `ibm-privileged-psp` Pod 安全原則。|
| `restricted-psp-user` |全部       | `ClusterRoleBinding` | 讓叢集管理者、已鑑別使用者、服務帳戶及節點可使用 `ibm-restricted-psp` Pod 安全原則。|
{: caption="您可以修改的預設 RBAC 資源" caption-side="top"}

您可以修改這些 RBAC 角色，以在原則中移除或新增管理者、使用者、服務或節點。

開始之前：
*  [登入您的帳戶。適用的話，請將適當的資源群組設為目標。設定叢集的環境定義。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  瞭解使用 RBAC 角色。如需相關資訊，請參閱[使用自訂 Kubernetes RBAC 角色授權使用者](/docs/containers?topic=containers-users#rbac)或 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview)。
* 確定您具有所有名稱空間的[**管理員** {{site.data.keyword.Bluemix_notm}} IAM 服務存取角色](/docs/containers?topic=containers-users#platform)。

當您修改預設配置時，可以防止重要叢集動作（例如 Pod 部署或叢集更新）。測試其他團隊未依賴的非正式作業叢集裡的變更。
{: important}

**若要修改 RBAC 資源**，請執行下列動作：
1.  取得 RBAC 叢集角色連結的名稱。
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  將叢集角色連結下載為您可在本端編輯的 `.yaml` 檔案。

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml > privileged-psp-user.yaml
    ```
    {: pre}

    您可能要儲存現有原則的副本，以在修改過的原則產生非預期結果時可回復為該副本。
    {: tip}

    **叢集角色連結檔範例**：

    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRoleBinding
    metadata:
      creationTimestamp: 2018-06-20T21:19:50Z
      name: privileged-psp-user
      resourceVersion: "1234567"
      selfLink: /apis/rbac.authorization.k8s.io/v1/clusterrolebindings/privileged-psp-user
      uid: aa1a1111-11aa-11a1-aaaa-1a1111aa11a1
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: ibm-privileged-psp-user
    subjects:
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:masters
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:nodes
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:serviceaccounts
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:authenticated
    ```
    {: codeblock}

3.  編輯叢集角色連結 `.yaml` 檔案。若要瞭解您可以編輯的項目，請檢閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)。動作範例：

    *   **服務帳戶**：您可能要授權服務帳戶，讓部署只能發生在特定名稱空間中。例如，如果您限定原則容許 `kube-system` 名稱空間內的動作，則可能會發生許多重要動作（例如叢集更新）。不過，不再授權其他名稱空間中的動作。

        若要限定原則容許特定名稱空間中的動作，請將 `system:serviceaccounts` 變更為 `system:serviceaccount:<namespace>`。
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:serviceaccount:kube-system
        ```
        {: codeblock}

    *   **使用者**：您可能要移除所有已鑑別使用者的授權，以部署具有特許存取的 Pod。請移除下列 `system:authenticated` 項目。
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:authenticated
        ```
        {: codeblock}

4.  在叢集裡建立修改過的叢集角色連結資源。

    ```
    kubectl apply -f privileged-psp-user.yaml
    ```
    {: pre}

5.  驗證已修改資源。

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml
    ```
    {: pre}

</br>
**若要刪除 RBAC 資源**，請執行下列動作：
1.  取得 RBAC 叢集角色連結的名稱。
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  刪除您要移除的 RBAC 角色。
    ```
    kubectl delete clusterrolebinding privileged-psp-user
    ```
    {: pre}

3.  驗證 RBAC 叢集角色連結不再存在於叢集裡。
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

</br>
**若要建立您自己的 Pod 安全原則**，請執行下列動作：</br>
若要建立您自己的 Pod 安全原則資源，並使用 RBAC 授權使用者，請檢閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)。

請確定您已修改現有原則，讓您建立的新原則不會與現有原則衝突。例如，現有原則允許使用者建立及更新特許 Pod。如果您建立不允許使用者建立或更新特許 Pod 的原則，則現有原則與新原則之間的衝突可能會導致非預期的結果。

## 瞭解 {{site.data.keyword.IBM_notm}} 叢集管理的預設資源
{: #ibm_psp}

{{site.data.keyword.containerlong_notm}} 中的 Kubernetes 叢集包含下列 Pod 安全原則及相關 RBAC 資源，以容許 {{site.data.keyword.IBM_notm}} 適當地管理叢集。
{: shortdesc}

預設 `PodSecurityPolicy` 資源指的是 {{site.data.keyword.IBM_notm}} 所設定的 Pod 安全原則。

**注意**：您不得刪除或修改這些資源。

|名稱| 名稱空間 |類型| 用途 |
|---|---|---|---|
| `ibm-anyuid-hostaccess-psp` |全部       | `PodSecurityPolicy` | 用於完整主機存取 Pod 建立的原則。|
| `ibm-anyuid-hostaccess-psp-user` |全部       | `ClusterRole` | 容許使用 `ibm-anyuid-hostaccess-psp` Pod 安全原則的叢集角色。|
| `ibm-anyuid-hostpath-psp` |全部       | `PodSecurityPolicy` | 用於主機路徑存取 Pod 建立的原則。|
| `ibm-anyuid-hostpath-psp-user` |全部       | `ClusterRole` | 容許使用 `ibm-anyuid-hostpath-psp` Pod 安全原則的叢集角色。|
| `ibm-anyuid-psp` |全部       | `PodSecurityPolicy` | 用於任何 UID/GID 執行檔 Pod 建立的原則。|
| `ibm-anyuid-psp-user` |全部       | `ClusterRole` | 容許使用 `ibm-anyuid-psp` Pod 安全原則的叢集角色。|
| `ibm-privileged-psp` |全部       | `PodSecurityPolicy` | 特許 Pod 建立的原則。|
| `ibm-privileged-psp-user` |全部       | `ClusterRole` | 容許使用 `ibm-privileged-psp` Pod 安全原則的叢集角色。|
| `ibm-privileged-psp-user` | `kube-system` | `RoleBinding` | 讓叢集管理者、服務帳戶及節點使用 `kube-system` 名稱空間中的 `ibm-privileged-psp` Pod 安全原則。|
| `ibm-privileged-psp-user` | `ibm-system` | `RoleBinding` | 讓叢集管理者、服務帳戶及節點使用 `ibm-system` 名稱空間中的 `ibm-privileged-psp` Pod 安全原則。|
| `ibm-privileged-psp-user` | `kubx-cit` | `RoleBinding` | 讓叢集管理者、服務帳戶及節點使用 `kubx-cit` 名稱空間中的 `ibm-privileged-psp` Pod 安全原則。|
| `ibm-restricted-psp` |全部       | `PodSecurityPolicy` | 非特許或受限 Pod 建立的原則。|
| `ibm-restricted-psp-user` |全部       | `ClusterRole` | 容許使用 `ibm-restricted-psp` Pod 安全原則的叢集角色。|
{: caption="您不得修改的 IBM Pod 安全原則資源" caption-side="top"}
