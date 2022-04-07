---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

keywords: kubernetes, iks, ImagePullBackOff, registry, image, failed to pull image,

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Resolução de problemas de clusters e nós do trabalhador
{: #cs_troubleshoot_clusters}

Ao usar o {{site.data.keyword.containerlong}}, considere estas técnicas para resolução de problemas de seus clusters e nós do trabalhador.
{: shortdesc}

<p class="tip">Se você tiver um problema mais geral, tente a [depuração do cluster](/docs/containers?topic=containers-cs_troubleshoot).<br>Além disso, enquanto soluciona problemas, é possível usar a [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) para executar testes e reunir informações pertinentes de seu cluster.</p>

## Não é possível criar um cluster ou gerenciar nós do trabalhador devido a erros de permissão
{: #cs_credentials}

{: tsSymptoms}
Você tenta gerenciar nós do trabalhador para um cluster novo ou existente executando um dos comandos a seguir.
* Fornecer trabalhadores: `ibmcloud ks cluster-create`, `ibmcloud ks worker-pool-rebalance` ou `ibmcloud ks worker-pool-resize`
* Recarregar trabalhadores: `ibmcloud ks worker-reload` ou `ibmcloud ks worker-update`
* Reinicializar trabalhadores:  ` ibmcloud ks worker-reboot `
* Exclua os trabalhadores: `ibmcloud ks cluster-rm`, `ibmcloud ks worker-rm`, `ibmcloud ks worker-pool-rebalance`ou `ibmcloud ks worker-Pool-reize`

No entanto, você recebe uma mensagem de erro semelhante a uma das seguintes.

```
Não foi possível se conectar à sua conta de infraestrutura do IBM Cloud (SoftLayer).
Criar um cluster padrão requer que você tenha uma conta
pré-paga vinculada a um termo da conta de infraestrutura do IBM Cloud (SoftLayer)
ou que tenha usado a CLI do {{site.data.keyword.containerlong_notm}} para
configurar as suas chaves API de Infraestrutura do {{site.data.keyword.Bluemix_notm}}.
```
{: screen}

```
Exceção de infraestrutura do {{site.data.keyword.Bluemix_notm}}:
'Item' deve ser pedido com permissão.
```
{: screen}

```
Worker not found. Review {{site.data.keyword.Bluemix_notm}} infrastructure permissions.
```
{: screen}

```
Exceção de infraestrutura do {{site.data.keyword.Bluemix_notm}}:
o usuário não tem as permissões de infraestrutura necessárias do {{site.data.keyword.Bluemix_notm}}
para incluir servidores
```
{: screen}

```
A solicitação de troca de token do IAM falhou: não é possível criar o token do portal do IMS, pois nenhuma conta do IMS está vinculada à conta selecionada do BSS
```
{: screen}

```
O cluster não pôde ser configurado com o registro. Certifique-se de que você tenha a função de Administrador para o {{site.data.keyword.registrylong_notm}}.
```
{: screen}

{: tsCauses}
As credenciais de infraestrutura que são configuradas para a região e o grupo de recursos estão ausentes das [permissões de infraestrutura](/docs/containers?topic=containers-access_reference#infra) apropriadas. As permissões de infraestrutura do usuário são mais comumente armazenadas como uma [chave de API](/docs/containers?topic=containers-users#api_key) para a região e o grupo de recursos. Mais raramente, se você usar um [tipo de conta do {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-users#understand_infra), poderá precisar [configurar credenciais de infraestrutura manualmente](/docs/containers?topic=containers-users#credentials). Se você usar uma conta de infraestrutura do IBM Cloud (SoftLayer) diferente para provisionar recursos de infraestrutura, também poderá ter [clusters órfãos](#orphaned) em sua conta.

{: tsResolve}
O proprietário da conta deve configurar as credenciais de conta de infraestrutura corretamente. As credenciais dependem do tipo de conta de infraestrutura que você está usando.

Antes de iniciar, [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

1.  Identifique quais credenciais do usuário são usadas para as permissões de infraestrutura da região e do grupo de recursos.
    1.  Verifique a chave de API para uma região e um grupo de recursos do cluster.
        ```
        ibmcloud ks api-key-info --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Saída de exemplo:
        ```
        Getting information about the API key owner for cluster <cluster_name>...
        OK
        Name                Email   
        <user_name>         <name@email.com>
        ```
        {: screen}
    2.  Verifique se a conta de infraestrutura para a região e o grupo de recursos é configurada manualmente para usar uma conta de infraestrutura do IBM Cloud (SoftLayer) diferente.
        ```
        ibmcloud ks credential-get --region <us-south>
        ```
        {: pre}

        **Saída de exemplo se as credenciais estiverem configuradas para usar uma conta diferente**. Nesse caso, as credenciais de infraestrutura do usuário serão usadas para a região e o grupo de recursos que você destinou, mesmo se as credenciais de um usuário diferente forem armazenadas na chave de API recuperada na etapa anterior.
        ```
        OK
        Infrastructure credentials for user name <1234567_name@email.com> set for resource group <resource_group_name>.
        ```
        {: screen}

        **Saída de exemplo se as credenciais não estiverem configuradas para usar uma conta diferente**. Nesse caso, o proprietário da chave de API que você recuperou na etapa anterior tem as credenciais de infraestrutura que são usadas para a região e o grupo de recursos.
        ```
        FAILED
        No credentials set for resource group <resource_group_name>.: The user credentials could not be found. (E0051)
        ```
        {: screen}
2.  Valide as permissões de infraestrutura que o usuário tem.
    1.  Liste as permissões de infraestrutura sugeridas e necessárias para a região e o grupo de recursos.
        ```
        ibmcloud ks infra-permissions-get --region <region>
        ```
        {: pre}
    2.  Certifique-se de que o [proprietário das credenciais de infraestrutura para a chave de API ou a conta configurada manualmente tenha as permissões corretas](/docs/containers?topic=containers-users#owner_permissions).
    3.  Se necessário, é possível mudar a [chave de API](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset) ou [configurar manualmente](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) o proprietário das credenciais de infraestrutura para a região e o grupo de recursos.
3.  Teste se as permissões mudadas permitem que os usuários autorizados executem operações de infraestrutura para o cluster.
    1.  Por exemplo, você pode tentar excluir um nó do trabalhador.
        ```
        ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}
    2.  Verifique se o nó do trabalhador foi removido.
        ```
        ibmcloud ks worker-get --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}

        Saída de exemplo se a remoção do nó do trabalhador for bem-sucedida. A operação `worker-get` falha porque o nó do trabalhador foi excluído. As permissões de infraestrutura estão configuradas corretamente.
        ```
        FAILED
        The specified worker node could not be found. (E0011)
        ```
        {: screen}

    3.  Se o nó do trabalhador não foi removido, revise os [campos **Estado** e **Status**](/docs/containers?topic=containers-cs_troubleshoot#debug_worker_nodes) e os [problemas comuns com nós do trabalhador](/docs/containers?topic=containers-cs_troubleshoot#common_worker_nodes_issues) para continuar a depuração.
    4.  Se você configurar manualmente as credenciais e ainda não puder ver os nós do trabalhador do cluster em sua conta de infraestrutura, será possível verificar se o [cluster está órfão](#orphaned).

<br />


## O firewall evita a execução de comandos da CLI
{: #ts_firewall_clis}

{: tsSymptoms}
Quando você executa os comandos `ibmcloud`, `kubectl` ou `calicoctl` na CLI, eles falham.

{: tsCauses}
Você pode ter políticas de rede corporativa que impedem o acesso de seu sistema local a terminais públicos por proxies ou firewalls.

{: tsResolve}
[Permita acesso TCP para os comandos da CLI funcionarem](/docs/containers?topic=containers-firewall#firewall_bx). Essa tarefa requer a [função da plataforma **Administrador** do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o cluster.


## Não é possível acessar recursos em meu cluster
{: #cs_firewall}

{: tsSymptoms}
Quando os nós do trabalhador em seu cluster não puderem se comunicar na rede privada, você poderá ver vários sintomas diferentes.

- Mensagem de erro de amostra quando você executa `kubectl exec`, `attach`, `logs`, `proxy` ou `port-forward`:
  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

- A mensagem de erro de amostra quando `kubectl proxy` é bem-sucedido, mas o painel do Kubernetes não está disponível:
  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}

- A mensagem de erro de amostra quando `kubectl proxy` falha ou a conexão com seu serviço falha:
  ```
  Conexão recusada
  ```
  {: screen}

  ```
  Conexão expirada
  ```
  {: screen}

  ```
  Não é possível se conectar ao servidor: net/http: tempo limite do handshake do TLS
  ```
  {: screen}


{: tsCauses}
Para acessar recursos no cluster, os nós do trabalhador devem ser capazes de se comunicar na rede privada. Você pode ter um Vyatta ou outro firewall configurado ou customizar suas configurações de firewall existentes em sua conta de infraestrutura do IBM Cloud (SoftLayer). O {{site.data.keyword.containerlong_notm}} requer que determinados endereços IP e portas sejam abertos para permitir a comunicação do nó do trabalhador com o mestre do Kubernetes e vice-versa. Se os nós do trabalhador forem difundidos entre múltiplas zonas, você deverá permitir a comunicação de rede privada ativando a ampliação de VLAN. A comunicação entre os nós do trabalhador também pode não ser possível se os nós do trabalhador estiverem presos em um loop de recarregamento.

{: tsResolve}
1. Liste os nós do trabalhador em seu cluster e verifique se os nós do trabalhador não estão presos em um estado de `Recarregando`.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_id>
   ```
   {: pre}

2. Se você tiver um cluster com múltiplas zonas e sua conta não estiver ativada para VRF, verifique se você [ativou a ampliação de VLAN](/docs/containers?topic=containers-subnets#subnet-routing) para a sua conta.
3. Se você tiver uma Vyatta ou configurações de firewall customizadas, certifique-se de ter [aberto as portas necessárias](/docs/containers?topic=containers-firewall#firewall_outbound) para permitir que o cluster acesse os recursos e serviços de infraestrutura.

<br />



## Não é possível visualizar ou trabalhar com um cluster
{: #cs_cluster_access}

{: tsSymptoms}
* Você não consegue localizar um cluster. Quando você executa `ibmcloud ks clusters`, o cluster não é listado na saída.
* Você não consegue trabalhar com um cluster. Quando você executa `ibmcloud ks cluster-config` ou outros comandos específicos do cluster, o cluster não é localizado.


{: tsCauses}
No {{site.data.keyword.Bluemix_notm}}, cada recurso deve estar em um grupo de recursos. Por exemplo, o cluster `mycluster` pode existir no grupo de recursos `default`. Quando o proprietário da conta fornece acesso a recursos, designando a você uma função da plataforma do {{site.data.keyword.Bluemix_notm}} IAM, o acesso pode ser a um recurso específico ou ao grupo de recursos. Quando você recebe acesso a um recurso específico, não é necessário acesso ao grupo de recursos. Nesse caso, você não precisa destinar um grupo de recursos para trabalhar com os clusters aos quais tem acesso. Se você destinar um grupo de recursos diferente do grupo no qual o cluster está, as ações com relação a esse cluster poderão falhar. Por outro lado, quando você recebe acesso a um recurso como parte de seu acesso a um grupo de recursos, deve-se destinar um grupo de recursos para trabalhar com um cluster nesse grupo. Se você não destinar a sua sessão da CLI para o grupo de recursos no qual o cluster está, as ações com relação a esse cluster poderão falhar.

Se não for possível localizar ou trabalhar com um cluster, você poderá estar experimentando um dos problemas a seguir:
* Você tem acesso ao cluster e ao grupo de recursos no qual o cluster está, mas a sessão da CLI não é destinada para o grupo de recursos no qual o cluster está.
* Você tem acesso ao cluster, mas não como parte do grupo de recursos no qual o cluster está. Sua sessão da CLI é destinada a esse ou outro grupo de recursos.
* Você não tem acesso ao cluster.

{: tsResolve}
Para verificar suas permissões de acesso de usuário:

1. Liste todas as suas permissões de usuário.
    ```
    ibmcloud iam user-policies <your_user_name>
    ```
    {: pre}

2. Verifique se você tem acesso ao cluster e ao grupo de recursos em que o cluster está.
    1. Procure uma política que tenha um valor de **Nome do grupo de recursos** do grupo de recursos do cluster e um valor de **Memorando** igual a `Policy applies to the resource group`. Se tiver essa política, você terá acesso ao grupo de recursos. Por exemplo, essa política indica que um usuário tem acesso ao grupo de recursos `test-rg`:
        ```
        Policy ID:   3ec2c069-fc64-4916-af9e-e6f318e2a16c
        Roles:       Viewer
        Resources:
                     Resource Group ID     50c9b81c983e438b8e42b2e8eca04065
                     Resource Group Name   test-rg
                     Memo                  Policy applies to the resource group
        ```
        {: screen}
    2. Procure uma política que tenha um valor de **Nome do grupo de recursos** do grupo de recursos do cluster, um valor de **Nome do serviço** igual a `containers-kubernetes` ou nenhum valor e um valor de **Memorando** igual a `Policy applies to the resource(s) within the resource group`. Se tiver essa política, você terá acesso a clusters ou a todos os recursos dentro do grupo de recursos. Por exemplo, essa política indica que um usuário tem acesso a clusters no grupo de recursos `test-rg`:
        ```
        Policy ID:   e0ad889d-56ba-416c-89ae-a03f3cd8eeea
        Roles:       Administrator
        Resources:
                     Resource Group ID     a8a12accd63b437bbd6d58fb6a462ca7
                     Resource Group Name   test-rg
                     Service Name          containers-kubernetes
                     Service Instance
                     Region
                     Resource Type
                     Resource
                     Memo                  Policy applies to the resource(s) within the resource group
        ```
        {: screen}
    3. Se você tiver ambas as políticas, vá para a Etapa 4, primeiro marcador. Se você não tiver a política da Etapa 2a, mas tiver a política da Etapa 2b, vá para a Etapa 4, segundo marcador. Se não tiver nenhuma dessas políticas, continue com a Etapa 3.

3. Verifique se você tem acesso ao cluster, mas não como parte de acesso ao grupo de recursos em que o cluster está.
    1. Procure uma política que não tenha valores além dos campos **ID de política** e **Funções**. Se tiver essa política, você terá acesso ao cluster como parte do acesso à conta inteira. Por exemplo, essa política indica que um usuário tem acesso a todos os recursos na conta:
        ```
        Policy ID:   8898bdfd-d520-49a7-85f8-c0d382c4934e
        Roles:       Administrator, Manager
        Resources:
                     Service Name
                     Service Instance
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    2. Procure uma política que possua um valor de **Nome do serviço** igual a `containers-kubernetes` e um valor de **Instância de serviço** do ID do cluster. É possível localizar um ID de cluster executando `ibmcloud ks cluster-get --cluster <cluster_name>`. Por exemplo, essa política indica que um usuário tem acesso a um cluster específico:
        ```
        Policy ID:   140555ce-93ac-4fb2-b15d-6ad726795d90
        Roles:       Administrator
        Resources:
                     Service Name       containers-kubernetes
                     Service Instance   df253b6025d64944ab99ed63bb4567b6
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    3. Se você tiver qualquer uma dessas políticas, vá para o segundo ponto de marcador da etapa 4. Se não tiver nenhuma dessas políticas, vá para o terceiro ponto de marcador da etapa 4.

4. Dependendo de suas políticas de acesso, escolha uma das opções a seguir.
    * Se você tiver acesso ao cluster e ao grupo de recursos em que o cluster está:
      1. Destine o grupo de recursos. **Nota**: não é possível trabalhar com clusters em outros grupos de recursos até que você cancele a destinação desse grupo de recursos.
          ```
          ibmcloud target -g <resource_group>
          ```
          {: pre}

      2. Destine o cluster.
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

    * Se você tiver acesso ao cluster, mas não ao grupo de recursos em que o cluster está:
      1. Não destinar um grupo de recursos. Se você já tiver destinado um grupo de recursos, remova seu destino:
        ```
        ibmcloud target --unset-resource-group
        ```
        {: pre}

      2. Destine o cluster.
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

    * Se você não tiver acesso ao cluster:
        1. Peça ao proprietário da conta para designar uma [{{site.data.keyword.Bluemix_notm}}função da plataforma IAM](/docs/containers?topic=containers-users#platform) a você para esse cluster.
        2. Não destinar um grupo de recursos. Se você já tiver destinado um grupo de recursos, remova seu destino:
          ```
          ibmcloud target --unset-resource-group
          ```
          {: pre}
        3. Destine o cluster.
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

<br />


## O acesso a seu nó do trabalhador com SSH falha
{: #cs_ssh_worker}

{: tsSymptoms}
Não é possível acessar seu nó do trabalhador usando uma conexão SSH.

{: tsCauses}
O SSH por senha está indisponível nos nós do trabalhador.

{: tsResolve}
Use um [`DaemonSet` do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) para ações que devem ser executadas em cada nó ou use tarefas para ações únicas que devem ser executadas.

<br />


## O ID da instância bare metal está inconsistente com os registros do trabalhador
{: #bm_machine_id}

{: tsSymptoms}
Ao usar os comandos `ibmcloud ks workerr ` com o nó do trabalhador bare metal, você vê uma mensagem semelhante à seguinte.

```
ID da instância inconsistente com registros do trabalhador
```
{: screen}

{: tsCauses}
O ID da máquina pode se tornar inconsistente com o registro do trabalhador do {{site.data.keyword.containerlong_notm}} quando a máquina tem problemas de hardware. Quando a infraestrutura do IBM Cloud (SoftLayer) resolve esse problema, um componente pode mudar dentro do sistema que o serviço não identifica.

{: tsResolve}
Para o {{site.data.keyword.containerlong_notm}} reidentificar a máquina, [recarregue o nó do trabalhador bare metal](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload). **Nota**: o recarregamento também atualiza a [versão de correção](/docs/containers?topic=containers-changelog) da máquina.

Também é possível [excluir o nó do trabalhador bare metal](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm). **Nota**: as instâncias bare metal são faturadas mensalmente.

<br />


## Não é possível modificar ou excluir a infraestrutura em um cluster órfão
{: #orphaned}

{: tsSymptoms}
Não é possível executar comandos relacionados à infraestrutura em seu cluster, tais como:
* Incluindo ou removendo nós do trabalhador
* Recarregando ou reinicializando nós do trabalhador
* Redimensionando conjuntos de trabalhadores
* Atualizando seu cluster

Não é possível visualizar os nós do trabalhador do cluster em sua conta de infraestrutura do IBM Cloud (SoftLayer). No entanto, é possível atualizar e gerenciar outros clusters na conta.

Além disso, você verificou que tem as [credenciais de infraestrutura adequadas](#cs_credentials).

{: tsCauses}
O cluster pode ser provisionado em uma conta de infraestrutura do IBM Cloud (SoftLayer) que não está mais vinculada à sua conta do {{site.data.keyword.containerlong_notm}}. O cluster é órfão. Como os recursos estão em uma conta diferente, você não tem as credenciais de infraestrutura para modificar os recursos.

Considere o cenário a seguir para entender como os clusters podem se tornar órfãos.
1.  Você tem uma conta pré-paga do {{site.data.keyword.Bluemix_notm}}.
2.  Você cria um cluster chamado `Cluster1`. Os nós do trabalhador e outros recursos de infraestrutura são provisionados na conta de infraestrutura fornecida com sua conta Pré-paga.
3.  Posteriormente, você descobre que sua equipe usa uma conta de infraestrutura do IBM Cloud (SoftLayer) anterior ou compartilhada. Você usa o comando `ibmcloud ks credential-set` para mudar as credenciais de infraestrutura do IBM Cloud (SoftLayer) para usar a sua conta de equipe.
4.  Você cria um outro cluster chamado  ` Cluster2 `. Os nós do trabalhador e outros recursos de infraestrutura são provisionados na conta de infraestrutura da equipe.
5.  Você observa que o `Cluster1` precisa de uma atualização do nó do trabalhador, um recarregamento do nó do trabalhador ou você deseja apenas limpar excluindo-o. No entanto, como o `Cluster1` foi provisionado em uma conta de infraestrutura diferente, não é possível modificar seus recursos de infraestrutura. ` Cluster1 `  é órfão.
6.  Você segue as etapas de resolução na seção a seguir, mas não configura as credenciais de infraestrutura de volta para sua conta de equipe. É possível excluir `Cluster1`, mas agora `Cluster2` está órfão.
7.  Você muda suas credenciais de infraestrutura de volta para a conta de equipe que criou o `Cluster2`. Agora, você não tem mais um cluster órfão!

<br>

{: tsResolve}
1.  Verifique qual conta de infraestrutura a região em que seu cluster está usa atualmente para provisionar clusters.
    1.  Efetue login no [console do cluster do {{site.data.keyword.containerlong_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/kubernetes/clusters).
    2.  A partir da tabela, selecione seu cluster.
    3.  Na guia **Visão geral**, verifique um campo **Usuário de infraestrutura**. Esse campo ajuda a determinar se a sua conta do {{site.data.keyword.containerlong_notm}} usa uma conta de infraestrutura diferente do padrão.
        * Se não vir o campo **Usuário de infraestrutura**, você terá uma conta Pré-paga vinculada que usa as mesmas credenciais para suas contas de infraestrutura e de plataforma. O cluster que não pode ser modificado pode ser provisionado em uma conta de infraestrutura diferente.
        * Se você vir um campo **Usuário de infraestrutura**, use uma conta de infraestrutura diferente daquela que veio com a sua conta Pré-paga. Essas credenciais diferentes se aplicam a todos os clusters dentro da região. O cluster que não pode ser modificado pode ser provisionado em sua conta Pré-paga ou em uma conta de infraestrutura diferente.
2.  Verifique qual conta de infraestrutura foi usada para provisionar o cluster.
    1.  Na guia **Nós do trabalhador**, selecione um nó do trabalhador e anote seu **ID**.
    2.  Abra o menu ![Ícone do menu](../icons/icon_hamburger.svg "Ícone do menu") e clique em **Infraestrutura clássica**.
    3.  Na área de janela de navegação de infraestrutura, clique em **Dispositivos > Lista de dispositivos**.
    4.  Procure o ID do nó do trabalhador que você anotou anteriormente.
    5.  Se você não localizar o ID do nó do trabalhador, o nó do trabalhador não será provisionado para essa conta de infraestrutura. Alterne para uma conta de infraestrutura diferente e tente novamente.
3.  'Use o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) `ibmcloud ks credential-set` para mudar suas credenciais de infraestrutura para a conta na qual os nós do trabalhador do cluster são provisionados, que você localizou na etapa anterior.
    Se você não tiver mais acesso e não puder obter as credenciais de infraestrutura, deverá abrir um caso de suporte do {{site.data.keyword.Bluemix_notm}} para remover o cluster órfão.
    {: note}
4.  [ Exclua o cluster ](/docs/containers?topic=containers-remove).
5.  Se desejar, reconfigure as credenciais de infraestrutura para a conta anterior. Observe que, se você criou clusters com uma conta de infraestrutura diferente da conta para a qual está alternando, poderá deixar órfãos esses clusters.
    * Para configurar credenciais para uma conta de infraestrutura diferente, use o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) `ibmcloud ks credential-set`.
    * Para usar as credenciais padrão que vêm com a sua conta de Pagamento por Uso do {{site.data.keyword.Bluemix_notm}}, use o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset) `ibmcloud ks credential-unset --region <region>`.

<br />


## Comandos de ` kubectl `  de tempo limite
{: #exec_logs_fail}

{: tsSymptoms}
Se executar comandos como `kubectl exec`, `kubectl attach`, `kubectl proxy`, `kubectl port-forward` ou `kubectl logs`, você verá a mensagem a seguir.

  ```
  WorkerIP> <: 10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
A conexão OpenVPN entre o nó principal e os nós do trabalhador não está funcionando corretamente.

{: tsResolve}
1. Se você tem múltiplas VLANs para um cluster, múltiplas sub-redes na mesma VLAN ou um cluster de múltiplas zonas, deve-se ativar um [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para sua conta de infraestrutura do IBM Cloud (SoftLayer) para que seus nós do trabalhador possam se comunicar entre si na rede privada. Para ativar o VRF, [entre em contato com o representante de conta da infraestrutura do IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Se não for possível ou você não desejar ativar o VRF, ative o [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para executar essa ação, você precisa da [permissão de infraestrutura](/docs/containers?topic=containers-users#infra_access) **Rede > Gerenciar a rede VLAN Spanning** ou é possível solicitar ao proprietário da conta para ativá-la. Para verificar se o VLAN Spanning já está ativado, use o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region<region>`.
2. Reinicie o pod cliente OpenVPN.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. Se você ainda vê a mesma mensagem de erro, então o nó do trabalhador em que o pod de VPN está pode não estar funcional. Para reiniciar o pod de VPN e reagendá-lo para um nó do trabalhador diferente, [bloqueie, drene e reinicialize o nó do trabalhador](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) em que o pod de VPN está.

<br />


## Ligar um serviço a um cluster resulta no erro de mesmo nome
{: #cs_duplicate_services}

{: tsSymptoms}
Ao executar `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`, você vê a mensagem a seguir.

```
Múltiplos serviços com o mesmo nome foram localizados.
Execute 'ibmcloud service list' para visualizar as instâncias de serviço disponíveis do Bluemix...
```
{: screen}

{: tsCauses}
Múltiplas instâncias de serviço podem ter o mesmo nome em regiões diferentes.

{: tsResolve}
Use o GUID de serviço em vez do nome da instância de serviço no comando `ibmcloud ks cluster-service-bind`.

1. [Efetue login na região do {{site.data.keyword.Bluemix_notm}} que inclui a instância de serviço a ser ligada.](/docs/containers?topic=containers-regions-and-zones#bluemix_regions)

2. Obtenha o GUID para a instância de serviço.
  ```
  ibmcloud service show < service_instance_name> -- guid
  ```
  {: pre}

  Saída:
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. Ligue o serviço ao cluster novamente.
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_GUID>
  ```
  {: pre}

<br />


## Ligar um serviço a um cluster resulta no erro de serviço não localizado
{: #cs_not_found_services}

{: tsSymptoms}
Ao executar `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`, você vê a mensagem a seguir.

```
Ligando o serviço a um namespace...
COM FALHA

O serviço IBM Cloud especificado não pôde ser localizado. Se você acabou de criar o serviço, espere um pouco e, em seguida, tente ligá-lo novamente. Para visualizar as instâncias de serviço disponíveis do IBM Cloud, execute 'ibmcloud service list'. (E0023)
```
{: screen}

{: tsCauses}
Para ligar serviços a um cluster, deve-se ter a função de usuário de desenvolvedor do Cloud Foundry para o espaço no qual a instância de serviço é provisionada. Além disso, deve-se ter o acesso da plataforma do {{site.data.keyword.Bluemix_notm}} IAM Editor ao {{site.data.keyword.containerlong}}. Para acessar a instância de serviço, deve-se ter efetuado login no espaço no qual a instância de serviço é provisionada.

{: tsResolve}

**Como o usuário:**

1. Efetue login no {{site.data.keyword.Bluemix_notm}}.
   ```
   ibmcloud login
   ```
   {: pre}

2. Destine a organização e o espaço nos quais a instância de serviço é provisionada.
   ```
   ibmcloud target -o <org> -s <space>
   ```
   {: pre}

3. Verifique se você está no espaço certo listando suas instâncias de serviço.
   ```
   Lista de serviços ibmcloud
   ```
   {: pre}

4. Tente ligar o serviço novamente. Se obtiver o mesmo erro, entre em contato com o administrador de conta e verifique se você tem permissões suficientes para ligar serviços (veja as etapas de administrador de conta).

**Como o administrador de conta:**

1. Verifique se o usuário que experiencia esse problema tem [permissões de Editor para o {{site.data.keyword.containerlong}}](/docs/iam?topic=iam-iammanidaccser#edit_existing).

2. Verifique se o usuário que experiencia esse problema tem a [função de desenvolvedor do Cloud Foundry para o espaço](/docs/iam?topic=iam-mngcf#update_cf_access) no qual o serviço é provisionado.

3. Se as permissões corretas existirem, tente designar uma permissão diferente e, em seguida, redesignar a permissão necessária.

4. Aguarde alguns minutos, em seguida, permita que o usuário tente ligar o serviço novamente.

5. Se isso não resolver o problema, as permissões do {{site.data.keyword.Bluemix_notm}} IAM estão fora de sincronização e não é possível resolver o problema sozinho. [Entre em contato com o suporte IBM](/docs/get-support?topic=get-support-getting-customer-support) abrindo um caso de suporte. Certifique-se de fornecer o ID do cluster, o ID do usuário e o ID da instância de serviço.
   1. Recupere o ID do cluster.
      ```
      ibmcloud ks clusters
      ```
      {: pre}

   2. Recupere o ID da instância de serviço.
      ```
      ibmcloud service show < service_name> -- guid
      ```
      {: pre}


<br />


## Ligar um serviço a um cluster resulta em serviço que não suporta erros de chaves de serviço
{: #cs_service_keys}

{: tsSymptoms}
Ao executar `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`, você vê a mensagem a seguir.

```
Esse serviço não suporta a criação de chaves
```
{: screen}

{: tsCauses}
Alguns serviços no {{site.data.keyword.Bluemix_notm}}, como o {{site.data.keyword.keymanagementservicelong}}, não suportam a criação de credenciais de serviço, também referidas como chaves de serviço. Sem o suporte de chaves de serviço, o serviço não é vinculável a um cluster. Para localizar uma lista de serviços que suportam a criação de chaves de serviço, consulte [Ativando apps externos para usar os serviços do {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).

{: tsResolve}
Para integrar serviços que não suportam chaves de serviço, verifique se o serviço fornece uma API que pode ser usada para acessar o serviço diretamente de seu app. Por exemplo, se você desejar usar {{site.data.keyword.keymanagementservicelong}}, consulte a [Referência de API ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/kms?language=curl).

<br />


## Após um nó do trabalhador ser atualizado ou recarregado, nós e pods duplicados aparecem
{: #cs_duplicate_nodes}

{: tsSymptoms}
Ao executar `kubectl get nodes`, você vê nós do trabalhador duplicados com o status **`NotReady`**. Os nós do trabalhador com **`NotReady`** têm endereços IP públicos, enquanto os nós do trabalhador com **`Ready`** possuem endereços IP privados.

{: tsCauses}
Os clusters mais antigos listam os nós do trabalhador pelo endereço IP público do cluster. Agora, os nós do trabalhador são listados pelo endereço IP privado do cluster. Ao recarregar ou atualizar um nó, o endereço IP é mudado, mas a referência ao endereço IP público permanece.

{: tsResolve}
O serviço não é interrompido devido a essas duplicatas, mas é possível remover as referências do nó do trabalhador antigo do servidor de API.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## Acessar um pod em um novo nó do trabalhador falha com um tempo limite
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
Você excluiu um nó do trabalhador no cluster e, em seguida, incluiu um nó do trabalhador. Ao implementar um pod ou um serviço do Kubernetes, o recurso não pode acessar o nó do trabalhador recém-criado e os tempos de conexão se esgotam.

{: tsCauses}
Se você exclui um nó do trabalhador de seu cluster e, em seguida, inclui um nó do trabalhador, o novo nó do trabalhador pode ser designado a endereço IP privado do nó do trabalhador excluído. O Calico usa esse endereço IP privado como uma tag e continua tentando acessar o nó excluído.

{: tsResolve}
Atualize manualmente a referência do endereço IP privado para apontar para o nó correto.

1.  Confirme que você tem dois nós do trabalhador com o mesmo endereço **IP privado**. Observe o **IP privado** e o **ID** do trabalhador excluído.

  ```
  ibmcloud ks workers --cluster <cluster_name_or_id>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Zone   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       normal    Ready    dal10      1.13.6
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       deleted    -       dal10      1.13.6
  ```
  {: screen}

2.  Instale a [CLI do Calico](/docs/containers?topic=containers-network_policies#adding_network_policies).
3.  Liste os nós do trabalhador disponíveis no Calico. Substitua <path_to_file> pelo caminho local para o arquivo de configuração do Calico.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Exclua o nó do trabalhador duplicado no Calico. Substitua NODE_ID pelo ID do nó do trabalhador.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  Reinicialize o nó do trabalhador que não foi excluído.

  ```
  ibmcloud ks worker-reboot --cluster <cluster_name_or_id> --worker <worker_id>
  ```
  {: pre}


O nó excluído não é mais listado no Calico.

<br />




## Os pods falham ao serem implementados por causa de uma política de segurança de pod
{: #cs_psp}

{: tsSymptoms}
Depois de criar um pod ou executar `kubectl get events` para verificar uma implementação de pod, você vê uma mensagem de erro semelhante à seguinte.

```
unable to validate against any pod security policy
```
{: screen}

{: tsCauses}
[O controlador de admissão `PodSecurityPolicy`](/docs/containers?topic=containers-psp) verifica a autorização do usuário ou da conta de serviço, como uma implementação ou um tiller do Helm, que tentou criar o pod. Se nenhuma política de segurança de pod suportar a conta de usuário ou de serviço, o controlador de admissão `PodSecurityPolicy` evitará que os pods sejam criados.

Se você excluiu um dos recursos de política de segurança de pod para o [gerenciamento de cluster do {{site.data.keyword.IBM_notm}}](/docs/containers?topic=containers-psp#ibm_psp), poderá ter problemas semelhantes.

{: tsResolve}
Certifique-se de que a conta de usuário ou de serviço esteja autorizada por uma política de segurança de pod. Pode ser necessário [modificar uma política existente](/docs/containers?topic=containers-psp#customize_psp).

Se você excluiu um recurso de gerenciamento de cluster do {{site.data.keyword.IBM_notm}}, atualize o mestre do Kubernetes para restaurá-lo.

1.  [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2.  Atualize o mestre do Kubernetes para restaurá-lo.

    ```
    ibmcloud ks apiserver-refresh
    ```
    {: pre}


<br />




## O cluster permanece em um estado pendente
{: #cs_cluster_pending}

{: tsSymptoms}
Quando você implementa o seu cluster, ele permanece em um estado pendente e não é iniciado.

{: tsCauses}
Se você acabou de criar o cluster, os nós do trabalhador podem ainda estar sendo configurados. Se já espera por um tempo, você pode ter uma VLAN inválida.

{: tsResolve}

É possível tentar uma das soluções a seguir:
  - Verifique o status de seu cluster executando `ibmcloud ks clusters`. Em seguida, certifique-se de que seus nós do trabalhador estejam implementados executando `ibmcloud ks workers --cluster <cluster_name>`.
  - Verifique se a sua VLAN é válida. Para ser válida, uma VLAN deve ser associada à infraestrutura que pode hospedar um trabalhador com armazenamento em disco local. É possível [listar suas VLANs](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans) executando `ibmcloud ks vlans --zone <zone>` e, caso a VLAN não apareça na lista, isso indica que ela não é válida. Escolha uma VLAN diferente.

<br />


## O erro de criação do cluster não pode fazer pull de imagens do registro
{: #ts_image_pull_create}

{: tsSymptoms}
Quando criou um cluster, você recebeu uma mensagem de erro semelhante à seguinte.


```
Seu cluster não pode fazer pull de imagens dos domínios do IBM Cloud Container Registry 'icr.io' porque uma política de acesso do IAM não pôde ser criada. Certifique-se de que você tenha a função da plataforma Administrador do IAM para o IBM Cloud Container Registry. Em seguida, crie um segredo de pull de imagem com credenciais do IAM para o registro executando 'ibmcloud ks cluster-pull-secret-apply'.
```
{: screen}

{: tsCauses}
Durante a criação do cluster, um ID de serviço é criado para seu cluster e designado à política de acesso de serviço de **Leitor** para o {{site.data.keyword.registrylong_notm}}. Em seguida, uma chave de API para esse ID de serviço é gerada e armazenada em [um segredo de pull de imagem](/docs/containers?topic=containers-images#cluster_registry_auth) para autorizar o cluster a fazer pull de imagens do {{site.data.keyword.registrylong_notm}}.

Para designar com êxito a política de acesso de serviço de **Leitor** ao ID de serviço durante a criação do cluster, deve-se ter a política de acesso da plataforma **Administrador** para o {{site.data.keyword.registrylong_notm}}.

{: tsResolve}

Etapas:
1.  Certifique-se de que o proprietário da conta forneça a você a função **Administrador** para o {{site.data.keyword.registrylong_notm}}.
    ```
    ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
    ```
    {: pre}
2.  [Use o comando `ibmcloud ks cluster-pull-secret-apply`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply) para recriar um segredo de pull de imagem com as credenciais de registro apropriadas.

<br />


## Falha ao fazer pull de imagem do registro com `ImagePullBackOff` ou erros de autorização
{: #ts_image_pull}

{: tsSymptoms}

Quando você implementa uma carga de trabalho que extrai uma imagem do {{site.data.keyword.registrylong_notm}}, seus pods falham com um status **`ImagePullBackOff`**.

```
kubectl get pods
```
{: pre}

```
NAME         READY     STATUS             RESTARTS   AGE
<pod_name>   0/1       ImagePullBackOff   0          2m
```
{: screen}

Quando você descreve o pod, você vê erros de autenticação semelhantes ao seguinte.

```
kubectl describe pod <pod_name>
```
{: pre}

```
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... 401 Desautorizado
```
{: screen}

```
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... 401 Desautorizado
```
{: screen}

{: tsCauses}
Seu cluster usa uma chave de API ou token que é armazenado em um [segredo de extração de imagem](/docs/containers?topic=containers-images#cluster_registry_auth) para autorizar o cluster a extrair imagens do {{site.data.keyword.registrylong_notm}}. Por padrão, novos clusters possuem segredos de pull de imagem que usam chaves de API para que o cluster possa fazer pull de imagens de qualquer registro regional `icr.io` para contêineres implementados no namespace do Kubernetes `default`. Se o cluster tiver um segredo de pull de imagem que usa um token, o acesso padrão ao {{site.data.keyword.registrylong_notm}} será restringido ainda mais a apenas determinados registros regionais que usam os domínios `<region>.registry.bluemix.net` descontinuados.

{: tsResolve}

1.  Verifique se você usa o nome correto e a tag da imagem em seu arquivo YAML de implementação.
    ```
    imagens ibmcloud cr
    ```
    {: pre}
2.  Obtenha o arquivo de configuração de pod de um pod com falha, e procure a seção `imagePullSecrets`.
    ```
    kubectl get pod < pod_name> -o yaml
    ```
    {: pre}

    Saída de exemplo:
    ```
    ...
    imagePullSecrets:
    - name: bluemix-default-secret
    - name: bluemix-default-secret-regional
    - nome: bluemix-default-secret-international
    - name: default-us-icr-io
    - name: default-uk-icr-io
    - name: default-de-icr-io
    - name: default-au-icr-io
    - name: default-jp-icr-io
    - nome: default-icr-io
    ...
    ```
    {: screen}
3.  Se nenhuma imagem de extração de imagem estiver listada, configure o segredo de extração da imagem em seu namespace.
    1.  [Copie os segredos de extração de imagem do namespace `default` do Kubernetes para o namespace no qual você deseja implementar sua carga de trabalho](/docs/containers?topic=containers-images#copy_imagePullSecret).
    2.  [Inclua o segredo de extração de imagem na conta de serviço para esse namespace do Kubernetes](/docs/containers?topic=containers-images#store_imagePullSecret) para que todos os pods no namespace possam usar as credenciais secretas de extração de imagem.
4.  Se os segredos de extração de imagem forem listados, determine qual tipo de credenciais você usa para acessar o registro do contêiner.
    *   **Descontinuado**: se o segredo tiver `bluemix` no nome, você usará um token de registro para autenticar com os nomes de domínio `registry.<region>.bluemix.net` descontinuados. Continue com [Resolução de problemas de segredos de extração de imagem que usam tokens](#ts_image_pull_token).
    *   Se o segredo tiver `icr` no nome, você usará uma chave de API para autenticar com os nomes de domínio `icr.io`. Continue com [Resolução de problemas de segredos de extração de imagem que usam chaves de API](#ts_image_pull_apikey).
    *   Se você tiver os tipos de segredos, então, use os métodos de autenticação. Avançando, use os nomes de domínio `icr.io` em seus YAMLs de implementação para a imagem de contêiner. Continue com [Resolução de problemas de segredos de extração de imagem que usam chaves de API](#ts_image_pull_apikey).

<br>
<br>

**Resolução de problemas de segredos de pull de imagem que usam chaves de API**</br>
{: #ts_image_pull_apikey}

Se a configuração do pod tiver um segredo de extração de imagem que usa uma chave de API, verifique se as credenciais da chave de API estão configuradas corretamente.
{: shortdesc}

As etapas a seguir assumem que a chave de API armazena as credenciais de um ID de serviço. Se você configurar seu segredo de extração de imagem para usar uma chave de API de um usuário individual, deverá verificar as permissões e as credenciais do usuário do {{site.data.keyword.Bluemix_notm}} IAM.
{: note}

1.  Localize o ID de serviço que a chave de API usa para o segredo de extração da imagem, revisando a **Descrição**. O ID de serviço criado com o cluster indica `ID for <cluster_name>` e é usado no namespace do Kubernetes `default`. Se você criou outro ID de serviço, como para acessar um namespace do Kubernetes diferente ou para modificar as permissões do {{site.data.keyword.Bluemix_notm}} IAM, você customizou a descrição.
    ```
    ibmcloud iam service-ids
    ```
    {: pre}

    Saída de exemplo:
    ```
    UUID                Name               Created At              Last Updated            Description                                                                                                                                                                                         Locked     
    ServiceId-aa11...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   ID for <cluster_name>                                                                                                                                         false   
    ServiceId-bb22...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>                                                                                                                                         false    
    ```
    {: screen}
2.  Verifique se o ID do serviço tem pelo menos uma [política de função de acesso de serviço de **Leitor** do {{site.data.keyword.Bluemix_notm}} IAM atribuída a ele no {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-user#create). Se o ID do serviço não tiver a função de serviço de **Leitor**, [edite as políticas do IAM](/docs/iam?topic=iam-serviceidpolicy#access_edit). Se as políticas estiverem corretas, continue com a próxima etapa para ver se as credenciais são válidas.
    ```
    ibmcloud iam service-policies <service_ID_name>
    ```
    {: pre}

    Saída de exemplo:
    ```              
    Policy ID:   a111a111-b22b-333c-d4dd-e555555555e5   
    Roles:       Reader   
    Recursos:                            
                  Service Name       container-registry      
                  Instância de serviço         
                  Região                  
                  Resource Type      namespace      
                  Resource           <registry_namespace>  
    ```
    {: screen}  
3.  Verifique se as credenciais do segredo de extração de imagem são válidas.
    1.  Obtenha a configuração de segredo de extração de imagem. Se o pod não estiver no namespace `default`, inclua a sinalização `-n`.
        ```
        kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
        ```
        {: pre}
    2.  Na saída, copie o valor codificado em base64 do campo `.dockercfg`.
        ```
        apiVersion: v1
        kind: Secret
        data:
          .dockercfg: eyJyZWdp...==
        ...
        ```
        {: screen}
    3.  Decode a sequência base64. Por exemplo, no OS X, é possível executar o comando a seguir.
        ```
        echo -n "<base64_string>" | base64 --decode
        ```
        {: pre}

        Saída de exemplo:
        ```
        {"auths":{"<region>.icr.io":{"username":"iamapikey","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
        ```
        {: screen}
    4.  Compare o nome do domínio do registro regional de extração de imagem com o nome de domínio que você especificou na imagem do contêiner. Por padrão, os novos clusters têm segredos de extração de imagem para cada nome de domínio de registro regional para contêineres que são executados no namespace `default` do Kubernetes. No entanto, se você modificou as configurações padrão ou está usando um namespace diferente do Kubernetes, talvez você não tenha um segredo de extração de imagem para o registro regional. [Copie um segredo de extração de imagem](/docs/containers?topic=containers-images#copy_imagePullSecret) para o nome de domínio do registro regional.
    5.  Efetue login no registro por meio de sua máquina local usando o `username` e a `password` de seu segredo de extração de imagem. Se não for possível efetuar login, talvez seja necessário corrigir o ID do serviço.
        ```
        docker login -u iamapikey -p <password_string> <region>.icr.io
        ```
        {: pre}
        1.  Recrie o ID do serviço de cluster, as políticas do {{site.data.keyword.Bluemix_notm}} IAM, a chave de API e os segredos de extração de imagem para contêineres que são executados no namespace `default` do Kubernetes.
            ```
            ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
            ```
            {: pre}
        2.  Recrie a sua implementação no namespace `default` do Kubernetes. Se você ainda vir uma mensagem de erro de autorização, repita as Etapas 1-5 com a nova imagem pull de segredos. Se você ainda não puder efetuar login, [entre em contato com a equipe da IBM no Slack ou abra um caso de suporte do {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
    6.  Se o login for bem-sucedido, puxe uma imagem localmente. Se o comando falhar com um erro `access denied`, a conta de registro estará em uma conta do {{site.data.keyword.Bluemix_notm}} diferente daquela em que seu cluster está. [Crie um segredo de extração de imagem para acessar imagens na outra conta](/docs/containers?topic=containers-images#other_registry_accounts). Caso seja possível fazer pull de uma imagem para sua máquina local, sua chave de API tem as permissões corretas, mas a configuração da API em seu cluster não está correta. Não é possível resolver esse problema. [Entre em contato com a equipe da IBM no Slack ou abra um caso de suporte do {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
        ```
        docker pull <region>icr.io/<namespace>/<image>:<tag>
        ```
        {: pre}

<br>
<br>

**Descontinuado: resolução de problemas de segredos de pull de imagem que usam tokens**</br>
{: #ts_image_pull_token}

Se a configuração de pod tiver um segredo de extração de imagem que usa um token, verifique se as credenciais do token são válidas.
{: shortdesc}

Esse método de usar um token para autorizar o acesso do cluster ao {{site.data.keyword.registrylong_notm}} é suportado para os nomes de domínio `registry.bluemix.net`, mas foi descontinuado. Em vez disso, [use o método da chave de API](/docs/containers?topic=containers-images#cluster_registry_auth) para autorizar o acesso do cluster aos novos nomes de domínio de registro `icr.io`.
{: deprecated}

1.  Obtenha a configuração de segredo de extração de imagem. Se o pod não estiver no namespace `default`, inclua a sinalização `-n`.
    ```
    kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
    ```
    {: pre}
2.  Na saída, copie o valor codificado em base64 do campo `.dockercfg`.
    ```
    apiVersion: v1
    kind: Secret
    data:
      .dockercfg: eyJyZWdp...==
    ...
    ```
    {: screen}
3.  Decode a sequência base64. Por exemplo, no OS X, é possível executar o comando a seguir.
    ```
    echo -n "<base64_string>" | base64 --decode
    ```
    {: pre}

    Saída de exemplo:
    ```
    {"auths":{"registry.<region>.bluemix.net":{"username":"token","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
    ```
    {: screen}
4.  Compare o nome de domínio do registro com o nome de domínio que você especificou na imagem de contêiner. Por exemplo, se o segredo de extração de imagem autorizar o acesso ao domínio `registry.ng.bluemix.net`, mas você especificou uma imagem que está armazenada em `registry.eu-de.bluemix.net`, deverá [criar um token para usar em um segredo de extração de imagem](/docs/containers?topic=containers-images#token_other_regions_accounts) para o `registry.eu-de.bluemix.net`.
5.  Efetue login no registro a partir de sua máquina local usando o `nome do usuário` e a `senha` da imagem pull secret. Se não for possível efetuar login, o token terá um problema que não pode ser resolvido. [Entre em contato com a equipe da IBM no Slack ou abra um caso de suporte do {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
    ```
    docker login -u token -p <password_string> registry.<region>.bluemix.net
    ```
    {: pre}
6.  Se o login for bem-sucedido, puxe uma imagem localmente. Se o comando falhar com um erro `access denied`, a conta de registro estará em uma conta do {{site.data.keyword.Bluemix_notm}} diferente daquela em que seu cluster está. [Crie um segredo de extração de imagem para acessar imagens na outra conta](/docs/containers?topic=containers-images#token_other_regions_accounts). Se o comando for bem-sucedido, [entre em contato com a equipe da IBM no Slack ou abra um caso de Suporte do {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
    ```
    docker pull registry.<region>.bluemix.net/<namespace>/<image>:<tag>
    ```
    {: pre}

<br />


## Os pods permanecem no estado pendente
{: #cs_pods_pending}

{: tsSymptoms}
Ao executar `kubectl get pods`, será possível ver pods que permanecem em um estado **Pendente**.

{: tsCauses}
Se você acabou de criar o cluster do Kubernetes, os nós do trabalhador podem ainda estar configurando.

Se esse cluster for um existente:
*  Você pode não ter capacidade suficiente em seu cluster para implementar o pod.
*  O pod pode ter excedido uma solicitação de recurso ou um limite.

{: tsResolve}
No {{site.data.keyword.Bluemix_notm}} IAM, essa tarefa requer a [função da plataforma **Administrador**](/docs/containers?topic=containers-users#platform) para o cluster e a função de serviço [**Gerenciador**](/docs/containers?topic=containers-users#platform) para todos os namespaces.

Se você acabou de criar o cluster do Kubernetes, execute o comando a seguir e aguarde a inicialização dos nós do trabalhador.

```
kubectl get nodes
```
{: pre}

Se esse cluster for um existente, verifique a capacidade dele.

1.  Configure o proxy com o número da porta padrão.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Abra o painel do Kubernetes.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  Verifique se há capacidade suficiente no cluster para implementar o pod.

4.  Se você não tiver capacidade suficiente em seu cluster, redimensione seu conjunto de trabalhadores para incluir mais nós.

    1.  Revise os tamanhos atuais e os tipos de máquina de seus conjuntos de trabalhadores para decidir qual deve ser redimensionado.

        ```
        ibmcloud ks worker-pools
        ```
        {: pre}

    2.  Redimensione seus conjuntos de trabalhadores para incluir mais nós em cada zona que o conjunto abrange.

        ```
        ibmcloud ks worker-pool-resize --worker-pool <worker_pool> --cluster <cluster_name_or_ID> --size-per-zone <workers_per_zone>
        ```
        {: pre}

5.  Opcional: verifique as solicitações de recurso do pod.

    1.  Confirme se os valores `resources.requests` não são maiores que a capacidade do nó do trabalhador. Por exemplo, se o pod solicitar `cpu: 4000m` ou 4 núcleos, mas o tamanho do nó do trabalhador for somente 2 núcleos, o pod não poderá ser implementado.

        ```
        kubectl get pod < pod_name> -o yaml
        ```
        {: pre}

    2.  Se a solicitação exceder a capacidade disponível, [inclua um novo conjunto de trabalhadores](/docs/containers?topic=containers-add_workers#add_pool) com os nós do trabalhador que podem cumprir a solicitação.

6.  Se os pods ainda estiverem em um estado **pendente** depois que o nó do trabalhador for totalmente implementado, revise a [documentação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) para solucionar posteriormente os problemas no estado pendente do pod.

<br />


## Os contêineres não iniciam
{: #containers_do_not_start}

{: tsSymptoms}
Os pods são implementados com êxito em clusters, mas os contêineres não iniciam.

{: tsCauses}
Os contêineres não podem ser iniciados quando a cota de registro é atingido.

{: tsResolve}
[Liberar armazenamento em {{site.data.keyword.registryshort_notm}}.](/docs/services/Registry?topic=registry-registry_quota#registry_quota_freeup)

<br />


## Os pods falham repetidamente ao serem reiniciados ou são removidos inesperadamente
{: #pods_fail}

{: tsSymptoms}
Sua pod estava funcional, mas inesperadamente é removido ou fica preso em um loop de reinicialização.

{: tsCauses}
Seus contêineres podem exceder seus limites de recurso ou seus pods podem ser substituídos por pods de prioridade mais alta.

{: tsResolve}
Para ver se um contêiner está sendo encerrado por causa de um limite de recurso:
<ol><li>Obtenha o nome de seu pod. Se você usou um rótulo, será possível incluí-lo para filtrar seus resultados.<pre class="pre"><code> kubectl get pods -- selector = 'app = wasliberty' </code></pre></li>
<li>Descreva o pod e procure a **Contagem de reinicializações**.<pre class="pre"><code>kubectl describe pod</code></pre></li>
<li>Se o pod reiniciou muitas vezes em um curto período de tempo, busque seu status. <pre class="pre"><code>kubectl get pod -o go-template={{range.status.containerStatuses}}{{"Container Name: "}}{{.name}}{{"\r\nLastState: "}}{{.lastState}}{{end}}</code></pre></li>
<li>Revise o motivo. Por exemplo, `OOM Killed` significa "falta de memória", indicando que o contêiner está travando devido a um limite de recurso.</li>
<li>Inclua capacidade em seu cluster para que os recursos possam ser preenchidos.</li></ol>

<br>

Para ver se seu pod está sendo substituído por pods de prioridade mais alta:
1.  Obtenha o nome de seu pod.

    ```
    kubectl get pods
    ```
    {: pre}

2.  Descreva seu pod YAML.

    ```
    kubectl get pod < pod_name> -o yaml
    ```
    {: pre}

3.  Verifique o campo  ` priorityClassName ` .

    1.  Se não houver nenhum valor de campo `priorityClassName`, seu pod terá a classe de prioridade `globalDefault`. Se seu administrador de cluster não configurou uma classe de prioridade `globalDefault`, o padrão será zero (0) ou a prioridade mais baixa. Qualquer pod com uma classe de prioridade mais alta pode priorizar ou remover seu pod.

    2.  Se houver um valor de campo `priorityClassName`, obtenha a classe de prioridade.

        ```
        kubectl get priorityclass < priority_class_name> -o yaml
        ```
        {: pre}

    3.  Anote o campo `value` para verificar a prioridade de seu pod.

4.  Liste as classes de prioridade existentes no cluster.

    ```
    kubectl get priorityclasses
    ```
    {: pre}

5.  Para cada classe de prioridade, obtenha o arquivo YAML e anote o campo `value`.

    ```
    kubectl get priorityclass < priority_class_name> -o yaml
    ```
    {: pre}

6.  Compare o valor da classe de prioridade de seu pod com os outros valores de classe de prioridade para ver se ele tem prioridade mais alta ou mais baixa.

7.  Repita as etapas 1 a 3 para outros pods no cluster, para verificar qual classe de prioridade eles estão usando. Se a classe de prioridade desses outros pods for maior que aquela de seu pod, seu pod não será provisionado a menos que haja recursos suficientes para ele e todos os pods com prioridade mais alta.

8.  Entre em contato com o administrador de cluster para incluir mais capacidade em seu cluster e confirmar se as classes de prioridade correta estão designadas.

<br />


## Não é possível instalar um gráfico Helm com valores de configuração atualizados
{: #cs_helm_install}

{: tsSymptoms}
Quando você tenta instalar um gráfico do Helm atualizado executando `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>`, você obtém a mensagem de erro `Error: failed to download "ibm/<chart_name>"`.

{: tsCauses}
A URL para o repositório do {{site.data.keyword.Bluemix_notm}} em sua instância do Helm pode estar incorreta.

{: tsResolve}
Para solucionar problemas de seu gráfico Helm:

1. Liste os repositórios disponíveis atualmente em sua instância do Helm.

    ```
    helm repo list
    ```
    {: pre}

2. Na saída, verifique se a URL para o repositório do {{site.data.keyword.Bluemix_notm}}, `ibm`, é `https://icr.io/helm/iks-charts`.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://icr.io/helm/iks-charts
    ```
    {: screen}

    * Se a URL estiver incorreta:

        1. Remova o repositório do {{site.data.keyword.Bluemix_notm}}.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. Inclua o repositório do {{site.data.keyword.Bluemix_notm}} novamente.

            ```
            helm repo add iks-charts https://icr.io/helm/iks-charts
            ```
            {: pre}

    * Se a URL estiver correta, obtenha as atualizações mais recentes do repositório.

        ```
        helm repo update
        ```
        {: pre}

3. Instale o gráfico Helm com suas atualizações.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

<br />


## Não é possível instalar o Helm tiller ou implementar contêineres a partir de imagens públicas em meu cluster
{: #cs_tiller_install}

{: tsSymptoms}

Quando você tenta instalar o tiller do Helm ou deseja implementar imagens de registros públicos, como o DockerHub, a instalação falha com um erro semelhante ao seguinte:

```
Failed to pull image "gcr.io/kubernetes-helm/tiller:v2.12.0": rpc error: code = Unknown desc = failed to resolve image "gcr.io/kubernetes-helm/tiller:v2.12.0": no available registry endpoint:
```
{: screen}

{: tsCauses}
Você pode ter configurado um firewall customizado, especificado políticas customizadas do Calico ou criado um cluster somente privado usando o terminal em serviço privado que bloqueia a conectividade de rede pública para o registro de contêiner no qual a imagem é armazenada.

{: tsResolve}
- Se você tiver um firewall customizado ou configurar políticas customizadas do Calico, permita o tráfego de rede de saída e de entrada entre os nós do trabalhador e o registro de contêiner no qual a imagem é armazenada. Se a imagem estiver armazenada no {{site.data.keyword.registryshort_notm}}, revise as portas necessárias em [Permitindo que o cluster acesse recursos de infraestrutura e outros serviços](/docs/containers?topic=containers-firewall#firewall_outbound).
- Se você criou um cluster privado ativando o terminal em serviço somente privado, será possível [ativar o terminal em serviço público](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable) para seu cluster. Se desejar instalar os gráficos Helm em um cluster privado sem abrir uma conexão pública, é possível instalar o Helm [com Tiller](/docs/containers?topic=containers-helm#private_local_tiller) ou [sem Tiller](/docs/containers?topic=containers-helm#private_install_without_tiller).

<br />


## Obtendo ajuda e suporte
{: #clusters_getting_help}

Ainda está tendo problemas com o seu cluster?
{: shortdesc}

-  No terminal, você é notificado quando atualizações para a CLI `ibmcloud` e plug-ins estão disponíveis. Certifique-se de manter sua CLI atualizada para que seja possível usar todos os comandos e sinalizações disponíveis.
-   Para ver se o {{site.data.keyword.Bluemix_notm}} está disponível, [verifique a página de status do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/status?selected=status).
-   Poste uma pergunta no [{{site.data.keyword.containerlong_notm}} Slack ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-container-service.slack.com).
    Se você não estiver usando um IBMid para a sua conta do {{site.data.keyword.Bluemix_notm}}, [solicite um convite](https://bxcs-slack-invite.mybluemix.net/) para essa Folga.
    {: tip}
-   Revise os fóruns para ver se outros usuários tiveram o mesmo problema. Ao usar os fóruns para fazer uma pergunta, marque sua pergunta para que ela seja vista pelas equipes de desenvolvimento do {{site.data.keyword.Bluemix_notm}}.
    -   Se você tiver questões técnicas sobre como desenvolver ou implementar clusters ou apps com o {{site.data.keyword.containerlong_notm}}, poste sua pergunta no [Stack Overflow ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") ](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) e identifique-a com `ibm-cloud`, `kubernetes` e `containers`.
    -   Para perguntas sobre o serviço e instruções de introdução, use o fórum do [IBM Developer Answers ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Inclua as tags `ibm-cloud` e `containers`.
    Consulte
[Obtendo
ajuda](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) para obter mais detalhes sobre o uso dos fóruns.
-   Entre em contato com o Suporte IBM abrindo um caso. Para saber mais sobre como abrir um caso de suporte IBM ou sobre os níveis de suporte e as severidades do caso, consulte [Entrando em contato com o suporte](/docs/get-support?topic=get-support-getting-customer-support).
Ao relatar um problema, inclua o ID do cluster. Para obter o ID do seu cluster, execute `ibmcloud ks clusters`. É possível também usar o [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) para reunir e exportar informações pertinentes de seu cluster para compartilhar com o Suporte IBM.
{: tip}

