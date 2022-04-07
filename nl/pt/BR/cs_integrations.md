---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-09"

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


# Integrando Serviços
{: #integrations}

É possível usar vários serviços externos e serviços de catálogo com um cluster do Kubernetes padrão no {{site.data.keyword.containerlong}}.
{:shortdesc}


## Serviços DevOps
{: #devops_services}
<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras do DevOps. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Serviços DevOps</caption>
<thead>
<tr>
<th>Serviço</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cfee_full_notm}}</td>
<td>Implemente e gerencie sua própria plataforma Cloud Foundry na parte superior de um cluster Kubernetes para desenvolver, empacotar, implementar e gerenciar apps de nuvem nativa e aproveitar o ecossistema do {{site.data.keyword.Bluemix_notm}} para ligar serviços adicionais a seus apps. Ao criar uma instância do {{site.data.keyword.cfee_full_notm}}, deve-se configurar o cluster Kubernetes, escolhendo o tipo de máquina e as VLANs para seus nós do trabalhador. Em seguida, seu cluster é fornecido com o {{site.data.keyword.containerlong_notm}} e o {{site.data.keyword.cfee_full_notm}} é implementado automaticamente em seu cluster. Para obter mais informações sobre como configurar o {{site.data.keyword.cfee_full_notm}}, consulte o [Tutorial de introdução](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started). </td>
</tr>
<tr>
<td>Codeship</td>
<td>É possível usar o <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> para a integração e entrega contínua de contêineres. Para obter mais informações, veja <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Usando o Codeship Pro para implementar cargas de trabalho no {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. </td>
</tr>
<tr>
<td>Grafeas</td>
<td>[Grafeas ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://grafeas.io) é um serviço CI/CD de software livre que fornece uma maneira comum para recuperar, armazenar e trocar metadados durante o processo de cadeia de suprimento de software. Por exemplo, se você integrar o Grafeas em seu processo de construção de app, o Grafeas poderá armazenar informações sobre o inicializador da solicitação de construção, os resultados da varredura de vulnerabilidade e o sign off de garantia de qualidade para que seja possível tomar uma decisão informada se um app puder ser implementado em produção. É possível usar esses metadados em auditorias ou para provar a conformidade para a cadeia de suprimento de software. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh" target="_blank">O Helm <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> é um gerenciador de pacote do Kubernetes. É possível criar novos gráficos Helm ou usar gráficos Helm preexistente para definir, instalar e fazer upgrade de aplicativos complexos do Kubernetes que são executados em clusters do {{site.data.keyword.containerlong_notm}}. <p>Para obter mais informações, veja [Configurando o Helm no {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-integrations#helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automatize as construções de app e as implementações do contêiner para clusters do Kubernetes usando uma cadeia de ferramentas. Para obter informações de configuração, consulte o blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Implementar pods do Kubernetes no {{site.data.keyword.containerlong_notm}} usando DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. </td>
</tr>
<tr>
<td>Istio no  {{site.data.keyword.containerlong_notm}}</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> é um serviço de software livre que fornece aos desenvolvedores uma maneira de conectar, proteger, gerenciar e monitorar uma rede de microsserviços, também conhecida como uma malha de serviços, em plataformas de orquestração de nuvem. Istio on {{site.data.keyword.containerlong}} fornece uma instalação de uma etapa do Istio no seu cluster por meio de um complemento gerenciado. Com um clique, é possível obter todos os componentes principais do Istio, rastreio adicional, monitoramento e visualização e o aplicativo de amostra BookInfo funcionando. Para iniciar, consulte [Usando o complemento Istio gerenciado (beta)](/docs/containers?topic=containers-istio).</td>
</tr>
<tr>
<td>Jenkins X</td>
<td>Jenkins X é uma plataforma de integração contínua nativa do Kubernetes e uma plataforma de entrega contínua que pode ser usada para automatizar seu processo de construção. Para obter mais informações sobre como instalá-lo no {{site.data.keyword.containerlong_notm}}, consulte [Introduzindo o projeto de software livre Jenkins X ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/blogs/bluemix/2018/08/installing-jenkins-x-on-ibm-cloud-kubernetes-service/).</td>
</tr>
<tr>
<td>Knative</td>
<td>[Knative ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/knative/docs) é uma plataforma de software livre que foi desenvolvida pela IBM, Google, Pivotal, Red Hat, Cisco e outras com o objetivo de estender os recursos do Kubernetes para ajudá-lo a criar apps modernos, de conteinerização centrados em origem e sem servidor sobre o cluster Kubernetes. A plataforma usa uma abordagem consistente entre linguagens de programação e estruturas para abstrair a carga operacional de construção, implementação e gerenciamento de cargas de trabalho em Kubernetes para que os desenvolvedores possam se concentrar no que mais importa para eles: o código-fonte. Para obter mais informações, consulte [Tutorial: usando Knative gerenciado para executar apps sem servidor em clusters Kubernetes](/docs/containers?topic=containers-knative_tutorial#knative_tutorial). </td>
</tr>
</tbody>
</table>

<br />



## Serviços de criação de log e de monitoramento
{: #health_services}
<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras de criação de log e monitoramento. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Serviços de criação de log e de monitoramento</caption>
<thead>
<tr>
<th>Serviço</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>Monitore nós do trabalhado, contêineres, conjuntos de réplicas, controladores de replicação e serviços com o <a href="https://www.newrelic.com/coscale" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. Para obter mais informações, veja <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitorando o {{site.data.keyword.containerlong_notm}} com o CoScale <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. </td>
</tr>
<tr>
<td>Datadog</td>
<td>Monitore seu cluster e visualize as métricas de desempenho da infraestrutura e do aplicativo com o <a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. Para obter mais informações, veja <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitorando o {{site.data.keyword.containerlong_notm}} com o Datadog <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Monitore a atividade administrativa feita em seu cluster, analisando logs por meio do Grafana. Para obter mais informações sobre o serviço, consulte a documentação do [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla). Para obter mais informações sobre os tipos de eventos que podem ser rastreados, consulte [Eventos do Activity Tracker](/docs/containers?topic=containers-at_events).</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full_notm}}</td>
<td>Inclua recursos de gerenciamento de log no cluster implementando LogDNA como um serviço de terceiro para seus nós do trabalhador para gerenciar logs dos contêineres de pod. Para obter mais informações, consulte [Gerenciando logs de cluster do Kubernetes com o {{site.data.keyword.loganalysisfull_notm}} com LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full_notm}}</td>
<td>Obtenha visibilidade operacional para o desempenho e o funcionamento de seus apps implementando o Sysdig como um serviço de terceiro para os nós do trabalhador para encaminhar métricas para o {{site.data.keyword.monitoringlong}}. Para obter mais informações, consulte [Analisando métricas para um app implementado em um cluster do Kubernetes](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). </td>
</tr>
<tr>
<td>Instana</td>
<td> O <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> fornece monitoramento de desempenho de infraestrutura e app com uma GUI que descobre automaticamente e mapeia seus apps. O Instana captura cada solicitação para seus apps, o que pode ser usado para solucionar problemas e executar análise de causa raiz para evitar que problemas aconteçam novamente. Consulte a postagem do blog sobre a <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">implementação do Instana no {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> para saber mais.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>O Prometheus é uma ferramenta de monitoramento, criação de log e alerta de software livre que foi projetada especificamente para o Kubernetes. O Prometheus recupera informações detalhadas sobre o cluster, os nós do trabalhador e o funcionamento de implementação com base nas informações de criação de log do Kubernetes. A atividade de CPU, memória, E/S e rede é coletada para cada contêiner em execução em um cluster. É possível usar os dados coletados em consultas ou alertas customizados para monitorar o desempenho e as cargas de trabalho em seu cluster.

<p>Para usar o Prometheus, siga as <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">instruções do CoreOS <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>Visualize métricas e logs para seus aplicativos conteinerizados usando o <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. Para obter mais informações, veja <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Monitoramento e criação de log para contêineres com o Sematext <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. </td>
</tr>
<tr>
<td>Splunk</td>
<td>Importe e procure os dados de criação de log, objeto e métricas do Kubernetes em Splunk usando o Splunk Connect para Kubernetes. O Splunk Connect for Kubernetes é uma coleção de gráficos do Helm que implementa uma implementação suportada por Splunk do Fluentd para o cluster Kubernetes, um plug-in do Coletor de eventos HTTP (HEC) do Fluentd construído por Splunk para enviar logs e metadados e uma implementação de métricas que captura suas métricas de cluster. Para obter mais informações, consulte <a href="https://www.ibm.com/blogs/bluemix/2019/02/solving-business-problems-with-splunk-on-ibm-cloud-kubernetes-service/" target="_blank">Solucionando problemas de negócios com o Splunk no {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.</td>
</tr>
<tr>
<td>Weave Scope</td>
<td>O Weave Scope fornece um diagrama visual de seus recursos dentro de um cluster do Kubernetes, incluindo serviços, pods, contêineres, processos, nós e muito mais. O Weave Scope fornece métricas interativas para CPU e memória e também fornece ferramentas para uso de tail e executável em um contêiner.<p>Para obter mais informações, veja [Visualizando os recursos de cluster do Kubernetes com o Weave Scope e o {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-integrations#weavescope).</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## Serviços de segurança
{: #security_services}

Deseja uma visualização abrangente de como integrar os serviços de segurança do {{site.data.keyword.Bluemix_notm}} ao seu cluster? Verifique o [tutorial Aplicar a segurança de ponta a ponta a um aplicativo em nuvem](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security).
{: shortdesc}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos de segurança extra. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Serviços de segurança</caption>
<thead>
<tr>
<th>Serviço</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>Inclua um nível de segurança para seus apps com [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started), requerendo que os usuários se conectem. Para autenticar solicitações de web ou de HTTP/HTTPS da API para o seu app, é possível integrar o {{site.data.keyword.appid_short_notm}} com o seu serviço do Ingress usando a [anotação do Ingress de autenticação do {{site.data.keyword.appid_short_notm}}](/docs/containers?topic=containers-ingress_annotation#appid-auth).</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>Como um suplemento para o <a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a>, é possível usar o <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> para melhorar a segurança de implementações de contêiner, reduzindo o que seu app pode fazer. Para obter mais informações, consulte <a href="https://blog.aquasec.com/securing-container-deployments-on-bluemix-with-aqua-security" target="_blank">Protegendo implementações de contêiner no {{site.data.keyword.Bluemix_notm}} com Aqua Security <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>É possível usar o <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> para armazenar e gerenciar certificados SSL para seus apps. Para obter mais informações, consulte <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Usar o {{site.data.keyword.cloudcerts_long_notm}} com o {{site.data.keyword.containerlong_notm}} para implementar certificados TLS de domínio customizado <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. </td>
</tr>
<tr>
  <td>{{site.data.keyword.datashield_full}}  (Beta)</td>
  <td>É possível usar o <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> para criptografar sua memória de dados. O {{site.data.keyword.datashield_short}} está integrado às tecnologias Intel® Software Guard Extensions (SGX) e Fortanix® para que seu código de carga de trabalho de contêiner do {{site.data.keyword.Bluemix_notm}} e os dados sejam protegidos em uso. O código do app e os dados são executados em enclaves reforçados pela CPU, que são áreas confiáveis de memória no nó do trabalhador que protegem os aspectos críticos do app, o que ajuda a manter o código e os dados confidenciais e não modificados.</td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>Configure o seu próprio repositório de imagem assegurado do Docker no qual é possível armazenar e compartilhar imagens com segurança entre usuários do cluster. Para obter mais informações, consulte a documentação do <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">{{site.data.keyword.registrylong}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.keymanagementservicefull}}</td>
  <td>Criptografe os segredos do Kubernetes que estão em seu cluster ativando o {{site.data.keyword.keymanagementserviceshort}}. A criptografia de seus segredos do Kubernetes evita que usuários não autorizados acessem informações confidenciais de cluster.<br>Para configurar, veja <a href="/docs/containers?topic=containers-encryption#keyprotect">Criptografando segredos do Kubernetes usando o {{site.data.keyword.keymanagementserviceshort}}</a>.<br>Para obter mais informações, consulte a documentação do <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.</td>
</tr>
<tr>
<td>NeuVector</td>
<td>Proteja os contêineres com um firewall de nuvem nativa usando o <a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. Para obter mais informações, veja <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. </td>
</tr>
<tr>
<td>Twistlock</td>
<td>Como um suplemento para o <a href="/docs/services/va?topic=va-va_index" target="_blank">Vulnerability Advisor</a>, é possível usar o <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> para gerenciar firewalls, proteção de ameaça e resposta de incidente. Para obter mais informações, consulte <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock no {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Serviços de Armazenamento
{: #storage_services}
<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos de armazenamento persistente. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Serviços de Armazenamento</caption>
<thead>
<tr>
<th>Serviço</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Velero</td>
  <td>É possível usar o <a href="https://github.com/heptio/velero" target="_blank">Heptio Velero <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> para fazer backup e restaurar recursos de cluster e volumes persistentes. Para obter mais informações, consulte o Heptio Velero <a href="https://github.com/heptio/velero/blob/release-0.9/docs/use-cases.md" target="_blank">Usar Casos para Recuperação de Desastre e Migração de Cluster <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
  <td>[{{site.data.keyword.Bluemix_notm}} O Armazenamento de bloco](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) é um armazenamento de iSCSI persistente, de alto desempenho que pode ser incluído em seus apps usando volumes persistentes (PVs) do Kubernetes. Use o armazenamento de bloco para implementar apps stateful em uma zona única ou como armazenamento de alto desempenho para os pods únicos. Para obter mais informações sobre como fornecer armazenamento de bloco em seu cluster, consulte [Armazenando dados no {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/containers?topic=containers-block_storage#block_storage)</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>Os dados armazenados com o {{site.data.keyword.cos_short}} são criptografados e dispersos em múltiplas localizações geográficas e acessados por HTTP usando uma API de REST. É possível usar [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) para configurar o serviço para fazer backups únicos ou planejados para dados em seus clusters. Para obter informações gerais sobre o serviço, consulte a documentação do <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage" target="_blank">{{site.data.keyword.cos_short}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.</td>
</tr>
  <tr>
  <td>{{site.data.keyword.Bluemix_notm}}  Armazenamento de arquivo</td>
  <td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) é um armazenamento de arquivo baseado em NFS persistente, rápido e flexível, que você pode incluir em seus apps usando volumes persistentes do Kubernetes. É possível escolher entre camadas de armazenamento predefinidas com tamanhos de GB e IOPS que atendam aos requisitos de suas cargas de trabalho. Para obter mais informações sobre como fornecer o armazenamento de arquivo em seu cluster, consulte [Armazenando dados no {{site.data.keyword.Bluemix_notm}} File Storage](/docs/containers?topic=containers-file_storage#file_storage).</td>
  </tr>
  <tr>
    <td>Portworx</td>
    <td>[Portworx ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://portworx.com/products/introduction/) é uma solução de armazenamento definido por software altamente disponível que pode ser usada para gerenciar o armazenamento persistente para seus bancos de dados conteinerizados e outros apps stateful ou para compartilhar dados entre os pods em múltiplas zonas. É possível instalar o Portworx com um gráfico do Helm e fornecer o armazenamento para seus apps usando volumes persistentes do Kubernetes. Para obter mais informações sobre como configurar o Portworx em seu cluster, consulte [Armazenando Dados em Armazenamento Definido por Software (SDS) com Portworx](/docs/containers?topic=containers-portworx#portworx).</td>
  </tr>
</tbody>
</table>

<br />


## Serviços de banco de
{: #database_services}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos do banco de dados. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Serviços de banco de</caption>
<thead>
<tr>
<th>Serviço</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.blockchainfull_notm}} Plataforma 2.0 beta</td>
    <td>Implemente e gerencie o seu próprio {{site.data.keyword.blockchainfull_notm}} Platform no {{site.data.keyword.containerlong_notm}}. Com o {{site.data.keyword.blockchainfull_notm}} Platform 2.0, é possível hospedar as redes do {{site.data.keyword.blockchainfull_notm}} ou criar organizações que podem se associar a outras redes do {{site.data.keyword.blockchainfull_notm}} 2.0. Para obter mais informações sobre como configurar o {{site.data.keyword.blockchainfull_notm}} no {{site.data.keyword.containerlong_notm}}, consulte [Sobre o {{site.data.keyword.blockchainfull_notm}} Platform free 2.0 beta](/docs/services/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview).</td>
  </tr>
<tr>
  <td>Bancos de dados em</td>
  <td>É possível escolher entre uma variedade de serviços de banco de dados do {{site.data.keyword.Bluemix_notm}}, como o {{site.data.keyword.composeForMongoDB_full}} ou o {{site.data.keyword.cloudantfull}} para implementar soluções de banco de dados altamente disponíveis e escaláveis em seu cluster. Para obter uma lista de bancos de dados em nuvem disponíveis, consulte o [Catálogo do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/catalog?category=databases).  </td>
  </tr>
  </tbody>
  </table>


## Incluindo serviços do {{site.data.keyword.Bluemix_notm}} nos clusters
{: #adding_cluster}

Inclua serviços do {{site.data.keyword.Bluemix_notm}} para aprimorar o cluster do Kubernetes com recursos extras em áreas como Watson AI, dados, segurança e Internet of Things (IoT).
{:shortdesc}

É possível ligar somente serviços que suportem chaves de serviço. Para localizar uma lista com serviços que suportem chaves de serviço, consulte [Permitindo que aplicativos externos usem serviços do {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).
{: note}

Antes de iniciar:
- Assegure-se de que você tenha as funções a seguir:
    - [Função de serviço **Editor** ou **Administrador** do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o cluster.
    - [Função de serviço **Gravador** ou **Gerenciador** do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o namespace no qual deseja ligar o serviço
    - [Função **Desenvolvedor** do Cloud Foundry](/docs/iam?topic=iam-mngcf#mngcf) para o espaço que você deseja usar
- [Efetue login em sua conta. Destine a região apropriada e, se aplicável, o grupo de recursos. Configure o contexto para seu cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Para incluir um serviço do {{site.data.keyword.Bluemix_notm}} em seu cluster:

1. [Crie uma instância do serviço do {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).
    * Alguns serviços do {{site.data.keyword.Bluemix_notm}} estão disponíveis somente em regiões selecionadas. Será possível ligar um serviço a seu cluster somente se o serviço estiver disponível na mesma região que seu cluster. Além disso, se você deseja criar uma instância de serviço na zona Washington DC, deve-se usar a CLI.
    * Deve-se criar a instância de serviço no mesmo grupo de recursos que seu cluster. Um recurso pode ser criado em apenas um grupo de recursos que não pode ser mudado posteriormente.

2. Verifique o tipo de serviço que você criou e anote o **Nome** da instância de serviço.
   - **Serviços do Cloud Foundry:**
     ```
     Lista de serviços ibmcloud
     ```
     {: pre}

     Saída de exemplo:
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **Serviços ativados pelo {{site.data.keyword.Bluemix_notm}} IAM:**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}

     Saída de exemplo:
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   Também é possível ver os diferentes tipos de serviço em seu painel como **Serviços do Cloud Foundry** e **Serviços**.

3. Identifique o espaço de nomes de cluster que você deseja usar para incluir o seu serviço. Escolha entre as opções
a seguir.
     ```
     kubectl get namespaces
     ```
     {: pre}

4.  Inclua o serviço em seu cluster usando o [comando](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_bind) `ibmcloud ks cluster-service-bind`. Para os serviços ativados pelo {{site.data.keyword.Bluemix_notm}} IAM, certifique-se de usar o alias do Cloud Foundry que você criou anteriormente. Para serviços ativados pelo IAM, também é possível usar a função de acesso de serviço **Gravador** padrão ou especificar a função de acesso de serviço com o sinalizador `--role`. O comando cria uma chave de serviço para a instância de serviço ou é possível incluir a sinalização `--key` para usar credenciais de chave de serviço existentes. Se você usar o sinalizador `--key`, não inclua o sinalizador `--role`.
    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
    ```
    {: pre}

    Quando o serviço é incluído com sucesso em seu cluster, é criado um segredo de cluster que contém as credenciais de sua instância de serviço. Os segredos são criptografados automaticamente em etcd para proteger seus dados.

    Saída de exemplo:
    ```
    ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

5.  Verifique as credenciais de serviço em seu segredo do Kubernetes.
    1. Obtenha os detalhes do segredo e anote o valor de **binding**. O valor de **binding** é codificado em Base64 e contém as credenciais para sua instância de serviço no formato JSON.
       ```
       kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
       ```
       {: pre}

       Saída de exemplo:
       ```
       apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
       ```
       {: screen}

    2. Decode o valor de ligação.
       ```
       echo "<binding>" | base64 -D
       ```
       {: pre}

       Saída de exemplo:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    3. Opcional: compare as credenciais de serviço que você decodificou na etapa anterior com as credenciais de serviço localizadas para sua instância de serviço no painel do {{site.data.keyword.Bluemix_notm}}.

6. Agora que seu serviço está ligado a seu cluster, deve-se configurar seu app para [acessar as credenciais de serviço no segredo do Kubernetes](#adding_app).


## Acessando credenciais de serviço de seus apps
{: #adding_app}

Para acessar uma instância de serviço do {{site.data.keyword.Bluemix_notm}} por meio de seu app, deve-se tornar as credenciais de serviço que estão armazenadas no segredo do Kubernetes disponíveis para seu app.
{: shortdesc}

As credenciais de uma instância de serviço são codificadas em base64 e armazenadas dentro de seu segredo no formato JSON. Para acessar os dados em seu segredo, escolha entre as opções a seguir:
- [Montar o segredo como um volume para o pod](#mount_secret)
- [ Referenciar o segredo em variáveis de ambiente ](#reference_secret)
<br>
Deseja tornar seus segredos ainda mais seguros? Peça ao administrador de cluster para [ativar o {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) em seu cluster para criptografar segredos novos e existentes, como o segredo que armazena as credenciais de suas instâncias de serviço do {{site.data.keyword.Bluemix_notm}}.
{: tip}

Antes de iniciar:
-  Assegure-se de que você tenha a [função de serviço **Gravador** ou **Gerenciador** do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o namespace `kube-system`.
- [Efetue login em sua conta. Destine a região apropriada e, se aplicável, o grupo de recursos. Configure o contexto para seu cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- [ Inclua um serviço  {{site.data.keyword.Bluemix_notm}}  em seu cluster ](#adding_cluster).

### Montando o segredo como um volume em seu pod
{: #mount_secret}

Quando você monta o segredo como um volume em seu pod, um arquivo denominado `binding` é armazenado no diretório de montagem do volume. O arquivo `binding` em formato JSON inclui todas as informações e credenciais necessárias para acessar o serviço do {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

1.  Liste os segredos disponíveis em seu cluster e anote o **nome** de seu segredo. Procure um segredo do tipo **Opaco**. Se existirem múltiplos segredos, entre em contato com o administrador de cluster para identificar o segredo do serviço correto.

    ```
    kubectl get secrets
    ```
    {: pre}

    Saída de exemplo:

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m

    ```
    {: screen}

2.  Crie um arquivo YAML para sua implementação do Kubernetes e monte o segredo como um volume em seu pod.
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: registry.bluemix.net/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>Entendendo os componentes de arquivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code> volumeMounts.mountPath </code></td>
    <td>O caminho absoluto do diretório no qual o volume está montado dentro do contêiner.</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>O nome do volume a ser montado no pod.</td>
    </tr>
    <tr>
    <td><code> secret.defaultMode </code></td>
    <td>As permissões de leitura e gravação no segredo. Use  ` 420 `  para configurar permissões somente leitura. </td>
    </tr>
    <tr>
    <td><code> secret.secretName </code></td>
    <td>O nome do segredo que você anotou na etapa anterior.</td>
    </tr></tbody></table>

3.  Crie o pod e monte o segredo como um volume.
    ```
    Kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  Verifique se o pod foi criado.
    ```
    kubectl get pods
    ```
    {: pre}

    Exemplo de saída da CLI:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  Acesse as credenciais de serviço.
    1. Efetue login em seu pod.
       ```
       kubectl exec < pod_name> -it bash
       ```
       {: pre}

    2. Navegue para o caminho de montagem do volume que você definiu anteriormente e liste os arquivos em seu caminho de montagem do volume.
       ```
       cd < volume_mountpath> & & ls
       ```
       {: pre}

       Saída de exemplo:
       ```
       ligando
       ```
       {: screen}

       O arquivo `binding` inclui as credenciais de serviço que você armazenou no segredo do Kubernetes.

    4. Visualize as credenciais de serviço. As credenciais são armazenadas como pares chave-valor em formato JSON.
       ```
       ligação cat
       ```
       {: pre}

       Saída de exemplo:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. Configure seu app para analisar o conteúdo JSON e recuperar as informações necessárias para acessar seu serviço.


### Referenciando o segredo em variáveis de ambiente
{: #reference_secret}

É possível incluir as credenciais de serviço e outros pares chave-valor de seu segredo do Kubernetes como variáveis de ambiente para a sua implementação.
{: shortdesc}

1. Liste os segredos disponíveis em seu cluster e anote o **nome** de seu segredo. Procure um segredo do tipo **Opaco**. Se existirem múltiplos segredos, entre em contato com o administrador de cluster para identificar o segredo do serviço correto.

    ```
    kubectl get secrets
    ```
    {: pre}

    Saída de exemplo:

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
    ```
    {: screen}

2. Obtenha os detalhes do seu segredo para localizar potenciais pares chave-valor que é possível referenciar como variáveis de ambiente em seu pod. As credenciais de serviço são armazenadas na chave `binding` de seu segredo.
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   Saída de exemplo:
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Crie um arquivo YAML para a sua implementação do Kubernetes e especifique uma variável de ambiente que referencie a chave `binding`.
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: registry.bluemix.net/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>Entendendo os componentes de arquivo YAML</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
     </thead>
     <tbody>
     <tr>
     <td><code> containers.env.name </code></td>
     <td>O nome de sua variável de ambiente.</td>
     </tr>
     <tr>
     <td><code> env.valueFrom.secretKeyRef.name </code></td>
     <td>O nome do segredo que você anotou na etapa anterior.</td>
     </tr>
     <tr>
     <td><code> env.valueFrom.secretKeyRef.key </code></td>
     <td>A chave que faz parte de seu segredo e que você deseja referenciar em sua variável de ambiente. Para referenciar as credenciais de serviço, deve-se usar a chave <strong>binding</strong>.  </td>
     </tr>
     </tbody></table>

4. Crie o pod que referencia a chave `binding` de seu segredo como uma variável de ambiente.
   ```
   Kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. Verifique se o pod foi criado.
   ```
   kubectl get pods
   ```
   {: pre}

   Exemplo de saída da CLI:
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. Verifique se a variável de ambiente está configurada corretamente.
   1. Efetue login em seu pod.
      ```
      kubectl exec < pod_name> -it bash
      ```
      {: pre}

   2. Liste todas as variáveis de ambiente no pod.
      ```
      env
      ```
      {: pre}

      Saída de exemplo:
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. Configure seu app para ler a variável de ambiente e para analisar o conteúdo JSON para recuperar as informações que você precisa para acessar seu serviço.

   Código de exemplo em Python:
   ```
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

## Configurando o Helm no  {{site.data.keyword.containerlong_notm}}
{: #helm}

[Helm ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://helm.sh) é um gerenciador de pacote do Kubernetes. É possível criar gráficos Helm ou usar gráficos Helm preexistentes para definir, instalar e fazer upgrade de aplicativos complexos do Kubernetes complexo de upgrade que são executados em clusters do {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Para implementar os gráficos do Helm, deve-se instalar a CLI do Helm em sua máquina local e instalar o Tiller do servidor Helm em seu cluster. A imagem para o Tiller é armazenada no Google Container Registry público. Para acessar a imagem durante a instalação do Tiller, seu cluster deve permitir a conectividade de rede pública para o Google Container Registry público. Os clusters que têm o terminal em serviço público ativado podem acessar automaticamente a imagem. Os clusters privados que são protegidos com um firewall customizado ou clusters que ativaram o terminal em serviço privado apenas, não permitem acesso à imagem do Tiller. Em vez disso, é possível [extrair a imagem para a sua máquina local e enviar por push a imagem para o seu namespace no {{site.data.keyword.registryshort_notm}}](#private_local_tiller) ou [instalar os gráficos do Helm sem usar o Tiller](#private_install_without_tiller).
{: note}

### Configurando o Helm em um cluster com acesso público
{: #public_helm_install}

Se o seu cluster tiver ativado o terminal em serviço público, será possível instalar o Tiller usando a imagem pública no Registro do Google Container.
{: shortdesc}

Antes de iniciar: [Efetue login em sua conta. Destine a região apropriada e, se aplicável, o grupo de recursos. Configure o contexto para seu cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

1. Instale a <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI do Helm <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.

2. **Importante**: para manter a segurança do cluster, crie uma conta de serviço para o Tiller no namespace `kube-system` e uma ligação de função de cluster RBAC do Kubernetes para o pod `tiller-deploy` aplicando o arquivo `.yaml` a seguir por meio do [repositório `kube-samples` do {{site.data.keyword.Bluemix_notm}}](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml). **Nota**: para instalar o Tiller com a conta do serviço e a ligação de função de cluster no namespace `kube-system`, deve-se ter a função [`cluster-admin`](/docs/containers?topic=containers-users#access_policies).
    ```
    kubectl aplicar -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: pre}

3. Inicialize o Helm e instale o Tiller com a conta de serviço que você criou.

    ```
    Helm init -- tiller de serviço da conta
    ```
    {: pre}

4.  Verifique se a instalação foi bem-sucedida.
    1.  Verifique se a conta de serviço do Tiller foi criada.
        ```
        kubectl get serviceaccount -n kube-system tiller
        ```
        {: pre}

        Saída de exemplo:

        ```
        NAME SECRETS AGE tiller 1 2m
        ```
        {: screen}

    2.  Verifique se o pod `tiller-deploy` tem um **Status** de `Executando` em seu cluster.
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        Saída de exemplo:

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

5. Inclua os repositórios Helm do {{site.data.keyword.Bluemix_notm}} em sua instância do Helm.
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

6. Atualize os repositórios para recuperar as versões mais recentes de todos os gráficos do Helm.
   ```
   helm repo update
   ```
   {: pre}

7. Liste os gráficos Helm que estão atualmente disponíveis nos repositórios do {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

8. Identifique o gráfico do Helm que você deseja instalar e siga as instruções no `README` do gráfico do Helm para instalar o gráfico do Helm em seu cluster.


### Clusters privados: enviando por push a imagem do Tiller para seu registro privado no {{site.data.keyword.registryshort_notm}}
{: #private_local_tiller}

É possível extrair a imagem do Tiller para sua máquina local, enviá-la por push para seu namespace no {{site.data.keyword.registryshort_notm}} e permitir que o Helm instale o Tiller usando a imagem no {{site.data.keyword.registryshort_notm}}.
{: shortdesc}

Antes de iniciar:
- Instale o Docker em sua máquina local. Se você instalou a [CLI do {{site.data.keyword.Bluemix_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli), o Docker já está instalado.
- [Instale o plug-in da CLI do {{site.data.keyword.registryshort_notm}} e configure um namespace](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install).

Para instalar o Tiller usando o  {{site.data.keyword.registryshort_notm}}:

1. Instale a <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI do Helm <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> em sua máquina local.
2. Conecte-se ao seu cluster privado usando o túnel VPN da infraestrutura do {{site.data.keyword.Bluemix_notm}} que você configurou.
3. **Importante**: para manter a segurança do cluster, crie uma conta de serviço para o Tiller no namespace `kube-system` e uma ligação de função de cluster RBAC do Kubernetes para o pod `tiller-deploy` aplicando o arquivo `.yaml` a seguir por meio do [repositório `kube-samples` do {{site.data.keyword.Bluemix_notm}}](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml). **Nota**: para instalar o Tiller com a conta do serviço e a ligação de função de cluster no namespace `kube-system`, deve-se ter a função [`cluster-admin`](/docs/containers?topic=containers-users#access_policies).
    1. [Obtenha a conta de serviço do Kubernetes e os arquivos YAML de ligação de função de cluster ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml).

    2. Crie os recursos do Kubernetes em seu cluster.
       ```
       kubectl aplicar -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [Localize a versão do Tiller ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30)] que você deseja instalar em seu cluster. Se você não precisar de uma versão específica, use a mais recente.

5. Extraia a imagem do Tiller para sua máquina local.
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   Saída de exemplo:
   ```
   docker, puxe gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling a partir de kubernetes-helm/tiller
   48ecbb6b270e: Pull concluído
   d3fa0712c71b: Pull concluído
   bf13a43b92e9: Pull concluído
   b3f98be98675: Pull concluído
   Digest: sha256 :c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbbb463aa6
   Status: Imagem mais recente descarregada para gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [Envie por push a imagem do Tiller para seu namespace no {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing).

7. [Copie o segredo de extração de imagem para acessar o {{site.data.keyword.registryshort_notm}} por meio do namespace padrão para o namespace `kube-system`](/docs/containers?topic=containers-images#copy_imagePullSecret).

8. Instale o Tiller em seu cluster privado usando a imagem que você armazenou em seu namespace no {{site.data.keyword.registryshort_notm}}.
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. Inclua os repositórios Helm do {{site.data.keyword.Bluemix_notm}} em sua instância do Helm.
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

10. Atualize os repositórios para recuperar as versões mais recentes de todos os gráficos do Helm.
    ```
    helm repo update
    ```
    {: pre}

11. Liste os gráficos Helm que estão atualmente disponíveis nos repositórios do {{site.data.keyword.Bluemix_notm}}.
    ```
    helm search ibm
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. Identifique o gráfico do Helm que você deseja instalar e siga as instruções no `README` do gráfico do Helm para instalar o gráfico do Helm em seu cluster.

### Clusters Privados: Instalando os Gráficos Helm sem Usar o Tiller
{: #private_install_without_tiller}

Se você não desejar instalar o Tiller em seu cluster privado, será possível criar manualmente os arquivos YAML do gráfico do Helm e aplicá-los usando os comandos `kubectl`.
{: shortdesc}

As etapas neste exemplo mostram como instalar os gráficos do Helm por meio dos repositórios do gráfico do Helm do {{site.data.keyword.Bluemix_notm}} em seu cluster privado. Se você deseja instalar um gráfico do Helm que não está armazenado em um dos repositórios do gráfico do Helm do {{site.data.keyword.Bluemix_notm}}, deve-se seguir as instruções neste tópico para criar os arquivos YAML para o seu gráfico do Helm. Além disso, deve-se fazer download da imagem do gráfico do Helm por meio do registro do contêiner público, enviá-la por push para seu namespace no {{site.data.keyword.registryshort_notm}} e atualizar o arquivo `values.yaml` para usar a imagem no {{site.data.keyword.registryshort_notm}}.
{: note}

1. Instale a <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI do Helm <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> em sua máquina local.
2. Conecte-se ao seu cluster privado usando o túnel VPN da infraestrutura do {{site.data.keyword.Bluemix_notm}} que você configurou.
3. Inclua os repositórios Helm do {{site.data.keyword.Bluemix_notm}} em sua instância do Helm.
   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://registry.bluemix.net/helm/ibm-charts
   ```
   {: pre}

4. Atualize os repositórios para recuperar as versões mais recentes de todos os gráficos do Helm.
   ```
   helm repo update
   ```
   {: pre}

5. Liste os gráficos Helm que estão atualmente disponíveis nos repositórios do {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search ibm
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. Identifique o gráfico do Helm que deseja instalar, faça download dele em sua máquina local e descompacte os arquivos de seu gráfico do Helm. O exemplo a seguir mostra como fazer download do gráfico Helm para o cluster autoscaler versão 1.0.3 e descompactar os arquivos em um diretório `cluster-autoscaler`.
   ```
   helm fetch ibm/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Navegue para o diretório no qual você descompactado os arquivos do gráfico do Helm.
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Crie um diretório `output` para os arquivos YAML que você gerar usando os arquivos em seu gráfico do Helm.
   ```
   Saída mkdir
   ```
   {: pre}

9. Abra o arquivo `values.yaml` e faça qualquer mudança que seja requerida pelas instruções de instalação do gráfico do Helm.
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. Use sua instalação do Helm local para criar todos os arquivos YAML do Kubernetes para seu gráfico do Helm. Os arquivos YAML são armazenados no diretório `output` que você criou anteriormente.
    ```
    gabarito do leme -- valores ./ibm-iks-cluster-autoscaler/values.yaml -- output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Saída de exemplo:
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. Implemente todos os arquivos YAML em seu cluster privado.
    ```
    kubectl apply -- recursive -- filename ./output
    ```
   {: pre}

12. Opcional: remova todos os arquivos YAML do diretório `output`.
    ```
    kubectl delete -- recursive -- filename ./output
    ```
    {: pre}


### Links relacionados do Helm
{: #helm_links}

* Para usar o gráfico Helm do strongSwan, veja [Configurando a conectividade VPN com o gráfico Helm do serviço de VPN IPSec do strongSwan](/docs/containers?topic=containers-vpn#vpn-setup).
* Visualize os gráficos Helm disponíveis que podem ser usados com o {{site.data.keyword.Bluemix_notm}} no [Catálogo de gráficos do Helm ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/kubernetes/solutions/helm-charts) no console.
* Saiba mais sobre os comandos do Helm que são usados para configurar e gerenciar os gráficos Helm na <a href="https://docs.helm.sh/helm/" target="_blank">documentação do Helm <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.
* Saiba mais sobre como é possível [aumentar a velocidade de implementação com os gráficos Helm do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).

## Visualizando recursos do cluster Kubernetes
{: #weavescope}

O Weave Scope fornece um diagrama visual de seus recursos dentro de um cluster do Kubernetes, incluindo serviços, pods, contêineres e muito mais. O Weave Scope fornece métricas interativas para CPU e memória e ferramentas para rodapé e executável em um contêiner.
{:shortdesc}

Antes de iniciar:

-   Lembre-se de não expor as suas informações do cluster na Internet pública. Conclua estas etapas para implementar o Weave Scope com segurança e acessá-lo por meio de um navegador da web localmente.
-   Se você não tiver nenhum ainda, [crie um cluster padrão](/docs/containers?topic=containers-clusters#clusters_ui). O Weave Scope pode ser intensivo de CPU, especialmente o app. Execute o Weave Scope com clusters padrão maiores, não clusters grátis.
-  Assegure-se de que você tenha a [função de **Gerenciador** do serviço {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para todos os namespaces.
-   [Efetue login em sua conta. Destine a região apropriada e, se aplicável, o grupo de recursos. Configure o contexto para seu cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).


Para usar o Weave Scope com um cluster:
2.  Implemente um dos arquivos de configuração de permissões do RBAC no cluster.

    Para ativar permissões de leitura/gravação:

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    Para ativar permissões somente leitura:

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    Saída:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Implemente o serviço do Weave Scope, que é particularmente acessível pelo endereço IP de cluster.

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    Saída:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  Execute um comando de encaminhamento de porta para abrir o serviço em seu computador. Na próxima vez que você acessar o Weave Scope, será possível executar esse comando de encaminhamento de porta sem concluir as etapas de configuração anteriores novamente.

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    Saída:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  Abra seu navegador para  ` http://localhost: 4040 `. Sem os componentes padrão implementados, você vê o diagrama a seguir. É possível escolher visualizar diagramas de topologia ou tabelas dos recursos do Kubernetes no cluster.

     <img src="images/weave_scope.png" alt="Topologia de exemplo do Weave Scope" style="width:357px;" />


[Saiba mais sobre os recursos do Weave Scope ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.weave.works/docs/scope/latest/features/).

<br />

