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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Resolução de problemas de rede do cluster
{: #cs_troubleshoot_network}

Ao usar o {{site.data.keyword.containerlong}}, considere estas técnicas para resolução de problemas de rede do cluster.
{: shortdesc}

Tendo problemas de conexão com seu app por meio do Ingress? Tente  [ Depurging Ingress ](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress).
{: tip}

Enquanto você soluciona problemas, é possível usar o [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) para executar testes e reunir informações pertinentes de rede, do Ingresso e do strongSwan por meio de seu cluster.
{: tip}

## Não é possível se conectar a um aplicativo por meio de um serviço de balanceador de carga de rede (NLB)
{: #cs_loadbalancer_fails}

{: tsSymptoms}
Você expôs publicamente seu aplicativo criando um serviço de NLB em seu cluster. Quando tentou se conectar ao seu aplicativo usando o endereço IP público do NLB, a conexão falhou ou atingiu o tempo limite.

{: tsCauses}
Seu serviço NLB pode não estar funcionando corretamente devido a um dos motivos a seguir:

-   O cluster é um cluster grátis ou um cluster padrão com somente um nó do trabalhador.
-   O cluster não está totalmente implementado ainda.
-   O script de configuração para seu serviço NLB inclui erros.

{: tsResolve}
Para solucionar problemas de seu serviço NLB:

1.  Verifique se você configurou um cluster padrão totalmente implementado que tenha pelo menos dois nós do trabalhador para garantir a alta disponibilidade para seu serviço NLB.

  ```
  ibmcloud ks workers --cluster <cluster_name_or_ID>
  ```
  {: pre}

    Na saída da CLI, certifique-se de que o **Status** dos nós do trabalhador exiba **Pronto** e que o **Tipo de máquina** mostre um tipo de máquina diferente de **livre**.

2. Para NLBs da versão 2.0: certifique-se de concluir os [pré-requisitos do NLB 2.0](/docs/containers?topic=containers-loadbalancer#ipvs_provision).

3. Verifique a exatidão do arquivo de configuração para seu serviço NLB.
    * NLBs da versão 2.0:
        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myservice
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
        spec:
          type: LoadBalancer
          selector:
            <selector_key>:<selector_value>
          ports:
           - protocol: TCP port: 8080 externalTrafficPolicy: Local
        ```
        {: screen}

        1. Verifique se você definiu **LoadBalancer** como o tipo para seu serviço.
        2. Verifique se você incluiu a anotação `service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"`.
        3. Na seção `spec.selector` do serviço LoadBalancer, certifique-se de que `<selector_key>` e `<selector_value>` sejam iguais ao par chave/valor usado na seção `spec.template.metadata.labels` de seu YAML de implementação. Se os rótulos não corresponderem, a seção **Terminais** em seu serviço LoadBalancer exibirá **<nenhum>** e seu app não ficará acessível na Internet.
        4. Verifique se usou a **porta** em que seu app atende.
        5. Verifique se você configurou `externalTrafficPolicy` para `Local`.

    * NLBs da versão 1.0:
        ```
        apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      type: LoadBalancer
      selector:
        <selector_key>:<selector_value>
      ports:
           - protocol: TCP port: 8080
        ```
        {: screen}

        1. Verifique se você definiu **LoadBalancer** como o tipo para seu serviço.
        2. Na seção `spec.selector` do serviço LoadBalancer, certifique-se de que `<selector_key>` e `<selector_value>` sejam iguais ao par chave/valor usado na seção `spec.template.metadata.labels` de seu YAML de implementação. Se os rótulos não corresponderem, a seção **Terminais** em seu serviço LoadBalancer exibirá **<nenhum>** e seu app não ficará acessível na Internet.
        3. Verifique se usou a **porta** em que seu app atende.

3.  Verifique seu serviço NLB e revise a seção **Eventos** para localizar possíveis erros.

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    Procure as mensagens de erro a seguir:

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>Para usar o serviço NLB, deve-se ter um cluster padrão com pelo menos dois nós do trabalhador.</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the NLB service request. Add a portable subnet to the cluster and try again</code></pre></br>Essa mensagem de erro indica que nenhum endereço IP público móvel ficou disponível para a alocação ao seu serviço NLB. Consulte <a href="/docs/containers?topic=containers-subnets#subnets">Incluindo sub-redes nos clusters</a> para localizar informações sobre como solicitar endereços IP públicos móveis para seu cluster. Depois que os endereços IP públicos móveis estiverem disponíveis para o cluster, o serviço NLB será criado automaticamente.</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>Você definiu um endereço IP público móvel para seu YAML do balanceador de carga usando a seção **`loadBalancerIP`**, mas ele não está disponível em sua sub-rede pública móvel. Na seção **`loadBalancerIP`** de seu script de configuração, remova o endereço IP existente e inclua um dos endereços IP públicos móveis disponíveis. Também é possível remover a seção **`loadBalancerIP`** de seu script para que um endereço IP público móvel disponível possa ser alocado automaticamente.</li>
    <li><pre class="screen"><code>No available nodes for NLB services</code></pre>Você não possui nós do trabalhador suficientes para implementar um serviço NLB. Um motivo talvez seja que você tenha implementado um cluster padrão com mais de um nó do trabalhador, mas o fornecimento dos nós do trabalhador tenha falhado.</li>
    <ol><li>Liste os nós do trabalhador disponíveis.</br><pre class="pre"><code>kubectl get nodes</code></pre></li>
    <li>Se pelo menos dois nós do trabalhador disponíveis forem localizados, liste os detalhes do nó do trabalhador.</br><pre class="pre"><code>ibmcloud ks worker-get --cluster &lt;cluster_name_or_ID&gt; --worker &lt;worker_ID&gt;</code></pre></li>
    <li>Certifique-se de que os IDs de VLAN pública e privada para os nós do trabalhador que foram retornados pela correspondência de comandos <code>kubectl get nodes</code> e <code>ibmcloud ks worker-get</code>.</li></ol></li></ul>

4.  Se você usar um domínio customizado para se conectar ao seu serviço NLB, certifique-se de que seu domínio customizado esteja mapeado para o endereço IP público de seu serviço NLB.
    1.  Localize o endereço IP público de seu serviço NLB.
        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  Verifique se seu domínio customizado está mapeado para o endereço IP público móvel de seu serviço NLB no Registro de ponteiro (PTR).

<br />


## Não é possível se conectar a um app por meio de Ingresso
{: #cs_ingress_fails}

{: tsSymptoms}
Você expôs publicamente seu app criando um recurso de Ingresso para seu app no cluster. Quando tentou se conectar ao seu app usando o endereço IP público ou subdomínio do balanceador de carga do aplicativo (ALB) de Ingresso, a conexão falhou ou atingiu o tempo limite.

{: tsResolve}
Primeiro, verifique se seu cluster está totalmente implementado e tem pelo menos 2 nós do trabalhador disponíveis por zona para assegurar alta disponibilidade para o ALB.
```
ibmcloud ks workers --cluster <cluster_name_or_ID>
```
{: pre}

Na saída da CLI, certifique-se de que o **Status** dos nós do trabalhador exiba **Pronto** e que o **Tipo de máquina** mostre um tipo de máquina diferente de **livre**.

* Se o seu cluster padrão estiver completamente implementado e tiver pelo menos 2 nós do trabalhador por zona, mas nenhum **Subdomínio do Ingress** estiver disponível, consulte [Não é possível obter um subdomínio para o ALB do Ingress](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit).
* Para outros problemas, solucione problemas de sua configuração do Ingress seguindo as etapas em [Depurando o Ingress](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress).

<br />


## Problemas secretos do balanceador de carga do aplicativo (ALB) Ingress
{: #cs_albsecret_fails}

{: tsSymptoms}
Depois de implementar um segredo do balanceador de carga do aplicativo do Ingress (ALB) para seu cluster usando o comando `ibmcloud ks alb-cert-deploy`, o campo `Descrição` não está sendo atualizado com o nome secreto quando você visualiza seu certificado no {{site.data.keyword.cloudcerts_full_notm}}.

Quando você lista informações sobre o segredo do ALB, o status indica `*_failed`. Por exemplo, `create_failed`, `update_failed`, `delete_failed`.

{: tsResolve}
Revise as razões a seguir por que o segredo do ALB pode falhar e as etapas de resolução de problemas correspondentes:

<table>
<caption>Resolução de Problemas do balanceador de carga de aplicativo de Ingresso segredos</caption>
 <thead>
 <th>Por que isso está acontecendo?</th>
 <th>Como corrigi-lo</th>
 </thead>
 <tbody>
 <tr>
 <td>Você não tem as funções de acesso necessárias para fazer download e atualizar os dados do certificado.</td>
 <td>Verifique com seu Administrador de conta para designar a você as funções do {{site.data.keyword.Bluemix_notm}} IAM a seguir:<ul><li>As funções de serviço **Gerente** e **Gravador** para sua instância do {{site.data.keyword.cloudcerts_full_notm}}. Para obter mais informações, veja <a href="/docs/services/certificate-manager?topic=certificate-manager-managing-service-access-roles#managing-service-access-roles">Gerenciando acesso de serviço</a> para {{site.data.keyword.cloudcerts_short}}.</li><li>A <a href="/docs/containers?topic=containers-users#platform">função da plataforma **Administrador**</a> para o cluster.</li></ul></td>
 </tr>
 <tr>
 <td>O CRN do certificado fornecido no tempo de criação, atualização ou remoção não pertence à mesma conta que o cluster.</td>
 <td>Verifique se o CRN do certificado fornecido é importado para uma instância do serviço {{site.data.keyword.cloudcerts_short}} que está implementado na mesma conta que seu cluster.</td>
 </tr>
 <tr>
 <td>O CRN do certificado fornecido no tempo de criação está incorreto.</td>
 <td><ol><li>Verifique a precisão da sequência CRN do certificado que você fornece.</li><li>Se o CRN do certificado for localizado como exato, tente atualizar o segredo: <code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Se esse comando resultar no status <code>update_failed</code>, remova o segredo: <code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Implemente o segredo novamente: <code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>O CRN do certificado fornecido no tempo de atualização está incorreto.</td>
 <td><ol><li>Verifique a precisão da sequência CRN do certificado que você fornece.</li><li>Se o CRN do certificado for localizado como exato, remova o segredo: <code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Implemente o segredo novamente: <code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Tente atualizar o segredo: <code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>O serviço {{site.data.keyword.cloudcerts_long_notm}} está tendo tempo de inatividade.</td>
 <td>Verifique se o serviço {{site.data.keyword.cloudcerts_short}} está funcionando.</td>
 </tr>
 <tr>
 <td>Seu segredo importado tem o mesmo nome que o segredo do Ingress fornecido pela IBM.</td>
 <td>Renomeie seu segredo. É possível verificar o nome do segredo do Ingress fornecido pela IBM executando `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress`.</td>
 </tr>
 </tbody></table>

<br />


## Não é possível obter um subdomínio para o ALB do Ingress, o ALB não é implementado em uma zona ou não é possível implementar um balanceador de carga
{: #cs_subnet_limit}

{: tsSymptoms}
* Nenhum subdomínio do Ingress: ao executar `ibmcloud ks cluster-get --cluster <cluster>`, seu cluster está em um estado `normal`, mas nenhum **Subdomínio do Ingress** está disponível.
* Um ALB não é implementado em uma zona: quando você tem um cluster de multizona e executa `ibmcloud ks albs --cluster <cluster>`, nenhum ALB é implementado em uma zona. Por exemplo, se você tiver nós do trabalhador em 3 zonas, poderá ver uma saída semelhante à seguinte na qual um ALB público não foi implementado na terceira zona.
  ```
  ALB ID                                            Enabled    Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
  private-cr96039a75fddb4ad1a09ced6699c88888-alb1   false      disabled   private   -                dal10   ingress:411/ingress-auth:315   2294021
  private-cr96039a75fddb4ad1a09ced6699c88888-alb2   false      disabled   private   -                dal12   ingress:411/ingress-auth:315   2234947
  private-cr96039a75fddb4ad1a09ced6699c88888-alb3   false      disabled   private   -                dal13   ingress:411/ingress-auth:315   2234943
  public-cr96039a75fddb4ad1a09ced6699c88888-alb1    true       enabled    public    169.xx.xxx.xxx   dal10   ingress:411/ingress-auth:315   2294019
  public-cr96039a75fddb4ad1a09ced6699c88888-alb2    true       enabled    public    169.xx.xxx.xxx   dal12   ingress:411/ingress-auth:315   2234945
  ```
  {: screen}
* Não é possível implementar um balanceador de carga: ao descrever o configmap `ibm-cloud-provider-vlan-ip-config`, você pode ver uma mensagem de erro semelhante à saída de exemplo a seguir.
  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config
  ```
  {: pre}
  ```
  Warning  CreatingLoadBalancerFailed ... ErrorSubnetLimitReached: There are already the maximum number of subnets permitted in this VLAN.
  ```
  {: screen}

{: tsCauses}
Em clusters padrão, na primeira vez que você criar um cluster em uma zona, uma VLAN pública e uma VLAN privada nessa zona serão provisionadas automaticamente para você em sua conta de infraestrutura do IBM Cloud (SoftLayer). Nessa zona, 1 sub-rede móvel pública é solicitada na VLAN pública especificada e 1 sub-rede móvel privada é solicitada na VLAN privada especificada. Para o {{site.data.keyword.containerlong_notm}}, as VLANs têm um limite de 40 sub-redes. Se a VLAN do cluster em uma zona já tiver atingido esse limite, o **Subdomínio do Ingress** falhará ao provisionar, o ALB do Ingress público para essa zona falhará ao provisionar ou você poderá não ter um endereço IP público móvel disponível para criar um balanceador de carga de rede (NLB).

Para visualizar quantas sub-redes uma VLAN tem:
1.  No [console da infraestrutura do IBM Cloud (SoftLayer)](https://cloud.ibm.com/classic?), selecione **Rede** > **IP de gerenciamento** > **VLANs**.
2.  Clique no **Número da VLAN** da VLAN usada para criar seu cluster. Revise a seção **Subnets** para ver se 40 ou mais sub-redes existem.

{: tsResolve}
Se precisar de uma nova VLAN, solicite uma [entrando em contato com o suporte do {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans). Em seguida, [crie um cluster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) que usa essa nova VLAN.

Se você tiver outra VLAN disponível, será possível [configurar a ampliação da VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) no cluster existente. Depois, será possível incluir novos nós do trabalhador no cluster que usam a outra VLAN com sub-redes disponíveis. Para verificar se o VLAN Spanning já está ativado, use o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region<region>`.

Se você não estiver usando todas as sub-redes na VLAN, será possível reutilizar sub-redes na VLAN incluindo-as em seu cluster.
1. Verifique se a sub-rede que você deseja usar está disponível.
  <p class="note">A conta de infraestrutura que você usa pode ser compartilhada em múltiplas contas do {{site.data.keyword.Bluemix_notm}}. Nesse caso, mesmo se você executar o comando `ibmcloud ks subnets` para ver sub-redes com **Clusters ligados**, será possível ver informações somente para seus clusters. Verifique com o proprietário da conta de infraestrutura para certificar-se de que as sub-redes estão disponíveis e não em uso por nenhuma outra conta ou equipe.</p>

2. Use o [comando `ibmcloud ks cluster-subnet-add`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add) para tornar uma sub-rede existente disponível para seu cluster.

3. Verifique se a sub-rede foi criada com sucesso e se foi incluída em seu cluster. O CIDR da sub-rede é listado na seção **Subnet VLANs**.
    ```
    ibmcloud ks cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

    Nesta saída de exemplo, uma segunda sub-rede foi incluída na VLAN pública `2234945`:
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR          Public   User-managed
    2234947   10.xxx.xx.xxx/29     false    false
    2234945   169.xx.xxx.xxx/29    true     false
    2234945   169.xx.xxx.xxx/29    true     false
    ```
    {: screen}

4. Verifique se os endereços IP móveis da sub-rede que você incluiu são usados para os ALBs ou balanceadores de carga em seu cluster. Pode levar vários minutos para os serviços usarem os endereços IP móveis da sub-rede recém-incluída.
  * Nenhum subdomínio do Ingress: execute `ibmcloud ks cluster-get --cluster <cluster>` para verificar se o **Subdomínio do Ingress** está preenchido.
  * Um ALB não é implementado em uma zona: execute `ibmcloud ks albs --cluster <cluster>` para verificar se o ALB ausente está implementado.
  * Não é possível implementar um balanceador de carga: execute `kubectl get svc -n kube-system` para verificar se o balanceador de carga tem um **EXTERNAL-IP**.

<br />


## A conexão via WebSocket é fechada após 60 segundos
{: #cs_ingress_websocket}

{: tsSymptoms}
Seu serviço Ingresso expõe um app que usa um WebSocket. No entanto, a conexão entre um cliente e o app WebSocket é fechada quando nenhum tráfego é enviado entre eles por 60 segundos.

{: tsCauses}
A conexão com seu app WebSocket pode cair após 60 segundos de inatividade por um dos motivos a seguir:

* A sua conexão de Internet tem um proxy ou firewall que não tolera conexões longas.
* Um tempo limite no ALB para o app WebSocket finaliza a conexão.

{: tsResolve}
Para evitar que a conexão seja fechada após 60 segundos de inatividade:

1. Se você se conectar ao seu app WebSocket por meio de um proxy ou firewall, certifique-se de que o proxy ou o firewall não esteja configurado para finalizar automaticamente as conexões longas.

2. Para manter a conexão ativa, é possível aumentar o valor do tempo limite ou configurar uma pulsação em seu app.
<dl><dt>Alterar o tempo limite</dt>
<dd>Aumente o valor do `proxy-read-timeout` em sua configuração do ALB. Por exemplo, para mudar o tempo limite de `60s` para um valor maior, como `300s`, inclua essa [anotação](/docs/containers?topic=containers-ingress_annotation#connection) em seu arquivo de recursos do Ingress: `ingress.bluemix.net/proxy-read-timeout: "serviceName=<service_name> timeout=300s"`. O tempo limite é mudado para todos os ALBs públicos em seu cluster.</dd>
<dt>Configurar uma pulsação</dt>
<dd>Se você não desejar mudar o valor de tempo limite de leitura padrão do ALB, configure uma pulsação em seu app WebSocket. Ao configurar um protocolo de pulsação usando uma estrutura como [WAMP ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://wamp-proto.org/), o servidor de envio de dados do app envia periodicamente uma mensagem "ping" em um intervalo cronometrado e o cliente responde com uma mensagem "pong". Configure o intervalo de pulsação para 58 segundos ou menos para que o tráfego "ping/pong" mantenha a conexão aberta antes que o tempo limite de 60 segundos seja cumprido.</dd></dl>

<br />


## A preservação de IP de origem falha ao usar nós contaminados
{: #cs_source_ip_fails}

{: tsSymptoms}
Você ativou a preservação de IP de origem para um [balanceador de carga da versão 1.0](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) ou um serviço [ALB do Ingress](/docs/containers?topic=containers-ingress#preserve_source_ip), mudando `externalTrafficPolicy` para `Local` no arquivo de configuração do serviço. No entanto, nenhum tráfego atinge o serviço de back-end para seu app.

{: tsCauses}
Ao ativar a preservação de IP de origem para os serviços de balanceador de carga ou ALB do Ingress, o endereço IP de origem da solicitação do cliente é preservado. O serviço encaminha o tráfego para os pods de app no mesmo nó do trabalhador somente para assegurar que o endereço IP do pacote de solicitações não seja mudado. Geralmente, os pods dos serviços de balanceador de carga ou ALB de Ingress são implementados nos mesmos nós do trabalhador nos quais os pods de app são implementados. No entanto, existem algumas situações em que os pods de serviço e os pods de app podem não ser planejados para o mesmo nó do trabalhador. Se você usar [contaminações do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) em nós do trabalhador, quaisquer pods que não tiverem uma tolerância de contaminação serão impedidos de serem executados nos nós do trabalhador contaminados. A preservação de IP de origem pode não estar funcionando com base no tipo de contaminação que você usou:

* **Contaminações do nó de borda**: você [incluiu o rótulo `dedicated=edge`](/docs/containers?topic=containers-edge#edge_nodes) em dois ou mais nós do trabalhador em cada VLAN pública em seu cluster para assegurar que os pods do Ingress e do balanceador de carga sejam implementados somente nesses nós do trabalhador. Em seguida, você também [contaminou esses nós de borda](/docs/containers?topic=containers-edge#edge_workloads) para evitar que quaisquer outras cargas de trabalho sejam executadas em nós de borda. No entanto, você não incluiu uma regra de afinidade de nó de borda e tolerância em sua implementação do app. Seus pods de app não podem ser planejados nos mesmos nós contaminados que os pods de serviço, e nenhum tráfego atinge o serviço de back-end para o seu app.

* **Contaminações customizadas**: você usou contaminações customizadas em vários nós para que somente os pods de app com essa tolerância de contaminação possam ser implementados nesses nós. Você incluiu regras de afinidade e tolerâncias para as implementações de seu app e o serviço de balanceador de carga ou Ingress para que seus pods sejam implementados somente nesses nós. No entanto, os pods `ibm-cloud-provider-ip` `keepalived` que são criados automaticamente no namespace `ibm-system` asseguram que os pods do balanceador de carga e os pods de app estejam sempre planejados para o mesmo nó do trabalhador. Esses pods `keepalived` não têm as tolerâncias para as contaminações customizadas que você usou. Eles não podem ser planejados nos mesmos nós contaminados nos quais seus pods de app estão em execução e nenhum tráfego atinge o serviço de back-end para o seu app.

{: tsResolve}
Resolva o problema escolhendo uma das opções a seguir:

* **Contaminações de nó de borda**: para assegurar que seus pods do balanceador de carga e do app sejam implementados em nós de borda contaminados, [inclua regras de afinidade e tolerâncias de nó de borda em sua implementação do app](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes). Os pods do balanceador de carga e do ALB do Ingress têm essas regras de afinidade e tolerâncias por padrão.

* **Contaminações customizadas**: remova as contaminações customizadas para as quais os pods `keepalived` não têm tolerâncias. Em vez disso, é possível [rotular nós do trabalhador como nós de borda e, em seguida, contaminar esses nós de borda](/docs/containers?topic=containers-edge).

Se você concluir uma das opções acima, mas os pods `keepalived` ainda não estiverem planejados, será possível obter mais informações sobre os pods `keepalived`:

1. Obtenha os pods  ` keepalived ` .
    ```
    kubectl get pods -n ibm-system
    ```
    {: pre}

2. Na saída, procure os pods `ibm-cloud-provider-ip` que têm um **Status** de `Pending`. Exemplo:
    ```
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t     0/1       Pending   0          2m        <none>          <none>
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-8ptvg     0/1       Pending   0          2m        <none>          <none>
    ```
    {:screen}

3. Descreva cada pod `keepalived` e procure a seção **Eventos**. Trate quaisquer mensagens de erro ou de aviso que estejam listadas.
    ```
    kubectl describe pod ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t -n ibm-system
    ```
    {: pre}

<br />


## Não é possível estabelecer a conectividade VPN com o gráfico Helm do strongSwan
{: #cs_vpn_fails}

{: tsSymptoms}
Quando você verifica a conectividade de VPN executando `kubectl exec $STRONGSWAN_POD -- ipsec status`, você não vê um status de `ESTABLISHED` ou o pod de VPN está em um estado de `ERROR` ou continua travando e reiniciando.

{: tsCauses}
Seu arquivo de configuração do gráfico Helm tem valores incorretos, valores ausentes ou erros de sintaxe.

{: tsResolve}
Quando você tentar estabelecer a conectividade VPN com o gráfico Helm do strongSwan, é provável que o status da VPN não seja `ESTABLISHED` na primeira vez. Você pode precisar verificar vários tipos de problemas e mudar seu arquivo de configuração de acordo. Para solucionar problemas de sua conectividade VPN do strongSwan:

1. [Teste e verifique a conectividade de VPN do strongSwan](/docs/containers?topic=containers-vpn#vpn_test) executando os cinco testes do Helm que estão incluídos na definição de gráfico do strongSwan.

2. Se não for possível estabelecer a conectividade de VPN depois de executar os testes do Helm, será possível executar a ferramenta de depuração de VPN que está empacotada dentro da imagem do pod VPN.

    1. Configure a variável de ambiente `STRONGSWAN_POD`.

        ```
        export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. Execute a ferramenta de depuração.

        ```
        kubectl exec  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        A ferramenta exibe várias páginas de informações conforme ela executa vários testes para problemas de rede comum. As linhas de saída que iniciam com `ERROR`, `WARNING`, `VERIFY` ou `CHECK` indicam erros possíveis com a conectividade VPN.

    <br />


## Não é possível instalar uma nova liberação do gráfico Helm do strongSwan
{: #cs_strongswan_release}

{: tsSymptoms}
Você modifica o gráfico Helm do strongSwan e tenta instalar sua nova liberação executando `helm install -f config.yaml --name=vpn ibm/strongswan`. No entanto, você vê o erro a seguir:
```
Error: release vpn failed: deployments.extensions "vpn-strongswan" already exists
```
{: screen}

{: tsCauses}
Esse erro indica que a versão anterior do gráfico do strongSwan não foi completamente desinstalada.

{: tsResolve}

1. Exclua a liberação do gráfico anterior.
    ```
    helm delete --purge vpn
    ```
    {: pre}

2. Exclua a implementação para a liberação anterior. A exclusão da implementação e do pod associado leva até 1 minuto.
    ```
    kubectl delete deploy vpn-strongswan
    ```
    {: pre}

3. Verifique se a implementação foi excluída. A implementação `vpn-strongswan` não aparece na lista.
    ```
    kubectl get deployments
    ```
    {: pre}

4. Reinstale o gráfico Helm do strongSwan atualizado com um novo nome de liberação.
    ```
    -f config.yaml helm install -- name=vpn ibm/strongswan
    ```
    {: pre}

<br />


## A conectividade VPN do strongSwan falha depois de incluir ou excluir nós do trabalhador
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
Você estabeleceu anteriormente uma conexão VPN de trabalho, usando o serviço de VPN IPSec do strongSwan. No entanto, após ter incluído ou excluído um nó do trabalhador em seu cluster, você tem um ou mais dos sintomas a seguir:

* Você não tem um status de VPN de `ESTABLISHED`
* Não é possível acessar novos nós do trabalhador por meio de sua rede no local
* Não é possível acessar a rede remota de pods que estão em execução em novos nós do trabalhador

{: tsCauses}
Se você incluiu um nó do trabalhador em um conjunto de trabalhadores:

* O nó do trabalhador foi provisionado em uma nova sub-rede privada que não é exposta sobre a conexão VPN por suas configurações `localSubnetNAT` ou `local.subnet` existentes
* as rotas de VPN não podem ser incluídas no nó do trabalhador porque o trabalhador tem contaminações ou rótulos que não estão incluídos em suas configurações `tolerations` ou `nodeSelector` existentes
* O pod de VPN está em execução no novo nó do trabalhador, mas o endereço IP público do nó do trabalhador não é permitido por meio do firewall no local

Se você tiver excluído um nó do trabalhador:

* Esse nó do trabalhador era o único nó em que um pod de VPN estava em execução, devido a restrições em certas contaminações ou rótulos em suas configurações `tolerations` ou `nodeSelector` existentes

{: tsResolve}
Atualize os valores do gráfico Helm para refletir as mudanças do nó do trabalhador:

1. Exclua o gráfico do Helm existente.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Abra o arquivo de configuração para seu serviço de VPN do strongSwan.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Verifique as configurações a seguir e mude as configurações para refletir os nós do trabalhador excluídos ou incluídos, conforme necessário.

    Se você tiver incluído um nó do trabalhador:

    <table>
    <caption>Configurações do nó do trabalhador < /caption?
     <thead>
     <th>Configuração</th>
     <th>Descrição</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>O trabalhador incluído pode ser implementado em uma sub-rede privada nova e diferente das outras sub-redes existentes em que outros nós do trabalhador estão. Se você usar a NAT de sub-rede para remapear os endereços IP locais privados do seu cluster e o trabalhador tiver sido incluído em uma nova sub-rede, inclua o novo CIDR de sub-rede nessa configuração.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Se você limitou anteriormente a implementação do pod de VPN para trabalhadores com um rótulo específico, assegure-se de que o nó do trabalhador incluído também tenha esse rótulo.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Se o nó do trabalhador incluído estiver contaminado, mude essa configuração para permitir que o pod de VPN seja executado em todos os trabalhadores contaminados com quaisquer contaminações ou contaminações específicas.</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>O trabalhador incluído pode ser implementado em uma sub-rede privada nova e diferente das sub-redes existentes em que outros trabalhadores estão. Se seus apps são expostos por serviços NodePort ou LoadBalancer na rede privada e os apps estão no trabalhador incluído, inclua a nova sub-rede CIDR nesta configuração. **Nota**: se você incluir valores para `local.subnet`, verifique as configurações de VPN para a sub-rede no local para ver se elas também devem ser atualizadas.</td>
     </tr>
     </tbody></table>

    Se você tiver excluído um nó do trabalhador:

    <table>
    <caption>Configurações do nó do trabalhador</caption>
     <thead>
     <th>Configuração</th>
     <th>Descrição</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>Se você usar a NAT de sub-rede para remapear endereços IP locais privados específicos, remova quaisquer endereços IP dessa configuração que sejam do trabalhador antigo. Se você usar a NAT de sub-rede para remapear sub-redes inteiras e nenhum trabalhador permanecer em uma sub-rede, remova esse CIDR de sub-rede dessa configuração.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Se você limitou anteriormente a implementação do pod de VPN a um trabalhador único e esse trabalhador foi excluído, mude essa configuração para permitir que o pod de VPN seja executado em outros trabalhadores.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Se o trabalhador que você excluiu não foi contaminado, mas os únicos trabalhadores que permanecem estão contaminados, mude essa configuração para permitir que o pod de VPN seja executado em trabalhadores com quaisquer contaminações ou contaminações específicas.
     </td>
     </tr>
     </tbody></table>

4. Instale o novo gráfico Helm com os seus valores atualizados.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. Verifique o status de implementação do gráfico. Quando o gráfico estiver pronto, o campo **STATUS** perto da parte superior da saída terá um valor de `DEPLOYED`.

    ```
    helm status <release_name>
    ```
    {: pre}

6. Em alguns casos, pode ser necessário mudar suas configurações no local e suas configurações de firewall para corresponderem às mudanças feitas no arquivo de configuração da VPN.

7. Inicie a VPN.
    * Se a conexão VPN é iniciada pelo cluster (`ipsec.auto` está configurado para `start`), inicie a VPN no gateway no local e, em seguida, inicie a VPN no cluster.
    * Se a conexão VPN é iniciada pelo gateway no local (`ipsec.auto` está configurado para `auto`), inicie a VPN no cluster e, em seguida, inicie a VPN no gateway no local.

8. Configure a variável de ambiente `STRONGSWAN_POD`.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. Verifique o status da VPN.

    ```
    Kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * Se a conexão VPN tem um status de `ESTABLISHED`, a conexão VPN foi bem-sucedida. Nenhuma ação adicional é necessária.

    * Se você ainda está tendo problemas de conexão, veja [Não é possível estabelecer a conectividade VPN com o gráfico Helm do strongSwan](#cs_vpn_fails) para solucionar problemas posteriormente de sua conexão VPN.

<br />



## Não é possível recuperar políticas de rede do Calico
{: #cs_calico_fails}

{: tsSymptoms}
Ao tentar visualizar políticas de rede do Calico em seu cluster executando `calicoctl get policy`, você obtém um dos resultados inesperados ou mensagens de erro a seguir:
- Uma lista vazia
- Uma lista de políticas antigas do Calico v2 em vez de políticas da v3
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`

Ao tentar visualizar políticas de rede do Calico em seu cluster executando `calicoctl get GlobalNetworkPolicy`, você obtém um dos resultados inesperados ou mensagens de erro a seguir:
- Uma lista vazia
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'v1'`
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`
- `Failed to get resources: Resource type 'GlobalNetworkPolicy' is not supported`

{: tsCauses}
Para usar políticas do Calico, quatro fatores devem todos alinhar: a versão do cluster Kubernetes, a versão da CLI Calico, a sintaxe do arquivo de configuração do Calico e os comandos de política de visualização. Um ou mais desses fatores não está na versão correta.

{: tsResolve}
Deve-se usar a CLI v3.3 ou mais recente do Calico, a sintaxe do arquivo de configuração da v3 `calicoctl.cfg` e os comandos `calicoctl get GlobalNetworkPolicy` e `calicoctl get NetworkPolicy`.

Para assegurar que todos os fatores do Calico estejam alinhados:

1. [Instale e configure uma CLI do Calico da versão 3.3 ou mais recente](/docs/containers?topic=containers-network_policies#cli_install).
2. Assegure-se de que quaisquer políticas que você criar e desejar aplicar a seu cluster usem a [Sintaxe do Calico v3 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.3/reference/calicoctl/resources/networkpolicy). Se você tiver um arquivo de política existente `.yaml` ou `.json` na sintaxe do Calico v2, será possível convertê-lo em sintaxe do Calico v3 usando o [comando `calicoctl convert` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.3/reference/calicoctl/commands/convert).
3. Para [visualizar políticas](/docs/containers?topic=containers-network_policies#view_policies), assegure-se de usar `calicoctl get GlobalNetworkPolicy` para políticas globais e `calicoctl get NetworkPolicy --namespace <policy_namespace>` para políticas com escopo definido para namespaces específicos.

<br />


## Não é possível incluir nós do trabalhador devido a um ID de VLAN inválido
{: #suspended}

{: tsSymptoms}
A sua conta do {{site.data.keyword.Bluemix_notm}} foi suspensa ou todos os nós do trabalhador em seu cluster foram excluídos. Após a reativação da conta, não é possível incluir nós do trabalhador ao tentar redimensionar ou rebalancear seu conjunto de trabalhadores. Você vê uma mensagem de erro semelhante à seguinte:

```
SoftLayerAPIError(SoftLayer_Exception_Public): Could not obtain network VLAN with id #123456.
```
{: screen}

{: tsCauses}
Quando uma conta é suspensa, os nós do trabalhador dentro da conta são excluídos. Se um cluster não tem nós do trabalhador, a infraestrutura do IBM Cloud (SoftLayer) recupera as VLANs públicas e privadas associadas. No entanto, o conjunto de trabalhadores do cluster ainda tem os IDs de VLAN anteriores em seus metadados e usa esses IDs indisponíveis quando você rebalanceia ou redimensiona o conjunto. Os nós falham ao serem criados porque as VLANs não estão mais associadas ao cluster.

{: tsResolve}

É possível [excluir seu conjunto de trabalhadores existente](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_rm), em seguida, [criar um novo conjunto de trabalhadores](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create).

Como alternativa, é possível manter o seu conjunto de trabalhadores existente pedindo novas VLANs e usando-as para criar novos nós do trabalhador no conjunto.

Antes de iniciar: [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Para obter as zonas para as quais você precisa de novos IDs de VLAN, anote o **Local** na saída de comando a seguir. **Nota**: se o seu cluster for de múltiplas zonas, serão necessários IDs de VLAN para cada zona.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

2.  Obtenha uma nova VLAN privada e pública para cada zona em que seu cluster está, [entrando em contato com o suporte do {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans).

3.  Anote os novos IDs de VLAN privada e pública para cada zona.

4.  Anote o nome de seus conjuntos de trabalhadores.

    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

5.  Use o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set) `zone-network-set` para mudar os metadados de rede do conjunto de trabalhadores.

    ```
    ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> -- worker-pools <worker-pool> --private-vlan <private_vlan_ID> --public-vlan <public_vlan_ID>
    ```
    {: pre}

6.  **Somente cluster de múltiplas zonas**: repita a **Etapa 5** para cada zone em seu cluster.

7.  Rebalanceie ou redimensione seu conjunto de trabalhadores para incluir nós do trabalhador que usam os novos IDs de VLAN. Por exemplo:

    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <worker_pool> --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

8.  Verifique se os nós do trabalhador estão criados.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <worker_pool>
    ```
    {: pre}

<br />



## Obtendo ajuda e suporte
{: #network_getting_help}

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

