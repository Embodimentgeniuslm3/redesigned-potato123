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



# Configurando sub-redes para clusters
{: #subnets}

Mude o conjunto de endereços IP públicos ou privados móveis disponíveis para serviços de balanceador de carga de rede (NLB) incluindo sub-redes em seu cluster Kubernetes.
{:shortdesc}



## Visão geral de rede no {{site.data.keyword.containerlong_notm}}
{: #basics}

Entenda os conceitos básicos de rede em clusters do {{site.data.keyword.containerlong_notm}}. O {{site.data.keyword.containerlong_notm}} usa VLANs, sub-redes e endereços IP para fornecer conectividade de rede de componentes do cluster.
{: shortdesc}

### VLANs
{: #basics_vlans}

Quando você cria um cluster, os nós do trabalhador do cluster são conectados automaticamente a uma VLAN. Uma VLAN configura um grupo de nós do trabalhador e pods como se eles estivessem conectados à mesma ligação física e fornece um canal para conectividade entre os trabalhadores e os pods.
{: shortdesc}

<dl>
<dt>VLANs para clusters grátis</dt>
<dd>Em clusters grátis, os nós do trabalhador do cluster são conectados a uma VLAN pública e uma VLAN privada de propriedade da IBM por padrão. Como a IBM controla as VLANs, as sub-redes e os endereços IP, não é possível criar clusters de múltiplas zonas ou incluir sub-redes em seu cluster e é possível usar somente serviços NodePort para expor seu app.</dd>
<dt>VLANs para clusters padrão</dt>
<dd>Em clusters padrão, na primeira vez que você criar um cluster em uma zona, uma VLAN pública e uma VLAN privada nessa zona serão provisionadas automaticamente para você em sua conta de infraestrutura do IBM Cloud (SoftLayer). Para cada cluster subsequente que você criar nessa zona, deverá ser especificado o par de VLAN que você deseja usar nessa zona. É possível reutilizar as mesmas VLANs públicas e privadas que foram criadas para você porque diversos clusters podem compartilhar VLANs.</br>
</br>É possível conectar seus nós do trabalhador a uma VLAN pública e à VLAN privada ou somente à VLAN privada. Se você deseja conectar seus nós do trabalhador somente a uma VLAN privada, é possível usar o ID de uma VLAN privada existente ou [criar uma VLAN privada](/docs/cli/reference/ibmcloud?topic=cloud-cli-manage-classic-vlans#sl_vlan_create) e usar o ID durante a criação do cluster.</dd></dl>

Para ver as VLANs provisionadas em cada zona para sua conta, execute `ibmcloud ks vlans --zone <zone>.` Para ver as VLANs nas quais um cluster está provisionado, execute `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> --showResources` e procure a seção **VLANs de sub-rede**.

A infraestrutura do IBM Cloud (SoftLayer) gerencia as VLANs que são provisionadas automaticamente quando você cria seu primeiro cluster em uma zona. Se você deixar que uma VLAN se torne inutilizável, como removendo todos os nós do trabalhador de uma VLAN, a infraestrutura do IBM Cloud (SoftLayer) recuperará a VLAN. Depois, se você precisar de uma nova VLAN, [entre em contato com o suporte do {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans).

**Posso mudar minha decisão de VLAN posteriormente?**</br>
É possível alterar a configuração de VLAN modificando os conjuntos de trabalhadores em seu cluster. Para obter mais informações, consulte [Mudando as conexões VLAN do nó do trabalhador](/docs/containers?topic=containers-cs_network_cluster#change-vlans).

### Subnets e endereços IP
{: #basics_subnets}

Além dos nós do trabalhador e dos pods, as sub-redes também são provisionadas automaticamente em VLANs. As sub-redes fornecem conectividade de rede para seus componentes do cluster designando endereços IP a eles.
{: shortdesc}

As sub-redes a seguir são provisionadas automaticamente nas VLANs públicas e privadas padrão:

** Sub-redes Public VLAN **
* A sub-rede pública primária determina os endereços IP públicos que são designados aos nós do trabalhador durante a criação do cluster. Vários clusters na mesma VLAN podem compartilhar uma sub-rede pública primária.
* A sub-rede pública móvel é ligada a somente um cluster e fornece o cluster com 8 endereços IP públicos. 3 IPs são reservados para as funções de infraestrutura do IBM Cloud (SoftLayer). 1 IP é usado pelo ALB do Ingress público padrão e 4 IPs podem ser usados para criar serviços de balanceador de carga de rede (NLB) públicos ou mais ALBs públicos. Os IPs públicos móveis são endereços IP fixos e permanentes que podem ser usados para acessar NLBs ou ALBs pela Internet. Se você precisar de mais de 4 IPs para NLBs ou ALBs, consulte [Incluindo endereços IP móveis](/docs/containers?topic=containers-subnets#adding_ips).

** Sub-redes de VLAN privada **
* A sub-rede privada primária determina os endereços IP privados que são designados aos nós do trabalhador durante a criação do cluster. Múltiplos clusters na mesma VLAN podem compartilhar uma sub-rede privada primária.
* A sub-rede privada móvel é ligada a somente um cluster e fornece o cluster com 8 endereços IP privados. 3 IPs são reservados para as funções de infraestrutura do IBM Cloud (SoftLayer). 1 IP é usado pelo ALB do Ingress privado padrão e 4 IPs podem ser usados para criar serviços de balanceador de carga de rede (NLB) privados ou mais ALBs privados. Os IPs privados móveis são endereços IP fixos e permanentes que podem ser usados para acessar NLBs ou ALBs por uma rede privada. Se você precisar de mais de 4 IPs para NLBs ou ALBs privados, consulte [Incluindo endereços IP móveis](/docs/containers?topic=containers-subnets#adding_ips).

Para ver todas as sub-redes provisionadas em sua conta, execute `ibmcloud ks subnets`. Para ver as sub-redes públicas e privadas móveis que estão ligadas a um cluster, é possível executar `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> --showResources` e procurar a seção **VLANs de sub-rede**.

No {{site.data.keyword.containerlong_notm}}, as VLANs têm um limite de 40 sub-redes. Se você atingir esse limite, primeiro verifique se é possível [reutilizar sub-redes na VLAN para criar novos clusters](/docs/containers?topic=containers-subnets#subnets_custom). Se precisar de uma nova VLAN, solicite uma [entrando em contato com o suporte do {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans). Em seguida, [crie um cluster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) que usa essa nova VLAN.
{: note}

**Os endereços IP para meus nós do trabalhador mudam?**</br>
Um endereço IP é designado ao seu nó do trabalhador nas VLANs públicas ou privadas que são usadas por seu cluster. Depois que o nó do trabalhador é provisionado, os endereços IP não mudam. Por exemplo, os endereços IP do nó do trabalhador persistem nas operações `reload`, `reboot` e `update`. Além disso, o endereço IP privado do nó do trabalhador é usado para sua identidade na maioria dos comandos `kubectl`. Se você mudar as VLANs que o conjunto do trabalhador usa, novos nós do trabalhador provisionados nesse conjunto usarão as novas VLANs para seus endereços IP. Os endereços IP do nó do trabalhador existente não são mudados, mas é possível optar por remover os nós do trabalhador que usam VLANs antigas.

### Segmentação de rede
{: #basics_segmentation}

A segmentação de rede descreve a abordagem para dividir uma rede em múltiplas sub-redes. Os apps que são executados em uma sub-rede não podem ver ou acessar apps em outra sub-rede. Para obter mais informações sobre as opções de segmentação de rede e como elas se relacionam com VLANs, consulte [este tópico de segurança de cluster](/docs/containers?topic=containers-security#network_segmentation).
{: shortdesc}

No entanto, em várias situações, os componentes em seu cluster devem ter permissão para se comunicar por meio de múltiplas VLANs privadas. Por exemplo, se você desejar criar um cluster de múltiplas zonas, se você tiver múltiplas VLANs para um cluster ou se tiver múltiplas sub-redes na mesma VLAN, os nós do trabalhador em sub-redes diferentes na mesma VLAN ou em VLANs diferentes não poderão se comunicar automaticamente entre si. Deve-se ativar um Virtual Router Function (VRF) ou uma ampliação de VLAN para a sua conta de infraestrutura do IBM Cloud (SoftLayer).

**O que são Virtual Router Functions (VRF) e a ampliação de VLAN?**</br>

<dl>
<dt>[ Virtual Router Function (VRF) ](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)</dt>
<dd>Um VRF permite que todas as VLANs e sub-redes em sua conta de infraestrutura se comuniquem umas com as outras. Além disso, um VRF é necessário para permitir que seus trabalhadores e o mestre se comuniquem por meio do terminal em serviço privado. Para ativar o VRF, [entre em contato com o representante de conta da infraestrutura do IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Observe que o VRF elimina a opção VLAN Spanning para sua conta, porque todas as VLANs são capazes de se comunicar, a menos que você configure um dispositivo de gateway para gerenciar o tráfego.</dd>
<dt>[ VLAN Spanning ](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)</dt>
<dd>Se não for possível ou você não desejar ativar o VRF, ative o VLAN Spanning. Para executar essa ação, você precisa da [permissão de infraestrutura](/docs/containers?topic=containers-users#infra_access) **Rede > Gerenciar a rede VLAN Spanning** ou é possível solicitar ao proprietário da conta para ativá-la. Para verificar se o VLAN Spanning já está ativado, use o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region<region>`. Observe que não é possível ativar o terminal em serviço privado se você optar por ativar a ampliação de VLAN em vez de um VRF.</dd>
</dl>

**Como o VRF ou a ampliação de VLAN afeta a segmentação de rede?**</br>

Quando o VRF ou a ampliação de VLAN estiver ativada, qualquer sistema que esteja conectado a qualquer uma das VLANs privadas na mesma conta do {{site.data.keyword.Bluemix_notm}} poderá se comunicar com os trabalhadores. É possível isolar seu cluster de outros sistemas na rede privada, aplicando [políticas de rede privada do Calico](/docs/containers?topic=containers-network_policies#isolate_workers). O {{site.data.keyword.containerlong_notm}} também é compatível com todas as [ofertas de firewall da infraestrutura do IBM Cloud (SoftLayer) ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud-computing/bluemix/network-security). É possível configurar um firewall, como um [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra), com políticas de rede customizadas para fornecer segurança de rede dedicada para seu cluster padrão e para detectar e remediar intrusão de rede.

<br />



## Usando sub-redes existentes para criar um cluster
{: #subnets_custom}

Ao criar um cluster padrão, as sub-redes são criadas automaticamente para você. No entanto, em vez de usar as sub-redes provisionadas automaticamente, é possível usar as sub-redes móveis existentes de sua conta de infraestrutura do IBM Cloud (SoftLayer) ou reutilizar sub-redes de um cluster excluído.
{:shortdesc}

Use essa opção para reter endereços IP estáticos estáveis em remoções e criações de cluster ou para pedir blocos maiores de endereços IP. Se, em vez disso, desejar obter mais endereços IP privados móveis para os serviços de balanceador de carga de rede (NLB) de seu cluster usando sua própria sub-rede de rede local, consulte [Incluindo IPs privados móveis por meio da inclusão de sub-redes gerenciadas pelo usuário em VLANs privadas](#subnet_user_managed).

Os endereços IP públicos móveis são cobrados mensalmente. Se você remover os endereços IP públicos móveis depois que o cluster for provisionado, ainda terá que pagar o encargo mensal, mesmo se os tiver usado apenas por um curto período de tempo.
{: note}

Antes de iniciar:
- [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Para reutilizar sub-redes de um cluster que você não precisa mais, exclua o cluster desnecessário. Crie o novo cluster imediatamente, porque as sub-redes serão excluídas dentro de 24 horas se você não as reutilizar.

   ```
   ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
   ```
   {: pre}

</br>Para usar uma sub-rede existente em seu portfólio de infraestrutura do IBM Cloud (SoftLayer):

1. Obtenha o ID da sub-rede a ser usada e o ID da VLAN na qual a sub-rede está.

    ```
    ibmcloud ks subnets
    ```
    {: pre}

    Nesta saída de exemplo, o ID de sub-rede é `1602829` e o ID de VLAN é `2234945`:
    ```
    Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public
    ```
    {: screen}

2. [Crie um cluster na CLI](/docs/containers?topic=containers-clusters#clusters_cli_steps) usando o ID da VLAN que você identificou. Inclua a sinalização `--no-subnet` para evitar que uma nova sub-rede IP pública móvel e uma nova sub-rede IP privada móvel seja criada automaticamente.

    ```
    ibmcloud ks cluster-create --zone dal10 --machine-type b3c.4x16 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name my_cluster
    ```
    {: pre}
    Se não conseguir se lembrar em qual zona a VLAN está para o sinalizador `--zone`, será possível verificar se a VLAN está em uma determinada zona executando `ibmcloud ks vlans --zone <zone>`.
    {: tip}

3.  Verifique se o cluster foi criado. Pode levar até 15 minutos para que as máquinas do nó do trabalhador sejam ordenadas e para que o cluster seja configurado e provisionado em sua conta.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

    Quando seu cluster estiver totalmente provisionado, o **State** mudará para `deployed`.

    ```
    Name         ID                                   State      Created          Workers    Zone      Version     Resource Group Name
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3          dal10     1.13.6      Default
    ```
    {: screen}

4.  Verifique o status dos nós do trabalhador.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Antes de continuar com a próxima etapa, os nós do trabalhador devem estar prontos. O **State** muda para `normal` e o **Status** é `Ready`.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Zone     Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx  free           normal     Ready    dal10      1.13.6
    ```
    {: screen}

5.  Inclua a sub-rede em seu cluster especificando o ID da sub-rede. Quando você disponibiliza uma sub-rede para um cluster, um configmap do Kubernetes é criado incluindo todos os endereços IP públicos móveis disponíveis que podem ser usados. Se nenhum ALB do Ingress existir na zona na qual a VLAN da sub-rede está localizada, um endereço IP privado móvel e um público móvel serão usados automaticamente para criar os ALBs públicos e privados para essa zona. É possível usar todos os outros endereços IP públicos e privados da sub-rede para criar serviços NLB para seus aplicativos.

  ```
  ibmcloud ks cluster-subnet-add --cluster <cluster_name_or_id> --subnet-id <subnet_ID>
  ```
  {: pre}

  Exemplo de comando:
  ```
  ibmcloud ks cluster-subnet-add --cluster mycluster --subnet-id 807861
  ```
  {: screen}

6. **Importante**: para ativar a comunicação entre trabalhadores que estiverem em sub-redes diferentes na mesma VLAN, deve-se [ativar o roteamento entre sub-redes na mesma VLAN](#subnet-routing).

<br />


## Gerenciando endereços IP móveis portáteis
{: #managing_ips}

Por padrão, 4 endereços IP públicos móveis e 4 endereços IP privados móveis podem ser usados para expor apps únicos para a rede pública ou privada [criando um serviço de balanceador de carga de rede (NLB)](/docs/containers?topic=containers-loadbalancer) ou [criando balanceadores de carga do aplicativo (ALBs) do Ingress adicionais](/docs/containers?topic=containers-ingress#scale_albs). Para criar um serviço NLB ou ALB, deve-se ter pelo menos 1 endereço IP móvel do tipo correto disponível. É possível visualizar os endereços IP móveis que estão disponíveis ou liberar um endereço IP móvel usado.
{: shortdesc}

### Visualizando endereços IP públicos móveis disponíveis
{: #review_ip}

Para listar todos os endereços IP móveis em seu cluster, ambos usados e disponíveis, é possível executar o comando a seguir.
{: shortdesc}

```
kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
```
{: pre}

Para listar somente os endereços IP públicos móveis que estão disponíveis para criar NLBs públicos ou mais ALBs públicos, é possível usar as etapas a seguir:

Antes de iniciar:
-  Assegure-se de que você tenha a [função de **Gravador** ou **Gerenciador** do serviço {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o namespace `default`.
- [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Para listar os endereços IP públicos móveis disponíveis:

1.  Crie um arquivo de configuração de serviço do Kubernetes denominado `myservice.yaml` e defina um serviço do tipo `LoadBalancer` com um endereço IP simulado do NLB. O exemplo a seguir usa o endereço IP 1.1.1.1 como o endereço IP do NLB. Substitua `<zone>` pela zona na qual deseja verificar IPs disponíveis.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
      annotations:
        service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "<zone>"
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  Crie o serviço em seu cluster.

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  Inspecione o serviço.

    ```
    kubectl describe service myservice
    ```
    {: pre}

    A criação desse serviço falha porque o principal do Kubernetes não pode localizar em seu configmap o endereço IP do NLB especificado. Quando você executa esse comando, é possível ver a mensagem de erro e uma lista de endereços IP públicos disponíveis para o cluster.

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. Os endereços IP do provedor em nuvem a seguir estão disponíveis: <list_of_IP_addresses>
    ```
    {: screen}

<br />


### Liberando os endereços IP usados
{: #free}

É possível liberar um endereço IP móvel usado excluindo o serviço de balanceador de carga de rede (NLB) ou desativando o balanceador de carga do aplicativo (ALB) do Ingress que está usando o endereço IP móvel.
{:shortdesc}

Antes de iniciar:
-  Assegure-se de que você tenha a [função de **Gravador** ou **Gerenciador** do serviço {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o namespace `default`.
- [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Para excluir um NLB ou desativar um ALB:

1. Liste os serviços disponíveis em seu cluster.
    ```
    kubectl get services | grep LoadBalancer
    ```
    {: pre}

2. Remova o serviço de balanceador de carga ou desative o ALB que usa um endereço IP público ou privado.
  * Exclua um NLB:
    ```
    kubectl delete service <service_name>
    ```
    {: pre}
  * Desative um ALB:
    ```
    ibmcloud ks alb-configure -- albID < ALB_ID> --disable
    ```
    {: pre}

<br />


## Incluindo endereços IP móveis
{: #adding_ips}

Por padrão, 4 endereços IP públicos móveis e 4 endereços IP privados móveis podem ser usados para expor apps únicos para a rede pública ou privada [criando um serviço de balanceador de carga de rede (NLB)](/docs/containers?topic=containers-loadbalancer). Para criar mais de quatro NLBs públicos ou mais de quatro privados, é possível obter mais endereços IP móveis incluindo sub-redes de rede no cluster.
{: shortdesc}

Quando você torna uma sub-rede disponível para um cluster, os endereços IP
dessa sub-rede são usados para propósitos de rede do cluster. Para evitar conflitos de endereço IP, certifique-se de usar uma sub-rede com somente um cluster. Não use uma sub-rede para múltiplos clusters ou para outros
propósitos fora do {{site.data.keyword.containerlong_notm}} ao mesmo
tempo.
{: important}

### Incluindo IPs móveis pedindo mais sub-redes
{: #request}

É possível obter mais IPs móveis para serviços NLB criando uma nova sub-rede em uma conta da infraestrutura do IBM Cloud (SoftLayer) e disponibilizando-a para seu cluster especificado.
{:shortdesc}

Os endereços IP públicos móveis são cobrados mensalmente. Se você remove os endereços IP públicos móveis após a sua sub-rede ser provisionada, deve-se ainda pagar o encargo mensal, mesmo que os tenha usado somente por uma curta quantia de tempo.
{: note}

Antes de iniciar:
-  Assegure-se de que tenha a [função de plataforma **Operador** ou **Administrador** do IAM do {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-users#platform) para o cluster.
- [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Para pedir uma sub-rede:

1. Provisione uma nova sub-rede.

    ```
    ibmcloud ks cluster-subnet-create --cluster <cluster_name_or_id> --size <subnet_size> --vlan <VLAN_ID>
    ```
    {: pre}

    <table>
    <caption>Entendendo os componentes deste comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes desse comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td>Substitua <code>&lt;cluster_name_or_id&gt;</code> pelo nome ou ID do cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td>Substitua <code>&lt;subnet_size&gt;</code> pelo número de endereços IP que você deseja criar na sub-rede móvel. Os valores aceitos são 8, 16, 32 ou 64. <p class="note"> Quando você inclui endereços IP móveis para sua sub-rede, três endereços IP são usados para estabelecer a rede interna do cluster. Não é possível usar esses três endereços IP para seus balanceadores de carga do aplicativo (ALBs) Ingress ou para criar serviços de balanceador de carga de rede (NLB). Por exemplo, se você solicitar oito endereços IP públicos móveis, será possível usar cinco deles para expor os seus apps ao público.</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td>Substitua <code>&lt;VLAN_ID&gt;</code> pelo ID da VLAN pública ou privada na qual você deseja alocar os endereços IP móveis públicos ou privados. Deve-se selecionar uma VLAN pública ou privada à qual um nó do trabalhador existente está conectado. Para revisar as VLANs públicas ou privadas às quais os nós do trabalhador estão conectados, execute <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code> e procure a seção <strong>VLANs da sub-rede</strong> na saída. A sub-rede é provisionada na mesma zona em que a VLAN está.</td>
    </tr>
    </tbody></table>

2. Verifique se a sub-rede foi criada com sucesso e se foi incluída em seu cluster. O CIDR da sub-rede é listado na seção **Subnet VLANs**.

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

3. **Importante**: para ativar a comunicação entre trabalhadores que estiverem em sub-redes diferentes na mesma VLAN, deve-se [ativar o roteamento entre sub-redes na mesma VLAN](#subnet-routing).

<br />


### Incluindo IPs privados portáteis incluindo sub-redes gerenciadas pelo usuário em VLANs privadas
{: #subnet_user_managed}

É possível obter mais IPs privados móveis para serviços de balanceador de carga de rede (NLB) ao disponibilizar uma sub-rede de uma rede local para seu cluster.
{:shortdesc}

Deseja reutilizar sub-redes móveis existentes em sua conta de infraestrutura do IBM Cloud (SoftLayer) em vez disso? Consulte [Usando sub-redes customizadas ou existentes da infraestrutura do IBM Cloud (SoftLayer) para criar um cluster](#subnets_custom).
{: tip}

Requisitos:
- Sub-redes gerenciadas pelo usuário podem ser incluídas em VLANs privadas apenas.
- O limite de comprimento de prefixo de sub-rede é /24 para /30. Por exemplo, `169.xx.xxx.xxx/24` especifica 253 endereços IP privados utilizáveis, enquanto `169.xx.xxx.xxx/30` especifica 1 endereço IP privado utilizável.
- O primeiro endereço IP na sub-rede deve ser usado como o gateway para a sub-rede.

Antes de iniciar:
- Configure o roteamento de tráfego de rede dentro e fora da sub-rede externa.
- Confirme se você tem conectividade VPN entre o gateway de rede do data center no local e o Virtual Router Appliance de rede privada ou o serviço VPN do strongSwan que é executado em seu cluster. Para obter mais informações, veja [Configurando a conectividade VPN](/docs/containers?topic=containers-vpn).
-  Assegure-se de que tenha a [função de plataforma **Operador** ou **Administrador** do IAM do {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-users#platform) para o cluster.
- [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)


Para incluir uma sub-rede de uma rede no local:

1. Visualize o ID da VLAN privada de seu cluster. Localize a seção  ** VLANs de sub-rede ** . No campo **Gerenciado pelo usuário**, identifique o ID da VLAN com _false_.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    Subnet VLANs VLAN ID Subnet CIDR Public User-managed 2234947 10.xxx.xx.xxx/29 false false 2234945 169.xx.xxx.xxx/29 true false
    ```
    {: screen}

2. Inclua a sub-rede externa em sua VLAN privada. Os endereços IP privados móveis são incluídos no configmap do cluster.

    ```
    ibmcloud ks cluster-user-subnet-add --cluster <cluster_name> --subnet-cidr <subnet_CIDR> --private-vlan <private_VLAN>
    ```
    {: pre}

    Exemplo:

    ```
    ibmcloud ks cluster-user-subnet-add --cluster mycluster --subnet-cidr 10.xxx.xx.xxx/24 --private-vlan 2234947
    ```
    {: pre}

3. Verifique se a sub-rede fornecida pelo usuário foi incluída. O campo **Gerenciado pelo usuário** é _true_.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    Nesta saída de exemplo, uma segunda sub-rede foi incluída na VLAN privada `2234947`:
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    2234947   10.xxx.xx.xxx/24  false    true
    ```
    {: screen}

4. [Ative o roteamento entre sub-redes na mesma VLAN](#subnet-routing).

5. Inclua um [serviço de balanceador de carga de rede (NLB) privada](/docs/containers?topic=containers-loadbalancer) ou ative um [ALB privado do Ingress](/docs/containers?topic=containers-ingress#private_ingress) para acessar seu aplicativo pela rede privada. Para usar um endereço IP privado por meio da sub-rede que você incluiu, deve-se especificar um endereço IP do CIDR da sub-rede. Caso contrário, um endereço IP será escolhido aleatoriamente das sub-redes de infraestrutura do IBM Cloud (SoftLayer) ou sub-redes fornecidas pelo usuário na VLAN privada.

<br />


## Gerenciando o roteamento de sub
{: #subnet-routing}

Se você tem múltiplas VLANs para um cluster, múltiplas sub-redes na mesma VLAN ou um cluster de múltiplas zonas, deve-se ativar um [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para sua conta de infraestrutura do IBM Cloud (SoftLayer) para que seus nós do trabalhador possam se comunicar entre si na rede privada. Para ativar o VRF, [entre em contato com o representante de conta da infraestrutura do IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Se não for possível ou você não desejar ativar o VRF, ative o [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para executar essa ação, você precisa da [permissão de infraestrutura](/docs/containers?topic=containers-users#infra_access) **Rede > Gerenciar a rede VLAN Spanning** ou é possível solicitar ao proprietário da conta para ativá-la. Para verificar se o VLAN Spanning já está ativado, use o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region<region>`.

Revise os cenários a seguir nos quais o VLAN Spanning também é necessário.

### Ativando o roteamento entre as sub-redes primárias na mesma VLAN
{: #vlan-spanning}

Quando você cria um cluster, as sub-redes públicas primárias e privadas são provisionadas nas VLANs públicas e privadas. A sub-rede pública primária termina em `/28` e fornece 14 IPs públicos para os nós do trabalhador. A sub-rede privada primária termina em `/26` e fornece IPs privados para até 62 nós do trabalhador.
{:shortdesc}

Você pode exceder os 14 IPs públicos e os 62 IPs privados iniciais para nós do trabalhador, tendo um cluster grande ou vários clusters menores na mesma localização na mesma VLAN. Quando uma sub-rede pública ou privada atinge o limite de nós do trabalhador, outra sub-rede primária na mesma VLAN é pedida.

Para assegurar que os trabalhadores nessas sub-redes primárias na mesma VLAN possam se comunicar, deve-se ativar o VLAN Spanning. Para obter instruções, veja [Ativar ou desativar a ampliação de VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning).

Para verificar se o VLAN Spanning já está ativado, use o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region<region>`.
{: tip}

### Gerenciando o roteamento de sub-rede para dispositivos de gateway
{: #vra-routing}

Ao criar um cluster, uma sub-rede privada móvel e uma pública móvel são pedidas nas VLANs às quais o cluster está conectado. Essas sub-redes fornecem endereços IP para os serviços do balanceador de carga do aplicativo (ALB) Ingress e do balanceador de carga de rede (NLB).
{: shortdesc}

No entanto, se você tiver um dispositivo de roteador existente, como um [Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra#about-the-vra), as sub-redes móveis recém-incluídas daquelas VLANs às quais o cluster está conectado não serão configuradas no roteador. Para usar NLBs ou ALBs do Ingress, deve-se garantir que os dispositivos de rede possam ser roteados entre diferentes sub-redes na mesma VLAN [ativando o VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning).

Para verificar se o VLAN Spanning já está ativado, use o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region<region>`.
{: tip}
