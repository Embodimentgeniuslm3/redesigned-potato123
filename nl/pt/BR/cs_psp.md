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


# Configurando políticas de segurança do pod
{: #psp}

Com [políticas de segurança de pod (PSPs) ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/), é possível
configurar políticas para autorizar quem pode criar e atualizar os pods no {{site.data.keyword.containerlong}}.

**Por que configuro políticas de segurança de pod?**</br>
Como um administrador de cluster, você deseja controlar o que acontece em seu cluster, especialmente as ações que afetam a segurança ou a prontidão do cluster. As políticas de segurança de pod podem ajudá-lo a controlar o uso de contêineres privilegiados, namespaces de raiz, rede e portas do host, tipos de volume, sistemas de arquivos host, permissões do Linux como somente leitura ou IDs de grupo, etc.

Com o controlador de admissão `PodSecurityPolicy`, nenhum pod pode ser criado até que você [autorize políticas](#customize_psp). A configuração de políticas de segurança de pod pode ter efeitos colaterais indesejados, portanto, certifique-se de testar uma implementação depois de mudar a política. Para implementar apps, as contas de usuário e de serviço devem ser todas autorizadas pelas políticas de segurança de pod que são necessárias para implementar pods. Por exemplo, se você instala apps usando [Helm](/docs/containers?topic=containers-helm#public_helm_install), o componente Helm tiller cria os pods e, portanto, deve-se ter a autorização de política de segurança de pod correta.

Tentando controlar quais usuários têm acesso ao {{site.data.keyword.containerlong_notm}}? Consulte [Designando acesso ao cluster](/docs/containers?topic=containers-users#users) para configurar as permissões do {{site.data.keyword.Bluemix_notm}} IAM e de infraestrutura.
{: tip}

** Há alguma política definida por padrão? O que posso incluir?**</br>
Por padrão, o {{site.data.keyword.containerlong_notm}} configura o controlador de admissão `PodSecurityPolicy` com [recursos para o gerenciamento de cluster {{site.data.keyword.IBM_notm}}](#ibm_psp) que não é possível excluir ou modificar. Também não é possível desativar o controlador de admissão.

As ações de pod não são bloqueadas por padrão. Em vez disso, dois recursos de controle de acesso baseado em função (RBAC) no cluster autorizam todos os administradores, usuários, serviços e nós para criar pods privilegiados e não privilegiados. Os recursos RBAC adicionais são incluídos para portabilidade com pacotes do {{site.data.keyword.Bluemix_notm}} Private que são usados para [implementações híbridas](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp).

Se você deseja evitar que determinados usuários criem ou atualizem pods, é possível [modificar esses recursos RBAC ou criar o seu próprio](#customize_psp).

**Como a autorização de política funciona?**</br>
Quando você, como um usuário, cria um pod diretamente e não usando um controlador como uma implementação, suas credenciais são validadas com relação às políticas de segurança de pod que você está autorizado a usar. Se nenhuma política suportar os requisitos de segurança de pod, o pod não será criado.

Ao criar um pod usando um controlador de recurso, como uma implementação, o Kubernetes valida as credenciais de conta do serviço do pod com relação às políticas de segurança de pod que a conta do serviço está autorizada a usar. Se nenhuma política suportar os requisitos de segurança de pod, o controlador será bem-sucedido, mas o pod não será criado.

Para mensagens de erro comum, consulte [Os pods falham ao serem implementados devido a uma política de segurança de pod](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_psp).

**Por que ainda posso criar pods privilegiados quando eu não faço parte da ligação de função de cluster `privileged-psp-user`?**<br>
Outras ligações de função de cluster ou ligações de função com escopo definido por namespace podem fornecer outras políticas de segurança de pod que autorizam você a criar pods privilegiados. Além disso, por padrão, os administradores de cluster têm acesso a todos os recursos, incluindo políticas de segurança de pod, e, portanto, podem se incluir em PSPs ou criar recursos privilegiados.

## Customizando políticas de segurança do pod
{: #customize_psp}

Para evitar ações de pod desautorizadas, é possível modificar os recursos de política de segurança de pod existentes ou criar o seu próprio. Deve-se ser um administrador de cluster para customizar políticas.
{: shortdesc}

**Quais políticas existentes posso modificar?**</br>
Por padrão, seu cluster contém os recursos RBAC a seguir que permitem que os administradores
de cluster, usuários autenticados, contas de serviço e nós usem as
políticas de segurança de pod `ibm-privileged-psp` e `ibm-restricted-psp`. Essas
políticas permitem que os usuários criem e atualizem os pods privilegiados e não privilegiados (restritos).

| Nome | Namespace | Tipo | Propósito |
|---|---|---|---|
| `privileged-psp-user` | Todos | `ClusterRoleBinding` | Ativa administradores de cluster, usuários autenticados, contas de serviço e nós para usar a política de segurança de pod `ibm-privileged-psp`. |
| `restricted-psp-user` | Todos | `ClusterRoleBinding` | Ativa administradores de cluster, usuários autenticados, contas de serviço e nós para usar a política de segurança de pod `ibm-restricted-psp`. |
{: caption="Recursos RBAC padrão que podem ser modificados" caption-side="top"}

É possível modificar essas funções RBAC para remover ou incluir administradores, usuários, serviços ou nós para a política.

Antes de iniciar:
*  [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  Entender trabalhar com funções RBAC. Para obter mais informações, consulte [Autorizando usuários com funções RBAC customizadas do Kubernetes](/docs/containers?topic=containers-users#rbac) ou a [documentação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview).
* Assegure-se de que você tenha a [função de acesso de serviço do **Manager** {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para todos os namespaces.

Quando você modifica a configuração padrão, é possível evitar ações de cluster importantes, como implementações de pod ou atualizações de cluster. Teste suas mudanças em um cluster não de produção do qual as outras equipes não dependem.
{: important}

**Para modificar os recursos RBAC**:
1.  Obtenha o nome da ligação de função de cluster do RBAC.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  Faça download da ligação de função de cluster como um arquivo `.yaml` que é possível editar localmente.

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml > privileged-psp-user.yaml
    ```
    {: pre}

    Você pode desejar salvar uma cópia da política existente para que possa ser revertida para ela se a política modificada produzir resultados inesperados.
    {: tip}

    ** Exemplo de arquivo de ligação de função de cluster **:

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

3.  Edite o arquivo de ligação de função de cluster `.yaml`. Para entender o que é possível editar, revise a [documentação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/). Exemplo de ações:

    *   **Contas de serviço **: você pode desejar autorizar as contas de serviço para que as implementações possam ocorrer somente em namespaces específicos. Por exemplo, se você definir o escopo da política para permitir ações dentro do namespace `kube-system`, muitas ações importantes, como atualizações de cluster, poderão ocorrer. No entanto, as ações em outros namespaces não serão mais autorizadas.

        Para definir o escopo da política para permitir ações em um namespace específico, mude o `system:serviceaccounts` para `system:serviceaccount:<namespace>`.
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:serviceaccount:kube-system
        ```
        {: codeblock}

    *   **Usuários**: você pode desejar remover a autorização para todos os usuários autenticados para implementar pods com acesso privilegiado. Remova a entrada `system:authenticated` a seguir.
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:authenticated
        ```
        {: codeblock}

4.  Crie o recurso de ligação de função de cluster modificado em seu cluster.

    ```
    kubectl apply -f privileged-psp-user.yaml
    ```
    {: pre}

5.  Verifique se o recurso foi modificado.

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml
    ```
    {: pre}

</br>
** Para excluir os recursos do RBAC **:
1.  Obtenha o nome da ligação de função de cluster do RBAC.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  Exclua a função RBAC que você deseja remover.
    ```
    kubectl delete clusterrolebinding privileged-psp-user
    ```
    {: pre}

3.  Verifique se a ligação de função de cluster RBAC não está mais em seu cluster.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

</br>
**Para criar sua própria política de segurança de pod**:</br>
Para criar seu próprio recurso de política de segurança de pod e autorizar usuários com RBAC, revise a [documentação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/).

Certifique-se de ter modificado as políticas existentes de modo que a nova política que você criar não entre em conflito com a política existente. Por exemplo, a política existente permite que os usuários criem e atualizem pods privilegiados. Se você criar uma política que não permita que os usuários criem ou atualizem pods privilegiados, o conflito entre a política nova e a existente poderá causar resultados inesperados.

## Entendendo os recursos padrão para o gerenciamento de cluster do  {{site.data.keyword.IBM_notm}}
{: #ibm_psp}

Seu cluster do Kubernetes no {{site.data.keyword.containerlong_notm}} contém as políticas de
segurança de pod a seguir e os recursos RBAC relacionados para permitir que a {{site.data.keyword.IBM_notm}} gerencie adequadamente seu cluster.
{: shortdesc}

Os recursos `PodSecurityPolicy` padrão referem-se às políticas de segurança de pod que são configuradas pelo {{site.data.keyword.IBM_notm}}.

**Atenção**: não se deve excluir ou modificar esses recursos.

| Nome | Namespace | Tipo | Propósito |
|---|---|---|---|
| `ibm-anyuid-hostaccess-psp` | Todos | `PodSecurityPolicy` | Política para a criação do pod de acesso de host integral. |
| `ibm-anyuid-hostaccess-psp-user` | Todos | `ClusterRole` | Função de cluster que permite o uso da política de segurança de pod `ibm-anyuid-hostaccess-psp`. |
| `ibm-anyuid-hostpath-psp` | Todos | `PodSecurityPolicy` | Política para a criação do pod de acesso de caminho do host. |
| `ibm-anyuid-hostpath-psp-user` | Todos | `ClusterRole` | Função de cluster que permite o uso da política de segurança de pod `ibm-anyuid-hostpath-psp`. |
| `ibm-anyuid-psp` | Todos | `PodSecurityPolicy` | Política para qualquer criação de pod executável UID/GID. |
| `ibm-anyuid-psp-user` | Todos | `ClusterRole` | Função de cluster que permite o uso da política de segurança de pod `ibm-anyuid-psp`. |
| `ibm-privileged-psp` | Todos | `PodSecurityPolicy` | Política para criação de pod privilegiado. |
| `ibm-privileged-psp-user` | Todos | `ClusterRole` | Função de cluster que permite o uso da política de segurança de pod `ibm-privileged-psp`. |
| `ibm-privileged-psp-user` | `kube-system` | `RoleBinding` | Permite que administradores de cluster, contas de serviço e nós usem a política de segurança de pod `ibm-privileged-psp` no namespace `kube-system`. |
| `ibm-privileged-psp-user` | `ibm-system` | `RoleBinding` | Permite que administradores de cluster, contas de serviço e nós usem a política de segurança de pod `ibm-privileged-psp` no namespace `ibm-system`. |
| `ibm-privileged-psp-user` | `kubx-cit` | `RoleBinding` | Permite que administradores de cluster, contas de serviço e nós usem a política de segurança de pod `ibm-privileged-psp` no namespace `kubx-cit`. |
| `ibm-restricted-psp` | Todos | `PodSecurityPolicy` | Política para criação não privilegiada ou restrita de pod. |
| `ibm-restricted-psp-user` | Todos | `ClusterRole` | Função de cluster que permite o uso da política de segurança de pod `ibm-restricted-psp`. |
{: caption="Recursos de políticas de segurança de pod da IBM que não devem ser modificados" caption-side="top"}
