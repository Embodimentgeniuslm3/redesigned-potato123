---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, audit

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



# {{site.data.keyword.cloudaccesstrailshort}} events
{: #at_events}

É possível visualizar, gerenciar e auditar atividades iniciadas pelo usuário em seu cluster do {{site.data.keyword.containerlong_notm}} usando o serviço {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

O  {{site.data.keyword.containershort_notm}}  gera dois tipos de eventos do  {{site.data.keyword.cloudaccesstrailshort}} :

* ** Eventos de gerenciamento de cluster **:
    * Esses eventos são gerados e encaminhados automaticamente para o {{site.data.keyword.cloudaccesstrailshort}}.
    * É possível visualizar esses eventos por meio do **domínio de contas** do {{site.data.keyword.cloudaccesstrailshort}}.

* ** Eventos de auditoria do servidor da API do Kubernetes **:
    * Esses eventos são gerados automaticamente, mas deve-se configurar seu cluster para encaminhar esses eventos para o serviço {{site.data.keyword.cloudaccesstrailshort}}.
    * É possível configurar seu cluster para enviar eventos para o **domínio de contas** ou para um **domínio de espaço** do {{site.data.keyword.cloudaccesstrailshort}}. Para obter mais informações, consulte [Enviando logs de auditoria](/docs/containers?topic=containers-health#api_forward).

Para obter mais informações sobre como o serviço funciona, consulte a [documentação do {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started). Para obter mais informações sobre as ações do Kubernetes que são rastreadas, revise a [documentação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/home/).

O {{site.data.keyword.containerlong_notm}} atualmente não está configurado para usar o {{site.data.keyword.at_full}}. Para gerenciar os eventos de gerenciamento de cluster e os logs de auditoria da API do Kubernetes, continue usando o {{site.data.keyword.cloudaccesstrailfull_notm}} com o LogAnalysis.
{: note}

## Localizando informações para eventos
{: #kube-find}

É possível monitorar as atividades em seu cluster examinando os logs no painel do Kibana.
{: shortdesc}

Para monitorar a atividade administrativa:

1. Efetue login em sua conta do  {{site.data.keyword.Bluemix_notm}} .
2. No catálogo, provisione uma instância do serviço {{site.data.keyword.cloudaccesstrailshort}} na mesma conta que sua instância do {{site.data.keyword.containerlong_notm}}.
3. Na guia **Gerenciar** do painel do {{site.data.keyword.cloudaccesstrailshort}}, selecione o domínio de conta ou de espaço.
  * **Logs de conta**: os eventos de gerenciamento de cluster e os eventos de auditoria do servidor de API do Kubernetes estão disponíveis no **domínio de contas** para a região do {{site.data.keyword.Bluemix_notm}} em que os eventos são gerados.
  * **Logs de espaço**: se você especificou um espaço quando configurou seu cluster para encaminhar eventos de auditoria do servidor de API do Kubernetes, esses eventos estarão disponíveis no **domínio de espaço** que está associado ao espaço do Cloud Foundry no qual o serviço {{site.data.keyword.cloudaccesstrailshort}} é provisionado.
4. Clique em  ** Visualizar no Kibana **.
5. Configure o prazo durante o qual deseja visualizar os logs. O padrão é 24 horas.
6. Para limitar sua procura, é possível clicar no ícone de edição para `ActivityTracker_Account_Search_in_24h` e incluir campos na coluna **Campos disponíveis**.

Para permitir que outros usuários visualizem eventos de conta e espaço, consulte [Concedendo permissões para ver eventos de conta](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions).
{: tip}

## Rastreando eventos de gerenciamento de cluster
{: #cluster-events}

Verifique a lista a seguir dos eventos de gerenciamento de cluster que são enviados para o {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
<tr>
<th>Ação</th>
<th>Descrição</th></tr><tr>
<td><code> containers-kubernetes.account-credentials.set </code></td>
<td>As credenciais de infraestrutura em uma região para um grupo de recursos são configuradas.</td></tr><tr>
<td><code>containers-kubernetes.account-credentials.unset</code></td>
<td>As credenciais de infraestrutura em uma região para um grupo de recursos são desconfiguradas.</td></tr><tr>
<td><code> containers-kubernetes.alb.create </code></td>
<td>Um ALB do Ingress é criado.</td></tr><tr>
<td><code> containers-kubernetes.alb.delete </code></td>
<td>Um ALB de Ingress é excluído.</td></tr><tr>
<td><code> containers-kubernetes.alb.get </code></td>
<td>As informações do ALB do Ingress são visualizadas.</td></tr><tr>
<td><code> containers-kubernetes.apikey.reset </code></td>
<td>Uma chave de API é reconfigurada para uma região e um grupo de recursos.</td></tr><tr>
<td><code> containers-kubernetes.cluster.create </code></td>
<td>Um cluster é criado.</td></tr><tr>
<td><code> containers-kubernetes.cluster.delete </code></td>
<td>Um cluster é excluído.</td></tr><tr>
<td><code> containers-kubernetes.cluster-feature.enable </code></td>
<td>Um recurso, como o Cálculo confiável para nós do trabalhador bare metal, está ativado em um cluster.</td></tr><tr>
<td><code>containers-kubernetes.cluster.get</code></td>
<td>As informações do cluster são visualizadas.</td></tr><tr>
<td><code> containers-kubernetes.logging-config.create </code></td>
<td>Uma configuração de encaminhamento de log é criada.</td></tr><tr>
<td><code> containers-kubernetes.logging-config.delete </code></td>
<td>Uma configuração de encaminhamento de log é excluída.</td></tr><tr>
<td><code> containers-kubernetes.logging-config.get </code></td>
<td>As informações para uma configuração de encaminhamento de log são visualizadas.</td></tr><tr>
<td><code> containers-kubernetes.logging-config.update </code></td>
<td>Uma configuração de encaminhamento de log é atualizada.</td></tr><tr>
<td><code> containers-kubernetes.logging-config.refresh </code></td>
<td>Uma configuração de encaminhamento de log é atualizada.</td></tr><tr>
<td><code> containers-kubernetes.logging-filter.create </code></td>
<td>Um filtro de criação de log é criado.</td></tr><tr>
<td><code> containers-kubernetes.logging-filter.delete </code></td>
<td>Um filtro de criação de log é excluído.</td></tr><tr>
<td><code> containers-kubernetes.logging-filter.get </code></td>
<td>As informações para um filtro de criação de log são visualizadas.</td></tr><tr>
<td><code>containers-kubernetes.logging-filter.update</code></td>
<td>Um filtro de criação de log é atualizado.</td></tr><tr>
<td><code> containers-kubernetes.logging-autoupdate.alteradas </code></td>
<td>O atualizador automático de complemento de criação de log está ativado ou desativado.</td></tr><tr>
<td><code> containers-kubernetes.mzlb.create </code></td>
<td>Um balanceador de carga de várias zonas é criado.</td></tr><tr>
<td><code> containers-kubernetes.mzlb.delete </code></td>
<td>Um balanceador de carga de várias zonas é excluído.</td></tr><tr>
<td><code> containers-kubernetes.service.bind </code></td>
<td>Um serviço é ligado a um cluster.</td></tr><tr>
<td><code> containers-kubernetes.service.unbind </code></td>
<td>Um serviço está desvinculado de um cluster.</td></tr><tr>
<td><code> containers-kubernetes.subnet.add </code></td>
<td>Uma sub-rede de infraestrutura do IBM Cloud (SoftLayer) existente é incluída em um cluster.</td></tr><tr>
<td><code> containers-kubernetes.subnet.create </code></td>
<td>Uma sub-rede é criada.</td></tr><tr>
<td><code> containers-kubernetes.usersubnet.add </code></td>
<td>Uma sub-rede gerenciada pelo usuário é incluída em um cluster.</td></tr><tr>
<td><code>containers-kubernetes.usersubnet.delete</code></td>
<td>Uma sub-rede gerenciada pelo usuário é removida de um cluster.</td></tr><tr>
<td><code> containers-kubernetes.version.update </code></td>
<td>A versão do Kubernetes de um nó do cluster mestre é atualizada.</td></tr><tr>
<td><code> containers-kubernetes.worker.create </code></td>
<td>Um nó do trabalhador é criado.</td></tr><tr>
<td><code> containers-kubernetes.worker.delete </code></td>
<td>Um nó do trabalhador é excluído.</td></tr><tr>
<td><code> containers-kubernetes.worker.get </code></td>
<td>As informações para um nó do trabalhador são visualizadas.</td></tr><tr>
<td><code> containers-kubernetes.worker.reboot </code></td>
<td>Um nó do trabalhador é reinicializado.</td></tr><tr>
<td><code> containers-kubernetes.worker.reload </code></td>
<td>Um nó do trabalhador é recarregado.</td></tr><tr>
<td><code> containers-kubernetes.worker.update </code></td>
<td>Um nó do trabalhador é atualizado.</td></tr>
</table>

## Rastreando eventos de auditoria do Kubernetes
{: #kube-events}

Verifique a tabela a seguir para obter uma lista dos eventos de auditoria do servidor de API do Kubernetes que são enviados para o {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

Antes de iniciar: certifique-se de que seu cluster esteja configurado para encaminhar os [eventos de auditoria da API do Kubernetes](/docs/containers?topic=containers-health#api_forward).

<table>
    <th>Ação</th>
    <th>Descrição</th><tr>
    <td><code> bindings.create </code></td>
    <td>Uma ligação é criada.</td></tr><tr>
    <td><code> certificatesigningrequests.create </code></td>
    <td>Uma solicitação para assinar um certificado é criada.</td></tr><tr>
    <td><code> certificatesigningrequests.delete </code></td>
    <td>Uma solicitação para assinar um certificado é excluída.</td></tr><tr>
    <td><code> certificatesigningrequests.patch </code></td>
    <td>Uma solicitação para assinar um certificado é corrigida.</td></tr><tr>
    <td><code> certificatesigningrequests.update </code></td>
    <td>Uma solicitação para assinar um certificado é atualizada.</td></tr><tr>
    <td><code> clusterbindings.create </code></td>
    <td>Uma ligação de função de cluster é criada.</td></tr><tr>
    <td><code> clusterbindings.deleted </code></td>
    <td>Uma ligação de função de cluster foi excluída.</td></tr><tr>
    <td><code> clusterbindings.patched </code></td>
    <td>Uma ligação de função de cluster é corrigida.</td></tr><tr>
    <td><code> clusterbindings.updated </code></td>
    <td>Uma ligação de função de cluster é atualizada.</td></tr><tr>
    <td><code> clusterroles.create </code></td>
    <td>Uma função de cluster é criada.</td></tr><tr>
    <td><code>clusterroles.deleted</code></td>
    <td>Uma função de cluster é excluída.</td></tr><tr>
    <td><code> clusterroles.patched </code></td>
    <td>Uma função de cluster é corrigida.</td></tr><tr>
    <td><code> clusterroles.updated </code></td>
    <td>Uma função de cluster é atualizada.</td></tr><tr>
    <td><code> configmaps.create </code></td>
    <td>Um mapa de configuração é criado.</td></tr><tr>
    <td><code> configmaps.delete </code></td>
    <td>Um mapa de configuração é excluído.</td></tr><tr>
    <td><code>configmaps.patch</code></td>
    <td>Um mapa de configuração é corrigido.</td></tr><tr>
    <td><code> configmaps.update </code></td>
    <td>Um mapa de configuração é atualizado.</td></tr><tr>
    <td><code> controllerrevisions.create </code></td>
    <td>Uma revisão do controlador é criada.</td></tr><tr>
    <td><code> controllerrevisions.delete </code></td>
    <td>Uma revisão do controlador é excluída.</td></tr><tr>
    <td><code> controllerrevisions.patch </code></td>
    <td>Uma revisão do controlador é corrigida.</td></tr><tr>
    <td><code> controllerrevisions.update </code></td>
    <td>Uma revisão do controlador é atualizada.</td></tr><tr>
    <td><code> daemonsets.create </code></td>
    <td>Um conjunto de daemon é criado.</td></tr><tr>
    <td><code> daemonsets.delete </code></td>
    <td>Um conjunto de daemon é excluído.</td></tr><tr>
    <td><code>daemonsets.patch</code></td>
    <td>Um conjunto de daemon é corrigido.</td></tr><tr>
    <td><code> daemonsets.update </code></td>
    <td>Um conjunto de daemon é atualizado.</td></tr><tr>
    <td><code> deployments.create </code></td>
    <td>Uma implementação é criada.</td></tr><tr>
    <td><code> deployments.delete </code></td>
    <td>Uma implementação é excluída.</td></tr><tr>
    <td><code> deployments.patch </code></td>
    <td>Uma implementação é corrigida.</td></tr><tr>
    <td><code> deployments.update </code></td>
    <td>Uma implementação é atualizada.</td></tr><tr>
    <td><code> events.create </code></td>
    <td>Um evento é criado.</td></tr><tr>
    <td><code> events.delete </code></td>
    <td>Um evento é excluído.</td></tr><tr>
    <td><code> events.patch </code></td>
    <td>Um evento é corrigido.</td></tr><tr>
    <td><code> events.update </code></td>
    <td>Um evento é atualizado.</td></tr><tr>
    <td><code>externaladmissionhookconfigurations.create</code></td>
    <td>No Kubernetes v1.8, uma configuração de gancho de admissões externas é criada.</td></tr><tr>
    <td><code> externaladmissionhookconfigurations.delete </code></td>
    <td>No Kubernetes v1.8, uma configuração de gancho de admissões externas é excluída.</td></tr><tr>
    <td><code> externaladmissionhookconfigurationconfigurations.patch </code></td>
    <td>No Kubernetes v1.8, uma configuração de gancho de admissões externas é corrigida.</td></tr><tr>
    <td><code> externaladmissionhookconfigurations.update </code></td>
    <td>No Kubernetes v1.8, uma configuração de gancho de admissões externas é atualizada.</td></tr><tr>
    <td><code> horizontalpodautoscalers.create </code></td>
    <td>Uma política de ajuste de escala automático de pod horizontal é criada.</td></tr><tr>
    <td><code> horizontalpodautoscalers.delete </code></td>
    <td>Uma política de ajuste de escala automático de pod horizontal é excluída.</td></tr><tr>
    <td><code> horizontalpodautoscalers.patch </code></td>
    <td>Uma política de ajuste de escala automático de pod horizontal é corrigida.</td></tr><tr>
    <td><code> horizontalpodautoscalers.update </code></td>
    <td>Uma política de ajuste de escala automático de pod horizontal é atualizada.</td></tr><tr>
    <td><code> ingresses.create </code></td>
    <td>Um ALB do Ingress é criado.</td></tr><tr>
    <td><code>ingresses.delete</code></td>
    <td>Um ALB do Ingress é excluído.</td></tr><tr>
    <td><code> ingresses.patch </code></td>
    <td>Um ALB do Ingress é corrigido.</td></tr><tr>
    <td><code> ingresses.update </code></td>
    <td>Um ALB do Ingress é atualizado.</td></tr><tr>
    <td><code> jobs.create </code></td>
    <td>Uma tarefa é criada.</td></tr><tr>
    <td><code> jobs.delete </code></td>
    <td>Uma tarefa é excluída.</td></tr><tr>
    <td><code> jobs.patch </code></td>
    <td>Uma tarefa é corrigida.</td></tr><tr>
    <td><code> jobs.update </code></td>
    <td>Uma tarefa é atualizada.</td></tr><tr>
    <td><code> localsubjectaccesscesscesscessreviews.create </code></td>
    <td>Uma revisão de acesso de assunto local é criada.</td></tr><tr>
    <td><code>limitranges.create</code></td>
    <td>Um limite de intervalo é criado.</td></tr><tr>
    <td><code> UNK tranges.delete </code></td>
    <td>Um limite de intervalo é excluído.</td></tr><tr>
    <td><code> UNK tranges.patch </code></td>
    <td>Um limite de intervalo é corrigido.</td></tr><tr>
    <td><code> UNK tranges.update </code></td>
    <td>Um limite de intervalo é atualizado.</td></tr><tr>
    <td><code> mutatingwebhookconfigurations.create </code></td>
    <td>Uma configuração de webhook mutante é criada.</td></tr><tr>
    <td><code> mutatingwebhookconfigurations.delete </code></td>
    <td>Uma configuração de webhook mutante é excluída.</td></tr><tr>
    <td><code> mutatingwebhookconfigurations.patch </code></td>
    <td>Uma configuração de webhook mutante é corrigida.</td></tr><tr>
    <td><code> mutatingwebhookconfigurations.update </code></td>
    <td>Uma configuração de webhook mutante é atualizada.</td></tr><tr>
    <td><code> namespaces.create </code></td>
    <td>Um namespace é criado.</td></tr><tr>
    <td><code> namespaces.delete </code></td>
    <td>Um namespace é excluído.</td></tr><tr>
    <td><code> namespaces.patch </code></td>
    <td>Um namespace é corrigido.</td></tr><tr>
    <td><code>namespaces.update</code></td>
    <td>Um namespace é atualizado.</td></tr><tr>
    <td><code> networkpolicies.create </code></td>
    <td>Uma política de rede é criada.</td></tr><tr><tr>
    <td><code> networkpolicies.delete </code></td>
    <td>Uma política de rede é excluída.</td></tr><tr>
    <td><code> networkpolicies.patch </code></td>
    <td>Uma política de rede é corrigida.</td></tr><tr>
    <td><code> networkpolicies.update </code></td>
    <td>Uma política de rede é atualizada.</td></tr><tr>
    <td><code> nodes.create </code></td>
    <td>Um nó é criado.</td></tr><tr>
    <td><code>nodes.delete</code></td>
    <td>Um nó é excluído.</td></tr><tr>
    <td><code> nodes.patch </code></td>
    <td>Um nó é corrigido.</td></tr><tr>
    <td><code>nodes.update</code></td>
    <td>Um nó é atualizado.</td></tr><tr>
    <td><code> persistentvolumeclaims.create </code></td>
    <td>Uma solicitação de volume persistente é criada.</td></tr><tr>
    <td><code> persistentvolumeclaims.delete </code></td>
    <td>Uma solicitação de volume persistente é excluída.</td></tr><tr>
    <td><code> persistentvolumeclaims.patch </code></td>
    <td>Uma solicitação de volume persistente é corrigida.</td></tr><tr>
    <td><code> persistentvolumeclaims.update </code></td>
    <td>Uma solicitação de volume persistente é atualizada.</td></tr><tr>
    <td><code> persistentvolumes.create </code></td>
    <td>Um volume persistente é criado.</td></tr><tr>
    <td><code> persistentvolumes.delete </code></td>
    <td>Um volume persistente é excluído.</td></tr><tr>
    <td><code> persistentvolumes.patch </code></td>
    <td>Um volume persistente é corrigido.</td></tr><tr>
    <td><code> persistentvolumes.update </code></td>
    <td>Um volume persistente é atualizado.</td></tr><tr>
    <td><code> poddisruptionbudgets.create </code></td>
    <td>Um orçamento de interrupção do pod é criado.</td></tr><tr>
    <td><code> poddisruptionbudgets.delete </code></td>
    <td>Um orçamento de interrupção do pod é excluído.</td></tr><tr>
    <td><code>poddisruptionbudgets.patch</code></td>
    <td>Um orçamento de interrupção do pod é corrigido.</td></tr><tr>
    <td><code> poddisruptionbudgets.update </code></td>
    <td>Um orçamento de interrupção do pod é atualizado.</td></tr><tr>
    <td><code> podpresets.create </code></td>
    <td>Uma pré-configuração de pod é criada.</td></tr><tr>
    <td><code> podpresets.deleted </code></td>
    <td>Uma pré-configuração de pod é excluída.</td></tr><tr>
    <td><code> podpresets.patched </code></td>
    <td>Uma pré-configuração de pod é corrigida.</td></tr><tr>
    <td><code> podpresets.updated </code></td>
    <td>Uma pré-configuração de pod é atualizada.</td></tr><tr>
    <td><code>pods.create</code></td>
    <td>Um pod é criado.</td></tr><tr>
    <td><code> pods.delete </code></td>
    <td>Um pod é excluído.</td></tr><tr>
    <td><code> pods.patch </code></td>
    <td>Um pod é corrigido.</td></tr><tr>
    <td><code> pods.update </code></td>
    <td>Um pod é atualizado.</td></tr><tr>
    <td><code> podsecuritypolicies.create </code></td>
    <td>Uma política de segurança de pod é criada.</td></tr><tr>
    <td><code> podsecuritypolicies.delete </code></td>
    <td>Uma política de segurança de pod é excluída.</td></tr><tr>
    <td><code> podsecuritypolicies.patch </code></td>
    <td>Uma política de segurança pod é corrigida.</td></tr><tr>
    <td><code>podsecuritypolicies.update</code></td>
    <td>Uma política de segurança de pod é atualizada.</td></tr><tr>
    <td><code> podtemplates.create </code></td>
    <td>Um modelo de pod é criado.</td></tr><tr>
    <td><code> podtemplates.delete </code></td>
    <td>Um modelo de pod é excluído.</td></tr><tr>
    <td><code>podtemplates.patch</code></td>
    <td>Um modelo de pod é corrigido.</td></tr><tr>
    <td><code> podtemplates.update </code></td>
    <td>Um modelo de pod é atualizado.</td></tr><tr>
    <td><code>replicasets.create</code></td>
    <td>Um conjunto de réplicas é criado.</td></tr><tr>
    <td><code> replicasets.delete </code></td>
    <td>Um conjunto de réplicas é excluído.</td></tr><tr>
    <td><code> replicasets.patch </code></td>
    <td>Um conjunto de réplicas é corrigido.</td></tr><tr>
    <td><code> replicasets.update </code></td>
    <td>Um conjunto de réplicas é atualizado.</td></tr><tr>
    <td><code> replicationcontrollers.create </code></td>
    <td>Um controlador de replicação é criado.</td></tr><tr>
    <td><code> replicationcontrollers.delete </code></td>
    <td>Um controlador de replicação é excluído.</td></tr><tr>
    <td><code> replicationcontrollers.patch </code></td>
    <td>Um controlador de replicação é corrigido.</td></tr><tr>
    <td><code> replicationcontrollers.update </code></td>
    <td>Um controlador de replicação é atualizado.</td></tr><tr>
    <td><code> resourcequotas.create </code></td>
    <td>Uma cota de recurso é criada.</td></tr><tr>
    <td><code> resourcequotas.delete </code></td>
    <td>Uma cota de recurso é excluída.</td></tr><tr>
    <td><code> resourcequotas.patch </code></td>
    <td>Uma cota de recurso é corrigida.</td></tr><tr>
    <td><code> resourcequotas.update </code></td>
    <td>Uma cota de recurso é atualizada.</td></tr><tr>
    <td><code> rolebindings.create </code></td>
    <td>Uma ligação de função é criada.</td></tr><tr>
    <td><code> rolebindings.deleted </code></td>
    <td>Uma ligação de função é excluída.</td></tr><tr>
    <td><code> rolebindings.patched </code></td>
    <td>Uma ligação de função é corrigida.</td></tr><tr>
    <td><code> rolebindings.updated </code></td>
    <td>Uma ligação de função é atualizada.</td></tr><tr>
    <td><code> roles.create </code></td>
    <td>Uma função é criada.</td></tr><tr>
    <td><code> roles.deleted </code></td>
    <td>Uma função é excluída.</td></tr><tr>
    <td><code> roles.patched </code></td>
    <td>Uma função é corrigida.</td></tr><tr>
    <td><code> roles.updated </code></td>
    <td>Uma função é atualizada.</td></tr><tr>
    <td><code>secrets.create</code></td>
    <td>Um segredo é criado.</td></tr><tr>
    <td><code> secrets.deleted </code></td>
    <td>Um segredo é excluído.</td></tr><tr>
    <td><code> secrets.get </code></td>
    <td>Um segredo é visualizado.</td></tr><tr>
    <td><code> secrets.patch </code></td>
    <td>Um segredo é corrigido.</td></tr><tr>
    <td><code> secrets.updated </code></td>
    <td>Um segredo é atualizado.</td></tr><tr>
    <td><code>selfsubjectaccessreviews.create</code></td>
    <td>Uma revisão de acesso de autoassunto é criada.</td></tr><tr>
    <td><code> selfsubjectrouesreviews.create </code></td>
    <td>Uma revisão de regras de autoassunto é criada.</td></tr><tr>
    <td><code> subjectaccessreviewreviews.create </code></td>
    <td>Uma revisão de acesso de assunto é criada.</td></tr><tr>
    <td><code>serviceaccounts.create</code></td>
    <td>Uma conta de serviço é criada.</td></tr><tr>
    <td><code> serviceaccounts.deleted </code></td>
    <td>Uma conta de serviço é excluída.</td></tr><tr>
    <td><code> serviceaccounts.patch </code></td>
    <td>Uma conta de serviço é corrigida.</td></tr><tr>
    <td><code> serviceaccounts.updated </code></td>
    <td>Uma conta de serviço é atualizada.</td></tr><tr>
    <td><code> services.create </code></td>
    <td>Um serviço é criado.</td></tr><tr>
    <td><code> services.deleted </code></td>
    <td>Um serviço é excluído.</td></tr><tr>
    <td><code> services.patch </code></td>
    <td>Um serviço é corrigido.</td></tr><tr>
    <td><code>services.updated</code></td>
    <td>Um serviço é atualizado.</td></tr><tr>
    <td><code> statefulsets.create </code></td>
    <td>Um conjunto stateful é criado.</td></tr><tr>
    <td><code> statefulsets.delete </code></td>
    <td>Um conjunto stateful é excluído.</td></tr><tr>
    <td><code> statefulsets.patch </code></td>
    <td>Um conjunto stateful é corrigido.</td></tr><tr>
    <td><code> statefulsets.update </code></td>
    <td>Um conjunto stateful é atualizado.</td></tr><tr>
    <td><code> tokenreviews.create </code></td>
    <td>Uma revisão de token é criada.</td></tr><tr>
    <td><code> validatingwebhookconfigurations.create </code></td>
    <td>Uma validação de configuração de webhook é criada.</td></tr><tr>
    <td><code> validatingwebhookconfigurations.delete </code></td>
    <td>Uma validação de configuração de webhook é excluída.</td></tr><tr>
    <td><code> validatingwebhookconfigurations.patch </code></td>
    <td>Uma validação de configuração de webhook é corrigida.</td></tr><tr>
    <td><code> validatingwebhookconfigurations.update </code></td>
    <td>Uma validação de configuração de webhook é atualizada.</td></tr>
</table>
