---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

keywords: kubernetes, iks, compliance, security standards

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
{:download: .download}
{:preview: .preview}
{:faq: data-hd-content-type='faq'}


# Perguntas frequentes
{: #faqs}

## O que é Kubernetes?
{: #kubernetes}
{: faq}

O Kubernetes é uma plataforma de software livre para gerenciar cargas de trabalho e serviços conteinerizados em múltiplos hosts, que oferece ferramentas de gerenciamento para implementar, automatizar, monitorar e escalar apps conteinerizados com intervenção manual mínima ou até nenhuma. Todos os contêineres que compõem seu microsserviço são agrupados em pods, uma unidade lógica para assegurar fácil gerenciamento e descoberta. Esses pods são executados em hosts de cálculo que são gerenciados em um cluster do Kubernetes que é móvel, extensível e com capacidade de recuperação automática em caso de falhas.
{: shortdesc}

Para obter mais informações sobre o Kubernetes, veja a [documentação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/home/?path=users&persona=app-developer&level=foundational).

## Como o IBM Cloud Kubernetes Service funciona?
{: #kubernetes_service}
{: faq}

Com o {{site.data.keyword.containerlong_notm}}, é possível criar seu próprio cluster do Kubernetes para implementar e gerenciar apps conteinerizados no {{site.data.keyword.Bluemix_notm}}. Seus apps conteinerizados são hospedados em hosts de cálculo de infraestrutura do IBM Cloud (SoftLayer) que são chamados nós do trabalhador. É possível optar por provisionar seus hosts de cálculo como [máquinas virtuais](/docs/containers?topic=containers-planning_worker_nodes#vm) com recursos compartilhados ou dedicados ou como [máquinas bare metal](/docs/containers?topic=containers-planning_worker_nodes#bm) que podem ser otimizadas para uso de armazenamento (SDS) definido por software e GPU. Os nós do trabalhador são controlados por um mestre do Kubernetes altamente disponível que é configurado, monitorado e gerenciado pela IBM. É possível usar a API ou CLI do {{site.data.keyword.containerlong_notm}} para trabalhar com seus recursos de infraestrutura do cluster e a API ou a CLI do Kubernetes para gerenciar suas implementações e serviços.

Para obter mais informações sobre como os recursos de cluster estão configurados, consulte a [Arquitetura de serviço](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#architecture). Para localizar uma lista de recursos e benefícios, consulte [Por que {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_ov#cs_ov)?

## Por que devo usar o IBM Cloud Kubernetes Service?
{: #faq_benefits}
{: faq}

O {{site.data.keyword.containerlong_notm}} é uma oferta do Kubernetes gerenciada que entrega ferramentas poderosas, uma experiência de usuário intuitiva e segurança integrada para entrega rápida de apps que é possível ligar aos serviços de nuvem relacionados ao IBM Watson®, IA, IoT, DevOps, segurança e análise de dados. Como um provedor do Kubernetes certificado, o {{site.data.keyword.containerlong_notm}} fornece planejamento inteligente, capacidade de recuperação automática, ajuste de escala horizontal, descoberta de serviço e balanceamento de carga, lançamentos automatizados e retrocessos e gerenciamento de segredo e configuração. O serviço também tem recursos avançados de gerenciamento de cluster simplificado, políticas de segurança e isolamento de contêiner, a capacidade de projetar seu próprio cluster e ferramentas operacionais integradas para consistência na implementação.

Para obter uma visão geral detalhada de recursos e benefícios, consulte [Por que {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_ov#cs_ov)?

## O serviço vem com um mestre do Kubernetes gerenciado e nós do trabalhador?
{: #managed_master_worker}
{: faq}

Cada cluster do Kubernetes no {{site.data.keyword.containerlong_notm}} é controlado por um mestre do Kubernetes dedicado que é gerenciado pela IBM em uma conta de infraestrutura do {{site.data.keyword.Bluemix_notm}} da IBM. O mestre do Kubernetes, incluindo todos os componentes principais, recursos de cálculo, de rede e de armazenamento, é monitorado continuamente pelos IBM Site Reliability Engineers (SREs). Os SREs aplicam os padrões de segurança mais recentes, detectam e corrigem atividades maliciosas e trabalham para assegurar a confiabilidade e disponibilidade do {{site.data.keyword.containerlong_notm}}. Os complementos, como Fluentd para criação de log, que são instalados automaticamente quando você provisiona o cluster são atualizados automaticamente pela IBM. No entanto, é possível optar por desativar atualizações automáticas para alguns complementos e atualizá-las manualmente separadamente dos nós principal e do trabalhador. Para obter mais informações, consulte [Atualizando complementos de cluster](/docs/containers?topic=containers-update#addons).

Periodicamente, o Kubernetes libera [atualizações principais, secundárias ou de correção](/docs/containers?topic=containers-cs_versions#version_types). Essas atualizações podem afetar a versão do servidor da API do Kubernetes ou outros componentes em seu mestre do Kubernetes. A IBM atualiza automaticamente a versão de correção, mas deve-se atualizar as versões principais e secundárias. Para obter mais informações, consulte [Atualizando o mestre do Kubernetes](/docs/containers?topic=containers-update#master).

Os nós do trabalhador em clusters padrão são provisionados para a sua conta de infraestrutura do {{site.data.keyword.Bluemix_notm}}. Os nós do trabalhador são dedicados à sua conta e você é responsável por solicitar atualizações oportunas aos nós do trabalhador para assegurar que o S.O. do nó do trabalhador e os componentes do {{site.data.keyword.containerlong_notm}} apliquem as atualizações e correções de segurança mais recentes. As atualizações e correções de segurança são disponibilizadas pelo IBM Site Reliability Engineers (SREs) que monitora continuamente a imagem do Linux que está instalada em seus nós do trabalhador para detectar vulnerabilidades e problemas de conformidade de segurança. Para obter mais informações, consulte [Atualizando nós do trabalhador](/docs/containers?topic=containers-update#worker_node).

## Os nós principal e do trabalhador do Kubernetes são altamente disponíveis?
{: #faq_ha}
{: faq}

A arquitetura e a infraestrutura do {{site.data.keyword.containerlong_notm}} são projetadas para assegurar a confiabilidade, a latência de processamento baixo e um tempo de atividade máximo do serviço. Por padrão, cada cluster no {{site.data.keyword.containerlong_notm}} é configurado com diversas instâncias de principal do Kubernetes para garantir a disponibilidade e a acessibilidade de seus recursos de cluster, mesmo se uma ou mais instâncias de seu principal do Kubernetes estiverem indisponíveis.

É possível tornar seu cluster ainda mais altamente disponível e proteger seu app de um tempo de inatividade, difundindo suas cargas de trabalho em múltiplos nós do trabalhador em múltiplas zonas de uma região. Essa configuração é chamada de [cluster de várias zonas](/docs/containers?topic=containers-ha_clusters#multizone) e assegura que seu app esteja acessível, mesmo que um nó do trabalhador ou uma zona inteira não esteja disponível.

Para proteger contra uma falha de região inteira, crie [múltiplos clusters e difunda-os entre as regiões do {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ha_clusters#multiple_clusters). Ao configurar um balanceador de carga de rede (NLB) para seus clusters, é possível obter o balanceamento de carga entre regiões e a rede entre regiões para seus clusters.

Se você tiver dados que devem estar disponíveis, mesmo que ocorra uma indisponibilidade, certifique-se de armazenar seus dados no [armazenamento persistente](/docs/containers?topic=containers-storage_planning#storage_planning).

Para obter mais informações sobre como obter alta disponibilidade para seu cluster, veja [Alta disponibilidade para o {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ha#ha).

## Quais opções eu tenho para proteger meu cluster?
{: #secure_cluster}
{: faq}

É possível usar recursos de segurança integrados no {{site.data.keyword.containerlong_notm}} para proteger os componentes em seu cluster, seus dados e implementações de app para assegurar a conformidade de segurança e a integridade de dados. Use esses recursos para proteger seu servidor de API do Kubernetes, armazenamento de dados etcd, nó do trabalhador, rede, armazenamento, imagens e implementações contra ataques maliciosos. Também é possível alavancar a criação de log integrada e as ferramentas de monitoramento para detectar ataques maliciosos e padrões de uso suspeitos.

Para obter mais informações sobre os componentes de seu cluster e como é possível proteger cada componente, consulte [Segurança para {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-security#security).

## Quais políticas de acesso eu forneço aos meus usuários do cluster?
{: #faq_access}
{: faq}

O {{site.data.keyword.containerlong_notm}} usa o {{site.data.keyword.iamshort}} (IAM) para conceder acesso aos recursos de cluster por meio de funções da plataforma do IAM e políticas de controle de acesso baseado na função (RBAC) do Kubernetes por meio de funções do serviço do IAM. Para obter mais informações sobre os tipos de políticas de acesso, consulte [Selecionar a política de acesso e a função corretas para seus usuários](/docs/containers?topic=containers-users#access_roles).
{: shortdesc}

As políticas de acesso que você designa aos usuários variam dependendo do que você deseja que seus usuários sejam capazes de fazer. É possível localizar mais informações sobre quais funções autorizam quais tipos de ações na [Página de referência de acesso de usuário](/docs/containers?topic=containers-access_reference) ou nos links da tabela a seguir. Para obter as etapas para designar políticas, consulte [Concedendo aos usuários o acesso ao seu cluster por meio do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform).

| Caso de uso | Exemplo de funções e escopo |
| --- | --- |
| Auditor do app | [Função da plataforma Visualizador para um cluster, região ou grupo de recursos](/docs/containers?topic=containers-access_reference#view-actions), [Função do serviço Leitor para um cluster, região ou grupo de recursos](/docs/containers?topic=containers-access_reference#service). |
| Desenvolvedores de aplicativo | [Função da plataforma Editor para um cluster](/docs/containers?topic=containers-access_reference#editor-actions), [Função do serviço Gravador com escopo definido para um namespace](/docs/containers?topic=containers-access_reference#service), [Função do espaço Desenvolvedor do Cloud Foundry](/docs/containers?topic=containers-access_reference#cloud-foundry). |
| Faturamento | [Função da plataforma Visualizador para um cluster, região ou grupo de recursos](/docs/containers?topic=containers-access_reference#view-actions). |
| Criar um cluster | Permissões no nível da conta para credenciais de infraestrutura de Superusuário, função da plataforma Administrador para o {{site.data.keyword.containerlong_notm}} e função da plataforma Administrador para o {{site.data.keyword.registrylong_notm}}. Para obter mais informações, consulte [Preparando para criar clusters](/docs/containers?topic=containers-clusters#cluster_prepare).|
| Administrador de cluster | [Função da plataforma Administrador para um cluster](/docs/containers?topic=containers-access_reference#admin-actions), [Função do serviço Gerenciador sem escopo definido para um namespace (para o cluster inteiro)](/docs/containers?topic=containers-access_reference#service).|
| Operador do DevOps | [Função da plataforma Operador para um cluster](/docs/containers?topic=containers-access_reference#operator-actions), [Função do serviço Gravador sem escopo definido para um namespace (para o cluster inteiro)](/docs/containers?topic=containers-access_reference#service), [Função do espaço Desenvolvedor do Cloud Foundry](/docs/containers?topic=containers-access_reference#cloud-foundry).  |
| Operador ou engenheiro de confiabilidade do site | [Função da plataforma Administrador para um cluster, região ou grupo de recursos](/docs/containers?topic=containers-access_reference#admin-actions), [Função do serviço Leitor para um cluster ou região](/docs/containers?topic=containers-access_reference#service) ou [Função do serviço Gerenciador para todos os namespaces do cluster](/docs/containers?topic=containers-access_reference#service) para poder usar os comandos `kubectl top nodes,pods`. |
{: caption="Tipos de funções que você pode designar para atender a diferentes casos de uso." caption-side="top"}

## Onde posso localizar uma lista de boletins de segurança que afetam meu cluster?
{: #faq_security_bulletins}
{: faq}

Se as vulnerabilidades forem localizadas no Kubernetes, o Kubernetes liberará CVEs em boletins de segurança para informar aos usuários e para descrever as ações que os usuários devem tomar para corrigir a vulnerabilidade. Os boletins de segurança do Kubernetes que afetam os usuários do {{site.data.keyword.containerlong_notm}} ou a plataforma do {{site.data.keyword.Bluemix_notm}} são publicados no [boletim de segurança do {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/status?component=containers-kubernetes&selected=security).

Alguns CVEs requerem a atualização de correção mais recente para uma versão do Kubernetes que pode ser instalada como parte do [processo de atualização de cluster](/docs/containers?topic=containers-update#update) regular no {{site.data.keyword.containerlong_notm}}. Certifique-se de aplicar as correções de segurança no tempo para proteger seu cluster de ataques maliciosos. Para obter informações sobre o que está incluído em uma correção de segurança, consulte [log de mudanças de versão](/docs/containers?topic=containers-changelog#changelog).

## O serviço oferece suporte para bare metal e GPU?
{: #bare_metal_gpu}
{: faq}

Sim, é possível provisionar seu nó do trabalhador como um servidor bare metal físico de único locatário. Os servidores bare metal vêm com benefícios de alto desempenho para cargas de trabalho, como dados, AI e GPU. Além disso, todos os recursos de hardware são dedicados a suas cargas de trabalho, para que você não tenha que se preocupar com "vizinhos barulhentos".

Para obter mais informações sobre os tipos de bare metal disponíveis e como o bare metal é diferente das máquinas virtuais, consulte [Máquinas físicas (bare metal)](/docs/containers?topic=containers-planning_worker_nodes#bm).

## Quais versões do Kubernetes o serviço suporta?
{: #supported_kube_versions}
{: faq}

O {{site.data.keyword.containerlong_notm}} suporta simultaneamente múltiplas versões do Kubernetes. Quando uma versão mais recente (n) é liberada, as versões até 2 atrás (n-2) são suportadas. As versões com mais de 2 atrás do mais recente (n-3) são descontinuadas primeiro e depois não suportadas. As versões a seguir são suportadas atualmente:

*   Mais recente: 1.14.2
*   Padrão: 1.13.6
*   Outro: 1.12.9

Para obter mais informações sobre versões suportadas e ações de atualização que devem ser executadas para mover de uma versão para outra, consulte [Informações de versão e ações de atualização](/docs/containers?topic=containers-cs_versions#cs_versions).

## Onde o serviço está disponível?
{: #supported_regions}
{: faq}

{{site.data.keyword.containerlong_notm}} está disponível no mundo todo. É possível criar clusters padrão em cada região suportada do {{site.data.keyword.containerlong_notm}}. Os clusters grátis estão disponíveis somente em regiões selecionadas.

Para obter mais informações sobre regiões suportadas, consulte [Locais](/docs/containers?topic=containers-regions-and-zones#regions-and-zones).

## Quais normas o serviço obedece?
{: #standards}
{: faq}

O {{site.data.keyword.containerlong_notm}} implementa controles equivalentes aos padrões a seguir:
- EU-US Privacy Shield e Swiss-US Privacy Shield Framework
- Health Insurance Portability and Accountability Act (HIPAA)
- Service Organization Control standards (SOC 1, SOC 2 Tipo 1)
- International Standard on Assurance Engagements 3402 (ISAE 3402), Assurance Reports on Controls at a Service Organization
- International Organization for Standardization (ISO 27001, ISO 27017, ISO 27018)
- Payment Card Industry Data Security Standard (PCI DSS)

## Posso usar o IBM Cloud e outros serviços com meu cluster?
{: #faq_integrations}
{: faq}

É possível incluir serviços de plataforma e infraestrutura do {{site.data.keyword.Bluemix_notm}}, bem como serviços de fornecedores de terceiros para seu cluster do {{site.data.keyword.containerlong_notm}} a fim de ativar a automação, melhorar a segurança ou aprimorar os recursos de monitoramento e criação de log no cluster.

Para obter uma lista de serviços suportados, consulte [Integrando serviços](/docs/containers?topic=containers-supported_integrations#supported_integrations).

## Posso conectar meu cluster no IBM Cloud Public com apps que são executados em meu data center no local?
{: #hybrid}
{: faq}

É possível conectar serviços no {{site.data.keyword.Bluemix_notm}} Public com seu data center no local para criar sua própria configuração de nuvem híbrida. Exemplos de como é possível alavancar o {{site.data.keyword.Bluemix_notm}} Public e Private com apps que são executados em seu data center no local incluem:
- Você cria um cluster com o {{site.data.keyword.containerlong_notm}} no {{site.data.keyword.Bluemix_notm}} Public, mas deseja conectá-lo a um banco de dados local.
- Você cria um cluster do Kubernetes no {{site.data.keyword.Bluemix_notm}} Private em seu próprio data center e implementa apps em seu cluster. No entanto, seu app pode usar um serviço do {{site.data.keyword.ibmwatson_notm}}, como o Tone Analyzer, no {{site.data.keyword.Bluemix_notm}} Public.

Para permitir a comunicação entre os serviços que são executados no {{site.data.keyword.Bluemix_notm}} Public e que são executados localmente, deve-se [configurar uma conexão VPN](/docs/containers?topic=containers-vpn#vpn). Para conectar seu ambiente {{site.data.keyword.Bluemix_notm}} Public ou Dedicated a um ambiente privado do {{site.data.keyword.Bluemix_notm}}, consulte [Usando o {{site.data.keyword.containerlong_notm}} com o {{site.data.keyword.Bluemix_notm}} Private](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp).

Para obter uma visão geral das ofertas suportadas do {{site.data.keyword.containerlong_notm}}, consulte [Comparação de ofertas e suas combinações](/docs/containers?topic=containers-cs_ov#differentiation).

## Posso implementar o IBM Cloud Kubernetes Service em meu próprio data center?
{: #private}
{: faq}

Se não desejar mover seus aplicativos para o {{site.data.keyword.Bluemix_notm}} Public, mas desejar utilizar os recursos do {{site.data.keyword.containerlong_notm}}, será possível instalar o {{site.data.keyword.Bluemix_notm}} Private. O {{site.data.keyword.Bluemix_notm}} Private é uma plataforma de aplicativo que pode ser instalada localmente em suas próprias máquinas e que é possível usar para desenvolver e gerenciar apps conteinerizados no local, em seu próprio ambiente controlado atrás de um firewall.

Para obter mais informações, consulte a [documentação do produto {{site.data.keyword.Bluemix_notm}} Private ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html).

## Do que eu sou cobrado quando eu uso o IBM Cloud Kubernetes Service?
{: #charges}
{: faq}

Com os clusters do {{site.data.keyword.containerlong_notm}}, é possível usar os recursos de cálculo, rede e armazenamento de infraestrutura do IBM Cloud (SoftLayer) com os serviços de plataforma, como o Watson AI ou o Compose Database-as-a-Service. Cada recurso pode resultar em seus próprios encargos que podem ser [fixos, medidos, camadas ou reservados](/docs/billing-usage?topic=billing-usage-charges#charges).
* [Nós do trabalhador](#nodes)
* [ Rede de saída ](#bandwidth)
* [ Endereços IP de sub-rede ](#subnet_ips)
* [Armazenamento](#persistent_storage)
* [{{site.data.keyword.Bluemix_notm}} remotos](#services)
* [Red Hat OpenShift on IBM Cloud](#rhos_charges)

<dl>
<dt id="nodes">Nós do trabalhador</dt>
  <dd><p>Os clusters podem ter dois tipos principais de nós do trabalhador: máquinas virtuais ou físicas (bare metal). A disponibilidade e a precificação do tipo de máquina variam de acordo com a zona na qual você implementa o cluster.</p>
  <p>As <strong>Máquinas virtuais</strong> apresentam maior flexibilidade, tempos de fornecimento mais rápidos e recursos de escalabilidade mais automáticos do que o bare metal, com um preço com custo mais reduzido do que o bare metal. No entanto, as VMs têm um troca de desempenho quando comparadas com especificações de bare metal, como limites de Gbps de rede, RAM e memória e opções de armazenamento. Tenha em mente estes fatores que afetam os custos da VM:</p>
  <ul><li><strong>Compartilhado vs. dedicado</strong>: se você compartilhar o hardware subjacente da VM, o custo será menor que o hardware dedicado, mas os recursos físicos não serão dedicados à sua VM.</li>
  <li><strong>Somente faturamento por hora</strong>: por hora oferece mais flexibilidade para pedir e cancelar VMs rapidamente.
  <li><strong>Horas em camadas por mês</strong>: o faturamento por hora é em camadas. À medida que sua VM permanece pedida para uma camada de horas dentro de um mês de faturamento, a taxa horária que é cobrada diminui. Os níveis de horas são os seguintes: 0 - 150 horas, 151 - 290 horas, 291 - 540 horas e 541+ horas.</li></ul>
  <p>As <strong>Máquinas físicas (bare metal)</strong> produzem benefícios de alto desempenho para cargas de trabalho, como dados, AI e GPU. Além disso, todos os recursos de hardware são dedicados às suas cargas de trabalho, para que você não tenha "vizinhos barulhentos". Tenha em mente estes fatores que afetam seus custos de bare metal:</p>
  <ul><li><strong>Somente faturamento mensal</strong>: todos os bare metals são cobrados mensalmente.</li>
  <li><strong>Processo de pedido mais longo</strong>: depois de pedir ou cancelar um servidor bare metal, o processo é concluído manualmente em sua conta de infraestrutura do IBM Cloud (SoftLayer). Portanto, isso pode levar mais de um dia útil para ser concluído.</li></ul>
  <p>Para obter detalhes sobre as especificações da máquina, veja [Hardware disponível para nós do trabalhador](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes).</p></dd>

<dt id="bandwidth">Largura da banda</dt>
  <dd><p>A largura da banda refere-se à transferência de dados públicos de tráfego de rede de entrada e saída, para e de recursos do {{site.data.keyword.Bluemix_notm}} em data centers no mundo inteiro. A largura da banda pública é cobrada por GB. É possível revisar seu resumo de largura da banda atual efetuando login no [{{site.data.keyword.Bluemix_notm}} console](https://cloud.ibm.com/), no menu ![Menu icon](../icons/icon_hamburger.svg "Menu icon") selecionando **Infra-estrutura Clássica**e, em seguida, selecionando a página **Rede >Largura da Banda >Resumo**.
  <p>Revise os fatores a seguir que afetam os encargos de largura da banda pública:</p>
  <ul><li><strong>Local</strong>: como ocorre com os nós do trabalhador, os encargos variam dependendo da zona em que seus recursos são implementados.</li>
  <li><strong>Largura de banda incluída ou Pré-pago</strong>: as máquinas do nó do trabalhador podem vir com certa alocação de rede de saída por mês, como 250 GB para VMs ou 500 GB para bare metal. Ou, a alocação pode ser Pré-pago, com base no uso de GB.</li>
  <li><strong>Pacotes em camadas</strong>: depois de exceder qualquer largura da banda incluída, você será cobrado de acordo com um esquema de uso em camadas que varia por local. Se você exceder uma dotação de camada, também poderá ser cobrada uma taxa de transferência de dados padrão.</li></ul>
  <p>Para obter mais informações, veja [Pacotes de largura da banda ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud/bandwidth).</p></dd>

<dt id="subnet_ips">Endereços IP de sub-</dt>
  <dd><p>Quando você cria um cluster padrão, uma sub-rede pública móvel com 8 endereços IP públicos é pedida e cobrada em sua conta mensalmente.</p><p>Se você já tiver sub-redes disponíveis em sua conta de infraestrutura, será possível usar essas sub-redes no lugar. Crie o cluster com a [sinalização](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) `--no-subnets` e, em seguida, [reutilize suas sub-redes](/docs/containers?topic=containers-subnets#subnets_custom).</p>
  </dd>

<dt id="persistent_storage">Armazenamento</dt>
  <dd>Quando você provisiona o armazenamento, é possível escolher o tipo de armazenamento e a classe de armazenamento que são corretos para o seu caso de uso. Os encargos variam de acordo com o tipo de armazenamento, o local e as especificações da instância de armazenamento. Algumas soluções de armazenamento, como armazenamento de arquivo e de bloco, oferecem planos por hora e mensais que você pode escolher. Para escolher a solução de armazenamento correta, veja [Planejando o armazenamento persistente altamente disponível](/docs/containers?topic=containers-storage_planning#storage_planning). Para obter informações adicionais, veja:
  <ul><li>[Precificação de armazenamento de arquivo NFS![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud/file-storage/pricing)</li>
  <li>[Precificação de armazenamento de bloco![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud/block-storage/pricing)</li>
  <li>[Planos de armazenamento de objetos![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</li></ul></dd>

<dt id="services">{{site.data.keyword.Bluemix_notm}}  serviços</dt>
  <dd>Cada serviço que você integra a seu cluster tem seu próprio modelo de precificação. Revise a documentação de cada produto e use o console do {{site.data.keyword.Bluemix_notm}} para [estimar custos](/docs/billing-usage?topic=billing-usage-cost#cost).</dd>

<dt id="rhos_charges">Red Hat OpenShift on IBM Cloud</dt>
  <dd>
  <p class="preview">O [Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial) está disponível como um beta para testar clusters do OpenShift.</p>Se você criar um [cluster do Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial), seus nós do trabalhador serão instalados com o sistema operacional Red Hat Enterprise Linux, o que aumenta o preço das [máquinas do nó do trabalhador](#nodes). Deve-se também deve ter uma licença do OpenShift, que incorre em custos mensais além dos custos de VM por hora ou custos de bare metal mensais. A licença do OpenShift é para cada 2 núcleos do tipo de nó do trabalhador. Se você excluir seu nó do trabalhador antes do término do mês, sua licença mensal estará disponível para outros nós do trabalhador no conjunto de trabalhadores a ser usado. Para obter mais informações sobre clusters do OpenShift, consulte [Criando um cluster do Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial).</dd>

</dl>
<br><br>

Os recursos mensais são faturados com base no primeiro dia do mês para uso no mês anterior. Se você pedir um recurso mensal no meio do mês, será cobrada uma quantia rateada para esse mês. No entanto, se você cancelar um recurso no meio do mês, ainda será cobrada a quantia integral para o recurso mensal.
{: note}

## Os recursos de plataforma e de infraestrutura são consolidados em uma conta?
{: #bill}
{: faq}

Quando você usa uma conta faturável do {{site.data.keyword.Bluemix_notm}}, os recursos de plataforma e infraestrutura são resumidos em uma conta. Se você vinculou suas contas de infraestrutura do {{site.data.keyword.Bluemix_notm}} e do IBM Cloud (SoftLayer), receberá uma [fatura consolidada](/docs/customer-portal?topic=customer-portal-unifybillaccounts#unifybillaccounts) para seus recursos de plataforma e infraestrutura do {{site.data.keyword.Bluemix_notm}}.

## Posso estimar meus custos?
{: #cost_estimate}
{: faq}

Sim, consulte [Estimando seus custos](/docs/billing-usage?topic=billing-usage-cost#cost). Lembre-se que alguns encargos não são refletidos na estimativa, como a precificação em camadas para uso aumentado de hora em hora. Para obter mais informações, consulte [Quanto é cobrado ao usar o {{site.data.keyword.containerlong_notm}}?](#charges).

## Posso visualizar meu uso atual?
{: #usage}
{: faq}

É possível verificar seu uso atual e os totais mensais estimados para seus recursos de plataforma e infraestrutura do {{site.data.keyword.Bluemix_notm}}. Para obter mais informações, consulte [Visualizando seu uso](/docs/billing-usage?topic=billing-usage-viewingusage#viewingusage). Para organizar seu faturamento, é possível agrupar seus recursos com [grupos de recursos](/docs/resources?topic=resources-bp_resourcegroups#bp_resourcegroups).
