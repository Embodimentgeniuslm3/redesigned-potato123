---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

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


# Tutorial Introdução
{: #getting-started}

Tenha sucesso desde o início com o {{site.data.keyword.containerlong}} implementando apps altamente disponíveis em contêineres do Docker que são executados em clusters do Kubernetes.
{:shortdesc}

Os contêineres são uma maneira padrão de empacotar apps e todas as suas dependências para que seja possível mover perfeitamente os apps entre ambientes. Ao contrário de máquinas virtuais, os contêineres não empacotam o sistema operacional. Somente o código de app, o tempo de execução, as ferramentas de sistema, as bibliotecas e as configurações são empacotados dentro de contêineres. Os contêineres são mais leves, móveis e eficientes do que máquinas virtuais.

## Introdução aos clusters
{: #clusters_gs}

Então você deseja implementar um app em um contêiner? Espere! Inicie criando um cluster do Kubernetes primeiro. Kubernetes é uma ferramenta de orquestração para contêineres. Com o Kubernetes, os desenvolvedores podem implementar apps altamente disponíveis em um flash usando o poder e a flexibilidade de clusters.
{:shortdesc}

E o que é um cluster? Um cluster é um conjunto de recursos, nós de trabalhador, redes e dispositivos de armazenamento que mantêm os apps altamente disponíveis. Após ter o seu cluster, é possível implementar seus apps em contêineres.

Antes de iniciar, obtenha o tipo de [Conta do {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/registration) que é o correto para você:
* **Faturável (Pagamento por uso ou Assinatura)**: é possível criar um cluster grátis. Também é possível provisionar os recursos de infraestrutura do IBM Cloud (SoftLayer) para criar e usar em clusters padrão.
* **Lite**: não é possível criar um cluster grátis ou padrão. [Faça upgrade de sua conta](/docs/account?topic=account-accountfaqs#changeacct) para uma conta faturável.

Para criar um cluster grátis:

1.  No [**Catálogo** do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/catalog?category=containers), selecione **{{site.data.keyword.containershort_notm}}** e clique em **Criar**. Uma página de configuração do cluster é aberta. Por padrão, o **grátis cluster** é selecionado.

2.  Forneça seu cluster um nome exclusivo.

3.  Clique em **Criar Cluster**. É criado um conjunto de trabalhadores que contém 1 nó do trabalhador. O nó do trabalhador pode demorar alguns minutos para provisionar, mas é possível ver o progresso na guia **Nós do trabalhador**. Quando o status atinge `Ready`, é possível começar a trabalhar com seu cluster!

<br>

Bom Trabalho! Você criou seu primeiro cluster do Kubernetes. Aqui estão alguns detalhes sobre seu cluster grátis:

*   **Tipo de máquina**: o cluster grátis tem um nó do trabalhador virtual que é agrupado em um conjunto de trabalhadores, com 2 CPUs, 4 GB de memória e um único disco SAN de 100 GB disponível para seus apps usarem. Ao criar um cluster padrão, é possível escolher entre máquinas físicas (bare metal) ou virtuais, juntamente com vários tamanhos de máquina.
*   **Principal gerenciado**: o nó do trabalhador é monitorado e gerenciado centralmente por um mestre do Kubernetes dedicado e altamente disponível pertencente à {{site.data.keyword.IBM_notm}} que controla e monitora todos os recursos do Kubernetes no cluster. É possível concentrar-se em seu nó do trabalhador e nos apps implementados nele sem se preocupar também com o gerenciamento desse mestre.
*   **Recursos de infraestrutura**: os recursos que são necessários para executar o cluster, como VLANs e endereços IP, são gerenciados em uma conta de infraestrutura do IBM Cloud (SoftLayer) pertencente ao {{site.data.keyword.IBM_notm}}. Ao criar um cluster padrão, você gerencia esses recursos em sua própria conta de infraestrutura do IBM Cloud (SoftLayer). É possível aprender mais sobre esses recursos e as [permissões necessárias](/docs/containers?topic=containers-users#infra_access) ao criar um cluster padrão.
*   **Outras opções**: os clusters grátis são implementados dentro da região que você seleciona, mas não é possível escolher qual zona. Para controlar a zona, a rede e o armazenamento persistente, crie um cluster padrão. [Saiba mais sobre os benefícios de clusters grátis e padrão](/docs/containers?topic=containers-cs_ov#cluster_types).

<br>

**O que vem a seguir?**</br>
Experimente algumas coisas com seu cluster grátis antes de expirar.

* Acesse o [primeiro tutorial do {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial) para criar um cluster do Kubernetes, instalando a CLI ou usando o Terminal do Kubernetes, criando um registro privado, configurando o seu ambiente de cluster e incluindo um serviço em seu cluster.
* Mantenha seu ritmo com o [segundo tutorial do {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial) sobre a implementação de apps no cluster.
* [Crie um cluster padrão](/docs/containers?topic=containers-clusters#clusters_ui) com múltiplos nós para disponibilidade mais alta.


