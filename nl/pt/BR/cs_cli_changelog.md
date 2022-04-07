---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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


# CLI changelog
{: #cs_cli_changelog}

No terminal, você é notificado quando atualizações para a CLI `ibmcloud` e plug-ins estão disponíveis. Certifique-se de manter sua CLI atualizada para que seja possível usar todos os comandos e sinalizações disponíveis.
{:shortdesc}

Para instalar o plug-in da CLI do {{site.data.keyword.containerlong}}, consulte [Instalando a CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).

Consulte a tabela a seguir para obter um resumo das mudanças para cada versão do plug-in da CLI do {{site.data.keyword.containerlong_notm}}.

<table summary="Visão geral das mudanças de versão do plug-in da CLI do {{site.data.keyword.containerlong_notm}}">
<caption>Changelog para o plug-in da CLI do  {{site.data.keyword.containerlong_notm}}</caption>
<thead>
<tr>
<th>Versão</th>
<th>Data de liberação</th>
<th>Mudanças</th>
</tr>
</thead>
<tbody>
<tr>
<td>0.3.34</td>
<td>31 de maio de 2019</td>
<td>Inclui suporte para criar clusters do Red Hat OpenShift on IBM Cloud.<ul>
<li>Inclui suporte para versões do OpenShift na sinalização `--kube-version` do comando `cluster-create`. Por exemplo, para criar um cluster padrão do OpenShift, é possível passar `--kube-version 3.11_openshift` em seu comando `cluster-create`.</li>
<li>Inclui o comando `versions` para listar todas as versões suportadas do Kubernetes e do OpenShift.</li>
<li>Descontinua o comando `kube-versions`.</li>
</ul></td>
</tr>
<tr>
<td>0.3.33</td>
<td>30 de maio de 2019</td>
<td><ul>
<li>Inclui a sinalização <code>--powershell</code> no comando `cluster-config` para recuperar variáveis de ambiente do Kubernetes no formato Windows PowerShell.</li>
<li>Descontinua os comandos `region-get`, `region-set` e `regions`. Para obter mais informações, consulte [funcionalidade de terminal global](/docs/containers?topic=containers-regions-and-zones#endpoint).</li>
</ul></td>
</tr>
<tr>
<td>0.3.28</td>
<td>23 de maio de 2019</td>
<td><ul><li>Inclui o comando [<code>ibmcloud ks alb-create</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_create) para criar ALBs do Ingress. Para obter mais informações, consulte [Escalando ALBs](/docs/containers?topic=containers-ingress#scale_albs).</li>
<li>Inclui o comando [<code>ibmcloud ks infra-permissions-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get) para verificar se as credenciais que permitem o [acesso ao portfólio da infraestrutura do IBM Cloud (SoftLayer)](/docs/containers?topic=containers-users#api_key) para o grupo de recursos e região de destino estão omitindo permissões de infraestrutura sugeridas ou necessárias.</li>
<li>Inclui a sinalização <code>--private-only</code> no comando `zone-network-set` para desconfigurar a VLAN pública para os metadados do conjunto de trabalhadores para que os nós do trabalhador subsequentes nessa zona do conjunto de trabalhadores sejam conectados somente a uma VLAN privada.</li>
<li>Remove a sinalização <code>--force-update</code> por meio do comando `worker-update`.</li>
<li>Inclui a coluna **ID da VLAN** com a saída dos comandos `albs` e `alb-get`.</li>
<li>Inclui a coluna **Metro de multizona** na saída do comando `supported-locations` para designar zonas com capacidade de multizona.</li>
<li>Inclui os campos **Estado principal** e **Funcionamento principal** na saída do comando `cluster-get`. Para obter mais informações, consulte [Estados principais](/docs/containers?topic=containers-health#states_master).</li>
<li>Atualiza as traduções do texto de ajuda.</li>
</ul></td>
</tr>
<tr>
<td>0.3.8</td>
<td>30 de abril de 2019</td>
<td>Inclui suporte para a [funcionalidade de terminal global](/docs/containers?topic=containers-regions-and-zones#endpoint) na versão `0.3`. Por padrão, é possível agora visualizar e gerenciar todos os seus recursos do {{site.data.keyword.containerlong_notm}} em todos os locais. Não é necessário ter como destino uma região para trabalhar com recursos.</li>
<ul><li>Inclui o comando [<code>ibmcloud ks supported-locations</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_supported-locations) para listar todos os locais que o {{site.data.keyword.containerlong_notm}} suporta.</li>
<li>Inclui a sinalização <code>--locations</code> nos comandos `clusters` e `zones` para filtrar recursos por um ou mais locais.</li>
<li>Inclui a sinalização <code>--region</code> nos comandos `credential-set/unset/get`, `api-key-reset` e `vlan-spanning-get`. Para executar esses comandos, deve-se especificar uma região na sinalização `--region`.</li></ul></td>
</tr>
<tr>
<td>0.2.102</td>
<td>15 de abril de 2019</td>
<td>Inclui o [grupo de comandos `ibmcloud ks nlb-dns`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#nlb-dns) para registrar e gerenciar um nome do host para endereços IP do balanceador de carga de rede (NLB) e o [grupo de comandos `ibmcloud ks nlb-dns-monitor`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor) para criar e modificar monitores de verificação de funcionamento para nomes de host do NLB. Para obter mais informações, consulte [Registrando IPs do NLB com um nome de host DNS](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname_dns).
</td>
</tr>
<tr>
<td>0.2.99</td>
<td>09 de abril de 2019</td>
<td><ul>
<li>Atualiza o texto de ajuda.</li>
<li>Atualiza a versão do Go para 1.12.2.</li>
</ul></td>
</tr>
<tr>
<td>0.2.95</td>
<td>03 de abril de 2019</td>
<td><ul>
<li>Inclui o suporte de versionamento para complementos de cluster gerenciados.</li>
<ul><li>Inclui o comando [<code>ibmcloud ks addon-versions</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_addon_versions).</li>
<li>Inclui o sinalizador <code>--version</code> em comandos [ibmcloud ks cluster-addon-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable).</li></ul>
<li>Atualiza as traduções do texto de ajuda.</li>
<li>Atualiza os links curtos para a documentação no texto de ajuda.</li>
<li>Corrige um erro que imprimia as mensagens de erro JSON em um formato incorreto.</li>
<li>Corrige um erro que, por meio do uso do sinalizador silencioso (`-s`) em alguns comandos, evitava a impressão de erros.</li>
</ul></td>
</tr>
<tr>
<td>0,2,80</td>
<td>19 de março de 2019</td>
<td><ul>
<li>Inclui suporte para ativar a [comunicação mestre-para-trabalhador com os terminais em serviço](/docs/containers?topic=containers-plan_clusters#workeruser-master) em clusters padrão que executam o Kubernetes versão 1.11 ou mais recente em [Contas ativadas para VRF](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).<ul>
<li>Inclui os sinalizadores `--private-service-endpoint` e `--public-service-endpoint` no comando [<code>ibmcloud ks cluster-create</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create).</li>
<li>Inclui os campos **URL de terminal em serviço público** e **URL de terminal em serviço privado** para a saída de <code>ibmcloud ks cluster-get</code>.</li>
<li>Inclui o comando [<code>ibmcloud ks cluster-feature-enable private-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_private_service_endpoint).</li>
<li>Inclui o comando [<code>ibmcloud ks cluster-feature-enable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_public_service_endpoint).</li>
<li>Inclui o comando [<code>ibmcloud ks cluster-feature-disable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable).</li>
</ul></li>
<li>Atualiza a documentação e a tradução.</li>
<li>Atualiza a versão do Go para 1.11.6.</li>
<li>Resolve problemas de rede intermitentes para usuários do macOS.</li>
</ul></td>
</tr>
<tr>
<td>0,2.75</td>
<td>14 de março de 2019</td>
<td><ul><li>Oculta HTML bruto a partir de saídas de erro.</li>
<li>Corrige typos em texto de ajuda.</li>
<li>Corrige a tradução do texto da ajuda.</li>
</ul></td>
</tr>
<tr>
<td>0,2.61</td>
<td>26 de fevereiro de 2019</td>
<td><ul>
<li>Inclui o comando `ibmcloud ks worker-update`, que cria um ID de serviço do IAM para o cluster, as políticas, a chave de API e os segredos de extração de imagem para que os contêineres que são executados no namespace `default` do Kubernetes possam extrair imagens do IBM Cloud Container Registry. Para novos clusters, os segredos de extração de imagem que usam credenciais do IAM são criados por padrão. Use esse comando para atualizar os clusters existentes ou se o seu cluster tiver um erro de segredo de extração de imagem durante a criação. Para obter mais informações, consulte  [ o doc ](https://test.cloud.ibm.com/docs/containers?topic=containers-images#cluster_registry_auth).</li>
<li>Corrige um erro no qual as falhas de `ibmcloud ks init` causaram a impressão da saída da ajuda.</li>
</ul></td>
</tr>
<tr>
<td>0,2.53</td>
<td>19 de fevereiro de 2019</td>
<td><ul><li>Corrige um erro no qual a região foi ignorada para `ibmcloud ks api-key-reset`, `ibmcloud ks credential-get/set` e `ibmcloud ks vlan-spanning-get`.</li>
<li>Melhora o desempenho para  ` ibmcloud ks worker-update `.</li>
<li>Inclui a versão do complemento em prompts `ibmcloud ks cluster-addon-enable`.</li>
</ul></td>
</tr>
<tr>
<td>0,2.44</td>
<td>08 de fevereiro de 2019</td>
<td><ul>
<li>Inclui a opção `-- skip-rbac` no comando `ibmcloud ks cluster-config` para ignorar a inclusão de funções RBAC do usuário Kubernetes com base nas funções de acesso ao serviço do {{site.data.keyword.Bluemix_notm}} IAM para a configuração do cluster. Inclua essa opção somente se você [gerenciar suas próprias funções RBAC do Kubernetes](/docs/containers?topic=containers-users#rbac). Se você usar [funções de acesso ao serviço {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-access_reference#service) para gerenciar todos os seus usuários do RBAC, não inclua essa opção.</li>
<li>Atualiza a versão do Go para 1.11.5.</li>
</ul></td>
</tr>
<tr>
<td>0,2,40</td>
<td>06 de fevereiro de 2019</td>
<td><ul>
<li>Inclui os comandos [<code>ibmcloud ks cluster-addons</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addons), [<code>ibmcloud ks cluster-addon-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable)e [<code>ibmcloud ks cluster-addon-disable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_disable) para trabalhar com complementos de cluster gerenciados, como o [Istio](/docs/containers?topic=containers-istio) e os complementos gerenciados [Knative](/docs/containers?topic=containers-serverless-apps-knative) para {{site.data.keyword.containerlong_notm}}.</li>
<li>Melhora o texto de ajuda para {{site.data.keyword.Bluemix_dedicated_notm}} usuários do comando <code>ibmcloud ks vlans</code>.</li></ul></td>
</tr>
<tr>
<td>0,2,30</td>
<td>31 de janeiro de 2019</td>
<td>Aumenta o valor de tempo limite padrão de `ibmcloud ks cluster-config` para `500s`.</td>
</tr>
<tr>
<td>0,2,19</td>
<td>16 de janeiro de 2019</td>
<td><ul><li>Inclui a variável de ambiente `IKS_BETA_VERSION` para ativar a versão beta reprojetada da CLI do plug-in do {{site.data.keyword.containerlong_notm}}. Para experimentar a versão reprojetada, consulte [Usando a estrutura de comando beta](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_beta).</li>
<li>Aumenta o valor de tempo limite padrão de `ibmcloud ks subnets` para `60s`.</li>
<li>Correções de bug e de erro de tradução.</li></ul></td>
</tr>
<tr>
<td>0,1.668</td>
<td>18 de dezembro de 2018</td>
<td><ul><li>Muda o terminal de API padrão de <code>https://containers.bluemix.net</code> para <code>https://containers.cloud.ibm.com</code>.</li>
<li>Corrige o erro para que as conversões sejam exibidas corretamente para a ajuda do comando e as mensagens de erro.</li>
<li>Exibe a ajuda do comando mais rápido.</li></ul></td>
</tr>
<tr>
<td>0.1.654</td>
<td>05 de dezembro de 2018</td>
<td>Atualiza a documentação e a tradução.</td>
</tr>
<tr>
<td>0.1.638</td>
<td>15 de novembro de 2018</td>
<td>
<ul><li>Inclui o alias [<code>ibmcloud ks cluster-refresh</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh) no comando `apiserver-refresh`.</li>
<li>Inclui o nome do grupo de recursos na saída de <code>ibmcloud ks cluster-get</code> e <code>ibmcloud ks clusters</code>.</li></ul>
</td>
</tr>
<tr>
<td>0.1.635</td>
<td>06 de novembro de 2018</td>
<td>Inclui comandos para gerenciar as atualizações automáticas do complemento de cluster ALB do Ingress incluído:<ul>
<li>[<code>ibmcloud ks alb-autoupdate-disable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_disable)</li>
<li>[<code>ibmcloud ks alb-autoupdate-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_enable)</li>
<li>[<code>ibmcloud ks alb-autoupdate-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_get)</li>
<li>[<code>ibmcloud ks alb-rollback</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_rollback)</li>
<li>[<code>ibmcloud ks alb-update</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_update)</li>
</ul></td>
</tr>
<tr>
<td>0.1.621</td>
<td>30 de outubro de 2018</td>
<td><ul>
<li>Inclui o [comando <code>ibmcloud ks credential-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credential_get).</li>
<li>Inclui suporte para a origem de log de <code>armazenamento</code> em todos os comandos de criação de log do cluster. Para obter mais informações, veja <a href="/docs/containers?topic=containers-health#logging">Entendendo o cluster e o encaminhamento de log do app</a>.</li>
<li>Inclui a sinalização `--network` no comando [<code>ibmcloud ks cluster-config</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config), que faz download do arquivo de configuração do Calico para Executar todos os comandos do Calico.</li>
<li>Correções de bug secundário e refatoração</li></ul>
</td>
</tr>
<tr>
<td>0,1.593</td>
<td>10 de outubro de 2018</td>
<td><ul><li>Inclui o ID do grupo de recursos na saída de <code>ibmcloud ks cluster-get</code>.</li>
<li>Quando o [{{site.data.keyword.keymanagementserviceshort}} está ativado](/docs/containers?topic=containers-encryption#keyprotect) como um provedor de serviço de gerenciamento de chaves (KMS) em seu cluster, inclui o campo ativado pelo KMS na saída de <code>ibmcloud ks cluster-get</code>.</li></ul></td>
</tr>
<tr>
<td>0,1.591</td>
<td>02 de outubro de 2018</td>
<td>Inclui suporte para  [ grupos de recursos ](/docs/containers?topic=containers-clusters#cluster_prepare).</td>
</tr>
<tr>
<td>0,1.590</td>
<td>01 de outubro de 2018</td>
<td><ul>
<li>Inclui os comandos [<code>ibmcloud ks logging-collect</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect) e [<code>ibmcloud ks logging-collect-status</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect_status) para coletar logs do servidor de API em seu cluster.</li>
<li>Inclui o [comando <code>ibmcloud ks key-protect-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_key_protect) para ativar o {{site.data.keyword.keymanagementserviceshort}} como um provedor de serviço de gerenciamento de chave (KMS) em seu cluster.</li>
<li>Inclui a sinalização <code>--skip-master-health</code> nos comandos [ibmcloud ks worker-reboot](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) e [ibmcloud ks worker-reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) para ignorar a verificação de funcionamento principal antes de iniciar a reinicialização ou o recarregamento.</li>
<li>Renomeia <code>Owner Email</code> para <code>Owner</code> na saída de <code>ibmcloud ks cluster-get</code>.</li></ul></td>
</tr>
</tbody>
</table>
