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


# ポッド・セキュリティー・ポリシーの構成
{: #psp}

[ポッド・セキュリティー・ポリシー (PSP) ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/) により、
{{site.data.keyword.containerlong}} のポッドの作成および更新をユーザーに許可するポリシーを構成できます。

**なぜポッド・セキュリティー・ポリシーを設定するのですか?**</br>
クラスター管理者は、クラスター内で行われること (特に、クラスターのセキュリティーや作動可能性に影響を与える操作) を制御する必要があります。 ポッド・セキュリティー・ポリシーにより、特権コンテナー、ルート名前空間、ホスト・ネットワーキングとポート、ボリューム・タイプ、ホスト・ファイル・システム、読み取り専用などの Linux 権限やグループ ID、その他の多くのものの使用を制御できます。

`PodSecurityPolicy` アドミッション・コントローラーにより、クラスター管理者が[ポリシーを許可](#customize_psp)しないとポッドは作成できません。 ポッド・セキュリティー・ポリシーをセットアップすると意図しない副次的影響が生じる可能性があるため、ポリシーを変更した後は、必ず、デプロイメントをテストしてください。 アプリをデプロイするには、ポッドをデプロイするために必要なポッド・セキュリティー・ポリシーで、ユーザーおよびサービス・アカウントをすべて許可しておく必要があります。 例えば、[Helm](/docs/containers?topic=containers-helm#public_helm_install) を使用してアプリをインストールするには、Helm tiller コンポーネントがポッドを作成するので、ポッド・セキュリティー・ポリシーで正しい許可を受けておく必要があります。

{{site.data.keyword.containerlong_notm}} にアクセスするユーザーを制御しようとしていますか? [クラスター・アクセス権限の割り当て](/docs/containers?topic=containers-users#users)を参照して、{{site.data.keyword.Bluemix_notm}} IAM とインフラストラクチャーの権限を設定してください。
{: tip}

**デフォルトで設定されているポリシーはあります? 何を追加できますか? **</br>
デフォルトで、{{site.data.keyword.containerlong_notm}} は、削除も変更もできない [{{site.data.keyword.IBM_notm}} クラスター管理のためのリソース](#ibm_psp)を `PodSecurityPolicy` アドミッション・コントローラーに構成しています。 アドミッション・コントローラーを無効にすることもできません。

デフォルトでは、ポッド操作はロックされていません。 代わりに、クラスターには役割ベース・アクセス制御 (RBAC) のリソースが 2 つあり、これらのリソースにより、すべての管理者、ユーザー、サービス、ノードに特権ポッドと非特権ポッドの作成が許可されています。 [ハイブリッド・デプロイメント](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp)に使用される {{site.data.keyword.Bluemix_notm}} Private パッケージの移植性のために、追加の RBAC リソースが含められています。

特定のユーザーにポッドの作成または更新を禁止したい場合は、[これらの RBAC リソースを変更するか、独自のリソースを作成する](#customize_psp)ことができます。

**ポリシー許可はどのように機能しますか?**</br>
ユーザーがデプロイメントなどのコントローラーを使用するのではなく直接ポッドを作成する場合は、ユーザーの資格情報が、そのユーザーに使用許可を与えられているポッド・セキュリティー・ポリシーに照らして検証されます。 ポッド・セキュリティー要件をサポートするポリシーがない場合、ポッドは作成されません。

デプロイメントなどのリソース・コントローラーを使用してポッドを作成する場合は、Kubernetes により、ポッドのサービス・アカウントの資格情報が、そのサービス・アカウントに使用許可を与えられているポッド・セキュリティー・ポリシーに照らして検証されます。 ポッド・セキュリティー要件をサポートするポリシーがない場合、コントローラーは正常に終了しますが、ポッドは作成されません。

一般的なエラー・メッセージについては、[ポッド・セキュリティー・ポリシーが原因でポッドをデプロイできない](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_psp)を参照してください。

**`privileged-psp-user` クラスター役割バインディングに属していない場合でも、特権ポッドを作成できるのはなぜですか?**<br>
他のクラスター役割バインディングまたは名前空間に有効範囲が設定された役割バインディングにより、特権ポッドの作成を許可する他のポッド・セキュリティー・ポリシーが提供される場合があります。 さらに、デフォルトでは、クラスター管理者はポッド・セキュリティー・ポリシーを含むすべてのリソースにアクセスできるため、自身を PSP に追加したり、特権リソースを作成したりできます。

## ポッド・セキュリティー・ポリシーのカスタマイズ
{: #customize_psp}

不正なポッド操作を防止するために、ポッド・セキュリティー・ポリシーの既存のリソースを変更したり、独自のリソースを作成したりできます。 ポリシーをカスタマイズするには、クラスター管理者でなければなりません。
{: shortdesc}

**どの既存のポリシーを変更できますか?**</br>
デフォルトでは、クラスター管理者、認証済みユーザー、サービス・アカウント、ノードに、` ibm-privileged-psp` および `ibm-restricted-psp` ポッド・セキュリティー・ポリシーの使用を許可する以下の RBAC リソースがクラスターに含まれています。 これらのポリシーにより、
前記ユーザーは特権ポッドと非特権 (制限付き) ポッドを作成および更新できます。

| 名前 | 名前空間 | タイプ | 目的 |
|---|---|---|---|
| `privileged-psp-user` | すべて | `ClusterRoleBinding` | クラスター管理者、認証済みユーザー、サービス・アカウント、ノードに `ibm-privileged-psp` ポッド・セキュリティー・ポリシーの使用を許可します。 |
| `restricted-psp-user` | すべて | `ClusterRoleBinding` | クラスター管理者、認証済みユーザー、サービス・アカウント、ノードに `ibm-restricted-psp` ポッド・セキュリティー・ポリシーの使用を許可します。 |
{: caption="変更できるデフォルトの RBAC リソース" caption-side="top"}

これらの RBAC ロールを変更して、管理者、ユーザー、サービス、またはノードを、ポリシーから削除したりポリシーに追加したりできます。

開始前に、以下のことを行います。
*  [アカウントにログインします。 該当する場合は、適切なリソース・グループをターゲットにします。 クラスターのコンテキストを設定します。](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  RBAC 役割の使用方法を理解します。 詳しくは、[カスタム Kubernetes RBAC 役割によるユーザーの許可](/docs/containers?topic=containers-users#rbac)または [Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview) を参照してください。
* すべての名前空間に対して[**管理者**の {{site.data.keyword.Bluemix_notm}} IAM サービス・アクセス役割](/docs/containers?topic=containers-users#platform)を持っていることを確認してください。

デフォルト構成を変更すると、ポッドのデプロイメントやクラスターの更新などの重要なクラスター操作を禁止できます。 他のチームが依存していない非実稼働クラスターで変更内容をテストしてください。
{: important}

**RBAC リソースを変更するには**:
1.  RBAC クラスター役割バインディングの名前を確認します。
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  ローカルで編集できる `.yaml` ファイルとして、クラスター役割バインディングをダウンロードします。

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml > privileged-psp-user.yaml
    ```
    {: pre}

    既存のポリシーのコピーを保存しておくと、変更したポリシーによって予期しない結果になった場合に復元することができます。
    {: tip}

    **クラスター役割バインディング・ファイルの例**:

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

3.  クラスター役割バインディング `.yaml` ファイルを編集します。 編集できる内容を理解するには、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/) を参照してください。 操作の例:

    *   **サービス・アカウント**: サービス・アカウントを許可して、特定の名前空間でのみデプロイメントが行われるようにすることができます。 例えば、`kube-system` 名前空間での操作を許可するようにポリシーの有効範囲を設定した場合、クラスターの更新などの多くの重要な操作が実行可能です。 しかし、その他の名前空間での操作は許可されなくなります。

        特定の名前空間での操作を許可するようにポリシーの有効範囲を設定するには、`system:serviceaccounts` を `system:serviceaccount:<namespace>` に変更します。
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:serviceaccount:kube-system
        ```
        {: codeblock}

    *   **ユーザー**: 特権アクセスを持つポッドをデプロイする許可を、すべての認証済みユーザーから取り消すことができます。 以下の `system:authenticated` という入力値を削除してください。
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:authenticated
        ```
        {: codeblock}

4.  変更したクラスター役割バインディング・リソースをクラスター内に作成します。

    ```
    kubectl apply -f privileged-psp-user.yaml
    ```
    {: pre}

5.  リソースが変更されたことを確認します。

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml
    ```
    {: pre}

</br>
**RBAC リソースを削除するには**:
1.  RBAC クラスター役割バインディングの名前を確認します。
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  削除する RBAC 役割を削除します。
    ```
    kubectl delete clusterrolebinding privileged-psp-user
    ```
    {: pre}

3.  RBAC クラスター役割バインディングがクラスター内に存在しなくなったことを確認します。
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

</br>
**独自のポッド・セキュリティー・ポリシーを作成するには、以下のようにします**。</br>
独自のポッド・セキュリティー・ポリシーのリソースを作成し、RBAC を使用してユーザーを許可するには、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/) を参照してください。

既存のポリシーを変更する場合は、作成した新しいポリシーが既存のポリシーと矛盾しないようにしてください。 例えば、既存のポリシーが特権ポッドの作成と更新をユーザーに許可しているとします。 特権ポッドの作成または更新をユーザーに許可しないポリシーを作成した場合は、既存のポリシーと新しいポリシーの矛盾が原因で、予期しない結果になる可能性があります。

## {{site.data.keyword.IBM_notm}} クラスター管理のためのデフォルトのリソースについて
{: #ibm_psp}

{{site.data.keyword.containerlong_notm}} の Kubernetes クラスターには、{{site.data.keyword.IBM_notm}} がクラスターを正しく管理できるようにするために、以下のポッド・セキュリティー・ポリシーおよび関連 RBAC リソースが含まれています。
{: shortdesc}

デフォルトの `PodSecurityPolicy` リソースは、{{site.data.keyword.IBM_notm}} によって設定されるポッド・セキュリティー・ポリシーを参照します。

**注意**: これらのリソースは削除も変更もしないでください。

| 名前 | 名前空間 | タイプ | 目的 |
|---|---|---|---|
| `ibm-anyuid-hostaccess-psp` | すべて | `PodSecurityPolicy` | フル・ホスト・アクセスのポッド作成用のポリシー。 |
| `ibm-anyuid-hostaccess-psp-user` | すべて | `ClusterRole` | `ibm-anyuid-hostaccess-psp` ポッド・セキュリティー・ポリシーの使用を許可するクラスター役割。 |
| `ibm-anyuid-hostpath-psp` | すべて | `PodSecurityPolicy` | ホスト・パス・アクセス・ポッド作成用のポリシー。 |
| `ibm-anyuid-hostpath-psp-user` | すべて | `ClusterRole` | `ibm-anyuid-hostpath-psp` ポッド・セキュリティー・ポリシーの使用を許可するクラスター役割。 |
| `ibm-anyuid-psp` | すべて | `PodSecurityPolicy` | UID/GID 実行可能ファイルのポッド作成用のポリシー。 |
| `ibm-anyuid-psp-user` | すべて | `ClusterRole` | `ibm-anyuid-psp` ポッド・セキュリティー・ポリシーの使用を許可するクラスター役割。 |
| `ibm-privileged-psp` | すべて | `PodSecurityPolicy` | 特権ポッドの作成のポリシー。 |
| `ibm-privileged-psp-user` | すべて | `ClusterRole` | `ibm-privileged-psp` ポッド・セキュリティー・ポリシーの使用を許可するクラスター役割。 |
| `ibm-privileged-psp-user` | `kube-system` | `RoleBinding` | クラスター管理者、サービス・アカウント、ノードに、`kube-system` 名前空間での `ibm-privileged-psp` ポッド・セキュリティー・ポリシーの使用を許可します。 |
| `ibm-privileged-psp-user` | `ibm-system` | `RoleBinding` | クラスター管理者、サービス・アカウント、ノードに、`ibm-system` 名前空間での `ibm-privileged-psp` ポッド・セキュリティー・ポリシーの使用を許可します。 |
| `ibm-privileged-psp-user` | `kubx-cit` | `RoleBinding` | クラスター管理者、サービス・アカウント、ノードに、`kubx-cit` 名前空間での `ibm-privileged-psp` ポッド・セキュリティー・ポリシーの使用を許可します。 |
| `ibm-restricted-psp` | すべて | `PodSecurityPolicy` | 非特権ポッドまたは制限付きポッドの作成のポリシー。 |
| `ibm-restricted-psp-user` | すべて | `ClusterRole` | `ibm-restricted-psp` ポッド・セキュリティー・ポリシーの使用を許可するクラスター役割。 |
{: caption="ユーザーが変更してはいけない IBM ポッド・セキュリティー・ポリシーのリソース" caption-side="top"}
