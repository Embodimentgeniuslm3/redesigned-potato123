---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-03"

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


# Definindo sua estratégia do Kubernetes
{: #strategy}

Com o {{site.data.keyword.containerlong}}, é possível implementar cargas de trabalho de contêiner de forma rápida e segura para seus aplicativos em produção. Saiba mais para que, ao planejar sua estratégia de cluster, você otimize sua configuração para aproveitar ao máximo os recursos de gerenciamento automatizado de implementação, dimensionamento e orquestração do [Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/).
{:shortdesc}

## Movendo suas cargas de trabalho para o {{site.data.keyword.Bluemix_notm}}
{: #cloud_workloads}

Há muitos motivos para mover suas cargas de trabalho para o {{site.data.keyword.Bluemix_notm}}: redução do custo total de propriedade, aumento da alta disponibilidade para seus aplicativos em um ambiente seguro e em conformidade, dimensionamento em resposta à demanda de seu usuário e muito mais. O {{site.data.keyword.containerlong_notm}} combina a tecnologia de contêiner com ferramentas de software livre, como o Kubernetes, para que seja possível construir um aplicativo nativo de nuvem que possa migrar entre diferentes ambientes de nuvem, evitando o bloqueio do fornecedor.
{:shortdesc}

Mas como chegar à nuvem? Quais são as opções ao longo do caminho? E, depois de chegar lá, como gerenciar suas cargas de trabalho?

Use essa página para aprender algumas estratégias para suas implementações do Kubernetes no {{site.data.keyword.containerlong_notm}}. Além disso, sinta-se sempre livre para entrar em contato com nossa equipe no [Slack. ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-container-service.slack.com)

Ainda não está no Slack? [Solicite um convite!](https://bxcs-slack-invite.mybluemix.net/)
{: tip}

### O que posso mover para o {{site.data.keyword.Bluemix_notm}}?
{: #move_to_cloud}

Com o {{site.data.keyword.Bluemix_notm}}, você tem flexibilidade para criar clusters do Kubernetes em [ambientes de nuvem fora do local, no local ou híbrido](/docs/containers?topic=containers-cs_ov#differentiation). A tabela a seguir fornece alguns exemplos de quais tipos de cargas de trabalho os usuários geralmente movem para os diversos tipos de nuvem. Também é possível escolher uma abordagem híbrida na qual você tem clusters em execução em ambos os ambientes.
{: shortdesc}

| Carga de trabalho | {{site.data.keyword.containershort_notm}} fora do local | on-prem |
| --- | --- | --- |
| Ferramentas de ativação de DevOps | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" /> | |
| Desenvolvendo e testando aplicativos | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" /> | |
| Aplicativos têm grandes mudanças na demanda e precisam ser dimensionados rapidamente | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" /> | |
| Aplicativos de negócios, como o CRM, o HCM, o ERP e o E-commerce | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" /> | |
| Ferramentas sociais e de colaboração, como e-mail | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" /> | |
| Cargas de trabalho Linux e x86 | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" /> | |
| Bare metal e recursos de cálculo da GPU | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" /> |
| Cargas de trabalho compatíveis com PCI e HIPAA | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" /> |
| Aplicativos anteriores com restrições e dependências de plataforma e infraestrutura | | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" /> |
| Aplicativos proprietários com designs restritos, licenciamento ou regulamentos exigentes | | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" /> |
| Dimensionando aplicativos na nuvem pública e sincronizando os dados para um banco de dados privado local | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" />  | <img src="images/confirm.svg" width="32" alt="Recurso disponível" style="width:32px;" /> |
{: caption="As implementações do {{site.data.keyword.Bluemix_notm}} suportam suas cargas de trabalho" caption-side="top"}

**Pronto para executar cargas de trabalho fora do local em {{site.data.keyword.containerlong_notm}}?**</br>
Ótimo! Você já está em nossa documentação de nuvem pública. Continue lendo para obter mais ideias de estratégia ou comece agora [criando um cluster](/docs/containers?topic=containers-getting-started).

**Interessado em uma nuvem no local?**</br>
Explore a [documentação do {{site.data.keyword.Bluemix_notm}} Private ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.1/kc_welcome_containers.html). Se já tiver investimentos significativos na tecnologia IBM, como o WebSphere Application Server e o Liberty, será possível otimizar sua estratégia de modernização do {{site.data.keyword.Bluemix_notm}} Private com diversas ferramentas.

**Deseja executar cargas de trabalho nas nuvens no local e fora do local?**</br>
Comece configurando uma conta do {{site.data.keyword.Bluemix_notm}} Private. Em seguida, consulte [Usando o {{site.data.keyword.containerlong_notm}} com o {{site.data.keyword.Bluemix_notm}} Private](/docs/containers?topic=containers-hybrid_iks_icp) para conectar seu ambiente do {{site.data.keyword.Bluemix_notm}} Private com um cluster no {{site.data.keyword.Bluemix_notm}} Public. Para gerenciar múltiplos clusters do Kubernetes em nuvem, como ao longo do {{site.data.keyword.Bluemix_notm}} Public e do {{site.data.keyword.Bluemix_notm}} Private, efetue check-out do [IBM Multicloud Manager ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).

### Quais tipos de aplicativo podem ser executados no {{site.data.keyword.containerlong_notm}}?
{: #app_types}

Deve ser possível executar seu aplicativo conteinerizado no sistema operacional suportado, o Ubuntu 16.64, 18.64. Também é recomendado considerar a capacidade de seu aplicativo de ser stateful.
{: shortdesc}

<dl>
<dt>Aplicativos stateless</dt>
  <dd><p>Aplicativos stateless são preferenciais para ambientes nativos de nuvem, como o Kubernetes. Eles são simples de migrar e escalar porque declaram dependências, armazenam configurações separadamente do código e tratam serviços auxiliares, como bancos de dados, como recursos conectados, em vez de acoplados ao aplicativo. Os pods do aplicativo não requerem armazenamento de dados persistente ou um endereço IP de rede estável e, por isso, podem ser finalizados, reprogramados e dimensionados em resposta às demandas de carga de trabalho. O aplicativo usa um banco de dados como um serviço para dados persistentes e serviços NodePort, de balanceador de carga ou do Ingress para expor a carga de trabalho em um endereço IP estável.</p></dd>
<dt>Apps stateful</dt>
  <dd><p>Os aplicativos stateful são mais complicados de configurar, gerenciar e dimensionar do que os stateless porque os pods requerem dados persistentes e uma identidade de rede estável. Aplicativos stateful são, frequentemente, bancos de dados ou outras cargas de trabalho distribuídas e intensivas em dados, nas quais o processamento é mais eficiente mais próximo dos dados em si.</p>
  <p>Se desejar implementar um aplicativo stateful, será necessário configurar o armazenamento persistente e montar um volume persistente para o pod controlado por um objeto StatefulSet. É possível optar por incluir o armazenamento de [arquivos](/docs/containers?topic=containers-file_storage#file_statefulset), de [bloco](/docs/containers?topic=containers-block_storage#block_statefulset) ou de [objeto](/docs/containers?topic=containers-object_storage#cos_statefulset) como o armazenamento persistente para seu conjunto stateful. Também é possível instalar o [Portworx](/docs/containers?topic=containers-portworx) sobre seus nós do trabalhador bare metal e usá-lo como uma solução de armazenamento definido por software altamente disponível para gerenciar o armazenamento persistente para seus aplicativos stateful. Para obter mais informações sobre o funcionamento dos conjuntos stateful, consulte a [Documentação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/).</p></dd>
</dl>

### Quais as diretrizes para o desenvolvimento de aplicativos stateless nativos de nuvem?
{: #12factor}

Confira o [Aplicativo de doze fatores ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://12factor.net/), uma metodologia abrangente de linguagem para considerar como desenvolver seu aplicativo em 12 fatores, resumidos a seguir.
{: shortdesc}

1.  **Código base**: use um único código base em um sistema de controle de versão para suas implementações. Ao fazer pull de uma imagem para sua implementação de contêiner, especifique uma tag de imagem testada em vez de usar `latest`.
2.  **Dependências**: declare e isole dependências externas explicitamente.
3.  **Configuração**: armazene configurações específicas da implementação em variáveis de ambiente, não no código.
4.  **Serviços auxiliares**: trate os serviços auxiliares, como armazenamentos de dados ou filas de mensagens, como recursos conectados ou substituíveis.
5.  **Estágios do aplicativo**: construa em estágios distintos, como `build`, `release` e `run`, com separação restrita entre eles.
6.  **Processos**: execute como um ou mais processos stateless que não compartilham nada e que usam o [armazenamento persistente](/docs/containers?topic=containers-storage_planning) para salvar dados.
7.  **Ligação de porta**: ligações de porta são autocontidas e fornecem um terminal de serviço em host e porta bem definidos.
8.  **Simultaneidade**: gerencie e escale seu aplicativo por meio de instâncias de processo, como réplicas e dimensionamento horizontal. Configure solicitações de recurso e limites para suas implementações. Observe que as políticas de rede do Calico não podem limitar a largura da banda. Em vez disso, considere o [Istio](/docs/containers?topic=containers-istio).
9.  **Descartabilidade**: projete seu aplicativo para ser descartável, com inicialização mínima, encerramento normal e tolerância para finalizações de processo abruptas. Lembre-se de que contêineres, pods e até mesmo nós do trabalhador devem ser descartáveis, portanto, planeje o seu aplicativo de forma apropriada.
10.  **Paridade de desenvolvimento para produção**: configure um pipeline de [integração contínua](https://www.ibm.com/cloud/garage/content/code/practice_continuous_integration/) e [entrega contínua](https://www.ibm.com/cloud/garage/content/deliver/practice_continuous_delivery/) para seu app, com mínima diferença entre o app em desenvolvimento e o app em produção.
11.  **Logs**: trate os logs como fluxos de eventos, pois o ambiente externo ou de hospedagem processa e roteia arquivos de log. **Importante**: no {{site.data.keyword.containerlong_notm}}, os logs não são ativados por padrão. Para ativá-los, consulte [Configurando o encaminhamento de log](/docs/containers?topic=containers-health#configuring).
12.  **Processos de administração**: mantenha quaisquer scripts de administrador únicos com seu app e execute-os como um [objeto de Tarefa do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) para assegurar que os scripts de administrador sejam executados com o mesmo ambiente que o app em si. Para a orquestração de pacotes maiores que você deseja executar em seus clusters Kubernetes, considere usar um gerenciador de pacotes, como o [Helm ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://helm.sh/).

### Já tenho um aplicativo. Como posso migrá-lo para o {{site.data.keyword.containerlong_notm}}?
{: #migrate_containerize}

A seguir, é possível executar algumas etapas gerais para conteinerizar seu aplicativo.
{: shortdesc}

1.  Use o [Aplicativo de doze fatores ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://12factor.net/) como um guia para isolar dependências, separando processos em serviços separados, e reduzir o máximo possível a capacidade de seu aplicativo de ser stateful.
2.  Localize uma imagem base apropriada a ser usada. É possível usar imagens disponíveis publicamente no [Docker Hub ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://hub.docker.com/), [imagens públicas da IBM](/docs/services/Registry?topic=registry-public_images#public_images) ou construir e gerenciar suas próprias em seu {{site.data.keyword.registryshort_notm}} privado.
3.  Inclua em sua imagem do Docker somente o que for necessário para executar o aplicativo.
4.  Em vez de depender do armazenamento local, planeje-se para usar o armazenamento persistente ou soluções de banco de dados como um serviço de nuvem para fazer backup dos dados do seu aplicativo.
5.  Com o tempo, refatore os processos de seu aplicativo em microsserviços.

Para obter mais informações, consulte os tutoriais a seguir:
*  [Migrando um aplicativo do Cloud Foundry para um cluster](/docs/containers?topic=containers-cf_tutorial#cf_tutorial)
*  [Movendo um aplicativo baseado em VM para o Kubernetes](/docs/tutorials?topic=solution-tutorials-vm-to-containers-and-kubernetes)



Continue com os tópicos a seguir para obter mais considerações para quando mover cargas de trabalho, como ambientes do Kubernetes, alta disponibilidade, descoberta de serviço e implementações.

<br />


### Quais conhecimentos e qualificações técnicas é bom ter antes de mover meus apps para o {{site.data.keyword.containerlong_notm}}?
{: #knowledge}

O Kubernetes é projetado para fornecer recursos para duas personas principais, o administrador do cluster e o desenvolvedor de aplicativo. Cada persona usa diferentes qualificações técnicas para executar e implementar com êxito apps em um cluster.
{: shortdesc}

**Quais são as principais tarefas e conhecimento técnico do administrador do cluster?**</br>
Como um administrador de cluster, você é responsável por configurar, operar, proteger e gerenciar a infraestrutura do {{site.data.keyword.Bluemix_notm}} de seu cluster. As tarefas típicas incluem:
- Dimensionar o cluster para fornecer capacidade suficiente para suas cargas de trabalho.
- Projetar um cluster para atender aos padrões de alta disponibilidade, recuperação de desastre e conformidade de sua empresa.
- Proteger o cluster configurando permissões de usuário e limitando ações dentro do cluster para proteger seus recursos de cálculo, sua rede e dados.
- Planejar e gerenciar a comunicação de rede entre os componentes de infraestrutura para assegurar a segurança de rede, segmentação e conformidade.
- Planejar opções de armazenamento persistente para atender aos requisitos de residência de dados e de proteção de dados.

A persona do administrador do cluster deve ter um amplo conhecimento que inclua cálculo, rede, armazenamento, segurança e conformidade. Em uma empresa típica, esse conhecimento é difundido entre múltiplos especialistas, como Engenheiros de Sistemas, Administradores do Sistema, Engenheiros de Rede, Arquitetos de Rede, Gerentes de TI ou Especialistas de Segurança e Conformidade. Considere designar a função de administrador do cluster a múltiplas pessoas em sua empresa para que você tenha o conhecimento necessário para operar com êxito seu cluster.

**Quais são as principais tarefas e qualificações técnicas do desenvolvedor de aplicativo?**</br>
Como desenvolvedor, você projeta, cria, protege, implementa, testa, executa e monitora apps nativos em nuvem, conteinerizados em um cluster Kubernetes. Para criar e executar esses apps, deve-se estar familiarizado com o conceito de microsserviços, as diretrizes do [app 12-factor](#12factor), os [princípios de docker e conteinerização](https://www.docker.com/) e as [opções de implementação do Kubernetes](/docs/containers?topic=containers-app#plan_apps) disponíveis. Se você desejar implementar apps serverless, familiarize-se com o [Knative](/docs/containers?topic=containers-cs_network_planning).

O Kubernetes e o {{site.data.keyword.containerlong_notm}} fornecem múltiplas opções de como [expor um app e manter um app privado](/docs/containers?topic=containers-cs_network_planning), [incluir armazenamento persistente](/docs/containers?topic=containers-storage_planning), [integrar outros serviços](/docs/containers?topic=containers-ibm-3rd-party-integrations) e como é possível [proteger suas cargas de trabalho e proteger dados sensíveis](/docs/containers?topic=containers-security#container). Antes de mover seu app para um cluster no {{site.data.keyword.containerlong_notm}}, verifique se é possível executar seu app como um app conteinerizado no sistema operacional Ubuntu 16.64, 18.64 suportado e se o Kubernetes e o {{site.data.keyword.containerlong_notm}} fornecem os recursos necessários para sua carga de trabalho.

**Os administradores e desenvolvedores de cluster interagem uns com os outros?**</br>
Sim. Os administradores e desenvolvedores de cluster devem interagir com frequência para que os administradores de cluster entendam os requisitos de carga de trabalho para fornecer esse recurso no cluster e para que os desenvolvedores saibam sobre limitações, integrações e princípios de segurança disponíveis que eles devem considerar em seu processo de desenvolvimento de app.

## Dimensionando seu cluster Kubernetes para suportar sua carga de trabalho
{: #sizing}

Descobrir quantos nós do trabalhador são necessários em seu cluster para suportar sua carga de trabalho não é uma ciência exata. Pode ser necessário testar configurações diferentes e adaptar-se. A boa notícia é que você está usando o {{site.data.keyword.containerlong_notm}}, no qual é possível incluir e remover nós do trabalhador em resposta às demandas de suas cargas de trabalho.
{: shortdesc}

Para começar a dimensionar seu cluster, pergunte a si mesmo o seguinte.

### Quantos recursos são necessários para o meu aplicativo?
{: #sizing_resources}

Primeiro, vamos começar com seu uso de carga de trabalho de projeto ou existente.

1.  Calcule o uso médio de CPU e de memória de sua carga de trabalho. Por exemplo, é possível visualizar o Gerenciador de Tarefas em uma máquina Windows ou executar o comando `top` em um Mac ou Linux. Também é possível usar um serviço de métricas e executar relatórios para calcular o uso da carga de trabalho.
2.  Preveja o número de solicitações que devem ser entregues por sua carga de trabalho para que seja possível decidir quantas réplicas de aplicativo você deseja para manipular a carga de trabalho. Por exemplo, é possível projetar uma instância de aplicativo para manipular 1000 solicitações por minuto e prever que sua carga de trabalho deve entregar 10.000 solicitações por minuto. Se ocorrer dessa forma, será possível decidir fazer 12 réplicas de aplicativo, com 10 para manipular a quantidade prevista e 2 adicionais para obter capacidade de aumento.

### O que, além do meu aplicativo, pode usar recursos no cluster?
{: #sizing_other}

Agora, incluiremos alguns outros recursos que podem ser usados.



1.  Considere se seu aplicativo faz pull de muitas imagens ou de imagens grandes, que podem ocupar todo o armazenamento local no nó do trabalhador.
2.  Decida se deseja [integrar serviços](/docs/containers?topic=containers-supported_integrations#supported_integrations) em seu cluster, como o [Helm](/docs/containers?topic=containers-helm#public_helm_install) ou o [Prometheus ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus). Esses serviços e complementos integrados aceleram os pods que consomem recursos de cluster.

### Que tipo de disponibilidade devo desejar para minha carga de trabalho?
{: #sizing_availability}

Não se esqueça de que você deseja que sua carga de trabalho esteja tão disponível quanto possível!

1.  Planeje as etapas de sua estratégia para [clusters altamente disponíveis](/docs/containers?topic=containers-ha_clusters#ha_clusters), como a decisão entre clusters únicos ou multizona.
2.  Revise as [implementações altamente disponíveis](/docs/containers?topic=containers-app#highly_available_apps) para obter ajuda para decidir como é possível tornar seu aplicativo disponível.

### Quantos nós do trabalhador são necessários para manipular minha carga de trabalho?
{: #sizing_workers}

Agora que você compreendeu bem como é sua carga de trabalho, mapearemos o uso estimado em suas configurações de cluster disponíveis.

1.  Estime a capacidade máxima do nó do trabalhador, que depende do tipo de cluster que você possui. Evite exceder a capacidade do nó do trabalhador no caso de um aumento ou de outro evento temporário acontecer.
    *  **Clusters de zona única**: planeje-se para ter pelo menos 3 nós do trabalhador em seu cluster. Além disso, tenha uma capacidade de memória e CPU relativa a um nó adicional disponível no cluster.
    *  **Clusters multizona**: planeje-se para ter pelo menos 2 nós do trabalhador por zona, totalizando 6 nós em 3 zonas. Além disso, planeje-se para que a capacidade total de seu cluster seja pelo menos 150% da capacidade total necessária de sua carga de trabalho, de modo que se uma zona ficar inativa, você tenha recursos disponíveis para manter a carga de trabalho.
2.  Alinhe o tamanho do aplicativo e a capacidade do nó do trabalhador com um dos [tipos de nó do trabalhador disponíveis](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes). Para ver os tipos disponíveis em uma zona, execute `ibmcloud ks machine-types <zone>`.
    *   **Não sobrecarregue nós do trabalhador**: para evitar que pods disputem CPU ou sejam executados de forma ineficiente, deve-se conhecer os recursos necessários aos aplicativos para que seja possível planejar o número de nós do trabalhador necessários. Por exemplo, se seus aplicativos precisam de menos recursos do que os que estão disponíveis no nó do trabalhador, é possível limitar o número de pods implementados em um nó do trabalhador. Mantenha o nó do trabalhador com cerca de 75% de sua capacidade para deixar espaço para outros pods que possam precisar ser planejados. Se seus aplicativos precisam de mais recursos do que há disponíveis em seu nó do trabalhador, use um tipo diferente de nó do trabalhador que possa atender a esses requisitos. É possível saber se seus nós do trabalhador estão sobrecarregados quando eles frequentemente retornam um status de `NotReady` ou despejam pods devido à falta de memória ou de outros recursos.
    *   **Tipos maiores e menores de nós do trabalhador**: os nós maiores podem ser mais economicamente rentáveis do que os menores, particularmente para cargas de trabalho projetadas para obter eficiência ao processar em uma máquina de alto desempenho. No entanto, se um nó do trabalhador grande ficar inativo, será necessário certificar-se de que seu cluster tenha capacidade suficiente para reagendar normalmente todos os pods de carga de trabalho em outros nós do trabalhador no cluster. Trabalhadores menores podem ajudá-lo a dimensionar mais normalmente.
    *   **Réplicas de seu aplicativo**: para determinar o número desejado de nós do trabalhador, também é possível considerar quantas réplicas de seu aplicativo você deseja executar. Por exemplo, se souber que sua carga de trabalho requer 32 núcleos da CPU e planejar executar 16 réplicas de seu aplicativo, cada pod de réplica precisará de 2 núcleos da CPU. Se desejar executar apenas um pod de aplicativo por nó do trabalhador, será possível solicitar um número apropriado de nós do trabalhador para seu tipo de cluster, para que seja possível suportar essa configuração.
3.  Execute testes de desempenho para continuar refinando o número de nós do trabalhador que você precisa em seu cluster, com requisitos representativos de latência, escalabilidade, conjunto de dados e carga de trabalho.
4.  Para cargas de trabalho que precisam ter o dimensionamento ampliado ou reduzido em resposta a solicitações de recursos, configure o [dimensionador automático horizontal de pod](/docs/containers?topic=containers-app#app_scaling) e o [dimensionador automático de conjunto do trabalhador do cluster](/docs/containers?topic=containers-ca#ca).

<br />


## Estruturando seu ambiente do Kubernetes
{: #kube_env}

Seu {{site.data.keyword.containerlong_notm}} está vinculado apenas a um portfólio de infraestrutura do IBM Cloud (SoftLayer). Dentro de sua conta, é possível criar clusters compostos de um principal e de diversos nós do trabalhador. A IBM gerencia o principal e é possível que você crie uma combinação de conjuntos do trabalhador que agrupem máquinas individuais do mesmo tipo ou especificações de memória e CPU. Dentro do cluster, é possível organizar adicionalmente os recursos usando namespaces e rótulos. Escolha a combinação ideal de cluster, tipos de máquina e estratégias de organização para que seja possível garantir que suas equipes e cargas de trabalho obtenham os recursos necessários.
{:shortdesc}

### Que tipos de cluster e de máquina devo obter?
{: #env_flavors}

**Tipos de clusters**: decida se deseja uma [configuração de diversos clusters, de zona única ou multizona](/docs/containers?topic=containers-ha_clusters#ha_clusters). Os clusters multizona estão disponíveis em [todas as seis regiões metro mundiais do {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-regions-and-zones#zones). Além disso, tenha em mente que os nós do trabalhador variam de acordo com a zona.

**Tipos de nós do trabalhador**: em geral, é mais adequado que suas cargas de trabalho intensivas sejam executadas em máquinas físicas bare metal, em contrapartida, para o trabalho de teste e desenvolvimento economicamente rentável, é possível escolher máquinas virtuais em hardware compartilhado ou compartilhado e dedicado. Com nós do trabalhador bare metal, seu cluster tem uma velocidade de rede de 10Gbps e núcleos hyper-threaded que oferecem maior rendimento. As máquinas virtuais são fornecidas com uma velocidade de rede de 1 Gbps e núcleos regulares que não oferecem hyper-threading. [Confira o isolamento e os tipos de máquina disponíveis](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes).

### Devo usar diversos clusters ou apenas incluir mais trabalhadores em um cluster existente?
{: #env_multicluster}

O número de clusters criados depende de sua carga de trabalho, políticas e regulamentações da empresa e o que você deseja fazer com os recursos de cálculo. Também é possível revisar as informações de segurança sobre essa decisão em [Isolamento e segurança de contêiner](/docs/containers?topic=containers-security#container).

**Diversos clusters**: é necessário configurar [um balanceador de carga global](/docs/containers?topic=containers-ha_clusters#multiple_clusters) e copiar e aplicar os mesmos arquivos YAML de configuração para balancear cargas de trabalho entre os clusters. Portanto, diversos clusters são geralmente mais complexos de gerenciar, mas podem ajudá-lo a atingir objetivos importantes, como o seguinte.
*  Obedecer às políticas de segurança que requerem o isolamento das cargas de trabalho.
*  Testar como seu aplicativo é executado em uma versão diferente do Kubernetes ou em outro software de cluster, como o Calico.
*  Criar um cluster com seu aplicativo em outra região para oferecer um desempenho mais alto para os usuários nessa área geográfica.
*  Configurar o acesso do usuário no nível da instância do cluster em vez de customizar e gerenciar diversas políticas RBAC para controlar o acesso dentro de um cluster no nível do namespace.

**Menos clusters ou clusters únicos**: menos clusters podem ajudar a reduzir o esforço operacional e os custos por cluster para recursos fixos. Em vez de criar mais clusters, é possível incluir conjuntos do trabalhador para diferentes tipos de máquinas de recursos de computação disponíveis para os componentes do aplicativo e do serviço que você deseja usar. Quando você desenvolve o aplicativo, os recursos usados por ele estão na mesma zona ou conectados de maneira próxima em uma multizona, para que seja possível fazer suposições sobre latência, largura da banda ou falhas correlacionadas. No entanto, é ainda mais importante que você organize seu cluster usando namespaces, cotas de recurso e rótulos.

### Como posso configurar meus recursos dentro do cluster?
{: #env_resources}

<dl>
<dt>Considere a capacidade de seu nó do trabalhador.</dt>
  <dd>Para obter o máximo desempenho de seu nó do trabalhador, considere o seguinte:
  <ul><li><strong>Acompanhe a potência de seu núcleo</strong>: cada máquina possui um determinado número de núcleos. Configure um limite para o número de pods por núcleo, como 10, de acordo com a carga de trabalho do seu aplicativo.</li>
  <li><strong>Evite a sobrecarga do nó</strong>: de forma semelhante, apenas porque um nó pode conter mais de 100 pods, isso não significa que isso seja o que você deseja. Configure um limite para o número de pods por nó, como 40, de acordo com a carga de trabalho de seu aplicativo.</li>
  <li><strong>Não exceda a largura da banda de seu cluster</strong>: tenha em mente que a largura da banda de rede em máquinas virtuais com ajuste de escala fica em torno de 1000 Mbps. Se precisar de centenas de nós do trabalhador em um cluster, divida-o em diversos clusters com menos nós ou solicite nós bare metal.</li>
  <li><strong>Classificando seus serviços</strong>: planeje a quantidade de serviços necessários para sua carga de trabalho antes de realizar a implementação. As regras de encaminhamento de porta e de rede são aplicadas em Iptables. Se prever um número maior de serviços, como mais de 5.000, divida o cluster em diversos clusters.</li></ul></dd>
<dt>Provisione tipos diferentes de máquinas para uma combinação de recursos de computação.</dt>
  <dd>Todo mundo gosta de ter opções, certo? Com o {{site.data.keyword.containerlong_notm}}, você tem [uma combinação de tipos de máquina](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) que podem ser implementadas: de bare metal para cargas de trabalho intensivas até máquinas virtuais para ajuste rápido de escala. Use rótulos ou namespaces para organizar implementações em suas máquinas. Ao criar uma implementação, limite-a para que o pod de seu aplicativo seja implementado somente em máquinas com a combinação ideal de recursos. Por exemplo, é possível que você deseje limitar um aplicativo de banco de dados a uma máquina bare metal com uma quantidade significativa de armazenamento em disco local, como o `md1c.28x512.4x4tb`.</dd>
<dt>Configure diversos namespaces quando tiver diversas equipes e projetos que compartilham o cluster.</dt>
  <dd><p>Os namespaces são como um cluster dentro do cluster. Eles são uma maneira de dividir recursos de cluster usando [cotas de recurso ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/policy/resource-quotas/) e [limites padrão ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/). Ao fazer novos namespaces, certifique-se de configurar as [políticas RBAC](/docs/containers?topic=containers-users#rbac) adequadas para controlar o acesso. Para obter mais informações, consulte [Compartilhar um cluster com namespaces ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/) na documentação do Kubernetes.</p>
  <p>Se tiver um cluster pequeno, duas dúzias de usuários e recursos semelhantes (como versões diferentes do mesmo software), provavelmente não serão necessários diversos namespaces. Em vez disso, é possível usar rótulos.</p></dd>
<dt>Configure as cotas de recurso para que os usuários em seu cluster precisem usar solicitações de recurso e limites</dt>
  <dd>Para garantir que cada equipe tenha os recursos necessários para implementar serviços e executar aplicativos no cluster, deve-se configurar [cotas de recurso](https://kubernetes.io/docs/concepts/policy/resource-quotas/) para cada namespace. As cotas de recurso determinam as restrições de implementação para um namespace, como o número de recursos do Kubernetes que podem ser implementados e a quantia de CPU e memória que pode ser consumida por esses recursos. Depois de configurar uma cota, os usuários devem incluir solicitações de recurso e limites em suas implementações.</dd>
<dt>Organize seus objetos do Kubernetes com rótulos</dt>
  <dd><p>Para organizar e selecionar os recursos do Kubernetes, como `pods` ou `nodes`, [use rótulos do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/). Por padrão, o {{site.data.keyword.containerlong_notm}} aplica alguns rótulos, incluindo `arch`, `os`, `region`, `zone` e `machine-type`.</p>
  <p>Casos de uso de exemplo para rótulos incluem [limitar o tráfego de rede para nós do trabalhador de borda](/docs/containers?topic=containers-edge), [implementar um aplicativo em uma máquina de GPU](/docs/containers?topic=containers-app#gpu_app) e [restringir suas cargas de trabalho do aplicativo![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/) para serem executadas em nós do trabalhador que atendam a determinados recursos SDS ou de tipo de máquina, como nós do trabalhador bare metal. Para ver quais rótulos já estão aplicados a um recurso, use o comando <code>kubectl get</code> com o sinalizador <code>--show-labels</code>. Por exemplo:</p>
  <p><pre class="pre"><code>kubectl get node &lt;node_ID&gt; --show-labels</code></pre></p>
  Para aplicar rótulos aos nós do trabalhador, [crie seu conjunto de trabalhadores](/docs/containers?topic=containers-add_workers#add_pool) com rótulos ou [atualize um conjunto de trabalhadores existente](/docs/containers?topic=containers-add_workers#worker_pool_labels)</dd>
</dl>




<br />


## Tornando seus recursos altamente disponíveis
{: #kube_ha}

Embora nenhum sistema seja totalmente infalível, é possível executar etapas para aumentar a alta disponibilidade de seus aplicativos e serviços no {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Revise mais informações sobre como tornar os recursos altamente disponíveis.
* [Reduza possíveis pontos de falha](/docs/containers?topic=containers-ha#ha).
* [Crie clusters multizona](/docs/containers?topic=containers-ha_clusters#ha_clusters).
* [Planeje implementações altamente disponíveis](/docs/containers?topic=containers-app#highly_available_apps) que usem recursos, como conjuntos de réplicas e antiafinidade de pod, em multizonas.
* [Execute contêineres baseados em imagens em um registro público baseado em nuvem](/docs/containers?topic=containers-images).
* [Planeje o armazenamento de dados](/docs/containers?topic=containers-storage_planning#persistent_storage_overview). Especialmente para clusters multizona, considere usar um serviço de nuvem, como o [{{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) ou o [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about).
* Para clusters multizona, ative um [serviço de balanceador de carga](/docs/containers?topic=containers-loadbalancer#multi_zone_config) ou o [balanceador de carga multizona](/docs/containers?topic=containers-ingress#ingress) do Ingress para expor publicamente seus aplicativos.

<br />


## Configurando a descoberta de serviço
{: #service_discovery}

Cada um de seus pods em seu cluster Kubernetes tem um endereço IP. Mas, ao implementar um aplicativo em seu cluster, você não deseja depender do endereço IP do pod para a descoberta de serviço e a rede. Os pods são removidos e substituídos frequentemente e dinamicamente. Em vez disso, use um serviço do Kubernetes que represente um grupo de pods e forneça um ponto de entrada estável por meio do endereço IP virtual do serviço, chamado de seu `cluster IP`. Para obter mais informações, consulte a documentação do Kubernetes em [Serviços ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/services-networking/service/#discovering-services).
{:shortdesc}

### Posso customizar o provedor DNS do cluster Kubernetes?
{: #services_dns}

Quando você cria serviços e pods, eles recebem a designação de um nome DNS para que seus contêineres de aplicativo possam usar o IP de serviço DNS para resolver nomes DNS. É possível customizar o DNS do pod para especificar servidores de nomes, procuras e opções de lista de objetos. Para obter mais informações, consulte [Configurando o provedor DNS do cluster](/docs/containers?topic=containers-cluster_dns#cluster_dns).
{: shortdesc}



### Como posso garantir que meus serviços estejam conectados às implementações adequadas e prontos para uso?
{: #services_connected}

Para a maioria dos serviços, inclua um seletor em seu arquivo de serviço `.yaml` para que ele seja aplicado aos pods que executam seus aplicativos por esse rótulo. Muitas vezes, você não deseja que seu aplicativo comece a processar solicitações logo quando é iniciado. Inclua uma análise de prontidão em sua implementação para que o tráfego seja enviado apenas para um pod considerado pronto. Para obter um exemplo de uma implementação com um serviço que usa rótulos e configura uma análise de prontidão, confira esse [NGINX YAML](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml).
{: shortdesc}

Às vezes, você não deseja que o serviço use um rótulo. Por exemplo, é possível que você tenha um banco de dados externo ou deseje apontar o serviço para outro serviço em um namespace diferente dentro do cluster. Quando isso acontecer, será preciso incluir manualmente um objeto de terminais e vinculá-lo ao serviço.


### Como controlo o tráfego de rede entre os serviços executados em meu cluster?
{: #services_network_traffic}

Por padrão, os pods podem se comunicar com outros pods no cluster, mas é possível bloquear o tráfego para determinados pods ou namespaces com políticas de rede. Além disso, se seu aplicativo é exposto externamente usando um serviço NodePort, de balanceador de carga ou Ingress, será possível configurar políticas de rede avançadas para bloquear o tráfego. No {{site.data.keyword.containerlong_notm}}, é possível usar o Calico para gerenciar as [políticas de rede de controle de tráfego](/docs/containers?topic=containers-network_policies#network_policies) do Kubernetes e do Calico.

Se tiver uma variedade de microsserviços executados em plataformas para as quais precisa conectar, gerenciar e proteger o tráfego de rede, considere o uso de uma ferramenta de malha de serviços, como o [complemento gerenciado do Istio](/docs/containers?topic=containers-istio).

Também é possível [configurar nós de borda](/docs/containers?topic=containers-edge#edge) para aumentar a segurança e o isolamento de seu cluster, restringindo a carga de trabalho de rede para selecionar nós do trabalhador.



### Como posso expor meus serviços na Internet?
{: #services_expose_apps}

É possível criar três tipos de serviços para a rede externa: NodePort, LoadBalancer e Ingress. Para obter mais informações, consulte [Planejando serviços de rede](/docs/containers?topic=containers-cs_network_planning#external).

À medida que planeja quantos objetos `Service` são necessários em seu cluster, tenha em mente que o Kubernetes usa `iptables` para manipular regras de encaminhamento de rede e porta. Se você executar um grande número de serviços em seu cluster, como 5000, o desempenho poderá ser impactado.



## Implementando cargas de trabalho de aplicativo em clusters
{: #deployments}

Com o Kubernetes, você declara muitos tipos de objetos em arquivos de configuração YAML, como pods, implementações e tarefas. Esses objetos descrevem informações como quais aplicativos conteinerizados estão em execução, quais recursos eles usam e quais políticas gerenciam seu comportamento para reiniciar, atualizar, replicar e muito mais. Para obter mais informações, consulte os documentos do Kubernetes para as [Melhores práticas de configuração ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/configuration/overview/).
{: shortdesc}

### Pensei que precisava colocar meu aplicativo em um contêiner. No entanto, o que são essas informações sobre pods?
{: #deploy_pods}

Um [pod ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/pods/pod/) é a menor unidade implementável que pode ser gerenciada pelo Kubernetes. Você coloca seu contêiner (ou um grupo de contêineres) em um pod e usa o arquivo de configuração de pod para informá-lo sobre como executar o contêiner e compartilhar recursos com outros pods. Todos os contêineres que você coloca em um pod são executados em um contexto compartilhado, o que significa que eles compartilham a mesma máquina virtual ou física.
{: shortdesc}

**O que colocar em um contêiner**: à medida que pensa sobre os componentes de seu aplicativo, considere se eles têm requisitos de recurso significativamente diferentes para itens como CPU e memória. É possível que alguns componentes funcionem de maneira bastante intensa, sendo aceitável que fiquem inativos brevemente para desviar recursos para outras áreas? Há algum outro componente voltado ao cliente, de maneira que seja fundamental que ele permaneça ativo? Divida-os em contêineres separados. É sempre possível implementá-los no mesmo pod para que sejam executados em sincronia.

**O que colocar em um pod**: os contêineres para seu aplicativo não precisam sempre estar no mesmo pod. Na verdade, se você tiver um componente stateful difícil de dimensionar, como um serviço de banco de dados, coloque-o em um pod diferente que possa ser planejado em um nó do trabalhador com mais recursos para manipular a carga de trabalho. Se seus contêineres funcionarem corretamente se eles forem executados em nós do trabalhador diferentes, use múltiplos pods. Se precisarem estar na mesma máquina e ter seu dimensionamento realizado em conjunto, agrupe-os no mesmo pod.

### Então, se é possível usar apenas um pod, por que preciso de todos esses tipos diferentes de objetos?
{: #deploy_objects}

Criar um arquivo YAML de pod é fácil. É possível escrever um com apenas algumas linhas, conforme a seguir.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
{: codeblock}

Mas ainda há mais possibilidades. Se o nó no qual seu pod é executado ficar inativo, seu pod também ficará e não será reprogramado. Em vez disso, use uma [implementação ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) para suportar a reprogramação de pods, conjuntos de réplicas e atualizações contínuas. Uma implementação básica é praticamente tão fácil de fazer quanto um pod. Em vez de definir o contêiner no `spec` por si só, no entanto, você especifica `replicas` e um `template` no `spec` da implementação. O modelo possui seu próprio `spec` para os contêineres dentro dele, como a seguir.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```
{: codeblock}

É possível continuar incluindo recursos, como antiafinidade de pod ou limites de recurso, todos no mesmo arquivo YAML.

Para obter uma explicação mais detalhada de diferentes recursos que podem ser incluídos em sua implementação, consulte [Criando seu arquivo YAML de implementação do app](/docs/containers?topic=containers-app#app_yaml).
{: tip}

### Como posso organizar minhas implementações para torná-las mais fáceis de atualizar e gerenciar?
{: #deploy_organize}

Agora que você compreendeu o que pode incluir em sua implementação, pode ocorrer a dúvida sobre como gerenciar todos esses arquivos YAML diferentes. Isso sem mencionar os objetos que eles criam em seu ambiente do Kubernetes.

Veja a seguir algumas dicas para organizar arquivos YAML de implementação:
*  Use um sistema de controle de versão, como o Git.
*  Agrupe objetos do Kubernetes estritamente relacionados em um único arquivo YAML. Por exemplo, se estiver criando uma `deployment`, também será possível incluir o arquivo `service` no YAML. Separe os objetos com `---`, como a seguir:
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    ...
    ---
    apiVersion: v1
    kind: Service
    metadata:
    ...
    ```
    {: codeblock}
*  É possível usar o comando `kubectl apply -f` para aplicar a um diretório inteiro, não apenas a um único arquivo.
*  Experimente o [projeto `kustomize`](/docs/containers?topic=containers-app#kustomize) que pode ser usado para ajudar a gravar, customizar e reutilizar as configurações de YAML do recurso do Kubernetes.

Dentro do arquivo YAML, é possível usar rótulos ou anotações, como metadados, para gerenciar suas implementações.

**Rótulos**: [rótulos ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) são pares `key:value` que podem ser conectados a objetos do Kubernetes, como pods e implementações. Eles podem ser o que você desejar e são úteis para selecionar objetos com base nas informações do rótulo. Rótulos fornecem a base para agrupar objetos. Algumas ideias para rótulos:
* `app: nginx
`
* `version: v1`
* `env: dev`

**Anotações**: [anotações ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) são semelhantes a rótulos por também serem pares `key:value`. Elas são melhores para informações não identificadas que podem ser utilizadas por ferramentas ou bibliotecas, como a retenção de informações adicionais sobre o local de origem de um objeto, como usá-lo, ponteiros para repositórios de rastreamento relacionados ou uma política sobre o objeto. Objetos não são selecionados com base em anotações.

### O que mais posso fazer para preparar meu aplicativo para a implementação?
{: #deploy_prep}

Muitas coisas. Consulte [Preparando seu aplicativo conteinerizado para ser executado em clusters](/docs/containers?topic=containers-app#plan_apps). O tópico inclui informações sobre:
*  Os tipos de aplicativos que podem ser executados no Kubernetes, incluindo dicas para aplicativos stateful e stateless.
*  A migração de aplicativos para o Kubernetes.
*  Dimensionamento de cluster com base em seus requisitos de carga de trabalho.
*  Configuração de recursos de aplicativo adicionais, como serviços, armazenamento, criação de log e monitoramento da IBM.
*  Uso de variáveis dentro de sua implementação.
*  Controle do acesso ao seu aplicativo.

<br />


## Empacotando seu aplicativo
{: #packaging}

Caso deseje executar seu aplicativo em diversos clusters, em ambientes públicos e privados ou, até mesmo, em diversos provedores em nuvem, é possível que você esteja se perguntando como fazer sua estratégia de implementação funcionar nesses ambientes. Com o {{site.data.keyword.Bluemix_notm}} e outras ferramentas de software livre, é possível empacotar seu aplicativo para ajudar a automatizar implementações.
{: shortdesc}

<dl>
<dt>Automatize sua infraestrutura</dt>
  <dd>É possível usar a ferramenta de software livre [Terraform](/docs/terraform?topic=terraform-getting-started#getting-started) para automatizar o fornecimento da infraestrutura do {{site.data.keyword.Bluemix_notm}}, incluindo clusters Kubernetes. Siga esse tutorial para [planejar, criar e atualizar ambientes de implementação](/docs/tutorials?topic=solution-tutorials-plan-create-update-deployments#plan-create-update-deployments). Depois de criar um cluster, também será possível configurar o [dimensionador automático de cluster do {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ca) para que seu conjunto do trabalhador amplie ou reduza o dimensionamento dos nós do trabalhador em resposta às solicitações de recurso de sua carga de trabalho.</dd>
<dt>Configurar um pipeline de integração e entrega contínuas (CI/CD)</dt>
  <dd>Com os arquivos de configuração de seu aplicativo organizados em um sistema de gerenciamento de controle de origem, como o Git, é possível construir seu pipeline para testar e implementar códigos em diferentes ambientes, como `test` e `prod`. Confira [esse tutorial sobre Implementação contínua no Kubernetes](/docs/tutorials?topic=solution-tutorials-continuous-deployment-to-kubernetes#continuous-deployment-to-kubernetes).</dd>
<dt>Empacotar os arquivos de configuração de seu aplicativo</dt>
  <dd>Com o gerenciador de pacotes Kubernetes do [Helm ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://helm.sh/docs/), é possível especificar todos os recursos do Kubernetes necessários para seu aplicativo em um gráfico do Helm. Em seguida, é possível usar o Helm para criar os arquivos de configuração YAML e implementá-los em seu cluster. Também é possível [integrar gráficos do Helm fornecidos pelo {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) para ampliar os recursos de seu cluster, como com um plug-in de armazenamento de bloco.<p class="tip">Você está apenas procurando uma maneira fácil de criar modelos de arquivo YAML? Algumas pessoas usam o Helm para fazer apenas isso ou é possível experimentar outras ferramentas de comunidade, como o [`ytt`![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://get-ytt.io/).</p></dd>
</dl>

<br />


## Mantendo seu aplicativo atualizado
{: #updating}

Muito esforço é dedicado ao se preparar para a próxima versão de seu aplicativo. É possível usar as ferramentas de atualização do {{site.data.keyword.Bluemix_notm}} e do Kubernetes para garantir que seu aplicativo esteja em execução em um ambiente em cluster seguro, bem como para apresentar versões diferentes de seu aplicativo.
{: shortdesc}

### Como posso manter meu cluster em um estado suportado?
{: #updating_kube}

Certifique-se de que seu cluster sempre execute uma [versão suportada do Kubernetes](/docs/containers?topic=containers-cs_versions#cs_versions). Quando uma nova versão secundária do Kubernetes é liberada, uma versão mais antiga é descontinuada logo em seguida e, após isso, torna-se não suportada. Para obter mais informações, consulte [Atualizando o principal do Kubernetes](/docs/containers?topic=containers-update#master) e [nós do trabalhador](/docs/containers?topic=containers-update#worker_node).

### Quais estratégias de atualização de aplicativo posso usar?
{: #updating_apps}

Para atualizar seu aplicativo, é possível escolher dentre uma variedade de estratégias, como a seguir. É possível começar com uma implementação contínua ou uma alternância instantânea antes de continuar para uma implementação canary mais complicada.

<dl>
<dt>Implementação contínua</dt>
  <dd>É possível usar a funcionalidade nativa do Kubernetes para criar uma implementação da `v2` e substituir gradualmente sua implementação anterior da `v1`. Essa abordagem requer que os aplicativos sejam compatíveis com versões anteriores, de forma que os usuários que recebem a versão `v2` do aplicativo não experimentem nenhuma mudança interruptiva. Para obter mais informações, consulte [Gerenciando implementações contínuas para atualizar seus aplicativos](/docs/containers?topic=containers-app#app_rolling).</dd>
<dt>Alternância instantânea</dt>
  <dd>Também conhecida como uma implementação azul-verde, uma alternância instantânea requer o dobro dos recursos de cálculo para ter duas versões de um mesmo aplicativo em execução ao mesmo tempo. Com essa abordagem, é possível alternar seus usuários para a versão mais recente em tempo quase real. Certifique-se de usar seletores de rótulo de serviço (como `version: green` e `version: blue`) para garantir que as solicitações sejam enviadas para a versão correta do aplicativo. É possível criar a nova implementação `version: green`, aguardar até que ela esteja pronta e, em seguida, excluir a implementação `version: blue`. Em vez disso, também é possível executar uma [atualização contínua](/docs/containers?topic=containers-app#app_rolling), mas configurar o parâmetro `maxUnavailable` para `0%` e o parâmetro `maxSurge` para `100%`.</dd>
<dt>Implementação canary ou A/B</dt>
  <dd>Uma implementação canary, que é uma estratégia de atualização mais complexa, consiste na escolha de uma porcentagem de usuários, como 5%, e seu envio para a nova versão do aplicativo. Você coleta métricas em suas ferramentas de criação de log e monitoramento sobre como a nova versão do aplicativo está sendo executada, realiza testes A/B e, em seguida, apresenta a atualização para mais usuários. Assim como com todas as implementações, a rotulagem do aplicativo (como `version: stable` e `version: canary`) é essencial. Para gerenciar as implementações canary, é possível [instalar a malha de serviços de complemento gerenciado do Istio](/docs/containers?topic=containers-istio#istio), [configurar o monitoramento do Sysdig para seu cluster](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster) e, em seguida, usar a malha de serviços do Istio para testes A/B, conforme descrito [nesta postagem do blog ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://sysdig.com/blog/monitor-istio/). Ou, use o Knative para implementações canárias.</dd>
</dl>

<br />


## Monitorando o desempenho de seu cluster
{: #monitoring_health}

Com a criação de log e o monitoramento efetivos de seu cluster e aplicativos, é possível entender melhor seu ambiente para otimizar a utilização de recursos e solucionar problemas que possam surgir. Para configurar as soluções de criação de log e monitoramento para seu cluster, consulte [Criação de log e monitoramento](/docs/containers?topic=containers-health#health).
{: shortdesc}

À medida que configura sua criação de log e seu monitoramento, considere o seguinte.

<dl>
<dt>Colete logs e métricas para determinar o funcionamento do cluster</dt>
  <dd>O Kubernetes inclui um servidor de métricas para ajudar a determinar o desempenho básico no nível do cluster. É possível revisar essas métricas em seu [painel do Kubernetes](/docs/containers?topic=containers-app#cli_dashboard) ou em um terminal, executando comandos `kubectl top (pods | nodes)`. É possível incluir esses comandos em sua automação.<br><br>
  Encaminhe os logs para uma ferramenta de análise de log para que seja possível analisar seus logs posteriormente. Defina o detalhamento e o nível de logs que deseja registrar para evitar o armazenamento de mais logs do que o necessário. Os logs podem consumir rapidamente muito armazenamento, o que pode impactar o desempenho de seu aplicativo e tornar a análise de log mais difícil.</dd>
<dt>Teste o desempenho do aplicativo</dt>
  <dd>Depois de configurar a criação de log e o monitoramento, conduza testes de desempenho. Em um ambiente de teste, crie deliberadamente uma variedade de cenários não ideais, como a exclusão de todos os nós do trabalhador em uma zona para replicar uma falha de zona. Revise os logs e as métricas para verificar como seu aplicativo se recupera.</dd>
<dt>Prepare-se para auditorias</dt>
  <dd>Além dos logs do aplicativo e das métricas do cluster, configure o rastreamento de atividades para ter um registro auditável de quem executou qual cluster e ações do Kubernetes. Para obter mais informações, consulte [{{site.data.keyword.cloudaccesstrailshort}}](/docs/containers?topic=containers-at_events#at_events).</dd>
</dl>
