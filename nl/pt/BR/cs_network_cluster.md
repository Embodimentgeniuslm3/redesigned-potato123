---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-06"

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


# Mudando terminais em serviço ou conexões VLAN
{: #cs_network_cluster}

Depois de configurar inicialmente sua rede ao [criar um cluster](/docs/containers?topic=containers-clusters), é possível mudar os terminais em serviço por meio dos quais o principal do Kubernetes é acessível ou mudar as conexões VLAN para os nós do trabalhador.
{: shortdesc}

## Configurando o terminal em serviço privado
{: #set-up-private-se}

Em clusters que executam o Kubernetes versão 1.11 ou mais recente, ative ou desative o terminal em serviço privado para seu cluster.
{: shortdesc}

O terminal em serviço privado torna seu mestre do Kubernetes acessível privadamente. Os nós do trabalhador e seus usuários de cluster autorizados podem se comunicar com o mestre do Kubernetes sobre a rede privada. Para determinar se é possível ativar o terminal em serviço privado, consulte [Comunicação de trabalhador para principal e de usuário para principal](/docs/containers?topic=containers-plan_clusters#internet-facing). Observe que não é possível desativar o terminal em serviço privado depois de ativá-lo.

Você criou um cluster com somente um terminal em serviço privado antes de ativar sua conta para [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) e [terminais em serviço](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)? Tente [configurar o terminal em serviço público](#set-up-public-se) para que seja possível usar seu cluster até que seus casos de suporte sejam processados para atualizar sua conta.
{: tip}

1. Ative o [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) em sua conta de infraestrutura do IBM Cloud (SoftLayer).
2. [Ative sua conta do {{site.data.keyword.Bluemix_notm}} para usar os terminais em serviço](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
3. Ative o terminal em serviço privado.
   ```
   ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
4. Atualize o servidor da API mestre do Kubernetes para usar o terminal em serviço privado. É possível seguir o prompt na CLI ou executar manualmente o comando a seguir.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}

5. [Crie um configmap](/docs/containers?topic=containers-update#worker-up-configmap) para controlar o número máximo de nós do trabalhador que podem estar indisponíveis por vez em seu cluster. Quando você atualiza seus nós do trabalhador, o configmap ajuda a evitar o tempo de inatividade para seus apps, pois os apps são reagendados de forma ordenada em nós do trabalhador disponíveis.
6. Atualize todos os nós do trabalhador em seu cluster para selecionar a configuração de terminal em serviço privado.

   <p class="important">Emitindo o comando de atualização, os nós do trabalhador são recarregados para selecionar a configuração de terminal em serviço. Se nenhuma atualização do trabalhador está disponível, deve-se [recarregar os nós do trabalhador manualmente](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli). Se você recarregar, certifique-se de bloquear, drenar e gerenciar o pedido para controlar o número máximo de nós do trabalhador que estão indisponíveis por vez.</p>
   ```
   ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
   ```
   {: pre}

8. Se o cluster estiver em um ambiente atrás de um firewall:
  * [Permita que os usuários de cluster autorizados executem comandos `kubectl` para acessar o mestre por meio do terminal em serviço privado. ](/docs/containers?topic=containers-firewall#firewall_kubectl)
  * [Permita o tráfego de rede de saída para os IPs privados](/docs/containers?topic=containers-firewall#firewall_outbound) para recursos de infraestrutura e para os serviços do {{site.data.keyword.Bluemix_notm}} que você planeja usar.

9. Opcional: para usar o terminal em serviço privado apenas, desative o terminal em serviço público.
   ```
   ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## Configurando o terminal em serviço público
{: #set-up-public-se}

Ative ou desative o terminal em serviço público para seu cluster.
{: shortdesc}

O terminal em serviço público torna o mestre do Kubernetes publicamente acessível. Seus nós do trabalhador e seus usuários de cluster autorizados podem se comunicar de forma segura com o mestre do Kubernetes na rede pública. Para obter mais informações, consulte [Comunicação de trabalhador para principal e de usuário para principal](/docs/containers?topic=containers-plan_clusters#internet-facing).

**Etapas para ativar**</br>
Se você desativou anteriormente o terminal público, será possível reativá-lo.
1. Ative o terminal em serviço público.
   ```
   ibmcloud ks cluster-feature-enable public-service-endpoint -- cluster < cluster_name_or_ID>
   ```
   {: pre}
2. Atualize o servidor da API do mestre do Kubernetes para usar o terminal em serviço público. É possível seguir o prompt na CLI ou executar manualmente o comando a seguir.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}

   </br>

**Etapas para a desativação**</br>
Para desativar o terminal de serviço público, deve-se primeiro ativar o terminal de serviço privado para que os nós do trabalhador possam se comunicar com o principal do Kubernetes.
1. Ative o terminal em serviço privado.
   ```
   ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
2. Atualize o servidor de API do mestre do Kubernetes para usar o terminal em serviço privado, seguindo o prompt da CLI ou executando manualmente o comando a seguir.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}
3. [Crie um configmap](/docs/containers?topic=containers-update#worker-up-configmap) para controlar o número máximo de nós do trabalhador que podem estar indisponíveis por vez em seu cluster. Quando você atualiza seus nós do trabalhador, o configmap ajuda a evitar o tempo de inatividade para seus apps, pois os apps são reagendados de forma ordenada em nós do trabalhador disponíveis.

4. Atualize todos os nós do trabalhador em seu cluster para selecionar a configuração de terminal em serviço privado.

   <p class="important">Emitindo o comando de atualização, os nós do trabalhador são recarregados para selecionar a configuração de terminal em serviço. Se nenhuma atualização do trabalhador está disponível, deve-se [recarregar os nós do trabalhador manualmente](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli). Se você recarregar, certifique-se de bloquear, drenar e gerenciar o pedido para controlar o número máximo de nós do trabalhador que estão indisponíveis por vez.</p>
   ```
   ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
   ```
  {: pre}
5. Desativar o terminal em serviço público.
   ```
   ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}

## Alternando do terminal em serviço público para o terminal em serviço privado
{: #migrate-to-private-se}

Em clusters que executam o Kubernetes versão 1.11 ou mais recente, ative os nós do trabalhador para se comunicar com o mestre por meio da rede privada em vez da rede pública, ativando o terminal em serviço privado.
{: shortdesc}

Todos os clusters que estão conectados a uma VLAN pública e privada usam o terminal em serviço público por padrão. Seus nós do trabalhador e seus usuários de cluster autorizados podem se comunicar de forma segura com o mestre do Kubernetes na rede pública. Para permitir que os nós do trabalhador se comuniquem com o mestre do Kubernetes por meio da rede privada em vez da rede pública, é possível ativar o terminal em serviço privado. Em seguida, será possível desativar opcionalmente o terminal de serviço público.
* Se você ativar o terminal em serviço privado e mantiver o terminal em serviço público ativado também, os trabalhadores sempre se comunicarão com o mestre por meio da rede privada, mas seus usuários poderão se comunicar com o mestre por meio da rede pública ou privada.
* Se você ativar o terminal em serviço privado, mas desativar o terminal em serviço público, os trabalhadores e os usuários deverão se comunicar com o mestre por meio da rede privada.

Observe que não é possível desativar o terminal em serviço privado depois de ativá-lo.

1. Ative o [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) em sua conta de infraestrutura do IBM Cloud (SoftLayer).
2. [Ative sua conta do {{site.data.keyword.Bluemix_notm}} para usar os terminais em serviço](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
3. Ative o terminal em serviço privado.
   ```
   ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
4. Atualize o servidor de API do mestre do Kubernetes para usar o terminal em serviço privado, seguindo o prompt da CLI ou executando manualmente o comando a seguir.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}
5. [Crie um configmap](/docs/containers?topic=containers-update#worker-up-configmap) para controlar o número máximo de nós do trabalhador que podem estar indisponíveis por vez em seu cluster. Quando você atualiza seus nós do trabalhador, o configmap ajuda a evitar o tempo de inatividade para seus apps, pois os apps são reagendados de forma ordenada em nós do trabalhador disponíveis.

6.  Atualize todos os nós do trabalhador em seu cluster para selecionar a configuração de terminal em serviço privado.

    <p class="important">Emitindo o comando de atualização, os nós do trabalhador são recarregados para selecionar a configuração de terminal em serviço. Se nenhuma atualização do trabalhador está disponível, deve-se [recarregar os nós do trabalhador manualmente](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli). Se você recarregar, certifique-se de bloquear, drenar e gerenciar o pedido para controlar o número máximo de nós do trabalhador que estão indisponíveis por vez.</p>
    ```
    ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
    ```
    {: pre}

7. Opcional: desative o terminal em serviço público.
   ```
   ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}

<br />


## Mudando as conexões VLAN do nó do trabalhador
{: #change-vlans}

Ao criar um cluster, você escolhe se deseja conectar os nós do trabalhador a uma VLAN privada e pública ou a uma VLAN somente privada. Os nós do trabalhador fazem parte de conjuntos de trabalhadores que armazenam metadados de rede que incluem as VLANs a serem usadas para fornecer os nós do trabalhador futuros no conjunto. Você pode desejar mudar a configuração da conectividade VLAN do seu cluster posteriormente, em casos como os seguintes.
{: shortdesc}

* As VLANs do conjunto de trabalhadores em uma zona fica sem capacidade e você precisa fornecer uma nova VLAN para seus nós do trabalhador do cluster usarem.
* Você tem um cluster com nós do trabalhador que estão em VLANs públicas e privadas, mas você deseja mudar para um [cluster somente privado](/docs/containers?topic=containers-plan_clusters#private_clusters).
* Você tem um cluster somente privado, mas deseja alguns nós do trabalhador, como um conjunto de trabalhadores de [nós de borda](/docs/containers?topic=containers-edge#edge) na VLAN pública para expor seus apps na Internet.

Tentando alterar o terminal em serviço para comunicação do trabalhador principal no lugar? Consulte os tópicos para configurar os terminais em serviço [públicos](#set-up-public-se) e [privados](#set-up-private-se).
{: tip}

Antes de iniciar:
* [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
* Se os nós do trabalhador forem independentes (não como parte de um conjunto de trabalhadores), [atualize-os para os conjuntos de trabalhadores](/docs/containers?topic=containers-update#standalone_to_workerpool).

Para mudar as VLANs que um conjunto de trabalhadores usa para fornecer nós do trabalhador:

1. Liste os nomes dos conjuntos de trabalhadores em seu cluster.
  ```
  ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. Determine as zonas para um dos conjuntos de trabalhadores. Na saída, procure o campo **Zonas**.
  ```
  ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <pool_name>
  ```
  {: pre}

3. Para cada zona que você localizou na etapa anterior, obtenha uma VLAN pública e privada disponível que sejam compatíveis entre si.

  1. Verifique as VLANs públicas e privadas disponíveis que estão listadas em **Tipo** na saída.
     ```
     ibmcloud ks vlans --zone <zone>
     ```
     {: pre}

  2. Verifique se as VLANs públicas e privadas na zona são compatíveis. Para serem compatíveis, o **Roteador** deve ter o mesmo ID de pod. Nesta saída de exemplo, os IDs do pod **Roteador** correspondem: `01a` e `01a`. Se um ID de pod fosse `01a` e o outro fosse `02a`, não seria possível configurar esses IDs de VLAN pública e privada para o conjunto de trabalhadores.
     ```
     ID        Name   Number   Type      Router         Supports Virtual Workers
    229xxxx          1234     private   bcr01a.dal12   true
    229xxxx          5678     public    fcr01a.dal12   true
     ```
     {: screen}

  3. Se você precisar pedir uma nova VLAN pública ou privada para a zona, será possível pedir no console do [{{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans) ou use o comando a seguir. Lembre-se de que as VLANs devem ser compatíveis, com os IDs de pod correspondentes do **Roteador** como na etapa anterior. Se você estiver criando um par de novas VLANs públicas e privadas, elas deverão ser compatíveis uma com a outra.
     ```
     ibmcloud sl vlan create -t [public|private] -d <zone> -r <compatible_router>
     ```
     {: pre}

  4. Observe os IDs das VLANs compatíveis.

4. Configure um conjunto de trabalhadores com os novos metadados de rede da VLAN para cada zona. É possível criar um novo conjunto de trabalhadores, ou modificar um conjunto de trabalhadores existente.

  * **Criar um novo conjunto de trabalhadores**: consulte [incluindo nós do trabalhador criando um novo conjunto de trabalhadores](/docs/containers?topic=containers-add_workers#add_pool).

  * **Modificar um conjunto de trabalhadores existente**: configure os metadados de rede do conjunto de trabalhadores para usar a VLAN para cada zona. Os nós do trabalhador que já foram criados no conjunto continuam a usar as VLANs anteriores, mas novos nós do trabalhador no conjunto usam novos metadados de VLAN que você configurou.

    * Exemplo para incluir VLANs públicas e privadas, como, se você mudar de somente privada para privada e pública:
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

    * Exemplo para incluir apenas uma VLAN privada, como mudar de VLANs públicas e privadas para somente privadas quando há uma [conta ativada para VRF que usa terminais em serviço](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started):
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

5. Inclua nós do trabalhador no conjunto de trabalhadores redimensionando o conjunto.
   ```
   ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <pool_name>  --size-per-zone <number_of_workers_per_zone>
   ```
   {: pre}

   Se você desejar remover nós do trabalhador que usam os metadados de rede anteriores, mude o número de trabalhadores por zona para duplicar a quantia anterior de trabalhadores por zona. Posteriormente nessas etapas, é possível unir, drenar e remover os nós do trabalhador anteriores.
  {: tip}

6. Verifique se os novos nós do trabalhador são criados com o **IP público** e **IP privado** apropriados na saída. Por exemplo, se você mudar o conjunto de trabalhadores de uma VLAN pública e privada para somente privada, os novos nós do trabalhador terão apenas um IP privado. Se você mudar o conjunto de trabalhadores de VLANs somente privadas para públicas e privadas, os novos nós do trabalhador terão IPs públicos e privados.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
   ```
   {: pre}

7. Opcional: remova os nós do trabalhador com os metadados de rede anteriores do conjunto de trabalhadores.
  1. Na saída da etapa anterior, anote o **ID** e o **IP privado** dos nós do trabalhador que você deseja remover do conjunto de trabalhadores.
  2. Marque o nó do trabalhador como não programável em um processo conhecido como bloqueio. Ao bloquear um nó do trabalhador, ele fica indisponível para planejamento futuro do pod.
     ```
     kubectl cordon <worker_private_ip>
     ```
     {: pre}
  3. Verifique se o planejamento do pod está desativado para seu nó do trabalhador.
     ```
     kubectl get nodes
     ```
     {: pre}
     O nó do trabalhador ficará desativado para planejamento do pod se o status exibir **`SchedulingDisabled`**.
  4. Force os pods para que sejam removidos do nó do trabalhador e reprogramados nos nós do trabalhador restantes no cluster.
     ```
     dreno kubectl de dreno < worker_private_ip>
     ```
     {: pre}
     Esse processo pode levar alguns minutos.
  5. Remova o nó do trabalhador. Use o ID do trabalhador que você recuperou anteriormente.
     ```
     ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_name_or_ID>
     ```
     {: pre}
  6. Verifique se o nó do trabalhador foi removido.
     ```
     ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
     ```
     {: pre}

8. Opcional: é possível repetir as etapas de 2 a 7 para cada conjunto de trabalhadores em seu cluster. Depois de concluir essas etapas, todos os nós do trabalhador em seu cluster são configurados com as novas VLANs.

9. Os ALBs padrão em seu cluster ainda estão ligados à VLAN antiga porque seus endereços IP são de uma sub-rede na VLAN. Como os ALBs não podem ser movidos através de VLANs, é possível, em vez disso, [criar ALBs nas novas VLANs e desativar ALBs nas VLANs antigas](/docs/containers?topic=containers-ingress#migrate-alb-vlan).
