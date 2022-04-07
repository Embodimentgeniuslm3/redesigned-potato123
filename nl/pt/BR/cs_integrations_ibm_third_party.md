---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-07"

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


# Serviços do IBM Cloud e integrações de terceiros
{: #ibm-3rd-party-integrations}

É possível usar os serviços de plataforma e de infraestrutura do {{site.data.keyword.Bluemix_notm}} e outras integrações de terceiros para incluir recursos extras em seu cluster.
{: shortdesc}

## Serviços do IBM Cloud
{: #ibm-cloud-services}

Revise as informações a seguir para ver como os serviços de plataforma e de infraestrutura do {{site.data.keyword.Bluemix_notm}} são integrados ao {{site.data.keyword.containerlong_notm}} e como é possível usá-los em seu cluster.
{: shortdesc}

### Serviços da plataforma IBM Cloud
{: #platform-services}

Todos os serviços da plataforma {{site.data.keyword.Bluemix_notm}} que suportam chaves de serviço podem ser integrados usando a [ligação de serviços](/docs/containers?topic=containers-service-binding) do {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

A ligação de serviços é uma maneira rápida de criar credenciais de serviço para um serviço do {{site.data.keyword.Bluemix_notm}} e armazená-las em um segredo do Kubernetes em seu cluster. O segredo do Kubernetes é criptografado automaticamente em etcd para proteger seus dados. Seus apps podem usar as credenciais no segredo para acessar sua instância de serviço do {{site.data.keyword.Bluemix_notm}}.

Os serviços que não suportam chaves de serviço geralmente fornecem uma API que pode ser usada diretamente em seu app.

Para localizar uma visão geral dos serviços populares do {{site.data.keyword.Bluemix_notm}}, consulte [Integrações populares](/docs/containers?topic=containers-supported_integrations#popular_services).

### Serviços de infraestrutura do IBM Cloud
{: #infrastructure-services}

Como o {{site.data.keyword.containerlong_notm}} permite criar um cluster Kubernetes que é baseado na infraestrutura do {{site.data.keyword.Bluemix_notm}}, alguns serviços de infraestrutura, como Virtual Servers, Bare Metal Servers ou VLANs, são totalmente integrados ao {{site.data.keyword.containerlong_notm}}. Você cria e trabalha com essas instâncias de serviços usando a API, a CLI ou o console do {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

As soluções de armazenamento persistente suportadas, como {{site.data.keyword.Bluemix_notm}} File Storage, {{site.data.keyword.Bluemix_notm}} Block Storage ou {{site.data.keyword.cos_full}} são integradas como drivers flex do Kubernetes e podem ser configuradas usando [gráficos do Helm](/docs/containers?topic=containers-helm). O gráfico do Helm configura automaticamente as classes de armazenamento do Kubernetes, o provedor de armazenamento e o driver de armazenamento em seu cluster. É possível usar as classes de armazenamento para provisionar o armazenamento persistente usando solicitações de volume persistente (PVCs).

Para proteger sua rede de cluster ou se conectar a um data center no local, é possível configurar uma das opções a seguir:
- [Serviço VPN IPSec do strongSwan](/docs/containers?topic=containers-vpn#vpn-setup)
- [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link)
- [Virtual Router Appliance (VRA)](/docs/containers?topic=containers-vpn#vyatta)
- [Fortigate Security Appliance (FSA)](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations)

## Comunidade do Kubernetes e integrações de software livre
{: #kube-community-tools}

Como você possui os clusters padrão criados no {{site.data.keyword.containerlong_notm}}, é possível escolher instalar soluções de terceiros para incluir recursos extras em seu cluster.
{: shortdesc}

Algumas tecnologias de software livre, como o Knative, o Istio, o LogDNA, o Sysdig ou o Portworx, são testadas pela IBM e fornecidas como complementos gerenciados, gráficos do Helm ou serviços do {{site.data.keyword.Bluemix_notm}} que são operados pelo provedor de serviços em parceria com a IBM. Essas ferramentas de software livre são totalmente integradas ao sistema de faturamento e suporte do {{site.data.keyword.Bluemix_notm}}.

É possível instalar outras ferramentas de software livre em seu cluster, mas essas ferramentas podem não ser gerenciadas, suportadas ou verificadas quanto ao funcionamento no {{site.data.keyword.containerlong_notm}}.

### Integrações operadas em parceria
{: #open-source-partners}

Para obter mais informações sobre os parceiros do {{site.data.keyword.containerlong_notm}} e o benefício de cada solução que eles fornecem, consulte [Parceiros do {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-service-partners).

### Complementos gerenciados
{: #cluster-add-ons}
O {{site.data.keyword.containerlong_notm}} integra integrações de software livre populares, como [Knative](/docs/containers?topic=containers-serverless-apps-knative) ou [Istio](/docs/containers?topic=containers-istio) usando [complementos gerenciados](/docs/containers?topic=containers-managed-addons). Os complementos gerenciados são uma maneira fácil de instalar uma ferramenta de software livre em seu cluster que é testada pela IBM e aprovada para ser usada no {{site.data.keyword.containerlong_notm}}.

Os complementos gerenciados são totalmente integrados na organização de suporte do {{site.data.keyword.Bluemix_notm}}. Se você tiver uma pergunta ou um problema com o uso dos complementos gerenciados, será possível usar um dos canais de suporte do {{site.data.keyword.containerlong_notm}}. Para obter mais informações, consulte [Obtendo ajuda e suporte](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

Se a ferramenta que você incluir em seu cluster incorrer em custos, esses custos serão integrados automaticamente e listados como parte de seu faturamento mensal do {{site.data.keyword.Bluemix_notm}}. O ciclo de faturamento é determinado pelo {{site.data.keyword.Bluemix_notm}}, dependendo de quando você ativou o complemento em seu cluster.

### Outras integrações de terceiros
{: #kube-community-helm}

É possível instalar qualquer ferramenta de software livre de terceiros que se integra ao Kubernetes. Por exemplo, a comunidade do Kubernetes designa determinados gráficos do Helm `stable` ou `incubator`. Observe que esses gráficos ou ferramentas não foram verificados quanto ao funcionamento no {{site.data.keyword.containerlong_notm}}. Se a ferramenta requer uma licença, deve-se comprar uma licença antes de usar a ferramenta. Para obter uma visão geral dos gráficos do Helm disponíveis na comunidade do Kubernetes, consulte os repositórios `kubernetes` e `kubernetes-incubator` no catálogo de [Gráficos do Helm ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/kubernetes/solutions/helm-charts).
{: shortdesc}

Os custos incorridos usando uma integração de software livre de terceiros não são incluídos em sua fatura mensal do {{site.data.keyword.Bluemix_notm}}.

A instalação de integrações de software livre de terceiros ou de gráficos do Helm da comunidade do Kubernetes pode mudar a configuração de cluster padrão e pode trazer seu cluster para um estado não suportado. Se você encontrar um problema com o uso de qualquer uma dessas ferramentas, consulte a comunidade do Kubernetes ou o provedor de serviços diretamente.
{: important}
