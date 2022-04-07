---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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
{:note: .note}


# Informações de Versão e ações de atualização
{: #cs_versions}

## Tipos de versão do Kubernetes
{: #version_types}

O {{site.data.keyword.containerlong}} suporta simultaneamente múltiplas versões do Kubernetes. Quando uma versão mais recente (n) é liberada, as versões até 2 atrás (n-2) são suportadas. As versões com mais de 2 atrás do mais recente (n-3) são descontinuadas primeiro e depois não suportadas.
{:shortdesc}

** Versões Suportadas do Kubernetes **:
*   Mais recente: 1.14.2 
*   Padrão: 1.13.6
*   Outro: 1.12.9

** Versões do Kubernetes descontinuadas e não suportadas **:
*   Descontinuado: 1.11
*   Não suportado: 1.5, 1.7, 1.8, 1.9, 1.10 

</br>

**Versões descontinuadas**: quando os clusters estão em execução em uma versão descontinuada do Kubernetes, você tem um mínimo de 30 dias para revisar e atualizar para uma versão suportada do Kubernetes antes de a versão se tornar não suportada. Durante o período de descontinuação, seu cluster ainda está funcional, mas pode requerer atualizações para uma liberação suportada a fim de corrigir vulnerabilidades de segurança. Por exemplo, é possível incluir e recarregar nós do trabalhador, mas não é possível criar novos clusters que usam a versão descontinuada quando a data não suportada está a 30 dias ou menos.

**Versões não suportadas**: se seus clusters executam uma versão do Kubernetes que não é suportada, revise os potenciais impactos de atualização a seguir e, então, imediatamente [atualize o cluster](/docs/containers?topic=containers-update#update) para continuar recebendo atualizações de segurança importantes e suporte. Os clusters não suportados não podem incluir ou recarregar nós do trabalhador existentes. É possível descobrir se o seu cluster é **não suportado** revisando o campo **Estado** na saída do comando `ibmcloud ks clusters` ou no [console do {{site.data.keyword.containerlong_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/kubernetes/clusters).

Se você esperar até que seu cluster tenha três ou mais versões secundárias para trás da versão suportada mais antiga, não será possível atualizar o cluster. Em vez disso, [crie um novo cluster](/docs/containers?topic=containers-clusters#clusters), [implemente seus apps](/docs/containers?topic=containers-app#app) no novo cluster e [exclua](/docs/containers?topic=containers-remove) o cluster não suportado.<br><br>Para evitar esse problema, atualize os clusters descontinuados para uma versão suportada menor que três à frente da versão atual, como da 1.11 até a 1.12, e, em seguida, atualize para a versão mais recente, a 1.14. Se os nós do trabalhador executarem uma versão três ou mais atrás do principal, você poderá ver seus pods falharem inserindo um estado como `MatchNodeSelector`, `CrashLoopBackOff` ou `ContainerCreating` até atualizar os nós do trabalhador para a mesma versão que o principal. Depois de atualizar de uma versão descontinuada para uma suportada, seu cluster pode continuar as operações normais e continuar recebendo suporte.
{: important}

</br>

Para verificar a versão do servidor de um cluster, execute o comando a seguir.
```
kubectl version  --short | grep -i server
```
{: pre}

Saída de exemplo:
```
Server Version: v1.13.6+IKS
```
{: screen}


## Tipos de atualização
{: #update_types}

Seu cluster do Kubernetes possui três tipos de atualizações: principais, secundárias e de correção.
{:shortdesc}

|Tipo de atualização|Exemplos de rótulos de versão|Atualizado por|Impacto
|-----|-----|-----|-----|
|Principal|1.x.x|Você|Mudanças de operação para clusters, incluindo scripts ou implementações.|
|Menor|x.9.x|Você|Mudanças de operação para clusters, incluindo scripts ou implementações.|
|Correção|x.x.4_1510|IBM e você|Correções do Kubernetes, bem como outras atualizações de componentes do Provedor {{site.data.keyword.Bluemix_notm}}, como correções de segurança e do sistema operacional. A IBM atualiza os mestres automaticamente, mas você aplica correções a nós do trabalhador. Veja mais sobre correções na seção a seguir.|
{: caption="Impactos de atualizações do Kubernetes" caption-side="top"}

À medida que atualizações são disponibilizadas, você recebe notificações ao visualizar as informações sobre o nó do trabalhador, como com os comandos `ibmcloud ks workers --cluster <cluster>` ou `ibmcloud ks worker-get --cluster <cluster> --worker <worker>`.
-  **Atualizações principais e secundárias (1.x)**: primeiro, [atualize seu nó principal](/docs/containers?topic=containers-update#master) e, em seguida, [atualize os nós do trabalhador](/docs/containers?topic=containers-update#worker_node). Os nós do Trabalhador não podem executar uma versão principal ou secundária do Kubernetes que é maior que os mestres.
   - Não é possível atualizar um mestre do Kubernetes três ou mais versões secundárias à frente. Por exemplo, se o seu principal atual está na versão 1.11 e você deseja atualizar para a 1.14, deve-se atualizar para a 1.12 primeiro.
   - Se você usa uma versão CLI do `kubectl` que corresponde pelo menos à versão `major.minor` de seus clusters, você pode ter resultados inesperados. Certifique-se de manter seu cluster do Kubernetes e as [versões da CLI](/docs/containers?topic=containers-cs_cli_install#kubectl) atualizados.
-  **Atualizações de correção (x.x.4_1510)**: as mudanças nas correções são documentadas no [log de mudanças da versão](/docs/containers?topic=containers-changelog). As correções principais são aplicadas automaticamente, mas você inicia as atualizações de correções do nó do trabalhador. Os nós do trabalhador também podem executar versões de correção que são maiores que as principais. Conforme as atualizações são disponibilizadas, você é notificado quando visualiza informações sobre os nós principal e do trabalhador no console ou na CLI do {{site.data.keyword.Bluemix_notm}}, assim como com os comandos a seguir: `ibmcloud ks clusters`, `cluster-get`, `workers` ou `worker-get`.
   - **Correções do nó do trabalhador**: verifique mensalmente se uma atualização está disponível e use o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update` ou o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` para aplicar essas correções de segurança e do sistema operacional. Durante uma atualização ou um recarregamento, a imagem da máquina do nó do trabalhador será recriada e os dados serão excluídos se não [armazenados fora do nó do trabalhador](/docs/containers?topic=containers-storage_planning#persistent_storage_overview).
   - **Correções de mestre**: as correções de mestre são aplicadas automaticamente ao longo do curso de vários dias, portanto, uma versão de correção de mestre pode aparecer como disponível antes de ser aplicada ao seu mestre. A automação de atualização também ignora clusters que estão em um estado não funcional ou têm operações atualmente em andamento. Ocasionalmente, a IBM pode desativar as atualizações automáticas para um fix pack de mestre específico, conforme observado no log de mudanças, como uma correção que será necessária somente se um mestre for atualizado de uma versão secundária para outra. Em qualquer um desses casos, é possível escolher usar com segurança o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update) `ibmcloud ks cluster-update` sem esperar que a automação de atualização se aplique.

</br>

{: #prep-up}
Essas informações resumem as atualizações que provavelmente têm impacto em apps implementados ao atualizar um cluster da versão anterior para uma nova versão.
-  [Ações de preparação](#cs_v114) da versão 1.14.
-  [ Ações de preparação ](#cs_v113) da versão 1.13.
-  [Ações de preparação](#cs_v112) da Versão 1.12.
-  **Descontinuado**: [ações de preparação](#cs_v111) da versão 1.11.
-  [ Archive ](#k8s_version_archive)  de versões não suportadas.

<br/>

Para obter uma lista completa de mudanças, revise as informações a seguir:
* [Log de mudanças do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).
* [Log de mudanças de versão da IBM](/docs/containers?topic=containers-changelog).

</br>

## Histórico de liberação
{: #release-history}

A tabela a seguir registra o histórico de liberação da versão do {{site.data.keyword.containerlong_notm}}. É possível usar essas informações para propósitos de planejamento, como para estimar os períodos de tempo gerais quando uma determinada liberação pode se tornar não suportada. Depois que a comunidade do Kubernetes libera uma atualização de versão, a equipe da IBM inicia um processo de reforço e teste da liberação para ambientes do {{site.data.keyword.containerlong_notm}}. As datas de liberação disponíveis e não suportadas dependem dos resultados desses testes, das atualizações de comunidade, das correções de segurança e das mudanças de tecnologia entre as versões. Planeje manter a versão do cluster principal e do nó do trabalhador atualizada de acordo com a política de suporte de versão `n-2`.
{: shortdesc}

O {{site.data.keyword.containerlong_notm}} foi primeiro geralmente disponível com o Kubernetes versão 1.5. As datas de liberação projetadas ou não suportadas estão sujeitas a mudança. Para ir para as etapas de preparação de atualização de versão, clique no número da versão.

As datas marcadas com o símbolo (`†`) são tentativas e estão sujeitas a mudança.
{: important}

<table summary="Esta tabela mostra o histórico de liberação para o {{site.data.keyword.containerlong_notm}}.">
<caption>Histórico de liberação para o  {{site.data.keyword.containerlong_notm}}.</caption>
<col width="20%" align="center">
<col width="20%">
<col width="30%">
<col width="30%">
<thead>
<tr>
<th>Suportado?</th>
<th>Versão</th>
<th>{{site.data.keyword.containerlong_notm}}<br>data de liberação</th>
<th>{{site.data.keyword.containerlong_notm}}<br>data não suportada</th>
</tr>
</thead>
<tbody>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="This version is supported."/></td>
  <td>[1.14](#cs_v114)</td>
  <td>07 de maio de 2019</td>
  <td>Março de 2020 `†`</td>
</tr>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="This version is supported."/></td>
  <td>[ 1,13 ](#cs_v113)</td>
  <td>05 de fevereiro de 2019</td>
  <td>Dec 2019  ` † `</td>
</tr>
<tr>
  <td><img src="images/checkmark-filled.png" align="left" width="32" style="width:32px;" alt="This version is supported."/></td>
  <td>[1.12](#cs_v112)</td>
  <td>07 de novembro de 2018</td>
  <td>Sep 2019  ` † `</td>
</tr>
<tr>
  <td><img src="images/warning-filled.png" align="left" width="32" style="width:32px;" alt="This version is deprecated."/></td>
  <td>[ 1,11 ](#cs_v111)</td>
  <td>14 de agosto de 2018</td>
  <td>27 de junho de 2019 `†`</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="This version is unsupported."/></td>
  <td>[ 1.10 ](#cs_v110)</td>
  <td>01 de maio de 2018</td>
  <td>16 de maio de 2019</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="This version is unsupported."/></td>
  <td>[ 1,9 ](#cs_v19)</td>
  <td>08 de fevereiro de 2018</td>
  <td>27 de dezembro de 2018</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="This version is unsupported."/></td>
  <td>[ 1.8 ](#cs_v18)</td>
  <td>08 de novembro de 2017</td>
  <td>22 de Sep de 2018</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="This version is unsupported."/></td>
  <td>[ 1,7 ](#cs_v17)</td>
  <td>19 de fevereiro de 2017</td>
  <td>21 de junho de 2018</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="This version is unsupported."/></td>
  <td>1,6</td>
  <td>N/A</td>
  <td>N/A</td>
</tr>
<tr>
  <td><img src="images/close-filled.png" align="left" width="32" style="width:32px;" alt="This version is unsupported."/></td>
  <td>[ 1,5 ](#cs_v1-5)</td>
  <td>23 maio 2017</td>
  <td>04 de abril de 2018</td>
</tr>
</tbody>
</table>

<br />


## Versão 1.14
{: #cs_v114}

<p><img src="images/certified_kubernetes_1x14.png" style="padding-right: 10px;" align="left" alt="Esse badge indica a certificação do Kubernetes versão 1.14 para o {{site.data.keyword.containerlong_notm}}."/> O {{site.data.keyword.containerlong_notm}} é um produto Kubernetes certificado para a versão 1.14 sob o programa CNCF Kubernetes Software Conformance Certification. _Kubernetes® é uma marca registrada da Linux Foundation nos Estados Unidos e em outros países e é usada nos termos de uma licença da Linux Foundation._</p>

Revise as mudanças que você pode precisar fazer quando atualizar da versão anterior do Kubernetes para a 1.14.
{: shortdesc}

O Kubernetes 1.14 apresenta novos recursos para você explorar. Experimente o novo [projeto `kustomize` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes-sigs/kustomize) que pode ser usado para gravar, customizar e reutilizar as configurações de YAML do recurso Kubernetes. Ou dê uma olhada nos novos [docs da CLI `kubectl` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubectl.docs.kubernetes.io/).
{: tip}

### Atualizar antes do mestre
{: #114_before}

A tabela a seguir mostra as ações que devem ser executadas antes de atualizar o mestre do Kubernetes.
{: shortdesc}

<table summary="Atualizações do Kubernetes para a versão 1.14">
<caption>Mudanças a serem feitas antes de atualizar o principal para o Kubernetes 1.14</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Mudança de estrutura do diretório de log de pod do CRI</td>
<td>O container runtime interface (CRI) mudou a estrutura do diretório de log de pod de `/var/log/pods/<UID>` para `/var/log/pods/<NAMESPACE_NAME_UID>`. Se seus apps efetuarem bypass do Kubernetes e o CRI para acessar os logs de pod diretamente nos nós do trabalhador, atualize-os para manipular as estruturas de diretório. O acesso aos logs de pod via Kubernetes, por exemplo, executando `kubectl logs`, não é afetado por essa mudança.</td>
</tr>
<tr>
<td>As verificações de funcionamento não seguem mais os redirecionamentos</td>
<td>As análises de vivacidade e prontidão de verificação de funcionamento que usam um `HTTPGetAction` não seguem mais os redirecionamentos para nomes de host que são diferentes da solicitação de análise original. Em vez disso, esses redirecionamentos não locais retornam uma resposta `Success` e um evento com a razão `ProbeWarning` é gerado para indicar que o redirecionamento foi ignorado. Se você dependia anteriormente do redirecionamento para executar verificações de funcionamento com relação a diferentes terminais de nome do host, a lógica de verificação de funcionamento deve ser executada fora do `kubelet`. Por exemplo, você pode fazer proxy do terminal externo em vez de redirecionar a solicitação de análise.</td>
</tr>
<tr>
<td>Não suportado: provedor DNS do cluster KubeDNS</td>
<td>O CoreDNS é agora o único provedor DNS do cluster suportado para clusters que executam o Kubernetes versão 1.14 e mais recente. Se você atualizar um cluster existente que usa KubeDNS como o provedor DNS do cluster para a versão 1.14, o KubeDNS será migrado automaticamente para o CoreDNS durante a atualização. Portanto, antes de atualizar o cluster, considere [configurar o CoreDNS como o provedor DNS do cluster](/docs/containers?topic=containers-cluster_dns#set_coredns) e testá-lo.<br><br>O CoreDNS suporta a [especificação DNS do cluster ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/dns/blob/master/docs/specification.md#25---records-for-external-name-services) para inserir um nome de domínio como o campo `ExternalName` do serviço do Kubernetes. O provedor DNS anterior do cluster, KubeDNS, não segue a especificação de DNS do cluster e, como tal, permite endereços IP para `ExternalName`. Se quaisquer serviços do Kubernetes estiverem usando endereços IP em vez de DNS, deve-se atualizar o `ExternalName` para DNS para a funcionalidade contínua.</td>
</tr>
<tr>
<td>Não suportado: recurso alfa `Initializers` do Kubernetes</td>
<td>O recurso alfa `Initializers` do Kubernetes, a versão da API `admissionregistration.k8s.io/v1alpha1`, o plug-in do controlador de admissão `Inicializadores` e o uso do campo da API `metadata.initializers` são removidos. Se você usar `Initializers`, alterne para usar [webhooks de admissão do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/) e exclua quaisquer objetos de API `InitializerConfiguration` existentes antes de atualizar o cluster.</td>
</tr>
<tr>
<td>Não suportado: contaminações de alfa do nó</td>
<td>O uso de contaminações `node.alpha.kubernetes.io/notReady` e `node.alpha.kubernetes.io/unreachable` não é mais suportado. Se você depende dessas contaminações, atualize seus apps para usar as contaminações `node.kubernetes.io/not-ready` e `node.kubernetes.io/unreachable` no lugar.</td>
</tr>
<tr>
<td>Não suportado: os documentos swagger da API do Kubernetes</td>
<td>Os docs de API do esquema `swagger/*`, `/swagger.json` e `/swagger-2.0.0.pb-v1` estão agora removidos em favor dos documentos de API do esquema `/openapi/v2`. Os docs do swagger foram descontinuados quando os docs do OpenAPI se tornaram disponíveis no Kubernetes versão 1.10. Além disso, o servidor de API do Kubernetes agrega agora somente os esquemas do OpenAPI de terminais `/openapi/v2` de servidores de API agregados. O fallback para agregar de `/swagger.json` foi removido. Se você instalou apps que fornecem extensões da API do Kubernetes, assegure-se de que seus apps suportem os docs de API do esquema `/openapi/v2`.</td>
</tr>
<tr>
<td>Não suportado e descontinuado: selecionar métricas</td>
<td>Revise as [métricas do Kubernetes removidas e descontinuadas ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.14.md#removed-and-deprecated-metrics). Se você usar qualquer uma dessas métricas descontinuadas, mude para a métrica de substituição disponível.</td>
</tr>
</tbody>
</table>

### Atualizar depois do mestre
{: #114_after}

A tabela a seguir mostra as ações que devem ser executadas depois de atualizar o mestre do Kubernetes.
{: shortdesc}

<table summary="Atualizações do Kubernetes para a versão 1.14">
<caption>Mudanças a serem feitas depois de atualizar o principal para Kubernetes 1.14</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Não suportado: `kubectl --show-all`</td>
<td>A sinalização `--show-all` e a sinalização abreviada `-a` não são mais suportadas. Se os seus scripts dependem dessas sinalizações, atualize-os.</td>
</tr>
<tr>
<td>Políticas RBAC padrão do Kubernetes para usuários não autenticados</td>
<td>As políticas padrão de controle de acesso baseado na função (RBAC) do Kubernetes não concedem mais acesso às [APIs de descoberta e de verificação de permissão para usuários não autenticados ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles). Essa mudança se aplica apenas aos novos clusters da versão 1.14. Se você atualizar um cluster de uma versão anterior, os usuários não autenticados ainda terão acesso às APIs de descoberta e de verificação de permissão. Se você desejar atualizar para o padrão mais seguro para usuários não autenticados, remova o grupo `system:unauthenticated` das ligações de função de cluster `system:basic-user` e `system:discovery`.</td>
</tr>
<tr>
<td>Descontinuado: consultas do Prometheus que usam os rótulos `pod_name` e `container_name`</td>
<td>Atualize quaisquer consultas do Prometheus que correspondam aos rótulos `pod_name` ou `container_name` para usar os rótulos `pod` ou `container` no lugar. As consultas de exemplo que podem usar esses rótulos descontinuados incluem métricas de análise do kubelet. Os rótulos `pod_name` e `container_name` descontinuados se tornarão não suportados na próxima liberação do Kubernetes.</td>
</tr>
</tbody>
</table>

<br />


## Versão 1.13
{: #cs_v113}

<p><img src="images/certified_kubernetes_1x13.png" style="padding-right: 10px;" align="left" alt="Esse badge indica a certificação do Kubernetes versão 1.13 para o {{site.data.keyword.containerlong_notm}}."/> O {{site.data.keyword.containerlong_notm}} é um produto Kubernetes certificado para a versão 1.13 sob o programa CNCF Kubernetes Software Conformance Certification. _Kubernetes® é uma marca registrada da Linux Foundation nos Estados Unidos e em outros países e é usada nos termos de uma licença da Linux Foundation._</p>

Revise as mudanças que você pode precisar fazer quando atualizar por meio da versão anterior do Kubernetes para a 1.13.
{: shortdesc}

### Atualizar antes do mestre
{: #113_before}

A tabela a seguir mostra as ações que devem ser executadas antes de atualizar o mestre do Kubernetes.
{: shortdesc}

<table summary="Kubernetes updates for version 1.13">
<caption>Mudanças a serem feitas antes de atualizar o mestre para o Kubernetes 1.13</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>N/A</td>
<td></td>
</tr>
</tbody>
</table>

### Atualizar depois do mestre
{: #113_after}

A tabela a seguir mostra as ações que devem ser executadas depois de atualizar o mestre do Kubernetes.
{: shortdesc}

<table summary="Kubernetes updates for version 1.13">
<caption>Mudanças a serem feitas depois de atualizar o mestre para o Kubernetes 1.13</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoreDNS disponível como o novo provedor DNS do cluster padrão</td>
<td>O CoreDNS é agora o provedor DNS de cluster padrão para novos clusters em Kubernetes 1.13 e posterior. Se você atualizar um cluster existente para 1.13 que usa KubeDNS como o provedor DNS do cluster, o KubeDNS continuará a ser o provedor DNS do cluster. No entanto, é possível escolher [usar o CoreDNS no lugar](/docs/containers?topic=containers-cluster_dns#dns_set).
<br><br>O CoreDNS suporta a [especificação DNS do cluster ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/dns/blob/master/docs/specification.md#25---records-for-external-name-services) para inserir um nome de domínio como o campo `ExternalName` do serviço do Kubernetes. O provedor DNS anterior do cluster, KubeDNS, não segue a especificação de DNS do cluster e, como tal, permite endereços IP para `ExternalName`. Se quaisquer serviços do Kubernetes estiverem usando endereços IP em vez de DNS, deve-se atualizar o `ExternalName` para DNS para a funcionalidade contínua.</td>
</tr>
<tr>
<td>Saída ` kubectl `  para  ` Implementação `  e  ` StatefulSet `</td>
<td>A saída `kubectl` para `Deployment` e `StatefulSet` agora inclui uma coluna `Ready` e é mais legível. Se seus scripts dependem do comportamento anterior, atualize-os.</td>
</tr>
<tr>
<td>Saída ` kubectl `  para  ` PriorityClass `</td>
<td>A saída `kubectl` para `PriorityClass` agora inclui uma coluna `Valor`. Se seus scripts dependem do comportamento anterior, atualize-os.</td>
</tr>
<tr>
<td>` kubectl get componentstatuses `</td>
<td>O comando `kubectl get componentstatuses` não relata adequadamente o funcionamento de alguns componentes principais do Kubernetes porque esses componentes não são mais acessíveis no servidor de API do Kubernetes agora que o `localhost` e as portas inseguras (HTTP) estão desativadas. Após a introdução de principais altamente disponíveis (HA) no Kubernetes versão 1.10, cada mestre do Kubernetes é configurado com múltiplas instâncias de `apiserver`, `controller-manager`, `scheduler` e `etcd`. Em vez disso, revise o funcionamento do cluster verificando o [console do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/kubernetes/landing) ou usando o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get) `ibmcloud ks cluster-get`.</td>
</tr>
<tr>
<tr>
<td>Não suportado:  ` kubectl run-container `</td>
<td>O comando  ` kubectl run-container `  é removido. Em vez disso, use o comando  ` kubectl run ` .</td>
</tr>
<tr>
<td>` kubectl rollout undo `</td>
<td>Ao executar `kubectl rollout undo` para uma revisão que não existe, um erro é retornado. Se seus scripts dependem do comportamento anterior, atualize-os.</td>
</tr>
<tr>
<td>Descontinuada: Anotação  ` scheduler.alpha.kubernetes.io/critical-pod `</td>
<td>A anotação  ` scheduler.alpha.kubernetes.io/critical-pod `  está agora descontinuada. Mude os pods que dependem dessa anotação para usar a [prioridade do pod](/docs/containers?topic=containers-pod_priority#pod_priority) no lugar.</td>
</tr>
</tbody>
</table>

### Atualizar após os nós do trabalhador
{: #113_after_workers}

A tabela a seguir mostra as ações que você deve executar depois de atualizar os nós do trabalhador.
{: shortdesc}

<table summary="Kubernetes updates for version 1.13">
<caption>Mudanças a serem feitas depois de atualizar os nós do trabalhador para o Kubernetes 1.13</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd  ` cri `  stream server</td>
<td>No versão 1.2 conteinerizada, o servidor de fluxo de plug-in `cri` agora serve em uma porta aleatória, `http://localhost:0`. Essa mudança suporta o proxy de fluxo `kubelet` e fornece uma interface de fluxo mais segura para as operações `exec` e `logs` do contêiner. Anteriormente, o servidor de fluxo `cri` atendia a interface de rede privada do nó do trabalhador usando a porta 10010. Se seus apps usam o plug-in `cri` do contêiner e contam com o comportamento anterior, atualize-os.</td>
</tr>
</tbody>
</table>

<br />


## Versão 1.12
{: #cs_v112}

<p><img src="images/certified_kubernetes_1x12.png" style="padding-right: 10px;" align="left" alt="Este badge indica a certificação do Kubernetes versão 1.12 para o IBM Cloud Container Service."/> O {{site.data.keyword.containerlong_notm}} é um produto Kubernetes certificado para a versão 1.12 sob o programa CNCF Kubernetes Software Conformance Certification. _Kubernetes® é uma marca registrada da Linux Foundation nos Estados Unidos e em outros países e é usada nos termos de uma licença da Linux Foundation._</p>

Revise as mudanças que você pode precisar fazer ao atualizar da versão anterior do Kubernetes para a 1.12.
{: shortdesc}

### Atualizar antes do mestre
{: #112_before}

A tabela a seguir mostra as ações que devem ser executadas antes de atualizar o mestre do Kubernetes.
{: shortdesc}

<table summary="Atualizações do Kubernetes para a versão 1.12">
<caption>Mudanças a serem feitas antes de atualizar o mestre para o Kubernetes 1.12</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes Metrics Server</td>
<td>Se você tiver atualmente os Kubernetes `metric-server` implementados em seu cluster, deve-se remover o `metric-server` antes de atualizar o cluster para o Kubernetes 1.12. Essa remoção evita conflitos com o `metric-server` que é implementado durante a atualização.</td>
</tr>
<tr>
<td>Ligações de função para a conta de serviço `kube-system` `default`</td>
<td>A conta de serviço `kube-system` `default` não tem mais acesso **cluster-admin** à API do Kubernetes. Se você implementar recursos ou complementos, como o [Helm](/docs/containers?topic=containers-helm#public_helm_install), que requerem acesso a processos em seu cluster, configure uma [conta de serviço ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/). Se você precisar de tempo para criar e configurar contas de serviço individuais com as permissões apropriadas, será possível conceder temporariamente a função **cluster-admin** com a ligação de função de cluster a seguir: `kubectl create clusterrolebinding kube-system:default --clusterrole=cluster-admin --serviceaccount=kube-system:default`</td>
</tr>
</tbody>
</table>

### Atualizar depois do mestre
{: #112_after}

A tabela a seguir mostra as ações que devem ser executadas depois de atualizar o mestre do Kubernetes.
{: shortdesc}

<table summary="Atualizações do Kubernetes para a versão 1.12">
<caption>Mudanças a serem feitas depois de atualizar o mestre para o Kubernetes 1.12</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>APIs para Kubernetes</td>
<td>A API do Kubernetes substitui as APIs descontinuadas da seguinte forma:
<ul><li><strong>apps/v1</strong>: a API `apps/v1` do Kubernetes substitui as APIs `apps/v1beta1` e `apps/v1alpha`. A API `apps/v1` também substitui a API `extensions/v1beta1` para os recursos `daemonset`, `deployment`, `replicaset` e `statefulset`. O projeto do Kubernetes está sendo descontinuado e o suporte descontinuado gradualmente para as APIs anteriores do `apiserver` e do cliente `kubectl`.</li>
<li><strong>networking.k8s.io/v1</strong>: a API `networking.k8s.io/v1` substitui a API `extensions/v1beta1` para recursos NetworkPolicy.</li>
<li><strong>policy/v1beta1</strong>: a API `policy/v1beta1` substitui a API `extensions/v1beta1` para recursos `podsecuritypolicy`.</li></ul>
<br><br>Atualize todos os seus campos YAML `apiVersion` para usar a API do Kubernetes apropriada antes que as APIs descontinuadas se tornem não suportadas. Além disso, revise os [docs do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) para mudanças que estão relacionadas a `apps/v1`, como as seguintes.
<ul><li>Após a criação de uma implementação, o campo `.spec.selector` fica imutável.</li>
<li>O campo `.spec.rollbackTo` está descontinuado. Em vez disso, use o comando `kubectl rollout undo`.</li></ul></td>
</tr>
<tr>
<td>CoreDNS disponível como provedor DNS de cluster</td>
<td>O projeto Kubernetes está no processo de executar a transição para suportar o CoreDNS em vez do DNS atual do Kubernetes (KubeDNS). Na versão 1.12, o DNS do cluster padrão permanece KubeDNS, mas é possível [optar por usar o CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>`kubectl apply --force`</td>
<td>Agora, quando você força uma ação de aplicação (`kubectl apply --force`) em recursos que não podem ser atualizados, como campos imutáveis em arquivos YAML, os recursos são recriados em vez disso. Se seus scripts dependem do comportamento anterior, atualize-os.</td>
</tr>
<tr>
<td>` kubectl get componentstatuses `</td>
<td>O comando `kubectl get componentstatuses` não relata adequadamente o funcionamento de alguns componentes principais do Kubernetes porque esses componentes não são mais acessíveis no servidor de API do Kubernetes agora que o `localhost` e as portas inseguras (HTTP) estão desativadas. Após a introdução de principais altamente disponíveis (HA) no Kubernetes versão 1.10, cada mestre do Kubernetes é configurado com múltiplas instâncias de `apiserver`, `controller-manager`, `scheduler` e `etcd`. Em vez disso, revise o funcionamento do cluster verificando o [console do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/kubernetes/landing) ou usando o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get) `ibmcloud ks cluster-get`.</td>
</tr>
<tr>
<td>`kubectl logs --interactive`</td>
<td>O sinalizador `--interactive` não é mais suportado para os logs `kubectl logs`. Atualize qualquer automação que use esse sinalizador.</td>
</tr>
<tr>
<td>`kubectl patch`</td>
<td>Se o comando `patch` resultar em nenhuma mudança (uma correção redundante), o comando não sairá mais com um código de retorno `1`. Se seus scripts dependem do comportamento anterior, atualize-os.</td>
</tr>
<tr>
<td>`kubectl version -c`</td>
<td>O sinalizador de abreviação `-c` não é mais suportado. Em vez disso, use o sinalizador `--client` integral. Atualize qualquer automação que use esse sinalizador.</td>
</tr>
<tr>
<td>`kubectl wait`</td>
<td>Se nenhum seletor correspondente for localizado, o comando agora imprimirá uma mensagem de erro e sairá com um código de retorno `1`. Se seus scripts dependem do comportamento anterior, atualize-os.</td>
</tr>
<tr>
<td>kubelet cAdvisor port</td>
<td>A UI da web do [Container Advisor (cAdvisor) ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/google/cadvisor) que o kubelet usado iniciando o `--cadvisor-port` é removida do Kubernetes 1.12. Se ainda for necessário executar o cAdvisor, [implemente o cAdvisor como um conjunto de daemon ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/google/cadvisor/tree/master/deploy/kubernetes).<br><br>No conjunto de daemon, especifique a seção de portas para que o cAdvisor possa ser acessado por meio de `http://node-ip:4194`, como a seguir. Os pods do cAdvisor falham até que os nós do trabalhador sejam atualizados para 1.12 porque as versões anteriores do kubelet usam a porta do host 4194 para o cAdvisor.
<pre class="screen"><code> portas:
          - name: http
            containerPort: 8080
            hostPort: 4194
            protocol: TCP</code></pre></td>
</tr>
<tr>
<td>Painel do Kubernetes</td>
<td>Se você acessar o painel por meio de `kubectl proxy`, o botão **SKIP** na página de login será removido. Em vez disso, [use um **Token** para efetuar login](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td id="metrics-server">Kubernetes Metrics Server</td>
<td>O Kubernetes Metrics Server substitui o Kubernetes Heapster (descontinuado desde o Kubernetes versão 1.8) como o provedor de métricas do cluster. Se você executar mais de 30 pods por nó do trabalhador em seu cluster, [ajuste a configuração de `metrics-server` para desempenho](/docs/containers?topic=containers-kernel#metrics).
<p>O painel do Kubernetes não funciona com o `metrics-server`. Se você desejar exibir métricas em um painel, escolha entre as opções a seguir.</p>
<ul><li>[Configure Grafana para analisar métricas](/docs/services/cloud-monitoring/tutorials?topic=cloud-monitoring-container_service_metrics#container_service_metrics) usando o Painel de monitoramento do cluster.</li>
<li>Implemente [Heapster ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/heapster) em seu cluster.
<ol><li>Copie os arquivos `heapster-rbac` [YAML ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/heapster-rbac.yaml), `heapster-service` [YAML ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/standalone/heapster-service.yaml) e `heapster-controller` [YAML ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/blob/release-1.12/cluster/addons/cluster-monitoring/standalone/heapster-controller.yaml).</li>
<li>Edite o YAML `heapster-controller`, substituindo as sequências a seguir.
<ul><li>Substitua `{{ nanny_memory }}` por `90Mi`</li>
<li>Substitua `{{ base_metrics_cpu }}` por `80m`</li>
<li>Substitua `{{ metrics_cpu_per_node }}` por `0.5m`</li>
<li>Substitua `{{ base_metrics_memory }}` por `140Mi`</li>
<li>Substitua `{{ metrics_memory_per_node }}` por `4Mi`</li>
<li>Substitua `{{ heapster_min_cluster_size }}` por `16`</li></ul></li>
<li>Aplique os arquivos YAML `heapster-rbac`, `heapster-service` e `heapster-controller` em seu cluster executando o comando `kubectl apply -f`.</li></ol></li></ul></td>
</tr>
<tr>
<td>API do Kubernetes `rbac.authorization.k8s.io/v1`</td>
<td>A API do Kubernetes `rbac.authorization.k8s.io/v1` (suportada desde o Kubernetes 1.8) está substituindo a API `rbac.authorization.k8s.io/v1alpha1` e `rbac.authorization.k8s.io/v1beta1`. Não é mais possível criar objetos RBAC, tais como funções ou ligações de função com a API `v1alpha` não suportada. Os objetos RBAC existentes são convertidos para a API `v1`.</td>
</tr>
</tbody>
</table>

<br />


## Descontinuado: versão 1.11
{: #cs_v111}

<p><img src="images/certified_kubernetes_1x11.png" style="padding-right: 10px;" align="left" alt="Esse badge indica a certificação do Kubernetes versão 1.11 para o IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} é um produto Kubernetes certificado para a versão 1.11 sob o programa CNCF Kubernetes Software Conformance Certification. _Kubernetes® é uma marca registrada da Linux Foundation nos Estados Unidos e em outros países e é usada nos termos de uma licença da Linux Foundation._</p>

Revise as mudanças que você pode precisar fazer quando atualizar da versão anterior do Kubernetes para a 1.11.
{: shortdesc}

O Kubernetes versão 1.11 foi descontinuado e torna-se não suportado em 27 de junho de 2019 (tentativa). [Revise o impacto potencial](/docs/containers?topic=containers-cs_versions#cs_versions) de cada atualização de versão do Kubernetes e, em seguida, [atualize seus clusters](/docs/containers?topic=containers-update#update) imediatamente para pelo menos 1.12.
{: deprecated}

Antes de poder atualizar com êxito um cluster do Kubernetes versão 1.9 ou anterior para a versão 1.11, deve-se seguir as etapas listadas em [Preparando para atualizar para o Calico v3](#111_calicov3).
{: important}

### Atualizar antes do mestre
{: #111_before}

A tabela a seguir mostra as ações que devem ser executadas antes de atualizar o mestre do Kubernetes.
{: shortdesc}

<table summary="Kubernetes updates for version 1.11">
<caption>Mudanças a serem feitas antes de você atualizar o mestre para o Kubernetes 1.11</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração de alta disponibilidade (HA) do cluster mestre</td>
<td>Atualizada a configuração do cluster mestre para aumentar a alta disponibilidade (HA). Os clusters agora têm três réplicas principais do Kubernetes configuradas com cada mestre implementado em hosts físicos separados. Além disso, se o cluster estiver em uma zona com capacidade para múltiplas zonas, os mestres serão difundidos pelas zonas.<br><br>Para ações que devem ser executadas, consulte [Atualizando para os clusters mestres altamente disponíveis](#ha-masters). Estas ações de preparação se aplicam:<ul>
<li>Se você tiver um firewall ou políticas de rede customizadas do Calico.</li>
<li>Se você estiver usando as portas do host `2040` ou `2041` nos nós do trabalhador.</li>
<li>Se você usou o endereço IP do cluster mestre para acesso no cluster ao mestre.</li>
<li>Se você tiver automação que chame a API ou a CLI do Calico (`calicoctl`), tal como para criar políticas do Calico.</li>
<li>Se você usar políticas de rede do Kubernetes ou do Calico para controlar o acesso de egresso do pod ao mestre.</li></ul></td>
</tr>
<tr>
<td>` containerd `  novo tempo de execução do contêiner do Kubernetes</td>
<td><p class="important">O `containerd` substitui o Docker como o novo tempo de execução do contêiner para Kubernetes. Para ações que devem ser executadas, consulte [Atualizando para `containerd` como o tempo de execução do contêiner](#containerd).</p></td>
</tr>
<tr>
<td>Criptografando dados em etcd</td>
<td>Anteriormente, os dados etcd eram armazenados em uma instância de armazenamento de arquivo NFS do mestre criptografada em repouso. Agora, os dados etcd são armazenados no disco local do mestre e submetidos a backup no {{site.data.keyword.cos_full_notm}}. Os dados são criptografados durante o trânsito para o {{site.data.keyword.cos_full_notm}} e em repouso. No entanto, os dados etcd no disco local do mestre não são criptografados. Se você desejar que os dados etcd locais do mestre sejam criptografados, [ative o {{site.data.keyword.keymanagementservicelong_notm}} em seu cluster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
<tr>
<td>Propagação de montagem do volume do contêiner do Kubernetes</td>
<td>O valor padrão para o [campo `mountPropagation` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/storage/volumes/#mount-propagation) para um contêiner `VolumeMount` mudou de `HostToContainer` para `None`. Essa mudança restaura o comportamento que existia no Kubernetes versão 1.9 e anterior. Se as suas especificações de pod dependem de `HostToContainer` ser o padrão, atualize-as.</td>
</tr>
<tr>
<td>Desserializador JSON do servidor da API do Kubernetes</td>
<td>O desserializador JSON do servidor da API do Kubernetes agora está com distinção entre maiúsculas e minúsculas. Essa mudança restaura o comportamento que existia no Kubernetes versão 1.7 e anterior. Se as suas definições de recurso JSON usam as maiúsculas e minúsculas incorretas, atualize-as. <br><br>Somente solicitações do servidor da API do Kubernetes diretas são impactadas. A CLI `kubectl` continua a cumprir chaves com distinção entre maiúsculas e minúsculas no Kubernetes versão 1.7 e mais recente, portanto, se gerenciar estritamente seus recursos com `kubectl`, você não será afetado.</td>
</tr>
</tbody>
</table>

### Atualizar depois do mestre
{: #111_after}

A tabela a seguir mostra as ações que devem ser executadas depois de atualizar o mestre do Kubernetes.
{: shortdesc}

<table summary="Kubernetes updates for version 1.11">
<caption>Mudanças a serem feitas depois de atualizar o mestre para o Kubernetes 1.11</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração de criação de log</td>
<td>O complemento de cluster `fluentd` é atualizado automaticamente com a versão 1.11, mesmo quando `logging-autoupdate` está desativado.<br><br>
O diretório de log do contêiner foi mudado de `/var/lib/docker/` para `/var/log/pods/`. Se você usa sua própria solução de criação de log que monitora o diretório anterior, atualize adequadamente.</td>
</tr>
<tr>
<td>Suporte ao {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)</td>
<td>Clusters que executam o Kubernetes versão 1.11 ou mais recente suportam [grupos de acesso](/docs/iam?topic=iam-groups#groups) e [IDs de serviço](/docs/iam?topic=iam-serviceids#serviceids) do IAM. Agora, é possível usar esses recursos para [autorizar o acesso ao seu cluster](/docs/containers?topic=containers-users#users).</td>
</tr>
<tr>
<td>Atualizar a configuração do Kubernetes</td>
<td>A configuração do OpenID Connect para o servidor de API do Kubernetes do cluster é atualizada para suportar os grupos de acesso do {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). Como resultado, deve-se atualizar a configuração do Kubernetes do cluster após a atualização principal do Kubernetes v1.11 executando `ibmcloud ks cluster-config --cluster <cluster_name_or_ID>`. Com esse comando, a configuração é aplicada a ligações de função no namespace `default`.<br><br>Se você não atualizar a configuração, as ações do cluster falharão com a mensagem de erro a seguir: `You must be logged in to the server (Unauthorized).`</td>
</tr>
<tr>
<td>Painel do Kubernetes</td>
<td>Se você acessar o painel por meio de `kubectl proxy`, o botão **SKIP** na página de login será removido. Em vez disso, [use um **Token** para efetuar login](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td>CLI ` kubectl `</td>
<td>A CLI `kubectl` para o Kubernetes versão 1.11 requer as APIs `apps/v1`. Como resultado, a CLI `kubectl` v1.11 não funciona para clusters que executam o Kubernetes versão 1.8 ou anterior. Use a versão da CLI `kubectl` que corresponda à versão do servidor da API do Kubernetes de seu cluster.</td>
</tr>
<tr>
<td>` kubectl auth can-i `</td>
<td>Agora, quando um usuário não está autorizado, o comando `kubectl auth can-i` falha com `exit code 1`. Se seus scripts dependem do comportamento anterior, atualize-os.</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>Agora, ao excluir recursos usando critérios de seleção, como rótulos, o comando `kubectl delete` ignora erros `not found` por padrão. Se seus scripts dependem do comportamento anterior, atualize-os.</td>
</tr>
<tr>
<td>Recurso  ` sysctls `  do Kubernetes</td>
<td>A anotação  ` security.alpha.kubernetes.io/sysctls `  agora é ignorada. Em vez disso, o Kubernetes incluiu campos para os objetos `PodSecurityPolicy` e `Pod` para especificar e controlar `sysctls`. Para obter mais informações, consulte [Usando sysctls no Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/administer-cluster/sysctl-cluster/). <br><br>Depois de atualizar o cluster mestre e os trabalhadores, atualize seus objetos `PodSecurityPolicy` e `Pod` para usar os novos campos `sysctls`.</td>
</tr>
</tbody>
</table>

### Atualizando para os mestres de cluster altamente disponíveis no Kubernetes 1.11
{: #ha-masters}

Para clusters que executam o Kubernetes versão 1.10.8_1530, 1.11.3_1531 ou mais recente, a configuração do cluster principal é atualizada para aumentar a alta disponibilidade (HA). Os clusters agora têm três réplicas principais do Kubernetes configuradas com cada mestre implementado em hosts físicos separados. Além disso, se o cluster estiver em uma zona com capacidade para múltiplas zonas, os mestres serão difundidos pelas zonas.
{: shortdesc}

É possível verificar se seu cluster tem uma configuração do principal de HA, verificando a URL do principal do cluster no console ou executando `ibmcloud ks cluster-get --cluster <cluster_name_or_ID`. Se a URL principal tiver um nome do host, como `https://c2.us-south.containers.cloud.ibm.com:xxxxx`, e não um endereço IP, como `https://169.xx.xx.xx:xxxxx`, o cluster terá uma configuração principal de HA. Você pode obter uma configuração principal de HA por causa de uma atualização automática de correção do principal ou aplicando uma atualização manualmente. Em qualquer um dos casos, ainda é necessário revisar os itens a seguir para assegurar que sua rede de cluster esteja configurada para tirar total proveito da configuração.

* Se você tiver um firewall ou políticas de rede customizadas do Calico.
* Se você estiver usando as portas do host `2040` ou `2041` nos nós do trabalhador.
* Se você usou o endereço IP do cluster mestre para acesso no cluster ao mestre.
* Se você tiver automação que chame a API ou a CLI do Calico (`calicoctl`), tal como para criar políticas do Calico.
* Se você usar políticas de rede do Kubernetes ou do Calico para controlar o acesso de egresso do pod ao mestre.

<br>
**Atualizando seu firewall ou as políticas customizadas de rede do host Calico para principais de HA**:</br>
{: #ha-firewall}
Se você usar um firewall ou as políticas de rede do host customizadas do Calico para controlar o egresso dos nós do trabalhador, permita o tráfego de saída para as portas e os endereços IP de todas as zonas dentro da região em que seu cluster está. Consulte [Permitindo que o cluster acesse recursos de infraestrutura e outros serviços](/docs/containers?topic=containers-firewall#firewall_outbound).

<br>
**Reservando as portas de host `2040` e `2041` em seus nós do trabalhador**:</br>
{: #ha-ports}
Para permitir o acesso ao cluster mestre em uma configuração de alta disponibilidade, deve-se deixar as portas do host `2040` e `2041` disponíveis em todos os nós do trabalhador.
* Atualize quaisquer pods com `hostPort` configurado como `2040` ou `2041` para usar portas diferentes.
* Atualize quaisquer pods com `hostNetwork` configurado como `true` que atendam nas portas `2040` ou `2041` para usar portas diferentes.

Para verificar se seus pods estão atualmente usando as portas `2040` ou `2041`, tenha seu cluster como destino e execute o comando a seguir.

```
kubectl get pods --all-namespaces -o yaml | grep -B 3 "hostPort: 204[0,1]"
```
{: pre}

Se você já tiver uma configuração principal de alta disponibilidade, verá resultados para `ibm-master-proxy-*` no espaço de nomes `kube-system`, como no exemplo a seguir. Se outros pods forem retornados, atualize as suas portas.

```
name: ibm-master-proxy-static ports: - containerPort: 2040 hostPort: 2040 name: apiserver protocol: TCP - containerPort: 2041 hostPort: 2041...
```
{: screen}


<br>
**Usando o domínio ou IP de cluster do serviço `kubernetes` para acesso interno ao principal no cluster**:</br>
{: #ha-incluster}
Para acessar o cluster mestre em uma configuração de alta disponibilidade de dentro do cluster, use um dos seguintes:
* O endereço IP do cluster de serviço do `kubernetes`, que por padrão é: `https://172.21.0.1`
* O nome do domínio de serviço do `kubernetes`, que por padrão é: `https://kubernetes.default.svc.cluster.local`

Se anteriormente você usou o endereço IP do cluster mestre, esse método continuará funcionando. No entanto, para melhorar a disponibilidade, atualize para usar o endereço IP ou o nome do domínio do cluster de serviço do `kubernetes`.

<br>
**Configurando o Calico para acesso ao principal fora do cluster com a configuração de HA**:</br>
{: #ha-outofcluster}
Os dados armazenados no configmap `calico-config` no namespace `kube-system` mudaram para suportar a configuração do mestre de alta disponibilidade. Especificamente, o valor `etcd_endpoints` agora suporta somente acesso no cluster. O uso desse valor para configurar a CLI do Calico para acesso de fora do cluster não funciona mais.

Em vez disso, use os dados armazenados no configmap `cluster-info` no namespace `kube-system`. Especificamente, use os valores `etcd_host` e `etcd_port` para configurar o terminal para a [CLI do Calico](/docs/containers?topic=containers-network_policies#cli_install) para acessar o mestre com a configuração de alta disponibilidade de fora do cluster.

<br>
**Atualizando as políticas de rede do Kubernetes ou do Calico**:</br>
{: #ha-networkpolicies}
Será necessário executar ações adicionais se você usar [políticas de rede do Kubernetes ou do Calico](/docs/containers?topic=containers-network_policies#network_policies) para controlar o acesso de egresso do pod ao cluster mestre e você estiver usando atualmente:
*  O IP do cluster de serviço do Kubernetes, que pode ser obtido executando `kubectl get service kubernetes -o yaml | grep clusterIP`.
*  O nome do domínio de serviço do kubernetes, que por padrão é: `https://kubernetes.default.svc.cluster.local`.
*  O IP do cluster mestre, que pode ser obtido executando `kubectl cluster-info | grep Kubernetes`.

As etapas a seguir descrevem como atualizar as políticas de rede do Kubernetes. Para atualizar políticas de rede do Calico, repita estas etapas com algumas mudanças de sintaxe de política menores e `calicoctl` para procurar se há impactos nas políticas.
{: note}

Antes de iniciar: [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Obtenha seu endereço IP do cluster mestre.
    ```
    kubectl cluster-info | grep Kubernetes
    ```
    {: pre}

2.  Procure se há impactos em suas políticas de rede do Kubernetes. Se nenhum YAML for retornado, o cluster não será afetado e não será necessário fazer mudanças adicionais.
    ```
    kubectl get networkpolicies --all-namespaces -o yaml | grep <cluster-master-ip>
    ```
    {: pre}

3.  Revise o YAML. Por exemplo, caso o seu cluster use a política de rede do Kubernetes a seguir para permitir que pods no namespace `default` acessem o cluster mestre por meio do IP do cluster de serviço do `kubernetes` ou do IP do cluster mestre, deve-se atualizar a política.
    ```
    apiVersion: networking.k8s.io/v1 kind: NetworkPolicy metadata: name: all-master-egress namespace: default spec: egress: # Allow access to cluster master using kubernetes service cluster IP address # or domain name or cluster master IP address.
      - ports:
        - protocol: TCP to:
        - ipBlock: cidr: 161.202.126.210/32 # Allow access to Kubernetes DNS in order to resolve the kubernetes service # domain name.
      - ports:
        - protocol: TCP port: 53
        - protocol: UDP port: 53 podSelector: {} policyTypes:
      - Egress
    ```
    {: screen}

4.  Revise a política de rede do Kubernetes para permitir o egresso para o endereço IP do proxy mestre no cluster `172.20.0.1`. Por enquanto, mantenha o endereço IP do cluster mestre. Por exemplo, o exemplo de política de rede anterior muda para o seguinte.

    Se anteriormente você configurou suas políticas de egresso para abrir apenas o endereço IP e a porta únicos para o mestre único do Kubernetes, agora use o intervalo de endereço IP do proxy mestre no cluster 172.20.0.1/32 e a porta 2040.
    {: tip}

    ```
    apiVersion: networking.k8s.io/v1 kind: NetworkPolicy metadata: name: all-master-egress namespace: default spec: egress: # Allow access to cluster master using kubernetes service cluster IP address # or domain name.
      - ports:
        - protocol: TCP to:
        - ipBlock: cidr: 172.20.0.1/32
        - ipBlock: cidr: 161.202.126.210/32 # Allow access to Kubernetes DNS in order to resolve the kubernetes service domain name.
      - ports:
        - protocol: TCP port: 53
        - protocol: UDP port: 53 podSelector: {} policyTypes:
      - Egress
    ```
    {: screen}

5.  Aplique a política de rede revisada ao cluster.
    ```
    kubectl apply -f all-master-egress.yaml
    ```
    {: pre}

6.  Depois de concluir todas as [ações de preparação](#ha-masters) (incluindo estas etapas), [atualize seu cluster mestre](/docs/containers?topic=containers-update#master) para o fix pack do mestre de alta disponibilidade.

7.  Depois que a atualização for concluída, remova o endereço IP do cluster mestre da política de rede. Por exemplo, na política de rede anterior, remova as linhas a seguir e, em seguida, reaplique a política.

    ```
    - ipBlock: cidr: 161.202.126.210/32
    ```
    {: screen}

    ```
    kubectl apply -f all-master-egress.yaml
    ```
    {: pre}

### Atualizando para `containerd` como o tempo de execução do contêiner
{: #containerd}

Para clusters que executam o Kubernetes versão 1.11 ou mais recente, o `containerd` substitui o Docker como o novo tempo de execução do contêiner para Kubernetes para aprimorar o desempenho. Se os seus pods dependem do Docker como o tempo de execução do contêiner do Kubernetes, deve-se atualizá-los para manipular o `containerd` como o tempo de execução do contêiner. Para obter mais informações, consulte o [comunicado do containerd do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/blog/2018/05/24/kubernetes-containerd-integration-goes-ga/).
{: shortdesc}

**Como sei se meus apps dependem do `docker` em vez do `containerd`?**<br>
Exemplos de vezes que você pode depender do Docker como o tempo de execução do contêiner:
*  Se você acessa o mecanismo do Docker ou a API diretamente usando contêineres privilegiados, atualize seus pods para suportar o `containerd` como o tempo de execução. Por exemplo, você pode chamar o soquete do Docker diretamente para ativar contêineres ou executar outras operações do Docker. O soquete do Docker mudou de `/var/run/docker.sock` para `/run/containerd/containerd.sock`. O protocolo que é usado no soquete `containerd` é um pouco diferente do que está no Docker. Tente atualizar seu app para o soquete `containerd`. Se você desejar continuar usando o soquete do Docker, procure usar o [Docker-inside-Docker (DinD) ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://hub.docker.com/_/docker/).
*  Alguns complementos de terceiros, como ferramentas de criação de log e de monitoramento, que você instala em seu cluster podem depender do mecanismo de Docker. Verifique com seu provedor para certificar-se de que as ferramentas sejam compatíveis com o containerd. Os casos de uso possíveis incluem:
   - Sua ferramenta de criação de log pode usar o diretório `/var/log/pods/<pod_uuid>/<container_name>/*.log` do contêiner `stderr/stdout` para acessar os logs. No Docker, esse diretório é um link simbólico para `/var/data/cripersistentstorage/containers/<container_uuid>/<container_uuid>-json.log`, enquanto no `containerd`, você acessa o diretório diretamente sem um link simbólico.
   - Sua ferramenta de monitoramento acessa o soquete do Docker diretamente. O soquete do Docker mudou de `/var/run/docker.sock` para `/run/containerd/containerd.sock`.

<br>

**Além da confiança no tempo de execução, eu preciso executar outras ações de preparação?**<br>

**Ferramenta manifest**: se você tiver imagens de multiplataforma que forem construídas com a ferramenta `docker manifest` [experimental ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.docker.com/edge/engine/reference/commandline/manifest/) antes do Docker versão 18.06, não será possível puxar a imagem do DockerHub usando `containerd`.

Ao verificar os eventos de pod, você pode ver um erro como o seguinte.
```
validação de tamanho com falha
```
{: screen}

Para usar uma imagem que é construída usando a ferramenta manifest com o `containerd`, escolha dentre as opções a seguir.

*  Reconstrua a imagem com a [ferramenta manifest ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/estesp/manifest-tool).
*  Reconstrua a imagem com a ferramenta `docker-manifest` depois de atualizar para o Docker versão 18.06 ou mais recente.

<br>

** O que não é afetado? Eu preciso mudar como implementar meus contêineres?**<br>
Em geral, os processos de implementação de contêiner não mudam. Ainda é possível usar um Dockerfile para definir uma imagem do Docker e construir um contêiner do Docker para seus apps. Se você usar os comandos `docker` para construir e enviar por push imagens para um registro, será possível continuar a usar o `docker` ou usar os comandos `ibmcloud cr` em seu lugar.

### Preparando para atualizar para o Calico v3
{: #111_calicov3}

Se você estiver atualizando um cluster do Kubernetes versão 1.9 ou anterior para a versão 1.11, prepare-se para a atualização do Calico v3 antes de atualizar o mestre. Durante o upgrade principal para o Kubernetes v1.11, novos pods e novas políticas de rede do Kubernetes ou Calico não são planejados. A quantia de tempo que a atualização evita novo planejamento varia. Clusters pequenos podem levar alguns minutos, com alguns minutos extras para cada 10 nós. As políticas de rede e os pods existentes continuam a ser executados.
{: shortdesc}

Se você estiver atualizando um cluster do Kubernetes versão 1.10 para a versão 1.11, ignore estas etapas porque você as concluiu quando atualizou para a 1.10.
{: note}

Antes de iniciar, seu cluster mestre e todos os nós do trabalhador devem estar executando o Kubernetes versão 1.8 ou 1.9 e devem ter pelo menos um nó do trabalhador.

1.  Verifique se os seus pods do Calico são funcionais.
    ```
    kubectl get pods -n kube-system -l k8s-app=calico-node -o wide
    ```
    {: pre}

2.  Se nenhum pod estiver em um estado **Executando**, exclua o pod e aguarde até que ele esteja em um estado **Executando** antes de continuar. Se o pod não retornar para um estado de **Execução**:
    1.  Verifique o **Estado** e o **Status** do nó do trabalhador.
        ```
        ibmcloud ks workers --cluster <cluster_name_or_ID>
        ```
        {: pre}
    2.  Se o estado do nó do trabalhador não for **Normal**, siga as etapas de [Depurando nós do trabalhador](/docs/containers?topic=containers-cs_troubleshoot#debug_worker_nodes). Por exemplo, um estado **Crítico** ou **Desconhecido** é frequentemente resolvido pelo [ recarregamento do nó do trabalhador](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload).

3.  Se você gerar automaticamente políticas do Calico ou outros recursos do Calico, atualize seu conjunto de ferramentas de automação para gerar esses recursos com a [sintaxe do Calico v3 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/).

4.  Se você usar [strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) para conectividade VPN, o gráfico Helm do strongSwan 2.0.0 não funcionará com o Calico v3 ou o Kubernetes 1.11. [Atualize o strongSwan](/docs/containers?topic=containers-vpn#vpn_upgrade) para o gráfico Helm 2.1.0, que é compatível com versões anteriores do Calico 2.6 e Kubernetes 1.7, 1.8 e 1.9.

5.  [Atualize seu cluster mestre para o Kubernetes v1.11](/docs/containers?topic=containers-update#master).

<br />


## Archive
{: #k8s_version_archive}

Localize uma visão geral das versões do Kubernetes que não são suportadas no {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

### Versão 1.10 (não suportada)
{: #cs_v110}

A partir de 16 de maio de 2019, os clusters do {{site.data.keyword.containerlong_notm}} que executam o [Kubernetes versão 1.10](/docs/containers?topic=containers-changelog#changelog_archive) não são suportados. Os clusters da versão 1.10 não podem receber atualizações de segurança nem suporte, a menos que eles sejam atualizados para a próxima versão mais recente.
{: shortdesc}

[Revise o impacto potencial](/docs/containers?topic=containers-cs_versions#cs_versions) de cada atualização de versão do Kubernetes e, em seguida, [atualize seus clusters](/docs/containers?topic=containers-update#update) para [Kubernetes 1.12](#cs_v112) porque o Kubernetes 1.11 foi descontinuado.

### Versão 1.9 (Não Suportado)
{: #cs_v19}

A partir de 27 de dezembro de 2018, os clusters do {{site.data.keyword.containerlong_notm}} que executam o [Kubernetes versão 1.9](/docs/containers?topic=containers-changelog#changelog_archive) não são suportados. Os clusters da versão 1.9 não podem receber atualizações de segurança nem suporte, a menos que eles sejam atualizados para a próxima versão mais recente.
{: shortdesc}

[Revise o impacto potencial](/docs/containers?topic=containers-cs_versions#cs_versions) de cada atualização de versão do Kubernetes e, em seguida, [atualize seus clusters](/docs/containers?topic=containers-update#update), primeiro para o [Kubernetes 1.11 descontinuado](#cs_v111) e, em seguida, imediatamente para o [Kubernetes 1.12](#cs_v112).

### Versão 1.8 (Não Suportado)
{: #cs_v18}

A partir de 22 de setembro de 2018, os clusters do {{site.data.keyword.containerlong_notm}} que executam o [Kubernetes versão 1.8](/docs/containers?topic=containers-changelog#changelog_archive) não são suportados. Os clusters da versão 1.8 não podem receber atualizações de segurança nem suporte.
{: shortdesc}

Para continuar a execução de seus apps no {{site.data.keyword.containerlong_notm}}, [crie um novo cluster](/docs/containers?topic=containers-clusters#clusters) e [implemente seus apps](/docs/containers?topic=containers-app#app) no novo cluster.

### Versão 1.7 (Não Suportado)
{: #cs_v17}

Desde 21 de junho de 2018, os clusters do {{site.data.keyword.containerlong_notm}} que executam o [Kubernetes versão 1.7](/docs/containers?topic=containers-changelog#changelog_archive) são não suportados. Os clusters da versão 1.7 não podem receber atualizações de segurança nem suporte.
{: shortdesc}

Para continuar a execução de seus apps no {{site.data.keyword.containerlong_notm}}, [crie um novo cluster](/docs/containers?topic=containers-clusters#clusters) e [implemente seus apps](/docs/containers?topic=containers-app#app) no novo cluster.

### Versão 1.5 (Não suportada)
{: #cs_v1-5}

A partir de 4 de abril de 2018, os clusters do {{site.data.keyword.containerlong_notm}} que executam o [Kubernetes versão 1.5](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.5.md) são não suportados. Os clusters da versão 1.5 não podem receber atualizações de segurança ou suporte.
{: shortdesc}

Para continuar a execução de seus apps no {{site.data.keyword.containerlong_notm}}, [crie um novo cluster](/docs/containers?topic=containers-clusters#clusters) e [implemente seus apps](/docs/containers?topic=containers-app#app) no novo cluster.
