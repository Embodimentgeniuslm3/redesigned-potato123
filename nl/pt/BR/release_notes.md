---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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

# Notas sobre a liberação
{: #iks-release}

Use as notas sobre a liberação para aprender sobre as mudanças mais recentes para a documentação do {{site.data.keyword.containerlong}} que são agrupadas por mês.
{:shortdesc}

## Junho de 2019
{: #jun19}

<table summary="A tabela mostra notas sobre a liberação. As linhas devem ser lidas da esquerda para a direita, com a data na coluna um, o título do recurso na coluna dois e uma descrição na coluna três.">
<caption>Atualizações da documentação do {{site.data.keyword.containerlong_notm}} em junho de 2019</caption>
<thead>
<th>Data</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
  <td>7 de junho de 2019</td>
  <td><ul>
  <li><strong>Acesso ao principal do Kubernetes por meio do terminal em serviço privado</strong>: incluídas [etapas](/docs/containers?topic=containers-clusters#access_on_prem) para expor o terminal em serviço privado por meio de um balanceador de carga privado. Depois de concluir essas etapas, seus usuários de cluster autorizados poderão acessar o principal do Kubernetes por meio de uma conexão VPN ou {{site.data.keyword.Bluemix_notm}} Direct Link.</li>
  <li><strong>{{site.data.keyword.BluDirectLink}}</strong>: incluído o {{site.data.keyword.Bluemix_notm}} Direct Link nas páginas [Conectividade VPN](/docs/containers?topic=containers-vpn) e [nuvem híbrida](/docs/containers?topic=containers-hybrid_iks_icp) como uma maneira de criar uma conexão privada direta entre seus ambientes de rede remota e o {{site.data.keyword.containerlong_notm}} sem rotear pela Internet pública.</li>
  <li><strong>Log de mudanças do ALB do Ingress</strong>: atualizado a imagem [`ingress-auth` do ALB para construção 330](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>Beta do OpenShift</strong>: [incluída uma lição](/docs/containers?topic=containers-openshift_tutorial#openshift_logdna_sysdig) sobre como modificar implementações de app para restrições de contexto de segurança privilegiada para os complementos {{site.data.keyword.la_full_notm}} e {{site.data.keyword.mon_full_notm}}.</li>
  </ul></td>
</tr>
<tr>
  <td>6 de junho de 2019</td>
  <td><ul>
  <li><strong>Log de mudanças do Fluentd</strong>: incluído um [log de mudanças da versão do Fluentd](/docs/containers?topic=containers-cluster-add-ons-changelog#fluentd_changelog).</li>
  <li><strong>Log de mudanças do ALB do Ingress</strong>: atualizada a imagem [`nginx-ingress` do ALB para construção 470](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  </ul></td>
</tr>
<tr>
  <td>5 de junho de 2019</td>
  <td><ul>
  <li><strong>Referência da CLI</strong>: atualizada a [página de referência da CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) para refletir múltiplas mudanças para a [liberação da versão 0.3.34](/docs/containers?topic=containers-cs_cli_changelog) do plug-in da CLI do {{site.data.keyword.containerlong_notm}}.</li>
  <li><strong>Novo! Clusters do Red Hat OpenShift on IBM Cloud clusters (beta)</strong>: com o beta do Red Hat OpenShift no IBM Cloud, é possível criar clusters do {{site.data.keyword.containerlong_notm}} com nós do trabalhador que vêm instalados com o software da plataforma de orquestração de contêiner do OpenShift. Você obtém todas as vantagens do {{site.data.keyword.containerlong_notm}} gerenciado para seu ambiente de infraestrutura do cluster, juntamente com o [conjunto de ferramentas e o catálogo do OpenShift ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.openshift.com/container-platform/3.11/welcome/index.html) que são executados no Red Hat Enterprise Linux para suas implementações do app. Para obter a introdução, consulte [Tutorial: criando um cluster do Red Hat OpenShift on IBM Cloud (beta)](/docs/containers?topic=containers-openshift_tutorial).</li>
  </ul></td>
</tr>
<tr>
  <td>4 de junho de 2019</td>
  <td><ul>
  <li><strong>Logs de mudanças de versão</strong>: atualizados os logs de mudanças para as liberações de correção [1.14.2_1521](/docs/containers?topic=containers-changelog#1142_1521), [1.13.6_1524](/docs/containers?topic=containers-changelog#1136_1524), [1.12.9_1555](/docs/containers?topic=containers-changelog#1129_1555) e [1.11.10_1561](/docs/containers?topic=containers-changelog#11110_1561).</li></ul>
  </td>
</tr>
<tr>
  <td>3 de junho de 2019</td>
  <td><ul>
  <li><strong>Trazendo seu próprio controlador do Ingress</strong>: atualizadas as [etapas](/docs/containers?topic=containers-ingress#user_managed) para refletir as mudanças no controlador de comunidade padrão e para requerer uma verificação de funcionamento para endereços IP do controlador em clusters de multizona.</li>
  <li><strong>{{site.data.keyword.cos_full_notm}}</strong>: atualizadas as [etapas](/docs/containers?topic=containers-object_storage#install_cos) para instalar o plug-in do {{site.data.keyword.cos_full_notm}} com ou sem o servidor do Helm, o Tiller.</li>
  <li><strong>Log de mudanças do ALB do Ingress</strong>: atualizada a imagem [`nginx-ingress` do ALB para construção 467](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>Kustomize</strong>: incluído um exemplo de [reutilização de arquivos de configuração do Kubernetes em múltiplos ambientes com o Kustomize](/docs/containers?topic=containers-app#kustomize).</li>
  <li><strong>Razee</strong>: incluído o [Razee ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/razee-io/Razee) nas integrações suportadas para visualizar informações de implementação no cluster e para automatizar a implementação de recursos do Kubernetes. </li></ul>
  </td>
</tr>
</tbody></table>

## Maio de 2019
{: #may19}

<table summary="A tabela mostra notas sobre a liberação. As linhas devem ser lidas da esquerda para a direita, com a data na coluna um, o título do recurso na coluna dois e uma descrição na coluna três.">
<caption>Atualizações da documentação do {{site.data.keyword.containerlong_notm}} em maio de 2019</caption>
<thead>
<th>Data</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
  <td>30 de maio de 2019</td>
  <td><ul>
  <li><strong>Referência da CLI</strong>: atualizada a [página de referência da CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) para refletir múltiplas mudanças para a [liberação da versão 0.3.33](/docs/containers?topic=containers-cs_cli_changelog) do plug-in da CLI do {{site.data.keyword.containerlong_notm}}.</li>
  <li><strong>Armazenamento de resolução de problemas</strong>: <ul>
  <li>Incluído um tópico de resolução de problemas de armazenamento de arquivo e de bloco para [estados pendentes da PVC](/docs/containers?topic=containers-cs_troubleshoot_storage#file_pvc_pending).</li>
  <li>Incluído um tópico de resolução de problemas de armazenamento de bloco para quando [um app não pode acessar ou gravar na PVC](/docs/containers?topic=containers-cs_troubleshoot_storage#block_app_failures).</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>28 de maio de 2019</td>
  <td><ul>
  <li><strong>Log de mudanças de complementos do cluster</strong>: atualizada a imagem [`nginx-ingress` do ALB para construção 462](/docs/containers?topic=containers-cluster-add-ons-changelog).</li>
  <li><strong>Registro de resolução de problemas</strong>: incluído um [tópico de resolução de problemas](/docs/containers?topic=containers-cs_troubleshoot_clusters#ts_image_pull_create) para quando seu cluster não pode fazer pull de imagens do {{site.data.keyword.registryfull}} devido a um erro durante a criação do cluster.
  </li>
  <li><strong>Armazenamento de resolução de problemas</strong>: <ul>
  <li>Incluído um tópico para [depurar falhas de armazenamento persistente](/docs/containers?topic=containers-cs_troubleshoot_storage#debug_storage).</li>
  <li>Incluído um tópico de resolução de problemas para [falhas de criação da PVC devido a permissões ausentes](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions).</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>23 de maio de 2019</td>
  <td><ul>
  <li><strong>Referência da CLI</strong>: atualizada a [página de referência da CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) para refletir múltiplas mudanças para a [liberação da versão 0.3.28](/docs/containers?topic=containers-cs_cli_changelog) do plug-in da CLI do {{site.data.keyword.containerlong_notm}}.</li>
  <li><strong>Log de mudanças de complementos do cluster</strong>: atualizada a imagem [`nginx-ingress` do ALB para construção 457](/docs/containers?topic=containers-cluster-add-ons-changelog).</li>
  <li><strong>Estados do cluster e do trabalhador</strong>: atualizada a [página Criação de log e monitoramento](/docs/containers?topic=containers-health#states) para incluir tabelas de referência sobre estados do cluster e do nó do trabalhador.</li>
  <li><strong>Planejamento e criação de cluster</strong>: agora é possível localizar informações sobre planejamento, criação e remoção de cluster e planejamento de rede nas páginas a seguir:
    <ul><li>[Planejando a configuração de rede do cluster](/docs/containers?topic=containers-plan_clusters)</li>
    <li>[Planejando seu cluster para alta disponibilidade](/docs/containers?topic=containers-ha_clusters)</li>
    <li>[Planejando a configuração do nó do trabalhador](/docs/containers?topic=containers-planning_worker_nodes)</li>
    <li>[Criando clusters](/docs/containers?topic=containers-clusters)</li>
    <li>[Incluindo nós do trabalhador e zonas em clusters](/docs/containers?topic=containers-add_workers)</li>
    <li>[Removendo clusters](/docs/containers?topic=containers-remove)</li>
    <li>[Mudando Terminais em serviço ou conexões VLAN](/docs/containers?topic=containers-cs_network_cluster)</li></ul>
  </li>
  <li><strong>Atualizações de versão do cluster</strong>: atualizada a [política de versões não suportadas](/docs/containers?topic=containers-cs_versions) para mencionar que não será possível atualizar clusters se a versão do principal for três ou mais versões atrás da versão suportada mais antiga. É possível visualizar se o cluster é **não suportado** revisando o campo **Estado** ao listar clusters.</li>
  <li><strong>Istio</strong>: atualizada a [página Istio](/docs/containers?topic=containers-istio) para remover a limitação de que o Istio não funciona em clusters que estão conectados somente a uma VLAN privada. Incluída uma etapa no [tópico Atualizando complementos gerenciados](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons) para recriar os gateways do Istio que usam seções do TLS após a atualização do complemento gerenciado pelo Istio ser concluída.</li>
  <li><strong>Tópicos populares</strong>: substituídos os tópicos populares por essa página de notas sobre a liberação para obter novos recursos e atualizações que são específicos para o {{site.data.keyword.containershort_notm}}. Para obter as informações mais recentes sobre produtos do {{site.data.keyword.Bluemix_notm}}, consulte os [Anúncios](https://www.ibm.com/cloud/blog/announcements).</li>
  </ul></td>
</tr>
<tr>
  <td>20 de maio de 2019</td>
  <td><ul>
  <li><strong>Logs de mudanças de versão</strong>: incluídos os [logs de mudanças do fix pack do nó do trabalhador](/docs/containers?topic=containers-changelog).</li>
  </ul></td>
</tr>
<tr>
  <td>16 de maio de 2019</td>
  <td><ul>
  <li><strong>Referência da CLI</strong>: atualizada a [página de referência da CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) para incluir os terminais COS para comandos `logging-collect` e para esclarecer que `apiserver-refresh` reinicia os componentes do principal do Kubernetes.</li>
  <li><strong>Não suportado: Kubernetes versão 1.10</strong>: o [Kubernetes versão 1.10](/docs/containers?topic=containers-cs_versions#cs_v114) agora não é suportado.</li>
  </ul></td>
</tr>
<tr>
  <td>15 de maio de 2019</td>
  <td><ul>
  <li><strong>Versão do Kubernetes padrão</strong>: a versão padrão do Kubernetes é agora 1.13.6.</li>
  <li><strong>Limites de serviço</strong>: incluído um [tópico de limitações de serviço](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#tech_limits).</li>
  </ul></td>
</tr>
<tr>
  <td>13 de maio de 2019</td>
  <td><ul>
  <li><strong>Logs de mudanças de versão</strong>: incluído que as novas atualizações de correção estão disponíveis para [1.14.1_1518](/docs/containers?topic=containers-changelog#1141_1518), [1.13.6_1521](/docs/containers?topic=containers-changelog#1136_1521), [1.12.8_1552](/docs/containers?topic=containers-changelog#1128_1552), [1.11.10_1558](/docs/containers?topic=containers-changelog#11110_1558) e [1.10.13_1558](/docs/containers?topic=containers-changelog#11013_1558).</li>
  <li><strong>Tipos de nó do trabalhador</strong>: removidos todos os [tipos de nó do trabalhador da máquina virtual](/docs/containers?topic=containers-planning_worker_nodes#vm) que têm 48 ou mais núcleos por [status de nuvem ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/status?component=containers-kubernetes&selected=status). Ainda é possível provisionar [nós do trabalhador de bare metal](/docs/containers?topic=containers-plan_clusters#bm) com 48 ou mais núcleos.</li></ul></td>
</tr>
<tr>
  <td>08 de maio de 2019</td>
  <td><ul>
  <li><strong>API</strong>: incluído um link para os [docs do swagger da API global ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://containers.cloud.ibm.com/global/swagger-global-api/#/).</li>
  <li><strong>Cloud Object Storage</strong>: [incluído um guia de resolução de problemas para o Cloud Object Storage](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_pvc_pending) em seus clusters do {{site.data.keyword.containerlong_notm}}.</li>
  <li><strong>Estratégia do Kubernetes</strong>: incluído um tópico sobre [Quais conhecimentos e qualificações técnicas é bom ter antes de mover meus apps para o {{site.data.keyword.containerlong_notm}}?](/docs/containers?topic=containers-strategy#knowledge).</li>
  <li><strong>Kubernetes versão 1.14</strong>: incluído que a [liberação do Kubernetes 1.14](/docs/containers?topic=containers-cs_versions#cs_v114) é certificada.</li>
  <li><strong>Tópicos de referência</strong>: informações atualizadas para várias operações de ligação de serviço, `logging` e `nlb` nas páginas de referência de [acesso de usuário](/docs/containers?topic=containers-access_reference) e [CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli).</li></ul></td>
</tr>
<tr>
  <td>07 de maio de 2019</td>
  <td><ul>
  <li><strong>Provedor DNS do cluster</strong>: [explicados os benefícios do CoreDNS](/docs/containers?topic=containers-cluster_dns) agora que os clusters que executam o Kubernetes 1.14 e mais recente suportam somente o CoreDNS.</li>
  <li><strong>Nós de borda</strong>: incluído o suporte ao balanceador de carga privado incluído para [nós de borda](/docs/containers?topic=containers-edge).</li>
  <li><strong>Clusters grátis</strong>: esclarecido onde os [clusters grátis](/docs/containers?topic=containers-regions-and-zones#regions_free) são suportados.</li>
  <li><strong>Novo! Integrações</strong>: incluídas informações de reestrutura sobre os [serviços do {{site.data.keyword.Bluemix_notm}} e integrações de terceiros](/docs/containers?topic=containers-ibm-3rd-party-integrations), [integrações populares](/docs/containers?topic=containers-supported_integrations) e [parcerias](/docs/containers?topic=containers-service-partners).</li>
  <li><strong>Novo! Kubernetes versão 1.14</strong>: crie ou atualize seus clusters para o [Kubernetes 1.14](/docs/containers?topic=containers-cs_versions#cs_v114).</li>
  <li><strong>Descontinuado o Kubernetes versão 1.11</strong>: [atualize](/docs/containers?topic=containers-update) quaisquer clusters que executem o [Kubernetes 1.11](/docs/containers?topic=containers-cs_versions#cs_v111) antes de se tornarem não suportados.</li>
  <li><strong>Permissões</strong>: incluída uma pergunta mais frequente, [Quais políticas de acesso eu forneço aos meus usuários do cluster?](/docs/containers?topic=containers-faqs#faq_access)</li>
  <li><strong>Conjuntos de trabalhadores</strong>: incluídas instruções de como [aplicar rótulos a conjuntos de trabalhadores existentes](/docs/containers?topic=containers-add_workers#worker_pool_labels).</li>
  <li><strong>Tópicos de referência</strong>: para suportar novos recursos, tais como Kubernetes 1.14, as páginas de referência do [log de mudanças](/docs/containers?topic=containers-changelog#changelog) são atualizadas.</li></ul></td>
</tr>
<tr>
  <td>1º de maio de 2019</td>
  <td><strong>Designando acesso à infraestrutura</strong>: revise as [etapas para designar permissões do IAM para abrir casos de suporte](/docs/containers?topic=containers-users#infra_access).</td>
</tr>
</tbody></table>


