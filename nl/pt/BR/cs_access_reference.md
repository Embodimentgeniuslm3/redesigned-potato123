---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="blank"}
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



# Permissões de acesso de
{: #access_reference}

Ao [designar permissões de cluster](/docs/containers?topic=containers-users), pode ser difícil julgar qual função precisa ser designada a um usuário. Use as tabelas nas seções a seguir para determinar o nível mínimo de permissões que são necessárias para executar tarefas comuns no {{site.data.keyword.containerlong}}.
{: shortdesc}

A partir de 30 de janeiro de 2019, o {{site.data.keyword.containerlong_notm}} tem uma nova maneira de autorizar usuários com o {{site.data.keyword.Bluemix_notm}} IAM: [funções de acesso ao serviço](#service). Essas funções de serviço são usadas para conceder acesso aos recursos dentro do cluster, como namespaces do Kubernetes. Para obter mais informações, consulte o blog [Introduzindo funções de serviço e namespaces no IAM para obter controle mais granular de acesso ao cluster ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/).
{: note}

## {{site.data.keyword.Bluemix_notm}} Funções da Plataforma IAM
{: #iam_platform}

O {{site.data.keyword.containerlong_notm}} é configurado para usar funções do {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). As funções da plataforma do {{site.data.keyword.Bluemix_notm}} IAM determinam as ações que os usuários podem executar em recursos do {{site.data.keyword.Bluemix_notm}}, como clusters, nós do trabalhador e balanceadores de carga do aplicativo (ALBs) Ingress. As funções da plataforma do {{site.data.keyword.Bluemix_notm}} IAM também configuram automaticamente as permissões de infraestrutura básica para os usuários. Para configurar funções da plataforma, consulte [Designando permissões de plataforma do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform).
{: shortdesc}

<p class="tip">Não designe funções de plataforma do IAM do {{site.data.keyword.Bluemix_notm}} ao mesmo tempo que uma função de serviço. Deve-se designar funções de plataforma e de serviço separadamente.</p>

Em cada uma das seções a seguir, as tabelas mostram as permissões de gerenciamento de cluster, de criação de log e do Ingress concedidas por cada função da plataforma do {{site.data.keyword.Bluemix_notm}} IAM. As tabelas são organizadas alfabeticamente por nome de comando da CLI.

* [ Ações que não requerem permissões ](#none-actions)
* [ Ações do visualizador ](#view-actions)
* [ Ações do Editor ](#editor-actions)
* [ Ações do Operador ](#operator-actions)
* [ Ações do Administrador ](#admin-actions)

### Ações que não requerem permissões
{: #none-actions}

Qualquer usuário em sua conta que execute o comando da CLI ou faça a chamada API para a ação na tabela a seguir verá o resultado, mesmo que não tenha permissões designadas.
{: shortdesc}

<table>
<caption>Visão geral de comandos da CLI e chamadas API que não requerem permissões no {{site.data.keyword.containerlong_notm}}</caption>
<thead>
<th id="none-actions-action">Ação</th>
<th id="none-actions-cli">comando da CLI</th>
<th id="none-actions-api">Chamada API</th>
</thead>
<tbody>
<tr>
<td>Visualize uma lista de versões suportadas para complementos gerenciados no {{site.data.keyword.containerlong_notm}}.</td>
<td><code>[ibmcloud ks addon-versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_addon_versions)</code></td>
<td><code>[GET /v1/addon](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetAddons)</code></td>
</tr>
<tr>
<td>Direcione ou visualize o terminal de API para {{site.data.keyword.containerlong_notm}}.</td>
<td><code>[ibmcloud ks api
](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cli_api)</code></td>
<td>-</td>
</tr>
<tr>
<td>Visualizar uma lista de comandos e parâmetros suportados.</td>
<td><code>[ibmcloud ks help](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_help)</code></td>
<td>-</td>
</tr>
<tr>
<td>Inicialize o plug-in do {{site.data.keyword.containerlong_notm}} ou especifique a região em que você deseja criar ou acessar clusters do Kubernetes.</td>
<td><code>[ibmcloud ks init](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_init)</code></td>
<td>-</td>
</tr>
<tr>
<td>Descontinuado: visualizar uma lista de versões do Kubernetes suportadas no {{site.data.keyword.containerlong_notm}}.</td>
<td><code>[ibmcloud ks kube-versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_kube_versions)</code></td>
<td><code>[GET /v1/kube-versions](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetKubeVersions)</code></td>
</tr>
<tr>
<td>Visualizar uma lista de tipos de máquina disponíveis para seus nós do trabalhador.</td>
<td><code>[ibmcloud ks machine-types](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types)</code></td>
<td><code>[GET /v1/datacenters/ { datacenter } /machine-types](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetDatacenterMachineTypes)</code></td>
</tr>
<tr>
<td>Visualize as mensagens atuais para o usuário do IBMid.</td>
<td><code>[ibmcloud ks messages](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_messages)</code></td>
<td><code>[GET /v1/messages](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetMessages)</code></td>
</tr>
<tr>
<td>Descontinuado: localizar a região do {{site.data.keyword.containerlong_notm}} na qual você está atualmente.</td>
<td><code>[ibmcloud ks region](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_region)</code></td>
<td>-</td>
</tr>
<tr>
<td>Descontinuado: configurar a região para o {{site.data.keyword.containerlong_notm}}.</td>
<td><code>[ibmcloud ks region-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_region-set)</code></td>
<td>-</td>
</tr>
<tr>
<td>Descontinuado: listar as regiões disponíveis.</td>
<td><code>[ibmcloud ks regions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_regions)</code></td>
<td><code>[GET /v1/regions](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetRegions)</code></td>
</tr>
<tr>
<td>Visualizar uma lista de locais suportados no {{site.data.keyword.containerlong_notm}}.</td>
<td><code>[ibmcloud ks supported-locations](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_supported-locations)</code></td>
<td><code>[GET /v1/locations](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/ListLocations)</code></td>
</tr>
<tr>
<td>Visualizar uma lista de versões suportadas no {{site.data.keyword.containerlong_notm}}.</td>
<td><code>[ibmcloud ks versions](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_versions)</code></td>
<td>-</td>
</tr>
<tr>
<td>Visualize uma lista de zonas disponíveis nas quais é possível criar um cluster.</td>
<td><code>[ibmcloud ks zones](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_datacenters)</code></td>
<td><code>[GET /v1/zones](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetZones)</code></td>
</tr>
</tbody>
</table>

### Ações do visualizador
{: #view-actions}

A função da plataforma **Visualizador** inclui as [ações que não requerem permissões](#none-actions), mais as permissões que são mostradas na tabela a seguir. Com a função **Visualizador**, os usuários, como os auditores ou os agentes de faturamento, podem ver detalhes do cluster, mas não modificar a infraestrutura.
{: shortdesc}

<table>
<caption>Visão geral de comandos da CLI e chamadas da API que requerem a função da plataforma Visualizador no {{site.data.keyword.containerlong_notm}}</caption>
<thead>
<th id="view-actions-mngt">Ação</th>
<th id="view-actions-cli">comando da CLI</th>
<th id="view-actions-api">Chamada API</th>
</thead>
<tbody>
<tr>
<td>Visualizar informações para uma Ingress ALB.</td>
<td><code>[ibmcloud ks alb-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_get)</code></td>
<td><code>[GET /albs/{albId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetClusterALB)</code></td>
</tr>
<tr>
<td>Visualize os tipos de ALB suportados na região.</td>
<td><code>[ibmcloud ks alb-types](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_types)</code></td>
<td><code>[GET /albtypes](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetAvailableALBTypes)</code></td>
</tr>
<tr>
<td>Liste todos os ALBs do Ingress em um cluster.</td>
<td><code>[ibmcloud ks albs](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_albs)</code></td>
<td><code>[GET /clusters/ { idOrName }](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetClusterALBs)</code></td>
</tr>
<tr>
<td>Visualize o nome e o endereço de e-mail para o proprietário da chave de API do {{site.data.keyword.Bluemix_notm}} IAM para um grupo de recursos e região.</td>
<td><code>[ibmcloud ks api-key-info](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_info)</code></td>
<td><code>[GET /v1/logging/{idOrName}/clusterkeyowner](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetClusterKeyOwner)</code></td>
</tr>
<tr>
<td>Faça download dos dados de configuração e certificados do Kubernetes para se conectar ao seu cluster e execute os comandos `kubectl`.</td>
<td><code>[ibmcloud ks cluster-config](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterConfig)</code></td>
</tr>
<tr>
<td>Visualizar informações para um cluster.</td>
<td><code>[ibmcloud ks cluster-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get)</code></td>
<td><code>[GET /v1/clusters/ { idOrName }](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetCluster)</code></td>
</tr>
<tr>
<td>Liste todos os serviços em todos os namespaces que estão ligados a um cluster.</td>
<td><code>[ibmcloud ks cluster-services](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_services)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ListServicesForAllNamespaces)</code></td>
</tr>
<tr>
<td>Listar todos os clusters.</td>
<td><code>[ibmcloud ks clusters](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_clusters)</code></td>
<td><code>[GET /v1/clusters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusters)</code></td>
</tr>
<tr>
<td>Obtenha as credenciais de infraestrutura que estão configuradas para a conta do {{site.data.keyword.Bluemix_notm}} para acessar um portfólio de infraestrutura diferente do IBM Cloud (SoftLayer).</td>
<td><code>[        ibmcloud ks credential-get
        ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credential_get)</code></td><td><code>[GET /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetUserCredentials)</code></td>
</tr>
<tr>
<td>Verificar se as credenciais que permitem o acesso ao portfólio da infraestrutura do IBM Cloud (SoftLayer) para a região de destino e o grupo de recursos estão omitindo permissões de infraestrutura sugeridas ou necessárias.</td>
<td><code>[ibmcloud ks infra-permissions-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get)</code></td>
<td><code>[GET /v1/infra-permissions](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetInfraPermissions)</code></td>
</tr>
<tr>
<td>Visualize o status para atualizações automáticas do complemento Fluentd.</td>
<td><code>[ibmcloud ks logging-autoupdate-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>Visualizar o terminal de criação de log padrão para a região de destino.</td>
<td>-</td>
<td><code>[GET /v1/logging/{idOrName}/default](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetDefaultLoggingEndpoint)</code></td>
</tr>
<tr>
<td>Liste todas as configurações de encaminhamento de log no cluster ou para uma origem de log específica no cluster.</td>
<td><code>[ibmcloud ks logging-config-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/FetchLoggingConfigs) e [GET /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/FetchLoggingConfigsForSource)</code></td>
</tr>
<tr>
<td>Visualize informações para uma configuração de filtragem de log.</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/FetchFilterConfig)</code></td>
</tr>
<tr>
<td>Liste todas as configurações de filtro de criação de log no cluster.</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/FetchFilterConfigs)</code></td>
</tr>
<tr>
<td>Listar todos os serviços que estão ligados a um namespace específico.</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/services/{namespace}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ListServicesInNamespace)</code></td>
</tr>
<tr>
<td>Liste todas as sub-redes gerenciadas pelo usuário que são ligadas a um cluster.</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/usersubnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterUserSubnet)</code></td>
</tr>
<tr>
<td>Liste as sub-redes disponíveis na conta de infraestrutura.</td>
<td><code>[ibmcloud ks subnets](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_subnets)</code></td>
<td><code>[GET /v1/subnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/properties/ListSubnets)</code></td>
</tr>
<tr>
<td>Visualize o status de Ampliação de VLAN para a conta de infraestrutura.</td>
<td><code>[ ibmcloud ks vlan-spanning-get ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get)</code></td>
<td><code>[GET /v1/subnets/vlan-spanning](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetVlanSpanning)</code></td>
</tr>
<tr>
<td>Quando configurado para um cluster: listar VLANs com as quais o cluster está conectado em uma zona.</br>Quando configurado para todos os clusters na conta: listar todas as VLANs disponíveis em uma zona.</td>
<td><code>[ibmcloud ks vlans](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans)</code></td>
<td><code>[GET /v1/datacenters/{datacenter}/vlans](https://containers.cloud.ibm.com/global/swagger-global-api/#/properties/GetDatacenterVLANs)</code></td>
</tr>
<tr>
<td>Liste todos os webhooks para um cluster.</td>
<td>-</td>
<td><code>[GET /v1/clusters/ { idOrName } /webhooks](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterWebhooks)</code></td>
</tr>
<tr>
<td>Visualizar informações para um nó do trabalhador.</td>
<td><code>[ibmcloud ks worker-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkers)</code></td>
</tr>
<tr>
<td>Visualizar informações para um conjunto de trabalhadores.</td>
<td><code>[ibmcloud ks worker-pool-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkerPool)</code></td>
</tr>
<tr>
<td>Liste todos os conjuntos de trabalhadores em um cluster.</td>
<td><code>[ibmcloud ks worker-pools](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pools)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkerPools)</code></td>
</tr>
<tr>
<td>Liste todos os nós do trabalhador em um cluster.</td>
<td><code>[ibmcloud ks workers](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_workers)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterWorkers)</code></td>
</tr>
</tbody>
</table>

### Ações do Editor
{: #editor-actions}

A função da plataforma **Editor** inclui as permissões que são concedidas pelo **Visualizador**, mais o seguinte. Com a função **Editor**, os usuários, como os desenvolvedores, podem ligar serviços, trabalhar com recursos do Ingress e configurar o encaminhamento de log para seus aplicativos, mas não modificar a infraestrutura. **Dica**: use essa função para desenvolvedores de aplicativos e designe a função **Desenvolvedor** do <a href="#cloud-foundry">Cloud Foundry</a>.
{: shortdesc}

<table>
<caption>Visão geral de comandos da CLI e chamadas da API que requerem a função da plataforma Editor no {{site.data.keyword.containerlong_notm}}</caption>
<thead>
<th id="editor-actions-mngt">Ação</th>
<th id="editor-actions-cli">comando da CLI</th>
<th id="editor-actions-api">Chamada API</th>
</thead>
<tbody>
<tr>
<td>Desative as atualizações automáticas para o complemento ALB do Ingress.</td>
<td><code>[ibmcloud ks alb-autoupdate-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_disable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Ative as atualizações automáticas para o complemento ALB do Ingress.</td>
<td><code>[ibmcloud ks alb-autoupdate-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_enable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Verifique se as atualizações automáticas para o complemento ALB do Ingress estão ativadas.</td>
<td><code>[ibmcloud ks alb-autoupdate-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_get)</code></td>
<td><code>[GET /clusters/ { idOrName } /updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>Ative ou desative um ALB de Ingresso.</td>
<td><code>[ibmcloud ks alb-configure](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure)</code></td>
<td><code>[POST /albs](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/EnableALB) e [DELETE /albs/{albId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/)</code></td>
</tr>
<tr>
<td>Criar um ALB do Ingress.</td>
<td><code>[ibmcloud ks alb-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_create)</code></td>
<td><code>[POST /clusters/{idOrName}/zone/{zoneId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/CreateALB)</code></td>
</tr>
<tr>
<td>Retroceder a atualização do complemento ALB do Ingress para a construção em que os pods do ALB estavam em execução anteriormente.</td>
<td><code>[ibmcloud ks alb-rollback](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_rollback)</code></td>
<td><code>[PUT /clusters/{idOrName}/updaterollback](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/RollbackUpdate)</code></td>
</tr>
<tr>
<td>Force uma atualização única de seus pods do ALB atualizando manualmente o complemento ALB do Ingress.</td>
<td><code>[ibmcloud ks alb-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_update)</code></td>
<td><code>[PUT /clusters/{idOrName}/update](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/UpdateALBs)</code></td>
</tr>
<tr>
<td>Crie um webhook de auditoria do servidor de API.</td>
<td><code>[ibmcloud ks apiserver-config-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_config_set)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/apiserverconfigs/UpdateAuditWebhook)</code></td>
</tr>
<tr>
<td>Exclua um webhook de auditoria do servidor de API.</td>
<td><code>[ibmcloud ks apiserver-config-unset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_config_unset)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/global/swagger-global-api/#/apiserverconfigs/DeleteAuditWebhook)</code></td>
</tr>
<tr>
<td>Ligue um serviço a um cluster. **Nota**: deve-se ter a função de Desenvolvedor do Cloud Foundry para o espaço em que a instância de serviço está.</td>
<td><code>[ibmcloud ks cluster-service-bind](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/BindServiceToNamespace)</code></td>
</tr>
<tr>
<td>Desvincular um serviço de um cluster. **Nota**: deve-se ter a função de Desenvolvedor do Cloud Foundry para o espaço em que a instância de serviço está.</td>
<td><code>[ibmcloud ks cluster-service-unbind](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_unbind)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/services/{namespace}/{serviceInstanceId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UnbindServiceFromNamespace)</code></td>
</tr>
<tr>
<td>Crie uma configuração de encaminhamento de log para todas as origens de log, exceto <code>kube-audit</code>.</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td>Atualize uma configuração de encaminhamento de log.</td>
<td><code>[ibmcloud ks logging-config-refresh](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_refresh)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/refresh](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/RefreshLoggingConfig)</code></td>
</tr>
<tr>
<td>Exclua uma configuração de encaminhamento de log para todas as origens de log, exceto <code>kube-audit</code>.</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfig)</code></td>
</tr>
<tr>
<td>Exclua todas as configurações de encaminhamento de log para um cluster.</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfigs)</code></td>
</tr>
<tr>
<td>Atualize uma configuração de encaminhamento de log.</td>
<td><code>[ibmcloud ks logging-config-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/UpdateLoggingConfig)</code></td>
</tr>
<tr>
<td>Crie uma configuração de filtragem de log.</td>
<td><code>[ibmcloud ks logging-filter-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/CreateFilterConfig)</code></td>
</tr>
<tr>
<td>Exclua uma configuração de filtragem de log.</td>
<td><code>[ibmcloud ks logging-filter-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_delete)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/DeleteFilterConfig)</code></td>
</tr>
<tr>
<td>Exclua todas as configurações de filtro de criação de log para o cluster Kubernetes.</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/DeleteFilterConfigs)</code></td>
</tr>
<tr>
<td>Atualizar uma configuração de filtragem de log.</td>
<td><code>[ibmcloud ks logging-filter-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/UpdateFilterConfig)</code></td>
</tr>
<tr>
<td>Incluir um endereço IP do NLB em um nome do host do NLB existente.</td>
<td><code>[ibmcloud ks nlb-dns-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-add)</code></td>
<td><code>[PUT /clusters/{idOrName}/add](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/UpdateDNSWithIP)</code></td>
</tr>
<tr>
<td>Criar um nome do host do DNS para registrar um endereço IP do NLB.</td>
<td><code>[ibmcloud ks nlb-dns-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-create)</code></td>
<td><code>[POST /clusters/{idOrName}/register](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/RegisterDNSWithIP)</code></td>
</tr>
<tr>
<td>Listar os nomes de host e endereços IP de NLB registrados em um cluster.</td>
<td><code>[ibmcloud ks nlb-dnss](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-ls)</code></td>
<td><code>[GET /clusters/{idOrName}/list](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/ListNLBIPsForSubdomain)</code></td>
</tr>
<tr>
<td>Remover um endereço IP de NLB de um nome de host.</td>
<td><code>[ibmcloud ks nlb-dns-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-rm)</code></td>
<td><code>[DELETE /clusters/{idOrName}/host/{nlbHost}/ip/{nlbIP}/remove](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45dns/UnregisterDNSWithIP)</code></td>
</tr>
<tr>
<td>Configure e, opcionalmente, ative um monitor de verificação de funcionamento para um nome de host do NLB existente em um cluster.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-configure](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-configure)</code></td>
<td><code>[POST /health/clusters/{idOrName}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/AddNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>Visualize as configurações para um monitor de verificação de funcionamento existente.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-get)</code></td>
<td><code>[GET /health/clusters/{idOrName}/host/{nlbHost}/config](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/GetNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>Desativar um monitor de verificação de funcionamento existente para um nome do host em um cluster.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-disable)</code></td>
<td><code>[PUT /clusters/{idOrName}/health](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/UpdateNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>Ative um monitor de verificação de funcionamento que você configurou.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-enable)</code></td>
<td><code>[PUT /clusters/{idOrName}/health](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/UpdateNlbDNSHealthMonitor)</code></td>
</tr>
<tr>
<td>Liste as configurações do monitor de verificação de funcionamento para cada nome de host do NLB em um cluster.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-ls](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-ls)</code></td>
<td><code>[GET /health/clusters/{idOrName}/list](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/ListNlbDNSHealthMonitors)</code></td>
</tr>
<tr>
<td>Listar o status de verificação de funcionamento de cada endereço IP que está registrado com um nome do host do NLB em um cluster.</td>
<td><code>[ibmcloud ks nlb-dns-monitor-status](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-status)</code></td>
<td><code>[GET /health/clusters/{idOrName}/status](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb45health45monitor/ListNlbDNSHealthMonitorStatus)</code></td>
</tr>
<tr>
<td>Crie um webhook em um cluster.</td>
<td><code>[ibmcloud ks webhook-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_webhook_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/webhooks](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterWebhooks)</code></td>
</tr>
</tbody>
</table>

### Ações do Operador
{: #operator-actions}

A função da plataforma **Operador** inclui as permissões que são concedidas pelo **Visualizador**, mais as permissões que são mostradas na tabela a seguir. Com a função **Operador**, os usuários, como os engenheiros de confiabilidade de site, os engenheiros de DevOps ou os administradores de cluster, podem incluir nós do trabalhador e solucionar problemas de infraestrutura, como recarregar um nó do trabalhador, mas não podem criar nem excluir o cluster, mudar as credenciais ou configurar recursos abrangentes de cluster, como terminais de serviço ou complementos gerenciados.
{: shortdesc}

<table>
<caption>Visão geral de comandos da CLI e chamadas da API que requerem a função da plataforma Operador no {{site.data.keyword.containerlong_notm}}</caption>
<thead>
<th id="operator-mgmt">Ação</th>
<th id="operator-cli">comando da CLI</th>
<th id="operator-api">Chamada API</th>
</thead>
<tbody>
<tr>
<td>Atualize o mestre do Kubernetes.</td>
<td><code>[ibmcloud ks apiserver-refresh](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh) (cluster-refresh)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/masters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/HandleMasterAPIServer)</code></td>
</tr>
<tr>
<td>Crie um ID de serviço do {{site.data.keyword.Bluemix_notm}} IAM para o cluster, crie uma política para o ID de serviço que designa a função de acesso ao serviço **Leitor** no {{site.data.keyword.registrylong_notm}} e, em seguida, crie uma chave de API para o ID de serviço.</td>
<td><code>[ ibmcloud ks cluster-pull-secret-apply ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply)</code></td>
<td>-</td>
</tr>
<tr>
<td>Inclua uma sub-rede em um cluster.</td>
<td><code>[ibmcloud ks cluster-subnet-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/subnets/{subnetId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterSubnet)</code></td>
</tr>
<tr>
<td>Criar uma sub-rede e incluí-la em um cluster.</td>
<td><code>[ibmcloud ks cluster-subnet-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/vlans/{vlanId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateClusterSubnet)</code></td>
</tr>
<tr>
<td>Atualizar um cluster.</td>
<td><code>[ibmcloud ks cluster-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateCluster)</code></td>
</tr>
<tr>
<td>Inclua uma sub-rede gerenciada pelo usuário em um cluster.</td>
<td><code>[ibmcloud ks cluster-user-subnet-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_user_subnet_add)</code></td>
<td><code>[POST /v1/clusters/ { idOrName } /usersubnets](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterUserSubnet)</code></td>
</tr>
<tr>
<td>Remova uma sub-rede gerenciada pelo usuário de um cluster.</td>
<td><code>[ibmcloud ks cluster-user-subnet-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_user_subnet_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/usersubnets/{subnetId}/vlans/{vlanId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveClusterUserSubnet)</code></td>
</tr>
<tr>
<td>Incluir nós do trabalhador.</td>
<td><code>[ibmcloud ks worker-add (descontinuado)](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterWorkers)</code></td>
</tr>
<tr>
<td>Crie um conjunto de trabalhadores.</td>
<td><code>[ibmcloud ks worker-pool-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateWorkerPool)</code></td>
</tr>
<tr>
<td>Rebalancear um conjunto de trabalhadores.</td>
<td><code>[ibmcloud ks worker-pool-rebalance](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>Redimensionar um conjunto de trabalhadores.</td>
<td><code>[ibmcloud ks worker-pool-resize](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>Excluir um conjunto de trabalhadores.</td>
<td><code>[ibmcloud ks worker-pool-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_rm)</code></td>
<td><code>[DELETE /v1/clusters/ { idOrName } /workerpools/ { poolidOrName }](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveWorkerPool)</code></td>
</tr>
<tr>
<td>Reinicialize um nó do trabalhador.</td>
<td><code>[ibmcloud ks worker-reboot](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>Recarregue um nó do trabalhador.</td>
<td><code>[ibmcloud ks worker-reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>Remover um nó do trabalhador.</td>
<td><code>[ibmcloud ks worker-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveClusterWorker)</code></td>
</tr>
<tr>
<td>Atualize um nó do trabalhador.</td>
<td><code>[ibmcloud ks worker-update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>Inclua uma zona em um conjunto de trabalhadores.</td>
<td><code>[ibmcloud ks zone-add](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddWorkerPoolZone)</code></td>
</tr>
<tr>
<td>Atualize a configuração de rede para uma determinada zona em um conjunto de trabalhadores.</td>
<td><code>[ibmcloud ks zone-network-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set)</code></td>
<td><code>[PATCH /v1/clusters/ { idOrName } /workerpools/ { poolidOrName } /zones/ { zoneid }](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddWorkerPoolZoneNetwork)</code></td>
</tr>
<tr>
<td>Remova uma zona do conjunto de trabalhadores.</td>
<td><code>[ibmcloud ks zone-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveWorkerPoolZone)</code></td>
</tr>
</tbody>
</table>

### Ações do Administrador
{: #admin-actions}

A função da plataforma **Administrador** inclui todas as permissões que são concedidas pelas funções **Visualizador**, **Editor** e **Operador**, mais o seguinte. Com a função **Administrador**, os usuários, como os administradores de cluster ou de conta, podem criar e excluir clusters ou configurar recursos abrangentes de cluster, como terminais de serviço ou complementos gerenciados. Para criar esses recursos de infraestrutura, como máquinas de nós do trabalhador, VLANs e sub-redes, os usuários Administradores precisam da <a href="#infra">função de infraestrutura</a> **Superusuário** ou da chave da API para que a região seja configurada com as permissões apropriadas.
{: shortdesc}

<table>
<caption>Visão geral de comandos da CLI e chamadas da API que requerem a função da plataforma Administrador no {{site.data.keyword.containerlong_notm}}</caption>
<thead>
<th id="admin-mgmt">Ação</th>
<th id="admin-cli">comando da CLI</th>
<th id="admin-api">Chamada API</th>
</thead>
<tbody>
<tr>
<td>Beta: implementar ou atualizar um certificado de sua instância do {{site.data.keyword.cloudcerts_long_notm}} para um ALB.</td>
<td><code>[ibmcloud ks alb-cert-deploy](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_deploy)</code></td>
<td><code>[POST /albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/CreateALBSecret) ou [PUT /albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/UpdateALBSecret)</code></td>
</tr>
<tr>
<td>Beta: visualizar detalhes para um segredo do ALB em um cluster.</td>
<td><code>[ibmcloud ks alb-cert-get](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_get)</code></td>
<td><code>[GET /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ViewClusterALBSecrets)</code></td>
</tr>
<tr>
<td>Beta: remover um segredo do ALB de um cluster.</td>
<td><code>[ibmcloud ks alb-cert-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_cert_rm)</code></td>
<td><code>[DELETE /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/DeleteClusterALBSecrets)</code></td>
</tr>
<tr>
<td>Liste todos os segredos do ALB em um cluster.</td>
<td><code>[ibmcloud ks alb-certs](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_certs)</code></td>
<td>-</td>
</tr>
<tr>
<td>Configure a chave de API para a conta do {{site.data.keyword.Bluemix_notm}} para acessar o portfólio de infraestrutura vinculado do IBM Cloud (SoftLayer).</td>
<td><code>[ibmcloud ks api-key-reset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset)</code></td>
<td><code>[POST /v1/keys](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/ResetUserAPIKey)</code></td>
</tr>
<tr>
<td>Desative um complemento gerenciado, como Istio ou Knative, em um cluster.</td>
<td><code>[ ibmcloud ks cluster-addon-disable ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_disable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>Ative um complemento gerenciado, como Istio ou Knative, em um cluster.</td>
<td><code>[ ibmcloud ks cluster-addon-enable ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>Listar o complemento gerenciado, como o Istio ou o Knative, que está ativado em um cluster.</td>
<td><code>[ibmcloud ks cluster-addons](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addons)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterAddons)</code></td>
</tr>
<tr>
<td>Crie um cluster grátis ou padrão. **Nota**: a função da plataforma Administrador para o {{site.data.keyword.registrylong_notm}} e a função de infraestrutura Superusuário também são necessárias.</td>
<td><code>[ibmcloud ks cluster-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create)</code></td>
<td><code>[POST /v1/clusters](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateCluster)</code></td>
</tr>
<tr>
<td>Desative um recurso especificado para um cluster, como o terminal em serviço público para o cluster mestre.</td>
<td><code>[ ibmcloud ks cluster-feature-disable ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable)</code></td>
<td>-</td>
</tr>
<tr>
<td>Ative um recurso especificado para um cluster, como o terminal em serviço privado para o cluster mestre.</td>
<td><code>[ibmcloud ks cluster-feature-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable)</code></td>
<td>-</td>
</tr>
<tr>
<td>Excluir um cluster.</td>
<td><code>[ibmcloud ks cluster-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveCluster)</code></td>
</tr>
<tr>
<td>Configure credenciais de infraestrutura para a conta do {{site.data.keyword.Bluemix_notm}} para acessar um portfólio diferente de infraestrutura do IBM Cloud (SoftLayer).</td>
<td><code>[ibmcloud ks credential-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set)</code></td>
<td><code>[POST /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/accounts/StoreUserCredentials)</code></td>
</tr>
<tr>
<td>Remova as credenciais de infraestrutura para a conta do {{site.data.keyword.Bluemix_notm}} para acessar um portfólio de infraestrutura diferente do IBM Cloud (SoftLayer).</td>
<td><code>[ibmcloud ks credential-unset](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset)</code></td>
<td><code>[DELETE /v1/credentials](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/accounts/RemoveUserCredentials)</code></td>
</tr>
<tr>
<td>Beta: criptografar segredos do Kubernetes usando o {{site.data.keyword.keymanagementservicefull}}.</td>
<td><code>[Teclas ibmcloud ks key-protect-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_messages)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/kms](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateKMSConfig)</code></td>
</tr>
<tr>
<td>Desative as atualizações automáticas para o complemento de cluster Fluentd.</td>
<td><code>[ibmcloud ks logging-autoupdate-disable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_disable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Ative as atualizações automáticas para o complemento de cluster Fluentd.</td>
<td><code>[ ks ibmcloud ks logging-autoupdate-enable ](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_enable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>Colete uma captura instantânea de logs do servidor de API em um depósito do {{site.data.keyword.cos_full_notm}}.</td>
<td><code>[ibmcloud ks logging-collect](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect)</code></td>
<td>[POST /v1/log-collector/{idOrName}/masterlogs](https://containers.cloud.ibm.com/global/swagger-global-api/#/log45collector/CreateMasterLogCollection)</td>
</tr>
<tr>
<td>Consulte o status da solicitação de captura instantânea de logs do servidor de API.</td>
<td><code>[ibmcloud ks logging-collect-status](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect_status)</code></td>
<td>[GET /v1/log-collector/{idOrName}/masterlogs](https://containers.cloud.ibm.com/global/swagger-global-api/#/log45collector/GetMasterLogCollectionStatus)</td>
</tr>
<tr>
<td>Crie uma configuração de encaminhamento de log para a origem de log <code>kube-audit</code>.</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td>Exclua uma configuração de encaminhamento de log para a origem de log <code>kube-audit</code>.</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfig)</code></td>
</tr>
</tbody>
</table>

<br />


## {{site.data.keyword.Bluemix_notm}}  Funções de serviço do IAM
{: #service}

Cada usuário que é designado a uma função de acesso de serviço do {{site.data.keyword.Bluemix_notm}} IAM também é designado automaticamente a uma função correspondente de controle de acesso baseado na função do Kubernetes (RBAC) em um namespace específico. Para saber mais sobre funções de acesso de serviço, consulte [Funções de serviço do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform). Não designe funções de plataforma do IAM do {{site.data.keyword.Bluemix_notm}} ao mesmo tempo que uma função de serviço. Deve-se designar funções de plataforma e de serviço separadamente.
{: shortdesc}

Procurando quais ações do Kubernetes cada função de serviço concede por meio do RBAC? Consulte [Permissões de recurso do Kubernetes por função RBAC](#rbac_ref). Para saber mais sobre as funções RBAC, consulte [Designando permissões RBAC](/docs/containers?topic=containers-users#role-binding) e [Ampliando as permissões existentes agregando funções de cluster](https://cloud.ibm.com/docs/containers?topic=containers-users#rbac_aggregate)
{: tip}

A tabela a seguir mostra as permissões de recurso do Kubernetes que são concedidas por cada função de serviço e sua função RBAC correspondente.

<table>
<caption>Permissões de recursos do Kubernetes por serviço e funções RBAC correspondentes</caption>
<thead>
    <th id="service-role">Função de serviço</th>
    <th id="rbac-role">Função RBAC Correspondente, Ligação e Escopo</th>
    <th id="kube-perm">Permissões de recursos do Kubernetes</th>
</thead>
<tbody>
  <tr>
    <td id="service-role-reader" headers="service-role">Função de Leitor</td>
    <td headers="service-role-reader rbac-role">Quando o escopo estiver definido para um namespace: função de cluster <strong><code>view</code></strong> aplicada pela ligação de função <strong><code>ibm-view</code></strong> nesse namespace</br><br>Quando tiver o escopo definido para todos os namespaces: função de cluster <strong><code>view</code></strong> aplicada pela ligação de função <strong><code>ibm-view</code></strong> em cada namespace do cluster</td>
    <td headers="service-role-reader kube-perm"><ul>
      <li>Acesso de leitura para recursos em um namespace</li>
      <li>Nenhum acesso de leitura a funções e ligações de função ou a segredos do Kubernetes</li>
      <li>Acessar o painel do Kubernetes para visualizar recursos em um namespace</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-writer" headers="service-role">Função de gravador</td>
    <td headers="service-role-writer rbac-role">Quando o escopo estiver definido para um namespace: função de cluster <strong><code>edit</code></strong> aplicada pela ligação de função <strong><code>ibm-edit</code></strong> nesse namespace</br><br>Quando tiver o escopo definido para todos os namespaces: função de cluster <strong><code>edit</code></strong> aplicada pela ligação de função <strong><code>ibm-edit</code></strong> em cada namespace do cluster</td>
    <td headers="service-role-writer kube-perm"><ul><li>Acesso de leitura / gravação para recursos em um namespace</li>
    <li>Nenhum acesso de leitura/gravação para funções e ligações de função</li>
    <li>Acessar o painel do Kubernetes para visualizar recursos em um namespace</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-manager" headers="service-role">função de gerenciador</td>
    <td headers="service-role-manager rbac-role">Quando o escopo estiver definido para um namespace: função de cluster <strong><code>admin</code></strong> aplicada pela ligação de função <strong><code>ibm-operate</code></strong> nesse namespace</br><br>Quando tiver o escopo definido para todos os namespaces: função de cluster <strong><code>cluster-admin</code></strong> aplicada pela ligação de função de cluster <strong><code>ibm-admin</code></strong></td> que se aplica a todos os namespaces
    <td headers="service-role-manager kube-perm">Quando com escopo definido para um namespace:
      <ul><li>Acesso de leitura/gravação para todos os recursos em um namespace, mas não para a cota de recurso ou para o próprio namespace</li>
      <li>Crie funções RBAC e ligações de função em um namespace</li>
      <li>Acesse o painel do Kubernetes para visualizar todos os recursos em um namespace</li></ul>
    </br>Quando o escopo estiver definido para todos os namespaces:
        <ul><li>Acesso de leitura / gravação a todos os recursos em cada espaço de nomes</li>
        <li>Criar funções RBAC e ligações de função em um namespace ou funções de cluster e ligações de função de cluster em todos os namespaces</li>
        <li>Acessar o painel do Kubernetes</li>
        <li>Criar um recurso Ingress que disponibiliza apps publicamente</li>
        <li>Revise as métricas de cluster, tais como com os comandos <code>kubectl top pods</code>, <code>kubectl top nodes</code> ou <code>kubectl get nodes</code></li></ul>
    </td>
  </tr>
</tbody>
</table>

<br />


## Permissões de recurso do Kubernetes por função RBAC
{: #rbac_ref}

Cada usuário que recebe uma função de acesso de serviço do {{site.data.keyword.Bluemix_notm}} IAM também recebe automaticamente uma função de controle de acesso baseada em função (RBAC) do Kubernetes predefinida correspondente. Se você planeja gerenciar suas próprias funções RBAC customizadas do Kubernetes, consulte [Criando permissões customizadas de RBAC para usuários, grupos ou contas de serviço](/docs/containers?topic=containers-users#rbac).
{: shortdesc}

Você está pensando se tem as permissões corretas para executar um determinado comando `kubectl` em um recurso em um namespace? Tente o [comando `kubectl auth can-i` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-can-i-em-).
{: tip}

A tabela a seguir mostra as permissões que são concedidas por cada função RBAC para recursos individuais do Kubernetes. As permissões são mostradas como verbos que um usuário com essa função pode concluir com relação ao recurso, como "obter", "listar", "descrever", "criar" ou "excluir".

<table>
 <caption>Permissões de recurso do Kubernetes concedidas por cada função RBAC predefinida</caption>
 <thead>
  <th>Recurso do Kubernetes</th>
  <th><code> visualização </code></th>
  <th><code> editar </code></th>
  <th><code> admin </code>  e  <code> cluster-admin </code></th>
 </thead>
<tbody>
<tr>
  <td><code>Ligações</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
</tr><tr>
  <td><code>configmaps</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>cronjobs.batch</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.apps </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.extensions</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/rollback</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/scale</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/rollback</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/scale</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>nós de extremidade</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>events</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
</tr><tr>
  <td><code>horizontalpodautoscalers.autoscaling</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>ingresses.extensions</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>jobs.batch</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>em tranges</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
</tr><tr>
  <td><code>localsubjectaccessrevisões</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code></td>
</tr><tr>
  <td><code>namespaces</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></br>**cluster-admin only:** <code>create</code>, <code>delete</code></td>
</tr><tr>
  <td><code>namespaces / status</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
</tr><tr>
  <td><code>networkpolicies</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>networkpolicies.extensions</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>persistentvolumeclaims</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>poddisruptionbudgets.policy</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>top</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods / attach</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods / exec</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods / log</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
</tr><tr>
  <td><code>pods / portforward</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods / proxy</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods / status</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
</tr><tr>
  <td><code>replicasets.apps</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.apps/scale</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions/scale</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers / scale</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers / status</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
</tr><tr>
  <td><code>replicationcontrollers.extensions/scale</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>resourcequotas</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
</tr><tr>
  <td><code>resourcequotas / status</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
</tr><tr>
  <td><code>Roleligações</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>funções</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>segredos</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>serviceaccounts</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code>, <code>impersonate</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code>, <code>impersonate</code></td>
</tr><tr>
  <td><code>remotos</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>serviços/proxy</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps/scale</code></td>
  <td><code> get </code>,  <code> list </code>,  <code> watch </code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr>
</tbody>
</table>

<br />


## Funções do Cloud Foundry
{: #cloud-foundry}

As funções do Cloud Foundry concedem acesso a organizações e espaços dentro da conta. Para ver a lista de serviços baseados no Cloud Foundry no {{site.data.keyword.Bluemix_notm}}, execute `ibmcloud service list`. Para saber mais, veja todas as [funções de organização e espaço](/docs/iam?topic=iam-cfaccess) disponíveis ou as etapas para [gerenciar o acesso do Cloud Foundry](/docs/iam?topic=iam-mngcf) na documentação do {{site.data.keyword.Bluemix_notm}} IAM.
{: shortdesc}

A tabela a seguir mostra as funções do Cloud Foundry que são necessárias para permissões de ação do cluster.

<table>
  <caption>Permissões de gerenciamento de cluster por função do Cloud Foundry</caption>
  <thead>
    <th>Função do Cloud Foundry</th>
    <th>Permissões de gerenciamento de cluster</th>
  </thead>
  <tbody>
  <tr>
    <td>Função de espaço: gerenciador</td>
    <td>Gerenciar acesso de usuário a um espaço do  {{site.data.keyword.Bluemix_notm}}</td>
  </tr>
  <tr>
    <td>Função de espaço: desenvolvedor</td>
    <td>
      <ul><li>Criar instâncias de serviço do
{{site.data.keyword.Bluemix_notm}}</li>
      <li>Ligar instâncias de serviço do {{site.data.keyword.Bluemix_notm}} a clusters</li>
      <li>Visualizar logs da configuração de encaminhamento de logs de um cluster no nível de espaço</li></ul>
    </td>
  </tr>
  </tbody>
</table>

## Funções de infraestrutura
{: #infra}

Um usuário com a função de acesso à infraestrutura **Superusuário** [configura a chave de API para uma região e um grupo de recursos](/docs/containers?topic=containers-users#api_key) para que as ações de infraestrutura possam ser executadas (ou mais raramente, [configura manualmente diferentes credenciais de conta](/docs/containers?topic=containers-users#credentials)). Em seguida, as ações de infraestrutura que outros usuários na conta podem executar são autorizadas por meio de funções da plataforma do {{site.data.keyword.Bluemix_notm}} IAM. Não é necessário editar as permissões de infraestrutura do IBM Cloud (SoftLayer) dos outros usuários. Use a tabela a seguir para customizar as permissões de infraestrutura do IBM Cloud (SoftLayer) dos usuários somente quando não é possível designar **Superusuário** ao usuário que configura a chave de API. Para obter instruções para designar permissões, consulte [Customizando permissões de infraestrutura](/docs/containers?topic=containers-users#infra_access).
{: shortdesc}



A tabela a seguir mostra as permissões de infraestrutura que são necessárias para concluir grupos de tarefas comuns.

<table>
<caption>Permissões de infraestrutura normalmente necessárias para o {{site.data.keyword.containerlong_notm}}</caption>
<thead>
  <th>Tarefas comuns no {{site.data.keyword.containerlong_notm}}</th>
  <th>Permissões de infraestrutura necessárias por categoria</th>
</thead>
<tbody>
<tr>
<td>
  <strong>Permissões mínimas</strong>: <ul>
  <li>Crie um cluster.</li></ul></td>
<td>
<strong>Conta</strong>: <ul>
<li>Incluir servidor</li></ul>
  <strong>Dispositivos</strong>:<ul>
  <li>Para nós do trabalhador bare metal: visualizar detalhes do hardware</li>
  <li>Gerenciamento Remoto do IPMI</li>
  <li>Recarregamentos do OS e Rescue Kernel</li>
  <li>Para nós do trabalhador da VM: visualizar detalhes do servidor virtual</li></ul></td>
</tr>
<tr>
<td>
<strong>Administração de cluster</strong>:<ul>
  <li>Criar, atualizar e excluir clusters.</li>
  <li>Incluir, recarregar e reinicializar nós do trabalhador.</li>
  <li>Visualizar VLANs.</li>
  <li>Criar sub-redes.</li>
  <li>Implementar pods e serviços do balanceador de carga.</li></ul>
  </td><td>
<strong>Conta</strong>:<ul>
  <li>Incluir servidor</li>
  <li>Cancelar servidor</li></ul>
<strong>Dispositivos</strong>:<ul>
  <li>Para nós do trabalhador bare metal: visualizar detalhes do hardware</li>
  <li>Gerenciamento Remoto do IPMI</li>
  <li>Recarregamentos do OS e Rescue Kernel</li>
  <li>Para nós do trabalhador da VM: visualizar detalhes do servidor virtual</li></ul>
<strong>Rede</strong>:<ul>
  <li>Incluir cálculo com porta de rede pública</li></ul>
<p class="important">Deve-se também designar ao usuário a capacidade de gerenciar casos de suporte. Consulte a etapa 8 de [Customizando permissões de infraestrutura](/docs/containers?topic=containers-users#infra_access).</p>
</td>
</tr>
<tr>
<td>
  <strong>Armazenamento</strong>: <ul>
  <li>Criar solicitações de volume persistente para provisionar volumes persistentes.</li>
  <li>Criar e gerenciar recursos de infraestrutura de armazenamento.</li></ul></td>
<td>
<strong>Conta</strong>:<ul>
  <li>Incluir / Fazer Upgrade de Armazenamento (StorageLayer)</li></ul>
<strong>Serviços</strong>:<ul>
  <li>Gerenciar de armazenamento</li></ul></td>
</tr>
<tr>
<td>
  <strong>Rede privada</strong>: <ul>
  <li>Gerenciar VLANs privadas para rede em cluster.</li>
  <li>Configurar a conectividade VPN para redes privadas.</li></ul></td>
<td>
  <strong>Rede</strong>:<ul>
  <li>Gerenciar rotas de sub-rede da rede</li></ul></td>
</tr>
<tr>
<td>
  <strong>Rede pública</strong>:<ul>
  <li>Configurar o balanceador de carga pública ou rede de Ingresso para expor apps.</li></ul></td>
<td>
<strong>Dispositivos</strong>:<ul>
<li>Gerenciar controle de porta</li>
  <li>Editar nome do host/domínio</li></ul>
<strong>Rede</strong>:<ul>
  <li>Incluir endereços IP</li>
  <li>Gerenciar rotas de sub-rede da rede</li>
  <li>Incluir cálculo com porta de rede pública</li></ul>
<strong>Serviços</strong>:<ul>
  <li>Gerenciar DNS</li>
  <li>Visualizar certificados (SSL)</li>
  <li>Gerenciar certificados (SSL)</li></ul></td>
</tr>
</tbody>
</table>
