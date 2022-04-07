---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-15"

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



# Tópicos populares para {{site.data.keyword.containerlong_notm}}
{: #cs_popular_topics}

Acompanhe o que está acontecendo no {{site.data.keyword.containerlong}}. Aprenda sobre os novos recursos para explorar, um truque para experimentar ou alguns tópicos populares que outros desenvolvedores estão achando úteis agora.
{:shortdesc}



## Tópicos populares em abril de 2019
{: #apr19}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em abril de 2019</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>15 de abril de 2019</td>
<td>[Registrando um nome de host de balanceador de carga de rede (NLB)](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname)</td>
<td>Agora, depois de configurar balanceadores de carga de rede (NLBs) pública para expor seus aplicativos à Internet, é possível criar entradas DNS para os IPs do NLB criando nomes de host. O {{site.data.keyword.Bluemix_notm}} é responsável pela geração e manutenção do certificado SSL curinga para o nome de host. Também é possível configurar monitores TCP/HTTP(S) para verificar o funcionamento dos endereços IP do NLB atrás de cada nome do host.</td>
</tr>
<tr>
<td>8 de abril de 2019</td>
<td>[Terminal do Kubernetes em seu navegador da web (beta)](/docs/containers?topic=containers-cs_cli_install#cli_web)</td>
<td>Se você usar o painel do cluster no console do {{site.data.keyword.Bluemix_notm}} para gerenciar os seus clusters, mas desejar fazer mudanças de configuração mais avançadas rapidamente, agora será possível executar comandos da CLI diretamente de seu navegador da web no Terminal do Kubernetes. Na página de detalhes de um cluster, inicie o Terminal do Kubernetes clicando no botão **Terminal**. Observe que o Terminal do Kubernetes é liberado como um complemento beta e não é destinado ao uso em clusters de produção.</td>
</tr>
<tr>
<td>4 de abril de 2019</td>
<td>[Principais altamente disponíveis em Sydney no momento](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>Quando você [cria um cluster](/docs/containers?topic=containers-clusters#clusters_ui) em um local metro multizona, agora incluindo Sydney, as réplicas do principal do Kubernetes são difundidas entre as zonas para fornecer alta disponibilidade.</td>
</tr>
</tbody></table>

## Tópicos populares em março de 2019
{: #mar19}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em março de 2019</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>21 de março de 2019</td>
<td>Introduzindo Terminais de Serviço Privados para o Cluster do Kubernetes do Kubernetes</td>
<td>Por padrão, o {{site.data.keyword.containerlong_notm}} configura seu cluster com acesso em uma VLAN pública e privada. Anteriormente, se você desejasse um [cluster de VLAN somente privada](/docs/containers?topic=containers-plan_clusters#private_clusters), era necessário configurar um dispositivo de gateway para conectar os nós do trabalhador do cluster ao mestre. Agora, é possível usar o terminal em serviço privado. Com o terminal em serviço privado ativado, todo o tráfego entre os nós do trabalhador e o mestre está na rede privada, sem a necessidade de um dispositivo de gateway. Além disso, o tráfego aumentado de segurança, de entrada e de saída na rede privada é [ilimitado e não cobrado ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud/bandwidth). Ainda é possível manter um terminal em serviço público para acesso seguro ao mestre do Kubernetes na Internet, por exemplo, para executar comandos `kubectl` sem estar na rede privada.<br><br>
Para usar terminais em serviço privados, deve-se ativar o [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) e os [terminais em serviço](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started) para sua conta de infraestrutura do IBM Cloud (SoftLayer). Seu cluster deve executar o Kubernetes versão 1.11 ou mais recente. Se o seu cluster executar uma versão anterior do Kubernetes, [atualize para pelo menos 1.11](/docs/containers?topic=containers-update#update). Para obter mais informações, consulte os links a seguir:<ul>
<li>[ Entendendo a Comunicação Master para o Trabalhador com Terminais de Serviço ](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master)</li>
<li>[ Configurando o terminal em serviço privado ](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se)</li>
<li>[Alternando de terminais em serviço públicos para privados](/docs/containers?topic=containers-cs_network_cluster#migrate-to-private-se)</li>
<li>Se você tiver um firewall na rede privada, [incluindo os endereços IP privados para o {{site.data.keyword.containerlong_notm}}, o {{site.data.keyword.registrylong_notm}} e outros serviços do {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-firewall#firewall_outbound)</li>
</ul>
<p class="important">Se você alternar para um cluster de terminal em serviço somente privado, certifique-se de que seu cluster ainda possa se comunicar com outros serviços do {{site.data.keyword.Bluemix_notm}} que você usar. O [armazenamento definido por software (SDS) do Portworx](/docs/containers?topic=containers-portworx#portworx) e o [dimensionador automático de cluster](/docs/containers?topic=containers-ca#ca) não suportam o terminal de serviço somente privado. Use um cluster com os terminais em serviço público e privado no lugar. O [armazenamento de arquivos baseado em NFS](/docs/containers?topic=containers-file_storage#file_storage) será suportado se seu cluster executar o Kubernetes versão 1.13.4_1513, 1.12.6_1544, 1.11.8_1550, 1.10.13_1551 ou mais recente.</p>
</td>
</tr>
<tr>
<td>07 de março de 2019</td>
<td>[Escalador automático de cluster se move de beta para GA](/docs/containers?topic=containers-ca#ca)</td>
<td>O autoscaler do cluster agora está geralmente disponível. Instale o plug-in Helm e comece a escalar os conjuntos de trabalhadores em seu cluster automaticamente para aumentar ou diminuir o número de nós do trabalhador com base nas necessidades dimensionadas de suas cargas de trabalho planejadas.<br><br>
Precisa de ajuda ou tem feedback sobre o escalador automático de cluster? Se você for um usuário externo, [registre-se para o Slack público ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://bxcs-slack-invite.mybluemix.net/) e poste no canal [#cluster-autoscaler ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-container-service.slack.com/messages/CF6APMLBB). Se você for um IBMista, poste no canal [Slack interno ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-argonauts.slack.com/messages/C90D3KZUL).</td>
</tr>
</tbody></table>




## Tópicos populares em fevereiro de 2019
{: #feb19}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em fevereiro de 2019</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>25 de Fevereiro</td>
<td>Mais controle granular sobre a extração de imagens do {{site.data.keyword.registrylong_notm}}</td>
<td>Agora, ao implementar suas cargas de trabalho em clusters do {{site.data.keyword.containerlong_notm}}, seus contêineres podem extrair imagens dos [novos nomes de domínio `icr.io` para o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-registry_overview#registry_regions). Além disso, é possível usar as políticas de acesso de baixa granularidade no {{site.data.keyword.Bluemix_notm}} IAM para controlar o acesso a suas imagens. Para obter mais informações, consulte [Entendendo como seu cluster está autorizado a extrair imagens](/docs/containers?topic=containers-images#cluster_registry_auth).</td>
</tr>
<tr>
<td>21 de Fevereiro</td>
<td>[ Zona disponível no México ](/docs/containers?topic=containers-regions-and-zones#regions-and-zones)</td>
<td>Bem-vindo ao México (`mex01`) como uma nova zona para clusters na região Sul dos EUA. Se você tiver um firewall, certifique-se de [abrir as portas de firewall](/docs/containers?topic=containers-firewall#firewall_outbound) para essa zona e as outras zonas dentro da região em que seu cluster está.</td>
</tr>
<tr><td>06 de fevereiro de 2019</td>
<td>[ Istio no  {{site.data.keyword.containerlong_notm}} ](/docs/containers?topic=containers-istio)</td>
<td>O Istio on {{site.data.keyword.containerlong_notm}} fornece uma instalação contínua do Istio, atualizações automáticas e gerenciamento de ciclo de vida de componentes de plano de controle do Istio e integração com a criação de log de plataforma e as ferramentas de monitoramento. Com um clique, é possível obter todos os componentes principais do Istio, rastreio adicional, monitoramento e visualização e o aplicativo de amostra BookInfo funcionando. O Istio no {{site.data.keyword.containerlong_notm}} é oferecido como um complemento gerenciado, portanto, o {{site.data.keyword.Bluemix_notm}} mantém automaticamente todos os seus componentes do Istio atualizados.</td>
</tr>
<tr>
<td>06 de fevereiro de 2019</td>
<td>[ Knative no  {{site.data.keyword.containerlong_notm}} ](/docs/containers?topic=containers-knative_tutorial)</td>
<td>Knative é uma plataforma de código aberto que estende as capacidades de Kubernetes para ajudá-lo a criar aplicativos modernos, centrados na origem e sem servidor, em cima do cluster de Kubernetes. O Knative gerenciado no {{site.data.keyword.containerlong_notm}} é um complemento gerenciado que integra o Knative e o Istio diretamente a seu cluster Kubernetes. As versões Knative e Istio no complemento são testadas pela IBM e suportadas para uso no {{site.data.keyword.containerlong_notm}}.</td>
</tr>
</tbody></table>


## Tópicos populares em janeiro de 2019
{: #jan19}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em janeiro de 2019</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>30 de janeiro</td>
<td>Funções de acesso ao serviço {{site.data.keyword.Bluemix_notm}} IAM e namespaces do Kubernetes</td>
<td>O {{site.data.keyword.containerlong_notm}}  agora suporta  {{site.data.keyword.Bluemix_notm}}  funções de acesso de serviço do IAM  [ ](/docs/containers?topic=containers-access_reference#service). Essas funções de acesso de serviço se alinham ao [Kubernetes RBAC](/docs/containers?topic=containers-users#role-binding) para autorizar usuários a executar ações `kubectl` dentro do cluster para gerenciar recursos do Kubernetes, como pods ou implementações. Além disso, é possível [limitar o acesso de usuário a um namespace específico do Kubernetes](/docs/containers?topic=containers-users#platform) dentro do cluster usando as funções de acesso de serviço do {{site.data.keyword.Bluemix_notm}} IAM. As [funções de acesso da plataforma](/docs/containers?topic=containers-access_reference#iam_platform) agora são usadas para autorizar os usuários a executar ações `ibmcloud ks` para gerenciar sua infraestrutura de cluster, como os nós do trabalhador.<br><br>As funções de acesso de serviço do {{site.data.keyword.Bluemix_notm}} IAM são incluídas automaticamente nas {{site.data.keyword.containerlong_notm}} contas e clusters existentes com base nas permissões que seus usuários tinham anteriormente com as funções de acesso da plataforma do IAM. Indo além, é possível usar o IAM para criar grupos de acesso, incluir usuários em grupos de acesso e designar as funções da plataforma de grupo ou de acesso de serviço. Para obter mais informações, consulte o blog [Introduzindo funções de serviço e namespaces no IAM para obter controle mais granular de acesso ao cluster ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/).</td>
</tr>
<tr>
<td>8 de janeiro</td>
<td>[ Beta de visualização do autocaler do cluster beta ](/docs/containers?topic=containers-ca#ca)</td>
<td>Escale os conjuntos de trabalhadores em seu cluster automaticamente para aumentar ou diminuir o número de nós do trabalhador no conjunto de trabalhadores com base nas necessidades de dimensionamento de suas cargas de trabalho planejadas. Para testar esse beta, deve-se solicitar acesso à lista de desbloqueio.</td>
</tr>
<tr>
<td>8 de janeiro</td>
<td>[ {{site.data.keyword.containerlong_notm}}  Diagnostics and Debug Tool ](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)</td>
<td>Às vezes, você acha difícil obter todos os arquivos YAML e outras informações que você precisa para ajudar a solucionar um problema? Experimente o {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool para uma interface gráfica com o usuário que ajuda a reunir informações de cluster, de implementação e de rede.</td>
</tr>
</tbody></table>

## Tópicos populares em dezembro de 2018
{: #dec18}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em dezembro de 2018</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>11 de dezembro</td>
<td>Suporte do SDS com Portworx</td>
<td>Gerencie o armazenamento persistente para seus bancos de dados conteinerizados e outros apps stateful ou compartilhe dados entre os pods em múltiplas zonas com o Portworx. [Portworx ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://portworx.com/products/introduction/) é uma solução de armazenamento definido por software (SDS) altamente disponível que gerencia o armazenamento de bloco local para os nós do trabalhador para criar uma camada de armazenamento persistente unificada para os apps. Usando a replicação de volume de cada volume de nível de contêiner em múltiplos nós do trabalhador, o Portworx assegura a persistência de dados e a acessibilidade de dados entre as zonas. Para obter mais informações, consulte [Armazenando dados em armazenamento definido por software (SDS) com Portworx](/docs/containers?topic=containers-portworx#portworx).    </td>
</tr>
<tr>
<td>6 de dezembro</td>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>Obtenha visibilidade operacional para o desempenho e o funcionamento de seus apps implementando o Sysdig como um serviço de terceiro para os nós do trabalhador para encaminhar métricas para o {{site.data.keyword.monitoringlong}}. Para obter mais informações, consulte [Analisando métricas para um app implementado em um cluster do Kubernetes](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). **Nota**: se você usar o {{site.data.keyword.mon_full_notm}} com clusters que executam o Kubernetes versão 1.11 ou mais recente, nem todas as métricas de contêiner serão coletadas, porque o Sysdig não suporta atualmente o `containerd`.</td>
</tr>
<tr>
<td>4 de Dezembro</td>
<td>[Reservas de recursos do nó do trabalhador](/docs/containers?topic=containers-plan_clusters#resource_limit_node)</td>
<td>Você está implementando tantos apps que está preocupado em exceder a capacidade do cluster? As reservas de recursos do nó do trabalhador e os despejos do Kubernetes protegem a capacidade de cálculo do cluster para que seu cluster tenha recursos suficientes para continuar em execução. Basta incluir mais alguns nós do trabalhador e seus pods começam a reprogramação para eles automaticamente. As reservas de recursos do nó do trabalhador são atualizadas nas [alterações de correção de versão](/docs/containers?topic=containers-changelog#changelog)mais recentes.</td>
</tr>
</tbody></table>

## Tópicos populares em novembro de 2018
{: #nov18}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em novembro de 2018</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>29 de novembro</td>
<td>[Zona disponível em Chennai](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Bem-vindo Chennai, Índia como uma nova zona para clusters na região Norte AP. Se você tiver um firewall, certifique-se de [abrir as portas de firewall](/docs/containers?topic=containers-firewall#firewall) para essa zona e as outras zonas dentro da região em que seu cluster está.</td>
</tr>
<tr>
<td>27 de novembro</td>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>Inclua recursos de gerenciamento de log no cluster implementando LogDNA como um serviço de terceiro para seus nós do trabalhador para gerenciar logs dos contêineres de pod. Para obter mais informações, consulte [Gerenciando logs de cluster do Kubernetes com o {{site.data.keyword.loganalysisfull_notm}} com LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>7 de novembro</td>
<td>Balanceador de carga de rede 2.0 (beta)</td>
<td>Agora, é possível escolher entre o [NLB 1.0 ou 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) para expor de forma segura seus aplicativos de cluster para o público.</td>
</tr>
<tr>
<td>7 de novembro</td>
<td>Kubernetes versão 1.12 disponível</td>
<td>Agora, é possível atualizar ou criar clusters que executam [Kubernetes versão 1.12](/docs/containers?topic=containers-cs_versions#cs_v112)! Clusters 1.12 vêm com os mestres do Kubernetes altamente disponíveis por padrão.</td>
</tr>
<tr>
<td>7 de novembro</td>
<td>Mestres altamente disponíveis</td>
<td>Os mestres altamente disponíveis estão disponíveis para clusters que executam o Kubernetes versão 1.10! Todos os benefícios descritos na entrada anterior para clusters 1.11 se aplicam aos clusters 1.10, assim como as [etapas de preparação](/docs/containers?topic=containers-cs_versions#110_ha-masters) que devem ser executadas.</td>
</tr>
<tr>
<td>1 de novembro</td>
<td>Mestres altamente disponíveis em clusters que executam o Kubernetes versão 1.11</td>
<td>Em uma zona única, seu mestre está altamente disponível e inclui réplicas em hosts físicos separados para seu servidor de API do Kubernetes, um etcd, um planejador e um gerenciador de controlador para proteger contra uma indisponibilidade, como durante uma atualização de cluster. Se o seu cluster estiver em uma zona com capacidade para várias zonas, o seu mestre altamente disponível também será distribuído entre zonas para ajudar a proteger contra uma falha zonal.<br>Para ações que devem ser executadas, consulte [Atualizando para os clusters mestres altamente disponíveis](/docs/containers?topic=containers-cs_versions#ha-masters). Estas ações de preparação se aplicam:<ul>
<li>Se você tiver um firewall ou políticas de rede customizadas do Calico.</li>
<li>Se você estiver usando as portas do host `2040` ou `2041` nos nós do trabalhador.</li>
<li>Se você usou o endereço IP do cluster mestre para acesso no cluster ao mestre.</li>
<li>Se você tiver automação que chame a API ou a CLI do Calico (`calicoctl`), tal como para criar políticas do Calico.</li>
<li>Se você usar políticas de rede do Kubernetes ou do Calico para controlar o acesso de egresso do pod ao mestre.</li></ul></td>
</tr>
</tbody></table>

## Tópicos populares em outubro de 2018
{: #oct18}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em outubro de 2018</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>25 de Outubro</td>
<td>[ Zona disponível em Milão ](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Bem-vindo Milão, Itália, como uma nova zona para os clusters pagos na região central da UE. Anteriormente, Milão estava disponível somente para clusters grátis. Se você tiver um firewall, certifique-se de [abrir as portas de firewall](/docs/containers?topic=containers-firewall#firewall) para essa zona e as outras zonas dentro da região em que seu cluster está.</td>
</tr>
<tr>
<td>22 de Outubro</td>
<td>[ Nova posição de multisona de Londres,  ` lon05 ` ](/docs/containers?topic=containers-regions-and-zones#zones)</td>
<td>O local metro multizona de Londres substitui a zona `lon02` pela nova zona `lon05`, que contém mais recursos de infraestrutura do que a `lon02`. Crie novos clusters multizona com  ` lon05 `. Os clusters existentes com `lon02` são suportados, mas os novos clusters de múltiplas zonas devem usar `lon05` no lugar.</td>
</tr>
<tr>
<td>05 de outubro</td>
<td>Integração com o  {{site.data.keyword.keymanagementservicefull}}  (beta)</td>
<td>É possível criptografar os segredos do Kubernetes em seu cluster [ativando o {{site.data.keyword.keymanagementserviceshort}} (beta)](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
<tr>
<td>04 de outubro</td>
<td>Agora, o [{{site.data.keyword.registrylong}} está integrado ao {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)](/docs/services/Registry?topic=registry-iam#iam)</td>
<td>É possível usar o {{site.data.keyword.Bluemix_notm}} IAM para controlar o acesso a seus recursos de registro, como puxar, enviar por push e construir imagens. Ao criar um cluster, você também cria um token de registro para que o cluster possa trabalhar com seu registro. Portanto, você precisa da função **Administrador** de gerenciamento de plataforma do registro de nível de conta para criar um cluster. Para ativar o {{site.data.keyword.Bluemix_notm}} IAM para sua conta de registro, veja [Ativando a aplicação de política para usuários existentes](/docs/services/Registry?topic=registry-user#existing_users).</td>
</tr>
<tr>
<td>01 de outubro</td>
<td>[Grupos de recursos](/docs/containers?topic=containers-users#resource_groups)</td>
<td>É possível usar grupos de recursos para separar seus recursos do {{site.data.keyword.Bluemix_notm}} em pipelines, departamentos ou outros agrupamentos para ajudar a designar o acesso e medir o uso. Agora, o {{site.data.keyword.containerlong_notm}} suporta a criação de clusters no grupo `default` ou em qualquer outro grupo de recursos que você criar!</td>
</tr>
</tbody></table>

## Tópicos populares em setembro de 2018
{: #sept18}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em setembro de 2018</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>25 de setembro</td>
<td>[Novas zonas disponíveis](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Agora você tem ainda mais opções de onde implementar seus apps!
<ul><li>Bem-vindo a San Jose como duas novas zonas na região sul dos EUA, `sjc03` e `sjc04`. Se você tiver um firewall, certifique-se de [abrir as portas do firewall](/docs/containers?topic=containers-firewall#firewall) para essa zona e as outras dentro da região em que seu cluster estiver.</li>
<li>Com duas novas zonas `tok04` e `tok05`, agora é possível [criar clusters de múltiplas zonas](/docs/containers?topic=containers-plan_clusters#multizone) em Tóquio na região Norte de AP.</li></ul></td>
</tr>
<tr>
<td>05 de setembro</td>
<td>[ Zona disponível em Oslo ](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Bem-vindo Oslo, Noruega, como uma nova zona na região Central da UE. Se você tiver um firewall, certifique-se de [abrir as portas do firewall](/docs/containers?topic=containers-firewall#firewall) para essa zona e as outras dentro da região em que seu cluster estiver.</td>
</tr>
</tbody></table>

## Tópicos populares em agosto de 2018
{: #aug18}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em agosto de 2018</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>31 de agosto</td>
<td>{{site.data.keyword.cos_full_notm}}  está agora integrado com o  {{site.data.keyword.containerlong}}</td>
<td>Use solicitações de volume persistente (PVC) nativas do Kubernetes para provisionar o {{site.data.keyword.cos_full_notm}} para seu cluster. O {{site.data.keyword.cos_full_notm}} é melhor usado para cargas de trabalho de leitura intensiva e se você deseja armazenar dados entre múltiplas zonas em um cluster de múltiplas zonas. Comece [criando uma instância de serviço do {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#create_cos_service) e [instalando o plug-in do {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#install_cos) em seu cluster. </br></br>Não tem certeza de qual solução de armazenamento pode ser a ideal para você? Inicie [aqui](/docs/containers?topic=containers-storage_planning#choose_storage_solution) para analisar seus dados e escolher a solução de armazenamento apropriada para seus dados. </td>
</tr>
<tr>
<td>14 de agosto</td>
<td>Atualizar seus clusters para o Kubernetes versões 1.11 para designar prioridade do pod</td>
<td>Depois de atualizar seu cluster para o [Kubernetes versão 1.11](/docs/containers?topic=containers-cs_versions#cs_v111), é possível aproveitar novos recursos, como aumento de desempenho do tempo de execução do contêiner com `containerd` ou [designando a prioridade do pod](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
</tbody></table>

## Tópicos populares em julho de 2018
{: #july18}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em julho de 2018</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>30 de julho</td>
<td>[ Traga seu próprio controlador de Ingresso ](/docs/containers?topic=containers-ingress#user_managed)</td>
<td>Você tem segurança muito específica ou outros requisitos customizados para o controlador de ingresso do seu cluster? Se sim, você pode desejar executar seu próprio controlador de ingresso em vez do padrão.</td>
</tr>
<tr>
<td>10 de julho</td>
<td>Introducing multizone clusters</td>
<td>Deseja melhorar a disponibilidade do cluster? Agora, é possível estender seu cluster em múltiplas zonas em áreas metro selecionadas. Para obter mais informações, veja [Criando clusters multizona no {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-plan_clusters#multizone).</td>
</tr>
</tbody></table>

## Tópicos populares em junho de 2018
{: #june18}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em junho de 2018</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>13 de junho</td>
<td>O nome do comando da CLI `bx` está sendo mudado para a CLI `ic`</td>
<td>Ao fazer download da versão mais recente da CLI do {{site.data.keyword.Bluemix_notm}}, agora você executa comandos usando o prefixo `ic` em vez de `bx`. Por exemplo, liste seus clusters executando `ibmcloud ks clusters`.</td>
</tr>
<tr>
<td>12 de junho</td>
<td>[ Políticas de segurança do Pod ](/docs/containers?topic=containers-psp)</td>
<td>Para clusters que executam o Kubernetes 1.10.3 ou mais recente, é possível
configurar políticas de segurança de pod para autorizar quem pode criar e atualizar pods no {{site.data.keyword.containerlong_notm}}.</td>
</tr>
<tr>
<td>06 de junho</td>
<td>[Suporte do TLS para o subdomínio curinga do Ingresso fornecido pela IBM](/docs/containers?topic=containers-ingress#wildcard_tls)</td>
<td>Para clusters criados em ou após 6 de junho de 2018, o certificado TLS de subdomínio do Ingresso fornecido pela IBM agora é um certificado curinga e pode ser usado para o subdomínio curinga registrado. Para clusters criados antes de 6 de junho de 2018, o certificado do TLS será atualizado para um certificado curinga quando o certificado do TLS atual for renovado.</td>
</tr>
</tbody></table>

## Tópicos populares em maio de 2018
{: #may18}


<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em maio de 2018</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>24 de maio</td>
<td>[Novo Ingress subdomain formato](/docs/containers?topic=containers-ingress)</td>
<td>Os clusters criados após 24 de maio são designados a um subdomínio do Ingresso em um novo formato: <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>. Quando você usa o Ingresso para expor os seus apps, é possível usar o novo subdomínio para acessar os seus apps na Internet.</td>
</tr>
<tr>
<td>14 de maio</td>
<td>[Atualização: implementar cargas de trabalho na GPU bare metal no mundo todo](/docs/containers?topic=containers-app#gpu_app)</td>
<td>Se você tem um [tipo de máquina de unidade de processamento de gráfico (GPU) bare metal](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node) em seu cluster, é possível planejar apps matematicamente intensivos. O nó do trabalhador de GPU pode processar a carga de trabalho de seu app na CPU e GPU para aumentar o desempenho.</td>
</tr>
<tr>
<td>Maio 03</td>
<td>[Contêiner Image Security Enforcement (beta)](/docs/services/Registry?topic=registry-security_enforce#security_enforce)</td>
<td>Sua equipe precisa de uma ajuda extra para saber qual imagem puxar em seus contêineres de app? Experimente o beta do Container Image Security Enforcement para verificar imagens de contêiner antes de implementá-las. Disponível para clusters que executam o Kubernetes versão 1.9 ou mais recente.</td>
</tr>
<tr>
<td>Maio 01</td>
<td>[Implementar o painel do Kubernetes por meio do console](/docs/containers?topic=containers-app#cli_dashboard)</td>
<td>Alguma vez você desejou acessar o painel do Kubernetes com um clique? Confira o botão **Painel do Kubernetes** no console do {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
</tbody></table>




## Tópicos populares em abril de 2018
{: #apr18}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em abril de 2018</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>17 de abril</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>Instale o [plug-in](/docs/containers?topic=containers-block_storage#install_block) do {{site.data.keyword.Bluemix_notm}} Block Storage para salvar dados persistentes no armazenamento de bloco. Em seguida, é possível [criar um novo](/docs/containers?topic=containers-block_storage#add_block) armazenamento de bloco ou [usar um existente](/docs/containers?topic=containers-block_storage#existing_block) para seu cluster.</td>
</tr>
<tr>
<td>13 de abril</td>
<td>[Novo tutorial para migrar apps Cloud Foundry para clusters](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)</td>
<td>Você tem um app Cloud Foundry? Saiba como implementar o mesmo código desse app em um contêiner executado em um cluster do Kubernetes.</td>
</tr>
<tr>
<td>05 de abril</td>
<td>[Filtrando logs](/docs/containers?topic=containers-health#filter-logs)</td>
<td>Filtrar logs específicos de serem encaminhados. Os logs podem ser filtrados em relação a um namespace, um nome de contêiner, um nível de log e uma sequência de mensagem específicos.</td>
</tr>
</tbody></table>

## Tópicos populares em março de 2018
{: #mar18}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em março de 2018</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>16 de março</td>
<td>[Provisionar um cluster bare metal com Cálculo confiável](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node)</td>
<td>Crie um cluster bare metal que execute o [Kubernetes versão 1.9](/docs/containers?topic=containers-cs_versions#cs_v19) ou mais recente e ative o Cálculo confiável para verificar os nós do trabalhador com relação à violação.</td>
</tr>
<tr>
<td>14 de março</td>
<td>[Conexão segura com o {{site.data.keyword.appid_full}}](/docs/containers?topic=containers-supported_integrations#appid)</td>
<td>Aprimore seus apps que estão em execução no {{site.data.keyword.containerlong_notm}} requerendo que os usuários se conectem.</td>
</tr>
<tr>
<td>13 de março</td>
<td>[Zona disponível em São Paulo](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Bem-vindo São Paulo, Brasil, como uma nova zona na região Sul dos EUA. Se você tiver um firewall, certifique-se de [abrir as portas do firewall](/docs/containers?topic=containers-firewall#firewall) para essa zona e as outras dentro da região em que seu cluster estiver.</td>
</tr>
</tbody></table>

## Tópicos populares em fevereiro de 2018
{: #feb18}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em fevereiro de 2018</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td>27 de fevereiro</td>
<td>Imagens de Hardware Virtual Machine (HVM) para nós do trabalhador</td>
<td>Aumente o desempenho de E/S de suas cargas de trabalho com imagens de HVM. Realize a ativação em cada nó do trabalhador existente usando o [comando](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) `ibmcloud ks worker-reload` ou o [comando](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>26 de fevereiro</td>
<td>[Ajuste automático de escala do KubeDNS](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>O KubeDNS agora é escalado com seu cluster conforme ele cresce. É possível ajustar as razões de ajuste de escala usando o comando a seguir: `kubectl -n kube-system edit cm kube-dns-autoscaler`.</td>
</tr>
<tr>
<td>23 de fevereiro</td>
<td>Visualize o console da web para [criação de log](/docs/containers?topic=containers-health#view_logs) e [métricas](/docs/containers?topic=containers-health#view_metrics)</td>
<td>Visualize facilmente os dados de log e de métrica em seu cluster e seus componentes com uma UI da Web melhorada. Veja suas páginas de detalhes do cluster para obter acesso.</td>
</tr>
<tr>
<td>20 de fevereiro</td>
<td>Imagens criptografados e [conteúdo assinado e confiável](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent)</td>
<td>No {{site.data.keyword.registryshort_notm}}, é possível assinar e criptografar imagens para assegurar a sua integridade ao armazenar as imagens em seu namespace de registro. Execute suas instâncias de contêiner usando somente conteúdo confiável.</td>
</tr>
<tr>
<td>19 de fevereiro</td>
<td>[Configurar a VPN IPSec do strongSwan](/docs/containers?topic=containers-vpn#vpn-setup)</td>
<td>Implemente rapidamente o gráfico Helm da VPN IPSec strongSwan para conectar seu cluster do {{site.data.keyword.containerlong_notm}} de forma segura a seu data center no local sem um Virtual Router Appliance.</td>
</tr>
<tr>
<td>14 de fevereiro</td>
<td>[ Zona disponível em Seul ](/docs/containers?topic=containers-regions-and-zones)</td>
<td>No prazo dos Jogos Olímpicos, implemente um cluster do Kubernetes para Seul na região Norte AP. Se você tiver um firewall, certifique-se de [abrir as portas do firewall](/docs/containers?topic=containers-firewall#firewall) para essa zona e as outras dentro da região em que seu cluster estiver.</td>
</tr>
<tr>
<td>08 de fevereiro</td>
<td>[Atualizar o Kubernetes 1.9](/docs/containers?topic=containers-cs_versions#cs_v19)</td>
<td>Revise as mudanças a serem feitas em seus clusters antes de atualizar para o Kubernetes 1.9.</td>
</tr>
</tbody></table>

## Tópicos populares em janeiro de 2018
{: #jan18}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Tópicos populares para contêineres e clusters Kubernetes em janeiro de 2018</caption>
<thead>
<th>Data</th>
<th>Título (Title)</th>
<th>Descrição</th>
</thead>
<tbody>
<td>25 de janeiro</td>
<td>[Registro global disponível](/docs/services/Registry?topic=registry-registry_overview#registry_regions)</td>
<td>Com o {{site.data.keyword.registryshort_notm}}, você pode usar o `registry.bluemix.net` global para puxar imagens públicas fornecidas pela IBM.</td>
</tr>
<tr>
<td>23 de janeiro</td>
<td>[Zonas disponíveis em Singapura e Montreal, CA](/docs/containers?topic=containers-regions-and-zones)</td>
<td>Singapura e Montreal são zonas disponíveis nas regiões Norte de AP e Leste dos EUA do {{site.data.keyword.containerlong_notm}}. Se você tiver um firewall, certifique-se de [abrir as portas de firewall](/docs/containers?topic=containers-firewall#firewall) para essas zonas e as outras dentro da região em que seu cluster está.</td>
</tr>
<tr>
<td>08 de janeiro</td>
<td>[Enhanced sabores disponíveis](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)</td>
<td>Os tipos de máquina virtual de série 2 incluem armazenamento SSD local e criptografia de disco. [Mova suas cargas de trabalho](/docs/containers?topic=containers-update#machine_type) para esses tipos para melhor desempenho e estabilidade.</td>
</tr>
</tbody></table>

## Bate-papo com desenvolvedores com os mesmos interesses no Slack
{: #slack}

É possível ver sobre o que os outros estão falando e fazer as suas próprias perguntas no [{{site.data.keyword.containerlong_notm}} Slack. ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-container-service.slack.com)
{:shortdesc}

Se você não estiver usando um IBMid para a sua conta do {{site.data.keyword.Bluemix_notm}}, [solicite um convite](https://bxcs-slack-invite.mybluemix.net/) para essa Folga.
{: tip}
