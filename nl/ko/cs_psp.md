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


# 팟(Pod) 보안 정책 구성
{: #psp}

[팟(Pod) 보안 정책(PSP) ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)을 사용하면
{{site.data.keyword.containerlong}}에서 팟(Pod)을 작성하고 업데이트할 수 있는 사용자에게 권한을 부여하는 정책을 구성할 수 있습니다.

**팟(Pod) 보안 정책을 설정하는 이유는 무엇입니까?**</br>
클러스터 관리자인 경우에는 클러스터에서 무슨 일이 발생하는지(특히, 클러스터의 보안 또는 준비에 영향을 주는 조치)를 제어하고자 합니다. 팟(Pod) 보안 정책은 권한 있는 컨테이너, 루트 네임스페이스, 호스트 네트워킹 및 포트, 볼륨 유형, 호스트 파일 시스템, Linux 권한(예: 읽기 전용) 또는 그룹 ID 등의 사용을 제어하는 데 도움을 줄 수 있습니다.

`PodSecurityPolicy` 허가 제어기에서는 사용자가 [정책을 권한 부여](#customize_psp)할 때까지 팟(Pod)이 작성될 수 없습니다. 팟(Pod) 보안 정책 설정에는 의도치 않은 부작용이 있을 수 있으므로 정책을 변경한 후에는 반드시 배치를 테스트해야 합니다. 앱을 배치하려면 사용자 및 서비스 계정이 팟(Pod) 배치에 필요한 팟(Pod) 보안 정책에 의해 모두 권한 부여되어야 합니다. 예를 들어, [Helm](/docs/containers?topic=containers-helm#public_helm_install)을 사용하여 앱을 설치하면 Helm Tiller 컴포넌트가 팟(Pod)을 작성하므로 사용자에게는 올바른 팟(Pod) 보안 정책 권한이 있어야 합니다.

{{site.data.keyword.containerlong_notm}}에 액세스할 수 있는 사용자를 제어하고자 합니까? [클러스터 액세스 권한 지정](/docs/containers?topic=containers-users#users)을 참조하여 {{site.data.keyword.Bluemix_notm}} IAM 및 인프라 권한을 설정하십시오.
{: tip}

**기본적으로 설정되는 정책이 있습니까? 무엇을 추가할 수 있습니까?**</br>
기본적으로, {{site.data.keyword.containerlong_notm}}는 사용자가 삭제하거나 수정할 수 없는 [{{site.data.keyword.IBM_notm}} 클러스터 관리용 리소스](#ibm_psp)를 사용하여 `PodSecurityPolicy` 허가 제어기를 구성합니다. 또한 사용자는 허가 제어기를 사용 중지할 수 없습니다.

팟(Pod) 조치는 기본적으로 잠겨 있지 않습니다. 그 대신에 클러스터에서 2개의 역할 기반 액세스 제어(RBAC) 리소스가 권한 있는 팟Pod) 및 권한이 없는 팟(Pod)을 작성할 수 있도록 모든 관리자, 사용자, 서비스 및 노드에 권한을 부여합니다. 추가 RBAC 리소스가 [하이브리드 배치](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp)에 사용되는 {{site.data.keyword.Bluemix_notm}} 개인용 패키지의 이식성을 위해 포함됩니다.

특정 사용자가 팟(Pod)을 작성하거나 업데이트하지 못하도록 방지하려면 [이러한 RBAC 리소스를 수정하거나 자체 리소스를 작성](#customize_psp)할 수 있습니다.

**정책 권한 부여는 어떻게 작동합니까?**</br>
배치 등의 제어기 사용 없이 사용자로서 직접 팟(Pod)을 작성하는 경우, 인증 정보는 사용 권한이 있는 팟(Pod) 보안 정책에 대해 유효성이 검증됩니다. 팟(Pod) 보안 요구사항을 지원하는 정책이 없으면 팟(Pod)이 작성되지 않습니다.

배치 등의 리소스 제어기를 사용하여 팟(Pod)을 작성하는 경우, Kubernetes는 서비스 계정이 사용 권한을 보유한 팟(Pod) 보안 정책에 대해 팟(Pod)의 서비스 계정 인증 정보를 유효성 검증합니다. 팟(Pod) 보안 요구사항을 지원하는 정책이 없는 경우, 제어기는 성공하지만 팟(Pod)은 작성되지 않습니다.

공통 오류 메시지에 대해서는 [팟(Pod) 보안 정책으로 인한 팟(Pod) 배치 실패](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_psp)를 참조하십시오.

**`privileged-psp-user` 클러스터 역할 바인딩에 속하지 않는 경우에도 권한 있는 팟(Pod)을 계속해서 작성할 수 있는 이유는 무엇입니까?**<br>
기타 클러스터 역할 바인딩 또는 네임스페이스 범위 역할 바인딩은 권한 있는 팟(Pod)을 작성하도록 권한을 부여하는 다른 팟(Pod) 보안 정책을 사용자에게 제공할 수 있습니다. 또한 기본적으로 클러스터 관리자는 팟(Pod) 보안 정책을 포함하여 모든 리소스에 대한 액세스 권한이 있으므로 PSP에 추가하거나 권한 있는 리소스를 작성할 수 있습니다. 

## 팟(Pod) 보안 정책의 사용자 정의
{: #customize_psp}

비인가된 팟(Pod) 조치를 방지하려면 기존의 팟(Pod) 보안 정책 리소스를 수정하거나 자체 리소스를 작성할 수 있습니다. 정책을 사용자 정의하려면 클러스터 관리자여야 합니다.
{: shortdesc}

**수정 가능한 기존 정책은 무엇입니까?**</br>
기본적으로, 클러스터에는 클러스터 관리자, 인증된 사용자, 서비스 계정 및 노드가 `ibm-privileged-psp` 및 `ibm-restricted-psp` 팟(Pod) 보안 정책을 사용할 수 있도록 하는 다음의 RBAC 리소스가 포함되어 있습니다. 이러한 정책을 사용하여 사용자는 권한 있는 팟(Pod) 및 권한이 없는(제한된) 팟(Pod)을 작성하고 업데이트할 수 있습니다.

|이름 |네임스페이스 |유형 |용도 |
|---|---|---|---|
| `privileged-psp-user` |모두 | `ClusterRoleBinding` |클러스터 관리자, 인증된 사용자, 서비스 계정 및 노드가 `ibm-privileged-psp` 팟(Pod) 보안 정책을 사용할 수 있도록 합니다. |
| `restricted-psp-user` |모두 | `ClusterRoleBinding` |클러스터 관리자, 인증된 사용자, 서비스 계정 및 노드가 `ibm-restricted-psp` 팟(Pod) 보안 정책을 사용할 수 있도록 합니다. |
{: caption="수정이 가능한 기본 RBAC 리소스" caption-side="top"}

이러한 RBAC 역할을 수정하여 관리자, 사용자, 서비스 또는 노드를 정책에서 제거하거나 추가할 수 있습니다.

시작하기 전에:
*  [계정에 로그인하십시오. 해당되는 경우, 적절한 리소스 그룹을 대상으로 지정하십시오. 클러스터의 컨텍스트를 설정하십시오.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  RBAC 역할 관련 작업에 대해 잘 숙지하십시오. 자세한 정보는 [사용자 정의 Kubernetes RBAC 역할로 사용자의 권한 부여](/docs/containers?topic=containers-users#rbac) 또는 [Kubernetes 문서![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview)를 참조하십시오.
* 모든 네임스페이스에 대해 [**관리자** {{site.data.keyword.Bluemix_notm}} IAM 서비스 액세스 역할](/docs/containers?topic=containers-users#platform)이 있는지 확인하십시오.

기본 구성을 수정하면 중요한 클러스터 조치(예: 팟(Pod) 배치 또는 클러스터 업데이트)를 방지할 수 있습니다. 다른 팀에게 영향을 주지 않는 비-프로덕션 클러스터에서 변경사항을 테스트하십시오.
{: important}

**RBAC 리소스 수정**:
1.  RBAC 클러스터 역할 바인딩의 이름을 가져오십시오.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  로컬로 편집할 수 있는 `.yaml` 파일로서 클러스터 역할 바인딩을 다운로드하십시오.

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml > privileged-psp-user.yaml
    ```
    {: pre}

    수정된 정책이 예상치 못한 결과를 가져오는 경우에 이를 되돌릴 수 있도록 기존 정책의 사본을 저장하고자 할 수 있습니다.
    {: tip}

    **예제 클러스터 역할 바인딩 파일**:

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

3.  클러스터 역할 바인딩 `.yaml` 파일을 편집하십시오. 편집할 수 있는 내용을 알아보려면 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)를 검토하십시오. 예제 조치:

    *   **서비스 계정**: 특정 네임스페이스에서만 배치가 발생할 수 있도록 서비스 계정에 권한을 부여하고자 할 수 있습니다. 예를 들어, `kube-system` 네임스페이스 내에서 조치를 허용하도록 정책 범위를 지정하는 경우에는 많은 중요한 조치(예: 클러스터 업데이트)가 발생할 수 있습니다. 그러나 기타 네임스페이스의 조치에는 더 이상 권한이 부여되지 않습니다.

        특정 네임스페이스의 조치를 허용하도록 정책 범위를 지정하려면 `system:serviceaccounts`를 `system:serviceaccount:<namespace>`로 변경하십시오.
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:serviceaccount:kube-system
        ```
        {: codeblock}

    *   **사용자**: 권한 있는 액세스 권한으로 팟(Pod)을 배치할 수 있도록 인증된 모든 사용자에 대해 권한을 제거하고자 할 수 있습니다. 다음의 `system:authenticated` 항목을 제거하십시오.
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:authenticated
        ```
        {: codeblock}

4.  클러스터에서 수정된 클러스터 역할 바인딩 리소스를 작성하십시오.

    ```
    kubectl apply -f privileged-psp-user.yaml
    ```
    {: pre}

5.  리소스가 수정되었는지 확인하십시오.

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml
    ```
    {: pre}

</br>
**RBAC 리소스 삭제**:
1.  RBAC 클러스터 역할 바인딩의 이름을 가져오십시오.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  제거할 RBAC 역할을 삭제하십시오.
    ```
    kubectl delete clusterrolebinding privileged-psp-user
    ```
    {: pre}

3.  RBAC 클러스터 역할 바인딩이 더 이상 클러스터에 없는지 확인하십시오.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

</br>
**자체 팟(Pod) 보안 정책 작성**:</br>
자체 팟(Pod) 보안 정책 리소스를 작성하고 RBAC로 사용자의 권한을 부여하려면 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)를 참조하십시오.

작성된 새 정책이 기존 정책과 충돌하지 않도록 기존 정책을 수정했는지 확인하십시오. 예를 들어, 기존 정책은 사용자가 권한 있는 팟(Pod)을 작성하고 업데이트할 수 있도록 허용합니다. 사용자가 권한 있는 팟(Pod)을 작성 및 업데이트할 수 있도록 허용하지 않는 정책을 작성하면 기존 정책과 새 정책 간의 충돌로 인해 예상치 못한 결과가 발생할 수 있습니다.

## {{site.data.keyword.IBM_notm}} 클러스터 관리용 기본 리소스 이해
{: #ibm_psp}

{{site.data.keyword.containerlong_notm}}의 Kubernetes 클러스터에는 {{site.data.keyword.IBM_notm}}이 클러스터를 적절하게 관리할 수 있도록 허용하는 다음의 팟(Pod) 보안 정책 및 관련 RBAC 리소스가 포함되어 있습니다.
{: shortdesc}

기본 `PodSecurityPolicy` 리소스는 {{site.data.keyword.IBM_notm}}에서 설정한 팟(Pod) 보안 정책을 참조합니다.

**주의**: 이러한 리소스는 절대로 삭제하거나 수정하지 마십시오.

|이름 |네임스페이스 |유형 |용도 |
|---|---|---|---|
| `ibm-anyuid-hostaccess-psp` |모두 | `PodSecurityPolicy` | 전체 호스트 액세스 팟(Pod) 작성을 위한 정책입니다. |
| `ibm-anyuid-hostaccess-psp-user` |모두 | `ClusterRole` | `ibm-anyuid-hostaccess-psp` 팟(Pod) 보안 정책의 사용을 허용하는 클러스터 역할입니다. |
| `ibm-anyuid-hostpath-psp` |모두 | `PodSecurityPolicy` | 호스트 경로 액세스 팟(Pod) 작성을 위한 정책입니다. |
| `ibm-anyuid-hostpath-psp-user` |모두 | `ClusterRole` |`ibm-anyuid-hostpath-psp` 팟(Pod) 보안 정책의 사용을 허용하는 클러스터 역할입니다. |
| `ibm-anyuid-psp` |모두 | `PodSecurityPolicy` | UID/GID 실행 가능 팟(Pod) 작성을 위한 정책입니다. |
| `ibm-anyuid-psp-user` |모두 | `ClusterRole` | `ibm-anyuid-psp` 팟(Pod) 보안 정책의 사용을 허용하는 클러스터 역할입니다. |
| `ibm-privileged-psp` |모두 | `PodSecurityPolicy` |권한 있는 팟(Pod) 작성을 위한 정책입니다. |
| `ibm-privileged-psp-user` |모두 | `ClusterRole` |`ibm-privileged-psp` 팟(Pod) 보안 정책의 사용을 허용하는 클러스터 역할입니다. |
| `ibm-privileged-psp-user` | `kube-system` | `RoleBinding` |클러스터 관리자, 서비스 계정 및 노드가 `kube-system` 네임스페이스에서 `ibm-privileged-psp` 팟(Pod) 보안 정책을 사용할 수 있도록 합니다. |
| `ibm-privileged-psp-user` | `ibm-system` | `RoleBinding` |클러스터 관리자, 서비스 계정 및 노드가 `ibm-system` 네임스페이스에서 `ibm-privileged-psp` 팟(Pod) 보안 정책을 사용할 수 있도록 합니다. |
| `ibm-privileged-psp-user` | `kubx-cit` | `RoleBinding` |클러스터 관리자, 서비스 계정 및 노드가 `kubx-cit` 네임스페이스에서 `ibm-privileged-psp` 팟(Pod) 보안 정책을 사용할 수 있도록 합니다. |
| `ibm-restricted-psp` |모두 | `PodSecurityPolicy` |비인가된 또는 제한된 팟(Pod) 작성에 대한 정책입니다. |
| `ibm-restricted-psp-user` |모두 | `ClusterRole` |`ibm-restricted-psp` 팟(Pod) 보안 정책의 사용을 허용하는 클러스터 역할입니다. |
{: caption="수정하지 않아야 할 IBM 팟(Pod) 보안 정책 리소스" caption-side="top"}
