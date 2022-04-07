---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-18"

keywords: kubernetes, iks, ibmcloud, ic, ks

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


# CLI do IBM Cloud Kubernetes Service
{: #cs_cli_reference}

Consulte estes comandos para criar e gerenciar clusters do Kubernetes no {{site.data.keyword.containerlong}}.
{:shortdesc}

Para instalar o plug-in da CLI, veja [Instalando a CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).

No terminal, você é notificado quando atualizações para a CLI `ibmcloud` e plug-ins estão disponíveis. Certifique-se de manter sua CLI atualizada para que seja possível usar todos os comandos e sinalizações disponíveis.

Procurando comandos `ibmcloud cr`? Consulte a [referência da CLI do {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-registry_cli_reference#registry_cli_reference). Procurando comandos `kubectl`? Consulte a [documentação de Kubernetes
![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubectl.docs.kubernetes.io/).
{:tip}


## Usando o plug-in beta do {{site.data.keyword.containerlong_notm}}
{: #cs_beta}

Uma versão reprojetada do plug-in do {{site.data.keyword.containerlong_notm}} está disponível como um beta. O plug-in do {{site.data.keyword.containerlong_notm}} reprojetado agrupa comandos em categorias e muda os comandos de uma estrutura hifenizada para uma estrutura espaçada.
{: shortdesc}

Para usar o plug-in do {{site.data.keyword.containerlong_notm}} reprojetado, configure a variável de ambiente `IKS_BETA_VERSION` para a versão beta que você deseja usar:

```
export IKS_BETA_VERSION= < beta_version>
```
{: pre}

As seguintes versões beta do plug-in do {{site.data.keyword.containerlong_notm}} reprojetado estão disponíveis. Observe que o comportamento padrão é `0.2`.

<table>
<caption>Versões Beta do plug-in {{site.data.keyword.containerlong_notm}} reprojetado</caption>
  <thead>
    <th>Versão Beta</th>
    <th>Estrutura de saída ` ibmcloud ks help `</th>
    <th>Estrutura de comando</th>
  </thead>
  <tbody>
    <tr>
      <td><code> 0.2 </code>  (padrão)</td>
      <td>Anterior: os comandos são mostrados na estrutura hifenizada e são listados alfabeticamente.</td>
      <td>Anterior e beta: é possível executar comandos na estrutura hifenizada anterior (`ibmcloud ks alb-cert-get`) ou na estrutura espaçada beta (`ibmcloud ks alb cert get`).</td>
    </tr>
    <tr>
      <td><code> 0,3 </code></td>
      <td>Beta: os comandos são mostrados na estrutura espaçada e são listados em categorias.</td>
      <td>Anterior e beta: é possível executar comandos na estrutura hifenizada anterior (`ibmcloud ks alb-cert-get`) ou na estrutura espaçada beta (`ibmcloud ks alb cert get`).</td>
    </tr>
    <tr>
      <td><code> 1.0 </code></td>
      <td>Beta: os comandos são mostrados na estrutura espaçada e são listados em categorias.</td>
      <td>Beta: é possível executar comandos apenas na estrutura espaçada beta (`ibmcloud ks alb cert get`).</td>
    </tr>
  </tbody>
</table>



## ibmcloud ks commands
{: #cs_commands}

**Dica:** para ver a versão do plug-in do {{site.data.keyword.containerlong_notm}}, execute o comando a seguir.

```
ibmcloud plugin list
```
{: pre}

<table summary="API commands table">
<caption>Comandos de API</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos de API</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks api
](#cs_cli_api)</td>
    <td>[ibmcloud ks api-key-info](#cs_api_key_info)</td>
    <td>[ibmcloud ks api-key-reset](#cs_api_key_reset)</td>
    <td>[ibmcloud ks apiserver-config-get](#cs_apiserver_config_get)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks apiserver-config-set](#cs_apiserver_config_set)</td>
    <td>[ibmcloud ks apiserver-config-unset](#cs_apiserver_config_unset)</td>
    <td>[ibmcloud ks apiserver-refresh](#cs_apiserver_refresh)</td>
    <td></td>
 </tr>
</tbody>
</table>

<br>

<table summary="CLI plug-in usage commands table">
<caption>Comandos de uso do plug-in da CLI</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos de uso do plug-in da CLI</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks help](#cs_help)</td>
    <td>[ibmcloud ks init](#cs_init)</td>
    <td>[ibmcloud ks messages](#cs_messages)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="Cluster commands: Management table">
<caption>Comandos do cluster: Gerenciamento de comandos</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos do cluster: gerenciamento</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks addon-versions](#cs_addon_versions)</td>
    <td>[ ibmcloud ks cluster-addon-disable ](#cs_cluster_addon_disable)</td>
    <td>[ ibmcloud ks cluster-addon-enable ](#cs_cluster_addon_enable)</td>
    <td>[ibmcloud ks cluster-addons](#cs_cluster_addons)</td>
  </tr>
  <tr>
  <td>[ibmcloud ks cluster-config](#cs_cluster_config)</td>
    <td>[ibmcloud ks cluster-create](#cs_cluster_create)</td>
    <td>[ ibmcloud ks cluster-feature-disable ](#cs_cluster_feature_disable)</td>
    <td>[ibmcloud ks cluster-feature-enable](#cs_cluster_feature_enable)</td>
  </tr>
  <tr>
  <td>[ibmcloud ks cluster-get](#cs_cluster_get)</td>
    <td>[ ibmcloud ks cluster-pull-secret-apply ](#cs_cluster_pull_secret_apply)</td>
    <td>[ibmcloud ks cluster-refresh](#cs_cluster_refresh)</td>
    <td>[ibmcloud ks cluster-rm](#cs_cluster_rm)</td>
  </tr>
  <tr>
  <td>[ibmcloud ks cluster-update](#cs_cluster_update)</td>
    <td>[ibmcloud ks clusters](#cs_clusters)</td>
    <td>[ibmcloud ks kube-versions](#cs_kube_versions)</td>
    <td> </td>
  </tr>
</tbody>
</table>

<br>

<table summary="Comandos de cluster: tabela de serviços e integrações">
<caption>Comandos do cluster: serviços e integrações comandos</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos do cluster: serviços e integrações</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[ibmcloud ks cluster-service-unbind](#cs_cluster_service_unbind)</td>
    <td>[ibmcloud ks cluster-services](#cs_cluster_services)</td>
    <td>[ibmcloud ks va](#cs_va)</td>
  </tr>
    <td>[ibmcloud ks webhook-create](#cs_webhook_create)</td>
  <tr>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabela Comandos de cluster: Subnets">
<caption>Comandos do cluster: Subnets comandos</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos do cluster: Subnets</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[ibmcloud ks cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[ibmcloud ks cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[ibmcloud ks cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks subnets](#cs_subnets)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

</br>

<table summary="Infrastructure commands table">
<caption>Comandos do cluster: Comandos de infraestrutura</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos de infraestrutura</th>
 </thead>
 <tbody>
  <tr>
    <td>[        ibmcloud ks credential-get
        ](#cs_credential_get)</td>
    <td>[ibmcloud ks credential-set](#cs_credentials_set)</td>
    <td>[ibmcloud ks credential-unset](#cs_credentials_unset)</td>
    <td>[ibmcloud ks machine-types](#cs_machine_types)</td>
  </tr>
  <tr>
    <td>[ibmcloud ks vlans](#cs_vlans)</td>
    <td>[ ibmcloud ks vlan-spanning-get ](#cs_vlan_spanning_get)</td>
    <td> </td>
    <td> </td>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabela de comandos do balanceador de carga do aplicativo (ALB) de Ingresso">
<caption>Comandos do balanceador de carga do aplicativo (ALB) Ingress</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Comandos do balanceador de carga do aplicativo (ALB) Ingress</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[ibmcloud ks alb-autoupdate-disable](#cs_alb_autoupdate_disable)</td>
      <td>[ibmcloud ks alb-autoupdate-enable](#cs_alb_autoupdate_enable)</td>
      <td>[ibmcloud ks alb-autoupdate-get](#cs_alb_autoupdate_get)</td>
      <td>[ibmcloud ks alb-cert-deploy](#cs_alb_cert_deploy)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks alb-cert-get](#cs_alb_cert_get)</td>
      <td>[ibmcloud ks alb-cert-rm](#cs_alb_cert_rm)</td>
      <td>[ibmcloud ks alb-certs](#cs_alb_certs)</td>
      <td>[ibmcloud ks alb-configure](#cs_alb_configure)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks alb-get](#cs_alb_get)</td>
      <td>[ibmcloud ks alb-rollback](#cs_alb_rollback)</td>
      <td>[ibmcloud ks alb-types](#cs_alb_types)</td>
      <td>[ibmcloud ks alb-update](#cs_alb_update)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks albs](#cs_albs)</td>
      <td> </td>
      <td> </td>
      <td> </td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tabela de comandos de criação de log">
<caption>Comandos de criação de log</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Comandos de criação de log</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[ ks ibmcloud ks logging-autoupdate-enable ](#cs_log_autoupdate_enable)</td>
      <td>[ibmcloud ks logging-autoupdate-disable](#cs_log_autoupdate_disable)</td>
      <td>[ibmcloud ks logging-autoupdate-get](#cs_log_autoupdate_get)</td>
      <td>[ibmcloud ks logging-config-create](#cs_logging_create)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks logging-config-get](#cs_logging_get)</td>
      <td>[ibmcloud ks logging-config-refresh](#cs_logging_refresh)</td>
      <td>[ibmcloud ks logging-config-rm](#cs_logging_rm)</td>
      <td>[ibmcloud ks logging-config-update](#cs_logging_update)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks logging-filter-create](#cs_log_filter_create)</td>
      <td>[ibmcloud ks logging-filter-update](#cs_log_filter_update)</td>
      <td>[ibmcloud ks logging-filter-get](#cs_log_filter_view)</td>
      <td>[ibmcloud ks logging-filter-rm](#cs_log_filter_delete)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks logging-collect](#cs_log_collect)</td>
      <td>[ibmcloud ks logging-collect-status](#cs_log_collect_status)</td>
      <td> </td>
      <td> </td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tabela de comandos do balanceador de carga de rede (NLB)">
<caption>Comandos do balanceador de carga de rede (NLB)</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Comandos do balanceador de carga de rede (NLB)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[ibmcloud ks nlb-dns-add](#cs_nlb-dns-add)</td>
      <td>[ibmcloud ks nlb-dns-create](#cs_nlb-dns-create)</td>
      <td>[ibmcloud ks nlb-dnss](#cs_nlb-dns-ls)</td>
      <td>[ibmcloud ks nlb-dns-rm](#cs_nlb-dns-rm)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks nlb-dns-monitor-configure](#cs_nlb-dns-monitor-configure)</td>
      <td>[ibmcloud ks nlb-dns-monitor-get](#cs_nlb-dns-monitor-get)</td>
      <td>[ibmcloud ks nlb-dns-monitor-disable](#cs_nlb-dns-monitor-disable)</td>
      <td>[ibmcloud ks nlb-dns-monitor-enable](#cs_nlb-dns-monitor-enable)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks nlb-dns-monitor-ls](#cs_nlb-dns-monitor-ls)</td>
      <td>[ibmcloud ks nlb-dns-monitor-status](#cs_nlb-dns-monitor-status)</td>
      <td> </td>
      <td> </td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tabela Comandos de região">
<caption>Comandos de região</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos de região</th>
 </thead>
 <tbody>
  <tr>
    <td>[ibmcloud ks region](#cs_region)</td>
    <td>[ibmcloud ks region-set](#cs_region-set)</td>
    <td>[ibmcloud ks regions](#cs_regions)</td>
  <td>[ibmcloud ks zones](#cs_datacenters)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabela Comandos de nó do trabalhador">
<caption>Comandos de nó do trabalhador</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos de nó do trabalhador</th>
 </thead>
 <tbody>
    <tr>
      <td>Descontinuado: [ibmcloud ks worker-add](#cs_worker_add)</td>
      <td>[ibmcloud ks worker-get](#cs_worker_get)</td>
      <td>[ibmcloud ks worker-reboot](#cs_worker_reboot)</td>
      <td>[ibmcloud ks worker-reload](#cs_worker_reload)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks worker-rm](#cs_worker_rm)</td>
      <td>[ibmcloud ks worker-update](#cs_worker_update)</td>
      <td>[ibmcloud ks workers](#cs_workers)</td>
      <td> </td>
    </tr>
  </tbody>
</table>

<table summary="Tabela Comandos do conjunto do trabalhador">
<caption>Comandos do conjunto do trabalhador</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos do conjunto do trabalhador</th>
 </thead>
 <tbody>
    <tr>
      <td>[ibmcloud ks worker-pool-create](#cs_worker_pool_create)</td>
      <td>[ibmcloud ks worker-pool-get](#cs_worker_pool_get)</td>
      <td>[ibmcloud ks worker-pool-rebalance](#cs_rebalance)</td>
      <td>[ibmcloud ks worker-pool-resize](#cs_worker_pool_resize)</td>
    </tr>
    <tr>
      <td>[ibmcloud ks worker-pool-rm](#cs_worker_pool_rm)</td>
      <td>[ibmcloud ks worker-pools](#cs_worker_pools)</td>
      <td>[ibmcloud ks zone-add](#cs_zone_add)</td>
      <td>[ibmcloud ks zone-network-set](#cs_zone_network_set)</td>
    </tr>
    <tr>
     <td>[ibmcloud ks zone-rm](#cs_zone_rm)</td>
     <td></td>
    </tr>
  </tbody>
</table>

## Comandos de API
{: #api_commands}

### ibmcloud ks api
{: #cs_cli_api}

Destine o terminal da API para o {{site.data.keyword.containerlong_notm}}. Se você não especificar um terminal, será possível visualizar informações sobre o terminal atual que está destinado.
{: shortdesc}

Os terminais a seguir são suportados:
* Global: `https://containers.cloud.ibm.com`
* Dallas (Sul dos EUA, us-south): `https://us-south.containers.cloud.ibm.com`
* Frankfurt (Central da UE, eu-de): `https://eu-central.containers.cloud.ibm.com`
* Londres (Sul do Reino Unido, eu-gb): `https://uk-south.containers.cloud.ibm.com`
* Sydney (AP Sul, au-syd): `https://ap-south.containers.cloud.ibm.com`
* Tokyo (AP Norte, jp-tok): `https://ap-north.containers.cloud.ibm.com`
* Washington, D.C. (Leste dos EUA, us-east): `https://us-east.containers.cloud.ibm.com`

```
ibmcloud ks api --endpoint ENDPOINT [--insecure] [--skip-ssl-validation] [--api-version VALUE] [-s]
```
{: pre}

<strong> Permissões mínimas necessárias </strong>: nenhuma

<strong>Opções de comando</strong>:

   <dl>
   <dt><code> -- endpoint  <em> ENDPOINT </em> </code></dt>
   <dd>O terminal da API do  {{site.data.keyword.containerlong_notm}} . Observe que esse terminal é diferente dos terminais do {{site.data.keyword.Bluemix_notm}}. Este valor é necessário para configurar o terminal de API.
   </dd>

   <dt><code>-- inseguro</code></dt>
   <dd>Permitir uma conexão HTTP insegura. Este sinalizador é opcional.</dd>

   <dt><code>--validation-skip-ssl</code></dt>
   <dd>Permitir certificados SSL insegura. Este sinalizador é opcional.</dd>

   <dt><code>-- api-version VALUE</code></dt>
   <dd>Especifique a versão da API do serviço que você deseja usar. Esse valor é opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplo**: visualize informações sobre o terminal de API atual que é destinado.
```
ibmcloud ks api
```
{: pre}

```
API Endpoint:          https://containers.cloud.ibm.com
API Version:           v1
Skip SSL Validation:   false
Region:                us-south
```
{: screen}


### ibmcloud ks api-key-info
{: #cs_api_key_info}

Visualize o nome e o endereço de e-mail do proprietário da chave de API do {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) em um grupo de recursos e região do {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks api-key-info --cluster CLUSTER [--json] [-s]
```
{: pre}

A chave de API do {{site.data.keyword.Bluemix_notm}} é configurada automaticamente para um grupo de recursos e região quando a primeira ação que requer a política de acesso de administrador do {{site.data.keyword.containerlong_notm}} é executada. Por exemplo, um de seus usuários administradores cria o primeiro cluster no grupo de recursos `default` na região `us-south`. Ao fazer isso, a chave de API do {{site.data.keyword.Bluemix_notm}} IAM para esse usuário é armazenada na conta para esse grupo de recursos e região. A chave API é usada para pedir recursos na infraestrutura do IBM Cloud (SoftLayer), como nós do trabalhador novo ou VLANs. Uma chave API diferente pode ser configurada para cada região dentro de um grupo de recursos.

Quando um usuário diferente executa uma ação nesse grupo de recursos e região que requer interação com o portfólio de infraestrutura do IBM Cloud (SoftLayer), como a criação de um novo cluster ou o recarregamento de um nó do trabalhador, a chave API armazenada é usada para determinar se existem permissões suficientes para executar essa ação. Para certificar-se de que as ações relacionadas à infraestrutura em seu cluster possam ser executadas com sucesso, designe a seus usuários administradores do {{site.data.keyword.containerlong_notm}} a política de acesso de infraestrutura **Superusuário**. Para obter mais informações, veja [Gerenciando o acesso de usuário](/docs/containers?topic=containers-users#infra_access).

Se você achar que é necessário atualizar a chave API que está armazenada para um grupo de recursos e região, é possível fazer isso executando o comando [ibmcloud ks api-key-reset](#cs_api_key_reset). Esse comando requer a política de acesso de administrador do {{site.data.keyword.containerlong_notm}} e armazena a chave API do usuário que executa esse comando na conta.

**Dica:** a chave de API retornada nesse comando poderá não ser usada se as credenciais de infraestrutura do IBM Cloud (SoftLayer) tiverem sido configuradas manualmente usando o comando [ibmcloud ks credential-set](#cs_credentials_set).

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplo**:

  ```
  ibmcloud ks api-key-info -- cluster my_cluster
  ```
  {: pre}


### ibmcloud ks api-key-reset
{: #cs_api_key_reset}

Substitua a chave de API atual do {{site.data.keyword.Bluemix_notm}} IAM em uma região do {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks api-key-reset [-s]
```
{: pre}

Esse comando requer a política de acesso de administrador do {{site.data.keyword.containerlong_notm}} e armazena a chave API do usuário que executa esse comando na conta. A chave de API do {{site.data.keyword.Bluemix_notm}} IAM é necessária para pedir a infraestrutura do portfólio de infraestrutura do IBM Cloud (SoftLayer). Quando armazenada, a chave API é usada para cada ação em uma região que requer permissões de infraestrutura independentemente do usuário que executa esse comando. Para obter mais informações sobre como as chaves API do {{site.data.keyword.Bluemix_notm}} IAM funcionam, consulte o [comando `ibmcloud ks api-key-info`](#cs_api_key_info).

Antes de usar esse comando, certifique-se de que o usuário que executa esse comando tenha as permissões do [{{site.data.keyword.containerlong_notm}} e da infraestrutura do IBM Cloud (SoftLayer) necessárias](/docs/containers?topic=containers-users#users).
{: important}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>


**Exemplo**:

  ```
  ibmcloud ks api-key-reset
  ```
  {: pre}



### ibmcloud ks apiserver-config-get
{: #cs_apiserver_config_get}

Obtenha informações sobre uma opção para a configuração do servidor de API do Kubernetes de um cluster. Esse comando deve ser combinado com um dos subcomandos a seguir para a opção de configuração sobre a qual você deseja informações.
{: shortdesc}

#### ibmcloud ks apiserver-config-get audit-webhook
{: #cs_apiserver_config_get_audit_webhook}

Visualize a URL para o serviço de criação de log remoto para o qual você está enviando logs de auditoria do servidor de API. A URL foi especificada quando você criou o back-end do webhook para a configuração do servidor de API.
{: shortdesc}

```
ibmcloud ks apiserver-config-get audit-webhook -- cluster CLUSTER
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks apiserver-config-get audit-webhook -- cluster my_cluster
  ```
  {: pre}


### ibmcloud ks apiserver-config-set
{: #cs_apiserver_config_set}

Defina uma opção para a configuração do servidor da API do Kubernetes de um cluster. Esse comando deve ser combinado com um dos seguintes subcomandos para a opção de configuração que você deseja definir.
{: shortdesc}

#### ibmcloud ks apiserver-config-set audit-webhook
{: #cs_apiserver_set}

Configure o backend do webhook para a configuração do servidor de API. O back-end do webhook encaminha os logs de auditoria do servidor de API para um servidor remoto. Uma configuração de webhook é criada com base nas informações fornecidas nas sinalizações desse comando. Se você não fornecer nenhuma informação nas sinalizações, uma configuração de webhook padrão será usada.
{: shortdesc}

```
ibmcloud ks apiserver-config-set audit-webhook --cluster CLUSTER [--remoteServer SERVER_URL_OR_IP] [--caCert CA_CERT_PATH] [--clientCert CLIENT_CERT_PATH] [--clientKey CLIENT_KEY_PATH]
```
{: pre}

Depois de configurar o webhook, deve-se executar o comando `ibmcloud ks apiserver-refresh` para aplicar as mudanças no mestre do Kubernetes.
{: note}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--remoteServer <em>SERVER_URL</em></code></dt>
   <dd>A URL ou o endereço IP para o serviço de criação de log remoto para o qual deseja enviar logs de auditoria. Se você fornecer uma URL insegura do servidor, todos os certificados serão ignorados. Esse valor é opcional.</dd>

   <dt><code>--caCert <em>CA_CERT_PATH</em></code></dt>
   <dd>O caminho de arquivo para o certificado de CA que é usado para verificar o serviço de criação de log remoto. Esse valor é opcional.</dd>

   <dt><code>--clientCert <em>CLIENT_CERT_PATH</em></code></dt>
   <dd>O caminho de arquivo para o certificado de cliente que é usado para autenticar com relação ao serviço de criação de log remoto. Esse valor é opcional.</dd>

   <dt><code>--clientKey <em> CLIENT_KEY_PATH</em></code></dt>
   <dd>O caminho de arquivo para a chave do cliente correspondente que é usada para se conectar ao serviço de criação de log remoto. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks apiserver-config-set audit-webhook --cluster my_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver-audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver-audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver-audit/key.pem
  ```
  {: pre}


### ibmcloud ks apiserver-config-unset
{: #cs_apiserver_config_unset}

Desative uma opção para a configuração do servidor de API do Kubernetes de um cluster. Esse comando deve ser combinado com um dos subcomandos a seguir para a opção de configuração que você deseja desconfigurar.
{: shortdesc}

#### ibmcloud ks apiserver-config-unset audit-webhook
{: #cs_apiserver_unset}

Desative a configuração de back-end do webhook para o servidor de API do cluster. Desativando o back-end do webhook para o encaminhamento de logs de auditoria do servidor de API para um servidor remoto.
{: shortdesc}

```
ibmcloud ks apiserver-config-unset audit-webhook -- cluster CLUSTER
```
{: pre}

Depois de desativar o webhook, deve-se executar o comando `ibmcloud ks apiserver-refresh` para aplicar as mudanças no mestre do Kubernetes.
{: note}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks apiserver-config-unset audit-webhook -- cluster my_cluster
  ```
  {: pre}

### ibmcloud ks apiserver-refresh
{: #cs_apiserver_refresh}

Aplique as mudanças de configuração para o mestre do Kubernetes que são solicitadas com os comandos `ibmcloud ks apiserver-config-set`, `apiserver-config-unset`, `cluster-feature-enable` ou `cluster-feature-disable`. Se uma mudança na configuração exigir uma reinicialização, o componente mestre do Kubernetes afetado será reiniciado. Se as mudanças na configuração puderem ser aplicadas sem uma reinicialização, nenhum componente mestre do Kubernetes será reiniciado. Os nós do trabalhador, os apps e os recursos não são modificados e continuam a ser executados.
{: shortdesc}

```
ibmcloud ks apiserver-refresh -- cluster CLUSTER [ -s ]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplo**:

  ```
  ibmcloud ks apiserver-refresh -- cluster my_cluster
  ```
  {: pre}


<br />


## Comandos de uso do plug-in da CLI
{: #cli_plug-in_commands}

### ibmcloud ks help
{: #cs_help}

Visualizar uma lista de comandos e parâmetros suportados.
{: shortdesc}

```
ibmcloud ks help
```
{: pre}

<strong> Permissões mínimas necessárias </strong>: nenhuma

<strong> Opções de comando </strong>: Nenhuma


### ibmcloud ks init
{: #cs_init}

Inicialize o plug-in do {{site.data.keyword.containerlong_notm}} ou especifique a região em que você deseja criar ou acessar clusters do Kubernetes.
{: shortdesc}

Os terminais a seguir são suportados:
* Global: `https://containers.cloud.ibm.com`
* Dallas (Sul dos EUA, us-south): `https://us-south.containers.cloud.ibm.com`
* Frankfurt (Central da UE, eu-de): `https://eu-central.containers.cloud.ibm.com`
* Londres (Sul do Reino Unido, eu-gb): `https://uk-south.containers.cloud.ibm.com`
* Sydney (AP Sul, au-syd): `https://ap-south.containers.cloud.ibm.com`
* Tokyo (AP Norte, jp-tok): `https://ap-north.containers.cloud.ibm.com`
* Washington, D.C. (Leste dos EUA, us-east): `https://us-east.containers.cloud.ibm.com`

```
ibmcloud ks init [--host HOST] [--insecure] [-p] [-u] [-s]
```
{: pre}

<strong> Permissões mínimas necessárias </strong>: nenhuma

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--host <em>HOST</em></code></dt>
   <dd>O  {{site.data.keyword.containerlong_notm}}  terminal de API a ser usado. Esse valor é opcional. [Visualize os valores de terminal de API disponíveis.](/docs/containers?topic=containers-regions-and-zones#container_regions)</dd>

   <dt><code>-- inseguro</code></dt>
   <dd>Permitir uma conexão HTTP insegura.</dd>

   <dt><code>-p</code></dt>
   <dd>Sua senha do IBM Cloud.</dd>

   <dt><code>-u</code></dt>
   <dd>O IBM Cloud username.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplos**:
*  Exemplo para que o terminal regional do Sul dos EUA seja o destino:
  ```
  ibmcloud ks init --host https://us-south.containers.cloud.ibm.com
  ```
  {: pre}
*  Exemplo para que o terminal global seja o destino:
  ```
  ibmcloud ks init --host https://containers.cloud.ibm.com
  ```
  {: pre}

### ibmcloud ks messages
{: #cs_messages}

Visualize as mensagens atuais para o usuário do IBMid.
{: shortdesc}

```
ibmcloud ks messages
```
{: pre}

<strong> Permissões mínimas necessárias </strong>: nenhuma

<br />


## Comandos do cluster: gerenciamento
{: #cluster_mgmt_commands}

### ibmcloud ks addon-versions
{: #cs_addon_versions}

Visualize uma lista de versões suportadas para complementos gerenciados no {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks addon-versions [--addon ADD-ON_NAME] [--json] [-s]
```
{: pre}

<strong> Permissões mínimas necessárias </strong>: nenhuma

**Opções de comando**:

  <dl>
  <dt><code>--addon <em>ADD-ON_NAME</em></code></dt>
  <dd>Opcional: especifique um nome de complemento, como <code>istio</code> ou <code>knative</code>, para o qual filtrar versões.</dd>

  <dt><code>--json</code></dt>
  <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
  </dl>

**Exemplo**:

  ```
  ibmcloud ks addon-versions --addon istio
  ```
  {: pre}

### ibmcloud ks cluster-addon-disable
{: #cs_cluster_addon_disable}

Desativar um complemento gerenciado em um cluster existente. Esse comando deve ser combinado com um dos subcomandos a seguir para o complemento gerenciado que você deseja desativar.
{: shortdesc}

#### ibmcloud ks cluster-addon-disable <ph class="ignoreSpelling">istio</ph>
{: #cs_cluster_addon_disable_istio}

Desativar o complemento Istio gerenciado. Remove todos os componentes principais do Istio do cluster, incluindo o Prometheus.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable istio --cluster CLUSTER [-f]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:
<dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
   <dt><code>-f</code></dt>
   <dd>Opcional: esse complemento é uma dependência para os complementos gerenciados <code>istio-extras</code>, <code>istio-sample-bookinfo</code> e <code>knative</code>. Inclua esse sinalizador para também desativar esses complementos.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-addon-disable istio -- cluster my_cluster
  ```
  {: pre}

#### ibmcloud ks cluster-addon-disable istio-extras
{: #cs_cluster_addon_disable_istio_extras}

Desative o Istio extras add-on gerenciado. Remove o Grafana, o Jeager e o Kiali do cluster.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable istio-extras -- cluster CLUSTER [ -f ]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>-f</code></dt>
   <dd>Opcional: este complemento é uma dependência para o complemento gerenciado <code>istio-sample-bookinfo</code>. Inclua essa sinalização para também desativar esse complemento.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-addon-disable istio-extras -- cluster my_cluster
  ```
  {: pre}

#### ibmcloud ks cluster-addon-disable istio-sample-bookinfo
{: #cs_cluster_addon_disable_istio_sample_bookinfo}

Desative o complemento do Istio BookInfo gerenciado. Remove do cluster todas as implementações, os pods e outros recursos do aplicativo BookInfo.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable istio-sample-bookinfo -- cluster CLUSTER
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-addon-disable istio-sample-bookinfo -- cluster my_cluster
  ```
  {: pre}

#### ibmcloud ks cluster-addon-disable <ph class="ignoreSpelling">knative</ph>
{: #cs_cluster_addon_disable_knative}

Desative o complemento Knative gerenciado para remover a estrutura Knative sem servidor do cluster.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable knative -- cluster CLUSTER
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks cluster-addon-disable knative -- cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-disable kube-terminal
{: #cs_cluster_addon_disable_kube-terminal}

Desative o complemento do [Terminal do Kubernetes](/docs/containers?topic=containers-cs_cli_install#cli_web). Para usar o Terminal do Kubernetes no console do cluster do {{site.data.keyword.containerlong_notm}}, deve-se primeiro ativar o complemento novamente.
{: shortdesc}

```
ibmcloud ks cluster-addon-disable kube-terminal --cluster CLUSTER
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
</dl>

**Exemplo**:

```
ibmcloud ks cluster-addon-disable kube-terminal --cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-addon-enable
{: #cs_cluster_addon_enable}

Ative um complemento gerenciado em um cluster existente. Esse comando deve ser combinado com um dos subcomandos a seguir para o complemento gerenciado que você deseja ativar.
{: shortdesc}

#### ibmcloud ks cluster-addon-enable <ph class="ignoreSpelling">istio</ph>
{: #cs_cluster_addon_enable_istio}

Ative o  [ Complemento Istio ](/docs/containers?topic=containers-istio) gerenciado. Instala os componentes principais do Istio, incluindo o Prometheus.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable istio --cluster CLUSTER [--version VERSION]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Opcional: especifique a versão do complemento a ser instalado. Se nenhuma versão for especificada, a versão padrão será instalada.</dd>
</dl>

**Exemplo**:

```
ibmcloud ks cluster-addon-enable istio -- cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-enable istio-extras
{: #cs_cluster_addon_enable_istio_extras}

Ative o Istio extras extras add-on. Instala o Grafana, o Jeager e o Kiali para fornecer monitoramento, rastreamento e visualização extras para o Istio.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable istio-extras --cluster CLUSTER [--version VERSION] [-y]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Opcional: especifique a versão do complemento a ser instalado. Se nenhuma versão for especificada, a versão padrão será instalada.</dd>

<dt><code> -y </code></dt>
<dd>Opcional: ative a dependência de complementos  <code> istio </code> .</dd>
</dl>

**Exemplo**:
```
ibmcloud ks cluster-addon-enable istio-extras -- cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-enable istio-sample-bookinfo
{: #cs_cluster_addon_enable_istio_sample_bookinfo}

Ative o complemento do Istio BookInfo gerenciado. Implementa o [aplicativo de amostra BookInfo para Istio ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://istio.io/docs/examples/bookinfo/) no namespace <code>padrão</code>.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster CLUSTER [--version VERSION] [-y]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Opcional: especifique a versão do complemento a ser instalado. Se nenhuma versão for especificada, a versão padrão será instalada.</dd>

<dt><code> -y </code></dt>
<dd>Opcional: ative as dependências de complemento <code>istio</code> e <code>istio-extras</code>.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks cluster-addon-enable istio-sample-bookinfo -- cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-enable <ph class="ignoreSpelling">knative</ph>
{: #cs_cluster_addon_enable_knative}

Ative o [Complemento Knative](/docs/containers?topic=containers-knative_tutorial) gerenciado para instalar a estrutura Knative sem servidor.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable knative --cluster CLUSTER [--version VERSION] [-y]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Opcional: especifique a versão do complemento a ser instalado. Se nenhuma versão for especificada, a versão padrão será instalada.</dd>

<dt><code> -y </code></dt>
<dd>Opcional: ative a dependência de complementos  <code> istio </code> .</dd>
</dl>

**Exemplo**:
```
ibmcloud ks cluster-addon-enable knative -- cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-addon-enable kube-terminal
{: #cs_cluster_addon_enable_kube-terminal}

Ative o complemento do [Terminal do Kubernetes](/docs/containers?topic=containers-cs_cli_install#cli_web) para usá-lo no console do cluster do {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks cluster-addon-enable kube-terminal --cluster CLUSTER [--version VERSION]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Opcional: especifique a versão do complemento a ser instalado. Se nenhuma versão for especificada, a versão padrão será instalada.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks cluster-addon-enable kube-terminal --cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-addons
{: #cs_cluster_addons}

Listar complementos gerenciados que são ativados em um cluster.
{: shortdesc}

```
ibmcloud ks cluster-addons -- cluster CLUSTER
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:
<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks cluster-addons -- cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-config
{: #cs_cluster_config}

Após efetuar login, faça download dos dados de configuração e certificados do Kubernetes para se conectar ao seu cluster e execute comandos `kubectl`. Os arquivos são transferidos por download para `user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>`.
{: shortdesc}

```
ibmcloud ks cluster-config --cluster CLUSTER [--admin] [--export] [--network] [--skip-rbac] [-s] [--yaml]
```
{: pre}

**Permissões mínimas necessárias**: função de serviço **Visualizador** ou **Leitor** do {{site.data.keyword.Bluemix_notm}} IAM para o cluster no {{site.data.keyword.containerlong_notm}}. Além disso, se você tiver apenas uma função da plataforma ou uma função de serviço, restrições adicionais serão aplicadas.
* **Plataforma**: se você tiver apenas uma função da plataforma, será possível executar esse comando, mas você precisará de uma [função de serviço](/docs/containers?topic=containers-users#platform) ou uma [política RBAC customizada](/docs/containers?topic=containers-users#role-binding) para executar ações do Kubernetes no cluster.
* **Serviço**: se você tiver apenas uma função de serviço, será possível executar esse comando. No entanto, seu administrador de cluster deve fornecer o nome do cluster e o ID porque não é possível executar o comando `ibmcloud ks clusters` ou ativar o console do {{site.data.keyword.containerlong_notm}} para visualizar clusters. Depois disso, é possível [ativar o painel do Kubernetes por meio da CLI](/docs/containers?topic=containers-app#db_cli) e trabalhar com o Kubernetes.

**Opções de comando**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--admin</code></dt>
<dd>Faça download dos certificados TLS e dos arquivos de permissão para a função de Super usuário. É possível usar os certificados para automatizar tarefas em um cluster sem precisar autenticar novamente. Os arquivos são transferidos por download para `<user_home_directory>/.bluemix/plugins/container-service/clusters/<cluster_name>-admin`. Esse valor é opcional.</dd>

<dt><code> -- network </code></dt>
<dd>Faça download do arquivo de configuração do Calico, certificados TLS e arquivos de permissão necessários para executar os comandos <code>calicoctl</code> em seu cluster. Esse valor é opcional. **Nota**: para obter o comando de exportação para os dados de configuração e certificados do Kubernetes transferidos por download, deve-se executar esse comando sem essa sinalização.</dd>

<dt><code>--export</code></dt>
<dd>Faça download dos dados de configuração e certificados do Kubernetes sem nenhuma mensagem diferente do comando de exportação. Como nenhuma mensagem é exibida, é possível usar essa sinalização quando você cria scripts automatizados. Esse valor é opcional.</dd>

<dt><code> -- skip-rbac </code></dt>
<dd>Ignore a inclusão de funções RBAC do usuário Kubernetes com base nas funções de acesso ao serviço do {{site.data.keyword.Bluemix_notm}} IAM para a configuração do cluster. Inclua essa opção somente se você [gerenciar suas próprias funções RBAC do Kubernetes](/docs/containers?topic=containers-users#rbac). Se você usar [funções de acesso ao serviço {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-access_reference#service) para gerenciar todos os seus usuários do RBAC, não inclua essa opção.</dd>

<dt><code>-s</code></dt>
<dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

<dt><code>--yaml</code></dt>
<dd>Imprime a saída de comando em formato YAML. Esse valor é opcional.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks cluster-config --cluster my_cluster
```
{: pre}


### ibmcloud ks cluster-create
{: #cs_cluster_create}

Crie um cluster em sua organização. Para clusters gratuitos, você especifica o nome do cluster; tudo o mais é configurado com um valor padrão. Um cluster grátis é excluído automaticamente após 30 dias. É possível ter um cluster grátis de cada vez. Para aproveitar os recursos integrais do Kubernetes, crie um cluster padrão.
{: shortdesc}

```
ibmcloud ks cluster-create [--file FILE_LOCATION] [--hardware HARDWARE] --zone ZONE --machine-type MACHINE_TYPE --name NAME [--kube-version MAJOR.MINOR.PATCH] [--no-subnet] [--private-vlan PRIVATE_VLAN] [--public-vlan PUBLIC_VLAN] [--private-only] [--private-service-endpoint] [--public-service-endpoint] [--workers WORKER] [--disable-disk-encrypt] [--trusted] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>:
* Função de plataforma **Administrador** para o {{site.data.keyword.containerlong_notm}} no nível de conta
* Função de plataforma **Administrador** para o {{site.data.keyword.registrylong_notm}} no nível de conta
* Função de **Superusuário** para a infraestrutura do IBM Cloud (SoftLayer)

<strong>Opções de comandos</strong>

<dl>
<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>O caminho para o arquivo YAML para criar seu cluster padrão. Em vez de definir as características de seu cluster usando as opções fornecidas nesse comando, será possível usar um arquivo YAML. Esse valor é opcional para clusters padrão e não está disponível para clusters livres. <p class="note">Se, no comando, você fornecer a mesma opção que o parâmetro no arquivo YAML, o valor no comando terá precedência sobre o valor no YAML. Por exemplo, você define um local em seu arquivo do YAML e usa a opção <code>--zone</code> no comando; o valor inserido nessa opção substitui o valor no arquivo do YAML.</p>

<pre class="codeblock">
<code>name: <em>&lt;cluster_name&gt;</em>
zone: <em>&lt;zone&gt;</em>
no-subnet: <em>&lt;no-subnet&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
kube-version: <em>&lt;kube-version&gt;</em>
diskEncryption: <em>false</em>
trusted: <em>true</em>
</code></pre>
</dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>O nível de isolamento de hardware para seu nó do trabalhador. Use dedicado para disponibilizar recursos físicos disponíveis apenas para você ou compartilhado para permitir que os recursos físicos sejam compartilhados com outros clientes da IBM. O padrão é shared. Esse valor é opcional para clusters padrão da VM e não está disponível para clusters grátis. Para tipos de máquina bare metal, especifique `dedicated`.</dd>

<dt><code>--zone <em>ZONE</em></code></dt>
<dd>Esse valor é necessário para clusters padrão. Clusters grátis podem ser criados na região que será o destino com o comando <code>ibmcloud ks region-set</code>, mas não é possível especificar a zona.

<p>Revise [zonas disponíveis](/docs/containers?topic=containers-regions-and-zones#zones).</p>

<p class="note">Quando você seleciona uma zona que está localizada fora de seu país, tenha em mente que você pode requerer autorização legal antes que os dados possam ser armazenados fisicamente em um país estrangeiro.</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Escolha um tipo de máquina. É possível implementar os nós do trabalhador como máquinas virtuais em hardware compartilhado ou dedicado ou como máquinas físicas no bare metal. Os tipos de máquinas físicas e virtuais disponíveis variam de acordo com a zona na qual você implementa o cluster. Para obter mais informações, consulte a documentação para o comando `ibmcloud ks machine-types` [](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types). Esse valor é necessário para clusters padrão e não está disponível para clusters livres.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>O nome para o cluster.  Este valor é obrigatório. O nome deve iniciar com uma letra, pode conter letras, números e hífen (-) e deve ter 35 caracteres ou menos. O nome do cluster e a região na qual o cluster está implementado formam o nome completo do domínio para o subdomínio do Ingress. Para assegurar que o subdomínio do Ingress seja exclusivo dentro de uma região, o nome do cluster pode ser truncado e anexado com um valor aleatório dentro do nome de domínio do Ingress.
</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>A versão do Kubernetes para o nó principal do cluster. Esse valor é opcional. Quando a versão não for especificada, o cluster será criado com o padrão de versões do Kubernetes suportadas. Para ver as versões disponíveis, execute <code>ibmcloud ks kube-versions</code>.
</dd>

<dt><code>--no-subnet</code></dt>
<dd>Por padrão, uma sub-rede móvel pública e uma privada são criadas na VLAN associada ao cluster. Inclua a sinalização <code>--no-subnet</code> para evitar a criação de sub-redes com o cluster. É possível [criar](#cs_cluster_subnet_create) ou [incluir](#cs_cluster_subnet_add) sub-redes em um cluster posteriormente.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>

<ul>
<li>Esse parâmetro não está disponível para clusters livres.</li>
<li>Se esse cluster padrão for o primeiro cluster padrão que você criar nessa zona, não inclua essa sinalização. Uma VLAN privada é criada para você quando o cluster é criado.</li>
<li>Se você criou um cluster padrão antes nessa zona ou criou uma VLAN privada na infraestrutura do IBM Cloud (SoftLayer) antes, deverá especificar essa VLAN privada. Os roteadores de VLAN privada sempre iniciam com <code>bcr</code> (roteador de backend) e roteadores de VLAN pública sempre iniciam com <code>fcr</code> (roteador de front-end). Ao criar um cluster e especificar as VLANs públicas e privadas, o número e a combinação de letras após esses prefixos devem corresponder.</li>
</ul>

<p>Para descobrir se você já tem uma VLAN privada para uma zona específica ou para localizar o nome de uma VLAN privada existente, execute <code>ibmcloud ks vlans <em>&lt;zone&gt;</em></code>.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>
<ul>
<li>Esse parâmetro não está disponível para clusters livres.</li>
<li>Se esse cluster padrão for o primeiro cluster padrão que você criar nessa zona, não use essa sinalização. Uma VLAN pública é criada para você quando o cluster é criado.</li>
<li>Se você criou um cluster padrão antes nessa zona ou criou uma VLAN pública na infraestrutura do IBM Cloud (SoftLayer) antes, especifique essa VLAN pública. Se você deseja conectar seus nós do trabalhador somente a uma VLAN privada, não especifique esta opção. Os roteadores de VLAN privada sempre iniciam com <code>bcr</code> (roteador de backend) e roteadores de VLAN pública sempre iniciam com <code>fcr</code> (roteador de front-end). Ao criar um cluster e especificar as VLANs públicas e privadas, o número e a combinação de letras após esses prefixos devem corresponder.</li>
</ul>

<p>Para descobrir se você já tem uma VLAN pública para uma zona específica ou para localizar o nome de uma VLAN pública existente, execute <code>ibmcloud ks vlans <em>&lt;zone&gt;</em></code>.</p></dd>

<dt><code>-- somente privado </code></dt>
  <dd>Use essa opção para evitar que uma VLAN pública seja criada. Necessário somente quando você especifica a sinalização `--private-vlan` e não inclui a sinalização `--public-vlan`. <p class="note">Se os nós do trabalhador estão configurados somente com uma VLAN privada, deve-se ativar o terminal em serviço privado ou configurar um dispositivo de gateway. Para obter mais informações, consulte  [ Clusters privados ](/docs/containers?topic=containers-plan_clusters#private_clusters).</p></dd>

<dt><code>--private-service-endpoint </code></dt>
  <dd>**Clusters padrão que executam Kubernetes versão 1.11 ou mais recente em [Contas ativadas pelo VRF](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started)**: Ative o [terminal em serviço privado](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private) para que o seu mestre Kubernetes e os nós do trabalhador se comuniquem através da VLAN privada. Além disso, é possível escolher ativar o terminal em serviço público usando a sinalização `--public-service-endpoint` para acessar seu cluster por meio da Internet. Se você ativa somente o terminal em serviço privado, deve-se estar conectado à VLAN privada para se comunicar com seu mestre do Kubernetes. Depois de ativar um terminal em serviço privado, não será possível desativá-lo posteriormente.<br><br>Depois de criar o cluster, será possível obter o terminal executando `ibmcloud ks cluster-get <cluster_name_or_ID>`.</dd>

<dt><code>--public-service-endpoint </code></dt>
  <dd>**Clusters padrão que executam o Kubernetes versão 1.11 ou mais recente**: ative o [terminal em serviço público](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_public) para que o mestre do Kubernetes possa ser acessado por meio da rede pública, por exemplo, para executar comandos `kubectl` por meio de seu terminal. Se você tiver uma [conta ativada para VRF](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started) e também incluir o sinalizador `--private-service-endpoint`, a [comunicação mestre-nó do trabalhador](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_both) estará na rede privada. É possível desativar posteriormente o terminal em serviço público se você desejar um cluster somente privado.<br><br>Depois de criar o cluster, será possível obter o terminal executando `ibmcloud ks cluster-get <cluster_name_or_ID>`.</dd>

<dt><code>--workers WORKER</code></dt>
<dd>O número de nós do trabalhador que
você deseja implementar em seu cluster. Se não
especificar essa opção, um cluster com 1 nó do trabalhador será criado. Esse valor é opcional para clusters padrão e não está disponível para clusters livres.

<p class="important">A cada nó do trabalhador é designado um ID de nó do trabalhador e um nome de domínio exclusivos que não devem ser
mudados manualmente após a criação do cluster. Mudar o ID ou o nome do domínio evita que o mestre do Kubernetes gerencie o cluster.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Os nós do trabalhador apresentam criptografia de disco AES de 256 bits por padrão; [saiba mais](/docs/containers?topic=containers-security#encrypted_disk). Para desativar a criptografia, inclua essa opção.</dd>

<dt><code>--trusted</code></dt>
<dd><p>**Somente bare metal**: ative [Cálculo confiável](/docs/containers?topic=containers-security#trusted_compute) para verificar os nós do trabalhador do bare metal com relação à violação. Se você não ativar a confiança durante a criação do cluster, mas desejar posteriormente, será possível usar o comando `ibmcloud ks feature-enable` [](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_enable). Depois de ativar a confiança, não é possível desativá-la posteriormente.</p>
<p>Para verificar se o tipo da máquina bare metal suporta a confiança, verifique o campo `Trustable` na saída do [comando](#cs_machine_types) `ibmcloud ks machine-types <zone>`. Para verificar se um cluster está ativado para confiança, visualize o campo **Pronto para confiança** na saída do comando `ibmcloud ks cluster-get` [](#cs_cluster_get). Para verificar se um nó do trabalhador bare metal está ativado para confiança, visualize o campo **Confiança** na saída do comando `ibmcloud ks worker-get` [](#cs_worker_get).</p></dd>

<dt><code>-s</code></dt>
<dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplos**:

**Criar um cluster grátis**: especifique somente o nome do cluster, todo o resto é configurado para um valor padrão. Um cluster grátis é excluído automaticamente após 30 dias. É possível ter um cluster grátis de cada vez. Para aproveitar os recursos integrais do Kubernetes, crie um cluster padrão.

```
ibmcloud ks cluster-create --name my_cluster
```
{: pre}

**Criar o seu primeiro cluster padrão**: o primeiro cluster padrão que é criado em uma zona também cria uma VLAN privada. Portanto, não inclua a sinalização `--public-vlan`.
{: #example_cluster_create}

```
ibmcloud ks cluster-create --zone dal10 --private-vlan my_private_VLAN_ID --machine-type b3c.4x16 --name my_cluster --hardware shared --workers 2
```
{: pre}

**Criar clusters padrão subsequentes**: se você já criou um cluster padrão nessa zona ou criou uma VLAN pública na infraestrutura do IBM Cloud (SoftLayer) antes, especifique essa VLAN pública com a sinalização `--public-vlan`. Para descobrir se você já tem uma VLAN pública para uma zona específica ou localizar o nome de uma VLAN pública existente, execute `ibmcloud ks vlans <zone>`.

```
ibmcloud ks cluster-create --zone dal10 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type b3c.4x16 --name my_cluster --hardware shared --workers 2
```
{: pre}

### ibmcloud ks cluster-feature-disable public-service-endpoint
{: #cs_cluster_feature_disable}

**Em clusters que executam o Kubernetes versão 1.11 ou mais recente**: desative o terminal em serviço público para um cluster.
{: shortdesc}

```
ibmcloud ks cluster-feature-disable public-service-endpoint --cluster CLUSTER [-s] [-f]
```
{: pre}

**Importante**: antes de desativar o terminal público, deve-se primeiro concluir as etapas a seguir para ativar o terminal em serviço privado:
1. Ative o terminal de serviço privado executando `ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name>`.
2. Siga o prompt na CLI para atualizar o servidor de API do mestre do Kubernetes.
3. [Recarregue todos os nós do trabalhador em seu cluster para selecionar a configuração de terminal privado.](#cs_worker_reload)

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

```
ibmcloud ks cluster-feature-disable public-service-endpoint -- cluster my_cluster
```
{: pre}


### ibmcloud ks cluster-feature-enable
{: #cs_cluster_feature_enable}

Ative um recurso em um cluster existente. Esse comando deve ser combinado com um dos subcomandos a seguir para o recurso que você deseja ativar.
{: shortdesc}

#### ibmcloud ks cluster-feature-enable private-service-endpoint
{: #cs_cluster_feature_enable_private_service_endpoint}

**Em clusters que executam Kubernetes versão 1.11 ou mais recente**: Ative o [terminal em serviço privado](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private) para tornar seu cluster principal acessível de forma privada.
{: shortdesc}

```
ibmcloud ks cluster-feature-enable private-service-endpoint --cluster CLUSTER [-s]
```
{: pre}

Para executar este comando:
1. Ative o [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) em sua conta de infraestrutura do IBM Cloud (SoftLayer).
2. [Ative sua conta do {{site.data.keyword.Bluemix_notm}} para usar os terminais em serviço](/docs/services/service-endpoint?topic=service-endpoint-getting-started#getting-started).
3. Execute `ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name>`.
4. Siga o prompt na CLI para atualizar o servidor de API do mestre do Kubernetes.
5. [Recarregue todos os nós do trabalhador](#cs_worker_reload) em seu cluster para selecionar a configuração de terminal privado.

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

```
ibmcloud ks cluster-feature-enable private-service-endpoint -- cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-feature-enable public-service-endpoint
{: #cs_cluster_feature_enable_public_service_endpoint}

**Em clusters que executam o Kubernetes versão 1.11 ou mais recente**: ative o [terminal em serviço público](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_public) para tornar seu cluster mestre publicamente acessível.
{: shortdesc}

```
ibmcloud ks cluster-feature-enable public-service-endpoint --cluster CLUSTER [-s]
```
{: pre}

Após a execução desse comando, deve-se atualizar o servidor de API para usar o terminal de serviço seguindo o prompt na CLI.

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

```
ibmcloud ks cluster-feature-enable public-service-endpoint -- cluster my_cluster
```
{: pre}

#### ibmcloud ks cluster-feature-enable confiável
{: #cs_cluster_feature_enable_trusted}

Ative [Cálculo confiável](/docs/containers?topic=containers-security#trusted_compute) para todos os nós do trabalhador bare metal suportados que estão no cluster. Depois de ativar a confiança, não é possível desativá-la posteriormente para o cluster.
{: shortdesc}

```
ibmcloud ks cluster-feature-enable trusted --cluster CLUSTER [-s] [-f]
```
{: pre}

Para verificar se o tipo da máquina bare metal suporta a confiança, verifique o campo **Trustable** na saída do [comando](#cs_machine_types) `ibmcloud ks machine-types <zone>`. Para verificar se um cluster está ativado para confiança, visualize o campo **Confiança pronta** na saída do [comando](#cs_cluster_get) `ibmcloud ks cluster get`. Para verificar se um nó do trabalhador bare metal está ativado para confiança, visualize o campo **Confiança** na saída do [comando](#cs_worker_get) `ibmcloud ks worker get`.

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>-f</code></dt>
   <dd>Use essa opção para forçar a opção <code>--trusted</code> sem prompts de usuário. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

```
ibmcloud ks cluster-feature-enable trusted -- cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-get
{: #cs_cluster_get}

Visualizar informações sobre um cluster em sua organização.
{: shortdesc}

```
ibmcloud ks cluster-get --cluster CLUSTER [--json] [--showResources] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

   <dt><code><em>--showResources</em></code></dt>
   <dd>Mostre mais recursos de cluster, como complementos, VLANs, sub-redes e armazenamento.</dd>


  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
  </dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-get -- cluster my_cluster -- showResources
  ```
  {: pre}

**Saída de exemplo**:

```
Name:                           mycluster
ID:                             df253b6025d64944ab99ed63bb4567b6
State:                          normal
Created:                        2018-09-28T15:43:15+0000
Location:                       dal10
Master URL:                     https://c3.<region>.containers.cloud.ibm.com:30426
Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
Master Location:                Dallas
Master Status:                  Ready (21 hours ago)
Ingress Subdomain:              mycluster.us-south.containers.appdomain.cloud
Ingress Secret:                 mycluster
Workers:                        6
Worker Zones:                   dal10, dal12
Version:                        1.11.3_1524
Owner:                          owner@email.com
Monitoring Dashboard:           ...
Resource Group ID:              a8a12accd63b437bbd6d58fb6a462ca7
Resource Group Name:            Default

Subnet VLANs VLAN ID Subnet CIDR Public User-managed 2234947 10.xxx.xx.xxx/29 false false 2234945 169.xx.xxx.xxx/29 true false

```
{: screen}

### ibmcloud ks cluster-pull-secret-apply
{: #cs_cluster_pull_secret_apply}

Crie um ID de serviço do {{site.data.keyword.Bluemix_notm}} IAM para o cluster, crie uma política para o ID de serviço que designa a função de acesso ao serviço **Leitor** no {{site.data.keyword.registrylong_notm}} e, em seguida, crie uma chave de API para o ID de serviço. A chave de API é, então, armazenada em um `imagePullSecret do Kubernetes` para que seja possível extrair imagens de seus namespaces do {{site.data.keyword.registryshort_notm}} para contêineres que estão no namespace `default` do Kubernetes. Esse processo acontece automaticamente quando você cria um cluster. Se você tiver um erro durante o processo de criação do cluster ou tiver um cluster existente, será possível usar esse comando para aplicar o processo novamente.
{: shortdesc}

Quando você executa esse comando, a criação de credenciais do IAM e de segredos de extração de imagem é iniciada e pode levar algum tempo para ser concluída. Não será possível implementar contêineres que puxam uma imagem dos domínios `icr.io` do {{site.data.keyword.registrylong_notm}} até que os segredos de extração de imagem sejam criados. Para verificar os segredos de extração da imagem, execute `kubectl get secrets | grep icr`.
{: important}

```
ibmcloud ks cluster-pull-secret-apply -- cluster CLUSTER
```
{: pre}

Esse método de chave de API substitui o método anterior de autorizar um cluster para acessar o {{site.data.keyword.registrylong_notm}} criando automaticamente um [token](/docs/services/Registry?topic=registry-registry_access#registry_tokens) e armazenando-o em um segredo de extração de imagem. Agora, ao usar as chaves da API do IAM para acessar o {{site.data.keyword.registrylong_notm}}, é possível customizar políticas do IAM para o ID de serviço para restringir o acesso aos seus namespaces ou imagens específicas. Por exemplo, é possível mudar as políticas de ID de serviço no segredo de extração da imagem do cluster para extrair imagens de apenas uma determinada região de registro ou namespace. Antes de poder customizar as políticas do IAM, deve-se [ ativar as políticas do {{site.data.keyword.Bluemix_notm}} IAM para o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-user#existing_users).

Para obter mais informações, consulte [Entendendo como seu cluster é autorizado a extrair imagens do {{site.data.keyword.registrylong_notm}}](/docs/containers?topic=containers-images#cluster_registry_auth).

Se você incluiu políticas do IAM em um ID de serviço existente, como para restringir o acesso a um registro regional, o ID do serviço, as políticas do IAM e a chave de API para o segredo de extração da imagem serão reconfigurados por esse comando.
{: important}

<strong>Permissões mínimas necessárias</strong>:
*  Função da plataforma **Operador ou Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}
*  Função de plataforma ** Administrador **  no  {{site.data.keyword.registrylong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   </dl>

**Exemplo**:

```
ibmcloud ks cluster-pull-secret-apply -- cluster my_cluster
```
{: pre}

### ibmcloud ks cluster-refresh
{: #cs_cluster_refresh}

Reinicie os nós do cluster mestre para aplicar novas mudanças na configuração da API do Kubernetes. Os nós do trabalhador, os apps e os recursos não são modificados e continuam a ser executados.
{: shortdesc}

```
ibmcloud ks cluster-refresh --cluster CLUSTER [-f] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>-f</code></dt>
   <dd>Use essa opção para forçar a remoção de um cluster sem prompts de usuário. Esse valor é opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-refresh --cluster my_cluster
  ```
  {: pre}

### ibmcloud ks cluster-rm
{: #cs_cluster_rm}

Remover um cluster de sua organização.
{: shortdesc}

```
ibmcloud ks cluster-rm -- cluster CLUSTER [--force-delete-storage ] [ -f ] [ -s ]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--force-delete-storage </code></dt>
   <dd>Exclui o cluster e qualquer armazenamento persistente usado pelo cluster. **Atenção**: se você incluir essa sinalização, os dados que estiverem armazenados no cluster ou suas instâncias de armazenamento associadas não poderão ser recuperados. Esse valor é opcional.</dd>

   <dt><code>-f</code></dt>
   <dd>Use essa opção para forçar a remoção de um cluster sem prompts de usuário. Esse valor é opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-rm -- cluster my_cluster
  ```
  {: pre}


### cluster-atualização do cluster ibmcloud
{: #cs_cluster_update}

Atualize o mestre do Kubernetes para a versão de API padrão. Durante a atualização, não é possível acessar nem mudar o cluster. Nós do trabalhador, apps e recursos que foram implementados pelo usuário não são modificados e continuam a ser executados.
{: shortdesc}

```
ibmcloud ks cluster-update --cluster CLUSTER [--kube-version MAJOR.MINOR.PATCH] [--force-update] [-f] [-s]
```
{: pre}

Pode ser necessário mudar seus arquivos YAML para implementações futuras. Revise essa [nota sobre a liberação](/docs/containers?topic=containers-cs_versions) para obter detalhes.

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>A versão do Kubernetes do cluster. Se você não especificar uma versão, o mestre do Kubernetes será atualizado para a versão de API padrão. Para ver as versões disponíveis, execute [ibmcloud ks kube-versions](#cs_kube_versions). Esse valor é opcional.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Tente a atualização mesmo se a mudança for maior que duas versões secundárias. Esse valor é opcional.</dd>

   <dt><code>-f</code></dt>
   <dd>Force o comando a ser executado sem prompts do usuário. Esse valor é opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-update -- cluster my_cluster
  ```
  {: pre}


### ibmcloud ks clusters
{: #cs_clusters}

Visualizar uma lista de clusters em sua organização.
{: shortdesc}

```
ibmcloud ks clusters [ -- json ] [ -s ]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

  <dl><dt><code>--json</code></dt>
  <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
  </dl>

**Exemplo**:

  ```
  ibmcloud ks clusters
  ```
  {: pre}


### ibmcloud ks kube-versions
{: #cs_kube_versions}

Visualize uma lista de versões do Kubernetes suportadas no {{site.data.keyword.containerlong_notm}}. Atualize o seu [cluster mestre](#cs_cluster_update) e [nós do trabalhador](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update) para a versão padrão para os recursos mais recentes, estáveis.
{: shortdesc}

```
ibmcloud ks kube-versions [ -- json ] [ -s ]
```
{: pre}

<strong> Permissões mínimas necessárias </strong>: nenhuma

**Opções de comando**:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
  </dl>

**Exemplo**:

  ```
  ibmcloud ks kube-versions
  ```
  {: pre}

<br />



## Comandos do cluster: serviços e integrações
{: #cluster_services_commands}


### ibmcloud ks cluster-service-bind
{: #cs_cluster_service_bind}

Inclua um serviço do {{site.data.keyword.Bluemix_notm}} em um cluster. Para visualizar os serviços do {{site.data.keyword.Bluemix_notm}} disponíveis por meio do catálogo do {{site.data.keyword.Bluemix_notm}}, execute `ibmcloud service offerings`. **Nota**: é possível incluir somente serviços do {{site.data.keyword.Bluemix_notm}} que suportam chaves de serviço.
{: shortdesc}

```
ibmcloud ks cluster-service-bind --cluster CLUSTER --namespace KUBERNETES_NAMESPACE --service SERVICE_INSTANCE_GUID [--key SERVICE_INSTANCE_KEY] [--role IAM_ROLE] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}} e a função **Desenvolvedor** do Cloud Foundry

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code> -- key  <em> SERVICE_INSTANCE_KEY </em> </code></dt>
   <dd>O nome ou o GUID da chave de serviço. Esse valor é opcional. Inclua esse valor se você desejar usar uma chave de serviço existente em vez de criar uma nova chave de serviço.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>O nome do namespace do Kubernetes. Este valor é obrigatório.</dd>

   <dt><code> -- service  <em> SERVICE_INSTANCE_GUID </em> </code></dt>
   <dd>O ID da instância de serviço do {{site.data.keyword.Bluemix_notm}} que você deseja ligar. Para localizar o ID, execute <code>ibmcloud service show <service_instance_name> --guid</code>. Este valor é obrigatório. </dd>

   <dt><code> -- role  <em> IAM_ROLE </em> </code></dt>
   <dd>A função do {{site.data.keyword.Bluemix_notm}} IAM que você deseja que a chave de serviço tenha. O valor padrão é a função de serviço `Writer` do IAM. Não inclua esse valor se você estiver usando uma chave de serviço existente ou para serviços que não estão ativados pelo IAM-enabled, como o Cloud Foundry Services.<br><br>
   Para listar as funções disponíveis para o serviço, execute `ibmcloud iam roles --service <service_name>`. O nome do serviço é o nome do serviço no catálogo que você pode obter executando `ibmcloud catalog search`.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-service-bind --cluster my_cluster --namespace my_namespace --service my_service_instance
  ```
  {: pre}


### ibmcloud ks cluster-service-unbind
{: #cs_cluster_service_unbind}

Remova um serviço do {{site.data.keyword.Bluemix_notm}} de um cluster.
{: shortdesc}

```
ibmcloud ks cluster-service-unbind --cluster CLUSTER --namespace KUBERNETES_NAMESPACE --service SERVICE_INSTANCE_GUID [-s]
```
{: pre}

Quando você remove um serviço do {{site.data.keyword.Bluemix_notm}}, as credenciais de serviço são removidas do cluster. Se um pod ainda está usando o serviço,
ele falha porque as credenciais de serviço não podem ser localizadas.
{: note}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}} e a função **Desenvolvedor** do Cloud Foundry

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>O nome do namespace do Kubernetes. Este valor é obrigatório.</dd>

   <dt><code> -- service  <em> SERVICE_INSTANCE_GUID </em> </code></dt>
   <dd>O ID da instância de serviço do {{site.data.keyword.Bluemix_notm}} que você deseja remover. Para localizar o ID da instância de serviço, execute `ibmcloud ks cluster-services <cluster_name_or_ID>`. Este valor é obrigatório.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-service-unbind --cluster my_cluster --namespace my_namespace --service 8567221
  ```
  {: pre}


### ibmcloud ks cluster-services
{: #cs_cluster_services}

Listar os serviços que estão ligados a um ou todos os namespaces do Kubernetes em um cluster. Se nenhuma
opção é especificada, os serviços para o namespace padrão são exibidos.
{: shortdesc}

```
ibmcloud ks cluster-services -- cluster CLUSTER [ -- namespace KUBERNETES_NAMESPACE ] [ -- all namespaces ] [ -- json ] [ -s ]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Inclua os serviços que forem limitados a um namespace específico em um cluster. Esse valor é opcional.</dd>

   <dt><code>--all-namespaces</code></dt>
   <dd>Inclua os serviços que forem limitados a todos os namespaces em um cluster. Esse valor é opcional.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-services --cluster my_cluster --namespace my_namespace
  ```
  {: pre}


### ibmcloud ks va
{: #cs_va}

Após você [instalar o scanner de contêiner](/docs/services/va?topic=va-va_index#va_install_container_scanner), visualize um relatório de avaliação de vulnerabilidade detalhado para um contêiner em seu cluster.
{: shortdesc}

```
ibmcloud ks va --container CONTAINER_ID [--extended] [--vulnerabilities] [--configuration-issues] [--json]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: função de acesso de serviço **Leitor** para o {{site.data.keyword.registrylong_notm}}. **Nota**: não designe políticas ao {{site.data.keyword.registryshort_notm}} no nível do grupo de recursos.

**Opções de comando**:

<dl>
<dt><code>--container CONTAINER_ID</code></dt>
<dd><p>O ID do contêiner. Este valor é obrigatório.</p>
<p>Para localizar o ID de seu contêiner:<ol><li>[Direcione a CLI do Kubernetes para o seu cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).</li><li>Liste os seus pods executando `kubectl get pods`.</li><li>Localize o campo **ID do contêiner** na saída do comando `kubectl describe pod <pod_name>`. Por exemplo,  ` Container ID: containerd://1a11a1aa2b2b22223333c44444ccc555667d7dd777888e8ef99f1011121314g15 `.</li><li>Remova o prefixo `containerd://` do ID antes de usar o ID do contêiner para o comando `ibmcloud ks va`. Por exemplo, `1a11a1aa2b2b22223333c44444ccc555667d7dd777888e8ef99f1011121314g15`.</li></ol></p></dd>

<dt><code>--extended</code></dt>
<dd><p>Estenda a saída de comando para mostrar mais informações de correção para pacotes vulneráveis. Esse valor é opcional.</p>
<p>Por padrão, os resultados da varredura mostram o ID, o status da política, os pacotes afetados e o modo de resolver. Com a sinalização `--extended`, isso inclui informações como o resumo, o aviso de segurança do fornecedor e o link de aviso oficial.</p></dd>

<dt><code>--vulnerabilities</code></dt>
<dd>Restrinja a saída de comando para mostrar apenas as vulnerabilidades do pacote. Esse valor é opcional. Não será possível usar essa sinalização se você usar a sinalização `--configuration-issues`.</dd>

<dt><code>--configuration-issues</code></dt>
<dd>Restrinja a saída de comando para mostrar apenas os problemas de configuração. Esse valor é opcional. Não será possível usar essa sinalização se você usar a sinalização `--vulnerabilities`.</dd>

<dt><code>--json</code></dt>
<dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

```
ibmcloud ks va --container 1a11a1aa2b2b22223333c44444ccc555667d7dd777888e8ef99f1011121314g15 --extended --vulnerabilities --json
```
{: pre}

### ibmcloud ks key-protect-enable
{: #cs_key_protect}

Criptografe seus segredos do Kubernetes usando o [{{site.data.keyword.keymanagementservicefull}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/key-protect?topic=key-protect-getting-started-tutorial#getting-started-tutorial) como um [provedor de serviço de gerenciamento de chaves (KMS) ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/administer-cluster/kms-provider/) em seu cluster. Para girar uma chave em um cluster com criptografia de chave existente, execute novamente esse comando com um novo ID de chave raiz.
{: shortdesc}

```
ibmcloud ks key-protect-enable --cluster CLUSTER_NAME_OR_ID --key-protect-url ENDPOINT --key-protect-instance INSTANCE_GUID --crk ROOT_KEY_ID
```
{: pre}

Não exclua chaves raiz em sua instância do {{site.data.keyword.keymanagementserviceshort}}. Não exclua chaves mesmo se você girar para usar uma nova chave. Não será possível acessar ou remover os dados em etcd ou os dados dos segredos em seu cluster se você excluir uma chave raiz.
{: important}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

**Opções de comando**:

<dl>
<dt><code> -- container CLUSTER_NAME_OR_ID </code></dt>
<dd>O nome ou ID do cluster.</dd>

<dt><code>--key-protect-url ENDPOINT </code></dt>
<dd>O terminal do  {{site.data.keyword.keymanagementserviceshort}}  para a instância do cluster. Para obter o terminal, veja [terminais em serviço por região](/docs/services/key-protect?topic=key-protect-regions#service-endpoints).</dd>

<dt><code>--key-protect-instance INSTANCE_GUID </code></dt>
<dd>Seu GUID da instância do  {{site.data.keyword.keymanagementserviceshort}} . Para obter o GUID da instância, execute <code>ibmcloud resource service-instance SERVICE_INSTANCE_NAME --id</code> e copie o segundo valor (não o CRN integral).</dd>

<dt><code> -- crk ROOT_KEY_ID </code></dt>
<dd>Seu ID da chave raiz  {{site.data.keyword.keymanagementserviceshort}} . Para obter o CRK, veja [Visualizando chaves](/docs/services/key-protect?topic=key-protect-view-keys#view-keys).</dd>
</dl>

**Exemplo**:

```
ibmcloud ks key-protect-enable --cluster mycluster --key-protect-url keyprotect.us-south.bluemix.net --key-protect-instance a11aa11a-bbb2-3333-d444-e5e555e5ee5 --crk 1a111a1a-bb22-3c3c-4d44-55e555e55e55
```
{: pre}


### ibmcloud ks webhook-create
{: #cs_webhook_create}

Registre um webhook.
{: shortdesc}

```
ibmcloud ks webhook-create --cluster CLUSTER --level LEVEL --type slack --url URL [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd>O nível de notificação, como <code>Normal</code> ou <code>Warning</code>. <code>Warning</code> é o valor padrão. Esse valor é opcional.</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>O tipo de webhook. A folga é atualmente suportada. Este valor é obrigatório.</dd>

   <dt><code>--url <em>URL</em></code></dt>
   <dd>A URL para o webhook. Este valor é obrigatório.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks webhook-create --cluster my_cluster --level Normal --type slack --url http://github.com/mywebhook
  ```
  {: pre}


<br />


## Comandos do cluster: Subnets
{: #cluster_subnets_commands}

### ibmcloud ks cluster-subnet-add
{: #cs_cluster_subnet_add}

É possível incluir sub-redes públicas ou privadas móveis existentes de sua conta de infraestrutura do IBM Cloud (SoftLayer) em seu cluster do Kubernetes ou reutilizar sub-redes por meio de um cluster excluído em vez de usar as sub-redes provisionadas automaticamente.
{: shortdesc}

```
ibmcloud ks cluster-subnet-add --cluster CLUSTER --subnet-id SUBNET [-s]
```
{: pre}

<p class="important">Os endereços IP públicos móveis são cobrados mensalmente. Se você remover endereços IP públicos móveis após o provisionamento de seu cluster, ainda será necessário pagar o encargo mensal, mesmo que os tenha usado apenas por um curto período de tempo.</br>
</br>Ao disponibilizar uma sub-rede para um cluster, os endereços IP dessa sub-rede serão usados para propósitos de rede do cluster. Para evitar conflitos de endereço IP, certifique-se de usar uma sub-rede com somente um cluster. Não use uma sub-rede para diversos clusters ou para outros propósitos fora do {{site.data.keyword.containerlong_notm}} ao mesmo tempo.</br>
</br>Para permitir a comunicação entre os trabalhadores em sub-redes diferentes na mesma VLAN, deve-se [permitir o roteamento entre sub-redes na mesma VLAN](/docs/containers?topic=containers-subnets#subnet-routing).</p>

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code> -- subnet-id  <em> SUBNET </em> </code></dt>
   <dd>O ID da sub-rede. Este valor é obrigatório.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-subnet-add --cluster my_cluster --subnet-id 1643389
  ```
  {: pre}


### ibmcloud ks cluster-sub-rede-criar
{: #cs_cluster_subnet_create}

Crie uma sub-rede em uma conta de infraestrutura do IBM Cloud (SoftLayer) e torne-a disponível para um cluster especificado em {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks cluster-subnet-create --cluster CLUSTER --size SIZE --vlan VLAN_ID [-s]
```
{: pre}

<p class="important">Quando você torna uma sub-rede disponível para um cluster, os endereços IP
dessa sub-rede são usados para propósitos de rede do cluster. Para evitar conflitos de endereço IP, certifique-se de usar uma sub-rede com somente um cluster. Não use uma sub-rede para diversos clusters ou para outros propósitos fora do {{site.data.keyword.containerlong_notm}} ao mesmo tempo.</br>
</br>Se você tiver múltiplas VLANs para um cluster, múltiplas sub-redes na mesma VLAN ou em um cluster de múltiplas zonas, deverá ativar uma [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para a sua conta de infraestrutura do IBM Cloud (SoftLayer) para que os seus nós do trabalhador possam se comunicar entre si na rede privada. Para ativar o VRF, [entre em contato com o representante de conta da infraestrutura do IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Se não for possível ou você não desejar ativar o VRF, ative o [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para executar essa ação, você precisa da [permissão de infraestrutura](/docs/containers?topic=containers-users#infra_access) **Rede > Gerenciar a rede VLAN Spanning** ou é possível solicitar ao proprietário da conta para ativá-la. Para verificar se o VLAN Spanning já está ativado, use o [comando](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.</p>

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório. Para listar os seus clusters, use o comando `ibmcloud ks clusters` [](#cs_clusters).</dd>

   <dt><code> -- size  <em> SIZE </em> </code></dt>
   <dd>O número de endereços IP de sub-rede. Este valor é obrigatório. Os valores possíveis são 8, 16, 32 ou 64.</dd>

   <dd>A VLAN na qual criar a sub-rede. Este valor é obrigatório. Para listar as VLANs disponíveis, use o [comando](#cs_vlans) `ibmcloud ks vlans <zone>`. A sub-rede é provisionada na mesma zona em que a VLAN está.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-subnet-create --cluster my_cluster --size 8 --vlan 1764905
  ```
  {: pre}


### ibmcloud ks cluster-user-subnet-add
{: #cs_cluster_user_subnet_add}

Traga a sua própria sub-rede privada para os seus clusters do {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks cluster-user-subnet-add --cluster CLUSTER --subnet-cidr SUBNET_CIDR --private-vlan PRIVATE_VLAN
```
{: pre}

Essa sub-rede privada não é uma fornecida pela infraestrutura do IBM Cloud (SoftLayer). Como tal, deve-se configurar qualquer roteamento de tráfego de rede de entrada e saída para a sub-rede. Para incluir uma sub-rede de infraestrutura do IBM Cloud (SoftLayer), use o comando `ibmcloud ks cluster-subnet-add` [](#cs_cluster_subnet_add).

<p class="important">Quando você torna uma sub-rede disponível para um cluster, os endereços IP
dessa sub-rede são usados para propósitos de rede do cluster. Para evitar conflitos de endereço IP, certifique-se de usar uma sub-rede com somente um cluster. Não use uma sub-rede para diversos clusters ou para outros propósitos fora do {{site.data.keyword.containerlong_notm}} ao mesmo tempo.</br>
</br>Se você tiver múltiplas VLANs para um cluster, múltiplas sub-redes na mesma VLAN ou em um cluster de múltiplas zonas, deverá ativar uma [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para a sua conta de infraestrutura do IBM Cloud (SoftLayer) para que os seus nós do trabalhador possam se comunicar entre si na rede privada. Para ativar o VRF, [entre em contato com o representante de conta da infraestrutura do IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Se não for possível ou você não desejar ativar o VRF, ative o [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para executar essa ação, você precisa da [permissão de infraestrutura](/docs/containers?topic=containers-users#infra_access) **Rede > Gerenciar a rede VLAN Spanning** ou é possível solicitar ao proprietário da conta para ativá-la. Para verificar se o VLAN Spanning já está ativado, use o [comando](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.</p>

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--subnet-cidr <em>SUBNET_CIDR</em></code></dt>
   <dd>O Classless InterDomain Routing (CIDR) de sub-rede. Esse valor é obrigatório e não deverá entrar em conflito com qualquer sub-rede que for usada pela infraestrutura do IBM Cloud (SoftLayer).

   Os prefixos suportados variam de `/30` (1 endereço IP) a `/24` (253 endereços IP). Se você definir o CIDR com um comprimento de prefixo e posteriormente precisar mudá-lo, primeiro inclua o novo CIDR, então, [remova o CIDR antigo](#cs_cluster_user_subnet_rm).</dd>

   <dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
   <dd>O ID da VLAN privada. Este valor é obrigatório. Ele deverá corresponder ao ID de VLAN privada de um ou mais nós do trabalhador no cluster.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-user-subnet-add --cluster my_cluster --subnet-cidr 169.xx.xxx.xxx/29 --private-vlan 1502175
  ```
  {: pre}


### ibmcloud ks cluster-user-subnet-rm
{: #cs_cluster_user_subnet_rm}

Remova a sua própria sub-rede privada de um cluster especificado. Qualquer serviço implementado em um endereço IP por meio de sua própria sub-rede privada permanecerá ativo após a remoção da sub-rede.
{: shortdesc}

```
ibmcloud ks cluster-user-subnet-rm --cluster CLUSTER --subnet-cidr SUBNET_CIDR --private-vlan PRIVATE_VLAN
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--subnet-cidr <em>SUBNET_CIDR</em></code></dt>
   <dd>O Classless InterDomain Routing (CIDR) de sub-rede. Esse valor é obrigatório e deve corresponder ao CIDR que foi configurado pelo comando `ibmcloud ks cluster-user-subnet-add` [](#cs_cluster_user_subnet_add).</dd>

   <dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
   <dd>O ID da VLAN privada. Esse valor é obrigatório e deve corresponder ao ID de VLAN que foi configurado pelo comando `ibmcloud ks cluster-user-subnet-add` [](#cs_cluster_user_subnet_add).</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks cluster-user-subnet-rm --cluster my_cluster --subnet-cidr 169.xx.xxx.xxx/29 --private-vlan 1502175
  ```
  {: pre}


### ibmcloud ks subnets
{: #cs_subnets}

Visualize uma lista de sub-redes que estão disponíveis em uma conta de infraestrutura do IBM Cloud (SoftLayer).
{: shortdesc}

```
ibmcloud ks sub-redes [ -- json ] [ -s ]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
  </dl>

**Exemplo**:

  ```
  ibmcloud ks subnets
  ```
  {: pre}


<br />


## Comandos do balanceador de carga do aplicativo (ALB) Ingress
{: #alb_commands}

### ibmcloud ks alb-autoupdate-disable
{: #cs_alb_autoupdate_disable}

Por padrão, as atualizações automáticas para o complemento do balanceador de carga de aplicativo (ALB) do Ingress são ativadas. Os pods do ALB são atualizados automaticamente quando uma nova versão de construção está disponível. Atualize o complemento manualmente; use esse comando para desativar as atualizações automáticas. Será possível, então, atualizar os pods do ALB executando o comando [`ibmcloud ks alb-update`](#cs_alb_update).
{: shortdesc}

```
ibmcloud ks alb-autoupdate-disable --cluster CLUSTER
```
{: pre}

Quando você atualiza a versão principal ou secundária do Kubernetes de seu cluster, a IBM faz as mudanças necessárias automaticamente na implementação do Ingress, mas não muda a versão de construção de seu complemento ALB do Ingress. Você é responsável por verificar a compatibilidade das versões mais recentes do Kubernetes e das imagens do complemento ALB do Ingress.

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Exemplo**:

```
ibmcloud ks alb-autoupdate-disable --cluster mycluster
```
{: pre}

### ibmcloud ks alb-autoupdate-enable
{: #cs_alb_autoupdate_enable}

Se as atualizações automáticas para o complemento ALB de Ingress estiverem desativadas, será possível reativar as atualizações automáticas. Sempre que a próxima versão de construção se torna disponível, os ALBs são atualizados automaticamente para a construção mais recente.
{: shortdesc}

```
ibmcloud ks alb-autoupdate-enable --cluster CLUSTER
```
{: pre}

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Exemplo**:

```
ibmcloud ks alb-autoupdate-enable --cluster mycluster
```
{: pre}

### ibmcloud ks alb-autoupdate-get
{: #cs_alb_autoupdate_get}

Verifique se as atualizações automáticas para o complemento ALB do Ingress estão ativadas e se os ALBs são atualizados para a versão de construção mais recente.
{: shortdesc}

```
ibmcloud ks alb-autoupdate-get --cluster CLUSTER
```
{: pre}

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Exemplo**:

```
ibmcloud ks alb-autoupdate-get --cluster mycluster
```
{: pre}

### ibmcloud ks alb-cert-deploy
{: #cs_alb_cert_deploy}

Implemente ou atualize um certificado de sua instância do {{site.data.keyword.cloudcerts_long_notm}} para o ALB em um cluster. É possível atualizar somente certificados que são importados da mesma instância do {{site.data.keyword.cloudcerts_long_notm}}.
{: shortdesc}

```
ibmcloud ks alb-cert-deploy [ -- update ] -- cluster CLUSTER -- secret-name SECRET_NAME -- cert-crn CERTIFICATE_CRN [ -- update ] [ -s ]
```
{: pre}

Quando você importa um certificado com esse comando, o segredo do certificado é criado em um namespace chamado `ibm-cert-store`. Uma referência a esse segredo é, então, criada no namespace `default`, que qualquer recurso Ingress em qualquer namespace pode acessar. Quando o ALB está processando solicitações, ele segue essa referência para selecionar e usar o segredo do certificado por meio do namespace `ibm-cert-store`.

Para permanecer dentro dos [limites de taxa](https://cloud.ibm.com/apidocs/certificate-manager#rate-limiting) configurados por {{site.data.keyword.cloudcerts_short}}, aguarde pelo menos 45 segundos entre os comandos `alb-cert-deploy` e `alb-cert-deploy -- update sucessivos`.
{: note}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comandos</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--update</code></dt>
   <dd>Atualize o certificado para um segredo do ALB em um cluster. Esse valor é opcional.</dd>

   <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
   <dd>Especifique um nome para o segredo do ALB quando ele for criado no cluster. Este valor é obrigatório. Certifique-se de que você não crie o segredo com o mesmo nome que o segredo de Ingress fornecido pela IBM. É possível obter o nome do segredo do Ingress fornecido pela IBM executando <code>ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress</code>.</dd>

   <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
   <dd>O CRN do certificado. Este valor é obrigatório.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>

**Exemplos**:

Exemplo para implementar um segredo do ALB:

   ```
   ibmcloud ks alb-cert-deploy --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
   ```
   {: pre}

Exemplo para atualizar um segredo do ALB existente:

 ```
 ibmcloud ks alb-cert-deploy --update --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:7e21fde8ee84a96d29240327daee3eb2
 ```
 {: pre}


### ibmcloud ks alb-cert-get
{: #cs_alb_cert_get}

Se você importou um certificado do {{site.data.keyword.cloudcerts_short}} para o ALB em um cluster, visualize informações sobre o certificado TLS, como os segredos que estão associados a ele.
{: shortdesc}

```
ibmcloud ks alb-cert-get --cluster CLUSTER [--secret-name SECRET_NAME] [--cert-crn CERTIFICATE_CRN] [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comandos</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>O nome do segredo do ALB. Esse valor é necessário para obter informações sobre um segredo do ALB específico no cluster.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>O CRN do certificado. Esse valor é necessário para obter informações sobre todos os segredos do ALB correspondentes a um CRN de certificado específico no cluster.</dd>

  <dt><code>--json</code></dt>
  <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
  </dl>

**Exemplos**:

 Exemplo para buscar informações sobre um segredo do ALB:

 ```
 ibmcloud ks alb-cert-get --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 Exemplo para buscar informações sobre todos os segredos do ALB que correspondem a um CRN de certificado especificado:

 ```
 ibmcloud ks alb-cert-get --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### ibmcloud ks alb-cert-rm
{: #cs_alb_cert_rm}

Se você importou um certificado do {{site.data.keyword.cloudcerts_short}} para o ALB em um cluster, remova o segredo do cluster.
{: shortdesc}

```
ibmcloud ks alb-cert-rm --cluster CLUSTER [--secret-name SECRET_NAME] [--cert-crn CERTIFICATE_CRN] [-s]
```
{: pre}

Para permanecer dentro dos [limites de taxa](https://cloud.ibm.com/apidocs/certificate-manager#rate-limiting) configurados por {{site.data.keyword.cloudcerts_short}}, aguarde pelo menos 45 segundos entre os comandos `alb-cert-rm` sucessivos.
{: note}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comandos</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>O nome do segredo do ALB. Esse valor é necessário para remover um segredo do ALB específico no cluster.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>O CRN do certificado. Esse valor é necessário para remover todos os segredos do ALB correspondentes a um CRN de certificado específico no cluster.</dd>

  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

  </dl>

**Exemplos**:

 Exemplo para remover um segredo do ALB:

 ```
 ibmcloud ks alb-cert-rm --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 Exemplo para remover todos os segredos do ALB que correspondem a um CRN de certificado especificado:

 ```
 ibmcloud ks alb-cert-rm --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### ibmcloud ks alb-certs
{: #cs_alb_certs}

Liste os certificados que você importou de sua instância do {{site.data.keyword.cloudcerts_long_notm}} para os ALBs em um cluster.
{: shortdesc}

```
ibmcloud ks alb-certs --cluster CLUSTER [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comandos</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
   <dt><code>--json</code></dt>
   <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>
   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

 ```
 ibmcloud ks alb-certs --cluster my_cluster
 ```
 {: pre}

### ibmcloud ks alb-configure
{: #cs_alb_configure}

Ative ou desative um ALB em seu cluster padrão. O ALB público é ativado por padrão.
{: shortdesc}

```
ibmcloud ks alb-configure --albID ALB_ID [--enable] [--user-ip USER_IP] [--disable] [--disable-deployment] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Opções de comando**:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>O ID para um ALB. Execute <code>ibmcloud ks albs <em>--cluster </em>CLUSTER</code> para visualizar os IDs para os ALBs em um cluster. Este valor é obrigatório.</dd>

   <dt><code>--enable</code></dt>
   <dd>Inclua essa sinalização para ativar um ALB em um cluster.</dd>

   <dt><code>--disable</code></dt>
   <dd>Inclua essa sinalização para desativar um ALB em um cluster.</dd>

   <dt><code> --disable-deployment </code></dt>
   <dd>Inclua essa sinalização para desativar a implementação do ALB fornecido pela IBM. Essa sinalização não remove o registro de DNS para o subdomínio do Ingress fornecido pela IBM ou o serviço de balanceador de carga que é usado para expor o controlador do Ingress.</dd>

   <dt><code>--user-ip <em>USER_IP</em></code></dt>
   <dd><ul>
     <li>Esse parâmetro está disponível para ativar um ALB privado apenas.</li>
     <li>O ALB privado é implementado com um endereço IP de uma sub-rede privada fornecida pelo usuário. Se nenhum endereço IP for fornecido, o ALB será implementado com um endereço IP privado da sub-rede privada móvel que foi provisionada automaticamente quando você criou o cluster.</li>
    </ul></dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplos**:

  Exemplo para ativar um ALB:

  ```
  ibmcloud ks alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable
  ```
  {: pre}

  Exemplo para ativar um ALB com um endereço IP fornecido pelo usuário:

  ```
  ibmcloud ks alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable --user-ip user_ip
  ```
  {: pre}

  Exemplo para desativar um ALB:

  ```
  ibmcloud ks alb-configure --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --disable
  ```
  {: pre}


### ibmcloud ks alb-get
{: #cs_alb_get}

Visualize os detalhes de um ALB.
{: shortdesc}

```
ibmcloud ks alb-get --albID ALB_ID [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>O ID para um ALB. Execute <code>ibmcloud ks albs --cluster <em>CLUSTER</em></code> para visualizar os IDs para os ALBs em um cluster. Este valor é obrigatório.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplo**:

  ```
  ibmcloud ks alb-get --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1
  ```
  {: pre}

### ibmcloud ks alb-rollback
{: #cs_alb_rollback}

Se os pods do ALB foram atualizados recentemente, mas uma configuração customizada para seus ALBs foi afetada pela construção mais recente, será possível recuperar a atualização para a construção em que os pods do ALB estavam em execução anteriormente. Depois de retroceder uma atualização, as atualizações automáticas para os pods do ALB são desativadas. Para reativar as atualizações automáticas, use o [comando `alb-autoupdate-enable`](#cs_alb_autoupdate_enable).
{: shortdesc}

```
ibmcloud ks alb-rollback --cluster CLUSTER
```
{: pre}

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Exemplo**:

```
ibmcloud ks alb-rollback --cluster mycluster
```
{: pre}

### ibmcloud ks alb-types
{: #cs_alb_types}

Visualize os tipos de ALB que são suportados na região.
{: shortdesc}

```
ibmcloud ks alb-types [ -- json ] [ -s ]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
  </dl>

**Exemplo**:

  ```
  ibmcloud ks alb-types
  ```
  {: pre}

### ibmcloud ks alb-update
{: #cs_alb_update}

Se as atualizações automáticas para o complemento ALB do Ingress estiverem desativadas e você desejar atualizar o complemento, será possível forçar uma atualização única de seus pods do ALB. Quando você escolhe atualizar manualmente o complemento, todos os pods do ALB no cluster são atualizados para a construção mais recente. Não é possível atualizar um ALB individual ou escolher para qual construção atualizar o complemento. As atualizações automáticas permanecem desativadas.
{: shortdesc}

```
ibmcloud ks alb-update --cluster CLUSTER
```
{: pre}

Quando você atualiza a versão principal ou secundária do Kubernetes de seu cluster, a IBM faz as mudanças necessárias automaticamente na implementação do Ingress, mas não muda a versão de construção de seu complemento ALB do Ingress. Você é responsável por verificar a compatibilidade das versões mais recentes do Kubernetes e das imagens do complemento ALB do Ingress.

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Exemplo**:

```
ibmcloud ks alb-update --cluster <cluster_name_or_ID>
```
{: pre}

### ibmcloud ks albs
{: #cs_albs}

Visualize o status de todos os ALBs em um cluster. Se nenhum ID de ALB for retornado, então, o cluster não terá uma sub-rede portátil. É possível [criar](#cs_cluster_subnet_create) ou [incluir](#cs_cluster_subnet_add) sub-redes em um cluster.
{: shortdesc}

```
ibmcloud ks albs --cluster CLUSTER [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>--cluster </em>CLUSTER</code></dt>
   <dd>O nome ou ID do cluster no qual você lista os ALBs disponíveis. Este valor é obrigatório.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplo**:

  ```
  ibmcloud ks albs --cluster my_cluster
  ```
  {: pre}


<br />



## Comandos de infraestrutura
{: #infrastructure_commands}

### ibmcloud ks credential-get
{: #cs_credential_get}

Se você configurar sua conta do IBM Cloud para usar credenciais diferentes para acessar o portfólio de infraestrutura do IBM Cloud, obtenha o nome de usuário da infraestrutura da região e do grupo de recursos para os quais você está direcionado atualmente.
{: shortdesc}

```
ibmcloud ks credential-get [ -s ] [ -- json ]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

 <dl>
 <dt><code>--json</code></dt>
 <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>
 <dt><code>-s</code></dt>
 <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
 </dl>

**Exemplo:
**
```
ibmcloud ks credential-get
```
{: pre}

**Saída de exemplo:**
```
Infrastructure credentials for user name user@email.com set for resource group default.
```

### ibmcloud ks credential-set
{: #cs_credentials_set}

Configure as credenciais de conta de infraestrutura do IBM Cloud (SoftLayer) para um grupo de recursos e região do {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

```
ibmcloud ks credential-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME [-s]
```
{: pre}

Se você tiver uma {{site.data.keyword.Bluemix_notm}}conta pré-paga, terá acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer) por padrão. No entanto, talvez você queira usar uma conta de infraestrutura do IBM Cloud (SoftLayer) diferente da que você já tem para pedir a infraestrutura. É possível vincular essa conta de infraestrutura à sua conta do {{site.data.keyword.Bluemix_notm}} conta usando este comando.

Se as credenciais de infraestrutura do IBM Cloud (SoftLayer) forem configuradas manualmente para uma região e um grupo de recursos, essas credenciais serão usadas para pedir a infraestrutura para todos os clusters dentro dessa região no grupo de recursos. Essas credenciais são usadas para determinar permissões de infraestrutura, mesmo que uma [chave de API do {{site.data.keyword.Bluemix_notm}} IAM](#cs_api_key_info) já exista para o grupo de recursos e a região. Se o usuário cujas credenciais estão armazenadas não tiver as permissões necessárias para pedir a infraestrutura, as ações relacionadas à infraestrutura, como a criação de um cluster ou o recarregamento de um nó do trabalhador, poderão falhar.

Não é possível configurar múltiplas credenciais para o mesmo grupo de recursos e região do {{site.data.keyword.containerlong_notm}}.

Antes de usar esse comando, certifique-se de que o usuário cujas credenciais são usadas tenha as permissões do [{{site.data.keyword.containerlong_notm}} e da infraestrutura do IBM Cloud (SoftLayer)](/docs/containers?topic=containers-users#users) necessárias.
{: important}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>Nome do usuário da API de conta de infraestrutura do IBM Cloud (SoftLayer). Este valor é obrigatório. Observe que o nome do usuário da API de infraestrutura não é o mesmo que o IBMid. Para visualizar o nome de usuário da API de infraestrutura:
   <ol>
   <li>Efetue login no [console do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com).
   <li>Na barra de menus, selecione **Gerenciar > Acesso (IAM)**.
   <li>Selecione a guia **Usuários** e, em seguida, clique em seu nome do usuário.
   <li>Na área de janela **Chaves de API**, localize a entrada **Chave de API de infraestrutura clássica** e clique no **Menu Ação** ![Ícone do menu Ação](../icons/action-menu-icon.svg "Ícone do menu Ação") **> Detalhes**.
   <li>Copie o nome do usuário da API.
   </ol></dd>


   <dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
   <dd>Chave API de infraestrutura do IBM Cloud infrastructure (SoftLayer). Este valor é obrigatório. Para visualizar ou gerar uma chave de API de infraestrutura:
  <li>Efetue login no [console do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com).
  <li>Na barra de menus, selecione **Gerenciar > Acesso (IAM)**.
  <li>Selecione a guia **Usuários** e, em seguida, clique em seu nome do usuário.
  <li>Na área de janela **Chaves de API**, localize a entrada **Chave de API de infraestrutura clássica** e clique no **Menu Ação** ![Ícone do menu Ação](../icons/action-menu-icon.svg "Ícone do menu Ação") **> Detalhes**. Se você não vir uma chave de API de infraestrutura clássica, gere uma clicando em **Criar uma chave de API do {{site.data.keyword.Bluemix_notm}}**.
  <li>Copie o nome do usuário da API.
  </ol>
  </dd>
  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

  </dl>

**Exemplo**:

  ```
  ibmcloud ks credential-set --infrastructure-api-key <api_key> --infrastructure-username dbmanager
  ```
  {: pre}


### ibmcloud ks credential-unset
{: #cs_credentials_unset}

Remova as credenciais de conta da infraestrutura do IBM Cloud (SoftLayer) de uma região do {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Depois de remover as credenciais, a [{{site.data.keyword.Bluemix_notm}}chave API do IAM](#cs_api_key_info) é usada para pedir recursos em infraestrutura do IBM Cloud (SoftLayer).

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

```
ibmcloud ks credential-unset [-s]
```
{: pre}

<strong>Opções de comando</strong>:

  <dl>
  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
  </dl>


### ibmcloud ks machine-types
{: #cs_machine_types}

Visualizar uma lista de tipos de máquina disponíveis para seus nós do trabalhador. Tipos de máquina variam por zona. Cada tipo de máquina inclui a
quantia de CPU, memória e espaço em disco virtual para cada nó do trabalhador no cluster. Por padrão, o diretório do disco de armazenamento secundário em que todos os dados do contêiner são armazenados, é criptografado com a criptografia AES de 256 bits LUKS. Se a opção `disable-disk-encrypt` for incluída durante a criação do cluster, os dados de tempo de execução do contêiner do host não serão criptografados. [ Saiba mais sobre a criptografia ](/docs/containers?topic=containers-security#encrypted_disk).
{:shortdesc}

```
ibmcloud ks machine-types --zone ZONE [--json] [-s]
```
{: pre}

É possível provisionar o nó do trabalhador como uma máquina virtual em hardware compartilhado ou dedicado ou como uma máquina física em bare metal. [Saiba mais sobre suas opções de tipo de máquina](/docs/containers?topic=containers-plan_clusters#shared_dedicated_node).

<strong> Permissões mínimas necessárias </strong>: nenhuma

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--zone <em>ZONE</em></code></dt>
   <dd>Insira a zona na qual você deseja listar os tipos de máquina disponíveis. Este valor é obrigatório. Revise [zonas disponíveis](/docs/containers?topic=containers-regions-and-zones#zones).</dd>

   <dt><code>--json</code></dt>
  <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
  </dl>

**Exemplo**:

  ```
  ibmcloud ks machine-types -- zona dal10
  ```
  {: pre}



### ibmcloud ks <ph class="ignoreSpelling">vlans</ph>
{: #cs_vlans}

Liste as VLANs públicas e privadas que estiverem disponíveis para uma zona em sua conta de infraestrutura do IBM Cloud (SoftLayer). Para listar as VLANs disponíveis, deve-se
ter uma conta paga.
{: shortdesc}

```
ibmcloud ks vlans --zone ZONE [--all] [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>:
* Para visualizar as VLANs às quais o cluster está conectado em uma zona: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}
* Para listar todas as VLANs disponíveis em uma zona: a função da plataforma **Visualizador** para a região no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--zone <em>ZONE</em></code></dt>
   <dd>Insira a zona na qual você deseja listar as suas VLANs privadas e públicas. Este valor é obrigatório. Revise [zonas disponíveis](/docs/containers?topic=containers-regions-and-zones#zones).</dd>

   <dt><code>--all</code></dt>
   <dd>Lista todas as VLANs disponíveis. Por padrão, as VLANs são filtradas para mostrar apenas as VLANs que são válidas. Para ser válida, uma VLAN deve ser associada à infraestrutura que pode hospedar um trabalhador com armazenamento em disco local.</dd>

   <dt><code>--json</code></dt>
  <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks vlans -- zona dal10
  ```
  {: pre}


### ibmcloud ks vlan-spanning-get
{: #cs_vlan_spanning_get}

Visualize o status do VLAN Spanning para uma conta de infraestrutura do IBM Cloud (SoftLayer). A ampliação de VLAN permite que todos os dispositivos em uma conta se comuniquem entre si por meio da rede privada, independentemente de sua VLAN designada.
{: shortdesc}

```
ibmcloud ks vlan-spanning-get [ -- json ] [ -s ]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
    <dt><code>--json</code></dt>
      <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

    <dt><code>-s</code></dt>
      <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks vlan-spanning-get
  ```
  {: pre}

<br />


## Comandos de criação de log
{: #logging_commands}

### ibmcloud ks logging-autoupdate-disable
{: #cs_log_autoupdate_disable}

Desative as atualizações automáticas de seus pods do Fluentd em um cluster específico. Quando você atualiza a versão principal ou secundária do Kubernetes de seu cluster, a IBM automaticamente faz mudanças necessárias no Fluentd configmap, mas não muda a versão de construção de seu Fluentd para criação de log de complemento. Você é responsável por verificar a compatibilidade das versões mais recentes do Kubernetes e de suas imagens de complemento.
{: shortdesc}

```
ibmcloud ks logging-autoupdate-disable --cluster CLUSTER
```
{: pre}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou o ID do cluster no qual você deseja desativar as atualizações automáticas para o complemento Fluentd. Este valor é obrigatório.</dd>
</dl>

### ibmcloud ks logging-autoupdate-enable
{: #cs_log_autoupdate_enable}

Ative atualizações automáticas para seus pods Fluentd em um cluster específico. Os pods do Fluentd são atualizados automaticamente quando uma nova versão de construção está disponível.
{: shortdesc}

```
ibmcloud ks logging-autoupdate-enable -- cluster CLUSTER
```
{: pre}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou o ID do cluster no qual você deseja ativar atualizações automáticas para o complemento Fluentd. Este valor é obrigatório.</dd>
</dl>

### ibmcloud ks logging-autoupdate-get
{: #cs_log_autoupdate_get}

Verifique se os pods do Fluentd estão configurados para serem atualizados automaticamente em um cluster específico.
{: shortdesc}

```
ibmcloud ks logging-autoupdate-get -- cluster CLUSTER
```
{: pre}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou o ID do cluster no qual você deseja verificar se as atualizações automáticas para o complemento Fluentd estão ativadas. Este valor é obrigatório.</dd>
</dl>

### ibmcloud ks logging-config-create
{: #cs_logging_create}

Crie uma configuração de criação de log. É possível usar esse comando para encaminhar logs para contêineres, aplicativos, nós do trabalhador, clusters do Kubernetes e balanceadores de carga do aplicativo Ingress para o {{site.data.keyword.loganalysisshort_notm}} ou para um servidor syslog externo.
{: shortdesc}

```
ibmcloud ks logging-config-create --cluster CLUSTER --logsource LOG_SOURCE --type LOG_TYPE [--namespace KUBERNETES_NAMESPACE] [--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT] [--space CLUSTER_SPACE] [--org CLUSTER_ORG] [--app-containers CONTAINERS] [--app-paths PATHS_TO_LOGS] [--syslog-protocol PROTOCOL]  [--json] [--skip-validation] [--force-update][-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Editor** para o cluster para todas as origens de log, exceto `kube-audit`, e a função da plataforma **Administrador** para o cluster para a origem de log `kube-audit`

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster.</dd>

  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>A origem do log para o qual ativar o encaminhamento de log. Esse argumento suporta uma lista separada por vírgula de origens de log para aplicar a configuração. Os valores aceitos são <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>storage</code>, <code>ingress</code> e <code>kube-audit</code>. Se você não fornecer uma origem de log, configurações serão criadas para <code>container</code> e <code>ingress</code>.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>Onde você deseja encaminhar os logs. As opções são <code>ibm</code>, que encaminha os logs para o {{site.data.keyword.loganalysisshort_notm}} e <code>syslog</code>, que encaminha os logs para um servidor externo.</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>O namespace do Kubernetes do qual você deseja encaminhar logs. O encaminhamento de log não é suportado para os namespaces do Kubernetes <code>ibm-system</code> e <code>kube-system</code>. Esse valor é válido somente para a origem de log do contêiner e é opcional. Se você não especificar um namespace, todos os namespaces no cluster usarão essa configuração.</dd>

  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
    <dd>Quando o tipo de criação de log for <code>syslog</code>, o nome do host ou endereço IP do servidor do coletor de log. Esse valor é necessário para <code>syslog</code>. Quando o tipo de criação de log for <code>ibm</code>, a URL de ingestão {{site.data.keyword.loganalysislong_notm}}. É possível localizar a lista de URLs de ingestão disponíveis
[aqui](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_ingestion#log_ingestion_urls). Se você não especificar uma URL de ingestão, o endpoint para a região na qual seu cluster foi criado será usado.</dd>

  <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
    <dd>A porta do servidor coletor do log. Esse valor é opcional. Se você não especificar uma porta, a porta padrão <code>514</code> será usada para <code>syslog</code> e a porta padrão <code>9091</code> será usada para <code>ibm</code>.</dd>

  <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
    <dd>Opcional: o nome do espaço do Cloud Foundry para o qual você deseja enviar logs. Esse valor é válido somente para o tipo de log <code>ibm</code> e é opcional. Se você não especificar um espaço, os logs serão enviados para o nível de conta. Caso afirmativo, deve-se também especificar uma organização.</dd>

  <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
    <dd>Opcional: o nome da organização do Cloud Foundry na qual o espaço estiver. Esse valor é válido somente para o tipo de log <code>ibm</code> e é necessário se você especificou um espaço.</dd>

  <dt><code>--app-paths</code></dt>
    <dd>O caminho no contêiner no qual os apps estão efetuando login. Para encaminhar logs com tipo de origem <code>application</code>, deve-se fornecer um caminho. Para especificar mais de um caminho, use uma lista separada por vírgula. Esse valor é necessário para origem de log <code>application</code>. Exemplo: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

  <dt><code>-- syslog-protocol</code></dt>
    <dd>O protocolo de camada de transferência que será usado quando o tipo de criação de log for <code>syslog</code>. Os valores suportados são <code>tcp</code>, <code>tls</code> e o <code>udp</code> padrão. Ao encaminhar para um servidor rsyslog com o protocolo <code>udp</code>, os logs com mais de 1 KB serão truncados.</dd>

  <dt><code>--app-containers</code></dt>
    <dd>Para encaminhar logs de apps, é possível especificar o nome do contêiner que contém o seu app. É possível especificar mais de um contêiner usando uma lista separada por vírgula. Se nenhum contêiner é especificado, os logs são encaminhados de todos os contêineres que contêm os caminhos que você forneceu. Essa opção é válida somente para a origem de log <code>application</code>.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>--skip-validation</code></dt>
    <dd>Ignore a validação dos nomes da organização e do espaço quando são especificados. Ignorar a validação diminui o tempo de processamento, mas uma configuração de criação de log inválida não encaminhará os logs corretamente. Esse valor é opcional.</dd>

  <dt><code>--force-update</code></dt>
    <dd>Force os seus pods do Fluentd para serem atualizados para a versão mais recente. O Fluentd deve estar na versão mais recente para fazer mudanças em suas configurações de criação de log.</dd>

    <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplos**:

Exemplo para o tipo de log `ibm` que encaminha de uma origem de log `container` na porta padrão 9091:

  ```
  ibmcloud ks logging-config-create my_cluster --logsource container --namespace my_namespace --hostname ingest.logging.ng.bluemix.net --type ibm
  ```
  {: pre}

Exemplo para o tipo de log `syslog` que encaminha de uma origem de log `container` na porta padrão 514:

  ```
  ibmcloud ks logging-config-create my_cluster --logsource container --namespace my_namespace --hostname 169.xx.xxx.xxx --type syslog
  ```
  {: pre}

Exemplo para o tipo de log `syslog` que encaminha logs de uma origem `ingress` em uma porta diferente do padrão:

  ```
  ibmcloud ks logging-config-create --cluster my_cluster --logsource container --hostname 169.xx.xxx.xxx --port 5514 --type syslog
  ```
  {: pre}


### ibmcloud ks logging-config-get
{: #cs_logging_get}

Visualize todas as configurações de encaminhamento de log para um cluster ou filtre configurações de criação de log com base em origem de log.
{: shortdesc}

```
ibmcloud ks logging-config-get --cluster CLUSTER [--logsource LOG_SOURCE] [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

 <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>O tipo de origem de log para a qual você deseja filtrar. Apenas as configurações de criação de log dessa origem de log no cluster são retornadas. Os valores aceitos são <code>container</code>, <code>storage</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>ingress</code> e <code>kube-audit</code>. Esse valor é opcional.</dd>

  <dt><code>--show-cobrindo-filtros</code></dt>
    <dd>Mostra os filtros de criação de log que tornam os filtros prévios obsoletos.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
 </dl>

**Exemplo**:

  ```
  ibmcloud ks logging-config-get --cluster my_cluster --logsource worker
  ```
  {: pre}


### ibmcloud ks logging-config-refresh
{: #cs_logging_refresh}

Atualize a configuração de criação de log para o cluster. Isso atualiza o token de criação de log para qualquer configuração de criação de log que está encaminhando para o nível de espaço em seu cluster.
{: shortdesc}

```
ibmcloud ks logging-config-refresh --cluster CLUSTER [--force-update] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--force-update</code></dt>
     <dd>Force os seus pods do Fluentd para serem atualizados para a versão mais recente. O Fluentd deve estar na versão mais recente para fazer mudanças em suas configurações de criação de log.</dd>

   <dt><code>-s</code></dt>
     <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks logging-config-refresh --cluster my_cluster
  ```
  {: pre}



### ibmcloud ks logging-config-rm
{: #cs_logging_rm}

Excluir uma configuração de encaminhamento de log ou todas as configurações de criação de log para um cluster. Isso para o encaminhamento de log para um servidor syslog remoto ou {{site.data.keyword.loganalysisshort_notm}}.
{: shortdesc}

```
ibmcloud ks logging-config-rm --cluster CLUSTER [--id LOG_CONFIG_ID] [--all] [--force-update] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Editor** para o cluster para todas as origens de log, exceto `kube-audit`, e a função da plataforma **Administrador** para o cluster para a origem de log `kube-audit`

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>Se você deseja remover uma configuração de criação de log única, o ID de configuração de criação de log.</dd>

  <dt><code>--all</code></dt>
   <dd>A sinalização para remover todas as configurações de criação de log em um cluster.</dd>

  <dt><code>--force-update</code></dt>
    <dd>Force os seus pods do Fluentd para serem atualizados para a versão mais recente. O Fluentd deve estar na versão mais recente para fazer mudanças em suas configurações de criação de log.</dd>

   <dt><code>-s</code></dt>
     <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks logging-config-rm --cluster my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e
  ```
  {: pre}


### ibmcloud ks logging-config-update
{: #cs_logging_update}

Atualize os detalhes de uma configuração de encaminhamento de log.
{: shortdesc}

```
ibmcloud ks logging-config-update --cluster CLUSTER --id LOG_CONFIG_ID --type LOG_TYPE  [--namespace NAMESPACE] [--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT] [--space CLUSTER_SPACE] [--org CLUSTER_ORG] [--app-paths PATH] [--app-containers PATH] [--json] [--skipValidation] [--force-update] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>O ID de configuração de criação de log que você deseja atualizar. Este valor é obrigatório.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
   <dd>O protocolo de encaminhamento de log que você deseja usar. Atualmente, <code>syslog</code> e <code>ibm</code> são suportados. Este valor é obrigatório.</dd>

  <dt><code>--namespace <em>NAMESPACE</em></code>
    <dd>O namespace do Kubernetes do qual você deseja encaminhar logs. O encaminhamento de log não é suportado para os namespaces do Kubernetes <code>ibm-system</code> e <code>kube-system</code>. Esse valor é válido somente para a origem de log do <code>container</code>. Se você não especificar um namespace, todos os namespaces no cluster usarão essa configuração.</dd>

  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
   <dd>Quando o tipo de criação de log for <code>syslog</code>, o nome do host ou endereço IP do servidor do coletor do log. Esse valor é necessário para <code>syslog</code>. Quando o tipo de criação de log for <code>ibm</code>, a URL de ingestão {{site.data.keyword.loganalysislong_notm}}. É possível localizar a lista de URLs de ingestão disponíveis
[aqui](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_ingestion#log_ingestion_urls). Se você não especificar uma URL de ingestão, o endpoint para a região na qual seu cluster foi criado será usado.</dd>

   <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
   <dd>A porta do servidor coletor do log. Esse valor será opcional quando o tipo de criação de log for <code>syslog</code>. Se você não especificar uma porta, a porta padrão <code>514</code> será usada para <code>syslog</code> e <code>9091</code>
será usada para <code>ibm</code>.</dd>

   <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
   <dd>Opcional: o nome do espaço para o qual você deseja enviar logs. Esse valor é válido somente para o tipo de log <code>ibm</code> e é opcional. Se você não especificar um espaço, os logs serão enviados para o nível de conta. Caso afirmativo, deve-se também especificar uma organização.</dd>

   <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
   <dd>Opcional: o nome da organização do Cloud Foundry na qual o espaço estiver. Esse valor é válido somente para o tipo de log <code>ibm</code> e é necessário se você especificou um espaço.</dd>

   <dt><code>-- app-caminhos <em>PATH</em>,<em>PATH</em></code></dt>
     <dd>Um caminho de arquivo absoluto no contêiner para coletar logs. Curingas, como '/var/log/*.log', podem ser usados, mas trechos recursivos, como '/var/log/**/test.log', não podem ser usados. Para especificar mais de um caminho, use uma lista separada por vírgula. Esse valor é obrigatório quando você especifica 'application' para a origem de log. </dd>

   <dt><code>-- app-containers <em>PATH</em>,<em>PATH</em></code></dt>
     <dd>O caminho nos contêineres nos quais os apps estão registrando. Para encaminhar logs com tipo de origem <code>application</code>, deve-se fornecer um caminho. Para especificar mais de um caminho, use uma lista separada por vírgula. Exemplo: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

   <dt><code>--json</code></dt>
    <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

   <dt><code>--skipValidation</code></dt>
    <dd>Ignore a validação dos nomes da organização e do espaço quando são especificados. Ignorar a validação diminui o tempo de processamento, mas uma configuração de criação de log inválida não encaminhará os logs corretamente. Esse valor é opcional.</dd>

  <dt><code>--force-update</code></dt>
    <dd>Force os seus pods do Fluentd para serem atualizados para a versão mais recente. O Fluentd deve estar na versão mais recente para fazer mudanças em suas configurações de criação de log.</dd>

   <dt><code>-s</code></dt>
     <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
  </dl>

**Exemplo para o tipo de log `ibm`**:

  ```
  ibmcloud ks logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**Exemplo para o tipo de log `syslog`**:

  ```
  ibmcloud ks logging-config-update --cluster my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}



### ibmcloud ks logging-filter-create
{: #cs_log_filter_create}

Crie um filtro de criação de log. É possível usar esse comando para filtrar logs que são encaminhados por sua configuração de criação de log.
{: shortdesc}

```
ibmcloud ks logging-filter-create --cluster CLUSTER --type LOG_TYPE [--logging-configs CONFIGS] [--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME] [--level LOGGING_LEVEL] [--message MESSAGE] [--regex-message MESSAGE] [--force-update] [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster para o qual você deseja criar um filtro de criação de log. Este valor é obrigatório.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>O tipo de logs nos quais você deseja aplicar o filtro. Atualmente <code>all</code>, <code>container</code> e <code>host</code> são suportados.</dd>

  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>Uma lista separada por vírgula de seus IDs de configuração de criação de log. Se não fornecido, o filtro será aplicado a todas as configurações de criação de log de cluster que forem passadas para o filtro. É possível visualizar as configurações de log que correspondem ao filtro usando a sinalização <code>--show-matching-configs</code> com o comando. Esse valor é opcional.</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>O namespace do Kubernetes do qual se deseja filtrar logs. Esse valor é opcional.</dd>

  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>O nome do contêiner do qual se deseja filtrar logs. Essa sinalização se aplicará apenas quando você estiver usando o tipo de log <code>container</code>. Esse valor é opcional.</dd>

  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>Filtra logs que estão no nível especificado e inferior. Os valores aceitáveis na ordem canônica são <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> e <code>trace</code>. Esse valor é opcional. Como um exemplo, se você filtrou logs no nível <code>info</code>, <code>debug</code> e <code>trace</code> também serão filtrados. **Nota**: é possível usar essa sinalização apenas quando as mensagens de log estiverem em formato JSON e contiverem um campo de nível. Saída de exemplo: <code>{"log": "hello", "level": "info"}</code></dd>

  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>Filtra quaisquer logs que contêm uma mensagem especificada em qualquer lugar no log. Esse valor é opcional. Exemplo: As mensagens "Hello", "!"e "Hello, World!"se aplicariam ao log "Hello, World!".</dd>

  <dt><code>--regex-message <em>MESSAGE</em></code></dt>
    <dd>Filtra quaisquer logs que contêm uma mensagem especificada que é gravada como uma expressão regular em qualquer lugar no log. Esse valor é opcional. Exemplo: o padrão "hello [0-9]" se aplicaria a "hello 1", "hello 2" e "hello 9".</dd>

  <dt><code>--force-update</code></dt>
    <dd>Force os seus pods do Fluentd para serem atualizados para a versão mais recente. O Fluentd deve estar na versão mais recente para fazer mudanças em suas configurações de criação de log.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplos**:

Este exemplo filtra todos os logs que são encaminhados de contêineres com o nome `test-container` no namespace padrão que estão no nível de depuração ou menos e têm uma mensagem de log que contém "solicitação GET".

  ```
  ibmcloud ks logging-filter-create --cluster example-cluster --type container --namespace default --container test-container --level debug --message "GET request"
  ```
  {: pre}

Este exemplo filtra todos os logs que são encaminhados, em um nível `info` ou inferior, de um cluster específico. A saída é retornada como JSON.

  ```
  ibmcloud ks logging-filter-create --cluster example-cluster --type all --level info --json
  ```
  {: pre}



### ibmcloud ks logging-filter-get
{: #cs_log_filter_view}

Visualize uma configuração de filtro de criação de log. É possível usar esse comando para visualizar os filtros de criação de log que você criou.
{: shortdesc}

```
ibmcloud ks logging-filter-get --cluster CLUSTER [--id FILTER_ID] [--show-matching-configs] [--show-covering-filters] [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster do qual você deseja visualizar filtros. Este valor é obrigatório.</dd>

  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>O ID do filtro de log que você deseja visualizar.</dd>

  <dt><code>--show-matching-configs</code></dt>
    <dd>Mostre as configurações de criação de log que correspondem à configuração que você está visualizando. Esse valor é opcional.</dd>

  <dt><code>--show-cobrindo-filtros</code></dt>
    <dd>Mostre os filtros de criação de log que tornam os filtros prévios obsoletos. Esse valor é opcional.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

   <dt><code>-s</code></dt>
     <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

```
ibmcloud ks logging-filter-get mycluster --id 885732 --show-matching-configs
```
{: pre}

### ibmcloud ks logging-filter-rm
{: #cs_log_filter_delete}

Exclua um filtro de criação de log É possível usar esse comando para remover um filtro de criação de log que você criou.
{: shortdesc}

```
ibmcloud ks logging-filter-rm --cluster CLUSTER [--id FILTER_ID] [--all] [--force-update] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster por meio do qual você deseja excluir um filtro.</dd>

  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>O ID do filtro de log a ser excluído.</dd>

  <dt><code>--all</code></dt>
    <dd>Exclua todos os seus filtros de encaminhamento de log. Esse valor é opcional.</dd>

  <dt><code>--force-update</code></dt>
    <dd>Force os seus pods do Fluentd para serem atualizados para a versão mais recente. O Fluentd deve estar na versão mais recente para fazer mudanças em suas configurações de criação de log.</dd>

  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

```
ibmcloud ks logging-filter-rm mycluster -- id 885732
```
{: pre}

### ibmcloud ks logging-filter-update
{: #cs_log_filter_update}

Atualize um filtro de criação de log. É possível usar esse comando para atualizar um filtro de criação de log que você criou.
{: shortdesc}

```
ibmcloud ks logging-filter-update --cluster CLUSTER --id FILTER_ID --type LOG_TYPE [--logging-configs CONFIGS] [--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME] [--level LOGGING_LEVEL] [--message MESSAGE] [--regex-message MESSAGE] [--force-update] [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster para o qual você deseja atualizar um filtro de criação de log. Este valor é obrigatório.</dd>

 <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>O ID do filtro de log a ser atualizado.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>O tipo de logs nos quais você deseja aplicar o filtro. Atualmente <code>all</code>, <code>container</code> e <code>host</code> são suportados.</dd>

  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>Uma lista separada por vírgula de seus IDs de configuração de criação de log. Se não fornecido, o filtro será aplicado a todas as configurações de criação de log de cluster que forem passadas para o filtro. É possível visualizar as configurações de log que correspondem ao filtro usando a sinalização <code>--show-matching-configs</code> com o comando. Esse valor é opcional.</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>O namespace do Kubernetes do qual se deseja filtrar logs. Esse valor é opcional.</dd>

  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>O nome do contêiner do qual se deseja filtrar logs. Essa sinalização se aplicará apenas quando você estiver usando o tipo de log <code>container</code>. Esse valor é opcional.</dd>

  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>Filtra logs que estão no nível especificado e inferior. Os valores aceitáveis na ordem canônica são <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> e <code>trace</code>. Esse valor é opcional. Como um exemplo, se você filtrou logs no nível <code>info</code>, <code>debug</code> e <code>trace</code> também serão filtrados. **Nota**: é possível usar essa sinalização apenas quando as mensagens de log estiverem em formato JSON e contiverem um campo de nível. Saída de exemplo: <code>{"log": "hello", "level": "info"}</code></dd>

  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>Filtra quaisquer logs que contêm uma mensagem especificada em qualquer lugar no log. Esse valor é opcional. Exemplo: As mensagens "Hello", "!"e "Hello, World!"se aplicariam ao log "Hello, World!".</dd>

  <dt><code>--regex-message <em>MESSAGE</em></code></dt>
    <dd>Filtra quaisquer logs que contêm uma mensagem especificada que é gravada como uma expressão regular em qualquer lugar no log. Esse valor é opcional. Exemplo: o padrão "hello [0-9]" se aplicaria a "hello 1", "hello 2" e "hello 9"</dd>

  <dt><code>--force-update</code></dt>
    <dd>Force os seus pods do Fluentd para serem atualizados para a versão mais recente. O Fluentd deve estar na versão mais recente para fazer mudanças em suas configurações de criação de log.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplos**:

Este exemplo filtra todos os logs que são encaminhados de contêineres com o nome `test-container` no namespace padrão que estão no nível de depuração ou menos e têm uma mensagem de log que contém "solicitação GET".

  ```
  ibmcloud ks logging-filter-update --cluster example-cluster --id 885274 --type container --namespace default --container test-container --level debug --message "GET request"
  ```
  {: pre}

Este exemplo filtra todos os logs que são encaminhados, em um nível `info` ou inferior, de um cluster específico. A saída é retornada como JSON.

  ```
  ibmcloud ks logging-filter-update --cluster example-cluster --id 274885 --type all --level info --json
  ```
  {: pre}

### ibmcloud ks logging-collect
{: #cs_log_collect}

Faça uma solicitação para obter uma captura instantânea de seus logs em um momento específico e, em seguida, armazene os logs em um bucket do {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

```
ibmcloud ks logging-collect --cluster CLUSTER --cos-bucket BUCKET_NAME --cos-endpoint ENDPOINT --hmac-key-id HMAC_KEY_ID --hmac-key HMAC_KEY --type LOG_TYPE [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster para o qual você deseja criar uma captura instantânea. Este valor é obrigatório.</dd>

 <dt><code>--cos-bucket <em>BUCKET_NAME</em></code></dt>
    <dd>O nome do bucket do {{site.data.keyword.cos_short}} no qual você deseja armazenar os logs. Este valor é obrigatório.</dd>

  <dt><code> -- cos-endpoint  <em> ENDPOINT </em> </code></dt>
    <dd>O terminal do {{site.data.keyword.cos_short}} para o depósito no qual você está armazenando os logs. Este valor é obrigatório.</dd>

  <dt><code>--hmac-key-id  <em> HMAC_KEY_ID </em> </code></dt>
    <dd>O ID exclusivo para suas credenciais HMAC para a instância do Object Storage. Este valor é obrigatório.</dd>

  <dt><code> -- hmac-key  <em> HMAC_KEY </em> </code></dt>
    <dd>A chave HMAC para sua instância do  {{site.data.keyword.cos_short}} . Este valor é obrigatório.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>O tipo de log do qual você deseja criar uma captura instantânea. Atualmente, `master` é a única opção, bem como a padrão.</dd>

  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks logging-collect --cluster mycluster --cos-bucket mybucket --cos-endpoint s3-api.us-geo.objectstorage.softlayer.net --hmac-key-id e2e7f5c9fo0144563c418dlhi3545m86 --hmac-key c485b9b9fo4376722f692b63743e65e1705301ab051em96j
  ```
  {: pre}

**Saída de exemplo**:

  ```
  Não há nenhum tipo de log especificado. O mestre padrão será usado.
  Enviando a solicitação de coleção de logs para logs principais para o cluster mycluster... OK A solicitação de coleção de logs foi enviada com êxito. Para visualizar o status da solicitação, execute ibmcloud ks logging-collect-status mycluster.
  ```
  {: screen}


### ibmcloud ks logging-collect-status
{: #cs_log_collect_status}

Verifique o status da solicitação de captura instantânea de coleção de logs para seu cluster.
{: shortdesc}

```
ibmcloud ks logging-collect-status --cluster CLUSTER [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Administrador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster para o qual você deseja criar uma captura instantânea. Este valor é obrigatório.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks logging-collect-status -- cluster mycluster
  ```
  {: pre}

**Saída de exemplo**:

  ```
  Getting the status of the last log collection request for cluster mycluster...
  OK
  State     Start Time             Error   Log URLs
  success   2018-09-18 16:49 PDT   - s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-0-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-1-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-2-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  ```
  {: screen}

<br />


## Comandos do balanceador de carga de rede (`nlb-dns`)
{: #nlb-dns}

Use esse grupo de comandos para criar e gerenciar nomes de host para endereços IP do balanceador de carga de rede (NLB) e monitores de verificação de funcionamento para nomes de host. Para obter mais informações, consulte [Registrando um nome de host de balanceador de carga](/docs/containers?topic=containers-loadbalancer#loadbalancer_hostname).
{: shortdesc}

### ibmcloud ks nlb-dns-add
{: #cs_nlb-dns-add}

Inclua um IP de balanceador de carga de rede (NLB) em um nome de host existente criado com o comando [`ibmcloud ks nlb-dns-create`](#cs_nlb-dns-create).
{: shortdesc}

```
ibmcloud ks nlb-dns-add --cluster CLUSTER --ip IP --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

Por exemplo, é possível criar um NLB em cada zona de um cluster multizona para expor um aplicativo. Primeiro, você registra um IP de NLB em uma zona com um nome de host executando `ibmcloud ks nlb-dns-create`, em seguida, é possível incluir os IPs do NLB de outra zona nesse nome de host existente. Quando um usuário acessa o seu nome do host do app, o cliente acessa um desses IPs aleatoriamente e a solicitação é enviada para esse NLB. Observe que é necessário executar o comando a seguir para cada endereço IP que você deseja incluir.

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Opções de comando**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--ip <em>IP</em></code></dt>
<dd>O IP do NLB que você deseja incluir no nome de host. Para ver os seus IPs do NLB, execute <code>kubectl get svc</code>.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>O nome de host no qual você deseja incluir IPs. Para ver os nomes de host existentes, execute <code>ibmcloud ks nlb-dnss</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

<dt><code>-s</code></dt>
<dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks nlb-dns-add --cluster mycluster --ip 1.1.1.1 --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

### ibmcloud ks nlb-dns-create
{: #cs_nlb-dns-create}

Exponha seu aplicativo publicamente criando um nome de host DNS para registrar um IP de balanceador de carga de rede (NLB).
{: shortdesc}

```
ibmcloud ks nlb-dns-create --cluster CLUSTER --ip IP [--json] [-s]
```
{: pre}

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Opções de comando**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--ip <em>IP</em></code></dt>
<dd>O endereço IP do balanceador de carga de rede que você deseja registrar. Para ver os seus IPs do NLB, execute <code>kubectl get svc</code>. Observe que é possível criar inicialmente o nome do host com apenas um endereço IP. Se tiver NLBs em cada zona de um cluster multizona que exponham um aplicativo, será possível incluir os IPs de outros NLBs no nome de host executando o [comando `ibmcloud ks nlb-dns-add`](#cs_nlb-dns-add).</dd>

<dt><code>--json</code></dt>
<dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

<dt><code>-s</code></dt>
<dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks nlb-dns-create --cluster mycluster --ip 1.1.1.1
```
{: pre}

### ibmcloud ks nlb-dnss
{: #cs_nlb-dns-ls}

Liste os nomes de host e os endereços IP do balanceador de carga de rede registrados em um cluster.
{: shortdesc}

```
ibmcloud ks nlb-dnss --cluster CLUSTER [--json] [-s]
```
{: pre}

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Opções de comando**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--json</code></dt>
<dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

<dt><code>-s</code></dt>
<dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks nlb-dnss --cluster mycluster
```
{: pre}

### ibmcloud ks nlb-dns-rm
{: #cs_nlb-dns-rm}

Remova um endereço IP de balanceador de carga de rede de um nome de host. Se você remover todos os IPs de um nome do host, o nome do host ainda existirá, mas nenhum IP será associado a ele. Observe que se deve executar esse comando para cada endereço IP que você deseja remover.
{: shortdesc}

```
ibmcloud ks nlb-dns-rm --cluster CLUSTER --ip IP --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Opções de comando**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--ip <em>IP</em></code></dt>
<dd>O IP do NLB que você deseja remover. Para ver os seus IPs do NLB, execute <code>kubectl get svc</code>.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>O nome de host do qual você deseja remover um IP. Para ver os nomes de host existentes, execute <code>ibmcloud ks nlb-dnss</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

<dt><code>-s</code></dt>
<dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks nlb-dns-rm --cluster mycluster --ip 1.1.1.1 --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

### ibmcloud ks nlb-dns-monitor
{: #cs_nlb-dns-monitor}

Crie, modifique e visualize monitores de verificação de funcionamento para nomes de host de balanceador de carga de rede em um cluster. Esse comando deve ser combinado com um dos subcomandos a seguir.
{: shortdesc}

#### ibmcloud ks nlb-dns-monitor-configure
{: #cs_nlb-dns-monitor-configure}

Configure e, opcionalmente, ative um monitor de verificação de funcionamento para um nome de host do NLB existente em um cluster. Quando você ativa um monitor para seu nome de host, ele verifica o funcionamento do IP do NLB em cada zona e mantém os resultados da consulta de DNS atualizados com base nessas verificações de funcionamento.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-configure --cluster CLUSTER --nlb-host HOST NAME [--enable] [--desc DESCRIPTION] [--type TYPE] [--method METHOD] [--path PATH] [--timeout TIMEOUT] [--retries RETRIES] [--interval INTERVAL] [--port PORT] [--header HEADER] [--expected-body BODY STRING] [--expected-codes HTTP CODES] [--follows-redirects TRUE] [--allows-insecure TRUE] [--json] [-s]
```
{: pre}

É possível usar esse comando para criar e ativar um novo monitor de verificação de funcionamento ou para atualizar as configurações para um monitor de verificação de funcionamento existente. Para criar um novo monitor, inclua o sinalizador `-- enable` e os sinalizadores para todas as configurações que deseja definir. Para atualizar um monitor existente, inclua apenas os sinalizadores para as configurações que deseja mudar.

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Opções de comando**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster no qual o nome de host está registrado.</dd>

<dt><code>--nlb-host <em>HOST NAME</em></code></dt>
<dd>O nome de host para o qual configurar um monitor de verificação de funcionamento. Para listar nomes de host, execute <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>.</dd>

<dt><code>--enable</code></dt>
<dd>Inclua esse sinalizador para criar e ativar um novo monitor de verificação de funcionamento para um nome de host.</dd>

<dt><code>--description <em>DESCRIPTION</em></code></dt>
<dd>Uma descrição do monitor de funcionamento.</dd>

<dt><code>--type <em>TYPE</em></code></dt>
<dd>O protocolo a ser usado para a verificação de funcionamento: <code>HTTP</code>, <code>HTTPS</code> ou <code>TCP</code>. Padrão: <code>HTTP</code></dd>

<dt><code>--method <em>METHOD</em></code></dt>
<dd>O método a ser usado para a verificação de funcionamento. Padrão para <code>type</code> <code>HTTP</code> e <code>HTTPS</code>: <code>GET</code>. Padrão para <code>type</code> <code>TCP</code>: <code>connection_established</code>.</dd>

<dt><code>--path <em>PATH</em></code></dt>
<dd>Quando <code>type</code> for <code>HTTPS</code>: o caminho de terminal com relação ao qual será feita a verificação de funcionamento. Padrão: <code>/</code></dd>

<dt><code>--timeout <em>TIMEOUT</em></code></dt>
<dd>O tempo limite, em segundos, antes que o IP seja considerado não funcional. Padrão: <code>5</code></dd>

<dt><code>--retries <em>RETRIES</em></code></dt>
<dd>Quando ocorrer um tempo limite, o número de novas tentativas para tentar antes do IP será considerado não funcional. Novas tentativas são tentadas imediatamente. Padrão: <code>2</code></dd>

<dt><code>--interval <em>INTERVAL</em></code></dt>
<dd>O intervalo, em segundos, entre cada verificação de funcionamento. Intervalos curtos podem melhorar o tempo de failover, mas aumentar o carregamento nos IPs. Padrão: <code>60</code></dd>

<dt><code>--port <em>PORT</em></code></dt>
<dd>O número da porta à qual se conectar para a verificação de funcionamento. Quando <code>type</code> for <code>TCP</code>, esse parâmetro será necessário. Quando <code>type</code> for <code>HTTP</code> ou <code>HTTPS</code>, defina a porta somente se você usar uma porta diferente de 80 para HTTP ou 443 para HTTPS. Padrão para TCP: <code>0</code>. Padrão para HTTP: <code>80</code>. Padrão para HTTPS: <code>443</code>.</dd>

<dt><code>--header <em>HEADER</em></code></dt>
<dd>Quando <code>type</code> é <code>HTTPS</code> ou <code>HTTPS</code>: os cabeçalhos da solicitação de HTTP a serem enviados na verificação de funcionamento, como um cabeçalho de Host. Não é possível substituir o cabeçalho do Agente do Usuário.</dd>

<dt><code>--expected-body <em>BODY STRING</em></code></dt>
<dd>Quando <code>type</code> for <code>HTTP</code> ou <code>HTTPS</code>: uma subsequência sem distinção entre maiúsculas e minúsculas que a verificação de funcionamento procurará no corpo de resposta. Se essa sequência não for localizada, o IP será considerado não funcional.</dd>

<dt><code>--expected-codes <em>HTTP CODES</em></code></dt>
<dd>Quando <code>type</code> for <code>HTTP</code> ou <code>HTTPS</code>: códigos de HTTP que a verificação de funcionamento procurará na resposta. Se o código de HTTP não for localizado, o IP será considerado não funcional. Padrão: <code>2xx</code></dd>

<dt><code>--allows-insecure <em>TRUE</em></code></dt>
<dd>Quando <code>type</code> for <code>HTTP</code> ou <code>HTTPS</code>: configure como <code>true</code> para não validar o certificado.</dd>

<dt><code>--follows-redirects <em>TRUE</em></code></dt>
<dd>Quando <code>type</code> for <code>HTTP</code> ou <code>HTTPS</code>: configure como <code>true</code> para seguir quaisquer redirecionamentos que forem retornados pelo IP.</dd>

<dt><code>--json</code></dt>
<dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

<dt><code>-s</code></dt>
<dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks nlb-dns-monitor-configure --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud --enable --desc "Login page monitor" --type HTTPS --method GET --path / --timeout 5 --retries 2 --interval 60  --expected-body "healthy" --expected-codes 2xx --follows-redirects true
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-get
{: #cs_nlb-dns-monitor-get}

Visualize as configurações para um monitor de verificação de funcionamento existente.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-get --cluster CLUSTER --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Opções de comando**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>O nome do host que o funcionamento do monitor verifica. Para listar nomes de host, execute <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

<dt><code>-s</code></dt>
<dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks nlb-dns-monitor-get --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-disable
{: #cs_nlb-dns-monitor-disable}

Desativar um monitor de verificação de funcionamento existente para um nome do host em um cluster.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-disable --cluster CLUSTER --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Opções de comando**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>O nome do host que o funcionamento do monitor verifica. Para listar nomes de host, execute <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

<dt><code>-s</code></dt>
<dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks nlb-dns-monitor-disable --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-enable
{: #cs_nlb-dns-monitor-enable}

Ative um monitor de verificação de funcionamento que você configurou.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-enable --cluster CLUSTER --nlb-host HOST_NAME [--json] [-s]
```
{: pre}

Observe que, na primeira vez que você cria um monitor de verificação de funcionamento, deve-se configurá-lo e ativá-lo com o comando `ibmcloud ks nlb-dns-monitor-configure`. Use o comando `ibmcloud ks nlb-dns-monitor-enable` somente para ativar um monitor configurado, mas ainda não ativado, ou para ativar novamente um monitor desativado anteriormente.

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Opções de comando**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--nlb-host <em>HOST_NAME</em></code></dt>
<dd>O nome do host que o funcionamento do monitor verifica. Para listar nomes de host, execute <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

<dt><code>-s</code></dt>
<dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks nlb-dns-monitor-enable --cluster mycluster --nlb-host mycluster-a1b2cdef345678g9hi012j3kl4567890-0001.us-south.containers.appdomain.cloud
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-ls
{: #cs_nlb-dns-monitor-ls}

Liste as configurações do monitor de verificação de funcionamento para cada nome de host do NLB em um cluster.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-ls --cluster CLUSTER [--json] [-s]
```
{: pre}

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Opções de comando**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--json</code></dt>
<dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

<dt><code>-s</code></dt>
<dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks nlb-dns-monitor-ls --cluster mycluster
```
{: pre}

#### ibmcloud ks nlb-dns-monitor-status
{: #cs_nlb-dns-monitor-status}

Liste o status de verificação de funcionamento para os IPs atrás de nomes de host do NLB em um cluster.
{: shortdesc}

```
ibmcloud ks nlb-dns-monitor-status --cluster CLUSTER [--json] [-s]
```
{: pre}

**Permissões mínimas necessárias**: a função da plataforma **Editor** para o cluster no {{site.data.keyword.containerlong_notm}}

**Opções de comando**:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>-- <em></em></code></dt>
<dd>Inclua esse sinalizador para visualizar o status para apenas um nome de host. Para listar nomes de host, execute <code>ibmcloud ks nlb-dns list --cluster CLUSTER</code>.</dd>

<dt><code>--json</code></dt>
<dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

<dt><code>-s</code></dt>
<dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:
```
ibmcloud ks nlb-dns-monitor-status --cluster mycluster
```
{: pre}

<br />


## Comandos de região
{: #region_commands}

Use esse grupo de comandos para visualizar locais disponíveis e para visualizar e configurar a região de destino atual.
{: shortdesc}

### ibmcloud ks region
{: #cs_region}

Localize a região de destino atual do {{site.data.keyword.containerlong_notm}}. É possível criar e gerenciar clusters específicos para a região. Use o comando `ibmcloud ks region-set` para mudar regiões.
{: shortdesc}

```
ibmcloud ks region
```
{: pre}

<strong> Permissões mínimas necessárias </strong>: nenhuma

### ibmcloud ks region-set
{: #cs_region-set}

Configure a região para o  {{site.data.keyword.containerlong_notm}}. É possível criar e gerenciar clusters específicos para a região e você pode querer clusters em múltiplas regiões para alta disponibilidade.
{: shortdesc}

Por exemplo, é possível efetuar login no {{site.data.keyword.Bluemix_notm}} na região sul dos EUA e criar um cluster. Em seguida, é possível usar `ibmcloud ks region-set eu-central` para destinar a região central da UE e criar outro cluster. Finalmente, é possível usar `ibmcloud ks region-set us-south` para retornar para o Sul dos EUA para gerenciar o seu cluster nessa região.

<strong> Permissões mínimas necessárias </strong>: nenhuma

```
ibmcloud ks region-set [ -- region REGION ]
```
{: pre}

**Opções de comando**:

<dl>
<dt><code> -- region  <em> REGION </em> </code></dt>
<dd>Insira a região que você deseja destinar. Esse valor é opcional. Se você não fornecer a região, será possível selecionar uma na lista na saída.

Para obter uma lista de regiões disponíveis, revise [regiões e zonas](/docs/containers?topic=containers-regions-and-zones) ou use o [comando](#cs_regions) `ibmcloud ks regions`.</dd></dl>

**Exemplo**:

```
ibmcloud ks region-set eu-central
```
{: pre}

```
ibmcloud ks region-set
```
{: pre}

**Saída**:
```
Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 3
OK
```
{: screen}

### ibmcloud ks regions
{: #cs_regions}

Lista as regiões disponíveis. O `Region Name` é o nome do {{site.data.keyword.containerlong_notm}} e o `Region Alias` é o nome geral do {{site.data.keyword.Bluemix_notm}} para a região.
{: shortdesc}

<strong> Permissões mínimas necessárias </strong>: nenhuma

**Exemplo**:

```
ibmcloud ks regions
```
{: pre}

**Saída**:
```
Region Name   Region Alias
ap-north      jp-tok
ap-south      au-syd
eu-central    eu-de
uk-south      eu-gb
us-east       us-east
us-south      us-south
```
{: screen}

### ibmcloud ks zones
{: #cs_datacenters}

Visualize uma lista de zonas disponíveis nas quais você criará um cluster. As zonas disponíveis variam pela região na qual você está com login efetuado. Para alternar regiões, execute `ibmcloud ks region-set`.
{: shortdesc}

```
Zonas ibmcloud ks zones [ -- region-only ] [ -- json ] [ -s ]
```
{: pre}

<strong>Opções de comando</strong>:

   <dl><dt><code> -- region-only </code></dt>
   <dd>Liste somente múltiplas zonas dentro da região em que você efetuou login. Esse valor é opcional.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>

**Permissões mínimas**: nenhuma

<br />



## Comandos de nó do trabalhador
{: worker_node_commands}


### Descontinuado: ibmcloud ks worker-add
{: #cs_worker_add}

Inclua nós do trabalhador independentes em seu cluster padrão que não estejam em um conjunto de trabalhadores.
{: deprecated}

```
ibmcloud ks worker-add --cluster CLUSTER [--file FILE_LOCATION] [--hardware HARDWARE] --machine-type MACHINE_TYPE --workers NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN [--disable-disk-encrypt] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>O caminho para o arquivo YAML incluir nós do trabalhador no cluster. Em vez de definir nós do trabalhador adicionais usando as opções fornecidas nesse comando, será possível usar um arquivo do YAML. Esse valor é opcional.

<p class="note">Se, no comando, você fornecer a mesma opção que o parâmetro no arquivo YAML, o valor no comando terá precedência sobre o valor no YAML. Por exemplo, você define um tipo de máquina em seu arquivo YAML e usa a opção --machine-type no comando, o valor inserido na opção de comando substituirá o valor no arquivo YAML.</p>

<pre class="codeblock">
<code>name: <em>&lt;cluster_name_or_ID&gt;</em>
zone: <em>&lt;zone&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
diskEncryption: <em>false</em></code></pre>

<table>
<caption>Entendendo os componentes de arquivo YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td>Substitua <code><em>&lt;cluster_name_or_ID&gt;</em></code> pelo nome ou ID do cluster no qual você deseja incluir nós do trabalhador.</td>
</tr>
<tr>
<td><code><em>zone</em></code></td>
<td>Substitua <code><em>&lt;zone&gt;</em></code> pela zona para implementar os seus nós do trabalhador. As zonas disponíveis são dependentes da região na qual você efetuou login. Para listar as zonas disponíveis, execute <code>ibmcloud ks zones</code>.</td>
</tr>
<tr>
<td><code><em>machine-type</em></code></td>
<td>Substitua <code><em>&lt;machine_type&gt;</em></code> pelo tipo de máquina na qual você deseja implementar os nós do trabalhador. É possível implementar os nós do trabalhador como máquinas virtuais em hardware compartilhado ou dedicado ou como máquinas físicas no bare metal. Os tipos de máquinas físicas e virtuais disponíveis variam de acordo com a zona na qual você implementa o cluster. Para obter mais informações, consulte o [comando](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types) `ibmcloud ks machine-types`.</td>
</tr>
<tr>
<td><code><em>private-vlan</em></code></td>
<td>Substitua <code><em>&lt;private_VLAN&gt;</em></code> pelo ID da VLAN privada que você deseja usar para os seus nós do trabalhador. Para listar VLANs disponíveis, execute <code>ibmcloud ks vlans <em>&lt;zone&gt;</em></code> e procure roteadores de VLAN que iniciem com <code>bcr</code> (roteador de backend).</td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td>Substitua <code>&lt;public_VLAN&gt;</code> pelo ID da VLAN pública que você deseja usar para os seus nós do trabalhador. Para listar VLANs disponíveis, execute <code>ibmcloud ks vlans &lt;zone&gt;</code> e procure roteadores de VLAN que iniciem com <code>fcr</code> (roteador de front-end). <br><strong>Nota</strong>: Se os nós do trabalhador forem configurados apenas com uma VLAN privada, você deverá permitir que os nós do trabalhador e o cluster principal se comuniquem por [ativação do terminal em serviço privado](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private) ou [configurando um dispositivo de gateway](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_gateway).</td>
</tr>
<tr>
<td><code>hardware</code></td>
<td>Para tipos de máquinas virtuais: o nível de isolamento de hardware para seu nó do trabalhador. Use dedicated se desejar que os recursos físicos disponíveis sejam dedicados somente a você ou shared para permitir que os recursos físicos sejam compartilhados com outros clientes IBM. O padrão é shared.</td>
</tr>
<tr>
<td><code>workerNum</code></td>
<td>Substitua <code><em>&lt;number_workers&gt;</em></code> pelo número de nós do trabalhador que você deseja implementar.</td>
</tr>
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>Os nós do trabalhador apresentam criptografia de disco AES de 256 bits por padrão; [saiba mais](/docs/containers?topic=containers-security#encrypted_disk). Para desativar a criptografia, inclua essa opção e configure o valor para <code>false</code>.</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>O nível de isolamento de hardware para seu nó do trabalhador. Use dedicated se desejar que os recursos físicos disponíveis sejam dedicados somente a você ou shared para permitir que os recursos físicos sejam compartilhados com outros clientes IBM. O padrão é shared. Esse valor é opcional. Para tipos de máquina bare metal, especifique `dedicated`.</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Escolha um tipo de máquina. É possível implementar os nós do trabalhador como máquinas virtuais em hardware compartilhado ou dedicado ou como máquinas físicas no bare metal. Os tipos de máquinas físicas e virtuais disponíveis variam de acordo com a zona na qual você implementa o cluster. Para obter mais informações, consulte a documentação para o comando `ibmcloud ks machine-types` [](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types). Esse valor é necessário para clusters padrão e não está disponível para clusters livres.</dd>

<dt><code>--workers <em>NUMBER</em></code></dt>
<dd>Um número inteiro que representa o número de nós do trabalhador a serem criados no cluster. O valor padrão é 1. Esse valor é opcional.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>A VLAN privada que foi especificada quando o cluster foi criado. Este valor é obrigatório. Os roteadores de VLAN privada sempre iniciam com <code>bcr</code> (roteador de backend) e roteadores de VLAN pública sempre iniciam com <code>fcr</code> (roteador de front-end). Ao criar um cluster e especificar as VLANs públicas e privadas, o número e a combinação de letras após esses prefixos devem corresponder.</dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>A VLAN pública que foi especificada quando o cluster foi criado. Esse valor é opcional. Se você deseja que os nós do trabalhador existam somente em uma VLAN privada, não forneça um ID de VLAN pública. Os roteadores de VLAN privada sempre iniciam com <code>bcr</code> (roteador de backend) e roteadores de VLAN pública sempre iniciam com <code>fcr</code> (roteador de front-end). Ao criar um cluster e especificar as VLANs públicas e privadas, o número e a combinação de letras após esses prefixos devem corresponder.<p class="note">Se os nós do trabalhador estão configurados somente com uma VLAN privada, deve-se permitir que os nós do trabalhador e o cluster mestre se comuniquem [ativando o terminal em serviço privado](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_private) ou [configurando um dispositivo de gateway](/docs/containers?topic=containers-cs_network_ov#cs_network_ov_master_gateway).</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Os nós do trabalhador apresentam criptografia de disco AES de 256 bits por padrão; [saiba mais](/docs/containers?topic=containers-security#encrypted_disk). Para desativar a criptografia, inclua essa opção.</dd>

<dt><code>-s</code></dt>
<dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

</dl>

**Exemplos**:

  ```
  ibmcloud ks worker-add --cluster my_cluster --workers 3 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type b3c.4x16 --hardware shared
  ```
  {: pre}

### ibmcloud ks worker-get
{: #cs_worker_get}

Visualize os detalhes de um nó do trabalhador.
{: shortdesc}

```
ibmcloud ks worker-get --cluster [CLUSTER_NAME_OR_ID] --worker WORKER_NODE_ID [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>-- cluster <em>CLUSTER_NAME_OR_ID</em></code></dt>
   <dd>O nome ou o ID do cluster do nó do trabalhador. Esse valor é opcional.</dd>

   <dt><code> -- worker  <em> WORKER_NODE_ID </em> </code></dt>
   <dd>O nome do seu nó do trabalhador. Execute <code>ibmcloud ks workers <em>CLUSTER</em></code> para visualizar os IDs para os nós do trabalhador em um cluster. Este valor é obrigatório.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks worker-get --cluster my_cluster --worker kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1
  ```
  {: pre}

**Saída de exemplo**:

  ```
  ID: kube-dal10-123456789-w1 State: normal Status: Ready Trust: disabled Private VLAN: 223xxxx Public VLAN: 223xxxx Private IP: 10.xxx.xx.xxx Public IP: 169.xx.xxx.xxx Hardware: shared Zone: dal10 Version: 1.8.11_1509
  ```
  {: screen}

### ibmcloud ks worker-reboot
{: #cs_worker_reboot}

Reinicialize um nó do trabalhador em um cluster. Durante a reinicialização, o estado do nó do trabalhador não muda. Por exemplo, você poderá usar uma reinicialização se o status do nó do trabalhador na infraestrutura do IBM Cloud (SoftLayer) for `Powered Off` e você precisar ativar o nó do trabalhador. Uma reinicialização limpa os diretórios temporários, mas não limpa o sistema de arquivos inteiro ou reformata os discos. O endereço IP público e privado do nó do trabalhador permanece igual após a operação de reinicialização.
{: shortdesc}

```
ibmcloud ks worker-reboot [-f] [--hard] --cluster CLUSTER --worker WORKER [WORKER] [--skip-master-healthcheck] [-s]
```
{: pre}

**Atenção:** reinicializando um nó do trabalhador pode causar distorção de dados no nó do trabalhador. Use esse comando com cuidado e quando souber que uma reinicialização pode ajudar a recuperar seu nó do trabalhador. Em todos os outros casos, [recarregue seu nó do trabalhador](#cs_worker_reload).

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

Antes de reinicializar o nó do trabalhador, certifique-se de que os pods estão reprogramados em outros nós do trabalhador para ajudar a evitar um tempo de inatividade para seu app ou distorção de dados em seu nó do trabalhador.

1. Liste todos os nós do trabalhador em seu cluster e anote o **nome** do nó do trabalhador que você deseja reinicializar.
   ```
   kubectl get nodes
   ```
   {: pre}
   O **nome** retornado nesse comando é o endereço IP privado designado ao nó do trabalhador. É possível localizar mais informações sobre seu nó do trabalhador ao executar o comando `ibmcloud ks workers <cluster_name_or_ID>` e procurar o nó do trabalhador com o mesmo endereço **IP privado**.
2. Marque o nó do trabalhador como não programável em um processo conhecido como bloqueio. Ao bloquear um nó do trabalhador, ele fica indisponível para planejamento futuro do pod. Use o **nome** do nó do trabalhador recuperado na etapa anterior.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Verifique se o planejamento do pod está desativado para seu nó do trabalhador.
   ```
   kubectl get nodes
   ```
   {: pre}
   O nó do trabalhador ficará desativado para planejamento do pod se o status exibir **`SchedulingDisabled`**.
 4. Force os pods para que sejam removidos do nó do trabalhador e reprogramados nos nós do trabalhador restantes no cluster.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    Esse processo pode levar alguns minutos.
 5. Reinicialize o nó do trabalhador. Use o ID do trabalhador retornado do comando `ibmcloud ks workers <cluster_name_or_ID>`.
    ```
    ibmcloud ks worker-reboot --cluster <cluster_name_or_ID> --workers <worker_name_or_ID>
    ```
    {: pre}
 6. Espere cerca de 5 minutos antes de disponibilizar o seu nó do trabalhador para planejamento de pod para assegurar que a reinicialização esteja concluída. Durante a reinicialização, o estado do nó do trabalhador não muda. A reinicialização de um nó do trabalhador é geralmente concluída em alguns segundos.
 7. Disponibilize o nó do trabalhador para planejamento do pod. Use o **nome** do nó do trabalhador retornado do comando `kubectl get nodes`.
    ```
    kubectl uncordon <worker_name>
    ```
    {: pre}

    </br>

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>-f</code></dt>
   <dd>Use essa opção para forçar a reinicialização do nó do trabalhador sem prompts de usuário. Esse valor é opcional.</dd>

   <dt><code>--hard</code></dt>
   <dd>Use esta opção para forçar uma reinicialização forçada de um nó do trabalhador cortando a energia para o nó do trabalhador. Use essa opção se o nó do trabalhador não estiver responsivo ou o tempo de execução do contêiner do nó do trabalhador não estiver responsivo. Esse valor é opcional.</dd>

   <dt><code>--workers <em>WORKER</em></code></dt>
   <dd>O nome ou ID de um ou mais nós do trabalhador. Use um espaço para listar múltiplos nós do
trabalhador. Este valor é obrigatório.</dd>

   <dt><code>--skip-master-healthcheck </code></dt>
   <dd>Ignore uma verificação de funcionamento de seu mestre antes de recarregar ou reinicializar os nós do trabalhador.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks worker-reboot --cluster my_cluster --worker kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}



### ibmcloud ks worker-reload
{: #cs_worker_reload}

Recarregue as configurações para um nó do trabalhador. Um recarregamento poderá ser útil se seu nó do trabalhador tiver problemas, como desempenho lento ou se o nó do trabalhador estiver preso em um estado inoperante. Observe que, durante o recarregamento, a máquina do nó do trabalhador é atualizada com a imagem mais recente e os dados são excluídos se não [armazenados fora do nó do trabalhador](/docs/containers?topic=containers-storage_planning#persistent_storage_overview). O endereço IP do nó do trabalhador permanece igual após a operação de recarregamento.
{: shortdesc}

```
ibmcloud ks worker-reload [-f] --cluster CLUSTER --workers WORKER [WORKER] [--skip-master-healthcheck] [-s]
```
{: pre}

O recarregamento de um nó do trabalhador aplica atualizações da versão de correção ao nó do trabalhador, mas não atualizações principais ou secundárias. Para ver as mudanças de uma versão de correção para a próxima, revise a documentação de [Log de mudanças da versão](/docs/containers?topic=containers-changelog#changelog).
{: tip}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

Antes de recarregar seu nó do trabalhador, certifique-se de que os pods estejam reprogramados em outros nós do trabalhador para ajudar a evitar um tempo de inatividade para seu app ou distorção de dados em seu nó do trabalhador.

1. Liste todos os nós do trabalhador em seu cluster e anote o **nome** do nó do trabalhador que você deseja recarregar.
   ```
   kubectl get nodes
   ```
   O **nome** retornado nesse comando é o endereço IP privado designado ao nó do trabalhador. É possível localizar mais informações sobre seu nó do trabalhador ao executar o comando `ibmcloud ks workers <cluster_name_or_ID>` e procurar o nó do trabalhador com o mesmo endereço **IP privado**.
2. Marque o nó do trabalhador como não programável em um processo conhecido como bloqueio. Ao bloquear um nó do trabalhador, ele fica indisponível para planejamento futuro do pod. Use o **nome** do nó do trabalhador recuperado na etapa anterior.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Verifique se o planejamento do pod está desativado para seu nó do trabalhador.
   ```
   kubectl get nodes
   ```
   {: pre}
   O nó do trabalhador ficará desativado para planejamento do pod se o status exibir **`SchedulingDisabled`**.
 4. Force os pods para que sejam removidos do nó do trabalhador e reprogramados nos nós do trabalhador restantes no cluster.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    Esse processo pode levar alguns minutos.
 5. Recarregue o nó do trabalhador. Use o ID do trabalhador retornado do comando `ibmcloud ks workers <cluster_name_or_ID>`.
    ```
    ibmcloud ks worker-reload --cluster <cluster_name_or_ID> --workers <worker_name_or_ID>
    ```
    {: pre}
 6. Aguarde o recarregamento ser concluído.
 7. Disponibilize o nó do trabalhador para planejamento do pod. Use o **nome** do nó do trabalhador retornado do comando `kubectl get nodes`.
    ```
    kubectl uncordon <worker_name>
    ```
</br>
<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>-f</code></dt>
   <dd>Use essa opção para forçar o recarregamento de um nó do trabalhador sem prompts de usuário. Esse valor é opcional.</dd>

   <dt><code>--worker <em>WORKER</em></code></dt>
   <dd>O nome ou ID de um ou mais nós do trabalhador. Use um espaço para listar múltiplos nós do
trabalhador. Este valor é obrigatório.</dd>

   <dt><code>--skip-master-healthcheck </code></dt>
   <dd>Ignore uma verificação de funcionamento de seu mestre antes de recarregar ou reinicializar os nós do trabalhador.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks worker-reload --cluster my_cluster --workers kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### ibmcloud ks worker-rm
{: #cs_worker_rm}

Remover um ou mais nós do trabalhador de um cluster. Se você remover um nó do trabalhador, o seu cluster se tornará desbalanceado. É possível rebalancear automaticamente o seu conjunto de trabalhadores executando o comando `ibmcloud ks worker-pool-rebalance` [](#cs_rebalance).
{: shortdesc}

```
ibmcloud ks worker-rm [-f] --cluster CLUSTER --workers WORKER[,WORKER] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

Antes de remover o seu nó do trabalhador, certifique-se de que os pods estejam reprogramados em outros nós do trabalhador para ajudar a evitar um tempo de inatividade para o seu app ou a distorção de dados em seu nó do trabalhador.
{: tip}

1. Liste todos os nós do trabalhador em seu cluster e anote o **nome** do nó do trabalhador que você deseja remover.
   ```
   kubectl get nodes
   ```
   {: pre}
   O **nome** retornado nesse comando é o endereço IP privado designado ao nó do trabalhador. É possível localizar mais informações sobre seu nó do trabalhador ao executar o comando `ibmcloud ks workers <cluster_name_or_ID>` e procurar o nó do trabalhador com o mesmo endereço **IP privado**.
2. Marque o nó do trabalhador como não programável em um processo conhecido como bloqueio. Ao bloquear um nó do trabalhador, ele fica indisponível para planejamento futuro do pod. Use o **nome** do nó do trabalhador recuperado na etapa anterior.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Verifique se o planejamento do pod está desativado para seu nó do trabalhador.
   ```
   kubectl get nodes
   ```
   {: pre}
   O nó do trabalhador ficará desativado para planejamento do pod se o status exibir **`SchedulingDisabled`**.
4. Force os pods para que sejam removidos do nó do trabalhador e reprogramados nos nós do trabalhador restantes no cluster.
   ```
   kubectl drain <worker_name>
   ```
   {: pre}
   Esse processo pode levar alguns minutos.
5. Remova o nó do trabalhador. Use o ID do trabalhador retornado do comando `ibmcloud ks workers <cluster_name_or_ID>`.
   ```
   ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_name_or_ID>
   ```
   {: pre}

6. Verifique se o nó do trabalhador foi removido.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}
</br>
<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>-f</code></dt>
   <dd>Use essa opção para forçar a remoção de um nó do trabalhador sem prompts de usuário. Esse valor é opcional.</dd>

   <dt><code>--workers <em>WORKER</em></code></dt>
   <dd>O nome ou ID de um ou mais nós do trabalhador. Use um espaço para listar múltiplos nós do
trabalhador. Este valor é obrigatório.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks worker-rm --cluster my_cluster --workers kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}



### ibmcloud ks worker-update
{: #cs_worker_update}

Atualize os nós do trabalhador para aplicar as atualizações e correções de segurança mais recentes ao sistema operacional e para atualizar a versão do Kubernetes para corresponder à versão do mestre do Kubernetes. É possível atualizar a versão do principal do Kubernetes com o [comando](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_update) `ibmcloud ks cluster-update`. O endereço IP do nó do trabalhador permanece igual após a operação de atualização.
{: shortdesc}

```
ibmcloud ks worker-update [-f] --cluster CLUSTER --workers WORKER[,WORKER] [--force-update] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

Executar o `ibmcloud ks worker-update` pode causar tempo de inatividade para seus apps e serviços. Durante a atualização, todos os pods são reprogramados em outros nós do trabalhador, o nó do trabalhador tem a imagem reinstalada e os dados são excluídos, se não armazenados fora do pod. Para evitar tempo de inatividade, [assegure-se de que você tenha nós do trabalhador suficientes para manipular a carga de trabalho enquanto os nós do trabalhador selecionados estão atualizando](/docs/containers?topic=containers-update#worker_node).
{: important}

Pode ser necessário mudar seus arquivos YAML para implementações antes de atualizar. Revise essa [nota sobre a liberação](/docs/containers?topic=containers-cs_versions) para obter detalhes.

<strong>Opções de comando</strong>:

   <dl>

   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster no qual você lista nós do trabalhador disponíveis. Este valor é obrigatório.</dd>

   <dt><code>-f</code></dt>
   <dd>Use essa opção para forçar a atualização do mestre sem prompts do usuário. Esse valor é opcional.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Tente a atualização mesmo se a mudança for maior que duas versões secundárias. Esse valor é opcional.</dd>

   <dt><code>--workers <em>WORKER</em></code></dt>
   <dd>O ID de um ou mais nós do trabalhador. Use um espaço para listar múltiplos nós do
trabalhador. Este valor é obrigatório.</dd>

   <dt><code>-s</code></dt>
   <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

   </dl>

**Exemplo**:

  ```
  ibmcloud ks worker-update --cluster my_cluster --worker kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### ibmcloud ks workers
{: #cs_workers}

Visualizar uma lista de nós do trabalhador e o status de cada um deles em um cluster.
{: shortdesc}

```
ibmcloud ks workers --cluster CLUSTER [--worker-pool POOL] [--show-pools] [--show-deleted] [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou o ID do cluster para os nós disponíveis do trabalhador. Este valor é obrigatório.</dd>

   <dt><code>--worker-pool <em>POOL</em></code></dt>
   <dd>Visualize somente os nós do trabalhador que pertencem ao conjunto de trabalhadores. Para listar os conjuntos do trabalhador disponíveis, execute `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`. Esse valor é opcional.</dd>

   <dt><code> -- show-pools </code></dt>
   <dd>Liste o conjunto de trabalhadores ao qual cada nó do trabalhador pertence. Esse valor é opcional.</dd>

   <dt><code>--show-deleted</code></dt>
   <dd>Visualize nós do trabalhador que foram excluídos do cluster, incluindo o motivo para a exclusão. Esse valor é opcional.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  ibmcloud ks workers -- cluster my_cluster
  ```
  {: pre}

<br />



## Comandos do conjunto do trabalhador
{: #worker-pool}

### ibmcloud ks worker-pool-create
{: #cs_worker_pool_create}

É possível criar um conjunto de trabalhadores em seu cluster. Quando você incluir um conjunto de trabalhadores, ele não terá uma zona designada por padrão. Você especifica o número de trabalhadores que você deseja em cada zona e os tipos de máquina para os trabalhadores. O conjunto de trabalhadores recebe as versões padrão do Kubernetes. Para concluir a criação de trabalhadores, [inclua uma zona ou zonas](#cs_zone_add) em seu conjunto.
{: shortdesc}

```
ibmcloud ks worker-pool-create --name POOL_NAME --cluster CLUSTER --machine-type MACHINE_TYPE --size-per-zone WORKERS_PER_ZONE --hardware ISOLATION [--labels LABELS] [--disable-disk-encrypt] [-s] [--json]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:
<dl>

  <dt><code>--name <em>POOL_NAME</em></code></dt>
    <dd>O nome que você deseja dar ao seu conjunto de trabalhadores.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
    <dd>Escolha um tipo de máquina. É possível implementar os nós do trabalhador como máquinas virtuais em hardware compartilhado ou dedicado ou como máquinas físicas no bare metal. Os tipos de máquinas físicas e virtuais disponíveis variam de acordo com a zona na qual você implementa o cluster. Para obter mais informações, consulte a documentação para o comando `ibmcloud ks machine-types` [](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types). Esse valor é necessário para clusters padrão e não está disponível para clusters livres.</dd>

  <dt><code>--size-per-zona <em>WORKERS_PER_ZONE</em></code></dt>
    <dd>O número de trabalhadores a serem criados em cada zona. Esse valor é obrigatório e deve ser 1 ou superior.</dd>

  <dt><code>--hardware <em>ISOLATION</em></code></dt>
    <dd>O nível de isolamento de hardware para seu nó do trabalhador. Use `dedicated` se desejar que os recursos físicos disponíveis sejam dedicados somente a você ou `shared` para permitir que os recursos físicos sejam compartilhados com outros clientes IBM. O padrão é `shared`. Para tipos de máquina bare metal, especifique `dedicated`. Este valor é obrigatório.</dd>

  <dt><code>--labels <em>LABELS</em></code></dt>
    <dd>Os rótulos que você deseja designar aos trabalhadores em seu conjunto. Exemplo: `<key1>=<val1>`,`<key2>=<val2>`</dd>

  <dt><code>--disable-disk-encrpyt </code></dt>
    <dd>Especifica que o disco não está criptografado. O valor padrão é <code>false</code>.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks worker-pool-create --name my_pool --cluster my_cluster --machine-type b3c.4x16 --size-per-zone 6
  ```
  {: pre}


### ibmcloud ks worker-pool-get
{: #cs_worker_pool_get}

Visualize os detalhes de um conjunto de trabalhadores.
{: shortdesc}

```
ibmcloud ks worker-pool-get --worker-pool WORKER_POOL --cluster CLUSTER [-s] [--json]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>-- worker-conjunto <em>WORKER_POOL</em></code></dt>
    <dd>O nome do conjunto de nós do trabalhador do qual você deseja visualizar os detalhes. Para listar os conjuntos do trabalhador disponíveis, execute `ibmcloud ks worker-pools --cluster <cluster_name_or_ID>`. Este valor é obrigatório.</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster no qual o conjunto de trabalhadores está localizado. Este valor é obrigatório.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks worker-pool-get --worker-pool pool1 --cluster my_cluster
  ```
  {: pre}

**Saída de exemplo**:

  ```
  Name:               pool
  ID:                 a1a11b2222222bb3c33c3d4d44d555e5-f6f777g
  State:              active
  Hardware:           shared
  Zones:              dal10,dal12
  Workers per zone:   3
  Machine type:       b3c.4x16.encrypted
  Labels:             -
  Version:            1.12.7_1512
  ```
  {: screen}

### ibmcloud ks worker-pool-rebalance
{: #cs_rebalance}

É possível rebalancear o seu conjunto de trabalhadores depois de excluir um nó do trabalhador. Ao executar esse comando, um novo trabalhador ou trabalhadores são incluídos em seu conjunto de trabalhadores.
{: shortdesc}

```
ibmcloud ks worker-pool-rebalance --cluster CLUSTER --worker-pool WORKER_POOL [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code><em>--cluster CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
  <dt><code><em>--worker-pool WORKER_POOL</em></code></dt>
    <dd>O conjunto de trabalhadores que você deseja rebalancear. Este valor é obrigatório.</dd>
  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks worker-pool-rebalance --cluster my_cluster --worker-pool my_pool
  ```
  {: pre}

### ibmcloud ks worker-pool-resize
{: #cs_worker_pool_resize}

Redimensione seu conjunto de trabalhadores para aumentar ou diminuir o número de nós do trabalhador que estão em cada zona de seu cluster. O seu conjunto de trabalhadores deve ter pelo menos 1 nó do trabalhador.
{: shortdesc}

```
ibmcloud ks worker-pool-resize --worker-pool WORKER_POOL --cluster CLUSTER --size-per-zone WORKERS_PER_ZONE [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>-- worker-conjunto <em>WORKER_POOL</em></code></dt>
    <dd>O nome do conjunto de nós do trabalhador que se deseja atualizar. Este valor é obrigatório.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster para o qual você deseja redimensionar conjuntos de trabalhadores. Este valor é obrigatório.</dd>

  <dt><code>--size-per-zona <em>WORKERS_PER_ZONE</em></code></dt>
    <dd>O número de trabalhadores que você deseja ter em cada zona. Esse valor é obrigatório e deve ser 1 ou superior.</dd>

  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>

</dl>

**Exemplo**:

  ```
  ibmcloud ks worker-pool-resize --cluster my_cluster --worker-pool my_pool --size-per-zone 3
  ```
  {: pre}

### ibmcloud ks worker-pool-rm
{: #cs_worker_pool_rm}

Remover um conjunto de trabalhadores do seu cluster. Todos os nós do trabalhador no conjunto são excluídos. Seus pods são reprogramados quando você exclui. Para evitar tempo de inatividade, certifique-se de que você tenha trabalhadores suficientes para executar a carga de trabalho.
{: shortdesc}

```
ibmcloud ks worker-pool-rm --worker-pool WORKER_POOL --cluster CLUSTER [-f] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>-- worker-conjunto <em>WORKER_POOL</em></code></dt>
    <dd>O nome do conjunto de nós do trabalhador que você deseja remover. Este valor é obrigatório.</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster do qual você deseja remover o conjunto de trabalhadores. Este valor é obrigatório.</dd>
  <dt><code>-f</code></dt>
    <dd>Force o comando a ser executado sem prompts do usuário. Esse valor é opcional.</dd>
  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks worker-pool-rm --cluster my_cluster --worker-pool pool1
  ```
  {: pre}

### ibmcloud ks worker-pools
{: #cs_worker_pools}

Visualize os conjuntos de trabalhadores que você tem em um cluster.
{: shortdesc}

```
ibmcloud ks worker-pools --cluster CLUSTER [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Visualizador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>-- cluster <em>CLUSTER_NAME_OR_ID</em></code></dt>
    <dd>O nome ou ID do cluster para o qual você deseja listar os conjuntos de trabalhadores. Este valor é obrigatório.</dd>
  <dt><code>--json</code></dt>
    <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>
  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks worker-pools --cluster my_cluster
  ```
  {: pre}

### ibmcloud ks zone-add
{: #cs_zone_add}

**Somente clusters de múltiplas zonas**: após criar um cluster ou conjunto de trabalhadores, será possível incluir uma zona. Quando você incluir uma zona, os nós do trabalhador serão incluídos na nova zona para corresponder ao número de trabalhadores por zona que você especificou para o conjunto de trabalhadores.
{: shortdesc}

```
ibmcloud ks zone-add --zone ZONE --cluster CLUSTER --worker-pools WORKER_POOL1[,WORKER_POOL2] --private-vlan PRIVATE_VLAN [--public-vlan PUBLIC_VLAN] [--private-only] [--json] [-s]
```
{: pre}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--zone <em>ZONE</em></code></dt>
    <dd>A zona que você deseja incluir. Ele deve ser uma [zona com capacidade para múltiplas zonas](/docs/containers?topic=containers-regions-and-zones#zones) dentro da região do cluster. Este valor é obrigatório.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>--worker-pool <em>WORKER_POOLS</em></code></dt>
    <dd>Uma lista separada por vírgulas de conjuntos de trabalhadores aos quais a zona é incluída. Pelo menos 1 conjunto de trabalhadores é necessário.</dd>

  <dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
    <dd><p>O ID da VLAN privada. Esse valor é condicional.</p>
    <p>Se você tiver uma VLAN privada na zona, esse valor deverá corresponder ao ID de VLAN privada de um ou mais nós do trabalhador no cluster. Para ver as VLANs que você tem disponíveis, execute <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code>. Novos nós do trabalhador são incluídos na VLAN especificada, mas as VLANs de quaisquer nós do trabalhador existentes não mudam.</p>
    <p>Se você não tiver uma VLAN privada ou pública nessa zona, não especifique essa opção. Uma VLAN privada e uma pública serão criadas automaticamente para você quando você incluir inicialmente uma nova zona em seu conjunto de trabalhadores.</p>
    <p>Se você tem múltiplas VLANs para um cluster, múltiplas sub-redes na mesma VLAN ou um cluster de múltiplas zonas, deve-se ativar um [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para sua conta de infraestrutura do IBM Cloud (SoftLayer) para que seus nós do trabalhador possam se comunicar entre si na rede privada. Para ativar o VRF, [entre em contato com o representante de conta da infraestrutura do IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Se não for possível ou você não desejar ativar o VRF, ative o [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para executar essa ação, você precisa da [permissão de infraestrutura](/docs/containers?topic=containers-users#infra_access) **Rede > Gerenciar a rede VLAN Spanning** ou é possível solicitar ao proprietário da conta para ativá-la. Para verificar se o VLAN Spanning já está ativado, use o [comando](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.</p></dd>

  <dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
    <dd><p>O ID da VLAN pública. Esse valor será necessário se você desejar expor as cargas de trabalho nos nós para o público depois de criar o cluster. Ele deve corresponder ao ID da VLAN pública de um ou mais nós do trabalhador no cluster para a zona. Para ver as VLANs que você tem disponíveis, execute <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code>. Novos nós do trabalhador são incluídos na VLAN especificada, mas as VLANs de quaisquer nós do trabalhador existentes não mudam.</p>
    <p>Se você não tiver uma VLAN privada ou pública nessa zona, não especifique essa opção. Uma VLAN privada e uma pública serão criadas automaticamente para você quando você incluir inicialmente uma nova zona em seu conjunto de trabalhadores.</p>
    <p>Se você tem múltiplas VLANs para um cluster, múltiplas sub-redes na mesma VLAN ou um cluster de múltiplas zonas, deve-se ativar um [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para sua conta de infraestrutura do IBM Cloud (SoftLayer) para que seus nós do trabalhador possam se comunicar entre si na rede privada. Para ativar o VRF, [entre em contato com o representante de conta da infraestrutura do IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Se não for possível ou você não desejar ativar o VRF, ative o [VLAN Spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Para executar essa ação, você precisa da [permissão de infraestrutura](/docs/containers?topic=containers-users#infra_access) **Rede > Gerenciar a rede VLAN Spanning** ou é possível solicitar ao proprietário da conta para ativá-la. Para verificar se o VLAN Spanning já está ativado, use o [comando](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.</p></dd>

  <dt><code>-- somente privado </code></dt>
    <dd>Use essa opção para evitar que uma VLAN pública seja criada. Necessário somente quando você especifica a sinalização `--private-vlan` e não inclui a sinalização `--public-vlan`. <p class="note">Se os nós do trabalhador estão configurados somente com uma VLAN privada, deve-se ativar o terminal em serviço privado ou configurar um dispositivo de gateway. Para obter mais informações, consulte  [ Clusters privados ](/docs/containers?topic=containers-plan_clusters#private_clusters).</p></dd>

  <dt><code>--json</code></dt>
    <dd>Imprime a saída de comando no formato JSON. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks zone-add --zone dal10 --cluster my_cluster --worker-pools pool1,pool2,pool3 --private-vlan 2294021
  ```
  {: pre}

### ibmcloud ks zone-network-set
{: #cs_zone_network_set}

**Somente clusters de múltiplas zonas**: configure os metadados de rede para um conjunto de trabalhadores para usar uma VLAN pública ou privada diferente para a zona do que foi usada anteriormente. Os nós do trabalhador que já foram criados no conjunto continuam a usar a VLAN pública ou privada anterior, mas os novos nós do trabalhador no conjunto usam os novos dados de rede.
{: shortdesc}

```
ibmcloud ks zone-network-set --zone ZONE --cluster CLUSTER --worker-pools WORKER_POOL1[,WORKER_POOL2] --private-vlan PRIVATE_VLAN [--public-vlan PUBLIC_VLAN] [-f] [-s]
```
{: pre}

  <strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

  Os roteadores de VLAN privada sempre iniciam com <code>bcr</code> (roteador de backend) e roteadores de VLAN pública sempre iniciam com <code>fcr</code> (roteador de front-end). Ao criar um cluster e especificar as VLANs públicas e privadas, o número e a combinação de letras após esses prefixos devem corresponder.
  <ol><li>Verifique as VLANs que estão disponíveis em seu cluster. <pre class="pre"><code> ibmcloud ks cluster-get -- cluster  &lt;cluster_name_or_ID&gt;  -- showResources </code></pre><p>Saída de exemplo:</p>
  <pre class="screen"><code>Subnet VLANs
VLAN ID   Subnet CIDR         Public   User-managed
229xxxx   169.xx.xxx.xxx/29   true     false
229xxxx   10.xxx.xx.x/29      false    false</code></pre></li>
  <li>Verifique se os IDs de VLAN pública e privada que você deseja usar são compatíveis. Para serem compatíveis, o <strong>Roteador</strong> deve ter o mesmo ID de pod.<pre class="pre"><code> ibmcloud ks vlans -- zone  &lt;zone&gt; </code></pre><p>Saída de exemplo:</p>
  <pre class="screen"><code>ID        Name   Number   Type      Router         Supports Virtual Workers
229xxxx          1234     private   bcr01a.dal12   true
229xxxx          5678     public    fcr01a.dal12   true</code></pre><p>Observe que os IDs de pod de <strong>Roteador</strong> correspondem: `01a` e `01a`. Se um ID de pod fosse `01a` e o outro fosse `02a`, não seria possível configurar esses IDs de VLAN pública e privada para o conjunto de trabalhadores.</p></li>
  <li>Se você não tiver nenhuma VLAN disponível, será possível <a href="/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans">solicitar novas VLANs</a>.</li></ol>

  <strong>Opções de comando</strong>:

  <dl>
    <dt><code>--zone <em>ZONE</em></code></dt>
      <dd>A zona que você deseja incluir. Ele deve ser uma [zona com capacidade para múltiplas zonas](/docs/containers?topic=containers-regions-and-zones#zones) dentro da região do cluster. Este valor é obrigatório.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>--worker-pool <em>WORKER_POOLS</em></code></dt>
    <dd>Uma lista separada por vírgulas de conjuntos de trabalhadores aos quais a zona é incluída. Pelo menos 1 conjunto de trabalhadores é necessário.</dd>

  <dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
    <dd>O ID da VLAN privada. Esse valor é necessário, se você deseja usar a mesma VLAN privada ou uma diferente daquela que usou para seus outros nós do trabalhador. Novos nós do trabalhador são incluídos na VLAN especificada, mas as VLANs de quaisquer nós do trabalhador existentes não mudam.<p class="note">As VLANs privadas e públicas devem ser compatíveis e podem ser determinadas por meio do prefixo de ID do **Roteador**.</p></dd>

  <dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
    <dd>O ID da VLAN pública. Esse valor será necessário somente se você desejar mudar a VLAN pública para a zona. Para mudar a VLAN pública, deve-se sempre fornecer uma VLAN privada compatível. Novos nós do trabalhador são incluídos na VLAN especificada, mas as VLANs de quaisquer nós do trabalhador existentes não mudam.<p class="note">As VLANs privadas e públicas devem ser compatíveis e podem ser determinadas por meio do prefixo de ID do **Roteador**.</p></dd>

  <dt><code>-f</code></dt>
    <dd>Force o comando a ser executado sem prompts do usuário. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
  </dl>

  **Exemplo**:

  ```
  ibmcloud ks zone-network-set --zone dal10 --cluster my_cluster --worker-pools pool1,pool2,pool3 --private-vlan 2294021
  ```
  {: pre}

### ibmcloud ks zone-rm
{: #cs_zone_rm}

**Somente clusters de múltiplas zonas**: remova uma zona de todos os conjuntos de trabalhadores em seu cluster. Todos os nós do trabalhador no conjunto de trabalhadores para essa zona são excluídos.
{: shortdesc}

```
ibmcloud ks zone-rm --zone ZONE --cluster CLUSTER [-f] [-s]
```
{: pre}

Antes de remover uma zona, certifique-se de que tenha nós do trabalhador suficientes em outras zonas no cluster para que os seus pods possam reagendar para ajudar a evitar um tempo de inatividade para o seu app ou uma distorção de dados em seu nó do trabalhador.
{: tip}

<strong>Permissões mínimas necessárias</strong>: a função da plataforma **Operador** para o cluster no {{site.data.keyword.containerlong_notm}}

<strong>Opções de comando</strong>:

<dl>
  <dt><code>--zone <em>ZONE</em></code></dt>
    <dd>A zona que você deseja incluir. Ele deve ser uma [zona com capacidade para múltiplas zonas](/docs/containers?topic=containers-regions-and-zones#zones) dentro da região do cluster. Este valor é obrigatório.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>-f</code></dt>
    <dd>Forçar a atualização sem prompts do usuário. Esse valor é opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>Não mostrar a mensagem do dia ou os lembretes de atualização. Esse valor é opcional.</dd>
</dl>

**Exemplo**:

  ```
  ibmcloud ks zone-rm --zone dal10 --cluster my_cluster
  ```
  {: pre}


