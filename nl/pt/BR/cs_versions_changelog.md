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


# Log de mudanças da versão
{: #changelog}

Visualize informações de mudanças de versão para atualizações principais, secundárias e de correção que estão disponíveis para os clusters do Kubernetes do {{site.data.keyword.containerlong}}. As mudanças incluem atualizações para o Kubernetes e componentes do Provedor do {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

A menos que seja indicado de outra forma nos logs de mudanças, a versão do provedor do {{site.data.keyword.containerlong_notm}} ativa as APIs e os recursos do Kubernetes que estão em beta. Os recursos alfa do Kubernetes, que estão sujeitos a mudança, estão desativados.

Para obter mais informações sobre versões principais, secundárias e de correção e ações de preparação entre versões secundárias, consulte [Versões do Kubernetes](/docs/containers?topic=containers-cs_versions).
{: tip}

Para obter informações sobre mudanças desde a versão anterior, veja os logs de mudanças a seguir.
-  [Log de mudanças](#114_changelog) da versão 1.14.
-  Versão 1.13  [ changelog ](#113_changelog).
-  [Log de mudanças](#112_changelog) da Versão 1.12.
-  **Descontinuado**: [log de mudanças](#111_changelog) da versão 1.11.
-  [ Archive ](#changelog_archive)  de changelogs para versões não suportadas.

Alguns logs de mudanças são para _fix packs do nó do trabalhador_ e se aplicam somente aos nós do trabalhador. Deve-se [aplicar essas correções](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) para assegurar a conformidade de segurança para seus nós do trabalhador. Esses fix packs do nó do trabalhador podem estar em uma versão mais alta do que o mestre porque alguns fix packs de construção são específicos para nós do trabalhador. Outros logs de mudanças são para _fix packs de mestre_ e se aplicam somente ao cluster mestre. Os fix packs de mestre podem não ser aplicados automaticamente. É possível escolher [aplicá-los manualmente](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update). Para obter mais informações sobre tipos de correção, veja [Tipos de atualização](/docs/containers?topic=containers-cs_versions#update_types).
{: note}

</br>

## Log de mudanças da versão 1.14
{: #114_changelog}

### Log de mudanças para 1.14.2_1521, liberado em 4 de junho de 2019
{: #1142_1521}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.14.2_1521.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.14.1_1519">
<caption>Mudanças desde a versão 1.14.1_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração de DNS do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Corrigido um erro que pode deixar os pods DNS e CoreDNS do Kubernetes em execução após as operações `create` ou `update` do cluster.</td>
</tr>
<tr>
<td>Configuração de alta disponibilidade do cluster mestre</td>
<td>N/A</td>
<td>N/A</td>
<td>Configuração atualizada para minimizar falhas de conectividade de rede intermitentes do mestre durante uma atualização do mestre.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Consulte as [notas sobre a liberação do etcd ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Imagem atualizada para [CVE-2018-10844 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) e [CVE-2019-5436 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.14.1-71</td>
<td>v1.14.2-100</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.14.2.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.14.1</td>
<td>v1.14.2</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.2).</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>Consulte as [Notas sobre a liberação do Kubernetes Metrics Server ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Imagem atualizada para [CVE-2018-10844 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) e [CVE-2019-5436 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.14.1_1519, liberado em 20 de maio de 2019
{: #1141_1519}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.14.1_1519.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.14.1_1518">
<caption>Mudanças desde a versão 1.14.1_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel do Ubuntu 16.04</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Imagens do nó do trabalhador atualizadas com uma atualização do kernel para [CVE-2018-12126 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Kernel do Ubuntu 18.04</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Imagens do nó do trabalhador atualizadas com uma atualização do kernel para [CVE-2018-12126 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.14.1_1518, liberado em 13 de maio de 2019
{: #1141_1518}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.14.1_1518.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.14.1_1516">
<caption>Mudanças desde a versão 1.14.1_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy de alta disponibilidade do cluster mestre</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Consulte as [notas sobre a liberação do HAProxy ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). A atualização resolve [CVE-2019-6706 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>A configuração de política de auditoria do servidor da API do Kubernetes é atualizada para não registrar a URL somente leitura `/openapi/v2*`. Além disso, a configuração do gerenciador do controlador do Kubernetes aumentou a duração da validade de certificados `kubelet` assinados de 1 para 3 anos.</td>
</tr>
<tr>
<td>Configuração do cliente OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>O pod `vpn-*` do cliente OpenVPN no namespace `kube-system` agora configura `dnsPolicy` como `Default` para evitar que o pod falhe quando o DNS do cluster estiver inativo.</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>e7182c7</td>
<td>13c7ef0</td>
<td>Atualizada a imagem para [CVE-2016-7076 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076) e [CVE-2017-1000368 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.14.1_1516, liberado em 7 de maio de 2019
{: #1141_1516}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.14.1_1516.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.13.5_1519">
<caption>Mudanças desde a versão 1.13.5_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.4.4</td>
<td>v3.6.1</td>
<td>Consulte as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.6/release-notes/).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.6</td>
<td>1.3.1</td>
<td>Consulte as [notas sobre a liberação do CoreDNS ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://coredns.io/2019/01/13/coredns-1.3.1-release/). A atualização inclui a adição de uma [porta de métricas ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://coredns.io/plugins/metrics/) no serviço DNS do cluster. <br><br>O CoreDNS é agora o único provedor DNS do cluster suportado. Se você atualizar um cluster para o Kubernetes versão 1.14 de uma versão anterior e usar o KubeDNS, KubeDNS será migrado automaticamente para o CoreDNS durante a atualização do cluster. Para obter mais informações ou para testar o CoreDNS antes de atualizar, consulte [Configurar o provedor DNS do cluster](https://cloud.ibm.com/docs/containers?topic=containers-cluster_dns#cluster_dns).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>9ff3fda</td>
<td>ed0dafc</td>
<td>Atualizada a imagem para [CVE-2019-1543 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.13.5-107</td>
<td>v1.14.1-71</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.14.1. Além disso, a versão `calicoctl` é atualizada para 3.6.1. Atualizações corrigidas para balanceadores de carga da versão 2.0 com somente um nó do trabalhador disponível para os pods do balanceador de carga. Os balanceadores de carga privados suportam agora a execução em [nós de trabalhadores de borda privados](/docs/containers?topic=containers-edge#edge).</td>
</tr>
<tr>
<td>Políticas de segurança de pod da IBM</td>
<td>N/A</td>
<td>N/A</td>
<td>As [políticas de segurança de pod da IBM](/docs/containers?topic=containers-psp#ibm_psp) são atualizadas para suportar o recurso [RunAsGroup ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#users-and-groups) do Kubernetes.</td>
</tr>
<tr>
<td>Configuração do `kubelet`</td>
<td>N/A</td>
<td>N/A</td>
<td>Configure a opção `--pod-max-pids` como `14336` para evitar que um único pod consuma todos os IDs do processo em um nó do trabalhador.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.14.1</td>
<td>Consulte as [notas sobre a liberação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.1) e o [blog do Kubernetes 1.14 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/blog/2019/03/25/kubernetes-1-14-release-announcement/).<br><br>As políticas padrão de controle de acesso baseado na função (RBAC) do Kubernetes não concedem mais acesso às [APIs de descoberta e de verificação de permissão para usuários não autenticados ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles). Essa mudança se aplica apenas aos novos clusters da versão 1.14. Se você atualizar um cluster de uma versão anterior, os usuários não autenticados ainda terão acesso às APIs de descoberta e de verificação de permissão.</td>
</tr>
<tr>
<td>Configuração de controladores de admissão do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td><ul>
<li>Incluído `NodeRestriction` na opção `--enable-admission-plugins` para o servidor da API do Kubernetes do cluster e configurado os recursos de cluster relacionados para suportar esse aprimoramento de segurança.</li>
<li>Removido `Initializers` da opção `--enable-admission-plugins` e `admissionregistration.k8s.io/v1alpha1=true` da opção `--runtime-config` para o servidor da API do Kubernetes do cluster porque essas APIs não são mais suportadas. Em vez disso, é possível usar [webhooks de admissão do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/).</li></ul></td>
</tr>
<tr>
<td>Autoscaler de DNS do Kubernetes</td>
<td>1.3.0</td>
<td>1.4.0</td>
<td>Veja as [notas sobre a liberação do escalador automático de DNS do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.4.0).</td>
</tr>
<tr>
<td>Configuração de portas do recurso Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td><ul>
  <li>Incluído `RuntimeClass=false` para desativar a seleção da configuração de tempo de execução do contêiner.</li>
  <li>Removido `ExperimentalCriticalPodAnnotation=true` porque a anotação de pod `scheduler.alpha.kubernetes.io/critical-pod` não é mais suportada. Em vez disso, é possível usar a [prioridade do pod do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/docs/containers?topic=containers-pod_priority#pod_priority).</li></ul></td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>e132aa4</td>
<td>e7182c7</td>
<td>Atualizada a imagem para [CVE-2019-11068 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

<br />


## Versão 1.13 changelog
{: #113_changelog}

### Log de mudanças para 1.13.6_1524, liberado em 4 de junho de 2019
{: #1136_1524}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.13.6_1524.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.13.6_1522">
<caption>Mudanças desde a versão 1.13.6_1522</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração de DNS do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Corrigido um erro que pode deixar os pods DNS e CoreDNS do Kubernetes em execução após as operações `create` ou `update` do cluster.</td>
</tr>
<tr>
<td>Configuração de alta disponibilidade do cluster mestre</td>
<td>N/A</td>
<td>N/A</td>
<td>Configuração atualizada para minimizar falhas de conectividade de rede intermitentes do mestre durante uma atualização do mestre.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Consulte as [notas sobre a liberação do etcd ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Imagem atualizada para [CVE-2018-10844 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) e [CVE-2019-5436 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>Consulte as [Notas sobre a liberação do Kubernetes Metrics Server ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Imagem atualizada para [CVE-2018-10844 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) e [CVE-2019-5436 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.13.6_1522, liberado em 20 de maio de 2019
{: #1136_1522}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.13.6_1522.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.13.6_1521">
<caption>Mudanças desde a versão 1.13.6_1521</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel do Ubuntu 16.04</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Imagens do nó do trabalhador atualizadas com uma atualização do kernel para [CVE-2018-12126 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Kernel do Ubuntu 18.04</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Imagens do nó do trabalhador atualizadas com uma atualização do kernel para [CVE-2018-12126 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.13.6_1521, liberado em 13 de maio de 2019
{: #1136_1521}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.13.6_1521.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.13.5_1519">
<caption>Mudanças desde a versão 1.13.5_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy de alta disponibilidade do cluster mestre</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Consulte as [notas sobre a liberação do HAProxy ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). A atualização resolve [CVE-2019-6706 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Atualizada a imagem para [CVE-2019-1543 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.13.5-107</td>
<td>v1.13.6-139</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.13.6. Além disso, corrija o processo de atualização do balanceador de carga versão 2.0 que tem apenas um nó do trabalhador disponível para os pods do balanceador de carga.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.13.6</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.6).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>A configuração de política de auditoria do servidor da API do Kubernetes é atualizada para não registrar a URL somente leitura `/openapi/v2*`. Além disso, a configuração do gerenciador do controlador do Kubernetes aumentou a duração da validade de certificados `kubelet` assinados de 1 para 3 anos.</td>
</tr>
<tr>
<td>Configuração do cliente OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>O pod `vpn-*` do cliente OpenVPN no namespace `kube-system` agora configura `dnsPolicy` como `Default` para evitar que o pod falhe quando o DNS do cluster estiver inativo.</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Imagem atualizada para [CVE-2016-7076 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) e [CVE-2019-11068 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.13.5_1519, liberado em 29 de abril de 2019
{: #1135_1519}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.13.5_1519.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.13.5_1518">
<caption>Mudanças desde a versão 1.13.5_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações para pacotes do Ubuntu instalados.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.5</td>
<td>1.2.6</td>
<td>Veja as [notas sobre a liberação de containerd![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.2.6).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack 1.13.5_1518 do nó do trabalhador, liberado em 15 de abril de 2019
{: #1135_1518}

A tabela a seguir mostra as mudanças incluídas no fix pack 1.13.5_1518 do nó do trabalhador.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.13.5_1517">
<caption>Mudanças feitas desde a versão 1.13.5_1517</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações em pacotes do Ubuntu instalados, incluindo `systemd` para [CVE-2019-3842 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Log de mudanças para a 1.13.5_1517, liberada em 8 de abril de 2019
{: #1135_1517}

A tabela a seguir mostra as mudanças incluídas na correção 1.13.5_1517.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.13.4_1516">
<caption>Mudanças feitas desde a versão 1.13.4_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.4.0</td>
<td>v3.4.4</td>
<td>Consulte as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.4/releases/#v344). A atualização resolve [CVE-2019-9946 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Proxy de alta disponibilidade do cluster mestre</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Consulte as [notas sobre a liberação do HAProxy ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). A atualização resolve [CVE-2018-0732 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) e [CVE-2019-1559 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.13.4-86</td>
<td>v1.13.5-107</td>
<td>Atualizado para suportar as liberações do Kubernetes 1.13.5 e do Calico 3.4.4.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.4</td>
<td>v1.13.5</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.5).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Imagem atualizada para [CVE-2017-12447 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Kernel do Ubuntu 16.04</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Imagens do nó do trabalhador atualizadas com atualização do kernel para [CVE-2019-9213 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Kernel do Ubuntu 18.04</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Imagens do nó do trabalhador atualizadas com atualização do kernel para [CVE-2019-9213 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack 1.13.4_1516 do nó do trabalhador, liberado em 1 de abril de 2019
{: #1134_1516}

A tabela a seguir mostra as mudanças incluídas no fix pack 1.13.4_1516 do nó do trabalhador.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.13.4_1515">
<caption>Mudanças feitas desde a versão 1.13.4_1515</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilização do recurso do nó do trabalhador</td>
<td>N/A</td>
<td>N/A</td>
<td>Reservas de memória aumentadas para o kubelet e o containerd para evitar que esses componentes sejam executados fora dos recursos. Para obter mais informações, consulte [Reservas de recursos do nó do trabalhador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack principal 1.13.4_1515, liberado em 26 de março de 2019
{: #1134_1515}

A tabela a seguir mostra as mudanças incluídas no fix pack principal 1.13.4_1515.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.13.4_1513">
<caption>Mudanças feitas desde a versão 1.13.4_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração de DNS do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>O processo de atualização do Kubernetes versão 1.11 foi corrigido para evitar que a atualização alterne o provedor DNS do cluster para o CoreDNS. Ainda é possível [configurar o CoreDNS como o provedor DNS do cluster](/docs/containers?topic=containers-cluster_dns#set_coredns) após a atualização.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>345</td>
<td>346</td>
<td>Imagem atualizada para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>166</td>
<td>167</td>
<td>Corrige os erros de `context deadline exceeded` e `timeout` intermitentes para gerenciar segredos do Kubernetes. Além disso, corrige as atualizações para o serviço de gerenciamento de chaves que pode deixar os segredos do Kubernetes existentes não criptografados. A atualização inclui correção para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Balanceador de carga e monitor de balanceador de carga para o {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Imagem atualizada para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.13.4_1513, liberado em 20 de março de 2019
{: #1134_1513}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.13.4_1513.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.13.4_1510">
<caption>Mudanças desde a versão 1.13.4_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração de DNS do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Corrigido um erro que pode fazer com que operações do cluster mestre, como `refresh` ou `update`, falhem quando deve ser feita a redução de escala do DNS do cluster não usado.</td>
</tr>
<tr>
<td>Configuração do proxy do mestre de alta disponibilidade</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizada a configuração para manipular melhor as falhas na conexão intermitente para o cluster mestre.</td>
</tr>
<tr>
<td>Configuração de CoreDNS</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizada a configuração do CoreDNS para suportar [múltiplos Corefiles ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://coredns.io/2017/07/23/corefile-explained/) após atualizar a versão do cluster Kubernetes de 1.12.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.4</td>
<td>1.2.5</td>
<td>Veja as [notas sobre a liberação de containerd![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.2.5). A atualização inclui a correção melhorada para [CVE-2019-5736 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Atualizados os drivers de GPU para [418.43 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.nvidia.com/object/unix.html). A atualização inclui correção para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>344</td>
<td>345</td>
<td>Incluído suporte para  [ terminais em serviço privado ](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Atualizadas as imagens do nó do trabalhador com a atualização do kernel para [CVE-2019-6133 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>136</td>
<td>166</td>
<td>Atualizada a imagem para [CVE-2018-16890 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) e [CVE-2019-3823 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Atualizada a imagem para [CVE-2018-10779 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) e [CVE-2019-7663 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.13.4_1510, liberado em 4 de março de 2019
{: #1134_1510}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.13.4_1510.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.13.2_1509">
<caption>Mudanças desde a versão 1.13.2_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provedor DNS do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Aumentado o limite de memória de pod do Kubernetes DNS e do CoreDNS de `170Mi` para `400Mi` para manipular mais serviços de cluster.</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Atualizadas as imagens para [CVE-2019-6454 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.13.2-62</td>
<td>v1.13.4-86</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.13.4. Corrigidos problemas de conectividade periódica para balanceadores de carga da versão 1.0 que configuram `externalTrafficPolicy` como `local`. Atualizados os eventos de balanceador de carga versão 1.0 e 2.0 para usar os links da documentação mais recente do {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>342</td>
<td>344</td>
<td>Mudado o sistema operacional de base para a imagem de Fedora para Alpine. Atualizada a imagem para [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>122.</td>
<td>136</td>
<td>Tempo limite do cliente aumentado para  {{site.data.keyword.keymanagementservicefull_notm}}. Atualizada a imagem para [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.2</td>
<td>v1.13.4</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.4). A atualização resolve [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) e [CVE-2019-1002100 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluído  ` ExperimentalCriticalPodAnnotation = true `  para a opção  ` -- feature-gates ` . Essa configuração ajuda a migrar os pods da anotação `scheduler.alpha.kubernetes.io/critical-pod` descontinuada para [Suporte de prioridade de pod do Kubernetes](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Balanceador de carga e monitor de balanceador de carga para o {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Atualizada a imagem para [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Cliente e servidor OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Atualizada a imagem para [CVE-2019-1559 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Atualizada a imagem para [CVE-2019-6454 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.13.2_1509, liberado em 27 de fevereiro de 2019
{: #1132_1509}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do trabalhador do nó 1.13.2_1509.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.13.2_1508">
<caption>Mudanças desde a versão 1.13.2_1508</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Atualizadas as imagens do nó do trabalhador com a atualização do kernel para [CVE-2018-19407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.13.2_1508, liberado em 15 de fevereiro de 2019
{: #1132_1508}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do trabalhador do nó 1.13.2_1508.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.13.2_1507">
<caption>Mudanças desde a versão 1.13.2_1507</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração do proxy do mestre de alta disponibilidade</td>
<td>N/A</td>
<td>N/A</td>
<td>Mudado o valor de configuração do pod `spec.priorityClassName` para `system-node-critical` e configurado o valor `spec.priority` para `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.2</td>
<td>1.2.4</td>
<td>Veja as [notas sobre a liberação de containerd![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.2.4). A atualização resolve [CVE-2019-5736 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configuração do Kubernetes  ` kubelet `</td>
<td>N/A</td>
<td>N/A</td>
<td>Ativada a porta do recurso `ExperimentalCriticalPodAnnotation` para evitar despejo crítico de pod estático. Configure a opção `event-qps` como `0` para evitar a criação de eventos de limite de taxa.</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.13.2_1507, liberado em 5 de fevereiro de 2019
{: #1132_1507}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.13.2_1507.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.12.4_1535">
<caption>Mudanças desde a versão 1.12.4_1535</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.4.0</td>
<td>Consulte as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.4/releases/#v340).</td>
</tr>
<tr>
<td>Provedor DNS do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Agora, CoreDNS é o provedor DNS de cluster padrão para novos clusters. Se você atualizar um cluster existente para 1.13 que usa KubeDNS como o provedor DNS do cluster, o KubeDNS continuará a ser o provedor DNS do cluster. No entanto, é possível escolher [usar o CoreDNS no lugar](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.2.2</td>
<td>Veja as [notas sobre a liberação de containerd![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.2.2).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.2</td>
<td>1.2.6</td>
<td>Consulte as [notas sobre a liberação do CoreDNS ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coredns/coredns/releases/tag/v1.2.6). Além disso, a configuração do CoreDNS é atualizada para [suportar diversos Corefiles ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://coredns.io/2017/07/23/corefile-explained/).</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Consulte as [notas sobre a liberação do etcd ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coreos/etcd/releases/v3.3.11). Além disso, os conjuntos de cifras suportados para etcd são agora restritos a um subconjunto com criptografia de alta segurança (128 bits ou mais).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Atualizadas as imagens para [CVE-2019-3462 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) e [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.4-118</td>
<td>v1.13.2-62</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.13.2. Além disso, a versão `calicoctl` é atualizada para 3.4.0. Corrigidas atualizações de configuração desnecessárias para balanceadores de carga da versão 2.0 em mudanças de status do nó do trabalhador.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>338</td>
<td>342</td>
<td>O plug-in de armazenamento de arquivo é atualizado conforme a seguir:
<ul><li>Suporta o fornecimento dinâmico com [planejamento com reconhecimento de topologia do volume](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignora a solicitação de volume persistente (PVC) para excluir erros se o armazenamento já tiver sido excluído.</li>
<li>Inclui uma anotação de mensagem de erro em PVCs com falha.</li>
<li>Otimiza as configurações de eleição de líder do controlador do fornecedor de armazenamento e de período de ressincronização e aumenta o tempo limite de fornecimento de 30 minutos para 1 hora.</li>
<li>Verifica as permissões de usuário antes de iniciar o fornecimento.</li>
<li>Inclui uma tolerância `CriticalAddonsOnly` com as implementações `ibm-file-plugin` e `ibm-storage-watcher` no namespace `kube-system`.</li></ul></td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>111</td>
<td>122.</td>
<td>Incluída a lógica de nova tentativa para evitar falhas temporárias quando os segredos do Kubernetes são gerenciados pelo {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.13.2</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.2).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>A configuração de política de auditoria do servidor de API do Kubernetes é atualizada para incluir o registro de metadados para solicitações `cluster-admin` e o registro do corpo da solicitação de solicitações `create`, `update` e `patch` da carga de trabalho.</td>
</tr>
<tr>
<td>Autoscaler de DNS do Kubernetes</td>
<td>1.2.0</td>
<td>1.3.0</td>
<td>Veja as [notas sobre a liberação do escalador automático de DNS do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.3.0).</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Atualizada a imagem para [CVE-2018-0734 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Incluída a tolerância `CriticalAddonsOnly` para a implementação `vpn` no namespace `kube-system`. Além disso, a configuração do pod é agora obtida de um segredo em vez de um configmap.</td>
</tr>
<tr>
<td>Servidor OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Atualizada a imagem para [CVE-2018-0734 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Correção de segurança para [CVE-2018-16864 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

<br />


## Log de mudanças da Versão 1.12
{: #112_changelog}

Revise o log de mudanças da versão 1.12.
{: shortdesc}

### Log de mudanças para 1.12.9_1555, liberado em 4 de junho de 2019
{: #1129_1555}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.12.9_1555.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.12.8_1553">
<caption>Mudanças desde a versão 1.12.8_1553</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração de DNS do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Corrigido um erro que pode deixar os pods DNS e CoreDNS do Kubernetes em execução após as operações `create` ou `update` do cluster.</td>
</tr>
<tr>
<td>Configuração de alta disponibilidade do cluster mestre</td>
<td>N/A</td>
<td>N/A</td>
<td>Configuração atualizada para minimizar falhas de conectividade de rede intermitentes do mestre durante uma atualização do mestre.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Consulte as [notas sobre a liberação do etcd ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Imagem atualizada para [CVE-2018-10844 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) e [CVE-2019-5436 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.8-210</td>
<td>v1.12.9-227</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.12.9.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.8</td>
<td>v1.12.9</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.9).</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>Consulte as [Notas sobre a liberação do Kubernetes Metrics Server ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Imagem atualizada para [CVE-2018-10844 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) e [CVE-2019-5436 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.12.8_1553, liberado em 20 de maio de 2019
{: #1128_1533}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.12.8_1553.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.12.8_1553">
<caption>Mudanças desde a versão 1.12.8_1553</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel do Ubuntu 16.04</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Imagens do nó do trabalhador atualizadas com uma atualização do kernel para [CVE-2018-12126 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Kernel do Ubuntu 18.04</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Imagens do nó do trabalhador atualizadas com uma atualização do kernel para [CVE-2018-12126 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.12.8_1552, liberado em 13 de maio de 2019
{: #1128_1552}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.12.8_1552.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.12.7_1550">
<caption>Mudanças desde a versão 1.12.7_1550</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy de alta disponibilidade do cluster mestre</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Consulte as [notas sobre a liberação do HAProxy ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). A atualização resolve [CVE-2019-6706 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Atualizada a imagem para [CVE-2019-1543 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.7-180</td>
<td>v1.12.8-210</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.12.8. Além disso, corrija o processo de atualização do balanceador de carga versão 2.0 que tem apenas um nó do trabalhador disponível para os pods do balanceador de carga.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.7</td>
<td>v1.12.8</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.8).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>A configuração de política de auditoria do servidor da API do Kubernetes é atualizada para não registrar a URL somente leitura `/openapi/v2*`. Além disso, a configuração do gerenciador do controlador do Kubernetes aumentou a duração da validade de certificados `kubelet` assinados de 1 para 3 anos.</td>
</tr>
<tr>
<td>Configuração do cliente OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>O pod `vpn-*` do cliente OpenVPN no namespace `kube-system` agora configura `dnsPolicy` como `Default` para evitar que o pod falhe quando o DNS do cluster estiver inativo.</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Imagem atualizada para [CVE-2016-7076 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) e [CVE-2019-11068 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.12.7_1550, liberado em 29 de abril de 2019
{: #1127_1550}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.12.7_1550.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.12.7_1549">
<caption>Mudanças desde a versão 1.12.7_1549</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações para pacotes do Ubuntu instalados.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>Veja as [notas sobre a liberação de containerd![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.1.7).</td>
</tr>
</tbody>
</table>


### Log de mudanças para o fix pack 1.12.7_1549 do nó do trabalhador, liberado em 15 de abril de 2019
{: #1127_1549}

A tabela a seguir mostra as mudanças incluídas no fix pack 1.12.7_1549 do nó do trabalhador.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.12.7_1548">
<caption>Mudanças feitas desde a versão 1.12.7_1548</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações em pacotes do Ubuntu instalados, incluindo `systemd` para [CVE-2019-3842 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Log de mudanças para a 1.12.7_1548, liberada em 8 de abril de 2019
{: #1127_1548}

A tabela a seguir mostra as mudanças incluídas na correção 1.12.7_1548.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.12.6_1547">
<caption>Mudanças feitas desde a versão 1.12.6_1547</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>Consulte as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.3/releases/#v336). A atualização resolve [CVE-2019-9946 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Proxy de alta disponibilidade do cluster mestre</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Consulte as [notas sobre a liberação do HAProxy ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). A atualização resolve [CVE-2018-0732 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) e [CVE-2019-1559 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.6-157</td>
<td>v1.12.7-180</td>
<td>Atualizado para suportar as liberações do Kubernetes 1.12.7 e do Calico 3.3.6.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.6</td>
<td>v1.12.7</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.7).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Imagem atualizada para [CVE-2017-12447 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Kernel do Ubuntu 16.04</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Imagens do nó do trabalhador atualizadas com atualização do kernel para [CVE-2019-9213 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Kernel do Ubuntu 18.04</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Imagens do nó do trabalhador atualizadas com atualização do kernel para [CVE-2019-9213 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack 1.12.6_1547 do nó do trabalhador, liberado em 1 de abril de 2019
{: #1126_1547}

A tabela a seguir mostra as mudanças incluídas no fix pack 1.12.6_1547 do nó do trabalhador.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.12.6_1546">
<caption>Mudanças feitas desde a versão 1.12.6_1546</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilização do recurso do nó do trabalhador</td>
<td>N/A</td>
<td>N/A</td>
<td>Reservas de memória aumentadas para o kubelet e o containerd para evitar que esses componentes sejam executados fora dos recursos. Para obter mais informações, consulte [Reservas de recursos do nó do trabalhador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>


### Log de mudanças para o fix pack principal 1.12.6_1546, liberado em 26 de março de 2019
{: #1126_1546}

A tabela a seguir mostra as mudanças incluídas no fix pack principal 1.12.6_1546.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.12.6_1544">
<caption>Mudanças feitas desde a versão 1.12.6_1544</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>345</td>
<td>346</td>
<td>Imagem atualizada para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>166</td>
<td>167</td>
<td>Corrige os erros de `context deadline exceeded` e `timeout` intermitentes para gerenciar segredos do Kubernetes. Além disso, corrige as atualizações para o serviço de gerenciamento de chaves que pode deixar os segredos do Kubernetes existentes não criptografados. A atualização inclui correção para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Balanceador de carga e monitor de balanceador de carga para o {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Imagem atualizada para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.12.6_1544, liberado em 20 de março de 2019
{: #1126_1544}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.12.6_1544.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.12.6_1541">
<caption>Mudanças desde a versão 1.12.6_1541</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração de DNS do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Corrigido um erro que pode fazer com que operações do cluster mestre, como `refresh` ou `update`, falhem quando deve ser feita a redução de escala do DNS do cluster não usado.</td>
</tr>
<tr>
<td>Configuração do proxy do mestre de alta disponibilidade</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizada a configuração para manipular melhor as falhas na conexão intermitente para o cluster mestre.</td>
</tr>
<tr>
<td>Configuração de CoreDNS</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizada a configuração do CoreDNS para suportar [múltiplos Corefiles ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://coredns.io/2017/07/23/corefile-explained/).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Atualizados os drivers de GPU para [418.43 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.nvidia.com/object/unix.html). A atualização inclui correção para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>344</td>
<td>345</td>
<td>Incluído suporte para  [ terminais em serviço privado ](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Atualizadas as imagens do nó do trabalhador com a atualização do kernel para [CVE-2019-6133 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>136</td>
<td>166</td>
<td>Atualizada a imagem para [CVE-2018-16890 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) e [CVE-2019-3823 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Atualizada a imagem para [CVE-2018-10779 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) e [CVE-2019-7663 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.12.6_1541, liberado em 4 de março de 2019
{: #1126_1541}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.12.6_1541.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.12.5_1540">
<caption>Mudanças desde a versão 1.12.5_1540</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provedor DNS do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Aumentado o limite de memória de pod do Kubernetes DNS e do CoreDNS de `170Mi` para `400Mi` para manipular mais serviços de cluster.</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Atualizadas as imagens para [CVE-2019-6454 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.5-137</td>
<td>v1.12.6-157</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.12.6. Corrigidos problemas de conectividade periódica para balanceadores de carga da versão 1.0 que configuram `externalTrafficPolicy` como `local`. Atualizados os eventos de balanceador de carga versão 1.0 e 2.0 para usar os links da documentação mais recente do {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>342</td>
<td>344</td>
<td>Mudado o sistema operacional de base para a imagem de Fedora para Alpine. Atualizada a imagem para [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>122.</td>
<td>136</td>
<td>Tempo limite do cliente aumentado para  {{site.data.keyword.keymanagementservicefull_notm}}. Atualizada a imagem para [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.5</td>
<td>v1.12.6</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.6). A atualização resolve [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) e [CVE-2019-1002100 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluído  ` ExperimentalCriticalPodAnnotation = true `  para a opção  ` -- feature-gates ` . Essa configuração ajuda a migrar os pods da anotação `scheduler.alpha.kubernetes.io/critical-pod` descontinuada para [Suporte de prioridade de pod do Kubernetes](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Balanceador de carga e monitor de balanceador de carga para o {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Atualizada a imagem para [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Cliente e servidor OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Atualizada a imagem para [CVE-2019-1559 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Atualizada a imagem para [CVE-2019-6454 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.12.5_1540, liberado em 27 de fevereiro de 2019
{: #1125_1540}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.12.5_1540.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.12.5_1540">
<caption>Mudanças desde a versão 1.12.5_1538</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Atualizadas as imagens do nó do trabalhador com a atualização do kernel para [CVE-2018-19407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.12.5_1538, liberado em 15 de fevereiro de 2019
{: #1125_1538}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.12.5_1538.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.12.5_1537">
<caption>Mudanças desde a versão 1.12.5_1537</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração do proxy do mestre de alta disponibilidade</td>
<td>N/A</td>
<td>N/A</td>
<td>Mudado o valor de configuração do pod `spec.priorityClassName` para `system-node-critical` e configurado o valor `spec.priority` para `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>Veja as [notas sobre a liberação de containerd![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.1.6). A atualização resolve [CVE-2019-5736 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configuração do Kubernetes  ` kubelet `</td>
<td>N/A</td>
<td>N/A</td>
<td>Ativada a porta do recurso `ExperimentalCriticalPodAnnotation` para evitar despejo crítico de pod estático.</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.12.5_1537, liberado em 5 de fevereiro de 2019
{: #1125_1537}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.12.5_1537.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.12.4_1535">
<caption>Mudanças desde a versão 1.12.4_1535</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Consulte as [notas sobre a liberação do etcd ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coreos/etcd/releases/v3.3.11). Além disso, os conjuntos de cifras suportados para etcd são agora restritos a um subconjunto com criptografia de alta segurança (128 bits ou mais).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Atualizadas as imagens para [CVE-2019-3462 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) e [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.4-118</td>
<td>v1.12.5-137</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.12.5. Além disso, a versão `calicoctl` é atualizada para 3.3.1. Corrigidas atualizações de configuração desnecessárias para balanceadores de carga da versão 2.0 em mudanças de status do nó do trabalhador.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>338</td>
<td>342</td>
<td>O plug-in de armazenamento de arquivo é atualizado conforme a seguir:
<ul><li>Suporta o fornecimento dinâmico com [planejamento com reconhecimento de topologia do volume](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignora a solicitação de volume persistente (PVC) para excluir erros se o armazenamento já tiver sido excluído.</li>
<li>Inclui uma anotação de mensagem de erro em PVCs com falha.</li>
<li>Otimiza as configurações de eleição de líder do controlador do fornecedor de armazenamento e de período de ressincronização e aumenta o tempo limite de fornecimento de 30 minutos para 1 hora.</li>
<li>Verifica as permissões de usuário antes de iniciar o fornecimento.</li></ul></td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>111</td>
<td>122.</td>
<td>Incluída a lógica de nova tentativa para evitar falhas temporárias quando os segredos do Kubernetes são gerenciados pelo {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.12.5</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.5).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>A configuração de política de auditoria do servidor de API do Kubernetes é atualizada para incluir o registro de metadados para solicitações `cluster-admin` e o registro do corpo da solicitação de solicitações `create`, `update` e `patch` da carga de trabalho.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Atualizada a imagem para [CVE-2018-0734 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Além disso, a configuração do pod é agora obtida de um segredo em vez de um configmap.</td>
</tr>
<tr>
<td>Servidor OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Atualizada a imagem para [CVE-2018-0734 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Correção de segurança para [CVE-2018-16864 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.12.4_1535, liberado em 28 de janeiro de 2019
{: #1124_1535}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.12.4_1535.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.12.5_1537">
<caption>Mudanças desde a versão 1.12.4_1534</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualiza para os pacotes Ubuntu instalados, incluindo `apt` para [CVE-2019-3462 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) e [USN-3863-1 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>


### Log de mudanças para 1.12.4_1534, liberado em 21 de janeiro de 2019
{: #1124_1534}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.12.3_1534.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.12.3_1533">
<caption>Mudanças desde a versão 1.12.3_1533</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.3-91</td>
<td>v1.12.4-118</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.12.4.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.3</td>
<td>v1.12.4</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.4).</td>
</tr>
<tr>
<td>Resgador de complemento do Kubernetes</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Consulte as [Notas sobre a liberação do redimensionador de complemento do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Painel do Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>Consulte as [Notas sobre a liberação do painel do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). A atualização resolve [CVE-2018-18264 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).</td>
</tr>
<tr>
<td>Instalador do GPU</td>
<td>390.12</td>
<td>410,79</td>
<td>Atualizados os drivers de GPU instalados para 410.79.</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.12.3_1533, liberado em 7 de janeiro de 2019
{: #1123_1533}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.12.3_1533.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.12.3_1533">
<caption>Mudanças desde a versão 1.12.3_1532</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Atualizadas as imagens do nó do trabalhador com a atualização do kernel para [CVE-2017-5753, CVE-2018-18690 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

### Changelog para o nó do trabalhador fix pack 1.12.3_1532, liberado em 17 de dezembro de 2018
{: #1123_1532}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.12.2_1532.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.12.3_1531">
<caption>Mudanças desde a versão 1.12.3_1531</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações para pacotes do Ubuntu instalados.</td>
</tr>
</tbody>
</table>


### Log de mudanças para 1.12.3_1531, liberado em 5 de dezembro de 2018
{: #1123_1531}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.12.3_1531.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.12.2_1530">
<caption>Mudanças desde a versão 1.12.2_1530</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.12.2-68</td>
<td>v1.12.3-91</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.12.3.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.2</td>
<td>v1.12.3</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.3). A atualização resolve o [CVE-2018-1002105 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.12.2_1530, liberado em 4 de dezembro de 2018
{: #1122_1530}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.12.2_1530.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.12.2_1529">
<caption>Mudanças desde a versão 1.12.2_1529</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilização do recurso do nó do trabalhador</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluídos cgroups dedicados para o kubelet e o containerd para evitar que esses componentes sejam executados sem recursos. Para obter mais informações, consulte [Reservas de recursos do nó do trabalhador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>



### Log de mudanças para 1.12.2_1529, liberado em 27 de novembro de 2018
{: #1122_1529}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.12.2_1529.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.12.2_1528">
<caption>Mudanças desde a versão 1.12.2_1528</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>Consulte as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.3/releases/#v331). A atualização resolve o [Tigera Technical Advisory TTA-2018-001 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Configuração de DNS do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Foi corrigido um erro que pode resultar em pods DNS e CoreDNS do Kubernetes para execução após as operações de criação ou atualização de cluster.</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.2.0</td>
<td>v1.1.5</td>
<td>Veja as [notas sobre a liberação de containerd![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.1.5). Containerd atualizado para corrigir um conflito que pode [parar a finalização dos pods ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/issues/2744).</td>
</tr>
<tr>
<td>Cliente e servidor OpenVPN</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Imagem atualizada para o [CVE-2018-0732 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) e o [CVE-2018-0737 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.12.2_1528, liberado em 19 de novembro de 2018
{: #1122_1528}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.12.2_1528.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.12.2_1527">
<caption>Mudanças desde a versão 1.12.2_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Atualizadas as imagens do nó do trabalhador com atualização do kernel para o [CVE-2018-7755 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>


### Log de mudanças para 1.12.2_1527, liberado em 7 de novembro de 2018
{: #1122_1527}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.12.2_1527.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.3_1533">
<caption>Mudanças desde a versão 1.11.3_1533</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração do Calico</td>
<td>N/A</td>
<td>N/A</td>
<td>Os pods do Calico `calico-*` no namespace `kube-system` agora configuram solicitações de recurso de CPU e de memória para todos os contêineres.</td>
</tr>
<tr>
<td>Provedor DNS do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>O DNS do Kubernetes (KubeDNS) permanece o provedor DNS padrão do cluster. No entanto, você agora tem a opção de [mudar o provedor DNS do cluster para CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>Provedor de métricas do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>O Kubernetes Metrics Server substitui o Kubernetes Heapster (descontinuado desde o Kubernetes versão 1.8) como o provedor de métricas do cluster. Para itens de ação, consulte [a ação de preparação `metrics-server`](/docs/containers?topic=containers-cs_versions#metrics-server).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.4</td>
<td>1.2.0</td>
<td>Veja as [notas sobre a liberação de containerd![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.2.0).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>N/A</td>
<td>1.2.2</td>
<td>Consulte as [notas sobre a liberação do CoreDNS ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coredns/coredns/releases/tag/v1.2.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.12.2</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.2).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluídas três novas políticas de segurança do pod da IBM e suas funções de cluster associadas. Para obter mais informações, consulte [Entendendo os recursos padrão para o gerenciamento de cluster da IBM](/docs/containers?topic=containers-psp#ibm_psp).</td>
</tr>
<tr>
<td>Painel do Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.0</td>
<td>Veja as [Notas sobre a liberação do Painel do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.0).<br><br>
Se você acessar o painel por meio de `kubectl proxy`, o botão **SKIP** na página de login será removido. Em vez disso, [use um **Token** para efetuar login](/docs/containers?topic=containers-app#cli_dashboard). Além disso, agora é possível aumentar a capacidade do número de pods do Painel do Kubernetes executando `kubectl -n kube-system scale deploy kubernetes-dashboard --replicas=3`.</td>
</tr>
<tr>
<td>DNS do Kubernetes</td>
<td>14.1.2010</td>
<td>1.14.13</td>
<td>Veja as [Notas sobre a liberação do DNS do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>N/A</td>
<td>v0.3.1</td>
<td>Consulte as [Notas sobre a liberação do Kubernetes Metrics Server ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.1).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.3-118</td>
<td>v1.12.2-68</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.12. As mudanças adicionais incluem o seguinte:
<ul><li>Os pods do balanceador de carga (`ibm-cloud-provider-ip-*` no namespace `ibm-system`) agora configuram as solicitações de recurso de CPU e de memória.</li>
<li>A anotação `service.kubernetes.io/ibm-load-balancecer-cloud-provider-vlan` é incluída para especificar a VLAN na qual o serviço de balanceador de carga é implementado. Para ver as VLANs disponíveis em seu cluster, execute `ibmcloud ks vlans --zone <zone>`.</li>
<li>Um novo [balanceador de carga 2.0](/docs/containers?topic=containers-loadbalancer#planning_ipvs) está disponível como um beta.</li></ul></td>
</tr>
<tr>
<td>Configuração do cliente OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>O cliente OpenVPN `vpn-* pod` no namespace `kube-system` agora configura solicitações de recurso de CPU e de memória.</td>
</tr>
</tbody>
</table>

## Descontinuado: log de mudanças da versão 1.11
{: #111_changelog}

Revise o log de mudanças da versão 1.11.
{: shortdesc}

O Kubernetes versão 1.11 foi descontinuado e torna-se não suportado em 27 de junho de 2019 (tentativa). [Revise o impacto potencial](/docs/containers?topic=containers-cs_versions#cs_versions) de cada atualização de versão do Kubernetes e, em seguida, [atualize seus clusters](/docs/containers?topic=containers-update#update) imediatamente para pelo menos 1.12.
{: deprecated}

### Log de mudanças para 1.11.10_1561, liberado em 4 de junho de 2019
{: #11110_1561}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.11.10_1561.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.10_1559">
<caption>Mudanças desde a versão 1.11.10_1559</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração de alta disponibilidade do cluster mestre</td>
<td>N/A</td>
<td>N/A</td>
<td>Configuração atualizada para minimizar falhas de conectividade de rede intermitentes do mestre durante uma atualização do mestre.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>Consulte as [notas sobre a liberação do etcd ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Imagem atualizada para [CVE-2018-10844 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) e [CVE-2019-5436 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Imagem atualizada para [CVE-2018-10844 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) e [CVE-2019-5436 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.11.10_1559, liberado em 20 de maio de 2019
{: #11110_1559}

A tabela a seguir mostra as mudanças que estão incluídas no pacote de correção 1.11.10_1559.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.10_1558">
<caption>Mudanças desde a versão 1.11.10_1558</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel do Ubuntu 16.04</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Imagens do nó do trabalhador atualizadas com uma atualização do kernel para [CVE-2018-12126 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Kernel do Ubuntu 18.04</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Imagens do nó do trabalhador atualizadas com uma atualização do kernel para [CVE-2018-12126 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html) e [CVE-2018-12130 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.11.10_1558, liberado em 13 de maio de 2019
{: #11110_1558}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.11.10_1558.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.9_1556">
<caption>Mudanças desde a versão 1.11.9_1556</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy de alta disponibilidade do cluster mestre</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Consulte as [notas sobre a liberação do HAProxy ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). A atualização resolve [CVE-2019-6706 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Atualizada a imagem para [CVE-2019-1543 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.9-241</td>
<td>v1.11.10-270</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.11.10.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.9</td>
<td>v1.11.10</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.10).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>A configuração de política de auditoria do servidor da API do Kubernetes é atualizada para não registrar a URL somente leitura `/openapi/v2*`. Além disso, a configuração do gerenciador do controlador do Kubernetes aumentou a duração da validade de certificados `kubelet` assinados de 1 para 3 anos.</td>
</tr>
<tr>
<td>Configuração do cliente OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>O pod `vpn-*` do cliente OpenVPN no namespace `kube-system` agora configura `dnsPolicy` como `Default` para evitar que o pod falhe quando o DNS do cluster estiver inativo.</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Imagem atualizada para [CVE-2016-7076 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368) e [CVE-2019-11068 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.11.9_1556, liberado em 29 de abril de 2019
{: #1119_1556}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.11.9_1556.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.9_1555">
<caption>Mudanças desde a versão 1.11.9_1555</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações para pacotes do Ubuntu instalados.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>Veja as [notas sobre a liberação de containerd![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.1.7).</td>
</tr>
</tbody>
</table>


### Log de mudanças para o fix pack 1.11.9_1555 do nó do trabalhador, liberado em 15 de abril de 2019
{: #1119_1555}

A tabela a seguir mostra as mudanças no fix pack 1.11.9_1555 do nó do trabalhador.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.11.9_1554">
<caption>Mudanças feitas desde a versão 1.11.9_1554</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações em pacotes do Ubuntu instalados, incluindo `systemd` para [CVE-2019-3842 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Log de mudanças para a 1.11.9_1554, liberada em 8 de abril de 2019
{: #1119_1554}

A tabela a seguir mostra as mudanças incluídas na correção 1.11.9_1554.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.11.8_1553">
<caption>Mudanças feitas desde a versão 1.11.8_1553</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>Consulte as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.3/releases/#v336). A atualização resolve [CVE-2019-9946 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Proxy de alta disponibilidade do cluster mestre</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Consulte as [notas sobre a liberação do HAProxy ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). A atualização resolve [CVE-2018-0732 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) e [CVE-2019-1559 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.8-219</td>
<td>v1.11.9-241</td>
<td>Atualizado para suportar as liberações do Kubernetes 1.11.9 e do Calico 3.3.6.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.8</td>
<td>v1.11.9</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.9).</td>
</tr>
<tr>
<td>DNS do Kubernetes</td>
<td>14.1.2010</td>
<td>1.14.13</td>
<td>Veja as [Notas sobre a liberação do DNS do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Imagem atualizada para [CVE-2017-12447 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Kernel do Ubuntu 16.04</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Imagens do nó do trabalhador atualizadas com atualização do kernel para [CVE-2019-9213 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Kernel do Ubuntu 18.04</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Imagens do nó do trabalhador atualizadas com atualização do kernel para [CVE-2019-9213 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack 1.11.8_1553 do nó do trabalhador, liberado em 1 de abril de 2019
{: #1118_1553}

A tabela a seguir mostra as mudanças incluídas na correção 1.11.8_1553 do nó do trabalhador.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.11.8_1552">
<caption>Mudanças feitas desde a versão 1.11.8_1552</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilização do recurso do nó do trabalhador</td>
<td>N/A</td>
<td>N/A</td>
<td>Reservas de memória aumentadas para o kubelet e o containerd para evitar que esses componentes sejam executados fora dos recursos. Para obter mais informações, consulte [Reservas de recursos do nó do trabalhador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack principal 1.11.8_1552, liberado em 26 de março de 2019
{: #1118_1552}

A tabela a seguir mostra as mudanças incluídas no fix pack principal 1.11.8_1552.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.11.8_1550">
<caption>Mudanças feitas desde a versão 1.11.8_1550</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>345</td>
<td>346</td>
<td>Imagem atualizada para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>166</td>
<td>167</td>
<td>Corrige os erros de `context deadline exceeded` e `timeout` intermitentes para gerenciar segredos do Kubernetes. Além disso, corrige as atualizações para o serviço de gerenciamento de chaves que pode deixar os segredos do Kubernetes existentes não criptografados. A atualização inclui correção para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Balanceador de carga e monitor de balanceador de carga para o {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Imagem atualizada para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.11.8_1550, liberado em 20 de março de 2019
{: #1118_1550}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.11.8_1550.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.11.8_1547">
<caption>Mudanças desde a versão 1.11.8_1547</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração do proxy do mestre de alta disponibilidade</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizada a configuração para manipular melhor as falhas na conexão intermitente para o cluster mestre.</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Atualizados os drivers de GPU para [418.43 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.nvidia.com/object/unix.html). A atualização inclui correção para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>344</td>
<td>345</td>
<td>Incluído suporte para  [ terminais em serviço privado ](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Atualizadas as imagens do nó do trabalhador com a atualização do kernel para [CVE-2019-6133 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>136</td>
<td>166</td>
<td>Atualizada a imagem para [CVE-2018-16890 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) e [CVE-2019-3823 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Atualizada a imagem para [CVE-2018-10779 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) e [CVE-2019-7663 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.11.8_1547, liberado em 4 de março de 2019
{: #1118_1547}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.11.8_1547.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.7_1546">
<caption>Mudanças desde a versão 1.11.7_1546</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Atualizadas as imagens para [CVE-2019-6454 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.7-198</td>
<td>v1.11.8-219</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.11.8. Corrigidos problemas de conectividade periódica para balanceadores de carga que configuram `externalTrafficPolicy` como `local`. Atualizados os eventos de balanceador de carga para usar os links da documentação mais recente do {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>342</td>
<td>344</td>
<td>Mudado o sistema operacional de base para a imagem de Fedora para Alpine. Atualizada a imagem para [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>122.</td>
<td>136</td>
<td>Tempo limite do cliente aumentado para  {{site.data.keyword.keymanagementservicefull_notm}}. Atualizada a imagem para [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.7</td>
<td>v1.11.8</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.8). A atualização resolve [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) e [CVE-2019-1002100 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluído  ` ExperimentalCriticalPodAnnotation = true `  para a opção  ` -- feature-gates ` . Essa configuração ajuda a migrar os pods da anotação `scheduler.alpha.kubernetes.io/critical-pod` descontinuada para [Suporte de prioridade de pod do Kubernetes](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>DNS do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Aumentado o limite de memória de pod do Kubernetes DNS de `170Mi` para `400Mi` para manipular mais serviços de cluster.</td>
</tr>
<tr>
<td>Balanceador de carga e monitor de balanceador de carga para o {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Atualizada a imagem para [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Cliente e servidor OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Atualizada a imagem para [CVE-2019-1559 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Atualizada a imagem para [CVE-2019-6454 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.11.7_1546, liberado em 27 de fevereiro de 2019
{: #1117_1546}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do trabalhador do nó 1.11.7_1546.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.7_1546">
<caption>Mudanças desde a versão 1.11.7_1546</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Atualizadas as imagens do nó do trabalhador com a atualização do kernel para [CVE-2018-19407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.11.7_1544, liberado em 15 de fevereiro de 2019
{: #1117_1544}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do trabalhador do trabalhador 1.11.7_1544.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.11.7_1543">
<caption>Mudanças desde a versão 1.11.7_1543</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração do proxy do mestre de alta disponibilidade</td>
<td>N/A</td>
<td>N/A</td>
<td>Mudado o valor de configuração do pod `spec.priorityClassName` para `system-node-critical` e configurado o valor `spec.priority` para `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>Veja as [notas sobre a liberação de containerd![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.1.6). A atualização resolve [CVE-2019-5736 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configuração do Kubernetes  ` kubelet `</td>
<td>N/A</td>
<td>N/A</td>
<td>Ativada a porta do recurso `ExperimentalCriticalPodAnnotation` para evitar despejo crítico de pod estático.</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.11.7_1543, liberado em 5 de fevereiro de 2019
{: #1117_1543}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.11.7_1543.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.11.6_1541">
<caption>Mudanças desde a versão 1.11.6_1541</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Consulte as [notas sobre a liberação do etcd ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coreos/etcd/releases/v3.3.11). Além disso, os conjuntos de cifras suportados para etcd são agora restritos a um subconjunto com criptografia de alta segurança (128 bits ou mais).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Atualizadas as imagens para [CVE-2019-3462 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) e [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.6-180</td>
<td>v1.11.7-198</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.11.7. Além disso, a versão `calicoctl` é atualizada para 3.3.1. Corrigidas atualizações de configuração desnecessárias para balanceadores de carga da versão 2.0 em mudanças de status do nó do trabalhador.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>338</td>
<td>342</td>
<td>O plug-in de armazenamento de arquivo é atualizado conforme a seguir:
<ul><li>Suporta o fornecimento dinâmico com [planejamento com reconhecimento de topologia do volume](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignora a solicitação de volume persistente (PVC) para excluir erros se o armazenamento já tiver sido excluído.</li>
<li>Inclui uma anotação de mensagem de erro em PVCs com falha.</li>
<li>Otimiza as configurações de eleição de líder do controlador do fornecedor de armazenamento e de período de ressincronização e aumenta o tempo limite de fornecimento de 30 minutos para 1 hora.</li>
<li>Verifica as permissões de usuário antes de iniciar o fornecimento.</li></ul></td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>111</td>
<td>122.</td>
<td>Incluída a lógica de nova tentativa para evitar falhas temporárias quando os segredos do Kubernetes são gerenciados pelo {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.6</td>
<td>v1.11.7</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.7).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>A configuração de política de auditoria do servidor de API do Kubernetes é atualizada para incluir o registro de metadados para solicitações `cluster-admin` e o registro do corpo da solicitação de solicitações `create`, `update` e `patch` da carga de trabalho.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Atualizada a imagem para [CVE-2018-0734 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Além disso, a configuração do pod é agora obtida de um segredo em vez de um configmap.</td>
</tr>
<tr>
<td>Servidor OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Atualizada a imagem para [CVE-2018-0734 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Correção de segurança para [CVE-2018-16864 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.11.6_1541, liberado em 28 de janeiro de 2019
{: #1116_1541}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.11.6_1541.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.11.6_1540">
<caption>Mudanças desde a versão 1.11.6_1540</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações para os pacotes Ubuntu instalados, incluindo `apt` para [CVE-2019-3462 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) / [USN-3863-1 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>

### Changelog para 1.11.6_1540, liberado 21 de janeiro de 2019
{: #1116_1540}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.11.6_1540.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.11.5_1539">
<caption>Mudanças desde a versão 1.11.5_1539</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.5-152</td>
<td>v1.11.6-180</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.11.6.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.5</td>
<td>v1.11.6</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.6).</td>
</tr>
<tr>
<td>Resgador de complemento do Kubernetes</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Consulte as [Notas sobre a liberação do redimensionador de complemento do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Painel do Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>Consulte as [Notas sobre a liberação do painel do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). A atualização resolve [CVE-2018-18264 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).<br><br>Se você acessar o painel por meio de `kubectl proxy`, o botão **SKIP** na página de login será removido. Em vez disso, [use um **Token** para efetuar login](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td>Instalador do GPU</td>
<td>390.12</td>
<td>410,79</td>
<td>Atualizados os drivers de GPU instalados para 410.79.</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.11.5_1539, liberado em 7 de janeiro de 2019
{: #1115_1539}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.11.5_1539.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.11.5_1538">
<caption>Mudanças desde a versão 1.11.5_1538</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Atualizadas as imagens do nó do trabalhador com a atualização do kernel para [CVE-2017-5753, CVE-2018-18690 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

### Changelog para o nó do trabalhador fix pack 1.11.5_1538, liberado em 17 de dezembro de 2018
{: #1115_1538}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.11.5_1538.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.11.5_1537">
<caption>Mudanças desde a versão 1.11.5_1537</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações para pacotes do Ubuntu instalados.</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.11.5_1537, liberado em 5 de dezembro de 2018
{: #1115_1537}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.11.5_1537.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.4_1536">
<caption>Mudanças desde a versão 1.11.4_1536</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.4-142</td>
<td>v1.11.5-152</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.11.5.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.4</td>
<td>v1.11.5</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.5). A atualização resolve o [CVE-2018-1002105 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.11.4_1536, liberado em 4 de dezembro de 2018
{: #1114_1536}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.11.4_1536.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.4_1535">
<caption>Mudanças desde a versão 1.11.4_1535</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilização do recurso do nó do trabalhador</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluídos cgroups dedicados para o kubelet e o containerd para evitar que esses componentes sejam executados sem recursos. Para obter mais informações, consulte [Reservas de recursos do nó do trabalhador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.11.4_1535, liberado em 27 de novembro de 2018
{: #1114_1535}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.11.4_1535.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.3_1534">
<caption>Mudanças desde a versão 1.11.3_1534</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>Consulte as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.3/releases/#v331). A atualização resolve o [Tigera Technical Advisory TTA-2018-001 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.1.4</td>
<td>v1.1.5</td>
<td>Veja as [notas sobre a liberação de containerd![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.1.5). Containerd atualizado para corrigir um conflito que pode [parar a finalização dos pods ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/issues/2744).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.3-127</td>
<td>v1.11.4-142</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.11.4.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.11.4</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.4).</td>
</tr>
<tr>
<td>Cliente e servidor OpenVPN</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Imagem atualizada para o [CVE-2018-0732 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) e o [CVE-2018-0737 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.11.3_1534, liberado em 19 de novembro de 2018
{: #1113_1534}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.11.3_1534.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.3_1533">
<caption>Mudanças desde a versão 1.11.3_1533</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Atualizadas as imagens do nó do trabalhador com atualização do kernel para o [CVE-2018-7755 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>


### Log de mudanças para 1.11.3_1533, liberado em 7 de novembro de 2018
{: #1113_1533}

A tabela a seguir mostra as mudanças incluídas na correção 1.11.3_1533.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.3_1531">
<caption>Mudanças desde a versão 1.11.3_1531</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Atualização de alta disponibilidade do cluster mestre</td>
<td>N/A</td>
<td>N/A</td>
<td>Foram corrigidos os mestres altamente disponíveis (HA) para clusters que usam webhooks de admissão, como `initializerconfigurations`, `mutatingwebhookconfigurations` ou `validatingwebhookconfigurations`. É possível usar esses webhooks com gráficos Helm, como para [Cumprimento de segurança de imagem de contêiner](/docs/services/Registry?topic=registry-security_enforce#security_enforce).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.3-100</td>
<td>v1.11.3-127</td>
<td>Incluída a anotação `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` para especificar a VLAN na qual o serviço de balanceador de carga é implementado. Para ver as VLANs disponíveis em seu cluster, execute `ibmcloud ks vlans --zone <zone>`.</td>
</tr>
<tr>
<td>Kernel ativado por TPM</td>
<td>N/A</td>
<td>N/A</td>
<td>Os nós do trabalhador do bare metal com chips TPM para Computação confiável usam o kernel Ubuntu padrão até que a confiança seja ativada. Se você [ativar a confiança](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) em um cluster existente, será necessário [recarregar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) quaisquer nós do trabalhador do bare metal existentes com chips TPM. Para verificar se um nó do trabalhador bare metal tem um chip TPM, revise o campo **Confiável** depois de executar o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-types --zone`.</td>
</tr>
</tbody>
</table>

### Log de mudanças para fix pack do mestre 1.11.3_1531, liberado em 1 de novembro de 2018
{: #1113_1531_ha-master}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do mestre 1.11.3_1531.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.3_1527">
<caption>Mudanças desde a versão 1.11.3_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Cluster mestre</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizada a configuração do cluster mestre para aumentar a alta disponibilidade (HA). Os clusters agora têm três réplicas principais do Kubernetes configuradas com uma configuração altamente disponível (HA), com cada mestre implementado em hosts físicos separados. Além disso, se o cluster estiver em uma zona com capacidade para múltiplas zonas, os mestres serão difundidos pelas zonas.<br>Para ações que devem ser executadas, consulte [Atualizando para os clusters mestres altamente disponíveis](/docs/containers?topic=containers-cs_versions#ha-masters). Estas ações de preparação se aplicam:<ul>
<li>Se você tiver um firewall ou políticas de rede customizadas do Calico.</li>
<li>Se você estiver usando as portas do host `2040` ou `2041` nos nós do trabalhador.</li>
<li>Se você usou o endereço IP do cluster mestre para acesso no cluster ao mestre.</li>
<li>Se você tiver automação que chame a API ou a CLI do Calico (`calicoctl`), tal como para criar políticas do Calico.</li>
<li>Se você usar políticas de rede do Kubernetes ou do Calico para controlar o acesso de egresso do pod ao mestre.</li></ul></td>
</tr>
<tr>
<td>Proxy de alta disponibilidade do cluster mestre</td>
<td>N/A</td>
<td>1.8.12-alpine</td>
<td>Incluído um pod `ibm-master-proxy-*` para balanceamento de carga do lado do cliente em todos os nós do trabalhador, para que cada cliente do nó do trabalhador possa rotear solicitações para uma réplica disponível do mestre de alta disponibilidade.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>Consulte as [notas sobre a liberação do etcd ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coreos/etcd/releases/v3.3.1).</td>
</tr>
<tr>
<td>Criptografando dados em etcd</td>
<td>N/A</td>
<td>N/A</td>
<td>Anteriormente, os dados etcd eram armazenados em uma instância de armazenamento de arquivo NFS do mestre que está criptografada em repouso. Agora, os dados etcd são armazenados no disco local do mestre e submetidos a backup no {{site.data.keyword.cos_full_notm}}. Os dados são criptografados durante o trânsito para o {{site.data.keyword.cos_full_notm}} e em repouso. No entanto, os dados etcd no disco local do mestre não são criptografados. Se você desejar que os dados etcd locais do mestre sejam criptografados, [ative o {{site.data.keyword.keymanagementservicelong_notm}} em seu cluster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack do nó do trabalhador 1.11.3_1531, liberado em 26 de outubro de 2018
{: #1113_1531}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.11.3_1531.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.3_1525">
<caption>Mudanças desde a versão 1.11.3_1525</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Manipulação de interrupção do S.O.</td>
<td>N/A</td>
<td>N/A</td>
<td>Substituiu o daemon do sistema de solicitação de interrupção (IRQ) por um manipulador de interrupção de maior desempenho.</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack de mestre 1.11.3_1527, liberado em 15 de outubro de 2018
{: #1113_1527}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do mestre 1.11.3_1527.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.3_1524">
<caption>Mudanças desde a versão 1.11.3_1524</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração do Calico</td>
<td>N/A</td>
<td>N/A</td>
<td>Corrigida a análise de prontidão do contêiner `calico-node` para manipular melhor as falhas do nó.</td>
</tr>
<tr>
<td>Atualização do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Corrigido o problema com a atualização de complementos de cluster quando o mestre é atualizado de uma versão não suportada.</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack 1.11.3_1525 do nó do trabalhador, liberado em 10 de outubro de 2018
{: #1113_1525}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.11.3_1525.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.3_1524">
<caption>Mudanças desde a versão 1.11.3_1524</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Imagens atualizadas do nó do trabalhador com atualização do kernel para [CVE-2018-14633, CVE-2018-17182 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Tempo limite de sessão inativa</td>
<td>N/A</td>
<td>N/A</td>
<td>Configure o tempo limite de sessão inativa para 5 minutos por motivos de conformidade.</td>
</tr>
</tbody>
</table>


### Log de mudanças para 1.11.3_1524, liberado em 2 de outubro de 2018
{: #1113_1524}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.11.3_1524.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.3_1521">
<caption>Mudanças desde a versão 1.11.3_1521</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>1.1.3</td>
<td>1.1.4</td>
<td>Veja as [notas sobre a liberação de containerd![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.1.4).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.3-91</td>
<td>v1.11.3-100</td>
<td>Atualizado o link de documentação nas mensagens de erro do balanceador de carga.</td>
</tr>
<tr>
<td>Classes de armazenamento de arquivo IBM</td>
<td>N/A</td>
<td>N/A</td>
<td>Removido o parâmetro `reclaimPolicy` duplicado nas classes de armazenamento de arquivo da IBM.<br><br>
Além disso, agora quando você atualiza o cluster mestre, a classe de armazenamento de arquivo padrão da IBM permanece inalterada. Se desejar mudar a classe de armazenamento padrão, execute `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` e substitua `<storageclass>` pelo nome da classe de armazenamento.</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.11.3_1521, liberado em 20 de setembro de 2018
{: #1113_1521}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.11.3_1521.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.2_1516">
<caption>Mudanças desde a versão 1.11.2_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.2-71</td>
<td>v1.11.3-91</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.11.3.</td>
</tr>
<tr>
<td>Classes de armazenamento de arquivo IBM</td>
<td>N/A</td>
<td>N/A</td>
<td>Removido `mountOptions` nas classes de armazenamento de arquivo da IBM para usar o padrão fornecido pelo nó do trabalhador.<br><br>
Além disso, agora quando você atualiza o cluster mestre, a classe de armazenamento de arquivo IBM padrão permanece `ibmc-file-bronze`. Se desejar mudar a classe de armazenamento padrão, execute `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` e substitua `<storageclass>` pelo nome da classe de armazenamento.</td>
</tr>
<tr>
<td>Provedor de serviço de gerenciamento de chaves</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluída a capacidade de usar o provedor de serviço de gerenciamento de chave (KMS) do Kubernetes no cluster, para suportar o {{site.data.keyword.keymanagementservicefull}}. Quando você [ativa o {{site.data.keyword.keymanagementserviceshort}} em seu cluster](/docs/containers?topic=containers-encryption#keyprotect), todos os seus segredos do Kubernetes são criptografados.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.2</td>
<td>v1.11.3</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.3).</td>
</tr>
<tr>
<td>Autoscaler de DNS do Kubernetes</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>Veja as [notas sobre a liberação do escalador automático de DNS do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0).</td>
</tr>
<tr>
<td>Girar log</td>
<td>N/A</td>
<td>N/A</td>
<td>Alternado para usar os cronômetros `systemd` em vez de `cronjobs` para evitar que `logrotate` falhe nos nós do trabalhador que não sejam recarregados ou atualizados em 90 dias. **Nota**: em todas as versões anteriores dessa liberação secundária, o disco primário é preenchido após a falha da tarefa cron porque os logs não são girados. A tarefa cron falha depois que o nó do trabalhador fica ativo por 90 dias sem ser atualizado ou recarregado. Se os logs preencherem o disco primário inteiro, o nó do trabalhador entrará em um estado de falha. O nó do trabalhador pode ser corrigido usando o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` ou o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>Expiração de senha raiz</td>
<td>N/A</td>
<td>N/A</td>
<td>As senhas raiz dos nós do trabalhador expiram depois de 90 dias por motivos de conformidade. Se o seu conjunto de ferramentas de automação precisar efetuar login no nó do trabalhador como raiz ou depender de tarefas cron executadas como raiz, será possível desativar a expiração de senha efetuando login no nó do trabalhador e executando `chage -M -1 root`. **Nota**: se você tiver requisitos de conformidade de segurança que evitem a execução como raiz ou a remoção da expiração de senha, não desative a expiração. Em vez disso, é possível [atualizar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) ou [recarregar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) seus nós do trabalhador pelo menos a cada 90 dias.</td>
</tr>
<tr>
<td>Componentes de tempo de execução do nó do trabalhador (`kubelet`, `kube-proxy`, `containerd`)</td>
<td>N/A</td>
<td>N/A</td>
<td>Dependências de componentes de tempo de execução removidas no disco primário. Esse aprimoramento evita que os nós do trabalhador falhem quando o disco primário é preenchido.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>Limpar periodicamente unidades de montagem transientes para evitar que se tornem sem limites. Essa ação trata do [Problema 57345 do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.11.2_1516, liberado em 4 de setembro de 2018
{: #1112_1516}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.11.2_1516.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.2_1514">
<caption>Mudanças desde a versão 1.11.2_1514</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>Consulte as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.2/releases/#v321).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.2</td>
<td>1.1.3</td>
<td>Consulte as [notas sobre a liberação do `containerd` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.1.3).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.11.2-60</td>
<td>v1.11.2-71</td>
<td>A configuração do provedor em nuvem mudou para manipular melhor as atualizações para serviços do balanceador de carga com `externalTrafficPolicy` configurado como `local`.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}}  Configuração de plug-in de armazenamento de arquivo</td>
<td>N/A</td>
<td>N/A</td>
<td>Removida a versão do NFS padrão das opções de montagem nas classes de armazenamento de arquivo fornecidas pela IBM. O sistema operacional do host agora negocia a versão do NFS com o servidor NFS da infraestrutura do IBM Cloud (SoftLayer). Para configurar manualmente uma versão específica do NFS ou para mudar a versão do NFS de seu PV que foi negociada pelo sistema operacional do host, veja [Mudando a versão padrão do NFS](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack 1.11.2_1514 do nó do trabalhador, liberado em 23 de agosto de 2018
{: #1112_1514}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.11.2_1514.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.11.2_1513">
<caption>Mudanças desde a versão 1.11.2_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>` systemd `  atualizado para corrigir o vazamento  ` cgroup ` .</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Atualizadas as imagens do nó do trabalhador com atualização do kernel para [CVE-2018-3620,CVE-2018-3646 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.11.2_1513, liberado em 14 de agosto de 2018
{: #1112_1513}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.11.2_1513.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.5_1518">
<caption>Mudanças desde a versão 1.10.5_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>containerd</td>
<td>N/A</td>
<td>1.1.2</td>
<td>O `containerd` substitui o Docker como o novo tempo de execução do contêiner para Kubernetes. Consulte as [notas sobre a liberação do `containerd` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/containerd/containerd/releases/tag/v1.1.2). Para ações que devem ser executadas, consulte [Atualizando para `containerd` como o tempo de execução do contêiner](/docs/containers?topic=containers-cs_versions#containerd).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>O `containerd` substitui o Docker como o novo tempo de execução do contêiner para Kubernetes, para aprimorar o desempenho. Para ações que devem ser executadas, consulte [Atualizando para `containerd` como o tempo de execução do contêiner](/docs/containers?topic=containers-cs_versions#containerd).</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.14</td>
<td>v3.2.18</td>
<td>Consulte as [notas sobre a liberação do etcd ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coreos/etcd/releases/v3.2.18).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.5-118</td>
<td>v1.11.2-60</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.11. Além disso, os pods do balanceador de carga agora usam a nova classe de prioridade do pod `ibm-app-cluster-critical`.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>334</td>
<td>338</td>
<td>Atualizada a versão do `incubator` para 1.8. O armazenamento de arquivo é provisionado para a zona específica que você seleciona. Não é possível atualizar os rótulos de uma instância de PV existente (estática), a menos que você esteja usando um cluster de múltiplas zonas e precise [incluir os rótulos de região e zona](/docs/containers?topic=containers-kube_concepts#storage_multizone).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.11.2</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.2).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizada a configuração do OpenID Connect para o servidor da API do Kubernetes do cluster para suportar os grupos de acesso do {{site.data.keyword.Bluemix_notm}} Identity Access and Management (IAM). `Priority` foi incluído na opção `--enable-admission-plugins` para o servidor da API do Kubernetes do cluster e o cluster foi configurado para suportar de pod. Para obter informações adicionais, veja:
<ul><li>[{{site.data.keyword.Bluemix_notm}} Grupos de acesso do IAM ](/docs/containers?topic=containers-users#rbac)</li>
<li>[ Configurando a prioridade do pod ](/docs/containers?topic=containers-pod_priority#pod_priority)</li></ul></td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>V1.5.2</td>
<td>v.1.5.4</td>
<td>Limites de recurso aumentados para o contêiner `heapster-nanny`. Consulte as [notas sobre a liberação do Kubernetes Heapster ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/heapster/releases/tag/v1.5.4).</td>
</tr>
<tr>
<td>Configuração de criação de log</td>
<td>N/A</td>
<td>N/A</td>
<td>O diretório de log do contêiner é agora `/var/log/pods/` em vez do `/var/lib/docker/containers/` anterior.</td>
</tr>
</tbody>
</table>

<br />


## Archive
{: #changelog_archive}

Versões do Kubernetes não suportadas:
*  [Version 1.10](#110_changelog)
*  [Versão 1.9](#19_changelog)
*  [Versão 1.8](#18_changelog)
*  [Versão 1.7](#17_changelog)

### Log de mudanças da versão 1.10 (não suportada a partir de 16 de maio de 2019)
{: #110_changelog}

Revise os logs de mudanças da versão 1.10.
{: shortdesc}

*   [Log de mudanças para o fix pack do nó do trabalhador 1.10.13_1558, liberado em 13 de maio de 2019](#11013_1558)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.10.13_1557, liberado em 29 de abril de 2019](#11013_1557)
*   [Log de mudanças para o fix pack 1.10.13_1556 do nó do trabalhador, liberado em 15 de abril de 2019](#11013_1556)
*   [Log de mudanças para a 1.10.13_1555, liberada em 8 de abril de 2019](#11013_1555)
*   [Log de mudanças para o fix pack 1.10.13_1554 do nó do trabalhador, liberado em 1 de abril de 2019](#11013_1554)
*   [Log de mudanças para o fix pack principal 1.10.13_1553, liberado em 26 de março de 2019](#11118_1553)
*   [Log de mudanças para 1.10.13_1551, liberado em 20 de março de 2019](#11013_1551)
*   [Log de mudanças para 1.10.13_1548, liberado em 4 de março de 2019](#11013_1548)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.10.12_1546, liberado em 27 de fevereiro de 2019](#11012_1546)
*   [Changelog para o nó do trabalhador fix pack 1.10.12_1544, liberado 15 de fevereiro de 2019](#11012_1544)
*   [Log de mudanças para 1.10.12_1543, liberado em 5 de fevereiro de 2019](#11012_1543)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.10.12_1541, liberado em 28 de janeiro de 2019](#11012_1541)
*   [Log de mudanças para 1.10.12_1540, liberado em 21 de janeiro de 2019](#11012_1540)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.10.11_1538, liberado em 7 de janeiro de 2019](#11011_1538)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.10.11_1537, liberado em 17 de dezembro de 2018](#11011_1537)
*   [Log de mudanças para 1.10.11_1536, liberado em 4 de dezembro de 2018](#11011_1536)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.10.8_1532, liberado em 27 de novembro de 2018](#1108_1532)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.10.8_1531, liberado em 19 de novembro de 2018](#1108_1531)
*   [Log de mudanças para 1.10.8_1530, liberado em 7 de novembro de 2018](#1108_1530_ha-master)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.10.8_1528, liberado em 26 de outubro de 2018](#1108_1528)
*   [Log de mudanças para o fix pack 1.10.8_1525 do nó do trabalhador, liberado em 10 de outubro de 2018](#1108_1525)
*   [Log de mudanças para 1.10.8_1524, liberado em 2 de outubro de 2018](#1108_1524)
*   [Log de mudanças para o fix pack 1.10.7_1521 do nó do trabalhador, liberado em 20 de setembro de 2018](#1107_1521)
*   [Log de mudanças para 1.10.7_1520, liberado em 4 de setembro de 2018](#1107_1520)
*   [Log de mudanças para o fix pack 1.10.5_1519 do nó do trabalhador, liberado em 23 de agosto de 2018](#1105_1519)
*   [Log de mudanças para o fix pack 1.10.5_1518 do nó do trabalhador, liberado em 13 de agosto de 2018](#1105_1518)
*   [Log de mudanças para 1.10.5_1517, liberado em 27 de julho de 2018](#1105_1517)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.10.3_1514, liberado em 3 de julho de 2018](#1103_1514)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.10.3_1513, liberado em 21 de junho de 2018](#1103_1513)
*   [Log de mudanças para 1.10.3_1512, liberado em 12 de junho de 2018](#1103_1512)
*   [Log de mudanças para o fix pack 1.10.1_1510 do nó do trabalhador, liberado em 18 de maio de 2018](#1101_1510)
*   [Log de mudanças para o fix pack 1.10.1_1509 do nó do trabalhador, liberado em 16 de maio de 2018](#1101_1509)
*   [Log de mudanças para 1.10.1_1508, liberado em 1º de maio de 2018](#1101_1508)

#### Log de mudanças para o fix pack do nó do trabalhador 1.10.13_1558, liberado em 13 de maio de 2019
{: #11013_1558}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.13_1558.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.13_1557">
<caption>Mudanças desde a versão 1.10.13_1557</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy de alta disponibilidade do cluster mestre</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>Consulte as [notas sobre a liberação do HAProxy ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). A atualização resolve [CVE-2019-6706 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.10.13_1557, liberado em 29 de abril de 2019
{: #11013_1557}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.13_1557.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.13_1556">
<caption>Mudanças desde 1.10.13_1556</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações para pacotes do Ubuntu instalados.</td>
</tr>
</tbody>
</table>


#### Log de mudanças para o fix pack 1.10.13_1556 do nó do trabalhador, liberado em 15 de abril de 2019
{: #11013_1556}

A tabela a seguir mostra as mudanças incluídas no fix pack 1.10.13_1556 do nó do trabalhador.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.10.13_1555">
<caption>Mudanças feitas desde a versão 1.10.13_1555</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações em pacotes do Ubuntu instalados, incluindo `systemd` para [CVE-2019-3842 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para a 1.10.13_1555, liberada em 8 de abril de 2019
{: #11013_1555}

A tabela a seguir mostra as mudanças incluídas na correção 1.10.13_1555.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.10.13_1554">
<caption>Mudanças feitas desde a versão 1.10.13_1554</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Proxy de alta disponibilidade do cluster mestre</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>Consulte as [notas sobre a liberação do HAProxy ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.haproxy.org/download/1.9/src/CHANGELOG). A atualização resolve [CVE-2018-0732 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543) e [CVE-2019-1559 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>DNS do Kubernetes</td>
<td>14.1.2010</td>
<td>1.14.13</td>
<td>Veja as [Notas sobre a liberação do DNS do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Imagem atualizada para [CVE-2017-12447 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Kernel do Ubuntu 16.04</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Imagens do nó do trabalhador atualizadas com atualização do kernel para [CVE-2019-9213 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Kernel do Ubuntu 18.04</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Imagens do nó do trabalhador atualizadas com atualização do kernel para [CVE-2019-9213 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.10.13_1554 do nó do trabalhador, liberado em 1 de abril de 2019
{: #11013_1554}

A tabela a seguir mostra as mudanças incluídas na correção 1.10.13_1554 do nó do trabalhador.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.10.13_1553">
<caption>Mudanças feitas desde a versão 1.10.13_1553</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilização do recurso do nó do trabalhador</td>
<td>N/A</td>
<td>N/A</td>
<td>Reservas de memória aumentadas para o kubelet e o containerd para evitar que esses componentes sejam executados fora dos recursos. Para obter mais informações, consulte [Reservas de recursos do nó do trabalhador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>


#### Log de mudanças para o fix pack principal 1.10.13_1553, liberado em 26 de março de 2019
{: #11118_1553}

A tabela a seguir mostra as mudanças incluídas no fix pack principal 1.10.13_1553.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.10.13_1551">
<caption>Mudanças feitas desde a versão 1.10.13_1551</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>345</td>
<td>346</td>
<td>Imagem atualizada para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>166</td>
<td>167</td>
<td>Corrige os erros de `context deadline exceeded` e `timeout` intermitentes para gerenciar segredos do Kubernetes. Além disso, corrige as atualizações para o serviço de gerenciamento de chaves que pode deixar os segredos do Kubernetes existentes não criptografados. A atualização inclui correção para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Balanceador de carga e monitor de balanceador de carga para o {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Imagem atualizada para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.10.13_1551, liberado em 20 de março de 2019
{: #11013_1551}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.10.13_1551.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.10.13_1548">
<caption>Mudanças desde a versão 1.10.13_1548</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração do proxy do mestre de alta disponibilidade</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizada a configuração para manipular melhor as falhas na conexão intermitente para o cluster mestre.</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Atualizados os drivers de GPU para [418.43 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.nvidia.com/object/unix.html). A atualização inclui correção para [CVE-2019-9741 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>344</td>
<td>345</td>
<td>Incluído suporte para  [ terminais em serviço privado ](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Atualizadas as imagens do nó do trabalhador com a atualização do kernel para [CVE-2019-6133 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>136</td>
<td>166</td>
<td>Atualizada a imagem para [CVE-2018-16890 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822) e [CVE-2019-3823 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Atualizada a imagem para [CVE-2018-10779 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128) e [CVE-2019-7663 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.10.13_1548, liberado em 4 de março de 2019
{: #11013_1548}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.10.13_1548.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.10.12_1546">
<caption>Mudanças desde a versão 1.10.12_1546</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Atualizadas as imagens para [CVE-2019-6454 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.12-252</td>
<td>v1.10.13-288</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.10.13. Corrigidos problemas de conectividade periódica para balanceadores de carga que configuram `externalTrafficPolicy` como `local`. Atualizados os eventos de balanceador de carga para usar os links da documentação mais recente do {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>342</td>
<td>344</td>
<td>Mudado o sistema operacional de base para a imagem de Fedora para Alpine. Atualizada a imagem para [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>122.</td>
<td>136</td>
<td>Tempo limite do cliente aumentado para  {{site.data.keyword.keymanagementservicefull_notm}}. Atualizada a imagem para [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.12</td>
<td>v1.10.13</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.13).</td>
</tr>
<tr>
<td>DNS do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Aumentado o limite de memória de pod do Kubernetes DNS de `170Mi` para `400Mi` para manipular mais serviços de cluster.</td>
</tr>
<tr>
<td>Balanceador de carga e monitor de balanceador de carga para o {{site.data.keyword.Bluemix_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Atualizada a imagem para [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Cliente e servidor OpenVPN</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Atualizada a imagem para [CVE-2019-1559 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Agente de cálculo confiável</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Atualizada a imagem para [CVE-2019-6454 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.10.12_1546, liberado em 27 de fevereiro de 2019
{: #11012_1546}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.12_1546.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.10.12_1544">
<caption>Mudanças desde a versão 1.10.12_1544</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Atualizadas as imagens do nó do trabalhador com a atualização do kernel para [CVE-2018-19407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

#### Changelog para o nó do trabalhador fix pack 1.10.12_1544, liberado 15 de fevereiro de 2019
{: #11012_1544}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.12_1544.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.10.12_1543">
<caption>Mudanças desde a versão 1.10.12_1543</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>18.06.1-ce</td>
<td>18.06.2-ce</td>
<td>Consulte as notas sobre a liberação do [Docker Community Edition ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/docker/docker-ce/releases/tag/v18.06.2-ce). A atualização resolve [CVE-2019-5736 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Configuração do Kubernetes  ` kubelet `</td>
<td>N/A</td>
<td>N/A</td>
<td>Ativada a porta do recurso `ExperimentalCriticalPodAnnotation` para evitar despejo crítico de pod estático.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.10.12_1543, liberado em 5 de fevereiro de 2019
{: #11012_1543}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.10.12_1543.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.10.12_1541">
<caption>Mudanças desde a versão 1.10.12_1541</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>Consulte as [notas sobre a liberação do etcd ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coreos/etcd/releases/v3.3.11). Além disso, os conjuntos de cifras suportados para etcd são agora restritos a um subconjunto com criptografia de alta segurança (128 bits ou mais).</td>
</tr>
<tr>
<td>Plug-in e instalador do dispositivo GPU</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Atualizadas as imagens para [CVE-2019-3462 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) e [CVE-2019-6486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>338</td>
<td>342</td>
<td>O plug-in de armazenamento de arquivo é atualizado conforme a seguir:
<ul><li>Suporta o fornecimento dinâmico com [planejamento com reconhecimento de topologia do volume](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignora a solicitação de volume persistente (PVC) para excluir erros se o armazenamento já tiver sido excluído.</li>
<li>Inclui uma anotação de mensagem de erro em PVCs com falha.</li>
<li>Otimiza as configurações de eleição de líder do controlador do fornecedor de armazenamento e de período de ressincronização e aumenta o tempo limite de fornecimento de 30 minutos para 1 hora.</li>
<li>Verifica as permissões de usuário antes de iniciar o fornecimento.</li></ul></td>
</tr>
<tr>
<td>Provedor do Key Management Service</td>
<td>111</td>
<td>122.</td>
<td>Incluída a lógica de nova tentativa para evitar falhas temporárias quando os segredos do Kubernetes são gerenciados pelo {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>A configuração de política de auditoria do servidor de API do Kubernetes é atualizada para incluir o registro de metadados para solicitações `cluster-admin` e o registro do corpo da solicitação de solicitações `create`, `update` e `patch` da carga de trabalho.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Atualizada a imagem para [CVE-2018-0734 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Além disso, a configuração do pod é agora obtida de um segredo em vez de um configmap.</td>
</tr>
<tr>
<td>Servidor OpenVPN</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Atualizada a imagem para [CVE-2018-0734 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) e [CVE-2018-5407 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Correção de segurança para [CVE-2018-16864 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.10.12_1541, liberado em 28 de janeiro de 2019
{: #11012_1541}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.12_1541.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.10.12_1540">
<caption>Mudanças desde a versão 1.10.12_1540</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualiza para os pacotes Ubuntu instalados, incluindo `apt` para [CVE-2019-3462 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) e [USN-3863-1 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.10.12_1540, liberado em 21 de janeiro de 2019
{: #11012_1540}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.10.12_1540.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.10.11_1538">
<caption>Mudanças desde a versão 1.10.11_1538</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.11-219</td>
<td>v1.10.12-252</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.10.12.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.11</td>
<td>v1.10.12</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.12).</td>
</tr>
<tr>
<td>Resgador de complemento do Kubernetes</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>Consulte as [Notas sobre a liberação do redimensionador de complemento do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Painel do Kubernetes</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>Consulte as [Notas sobre a liberação do painel do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). A atualização resolve [CVE-2018-18264 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).<br><br>Se você acessar o painel por meio de `kubectl proxy`, o botão **SKIP** na página de login será removido. Em vez disso, [use um **Token** para efetuar login](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td>Instalador do GPU</td>
<td>390.12</td>
<td>410,79</td>
<td>Atualizados os drivers de GPU instalados para 410.79.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.10.11_1538, liberado em 7 de janeiro de 2019
{: #11011_1538}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do trabalhador do fix pack 1.10.11_1538.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.10.11_1537">
<caption>Mudanças desde a versão 1.10.11_1537</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Atualizadas as imagens do nó do trabalhador com a atualização do kernel para [CVE-2017-5753, CVE-2018-18690 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.10.11_1537, liberado em 17 de dezembro de 2018
{: #11011_1537}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.11_1537.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.10.11_1536">
<caption>Mudanças desde a versão 1.10.11_1536</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações para pacotes do Ubuntu instalados.</td>
</tr>
</tbody>
</table>


#### Log de mudanças para 1.10.11_1536, liberado em 4 de dezembro de 2018
{: #11011_1536}

A tabela a seguir mostra as mudanças incluídas na correção 1.10.11_1536.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.8_1532">
<caption>Mudanças desde a versão 1.10.8_1532</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>Consulte as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.3/releases/#v331). A atualização resolve o [Tigera Technical Advisory TTA-2018-001 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.8-197</td>
<td>v1.10.11-219</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.10.11.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.8</td>
<td>v1.10.11</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.11). A atualização resolve o [CVE-2018-1002105 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
<tr>
<td>Cliente e servidor OpenVPN</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Imagem atualizada para o [CVE-2018-0732 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) e o [CVE-2018-0737 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
<tr>
<td>Utilização do recurso do nó do trabalhador</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluídos cgroups dedicados para o kubelet e o docker para evitar que esses componentes sejam executados sem recursos. Para obter mais informações, consulte [Reservas de recursos do nó do trabalhador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.10.8_1532, liberado em 27 de novembro de 2018
{: #1108_1532}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.8_1532.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.8_1531">
<caption>Mudanças desde a versão 1.10.8_1531</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>Consulte as [Notas sobre a liberação do Docker ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.docker.com/engine/release-notes/#18061-ce).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.10.8_1531, liberado em 19 de novembro de 2018
{: #1108_1531}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.8_1531.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.8_1530">
<caption>Mudanças desde a versão 1.10.8_1530</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Atualizadas as imagens do nó do trabalhador com atualização do kernel para o [CVE-2018-7755 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.10.8_1530, liberado em 7 de novembro de 2018
{: #1108_1530_ha-master}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.10.8_1530.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.8_1528">
<caption>Mudanças desde a versão 1.10.8_1528</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Cluster mestre</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizada a configuração do cluster mestre para aumentar a alta disponibilidade (HA). Os clusters agora têm três réplicas principais do Kubernetes configuradas com uma configuração altamente disponível (HA), com cada mestre implementado em hosts físicos separados. Além disso, se o cluster estiver em uma zona com capacidade para múltiplas zonas, os mestres serão difundidos pelas zonas.<br>Para ações que devem ser executadas, consulte [Atualizando para os clusters mestres altamente disponíveis](/docs/containers?topic=containers-cs_versions#ha-masters). Estas ações de preparação se aplicam:<ul>
<li>Se você tiver um firewall ou políticas de rede customizadas do Calico.</li>
<li>Se você estiver usando as portas do host `2040` ou `2041` nos nós do trabalhador.</li>
<li>Se você usou o endereço IP do cluster mestre para acesso no cluster ao mestre.</li>
<li>Se você tiver automação que chame a API ou a CLI do Calico (`calicoctl`), tal como para criar políticas do Calico.</li>
<li>Se você usar políticas de rede do Kubernetes ou do Calico para controlar o acesso de egresso do pod ao mestre.</li></ul></td>
</tr>
<tr>
<td>Proxy de alta disponibilidade do cluster mestre</td>
<td>N/A</td>
<td>1.8.12-alpine</td>
<td>Incluído um pod `ibm-master-proxy-*` para balanceamento de carga do lado do cliente em todos os nós do trabalhador, para que cada cliente do nó do trabalhador possa rotear solicitações para uma réplica disponível do mestre de alta disponibilidade.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>Consulte as [notas sobre a liberação do etcd ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/coreos/etcd/releases/v3.3.1).</td>
</tr>
<tr>
<td>Criptografando dados em etcd</td>
<td>N/A</td>
<td>N/A</td>
<td>Anteriormente, os dados etcd eram armazenados em uma instância de armazenamento de arquivo NFS do mestre que está criptografada em repouso. Agora, os dados etcd são armazenados no disco local do mestre e submetidos a backup no {{site.data.keyword.cos_full_notm}}. Os dados são criptografados durante o trânsito para o {{site.data.keyword.cos_full_notm}} e em repouso. No entanto, os dados etcd no disco local do mestre não são criptografados. Se você desejar que os dados etcd locais do mestre sejam criptografados, [ative o {{site.data.keyword.keymanagementservicelong_notm}} em seu cluster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.8-172</td>
<td>v1.10.8-197</td>
<td>Incluída a anotação `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` para especificar a VLAN na qual o serviço de balanceador de carga é implementado. Para ver as VLANs disponíveis em seu cluster, execute `ibmcloud ks vlans --zone <zone>`.</td>
</tr>
<tr>
<td>Kernel ativado por TPM</td>
<td>N/A</td>
<td>N/A</td>
<td>Os nós do trabalhador do bare metal com chips TPM para Computação confiável usam o kernel Ubuntu padrão até que a confiança seja ativada. Se você [ativar a confiança](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) em um cluster existente, será necessário [recarregar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) quaisquer nós do trabalhador do bare metal existentes com chips TPM. Para verificar se um nó do trabalhador bare metal tem um chip TPM, revise o campo **Confiável** depois de executar o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-types --zone`.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.10.8_1528, liberado em 26 de outubro de 2018
{: #1108_1528}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.8_1528.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.8_1527">
<caption>Mudanças desde a versão 1.10.8_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Manipulação de interrupção do S.O.</td>
<td>N/A</td>
<td>N/A</td>
<td>Substituiu o daemon do sistema de solicitação de interrupção (IRQ) por um manipulador de interrupção de maior desempenho.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack de mestre 1.10.8_1527, liberado em 15 de outubro de 2018
{: #1108_1527}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do mestre 1.10.8_1527.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.8_1524">
<caption>Mudanças desde a versão 1.10.8_1524</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuração do Calico</td>
<td>N/A</td>
<td>N/A</td>
<td>Corrigida a análise de prontidão do contêiner `calico-node` para manipular melhor as falhas do nó.</td>
</tr>
<tr>
<td>Atualização do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Corrigido o problema com a atualização de complementos de cluster quando o mestre é atualizado de uma versão não suportada.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.10.8_1525 do nó do trabalhador, liberado em 10 de outubro de 2018
{: #1108_1525}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.8_1525.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.8_1524">
<caption>Mudanças desde a versão 1.10.8_1524</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Imagens atualizadas do nó do trabalhador com atualização do kernel para [CVE-2018-14633, CVE-2018-17182 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Tempo limite de sessão inativa</td>
<td>N/A</td>
<td>N/A</td>
<td>Configure o tempo limite de sessão inativa para 5 minutos por motivos de conformidade.</td>
</tr>
</tbody>
</table>


#### Log de mudanças para 1.10.8_1524, liberado em 2 de outubro de 2018
{: #1108_1524}

A tabela a seguir mostra as mudanças incluídas na correção 1.10.8_1524.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.7_1520">
<caption>Mudanças desde a versão 1.10.7_1520</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provedor de serviço de gerenciamento de chaves</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluída a capacidade de usar o provedor de serviço de gerenciamento de chave (KMS) do Kubernetes no cluster, para suportar o {{site.data.keyword.keymanagementservicefull}}. Quando você [ativa o {{site.data.keyword.keymanagementserviceshort}} em seu cluster](/docs/containers?topic=containers-encryption#keyprotect), todos os seus segredos do Kubernetes são criptografados.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.7</td>
<td>v1.10.8</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.8).</td>
</tr>
<tr>
<td>Autoscaler de DNS do Kubernetes</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>Veja as [notas sobre a liberação do escalador automático de DNS do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.7-146</td>
<td>v1.10.8-172</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.10.8. Além disso, atualizado o link de documentação nas mensagens de erro do balanceador de carga.</td>
</tr>
<tr>
<td>Classes de armazenamento de arquivo IBM</td>
<td>N/A</td>
<td>N/A</td>
<td>Removido `mountOptions` nas classes de armazenamento de arquivo da IBM para usar o padrão fornecido pelo nó do trabalhador. Removido o parâmetro `reclaimPolicy` duplicado nas classes de armazenamento de arquivo da IBM.<br><br>
Além disso, agora quando você atualiza o cluster mestre, a classe de armazenamento de arquivo padrão da IBM permanece inalterada. Se desejar mudar a classe de armazenamento padrão, execute `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` e substitua `<storageclass>` pelo nome da classe de armazenamento.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.10.7_1521 do nó do trabalhador, liberado em 20 de setembro de 2018
{: #1107_1521}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.7_1521.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.7_1520">
<caption>Mudanças desde a versão 1.10.7_1520</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Girar log</td>
<td>N/A</td>
<td>N/A</td>
<td>Alternado para usar os cronômetros `systemd` em vez de `cronjobs` para evitar que `logrotate` falhe nos nós do trabalhador que não sejam recarregados ou atualizados em 90 dias. **Nota**: em todas as versões anteriores dessa liberação secundária, o disco primário é preenchido após a falha da tarefa cron porque os logs não são girados. A tarefa cron falha depois que o nó do trabalhador fica ativo por 90 dias sem ser atualizado ou recarregado. Se os logs preencherem o disco primário inteiro, o nó do trabalhador entrará em um estado de falha. O nó do trabalhador pode ser corrigido usando o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` ou o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>Componentes de tempo de execução do nó do trabalhador (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/A</td>
<td>N/A</td>
<td>Dependências de componentes de tempo de execução removidas no disco primário. Esse aprimoramento evita que os nós do trabalhador falhem quando o disco primário é preenchido.</td>
</tr>
<tr>
<td>Expiração de senha raiz</td>
<td>N/A</td>
<td>N/A</td>
<td>As senhas raiz dos nós do trabalhador expiram depois de 90 dias por motivos de conformidade. Se o seu conjunto de ferramentas de automação precisar efetuar login no nó do trabalhador como raiz ou depender de tarefas cron executadas como raiz, será possível desativar a expiração de senha efetuando login no nó do trabalhador e executando `chage -M -1 root`. **Nota**: se você tiver requisitos de conformidade de segurança que evitem a execução como raiz ou a remoção da expiração de senha, não desative a expiração. Em vez disso, é possível [atualizar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) ou [recarregar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) seus nós do trabalhador pelo menos a cada 90 dias.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>Limpar periodicamente unidades de montagem transientes para evitar que se tornem sem limites. Essa ação trata do [Problema 57345 do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>Desativada a ponte de Docker padrão para que o intervalo de IP `172.17.0.0/16` seja agora usado para rotas privadas. Se você depender da construção de contêineres do Docker em nós do trabalhador executando comandos `docker` no host diretamente ou usando um pod que monta o soquete do Docker, escolha entre as opções a seguir.<ul><li>Para assegurar a conectividade de rede externa ao construir o contêiner, execute `docker build. -- host de rede `.</li>
<li>Para criar explicitamente uma rede a ser usada ao construir o contêiner, execute `docker network create` e, em seguida, use essa rede.</li></ul>
**Nota**: tem dependências no soquete do Docker ou no Docker diretamente? [Atualize para `containerd` em vez de `docker` como o tempo de execução do contêiner](/docs/containers?topic=containers-cs_versions#containerd) para que seus clusters estejam preparados para executar o Kubernetes versão 1.11 ou mais recente.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.10.7_1520, liberado em 4 de setembro de 2018
{: #1107_1520}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.10.7_1520.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.5_1519">
<caption>Mudanças desde a versão 1.10.5_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>Veja as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.2/releases/#v321).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.5-118</td>
<td>v1.10.7-146</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.10.7. Além disso, a configuração do provedor em nuvem foi mudada para manipular melhor as atualizações para serviços de balanceador de carga com `externalTrafficPolicy` configurado como `local`.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>334</td>
<td>338</td>
<td>Atualizada a versão de incubadora para 1.8. O armazenamento de arquivo é provisionado para a zona específica que você seleciona. Não é possível atualizar os rótulos de uma instância de PV existente (estática), a menos que você esteja usando um cluster de múltiplas zonas e precise incluir os rótulos de região e zona.<br><br> Removida a versão do NFS padrão das opções de montagem nas classes de armazenamento de arquivo fornecidas pela IBM. O sistema operacional do host agora negocia a versão do NFS com o servidor NFS da infraestrutura do IBM Cloud (SoftLayer). Para configurar manualmente uma versão específica do NFS ou para mudar a versão do NFS de seu PV que foi negociada pelo sistema operacional do host, veja [Mudando a versão padrão do NFS](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.10.7</td>
<td>Veja as [notas sobre a liberação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.7).</td>
</tr>
<tr>
<td>Configuração do Kubernetes Heapster</td>
<td>N/A</td>
<td>N/A</td>
<td>Limites de recurso aumentados para o contêiner `heapster-nanny`.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.10.5_1519 do nó do trabalhador, liberado em 23 de agosto de 2018
{: #1105_1519}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.5_1519.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.5_1518">
<caption>Mudanças desde a versão 1.10.5_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>` systemd `  atualizado para corrigir o vazamento  ` cgroup ` .</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Atualizadas as imagens do nó do trabalhador com atualização do kernel para [CVE-2018-3620,CVE-2018-3646 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>


#### Log de mudanças para o fix pack 1.10.5_1518 do nó do trabalhador, liberado em 13 de agosto de 2018
{: #1105_1518}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.5_1518.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.5_1517">
<caption>Mudanças desde a versão 1.10.5_1517</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações para pacotes do Ubuntu instalados.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.10.5_1517, liberado em 27 de julho de 2018
{: #1105_1517}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.10.5_1517.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.3_1514">
<caption>Mudanças desde a versão 1.10.3_1514</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>V3.1.1</td>
<td>v3.1.3</td>
<td>Veja as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.1/releases/#v313).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.3-85</td>
<td>v1.10.5-118</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.10.5. Além disso, os eventos `create failure` do serviço do LoadBalancer agora incluem qualquer erro de sub-rede móvel.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>320</td>
<td>334</td>
<td>Foi aumentado o tempo limite para criação de volume persistente de 15 a 30 minutos. Mudado o tipo de faturamento padrão para `hourly`. Incluídas opções de montagem nas classes de armazenamento predefinidas. Na instância de armazenamento de arquivo do NFS em sua conta de infraestrutura do IBM Cloud (SoftLayer), foi mudado o campo **Notas** para o formato da JSON e foi incluído o namespace do Kubernetes no qual o PV está implementado. Para suportar clusters de múltiplas zonas, inclua rótulos de zona e região em volumes persistentes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.3</td>
<td>v1.10.5</td>
<td>Veja as [notas sobre a liberação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.5).</td>
</tr>
<tr>
<td>Kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>Aprimoramentos menores para configurações de rede do nó do trabalhador para cargas de trabalho de rede de alto desempenho.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>A implementação do cliente do OpenVPN `vpn` que é executada no namespace `kube-system` agora é gerenciada pelo `addon-manager` do Kubernetes.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.10.3_1514, liberado em 3 de julho de 2018
{: #1103_1514}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.3_1514.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.3_1513">
<caption>Mudanças desde a versão 1.10.3_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>`sysctl` otimizado para cargas de trabalho de rede de alto desempenho.</td>
</tr>
</tbody>
</table>


#### Log de mudanças para o fix pack do nó do trabalhador 1.10.3_1513, liberado em 21 de junho de 2018
{: #1103_1513}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.3_1513.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.3_1512">
<caption>Mudanças desde a versão 1.10.3_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>Para tipos de máquina não criptografados, o disco secundário é limpo obtendo um novo sistema de arquivos quando você recarrega ou atualiza o nó do trabalhador.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.10.3_1512, liberado em 12 de junho de 2018
{: #1103_1512}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.10.3_1512.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.1_1510">
<caption>Mudanças desde a versão 1.10.1_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.10.1</td>
<td>v1.10.3</td>
<td>Veja as [notas sobre a liberação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.3).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluído `PodSecurityPolicy` na opção `--enable-admission-plugins` para o servidor de API do Kubernetes do cluster e configurado o cluster para suportar políticas de segurança de pod. Para obter mais informações, consulte [Configurando políticas de segurança do pod](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>Configuração do Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Ativada a opção `--authentication-token-webhook` para suportar tokens de conta do serviço e acesso da API para autenticação no terminal HTTPS `kubelet`.</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.10.1-52</td>
<td>v1.10.3-85</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.10.3.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluído `livenessProbe` na implementação do cliente do OpenVPN `vpn` que é executada no namespace `kube-system`.</td>
</tr>
<tr>
<td>Atualização do kernel</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Novas imagens do nó do trabalhador com atualização do kernel para [CVE-2018-3639 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>



#### Log de mudanças para o fix pack 1.10.1_1510 do nó do trabalhador, liberado em 18 de maio de 2018
{: #1101_1510}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.1_1510.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.1_1509">
<caption>Mudanças desde a versão 1.10.1_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correção para tratar de um erro ocorrido se você tiver usado o plug-in de armazenamento de bloco.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.10.1_1509 do nó do trabalhador, liberado em 16 de maio de 2018
{: #1101_1509}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.10.1_1509.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.10.1_1508">
<caption>Mudanças desde a versão 1.10.1_1508</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Os dados armazenados no diretório-raiz `kubelet` agora são salvos no disco secundário maior da máquina do nó do trabalhador.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.10.1_1508, liberado em 1º de maio de 2018
{: #1101_1508}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.10.1_1508.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.7_1510">
<caption>Mudanças desde a versão 1.9.7_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>V3.1.1</td>
<td>Veja as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.1/releases/#v311).</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>V1.5.0</td>
<td>V1.5.2</td>
<td>Veja as [notas sobre a liberação do Kubernetes Heapster ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.10.1</td>
<td>Veja as [notas sobre a liberação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluído <code>StorageObjectInUseProtection</code> na opção <code>--enable-admission-plugins</code> para o servidor de API do Kubernetes do cluster.</td>
</tr>
<tr>
<td>DNS do Kubernetes</td>
<td>1.14.8</td>
<td>14.1.2010</td>
<td>Veja as [notas sobre a liberação do DNS do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/dns/releases/tag/1.14.10).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.7-102</td>
<td>v1.10.1-52</td>
<td>Atualizado para suportar Kubernetes 1,10 release.</td>
</tr>
<tr>
<td>Suporte de GPU</td>
<td>N/A</td>
<td>N/A</td>
<td>O suporte para as [cargas de trabalho do contêiner de unidade de processamento de gráfico (GPU)](/docs/containers?topic=containers-app#gpu_app) está agora disponível para planejamento e execução. Para obter uma lista de tipos de máquina de GPU disponíveis, veja [Hardware para nós do trabalhador](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes). Para obter mais informações, veja a documentação do Kubernetes para [Planejar GPUs ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).</td>
</tr>
</tbody>
</table>

<br />


### Log de mudanças da versão 1.9 (não suportado a partir de 27 de dezembro de 2018)
{: #19_changelog}

Revise os logs de mudanças da versão 1.9.
{: shortdesc}

*   [Log de mudanças para o fix pack do nó do trabalhador 1.9.11_1539, liberado em 17 de dezembro de 2018](#1911_1539)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.9.11_1538, liberado em 4 de dezembro de 2018](#1911_1538)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.9.11_1537, liberado em 27 de novembro de 2018](#1911_1537)
*   [Log de mudanças para 1.9.11_1536, liberado em 19 de novembro de 2018](#1911_1536)
*   [Log de mudanças para a correção do nó do trabalhador 1.9.10_1532, liberado em 7 de novembro de 2018](#1910_1532)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.9.10_1531, liberado em 26 de outubro de 2018](#1910_1531)
*   [Log de mudanças para o fix pack de mestre 1.9.10_1530 liberado em 15 de outubro de 2018](#1910_1530)
*   [Log de mudanças para o fix pack 1.9.10_1528 do nó do trabalhador, liberado em 10 de outubro de 2018](#1910_1528)
*   [Log de mudanças para 1.9.10_1527, liberado em 2 de outubro de 2018](#1910_1527)
*   [Log de mudanças para o fix pack 1.9.10_1524 do nó do trabalhador, liberado em 20 de setembro de 2018](#1910_1524)
*   [Log de mudanças para 1.9.10_1523, liberado em 4 de setembro de 2018](#1910_1523)
*   [Log de mudanças para o fix pack 1.9.9_1522 do nó do trabalhador, liberado em 23 de agosto de 2018](#199_1522)
*   [Log de mudanças para o fix pack 1.9.9_1521 do nó do trabalhador, liberado em 13 de agosto de 2018](#199_1521)
*   [Log de mudanças para 1.9.9_1520, liberado em 27 de julho de 2018](#199_1520)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.9.8_1517, liberado em 3 de julho de 2018](#198_1517)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.9.8_1516, liberado em 21 de junho de 2018](#198_1516)
*   [Log de mudanças para 1.9.8_1515, liberado em 19 de junho de 2018](#198_1515)
*   [Log de mudanças para o fix pack do trabalhador do trabalhador 1.9.7_1513, liberado em 11 de junho de 2018](#197_1513)
*   [Log de mudanças para o fix pack 1.9.7_1512 do nó do trabalhador, liberado em 18 de maio de 2018](#197_1512)
*   [Log de mudanças para o fix pack 1.9.7_1511 nó do trabalhador, liberado em 16 de maio de 2018](#197_1511)
*   [Log de mudanças para 1.9.7_1510, liberado em 30 de abril de 2018](#197_1510)

#### Log de mudanças para o fix pack do nó do trabalhador 1.9.11_1539, liberado em 17 de dezembro de 2018
{: #1911_1539}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do trabalhador do fix pack 1.9.11_1539.
{: shortdesc}

<table summary="Mudanças feitas desde a versão 1.9.11_1538">
<caption>Mudanças desde a versão 1.9.11_1538</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações para pacotes do Ubuntu instalados.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.9.11_1538, liberado em 4 de dezembro de 2018
{: #1911_1538}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.11_1538.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.11_1537">
<caption>Mudanças desde a versão 1.9.11_1537</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Utilização do recurso do nó do trabalhador</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluídos cgroups dedicados para o kubelet e o docker para evitar que esses componentes sejam executados sem recursos. Para obter mais informações, consulte [Reservas de recursos do nó do trabalhador](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.9.11_1537, liberado em 27 de novembro de 2018
{: #1911_1537}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.11_1537.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.11_1536">
<caption>Mudanças desde a versão 1.9.11_1536</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>Consulte as [Notas sobre a liberação do Docker ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.docker.com/engine/release-notes/#18061-ce).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.9.11_1536, liberado em 19 de novembro de 2018
{: #1911_1536}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.9.11_1536.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.10_1532">
<caption>Mudanças desde a versão 1.9.10_1532</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v2.6.12</td>
<td>Consulte as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v2.6/releases/#v2612). A atualização resolve o [Tigera Technical Advisory TTA-2018-001 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Atualizadas as imagens do nó do trabalhador com atualização do kernel para o [CVE-2018-7755 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.10</td>
<td>v1.9.11</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.11).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.10-219</td>
<td>v1.9.11-249</td>
<td>Atualizado para suportar a liberação 1.9.11 do Kubernetes.</td>
</tr>
<tr>
<td>Cliente e servidor OpenVPN</td>
<td>2.4.4-r2</td>
<td>2.4.6-r3-IKS-8</td>
<td>Imagem atualizada para o [CVE-2018-0732 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) e o [CVE-2018-0737 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para a correção do nó do trabalhador 1.9.10_1532, liberado em 7 de novembro de 2018
{: #1910_1532}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.11_1532.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.10_1531">
<caption>Mudanças desde a versão 1.9.10_1531</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel ativado por TPM</td>
<td>N/A</td>
<td>N/A</td>
<td>Os nós do trabalhador do bare metal com chips TPM para Computação confiável usam o kernel Ubuntu padrão até que a confiança seja ativada. Se você [ativar a confiança](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) em um cluster existente, será necessário [recarregar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) quaisquer nós do trabalhador do bare metal existentes com chips TPM. Para verificar se um nó do trabalhador bare metal tem um chip TPM, revise o campo **Confiável** depois de executar o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-types --zone`.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.9.10_1531, liberado em 26 de outubro de 2018
{: #1910_1531}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.10_1531.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.10_1530">
<caption>Mudanças desde a versão 1.9.10_1530</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Manipulação de interrupção do S.O.</td>
<td>N/A</td>
<td>N/A</td>
<td>Substituiu o daemon do sistema de solicitação de interrupção (IRQ) por um manipulador de interrupção de maior desempenho.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack de mestre 1.9.10_1530 liberado em 15 de outubro de 2018
{: #1910_1530}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.10_1530.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.10_1527">
<caption>Mudanças desde a versão 1.9.10_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Atualização do cluster</td>
<td>N/A</td>
<td>N/A</td>
<td>Corrigido o problema com a atualização de complementos de cluster quando o mestre é atualizado de uma versão não suportada.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.9.10_1528 do nó do trabalhador, liberado em 10 de outubro de 2018
{: #1910_1528}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.10_1528.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.10_1527">
<caption>Mudanças desde a versão 1.9.10_1527</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Imagens atualizadas do nó do trabalhador com atualização do kernel para [CVE-2018-14633, CVE-2018-17182 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Tempo limite de sessão inativa</td>
<td>N/A</td>
<td>N/A</td>
<td>Configure o tempo limite de sessão inativa para 5 minutos por motivos de conformidade.</td>
</tr>
</tbody>
</table>


#### Log de mudanças para 1.9.10_1527, liberado em 2 de outubro de 2018
{: #1910_1527}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.9.10_1527.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.10_1523">
<caption>Mudanças desde a versão 1.9.10_1523</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.10-192</td>
<td>v1.9.10-219</td>
<td>Atualizado o link de documentação nas mensagens de erro do balanceador de carga.</td>
</tr>
<tr>
<td>Classes de armazenamento de arquivo IBM</td>
<td>N/A</td>
<td>N/A</td>
<td>Removido `mountOptions` nas classes de armazenamento de arquivo da IBM para usar o padrão fornecido pelo nó do trabalhador. Removido o parâmetro `reclaimPolicy` duplicado nas classes de armazenamento de arquivo da IBM.<br><br>
Além disso, agora quando você atualiza o cluster mestre, a classe de armazenamento de arquivo padrão da IBM permanece inalterada. Se desejar mudar a classe de armazenamento padrão, execute `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` e substitua `<storageclass>` pelo nome da classe de armazenamento.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.9.10_1524 do nó do trabalhador, liberado em 20 de setembro de 2018
{: #1910_1524}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.10_1524.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.10_1523">
<caption>Mudanças desde a versão 1.9.10_1523</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Girar log</td>
<td>N/A</td>
<td>N/A</td>
<td>Alternado para usar os cronômetros `systemd` em vez de `cronjobs` para evitar que `logrotate` falhe nos nós do trabalhador que não sejam recarregados ou atualizados em 90 dias. **Nota**: em todas as versões anteriores dessa liberação secundária, o disco primário é preenchido após a falha da tarefa cron porque os logs não são girados. A tarefa cron falha depois que o nó do trabalhador fica ativo por 90 dias sem ser atualizado ou recarregado. Se os logs preencherem o disco primário inteiro, o nó do trabalhador entrará em um estado de falha. O nó do trabalhador pode ser corrigido usando o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` ou o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>Componentes de tempo de execução do nó do trabalhador (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/A</td>
<td>N/A</td>
<td>Dependências de componentes de tempo de execução removidas no disco primário. Esse aprimoramento evita que os nós do trabalhador falhem quando o disco primário é preenchido.</td>
</tr>
<tr>
<td>Expiração de senha raiz</td>
<td>N/A</td>
<td>N/A</td>
<td>As senhas raiz dos nós do trabalhador expiram depois de 90 dias por motivos de conformidade. Se o seu conjunto de ferramentas de automação precisar efetuar login no nó do trabalhador como raiz ou depender de tarefas cron executadas como raiz, será possível desativar a expiração de senha efetuando login no nó do trabalhador e executando `chage -M -1 root`. **Nota**: se você tiver requisitos de conformidade de segurança que evitem a execução como raiz ou a remoção da expiração de senha, não desative a expiração. Em vez disso, é possível [atualizar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) ou [recarregar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) seus nós do trabalhador pelo menos a cada 90 dias.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>Limpar periodicamente unidades de montagem transientes para evitar que se tornem sem limites. Essa ação trata do [Problema 57345 do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>Desativada a ponte de Docker padrão para que o intervalo de IP `172.17.0.0/16` seja agora usado para rotas privadas. Se você depender da construção de contêineres do Docker em nós do trabalhador executando comandos `docker` no host diretamente ou usando um pod que monta o soquete do Docker, escolha entre as opções a seguir.<ul><li>Para assegurar a conectividade de rede externa ao construir o contêiner, execute `docker build. -- host de rede `.</li>
<li>Para criar explicitamente uma rede a ser usada ao construir o contêiner, execute `docker network create` e, em seguida, use essa rede.</li></ul>
**Nota**: tem dependências no soquete do Docker ou no Docker diretamente? [Atualize para `containerd` em vez de `docker` como o tempo de execução do contêiner](/docs/containers?topic=containers-cs_versions#containerd) para que seus clusters estejam preparados para executar o Kubernetes versão 1.11 ou mais recente.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.9.10_1523, liberado em 4 de setembro de 2018
{: #1910_1523}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.9.10_1523.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.9_1522">
<caption>Mudanças desde a versão 1.9.9_1522</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.9-167</td>
<td>v1.9.10-192</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.9.10. Além disso, a configuração do provedor em nuvem foi mudada para manipular melhor as atualizações para serviços de balanceador de carga com `externalTrafficPolicy` configurado como `local`.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>334</td>
<td>338</td>
<td>Atualizada a versão de incubadora para 1.8. O armazenamento de arquivo é provisionado para a zona específica que você seleciona. Não é possível atualizar os rótulos de uma instância de PV existente (estática), a menos que você esteja usando um cluster de múltiplas zonas e precise incluir os rótulos de região e zona.<br><br>Removida a versão do NFS padrão das opções de montagem nas classes de armazenamento de arquivo fornecidas pela IBM. O sistema operacional do host agora negocia a versão do NFS com o servidor NFS da infraestrutura do IBM Cloud (SoftLayer). Para configurar manualmente uma versão específica do NFS ou para mudar a versão do NFS de seu PV que foi negociada pelo sistema operacional do host, veja [Mudando a versão padrão do NFS](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.9</td>
<td>v1.9.10</td>
<td>Veja as [notas sobre a liberação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.10).</td>
</tr>
<tr>
<td>Configuração do Kubernetes Heapster</td>
<td>N/A</td>
<td>N/A</td>
<td>Limites de recurso aumentados para o contêiner `heapster-nanny`.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.9.9_1522 do nó do trabalhador, liberado em 23 de agosto de 2018
{: #199_1522}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.9_1522.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.9_1521">
<caption>Mudanças desde a versão 1.9.9_1521</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>` systemd `  atualizado para corrigir o vazamento  ` cgroup ` .</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Atualizadas as imagens do nó do trabalhador com atualização do kernel para [CVE-2018-3620,CVE-2018-3646 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>


#### Log de mudanças para o fix pack 1.9.9_1521 do nó do trabalhador, liberado em 13 de agosto de 2018
{: #199_1521}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.9_1521.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.9_1520">
<caption>Mudanças desde a versão 1.9.9_1520</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações para pacotes do Ubuntu instalados.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.9.9_1520, liberado em 27 de julho de 2018
{: #199_1520}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.9.9_1520.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.8_1517">
<caption>Mudanças desde a versão 1.9.8_1517</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.8-141</td>
<td>v1.9.9-167</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.9.9. Além disso, os eventos `create failure` do serviço do LoadBalancer agora incluem qualquer erro de sub-rede móvel.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>320</td>
<td>334</td>
<td>Foi aumentado o tempo limite para criação de volume persistente de 15 a 30 minutos. Mudado o tipo de faturamento padrão para `hourly`. Incluídas opções de montagem nas classes de armazenamento predefinidas. Na instância de armazenamento de arquivo do NFS em sua conta de infraestrutura do IBM Cloud (SoftLayer), foi mudado o campo **Notas** para o formato da JSON e foi incluído o namespace do Kubernetes no qual o PV está implementado. Para suportar clusters de múltiplas zonas, inclua rótulos de zona e região em volumes persistentes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.8</td>
<td>v1.9.9</td>
<td>Veja as [notas sobre a liberação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.9).</td>
</tr>
<tr>
<td>Kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>Aprimoramentos menores para configurações de rede do nó do trabalhador para cargas de trabalho de rede de alto desempenho.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>A implementação do cliente do OpenVPN `vpn` que é executada no namespace `kube-system` agora é gerenciada pelo `addon-manager` do Kubernetes.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.9.8_1517, liberado em 3 de julho de 2018
{: #198_1517}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.8_1517.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.8_1516">
<caption>Mudanças desde a versão 1.9.8_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>`sysctl` otimizado para cargas de trabalho de rede de alto desempenho.</td>
</tr>
</tbody>
</table>


#### Log de mudanças para o fix pack do nó do trabalhador 1.9.8_1516, liberado em 21 de junho de 2018
{: #198_1516}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.8_1516.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.8_1515">
<caption>Mudanças desde a versão 1.9.8_1515</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>Para tipos de máquina não criptografados, o disco secundário é limpo obtendo um novo sistema de arquivos quando você recarrega ou atualiza o nó do trabalhador.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.9.8_1515, liberado em 19 de junho de 2018
{: #198_1515}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.9.8_1515.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.7_1513">
<caption>Mudanças desde a versão 1.9.7_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.9.8</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.8).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluído `PodSecurityPolicy` na opção `--admission-control` para o servidor de API do Kubernetes do cluster e configurado para suportar políticas de segurança de pod. Para obter mais informações, consulte [Configurando políticas de segurança do pod](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>Provedor do IBM Cloud</td>
<td>v1.9.7-102</td>
<td>v1.9.8-141</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.9.8.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluído <code>livenessProbe</code> na implementação do cliente do OpenVPN <code>vpn</code> que é executada no namespace <code>kube-system</code>.</td>
</tr>
</tbody>
</table>


#### Log de mudanças para o fix pack do trabalhador do trabalhador 1.9.7_1513, liberado em 11 de junho de 2018
{: #197_1513}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.7_1513.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.7_1512">
<caption>Mudanças desde a versão 1.9.7_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Atualização do kernel</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Novas imagens do nó do trabalhador com atualização do kernel para [CVE-2018-3639 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.9.7_1512 do nó do trabalhador, liberado em 18 de maio de 2018
{: #197_1512}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.7_1512.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.7_1511">
<caption>Mudanças desde a versão 1.9.7_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correção para tratar de um erro ocorrido se você tiver usado o plug-in de armazenamento de bloco.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.9.7_1511 nó do trabalhador, liberado em 16 de maio de 2018
{: #197_1511}

A tabela a seguir mostra as mudanças que estão incluídas no fix pack do nó do trabalhador 1.9.7_1511.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.7_1510">
<caption>Mudanças desde a versão 1.9.7_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Os dados armazenados no diretório-raiz `kubelet` agora são salvos no disco secundário maior da máquina do nó do trabalhador.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.9.7_1510, liberado em 30 de abril de 2018
{: #197_1510}

A tabela a seguir mostra as mudanças que estão incluídas na correção 1.9.7_1510.
{: shortdesc}

<table summary="Mudanças que foram feitas desde a versão 1.9.3_1506">
<caption>Mudanças desde a versão 1.9.3_1506</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>V1.9.3</td>
<td>v1.9.7	</td>
<td><p>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7). Essa liberação trata das vulnerabilidades [CVE-2017-1002101 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) e [CVE-2017-1002102 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p><strong>Nota</strong>: agora, `secret`, `configMap`, `downwardAPI` e volumes projetados são montados como somente leitura. Anteriormente, os apps podiam gravar dados nesses volumes, mas o sistema podia reverter automaticamente os dados. Se os seus apps dependerem do comportamento inseguro prévio, modifique-os de modo correspondente.</p></td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluído `admissionregistration.k8s.io/v1alpha1=true` na opção `--runtime-config` para o servidor de API do Kubernetes do cluster.</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.3-71</td>
<td>v1.9.7-102</td>
<td>Os serviços `NodePort` e `LoadBalancer` agora suportam [preservar o IP de origem do cliente](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) configurando `service.spec.externalTrafficPolicy` como `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corrija a configuração de tolerância do [nó de borda](/docs/containers?topic=containers-edge#edge) para clusters mais velhos.</td>
</tr>
</tbody>
</table>

<br />


### Changelog Versão 1.8 (Não suportado)
{: #18_changelog}

Revise os logs de mudanças da versão 1.8.
{: shortdesc}

*   [Log de mudanças para o fix pack 1.8.15_1521 do nó do trabalhador, liberado em 20 de setembro de 2018](#1815_1521)
*   [Log de mudanças para o fix pack 1.8.15_1520 do nó do trabalhador, liberado em 23 de agosto de 2018](#1815_1520)
*   [Log de mudanças para o fix pack 1.8.15_1519 do nó do trabalhador, liberado em 13 de agosto de 2018](#1815_1519)
*   [Log de mudanças para 1.8.15_1518, liberado em 27 de julho de 2018](#1815_1518)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.8.13_1516, liberado em 3 de julho de 2018](#1813_1516)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.8.13_1515, liberado em 21 de junho de 2018](#1813_1515)
*   [Log de mudanças 1.8.13_1514, liberado em 19 de junho de 2018](#1813_1514)
*   [Log de mudanças para o fix pack do nó do trabalhador 1.8.11_1512, liberado em 11 de junho de 2018](#1811_1512)
*   [Log de mudanças para o fix pack 1.8.11_1511 do nó do trabalhador, liberado em 18 de maio de 2018](#1811_1511)
*   [Mudanças de log para o fix pack 1.8.11_1510 do nó do trabalhador, liberado em 16 de maio de 2018](#1811_1510)
*   [Log de mudanças para 1.8.11_1509, liberado em 19 de abril de 2018](#1811_1509)

#### Log de mudanças para o fix pack 1.8.15_1521 do nó do trabalhador, liberado em 20 de setembro de 2018
{: #1815_1521}

<table summary="Mudanças que foram feitas desde a versão 1.8.15_1520">
<caption>Mudanças desde a versão 1.8.15_1520</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Girar log</td>
<td>N/A</td>
<td>N/A</td>
<td>Alternado para usar os cronômetros `systemd` em vez de `cronjobs` para evitar que `logrotate` falhe nos nós do trabalhador que não sejam recarregados ou atualizados em 90 dias. **Nota**: em todas as versões anteriores dessa liberação secundária, o disco primário é preenchido após a falha da tarefa cron porque os logs não são girados. A tarefa cron falha depois que o nó do trabalhador fica ativo por 90 dias sem ser atualizado ou recarregado. Se os logs preencherem o disco primário inteiro, o nó do trabalhador entrará em um estado de falha. O nó do trabalhador pode ser corrigido usando o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) `ibmcloud ks worker-reload` ou o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) `ibmcloud ks worker-update`.</td>
</tr>
<tr>
<td>Componentes de tempo de execução do nó do trabalhador (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/A</td>
<td>N/A</td>
<td>Dependências de componentes de tempo de execução removidas no disco primário. Esse aprimoramento evita que os nós do trabalhador falhem quando o disco primário é preenchido.</td>
</tr>
<tr>
<td>Expiração de senha raiz</td>
<td>N/A</td>
<td>N/A</td>
<td>As senhas raiz dos nós do trabalhador expiram depois de 90 dias por motivos de conformidade. Se o seu conjunto de ferramentas de automação precisar efetuar login no nó do trabalhador como raiz ou depender de tarefas cron executadas como raiz, será possível desativar a expiração de senha efetuando login no nó do trabalhador e executando `chage -M -1 root`. **Nota**: se você tiver requisitos de conformidade de segurança que evitem a execução como raiz ou a remoção da expiração de senha, não desative a expiração. Em vez disso, é possível [atualizar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) ou [recarregar](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) seus nós do trabalhador pelo menos a cada 90 dias.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>Limpar periodicamente unidades de montagem transientes para evitar que se tornem sem limites. Essa ação trata do [Problema 57345 do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.8.15_1520 do nó do trabalhador, liberado em 23 de agosto de 2018
{: #1815_1520}

<table summary="Mudanças que foram feitas desde a versão 1.8.15_1519">
<caption>Mudanças desde a versão 1.8.15_1519</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>` systemd `  atualizado para corrigir o vazamento  ` cgroup ` .</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Atualizadas as imagens do nó do trabalhador com atualização do kernel para [CVE-2018-3620,CVE-2018-3646 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.8.15_1519 do nó do trabalhador, liberado em 13 de agosto de 2018
{: #1815_1519}

<table summary="Mudanças que foram feitas desde a versão 1.8.15_1518">
<caption>Mudanças desde a versão 1.8.15_1518</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pacotes do Ubuntu</td>
<td>N/A</td>
<td>N/A</td>
<td>Atualizações para pacotes do Ubuntu instalados.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.8.15_1518, liberado em 27 de julho de 2018
{: #1815_1518}

<table summary="Mudanças que foram feitas desde a versão 1.8.13_1516">
<caption>Mudanças desde a versão 1.8.13_1516</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.8.13-176</td>
<td>v1.8.15-204</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.8.15. Além disso, os eventos `create failure` do serviço do LoadBalancer agora incluem qualquer erro de sub-rede móvel.</td>
</tr>
<tr>
<td>Plug-in do {{site.data.keyword.Bluemix_notm}}  File Storage</td>
<td>320</td>
<td>334</td>
<td>Foi aumentado o tempo limite para criação de volume persistente de 15 a 30 minutos. Mudado o tipo de faturamento padrão para `hourly`. Incluídas opções de montagem nas classes de armazenamento predefinidas. Na instância de armazenamento de arquivo do NFS em sua conta de infraestrutura do IBM Cloud (SoftLayer), foi mudado o campo **Notas** para o formato da JSON e foi incluído o namespace do Kubernetes no qual o PV está implementado. Para suportar clusters de múltiplas zonas, inclua rótulos de zona e região em volumes persistentes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.8.13</td>
<td>v1.8.15</td>
<td>Veja as [notas sobre a liberação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.15).</td>
</tr>
<tr>
<td>Kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>Aprimoramentos menores para configurações de rede do nó do trabalhador para cargas de trabalho de rede de alto desempenho.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>A implementação do cliente do OpenVPN `vpn` que é executada no namespace `kube-system` agora é gerenciada pelo `addon-manager` do Kubernetes.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack do nó do trabalhador 1.8.13_1516, liberado em 3 de julho de 2018
{: #1813_1516}

<table summary="Mudanças que foram feitas desde a versão 1.8.13_1515">
<caption>Mudanças desde a versão 1.8.13_1515</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>`sysctl` otimizado para cargas de trabalho de rede de alto desempenho.</td>
</tr>
</tbody>
</table>


#### Log de mudanças para o fix pack do nó do trabalhador 1.8.13_1515, liberado em 21 de junho de 2018
{: #1813_1515}

<table summary="Mudanças que foram feitas desde a versão 1.8.13_1514">
<caption>Mudanças desde a versão 1.8.13_1514</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>Para tipos de máquina não criptografados, o disco secundário é limpo obtendo um novo sistema de arquivos quando você recarrega ou atualiza o nó do trabalhador.</td>
</tr>
</tbody>
</table>

#### Log de mudanças 1.8.13_1514, liberado em 19 de junho de 2018
{: #1813_1514}

<table summary="Mudanças que foram feitas desde a versão 1.8.11_1512">
<caption>Mudanças desde a versão 1.8.11_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.11</td>
<td>v1.8.13</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.13).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluído `PodSecurityPolicy` na opção `--admission-control` para o servidor de API do Kubernetes do cluster e configurado para suportar políticas de segurança de pod. Para obter mais informações, consulte [Configurando políticas de segurança do pod](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>Provedor do IBM Cloud</td>
<td>v1.8.11-126</td>
<td>v1.8.13-176</td>
<td>Atualizado para suportar a liberação do Kubernetes 1.8.13.</td>
</tr>
<tr>
<td>Cliente OpenVPN</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluído <code>livenessProbe</code> na implementação do cliente do OpenVPN <code>vpn</code> que é executada no namespace <code>kube-system</code>.</td>
</tr>
</tbody>
</table>


#### Log de mudanças para o fix pack do nó do trabalhador 1.8.11_1512, liberado em 11 de junho de 2018
{: #1811_1512}

<table summary="Mudanças que foram feitas desde a versão 1.8.11_1511">
<caption>Mudanças desde a versão 1.8.11_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Atualização do kernel</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Novas imagens do nó do trabalhador com atualização do kernel para [CVE-2018-3639 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>


#### Log de mudanças para o fix pack 1.8.11_1511 do nó do trabalhador, liberado em 18 de maio de 2018
{: #1811_1511}

<table summary="Mudanças que foram feitas desde a versão 1.8.11_1510">
<caption>Mudanças desde a versão 1.8.11_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correção para tratar de um erro ocorrido se você tiver usado o plug-in de armazenamento de bloco.</td>
</tr>
</tbody>
</table>

#### Mudanças de log para o fix pack 1.8.11_1510 do nó do trabalhador, liberado em 16 de maio de 2018
{: #1811_1510}

<table summary="Mudanças que foram feitas desde a versão 1.8.11_1509">
<caption>Mudanças desde a versão 1.8.11_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Os dados armazenados no diretório-raiz `kubelet` agora são salvos no disco secundário maior da máquina do nó do trabalhador.</td>
</tr>
</tbody>
</table>


#### Log de mudanças para 1.8.11_1509, liberado em 19 de abril de 2018
{: #1811_1509}

<table summary="Mudanças que foram feitas desde a versão 1.8.8_1507">
<caption>Mudanças desde a versão 1.8.8_1507</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td><p>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11). Essa liberação trata das vulnerabilidades [CVE-2017-1002101 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) e [CVE-2017-1002102 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p>Agora, `secret`, `configMap`, `downwardAPI` e volumes projetados são montados como somente leitura. Anteriormente, os apps podiam gravar dados nesses volumes, mas o sistema podia reverter automaticamente os dados. Se os seus apps dependerem do comportamento inseguro prévio, modifique-os de modo correspondente.</p></td>
</tr>
<tr>
<td>Pause a imagem do contêiner</td>
<td>3.0</td>
<td>3.1</td>
<td>Remove processos zumbi órfãos herdados.</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>Os serviços `NodePort` e `LoadBalancer` agora suportam [preservar o IP de origem do cliente](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) configurando `service.spec.externalTrafficPolicy` como `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corrija a configuração de tolerância do [nó de borda](/docs/containers?topic=containers-edge#edge) para clusters mais velhos.</td>
</tr>
</tbody>
</table>

<br />


### Version 1.7 changelog (Unsupported)
{: #17_changelog}

Revise os logs de mudanças da versão 1.7.
{: shortdesc}

*   [Log de mudanças para o fix pack do nó do trabalhador 1.7.16_1514, liberado em 11 de junho de 2018](#1716_1514)
*   [Log de mudanças para o fix pack 1.7.16_1513 do nó do trabalhador, liberado em 18 de maio de 2018](#1716_1513)
*   [Log de mudanças para o fix pack 1.7.16_1512 do nó do trabalhador, liberado em 16 de maio de 2018](#1716_1512)
*   [Log de mudanças para 1.7.16_1511, liberado em 19 de abril de 2018](#1716_1511)

#### Log de mudanças para o fix pack do nó do trabalhador 1.7.16_1514, liberado em 11 de junho de 2018
{: #1716_1514}

<table summary="Mudanças que foram feitas desde a versão 1.7.16_1513">
<caption>Mudanças desde a versão 1.7.16_1513</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Atualização do kernel</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Novas imagens do nó do trabalhador com atualização do kernel para [CVE-2018-3639 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.7.16_1513 do nó do trabalhador, liberado em 18 de maio de 2018
{: #1716_1513}

<table summary="Mudanças que foram feitas desde a versão 1.7.16_1512">
<caption>Mudanças desde a versão 1.7.16_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correção para tratar de um erro ocorrido se você tiver usado o plug-in de armazenamento de bloco.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.7.16_1512 do nó do trabalhador, liberado em 16 de maio de 2018
{: #1716_1512}

<table summary="Mudanças que foram feitas desde a versão 1.7.16_1511">
<caption>Mudanças desde a versão 1.7.16_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Os dados armazenados no diretório-raiz `kubelet` agora são salvos no disco secundário maior da máquina do nó do trabalhador.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.7.16_1511, liberado em 19 de abril de 2018
{: #1716_1511}

<table summary="Mudanças que foram feitas desde a versão 1.7.4_1509">
<caption>Mudanças desde a versão 1.7.4_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td><p>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16). Essa liberação trata das vulnerabilidades [CVE-2017-1002101 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) e [CVE-2017-1002102 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p>Agora, `secret`, `configMap`, `downwardAPI` e volumes projetados são montados como somente leitura. Anteriormente, os apps podiam gravar dados nesses volumes, mas o sistema podia reverter automaticamente os dados. Se os seus apps dependerem do comportamento inseguro prévio, modifique-os de modo correspondente.</p></td>
</tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>Os serviços `NodePort` e `LoadBalancer` agora suportam [preservar o IP de origem do cliente](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) configurando `service.spec.externalTrafficPolicy` como `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corrija a configuração de tolerância do [nó de borda](/docs/containers?topic=containers-edge#edge) para clusters mais velhos.</td>
</tr>
</tbody>
</table>
