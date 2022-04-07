---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, helm

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


# Parceiros do IBM Cloud Kubernetes Service
{: #service-partners}

A IBM está dedicada a tornar o {{site.data.keyword.containerlong_notm}} o melhor serviço do Kubernetes que ajuda a migrar, operar e administrar suas cargas de trabalho do Kubernetes. Para fornecer a você todos os recursos necessários para executar cargas de trabalho de produção na nuvem, o {{site.data.keyword.containerlong_notm}} se associa a outros provedores de serviços de terceiros para aprimorar seu cluster com as ferramentas de criação de log, monitoramento e armazenamento de alta qualidade.
{: shortdesc}

Revise nossos parceiros e os benefícios de cada solução que eles fornecem. Para localizar outros serviços de software livre de proprietário do {{site.data.keyword.Bluemix_notm}} e de terceiros que podem ser usados em seu cluster, consulte [Entendendo integrações do {{site.data.keyword.Bluemix_notm}} e de terceiros](/docs/containers?topic=containers-ibm-3rd-party-integrations).

## LogDNA
{: #logdna-partner}

O {{site.data.keyword.la_full_notm}} fornece o [LogDNA ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://logdna.com/) como um serviço de terceiro que pode ser usado para incluir recursos de criação de log inteligentes em seu cluster e apps.

### Vantagens
{: #logdna-benefits}

Revise a tabela a seguir para localizar uma lista de benefícios chave que podem ser obtidos usando o LogDNA.
{: shortdesc}

|Benefício|Descrição|
|-------------|------------------------------|
|Gerenciamento de log centralizado e análise de log|Quando você configura seu cluster como uma origem de log, o LogDNA inicia automaticamente a coleta de informações de criação de log para os nós do trabalhador, os pods, os apps e a rede. Seus logs são analisados, indexados, identificados e agregados automaticamente pelo LogDNA e visualizados no painel LogDNA para que seja possível se aprofundar facilmente em seus recursos de cluster. É possível usar a ferramenta de gráfico integrada para visualizar os códigos de erro ou entradas de log mais comuns. |
|Facilidade de localização com a sintaxe procura semelhante ao Google|O LogDNA usa a sintaxe de procura semelhante ao Google que suporta termos padrão, operações `AND` e `OR` e permite excluir ou combinar termos de procura para ajudá-lo a localizar seus logs mais facilmente. Com a indexação inteligente de logs, é possível ir para uma entrada de log específica em qualquer momento. |
|Criptografia em trânsito e em repouso|O LogDNA criptografa automaticamente seus logs para proteger seus logs durante o trânsito e em repouso. |
|Alertas customizados e visualizações de log|É possível usar o painel para localizar os logs que correspondem aos seus critérios de procura, salvar esses logs em uma visualização e compartilhar essa visualização com outros usuários para simplificar a depuração entre os membros da equipe. Também é possível usar essa visualização para criar um alerta que pode ser enviado para sistemas de recebimento de dados, como PagerDuty, Slack ou e-mail. |
|Painéis customizados e prontos para usar|É possível escolher entre uma variedade de painéis existentes ou criar seu próprio painel para visualizar logs da maneira que você precisar. |

### Integração com o  {{site.data.keyword.containerlong_notm}}
{: #logdna-integration}

O LogDNA é fornecido pelo {{site.data.keyword.la_full_notm}}, um serviço da plataforma {{site.data.keyword.Bluemix_notm}} que pode ser usado com seu cluster. O {{site.data.keyword.la_full_notm}} é operado pelo LogDNA em parceria com a IBM.
{: shortdesc}

Para usar o LogDNA em seu cluster, deve-se provisionar uma instância do {{site.data.keyword.la_full_notm}} em sua conta do {{site.data.keyword.Bluemix_notm}} e configurar seus clusters Kubernetes como uma origem de log. Após a configuração do cluster, os logs são coletados automaticamente e encaminhados para a instância de serviço do {{site.data.keyword.la_full_notm}}. É possível usar o painel do {{site.data.keyword.la_full_notm}} para acessar seus logs.   

Para obter mais informações, consulte [Gerenciando logs do cluster Kubernetes com o {{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube).

### Faturamento e suporte
{: #logdna-billing-support}

O {{site.data.keyword.la_full_notm}} é totalmente integrado ao sistema de suporte do {{site.data.keyword.Bluemix_notm}}. Se você encontrar um problema com o uso do {{site.data.keyword.la_full_notm}}, poste uma pergunta no canal `logdna-on-iks` no [{{site.data.keyword.containerlong_notm}} Slack ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-container-service.slack.com/) ou abra um [caso de suporte do {{site.data.keyword.Bluemix_notm}}](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support). Efetue login no Slack usando seu IBMid. Se você não usar um IBMid para sua conta do {{site.data.keyword.Bluemix_notm}}, [solicite um convite para esse Slack ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://bxcs-slack-invite.mybluemix.net/).

## Sysdig
{: #sydig-partner}

O {{site.data.keyword.mon_full_notm}} fornece o [Sysdig Monitor ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://sysdig.com/products/monitor/) como um sistema de análise de dados de contêiner nativo de nuvem de terceiro que pode ser usado para obter insight sobre o desempenho e o funcionamento de seus hosts de cálculo, apps, contêineres e redes.
{: shortdesc}

### Vantagens
{: #sydig-benefits}

Revise a tabela a seguir para localizar uma lista de benefícios chave que podem ser obtidos usando Sysdig.
{: shortdesc}

|Benefício|Descrição|
|-------------|------------------------------|
|Acesso automático a métricas nativas de nuvem e customizadas do Prometheus|Escolha entre uma variedade de métricas nativas de nuvem e customizadas do Prometheus predefinidas para obter insight sobre o desempenho e o funcionamento de seus hosts de cálculo, apps, contêineres e redes.|
|Resolução de problemas com filtros avançados|O Sysdig Monitor cria topologias de rede que mostram como os nós do trabalhador estão conectados e como os serviços do Kubernetes se comunicam entre si. É possível navegar de seus nós do trabalhador para contêineres e chamadas de sistema único, além de agrupar e visualizar métricas importantes para cada recurso ao longo do caminho. Por exemplo, use essas métricas para localizar serviços que recebem a maioria das solicitações ou serviços com consultas e tempos de resposta lentos. É possível combinar esses dados com eventos do Kubernetes, eventos CI/CD customizados ou confirmações de código.
|Detecção automática de anomalias e alertas customizados|Defina regras e limites para quando você deseja ser notificado para detectar anomalias em seus recursos de cluster ou de grupo para permitir que o Sysdig o notifique quando um recurso agir de forma diferente do restante. É possível enviar esses alertas para as ferramentas de recebimento de dados, tais como o ServiceNow, o PagerDuty, o Slack, o VictorOps ou e-mail.|
|Painéis customizados e prontos para usar|É possível escolher entre uma variedade de painéis existentes ou criar seu próprio painel para visualizar métricas de seus microsserviços da maneira que você precisar. |
{: caption="Benefícios do uso do Sysdig Monitor" caption-side="top"}

### Integração com o  {{site.data.keyword.containerlong_notm}}
{: #sysdig-integration}

O Sysdig Monitor é fornecido pelo {{site.data.keyword.mon_full_notm}}, um serviço da plataforma {{site.data.keyword.Bluemix_notm}} que pode ser usado com seu cluster. O {{site.data.keyword.mon_full_notm}} é operado pelo Sysdig em parceria com a IBM.
{: shortdesc}

Para usar o Sysdig Monitor em seu cluster, deve-se provisionar uma instância do {{site.data.keyword.mon_full_notm}} em sua conta do {{site.data.keyword.Bluemix_notm}} e configurar seus clusters Kubernetes como uma origem de métricas. Após a configuração do cluster, as métricas são coletadas e encaminhadas automaticamente para a instância de serviço do {{site.data.keyword.mon_full_notm}}. É possível usar o painel do {{site.data.keyword.mon_full_notm}} para acessar suas métricas.   

Para obter mais informações, consulte [Analisar métricas para um app que é implementado em um cluster Kubernetes](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster).

### Faturamento e suporte
{: #sysdig-billing-support}

Como o Sysdig Monitor é fornecido pelo {{site.data.keyword.mon_full_notm}}, seu uso é incluído na conta do {{site.data.keyword.Bluemix_notm}} para serviços de plataforma. Para obter informações de precificação, revise os planos disponíveis no [catálogo do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/observe/monitoring/create).

O {{site.data.keyword.mon_full_notm}} é totalmente integrado ao sistema de suporte do {{site.data.keyword.Bluemix_notm}}. Se você encontrar um problema com o uso do {{site.data.keyword.mon_full_notm}}, poste uma pergunta no canal `sysdig-monitoring` no [{{site.data.keyword.containerlong_notm}} Slack ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-container-service.slack.com/) ou abra um [caso de suporte do {{site.data.keyword.Bluemix_notm}}](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support). Efetue login no Slack usando seu IBMid. Se você não usar um IBMid para sua conta do {{site.data.keyword.Bluemix_notm}}, [solicite um convite para esse Slack ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://bxcs-slack-invite.mybluemix.net/).

## Portworx
{: #portworx-parter}

O [Portworx ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://portworx.com/products/introduction/) é uma solução de armazenamento definido por software altamente disponível que pode ser usada para gerenciar armazenamento persistente local para seus bancos de dados conteinerizados e outros apps stateful ou para compartilhar dados entre os pods em múltiplas zonas.
{: shortdesc}

**O que é armazenamento definido pelo software (SDS)?** </br>
Uma solução SDS abstrata dispositivos de armazenamento de vários tipos, tamanhos ou de diferentes fornecedores que estão conectados aos nós do trabalhador em seu cluster. Os nós do trabalhador com armazenamento disponível em discos rígidos são incluídos como um nó em um cluster de armazenamento. Nesse cluster, o armazenamento físico é virtualizado e apresentado como um conjunto de armazenamento virtual para o usuário. O cluster de armazenamento é gerenciado pelo software SDS. Se os dados devem ser armazenados no cluster de armazenamento, o software SDS decidirá onde armazenar os dados para a mais alta disponibilidade. Seu armazenamento virtual é fornecido com um conjunto comum de recursos e serviços que você pode aproveitar sem se preocupar com a arquitetura de armazenamento subjacente real.

### Vantagens
{: #portworx-benefits}

Revise a tabela a seguir para localizar uma lista de benefícios chave que podem ser obtidos usando o Portworx.
{: shortdesc}

|Benefício|Descrição|
|-------------|------------------------------|
|Armazenamento nativo de nuvem e gerenciamento de dados para apps stateful|O Portworx agrega o armazenamento local disponível que está conectado aos nós do trabalhador e que pode variar em tamanho ou tipo e cria uma camada de armazenamento persistente unificada para bancos de dados conteinerizados ou outros apps stateful que você deseja executar no cluster. Usando as solicitações de volume persistente (PVC) do Kubernetes, é possível incluir armazenamento persistente local em seus apps para armazenar seus dados.|
|Dados altamente disponíveis com replicação de volume|O Portworx replica automaticamente os dados em seus volumes entre os nós do trabalhador e zonas em seu cluster para que seus dados possam ser acessados o tempo todo e seu app stateful possa ser reagendado para outro nó do trabalhador em caso de uma falha do nó do trabalhador ou reinicialização. |
|Suporte para executar `hyper-converged`|O Portworx pode ser configurado para executar [`hyper-converged`![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/hyperconvergence/) para assegurar que seus recursos de cálculo e o armazenamento sejam sempre colocados no mesmo nó do trabalhador. Quando o seu app deve ser reagendado, o Portworx move seu app para um nó do trabalhador no qual uma das suas réplicas de volume reside para assegurar a velocidade de acesso ao disco local e alto desempenho para seu app stateful. |
|Criptografar dados com o {{site.data.keyword.keymanagementservicelong_notm}}|É possível [configurar chaves de criptografia do {{site.data.keyword.keymanagementservicelong_notm}}](/docs/containers?topic=containers-portworx#encrypt_volumes) que são protegidas por hardware security module (HSMs) baseados em nuvem e certificados pelo FIPS 140-2 Nível 2 para proteger os dados em seus volumes. É possível escolher entre usar uma chave de criptografia para criptografar todos os seus volumes em um cluster ou usando uma chave de criptografia para cada volume. O Portworx usa essa chave para criptografar dados em repouso e durante o trânsito quando os dados são enviados para um nó do trabalhador diferente.|
|Capturas instantâneas integradas e backups em nuvem|É possível salvar o estado atual de um volume e seus dados criando uma [captura instantânea do Portworx ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/create-snapshots/). As capturas instantâneas podem ser armazenadas em seu cluster Portworx local ou na nuvem.|
|Monitoramento integrado com o Lighthouse|O [Lighthouse ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.portworx.com/reference/lighthouse/) é uma ferramenta gráfica intuitiva para ajudar a gerenciar e monitorar os clusters Portworx e as capturas instantâneas do volume. Com o Lighthouse, é possível visualizar o funcionamento do cluster Portworx, incluindo o número de nós de armazenamento disponíveis, volumes e capacidade disponível, e analisar seus dados em Prometheus, Grafana ou Kibana.|
{: caption="Benefícios do uso do Portworx" caption-side="top"}

### Integração com o  {{site.data.keyword.containerlong_notm}}
{: #portworx-integration}

O {{site.data.keyword.containerlong_notm}} fornece tipos de nó do trabalhador que são otimizados para uso do SDS e que vêm com um ou mais discos locais brutos, não formatados e desmontados, que podem ser usados para armazenar seus dados. O Portworx oferece o melhor desempenho quando você usa as [máquinas do nó do trabalhador SDS](/docs/containers?topic=containers-planning_worker_nodes#sds) que vêm com a velocidade de rede de 10 Gbps. No entanto, é possível instalar o Portworx em tipos de nó do trabalhador não SDS, mas é possível que você não obtenha os benefícios de desempenho que seu app requer.
{: shortdesc}

O Portworx é instalado usando um [gráfico do Helm](/docs/containers?topic=containers-portworx#install_portworx). Quando você instala o gráfico do Helm, o Portworx analisa automaticamente o armazenamento persistente local que está disponível em seu cluster e inclui o armazenamento na camada de armazenamento do Portworx. Para incluir armazenamento de sua camada de armazenamento do Portworx em seus apps, deve-se usar [solicitações de volume persistente do Kubernetes](/docs/containers?topic=containers-portworx#add_portworx_storage).

Para instalar o Portworx em seu cluster, deve-se ter uma licença do Portworx. Se você estiver usando pela primeira vez, será possível usar a edição `px-enterprise` como uma versão de Avaliação. A versão de Avaliação fornece a funcionalidade completa do Portworx que é possível testar por 30 dias. Após a expiração da versão de Avaliação, deve-se [comprar uma licença do Portworx ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/containers?topic=containers-portworx#portworx_license) para continuar usando o cluster Portworx.

Para obter mais informações sobre como instalar e usar o Portworx com o {{site.data.keyword.containerlong_notm}}, consulte [Armazenando dados no software-defined storage (SDS) com o Portworx](/docs/containers?topic=containers-portworx).

### Faturamento e suporte
{: #portworx-billing-support}

As máquinas de nós do trabalhador do SDS que vêm com discos locais e as máquinas virtuais que você usa para o Portworx são incluídas em sua conta mensal do {{site.data.keyword.containerlong_notm}}. Para obter informações de precificação, consulte o [catálogo do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/kubernetes/catalog/cluster). A licença do Portworx é um custo separado e não está incluído em sua conta mensal.
{: shortdesc}

Se você tiver algum problema com o uso do Portworx ou desejar conversar sobre as configurações do Portworx para seu caso de uso específico, poste uma pergunta no canal `portworx-on-iks` no [{{site.data.keyword.containerlong_notm}} Slack ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-container-service.slack.com/). Efetue login no Slack usando seu IBMid. Se você não usar um IBMid para sua conta do {{site.data.keyword.Bluemix_notm}}, [solicite um convite para esse Slack ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://bxcs-slack-invite.mybluemix.net/).
