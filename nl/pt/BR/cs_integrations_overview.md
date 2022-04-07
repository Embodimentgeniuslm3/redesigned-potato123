---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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
{:preview: .preview}f


# Integrações suportadas do IBM Cloud e de terceiros
{: #supported_integrations}

É possível usar vários serviços externos e serviços de catálogo com um cluster do Kubernetes padrão no {{site.data.keyword.containerlong}}.
{:shortdesc}

## Integrações populares
{: #popular_services}

<table summary="A tabela mostra os serviços disponíveis que podem ser incluídos em seu cluster e que são populares entre os usuários do {{site.data.keyword.containerlong_notm}}. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um e uma descrição do serviço na coluna dois.">
<caption>Serviços populares</caption>
<thead>
<tr>
<th>Serviço</th>
<th>Categoria</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Logs de atividades do cluster</td>
<td>Monitore a atividade administrativa feita em seu cluster analisando logs por meio do Grafana. Para obter mais informações sobre o serviço, consulte a documentação do [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started). Para obter mais informações sobre os tipos de eventos que podem ser rastreados, consulte [Eventos do Activity Tracker](/docs/containers?topic=containers-at_events).</td>
</tr>
<tr>
<td>{{site.data.keyword.appid_full}}</td>
<td>Autenticação</td>
<td>Inclua um nível de segurança para seus apps com [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started), requerendo que os usuários se conectem. Para autenticar solicitações HTTP/HTTPS da web ou da API para seu app, é possível integrar o {{site.data.keyword.appid_short_notm}} ao serviço Ingress usando a [anotação do Ingress de autenticação do {{site.data.keyword.appid_short_notm}}](/docs/containers?topic=containers-ingress_annotation#appid-auth).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}} Block Storage</td>
<td>Armazenamento de bloco</td>
<td>[O {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) é um armazenamento iSCSI persistente e de alto desempenho que pode ser incluído em seus apps usando os volumes persistentes do Kubernetes (PVs). Use o armazenamento de bloco para implementar apps stateful em uma única zona ou como armazenamento de alto desempenho para pods únicos. Para obter mais informações sobre como fornecer armazenamento de bloco em seu cluster, consulte [Armazenando dados no {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/containers?topic=containers-block_storage#block_storage)</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>Certificados TLS</td>
<td>É possível usar o <a href="/docs/services/certificate-manager?topic=certificate-manager-getting-started#getting-started" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> para armazenar e gerenciar certificados SSL para seus apps. Para obter mais informações, consulte <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Usar o {{site.data.keyword.cloudcerts_long_notm}} com o {{site.data.keyword.containerlong_notm}} para implementar certificados TLS de domínio customizado <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.registrylong}}</td>
<td>Imagens do contêiner</td>
<td>Configure o seu próprio repositório de imagem assegurado do Docker no qual é possível armazenar e compartilhar imagens com segurança entre usuários do cluster. Para obter mais informações, consulte a <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">documentação do {{site.data.keyword.registrylong}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.</td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automação de construção</td>
<td>Automatize as construções de app e as implementações do contêiner para clusters do Kubernetes usando uma cadeia de ferramentas. Para obter mais informações sobre a configuração, consulte o blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.datashield_full}}  (Beta)</td>
<td>Criptografia de memória</td>
<td>É possível usar o <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> para criptografar sua memória de dados. O {{site.data.keyword.datashield_short}} está integrado às tecnologias Intel® Software Guard Extensions (SGX) e Fortanix® para que seu código de carga de trabalho de contêiner do {{site.data.keyword.Bluemix_notm}} e os dados sejam protegidos em uso. O código do app e os dados são executados em enclaves reforçados pela CPU, que são áreas confiáveis de memória no nó do trabalhador que protegem os aspectos críticos do app, o que ajuda a manter o código e os dados confidenciais e não modificados.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix}}  Armazenamento de arquivo</td>
<td>Armazenamento de arquivo</td>
<td>[{{site.data.keyword.Bluemix_notm}} File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#getting-started) é um armazenamento de arquivo baseado em NFS persistente, rápido e flexível, que você pode incluir em seus apps usando volumes persistentes do Kubernetes. É possível escolher entre camadas de armazenamento predefinidas com tamanhos de GB e IOPS que atendam aos requisitos de suas cargas de trabalho. Para obter mais informações sobre como fornecer o armazenamento de arquivo em seu cluster, consulte [Armazenando dados no {{site.data.keyword.Bluemix_notm}} File Storage](/docs/containers?topic=containers-file_storage#file_storage).</td>
</tr>
<tr>
<td>{{site.data.keyword.keymanagementservicefull}}</td>
<td>Criptografia de dados</td>
<td>Criptografe os segredos do Kubernetes que estão em seu cluster ativando o {{site.data.keyword.keymanagementserviceshort}}. A criptografia de seus segredos do Kubernetes evita que usuários não autorizados acessem informações confidenciais de cluster.<br>Para configurar, veja <a href="/docs/containers?topic=containers-encryption#keyprotect">Criptografando segredos do Kubernetes usando o {{site.data.keyword.keymanagementserviceshort}}</a>.<br>Para obter mais informações, consulte a documentação do <a href="/docs/services/key-protect?topic=key-protect-getting-started-tutorial" target="_blank">{{site.data.keyword.keymanagementserviceshort}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.</td>
</tr>
<tr>
<td>{{site.data.keyword.la_full}}</td>
<td>Logs do cluster e do app</td>
<td>Inclua recursos de gerenciamento de log no cluster implementando LogDNA como um serviço de terceiro para seus nós do trabalhador para gerenciar logs dos contêineres de pod. Para obter mais informações, consulte [Gerenciando logs de cluster do Kubernetes com o {{site.data.keyword.loganalysisfull_notm}} com LogDNA](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-kube#kube).</td>
</tr>
<tr>
<td>{{site.data.keyword.mon_full}}</td>
<td>Métricas do cluster e do app</td>
<td>Obtenha visibilidade operacional para o desempenho e o funcionamento de seus apps implementando o Sysdig como um serviço de terceiro para os nós do trabalhador para encaminhar métricas para o {{site.data.keyword.monitoringlong}}. Para obter mais informações, consulte [Analisando métricas para um app implementado em um cluster do Kubernetes](/docs/services/Monitoring-with-Sysdig/tutorials?topic=Sysdig-kubernetes_cluster#kubernetes_cluster). </td>
</tr>
<tr>
<td>{{site.data.keyword.cos_full}}</td>
<td>Armazenamento de objetos</td>
<td>Os dados armazenados com o {{site.data.keyword.cos_short}} são criptografados e dispersos em múltiplas localizações geográficas e acessados por HTTP usando uma API de REST. É possível usar [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) para configurar o serviço para fazer backups únicos ou planejados para dados em seus clusters. Para obter mais informações sobre o serviço, consulte a <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">documentação do {{site.data.keyword.cos_short}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.</td>
</tr>
<tr>
<td>Istio no  {{site.data.keyword.containerlong_notm}}</td>
<td>Gerenciamento de microsserviço</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> é um serviço de software livre que fornece aos desenvolvedores uma maneira de conectar, proteger, gerenciar e monitorar uma rede de microsserviços, também conhecida como uma malha de serviços, em plataformas de orquestração de nuvem. Istio on {{site.data.keyword.containerlong}} fornece uma instalação de uma etapa do Istio no seu cluster por meio de um complemento gerenciado. Com um clique, é possível obter todos os componentes principais do Istio, rastreio adicional, monitoramento e visualização e o aplicativo de amostra BookInfo funcionando. Para iniciar, consulte [Usando o complemento Istio gerenciado (beta)](/docs/containers?topic=containers-istio).</td>
</tr>
<tr>
<td>Knative</td>
<td>Apps serverless</td>
<td>O [Knative ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/knative/docs) é uma plataforma de software livre desenvolvida pelas empresas IBM, Google, Pivotal, Red Hat, Cisco, entre outras, com o objetivo de ampliar os recursos do Kubernetes para ajudá-lo a criar apps modernos, centralizados em contêineres de origem e sem servidor sobre o cluster do Kubernetes. A plataforma usa uma abordagem consistente entre linguagens de programação e estruturas para abstrair a carga operacional de construção, implementação e gerenciamento de cargas de trabalho no Kubernetes para que os desenvolvedores possam se concentrar no que mais importa para eles: o código-fonte. Para obter mais informações, consulte [Implementando apps sem servidor com o Knative](/docs/containers?topic=containers-serverless-apps-knative). </td>
</tr>
<tr>
<td>Portworx</td>
<td>Armazenamento para apps stateful</td>
<td>[Portworx ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://portworx.com/products/introduction/) é uma solução de armazenamento definido por software altamente disponível que pode ser usada para gerenciar o armazenamento persistente para seus bancos de dados conteinerizados e outros apps stateful ou para compartilhar dados entre os pods em múltiplas zonas. É possível instalar o Portworx com um gráfico do Helm e fornecer o armazenamento para seus apps usando volumes persistentes do Kubernetes. Para obter mais informações sobre como configurar o Portworx em seu cluster, consulte [Armazenando Dados em Armazenamento Definido por Software (SDS) com Portworx](/docs/containers?topic=containers-portworx#portworx).</td>
</tr>
<tr>
<td>Razee</td>
<td>Automação de implementação</td>
<td>O [Razee ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://razee.io/) é um projeto de software livre que automatiza e gerencia a implementação de recursos do Kubernetes em clusters, ambientes e provedores em nuvem, além de ajudar a visualizar informações de implementação para seus recursos para que seja possível monitorar o processo de lançamento e localizar problemas de implementação mais rapidamente. Para obter mais informações sobre o Razee e como configurá-lo em seu cluster para automatizar seu processo de implementação, consulte a [documentação do Razee ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/razee-io/Razee).</td>
</tr>
</tbody>
</table>

<br />


## Serviços de DevOps
{: #devops_services}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos extras do DevOps. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Serviços de DevOps</caption>
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
<td> <a href="https://helm.sh" target="_blank">O Helm <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> é um gerenciador de pacote do Kubernetes. É possível criar novos gráficos Helm ou usar gráficos Helm preexistente para definir, instalar e fazer upgrade de aplicativos complexos do Kubernetes que são executados em clusters do {{site.data.keyword.containerlong_notm}}. <p>Para obter mais informações, veja [Configurando o Helm no {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automatize as construções de app e as implementações do contêiner para clusters do Kubernetes usando uma cadeia de ferramentas. Para obter mais informações sobre a configuração, consulte o blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>. </td>
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
<td>O [Knative ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/knative/docs) é uma plataforma de software livre desenvolvida pelas empresas IBM, Google, Pivotal, Red Hat, Cisco, entre outras, com o objetivo de ampliar os recursos do Kubernetes para ajudá-lo a criar apps modernos, centralizados em contêineres de origem e sem servidor sobre o cluster do Kubernetes. A plataforma usa uma abordagem consistente entre linguagens de programação e estruturas para abstrair a carga operacional de construção, implementação e gerenciamento de cargas de trabalho no Kubernetes para que os desenvolvedores possam se concentrar no que mais importa para eles: o código-fonte. Para obter mais informações, consulte [Implementando apps sem servidor com o Knative](/docs/containers?topic=containers-serverless-apps-knative). </td>
</tr>
<tr>
<td>Razee</td>
<td>O [Razee ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://razee.io/) é um projeto de software livre que automatiza e gerencia a implementação de recursos do Kubernetes em clusters, ambientes e provedores em nuvem, além de ajudar a visualizar informações de implementação para seus recursos para que seja possível monitorar o processo de lançamento e localizar problemas de implementação mais rapidamente. Para obter mais informações sobre o Razee e como configurá-lo em seu cluster para automatizar seu processo de implementação, consulte a [documentação do Razee ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/razee-io/Razee).</td>
</tr>
</tbody>
</table>

<br />


## Serviços de nuvem híbrida
{: #hybrid_cloud_services}

<table summary="A tabela mostra os serviços disponíveis que podem ser usados para conectar seu cluster a data centers no local. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um e uma descrição do serviço na coluna dois.">
<caption>Serviços de nuvem híbrida</caption>
<thead>
<tr>
<th>Serviço</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.BluDirectLink}}</td>
    <td>O [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link) permite criar uma conexão direta e privada entre seus ambientes de rede remota e o {{site.data.keyword.containerlong_notm}} sem rotear pela Internet pública. As ofertas do {{site.data.keyword.Bluemix_notm}} Direct Link são úteis quando se deve implementar cargas de trabalho híbridas, cargas de trabalho entre provedores, transferências de dados grandes ou frequentes ou cargas de trabalho privadas. Para escolher uma oferta {{site.data.keyword.Bluemix_notm}} Direct Link e configurar uma conexão do {{site.data.keyword.Bluemix_notm}} Direct Link, consulte [Introdução ao {{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-) na documentação do {{site.data.keyword.Bluemix_notm}} Direct Link.</td>
  </tr>
<tr>
  <td>Serviço VPN IPSec do strongSwan</td>
  <td>Configure um [serviço VPN IPSec do strongSwan ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.strongswan.org/about.html) que conecte de forma segura seu cluster Kubernetes a uma rede no local. O serviço de VPN do IPSec do strongSwan fornece um canal de comunicação seguro de ponta a ponta sobre a Internet que é baseado no conjunto de protocolos padrão de mercado da Internet Protocol Security (IPSec). Para configurar uma conexão segura entre seu cluster e uma rede no local, [configure e implemente o serviço VPN IPSec do strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) diretamente em um pod no cluster.</td>
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
<td>Monitore a atividade administrativa feita em seu cluster analisando logs por meio do Grafana. Para obter mais informações sobre o serviço, consulte a documentação do [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started). Para obter mais informações sobre os tipos de eventos que podem ser rastreados, consulte [Eventos do Activity Tracker](/docs/containers?topic=containers-at_events).</td>
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
<td>Prometheus é uma ferramenta de monitoramento, criação de log e alerta de software livre que foi projetada para o Kubernetes. O Prometheus recupera informações detalhadas sobre o cluster, os nós do trabalhador e o funcionamento de implementação com base nas informações de criação de log do Kubernetes. A atividade de CPU, memória, E/S e rede é coletada para cada contêiner que é executado em um cluster. É possível usar os dados coletados em consultas ou alertas customizados para monitorar o desempenho e as cargas de trabalho em seu cluster.

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
<td>O [Weave Scope ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.weave.works/oss/scope/) fornece um diagrama visual de seus recursos dentro de um cluster Kubernetes, incluindo serviços, pods, contêineres, processos, nós e muito mais. O Weave Scope fornece métricas interativas para CPU e memória e também fornece ferramentas para uso de tail e executável em um contêiner.</li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## Serviços de Segurança
{: #security_services}

Deseja uma visualização abrangente de como integrar os serviços de segurança do {{site.data.keyword.Bluemix_notm}} ao seu cluster? Verifique o [tutorial Aplicar a segurança de ponta a ponta a um aplicativo em nuvem](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security).
{: shortdesc}

<table summary="A tabela mostra os serviços disponíveis que você pode incluir em seu cluster para incluir recursos de segurança extra. As linhas devem ser lidas da esquerda para a direita, com o nome do serviço na coluna um, e uma descrição do serviço na coluna dois. ">
<caption>Serviços de Segurança</caption>
<thead>
<tr>
<th>Serviço</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
  <tr>
    <td>{{site.data.keyword.appid_full}}</td>
    <td>Inclua um nível de segurança para seus apps com [{{site.data.keyword.appid_short}}](/docs/services/appid?topic=appid-getting-started), requerendo que os usuários se conectem. Para autenticar solicitações HTTP/HTTPS da web ou da API para seu app, é possível integrar o {{site.data.keyword.appid_short_notm}} ao serviço Ingress usando a [anotação do Ingress de autenticação do {{site.data.keyword.appid_short_notm}}](/docs/containers?topic=containers-ingress_annotation#appid-auth).</td>
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
  <td>Configure o seu próprio repositório de imagem assegurado do Docker no qual é possível armazenar e compartilhar imagens com segurança entre usuários do cluster. Para obter mais informações, consulte a <a href="/docs/services/Registry?topic=registry-getting-started" target="_blank">documentação do {{site.data.keyword.registrylong}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.</td>
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
  <td>[O {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started) é um armazenamento iSCSI persistente e de alto desempenho que pode ser incluído em seus apps usando os volumes persistentes do Kubernetes (PVs). Use o armazenamento de bloco para implementar apps stateful em uma única zona ou como armazenamento de alto desempenho para pods únicos. Para obter mais informações sobre como fornecer armazenamento de bloco em seu cluster, consulte [Armazenando dados no {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/containers?topic=containers-block_storage#block_storage)</td>
  </tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>Os dados armazenados com o {{site.data.keyword.cos_short}} são criptografados e dispersos em múltiplas localizações geográficas e acessados por HTTP usando uma API de REST. É possível usar [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter) para configurar o serviço para fazer backups únicos ou planejados para dados em seus clusters. Para obter mais informações sobre o serviço, consulte a <a href="/docs/services/cloud-object-storage?topic=cloud-object-storage-about" target="_blank">documentação do {{site.data.keyword.cos_short}} <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.</td>
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
  <td>É possível escolher entre vários serviços de banco de dados do {{site.data.keyword.Bluemix_notm}}, tais como {{site.data.keyword.composeForMongoDB_full}} ou {{site.data.keyword.cloudantfull}}, para implementar soluções de banco de dados altamente disponíveis e escaláveis em seu cluster. Para obter uma lista de bancos de dados em nuvem disponíveis, consulte o [Catálogo do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/catalog?category=databases).  </td>
  </tr>
  </tbody>
  </table>
